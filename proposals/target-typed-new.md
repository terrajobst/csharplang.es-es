---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483533"
---

# <a name="target-typed-new-expressions"></a>Expresiones de `new` con tipo de destino

* [x] propuesto
* [x] [prototipo](https://github.com/alrz/roslyn/tree/features/target-typed-new)
* [] Implementación
* [] Especificación

## <a name="summary"></a>Resumen
[summary]: #summary

No es necesario especificar la especificación de tipo para los constructores cuando se conoce el tipo. 

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Permite la inicialización de campos sin duplicar el tipo.
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
Permite omitir el tipo cuando se puede inferir del uso.
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
Cree una instancia de un objeto sin deletrear el tipo.
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

La sintaxis de *object_creation_expression* se modificaría para que el *tipo* fuera opcional cuando haya paréntesis. Esto es necesario para resolver la ambigüedad con *anonymous_object_creation_expression*.
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
Un `new` con tipo de destino se pueda convertir a cualquier tipo. Como resultado, no contribuye a la resolución de sobrecarga. Esto es principalmente para evitar cambios importantes imprevisibles.

La lista de argumentos y las expresiones de inicializador se enlazarán después de que se determine el tipo.

El tipo de la expresión se deduce del tipo de destino, que sería necesario que sea uno de los siguientes:

- **Cualquier tipo de struct**
- **Cualquier tipo de referencia**
- **Cualquier parámetro de tipo** con un constructor o una restricción `struct`

con las siguientes excepciones:

- **Tipos de enumeración:** no todos los tipos de enumeración contienen la constante cero, por lo que debe ser conveniente usar el miembro de enumeración explícito.
- **Tipos de interfaz:** se trata de una característica de nicho y debe ser preferible mencionar explícitamente el tipo.
- **Tipos de matriz:** las matrices necesitan una sintaxis especial para proporcionar la longitud.
- **Constructor predeterminado de struct**: esta regla extrae todos los tipos primitivos y la mayoría de los tipos de valor. Si desea utilizar el valor predeterminado de estos tipos, podría escribir `default` en su lugar.

Todos los demás tipos que no se permiten en el *object_creation_expression* también se excluyen, por ejemplo, los tipos de puntero.

> **Problema de apertura:** ¿se deben permitir los delegados y las tuplas como tipo de destino?

Las reglas anteriores incluyen delegados (un tipo de referencia) y tuplas (un tipo de estructura). Aunque ambos tipos se pueden construir, si el tipo es inferido, ya se puede usar una función anónima o un literal de tupla.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> **Problema de apertura:** ¿se debe permitir `throw new()` con `Exception` como tipo de destino?

Tenemos `throw null` hoy, pero no `throw default` (aunque tendría el mismo efecto). Por otro lado, `throw new()` podría ser realmente útil como una forma abreviada de `throw new Exception(...)`. Tenga en cuenta que la especificación actual ya lo permite. `Exception` es un tipo de referencia y la especificación de la instrucción throw indica que la expresión se convierte en `Exception`.

> **Problema de apertura:** ¿se deben permitir los usos de `new` con tipo de destino con operadores aritméticos y de comparación definidos por el usuario?

Para la comparación, `default` solo admite operadores de igualdad (definidos por el usuario e integrados). ¿Tendría sentido admitir también otros operadores para `new()`?

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Ninguno.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

La mayoría de las quejas acerca de los tipos que son demasiado largos para duplicarlos en la inicialización de campos es acerca de los *argumentos de tipo* que no son del tipo en sí, solo se podían inferir argumentos de tipo como `new Dictionary(...)` (o similares) e inferir argumentos de tipo localmente a partir de argumentos o del inicializador de colección.

## <a name="questions"></a>Preguntas
[questions]: #questions

- ¿Se deben prohibir los usos en los árboles de expresión? ninguno
- ¿Cómo interactúa la característica con `dynamic` argumentos? (sin tratamiento especial)
- ¿Cómo debería funcionar IntelliSense con `new()`? (solo cuando hay un solo tipo de destino)
## <a name="design-meetings"></a>Design Meetings

- [LDM: 2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
