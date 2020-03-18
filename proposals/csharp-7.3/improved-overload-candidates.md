---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483581"
---
# <a name="improved-overload-candidates"></a>Mejoras en los candidatos de sobrecarga

## <a name="summary"></a>Resumen
[summary]: #summary

Las reglas de resolución de sobrecarga se han actualizado en C# casi todas las actualizaciones de idioma para mejorar la experiencia de los programadores, haciendo que las invocaciones ambiguas seleccionen la opción "obvio". Esto se debe hacer con cuidado para mantener la compatibilidad con versiones anteriores, pero dado que normalmente estamos resolviendo lo que, de otro modo, serían casos de error, estas mejoras normalmente funcionan bien.

1. Cuando un grupo de métodos contiene miembros estáticos y de instancia, se descartan los miembros de instancia si se invocan sin un receptor o contexto de instancia y se descartan los miembros estáticos si se invocan con un receptor de instancia. Cuando no hay ningún receptor, se incluyen solo los miembros estáticos en un contexto estático, de lo contrario, los miembros estáticos y de instancia. Cuando el receptor es ambiguomente una instancia o un tipo debido a una situación de color del color, incluimos ambos. Un contexto estático, donde no se puede usar un receptor implícito de esta instancia, incluye el cuerpo de los miembros en los que no está definido, como los miembros estáticos, así como los lugares en los que no se puede usar, como los inicializadores de campo y los inicializadores de constructor.
2. Cuando un grupo de métodos contiene algunos métodos genéricos cuyos argumentos de tipo no cumplen con sus restricciones, estos miembros se eliminan del conjunto de candidatos.
3. En el caso de una conversión de grupo de métodos, los métodos candidatos cuyo tipo de valor devuelto no coincide con el tipo de valor devuelto del delegado se eliminan del conjunto.
