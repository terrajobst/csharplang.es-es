---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483557"
---
# <a name="compiler-intrinsics"></a><span data-ttu-id="b3cfa-101">Intrínsecos del controlador</span><span class="sxs-lookup"><span data-stu-id="b3cfa-101">Compiler Intrinsics</span></span>

## <a name="summary"></a><span data-ttu-id="b3cfa-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="b3cfa-102">Summary</span></span>

<span data-ttu-id="b3cfa-103">Esta propuesta proporciona construcciones de lenguaje que exponen códigos de tiempo de IL de bajo nivel a los que actualmente no se puede tener acceso de forma eficaz, o en absoluto: `ldftn`, `ldvirtftn`, `ldtoken` y `calli`.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-103">This proposal provides language constructs that expose low level IL opcodes that cannot currently be accessed efficiently, or at all: `ldftn`, `ldvirtftn`, `ldtoken` and `calli`.</span></span> <span data-ttu-id="b3cfa-104">Estos códigos de acceso de bajo nivel pueden ser importantes en el código de alto rendimiento y los desarrolladores necesitan una manera eficaz de acceder a ellos.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-104">These low level opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="b3cfa-105">Motivación</span><span class="sxs-lookup"><span data-stu-id="b3cfa-105">Motivation</span></span>

<span data-ttu-id="b3cfa-106">Las motivaciones y el fondo de esta característica se describen en el siguiente problema (como una posible implementación de la característica):</span><span class="sxs-lookup"><span data-stu-id="b3cfa-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span> 

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="b3cfa-107">Esta propuesta de diseño alternativa viene después de revisar una implementación de prototipo de la propuesta original, @msjabby así como el uso a lo largo de una base de código importante.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-107">This alternate design proposal comes after reviewing a prototype implementation of the original proposal by @msjabby as well as the use throughout a significant code base.</span></span> <span data-ttu-id="b3cfa-108">Este diseño se realizó con una entrada importante de @mjsabby, @tmat y @jkotas.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-108">This design was done with significant input from @mjsabby, @tmat and @jkotas.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b3cfa-109">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="b3cfa-109">Detailed Design</span></span> 

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="b3cfa-110">Permitir la dirección de a los métodos de destino</span><span class="sxs-lookup"><span data-stu-id="b3cfa-110">Allow address of to target methods</span></span>

<span data-ttu-id="b3cfa-111">Ahora se permitirá a los grupos de métodos como argumentos para una expresión Address-of.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-111">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="b3cfa-112">El tipo de esta expresión se `void*`.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-112">The type of such an expression will be `void*`.</span></span> 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

<span data-ttu-id="b3cfa-113">Dado que no hay ninguna conversión de delegado, el único mecanismo para filtrar los miembros del grupo de métodos es el acceso estático/de instancia.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-113">Given there is no delegate conversion here the only mechanism for filtering members in the method group is by static / instance access.</span></span> <span data-ttu-id="b3cfa-114">Si eso no puede distinguir los miembros, se producirá un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-114">If that cannot distinguish the members then a compile time error will occur.</span></span>

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

<span data-ttu-id="b3cfa-115">La expresión AddressOf en este contexto se implementará de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="b3cfa-115">The addressof expression in this context will be implemented in the following manner:</span></span>

- <span data-ttu-id="b3cfa-116">ldftn: cuando el método no es virtual.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-116">ldftn: when the method is non-virtual.</span></span>
- <span data-ttu-id="b3cfa-117">ldvirtftn: cuando el método es virtual.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-117">ldvirtftn: when the method is virtual.</span></span>

<span data-ttu-id="b3cfa-118">Restricciones de esta característica:</span><span class="sxs-lookup"><span data-stu-id="b3cfa-118">Restrictions of this feature:</span></span>

- <span data-ttu-id="b3cfa-119">Los métodos de instancia solo se pueden especificar cuando se usa una expresión de invocación en un valor.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-119">Instance methods can only be specified when using an invocation expression on a value</span></span>
- <span data-ttu-id="b3cfa-120">Las funciones locales no se pueden usar en `&`.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-120">Local functions cannot be used in `&`.</span></span> <span data-ttu-id="b3cfa-121">El lenguaje no especifica deliberadamente los detalles de implementación de estos métodos.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-121">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="b3cfa-122">Esto incluye si son estáticos frente a instancia o exactamente con qué firma se emiten.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-122">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="handleof"></a><span data-ttu-id="b3cfa-123">Controle</span><span class="sxs-lookup"><span data-stu-id="b3cfa-123">handleof</span></span>

