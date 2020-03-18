---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483485"
---
# <a name="readonly-locals-and-parameters"></a><span data-ttu-id="0891d-101">variables locales y parámetros de solo lectura</span><span class="sxs-lookup"><span data-stu-id="0891d-101">readonly locals and parameters</span></span>

* <span data-ttu-id="0891d-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="0891d-102">[x] Proposed</span></span>
* <span data-ttu-id="0891d-103">[] Prototipo</span><span class="sxs-lookup"><span data-stu-id="0891d-103">[ ] Prototype</span></span>
* <span data-ttu-id="0891d-104">[] Implementación</span><span class="sxs-lookup"><span data-stu-id="0891d-104">[ ] Implementation</span></span>
* <span data-ttu-id="0891d-105">[] Especificación</span><span class="sxs-lookup"><span data-stu-id="0891d-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="0891d-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="0891d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="0891d-107">Permita anotar variables locales y parámetros como de solo lectura para evitar la mutación superficial de esas variables locales y parámetros.</span><span class="sxs-lookup"><span data-stu-id="0891d-107">Allow locals and parameters to be annotated as readonly in order to prevent shallow mutation of those locals and parameters.</span></span>

## <a name="motivation"></a><span data-ttu-id="0891d-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="0891d-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="0891d-109">En la actualidad, la palabra clave `readonly` se puede aplicar a los campos; Esto tiene el efecto de asegurarse de que un campo solo se puede escribir en durante la construcción (construcción estática en el caso de un campo estático o una construcción de instancia en el caso de un campo de instancia), lo que ayuda a los desarrolladores a evitar errores al sobrescribir accidentalmente el estado que no debe modificarse.</span><span class="sxs-lookup"><span data-stu-id="0891d-109">Today, the `readonly` keyword can be applied to fields; this has the effect of ensuring that a field can only be written to during construction (static construction in the case of a static field, or instance construction in the case of an instance field), which helps developers avoid mistakes by accidentally overwriting state which should not be modified.</span></span> <span data-ttu-id="0891d-110">Pero los campos no son los únicos lugares en los que los desarrolladores quieren asegurarse de que los valores no están mutados.</span><span class="sxs-lookup"><span data-stu-id="0891d-110">But fields aren't the only places developers want to ensure that values aren't mutated.</span></span> <span data-ttu-id="0891d-111">En concreto, es habitual crear una variable local para almacenar el estado temporal y actualizar accidentalmente ese estado temporal puede producir cálculos erróneos y otros errores, especialmente cuando estas "variables locales" se capturan en lambdas, momento en el que se elevan a campos, pero actualmente no hay ninguna manera de marcar tales campos de elevación como `readonly`.</span><span class="sxs-lookup"><span data-stu-id="0891d-111">In particular, it's common to create a local variable to store temporary state, and accidentally updating that temporary state can result in erroneous calculations and other such bugs, especially when such "locals" are captured in lambdas, at which point they are lifted to fields, but there's no way today to mark such lifted fields as `readonly`.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="0891d-112">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="0891d-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="0891d-113">Las variables locales también se pueden anotar como `readonly`, con el compilador que garantiza que solo se establecen en el momento de la declaración C# (ciertas variables locales en ya son implícitamente de solo lectura, como la variable de iteración en un bucle ' foreach ' o la variable usada en un bloque ' Using ', pero actualmente un desarrollador no tiene la capacidad de marcar otras variables locales como `readonly`)</span><span class="sxs-lookup"><span data-stu-id="0891d-113">Locals will be annotatable as `readonly` as well, with the compiler ensuring that they're only set at the time of declaration (certain locals in C# are already implicitly readonly, such as the iteration variable in a 'foreach' loop or the used variable in a 'using' block, but currently a developer has no ability to mark other locals as `readonly`).</span></span> <span data-ttu-id="0891d-114">`readonly` variables locales deben tener un inicializador:</span><span class="sxs-lookup"><span data-stu-id="0891d-114">Such `readonly` locals must have an initializer:</span></span>

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="0891d-115">Y como abreviatura para `readonly var`, se puede usar la palabra clave contextual existente `let`, por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="0891d-115">And as shorthand for `readonly var`, the existing contextual keyword `let` may be used, e.g.</span></span>

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

<span data-ttu-id="0891d-116">No hay ninguna restricción especial para lo que puede ser el inicializador y puede ser cualquier elemento que sea válido actualmente como inicializador para variables locales, por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="0891d-116">There are no special constraints for what the initializer can be, and can be anything currently valid as an initializer for locals, e.g.</span></span>

```csharp
readonly T data = arg1 ?? arg2;
```

