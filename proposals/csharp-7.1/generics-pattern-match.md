---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483677"
---
# <a name="pattern-matching-with-generics"></a><span data-ttu-id="3e920-101">coincidencia de patrones con genéricos</span><span class="sxs-lookup"><span data-stu-id="3e920-101">pattern-matching with generics</span></span>

* <span data-ttu-id="3e920-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="3e920-102">[x] Proposed</span></span>
* <span data-ttu-id="3e920-103">[] Prototipo:</span><span class="sxs-lookup"><span data-stu-id="3e920-103">[ ] Prototype:</span></span>
* <span data-ttu-id="3e920-104">[] Implementación:</span><span class="sxs-lookup"><span data-stu-id="3e920-104">[ ] Implementation:</span></span>
* <span data-ttu-id="3e920-105">[] Especificación:</span><span class="sxs-lookup"><span data-stu-id="3e920-105">[ ] Specification:</span></span>

## <a name="summary"></a><span data-ttu-id="3e920-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="3e920-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3e920-107">La especificación para el [operador C# as existente](../../spec/expressions.md#the-as-operator) permite que no haya conversión entre el tipo del operando y el tipo especificado cuando uno de los dos es un tipo abierto.</span><span class="sxs-lookup"><span data-stu-id="3e920-107">The specification for the [existing C# as operator](../../spec/expressions.md#the-as-operator) permits there to be no conversion between the type of the operand and the specified type when either is an open type.</span></span> <span data-ttu-id="3e920-108">Sin embargo, C# en 7 el patrón `Type identifier` requiere que haya una conversión entre el tipo de la entrada y el tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="3e920-108">However, in C# 7 the `Type identifier` pattern requires there be a conversion between the type of the input and the given type.</span></span>

<span data-ttu-id="3e920-109">Se propone relajarse y cambiar `expression is Type identifier`, además de permitirse en las condiciones en las que se permite en C# 7, que también se permitan cuando se permitan `expression as Type`.</span><span class="sxs-lookup"><span data-stu-id="3e920-109">We propose to relax this and change `expression is Type identifier`, in addition to being permitted in the conditions when it is permitted in C# 7, to also be permitted when `expression as Type` would be allowed.</span></span> <span data-ttu-id="3e920-110">En concreto, los nuevos casos son casos en los que el tipo de la expresión o el tipo especificado es un tipo abierto.</span><span class="sxs-lookup"><span data-stu-id="3e920-110">Specifically, the new cases are cases where the type of the expression or the specified type is an open type.</span></span> 

## <a name="motivation"></a><span data-ttu-id="3e920-111">Motivación</span><span class="sxs-lookup"><span data-stu-id="3e920-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3e920-112">Los casos en los que la coincidencia de patrones debe permitirse "obviamente" no se pueden compilar actualmente.</span><span class="sxs-lookup"><span data-stu-id="3e920-112">Cases where pattern-matching should "obviously" be permitted currently fail to compile.</span></span> <span data-ttu-id="3e920-113">Consulte, por ejemplo, https://github.com/dotnet/roslyn/issues/16195.</span><span class="sxs-lookup"><span data-stu-id="3e920-113">See, for example, https://github.com/dotnet/roslyn/issues/16195.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3e920-114">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="3e920-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="3e920-115">Cambiamos el párrafo en la especificación de coincidencia de patrones (la suma propuesta se muestra en negrita):</span><span class="sxs-lookup"><span data-stu-id="3e920-115">We change the paragraph in the pattern-matching specification (the proposed addition is shown in bold):</span></span>

> <span data-ttu-id="3e920-116">Ciertas combinaciones de tipo estático del lado izquierdo y del tipo especificado se consideran incompatibles y producen un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="3e920-116">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="3e920-117">Se dice que un valor de tipo estático `E` es *compatible* con el tipo `T` si existe una conversión de identidad, una conversión de referencia implícita, una conversión boxing, una conversión de referencia explícita o una conversión unboxing de `E` a `T` **, o bien, si `E` o `T` es un tipo abierto**.</span><span class="sxs-lookup"><span data-stu-id="3e920-117">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`**, or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="3e920-118">Es un error en tiempo de compilación si una expresión de tipo `E` no es compatible con el tipo en un patrón de tipo con el que se encuentra una coincidencia.</span><span class="sxs-lookup"><span data-stu-id="3e920-118">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="3e920-119">Desventajas</span><span class="sxs-lookup"><span data-stu-id="3e920-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="3e920-120">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="3e920-120">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="3e920-121">Alternativas</span><span class="sxs-lookup"><span data-stu-id="3e920-121">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3e920-122">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="3e920-122">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="3e920-123">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="3e920-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="3e920-124">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="3e920-124">None.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="3e920-125">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="3e920-125">Design meetings</span></span>

<span data-ttu-id="3e920-126">LDM consideró esta pregunta y creía que era un cambio de nivel de corrección de errores.</span><span class="sxs-lookup"><span data-stu-id="3e920-126">LDM considered this question and felt it was a bug-fix level change.</span></span> <span data-ttu-id="3e920-127">Lo tratamos como una característica de lenguaje independiente porque la realización del cambio una vez que se ha lanzado el idioma presentaría una incompatibilidad de avance.</span><span class="sxs-lookup"><span data-stu-id="3e920-127">We are treating it as a separate language feature because just making the change after the language has been released would introduce a forward incompatibility.</span></span> <span data-ttu-id="3e920-128">El uso del cambio propuesto requiere que el programador especifique la versión de idioma 7,1.</span><span class="sxs-lookup"><span data-stu-id="3e920-128">Using the proposed change requires that the programmer specify language version 7.1.</span></span>
