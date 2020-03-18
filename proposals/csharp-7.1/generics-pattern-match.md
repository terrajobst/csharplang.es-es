---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483677"
---
# <a name="pattern-matching-with-generics"></a>coincidencia de patrones con genéricos

* [x] propuesto
* [] Prototipo:
* [] Implementación:
* [] Especificación:

## <a name="summary"></a>Resumen
[summary]: #summary

La especificación para el [operador C# as existente](../../spec/expressions.md#the-as-operator) permite que no haya conversión entre el tipo del operando y el tipo especificado cuando uno de los dos es un tipo abierto. Sin embargo, C# en 7 el patrón `Type identifier` requiere que haya una conversión entre el tipo de la entrada y el tipo especificado.

Se propone relajarse y cambiar `expression is Type identifier`, además de permitirse en las condiciones en las que se permite en C# 7, que también se permitan cuando se permitan `expression as Type`. En concreto, los nuevos casos son casos en los que el tipo de la expresión o el tipo especificado es un tipo abierto. 

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Los casos en los que la coincidencia de patrones debe permitirse "obviamente" no se pueden compilar actualmente. Consulte, por ejemplo, https://github.com/dotnet/roslyn/issues/16195.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Cambiamos el párrafo en la especificación de coincidencia de patrones (la suma propuesta se muestra en negrita):

> Ciertas combinaciones de tipo estático del lado izquierdo y del tipo especificado se consideran incompatibles y producen un error en tiempo de compilación. Se dice que un valor de tipo estático `E` es *compatible* con el tipo `T` si existe una conversión de identidad, una conversión de referencia implícita, una conversión boxing, una conversión de referencia explícita o una conversión unboxing de `E` a `T` **, o bien, si `E` o `T` es un tipo abierto**. Es un error en tiempo de compilación si una expresión de tipo `E` no es compatible con el tipo en un patrón de tipo con el que se encuentra una coincidencia.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Ninguno.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Ninguno.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

Ninguno.

## <a name="design-meetings"></a>Design Meetings

LDM consideró esta pregunta y creía que era un cambio de nivel de corrección de errores. Lo tratamos como una característica de lenguaje independiente porque la realización del cambio una vez que se ha lanzado el idioma presentaría una incompatibilidad de avance. El uso del cambio propuesto requiere que el programador especifique la versión de idioma 7,1.
