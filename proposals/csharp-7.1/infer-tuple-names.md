---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483689"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a><span data-ttu-id="45043-101">Inferir nombres de tupla (también conocidos como</span><span class="sxs-lookup"><span data-stu-id="45043-101">Infer tuple names (aka.</span></span> <span data-ttu-id="45043-102">inicializadores de proyección de tupla)</span><span class="sxs-lookup"><span data-stu-id="45043-102">tuple projection initializers)</span></span>

## <a name="summary"></a><span data-ttu-id="45043-103">Resumen</span><span class="sxs-lookup"><span data-stu-id="45043-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="45043-104">En una serie de casos comunes, esta característica permite omitir los nombres de elemento de tupla y, en su lugar, deducirlos.</span><span class="sxs-lookup"><span data-stu-id="45043-104">In a number of common cases, this feature allows the tuple element names to be omitted and instead be inferred.</span></span> <span data-ttu-id="45043-105">Por ejemplo, en lugar de escribir `(f1: x.f1, f2: x?.f2)`, los nombres de elemento "F1" y "F2" se pueden inferir de `(x.f1, x?.f2)`.</span><span class="sxs-lookup"><span data-stu-id="45043-105">For instance, instead of typing `(f1: x.f1, f2: x?.f2)`, the element names "f1" and "f2" can be inferred from `(x.f1, x?.f2)`.</span></span>

<span data-ttu-id="45043-106">Esto es paralelo al comportamiento de los tipos anónimos, que permiten deducir los nombres de miembro durante la creación.</span><span class="sxs-lookup"><span data-stu-id="45043-106">This parallels the behavior of  anonymous types, which allow inferring member names during creation.</span></span> <span data-ttu-id="45043-107">Por ejemplo, `new { x.f1, y?.f2 }` declara los miembros "F1" y "F2".</span><span class="sxs-lookup"><span data-stu-id="45043-107">For instance, `new { x.f1, y?.f2 }` declares members "f1" and "f2".</span></span>

<span data-ttu-id="45043-108">Esto es especialmente útil cuando se usan tuplas en LINQ:</span><span class="sxs-lookup"><span data-stu-id="45043-108">This is particularly handy when using tuples in LINQ:</span></span>

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a><span data-ttu-id="45043-109">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="45043-109">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="45043-110">Hay dos partes del cambio:</span><span class="sxs-lookup"><span data-stu-id="45043-110">There are two parts to the change:</span></span>

