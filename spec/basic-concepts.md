---
ms.openlocfilehash: 1c3d05674f8f7b69e70e0d9e06021537fc45f7ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488488"
---
# <a name="basic-concepts"></a>Conceptos básicos

## <a name="application-startup"></a>Inicio de la aplicación

Un ensamblado que tiene un ***punto de entrada*** se denomina un ***aplicación***. Cuando una aplicación es ejecutar una nueva ***dominio de aplicación*** se crea. Pueden existir varias creaciones de instancias diferentes de una aplicación en el mismo equipo al mismo tiempo, y cada uno tiene su propio dominio de aplicación.

Un dominio de aplicación habilita el aislamiento de aplicaciones de forma que actúa como contenedor de estado de la aplicación. Un dominio de aplicación actúa como un contenedor y el límite de los tipos definidos en la aplicación y las bibliotecas de clases que utiliza. Los tipos cargados en un dominio de aplicación son distintos de los mismos tipos cargados en otro dominio de aplicación y las instancias de objetos no se comparten directamente entre dominios de aplicación. Por ejemplo, cada dominio de aplicación tiene su propia copia de variables estáticas para estos tipos, y un constructor estático para un tipo se ejecuta como máximo una vez por dominio de aplicación. Las implementaciones son gratis proporcionar mecanismos o directivas específicas de la implementación para la creación y destrucción de dominios de aplicación.

***Inicio de la aplicación*** se produce cuando el entorno de ejecución llama a un método designado, que se conoce como punto de entrada de la aplicación. Este método de punto de entrada siempre se denomina `Main`y puede tener uno de las firmas siguientes:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Como se muestra, el punto de entrada, opcionalmente, puede devolver un `int` valor. Esto devuelve el valor se utiliza en la finalización de la aplicación ([finalización de la aplicación](basic-concepts.md#application-termination)).

El punto de entrada podría tener opcionalmente un parámetro formal. El parámetro puede tener cualquier nombre, pero el tipo del parámetro debe ser `string[]`. Si el parámetro formal está presente, el entorno de ejecución crea y pasa un `string[]` argumento que contiene los argumentos de línea de comandos que especificó cuando se inició la aplicación. El `string[]` argumento nunca es null, pero puede tener una longitud de cero si no se especificó ningún argumento de línea de comandos.

Puesto que C# admite la sobrecarga de métodos, una clase o estructura puede contener varias definiciones de algún método, siempre que cada una tiene una firma diferente. Sin embargo, dentro de un único programa, ninguna clase o estructura la puede contener más de un método llamado `Main` cuya definición es apta para usarse como un punto de entrada de la aplicación. Otras versiones sobrecargadas de `Main` se permiten, sin embargo, siempre que tengan más de un parámetro o su único parámetro no sea de tipo `string[]`.

Una aplicación puede estar formada por varias clases o structs. Es posible que más de una de estas clases o structs que contiene un método llamado `Main` cuya definición es apta para usarse como un punto de entrada de la aplicación. En tales casos, debe usarse un mecanismo externo (por ejemplo, una opción del compilador de línea de comandos) para seleccionar uno de estos `Main` métodos como punto de entrada.

En C#, todos los métodos deben definirse como un miembro de una clase o struct. Normalmente, la accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)) de un método viene determinada por los modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)) especificado en su declaración y del mismo modo declarado accesibilidad de un tipo se determina mediante los modificadores de acceso especificados en su declaración. En el orden de un método determinado de un tipo determinado para poder llamar a, el tipo y el miembro deben ser accesible. Sin embargo, el punto de entrada de la aplicación es un caso especial. En concreto, el entorno de ejecución puede tener acceso a punto de entrada de la aplicación, independientemente de su accesibilidad declarada y sin tener en cuenta la accesibilidad declarada de sus declaraciones de tipo envolvente.

El método de punto de entrada de aplicación no puede ser en una declaración de clase genérica.

En todos los demás aspectos, los métodos de punto de entrada se comportan como los que no son puntos de entrada.

## <a name="application-termination"></a>Finalización de la aplicación

***Finalización de la aplicación*** devuelve el control al entorno de ejecución.

Si el tipo de valor devuelto de la aplicación ***punto de entrada*** método es `int`, el valor devuelto actúa como la aplicación ***código de estado de terminación***. El propósito de este código es permitir la comunicación de éxito o error para el entorno de ejecución.

Si el tipo de valor devuelto del método de punto de entrada es `void`, llegar a la llave de cierre (`}`) que termina ese método o ejecutar un `return` instrucción que no contiene una expresión, da como resultado un código de estado de terminación `0`.

Antes de la finalización de la aplicación, se llaman a destructores para todos sus objetos que aún no se han recolectado, a menos que se ha suprimido esta limpieza (mediante una llamada al método de biblioteca `GC.SuppressFinalize`, por ejemplo).

## <a name="declarations"></a>Declaraciones

Las declaraciones en un programa de C# definen los elementos constituyentes del programa. Programas de C# se organizan mediante espacios de nombres ([espacios de nombres](namespaces.md)), que puede contener el tipo de declaraciones y las declaraciones de espacio de nombres anidado. Declaraciones de tipos ([declaraciones de tipo](namespaces.md#type-declarations)) se usan para definir clases ([clases](classes.md)), estructuras ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md) ), enumeraciones ([enumeraciones](enums.md)) y los delegados ([delegados](delegates.md)). Los tipos de miembros permitidos en una declaración de tipos dependen de la forma de la declaración de tipos. Por ejemplo, las declaraciones de clase pueden contener declaraciones de constantes ([constantes](classes.md#constants)), campos ([campos](classes.md#fields)), los métodos ([métodos](classes.md#methods)), propiedades ([ Propiedades](classes.md#properties)), los eventos ([eventos](classes.md#events)), los indizadores ([indizadores](classes.md#indexers)), operadores ([operadores](classes.md#operators)), constructores de instancias ([ Constructores de instancias](classes.md#instance-constructors)), los constructores estáticos ([constructores estáticos](classes.md#static-constructors)), los destructores ([destructores](classes.md#destructors)) y los tipos anidados ([tiposanidados](classes.md#nested-types)).

Una declaración define un nombre en el ***espacio de declaración*** al que pertenece la declaración. Excepto para sobrecargar los miembros ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)), es un error de tiempo de compilación tiene dos o más declaraciones que introducen miembros con el mismo nombre en un espacio de declaración. Nunca es posible que un espacio de declaración contener distintos tipos de miembros con el mismo nombre. Por ejemplo, un espacio de declaración nunca puede contener un campo y un método con el mismo nombre.

Hay varios tipos diferentes de espacios de declaración, como se describe en la siguiente.

