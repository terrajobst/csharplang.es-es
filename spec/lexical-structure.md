# <a name="lexical-structure"></a><span data-ttu-id="fd10f-101">Estructura léxica</span><span class="sxs-lookup"><span data-stu-id="fd10f-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="fd10f-102">Programas</span><span class="sxs-lookup"><span data-stu-id="fd10f-102">Programs</span></span>

<span data-ttu-id="fd10f-103">C# ***programa*** consta de uno o varios ***archivos de código fuente***, que se conoce formalmente como ***unidades de compilación*** ([unidades de compilación](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="fd10f-104">Un archivo de origen es una secuencia ordenada de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="fd10f-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="fd10f-105">Archivos de código fuente normalmente tienen una correspondencia uno a uno con los archivos en un sistema de archivos, pero no se requiere esta correspondencia.</span><span class="sxs-lookup"><span data-stu-id="fd10f-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="fd10f-106">Para maximizar la portabilidad, se recomienda que los archivos en un sistema de archivos se puede codificar con UTF-8 de codificación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="fd10f-107">En términos conceptuales, un programa se compila en tres pasos:</span><span class="sxs-lookup"><span data-stu-id="fd10f-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="fd10f-108">Transformación que convierte un archivo desde un repertorio de caracteres determinado y un esquema de codificación en una secuencia de caracteres Unicode.</span><span class="sxs-lookup"><span data-stu-id="fd10f-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="fd10f-109">Análisis léxico, que conviertan una secuencia de caracteres de entrada Unicode en una secuencia de tokens.</span><span class="sxs-lookup"><span data-stu-id="fd10f-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="fd10f-110">Análisis sintáctico, que convierte la secuencia de tokens en el código ejecutable.</span><span class="sxs-lookup"><span data-stu-id="fd10f-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="fd10f-111">Gramáticas</span><span class="sxs-lookup"><span data-stu-id="fd10f-111">Grammars</span></span>

<span data-ttu-id="fd10f-112">Esta especificación presenta la sintaxis del lenguaje C# programación mediante dos gramáticas.</span><span class="sxs-lookup"><span data-stu-id="fd10f-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="fd10f-113">El ***gramática léxica*** ([gramática léxica](lexical-structure.md#lexical-grammar)) define cómo se combinan los caracteres Unicode para formar terminadores de línea, espacio en blanco, comentarios, los tokens y las directivas de preprocesamiento.</span><span class="sxs-lookup"><span data-stu-id="fd10f-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="fd10f-114">El ***gramática sintáctica*** ([gramática sintáctica](lexical-structure.md#syntactic-grammar)) define cómo se combinan los tokens resultantes de la gramática léxica para formar programas de C#.</span><span class="sxs-lookup"><span data-stu-id="fd10f-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="fd10f-115">Notación gramatical</span><span class="sxs-lookup"><span data-stu-id="fd10f-115">Grammar notation</span></span>

<span data-ttu-id="fd10f-116">Las gramáticas léxicas y sintácticas se presentan en forma de Backus-Naur mediante la notación de la herramienta de gramática ANTLR.</span><span class="sxs-lookup"><span data-stu-id="fd10f-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="fd10f-117">Gramática léxica</span><span class="sxs-lookup"><span data-stu-id="fd10f-117">Lexical grammar</span></span>

<span data-ttu-id="fd10f-118">La gramática léxica de C# se presenta en [análisis léxico](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), y [directivas de preprocesamiento](lexical-structure.md#pre-processing-directives).</span><span class="sxs-lookup"><span data-stu-id="fd10f-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="fd10f-119">Los símbolos terminales de la gramática léxica son los caracteres del juego de caracteres Unicode y la gramática léxica especifica cómo se combinan los caracteres a los tokens de formulario ([Tokens](lexical-structure.md#tokens)), espacio en blanco ([espacio en blanco](lexical-structure.md#white-space)), los comentarios ([comentarios](lexical-structure.md#comments)) y las directivas de preprocesamiento ([directivas de preprocesamiento](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="fd10f-120">Cada archivo de código fuente en un programa de C# debe ajustarse a la *entrada* producción de la gramática léxica ([análisis léxico](lexical-structure.md#lexical-analysis)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="fd10f-121">Gramática sintáctica</span><span class="sxs-lookup"><span data-stu-id="fd10f-121">Syntactic grammar</span></span>

<span data-ttu-id="fd10f-122">La gramática sintáctica de C# se presenta en los capítulos y apéndices que siguen este capítulo.</span><span class="sxs-lookup"><span data-stu-id="fd10f-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="fd10f-123">Los símbolos terminales de la gramática sintáctica son los tokens definidos por la gramática léxica, y la gramática sintáctica especifica cómo se combinan los tokens para formar programas de C#.</span><span class="sxs-lookup"><span data-stu-id="fd10f-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="fd10f-124">Cada archivo de código fuente en un C# programa debe ajustarse a la *compilation_unit* producción de la gramática sintáctica ([unidades de compilación](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="fd10f-125">Análisis léxico</span><span class="sxs-lookup"><span data-stu-id="fd10f-125">Lexical analysis</span></span>

<span data-ttu-id="fd10f-126">El *entrada* producción define la estructura léxica de un archivo de código fuente de C#.</span><span class="sxs-lookup"><span data-stu-id="fd10f-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="fd10f-127">Cada archivo de código fuente en un programa de C# debe ajustarse a este trabajo de producción del léxico.</span><span class="sxs-lookup"><span data-stu-id="fd10f-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

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

<span data-ttu-id="fd10f-128">Cinco elementos básicos constituyen la estructura léxica de un C# archivo de código fuente: Terminadores de línea ([terminadores de línea](lexical-structure.md#line-terminators)), espacio en blanco ([espacio en blanco](lexical-structure.md#white-space)), los comentarios ([comentarios](lexical-structure.md#comments)), los tokens ([Tokens](lexical-structure.md#tokens)), y directivas de preprocesamiento ([directivas de preprocesamiento](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="fd10f-129">Estos elementos básicos, solo los tokens son significativos en la gramática sintáctica de un programa de C# ([gramática sintáctica](lexical-structure.md#syntactic-grammar)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="fd10f-130">El procesamiento léxico de un archivo de código fuente de C# consiste en reducir el archivo a una secuencia de tokens que se convierte en la entrada del análisis sintáctico.</span><span class="sxs-lookup"><span data-stu-id="fd10f-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="fd10f-131">Los terminadores de línea, espacio en blanco y los comentarios que pueden servir para separar los tokens y las directivas de preprocesamiento pueden hacer secciones del archivo de origen se omite, pero estos elementos léxicos no tienen ningún impacto en la estructura sintáctica de un programa de C#.</span><span class="sxs-lookup"><span data-stu-id="fd10f-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="fd10f-132">En el caso de los literales de cadena interpolada ([interpoladas literales de cadena](lexical-structure.md#interpolated-string-literals)) análisis léxico genera inicialmente un token único, pero se divide en varios elementos de entrada que repetidamente se someten a análisis léxico hasta que se han resueltos todos los literales de cadena interpolada.</span><span class="sxs-lookup"><span data-stu-id="fd10f-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="fd10f-133">Los tokens resultantes después puedan actuar como entrada para el análisis sintáctico.</span><span class="sxs-lookup"><span data-stu-id="fd10f-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="fd10f-134">Cuando varias producciones de gramática léxica coincide con una secuencia de caracteres en un archivo de origen, el procesamiento léxico siempre forma el elemento léxico más largo posible.</span><span class="sxs-lookup"><span data-stu-id="fd10f-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="fd10f-135">Por ejemplo, la secuencia de caracteres `//` se procesa como el principio de una sola línea de comentario porque dicho elemento léxico tiene más de una sola `/` token.</span><span class="sxs-lookup"><span data-stu-id="fd10f-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="fd10f-136">Terminadores de línea</span><span class="sxs-lookup"><span data-stu-id="fd10f-136">Line terminators</span></span>

