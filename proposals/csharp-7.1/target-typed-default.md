---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "79483917"
---
# <a name="target-typed-default-literal"></a><span data-ttu-id="2a3da-101">Literal "default" de tipo de destino</span><span class="sxs-lookup"><span data-stu-id="2a3da-101">Target-typed "default" literal</span></span>

* <span data-ttu-id="2a3da-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="2a3da-102">[x] Proposed</span></span>
* <span data-ttu-id="2a3da-103">[x] prototipo</span><span class="sxs-lookup"><span data-stu-id="2a3da-103">[x] Prototype</span></span>
* <span data-ttu-id="2a3da-104">[x] implementación</span><span class="sxs-lookup"><span data-stu-id="2a3da-104">[x] Implementation</span></span>
* <span data-ttu-id="2a3da-105">[] Especificación</span><span class="sxs-lookup"><span data-stu-id="2a3da-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="2a3da-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="2a3da-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="2a3da-107">La característica `default` con tipo de destino es una variación de forma más corta del operador `default(T)`, que permite omitir el tipo.</span><span class="sxs-lookup"><span data-stu-id="2a3da-107">The target-typed `default` feature is a shorter form variation of the `default(T)` operator, which allows the type to be omitted.</span></span> <span data-ttu-id="2a3da-108">En su lugar, se deduce su tipo mediante el establecimiento de destinos.</span><span class="sxs-lookup"><span data-stu-id="2a3da-108">Its type is inferred by target-typing instead.</span></span> <span data-ttu-id="2a3da-109">Aparte de eso, se comporta como `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="2a3da-109">Aside from that, it behaves like `default(T)`.</span></span>

## <a name="motivation"></a><span data-ttu-id="2a3da-110">Motivación</span><span class="sxs-lookup"><span data-stu-id="2a3da-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="2a3da-111">La principal motivación es evitar escribir información redundante.</span><span class="sxs-lookup"><span data-stu-id="2a3da-111">The main motivation is to avoid typing redundant information.</span></span>

<span data-ttu-id="2a3da-112">Por ejemplo, al invocar `void Method(ImmutableArray<SomeType> array)`, el literal *predeterminado* permite `M(default)` en lugar de `M(default(ImmutableArray<SomeType>))`.</span><span class="sxs-lookup"><span data-stu-id="2a3da-112">For instance, when invoking `void Method(ImmutableArray<SomeType> array)`, the *default* literal allows `M(default)` in place of `M(default(ImmutableArray<SomeType>))`.</span></span>

<span data-ttu-id="2a3da-113">Esto es aplicable en varios escenarios, como:</span><span class="sxs-lookup"><span data-stu-id="2a3da-113">This is applicable in a number of scenarios, such as:</span></span>

