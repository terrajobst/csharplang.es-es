---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108379"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="ea4ab-101">Expresiones de `new` con tipo de destino</span><span class="sxs-lookup"><span data-stu-id="ea4ab-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="ea4ab-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="ea4ab-102">[x] Proposed</span></span>
* <span data-ttu-id="ea4ab-103">[x] [prototipo](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="ea4ab-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="ea4ab-104">[] Implementación</span><span class="sxs-lookup"><span data-stu-id="ea4ab-104">[ ] Implementation</span></span>
* <span data-ttu-id="ea4ab-105">[] Especificación</span><span class="sxs-lookup"><span data-stu-id="ea4ab-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="ea4ab-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="ea4ab-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ea4ab-107">No es necesario especificar la especificación de tipo para los constructores cuando se conoce el tipo.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="ea4ab-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="ea4ab-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="ea4ab-109">Permite la inicialización de campos sin duplicar el tipo.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="ea4ab-110">Permite omitir el tipo cuando se puede inferir del uso.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="ea4ab-111">Cree una instancia de un objeto sin deletrear el tipo.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="ea4ab-112">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="ea4ab-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="ea4ab-113">La sintaxis de *object_creation_expression* se modificaría para que el *tipo* fuera opcional cuando haya paréntesis.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="ea4ab-114">Esto es necesario para resolver la ambigüedad con *anonymous_object_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="ea4ab-115">Un `new` con tipo de destino se pueda convertir a cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="ea4ab-116">Como resultado, no contribuye a la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="ea4ab-117">Esto es principalmente para evitar cambios importantes imprevisibles.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="ea4ab-118">La lista de argumentos y las expresiones de inicializador se enlazarán después de que se determine el tipo.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="ea4ab-119">El tipo de la expresión se deduce del tipo de destino, que sería necesario que sea uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="ea4ab-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="ea4ab-120">**Cualquier tipo de struct** (incluidos los tipos de tupla)</span><span class="sxs-lookup"><span data-stu-id="ea4ab-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="ea4ab-121">**Cualquier tipo de referencia** (incluidos los tipos de delegado)</span><span class="sxs-lookup"><span data-stu-id="ea4ab-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="ea4ab-122">**Cualquier parámetro de tipo** con un constructor o una restricción `struct`</span><span class="sxs-lookup"><span data-stu-id="ea4ab-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="ea4ab-123">con las siguientes excepciones:</span><span class="sxs-lookup"><span data-stu-id="ea4ab-123">with the following exceptions:</span></span>

- <span data-ttu-id="ea4ab-124">**Tipos de enumeración:** no todos los tipos de enumeración contienen la constante cero, por lo que debe ser conveniente usar el miembro de enumeración explícito.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="ea4ab-125">**Tipos de interfaz:** se trata de una característica de nicho y debe ser preferible mencionar explícitamente el tipo.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="ea4ab-126">**Tipos de matriz:** las matrices necesitan una sintaxis especial para proporcionar la longitud.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="ea4ab-127">**dinámico:** no se permite el `new dynamic()`, por lo que no se permite el `new()` con `dynamic` como tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="ea4ab-128">Todos los demás tipos que no se permiten en el *object_creation_expression* también se excluyen, por ejemplo, los tipos de puntero.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="ea4ab-129">Cuando el tipo de destino es un tipo de valor que acepta valores NULL, el `new` con tipo de destino se convertirá en el tipo subyacente en lugar del tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="ea4ab-130">**Problema de apertura:** ¿se deben permitir los delegados y las tuplas como tipo de destino?</span><span class="sxs-lookup"><span data-stu-id="ea4ab-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="ea4ab-131">Las reglas anteriores incluyen delegados (un tipo de referencia) y tuplas (un tipo de estructura).</span><span class="sxs-lookup"><span data-stu-id="ea4ab-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="ea4ab-132">Aunque ambos tipos se pueden construir, si el tipo es inferido, ya se puede usar una función anónima o un literal de tupla.</span><span class="sxs-lookup"><span data-stu-id="ea4ab-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
