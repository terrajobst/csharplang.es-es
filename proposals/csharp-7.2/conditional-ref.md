---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483671"
---
# <a name="conditional-ref-expressions"></a><span data-ttu-id="cb4d2-101">Expresiones de referencia condicional</span><span class="sxs-lookup"><span data-stu-id="cb4d2-101">Conditional ref expressions</span></span>

<span data-ttu-id="cb4d2-102">El patrón de enlazar una variable de referencia a una u otra expresión condicional no se expresa actualmente en C#.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-102">The pattern of binding a ref variable to one or another expression conditionally is not currently expressible in C#.</span></span>

<span data-ttu-id="cb4d2-103">La solución habitual es introducir un método como:</span><span class="sxs-lookup"><span data-stu-id="cb4d2-103">The typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="cb4d2-104">Tenga en cuenta que esto no es un reemplazo exacto de ternario, ya que todos los argumentos deben evaluarse en el sitio de llamada.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-104">Note that this is not an exact replacement of a ternary since all arguments must be evaluated at the call site.</span></span>

<span data-ttu-id="cb4d2-105">Lo siguiente no funcionará según lo esperado:</span><span class="sxs-lookup"><span data-stu-id="cb4d2-105">The following will not work as expected:</span></span>

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

<span data-ttu-id="cb4d2-106">La sintaxis propuesta sería la siguiente:</span><span class="sxs-lookup"><span data-stu-id="cb4d2-106">The proposed syntax would look like:</span></span>

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

<span data-ttu-id="cb4d2-107">El intento anterior con "Choice" se puede escribir _correctamente_ con Ref ternario como:</span><span class="sxs-lookup"><span data-stu-id="cb4d2-107">The above attempt with "Choice" can be _correctly_ written using ref ternary as:</span></span>

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="cb4d2-108">La diferencia de Choice es que se tiene acceso a las expresiones de consecuencia y alternativas de una manera _realmente_ condicional, por lo que no vemos un bloqueo si ```arr == null```</span><span class="sxs-lookup"><span data-stu-id="cb4d2-108">The difference from Choice is that consequence and alternative expressions are accessed in a _truly_ conditional manner, so we do not see a crash if ```arr == null```</span></span>

<span data-ttu-id="cb4d2-109">La referencia ternaria es simplemente un ternario, donde tanto la alternativa como la consecuencia son refs.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-109">The ternary ref is just a ternary where both alternative and consequence are refs.</span></span> <span data-ttu-id="cb4d2-110">De forma natural, será necesario que los operandos de consecuencia/alternativos sean LValues.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-110">It will naturally require that consequence/alternative operands are LValues.</span></span> <span data-ttu-id="cb4d2-111">También se requerirá que la consecuencia y la alternativa tengan tipos que sean convertibles entre sí.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-111">It will also require that consequence and alternative have types that are identity convertible to each other.</span></span>

<span data-ttu-id="cb4d2-112">El tipo de la expresión se calculará de forma similar a la del ternario normal.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-112">The type of the expression will be computed similarly to the one for the regular ternary.</span></span> <span data-ttu-id="cb4d2-113">es decir,.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-113">I.E.</span></span> <span data-ttu-id="cb4d2-114">en caso de que la consecuencia y la alternativa tengan identidad convertible, pero tipos diferentes, se aplicarán las reglas de combinación de tipos existentes.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-114">in a case if consequence and alternative have identity convertible, but different types, the existing type-merging rules will apply.</span></span>

<span data-ttu-id="cb4d2-115">De forma segura a la devolución se asumirán con cautela los operandos condicionales.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-115">Safe-to-return will be assumed conservatively from the conditional operands.</span></span> <span data-ttu-id="cb4d2-116">Si no es seguro devolver todo lo que no es seguro para devolver.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-116">If either is unsafe to return the whole thing is unsafe to return.</span></span>

<span data-ttu-id="cb4d2-117">Ref ternario es un valor l y, como tal, se puede pasar, asignar o devolver por referencia;</span><span class="sxs-lookup"><span data-stu-id="cb4d2-117">Ref ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="cb4d2-118">Ser un valor l; también se puede asignar a.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-118">Being an LValue, it can also be assigned to.</span></span> 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

<span data-ttu-id="cb4d2-119">La referencia ternaria se puede usar también en un contexto normal (no Ref).</span><span class="sxs-lookup"><span data-stu-id="cb4d2-119">Ref ternary can be used in a regular (not ref) context as well.</span></span> <span data-ttu-id="cb4d2-120">Aunque no sería común, ya que también podía usar un ternario normal.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-120">Although it would not be common since you could as well just use a regular ternary.</span></span>

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

<span data-ttu-id="cb4d2-121">Notas de implementación:</span><span class="sxs-lookup"><span data-stu-id="cb4d2-121">Implementation notes:</span></span> 

<span data-ttu-id="cb4d2-122">La complejidad de la implementación parece ser el tamaño de una corrección de errores de moderado a grande.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-122">The complexity of the implementation would seem to be the size of a moderate-to-large bug fix.</span></span> <span data-ttu-id="cb4d2-123">-I. E no es muy caro.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-123">- I.E not very expensive.</span></span>
<span data-ttu-id="cb4d2-124">No creo que sea necesario realizar cambios en la sintaxis o el análisis.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-124">I do not think we need any changes to the syntax or parsing.</span></span>
<span data-ttu-id="cb4d2-125">No hay ningún efecto en los metadatos o la interoperabilidad.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-125">There is no effect on metadata or interop.</span></span> <span data-ttu-id="cb4d2-126">La característica se basa completamente en expresiones.</span><span class="sxs-lookup"><span data-stu-id="cb4d2-126">The feature is completely expression based.</span></span>
<span data-ttu-id="cb4d2-127">No hay ningún efecto en la depuración/PDB</span><span class="sxs-lookup"><span data-stu-id="cb4d2-127">No effect on debugging/PDB either</span></span>
