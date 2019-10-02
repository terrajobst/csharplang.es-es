---
ms.openlocfilehash: 4676bcd3f0a92260b4e5e20a0aa5b5ec00bf204e
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704073"
---
# <a name="lexical-structure"></a>Estructura léxica

## <a name="programs"></a>Programas

Un C# ***programa*** se compone de uno o varios ***archivos de código fuente***, conocido formalmente como ***unidades de compilación*** (unidades de[compilación](namespaces.md#compilation-units)). Un archivo de origen es una secuencia ordenada de caracteres Unicode. Los archivos de origen suelen tener una correspondencia uno a uno con los archivos de un sistema de archivos, pero esta correspondencia no es necesaria. Para obtener la máxima portabilidad, se recomienda codificar los archivos de un sistema de archivos con la codificación UTF-8.

En términos conceptuales, un programa se compila mediante tres pasos:

1. Transformación, que convierte un archivo de un repertorio de caracteres determinado y un esquema de codificación en una secuencia de caracteres Unicode.
2. Análisis léxico, que convierte una secuencia de caracteres de entrada Unicode en una secuencia de tokens.
3. Análisis sintáctico, que convierte el flujo de tokens en código ejecutable.

## <a name="grammars"></a>Gramáticas

Esta especificación presenta la sintaxis del lenguaje C# de programación con dos gramáticas. La ***gramática léxica*** ([gramática léxica](lexical-structure.md#lexical-grammar)) define cómo se combinan los caracteres Unicode para formar terminadores de línea, espacios en blanco, comentarios, tokens y directivas de procesamiento previo. La ***gramática sintáctica*** ([gramática sintáctica](lexical-structure.md#syntactic-grammar)) define el modo en que los tokens resultantes de la gramática léxica C# se combinan para formar programas.

### <a name="grammar-notation"></a>Notación gramatical

Las gramáticas léxicas y sintácticas se presentan en formato de Backus-Naur mediante la notación de la herramienta de gramática de ANTLR.

### <a name="lexical-grammar"></a>Gramática léxica

La gramática léxica de C# se presenta en el [análisis léxico](lexical-structure.md#lexical-analysis), los [tokens](lexical-structure.md#tokens)y las [directivas de procesamiento previo](lexical-structure.md#pre-processing-directives). Los símbolos de terminal de la gramática léxica son los caracteres del juego de caracteres Unicode y la gramática léxica especifica cómo se combinan los caracteres para formar tokens ([tokens](lexical-structure.md#tokens)), espacios en blanco ([espacios en blanco](lexical-structure.md#white-space)), comentarios ([comentarios](lexical-structure.md#comments)) y directivas de procesamiento previo ([directivas de procesamiento previo](lexical-structure.md#pre-processing-directives)).

Cada archivo de código fuente C# de un programa debe ajustarse a la producción de *entrada* de la gramática léxica ([análisis léxico](lexical-structure.md#lexical-analysis)).

### <a name="syntactic-grammar"></a>Gramática sintáctica

La gramática sintáctica de C# se presenta en los capítulos y apéndices que siguen este capítulo. Los símbolos de terminal de la gramática sintáctica son los tokens definidos por la gramática léxica y la gramática sintáctica especifica cómo se combinan los tokens para C# formar programas.

Cada archivo de código fuente C# de un programa debe ajustarse a la producción *compilation_unit* de la gramática sintáctica ([unidades de compilación](namespaces.md#compilation-units)).

## <a name="lexical-analysis"></a>Análisis léxico

La producción de *entrada* define la estructura léxica de C# un archivo de código fuente. Cada archivo de código fuente C# de un programa debe ajustarse a esta producción de gramática léxica.

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

Los cinco elementos básicos componen la estructura léxica C# de un archivo de código fuente: Terminadores de línea ([terminadores de línea](lexical-structure.md#line-terminators)), espacio en blanco ([espacio en blanco](lexical-structure.md#white-space)), comentarios ([comentarios](lexical-structure.md#comments)), tokens ([tokens](lexical-structure.md#tokens)) y directivas de preprocesamiento ([directivas de procesamiento previo](lexical-structure.md#pre-processing-directives)). De estos elementos básicos, solo los tokens son significativos en la gramática sintáctica de C# un programa ([gramática sintáctica](lexical-structure.md#syntactic-grammar)).

El procesamiento léxico de un C# archivo de código fuente consiste en reducir el archivo en una secuencia de tokens que se convierte en la entrada del análisis sintáctico. Los terminadores de línea, los espacios en blanco y los comentarios pueden servir para separar los tokens, y las directivas de procesamiento previo pueden provocar que se omitan las secciones del archivo de código fuente, pero, de lo contrario, C# estos elementos léxicos no tienen ningún impacto en la estructura sintáctica de un programa.

