# <a name="lexical-structure"></a>Estructura léxica

## <a name="programs"></a>Programas

C# ***programa*** consta de uno o varios ***archivos de código fuente***, que se conoce formalmente como ***unidades de compilación*** ([unidades de compilación](namespaces.md#compilation-units)). Un archivo de origen es una secuencia ordenada de caracteres Unicode. Archivos de código fuente normalmente tienen una correspondencia uno a uno con los archivos en un sistema de archivos, pero no se requiere esta correspondencia. Para maximizar la portabilidad, se recomienda que los archivos en un sistema de archivos se puede codificar con UTF-8 de codificación.

En términos conceptuales, un programa se compila en tres pasos:

1. Transformación que convierte un archivo desde un repertorio de caracteres determinado y un esquema de codificación en una secuencia de caracteres Unicode.
2. Análisis léxico, que conviertan una secuencia de caracteres de entrada Unicode en una secuencia de tokens.
3. Análisis sintáctico, que convierte la secuencia de tokens en el código ejecutable.

## <a name="grammars"></a>Gramáticas

Esta especificación presenta la sintaxis del lenguaje C# programación mediante dos gramáticas. El ***gramática léxica*** ([gramática léxica](lexical-structure.md#lexical-grammar)) define cómo se combinan los caracteres Unicode para formar terminadores de línea, espacio en blanco, comentarios, los tokens y las directivas de preprocesamiento. El ***gramática sintáctica*** ([gramática sintáctica](lexical-structure.md#syntactic-grammar)) define cómo se combinan los tokens resultantes de la gramática léxica para formar programas de C#.

### <a name="grammar-notation"></a>Notación gramatical

Las gramáticas léxicas y sintácticas se presentan en forma de Backus-Naur mediante la notación de la herramienta de gramática ANTLR.

### <a name="lexical-grammar"></a>Gramática léxica

La gramática léxica de C# se presenta en [análisis léxico](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), y [directivas de preprocesamiento](lexical-structure.md#pre-processing-directives). Los símbolos terminales de la gramática léxica son los caracteres del juego de caracteres Unicode y la gramática léxica especifica cómo se combinan los caracteres a los tokens de formulario ([Tokens](lexical-structure.md#tokens)), espacio en blanco ([espacio en blanco](lexical-structure.md#white-space)), los comentarios ([comentarios](lexical-structure.md#comments)) y las directivas de preprocesamiento ([directivas de preprocesamiento](lexical-structure.md#pre-processing-directives)).

Cada archivo de código fuente en un programa de C# debe ajustarse a la *entrada* producción de la gramática léxica ([análisis léxico](lexical-structure.md#lexical-analysis)).

### <a name="syntactic-grammar"></a>Gramática sintáctica

La gramática sintáctica de C# se presenta en los capítulos y apéndices que siguen este capítulo. Los símbolos terminales de la gramática sintáctica son los tokens definidos por la gramática léxica, y la gramática sintáctica especifica cómo se combinan los tokens para formar programas de C#.

Cada archivo de código fuente en un programa de C# debe ajustarse a la *compilation_unit* producción de la gramática sintáctica ([unidades de compilación](namespaces.md#compilation-units)).

## <a name="lexical-analysis"></a>Análisis léxico

El *entrada* producción define la estructura léxica de un archivo de código fuente de C#. Cada archivo de código fuente en un programa de C# debe ajustarse a este trabajo de producción del léxico.

```antlr
input
    : input_section?
    ;

input_section
    : input_section_part+
    ;

input_section_part
    : input_element* new_line
    | pp_directive
    ;

input_element
    : whitespace
    | comment
    | token
    ;
```

Cinco elementos básicos constituyen la estructura léxica de un archivo de código fuente de C#: terminadores de línea ([terminadores de línea](lexical-structure.md#line-terminators)), espacio en blanco ([espacio en blanco](lexical-structure.md#white-space)), los comentarios ([comentarios](lexical-structure.md#comments)), los tokens ([Tokens](lexical-structure.md#tokens)) y las directivas de preprocesamiento ([directivas de preprocesamiento](lexical-structure.md#pre-processing-directives)). Estos elementos básicos, solo los tokens son significativos en la gramática sintáctica de un programa de C# ([gramática sintáctica](lexical-structure.md#syntactic-grammar)).

El procesamiento léxico de un archivo de código fuente de C# consiste en reducir el archivo a una secuencia de tokens que se convierte en la entrada del análisis sintáctico. Los terminadores de línea, espacio en blanco y los comentarios que pueden servir para separar los tokens y las directivas de preprocesamiento pueden hacer secciones del archivo de origen se omite, pero estos elementos léxicos no tienen ningún impacto en la estructura sintáctica de un programa de C#.

