# <a name="documentation-comments"></a>Comentarios de documentación

C# proporciona un mecanismo para que los programadores puedan documentar el código mediante una sintaxis de comentario especial que contiene el texto XML. En los archivos de código fuente, comentarios con un formato determinado pueden usarse para dirigir una herramienta para generar XML desde estos comentarios y los elementos de código fuente que preceden. Los comentarios con esta sintaxis se denominan ***comentarios de documentación***. Debe preceder inmediatamente a un tipo definido por el usuario (por ejemplo, una clase, un delegado o una interfaz) o un miembro (por ejemplo, un campo, evento, propiedad o método). La herramienta de generación de XML se denomina el ***generador de documentación***. (Este generador podría ser, pero no es necesario, el propio compilador de C#). El resultado producido por el generador de documentación se denomina el ***archivo de documentación***. Un archivo de documentación se usa como entrada para un ***Visor de la documentación***; una herramienta diseñada para producir algún tipo de presentación visual de la información de tipo y su documentación asociada.

Esta especificación sugiere un conjunto de etiquetas que se usará en los comentarios de documentación, pero no se requiere el uso de estas etiquetas y otras etiquetas se pueden usar si lo desea, se siguen larga las reglas de XML con formato correcto.

## <a name="introduction"></a>Introducción

