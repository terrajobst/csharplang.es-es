---
ms.openlocfilehash: d9080202f9413f8beb80db222d47f5fc082ae641
ms.sourcegitcommit: f3170512e7a3193efbcea52ec330648375e36915
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79484361"
---
# <a name="function-pointers"></a><span data-ttu-id="bb5c8-101">Punteros a funciones</span><span class="sxs-lookup"><span data-stu-id="bb5c8-101">Function Pointers</span></span>

## <a name="summary"></a><span data-ttu-id="bb5c8-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="bb5c8-102">Summary</span></span>

<span data-ttu-id="bb5c8-103">Esta propuesta proporciona construcciones de lenguaje que exponen códigos de tiempo de IL a los que actualmente no se puede tener C# acceso de forma eficaz, o bien, en la actualidad: `ldftn` y `calli`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-103">This proposal provides language constructs that expose IL opcodes that cannot currently be accessed efficiently, or at all, in C# today: `ldftn` and `calli`.</span></span> <span data-ttu-id="bb5c8-104">Estos códigos de acceso de IL pueden ser importantes en el código de alto rendimiento y los desarrolladores necesitan un método eficaz para acceder a ellos.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-104">These IL opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="bb5c8-105">Motivación</span><span class="sxs-lookup"><span data-stu-id="bb5c8-105">Motivation</span></span>

