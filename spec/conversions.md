# <a name="conversions"></a>Conversiones

Un ***conversión*** permite que una expresión se traten como de un tipo determinado. Una conversión puede producir una expresión de un tipo determinado se traten como si tuviera un tipo diferente, o puede hacer que una expresión sin un tipo que se va a obtener un tipo. Las conversiones pueden ser ***implícita*** o ***explícita***, y esto determina si se requiere una conversión explícita. Por ejemplo, la conversión del tipo `int` escriba `long` es implícito, es así expresiones de tipo `int` implícitamente se pueden tratar como tipo `long`. La conversión opuesta, del tipo `long` escriba `int`, es explícita, por lo que se requiere una conversión explícita.

```csharp
int a = 123;
long b = a;         // implicit conversion from int to long
int c = (int) b;    // explicit conversion from long to int
```

Algunas conversiones se definen mediante el lenguaje. Los programas también pueden definir sus propias conversiones ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)).

## <a name="implicit-conversions"></a>Conversiones implícitas

Las conversiones siguientes se clasifican como conversiones implícitas:

*  Conversiones de identidad
*  Conversiones numéricas implícitas
*  Conversiones implícitas de enumeración.
*  Conversiones implícitas que acepta valores null
*  Conversiones de literal null
*  Conversiones implícitas de referencia
*  Conversiones boxing
*  Conversiones implícitas de dinámicas
*  Conversiones implícitas de expresión constante
*  Conversiones implícitas definido por el usuario
*  Conversiones de función anónima
*  Conversiones de métodos de grupo

