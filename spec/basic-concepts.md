---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704007"
---
# <a name="basic-concepts"></a>Conceptos básicos

## <a name="application-startup"></a>Inicio de la aplicación

Un ensamblado que tiene un ***punto de entrada*** se denomina ***aplicación***. Cuando se ejecuta una aplicación, se crea un nuevo ***dominio de aplicación*** . Puede haber varias instancias diferentes de una aplicación en el mismo equipo al mismo tiempo, y cada una tiene su propio dominio de aplicación.

Un dominio de aplicación permite el aislamiento de aplicaciones actuando como un contenedor para el estado de la aplicación. Un dominio de aplicación actúa como contenedor y límite para los tipos definidos en la aplicación y las bibliotecas de clases que usa. Los tipos que se cargan en un dominio de aplicación son distintos del mismo tipo cargado en otro dominio de aplicación y las instancias de objetos no se comparten directamente entre los dominios de aplicación. Por ejemplo, cada dominio de aplicación tiene su propia copia de variables estáticas para estos tipos, y un constructor estático para un tipo se ejecuta como máximo una vez por dominio de aplicación. Las implementaciones son gratuitas para proporcionar directivas específicas de implementación o mecanismos para la creación y destrucción de dominios de aplicación.

El inicio de la ***aplicación*** se produce cuando el entorno de ejecución llama a un método designado, al que se hace referencia como punto de entrada de la aplicación. Este método de punto de entrada siempre se denomina `Main` y puede tener una de las firmas siguientes:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Como se muestra, el punto de entrada puede devolver opcionalmente un valor `int`. Este valor devuelto se usa en la finalización de la aplicación ([finalización](basic-concepts.md#application-termination)de la aplicación).

El punto de entrada puede tener opcionalmente un parámetro formal. El parámetro puede tener cualquier nombre, pero el tipo del parámetro debe ser `string[]`. Si el parámetro formal está presente, el entorno de ejecución crea y pasa un argumento `string[]` que contiene los argumentos de la línea de comandos que se especificaron al iniciarse la aplicación. El argumento `string[]` nunca es null, pero puede tener una longitud de cero si no se especificó ningún argumento de línea de comandos.

Puesto C# que admite la sobrecarga de métodos, una clase o struct puede contener varias definiciones de algún método, siempre que cada una tenga una firma diferente. Sin embargo, dentro de un único programa, ninguna clase o struct puede contener más de un método llamado `Main` cuya definición se pueda usar como punto de entrada de la aplicación. No obstante, se permiten otras versiones sobrecargadas de `Main`, siempre y cuando tengan más de un parámetro, o su único parámetro sea distinto del tipo `string[]`.

Una aplicación puede estar formada por varias clases o Structs. Es posible que más de una de estas clases o Structs contengan un método denominado `Main` cuya definición sea apta para su uso como punto de entrada de la aplicación. En tales casos, se debe usar un mecanismo externo (como una opción del compilador de línea de comandos) para seleccionar uno de estos métodos `Main` como punto de entrada.

En C#, cada método se debe definir como miembro de una clase o struct. Normalmente, la accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)) de un método viene determinada por los modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)) especificados en su declaración y, de igual forma, la accesibilidad declarada de un tipo viene determinada por la modificadores de acceso especificados en su declaración. Para que se pueda llamar a un método dado de un tipo determinado, tanto el tipo como el miembro deben ser accesibles. Sin embargo, el punto de entrada de la aplicación es un caso especial. En concreto, el entorno de ejecución puede tener acceso al punto de entrada de la aplicación, independientemente de su accesibilidad declarada y sin tener en consideración la accesibilidad declarada de sus declaraciones de tipos envolventes.

Es posible que el método de punto de entrada de la aplicación no esté en una declaración de clase genérica.

En todos los demás aspectos, los métodos de punto de entrada se comportan como los que no son puntos de entrada.

## <a name="application-termination"></a>Finalización de la aplicación

La ***finalización*** de la aplicación devuelve el control al entorno de ejecución.

Si el tipo de valor devuelto del método de ***punto de entrada*** de la aplicación es `int`, el valor devuelto actúa como el código de ***Estado de terminación***de la aplicación. El propósito de este código es permitir la comunicación de éxito o error en el entorno de ejecución.

Si el tipo de valor devuelto del método de punto de entrada es `void`, al llegar a la llave de cierre (`}`) que finaliza ese método o al ejecutar una instrucción @no__t 2 que no tiene ninguna expresión, se genera un código de estado de finalización de `0`.

Antes de la terminación de una aplicación, se llama a los destructores para todos sus objetos que todavía no se han recolectado como elemento no utilizado, a menos que se haya suprimido dicha limpieza (mediante una llamada al método de biblioteca `GC.SuppressFinalize`, por ejemplo).

## <a name="declarations"></a>Declaraciones

Las declaraciones de un C# programa definen los elementos constituyentes del programa. C#los programas se organizan mediante espacios de nombres ([espacios](namespaces.md)de nombres), que pueden contener declaraciones de tipos y declaraciones de espacio de nombres anidadas. Las declaraciones de tipos ([declaraciones de tipo](namespaces.md#type-declarations)) se utilizan para definir clases ([clases](classes.md)), Structs ([Structs](structs.md)), interfaces ([interfaces](interfaces.md)), enumeraciones ([enumeraciones](enums.md)) y delegados ([delegados](delegates.md)). Los tipos de miembros permitidos en una declaración de tipo dependen del formulario de la declaración de tipos. Por ejemplo, las declaraciones de clase pueden contener declaraciones de constantes ([constantes](classes.md#constants)), campos ([campos](classes.md#fields)), métodos ([métodos](classes.md#methods)), propiedades ([propiedades](classes.md#properties)), eventos ([eventos](classes.md#events)), indizadores ([indizadores](classes.md#indexers)). operadores ([operadores](classes.md#operators)), constructores de instancias ([constructores de instancias](classes.md#instance-constructors)), constructores estáticos ([constructores estáticos](classes.md#static-constructors)), destructores ([destructores](classes.md#destructors)) y tipos anidados ([tipos anidados](classes.md#nested-types)).

Una declaración define un nombre en el ***espacio de declaración*** al que pertenece la declaración. A excepción de los miembros sobrecargados ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)), es un error en tiempo de compilación tener dos o más declaraciones que introducen miembros con el mismo nombre en un espacio de declaración. Nunca es posible que un espacio de declaración contenga distintos tipos de miembros con el mismo nombre. Por ejemplo, un espacio de declaración nunca puede contener un campo y un método con el mismo nombre.

Hay varios tipos diferentes de espacios de declaración, como se describe a continuación.