<span data-ttu-id="0891d-117">`readonly` en variables locales es especialmente útil cuando se trabaja con expresiones lambda y cierres.</span><span class="sxs-lookup"><span data-stu-id="0891d-117">`readonly` on locals is particularly valuable when working with lambdas and closures.</span></span> <span data-ttu-id="0891d-118">Cuando un método anónimo o una expresión lambda accede al estado local desde el ámbito de inclusión, el compilador, que se representa mediante una "clase de presentación", lo captura en una clausura.</span><span class="sxs-lookup"><span data-stu-id="0891d-118">When an anonymous method or lambda accesses local state from the enclosing scope, that state is captured into a closure by the compiler, which is represented by a "display class."</span></span>  <span data-ttu-id="0891d-119">Cada "local" que se captura es un campo de esta clase, aunque el compilador está generando este campo en su nombre, no tiene la oportunidad de anotarlo como `readonly` para evitar que la expresión lambda escriba erróneamente en el "local" (entre comillas porque realmente no es local, al menos no en el MSIL resultante).</span><span class="sxs-lookup"><span data-stu-id="0891d-119">Each "local" that's captured is a field in this class, yet because the compiler is generating this field on your behalf, you have no opportunity to annotate it as `readonly` in order to prevent the lambda from erroneously writing to the "local" (in quotes because it's really not a local, at least not in the resulting MSIL).</span></span> <span data-ttu-id="0891d-120">Con `readonly` variables locales, el compilador puede impedir que la expresión lambda se escriba en una variable local, lo que es especialmente útil en escenarios que implican multithreading, donde una escritura errónea podría producir un error de simultaneidad peligroso pero poco frecuente y difícil de encontrar.</span><span class="sxs-lookup"><span data-stu-id="0891d-120">With `readonly` locals, the compiler can prevent the lambda from writing to local, which is particularly valuable in scenarios involving multithreading where an erroneous write could result in a dangerous but rare and hard-to-find concurrency bug.</span></span>

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

<span data-ttu-id="0891d-121">Como forma especial de local, los parámetros también se pueden anotar como `readonly`.</span><span class="sxs-lookup"><span data-stu-id="0891d-121">As a special form of local, parameters will also be annotatable as `readonly`.</span></span> <span data-ttu-id="0891d-122">Esto no tendría ningún efecto en lo que el llamador del método puede pasar al parámetro (de la misma forma que no hay ninguna restricción sobre los valores que se pueden almacenar en un campo de `readonly`), pero como con cualquier `readonly` local, el compilador prohibiría que el código escribiera en el parámetro después de la declaración, lo que significa que el cuerpo del método no puede escribir en el parámetro</span><span class="sxs-lookup"><span data-stu-id="0891d-122">This would have no effect on what the caller of the method is able to pass to the parameter (just as there's no constraint on what values may be stored into a `readonly` field), but as with any `readonly` local, the compiler would prohibit code from writing to the parameter after declaration, which means the body of the method is prohibited from writing to the parameter.</span></span>

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

<span data-ttu-id="0891d-123">`readonly` parámetros no afectan a la firma o los metadatos emitidos por el compilador para ese método y simplemente afectan a la forma en que el compilador controla la compilación del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="0891d-123">`readonly` parameters do not affect the signature/metadata emitted by the compiler for that method, and simply affect how the compiler handles the compilation of the method's body.</span></span> <span data-ttu-id="0891d-124">Por lo tanto, por ejemplo, un método virtual base podría tener un parámetro `readonly`, y ese parámetro podría ser grabable en una invalidación.</span><span class="sxs-lookup"><span data-stu-id="0891d-124">Thus, for example, a base virtual method could have a `readonly` parameter, and that parameter could be writable in an override.</span></span>

<span data-ttu-id="0891d-125">Al igual que con los campos, `readonly` para variables locales y parámetros es superficial, lo que afecta a la ubicación de almacenamiento, pero no afecta de forma transitiva al gráfico de objetos.</span><span class="sxs-lookup"><span data-stu-id="0891d-125">As with fields, `readonly` for locals and parameters is shallow, affecting the storage location but not transitively affecting the object graph.</span></span> <span data-ttu-id="0891d-126">Sin embargo, al igual que con los campos, llamar a un método en un `readonly` struct local/parámetro realizará realmente una copia de la estructura y llamará al método en la copia, con el fin de evitar la mutación interna de `this`.</span><span class="sxs-lookup"><span data-stu-id="0891d-126">However, also as with fields, calling a method on a `readonly` local/parameter struct will actually make a copy of the struct and call the method on the copy, in order to avoid internal mutation of `this`.</span></span>

<span data-ttu-id="0891d-127">`readonly` variables locales y parámetros no se pueden pasar como argumentos `ref` o `out`, a menos que también se admita `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="0891d-127">`readonly` locals and parameters can't be passed as `ref` or `out` arguments, unless/until `ref readonly` is also supported.</span></span>

## <a name="alternatives"></a><span data-ttu-id="0891d-128">Alternativas</span><span class="sxs-lookup"><span data-stu-id="0891d-128">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="0891d-129">`val` podría usarse como una forma abreviada alternativa a `let`.</span><span class="sxs-lookup"><span data-stu-id="0891d-129">`val` could be used as an alternative shorthand to `let`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="0891d-130">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="0891d-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="0891d-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: he dejado la pregunta sobre cómo administrar `ref readonly` como independiente de esta propuesta.</span><span class="sxs-lookup"><span data-stu-id="0891d-131">`readonly ref` / `ref readonly` / `readonly ref readonly`: I've left the question of how to handle `ref readonly` as separate from this proposal.</span></span>
- <span data-ttu-id="0891d-132">Esta propuesta no aborda Structs de solo lectura ni tipos inmutables.</span><span class="sxs-lookup"><span data-stu-id="0891d-132">This proposal does not tackle readonly structs / immutable types.</span></span> <span data-ttu-id="0891d-133">Se deja para una propuesta independiente.</span><span class="sxs-lookup"><span data-stu-id="0891d-133">That is left for a separate proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="0891d-134">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="0891d-134">Design meetings</span></span>

- <span data-ttu-id="0891d-135">Se describe brevemente el 21 de enero de 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span><span class="sxs-lookup"><span data-stu-id="0891d-135">Briefly discussed on Jan 21, 2015 (<https://github.com/dotnet/roslyn/issues/98>)</span></span>
