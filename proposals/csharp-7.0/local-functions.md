---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483449"
---
# <a name="local-functions"></a>Funciones locales

Ampliamos C# para admitir la declaración de funciones en el ámbito de bloque. Las funciones locales pueden utilizar (Capture) variables del ámbito de inclusión.

El compilador usa el análisis de flujo para detectar las variables que usa una función local antes de asignarle un valor. Cada llamada de la función requiere que estas variables se asignen de forma definitiva. Del mismo modo, el compilador determina qué variables se asignan definitivamente en la devolución. Estas variables se consideran definitivamente asignadas después de invocar la función local.

Se puede llamar a las funciones locales desde un punto léxico antes de su definición. Las instrucciones de declaración de funciones locales no provocan una advertencia cuando no se puede tener acceso a ellas.

TODO: _escribir especificación_

## <a name="syntax-grammar"></a>Gramática de sintaxis

Esta gramática se representa como una diferencia de la gramática de especificación actual.

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

Las funciones locales pueden usar variables definidas en el ámbito de inclusión. La implementación actual requiere que todas las variables leídas dentro de una función local se asignen definitivamente, como si se ejecutara la función local en su punto de definición. Además, la definición de función local se debe haber "ejecutado" en cualquier punto de uso.

Después de experimentar con ese bit (por ejemplo, no es posible definir dos funciones locales recursivas mutuamente), hemos revisado cómo deseamos que funcione la asignación definitiva. La revisión (aún no implementada) es que todas las variables locales leídas en una función local deben asignarse definitivamente en cada invocación de la función local. Eso es realmente más sutil que el sonido y hay una gran cantidad de trabajo que queda para que funcione. Una vez que lo haya hecho, podrá trasladar las funciones locales al final de su bloque de inclusión.

Las nuevas reglas de asignación definitiva son incompatibles con la deducción del tipo de valor devuelto de una función local, por lo que es probable que se quite la compatibilidad para deducir el tipo de valor devuelto.

A menos que se convierta una función local en un delegado, la captura se realiza en marcos que son tipos de valor. Esto significa que no obtiene ninguna presión de GC del uso de funciones locales con captura.

### <a name="reachability"></a>Accesibilidad

Agregamos a la especificación

> El cuerpo de una expresión lambda con cuerpo de instrucción o función local se considera alcanzable.