*  Dentro de todos los archivos de código fuente de un programa, *namespace_member_declaration*s sin *namespace_declaration* envolventes son miembros de un único espacio de declaración combinado denominado ***espacio de declaración global***.
*  Dentro de todos los archivos de código fuente de un programa, *namespace_member_declaration*s dentro de *namespace_declaration*s que tienen el mismo nombre de espacio de nombres completo son miembros de un solo espacio de declaración combinado.
*  Cada declaración de clase, struct o interfaz crea un nuevo espacio de declaración. Los nombres se introducen en este espacio de declaración a través de *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s o *type_parameter*s. A excepción de las declaraciones de constructor de instancia sobrecargadas y las declaraciones de constructor estático, una clase o struct no puede contener una declaración de miembro con el mismo nombre que la clase o la estructura. Una clase, estructura o interfaz permite la declaración de métodos sobrecargados e indexadores. Además, una clase o struct permite la declaración de constructores de instancia sobrecargados y operadores. Por ejemplo, una clase, un struct o una interfaz pueden contener varias declaraciones de método con el mismo nombre, siempre que estas declaraciones de método difieran en su firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)). Tenga en cuenta que las clases base no contribuyen al espacio de declaración de una clase, y las interfaces base no contribuyen al espacio de declaración de una interfaz. Por lo tanto, una clase derivada o una interfaz pueden declarar un miembro con el mismo nombre que un miembro heredado. Este miembro se dice que ***oculta*** el miembro heredado.
*  Cada declaración de delegado crea un nuevo espacio de declaración. Los nombres se introducen en este espacio de declaración a través de los parámetros formales (*fixed_parameter*s y *parameter_array*s) y *type_parameter*s.
*  Cada declaración de enumeración crea un nuevo espacio de declaración. Los nombres se introducen en este espacio de declaración a través de *enum_member_declarations*.
*  Cada declaración de método, declaración de indexador, declaración de operador, declaración de constructor de instancia y función anónima crea un nuevo espacio de declaración denominado ***espacio de declaración de variable local***. Los nombres se introducen en este espacio de declaración a través de los parámetros formales (*fixed_parameter*s y *parameter_array*s) y *type_parameter*s. El cuerpo del miembro de función o de la función anónima, si existe, se considera anidado dentro del espacio de declaración de la variable local. Es un error que un espacio de declaración de variable local y un espacio de declaración de variable local anidada contengan elementos con el mismo nombre. Por lo tanto, dentro de un espacio de declaración anidado no es posible declarar una variable o constante local con el mismo nombre que una variable o constante local en un espacio de declaración de inclusión. Es posible que dos espacios de declaración contengan elementos con el mismo nombre siempre y cuando ningún espacio de declaración contenga el otro.
*  Cada *bloque* o *switch_block* , así como una instrucción *for*, *foreach* y *using* , crea un espacio de declaración de variable local para las variables locales y las constantes locales. Los nombres se introducen en este espacio de declaración a través de *local_variable_declaration*s y *local_constant_declaration*s. Tenga en cuenta que los bloques que se producen como o dentro del cuerpo de un miembro de función o de una función anónima están anidados dentro del espacio de declaración de variable local declarado por esas funciones para sus parámetros. Por lo tanto, es un error tener, por ejemplo, un método con una variable local y un parámetro con el mismo nombre.
*  Cada *bloque* o *switch_block* crea un espacio de declaración independiente para las etiquetas. Los nombres se introducen en este espacio de declaración a través de *labeled_statement*s y se hace referencia a los nombres a través de *goto_statement*s. El ***espacio de declaración de etiqueta*** de un bloque incluye cualquier bloque anidado. Por lo tanto, dentro de un bloque anidado no es posible declarar una etiqueta con el mismo nombre que una etiqueta en un bloque de inclusión.

El orden textual en el que se declaran los nombres no suele ser significativo. En concreto, el orden textual no es significativo para la declaración y el uso de espacios de nombres, constantes, métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores, constructores estáticos y tipos. El orden de declaración es significativo de las siguientes maneras:

