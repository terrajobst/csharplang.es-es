---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483509"
---
# <a name="out-variable-declarations"></a>Declaraciones de variable out

La característica de *declaración de variable out* permite declarar una variable en la ubicación en la que se pasa como un argumento `out`.

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

Una variable declarada de esta manera se denomina una *variable out*. Puede usar la palabra clave contextual `var` para el tipo de la variable. El ámbito será el mismo que para una *variable de patrón* introducida a través de la coincidencia de patrones.

Según la especificación del lenguaje (sección acceso al elemento 7.6.7), la lista de argumentos de un acceso a elementos (expresión de indización) no contiene argumentos Ref o out. Sin embargo, las permite el compilador para varios escenarios, por ejemplo, los indizadores declarados en metadatos que aceptan `out`.

Dentro del ámbito de una variable local introducida por un argument_value, es un error en tiempo de compilación hacer referencia a esa variable local en una posición textual que precede a su declaración.

También es un error hacer referencia a una variable out de tipo implícito (§ 8.5.1) en la misma lista de argumentos que contiene inmediatamente su declaración.

La resolución de sobrecarga se modifica de la siguiente manera:

Agregamos una nueva conversión:

> Existe una *conversión de la expresión* de una declaración de variable out implícitamente a cada tipo.

Asimismo

> El tipo de un argumento de variable out explícitamente escrito es el tipo declarado.

y

> Un argumento de variable out con tipo implícito no tiene ningún tipo.

La *conversión de la expresión* de una declaración de variable out implícitamente no se considera mejor que cualquier otra *conversión de la expresión*.

El tipo de una variable de salida con tipo implícito es el tipo del parámetro correspondiente en la firma del método seleccionado por la resolución de sobrecarga.

Se agrega el nuevo nodo de sintaxis `DeclarationExpressionSyntax` para representar la declaración en un argumento de salida var.
