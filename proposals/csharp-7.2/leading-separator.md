---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483683"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a>Permitir separador de dígitos después del 0B o 0x

En C# 7,2, ampliamos el conjunto de lugares en los que los separadores de dígitos (el carácter de subrayado) pueden aparecer en literales enteros. [A partir C# de 7,0, se permiten separadores entre los dígitos de un literal](../csharp-7.0/digit-separators.md). Ahora, en C# 7,2, también se permiten separadores de dígitos antes del primer dígito significativo de un literal binario o hexadecimal, después del prefijo.

```csharp
    123      // permitted in C# 1.0 and later
    1_2_3    // permitted in C# 7.0 and later
    0x1_2_3  // permitted in C# 7.0 and later
    0b101    // binary literals added in C# 7.0
    0b1_0_1  // permitted in C# 7.0 and later

    // in C# 7.2, _ is permitted after the `0x` or `0b`
    0x_1_2   // permitted in C# 7.2 and later
    0b_1_0_1 // permitted in C# 7.2 and later
```

No se permite que un literal entero decimal tenga un carácter de subrayado inicial. Un token como `_123` es un identificador.