*  El orden de declaración de las declaraciones de campo y las declaraciones de variables locales determina el orden en que se ejecutan sus inicializadores (si existen).
*  Las variables locales deben definirse antes de que se usen ([ámbitos](basic-concepts.md#scopes)).
*  El orden de declaración para las declaraciones de miembros de enumeración ([miembros de enumeración](enums.md#enum-members)) es importante cuando se omiten los valores de *constant_expression* .

El espacio de declaración de un espacio de nombres es "Open Terminated" y dos declaraciones de espacios de nombres con el mismo nombre completo contribuyen al mismo espacio de declaración. Por ejemplo
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

Las dos declaraciones de espacio de nombres anteriores contribuyen al mismo espacio de declaración; en este caso, se declaran dos clases con los nombres completos `Megacorp.Data.Customer` y `Megacorp.Data.Order`. Dado que las dos declaraciones contribuyen al mismo espacio de declaración, habría producido un error en tiempo de compilación si cada una de ellas contenía una declaración de una clase con el mismo nombre.

Tal y como se especificó anteriormente, el espacio de declaración de un bloque incluye cualquier bloque anidado. Por lo tanto, en el ejemplo siguiente, los métodos `F` y `G` producen un error en tiempo de compilación porque el nombre `i` se declara en el bloque exterior y no se puede volver a declarar en el bloque interno. Sin embargo, los métodos `H` y `I` son válidos, ya que los dos @no__t 2 se declaran en bloques independientes no anidados.

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a>Miembros

Los espacios de nombres y ***los***tipos tienen miembros. Los miembros de una entidad están disponibles con carácter general a través del uso de un nombre completo que empieza por una referencia a la entidad, seguido de un token "`.`", seguido del nombre del miembro.

Los miembros de un tipo se declaran en la declaración de tipos o se ***heredan*** de la clase base del tipo. Cuando un tipo hereda de una clase base, todos los miembros de la clase base, excepto los constructores de instancia, destructores y constructores estáticos, se convierten en miembros del tipo derivado. La accesibilidad declarada de un miembro de clase base no controla si el miembro se hereda (la herencia se extiende a cualquier miembro que no sea un constructor de instancia, un constructor estático o un destructor). Sin embargo, es posible que no se pueda obtener acceso a un miembro heredado en un tipo derivado, ya sea debido a su accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)) o porque está oculta por una declaración en el propio tipo ([ocultando a través](basic-concepts.md#hiding-through-inheritance)de la herencia).

### <a name="namespace-members"></a>Miembros de espacio de nombres

Los espacios de nombres y los tipos que no tienen ningún espacio de nombres envolvente son miembros del ***espacio de nombres global***. Se corresponde directamente con los nombres declarados en el espacio de declaración global.

Los espacios de nombres y los tipos declarados dentro de un espacio de nombres son miembros de ese espacio de nombres. Esto corresponde directamente a los nombres declarados en el espacio de declaración del espacio de nombres.

Los espacios de nombres no tienen restricciones de acceso. No es posible declarar espacios de nombres privados, protegidos o internos, y los nombres de los espacios de nombres siempre son accesibles públicamente.

### <a name="struct-members"></a>Miembros de struct

Los miembros de un struct son los miembros declarados en la estructura y los miembros heredados de la clase base directa de la estructura `System.ValueType` y la clase base indirecta `object`.

Los miembros de un tipo simple se corresponden directamente con los miembros del tipo de struct con alias del tipo simple:

*  Los miembros de `sbyte` son los miembros del struct `System.SByte`.
*  Los miembros de `byte` son los miembros del struct `System.Byte`.
*  Los miembros de `short` son los miembros del struct `System.Int16`.
*  Los miembros de `ushort` son los miembros del struct `System.UInt16`.
*  Los miembros de `int` son los miembros del struct `System.Int32`.
*  Los miembros de `uint` son los miembros del struct `System.UInt32`.
*  Los miembros de `long` son los miembros del struct `System.Int64`.
*  Los miembros de `ulong` son los miembros del struct `System.UInt64`.
*  Los miembros de `char` son los miembros del struct `System.Char`.
*  Los miembros de `float` son los miembros del struct `System.Single`.
*  Los miembros de `double` son los miembros del struct `System.Double`.
*  Los miembros de `decimal` son los miembros del struct `System.Decimal`.
*  Los miembros de `bool` son los miembros del struct `System.Boolean`.

### <a name="enumeration-members"></a>Miembros de enumeración

Los miembros de una enumeración son las constantes declaradas en la enumeración y los miembros heredados de la clase base directa de la enumeración `System.Enum` y las clases base indirectas `System.ValueType` y `object`.

### <a name="class-members"></a>Miembros de la clase

Los miembros de una clase son los miembros declarados en la clase y los miembros heredados de la clase base (excepto para la clase `object` que no tiene ninguna clase base). Los miembros heredados de la clase base incluyen las constantes, campos, métodos, propiedades, eventos, indizadores, operadores y tipos de la clase base, pero no los constructores de instancias, destructores y constructores estáticos de la clase base. Los miembros de clase base se heredan sin tener en cuenta su accesibilidad.

Una declaración de clase puede contener declaraciones de constantes, campos, métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores, constructores estáticos y tipos.

Los miembros de `object` y `string` se corresponden directamente con los miembros de los tipos de clase a los que se les ha contorno:

*  Los miembros de `object` son los miembros de la clase `System.Object`.
*  Los miembros de `string` son los miembros de la clase `System.String`.

### <a name="interface-members"></a>Miembros de interfaz

Los miembros de una interfaz son los miembros declarados en la interfaz y en todas las interfaces base de la interfaz. Los miembros de la clase `object` no son, estrictamente hablando, miembros de cualquier interfaz (miembros de la[interfaz](interfaces.md#interface-members)). Sin embargo, los miembros de la clase `object` están disponibles a través de la búsqueda de miembros en cualquier tipo de interfaz ([búsqueda de miembros](expressions.md#member-lookup)).

### <a name="array-members"></a>Miembros de matriz

Los miembros de una matriz son los miembros heredados de la clase `System.Array`.

### <a name="delegate-members"></a>Miembros de delegado

Los miembros de un delegado son los miembros heredados de la clase `System.Delegate`.

## <a name="member-access"></a>Acceso a miembros

Las declaraciones de miembros permiten el control sobre el acceso a miembros. La accesibilidad de un miembro se establece mediante la accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)) del miembro combinado con la accesibilidad del tipo contenedor inmediato, si existe.

Cuando se permite el acceso a un miembro determinado, se dice que el miembro es ***accesible***. Por el contrario, cuando no se permite el acceso a un miembro determinado, se dice que el miembro no es ***accesible***. Se permite el acceso a un miembro cuando la ubicación textual en la que tiene lugar el acceso se incluye en el dominio de accesibilidad ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)) del miembro.

### <a name="declared-accessibility"></a>Accesibilidad declarada

La ***accesibilidad declarada*** de un miembro puede ser una de las siguientes:

*  Público, que se selecciona mediante la inclusión de un modificador `public` en la declaración de miembro. El significado intuitivo de `public` es "acceso no limitado".
*  Protected, que se selecciona mediante la inclusión de un modificador `protected` en la declaración de miembro. El significado intuitivo de `protected` es el acceso limitado a la clase contenedora o a los tipos derivados de la clase contenedora.
*  Internal, que se selecciona mediante la inclusión de un modificador `internal` en la declaración de miembro. El significado intuitivo de `internal` es "acceso limitado a este programa".
*  Protected internal (es decir, Protected o Internal), que se selecciona mediante la inclusión de `protected` y un modificador `internal` en la declaración de miembro. El significado intuitivo de `protected internal` es "acceso limitado a este programa o tipos derivados de la clase contenedora".
*  Private, que se selecciona mediante la inclusión de un modificador `private` en la declaración de miembro. El significado intuitivo de `private` es "acceso limitado al tipo contenedor".

Dependiendo del contexto en el que tenga lugar una declaración de miembro, solo se permiten determinados tipos de accesibilidad declarada. Además, cuando una declaración de miembro no incluye modificadores de acceso, el contexto en el que se produce la declaración determina la accesibilidad declarada predeterminada.

*  Los espacios de nombres tienen implícitamente la accesibilidad `public`. No se permiten modificadores de acceso en las declaraciones de espacio de nombres.
*  Los tipos declarados en unidades de compilación o espacios de nombres pueden tener @no__t la accesibilidad declarada como 0 o `internal` y tienen como valor predeterminado la accesibilidad declarada @no__t 2.
*  Los miembros de clase pueden tener cualquiera de los cinco tipos de accesibilidad declarada y su valor predeterminado es `private`, que es una accesibilidad declarada. (Tenga en cuenta que un tipo declarado como miembro de una clase puede tener cualquiera de los cinco tipos de accesibilidad declarada, mientras que un tipo declarado como miembro de un espacio de nombres solo puede tener la accesibilidad declarada `public` o `internal`).
*  Los miembros de struct pueden tener la accesibilidad declarada `public`, `internal` o `private` y de forma predeterminada a `private`, ya que los Structs están sellados implícitamente. Los miembros de estructura introducidos en un struct (es decir, no heredados por ese struct) no pueden tener accesibilidad declarada `protected` o `protected internal`. (Tenga en cuenta que un tipo declarado como miembro de un struct puede tener la accesibilidad declarada `public`, `internal` o `private`, mientras que un tipo declarado como miembro de un espacio de nombres solo puede tener la accesibilidad declarada `public` o `internal`).
*  Los miembros de interfaz tienen implícitamente la accesibilidad `public`. No se permiten modificadores de acceso en las declaraciones de miembros de interfaz.
*  Los miembros de enumeración tienen implícitamente la accesibilidad declarada `public`. No se permiten modificadores de acceso en las declaraciones de miembros de enumeración.

### <a name="accessibility-domains"></a>Dominios de accesibilidad

El ***dominio de accesibilidad*** de un miembro se compone de las secciones (posiblemente disjuntos) del texto del programa en el que se permite el acceso al miembro. A efectos de definir el dominio de accesibilidad de un miembro, se dice que un miembro es de ***nivel superior*** si no se declara dentro de un tipo, y se dice que un miembro está ***anidado*** si se declara dentro de otro tipo. Además, el ***texto*** del programa de un programa se define como todo el texto del programa contenido en todos los archivos de código fuente del programa, y el texto del programa de un tipo se define como todo el texto del programa contenido en el *type_declaration*s de ese tipo (incluido, posiblemente, los tipos que están anidados dentro del tipo).

El dominio de accesibilidad de un tipo predefinido (como `object`, `int` o `double`) es ilimitado.

El dominio de accesibilidad de un tipo sin enlazar de nivel superior `T` ([tipos enlazados y sin enlazar](types.md#bound-and-unbound-types)) que se declara en un programa `P` se define de la manera siguiente:

*  Si la accesibilidad declarada de `T` es `public`, el dominio de accesibilidad de `T` es el texto de programa de `P` y cualquier programa que haga referencia a `P`.
*  Si la accesibilidad declarada de `T` es `internal`, el dominio de accesibilidad de `T` es el texto de programa de `P`.

A partir de estas definiciones, sigue que el dominio de accesibilidad de un tipo sin enlazar de nivel superior siempre es al menos el texto del programa del programa en el que se declara ese tipo.

El dominio de accesibilidad de un tipo construido `T<A1, ..., An>` es la intersección del dominio de accesibilidad del tipo genérico sin enlazar `T` y los dominios de accesibilidad de los argumentos de tipo `A1, ..., An`.

El dominio de accesibilidad de un miembro anidado `M` declarado en un tipo `T` dentro de un programa `P` se define de la manera siguiente (teniendo en cuenta que `M` puede ser un tipo):

*  Si la accesibilidad declarada de `M` es `public`, el dominio de accesibilidad de `M` es el dominio de accesibilidad de `T`.
*  Si la accesibilidad declarada de `M` es `protected internal`, permita que `D` sea la Unión del texto del programa de `P` y el texto del programa de cualquier tipo derivado de `T`, que se declara fuera de `P`. El dominio de accesibilidad de `M` es la intersección del dominio de accesibilidad de `T` con `D`.
*  Si la accesibilidad declarada de `M` es `protected`, permita que `D` sea la Unión del texto del programa de `T` y el texto del programa de cualquier tipo derivado de `T`. El dominio de accesibilidad de `M` es la intersección del dominio de accesibilidad de `T` con `D`.
*  Si la accesibilidad declarada de `M` es `internal`, el dominio de accesibilidad de `M` es la intersección del dominio de accesibilidad de `T` con el texto de programa de `P`.
*  Si la accesibilidad declarada de `M` es `private`, el dominio de accesibilidad de `M` es el texto de programa de `T`.

A partir de estas definiciones, sigue que el dominio de accesibilidad de un miembro anidado siempre es al menos el texto del programa del tipo en el que se declara el miembro. Además, sigue que el dominio de accesibilidad de un miembro nunca es más inclusivo que el dominio de accesibilidad del tipo en el que se declara el miembro.

En términos intuitivos, cuando se tiene acceso a un tipo o miembro `M`, se evalúan los pasos siguientes para asegurarse de que se permite el acceso:

*  En primer lugar, si `M` se declara dentro de un tipo (en lugar de una unidad de compilación o un espacio de nombres), se produce un error en tiempo de compilación si ese tipo no es accesible.
*  A continuación, si `M` es `public`, se permite el acceso.
*  De lo contrario, si `M` es `protected internal`, se permite el acceso si se produce en el programa en el que se declara `M`, o bien, si se produce dentro de una clase derivada de la clase en la que se declara `M` y tiene lugar a través del tipo de clase derivada (protegido).[ acceso para los miembros de instancia](basic-concepts.md#protected-access-for-instance-members)).
*  De lo contrario, si `M` es `protected`, se permite el acceso si se produce dentro de la clase en la que se declara `M`, o bien, si se produce en una clase derivada de la clase en la que se declara `M` y tiene lugar a través del tipo de clase derivada (protegido).[ acceso para los miembros de instancia](basic-concepts.md#protected-access-for-instance-members)).
*  De lo contrario, si `M` es `internal`, se permite el acceso si se produce en el programa en el que se declara `M`.
*  De lo contrario, si `M` es `private`, se permite el acceso si se produce en el tipo en el que se declara `M`.
*  De lo contrario, no se puede obtener acceso al tipo o miembro y se produce un error en tiempo de compilación.

En el ejemplo
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
las clases y los miembros tienen los siguientes dominios de accesibilidad:

*  El dominio de accesibilidad de `A` y `A.X` es ilimitado.
*  El dominio de accesibilidad de `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X` y `B.C.Y` es el texto del programa que lo contiene.
*  El dominio de accesibilidad de `A.Z` es el texto del programa de `A`.
*  El dominio de accesibilidad de `B.Z` y `B.D` es el texto de programa de `B`, incluido el texto del programa de `B.C` y `B.D`.
*  El dominio de accesibilidad de `B.C.Z` es el texto del programa de `B.C`.
*  El dominio de accesibilidad de `B.D.X` y `B.D.Y` es el texto de programa de `B`, incluido el texto del programa de `B.C` y `B.D`.
*  El dominio de accesibilidad de `B.D.Z` es el texto del programa de `B.D`.

Como se muestra en el ejemplo, el dominio de accesibilidad de un miembro nunca es mayor que el de un tipo contenedor. Por ejemplo, aunque todos los miembros `X` tengan una accesibilidad declarada pública, todos salvo `A.X` tienen dominios de accesibilidad que están restringidos por un tipo contenedor.

Como se describe en [miembros](basic-concepts.md#members), todos los miembros de una clase base, excepto los constructores de instancias, destructores y constructores estáticos, los heredan los tipos derivados. Esto incluye incluso miembros privados de una clase base. Sin embargo, el dominio de accesibilidad de un miembro privado incluye solo el texto del programa del tipo en el que se declara el miembro. En el ejemplo
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
la clase `B` hereda el miembro privado `x` de la clase `A`. Dado que el miembro es privado, solo es accesible dentro del *class_body* de `A`. Por lo tanto, el acceso a `b.x` se realiza correctamente en el método `A.F`, pero produce un error en el método `B.F`.

### <a name="protected-access-for-instance-members"></a>Acceso protegido para miembros de instancia

Cuando se tiene acceso a un miembro de instancia `protected` fuera del texto del programa de la clase en la que se declara, y cuando se tiene acceso a un miembro de instancia `protected internal` fuera del texto del programa en el que se declara, el acceso debe realizarse dentro de una clase. declaración que se deriva de la clase en la que se declara. Además, el acceso debe realizarse a través de una instancia de ese tipo de clase derivada o de un tipo de clase construido a partir de él. Esta restricción evita que una clase derivada tenga acceso a miembros protegidos de otras clases derivadas, incluso cuando los miembros se heredan de la misma clase base.

Permita que `B` sea una clase base que declare un miembro de instancia protegido `M` y deje que `D` sea una clase que derive de `B`. Dentro del *class_body* de `D`, el acceso a `M` puede adoptar uno de los siguientes formatos:

*  Un *type_name* o *primary_expression* no calificado con el formato `M`.
*  Un *primary_expression* con el formato `E.M`, siempre que el tipo de `E` sea `T` o una clase derivada de `T`, donde `T` es el tipo de clase `D` o un tipo de clase construido a partir de `D`
*  *Primary_expression* con el formato `base.M`.

Además de estas formas de acceso, una clase derivada puede tener acceso a un constructor de instancia protegido de una clase base en un *constructor_initializer* ([inicializadores de constructor](classes.md#constructor-initializers)).

En el ejemplo
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
dentro de `A`, es posible tener acceso a `x` a través de instancias de `A` y `B`, ya que en cualquier caso el acceso se realiza a través de una instancia de `A` o una clase derivada de `A`. Sin embargo, en `B`, no es posible tener acceso a `x` a través de una instancia de `A`, ya que `A` no se deriva de `B`.

En el ejemplo
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
las tres asignaciones a `x` se permiten porque todas tienen lugar a través de instancias de tipos de clase construidas a partir del tipo genérico.

### <a name="accessibility-constraints"></a>Restricciones de accesibilidad

Varias construcciones del C# lenguaje requieren que un tipo sea ***al menos tan accesible como*** miembro u otro tipo. Se dice que un tipo `T` es al menos tan accesible como miembro o tipo `M` si el dominio de accesibilidad de `T` es un supraconjunto del dominio de accesibilidad de `M`. En otras palabras, `T` es al menos tan accesible como `M` si `T` es accesible en todos los contextos en los que se puede tener acceso a `M`.

Existen las siguientes restricciones de accesibilidad:

*  La clase base directa de un tipo de clase debe ser al menos igual de accesible que el propio tipo de clase.
*  Las interfaces base explícitas de un tipo de interfaz deben ser al menos igual de accesibles que el propio tipo de interfaz.
*  El tipo de valor devuelto y los tipos de parámetros de un tipo de delegado deben ser al menos igual de accesibles que el propio tipo de delegado.
*  El tipo de una constante debe ser al menos igual de accesible que la propia constante.
*  El tipo de un campo debe ser al menos igual de accesible que el propio campo.
*  El tipo de valor devuelto y los tipos de parámetros de un método deben ser al menos igual de accesibles que el propio método.
*  El tipo de una propiedad debe ser al menos igual de accesible que la misma propiedad.
*  El tipo de un evento debe ser al menos igual de accesible que el propio evento.
*  Los tipos de parámetro y el tipo de un indexador deben ser al menos igual de accesibles que el propio indexador.
*  El tipo de valor devuelto y los tipos de parámetro de un operador deben ser al menos igual de accesibles que el propio operador.
*  Los tipos de parámetro de un constructor de instancia deben ser al menos tan accesibles como el propio constructor de instancia.

En el ejemplo
```csharp
class A {...}

public class B: A {...}
```
la clase `B` produce un error en tiempo de compilación porque `A` no es al menos tan accesible como `B`.

Del mismo modo, en el ejemplo
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
el método `H` de `B` produce un error en tiempo de compilación porque el tipo de valor devuelto `A` no es al menos tan accesible como el método.

## <a name="signatures-and-overloading"></a>Firmas y sobrecarga

Los métodos, los constructores de instancia, los indizadores y los operadores se caracterizan por sus ***firmas***:

*  La firma de un método consta del nombre del método, el número de parámetros de tipo y el tipo y la clase (valor, referencia o salida) de cada uno de sus parámetros formales, que se considera en el orden de izquierda a derecha. Para estos propósitos, cualquier parámetro de tipo del método que se produce en el tipo de un parámetro formal se identifica no por su nombre, sino por su posición ordinal en la lista de argumentos de tipo del método. La firma de un método no incluye específicamente el tipo de valor devuelto, el modificador `params` que se puede especificar para el parámetro situado más a la derecha, ni las restricciones de parámetro de tipo opcional.
*  La firma de un constructor de instancia consta del tipo y el tipo (valor, referencia o salida) de cada uno de sus parámetros formales, considerados en el orden de izquierda a derecha. La firma de un constructor de instancia no incluye específicamente el modificador `params` que se puede especificar para el parámetro situado más a la derecha.
*  La firma de un indexador consta del tipo de cada uno de sus parámetros formales, que se considera en el orden de izquierda a derecha. La firma de un indexador no incluye específicamente el tipo de elemento, ni incluye el modificador `params` que se puede especificar para el parámetro situado más a la derecha.
*  La firma de un operador consta del nombre del operador y el tipo de cada uno de sus parámetros formales, considerados en el orden de izquierda a derecha. La firma de un operador no incluye específicamente el tipo de resultado.

Las firmas son el mecanismo de habilitación para la ***sobrecarga*** de miembros en clases, Structs e interfaces:

*  La sobrecarga de métodos permite a una clase, estructura o interfaz declarar varios métodos con el mismo nombre, siempre que sus firmas sean únicas dentro de esa clase, estructura o interfaz.
*  La sobrecarga de constructores de instancia permite a una clase o struct declarar varios constructores de instancia, siempre que sus firmas sean únicas dentro de esa clase o estructura.
*  La sobrecarga de indexadores permite a una clase, estructura o interfaz declarar varios indexadores, siempre que sus firmas sean únicas dentro de esa clase, estructura o interfaz.
*  La sobrecarga de los operadores permite a una clase o struct declarar varios operadores con el mismo nombre, siempre que sus firmas sean únicas dentro de esa clase o estructura.

Aunque los modificadores de parámetro `out` y `ref` se consideran parte de una firma, los miembros declarados en un tipo único no pueden diferir en la firma únicamente en `ref` y `out`. Se produce un error en tiempo de compilación si dos miembros se declaran en el mismo tipo con firmas que serían iguales si se cambiaran todos los parámetros de ambos métodos con modificadores `out` a los modificadores `ref`. Para otros propósitos de coincidencia de la firma (por ejemplo, ocultar o reemplazar), `ref` y `out` se consideran parte de la firma y no coinciden entre sí. (Esta restricción consiste en permitir C# que los programas se traduzcan fácilmente para ejecutarse en el Common Language Infrastructure (CLI), que no proporciona una manera de definir métodos que difieren únicamente en `ref` y `out`).

En el caso de las firmas, los tipos `object` y `dynamic` se consideran iguales. Los miembros declarados en un tipo único pueden, por tanto, no diferir en la firma únicamente en `object` y `dynamic`.

En el ejemplo siguiente se muestra un conjunto de declaraciones de método sobrecargado junto con sus firmas.
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

Tenga en cuenta que los modificadores de parámetro `ref` y `out` ([parámetros de método](classes.md#method-parameters)) forman parte de una firma. Por lo tanto, `F(int)` y `F(ref int)` son firmas únicas. Sin embargo, `F(ref int)` y `F(out int)` no se pueden declarar dentro de la misma interfaz porque las firmas difieren únicamente en `ref` y `out`. Además, tenga en cuenta que el tipo de valor devuelto y el modificador `params` no forman parte de una firma, por lo que no es posible sobrecargar únicamente en función del tipo de valor devuelto o de la inclusión o exclusión del modificador `params`. Como tal, las declaraciones de los métodos `F(int)` y `F(params string[])` identificados anteriormente provocan un error en tiempo de compilación.

## <a name="scopes"></a>Ámbitos

El ***ámbito*** de un nombre es la región del texto del programa en la que es posible hacer referencia a la entidad declarada por el nombre sin la calificación del nombre. Los ámbitos se pueden ***anidar***y un ámbito interno puede volver a declarar el significado de un nombre desde un ámbito externo (sin embargo, no se quita la restricción impuesta por las [declaraciones](basic-concepts.md#declarations) que se encuentran dentro de un bloque anidado no es posible declarar una variable local con el el mismo nombre que una variable local en un bloque de inclusión). A continuación, se dice que el nombre del ámbito externo está ***oculto*** en la región del texto del programa que abarca el ámbito interno y el acceso al nombre externo solo es posible mediante la calificación del nombre.

*  El ámbito de un miembro de espacio de nombres declarado por un *namespace_member_declaration* ([miembros de espacio de nombres](namespaces.md#namespace-members)) sin ningún *namespace_declaration* envolvente es todo el texto del programa.
*  El ámbito de un miembro de espacio de nombres declarado por un *namespace_member_declaration* dentro de un *namespace_declaration* cuyo nombre completo es `N` es el *namespace_body* de cada *namespace_declaration* cuyo el nombre completo es `N` o empieza por `N`, seguido de un punto.
*  El ámbito del nombre definido por un *extern_alias_directive* se extiende a través de *using_directive*s, *global_attributes* y *namespace_member_declaration*s de su unidad de compilación o cuerpo de espacio de nombres que contiene inmediatamente. Un *extern_alias_directive* no aporta ningún miembro nuevo al espacio de declaración subyacente. En otras palabras, una *extern_alias_directive* no es transitiva, sino que solo afecta a la unidad de compilación o al cuerpo del espacio de nombres en el que se produce.
*  El ámbito de un nombre definido o importado por una *using_directive* ([mediante directivas](namespaces.md#using-directives)) se extiende a través de los *namespace_member_declaration*de *compilation_unit* o *namespace_body* en los que *using_directive* se produce. Una *using_directive* puede hacer que cero o más nombres de espacio de nombres, tipo o miembro estén disponibles en un *compilation_unit* o *namespace_body*determinado, pero no contribuye a ningún miembro nuevo en el espacio de declaración subyacente. En otras palabras, un *using_directive* no es transitivo sino que solo afecta al *compilation_unit* o *namespace_body* en el que se produce.
*  El ámbito de un parámetro de tipo declarado por un *type_parameter_list* en un *class_declaration* ([declaraciones de clase](classes.md#class-declarations)) es *class_base*, *type_parameter_constraints_clause*s y *class_body* de ese  *class_declaration*.
*  El ámbito de un parámetro de tipo declarado por *type_parameter_list* en un *struct_declaration* ([declaraciones de struct](structs.md#struct-declarations)) es *struct_interfaces*, *type_parameter_constraints_clause*s y *struct_body* de esa *struct_declaration*.
*  El ámbito de un parámetro de tipo declarado por un *type_parameter_list* en un *interface_declaration* ([declaraciones de interfaz](interfaces.md#interface-declarations)) es *interface_base*, *type_parameter_constraints_clause*s y *interface_body* de ese *interface_declaration*.
*  El ámbito de un parámetro de tipo declarado por un *type_parameter_list* en un *delegate_declaration* ([declaraciones de delegado](delegates.md#delegate-declarations)) es *el tipo*, *formal_parameter_list*y *type_parameter_constraints_clause* s de ese *delegate_declaration*.
*  El ámbito de un miembro declarado por un *class_member_declaration* ([cuerpo de clase](classes.md#class-body)) es el *class_body* en el que se produce la declaración. Además, el ámbito de un miembro de clase se extiende hasta el *class_body* de las clases derivadas que se incluyen en el dominio de accesibilidad ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)) del miembro.
*  El ámbito de un miembro declarado por un *struct_member_declaration* ([miembros de struct](structs.md#struct-members)) es el *struct_body* en el que se produce la declaración.
*  El ámbito de un miembro declarado por un *enum_member_declaration* ([enumerar miembros](enums.md#enum-members)) es el *enum_body* en el que se produce la declaración.
*  El ámbito de un parámetro declarado en una *method_declaration* ([métodos](classes.md#methods)) es el *method_body* de ese *method_declaration*.
*  El ámbito de un parámetro declarado en un *indexer_declaration* ([indexadores](classes.md#indexers)) es el *accessor_declarations* de ese *indexer_declaration*.
*  El ámbito de un parámetro declarado en una *operator_declaration* ([operadores](classes.md#operators)) es el *bloque* de ese *operator_declaration*.
*  El ámbito de un parámetro declarado en un *constructor_declaration* ([constructores de instancia](classes.md#instance-constructors)) es el *constructor_initializer* y el *bloque* de ese *constructor_declaration*.
*  El ámbito de un parámetro declarado en una *lambda_expression* ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) es el *anonymous_function_body* de ese *lambda_expression*
*  El ámbito de un parámetro declarado en una *anonymous_method_expression* ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) es el *bloque* de ese *anonymous_method_expression*.
*  El ámbito de una etiqueta declarada en un *labeled_statement* ([instrucciones con etiqueta](statements.md#labeled-statements)) es el *bloque* en el que se produce la declaración.
*  El ámbito de una variable local declarada en un *local_variable_declaration* ([declaraciones de variables locales](statements.md#local-variable-declarations)) es el bloque en el que se produce la declaración.
*  El ámbito de una variable local declarada en un *switch_block* de una instrucción `switch` ([la instrucción switch](statements.md#the-switch-statement)) es *switch_block*.
*  El ámbito de una variable local declarada en un *for_initializer* de una instrucción `for` ([la instrucción for](statements.md#the-for-statement)) *es for_initializer*, *for_condition*, *for_iterator*y la *instrucción* contenida del instrucción `for`.
*  El ámbito de una constante local declarada en un *local_constant_declaration* ([declaraciones de constantes locales](statements.md#local-constant-declarations)) es el bloque en el que se produce la declaración. Es un error en tiempo de compilación hacer referencia a una constante local en una posición textual que precede a su *constant_declarator*.
*  El ámbito de una variable declarada como parte de *foreach_statement*, *using_statement*, *lock_statement* o *query_expression* viene determinado por la expansión de la construcción especificada.

Dentro del ámbito de un espacio de nombres, una clase, un struct o un miembro de enumeración, es posible hacer referencia al miembro en una posición textual que preceda a la declaración del miembro. Por ejemplo
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
Aquí, es válido para que `F` haga referencia a `i` antes de que se declare.

Dentro del ámbito de una variable local, se trata de un error en tiempo de compilación para hacer referencia a la variable local en una posición textual que precede a la *local_variable_declarator* de la variable local. Por ejemplo
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

En el método `F` anterior, la primera asignación a `i` no hace referencia específicamente al campo declarado en el ámbito externo. En su lugar, hace referencia a la variable local y produce un error en tiempo de compilación porque precede textualmente a la declaración de la variable. En el método `G`, el uso de `j` en el inicializador para la declaración de `j` es válido porque el uso no precede a *local_variable_declarator*. En el método `H`, un *local_variable_declarator* subsiguiente hace referencia correctamente a una variable local declarada en un *local_variable_declarator* anterior dentro del mismo *local_variable_declaration*.

Las reglas de ámbito de las variables locales están diseñadas para garantizar que el significado de un nombre usado en un contexto de expresión siempre es el mismo dentro de un bloque. Si el ámbito de una variable local solo se extiende desde su declaración hasta el final del bloque, en el ejemplo anterior, la primera asignación se asignaría a la variable de instancia y la segunda asignación asignaría a la variable local, lo que posiblemente provocaría errores en tiempo de compilación si las instrucciones del bloque se van a reorganizar posteriormente.

El significado de un nombre dentro de un bloque puede variar en función del contexto en el que se usa el nombre. En el ejemplo
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
el nombre `A` se usa en un contexto de expresión para hacer referencia a la variable local `A` y en un contexto de tipo para hacer referencia a la clase `A`.

### <a name="name-hiding"></a>Ocultación de nombres

El ámbito de una entidad suele abarcar más texto del programa que el espacio de declaración de la entidad. En concreto, el ámbito de una entidad puede incluir declaraciones que introducen nuevos espacios de declaración que contienen entidades con el mismo nombre. Dichas declaraciones hacen que la entidad original se ***oculte***. Por el contrario, se dice que una entidad es ***visible*** cuando no está oculta.

La ocultación de nombres se produce cuando los ámbitos se superponen mediante anidamiento y cuando los ámbitos se superponen a través de la herencia. En las secciones siguientes se describen las características de los dos tipos de ocultación.

#### <a name="hiding-through-nesting"></a>Ocultar mediante anidamiento

La ocultación de nombres mediante el anidamiento puede producirse como resultado de anidar espacios de nombres o tipos dentro de los espacios de nombres, como resultado de anidar tipos dentro de clases o Structs, y como resultado de parámetros y declaraciones de variables locales.

En el ejemplo
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
dentro del método `F`, la variable de instancia `i` está oculta por la variable local `i`, pero dentro del método `G`, `i` todavía hace referencia a la variable de instancia.

Cuando un nombre de un ámbito interno oculta un nombre en un ámbito externo, oculta todas las apariciones sobrecargadas de ese nombre. En el ejemplo
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
la llamada a `F(1)` invoca el `F` declarado en @no__t 2 porque todas las repeticiones externas de `F` están ocultas por la declaración interna. Por la misma razón, la llamada a `F("Hello")` produce un error en tiempo de compilación.

#### <a name="hiding-through-inheritance"></a>Ocultar a través de la herencia

La ocultación de nombres a través de la herencia se produce cuando las clases o Structs declaran nombres que se heredaron de clases base. Este tipo de ocultación de nombres adopta uno de los siguientes formatos:

*  Una constante, un campo, una propiedad, un evento o un tipo introducidos en una clase o struct oculta todos los miembros de clase base con el mismo nombre.
*  Un método introducido en una clase o struct oculta todos los miembros de clase base que no son de método con el mismo nombre y todos los métodos de clase base con la misma firma (nombre de método y número de parámetros, modificadores y tipos).
*  Un indizador introducido en una clase o struct oculta todos los indizadores de clase base con la misma firma (número de parámetros y tipos).

Las reglas que rigen las declaraciones de operador ([operadores](classes.md#operators)) hacen imposible que una clase derivada declare un operador con la misma signatura que un operador en una clase base. Por lo tanto, los operadores nunca se ocultan entre sí.

Al contrario que ocultar un nombre de un ámbito externo, si se oculta un nombre accesible desde un ámbito heredado, se genera una advertencia. En el ejemplo
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
la declaración de `F` en `Derived` provoca la notificación de una advertencia. Ocultar un nombre heredado no es específicamente un error, ya que esto impedirá la evolución independiente de las clases base. Por ejemplo, la situación anterior podría haber surgido debido a que una versión posterior de `Base` presentó un método `F` que no estaba presente en una versión anterior de la clase. Si la situación anterior hubiera sido un error, cualquier cambio realizado en una clase base en una biblioteca de clases con versiones independientes podría provocar que las clases derivadas dejen de ser válidas.

La advertencia causada por ocultar un nombre heredado se puede eliminar mediante el uso del modificador `new`:
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

El modificador `new` indica que el `F` de `Derived` es "New" y que, en realidad, está pensado para ocultar el miembro heredado.

Una declaración de un nuevo miembro oculta un miembro heredado solo dentro del ámbito del nuevo miembro.

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

En el ejemplo anterior, la declaración de `F` en `Derived` oculta el @no__t 2 que se heredó de `Base`, pero como el nuevo `F` en `Derived` tiene acceso privado, su ámbito no se extiende a `MoreDerived`. Por lo tanto, la llamada `F()` en `MoreDerived.G` es válida y llamará a @no__t 2.

## <a name="namespace-and-type-names"></a>Nombres de espacios de nombres y tipos

Varios contextos de un C# programa requieren que se especifique un *namespace_name* o *type_name* .

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

Un *namespace_name* es un *namespace_or_type_name* que hace referencia a un espacio de nombres. Después de la resolución tal y como se describe a continuación, el *namespace_or_type_name* de un *namespace_name* debe hacer referencia a un espacio de nombres o, de lo contrario, se producirá un error en tiempo de compilación. Ningún argumento de tipo ([argumentos de tipo](types.md#type-arguments)) puede estar presente en un *namespace_name* (solo los tipos pueden tener argumentos de tipo).

Un *type_name* es un *namespace_or_type_name* que hace referencia a un tipo. Después de la resolución tal y como se describe a continuación, el *namespace_or_type_name* de un *type_name* debe hacer referencia a un tipo o, de lo contrario, se producirá un error en tiempo de compilación.

Si el *namespace_or_type_name* es un miembro de alias calificado, su significado es como se describe en [calificadores de alias de espacio de nombres](namespaces.md#namespace-alias-qualifiers). De lo contrario, un *namespace_or_type_name* tiene una de cuatro formas:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

donde `I` es un identificador único, `N` es un *namespace_or_type_name* y `<A1, ..., Ak>` es un *type_argument_list*opcional. Si no se especifica *type_argument_list* , considere la posibilidad de `k` para que sea cero.

El significado de un *namespace_or_type_name* se determina de la siguiente manera:

*   Si el *namespace_or_type_name* tiene el formato `I` o el formulario `I<A1, ..., Ak>`:
    * Si `K` es cero y *namespace_or_type_name* aparece dentro de una declaración de método genérico ([métodos](classes.md#methods)) y la Declaración incluye un parámetro de tipo ([parámetros de tipo](classes.md#type-parameters)) con el nombre @ no__t-4, entonces *namespace_or_type_ nombre* hace referencia a ese parámetro de tipo.
    * De lo contrario, si el *namespace_or_type_name* aparece dentro de una declaración de tipos, por cada tipo de instancia @ no__t-1 ([el tipo de instancia](classes.md#the-instance-type)), empezando por el tipo de instancia de esa declaración de tipos y continuando con el tipo de instancia de cada uno Declaración de clase o estructura envolvente (si existe):
        * Si `K` es cero y la declaración de `T` incluye un parámetro de tipo con el nombre @ no__t-2, el *namespace_or_type_name* hace referencia a ese parámetro de tipo.
        * De lo contrario, si el *namespace_or_type_name* aparece dentro del cuerpo de la declaración de tipos, y `T` o cualquiera de sus tipos base contienen un tipo accesible anidado con los parámetros Name @ no__t-2 y `K` @ no__t-4Type, *namespace_or_type _name* hace referencia a ese tipo construido con los argumentos de tipo especificados. Si hay más de un tipo de este tipo, se selecciona el tipo declarado en el tipo más derivado. Tenga en cuenta que los miembros que no son de tipo (constantes, campos, métodos, propiedades, indizadores, operadores, constructores de instancias, destructores y constructores estáticos) y miembros de tipo con un número diferente de parámetros de tipo se omiten al determinar el significado del *namespace_or_type_name*.
    * Si los pasos anteriores no se realizaron correctamente, para cada espacio de nombres @ no__t-0, empezando por el espacio de nombres en el que se produce *namespace_or_type_name* , continuando con cada espacio de nombres envolvente (si existe) y finalizando con el espacio de nombres global, lo siguiente los pasos se evalúan hasta que se encuentra una entidad:
        * Si `K` es cero y `I` es el nombre de un espacio de nombres en @ no__t-2, entonces:
            * Si la ubicación donde se produce el *namespace_or_type_name* se incluye en una declaración de espacio de nombres para `N` y la declaración del espacio de nombres contiene un *extern_alias_directive* o un *using_alias_directive* que asocia el nombre @ no __t-4 con un espacio de nombres o un tipo, el *namespace_or_type_name* es ambiguo y se produce un error en tiempo de compilación.
            * De lo contrario, *namespace_or_type_name* hace referencia al espacio de nombres denominado `I` en `N`.
        * De lo contrario, si `N` contiene un tipo accesible con los parámetros Name @ no__t-1 y `K` @ no__t-3type, entonces:
            * Si `K` es cero y la ubicación donde se produce el *namespace_or_type_name* se incluye en una declaración de espacio de nombres para `N` y la declaración del espacio de nombres contiene un *extern_alias_directive* o *using_alias_directive* que asocia el nombre @ no__t-5 con un espacio de nombres o tipo, el *namespace_or_type_name* es ambiguo y se produce un error en tiempo de compilación.
            * De lo contrario, *namespace_or_type_name* hace referencia al tipo construido con los argumentos de tipo especificados.
        * De lo contrario, si la ubicación donde se produce el *namespace_or_type_name* se incluye en una declaración de espacio de nombres para `N`:
            * Si `K` es cero y la declaración del espacio de nombres contiene un *extern_alias_directive* o un *using_alias_directive* que asocia el nombre @ no__t-3 con un espacio de nombres o tipo importado, el *namespace_or_type_name* hace referencia a ese espacio de nombres o tipo.
            * De lo contrario, si los espacios de nombres y las declaraciones de tipos importados por *using_namespace_directive*s y *using_alias_directive*s de la declaración del espacio de nombres contienen exactamente un tipo accesible con el nombre @ no__t-2 y `K` @ no__t-4Type los parámetros, después, el *namespace_or_type_name* hace referencia a ese tipo construido con los argumentos de tipo especificados.
            * De lo contrario, si los espacios de nombres y las declaraciones de tipos importados por *using_namespace_directive*s y *using_alias_directive*s de la declaración del espacio de nombres contienen más de un tipo accesible con el nombre @ no__t-2 y `K` @ no__t-4Type los parámetros, *namespace_or_type_name* , son ambiguos y se produce un error.
    * De lo contrario, *namespace_or_type_name* no está definido y se produce un error en tiempo de compilación.
*  De lo contrario, el *namespace_or_type_name* tiene el formato `N.I` o el formato `N.I<A1, ..., Ak>`. `N` se resuelve primero como *namespace_or_type_name*. Si la resolución de `N` no se realiza correctamente, se produce un error en tiempo de compilación. De lo contrario, `N.I` o `N.I<A1, ..., Ak>` se resuelve como sigue:
    * Si `K` es cero y `N` hace referencia a un espacio de nombres y `N` contiene un espacio de nombres anidado con el nombre `I`, el *namespace_or_type_name* hace referencia a ese espacio de nombres anidado.
    * De lo contrario, si `N` hace referencia a un espacio de nombres y `N` contiene un tipo accesible con los parámetros Name @ no__t-2 y `K` @ no__t-4Type, el *namespace_or_type_name* hace referencia a ese tipo construido con los argumentos de tipo especificados.
    * De lo contrario, si `N` hace referencia a un tipo de clase o struct (posiblemente construido) y `N` o cualquiera de sus clases base contienen un tipo accesible anidado con los parámetros Name @ no__t-2 y `K` @ no__t-4Type, el *namespace_or_type_name* hace referencia a ese tipo construido con los argumentos de tipo especificados. Si hay más de un tipo de este tipo, se selecciona el tipo declarado en el tipo más derivado. Tenga en cuenta que si el significado de `N.I` se está determinando como parte de la resolución de la especificación de clase base de `N`, la clase base directa de `N` se considera objeto ([clases base](classes.md#base-classes)).
    * De lo contrario, `N.I` es un *namespace_or_type_name*no válido y se produce un error en tiempo de compilación.

Una *namespace_or_type_name* puede hacer referencia a una clase estática ([clases estáticas](classes.md#static-classes)) solo si

*  *Namespace_or_type_name* es el `T` en un *namespace_or_type_name* con el formato `T.I`, o bien
*  *Namespace_or_type_name* es el `T` en *typeof_expression* (listas de[argumentos](expressions.md#argument-lists)1) con el formato `typeof(T)`.

### <a name="fully-qualified-names"></a>Nombres completos

Cada espacio de nombres y tipo tiene un ***nombre completo***, que identifica de forma única el espacio de nombres o el tipo entre todos los demás. El nombre completo de un espacio de nombres o tipo `N` se determina de la siguiente manera:

*  Si `N` es un miembro del espacio de nombres global, su nombre completo es `N`.
*  De lo contrario, su nombre completo es `S.N`, donde `S` es el nombre completo del espacio de nombres o tipo en el que se declara `N`.

En otras palabras, el nombre completo de `N` es la ruta de acceso jerárquica completa de los identificadores que conducen a `N`, comenzando por el espacio de nombres global. Dado que cada miembro de un espacio de nombres o tipo debe tener un nombre único, sigue que el nombre completo de un espacio de nombres o tipo es siempre único.

En el ejemplo siguiente se muestran varias declaraciones de espacio de nombres y tipo junto con sus nombres completos asociados.
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a>Administración de memoria automática

C#emplea la administración de memoria automática, que libera a los desarrolladores de la asignación y liberación manuales de la memoria ocupada por los objetos. Las directivas de administración de memoria automática se implementan mediante un ***recolector de elementos no utilizados***. El ciclo de vida de la administración de memoria de un objeto es el siguiente:

1. Cuando se crea el objeto, se le asigna memoria, se ejecuta el constructor y el objeto se considera activo.
2. Si no se puede tener acceso al objeto, o a cualquier parte del mismo, por cualquier continuación posible de ejecución, que no sea la ejecución de destructores, el objeto se considera que ya no está en uso y se puede elegir para su destrucción. El C# compilador y el recolector de elementos no utilizados pueden optar por analizar el código para determinar qué referencias a un objeto se pueden usar en el futuro. Por ejemplo, si una variable local que se encuentra en el ámbito es la única referencia existente a un objeto, pero nunca se hace referencia a esa variable local en ninguna continuación posible de ejecución del punto de ejecución actual del procedimiento, el recolector de elementos no utilizados puede (pero no es obligatorio para) tratar el objeto como ya no está en uso.
3. Una vez que el objeto es válido para la destrucción, en algún momento posterior no especificado, se ejecuta el destructor[(destructores) (](classes.md#destructors)si existe) para el objeto. En circunstancias normales, el destructor del objeto solo se ejecuta una vez, aunque las API específicas de la implementación pueden permitir que se invalide este comportamiento.
4. Una vez que se ejecuta el destructor de un objeto, si no se puede tener acceso a ese objeto o a cualquier parte del mismo, cualquier continuación posible de ejecución, incluida la ejecución de destructores, el objeto se considera inaccesible y el objeto pasa a ser válido para la colección.
5. Por último, en algún momento después de que el objeto sea válido para la recolección, el recolector de elementos no utilizados libera la memoria asociada a ese objeto.

El recolector de elementos no utilizados mantiene información sobre el uso de los objetos y usa esta información para tomar decisiones de administración de memoria, como la ubicación de la memoria para buscar un objeto recién creado, Cuándo se debe reubicar un objeto y cuándo un objeto ya no está en uso o es inaccesible.

Al igual que otros lenguajes que suponen la existencia C# de un recolector de elementos no utilizados, está diseñado de modo que el recolector de elementos no utilizados pueda implementar una amplia gama de directivas de administración de memoria. Por ejemplo, C# no requiere que se ejecuten destructores o que los objetos se recopilen tan pronto como sean válidos, o que los destructores se ejecuten en un orden determinado o en un subproceso determinado.

El comportamiento del recolector de elementos no utilizados puede controlarse, hasta cierto punto, a través de métodos estáticos en la clase `System.GC`. Esta clase se puede usar para solicitar que se produzca una colección, destructores que se van a ejecutar (o no ejecutar), etc.

Dado que el recolector de elementos no utilizados tiene una latitud ancha a la hora de decidir cuándo recopilar objetos y ejecutar destructores, una implementación compatible puede generar resultados que sean diferentes de los que se muestran en el código siguiente. El programa
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
crea una instancia de la clase `A` y una instancia de la clase `B`. Estos objetos son válidos para la recolección de elementos no utilizados cuando la variable `b` tiene asignado el valor `null`, ya que, después de este tiempo, no es posible que ningún código escrito por el usuario tenga acceso a ellos. El resultado puede ser

```console
Destruct instance of A
Destruct instance of B
```
o
```console
Destruct instance of B
Destruct instance of A
```
Dado que el lenguaje no impone ninguna restricción en el orden en el que los objetos se recolectan como elementos no utilizados.

En casos sutiles, la distinción entre "candidato a la destrucción" y "puede ser válida para la recopilación" puede ser importante. Por ejemplo,
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

En el programa anterior, si el recolector de elementos no utilizados elige ejecutar el destructor de `A` antes del destructor de `B`, la salida de este programa podría ser:
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Tenga en cuenta que aunque la instancia de `A` no se estaba usando y se ejecutó el destructor de `A`, todavía es posible llamar a los métodos de `A` (en este caso, `F`) desde otro destructor. Además, tenga en cuenta que la ejecución de un destructor puede hacer que un objeto se pueda utilizar de nuevo desde el programa principal. En este caso, la ejecución del destructor `B` produjo una instancia de `A` que antes no estaba en uso para ser accesible desde la referencia activa `Test.RefA`. Después de la llamada a `WaitForPendingFinalizers`, la instancia de `B` es válida para la recolección, pero la instancia de `A` no es, debido a la referencia `Test.RefA`.

Para evitar la confusión y el comportamiento inesperado, suele ser una buena idea que los destructores solo realicen la limpieza en los datos almacenados en sus propios campos del objeto y no realicen ninguna acción en los objetos a los que se hace referencia ni en los campos estáticos.

Una alternativa al uso de destructores es dejar que una clase implemente la interfaz `System.IDisposable`. Esto permite al cliente del objeto determinar cuándo liberar los recursos del objeto, normalmente mediante el acceso al objeto como un recurso en una instrucción `using` ([la instrucción using](statements.md#the-using-statement)).

## <a name="execution-order"></a>Orden de ejecución

La ejecución de C# un programa continúa de tal forma que los efectos secundarios de cada subproceso en ejecución se conserven en puntos de ejecución críticos. Un ***efecto secundario*** se define como una lectura o escritura de un campo volátil, una escritura en una variable no volátil, una escritura en un recurso externo y el inicio de una excepción. Los puntos de ejecución críticos en los que se debe conservar el orden de estos efectos secundarios son referencias a campos volátiles ([campos volátiles](classes.md#volatile-fields)), instrucciones `lock` ([instrucción lock](statements.md#the-lock-statement)) y creación y terminación de subprocesos. El entorno de ejecución es gratuito para cambiar el orden de ejecución de C# un programa, sujeto a las siguientes restricciones:

*  La dependencia de los datos se conserva dentro de un subproceso de ejecución. Es decir, el valor de cada variable se calcula como si todas las instrucciones del subproceso se ejecutaran en el orden del programa original.
*  Se conservan las reglas de ordenación de inicialización ([inicialización de campos](classes.md#field-initialization) e [inicializadores de variables](classes.md#variable-initializers)).
*  El orden de los efectos secundarios se conserva con respecto a las lecturas y escrituras volátiles ([campos volátiles](classes.md#volatile-fields)). Además, el entorno de ejecución no necesita evaluar parte de una expresión si puede deducir que el valor de esa expresión no se usa y que no se producen efectos secundarios necesarios (incluidos los causados por una llamada a un método o el acceso a un campo volátil). Cuando se interrumpe la ejecución del programa mediante un evento asincrónico (como una excepción iniciada por otro subproceso), no se garantiza que los efectos secundarios observables estén visibles en el orden del programa original.
