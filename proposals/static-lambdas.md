---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "79484001"
---
# <a name="static-lambdas"></a>Expresiones lambda estáticas

## <a name="summary"></a>Resumen

Admite lambdas que no permiten capturar el estado del ámbito de inclusión.

## <a name="motivation"></a>Motivación

Evite capturar involuntariamente el estado del contexto envolvente.

## <a name="detailed-design"></a>Diseño detallado

Una expresión lambda con `static` no puede capturar el estado del ámbito de inclusión.
Como resultado, las variables locales, los parámetros y `this` del ámbito de inclusión no están disponibles dentro de una expresión lambda `static`.

Una expresión lambda de `static` no puede hacer referencia a miembros de instancia desde una `this` implícita o explícita o una referencia `base`.

Una expresión lambda `static` puede hacer referencia a `static` miembros del ámbito de inclusión.

Una expresión lambda `static` puede hacer referencia a definiciones de `constant` del ámbito de inclusión.

`nameof()` en una expresión lambda `static` puede hacer referencia a variables locales, parámetros o `this` o `base` del ámbito de inclusión.

Las reglas de accesibilidad para `private` miembros del ámbito de inclusión son las mismas para las expresiones lambda `static` y`static`.

No se garantiza que se emita una `static` definición lambda como método de `static` en los metadatos. Esto se deja hasta la implementación del compilador para optimizar.

Una función local o lambda que no sea`static` puede capturar el estado de una expresión lambda `static`, pero no puede capturar el estado fuera de la expresión lambda `static` envolvente.

Una expresión lambda de `static` se puede usar en un árbol de expresión.

Al quitar el modificador `static` de una expresión lambda en un programa válido, no se cambia el significado del programa.
