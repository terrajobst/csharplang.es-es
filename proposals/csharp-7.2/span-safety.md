---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "79483965"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a><span data-ttu-id="b9a4f-101">Aplicación del tiempo de compilación de seguridad para los tipos de referencia</span><span class="sxs-lookup"><span data-stu-id="b9a4f-101">Compile time enforcement of safety for ref-like types</span></span>

## <a name="introduction"></a><span data-ttu-id="b9a4f-102">Introducción</span><span class="sxs-lookup"><span data-stu-id="b9a4f-102">Introduction</span></span>

<span data-ttu-id="b9a4f-103">La razón principal de las reglas de seguridad adicionales cuando se trabaja con tipos como `Span<T>` y `ReadOnlySpan<T>` es que estos tipos se deben limitar a la pila de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-103">The main reason for the additional safety rules when dealing with types like `Span<T>` and `ReadOnlySpan<T>` is that such types must be confined to the execution stack.</span></span>
 
<span data-ttu-id="b9a4f-104">Hay dos razones por las que `Span<T>` y tipos similares deben ser tipos de solo pila.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-104">There are two reasons why `Span<T>` and similar types must be a stack-only types.</span></span>

1. <span data-ttu-id="b9a4f-105">`Span<T>` es semánticamente un struct que contiene una referencia y un intervalo `(ref T data, int length)`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-105">`Span<T>` is semantically a struct containing a reference and a range - `(ref T data, int length)`.</span></span> <span data-ttu-id="b9a4f-106">Independientemente de la implementación real, las escrituras en dicho struct no serían atómicas.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-106">Regardless of actual implementation, writes to such struct would not be atomic.</span></span> <span data-ttu-id="b9a4f-107">El "desgarro" simultáneo de este struct provocaría la posibilidad de `length` no coincidir con el `data`, lo que provocaría accesos fuera de intervalo e infracciones de seguridad de tipos, lo que en última instancia podría provocar daños en el montón GC en código aparentemente "seguro".</span><span class="sxs-lookup"><span data-stu-id="b9a4f-107">Concurrent "tearing" of such struct would lead to the possibility of `length` not matching the `data`, causing out-of-range accesses and type-safety violations, which ultimately could result in GC heap corruption in seemingly "safe" code.</span></span>
2. <span data-ttu-id="b9a4f-108">Algunas implementaciones de `Span<T>` contienen literalmente un puntero administrado en uno de sus campos.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-108">Some implementations of `Span<T>` literally contain a managed pointer in one of its fields.</span></span> <span data-ttu-id="b9a4f-109">Los punteros administrados no se admiten como campos de objetos de montón y el código que administra para colocar un puntero administrado en el montón GC normalmente se bloquea en tiempo JIT.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-109">Managed pointers are not supported as fields of heap objects and code that manages to put a managed pointer on the GC heap typically crashes at JIT time.</span></span>

<span data-ttu-id="b9a4f-110">Todos los problemas anteriores se solucionarían si las instancias de `Span<T>` están restringidas a existir solo en la pila de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-110">All the above problems would be alleviated if instances of `Span<T>` are constrained to exist only on the execution stack.</span></span> 

<span data-ttu-id="b9a4f-111">Se produce un problema adicional debido a la composición.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-111">An additional problem arises due to composition.</span></span> <span data-ttu-id="b9a4f-112">Por lo general, sería conveniente crear tipos de datos más complejos que incrustaran `Span<T>` y `ReadOnlySpan<T>` instancias.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-112">It would be generally desirable to build more complex data types that would embed `Span<T>` and `ReadOnlySpan<T>` instances.</span></span> <span data-ttu-id="b9a4f-113">Estos tipos compuestos deben ser Structs y compartir todos los peligros y los requisitos de `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-113">Such composite types would have to be structs and would share all the hazards and requirements of `Span<T>`.</span></span> <span data-ttu-id="b9a4f-114">Como resultado, las reglas de seguridad que se describen aquí deben verse como corresponda a toda la gama de **_tipos de referencia_** .</span><span class="sxs-lookup"><span data-stu-id="b9a4f-114">As a result the safety rules described here should be viewed as applicable to the whole range of **_ref-like types_**.</span></span>

<span data-ttu-id="b9a4f-115">La [especificación del lenguaje de borrador](#draft-language-specification) está pensada para garantizar que los valores de un tipo de referencia solo se producen en la pila.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-115">The [draft language specification](#draft-language-specification) is intended to ensure that values of a ref-like type occurs only on the stack.</span></span>

## <a name="generalized-ref-like-types-in-source-code"></a><span data-ttu-id="b9a4f-116">Tipos de `ref-like` generalizados en el código fuente</span><span class="sxs-lookup"><span data-stu-id="b9a4f-116">Generalized `ref-like` types in source code</span></span>

<span data-ttu-id="b9a4f-117">`ref-like` Structs se marcan explícitamente en el código fuente mediante el modificador `ref`:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-117">`ref-like` structs are explicitly marked in the source code using `ref` modifier:</span></span>

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

<span data-ttu-id="b9a4f-118">Designar un struct como referencia permite que el struct tenga campos de instancia de tipo REF y también hará que todos los requisitos de los tipos de referencia se apliquen a la estructura.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-118">Designating a struct as ref-like will allow the struct to have ref-like instance fields and will also make all the requirements of ref-like types applicable to the struct.</span></span> 

## <a name="metadata-representation-or-ref-like-structs"></a><span data-ttu-id="b9a4f-119">Representación de metadatos o Structs de tipo Ref</span><span class="sxs-lookup"><span data-stu-id="b9a4f-119">Metadata representation or ref-like structs</span></span>

<span data-ttu-id="b9a4f-120">Las estructuras de tipo REF se marcarán con el atributo **System. Runtime. CompilerServices. IsRefLikeAttribute** .</span><span class="sxs-lookup"><span data-stu-id="b9a4f-120">Ref-like structs will be marked with **System.Runtime.CompilerServices.IsRefLikeAttribute** attribute.</span></span>

<span data-ttu-id="b9a4f-121">El atributo se agregará a las bibliotecas base comunes, como `mscorlib`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-121">The attribute will be added to common base libraries such as `mscorlib`.</span></span> <span data-ttu-id="b9a4f-122">En caso de que el atributo no esté disponible, el compilador generará uno interno de forma similar a otros atributos incrustados a petición, como `IsReadOnlyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-122">In a case if the attribute is not available, compiler will generate an internal one similarly to other embedded-on-demand attributes such as `IsReadOnlyAttribute`.</span></span>

