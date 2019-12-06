---
ms.openlocfilehash: 4d6d28a3127bc701867afe157aa5496377a06f69
ms.sourcegitcommit: 63d276488c9770a565fd787020783ffc1d2af9d6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868009"
---
# <a name="conversions"></a>Conversiones

Una ***conversión*** permite que una expresión se trate de un tipo determinado. Una conversión puede hacer que una expresión de un tipo determinado se trate como si tuviera un tipo diferente o que una expresión sin un tipo obtenga un tipo. Las conversiones pueden ser ***implícitas*** o ***explícitas***, y esto determina si se requiere una conversión explícita. Por ejemplo, la conversión del tipo `int` al tipo `long` es implícita, por lo que las expresiones de tipo `int` se pueden tratar implícitamente como `long`de tipo. La conversión opuesta, del tipo `long` al tipo `int`, es explícita y, por tanto, se requiere una conversión explícita.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Algunas conversiones están definidas por el lenguaje. Los programas también pueden definir sus propias conversiones ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)).

## <a name="implicit-conversions"></a>Conversiones implícitas

Las conversiones siguientes se clasifican como conversiones implícitas:

*  Conversiones de identidad
*  Conversiones numéricas implícitas
*  Conversiones de enumeración IMPLÍCITAS
*  Conversiones de cadenas interpoladas IMPLÍCITAS
*  Conversiones implícitas que aceptan valores NULL
*  Conversiones de literales null
*  Conversiones de referencias implícitas
*  Conversiones Boxing
*  Conversiones dinámicas IMPLÍCITAS
*  Conversiones implícitas de expresiones constantes
*  Conversiones implícitas definidas por el usuario
*  Conversiones de funciones anónimas
*  Conversiones de grupos de métodos

