---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2019
ms.locfileid: "79483893"
---
# <a name="null-coalescing-assignment"></a><span data-ttu-id="df2db-101">Asignación de uso combinado de NULL</span><span class="sxs-lookup"><span data-stu-id="df2db-101">null coalescing assignment</span></span>

* <span data-ttu-id="df2db-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="df2db-102">[x] Proposed</span></span>
* <span data-ttu-id="df2db-103">[] Prototipo: no iniciado</span><span class="sxs-lookup"><span data-stu-id="df2db-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="df2db-104">[] Implementación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="df2db-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="df2db-105">[] Especificación: abajo</span><span class="sxs-lookup"><span data-stu-id="df2db-105">[ ] Specification: Below</span></span>

## <a name="summary"></a><span data-ttu-id="df2db-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="df2db-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="df2db-107">Simplifica un patrón de codificación común en el que se asigna un valor a una variable si es NULL.</span><span class="sxs-lookup"><span data-stu-id="df2db-107">Simplifies a common coding pattern where a variable is assigned a value if it is null.</span></span>

<span data-ttu-id="df2db-108">Como parte de esta propuesta, también se van a aflojar los requisitos de tipo en `??` para permitir una expresión cuyo tipo sea un parámetro de tipo sin restricciones que se va a usar en el lado izquierdo.</span><span class="sxs-lookup"><span data-stu-id="df2db-108">As part of this proposal, we will also loosen the type requirements on `??` to allow an expression whose type is an unconstrained type parameter to be used on the left-hand side.</span></span>

## <a name="motivation"></a><span data-ttu-id="df2db-109">Motivación</span><span class="sxs-lookup"><span data-stu-id="df2db-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="df2db-110">Es habitual ver el código del formulario</span><span class="sxs-lookup"><span data-stu-id="df2db-110">It is common to see code of the form</span></span>

```csharp
if (variable == null)
{
    variable = expression;
}
```

<span data-ttu-id="df2db-111">Esta propuesta agrega un operador binario no sobrecargable al lenguaje que realiza esta función.</span><span class="sxs-lookup"><span data-stu-id="df2db-111">This proposal adds a non-overloadable binary operator to the language that performs this function.</span></span>

<span data-ttu-id="df2db-112">Hay al menos ocho solicitudes de comunidad independientes para esta característica.</span><span class="sxs-lookup"><span data-stu-id="df2db-112">There have been at least eight separate community requests for this feature.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="df2db-113">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="df2db-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="df2db-114">Agregamos un nuevo formulario de operador de asignación</span><span class="sxs-lookup"><span data-stu-id="df2db-114">We add a new form of assignment operator</span></span>

``` antlr
assignment_operator
    : '??='
    ;
```

