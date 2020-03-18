---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483527"
---
# <a name="nullable-enhanced-common-type"></a><span data-ttu-id="15b1d-101">Tipo común mejorado con valores NULL</span><span class="sxs-lookup"><span data-stu-id="15b1d-101">Nullable-Enhanced Common Type</span></span>

* <span data-ttu-id="15b1d-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="15b1d-102">[x] Proposed</span></span>
* <span data-ttu-id="15b1d-103">[] Prototipo: ninguno</span><span class="sxs-lookup"><span data-stu-id="15b1d-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="15b1d-104">[] Implementación: ninguno</span><span class="sxs-lookup"><span data-stu-id="15b1d-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="15b1d-105">[] Especificación: vea a continuación</span><span class="sxs-lookup"><span data-stu-id="15b1d-105">[ ] Specification: See below</span></span>

## <a name="summary"></a><span data-ttu-id="15b1d-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="15b1d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="15b1d-107">Existe una situación en la que el resultado del algoritmo de tipo común actual es un contador intuitivo y hace que el programador agregue lo que se parece a una conversión redundante en el código.</span><span class="sxs-lookup"><span data-stu-id="15b1d-107">There is a situation in which the current common-type algorithm results are counter-intuitive, and results in the programmer adding what feels like a redundant cast to the code.</span></span> <span data-ttu-id="15b1d-108">Con este cambio, una expresión como `condition ? 1 : null` daría como resultado un valor de tipo `int?`y una expresión como `condition ? x : 1.0` donde `x` es de tipo `int?` daría como resultado un valor de tipo `double?`.</span><span class="sxs-lookup"><span data-stu-id="15b1d-108">With this change, an expression such as `condition ? 1 : null` would result in a value of type `int?`, and an expression such as `condition ? x : 1.0` where `x` is of type `int?` would result in a value of type `double?`.</span></span>

## <a name="motivation"></a><span data-ttu-id="15b1d-109">Motivación</span><span class="sxs-lookup"><span data-stu-id="15b1d-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="15b1d-110">Se trata de una causa común de lo que le parece al programador, como el código reutilizable no necesario.</span><span class="sxs-lookup"><span data-stu-id="15b1d-110">This is a common cause of what feels to the programmer like needless boilerplate code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="15b1d-111">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="15b1d-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="15b1d-112">Se modifica la especificación para [Buscar el mejor tipo común de un conjunto de expresiones](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) que afecten a las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="15b1d-112">We modify the specification for [finding the best common type of a set of expressions](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) to affect the following situations:</span></span>

- <span data-ttu-id="15b1d-113">Si una expresión es de un tipo de valor que no acepta valores NULL `T` y la otra es un literal null, el resultado es de tipo `T?`.</span><span class="sxs-lookup"><span data-stu-id="15b1d-113">If one expression is of a non-nullable value type `T` and the other is a null literal, the result is of type `T?`.</span></span>
- <span data-ttu-id="15b1d-114">Si una expresión es de un tipo de valor que acepta valores NULL `T?` y la otra es de un tipo de valor `U`, y hay una conversión implícita de `T` a `U`, el resultado es de tipo `U?`.</span><span class="sxs-lookup"><span data-stu-id="15b1d-114">If one expression is of a nullable value type `T?` and the other is of a value type `U`, and there is an implicit conversion from `T` to `U`, then the result is of type `U?`.</span></span>

<span data-ttu-id="15b1d-115">Se espera que afecte a los siguientes aspectos del lenguaje:</span><span class="sxs-lookup"><span data-stu-id="15b1d-115">This is expected to affect the following aspects of the language:</span></span>

