---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483461"
---
# <a name="covariant-return-types"></a><span data-ttu-id="5411f-101">tipos devueltos de covariante</span><span class="sxs-lookup"><span data-stu-id="5411f-101">covariant return types</span></span>

* <span data-ttu-id="5411f-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="5411f-102">[x] Proposed</span></span>
* <span data-ttu-id="5411f-103">[] Prototipo: no iniciado</span><span class="sxs-lookup"><span data-stu-id="5411f-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="5411f-104">[] Implementación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="5411f-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="5411f-105">[] Especificación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="5411f-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="5411f-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="5411f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="5411f-107">Admitir _tipos de valor devuelto covariante_.</span><span class="sxs-lookup"><span data-stu-id="5411f-107">Support _covariant return types_.</span></span> <span data-ttu-id="5411f-108">En concreto, permita que un método de reemplazo tenga un tipo de referencia más derivado que el método que reemplaza.</span><span class="sxs-lookup"><span data-stu-id="5411f-108">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span>

## <a name="motivation"></a><span data-ttu-id="5411f-109">Motivación</span><span class="sxs-lookup"><span data-stu-id="5411f-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="5411f-110">Es un patrón común en el código que los distintos nombres de método deben inventar para solucionar la restricción de lenguaje que las invalidaciones deben devolver el mismo tipo que el método invalidado.</span><span class="sxs-lookup"><span data-stu-id="5411f-110">It is a common pattern in code that different method names have to be invented to work around the language constraint that overrides must return the same type as the overridden method.</span></span> <span data-ttu-id="5411f-111">A continuación se muestra un ejemplo de la base de código Roslyn.</span><span class="sxs-lookup"><span data-stu-id="5411f-111">See below for an example from the Roslyn code base.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="5411f-112">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="5411f-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="5411f-113">Admitir _tipos de valor devuelto covariante_.</span><span class="sxs-lookup"><span data-stu-id="5411f-113">Support _covariant return types_.</span></span> <span data-ttu-id="5411f-114">En concreto, permita que un método de reemplazo tenga un tipo de referencia más derivado que el método que reemplaza.</span><span class="sxs-lookup"><span data-stu-id="5411f-114">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span> <span data-ttu-id="5411f-115">Esto se aplica a los métodos y propiedades, y se admite en clases e interfaces.</span><span class="sxs-lookup"><span data-stu-id="5411f-115">This would apply to methods and properties, and be supported in classes and interfaces.</span></span>

<span data-ttu-id="5411f-116">Esto sería útil en el patrón de fábrica.</span><span class="sxs-lookup"><span data-stu-id="5411f-116">This would be useful in the factory pattern.</span></span> <span data-ttu-id="5411f-117">Por ejemplo, en la base de código Roslyn deberíamos tener</span><span class="sxs-lookup"><span data-stu-id="5411f-117">For example, in the Roslyn code base we would have</span></span>

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

<span data-ttu-id="5411f-118">La implementación de esto sería para que el compilador emita el método de reemplazo como un método virtual "nuevo" que oculta el método de la clase base, junto con un _método de puente_ que implementa el método de clase base con una llamada al método de la clase derivada.</span><span class="sxs-lookup"><span data-stu-id="5411f-118">The implementation of this would be for the compiler to emit the overriding method as a "new" virtual method that hides the base class method, along with a _bridge method_ that implements the base class method with a call to the derived class method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="5411f-119">Desventajas</span><span class="sxs-lookup"><span data-stu-id="5411f-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="5411f-120">[] Todos los cambios de idioma deben pagarse por sí mismos.</span><span class="sxs-lookup"><span data-stu-id="5411f-120">[ ] Every language change must pay for itself.</span></span>
- <span data-ttu-id="5411f-121">[] Debemos asegurarnos de que el rendimiento sea razonable, incluso en el caso de las jerarquías de herencia profunda.</span><span class="sxs-lookup"><span data-stu-id="5411f-121">[ ] We should ensure that the performance is reasonable, even in the case of deep inheritance hierarchies</span></span>
- <span data-ttu-id="5411f-122">[] Debemos asegurarnos de que los artefactos de la estrategia de traslación no afecten a la semántica del lenguaje, incluso cuando se utiliza un nuevo IL de compiladores antiguos.</span><span class="sxs-lookup"><span data-stu-id="5411f-122">[ ] We should ensure that artifacts of the translation strategy do not affect language semantics, even when consuming new IL from old compilers.</span></span>

## <a name="alternatives"></a><span data-ttu-id="5411f-123">Alternativas</span><span class="sxs-lookup"><span data-stu-id="5411f-123">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="5411f-124">Podríamos relajar las reglas de idioma ligeramente para permitir, en origen,</span><span class="sxs-lookup"><span data-stu-id="5411f-124">We could relax the language rules slightly to allow, in source,</span></span>

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a><span data-ttu-id="5411f-125">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="5411f-125">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="5411f-126">[] ¿Cómo funcionarán las API que se han compilado para usar esta característica en versiones anteriores del lenguaje?</span><span class="sxs-lookup"><span data-stu-id="5411f-126">[ ] How will APIs that have been compiled to use this feature work in older versions of the language?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="5411f-127">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="5411f-127">Design meetings</span></span>

<span data-ttu-id="5411f-128">Ninguno todavía.</span><span class="sxs-lookup"><span data-stu-id="5411f-128">None yet.</span></span> <span data-ttu-id="5411f-129">Se ha realizado algún debate en <https://github.com/dotnet/roslyn/issues/357>.</span><span class="sxs-lookup"><span data-stu-id="5411f-129">There has been some discussion at <https://github.com/dotnet/roslyn/issues/357>.</span></span>