- <span data-ttu-id="2a3da-114">declarar variables locales (`ImmutableArray<SomeType> x = default;`)</span><span class="sxs-lookup"><span data-stu-id="2a3da-114">declaring locals (`ImmutableArray<SomeType> x = default;`)</span></span>
- <span data-ttu-id="2a3da-115">operaciones ternaria (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span><span class="sxs-lookup"><span data-stu-id="2a3da-115">ternary operations (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span></span>
- <span data-ttu-id="2a3da-116">devolver métodos y expresiones lambda (`return default;`)</span><span class="sxs-lookup"><span data-stu-id="2a3da-116">returning in methods and lambdas (`return default;`)</span></span>
- <span data-ttu-id="2a3da-117">declarar valores predeterminados para parámetros opcionales (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span><span class="sxs-lookup"><span data-stu-id="2a3da-117">declaring default values for optional parameters (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span></span>
- <span data-ttu-id="2a3da-118">incluir valores predeterminados en expresiones de creación de matrices (`var x = new[] { default, ImmutableArray.Create(y) };`)</span><span class="sxs-lookup"><span data-stu-id="2a3da-118">including default values in array creation expressions (`var x = new[] { default, ImmutableArray.Create(y) };`)</span></span>


## <a name="detailed-design"></a><span data-ttu-id="2a3da-119">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="2a3da-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="2a3da-120">Se introduce una nueva expresión, el literal *predeterminado* .</span><span class="sxs-lookup"><span data-stu-id="2a3da-120">A new expression is introduced, the *default* literal.</span></span> <span data-ttu-id="2a3da-121">Una expresión con esta clasificación se puede convertir implícitamente a cualquier tipo mediante una *conversión literal predeterminada*.</span><span class="sxs-lookup"><span data-stu-id="2a3da-121">An expression with this classification can be implicitly converted to any type, by a *default literal conversion*.</span></span> 

<span data-ttu-id="2a3da-122">La inferencia del tipo del literal *predeterminado* funciona igual que para el literal *null* , salvo que se permite cualquier tipo (no solo los tipos de referencia).</span><span class="sxs-lookup"><span data-stu-id="2a3da-122">The inference of the type for the *default* literal works the same as that for the *null* literal, except that any type is allowed (not just reference types).</span></span>

<span data-ttu-id="2a3da-123">Esta conversión genera el valor predeterminado del tipo deducido.</span><span class="sxs-lookup"><span data-stu-id="2a3da-123">This conversion produces the default value of the inferred type.</span></span>

<span data-ttu-id="2a3da-124">El literal *predeterminado* puede tener un valor constante, dependiendo del tipo deducido.</span><span class="sxs-lookup"><span data-stu-id="2a3da-124">The *default* literal may have a constant value, depending on the inferred type.</span></span> <span data-ttu-id="2a3da-125">Por tanto `const int x = default;` es válido, pero `const int? y = default;` no.</span><span class="sxs-lookup"><span data-stu-id="2a3da-125">So `const int x = default;` is legal, but `const int? y = default;` is not.</span></span>

<span data-ttu-id="2a3da-126">El literal *predeterminado* puede ser el operando de los operadores de igualdad, siempre que el otro operando tenga un tipo.</span><span class="sxs-lookup"><span data-stu-id="2a3da-126">The *default* literal can be the operand of equality operators, as long as the other operand has a type.</span></span> <span data-ttu-id="2a3da-127">Por tanto `default == x` y `x == default` son expresiones válidas, pero `default == default` no es válido.</span><span class="sxs-lookup"><span data-stu-id="2a3da-127">So `default == x` and `x == default` are valid expressions, but `default == default` is illegal.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="2a3da-128">Desventajas</span><span class="sxs-lookup"><span data-stu-id="2a3da-128">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="2a3da-129">Una desventaja menor es que se puede usar el literal *predeterminado* en lugar de un literal *nulo* en la mayoría de los contextos.</span><span class="sxs-lookup"><span data-stu-id="2a3da-129">A minor drawback is that *default* literal can be used in place of *null* literal in most contexts.</span></span> <span data-ttu-id="2a3da-130">Dos de las excepciones son `throw null;` y `null == null`, que se permiten para el literal *null* , pero no para el literal *predeterminado* .</span><span class="sxs-lookup"><span data-stu-id="2a3da-130">Two of the exceptions are `throw null;` and `null == null`, which are allowed for the *null* literal, but not the *default* literal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="2a3da-131">Alternativas</span><span class="sxs-lookup"><span data-stu-id="2a3da-131">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="2a3da-132">Hay un par de alternativas a tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="2a3da-132">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="2a3da-133">El estado quo: la característica no está justificada por sus propios méritos y los desarrolladores siguen usando el operador predeterminado con un tipo explícito.</span><span class="sxs-lookup"><span data-stu-id="2a3da-133">The status quo:  The feature is not justified on its own merits and developers continue to use the default operator with an explicit type.</span></span>
- <span data-ttu-id="2a3da-134">Extender el literal NULL: este es el enfoque de VB con `Nothing`.</span><span class="sxs-lookup"><span data-stu-id="2a3da-134">Extending the null literal: This is the VB approach with `Nothing`.</span></span> <span data-ttu-id="2a3da-135">Podríamos permitir el `int x = null;`.</span><span class="sxs-lookup"><span data-stu-id="2a3da-135">We could allow `int x = null;`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="2a3da-136">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="2a3da-136">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="2a3da-137">[x] ¿se debe permitir de *forma predeterminada* como operando de los operadores *is* o *as* ?</span><span class="sxs-lookup"><span data-stu-id="2a3da-137">[x] Should *default* be allowed as the operand of the *is* or *as* operators?</span></span> <span data-ttu-id="2a3da-138">Respuesta: no permitir `default is T`, permitir `x is default`, permitir `default as RefType` (con ADVERTENCIA siempre nula)</span><span class="sxs-lookup"><span data-stu-id="2a3da-138">Answer:  disallow `default is T`, allow `x is default`, allow `default as RefType` (with always-null warning)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="2a3da-139">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="2a3da-139">Design meetings</span></span>

- [<span data-ttu-id="2a3da-140">LDM 3/7/2017</span><span class="sxs-lookup"><span data-stu-id="2a3da-140">LDM 3/7/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [<span data-ttu-id="2a3da-141">LDM 3/28/2017</span><span class="sxs-lookup"><span data-stu-id="2a3da-141">LDM 3/28/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [<span data-ttu-id="2a3da-142">LDM 5/31/2017</span><span class="sxs-lookup"><span data-stu-id="2a3da-142">LDM 5/31/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