<span data-ttu-id="b9a4f-123">Se realizará una medida adicional para evitar el uso de Structs similares en los compiladores que no están familiarizados con las reglas de seguridad C# (esto incluye los compiladores anteriores a aquél en el que se implementa esta característica).</span><span class="sxs-lookup"><span data-stu-id="b9a4f-123">An additional measure will be taken to prevent the use of ref-like structs in compilers not familiar with the safety rules (this includes C# compilers prior to the one in which this feature is implemented).</span></span> 

<span data-ttu-id="b9a4f-124">Si no hay ninguna otra buena alternativa que funcione en los compiladores antiguos sin servicio, se agregará un `Obsolete` atributo con una cadena conocida a todos los Structs de tipo Ref.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-124">Having no other good alternatives that work in old compilers without servicing, an `Obsolete` attribute with a known string will be added to all ref-like structs.</span></span> <span data-ttu-id="b9a4f-125">Los compiladores que sepan usar tipos de tipo REF no tendrán en cuenta esta forma determinada de `Obsolete`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-125">Compilers that know how to use ref-like types will ignore this particular form of `Obsolete`.</span></span>

<span data-ttu-id="b9a4f-126">Una representación de metadatos típica:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-126">A typical metadata representation:</span></span>

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

<span data-ttu-id="b9a4f-127">Nota: no es el objetivo de hacerlo para que cualquier uso de tipos de tipo REF en los compiladores anteriores produzca un error 100%.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-127">NOTE: it is not the goal to make it so that any use of ref-like types on old compilers fails 100%.</span></span> <span data-ttu-id="b9a4f-128">Esto es difícil de lograr y no es estrictamente necesario.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-128">That is hard to achieve and is not strictly necessary.</span></span> <span data-ttu-id="b9a4f-129">Por ejemplo, siempre sería una manera de evitar el `Obsolete` mediante código dinámico o, por ejemplo, crear una matriz de tipos de tipo REF a través de la reflexión.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-129">For example there would always be a way to get around the `Obsolete` using dynamic code or, for example, creating an array of ref-like types through reflection.</span></span>

<span data-ttu-id="b9a4f-130">En concreto, si el usuario desea colocar realmente un `Obsolete` o `Deprecated` atributo en un tipo de referencia, no tendrá ninguna opción aparte de no emitir el predefinido, ya que `Obsolete` atributo no se puede aplicar más de una vez.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-130">In particular, if user wants to actually put an `Obsolete` or `Deprecated` attribute on a ref-like type, we will have no choice other than not emitting the predefined one since `Obsolete` attribute cannot be applied more than once..</span></span>  

## <a name="examples"></a><span data-ttu-id="b9a4f-131">Ejemplos:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-131">Examples:</span></span>

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a><span data-ttu-id="b9a4f-132">Especificación del lenguaje borrador</span><span class="sxs-lookup"><span data-stu-id="b9a4f-132">Draft language specification</span></span>

<span data-ttu-id="b9a4f-133">A continuación se describe un conjunto de reglas de seguridad para los tipos de referencia (`ref struct`s) para asegurarse de que los valores de estos tipos solo se producen en la pila.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-133">Below we describe a set of safety rules for ref-like types (`ref struct`s) to ensure that values of these types occur only on the stack.</span></span> <span data-ttu-id="b9a4f-134">Sería posible un conjunto de reglas de seguridad diferente y más sencillo si las variables locales no se pueden pasar por referencia.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-134">A different, simpler set of safety rules would be possible if locals cannot be passed by reference.</span></span> <span data-ttu-id="b9a4f-135">Esta especificación también permitiría la reasignación segura de variables locales de referencia.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-135">This specification would also permit the safe reassignment of ref locals.</span></span>

### <a name="overview"></a><span data-ttu-id="b9a4f-136">Información general</span><span class="sxs-lookup"><span data-stu-id="b9a4f-136">Overview</span></span>

<span data-ttu-id="b9a4f-137">Asociamos con cada expresión en tiempo de compilación el concepto del ámbito al que se permite el escape de la expresión, "Safe-to-escape".</span><span class="sxs-lookup"><span data-stu-id="b9a4f-137">We associate with each expression at compile-time the concept of what scope that expression is permitted to escape to, "safe-to-escape".</span></span> <span data-ttu-id="b9a4f-138">Del mismo modo, para cada valor l, se mantiene un concepto del ámbito al que se permite que se escape una referencia, "ref-Safe-to-escape".</span><span class="sxs-lookup"><span data-stu-id="b9a4f-138">Similarly, for each lvalue we maintain a concept of what scope a reference to it is permitted to escape to, "ref-safe-to-escape".</span></span> <span data-ttu-id="b9a4f-139">Para una expresión lvalue determinada, pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-139">For a given lvalue expression, these may be different.</span></span>