*  En todos los archivos de código fuente de un programa, *namespace_member_declaration*s con ningún envolvente *namespace_declaration* son miembros de un espacio de declaración combinado único denominado el ***global espacio de declaración***.
*  En todos los archivos de código fuente de un programa, *namespace_member_declaration*s dentro de *namespace_declaration*que tienen el mismo nombre completo del espacio de nombres son miembros de una sola declaración combinada espacio.
*  Cada clase, struct o declaración de interfaz crea un nuevo espacio de declaración. Los nombres se incluyen en este espacio de declaración mediante *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, o *tipo_parámetro*s. Excepto para el constructor de instancia sobrecargada declaraciones y un constructor estático declaraciones, una clase o struct no pueden contener una declaración de miembro con el mismo nombre que la clase o estructura. Una clase, estructura o interfaz permite la declaración de métodos sobrecargados e indizadores. Además, una clase o estructura permite la declaración de operadores y constructores de instancias sobrecargadas. Por ejemplo, una clase, struct o interfaz puede contener varias declaraciones de método con el mismo nombre, siempre que estas declaraciones de método difieren en su firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)). Tenga en cuenta que las clases base no contribuyen al espacio de declaración de una clase y las interfaces base no contribuyen al espacio de declaración de una interfaz. Por lo tanto, una clase derivada o una interfaz puede declarar a un miembro con el mismo nombre que un miembro heredado. Se dice que un miembro de este tipo ***ocultar*** el miembro heredado.
*  Cada declaración de delegado crea un nuevo espacio de declaración. Los nombres se incluyen en este espacio de declaración a través de parámetros formales (*fixed_parameter*s y *parameter_array*s) y *tipo_parámetro*s.
*  Cada declaración de enumeración crea un nuevo espacio de declaración. Los nombres se incluyen en este espacio de declaración mediante *enum_member_declarations*.
*  Cada declaración de método, declaración de indexador, declaración del operador, la declaración de constructor de instancia y función anónima crean un nuevo espacio de declaración que se llama a un ***espacio de declaración de variable local***. Los nombres se incluyen en este espacio de declaración a través de parámetros formales (*fixed_parameter*s y *parameter_array*s) y *tipo_parámetro*s. El cuerpo del miembro de función o función anónima, si existe, se considera que se puede anidar dentro del espacio de declaración de variable local. Es un error para un espacio de declaración de variable local y un espacio de declaración de variable local anidado contengan elementos con el mismo nombre. Por lo tanto, dentro de un espacio de declaración anidado no es posible declarar a una variable local variable o constante con el mismo nombre que una variable local o una constante en un espacio de declaración envolvente. Es posible que dos espacios de declaración contengan elementos con el mismo nombre siempre y cuando ningún espacio de declaración contenga el otro.
*  Cada *bloque* o *switch_block* , así como un *para*, *foreach* y *mediante* instrucción, crea un espacio de declaración de variable local para las variables locales y constantes locales. Los nombres se incluyen en este espacio de declaración mediante *local_variable_declaration*s y *local_constant_declaration*s. Tenga en cuenta que los bloques que se producen como o dentro del cuerpo de un miembro de función o función anónima están anidados dentro del espacio de declaración de variable local declarado por esas funciones para sus parámetros. Por lo tanto es un error, por ejemplo, tener un método con un parámetro del mismo nombre y una variable local.
*  Cada *bloque* o *switch_block* crea un espacio de declaración independiente para las etiquetas. Los nombres se incluyen en este espacio de declaración mediante *labeled_statement*s y los nombres se identifican mediante *goto_statement*s. El ***etiqueta espacio de declaración*** de un bloque incluye bloques anidados. Por lo tanto, dentro de un bloque anidado no es posible declarar una etiqueta con el mismo nombre que una etiqueta en un bloque de inclusión.

El orden textual en que se declaran los nombres suele ser ninguna importancia. En concreto, no es significativo para la declaración y el uso de espacios de nombres, constantes, métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores, constructores estáticos y tipos de orden textual. El orden de declaración es significativo de las maneras siguientes:

*  Orden de declaración de las declaraciones de campo y declaraciones de variable local determina el orden en que se ejecutan sus inicializadores (si existe).
*  Se deben definir las variables locales antes de utilizarlas ([ámbitos](basic-concepts.md#scopes)).
*  Orden de declaración de las declaraciones de miembro de enumeración ([miembros de enumeración](enums.md#enum-members)) es significativo cuando *constant_expression* los valores se omiten.

El espacio de declaración del espacio de nombres es "de extremo abierto" y espacio de nombres dos declaraciones con el mismo nombre completo contribuyen al mismo espacio de declaración. Por ejemplo
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

Las dos declaraciones de espacio de nombres anteriores contribuyen al mismo espacio de declaración, en este caso declarar dos clases con los nombres completos `Megacorp.Data.Customer` y `Megacorp.Data.Order`. Dado que las dos declaraciones contribuyen al mismo espacio de declaración, habría causado un error en tiempo de compilación si cada una contiene una declaración de una clase con el mismo nombre.

Según lo especificado anteriormente, el espacio de declaración de un bloque incluye bloques anidados. Por lo tanto, en el ejemplo siguiente, la `F` y `G` métodos producen un error en tiempo de compilación porque el nombre `i` se declara en el bloque externo y no se puede volver a declarar en el bloque interno. Sin embargo, el `H` y `I` métodos son válidos porque los dos `i`del se declaran en bloques independientes no anidados.

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

Los espacios de nombres y tipos tienen ***miembros***. Los miembros de una entidad están generalmente disponibles mediante el uso de un nombre completo que se inicia con una referencia a la entidad, seguido por un "`.`" símbolo (token), seguido del nombre del miembro.

Los miembros de un tipo se declaren en la declaración de tipos o ***heredado*** de la clase base del tipo. Cuando un tipo hereda de una clase base, todos los miembros de la clase base, excepto los constructores de instancias, destructores y constructores estáticos, se convierten en miembros del tipo derivado. La accesibilidad declarada de un miembro de clase base no controla si el miembro se hereda, herencia se amplía a cualquier miembro que no es un constructor de instancia, un constructor estático o un destructor. Sin embargo, un miembro heredado puede no estar accesible en un tipo derivado, ya sea a causa de su accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)) o porque está oculto por una declaración en el propio tipo ([ocultar mediante herencia](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Miembros de Namespace

Los espacios de nombres y tipos que no tienen ningún espacio de nombres envolvente son miembros de la ***espacio de nombres global***. Esto corresponde directamente a los nombres declarados en el espacio de declaración global.

Espacios de nombres y tipos declarados dentro de un espacio de nombres son miembros de ese espacio de nombres. Esto corresponde directamente a los nombres declarados en el espacio de declaración del espacio de nombres.

Los espacios de nombres no tienen restricciones de acceso. No es posible declarar espacios de nombres privado, protegido o interno y espacios de nombres siempre son accesibles públicamente.

### <a name="struct-members"></a>Miembros de estructura

Los miembros de un struct son los miembros declarados en la estructura y los miembros heredados de la clase base directa del struct `System.ValueType` y la clase base indirecta `object`.

Los miembros de un tipo simple se corresponden directamente a los miembros de lo alias de tipo struct mediante el tipo simple:

*  Los miembros de `sbyte` son los miembros de la `System.SByte` struct.
*  Los miembros de `byte` son los miembros de la `System.Byte` struct.
*  Los miembros de `short` son los miembros de la `System.Int16` struct.
*  Los miembros de `ushort` son los miembros de la `System.UInt16` struct.
*  Los miembros de `int` son los miembros de la `System.Int32` struct.
*  Los miembros de `uint` son los miembros de la `System.UInt32` struct.
*  Los miembros de `long` son los miembros de la `System.Int64` struct.
*  Los miembros de `ulong` son los miembros de la `System.UInt64` struct.
*  Los miembros de `char` son los miembros de la `System.Char` struct.
*  Los miembros de `float` son los miembros de la `System.Single` struct.
*  Los miembros de `double` son los miembros de la `System.Double` struct.
*  Los miembros de `decimal` son los miembros de la `System.Decimal` struct.
*  Los miembros de `bool` son los miembros de la `System.Boolean` struct.

### <a name="enumeration-members"></a>Miembros de enumeración

Los miembros de una enumeración son las constantes que se declaran en la enumeración y los miembros heredados de la clase base directa de la enumeración `System.Enum` y las clases base indirectas `System.ValueType` y `object`.

### <a name="class-members"></a>Miembros de la clase

Los miembros de una clase son los miembros declarados en la clase y los miembros heredados de la clase base (excepto para la clase `object` que no tiene clase base). Los miembros heredados de la clase base incluyen las constantes, campos, métodos, propiedades, eventos, indizadores, operadores y tipos de la clase base, pero no los constructores de instancias, destructores y constructores estáticos de la clase base. Los miembros de clase base se heredan con independencia de su accesibilidad.

Una declaración de clase puede contener declaraciones de constantes, campos, métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores, constructores estáticos y tipos.

Los miembros de `object` y `string` corresponden directamente a los miembros de los tipos de clase alias:

*  Los miembros de `object` son los miembros de la `System.Object` clase.
*  Los miembros de `string` son los miembros de la `System.String` clase.

### <a name="interface-members"></a>Miembros de interfaz

Los miembros de una interfaz son los miembros declarados en la interfaz y en todas las interfaces base de la interfaz. Los miembros de clase `object` no son estrictamente hablando, los miembros de cualquier interfaz ([miembros de interfaz](interfaces.md#interface-members)). Sin embargo, los miembros de clase `object` están disponibles a través de la búsqueda de miembros en cualquier tipo de interfaz ([búsqueda de miembros](expressions.md#member-lookup)).

### <a name="array-members"></a>Miembros de la matriz

Los miembros de la matriz son los miembros heredados de la clase `System.Array`.

### <a name="delegate-members"></a>Miembros de delegado

Los miembros de un delegado son los miembros heredados de la clase `System.Delegate`.

## <a name="member-access"></a>Acceso a miembros

Las declaraciones de miembros permiten un control sobre el acceso de miembro. La accesibilidad de un miembro se establece mediante la accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)) del miembro combinada con la accesibilidad del tipo contenedor inmediato, si existe.

Cuando se permite el acceso a un miembro concreto, se dice que el miembro sea ***accesible***. Por el contrario, cuando no se permite el acceso a un miembro determinado, el miembro se dice que ***inaccesible***. Se permite el acceso a un miembro cuando se incluye la ubicación del texto en el que realiza el acceso en el dominio de accesibilidad ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)) del miembro.

### <a name="declared-accessibility"></a>Accesibilidad declarada

El ***accesibilidad declarada*** de un miembro puede ser uno de los siguientes:

*  Público, que se selecciona mediante la inclusión de un `public` modificador en la declaración del miembro. El significado intuitivo de `public` es "acceso no restringido".
*  Protegido, que se selecciona mediante la inclusión de un `protected` modificador en la declaración del miembro. El significado intuitivo de `protected` es "acceso limitado a la clase contenedora o a tipos derivado de la clase contenedora".
*  Interna, que se selecciona mediante la inclusión de un `internal` modificador en la declaración del miembro. El significado intuitivo de `internal` es "acceso limitado a este programa".
*  Protected internal (es decir, protegidos o internos), que está seleccionada si se incluyen un `protected` y un `internal` modificador en la declaración del miembro. El significado intuitivo de `protected internal` es "acceso limitado a este programa o tipos derivados de la clase contenedora".
*  Private, que se selecciona mediante la inclusión de un `private` modificador en la declaración del miembro. El significado intuitivo de `private` es "acceso limitado al tipo contenedor".

Dependiendo del contexto en el que tiene una declaración de miembro produce, se permiten solo determinados tipos de accesibilidad declarada. Además, cuando una declaración de miembro no incluye ningún modificador de acceso, el contexto en el que tiene lugar la declaración determina la accesibilidad declarada predeterminada.

*  Los espacios de nombres tienen implícitamente `public` accesibilidad declarada. Modificadores de acceso no se permiten en declaraciones de espacio de nombres.
*  Pueden tener tipos declarados en unidades de compilación o espacios de nombres `public` o `internal` declarado el valor predeterminado y accesibilidad `internal` accesibilidad declarada.
*  Los miembros de clase pueden tener cualquiera de los cinco tipos de accesibilidad declarada y de forma predeterminada `private` accesibilidad declarada. (Tenga en cuenta que un tipo declarado como un miembro de una clase puede tener cualquiera de los cinco tipos de accesibilidad declarada, mientras que un tipo declarado como un miembro de un espacio de nombres puede tener solo `public` o `internal` accesibilidad declarada.)
*  Pueden tener miembros de struct `public`, `internal`, o `private` declarado el valor predeterminado y accesibilidad `private` accesibilidad declarada porque structs están sellados implícitamente. No pueden tener miembros de estructura introducidos en un struct (que es, no heredan esa estructura) `protected` o `protected internal` accesibilidad declarada. (Tenga en cuenta que un tipo declarado como un miembro de un struct puede tener `public`, `internal`, o `private` declara la accesibilidad, mientras que un tipo declarado como un miembro de un espacio de nombres puede tener solo `public` o `internal` accesibilidad declarada.)
*  Los miembros de interfaz tienen implícitamente `public` accesibilidad declarada. Modificadores de acceso no se permiten en las declaraciones de miembros de interfaz.
*  Los miembros de enumeración tienen implícitamente `public` accesibilidad declarada. Modificadores de acceso no se permiten en declaraciones de miembro de enumeración.

### <a name="accessibility-domains"></a>Dominios de accesibilidad

El ***dominio de accesibilidad*** de un miembro consta de las secciones del texto del programa en el que se permite el acceso al miembro (posiblemente separadas). Para fines de definir el dominio de accesibilidad de un miembro, un miembro se dice que ***nivel superior*** si no se ha declarado dentro de un tipo, y se dice que ser un miembro ***anidada*** si se declara dentro de otro tipo. Además, el ***texto de programa*** de un programa se define como todos los programa texto contenido en todos los archivos de código fuente del programa, y el texto del programa de un tipo se define como todos los programa texto contenido en el *type_declaration*s de ese tipo (incluidos, posiblemente, los tipos que están anidados dentro del tipo).

El dominio de accesibilidad de un tipo predefinido (como `object`, `int`, o `double`) es ilimitado.

El dominio de accesibilidad de un nivel superior sin enlazar tipo `T` ([dependientes e independientes tipos](types.md#bound-and-unbound-types)) que se declara en un programa `P` se define como sigue:

*  Si la accesibilidad declarada de `T` es `public`, el dominio de accesibilidad de `T` es el texto del programa `P` y cualquier programa que hace referencia a `P`.
*  Si la accesibilidad declarada de `T` es `internal`, el dominio de accesibilidad de `T` es el texto de programa de `P`.

Desde estas definiciones se deduce que el dominio de accesibilidad de un tipo sin enlazar de nivel superior es, al menos el texto de programa del programa en el que ese tipo se declara.

El dominio de accesibilidad para un tipo construido `T<A1, ..., An>` es la intersección del dominio de accesibilidad del tipo genérico sin enlazar `T` y los dominios de accesibilidad de los argumentos de tipo `A1, ..., An`.

El dominio de accesibilidad de un miembro anidado `M` declarados en un tipo `T` dentro de un programa `P` se define como sigue (teniendo en cuenta que `M` probablemente sea un tipo):

*  Si la accesibilidad declarada de `M` es `public`, el dominio de accesibilidad de `M` es el dominio de accesibilidad de `T`.
*  Si la accesibilidad declarada de `M` es `protected internal`, permiten `D` será la unión del texto del programa `P` y el texto del programa de cualquier tipo derivado de `T`, que se declara fuera `P`. El dominio de accesibilidad de `M` es la intersección del dominio de accesibilidad de `T` con `D`.
*  Si la accesibilidad declarada de `M` es `protected`, permiten `D` será la unión del texto del programa `T` y el texto del programa de cualquier tipo derivado de `T`. El dominio de accesibilidad de `M` es la intersección del dominio de accesibilidad de `T` con `D`.
*  Si la accesibilidad declarada de `M` es `internal`, el dominio de accesibilidad de `M` es la intersección del dominio de accesibilidad de `T` con el texto de programa de `P`.
*  Si la accesibilidad declarada de `M` es `private`, el dominio de accesibilidad de `M` es el texto de programa de `T`.

Desde estas definiciones se deduce que el dominio de accesibilidad de un miembro anidado es, al menos el texto de programa del tipo en el que se declara el miembro. Además, se deduce que el dominio de accesibilidad de un miembro nunca es más inclusivo que el dominio de accesibilidad del tipo en el que se declara el miembro.

En términos intuitivos, cuando un tipo o miembro `M` es tener acceso a, se evalúan los pasos siguientes para asegurarse de que se permite el acceso:

*  Primero, if `M` se declara dentro de un tipo (en oposición a una unidad de compilación o un espacio de nombres), se produce un error de tiempo de compilación si ese tipo no es accesible.
*  A continuación, si `M` es `public`, se permite el acceso.
*  De lo contrario, si `M` es `protected internal`, se permite el acceso si se produce dentro del programa en el que `M` se declara, o si se produce dentro de una clase derivada de la clase en la que `M` se declara y se realiza a través de la clase derivada tipo de clase ([protegido por ejemplo acceder a los miembros](basic-concepts.md#protected-access-for-instance-members)).
*  De lo contrario, si `M` es `protected`, se permite el acceso si se produce dentro de la clase en el que `M` se declara, o si se produce dentro de una clase derivada de la clase en la que `M` se declara y se realiza a través de la clase derivada tipo de clase ([protegido por ejemplo acceder a los miembros](basic-concepts.md#protected-access-for-instance-members)).
*  De lo contrario, si `M` es `internal`, se permite el acceso si se produce dentro del programa en el que `M` se declara.
*  De lo contrario, si `M` es `private`, se permite el acceso si se produce dentro del tipo en el que `M` se declara.
*  En caso contrario, el tipo o miembro es inaccesible y se produce un error de tiempo de compilación.

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
las clases y miembros tienen los dominios de accesibilidad siguientes:

*  El dominio de accesibilidad de `A` y `A.X` es ilimitado.
*  El dominio de accesibilidad de `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, y `B.C.Y` es el texto de programa del programa que lo contiene.
*  El dominio de accesibilidad de `A.Z` es el texto del programa `A`.
*  El dominio de accesibilidad de `B.Z` y `B.D` es el texto del programa `B`, incluido el texto del programa `B.C` y `B.D`.
*  El dominio de accesibilidad de `B.C.Z` es el texto del programa `B.C`.
*  El dominio de accesibilidad de `B.D.X` y `B.D.Y` es el texto del programa `B`, incluido el texto del programa `B.C` y `B.D`.
*  El dominio de accesibilidad de `B.D.Z` es el texto del programa `B.D`.

Como se muestra en el ejemplo siguiente, el dominio de accesibilidad de un miembro nunca es mayor que el de un tipo contenedor. Por ejemplo, aunque todos los `X` los miembros tienen accesibilidad declarada public, todos excepto `A.X` tiene dominios de accesibilidad que están restringidos por un tipo contenedor.

Como se describe en [miembros](basic-concepts.md#members), todos los miembros de una clase base, excepto por ejemplo, constructores, destructores y constructores estáticos, son heredados por tipos derivados. Esto incluye incluso miembros privados de una clase base. Sin embargo, el dominio de accesibilidad de un miembro privado incluye sólo el texto de programa del tipo en el que se declara el miembro. En el ejemplo
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
el `B` clase hereda el miembro privado `x` desde el `A` clase. Dado que el miembro es privado, solo es accesible dentro de la *class_body* de `A`. Por lo tanto, el acceso a `b.x` funciona correctamente en el `A.F` método, pero se produce un error en la `B.F` método.

### <a name="protected-access-for-instance-members"></a>Acceso protegido para miembros de instancia

Cuando un `protected` se tiene acceso a miembros de instancia fuera del texto de la clase en el que se declara, y cuándo una `protected internal` se tiene acceso a miembros de instancia fuera del texto de programa del programa en el que se declara, el acceso debe tener lugar dentro de un declaración de clase que deriva de la clase en el que se declara. Además, el acceso se requiere que tenga lugar a través de una instancia de ese tipo de clase derivada o un tipo de clase construida a partir de él. Esta restricción impide que una clase derivada tengan acceso a miembros protegidos de otras clases derivadas, incluso cuando los miembros se heredan de la misma clase base.

Permiten `B` ser una clase base que declara un miembro de instancia protegida `M`y permiten `D` ser una clase que deriva de `B`. Dentro de la *class_body* de `D`, el acceso a `M` puede tomar uno de los formatos siguientes:

*  Un sin calificar *type_name* o *primary_expression* del formulario `M`.
*  Un *primary_expression* del formulario `E.M`, siempre que el tipo de `E` es `T` o una clase derivada de `T`, donde `T` es el tipo de clase `D`, o un tipo de clase construido a partir de `D`
*  Un *primary_expression* del formulario `base.M`.

Además de estas formas de acceso, una clase derivada puede tener acceso a un constructor de instancia protegida de una clase base en un *constructor_initializer* ([inicializadores de Constructor](classes.md#constructor-initializers)).

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
dentro de `A`, es posible obtener acceso a `x` a través de instancias de ambos `A` y `B`, ya que en ambos casos el acceso realiza a través de una instancia de `A` o una clase derivada de `A`. Sin embargo, dentro de `B`, no es posible tener acceso `x` a través de una instancia de `A`, puesto que `A` no se deriva de `B`.

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
las tres asignaciones a `x` se permiten porque tienen lugar a través de las instancias de tipos de clase construidos a partir del tipo genérico.

### <a name="accessibility-constraints"></a>Restricciones de accesibilidad

Varias construcciones del lenguaje C# requieren un tipo sea ***al menos tan accesibles como*** miembro u otro tipo. Un tipo `T` se dice que ser al menos tan accesibles como un miembro o tipo `M` si el dominio de accesibilidad de `T` es un superconjunto del dominio de accesibilidad de `M`. En otras palabras, `T` es al menos tan accesibles como `M` si `T` es accesible en todos los contextos en los que `M` es accesible.

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
*  Los tipos de parámetros de un constructor de instancia deben ser al menos tan accesibles como el propio constructor de instancia.

En el ejemplo
```csharp
class A {...}

public class B: A {...}
```
el `B` clase produce un error en tiempo de compilación porque `A` no es al menos tan accesibles como `B`.

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
el `H` método `B` da como resultado un error en tiempo de compilación porque el tipo de devolución `A` no es al menos tan accesibles como el método.

## <a name="signatures-and-overloading"></a>Firmas y sobrecarga

Los métodos, constructores de instancias, indizadores y los operadores se caracterizan por su ***firmas***:

*  La firma de un método se compone del nombre del método, el número de parámetros de tipo y el tipo y la categoría (valor, referencia o salida) de cada uno de sus parámetros formales, considerados en el orden de izquierda a derecha. Para estos fines, se identifica cualquier parámetro de tipo del método que se produce en el tipo de un parámetro formal no por su nombre, pero por su posición ordinal en la lista de argumentos de tipo del método. En concreto la firma de un método no incluye el tipo de valor devuelto, el `params` modificador que se puede especificar para el parámetro de la derecha, ni las restricciones de parámetro de tipo opcional.
*  La firma de un constructor de instancia se compone del tipo y la categoría (valor, referencia o salida) de cada uno de sus parámetros formales, considerados en el orden de izquierda a derecha. No incluye la firma de un constructor de instancia en concreto la `params` modificador que se puede especificar para el parámetro de la derecha.
*  La firma de un indizador se compone del tipo de cada uno de sus parámetros formales, considerados en el orden de izquierda a derecha. La firma de un indizador específicamente no incluye el tipo de elemento, ni incluye el `params` modificador que se puede especificar para el parámetro de la derecha.
*  La firma de un operador se compone del nombre del operador y el tipo de cada uno de sus parámetros formales, considerados en el orden de izquierda a derecha. En concreto, la firma de un operador no incluye el tipo de resultado.

Las firmas son el mecanismo que permite ***sobrecarga*** de los miembros de clases, structs e interfaces:

*  La sobrecarga de métodos permite que una clase, estructura o interfaz declare varios métodos con el mismo nombre, siempre que sus firmas sean únicas dentro de esa clase, estructura o interfaz.
*  La sobrecarga de constructores de instancia permite que una clase o struct declare varios constructores de instancia, siempre que sus firmas sean únicas dentro de esa clase o struct.
*  La sobrecarga de indizadores permite que una clase, estructura o interfaz declare varios indizadores, siempre que sus firmas sean únicas dentro de esa clase, estructura o interfaz.
*  La sobrecarga de operadores permite que una clase o struct declare varios operadores con el mismo nombre, siempre que sus firmas sean únicos dentro de esa clase o struct.

Aunque `out` y `ref` modificadores de parámetro se consideran parte de una firma, los miembros declarados en un tipo único no pueden diferenciarse únicamente mediante en firma `ref` y `out`. Se produce un error de tiempo de compilación si se declaran dos miembros en el mismo tipo con firmas que sería el misma si todos los parámetros de ambos métodos con `out` modificadores cambiaron a `ref` modificadores. Para otros fines de coincidencia de firma (por ejemplo, ocultar o reemplazar) `ref` y `out` se consideran parte de la firma y no coinciden entre sí. (Esta restricción es permitirle C# programas que se deben traducir fácilmente para ejecutarse en el Common Language Infrastructure (CLI), que no proporciona una manera de definir métodos que difieran únicamente en `ref` y `out`.)

Para los fines de firmas, los tipos de `object` y `dynamic` se consideran iguales. Los miembros declarados en un tipo único pueden, por tanto, no difieren en firma únicamente mediante `object` y `dynamic`.

El ejemplo siguiente muestra un conjunto de declaraciones de método sobrecargado, junto con sus firmas.
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

Tenga en cuenta que cualquier `ref` y `out` modificadores de parámetro ([parámetros del método](classes.md#method-parameters)) forman parte de una firma. Por lo tanto, `F(int)` y `F(ref int)` son firmas únicas. Sin embargo, `F(ref int)` y `F(out int)` no se pueden declarar dentro de la misma interfaz porque sus firmas se diferencian solamente por `ref` y `out`. Además, tenga en cuenta que el tipo de devolución y el `params` modificador no forman parte de una firma, por lo que no es posible sobrecargar solamente basado en el tipo de valor devuelto o en la inclusión o exclusión de la `params` modificador. Como tal, las declaraciones de los métodos `F(int)` y `F(params string[])` anteriormente identificadas producen un error en tiempo de compilación.

## <a name="scopes"></a>Ámbitos

El ***ámbito*** de un nombre es la región de texto dentro del cual es posible hacer referencia a la entidad declarada por el nombre sin la calificación del nombre del programa. Los ámbitos pueden ser ***anidada***, y un ámbito interno puede volver a declarar el significado de un nombre de un ámbito externo (no, sin embargo, se quitará la restricción impuesta por [declaraciones](basic-concepts.md#declarations) que dentro de un bloque anidado no es es posible declarar una variable local con el mismo nombre que una variable local en un bloque de inclusión). El nombre del ámbito externo, a continuación, se dice que ***oculto*** en la región del programa de texto cubierto por el ámbito interno, y acceso para el nombre exterior solo es posible calificando el nombre.

*  El ámbito de un miembro del espacio de nombres declarado por una *namespace_member_declaration* ([Namespace miembros](namespaces.md#namespace-members)) con ningún envolvente *namespace_declaration* es todo el programa texto.
*  El ámbito de un miembro del espacio de nombres declarado por una *namespace_member_declaration* dentro de un *namespace_declaration* cuyo nombre completo es `N` es el *namespace_body*  de cada *namespace_declaration* cuyo nombre completo es `N` o empieza por `N`, seguido de un período.
*  El ámbito del nombre definido por un *extern_alias_directive* se extiende a través de la *using_directive*s, *global_attributes* y *namespace_member_ declaración*s de su cuerpo contenedor inmediato de espacio de nombres o la unidad de compilación. Un *extern_alias_directive* no contribuye con ningún miembro nuevo al espacio de declaración subyacente. En otras palabras, un *extern_alias_directive* no es transitiva, pero, en su lugar, sólo afecta a la compilación unidad o el espacio de nombres de cuerpo en el que se produce.
*  El ámbito de un nombre definido o importado por un *using_directive* ([directivas Using](namespaces.md#using-directives)) se extiende a través de la *namespace_member_declaration*s de la  *compilation_unit* o *namespace_body* en el que el *using_directive* se produce. Un *using_directive* puede poner a cero o más nombres de espacio de nombres, tipo o miembro disposición dentro de un determinado *compilation_unit* o *namespace_body*, pero no lo hace contribuir con los nuevos miembros al espacio de declaración subyacente. En otras palabras, un *using_directive* no es transitiva, sino más bien afecta solo a la *compilation_unit* o *namespace_body* en el que se produce.
*  El ámbito de un parámetro de tipo declarado por una *type_parameter_list* en un *class_declaration* ([declaraciones de clase](classes.md#class-declarations)) es el *class_base*, *type_parameter_constraints_clause*s, y *class_body* eso *class_declaration*.
*  El ámbito de un parámetro de tipo declarado por una *type_parameter_list* en un *struct_declaration* ([declaraciones de estructura](structs.md#struct-declarations)) es el *struct_interfaces* , *type_parameter_constraints_clause*s, y *struct_body* eso *struct_declaration*.
*  El ámbito de un parámetro de tipo declarado por una *type_parameter_list* en un *interface_declaration* ([declaraciones de interfaz](interfaces.md#interface-declarations)) es el *interface_base* , *type_parameter_constraints_clause*s, y *interface_body* eso *interface_declaration*.
*  El ámbito de un parámetro de tipo declarado por una *type_parameter_list* en un *delegate_declaration* ([declaraciones de delegado](delegates.md#delegate-declarations)) es el *return_type*, *formal_parameter_list*, y *type_parameter_constraints_clause*s eso *delegate_declaration*.
*  El ámbito de un miembro declarado por una *class_member_declaration* ([clase cuerpo](classes.md#class-body)) es el *class_body* en que se produce la declaración. Además, el ámbito de un miembro de clase se extiende a la *class_body* de las clases derivadas que se incluyen en el dominio de accesibilidad ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)) del miembro.
*  El ámbito de un miembro declarado por una *struct_member_declaration* ([los miembros de Struct](structs.md#struct-members)) es el *struct_body* en que se produce la declaración.
*  El ámbito de un miembro declarado por una *enum_member_declaration* ([miembros de enumeración](enums.md#enum-members)) es el *enum_body* en que se produce la declaración.
*  El ámbito de un parámetro declarado en un *method_declaration* ([métodos](classes.md#methods)) es el *cuerpoMétodo* eso *method_declaration*.
*  El ámbito de un parámetro declarado en una *indexer_declaration* ([indizadores](classes.md#indexers)) es el *accessor_declarations* eso *indexer_declaration*.
*  El ámbito de un parámetro declarado en una *operator_declaration* ([operadores](classes.md#operators)) es el *bloque* eso *operator_declaration*.
*  El ámbito de un parámetro declarado en un *constructor_declaration* ([constructores de instancias](classes.md#instance-constructors)) es el *constructor_initializer* y *bloque* eso *constructor_declaration*.
*  El ámbito de un parámetro declarado en un *lambda_expression* ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) es el *anonymous_function_body* eso *lambda_ expresión*
*  El ámbito de un parámetro declarado en una *anonymous_method_expression* ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) es el *bloque* eso *anonymous_method _expression*.
*  El ámbito de una etiqueta declarada en una *labeled_statement* ([con la etiqueta instrucciones](statements.md#labeled-statements)) es el *bloque* en que se produce la declaración.
*  El ámbito de una variable local declarada en un *local_variable_declaration* ([declaraciones de variable Local](statements.md#local-variable-declarations)) es el bloque en que se produce la declaración.
*  El ámbito de una variable local declarada en un *switch_block* de un `switch` instrucción ([la instrucción switch](statements.md#the-switch-statement)) es el *switch_block*.
*  El ámbito de una variable local declarada en un *for_initializer* de un `for` instrucción ([la instrucción for](statements.md#the-for-statement)) es el *for_initializer*, el  *for_condition*, *for_iterator*y el objeto contenido *instrucción* de la `for` instrucción.
*  El ámbito de una constante local declarada en un *local_constant_declaration* ([declaraciones de constante Local](statements.md#local-constant-declarations)) es el bloque en que se produce la declaración. Es un error en tiempo de compilación para hacer referencia a una constante local en una posición textual que precede a su *constant_declarator*.
*  El ámbito de una variable declarada como parte de un *foreach_statement*, *using_statement*, *lock_statement* o *expresión_de_consulta* es determinado por la expansión de la construcción.

Dentro del ámbito de un miembro del espacio de nombres, clase, estructura o enumeración es posible hacer referencia al miembro en una posición textual que precede a la declaración del miembro. Por ejemplo
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
En este caso, es válido para `F` para hacer referencia a `i` antes de declararla.

Dentro del ámbito de una variable local, es un error en tiempo de compilación para hacer referencia a la variable local en una posición textual que precede a la *local_variable_declarator* de la variable local. Por ejemplo
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

En el `F` método anterior, la primera asignación `i` específicamente no hace referencia al campo declarado en el ámbito externo. En su lugar, hace referencia a la variable local y produce un error en tiempo de compilación porque le precede en la declaración de la variable. En el `G` método, el uso de `j` en el inicializador de la declaración de `j` es válido porque no precede el *local_variable_declarator*. En el `H` método, un posterior *local_variable_declarator* correctamente hace referencia a una variable local declarada en una sesión *local_variable_declarator* dentro del mismo  *local_variable_declaration*.

Las reglas de ámbito para las variables locales están diseñadas para garantizar que el significado de un nombre usado en un contexto de expresión siempre es el mismo dentro de un bloque. Si fuera del ámbito de una variable local ampliar solo de su declaración hasta el final del bloque, a continuación, en el ejemplo anterior, la primera asignación asignará a la variable de instancia y la segunda asignación asignaría a la variable local, es posible errores de tiempo de compilación si más adelante se reorganicen fueron de las instrucciones del bloque.

El significado de un nombre dentro de un bloque puede diferir según el contexto en el que se usa el nombre. En el ejemplo
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

El ámbito de una entidad normalmente abarca más texto del programa que el espacio de declaración de la entidad. En concreto, el ámbito de una entidad puede incluir declaraciones que introducen nuevos espacios de declaración que contiene las entidades del mismo nombre. Estas declaraciones hacen que la entidad original para convertirse en ***oculto***. Por el contrario, se dice que una entidad se ***visible*** cuando no está oculto.

Ocultación de nombres se produce cuando los ámbitos se superponen a través de anidamiento y cuando se superponen a través de herencia. Las características de los dos tipos de ocultación se describen en las secciones siguientes.

#### <a name="hiding-through-nesting"></a>Ocultar mediante anidación

A través de anidamiento de la ocultación de nombres pueden producir como resultado de anidamiento de los espacios de nombres o tipos dentro de espacios de nombres, como resultado de anidamiento de tipos dentro de clases o structs y el resultado de parámetro y declaraciones de variable local.

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
dentro de la `F` método, la variable de instancia `i` está oculto por la variable local `i`, pero dentro del `G` método, `i` todavía hace referencia a la variable de instancia.

Cuando un nombre en un ámbito interno oculta un nombre en un ámbito externo, oculta todas las apariciones sobrecargadas de ese nombre. En el ejemplo
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
la llamada `F(1)` invoca el `F` declarado en `Inner` porque todas las apariciones externas de `F` oculta la declaración interna. Para la misma razón, la llamada `F("Hello")` da como resultado un error en tiempo de compilación.

#### <a name="hiding-through-inheritance"></a>Ocultar mediante herencia

A través de la herencia de la ocultación de nombres se produce cuando las clases o structs declarar nombres que se han heredado de clases base. Este tipo de ocultación de nombres toma una de las siguientes formas:

*  Constante, campo, propiedad, evento o tipo introducido en una clase o struct oculta a todos los miembros de clase base con el mismo nombre.
*  Un método introducido en una clase o struct oculta todos los miembros de clase base que no son de método con el mismo nombre y todos los métodos de clase base con la misma firma (nombre del método y el número de parámetros, modificadores y tipos).
*  Un indizador introducido en una clase o struct oculta todos los indizadores de clase base con la misma firma (número de parámetros y tipos).

Las reglas que rigen las declaraciones de operadores ([operadores](classes.md#operators)) hacen que sea imposible que una clase derivada declarar un operador con la misma firma que un operador en una clase base. Por lo tanto, los operadores nunca ocultan entre sí.

Al contrario de ocultar un nombre de un ámbito externo, un nombre accesible desde un ámbito heredado se ocultan, una advertencia que se notificarán. En el ejemplo
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
la declaración de `F` en `Derived` , notificará un mensaje de advertencia. Ocultar un nombre heredado no es específicamente un error, puesto que esto impediría la evolución independiente de las clases base. Por ejemplo, la situación anterior podría haber producido porque una versión posterior de `Base` introdujo un `F` método que no estaba presente en una versión anterior de la clase. Si la situación anterior que hubiera un error, a continuación, cualquier cambio realizado en una clase base en una biblioteca de clases por separado con control de versiones podría convertir las clases derivadas para dejar de ser válido.

La advertencia causada por la ocultación de un nombre heredado puede eliminarse mediante el uso de la `new` modificador:
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

El `new` modificador indica que el `F` en `Derived` es "new", y que realmente está pensado para ocultar el miembro heredado.

Una declaración de un nuevo miembro oculta a un miembro heredado solo dentro del ámbito del nuevo miembro.

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

En el ejemplo anterior, la declaración de `F` en `Derived` oculta la `F` que heredó `Base`, pero como el nuevo `F` en `Derived` tiene acceso privado, su ámbito no se extiende a `MoreDerived` . Por lo tanto, la llamada `F()` en `MoreDerived.G` es válido y que invocará `Base.F`.

## <a name="namespace-and-type-names"></a>Namespace y nombres de tipo

Algunos contextos de un C# programa requiere una *namespace_name* o un *type_name* que se especifique.

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

Un *namespace_name* es un *namespace_or_type_name* que hace referencia a un espacio de nombres. Seguir resolución tal y como se describe a continuación, el *namespace_or_type_name* de un *namespace_name* debe hacer referencia a un espacio de nombres o de lo contrario se produce un error en tiempo de compilación. Ningún argumento de tipo ([argumentos de tipo](types.md#type-arguments)) pueden estar presentes en un *namespace_name* (solo tipos pueden tener argumentos de tipo).

Un *type_name* es un *namespace_or_type_name* que hace referencia a un tipo. Seguir resolución tal y como se describe a continuación, el *namespace_or_type_name* de un *type_name* debe hacer referencia a un tipo, o en caso contrario, se produce un error en tiempo de compilación.

Si el *namespace_or_type_name* es un completo-alias-miembro de su significado como se describe en [calificadores de alias Namespace](namespaces.md#namespace-alias-qualifiers). En caso contrario, un *namespace_or_type_name* tiene uno de estos cuatro formatos:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

donde `I` es un identificador único, `N` es un *namespace_or_type_name* y `<A1, ..., Ak>` es una función opcional *type_argument_list*. Cuando no hay ninguna *type_argument_list* está especificado, considere la posibilidad de `k` sea cero.

El significado de un *namespace_or_type_name* se determina como sigue:

*   Si el *namespace_or_type_name* tiene el formato `I` o tiene el formato `I<A1, ..., Ak>`:
    * Si `K` es cero y el *namespace_or_type_name* aparece dentro de una declaración de método genérico ([métodos](classes.md#methods)) y si dicha declaración incluye un parámetro de tipo ([tipo parámetros](classes.md#type-parameters)) con el nombre `I`, el *namespace_or_type_name* hace referencia a ese parámetro de tipo.
    * De lo contrario, si la *namespace_or_type_name* aparece dentro de una declaración de tipo y, después, para cada tipo de instancia `T` ([el tipo de instancia](classes.md#the-instance-type)), empezando por el tipo de instancia de ese tipo declaración y continuando con el tipo de instancia de cada declaración de clase o estructura envolvente (si existe):
        * Si `K` es cero y la declaración de `T` incluye un parámetro de tipo con nombre `I`, el *namespace_or_type_name* hace referencia a ese parámetro de tipo.
        * De lo contrario, si la *namespace_or_type_name* aparece dentro del cuerpo de la declaración de tipos, y `T` o cualquiera de sus tipos base contiene un tipo accesible anidado con el nombre `I` y `K`  parámetros de tipo, el *namespace_or_type_name* hace referencia a ese tipo construido con los argumentos de tipo especificado. Si hay más de uno de estos tipos, se selecciona el tipo declarado dentro del tipo más derivado. Tenga en cuenta que los miembros sin tipo (constantes, campos, métodos, propiedades, indizadores, operadores, constructores de instancias, destructores y constructores estáticos) y los miembros de tipo con un número diferente de parámetros de tipo se omiten al determinar el significado de la *namespace_or_type_name*.
    * Si los pasos anteriores eran incorrectas, a continuación, para cada espacio de nombres `N`, empezando por el espacio de nombres en el que el *namespace_or_type_name* se produce, continúe con cada espacio de nombres envolvente (si existe) y terminando con el espacio de nombres global, se evalúan los pasos siguientes hasta que se encuentra una entidad:
        * Si `K` es cero y `I` es el nombre de un espacio de nombres en `N`, a continuación:
            * Si la ubicación donde el *namespace_or_type_name* se produce entre una declaración de espacio de nombres para `N` y la declaración de espacio de nombres contiene un *extern_alias_directive* o *using_alias_directive* que asocia el nombre `I` con un espacio de nombres o tipo, el *namespace_or_type_name* es ambiguo y se produce un error en tiempo de compilación.
            * En caso contrario, el *namespace_or_type_name* hace referencia al espacio de nombres denominado `I` en `N`.
        * De lo contrario, si `N` contiene un tipo accesible con nombre `I` y `K`  parámetros de tipo:
            * Si `K` es cero y la ubicación donde el *namespace_or_type_name* se produce entre una declaración de espacio de nombres para `N` y la declaración de espacio de nombres contiene un *extern_alias_directive*  o *using_alias_directive* que asocia el nombre `I` con un espacio de nombres o tipo, el *namespace_or_type_name* es ambiguo y un tiempo de compilación se produce un error.
            * En caso contrario, el *namespace_or_type_name* hace referencia al tipo construido con los argumentos de tipo especificado.
        * De lo contrario, si la ubicación donde el *namespace_or_type_name* se produce entre una declaración de espacio de nombres para `N`:
            * Si `K` es cero y la declaración de espacio de nombres contiene un *extern_alias_directive* o *using_alias_directive* que asocia el nombre `I` con un espacio de nombres importado o tipo, el *namespace_or_type_name* hace referencia a dicho espacio de nombres o tipo.
            * En caso contrario, si las declaraciones de espacios de nombres y el tipo importan por el *using_namespace_directive*s y *using_alias_directive*s de la declaración de espacio de nombres contiene exactamente un tipo accesible con el nombre `I` y `K`  parámetros de tipo, el *namespace_or_type_name* hace referencia a ese tipo construido con los argumentos de tipo especificado.
            * En caso contrario, si las declaraciones de espacios de nombres y el tipo importan por el *using_namespace_directive*s y *using_alias_directive*s de la declaración de espacio de nombres contiene más de un tipo accesible con el nombre `I` y `K`  parámetros de tipo, el *namespace_or_type_name* es ambiguo y se produce un error.
    * En caso contrario, el *namespace_or_type_name* es no definido y se produce un error de tiempo de compilación.
*  En caso contrario, el *namespace_or_type_name* tiene el formato `N.I` o tiene el formato `N.I<A1, ..., Ak>`. `N` en primer lugar se resuelve como un *namespace_or_type_name*. Si la resolución de `N` no es correcta, se produce un error de tiempo de compilación. En caso contrario, `N.I` o `N.I<A1, ..., Ak>` se resuelve como sigue:
    * Si `K` es cero y `N` hace referencia a un espacio de nombres y `N` contiene un espacio de nombres anidado con nombre `I`, el *namespace_or_type_name* hace referencia a ese espacio de nombres anidado.
    * De lo contrario, si `N` hace referencia a un espacio de nombres y `N` contiene un tipo accesible con nombre `I` y `K`  parámetros de tipo, el *namespace_or_type_name* hace referencia en ese tipo construido con los argumentos de tipo especificado.
    * De lo contrario, si `N` hace referencia a un tipo de clase o struct (posiblemente construido) y `N` o cualquiera de sus clases base contiene un tipo accesible anidado con el nombre `I` y `K`  escriba los parámetros y, a continuación, el *namespace_or_type_name* hace referencia a ese tipo construido con los argumentos de tipo especificado. Si hay más de uno de estos tipos, se selecciona el tipo declarado dentro del tipo más derivado. Observe que si el significado de `N.I` se determina como parte de la resolución de la especificación de clase base `N` , a continuación, la clase base directa de `N` se considera que es objeto ([clases Base](classes.md#base-classes)).
    * En caso contrario, `N.I` no es válida *namespace_or_type_name*, y se produce un error en tiempo de compilación.

Un *namespace_or_type_name* tiene permiso para hacer referencia a una clase estática ([clases estáticas](classes.md#static-classes)) solo si

*  El *namespace_or_type_name* es el `T` en un *namespace_or_type_name* del formulario `T.I`, o
*  El *namespace_or_type_name* es el `T` en un *typeof_expression* ([listas de argumentos](expressions.md#argument-lists)1) del formulario `typeof(T)`.

### <a name="fully-qualified-names"></a>Nombres completos

Cada espacio de nombres y el tipo tiene un ***nombre completo***, que identifica el espacio de nombres o tipo entre todos los demás. El nombre completo de un espacio de nombres o tipo `N` se determina como sigue:

*  Si `N` es un miembro del espacio de nombres global, su nombre completo es `N`.
*  En caso contrario, es su nombre completo `S.N`, donde `S` es el nombre completo del espacio de nombres o tipo en el que `N` se declara.

En otras palabras, el nombre completo de `N` es la ruta de acceso jerárquica completa de identificadores que conducen a `N`, empezando por el espacio de nombres global. Dado que todos los miembros de un tipo o espacio de nombres deben tener un nombre único, se deduce que el nombre completo de un espacio de nombres o tipo siempre es único.

El ejemplo siguiente muestra varias declaraciones de espacio de nombres y tipo junto con sus nombres completos asociados.
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

C# usa la administración automática de la memoria, lo que libera a los desarrolladores de forma manual, asignar y liberar la memoria ocupada por objetos. Las directivas de administración automática de la memoria se implementan mediante un ***recolector de elementos no utilizados***. El ciclo de vida de administración de memoria de un objeto es como sigue:

1. Cuando se crea el objeto, se le asigna memoria, se ejecuta el constructor y el objeto se considera activo.
2. Si el objeto, o cualquier parte del mismo, no se puede tener acceso a las posibles continuaciones de la ejecución de, aparte de los destructores, se considera el objeto ya no está en uso, y se convierte en apto para su destrucción. El compilador de C# y el recolector de elementos no utilizados puede analizar el código para determinar qué referencias a un objeto pueden utilizarse en el futuro. Por ejemplo, si una variable local que está en el ámbito es la única referencia existente a un objeto, pero esa variable local nunca se hace referencia en cualquier continuación posible de la ejecución de la ejecución actual del punto en el procedimiento, el recolector de elementos no utilizados puede (pero no es debe) tratar el objeto que ya no usa.
3. Una vez que el objeto es elegible para su destrucción, algunas más adelante no se especifica el tiempo el destructor ([destructores](classes.md#destructors)) (si existe) para el objeto se ejecuta. En circunstancias normales el destructor del objeto se ejecuta solo una vez, aunque las API específicas de la implementación pueden permitir que invalidar este comportamiento.
4. Una vez que se ejecuta el destructor de un objeto, si ese objeto, o cualquier parte del mismo, no se puede tener acceso a las posibles continuaciones de ejecución, incluida la ejecución de los destructores, el objeto se considera inaccesible y el objeto se convierte en apto para la colección.
5. Por último, en algún momento después de que el objeto se convierte en apto para la recolección, el recolector de elementos no utilizados libera la memoria asociada con ese objeto.

El recolector de elementos no utilizados mantiene información sobre el uso del objeto y usa esta información para tomar decisiones de administración, de memoria, tales como where en la memoria para localizar un objeto recién creado, cuando reubicar un objeto y cuando un objeto ya no está en uso o es inaccesible.

Al igual que otros lenguajes que asumen la existencia de un recolector de elementos no utilizados, C# está diseñado para que el recolector de elementos no utilizados puede implementar una amplia gama de directivas de administración de memoria. Por ejemplo, C# no requiere que los destructores se ejecuten o que se recopilan objetos tan pronto como son aptos o que los destructores se ejecute en un orden determinado, o en un subproceso concreto.

Se puede controlar el comportamiento del recolector de elementos no utilizados, hasta cierto punto, a través de métodos estáticos en la clase `System.GC`. Esta clase puede utilizarse para solicitar una colección a los destructores se ejecutará (o no) y así sucesivamente.

Dado que el recolector de elementos no utilizados se permite una amplia libertad para decidir cuándo debe recolectar objetos y ejecutar destructores, una implementación compatible puede producir un resultado diferente del que se muestra en el código siguiente. El programa
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
crea una instancia de clase `A` y una instancia de la clase `B`. Estos objetos son candidatos para la recolección cuando la variable `b` se asigna el valor `null`, ya que esta tarde es imposible que cualquier código escrito por el usuario para tener acceso a ellos. El resultado puede ser
```
Destruct instance of A
Destruct instance of B
```
o
```
Destruct instance of B
Destruct instance of A
```
Dado que el idioma impone ninguna restricción en el orden en que los objetos se recolectan.

En casos concretos, la distinción entre "elegible para su destrucción" y "elegible para la recolección" puede ser importante. Por ejemplo,
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

En el programa anterior, si opta por el recolector de elementos no utilizados ejecuta el destructor de `A` antes que el destructor de `B`, a continuación, el resultado de este programa puede ser:
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Tenga en cuenta que aunque la instancia de `A` no estaba en uso y `A`del destructor se ejecutó, sigue siendo posible para los métodos de `A` (en este caso, `F`) que se llame desde otro destructor. Además, tenga en cuenta que la ejecución de un destructor puede producir un objeto vuelva a ser utilizable desde el programa principal. En este caso, la ejecución de `B`del destructor hizo que una instancia de `A` que anteriormente estaba no en uso para ser accesible desde la referencia activa `Test.RefA`. Después de llamar a `WaitForPendingFinalizers`, la instancia de `B` es apto para la colección, pero la instancia de `A` no lo es, debido a la referencia `Test.RefA`.

Para evitar confusiones y comportamientos inesperados, por lo general es una buena idea para los destructores para realizar la limpieza únicamente en datos almacenados en los campos del objeto y no para realizar acciones en los objetos que se hace referencia o campos estáticos.

Es una alternativa al uso de los destructores permitir que una clase implementa la `System.IDisposable` interfaz. Esto permite al cliente del objeto determinar cuándo se debe liberar los recursos del objeto, normalmente mediante el acceso a los objetos como un recurso en un `using` instrucción ([la instrucción using](statements.md#the-using-statement)).

## <a name="execution-order"></a>Orden de ejecución

Ejecución de un programa de C# continúa tal que se conservan los efectos secundarios de cada subproceso en ejecución en puntos críticos de ejecución. Un ***efecto*** se define como una lectura o escritura de un campo volátil, una escritura en una variable no volátil, la escritura en un recurso externo y el inicio de una excepción. Los puntos de ejecución críticos en el que debe conservarse el orden de estos efectos son referencias a campos volátiles ([campos volátiles](classes.md#volatile-fields)), `lock` instrucciones ([la instrucción lock](statements.md#the-lock-statement)), y creación de subprocesos y terminación. El entorno de ejecución es libre de cambiar el orden de ejecución de un programa de C#, sujeto a las restricciones siguientes:

*  Se conservará la dependencia de datos dentro de un subproceso de ejecución. Es decir, el valor de cada variable se calcula como si todas las instrucciones en el subproceso se ejecutaron en el orden del programa original.
*  Las reglas se conservan el orden de inicialización ([inicialización de campos](classes.md#field-initialization) y [inicializadores de variables](classes.md#variable-initializers)).
*  El orden de los efectos secundarios se conserva con respecto a las lecturas y escrituras ([campos volátiles](classes.md#volatile-fields)). Además, el entorno de ejecución no necesitará evaluar parte de una expresión si puede deducir que el valor de la expresión no se utiliza y que no hay efectos secundarios necesarios se crean (incluidos los causados por una llamada a un método o tener acceso a un campo volátil). Cuando se interrumpe la ejecución del programa un evento asincrónico (por ejemplo, una excepción iniciada por otro subproceso), no se garantiza que los efectos secundarios observables son visibles en el orden del programa original.