<span data-ttu-id="b3cfa-124">La palabra clave contextual `handleof` traducirá un campo, miembro o tipo en su tipo de `RuntimeHandle` equivalente mediante la instrucción `ldtoken`.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-124">The `handleof` contextual keyword will translate a field, member or type into their equivalent `RuntimeHandle` type using the `ldtoken` instruction.</span></span> <span data-ttu-id="b3cfa-125">El tipo exacto de la expresión dependerá del tipo del nombre en `handleof`:</span><span class="sxs-lookup"><span data-stu-id="b3cfa-125">The exact type of the expression will depend on the kind of the name in `handleof`:</span></span>

- <span data-ttu-id="b3cfa-126">campo: `RuntimeFieldHandle`</span><span class="sxs-lookup"><span data-stu-id="b3cfa-126">field: `RuntimeFieldHandle`</span></span>
- <span data-ttu-id="b3cfa-127">Tipo: `RuntimeTypeHandle`</span><span class="sxs-lookup"><span data-stu-id="b3cfa-127">type: `RuntimeTypeHandle`</span></span>
- <span data-ttu-id="b3cfa-128">método: `RuntimeMethodHandle`</span><span class="sxs-lookup"><span data-stu-id="b3cfa-128">method: `RuntimeMethodHandle`</span></span>

<span data-ttu-id="b3cfa-129">Los argumentos para `handleof` son idénticos a `nameof`.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-129">The arguments to `handleof` are identical to `nameof`.</span></span> <span data-ttu-id="b3cfa-130">Debe ser un nombre sencillo, nombre completo, acceso a miembros, acceso base con un miembro especificado o este acceso con un miembro especificado.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-130">It must be a simple name, qualified name, member access, base access with a specified member, or this access with a specified member.</span></span> <span data-ttu-id="b3cfa-131">La expresión de argumento identifica una definición de código, pero nunca se evalúa.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-131">The argument expression identifies a code definition, but it is never evaluated.</span></span>

<span data-ttu-id="b3cfa-132">La expresión `handleof` se evalúa en tiempo de ejecución y tiene un tipo de valor devuelto de `RuntimeHandle`.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-132">The `handleof` expression is evaluated at runtime and has a return type of `RuntimeHandle`.</span></span> <span data-ttu-id="b3cfa-133">Se puede ejecutar en código seguro y no seguro.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-133">This can be executed in safe code as well as unsafe.</span></span> 

``` 
RuntimeHandle stringHandle = handleof(string);
```

<span data-ttu-id="b3cfa-134">Restricciones de esta característica:</span><span class="sxs-lookup"><span data-stu-id="b3cfa-134">Restrictions of this feature:</span></span>

- <span data-ttu-id="b3cfa-135">Las propiedades no se pueden usar en una expresión `handleof`.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-135">Properties cannot be used in a `handleof` expression.</span></span>
- <span data-ttu-id="b3cfa-136">No se puede usar la expresión `handleof` cuando hay un nombre de `handleof` existente en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-136">The `handleof` expression cannot be used when there is an existing `handleof` name in scope.</span></span> <span data-ttu-id="b3cfa-137">Por ejemplo, un tipo, un espacio de nombres, etc...</span><span class="sxs-lookup"><span data-stu-id="b3cfa-137">For example a type, namespace, etc ...</span></span>

### <a name="calli"></a><span data-ttu-id="b3cfa-138">calli chybí</span><span class="sxs-lookup"><span data-stu-id="b3cfa-138">calli</span></span>

<span data-ttu-id="b3cfa-139">El compilador agregará compatibilidad con un nuevo tipo de función `extern` que se traduce eficazmente en una instrucción `.calli`.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-139">The compiler will add support for a new type of `extern` function that efficiently translates into a `.calli` instruction.</span></span> <span data-ttu-id="b3cfa-140">El atributo extern se marcará con un atributo de la forma siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3cfa-140">The extern attribute will be marked with an attribute of the following shape:</span></span>

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

