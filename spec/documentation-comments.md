# <a name="documentation-comments"></a><span data-ttu-id="5276e-101">Comentarios de documentación</span><span class="sxs-lookup"><span data-stu-id="5276e-101">Documentation comments</span></span>

<span data-ttu-id="5276e-102">C# proporciona un mecanismo para que los programadores puedan documentar el código mediante una sintaxis de comentario especial que contiene el texto XML.</span><span class="sxs-lookup"><span data-stu-id="5276e-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="5276e-103">En los archivos de código fuente, comentarios con un formato determinado pueden usarse para dirigir una herramienta para generar XML desde estos comentarios y los elementos de código fuente que preceden.</span><span class="sxs-lookup"><span data-stu-id="5276e-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="5276e-104">Los comentarios con esta sintaxis se denominan ***comentarios de documentación***.</span><span class="sxs-lookup"><span data-stu-id="5276e-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="5276e-105">Debe preceder inmediatamente a un tipo definido por el usuario (por ejemplo, una clase, un delegado o una interfaz) o un miembro (por ejemplo, un campo, evento, propiedad o método).</span><span class="sxs-lookup"><span data-stu-id="5276e-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="5276e-106">La herramienta de generación de XML se denomina el ***generador de documentación***.</span><span class="sxs-lookup"><span data-stu-id="5276e-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="5276e-107">(Este generador podría ser, pero no es necesario, el propio compilador de C#). El resultado producido por el generador de documentación se denomina el ***archivo de documentación***.</span><span class="sxs-lookup"><span data-stu-id="5276e-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="5276e-108">Un archivo de documentación se usa como entrada para un ***Visor de la documentación***; una herramienta diseñada para producir algún tipo de presentación visual de la información de tipo y su documentación asociada.</span><span class="sxs-lookup"><span data-stu-id="5276e-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="5276e-109">Esta especificación sugiere un conjunto de etiquetas que se usará en los comentarios de documentación, pero no se requiere el uso de estas etiquetas y otras etiquetas se pueden usar si lo desea, se siguen larga las reglas de XML con formato correcto.</span><span class="sxs-lookup"><span data-stu-id="5276e-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="5276e-110">Introducción</span><span class="sxs-lookup"><span data-stu-id="5276e-110">Introduction</span></span>