1.  <span data-ttu-id="45043-111">Intente inferir un nombre de candidato para cada elemento de tupla que no tenga un nombre explícito:</span><span class="sxs-lookup"><span data-stu-id="45043-111">Try to infer a candidate name for each tuple element which does not have an explicit name:</span></span>
    -   <span data-ttu-id="45043-112">Usar las mismas reglas que la inferencia de nombres para los tipos anónimos.</span><span class="sxs-lookup"><span data-stu-id="45043-112">Using same rules as name inference for anonymous types.</span></span>
        - <span data-ttu-id="45043-113">En C#, esto permite tres casos: `y` (identificador), `x.y` (acceso a miembros simple) y `x?.y` (acceso condicional).</span><span class="sxs-lookup"><span data-stu-id="45043-113">In C#, this allows three cases: `y` (identifier), `x.y` (simple member access) and `x?.y` (conditional access).</span></span>
        - <span data-ttu-id="45043-114">En VB, esto permite casos adicionales, como `x.y()`.</span><span class="sxs-lookup"><span data-stu-id="45043-114">In VB, this allows for additional cases, such as `x.y()`.</span></span>
    -   <span data-ttu-id="45043-115">Rechazar nombres de tupla reservados (distingue mayúsculas de C#minúsculas en, sin distinción de mayúsculas y minúsculas en VB), ya que están prohibidos o ya están implícitos.</span><span class="sxs-lookup"><span data-stu-id="45043-115">Rejecting reserved tuple names (case-sensitive in C#, case-insensitive in VB), as they are either forbidden or already implicit.</span></span> <span data-ttu-id="45043-116">Por ejemplo, como `ItemN`, `Rest`y `ToString`.</span><span class="sxs-lookup"><span data-stu-id="45043-116">For instance, such as `ItemN`, `Rest`, and `ToString`.</span></span>
    -   <span data-ttu-id="45043-117">Si los nombres de candidatos son duplicados (con distinción de C#mayúsculas y minúsculas en, sin distinción de mayúsculas y minúsculas en VB) en toda la tupla, se quitan esos candidatos.</span><span class="sxs-lookup"><span data-stu-id="45043-117">If any candidate names are duplicates (case-sensitive in C#, case-insensitive in VB) within the entire tuple, we drop those candidates,</span></span>
2.  <span data-ttu-id="45043-118">Durante las conversiones (que comprueban y avisan sobre la eliminación de nombres de los literales de tupla), los nombres inferidos no generarán ninguna advertencia.</span><span class="sxs-lookup"><span data-stu-id="45043-118">During conversions (which check and warn about dropping names from tuple literals), inferred names would not produce any warnings.</span></span> <span data-ttu-id="45043-119">Esto evita que se interrumpa el código de tupla existente.</span><span class="sxs-lookup"><span data-stu-id="45043-119">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="45043-120">Tenga en cuenta que la regla para administrar duplicados es diferente de la de los tipos anónimos.</span><span class="sxs-lookup"><span data-stu-id="45043-120">Note that the rule for handling duplicates is different than that for anonymous types.</span></span> <span data-ttu-id="45043-121">Por ejemplo, `new { x.f1, x.f1 }` produce un error, pero todavía se permite el `(x.f1, x.f1)` (solo sin nombres inferidos).</span><span class="sxs-lookup"><span data-stu-id="45043-121">For instance, `new { x.f1, x.f1 }` produces an error, but `(x.f1, x.f1)` would still be allowed (just without any inferred names).</span></span> <span data-ttu-id="45043-122">Esto evita que se interrumpa el código de tupla existente.</span><span class="sxs-lookup"><span data-stu-id="45043-122">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="45043-123">Por coherencia, lo mismo se aplica a las tuplas generadas por las asignaciones de desconstrucción ( C#en):</span><span class="sxs-lookup"><span data-stu-id="45043-123">For consistency, the same would apply to tuples produced by deconstruction-assignments (in C#):</span></span>

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

<span data-ttu-id="45043-124">Lo mismo se aplica también a las tuplas de VB, mediante las reglas específicas de VB para deducir el nombre de las comparaciones de expresiones y de nombres que no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="45043-124">The same would also apply to VB tuples, using the VB-specific rules for inferring name from expression and case-insensitive name comparisons.</span></span>

<span data-ttu-id="45043-125">Al usar el C# compilador 7,1 (o posterior) con la versión de idioma "7,0", los nombres de los elementos se deducen (a pesar de que la característica no esté disponible), pero se producirá un error de uso del sitio para intentar tener acceso a ellos.</span><span class="sxs-lookup"><span data-stu-id="45043-125">When using the C# 7.1 compiler (or later) with language version "7.0", the element names will be inferred (despite the feature not being available), but there will be a use-site error for trying to access them.</span></span> <span data-ttu-id="45043-126">Esto limitará las adiciones de código nuevo que posteriormente se verán ante el problema de compatibilidad (que se describe a continuación).</span><span class="sxs-lookup"><span data-stu-id="45043-126">This will limit additions of new code that would later face the compatibility issue (described below).</span></span>

## <a name="drawbacks"></a><span data-ttu-id="45043-127">Desventajas</span><span class="sxs-lookup"><span data-stu-id="45043-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="45043-128">La principal desventaja es que esto presenta una interrupción de compatibilidad C# de 7,0:</span><span class="sxs-lookup"><span data-stu-id="45043-128">The main drawback is that this introduces a compatibility break from C# 7.0:</span></span>

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

<span data-ttu-id="45043-129">El Consejo de compatibilidad encontró este salto aceptable, dado que es limitado y el período de tiempo desde que se enviaron C# tuplas (en 7,0) es corto.</span><span class="sxs-lookup"><span data-stu-id="45043-129">The compatibility council found this break acceptable, given that it is limited and the time window since tuples shipped (in C# 7.0) is short.</span></span>

## <a name="references"></a><span data-ttu-id="45043-130">Referencias</span><span class="sxs-lookup"><span data-stu-id="45043-130">References</span></span>
- [<span data-ttu-id="45043-131">LDM, 4 de abril de 2017</span><span class="sxs-lookup"><span data-stu-id="45043-131">LDM April 4th 2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- <span data-ttu-id="45043-132">[Debate en github](https://github.com/dotnet/csharplang/issues/370) (gracias @alrz para llevar a cabo este problema)</span><span class="sxs-lookup"><span data-stu-id="45043-132">[Github discussion](https://github.com/dotnet/csharplang/issues/370) (thanks @alrz for bringing this issue up)</span></span>
- [<span data-ttu-id="45043-133">Diseño de tuplas</span><span class="sxs-lookup"><span data-stu-id="45043-133">Tuples design</span></span>](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
