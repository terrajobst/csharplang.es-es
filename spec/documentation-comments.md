---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912437"
---
# <a name="documentation-comments"></a><span data-ttu-id="c13b6-101">Comentarios de documentación</span><span class="sxs-lookup"><span data-stu-id="c13b6-101">Documentation comments</span></span>

<span data-ttu-id="c13b6-102">C#proporciona un mecanismo para que los programadores documenten su código mediante una sintaxis de comentario especial que contiene texto XML.</span><span class="sxs-lookup"><span data-stu-id="c13b6-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="c13b6-103">En los archivos de código fuente, se pueden usar comentarios que tengan un formato determinado para dirigir una herramienta a fin de generar XML a partir de los comentarios y los elementos de código fuente, que preceden.</span><span class="sxs-lookup"><span data-stu-id="c13b6-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="c13b6-104">Los comentarios que usan esta sintaxis se denominan ***comentarios de documentación***.</span><span class="sxs-lookup"><span data-stu-id="c13b6-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="c13b6-105">Deben preceder inmediatamente a un tipo definido por el usuario (como una clase, un delegado o una interfaz) o un miembro (por ejemplo, un campo, un evento, una propiedad o un método).</span><span class="sxs-lookup"><span data-stu-id="c13b6-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="c13b6-106">La herramienta de generación de XML se denomina ***generador de documentación***.</span><span class="sxs-lookup"><span data-stu-id="c13b6-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="c13b6-107">(Este generador podría ser, pero no es necesario, el C# propio compilador). La salida generada por el generador de documentación se denomina ***archivo de documentación***.</span><span class="sxs-lookup"><span data-stu-id="c13b6-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="c13b6-108">Un archivo de documentación se usa como entrada para un ***visor de documentación***; herramienta diseñada para generar algún tipo de presentación visual de información de tipos y su documentación asociada.</span><span class="sxs-lookup"><span data-stu-id="c13b6-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="c13b6-109">Esta especificación sugiere un conjunto de etiquetas para usar en los comentarios de documentación, pero no se requiere el uso de estas etiquetas, y se pueden usar otras etiquetas si se desea, siempre que se sigan las reglas de XML con el formato correcto.</span><span class="sxs-lookup"><span data-stu-id="c13b6-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="c13b6-110">Introducción</span><span class="sxs-lookup"><span data-stu-id="c13b6-110">Introduction</span></span>

<span data-ttu-id="c13b6-111">Los comentarios que tienen una forma especial se pueden usar para dirigir una herramienta a fin de generar XML a partir de los comentarios y los elementos de código fuente, que preceden.</span><span class="sxs-lookup"><span data-stu-id="c13b6-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="c13b6-112">Estos comentarios son comentarios de una sola línea que comienzan con tres barras diagonales (`///`) o comentarios delimitados que empiezan con una barra diagonal y dos estrellas (`/**`).</span><span class="sxs-lookup"><span data-stu-id="c13b6-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="c13b6-113">Deben preceder inmediatamente a un tipo definido por el usuario (como una clase, un delegado o una interfaz) o un miembro (como un campo, un evento, una propiedad o un método) que anotan.</span><span class="sxs-lookup"><span data-stu-id="c13b6-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="c13b6-114">Las secciones de atributos ([especificación de atributos](attributes.md#attribute-specification)) se consideran parte de las declaraciones, por lo que los comentarios de documentación deben preceder a los atributos aplicados a un tipo o miembro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="c13b6-115">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="c13b6-116">En un *single_line_doc_comment*, si hay un carácter de *espacio* en blanco `///` después de los caracteres de cada uno de los *single_line_doc_comment*de elementos adyacentes al *single_line_doc_comment*actual, entoncesel carácter de espacio en blanco no se incluye en la salida XML.</span><span class="sxs-lookup"><span data-stu-id="c13b6-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="c13b6-117">En un comentario delimitado-doc-comment, si el primer carácter que no es un espacio en blanco de la segunda línea es un asterisco y el mismo patrón de caracteres de espacio en blanco opcionales, y se repite un carácter de asterisco al principio de cada línea dentro del comentario Delimited-doc-comment, después, los caracteres del patrón repetido no se incluyen en la salida XML.</span><span class="sxs-lookup"><span data-stu-id="c13b6-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="c13b6-118">El patrón puede incluir caracteres de espacio en blanco después de, así como el carácter de asterisco.</span><span class="sxs-lookup"><span data-stu-id="c13b6-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="c13b6-119">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-119">__Example:__</span></span>

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