<span data-ttu-id="b9a4f-140">Son análogos a "Safe to Return" de la característica Ref locals, pero es más específica.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-140">These are analogous to the "safe to return" of the ref locals feature, but it is more fine-grained.</span></span> <span data-ttu-id="b9a4f-141">Cuando el valor de "Safe-to-Return" de una expresión solo registra si (o no) puede escapar el método envolvente en su conjunto, los registros de seguridad a escape a los que puede escapar (el ámbito en el que podría no escapar).</span><span class="sxs-lookup"><span data-stu-id="b9a4f-141">Where the "safe-to-return" of an expression records only whether (or not) it may escape the enclosing method as a whole, the safe-to-escape records which scope it may escape to (which scope it may not escape beyond).</span></span> <span data-ttu-id="b9a4f-142">El mecanismo de seguridad básica se aplica como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-142">The basic safety mechanism is enforced as follows.</span></span> <span data-ttu-id="b9a4f-143">Dada una asignación desde una expresión E1 con un ámbito de salida a escape S1, a una expresión (lvalue) E2 con un ámbito de seguridad a escape S2, es un error si S2 es un ámbito más amplio que S1.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-143">Given an assignment from an expression E1 with a safe-to-escape scope S1, to an (lvalue) expression E2 with safe-to-escape scope S2, it is an error if S2 is a wider scope than S1.</span></span> <span data-ttu-id="b9a4f-144">Por construcción, los dos ámbitos S1 y S2 están en una relación de anidamiento, ya que una expresión válida siempre es segura para devolver desde algún ámbito que incluya la expresión.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-144">By construction, the two scopes S1 and S2 are in a nesting relationship, because a legal expression is always safe-to-return from some scope enclosing the expression.</span></span>

<span data-ttu-id="b9a4f-145">En el momento en que es suficiente, para el análisis, admitir solo dos ámbitos: externo al método y el ámbito de nivel superior del método.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-145">For the time being it is sufficient, for the purpose of the analysis, to support just two scopes - external to the method, and top-level scope of the method.</span></span> <span data-ttu-id="b9a4f-146">Esto se debe a que no se pueden crear valores de tipo REF con ámbitos internos y las variables locales de referencia no admiten la reasignación.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-146">That is because ref-like values with inner scopes cannot be created and ref locals do not support re-assignment.</span></span> <span data-ttu-id="b9a4f-147">Sin embargo, las reglas pueden admitir más de dos niveles de ámbito.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-147">The rules, however, can support more than two scope levels.</span></span>

<span data-ttu-id="b9a4f-148">A continuación se indican las reglas precisas para calcular el estado de seguridad de la *devolución* de una expresión y las reglas que rigen la legalidad de las expresiones.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-148">The precise rules for computing the *safe-to-return* status of an expression, and the rules governing the legality of expressions, follow.</span></span>

### <a name="ref-safe-to-escape"></a><span data-ttu-id="b9a4f-149">Ref-Safe-to-escape</span><span class="sxs-lookup"><span data-stu-id="b9a4f-149">ref-safe-to-escape</span></span>

<span data-ttu-id="b9a4f-150">*Ref-Safe-to-escape* es un ámbito, que incluye una expresión lvalue, a la que es seguro para una referencia al valor l a la que se debe escapar.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-150">The *ref-safe-to-escape* is a scope, enclosing an lvalue expression, to which it is safe for a ref to the lvalue to escape to.</span></span> <span data-ttu-id="b9a4f-151">Si ese ámbito es el método completo, se indica que una referencia a lvalue es *segura para devolver* desde el método.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-151">If that scope is the entire method, we say that a ref to the lvalue is *safe to return* from the method.</span></span>

### <a name="safe-to-escape"></a><span data-ttu-id="b9a4f-152">Safe-to-escape</span><span class="sxs-lookup"><span data-stu-id="b9a4f-152">safe-to-escape</span></span>

<span data-ttu-id="b9a4f-153">*Safe-to-escape* es un ámbito, que incluye una expresión, a la que es seguro que el valor se escape.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-153">The *safe-to-escape* is a scope, enclosing an expression, to which it is safe for the value to escape to.</span></span> <span data-ttu-id="b9a4f-154">Si ese ámbito es el método completo, se indica que un valor es *seguro para volver* del método.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-154">If that scope is the entire method, we say that a the value is *safe to return* from the method.</span></span>

<span data-ttu-id="b9a4f-155">Una expresión cuyo tipo no es un tipo `ref struct` es *seguro para devolver* desde todo el método envolvente.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-155">An expression whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span> <span data-ttu-id="b9a4f-156">En caso contrario, hacemos referencia a las reglas siguientes.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-156">Otherwise we refer to the rules below.</span></span>

#### <a name="parameters"></a><span data-ttu-id="b9a4f-157">Parámetros</span><span class="sxs-lookup"><span data-stu-id="b9a4f-157">Parameters</span></span>