<span data-ttu-id="df2db-115">Que sigue las [reglas semánticas existentes para los operadores de asignación compuesta](../../spec/expressions.md#compound-assignment), salvo que Elide la asignación si el lado izquierdo no es NULL.</span><span class="sxs-lookup"><span data-stu-id="df2db-115">Which follows the [existing semantic rules for compound assignment operators](../../spec/expressions.md#compound-assignment), except that we elide the assignment if the left-hand side is non-null.</span></span> <span data-ttu-id="df2db-116">Las reglas para esta característica son las siguientes.</span><span class="sxs-lookup"><span data-stu-id="df2db-116">The rules for this feature are as follows.</span></span>

<span data-ttu-id="df2db-117">Dado `a ??= b`, donde `A` es el tipo de `a`, `B` es el tipo de `b`y `A0` es el tipo subyacente de `A` si `A` es un tipo de valor que acepta valores NULL:</span><span class="sxs-lookup"><span data-stu-id="df2db-117">Given `a ??= b`, where `A` is the type of `a`, `B` is the type of `b`, and `A0` is the underlying type of `A` if `A` is a nullable value type:</span></span>

1. <span data-ttu-id="df2db-118">Si `A` no existe o es un tipo de valor que no acepta valores NULL, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="df2db-118">If `A` does not exist or is a non-nullable value type, a compile-time error occurs.</span></span>
2. <span data-ttu-id="df2db-119">Si `B` no se pueden convertir implícitamente a `A` o `A0` (si `A0` existe), se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="df2db-119">If `B` is not implicitly convertible to `A` or `A0` (if `A0` exists), a compile-time error occurs.</span></span>
3. <span data-ttu-id="df2db-120">Si `A0` existe y `B` es implícitamente convertible a `A0`y `B` no es dinámico, se `a ??= b` el tipo de `A0`.</span><span class="sxs-lookup"><span data-stu-id="df2db-120">If `A0` exists and `B` is implicitly convertible to `A0`, and `B` is not dynamic, then the type of `a ??= b` is `A0`.</span></span> <span data-ttu-id="df2db-121">`a ??= b` se evalúa en tiempo de ejecución como:</span><span class="sxs-lookup"><span data-stu-id="df2db-121">`a ??= b` is evaluated at runtime as:</span></span>
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   <span data-ttu-id="df2db-122">Salvo que `a` solo se evalúa una vez.</span><span class="sxs-lookup"><span data-stu-id="df2db-122">Except that `a` is only evaluated once.</span></span>
4. <span data-ttu-id="df2db-123">De lo contrario, el tipo de `a ??= b` se `A`.</span><span class="sxs-lookup"><span data-stu-id="df2db-123">Otherwise, the type of `a ??= b` is `A`.</span></span> <span data-ttu-id="df2db-124">`a ??= b` se evalúa en tiempo de ejecución como `a ?? (a = b)`, salvo que `a` solo se evalúa una vez.</span><span class="sxs-lookup"><span data-stu-id="df2db-124">`a ??= b` is evaluated at runtime as `a ?? (a = b)`, except that `a` is only evaluated once.</span></span>


<span data-ttu-id="df2db-125">Para la flexibilización de los requisitos de tipo de `??`, actualizamos la especificación en la que actualmente se indica que, en la `a ?? b`, donde `A` es el tipo de `a`:</span><span class="sxs-lookup"><span data-stu-id="df2db-125">For the relaxation of the type requirements of `??`, we update the spec where it currently states that, given `a ?? b`, where `A` is the type of `a`:</span></span>

> 1. <span data-ttu-id="df2db-126">Si existe y no es un tipo que acepta valores NULL o un tipo de referencia, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="df2db-126">If A exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>

<span data-ttu-id="df2db-127">Este requisito se reduce a:</span><span class="sxs-lookup"><span data-stu-id="df2db-127">We relax this requirement to:</span></span>

1. <span data-ttu-id="df2db-128">Si existe y es un tipo de valor que no acepta valores NULL, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="df2db-128">If A exists and is a non-nullable value type, a compile-time error occurs.</span></span>

<span data-ttu-id="df2db-129">Esto permite que el operador de uso combinado de NULL funcione en parámetros de tipo sin restricciones, ya que el parámetro de tipo sin restricciones T existe, no es un tipo que acepta valores NULL y no es un tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="df2db-129">This allows the null coalescing operator to work on unconstrained type parameters, as the unconstrained type parameter T exists, is not a nullable type, and is not a reference type.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="df2db-130">Desventajas</span><span class="sxs-lookup"><span data-stu-id="df2db-130">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="df2db-131">Al igual que con cualquier característica de lenguaje, debemos cuestionar si se reembolsa la complejidad adicional en el idioma en la claridad adicional que se C# ofrece al cuerpo de los programas que se beneficiarían de la característica.</span><span class="sxs-lookup"><span data-stu-id="df2db-131">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="df2db-132">Alternativas</span><span class="sxs-lookup"><span data-stu-id="df2db-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="df2db-133">El programador puede escribir `(x = x ?? y)`, `if (x == null) x = y;`o `x ?? (x = y)` a mano.</span><span class="sxs-lookup"><span data-stu-id="df2db-133">The programmer can write `(x = x ?? y)`, `if (x == null) x = y;`, or `x ?? (x = y)` by hand.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="df2db-134">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="df2db-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="df2db-135">[] Requiere la revisión de LDM</span><span class="sxs-lookup"><span data-stu-id="df2db-135">[ ] Requires LDM review</span></span>
- <span data-ttu-id="df2db-136">[] ¿También se deben admitir los operadores `&&=` y `||=`?</span><span class="sxs-lookup"><span data-stu-id="df2db-136">[ ] Should we also support `&&=` and `||=` operators?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="df2db-137">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="df2db-137">Design meetings</span></span>

<span data-ttu-id="df2db-138">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="df2db-138">None.</span></span>