<span data-ttu-id="c13b6-120">El texto incluido en los comentarios de documentación debe ser correcto de acuerdo con las reglas https://www.w3.org/TR/REC-xml) de XML (.</span><span class="sxs-lookup"><span data-stu-id="c13b6-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="c13b6-121">Si el código XML tiene un formato incorrecto, se genera una advertencia y el archivo de documentación contendrá un comentario que indica que se ha detectado un error.</span><span class="sxs-lookup"><span data-stu-id="c13b6-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="c13b6-122">Aunque los desarrolladores pueden crear su propio conjunto de etiquetas, se define un conjunto recomendado en las [etiquetas recomendadas](documentation-comments.md#recommended-tags).</span><span class="sxs-lookup"><span data-stu-id="c13b6-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="c13b6-123">Algunas de las etiquetas recomendadas tienen significados especiales:</span><span class="sxs-lookup"><span data-stu-id="c13b6-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="c13b6-124">La `<param>` etiqueta se usa para describir los parámetros.</span><span class="sxs-lookup"><span data-stu-id="c13b6-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="c13b6-125">Si se usa una etiqueta de este tipo, el generador de documentación debe comprobar que el parámetro especificado existe y que todos los parámetros se describen en los comentarios de documentación.</span><span class="sxs-lookup"><span data-stu-id="c13b6-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="c13b6-126">Si se produce un error en esta comprobación, el generador de documentación emite una advertencia.</span><span class="sxs-lookup"><span data-stu-id="c13b6-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="c13b6-127">El atributo `cref` se puede asociar a cualquier etiqueta para proporcionar una referencia a un elemento de código.</span><span class="sxs-lookup"><span data-stu-id="c13b6-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="c13b6-128">El generador de documentación debe comprobar que este elemento de código existe.</span><span class="sxs-lookup"><span data-stu-id="c13b6-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="c13b6-129">Si se produce un error en la comprobación, el generador de documentación emite una advertencia.</span><span class="sxs-lookup"><span data-stu-id="c13b6-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="c13b6-130">Al buscar un nombre descrito en un `cref` atributo, el generador de documentación debe respetar la visibilidad del espacio de nombres según las instrucciones que `using` aparecen en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="c13b6-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="c13b6-131">En el caso de los elementos de código que son genéricos, no se puede`List<T>`usar la sintaxis genérica normal (es decir, "") porque genera XML no válido.</span><span class="sxs-lookup"><span data-stu-id="c13b6-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="c13b6-132">Se pueden usar llaves en lugar de corchetes (es decir, "`List{T}`") o se puede usar la sintaxis de escape XML (es decir, "`List&lt;T&gt;`").</span><span class="sxs-lookup"><span data-stu-id="c13b6-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="c13b6-133">La `<summary>` etiqueta está pensada para que la use un visor de documentación para mostrar información adicional sobre un tipo o miembro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="c13b6-134">La `<include>` etiqueta incluye información de un archivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="c13b6-135">Observe detenidamente que el archivo de documentación no proporciona información completa sobre el tipo y los miembros (por ejemplo, no contiene ninguna información de tipo).</span><span class="sxs-lookup"><span data-stu-id="c13b6-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="c13b6-136">Para obtener esta información sobre un tipo o miembro, el archivo de documentación debe utilizarse junto con la reflexión en el tipo o miembro real.</span><span class="sxs-lookup"><span data-stu-id="c13b6-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="c13b6-137">Etiquetas recomendadas</span><span class="sxs-lookup"><span data-stu-id="c13b6-137">Recommended tags</span></span>

