---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483497"
---
# <a name="null-conditional-await"></a><span data-ttu-id="423c7-101">Await condicional null</span><span class="sxs-lookup"><span data-stu-id="423c7-101">null-conditional await</span></span>

* <span data-ttu-id="423c7-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="423c7-102">[x] Proposed</span></span>
* <span data-ttu-id="423c7-103">[] Prototipo: ninguno</span><span class="sxs-lookup"><span data-stu-id="423c7-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="423c7-104">[] Implementación: ninguno</span><span class="sxs-lookup"><span data-stu-id="423c7-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="423c7-105">[] Especificación: iniciada, a continuación</span><span class="sxs-lookup"><span data-stu-id="423c7-105">[ ] Specification: Started, below</span></span>

## <a name="summary"></a><span data-ttu-id="423c7-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="423c7-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="423c7-107">Admite una expresión con el formato `await? e`, que espera `e` si no es null; en caso contrario, da como resultado `null`.</span><span class="sxs-lookup"><span data-stu-id="423c7-107">Support an expression of the form `await? e`, which awaits `e` if it is non-null, otherwise it results in `null`.</span></span>

## <a name="motivation"></a><span data-ttu-id="423c7-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="423c7-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="423c7-109">Se trata de un patrón de codificación común y esta característica tendría una buena sinergia con los operadores existentes de propagación de NULL y de fusión de valores NULL.</span><span class="sxs-lookup"><span data-stu-id="423c7-109">This is a common coding pattern, and this feature would have nice synergy with the existing null-propagating and null-coalescing operators.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="423c7-110">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="423c7-110">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="423c7-111">Agregamos un nuevo formulario de la *await_expression*:</span><span class="sxs-lookup"><span data-stu-id="423c7-111">We add a new form of the *await_expression*:</span></span>

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

<span data-ttu-id="423c7-112">El operador de `await` condicional null espera su operando solo si ese operando no es NULL.</span><span class="sxs-lookup"><span data-stu-id="423c7-112">The null-conditional `await` operator awaits its operand only if that operand is non-null.</span></span> <span data-ttu-id="423c7-113">De lo contrario, el resultado de aplicar el operador es NULL.</span><span class="sxs-lookup"><span data-stu-id="423c7-113">Otherwise the result of applying the operator is null.</span></span>

<span data-ttu-id="423c7-114">El tipo del resultado se calcula mediante las reglas del [operador condicional null](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span><span class="sxs-lookup"><span data-stu-id="423c7-114">The type of the result is computed using the [rules for the null-conditional operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span></span>

> <span data-ttu-id="423c7-115">**Nota:** Si `e` es de tipo `Task`, `await? e;` no haría nada si `e` es `null`y Await `e` si no se `null`.</span><span class="sxs-lookup"><span data-stu-id="423c7-115">**NOTE:** If `e` is of type `Task`, then `await? e;` would do nothing if `e` is `null`, and await `e` if it is not `null`.</span></span>
>
> <span data-ttu-id="423c7-116">Si `e` es de tipo `Task<K>` donde `K` es un tipo de valor, entonces `await? e` produciría un valor de tipo `K?`.</span><span class="sxs-lookup"><span data-stu-id="423c7-116">If `e` is of type `Task<K>` where `K` is a value type, then `await? e` would yield a value of type `K?`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="423c7-117">Desventajas</span><span class="sxs-lookup"><span data-stu-id="423c7-117">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="423c7-118">Al igual que con cualquier característica de lenguaje, debemos cuestionar si se reembolsa la complejidad adicional en el idioma en la claridad adicional que se C# ofrece al cuerpo de los programas que se beneficiarían de la característica.</span><span class="sxs-lookup"><span data-stu-id="423c7-118">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="423c7-119">Alternativas</span><span class="sxs-lookup"><span data-stu-id="423c7-119">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="423c7-120">Aunque requiere algún código reutilizable, los usos de este operador a menudo se pueden reemplazar por una expresión similar a `(e == null) ? null : await e` o una instrucción como `if (e != null) await e`.</span><span class="sxs-lookup"><span data-stu-id="423c7-120">Although it requires some boilerplate code, uses of this operator can often be replaced by an expression something like `(e == null) ? null : await e` or a statement like `if (e != null) await e`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="423c7-121">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="423c7-121">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="423c7-122">[] Requiere la revisión de LDM</span><span class="sxs-lookup"><span data-stu-id="423c7-122">[ ] Requires LDM review</span></span>

## <a name="design-meetings"></a><span data-ttu-id="423c7-123">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="423c7-123">Design meetings</span></span>

<span data-ttu-id="423c7-124">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="423c7-124">None.</span></span>