En el caso de los literales de cadena interpolada ([interpoladas literales de cadena](lexical-structure.md#interpolated-string-literals)) análisis léxico genera inicialmente un token único, pero se divide en varios elementos de entrada que repetidamente se someten a análisis léxico hasta que se han resueltos todos los literales de cadena interpolada. Los tokens resultantes después puedan actuar como entrada para el análisis sintáctico.

Cuando varias producciones de gramática léxica coincide con una secuencia de caracteres en un archivo de origen, el procesamiento léxico siempre forma el elemento léxico más largo posible. Por ejemplo, la secuencia de caracteres `//` se procesa como el principio de una sola línea de comentario porque dicho elemento léxico tiene más de una sola `/` token.

### <a name="line-terminators"></a>Terminadores de línea

Los terminadores de línea dividen los caracteres de un archivo de código fuente de C# en líneas.

```antlr
new_line
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Carriage return character (U+000D) followed by line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;
```

Para herramientas de edición que agregan marcadores de fin del archivo de código de compatibilidad con el origen y para habilitar un origen de archivo para verse correctamente como una secuencia de líneas terminadas, las transformaciones siguientes se aplican en orden, para cada archivo de código fuente en un programa de C#:

*  Si el último carácter del archivo de origen es un carácter de Control a la Z (`U+001A`), se elimina este carácter.
*  Un carácter de retorno de carro (`U+000D`) se agrega al final del archivo de origen si el archivo de código fuente no está vacío y si el último carácter del archivo de origen no es un retorno de carro (`U+000D`), un avance de línea (`U+000A`), un separador de línea (`U+2028`), o un separador de párrafos (`U+2029`).

### <a name="comments"></a>Comentarios

Se admiten dos formas de comentarios: comentarios de una línea y comentarios delimitados. ***Comentarios de una línea*** empiezan con los caracteres `//` y extender al final de la línea de código fuente. ***Comentarios delimitados*** empiezan con los caracteres `/*` y terminan con los caracteres `*/`. Comentarios delimitados pueden abarcar varias líneas.

```antlr
comment
    : single_line_comment
    | delimited_comment
    ;

single_line_comment
    : '//' input_character*
    ;

input_character
    : '<Any Unicode character except a new_line_character>'
    ;

new_line_character
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;

delimited_comment
    : '/*' delimited_comment_section* asterisk* '/'
    ;

delimited_comment_section
    : '/'
    | asterisk* not_slash_or_asterisk
    ;

asterisk
    : '*'
    ;

not_slash_or_asterisk
    : '<Any Unicode character except / or *>'
    ;
```

Comentarios no pueden anidarse. Las secuencias de caracteres `/*` y `*/` no tienen ningún significado especial dentro de un `//` comentario y las secuencias de caracteres `//` y `/*` no tienen ningún significado especial dentro de un comentario delimitado.

Los comentarios no se procesan dentro de literales de carácter y cadena.

El ejemplo
```csharp
/* Hello, world program
   This program writes "hello, world" to the console
*/
class Hello
{
    static void Main() {
        System.Console.WriteLine("hello, world");
    }
}
```
incluye un comentario delimitado.

El ejemplo
```csharp
// Hello, world program
// This program writes "hello, world" to the console
//
class Hello // any name will do for this class
{
    static void Main() { // this method must be named "Main"
        System.Console.WriteLine("hello, world");
    }
}
```
muestra varios comentarios en una línea.

### <a name="white-space"></a>Espacio en blanco

Espacio en blanco se define como carácter de avance de cualquier carácter con la clase Unicode Zs (que incluye el carácter de espacio), así como el carácter de tabulación horizontal, el carácter de tabulación vertical y el formulario.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>tokens

Hay varios tipos de tokens: identificadores, palabras clave, literales, operadores y signos de puntuación. Espacio en blanco y los comentarios no son tokens, aunque actúan como separadores de los tokens.

```antlr
token
    : identifier
    | keyword
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | interpolated_string_literal
    | operator_or_punctuator
    ;
```

### <a name="unicode-character-escape-sequences"></a>Secuencias de escape de caracteres Unicode

Una secuencia de escape de caracteres Unicode representa un carácter Unicode. Las secuencias de escape de carácter Unicode se procesan en los identificadores ([identificadores](lexical-structure.md#identifiers)), literales de caracteres ([literales de caracteres](lexical-structure.md#character-literals)) y literales de cadena regulares ([literalesdecadena](lexical-structure.md#string-literals)). No se procesa un carácter de escape Unicode en cualquier otra ubicación (por ejemplo, para formar un operador, un signo de puntuación o una palabra clave).

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Una secuencia de escape Unicode representa el carácter Unicode único formado por el número hexadecimal que sigue el "`\u`"o"`\U`" caracteres. Puesto que C# usa una codificación de 16 bits de los puntos de código Unicode en caracteres y valores de cadena, un carácter Unicode en el intervalo de u+10000 a 10FFFF U + no está permitido en un literal de carácter y se representa mediante un par suplente Unicode en un literal de cadena. No se admiten caracteres Unicode con puntos de código por encima de 0x10FFFF.

No se realizan varias traducciones. Por ejemplo, la cadena literal "`\u005Cu005C`"es equivalente a"`\u005C`"en lugar de"`\`". El valor Unicode `\u005C` es el carácter "`\`".

El ejemplo
```csharp
class Class1
{
    static void Test(bool \u0066) {
        char c = '\u0066';
        if (\u0066)
            System.Console.WriteLine(c.ToString());
    }        
}
```
se muestran varios usos de `\u0066`, que es la secuencia de escape para la letra "`f`". El programa es equivalente a
```csharp
class Class1
{
    static void Test(bool f) {
        char c = 'f';
        if (f)
            System.Console.WriteLine(c.ToString());
    }        
}
```

### <a name="identifiers"></a>Identificadores

Las reglas para identificadores proporcionadas en esta sección se corresponden exactamente a las recomendaciones mediante el Unicode estándar anexo 31, excepto en que se permite el carácter de subrayado como carácter inicial (como era tradicional en el lenguaje de programación de C), son secuencias de escape Unicode se permite en identificadores y el "`@`" se permite el carácter como prefijo para habilitar las palabras clave que se usará como identificadores.

```antlr
identifier
    : available_identifier
    | '@' identifier_or_keyword
    ;

available_identifier
    : '<An identifier_or_keyword that is not a keyword>'
    ;

identifier_or_keyword
    : identifier_start_character identifier_part_character*
    ;

identifier_start_character
    : letter_character
    | '_'
    ;

identifier_part_character
    : letter_character
    | decimal_digit_character
    | connecting_character
    | combining_character
    | formatting_character
    ;

letter_character
    : '<A Unicode character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    | '<A unicode_escape_sequence representing a character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    ;

combining_character
    : '<A Unicode character of classes Mn or Mc>'
    | '<A unicode_escape_sequence representing a character of classes Mn or Mc>'
    ;

decimal_digit_character
    : '<A Unicode character of the class Nd>'
    | '<A unicode_escape_sequence representing a character of the class Nd>'
    ;

connecting_character
    : '<A Unicode character of the class Pc>'
    | '<A unicode_escape_sequence representing a character of the class Pc>'
    ;

formatting_character
    : '<A Unicode character of the class Cf>'
    | '<A unicode_escape_sequence representing a character of the class Cf>'
    ;
```

Para obtener información sobre las clases de caracteres Unicode que se mencionó anteriormente, vea el estándar Unicode, versión 3.0, sección 4.5.

Algunos ejemplos de identificadores válidos son "`identifier1`","`_identifier2`", y "`@if`".

Debe ser un identificador en un programa que cumplen las especificaciones en el formato canónico definido por la forma de normalización Unicode C, como se define en Unicode Standard anexo 15. El comportamiento al encontrar un identificador no está en forma de normalización C es definido por la implementación; Sin embargo, no es necesario un diagnóstico.

El prefijo "`@`" permite el uso de palabras clave como identificadores, lo que resulta útil cuando se interactúa con otros lenguajes de programación. El carácter `@` no es realmente parte del identificador, por lo que el identificador podría verse en otros lenguajes como un identificador normal, sin el prefijo. Un identificador con un `@` prefijo se denomina un ***identificador textual***. El uso de la `@` prefijo para los identificadores que no son palabras clave está permitido, pero no se recomienda como una cuestión de estilo.

El ejemplo:
```csharp
class @class
{
    public static void @static(bool @bool) {
        if (@bool)
            System.Console.WriteLine("true");
        else
            System.Console.WriteLine("false");
    }    
}

class Class1
{
    static void M() {
        cl\u0061ss.st\u0061tic(true);
    }
}
```
define una clase denominada "`class`"con un método estático denominado"`static`"que toma un parámetro denominado"`bool`". Tenga en cuenta que puesto que establece secuencias de escape Unicode no se permiten en las palabras clave, el token "`cl\u0061ss`"es un identificador, y es el mismo identificador que"`@class`".

Dos identificadores se consideran iguales si son idénticos después de aplicarán las transformaciones siguientes, en orden:

*  El prefijo "`@`", si se usa, se quita.
*  Cada *unicode_escape_sequence* se transforma en su carácter Unicode correspondiente.
*  Cualquier *formatting_character*s se quitan.

Los identificadores que contienen dos consecutivos los caracteres de subrayado (`U+005F`) están reservados para su uso por la implementación. Por ejemplo, una implementación podría proporcionar palabras clave extendidas que comienzan por dos caracteres de subrayado.

### <a name="keywords"></a>Palabras clave

Un ***palabra clave*** es una secuencia similar a identificador de caracteres que está reservado y no se puede usar como un identificador excepto cuando va precedido por el `@` caracteres.

```antlr
keyword
    : 'abstract' | 'as'       | 'base'       | 'bool'      | 'break'
    | 'byte'     | 'case'     | 'catch'      | 'char'      | 'checked'
    | 'class'    | 'const'    | 'continue'   | 'decimal'   | 'default'
    | 'delegate' | 'do'       | 'double'     | 'else'      | 'enum'
    | 'event'    | 'explicit' | 'extern'     | 'false'     | 'finally'
    | 'fixed'    | 'float'    | 'for'        | 'foreach'   | 'goto'
    | 'if'       | 'implicit' | 'in'         | 'int'       | 'interface'
    | 'internal' | 'is'       | 'lock'       | 'long'      | 'namespace'
    | 'new'      | 'null'     | 'object'     | 'operator'  | 'out'
    | 'override' | 'params'   | 'private'    | 'protected' | 'public'
    | 'readonly' | 'ref'      | 'return'     | 'sbyte'     | 'sealed'
    | 'short'    | 'sizeof'   | 'stackalloc' | 'static'    | 'string'
    | 'struct'   | 'switch'   | 'this'       | 'throw'     | 'true'
    | 'try'      | 'typeof'   | 'uint'       | 'ulong'     | 'unchecked'
    | 'unsafe'   | 'ushort'   | 'using'      | 'virtual'   | 'void'
    | 'volatile' | 'while'
    ;
```

En algunos lugares de la gramática, identificadores específicos tienen un significado especial, pero no son palabras clave. Estos identificadores se denominan a veces "palabras clave contextuales". Por ejemplo, dentro de una declaración de propiedad, el "`get`"y"`set`" identificadores tienen un significado especial ([descriptores de acceso](classes.md#accessors)). Un identificador distinto de `get` o `set` no está permitido en estas ubicaciones, por lo que este uso no entra en conflicto con un uso de estas palabras como identificadores. En otros casos, al igual que con el identificador "`var`" en declaraciones de variables locales con tipo implícito ([declaraciones de variable Local](statements.md#local-variable-declarations)), una palabra clave contextual puede entrar en conflicto con los nombres declarados. En tales casos, el nombre declarado prevalece sobre el uso del identificador como una palabra clave contextual.

### <a name="literals"></a>Literales

Un ***literal*** es una representación de código fuente de un valor.

```antlr
literal
    : boolean_literal
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | null_literal
    ;
```

#### <a name="boolean-literals"></a>Literales booleanos

Hay dos valores literales booleanos: `true` y `false`.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

El tipo de un *boolean_literal* es `bool`.

#### <a name="integer-literals"></a>Literales enteros

Los literales enteros se usan para escribir los valores de tipos `int`, `uint`, `long`, y `ulong`. Los literales enteros tienen dos formatos posibles: decimal y hexadecimal.

```antlr
integer_literal
    : decimal_integer_literal
    | hexadecimal_integer_literal
    ;

decimal_integer_literal
    : decimal_digit+ integer_type_suffix?
    ;

decimal_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

integer_type_suffix
    : 'U' | 'u' | 'L' | 'l' | 'UL' | 'Ul' | 'uL' | 'ul' | 'LU' | 'Lu' | 'lU' | 'lu'
    ;

hexadecimal_integer_literal
    : '0x' hex_digit+ integer_type_suffix?
    | '0X' hex_digit+ integer_type_suffix?
    ;

hex_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'a' | 'b' | 'c' | 'd' | 'e' | 'f';
```

El tipo de un literal entero se determina como sigue:

*  Si el literal no tiene sufijo, es el primero de estos tipos en el que se puede representar su valor: `int`, `uint`, `long`, `ulong`.
*  Si el sufijo literal `U` o `u`, es el primero de estos tipos en el que se puede representar su valor: `uint`, `ulong`.
*  Si el sufijo literal `L` o `l`, es el primero de estos tipos en el que se puede representar su valor: `long`, `ulong`.
*  Si el sufijo literal `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, o `lu`, es de tipo `ulong`.

Si el valor representado por un literal entero está fuera del intervalo de la `ulong` escribe, se produce un error de tiempo de compilación.

Como una cuestión de estilo, se recomienda que "`L`"se puede usar en lugar de"`l`" al escribir literales de tipo `long`, ya que es fácil confundir la letra "`l`"con el dígito"`1`".

Para permitir el más pequeño posible `int` y `long` valores a escribirse como literales de entero decimal, existen las dos reglas siguientes:

* Cuando un *decimal_integer_literal* con el valor 2147483648 (2 ^ 31) y no *integer_type_suffix* aparece como el token inmediatamente después de un símbolo de operador unario menos ([unario menos operador](expressions.md#unary-minus-operator)), el resultado es una constante de tipo `int` con el valor entre -2147483648 (-2 ^ 31). En el resto de situaciones, este tipo una *decimal_integer_literal* es de tipo `uint`.
* Cuando un *decimal_integer_literal* con el valor 9223372036854775808 (2 ^ 63) y no *integer_type_suffix* o *integer_type_suffix* `L` o `l` aparece como el token inmediatamente después de un símbolo de operador unario menos ([operador unario menos](expressions.md#unary-minus-operator)), el resultado es una constante de tipo `long` con el valor de -9223372036854775808 (-2 ^ 63). En el resto de situaciones, este tipo una *decimal_integer_literal* es de tipo `ulong`.

#### <a name="real-literals"></a>Literales reales

Los literales reales se usan para escribir los valores de tipos `float`, `double`, y `decimal`.

```antlr
real_literal
    : decimal_digit+ '.' decimal_digit+ exponent_part? real_type_suffix?
    | '.' decimal_digit+ exponent_part? real_type_suffix?
    | decimal_digit+ exponent_part real_type_suffix?
    | decimal_digit+ real_type_suffix
    ;

exponent_part
    : 'e' sign? decimal_digit+
    | 'E' sign? decimal_digit+
    ;

sign
    : '+'
    | '-'
    ;

real_type_suffix
    : 'F' | 'f' | 'D' | 'd' | 'M' | 'm'
    ;
```

Si no hay ningún *real_type_suffix* se especifica, el tipo del literal real es `double`. En caso contrario, el sufijo de tipo real determina el tipo del literal real, como sigue:

*  Un literal real con el sufijo `F` o `f` es de tipo `float`. Por ejemplo, los literales `1f`, `1.5f`, `1e10f`, y `123.456F` son todas del tipo `float`.
*  Un literal real con el sufijo `D` o `d` es de tipo `double`. Por ejemplo, los literales `1d`, `1.5d`, `1e10d`, y `123.456D` son todas del tipo `double`.
*  Un literal real con el sufijo `M` o `m` es de tipo `decimal`. Por ejemplo, los literales `1m`, `1.5m`, `1e10m`, y `123.456M` son todas del tipo `decimal`. Este literal se convierte en un `decimal` valor tomando el valor exacto y, si es necesario, redondeando al valor que se puede representar más cercano mediante el redondeo bancario ([tipo decimal](types.md#the-decimal-type)). Cualquier escala evidente en el literal se conserva a menos que se redondea el valor o el valor es cero (en este caso, el inicio de sesión y la escala será 0). Por lo tanto, el literal `2.900m` se analizará para formar el decimal con signo `0`, coeficiente `2900`y la escala `3`.

Si el literal especificado no se puede representar en el tipo indicado, se produce un error en tiempo de compilación.

El valor de un literal de tipo real `float` o `double` se determina mediante el uso de IEEE modo "redondeo al más cercano".

Tenga en cuenta que en un literal real, siempre son necesarios dígitos decimales después del separador decimal. Por ejemplo, `1.3F` es un literal real pero `1.F` no es.

#### <a name="character-literals"></a>Literales de carácter

Un literal de carácter representa un único carácter y suele estar compuesto por un carácter de comillas, como en `'a'`.

Nota: La notación gramatical ANTLR hace lo siguiente confuso! En ANTLR, al escribir `\'` significa una comilla `'`. Y al escribir `\\` significa una sola barra diagonal inversa `\`. Por lo tanto, la primera regla para un literal de carácter significa que comienza con una comilla simple, un carácter y, luego, una comilla simple. Y las secuencias de escape sencillas posible once son `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.

```antlr
character_literal
    : '\'' character '\''
    ;

character
    : single_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_character
    : '<Any character except \' (U+0027), \\ (U+005C), and new_line_character>'
    ;

simple_escape_sequence
    : '\\\'' | '\\"' | '\\\\' | '\\0' | '\\a' | '\\b' | '\\f' | '\\n' | '\\r' | '\\t' | '\\v'
    ;

hexadecimal_escape_sequence
    : '\\x' hex_digit hex_digit? hex_digit? hex_digit?;
```

Un carácter que sigue a un carácter de barra diagonal inversa (`\`) en un *carácter* debe ser uno de los siguientes caracteres: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. De lo contrario, se produce un error en tiempo de compilación.

Una secuencia de escape hexadecimal representa un único carácter Unicode, con el valor está formado por el siguiente número hexadecimal "`\x`".

Si es mayor que el valor representado por un literal de carácter `U+FFFF`, se produce un error de tiempo de compilación.

Una secuencia de escape de caracteres Unicode ([secuencias de escape de caracteres Unicode](lexical-structure.md#unicode-character-escape-sequences)) en un literal de caracteres debe estar en el intervalo `U+0000` a `U+FFFF`.

Una secuencia de escape simples representa una codificación de caracteres Unicode, como se describe en la tabla siguiente.


| __Secuencia de escape__ | __Nombre de carácter__ | __Codificación Unicode__ |
|---------------------|--------------------|----------------------|
| `\'`                | Comilla simple       | `0x0027`             | 
| `\"`                | Comilla doble       | `0x0022`             | 
| `\\`                | Barra diagonal inversa          | `0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | Alerta              | `0x0007`             | 
| `\b`                | Retroceso          | `0x0008`             | 
| `\f`                | Avance de página          | `0x000C`             | 
| `\n`                | Nueva línea           | `0x000A`             | 
| `\r`                | Retorno de carro    | `0x000D`             | 
| `\t`                | Tabulación horizontal     | `0x0009`             | 
| `\v`                | Tabulación vertical       | `0x000B`             | 

El tipo de un *character_literal* es `char`.

#### <a name="string-literals"></a>Literales de cadena

C# admite dos formatos de literales de cadena: ***literales de cadena regulares*** y ***literales de cadena textual***.

Un literal de cadena regular consta de cero o más caracteres entre comillas dobles, como en `"hello"`y pueden incluir ambas secuencias de escape sencillas (como `\t` para el carácter de tabulación), hexadecimales y secuencias de escape Unicode.

Un literal de cadena textual consta de un `@` carácter seguido de un carácter de comillas dobles, cero o más caracteres y un carácter de comillas dobles de cierre. Un ejemplo sencillo es `@"hello"`. En un literal de cadena textual, los caracteres entre los delimitadores se interpretan literalmente, la única excepción que se va a un *quote_escape_sequence*. En concreto, las secuencias de escape simples, hexadecimales y secuencias de escape Unicode no se procesan en literales de cadena textual. Un literal de cadena textual puede abarcar varias líneas.

```antlr
string_literal
    : regular_string_literal
    | verbatim_string_literal
    ;

regular_string_literal
    : '"' regular_string_literal_character* '"'
    ;

regular_string_literal_character
    : single_regular_string_literal_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_regular_string_literal_character
    : '<Any character except " (U+0022), \\ (U+005C), and new_line_character>'
    ;

verbatim_string_literal
    : '@"' verbatim_string_literal_character* '"'
    ;

verbatim_string_literal_character
    : single_verbatim_string_literal_character
    | quote_escape_sequence
    ;

single_verbatim_string_literal_character
    : '<any character except ">'
    ;

quote_escape_sequence
    : '""'
    ;
```

Un carácter que sigue a un carácter de barra diagonal inversa (`\`) en un *regular_string_literal_character* debe ser uno de los siguientes caracteres: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. De lo contrario, se produce un error en tiempo de compilación.

El ejemplo
```csharp
string a = "hello, world";                   // hello, world
string b = @"hello, world";                  // hello, world

string c = "hello \t world";                 // hello      world
string d = @"hello \t world";                // hello \t world

string e = "Joe said \"Hello\" to me";       // Joe said "Hello" to me
string f = @"Joe said ""Hello"" to me";      // Joe said "Hello" to me

string g = "\\\\server\\share\\file.txt";    // \\server\share\file.txt
string h = @"\\server\share\file.txt";       // \\server\share\file.txt

string i = "one\r\ntwo\r\nthree";
string j = @"one
two
three";
```
muestra una variedad de literales de cadena. La última cadena literal, `j`, es un literal de cadena textual que abarca varias líneas. Los caracteres entre comillas, incluidos espacios en blanco como caracteres de nueva línea, se conservan literalmente.

Puesto que una secuencia de escape hexadecimal puede tener un número variable de dígitos hexadecimales, el literal de cadena `"\x123"` contiene un único carácter con el valor hexadecimal 123. Para crear una cadena que contiene el carácter con el valor hexadecimal 12 seguido del carácter 3, podría escribirse `"\x00123"` o `"\x12" + "3"` en su lugar.

El tipo de un *string_literal* es `string`.

Cada literal de cadena no produce necesariamente en una nueva instancia de cadena. Cuando dos o más literales de cadena que son equivalentes según el operador de igualdad de cadenas ([operadores de igualdad de cadenas](expressions.md#string-equality-operators)) aparecen en el mismo programa, estos literales de cadena hacen referencia a la misma instancia de cadena. Por ejemplo, el resultado producido por
```csharp
class Test
{
    static void Main() {
        object a = "hello";
        object b = "hello";
        System.Console.WriteLine(a == b);
    }
}
```
es `True` porque los dos literales hacen referencia a la misma instancia de cadena.

#### <a name="interpolated-string-literals"></a>Literales de cadena interpolada

Literales de cadena interpolada son similares a los literales de cadena, pero contener orificios delimitadas por `{` y `}`, en la que pueden producirse las expresiones. En tiempo de ejecución, las expresiones se evalúan con el objetivo de tener sus formularios textuales sustituye en la cadena en el lugar donde se produce el orificio. La sintaxis y semántica de interpolación de cadenas que se describe en la sección ([cadenas interpoladas](expressions.md#interpolated-strings)).

Como literales de cadena, los literales de cadena interpolada pueden ser normal o textual. Literales de cadena regulares interpolada se delimitan mediante `$"` y `"`, y los literales de cadena interpolada textual se delimitan mediante `$@"` y `"`.

Al igual que otros literales, análisis léxico de un literal de cadena interpolada inicialmente da como resultado un token único, según la siguiente gramática. Sin embargo, antes de análisis sintáctico, el token único de un literal de cadena interpolada se divide en varios tokens para las partes de la cadena envolvente los huecos y los elementos de entrada que se producen en los orificios léxicamente se analizaron nuevo. A su vez, esto puede producir más los literales de cadena interpolada para procesarse, pero, si léxicamente corregir, finalmente, dará lugar a una secuencia de tokens para el análisis sintáctico procesar.

```antlr
interpolated_string_literal
    : '$' interpolated_regular_string_literal
    | '$' interpolated_verbatim_string_literal
    ;

interpolated_regular_string_literal
    : interpolated_regular_string_whole
    | interpolated_regular_string_start  interpolated_regular_string_literal_body interpolated_regular_string_end
    ;

interpolated_regular_string_literal_body
    : regular_balanced_text
    | interpolated_regular_string_literal_body interpolated_regular_string_mid regular_balanced_text
    ;

interpolated_regular_string_whole
    : '"' interpolated_regular_string_character* '"'
    ;

interpolated_regular_string_start
    : '"' interpolated_regular_string_character* '{'
    ;

interpolated_regular_string_mid
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '{'
    ;

interpolated_regular_string_end
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '"'
    ;

interpolated_regular_string_characters_after_brace
    : interpolated_regular_string_character_no_brace
    | interpolated_regular_string_characters_after_brace interpolated_regular_string_character
    ;

interpolated_regular_string_character
    : single_interpolated_regular_string_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;

interpolated_regular_string_character_no_brace
    : '<Any interpolated_regular_string_character except close_brace_escape_sequence and any hexadecimal_escape_sequence or unicode_escape_sequence designating } (U+007D)>'
    ;

single_interpolated_regular_string_character
    : '<Any character except \" (U+0022), \\ (U+005C), { (U+007B), } (U+007D), and new_line_character>'
    ;

open_brace_escape_sequence
    : '{{'
    ;

    close_brace_escape_sequence
    : '}}'
    ;
    
regular_balanced_text
    : regular_balanced_text_part+
    ;

regular_balanced_text_part
    : single_regular_balanced_text_character
    | delimited_comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' regular_balanced_text ')'
    | '[' regular_balanced_text ']'
    | '{' regular_balanced_text '}'
    ;
    
single_regular_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B), } (U+007D) and new_line_character>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
    
interpolation_format
    : interpolation_format_character+
    ;
    
interpolation_format_character
    : '<Any character except \" (U+0022), : (U+003A), { (U+007B) and } (U+007D)>'
    ;
    
interpolated_verbatim_string_literal
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_literal_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_literal_body
    : verbatim_balanced_text
    | interpolated_verbatim_string_literal_body interpolated_verbatim_string_mid verbatim_balanced_text
    ;
    
interpolated_verbatim_string_whole
    : '@"' interpolated_verbatim_string_character* '"'
    ;
    
interpolated_verbatim_string_start
    : '@"' interpolated_verbatim_string_character* '{'
    ;
    
interpolated_verbatim_string_mid
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '{'
    ;
    
interpolated_verbatim_string_end
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '"'
    ;
    
interpolated_verbatim_string_characters_after_brace
    : interpolated_verbatim_string_character_no_brace
    | interpolated_verbatim_string_characters_after_brace interpolated_verbatim_string_character
    ;
    
interpolated_verbatim_string_character
    : single_interpolated_verbatim_string_character
    | quote_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;
    
interpolated_verbatim_string_character_no_brace
    : '<Any interpolated_verbatim_string_character except close_brace_escape_sequence>'
    ;
    
single_interpolated_verbatim_string_character
    : '<Any character except \" (U+0022), { (U+007B) and } (U+007D)>'
    ;
    
verbatim_balanced_text
    : verbatim_balanced_text_part+
    ;

verbatim_balanced_text_part
    : single_verbatim_balanced_text_character
    | comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' verbatim_balanced_text ')'
    | '[' verbatim_balanced_text ']'
    | '{' verbatim_balanced_text '}'
    ;
    
single_verbatim_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B) and } (U+007D)>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
```

Un *interpolated_string_literal* token se reinterprete como varios tokens y otros elementos de entrada como se indica a continuación, en orden de aparición en el *interpolated_string_literal*:

* Las apariciones de los siguientes valores se reinterpretan como tokens individuales independientes: el interlineado `$` inicio de sesión, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*,  *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* y *interpolated_verbatim_string_end*.
* Las apariciones de *regular_balanced_text* y *verbatim_balanced_text* entre estos se vuelven a procesar como un *input_section* ([análisis léxico ](lexical-structure.md#lexical-analysis)) y se reinterpretan como la secuencia de elementos de entrada resultante. A su vez, estos pueden incluir tokens literales de cadena interpolada se reinterprete.

Análisis sintáctico se vuelven a combinar los tokens en un *interpolated_string_expression* ([cadenas interpoladas](expressions.md#interpolated-strings)).

Ejemplos de tareas pendientes


#### <a name="the-null-literal"></a>El literal null

```antlr
null_literal
    : 'null'
    ;
```

El *null_literal* puede convertirse implícitamente a un tipo de referencia o tipo que acepta valores NULL.

### <a name="operators-and-punctuators"></a>Operadores y signos de puntuación

Hay varios tipos de operadores y signos de puntuación. Los operadores se utilizan en expresiones para describir las operaciones que implican a uno o más operandos. Por ejemplo, la expresión `a + b` usa el `+` operador para agregar los dos operandos `a` y `b`. Signos de puntuación sirven para agrupar y separar.

```antlr
operator_or_punctuator
    : '{'  | '}'  | '['  | ']'  | '('   | ')'  | '.'  | ','  | ':'  | ';'
    | '+'  | '-'  | '*'  | '/'  | '%'   | '&'  | '|'  | '^'  | '!'  | '~'
    | '='  | '<'  | '>'  | '?'  | '??'  | '::' | '++' | '--' | '&&' | '||'
    | '->' | '==' | '!=' | '<=' | '>='  | '+=' | '-=' | '*=' | '/=' | '%='
    | '&=' | '|=' | '^=' | '<<' | '<<=' | '=>'
    ;

right_shift
    : '>>'
    ;

right_shift_assignment
    : '>>='
    ;
```

La barra vertical en el *right_shift* y *right_shift_assignment* producciones se utilizan para indicar que, a diferencia de otras producciones en la gramática sintáctica, no hay ningún tipo de carácter (ni siquiera espacio en blanco) se permiten entre los tokens. Se tratan estos producciones especialmente con el fin de habilitar la administración correcta de *type_parameter_list*s ([parámetros de tipo](classes.md#type-parameters)).

## <a name="pre-processing-directives"></a>Las directivas de preprocesamiento

Las directivas de preprocesamiento proporcionan la capacidad de omitir condicionalmente secciones de archivos de origen, para informar del error y las condiciones de advertencia y distinguir las distintas regiones de código fuente. El término "directivas de preprocesamiento" se usa solo para mantener la coherencia con los lenguajes de programación C y C++. En C#, no hay ningún paso de preprocesamiento independiente; las directivas de preprocesamiento se procesan como parte de la fase de análisis léxico.

```antlr
pp_directive
    : pp_declaration
    | pp_conditional
    | pp_line
    | pp_diagnostic
    | pp_region
    | pp_pragma
    ;
```

Están disponibles las siguientes directivas de preprocesamiento:

*  `#define` y `#undef`, que se usan para definir y anular, respectivamente, símbolos de compilación condicional ([directivas de declaraciones](lexical-structure.md#declaration-directives)).
*  `#if`, `#elif`, `#else`, y `#endif`, que se usan para omitir condicionalmente secciones de código fuente ([directivas de compilación condicional](lexical-structure.md#conditional-compilation-directives)).
*  `#line`, que se usa para controlar los números de línea de errores y advertencias ([línea directivas](lexical-structure.md#line-directives)).
*  `#error` y `#warning`, que se usan para emitir errores y advertencias, respectivamente ([directivas de diagnóstico](lexical-structure.md#diagnostic-directives)).
*  `#region` y `#endregion`, que se usan para marcar explícitamente las secciones de código fuente ([las directivas de región](lexical-structure.md#region-directives)).
*  `#pragma`, que se usa para especificar información contextual adicional para el compilador ([directivas Pragma](lexical-structure.md#pragma-directives)).

Una directiva de preprocesamiento siempre ocupa una línea de código fuente independiente y siempre comienza con un `#` carácter y un nombre de directiva de procesamiento previo. Puede haber un espacio en blanco antes de la `#` carácter y entre el `#` carácter y el nombre de la directiva.

Una línea de código fuente que contiene un `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, o `#endregion` directiva puede terminar con una sola línea de comentario. Comentarios delimitados (el `/* */` estilo de comentarios) no se permiten en las líneas de código fuente que contiene las directivas de preprocesamiento.

Las directivas de preprocesamiento no son tokens y no forman parte de la gramática sintáctica de C#. Sin embargo, las directivas de preprocesamiento pueden usarse para incluir o excluir las secuencias de tokens y pueden afectar el significado de un programa de C# de esa manera. Por ejemplo, cuando se compila, el programa:
```csharp
#define A
#undef B

class C
{
#if A
    void F() {}
#else
    void G() {}
#endif

#if B
    void H() {}
#else
    void I() {}
#endif
}
```
resultados en la misma secuencia de tokens que el programa:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

Por lo tanto, mientras que léxicamente, los dos programas son bastante diferentes, sintácticamente, son idénticos.

### <a name="conditional-compilation-symbols"></a>Símbolos de compilación condicional

La funcionalidad de compilación condicional proporcionada por el `#if`, `#elif`, `#else`, y `#endif` directivas se controla a través de las expresiones de preprocesamiento ([las expresiones de preprocesamiento](lexical-structure.md#pre-processing-expressions)) y símbolos de compilación condicional.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Símbolo de compilación condicional tiene dos estados posibles: ***definido*** o ***indefinido***. Al principio del procesamiento léxico de un archivo de código fuente, un símbolo de compilación condicional está definido a menos que se ha definido explícitamente mediante un mecanismo externo (por ejemplo, una opción del compilador de línea de comandos). Cuando un `#define` se procesa la directiva, el símbolo de compilación condicional nombrado en la directiva queda definido en el archivo de código fuente. El símbolo permanece definido hasta un `#undef` la directiva para que se procesa el mismo símbolo, o hasta que se alcanza el final del archivo de origen. Una implicación de esto es que `#define` y `#undef` directivas en un archivo de origen no tienen ningún efecto en otros archivos de origen en el mismo programa.

Cuando se hace referencia en una expresión de preprocesamiento, un símbolo de compilación condicional definido tiene el valor booleano `true`, y un símbolo de compilación condicional no definido tiene el valor booleano `false`. No es necesario que los símbolos de compilación condicional se declaren explícitamente antes de que se hace referencia en las expresiones de preprocesamiento. En su lugar, los símbolos no declarados simplemente no se definen y, por tanto, tienen el valor `false`.

El espacio de nombres de símbolos de compilación condicional es distinto e independiente de todas las entidades con nombre en un programa de C#. Solo se pueden hacer referencia a los símbolos de compilación condicional en `#define` y `#undef` directivas y en las expresiones de preprocesamiento.

### <a name="pre-processing-expressions"></a>Las expresiones de preprocesamiento

Las expresiones de preprocesamiento pueden producirse en `#if` y `#elif` directivas. Los operadores `!`, `==`, `!=`, `&&` y `||` se permiten en expresiones, el preprocesamiento y se pueden usar paréntesis para agrupar.

```antlr
pp_expression
    : whitespace? pp_or_expression whitespace?
    ;

pp_or_expression
    : pp_and_expression
    | pp_or_expression whitespace? '||' whitespace? pp_and_expression
    ;

pp_and_expression
    : pp_equality_expression
    | pp_and_expression whitespace? '&&' whitespace? pp_equality_expression
    ;

pp_equality_expression
    : pp_unary_expression
    | pp_equality_expression whitespace? '==' whitespace? pp_unary_expression
    | pp_equality_expression whitespace? '!=' whitespace? pp_unary_expression
    ;

pp_unary_expression
    : pp_primary_expression
    | '!' whitespace? pp_unary_expression
    ;

pp_primary_expression
    : 'true'
    | 'false'
    | conditional_symbol
    | '(' whitespace? pp_expression whitespace? ')'
    ;
```

Cuando se hace referencia en una expresión de preprocesamiento, un símbolo de compilación condicional definido tiene el valor booleano `true`, y un símbolo de compilación condicional no definido tiene el valor booleano `false`.

Evaluación de una expresión de preprocesamiento siempre produce un valor booleano. Las reglas de evaluación de una expresión de preprocesamiento son las mismas que para una expresión constante ([expresiones constantes](expressions.md#constant-expressions)), excepto en que las únicas entidades definidas por el usuario que pueden hacer referencia a los símbolos de compilación condicional .

### <a name="declaration-directives"></a>Directivas de declaraciones

Las directivas de declaración se usan para definir o anular la definición de símbolos de compilación condicional.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

El procesamiento de un `#define` directiva hace que la definición, del símbolo de compilación condicional dado a partir de la línea de código fuente que sigue a la directiva. Del mismo modo, el procesamiento de un `#undef` directiva hace que el símbolo de compilación condicional dado que quede sin definir, a partir de la línea de código fuente que sigue a la directiva.

Cualquier `#define` y `#undef` directivas en un archivo de origen deben aparecer antes del primer *token* ([Tokens](lexical-structure.md#tokens)) en el archivo de origen; en caso contrario, tiempo de compilación se produce un error. En términos intuitivos, `#define` y `#undef` directivas deben preceder a cualquier "código real" en el archivo de origen.

El ejemplo:
```csharp
#define Enterprise

#if Professional || Enterprise
    #define Advanced
#endif

namespace Megacorp.Data
{
    #if Advanced
    class PivotTable {...}
    #endif
}
```
es válido porque la `#define` directivas preceden el primer token (la `namespace` palabra clave) en el archivo de origen.

En el ejemplo siguiente, se produce un error en tiempo de compilación porque un `#define` sigue el código real:
```csharp
#define A
namespace N
{
    #define B
    #if B
    class Class1 {}
    #endif
}
```

Un `#define` puede definir un símbolo de compilación condicional que ya esté definido, sin necesidad de ninguna intervención de que `#undef` para dicho símbolo. El ejemplo siguiente define un símbolo de compilación condicional `A` y, a continuación, define.
```csharp
#define A
#define A
```

Un `#undef` puede "Anular" un símbolo de compilación condicional no está definido. El ejemplo siguiente define un símbolo de compilación condicional `A` y, a continuación, eliminar dicha definición dos veces; aunque la segunda `#undef` no tiene ningún efecto, aún es válido.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Directivas de compilación condicional

Se usan las directivas de compilación condicional permiten incluir o excluir partes de un archivo de origen.

```antlr
pp_conditional
    : pp_if_section pp_elif_section* pp_else_section? pp_endif
    ;

pp_if_section
    : whitespace? '#' whitespace? 'if' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_elif_section
    : whitespace? '#' whitespace? 'elif' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_else_section:
    | whitespace? '#' whitespace? 'else' pp_new_line conditional_section?
    ;

pp_endif
    : whitespace? '#' whitespace? 'endif' pp_new_line
    ;

conditional_section
    : input_section
    | skipped_section
    ;

skipped_section
    : skipped_section_part+
    ;

skipped_section_part
    : skipped_characters? new_line
    | pp_directive
    ;

skipped_characters
    : whitespace? not_number_sign input_character*
    ;

not_number_sign
    : '<Any input_character except #>'
    ;
```

Tal y como indica la sintaxis, las directivas de compilación condicional deben escribirse como conjuntos que consta de, en orden, una `#if` directiva, cero o más `#elif` directivas, cero o uno `#else` directiva y un `#endif` directiva. Entre las directivas son secciones condicionales de código fuente. Cada sección se controla mediante la directiva inmediatamente anterior. Una sección condicional puede contener directivas de compilación condicional anidadas proporcionado estas directivas forman conjuntos completos.

Un *pp_conditional* selecciona al menos uno de los contenidos *conditional_section*s para el procesamiento léxico normal:

*  El *pp_expression*s de la `#if` y `#elif` directivas se evalúan en orden hasta que uno da como resultado `true`. Si el resultado de una expresión `true`, *conditional_section* está seleccionado de la directiva correspondiente.
*  Si todos los *pp_expression*yield s `false`y si un `#else` directiva está presente, el *conditional_section* de la `#else` directiva está seleccionada.
*  En caso contrario, no *conditional_section* está seleccionada.

Seleccionado *conditional_section*, si existe, se procesa como normal *input_section*: código fuente incluido en la sección debe adherirse a la gramática léxica; los tokens se generan a partir del origen código en la sección. y las directivas de preprocesamiento en la sección tienen los efectos prescritos.

Los restantes *conditional_section*s, si hay alguno, se procesan como *skipped_section*s: excepto para las directivas de preprocesamiento, el código fuente en la sección no necesita cumplir el léxico gramática; no los tokens se generan desde el código fuente en la sección. y las directivas de preprocesamiento en la sección deben ser léxicamente correctas, pero no se procesan en caso contrario. Dentro de un *conditional_section* que se está procesando como una *skipped_section*cualquier anidado *conditional_section*s (contenidos en anidado `#if`... `#endif` y `#region`... `#endregion` construye) también se procesan como *skipped_section*s.

El ejemplo siguiente muestra las compilación condicional cómo pueden anidarse directivas:
```csharp
#define Debug       // Debugging on
#undef Trace        // Tracing off

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
            #if Trace
                WriteToLog(this.ToString());
            #endif
        #endif
        CommitHelper();
    }
}
```

Excepto para las directivas de preprocesamiento, el código fuente omitidos no está sujeto a análisis léxico. Por ejemplo, la siguiente es válida, a pesar del comentario sin terminar en la `#else` sección:
```csharp
#define Debug        // Debugging on

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
        #else
            /* Do something else
        #endif
    }
}
```

Sin embargo, tenga en cuenta que las directivas de preprocesamiento se necesitan para ser léxicamente correcta incluso en las secciones omitidas del código fuente.

Las directivas de preprocesamiento no se procesan cuando aparecen dentro de los elementos de entrada multilínea. Por ejemplo, el programa:
```csharp
class Hello
{
    static void Main() {
        System.Console.WriteLine(@"hello, 
#if Debug
        world
#else
        Nebraska
#endif
        ");
    }
}
```
resultados en la salida:
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

En casos concretos, el conjunto de directivas de preprocesamiento que se procesa puede depender de la evaluación de la *pp_expression*. El ejemplo:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
siempre produce la misma secuencia de símbolos (`class` `Q` `{` `}`), independientemente de si `X` está definido. Si `X` está definido, las directivas que se procesan sola son `#if` y `#endif`, debido a que el comentario de varias líneas. Si `X` es undefined, a continuación, tres directivas (`#if`, `#else`, `#endif`) forman parte del conjunto de directivas.

### <a name="diagnostic-directives"></a>Directivas de diagnóstico

Las directivas de diagnóstico se usan para generar mensajes de error y advertencia que se muestran en la misma manera que otros errores de tiempo de compilación y advertencias de forma explícita.

```antlr
pp_diagnostic
    : whitespace? '#' whitespace? 'error' pp_message
    | whitespace? '#' whitespace? 'warning' pp_message
    ;

pp_message
    : new_line
    | whitespace input_character* new_line
    ;
```

El ejemplo:
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
siempre genera una advertencia ("revisión de código necesario antes de protegerlo") y produce un error en tiempo de compilación ("una compilación no puede ser comercial y depuración") si los símbolos condicionales `Debug` y `Retail` se definen. Tenga en cuenta que un *pp_message* puede contener texto arbitrario; en concreto, no necesita contener tokens con formato correcto, como se muestra en las comillas simples en la palabra `can't`.

### <a name="region-directives"></a>Directivas de región

Las directivas de región se usan para marcar explícitamente las regiones de código fuente.

```antlr
pp_region
    : pp_start_region conditional_section? pp_end_region
    ;

pp_start_region
    : whitespace? '#' whitespace? 'region' pp_message
    ;

pp_end_region
    : whitespace? '#' whitespace? 'endregion' pp_message
    ;
```

No se adjunta ningún significado semántico a una región; las regiones están pensadas para su uso por el programador o mediante herramientas automatizadas para marcar una sección de código fuente. El mensaje especificado en un `#region` o `#endregion` directiva del mismo modo no tiene ningún significado semántico; sencillamente, se usa para identificar la región. Coincidencia `#region` y `#endregion` directivas pueden tener diferentes *pp_message*s.

El procesamiento léxico de una región:
```csharp
#region
...
#endregion
```
corresponde exactamente con el procesamiento léxico de una directiva de compilación condicional del formulario:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Directivas de línea

Las directivas de línea pueden usarse para modificar los números de línea y nombres de archivo de origen que son notificados por el compilador en la salida como advertencias y errores, y que se usan los atributos de información de llamador ([atributos de información del llamador](attributes.md#caller-info-attributes)).

Las directivas de línea se usan con más frecuencia en herramientas de metaprogramación que generan código fuente C# a partir de otras entradas de texto.

```antlr
pp_line
    : whitespace? '#' whitespace? 'line' whitespace line_indicator pp_new_line
    ;

line_indicator
    : decimal_digit+ whitespace file_name
    | decimal_digit+
    | 'default'
    | 'hidden'
    ;

file_name
    : '"' file_name_character+ '"'
    ;

file_name_character
    : '<Any input_character except ">'
    ;
```

Cuando no hay ninguna `#line` directivas están presentes, el compilador muestra números de línea y nombres de archivo de origen en su salida. Al procesar un `#line` directiva que incluye un *line_indicator* que no es `default`, el compilador trata la línea después de la directiva como si tuviera el número de línea especificado (y el nombre de archivo, si se especifica).

Un `#line default` directiva invierte el efecto de todas las directivas #line anteriores. El compilador notifica información de línea es true para las líneas siguientes, precisamente como si no `#line` directivas se hubiera procesado.

Un `#line hidden` directiva no tiene ningún efecto en el archivo y números de línea que se indica en el error mensajes, pero afecta a la depuración de nivel de origen. Al depurar, todas las líneas entre un `#line hidden` directiva y la subsiguiente `#line` directiva (que no es `#line hidden`) no tienen ninguna información de número de línea. Al recorrer el código en el depurador, estas líneas se omitirá completamente.

Tenga en cuenta que un *file_name* difiere de un literal de cadena normal en que no se procesan los caracteres de escape; el "`\`" carácter simplemente designa un carácter de barra diagonal inversa ordinaria dentro de un *file_name*.

### <a name="pragma-directives"></a>Directivas pragma

El `#pragma` preprocesamiento de directiva se usa para especificar información contextual adicional para el compilador. La información proporcionada en un `#pragma` directiva nunca cambiará la semántica del programa.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C# proporciona `#pragma` directivas para controlar las advertencias del compilador. Las versiones futuras del lenguaje pueden incluir adicionales `#pragma` directivas. Para garantizar la interoperabilidad con otros compiladores de C#, el compilador de C# de Microsoft no emite errores de compilación si no se conoce `#pragma` directivas; este tipo no de las directivas pero generan advertencias.

#### <a name="pragma-warning"></a>Advertencia pragma

El `#pragma warning` directiva se usa para deshabilitar o restaurar todos o mensajes de un conjunto determinado de advertencia durante la compilación del texto del programa subsiguiente.

```antlr
pragma_warning_body
    : 'warning' whitespace warning_action
    | 'warning' whitespace warning_action whitespace warning_list
    ;

warning_action
    : 'disable'
    | 'restore'
    ;

warning_list
    : decimal_digit+ (whitespace? ',' whitespace? decimal_digit+)*
    ;
```

Un `#pragma warning` directiva que omita la lista de advertencias afecta a todas las advertencias. Un `#pragma warning` directiva el incluye una lista de advertencias afecta solo a esas advertencias que se especifican en la lista.

Un `#pragma warning disable` directiva deshabilita todas o el conjunto dado de advertencias.

Un `#pragma warning restore` directiva restaura todas o el conjunto dado de advertencias al estado que estaba en efecto al principio de la unidad de compilación. Tenga en cuenta que si una advertencia concreta se deshabilitó externamente, un `#pragma warning restore` (sea para todos o la advertencia concreta) no volverá a habilitar dicha advertencia.

El ejemplo siguiente muestra el uso de `#pragma warning` deshabilitar temporalmente la advertencia notificados cuando obsoleto se hace referencia a los miembros, con el número de advertencia del compilador de C# de Microsoft.
```csharp
using System;

class Program
{
    [Obsolete]
    static void Foo() {}

    static void Main() {
#pragma warning disable 612
    Foo();
#pragma warning restore 612
    }
}
```
