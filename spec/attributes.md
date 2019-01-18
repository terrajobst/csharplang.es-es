---
ms.openlocfilehash: a8ad8a8b3eda1d00fa745bd92e4371eacc36b79f
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229739"
---
# <a name="attributes"></a>Atributos

Gran parte del lenguaje C# permite al programador especificar información declarativa sobre las entidades definidas en el programa. Por ejemplo, la accesibilidad de un método en una clase se especifica al decorarlo con el *method_modifier*s `public`, `protected`, `internal`, y `private`.

C# permite a los programadores inventar nuevas clases de información declarativa, denominadas ***atributos***. Los programadores, a continuación, pueden asociar los atributos a varias entidades de programa y recuperar información de atributos en un entorno de tiempo de ejecución. Por ejemplo, podría definir un marco de trabajo un `HelpAttribute` atributo que se puede colocar en determinados elementos de programa (por ejemplo, clases y métodos) para proporcionar una asignación de los elementos de programa a su documentación.

Los atributos se definen mediante la declaración de clases de atributos ([clases de atributo](attributes.md#attribute-classes)), que puede tener parámetros posicionales y con nombre ([posicional y parámetros con nombre](attributes.md#positional-and-named-parameters)). Los atributos están asociados a las entidades de un programa de C# mediante las especificaciones de atributo ([especificación de atributos](attributes.md#attribute-specification)) y se pueden recuperar en tiempo de ejecución como instancias de atributo ([instancias de atributos](attributes.md#attribute-instances)).

## <a name="attribute-classes"></a>Clases de atributos

Una clase que derive de la clase abstracta `System.Attribute`, ya sea directa o indirectamente, es un ***clase de atributos***. La declaración de una clase de atributo define un nuevo tipo de ***atributo*** que puede colocarse en una declaración. Por convención, las clases de atributos se denominan con el sufijo `Attribute`. Usos de un atributo pueden incluir u omitir este sufijo.

### <a name="attribute-usage"></a>Uso de atributos

El atributo `AttributeUsage` ([atributo AttributeUsage The](attributes.md#the-attributeusage-attribute)) se usa para describir cómo se puede usar una clase de atributo.

`AttributeUsage` tiene un parámetro posicional ([posicional y parámetros con nombre](attributes.md#positional-and-named-parameters)) que permite que una clase de atributo especificar los tipos de declaraciones en el que se puede usar. El ejemplo

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

define una clase de atributo denominada `SimpleAttribute` que puede colocarse en *class_declaration*s y *interface_declaration*s solo. El ejemplo

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

se muestran varios usos de la `Simple` atributo. Aunque este atributo se define con el nombre `SimpleAttribute`, cuando se utiliza este atributo, el `Attribute` sufijo puede omitirse, lo que resulta en el nombre corto `Simple`. Por lo tanto, el ejemplo anterior es semánticamente equivalente a la siguiente:

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

`AttributeUsage` tiene un parámetro con nombre ([posicional y parámetros con nombre](attributes.md#positional-and-named-parameters)) llamado `AllowMultiple`, lo que indica si el atributo se puede especificar más de una vez para una entidad determinada. Si `AllowMultiple` para un atributo de clase es true, entonces esa clase de atributo es un ***clase de atributos multiuso***y se puede especificar más de una vez en una entidad. Si `AllowMultiple` para un atributo de clase es false o no se especifica y, a continuación, esa clase de atributo es un ***clase de atributo de uso único***y se puede especificar como máximo una vez en una entidad.

El ejemplo

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

define una clase de atributo de uso múltiple denominada `AuthorAttribute`. El ejemplo

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

muestra una declaración de clase con dos usos de la `Author` atributo.

`AttributeUsage` tiene otro parámetro con nombre denominado `Inherited`, lo que indica si el atributo, cuando se especifican en una clase base, también es heredado por las clases que derivan de esa clase base. Si `Inherited` para un atributo de clase es true, entonces ese atributo es heredado. Si `Inherited` para un atributo de clase es false, no se hereda ese atributo. Si no se especifica, su valor predeterminado es true.

Una clase de atributo `X` no tener un `AttributeUsage` atributo asociado a él, como en

```csharp
using System;

class X: Attribute {...}
```

es equivalente a la siguiente:

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a>Parámetros posicionales y con nombre

Las clases de atributo pueden tener ***parámetros posicionales*** y ***parámetros con nombre***. Cada constructor de instancia pública de una clase de atributo define una secuencia válida de parámetros posicionales para esa clase de atributos. Cada campo de lectura y escritura público no estático y la propiedad para una clase de atributo definen un parámetro con nombre para la clase de atributo.

El ejemplo

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

define una clase de atributo denominada `HelpAttribute` que tiene un parámetro posicional, `url`y un parámetro con nombre, `Topic`. Aunque es no estático y público, la propiedad `Url` no define un parámetro con nombre, ya que no es de lectura y escritura.

Esta clase de atributo se puede usar como sigue:

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a>Tipos de parámetro de atributo

Los tipos de parámetros con nombre y posicionales para una clase de atributo se limitan a la ***tipos de parámetro de atributo***, que son:

*  Uno de los siguientes tipos: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.
*  El tipo `object`.
*  El tipo `System.Type`.
*  Proporcionó un tipo de enumeración, tiene accesibilidad pública y los tipos en la que está anidado (si existe) también tienen accesibilidad pública ([especificación de atributos](attributes.md#attribute-specification)).
*  Matrices unidimensionales de los tipos anteriores.
*  Un argumento de constructor o un campo público que no tiene uno de estos tipos, no se puede usar como un parámetro posicional o con nombre en una especificación de atributo.

## <a name="attribute-specification"></a>Especificación de atributo

***Especificación de atributos*** es la aplicación de un atributo definido previamente en una declaración. Un atributo es un fragmento de información declarativa adicional que se especifica para una declaración. Atributos que se pueden especificar en el ámbito global (para especificar atributos en el ensamblado o módulo que lo contiene) y para *type_declaration*s ([declaraciones de tipo](namespaces.md#type-declarations)), *class_member_declaration* s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([miembros de interfaz](interfaces.md#interface-members)), *struct_member _declaration*s ([los miembros de Struct](structs.md#struct-members)), *enum_member_declaration*s ([miembros de enumeración](enums.md#enum-members)), *accessor_declarations*  ([Descriptores de acceso](classes.md#accessors)), *event_accessor_declarations* ([eventos como campos](classes.md#field-like-events)), y *formal_parameter_list*s ([parámetros del método](classes.md#method-parameters)).

Los atributos se especifican en ***atributo secciones***. Una sección de atributos consta de un par de corchetes que encierran una lista separada por comas de uno o varios atributos. El orden en el que se especifican los atributos en la lista y se organizan el orden de las secciones adjuntas a la misma entidad de programa, no es significativo. Por ejemplo, las especificaciones del atributo `[A][B]`, `[B][A]`, `[A,B]`, y `[B,A]` son equivalentes.

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

Un atributo consta de un *attribute_name* y una lista opcional de argumentos posicionales y con nombre. Los argumentos posicionales (si existe) preceden a los argumentos con nombre. Un argumento posicional se compone de un *attribute_argument_expression*; un argumento con nombre consta de un nombre, seguido por un signo igual, seguido por un *attribute_argument_expression*, que, juntos , están restringidas por las mismas reglas de asignación simple. El orden de los argumentos con nombre no es significativo.

El *attribute_name* identifica una clase de atributo. Si el formulario de *attribute_name* es *type_name* , a continuación, este nombre debe hacer referencia a una clase de atributo. De lo contrario, se produce un error en tiempo de compilación. El ejemplo

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

da como resultado un error en tiempo de compilación porque intenta utilizar `Class1` como un atributo de clase cuando `Class1` no es una clase de atributo.

Algunos contextos permiten la especificación de un atributo en más de un destino. Un programa puede especificar explícitamente el destino mediante la inclusión de un *attribute_target_specifier*. Cuando se coloca un atributo en el nivel global, un *global_attribute_target_specifier* es necesario. En todas las demás ubicaciones, se aplica un valor predeterminado razonable, pero un *attribute_target_specifier* puede usarse para afirmar o reemplazar el valor predeterminado en ciertos casos ambiguos (o sólo para afirmar el valor predeterminado en los casos que no sean ambiguas). Por lo tanto, normalmente, *attribute_target_specifier*s puede omitirse, excepto en el nivel global. Los contextos potencialmente ambiguos se resuelven como sigue:

*  Puede aplicar un atributo especificado en el ámbito global en el ensamblado de destino o en el módulo de destino. Existe ningún valor predeterminado para este contexto, por lo tanto un *attribute_target_specifier* siempre es necesaria en este contexto. La presencia de la `assembly` *attribute_target_specifier* indica que el atributo se aplica al destino ensamblado; la presencia de la `module` *attribute_target_specifier* indica que el atributo se aplica al módulo de destino.
*  Puede aplicar un atributo especificado en una declaración de delegado para el delegado que se declara o a su valor devuelto. En ausencia de un *attribute_target_specifier*, el atributo se aplica al delegado. La presencia de la `type` *attribute_target_specifier* indica que el atributo se aplica al delegado; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.
*  Puede aplicar un atributo especificado en una declaración de método para el método que se declara o a su valor devuelto. En ausencia de un *attribute_target_specifier*, el atributo se aplica al método. La presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica al método; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.
*  Puede aplicar un atributo especificado en una declaración de operador para el operador que se declara o a su valor devuelto. En ausencia de un *attribute_target_specifier*, el atributo se aplica al operador. La presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica al operador; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.
*  Un atributo especificado en una declaración de evento que omite los descriptores de acceso de eventos puede aplicar al evento que se declara, al campo asociado (si el evento no es abstracto) o el asociado agregar y quitar métodos. En ausencia de un *attribute_target_specifier*, el atributo se aplica al evento. La presencia de la `event` *attribute_target_specifier* indica que el atributo se aplica al evento; la presencia de la `field` *attribute_target_specifier* indica que el atributo se aplica al campo; y la presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica a los métodos.
*  Puede aplicar un atributo especificado en una declaración de descriptor de acceso get de una declaración de propiedad o indizador al método asociado o a su valor devuelto. En ausencia de un *attribute_target_specifier*, el atributo se aplica al método. La presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica al método; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.
*  Puede aplicar un atributo especificado en un descriptor de acceso para una declaración de propiedad o indizador al método asociado o a su parámetro implícito. En ausencia de un *attribute_target_specifier*, el atributo se aplica al método. La presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica al método; la presencia de la `param` *attribute_target_specifier* indica que el atributo se aplica al parámetro; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.
*  Un atributo especificado en una declaración de descriptor de acceso add o remove para una declaración de evento se aplica al método asociado o a su parámetro. En ausencia de un *attribute_target_specifier*, el atributo se aplica al método. La presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica al método; la presencia de la `param` *attribute_target_specifier* indica que el atributo se aplica al parámetro; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.

En otros contextos, la inclusión de un *attribute_target_specifier* está permitido pero innecesarios. Por ejemplo, puede incluir u omitir el especificador de una declaración de clase `type`:

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

Es un error especificar no es válido *attribute_target_specifier*. Por ejemplo, el especificador de `param` no se puede usar en una declaración de clase:

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

Por convención, las clases de atributos se denominan con el sufijo `Attribute`. Un *attribute_name* del formulario *type_name* puede incluir u omitir este sufijo. Si se encuentra una clase de atributos con y sin este sufijo, una ambigüedad está presente y se producirá un error de tiempo de compilación. Si el *attribute_name* está escrito tal que su derecha *identificador* es un identificador textual ([identificadores](lexical-structure.md#identifiers)), a continuación, se asocia a solo un atributo sin un sufijo, lo que permite que se resuelva una ambigüedad. El ejemplo

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

muestra dos clases de atributos denominados `X` y `XAttribute`. El atributo `[X]` es ambiguo, porque podría referirse a cualquiera `X` o `XAttribute`. El uso de un identificador textual permite la intención exacta que se especifique en estos casos poco frecuentes. El atributo `[XAttribute]` no es ambiguo (aunque lo sería si se ha producido una clase de atributo denominada `XAttributeAttribute`!). Si la declaración de clase `X` se quita, a continuación, ambos atributos hacen referencia a la clase de atributo denominada `XAttribute`, como sigue:

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

Es un error en tiempo de compilación para usar una clase de atributo de uso único más de una vez en la misma entidad. El ejemplo

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

da como resultado un error en tiempo de compilación porque intenta utilizar `HelpString`, que es una clase de atributo de uso único, más de una vez en la declaración de `Class1`.

Una expresión `E` es un *attribute_argument_expression* si se cumplen todas las instrucciones siguientes:

*  El tipo de `E` es un tipo de parámetro de atributo ([tipos de parámetro de atributo](attributes.md#attribute-parameter-types)).
*  En tiempo de compilación, el valor de `E` puede resolverse en uno de los siguientes:
   * Un valor constante.
   * Objeto `System.Type`.
   * Una matriz unidimensional de *attribute_argument_expression*s.

Por ejemplo:

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

Un *typeof_expression* ([el operador typeof](expressions.md#the-typeof-operator)) utiliza como una expresión de argumento de atributo puede hacer referencia a un tipo no genérico, un tipo construido cerrado o un tipo genérico sin enlazar, pero no se puede hacer referencia a un Abra el tipo. Esto es para asegurarse de que la expresión se puede resolver en tiempo de compilación.

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a>Instancias de atributo

Un ***instancia de atributo*** es una instancia que representa un atributo en tiempo de ejecución. Un atributo está había definido con una clase de atributos, los argumentos posicionales y argumentos con nombre. Una instancia de atributo es una instancia de la clase de atributo que se inicializa con los argumentos posicionales y con nombre.

Recuperación de una instancia de atributo implica el procesamiento de tiempo de compilación y tiempo de ejecución, como se describe en las secciones siguientes.

### <a name="compilation-of-an-attribute"></a>Compilación de un atributo

La compilación de un *atributo* con la clase de atributo `T`, *positional_argument_list* `P` y *named_argument_list* `N`, consta de los pasos siguientes:

*  Siga los pasos de procesamiento en tiempo de compilación para compilar un *object_creation_expression* del formulario `new T(P)`. Estos pasos dan lugar a un error en tiempo de compilación o determinar un constructor de instancia `C` en `T` que se puede invocar en tiempo de ejecución.
*  Si `C` no tiene accesibilidad pública y, a continuación, se produce un error de tiempo de compilación.
*  Para cada *named_argument* `Arg` en `N`:
   * Permiten `Name` ser el *identificador* de la *named_argument* `Arg`.
   * `Name` debe identificar un campo público de lectura y escritura no estático o una propiedad en `T`. Si `T` tiene no existe ese campo o propiedad, a continuación, se produce un error de tiempo de compilación.
*  Recuerde la siguiente información para la creación de instancias de tiempo de ejecución del atributo: la clase de atributo `T`, el constructor de instancia `C` en `T`, *positional_argument_list* `P` y el *named_argument_list* `N`.

### <a name="run-time-retrieval-of-an-attribute-instance"></a>Recuperación de tiempo de ejecución de una instancia de atributo

Compilación de un *atributo* da como resultado una clase de atributo `T`, un constructor de instancia `C` en `T`, un *positional_argument_list* `P`y un *named_argument_list* `N`. Dada esta información, se puede recuperar una instancia de atributo en tiempo de ejecución mediante los siguientes pasos:

*  Siga los pasos de procesamiento en tiempo de ejecución para ejecutar un *object_creation_expression* del formulario `new T(P)`, utilizando el constructor de instancia `C` como se determina en tiempo de compilación. Estos pasos dan lugar a una excepción, o generar una instancia `O` de `T`.
*  Para cada *named_argument* `Arg` en `N`, en orden:
   * Permiten `Name` ser el *identificador* de la *named_argument* `Arg`. Si `Name` no identifica un campo de lectura y escritura público no estático o una propiedad en `O`, a continuación, se produce una excepción.
   * Permiten `Value` ser el resultado de evaluar la *attribute_argument_expression* de `Arg`.
   * Si `Name` identifica un campo en `O`, a continuación, establezca este campo en `Value`.
   * En caso contrario, `Name` identifica una propiedad en `O`. Establezca esta propiedad en `Value`.
   * El resultado es `O`, una instancia de la clase de atributo `T` que se ha inicializado con el *positional_argument_list* `P` y *named_argument_list* `N`.

## <a name="reserved-attributes"></a>Atributos reservados

Un pequeño número de atributos afecta al lenguaje de alguna manera. Estos atributos incluyen:

*  `System.AttributeUsageAttribute` ([Atributo AttributeUsage the](attributes.md#the-attributeusage-attribute)), que se usa para describir las formas en que se puede usar una clase de atributo.
*  `System.Diagnostics.ConditionalAttribute` ([El atributo Conditional](attributes.md#the-conditional-attribute)), que se usa para definir métodos condicionales.
*  `System.ObsoleteAttribute` ([Obsoleto el atributo](attributes.md#the-obsolete-attribute)), que se usa para marcar un miembro como obsoleto.
*  `System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` y `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([atributos de información del llamador](attributes.md#caller-info-attributes)), que se usan para proporcionar información sobre el contexto de llamada a los parámetros opcionales.

### <a name="the-attributeusage-attribute"></a>El atributo AttributeUsage

El atributo `AttributeUsage` se usa para describir la manera en que se puede usar la clase de atributo.

Una clase decorada con el `AttributeUsage` atributo debe derivar de `System.Attribute`, directa o indirectamente. De lo contrario, se produce un error en tiempo de compilación.

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a>El atributo Conditional

El atributo `Conditional` permite la definición de ***métodos condicionales*** y ***clases de atributos condicionales***.

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a>Métodos condicionales

Un método decorado con el `Conditional` atributo es un método condicional. El `Conditional` atributo indica una condición mediante la comprobación de un símbolo de compilación condicional. Las llamadas a un método condicional se incluyen o se omiten en función de si se define este símbolo en el momento de la llamada. Si el símbolo está definido, la llamada se incluye; en caso contrario, se omite la llamada (incluida la evaluación del receptor y los parámetros de la llamada).

Un método condicional está sujeto a las restricciones siguientes:

*  El método condicional debe ser un método en un *class_declaration* o *struct_declaration*. Se produce un error de tiempo de compilación si el `Conditional` se especifica el atributo en un método en una declaración de interfaz.
*  El método condicional debe tener un tipo de valor devuelto de `void`.
*  El método condicional no se debe marcar con el `override` modificador. Un método condicional puede marcarse con el `virtual` modificador, sin embargo. Invalidaciones de este método son implícitamente condicionales y no deben marcarse explícitamente con un `Conditional` atributo.
*  El método condicional no debe ser una implementación de un método de interfaz. De lo contrario, se produce un error en tiempo de compilación.

Además, se produce un error de tiempo de compilación si un método condicional se usa en un *delegate_creation_expression*. El ejemplo

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

declara `Class1.M` como un método condicional. `Class2`del `Test` método llama a este método. Desde el símbolo de compilación condicional `DEBUG` se define, si `Class2.Test` es llamado, llamará a `M`. Si el símbolo `DEBUG` no estaba definido, a continuación, `Class2.Test` no llamaría a `Class1.M`.

Es importante tener en cuenta que la inclusión o exclusión de una llamada a un método condicional se controla mediante los símbolos de compilación condicional en el momento de la llamada. En el ejemplo

Archivo `class1.cs`:

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

Archivo `class2.cs`:

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

Archivo `class3.cs`:

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

las clases `Class2` y `Class3` cada uno contiene las llamadas al método condicional `Class1.F`, que es condicional o no según `DEBUG` está definido. Puesto que este símbolo se define en el contexto de `Class2` pero no `Class3`, la llamada a `F` en `Class2` se incluye, mientras que la llamada a `F` en `Class3` se omite.

El uso de métodos condicionales en una cadena de herencia puede resultar confuso. Las llamadas realizadas a un método condicional mediante `base`, del formulario `base.M`, están sujetos a las reglas de llamada de método condicional normal. En el ejemplo

Archivo `class1.cs`:

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

Archivo `class2.cs`:

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

Archivo `class3.cs`:

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

`Class2` incluye una llamada a la `M` definido en su clase base. Esta llamada se omite porque el método base es condicional basándose en la presencia del símbolo `DEBUG`, que no está definido. Por lo tanto, el método escribe en la consola "`Class2.M executed`" sólo. Un uso razonable de *pp_declaration*s puede eliminar tales problemas.

#### <a name="conditional-attribute-classes"></a>Clases de atributo Conditional

Una clase de atributos ([clases de atributo](attributes.md#attribute-classes)) decorada con uno o varios `Conditional` atributos es un ***clase de atributo condicional***. Una clase de atributo condicional, por tanto, se asocia con los símbolos de compilación condicional declarados en sus `Conditional` atributos. En este ejemplo:

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

declara `TestAttribute` como una clase de atributos condicionales asociada con los símbolos de compilaciones condicionales `ALPHA` y `BETA`.

Las especificaciones del atributo ([especificación de atributos](attributes.md#attribute-specification)) de un atributo conditional se incluyen si uno o varios de sus símbolos de compilación condicional asociado se definen en el punto de especificación, en caso contrario, el atributo se omite la especificación.

Es importante tener en cuenta que la inclusión o exclusión de una especificación de atributo de una clase de atributo conditional se controla mediante los símbolos de compilación condicional en el momento de la especificación. En el ejemplo

Archivo `test.cs`:

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

Archivo `class1.cs`:

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

Archivo `class2.cs`:

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

las clases `Class1` y `Class2` son decoradas con el atributo `Test`, que es condicional o no según `DEBUG` está definido. Puesto que este símbolo se define en el contexto de `Class1` pero no `Class2`, la especificación de la `Test` atributo `Class1` se incluye, mientras la especificación de la `Test` atributo `Class2` se omite.

### <a name="the-obsolete-attribute"></a>El atributo Obsolete

El atributo `Obsolete` se usa para marcar los tipos y miembros de tipos que ya no se deben usar.

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

Si un programa usa un tipo o miembro que está decorada con el `Obsolete` atributo, el compilador emite una advertencia o un error. En concreto, el compilador emite una advertencia si no se proporciona ningún parámetro de error, o si el parámetro de error se proporciona y tiene el valor `false`. El compilador emite un error si el parámetro de error especificado y tiene el valor `true`.

En el ejemplo

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

la clase `A` está decorada con el `Obsolete` atributo. Cada uso de `A` en `Main` produce una advertencia que incluye el mensaje especificado, "esta clase está obsoleta; Use la clase B en su lugar."

### <a name="caller-info-attributes"></a>Atributos de información del llamador

Para fines como registros e informes, a veces resulta útil para un miembro de función obtener determinada información de tiempo de compilación sobre el código de llamada. Los atributos de información de llamador proporcionan una manera de pasar esa información de forma transparente.

Si un parámetro opcional está anotado con uno de los atributos de información del llamador, si se omite el argumento correspondiente en una llamada no hace que necesariamente el valor de parámetro predeterminado que se sustituirá. En su lugar, si está disponible la información especificada sobre el contexto de llamada, esa información se pasarán como el valor del argumento.

Por ejemplo:

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

Una llamada a `Log()` sin argumentos imprimía la línea número y la ruta de la llamada, así como el nombre del miembro en el que se ha producido la llamada.

Atributos de información del autor de la llamada pueden producirse en los parámetros opcionales en cualquier lugar, incluido en las declaraciones de delegado. Sin embargo, los atributos de información de llamador concreto tengan restricciones en los tipos de los parámetros que puede atribuir, por lo que siempre habrá una conversión implícita de un valor sustituido para el tipo de parámetro.

Es un error con el mismo atributo de información del llamador en un parámetro de tanto la definición e implementación de parte de una declaración de método parcial. Se aplican sólo atributos de información de llamador en la parte de la definición, mientras que se omiten los atributos de información del autor de llamada que se producen solo en la parte de la implementación.

Información del llamador no afecta a la resolución de sobrecarga. Como todavía se omiten los parámetros opcionales con atributos del código fuente del llamador, la resolución de sobrecarga ignora los parámetros de la misma manera omite otros parámetros opcionales se omite ([de resolución de sobrecarga](expressions.md#overload-resolution)).

Información del llamador se sustituye solo cuando una función se invoca explícitamente en el código fuente. Invocaciones implícitas como llamadas de constructor de la ventana primaria implícita no tiene una ubicación de origen y no sustituirá la información del llamador. Además, las llamadas que se enlazan dinámicamente no sustituirá a la información del llamador. Cuando una información del llamador con atributos de parámetro se omite en tales casos, el valor predeterminado especificado del parámetro se usa en su lugar.

Una excepción son las expresiones de consulta. Se consideran las expansiones sintácticas y si las llamadas se expanden para omitir los parámetros opcionales con atributos de información del llamador, se sustituirá la información del llamador. La ubicación utilizada es la ubicación de la cláusula de consulta que generó la llamada.

Si se especifica más de un atributo de información del llamador en un parámetro determinado, están la preferida en el siguiente orden: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.

#### <a name="the-callerlinenumber-attribute"></a>El atributo CallerLineNumber

El `System.Runtime.CompilerServices.CallerLineNumberAttribute` se permite en parámetros opcionales cuando no hay una conversión implícita estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) desde el valor constante `int.MaxValue` para el tipo del parámetro. Esto garantiza que se puede pasar cualquier número de línea no negativo hasta ese valor sin errores.

En caso de una invocación de función desde una ubicación en el código fuente omite un parámetro opcional con el `CallerLineNumberAttribute`, un literal numérico que representa el número de línea de la ubicación se utiliza como argumento a la invocación en lugar del valor de parámetro predeterminado.

Si la invocación abarca varias líneas, la línea elegida es depende de la implementación.

Tenga en cuenta que el número de línea puede verse afectado por `#line` directivas ([línea directivas](lexical-structure.md#line-directives)).

#### <a name="the-callerfilepath-attribute"></a>El atributo CallerFilePath

El `System.Runtime.CompilerServices.CallerFilePathAttribute` se permite en parámetros opcionales cuando no hay una conversión implícita estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) desde `string` para el tipo del parámetro.

En caso de una invocación de función desde una ubicación en el código fuente omite un parámetro opcional con el `CallerFilePathAttribute`, a continuación, se usa un literal de cadena que representa la ruta de la ubicación del archivo como un argumento a la invocación en lugar del valor de parámetro predeterminado.

El formato de la ruta de acceso de archivo es depende de la implementación.

Tenga en cuenta que la ruta de acceso puede verse afectado por `#line` directivas ([línea directivas](lexical-structure.md#line-directives)).

#### <a name="the-callermembername-attribute"></a>El atributo CallerMemberName

El `System.Runtime.CompilerServices.CallerMemberNameAttribute` se permite en parámetros opcionales cuando no hay una conversión implícita estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) desde `string` para el tipo del parámetro.

Si una invocación de función desde una ubicación dentro del cuerpo de un miembro de función o dentro de un atributo aplicado al miembro de función propio o su tipo de valor devuelto, los parámetros o parámetros de tipo en el código fuente omite un parámetro opcional con el `CallerMemberNameAttribute`, un literal de cadena que representa el nombre de ese miembro se usa como argumento a la invocación en lugar del valor de parámetro predeterminado.

Para las llamadas que se producen dentro de los métodos genéricos, se usa solo el nombre del método propio, sin la lista de parámetros de tipo.

Para las llamadas que se producen dentro de las implementaciones de miembros de interfaz explícita, se usa solo el nombre del método propio, sin la calificación de interfaz anterior.

Para invocaciones que se producen dentro de los descriptores de acceso de propiedad o evento, el nombre de miembro utilizado es de la propiedad o el propio evento.

Para las llamadas que se producen dentro de los descriptores de acceso, el nombre de miembro utilizado es que proporcionada por un `IndexerNameAttribute` ([el atributo IndexerName](attributes.md#the-indexername-attribute)) en el miembro de indizador, si está presente, o el nombre predeterminado `Item` en caso contrario.

Para las llamadas que se producen dentro de las declaraciones de constructores de instancias, los constructores estáticos, los destructores y operadores miembro nombre utilizado es depende de la implementación.

## <a name="attributes-for-interoperation"></a>Atributos para la interoperación

Nota: En esta sección solo es aplicable a la implementación de Microsoft .NET C#.

### <a name="interoperation-with-com-and-win32-components"></a>Interoperabilidad con componentes COM y Win32

El tiempo de ejecución .NET proporciona un gran número de atributos que permiten que los programas de C# interoperar con componentes escritos con COM y las DLL de Win32. Por ejemplo, el `DllImport` atributo se puede usar en un `static extern` método para indicar que la implementación del método es que se encuentren en un archivo DLL de Win32. Estos atributos se encuentran en el `System.Runtime.InteropServices` espacio de nombres y documentación detallada acerca de estos atributos se encuentra en la documentación de .NET en tiempo de ejecución.

### <a name="interoperation-with-other-net-languages"></a>Interoperación con otros lenguajes de .NET

#### <a name="the-indexername-attribute"></a>El atributo IndexerName

Los indizadores se implementan en .NET mediante las propiedades indizadas y tengan un nombre en los metadatos de .NET. Si no hay ningún `IndexerName` está presente para un indizador, a continuación, el nombre del atributo `Item` se usa de forma predeterminada. El `IndexerName` atributo permite que un desarrollador al invalidar este comportamiento predeterminado y especifique un nombre diferente.

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
