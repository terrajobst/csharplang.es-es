---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483503"
---
# <a name="digit-separators"></a>Separadores de dígitos

La posibilidad de agrupar los dígitos en literales numéricos de gran tamaño tendría un gran impacto en la legibilidad y sin ningún inconveniente significativo. 

Agregar literales binarios (#215) aumentaría la probabilidad de que los literales numéricos sean largos, por lo que las dos características se mejoran. 

Seguiremos Java y otros, y usaremos un carácter de subrayado `_` como separador de dígitos. Podría producirse en cualquier parte en un literal numérico (excepto en el primer y el último carácter), ya que las agrupaciones diferentes pueden tener sentido en escenarios diferentes y especialmente en diferentes bases numéricas:

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

Cualquier secuencia de dígitos puede estar separada por caracteres de subrayado, posiblemente más de un carácter de subrayado entre dos dígitos consecutivos. Se permiten en los decimales y en los exponentes, pero después de la regla anterior, es posible que no aparezcan junto a la coma decimal (`10_.0`), junto al carácter de exponente (`1.1e_1`) o al lado del especificador de tipo (`10_f`). Cuando se usa en literales binarios y hexadecimales, puede que no aparezcan inmediatamente después del `0x` o `0b`.

La sintaxis es sencilla y los separadores no tienen ningún impacto semántico; simplemente se omiten.

Esto tiene un gran valor y es fácil de implementar.