<span data-ttu-id="bb5c8-106">Las motivaciones y el fondo de esta característica se describen en el siguiente problema (como una posible implementación de la característica):</span><span class="sxs-lookup"><span data-stu-id="bb5c8-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span>

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="bb5c8-107">Se trata de una propuesta de diseño alternativa a las [funciones intrínsecas del compilador](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span><span class="sxs-lookup"><span data-stu-id="bb5c8-107">This is an alternate design proposal to [compiler intrinsics](https://github.com/dotnet/csharplang/blob/master/proposals/intrinsics.md)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="bb5c8-108">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="bb5c8-108">Detailed Design</span></span>

### <a name="function-pointers"></a><span data-ttu-id="bb5c8-109">Punteros de función</span><span class="sxs-lookup"><span data-stu-id="bb5c8-109">Function pointers</span></span>

<span data-ttu-id="bb5c8-110">El lenguaje permitirá la declaración de punteros a función mediante la sintaxis de `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-110">The language will allow for the declaration of function pointers using the `delegate*` syntax.</span></span> <span data-ttu-id="bb5c8-111">La sintaxis completa se describe en detalle en la sección siguiente, pero está diseñada para ser similar a la sintaxis usada por `Func` y `Action` declaraciones de tipos.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-111">The full syntax is described in detail in the next section but it is meant to resemble the syntax used by `Func` and `Action` type declarations.</span></span>

``` csharp
unsafe class Example {
    void Example(Action<int> a, delegate*<int, void> f) {
        a(42);
        f(42);
    }
}
```

<span data-ttu-id="bb5c8-112">Estos tipos se representan mediante el tipo de puntero de función tal y como se describe en ECMA-335.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-112">These types are represented using the function pointer type as outlined in ECMA-335.</span></span> <span data-ttu-id="bb5c8-113">Esto significa que la invocación de una `delegate*` usará `calli` donde la invocación de una `delegate` usará `callvirt` en el método `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-113">This means invocation of a `delegate*` will use `calli` where invocation of a `delegate` will use `callvirt` on the `Invoke` method.</span></span>
<span data-ttu-id="bb5c8-114">Sintácticamente, a pesar de que la invocación es idéntica para ambas construcciones.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-114">Syntactically though invocation is identical for both constructs.</span></span>

<span data-ttu-id="bb5c8-115">La definición de ECMA-335 de punteros de método incluye la Convención de llamada como parte de la signatura de tipo (sección 7,1).</span><span class="sxs-lookup"><span data-stu-id="bb5c8-115">The ECMA-335 definition of method pointers includes the calling convention as part of the type signature (section 7.1).</span></span>
<span data-ttu-id="bb5c8-116">La Convención de llamada predeterminada se `managed`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-116">The default calling convention will be `managed`.</span></span> <span data-ttu-id="bb5c8-117">Se pueden especificar formas alternativas agregando el modificador adecuado después de la sintaxis de `delegate*`: `managed`, `cdecl`, `stdcall`, `thiscall`o `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-117">Alternate forms can be specified by adding the appropriate modifier after the `delegate*` syntax: `managed`, `cdecl`, `stdcall`, `thiscall`, or `unmanaged`.</span></span> <span data-ttu-id="bb5c8-118">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-118">Example:</span></span>

``` csharp
// This method will be invoked using the cdecl calling convention
delegate* cdecl<int, int>;

// This method will be invoked using the stdcall calling convention
delegate* stdcall<int, int>;
```

<span data-ttu-id="bb5c8-119">Las conversiones entre `delegate*` tipos se realizan en función de su firma, incluida la Convención de llamada.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-119">Conversions between `delegate*` types is done based on their signature including the calling convention.</span></span>

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

<span data-ttu-id="bb5c8-120">Un tipo de `delegate*` es un tipo de puntero, lo que significa que tiene todas las capacidades y restricciones de un tipo de puntero estándar:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-120">A `delegate*` type is a pointer type which means it has all of the capabilities and restrictions of a standard pointer type:</span></span>

- <span data-ttu-id="bb5c8-121">Solo es válido en un contexto de `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-121">Only valid in an `unsafe` context.</span></span>
- <span data-ttu-id="bb5c8-122">Solo se puede llamar a los métodos que contienen un parámetro `delegate*` o tipo de valor devuelto desde un contexto de `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-122">Methods which contain a `delegate*` parameter or return type can only be called from an `unsafe` context.</span></span>
- <span data-ttu-id="bb5c8-123">No se puede convertir en `object`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-123">Cannot be converted to `object`.</span></span>
- <span data-ttu-id="bb5c8-124">No se puede usar como argumento genérico.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-124">Cannot be used as a generic argument.</span></span>
- <span data-ttu-id="bb5c8-125">Puede convertir implícitamente `delegate*` en `void*`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-125">Can implicitly convert `delegate*` to `void*`.</span></span>
- <span data-ttu-id="bb5c8-126">Puede convertir explícitamente de `void*` a `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-126">Can explicitly convert from `void*` to `delegate*`.</span></span>

<span data-ttu-id="bb5c8-127">Restricciones:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-127">Restrictions:</span></span>

- <span data-ttu-id="bb5c8-128">Los atributos personalizados no se pueden aplicar a un `delegate*` ni a ninguno de sus elementos.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-128">Custom attributes cannot be applied to a `delegate*` or any of its elements.</span></span>
- <span data-ttu-id="bb5c8-129">Un parámetro de `delegate*` no se puede marcar como `params`</span><span class="sxs-lookup"><span data-stu-id="bb5c8-129">A `delegate*` parameter cannot be marked as `params`</span></span>
- <span data-ttu-id="bb5c8-130">Un tipo de `delegate*` tiene todas las restricciones de un tipo de puntero normal.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-130">A `delegate*` type has all of the restrictions of a normal pointer type.</span></span>

### <a name="function-pointer-syntax"></a><span data-ttu-id="bb5c8-131">Sintaxis de puntero de función</span><span class="sxs-lookup"><span data-stu-id="bb5c8-131">Function pointer syntax</span></span>

<span data-ttu-id="bb5c8-132">La sintaxis de puntero de función completa se representa mediante la siguiente gramática:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-132">The full function pointer syntax is represented by the following grammar:</span></span>

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

<span data-ttu-id="bb5c8-133">La Convención de llamada de `unmanaged` representa la Convención de llamada predeterminada para el código nativo en la plataforma actual y está codificada como WINAPI.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-133">The `unmanaged` calling convention represents the default calling convention for native code on the current platform, and is encoded as winapi.</span></span>
<span data-ttu-id="bb5c8-134">Todas `calling_convention`s son palabras clave contextuales cuando van precedidas de un `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-134">All `calling_convention`s are contextual keywords when preceded by a `delegate*`.</span></span>

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

### <a name="function-pointer-conversions"></a><span data-ttu-id="bb5c8-135">Conversiones de puntero de función</span><span class="sxs-lookup"><span data-stu-id="bb5c8-135">Function pointer conversions</span></span>

<span data-ttu-id="bb5c8-136">En un contexto no seguro, el conjunto de conversiones implícitas disponibles (conversiones implícitas) se extiende para incluir las siguientes conversiones de puntero implícitas:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-136">In an unsafe context, the set of available implicit conversions (Implicit conversions) is extended to include the following implicit pointer conversions:</span></span>
- [<span data-ttu-id="bb5c8-137">_Conversiones existentes_</span><span class="sxs-lookup"><span data-stu-id="bb5c8-137">_Existing conversions_</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#pointer-conversions)
- <span data-ttu-id="bb5c8-138">En _funcptr\_escriba_ `F0` a otro _tipo de funcptr\__ `F1`, siempre y cuando se cumplan todas las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-138">From _funcptr\_type_ `F0` to another _funcptr\_type_ `F1`, provided all of the following are true:</span></span>
    - <span data-ttu-id="bb5c8-139">`F0` y `F1` tienen el mismo número de parámetros, y cada parámetro `D0n` en `F0` tiene los mismos modificadores `ref`, `out`o `in` que el parámetro correspondiente `D1n` en `F1`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-139">`F0` and `F1` have the same number of parameters, and each parameter `D0n` in `F0` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter `D1n` in `F1`.</span></span>
    - <span data-ttu-id="bb5c8-140">Para cada parámetro de valor (un parámetro sin `ref`, `out`o `in`), existe una conversión de identidad, una conversión de referencia implícita o una conversión de puntero implícita desde el tipo de parámetro de `F0` al tipo de parámetro correspondiente en `F1`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-140">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `F0` to the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="bb5c8-141">Para cada `ref`, `out`o `in` parámetro, el tipo de parámetro en `F0` es el mismo que el tipo de parámetro correspondiente en `F1`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-141">For each `ref`, `out`, or `in` parameter, the parameter type in `F0` is the same as the corresponding parameter type in `F1`.</span></span>
    - <span data-ttu-id="bb5c8-142">Si el tipo de valor devuelto es por valor (no `ref` ni `ref readonly`), existe una identidad, una referencia implícita o una conversión de puntero implícita del tipo de valor devuelto de `F1` al tipo de valor devuelto de `F0`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-142">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F1` to the return type of `F0`.</span></span>
    - <span data-ttu-id="bb5c8-143">Si el tipo de valor devuelto es por referencia (`ref` o `ref readonly`), el tipo de valor devuelto y los modificadores de `ref` de `F1` son los mismos que el tipo de valor devuelto y los modificadores de `ref` de `F0`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-143">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F1` are the same as the return type and `ref` modifiers of `F0`.</span></span>
    - <span data-ttu-id="bb5c8-144">La Convención de llamada de `F0` es la misma que la Convención de llamada de `F1`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-144">The calling convention of `F0` is the same as the calling convention of `F1`.</span></span>

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="bb5c8-145">Permitir la dirección de los métodos de destino</span><span class="sxs-lookup"><span data-stu-id="bb5c8-145">Allow address-of to target methods</span></span>

<span data-ttu-id="bb5c8-146">Ahora se permitirá a los grupos de métodos como argumentos para una expresión Address-of.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-146">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="bb5c8-147">El tipo de esta expresión será una `delegate*` que tenga la firma equivalente del método de destino y una Convención de llamada administrada:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-147">The type of such an expression will be a `delegate*` which has the equivalent signature of the target method and a managed calling convention:</span></span>

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

<span data-ttu-id="bb5c8-148">En un contexto no seguro, un método `M` es compatible con un tipo de puntero de función `F` si se cumplen todas las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-148">In an unsafe context, a method `M` is compatible with a function pointer type `F` if all of the following are true:</span></span>
- <span data-ttu-id="bb5c8-149">`M` y `F` tienen el mismo número de parámetros y cada parámetro de `D` tiene los mismos modificadores `ref`, `out`o `in` que el parámetro correspondiente de `F`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-149">`M` and `F` have the same number of parameters, and each parameter in `D` has the same `ref`, `out`, or `in` modifiers as the corresponding parameter in `F`.</span></span>
- <span data-ttu-id="bb5c8-150">Para cada parámetro de valor (un parámetro sin `ref`, `out`o `in`), existe una conversión de identidad, una conversión de referencia implícita o una conversión de puntero implícita desde el tipo de parámetro de `M` al tipo de parámetro correspondiente en `F`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-150">For each value parameter (a parameter with no `ref`, `out`, or `in` modifier), an identity conversion, implicit reference conversion, or implicit pointer conversion exists from the parameter type in `M` to the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="bb5c8-151">Para cada `ref`, `out`o `in` parámetro, el tipo de parámetro en `M` es el mismo que el tipo de parámetro correspondiente en `F`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-151">For each `ref`, `out`, or `in` parameter, the parameter type in `M` is the same as the corresponding parameter type in `F`.</span></span>
- <span data-ttu-id="bb5c8-152">Si el tipo de valor devuelto es por valor (no `ref` ni `ref readonly`), existe una identidad, una referencia implícita o una conversión de puntero implícita del tipo de valor devuelto de `F` al tipo de valor devuelto de `M`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-152">If the return type is by value (no `ref` or `ref readonly`), an identity, implicit reference, or implicit pointer conversion exists from the return type of `F` to the return type of `M`.</span></span>
- <span data-ttu-id="bb5c8-153">Si el tipo de valor devuelto es por referencia (`ref` o `ref readonly`), el tipo de valor devuelto y los modificadores de `ref` de `F` son los mismos que el tipo de valor devuelto y los modificadores de `ref` de `M`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-153">If the return type is by reference (`ref` or `ref readonly`), the return type and `ref` modifiers of `F` are the same as the return type and `ref` modifiers of `M`.</span></span>
- <span data-ttu-id="bb5c8-154">La Convención de llamada de `M` es la misma que la Convención de llamada de `F`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-154">The calling convention of `M` is the same as the calling convention of `F`.</span></span>
- <span data-ttu-id="bb5c8-155">`M` es un método estático.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-155">`M` is a static method.</span></span>

<span data-ttu-id="bb5c8-156">En un contexto no seguro, existe una conversión implícita de una dirección de una expresión cuyo destino es un grupo de métodos `E` a un tipo de puntero de función compatible `F` si `E` contiene al menos un método que es aplicable en su forma normal a una lista de argumentos construida mediante el uso de los tipos de parámetros y modificadores de `F`, como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-156">In an unsafe context, an implicit conversion exists from an address-of expression whose target is a method group `E` to a compatible function pointer type `F` if `E` contains at least one method that is applicable in its normal form to an argument list constructed by use of the parameter types and modifiers of `F`, as described in the following.</span></span>
- <span data-ttu-id="bb5c8-157">Se selecciona un único método `M` que corresponde a una invocación de método del formulario `E(A)` con las modificaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-157">A single method `M` is selected corresponding to a method invocation of the form `E(A)` with the following modifications:</span></span>
   - <span data-ttu-id="bb5c8-158">La lista de argumentos `A` es una lista de expresiones, cada una clasificada como una variable y con el tipo y el modificador (`ref`, `out`o `in`) del _parámetro de\_formal correspondiente\_lista_ de `D`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-158">The arguments list `A` is a list of expressions, each classified as a variable and with the type and modifier (`ref`, `out`, or `in`) of the corresponding _formal\_parameter\_list_ of `D`.</span></span>
   - <span data-ttu-id="bb5c8-159">Los métodos candidatos son solo aquellos métodos que son solo aquellos que se aplican en su forma normal, no los que se aplican en su forma expandida.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-159">The candidate methods are only those methods that are only those methods that are applicable in their normal form, not those applicable in their expanded form.</span></span>
- <span data-ttu-id="bb5c8-160">Si el algoritmo de las invocaciones de método produce un error, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-160">If the algorithm of Method invocations produces an error, then a compile-time error occurs.</span></span> <span data-ttu-id="bb5c8-161">De lo contrario, el algoritmo genera un único método mejor `M` tener el mismo número de parámetros que `F` y se considera que la conversión existe.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-161">Otherwise, the algorithm produces a single best method `M` having the same number of parameters as `F` and the conversion is considered to exist.</span></span>
- <span data-ttu-id="bb5c8-162">El método seleccionado `M` debe ser compatible (tal y como se definió anteriormente) con el tipo de puntero de función `F`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-162">The selected method `M` must be compatible (as defined above) with the function pointer type `F`.</span></span> <span data-ttu-id="bb5c8-163">De lo contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-163">Otherwise, a compile-time error occurs.</span></span>
- <span data-ttu-id="bb5c8-164">El resultado de la conversión es un puntero de función de tipo `F`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-164">The result of the conversion is a function pointer of type `F`.</span></span>

<span data-ttu-id="bb5c8-165">Existe una conversión implícita de una dirección de una expresión cuyo destino es un grupo de métodos `E` a `void*` si solo hay un método estático `M` en `E`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-165">An implicit conversion exists from an address-of expression whose target is a method group `E` to `void*` if there is only one static method `M` in `E`.</span></span>
<span data-ttu-id="bb5c8-166">Si hay un método estático, se `M`el mejor método de `E`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-166">If there is one static method, then the single best method from `E` is `M`.</span></span>
<span data-ttu-id="bb5c8-167">De lo contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-167">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="bb5c8-168">Esto significa que los desarrolladores pueden depender de reglas de resolución de sobrecarga para trabajar junto con el operador Address-of:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-168">This means developers can depend on overload resolution rules to work in conjunction with the address-of operator:</span></span>

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

<span data-ttu-id="bb5c8-169">El operador Address-of se implementará con la instrucción `ldftn`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-169">The address-of operator will be implemented using the `ldftn` instruction.</span></span>

<span data-ttu-id="bb5c8-170">Restricciones de esta característica:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-170">Restrictions of this feature:</span></span>

- <span data-ttu-id="bb5c8-171">Solo se aplica a los métodos marcados como `static`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-171">Only applies to methods marked as `static`.</span></span>
- <span data-ttu-id="bb5c8-172">No se pueden usar funciones locales no`static` en `&`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-172">Non-`static` local functions cannot be used in `&`.</span></span> <span data-ttu-id="bb5c8-173">El lenguaje no especifica deliberadamente los detalles de implementación de estos métodos.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-173">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="bb5c8-174">Esto incluye si son estáticos frente a instancia o exactamente con qué firma se emiten.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-174">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="better-function-member"></a><span data-ttu-id="bb5c8-175">Mejor miembro de función</span><span class="sxs-lookup"><span data-stu-id="bb5c8-175">Better function member</span></span>

<span data-ttu-id="bb5c8-176">Se cambiará la especificación de miembro de función mejor para incluir la siguiente línea:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-176">The better function member specification will be changed to include the following line:</span></span>

> <span data-ttu-id="bb5c8-177">Un `delegate*` es más específico que `void*`</span><span class="sxs-lookup"><span data-stu-id="bb5c8-177">A `delegate*` is more specific than `void*`</span></span>

<span data-ttu-id="bb5c8-178">Esto significa que es posible sobrecargar en `void*` y un `delegate*` y seguir implementar usar el operador Address-of.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-178">This means that it is possible to overload on `void*` and a `delegate*` and still sensibly use the address-of operator.</span></span>

## <a name="open-issues"></a><span data-ttu-id="bb5c8-179">Problemas abiertos</span><span class="sxs-lookup"><span data-stu-id="bb5c8-179">Open Issues</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="bb5c8-180">NativeCallableAttribute</span><span class="sxs-lookup"><span data-stu-id="bb5c8-180">NativeCallableAttribute</span></span>

<span data-ttu-id="bb5c8-181">Se trata de un atributo usado por el CLR para evitar el prólogo administrado a en nativo al invocar a.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-181">This is an attribute used by the CLR to avoid the managed to native prologue when invoking.</span></span> <span data-ttu-id="bb5c8-182">Los métodos marcados por este atributo solo se pueden llamar desde código nativo, no administrado (no se puede llamar a métodos, crear un delegado, etc.).</span><span class="sxs-lookup"><span data-stu-id="bb5c8-182">Methods marked by this attribute are only callable from native code, not managed (can’t call methods, create a delegate, etc …).</span></span> <span data-ttu-id="bb5c8-183">El atributo no es especial para mscorlib; el tiempo de ejecución tratará cualquier atributo con este nombre con la misma semántica.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-183">The attribute is not special to mscorlib; the runtime will treat any attribute with this name with the same semantics.</span></span>

<span data-ttu-id="bb5c8-184">Es posible que el tiempo de ejecución y el lenguaje funcionen juntos para que se admitan por completo.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-184">It's possible for the runtime and language to work together to fully support this.</span></span> <span data-ttu-id="bb5c8-185">El lenguaje podría optar por tratar `static` miembros de dirección con un atributo de `NativeCallable` como un `delegate*` con la Convención de llamada especificada.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-185">The language could choose to treat address-of `static` members with a `NativeCallable` attribute as a `delegate*` with the specified calling convention.</span></span>

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

<span data-ttu-id="bb5c8-186">Además, es probable que el lenguaje también desee:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-186">Additionally the language would likely also want to:</span></span>

- <span data-ttu-id="bb5c8-187">Marque cualquier llamada administrada a un método etiquetado con `NativeCallable` como un error.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-187">Flag any managed calls to a method tagged with `NativeCallable` as an error.</span></span> <span data-ttu-id="bb5c8-188">Dado que la función no se puede invocar desde el código administrado, el compilador debe evitar que los desarrolladores intenten una invocación.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-188">Given the function can't be invoked from managed code the compiler should prevent developers from attempting such an invocation.</span></span>
- <span data-ttu-id="bb5c8-189">Evite que las conversiones de grupos de métodos `delegate` cuando el método se etiquete con `NativeCallable`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-189">Prevent method group conversions to `delegate` when the method is tagged with `NativeCallable`.</span></span>

<span data-ttu-id="bb5c8-190">Sin embargo, esto no es necesario para admitir `NativeCallable`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-190">This is not necessary to support `NativeCallable` though.</span></span> <span data-ttu-id="bb5c8-191">El compilador puede admitir el atributo `NativeCallable` como si usara la sintaxis existente.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-191">The compiler can support the `NativeCallable` attribute as is using the existing syntax.</span></span> <span data-ttu-id="bb5c8-192">El programa simplemente tendría que convertirse en `void*` antes de convertirlo en la firma de `delegate*` correcta.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-192">The program would simply need to cast to `void*` before casting to the correct `delegate*` signature.</span></span> <span data-ttu-id="bb5c8-193">Eso no sería peor que el soporte técnico de hoy en día.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-193">That would be no worse than the support today.</span></span>

``` csharp
void* v = &CloseHandle;
delegate* cdecl<IntPtr, bool> f1 = (delegate* cdecl<IntPtr, bool>)v;
```

### <a name="extensible-set-of-unmanaged-calling-conventions"></a><span data-ttu-id="bb5c8-194">Conjunto extensible de convenciones de llamada no administradas</span><span class="sxs-lookup"><span data-stu-id="bb5c8-194">Extensible set of unmanaged calling conventions</span></span>

<span data-ttu-id="bb5c8-195">El conjunto de convenciones de llamada no administradas admitidas por las codificaciones ECMA-335 actual no está actualizado.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-195">The set of unmanaged calling conventions supported by the current ECMA-335 encodings is outdated.</span></span> <span data-ttu-id="bb5c8-196">Hemos detectado solicitudes para agregar compatibilidad para otras convenciones de llamada no administradas, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-196">We have seen requests to add support for more unmanaged calling conventions, for example:</span></span>

- <span data-ttu-id="bb5c8-197">https://github.com/dotnet/coreclr/issues/12120 [vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall)</span><span class="sxs-lookup"><span data-stu-id="bb5c8-197">[vectorcall](https://docs.microsoft.com/cpp/cpp/vectorcall) https://github.com/dotnet/coreclr/issues/12120</span></span>
- <span data-ttu-id="bb5c8-198">StdCall con este https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750 explícito</span><span class="sxs-lookup"><span data-stu-id="bb5c8-198">StdCall with explicit this https://github.com/dotnet/coreclr/pull/23974#issuecomment-482991750</span></span>

<span data-ttu-id="bb5c8-199">El diseño de esta característica debe permitir extender el conjunto de convenciones de llamada no administradas según sea necesario en el futuro.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-199">The design of this feature should allow extending the set of unmanaged calling conventions as needed in future.</span></span> <span data-ttu-id="bb5c8-200">Entre los problemas se incluye el espacio limitado para las convenciones de llamada de codificación (12 de 16 valores se toman en `IMAGE_CEE_CS_CALLCONV_MASK`) y el número de lugares que deben tocarse para agregar una nueva Convención de llamada.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-200">The problems include limited space for encoding calling conventions (12 out of 16 values are taken in `IMAGE_CEE_CS_CALLCONV_MASK`) and number of places that need to be touched in order to add a new calling convention.</span></span> <span data-ttu-id="bb5c8-201">Una posible solución consiste en introducir una nueva codificación que represente la Convención de llamada mediante [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enumeración.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-201">A potential solution is to introduce a new encoding that represents the calling convention using [`System.Runtime.InteropServices.CallingConvention`](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.interopservices.callingconvention) enum.</span></span>

<span data-ttu-id="bb5c8-202">Como referencia, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h tiene la lista de convenciones de llamada admitidas por LLVM.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-202">For reference, https://github.com/llvm/llvm-project/blob/master/llvm/include/llvm/IR/CallingConv.h has the list of calling conventions supported by LLVM.</span></span> <span data-ttu-id="bb5c8-203">Aunque es improbable que .NET deba admitir todos ellos, se demuestra que el espacio de las convenciones de llamada es muy rico.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-203">While it is unlikely that .NET will ever need to support all of them, it demonstrates that the space of calling conventions is very rich.</span></span>

## <a name="considerations"></a><span data-ttu-id="bb5c8-204">Consideraciones</span><span class="sxs-lookup"><span data-stu-id="bb5c8-204">Considerations</span></span>

### <a name="allow-instance-methods"></a><span data-ttu-id="bb5c8-205">Permitir métodos de instancia</span><span class="sxs-lookup"><span data-stu-id="bb5c8-205">Allow instance methods</span></span>

<span data-ttu-id="bb5c8-206">La propuesta podría ampliarse para admitir métodos de instancia aprovechando las ventajas de la Convención de llamada de la CLI C# de `EXPLICITTHIS` (denominada `instance` en el código).</span><span class="sxs-lookup"><span data-stu-id="bb5c8-206">The proposal could be extended to support instance methods by taking advantage of the `EXPLICITTHIS` CLI calling convention (named `instance` in C# code).</span></span> <span data-ttu-id="bb5c8-207">Esta forma de punteros de función de la CLI coloca el parámetro `this` como primer parámetro explícito de la sintaxis de puntero de función.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-207">This form of CLI function pointers puts the `this` parameter as an explicit first parameter of the function pointer syntax.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        delegate* instance<Instance, string> f = &ToString;
        f(this);
    }
}
```

<span data-ttu-id="bb5c8-208">Este es un sonido, pero agrega cierta complicación a la propuesta.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-208">This is sound but adds some complication to the proposal.</span></span> <span data-ttu-id="bb5c8-209">En particular, dado que los punteros de función que difieren en la Convención de llamada `instance` y `managed` serían incompatibles aunque ambos casos se usen para C# invocar métodos administrados con la misma firma.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-209">Particularly because function pointers which differed by the calling convention `instance` and `managed` would be incompatible even though both cases are used to invoke managed methods with the same C# signature.</span></span> <span data-ttu-id="bb5c8-210">Además, en todos los casos en los que sería útil tener una solución sencilla: usar una función local `static`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-210">Also in every case considered where this would be valuable to have there was a simple work around: use a `static` local function.</span></span>

``` csharp
unsafe class Instance {
    void Use() {
        static string toString(Instance i) = i.ToString();
        delgate*<Instance, string> f = &toString;
        f(this);
    }
}
```

### <a name="dont-require-unsafe-at-declaration"></a><span data-ttu-id="bb5c8-211">No requerir Unsafe en la declaración</span><span class="sxs-lookup"><span data-stu-id="bb5c8-211">Don't require unsafe at declaration</span></span>

<span data-ttu-id="bb5c8-212">En lugar de requerir `unsafe` en cada uso de un `delegate*`, solo es necesario en el punto en el que se convierte un grupo de métodos en un `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-212">Instead of requiring `unsafe` at every use of a `delegate*`, only require it at the point where a method group is converted to a `delegate*`.</span></span> <span data-ttu-id="bb5c8-213">Aquí es donde entran en juego los problemas de seguridad principales (sabiendo que no se puede descargar el ensamblado contenedor mientras el valor está activo).</span><span class="sxs-lookup"><span data-stu-id="bb5c8-213">This is where the core safety issues come into play (knowing that the containing assembly cannot be unloaded while the value is alive).</span></span> <span data-ttu-id="bb5c8-214">La exigencia de `unsafe` en las otras ubicaciones puede verse como excesiva.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-214">Requiring `unsafe` on the other locations can be seen as excessive.</span></span>

<span data-ttu-id="bb5c8-215">Así es como se diseñó originalmente el diseño.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-215">This is how the design was originally intended.</span></span> <span data-ttu-id="bb5c8-216">Sin embargo, las reglas de lenguaje resultaron muy poco complicadas.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-216">But the resulting language rules felt very awkward.</span></span> <span data-ttu-id="bb5c8-217">No es posible ocultar el hecho de que se trata de un valor de puntero y de que se conserve la inspección incluso sin la palabra clave `unsafe`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-217">It's impossible to hide the fact that this is a pointer value and it kept peeking through even without the `unsafe` keyword.</span></span> <span data-ttu-id="bb5c8-218">Por ejemplo, no se permite la conversión a `object`, no puede ser miembro de un `class`, etc... El C# diseño es requerir `unsafe` para todos los usos de los punteros y, por lo tanto, este diseño lo sigue.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-218">For example the conversion to `object` can't be allowed, it can't be a member of a `class`, etc ... The C# design is to require `unsafe` for all pointer uses and hence this design follows that.</span></span>

<span data-ttu-id="bb5c8-219">Los desarrolladores seguirán pudiendo presentar un contenedor _seguro_ sobre los valores de `delegate*` de la misma manera que en los tipos de puntero normales hoy en día.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-219">Developers will still be capable of presenting a _safe_ wrapper on top of `delegate*` values the same way that they do for normal pointer types today.</span></span> <span data-ttu-id="bb5c8-220">Considere:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-220">Consider:</span></span>

``` csharp
unsafe struct Action {
    delegate*<void> _ptr;

    Action(delegate*<void> ptr) => _ptr = ptr;
    public void Invoke() => _ptr();
}
```

### <a name="using-delegates"></a><span data-ttu-id="bb5c8-221">Usar delegados</span><span class="sxs-lookup"><span data-stu-id="bb5c8-221">Using delegates</span></span>

<span data-ttu-id="bb5c8-222">En lugar de usar un nuevo elemento de sintaxis, `delegate*`, simplemente use los tipos de `delegate` existentes con un `*` después del tipo:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-222">Instead of using a new syntax element, `delegate*`, simply use existing `delegate` types with a `*` following the type:</span></span>

``` csharp
Func<object, object, bool>* ptr = &object.ReferenceEquals;
```

<span data-ttu-id="bb5c8-223">La administración de la Convención de llamada se puede realizar anotando los tipos de `delegate` con un atributo que especifica un valor de `CallingConvention`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-223">Handling calling convention can be done by annotating the `delegate` types with an attribute that specifies a `CallingConvention` value.</span></span> <span data-ttu-id="bb5c8-224">La falta de un atributo significaría la Convención de llamada administrada.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-224">The lack of an attribute would signify the managed calling convention.</span></span>

<span data-ttu-id="bb5c8-225">La codificación en IL es problemática.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-225">Encoding this in IL is problematic.</span></span> <span data-ttu-id="bb5c8-226">El valor subyacente debe representarse como un puntero, pero también debe:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-226">The underlying value needs to be represented as a pointer yet it also must:</span></span>

1. <span data-ttu-id="bb5c8-227">Tener un tipo único para permitir sobrecargas con distintos tipos de puntero de función.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-227">Have a unique type to allow for overloads with different function pointer types.</span></span>
1. <span data-ttu-id="bb5c8-228">Ser equivalente a fines de OHI en los límites de los ensamblados.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-228">Be equivalent for OHI purposes across assembly boundaries.</span></span>

<span data-ttu-id="bb5c8-229">El último punto es especialmente problemático.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-229">The last point is particularly problematic.</span></span> <span data-ttu-id="bb5c8-230">Esto significa que cada ensamblado que usa `Func<int>*` debe codificar un tipo equivalente en los metadatos, aunque `Func<int>*` se haya definido en un ensamblado aunque no sea de control.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-230">This mean that every assembly which uses `Func<int>*` must encode an equivalent type in metadata even though `Func<int>*` is defined in an assembly though don't control.</span></span>
<span data-ttu-id="bb5c8-231">Además, cualquier otro tipo que se defina con el nombre `System.Func<T>` en un ensamblado que no sea mscorlib debe ser diferente de la versión definida en mscorlib.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-231">Additionally any other type which is defined with the name `System.Func<T>` in an assembly that is not mscorlib must be different than the version defined in mscorlib.</span></span>

<span data-ttu-id="bb5c8-232">Una opción que se exploró fue emitir este tipo de puntero como `mod_req(Func<int>) void*`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-232">One option that was explored was emitting such a pointer as `mod_req(Func<int>) void*`.</span></span> <span data-ttu-id="bb5c8-233">Esto no funciona aunque como `mod_req` no se puede enlazar a un `TypeSpec` y, por tanto, no puede tener como destino instancias genéricas.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-233">This doesn't work though as a `mod_req` cannot bind to a `TypeSpec` and hence cannot target generic instantiations.</span></span>

### <a name="named-function-pointers"></a><span data-ttu-id="bb5c8-234">Punteros de función con nombre</span><span class="sxs-lookup"><span data-stu-id="bb5c8-234">Named function pointers</span></span>

<span data-ttu-id="bb5c8-235">La sintaxis de puntero de función puede ser complicada, especialmente en casos complejos como punteros de función anidados.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-235">The function pointer syntax can be cumbersome, particularly in complex cases like nested function pointers.</span></span> <span data-ttu-id="bb5c8-236">En lugar de hacer que los desarrolladores escriban la firma cada vez que el lenguaje pueda permitir declaraciones con nombre de punteros de función, como se hace con `delegate`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-236">Rather than have developers type out the signature every time the language could allow for named declarations of function pointers as is done with `delegate`.</span></span>

``` csharp
func* void Action();

unsafe class NamedExample {
    void M(Action a) {
        a();
    }
}
```

<span data-ttu-id="bb5c8-237">Parte del problema aquí es que el primitivo subyacente de la CLI no tiene nombres, por lo tanto C# , esto sería puramente una invención y requería un poco de trabajo de metadatos para habilitarlo.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-237">Part of the problem here is the underlying CLI primitive doesn't have names hence this would be purely a C# invention and require a bit of metadata work to enable.</span></span> <span data-ttu-id="bb5c8-238">Es decir, factible, pero es una tarea importante sobre el trabajo.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-238">That is doable but is a significant about of work.</span></span> <span data-ttu-id="bb5c8-239">En esencia, C# es necesario tener un complemento a la tabla de tipo Def únicamente para estos nombres.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-239">It essentially requires C# to have a companion to the type def table purely for these names.</span></span>

<span data-ttu-id="bb5c8-240">Además, cuando se examinan los argumentos de los punteros de función con nombre, encontramos que podrían aplicarse igualmente bien a otros escenarios.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-240">Also when the arguments for named function pointers were examined we found they could apply equally well to a number of other scenarios.</span></span> <span data-ttu-id="bb5c8-241">Por ejemplo, sería tan práctico declarar tuplas con nombre para reducir la necesidad de escribir la firma completa en todos los casos.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-241">For example it would be just as convenient to declare named tuples to reduce the need to type out the full signature in all cases.</span></span>

``` csharp
(int x, int y) Point;

class NamedTupleExample {
    void M(Point p) {
        Console.WriteLine(p.x);
    }
}
```

<span data-ttu-id="bb5c8-242">Después de la explicación, decidimos no permitir la declaración con nombre de tipos de `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-242">After discussion we decided to not allow named declaration of `delegate*` types.</span></span> <span data-ttu-id="bb5c8-243">Si encontramos una necesidad importante para esto en función de los comentarios de uso de los clientes, investigaremos una solución de nomenclatura que funciona para punteros de función, tuplas, genéricos, etc. Es probable que esto sea similar en forma de otras sugerencias, como la compatibilidad completa de `typedef` en el lenguaje.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-243">If we find there is significant need for this based on customer usage feedback then we will investigate a naming solution that works for function pointers, tuples, generics, etc ... This is likely to be similar in form to other suggestions like full `typedef` support in the language.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="bb5c8-244">Consideraciones futuras</span><span class="sxs-lookup"><span data-stu-id="bb5c8-244">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="bb5c8-245">funciones locales estáticas</span><span class="sxs-lookup"><span data-stu-id="bb5c8-245">static local functions</span></span>

<span data-ttu-id="bb5c8-246">Esto hace referencia a [la propuesta](https://github.com/dotnet/csharplang/issues/1565) para permitir el modificador `static` en las funciones locales.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-246">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="bb5c8-247">Tal función se garantiza que se emitirá como `static` y con la firma exacta especificada en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-247">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="bb5c8-248">Este tipo de función debe ser un argumento válido para `&` porque no contiene ninguno de los problemas que las funciones locales tienen actualmente.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-248">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today</span></span>

### <a name="static-delegates"></a><span data-ttu-id="bb5c8-249">delegados estáticos</span><span class="sxs-lookup"><span data-stu-id="bb5c8-249">static delegates</span></span>

<span data-ttu-id="bb5c8-250">Esto hace referencia a [la propuesta](https://github.com/dotnet/csharplang/issues/302) para permitir la declaración de tipos de `delegate` que solo pueden hacer referencia a miembros de `static`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-250">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/302) to allow for the declaration of `delegate` types which can only refer to `static` members.</span></span> <span data-ttu-id="bb5c8-251">La ventaja es que estas instancias de `delegate` se pueden asignar de forma gratuita y mejor en escenarios sensibles al rendimiento.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-251">The advantage being that such `delegate` instances can be allocation free and better in performance sensitive scenarios.</span></span>

<span data-ttu-id="bb5c8-252">Si se implementa la característica de puntero de función, es probable que la propuesta de `static delegate` se cierre. La ventaja propuesta de esa característica es la naturaleza gratuita de la asignación.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-252">If the function pointer feature is implemented the `static delegate` proposal will likely be closed out. The proposed advantage of that feature is the allocation free nature.</span></span> <span data-ttu-id="bb5c8-253">Sin embargo, las investigaciones recientes han detectado que no es posible lograr debido a la descarga de ensamblados.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-253">However recent investigations have found that is not possible to achieve due to assembly unloading.</span></span> <span data-ttu-id="bb5c8-254">Debe haber un identificador seguro desde el `static delegate` al método al que hace referencia para evitar que el ensamblado se descargue fuera de él.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-254">There must be a strong handle from the `static delegate` to the method it refers to in order to keep the assembly from being unloaded out from under it.</span></span>

<span data-ttu-id="bb5c8-255">Para mantener cada `static delegate` instancia sería necesario asignar un nuevo identificador que ejecute el contador en los objetivos de la propuesta.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-255">To maintain every `static delegate` instance would be required to allocate a new handle which runs counter to the goals of the proposal.</span></span> <span data-ttu-id="bb5c8-256">Había algunos diseños en los que la asignación podía amortizarse en una única asignación por sitio de llamada, pero esto era un poco complejo y no parecía merecer la pena para el compromiso.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-256">There were some designs where the allocation could be amortized to a single allocation per call-site but that was a bit complex and didn't seem worth the trade off.</span></span>

<span data-ttu-id="bb5c8-257">Esto significa que los desarrolladores tienen que decidir esencialmente entre las siguientes ventajas e inconvenientes:</span><span class="sxs-lookup"><span data-stu-id="bb5c8-257">That means developers essentially have to decide between the following trade offs:</span></span>

1. <span data-ttu-id="bb5c8-258">Seguridad en el aspecto de la descarga de ensamblados: requiere asignaciones y, por tanto, `delegate` ya es una opción suficiente.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-258">Safety in the face of assembly unloading: this requires allocations and hence `delegate` is already a sufficient option.</span></span>
1. <span data-ttu-id="bb5c8-259">No hay seguridad en la descarga de ensamblados: Use un `delegate*`.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-259">No safety in face of assembly unloading: use a `delegate*`.</span></span> <span data-ttu-id="bb5c8-260">Esto se puede ajustar en un `struct` para permitir el uso fuera de un contexto `unsafe` en el resto del código.</span><span class="sxs-lookup"><span data-stu-id="bb5c8-260">This can be wrapped in a `struct` to allow usage outside an `unsafe` context in the rest of the code.</span></span>
