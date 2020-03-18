---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483575"
---
# <a name="fixed-sized-buffers"></a><span data-ttu-id="26024-101">Búferes de tamaño fijo</span><span class="sxs-lookup"><span data-stu-id="26024-101">Fixed Sized Buffers</span></span>

* <span data-ttu-id="26024-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="26024-102">[x] Proposed</span></span>
* <span data-ttu-id="26024-103">[] Prototipo: no iniciado</span><span class="sxs-lookup"><span data-stu-id="26024-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="26024-104">[] Implementación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="26024-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="26024-105">[] Especificación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="26024-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="26024-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="26024-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="26024-107">Proporcionan un mecanismo de uso general y seguro para declarar búferes de tamaño fijo en el C# lenguaje.</span><span class="sxs-lookup"><span data-stu-id="26024-107">Provide a general-purpose and safe mechanism for declaring fixed sized buffers to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="26024-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="26024-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="26024-109">Hoy en día, los usuarios tienen la capacidad de crear búferes de tamaño fijo en un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="26024-109">Today, users have the ability to create fixed-sized buffers in an unsafe-context.</span></span> <span data-ttu-id="26024-110">Sin embargo, esto requiere que el usuario trate con punteros, realice comprobaciones de límites manualmente y solo admita un conjunto limitado de tipos (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`y `double`).</span><span class="sxs-lookup"><span data-stu-id="26024-110">However, this requires the user to deal with pointers, manually perform bounds checks, and only supports a limited set of types (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`, and `double`).</span></span>

<span data-ttu-id="26024-111">La queja más común es que los búferes de tamaño fijo no se pueden indizar en código seguro.</span><span class="sxs-lookup"><span data-stu-id="26024-111">The most common complaint is that fixed-size buffers cannot be indexed in safe code.</span></span> <span data-ttu-id="26024-112">La imposibilidad de usar más tipos es el segundo.</span><span class="sxs-lookup"><span data-stu-id="26024-112">Inability to use more types is the second.</span></span>

<span data-ttu-id="26024-113">Con unos pocos ajustes menores, podríamos proporcionar búferes de tamaño fijo de uso general que admitan cualquier tipo, se puede usar en un contexto seguro y hacer que se realice una comprobación automática de los límites.</span><span class="sxs-lookup"><span data-stu-id="26024-113">With a few minor tweaks, we could provide general-purpose fixed-sized buffers which support any type, can be used in a safe context, and have automatic bounds checking performed.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="26024-114">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="26024-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="26024-115">Uno declararía un búfer de tamaño fijo seguro a través de lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="26024-115">One would declare a safe fixed-sized buffer via the following:</span></span>

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

<span data-ttu-id="26024-116">El compilador traducirá la declaración en una representación interna similar a la siguiente</span><span class="sxs-lookup"><span data-stu-id="26024-116">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

<span data-ttu-id="26024-117">Dado que estos búferes de tamaño fijo ya no requieren el uso de `fixed`, tiene sentido permitir cualquier tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="26024-117">Since such fixed-sized buffers no longer require use of `fixed`, it makes sense to allow any element type.</span></span>  

> <span data-ttu-id="26024-118">Nota: se seguirá admitiendo `fixed`, pero solo si el tipo de elemento es `blittable`</span><span class="sxs-lookup"><span data-stu-id="26024-118">NOTE: `fixed` will still be supported, but only if the element type is `blittable`</span></span>

## <a name="drawbacks"></a><span data-ttu-id="26024-119">Desventajas</span><span class="sxs-lookup"><span data-stu-id="26024-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