<span data-ttu-id="c13b6-138">El generador de documentación debe aceptar y procesar cualquier etiqueta que sea válida de acuerdo con las reglas de XML.</span><span class="sxs-lookup"><span data-stu-id="c13b6-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="c13b6-139">Las siguientes etiquetas proporcionan funcionalidad de uso común en la documentación del usuario.</span><span class="sxs-lookup"><span data-stu-id="c13b6-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="c13b6-140">(Por supuesto, son posibles otras etiquetas).</span><span class="sxs-lookup"><span data-stu-id="c13b6-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="c13b6-141">__Etiqueta__</span><span class="sxs-lookup"><span data-stu-id="c13b6-141">__Tag__</span></span>          | <span data-ttu-id="c13b6-142">__Sección__</span><span class="sxs-lookup"><span data-stu-id="c13b6-142">__Section__</span></span>                                            | <span data-ttu-id="c13b6-143">__Propósito__</span><span class="sxs-lookup"><span data-stu-id="c13b6-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="c13b6-144">Establecer texto en una fuente similar a la de código</span><span class="sxs-lookup"><span data-stu-id="c13b6-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="c13b6-145">Establecer una o más líneas de código fuente o resultado del programa</span><span class="sxs-lookup"><span data-stu-id="c13b6-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="c13b6-146">Indicar un ejemplo</span><span class="sxs-lookup"><span data-stu-id="c13b6-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="c13b6-147">Identifica las excepciones que un método puede producir</span><span class="sxs-lookup"><span data-stu-id="c13b6-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="c13b6-148">Incluye XML de un archivo externo</span><span class="sxs-lookup"><span data-stu-id="c13b6-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="c13b6-149">Crear una lista o una tabla</span><span class="sxs-lookup"><span data-stu-id="c13b6-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="c13b6-150">Permitir agregar estructura a texto</span><span class="sxs-lookup"><span data-stu-id="c13b6-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="c13b6-151">Describir un parámetro para un método o constructor</span><span class="sxs-lookup"><span data-stu-id="c13b6-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="c13b6-152">Identificar que una palabra es un nombre de parámetro</span><span class="sxs-lookup"><span data-stu-id="c13b6-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="c13b6-153">Documentar la accesibilidad de seguridad de un miembro</span><span class="sxs-lookup"><span data-stu-id="c13b6-153">Document the security accessibility of a member</span></span>        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | <span data-ttu-id="c13b6-154">Describir información adicional sobre un tipo</span><span class="sxs-lookup"><span data-stu-id="c13b6-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="c13b6-155">Describir el valor devuelto de un método</span><span class="sxs-lookup"><span data-stu-id="c13b6-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="c13b6-156">Especificar un vínculo</span><span class="sxs-lookup"><span data-stu-id="c13b6-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="c13b6-157">Generar una entrada vea también</span><span class="sxs-lookup"><span data-stu-id="c13b6-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="c13b6-158">Describir un tipo o un miembro de un tipo</span><span class="sxs-lookup"><span data-stu-id="c13b6-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="c13b6-159">Describir una propiedad</span><span class="sxs-lookup"><span data-stu-id="c13b6-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="c13b6-160">Describir un parámetro de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="c13b6-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="c13b6-161">Identificar que una palabra es un nombre de parámetro de tipo</span><span class="sxs-lookup"><span data-stu-id="c13b6-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="c13b6-162">Esta etiqueta proporciona un mecanismo para indicar que un fragmento de texto dentro de una descripción debe establecerse en una fuente especial, como la que se usa para un bloque de código.</span><span class="sxs-lookup"><span data-stu-id="c13b6-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="c13b6-163">Para las líneas de código real, `<code>` use[`<code>`](documentation-comments.md#code)().</span><span class="sxs-lookup"><span data-stu-id="c13b6-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="c13b6-164">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="c13b6-165">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="c13b6-166">Esta etiqueta se usa para establecer una o más líneas de código fuente o el resultado del programa en alguna fuente especial.</span><span class="sxs-lookup"><span data-stu-id="c13b6-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="c13b6-167">Para fragmentos de código pequeños en narrativa, `<c>` use[`<c>`](documentation-comments.md#c)().</span><span class="sxs-lookup"><span data-stu-id="c13b6-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="c13b6-168">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="c13b6-169">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-169">__Example:__</span></span>

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

<span data-ttu-id="c13b6-170">Esta etiqueta permite el código de ejemplo dentro de un comentario, para especificar cómo se puede usar un método u otro miembro de la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="c13b6-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="c13b6-171">Normalmente, también implicaría el uso de la etiqueta `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="c13b6-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="c13b6-172">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="c13b6-173">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-173">__Example:__</span></span>

<span data-ttu-id="c13b6-174">Vea `<code>` [(`<code>`](documentation-comments.md#code)) para obtener un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="c13b6-175">Esta etiqueta proporciona una manera de documentar las excepciones que un método puede iniciar.</span><span class="sxs-lookup"><span data-stu-id="c13b6-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="c13b6-176">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="c13b6-177">donde</span><span class="sxs-lookup"><span data-stu-id="c13b6-177">where</span></span>

* <span data-ttu-id="c13b6-178">`member`es el nombre de un miembro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-178">`member` is the name of a member.</span></span> <span data-ttu-id="c13b6-179">El generador de documentación comprueba que el miembro dado existe y `member` se convierte en el nombre de elemento canónico en el archivo de documentación.</span><span class="sxs-lookup"><span data-stu-id="c13b6-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="c13b6-180">`description`es una descripción de las circunstancias en las que se produce la excepción.</span><span class="sxs-lookup"><span data-stu-id="c13b6-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="c13b6-181">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-181">__Example:__</span></span>

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

<span data-ttu-id="c13b6-182">Esta etiqueta permite incluir información de un documento XML que es externo al archivo de código fuente.</span><span class="sxs-lookup"><span data-stu-id="c13b6-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="c13b6-183">El archivo externo debe ser un documento XML con el formato correcto y se aplica una expresión XPath a ese documento para especificar el XML de ese documento que se va a incluir.</span><span class="sxs-lookup"><span data-stu-id="c13b6-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="c13b6-184">A `<include>` continuación, la etiqueta se reemplaza con el XML seleccionado del documento externo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="c13b6-185">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="c13b6-186">donde</span><span class="sxs-lookup"><span data-stu-id="c13b6-186">where</span></span>

* <span data-ttu-id="c13b6-187">`filename`es el nombre de archivo de un archivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="c13b6-188">El nombre de archivo se interpreta en relación con el archivo que contiene la etiqueta include.</span><span class="sxs-lookup"><span data-stu-id="c13b6-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="c13b6-189">`xpath`es una expresión XPath que selecciona parte de XML en el archivo XML externo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="c13b6-190">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-190">__Example:__</span></span>

<span data-ttu-id="c13b6-191">Si el código fuente contiene una declaración como:</span><span class="sxs-lookup"><span data-stu-id="c13b6-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="c13b6-192">y el archivo externo "docs. xml" tenía el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="c13b6-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="c13b6-193">a continuación, la misma documentación se genera como si el código fuente incluyera:</span><span class="sxs-lookup"><span data-stu-id="c13b6-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="c13b6-194">Esta etiqueta se usa para crear una lista o una tabla de elementos.</span><span class="sxs-lookup"><span data-stu-id="c13b6-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="c13b6-195">Puede contener un `<listheader>` bloque para definir la fila de encabezado de una tabla o de una lista de definiciones.</span><span class="sxs-lookup"><span data-stu-id="c13b6-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="c13b6-196">(Al definir una tabla, solo es necesario proporcionar `term` una entrada para en el encabezado).</span><span class="sxs-lookup"><span data-stu-id="c13b6-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="c13b6-197">Cada elemento de la lista se especifica con un `<item>` bloque.</span><span class="sxs-lookup"><span data-stu-id="c13b6-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="c13b6-198">Al crear una lista de definiciones, `term` deben `description` especificarse y.</span><span class="sxs-lookup"><span data-stu-id="c13b6-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="c13b6-199">Sin embargo, para una tabla, una lista con viñetas o una lista numerada, solo `description` es necesario especificar.</span><span class="sxs-lookup"><span data-stu-id="c13b6-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="c13b6-200">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-200">__Syntax:__</span></span>

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

<span data-ttu-id="c13b6-201">donde</span><span class="sxs-lookup"><span data-stu-id="c13b6-201">where</span></span>

* <span data-ttu-id="c13b6-202">`term`es el término que se va a definir, cuya `description`definición está en.</span><span class="sxs-lookup"><span data-stu-id="c13b6-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="c13b6-203">`description`es un elemento de una lista con viñetas o numerada, o la definición de `term`un objeto.</span><span class="sxs-lookup"><span data-stu-id="c13b6-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="c13b6-204">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-204">__Example:__</span></span>

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

<span data-ttu-id="c13b6-205">Esta etiqueta se usa dentro de `<summary>` otras etiquetas, como ([`<remarks>`](documentation-comments.md#remarks)) o `<returns>` ([`<returns>`](documentation-comments.md#returns)), y permite agregar la estructura al texto.</span><span class="sxs-lookup"><span data-stu-id="c13b6-205">This tag is for use inside other tags, such as `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="c13b6-206">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="c13b6-207">donde `content` es el texto del párrafo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="c13b6-208">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-208">__Example:__</span></span>

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

<span data-ttu-id="c13b6-209">Esta etiqueta se usa para describir un parámetro para un método, un constructor o un indizador.</span><span class="sxs-lookup"><span data-stu-id="c13b6-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="c13b6-210">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="c13b6-211">donde</span><span class="sxs-lookup"><span data-stu-id="c13b6-211">where</span></span>

* <span data-ttu-id="c13b6-212">`name`es el nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="c13b6-213">`description`es una descripción del parámetro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="c13b6-214">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-214">__Example:__</span></span>

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

<span data-ttu-id="c13b6-215">Esta etiqueta se usa para indicar que una palabra es un parámetro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="c13b6-216">El archivo de documentación se puede procesar para dar formato a este parámetro de alguna manera distinta.</span><span class="sxs-lookup"><span data-stu-id="c13b6-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="c13b6-217">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="c13b6-218">donde `name` es el nombre del parámetro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="c13b6-219">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-219">__Example:__</span></span>

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

<span data-ttu-id="c13b6-220">Esta etiqueta permite documentar la accesibilidad de seguridad de un miembro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="c13b6-221">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="c13b6-222">donde</span><span class="sxs-lookup"><span data-stu-id="c13b6-222">where</span></span>

* <span data-ttu-id="c13b6-223">`member`es el nombre de un miembro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-223">`member` is the name of a member.</span></span> <span data-ttu-id="c13b6-224">El generador de documentación comprueba que el elemento de código dado existe y traduce el *miembro* al nombre de elemento canónico en el archivo de documentación.</span><span class="sxs-lookup"><span data-stu-id="c13b6-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="c13b6-225">`description`es una descripción del acceso al miembro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="c13b6-226">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

<span data-ttu-id="c13b6-227">Esta etiqueta se usa para especificar información adicional sobre un tipo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="c13b6-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) para describir el tipo en sí y los miembros de un tipo).</span><span class="sxs-lookup"><span data-stu-id="c13b6-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="c13b6-229">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-229">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="c13b6-230">donde `description` es el texto del comentario.</span><span class="sxs-lookup"><span data-stu-id="c13b6-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="c13b6-231">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remarks>Uses polar coordinates</remarks>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="c13b6-232">Esta etiqueta se utiliza para describir el valor devuelto de un método.</span><span class="sxs-lookup"><span data-stu-id="c13b6-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="c13b6-233">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="c13b6-234">donde `description` es una descripción del valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="c13b6-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="c13b6-235">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="c13b6-236">Esta etiqueta permite especificar un vínculo dentro del texto.</span><span class="sxs-lookup"><span data-stu-id="c13b6-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="c13b6-237">Use `<seealso>` [(`<seealso>`](documentation-comments.md#seealso)) para indicar el texto que va a aparecer en una sección Vea también.</span><span class="sxs-lookup"><span data-stu-id="c13b6-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="c13b6-238">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="c13b6-239">donde `member` es el nombre de un miembro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-239">where `member` is the name of a member.</span></span> <span data-ttu-id="c13b6-240">El generador de documentación comprueba que el elemento de código dado existe y cambia el *miembro* al nombre del elemento en el archivo de documentación generado.</span><span class="sxs-lookup"><span data-stu-id="c13b6-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="c13b6-241">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-241">__Example:__</span></span>

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

<span data-ttu-id="c13b6-242">Esta etiqueta permite generar una entrada para la sección Vea también.</span><span class="sxs-lookup"><span data-stu-id="c13b6-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="c13b6-243">Use `<see>` [(`<see>`](documentation-comments.md#see)) para especificar un vínculo desde dentro del texto.</span><span class="sxs-lookup"><span data-stu-id="c13b6-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="c13b6-244">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="c13b6-245">donde `member` es el nombre de un miembro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-245">where `member` is the name of a member.</span></span> <span data-ttu-id="c13b6-246">El generador de documentación comprueba que el elemento de código dado existe y cambia el *miembro* al nombre del elemento en el archivo de documentación generado.</span><span class="sxs-lookup"><span data-stu-id="c13b6-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="c13b6-247">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-247">__Example:__</span></span>

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

Esta etiqueta se puede utilizar para describir un tipo o un miembro de un tipo. <span data-ttu-id="c13b6-249">Use `<remarks>` [(`<remarks>`](documentation-comments.md#remarks)) para describir el propio tipo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-249">Use `<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) to describe the type itself.</span></span>

<span data-ttu-id="c13b6-250">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="c13b6-251">donde `description` es un resumen del tipo o miembro.</span><span class="sxs-lookup"><span data-stu-id="c13b6-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="c13b6-252">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="c13b6-253">Esta etiqueta permite describir una propiedad.</span><span class="sxs-lookup"><span data-stu-id="c13b6-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="c13b6-254">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="c13b6-255">donde `property description` es una descripción de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="c13b6-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="c13b6-256">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="c13b6-257">Esta etiqueta se usa para describir un parámetro de tipo genérico para una clase, una estructura, una interfaz, un delegado o un método.</span><span class="sxs-lookup"><span data-stu-id="c13b6-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="c13b6-258">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="c13b6-259">donde `name` es el nombre del parámetro de tipo y `description` es su descripción.</span><span class="sxs-lookup"><span data-stu-id="c13b6-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="c13b6-260">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="c13b6-261">Esta etiqueta se usa para indicar que una palabra es un parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="c13b6-262">El archivo de documentación se puede procesar para dar formato a este parámetro de tipo de una manera distinta.</span><span class="sxs-lookup"><span data-stu-id="c13b6-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="c13b6-263">__Sintáctica__</span><span class="sxs-lookup"><span data-stu-id="c13b6-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="c13b6-264">donde `name` es el nombre del parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="c13b6-265">__Ejemplo:__</span><span class="sxs-lookup"><span data-stu-id="c13b6-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="c13b6-266">Procesar el archivo de documentación</span><span class="sxs-lookup"><span data-stu-id="c13b6-266">Processing the documentation file</span></span>

<span data-ttu-id="c13b6-267">El generador de documentación genera una cadena de identificador para cada elemento del código fuente que se etiqueta con un Comentario de documentación.</span><span class="sxs-lookup"><span data-stu-id="c13b6-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="c13b6-268">Esta cadena de identificador identifica de forma única un elemento de origen.</span><span class="sxs-lookup"><span data-stu-id="c13b6-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="c13b6-269">Un visor de documentación puede usar una cadena de identificador para identificar el elemento de metadatos/reflexión correspondiente al que se aplica la documentación.</span><span class="sxs-lookup"><span data-stu-id="c13b6-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="c13b6-270">El archivo de documentación no es una representación jerárquica del código fuente. en su lugar, es una lista plana con una cadena de identificador generada para cada elemento.</span><span class="sxs-lookup"><span data-stu-id="c13b6-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="c13b6-271">Formato de cadena de identificador</span><span class="sxs-lookup"><span data-stu-id="c13b6-271">ID string format</span></span>

<span data-ttu-id="c13b6-272">El generador de documentación observa las siguientes reglas cuando genera las cadenas de identificador:</span><span class="sxs-lookup"><span data-stu-id="c13b6-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="c13b6-273">No se coloca espacio en blanco en la cadena.</span><span class="sxs-lookup"><span data-stu-id="c13b6-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="c13b6-274">La primera parte de la cadena identifica el tipo de miembro que se va a documentar, a través de un solo carácter seguido de un signo de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="c13b6-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="c13b6-275">Se definen los siguientes tipos de miembros:</span><span class="sxs-lookup"><span data-stu-id="c13b6-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="c13b6-276">__Carácter__</span><span class="sxs-lookup"><span data-stu-id="c13b6-276">__Character__</span></span> | <span data-ttu-id="c13b6-277">__Descripción__</span><span class="sxs-lookup"><span data-stu-id="c13b6-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="c13b6-278">E</span><span class="sxs-lookup"><span data-stu-id="c13b6-278">E</span></span>             | <span data-ttu-id="c13b6-279">Evento</span><span class="sxs-lookup"><span data-stu-id="c13b6-279">Event</span></span>                                                       |
   | <span data-ttu-id="c13b6-280">F</span><span class="sxs-lookup"><span data-stu-id="c13b6-280">F</span></span>             | <span data-ttu-id="c13b6-281">Campo</span><span class="sxs-lookup"><span data-stu-id="c13b6-281">Field</span></span>                                                       |
   | <span data-ttu-id="c13b6-282">M</span><span class="sxs-lookup"><span data-stu-id="c13b6-282">M</span></span>             | <span data-ttu-id="c13b6-283">Método (incluidos constructores, destructores y operadores)</span><span class="sxs-lookup"><span data-stu-id="c13b6-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="c13b6-284">N</span><span class="sxs-lookup"><span data-stu-id="c13b6-284">N</span></span>             | <span data-ttu-id="c13b6-285">Espacio de nombres</span><span class="sxs-lookup"><span data-stu-id="c13b6-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="c13b6-286">P</span><span class="sxs-lookup"><span data-stu-id="c13b6-286">P</span></span>             | <span data-ttu-id="c13b6-287">Propiedad (incluidos los indizadores)</span><span class="sxs-lookup"><span data-stu-id="c13b6-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="c13b6-288">T</span><span class="sxs-lookup"><span data-stu-id="c13b6-288">T</span></span>             | <span data-ttu-id="c13b6-289">Tipo (como clase, delegado, enumeración, interfaz y struct)</span><span class="sxs-lookup"><span data-stu-id="c13b6-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="c13b6-290">!</span><span class="sxs-lookup"><span data-stu-id="c13b6-290">!</span></span>             | <span data-ttu-id="c13b6-291">Cadena de error; en el resto de la cadena se proporciona información sobre el error.</span><span class="sxs-lookup"><span data-stu-id="c13b6-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="c13b6-292">Por ejemplo, el generador de documentación genera información de error para los vínculos que no se pueden resolver.</span><span class="sxs-lookup"><span data-stu-id="c13b6-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="c13b6-293">La segunda parte de la cadena es el nombre completo del elemento, que empieza en la raíz del espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="c13b6-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="c13b6-294">El nombre del elemento, sus tipos envolventes y su espacio de nombres se separan por puntos.</span><span class="sxs-lookup"><span data-stu-id="c13b6-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="c13b6-295">Si el nombre del propio elemento tiene puntos, se reemplazan por `#(U+0023)` caracteres.</span><span class="sxs-lookup"><span data-stu-id="c13b6-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="c13b6-296">(Se supone que ningún elemento tiene este carácter en su nombre).</span><span class="sxs-lookup"><span data-stu-id="c13b6-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="c13b6-297">En el caso de los métodos y propiedades con argumentos, la lista de argumentos sigue entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="c13b6-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="c13b6-298">Para aquellos que no tienen argumentos, se omiten los paréntesis.</span><span class="sxs-lookup"><span data-stu-id="c13b6-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="c13b6-299">Los argumentos están separados por comas.</span><span class="sxs-lookup"><span data-stu-id="c13b6-299">The arguments are separated by commas.</span></span> <span data-ttu-id="c13b6-300">La codificación de cada argumento es la misma que la firma de la CLI, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="c13b6-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="c13b6-301">Los argumentos se representan mediante el nombre de la documentación, que se basa en su nombre completo, modificado de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="c13b6-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="c13b6-302">Los argumentos que representan tipos genéricos tienen un `` ` `` carácter anexado (acento grave) seguido por el número de parámetros de tipo</span><span class="sxs-lookup"><span data-stu-id="c13b6-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="c13b6-303">Los argumentos que `out` tienen `ref` el modificador o `@` tienen un nombre de tipo siguiente.</span><span class="sxs-lookup"><span data-stu-id="c13b6-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="c13b6-304">Los argumentos que se pasan por `params` valor o por Via no tienen ninguna notación especial.</span><span class="sxs-lookup"><span data-stu-id="c13b6-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="c13b6-305">Los argumentos que son matrices se representan como `[lowerbound:size, ... , lowerbound:size]` donde el número de comas es el rango menos uno, y los límites inferiores y el tamaño de cada dimensión, si se conocen, se representan en formato decimal.</span><span class="sxs-lookup"><span data-stu-id="c13b6-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="c13b6-306">Si no se especifica un límite inferior o un tamaño, se omite.</span><span class="sxs-lookup"><span data-stu-id="c13b6-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="c13b6-307">Si se omiten el límite inferior y el tamaño de una dimensión determinada `:` , también se omite.</span><span class="sxs-lookup"><span data-stu-id="c13b6-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="c13b6-308">Las matrices escalonadas se representan en `[]` una por nivel.</span><span class="sxs-lookup"><span data-stu-id="c13b6-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="c13b6-309">Los argumentos que tienen tipos de puntero distintos de void se representan `*` mediante un siguiente nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="c13b6-310">Un puntero void se representa utilizando un nombre de tipo `System.Void`de.</span><span class="sxs-lookup"><span data-stu-id="c13b6-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="c13b6-311">Los argumentos que hacen referencia a los parámetros de tipo genérico definidos en los tipos se `` ` `` codifican mediante el carácter (acento grave) seguido por el índice de base cero del parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="c13b6-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="c13b6-312">Los argumentos que utilizan parámetros de tipo genérico definidos en métodos utilizan un acento ``` `` ``` doble en lugar del utilizado para los `` ` `` tipos.</span><span class="sxs-lookup"><span data-stu-id="c13b6-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="c13b6-313">Los argumentos que hacen referencia a tipos genéricos construidos se codifican utilizando el tipo genérico, `{`seguido de, seguido de una lista separada por comas de argumentos de `}`tipo, seguida de.</span><span class="sxs-lookup"><span data-stu-id="c13b6-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="c13b6-314">Ejemplos de cadenas de ID.</span><span class="sxs-lookup"><span data-stu-id="c13b6-314">ID string examples</span></span>

<span data-ttu-id="c13b6-315">Los ejemplos siguientes muestran un fragmento de C# código, junto con la cadena de identificador generada a partir de cada elemento de origen capaz de tener un Comentario de documentación:</span><span class="sxs-lookup"><span data-stu-id="c13b6-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="c13b6-316">Los tipos se representan mediante su nombre completo, ampliados con información genérica:</span><span class="sxs-lookup"><span data-stu-id="c13b6-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="c13b6-317">Los campos se representan mediante su nombre completo:</span><span class="sxs-lookup"><span data-stu-id="c13b6-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="c13b6-318">Constructores.</span><span class="sxs-lookup"><span data-stu-id="c13b6-318">Constructors.</span></span>

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

*  <span data-ttu-id="c13b6-319">Destructores.</span><span class="sxs-lookup"><span data-stu-id="c13b6-319">Destructors.</span></span>

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

*  <span data-ttu-id="c13b6-320">Modalidades.</span><span class="sxs-lookup"><span data-stu-id="c13b6-320">Methods.</span></span>

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

*  <span data-ttu-id="c13b6-321">Propiedades e indizadores.</span><span class="sxs-lookup"><span data-stu-id="c13b6-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="c13b6-322">Ceso.</span><span class="sxs-lookup"><span data-stu-id="c13b6-322">Events.</span></span>

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

*  <span data-ttu-id="c13b6-323">Operadores unarios.</span><span class="sxs-lookup"><span data-stu-id="c13b6-323">Unary operators.</span></span>

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

   <span data-ttu-id="c13b6-324">El conjunto completo de nombres de función de operador unario se usa `op_UnaryPlus`como se `op_LogicalNot`indica a `op_Increment`continuación `op_Decrement`: `op_True`, `op_UnaryNegation`, `op_False`, `op_OnesComplement`,,, y.</span><span class="sxs-lookup"><span data-stu-id="c13b6-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="c13b6-325">Operadores binarios.</span><span class="sxs-lookup"><span data-stu-id="c13b6-325">Binary operators.</span></span>

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

   <span data-ttu-id="c13b6-326">El conjunto completo de nombres de función de operador binario utilizado es `op_Addition`el `op_Subtraction`siguiente: `op_Division`, `op_Modulus`, `op_BitwiseAnd` `op_Multiply`,, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift` `op_BitwiseOr`,,,, `op_Equality`, ,,`op_LessThan`, y`op_GreaterThan`. `op_LessThanOrEqual` `op_Inequality` `op_GreaterThanOrEqual`</span><span class="sxs-lookup"><span data-stu-id="c13b6-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="c13b6-327">Los operadores de conversión tienen un "`~`" final seguido del tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="c13b6-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="c13b6-328">Un ejemplo</span><span class="sxs-lookup"><span data-stu-id="c13b6-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="c13b6-329">C#código fuente</span><span class="sxs-lookup"><span data-stu-id="c13b6-329">C# source code</span></span>

<span data-ttu-id="c13b6-330">En el ejemplo siguiente se muestra el código fuente `Point` de una clase:</span><span class="sxs-lookup"><span data-stu-id="c13b6-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="c13b6-331">XML resultante</span><span class="sxs-lookup"><span data-stu-id="c13b6-331">Resulting XML</span></span>

<span data-ttu-id="c13b6-332">A continuación se muestra la salida generada por un generador de documentación cuando se proporciona `Point`el código fuente de la clase, mostrado anteriormente:</span><span class="sxs-lookup"><span data-stu-id="c13b6-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
