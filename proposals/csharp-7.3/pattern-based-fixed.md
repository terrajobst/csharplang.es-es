---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483635"
---
# <a name="pattern-based-fixed-statement"></a><span data-ttu-id="4bbd8-101">Instrucción `fixed` basada en patrones</span><span class="sxs-lookup"><span data-stu-id="4bbd8-101">Pattern-based `fixed` statement</span></span>

## <a name="summary"></a><span data-ttu-id="4bbd8-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="4bbd8-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="4bbd8-103">Introduzca un patrón que permita a los tipos participar en `fixed` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-103">Introduce a pattern that would allow types to participate in `fixed` statements.</span></span> 

## <a name="motivation"></a><span data-ttu-id="4bbd8-104">Motivación</span><span class="sxs-lookup"><span data-stu-id="4bbd8-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="4bbd8-105">El lenguaje proporciona un mecanismo para anclar datos administrados y obtener un puntero nativo al búfer subyacente.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-105">The language provides a mechanism for pinning managed data and obtain a native pointer to the underlying buffer.</span></span>

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

<span data-ttu-id="4bbd8-106">El conjunto de tipos que pueden participar en `fixed` se codifica y se limita a las matrices y `System.String`.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-106">The set of types that can participate in `fixed` is hardcoded and limited to arrays and `System.String`.</span></span> <span data-ttu-id="4bbd8-107">Los tipos "especiales" de codificar no se escalan cuando se introducen nuevos primitivos como `ImmutableArray<T>`, `Span<T>``Utf8String`.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-107">Hardcoding "special" types does not scale when new primitives such as `ImmutableArray<T>`, `Span<T>`, `Utf8String` are introduced.</span></span> 

<span data-ttu-id="4bbd8-108">Además, la solución actual para `System.String` se basa en una API bastante rígida.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-108">In addition, the current solution for `System.String` relies on a fairly rigid API.</span></span> <span data-ttu-id="4bbd8-109">La forma de la API implica que `System.String` es un objeto contiguo que incrusta los datos codificados en UTF16 en un desplazamiento fijo del encabezado de objeto.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-109">The shape of the API implies that `System.String` is a contiguous object that embeds UTF16 encoded data at a fixed offset from the object header.</span></span> <span data-ttu-id="4bbd8-110">Este enfoque se ha encontrado problemático en varias propuestas que podrían requerir cambios en el diseño subyacente.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-110">Such approach has been found problematic in several proposals that could require changes to the underlying layout.</span></span> <span data-ttu-id="4bbd8-111">Sería conveniente poder cambiar a algo más flexible que desacople `System.String` objeto de su representación interna con el fin de interoperabilidad no administrada.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-111">It would be desirable to be able to switch to something more flexible that decouples `System.String` object from its internal representation for the purpose of unmanaged interop.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="4bbd8-112">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="4bbd8-112">Detailed design</span></span>
[design]: #detailed-design

## <a name="pattern"></a><span data-ttu-id="4bbd8-113">*Patrón*</span><span class="sxs-lookup"><span data-stu-id="4bbd8-113">*Pattern*</span></span> ##
<span data-ttu-id="4bbd8-114">Un "fijo" viable basado en patrones debe:</span><span class="sxs-lookup"><span data-stu-id="4bbd8-114">A viable pattern-based “fixed” need to:</span></span>
-   <span data-ttu-id="4bbd8-115">Proporcione las referencias administradas para anclar la instancia e inicializar el puntero (preferiblemente es la misma referencia).</span><span class="sxs-lookup"><span data-stu-id="4bbd8-115">Provide the managed references to pin the instance and to initialize the pointer (preferably this is the same reference)</span></span>
-   <span data-ttu-id="4bbd8-116">Transmite de forma inequívoca el tipo del elemento no administrado (es decir, "Char" para "String").</span><span class="sxs-lookup"><span data-stu-id="4bbd8-116">Convey unambiguously the type of the unmanaged element   (i.e. “char” for “string”)</span></span>
-   <span data-ttu-id="4bbd8-117">Prescribe el comportamiento en caso "vacío" cuando no hay nada al que hacer referencia.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-117">Prescribe the behavior in "empty" case when there is nothing to refer to.</span></span> 
-   <span data-ttu-id="4bbd8-118">No debe enviar a los creadores de la API hacia las decisiones de diseño que perjudiquen el uso del tipo fuera de `fixed`.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-118">Should not push API authors toward design decisions that hurt the use of the type outside of `fixed`.</span></span>

<span data-ttu-id="4bbd8-119">Creo que lo anterior se podría satisfacer reconociendo un miembro de devolución especial con nombre: `ref [readonly] T GetPinnableReference()`.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-119">I think the above could be satisfied by recognizing a specially named ref-returning member: `ref [readonly] T GetPinnableReference()`.</span></span>

<span data-ttu-id="4bbd8-120">Para que la instrucción `fixed` la use, deben cumplirse las siguientes condiciones:</span><span class="sxs-lookup"><span data-stu-id="4bbd8-120">In order to be used by the `fixed` statement the following conditions must be met:</span></span>

