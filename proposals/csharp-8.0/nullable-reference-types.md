---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483599"
---
# <a name="nullable-reference-types-in-c"></a>Tipos de referencia que aceptan valores NULL enC# #

El objetivo de esta característica es:

* Permite a los desarrolladores expresar si una variable, un parámetro o un resultado de un tipo de referencia está pensado para ser null o no.
* Proporcionar advertencias cuando tales variables, parámetros y resultados no se usan según esa intención.

## <a name="expression-of-intent"></a>Expresión de intención

El lenguaje ya contiene la sintaxis de `T?` para los tipos de valor. Es sencillo extender esta sintaxis a tipos de referencia.

Se supone que el intento de un tipo de referencia no adornado `T` es que no sea NULL.

## <a name="checking-of-nullable-references"></a>Comprobando referencias que aceptan valores NULL

Un análisis de flujo realiza un seguimiento de las variables de referencia que aceptan valores NULL. Si el análisis considera que no sería null (por ejemplo, después de una comprobación o una asignación), su valor se considerará una referencia no NULL.

Una referencia que acepta valores null también se puede tratar explícitamente como no NULL con el operador de `x!` postfijo (el operador "Dammit"), para cuando el análisis de flujo no pueda establecer una situación que no sea NULL y sepa que el desarrollador está allí.

De lo contrario, se proporciona una advertencia si se desreferencia una referencia que acepta valores NULL o si se convierte en un tipo que no sea NULL.

Se proporciona una advertencia al convertir de `S[]` a `T?[]` y de `S?[]` a `T[]`.

Se proporciona una advertencia al convertir de `C<S>` a `C<T?>` excepto cuando el parámetro de tipo es covariante (`out`), y al convertir de `C<S?>` a `C<T>` excepto cuando el parámetro de tipo es contravariante (`in`).

Se proporciona una advertencia en `C<T?>` si el parámetro de tipo tiene restricciones que no son NULL. 

## <a name="checking-of-non-null-references"></a>Comprobando referencias no nulas

Se proporciona una advertencia si un literal NULL se asigna a una variable que no es null o se pasa como un parámetro que no es NULL.

También se proporciona una advertencia si un constructor no inicializa explícitamente los campos de referencia no NULL.

No se puede realizar un seguimiento adecuado de que se inicializan todos los elementos de una matriz de referencias no nulas. Sin embargo, podríamos emitir una advertencia si no se asigna ningún elemento de una matriz recién creada antes de que la matriz se lea o se pase. Esto podría controlar el caso común sin ser demasiado ruidoso.

Necesitamos decidir si `default(T)` genera una advertencia o simplemente se trata como el tipo `T?`.

## <a name="metadata-representation"></a>Representación de metadatos

Los elementos gráficos de nulabilidad se deben representar en metadatos como atributos. Esto significa que los compiladores de nivel inferior lo ignorarán.

Necesitamos decidir si solo se incluyen las anotaciones que aceptan valores NULL, o si hay también alguna indicación de si el valor de "ON" no es null en el ensamblado.

## <a name="generics"></a>Genéricos

Si un parámetro de tipo `T` tiene restricciones que no aceptan valores NULL, se trata como un valor que no acepta valores NULL dentro de su ámbito.

Si un parámetro de tipo no está restringido o solo tiene restricciones que aceptan valores NULL, la situación es un poco más compleja: Esto significa que el argumento de tipo correspondiente podría admitir valores NULL o que no admita *valores NULL.* Lo más seguro que hacer en esa situación es tratar el *parámetro de tipo como valores* NULL y que no aceptan valores NULL, lo que permite recibir advertencias cuando se infringe cualquiera de ellos. 

Merece la pena considerar si se deben permitir las restricciones de referencia explícitas que aceptan valores NULL. Tenga en cuenta, sin embargo, que no se puede evitar que los tipos de referencia que aceptan valores NULL sean restricciones de forma *implícita* en ciertos casos (restricciones heredadas).

La restricción `class` no es NULL. Podemos considerar si `class?` debe ser una restricción válida que acepte valores NULL que indique "tipo de referencia que acepta valores NULL".

## <a name="type-inference"></a>Inferencia de tipos

