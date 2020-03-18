---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483557"
---
# <a name="compiler-intrinsics"></a>Intrínsecos del controlador

## <a name="summary"></a>Resumen

Esta propuesta proporciona construcciones de lenguaje que exponen códigos de tiempo de IL de bajo nivel a los que actualmente no se puede tener acceso de forma eficaz, o en absoluto: `ldftn`, `ldvirtftn`, `ldtoken` y `calli`. Estos códigos de acceso de bajo nivel pueden ser importantes en el código de alto rendimiento y los desarrolladores necesitan una manera eficaz de acceder a ellos.

## <a name="motivation"></a>Motivación

Las motivaciones y el fondo de esta característica se describen en el siguiente problema (como una posible implementación de la característica): 

https://github.com/dotnet/csharplang/issues/191

Esta propuesta de diseño alternativa viene después de revisar una implementación de prototipo de la propuesta original, @msjabby así como el uso a lo largo de una base de código importante. Este diseño se realizó con una entrada importante de @mjsabby, @tmat y @jkotas.

## <a name="detailed-design"></a>Diseño detallado 

### <a name="allow-address-of-to-target-methods"></a>Permitir la dirección de a los métodos de destino

Ahora se permitirá a los grupos de métodos como argumentos para una expresión Address-of. El tipo de esta expresión se `void*`. 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

Dado que no hay ninguna conversión de delegado, el único mecanismo para filtrar los miembros del grupo de métodos es el acceso estático/de instancia. Si eso no puede distinguir los miembros, se producirá un error en tiempo de compilación.

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

La expresión AddressOf en este contexto se implementará de la siguiente manera:

- ldftn: cuando el método no es virtual.
- ldvirtftn: cuando el método es virtual.

Restricciones de esta característica:

- Los métodos de instancia solo se pueden especificar cuando se usa una expresión de invocación en un valor.
- Las funciones locales no se pueden usar en `&`. El lenguaje no especifica deliberadamente los detalles de implementación de estos métodos. Esto incluye si son estáticos frente a instancia o exactamente con qué firma se emiten.

### <a name="handleof"></a>Controle

La palabra clave contextual `handleof` traducirá un campo, miembro o tipo en su tipo de `RuntimeHandle` equivalente mediante la instrucción `ldtoken`. El tipo exacto de la expresión dependerá del tipo del nombre en `handleof`:

- campo: `RuntimeFieldHandle`
- Tipo: `RuntimeTypeHandle`
- método: `RuntimeMethodHandle`

Los argumentos para `handleof` son idénticos a `nameof`. Debe ser un nombre sencillo, nombre completo, acceso a miembros, acceso base con un miembro especificado o este acceso con un miembro especificado. La expresión de argumento identifica una definición de código, pero nunca se evalúa.

La expresión `handleof` se evalúa en tiempo de ejecución y tiene un tipo de valor devuelto de `RuntimeHandle`. Se puede ejecutar en código seguro y no seguro. 

``` 
RuntimeHandle stringHandle = handleof(string);
```

Restricciones de esta característica:

- Las propiedades no se pueden usar en una expresión `handleof`.
- No se puede usar la expresión `handleof` cuando hay un nombre de `handleof` existente en el ámbito. Por ejemplo, un tipo, un espacio de nombres, etc...

### <a name="calli"></a>calli chybí

El compilador agregará compatibilidad con un nuevo tipo de función `extern` que se traduce eficazmente en una instrucción `.calli`. El atributo extern se marcará con un atributo de la forma siguiente:

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

Esto permite a los desarrolladores definir métodos de la forma siguiente:

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

Restricciones en el método que tiene aplicado el atributo `CallIndirect`:

- No se puede tener un atributo `DllImport`.
- No puede ser genérico.

## <a name="open-issues"></a>Problemas abiertos

### <a name="callingconvention"></a>CallingConvention

El `CallIndirectAttribute` tal y como se ha diseñado utiliza la enumeración `CallingConvention` que carece de una entrada para las convenciones de llamada administradas. La enumeración debe extenderse para incluir esta Convención de llamada o el atributo debe adoptar un enfoque diferente.

## <a name="considerations"></a>Consideraciones

### <a name="disambiguating-method-groups"></a>Grupos de métodos ambigüedad

Se ha producido una explicación de las características que facilitarían la ambigüedad entre los grupos de métodos pasados a una expresión Address-of. Por ejemplo, puede agregar elementos de firma a la sintaxis:

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

Esto se rechazó porque no se pudo crear un caso atractivo, ni podría usarse aquí una sintaxis sencilla. También hay un trabajo de avance bastante sencillo: defina otro método que no sea ambiguo y que use C# código para llamar a la función deseada. 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

Esto es incluso más sencillo si `static` funciones locales especifican el idioma. La solución alternativa podría definirse en la misma función que usaba la dirección de la operación ambigua:

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a>LoadTypeTokenInt32

La propuesta original permitía cargar los tokens de metadatos como valores `int` en tiempo de compilación. Esencialmente, tienen `tokenof` que tiene los mismos argumentos que `handleof` pero se evalúa en tiempo de compilación en una constante `int`. 

Esto se rechazó porque causa un problema significativo en las reescrituras de IL (de las que .NET tiene muchos). Estos reescrituras suelen manipular las tablas de metadatos de una manera que podría invalidar estos valores. No hay ninguna manera razonable de que estos reescrituras actualicen estos valores cuando se almacenan como valores `int` simples.

El equipo en tiempo de ejecución seguirá explorando la idea subyacente de tener un identificador opaco para las entradas de metadatos. 

## <a name="future-considerations"></a>Consideraciones futuras

### <a name="static-local-functions"></a>funciones locales estáticas

Esto hace referencia a [la propuesta](https://github.com/dotnet/csharplang/issues/1565) para permitir el modificador `static` en las funciones locales. Tal función se garantiza que se emitirá como `static` y con la firma exacta especificada en el código fuente. Este tipo de función debe ser un argumento válido para `&` porque no contiene ninguno de los problemas que las funciones locales tienen actualmente.

### <a name="nativecallableattribute"></a>NativeCallableAttribute

El CLR tiene una característica que permite emitir métodos administrados de forma que se puedan llamar directamente desde código nativo. Esto se hace agregando el `NativeCallableAttribute` a los métodos. Este tipo de método solo se puede llamar desde código nativo y, por lo tanto, debe contener solo tipos que pueden transferirse en bytes en la signatura. La llamada a desde código administrado produce un error en tiempo de ejecución. 

Esta característica tendría un patrón adecuado con esta propuesta, tal y como lo permitiría:

- Pasar una función definida en código administrado a código nativo como un puntero de función (a través de la dirección) sin sobrecarga en código administrado o nativo. 
- El tiempo de ejecución puede introducir errores de sitio de uso para estas funciones en código administrado para evitar que se invoquen en tiempo de compilación.




