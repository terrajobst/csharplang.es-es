---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483527"
---
# <a name="nullable-enhanced-common-type"></a>Tipo común mejorado con valores NULL

* [x] propuesto
* [] Prototipo: ninguno
* [] Implementación: ninguno
* [] Especificación: vea a continuación

## <a name="summary"></a>Resumen
[summary]: #summary

Existe una situación en la que el resultado del algoritmo de tipo común actual es un contador intuitivo y hace que el programador agregue lo que se parece a una conversión redundante en el código. Con este cambio, una expresión como `condition ? 1 : null` daría como resultado un valor de tipo `int?`y una expresión como `condition ? x : 1.0` donde `x` es de tipo `int?` daría como resultado un valor de tipo `double?`.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Se trata de una causa común de lo que le parece al programador, como el código reutilizable no necesario.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Se modifica la especificación para [Buscar el mejor tipo común de un conjunto de expresiones](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) que afecten a las siguientes situaciones:

- Si una expresión es de un tipo de valor que no acepta valores NULL `T` y la otra es un literal null, el resultado es de tipo `T?`.
- Si una expresión es de un tipo de valor que acepta valores NULL `T?` y la otra es de un tipo de valor `U`, y hay una conversión implícita de `T` a `U`, el resultado es de tipo `U?`.

Se espera que afecte a los siguientes aspectos del lenguaje:

- [expresión ternaria](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)
- expresión de creación de [matriz](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions) con tipo implícito
- inferir el [tipo de valor devuelto de una expresión lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) para la inferencia de tipos
- los casos que implican genéricos, como invocar `M<T>(T a, T b)` como `M(1, null)`.

Más concretamente, cambiamos las siguientes secciones de la especificación (inserciones en negrita, eliminaciones en tachado):

> #### <a name="output-type-inferences"></a>Inferencias de tipos de salida
> 
> Una *inferencia de tipo de salida* se realiza *desde* una expresión `E` *a* un tipo `T` de la siguiente manera:
> 
> *  Si `E` es una función anónima con el tipo de valor devuelto inferido `U` ([tipo de valor](expressions.md#inferred-return-type)devuelto deducido) y `T` es un tipo de delegado o un tipo de árbol de expresión con el tipo de valor devuelto `Tb`, se realiza una *inferencia de límite inferior* ([inferencias con límite inferior](expressions.md#lower-bound-inferences)) *de* `U` *a* `Tb`.
> *  De lo contrario, si `E` es un grupo de métodos y `T` es un tipo de delegado o un tipo de árbol de expresión con tipos de parámetro `T1...Tk` y `Tb`de tipo de valor devuelto, y la resolución de sobrecarga de `E` con los tipos `T1...Tk` produce un único método con el tipo de valor devuelto `U`, se realiza una *inferencia con un límite inferior* *desde* `U` *a* `Tb`.
> *  \* * *De* lo contrario, si `E` es una expresión con un tipo de valor que acepta valores NULL `U?`, se realiza una *inferencia con límite inferior* desde `U` *a* `T` y se agrega un *enlace nulo* a `T`. **
> *  De lo contrario, si `E` es una expresión con el tipo `U`, se realiza una *inferencia de enlace inferior* *desde* `U` *a* `T`.
> *  **De lo contrario, si `E` es una expresión constante con el valor `null`, se agrega un *enlace nulo* a `T`** 
> *  De lo contrario, no se realiza ninguna inferencia.

> #### <a name="fixing"></a>Corrección de
> 
> Una variable de tipo sin *fixed* `Xi` con un conjunto de límites se *fija* de la siguiente manera:
> 
> *  El conjunto de *tipos candidatos* `Uj` comienza como el conjunto de todos los tipos del conjunto de límites de `Xi`.
> *  A continuación, examinaremos cada límite por `Xi` a su vez: para cada `U` enlazado exacto de `Xi` todos los tipos `Uj` que no son idénticos a `U` se quitan del conjunto de candidatos. Para cada `U` de límite inferior de `Xi` todos los tipos `Uj` en los que *no* se ha quitado una conversión implícita de `U` del conjunto de candidatos. Para cada `U` de enlace superior de `Xi` todos los tipos `Uj` desde el que *no* se quita una conversión implícita a `U` del conjunto de candidatos.
> *  Si está entre los tipos de candidatos restantes `Uj` hay un tipo único `V` desde el que hay una conversión implícita a todos los demás tipos candidatos, ~~`Xi` se fija en `V`.~~
>     -  **Si `V` es un tipo de valor y hay un *enlace nulo* para `Xi`, `Xi` se fija en `V?`**
>     -  **De lo contrario `Xi` se fija en `V`**
> *  De lo contrario, se produce un error en la inferencia de tipos.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Puede haber algunas incompatibilidades introducidas en esta propuesta.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Ninguno.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- [] ¿Cuál es la gravedad de incompatibilidad introducida en esta propuesta, si la hubiera, y cómo se puede moderar?

## <a name="design-meetings"></a>Design Meetings

Ninguno.