<span data-ttu-id="5276e-111">Comentarios con un formato especial pueden usarse para dirigir una herramienta para generar XML desde estos comentarios y los elementos de código fuente que preceden.</span><span class="sxs-lookup"><span data-stu-id="5276e-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="5276e-112">Estos comentarios son comentarios de una línea que comienzan con tres barras diagonales (`///`), o delimitado por comentarios que comienzan con una barra diagonal y dos asteriscos (`/**`).</span><span class="sxs-lookup"><span data-stu-id="5276e-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="5276e-113">Debe preceder inmediatamente a un tipo definido por el usuario (por ejemplo, una clase, un delegado o una interfaz) o un miembro (por ejemplo, un campo, evento, propiedad o método) que hacen referencia los comentarios.</span><span class="sxs-lookup"><span data-stu-id="5276e-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="5276e-114">Atributo secciones ([especificación de atributos](attributes.md#attribute-specification)) se consideran parte de las declaraciones, por lo que los comentarios de documentación deben preceder a los atributos aplicados a un tipo o miembro.</span><span class="sxs-lookup"><span data-stu-id="5276e-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="5276e-115">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="5276e-116">En un *single_line_doc_comment*, si hay un *espacio en blanco* siguiente carácter el `///` caracteres en cada uno de los *single_line_doc_comment*s adyacentes a la actual *single_line_doc_comment*, a continuación, que *espacio en blanco* carácter no se incluye en la salida XML.</span><span class="sxs-lookup"><span data-stu-id="5276e-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="5276e-117">En un comentario-delimitados-doc, si el primer carácter que no sean espacios en blanco en la segunda línea es un asterisco y el mismo patrón de caracteres en blanco opcional y un carácter de asterisco se repiten al principio de cada uno de la línea en el comentario-delimitados-doc, a continuación, no se incluyen los caracteres del patrón repetido en la salida XML.</span><span class="sxs-lookup"><span data-stu-id="5276e-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="5276e-118">El patrón puede incluir caracteres de espacio en blanco después de, así como antes, el carácter de asterisco.</span><span class="sxs-lookup"><span data-stu-id="5276e-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="5276e-119">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-119">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>
///
public class Point 
{
    /// <summary>method <c>draw</c> renders the point.</summary>
    void draw() {...}
}
```

<span data-ttu-id="5276e-120">El texto dentro de los comentarios de documentación debe tener un formato correcto según las reglas de XML (https://www.w3.org/TR/REC-xml).</span><span class="sxs-lookup"><span data-stu-id="5276e-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="5276e-121">Si el XML está enfermo formado, se genera una advertencia y el archivo de documentación incluirá un comentario que indica que se detectó un error.</span><span class="sxs-lookup"><span data-stu-id="5276e-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="5276e-122">Aunque los desarrolladores son gratuitos crear su propio conjunto de etiquetas, se define un conjunto recomendado de [etiquetas recomendadas](documentation-comments.md#recommended-tags).</span><span class="sxs-lookup"><span data-stu-id="5276e-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="5276e-123">Algunas de las etiquetas recomendadas tienen significados especiales:</span><span class="sxs-lookup"><span data-stu-id="5276e-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="5276e-124">El `<param>` etiqueta a la que se usa para describir los parámetros.</span><span class="sxs-lookup"><span data-stu-id="5276e-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="5276e-125">Si se utiliza dicha etiqueta, el generador de documentación debe comprobar que el parámetro especificado existe y que todos los parámetros se describen en los comentarios de documentación.</span><span class="sxs-lookup"><span data-stu-id="5276e-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="5276e-126">Si se produce un error en dicha comprobación, el generador de documentación emite una advertencia.</span><span class="sxs-lookup"><span data-stu-id="5276e-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="5276e-127">El atributo `cref` se puede asociar a cualquier etiqueta para proporcionar una referencia a un elemento de código.</span><span class="sxs-lookup"><span data-stu-id="5276e-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="5276e-128">El generador de documentación debe comprobar la existencia de este elemento de código.</span><span class="sxs-lookup"><span data-stu-id="5276e-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="5276e-129">Si se produce un error en la comprobación, el generador de documentación emite una advertencia.</span><span class="sxs-lookup"><span data-stu-id="5276e-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="5276e-130">Al buscar un nombre que se describe en un `cref` atributo, el generador de documentación debe respetar la visibilidad del espacio de nombres según `using` instrucciones que aparecen en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="5276e-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="5276e-131">Los elementos de código que son genéricos, la sintaxis genérica normal (es decir, "`List<T>`") no se puede usar porque no genera un XML no válido.</span><span class="sxs-lookup"><span data-stu-id="5276e-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="5276e-132">Se pueden usar llaves en lugar de corchetes (es decir, "`List{T}`"), o se puede usar la sintaxis de escape XML (es decir, "`List&lt;T&gt;`").</span><span class="sxs-lookup"><span data-stu-id="5276e-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="5276e-133">El `<summary>` etiqueta está pensada para usarse con un visor de documentación para mostrar información adicional sobre un tipo o miembro.</span><span class="sxs-lookup"><span data-stu-id="5276e-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="5276e-134">El `<include>` tag incluye información de un archivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="5276e-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="5276e-135">Observe que el archivo de documentación no proporciona información completa sobre los tipos y miembros (por ejemplo, no contiene ninguna información de tipos).</span><span class="sxs-lookup"><span data-stu-id="5276e-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="5276e-136">Para obtener información acerca de un tipo o miembro, el archivo de documentación debe usarse junto con la reflexión en el tipo o miembro reales.</span><span class="sxs-lookup"><span data-stu-id="5276e-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="5276e-137">Etiquetas recomendadas</span><span class="sxs-lookup"><span data-stu-id="5276e-137">Recommended tags</span></span>

<span data-ttu-id="5276e-138">El generador de documentación debe aceptar y procesar cualquier etiqueta válida según las reglas de XML.</span><span class="sxs-lookup"><span data-stu-id="5276e-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="5276e-139">Las siguientes etiquetas proporcionan funcionalidad de uso común en la documentación de usuario.</span><span class="sxs-lookup"><span data-stu-id="5276e-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="5276e-140">(Por supuesto, otras etiquetas son posibles).</span><span class="sxs-lookup"><span data-stu-id="5276e-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="5276e-141">__Tag__</span><span class="sxs-lookup"><span data-stu-id="5276e-141">__Tag__</span></span>          | <span data-ttu-id="5276e-142">__Sección__</span><span class="sxs-lookup"><span data-stu-id="5276e-142">__Section__</span></span>                                            | <span data-ttu-id="5276e-143">__Propósito__</span><span class="sxs-lookup"><span data-stu-id="5276e-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="5276e-144">Establecer el texto en una fuente como código</span><span class="sxs-lookup"><span data-stu-id="5276e-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="5276e-145">Establezca una o varias líneas de salida de código o programa de origen</span><span class="sxs-lookup"><span data-stu-id="5276e-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="5276e-146">Indicar un ejemplo</span><span class="sxs-lookup"><span data-stu-id="5276e-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="5276e-147">Identifica las excepciones que puede iniciar un método</span><span class="sxs-lookup"><span data-stu-id="5276e-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="5276e-148">Incluye el XML desde un archivo externo</span><span class="sxs-lookup"><span data-stu-id="5276e-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="5276e-149">Crear una tabla o lista</span><span class="sxs-lookup"><span data-stu-id="5276e-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="5276e-150">Permitir la estructura que se agregarán a texto</span><span class="sxs-lookup"><span data-stu-id="5276e-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="5276e-151">Describe un parámetro para un método o constructor</span><span class="sxs-lookup"><span data-stu-id="5276e-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="5276e-152">Identificar que una palabra es un nombre de parámetro</span><span class="sxs-lookup"><span data-stu-id="5276e-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="5276e-153">La accesibilidad de seguridad de un miembro de documento</span><span class="sxs-lookup"><span data-stu-id="5276e-153">Document the security accessibility of a member</span></span>        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | <span data-ttu-id="5276e-154">Describe información adicional sobre un tipo</span><span class="sxs-lookup"><span data-stu-id="5276e-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="5276e-155">Describir el valor devuelto de un método</span><span class="sxs-lookup"><span data-stu-id="5276e-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="5276e-156">Especifique un vínculo</span><span class="sxs-lookup"><span data-stu-id="5276e-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="5276e-157">Generar una entrada en Vea también</span><span class="sxs-lookup"><span data-stu-id="5276e-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="5276e-158">Describir un tipo o miembro de un tipo</span><span class="sxs-lookup"><span data-stu-id="5276e-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="5276e-159">Describir una propiedad</span><span class="sxs-lookup"><span data-stu-id="5276e-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="5276e-160">Describir un parámetro de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="5276e-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="5276e-161">Identificar que una palabra es un nombre de parámetro de tipo</span><span class="sxs-lookup"><span data-stu-id="5276e-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="5276e-162">Esta etiqueta ofrece un mecanismo para indicar que un fragmento de texto dentro de una descripción debe establecerse en una fuente como los utilizados para un bloque de código especial.</span><span class="sxs-lookup"><span data-stu-id="5276e-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="5276e-163">Para las líneas de código real, utilice `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="5276e-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="5276e-164">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="5276e-165">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="5276e-166">Esta etiqueta se usa para establecer una o varias líneas de salida de código o programa de origen en alguna fuente especial.</span><span class="sxs-lookup"><span data-stu-id="5276e-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="5276e-167">Para pequeños fragmentos de código en texto narrativo, utilice `<c>` ([`<c>`](documentation-comments.md#c)).</span><span class="sxs-lookup"><span data-stu-id="5276e-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="5276e-168">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="5276e-169">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-169">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// <example>For example:
/// <code>
///    Point p = new Point(3,5);
///    p.Translate(-1,3);
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>

public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}   
```

### `<example>`

<span data-ttu-id="5276e-170">Esta etiqueta permite que el código de ejemplo de un comentario, para especificar cómo se puede usar un método u otro miembro de biblioteca.</span><span class="sxs-lookup"><span data-stu-id="5276e-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="5276e-171">Normalmente, esto también implica la utilización de la etiqueta `<code>` ([`<code>`](documentation-comments.md#code)) también.</span><span class="sxs-lookup"><span data-stu-id="5276e-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="5276e-172">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="5276e-173">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-173">__Example:__</span></span>

<span data-ttu-id="5276e-174">Consulte `<code>` ([`<code>`](documentation-comments.md#code)) para obtener un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="5276e-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="5276e-175">Esta etiqueta proporciona una manera de documentar las excepciones que se puede producir un método.</span><span class="sxs-lookup"><span data-stu-id="5276e-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="5276e-176">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="5276e-177">donde</span><span class="sxs-lookup"><span data-stu-id="5276e-177">where</span></span>

* <span data-ttu-id="5276e-178">`member` es el nombre de un miembro.</span><span class="sxs-lookup"><span data-stu-id="5276e-178">`member` is the name of a member.</span></span> <span data-ttu-id="5276e-179">El generador de documentación comprueba si el miembro especificado existe y traduce `member` al nombre de elemento canónico en el archivo de documentación.</span><span class="sxs-lookup"><span data-stu-id="5276e-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="5276e-180">`description` es una descripción de las circunstancias en que se produce la excepción.</span><span class="sxs-lookup"><span data-stu-id="5276e-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="5276e-181">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-181">__Example:__</span></span>

```csharp
public class DataBaseOperations
{
    /// <exception cref="MasterFileFormatCorruptException"></exception>
    /// <exception cref="MasterFileLockedOpenException"></exception>
    public static void ReadRecord(int flag) {
        if (flag == 1)
            throw new MasterFileFormatCorruptException();
        else if (flag == 2)
            throw new MasterFileLockedOpenException();
        // ...
    } 
}
```

### `<include>`

<span data-ttu-id="5276e-182">Esta etiqueta permite incluir información de un documento XML que es externo al archivo de código fuente.</span><span class="sxs-lookup"><span data-stu-id="5276e-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="5276e-183">El archivo externo debe ser un documento XML bien formado, y una expresión XPath se aplica a ese documento para especificar qué XML de ese documento para incluir.</span><span class="sxs-lookup"><span data-stu-id="5276e-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="5276e-184">El `<include>` etiqueta, a continuación, se reemplaza por el XML del documento externo seleccionado.</span><span class="sxs-lookup"><span data-stu-id="5276e-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="5276e-185">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="5276e-186">donde</span><span class="sxs-lookup"><span data-stu-id="5276e-186">where</span></span>

* <span data-ttu-id="5276e-187">`filename` es el nombre de archivo de un archivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="5276e-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="5276e-188">El nombre de archivo se interpreta en relación con el archivo que contiene la etiqueta de inclusión.</span><span class="sxs-lookup"><span data-stu-id="5276e-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="5276e-189">`xpath` es una expresión XPath que selecciona parte del código XML en el archivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="5276e-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="5276e-190">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-190">__Example:__</span></span>

<span data-ttu-id="5276e-191">Si el código fuente contiene una declaración como:</span><span class="sxs-lookup"><span data-stu-id="5276e-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="5276e-192">y el archivo externo "docs.xml" tuviera el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="5276e-192">and the external file "docs.xml" had the following contents:</span></span>

```xml
<?xml version="1.0"?>
<extradoc>
  <class name="IntList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
  <class name="StringList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
</extradoc>
```

<span data-ttu-id="5276e-193">a continuación, la documentación del misma es el resultado como si el código fuente contenidos:</span><span class="sxs-lookup"><span data-stu-id="5276e-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="5276e-194">Esta etiqueta se utiliza para crear una lista o tabla de elementos.</span><span class="sxs-lookup"><span data-stu-id="5276e-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="5276e-195">Puede contener un `<listheader>` bloque para definir la fila de encabezado de una tabla o una definición de lista.</span><span class="sxs-lookup"><span data-stu-id="5276e-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="5276e-196">(Cuando se define una tabla, solo una entrada para `term` en el encabezado debe proporcionarse.)</span><span class="sxs-lookup"><span data-stu-id="5276e-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="5276e-197">Cada elemento de la lista se especifica con un `<item>` bloque.</span><span class="sxs-lookup"><span data-stu-id="5276e-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="5276e-198">Al crear una lista de definiciones, ambos `term` y `description` debe especificarse.</span><span class="sxs-lookup"><span data-stu-id="5276e-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="5276e-199">Sin embargo, para una tabla, lista con viñetas o lista numerada, solo `description` debe especificarse.</span><span class="sxs-lookup"><span data-stu-id="5276e-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="5276e-200">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-200">__Syntax:__</span></span>

```xml
<list type="bullet" | "number" | "table">
   <listheader>
      <term>term</term>
      <description>*description*</description>
   </listheader>
   <item>
      <term>term</term>
      <description>*description*</description>
   </item>
    ...
   <item>
      <term>term</term>
      <description>description</description>
   </item>
</list>
```

<span data-ttu-id="5276e-201">donde</span><span class="sxs-lookup"><span data-stu-id="5276e-201">where</span></span>

* <span data-ttu-id="5276e-202">`term` es el término para definir, cuya definición se encuentra en `description`.</span><span class="sxs-lookup"><span data-stu-id="5276e-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="5276e-203">`description` es un elemento de viñeta o una lista numerada o la definición de un `term`.</span><span class="sxs-lookup"><span data-stu-id="5276e-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="5276e-204">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-204">__Example:__</span></span>

```csharp
public class MyClass
{
    /// <summary>Here is an example of a bulleted list:
    /// <list type="bullet">
    /// <item>
    /// <description>Item 1.</description>
    /// </item>
    /// <item>
    /// <description>Item 2.</description>
    /// </item>
    /// </list>
    /// </summary>
    public static void Main () {
        // ...
    }
}
```

### `<para>`

<span data-ttu-id="5276e-205">Esta etiqueta es para su uso dentro de otras etiquetas, como `<summary>` ([`<remark>`](documentation-comments.md#remark)) o `<returns>` ([`<returns>`](documentation-comments.md#returns)) y permite agregar al texto de la estructura.</span><span class="sxs-lookup"><span data-stu-id="5276e-205">This tag is for use inside other tags, such as `<summary>` ([`<remark>`](documentation-comments.md#remark)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="5276e-206">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="5276e-207">donde `content` es el texto del párrafo.</span><span class="sxs-lookup"><span data-stu-id="5276e-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="5276e-208">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-208">__Example:__</span></span>

```csharp
/// <summary>This is the entry point of the Point class testing program.
/// <para>This program tests each method and operator, and
/// is intended to be run after any non-trivial maintenance has
/// been performed on the Point class.</para></summary>
public static void Main() {
    // ...
}
```

### `<param>`

<span data-ttu-id="5276e-209">Esta etiqueta se utiliza para describir un parámetro para un método, constructor o indizador.</span><span class="sxs-lookup"><span data-stu-id="5276e-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="5276e-210">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="5276e-211">donde</span><span class="sxs-lookup"><span data-stu-id="5276e-211">where</span></span>

* <span data-ttu-id="5276e-212">`name` Es el nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="5276e-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="5276e-213">`description` es una descripción del parámetro.</span><span class="sxs-lookup"><span data-stu-id="5276e-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="5276e-214">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-214">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <param name="xor">the new x-coordinate.</param>
/// <param name="yor">the new y-coordinate.</param>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<paramref>`

<span data-ttu-id="5276e-215">Esta etiqueta se utiliza para indicar que una palabra es un parámetro.</span><span class="sxs-lookup"><span data-stu-id="5276e-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="5276e-216">El archivo de documentación se puede procesar para dar formato a este parámetro de alguna manera distinta.</span><span class="sxs-lookup"><span data-stu-id="5276e-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="5276e-217">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="5276e-218">donde `name` es el nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="5276e-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="5276e-219">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-219">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to
///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
/// <param name="xor">the new Point's x-coordinate.</param>
/// <param name="yor">the new Point's y-coordinate.</param>

public Point(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<permission>`

<span data-ttu-id="5276e-220">Esta etiqueta permite la accesibilidad de seguridad de un miembro que va a documentarse.</span><span class="sxs-lookup"><span data-stu-id="5276e-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="5276e-221">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="5276e-222">donde</span><span class="sxs-lookup"><span data-stu-id="5276e-222">where</span></span>

* <span data-ttu-id="5276e-223">`member` es el nombre de un miembro.</span><span class="sxs-lookup"><span data-stu-id="5276e-223">`member` is the name of a member.</span></span> <span data-ttu-id="5276e-224">El generador de documentación comprueba si el elemento de código dado existe y traduce *miembro* al nombre de elemento canónico en el archivo de documentación.</span><span class="sxs-lookup"><span data-stu-id="5276e-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="5276e-225">`description` es una descripción del acceso al miembro.</span><span class="sxs-lookup"><span data-stu-id="5276e-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="5276e-226">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

<span data-ttu-id="5276e-227">Esta etiqueta se utiliza para especificar información adicional sobre un tipo.</span><span class="sxs-lookup"><span data-stu-id="5276e-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="5276e-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) para describir el propio tipo y los miembros de un tipo.)</span><span class="sxs-lookup"><span data-stu-id="5276e-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="5276e-229">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-229">__Syntax:__</span></span>

```xml
<remark>description</remark>
```

<span data-ttu-id="5276e-230">donde `description` es el texto del comentario.</span><span class="sxs-lookup"><span data-stu-id="5276e-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="5276e-231">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remark>Uses polar coordinates</remark>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="5276e-232">Esta etiqueta se utiliza para describir el valor devuelto de un método.</span><span class="sxs-lookup"><span data-stu-id="5276e-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="5276e-233">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="5276e-234">donde `description` es una descripción del valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="5276e-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="5276e-235">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="5276e-236">Esta etiqueta permite un vínculo al especificarse dentro del texto.</span><span class="sxs-lookup"><span data-stu-id="5276e-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="5276e-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) para indicar el texto que va a aparecer en una sección Vea también.</span><span class="sxs-lookup"><span data-stu-id="5276e-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="5276e-238">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="5276e-239">donde `member` es el nombre de un miembro.</span><span class="sxs-lookup"><span data-stu-id="5276e-239">where `member` is the name of a member.</span></span> <span data-ttu-id="5276e-240">El generador de documentación comprueba si el elemento de código dado existe y cambia *miembro* al nombre de elemento en el archivo de documentación generado.</span><span class="sxs-lookup"><span data-stu-id="5276e-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="5276e-241">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-241">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <see cref="Translate"/>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}

/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}
```

### `<seealso>`

<span data-ttu-id="5276e-242">Esta etiqueta permite una entrada que se generará para la sección Vea también.</span><span class="sxs-lookup"><span data-stu-id="5276e-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="5276e-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) para especificar un vínculo desde dentro del texto.</span><span class="sxs-lookup"><span data-stu-id="5276e-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="5276e-244">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="5276e-245">donde `member` es el nombre de un miembro.</span><span class="sxs-lookup"><span data-stu-id="5276e-245">where `member` is the name of a member.</span></span> <span data-ttu-id="5276e-246">El generador de documentación comprueba si el elemento de código dado existe y cambia *miembro* al nombre de elemento en el archivo de documentación generado.</span><span class="sxs-lookup"><span data-stu-id="5276e-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="5276e-247">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-247">__Example:__</span></span>

```csharp
/// <summary>This method determines whether two Points have the same
///    location.</summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals(object o) {
    // ...
}
```

### `<summary>`

Esta etiqueta se puede usar para describir un tipo o un miembro de un tipo. <span data-ttu-id="5276e-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) para describir el propio tipo.</span><span class="sxs-lookup"><span data-stu-id="5276e-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) to describe the type itself.</span></span>