<span data-ttu-id="b3cfa-141">Esto permite a los desarrolladores definir métodos de la forma siguiente:</span><span class="sxs-lookup"><span data-stu-id="b3cfa-141">This allows developers to define methods in the following form:</span></span>

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

<span data-ttu-id="b3cfa-142">Restricciones en el método que tiene aplicado el atributo `CallIndirect`:</span><span class="sxs-lookup"><span data-stu-id="b3cfa-142">Restrictions on the method which has the `CallIndirect` attribute applied:</span></span>

- <span data-ttu-id="b3cfa-143">No se puede tener un atributo `DllImport`.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-143">Cannot have a `DllImport` attribute.</span></span>
- <span data-ttu-id="b3cfa-144">No puede ser genérico.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-144">Cannot be generic.</span></span>

## <a name="open-issues"></a><span data-ttu-id="b3cfa-145">Problemas abiertos</span><span class="sxs-lookup"><span data-stu-id="b3cfa-145">Open Issues</span></span>

### <a name="callingconvention"></a><span data-ttu-id="b3cfa-146">CallingConvention</span><span class="sxs-lookup"><span data-stu-id="b3cfa-146">CallingConvention</span></span>

<span data-ttu-id="b3cfa-147">El `CallIndirectAttribute` tal y como se ha diseñado utiliza la enumeración `CallingConvention` que carece de una entrada para las convenciones de llamada administradas.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-147">The `CallIndirectAttribute` as designed uses the `CallingConvention` enum which lacks an entry for managed calling conventions.</span></span> <span data-ttu-id="b3cfa-148">La enumeración debe extenderse para incluir esta Convención de llamada o el atributo debe adoptar un enfoque diferente.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-148">The enum either needs to be extended to include this calling convention or the attribute needs to take a different approach.</span></span>

## <a name="considerations"></a><span data-ttu-id="b3cfa-149">Consideraciones</span><span class="sxs-lookup"><span data-stu-id="b3cfa-149">Considerations</span></span>

### <a name="disambiguating-method-groups"></a><span data-ttu-id="b3cfa-150">Grupos de métodos ambigüedad</span><span class="sxs-lookup"><span data-stu-id="b3cfa-150">Disambiguating method groups</span></span>

<span data-ttu-id="b3cfa-151">Se ha producido una explicación de las características que facilitarían la ambigüedad entre los grupos de métodos pasados a una expresión Address-of.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-151">There was some discussion around features that would make it easier to disambiguate method groups passed to an address-of expression.</span></span> <span data-ttu-id="b3cfa-152">Por ejemplo, puede agregar elementos de firma a la sintaxis:</span><span class="sxs-lookup"><span data-stu-id="b3cfa-152">For instance potentially adding signature elements to the syntax:</span></span>

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

<span data-ttu-id="b3cfa-153">Esto se rechazó porque no se pudo crear un caso atractivo, ni podría usarse aquí una sintaxis sencilla.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-153">This was rejected because a compelling case could not be made nor could a simple syntax be envisioned here.</span></span> <span data-ttu-id="b3cfa-154">También hay un trabajo de avance bastante sencillo: defina otro método que no sea ambiguo y que use C# código para llamar a la función deseada.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-154">Also there is a fairly straight forward work around: simple define another method that is unambiguous and uses C# code to call into the desired function.</span></span> 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

<span data-ttu-id="b3cfa-155">Esto es incluso más sencillo si `static` funciones locales especifican el idioma.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-155">This becomes even simpler if `static` local functions enter the language.</span></span> <span data-ttu-id="b3cfa-156">La solución alternativa podría definirse en la misma función que usaba la dirección de la operación ambigua:</span><span class="sxs-lookup"><span data-stu-id="b3cfa-156">Then the work around could be defined in the same function that used the ambiguous address-of operation:</span></span>

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a><span data-ttu-id="b3cfa-157">LoadTypeTokenInt32</span><span class="sxs-lookup"><span data-stu-id="b3cfa-157">LoadTypeTokenInt32</span></span>

