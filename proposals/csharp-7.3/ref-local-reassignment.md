---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483587"
---
# <a name="ref-local-reassignment"></a><span data-ttu-id="ee71e-101">Reasignación local de referencia</span><span class="sxs-lookup"><span data-stu-id="ee71e-101">Ref Local Reassignment</span></span>

<span data-ttu-id="ee71e-102">En C# 7,3, se agrega compatibilidad para reenlazar el referente de una variable local de referencia o un parámetro ref.</span><span class="sxs-lookup"><span data-stu-id="ee71e-102">In C# 7.3, we add support for rebinding the referent of a ref local variable or a ref parameter.</span></span>

<span data-ttu-id="ee71e-103">Agregamos lo siguiente al conjunto de `assignment_operator`s.</span><span class="sxs-lookup"><span data-stu-id="ee71e-103">We add the following to the set of `assignment_operator`s.</span></span>

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

<span data-ttu-id="ee71e-104">El operador `=ref` se denomina ***operador de asignación Ref***.</span><span class="sxs-lookup"><span data-stu-id="ee71e-104">The `=ref` operator is called the ***ref assignment operator***.</span></span> <span data-ttu-id="ee71e-105">No es un *operador de asignación compuesta*.</span><span class="sxs-lookup"><span data-stu-id="ee71e-105">It is not a *compound assignment operator*.</span></span> <span data-ttu-id="ee71e-106">El operando izquierdo debe ser una expresión que se enlaza a una variable local ref, un parámetro ref (que no sea `this`) o un parámetro out.</span><span class="sxs-lookup"><span data-stu-id="ee71e-106">The left operand must be an expression that binds to a ref local variable, a ref parameter (other than `this`), or an out parameter.</span></span> <span data-ttu-id="ee71e-107">El operando derecho debe ser una expresión que produzca un valor l que designe un valor del mismo tipo que el operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="ee71e-107">The right operand must be an expression that yields an lvalue designating a value of the same type as the left operand.</span></span>

<span data-ttu-id="ee71e-108">El operando derecho debe estar asignado definitivamente en el punto de la asignación de referencia.</span><span class="sxs-lookup"><span data-stu-id="ee71e-108">The right operand must be definitely assigned at the point of the ref assignment.</span></span>

<span data-ttu-id="ee71e-109">Cuando el operando izquierdo se enlaza a un parámetro `out`, es un error si ese parámetro `out` no se ha asignado definitivamente al principio del operador de asignación Ref.</span><span class="sxs-lookup"><span data-stu-id="ee71e-109">When the left operand binds to an `out` parameter, it is an error if that `out` parameter has not been definitely assigned at the beginning of the ref assignment operator.</span></span>

<span data-ttu-id="ee71e-110">Si el operando izquierdo es una referencia grabable (es decir, designa algo distinto de un parámetro `ref readonly` local o de `in`), el operando derecho debe ser un valor l grabable.</span><span class="sxs-lookup"><span data-stu-id="ee71e-110">If the left operand is a writeable ref (i.e. it designates anything other than a `ref readonly` local or  `in` parameter), then the right operand must be a writeable lvalue.</span></span>

<span data-ttu-id="ee71e-111">El operador de asignación de referencia produce un valor l del tipo asignado.</span><span class="sxs-lookup"><span data-stu-id="ee71e-111">The ref assignment operator yields an lvalue of the assigned type.</span></span> <span data-ttu-id="ee71e-112">Es grabable si el operando izquierdo es grabable (es decir, no `ref readonly` ni `in`).</span><span class="sxs-lookup"><span data-stu-id="ee71e-112">It is writeable if the left operand is writeable (i.e. not `ref readonly` or `in`).</span></span>

<span data-ttu-id="ee71e-113">Las reglas de seguridad para este operador son:</span><span class="sxs-lookup"><span data-stu-id="ee71e-113">The safety rules for this operator are:</span></span>

- <span data-ttu-id="ee71e-114">En el caso de una `e1 = ref e2`de reasignación de referencia, el *parámetro ref-Safe-to-escape* de `e2` debe ser al menos tan ancho como el de la *referencia-Safe-to-escape* de `e1`.</span><span class="sxs-lookup"><span data-stu-id="ee71e-114">For a ref reassignment `e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

<span data-ttu-id="ee71e-115">Donde *ref-Safe-to-escape* se define en [seguridad para los tipos de tipo REF-like](../csharp-7.2/span-safety.md)</span><span class="sxs-lookup"><span data-stu-id="ee71e-115">Where *ref-safe-to-escape* is defined in [Safety for ref-like types](../csharp-7.2/span-safety.md)</span></span>
