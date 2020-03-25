---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217208"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="2620f-101">Expresiones de `new` con tipo de destino</span><span class="sxs-lookup"><span data-stu-id="2620f-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="2620f-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="2620f-102">[x] Proposed</span></span>
* <span data-ttu-id="2620f-103">[x] [prototipo](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="2620f-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="2620f-104">[] Implementación</span><span class="sxs-lookup"><span data-stu-id="2620f-104">[ ] Implementation</span></span>
* <span data-ttu-id="2620f-105">[] Especificación</span><span class="sxs-lookup"><span data-stu-id="2620f-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="2620f-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="2620f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="2620f-107">No es necesario especificar la especificación de tipo para los constructores cuando se conoce el tipo.</span><span class="sxs-lookup"><span data-stu-id="2620f-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="2620f-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="2620f-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="2620f-109">Permite la inicialización de campos sin duplicar el tipo.</span><span class="sxs-lookup"><span data-stu-id="2620f-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="2620f-110">Permite omitir el tipo cuando se puede inferir del uso.</span><span class="sxs-lookup"><span data-stu-id="2620f-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="2620f-111">Cree una instancia de un objeto sin deletrear el tipo.</span><span class="sxs-lookup"><span data-stu-id="2620f-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="2620f-112">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="2620f-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="2620f-113">La sintaxis de *object_creation_expression* se modificaría para que el *tipo* fuera opcional cuando haya paréntesis.</span><span class="sxs-lookup"><span data-stu-id="2620f-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="2620f-114">Esto es necesario para resolver la ambigüedad con *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="2620f-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="2620f-115">Un `new` con tipo de destino se pueda convertir a cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="2620f-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="2620f-116">Como resultado, no contribuye a la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="2620f-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="2620f-117">Esto es principalmente para evitar cambios importantes imprevisibles.</span><span class="sxs-lookup"><span data-stu-id="2620f-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="2620f-118">La lista de argumentos y las expresiones de inicializador se enlazarán después de que se determine el tipo.</span><span class="sxs-lookup"><span data-stu-id="2620f-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="2620f-119">El tipo de la expresión se deduce del tipo de destino, que sería necesario que sea uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="2620f-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="2620f-120">**Cualquier tipo de struct** (incluidos los tipos de tupla)</span><span class="sxs-lookup"><span data-stu-id="2620f-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="2620f-121">**Cualquier tipo de referencia** (incluidos los tipos de delegado)</span><span class="sxs-lookup"><span data-stu-id="2620f-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="2620f-122">**Cualquier parámetro de tipo** con un constructor o una restricción `struct`</span><span class="sxs-lookup"><span data-stu-id="2620f-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="2620f-123">con las siguientes excepciones:</span><span class="sxs-lookup"><span data-stu-id="2620f-123">with the following exceptions:</span></span>

- <span data-ttu-id="2620f-124">**Tipos de enumeración:** no todos los tipos de enumeración contienen la constante cero, por lo que debe ser conveniente usar el miembro de enumeración explícito.</span><span class="sxs-lookup"><span data-stu-id="2620f-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="2620f-125">**Tipos de interfaz:** se trata de una característica de nicho y debe ser preferible mencionar explícitamente el tipo.</span><span class="sxs-lookup"><span data-stu-id="2620f-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="2620f-126">**Tipos de matriz:** las matrices necesitan una sintaxis especial para proporcionar la longitud.</span><span class="sxs-lookup"><span data-stu-id="2620f-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="2620f-127">**dinámico:** no se permite el `new dynamic()`, por lo que no se permite el `new()` con `dynamic` como tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="2620f-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="2620f-128">Todos los demás tipos que no se permiten en el *object_creation_expression* también se excluyen, por ejemplo, los tipos de puntero.</span><span class="sxs-lookup"><span data-stu-id="2620f-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="2620f-129">Cuando el tipo de destino es un tipo de valor que acepta valores NULL, el `new` con tipo de destino se convertirá en el tipo subyacente en lugar del tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="2620f-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="2620f-130">**Problema de apertura:** ¿se deben permitir los delegados y las tuplas como tipo de destino?</span><span class="sxs-lookup"><span data-stu-id="2620f-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="2620f-131">Las reglas anteriores incluyen delegados (un tipo de referencia) y tuplas (un tipo de estructura).</span><span class="sxs-lookup"><span data-stu-id="2620f-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="2620f-132">Aunque ambos tipos se pueden construir, si el tipo es inferido, ya se puede usar una función anónima o un literal de tupla.</span><span class="sxs-lookup"><span data-stu-id="2620f-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="2620f-133">Varios</span><span class="sxs-lookup"><span data-stu-id="2620f-133">Miscellaneous</span></span>

<span data-ttu-id="2620f-134">`throw new()` no está permitido.</span><span class="sxs-lookup"><span data-stu-id="2620f-134">`throw new()` is disallowed.</span></span>

<span data-ttu-id="2620f-135">No se permite el `new` con tipo de destino con operadores binarios.</span><span class="sxs-lookup"><span data-stu-id="2620f-135">Target-typed `new` is not allowed with binary operators.</span></span>

<span data-ttu-id="2620f-136">No se permite cuando no hay ningún tipo para el destino: operadores unarios, colección de un `foreach`, en un `using`, en una desconstrucción, en una expresión de `await`, como una propiedad de tipo anónimo (`new { Prop = new() }`), en una instrucción `lock`, en una `sizeof`, en una instrucción `fixed`, en un acceso de miembro (`new().field`), en una operación distribuida dinámicamente (`someDynamic.Method(new())`), en una consulta LINQ, como operando del operador `is`, como operando izquierdo del operador `??` ,  ...</span><span class="sxs-lookup"><span data-stu-id="2620f-136">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>

<span data-ttu-id="2620f-137">También se deniega como `ref`.</span><span class="sxs-lookup"><span data-stu-id="2620f-137">It is also disallowed as a `ref`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="2620f-138">Desventajas</span><span class="sxs-lookup"><span data-stu-id="2620f-138">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="2620f-139">Había algunos problemas con el tipo de destino `new` crear nuevas categorías de cambios importantes, pero ya tenemos eso con `null` y `default`, y que no ha sido un problema importante.</span><span class="sxs-lookup"><span data-stu-id="2620f-139">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="2620f-140">Alternativas</span><span class="sxs-lookup"><span data-stu-id="2620f-140">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="2620f-141">La mayoría de las quejas acerca de los tipos que son demasiado largos para duplicarlos en la inicialización de campos es acerca de los *argumentos de tipo* que no son del tipo en sí, solo se podían inferir argumentos de tipo como `new Dictionary(...)` (o similares) e inferir argumentos de tipo localmente a partir de argumentos o del inicializador de colección.</span><span class="sxs-lookup"><span data-stu-id="2620f-141">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="2620f-142">Preguntas</span><span class="sxs-lookup"><span data-stu-id="2620f-142">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="2620f-143">¿Se deben prohibir los usos en los árboles de expresión?</span><span class="sxs-lookup"><span data-stu-id="2620f-143">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="2620f-144">ninguno</span><span class="sxs-lookup"><span data-stu-id="2620f-144">(no)</span></span>
- <span data-ttu-id="2620f-145">¿Cómo interactúa la característica con `dynamic` argumentos?</span><span class="sxs-lookup"><span data-stu-id="2620f-145">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="2620f-146">(sin tratamiento especial)</span><span class="sxs-lookup"><span data-stu-id="2620f-146">(no special treatment)</span></span>
- <span data-ttu-id="2620f-147">¿Cómo debería funcionar IntelliSense con `new()`?</span><span class="sxs-lookup"><span data-stu-id="2620f-147">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="2620f-148">(solo cuando hay un solo tipo de destino)</span><span class="sxs-lookup"><span data-stu-id="2620f-148">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="2620f-149">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="2620f-149">Design meetings</span></span>

- [<span data-ttu-id="2620f-150">LDM: 2017-10-18</span><span class="sxs-lookup"><span data-stu-id="2620f-150">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="2620f-151">LDM: 2018-05-21</span><span class="sxs-lookup"><span data-stu-id="2620f-151">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="2620f-152">LDM: 2018-06-25</span><span class="sxs-lookup"><span data-stu-id="2620f-152">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="2620f-153">LDM: 2018-08-22</span><span class="sxs-lookup"><span data-stu-id="2620f-153">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="2620f-154">LDM: 2018-10-17</span><span class="sxs-lookup"><span data-stu-id="2620f-154">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
