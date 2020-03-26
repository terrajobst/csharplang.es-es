---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281975"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="e414b-101">Expresiones de `new` con tipo de destino</span><span class="sxs-lookup"><span data-stu-id="e414b-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="e414b-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="e414b-102">[x] Proposed</span></span>
* <span data-ttu-id="e414b-103">[x] [prototipo](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="e414b-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="e414b-104">[] Implementación</span><span class="sxs-lookup"><span data-stu-id="e414b-104">[ ] Implementation</span></span>
* <span data-ttu-id="e414b-105">[] Especificación</span><span class="sxs-lookup"><span data-stu-id="e414b-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="e414b-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="e414b-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e414b-107">No es necesario especificar la especificación de tipo para los constructores cuando se conoce el tipo.</span><span class="sxs-lookup"><span data-stu-id="e414b-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="e414b-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="e414b-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="e414b-109">Permite la inicialización de campos sin duplicar el tipo.</span><span class="sxs-lookup"><span data-stu-id="e414b-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="e414b-110">Permite omitir el tipo cuando se puede inferir del uso.</span><span class="sxs-lookup"><span data-stu-id="e414b-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="e414b-111">Cree una instancia de un objeto sin deletrear el tipo.</span><span class="sxs-lookup"><span data-stu-id="e414b-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="specification"></a><span data-ttu-id="e414b-112">Especificación</span><span class="sxs-lookup"><span data-stu-id="e414b-112">Specification</span></span>
[design]: #detailed-design

<span data-ttu-id="e414b-113">Nueva forma sintáctica, *target_typed_new* de la *object_creation_expression* se acepta en la que el *tipo* es opcional.</span><span class="sxs-lookup"><span data-stu-id="e414b-113">A new syntactic form, *target_typed_new* of the *object_creation_expression* is accepted in which the *type* is optional.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

<span data-ttu-id="e414b-114">Una expresión *target_typed_new* no tiene un tipo.</span><span class="sxs-lookup"><span data-stu-id="e414b-114">A *target_typed_new* expression does not have a type.</span></span> <span data-ttu-id="e414b-115">Sin embargo, hay una nueva *conversión de creación de objetos* que es una conversión implícita de la expresión, que existe de una *target_typed_new* a cada tipo.</span><span class="sxs-lookup"><span data-stu-id="e414b-115">However, there is a new *object creation conversion* that is an implicit conversion from expression, that exists from a *target_typed_new* to every type.</span></span>

<span data-ttu-id="e414b-116">Dado un tipo de destino `T`, el `T0` de tipo es el tipo subyacente `T`si `T` es una instancia de `System.Nullable`.</span><span class="sxs-lookup"><span data-stu-id="e414b-116">Given a target type `T`, the type `T0` is `T`'s underlying type if `T` is an instance of `System.Nullable`.</span></span> <span data-ttu-id="e414b-117">De lo contrario `T0` es `T`.</span><span class="sxs-lookup"><span data-stu-id="e414b-117">Otherwise `T0` is `T`.</span></span> <span data-ttu-id="e414b-118">El significado de una expresión *target_typed_new* que se convierte al tipo `T` es igual que el significado de una *object_creation_expression* correspondiente que especifica `T0` como el tipo.</span><span class="sxs-lookup"><span data-stu-id="e414b-118">The meaning of a *target_typed_new* expression that is converted to the type `T` is the same as the meaning of a corresponding *object_creation_expression* that specifies `T0` as the type.</span></span>

<span data-ttu-id="e414b-119">Es un error en tiempo de compilación si se utiliza un *target_typed_new* como operando de un operador unario o binario, o si se utiliza cuando no está sujeto a una *conversión de creación de objeto*.</span><span class="sxs-lookup"><span data-stu-id="e414b-119">It is a compile-time error if a *target_typed_new* is used as an operand of a unary or binary operator, or if it is used where it is not subject to an *object creation conversion*.</span></span>

> <span data-ttu-id="e414b-120">**Problema de apertura:** ¿se deben permitir los delegados y las tuplas como tipo de destino?</span><span class="sxs-lookup"><span data-stu-id="e414b-120">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="e414b-121">Las reglas anteriores incluyen delegados (un tipo de referencia) y tuplas (un tipo de estructura).</span><span class="sxs-lookup"><span data-stu-id="e414b-121">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="e414b-122">Aunque ambos tipos se pueden construir, si el tipo es inferido, ya se puede usar una función anónima o un literal de tupla.</span><span class="sxs-lookup"><span data-stu-id="e414b-122">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="e414b-123">Varios</span><span class="sxs-lookup"><span data-stu-id="e414b-123">Miscellaneous</span></span>

<span data-ttu-id="e414b-124">Las siguientes son las consecuencias de la especificación:</span><span class="sxs-lookup"><span data-stu-id="e414b-124">The following are consequences of the specification:</span></span>

- <span data-ttu-id="e414b-125">se permite `throw new()` (el tipo de destino es `System.Exception`)</span><span class="sxs-lookup"><span data-stu-id="e414b-125">`throw new()` is allowed (the target type is `System.Exception`)</span></span>
- <span data-ttu-id="e414b-126">No se permite el `new` con tipo de destino con operadores binarios.</span><span class="sxs-lookup"><span data-stu-id="e414b-126">Target-typed `new` is not allowed with binary operators.</span></span>
- <span data-ttu-id="e414b-127">No se permite cuando no hay ningún tipo para el destino: operadores unarios, colección de un `foreach`, en un `using`, en una desconstrucción, en una expresión de `await`, como una propiedad de tipo anónimo (`new { Prop = new() }`), en una instrucción `lock`, en una `sizeof`, en una instrucción `fixed`, en un acceso de miembro (`new().field`), en una operación distribuida dinámicamente (`someDynamic.Method(new())`), en una consulta LINQ, como operando del operador `is`, como operando izquierdo del operador `??` ,  ...</span><span class="sxs-lookup"><span data-stu-id="e414b-127">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>
- <span data-ttu-id="e414b-128">También se deniega como `ref`.</span><span class="sxs-lookup"><span data-stu-id="e414b-128">It is also disallowed as a `ref`.</span></span>
- <span data-ttu-id="e414b-129">No se permiten los siguientes tipos de tipos como destinos de la conversión</span><span class="sxs-lookup"><span data-stu-id="e414b-129">The following kinds of types are not permitted as targets of the conversion</span></span>
  - <span data-ttu-id="e414b-130">**Tipos de enumeración:** `new()` funcionará (a medida que `new Enum()` funcione para proporcionar el valor predeterminado), pero `new(1)` no funcionará porque los tipos de enumeración no tienen un constructor.</span><span class="sxs-lookup"><span data-stu-id="e414b-130">**Enum types:** `new()` will work (as `new Enum()` works to give the default value), but `new(1)` will not work as enum types do not have a constructor.</span></span>
  - <span data-ttu-id="e414b-131">**Tipos de interfaz:** Esto funcionaría igual que la expresión de creación correspondiente para los tipos COM.</span><span class="sxs-lookup"><span data-stu-id="e414b-131">**Interface types:** This would work the same as the corresponding creation expression for COM types.</span></span>
  - <span data-ttu-id="e414b-132">**Tipos de matriz:** las matrices necesitan una sintaxis especial para proporcionar la longitud.</span><span class="sxs-lookup"><span data-stu-id="e414b-132">**Array types:** arrays need a special syntax to provide the length.</span></span>    
  - <span data-ttu-id="e414b-133">**dinámico:** no se permite el `new dynamic()`, por lo que no se permite el `new()` con `dynamic` como tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="e414b-133">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>
  - <span data-ttu-id="e414b-134">**tuplas:** Tienen el mismo significado que la creación de un objeto utilizando el tipo subyacente.</span><span class="sxs-lookup"><span data-stu-id="e414b-134">**tuples:** These have the same meaning as an object creation using the underlying type.</span></span>
  - <span data-ttu-id="e414b-135">Todos los demás tipos que no se permiten en el *object_creation_expression* también se excluyen, por ejemplo, los tipos de puntero.</span><span class="sxs-lookup"><span data-stu-id="e414b-135">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>   

## <a name="drawbacks"></a><span data-ttu-id="e414b-136">Desventajas</span><span class="sxs-lookup"><span data-stu-id="e414b-136">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="e414b-137">Había algunos problemas con el tipo de destino `new` crear nuevas categorías de cambios importantes, pero ya tenemos eso con `null` y `default`, y que no ha sido un problema importante.</span><span class="sxs-lookup"><span data-stu-id="e414b-137">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="e414b-138">Alternativas</span><span class="sxs-lookup"><span data-stu-id="e414b-138">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e414b-139">La mayoría de las quejas acerca de los tipos que son demasiado largos para duplicarlos en la inicialización de campos es acerca de los *argumentos de tipo* que no son del tipo en sí, solo se podían inferir argumentos de tipo como `new Dictionary(...)` (o similares) e inferir argumentos de tipo localmente a partir de argumentos o del inicializador de colección.</span><span class="sxs-lookup"><span data-stu-id="e414b-139">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="e414b-140">Preguntas</span><span class="sxs-lookup"><span data-stu-id="e414b-140">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="e414b-141">¿Se deben prohibir los usos en los árboles de expresión?</span><span class="sxs-lookup"><span data-stu-id="e414b-141">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="e414b-142">ninguno</span><span class="sxs-lookup"><span data-stu-id="e414b-142">(no)</span></span>
- <span data-ttu-id="e414b-143">¿Cómo interactúa la característica con `dynamic` argumentos?</span><span class="sxs-lookup"><span data-stu-id="e414b-143">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="e414b-144">(sin tratamiento especial)</span><span class="sxs-lookup"><span data-stu-id="e414b-144">(no special treatment)</span></span>
- <span data-ttu-id="e414b-145">¿Cómo debería funcionar IntelliSense con `new()`?</span><span class="sxs-lookup"><span data-stu-id="e414b-145">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="e414b-146">(solo cuando hay un solo tipo de destino)</span><span class="sxs-lookup"><span data-stu-id="e414b-146">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="e414b-147">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="e414b-147">Design meetings</span></span>

- [<span data-ttu-id="e414b-148">LDM: 2017-10-18</span><span class="sxs-lookup"><span data-stu-id="e414b-148">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="e414b-149">LDM: 2018-05-21</span><span class="sxs-lookup"><span data-stu-id="e414b-149">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="e414b-150">LDM: 2018-06-25</span><span class="sxs-lookup"><span data-stu-id="e414b-150">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="e414b-151">LDM: 2018-08-22</span><span class="sxs-lookup"><span data-stu-id="e414b-151">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="e414b-152">LDM: 2018-10-17</span><span class="sxs-lookup"><span data-stu-id="e414b-152">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [<span data-ttu-id="e414b-153">LDM: 2020-03-25</span><span class="sxs-lookup"><span data-stu-id="e414b-153">LDM-2020-03-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