En el caso de los literales de cadena interpolados ([literales de cadena interpolados](lexical-structure.md#interpolated-string-literals)), un único token lo genera inicialmente el análisis léxico, pero se divide en varios elementos de entrada que se someten repetidamente al análisis léxico hasta que se interpolan todos se han resuelto los literales de cadena. Los tokens resultantes sirven como entrada para el análisis sintáctico.

Cuando varias producciones de gramática léxica coinciden con una secuencia de caracteres de un archivo de código fuente, el procesamiento léxico siempre forma el elemento léxico más largo posible. Por ejemplo, la secuencia `//` de caracteres se procesa como el principio de un Comentario de una sola línea porque ese elemento léxico es más largo que un token único. `/`

### <a name="line-terminators"></a>Terminadores de línea

Los terminadores de línea dividen los caracteres C# de un archivo de origen en líneas.

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

Por compatibilidad con las herramientas de edición de código fuente que agregan marcadores de fin de archivo y para permitir que un archivo de origen se vea como una secuencia de líneas terminadas correctamente, se aplican las transformaciones siguientes, en orden, a C# cada archivo de código fuente de un programa:

*  Si el último carácter del archivo de código fuente es un carácter control-Z`U+001A`(), este carácter se elimina.
*  Un carácter de retorno de carro`U+000D`() se agrega al final del archivo de código fuente si ese archivo de código fuente no está vacío y si el último carácter del archivo de origen no es un retorno`U+000D`de carro (), un`U+000A`salto de línea (), un separador de líneas (`U+2028`) o un separador de párrafo`U+2029`().

### <a name="comments"></a>Comentarios

Se admiten dos formas de comentarios: comentarios de una sola línea y comentarios delimitados. Los ***comentarios de una sola línea*** comienzan con `//` los caracteres y se extienden hasta el final de la línea de código fuente. Los ***comentarios delimitados*** comienzan con `/*` los caracteres y terminan `*/`con los caracteres. Los comentarios delimitados pueden abarcar varias líneas.

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
    : '/*' delimited_comment_section* asterisk+ '/'
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

Los comentarios no se anidan. `/*` Las secuencias de caracteres `*/` y no tienen ningún significado especial `//` dentro de un comentario y las `//` secuencias `/*` de caracteres y no tienen ningún significado especial dentro de un comentario delimitado.

Los comentarios no se procesan dentro de los literales de carácter y de cadena.

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
muestra varios comentarios de una sola línea.

### <a name="white-space"></a>Espacio en blanco

El espacio en blanco se define como cualquier carácter con la clase Unicode ZS (que incluye el carácter de espacio), así como el carácter de tabulación horizontal, el carácter de tabulación vertical y el carácter de avance de la forma.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>tokens

Hay varios tipos de tokens: identificadores, palabras clave, literales, operadores y signos de puntuación. Los espacios en blanco y los comentarios no son tokens, aunque actúan como separadores de tokens.

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

Una secuencia de escape de caracteres Unicode representa un carácter Unicode. Las secuencias de escape de caracteres Unicode se procesan en identificadores ([identificadores](lexical-structure.md#identifiers)), literales de carácter ([literales de carácter](lexical-structure.md#character-literals)) y literales de cadena normales ([literales de cadena](lexical-structure.md#string-literals)). Un escape de carácter Unicode no se procesa en ninguna otra ubicación (por ejemplo, para formar un operador, un signo de puntuación o una palabra clave).

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Una secuencia de escape Unicode representa el carácter Unicode que forma el número hexadecimal después de los`\u`caracteres ""`\U`o "". Puesto C# que utiliza una codificación de 16 bits de puntos de código Unicode en caracteres y valores de cadena, no se permite un carácter Unicode en el intervalo de u + 10000 a U + 10FFFF en un literal de carácter y se representa mediante un par suplente Unicode en un literal de cadena. No se admiten los caracteres Unicode con puntos de código anteriores a 0x10FFFF.

No se realizan varias traducciones. Por ejemplo, el literal de cadena`\u005Cu005C`"" es equivalente a`\u005C`"" en lugar`\`de "". El valor `\u005C` Unicode es el carácter "`\`".

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
muestra varios usos de `\u0066`, que es la secuencia de escape para la letra`f`"". El programa es equivalente a
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

Las reglas para los identificadores que se proporcionan en esta sección corresponden exactamente a las recomendadas por el Anexo 31 del estándar Unicode, con la excepción de que el carácter de subrayado se permite como carácter inicial (como el tradicional en el lenguaje de programación C), las secuencias de escape Unicode son se permite en los identificadores y el`@`carácter "" se permite como prefijo para habilitar las palabras clave que se usarán como identificadores.

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

Para obtener información sobre las clases de caracteres Unicode mencionadas anteriormente, vea el estándar Unicode, versión 3,0, sección 4,5.

Entre los ejemplos de identificadores válidos se incluyen`_identifier2`"`identifier1`", "`@if`" y "".

Un identificador en un programa conforme debe estar en el formato canónico definido por la forma de normalización Unicode C, tal y como se define en el Anexo 15 del estándar Unicode. El comportamiento cuando se encuentra un identificador no en la forma de normalización C está definido por la implementación; sin embargo, no es necesario un diagnóstico.

El prefijo`@`"" permite el uso de palabras clave como identificadores, lo que resulta útil al interactuar con otros lenguajes de programación. El carácter `@` no es realmente parte del identificador, por lo que el identificador podría verse en otros idiomas como un identificador normal, sin el prefijo. Un identificador con un `@` prefijo se denomina ***identificador textual***. Se permite el `@` uso del prefijo para los identificadores que no son palabras clave, pero se desaconseja encarecidamente como una cuestión de estilo.

En el ejemplo:
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
define una clase denominada "`class`" con un método estático denominado "`static`" que toma un parámetro con el`bool`nombre "". Tenga en cuenta que, puesto que no se permiten los escapes de Unicode`cl\u0061ss`en palabras clave, el token "" es un identificador y es el`@class`mismo identificador que "".

Dos identificadores se consideran iguales si son idénticos después de aplicar las transformaciones siguientes, en orden:

*  El prefijo`@`"", si se utiliza, se quita.
*  Cada *unicode_escape_sequence* se transforma en el carácter Unicode correspondiente.
*  Se quitan los *formatting_character*s.

Los identificadores que contienen dos caracteres de subrayado consecutivos (`U+005F`) se reservan para su uso por parte de la implementación. Por ejemplo, una implementación podría proporcionar palabras clave extendidas que comienzan con dos guiones bajos.

### <a name="keywords"></a>Palabras clave

Una ***palabra clave*** es una secuencia similar a un identificador de caracteres que está reservada y no se puede usar como identificador excepto cuando está precedida por el `@` carácter.

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

En algunos lugares de la gramática, los identificadores específicos tienen un significado especial, pero no son palabras clave. Dichos identificadores se denominan a veces "palabras clave contextuales". Por ejemplo, dentro de una declaración de propiedad,`get`los identificadores`set`"" y "" tienen un significado especial ([descriptores de acceso](classes.md#accessors)). Un identificador distinto de `get` o `set` nunca se permite en estas ubicaciones, por lo que este uso no entra en conflicto con el uso de estas palabras como identificadores. En otros casos, como con el identificador "`var`" en las declaraciones de variables locales con tipo implícito (declaraciones de[variables locales](statements.md#local-variable-declarations)), una palabra clave contextual puede entrar en conflicto con los nombres declarados. En tales casos, el nombre declarado tiene prioridad sobre el uso del identificador como palabra clave contextual.

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

Hay dos valores literales booleanos `true` : `false`y.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

El tipo de *boolean_literal* es `bool`.

#### <a name="integer-literals"></a>Literales enteros

Los literales enteros se utilizan para escribir valores de tipos `int` `long`, `uint`, y `ulong`. Los literales enteros tienen dos formas posibles: decimal y hexadecimal.

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

El tipo de un literal entero se determina de la siguiente manera:

*  Si el literal no tiene sufijo, tiene el primero de estos tipos en el que se puede representar su valor: `int`, `uint`, `long`, `ulong`.
*  Si el literal tiene el `U` sufijo o `u`, tiene el primero de estos tipos en el que se puede representar su valor: `uint`, `ulong`.
*  Si el literal tiene el `L` sufijo o `l`, tiene el primero de estos tipos en el que se puede representar su valor: `long`, `ulong`.
*  Si el literal `UL`tiene el sufijo, `LU` `ul` `Ul`, `uL`,,, `Lu`, `lU`o `ulong`, es de tipo. `lu`

Si el valor representado por un literal entero está fuera del intervalo del `ulong` tipo, se produce un error en tiempo de compilación.

Como cuestión de estilo, se sugiere que "`L`" se use en lugar de "`l`" cuando se escriben literales de tipo `long`, ya que es fácil confundir la letra "`l`" con el dígito "`1`".

Para permitir que los valores y `int` `long` más pequeños posibles se escriban como literales enteros decimales, existen las dos reglas siguientes:

* Cuando un *decimal_integer_literal* con el valor 2147483648 (2 ^ 31) y no *integer_type_suffix* aparece como el token inmediatamente después de un token de operador unario menos ([operador unario menos](expressions.md#unary-minus-operator)), el resultado es una constante de tipo `int` con el valor-2147483648 (-2 ^ 31). En todas las demás situaciones, tal *decimal_integer_literal* es del tipo `uint`.
* Cuando un *decimal_integer_literal* con el valor 9.223.372.036.854.775.808 (2 ^ 63) y no *integer_type_suffix* ni *integer_type_suffix* `L` o `l` aparece como el token inmediatamente después de un token de operador unario menos ([ Unario menos operador](expressions.md#unary-minus-operator)), el resultado es una constante de tipo `long` con el valor-9.223.372.036.854.775.808 (-2 ^ 63). En todas las demás situaciones, tal *decimal_integer_literal* es del tipo `ulong`.

#### <a name="real-literals"></a>Literales reales

Los literales reales se utilizan para escribir valores de `float`tipos `double`, y `decimal`.

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

Si no se especifica *real_type_suffix* , el tipo del literal real es `double`. De lo contrario, el sufijo de tipo real determina el tipo del literal real, como se indica a continuación:

*  Un literal real con `F` sufijo o `f` es de tipo `float`. Por ejemplo, los literales `1f`, `1.5f`, `1e10f`y `123.456F` son de tipo `float`.
*  Un literal real con `D` sufijo o `d` es de tipo `double`. Por ejemplo, los literales `1d`, `1.5d`, `1e10d`y `123.456D` son de tipo `double`.
*  Un literal real con `M` sufijo o `m` es de tipo `decimal`. Por ejemplo, los literales `1m`, `1.5m`, `1e10m`y `123.456M` son de tipo `decimal`. Este literal se convierte en un `decimal` valor mediante el uso del valor exacto, y, si es necesario, se redondea al valor representable más cercano mediante el redondeo bancario ([el tipo decimal](types.md#the-decimal-type)). Cualquier escala aparente en el literal se conserva a menos que el valor se redondee o el valor sea cero (en cuyo caso, el signo y la escala serán 0). Por lo tanto, `2.900m` el literal se analizará para formar el decimal con `0`el signo, `2900`el coeficiente `3`y la escala.

Si el literal especificado no se puede representar en el tipo indicado, se produce un error en tiempo de compilación.

El valor de un literal real de tipo `float` o `double` se determina mediante el modo "redondeo al más cercano" de IEEE.

Tenga en cuenta que, en un literal real, siempre se requieren dígitos decimales después del separador decimal. Por ejemplo, `1.3F` es un literal real pero `1.F` no es.

#### <a name="character-literals"></a>Literales de carácter

Un literal de carácter representa un carácter único y normalmente consta de un carácter entre comillas, como `'a'`en.

Nota: La notación gramatical ANTLR hace que la siguiente confusión. En ANTLR, cuando escribe `\'` , representa una comilla `'`simple. Y, cuando se `\\` escribe, representa una sola barra diagonal `\`inversa. Por lo tanto, la primera regla para un literal de carácter significa que empieza con una comilla simple, un carácter y, a continuación, una comilla simple. Y las once secuencias de escape simples posibles `\'`son `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, ,`\t` ,`\v`.

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

Un carácter que sigue a un carácter de barra`\`diagonal inversa () en un *carácter* debe ser uno de los `'`siguientes `"`caracteres `\`:, `a`, `b`, `f` `0`,,, , `n`, `r`, `t`, `u`, `U`, `x`, `v`. De lo contrario, se produce un error en tiempo de compilación.

Una secuencia de escape hexadecimal representa un único carácter Unicode, con el valor formado por el número hexadecimal siguiente`\x`a "".

Si el valor representado por un literal de carácter es mayor `U+FFFF`que, se produce un error en tiempo de compilación.

Una secuencia de escape de caracteres Unicode ([secuencias de escape de caracteres Unicode](lexical-structure.md#unicode-character-escape-sequences)) en un literal de carácter `U+0000` debe `U+FFFF`estar en el intervalo de.

Una secuencia de escape simple representa una codificación de caracteres Unicode, tal y como se describe en la tabla siguiente.


| __Secuencia de escape__ | __Nombre del carácter__ | __Codificación Unicode__ |
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

El tipo de *character_literal* es `char`.

#### <a name="string-literals"></a>Literales de cadena

C#admite dos formatos de literales de cadena: literales de ***cadena normales*** y ***literales de cadena textuales***.

Un literal de cadena normal consta de cero o más caracteres entre comillas dobles, como `"hello"`en, y puede incluir secuencias de escape simples ( `\t` por ejemplo, para el carácter de tabulación) y secuencias de escape hexadecimal y Unicode.

Un literal de cadena textual se compone `@` de un carácter seguido de un carácter de comilla doble, cero o más caracteres, y un carácter de comilla doble de cierre. Un ejemplo sencillo es `@"hello"`. En un literal de cadena textual, los caracteres entre los delimitadores se interpretan literalmente, la única excepción es *quote_escape_sequence*. En concreto, las secuencias de escape simples, y las secuencias de escape hexadecimales y Unicode no se procesan en literales de cadena textuales. Un literal de cadena textual puede abarcar varias líneas.

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

Un carácter que sigue a un carácter de barra diagonal inversa (`\`) en un *regular_string_literal_character* debe ser uno de los siguientes caracteres: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, 0, 1, @no__ t-12, 3, 4, 5. De lo contrario, se produce un error en tiempo de compilación.

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
muestra varios literales de cadena. El último literal de cadena `j`,, es un literal de cadena textual que abarca varias líneas. Los caracteres entre comillas, incluidos los espacios en blanco, como los caracteres de nueva línea, se conservan literalmente.

Dado que una secuencia de escape hexadecimal puede tener un número variable de dígitos hexadecimales, `"\x123"` el literal de cadena contiene un carácter único con el valor hexadecimal 123. Para crear una cadena que contenga el carácter con el valor hexadecimal 12 seguido del carácter 3, puede `"\x00123"` escribir `"\x12" + "3"` en su lugar o.

El tipo de *string_literal* es `string`.

Cada literal de cadena no tiene necesariamente como resultado una nueva instancia de cadena. Cuando dos o más literales de cadena que son equivalentes de acuerdo con el operador de igualdad de cadena ([operadores de igualdad de cadena](expressions.md#string-equality-operators)) aparecen en el mismo programa, estos literales de cadena hacen referencia a la misma instancia de cadena. Por ejemplo, la salida generada por
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
se `True` debe a que los dos literales hacen referencia a la misma instancia de cadena.

#### <a name="interpolated-string-literals"></a>Literales de cadena interpolados

Los literales de cadena interpolados son similares a los literales de cadena, pero contienen `{` huecos delimitados por y `}`, donde se pueden producir expresiones. En tiempo de ejecución, las expresiones se evalúan con el propósito de tener sus formularios de texto sustituidos en la cadena en el lugar donde se produce el agujero. La sintaxis y la semántica de la interpolación de cadenas se describen en la sección ([cadenas interpoladas](expressions.md#interpolated-strings)).

Al igual que los literales de cadena, los literales de cadena interpolados pueden ser normales o literales. Los literales de cadena normales interpolados están delimitados `$"` por y `"`, y los literales de cadena textual interpolados `"`están delimitados por `$@"` y.

Al igual que otros literales, el análisis léxico de un literal de cadena interpolada produce inicialmente un token único, según la gramática siguiente. Sin embargo, antes del análisis sintáctico, el token único de un literal de cadena interpolada se divide en varios tokens para las partes de la cadena que los rodean, y los elementos de entrada que se producen en los huecos se analizan léxicamente de nuevo. Esto puede, a su vez, generar más literales de cadena interpolados que se van a procesar, pero, si léxicamente correcto, dará lugar a una secuencia de tokens para que los procese el análisis sintáctico.

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

Un token de *interpolated_string_literal* se reinterpreta como varios tokens y otros elementos de entrada como se indica a continuación, en el orden de aparición en el *interpolated_string_literal*:

* Las apariciones de lo siguiente se interpretan como tokens individuales independientes: el signo `$` inicial, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*,  *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* y *interpolated_verbatim_string_end*.
* Las repeticiones de *regular_balanced_text* y *verbatim_balanced_text* entre estas se reprocesan como *input_section* ([análisis léxico](lexical-structure.md#lexical-analysis)) y se reinterpretan como la secuencia resultante de los elementos de entrada. A su vez, pueden incluir tokens literales de cadena interpolados que se van a reinterpretar.

El análisis sintáctico volverá a combinar los tokens en una *interpolated_string_expression* ([cadenas interpoladas](expressions.md#interpolated-strings)).

Ejemplos TODO


#### <a name="the-null-literal"></a>El literal null

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal* se puede convertir implícitamente a un tipo de referencia o a un tipo que acepta valores NULL.

### <a name="operators-and-punctuators"></a>Operadores y signos de puntuación

Hay varios tipos de operadores y signos de puntuación. Los operadores se utilizan en expresiones para describir las operaciones que implican a uno o varios operandos. Por ejemplo, la expresión `a + b` usa el `+` operador para `a` agregar los dos operandos y `b`. Los signos de puntuación son para agrupar y separar.

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

La barra vertical de las producciones *RIGHT_SHIFT* y *right_shift_assignment* se usa para indicar que, a diferencia de otras producciones en la gramática sintáctica, no se permiten caracteres de ningún tipo (ni siquiera espacios en blanco) entre los tokens. Estas producciones se tratan de forma especial para habilitar el control correcto de *type_parameter_list*s ([parámetros de tipo](classes.md#type-parameters)).

## <a name="pre-processing-directives"></a>Directivas de procesamiento previo

Las directivas de procesamiento previo proporcionan la capacidad de omitir condicionalmente las secciones de los archivos de código fuente, notificar las condiciones de error y ADVERTENCIA, y para delimitar distintas regiones del código fuente. El término "directivas de procesamiento previo" solo se usa para mantener la coherencia con los C++ lenguajes de programación C y. En C#, no hay ningún paso de procesamiento previo independiente; las directivas de procesamiento previo se procesan como parte de la fase de análisis léxico.

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

Están disponibles las siguientes directivas de procesamiento previo:

*  `#define`y `#undef`, que se usan para definir y anular la definición, respectivamente, de símbolos de compilación condicional ([directivas de declaración](lexical-structure.md#declaration-directives)).
*  `#if`, `#elif`, `#else` y`#endif`, que se usan para omitir condicionalmente las secciones del código fuente ([directivas de compilación condicional](lexical-structure.md#conditional-compilation-directives)).
*  `#line`, que se usa para controlar los números de línea emitidos para errores y advertencias ([directivas de línea](lexical-structure.md#line-directives)).
*  `#error`y `#warning`, que se usan para emitir errores y advertencias, respectivamente ([directivas de diagnóstico](lexical-structure.md#diagnostic-directives)).
*  `#region`y `#endregion`, que se usan para marcar explícitamente las secciones del código fuente ([directivas de región](lexical-structure.md#region-directives)).
*  `#pragma`, que se usa para especificar información contextual opcional para el compilador ([directivas pragma](lexical-structure.md#pragma-directives)).

Una directiva de procesamiento previo siempre ocupa una línea de código fuente independiente y siempre comienza con un `#` carácter y un nombre de directiva de preprocesamiento. Los espacios en blanco pueden aparecer `#` antes del carácter y `#` entre el carácter y el nombre de la Directiva.

Una línea de código fuente `#define`que contiene una `#elif`directiva `#else`, `#endif` `#if` `#undef`, `#line`,, `#endregion` ,, o puede terminar con un Comentario de una sola línea. Los comentarios delimitados `/* */` (el estilo de los comentarios) no se permiten en líneas de código fuente que contengan directivas de procesamiento previo.

Las directivas de procesamiento previo no son tokens y no forman parte de la gramática sintáctica C#de. Sin embargo, las directivas de procesamiento previo se pueden usar para incluir o excluir secuencias de tokens y, de ese modo, pueden C# afectar al significado de un programa. Por ejemplo, cuando se compila, el programa:
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
da como resultado la misma secuencia de tokens que el programa:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

Por lo tanto, mientras que los dos programas son bastante diferentes y sintácticamente, son idénticos.

### <a name="conditional-compilation-symbols"></a>Símbolos de compilación condicional

La funcionalidad de compilación condicional proporcionada por `#if`las `#elif`directivas `#else`,, `#endif` y se controla a través de expresiones de procesamiento previo ([expresiones de procesamiento previo](lexical-structure.md#pre-processing-expressions)) y condicional. símbolos de compilación.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Un símbolo de compilación condicional tiene dos Estados posibles: ***definido*** o no ***definido***. Al principio del procesamiento léxico de un archivo de código fuente, un símbolo de compilación condicional es indefinido, a menos que se haya definido explícitamente mediante un mecanismo externo (como una opción del compilador de línea de comandos). Cuando se `#define` procesa una directiva, el símbolo de compilación condicional denominado en esa Directiva se define en ese archivo de código fuente. El símbolo permanece definido hasta que `#undef` se procesa una directiva para el mismo símbolo, o hasta que se alcanza el final del archivo de código fuente. Una implicación de esto es que `#define` las directivas y `#undef` de un archivo de código fuente no tienen ningún efecto en otros archivos de código fuente del mismo programa.

Cuando se hace referencia en una expresión de procesamiento previo, un símbolo de compilación condicional definido tiene el `true`valor booleano y un símbolo de compilación condicional sin definir tiene el `false`valor booleano. No hay ningún requisito de que los símbolos de compilación condicional se declaren explícitamente antes de que se haga referencia a ellos en expresiones de procesamiento previo. En su lugar, los símbolos no declarados simplemente están sin definir y `false`, por tanto, tienen el valor.

El espacio de nombres de los símbolos de compilación condicional es distinto y separado de todas las demás C# entidades con nombre en un programa. Solo se puede hacer referencia a los símbolos de `#define` compilación `#undef` condicional en las directivas y y en las expresiones de procesamiento previo.

### <a name="pre-processing-expressions"></a>Expresiones de procesamiento previo

Las expresiones de procesamiento previo pueden producirse `#elif` en `#if` las directivas y. Los operadores `!` `==` ,`&&` , y sepermitenenlasexpresionesdeprocesamientoprevioylosparéntesissepuedenusarparalaagrupación.`||` `!=`

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

Cuando se hace referencia en una expresión de procesamiento previo, un símbolo de compilación condicional definido tiene el `true`valor booleano y un símbolo de compilación condicional sin definir tiene el `false`valor booleano.

La evaluación de una expresión de procesamiento previo siempre produce un valor booleano. Las reglas de evaluación de una expresión de procesamiento previo son las mismas que las de una expresión constante ([expresiones constantes](expressions.md#constant-expressions)), salvo que las únicas entidades definidas por el usuario a las que se puede hacer referencia son símbolos de compilación condicionales.

### <a name="declaration-directives"></a>Directivas de declaración

Las directivas de declaración se utilizan para definir o anular la definición de símbolos de compilación condicional.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

El procesamiento de una `#define` Directiva hace que se defina el símbolo de compilación condicional dado, empezando por la línea de código fuente que sigue a la Directiva. Del mismo modo, el procesamiento `#undef` de una directiva hace que el símbolo de compilación condicional dado quede sin definir, empezando por la línea de código fuente que sigue a la Directiva.

Las `#define` directivas `#undef` y de un archivo de código fuente deben aparecer antes del primer *token* ([tokens](lexical-structure.md#tokens)) en el archivo de código fuente; de lo contrario, se produce un error en tiempo de compilación. En términos intuitivos `#define` , `#undef` y las directivas deben preceder a cualquier "código real" en el archivo de código fuente.

En el ejemplo:
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
es válido porque las `#define` directivas preceden al primer token `namespace` (la palabra clave) en el archivo de código fuente.

En el ejemplo siguiente se produce un error en tiempo de compilación `#define` porque un sigue código real:
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

Puede definir un símbolo `#undef` de compilación condicional que ya esté definido, sin que intervenga ningún símbolo. `#define` En el ejemplo siguiente se define un símbolo `A` de compilación condicional y, a continuación, se define de nuevo.
```csharp
#define A
#define A
```

Un `#undef` puede "anular la definición" de un símbolo de compilación condicional que no está definido. En el ejemplo siguiente se define un símbolo `A` de compilación condicional y, a continuación, se anula su `#undef` definición dos veces; aunque el segundo no tiene ningún efecto, sigue siendo válido.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Directivas de compilación condicional

Las directivas de compilación condicional se usan para incluir o excluir de forma condicional partes de un archivo de código fuente.

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

Como se indica en la sintaxis, las directivas de compilación condicional se deben escribir como conjuntos compuestos de, en orden `#if` , una directiva, cero `#elif` o más directivas, cero `#else` o una directiva y `#endif` una directiva. Entre las directivas se encuentran las secciones condicionales del código fuente. Cada sección se controla mediante la Directiva inmediatamente anterior. Una sección condicional puede contener directivas de compilación condicional anidadas, siempre que estas directivas formen conjuntos completos.

Un *pp_conditional* selecciona como máximo una de las *conditional_section*s contenidas para el procesamiento léxico normal:

*  Las *pp_expression*s de las directivas `#if` y `#elif` se evalúan en orden hasta que un produce `true`. Si una expresión produce `true`, se selecciona el *conditional_section* de la directiva correspondiente.
*  Si todos los *pp_expression*de rendimiento son `false`, y si hay una directiva `#else`, se selecciona el *conditional_section* de la Directiva `#else`.
*  De lo contrario, no se selecciona ningún *conditional_section* .

El *conditional_section*seleccionado, si existe, se procesa como un *input_section*normal: el código fuente contenido en la sección debe adherirse a la gramática léxica; los tokens se generan a partir del código fuente de la sección. y las directivas de procesamiento previo de la sección tienen los efectos prescritos.

El resto de *conditional_section*s, si los hay, se procesan como *skipped_section*s: excepto en el caso de las directivas de procesamiento previo, el código fuente de la sección no necesita adherirse a la gramática léxica; no se generan tokens a partir del código fuente de la sección; y las directivas de procesamiento previo de la sección deben ser léxicamente correctas, pero no se procesan de otra manera. Dentro de un *conditional_section* que se está procesando como *skipped_section*, cualquier *conditional_section*anidada (incluida en construcciones `#if`... `#endif` y `#region`... `#endregion` anidadas) también se procesa como *skipped_ secciones*s.

En el ejemplo siguiente se muestra cómo se pueden anidar las directivas de compilación condicional:
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

Excepto en el caso de las directivas de procesamiento previo, el código fuente omitido no está sujeto al análisis léxico. Por ejemplo, lo siguiente es válido a pesar del comentario sin terminar en la `#else` sección:
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

Sin embargo, tenga en cuenta que las directivas de procesamiento previo deben ser léxicamente correctas incluso en secciones omitidas del código fuente.

Las directivas de procesamiento previo no se procesan cuando aparecen dentro de elementos de entrada de varias líneas. Por ejemplo, el programa:
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
da como resultado el resultado:
```console
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

En casos peculiares, el conjunto de directivas de procesamiento previo que se procesa puede depender de la evaluación de *pp_expression*. En el ejemplo:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
siempre genera el mismo flujo de token`class` ( `}` `Q` `{` ), independientemente de si está `X` definido o no. Si `X` se define, las únicas directivas procesadas `#if` son `#endif`y, debido al comentario de varias líneas. Si `X` es undefined, tres directivas (`#if`, `#else`, `#endif`) forman parte del conjunto de directivas.

### <a name="diagnostic-directives"></a>Directivas de diagnóstico

Las directivas de diagnóstico se utilizan para generar explícitamente mensajes de error y de advertencia que se detectan de la misma manera que otros errores y advertencias en tiempo de compilación.

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

En el ejemplo:
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
siempre genera una advertencia ("se requiere una revisión del código antes de la inserción en el repositorio") y genera un error en tiempo de compilación ("una compilación no puede ser Debug and Retail `Retail` ") si se definen los símbolos `Debug` condicionales y. Tenga en cuenta que un *pp_message* puede contener texto arbitrario; en concreto, no es necesario que contenga tokens con formato correcto, tal como se muestra en la palabra `can't`.

### <a name="region-directives"></a>Directivas de región

Las directivas region se usan para marcar explícitamente las regiones del código fuente.

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

No se adjunta ningún significado semántico a una región; las regiones están pensadas para que las use el programador o las herramientas automatizadas para marcar una sección del código fuente. El mensaje especificado en una `#region` directiva `#endregion` o del mismo modo no tiene ningún significado semántico; simplemente sirve para identificar la región. La coincidencia de las directivas `#region` y `#endregion` puede tener diferentes *pp_message*.

El procesamiento léxico de una región:
```csharp
#region
...
#endregion
```
corresponde exactamente al procesamiento léxico de una directiva de compilación condicional del formulario:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Directivas de línea

Las directivas de línea se pueden usar para modificar los números de línea y los nombres de archivo de origen que el compilador indica en la salida, como advertencias y errores, y que se usan en los atributos de información de llamador ([atributos de información de llamador](attributes.md#caller-info-attributes)).

Las directivas de línea se utilizan normalmente en herramientas de metaprogramaciones C# que generan código fuente a partir de otra entrada de texto.

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

Cuando no `#line` hay directivas presentes, el compilador informa de los números de línea verdaderos y los nombres de archivo de origen en su salida. Al procesar una directiva de @no__t 0 que incluye un *line_indicator* que no es `default`, el compilador trata la línea después de la Directiva con el número de línea y el nombre de archivo especificados, si se especifica.

Una `#line default` Directiva invierte el efecto de todas las directivas de #line anteriores. El compilador notifica la información de línea verdadera para las líneas siguientes `#line` , exactamente como si no se hubieran procesado directivas.

Una `#line hidden` Directiva no tiene ningún efecto en el archivo y los números de línea indicados en los mensajes de error, pero afecta a la depuración de nivel de origen. Al depurar, todas las líneas `#line hidden` entre una directiva y `#line` la Directiva subsiguiente (que `#line hidden`no es) no tienen información de número de línea. Al recorrer el código en el depurador, estas líneas se omitirán por completo.

Tenga en cuenta que un *file_name* difiere de un literal de cadena normal en el que no se procesan los caracteres de escape; el carácter "`\`" simplemente designa un carácter de barra diagonal inversa normal dentro de un *file_name*.

### <a name="pragma-directives"></a>Directivas pragma

La `#pragma` Directiva de preprocesamiento se usa para especificar información contextual opcional para el compilador. La información proporcionada en una `#pragma` directiva nunca cambiará la semántica del programa.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C#proporciona `#pragma` directivas para controlar las advertencias del compilador. Las versiones futuras del lenguaje pueden incluir directivas `#pragma` adicionales. Para garantizar la interoperabilidad C# con otros compiladores, C# el compilador de Microsoft no emite `#pragma` errores de compilación para las directivas desconocidas; sin embargo, estas directivas sí generan advertencias.

#### <a name="pragma-warning"></a>ADVERTENCIA de pragma

La `#pragma warning` Directiva se usa para deshabilitar o restaurar todo o un conjunto determinado de mensajes de advertencia durante la compilación del texto del programa subsiguiente.

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

Una `#pragma warning` Directiva que omite la lista de advertencias afecta a todas las advertencias. Una `#pragma warning` Directiva que incluye una lista de advertencias afecta solo a las advertencias especificadas en la lista.

Una `#pragma warning disable` Directiva deshabilita todos o el conjunto de advertencias especificado.

Una `#pragma warning restore` Directiva restaura todos o el conjunto de advertencias especificado en el estado que estaba en vigor al principio de la unidad de compilación. Tenga en cuenta que si se ha deshabilitado una advertencia `#pragma warning restore` determinada externamente, una (ya sea para toda o la advertencia específica) no volverá a habilitar esa advertencia.

En el ejemplo siguiente se muestra `#pragma warning` el uso de para deshabilitar temporalmente la advertencia que se indica cuando se hace referencia a los miembros obsoletos C# , utilizando el número de advertencia del compilador de Microsoft.
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
