---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483497"
---
# <a name="null-conditional-await"></a>Await condicional null

* [x] propuesto
* [] Prototipo: ninguno
* [] Implementación: ninguno
* [] Especificación: iniciada, a continuación

## <a name="summary"></a>Resumen
[summary]: #summary

Admite una expresión con el formato `await? e`, que espera `e` si no es null; en caso contrario, da como resultado `null`.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Se trata de un patrón de codificación común y esta característica tendría una buena sinergia con los operadores existentes de propagación de NULL y de fusión de valores NULL.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Agregamos un nuevo formulario de la *await_expression*:

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

El operador de `await` condicional null espera su operando solo si ese operando no es NULL. De lo contrario, el resultado de aplicar el operador es NULL.

El tipo del resultado se calcula mediante las reglas del [operador condicional null](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).

> **Nota:** Si `e` es de tipo `Task`, `await? e;` no haría nada si `e` es `null`y Await `e` si no se `null`.
>
> Si `e` es de tipo `Task<K>` donde `K` es un tipo de valor, entonces `await? e` produciría un valor de tipo `K?`.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Al igual que con cualquier característica de lenguaje, debemos cuestionar si se reembolsa la complejidad adicional en el idioma en la claridad adicional que se C# ofrece al cuerpo de los programas que se beneficiarían de la característica.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Aunque requiere algún código reutilizable, los usos de este operador a menudo se pueden reemplazar por una expresión similar a `(e == null) ? null : await e` o una instrucción como `if (e != null) await e`.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- [] Requiere la revisión de LDM

## <a name="design-meetings"></a>Design Meetings

Ninguno.
