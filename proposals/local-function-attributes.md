---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509496"
---
# <a name="attributes-on-local-functions"></a>Atributos en funciones locales

## <a name="attributes"></a>Atributos

Ahora se permite que las declaraciones de función local tengan [atributos](../spec/attributes.md). Los parámetros y los parámetros de tipo de las funciones locales también pueden tener atributos.

Los atributos con un significado especificado cuando se aplican a un método, sus parámetros o sus parámetros de tipo tendrán el mismo significado cuando se aplican a una función local, sus parámetros o sus parámetros de tipo, respectivamente.

Una función local puede hacerse condicional en el mismo sentido que un [método condicional](../spec/attributes.md#the-conditional-attribute) al decorarla con un `[ConditionalAttribute]`. También se debe `static`una función local condicional. Todas las restricciones de los métodos condicionales también se aplican a las funciones locales condicionales, lo que incluye que el tipo de valor devuelto debe ser `void`.

## <a name="extern"></a>Externas

Ahora se permite el modificador `extern` en funciones locales. Esto hace que la función local sea externa en el mismo sentido que un [método externo](../spec/classes.md#external-methods).

De forma similar a un método externo, el cuerpo de la *función local* de una función local externa debe ser un punto y coma. Solo se permite un *cuerpo de función local* de punto y coma en una función local externa. 

También se debe `static`una función local externa.

## <a name="syntax"></a>Sintaxis

La [gramática de funciones locales](csharp-7.0/local-functions.md#syntax-grammar) se modifica de la siguiente manera:
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
