---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "79483941"
---
# <a name="efficient-params-and-string-formatting"></a>Parámetros eficientes y formato de cadena

## <a name="summary"></a>Resumen
Esta combinación de características aumentará la eficacia del formato `string` valores y el paso de los argumentos de estilo `params`.

## <a name="motivation"></a>Motivación
La sobrecarga de asignación del formato `string` valores puede dominar el rendimiento de muchas aplicaciones basadas en texto: a partir de la penalización Boxing de los tipos de `struct`, la asignación de `object[]` para `params` y las asignaciones de `string` intermedias durante `string.Format` llamadas. Con el fin de mantener la eficacia, estas aplicaciones suelen necesitar abandonar características de productividad, como `params` y `string` interpolación y pasar a soluciones codificadas no estándar. 

Considere MSBuild como ejemplo. Esto se escribe con muchas características modernas C# de los desarrolladores que son conscientes del rendimiento. Sin embargo, en un ejemplo de compilación representativo MSBuild generará 262MB de asignación `string` con un nivel de detalle mínimo. De que 1/2 de las asignaciones son asignaciones de corta duración dentro de `string.Format`. Estas características quitarían gran parte de eso en el escritorio de .NET y la bajamos a casi cero en .NET Core debido a la disponibilidad de `Span<T>`

El conjunto de características del lenguaje que se describe aquí permitirá a las aplicaciones seguir usando estas características, con muy poca o ninguna actividad en la base de código de la aplicación, al tiempo que se elimina la sobrecarga de asignación no deseada en la mayoría de los casos.

## <a name="detailed-design"></a>Diseño detallado 
Hay un conjunto de características que se utilizarán aquí para lograr estos resultados:

- Expandir `params` para admitir un conjunto más amplio de tipos de colección.
- Permite a los desarrolladores personalizar cómo se consigue `string` interpolación. 
- Permite que los `string` interpolados se enlacen a sobrecargas de `string.Format` más eficaces.

### <a name="extending-params"></a>Extender los parámetros
El lenguaje permitirá que `params` en una signatura de método tenga los tipos `Span<T>`, `ReadOnlySpan<T>` y `IEnumerable<T>`. Las mismas reglas de invocación se aplicarán a estos nuevos tipos que se aplican a `params T[]`:

- No se puede sobrecargar cuando la única diferencia es una palabra clave de `params`.
- Puede invocar pasando una serie de argumentos que se pueden convertir implícitamente en `T` o en un único `Span<T>` / 
`ReadOnlySpan<T>` / argumento.`IEnumerable<T>`
- Debe ser el último parámetro de una firma de método.
- Etc... 

Se hará referencia a las variantes `Span<T>` y `ReadOnlySpan<T>` como `Span<T>` por motivos de simplicidad. En los casos en los que el comportamiento de `ReadOnlySpan<T>` difiere, se llamará explícitamente. 

La ventaja que proporcionan las variantes `Span<T>` de `params` es que proporciona al compilador una gran flexibilidad en cuanto a cómo asigna el almacenamiento de seguridad para el valor de `Span<T>`. Con un `params T[]` el compilador debe asignar un nuevo `T[]` para cada invocación de un método `params`. La reutilización no es posible porque debe asumir que el destinatario de la llamada almacenó y volvió a usar el parámetro. Esto puede dar lugar a una gran ineficacia en métodos con muchas de las invocaciones de `params`.

Determinadas variantes de `Span<T>` se `ref struct` el destinatario no puede almacenar el argumento. Por lo tanto, el compilador puede optimizar los sitios de llamada realizando acciones como volver a usar el argumento. Esto puede hacer que las invocaciones repetidas sean muy eficaces en comparación con `T[]`. Sin embargo, el lenguaje no realizará ninguna garantía específica sobre cómo se optimizan estos sitios. Tenga en cuenta solo que el compilador es libre de usar valores distintos de `T[]` al invocar un método `params Span<T>`. 