<span data-ttu-id="fd10f-137">Los terminadores de línea dividen los caracteres de un archivo de código fuente de C# en líneas.</span><span class="sxs-lookup"><span data-stu-id="fd10f-137">Line terminators divide the characters of a C# source file into lines.</span></span>

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

<span data-ttu-id="fd10f-138">Para herramientas de edición que agregan marcadores de fin del archivo de código de compatibilidad con el origen y para habilitar un origen de archivo para verse correctamente como una secuencia de líneas terminadas, las transformaciones siguientes se aplican en orden, para cada archivo de código fuente en un programa de C#:</span><span class="sxs-lookup"><span data-stu-id="fd10f-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="fd10f-139">Si el último carácter del archivo de origen es un carácter de Control a la Z (`U+001A`), se elimina este carácter.</span><span class="sxs-lookup"><span data-stu-id="fd10f-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="fd10f-140">Un carácter de retorno de carro (`U+000D`) se agrega al final del archivo de origen si el archivo de código fuente no está vacío y si el último carácter del archivo de origen no es un retorno de carro (`U+000D`), un avance de línea (`U+000A`), un separador de línea (`U+2028`), o un separador de párrafos (`U+2029`).</span><span class="sxs-lookup"><span data-stu-id="fd10f-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="fd10f-141">Comentarios</span><span class="sxs-lookup"><span data-stu-id="fd10f-141">Comments</span></span>

<span data-ttu-id="fd10f-142">Se admiten dos formas de comentarios: comentarios de una línea y comentarios delimitados.</span><span class="sxs-lookup"><span data-stu-id="fd10f-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="fd10f-143">***Comentarios de una línea*** empiezan con los caracteres `//` y extender al final de la línea de código fuente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="fd10f-144">***Comentarios delimitados*** empiezan con los caracteres `/*` y terminan con los caracteres `*/`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="fd10f-145">Comentarios delimitados pueden abarcar varias líneas.</span><span class="sxs-lookup"><span data-stu-id="fd10f-145">Delimited comments may span multiple lines.</span></span>

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

<span data-ttu-id="fd10f-146">Comentarios no pueden anidarse.</span><span class="sxs-lookup"><span data-stu-id="fd10f-146">Comments do not nest.</span></span> <span data-ttu-id="fd10f-147">Las secuencias de caracteres `/*` y `*/` no tienen ningún significado especial dentro de un `//` comentario y las secuencias de caracteres `//` y `/*` no tienen ningún significado especial dentro de un comentario delimitado.</span><span class="sxs-lookup"><span data-stu-id="fd10f-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="fd10f-148">Los comentarios no se procesan dentro de literales de carácter y cadena.</span><span class="sxs-lookup"><span data-stu-id="fd10f-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="fd10f-149">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="fd10f-149">The example</span></span>
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
<span data-ttu-id="fd10f-150">incluye un comentario delimitado.</span><span class="sxs-lookup"><span data-stu-id="fd10f-150">includes a delimited comment.</span></span>

<span data-ttu-id="fd10f-151">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="fd10f-151">The example</span></span>
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
<span data-ttu-id="fd10f-152">muestra varios comentarios en una línea.</span><span class="sxs-lookup"><span data-stu-id="fd10f-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="fd10f-153">Espacio en blanco</span><span class="sxs-lookup"><span data-stu-id="fd10f-153">White space</span></span>

<span data-ttu-id="fd10f-154">Espacio en blanco se define como carácter de avance de cualquier carácter con la clase Unicode Zs (que incluye el carácter de espacio), así como el carácter de tabulación horizontal, el carácter de tabulación vertical y el formulario.</span><span class="sxs-lookup"><span data-stu-id="fd10f-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="fd10f-155">tokens</span><span class="sxs-lookup"><span data-stu-id="fd10f-155">Tokens</span></span>

<span data-ttu-id="fd10f-156">Hay varios tipos de tokens: identificadores, palabras clave, literales, operadores y signos de puntuación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="fd10f-157">Espacio en blanco y los comentarios no son tokens, aunque actúan como separadores de los tokens.</span><span class="sxs-lookup"><span data-stu-id="fd10f-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

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

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="fd10f-158">Secuencias de escape de caracteres Unicode</span><span class="sxs-lookup"><span data-stu-id="fd10f-158">Unicode character escape sequences</span></span>