<span data-ttu-id="b9a4f-158">Un valor l que designa un parámetro formal es *ref-Safe-to-escape* (por referencia) de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-158">An lvalue designating a formal parameter is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="b9a4f-159">Si el parámetro es un parámetro `ref`, `out`o `in`, es *ref-Safe-to-escape* del método completo (por ejemplo, mediante una instrucción `return ref`); casos</span><span class="sxs-lookup"><span data-stu-id="b9a4f-159">If the parameter is a `ref`, `out`, or `in` parameter, it is *ref-safe-to-escape* from the entire method (e.g. by a `return ref` statement); otherwise</span></span>
- <span data-ttu-id="b9a4f-160">Si el parámetro es el parámetro `this` de un tipo de estructura, es *ref-Safe-to-escape* en el ámbito de nivel superior del método (pero no en todo el método); [Ejemplo](#struct-this-escape) de</span><span class="sxs-lookup"><span data-stu-id="b9a4f-160">If the parameter is the `this` parameter of a struct type, it is *ref-safe-to-escape* to the top-level scope of the method (but not from the entire method itself); [Sample](#struct-this-escape)</span></span>
- <span data-ttu-id="b9a4f-161">De lo contrario, el parámetro es un parámetro de valor y es *ref-Safe-to-escape* al ámbito de nivel superior del método (pero no al propio método).</span><span class="sxs-lookup"><span data-stu-id="b9a4f-161">Otherwise the parameter is a value parameter, and it is *ref-safe-to-escape* to the top-level scope of the method (but not from the method itself).</span></span>

<span data-ttu-id="b9a4f-162">Una expresión que es un valor r que designa el uso de un parámetro formal es *Safe-to-escape* (por valor) desde el método completo (por ejemplo, mediante una instrucción `return`).</span><span class="sxs-lookup"><span data-stu-id="b9a4f-162">An expression that is an rvalue designating the use of a formal parameter is *safe-to-escape* (by value) from the entire method (e.g. by a `return` statement).</span></span> <span data-ttu-id="b9a4f-163">Esto se aplica también al parámetro `this`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-163">This applies to the `this` parameter as well.</span></span>

#### <a name="locals"></a><span data-ttu-id="b9a4f-164">Locals</span><span class="sxs-lookup"><span data-stu-id="b9a4f-164">Locals</span></span>

<span data-ttu-id="b9a4f-165">Un valor l que designa una variable local es *ref-Safe-to-escape* (por referencia) de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-165">An lvalue designating a local variable is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="b9a4f-166">Si la variable es una variable de `ref`, su *ref-Safe-to-escape* se toma de la *referencia-Safe-to-* escape de la expresión de inicialización. casos</span><span class="sxs-lookup"><span data-stu-id="b9a4f-166">If the variable is a `ref` variable, then its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of its initializing expression; otherwise</span></span>
- <span data-ttu-id="b9a4f-167">La variable es *ref-Safe-to-escape* el ámbito en el que se declaró.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-167">The variable is *ref-safe-to-escape* the scope in which it was declared.</span></span>

<span data-ttu-id="b9a4f-168">Una expresión que es un valor r que designa el uso de una variable local es *segura a escape* (por valor) de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-168">An expression that is an rvalue designating the use of a local variable is *safe-to-escape* (by value) as follows:</span></span>
- <span data-ttu-id="b9a4f-169">Pero la regla general anterior, una variable local cuyo tipo no es un tipo `ref struct` es *seguro para devolver* desde todo el método envolvente.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-169">But the general rule above, a local whose type is not a `ref struct` type is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="b9a4f-170">Si la variable es una variable de iteración de un bucle `foreach`, el ámbito de *seguridad* de la variable de la variable es el mismo que *el de la* expresión del bucle `foreach`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-170">If the variable is an iteration variable of a `foreach` loop, then the variable's *safe-to-escape* scope is the same as the *safe-to-escape* of the `foreach` loop's expression.</span></span>
- <span data-ttu-id="b9a4f-171">Una variable local de `ref struct` tipo y no inicializado en el punto de declaración es *segura para devolver* desde todo el método de inclusión.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-171">A local of `ref struct` type and uninitialized at the point of declaration is *safe-to-return* from the entire enclosing method.</span></span>
- <span data-ttu-id="b9a4f-172">De lo contrario, el tipo de la variable es un tipo de `ref struct`, y la declaración de la variable requiere un inicializador.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-172">Otherwise the variable's type is a `ref struct` type, and the variable's declaration requires an initializer.</span></span> <span data-ttu-id="b9a4f-173">El ámbito de *seguridad a escape* de la variable es el mismo *que el del* inicializador.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-173">The variable's *safe-to-escape* scope is the same as the *safe-to-escape* of its initializer.</span></span>

#### <a name="field-reference"></a><span data-ttu-id="b9a4f-174">Referencia de campo</span><span class="sxs-lookup"><span data-stu-id="b9a4f-174">Field reference</span></span>

<span data-ttu-id="b9a4f-175">Un valor l que designa una referencia a un campo, `e.F`, es *ref-Safe-to-escape* (por referencia) de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-175">An lvalue designating a reference to a field, `e.F`, is *ref-safe-to-escape* (by reference) as follows:</span></span>
- <span data-ttu-id="b9a4f-176">Si `e` es de un tipo de referencia, es *ref-Safe-to-escape* del método completo; casos</span><span class="sxs-lookup"><span data-stu-id="b9a4f-176">If `e` is of a reference type, it is *ref-safe-to-escape* from the entire method; otherwise</span></span>
- <span data-ttu-id="b9a4f-177">Si `e` es de un tipo de valor, su *ref-Safe-to-escape* se toma del *parámetro ref-Safe-to-escape* de `e`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-177">If `e` is of a value type, its *ref-safe-to-escape* is taken from the *ref-safe-to-escape* of `e`.</span></span>

<span data-ttu-id="b9a4f-178">Un valor r que designe una referencia a un campo, `e.F`, tiene un ámbito de *seguridad a escape* que es el mismo que el de la `e`de *seguridad* .</span><span class="sxs-lookup"><span data-stu-id="b9a4f-178">An rvalue designating a reference to a field, `e.F`, has a *safe-to-escape* scope that is the same as the *safe-to-escape* of `e`.</span></span>

#### <a name="operators-including-"></a><span data-ttu-id="b9a4f-179">Operadores que incluyen `?:`</span><span class="sxs-lookup"><span data-stu-id="b9a4f-179">Operators including `?:`</span></span>

<span data-ttu-id="b9a4f-180">La aplicación de un operador definido por el usuario se trata como una invocación de método.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-180">The application of a user-defined operator is treated as a method invocation.</span></span>

<span data-ttu-id="b9a4f-181">Para un operador que produce un valor r, como `e1 + e2` o `c ? e1 : e2`, el *valor de Safe-to-escape* del resultado es el ámbito más estrecho entre el *valor de Safe-to-escape* de los operandos del operador.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-181">For an operator that yields an rvalue, such as `e1 + e2` or `c ? e1 : e2`, the *safe-to-escape* of the result is the narrowest scope among the *safe-to-escape* of the operands of the operator.</span></span>  <span data-ttu-id="b9a4f-182">Como consecuencia, para un operador unario que produce un valor r, como `+e`, el valor de *Safe-to-escape* del resultado es el carácter de *escape seguro* del operando.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-182">As a consequence, for a unary operator that yields an rvalue, such as `+e`, the *safe-to-escape* of the result is the *safe-to-escape* of the operand.</span></span>

<span data-ttu-id="b9a4f-183">Para un operador que produce un valor l, como `c ? ref e1 : ref e2`</span><span class="sxs-lookup"><span data-stu-id="b9a4f-183">For an operator that yields an lvalue, such as `c ? ref e1 : ref e2`</span></span>
- <span data-ttu-id="b9a4f-184">*ref-Safe-to-escape* del resultado es el ámbito más restringido entre *ref-Safe-to-escape* de los operandos del operador.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-184">the *ref-safe-to-escape* of the result is the narrowest scope among the *ref-safe-to-escape* of the operands of the operator.</span></span>
- <span data-ttu-id="b9a4f-185">el valor de *Safe-to-escape* de los operandos debe coincidir, y es el *valor de Safe-to-escape* del valor l resultante.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-185">the *safe-to-escape* of the operands must agree, and that is the *safe-to-escape* of the resulting lvalue.</span></span>

#### <a name="method-invocation"></a><span data-ttu-id="b9a4f-186">Invocación de método</span><span class="sxs-lookup"><span data-stu-id="b9a4f-186">Method invocation</span></span>

<span data-ttu-id="b9a4f-187">Un valor l resultante de una invocación de método de devolución de referencia `e1.M(e2, ...)` es *ref-Safe-to-escape* el menor de los siguientes ámbitos:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-187">An lvalue resulting from a ref-returning method invocation `e1.M(e2, ...)` is *ref-safe-to-escape* the smallest of the following scopes:</span></span>
- <span data-ttu-id="b9a4f-188">Todo el método envolvente</span><span class="sxs-lookup"><span data-stu-id="b9a4f-188">The entire enclosing method</span></span>
- <span data-ttu-id="b9a4f-189">*ref-Safe-to-escape* de todas las expresiones de argumentos `ref` y `out` (sin incluir el receptor)</span><span class="sxs-lookup"><span data-stu-id="b9a4f-189">the *ref-safe-to-escape* of all `ref` and `out` argument expressions (excluding the receiver)</span></span>
- <span data-ttu-id="b9a4f-190">Para cada `in` parámetro del método, si hay una expresión correspondiente que es un valor l, su *referencia-Safe-to-escape*, de lo contrario, el ámbito de inclusión más próximo</span><span class="sxs-lookup"><span data-stu-id="b9a4f-190">For each `in` parameter of the method, if there is a corresponding expression that is an lvalue, its *ref-safe-to-escape*, otherwise the nearest enclosing scope</span></span>
- <span data-ttu-id="b9a4f-191">el carácter de *escape seguro* de todas las expresiones de argumentos (incluido el receptor)</span><span class="sxs-lookup"><span data-stu-id="b9a4f-191">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

> <span data-ttu-id="b9a4f-192">Nota: la última viñeta es necesaria para controlar el código como</span><span class="sxs-lookup"><span data-stu-id="b9a4f-192">Note: the last bullet is necessary to handle code such as</span></span>
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> <span data-ttu-id="b9a4f-193">or</span><span class="sxs-lookup"><span data-stu-id="b9a4f-193">or</span></span>
> ```csharp
> return ref M(sp, 0);
> ```

<span data-ttu-id="b9a4f-194">Un valor r que resulta de una invocación de método `e1.M(e2, ...)` es *seguro para el escape* de la menor de los siguientes ámbitos:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-194">An rvalue resulting from a method invocation `e1.M(e2, ...)` is *safe-to-escape* from the smallest of the following scopes:</span></span>
- <span data-ttu-id="b9a4f-195">Todo el método envolvente</span><span class="sxs-lookup"><span data-stu-id="b9a4f-195">The entire enclosing method</span></span>
- <span data-ttu-id="b9a4f-196">el carácter de *escape seguro* de todas las expresiones de argumentos (incluido el receptor)</span><span class="sxs-lookup"><span data-stu-id="b9a4f-196">the *safe-to-escape* of all argument expressions (including the receiver)</span></span>

#### <a name="an-rvalue"></a><span data-ttu-id="b9a4f-197">Un valor r</span><span class="sxs-lookup"><span data-stu-id="b9a4f-197">An Rvalue</span></span>
<span data-ttu-id="b9a4f-198">Un valor r es *ref-Safe-to-escape* del ámbito de inclusión más cercano.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-198">An rvalue is *ref-safe-to-escape* from the nearest enclosing scope.</span></span> <span data-ttu-id="b9a4f-199">Esto sucede, por ejemplo, en una invocación como `M(ref d.Length)` donde `d` es de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-199">This occurs for example in an invocation such as `M(ref d.Length)` where `d` is of type `dynamic`.</span></span> <span data-ttu-id="b9a4f-200">También es coherente con (y quizás subsumes) nuestro control de los argumentos correspondientes a los parámetros de `in`. \*</span><span class="sxs-lookup"><span data-stu-id="b9a4f-200">It is also consistent with (and perhaps subsumes) our handling of arguments corresponding to `in` parameters.\*</span></span>

#### <a name="property-invocations"></a><span data-ttu-id="b9a4f-201">Invocaciones de propiedad</span><span class="sxs-lookup"><span data-stu-id="b9a4f-201">Property invocations</span></span>

<span data-ttu-id="b9a4f-202">Una invocación de propiedad (ya sea `get` o `set`) se trata como una invocación de método del método subyacente por parte de las reglas anteriores.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-202">A property invocation (either `get` or `set`) it treated as a method invocation of the underlying method by the above rules.</span></span>

#### `stackalloc`

<span data-ttu-id="b9a4f-203">Una expresión stackalloc es un valor r que es *seguro* para el ámbito de nivel superior del método, pero no del propio método completo.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-203">A stackalloc expression is an rvalue that is *safe-to-escape* to the top-level scope of the method (but not from the entire method itself).</span></span>

#### <a name="constructor-invocations"></a><span data-ttu-id="b9a4f-204">Invocaciones del constructor</span><span class="sxs-lookup"><span data-stu-id="b9a4f-204">Constructor invocations</span></span>

<span data-ttu-id="b9a4f-205">Una expresión `new` que invoca un constructor obedece a las mismas reglas que una invocación de método que se considera que devuelve el tipo que se está construyendo.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-205">A `new` expression that invokes a constructor obeys the same rules as a method invocation that is considered to return the type being constructed.</span></span>

<span data-ttu-id="b9a4f-206">Además, el valor de *Safe-to-escape* no es más ancho que el más pequeño *de los argumentos* y operandos de la expresión de inicializador de objeto, de forma recursiva, si el inicializador está presente.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-206">In addition *safe-to-escape* is no wider than the smallest of the *safe-to-escape* of all arguments/operands of the object initializer expressions, recursively, if initializer is present.</span></span> 

#### <a name="span-constructor"></a><span data-ttu-id="b9a4f-207">Span (constructor)</span><span class="sxs-lookup"><span data-stu-id="b9a4f-207">Span constructor</span></span>
<span data-ttu-id="b9a4f-208">El lenguaje se basa en `Span<T>` no tener un constructor con el formato siguiente:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-208">The language relies on `Span<T>` not having a constructor of the following form:</span></span>

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

<span data-ttu-id="b9a4f-209">Este tipo de constructor hace `Span<T>` que se usan como campos indistinguibles de un campo `ref`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-209">Such a constructor makes `Span<T>` which are used as fields indistinguishable from a `ref` field.</span></span> <span data-ttu-id="b9a4f-210">Las reglas de seguridad descritas en este documento dependen de `ref` campos que no sean una C#construcción válida en, o .net.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-210">The safety rules described in this document depend on `ref` fields not being a valid construct in C#, or .NET.</span></span>

#### <a name="default-expressions"></a><span data-ttu-id="b9a4f-211">Expresiones `default`</span><span class="sxs-lookup"><span data-stu-id="b9a4f-211">`default` expressions</span></span>

<span data-ttu-id="b9a4f-212">Una expresión `default` es *segura a escape* desde el método de inclusión completo.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-212">A `default` expression is *safe-to-escape* from the entire enclosing method.</span></span>

## <a name="language-constraints"></a><span data-ttu-id="b9a4f-213">Restricciones de lenguaje</span><span class="sxs-lookup"><span data-stu-id="b9a4f-213">Language Constraints</span></span>

<span data-ttu-id="b9a4f-214">Queremos asegurarnos de que no haya ningún `ref` variable local y que no haya ninguna variable de `ref struct` tipo, que haga referencia a la memoria de la pila o a las variables que ya no estén activas.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-214">We wish to ensure that no `ref` local variable, and no variable of `ref struct` type, refers to stack memory or variables that are no longer alive.</span></span> <span data-ttu-id="b9a4f-215">Por lo tanto, tenemos las siguientes restricciones de lenguaje:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-215">We therefore have the following language constraints:</span></span>

- <span data-ttu-id="b9a4f-216">No se puede elevar un parámetro ref ni una variable local de tipo REF ni un parámetro o una variable local de un tipo `ref struct` en una función lambda o local.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-216">Neither a ref parameter, nor a ref local, nor a parameter or local of a `ref struct` type can be lifted into a lambda or local function.</span></span>

- <span data-ttu-id="b9a4f-217">Ni un parámetro ref ni un parámetro de un tipo `ref struct` pueden ser un argumento en un método iterador o un método `async`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-217">Neither a ref parameter nor a parameter of a `ref struct` type may be an argument on an iterator method or an `async` method.</span></span>

- <span data-ttu-id="b9a4f-218">Ni una variable local de tipo REF ni una variable local de un tipo `ref struct` pueden estar en el ámbito en el punto de una instrucción `yield return` o una expresión `await`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-218">Neither a ref local, nor a local of a `ref struct` type may be in scope at the point of a `yield return` statement or an `await` expression.</span></span>

- <span data-ttu-id="b9a4f-219">Un tipo de `ref struct` no se puede usar como un argumento de tipo o como un tipo de elemento en un tipo de tupla.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-219">A `ref struct` type may not be used as a type argument, or as an element type in a tuple type.</span></span>

- <span data-ttu-id="b9a4f-220">Un tipo de `ref struct` no puede ser el tipo declarado de un campo, salvo que puede ser el tipo declarado de un campo de instancia de otro `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-220">A `ref struct` type may not be the declared type of a field, except that it may be the declared type of an instance field of another `ref struct`.</span></span>

- <span data-ttu-id="b9a4f-221">Un tipo de `ref struct` no puede ser el tipo de elemento de una matriz.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-221">A `ref struct` type may not be the element type of an array.</span></span>

- <span data-ttu-id="b9a4f-222">No se puede aplicar la conversión boxing a un valor de un tipo `ref struct`:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-222">A value of a `ref struct` type may not be boxed:</span></span>
  - <span data-ttu-id="b9a4f-223">No hay ninguna conversión de un tipo de `ref struct` al tipo `object` o `System.ValueType`de tipo.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-223">There is no conversion from a `ref struct` type to the type `object` or the type `System.ValueType`.</span></span>
  - <span data-ttu-id="b9a4f-224">Un tipo de `ref struct` no se puede declarar para implementar ninguna interfaz</span><span class="sxs-lookup"><span data-stu-id="b9a4f-224">A `ref struct` type may not be declared to implement any interface</span></span>
  - <span data-ttu-id="b9a4f-225">No se puede llamar a ningún método de instancia declarado en `object` o en `System.ValueType` pero que no se invalide en un tipo de `ref struct` con un receptor de ese tipo de `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-225">No instance method declared in `object` or in `System.ValueType` but not overridden in a `ref struct` type may be called with a receiver of that `ref struct` type.</span></span>
  - <span data-ttu-id="b9a4f-226">No se puede capturar ningún método de instancia de un tipo de `ref struct` mediante la conversión de método a un tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-226">No instance method of a `ref struct` type may be captured by method conversion to a delegate type.</span></span>

- <span data-ttu-id="b9a4f-227">En el caso de una `ref e1 = ref e2`de reasignación de referencia, el *parámetro ref-Safe-to-escape* de `e2` debe ser al menos tan ancho como el de la *referencia-Safe-to-escape* de `e1`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-227">For a ref reassignment `ref e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="b9a4f-228">En el caso de una instrucción Ref Return `return ref e1`, el valor *ref-Safe-to-escape* de `e1` debe ser *ref-Safe-to-escape* desde el método completo.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-228">For a ref return statement `return ref e1`, the *ref-safe-to-escape* of `e1` must be *ref-safe-to-escape* from the entire method.</span></span> <span data-ttu-id="b9a4f-229">(TODO: ¿también necesitamos una regla que `e1` deba ser *segura para el escape* desde el método completo, o que sea redundante?)</span><span class="sxs-lookup"><span data-stu-id="b9a4f-229">(TODO: Do we also need a rule that `e1` must be *safe-to-escape* from the entire method, or is that redundant?)</span></span>

- <span data-ttu-id="b9a4f-230">En el caso de una instrucción return `return e1`, el valor de *Safe-to-escape* de `e1` debe ser *seguro para* el método completo.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-230">For a return statement `return e1`, the *safe-to-escape* of `e1` must be *safe-to-escape* from the entire method.</span></span>

- <span data-ttu-id="b9a4f-231">En el caso de una `e1 = e2`de asignación, si el tipo de `e1` es un tipo de `ref struct`, el nivel de *escape seguro* de `e2` debe ser al menos tan ancho como un ámbito de *seguridad* de la `e1`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-231">For an assignment `e1 = e2`, if the type of `e1` is a `ref struct` type, then the *safe-to-escape* of `e2` must be at least as wide a scope as the *safe-to-escape* of `e1`.</span></span>

- <span data-ttu-id="b9a4f-232">En el caso de una invocación de método si hay un `ref` o `out` argumento de un tipo `ref struct` (incluido el receptor), con *Safe-to-escape* E1, ningún argumento (incluido el receptor) puede tener un método *Safe-to-escape* más estrecho que E1.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-232">For a method invocation if there is a `ref` or `out` argument of a `ref struct` type (including the receiver), with *safe-to-escape* E1, then no argument (including the receiver) may have a narrower *safe-to-escape* than E1.</span></span> [<span data-ttu-id="b9a4f-233">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b9a4f-233">Sample</span></span>](#method-arguments-must-match)

- <span data-ttu-id="b9a4f-234">Es posible que una función local o anónima no haga referencia a una variable local o a un parámetro de `ref struct` tipo declarado en un ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-234">A local function or anonymous function may not refer to a local or parameter of `ref struct` type declared in an enclosing scope.</span></span>

> <span data-ttu-id="b9a4f-235">***Problema abierto:*** Se necesita una regla que nos permita generar un error cuando se necesita sobrepasar un valor de la pila de un `ref struct` tipo en una expresión Await, por ejemplo en el código.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-235">***Open Issue:*** We need some rule that permits us to produce an error when needing to spill a stack value of a `ref struct` type at an await expression, for example in the code</span></span>
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a><span data-ttu-id="b9a4f-236">Explicaciones</span><span class="sxs-lookup"><span data-stu-id="b9a4f-236">Explanations</span></span>
<span data-ttu-id="b9a4f-237">Estas explicaciones y ejemplos ayudan a explicar por qué muchas de las reglas de seguridad anteriores existen.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-237">These explanations and samples help explain why many of the safety rules above exist</span></span>

### <a name="method-arguments-must-match"></a><span data-ttu-id="b9a4f-238">Los argumentos de método deben coincidir</span><span class="sxs-lookup"><span data-stu-id="b9a4f-238">Method Arguments Must Match</span></span>
<span data-ttu-id="b9a4f-239">Al invocar un método en el que hay un `out`, `ref` parámetro que es un `ref struct` incluido el receptor y, a continuación, todos los `ref struct` deben tener la misma duración.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-239">When invoking a method where there is an `out`, `ref` parameter that is a `ref struct` including the receiver then all of the `ref struct` need to have the same lifetime.</span></span> <span data-ttu-id="b9a4f-240">Esto es necesario porque C# debe tomar todas sus decisiones sobre la seguridad de la duración en función de la información disponible en la firma del método y la duración de los valores en el sitio de la llamada.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-240">This is necessary because C# must make all of it's decisions around lifetime safety based on the information available in the signature of the method and the lifetime of the values at the call site.</span></span> 

<span data-ttu-id="b9a4f-241">Cuando hay `ref` parámetros que se `ref struct` entonces hay posibilidad que podrían cambiar en torno a su contenido.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-241">When there are `ref` parameters that are `ref struct` then there is the possiblity they could swap around their contents.</span></span> <span data-ttu-id="b9a4f-242">Por lo tanto, en el sitio de llamada, debemos asegurarnos de que todos estos swaps **potenciales** sean compatibles.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-242">Hence at the call site we must ensure all of these **potential** swaps are compatible.</span></span> <span data-ttu-id="b9a4f-243">Si el lenguaje no aplicó, se permitirá un código incorrecto como el siguiente.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-243">If the language didn't enforce that then it will allow for bad code like the following.</span></span>

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

<span data-ttu-id="b9a4f-244">La restricción en el receptor es necesaria porque aunque ninguno de sus contenidos es Ref-Safe-to-escape, puede almacenar los valores proporcionados.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-244">The restriction on the receiver is necessary because while none of its contents are ref-safe-to-escape it can store the provided values.</span></span> <span data-ttu-id="b9a4f-245">Esto significa que las duraciones no coinciden, por lo que podría crear una carencia de seguridad de tipos de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-245">This means with mismatched lifetimes you could create a type safety hole in the following way:</span></span>

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a><span data-ttu-id="b9a4f-246">Struct este escape</span><span class="sxs-lookup"><span data-stu-id="b9a4f-246">Struct This Escape</span></span>
<span data-ttu-id="b9a4f-247">Cuando llega a abarcar las reglas de seguridad, el valor `this` de un miembro de instancia se modela como un parámetro para el miembro.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-247">When it comes to span safety rules the `this` value in an instance member is modeled as a parameter to the member.</span></span> <span data-ttu-id="b9a4f-248">Ahora, para un `struct` el tipo de `this` realmente `ref S` en el que en una `class` simplemente es `S` (para los miembros de una `class / struct` denominada S).</span><span class="sxs-lookup"><span data-stu-id="b9a4f-248">Now for a `struct` the type of `this` is actually `ref S` where in a `class` it's simply `S` (for members of a `class / struct` named S).</span></span> 

<span data-ttu-id="b9a4f-249">Todavía `this` tiene diferentes reglas de escape que otros parámetros de `ref`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-249">Yet `this` has different escaping rules than other `ref` parameters.</span></span> <span data-ttu-id="b9a4f-250">En concreto, no es un carácter de escape de referencia mientras que otros parámetros son:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-250">Specifically it is not ref-safe-to-escape while other parameters are:</span></span>

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

<span data-ttu-id="b9a4f-251">La razón de esta restricción realmente tiene poco que hacer con `struct` invocación de miembros.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-251">The reason for this restriction actually has little to do with `struct` member invocation.</span></span> <span data-ttu-id="b9a4f-252">Hay algunas reglas que deben realizarse con respecto a la invocación de miembros en `struct` miembros en los que el receptor es un valor r.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-252">There are some rules that need to be worked out with respect to member invocation on `struct` members where the receiver is an rvalue.</span></span> <span data-ttu-id="b9a4f-253">Pero es muy accesible.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-253">But that is very approachable.</span></span> 

<span data-ttu-id="b9a4f-254">La razón de esta restricción es realmente la invocación de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-254">The reason for this restriction is actually about interface invocation.</span></span> <span data-ttu-id="b9a4f-255">En concreto, se refiere a si el siguiente ejemplo debe o no debe compilarse;</span><span class="sxs-lookup"><span data-stu-id="b9a4f-255">Specifically it comes down to whether or not the following sample should or should not compile;</span></span>

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

<span data-ttu-id="b9a4f-256">Considere el caso en el que se crea una instancia de `T` como un `struct`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-256">Consider the case where `T` is instantiated as a `struct`.</span></span> <span data-ttu-id="b9a4f-257">Si el parámetro `this` es Ref-Safe-to-escape, la devolución de `p.Get` podría apuntar a la pila (en concreto, podría ser un campo dentro del tipo de instancia de `T`).</span><span class="sxs-lookup"><span data-stu-id="b9a4f-257">If the `this` parameter is ref-safe-to-escape then the return of `p.Get` could point to the stack (specifically it could be a field inside of the instantiated type of `T`).</span></span> <span data-ttu-id="b9a4f-258">Esto significa que el lenguaje no puede permitir que se compile este ejemplo, ya que podría devolver una `ref` a una ubicación de la pila.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-258">That means the language could not allow this sample to compile as it could be returning a `ref` to a stack location.</span></span> <span data-ttu-id="b9a4f-259">Por otro lado, si `this` no es Ref-Safe-to-escape, `p.Get` no puede hacer referencia a la pila y, por lo tanto, es seguro volver.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-259">On the other hand if `this` is not ref-safe-to-escape then `p.Get` cannot refer to the stack and hence it's safe to return.</span></span> 

<span data-ttu-id="b9a4f-260">Este es el motivo por el que la elusión de `this` en un `struct` es realmente una de las interfaces.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-260">This is why the escapability of `this` in a `struct` is really all about interfaces.</span></span> <span data-ttu-id="b9a4f-261">Se puede realizar de forma absoluta en el trabajo, pero tiene una desventaja.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-261">It can absolutely be made to work but it has a trade off.</span></span> <span data-ttu-id="b9a4f-262">Finalmente, el diseño apareció en favor de que las interfaces sean más flexibles.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-262">The design eventually came down in favor of making interfaces more flexible.</span></span> 

<span data-ttu-id="b9a4f-263">Sin embargo, es posible que esto se Relájese en el futuro.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-263">There is potential for us to relax this in the future though.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="b9a4f-264">Consideraciones futuras</span><span class="sxs-lookup"><span data-stu-id="b9a4f-264">Future Considerations</span></span>

### <a name="length-one-spant-over-ref-values"></a><span data-ttu-id="b9a4f-265">Longitud de un intervalo\<T > sobre valores de referencia</span><span class="sxs-lookup"><span data-stu-id="b9a4f-265">Length one Span\<T> over ref values</span></span>
<span data-ttu-id="b9a4f-266">Aunque no es legal hoy en día hay casos en los que la creación de una longitud de una `Span<T>` instancia sobre un valor sería beneficiosa:</span><span class="sxs-lookup"><span data-stu-id="b9a4f-266">Though not legal today there are cases where creating a length one `Span<T>` instance over a value would be beneficial:</span></span>

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

<span data-ttu-id="b9a4f-267">Esta característica resulta más atractiva si se eliminan las restricciones en los [búferes de tamaño fijo](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) , ya que esto permitiría `Span<T>` instancias de una longitud incluso mayor.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-267">This feature gets more compelling if we lift the restrictions on [fixed sized buffers](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) as it would allow for `Span<T>` instances of even greater length.</span></span> 

<span data-ttu-id="b9a4f-268">Si alguna vez es necesario bajar esta ruta de acceso, el lenguaje podría dar cabida a este caso asegurándose de que las instancias de `Span<T>` solo estaban orientadas hacia abajo.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-268">If there is ever a need to go down this path then the language could accommodate this by ensuring such `Span<T>` instances were downward facing only.</span></span> <span data-ttu-id="b9a4f-269">Es decir, solo estaban *seguros* para el ámbito en el que se crearon.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-269">That is they were only ever *safe-to-escape* to the scope in which they were created.</span></span> <span data-ttu-id="b9a4f-270">Esto garantiza que el lenguaje nunca tenía que considerar un valor `ref` escapar un método a través de un `ref struct` devuelto o un campo de `ref struct`.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-270">This ensure the language never had to consider a `ref` value escaping a method via a `ref struct` return or field of `ref struct`.</span></span> <span data-ttu-id="b9a4f-271">Lo más probable es que también requiera cambios adicionales para reconocer tales constructores como la captura de un parámetro de `ref` de esta manera.</span><span class="sxs-lookup"><span data-stu-id="b9a4f-271">This would likely also require further changes to recognize such constructors as capturing a `ref` parameter in this way though.</span></span>