<span data-ttu-id="5276e-250">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="5276e-251">donde `description` es un resumen del tipo o miembro.</span><span class="sxs-lookup"><span data-stu-id="5276e-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="5276e-252">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="5276e-253">Esta etiqueta permite que una propiedad para describirse.</span><span class="sxs-lookup"><span data-stu-id="5276e-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="5276e-254">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="5276e-255">donde `property description` es una descripción para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="5276e-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="5276e-256">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="5276e-257">Esta etiqueta se utiliza para describir un parámetro de tipo genérico para una clase, struct, interfaz, delegado o método.</span><span class="sxs-lookup"><span data-stu-id="5276e-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="5276e-258">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="5276e-259">donde `name` es el nombre del parámetro de tipo, y `description` es su descripción.</span><span class="sxs-lookup"><span data-stu-id="5276e-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="5276e-260">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="5276e-261">Esta etiqueta se utiliza para indicar que una palabra es un parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="5276e-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="5276e-262">El archivo de documentación se puede procesar para dar formato a este parámetro de tipo de alguna manera distinta.</span><span class="sxs-lookup"><span data-stu-id="5276e-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="5276e-263">__Sintaxis:__</span><span class="sxs-lookup"><span data-stu-id="5276e-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="5276e-264">donde `name` es el nombre del parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="5276e-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="5276e-265">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="5276e-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="5276e-266">Procesar el archivo de documentación</span><span class="sxs-lookup"><span data-stu-id="5276e-266">Processing the documentation file</span></span>

