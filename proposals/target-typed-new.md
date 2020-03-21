---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108379"
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

- **Cualquier tipo de struct** (incluidos los tipos de tupla)
- **Cualquier tipo de referencia** (incluidos los tipos de delegado)
- **Cualquier parámetro de tipo** con un constructor o una restricción `struct`

con las siguientes excepciones:

- **Tipos de enumeración:** no todos los tipos de enumeración contienen la constante cero, por lo que debe ser conveniente usar el miembro de enumeración explícito.
- **Tipos de interfaz:** se trata de una característica de nicho y debe ser preferible mencionar explícitamente el tipo.
- **Tipos de matriz:** las matrices necesitan una sintaxis especial para proporcionar la longitud.
- **dinámico:** no se permite el `new dynamic()`, por lo que no se permite el `new()` con `dynamic` como tipo de destino.

Todos los demás tipos que no se permiten en el *object_creation_expression* también se excluyen, por ejemplo, los tipos de puntero.

Cuando el tipo de destino es un tipo de valor que acepta valores NULL, el `new` con tipo de destino se convertirá en el tipo subyacente en lugar del tipo que acepta valores NULL.

> **Problema de apertura:** ¿se deben permitir los delegados y las tuplas como tipo de destino?

Las reglas anteriores incluyen delegados (un tipo de referencia) y tuplas (un tipo de estructura). Aunque ambos tipos se pueden construir, si el tipo es inferido, ya se puede usar una función anónima o un literal de tupla.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