Una de estas posibles implementaciones es la siguiente. Considere todas `params` invocación en un cuerpo de método. El compilador podría asignar una matriz que tiene un tamaño igual a la invocación de `params` más grande y usarla para todas las invocaciones creando instancias de `Span<T>` de tamaño adecuado en la matriz. Por ejemplo:

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

El compilador podría elegir emitir el cuerpo de `Go` como se indica a continuación:

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

Esto puede reducir significativamente el número de matrices asignadas en una aplicación. Las asignaciones se pueden reducir aún más si el tiempo de ejecución proporciona utilidades para la asignación de matrices más inteligente.

Sin embargo, esta optimización no siempre se puede aplicar. Aunque el destinatario no puede capturar el argumento `params`, todavía se puede capturar en el llamador cuando hay un `ref` o un parámetro `out / ref` que es en sí mismo un `ref struct` tipo. 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

Sin embargo, estos casos se detectan estáticamente. Puede producirse siempre que haya un `ref` devolver o un parámetro `ref struct` pasado por `out` o `ref`. En tal caso, el compilador debe asignar un `T[]` nuevo para cada invocación. 

Al final de este documento se describen otras estrategias de optimización potenciales.

La variante `IEnumerable<T>` es una simple sobrecarga. Es útil en escenarios que tienen usos frecuentes de `IEnumerable<T>` pero también tienen muchos `params` uso. Cuando se invoca en `T` forma de argumento, el almacenamiento de respaldo se asignará como un `T[]` igual que `params T[]` se realiza hoy.

### <a name="params-overload-resolution-changes"></a>cambios en la resolución de sobrecarga de params
Esta propuesta significa que el lenguaje tiene ahora cuatro variantes de `params` donde antes tenía una. Es sensato que los métodos definan sobrecargas de métodos que solo difieren en el tipo de una `params` declaraciones. 

Tenga en cuenta que `StringBuilder.AppendFormat` ciertamente agregaría una sobrecarga de `params ReadOnlySpan<object>` además de la `params object[]`. Esto permitiría mejorar considerablemente el rendimiento al reducir las asignaciones de colección sin necesidad de realizar cambios en el código de llamada. 

Para facilitar este procedimiento, se presentará la siguiente regla de interrupción de la resolución de sobrecarga. Cuando los métodos candidatos solo difieren en el parámetro `params`, se preferirán los candidatos en el orden siguiente:

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

Este orden es el menos eficaz para el caso general.