<span data-ttu-id="5276e-267">El generador de documentación genera una cadena de identificador para cada elemento en el código fuente que se etiqueta con un comentario de documentación.</span><span class="sxs-lookup"><span data-stu-id="5276e-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="5276e-268">Esta cadena de identificador identifica de forma única un elemento de origen.</span><span class="sxs-lookup"><span data-stu-id="5276e-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="5276e-269">Un visor de la documentación puede utilizar una cadena de identificador para identificar el elemento de reflexión o de metadatos correspondiente a la que se aplica la documentación.</span><span class="sxs-lookup"><span data-stu-id="5276e-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="5276e-270">El archivo de documentación no es una representación jerárquica del código fuente; en su lugar, es una lista plana con una cadena de identificador generada para cada elemento.</span><span class="sxs-lookup"><span data-stu-id="5276e-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="5276e-271">Formato de cadena de Id.</span><span class="sxs-lookup"><span data-stu-id="5276e-271">ID string format</span></span>

<span data-ttu-id="5276e-272">El generador de documentación cumple las siguientes reglas cuando genera las cadenas de identificación:</span><span class="sxs-lookup"><span data-stu-id="5276e-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="5276e-273">No se coloca espacio en blanco en la cadena.</span><span class="sxs-lookup"><span data-stu-id="5276e-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="5276e-274">La primera parte de la cadena identifica el tipo de miembro que se documentan a través de un único carácter seguido de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="5276e-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="5276e-275">Se definen los siguientes tipos de miembros:</span><span class="sxs-lookup"><span data-stu-id="5276e-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="5276e-276">__Carácter__</span><span class="sxs-lookup"><span data-stu-id="5276e-276">__Character__</span></span> | <span data-ttu-id="5276e-277">__Descripción__</span><span class="sxs-lookup"><span data-stu-id="5276e-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="5276e-278">E</span><span class="sxs-lookup"><span data-stu-id="5276e-278">E</span></span>             | <span data-ttu-id="5276e-279">evento</span><span class="sxs-lookup"><span data-stu-id="5276e-279">Event</span></span>                                                       |
   | <span data-ttu-id="5276e-280">F</span><span class="sxs-lookup"><span data-stu-id="5276e-280">F</span></span>             | <span data-ttu-id="5276e-281">Campo</span><span class="sxs-lookup"><span data-stu-id="5276e-281">Field</span></span>                                                       |
   | <span data-ttu-id="5276e-282">M</span><span class="sxs-lookup"><span data-stu-id="5276e-282">M</span></span>             | <span data-ttu-id="5276e-283">Método (incluidos constructores, destructores y operadores)</span><span class="sxs-lookup"><span data-stu-id="5276e-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="5276e-284">N</span><span class="sxs-lookup"><span data-stu-id="5276e-284">N</span></span>             | <span data-ttu-id="5276e-285">Espacio de nombres</span><span class="sxs-lookup"><span data-stu-id="5276e-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="5276e-286">P</span><span class="sxs-lookup"><span data-stu-id="5276e-286">P</span></span>             | <span data-ttu-id="5276e-287">Propiedad (incluidos indizadores)</span><span class="sxs-lookup"><span data-stu-id="5276e-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="5276e-288">T</span><span class="sxs-lookup"><span data-stu-id="5276e-288">T</span></span>             | <span data-ttu-id="5276e-289">Escriba (por ejemplo, la clase, delegado, enumeración, interfaz y struct)</span><span class="sxs-lookup"><span data-stu-id="5276e-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="5276e-290">!</span><span class="sxs-lookup"><span data-stu-id="5276e-290">!</span></span>             | <span data-ttu-id="5276e-291">Cadena de error; el resto de la cadena proporciona información sobre el error.</span><span class="sxs-lookup"><span data-stu-id="5276e-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="5276e-292">Por ejemplo, el generador de documentación genera información de error para vínculos que no se puede resolver.</span><span class="sxs-lookup"><span data-stu-id="5276e-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="5276e-293">La segunda parte de la cadena es el nombre completo del elemento, empezando por la raíz del espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="5276e-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="5276e-294">El nombre del elemento, su tipos envolventes y el espacio de nombres están separados por puntos.</span><span class="sxs-lookup"><span data-stu-id="5276e-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="5276e-295">Si el nombre del elemento ya contiene puntos, éstos se reemplazan por `#(U+0023)` caracteres.</span><span class="sxs-lookup"><span data-stu-id="5276e-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="5276e-296">(Se supone que ningún elemento tiene este carácter en su nombre.)</span><span class="sxs-lookup"><span data-stu-id="5276e-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="5276e-297">Para los métodos y propiedades con argumentos, el argumento de lista se indica a continuación, incluya entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="5276e-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="5276e-298">Para aquellos sin argumentos, se omiten los paréntesis.</span><span class="sxs-lookup"><span data-stu-id="5276e-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="5276e-299">Los argumentos están separados por comas.</span><span class="sxs-lookup"><span data-stu-id="5276e-299">The arguments are separated by commas.</span></span> <span data-ttu-id="5276e-300">La codificación de cada argumento es el mismo que una firma de la CLI, como sigue:</span><span class="sxs-lookup"><span data-stu-id="5276e-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="5276e-301">Los argumentos se representan por su nombre de la documentación, que se basa en su nombre completo, puede modificado como sigue:</span><span class="sxs-lookup"><span data-stu-id="5276e-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="5276e-302">Argumentos que representan tipos genéricos tienen un anexados `` ` `` carácter (comilla simple) seguido del número de parámetros de tipo</span><span class="sxs-lookup"><span data-stu-id="5276e-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="5276e-303">Argumentos que contienen el `out` o `ref` modificador tiene una `@` siguiendo su nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="5276e-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="5276e-304">Argumentos pasan por valor o a través de `params` no tienen una ninguna anotación especial.</span><span class="sxs-lookup"><span data-stu-id="5276e-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="5276e-305">Argumentos que son matrices se representan como `[lowerbound:size, ... , lowerbound:size]` donde el número de comas es el rango menos 1, y los límites inferiores y el tamaño de cada dimensión, si se conocen, se representan en formato decimal.</span><span class="sxs-lookup"><span data-stu-id="5276e-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="5276e-306">Si no se especifica un límite inferior ni el tamaño, se omite.</span><span class="sxs-lookup"><span data-stu-id="5276e-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="5276e-307">Si se omiten el límite inferior y el tamaño de una dimensión determinada, el `:` también se omite.</span><span class="sxs-lookup"><span data-stu-id="5276e-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="5276e-308">Matrices escalonadas se representan mediante uno `[]` por nivel.</span><span class="sxs-lookup"><span data-stu-id="5276e-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="5276e-309">Argumentos que tienen tipos de puntero no es void se representan mediante un `*` después del nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="5276e-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="5276e-310">Un puntero void se representa mediante un nombre de tipo de `System.Void`.</span><span class="sxs-lookup"><span data-stu-id="5276e-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="5276e-311">Argumentos que hacen referencia a parámetros de tipo genérico que se definen en los tipos se codifican utilizando el `` ` `` carácter (comilla simple) seguido por el índice de base cero del parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="5276e-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="5276e-312">Argumentos que usan parámetros de tipo genérico definidos en los métodos use un doble-acento grave ``` `` ``` en lugar de la `` ` `` utiliza para los tipos.</span><span class="sxs-lookup"><span data-stu-id="5276e-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="5276e-313">Argumentos que hacen referencia a tipos genéricos construidos se codifican utilizando el tipo genérico, seguido de `{`, seguido de una lista separada por comas de argumentos de tipo, seguido de `}`.</span><span class="sxs-lookup"><span data-stu-id="5276e-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="5276e-314">Ejemplos de cadenas de identificador</span><span class="sxs-lookup"><span data-stu-id="5276e-314">ID string examples</span></span>