1. <span data-ttu-id="4bbd8-121">Solo se proporciona un miembro de este tipo para un tipo.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-121">There is only one such member provided for a type.</span></span>
1. <span data-ttu-id="4bbd8-122">Devuelve por `ref` o `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-122">Returns by `ref` or `ref readonly`.</span></span> <span data-ttu-id="4bbd8-123">(`readonly` se permite para que los autores de tipos inmutables y de solo lectura puedan implementar el patrón sin agregar la API grabable que podría usarse en código seguro)</span><span class="sxs-lookup"><span data-stu-id="4bbd8-123">(`readonly` is permitted so that authors of immutable/readonly types could implement the pattern without adding writeable API that could be used in safe code)</span></span>
1. <span data-ttu-id="4bbd8-124">T es un tipo no administrado.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-124">T is an unmanaged type.</span></span>
<span data-ttu-id="4bbd8-125">(dado que `T*` se convierte en el tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-125">(since `T*` becomes the pointer type.</span></span> <span data-ttu-id="4bbd8-126">La restricción se expande de forma natural si se expande la noción de "no administrada")</span><span class="sxs-lookup"><span data-stu-id="4bbd8-126">The restriction will naturally expand if/when the notion of "unmanaged" is expanded)</span></span>
1. <span data-ttu-id="4bbd8-127">Devuelve `nullptr` administrados cuando no hay datos para anclar, probablemente la manera más barata de transmitir Emptiness.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-127">Returns managed `nullptr` when there is no data to pin – probably the cheapest way to convey emptiness.</span></span>
<span data-ttu-id="4bbd8-128">(tenga en cuenta que "" cadena devuelve una referencia a "\ 0", ya que las cadenas terminan en null)</span><span class="sxs-lookup"><span data-stu-id="4bbd8-128">(note that “” string returns a ref to '\0' since strings are null-terminated)</span></span>

<span data-ttu-id="4bbd8-129">Como alternativa, en el `#3` podemos permitir que el resultado en casos vacíos sea no definido o específico de la implementación.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-129">Alternatively for the `#3` we can allow the result in empty cases be undefined or implementation-specific.</span></span> <span data-ttu-id="4bbd8-130">Sin embargo, eso puede hacer que la API sea más peligrosa y propenso a abusos y cargas de compatibilidad no intencionadas.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-130">That, however, may make the API more dangerous and prone to abuse and unintended compatibility burdens.</span></span> 

## <a name="translation"></a><span data-ttu-id="4bbd8-131">*Traducción*</span><span class="sxs-lookup"><span data-stu-id="4bbd8-131">*Translation*</span></span> ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

<span data-ttu-id="4bbd8-132">se convierte en el pseudocódigo siguiente (no todos los elementos C#que se van a expresar en)</span><span class="sxs-lookup"><span data-stu-id="4bbd8-132">becomes the following pseudocode (not all expressible in C#)</span></span>

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a><span data-ttu-id="4bbd8-133">Desventajas</span><span class="sxs-lookup"><span data-stu-id="4bbd8-133">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="4bbd8-134">GetPinnableReference está pensado para usarse solo en `fixed`, pero nada impide su uso en código seguro, por lo que el implementador debe tenerlo en cuenta.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-134">GetPinnableReference is intended to be used only in `fixed`, but nothing prevents its use in safe code, so implementor must keep that in mind.</span></span>

## <a name="alternatives"></a><span data-ttu-id="4bbd8-135">Alternativas</span><span class="sxs-lookup"><span data-stu-id="4bbd8-135">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="4bbd8-136">Los usuarios pueden introducir GetPinnableReference o un miembro similar y usarlo como</span><span class="sxs-lookup"><span data-stu-id="4bbd8-136">Users can introduce GetPinnableReference or similar member and use it as</span></span>
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

<span data-ttu-id="4bbd8-137">No hay ninguna solución para `System.String` si se desea una solución alternativa.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-137">There is no solution for `System.String` if alternative solution is desired.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="4bbd8-138">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="4bbd8-138">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="4bbd8-139">[] Comportamiento en estado "vacío".</span><span class="sxs-lookup"><span data-stu-id="4bbd8-139">[ ] Behavior in "empty" state.</span></span><span data-ttu-id="4bbd8-140">¿ - `nullptr` o `undefined`?</span><span class="sxs-lookup"><span data-stu-id="4bbd8-140"> - `nullptr` or `undefined` ?</span></span> 
- <span data-ttu-id="4bbd8-141">[] ¿Deben tenerse en cuenta los métodos de extensión?</span><span class="sxs-lookup"><span data-stu-id="4bbd8-141">[ ] Should the extension methods be considered ?</span></span> 
- <span data-ttu-id="4bbd8-142">[] Si se detecta un patrón en `System.String`, ¿debe ganarse?</span><span class="sxs-lookup"><span data-stu-id="4bbd8-142">[ ] If a pattern is detected on `System.String`, should it win over ?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="4bbd8-143">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="4bbd8-143">Design meetings</span></span>

<span data-ttu-id="4bbd8-144">Ninguno todavía.</span><span class="sxs-lookup"><span data-stu-id="4bbd8-144">None yet.</span></span> 
