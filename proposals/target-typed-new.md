---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281975"
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

## <a name="specification"></a>Especificación
[design]: #detailed-design

Nueva forma sintáctica, *target_typed_new* de la *object_creation_expression* se acepta en la que el *tipo* es opcional.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

Una expresión *target_typed_new* no tiene un tipo. Sin embargo, hay una nueva *conversión de creación de objetos* que es una conversión implícita de la expresión, que existe de una *target_typed_new* a cada tipo.

Dado un tipo de destino `T`, el `T0` de tipo es el tipo subyacente `T`si `T` es una instancia de `System.Nullable`. De lo contrario `T0` es `T`. El significado de una expresión *target_typed_new* que se convierte al tipo `T` es igual que el significado de una *object_creation_expression* correspondiente que especifica `T0` como el tipo.

Es un error en tiempo de compilación si se utiliza un *target_typed_new* como operando de un operador unario o binario, o si se utiliza cuando no está sujeto a una *conversión de creación de objeto*.

> **Problema de apertura:** ¿se deben permitir los delegados y las tuplas como tipo de destino?

Las reglas anteriores incluyen delegados (un tipo de referencia) y tuplas (un tipo de estructura). Aunque ambos tipos se pueden construir, si el tipo es inferido, ya se puede usar una función anónima o un literal de tupla.
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a>Varios

Las siguientes son las consecuencias de la especificación:

- se permite `throw new()` (el tipo de destino es `System.Exception`)
- No se permite el `new` con tipo de destino con operadores binarios.
- No se permite cuando no hay ningún tipo para el destino: operadores unarios, colección de un `foreach`, en un `using`, en una desconstrucción, en una expresión de `await`, como una propiedad de tipo anónimo (`new { Prop = new() }`), en una instrucción `lock`, en una `sizeof`, en una instrucción `fixed`, en un acceso de miembro (`new().field`), en una operación distribuida dinámicamente (`someDynamic.Method(new())`), en una consulta LINQ, como operando del operador `is`, como operando izquierdo del operador `??` ,  ...
- También se deniega como `ref`.
- No se permiten los siguientes tipos de tipos como destinos de la conversión
  - **Tipos de enumeración:** `new()` funcionará (a medida que `new Enum()` funcione para proporcionar el valor predeterminado), pero `new(1)` no funcionará porque los tipos de enumeración no tienen un constructor.
  - **Tipos de interfaz:** Esto funcionaría igual que la expresión de creación correspondiente para los tipos COM.
  - **Tipos de matriz:** las matrices necesitan una sintaxis especial para proporcionar la longitud.    
  - **dinámico:** no se permite el `new dynamic()`, por lo que no se permite el `new()` con `dynamic` como tipo de destino.
  - **tuplas:** Tienen el mismo significado que la creación de un objeto utilizando el tipo subyacente.
  - Todos los demás tipos que no se permiten en el *object_creation_expression* también se excluyen, por ejemplo, los tipos de puntero.   

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Había algunos problemas con el tipo de destino `new` crear nuevas categorías de cambios importantes, pero ya tenemos eso con `null` y `default`, y que no ha sido un problema importante.

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
- [LDM: 2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM: 2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM: 2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM: 2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [LDM: 2020-03-25](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
