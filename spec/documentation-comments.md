---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912437"
---
# <a name="documentation-comments"></a>Comentarios de documentación

C#proporciona un mecanismo para que los programadores documenten su código mediante una sintaxis de comentario especial que contiene texto XML. En los archivos de código fuente, se pueden usar comentarios que tengan un formato determinado para dirigir una herramienta a fin de generar XML a partir de los comentarios y los elementos de código fuente, que preceden. Los comentarios que usan esta sintaxis se denominan ***comentarios de documentación***. Deben preceder inmediatamente a un tipo definido por el usuario (como una clase, un delegado o una interfaz) o un miembro (por ejemplo, un campo, un evento, una propiedad o un método). La herramienta de generación de XML se denomina ***generador de documentación***. (Este generador podría ser, pero no es necesario, el C# propio compilador). La salida generada por el generador de documentación se denomina ***archivo de documentación***. Un archivo de documentación se usa como entrada para un ***visor de documentación***; herramienta diseñada para generar algún tipo de presentación visual de información de tipos y su documentación asociada.

Esta especificación sugiere un conjunto de etiquetas para usar en los comentarios de documentación, pero no se requiere el uso de estas etiquetas, y se pueden usar otras etiquetas si se desea, siempre que se sigan las reglas de XML con el formato correcto.

## <a name="introduction"></a>Introducción

