---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483437"
---
# <a name="binary-literals"></a><span data-ttu-id="a55b9-101">Literales binarios</span><span class="sxs-lookup"><span data-stu-id="a55b9-101">Binary literals</span></span>

<span data-ttu-id="a55b9-102">Hay una solicitud relativamente común para agregar literales binarios a C# y VB.</span><span class="sxs-lookup"><span data-stu-id="a55b9-102">There’s a relatively common request to add binary literals to C# and VB.</span></span> <span data-ttu-id="a55b9-103">En el caso de las máscaras de. (por ejemplo, las enumeraciones de marcas) esto parece realmente útil, pero también sería excelente solo con fines educativos.</span><span class="sxs-lookup"><span data-stu-id="a55b9-103">For bitmasks (e.g. flag enums) this seems genuinely useful, but it would also be great just for educational purposes.</span></span>

<span data-ttu-id="a55b9-104">Los literales binarios tendrían el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="a55b9-104">Binary literals would look like this:</span></span>

```csharp
int nineteen = 0b10011;
```

<span data-ttu-id="a55b9-105">Sintácticamente y semánticamente son idénticos a los literales hexadecimales, excepto por el uso de `b`/`B` en lugar de `x`/`X`, que solo tienen dígitos `0` y `1` y se interpretan en base 2 en lugar de 16.</span><span class="sxs-lookup"><span data-stu-id="a55b9-105">Syntactically and semantically they are identical to hexadecimal literals, except for using `b`/`B` instead of `x`/`X`, having only digits `0` and `1` and being interpreted in base 2 instead of 16.</span></span>

<span data-ttu-id="a55b9-106">El costo de la implementación de estos y la poca sobrecarga conceptual para los usuarios del lenguaje es escaso.</span><span class="sxs-lookup"><span data-stu-id="a55b9-106">There’s little cost to implementing these, and little conceptual overhead to users of the language.</span></span>

## <a name="syntax"></a><span data-ttu-id="a55b9-107">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="a55b9-107">Syntax</span></span>

<span data-ttu-id="a55b9-108">La gramática sería la siguiente:</span><span class="sxs-lookup"><span data-stu-id="a55b9-108">The grammar would be as follows:</span></span>

```antlr
integer-literal:
    : ...
    | binary-integer-literal
    ;
binary-integer-literal:
    : `0b` binary-digits integer-type-suffix-opt
    | `0B` binary-digits integer-type-suffix-opt
    ;
binary-digits:
    : binary-digit
    | binary-digits binary-digit
    ;
binary-digit:
    : `0`
    | `1`
    ;
```