<span data-ttu-id="5276e-315">Los ejemplos siguientes muestra un fragmento de código C#, junto con la cadena de identificador generada a partir de cada elemento de origen pueden tener un comentario de documentación:</span><span class="sxs-lookup"><span data-stu-id="5276e-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="5276e-316">Los tipos se representan mediante su nombre completo, ampliada con información genérica:</span><span class="sxs-lookup"><span data-stu-id="5276e-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

   ```csharp
   enum Color { Red, Blue, Green }

   namespace Acme
   {
       interface IProcess {...}

       struct ValueType {...}

       class Widget: IProcess
       {
           public class NestedClass {...}
           public interface IMenuItem {...}
           public delegate void Del(int i);
           public enum Direction { North, South, East, West }
       }

       class MyList<T>
       {
           class Helper<U,V> {...}
       }
   }

   "T:Color"
   "T:Acme.IProcess"
   "T:Acme.ValueType"
   "T:Acme.Widget"
   "T:Acme.Widget.NestedClass"
   "T:Acme.Widget.IMenuItem"
   "T:Acme.Widget.Del"
   "T:Acme.Widget.Direction"
   "T:Acme.MyList`1"
   "T:Acme.MyList`1.Helper`2"
   ```

*  <span data-ttu-id="5276e-317">Los campos se representan mediante su nombre completo:</span><span class="sxs-lookup"><span data-stu-id="5276e-317">Fields are represented by their fully qualified name:</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           private int total;
       }
   
       class Widget: IProcess
       {
           public class NestedClass
           {
               private int value;
           }
   
           private string message;
           private static Color defaultColor;
           private const double PI = 3.14159;
           protected readonly double monthlyAverage;
           private long[] array1;
           private Widget[,] array2;
           private unsafe int *pCount;
           private unsafe float **ppValues;
       }
   }

   "F:Acme.ValueType.total"
   "F:Acme.Widget.NestedClass.value"
   "F:Acme.Widget.message"
   "F:Acme.Widget.defaultColor"
   "F:Acme.Widget.PI"
   "F:Acme.Widget.monthlyAverage"
   "F:Acme.Widget.array1"
   "F:Acme.Widget.array2"
   "F:Acme.Widget.pCount"
   "F:Acme.Widget.ppValues"
   ```

