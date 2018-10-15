# <a name="types"></a>Tipos

Los tipos del lenguaje C# se dividen en dos categorías principales: ***los tipos de valor*** y ***hacen referencia a tipos***. Tipos de valor y tipos de referencia pueden ser ***tipos genéricos***, que tomar una o más ***parámetros de tipo***. Parámetros de tipo pueden designar ambos tipos de valor y tipos de referencia.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

La categoría de tipos de punteros, final solo está disponible en código no seguro. Esto se explica con más detalle en [tipos de puntero](unsafe-code.md#pointer-types).

Tipos de valor difieren de los tipos de referencia en que las variables de los tipos de valor contienen directamente sus datos, mientras que las variables de la referencia de tipos de almacén ***referencias*** a sus datos, el que se conocen como ***objetos***. Tipos de referencia, es posible que dos variables hagan referencia al mismo objeto y, por tanto, las operaciones en una variable afecten al objeto al que hace referencia la otra variable. Con los tipos de valor, cada variable tiene su propia copia de los datos y no es posible que las operaciones en una variable afecten a la otra.

El sistema de tipos de C# está unificado, que un valor de cualquier tipo puede tratarse como un objeto. Todos los tipos de C# directa o indirectamente se derivan del tipo de clase `object`, y `object` es la clase base definitiva de todos los tipos. Los valores de tipos de referencia se tratan como objetos mediante la visualización de los valores como tipo `object`. Los valores de tipos de valor se tratan como objetos mediante la realización de operaciones de conversión boxing y unboxing ([conversiones Boxing y unboxing](types.md#boxing-and-unboxing)).

## <a name="value-types"></a>Tipos de valor

Un tipo de valor es un tipo de estructura o un tipo de enumeración. C# proporciona un conjunto de tipos de struct predefinidos denominados el ***tipos simples***. Los tipos simples se identifican mediante palabras reservadas.

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

A diferencia de una variable de un tipo de referencia, una variable de un tipo de valor puede contener el valor `null` sólo si el tipo de valor es un tipo que acepta valores NULL.  Para cada tipo de valor distinto de null, hay un tipo de valor que acepta valores NULL correspondiente que denota el mismo conjunto de valores más el valor `null`.

La asignación a una variable de un tipo de valor, crea una copia del valor asignado. Esto difiere de la asignación a una variable de un tipo de referencia, que copia la referencia pero no el objeto identificado por la referencia.

### <a name="the-systemvaluetype-type"></a>El tipo System.ValueType

Todos los tipos de valor heredan implícitamente de la clase `System.ValueType`, que, a su vez, hereda de la clase `object`. No es posible que cualquier tipo se derive de un tipo de valor y tipos de valor, por tanto, están sellados implícitamente ([clases Sealed](classes.md#sealed-classes)).

Tenga en cuenta que `System.ValueType` no es un *value_type*. Se trata de un *class_type* de los cuales *value_type*s se deriva automáticamente.

### <a name="default-constructors"></a>Constructores predeterminados

Todos los tipos de valor declaran implícitamente un constructor de instancia sin parámetros público denominado el ***constructor predeterminado***. El constructor predeterminado devuelve una instancia de inicializado en cero se conoce como el ***valor predeterminado*** para el tipo de valor:

*  Para todos los *simple_type*s, el valor predeterminado es el valor producido por un patrón de bits de todos los ceros:
    * Para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, y `ulong`, el valor predeterminado es `0`.
    * Para `char`, el valor predeterminado es `'\x0000'`.
    * Para `float`, el valor predeterminado es `0.0f`.
    * Para `double`, el valor predeterminado es `0.0d`.
    * Para `decimal`, el valor predeterminado es `0.0m`.
    * Para `bool`, el valor predeterminado es `false`.
*  Para un *enum_type* `E`, el valor predeterminado es `0`, convertido al tipo `E`.
*  Para un *struct_type*, el valor predeterminado es el valor generado al establecer todos los campos de tipo de valor en sus valores predeterminados y referencia de todos los campos de tipo a `null`.
*  Para un *nullable_type* el valor predeterminado es una instancia para el que el `HasValue` propiedad es false y el `Value` propiedad no está definida. El valor predeterminado es también conocida como la ***valor null*** del tipo que acepta valores NULL.

Al igual que cualquier otro constructor de instancia, se invoca el constructor predeterminado de un tipo de valor mediante el `new` operador. Por motivos de eficiencia, este requisito no pretende lograr que la implementación para generar una llamada al constructor. En el ejemplo siguiente, las variables `i` y `j` se inicializan en cero.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Dado que cada tipo de valor tiene implícitamente un constructor de instancia sin parámetros público, no es posible para un tipo struct para que contenga una declaración explícita de un constructor sin parámetros. No obstante, un tipo de estructura se puede declarar constructores de instancias con parámetros ([constructores](structs.md#constructors)).

### <a name="struct-types"></a>Tipos de estructura

Un tipo struct es un tipo de valor que se puede declarar constantes, campos, métodos, propiedades, indizadores, operadores, constructores de instancias, los constructores estáticos y tipos anidados. La declaración de tipos de struct se describe en [declaraciones de estructura](structs.md#struct-declarations).

### <a name="simple-types"></a>Tipos simples

C# proporciona un conjunto de tipos de struct predefinidos denominados el ***tipos simples***. Los tipos simples se identifican mediante palabras reservadas, pero estas palabras reservadas son simplemente los alias predefinidos para tipos de estructura en el `System` espacio de nombres, como se describe en la tabla siguiente.


| __Palabra reservada__ | __Tipo de alias__ |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

Dado que un tipo simple equivale a un tipo de estructura, cada tipo simple tiene miembros. Por ejemplo, `int` tiene los miembros declarados en `System.Int32` y los miembros heredados de `System.Object`, y se permiten las instrucciones siguientes:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Los tipos simples se diferencian de otros tipos de struct en que permiten determinadas operaciones adicionales:

*  La mayoría de los tipos simples permiten crear escribiendo valores *literales* ([literales](lexical-structure.md#literals)). Por ejemplo, `123` es un literal de tipo `int` y `'a'` es un literal de tipo `char`. C# no hace ninguna disposición para los literales de tipos struct en general y los valores no predeterminados de otros tipos de struct en última instancia, siempre se crean a través de constructores de instancias de esos tipos de struct.
*  Cuando los operandos de expresión son todos constantes de tipo simple, es posible que el compilador evaluar la expresión en tiempo de compilación. Se conoce como una expresión como un *constant_expression* ([expresiones constantes](expressions.md#constant-expressions)). Las expresiones que implican los operadores definidos por otros tipos struct no se consideran expresiones constantes.
*  A través de `const` declaraciones es posible declarar constantes de los tipos simples ([constantes](classes.md#constants)). No es posible tener constantes de otros tipos struct, pero se proporciona un efecto similar `static readonly` campos.
*  Las conversiones que involucran tipos simples pueden participar en la evaluación de operadores de conversión definidos por otros tipos de struct, pero un operador de conversión definido por el usuario no puede participar en la evaluación de otro operador definido por el usuario ([evaluación de conversiones definidas por el usuario](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Tipos enteros

C# admite nueve tipos integrales: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, y `char`. Los tipos enteros tienen los siguientes tamaños e intervalos de valores:

*  El `sbyte` firmado de tipo representa enteros de 8 bits con valores comprendidos entre -128 y 127.
*  El `byte` tipo representa los enteros de 8 bits sin signo con valores comprendidos entre 0 y 255.
*  El `short` firmado de tipo representa enteros de 16 bits con valores comprendidos entre -32768 y 32767.
*  El `ushort` tipo representa los enteros de 16 bits sin signo con valores comprendidos entre 0 y 65535.
*  El `int` firmado de tipo representa enteros de 32 bits con valores comprendidos entre -2147483648 y 2147483647.
*  El `uint` tipo representa los enteros de 32 bits sin signo con valores comprendidos entre 0 y 4294967295.
*  El `long` firmado de tipo representa enteros de 64 bits con valores comprendidos entre -9223372036854775808 y 9223372036854775807.
*  El `ulong` tipo representa los enteros de 64 bits sin signo con valores comprendidos entre 0 y 18446744073709551615.
*  El `char` tipo representa los enteros de 16 bits sin signo con valores comprendidos entre 0 y 65535. El conjunto de valores posibles para el `char` tipo corresponde al juego de caracteres Unicode. Aunque `char` tiene la misma representación `ushort`, no todas las operaciones permitidas en un tipo están permitidas en el otro.

El tipo entero operadores unarios y binarios siempre funcionan con precisión de 32 bits con signo, precisión de 32 bits sin signo, precisión de 64 bits con signo o sin signo de 64 bits de precisión:

*  Para el operador unario `+` y `~` operadores, el operando se convierte al tipo `T`, donde `T` es el primero de `int`, `uint`, `long`, y `ulong` que puede representar por completo todo valores posibles del operando. La operación, a continuación, se realiza con la precisión del tipo `T`, y el tipo del resultado es `T`.
*  Para el operador unario `-` operador, el operando se convierte al tipo `T`, donde `T` es el primero de `int` y `long` que puede representar completamente todos los valores posibles del operando. La operación, a continuación, se realiza con la precisión del tipo `T`, y el tipo del resultado es `T`. El operador unario `-` operador no se puede aplicar a operandos de tipo `ulong`.
*  Para el archivo binario `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, y `<=` operadores, los operandos se convierten al tipo `T`, donde `T` es el primero de `int`, `uint`, `long`, y `ulong` que puede representar completamente todos los posibles valores de ambos operandos. La operación, a continuación, se realiza con la precisión del tipo `T`, y el tipo del resultado es `T` (o `bool` para los operadores relacionales). No se permite para que un operando de tipo `long` y la otra para ser de tipo `ulong` con los operadores binarios.
*  Para el archivo binario `<<` y `>>` operadores, el operando izquierdo se convierte al tipo `T`, donde `T` es el primero de `int`, `uint`, `long`, y `ulong` que puede representar por completo todo valores posibles del operando. La operación, a continuación, se realiza con la precisión del tipo `T`, y el tipo del resultado es `T`.

El `char` tipo se clasifica como un tipo entero, pero difiere de otros tipos integrales de dos maneras:

*  No hay ninguna conversión implícita de otros tipos a los `char` tipo. En concreto, aunque el `sbyte`, `byte`, y `ushort` tipos tienen intervalos de valores que son totalmente que se puede representar utilizando el `char` escriba, las conversiones implícitas de `sbyte`, `byte`, o `ushort` a `char` no existen.
*  Constantes de la `char` tipo debe escribirse como *character_literal*s o como *integer_literal*s en combinación con una conversión al tipo `char`. Por ejemplo, `(char)10` es lo mismo que `'\x000A'`.

El `checked` y `unchecked` operadores e instrucciones sirven para controlar la comprobación de desbordamiento para conversiones y operaciones aritméticas de tipo entero ([los operadores checked y unchecked](expressions.md#the-checked-and-unchecked-operators)). En un `checked` contexto, un desbordamiento produce un error en tiempo de compilación o provoca un `System.OverflowException` que se produzca. En un `unchecked` contexto, los desbordamientos se omiten y se descartan los bits de orden superior que no caben en el tipo de destino.

### <a name="floating-point-types"></a>Tipos de punto flotante

C# admite dos tipos de punto flotante: `float` y `double`. El `float` y `double` tipos se representan mediante los formatos IEEE 754 de 32 bits de precisión sencilla y de 64 bits de precisión doble, que proporcionan los siguientes conjuntos de valores:

*  Positivo cero y cero negativo. En la mayoría de las situaciones, cero positivo y negativo cero se comportan exactamente igual como del valor simple cero, pero algunas operaciones distinguen entre los dos ([operador de división](expressions.md#division-operator)).
*  Infinito positivo e infinito negativo. Infinitos son generados por operaciones tales como dividir un número distinto de cero por cero. Por ejemplo, `1.0 / 0.0` da como resultado infinito positivo, y `-1.0 / 0.0` es infinito negativo.
*  El ***Not a Number*** valor, a menudo abreviado como NaN. NaN producen las operaciones de punto flotante no válidas, como dividir cero por cero.
*  El conjunto finito de valores distintos de cero del formulario `s * m * 2^e`, donde `s` es 1 o -1, y `m` y `e` están determinados por el tipo de punto flotante concreto: para `float`, `0 < m < 2^24` y `-149 <= e <= 104`y para `double`, `0 < m < 2^53` y `1075 <= e <= 970`. Números de punto flotante desnormalizados se consideran válidos valores distintos de cero.

El `float` tipo puede representar los valores comprendidos entre aproximadamente `1.5 * 10^-45` a `3.4 * 10^38` con una precisión de 7 dígitos.

El `double` tipo puede representar los valores comprendidos entre aproximadamente `5.0 * 10^-324` a `1.7 × 10^308` con una precisión de 15-16 dígitos.

Si uno de los operandos de un operador binario es de un tipo de punto flotante, a continuación, el otro operando debe ser de un tipo entero o un tipo de punto flotante, y la operación se evalúa como sigue:

*  Si uno de los operandos es de tipo entero, ese operando se convierte al tipo de punto flotante del otro operando.
*  A continuación, si cualquiera de los operandos es de tipo `double`, el otro operando se convierte en `double`, la operación se realiza con al menos `double` intervalo y precisión y el tipo del resultado es `double` (o `bool` para el operadores relacionales).
*  En caso contrario, la operación se realiza con al menos `float` intervalo y precisión y el tipo del resultado es `float` (o `bool` para los operadores relacionales).

Los operadores de punto flotante, incluidos los operadores de asignación, nunca producen excepciones. En su lugar, en situaciones excepcionales, las operaciones de punto flotante producen cero, infinito o NaN, como se describe a continuación:

*  Si el resultado de una operación de punto flotante es demasiado pequeño para el formato de destino, el resultado de la operación se convierte en cero positivo o cero negativo.
*  Si el resultado de una operación de punto flotante es demasiado grande para el formato de destino, el resultado de la operación se convierte en infinito positivo o infinito negativo.
*  Si una operación de punto flotante no es válida, el resultado de la operación es NaN.
*  Si uno o ambos operandos de una operación de punto flotante es NaN, el resultado de la operación es NaN.

Operaciones de punto flotante se pueden realizar con mayor precisión que el tipo de resultado de la operación. Por ejemplo, algunas arquitecturas de hardware compatible con un tipo de punto flotante "extended" o "long double" con mayor intervalo y precisión que el `double` escriba e implícita realizar todas las operaciones de punto flotante mediante este tipo de mayor precisión. Solo con un costo excesivo en el rendimiento de dichas arquitecturas de hardware se pueden realizar para realizar operaciones de punto flotante con menos precisión, y en lugar de requerir una implementación pierda el rendimiento y precisión, C# permite un tipo de precisión mayor que se utiliza para todas las operaciones de punto flotante. Aparte de proporcionar resultados más precisos, esto rara vez tiene efectos medibles. Sin embargo, en expresiones de la forma `x * y / z`, donde la multiplicación produce un resultado que está fuera del `double` intervalo, pero la división posterior devuelve el resultado temporal en el `double` de intervalo, el hecho de que la expresión es evalúa en un intervalo mayor formato puede producir un resultado finito en lugar de infinito.

### <a name="the-decimal-type"></a>El tipo decimal

El tipo `decimal` es un tipo de datos de 128 bits adecuado para cálculos financieros y monetarios. El `decimal` tipo puede representar los valores comprendidos entre `1.0 * 10^-28` a aproximadamente `7.9 * 10^28` con 28-29 dígitos significativos.

El conjunto finito de valores de tipo `decimal` tienen el formato `(-1)^s * c * 10^-e`, donde el signo `s` es 0 o 1, el coeficiente `c` viene dado por `0 <= *c* < 2^96`y la escala `e` es tal que `0 <= e <= 28`. El `decimal` tipo no admite ceros con signo, infinitos o NaN. Un `decimal` se representa como un entero de 96 bits multiplicado por una potencia de diez. Para `decimal`s con un valor absoluto menor `1.0m`, el valor es exacta a la posición decimal 28, pero no más. Para `decimal`s con un valor absoluto mayor que o igual que `1.0m`, el valor es exacto a 28 o 29 dígitos. Contrariamente a la `float` y `double` tipos de datos, se pueden representar números fraccionarios decimales como 0,1 exactamente en el `decimal` representación. En el `float` y `double` representaciones, estos números son a menudo fracciones infinitas, lo que las hace más propensos a redondear errores.

Si uno de los operandos de un operador binario es de tipo `decimal`, a continuación, el otro operando debe ser de tipo entero o de tipo `decimal`. Si hay un operando de tipo entero, se convierte en `decimal` antes de realizar la operación.

El resultado de una operación en los valores de tipo `decimal` es que resultaría de calcular un resultado exacto (conservación escala, según se define para cada operador) y, a continuación, redondear para ajustarse a la representación. Los resultados se redondean hasta el valor representable más próximo y, cuando un resultado está equidistante a dos valores que se puede representar, en el valor que tiene un número par en la posición de dígitos menos significativa (Esto se conoce como "redondeo bancario"). Un resultado cero siempre tiene un inicio de sesión 0 y una escala de 0.

Si una operación aritmética decimal genera un valor menor o igual que `5 * 10^-29` en valor absoluto, el resultado de la operación se convierte en cero. Si un `decimal` operación aritmética genera un resultado que es demasiado grande para el `decimal` formato, un `System.OverflowException` se produce.

El `decimal` tipo tiene una precisión mayor intervalo pero menor que los tipos de punto flotante. Por lo tanto, las conversiones de tipos de punto flotante a `decimal` podría generar excepciones de desbordamiento y conversiones de `decimal` a los tipos de punto flotante podría provocar la pérdida de precisión. Por estas razones, no existe ninguna conversión implícita entre los tipos de punto flotante y `decimal`, y sin conversiones explícitas, no es posible mezclar de punto flotante y `decimal` operandos en la misma expresión.

### <a name="the-bool-type"></a>El tipo bool

El `bool` tipo representa cantidades lógicas booleano. Los valores posibles del tipo `bool` son `true` y `false`.

No existe ninguna conversión estándar entre `bool` y otros tipos. En concreto, el `bool` tipo es distinto e independiente de los tipos enteros y un `bool` valor no se puede usar en lugar de un valor entero y viceversa.

En los lenguajes C y C++, se puede convertir un valor de cero integral o de punto flotante o un puntero nulo en el valor booleano `false`, y un valor de punto flotante o entero distinto de cero o un puntero null no se puede convertir en el valor booleano `true`. En C#, estas conversiones se realizan mediante la comparación explícita de un valor integral o de punto flotante con cero o comparando explícitamente una referencia de objeto `null`.

### <a name="enumeration-types"></a>Tipos de enumeración

Un tipo de enumeración es un tipo distinto con constantes con nombre. Cada tipo de enumeración tiene un tipo subyacente, que debe ser `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` o `ulong`. El conjunto de valores del tipo de enumeración es el mismo que el conjunto de valores del tipo subyacente. Valores del tipo de enumeración no se limitan a los valores de las constantes con nombre. Tipos de enumeración se definen mediante declaraciones de enumeración ([las declaraciones de enumeración](enums.md#enum-declarations)).

### <a name="nullable-types"></a>Tipos que aceptan valores NULL

Un tipo que acepta valores NULL puede representar todos los valores de sus ***tipo subyacente*** además un valor de null adicional. Un tipo que acepta valores NULL se escribe `T?`, donde `T` es el tipo subyacente. Esta sintaxis es una abreviatura de `System.Nullable<T>`, y las dos formas se pueden usar indistintamente.

Un ***tipo de valor distinto de NULL*** a la inversa es cualquier tipo de valor que no sean `System.Nullable<T>` y su forma abreviada `T?` (para cualquier `T`), además de cualquier parámetro de tipo está restringido a ser un tipo de valor distinto de null (es decir, cualquier parámetro de tipo con un `struct` restricción). El `System.Nullable<T>` tipo especifica la restricción de tipo de valor para `T` ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), lo que significa que el tipo subyacente de un tipo que acepta valores NULL puede ser cualquier tipo de valor distinto de NULL. El tipo subyacente de un tipo que acepta valores NULL no puede ser un tipo que acepta valores NULL o un tipo de referencia. Por ejemplo, `int??` y `string?` son tipos válidos.

Una instancia de un tipo que acepta valores NULL `T?` tiene dos propiedades públicas de solo lectura:

*  Un `HasValue` propiedad de tipo `bool`
*  Un `Value` propiedad de tipo `T`

Una instancia para el que `HasValue` es true se dice que es distinto de null. Una instancia de no null contiene un valor conocido y `Value` devuelve ese valor.

Una instancia para el que `HasValue` es false se dice que es null. Una instancia null tiene un valor indefinido. Al intentar leer el `Value` de una instancia null hace que un `System.InvalidOperationException` que se produzca. El proceso de obtener acceso a la `Value` propiedad de una instancia que acepta valores NULL se conoce como ***desencapsulado***.

El constructor predeterminado, todos los tipos que aceptan valores NULL además de `T?` tiene un constructor público que toma un único argumento de tipo `T`. Dado un valor `x` de tipo `T`, una invocación del constructor del formulario

```csharp
new T?(x)
```
crea una instancia de no null de `T?` para que el `Value` propiedad es `x`. El proceso de creación de una instancia de no null de un tipo que acepta valores NULL para un valor determinado se denomina ***ajuste***.

Las conversiones implícitas están disponibles en el `null` literal `T?` ([Null literales conversiones](conversions.md#null-literal-conversions)) y de `T` a `T?` ([conversiones implícitas que acepta valores NULL](conversions.md#implicit-nullable-conversions)).

## <a name="reference-types"></a>Tipos de referencia

Un tipo de referencia es un tipo de clase, un tipo de interfaz, un tipo de matriz o un tipo de delegado.

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;

delegate_type
    : type_name
    ;
```

Un valor de tipo de referencia es una referencia a un ***instancia*** del tipo, que se conoce como un ***objeto***. El valor especial `null` es compatible con todos los tipos de referencia e indica la ausencia de una instancia.

### <a name="class-types"></a>Tipos de clase

Un tipo de clase define una estructura de datos que contiene los miembros de datos (constantes y campos), miembros de función (métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos) y tipos anidados. Tipos de clase admiten la herencia, un mecanismo mediante el cual las clases derivadas pueden extender y especializar clases base. Se crean instancias de tipos de clase mediante *object_creation_expression*s ([expresiones de creación de objeto](expressions.md#object-creation-expressions)).

Se describen los tipos de clase en [clases](classes.md).

Ciertos tipos de clases predefinidos tienen un significado especial en el lenguaje C#, como se describe en la tabla siguiente.


| __Tipo de clase__     | __Descripción__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | La clase base fundamental de todos los demás tipos. Consulte [el tipo de objeto](types.md#the-object-type). | 
| `System.String`    | El tipo de cadena del lenguaje C#. Consulte [el tipo de cadena](types.md#the-string-type).         |
| `System.ValueType` | La clase base de todos los tipos de valor. Consulte [System.ValueType el tipo](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | La clase base de todos los tipos de enumeración. Consulte [enumeraciones](enums.md).              |
| `System.Array`     | La clase base de todos los tipos de matriz. Vea [Matrices](arrays.md).             |
| `System.Delegate`  | La clase base de todos los tipos de delegado. Consulte [delegados](delegates.md).          |
| `System.Exception` | La clase base de todos los tipos de excepción. Consulte [excepciones](exceptions.md).         |

### <a name="the-object-type"></a>El tipo de objeto

El `object` tipo de clase es la clase base fundamental de todos los demás tipos. Todos los tipos en C# directa o indirectamente se derivan de la `object` tipo de clase.

La palabra clave `object` es simplemente un alias de la clase predefinida `System.Object`.

### <a name="the-dynamic-type"></a>El tipo dinámico

El `dynamic` escriba, como `object`, puede hacer referencia a cualquier objeto. Cuando se aplican los operadores en expresiones de tipo `dynamic`, su resolución se aplaza hasta que se ejecuta el programa. Por lo tanto, si el operador legalmente no se puede aplicar al objeto que se hace referencia, no se proporciona ningún error durante la compilación. En su lugar, se producirá una excepción cuando se produce un error en la resolución del operador en tiempo de ejecución.

Su propósito es permitir que el enlace dinámico, que se describe en detalle en [enlace dinámico](expressions.md#dynamic-binding).

`dynamic` se considera idéntico al `object` excepto en los siguientes aspectos:

*  Operaciones en las expresiones de tipo `dynamic` puede enlazarse dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).
*  Inferencia de tipos ([inferencia](expressions.md#type-inference)) preferirán `dynamic` sobre `object` si ambos son candidatos.

Debido a esta equivalencia, contiene lo siguiente:

*  Hay una conversión implícita de identidad entre `object` y `dynamic`y entre tipos construidos que son iguales cuando se reemplaza `dynamic` con `object`
*  Conversiones implícitas y explícitas a y desde `object` también se aplican a y desde `dynamic`.
*  Las firmas de método que son iguales cuando se reemplaza `dynamic` con `object` se consideran la misma firma
*  El tipo `dynamic` es indistinguible de `object` en tiempo de ejecución.
*  Una expresión del tipo `dynamic` se conoce como un ***expresión dinámica***.

### <a name="the-string-type"></a>El tipo de cadena

El `string` tipo es un tipo de clase sealed que hereda directamente de `object`. Las instancias de la `string` clase representar cadenas de caracteres Unicode.

Los valores de la `string` tipo puede escribirse como literales de cadena ([literales de cadena](lexical-structure.md#string-literals)).

La palabra clave `string` es simplemente un alias de la clase predefinida `System.String`.

### <a name="interface-types"></a>Tipos de interfaz

Una interfaz define un contrato. Una clase o estructura que implementa una interfaz debe adherirse a su contrato. Una interfaz puede heredar de varias interfaces base, y una clase o struct puede implementar varias interfaces.

Se describen los tipos de interfaz en [Interfaces](interfaces.md).

### <a name="array-types"></a>Tipos de matriz

Una matriz es una estructura de datos que contiene cero o más variables que se accede mediante índices calculados. Las variables contenidas en una matriz, también conocida como elementos de la matriz, son todas del mismo tipo, y este tipo se denomina tipo de elemento de la matriz.

Se describen los tipos de matriz en [matrices](arrays.md).

### <a name="delegate-types"></a>Tipos delegados

Un delegado es una estructura de datos que hace referencia a uno o varios métodos. Por ejemplo los métodos, también hace referencia a sus instancias de objeto correspondiente.

El equivalente más cercano de un delegado en C o C++ es un puntero de función, pero mientras que un puntero de función solo puede hacer referencia a funciones estáticas, un delegado puede hacer referencia a estáticos y métodos de instancia. En el último caso, el delegado almacena una referencia al punto de entrada del método pero una referencia a la instancia del objeto en el que se va a invocar el método.

Se describen los tipos de delegado en [delegados](delegates.md).

## <a name="boxing-and-unboxing"></a>Conversión boxing y conversión unboxing

El concepto de conversión boxing y unboxing es fundamental para el sistema de tipos de C#. Proporciona un puente entre *value_type*s y *reference_type*s permitiendo cualquier valor de un *value_type* va a convertir a y desde tipo `object`. Conversión boxing y unboxing permite una vista unificada del sistema de tipos en la que un valor de cualquier tipo en última instancia, puede tratarse como un objeto.

### <a name="boxing-conversions"></a>Conversiones boxing

Una conversión boxing permite un *value_type* convertirse implícitamente a un *reference_type*. Existen las siguientes conversiones boxing:

*  Desde cualquier *value_type* al tipo `object`.
*  Desde cualquier *value_type* al tipo `System.ValueType`.
*  Desde cualquier *non_nullable_value_type* a cualquier *interface_type* implementado por el *value_type*.
*  Desde cualquier *nullable_type* a cualquier *interface_type* implementadas por el tipo subyacente de la *nullable_type*.
*  Desde cualquier *enum_type* al tipo `System.Enum`.
*  Desde cualquier *nullable_type* con una subyacente *enum_type* al tipo `System.Enum`.
*  Tenga en cuenta que se ejecutará una conversión implícita de un parámetro de tipo como una conversión boxing si en tiempo de ejecución termina convertir de un tipo de valor a un tipo de referencia ([conversiones implícitas que implica parámetros de tipo](conversions.md#implicit-conversions-involving-type-parameters)).

Conversión boxing de un valor de un *non_nullable_value_type* consiste en asignar una instancia del objeto y copiar el *non_nullable_value_type* valor en esa instancia.

Conversión boxing de un valor de un *nullable_type* genera una referencia nula si es el `null` valor (`HasValue` es `false`), o el resultado de acción de desencapsular y conversión boxing de lo contrario, el valor subyacente.

El proceso real de conversión boxing de un valor de un *non_nullable_value_type* se explica mejor por imaginar la existencia de un tipo genérico ***clase boxing***, que se comporta como si se declara como sigue:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

Conversión boxing de un valor `v` typu `T` ahora consiste en ejecutar la expresión `new Box<T>(v)`y devolver la instancia resultante como un valor de tipo `object`. Por lo tanto, las instrucciones
```csharp
int i = 123;
object box = i;
```
Conceptualmente se corresponden con
```csharp
int i = 123;
object box = new Box<int>(i);
```

Una clase de conversión boxing como `Box<T>` encima realmente no existe y el tipo dinámico de un valor con conversión boxing no es realmente un tipo de clase. En su lugar, un valor con conversión boxing del tipo `T` tiene el tipo dinámico `T`y una comprobación de tipo dinámico usando el `is` operador simplemente puede hacer referencia a tipo `T`. Por ejemplo,
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
dará como resultado la cadena "`Box contains an int`" en la consola.

Una conversión boxing implica realizar una copia del valor que se va a aplicar la conversión boxing. Esto es diferente de la conversión de un *reference_type* escriba `object`, en el que el valor continúa hacer referencia a la misma instancia y sencillamente se considera como el tipo menos derivado `object`. Por ejemplo, dada la declaración
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
las siguientes instrucciones
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
dará como resultado el valor 10 en la consola porque la operación de conversión boxing implícita que se produce en la asignación de `p` a `box` hace que el valor de `p` va a copiar. Tenía `Point` sido declarado un `class` en su lugar, sería el valor de 20 salida porque `p` y `box` haría referencia a la misma instancia.

### <a name="unboxing-conversions"></a>Conversiones unboxing

Una conversión unboxing permite una *reference_type* convertirse explícitamente a un *value_type*. Existen las siguientes conversiones unboxing:

*  Desde el tipo `object` a cualquier *value_type*.
*  Desde el tipo `System.ValueType` a cualquier *value_type*.
*  Desde cualquier *interface_type* a cualquier *non_nullable_value_type* que implementa el *interface_type*.
*  Desde cualquier *interface_type* a cualquier *nullable_type* cuyo tipo subyacente implementa el *interface_type*.
*  Desde el tipo `System.Enum` a cualquier *enum_type*.
*  Desde el tipo `System.Enum` a cualquier *nullable_type* con una subyacente *enum_type*.
*  Tenga en cuenta que una conversión explícita a un parámetro de tipo se ejecuta como una conversión unboxing si en tiempo de ejecución termina convertir de un tipo de referencia a un tipo de valor ([las conversiones explícitas de dinámicas](conversions.md#explicit-dynamic-conversions)).

Una operación de conversión unboxing a un *non_nullable_value_type* consta de comprobar primero que la instancia de objeto es un valor con conversión boxing de la dada *non_nullable_value_type*y, a continuación, copia el valor de la instancia de.

Conversión unboxing a un *nullable_type* genera el valor null de la *nullable_type* si el operando de origen es `null`, o el resultado de la conversión unboxing al tipo subyacente de la instancia del objeto ajustado el *nullable_type* en caso contrario.

Hacer referencia a la clase de conversión boxing imaginaria descrita en la sección anterior, una conversión unboxing de un objeto `box` a un *value_type* `T` consiste en ejecutar la expresión `((Box<T>)box).value`. Por lo tanto, las instrucciones
```csharp
object box = 123;
int i = (int)box;
```
Conceptualmente se corresponden con
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Para una conversión unboxing a un determinado *non_nullable_value_type* para tener éxito en tiempo de ejecución, el valor del operando de origen debe ser una referencia a un valor con conversión boxing de los que *non_nullable_value_type*. Si el operando de origen es `null`, un `System.NullReferenceException` se produce. Si el operando de origen es una referencia a un objeto incompatible, un `System.InvalidCastException` se produce.

Para una conversión unboxing a un determinado *nullable_type* para tener éxito en tiempo de ejecución, el valor del operando de origen debe ser `null` o una referencia a un valor con conversión boxing de subyacente *non_nullable_value_type* de la *nullable_type*. Si el operando de origen es una referencia a un objeto incompatible, un `System.InvalidCastException` se produce.

## <a name="constructed-types"></a>Tipos construidos

Una declaración de tipo genérico, por sí mismo, denota un ***sin enlazar el tipo genérico*** que se usa como un "plano" para formar muchos tipos diferentes, por medio de aplicar ***argumentos de tipo***. Los argumentos de tipo se escriben entre corchetes angulares (`<` y `>`) inmediatamente después del nombre del tipo genérico. Se llama a un tipo que incluye al menos un argumento de tipo un ***tipo construido***. Un tipo construido puede usarse en la mayoría de los lugares en el idioma en el que puede aparecer un nombre de tipo. Solo puede usarse dentro de un tipo genérico sin enlazar un *typeof_expression* ([el operador typeof](expressions.md#the-typeof-operator)).

Tipos construidos también pueden usarse en expresiones como nombres simples ([nombres simples](expressions.md#simple-names)) o al obtener acceso a un miembro ([acceso a miembros](expressions.md#member-access)).

Cuando un *namespace_or_type_name* es evaluados, solo los tipos genéricos con el número correcto de se consideran parámetros de tipo. Por lo tanto, es posible utilizar el mismo identificador para identificar los diferentes tipos, siempre y cuando los tipos tienen un número diferente de parámetros de tipo. Esto resulta útil cuando se mezclan las clases no genéricas y en el mismo programa:

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

Un *type_name* puede identificar un tipo construido, incluso si no especifica los parámetros de tipo directamente. Esto puede ocurrir que un tipo está anidado dentro de una declaración de clase genérica y se utiliza implícitamente el tipo de instancia de la declaración del contenedor para la búsqueda de nombres ([tipos en clases genéricas anidados](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

En el código no seguro, no se puede usar un tipo construido como un *unmanaged_type* ([tipos de puntero](unsafe-code.md#pointer-types)).

### <a name="type-arguments"></a>Argumentos de tipo

Cada argumento de una lista de argumentos de tipo es simplemente un *tipo*.

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

En código no seguro ([código no seguro](unsafe-code.md)), un *type_argument* no puede ser un tipo de puntero. Cada argumento de tipo debe cumplir las restricciones en el correspondiente parámetro de tipo ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).

### <a name="open-and-closed-types"></a>Tipos abiertos y cerrados

Todos los tipos pueden estar clasificados como ***tipos abiertos*** o ***tipos cerrados***. Un tipo abierto es un tipo que implica parámetros de tipo. Más específicamente:

*  Un parámetro de tipo define un tipo abierto.
*  Un tipo de matriz es un tipo abierto si y solo si el tipo de elemento es un tipo abierto.
*  Un tipo construido es un tipo abierto si y solo si uno o varios de sus argumentos de tipo son un tipo abierto. Un tipo construido anidado es un tipo abierto si y solo si uno o varios de sus argumentos de tipo o los argumentos de tipo de sus tipos de contenedor son un tipo abierto.

Un tipo cerrado es un tipo que no es un tipo abierto.

En tiempo de ejecución, todo el código dentro de una declaración de tipo genérico se ejecuta en el contexto de un tipo construido cerrado que se creó mediante la aplicación de argumentos de tipo para la declaración genérica. Cada parámetro de tipo dentro del tipo genérico se enlaza a un tipo determinado de tiempo de ejecución. El procesamiento en tiempo de ejecución de todas las instrucciones y expresiones siempre se produce con tipos cerrados y tipos abiertos solo se producen durante el tiempo de compilación al procesamiento.

Cada tipo construido cerrado tiene su propio conjunto de variables estáticas, que no se comparten con otros tipos construidos cerrados. Puesto que no existe un tipo abierto en tiempo de ejecución, no hay ninguna variable estática asociada con un tipo abierto. Dos tipos construidos cerrados son del mismo tipo si están construidos desde el mismo tipo genérico sin enlazar, y sus argumentos de tipo correspondientes son del mismo tipo.

### <a name="bound-and-unbound-types"></a>Tipos dependientes e independientes

El término ***desenlazada tipo*** hace referencia a un tipo no genérico o un tipo genérico sin enlazar. El término ***enlazado tipo*** hace referencia a un tipo no genérico o un tipo construido.

Un tipo sin enlazar se refiere a la entidad declarada por una declaración de tipos. Un tipo genérico sin enlazar no es un tipo y no se puede usar como el tipo de una variable, argumento o valor devuelto o como un tipo base. Es la única construcción en el que se puede hacer referencia a un tipo genérico sin enlazar la `typeof` expresión ([el operador typeof](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Restricciones satisfactorios

Cada vez que se hace referencia a un tipo construido o método genérico, se comprueban los argumentos de tipo proporcionado con las restricciones de parámetro de tipo declaradas en el tipo o método genérico ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)). Para cada `where` cláusula, el argumento de tipo `A` que corresponde a la instancia con nombre parámetro de tipo se compara con cada restricción como sigue:

*  Si la restricción es un tipo de clase, un tipo de interfaz o un parámetro de tipo, permiten `C` representan que restricción con los argumentos de tipo suministrado se sustituye por los parámetros de tipo que aparecen en la restricción. Para satisfacer la restricción, debe ser el caso de que escriba `A` es convertible al tipo `C` por uno de los siguientes:
    * Una conversión de identidad ([conversión de identidad](conversions.md#identity-conversion))
    * Una conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions))
    * Una conversión boxing ([conversiones Boxing](conversions.md#boxing-conversions)), siempre que sea de tipo A un tipo de valor distinto de NULL.
    * Una conversión implícita de parámetro de referencia, boxing o tipo de un parámetro de tipo `A` a `C`.
*  Si la restricción es la restricción de tipo de referencia (`class`), el tipo `A` debe cumplir una de las siguientes acciones:
    * `A` es un tipo de interfaz, tipo de clase, tipo de delegado o tipo de matriz. Tenga en cuenta que `System.ValueType` y `System.Enum` son tipos de referencia que se aplica esta restricción.
    * `A` es un parámetro de tipo que se sabe que es un tipo de referencia ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).
*  Si la restricción es la restricción de tipo de valor (`struct`), el tipo `A` debe cumplir una de las siguientes acciones:
    * `A` es un tipo de estructura o tipo de enumeración, pero no es un tipo que acepta valores NULL. Tenga en cuenta que `System.ValueType` y `System.Enum` son tipos de referencia que no cumplen con esta restricción.
    * `A` es un parámetro de tipo que tiene la restricción de tipo de valor ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).
*  Si la restricción es la restricción de constructor `new()`, el tipo `A` no debe ser `abstract` y debe tener un constructor público sin parámetros. Esto se cumple si se cumple una de las siguientes acciones:
    * `A` es un tipo de valor, ya que todos los tipos de valor tienen un constructor predeterminado público ([constructores predeterminados](types.md#default-constructors)).
    * `A` es un parámetro de tipo que tiene la restricción de constructor ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).
    * `A` es un parámetro de tipo que tiene la restricción de tipo de valor ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).
    * `A` es una clase que no es `abstract` y contiene declarado explícitamente `public` constructor sin parámetros.
    * `A` no es `abstract` y tiene un constructor predeterminado ([constructores predeterminados](classes.md#default-constructors)).

Se produce un error de tiempo de compilación si no se cumplen una o varias de las restricciones de un parámetro de tipo por los argumentos de tipo especificado.

Puesto que no se heredan los parámetros de tipo, las restricciones no son nunca heredan. En el ejemplo siguiente, `D` debe especificar la restricción en su parámetro de tipo `T` poder `T` satisface las restricciones impuestas por la clase base `B<T>`. En cambio, la clase `E` no necesitan especificar una restricción, ya que `List<T>` implementa `IEnumerable` para cualquier `T`.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Parámetros de tipo

Un parámetro de tipo es un identificador que designa un tipo de valor o tipo de referencia que está enlazado el parámetro en tiempo de ejecución.

```antlr
type_parameter
    : identifier
    ;
```

Puesto que un parámetro de tipo se puede crear instancias con muchos argumentos de tipo real diferente, los parámetros de tipo tienen ligeramente diferentes operaciones y restricciones que otros tipos. Se incluyen los siguientes:

*  Un parámetro de tipo no se puede usar directamente para declarar una clase base ([clase Base](classes.md#base-class)) o interfaz ([listas de parámetros de tipo variante](interfaces.md#variant-type-parameter-lists)).
*  Las reglas para la búsqueda de miembros en el tipo de parámetros dependen de las restricciones, si existe, se aplican al parámetro de tipo. Se detallan en [búsqueda de miembros](expressions.md#member-lookup).
*  Las conversiones disponibles para un parámetro de tipo dependen de las restricciones, si hay alguno, se aplican al parámetro de tipo. Se detallan en [conversiones implícitas que implica parámetros de tipo](conversions.md#implicit-conversions-involving-type-parameters) y [las conversiones explícitas de dinámicas](conversions.md#explicit-dynamic-conversions).
*  El literal `null` no se puede convertir a un tipo dado por un parámetro de tipo, excepto si el parámetro de tipo se conoce como un tipo de referencia ([conversiones implícitas que implica parámetros de tipo](conversions.md#implicit-conversions-involving-type-parameters)). Sin embargo, un `default` expresión ([expresiones de valor predeterminado](expressions.md#default-value-expressions)) se puede usar en su lugar. Además, se puede comparar un valor con un tipo especificado por un parámetro de tipo con `null` mediante `==` y `!=` ([operadores de igualdad de referencia tipo](expressions.md#reference-type-equality-operators)) a menos que el parámetro de tipo tiene la restricción de tipo de valor.
*  Un `new` expresión ([expresiones de creación de objetos](expressions.md#object-creation-expressions)) sólo puede utilizarse con un parámetro de tipo si el parámetro de tipo está restringido por una *constructor_constraint* o el valor de tipo de restricción ([ Restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).
*  Un parámetro de tipo no se puede usar en cualquier lugar dentro de un atributo.
*  No se puede usar un parámetro de tipo en un acceso a miembros ([acceso a miembros](expressions.md#member-access)) o nombre de tipo ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)) para identificar un miembro estático o un tipo anidado.
*  En el código no seguro, no se puede usar un parámetro de tipo como un *unmanaged_type* ([tipos de puntero](unsafe-code.md#pointer-types)).

Como un tipo, parámetros de tipo son puramente una construcción de tiempo de compilación. En tiempo de ejecución, cada parámetro de tipo está enlazado a un tipo de tiempo de ejecución que se especifica al proporcionar un argumento de tipo a la declaración de tipo genérico. Por lo tanto, el tipo de una variable declarada con un testamento de parámetro de tipo en tiempo de ejecución, ser un tipo construido cerrado ([abierto y cerrado tipos](types.md#open-and-closed-types)). El tiempo de ejecución de todas las instrucciones y expresiones que implican los parámetros de tipo utiliza el tipo real que se proporcionó como el argumento de tipo para ese parámetro.

## <a name="expression-tree-types"></a>Tipos de árbol de expresión

***Árboles de expresión*** permiten las expresiones lambda para representarse como estructuras de datos en lugar de código ejecutable. Árboles de expresiones son valores de ***tipos de árbol de expresión*** del formulario `System.Linq.Expressions.Expression<D>`, donde `D` es cualquier tipo de delegado. Para el resto de esta especificación se hará referencia a estos tipos utilizando la forma abreviada `Expression<D>`.

Si existe una conversión de una expresión lambda a un tipo delegado `D`, también existe una conversión al tipo de árbol de expresión `Expression<D>`. Mientras que la conversión de una expresión lambda a un tipo delegado genera a un delegado que hace referencia a código ejecutable de la expresión lambda, la conversión a un tipo de árbol de expresión crea una representación de árbol de expresión de la expresión lambda.

Árboles de expresión son representaciones de datos en memoria eficiente de las expresiones lambda y hacer que la estructura de la expresión lambda transparente y explícita.

Al igual que un tipo de delegado `D`, `Expression<D>` se dice que tiene parámetros y tipos de valor devuelto, que son los mismos que los de `D`.

El ejemplo siguiente representa una expresión lambda como código ejecutable y como un árbol de expresión. Ya existe una conversión a `Func<int,int>`, también existe una conversión a `Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Siga estas asignaciones, el delegado `del` hace referencia a un método que devuelva `x + 1`y el árbol de expresión `exp` hace referencia a una estructura de datos que describe la expresión `x => x + 1`.

La definición exacta del tipo genérico `Expression<D>` , así como las reglas precisas para construir un árbol de expresión cuando una expresión lambda se convierte en un tipo de árbol de expresión, están fuera del ámbito de esta especificación.

Dos cosas son importantes para hacer explícito:

*  No todas las expresiones lambda se pueden convertir en árboles de expresión. Por ejemplo, las expresiones lambda con cuerpos de instrucción y las expresiones lambda que contiene expresiones de asignación no se puede representar. En estos casos, una conversión todavía existe, pero se producirá un error en tiempo de compilación. Estas excepciones se detallan en [conversiones de función anónima](conversions.md#anonymous-function-conversions).
*   `Expression<D>` ofrece un método de instancia `Compile` lo que genera un delegado del tipo `D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Al invocar a este delegado hace que el código representado por el árbol de expresión que se ejecutará. Por lo tanto, dadas las definiciones anteriores, SUPR y del2 son equivalentes y las dos instrucciones siguientes tendrán el mismo efecto:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Después de ejecutar este código, `i1` y `i2` , ambos tienen el valor `2`.

