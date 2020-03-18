---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484145"
---
# <a name="support-for--and--on-tuple-types"></a>Compatibilidad con = = y! = en tipos de tupla

Permite expresiones `t1 == t2` en las que `t1` y `t2` son tipos de tupla que aceptan valores NULL de tupla o que aceptan valores NULL y los evalúan aproximadamente como `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` (suponiendo `var temp1 = t1; var temp2 = t2;`).

Por el contrario, permitiría `t1 != t2` y la evaluaría como `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`.

En el caso que acepta valores NULL, se usan comprobaciones adicionales para `temp1.HasValue` y `temp2.HasValue`. Por ejemplo, `nullableT1 == nullableT2` se evalúa como `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`.

Cuando una comparación de elementos devuelve un resultado no bool (por ejemplo, cuando se utiliza un `operator ==` o `operator !=` definido por el usuario no bool, o en una comparación dinámica), ese resultado se convertirá en `bool` o se ejecutará a través de `operator true` o `operator false` para obtener una `bool`. La comparación de tupla siempre termina con la devolución de un `bool`.

A partir C# de 7,2, este código produce un error (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`), a menos que haya un `operator==`definido por el usuario.

## <a name="details"></a>Detalles

Al enlazar el operador `==` (o `!=`), las reglas existentes son: (1) caso dinámico, (2) resolución de sobrecarga y (3) producen un error.
Esta propuesta agrega un caso de tupla entre (1) y (2): si ambos operandos de un operador de comparación son tuplas (tienen tipos de tupla o son literales de tupla) y tienen una cardinalidad coincidente, la comparación se realiza a elemento. Esta igualdad de tupla también se eleva en tuplas que aceptan valores NULL.

Ambos operandos (y, en el caso de los literales de tupla, sus elementos) se evalúan en orden de izquierda a derecha. A continuación, cada par de elementos se utiliza como operandos para enlazar el operador `==` (o `!=`), de forma recursiva. Los elementos con el tipo en tiempo de compilación `dynamic` producen un error. Los resultados de esas comparaciones de elementos se usan como operandos en una cadena de operadores condicionales AND (or OR).

Por ejemplo, en el contexto de `(int, (int, int)) t1, t2;`, `t1 == (1, (2, 3))` se evaluaría como `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`.

Cuando se usa un literal de tupla como operando (en cualquiera de los dos lados), recibe un tipo de tupla convertido formado por las conversiones de elementos que se introducen al enlazar el operador `==` (o `!=`). 

Por ejemplo, en `(1L, 2, "hello") == (1, 2L, null)`, el tipo convertido para ambos literales de tupla es `(long, long, string)` y el segundo literal no tiene ningún tipo natural.


### <a name="deconstruction-and-conversions-to-tuple"></a>Desconstrucción y conversiones a tupla
En `(a, b) == x`, el hecho de que `x` puede deconstruir en dos elementos no desempeña un rol. Esto podría estar en una propuesta futura, aunque podría plantear preguntas sobre `x == y` (es una comparación simple o una comparación de elementos y, si así se usa la cardinalidad?).
Del mismo modo, las conversiones a tupla no juegan ningún rol.

### <a name="tuple-element-names"></a>Nombres de elementos de tupla

Al convertir un literal de tupla, se advierte cuando se proporciona un nombre de elemento de tupla explícito en el literal, pero no coincide con el nombre de elemento de la tupla de destino.
Usamos la misma regla en la comparación de tuplas, de modo que Supongamos `(int a, int b) t` se advierte de `d` en `t == (c, d: 0)`.

### <a name="non-bool-element-wise-comparison-results"></a>Resultados de la comparación de elementos no booleanos

Si una comparación de elementos es dinámica en una igualdad de tupla, usamos una invocación dinámica del operador `false` y niega eso para obtener un `bool` y continuar con comparaciones de elementos adicionales. 

Si una comparación de elementos devuelve algún otro tipo que no sea bool en una igualdad de tupla, hay dos casos:
- Si el tipo no bool se convierte en `bool`, se aplica esa conversión.
- Si no hay tal conversión, pero el tipo tiene un operador `false`, lo usaremos y negará el resultado.

En una desigualdad de tupla, se aplican las mismas reglas excepto que usaremos el operador `true` (sin negación) en lugar del operador `false`.

Estas reglas son similares a las reglas implicadas para usar un tipo que no sea bool en una instrucción `if` y otros contextos existentes.

## <a name="evaluation-order-and-special-cases"></a>Orden de evaluación y casos especiales
El valor del lado izquierdo se evalúa primero, después el valor del lado derecho, y, a continuación, las comparaciones de elementos de izquierda a derecha (incluidas las conversiones y con la salida temprana en función de las reglas existentes para los operadores condicionales y/o).

Por ejemplo, si hay una conversión del tipo `A` al tipo `B` y un método `(A, A) GetTuple()`, evaluar `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` significa:
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- a continuación, se evalúan las conversiones y comparaciones de elementos y la lógica condicional (convertir `new A(1)` al tipo `B`, compararlo con `new B(4)`, etc.).

### <a name="comparing-null-to-null"></a>Comparar `null` con `null`

Este es un caso especial de las comparaciones normales, que lleva a las comparaciones de tupla. Se permite la comparación de `null == null` y los literales de `null` no obtienen ningún tipo.
En igualdad de tupla, esto significa que también se permite `(0, null) == (0, null)` y los literales de `null` y tupla no obtienen un tipo.

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a>Comparar un struct que acepta valores NULL con `null` sin `operator==`

Este es otro caso especial de las comparaciones normales, que lleva a las comparaciones de tupla.
Si tiene un `struct S` sin `operator==`, se permite la comparación de `(S?)x == null` y se interpreta como `((S?).x).HasValue`.
En la igualdad de tupla, se aplica la misma regla, por lo que se permite `(0, (S?)x) == (0, null)`.

## <a name="compatibility"></a>Compatibilidad

Si alguien escribe sus propios tipos de `ValueTuple` con una implementación del operador de comparación, se habrían elegido previamente por la resolución de sobrecarga. Pero como el nuevo caso de tupla viene antes de la resolución de sobrecarga, se trataría este caso con la comparación de tupla en lugar de depender de la comparación definida por el usuario.

----

Está relacionado con los [operadores relacionales y de prueba de tipos](../../spec/expressions.md#relational-and-type-testing-operators) relacionados con [#190](https://github.com/dotnet/csharplang/issues/190)