*  <span data-ttu-id="5276e-318">Constructores.</span><span class="sxs-lookup"><span data-stu-id="5276e-318">Constructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           static Widget() {...}
           public Widget() {...}
           public Widget(string s) {...}
       }
   }

   "M:Acme.Widget.#cctor"
   "M:Acme.Widget.#ctor"
   "M:Acme.Widget.#ctor(System.String)"
   ```

*  <span data-ttu-id="5276e-319">Destructores.</span><span class="sxs-lookup"><span data-stu-id="5276e-319">Destructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           ~Widget() {...}
       }
   }
   
   "M:Acme.Widget.Finalize"
   ```

*  <span data-ttu-id="5276e-320">Métodos.</span><span class="sxs-lookup"><span data-stu-id="5276e-320">Methods.</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           public void M(int i) {...}
       }

       class Widget: IProcess
       {
           public class NestedClass
           {
               public void M(int i) {...}
           }

           public static void M0() {...}
           public void M1(char c, out float f, ref ValueType v) {...}
           public void M2(short[] x1, int[,] x2, long[][] x3) {...}
           public void M3(long[][] x3, Widget[][,,] x4) {...}
           public unsafe void M4(char *pc, Color **pf) {...}
           public unsafe void M5(void *pv, double *[][,] pd) {...}
           public void M6(int i, params object[] args) {...}
       }

       class MyList<T>
       {
           public void Test(T t) { }
       }

       class UseList
       {
           public void Process(MyList<int> list) { }
           public MyList<T> GetValues<T>(T inputValue) { return null; }
       }
   }

   "M:Acme.ValueType.M(System.Int32)"
   "M:Acme.Widget.NestedClass.M(System.Int32)"
   "M:Acme.Widget.M0"
   "M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
   "M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
   "M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
   "M:Acme.Widget.M4(System.Char*,Color**)"
   "M:Acme.Widget.M5(System.Void*,System.Double*[0:,0:][])"
   "M:Acme.Widget.M6(System.Int32,System.Object[])"
   "M:Acme.MyList`1.Test(`0)"
   "M:Acme.UseList.Process(Acme.MyList{System.Int32})"
   "M:Acme.UseList.GetValues``(``0)"
   ```

*  <span data-ttu-id="5276e-321">Propiedades e indizadores.</span><span class="sxs-lookup"><span data-stu-id="5276e-321">Properties and indexers.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public int Width { get {...} set {...} }
           public int this[int i] { get {...} set {...} }
           public int this[string s, int i] { get {...} set {...} }
       }
   }

   "P:Acme.Widget.Width"
   "P:Acme.Widget.Item(System.Int32)"
   "P:Acme.Widget.Item(System.String,System.Int32)"
   ```

