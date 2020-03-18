---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "79483917"
---
# <a name="target-typed-default-literal"></a>Literal "default" de tipo de destino

* [x] propuesto
* [x] prototipo
* [x] implementación
* [] Especificación

## <a name="summary"></a>Resumen
[summary]: #summary

La característica `default` con tipo de destino es una variación de forma más corta del operador `default(T)`, que permite omitir el tipo. En su lugar, se deduce su tipo mediante el establecimiento de destinos. Aparte de eso, se comporta como `default(T)`.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

La principal motivación es evitar escribir información redundante.

Por ejemplo, al invocar `void Method(ImmutableArray<SomeType> array)`, el literal *predeterminado* permite `M(default)` en lugar de `M(default(ImmutableArray<SomeType>))`.

Esto es aplicable en varios escenarios, como:

- declarar variables locales (`ImmutableArray<SomeType> x = default;`)
- operaciones ternaria (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)
- devolver métodos y expresiones lambda (`return default;`)
- declarar valores predeterminados para parámetros opcionales (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)
- incluir valores predeterminados en expresiones de creación de matrices (`var x = new[] { default, ImmutableArray.Create(y) };`)


## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Se introduce una nueva expresión, el literal *predeterminado* . Una expresión con esta clasificación se puede convertir implícitamente a cualquier tipo mediante una *conversión literal predeterminada*. 

La inferencia del tipo del literal *predeterminado* funciona igual que para el literal *null* , salvo que se permite cualquier tipo (no solo los tipos de referencia).

Esta conversión genera el valor predeterminado del tipo deducido.

El literal *predeterminado* puede tener un valor constante, dependiendo del tipo deducido. Por tanto `const int x = default;` es válido, pero `const int? y = default;` no.

El literal *predeterminado* puede ser el operando de los operadores de igualdad, siempre que el otro operando tenga un tipo. Por tanto `default == x` y `x == default` son expresiones válidas, pero `default == default` no es válido.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Una desventaja menor es que se puede usar el literal *predeterminado* en lugar de un literal *nulo* en la mayoría de los contextos. Dos de las excepciones son `throw null;` y `null == null`, que se permiten para el literal *null* , pero no para el literal *predeterminado* .

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Hay un par de alternativas a tener en cuenta:

- El estado quo: la característica no está justificada por sus propios méritos y los desarrolladores siguen usando el operador predeterminado con un tipo explícito.
- Extender el literal NULL: este es el enfoque de VB con `Nothing`. Podríamos permitir el `int x = null;`.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- [x] ¿se debe permitir de *forma predeterminada* como operando de los operadores *is* o *as* ? Respuesta: no permitir `default is T`, permitir `x is default`, permitir `default as RefType` (con ADVERTENCIA siempre nula)

## <a name="design-meetings"></a>Design Meetings

- [LDM 3/7/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [LDM 3/28/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [LDM 5/31/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