### <a name="variant"></a>Variant
CoreFX está prototipo de un nuevo tipo administrado denominado [Variant](https://github.com/dotnet/corefxlab/pull/2595). Este tipo está pensado para usarse en las API que esperan valores heterogéneos, pero no desea que la sobrecarga que se lleva a cabo mediante `object` como parámetro. El tipo de `Variant` proporciona almacenamiento universal, pero evita la asignación de conversión boxing para los tipos de uso más frecuente. El uso de este tipo en las API como `string.Format` puede eliminar la sobrecarga de conversión boxing en la mayoría de los casos.

Este tipo no es necesariamente especial en el lenguaje. No obstante, se introduce en este documento por separado, ya que se trata de un detalle de implementación de otras partes de la propuesta. 

### <a name="efficient-interpolated-strings"></a>Cadenas interpoladas eficaces
Las cadenas interpoladas son una característica popular que todavía no C#es eficaz en. La sintaxis más común, utilizando un `string` interpolado como `string`, se convierte en una llamada a `string.Format(string, params object[])`. Esto incurrirá en las asignaciones de conversión boxing para todos los tipos de valor, `string` las asignaciones intermedias a medida que la implementación usa en gran medida `object.ToString` para el formato, así como las asignaciones de matriz una vez que el número de argumentos supera la cantidad de parámetros de las sobrecargas "rápidas" de `string.Format`. 

El idioma cambiará su interpolación disminuyendo para considerar sobrecargas alternativas de `string.Format`. Tendrá en cuenta todas las formas de `string.Format(string, params)` y elegirá la sobrecarga "Best" que satisfaga los tipos de argumento.
La "mejor" `params` sobrecarga se determinará según las reglas descritas anteriormente. Esto significa que los `string` interpolados ahora se pueden enlazar a sobrecargas muy eficaces como `string.Format(string format, params ReadOnlySpan<Variant> args)`. En muchos casos, se quitarán todas las asignaciones intermedias.

### <a name="customizable-interpolated-strings"></a>Cadenas interpoladas personalizables
Los desarrolladores pueden personalizar el comportamiento de las cadenas interpoladas con `FormattableString`. Contiene los datos que entran en una cadena interpolada: el formato `string` y los argumentos como una matriz. Aunque todavía tiene la conversión boxing y la asignación de la matriz de argumentos, así como la asignación para `FormattableString` (es un `abstract class`). Por lo tanto, es de poco uso en las aplicaciones que se asignan con gran cantidad de `string` formato.

Para que el formato de cadena interpolada sea eficaz, el lenguaje reconocerá un nuevo tipo: `System.ValueFormattableString`. Todas las cadenas interpoladas tendrán una conversión de tipo de destino a este tipo. Esto se implementará mediante la conversión de la cadena interpolada en la llamada `ValueFormattableString.Create` tal como se hace en `FormattableString.Create` hoy. El lenguaje admitirá todas las opciones de `params` descritas en este documento al buscar el método de `ValueFormattableString.Create` más adecuado. 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

Las reglas de resolución de sobrecarga se cambiarán para preferir `ValueFormattableString` sobre `string` cuando el argumento sea una cadena interpolada. Esto significa que será útil tener sobrecargas que solo difieran en `string` y `ValueFormattableString`. Hoy en día, este tipo de sobrecarga con `FormattableString` no es valioso, ya que el compilador siempre preferirá la versión `string` (a menos que el desarrollador use una conversión explícita). 

## <a name="open-issues"></a>Problemas abiertos

### <a name="valueformattablestring-breaking-change"></a>ValueFormattableString cambio de interrupción
El cambio que prefiere `ValueFormattableString` durante la resolución de sobrecargas en `string` es un cambio importante. Es posible que un desarrollador haya definido un tipo llamado `ValueFormattableString` hoy y usarlo en sobrecargas de método con `string`. Este cambio propuesto haría que el compilador seleccionara una sobrecarga diferente una vez implementado este conjunto de características. 

La posibilidad de esto parece razonablemente baja. El tipo necesitaría el nombre completo `System.ValueFormattableString` y tendría que tener `static` métodos denominados `Create`. Dado que se recomienda encarecidamente que los desarrolladores puedan definir cualquier tipo en el espacio de nombres `System` este salto parece un riesgo razonable.

### <a name="expanding-to-more-types"></a>Expandir hasta más tipos
Dado que estamos en el área, deberíamos considerar agregar `IList<T>`, `ICollection<T>` y `IReadOnlyList<T>` al conjunto de colecciones para el que se admite `params`. En lo que respecta a la implementación, el costo es una pequeña cantidad sobre el otro trabajo aquí.

Sin embargo, LDM debe decidir si merece la pena para el lenguaje. La adición de `IEnumerable<T>` quita un punto de fricción muy específico. Si falta esta solución `params` muchos clientes tenían que asignar `T[]` de un `IEnumerable<T>` al llamar a un método de `params`. No obstante, la adición de `IEnumerable<T>` se corrige. No hay ningún punto de fricción específico que las otras interfaces corrijan aquí. 

## <a name="considerations"></a>Consideraciones

### <a name="variant2-and-variant3"></a>Variant2 y Variant3
El equipo de CoreFX también tiene un conjunto de tipos de almacenamiento sin asignación para un máximo de tres argumentos de `Variant`. Se trata de una única `Variant`, `Variant2` y `Variant3`. Todos tienen un par de métodos para obtener una asignación gratuita `Span<Variant>` de ellas: `CreateSpan` y `KeepAlive`. Esto significa que para una `params Span<Variant>` de hasta tres argumentos, el sitio de la llamada puede ser totalmente gratuito.

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

El método `Go` se puede reducir a lo siguiente:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

Esto requiere muy poco trabajo en la parte superior de la propuesta para volver a usar `T[]` entre llamadas a `params Span<T>`. El compilador ya necesita administrar un temporal por llamada y realizar una limpieza de trabajo después de (aunque en un caso es simplemente marcar una Temp interna como disponible). 

Nota: la función `KeepAlive` solo es necesaria en el escritorio. En .NET Core, el método no estará disponible y, por lo tanto, el compilador no emitirá una llamada a él.

### <a name="clr-stack-allocation-helpers"></a>Aplicaciones auxiliares de asignación de pila de CLR
CLR solo proporciona solamente [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) para la asignación de la memoria contigua de la pila. Esta instrucción está limitada, ya que solo funciona para tipos de `unmanaged`. Esto significa que no se puede usar como una solución universal para asignar eficazmente el almacenamiento de copia de seguridad para `params 
 Span<T>`. 

Sin embargo, esta limitación no es una restricción fundamental, sino que, en su lugar, se trata de un artefacto de historial. CLR podría optar por agregar nuevos códigos de operación o intrínsecos que proporcionan la asignación de la pila universal. A continuación, se pueden usar para asignar el almacenamiento de respaldo para la mayoría de las llamadas `params Span<T>`.

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

El método `Go` se puede reducir a lo siguiente:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

Aunque este enfoque es muy eficaz para el montón, produce un uso adicional de la pila. En un algoritmo que tiene una pila profunda y una gran cantidad de `params` uso, es posible que se genere un `StackOverflowException` en el que una sencilla asignación de `T[]` se realizara correctamente. 

Desafortunadamente C# , no está configurado para el tipo de análisis entre métodos, en el que podría tomar una determinación fundada de si la llamada debe usar la asignación de la pila o el montón de `params`. En realidad, solo puede considerar cada llamada por sí misma.

CLR es la mejor configuración para crear este tipo de determinación en tiempo de ejecución. Por lo tanto, es probable que el tiempo de ejecución proporcione dos métodos para la asignación de la pila universal:

1. `Span<T> StackAlloc<T>(int length)`: esto tiene los mismos comportamientos y limitaciones de `localloc`, salvo que puede funcionar en cualquier tipo `T`. 
1. `Span<T> MaybeStackAlloc<T>(int length)`: este Runtime puede optar por implementar esto mediante una pila o una asignación de montón. Después, el tiempo de ejecución puede utilizar el contexto de ejecución en el que se llama para determinar cómo se asigna el `Span<T>`. Sin embargo, el autor de la llamada siempre lo tratará como si estuviera asignada a la pila.

En casos muy sencillos, como uno o dos argumentos, C# el compilador siempre podría usar `StackAlloc<T>` Variant. No es probable que contribuya significativamente al agotamiento de la pila en la mayoría de los casos. En otros casos, el compilador podría optar por usar `MaybeStackAlloc<T>` en su lugar y dejar que el Runtime realice la llamada.

La forma de elegir probablemente requerirá una investigación y un examen más profundos de las aplicaciones del mundo real. Pero si estas nuevas funciones intrínsecas están disponibles, nos proporcionará este tipo de flexibilidad.

### <a name="why-not-varargs"></a>¿Por qué no varargs? 
La característica [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) existente se consideró aquí como una posible solución. Aunque esta característica está pensada principalmente C++para escenarios de/CLI y tiene lagunas conocidas para otros escenarios. Además, hay un costo significativo en la migración de esto a Unix. Por lo tanto, no se considera una solución viable.

## <a name="related-issues"></a>Problemas relacionados
Esta especificación está relacionada con los siguientes problemas: 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