*  <span data-ttu-id="5276e-322">eventos.</span><span class="sxs-lookup"><span data-stu-id="5276e-322">Events.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public event Del AnEvent;
       }
   }

   "E:Acme.Widget.AnEvent"
   ```

*  <span data-ttu-id="5276e-323">Operadores unarios.</span><span class="sxs-lookup"><span data-stu-id="5276e-323">Unary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
   ```

   <span data-ttu-id="5276e-324">El conjunto completo de los nombres de función de operador unario utilizado es como sigue: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, y `op_False`.</span><span class="sxs-lookup"><span data-stu-id="5276e-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="5276e-325">Operadores binarios.</span><span class="sxs-lookup"><span data-stu-id="5276e-325">Binary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x1, Widget x2) {...}
       }
   }

   "M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
   ```

   <span data-ttu-id="5276e-326">El conjunto completo de usa los nombres de función de operador binario es como sigue: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, y `op_GreaterThanOrEqual`.</span><span class="sxs-lookup"><span data-stu-id="5276e-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="5276e-327">Los operadores de conversión tienen un carácter final "`~`" seguido por el tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="5276e-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static explicit operator int(Widget x) {...}
           public static implicit operator long(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
   "M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
   ```

## <a name="an-example"></a><span data-ttu-id="5276e-328">Un ejemplo</span><span class="sxs-lookup"><span data-stu-id="5276e-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="5276e-329">Código fuente de C#</span><span class="sxs-lookup"><span data-stu-id="5276e-329">C# source code</span></span>