Los comentarios que tienen una forma especial se pueden usar para dirigir una herramienta a fin de generar XML a partir de los comentarios y los elementos de código fuente, que preceden. Estos comentarios son comentarios de una sola línea que comienzan con tres barras diagonales (`///`) o comentarios delimitados que empiezan con una barra diagonal y dos estrellas (`/**`). Deben preceder inmediatamente a un tipo definido por el usuario (como una clase, un delegado o una interfaz) o un miembro (como un campo, un evento, una propiedad o un método) que anotan. Las secciones de atributos ([especificación de atributos](attributes.md#attribute-specification)) se consideran parte de las declaraciones, por lo que los comentarios de documentación deben preceder a los atributos aplicados a un tipo o miembro.

__Sintáctica__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

En un *single_line_doc_comment*, si hay un carácter de *espacio* en blanco `///` después de los caracteres de cada uno de los *single_line_doc_comment*de elementos adyacentes al *single_line_doc_comment*actual, entoncesel carácter de espacio en blanco no se incluye en la salida XML.

En un comentario delimitado-doc-comment, si el primer carácter que no es un espacio en blanco de la segunda línea es un asterisco y el mismo patrón de caracteres de espacio en blanco opcionales, y se repite un carácter de asterisco al principio de cada línea dentro del comentario Delimited-doc-comment, después, los caracteres del patrón repetido no se incluyen en la salida XML. El patrón puede incluir caracteres de espacio en blanco después de, así como el carácter de asterisco.

__Ejemplo:__

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

El texto incluido en los comentarios de documentación debe ser correcto de acuerdo con las reglas https://www.w3.org/TR/REC-xml) de XML (. Si el código XML tiene un formato incorrecto, se genera una advertencia y el archivo de documentación contendrá un comentario que indica que se ha detectado un error.

Aunque los desarrolladores pueden crear su propio conjunto de etiquetas, se define un conjunto recomendado en las [etiquetas recomendadas](documentation-comments.md#recommended-tags). Algunas de las etiquetas recomendadas tienen significados especiales:

*  La `<param>` etiqueta se usa para describir los parámetros. Si se usa una etiqueta de este tipo, el generador de documentación debe comprobar que el parámetro especificado existe y que todos los parámetros se describen en los comentarios de documentación. Si se produce un error en esta comprobación, el generador de documentación emite una advertencia.
*  El atributo `cref` se puede asociar a cualquier etiqueta para proporcionar una referencia a un elemento de código. El generador de documentación debe comprobar que este elemento de código existe. Si se produce un error en la comprobación, el generador de documentación emite una advertencia. Al buscar un nombre descrito en un `cref` atributo, el generador de documentación debe respetar la visibilidad del espacio de nombres según las instrucciones que `using` aparecen en el código fuente. En el caso de los elementos de código que son genéricos, no se puede`List<T>`usar la sintaxis genérica normal (es decir, "") porque genera XML no válido. Se pueden usar llaves en lugar de corchetes (es decir, "`List{T}`") o se puede usar la sintaxis de escape XML (es decir, "`List&lt;T&gt;`").
*  La `<summary>` etiqueta está pensada para que la use un visor de documentación para mostrar información adicional sobre un tipo o miembro.
*  La `<include>` etiqueta incluye información de un archivo XML externo.

Observe detenidamente que el archivo de documentación no proporciona información completa sobre el tipo y los miembros (por ejemplo, no contiene ninguna información de tipo). Para obtener esta información sobre un tipo o miembro, el archivo de documentación debe utilizarse junto con la reflexión en el tipo o miembro real.

## <a name="recommended-tags"></a>Etiquetas recomendadas

El generador de documentación debe aceptar y procesar cualquier etiqueta que sea válida de acuerdo con las reglas de XML. Las siguientes etiquetas proporcionan funcionalidad de uso común en la documentación del usuario. (Por supuesto, son posibles otras etiquetas).


| __Etiqueta__          | __Sección__                                            | __Propósito__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Establecer texto en una fuente similar a la de código                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Establecer una o más líneas de código fuente o resultado del programa |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Indicar un ejemplo                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Identifica las excepciones que un método puede producir           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Incluye XML de un archivo externo                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Crear una lista o una tabla                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Permitir agregar estructura a texto                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Describir un parámetro para un método o constructor       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Identificar que una palabra es un nombre de parámetro               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | Documentar la accesibilidad de seguridad de un miembro        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | Describir información adicional sobre un tipo           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Describir el valor devuelto de un método                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Especificar un vínculo                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Generar una entrada vea también                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Describir un tipo o un miembro de un tipo                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Describir una propiedad                                    |
| `<typeparam>`    |                                                        | Describir un parámetro de tipo genérico                      |
| `<typeparamref>` |                                                        | Identificar que una palabra es un nombre de parámetro de tipo          |

### `<c>`

Esta etiqueta proporciona un mecanismo para indicar que un fragmento de texto dentro de una descripción debe establecerse en una fuente especial, como la que se usa para un bloque de código. Para las líneas de código real, `<code>` use[`<code>`](documentation-comments.md#code)().

__Sintáctica__

```xml
<c>text</c>
```

__Ejemplo:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

Esta etiqueta se usa para establecer una o más líneas de código fuente o el resultado del programa en alguna fuente especial. Para fragmentos de código pequeños en narrativa, `<c>` use[`<c>`](documentation-comments.md#c)().

__Sintáctica__

```xml
<code>source code or program output</code>
```

__Ejemplo:__

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

Esta etiqueta permite el código de ejemplo dentro de un comentario, para especificar cómo se puede usar un método u otro miembro de la biblioteca. Normalmente, también implicaría el uso de la etiqueta `<code>` ([`<code>`](documentation-comments.md#code)).

__Sintáctica__

```xml
<example>description</example>
```

__Ejemplo:__

Vea `<code>` [(`<code>`](documentation-comments.md#code)) para obtener un ejemplo.

### `<exception>`

Esta etiqueta proporciona una manera de documentar las excepciones que un método puede iniciar.

__Sintáctica__

```xml
<exception cref="member">description</exception>
```

donde

* `member`es el nombre de un miembro. El generador de documentación comprueba que el miembro dado existe y `member` se convierte en el nombre de elemento canónico en el archivo de documentación.
* `description`es una descripción de las circunstancias en las que se produce la excepción.

__Ejemplo:__

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

Esta etiqueta permite incluir información de un documento XML que es externo al archivo de código fuente. El archivo externo debe ser un documento XML con el formato correcto y se aplica una expresión XPath a ese documento para especificar el XML de ese documento que se va a incluir. A `<include>` continuación, la etiqueta se reemplaza con el XML seleccionado del documento externo.

__Sintáctica__

```
<include file="filename" path="xpath" />
```

donde

* `filename`es el nombre de archivo de un archivo XML externo. El nombre de archivo se interpreta en relación con el archivo que contiene la etiqueta include.
* `xpath`es una expresión XPath que selecciona parte de XML en el archivo XML externo.

__Ejemplo:__

Si el código fuente contiene una declaración como:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

y el archivo externo "docs. xml" tenía el siguiente contenido:

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

a continuación, la misma documentación se genera como si el código fuente incluyera:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Esta etiqueta se usa para crear una lista o una tabla de elementos. Puede contener un `<listheader>` bloque para definir la fila de encabezado de una tabla o de una lista de definiciones. (Al definir una tabla, solo es necesario proporcionar `term` una entrada para en el encabezado).

Cada elemento de la lista se especifica con un `<item>` bloque. Al crear una lista de definiciones, `term` deben `description` especificarse y. Sin embargo, para una tabla, una lista con viñetas o una lista numerada, solo `description` es necesario especificar.

__Sintáctica__

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

donde

* `term`es el término que se va a definir, cuya `description`definición está en.
* `description`es un elemento de una lista con viñetas o numerada, o la definición de `term`un objeto.

__Ejemplo:__

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

Esta etiqueta se usa dentro de `<summary>` otras etiquetas, como ([`<remarks>`](documentation-comments.md#remarks)) o `<returns>` ([`<returns>`](documentation-comments.md#returns)), y permite agregar la estructura al texto.

__Sintáctica__

```xml
<para>content</para>
```

donde `content` es el texto del párrafo.

__Ejemplo:__

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

Esta etiqueta se usa para describir un parámetro para un método, un constructor o un indizador.

__Sintáctica__

```xml
<param name="name">description</param>
```

donde

* `name`es el nombre del parámetro.
* `description`es una descripción del parámetro.

__Ejemplo:__

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

Esta etiqueta se usa para indicar que una palabra es un parámetro. El archivo de documentación se puede procesar para dar formato a este parámetro de alguna manera distinta.

__Sintáctica__

```xml
<paramref name="name"/>
```

donde `name` es el nombre del parámetro.

__Ejemplo:__

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

Esta etiqueta permite documentar la accesibilidad de seguridad de un miembro.

__Sintáctica__

```xml
<permission cref="member">description</permission>
```

donde

* `member`es el nombre de un miembro. El generador de documentación comprueba que el elemento de código dado existe y traduce el *miembro* al nombre de elemento canónico en el archivo de documentación.
* `description`es una descripción del acceso al miembro.

__Ejemplo:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

Esta etiqueta se usa para especificar información adicional sobre un tipo. (Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) para describir el tipo en sí y los miembros de un tipo).

__Sintáctica__

```xml
<remarks>description</remarks>
```

donde `description` es el texto del comentario.

__Ejemplo:__

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

Esta etiqueta se utiliza para describir el valor devuelto de un método.

__Sintáctica__

```xml
<returns>description</returns>
```

donde `description` es una descripción del valor devuelto.

__Ejemplo:__

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

Esta etiqueta permite especificar un vínculo dentro del texto. Use `<seealso>` [(`<seealso>`](documentation-comments.md#seealso)) para indicar el texto que va a aparecer en una sección Vea también.

__Sintáctica__

```xml
<see cref="member"/>
```

donde `member` es el nombre de un miembro. El generador de documentación comprueba que el elemento de código dado existe y cambia el *miembro* al nombre del elemento en el archivo de documentación generado.

__Ejemplo:__

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

Esta etiqueta permite generar una entrada para la sección Vea también. Use `<see>` [(`<see>`](documentation-comments.md#see)) para especificar un vínculo desde dentro del texto.

__Sintáctica__

```xml
<seealso cref="member"/>
```

donde `member` es el nombre de un miembro. El generador de documentación comprueba que el elemento de código dado existe y cambia el *miembro* al nombre del elemento en el archivo de documentación generado.

__Ejemplo:__

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

Esta etiqueta se puede utilizar para describir un tipo o un miembro de un tipo. Use `<remarks>` [(`<remarks>`](documentation-comments.md#remarks)) para describir el propio tipo.

__Sintáctica__

```xml
<summary>description</summary>
```

donde `description` es un resumen del tipo o miembro.

__Ejemplo:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Esta etiqueta permite describir una propiedad.

__Sintáctica__

```xml
<value>property description</value>
```

donde `property description` es una descripción de la propiedad.

__Ejemplo:__

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

Esta etiqueta se usa para describir un parámetro de tipo genérico para una clase, una estructura, una interfaz, un delegado o un método.

__Sintáctica__

```xml
<typeparam name="name">description</typeparam>
```

donde `name` es el nombre del parámetro de tipo y `description` es su descripción.

__Ejemplo:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Esta etiqueta se usa para indicar que una palabra es un parámetro de tipo. El archivo de documentación se puede procesar para dar formato a este parámetro de tipo de una manera distinta.

__Sintáctica__

```xml
<typeparamref name="name"/>
```

donde `name` es el nombre del parámetro de tipo.

__Ejemplo:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>Procesar el archivo de documentación

El generador de documentación genera una cadena de identificador para cada elemento del código fuente que se etiqueta con un Comentario de documentación. Esta cadena de identificador identifica de forma única un elemento de origen. Un visor de documentación puede usar una cadena de identificador para identificar el elemento de metadatos/reflexión correspondiente al que se aplica la documentación.

El archivo de documentación no es una representación jerárquica del código fuente. en su lugar, es una lista plana con una cadena de identificador generada para cada elemento.

### <a name="id-string-format"></a>Formato de cadena de identificador

El generador de documentación observa las siguientes reglas cuando genera las cadenas de identificador:

*  No se coloca espacio en blanco en la cadena.

*  La primera parte de la cadena identifica el tipo de miembro que se va a documentar, a través de un solo carácter seguido de un signo de dos puntos. Se definen los siguientes tipos de miembros:

   | __Carácter__ | __Descripción__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | Evento                                                       |
   | F             | Campo                                                       |
   | M             | Método (incluidos constructores, destructores y operadores) |
   | N             | Espacio de nombres                                                   |
   | P             | Propiedad (incluidos los indizadores)                               |
   | T             | Tipo (como clase, delegado, enumeración, interfaz y struct) |
   | !             | Cadena de error; en el resto de la cadena se proporciona información sobre el error. Por ejemplo, el generador de documentación genera información de error para los vínculos que no se pueden resolver. |

*  La segunda parte de la cadena es el nombre completo del elemento, que empieza en la raíz del espacio de nombres. El nombre del elemento, sus tipos envolventes y su espacio de nombres se separan por puntos. Si el nombre del propio elemento tiene puntos, se reemplazan por `#(U+0023)` caracteres. (Se supone que ningún elemento tiene este carácter en su nombre).
*  En el caso de los métodos y propiedades con argumentos, la lista de argumentos sigue entre paréntesis. Para aquellos que no tienen argumentos, se omiten los paréntesis. Los argumentos están separados por comas. La codificación de cada argumento es la misma que la firma de la CLI, como se indica a continuación:
   *  Los argumentos se representan mediante el nombre de la documentación, que se basa en su nombre completo, modificado de la siguiente manera:
      * Los argumentos que representan tipos genéricos tienen un `` ` `` carácter anexado (acento grave) seguido por el número de parámetros de tipo
      * Los argumentos que `out` tienen `ref` el modificador o `@` tienen un nombre de tipo siguiente. Los argumentos que se pasan por `params` valor o por Via no tienen ninguna notación especial.
      * Los argumentos que son matrices se representan como `[lowerbound:size, ... , lowerbound:size]` donde el número de comas es el rango menos uno, y los límites inferiores y el tamaño de cada dimensión, si se conocen, se representan en formato decimal. Si no se especifica un límite inferior o un tamaño, se omite. Si se omiten el límite inferior y el tamaño de una dimensión determinada `:` , también se omite. Las matrices escalonadas se representan en `[]` una por nivel.
      * Los argumentos que tienen tipos de puntero distintos de void se representan `*` mediante un siguiente nombre de tipo. Un puntero void se representa utilizando un nombre de tipo `System.Void`de.
      * Los argumentos que hacen referencia a los parámetros de tipo genérico definidos en los tipos se `` ` `` codifican mediante el carácter (acento grave) seguido por el índice de base cero del parámetro de tipo.
      * Los argumentos que utilizan parámetros de tipo genérico definidos en métodos utilizan un acento ``` `` ``` doble en lugar del utilizado para los `` ` `` tipos.
      * Los argumentos que hacen referencia a tipos genéricos construidos se codifican utilizando el tipo genérico, `{`seguido de, seguido de una lista separada por comas de argumentos de `}`tipo, seguida de.

### <a name="id-string-examples"></a>Ejemplos de cadenas de ID.

Los ejemplos siguientes muestran un fragmento de C# código, junto con la cadena de identificador generada a partir de cada elemento de origen capaz de tener un Comentario de documentación:

*  Los tipos se representan mediante su nombre completo, ampliados con información genérica:

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

*  Los campos se representan mediante su nombre completo:

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

*  Constructores.

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

*  Destructores.

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

*  Modalidades.

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

*  Propiedades e indizadores.

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

*  Ceso.

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

*  Operadores unarios.

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

   El conjunto completo de nombres de función de operador unario se usa `op_UnaryPlus`como se `op_LogicalNot`indica a `op_Increment`continuación `op_Decrement`: `op_True`, `op_UnaryNegation`, `op_False`, `op_OnesComplement`,,, y.

*  Operadores binarios.

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

   El conjunto completo de nombres de función de operador binario utilizado es `op_Addition`el `op_Subtraction`siguiente: `op_Division`, `op_Modulus`, `op_BitwiseAnd` `op_Multiply`,, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift` `op_BitwiseOr`,,,, `op_Equality`, ,,`op_LessThan`, y`op_GreaterThan`. `op_LessThanOrEqual` `op_Inequality` `op_GreaterThanOrEqual`

*  Los operadores de conversión tienen un "`~`" final seguido del tipo de valor devuelto.

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

## <a name="an-example"></a>Un ejemplo

### <a name="c-source-code"></a>C#código fuente

En el ejemplo siguiente se muestra el código fuente `Point` de una clase:

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

### <a name="resulting-xml"></a>XML resultante

A continuación se muestra la salida generada por un generador de documentación cuando se proporciona `Point`el código fuente de la clase, mostrado anteriormente:

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
