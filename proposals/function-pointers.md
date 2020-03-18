---
ms.openlocfilehash: d9080202f9413f8beb80db222d47f5fc082ae641
ms.sourcegitcommit: f3170512e7a3193efbcea52ec330648375e36915
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79484361"
---
# <a name="function-pointers"></a>Punteros a funciones

## <a name="summary"></a>Resumen

Esta propuesta proporciona construcciones de lenguaje que exponen códigos de tiempo de IL a los que actualmente no se puede tener C# acceso de forma eficaz, o bien, en la actualidad: `ldftn` y `calli`. Estos códigos de acceso de IL pueden ser importantes en el código de alto rendimiento y los desarrolladores necesitan un método eficaz para acceder a ellos.

## <a name="motivation"></a>Motivación

Las motivaciones y el fondo de esta característica se describen en el siguiente problema (como una posible implementación de la característica):

https://github.com/dotnet/csharplang/issues/191

Se trata de una propuesta de diseño alternativa a las [funciones intrínsecas del compilador](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)

## <a name="detailed-design"></a>Diseño detallado

### <a name="function-pointers"></a>Punteros de función

El lenguaje permitirá la declaración de punteros a función mediante la sintaxis de `delegate*`. La sintaxis completa se describe en detalle en la sección siguiente, pero está diseñada para ser similar a la sintaxis usada por `Func` y `Action` declaraciones de tipos.

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

Estos tipos se representan mediante el tipo de puntero de función tal y como se describe en ECMA-335. Esto significa que la invocación de una `delegate*` usará `calli` donde la invocación de una `delegate` usará `callvirt` en el método `Invoke`.
Sintácticamente, a pesar de que la invocación es idéntica para ambas construcciones.

La definición de ECMA-335 de punteros de método incluye la Convención de llamada como parte de la signatura de tipo (sección 7,1).
La Convención de llamada predeterminada se `managed`. Se pueden especificar formas alternativas agregando el modificador adecuado después de la sintaxis de `delegate*`: `managed`, `cdecl`, `stdcall`, `thiscall`o `unmanaged`. Ejemplo:

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

Las conversiones entre `delegate*` tipos se realizan en función de su firma, incluida la Convención de llamada.

``` csharp
unsafe class Example {
    void Conversions() {
        delegate*<int, int, int> p1 = ...;
        delegate* managed<int, int, int> p2 = ...;
        delegate* cdecl<int, int, int> p3 = ...;

        p1 = p2; // okay p1 and p2 have compatible signatures
        Console.WriteLine(p2 == p1); // True
        p2 = p3; // error: calling conventions are incompatible
    }
}
```

Un tipo de `delegate*` es un tipo de puntero, lo que significa que tiene todas las capacidades y restricciones de un tipo de puntero estándar:

- Solo es válido en un contexto de `unsafe`.
- Solo se puede llamar a los métodos que contienen un parámetro `delegate*` o tipo de valor devuelto desde un contexto de `unsafe`.
- No se puede convertir en `object`.
- No se puede usar como argumento genérico.
- Puede convertir implícitamente `delegate*` en `void*`.
- Puede convertir explícitamente de `void*` a `delegate*`.

Restricciones:

- Los atributos personalizados no se pueden aplicar a un `delegate*` ni a ninguno de sus elementos.
- Un parámetro de `delegate*` no se puede marcar como `params`
- Un tipo de `delegate*` tiene todas las restricciones de un tipo de puntero normal.

### <a name="function-pointer-syntax"></a>Sintaxis de puntero de función

La sintaxis de puntero de función completa se representa mediante la siguiente gramática:

```antlr
pointer_type
    : ...
    | funcptr_type
    ;

funcptr_type
    : 'delegate' '*' calling_convention? '<' (funcptr_parameter_modifier? type ',')* funcptr_return_modifier? return_type '>'
    ;

calling_convention
    : 'cdecl'
    | 'managed'
    | 'stdcall'
    | 'thiscall'
    | 'unmanaged'
    ;

funcptr_parameter_modifier
    : 'ref'
    | 'out'
    | 'in'
    ;

funcptr_return_modifier
    : 'ref'
    | 'ref readonly'
    ;
```

