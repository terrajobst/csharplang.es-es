---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483437"
---
# <a name="binary-literals"></a>Literales binarios

Hay una solicitud relativamente común para agregar literales binarios a C# y VB. En el caso de las máscaras de. (por ejemplo, las enumeraciones de marcas) esto parece realmente útil, pero también sería excelente solo con fines educativos.

Los literales binarios tendrían el siguiente aspecto:

```csharp
int nineteen = 0b10011;
```

Sintácticamente y semánticamente son idénticos a los literales hexadecimales, excepto por el uso de `b`/`B` en lugar de `x`/`X`, que solo tienen dígitos `0` y `1` y se interpretan en base 2 en lugar de 16.

El costo de la implementación de estos y la poca sobrecarga conceptual para los usuarios del lenguaje es escaso.

## <a name="syntax"></a>Sintaxis

La gramática sería la siguiente:

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
