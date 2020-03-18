---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483593"
---
# <a name="stackalloc-array-initializers"></a><span data-ttu-id="17f86-101">Inicializadores de matriz stackalloc</span><span class="sxs-lookup"><span data-stu-id="17f86-101">Stackalloc array initializers</span></span>

## <a name="summary"></a><span data-ttu-id="17f86-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="17f86-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="17f86-103">Permite usar la sintaxis del inicializador de matriz con `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="17f86-103">Allow array initializer syntax to be used with `stackalloc`.</span></span>

## <a name="motivation"></a><span data-ttu-id="17f86-104">Motivación</span><span class="sxs-lookup"><span data-stu-id="17f86-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="17f86-105">Las matrices normales pueden inicializar sus elementos en el momento de su creación.</span><span class="sxs-lookup"><span data-stu-id="17f86-105">Ordinary arrays can have their elements initialized at creation time.</span></span> <span data-ttu-id="17f86-106">Parece razonable permitirlo en `stackalloc` caso.</span><span class="sxs-lookup"><span data-stu-id="17f86-106">It seems reasonable to allow that in `stackalloc` case.</span></span>

<span data-ttu-id="17f86-107">La pregunta de por qué no se permite esta sintaxis con `stackalloc` se produce con bastante frecuencia.</span><span class="sxs-lookup"><span data-stu-id="17f86-107">The question of why such syntax is not allowed with `stackalloc` arises fairly frequently.</span></span>  
<span data-ttu-id="17f86-108">Consulte, por ejemplo, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span><span class="sxs-lookup"><span data-stu-id="17f86-108">See, for example, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="17f86-109">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="17f86-109">Detailed design</span></span>

<span data-ttu-id="17f86-110">Las matrices ordinarias se pueden crear mediante la sintaxis siguiente:</span><span class="sxs-lookup"><span data-stu-id="17f86-110">Ordinary arrays can be created through the following syntax:</span></span>

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

<span data-ttu-id="17f86-111">Deberíamos permitir la creación de matrices asignadas de pila mediante:</span><span class="sxs-lookup"><span data-stu-id="17f86-111">We should allow stack allocated arrays be created through:</span></span>  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

<span data-ttu-id="17f86-112">La semántica de todos los casos es aproximadamente la misma que con las matrices.</span><span class="sxs-lookup"><span data-stu-id="17f86-112">The semantics of all cases is roughly the same as with arrays.</span></span>  
<span data-ttu-id="17f86-113">Por ejemplo: en el último caso, el tipo de elemento se deduce del inicializador y debe ser un tipo "no administrado".</span><span class="sxs-lookup"><span data-stu-id="17f86-113">For example: in the last case the element type is inferred from the initializer and must be an "unmanaged" type.</span></span>

<span data-ttu-id="17f86-114">Nota: la característica no depende de que el destino sea un `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="17f86-114">NOTE: the feature is not dependent on the target being a `Span<T>`.</span></span> <span data-ttu-id="17f86-115">Es tan aplicable en `T*` caso, por lo que no parece razonable que lo predicase en `Span<T>` caso.</span><span class="sxs-lookup"><span data-stu-id="17f86-115">It is just as applicable in `T*` case, so it does not seem reasonable to predicate it on `Span<T>` case.</span></span>  

## <a name="translation"></a><span data-ttu-id="17f86-116">Traducción</span><span class="sxs-lookup"><span data-stu-id="17f86-116">Translation</span></span>

<span data-ttu-id="17f86-117">La implementación de Naive podría simplemente inicializar la matriz justo después de la creación a través de una serie de asignaciones de elementos.</span><span class="sxs-lookup"><span data-stu-id="17f86-117">The naive implementation could just initialize the array right after creation through a series of element-wise assignments.</span></span>  

<span data-ttu-id="17f86-118">Del mismo modo que con las matrices, puede ser posible y deseable detectar los casos en los que todos o la mayoría de los elementos son tipos que pueden transferirse en exceso de bits y usar técnicas más eficaces copiando el estado creado previamente de todos los elementos constantes.</span><span class="sxs-lookup"><span data-stu-id="17f86-118">Similarly to the case with arrays, it might be possible and desirable to detect cases where all or most of the elements are blittable types and use more efficient techniques by copying over the pre-created state of all the constant elements.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="17f86-119">Desventajas</span><span class="sxs-lookup"><span data-stu-id="17f86-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

## <a name="alternatives"></a><span data-ttu-id="17f86-120">Alternativas</span><span class="sxs-lookup"><span data-stu-id="17f86-120">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="17f86-121">Esta es una característica útil.</span><span class="sxs-lookup"><span data-stu-id="17f86-121">This is a convenience feature.</span></span> <span data-ttu-id="17f86-122">Solo es posible hacer nada.</span><span class="sxs-lookup"><span data-stu-id="17f86-122">It is possible to just do nothing.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="17f86-123">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="17f86-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="17f86-124">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="17f86-124">Design meetings</span></span>

<span data-ttu-id="17f86-125">Ninguno todavía.</span><span class="sxs-lookup"><span data-stu-id="17f86-125">None yet.</span></span> 
