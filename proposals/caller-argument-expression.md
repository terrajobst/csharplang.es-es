---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483521"
---
# <a name="callerargumentexpression"></a>CallerArgumentExpression

* [x] propuesto
* [] Prototipo: no iniciado
* [] Implementación: no iniciada
* [] Especificación: no iniciada

## <a name="summary"></a>Resumen
[summary]: #summary

Permite a los desarrolladores capturar las expresiones que se pasan a un método para habilitar mejores mensajes de error en las API de diagnóstico y prueba y reducir las pulsaciones de teclas.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Cuando se produce un error en la validación de una aserción o un argumento, el desarrollador desea saber lo máximo posible sobre dónde y por qué se produjo el error. Sin embargo, las API de diagnóstico de hoy en día no lo facilitan totalmente. Tenga en cuenta el método siguiente:

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

Cuando se produce un error en una de las aserciones, solo se proporcionarán el nombre de archivo, el número de línea y el nombre del método en el seguimiento de la pila. El desarrollador no podrá saber qué aserción produjo un error en esta información: (s) tendrá que abrir el archivo y navegar al número de línea proporcionado para ver qué salió mal.

Este es también el motivo por el que los marcos de pruebas deben proporcionar una variedad de métodos de aserción. Con xUnit, no se usan con frecuencia `Assert.True` y `Assert.False` porque no proporcionan suficiente contexto sobre el error.

Aunque la situación es un poco mejor para la validación de argumentos porque los nombres de argumentos no válidos se muestran al desarrollador, el desarrollador debe pasar estos nombres a las excepciones manualmente. Si el ejemplo anterior se reescribió para usar la validación de argumentos tradicional en lugar de `Debug.Assert`, tendría el aspecto siguiente.

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

Observe que `nameof(array)` se debe pasar a cada excepción, aunque ya está claro del contexto qué argumento no es válido.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

En los ejemplos anteriores, incluir la cadena `"array != null"` o `"array.Length == 1"` en el mensaje de aserción ayudaría al desarrollador a determinar el error. Escriba `CallerArgumentExpression`: es un atributo que el marco de trabajo puede usar para obtener la cadena asociada a un argumento de método determinado. Lo agregaremos a `Debug.Assert` similar

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

El código fuente del ejemplo anterior seguiría siendo el mismo. Sin embargo, el código que el compilador emite realmente correspondería a

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

El compilador reconoce especialmente el atributo en `Debug.Assert`. Pasa la cadena asociada al argumento al que se hace referencia en el constructor del atributo (en este caso, `condition`) en el sitio de llamada. Cuando se produce un error en una aserción, se muestra al desarrollador la condición falsa y sabrá cuál produjo un error.

Para la validación de argumentos, el atributo no se puede usar directamente, pero se puede usar a través de una clase auxiliar:

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

Se está llevando a cabo una propuesta para agregar una clase auxiliar a la plataforma en https://github.com/dotnet/corefx/issues/17068. Si se ha implementado esta característica de lenguaje, la propuesta podría actualizarse para aprovecharse de esta característica.

### <a name="extension-methods"></a>Métodos de extensión

`CallerArgumentExpression`puede hacer referencia al parámetro `this` de un método de extensión. Por ejemplo:

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

`thisExpression` recibirá la expresión correspondiente al objeto antes del punto. Si se llama con sintaxis de método estático, por ejemplo `Ext.ShouldBe(contestant.Points, 1337)`, se comportará como si el primer parámetro no estuviera marcado `this`.

Siempre debe haber una expresión correspondiente al parámetro `this`. Incluso si una instancia de una clase llama a un método de extensión en sí mismo, por ejemplo, `this.Single()` desde dentro de un tipo de colección, el compilador exige el `this` para que `"this"` se pase. Si se cambia esta regla en el futuro, se puede considerar la posibilidad de pasar `null` o la cadena vacía.

### <a name="extra-details"></a>Detalles adicionales

- Al igual que los demás atributos de `Caller*`, como `CallerMemberName`, este atributo solo se puede usar en parámetros con valores predeterminados.
- Se permiten varios parámetros marcados con `CallerArgumentExpression`, como se muestra anteriormente.
- El espacio de nombres del atributo se `System.Runtime.CompilerServices`.
- Si se proporciona `null` o una cadena que no es un nombre de parámetro (por ejemplo, `"notAParameterName"`), el compilador pasará una cadena vacía.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

- Las personas que sepan cómo usar los descompiladores podrán ver parte del código fuente en los sitios de llamada para los métodos marcados con este atributo. Esto puede no ser deseable o inesperado para el software de código fuente cerrado.

- Aunque esto no es un error en la propia característica, una fuente de preocupación puede ser que existe una API `Debug.Assert` hoy que solo toma un `bool`. Incluso si la sobrecarga que toma un mensaje tenía su segundo parámetro marcado con este atributo y se hizo opcional, el compilador seguiría seleccionando el no-Message en la resolución de sobrecarga. Por lo tanto, la sobrecarga sin mensaje tendría que quitarse para aprovecharse de esta característica, lo que sería un cambio de interrupción binario (aunque no de origen).

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

- Si es capaz de ver el código fuente en los sitios de llamada para los métodos que usan este atributo se considera un problema, podemos hacer que los efectos del atributo participen. Los desarrolladores lo habilitarán a través de un atributo de `[assembly: EnableCallerArgumentExpression]` de todo el ensamblado que colocan en `AssemblyInfo.cs`.
  - En el caso de que los efectos del atributo no estén habilitados, llamar a los métodos marcados con el atributo no sería un error, para permitir que los métodos existentes utilicen el atributo y mantengan la compatibilidad con el origen. Sin embargo, se omitirá el atributo y se llamará al método con el valor predeterminado proporcionado.

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- Para evitar que se produzca el[ drawbacks] [problema de compatibilidad binaria] cada vez que queremos agregar nueva información de autor de llamada a `Debug.Assert`, una solución alternativa sería agregar un `CallerInfo` struct al marco de trabajo que contenga toda la información necesaria sobre el autor de la llamada.

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

Esto se propuso originalmente en https://github.com/dotnet/csharplang/issues/87.

Este enfoque tiene algunas desventajas:

- A pesar de ser muy descriptivos al permitirle especificar qué propiedades necesita, todavía podría perjudicar el rendimiento mediante la asignación de una matriz para las expresiones/llamadas `MethodBase.GetCurrentMethod` incluso cuando se pasa la aserción.

- Además, aunque pasar una nueva marca al atributo `CallerInfo` no será un cambio importante, `Debug.Assert` no se garantiza que reciba realmente ese nuevo parámetro de sitios de llamada que se compilaron con una versión anterior del método.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

TBD

## <a name="design-meetings"></a>Design Meetings

N/D
