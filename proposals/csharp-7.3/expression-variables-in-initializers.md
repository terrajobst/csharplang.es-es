---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483863"
---
# <a name="expression-variables-in-initializers"></a>Variables de expresión en inicializadores

## <a name="summary"></a>Resumen
[summary]: #summary

Se amplían las características introducidas en C# 7 para permitir expresiones que contengan variables de expresión (modelos de declaración y declaraciones de variables de salida) en inicializadores de campo, inicializadores de propiedad, constructores de inicializadores y cláusulas de consulta.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Esto completa un par de bordes aproximados que quedan en el C# idioma debido a la falta de tiempo.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Quitamos la restricción que impide la declaración de variables de expresión (declaraciones de variables de salida y modelos de declaración) en un inicializador de constructor. Este tipo de variable declarado está en el ámbito de todo el cuerpo del constructor.

Quitamos la restricción que impide la declaración de variables de expresión (declaraciones de variables y modelos de declaración) en un inicializador de campo o propiedad. Este tipo de variable declarado está en el ámbito de la expresión de inicialización.

Quitamos la restricción que impide la declaración de variables de expresión (declaraciones de variables de salida y modelos de declaración) en una cláusula de expresión de consulta que se traduce en el cuerpo de una expresión lambda. Este tipo de variable declarado está en el ámbito de esa expresión de la cláusula de consulta.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Ninguno.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

El ámbito adecuado para las variables de expresión declaradas en estos contextos no es obvio y merece más información sobre el LDM.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- [] ¿Cuál es el ámbito adecuado para estas variables?

## <a name="design-meetings"></a>Design Meetings

Ninguno.