En la inferencia de tipos, si un tipo de contribución es un tipo de referencia que acepta valores NULL, el tipo resultante debe admitir valores NULL. En otras palabras, se propaga la nulación.

Debemos tener en cuenta si el `null` literal como una expresión participante debe aportar null. En la actualidad: para los tipos de valor, conduce a un error, mientras que en los tipos de referencia el valor NULL se convierte correctamente en el tipo simple. 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a>Últimos cambios

Las advertencias que no son NULL son un cambio importante evidente en el código existente y deben ir acompañados de un mecanismo de participación.

Menos obvio, las advertencias de tipos que aceptan valores NULL (como se describió anteriormente) son un cambio importante en el código existente en ciertos escenarios en los que la nulabilidad es implícita:

* Los parámetros de tipo sin restricciones se tratarán como Nullable implícitamente, por lo que asignarlos a `object` o tener acceso a, por ejemplo, `ToString` producirá advertencias.
* Si la inferencia de tipos infiere la nulidad de `null` expresiones, a veces el código existente producirá valores NULL en lugar de tipos que no aceptan valores NULL, lo que puede dar lugar a nuevas advertencias.

Por lo tanto, las advertencias que aceptan valores null también deben ser opcionales

Por último, la adición de anotaciones a una API existente será un cambio importante para los usuarios que hayan participado en las advertencias al actualizar la biblioteca. Esto también merece la posibilidad de participar o no. "Quiero correcciones de errores, pero no estoy listo para tratar con sus nuevas anotaciones"

En Resumen, debe ser capaz de participar o no de:
* Advertencias que aceptan valores NULL
* Advertencias no nulas
* Advertencias de anotaciones en otros archivos

La granularidad de la participación sugiere un modelo similar a un analizador, en el que el usuario puede elegir y deshabilitar swaths de código con las directivas pragma y los niveles de gravedad. Además, las opciones por biblioteca ("omitir las anotaciones de JSON.NET hasta que esté listo para tratar con el descenso") pueden expresarse en el código como atributos.

El diseño de la experiencia de participación o transición es fundamental para el éxito y la utilidad de esta característica. Debemos asegurarnos de que:

* Los usuarios pueden adoptar la comprobación de la nulabilidad gradualmente como deseen
* Los autores de bibliotecas pueden agregar anotaciones de nulabilidad sin miedo a los clientes de la interrupción
* A pesar de esto, no hay una idea de la "pesadilla de configuración".

## <a name="tweaks"></a>Ajustes

Podríamos considerar no usar las anotaciones de `?` en variables locales, sino que solo observa si se usan de acuerdo con lo que se les asigna. No me gusta esto; Creo que deberíamos permitir que los usuarios expresen su intención.

Podríamos considerar una `T! x` abreviada en los parámetros, que genera automáticamente una comprobación de NULL en tiempo de ejecución.

Algunos patrones de tipos genéricos, como `FirstOrDefault` o `TryGet`, tienen un comportamiento ligeramente extraño con argumentos de tipo que no aceptan valores NULL, ya que producen explícitamente valores predeterminados en determinadas situaciones. Podríamos intentar maticar el sistema de tipos para acomodarlos mejor. Por ejemplo, podríamos permitir `?` en parámetros de tipo sin restricciones, aunque el argumento de tipo ya podría aceptar valores NULL. No dude en que merezca la pena, y conduce a la extrañaidad relacionada con la interacción con tipos de *valor* que aceptan valores NULL. 

## <a name="nullable-value-types"></a>Tipos de valor que aceptan valores NULL

Podríamos considerar adoptar también algunas de las semánticas anteriores para los tipos de valor que aceptan valores NULL.

Ya hemos mencionado la inferencia de tipos, donde podríamos deducir `int?` de `(7, null)`, en lugar de simplemente producir un error.

Otra posibilidad es aplicar el análisis de flujo a tipos de valor que aceptan valores NULL. Cuando se consideran no NULL, en realidad podríamos permitir el uso de como tipo que no acepta valores NULL de ciertas maneras (por ejemplo, acceso a miembros). Simplemente tenemos que tener cuidado de que se prefieran las cosas que *ya* puede realizar en un tipo de valor que acepta valores NULL, por motivos de compatibilidad con las copias de seguridad.
