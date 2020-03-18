---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483587"
---
# <a name="ref-local-reassignment"></a>Reasignación local de referencia

En C# 7,3, se agrega compatibilidad para reenlazar el referente de una variable local de referencia o un parámetro ref.

Agregamos lo siguiente al conjunto de `assignment_operator`s.

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

El operador `=ref` se denomina ***operador de asignación Ref***. No es un *operador de asignación compuesta*. El operando izquierdo debe ser una expresión que se enlaza a una variable local ref, un parámetro ref (que no sea `this`) o un parámetro out. El operando derecho debe ser una expresión que produzca un valor l que designe un valor del mismo tipo que el operando izquierdo.

El operando derecho debe estar asignado definitivamente en el punto de la asignación de referencia.

Cuando el operando izquierdo se enlaza a un parámetro `out`, es un error si ese parámetro `out` no se ha asignado definitivamente al principio del operador de asignación Ref.

Si el operando izquierdo es una referencia grabable (es decir, designa algo distinto de un parámetro `ref readonly` local o de `in`), el operando derecho debe ser un valor l grabable.

El operador de asignación de referencia produce un valor l del tipo asignado. Es grabable si el operando izquierdo es grabable (es decir, no `ref readonly` ni `in`).

Las reglas de seguridad para este operador son:

- En el caso de una `e1 = ref e2`de reasignación de referencia, el *parámetro ref-Safe-to-escape* de `e2` debe ser al menos tan ancho como el de la *referencia-Safe-to-escape* de `e1`.

Donde *ref-Safe-to-escape* se define en [seguridad para los tipos de tipo REF-like](../csharp-7.2/span-safety.md)
