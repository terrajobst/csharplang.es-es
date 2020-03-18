---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483479"
---
# <a name="static-delegates"></a><span data-ttu-id="afbf8-101">Delegados estáticos</span><span class="sxs-lookup"><span data-stu-id="afbf8-101">Static Delegates</span></span>

* <span data-ttu-id="afbf8-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="afbf8-102">[x] Proposed</span></span>
* <span data-ttu-id="afbf8-103">[] Prototipo: no iniciado</span><span class="sxs-lookup"><span data-stu-id="afbf8-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="afbf8-104">[] Implementación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="afbf8-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="afbf8-105">[] Especificación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="afbf8-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="afbf8-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="afbf8-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="afbf8-107">Proporcionar una funcionalidad de devolución de llamada ligera y de uso C# general al lenguaje.</span><span class="sxs-lookup"><span data-stu-id="afbf8-107">Provide a general-purpose, lightweight callback capability to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="afbf8-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="afbf8-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="afbf8-109">Hoy en día, los usuarios tienen la capacidad de crear devoluciones de llamada mediante el tipo de `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="afbf8-109">Today, users have the ability to create callbacks using the `System.Delegate` type.</span></span> <span data-ttu-id="afbf8-110">Sin embargo, son bastante pesadas (como requerir una asignación de montón y siempre tener el control para encadenar las devoluciones de llamada juntas).</span><span class="sxs-lookup"><span data-stu-id="afbf8-110">However, these are fairly heavyweight (such as requiring a heap allocation and always having handling for chaining callbacks together).</span></span>

<span data-ttu-id="afbf8-111">Además, `System.Delegate` no proporciona la mejor interoperabilidad con punteros de función no administrados, es decir, debido a que no se pueden representar como bits/bytes y requerir la serialización cada vez que atraviesa el límite administrado o no administrado.</span><span class="sxs-lookup"><span data-stu-id="afbf8-111">Additionally, `System.Delegate` does not provide the best interop with unmanaged function pointers, namely due being non-blittable and requiring marshalling anytime it crosses the managed/unmanaged boundary.</span></span>

<span data-ttu-id="afbf8-112">Con unos pocos ajustes menores, podríamos proporcionar un nuevo tipo de delegado que sea ligero, de uso general y de interoperabilidad con código nativo.</span><span class="sxs-lookup"><span data-stu-id="afbf8-112">With a few minor tweaks, we could provide a new type of delegate that is lightweight, general-purpose, and interops well with native code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="afbf8-113">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="afbf8-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="afbf8-114">Uno declararía un delegado estático mediante lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="afbf8-114">One would declare a static delegate via the following:</span></span>

```C#
static delegate int Func()
```

<span data-ttu-id="afbf8-115">También podría atribuir la declaración con algo similar a `System.Runtime.InteropServices.UnmanagedFunctionPointer` de modo que se pueda controlar la Convención de llamada, la serialización de cadenas y el comportamiento del último error.</span><span class="sxs-lookup"><span data-stu-id="afbf8-115">One could additionally attribute the declaration with something similar to `System.Runtime.InteropServices.UnmanagedFunctionPointer` so that the calling convention, string marshalling, and set last error behavior can be controlled.</span></span> <span data-ttu-id="afbf8-116">Nota: el uso de `System.Runtime.InteropServices.UnmanagedFunctionPointer` mismo no funcionará, ya que solo se puede usar en los delegados.</span><span class="sxs-lookup"><span data-stu-id="afbf8-116">NOTE: Using `System.Runtime.InteropServices.UnmanagedFunctionPointer` itself will not work, as it is only usable on Delegates.</span></span>

<span data-ttu-id="afbf8-117">El compilador traducirá la declaración en una representación interna similar a la siguiente</span><span class="sxs-lookup"><span data-stu-id="afbf8-117">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

