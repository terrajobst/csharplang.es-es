---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616136"
---
# <a name="types"></a>Tipos

Los tipos del C# lenguaje se dividen en dos categorías principales: ***tipos de valor*** y ***tipos de referencia***. Ambos tipos de valor y tipos de referencia pueden ser ***tipos genéricos***, que toman uno o varios ***parámetros de tipo***. Los parámetros de tipo pueden designar tanto tipos de valor como tipos de referencia.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

La categoría final de tipos, punteros, solo está disponible en código no seguro. Esto se describe con más detalle en [tipos de puntero](unsafe-code.md#pointer-types).

Los tipos de valor se diferencian de los tipos de referencia en que las variables de los tipos de valor contienen directamente sus datos, mientras que las variables de los tipos de referencia almacenan ***referencias*** a sus datos, lo que se conoce como ***objetos***. Con los tipos de referencia, es posible que dos variables hagan referencia al mismo objeto y, por lo tanto, las operaciones en una variable afecten al objeto al que hace referencia la otra variable. Con los tipos de valor, cada variable tiene su propia copia de los datos y no es posible que las operaciones en una afecten a la otra.

C#el sistema de tipos de está unificado, de modo que un valor de cualquier tipo se puede tratar como un objeto. Todos los tipos de C# directa o indirectamente se derivan del tipo de clase `object`, y `object` es la clase base definitiva de todos los tipos. Los valores de tipos de referencia se tratan como objetos mediante la visualización de los valores como tipo `object`. Los valores de los tipos de valor se tratan como objetos realizando las operaciones de conversión boxing y unboxing ([conversión boxing y conversión unboxing](types.md#boxing-and-unboxing)).

## <a name="value-types"></a>Tipos de valor

Un tipo de valor es un tipo de estructura o un tipo de enumeración. C#proporciona un conjunto de tipos de struct predefinidos denominados ***tipos simples***. Los tipos simples se identifican mediante palabras reservadas.

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

A diferencia de una variable de un tipo de referencia, una variable de un tipo de valor solo puede contener el valor `null` si el tipo de valor es un tipo que acepta valores NULL.  Para cada tipo de valor que no acepta valores NULL hay un tipo de valor que acepta valores NULL correspondiente que denota el mismo conjunto de valores más el valor `null`.

La asignación a una variable de un tipo de valor crea una copia del valor que se va a asignar. Esto difiere de la asignación a una variable de un tipo de referencia, que copia la referencia pero no el objeto identificado por la referencia.

### <a name="the-systemvaluetype-type"></a>Tipo System. ValueType

Todos los tipos de valor heredan implícitamente de la clase `System.ValueType`, que, a su vez, hereda de la clase `object`. No es posible que ningún tipo derive de un tipo de valor y, por tanto, los tipos de valor están sellados implícitamente ([clases selladas](classes.md#sealed-classes)).

Tenga en cuenta que `System.ValueType` no es *value_type*a sí mismo. En su lugar, se trata de un *class_type* desde el que se derivan automáticamente todos los *value_type*s.

### <a name="default-constructors"></a>Constructores predeterminados

Todos los tipos de valor declaran implícitamente un constructor de instancia sin parámetros público denominado ***constructor predeterminado***. El constructor predeterminado devuelve una instancia inicializada en cero que se conoce como ***valor predeterminado*** para el tipo de valor:

*  En todos los *Simple_Type*s, el valor predeterminado es el valor generado por un patrón de bits de todos los ceros:
    * Para `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`y `ulong`, el valor predeterminado es `0`.
    * Por `char`, el valor predeterminado es `'\x0000'`.
    * Por `float`, el valor predeterminado es `0.0f`.
    * Por `double`, el valor predeterminado es `0.0d`.
    * Por `decimal`, el valor predeterminado es `0.0m`.
    * Por `bool`, el valor predeterminado es `false`.
*  Para un `E`*enum_type* , el valor predeterminado es `0`, convertido al tipo `E`.
*  En el caso de *struct_type*, el valor predeterminado es el valor generado al establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en `null`.
*  En el caso de *nullable_type* , el valor predeterminado es una instancia para la que la propiedad `HasValue` es false y la propiedad `Value` es undefined. El valor predeterminado también se conoce como el ***valor null*** del tipo que acepta valores NULL.

Al igual que cualquier otro constructor de instancia, el constructor predeterminado de un tipo de valor se invoca mediante el operador `new`. Por motivos de eficacia, este requisito no está diseñado para tener realmente la implementación que genera una llamada al constructor. En el ejemplo siguiente, las variables `i` y `j` se inicializan en cero.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Dado que cada tipo de valor tiene implícitamente un constructor de instancia sin parámetros público, no es posible que un tipo de struct contenga una declaración explícita de un constructor sin parámetros. Sin embargo, se permite que un tipo de estructura declare constructores de instancia con parámetros ([constructores](structs.md#constructors)).

### <a name="struct-types"></a>Tipos de estructura

Un tipo de estructura es un tipo de valor que puede declarar constantes, campos, métodos, propiedades, indizadores, operadores, constructores de instancias, constructores estáticos y tipos anidados. La declaración de tipos de struct se describe en [declaraciones de struct](structs.md#struct-declarations).

### <a name="simple-types"></a>Tipos simples

C#proporciona un conjunto de tipos de struct predefinidos denominados ***tipos simples***. Los tipos simples se identifican mediante palabras reservadas, pero estas palabras reservadas son simplemente alias para los tipos de struct predefinidos en el espacio de nombres `System`, como se describe en la tabla siguiente.


| __Palabra reservada__ | __Tipo con alias__ |
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

Dado que un tipo simple incluye un alias para un tipo de estructura, cada tipo simple tiene miembros. Por ejemplo, `int` tiene los miembros declarados en `System.Int32` y los miembros heredados de `System.Object`, y se permiten las siguientes instrucciones:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Los tipos simples se diferencian de otros tipos struct en que permiten determinadas operaciones adicionales:

*  La mayoría de los tipos simples permiten crear valores escribiendo *literales* ([literales](lexical-structure.md#literals)). Por ejemplo, `123` es un literal de tipo `int` y `'a'` es un literal de tipo `char`. C#no proporciona ningún aprovisionamiento para los literales de tipos struct en general y los valores no predeterminados de otros tipos struct siempre se crean en última instancia mediante constructores de instancias de esos tipos de estructura.
*  Cuando los operandos de una expresión son constantes de tipo simple, es posible que el compilador evalúe la expresión en tiempo de compilación. Este tipo de expresión se conoce como *constant_expression* ([expresiones constantes](expressions.md#constant-expressions)). Las expresiones que implican operadores definidos por otros tipos struct no se consideran expresiones constantes.
*  A través de `const` declaraciones es posible declarar constantes de los tipos simples ([constantes](classes.md#constants)). No es posible tener constantes de otros tipos struct, pero los campos `static readonly` proporcionan un efecto similar.
*  Las conversiones que implican tipos simples pueden participar en la evaluación de los operadores de conversión definidos por otros tipos struct, pero un operador de conversión definido por el usuario nunca puede participar en la evaluación de otro operador definido por el usuario ([evaluación de conversiones definidas por el usuario](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Tipos enteros

C#admite nueve tipos enteros: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`y `char`. Los tipos enteros tienen los siguientes tamaños y rangos de valores:

*  El tipo de `sbyte` representa enteros de 8 bits con signo con valores comprendidos entre-128 y 127.
*  El tipo de `byte` representa enteros de 8 bits sin signo con valores comprendidos entre 0 y 255.
*  El tipo de `short` representa enteros de 16 bits con signo con valores comprendidos entre-32768 y 32767.
*  El tipo de `ushort` representa enteros de 16 bits sin signo con valores comprendidos entre 0 y 65535.
*  El tipo de `int` representa enteros de 32 bits con signo con valores comprendidos entre-2147483648 y 2147483647.
*  El tipo de `uint` representa enteros de 32 bits sin signo con valores comprendidos entre 0 y 4294967295.
*  El tipo de `long` representa enteros de 64 bits con signo con valores comprendidos entre-9223372036854775808 y 9223372036854775807.
*  El tipo de `ulong` representa enteros de 64 bits sin signo con valores comprendidos entre 0 y 18446744073709551615.
*  El tipo de `char` representa enteros de 16 bits sin signo con valores comprendidos entre 0 y 65535. El conjunto de valores posibles para el tipo de `char` corresponde al Juego de caracteres Unicode. Aunque `char` tiene la misma representación que `ushort`, no todas las operaciones permitidas en un tipo se permiten en el otro.

Los operadores unarios y binarios de tipo entero siempre operan con una precisión de 32 bits con signo, una precisión de 32 bits sin signo, una precisión de 64 con signo o una precisión de bit 64 sin signo:

*  En el caso de los operadores unario `+` y `~`, el operando se convierte al tipo `T`, donde `T` es el primero de `int`, `uint`, `long`y `ulong` que pueden representar por completo todos los valores posibles del operando. La operación se realiza entonces con la precisión de tipo `T`y el tipo del resultado es `T`.
*  Para el operador unario `-`, el operando se convierte al tipo `T`, donde `T` es el primero de `int` y `long` que pueden representar por completo todos los valores posibles del operando. La operación se realiza entonces con la precisión de tipo `T`y el tipo del resultado es `T`. No se puede aplicar el operador unario `-` a los operandos de tipo `ulong`.
*  Para los operadores binarios `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`y `<=` , los operandos se convierten al tipo `T`, donde `T` es el primero de `int`, `uint`, `long`y `ulong` que pueden representar por completo todos los valores posibles de ambos operandos. A continuación, la operación se realiza utilizando la precisión de tipo `T`y el tipo del resultado es `T` (o `bool` para los operadores relacionales). No se permite que un operando sea de tipo `long` y el otro es de tipo `ulong` con los operadores binarios.
*  En el caso de los operadores binarios `<<` y `>>`, el operando izquierdo se convierte al tipo `T`, donde `T` es el primero de `int`, `uint`, `long`y `ulong` que pueden representar por completo todos los valores posibles del operando. La operación se realiza entonces con la precisión de tipo `T`y el tipo del resultado es `T`.

El tipo de `char` se clasifica como un tipo entero, pero difiere de los demás tipos enteros de dos maneras:

*  No hay ninguna conversión implícita de otros tipos al tipo `char`. En concreto, aunque los tipos `sbyte`, `byte`y `ushort` tienen intervalos de valores que se pueden representar completamente mediante el tipo de `char`, las conversiones implícitas de `sbyte`, `byte`o `ushort` a `char` no existen.
*  Las constantes del tipo `char` deben escribirse como *character_literal*s o como *integer_literal*s en combinación con una conversión al tipo `char`. Por ejemplo, `(char)10` es lo mismo que `'\x000A'`.

Los operadores y las instrucciones `checked` y `unchecked` se utilizan para controlar la comprobación de desbordamiento para las operaciones aritméticas de tipo entero y las conversiones ([los operadores Checked y unchecked](expressions.md#the-checked-and-unchecked-operators)). En un contexto de `checked`, un desbordamiento produce un error en tiempo de compilación o hace que se inicie una `System.OverflowException`. En un contexto de `unchecked`, se omiten los desbordamientos y se descartan los bits de orden superior que no caben en el tipo de destino.

### <a name="floating-point-types"></a>Tipos de punto flotante

C#admite dos tipos de punto flotante: `float` y `double`. Los tipos `float` y `double` se representan mediante los formatos IEEE 754 de precisión sencilla de 32 bits y de doble precisión de 64 bits, que proporcionan los siguientes conjuntos de valores:

*  Cero positivo y cero negativo. En la mayoría de los casos, cero positivo y cero negativo se comportan exactamente igual que el valor cero simple, pero ciertas operaciones distinguen entre los dos ([operador de división](expressions.md#division-operator)).
*  Infinito positivo y infinito negativo. Dichas operaciones producen infinitos, como la división de un número distinto de cero por cero. Por ejemplo, `1.0 / 0.0` produce infinito positivo y `-1.0 / 0.0` produce infinito negativo.
*  El valor ***no numérico*** , a menudo abreviado como Nan. Los Nan se generan mediante operaciones de punto flotante no válidas, como la división de cero por cero.
*  El conjunto finito de valores distintos de cero del formulario `s * m * 2^e`, donde `s` es 1 o-1, y `m` y `e` vienen determinados por el tipo de punto flotante determinado: por `float`, `0 < m < 2^24` y `-149 <= e <= 104`y para `double`, `0 < m < 2^53` y `-1075 <= e <= 970`. Los números de punto flotante desnormalizados se consideran valores distintos de cero válidos.

El tipo de `float` puede representar valores comprendidos entre aproximadamente `1.5 * 10^-45` para `3.4 * 10^38` con una precisión de 7 dígitos.

El tipo de `double` puede representar valores que van desde aproximadamente `5.0 * 10^-324` a `1.7 × 10^308` con una precisión de 15-16 dígitos.

Si uno de los operandos de un operador binario es de un tipo de punto flotante, el otro operando debe ser de un tipo entero o de un tipo de punto flotante, y la operación se evalúa como sigue:

*  Si uno de los operandos es de un tipo entero, ese operando se convierte en el tipo de punto flotante del otro operando.
*  Después, si alguno de los operandos es de tipo `double`, el otro operando se convierte en `double`, la operación se realiza utilizando al menos `double` intervalo y precisión, y el tipo del resultado es `double` (o `bool` para los operadores relacionales).
*  De lo contrario, la operación se realiza utilizando al menos `float` intervalo y la precisión, y el tipo del resultado es `float` (o `bool` para los operadores relacionales).

Los operadores de punto flotante, incluidos los operadores de asignación, nunca generan excepciones. En su lugar, en situaciones excepcionales, las operaciones de punto flotante producen cero, infinito o NaN, como se describe a continuación:

*  Si el resultado de una operación de punto flotante es demasiado pequeño para el formato de destino, el resultado de la operación se convierte en cero positivo o negativo.
*  Si el resultado de una operación de punto flotante es demasiado grande para el formato de destino, el resultado de la operación se convierte en infinito positivo o infinito negativo.
*  Si una operación de punto flotante no es válida, el resultado de la operación se convierte en NaN.
*  Si uno o ambos operandos de una operación de punto flotante son NaN, el resultado de la operación se convierte en NaN.

Las operaciones de punto flotante se pueden realizar con una precisión mayor que el tipo de resultado de la operación. Por ejemplo, algunas arquitecturas de hardware admiten un tipo de punto flotante "extendido" o "Long Double" con un intervalo y una precisión mayores que el tipo de `double` y realizan implícitamente todas las operaciones de punto flotante con este tipo de precisión superior. Solo con un costo excesivo en el rendimiento se pueden realizar estas arquitecturas de hardware para realizar operaciones de punto flotante con menos precisión, y en lugar de requerir una implementación para que pierda C# rendimiento y precisión, permite un tipo de precisión mayor. que se va a utilizar para todas las operaciones de punto flotante. Aparte de ofrecer resultados más precisos, esto rara vez tiene efectos medibles. Sin embargo, en las expresiones con el formato `x * y / z`, donde la multiplicación genera un resultado que está fuera del intervalo de `double`, pero la división posterior devuelve el resultado temporal al intervalo de `double`, el hecho de que la expresión se evalúa en un un formato de intervalo superior puede provocar que se genere un resultado finito en lugar de un infinito.

### <a name="the-decimal-type"></a>Tipo decimal

El tipo `decimal` es un tipo de datos de 128 bits adecuado para cálculos financieros y monetarios. El tipo de `decimal` puede representar valores comprendidos entre `1.0 * 10^-28` aproximadamente `7.9 * 10^28` con 28-29 dígitos significativos.

El conjunto finito de valores de tipo `decimal` tiene el formato `(-1)^s * c * 10^-e`, donde el signo `s` es 0 ó 1, el coeficiente `c` viene dado por `0 <= *c* < 2^96`y el `e` de escala es tal que `0 <= e <= 28`. El tipo de `decimal` no admite ceros con signo, infinitos o NaN. Un `decimal` se representa como un entero de 96 bits escalado por una potencia de diez. En el caso de `decimal`s con un valor absoluto inferior a `1.0m`, el valor es exacto hasta la posición decimal 28, pero no más. Para `decimal`s con un valor absoluto mayor o igual que `1.0m`, el valor es exacto a 28 o 29 dígitos. Al contrario que los tipos de datos `float` y `double`, los números fraccionarios decimales como 0,1 se pueden representar exactamente en la representación `decimal`. En las representaciones de `float` y `double`, estos números suelen ser fracciones infinitas, por lo que las representaciones son más propensas a errores de redondeo.

Si uno de los operandos de un operador binario es de tipo `decimal`, el otro operando debe ser de un tipo entero o de un tipo `decimal`. Si hay un operando de tipo entero, se convierte en `decimal` antes de que se realice la operación.

El resultado de una operación con valores de tipo `decimal` es que resultaría de calcular un resultado exacto (conservando la escala, tal y como se define para cada operador) y, a continuación, redondear para ajustarse a la representación. Los resultados se redondean al valor representable más cercano y, cuando un resultado está igualmente cerca de dos valores representables, al valor que tiene un número par en la posición del dígito menos significativo (esto se conoce como "redondeo bancario"). Un resultado de cero siempre tiene un signo de 0 y una escala de 0.

Si una operación aritmética decimal produce un valor menor o igual que `5 * 10^-29` en valor absoluto, el resultado de la operación se convierte en cero. Si una operación aritmética `decimal` genera un resultado que es demasiado grande para el formato de `decimal`, se produce una `System.OverflowException`.

El tipo de `decimal` tiene una precisión mayor pero menor que los tipos de punto flotante. Por lo tanto, las conversiones de los tipos de punto flotante a `decimal` pueden producir excepciones de desbordamiento, y las conversiones de `decimal` a los tipos de punto flotante podrían provocar la pérdida de precisión. Por estos motivos, no existe ninguna conversión implícita entre los tipos de punto flotante y `decimal`, y sin conversiones explícitas, no es posible mezclar operandos de punto flotante y `decimal` en la misma expresión.

### <a name="the-bool-type"></a>Tipo bool

El tipo de `bool` representa las cantidades lógicas booleanas. Los valores posibles de tipo `bool` son `true` y `false`.

No existe ninguna conversión estándar entre `bool` y otros tipos. En concreto, el tipo de `bool` es distinto y separado de los tipos enteros, y no se puede usar un valor `bool` en lugar de un valor entero y viceversa.

En los lenguajes C++ C y, un valor entero o de punto flotante cero, o un puntero nulo se puede convertir al valor booleano `false`, y un valor entero o de punto flotante distinto de cero, o un puntero no nulo se puede convertir al valor booleano `true`. En C#, estas conversiones se realizan comparando explícitamente un valor entero o de punto flotante en cero, o comparando explícitamente una referencia de objeto a `null`.

### <a name="enumeration-types"></a>Tipos de enumeración

Un tipo de enumeración es un tipo distinto con constantes con nombre. Cada tipo de enumeración tiene un tipo subyacente, que debe ser `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` o `ulong`. El conjunto de valores del tipo de enumeración es el mismo que el conjunto de valores del tipo subyacente. Los valores del tipo de enumeración no están restringidos a los valores de las constantes con nombre. Los tipos de enumeración se definen mediante declaraciones de enumeración ([declaraciones](enums.md#enum-declarations)de enumeración).

### <a name="nullable-types"></a>Tipos que aceptan valores NULL

Un tipo que acepta valores NULL puede representar todos los valores de su ***tipo subyacente*** más un valor null adicional. Un tipo que acepta valores NULL se escribe `T?`, donde `T` es el tipo subyacente. Esta sintaxis es una abreviatura de `System.Nullable<T>`y las dos formas se pueden usar indistintamente.

Un ***tipo de valor que no acepta valores NULL*** es un tipo de valor distinto de `System.Nullable<T>` y su `T?` abreviado (para cualquier `T`), además de cualquier parámetro de tipo restringido para ser un tipo de valor que no acepta valores NULL (es decir, cualquier parámetro de tipo con una `struct` restricción). El tipo de `System.Nullable<T>` especifica la restricción de tipo de valor para `T` ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), lo que significa que el tipo subyacente de un tipo que acepta valores NULL puede ser cualquier tipo de valor que no acepte valores NULL. El tipo subyacente de un tipo que acepta valores NULL no puede ser un tipo que acepta valores NULL ni un tipo de referencia. Por ejemplo, `int??` y `string?` son tipos no válidos.

Una instancia de un tipo que acepta valores NULL `T?` tiene dos propiedades públicas de solo lectura:

*  Propiedad `HasValue` de tipo `bool`
*  Propiedad `Value` de tipo `T`

Una instancia para la que `HasValue` es true se dice que no es NULL. Una instancia no NULL contiene un valor conocido y `Value` devuelve ese valor.

Una instancia para la que `HasValue` es false se dice que es NULL. Una instancia null tiene un valor sin definir. Al intentar leer el `Value` de una instancia null, se produce una `System.InvalidOperationException`. El proceso de acceso a la propiedad `Value` de una instancia que acepta valores NULL se conoce como ***desencapsulado***.

Además del constructor predeterminado, todos los tipos que aceptan valores NULL `T?` tienen un constructor público que toma un único argumento de tipo `T`. Dado un valor `x` de tipo `T`, una invocación del constructor con el formato

```csharp
new T?(x)
```
crea una instancia no NULL de `T?` para la que se `x`la propiedad `Value`. El proceso de creación de una instancia no NULL de un tipo que acepta valores NULL para un valor determinado se conoce como ***ajuste***.

Las conversiones implícitas están disponibles desde el literal `null` a `T?` ([conversiones de literales null](conversions.md#null-literal-conversions)) y de `T` a `T?` ([conversiones implícitas que aceptan valores NULL](conversions.md#implicit-nullable-conversions)).

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

Un valor de tipo de referencia es una referencia a una ***instancia*** del tipo, la última conocida como un ***objeto***. El valor especial `null` es compatible con todos los tipos de referencia e indica la ausencia de una instancia.

### <a name="class-types"></a>Tipos de clase

Un tipo de clase define una estructura de datos que contiene miembros de datos (constantes y campos), miembros de función (métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos) y tipos anidados. Los tipos de clase admiten la herencia, un mecanismo por el que las clases derivadas pueden extender y especializar clases base. Las instancias de tipos de clase se crean mediante *object_creation_expression*s ([expresiones de creación de objetos](expressions.md#object-creation-expressions)).

Los tipos de clase se describen en [clases](classes.md).

Algunos tipos de clase predefinidos tienen un significado C# especial en el idioma, tal y como se describe en la tabla siguiente.


| __Tipo de clase__     | __Descripción__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | Última clase base de todos los demás tipos. Vea [el tipo de objeto](types.md#the-object-type). | 
| `System.String`    | Tipo de cadena del C# lenguaje. Vea [el tipo de cadena](types.md#the-string-type).         |
| `System.ValueType` | Clase base de todos los tipos de valor. Vea [el tipo System. ValueType](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | La clase base de todos los tipos de enumeración. Vea [enumeraciones](enums.md).              |
| `System.Array`     | La clase base de todos los tipos de matriz. Vea [Matrices](arrays.md).             |
| `System.Delegate`  | La clase base de todos los tipos de delegado. Vea [delegados](delegates.md).          |
| `System.Exception` | La clase base de todos los tipos de excepción. Vea [excepciones](exceptions.md).         |

### <a name="the-object-type"></a>Tipo object

El tipo de clase `object` es la última clase base de todos los demás tipos. Cada tipo se C# deriva directa o indirectamente del tipo de clase `object`.

La palabra clave `object` es simplemente un alias para la clase predefinida `System.Object`.

### <a name="the-dynamic-type"></a>Tipo dynamic

El tipo de `dynamic`, como `object`, puede hacer referencia a cualquier objeto. Cuando se aplican operadores a expresiones de tipo `dynamic`, su resolución se aplaza hasta que se ejecuta el programa. Por lo tanto, si el operador no se puede aplicar legalmente al objeto al que se hace referencia, no se proporciona ningún error durante la compilación. En su lugar, se producirá una excepción cuando se produzca un error en la resolución del operador en tiempo de ejecución.

Su finalidad es permitir el enlace dinámico, que se describe en detalle en el [enlace dinámico](expressions.md#dynamic-binding).

`dynamic` se considera idéntica a `object` excepto en los siguientes aspectos:

*  Las operaciones en expresiones de tipo `dynamic` se pueden enlazar dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).
*  La inferencia de tipos ([inferencia de tipos](expressions.md#type-inference)) preferirá `dynamic` sobre `object` si ambos son candidatos.

Debido a esta equivalencia, lo siguiente incluye:

*  Existe una conversión implícita de identidad entre `object` y `dynamic`, y entre tipos construidos que son iguales al reemplazar `dynamic` por `object`
*  Las conversiones implícitas y explícitas a y desde `object` también se aplican a y desde `dynamic`.
*  Las firmas de método que son iguales al reemplazar `dynamic` por `object` se consideran la misma firma
*  No se distingue el tipo `dynamic` de `object` en tiempo de ejecución.
*  Una expresión de tipo `dynamic` se conoce como una ***expresión dinámica***.

### <a name="the-string-type"></a>Tipo string

El tipo de `string` es un tipo de clase sellado que hereda directamente de `object`. Las instancias de la clase `string` representan cadenas de caracteres Unicode.

Los valores del tipo `string` pueden escribirse como literales de cadena ([literales de cadena](lexical-structure.md#string-literals)).

La palabra clave `string` es simplemente un alias para la clase predefinida `System.String`.

### <a name="interface-types"></a>Tipos de interfaz

Una interfaz define un contrato. Una clase o estructura que implementa una interfaz debe adherirse a su contrato. Una interfaz puede heredar de varias interfaces base, y una clase o estructura puede implementar varias interfaces.

Los tipos de interfaz se describen en [interfaces](interfaces.md).

### <a name="array-types"></a>Tipos de matriz

Una matriz es una estructura de datos que contiene cero o más variables a las que se tiene acceso a través de índices calculados. Las variables contenidas en una matriz, también denominadas elementos de la matriz, son todas del mismo tipo, y este tipo se denomina tipo de elemento de la matriz.

Los tipos de matriz se describen en [matrices](arrays.md).

### <a name="delegate-types"></a>Tipos delegados

Un delegado es una estructura de datos que hace referencia a uno o más métodos. En el caso de los métodos de instancia, también hace referencia a sus instancias de objeto correspondientes.

El equivalente más cercano de un delegado en C C++ o es un puntero de función, pero mientras que un puntero de función solo puede hacer referencia a funciones estáticas, un delegado puede hacer referencia a métodos estáticos y de instancia. En el último caso, el delegado almacena no solo una referencia al punto de entrada del método, sino también una referencia a la instancia de objeto en la que se invoca el método.

Los tipos de delegado se describen en [delegados](delegates.md).

## <a name="boxing-and-unboxing"></a>Conversión boxing y conversión unboxing

El concepto de conversión boxing y unboxing es central C#para el sistema de tipos de. Proporciona un puente entre *value_type*s y *reference_type*s, ya que permite que cualquier valor de una *value_type* se convierta al tipo `object`y desde este. Las conversiones boxing y unboxing permiten una vista unificada del sistema de tipos en el que un valor de cualquier tipo se puede tratar en última instancia como un objeto.

### <a name="boxing-conversions"></a>Conversiones Boxing

Una conversión boxing permite que un *value_type* se convierta implícitamente en un *reference_type*. Existen las siguientes conversiones Boxing:

*  De cualquier *value_type* al tipo `object`.
*  De cualquier *value_type* al tipo `System.ValueType`.
*  De cualquier *non_nullable_value_type* a cualquier *interface_type* implementado por *value_type*.
*  De cualquier *nullable_type* a cualquier *interface_type* implementado por el tipo subyacente de *nullable_type*.
*  De cualquier *enum_type* al tipo `System.Enum`.
*  Desde cualquier *nullable_type* con un *enum_type* subyacente al tipo `System.Enum`.
*  Tenga en cuenta que una conversión implícita de un parámetro de tipo se ejecutará como conversión boxing si en tiempo de ejecución termina la conversión de un tipo de valor a un tipo de referencia ([conversiones implícitas que implican parámetros de tipo](conversions.md#implicit-conversions-involving-type-parameters)).

La conversión boxing de un valor de *non_nullable_value_type* consiste en asignar una instancia de objeto y copiar el valor de *non_nullable_value_type* en esa instancia.

La conversión boxing de un valor de *nullable_type* genera una referencia nula si es el valor `null` (`HasValue` se `false`) o el resultado de desencapsular y convertir en Boxing el valor subyacente en caso contrario.

El proceso real de conversión boxing de un valor de *non_nullable_value_type* se explica mejor al imaginarse la existencia de una ***clase de conversión boxing***genérica, que se comporta como si se hubiese declarado de la siguiente manera:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

La conversión boxing de un valor `v` de tipo `T` ahora consiste en ejecutar la expresión `new Box<T>(v)`y devolver la instancia resultante como un valor de tipo `object`. Por lo tanto, las instrucciones
```csharp
int i = 123;
object box = i;
```
se corresponden conceptualmente con
```csharp
int i = 123;
object box = new Box<int>(i);
```

Una clase de conversión boxing como `Box<T>` anterior no existe realmente y el tipo dinámico de un valor de conversión boxing no es realmente un tipo de clase. En su lugar, un valor con conversión boxing de tipo `T` tiene el tipo dinámico `T`y una comprobación de tipos dinámicos mediante el operador `is` puede simplemente hacer referencia a `T`de tipo. Por ejemplo,
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
generará la cadena "`Box contains an int`" en la consola.

Una conversión boxing implica que se realice una copia del valor al que se va a aplicar la conversión boxing. Esto es diferente de la conversión de *reference_type* al tipo `object`, en el que el valor sigue haciendo referencia a la misma instancia y simplemente se considera el tipo menos derivado `object`. Por ejemplo, dada la declaración
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
las instrucciones siguientes
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
generará el valor 10 en la consola porque la operación de conversión boxing implícita que se produce en la asignación de `p` a `box` hace que se copie el valor de `p`. Si `Point` ha declarado un `class` en su lugar, se generará el valor 20 porque `p` y `box` harían referencia a la misma instancia.

### <a name="unboxing-conversions"></a>Conversiones unboxing

Una conversión unboxing permite que un *reference_type* se convierta explícitamente en un *value_type*. Existen las siguientes conversiones unboxing:

*  Del tipo `object` a cualquier *value_type*.
*  Del tipo `System.ValueType` a cualquier *value_type*.
*  De cualquier *interface_type* a cualquier *non_nullable_value_type* que implemente *interface_type*.
*  De cualquier *interface_type* a cualquier *nullable_type* cuyo tipo subyacente implementa *interface_type*.
*  Del tipo `System.Enum` a cualquier *enum_type*.
*  Del tipo `System.Enum` a cualquier *nullable_type* con un *enum_type*subyacente.
*  Tenga en cuenta que una conversión explícita a un parámetro de tipo se ejecutará como una conversión unboxing si en tiempo de ejecución termina la conversión de un tipo de referencia a un tipo de valor ([conversiones dinámicas explícitas](conversions.md#explicit-dynamic-conversions)).

Una operación de conversión unboxing a un *non_nullable_value_type* consiste en comprobar primero que la instancia de objeto es un valor de conversión boxing de la *non_nullable_value_type*especificada y, a continuación, copiar el valor fuera de la instancia.

La conversión unboxing a un *nullable_type* genera el valor null de *nullable_type* si el operando de origen es `null`, o el resultado ajustado de la conversión unboxing de la instancia de objeto en el tipo subyacente del *nullable_type* en caso contrario.

Al hacer referencia a la clase de conversión boxing imaginaria descrita en la sección anterior, una conversión unboxing de un objeto `box` a un `T` *value_type* consiste en ejecutar la expresión `((Box<T>)box).value`. Por lo tanto, las instrucciones
```csharp
object box = 123;
int i = (int)box;
```
se corresponden conceptualmente con
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Para que una conversión unboxing a un *non_nullable_value_type* determinado se realice correctamente en tiempo de ejecución, el valor del operando de origen debe ser una referencia a un valor de conversión boxing de ese *non_nullable_value_type*. Si el operando de origen es `null`, se produce una `System.NullReferenceException`. Si el operando de origen es una referencia a un objeto incompatible, se produce una `System.InvalidCastException`.

Para que una conversión unboxing a un *nullable_type* determinado se realice correctamente en tiempo de ejecución, el valor del operando de origen debe ser `null` o una referencia a un valor de conversión boxing de la *non_nullable_value_type* subyacente de *nullable_type*. Si el operando de origen es una referencia a un objeto incompatible, se produce una `System.InvalidCastException`.

## <a name="constructed-types"></a>Tipos construidos

Una declaración de tipo genérico, por sí sola, denota un ***tipo genérico sin enlazar*** que se usa como "Blueprint" para formar muchos tipos diferentes, por medio de la aplicación de ***argumentos de tipo***. Los argumentos de tipo se escriben entre corchetes angulares (`<` y `>`) inmediatamente después del nombre del tipo genérico. Un tipo que incluye al menos un argumento de tipo se denomina ***tipo construido***. Un tipo construido se puede usar en la mayoría de los lugares del lenguaje en el que puede aparecer un nombre de tipo. Un tipo genérico sin enlazar solo se puede usar dentro de un *typeof_expression* ([el operador typeof](expressions.md#the-typeof-operator)).

Los tipos construidos también se pueden usar en expresiones como nombres simples ([nombres simples](expressions.md#simple-names)) o al obtener acceso a un miembro ([acceso a miembros](expressions.md#member-access)).

Cuando se evalúa un *namespace_or_type_name* , solo se tienen en cuenta los tipos genéricos con el número correcto de parámetros de tipo. Por lo tanto, es posible usar el mismo identificador para identificar distintos tipos, siempre que los tipos tengan distintos números de parámetros de tipo. Esto resulta útil cuando se combinan clases genéricas y no genéricas en el mismo programa:

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

Un *type_name* podría identificar un tipo construido aunque no especifique directamente los parámetros de tipo. Esto puede ocurrir cuando un tipo está anidado dentro de una declaración de clase genérica y el tipo de instancia de la declaración contenedora se usa implícitamente para la búsqueda de nombres ([tipos anidados en clases genéricas](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

En código no seguro, un tipo construido no se puede usar como *unmanaged_type* ([tipos de puntero](unsafe-code.md#pointer-types)).

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

En el código no seguro ([código no seguro](unsafe-code.md)), un *type_argument* no puede ser un tipo de puntero. Cada argumento de tipo debe satisfacer las restricciones en el parámetro de tipo correspondiente ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).

### <a name="open-and-closed-types"></a>Tipos abiertos y cerrados

Todos los tipos se pueden clasificar como ***tipos abiertos*** o ***tipos cerrados***. Un tipo abierto es un tipo que implica parámetros de tipo. Más específicamente:

*  Un parámetro de tipo define un tipo abierto.
*  Un tipo de matriz es un tipo abierto solo si su tipo de elemento es un tipo abierto.
*  Un tipo construido es un tipo abierto solo si uno o varios de sus argumentos de tipo es un tipo abierto. Un tipo anidado construido es un tipo abierto si y solo si uno o varios de sus argumentos de tipo o los argumentos de tipo de sus tipos contenedores es un tipo abierto.

Un tipo cerrado es un tipo que no es un tipo abierto.

En tiempo de ejecución, todo el código dentro de una declaración de tipos genéricos se ejecuta en el contexto de un tipo construido cerrado que se creó mediante la aplicación de argumentos de tipo a la declaración genérica. Cada parámetro de tipo dentro del tipo genérico se enlaza a un tipo en tiempo de ejecución determinado. El procesamiento en tiempo de ejecución de todas las instrucciones y expresiones siempre se produce con tipos cerrados y los tipos abiertos solo se producen durante el procesamiento en tiempo de compilación.

Cada tipo construido cerrado tiene su propio conjunto de variables estáticas, que no se comparten con otros tipos construidos cerrados. Puesto que no existe un tipo abierto en tiempo de ejecución, no hay variables estáticas asociadas a un tipo abierto. Dos tipos construidos cerrados son del mismo tipo si se construyen a partir del mismo tipo genérico sin enlazar y sus argumentos de tipo correspondientes son del mismo tipo.

### <a name="bound-and-unbound-types"></a>Tipos enlazados y sin enlazar

El término ***tipo sin enlazar*** hace referencia a un tipo no genérico o a un tipo genérico sin enlazar. El término de ***tipo enlazado*** hace referencia a un tipo no genérico o a un tipo construido.

Un tipo sin enlazar hace referencia a la entidad declarada por una declaración de tipos. Un tipo genérico sin enlazar no es en sí mismo un tipo y no se puede usar como el tipo de una variable, argumento o valor devuelto, o como un tipo base. La única construcción en la que se puede hacer referencia a un tipo genérico sin enlazar es la expresión `typeof` ([el operador typeof](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Satisfacer restricciones

Siempre que se hace referencia a un tipo construido o a un método genérico, los argumentos de tipo proporcionados se comprueban con las restricciones de parámetro de tipo declaradas en el tipo o método genérico ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)). Para cada cláusula `where`, el argumento de tipo `A` que corresponde al parámetro de tipo con nombre se compara con cada restricción de la siguiente manera:

*  Si la restricción es un tipo de clase, un tipo de interfaz o un parámetro de tipo, deje `C` representar esa restricción con los argumentos de tipo proporcionados que se sustituyen por los parámetros de tipo que aparecen en la restricción. Para satisfacer la restricción, debe ser el caso de que el tipo `A` sea convertible al tipo `C` por una de las siguientes acciones:
    * Una conversión de identidad ([conversión de identidad](conversions.md#identity-conversion))
    * Una conversión de referencia implícita ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions))
    * Una conversión boxing ([conversiones boxing](conversions.md#boxing-conversions)), siempre que el tipo a sea un tipo de valor que no acepte valores NULL.
    * Referencia implícita, conversión boxing o conversión de parámetros de tipo de un parámetro de tipo `A` a `C`.
*  Si la restricción es la restricción de tipo de referencia (`class`), el tipo `A` debe cumplir uno de los siguientes elementos:
    * `A` es un tipo de interfaz, tipo de clase, tipo de delegado o tipo de matriz. Tenga en cuenta que `System.ValueType` y `System.Enum` son tipos de referencia que cumplen esta restricción.
    * `A` es un parámetro de tipo que se sabe que es un tipo de referencia ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).
*  Si la restricción es la restricción de tipo de valor (`struct`), el tipo `A` debe cumplir uno de los siguientes elementos:
    * `A` es un tipo de estructura o un tipo de enumeración, pero no un tipo que acepta valores NULL. Tenga en cuenta que `System.ValueType` y `System.Enum` son tipos de referencia que no cumplen esta restricción.
    * `A` es un parámetro de tipo que tiene la restricción de tipo de valor ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).
*  Si la restricción es la restricción de constructor `new()`, el `A` de tipo no debe estar `abstract` y debe tener un constructor sin parámetros público. Esto se cumple si se cumple una de las siguientes condiciones:
    * `A` es un tipo de valor, ya que todos los tipos de valor tienen un constructor predeterminado público ([constructores predeterminados](types.md#default-constructors)).
    * `A` es un parámetro de tipo que tiene la restricción de constructor ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).
    * `A` es un parámetro de tipo que tiene la restricción de tipo de valor ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).
    * `A` es una clase que no se `abstract` y contiene un constructor de `public` explícitamente declarado sin parámetros.
    * `A` no es `abstract` y tiene un constructor predeterminado ([constructores predeterminados](classes.md#default-constructors)).

Se produce un error en tiempo de compilación si los argumentos de tipo especificados no satisfacen una o varias restricciones de un parámetro de tipo.

Dado que los parámetros de tipo no se heredan, las restricciones nunca se heredan. En el ejemplo siguiente, `D` debe especificar la restricción en su parámetro de tipo `T` para que `T` cumpla la restricción impuesta por la `B<T>`de clase base. En cambio, la clase `E` no necesita especificar una restricción, porque `List<T>` implementa `IEnumerable` para cualquier `T`.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Parámetros de tipo

Un parámetro de tipo es un identificador que designa un tipo de valor o un tipo de referencia al que está enlazado el parámetro en tiempo de ejecución.

```antlr
type_parameter
    : identifier
    ;
```

Dado que se pueden crear instancias de un parámetro de tipo con muchos argumentos de tipo reales diferentes, los parámetros de tipo tienen operaciones y restricciones ligeramente diferentes a las de otros tipos. Se incluyen los siguientes:

*  Un parámetro de tipo no se puede usar directamente para declarar una clase base ([clase base](classes.md#base-class)) o una interfaz ([listas de parámetros de tipo variante](interfaces.md#variant-type-parameter-lists)).
*  Las reglas para la búsqueda de miembros en parámetros de tipo dependen de las restricciones, si las hay, que se aplican al parámetro de tipo. Se detallan en la [búsqueda de miembros](expressions.md#member-lookup).
*  Las conversiones disponibles para un parámetro de tipo dependen de las restricciones, si las hay, que se aplican al parámetro de tipo. Se detallan en [conversiones implícitas que implican parámetros de tipo](conversions.md#implicit-conversions-involving-type-parameters) y [conversiones dinámicas explícitas](conversions.md#explicit-dynamic-conversions).
*  El `null` literal no se puede convertir en un tipo proporcionado por un parámetro de tipo, excepto si se sabe que el parámetro de tipo es un tipo de referencia ([conversiones implícitas que implican parámetros de tipo](conversions.md#implicit-conversions-involving-type-parameters)). Sin embargo, en su lugar se puede usar una expresión `default` ([expresiones de valor predeterminado](expressions.md#default-value-expressions)). Además, un valor con un tipo proporcionado por un parámetro de tipo puede compararse con `null` mediante `==` y `!=` ([operadores de igualdad de tipos de referencia](expressions.md#reference-type-equality-operators)), a menos que el parámetro de tipo tenga la restricción de tipo de valor.
*  Una expresión `new` ([expresiones de creación de objetos](expressions.md#object-creation-expressions)) solo se puede utilizar con un parámetro de tipo si el parámetro de tipo está restringido por un *constructor_constraint* o la restricción de tipo de valor (restricciones de[parámetro de tipo](classes.md#type-parameter-constraints)).
*  Un parámetro de tipo no se puede usar en ningún lugar dentro de un atributo.
*  No se puede usar un parámetro de tipo en un acceso de miembro ([acceso a miembros](expressions.md#member-access)) o nombre de tipo (espacio de nombres[y nombres de tipo](basic-concepts.md#namespace-and-type-names)) para identificar un miembro estático o un tipo anidado.
*  En código no seguro, no se puede usar un parámetro de tipo como *unmanaged_type* ([tipos de puntero](unsafe-code.md#pointer-types)).

Como tipo, los parámetros de tipo son únicamente una construcción en tiempo de compilación. En tiempo de ejecución, cada parámetro de tipo se enlaza a un tipo en tiempo de ejecución que se especificó proporcionando un argumento de tipo a la declaración de tipos genéricos. Por lo tanto, el tipo de una variable declarada con un parámetro de tipo, en tiempo de ejecución, será un tipo construido cerrado ([tipos abiertos y cerrados](types.md#open-and-closed-types)). La ejecución en tiempo de ejecución de todas las instrucciones y expresiones que impliquen parámetros de tipo usa el tipo real que se proporcionó como argumento de tipo para ese parámetro.

## <a name="expression-tree-types"></a>Tipos de árbol de expresión

Los ***árboles de expresión*** permiten representar expresiones lambda como estructuras de datos en lugar de código ejecutable. Los árboles de expresión son valores de ***tipos de árbol de expresión*** con el formato `System.Linq.Expressions.Expression<D>`, donde `D` es cualquier tipo de delegado. En el resto de esta especificación, haremos referencia a estos tipos mediante el `Expression<D>`abreviado.

Si existe una conversión de una expresión lambda a un tipo de delegado `D`, también existe una conversión al tipo de árbol de expresión `Expression<D>`. Mientras que la conversión de una expresión lambda a un tipo de delegado genera un delegado que hace referencia al código ejecutable de la expresión lambda, la conversión a un tipo de árbol de expresión crea una representación de árbol de expresión de la expresión lambda.

Los árboles de expresión son representaciones eficaces de datos en memoria de expresiones lambda y hacen que la estructura de la expresión lambda sea transparente y explícita.

Al igual que un tipo de delegado `D`, se dice que `Expression<D>` tiene tipos de parámetro y de valor devuelto, que son los mismos que los de `D`.

En el ejemplo siguiente se representa una expresión lambda como código ejecutable y como un árbol de expresión. Dado que existe una conversión a `Func<int,int>`, también existe una conversión para `Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Después de estas asignaciones, el delegado `del` hace referencia a un método que devuelve `x + 1`y el árbol de expresión `exp` hace referencia a una estructura de datos que describe la `x => x + 1`de la expresión.

La definición exacta del tipo genérico `Expression<D>` así como las reglas precisas para construir un árbol de expresión cuando una expresión lambda se convierte en un tipo de árbol de expresión, está fuera del ámbito de esta especificación.

Es importante hacer dos cosas:

*  No todas las expresiones lambda se pueden convertir en árboles de expresión. Por ejemplo, las expresiones lambda con cuerpos de instrucciones y expresiones lambda que contienen expresiones de asignación no se pueden representar. En estos casos, todavía existe una conversión, pero se producirá un error en tiempo de compilación. Estas excepciones se detallan en [conversiones de funciones anónimas](conversions.md#anonymous-function-conversions).
*   `Expression<D>` ofrece un método de instancia `Compile` que genera un delegado de tipo `D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Al invocar este delegado se produce el código representado por el árbol de expresión que se va a ejecutar. Por lo tanto, dado que las definiciones anteriores, del y DEL2 son equivalentes, y las dos instrucciones siguientes tendrán el mismo efecto:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Después de ejecutar este código, `i1` y `i2` tendrán el valor `2`.