<span data-ttu-id="b3cfa-158">La propuesta original permitía cargar los tokens de metadatos como valores `int` en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-158">The original proposal allowed for metadata tokens to be loaded as `int` values at compile time.</span></span> <span data-ttu-id="b3cfa-159">Esencialmente, tienen `tokenof` que tiene los mismos argumentos que `handleof` pero se evalúa en tiempo de compilación en una constante `int`.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-159">Essentially have `tokenof` that has the same arguments as `handleof` but is evaluated at compile time to an `int` constant.</span></span> 

<span data-ttu-id="b3cfa-160">Esto se rechazó porque causa un problema significativo en las reescrituras de IL (de las que .NET tiene muchos).</span><span class="sxs-lookup"><span data-stu-id="b3cfa-160">This was rejected as it causes significant problem for IL rewrites (of which .NET has many).</span></span> <span data-ttu-id="b3cfa-161">Estos reescrituras suelen manipular las tablas de metadatos de una manera que podría invalidar estos valores.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-161">Such rewriters often manipulate the metadata tables in a way that could invalidate these values.</span></span> <span data-ttu-id="b3cfa-162">No hay ninguna manera razonable de que estos reescrituras actualicen estos valores cuando se almacenan como valores `int` simples.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-162">There is no reasonable way for such rewriters to update these values when they are stored as simple `int` values.</span></span>

<span data-ttu-id="b3cfa-163">El equipo en tiempo de ejecución seguirá explorando la idea subyacente de tener un identificador opaco para las entradas de metadatos.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-163">The underlying idea of having an opaque handle for metadata entries will continue to be explored by the runtime team.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="b3cfa-164">Consideraciones futuras</span><span class="sxs-lookup"><span data-stu-id="b3cfa-164">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="b3cfa-165">funciones locales estáticas</span><span class="sxs-lookup"><span data-stu-id="b3cfa-165">static local functions</span></span>

<span data-ttu-id="b3cfa-166">Esto hace referencia a [la propuesta](https://github.com/dotnet/csharplang/issues/1565) para permitir el modificador `static` en las funciones locales.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-166">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="b3cfa-167">Tal función se garantiza que se emitirá como `static` y con la firma exacta especificada en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-167">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="b3cfa-168">Este tipo de función debe ser un argumento válido para `&` porque no contiene ninguno de los problemas que las funciones locales tienen actualmente.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-168">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today.</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="b3cfa-169">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="b3cfa-169">NativeCallableAttribute</span></span>

<span data-ttu-id="b3cfa-170">El CLR tiene una característica que permite emitir métodos administrados de forma que se puedan llamar directamente desde código nativo.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-170">The CLR has a feature that allows for managed methods to be emitted in such a way that they are directly callable from native code.</span></span> <span data-ttu-id="b3cfa-171">Esto se hace agregando el `NativeCallableAttribute` a los métodos.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-171">This is done by adding the `NativeCallableAttribute` to methods.</span></span> <span data-ttu-id="b3cfa-172">Este tipo de método solo se puede llamar desde código nativo y, por lo tanto, debe contener solo tipos que pueden transferirse en bytes en la signatura.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-172">Such a method is only callable from native code and hence must contain only blittable types in the signature.</span></span> <span data-ttu-id="b3cfa-173">La llamada a desde código administrado produce un error en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-173">Calling from managed code results in a runtime error.</span></span> 

<span data-ttu-id="b3cfa-174">Esta característica tendría un patrón adecuado con esta propuesta, tal y como lo permitiría:</span><span class="sxs-lookup"><span data-stu-id="b3cfa-174">This feature would pattern well with this proposal as it would allow:</span></span>

- <span data-ttu-id="b3cfa-175">Pasar una función definida en código administrado a código nativo como un puntero de función (a través de la dirección) sin sobrecarga en código administrado o nativo.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-175">Passing a function defined in managed code to native code as a function pointer (via address-of) with no overhead in managed or native code.</span></span> 
- <span data-ttu-id="b3cfa-176">El tiempo de ejecución puede introducir errores de sitio de uso para estas funciones en código administrado para evitar que se invoquen en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="b3cfa-176">Runtime can introduce use site errors for such functions in managed code to prevent them from being invoked at compile time.</span></span>




