---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483503"
---
# <a name="digit-separators"></a><span data-ttu-id="dde6a-101">Separadores de dígitos</span><span class="sxs-lookup"><span data-stu-id="dde6a-101">Digit separators</span></span>

<span data-ttu-id="dde6a-102">La posibilidad de agrupar los dígitos en literales numéricos de gran tamaño tendría un gran impacto en la legibilidad y sin ningún inconveniente significativo.</span><span class="sxs-lookup"><span data-stu-id="dde6a-102">Being able to group digits in large numeric literals would have great readability impact and no significant downside.</span></span> 

<span data-ttu-id="dde6a-103">Agregar literales binarios (#215) aumentaría la probabilidad de que los literales numéricos sean largos, por lo que las dos características se mejoran.</span><span class="sxs-lookup"><span data-stu-id="dde6a-103">Adding binary literals (#215) would increase the likelihood of numeric literals being long, so the two features enhance each other.</span></span> 

<span data-ttu-id="dde6a-104">Seguiremos Java y otros, y usaremos un carácter de subrayado `_` como separador de dígitos.</span><span class="sxs-lookup"><span data-stu-id="dde6a-104">We would follow Java and others, and use an underscore `_` as a digit separator.</span></span> <span data-ttu-id="dde6a-105">Podría producirse en cualquier parte en un literal numérico (excepto en el primer y el último carácter), ya que las agrupaciones diferentes pueden tener sentido en escenarios diferentes y especialmente en diferentes bases numéricas:</span><span class="sxs-lookup"><span data-stu-id="dde6a-105">It would be able to occur everywhere in a numeric literal (except as the first and last character), since different groupings may make sense in different scenarios and especially for different numeric bases:</span></span>

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

<span data-ttu-id="dde6a-106">Cualquier secuencia de dígitos puede estar separada por caracteres de subrayado, posiblemente más de un carácter de subrayado entre dos dígitos consecutivos.</span><span class="sxs-lookup"><span data-stu-id="dde6a-106">Any sequence of digits may be separated by underscores, possibly more than one underscore between two consecutive digits.</span></span> <span data-ttu-id="dde6a-107">Se permiten en los decimales y en los exponentes, pero después de la regla anterior, es posible que no aparezcan junto a la coma decimal (`10_.0`), junto al carácter de exponente (`1.1e_1`) o al lado del especificador de tipo (`10_f`).</span><span class="sxs-lookup"><span data-stu-id="dde6a-107">They are allowed in decimals as well as exponents, but following the previous rule, they may not appear next to the decimal (`10_.0`), next to the exponent character (`1.1e_1`), or next to the type specifier (`10_f`).</span></span> <span data-ttu-id="dde6a-108">Cuando se usa en literales binarios y hexadecimales, puede que no aparezcan inmediatamente después del `0x` o `0b`.</span><span class="sxs-lookup"><span data-stu-id="dde6a-108">When used in binary and hexadecimal literals, they may not appear immediately following the `0x` or `0b`.</span></span>

<span data-ttu-id="dde6a-109">La sintaxis es sencilla y los separadores no tienen ningún impacto semántico; simplemente se omiten.</span><span class="sxs-lookup"><span data-stu-id="dde6a-109">The syntax is straightforward, and the separators have no semantic impact - they are simply ignored.</span></span>

<span data-ttu-id="dde6a-110">Esto tiene un gran valor y es fácil de implementar.</span><span class="sxs-lookup"><span data-stu-id="dde6a-110">This has broad value and is easy to implement.</span></span>
