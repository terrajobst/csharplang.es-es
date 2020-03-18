---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2019
ms.locfileid: "79484049"
---
# <a name="permit-stackalloc-in-nested-contexts"></a>Permitir `stackalloc` en contextos anidados

### <a name="stack-allocation"></a>Asignación de la pila

Se modifica la sección [*asignación*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) de la pila C# de la especificación del lenguaje para relajar los lugares en los que puede aparecer una expresión `stackalloc`. Eliminamos

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

y reemplácelas por

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

Tenga en cuenta que la adición de una *array_initializer* a *stackalloc_initializer* (y que la expresión de índice es opcional) era una [extensión en C# 7,3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) y no se describe aquí.

El *tipo de elemento* de la expresión `stackalloc` es el *unmanaged_type* denominado en la expresión stackalloc, si existe, o el tipo común entre los elementos de la *array_initializer* en caso contrario.

El tipo de *stackalloc_initializer* con el *tipo de elemento* `K` depende de su contexto sintáctico:
- Si el *stackalloc_initializer* aparece directamente como *local_variable_initializer* de una instrucción de *local_variable_declaration* o un *for_initializer*, su tipo es `K*`.
- De lo contrario, su tipo es `System.Span<K>`.

### <a name="stackalloc-conversion"></a>Conversión stackalloc

La *conversión stackalloc* es una nueva conversión implícita integrada de la expresión. Cuando se `K*`el tipo de una *stackalloc_initializer* , hay una *conversión stackalloc* implícita desde el *stackalloc_initializer* al `System.Span<K>`de tipo.