<span data-ttu-id="fd10f-159">Una secuencia de escape de caracteres Unicode representa un carácter Unicode.</span><span class="sxs-lookup"><span data-stu-id="fd10f-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="fd10f-160">Las secuencias de escape de carácter Unicode se procesan en los identificadores ([identificadores](lexical-structure.md#identifiers)), literales de caracteres ([literales de caracteres](lexical-structure.md#character-literals)) y literales de cadena regulares ([literalesdecadena](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="fd10f-161">No se procesa un carácter de escape Unicode en cualquier otra ubicación (por ejemplo, para formar un operador, un signo de puntuación o una palabra clave).</span><span class="sxs-lookup"><span data-stu-id="fd10f-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="fd10f-162">Una secuencia de escape Unicode representa el carácter Unicode único formado por el número hexadecimal que sigue el "`\u`"o"`\U`" caracteres.</span><span class="sxs-lookup"><span data-stu-id="fd10f-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="fd10f-163">Puesto que C# usa una codificación de 16 bits de los puntos de código Unicode en caracteres y valores de cadena, un carácter Unicode en el intervalo de u+10000 a 10FFFF U + no está permitido en un literal de carácter y se representa mediante un par suplente Unicode en un literal de cadena.</span><span class="sxs-lookup"><span data-stu-id="fd10f-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="fd10f-164">No se admiten caracteres Unicode con puntos de código por encima de 0x10FFFF.</span><span class="sxs-lookup"><span data-stu-id="fd10f-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="fd10f-165">No se realizan varias traducciones.</span><span class="sxs-lookup"><span data-stu-id="fd10f-165">Multiple translations are not performed.</span></span> <span data-ttu-id="fd10f-166">Por ejemplo, la cadena literal "`\u005Cu005C`"es equivalente a"`\u005C`"en lugar de"`\`".</span><span class="sxs-lookup"><span data-stu-id="fd10f-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="fd10f-167">El valor Unicode `\u005C` es el carácter "`\`".</span><span class="sxs-lookup"><span data-stu-id="fd10f-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="fd10f-168">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="fd10f-168">The example</span></span>
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
<span data-ttu-id="fd10f-169">se muestran varios usos de `\u0066`, que es la secuencia de escape para la letra "`f`".</span><span class="sxs-lookup"><span data-stu-id="fd10f-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="fd10f-170">El programa es equivalente a</span><span class="sxs-lookup"><span data-stu-id="fd10f-170">The program is equivalent to</span></span>
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

### <a name="identifiers"></a><span data-ttu-id="fd10f-171">Identificadores</span><span class="sxs-lookup"><span data-stu-id="fd10f-171">Identifiers</span></span>

<span data-ttu-id="fd10f-172">Las reglas para identificadores proporcionadas en esta sección se corresponden exactamente a las recomendaciones mediante el Unicode estándar anexo 31, excepto en que se permite el carácter de subrayado como carácter inicial (como era tradicional en el lenguaje de programación de C), son secuencias de escape Unicode se permite en identificadores y el "`@`" se permite el carácter como prefijo para habilitar las palabras clave que se usará como identificadores.</span><span class="sxs-lookup"><span data-stu-id="fd10f-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

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

<span data-ttu-id="fd10f-173">Para obtener información sobre las clases de caracteres Unicode que se mencionó anteriormente, vea el estándar Unicode, versión 3.0, sección 4.5.</span><span class="sxs-lookup"><span data-stu-id="fd10f-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="fd10f-174">Algunos ejemplos de identificadores válidos son "`identifier1`","`_identifier2`", y "`@if`".</span><span class="sxs-lookup"><span data-stu-id="fd10f-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="fd10f-175">Debe ser un identificador en un programa que cumplen las especificaciones en el formato canónico definido por la forma de normalización Unicode C, como se define en Unicode Standard anexo 15.</span><span class="sxs-lookup"><span data-stu-id="fd10f-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="fd10f-176">El comportamiento al encontrar un identificador no está en forma de normalización C es definido por la implementación; Sin embargo, no es necesario un diagnóstico.</span><span class="sxs-lookup"><span data-stu-id="fd10f-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="fd10f-177">El prefijo "`@`" permite el uso de palabras clave como identificadores, lo que resulta útil cuando se interactúa con otros lenguajes de programación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="fd10f-178">El carácter `@` no es realmente parte del identificador, por lo que el identificador podría verse en otros lenguajes como un identificador normal, sin el prefijo.</span><span class="sxs-lookup"><span data-stu-id="fd10f-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="fd10f-179">Un identificador con un `@` prefijo se denomina un ***identificador textual***.</span><span class="sxs-lookup"><span data-stu-id="fd10f-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="fd10f-180">El uso de la `@` prefijo para los identificadores que no son palabras clave está permitido, pero no se recomienda como una cuestión de estilo.</span><span class="sxs-lookup"><span data-stu-id="fd10f-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="fd10f-181">El ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd10f-181">The example:</span></span>
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
<span data-ttu-id="fd10f-182">define una clase denominada "`class`"con un método estático denominado"`static`"que toma un parámetro denominado"`bool`".</span><span class="sxs-lookup"><span data-stu-id="fd10f-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="fd10f-183">Tenga en cuenta que puesto que establece secuencias de escape Unicode no se permiten en las palabras clave, el token "`cl\u0061ss`"es un identificador, y es el mismo identificador que"`@class`".</span><span class="sxs-lookup"><span data-stu-id="fd10f-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="fd10f-184">Dos identificadores se consideran iguales si son idénticos después de aplicarán las transformaciones siguientes, en orden:</span><span class="sxs-lookup"><span data-stu-id="fd10f-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="fd10f-185">El prefijo "`@`", si se usa, se quita.</span><span class="sxs-lookup"><span data-stu-id="fd10f-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="fd10f-186">Cada *unicode_escape_sequence* se transforma en su carácter Unicode correspondiente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="fd10f-187">Cualquier *formatting_character*s se quitan.</span><span class="sxs-lookup"><span data-stu-id="fd10f-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="fd10f-188">Los identificadores que contienen dos consecutivos los caracteres de subrayado (`U+005F`) están reservados para su uso por la implementación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="fd10f-189">Por ejemplo, una implementación podría proporcionar palabras clave extendidas que comienzan por dos caracteres de subrayado.</span><span class="sxs-lookup"><span data-stu-id="fd10f-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="fd10f-190">Palabras clave</span><span class="sxs-lookup"><span data-stu-id="fd10f-190">Keywords</span></span>

<span data-ttu-id="fd10f-191">Un ***palabra clave*** es una secuencia similar a identificador de caracteres que está reservado y no se puede usar como un identificador excepto cuando va precedido por el `@` caracteres.</span><span class="sxs-lookup"><span data-stu-id="fd10f-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

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

<span data-ttu-id="fd10f-192">En algunos lugares de la gramática, identificadores específicos tienen un significado especial, pero no son palabras clave.</span><span class="sxs-lookup"><span data-stu-id="fd10f-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="fd10f-193">Estos identificadores se denominan a veces "palabras clave contextuales".</span><span class="sxs-lookup"><span data-stu-id="fd10f-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="fd10f-194">Por ejemplo, dentro de una declaración de propiedad, el "`get`"y"`set`" identificadores tienen un significado especial ([descriptores de acceso](classes.md#accessors)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="fd10f-195">Un identificador distinto de `get` o `set` no está permitido en estas ubicaciones, por lo que este uso no entra en conflicto con un uso de estas palabras como identificadores.</span><span class="sxs-lookup"><span data-stu-id="fd10f-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="fd10f-196">En otros casos, al igual que con el identificador "`var`" en declaraciones de variables locales con tipo implícito ([declaraciones de variable Local](statements.md#local-variable-declarations)), una palabra clave contextual puede entrar en conflicto con los nombres declarados.</span><span class="sxs-lookup"><span data-stu-id="fd10f-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="fd10f-197">En tales casos, el nombre declarado prevalece sobre el uso del identificador como una palabra clave contextual.</span><span class="sxs-lookup"><span data-stu-id="fd10f-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="fd10f-198">Literales</span><span class="sxs-lookup"><span data-stu-id="fd10f-198">Literals</span></span>

<span data-ttu-id="fd10f-199">Un ***literal*** es una representación de código fuente de un valor.</span><span class="sxs-lookup"><span data-stu-id="fd10f-199">A ***literal*** is a source code representation of a value.</span></span>

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

#### <a name="boolean-literals"></a><span data-ttu-id="fd10f-200">Literales booleanos</span><span class="sxs-lookup"><span data-stu-id="fd10f-200">Boolean literals</span></span>

<span data-ttu-id="fd10f-201">Hay dos valores literales booleanos: `true` y `false`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="fd10f-202">El tipo de un *boolean_literal* es `bool`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="fd10f-203">Literales enteros</span><span class="sxs-lookup"><span data-stu-id="fd10f-203">Integer literals</span></span>

<span data-ttu-id="fd10f-204">Los literales enteros se usan para escribir los valores de tipos `int`, `uint`, `long`, y `ulong`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="fd10f-205">Los literales enteros tienen dos formatos posibles: decimal y hexadecimal.</span><span class="sxs-lookup"><span data-stu-id="fd10f-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

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

<span data-ttu-id="fd10f-206">El tipo de un literal entero se determina como sigue:</span><span class="sxs-lookup"><span data-stu-id="fd10f-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="fd10f-207">Si el literal no tiene sufijo, es el primero de estos tipos en el que se puede representar su valor: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="fd10f-208">Si el sufijo literal `U` o `u`, es el primero de estos tipos en el que se puede representar su valor: `uint`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="fd10f-209">Si el sufijo literal `L` o `l`, es el primero de estos tipos en el que se puede representar su valor: `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="fd10f-210">Si el sufijo literal `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, o `lu`, es de tipo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="fd10f-211">Si el valor representado por un literal entero está fuera del intervalo de la `ulong` escribe, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="fd10f-212">Como una cuestión de estilo, se recomienda que "`L`"se puede usar en lugar de"`l`" al escribir literales de tipo `long`, ya que es fácil confundir la letra "`l`"con el dígito"`1`".</span><span class="sxs-lookup"><span data-stu-id="fd10f-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="fd10f-213">Para permitir el más pequeño posible `int` y `long` valores a escribirse como literales de entero decimal, existen las dos reglas siguientes:</span><span class="sxs-lookup"><span data-stu-id="fd10f-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="fd10f-214">Cuando un *decimal_integer_literal* con el valor 2147483648 (2 ^ 31) y no *integer_type_suffix* aparece como el token inmediatamente después de un símbolo de operador unario menos ([unario menos operador](expressions.md#unary-minus-operator)), el resultado es una constante de tipo `int` con el valor entre -2147483648 (-2 ^ 31).</span><span class="sxs-lookup"><span data-stu-id="fd10f-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="fd10f-215">En el resto de situaciones, este tipo una *decimal_integer_literal* es de tipo `uint`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="fd10f-216">Cuando un *decimal_integer_literal* con el valor 9223372036854775808 (2 ^ 63) y no *integer_type_suffix* o *integer_type_suffix* `L` o `l` aparece como el token inmediatamente después de un símbolo de operador unario menos ([operador unario menos](expressions.md#unary-minus-operator)), el resultado es una constante de tipo `long` con el valor de -9223372036854775808 (-2 ^ 63).</span><span class="sxs-lookup"><span data-stu-id="fd10f-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="fd10f-217">En el resto de situaciones, este tipo una *decimal_integer_literal* es de tipo `ulong`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="fd10f-218">Literales reales</span><span class="sxs-lookup"><span data-stu-id="fd10f-218">Real literals</span></span>

<span data-ttu-id="fd10f-219">Los literales reales se usan para escribir los valores de tipos `float`, `double`, y `decimal`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

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

<span data-ttu-id="fd10f-220">Si no hay ningún *real_type_suffix* se especifica, el tipo del literal real es `double`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="fd10f-221">En caso contrario, el sufijo de tipo real determina el tipo del literal real, como sigue:</span><span class="sxs-lookup"><span data-stu-id="fd10f-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="fd10f-222">Un literal real con el sufijo `F` o `f` es de tipo `float`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="fd10f-223">Por ejemplo, los literales `1f`, `1.5f`, `1e10f`, y `123.456F` son todas del tipo `float`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="fd10f-224">Un literal real con el sufijo `D` o `d` es de tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="fd10f-225">Por ejemplo, los literales `1d`, `1.5d`, `1e10d`, y `123.456D` son todas del tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="fd10f-226">Un literal real con el sufijo `M` o `m` es de tipo `decimal`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="fd10f-227">Por ejemplo, los literales `1m`, `1.5m`, `1e10m`, y `123.456M` son todas del tipo `decimal`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="fd10f-228">Este literal se convierte en un `decimal` valor tomando el valor exacto y, si es necesario, redondeando al valor que se puede representar más cercano mediante el redondeo bancario ([tipo decimal](types.md#the-decimal-type)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="fd10f-229">Cualquier escala evidente en el literal se conserva a menos que se redondea el valor o el valor es cero (en este caso, el inicio de sesión y la escala será 0).</span><span class="sxs-lookup"><span data-stu-id="fd10f-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="fd10f-230">Por lo tanto, el literal `2.900m` se analizará para formar el decimal con signo `0`, coeficiente `2900`y la escala `3`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="fd10f-231">Si el literal especificado no se puede representar en el tipo indicado, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="fd10f-232">El valor de un literal de tipo real `float` o `double` se determina mediante el uso de IEEE modo "redondeo al más cercano".</span><span class="sxs-lookup"><span data-stu-id="fd10f-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="fd10f-233">Tenga en cuenta que en un literal real, siempre son necesarios dígitos decimales después del separador decimal.</span><span class="sxs-lookup"><span data-stu-id="fd10f-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="fd10f-234">Por ejemplo, `1.3F` es un literal real pero `1.F` no es.</span><span class="sxs-lookup"><span data-stu-id="fd10f-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="fd10f-235">Literales de carácter</span><span class="sxs-lookup"><span data-stu-id="fd10f-235">Character literals</span></span>

<span data-ttu-id="fd10f-236">Un literal de carácter representa un único carácter y suele estar compuesto por un carácter de comillas, como en `'a'`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="fd10f-237">Nota: ¡La notación gramatical ANTLR hace lo siguiente confuso!</span><span class="sxs-lookup"><span data-stu-id="fd10f-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="fd10f-238">En ANTLR, al escribir `\'` significa una comilla `'`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="fd10f-239">Y al escribir `\\` significa una sola barra diagonal inversa `\`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="fd10f-240">Por lo tanto, la primera regla para un literal de carácter significa que comienza con una comilla simple, un carácter y, luego, una comilla simple.</span><span class="sxs-lookup"><span data-stu-id="fd10f-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="fd10f-241">Y las secuencias de escape sencillas posible once son `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

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