Las conversiones implícitas pueden producirse en diversas situaciones, incluidas las invocaciones de miembros de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), las expresiones de conversión ([expresiones de conversión](expressions.md#cast-expressions)) y las asignaciones (operadores de[asignación](expressions.md#assignment-operators)).

Las conversiones implícitas predefinidas siempre se realizan correctamente y nunca provocan que se inicien excepciones. Las conversiones implícitas definidas por el usuario diseñadas correctamente deberían presentar también estas características.

En lo que respecta a la conversión, los tipos `object` y `dynamic` se consideran equivalentes.

Sin embargo, las conversiones dinámicas (conversiones dinámicas[implícitas](conversions.md#implicit-dynamic-conversions) y [conversiones dinámicas explícitas](conversions.md#explicit-dynamic-conversions)) solo se aplican a las expresiones de tipo `dynamic` ([el tipo dinámico](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Conversión de identidad

Una conversión de identidad convierte de cualquier tipo al mismo tipo. Esta conversión existe de manera que se puede decir que una entidad que ya tiene un tipo necesario se puede convertir en ese tipo.

*  Dado que `object` y `dynamic` se consideran equivalentes, hay una conversión de identidad entre `object` y `dynamic`, y entre los tipos construidos que son iguales al reemplazar todas las apariciones de `dynamic` por `object`.

### <a name="implicit-numeric-conversions"></a>Conversiones numéricas implícitas

Las conversiones numéricas implícitas son:

*  De `sbyte` a `short`, `int`, `long`, `float`, `double`o `decimal`.
*  De `byte` a `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`o `decimal`.
*  De `short` a `int`, `long`, `float`, `double`o `decimal`.
*  De `ushort` a `int`, `uint`, `long`, `ulong`, `float`, `double`o `decimal`.
*  De `int` a `long`, `float`, `double`o `decimal`.
*  De `uint` a `long`, `ulong`, `float`, `double`o `decimal`.
*  De `long` a `float`, `double`o `decimal`.
*  De `ulong` a `float`, `double`o `decimal`.
*  De `char` a `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`o `decimal`.
*  De `float` a `double`.

Las conversiones de `int`, `uint`, `long`o `ulong` a `float` y desde `long` o `ulong` a `double` pueden provocar una pérdida de precisión, pero nunca producirán una pérdida de magnitud. El resto de conversiones numéricas implícitas nunca pierden información.

No hay conversiones implícitas al tipo de `char`, por lo que los valores de los otros tipos enteros no se convierten automáticamente al tipo de `char`.

### <a name="implicit-enumeration-conversions"></a>Conversiones de enumeración IMPLÍCITAS

Una conversión de enumeración implícita permite convertir el `0` de *decimal_integer_literal* en cualquier *enum_type* y en cualquier *nullable_type* cuyo tipo subyacente sea una *enum_type*. En el último caso, la conversión se evalúa convirtiendo en el *enum_type* subyacente y ajustando el resultado ([tipos que aceptan valores NULL](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Conversiones de cadenas interpoladas IMPLÍCITAS

Una conversión de cadena interpolada implícita permite convertir un *interpolated_string_expression* ([cadenas interpoladas](expressions.md#interpolated-strings)) en `System.IFormattable` o `System.FormattableString` (que implementa `System.IFormattable`).

Cuando se aplica esta conversión, un valor de cadena no se compone de la cadena interpolada. En su lugar, se crea una instancia de `System.FormattableString`, como se describe en [cadenas interpoladas](expressions.md#interpolated-strings).

### <a name="implicit-nullable-conversions"></a>Conversiones implícitas que aceptan valores NULL

Las conversiones implícitas predefinidas que operan en tipos de valor que no aceptan valores null también se pueden usar con formas que aceptan valores NULL de esos tipos. Para cada una de las conversiones implícitas y numéricas predefinidas que convierten de un tipo de valor que no acepta valores NULL `S` a un `T`tipo de valor que no acepta valores NULL, existen las siguientes conversiones implícitas que aceptan valores NULL:

*  Conversión implícita de `S?` a `T?`.
*  Conversión implícita de `S` a `T?`.

La evaluación de una conversión implícita que acepta valores NULL en función de una conversión subyacente de `S` a `T` continúa de la siguiente manera:

*  Si la conversión que acepta valores NULL es de `S?` a `T?`:
    * Si el valor de origen es null (`HasValue` propiedad es false), el resultado es el valor null de tipo `T?`.
    * De lo contrario, la conversión se evalúa como un desencapsulado de `S?` a `S`, seguido de la conversión subyacente de `S` a `T`, seguido de un ajuste ([tipos que aceptan valores NULL](types.md#nullable-types)) de `T` a `T?`.

*  Si la conversión que acepta valores NULL es de `S` a `T?`, la conversión se evalúa como la conversión subyacente de `S` a `T` seguido de un ajuste de `T` a `T?`.

### <a name="null-literal-conversions"></a>Conversiones de literales null

Existe una conversión implícita del literal `null` a cualquier tipo que acepte valores NULL. Esta conversión genera el valor null ([tipos que aceptan valores NULL](types.md#nullable-types)) del tipo que acepta valores NULL dado.

### <a name="implicit-reference-conversions"></a>Conversiones de referencias implícitas

Las conversiones de referencia implícitas son:

*  Desde cualquier *reference_type* a `object` y `dynamic`.
*  Desde cualquier *class_type* `S` a cualquier `T`de *class_type* , proporcionado `S` se deriva de `T`.
*  Desde cualquier *class_type* `S` a cualquier `T`de *interface_type* , proporcionado `S` implementa `T`.
*  Desde cualquier *interface_type* `S` a cualquier `T`de *interface_type* , proporcionado `S` se deriva de `T`.
*  A partir de un `S` de *array_type* con un tipo de elemento `SE` a un `T` de *array_type* con un tipo de elemento `TE`, siempre que se cumplan todas las condiciones siguientes:
    * `S` y `T` solo difieren en el tipo de elemento. En otras palabras, `S` y `T` tienen el mismo número de dimensiones.
    * Tanto `SE` como `TE` son *reference_type*s.
    * Existe una conversión de referencia implícita de `SE` a `TE`.
*  Desde cualquier *array_type* a `System.Array` y las interfaces que implementa.
*  Desde un tipo de matriz unidimensional `S[]` a `System.Collections.Generic.IList<T>` y sus interfaces base, siempre que haya una conversión implícita de identidad o de referencia de `S` a `T`.
*  Desde cualquier *delegate_type* a `System.Delegate` y las interfaces que implementa.
*  Del literal null a cualquier *reference_type*.
*  Desde cualquier *reference_type* a un *reference_type* `T` si tiene una conversión implícita de identidad o de referencia a una *reference_type* `T0` y `T0` tiene una conversión de identidad en `T`.
*  Desde cualquier *reference_type* a un tipo de interfaz o delegado `T` si tiene una conversión implícita o de referencia a una interfaz o un tipo de delegado `T0` y `T0` es de varianza ([conversión de varianza](interfaces.md#variance-conversion)) para `T`.
*  Conversiones implícitas que implican parámetros de tipo que se sabe que son tipos de referencia. Vea [conversiones implícitas que implican parámetros de tipo](conversions.md#implicit-conversions-involving-type-parameters) para obtener más información sobre las conversiones implícitas que implican parámetros de tipo.

Las conversiones de referencia implícitas son las conversiones entre *reference_type*s que se pueden demostrar que siempre se realizan correctamente y, por tanto, no requieren comprobaciones en tiempo de ejecución.

Las conversiones de referencia, implícitas o explícitas, nunca cambian la identidad referencial del objeto que se va a convertir. En otras palabras, aunque una conversión de referencia puede cambiar el tipo de la referencia, nunca cambia el tipo o valor del objeto al que se hace referencia.

### <a name="boxing-conversions"></a>Conversiones Boxing

Una conversión boxing permite que un *value_type* se convierta implícitamente en un tipo de referencia. Existe una conversión boxing de cualquier *non_nullable_value_type* a `object` y `dynamic`, a `System.ValueType` y a cualquier *interface_type* implementado por el *non_nullable_value_type*. Además, un *enum_type* se puede convertir al tipo `System.Enum`.

Existe una conversión boxing de una *nullable_type* a un tipo de referencia, solo si existe una conversión boxing del *non_nullable_value_type* subyacente al tipo de referencia.

Un tipo de valor tiene una conversión boxing a un tipo de interfaz `I` si tiene una conversión boxing a un tipo de interfaz `I0` y `I0` tiene una conversión de identidad en `I`.

Un tipo de valor tiene una conversión boxing a un tipo de interfaz `I` si tiene una conversión boxing a un tipo de interfaz o delegado `I0` y `I0` es convertible por varianza ([conversión de varianza](interfaces.md#variance-conversion)) en `I`.

La conversión boxing de un valor de una *non_nullable_value_type* consiste en asignar una instancia de objeto y copiar el valor de *value_type* en esa instancia. Se puede aplicar la conversión boxing a un struct al `System.ValueType`de tipos, ya que es una clase base para todos los Structs ([herencia](structs.md#inheritance)).

La conversión boxing de un valor de una *nullable_type* procede de la siguiente manera:

*  Si el valor de origen es null (`HasValue` propiedad es false), el resultado es una referencia nula del tipo de destino.
*  De lo contrario, el resultado es una referencia a una `T` con conversión boxing generada al desempaquetar y aplicar la conversión boxing al valor de origen.

Las conversiones Boxing se describen con más detalle en [conversiones boxing](types.md#boxing-conversions).

### <a name="implicit-dynamic-conversions"></a>Conversiones dinámicas IMPLÍCITAS

Existe una conversión dinámica implícita de una expresión de tipo `dynamic` a cualquier tipo `T`. La conversión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)), lo que significa que se buscará una conversión implícita en tiempo de ejecución desde el tipo en tiempo de ejecución de la expresión que se va a `T`. Si no se encuentra ninguna conversión, se produce una excepción en tiempo de ejecución.

Tenga en cuenta que esta conversión implícita aparentemente infringe el Consejo en el principio de las [conversiones implícitas](conversions.md#implicit-conversions) que una conversión implícita nunca debe producir una excepción. Sin embargo, no es la propia conversión, sino la *búsqueda* de la conversión que produce la excepción. El riesgo de que se produzcan excepciones en tiempo de ejecución es inherente al uso del enlace dinámico. Si no se desea el enlace dinámico de la conversión, la expresión se puede convertir primero en `object`y, a continuación, en el tipo deseado.

En el ejemplo siguiente se muestran las conversiones dinámicas implícitas:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

Las asignaciones a `s2` y `i` emplean conversiones dinámicas IMPLÍCITAS, donde el enlace de las operaciones se suspende hasta el tiempo de ejecución. En tiempo de ejecución, se buscan conversiones implícitas desde el tipo en tiempo de ejecución de `d` -- `string` al tipo de destino. Se ha encontrado una conversión en `string` pero no `int`.

### <a name="implicit-constant-expression-conversions"></a>Conversiones implícitas de expresiones constantes

Una conversión de expresión constante implícita permite las siguientes conversiones:

*  Un *constant_expression* ([expresiones constantes](expressions.md#constant-expressions)) de tipo `int` se puede convertir al tipo `sbyte`, `byte`, `short`, `ushort`, `uint`o `ulong`, siempre que el valor de la *constant_expression* esté dentro del intervalo del tipo de destino.
*  Un *constant_expression* de tipo `long` se puede convertir al tipo `ulong`, siempre que el valor de la *constant_expression* no sea negativo.

### <a name="implicit-conversions-involving-type-parameters"></a>Conversiones implícitas que implican parámetros de tipo

Existen las siguientes conversiones implícitas para un parámetro de tipo determinado `T`:

*  Desde `T` a su clase base efectiva `C`, desde `T` a cualquier clase base de `C`y desde `T` a cualquier interfaz implementada por `C`. En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como conversión boxing. De lo contrario, la conversión se ejecuta como una conversión de referencia implícita o una conversión de identidad.
*  Desde `T` a un tipo de interfaz `I` en el conjunto de interfaz efectivo de `T`y de `T` a cualquier interfaz base de `I`. En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como conversión boxing. De lo contrario, la conversión se ejecuta como una conversión de referencia implícita o una conversión de identidad.
*  De `T` a un `U`de parámetro de tipo, proporcionado `T` depende de `U` ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)). En tiempo de ejecución, si `U` es un tipo de valor, `T` y `U` son necesariamente el mismo tipo y no se realiza ninguna conversión. De lo contrario, si `T` es un tipo de valor, la conversión se ejecuta como conversión boxing. De lo contrario, la conversión se ejecuta como una conversión de referencia implícita o una conversión de identidad.
*  Desde el literal null hasta `T`, se sabe que el `T` proporcionado es un tipo de referencia.
*  De `T` a un tipo de referencia `I` si tiene una conversión implícita a un tipo de referencia `S0` y `S0` tiene una conversión de identidad a `S`. En tiempo de ejecución, la conversión se ejecuta de la misma forma que la conversión en `S0`.
*  De `T` a un tipo de interfaz `I` si tiene una conversión implícita a una interfaz o un tipo de delegado `I0` y `I0` se pueden convertir en `I` ([conversión de varianza](interfaces.md#variance-conversion)). En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como conversión boxing. De lo contrario, la conversión se ejecuta como una conversión de referencia implícita o una conversión de identidad.

Si se sabe que `T` es un tipo de referencia ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), las conversiones anteriores se clasifican como conversiones de referencia IMPLÍCITAS ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions)). Si no se sabe que `T` es un tipo de referencia, las conversiones anteriores se clasifican como conversiones Boxing (conversiones[Boxing](conversions.md#boxing-conversions)).

### <a name="user-defined-implicit-conversions"></a>Conversiones implícitas definidas por el usuario

Una conversión implícita definida por el usuario se compone de una conversión implícita estándar opcional, seguida de una ejecución de un operador de conversión implícita definido por el usuario, seguida de otra conversión implícita estándar opcional. Las reglas exactas para evaluar las conversiones implícitas definidas por el usuario se describen en [procesamiento de conversiones implícitas definidas por el usuario](conversions.md#processing-of-user-defined-implicit-conversions).

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Conversiones de funciones anónimas y conversiones de grupos de métodos

Las funciones anónimas y los grupos de métodos no tienen tipos en y, pero se pueden convertir implícitamente en tipos de delegado o tipos de árbol de expresión. Las conversiones de función anónima se describen con más detalle en conversiones de [funciones anónimas](conversions.md#anonymous-function-conversions) y conversiones de grupos de métodos en [conversiones de grupos de métodos](conversions.md#method-group-conversions).

## <a name="explicit-conversions"></a>Conversiones explícitas

Las conversiones siguientes se clasifican como conversiones explícitas:

*  Todas las conversiones implícitas.
*  Conversiones numéricas explícitas.
*  Conversiones de enumeración explícitas.
*  Conversiones explícitas que aceptan valores NULL.
*  Conversiones de referencia explícitas.
*  Conversiones explícitas de la interfaz.
*  Conversiones unboxing.
*  Conversiones dinámicas explícitas
*  Conversiones explícitas definidas por el usuario.

Las conversiones explícitas pueden producirse en expresiones de conversión ([expresiones de conversión](expressions.md#cast-expressions)).

El conjunto de conversiones explícitas incluye todas las conversiones implícitas. Esto significa que se permiten expresiones de conversión redundantes.

Las conversiones explícitas que no son conversiones implícitas son conversiones que no se pueden demostrar que siempre se realizan correctamente, conversiones en las que se sabe que podrían perder información y conversiones entre dominios de tipos lo suficientemente diferentes como para méritos explícitos. notación.

### <a name="explicit-numeric-conversions"></a>Conversiones numéricas explícitas

Las conversiones numéricas explícitas son las conversiones de un *numeric_type* a otro *numeric_type* para el que no existe una conversión numérica implícita ([Conversiones numéricas implícitas](conversions.md#implicit-numeric-conversions)):

*  De `sbyte` a `byte`, `ushort`, `uint`, `ulong`o `char`.
*  De `byte` a `sbyte` y `char`.
*  De `short` a `sbyte`, `byte`, `ushort`, `uint`, `ulong`o `char`.
*  De `ushort` a `sbyte`, `byte`, `short`o `char`.
*  De `int` a `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`o `char`.
*  De `uint` a `sbyte`, `byte`, `short`, `ushort`, `int`o `char`.
*  De `long` a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`o `char`.
*  De `ulong` a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`o `char`.
*  De `char` a `sbyte`, `byte`o `short`.
*  De `float` a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`o `decimal`.
*  De `double` a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`o `decimal`.
*  De `decimal` a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`o `double`.

Dado que las conversiones explícitas incluyen todas las conversiones numéricas implícitas y explícitas, siempre es posible convertir de cualquier *numeric_type* a cualquier otra *numeric_type* mediante una expresión de conversión ([expresiones de conversión](expressions.md#cast-expressions)).

Las conversiones numéricas explícitas podrían perder información o provocar que se produzcan excepciones. Una conversión numérica explícita se procesa de la siguiente manera:

*  En el caso de una conversión de un tipo entero a otro tipo entero, el procesamiento depende del contexto de comprobación de desbordamiento ([los operadores comprobados y sin comprobar](expressions.md#the-checked-and-unchecked-operators)) en el que tiene lugar la conversión:
    * En un contexto de `checked`, la conversión se realiza correctamente si el valor del operando de origen está dentro del intervalo del tipo de destino, pero produce una `System.OverflowException` si el valor del operando de origen está fuera del intervalo del tipo de destino.
    * En un contexto de `unchecked`, la conversión siempre se realiza correctamente y continúa como se indica a continuación.
        * Si el tipo de origen es mayor que el tipo de destino, el valor de origen se trunca al descartar sus bits "extra" más significativos. El resultado se trata como un valor del tipo de destino.
        * Si el tipo de origen es menor que el tipo de destino, el valor de origen se amplía mediante signos o ceros para que tenga el mismo tamaño que el tipo de destino. La ampliación mediante signos se usa si el tipo de origen tiene signo; se emplea ampliación mediante ceros si el tipo de origen no tiene signo. El resultado se trata como un valor del tipo de destino.
        * Si el tipo de origen es del mismo tamaño que el tipo de destino, el valor de origen se trata como un valor del tipo de destino.
*  Para una conversión de `decimal` a un tipo entero, el valor de origen se redondea hacia cero al valor entero más cercano, y este valor entero se convierte en el resultado de la conversión. Si el valor entero resultante está fuera del intervalo del tipo de destino, se produce una `System.OverflowException`.
*  Para una conversión desde `float` o `double` a un tipo entero, el procesamiento depende del contexto de comprobación de desbordamiento ([los operadores comprobados y sin comprobar](expressions.md#the-checked-and-unchecked-operators)) en el que tiene lugar la conversión:
    * En un contexto de `checked`, la conversión se realiza de la siguiente manera:
        * Si el valor del operando es NaN o infinito, se produce una `System.OverflowException`.
        * De lo contrario, el operando de origen se redondea hacia cero al valor entero más cercano. Si este valor entero está dentro del intervalo del tipo de destino, este valor es el resultado de la conversión.
        * De lo contrario, se produce una excepción `System.OverflowException`.
    * En un contexto de `unchecked`, la conversión siempre se realiza correctamente y continúa como se indica a continuación.
        * Si el valor del operando es NaN o infinito, el resultado de la conversión es un valor no especificado del tipo de destino.
        * De lo contrario, el operando de origen se redondea hacia cero al valor entero más cercano. Si este valor entero está dentro del intervalo del tipo de destino, este valor es el resultado de la conversión.
        * De lo contrario, el resultado de la conversión es un valor no especificado del tipo de destino.
*  Para una conversión desde `double` a `float`, el valor de `double` se redondea al valor de `float` más próximo. Si el valor de `double` es demasiado pequeño para representarlo como un `float`, el resultado es cero positivo o cero negativo. Si el valor de `double` es demasiado grande para representarlo como un `float`, el resultado se convierte en infinito positivo o infinito negativo. Si el valor de `double` es NaN, el resultado es también NaN.
*  Para una conversión desde `float` o `double` a `decimal`, el valor de origen se convierte en `decimal` representación y se redondea al número más cercano después de la posición decimal 28 si es necesario ([el tipo decimal](types.md#the-decimal-type)). Si el valor de origen es demasiado pequeño para representarlo como `decimal`, el resultado es cero. Si el valor de origen es NaN, infinito o demasiado grande para representarse como `decimal`, se produce una `System.OverflowException`.
*  Para realizar una conversión de `decimal` a `float` o `double`, el valor `decimal` se redondea al valor `double` o `float` más próximo. Aunque esta conversión puede perder precisión, nunca provoca que se produzca una excepción.

### <a name="explicit-enumeration-conversions"></a>Conversiones explícitas de enumeración

Las conversiones de enumeración explícitas son:

*  Desde `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`o `decimal` a cualquier *enum_type*.
*  Desde cualquier *enum_type* a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`o `decimal`.
*  Desde cualquier *enum_type* a cualquier otro *enum_type*.

Una conversión de enumeración explícita entre dos tipos se procesa tratando cualquier *enum_type* participante como el tipo subyacente de ese *enum_type*y, a continuación, realizando una conversión numérica implícita o explícita entre los tipos resultantes. Por ejemplo, dado un *enum_type* `E` con y el tipo subyacente de `int`, una conversión de `E` a `byte` se procesa como una conversión numérica explícita ([Conversiones numéricas explícitas](conversions.md#explicit-numeric-conversions)) de `int` a `byte`, y una conversión de `byte` a `E` se procesa como una conversión numérica implícita ([Conversiones numéricas implícitas](conversions.md#implicit-numeric-conversions)) de `byte` a `int`.

### <a name="explicit-nullable-conversions"></a>Conversiones explícitas que aceptan valores NULL

Las ***conversiones explícitas que aceptan valores NULL*** permiten conversiones explícitas explícitas que operan en tipos de valor que no aceptan valores NULL para usarse también con formas que aceptan valores NULL de esos tipos. Para cada una de las conversiones explícitas predefinidas que convierten de un tipo de valor que no acepta valores NULL `S` a un tipo de valor que no acepta valores NULL `T` ([conversión de identidad](conversions.md#identity-conversion), [Conversiones numéricas implícitas](conversions.md#implicit-numeric-conversions), conversiones de [enumeración implícita](conversions.md#implicit-enumeration-conversions), [Conversiones numéricas explícitas](conversions.md#explicit-numeric-conversions)y [conversiones de enumeración explícita](conversions.md#explicit-enumeration-conversions)), existen las siguientes conversiones que aceptan valores NULL

*  Conversión explícita de `S?` en `T?`.
*  Conversión explícita de `S` en `T?`.
*  Conversión explícita de `S?` en `T`.

La evaluación de una conversión que acepta valores NULL en función de una conversión subyacente de `S` a `T` continúa de la siguiente manera:

*  Si la conversión que acepta valores NULL es de `S?` a `T?`:
    * Si el valor de origen es null (`HasValue` propiedad es false), el resultado es el valor null de tipo `T?`.
    * De lo contrario, la conversión se evalúa como un desencapsulado de `S?` a `S`, seguido de la conversión subyacente de `S` a `T`, seguido de un ajuste de `T` a `T?`.
*  Si la conversión que acepta valores NULL es de `S` a `T?`, la conversión se evalúa como la conversión subyacente de `S` a `T` seguido de un ajuste de `T` a `T?`.
*  Si la conversión que acepta valores NULL es de `S?` a `T`, la conversión se evalúa como un desencapsulado de `S?` a `S` seguido de la conversión subyacente de `S` a `T`.

Tenga en cuenta que un intento de desencapsular un valor que acepta valores null producirá una excepción si el valor es `null`.

### <a name="explicit-reference-conversions"></a>Conversiones de referencia explícitas

Las conversiones de referencia explícitas son:

*  Desde `object` y `dynamic` a cualquier otro *reference_type*.
*  Desde cualquier *class_type* `S` a cualquier `T`de *class_type* , proporcionado `S` es una clase base de `T`.
*  De cualquier *class_type* `S` a cualquier `T`de *interface_type* , se proporciona `S` no se sella y se proporciona `S` no implementa `T`.
*  Desde cualquier `S` de *interface_type* a cualquier `T`de *class_type* , se proporciona `T` que no está sellada ni se proporciona `T` implementa `S`.
*  Desde cualquier *interface_type* `S` a cualquier `T`de *interface_type* , proporcionado `S` no se deriva de `T`.
*  A partir de un `S` de *array_type* con un tipo de elemento `SE` a un `T` de *array_type* con un tipo de elemento `TE`, siempre que se cumplan todas las condiciones siguientes:
    * `S` y `T` solo difieren en el tipo de elemento. En otras palabras, `S` y `T` tienen el mismo número de dimensiones.
    * Tanto `SE` como `TE` son *reference_type*s.
    * Existe una conversión de referencia explícita de `SE` a `TE`.
*  Desde `System.Array` y las interfaces que implementa en cualquier *array_type*.
*  Desde un tipo de matriz unidimensional `S[]` a `System.Collections.Generic.IList<T>` y sus interfaces base, siempre que haya una conversión de referencia explícita de `S` a `T`.
*  Desde `System.Collections.Generic.IList<S>` y sus interfaces base a un tipo de matriz unidimensional `T[]`, siempre que haya una conversión explícita de identidad o de referencia de `S` a `T`.
*  Desde `System.Delegate` y las interfaces que implementa en cualquier *delegate_type*.
*  Desde un tipo de referencia a un tipo de referencia `T` si tiene una conversión de referencia explícita a un tipo de referencia `T0` y `T0` tiene un `T`de conversión de identidad.
*  Desde un tipo de referencia a un tipo de interfaz o delegado `T` si tiene una conversión de referencia explícita a una interfaz o un tipo de delegado `T0` y `T0` se puede convertir en `T` o `T` de varianza a `T0` ([conversión de varianza](interfaces.md#variance-conversion)).
*  De `D<S1...Sn>` a `D<T1...Tn>` donde `D<X1...Xn>` es un tipo de delegado genérico, `D<S1...Sn>` no es compatible con o es idéntico a `D<T1...Tn>`, y para cada parámetro de tipo `Xi` de `D` las siguientes:
    * Si `Xi` es invariable, `Si` es idéntica a `Ti`.
    * Si `Xi` es covariante, hay una conversión implícita o explícita o una conversión de referencia de `Si` a `Ti`.
    * Si `Xi` es contravariante, `Si` y `Ti` son idénticos o ambos tipos de referencia.
*  Conversiones explícitas que implican parámetros de tipo que se sabe que son tipos de referencia. Para obtener más información sobre las conversiones explícitas que implican parámetros de tipo, vea [conversiones explícitas que implican parámetros de tipo](conversions.md#explicit-conversions-involving-type-parameters).

Las conversiones de referencia explícitas son esas conversiones entre los tipos de referencia que requieren comprobaciones en tiempo de ejecución para asegurarse de que son correctas.

Para que una conversión de referencia explícita se realice correctamente en tiempo de ejecución, el valor del operando de origen debe ser `null`, o el tipo real del objeto al que hace referencia el operando de origen debe ser un tipo que se pueda convertir al tipo de destino mediante una conversión de referencia implícita ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions)) o una conversión boxing ([conversiones boxing](conversions.md#boxing-conversions)). Si se produce un error en una conversión de referencia explícita, se produce una `System.InvalidCastException`.

Las conversiones de referencia, implícitas o explícitas, nunca cambian la identidad referencial del objeto que se va a convertir. En otras palabras, aunque una conversión de referencia puede cambiar el tipo de la referencia, nunca cambia el tipo o valor del objeto al que se hace referencia.

### <a name="unboxing-conversions"></a>Conversiones unboxing

Una conversión unboxing permite que un tipo de referencia se convierta explícitamente en un *value_type*. Existe una conversión unboxing de los tipos `object`, `dynamic` y `System.ValueType` a cualquier *non_nullable_value_type*y de cualquier *interface_type* a cualquier *non_nullable_value_type* que implemente el *interface_type*. Además, se puede aplicar la conversión unboxing a `System.Enum` de tipos a cualquier *enum_type*.

Existe una conversión unboxing de un tipo de referencia a un *nullable_type* si existe una conversión unboxing del tipo de referencia al *non_nullable_value_type* subyacente de la *nullable_type*.

Un tipo de valor `S` tiene una conversión unboxing de un tipo de interfaz `I` si tiene una conversión unboxing de un tipo de interfaz `I0` y `I0` tiene una conversión de identidad en `I`.

Un tipo de valor `S` tiene una conversión unboxing de un tipo de interfaz `I` si tiene una conversión unboxing de una interfaz o de un tipo de delegado `I0` y `I0` es la varianza que se puede convertir en `I` o `I` es una varianza que se puede convertir en `I0` ([conversión de varianza](interfaces.md#variance-conversion)).

Una operación de conversión unboxing consiste en comprobar primero que la instancia de objeto es un valor de conversión boxing del *value_type*especificado y, a continuación, copiar el valor fuera de la instancia. Al aplicar la conversión unboxing a una referencia nula a un *nullable_type* , se genera el valor null de la *nullable_type*. Se puede aplicar la conversión unboxing a un struct del tipo `System.ValueType`, ya que es una clase base para todos los Structs ([herencia](structs.md#inheritance)).

Las conversiones unboxing se describen con más detalle en [conversiones unboxing](types.md#unboxing-conversions).

### <a name="explicit-dynamic-conversions"></a>Conversiones dinámicas explícitas

Existe una conversión dinámica explícita de una expresión de tipo `dynamic` a cualquier tipo `T`. La conversión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)), lo que significa que se buscará una conversión explícita en tiempo de ejecución desde el tipo en tiempo de ejecución de la expresión que se va a `T`. Si no se encuentra ninguna conversión, se produce una excepción en tiempo de ejecución.

Si no se desea el enlace dinámico de la conversión, la expresión se puede convertir primero en `object`y, a continuación, en el tipo deseado.

Supongamos que se define la clase siguiente:
```csharp
class C
{
    int i;

    public C(int i) { this.i = i; }

    public static explicit operator C(string s) 
    {
        return new C(int.Parse(s));
    }
}
```

En el ejemplo siguiente se muestran las conversiones dinámicas explícitas:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

La mejor conversión de `o` en `C` se encuentra en tiempo de compilación para que sea una conversión de referencia explícita. Esto produce un error en tiempo de ejecución, porque `"1"` no es en realidad una `C`. La conversión de `d` en `C` sin embargo, como una conversión dinámica explícita, se suspende en tiempo de ejecución, donde se encuentra una conversión definida por el usuario del tipo en tiempo de ejecución de `d` -- `string`--hasta `C` y se realiza correctamente.

### <a name="explicit-conversions-involving-type-parameters"></a>Conversiones explícitas que implican parámetros de tipo

Existen las siguientes conversiones explícitas para un parámetro de tipo determinado `T`:

*  Desde la clase base efectiva `C` de `T` a `T` y de cualquier clase base de `C` a `T`. En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como conversión unboxing. De lo contrario, la conversión se ejecuta como una conversión de referencia explícita o una conversión de identidad.
*  De cualquier tipo de interfaz que se va a `T`. En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como conversión unboxing. De lo contrario, la conversión se ejecuta como una conversión de referencia explícita o una conversión de identidad.
*  De `T` a cualquier *interface_type* `I` siempre y cuando no haya una conversión implícita de `T` a `I`. En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como una conversión boxing seguida de una conversión de referencia explícita. De lo contrario, la conversión se ejecuta como una conversión de referencia explícita o una conversión de identidad.
*  De un parámetro de tipo `U` a `T`, siempre y cuando `T` dependa de `U` ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)). En tiempo de ejecución, si `U` es un tipo de valor, `T` y `U` son necesariamente el mismo tipo y no se realiza ninguna conversión. De lo contrario, si `T` es un tipo de valor, la conversión se ejecuta como conversión unboxing. De lo contrario, la conversión se ejecuta como una conversión de referencia explícita o una conversión de identidad.

Si se sabe que `T` es un tipo de referencia, las conversiones anteriores se clasifican como conversiones de referencia explícitas ([conversiones de referencia explícita](conversions.md#explicit-reference-conversions)). Si no se sabe que `T` es un tipo de referencia, las conversiones anteriores se clasifican como conversiones unboxing (conversiones[unboxing](conversions.md#unboxing-conversions)).

Las reglas anteriores no permiten una conversión explícita directa de un parámetro de tipo sin restricciones a un tipo que no sea de interfaz, lo que podría ser sorprendente. La razón de esta regla es evitar la confusión y hacer que la semántica de dichas conversiones sea clara. Por ejemplo, consideremos la siguiente declaración:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Si se permitía la conversión directa explícita de `t` a `int`, es posible que se espere fácilmente que `X<int>.F(7)` devuelva `7L`. Sin embargo, no lo haría, porque las conversiones numéricas estándar solo se tienen en cuenta cuando se sabe que los tipos son numéricos en tiempo de enlace. Para que la semántica esté clara, en su lugar se debe escribir en el ejemplo anterior:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Este código se compilará ahora, pero la ejecución de `X<int>.F(7)` producirá una excepción en tiempo de ejecución, ya que no se puede convertir directamente un `int` con conversión boxing en un `long`.

### <a name="user-defined-explicit-conversions"></a>Conversiones explícitas definidas por el usuario

Una conversión explícita definida por el usuario se compone de una conversión explícita estándar opcional, seguida de una ejecución de un operador de conversión implícito o explícito definido por el usuario, seguida de otra conversión explícita estándar opcional. Las reglas exactas para evaluar las conversiones explícitas definidas por el usuario se describen en [procesamiento de conversiones explícitas definidas por el usuario](conversions.md#processing-of-user-defined-explicit-conversions).

## <a name="standard-conversions"></a>Conversiones estándar

Las conversiones estándar son las conversiones predefinidas que pueden producirse como parte de una conversión definida por el usuario.

### <a name="standard-implicit-conversions"></a>Conversiones implícitas estándar

Las siguientes conversiones implícitas se clasifican como conversiones implícitas estándar:

*  Conversiones de identidad ([conversión de identidad](conversions.md#identity-conversion))
*  Conversiones numéricas IMPLÍCITAS ([Conversiones numéricas implícitas](conversions.md#implicit-numeric-conversions))
*  Conversiones implícitas que aceptan valores NULL ([conversiones implícitas que aceptan valores NULL](conversions.md#implicit-nullable-conversions))
*  Conversiones de referencia IMPLÍCITAS ([conversiones de referencia implícitas](conversions.md#implicit-reference-conversions))
*  Conversiones Boxing ([conversiones boxing](conversions.md#boxing-conversions))
*  Conversiones implícitas de expresiones constantes ([conversiones dinámicas implícitas](conversions.md#implicit-dynamic-conversions))
*  Conversiones implícitas que implican parámetros de tipo ([conversiones implícitas que implican parámetros de tipo](conversions.md#implicit-conversions-involving-type-parameters))

Las conversiones implícitas estándar excluyen específicamente las conversiones implícitas definidas por el usuario.

### <a name="standard-explicit-conversions"></a>Conversiones explícitas estándar

Las conversiones explícitas estándar son todas las conversiones implícitas estándar más el subconjunto de las conversiones explícitas para las que existe una conversión implícita estándar opuesta. En otras palabras, si existe una conversión implícita estándar de un tipo `A` a un tipo `B`, existe una conversión explícita estándar del tipo `A` al tipo `B` y del tipo `B` al tipo `A`.

## <a name="user-defined-conversions"></a>Conversiones definidas por el usuario

C#permite aumentar las conversiones implícitas y explícitas predefinidas mediante ***conversiones definidas por el usuario***. Las conversiones definidas por el usuario se introducen mediante la declaración de operadores de conversión ([operadores de conversión](classes.md#conversion-operators)) en tipos de clase y estructura.

### <a name="permitted-user-defined-conversions"></a>Conversiones permitidas definidas por el usuario

C#permite declarar solo determinadas conversiones definidas por el usuario. En concreto, no es posible volver a definir una conversión implícita o explícita ya existente.

Para un tipo de origen determinado `S` y el tipo de destino `T`, si `S` o `T` son tipos que aceptan valores NULL, permita que `S0` y `T0` hagan referencia a sus tipos subyacentes; de lo contrario, `S0` y `T0` son iguales a `S` y `T` respectivamente. Una clase o struct puede declarar una conversión de un tipo de origen `S` a un tipo de destino `T` solo si se cumplen todas las condiciones siguientes:

*  `S0` y `T0` son tipos diferentes.
*  `S0` o `T0` es el tipo de clase o estructura en el que tiene lugar la declaración del operador.
*  Ni `S0` ni `T0` es una *interface_type*.
*  Sin incluir las conversiones definidas por el usuario, no existe una conversión de `S` a `T` o de `T` a `S`.

Las restricciones que se aplican a las conversiones definidas por el usuario se explican en los [operadores de conversión](classes.md#conversion-operators).

### <a name="lifted-conversion-operators"></a>Operadores de conversión de elevación

Dado un operador de conversión definido por el usuario que convierte de un tipo de valor que no acepta valores NULL `S` a un tipo de valor que no acepta valores NULL `T`, existe un ***operador de conversión de elevación*** que convierte `S?` en `T?`. Este operador de conversión de elevación realiza un desajuste de `S?` a `S` seguido de la conversión definida por el usuario de `S` a `T` seguido de un ajuste de `T` a `T?`, salvo que un valor null `S?` convierte directamente en una `T?`con valores NULL.

Un operador de conversión de elevación tiene la misma clasificación implícita o explícita que su operador de conversión definido por el usuario subyacente. El término "conversión definida por el usuario" se aplica al uso de operadores de conversión definidos por el usuario y de elevación.

### <a name="evaluation-of-user-defined-conversions"></a>Evaluación de conversiones definidas por el usuario

Una conversión definida por el usuario convierte un valor de su tipo, denominado ***tipo de origen***, a otro tipo, denominado ***tipo de destino***. La evaluación de una conversión definida por el usuario se centra en buscar el operador de conversión definido por el usuario ***más específico*** para los tipos de origen y de destino determinados. Esta determinación se divide en varios pasos:

*  Buscar el conjunto de clases y estructuras desde las que se considerarán los operadores de conversión definidos por el usuario. Este conjunto está formado por el tipo de origen y sus clases base, así como el tipo de destino y sus clases base (con suposiciones implícitas de que solo las clases y los Structs pueden declarar operadores definidos por el usuario, y que los tipos que no son de clase no tienen clases base). Para los fines de este paso, si el tipo de origen o de destino es un *nullable_type*, se utiliza en su lugar su tipo subyacente.
*  A partir de ese conjunto de tipos, determinar qué operadores de conversión definidos por el usuario y de elevación son aplicables. Para que un operador de conversión sea aplicable, debe ser posible realizar una conversión estándar ([conversiones estándar](conversions.md#standard-conversions)) del tipo de origen al tipo de operando del operador, y debe ser posible realizar una conversión estándar del tipo de resultado del operador al tipo de destino.
*  Del conjunto de operadores definidos por el usuario aplicables, que determinan qué operador es el más específico de forma inequívoca. En términos generales, el operador más específico es el operador cuyo tipo de operando es "más cercano" al tipo de origen y cuyo tipo de resultado es "más cercano" al tipo de destino. Los operadores de conversión definidos por el usuario son preferibles a los operadores de conversión de elevación. En las secciones siguientes se definen las reglas exactas para establecer el operador de conversión definido por el usuario más específico.

Una vez que se ha identificado un operador de conversión definido por el usuario más específico, la ejecución real de la conversión definida por el usuario implica hasta tres pasos:

*  En primer lugar, si es necesario, realizar una conversión estándar del tipo de origen al tipo de operando del operador de conversión definido por el usuario o de elevación.
*  A continuación, se invoca el operador de conversión definido por el usuario o de elevación para realizar la conversión.
*  Por último, si es necesario, realizar una conversión estándar del tipo de resultado del operador de conversión definido por el usuario o de elevación al tipo de destino.

La evaluación de una conversión definida por el usuario nunca implica más de un operador de conversión definido por el usuario o de elevación. En otras palabras, una conversión del tipo `S` al tipo `T` nunca ejecutará en primer lugar una conversión definida por el usuario de `S` a `X` y, a continuación, ejecutará una conversión definida por el usuario de `X` a `T`.

En las secciones siguientes se proporcionan definiciones exactas de la evaluación de las conversiones implícitas o explícitas definidas por el usuario. Las definiciones hacen uso de los siguientes términos:

*  Si existe una conversión implícita estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) de un tipo `A` a un tipo `B`, y si ninguna `A` ni `B` se *interface_type*s, se dice que `A` se ***incluye*** en `B`y `B` se dice que ***abarca*** `A`.
*  El ***tipo más abarcado*** en un conjunto de tipos es el que abarca todos los demás tipos del conjunto. Si no hay ningún tipo único que abarque todos los demás tipos, el conjunto no tiene más tipo de englobado. En términos más intuitivos, el tipo más amplio es el tipo "mayor" del conjunto (el tipo al que se puede convertir cada uno de los demás tipos de forma implícita).
*  El ***tipo más abarcado*** en un conjunto de tipos es el que se incluye en todos los demás tipos del conjunto. Si no hay ningún tipo único incluido en el resto de tipos, el conjunto no tiene el tipo más abarcado. En términos más intuitivos, el tipo más abarcado es el tipo "más pequeño" del conjunto (el tipo que se puede convertir implícitamente en cada uno de los demás tipos).

### <a name="processing-of-user-defined-implicit-conversions"></a>Procesamiento de conversiones implícitas definidas por el usuario

Una conversión implícita definida por el usuario del tipo `S` al tipo `T` se procesa de la siguiente manera:

*  Determine los tipos `S0` y `T0`. Si `S` o `T` son tipos que aceptan valores NULL, `S0` y `T0` son sus tipos subyacentes; de lo contrario, `S0` y `T0` son iguales a `S` y `T` respectivamente.
*  Busque el conjunto de tipos, `D`, desde el que se tendrán en cuenta los operadores de conversión definidos por el usuario. Este conjunto consta de `S0` (si `S0` es una clase o estructura), las clases base de `S0` (si `S0` es una clase) y `T0` (si `T0` es una clase o struct).
*  Busque el conjunto de operadores de conversión de elevación y definidos por el usuario aplicables `U`. Este conjunto consta de los operadores de conversión implícita definidos por el usuario y de elevación que se declaran en las clases o Structs de `D` que convierten de un tipo que abarca `S` a un tipo englobado por `T`. Si `U` está vacío, la conversión no está definida y se produce un error en tiempo de compilación.
*  Busque el tipo de origen más específico, `SX`, de los operadores en `U`:
    * Si alguno de los operadores de `U` convierte de `S`, se `S``SX`.
    * De lo contrario, `SX` es el tipo más abarcado en el conjunto combinado de tipos de origen de los operadores de `U`. Si no se encuentra exactamente un tipo más abarcado, la conversión es ambigua y se produce un error en tiempo de compilación.
*  Busque el tipo de destino más específico, `TX`, de los operadores en `U`:
    * Si alguno de los operadores de `U` convierte en `T`, se `T``TX`.
    * De lo contrario, `TX` es el tipo más abarcado en el conjunto combinado de tipos de destino de los operadores de `U`. Si no se encuentra exactamente un tipo más englobado, la conversión es ambigua y se produce un error en tiempo de compilación.
*  Busque el operador de conversión más específico:
    * Si `U` contiene exactamente un operador de conversión definido por el usuario que convierte de `SX` a `TX`, éste es el operador de conversión más específico.
    * De lo contrario, si `U` contiene exactamente un operador de conversión de elevación que convierte de `SX` a `TX`, éste es el operador de conversión más específico.
    * De lo contrario, la conversión es ambigua y se produce un error en tiempo de compilación.
*  Por último, aplique la conversión:
    * Si `S` no se `SX`, se realiza una conversión implícita estándar de `S` a `SX`.
    * Se invoca el operador de conversión más específico para convertir de `SX` a `TX`.
    * Si `TX` no se `T`, se realiza una conversión implícita estándar de `TX` a `T`.

### <a name="processing-of-user-defined-explicit-conversions"></a>Procesamiento de conversiones explícitas definidas por el usuario

Una conversión explícita definida por el usuario del tipo `S` al tipo `T` se procesa de la siguiente manera:

*  Determine los tipos `S0` y `T0`. Si `S` o `T` son tipos que aceptan valores NULL, `S0` y `T0` son sus tipos subyacentes; de lo contrario, `S0` y `T0` son iguales a `S` y `T` respectivamente.
*  Busque el conjunto de tipos, `D`, desde el que se tendrán en cuenta los operadores de conversión definidos por el usuario. Este conjunto consta de `S0` (si `S0` es una clase o estructura), las clases base de `S0` (si `S0` es una clase), `T0` (si `T0` es una clase o estructura) y las clases base de `T0` (si `T0` es una clase).
*  Busque el conjunto de operadores de conversión de elevación y definidos por el usuario aplicables `U`. Este conjunto consta de los operadores de conversión implícitos y de elevación definidos por el usuario declarados por las clases o Structs en `D` que se convierten de un tipo que abarca o engloba `S` a un tipo englobado o englobado por `T`. Si `U` está vacío, la conversión no está definida y se produce un error en tiempo de compilación.
*  Busque el tipo de origen más específico, `SX`, de los operadores en `U`:
    * Si alguno de los operadores de `U` convierte de `S`, se `S``SX`.
    * De lo contrario, si cualquiera de los operadores de `U` convierte de tipos que abarcan `S`, `SX` es el tipo más abarcado en el conjunto combinado de tipos de origen de esos operadores. Si no se puede encontrar ningún tipo más abarcado, la conversión es ambigua y se produce un error en tiempo de compilación.
    * De lo contrario, `SX` es el tipo más abarcado en el conjunto combinado de tipos de origen de los operadores de `U`. Si no se encuentra exactamente un tipo más englobado, la conversión es ambigua y se produce un error en tiempo de compilación.
*  Busque el tipo de destino más específico, `TX`, de los operadores en `U`:
    * Si alguno de los operadores de `U` convierte en `T`, se `T``TX`.
    * De lo contrario, si cualquiera de los operadores de `U` convierte en tipos que están englobados por `T`, `TX` es el tipo más abarcado en el conjunto combinado de tipos de destino de esos operadores. Si no se encuentra exactamente un tipo más englobado, la conversión es ambigua y se produce un error en tiempo de compilación.
    * De lo contrario, `TX` es el tipo más abarcado en el conjunto combinado de tipos de destino de los operadores de `U`. Si no se puede encontrar ningún tipo más abarcado, la conversión es ambigua y se produce un error en tiempo de compilación.
*  Busque el operador de conversión más específico:
    * Si `U` contiene exactamente un operador de conversión definido por el usuario que convierte de `SX` a `TX`, éste es el operador de conversión más específico.
    * De lo contrario, si `U` contiene exactamente un operador de conversión de elevación que convierte de `SX` a `TX`, éste es el operador de conversión más específico.
    * De lo contrario, la conversión es ambigua y se produce un error en tiempo de compilación.
*  Por último, aplique la conversión:
    * Si `S` no se `SX`, se realiza una conversión explícita estándar de `S` a `SX`.
    * Se invoca el operador de conversión definido por el usuario más específico para convertir de `SX` a `TX`.
    * Si `TX` no se `T`, se realiza una conversión explícita estándar de `TX` a `T`.

## <a name="anonymous-function-conversions"></a>Conversiones de funciones anónimas

Un *anonymous_method_expression* o *lambda_expression* se clasifica como una función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)). La expresión no tiene un tipo, pero se puede convertir de forma implícita a un tipo de delegado o un tipo de árbol de expresión compatible. En concreto, una función anónima `F` es compatible con un tipo de delegado `D` proporcionado:

*  Si `F` contiene un *anonymous_function_signature*, `D` y `F` tienen el mismo número de parámetros.
*  Si `F` no contiene un *anonymous_function_signature*, `D` puede tener cero o más parámetros de cualquier tipo, siempre que ningún parámetro de `D` tenga el modificador de parámetro `out`.
*  Si `F` tiene una lista de parámetros con tipo explícito, cada parámetro de `D` tiene el mismo tipo y modificadores que el parámetro correspondiente en `F`.
*  Si `F` tiene una lista de parámetros con tipo implícito, `D` no tiene ningún parámetro `ref` o `out`.
*  Si el cuerpo de `F` es una expresión y `D` tiene un tipo de valor devuelto `void` o `F` es Async y `D` tiene el tipo de valor devuelto `Task`, cuando cada parámetro de `F` recibe el tipo del parámetro correspondiente en `D`, el cuerpo de `F` es una expresión válida (las [expresiones](expressions.md)WRT) que se permitiría como *statement_expression* ([instrucciones de expresión](statements.md#expression-statements)).
*  Si el cuerpo de `F` es un bloque de instrucciones y `D` tiene un tipo de valor devuelto `void` o `F` es Async y `D` tiene el tipo de valor devuelto `Task`, cuando cada parámetro de `F` recibe el tipo del parámetro correspondiente en `D`, el cuerpo de `F` es un bloque de instrucciones válido (WRT [bloques](statements.md#blocks)) en el que ninguna instrucción `return` especifica una expresión.
*  Si el cuerpo de `F` es una expresión *y `F` no es Async y `D`* tiene un tipo de valor devuelto distinto de void `T`, *o* `F` es Async y `D` tiene un tipo de valor devuelto `Task<T>`, cuando cada parámetro de `F` recibe el tipo del parámetro correspondiente en `D`, el cuerpo de `F` es una expresión válida (WRT [expresiones](expressions.md)) que es implícitamente convertible a `T`.
*  Si el cuerpo de `F` es un bloque de instrucciones y *`F` no es asincrónico y `D`* tiene un tipo de valor devuelto distinto de void `T`, *o* `F` es Async y `D` tiene un tipo de valor devuelto `Task<T>`, cuando cada parámetro de `F` recibe el tipo del parámetro correspondiente en `D`, el cuerpo de `F` es un bloque de instrucciones válido (WRT [bloques](statements.md#blocks)) con un punto final no accesible en el que cada instrucción `return` especifica una expresión que se puede convertir implícitamente en `T`.

Por motivos de brevedad, en esta sección se usa la forma abreviada para los tipos de tarea `Task` y `Task<T>` ([funciones asincrónicas](classes.md#async-functions)).

Una expresión lambda `F` es compatible con un tipo de árbol de expresión `Expression<D>` si `F` es compatible con el tipo de delegado `D`. Tenga en cuenta que esto no se aplica a los métodos anónimos, solo a las expresiones lambda.

Ciertas expresiones lambda no se pueden convertir en tipos de árbol de expresión: aunque la conversión *existe*, se produce un error en tiempo de compilación. Este es el caso si la expresión lambda:

*  Tiene un cuerpo de *bloque*
*  Contiene operadores de asignación simples o compuestos
*  Contiene una expresión enlazada dinámicamente
*  Async

En los ejemplos siguientes se usa un tipo de delegado genérico `Func<A,R>` que representa una función que toma un argumento de tipo `A` y devuelve un valor de tipo `R`:
```csharp
delegate R Func<A,R>(A arg);
```

En las asignaciones
```csharp
Func<int,int> f1 = x => x + 1;                 // Ok

Func<int,double> f2 = x => x + 1;              // Ok

Func<double,int> f3 = x => x + 1;              // Error

Func<int, Task<int>> f4 = async x => x + 1;    // Ok
```
los tipos de valor devuelto y de parámetro de cada función anónima se determinan a partir del tipo de la variable a la que se asigna la función anónima.

La primera asignación convierte correctamente la función anónima en el tipo de delegado `Func<int,int>` porque, cuando `x` tiene el tipo `int`, `x+1` es una expresión válida que se pueda convertir implícitamente al tipo `int`.

Del mismo modo, la segunda asignación convierte correctamente la función anónima en el tipo de delegado `Func<int,double>` porque el resultado de `x+1` (de tipo `int`) es implícitamente convertible al tipo `double`.

Sin embargo, la tercera asignación es un error en tiempo de compilación porque, cuando `x` tiene el tipo `double`, el resultado de `x+1` (de tipo `double`) no se pueden convertir implícitamente al tipo `int`.

La cuarta asignación convierte correctamente la función asincrónica anónima en el tipo de delegado `Func<int, Task<int>>` porque el resultado de `x+1` (de tipo `int`) es implícitamente convertible al tipo de resultado `int` del tipo de tarea `Task<int>`.

Las funciones anónimas pueden influir en la resolución de sobrecarga y participar en la inferencia de tipos. Consulte [miembros de función](expressions.md#function-members) para obtener más detalles.

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Evaluación de conversiones de funciones anónimas a tipos de delegado

La conversión de una función anónima a un tipo de delegado genera una instancia de delegado que hace referencia a la función anónima y al conjunto (posiblemente vacío) de variables externas capturadas que están activas en el momento de la evaluación. Cuando se invoca el delegado, se ejecuta el cuerpo de la función anónima. El código del cuerpo se ejecuta utilizando el conjunto de variables externas capturadas al que hace referencia el delegado.

La lista de invocaciones de un delegado generado a partir de una función anónima contiene una sola entrada. No se especifican el objeto de destino exacto y el método de destino del delegado. En concreto, no se especifica si el objeto de destino del delegado es `null`, el valor `this` del miembro de función envolvente o algún otro objeto.

Se permiten las conversiones de funciones anónimas semánticamente idénticas con el mismo conjunto (posiblemente vacío) de instancias de variables externas capturadas en los mismos tipos de delegado (aunque no es necesario) para devolver la misma instancia de delegado. El término semánticamente idéntico se usa aquí para indicar que la ejecución de las funciones anónimas, en todos los casos, produce los mismos efectos dados los mismos argumentos. Esta regla permite optimizar el código como el siguiente.

```csharp
delegate double Function(double x);

class Test
{
    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void F(double[] a, double[] b) {
        a = Apply(a, (double x) => Math.Sin(x));
        b = Apply(b, (double y) => Math.Sin(y));
        ...
    }
}
```

Dado que los dos delegados de función anónimos tienen el mismo conjunto (vacío) de variables externas capturadas y como las funciones anónimas son idénticas semánticamente, el compilador puede hacer que los delegados hagan referencia al mismo método de destino. En realidad, el compilador puede devolver la misma instancia de delegado de ambas expresiones de función anónimas.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>Evaluación de conversiones de funciones anónimas a tipos de árbol de expresión

La conversión de una función anónima en un tipo de árbol de expresión genera un árbol de expresión ([tipos de árbol de expresión](types.md#expression-tree-types)). Más concretamente, la evaluación de la conversión de funciones anónimas conduce a la construcción de una estructura de objeto que representa la estructura de la propia función anónima. La estructura precisa del árbol de expresión, así como el proceso exacto para crearla, se definen en la implementación.

### <a name="implementation-example"></a>Ejemplo de implementación

En esta sección se describe una posible implementación de conversiones de funciones anónimas en C# términos de otras construcciones. La implementación que se describe aquí se basa en los mismos principios utilizados por C# el compilador de Microsoft, pero no es una implementación asignada, ni es lo único posible. Solo se mencionan brevemente las conversiones a los árboles de expresión, ya que su semántica exacta está fuera del ámbito de esta especificación.

En el resto de esta sección se proporcionan varios ejemplos de código que contiene funciones anónimas con diferentes características. En cada ejemplo, se proporciona una traducción correspondiente al código que usa C# solo otras construcciones. En los ejemplos, se supone que el identificador `D` representa el siguiente tipo de delegado:
```csharp
public delegate void D();
```

La forma más sencilla de una función anónima es aquella que no captura variables externas:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

Esto se puede traducir a una creación de instancias de delegado que hace referencia a un método estático generado por el compilador en el que se coloca el código de la función anónima:
```csharp
class Test
{
    static void F() {
        D d = new D(__Method1);
    }

    static void __Method1() {
        Console.WriteLine("test");
    }
}
```

En el ejemplo siguiente, la función anónima hace referencia a los miembros de instancia de `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

Esto puede traducirse a un método de instancia generado por el compilador que contenga el código de la función anónima:
```csharp
class Test
{
    int x;

    void F() {
        D d = new D(__Method1);
    }

    void __Method1() {
        Console.WriteLine(x);
    }
}
```

En este ejemplo, la función anónima captura una variable local:
```csharp
class Test
{
    void F() {
        int y = 123;
        D d = () => { Console.WriteLine(y); };
    }
}
```

La duración de la variable local debe extenderse ahora a al menos la duración del delegado de función anónima. Esto puede lograrse mediante la "activación" de la variable local en un campo de una clase generada por el compilador. La creación de instancias de la variable local (creación[de instancias de variables locales](expressions.md#instantiation-of-local-variables)) corresponde entonces a la creación de una instancia de la clase generada por el compilador y el acceso a la variable local corresponde al acceso a un campo en la instancia de la clase generada por el compilador. Además, la función anónima se convierte en un método de instancia de la clase generada por el compilador:
```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.y = 123;
        D d = new D(__locals1.__Method1);
    }

    class __Locals1
    {
        public int y;

        public void __Method1() {
            Console.WriteLine(y);
        }
    }
}
```

Por último, la función anónima siguiente captura `this` así como dos variables locales con distintas duraciones:
```csharp
class Test
{
    int x;

    void F() {
        int y = 123;
        for (int i = 0; i < 10; i++) {
            int z = i * 2;
            D d = () => { Console.WriteLine(x + y + z); };
        }
    }
}
```

En este caso, se crea una clase generada por el compilador para cada bloque de instrucciones en el que se capturan las variables locales de forma que las variables locales de los diferentes bloques puedan tener duraciones independientes. Una instancia de `__Locals2`, la clase generada por el compilador para el bloque de instrucciones interno, contiene la variable local `z` y un campo que hace referencia a una instancia de `__Locals1`.  Una instancia de `__Locals1`, la clase generada por el compilador para el bloque de instrucciones exterior, contiene la variable local `y` y un campo que hace referencia a `this` del miembro de función envolvente. Con estas estructuras de datos es posible llegar a todas las variables externas capturadas a través de una instancia de `__Local2`, y el código de la función anónima se puede implementar como un método de instancia de esa clase.

```csharp
class Test
{
    void F() {
        __Locals1 __locals1 = new __Locals1();
        __locals1.__this = this;
        __locals1.y = 123;
        for (int i = 0; i < 10; i++) {
            __Locals2 __locals2 = new __Locals2();
            __locals2.__locals1 = __locals1;
            __locals2.z = i * 2;
            D d = new D(__locals2.__Method1);
        }
    }

    class __Locals1
    {
        public Test __this;
        public int y;
    }

    class __Locals2
    {
        public __Locals1 __locals1;
        public int z;

        public void __Method1() {
            Console.WriteLine(__locals1.__this.x + __locals1.y + z);
        }
    }
}
```

También se puede usar la misma técnica que se aplica aquí para capturar variables locales al convertir funciones anónimas en árboles de expresión: las referencias a los objetos generados por el compilador se pueden almacenar en el árbol de expresión y el acceso a las variables locales puede ser se representa como accesos de campo en estos objetos. La ventaja de este enfoque es que permite compartir las variables locales "levantadas" entre los delegados y los árboles de expresión.

## <a name="method-group-conversions"></a>Conversiones de grupos de métodos

Existe una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) de un grupo de métodos ([clasificaciones de expresión](expressions.md#expression-classifications)) a un tipo de delegado compatible. Dado un tipo de delegado `D` y una expresión `E` que se clasifica como un grupo de métodos, existe una conversión implícita de `E` a `D` si `E` contiene al menos un método que es aplicable en su forma normal ([miembro de función aplicable](expressions.md#applicable-function-member)) a una lista de argumentos construida mediante el uso de los tipos de parámetros y modificadores de `D`, como se describe en el siguiente.

La aplicación en tiempo de compilación de una conversión de un grupo de métodos `E` a un tipo de delegado `D` se describe en lo siguiente. Tenga en cuenta que la existencia de una conversión implícita de `E` a `D` no garantiza que la aplicación en tiempo de compilación de la conversión se realice correctamente sin errores.

*  Se selecciona un único método `M` que corresponde a una invocación de método ([invocaciones de método](expressions.md#method-invocations)) del formulario `E(A)`, con las siguientes modificaciones:
    * La lista de argumentos `A` es una lista de expresiones, cada una clasificada como una variable y con el tipo y el modificador (`ref` o `out`) del parámetro correspondiente en la *formal_parameter_list* de `D`.
    * Los métodos candidatos considerados solo son aquellos que se aplican en su forma normal ([miembro de función aplicable](expressions.md#applicable-function-member)), no los que solo se aplican en su forma expandida.
*  Si el algoritmo de las [invocaciones de método](expressions.md#method-invocations) produce un error, se produce un error en tiempo de compilación. De lo contrario, el algoritmo genera un único método mejor `M` tener el mismo número de parámetros que `D` y se considera que la conversión existe.
*  El método seleccionado `M` debe ser compatible con el tipo[de delegado `D`](delegates.md#delegate-compatibility), o de lo contrario, se producirá un error en tiempo de compilación.
*  Si el método seleccionado `M` es un método de instancia, la expresión de instancia asociada a `E` determina el objeto de destino del delegado.
*  Si el método seleccionado M es un método de extensión que se indica mediante un acceso a miembro en una expresión de instancia, esa expresión de instancia determina el objeto de destino del delegado.
*  El resultado de la conversión es un valor de tipo `D`, es decir, un delegado recién creado que hace referencia al método seleccionado y al objeto de destino.
*  Tenga en cuenta que este proceso puede dar lugar a la creación de un delegado a un método de extensión, si el algoritmo de las [invocaciones de método](expressions.md#method-invocations) no encuentra un método de instancia pero realiza correctamente el procesamiento de la invocación de `E(A)` como una invocación de método de extensión ([invocaciones de método de extensión](expressions.md#extension-method-invocations)). Por lo tanto, un delegado creado captura el método de extensión, así como su primer argumento.

En el ejemplo siguiente se muestran las conversiones de grupos de métodos:
```csharp
delegate string D1(object o);

delegate object D2(string s);

delegate object D3();

delegate string D4(object o, params object[] a);

delegate string D5(int i);

class Test
{
    static string F(object o) {...}

    static void G() {
        D1 d1 = F;            // Ok
        D2 d2 = F;            // Ok
        D3 d3 = F;            // Error -- not applicable
        D4 d4 = F;            // Error -- not applicable in normal form
        D5 d5 = F;            // Error -- applicable but not compatible

    }
}
```

La asignación a `d1` convierte implícitamente el grupo de métodos `F` en un valor de tipo `D1`.

La asignación a `d2` muestra cómo se puede crear un delegado para un método que tiene tipos de parámetro menos derivados (contravariante) y un tipo de valor devuelto más derivado (covariante).

La asignación a `d3` muestra el modo en que no existe ninguna conversión si el método no es aplicable.

La asignación a `d4` muestra cómo el método debe ser aplicable en su forma normal.

La asignación a `d5` muestra cómo se permite que los tipos de parámetro y de valor devuelto del delegado y el método difieran solo para los tipos de referencia.

Como con todas las demás conversiones implícitas y explícitas, el operador de conversión se puede usar para realizar explícitamente una conversión de grupo de métodos. Por lo tanto, el ejemplo
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
en su lugar, se puede escribir
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Los grupos de métodos pueden influir en la resolución de sobrecarga y participar en la inferencia de tipos. Consulte [miembros de función](expressions.md#function-members) para obtener más detalles.

La evaluación en tiempo de ejecución de una conversión de grupo de métodos continúa como sigue:

*  Si el método seleccionado en tiempo de compilación es un método de instancia, o es un método de extensión al que se tiene acceso como un método de instancia, el objeto de destino del delegado se determina a partir de la expresión de instancia asociada a `E`:
    * Se evalúa la expresión de instancia. Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.
    * Si la expresión de instancia es de un *reference_type*, el valor calculado por la expresión de instancia se convierte en el objeto de destino. Si el método seleccionado es un método de instancia y el objeto de destino es `null`, se produce una `System.NullReferenceException` y no se ejecuta ningún otro paso.
    * Si la expresión de instancia es de un *value_type*, se realiza una operación de conversión boxing ([conversiones boxing](types.md#boxing-conversions)) para convertir el valor en un objeto y este objeto se convierte en el objeto de destino.
*  De lo contrario, el método seleccionado forma parte de una llamada al método estático y el objeto de destino del delegado es `null`.
*  Se asigna una nueva instancia del tipo de delegado `D`. Si no hay suficiente memoria disponible para asignar la nueva instancia, se produce una `System.OutOfMemoryException` y no se ejecuta ningún otro paso.
*  La nueva instancia de delegado se inicializa con una referencia al método que se determinó en tiempo de compilación y una referencia al objeto de destino calculado anteriormente.