<span data-ttu-id="afbf8-118">Es decir, se representa internamente mediante un struct que tiene un único miembro de tipo `IntPtr` (este tipo de estructura es un elemento que no se tiene en cuenta y no tiene ninguna asignación de montón):</span><span class="sxs-lookup"><span data-stu-id="afbf8-118">That is to say, it is internally represented by a struct that has a single member of type `IntPtr` (such a struct is blittable and does not incur any heap allocations):</span></span>
* <span data-ttu-id="afbf8-119">El miembro contiene la dirección de la función que va a ser la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="afbf8-119">The member contains the address of the function that is to be the callback.</span></span>
* <span data-ttu-id="afbf8-120">El tipo declara un método que coincide con la firma del método de la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="afbf8-120">The type declares a method matching the method signature of the callback.</span></span>
* <span data-ttu-id="afbf8-121">El nombre del struct no debe ser constructor por el usuario (como hacemos con otras estructuras de respaldo generadas internamente).</span><span class="sxs-lookup"><span data-stu-id="afbf8-121">The name of the struct should not be user-constructible (as we do with other internally generated backing structures).</span></span>
 * <span data-ttu-id="afbf8-122">Por ejemplo: los búferes de tamaño fijo generan un struct con un nombre con el formato `<name>e__FixedBuffer` (`<` y `>` forman parte del identificador y hacen que el identificador no se pueda construir en C#, pero todavía se puede seguir usando en Il).</span><span class="sxs-lookup"><span data-stu-id="afbf8-122">For example: fixed size buffers generate a struct with a name in the format of `<name>e__FixedBuffer` (`<` and `>` are part of the identifier and make the identifier not constructible in C#, but still useable in IL).</span></span>
* <span data-ttu-id="afbf8-123">El nombre de la declaración del método debe ser un nombre conocido que se utiliza en todos los tipos de delegado estáticos (esto permite que el compilador conozca el nombre que se debe buscar al determinar la firma).</span><span class="sxs-lookup"><span data-stu-id="afbf8-123">The name of the method declaration should be a well known name used across all static delegate types (this allows the compiler to know the name to look for when determining the signature).</span></span>

<span data-ttu-id="afbf8-124">El valor del delegado estático solo se puede enlazar a un método estático que coincida con la firma de la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="afbf8-124">The value of the static delegate can only be bound to a static method that matches the signature of the callback.</span></span>

<span data-ttu-id="afbf8-125">No se admite el encadenamiento de devoluciones de llamada juntas.</span><span class="sxs-lookup"><span data-stu-id="afbf8-125">Chaining callbacks together is not supported.</span></span>

<span data-ttu-id="afbf8-126">La invocación de la devolución de llamada la implementaría la instrucción `calli`.</span><span class="sxs-lookup"><span data-stu-id="afbf8-126">Invocation of the callback would be implemented by the `calli` instruction.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="afbf8-127">Desventajas</span><span class="sxs-lookup"><span data-stu-id="afbf8-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="afbf8-128">Los delegados estáticos no funcionarían con las API existentes que usan delegados normales (es necesario ajustar dicho delegado estático en un delegado normal de la misma firma).</span><span class="sxs-lookup"><span data-stu-id="afbf8-128">Static Delegates would not work with existing APIs that use regular delegates (one would need to wrap said static delegate in a regular delegate of the same signature).</span></span>
* <span data-ttu-id="afbf8-129">Dado que `System.Delegate` se representa internamente como un conjunto de campos de `object` y `IntPtr` (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), sería posible permitir la conversión implícita de un delegado estático a una `System.Delegate` que tenga una firma de método coincidente.</span><span class="sxs-lookup"><span data-stu-id="afbf8-129">Given that `System.Delegate` is represented internally as a set of `object` and `IntPtr` fields (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), it would be possible to allow implicit conversion of a static delegate to a `System.Delegate` that has a matching method signature.</span></span> <span data-ttu-id="afbf8-130">También sería posible proporcionar una conversión explícita en la dirección opuesta, siempre que el `System.Delegate` se ajuste a todos los requisitos de ser un delegado estático.</span><span class="sxs-lookup"><span data-stu-id="afbf8-130">It would also be possible to provide an explicit conversion in the opposite direction, provided the `System.Delegate` conformed to all the requirements of being a static delegate.</span></span>

<span data-ttu-id="afbf8-131">Se necesitará trabajo adicional para que el delegado estático pueda usarse fácilmente en el marco principal.</span><span class="sxs-lookup"><span data-stu-id="afbf8-131">Additional work would be needed to make Static Delegate readily usable in the core framework.</span></span>

## <a name="alternatives"></a><span data-ttu-id="afbf8-132">Alternativas</span><span class="sxs-lookup"><span data-stu-id="afbf8-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="afbf8-133">TBD</span><span class="sxs-lookup"><span data-stu-id="afbf8-133">TBD</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="afbf8-134">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="afbf8-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="afbf8-135">¿Qué partes del diseño siguen siendo TBD?</span><span class="sxs-lookup"><span data-stu-id="afbf8-135">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="afbf8-136">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="afbf8-136">Design meetings</span></span>

<span data-ttu-id="afbf8-137">Vínculo a las notas de diseño que afectan a esta propuesta y describa en una oración cada uno de los cambios que han producido.</span><span class="sxs-lookup"><span data-stu-id="afbf8-137">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>