La Convención de llamada de `unmanaged` representa la Convención de llamada predeterminada para el código nativo en la plataforma actual y está codificada como WINAPI.
Todas `calling_convention`s son palabras clave contextuales cuando van precedidas de un `delegate*`.

``` csharp
delegate int Func1(string s);
delegate Func1 Func2(Func1 f);

// Function pointer equivalent without calling convention
delegate*<string, int>;
delegate*<delegate*<string, int>, delegate*<string, int>>;

// Function pointer equivalent with calling convention
delegate* managed<string, int>;
delegate*<delegate* managed<string, int>, delegate*<string, int>>;
```

### <a name="function-pointer-conversions"></a>Conversiones de puntero de función

En un contexto no seguro, el conjunto de conversiones implícitas disponibles (conversiones implícitas) se extiende para incluir las siguientes conversiones de puntero implícitas:
- [_Conversiones existentes_](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- En _funcptr\_escriba_ `F0` a otro _tipo de funcptr\__ `F1`, siempre y cuando se cumplan todas las condiciones siguientes:
    - `F0` y `F1` tienen el mismo número de parámetros, y cada parámetro `D0n` en `F0` tiene los mismos modificadores `ref`, `out`o `in` que el parámetro correspondiente `D1n` en `F1`.
    - Para cada parámetro de valor (un parámetro sin `ref`, `out`o `in`), existe una conversión de identidad, una conversión de referencia implícita o una conversión de puntero implícita desde el tipo de parámetro de `F0` al tipo de parámetro correspondiente en `F1`.
    - Para cada `ref`, `out`o `in` parámetro, el tipo de parámetro en `F0` es el mismo que el tipo de parámetro correspondiente en `F1`.
    - Si el tipo de valor devuelto es por valor (no `ref` ni `ref readonly`), existe una identidad, una referencia implícita o una conversión de puntero implícita del tipo de valor devuelto de `F1` al tipo de valor devuelto de `F0`.
    - Si el tipo de valor devuelto es por referencia (`ref` o `ref readonly`), el tipo de valor devuelto y los modificadores de `ref` de `F1` son los mismos que el tipo de valor devuelto y los modificadores de `ref` de `F0`.
    - La Convención de llamada de `F0` es la misma que la Convención de llamada de `F1`.

### <a name="allow-address-of-to-target-methods"></a>Permitir la dirección de los métodos de destino

Ahora se permitirá a los grupos de métodos como argumentos para una expresión Address-of. El tipo de esta expresión será una `delegate*` que tenga la firma equivalente del método de destino y una Convención de llamada administrada:

``` csharp
unsafe class Util {
    public static void Log() { }

    void Use() {
        delegate*<void> ptr1 = &Util.Log;

        // Error: type "delegate*<void>" not compatible with "delegate*<int>";
        delegate*<int> ptr2 = &Util.Log;

        // Okay. Conversion to void* is always allowed.
        void* v = &Util.Log;
   }
}
```

En un contexto no seguro, un método `M` es compatible con un tipo de puntero de función `F` si se cumplen todas las condiciones siguientes:
- `M` y `F` tienen el mismo número de parámetros y cada parámetro de `D` tiene los mismos modificadores `ref`, `out`o `in` que el parámetro correspondiente de `F`.
- Para cada parámetro de valor (un parámetro sin `ref`, `out`o `in`), existe una conversión de identidad, una conversión de referencia implícita o una conversión de puntero implícita desde el tipo de parámetro de `M` al tipo de parámetro correspondiente en `F`.
- Para cada `ref`, `out`o `in` parámetro, el tipo de parámetro en `M` es el mismo que el tipo de parámetro correspondiente en `F`.
- Si el tipo de valor devuelto es por valor (no `ref` ni `ref readonly`), existe una identidad, una referencia implícita o una conversión de puntero implícita del tipo de valor devuelto de `F` al tipo de valor devuelto de `M`.
- Si el tipo de valor devuelto es por referencia (`ref` o `ref readonly`), el tipo de valor devuelto y los modificadores de `ref` de `F` son los mismos que el tipo de valor devuelto y los modificadores de `ref` de `M`.
- La Convención de llamada de `M` es la misma que la Convención de llamada de `F`.
- `M` es un método estático.

En un contexto no seguro, existe una conversión implícita de una dirección de una expresión cuyo destino es un grupo de métodos `E` a un tipo de puntero de función compatible `F` si `E` contiene al menos un método que es aplicable en su forma normal a una lista de argumentos construida mediante el uso de los tipos de parámetros y modificadores de `F`, como se describe a continuación.
- Se selecciona un único método `M` que corresponde a una invocación de método del formulario `E(A)` con las modificaciones siguientes:
   - La lista de argumentos `A` es una lista de expresiones, cada una clasificada como una variable y con el tipo y el modificador (`ref`, `out`o `in`) del _parámetro de\_formal correspondiente\_lista_ de `D`.
   - Los métodos candidatos son solo aquellos métodos que son solo aquellos que se aplican en su forma normal, no los que se aplican en su forma expandida.
- Si el algoritmo de las invocaciones de método produce un error, se produce un error en tiempo de compilación. De lo contrario, el algoritmo genera un único método mejor `M` tener el mismo número de parámetros que `F` y se considera que la conversión existe.
- El método seleccionado `M` debe ser compatible (tal y como se definió anteriormente) con el tipo de puntero de función `F`. De lo contrario, se produce un error en tiempo de compilación.
- El resultado de la conversión es un puntero de función de tipo `F`.

Existe una conversión implícita de una dirección de una expresión cuyo destino es un grupo de métodos `E` a `void*` si solo hay un método estático `M` en `E`.
Si hay un método estático, se `M`el mejor método de `E`.
De lo contrario, se produce un error en tiempo de compilación.

Esto significa que los desarrolladores pueden depender de reglas de resolución de sobrecarga para trabajar junto con el operador Address-of:

``` csharp
unsafe class Util {
    public static void Log() { }
    public static void Log(string p1) { }
    public static void Log(int i) { };

    void Use() {
        delegate*<void> a1 = &Log; // Log()
        delegate*<int, void> a2 = &Log; // Log(int i)

        // Error: ambiguous conversion from method group Log to "void*"
        void* v = &Log;
    }
```

El operador Address-of se implementará con la instrucción `ldftn`.

Restricciones de esta característica:

- Solo se aplica a los métodos marcados como `static`.
- No se pueden usar funciones locales no`static` en `&`. El lenguaje no especifica deliberadamente los detalles de implementación de estos métodos. Esto incluye si son estáticos frente a instancia o exactamente con qué firma se emiten.

### <a name="better-function-member"></a>Mejor miembro de función

Se cambiará la especificación de miembro de función mejor para incluir la siguiente línea:

> Un `delegate*` es más específico que `void*`

Esto significa que es posible sobrecargar en `void*` y un `delegate*` y seguir implementar usar el operador Address-of.

## <a name="open-issues"></a>Problemas abiertos

### <a name="nativecallableattribute"></a>NativeCallableAttribute

Se trata de un atributo usado por el CLR para evitar el prólogo administrado a en nativo al invocar a. Los métodos marcados por este atributo solo se pueden llamar desde código nativo, no administrado (no se puede llamar a métodos, crear un delegado, etc.). El atributo no es especial para mscorlib; el tiempo de ejecución tratará cualquier atributo con este nombre con la misma semántica.

Es posible que el tiempo de ejecución y el lenguaje funcionen juntos para que se admitan por completo. El lenguaje podría optar por tratar `static` miembros de dirección con un atributo de `NativeCallable` como un `delegate*` con la Convención de llamada especificada.

``` csharp
unsafe class NativeCallableExample {
    [NativeCallable(CallingConvention.CDecl)]
    static void CloseHandle(IntPtr p) => Marshal.FreeHGlobal(p);

    void Use() {
        delegate*<IntPtr, void> p1 = &CloseHandle; // Error: Invalid calling convention

        delegate* cdecl<IntPtr, void> p2 = &CloseHandle; // Okay
    }
}

```

Además, es probable que el lenguaje también desee:

- Marque cualquier llamada administrada a un método etiquetado con `NativeCallable` como un error. Dado que la función no se puede invocar desde el código administrado, el compilador debe evitar que los desarrolladores intenten una invocación.
- Evite que las conversiones de grupos de métodos `delegate` cuando el método se etiquete con `NativeCallable`.

Sin embargo, esto no es necesario para admitir `NativeCallable`. El compilador puede admitir el atributo `NativeCallable` como si usara la sintaxis existente. El programa simplemente tendría que convertirse en `void*` antes de convertirlo en la firma de `delegate*` correcta. Eso no sería peor que el soporte técnico de hoy en día.

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a>Conjunto extensible de convenciones de llamada no administradas

El conjunto de convenciones de llamada no administradas admitidas por las codificaciones ECMA-335 actual no está actualizado. Hemos detectado solicitudes para agregar compatibilidad para otras convenciones de llamada no administradas, por ejemplo:

- https://github.com/dotnet/coreclr/issues/12120 [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall)
- StdCall con este https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750 explícito

El diseño de esta característica debe permitir extender el conjunto de convenciones de llamada no administradas según sea necesario en el futuro. Entre los problemas se incluye el espacio limitado para las convenciones de llamada de codificación (12 de 16 valores se toman en `IMAGE_CEE_CS_CALLCONV_MASK`) y el número de lugares que deben tocarse para agregar una nueva Convención de llamada. Una posible solución consiste en introducir una nueva codificación que represente la Convención de llamada mediante [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enumeración.

Como referencia, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h tiene la lista de convenciones de llamada admitidas por LLVM. Aunque es improbable que .NET deba admitir todos ellos, se demuestra que el espacio de las convenciones de llamada es muy rico.

## <a name="considerations"></a>Consideraciones

### <a name="allow-instance-methods"></a>Permitir métodos de instancia

La propuesta podría ampliarse para admitir métodos de instancia aprovechando las ventajas de la Convención de llamada de la CLI C# de `EXPLICITTHIS` (denominada `instance` en el código). Esta forma de punteros de función de la CLI coloca el parámetro `this` como primer parámetro explícito de la sintaxis de puntero de función.

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

Este es un sonido, pero agrega cierta complicación a la propuesta. En particular, dado que los punteros de función que difieren en la Convención de llamada `instance` y `managed` serían incompatibles aunque ambos casos se usen para C# invocar métodos administrados con la misma firma. Además, en todos los casos en los que sería útil tener una solución sencilla: usar una función local `static`.

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a>No requerir Unsafe en la declaración

En lugar de requerir `unsafe` en cada uso de un `delegate*`, solo es necesario en el punto en el que se convierte un grupo de métodos en un `delegate*`. Aquí es donde entran en juego los problemas de seguridad principales (sabiendo que no se puede descargar el ensamblado contenedor mientras el valor está activo). La exigencia de `unsafe` en las otras ubicaciones puede verse como excesiva.

Así es como se diseñó originalmente el diseño. Sin embargo, las reglas de lenguaje resultaron muy poco complicadas. No es posible ocultar el hecho de que se trata de un valor de puntero y de que se conserve la inspección incluso sin la palabra clave `unsafe`. Por ejemplo, no se permite la conversión a `object`, no puede ser miembro de un `class`, etc... El C# diseño es requerir `unsafe` para todos los usos de los punteros y, por lo tanto, este diseño lo sigue.

Los desarrolladores seguirán pudiendo presentar un contenedor _seguro_ sobre los valores de `delegate*` de la misma manera que en los tipos de puntero normales hoy en día. Considere:

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a>Usar delegados

En lugar de usar un nuevo elemento de sintaxis, `delegate*`, simplemente use los tipos de `delegate` existentes con un `*` después del tipo:

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

La administración de la Convención de llamada se puede realizar anotando los tipos de `delegate` con un atributo que especifica un valor de `CallingConvention`. La falta de un atributo significaría la Convención de llamada administrada.

La codificación en IL es problemática. El valor subyacente debe representarse como un puntero, pero también debe:

1. Tener un tipo único para permitir sobrecargas con distintos tipos de puntero de función.
1. Ser equivalente a fines de OHI en los límites de los ensamblados.

El último punto es especialmente problemático. Esto significa que cada ensamblado que usa `Func<int>*` debe codificar un tipo equivalente en los metadatos, aunque `Func<int>*` se haya definido en un ensamblado aunque no sea de control.
Además, cualquier otro tipo que se defina con el nombre `System.Func<T>` en un ensamblado que no sea mscorlib debe ser diferente de la versión definida en mscorlib.

Una opción que se exploró fue emitir este tipo de puntero como `mod_req(Func<int>) void*`. Esto no funciona aunque como `mod_req` no se puede enlazar a un `TypeSpec` y, por tanto, no puede tener como destino instancias genéricas.

### <a name="named-function-pointers"></a>Punteros de función con nombre

La sintaxis de puntero de función puede ser complicada, especialmente en casos complejos como punteros de función anidados. En lugar de hacer que los desarrolladores escriban la firma cada vez que el lenguaje pueda permitir declaraciones con nombre de punteros de función, como se hace con `delegate`.

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

Parte del problema aquí es que el primitivo subyacente de la CLI no tiene nombres, por lo tanto C# , esto sería puramente una invención y requería un poco de trabajo de metadatos para habilitarlo. Es decir, factible, pero es una tarea importante sobre el trabajo. En esencia, C# es necesario tener un complemento a la tabla de tipo Def únicamente para estos nombres.

Además, cuando se examinan los argumentos de los punteros de función con nombre, encontramos que podrían aplicarse igualmente bien a otros escenarios. Por ejemplo, sería tan práctico declarar tuplas con nombre para reducir la necesidad de escribir la firma completa en todos los casos.

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

Después de la explicación, decidimos no permitir la declaración con nombre de tipos de `delegate*`. Si encontramos una necesidad importante para esto en función de los comentarios de uso de los clientes, investigaremos una solución de nomenclatura que funciona para punteros de función, tuplas, genéricos, etc. Es probable que esto sea similar en forma de otras sugerencias, como la compatibilidad completa de `typedef` en el lenguaje.

## <a name="future-considerations"></a>Consideraciones futuras

### <a name="static-local-functions"></a>funciones locales estáticas

Esto hace referencia a [la propuesta](https://github.com/dotnet/csharplang/issues/1565) para permitir el modificador `static` en las funciones locales. Tal función se garantiza que se emitirá como `static` y con la firma exacta especificada en el código fuente. Este tipo de función debe ser un argumento válido para `&` porque no contiene ninguno de los problemas que las funciones locales tienen actualmente.

### <a name="static-delegates"></a>delegados estáticos

Esto hace referencia a [la propuesta](https://github.com/dotnet/csharplang/issues/302) para permitir la declaración de tipos de `delegate` que solo pueden hacer referencia a miembros de `static`. La ventaja es que estas instancias de `delegate` se pueden asignar de forma gratuita y mejor en escenarios sensibles al rendimiento.

Si se implementa la característica de puntero de función, es probable que la propuesta de `static delegate` se cierre. La ventaja propuesta de esa característica es la naturaleza gratuita de la asignación. Sin embargo, las investigaciones recientes han detectado que no es posible lograr debido a la descarga de ensamblados. Debe haber un identificador seguro desde el `static delegate` al método al que hace referencia para evitar que el ensamblado se descargue fuera de él.

Para mantener cada `static delegate` instancia sería necesario asignar un nuevo identificador que ejecute el contador en los objetivos de la propuesta. Había algunos diseños en los que la asignación podía amortizarse en una única asignación por sitio de llamada, pero esto era un poco complejo y no parecía merecer la pena para el compromiso.

Esto significa que los desarrolladores tienen que decidir esencialmente entre las siguientes ventajas e inconvenientes:

1. Seguridad en el aspecto de la descarga de ensamblados: requiere asignaciones y, por tanto, `delegate` ya es una opción suficiente.
1. No hay seguridad en la descarga de ensamblados: Use un `delegate*`. Esto se puede ajustar en un `struct` para permitir el uso fuera de un contexto `unsafe` en el resto del código.