<span data-ttu-id="fd10f-242">Un carácter que sigue a un carácter de barra diagonal inversa (`\`) en un *carácter* debe ser uno de los siguientes caracteres: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="fd10f-243">De lo contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="fd10f-244">Una secuencia de escape hexadecimal representa un único carácter Unicode, con el valor está formado por el siguiente número hexadecimal "`\x`".</span><span class="sxs-lookup"><span data-stu-id="fd10f-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="fd10f-245">Si es mayor que el valor representado por un literal de carácter `U+FFFF`, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="fd10f-246">Una secuencia de escape de caracteres Unicode ([secuencias de escape de caracteres Unicode](lexical-structure.md#unicode-character-escape-sequences)) en un literal de caracteres debe estar en el intervalo `U+0000` a `U+FFFF`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="fd10f-247">Una secuencia de escape simples representa una codificación de caracteres Unicode, como se describe en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="fd10f-248">__Secuencia de escape__</span><span class="sxs-lookup"><span data-stu-id="fd10f-248">__Escape sequence__</span></span> | <span data-ttu-id="fd10f-249">__Nombre de carácter__</span><span class="sxs-lookup"><span data-stu-id="fd10f-249">__Character name__</span></span> | <span data-ttu-id="fd10f-250">__Codificación Unicode__</span><span class="sxs-lookup"><span data-stu-id="fd10f-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="fd10f-251">Comilla simple</span><span class="sxs-lookup"><span data-stu-id="fd10f-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="fd10f-252">Comilla doble</span><span class="sxs-lookup"><span data-stu-id="fd10f-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="fd10f-253">Barra diagonal inversa</span><span class="sxs-lookup"><span data-stu-id="fd10f-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="fd10f-254">Null</span><span class="sxs-lookup"><span data-stu-id="fd10f-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="fd10f-255">Alerta</span><span class="sxs-lookup"><span data-stu-id="fd10f-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="fd10f-256">Retroceso</span><span class="sxs-lookup"><span data-stu-id="fd10f-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="fd10f-257">Avance de página</span><span class="sxs-lookup"><span data-stu-id="fd10f-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="fd10f-258">Nueva línea</span><span class="sxs-lookup"><span data-stu-id="fd10f-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="fd10f-259">Retorno de carro</span><span class="sxs-lookup"><span data-stu-id="fd10f-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="fd10f-260">Tabulación horizontal</span><span class="sxs-lookup"><span data-stu-id="fd10f-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="fd10f-261">Tabulación vertical</span><span class="sxs-lookup"><span data-stu-id="fd10f-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="fd10f-262">El tipo de un *character_literal* es `char`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="fd10f-263">Literales de cadena</span><span class="sxs-lookup"><span data-stu-id="fd10f-263">String literals</span></span>

<span data-ttu-id="fd10f-264">C# admite dos formatos de literales de cadena: ***literales de cadena regulares*** y ***literales de cadena textual***.</span><span class="sxs-lookup"><span data-stu-id="fd10f-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="fd10f-265">Un literal de cadena regular consta de cero o más caracteres entre comillas dobles, como en `"hello"`y pueden incluir ambas secuencias de escape sencillas (como `\t` para el carácter de tabulación), hexadecimales y secuencias de escape Unicode.</span><span class="sxs-lookup"><span data-stu-id="fd10f-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="fd10f-266">Un literal de cadena textual consta de un `@` carácter seguido de un carácter de comillas dobles, cero o más caracteres y un carácter de comillas dobles de cierre.</span><span class="sxs-lookup"><span data-stu-id="fd10f-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="fd10f-267">Un ejemplo sencillo es `@"hello"`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="fd10f-268">En un literal de cadena textual, los caracteres entre los delimitadores se interpretan literalmente, la única excepción que se va a un *quote_escape_sequence*.</span><span class="sxs-lookup"><span data-stu-id="fd10f-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="fd10f-269">En concreto, las secuencias de escape simples, hexadecimales y secuencias de escape Unicode no se procesan en literales de cadena textual.</span><span class="sxs-lookup"><span data-stu-id="fd10f-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="fd10f-270">Un literal de cadena textual puede abarcar varias líneas.</span><span class="sxs-lookup"><span data-stu-id="fd10f-270">A verbatim string literal may span multiple lines.</span></span>

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

<span data-ttu-id="fd10f-271">Un carácter que sigue a un carácter de barra diagonal inversa (`\`) en un *regular_string_literal_character* debe ser uno de los siguientes caracteres: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="fd10f-272">De lo contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="fd10f-273">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="fd10f-273">The example</span></span>
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
<span data-ttu-id="fd10f-274">muestra una variedad de literales de cadena.</span><span class="sxs-lookup"><span data-stu-id="fd10f-274">shows a variety of string literals.</span></span> <span data-ttu-id="fd10f-275">La última cadena literal, `j`, es un literal de cadena textual que abarca varias líneas.</span><span class="sxs-lookup"><span data-stu-id="fd10f-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="fd10f-276">Los caracteres entre comillas, incluidos espacios en blanco como caracteres de nueva línea, se conservan literalmente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="fd10f-277">Puesto que una secuencia de escape hexadecimal puede tener un número variable de dígitos hexadecimales, el literal de cadena `"\x123"` contiene un único carácter con el valor hexadecimal 123.</span><span class="sxs-lookup"><span data-stu-id="fd10f-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="fd10f-278">Para crear una cadena que contiene el carácter con el valor hexadecimal 12 seguido del carácter 3, podría escribirse `"\x00123"` o `"\x12" + "3"` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="fd10f-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="fd10f-279">El tipo de un *string_literal* es `string`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="fd10f-280">Cada literal de cadena no produce necesariamente en una nueva instancia de cadena.</span><span class="sxs-lookup"><span data-stu-id="fd10f-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="fd10f-281">Cuando dos o más literales de cadena que son equivalentes según el operador de igualdad de cadenas ([operadores de igualdad de cadenas](expressions.md#string-equality-operators)) aparecen en el mismo programa, estos literales de cadena hacen referencia a la misma instancia de cadena.</span><span class="sxs-lookup"><span data-stu-id="fd10f-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="fd10f-282">Por ejemplo, el resultado producido por</span><span class="sxs-lookup"><span data-stu-id="fd10f-282">For instance, the output produced by</span></span>
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
<span data-ttu-id="fd10f-283">es `True` porque los dos literales hacen referencia a la misma instancia de cadena.</span><span class="sxs-lookup"><span data-stu-id="fd10f-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="fd10f-284">Literales de cadena interpolada</span><span class="sxs-lookup"><span data-stu-id="fd10f-284">Interpolated string literals</span></span>

<span data-ttu-id="fd10f-285">Literales de cadena interpolada son similares a los literales de cadena, pero contener orificios delimitadas por `{` y `}`, en la que pueden producirse las expresiones.</span><span class="sxs-lookup"><span data-stu-id="fd10f-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="fd10f-286">En tiempo de ejecución, las expresiones se evalúan con el objetivo de tener sus formularios textuales sustituye en la cadena en el lugar donde se produce el orificio.</span><span class="sxs-lookup"><span data-stu-id="fd10f-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="fd10f-287">La sintaxis y semántica de interpolación de cadenas que se describe en la sección ([cadenas interpoladas](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="fd10f-288">Como literales de cadena, los literales de cadena interpolada pueden ser normal o textual.</span><span class="sxs-lookup"><span data-stu-id="fd10f-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="fd10f-289">Literales de cadena regulares interpolada se delimitan mediante `$"` y `"`, y los literales de cadena interpolada textual se delimitan mediante `$@"` y `"`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="fd10f-290">Al igual que otros literales, análisis léxico de un literal de cadena interpolada inicialmente da como resultado un token único, según la siguiente gramática.</span><span class="sxs-lookup"><span data-stu-id="fd10f-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="fd10f-291">Sin embargo, antes de análisis sintáctico, el token único de un literal de cadena interpolada se divide en varios tokens para las partes de la cadena envolvente los huecos y los elementos de entrada que se producen en los orificios léxicamente se analizaron nuevo.</span><span class="sxs-lookup"><span data-stu-id="fd10f-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="fd10f-292">A su vez, esto puede producir más los literales de cadena interpolada para procesarse, pero, si léxicamente corregir, finalmente, dará lugar a una secuencia de tokens para el análisis sintáctico procesar.</span><span class="sxs-lookup"><span data-stu-id="fd10f-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

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

<span data-ttu-id="fd10f-293">Un *interpolated_string_literal* token se reinterprete como varios tokens y otros elementos de entrada como se indica a continuación, en orden de aparición en el *interpolated_string_literal*:</span><span class="sxs-lookup"><span data-stu-id="fd10f-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="fd10f-294">Las apariciones de los siguientes valores se reinterpretan como tokens individuales independientes: el interlineado `$` inicio de sesión, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*,  *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* y *interpolated_verbatim_string_end*.</span><span class="sxs-lookup"><span data-stu-id="fd10f-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="fd10f-295">Las apariciones de *regular_balanced_text* y *verbatim_balanced_text* entre estos se vuelven a procesar como un *input_section* ([análisis léxico ](lexical-structure.md#lexical-analysis)) y se reinterpretan como la secuencia de elementos de entrada resultante.</span><span class="sxs-lookup"><span data-stu-id="fd10f-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="fd10f-296">A su vez, estos pueden incluir tokens literales de cadena interpolada se reinterprete.</span><span class="sxs-lookup"><span data-stu-id="fd10f-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="fd10f-297">Análisis sintáctico se vuelven a combinar los tokens en un *interpolated_string_expression* ([cadenas interpoladas](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="fd10f-298">Ejemplos de tareas pendientes</span><span class="sxs-lookup"><span data-stu-id="fd10f-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="fd10f-299">El literal null</span><span class="sxs-lookup"><span data-stu-id="fd10f-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="fd10f-300">El *null_literal* puede convertirse implícitamente a un tipo de referencia o tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="fd10f-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="fd10f-301">Operadores y signos de puntuación</span><span class="sxs-lookup"><span data-stu-id="fd10f-301">Operators and punctuators</span></span>

<span data-ttu-id="fd10f-302">Hay varios tipos de operadores y signos de puntuación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="fd10f-303">Los operadores se utilizan en expresiones para describir las operaciones que implican a uno o más operandos.</span><span class="sxs-lookup"><span data-stu-id="fd10f-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="fd10f-304">Por ejemplo, la expresión `a + b` usa el `+` operador para agregar los dos operandos `a` y `b`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="fd10f-305">Signos de puntuación sirven para agrupar y separar.</span><span class="sxs-lookup"><span data-stu-id="fd10f-305">Punctuators are for grouping and separating.</span></span>

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

<span data-ttu-id="fd10f-306">La barra vertical en el *right_shift* y *right_shift_assignment* producciones se utilizan para indicar que, a diferencia de otras producciones en la gramática sintáctica, no hay ningún tipo de carácter (ni siquiera espacio en blanco) se permiten entre los tokens.</span><span class="sxs-lookup"><span data-stu-id="fd10f-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="fd10f-307">Se tratan estos producciones especialmente con el fin de habilitar la administración correcta de *type_parameter_list*s ([parámetros de tipo](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="fd10f-308">Las directivas de preprocesamiento</span><span class="sxs-lookup"><span data-stu-id="fd10f-308">Pre-processing directives</span></span>

<span data-ttu-id="fd10f-309">Las directivas de preprocesamiento proporcionan la capacidad de omitir condicionalmente secciones de archivos de origen, para informar del error y las condiciones de advertencia y distinguir las distintas regiones de código fuente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="fd10f-310">El término "directivas de preprocesamiento" se usa solo para mantener la coherencia con los lenguajes de programación C y C++.</span><span class="sxs-lookup"><span data-stu-id="fd10f-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="fd10f-311">En C#, no hay ningún paso de preprocesamiento independiente; las directivas de preprocesamiento se procesan como parte de la fase de análisis léxico.</span><span class="sxs-lookup"><span data-stu-id="fd10f-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

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

<span data-ttu-id="fd10f-312">Están disponibles las siguientes directivas de preprocesamiento:</span><span class="sxs-lookup"><span data-stu-id="fd10f-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="fd10f-313">`#define` y `#undef`, que se usan para definir y anular, respectivamente, símbolos de compilación condicional ([directivas de declaraciones](lexical-structure.md#declaration-directives)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="fd10f-314">`#if`, `#elif`, `#else`, y `#endif`, que se usan para omitir condicionalmente secciones de código fuente ([directivas de compilación condicional](lexical-structure.md#conditional-compilation-directives)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="fd10f-315">`#line`, que se usa para controlar los números de línea de errores y advertencias ([línea directivas](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="fd10f-316">`#error` y `#warning`, que se usan para emitir errores y advertencias, respectivamente ([directivas de diagnóstico](lexical-structure.md#diagnostic-directives)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="fd10f-317">`#region` y `#endregion`, que se usan para marcar explícitamente las secciones de código fuente ([las directivas de región](lexical-structure.md#region-directives)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="fd10f-318">`#pragma`, que se usa para especificar información contextual adicional para el compilador ([directivas Pragma](lexical-structure.md#pragma-directives)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="fd10f-319">Una directiva de preprocesamiento siempre ocupa una línea de código fuente independiente y siempre comienza con un `#` carácter y un nombre de directiva de procesamiento previo.</span><span class="sxs-lookup"><span data-stu-id="fd10f-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="fd10f-320">Puede haber un espacio en blanco antes de la `#` carácter y entre el `#` carácter y el nombre de la directiva.</span><span class="sxs-lookup"><span data-stu-id="fd10f-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="fd10f-321">Una línea de código fuente que contiene un `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, o `#endregion` directiva puede terminar con una sola línea de comentario.</span><span class="sxs-lookup"><span data-stu-id="fd10f-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="fd10f-322">Comentarios delimitados (el `/* */` estilo de comentarios) no se permiten en las líneas de código fuente que contiene las directivas de preprocesamiento.</span><span class="sxs-lookup"><span data-stu-id="fd10f-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="fd10f-323">Las directivas de preprocesamiento no son tokens y no forman parte de la gramática sintáctica de C#.</span><span class="sxs-lookup"><span data-stu-id="fd10f-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="fd10f-324">Sin embargo, las directivas de preprocesamiento pueden usarse para incluir o excluir las secuencias de tokens y pueden afectar el significado de un programa de C# de esa manera.</span><span class="sxs-lookup"><span data-stu-id="fd10f-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="fd10f-325">Por ejemplo, cuando se compila, el programa:</span><span class="sxs-lookup"><span data-stu-id="fd10f-325">For example, when compiled, the program:</span></span>
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
<span data-ttu-id="fd10f-326">resultados en la misma secuencia de tokens que el programa:</span><span class="sxs-lookup"><span data-stu-id="fd10f-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="fd10f-327">Por lo tanto, mientras que léxicamente, los dos programas son bastante diferentes, sintácticamente, son idénticos.</span><span class="sxs-lookup"><span data-stu-id="fd10f-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="fd10f-328">Símbolos de compilación condicional</span><span class="sxs-lookup"><span data-stu-id="fd10f-328">Conditional compilation symbols</span></span>

<span data-ttu-id="fd10f-329">La funcionalidad de compilación condicional proporcionada por el `#if`, `#elif`, `#else`, y `#endif` directivas se controla a través de las expresiones de preprocesamiento ([las expresiones de preprocesamiento](lexical-structure.md#pre-processing-expressions)) y símbolos de compilación condicional.</span><span class="sxs-lookup"><span data-stu-id="fd10f-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="fd10f-330">Símbolo de compilación condicional tiene dos estados posibles: ***definido*** o ***indefinido***.</span><span class="sxs-lookup"><span data-stu-id="fd10f-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="fd10f-331">Al principio del procesamiento léxico de un archivo de código fuente, un símbolo de compilación condicional está definido a menos que se ha definido explícitamente mediante un mecanismo externo (por ejemplo, una opción del compilador de línea de comandos).</span><span class="sxs-lookup"><span data-stu-id="fd10f-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="fd10f-332">Cuando un `#define` se procesa la directiva, el símbolo de compilación condicional nombrado en la directiva queda definido en el archivo de código fuente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="fd10f-333">El símbolo permanece definido hasta un `#undef` la directiva para que se procesa el mismo símbolo, o hasta que se alcanza el final del archivo de origen.</span><span class="sxs-lookup"><span data-stu-id="fd10f-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="fd10f-334">Una implicación de esto es que `#define` y `#undef` directivas en un archivo de origen no tienen ningún efecto en otros archivos de origen en el mismo programa.</span><span class="sxs-lookup"><span data-stu-id="fd10f-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="fd10f-335">Cuando se hace referencia en una expresión de preprocesamiento, un símbolo de compilación condicional definido tiene el valor booleano `true`, y un símbolo de compilación condicional no definido tiene el valor booleano `false`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="fd10f-336">No es necesario que los símbolos de compilación condicional se declaren explícitamente antes de que se hace referencia en las expresiones de preprocesamiento.</span><span class="sxs-lookup"><span data-stu-id="fd10f-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="fd10f-337">En su lugar, los símbolos no declarados simplemente no se definen y, por tanto, tienen el valor `false`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="fd10f-338">El espacio de nombres de símbolos de compilación condicional es distinto e independiente de todas las entidades con nombre en un programa de C#.</span><span class="sxs-lookup"><span data-stu-id="fd10f-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="fd10f-339">Solo se pueden hacer referencia a los símbolos de compilación condicional en `#define` y `#undef` directivas y en las expresiones de preprocesamiento.</span><span class="sxs-lookup"><span data-stu-id="fd10f-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="fd10f-340">Las expresiones de preprocesamiento</span><span class="sxs-lookup"><span data-stu-id="fd10f-340">Pre-processing expressions</span></span>

<span data-ttu-id="fd10f-341">Las expresiones de preprocesamiento pueden producirse en `#if` y `#elif` directivas.</span><span class="sxs-lookup"><span data-stu-id="fd10f-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="fd10f-342">Los operadores `!`, `==`, `!=`, `&&` y `||` se permiten en expresiones, el preprocesamiento y se pueden usar paréntesis para agrupar.</span><span class="sxs-lookup"><span data-stu-id="fd10f-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

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

<span data-ttu-id="fd10f-343">Cuando se hace referencia en una expresión de preprocesamiento, un símbolo de compilación condicional definido tiene el valor booleano `true`, y un símbolo de compilación condicional no definido tiene el valor booleano `false`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="fd10f-344">Evaluación de una expresión de preprocesamiento siempre produce un valor booleano.</span><span class="sxs-lookup"><span data-stu-id="fd10f-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="fd10f-345">Las reglas de evaluación de una expresión de preprocesamiento son las mismas que para una expresión constante ([expresiones constantes](expressions.md#constant-expressions)), excepto en que las únicas entidades definidas por el usuario que pueden hacer referencia a los símbolos de compilación condicional .</span><span class="sxs-lookup"><span data-stu-id="fd10f-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="fd10f-346">Directivas de declaraciones</span><span class="sxs-lookup"><span data-stu-id="fd10f-346">Declaration directives</span></span>

<span data-ttu-id="fd10f-347">Las directivas de declaración se usan para definir o anular la definición de símbolos de compilación condicional.</span><span class="sxs-lookup"><span data-stu-id="fd10f-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="fd10f-348">El procesamiento de un `#define` directiva hace que la definición, del símbolo de compilación condicional dado a partir de la línea de código fuente que sigue a la directiva.</span><span class="sxs-lookup"><span data-stu-id="fd10f-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="fd10f-349">Del mismo modo, el procesamiento de un `#undef` directiva hace que el símbolo de compilación condicional dado que quede sin definir, a partir de la línea de código fuente que sigue a la directiva.</span><span class="sxs-lookup"><span data-stu-id="fd10f-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="fd10f-350">Cualquier `#define` y `#undef` directivas en un archivo de origen deben aparecer antes del primer *token* ([Tokens](lexical-structure.md#tokens)) en el archivo de origen; en caso contrario, tiempo de compilación se produce un error.</span><span class="sxs-lookup"><span data-stu-id="fd10f-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="fd10f-351">En términos intuitivos, `#define` y `#undef` directivas deben preceder a cualquier "código real" en el archivo de origen.</span><span class="sxs-lookup"><span data-stu-id="fd10f-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="fd10f-352">El ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd10f-352">The example:</span></span>
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
<span data-ttu-id="fd10f-353">es válido porque la `#define` directivas preceden el primer token (la `namespace` palabra clave) en el archivo de origen.</span><span class="sxs-lookup"><span data-stu-id="fd10f-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="fd10f-354">En el ejemplo siguiente, se produce un error en tiempo de compilación porque un `#define` sigue el código real:</span><span class="sxs-lookup"><span data-stu-id="fd10f-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
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

<span data-ttu-id="fd10f-355">Un `#define` puede definir un símbolo de compilación condicional que ya esté definido, sin necesidad de ninguna intervención de que `#undef` para dicho símbolo.</span><span class="sxs-lookup"><span data-stu-id="fd10f-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="fd10f-356">El ejemplo siguiente define un símbolo de compilación condicional `A` y, a continuación, define.</span><span class="sxs-lookup"><span data-stu-id="fd10f-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="fd10f-357">Un `#undef` puede "Anular" un símbolo de compilación condicional no está definido.</span><span class="sxs-lookup"><span data-stu-id="fd10f-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="fd10f-358">El ejemplo siguiente define un símbolo de compilación condicional `A` y, a continuación, eliminar dicha definición dos veces; aunque la segunda `#undef` no tiene ningún efecto, aún es válido.</span><span class="sxs-lookup"><span data-stu-id="fd10f-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="fd10f-359">Directivas de compilación condicional</span><span class="sxs-lookup"><span data-stu-id="fd10f-359">Conditional compilation directives</span></span>

<span data-ttu-id="fd10f-360">Se usan las directivas de compilación condicional permiten incluir o excluir partes de un archivo de origen.</span><span class="sxs-lookup"><span data-stu-id="fd10f-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

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

<span data-ttu-id="fd10f-361">Tal y como indica la sintaxis, las directivas de compilación condicional deben escribirse como conjuntos que consta de, en orden, una `#if` directiva, cero o más `#elif` directivas, cero o uno `#else` directiva y un `#endif` directiva.</span><span class="sxs-lookup"><span data-stu-id="fd10f-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="fd10f-362">Entre las directivas son secciones condicionales de código fuente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="fd10f-363">Cada sección se controla mediante la directiva inmediatamente anterior.</span><span class="sxs-lookup"><span data-stu-id="fd10f-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="fd10f-364">Una sección condicional puede contener directivas de compilación condicional anidadas proporcionado estas directivas forman conjuntos completos.</span><span class="sxs-lookup"><span data-stu-id="fd10f-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="fd10f-365">Un *pp_conditional* selecciona al menos uno de los contenidos *conditional_section*s para el procesamiento léxico normal:</span><span class="sxs-lookup"><span data-stu-id="fd10f-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="fd10f-366">El *pp_expression*s de la `#if` y `#elif` directivas se evalúan en orden hasta que uno da como resultado `true`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="fd10f-367">Si el resultado de una expresión `true`, *conditional_section* está seleccionado de la directiva correspondiente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="fd10f-368">Si todos los *pp_expression*yield s `false`y si un `#else` directiva está presente, el *conditional_section* de la `#else` directiva está seleccionada.</span><span class="sxs-lookup"><span data-stu-id="fd10f-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="fd10f-369">En caso contrario, no *conditional_section* está seleccionada.</span><span class="sxs-lookup"><span data-stu-id="fd10f-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="fd10f-370">Seleccionado *conditional_section*, si existe, se procesa como normal *input_section*: código fuente incluido en la sección debe adherirse a la gramática léxica; los tokens se generan a partir del origen código en la sección. y las directivas de preprocesamiento en la sección tienen los efectos prescritos.</span><span class="sxs-lookup"><span data-stu-id="fd10f-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="fd10f-371">Los restantes *conditional_section*s, si hay alguno, se procesan como *skipped_section*s: excepto para las directivas de preprocesamiento, el código fuente en la sección no necesita cumplir el léxico gramática; no los tokens se generan desde el código fuente en la sección. y las directivas de preprocesamiento en la sección deben ser léxicamente correctas, pero no se procesan en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="fd10f-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="fd10f-372">Dentro de un *conditional_section* que se está procesando como una *skipped_section*cualquier anidado *conditional_section*s (contenidos en anidado `#if`... `#endif` y `#region`... `#endregion` construye) también se procesan como *skipped_section*s.</span><span class="sxs-lookup"><span data-stu-id="fd10f-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="fd10f-373">El ejemplo siguiente muestra las compilación condicional cómo pueden anidarse directivas:</span><span class="sxs-lookup"><span data-stu-id="fd10f-373">The following example illustrates how conditional compilation directives can nest:</span></span>
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

<span data-ttu-id="fd10f-374">Excepto para las directivas de preprocesamiento, el código fuente omitidos no está sujeto a análisis léxico.</span><span class="sxs-lookup"><span data-stu-id="fd10f-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="fd10f-375">Por ejemplo, la siguiente es válida, a pesar del comentario sin terminar en la `#else` sección:</span><span class="sxs-lookup"><span data-stu-id="fd10f-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
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

<span data-ttu-id="fd10f-376">Sin embargo, tenga en cuenta que las directivas de preprocesamiento se necesitan para ser léxicamente correcta incluso en las secciones omitidas del código fuente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="fd10f-377">Las directivas de preprocesamiento no se procesan cuando aparecen dentro de los elementos de entrada multilínea.</span><span class="sxs-lookup"><span data-stu-id="fd10f-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="fd10f-378">Por ejemplo, el programa:</span><span class="sxs-lookup"><span data-stu-id="fd10f-378">For example, the program:</span></span>
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
<span data-ttu-id="fd10f-379">resultados en la salida:</span><span class="sxs-lookup"><span data-stu-id="fd10f-379">results in the output:</span></span>
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="fd10f-380">En casos concretos, el conjunto de directivas de preprocesamiento que se procesa puede depender de la evaluación de la *pp_expression*.</span><span class="sxs-lookup"><span data-stu-id="fd10f-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="fd10f-381">El ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd10f-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="fd10f-382">siempre produce la misma secuencia de símbolos (`class` `Q` `{` `}`), independientemente de si `X` está definido.</span><span class="sxs-lookup"><span data-stu-id="fd10f-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="fd10f-383">Si `X` está definido, las directivas que se procesan sola son `#if` y `#endif`, debido a que el comentario de varias líneas.</span><span class="sxs-lookup"><span data-stu-id="fd10f-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="fd10f-384">Si `X` es undefined, a continuación, tres directivas (`#if`, `#else`, `#endif`) forman parte del conjunto de directivas.</span><span class="sxs-lookup"><span data-stu-id="fd10f-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="fd10f-385">Directivas de diagnóstico</span><span class="sxs-lookup"><span data-stu-id="fd10f-385">Diagnostic directives</span></span>

<span data-ttu-id="fd10f-386">Las directivas de diagnóstico se usan para generar mensajes de error y advertencia que se muestran en la misma manera que otros errores de tiempo de compilación y advertencias de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="fd10f-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

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

<span data-ttu-id="fd10f-387">El ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd10f-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="fd10f-388">siempre genera una advertencia ("revisión de código necesario antes de protegerlo") y produce un error en tiempo de compilación ("una compilación no puede ser comercial y depuración") si los símbolos condicionales `Debug` y `Retail` se definen.</span><span class="sxs-lookup"><span data-stu-id="fd10f-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="fd10f-389">Tenga en cuenta que un *pp_message* puede contener texto arbitrario; en concreto, no necesita contener tokens con formato correcto, como se muestra en las comillas simples en la palabra `can't`.</span><span class="sxs-lookup"><span data-stu-id="fd10f-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="fd10f-390">Directivas de región</span><span class="sxs-lookup"><span data-stu-id="fd10f-390">Region directives</span></span>

<span data-ttu-id="fd10f-391">Las directivas de región se usan para marcar explícitamente las regiones de código fuente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-391">The region directives are used to explicitly mark regions of source code.</span></span>

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

<span data-ttu-id="fd10f-392">No se adjunta ningún significado semántico a una región; las regiones están pensadas para su uso por el programador o mediante herramientas automatizadas para marcar una sección de código fuente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="fd10f-393">El mensaje especificado en un `#region` o `#endregion` directiva del mismo modo no tiene ningún significado semántico; sencillamente, se usa para identificar la región.</span><span class="sxs-lookup"><span data-stu-id="fd10f-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="fd10f-394">Coincidencia `#region` y `#endregion` directivas pueden tener diferentes *pp_message*s.</span><span class="sxs-lookup"><span data-stu-id="fd10f-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="fd10f-395">El procesamiento léxico de una región:</span><span class="sxs-lookup"><span data-stu-id="fd10f-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="fd10f-396">corresponde exactamente con el procesamiento léxico de una directiva de compilación condicional del formulario:</span><span class="sxs-lookup"><span data-stu-id="fd10f-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="fd10f-397">Directivas de línea</span><span class="sxs-lookup"><span data-stu-id="fd10f-397">Line directives</span></span>

<span data-ttu-id="fd10f-398">Las directivas de línea pueden usarse para modificar los números de línea y nombres de archivo de origen que son notificados por el compilador en la salida como advertencias y errores, y que se usan los atributos de información de llamador ([atributos de información del llamador](attributes.md#caller-info-attributes)).</span><span class="sxs-lookup"><span data-stu-id="fd10f-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="fd10f-399">Las directivas de línea se usan con más frecuencia en herramientas de metaprogramación que generan código fuente C# a partir de otras entradas de texto.</span><span class="sxs-lookup"><span data-stu-id="fd10f-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

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

<span data-ttu-id="fd10f-400">Cuando no hay ninguna `#line` directivas están presentes, el compilador muestra números de línea y nombres de archivo de origen en su salida.</span><span class="sxs-lookup"><span data-stu-id="fd10f-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="fd10f-401">Al procesar un `#line` directiva que incluye un *line_indicator* que no es `default`, el compilador trata la línea después de la directiva como si tuviera el número de línea especificado (y el nombre de archivo, si se especifica).</span><span class="sxs-lookup"><span data-stu-id="fd10f-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="fd10f-402">Un `#line default` directiva invierte el efecto de todas las directivas #line anteriores.</span><span class="sxs-lookup"><span data-stu-id="fd10f-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="fd10f-403">El compilador notifica información de línea es true para las líneas siguientes, precisamente como si no `#line` directivas se hubiera procesado.</span><span class="sxs-lookup"><span data-stu-id="fd10f-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="fd10f-404">Un `#line hidden` directiva no tiene ningún efecto en el archivo y números de línea que se indica en el error mensajes, pero afecta a la depuración de nivel de origen.</span><span class="sxs-lookup"><span data-stu-id="fd10f-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="fd10f-405">Al depurar, todas las líneas entre un `#line hidden` directiva y la subsiguiente `#line` directiva (que no es `#line hidden`) no tienen ninguna información de número de línea.</span><span class="sxs-lookup"><span data-stu-id="fd10f-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="fd10f-406">Al recorrer el código en el depurador, estas líneas se omitirá completamente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="fd10f-407">Tenga en cuenta que un *file_name* difiere de un literal de cadena normal en que no se procesan los caracteres de escape; el "`\`" carácter simplemente designa un carácter de barra diagonal inversa ordinaria dentro de un *file_name*.</span><span class="sxs-lookup"><span data-stu-id="fd10f-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="fd10f-408">Directivas pragma</span><span class="sxs-lookup"><span data-stu-id="fd10f-408">Pragma directives</span></span>

<span data-ttu-id="fd10f-409">El `#pragma` preprocesamiento de directiva se usa para especificar información contextual adicional para el compilador.</span><span class="sxs-lookup"><span data-stu-id="fd10f-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="fd10f-410">La información proporcionada en un `#pragma` directiva nunca cambiará la semántica del programa.</span><span class="sxs-lookup"><span data-stu-id="fd10f-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="fd10f-411">C# proporciona `#pragma` directivas para controlar las advertencias del compilador.</span><span class="sxs-lookup"><span data-stu-id="fd10f-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="fd10f-412">Las versiones futuras del lenguaje pueden incluir adicionales `#pragma` directivas.</span><span class="sxs-lookup"><span data-stu-id="fd10f-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="fd10f-413">Para garantizar la interoperabilidad con otros compiladores de C#, el compilador de C# de Microsoft no emite errores de compilación si no se conoce `#pragma` directivas; este tipo no de las directivas pero generan advertencias.</span><span class="sxs-lookup"><span data-stu-id="fd10f-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="fd10f-414">Advertencia pragma</span><span class="sxs-lookup"><span data-stu-id="fd10f-414">Pragma warning</span></span>

<span data-ttu-id="fd10f-415">El `#pragma warning` directiva se usa para deshabilitar o restaurar todos o mensajes de un conjunto determinado de advertencia durante la compilación del texto del programa subsiguiente.</span><span class="sxs-lookup"><span data-stu-id="fd10f-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

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

<span data-ttu-id="fd10f-416">Un `#pragma warning` directiva que omita la lista de advertencias afecta a todas las advertencias.</span><span class="sxs-lookup"><span data-stu-id="fd10f-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="fd10f-417">Un `#pragma warning` directiva el incluye una lista de advertencias afecta solo a esas advertencias que se especifican en la lista.</span><span class="sxs-lookup"><span data-stu-id="fd10f-417">A `#pragma warning` directive the includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="fd10f-418">Un `#pragma warning disable` directiva deshabilita todas o el conjunto dado de advertencias.</span><span class="sxs-lookup"><span data-stu-id="fd10f-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="fd10f-419">Un `#pragma warning restore` directiva restaura todas o el conjunto dado de advertencias al estado que estaba en efecto al principio de la unidad de compilación.</span><span class="sxs-lookup"><span data-stu-id="fd10f-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="fd10f-420">Tenga en cuenta que si una advertencia concreta se deshabilitó externamente, un `#pragma warning restore` (sea para todos o la advertencia concreta) no volverá a habilitar dicha advertencia.</span><span class="sxs-lookup"><span data-stu-id="fd10f-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="fd10f-421">El ejemplo siguiente muestra el uso de `#pragma warning` deshabilitar temporalmente la advertencia notificados cuando obsoleto se hace referencia a los miembros, con el número de advertencia del compilador de C# de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="fd10f-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
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