- <span data-ttu-id="15b1d-116">[expresión ternaria](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span><span class="sxs-lookup"><span data-stu-id="15b1d-116">the [ternary expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span></span>
- <span data-ttu-id="15b1d-117">expresión de creación de [matriz](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions) con tipo implícito</span><span class="sxs-lookup"><span data-stu-id="15b1d-117">implicitly typed [array creation expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span></span>
- <span data-ttu-id="15b1d-118">inferir el [tipo de valor devuelto de una expresión lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) para la inferencia de tipos</span><span class="sxs-lookup"><span data-stu-id="15b1d-118">inferring the [return type of a lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) for type inference</span></span>
- <span data-ttu-id="15b1d-119">los casos que implican genéricos, como invocar `M<T>(T a, T b)` como `M(1, null)`.</span><span class="sxs-lookup"><span data-stu-id="15b1d-119">cases involving generics, such as invoking `M<T>(T a, T b)` as `M(1, null)`.</span></span>

<span data-ttu-id="15b1d-120">Más concretamente, cambiamos las siguientes secciones de la especificación (inserciones en negrita, eliminaciones en tachado):</span><span class="sxs-lookup"><span data-stu-id="15b1d-120">More precisely, we change the following sections of the specification (insertions in bold, deletions in strikethrough):</span></span>

> #### <a name="output-type-inferences"></a><span data-ttu-id="15b1d-121">Inferencias de tipos de salida</span><span class="sxs-lookup"><span data-stu-id="15b1d-121">Output type inferences</span></span>
> 
> <span data-ttu-id="15b1d-122">Una *inferencia de tipo de salida* se realiza *desde* una expresión `E` *a* un tipo `T` de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="15b1d-122">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>
> 
> *  <span data-ttu-id="15b1d-123">Si `E` es una función anónima con el tipo de valor devuelto inferido `U` ([tipo de valor](expressions.md#inferred-return-type)devuelto deducido) y `T` es un tipo de delegado o un tipo de árbol de expresión con el tipo de valor devuelto `Tb`, se realiza una *inferencia de límite inferior* ([inferencias con límite inferior](expressions.md#lower-bound-inferences)) *de* `U` *a* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="15b1d-123">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="15b1d-124">De lo contrario, si `E` es un grupo de métodos y `T` es un tipo de delegado o un tipo de árbol de expresión con tipos de parámetro `T1...Tk` y `Tb`de tipo de valor devuelto, y la resolución de sobrecarga de `E` con los tipos `T1...Tk` produce un único método con el tipo de valor devuelto `U`, se realiza una *inferencia con un límite inferior* *desde* `U` *a* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="15b1d-124">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="15b1d-125">\* \* *De* lo contrario, si `E` es una expresión con un tipo de valor que acepta valores NULL `U?`, se realiza una *inferencia con límite inferior* desde `U` *a* `T` y se agrega un *enlace nulo* a `T`.</span><span class="sxs-lookup"><span data-stu-id="15b1d-125">\*\*Otherwise, if `E` is an expression with nullable value type `U?`, then a *lower-bound inference* is made *from* `U` *to* `T` and a *null bound* is added to `T`.</span></span> **
> *  <span data-ttu-id="15b1d-126">De lo contrario, si `E` es una expresión con el tipo `U`, se realiza una *inferencia de enlace inferior* *desde* `U` *a* `T`.</span><span class="sxs-lookup"><span data-stu-id="15b1d-126">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
> *  <span data-ttu-id="15b1d-127">**De lo contrario, si `E` es una expresión constante con el valor `null`, se agrega un *enlace nulo* a `T`**</span><span class="sxs-lookup"><span data-stu-id="15b1d-127">**Otherwise, if `E` is a constant expression with value `null`, then a *null bound* is added to `T`**</span></span> 
> *  <span data-ttu-id="15b1d-128">De lo contrario, no se realiza ninguna inferencia.</span><span class="sxs-lookup"><span data-stu-id="15b1d-128">Otherwise, no inferences are made.</span></span>

> #### <a name="fixing"></a><span data-ttu-id="15b1d-129">Corrección de</span><span class="sxs-lookup"><span data-stu-id="15b1d-129">Fixing</span></span>
> 
> <span data-ttu-id="15b1d-130">Una variable de tipo sin *fixed* `Xi` con un conjunto de límites se *fija* de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="15b1d-130">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>
> 
> *  <span data-ttu-id="15b1d-131">El conjunto de *tipos candidatos* `Uj` comienza como el conjunto de todos los tipos del conjunto de límites de `Xi`.</span><span class="sxs-lookup"><span data-stu-id="15b1d-131">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
> *  <span data-ttu-id="15b1d-132">A continuación, examinaremos cada límite por `Xi` a su vez: para cada `U` enlazado exacto de `Xi` todos los tipos `Uj` que no son idénticos a `U` se quitan del conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="15b1d-132">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="15b1d-133">Para cada `U` de límite inferior de `Xi` todos los tipos `Uj` en los que *no* se ha quitado una conversión implícita de `U` del conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="15b1d-133">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="15b1d-134">Para cada `U` de enlace superior de `Xi` todos los tipos `Uj` desde el que *no* se quita una conversión implícita a `U` del conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="15b1d-134">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
> *  <span data-ttu-id="15b1d-135">Si está entre los tipos de candidatos restantes `Uj` hay un tipo único `V` desde el que hay una conversión implícita a todos los demás tipos candidatos, ~~`Xi` se fija en `V`.~~</span><span class="sxs-lookup"><span data-stu-id="15b1d-135">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then ~~`Xi` is fixed to `V`.~~</span></span>
>     -  <span data-ttu-id="15b1d-136">**Si `V` es un tipo de valor y hay un *enlace nulo* para `Xi`, `Xi` se fija en `V?`**</span><span class="sxs-lookup"><span data-stu-id="15b1d-136">**If `V` is a value type and there is a *null bound* for `Xi`, then `Xi` is fixed to `V?`**</span></span>
>     -  <span data-ttu-id="15b1d-137">**De lo contrario `Xi` se fija en `V`**</span><span class="sxs-lookup"><span data-stu-id="15b1d-137">**Otherwise   `Xi` is fixed to `V`**</span></span>
> *  <span data-ttu-id="15b1d-138">De lo contrario, se produce un error en la inferencia de tipos.</span><span class="sxs-lookup"><span data-stu-id="15b1d-138">Otherwise, type inference fails.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="15b1d-139">Desventajas</span><span class="sxs-lookup"><span data-stu-id="15b1d-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="15b1d-140">Puede haber algunas incompatibilidades introducidas en esta propuesta.</span><span class="sxs-lookup"><span data-stu-id="15b1d-140">There may be some incompatibilities introduced by this proposal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="15b1d-141">Alternativas</span><span class="sxs-lookup"><span data-stu-id="15b1d-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="15b1d-142">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="15b1d-142">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="15b1d-143">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="15b1d-143">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="15b1d-144">[] ¿Cuál es la gravedad de incompatibilidad introducida en esta propuesta, si la hubiera, y cómo se puede moderar?</span><span class="sxs-lookup"><span data-stu-id="15b1d-144">[ ] What is the severity of incompatibility introduced by this proposal, if any, and how can it be moderated?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="15b1d-145">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="15b1d-145">Design meetings</span></span>

<span data-ttu-id="15b1d-146">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="15b1d-146">None.</span></span>
