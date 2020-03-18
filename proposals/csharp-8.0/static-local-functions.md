---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "79484013"
---
# <a name="static-local-functions"></a>Funciones locales estáticas

## <a name="summary"></a>Resumen

Admitir funciones locales que no permiten capturar el estado del ámbito de inclusión.

## <a name="motivation"></a>Motivación

Evite capturar involuntariamente el estado del contexto envolvente.
Permite usar funciones locales en escenarios en los que se requiere un método `static`.

## <a name="detailed-design"></a>Diseño detallado

Una función local declarada `static` no puede capturar el estado del ámbito de inclusión.
Como resultado, las variables locales, los parámetros y `this` del ámbito de inclusión no están disponibles dentro de una función `static` local.

Una función local `static` no puede hacer referencia a miembros de instancia desde una `this` implícita o explícita o una referencia `base`.

Una función local `static` puede hacer referencia a `static` miembros del ámbito de inclusión.

Una función local `static` puede hacer referencia a definiciones de `constant` del ámbito de inclusión.

`nameof()` en una función local `static` puede hacer referencia a variables locales, parámetros o `this` o `base` del ámbito de inclusión.

Las reglas de accesibilidad para `private` miembros del ámbito de inclusión son las mismas para las funciones locales `static` y`static`.

Un `static` definición de función local se emite como un método de `static` en los metadatos, incluso si solo se usa en un delegado.

Una función local o lambda que no sea`static` puede capturar el estado de una función local `static`, pero no puede capturar el estado fuera de la función local envolvente `static`.

No se puede invocar una función local `static` en un árbol de expresión.

Una llamada a una función local se emite como `call` en lugar de `callvirt`, independientemente de si la función local está `static`.

Resolución de sobrecarga de una llamada en una función local que no se ve afectada por si la función local está `static`.

Al quitar el modificador `static` de una función local en un programa válido, no se cambia el significado del programa.

## <a name="design-meetings"></a>Design Meetings

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
