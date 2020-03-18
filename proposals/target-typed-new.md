---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483533"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="c261f-101">Expresiones de `new` con tipo de destino</span><span class="sxs-lookup"><span data-stu-id="c261f-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="c261f-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="c261f-102">[x] Proposed</span></span>
* <span data-ttu-id="c261f-103">[x] [prototipo](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="c261f-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="c261f-104">[] Implementación</span><span class="sxs-lookup"><span data-stu-id="c261f-104">[ ] Implementation</span></span>
* <span data-ttu-id="c261f-105">[] Especificación</span><span class="sxs-lookup"><span data-stu-id="c261f-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="c261f-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="c261f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="c261f-107">No es necesario especificar la especificación de tipo para los constructores cuando se conoce el tipo.</span><span class="sxs-lookup"><span data-stu-id="c261f-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="c261f-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="c261f-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="c261f-109">Permite la inicialización de campos sin duplicar el tipo.</span><span class="sxs-lookup"><span data-stu-id="c261f-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
<span data-ttu-id="c261f-110">Permite omitir el tipo cuando se puede inferir del uso.</span><span class="sxs-lookup"><span data-stu-id="c261f-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
<span data-ttu-id="c261f-111">Cree una instancia de un objeto sin deletrear el tipo.</span><span class="sxs-lookup"><span data-stu-id="c261f-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a><span data-ttu-id="c261f-112">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="c261f-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="c261f-113">La sintaxis de *object_creation_expression* se modificaría para que el *tipo* fuera opcional cuando haya paréntesis.</span><span class="sxs-lookup"><span data-stu-id="c261f-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="c261f-114">Esto es necesario para resolver la ambigüedad con *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="c261f-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
<span data-ttu-id="c261f-115">Un `new` con tipo de destino se pueda convertir a cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="c261f-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="c261f-116">Como resultado, no contribuye a la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="c261f-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="c261f-117">Esto es principalmente para evitar cambios importantes imprevisibles.</span><span class="sxs-lookup"><span data-stu-id="c261f-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="c261f-118">La lista de argumentos y las expresiones de inicializador se enlazarán después de que se determine el tipo.</span><span class="sxs-lookup"><span data-stu-id="c261f-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="c261f-119">El tipo de la expresión se deduce del tipo de destino, que sería necesario que sea uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="c261f-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="c261f-120">**Cualquier tipo de struct**</span><span class="sxs-lookup"><span data-stu-id="c261f-120">**Any struct type**</span></span>
- <span data-ttu-id="c261f-121">**Cualquier tipo de referencia**</span><span class="sxs-lookup"><span data-stu-id="c261f-121">**Any reference type**</span></span>
- <span data-ttu-id="c261f-122">**Cualquier parámetro de tipo** con un constructor o una restricción `struct`</span><span class="sxs-lookup"><span data-stu-id="c261f-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="c261f-123">con las siguientes excepciones:</span><span class="sxs-lookup"><span data-stu-id="c261f-123">with the following exceptions:</span></span>

- <span data-ttu-id="c261f-124">**Tipos de enumeración:** no todos los tipos de enumeración contienen la constante cero, por lo que debe ser conveniente usar el miembro de enumeración explícito.</span><span class="sxs-lookup"><span data-stu-id="c261f-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="c261f-125">**Tipos de interfaz:** se trata de una característica de nicho y debe ser preferible mencionar explícitamente el tipo.</span><span class="sxs-lookup"><span data-stu-id="c261f-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="c261f-126">**Tipos de matriz:** las matrices necesitan una sintaxis especial para proporcionar la longitud.</span><span class="sxs-lookup"><span data-stu-id="c261f-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="c261f-127">**Constructor predeterminado de struct**: esta regla extrae todos los tipos primitivos y la mayoría de los tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="c261f-127">**Struct default constructor**: this rules out all primitive types and most value types.</span></span> <span data-ttu-id="c261f-128">Si desea utilizar el valor predeterminado de estos tipos, podría escribir `default` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="c261f-128">If you wanted to use the default value of such types you could write `default` instead.</span></span>

<span data-ttu-id="c261f-129">Todos los demás tipos que no se permiten en el *object_creation_expression* también se excluyen, por ejemplo, los tipos de puntero.</span><span class="sxs-lookup"><span data-stu-id="c261f-129">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

> <span data-ttu-id="c261f-130">**Problema de apertura:** ¿se deben permitir los delegados y las tuplas como tipo de destino?</span><span class="sxs-lookup"><span data-stu-id="c261f-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="c261f-131">Las reglas anteriores incluyen delegados (un tipo de referencia) y tuplas (un tipo de estructura).</span><span class="sxs-lookup"><span data-stu-id="c261f-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="c261f-132">Aunque ambos tipos se pueden construir, si el tipo es inferido, ya se puede usar una función anónima o un literal de tupla.</span><span class="sxs-lookup"><span data-stu-id="c261f-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> <span data-ttu-id="c261f-133">**Problema de apertura:** ¿se debe permitir `throw new()` con `Exception` como tipo de destino?</span><span class="sxs-lookup"><span data-stu-id="c261f-133">**Open Issue:** should we allow `throw new()` with `Exception` as the target-type?</span></span>

<span data-ttu-id="c261f-134">Tenemos `throw null` hoy, pero no `throw default` (aunque tendría el mismo efecto).</span><span class="sxs-lookup"><span data-stu-id="c261f-134">We have `throw null` today, but not `throw default` (though it would have the same effect).</span></span> <span data-ttu-id="c261f-135">Por otro lado, `throw new()` podría ser realmente útil como una forma abreviada de `throw new Exception(...)`.</span><span class="sxs-lookup"><span data-stu-id="c261f-135">On the other hand, `throw new()` could be actually useful as a shorthand for `throw new Exception(...)`.</span></span> <span data-ttu-id="c261f-136">Tenga en cuenta que la especificación actual ya lo permite.</span><span class="sxs-lookup"><span data-stu-id="c261f-136">Note that it is already allowed by the current specification.</span></span> <span data-ttu-id="c261f-137">`Exception` es un tipo de referencia y la especificación de la instrucción throw indica que la expresión se convierte en `Exception`.</span><span class="sxs-lookup"><span data-stu-id="c261f-137">`Exception` is a reference type, and the specification for the throw statement says that the expression is converted to `Exception`.</span></span>

> <span data-ttu-id="c261f-138">**Problema de apertura:** ¿se deben permitir los usos de `new` con tipo de destino con operadores aritméticos y de comparación definidos por el usuario?</span><span class="sxs-lookup"><span data-stu-id="c261f-138">**Open Issue:** should we allow usages of a target-typed `new` with user-defined comparison and arithmetic operators?</span></span>

<span data-ttu-id="c261f-139">Para la comparación, `default` solo admite operadores de igualdad (definidos por el usuario e integrados).</span><span class="sxs-lookup"><span data-stu-id="c261f-139">For comparison, `default` only supports equality (user-defined and built-in) operators.</span></span> <span data-ttu-id="c261f-140">¿Tendría sentido admitir también otros operadores para `new()`?</span><span class="sxs-lookup"><span data-stu-id="c261f-140">Would it make sense to support other operators for `new()` as well?</span></span>

## <a name="drawbacks"></a><span data-ttu-id="c261f-141">Desventajas</span><span class="sxs-lookup"><span data-stu-id="c261f-141">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="c261f-142">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="c261f-142">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="c261f-143">Alternativas</span><span class="sxs-lookup"><span data-stu-id="c261f-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="c261f-144">La mayoría de las quejas acerca de los tipos que son demasiado largos para duplicarlos en la inicialización de campos es acerca de los *argumentos de tipo* que no son del tipo en sí, solo se podían inferir argumentos de tipo como `new Dictionary(...)` (o similares) e inferir argumentos de tipo localmente a partir de argumentos o del inicializador de colección.</span><span class="sxs-lookup"><span data-stu-id="c261f-144">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="c261f-145">Preguntas</span><span class="sxs-lookup"><span data-stu-id="c261f-145">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="c261f-146">¿Se deben prohibir los usos en los árboles de expresión?</span><span class="sxs-lookup"><span data-stu-id="c261f-146">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="c261f-147">ninguno</span><span class="sxs-lookup"><span data-stu-id="c261f-147">(no)</span></span>
- <span data-ttu-id="c261f-148">¿Cómo interactúa la característica con `dynamic` argumentos?</span><span class="sxs-lookup"><span data-stu-id="c261f-148">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="c261f-149">(sin tratamiento especial)</span><span class="sxs-lookup"><span data-stu-id="c261f-149">(no special treatment)</span></span>
- <span data-ttu-id="c261f-150">¿Cómo debería funcionar IntelliSense con `new()`?</span><span class="sxs-lookup"><span data-stu-id="c261f-150">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="c261f-151">(solo cuando hay un solo tipo de destino)</span><span class="sxs-lookup"><span data-stu-id="c261f-151">(only when there is a single target-type)</span></span>
## <a name="design-meetings"></a><span data-ttu-id="c261f-152">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="c261f-152">Design meetings</span></span>

- [<span data-ttu-id="c261f-153">LDM: 2017-10-18</span><span class="sxs-lookup"><span data-stu-id="c261f-153">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
