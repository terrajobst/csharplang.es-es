---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2019
ms.locfileid: "79483893"
---
# <a name="null-coalescing-assignment"></a>Asignación de uso combinado de NULL

* [x] propuesto
* [] Prototipo: no iniciado
* [] Implementación: no iniciada
* [] Especificación: abajo

## <a name="summary"></a>Resumen
[summary]: #summary

Simplifica un patrón de codificación común en el que se asigna un valor a una variable si es NULL.

Como parte de esta propuesta, también se van a aflojar los requisitos de tipo en `??` para permitir una expresión cuyo tipo sea un parámetro de tipo sin restricciones que se va a usar en el lado izquierdo.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Es habitual ver el código del formulario

```csharp
if (variable == null)
{
    variable = expression;
}
```

Esta propuesta agrega un operador binario no sobrecargable al lenguaje que realiza esta función.

Hay al menos ocho solicitudes de comunidad independientes para esta característica.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Agregamos un nuevo formulario de operador de asignación

``` antlr
assignment_operator
    : '??='
    ;
```

Que sigue las [reglas semánticas existentes para los operadores de asignación compuesta](../../spec/expressions.md#compound-assignment), salvo que Elide la asignación si el lado izquierdo no es NULL. Las reglas para esta característica son las siguientes.

Dado `a ??= b`, donde `A` es el tipo de `a`, `B` es el tipo de `b`y `A0` es el tipo subyacente de `A` si `A` es un tipo de valor que acepta valores NULL:

1. Si `A` no existe o es un tipo de valor que no acepta valores NULL, se produce un error en tiempo de compilación.
2. Si `B` no se pueden convertir implícitamente a `A` o `A0` (si `A0` existe), se produce un error en tiempo de compilación.
3. Si `A0` existe y `B` es implícitamente convertible a `A0`y `B` no es dinámico, se `a ??= b` el tipo de `A0`. `a ??= b` se evalúa en tiempo de ejecución como:
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   Salvo que `a` solo se evalúa una vez.
4. De lo contrario, el tipo de `a ??= b` se `A`. `a ??= b` se evalúa en tiempo de ejecución como `a ?? (a = b)`, salvo que `a` solo se evalúa una vez.


Para la flexibilización de los requisitos de tipo de `??`, actualizamos la especificación en la que actualmente se indica que, en la `a ?? b`, donde `A` es el tipo de `a`:

> 1. Si existe y no es un tipo que acepta valores NULL o un tipo de referencia, se produce un error en tiempo de compilación.

Este requisito se reduce a:

1. Si existe y es un tipo de valor que no acepta valores NULL, se produce un error en tiempo de compilación.

Esto permite que el operador de uso combinado de NULL funcione en parámetros de tipo sin restricciones, ya que el parámetro de tipo sin restricciones T existe, no es un tipo que acepta valores NULL y no es un tipo de referencia.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Al igual que con cualquier característica de lenguaje, debemos cuestionar si se reembolsa la complejidad adicional en el idioma en la claridad adicional que se C# ofrece al cuerpo de los programas que se beneficiarían de la característica.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

El programador puede escribir `(x = x ?? y)`, `if (x == null) x = y;`o `x ?? (x = y)` a mano.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- [] Requiere la revisión de LDM
- [] ¿También se deben admitir los operadores `&&=` y `||=`?

## <a name="design-meetings"></a>Design Meetings

Ninguno.