<span data-ttu-id="5276e-330">El ejemplo siguiente muestra el código fuente de un `Point` clase:</span><span class="sxs-lookup"><span data-stu-id="5276e-330">The following example shows the source code of a `Point` class:</span></span>

```csharp
namespace Graphics
{

/// <summary>Class <c>Point</c> models a point in a two-dimensional plane.
/// </summary>
public class Point 
{

    /// <summary>Instance variable <c>x</c> represents the point's
    ///    x-coordinate.</summary>
    private int x;

    /// <summary>Instance variable <c>y</c> represents the point's
    ///    y-coordinate.</summary>
    private int y;

    /// <value>Property <c>X</c> represents the point's x-coordinate.</value>
    public int X
    {
        get { return x; }
        set { x = value; }
    }

    /// <value>Property <c>Y</c> represents the point's y-coordinate.</value>
    public int Y
    {
        get { return y; }
        set { y = value; }
    }

    /// <summary>This constructor initializes the new Point to
    ///    (0,0).</summary>
    public Point() : this(0,0) {}

    /// <summary>This constructor initializes the new Point to
    ///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
    /// <param><c>xor</c> is the new Point's x-coordinate.</param>
    /// <param><c>yor</c> is the new Point's y-coordinate.</param>
    public Point(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location to
    ///    the given coordinates.</summary>
    /// <param><c>xor</c> is the new x-coordinate.</param>
    /// <param><c>yor</c> is the new y-coordinate.</param>
    /// <see cref="Translate"/>
    public void Move(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location by
    ///    the given x- and y-offsets.
    /// <example>For example:
    /// <code>
    ///    Point p = new Point(3,5);
    ///    p.Translate(-1,3);
    /// </code>
    /// results in <c>p</c>'s having the value (2,8).
    /// </example>
    /// </summary>
    /// <param><c>xor</c> is the relative x-offset.</param>
    /// <param><c>yor</c> is the relative y-offset.</param>
    /// <see cref="Move"/>
    public void Translate(int xor, int yor) {
        X += xor;
        Y += yor;
    }

    /// <summary>This method determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>o</c> is the object to be compared to the current object.
    /// </param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="operator=="/>
    /// <seealso cref="operator!="/>
    public override bool Equals(object o) {
        if (o == null) {
            return false;
        }

        if (this == o) {
            return true;
        }

        if (GetType() == o.GetType()) {
            Point p = (Point)o;
            return (X == p.X) && (Y == p.Y);
        }
        return false;
    }

    /// <summary>Report a point's location as a string.</summary>
    /// <returns>A string representing a point's location, in the form (x,y),
    ///    without any leading, training, or embedded whitespace.</returns>
    public override string ToString() {
        return "(" + X + "," + Y + ")";
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator!="/>
    public static bool operator==(Point p1, Point p2) {
        if ((object)p1 == null || (object)p2 == null) {
            return false;
        }

        if (p1.GetType() == p2.GetType()) {
            return (p1.X == p2.X) && (p1.Y == p2.Y);
        }

        return false;
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points do not have the same location and the
    ///    exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator=="/>
    public static bool operator!=(Point p1, Point p2) {
        return !(p1 == p2);
    }

    /// <summary>This is the entry point of the Point class testing
    /// program.
    /// <para>This program tests each method and operator, and
    /// is intended to be run after any non-trivial maintenance has
    /// been performed on the Point class.</para></summary>
    public static void Main() {
        // class test code goes here
    }
}
}
```

### <a name="resulting-xml"></a><span data-ttu-id="5276e-331">XML resultante</span><span class="sxs-lookup"><span data-stu-id="5276e-331">Resulting XML</span></span>

<span data-ttu-id="5276e-332">Este es el resultado producido por un generador de documentación cuando se especifica el código fuente para la clase `Point`, como se muestra anteriormente:</span><span class="sxs-lookup"><span data-stu-id="5276e-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <summary>Class <c>Point</c> models a point in a two-dimensional
            plane.
            </summary>
        </member>

        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>

        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>

        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
        (0,0).</summary>
        </member>

        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
            <param><c>xor</c> is the new Point's x-coordinate.</param>
            <param><c>yor</c> is the new Point's y-coordinate.</param>
        </member>

        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>xor</c> is the new x-coordinate.</param>
            <param><c>yor</c> is the new y-coordinate.</param>
            <see cref="M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>

        <member
            name="M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by
            the given x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>xor</c> is the relative x-offset.</param>
            <param><c>yor</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>

        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the same
            location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.
            </param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
            <seealso
      cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y),
            without any leading, training, or embedded whitespace.</returns>
        </member>

        <member
       name="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
     cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member
      name="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the
            exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trivial maintenance has
            been performed on the Point class.</para></summary>
        </member>

        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>

        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