Conversiones implícitas pueden ocurrir en una variedad de situaciones, incluidas las llamadas a funciones miembro ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), las expresiones de conversión ([expresiones de conversión](expressions.md#cast-expressions)), y las asignaciones ([operadores de asignación](expressions.md#assignment-operators)).

Las conversiones implícitas predefinidas siempre se ejecuta correctamente y nunca causan que se produzcan excepciones. Las conversiones implícitas diseñadas correctamente definido por el usuario deben presentar estas características también.

Para los fines de conversión, los tipos de `object` y `dynamic` se consideran equivalentes.

Sin embargo, las conversiones dinámicas ([conversiones implícitas de dinámicas](conversions.md#implicit-dynamic-conversions) y [las conversiones explícitas de dinámicas](conversions.md#explicit-dynamic-conversions)) solo se aplican a expresiones de tipo `dynamic` ([el tipo dinámico](types.md#the-dynamic-type)).

### <a name="identity-conversion"></a>Conversión de identidad

Convierte una conversión de identidad de cualquier tipo en el mismo tipo. Esta conversión existe de forma que se puede decir una entidad que ya tiene un tipo necesario se pueda convertir a ese tipo.

*  Porque el objeto y dinámicos se consideran equivalentes hay una conversión de identidad entre `object` y `dynamic`y entre tipos construidos que son iguales cuando se reemplaza todas las apariciones de `dynamic` con `object`.

### <a name="implicit-numeric-conversions"></a>Conversiones numéricas implícitas

Las conversiones numéricas implícitas son:

*  Desde `sbyte` a `short`, `int`, `long`, `float`, `double`, o `decimal`.
*  Desde `byte` a `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, o `decimal`.
*  Desde `short` a `int`, `long`, `float`, `double`, o `decimal`.
*  Desde `ushort` a `int`, `uint`, `long`, `ulong`, `float`, `double`, o `decimal`.
*  Desde `int` a `long`, `float`, `double`, o `decimal`.
*  Desde `uint` a `long`, `ulong`, `float`, `double`, o `decimal`.
*  Desde `long` a `float`, `double`, o `decimal`.
*  Desde `ulong` a `float`, `double`, o `decimal`.
*  Desde `char` a `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, o `decimal`.
*  Desde `float` a `double`.

Conversiones de `int`, `uint`, `long`, o `ulong` a `float` y desde `long` o `ulong` a `double` puede provocar una pérdida de precisión, pero no una pérdida de magnitud. Las conversiones numéricas implícitas nunca pierden información.

No hay ninguna conversión implícita para el `char` escriba, por lo que los valores de los otros tipos enteros no se convierten automáticamente en el `char` tipo.

### <a name="implicit-enumeration-conversions"></a>Conversiones implícitas de enumeración

Permite una conversión implícita de enumeración el *decimal_integer_literal* `0` convertirse a cualquier *enum_type* y a cualquier *nullable_type* cuyo el tipo subyacente es un *enum_type*. En este caso, la conversión se evalúa mediante la conversión subyacente *enum_type* y ajuste el resultado ([tipos que aceptan valores NULL](types.md#nullable-types)).

### <a name="implicit-interpolated-string-conversions"></a>Conversiones implícitas de cadena interpolada

Implicit interpoladas cadena conversión permite un *interpolated_string_expression* ([cadenas interpoladas](expressions.md#interpolated-strings)) va a convertir en `System.IFormattable` o `System.FormattableString` (que implementa `System.IFormattable`).

Cuando esta conversión se aplica un valor de cadena no está compuesto por la cadena interpolada. En su lugar una instancia de `System.FormattableString` está creado, tal como se describe en [cadenas interpoladas](expressions.md#interpolated-strings).

### <a name="implicit-nullable-conversions"></a>Conversiones implícitas que acepta valores null

Las conversiones implícitas predefinidas que funcionan en tipos de valor distinto de NULL también pueden utilizarse con los formularios de esos tipos que aceptan valores NULL. Para cada identidad implícita predefinida y las conversiones numéricas que conversión de un tipo de valor distinto de NULL `S` a un tipo de valor distinto de NULL `T`, existen las siguientes conversiones implícitas que acepta valores NULL:

*  Una conversión implícita de `S?` a `T?`.
*  Una conversión implícita de `S` a `T?`.

Evaluación de una conversión implícita que acepta valores NULL en función de una conversión subyacente de `S` a `T` continúa como sigue:

*  Si la conversión que acepta valores NULL es desde `S?` a `T?`:
    * Si el valor de origen es null (`HasValue` propiedad es false), el resultado es el valor null del tipo `T?`.
    * En caso contrario, la conversión se evalúa como un desajuste de `S?` a `S`, seguido de la conversión subyacente de `S` a `T`, seguido de un ajuste ([tipos que aceptan valores NULL](types.md#nullable-types)) desde `T` a `T?`.

*  Si la conversión que acepta valores NULL es desde `S` a `T?`, la conversión se evalúa como la conversión subyacente de `S` a `T` seguida de un ajuste de `T` a `T?`.

### <a name="null-literal-conversions"></a>Conversiones de literal null

Existe una conversión implícita desde el `null` literal a cualquier tipo que acepta valores NULL. Esta conversión genera el valor null ([tipos que aceptan valores NULL](types.md#nullable-types)) del tipo que acepta valores NULL especificado.

### <a name="implicit-reference-conversions"></a>Conversiones implícitas de referencia

Las conversiones implícitas de referencia son:

*  Desde cualquier *reference_type* a `object` y `dynamic`.
*  Desde cualquier *class_type* `S` a cualquier *class_type* `T`, que proporciona `S` se deriva de `T`.
*  Desde cualquier *class_type* `S` a cualquier *interface_type* `T`, que proporciona `S` implementa `T`.
*  Desde cualquier *interface_type* `S` a cualquier *interface_type* `T`, que proporciona `S` se deriva de `T`.
*  Desde un *array_type* `S` con un tipo de elemento `SE` a un *array_type* `T` con un tipo de elemento `TE`, siempre que todos los elementos siguientes son verdaderas:
    * `S` y `T` difieren solo en el tipo de elemento. En otras palabras, `S` y `T` tienen el mismo número de dimensiones.
    * Ambos `SE` y `TE` son *reference_type*s.
    * Existe una conversión implícita de referencia de `SE` a `TE`.
*  Desde cualquier *array_type* a `System.Array` y las interfaces que implementa.
*  Desde un tipo de matriz unidimensional `S[]` a `System.Collections.Generic.IList<T>` y sus interfaces base, proporcionadas que hay una conversión implícita de referencia o identidad de `S` a `T`.
*  Desde cualquier *delegate_type* a `System.Delegate` y las interfaces que implementa.
*  Desde el literal null para alguna *reference_type*.
*  Desde cualquier *reference_type* a un *reference_type* `T` si tiene una conversión implícita de identidad o una referencia a un *reference_type* `T0` y `T0` tiene una conversión de identidad a `T`.
*  Desde cualquier *reference_type* a un tipo de interfaz o delegado `T` si tiene una conversión implícita de identidad o una referencia a un tipo de interfaz o delegado `T0` y `T0` es convertible de varianza ([ Conversión de varianza](interfaces.md#variance-conversion)) a `T`.
*  Conversiones implícitas que implica parámetros de tipo que se sabe que son tipos de referencia. Consulte [conversiones implícitas que implica parámetros de tipo](conversions.md#implicit-conversions-involving-type-parameters) para obtener más detalles sobre las conversiones implícitas que implica parámetros de tipo.

Las conversiones implícitas de referencia son aquellas conversiones entre *reference_type*s que se pueda demostrar que siempre se realizan correctamente y, por lo tanto, no requieren comprobaciones en tiempo de ejecución.

Las conversiones de referencia, implícitas o explícitas, nunca cambian la identidad referencial del objeto que se va a convertir. En otras palabras, mientras que una conversión de referencia puede cambiar el tipo de la referencia, nunca cambia el tipo o valor del objeto que hace referencia.

### <a name="boxing-conversions"></a>Conversiones boxing

Una conversión boxing permite un *value_type* convertir implícitamente a un tipo de referencia. Existe una conversión boxing de cualquier *non_nullable_value_type* a `object` y `dynamic`a `System.ValueType` y a cualquier *interface_type* implementado por el *non_ nullable_value_type*. Además un *enum_type* puede convertirse al tipo `System.Enum`.

Existe una conversión boxing de un *nullable_type* a un tipo de referencia, si y solo si una conversión boxing existe subyacente *non_nullable_value_type* al tipo de referencia.

Un tipo de valor tiene una conversión boxing a un tipo de interfaz `I` si tiene una conversión boxing a un tipo de interfaz `I0` y `I0` tiene una conversión de identidad a `I`.

Un tipo de valor tiene una conversión boxing a un tipo de interfaz `I` si tiene una conversión boxing a un tipo de interfaz o delegado `I0` y `I0` es convertible de varianza ([conversión de varianza](interfaces.md#variance-conversion)) a `I`.

Conversión boxing de un valor de un *non_nullable_value_type* consiste en asignar una instancia del objeto y copiar el *value_type* valor en esa instancia. Un struct puede realizar la conversión boxing al tipo `System.ValueType`, ya que es una clase base para todas las estructuras ([herencia](structs.md#inheritance)).

Conversión boxing de un valor de un *nullable_type* continúa como sigue:

*  Si el valor de origen es null (`HasValue` propiedad es false), el resultado es una referencia nula del tipo de destino.
*  En caso contrario, el resultado es una referencia a una conversión boxing `T` producidos por boxing el valor de origen y la acción de desencapsular.

Se describe más detalladamente en las conversiones boxing [conversiones Boxing](types.md#boxing-conversions).

### <a name="implicit-dynamic-conversions"></a>Conversiones implícitas de dinámicas

Existe una conversión implícita de dinámica de una expresión de tipo `dynamic` a cualquier tipo `T`. La conversión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)), lo que significa que se buscará una conversión implícita en tiempo de ejecución desde el tipo de tiempo de ejecución de la expresión `T`. Si no se encuentra ninguna conversión, se produce una excepción en tiempo de ejecución.

Tenga en cuenta que esta conversión implícita aparentemente infringe el Consejo al principio de [conversiones implícitas](conversions.md#implicit-conversions) que una conversión implícita nunca debe producir una excepción. Sin embargo no es la conversión, pero la *buscar* de la conversión que produce la excepción. El riesgo de las excepciones de tiempo de ejecución es inherente en el uso de enlace dinámico. Si el enlace dinámico de la conversión no es el deseado, la expresión se puede convertir en primer lugar a `object`y, a continuación, al tipo deseado.

El ejemplo siguiente muestra las conversiones implícitas de dinámicas:

```csharp
object o  = "object"
dynamic d = "dynamic";

string s1 = o; // Fails at compile-time -- no conversion exists
string s2 = d; // Compiles and succeeds at run-time
int i     = d; // Compiles but fails at run-time -- no conversion exists
```

Las asignaciones a `s2` y `i` ambos emplean las conversiones implícitas dinámicas, donde se suspende el enlace de las operaciones hasta el tiempo de ejecución. En tiempo de ejecución, se buscan las conversiones implícitas del tipo de tiempo de ejecución de `d`  --  `string` --al tipo de destino. Se encuentra una conversión a `string` pero no a `int`.

### <a name="implicit-constant-expression-conversions"></a>Conversiones implícitas de expresión constante

Una conversión implícita de expresión constante permite las siguientes conversiones:

*  Un *constant_expression* ([expresiones constantes](expressions.md#constant-expressions)) de tipo `int` puede convertirse al tipo `sbyte`, `byte`, `short`, `ushort`, `uint`, o `ulong`, siempre que el valor de la *constant_expression* está dentro del intervalo del tipo de destino.
*  Un *constant_expression* typu `long` puede convertirse al tipo `ulong`, siempre que el valor de la *constant_expression* no es negativo.

### <a name="implicit-conversions-involving-type-parameters"></a>Conversiones implícitas que implica parámetros de tipo

Existen las siguientes conversiones implícitas para un parámetro de tipo dado `T`:

*  Desde `T` a su clase base eficaz `C`, desde `T` a cualquier clase base de `C`y desde `T` a cualquier interfaz implementada por `C`. En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como una conversión boxing. En caso contrario, la conversión se ejecuta como una conversión implícita de referencia o una conversión de identidad.
*  Desde `T` a un tipo de interfaz `I` en `T`del conjunto de interfaces efectivas y desde `T` a cualquier interfaz base de `I`. En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como una conversión boxing. En caso contrario, la conversión se ejecuta como una conversión implícita de referencia o una conversión de identidad.
*  Desde `T` a un parámetro de tipo `U`, que proporciona `T` depende `U` ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)). En tiempo de ejecución, si `U` es un tipo de valor, a continuación, `T` y `U` son necesariamente del mismo tipo y se realiza ninguna conversión. De lo contrario, si `T` es un tipo de valor, la conversión se ejecuta como una conversión boxing. En caso contrario, la conversión se ejecuta como una conversión implícita de referencia o una conversión de identidad.
*  Desde el literal null a `T`, que proporciona `T` se sabe que es un tipo de referencia.
*  Desde `T` a un tipo de referencia `I` si tiene una conversión implícita a un tipo de referencia `S0` y `S0` tiene una conversión de identidad a `S`. En tiempo de ejecución, la conversión se ejecuta la misma manera que la conversión a `S0`.
*  Desde `T` a un tipo de interfaz `I` si tiene una conversión implícita a un tipo de interfaz o delegado `I0` y `I0` es convertible de varianza a `I` ([conversión de varianza](interfaces.md#variance-conversion) ). En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como una conversión boxing. En caso contrario, la conversión se ejecuta como una conversión implícita de referencia o una conversión de identidad.

Si `T` se sabe que es un tipo de referencia ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), las conversiones anteriores se clasifican como conversiones implícitas de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)). Si `T` no es sabe que un tipo de referencia, las conversiones anteriores se clasifican como conversiones boxing ([conversiones Boxing](conversions.md#boxing-conversions)).

### <a name="user-defined-implicit-conversions"></a>Conversiones implícitas definido por el usuario

Una conversión implícita definido por el usuario consta de una conversión implícita opcional estándar, seguida por la ejecución de un operador de conversión implícito definido por el usuario, seguido de otra conversión implícita opcional estándar. Las reglas exactas para evaluar las conversiones implícitas definido por el usuario se describen en [de procesamiento de las conversiones implícitas definido por el usuario](conversions.md#processing-of-user-defined-implicit-conversions).

### <a name="anonymous-function-conversions-and-method-group-conversions"></a>Conversiones de función anónima y conversiones de métodos de grupo

Funciones anónimas y grupos de métodos no tienen tipos de por sí mismos, pero se pueden convertir implícitamente para delegar tipos o tipos de árbol de expresión. Conversiones de función anónima se describen con más detalle en [conversiones de función anónima](conversions.md#anonymous-function-conversions) y conversiones de grupo de métodos en [conversiones de métodos de grupo](conversions.md#method-group-conversions).

## <a name="explicit-conversions"></a>Conversiones explícitas

Las conversiones siguientes se clasifican como las conversiones explícitas:

*  Todas las conversiones implícitas.
*  Conversiones numéricas explícitas.
*  Conversiones explícitas de enumeración.
*  Conversiones explícitas que acepta valores NULL.
*  Conversiones explícitas de referencia.
*  Conversiones de interfaz explícita.
*  Conversiones unboxing.
*  Conversiones explícitas de dinámicas
*  Conversiones explícitas definidas por el usuario.

Las conversiones explícitas pueden producirse en expresiones de conversión ([expresiones de conversión](expressions.md#cast-expressions)).

El conjunto de conversiones explícitas incluye todas las conversiones implícitas. Esto significa que se permiten expresiones de conversión redundantes.

Las conversiones explícitas que no son las conversiones implícitas son las conversiones que no se puede demostrar que siempre se realizan correctamente, las conversiones que se sabe que posiblemente se pierde información y las conversiones en dominios de tipos lo suficientemente diferentes para ameritar explícita notación.

### <a name="explicit-numeric-conversions"></a>Conversiones numéricas explícitas

Las conversiones numéricas explícitas son las conversiones de un *numeric_type* a otro *numeric_type* para que una conversión numérica implícita ([deconversionesnuméricasimplícitas](conversions.md#implicit-numeric-conversions)) aún no existe:

*  Desde `sbyte` a `byte`, `ushort`, `uint`, `ulong`, o `char`.
*  Desde `byte` a `sbyte` y `char`.
*  Desde `short` a `sbyte`, `byte`, `ushort`, `uint`, `ulong`, o `char`.
*  Desde `ushort` a `sbyte`, `byte`, `short`, o `char`.
*  Desde `int` a `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, o `char`.
*  Desde `uint` a `sbyte`, `byte`, `short`, `ushort`, `int`, o `char`.
*  Desde `long` a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, o `char`.
*  Desde `ulong` a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, o `char`.
*  Desde `char` a `sbyte`, `byte`, o `short`.
*  Desde `float` a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, o `decimal`.
*  Desde `double` a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, o `decimal`.
*  Desde `decimal` a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, o `double`.

Dado que las conversiones explícitas incluyen todas las conversiones numéricas implícitas y explícitas, siempre es posible convertir desde cualquier *numeric_type* a cualquier otro *numeric_type* mediante una expresión de conversión () [Expresiones de conversión](expressions.md#cast-expressions)).

Las conversiones numéricas explícitas producir pérdida de información o incluso producen que se produzcan excepciones. Una conversión numérica explícita se procesa como sigue:

*  Para una conversión de un tipo integral a otro tipo entero, el procesamiento depende del contexto de comprobación de desbordamiento ([los operadores checked y unchecked](expressions.md#the-checked-and-unchecked-operators)) en que se toma la conversión colocar:
    * En un `checked` contexto, la conversión se realiza correctamente si el valor del operando de origen está dentro del intervalo del tipo de destino, pero inicia una `System.OverflowException` si el valor del operando de origen está fuera del intervalo del tipo de destino.
    * En un `unchecked` contexto, la conversión siempre se realiza correctamente y continúa como sigue.
        * Si el tipo de origen es mayor que el tipo de destino, el valor de origen se trunca al descartar sus bits "extra" más significativos. El resultado se trata como un valor del tipo de destino.
        * Si el tipo de origen es menor que el tipo de destino, el valor de origen se amplía mediante signos o ceros para que tenga el mismo tamaño que el tipo de destino. La ampliación mediante signos se usa si el tipo de origen tiene signo; se emplea ampliación mediante ceros si el tipo de origen no tiene signo. El resultado se trata como un valor del tipo de destino.
        * Si el tipo de origen es del mismo tamaño que el tipo de destino, el valor de origen se trata como un valor del tipo de destino.
*  Para una conversión de `decimal` a un tipo entero, el valor de origen se redondea hacia cero al valor entero más cercano, y este valor entero se convierte en el resultado de la conversión. Si el valor entero resultante está fuera del intervalo del tipo de destino, un `System.OverflowException` se produce.
*  Para una conversión de `float` o `double` a un tipo entero, el procesamiento depende del contexto de comprobación de desbordamiento ([los operadores checked y unchecked](expressions.md#the-checked-and-unchecked-operators)) en que se toma la conversión colocar:
    * En un `checked` contexto, la conversión continúa como sigue:
        * Si el valor del operando es NaN o infinito, un `System.OverflowException` se produce.
        * En caso contrario, el operando de origen se redondea hacia cero al valor entero más cercano. Si este valor entero está dentro del intervalo del tipo de destino, a continuación, este valor es el resultado de la conversión.
        * De lo contrario, se produce una excepción `System.OverflowException`.
    * En un `unchecked` contexto, la conversión siempre se realiza correctamente y continúa como sigue.
        * Si el valor del operando es NaN o infinito, el resultado de la conversión es un valor no especificado del tipo de destino.
        * En caso contrario, el operando de origen se redondea hacia cero al valor entero más cercano. Si este valor entero está dentro del intervalo del tipo de destino, a continuación, este valor es el resultado de la conversión.
        * En caso contrario, el resultado de la conversión es un valor no especificado del tipo de destino.
*  Para una conversión de `double` a `float`, `double` valor se redondea al más cercano `float` valor. Si el `double` valor es demasiado pequeño para representarse como un `float`, el resultado se convierte en cero positivo o cero negativo. Si el `double` valor es demasiado grande para representarse como un `float`, el resultado es infinito positivo o infinito negativo. Si el `double` valor es NaN, el resultado también es NaN.
*  Para una conversión de `float` o `double` a `decimal`, el valor de origen se convierte en `decimal` representación y redondea al número más cercano después de la posición decimal 28 si es necesario ([el tipo decimal](types.md#the-decimal-type)). Si el valor de origen es demasiado pequeño para representarse como un `decimal`, el resultado se convierte en cero. Si el valor de origen es NaN, infinito o demasiado grande para representarse como un `decimal`, un `System.OverflowException` se produce.
*  Para una conversión de `decimal` a `float` o `double`, `decimal` valor se redondea al más cercano `double` o `float` valor. Aunque esta conversión puede perder precisión, nunca hace que se produzca una excepción.

### <a name="explicit-enumeration-conversions"></a>Conversiones explícitas de enumeración

Las conversiones explícitas de enumeración son:

*  Desde `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, o `decimal` a cualquier *enum_type*.
*  Desde cualquier *enum_type* a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, o `decimal`.
*  Desde cualquier *enum_type* a cualquier otro *enum_type*.

Una conversión explícita de enumeración entre dos tipos se procesa al tratar cualquier participan *enum_type* como el tipo subyacente de la que *enum_type*a continuación, realiza un implícita o explícita conversión numérica entre los tipos resultantes. Por ejemplo, dada una *enum_type* `E` con y el tipo subyacente de `int`, una conversión de `E` a `byte` se procesa como una conversión numérica explícita ([Explicit conversiones numéricas](conversions.md#explicit-numeric-conversions)) desde `int` a `byte`y una conversión de `byte` a `E` se procesa como una conversión numérica implícita ([deconversionesnuméricasimplícitas](conversions.md#implicit-numeric-conversions)) desde `byte` a `int`.

### <a name="explicit-nullable-conversions"></a>Conversiones explícitas que acepta valores null

***Las conversiones que aceptan valores NULL explícitas*** predefinidos de permitir las conversiones explícitas que funcionan en tipos de valor distinto de NULL para utilizarse también con los formularios de esos tipos que aceptan valores NULL. Para cada una de las conversiones explícitas predefinidas que convierten de un tipo de valor distinto de NULL `S` a un tipo de valor distinto de NULL `T` ([conversión de identidad](conversions.md#identity-conversion), [deconversionesnuméricasimplícitas](conversions.md#implicit-numeric-conversions), [Conversiones implícitas de enumeración](conversions.md#implicit-enumeration-conversions), [conversiones numéricas explícitas](conversions.md#explicit-numeric-conversions), y [conversiones explícitas de enumeración](conversions.md#explicit-enumeration-conversions)), lo siguiente Existen las conversiones que aceptan valores NULL:

*  Una conversión explícita de `S?` a `T?`.
*  Una conversión explícita de `S` a `T?`.
*  Una conversión explícita de `S?` a `T`.

Evaluación de una conversión que acepta valores NULL en función de una conversión subyacente de `S` a `T` continúa como sigue:

*  Si la conversión que acepta valores NULL es desde `S?` a `T?`:
    * Si el valor de origen es null (`HasValue` propiedad es false), el resultado es el valor null del tipo `T?`.
    * En caso contrario, la conversión se evalúa como un desajuste de `S?` a `S`, seguido de la conversión subyacente de `S` a `T`, seguido de un ajuste de `T` a `T?`.
*  Si la conversión que acepta valores NULL es desde `S` a `T?`, la conversión se evalúa como la conversión subyacente de `S` a `T` seguida de un ajuste de `T` a `T?`.
*  Si la conversión que acepta valores NULL es desde `S?` a `T`, la conversión se evalúa como un desajuste de `S?` a `S` seguido de la conversión subyacente de `S` a `T`.

Tenga en cuenta que un intento para desempaquetar un valor que acepta valores NULL producirá una excepción si el valor es `null`.

### <a name="explicit-reference-conversions"></a>Conversiones explícitas de referencia

Las conversiones explícitas de referencia son:

*  Desde `object` y `dynamic` a cualquier otro *reference_type*.
*  Desde cualquier *class_type* `S` a cualquier *class_type* `T`, que proporciona `S` es una clase base de `T`.
*  Desde cualquier *class_type* `S` a cualquier *interface_type* `T`, que proporciona `S` no está sellado y proporciona `S` no implementa `T`.
*  Desde cualquier *interface_type* `S` a cualquier *class_type* `T`, que proporciona `T` no está sellado o proporcionado `T` implementa `S`.
*  Desde cualquier *interface_type* `S` a cualquier *interface_type* `T`, que proporciona `S` no se deriva `T`.
*  Desde un *array_type* `S` con un tipo de elemento `SE` a un *array_type* `T` con un tipo de elemento `TE`, siempre que todos los elementos siguientes son verdaderas:
    * `S` y `T` difieren solo en el tipo de elemento. En otras palabras, `S` y `T` tienen el mismo número de dimensiones.
    * Ambos `SE` y `TE` son *reference_type*s.
    * Existe una conversión explícita de referencia de `SE` a `TE`.
*  Desde `System.Array` y las interfaces que implementa a cualquier *array_type*.
*  Desde un tipo de matriz unidimensional `S[]` a `System.Collections.Generic.IList<T>` y sus interfaces base, siempre que una conversión explícita de referencia de `S` a `T`.
*  Desde `System.Collections.Generic.IList<S>` y sus interfaces base a un tipo de matriz unidimensional `T[]`, siempre que hay una conversión explícita de referencia o identidad de `S` a `T`.
*  Desde `System.Delegate` y las interfaces que implementa a cualquier *delegate_type*.
*  Un tipo de referencia a un tipo de referencia `T` si tiene una conversión de referencia explícita a un tipo de referencia `T0` y `T0` tiene una conversión de identidad `T`.
*  Un tipo de referencia a un tipo de interfaz o delegado `T` si tiene una conversión de referencia explícita a un tipo de interfaz o delegado `T0` y `T0` es convertible de varianza a `T` o `T` es convertir a varianza `T0` ([conversión de varianza](interfaces.md#variance-conversion)).
*  Desde `D<S1...Sn>` a `D<T1...Tn>` donde `D<X1...Xn>` es un tipo de delegado genérico, `D<S1...Sn>` no es compatible con o idénticos a `D<T1...Tn>`y para cada parámetro de tipo `Xi` de `D` contiene la siguiente:
    * Si `Xi` es invariable, a continuación, `Si` es idéntico a `Ti`.
    * Si `Xi` es covariante, a continuación, hay una conversión de referencia o identidad implícita o explícita desde `Si` a `Ti`.
    * Si `Xi` es contravariante, a continuación, `Si` y `Ti` son idénticos o ambos tipos de referencia.
*  Conversiones explícitas que implica parámetros de tipo que se sabe que son tipos de referencia. Para obtener más información sobre las conversiones explícitas que implica parámetros de tipo, consulte [las conversiones explícitas que implica parámetros de tipo](conversions.md#explicit-conversions-involving-type-parameters).

Las conversiones explícitas de referencia son aquellas conversiones entre tipos de referencia que requieren comprobaciones en tiempo de ejecución para garantizar que son correctos.

Para que una conversión de referencia explícita sea correcta en tiempo de ejecución, el valor del operando de origen debe ser `null`, o el tipo real del objeto al que hace referencia el operando de origen debe ser un tipo que pueda convertirse al tipo de destino mediante una referencia implícita conversión ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) o una conversión boxing ([conversiones Boxing](conversions.md#boxing-conversions)). Si se produce un error en una conversión explícita de referencia, un `System.InvalidCastException` se produce.

Las conversiones de referencia, implícitas o explícitas, nunca cambian la identidad referencial del objeto que se va a convertir. En otras palabras, mientras que una conversión de referencia puede cambiar el tipo de la referencia, nunca cambia el tipo o valor del objeto que hace referencia.

### <a name="unboxing-conversions"></a>Conversiones unboxing

Una conversión unboxing permite un tipo de referencia para convertirse explícitamente a un *value_type*. Existe una conversión unboxing de los tipos `object`, `dynamic` y `System.ValueType` a cualquier *non_nullable_value_type*y desde cualquier *interface_type* a cualquier *non_ nullable_value_type* que implementa el *interface_type*. Escriba además `System.Enum` puede aplicar la conversión unboxing a cualquier *enum_type*.

Existe una conversión unboxing de un tipo de referencia a un *nullable_type* si existe una conversión unboxing del tipo de referencia subyacente *non_nullable_value_type* de la  *nullable_type*.

Un tipo de valor `S` tiene una conversión unboxing de un tipo de interfaz `I` si tiene una conversión unboxing de un tipo de interfaz `I0` y `I0` tiene una conversión de identidad a `I`.

Un tipo de valor `S` tiene una conversión unboxing de un tipo de interfaz `I` si tiene una conversión unboxing de un tipo de interfaz o delegado `I0` y `I0` es convertible de varianza a `I` o `I`es convertible de varianza a `I0` ([conversión de varianza](interfaces.md#variance-conversion)).

Una operación de conversión unboxing consiste en comprobar primero que la instancia de objeto es un valor con conversión boxing de la dada *value_type*y, a continuación, copia el valor fuera de la instancia. Conversión unboxing a una referencia nula a un *nullable_type* genera el valor null de la *nullable_type*. Un struct puede aplicar la conversión unboxing del tipo `System.ValueType`, ya que es una clase base para todas las estructuras ([herencia](structs.md#inheritance)).

Se describe más detalladamente en las conversiones unboxing [Conversiones Unboxing](types.md#unboxing-conversions).

### <a name="explicit-dynamic-conversions"></a>Conversiones explícitas de dinámicas

Existe una conversión explícita de dinámica de una expresión de tipo `dynamic` a cualquier tipo `T`. La conversión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)), lo que significa que se buscará una conversión explícita en tiempo de ejecución desde el tipo de tiempo de ejecución de la expresión `T`. Si no se encuentra ninguna conversión, se produce una excepción en tiempo de ejecución.

Si el enlace dinámico de la conversión no es el deseado, la expresión se puede convertir en primer lugar a `object`y, a continuación, al tipo deseado.

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

El ejemplo siguiente muestra las conversiones explícitas de dinámicas:
```csharp
object o  = "1";
dynamic d = "2";

var c1 = (C)o; // Compiles, but explicit reference conversion fails
var c2 = (C)d; // Compiles and user defined conversion succeeds
```

La conversión de mejor `o` a `C` se encuentra en tiempo de compilación para ser una conversión explícita de referencia. Esto se produce un error en tiempo de ejecución, porque `"1"` no es en realidad un `C`. La conversión de `d` a `C` sin embargo, como una conversión explícita dinámica, se suspende en tiempo de ejecución, donde un usuario definió la conversión del tipo de tiempo de ejecución de `d`  --  `string` --a `C` se encuentra, y se realiza correctamente.

### <a name="explicit-conversions-involving-type-parameters"></a>Conversiones explícitas que implica parámetros de tipo

Existen las siguientes conversiones explícitas para un parámetro de tipo dado `T`:

*  De la clase base eficaz `C` de `T` a `T` y desde cualquier clase base de `C` a `T`. En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como una conversión unboxing. En caso contrario, la conversión se ejecuta como una conversión de referencia explícita o conversión de identidad.
*  De cualquier tipo de interfaz a `T`. En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como una conversión unboxing. En caso contrario, la conversión se ejecuta como una conversión de referencia explícita o conversión de identidad.
*  Desde `T` a cualquier *interface_type* `I` proporcionado no exista ya una conversión implícita de `T` a `I`. En tiempo de ejecución, si `T` es un tipo de valor, la conversión se ejecuta como una conversión boxing seguida de una conversión explícita de referencia. En caso contrario, la conversión se ejecuta como una conversión de referencia explícita o conversión de identidad.
*  Un parámetro de tipo `U` a `T`, que proporciona `T` depende `U` ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)). En tiempo de ejecución, si `U` es un tipo de valor, a continuación, `T` y `U` son necesariamente del mismo tipo y se realiza ninguna conversión. De lo contrario, si `T` es un tipo de valor, la conversión se ejecuta como una conversión unboxing. En caso contrario, la conversión se ejecuta como una conversión de referencia explícita o conversión de identidad.

Si `T` es sabe que un tipo de referencia, las conversiones anteriores están clasificadas como conversiones explícitas de referencia ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)). Si `T` no es sabe que un tipo de referencia, las conversiones anteriores se clasifican como conversiones unboxing ([Conversiones Unboxing](conversions.md#unboxing-conversions)).

Las reglas anteriores no permiten que una conversión explícita directa de un parámetro de tipo sin restricciones a un tipo de interfaz, lo que podría resultar sorprendente. La razón de esta regla es evitar la confusión y realizar la semántica de estas conversiones no cifradas. Por ejemplo, consideremos la siguiente declaración:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)t;                // Error 
    }
}
```

Si la conversión explícita directa de `t` a `int` permiten, fácilmente uno podría esperar que `X<int>.F(7)` devolvería `7L`. Sin embargo, no es así porque las conversiones numéricas estándares solo se consideran cuando se conocen los tipos numéricos en tiempo de enlace. Para hacer que la semántica claras y en el ejemplo anterior se debe escribir:
```csharp
class X<T>
{
    public static long F(T t) {
        return (long)(object)t;        // Ok, but will only work when T is long
    }
}
```

Este código se compilará, pero si se ejecuta `X<int>.F(7)` , a continuación, generaría una excepción en tiempo de ejecución, desde una conversión boxing `int` no se puede convertir directamente a un `long`.

### <a name="user-defined-explicit-conversions"></a>Conversiones explícitas definidas por el usuario

Una conversión explícita definido por el usuario consta de una conversión explícita opcional estándar, seguida por la ejecución de un operador de conversión implícita o explícita definido por el usuario, seguido de otra conversión explícita opcional estándar. Las reglas exactas para evaluar las conversiones explícitas definidas por el usuario se describen en [de procesamiento de las conversiones explícitas definidas por el usuario](conversions.md#processing-of-user-defined-explicit-conversions).

## <a name="standard-conversions"></a>Conversiones estándar

Las conversiones estándar son aquellas conversiones predefinidas que se pueden producir como parte de una conversión definida por el usuario.

### <a name="standard-implicit-conversions"></a>Conversiones implícitas estándar

Las siguientes conversiones implícitas se clasifican como conversiones implícitas estándar:

*  Conversiones de identidad ([conversión de identidad](conversions.md#identity-conversion))
*  Conversiones numéricas implícitas ([conversiones numéricas implícitas](conversions.md#implicit-numeric-conversions))
*  Las conversiones implícitas que acepta valores null ([conversiones que aceptan valores NULL implícitas](conversions.md#implicit-nullable-conversions))
*  Conversiones implícitas de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions))
*  Las conversiones boxing ([conversiones Boxing](conversions.md#boxing-conversions))
*  Conversiones implícitas de expresión constante ([conversiones implícitas de dinámicas](conversions.md#implicit-dynamic-conversions))
*  Las conversiones implícitas que implica parámetros de tipo ([conversiones implícitas que implica parámetros de tipo](conversions.md#implicit-conversions-involving-type-parameters))

Las conversiones implícitas estándares excluyen específicamente las conversiones implícitas definido por el usuario.

### <a name="standard-explicit-conversions"></a>Conversiones explícitas estándar

Las conversiones explícitas estándares son todas las conversiones implícitas estándares más el subconjunto de las conversiones explícitas para el que existe una conversión implícita estándar opuesta. En otras palabras, si un estándar implícito no existe conversión de un tipo `A` a un tipo `B`, a continuación, existe una conversión explícita estándar de tipo `A` escriba `B` y del tipo `B` escriba `A`.

## <a name="user-defined-conversions"></a>Conversiones definidas por el usuario

C# permite que las conversiones implícitas y explícitas predefinidas para ampliarse por ***conversiones definidas por el usuario***. Conversiones definidas por el usuario se introducen mediante la declaración de operadores de conversión ([operadores de conversión](classes.md#conversion-operators)) en tipos de clase y estructura.

### <a name="permitted-user-defined-conversions"></a>Permite conversiones definidas por el usuario

C# permite que solo determinadas conversiones definidas por el usuario a declararse. En concreto, no es posible volver a definir una conversión implícita o explícita ya existente.

Para un tipo de origen dado `S` y tipo de destino `T`si `S` o `T` son tipos que aceptan valores NULL, permiten `S0` y `T0` hacen referencia a sus tipos subyacentes, de lo contrario `S0` y `T0` son igual que `S` y `T` respectivamente. Una clase o struct se puede declarar una conversión de un tipo de origen `S` a un tipo de destino `T` sólo si se cumplen todas las opciones siguientes:

*  `S0` y `T0` son tipos diferentes.
*  Ya sea `S0` o `T0` es el tipo de clase o estructura en la que realiza la declaración del operador.
*  Ni `S0` ni `T0` es un *interface_type*.
*  Excluyendo las conversiones definidas por el usuario, no existe una conversión desde `S` a `T` o desde `T` a `S`.

Las restricciones que se aplican a las conversiones definidas por el usuario se explican con más detalle en [operadores de conversión](classes.md#conversion-operators).

### <a name="lifted-conversion-operators"></a>Operadores de conversión de elevación

Dado un operador de conversión definido por el usuario que convierte de un tipo de valor distinto de NULL `S` a un tipo de valor distinto de NULL `T`, un ***eleva el operador de conversión*** existe que convierte de `S?` a `T?`. Este operador de conversión de elevación realiza un desajuste de `S?` a `S` seguido de la conversión definida por el usuario de `S` a `T` seguida de un ajuste de `T` a `T?`, excepto en que un valor null con valores `S?` pasa directamente a un valor null con valores `T?`.

Un operador de conversión de elevación tiene la misma clasificación implícita o explícita, como su operador de conversión definido por el usuario subyacente. El término "conversión definida por el usuario" se aplica al uso de ambos definido por el usuario y los operadores de conversión de elevación.

### <a name="evaluation-of-user-defined-conversions"></a>Evaluación de conversiones definidas por el usuario

Una conversión definida por el usuario convierte un valor de su tipo, denominado el ***tipo de origen***, a otro tipo, denominado el ***tipo de destino***. Evaluación de una conversión definida por el usuario se centra en encontrar el ***más específica*** operador de conversión definido por el usuario para los tipos de origen y de destino determinados. Esta determinación se divide en varios pasos:

*  Encontrar el conjunto de clases y structs desde el que se considerarán los operadores de conversión definido por el usuario. Este conjunto está formado por el tipo de origen y sus clases base y el tipo de destino y sus clases base (con los supuestos implícitos que sólo las clases y structs pueden declarar los operadores definidos por el usuario, y que los tipos de clase no tienen ninguna clase base). Para los fines de este paso, si el tipo de origen o destino es un *nullable_type*, su tipo subyacente se usa en su lugar.
*  Desde ese conjunto de tipos, determinar que definido por el usuario y eleva los operadores de conversión son aplicables. Para que un operador de conversión sea aplicable, debe ser posible realizar una conversión estándar ([conversiones estándar](conversions.md#standard-conversions)) del tipo de origen que el operando tipo del operador y debe ser posible realizar una conversión estándar desde el tipo de resultado del operador para el tipo de destino.
*  Desde el conjunto de operadores definidos por el usuario aplicables, determinar qué operador es la más específica sin ambigüedades. En términos generales, el operador más específico es el operador cuyo tipo de operando es "más cercana" al tipo de origen y cuyo tipo de resultado es "más cercana" al tipo de destino. Operadores de conversión de elevación se prefieren los operadores de conversión definido por el usuario. Las reglas exactas para establecer el operador de conversión definido por el usuario más específico se definen en las secciones siguientes.

Una vez que se ha identificado un operador de conversión definido por el usuario más específico, la ejecución real de la conversión definida por el usuario implica hasta tres pasos:

*  En primer lugar, si es necesario, realizar una conversión estándar del tipo de origen para el tipo de operando del operador de conversión definido por el usuario o la elevación.
*  A continuación, invocar el operador de conversión definido por el usuario o la elevación para llevar a cabo la conversión.
*  Por último, si es necesario, realizar una conversión estándar desde el tipo de resultado del operador de conversión definido por el usuario o la elevación para el tipo de destino.

Evaluación de una conversión definida por el usuario nunca implica a más de un operador de conversión definido por el usuario o la elevación. En otras palabras, una conversión de tipo `S` escriba `T` ejecutará nunca primero una conversión definida por el usuario de `S` a `X` y, a continuación, ejecutar una conversión definida por el usuario de `X` a `T`.

Las definiciones exactas de la evaluación de las conversiones implícitas o explícitas definidas por el usuario se proporcionan en las secciones siguientes. Las definiciones de hacer uso de los siguientes términos:

*  Si una conversión implícita estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) existe desde un tipo `A` a un tipo `B`y si no `A` ni `B` son *interface_type*s, `A` se dice que ***abarcado por*** `B`, y `B` se dice que ***abarcan*** `A`.
*  El ***tipo más incluyente*** en un conjunto de tipos es un tipo que abarca todos los demás tipos en el conjunto. Si ningún tipo único abarca a todos los demás tipos, el conjunto no tiene ningún tipo más completa. En términos más intuitivos, el tipo más incluyente es el tipo "más grande" en el conjunto, un tipo al que puede convertirse implícitamente cada uno de los otros tipos.
*  El ***un tipo más abarcado*** en un conjunto de tipos es un tipo que está incluido en todos los demás tipos en el conjunto. Si no los tipos es abarcado por todos los demás tipos, el conjunto más no ha abarcado a tipo. En términos más intuitivos, el tipo más abarcado es el tipo "más pequeño" en el conjunto, un tipo que pueda convertirse implícitamente a cada uno de los otros tipos.

### <a name="processing-of-user-defined-implicit-conversions"></a>Procesamiento de las conversiones implícitas definido por el usuario

Una conversión implícita del tipo definido por el usuario `S` escriba `T` se procesa como sigue:

*  Determinar los tipos `S0` y `T0`. Si `S` o `T` son tipos que aceptan valores NULL, `S0` y `T0` son sus tipos subyacentes, de lo contrario, `S0` y `T0` son iguales a `S` y `T` respectivamente.
*  Busque el conjunto de tipos, `D`, desde qué conversión definida por el usuario se considerarán los operadores. Este conjunto consta de `S0` (si `S0` es una clase o struct), las clases base de `S0` (si `S0` es una clase), y `T0` (si `T0` es una clase o struct).
*  Busque el conjunto de operadores de conversión definido por el usuario y la elevación aplicables, `U`. Este conjunto consta de los operadores de conversión implícita de elevación y definido por el usuario declarados por las clases o structs en `D` que convertir de un tipo que abarca `S` a un tipo englobado por `T`. Si `U` está vacía, la conversión es indefinida y se produce un error de tiempo de compilación.
*  Busca el tipo de origen más específico, `SX`, de los operadores de `U`:
    * Si cualquiera de los operadores en `U` convertir de `S`, a continuación, `SX` es `S`.
    * En caso contrario, `SX` es el tipo más abarcado en el conjunto combinado de tipos de origen de los operadores de `U`. Si exactamente uno más abarcado no se encuentra el tipo, a continuación, la conversión es ambigua y se produce un error de tiempo de compilación.
*  Busca el tipo de destino más específico, `TX`, de los operadores de `U`:
    * Si cualquiera de los operadores en `U` convertir en `T`, a continuación, `TX` es `T`.
    * En caso contrario, `TX` es el tipo más completa en el conjunto combinado de tipos de destino de los operadores de `U`. Si no se encuentra exactamente un tipo más incluyente, a continuación, la conversión es ambigua y se produce un error de tiempo de compilación.
*  Buscar el operador de conversión más específico:
    * Si `U` contiene exactamente un operador de conversión definido por el usuario que convierte de `SX` a `TX`, este es el operador de conversión más específico.
    * De lo contrario, si `U` contiene exactamente un operador de conversión de elevación que convierte de `SX` a `TX`, este es el operador de conversión más específico.
    * En caso contrario, la conversión es ambigua y se produce un error de tiempo de compilación.
*  Por último, aplique la conversión:
    * Si `S` no `SX`, a continuación, una conversión implícita estándar de `S` a `SX` se lleva a cabo.
    * Se invoca el operador de conversión más específico para convertir de `SX` a `TX`.
    * Si `TX` no `T`, a continuación, una conversión implícita estándar de `TX` a `T` se lleva a cabo.

### <a name="processing-of-user-defined-explicit-conversions"></a>Procesamiento de las conversiones explícitas definidas por el usuario

Una conversión explícita de tipo definido por el usuario `S` escriba `T` se procesa como sigue:

*  Determinar los tipos `S0` y `T0`. Si `S` o `T` son tipos que aceptan valores NULL, `S0` y `T0` son sus tipos subyacentes, de lo contrario, `S0` y `T0` son iguales a `S` y `T` respectivamente.
*  Busque el conjunto de tipos, `D`, desde qué conversión definida por el usuario se considerarán los operadores. Este conjunto consta de `S0` (si `S0` es una clase o struct), las clases base de `S0` (si `S0` es una clase), `T0` (si `T0` es una clase o struct) y las clases base de `T0` (si `T0`es una clase).
*  Busque el conjunto de operadores de conversión definido por el usuario y la elevación aplicables, `U`. Este conjunto consta de definido por el usuario y elevación implícita o declaran de operadores de conversión explícitos por las clases o structs en `D` que convertir de un tipo que abarca o abarcado por `S` a un tipo que abarca o abarcado por `T`. Si `U` está vacía, la conversión es indefinida y se produce un error de tiempo de compilación.
*  Busca el tipo de origen más específico, `SX`, de los operadores de `U`:
    * Si cualquiera de los operadores en `U` convertir de `S`, a continuación, `SX` es `S`.
    * En caso contrario, si cualquiera de los operadores en `U` convertir de tipos que abarca `S`, a continuación, `SX` es el tipo más abarcado en el conjunto combinado de tipos de origen de estos operadores. Si no hay más abarcado se encuentra un tipo, a continuación, la conversión es ambigua y se produce un error de tiempo de compilación.
    * En caso contrario, `SX` es el tipo más completa en el conjunto combinado de tipos de origen de los operadores de `U`. Si no se encuentra exactamente un tipo más incluyente, a continuación, la conversión es ambigua y se produce un error de tiempo de compilación.
*  Busca el tipo de destino más específico, `TX`, de los operadores de `U`:
    * Si cualquiera de los operadores en `U` convertir en `T`, a continuación, `TX` es `T`.
    * En caso contrario, si cualquiera de los operadores en `U` convertir a tipos abarcados por `T`, a continuación, `TX` es el tipo más completa en el conjunto combinado de tipos de destino de los operadores. Si no se encuentra exactamente un tipo más incluyente, a continuación, la conversión es ambigua y se produce un error de tiempo de compilación.
    * En caso contrario, `TX` es el tipo más abarcado en el conjunto combinado de tipos de destino de los operadores de `U`. Si no hay más abarcado se encuentra un tipo, a continuación, la conversión es ambigua y se produce un error de tiempo de compilación.
*  Buscar el operador de conversión más específico:
    * Si `U` contiene exactamente un operador de conversión definido por el usuario que convierte de `SX` a `TX`, este es el operador de conversión más específico.
    * De lo contrario, si `U` contiene exactamente un operador de conversión de elevación que convierte de `SX` a `TX`, este es el operador de conversión más específico.
    * En caso contrario, la conversión es ambigua y se produce un error de tiempo de compilación.
*  Por último, aplique la conversión:
    * Si `S` no `SX`, a continuación, una conversión explícita estándar de `S` a `SX` se lleva a cabo.
    * Se invoca el operador de conversión definido por el usuario más específico para convertir de `SX` a `TX`.
    * Si `TX` no `T`, a continuación, una conversión explícita estándar de `TX` a `T` se lleva a cabo.

## <a name="anonymous-function-conversions"></a>Conversiones de función anónima

Un *anonymous_method_expression* o *lambda_expression* se clasifica como una función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)). La expresión no tiene un tipo, pero puede convertirse implícitamente a un tipo de árbol de expresión o delegado compatible. En concreto, una función anónima `F` es compatible con un tipo de delegado `D` proporcionado:

*  Si `F` contiene un *anonymous_function_signature*, a continuación, `D` y `F` tienen el mismo número de parámetros.
*  Si `F` no contiene un *anonymous_function_signature*, a continuación, `D` puede tener cero o más parámetros de cualquier tipo, siempre y cuando ningún parámetro de `D` tiene la `out` modificador de parámetro.
*  Si `F` tiene una lista de parámetros con tipo explícito, cada parámetro de `D` tiene el mismo tipo y modificadores como el parámetro correspondiente en `F`.
*  Si `F` tiene una lista de parámetros con tipo implícito, `D` no tiene ningún `ref` o `out` parámetros.
*  Si el cuerpo de `F` es una expresión y `D` tiene un `void` tipo de valor devuelto o `F` es asincrónico y `D` tiene el tipo de valor devuelto `Task`, entonces, cuando cada parámetro de `F` se le asigna el tipo de la el parámetro correspondiente en `D`, el cuerpo de `F` es una expresión válida (wrt [expresiones](expressions.md)) que se permitirían como un *statement_expression* ([Las instrucciones de expresión](statements.md#expression-statements)).
*  Si el cuerpo de `F` es un bloque de instrucciones y `D` tiene un `void` tipo de valor devuelto o `F` es asincrónico y `D` tiene el tipo de valor devuelto `Task`, entonces, cuando cada parámetro de `F` se le asigna el tipo de el parámetro correspondiente en `D`, el cuerpo de `F` es un bloque de instrucciones válido (wrt [bloques](statements.md#blocks)) no `return` instrucción especifica una expresión.
*  Si el cuerpo de `F` es una expresión, y *cualquier* `F` es que no es asincrónico y `D` tiene un tipo de valor devuelto distinto de void `T`, *o* `F` es asincrónico y `D` tiene un tipo de valor devuelto `Task<T>`, entonces, cuando cada parámetro de `F` tiene el tipo del parámetro correspondiente en `D`, el cuerpo de `F` es una expresión válida (wrt [ Las expresiones](expressions.md)) que es implícitamente convertible a `T`.
*  Si el cuerpo de `F` es un bloque de instrucciones, y *cualquier* `F` es que no es asincrónico y `D` tiene un tipo de valor devuelto distinto de void `T`, *o* `F` es asincrónico y `D` tiene un tipo de valor devuelto `Task<T>`, entonces, cuando cada parámetro de `F` tiene el tipo del parámetro correspondiente en `D`, el cuerpo de `F` es un bloque de instrucciones válido (wrt [bloques ](statements.md#blocks)) con un punto de conexión que no sea accesible en el que cada `return` instrucción especifica una expresión que es implícitamente convertible a `T`.

Para mayor brevedad, esta sección usa la forma abreviada para los tipos de tareas `Task` y `Task<T>` ([funciones asincrónicas](classes.md#async-functions)).

Una expresión lambda `F` es compatible con un tipo de árbol de expresión `Expression<D>` si `F` es compatible con el tipo de delegado `D`. Tenga en cuenta que esto no es aplicable a los métodos anónimos, solo las expresiones lambda.

Algunas expresiones lambda no se puede convertir a tipos de árbol de expresión: Aunque la conversión *existe*, se produce un error en tiempo de compilación. Este es el caso si la expresión lambda:

*  Tiene un *bloque* cuerpo
*  Contiene los operadores de asignación simple o compuesta
*  Contiene una expresión enlazada dinámicamente
*  Es la asincronía

Los ejemplos siguientes usan un tipo de delegado genérico `Func<A,R>` que representa una función que toma un argumento de tipo `A` y devuelve un valor de tipo `R`:
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
los tipos de parámetro y valor devuelto de cada función anónima se determinan a partir del tipo de la variable al que se asigna la función anónima.

La primera asignación convierte correctamente la función anónima en el tipo de delegado `Func<int,int>` porque, cuando `x` tiene tipo `int`, `x+1` es una expresión válida que sea implícitamente convertible al tipo `int`.

Del mismo modo, la segunda asignación convierte correctamente la función anónima en el tipo de delegado `Func<int,double>` porque el resultado de `x+1` (de tipo `int`) es implícitamente convertible al tipo `double`.

Sin embargo, la asignación de terceros es un error en tiempo de compilación porque, cuando `x` tiene tipo `double`, el resultado de `x+1` (de tipo `double`) no es implícitamente convertible al tipo `int`.

El cuarta asignación convierte correctamente la función anónima asincrónico para el tipo de delegado `Func<int, Task<int>>` porque el resultado de `x+1` (de tipo `int`) es implícitamente convertible al tipo de resultado `int` del tipo de tarea `Task<int>`.

Funciones anónimas pueden influir en la resolución de sobrecarga y participar en la inferencia de tipos. Consulte [miembros de función](expressions.md#function-members) para obtener más detalles.

### <a name="evaluation-of-anonymous-function-conversions-to-delegate-types"></a>Evaluación de función anónima conversiones a tipos de delegado

Conversión de una función anónima para un tipo de delegado, genera una instancia de delegado que hace referencia a la función anónima y el conjunto de variables externas capturadas que están activos en el momento de la evaluación (posiblemente vacío). Cuando se invoca el delegado, se ejecuta el cuerpo de la función anónima. Se ejecuta el código en el cuerpo con el conjunto de variables externas capturadas al que hace referencia el delegado.

La lista de invocaciones de un delegado generada a partir de una función anónima contiene una sola entrada. El objeto de destino exacto y el método de destino del delegado se especifican. En concreto, no se especifica si el objeto de destino del delegado es `null`, el `this` valor del miembro de función envolvente o algún otro objeto.

Las conversiones de funciones anónimas semánticamente idénticas con el mismo conjunto (posiblemente vacía) de instancias de variables externas capturadas en los mismos tipos de delegado se permiten (aunque no es necesario) para devolver la misma instancia de delegado. El término semánticamente idéntico se usa aquí para indicar que la ejecución de las funciones anónimas, en todos los casos, generará el mismo efecto que tiene los mismos argumentos. Esta regla permite que el código como el siguiente que se optimizarán.

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

Puesto que los dos delegados de función anónima tienen igual (vacío) conjunto de variables externas capturadas y puesto que las funciones anónimas son semánticamente idénticas, el compilador puede tener delegados que hacen referencia al mismo método de destino. De hecho, el compilador puede devolver la misma instancia de delegado de ambas expresiones de función anónima.

### <a name="evaluation-of-anonymous-function-conversions-to-expression-tree-types"></a>Evaluación de función anónima conversiones a tipos de árbol de expresión

Conversión de una función anónima en un tipo de árbol de expresión genera un árbol de expresión ([tipos de árbol de expresión](types.md#expression-tree-types)). Más concretamente, la evaluación de la conversión de función anónima conduce a la construcción de una estructura de objeto que representa la estructura de la propia función anónima. La estructura de árbol de expresión, así como el proceso exacto para crearlo, precisa son implementación definida.

### <a name="implementation-example"></a>Ejemplo de implementación

Esta sección describe una posible implementación de las conversiones de función anónima en cuanto a otras construcciones de C#. La implementación que se describen aquí se basa en los mismos principios utilizados por el compilador de C# de Microsoft, pero no es una implementación obligatoria, ni es el único posible. Mencionan brevemente conversiones en árboles de expresión, como su semántica exacta está fuera del ámbito de esta especificación.

El resto de esta sección proporciona varios ejemplos de código que contiene las funciones anónimas con diferentes características. Para cada ejemplo, se proporciona una traducción correspondiente al código que usa solo otras construcciones de C#. En los ejemplos, el identificador `D` supone representan el siguiente tipo de delegado:
```csharp
public delegate void D();
```

La forma más sencilla de una función anónima es uno que no captura variables externas:
```csharp
class Test
{
    static void F() {
        D d = () => { Console.WriteLine("test"); };
    }
}
```

Esto se puede traducir a una instancia de delegado que hace referencia a un método estático generado por el compilador en el que se coloca el código de la función anónima:
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

En el ejemplo siguiente, la función anónima, hace referencia a miembros de instancia de `this`:
```csharp
class Test
{
    int x;

    void F() {
        D d = () => { Console.WriteLine(x); };
    }
}
```

Esto se puede traducir a un método de instancia generado por el compilador que contiene el código de la función anónima:
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

La duración de la variable local debe ser ampliada al menos la vigencia del delegado de función anónima. Esto puede lograrse mediante "uno" de la variable local en un campo de una clase generada por compilador. Creación de instancias de la variable local ([creación de instancias de las variables locales](expressions.md#instantiation-of-local-variables)), a continuación, corresponde a la creación de una instancia de la clase generada por compilador y el acceso a la variable local corresponde al acceso a un campo en la instancia de la clase generada por compilador. Además, la función anónima se convierte en un método de instancia de la clase generada por compilador:
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

Por último, la siguiente anónima función capturas `this` , así como dos variables locales con diferentes períodos de duración:
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

En este caso, se crea una clase generada por compilador para cada instrucción bloque en que se capturan las variables locales que las variables locales en los distintos bloques pueden tengan duraciones independientes. Una instancia de `__Locals2`, la clase generada por compilador para el bloque de instrucciones internas, contiene la variable local `z` y un campo que hace referencia a una instancia de `__Locals1`.  Una instancia de `__Locals1`, la clase generada por compilador para el bloque de instrucción externa, contiene la variable local `y` y un campo que hace referencia a `this` del miembro de función envolvente. Con estas estructuras de datos, es posible obtener acceso a todas capturar variables externas a través de una instancia de `__Local2`, y el código de la función anónima, por tanto, se puede implementar como un método de instancia de esa clase.

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

También puede utilizar la misma técnica que se aplica aquí para capturar las variables locales al convertir las funciones anónimas en árboles de expresión: Referencias a los objetos generados por el compilador pueden almacenarse en el árbol de expresión y el acceso a las variables locales se puede representar como campo tiene acceso a estos objetos. La ventaja de este enfoque es que permite que las variables locales "elevación" para compartirse entre los delegados y árboles de expresión.

## <a name="method-group-conversions"></a>Conversiones de métodos de grupo

Una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde un grupo de métodos ([clasificaciones de expresiones](expressions.md#expression-classifications)) a un tipo delegado compatible. Dado un tipo de delegado `D` y una expresión `E` que se clasifica como un grupo de métodos, existe una conversión implícita de `E` a `D` si `E` contiene al menos un método que es aplicable en su forma normal () [Miembro de función aplicable](expressions.md#applicable-function-member)) a una lista de argumentos que se construye mediante el uso de los tipos de parámetros y modificadores de `D`, como se describe en la siguiente.

La aplicación en tiempo de compilación de una conversión de un grupo de métodos `E` a un tipo delegado `D` se describe en la siguiente. Tenga en cuenta que la existencia de una conversión implícita de `E` a `D` no garantiza que la aplicación en tiempo de compilación de la conversión se realizará correctamente sin errores.

*  Un único método `M` se selecciona correspondiente a una invocación de método ([las invocaciones de método](expressions.md#method-invocations)) del formulario `E(A)`, con las siguientes modificaciones:
    * La lista de argumentos `A` es una lista de expresiones, cada uno de ellos clasificadas como una variable y con el tipo y el modificador (`ref` o `out`) del parámetro correspondiente en el *formal_parameter_list* de `D`.
    * Los métodos de candidato considerados son solo aquellos métodos que son aplicables en su forma normal ([miembro de función aplicable](expressions.md#applicable-function-member)), no los aplicables únicamente en su forma expandida.
*  Si el algoritmo de [las invocaciones de método](expressions.md#method-invocations) produce un error, a continuación, se produce un error de tiempo de compilación. En caso contrario, el algoritmo genera un único método mejor `M` tener el mismo número de parámetros como `D` y la conversión se considera que existe.
*  El método seleccionado `M` deben ser compatibles ([compatibilidad de delegado](delegates.md#delegate-compatibility)) con el tipo de delegado `D`, o en caso contrario, se produce un error en tiempo de compilación.
*  Si el método seleccionado `M` es un método de instancia, la expresión de instancia asociada `E` determina el objeto de destino del delegado.
*  Si el método seleccionado M es un método de extensión que se indica mediante un acceso de miembro en una expresión de instancia, esa expresión de instancia determina el objeto de destino del delegado.
*  El resultado de la conversión es un valor de tipo `D`, es decir, un delegado recién creado que hace referencia al objeto de método y el destino seleccionado.
*  Tenga en cuenta que este proceso puede dar lugar a la creación de un delegado a un método de extensión, si el algoritmo de [las invocaciones de método](expressions.md#method-invocations) no puede encontrar un método de instancia, pero se realiza correctamente en el procesamiento de la invocación de `E(A)` como una extensión invocación de método ([las invocaciones de método de extensión](expressions.md#extension-method-invocations)). Un delegado creado, por tanto, captura el método de extensión, así como su primer argumento.

El ejemplo siguiente muestra las conversiones de grupo de métodos:
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

La asignación a `d2` muestra cómo es posible crear un delegado a un método que tiene tipos de parámetro menos derivados (contravariante) y una más derivado a tipo de valor devuelto (covariante).

La asignación a `d3` muestra cómo no existe ninguna conversión si el método no es aplicable.

La asignación a `d4` se muestra cómo el método debe ser aplicable en su forma normal.

La asignación a `d5` se muestra cómo se permiten los tipos de parámetro y valor devuelto del método y delegado difieran solo para los tipos de referencia.

Al igual que con todas las otras conversiones implícitas y explícitas, el operador de conversión puede usarse para realizar explícitamente la conversión de grupos de un método. Por lo tanto, el ejemplo
```csharp
object obj = new EventHandler(myDialog.OkClick);
```
en su lugar se podría escribir
```csharp
object obj = (EventHandler)myDialog.OkClick;
```

Grupos de métodos pueden influir en la resolución de sobrecarga y participar en la inferencia de tipos. Consulte [miembros de función](expressions.md#function-members) para obtener más detalles.

La evaluación de tiempo de ejecución de la conversión de grupos de un método continúa como sigue:

*  Si el método seleccionado en tiempo de compilación es un método de instancia, o es un método de extensión que se obtiene acceso como un método de instancia, el objeto de destino del delegado se determina a partir de la expresión de instancia asociada `E`:
    * Se evalúa la expresión de instancia. Si esta evaluación, produce una excepción, no se ejecuta ningún paso adicional.
    * Si la expresión de instancia es de un *reference_type*, el valor calculado por la expresión de instancia se convierte en el objeto de destino. Si el método seleccionado es un método de instancia y el objeto de destino es `null`, un `System.NullReferenceException` se inicia y no se ejecuta ningún paso adicional.
    * Si la expresión de instancia es de un *value_type*, una operación de conversión boxing ([conversiones Boxing](types.md#boxing-conversions)) se realiza para convertir el valor a un objeto, y este objeto se convierte en el objeto de destino.
*  En caso contrario, el método seleccionado es parte de una llamada al método estático y el objeto de destino del delegado es `null`.
*  Una nueva instancia del tipo de delegado `D` está asignada. Si no hay suficiente memoria disponible para asignar la nueva instancia, un `System.OutOfMemoryException` se inicia y no se ejecuta ningún paso adicional.
*  La nueva instancia de delegado se inicializa con una referencia al método que se determinó en tiempo de compilación y una referencia al objeto de destino calculado anteriormente.