* <span data-ttu-id="26024-120">Podría haber algunos desafíos con la compatibilidad con versiones anteriores, pero dado que los búferes de tamaño fijo existentes solo funcionan con una selección de tipos primitivos, es posible que el compilador continúe "Just-Working" si el usuario trata el búfer fijo como un puntero.</span><span class="sxs-lookup"><span data-stu-id="26024-120">There could be some challenges with backwards compatibility, but given that the existing fixed-sized buffers only work with a selection of primitive types, it should be possible for the compiler to continue "just-working" if the user treats the fixed-buffer as a pointer.</span></span>
* <span data-ttu-id="26024-121">Las construcciones incompatibles pueden necesitar usar una codificación de `v2` ligeramente diferente para ocultar los campos del compilador anterior.</span><span class="sxs-lookup"><span data-stu-id="26024-121">Incompatible constructs may need to use slightly different `v2` encoding to hide the fields from old compiler.</span></span>
* <span data-ttu-id="26024-122">El empaquetado no está bien definido en la especificación de IL para tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="26024-122">Packing is not well defined in IL spec for generic types.</span></span> <span data-ttu-id="26024-123">Aunque el enfoque debería funcionar, se aplicará un borde a un comportamiento no documentado.</span><span class="sxs-lookup"><span data-stu-id="26024-123">While the approach should work, we will be bordering on undocumented behavior.</span></span> <span data-ttu-id="26024-124">Debemos hacer esto documentado y asegurarnos de que otros JITs como mono tienen el mismo comportamiento.</span><span class="sxs-lookup"><span data-stu-id="26024-124">We should make that documented and make sure other JITs like Mono have the same behavior.</span></span>
* <span data-ttu-id="26024-125">Si se especifica un tipo independiente para cada longitud (posiblemente otro para los campos `readonly`, si se admiten), afectará a los metadatos.</span><span class="sxs-lookup"><span data-stu-id="26024-125">Specifying a separate type for every length (an possibly another for `readonly` fields, if supported) will have impact on metadata.</span></span> <span data-ttu-id="26024-126">Estará limitado por el número de matrices de distintos tamaños en la aplicación especificada.</span><span class="sxs-lookup"><span data-stu-id="26024-126">It will be bound by the number of arrays of different sizes in the given app.</span></span>
* <span data-ttu-id="26024-127">`ref` Math no es comprobable formalmente (ya que no es seguro).</span><span class="sxs-lookup"><span data-stu-id="26024-127">`ref` math is not formally verifiable (since it is unsafe).</span></span> <span data-ttu-id="26024-128">Necesitamos encontrar una manera de actualizar las reglas de comprobación para saber que nuestro uso es correcto.</span><span class="sxs-lookup"><span data-stu-id="26024-128">We will need to find a way to update verification rules to know that our use is ok.</span></span>

## <a name="alternatives"></a><span data-ttu-id="26024-129">Alternativas</span><span class="sxs-lookup"><span data-stu-id="26024-129">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="26024-130">Declare manualmente las estructuras y use código no seguro para construir indexadores.</span><span class="sxs-lookup"><span data-stu-id="26024-130">Manually declare your structures and use unsafe code to construct indexers.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="26024-131">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="26024-131">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="26024-132">¿se debe permitir `readonly`?</span><span class="sxs-lookup"><span data-stu-id="26024-132">should we allow `readonly`?</span></span>  <span data-ttu-id="26024-133">(con indizador de solo lectura)</span><span class="sxs-lookup"><span data-stu-id="26024-133">(with readonly indexer)</span></span>
- <span data-ttu-id="26024-134">¿se deben permitir los inicializadores de matriz?</span><span class="sxs-lookup"><span data-stu-id="26024-134">should we allow array initializers?</span></span>
- <span data-ttu-id="26024-135">¿es `fixed` palabra clave necesaria?</span><span class="sxs-lookup"><span data-stu-id="26024-135">is `fixed` keyword necessary?</span></span>
- <span data-ttu-id="26024-136">`foreach`?</span><span class="sxs-lookup"><span data-stu-id="26024-136">`foreach`?</span></span>
- <span data-ttu-id="26024-137">¿solo campos de instancia en Structs?</span><span class="sxs-lookup"><span data-stu-id="26024-137">only instance fields in structs?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="26024-138">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="26024-138">Design meetings</span></span>

<span data-ttu-id="26024-139">Vínculo a las notas de diseño que afectan a esta propuesta y describa en una oración cada uno de los cambios que han producido.</span><span class="sxs-lookup"><span data-stu-id="26024-139">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>