Comentarios con un formato especial pueden usarse para dirigir una herramienta para generar XML desde estos comentarios y los elementos de código fuente que preceden. Estos comentarios son comentarios de una línea que comienzan con tres barras diagonales (`///`), o delimitado por comentarios que comienzan con una barra diagonal y dos asteriscos (`/**`). Debe preceder inmediatamente a un tipo definido por el usuario (por ejemplo, una clase, un delegado o una interfaz) o un miembro (por ejemplo, un campo, evento, propiedad o método) que hacen referencia los comentarios. Atributo secciones ([especificación de atributos](attributes.md#attribute-specification)) se consideran parte de las declaraciones, por lo que los comentarios de documentación deben preceder a los atributos aplicados a un tipo o miembro.

__Sintaxis:__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

En un *single_line_doc_comment*, si hay un *espacio en blanco* siguiente carácter el `///` caracteres en cada uno de los *single_line_doc_comment*s adyacentes a la actual *single_line_doc_comment*, a continuación, que *espacio en blanco* carácter no se incluye en la salida XML.

En un comentario-delimitados-doc, si el primer carácter que no sean espacios en blanco en la segunda línea es un asterisco y el mismo patrón de caracteres en blanco opcional y un carácter de asterisco se repiten al principio de cada uno de la línea en el comentario-delimitados-doc, a continuación, no se incluyen los caracteres del patrón repetido en la salida XML. El patrón puede incluir caracteres de espacio en blanco después de, así como antes, el carácter de asterisco.

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

El texto dentro de los comentarios de documentación debe tener un formato correcto según las reglas de XML (https://www.w3.org/TR/REC-xml). Si el XML está enfermo formado, se genera una advertencia y el archivo de documentación incluirá un comentario que indica que se detectó un error.

Aunque los desarrolladores son gratuitos crear su propio conjunto de etiquetas, se define un conjunto recomendado de [etiquetas recomendadas](documentation-comments.md#recommended-tags). Algunas de las etiquetas recomendadas tienen significados especiales:

*  El `<param>` etiqueta a la que se usa para describir los parámetros. Si se utiliza dicha etiqueta, el generador de documentación debe comprobar que el parámetro especificado existe y que todos los parámetros se describen en los comentarios de documentación. Si se produce un error en dicha comprobación, el generador de documentación emite una advertencia.
*  El atributo `cref` se puede asociar a cualquier etiqueta para proporcionar una referencia a un elemento de código. El generador de documentación debe comprobar la existencia de este elemento de código. Si se produce un error en la comprobación, el generador de documentación emite una advertencia. Al buscar un nombre que se describe en un `cref` atributo, el generador de documentación debe respetar la visibilidad del espacio de nombres según `using` instrucciones que aparecen en el código fuente. Los elementos de código que son genéricos, la sintaxis genérica normal (es decir, "`List<T>`") no se puede usar porque no genera un XML no válido. Se pueden usar llaves en lugar de corchetes (es decir, "`List{T}`"), o se puede usar la sintaxis de escape XML (es decir, "`List&lt;T&gt;`").
*  El `<summary>` etiqueta está pensada para usarse con un visor de documentación para mostrar información adicional sobre un tipo o miembro.
*  El `<include>` tag incluye información de un archivo XML externo.

Observe que el archivo de documentación no proporciona información completa sobre los tipos y miembros (por ejemplo, no contiene ninguna información de tipos). Para obtener información acerca de un tipo o miembro, el archivo de documentación debe usarse junto con la reflexión en el tipo o miembro reales.

## <a name="recommended-tags"></a>Etiquetas recomendadas

El generador de documentación debe aceptar y procesar cualquier etiqueta válida según las reglas de XML. Las siguientes etiquetas proporcionan funcionalidad de uso común en la documentación de usuario. (Por supuesto, otras etiquetas son posibles).


| __Tag__          | __Sección__                                            | __Propósito__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Establecer el texto en una fuente como código                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Establezca una o varias líneas de salida de código o programa de origen |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Indicar un ejemplo                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Identifica las excepciones que puede iniciar un método           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Incluye el XML desde un archivo externo                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Crear una tabla o lista                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Permitir la estructura que se agregarán a texto                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Describe un parámetro para un método o constructor       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Identificar que una palabra es un nombre de parámetro               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | La accesibilidad de seguridad de un miembro de documento        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | Describe información adicional sobre un tipo           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Describir el valor devuelto de un método                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Especifique un vínculo                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Generar una entrada en Vea también                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Describir un tipo o miembro de un tipo                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Describir una propiedad                                    |
| `<typeparam>`    |                                                        | Describir un parámetro de tipo genérico                      |
| `<typeparamref>` |                                                        | Identificar que una palabra es un nombre de parámetro de tipo          |

### `<c>`

Esta etiqueta ofrece un mecanismo para indicar que un fragmento de texto dentro de una descripción debe establecerse en una fuente como los utilizados para un bloque de código especial. Para las líneas de código real, utilice `<code>` ([`<code>`](documentation-comments.md#code)).

__Sintaxis:__

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

Esta etiqueta se usa para establecer una o varias líneas de salida de código o programa de origen en alguna fuente especial. Para pequeños fragmentos de código en texto narrativo, utilice `<c>` ([`<c>`](documentation-comments.md#c)).

__Sintaxis:__

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

Esta etiqueta permite que el código de ejemplo de un comentario, para especificar cómo se puede usar un método u otro miembro de biblioteca. Normalmente, esto también implica la utilización de la etiqueta `<code>` ([`<code>`](documentation-comments.md#code)) también.

__Sintaxis:__

```xml
<example>description</example>
```

__Ejemplo:__

Consulte `<code>` ([`<code>`](documentation-comments.md#code)) para obtener un ejemplo.

### `<exception>`

Esta etiqueta proporciona una manera de documentar las excepciones que se puede producir un método.

__Sintaxis:__

```xml
<exception cref="member">description</exception>
```

donde

* `member` es el nombre de un miembro. El generador de documentación comprueba si el miembro especificado existe y traduce `member` al nombre de elemento canónico en el archivo de documentación.
* `description` es una descripción de las circunstancias en que se produce la excepción.

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

Esta etiqueta permite incluir información de un documento XML que es externo al archivo de código fuente. El archivo externo debe ser un documento XML bien formado, y una expresión XPath se aplica a ese documento para especificar qué XML de ese documento para incluir. El `<include>` etiqueta, a continuación, se reemplaza por el XML del documento externo seleccionado.

__Sintaxis:__

```
<include file="filename" path="xpath" />
```

donde

* `filename` es el nombre de archivo de un archivo XML externo. El nombre de archivo se interpreta en relación con el archivo que contiene la etiqueta de inclusión.
* `xpath` es una expresión XPath que selecciona parte del código XML en el archivo XML externo.

__Ejemplo:__

Si el código fuente contiene una declaración como:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

y el archivo externo "docs.xml" tuviera el siguiente contenido:

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

a continuación, la documentación del misma es el resultado como si el código fuente contenidos:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Esta etiqueta se utiliza para crear una lista o tabla de elementos. Puede contener un `<listheader>` bloque para definir la fila de encabezado de una tabla o una definición de lista. (Cuando se define una tabla, solo una entrada para `term` en el encabezado debe proporcionarse.)

Cada elemento de la lista se especifica con un `<item>` bloque. Al crear una lista de definiciones, ambos `term` y `description` debe especificarse. Sin embargo, para una tabla, lista con viñetas o lista numerada, solo `description` debe especificarse.

__Sintaxis:__

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

* `term` es el término para definir, cuya definición se encuentra en `description`.
* `description` es un elemento de viñeta o una lista numerada o la definición de un `term`.

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

Esta etiqueta es para su uso dentro de otras etiquetas, como `<summary>` ([`<remark>`](documentation-comments.md#remark)) o `<returns>` ([`<returns>`](documentation-comments.md#returns)) y permite agregar al texto de la estructura.

__Sintaxis:__

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

Esta etiqueta se utiliza para describir un parámetro para un método, constructor o indizador.

__Sintaxis:__

```xml
<param name="name">description</param>
```

donde

* `name` Es el nombre del parámetro.
* `description` es una descripción del parámetro.

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

Esta etiqueta se utiliza para indicar que una palabra es un parámetro. El archivo de documentación se puede procesar para dar formato a este parámetro de alguna manera distinta.

__Sintaxis:__

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

Esta etiqueta permite la accesibilidad de seguridad de un miembro que va a documentarse.

__Sintaxis:__

```xml
<permission cref="member">description</permission>
```

donde

* `member` es el nombre de un miembro. El generador de documentación comprueba si el elemento de código dado existe y traduce *miembro* al nombre de elemento canónico en el archivo de documentación.
* `description` es una descripción del acceso al miembro.

__Ejemplo:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

Esta etiqueta se utiliza para especificar información adicional sobre un tipo. (Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) para describir el propio tipo y los miembros de un tipo.)

__Sintaxis:__

```xml
<remark>description</remark>
```

donde `description` es el texto del comentario.

__Ejemplo:__

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

Esta etiqueta se utiliza para describir el valor devuelto de un método.

__Sintaxis:__

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

Esta etiqueta permite un vínculo al especificarse dentro del texto. Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) para indicar el texto que va a aparecer en una sección Vea también.

__Sintaxis:__

```xml
<see cref="member"/>
```

donde `member` es el nombre de un miembro. El generador de documentación comprueba si el elemento de código dado existe y cambia *miembro* al nombre de elemento en el archivo de documentación generado.

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

Esta etiqueta permite una entrada que se generará para la sección Vea también. Use `<see>` ([`<see>`](documentation-comments.md#see)) para especificar un vínculo desde dentro del texto.

__Sintaxis:__

```xml
<seealso cref="member"/>
```

donde `member` es el nombre de un miembro. El generador de documentación comprueba si el elemento de código dado existe y cambia *miembro* al nombre de elemento en el archivo de documentación generado.

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

Esta etiqueta se puede usar para describir un tipo o un miembro de un tipo. Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) para describir el propio tipo.

__Sintaxis:__

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

Esta etiqueta permite que una propiedad para describirse.

__Sintaxis:__

```xml
<value>property description</value>
```

donde `property description` es una descripción para la propiedad.

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

Esta etiqueta se utiliza para describir un parámetro de tipo genérico para una clase, struct, interfaz, delegado o método.

__Sintaxis:__

```xml
<typeparam name="name">description</typeparam>
```

donde `name` es el nombre del parámetro de tipo, y `description` es su descripción.

__Ejemplo:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Esta etiqueta se utiliza para indicar que una palabra es un parámetro de tipo. El archivo de documentación se puede procesar para dar formato a este parámetro de tipo de alguna manera distinta.

__Sintaxis:__

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

El generador de documentación genera una cadena de identificador para cada elemento en el código fuente que se etiqueta con un comentario de documentación. Esta cadena de identificador identifica de forma única un elemento de origen. Un visor de la documentación puede utilizar una cadena de identificador para identificar el elemento de reflexión o de metadatos correspondiente a la que se aplica la documentación.

El archivo de documentación no es una representación jerárquica del código fuente; en su lugar, es una lista plana con una cadena de identificador generada para cada elemento.

### <a name="id-string-format"></a>Formato de cadena de Id.

El generador de documentación cumple las siguientes reglas cuando genera las cadenas de identificación:

*  No se coloca espacio en blanco en la cadena.

*  La primera parte de la cadena identifica el tipo de miembro que se documentan a través de un único carácter seguido de dos puntos. Se definen los siguientes tipos de miembros:

   | __Carácter__ | __Descripción__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | evento                                                       |
   | F             | Campo                                                       |
   | M             | Método (incluidos constructores, destructores y operadores) |
   | N             | Espacio de nombres                                                   |
   | P             | Propiedad (incluidos indizadores)                               |
   | T             | Escriba (por ejemplo, la clase, delegado, enumeración, interfaz y struct) |
   | !             | Cadena de error; el resto de la cadena proporciona información sobre el error. Por ejemplo, el generador de documentación genera información de error para vínculos que no se puede resolver. |

*  La segunda parte de la cadena es el nombre completo del elemento, empezando por la raíz del espacio de nombres. El nombre del elemento, su tipos envolventes y el espacio de nombres están separados por puntos. Si el nombre del elemento ya contiene puntos, éstos se reemplazan por `#(U+0023)` caracteres. (Se supone que ningún elemento tiene este carácter en su nombre.)
*  Para los métodos y propiedades con argumentos, el argumento de lista se indica a continuación, incluya entre paréntesis. Para aquellos sin argumentos, se omiten los paréntesis. Los argumentos están separados por comas. La codificación de cada argumento es el mismo que una firma de la CLI, como sigue:
   *  Los argumentos se representan por su nombre de la documentación, que se basa en su nombre completo, puede modificado como sigue:
      * Argumentos que representan tipos genéricos tienen un anexados `` ` `` carácter (comilla simple) seguido del número de parámetros de tipo
      * Argumentos que contienen el `out` o `ref` modificador tiene una `@` siguiendo su nombre de tipo. Argumentos pasan por valor o a través de `params` no tienen una ninguna anotación especial.
      * Argumentos que son matrices se representan como `[lowerbound:size, ... , lowerbound:size]` donde el número de comas es el rango menos 1, y los límites inferiores y el tamaño de cada dimensión, si se conocen, se representan en formato decimal. Si no se especifica un límite inferior ni el tamaño, se omite. Si se omiten el límite inferior y el tamaño de una dimensión determinada, el `:` también se omite. Matrices escalonadas se representan mediante uno `[]` por nivel.
      * Argumentos que tienen tipos de puntero no es void se representan mediante un `*` después del nombre de tipo. Un puntero void se representa mediante un nombre de tipo de `System.Void`.
      * Argumentos que hacen referencia a parámetros de tipo genérico que se definen en los tipos se codifican utilizando el `` ` `` carácter (comilla simple) seguido por el índice de base cero del parámetro de tipo.
      * Argumentos que usan parámetros de tipo genérico definidos en los métodos use un doble-acento grave ``` `` ``` en lugar de la `` ` `` utiliza para los tipos.
      * Argumentos que hacen referencia a tipos genéricos construidos se codifican utilizando el tipo genérico, seguido de `{`, seguido de una lista separada por comas de argumentos de tipo, seguido de `}`.

### <a name="id-string-examples"></a>Ejemplos de cadenas de identificador

Los ejemplos siguientes muestra un fragmento de código C#, junto con la cadena de identificador generada a partir de cada elemento de origen pueden tener un comentario de documentación:

*  Los tipos se representan mediante su nombre completo, ampliada con información genérica:

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

*  Métodos.

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

*  eventos.

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

   El conjunto completo de los nombres de función de operador unario utilizado es como sigue: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, y `op_False`.

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

   El conjunto completo de usa los nombres de función de operador binario es como sigue: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, y `op_GreaterThanOrEqual`.

*  Los operadores de conversión tienen un carácter final "`~`" seguido por el tipo de valor devuelto.

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

### <a name="c-source-code"></a>Código fuente de C#

El ejemplo siguiente muestra el código fuente de un `Point` clase:

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

Este es el resultado producido por un generador de documentación cuando se especifica el código fuente para la clase `Point`, como se muestra anteriormente:

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
