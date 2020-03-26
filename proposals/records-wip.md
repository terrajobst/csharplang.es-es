---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281962"
---
# <a name="records-work-in-progress"></a>Registra el trabajo en curso

A diferencia de las demás propuestas de registros, esto no es una propuesta en sí misma, sino un trabajo en curso diseñado para registrar las decisiones de diseño de consenso para la característica de registros. Los detalles de la especificación se agregarán según sea necesario para resolver las preguntas.

La sintaxis de un registro se propone para agregarse de la siguiente manera:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

El `attributes` no terminal también permitirá un nuevo atributo contextual, `data`.

Una clase (struct) declarada con una lista de parámetros o un modificador de `data` se denomina clase de registro (struct de registros), cualquiera de los cuales es un tipo de registro.

Es un error declarar un tipo de registro sin una lista de parámetros y el modificador `data`.

## <a name="members-of-a-record-type"></a>Miembros de un tipo de registro

Además de los miembros declarados en el cuerpo de clase, un tipo de registro tiene los siguientes miembros adicionales:

### <a name="primary-constructor"></a>Constructor principal

Un tipo de registro tiene un constructor público cuya firma corresponde a los parámetros de valor de la declaración de tipos. Esto se denomina constructor principal para el tipo y hace que se suprima el constructor predeterminado declarado implícitamente. Es un error tener un constructor principal y un constructor con la misma firma ya presente en la clase.
En tiempo de ejecución del constructor principal 

1. ejecuta los inicializadores de campo de instancia que aparecen en el cuerpo de la clase; y, a continuación, invoca el constructor de clase base sin argumentos.

1. Inicializa los campos de respaldo generados por el compilador para las propiedades correspondientes a los parámetros de valor (si estas propiedades son proporcionadas por el compilador; consulte [propiedades sintetizadas](#Synthesized Properties)).


[] TODO: agregar sintaxis de llamada base y especificación sobre cómo elegir el constructor base mediante la resolución de sobrecarga

### <a name="properties"></a>Propiedades

Para cada parámetro de registro de una declaración de tipo de registro, hay un miembro de propiedad público correspondiente cuyo nombre y tipo se toman de la declaración del parámetro value. Si no hay ninguna propiedad concreta (es decir, no abstracta) con un descriptor de acceso get y con este nombre y tipo se declara o hereda explícitamente, el compilador lo genera de la manera siguiente:

Para una clase de registro o de registro:

* Se crea una propiedad automática de solo obtención pública. Su valor se inicializa durante la construcción con el valor del parámetro del constructor principal correspondiente. Se invalida cada descriptor de acceso get de la propiedad abstracta heredada "matching".

### <a name="equality-members"></a>Miembros de igualdad

Los tipos de registros producen implementaciones sintetizadas para los métodos siguientes:

* invalidación de `object.GetHashCode()`, a menos que esté sellada o proporcionada por el usuario
* invalidación de `object.Equals(object)`, a menos que esté sellada o proporcionada por el usuario
* `T Equals(T)` método, donde `T` es el tipo actual

`T Equals(T)` se especifica para realizar la igualdad de valores, comparando la propiedad con el mismo nombre que cada parámetro del constructor principal con la propiedad correspondiente del otro tipo.
`object.Equals` realiza el equivalente de

```C#
override Equals(object o) => Equals(o as T);
```

## <a name="with-expression"></a>Expresión `with`

Una expresión de `with` es una expresión nueva con la sintaxis siguiente.

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

Una expresión `with` permite la "mutación no destructiva", diseñada para producir una copia de la expresión del receptor con modificaciones en las propiedades enumeradas en el `anonymous_object_initializer`.

Una expresión `with` válida tiene un receptor con un tipo que no es void. El tipo de receptor debe contener un método de instancia accesible denominado `With` con los parámetros adecuados y el tipo de valor devuelto. Es un error si hay varios métodos `With` no invalidados. Si hay varias invalidaciones de `With`, debe haber un método de `With` no invalidado, que es el método de destino. De lo contrario, debe haber exactamente un método `With`.

En el lado derecho de la expresión de `with` es una `anonymous_object_initializer` con una secuencia de asignaciones con un campo o propiedad del receptor en el lado izquierdo de la asignación, y una expresión arbitraria en el lado derecho, que se puede convertir implícitamente al tipo del lado izquierdo del mismo.

Dado un método de `With` de destino, el tipo de valor devuelto debe ser el tipo del tipo de expresión de receptor o un tipo base de este. Para cada parámetro del método `With`, debe haber un campo de instancia correspondiente accesible o una propiedad legible en el tipo de receptor con el mismo nombre y el mismo tipo. Cada propiedad o campo del lado derecho de la expresión with también debe corresponder a un parámetro con el mismo nombre en el método `With`.

Dado un método `With` válido, la evaluación de una expresión de `with` es equivalente a llamar al método `With` con las expresiones de la `anonymous_object_initializer` sustituida por el parámetro con el mismo nombre que la propiedad del lado izquierdo. Si no hay ninguna propiedad coincidente para un parámetro determinado en el `anonymous_object_initializer`, el argumento es la evaluación del campo o propiedad del mismo nombre en el receptor.

El orden de evaluación de los efectos secundarios es el siguiente, y cada expresión se evalúa exactamente una vez:

1. Expresión de receptor

2. Expresiones en el `anonymous_object_initializer`, en orden léxico

3. Evaluación de las propiedades que coinciden con los parámetros del método `With`, en orden de definición de los parámetros del método `With`.