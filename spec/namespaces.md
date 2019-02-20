# <a name="namespaces"></a>Espacios de nombres

Programas de C# se organizan mediante espacios de nombres. Los espacios de nombres se utilizan como un sistema de organización "interno" para un programa y como un sistema de organización "external": una forma de presentar los elementos de programa que se exponen a otros programas.

Las directivas using ([directivas Using](namespaces.md#using-directives)) se proporcionan para facilitar el uso de espacios de nombres.

## <a name="compilation-units"></a>Unidades de compilación

Un *compilation_unit* define la estructura general de un archivo de origen. Una unidad de compilación se compone de cero o más *using_directive*s seguido de cero o más *global_attributes* seguido de cero o más *namespace_member_declaration*s .

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

Un programa de C# consta de uno o más unidades de compilación, contenidos todos ellos en un archivo de código fuente independiente. Cuando se compila un programa de C#, todas las unidades de compilación se procesan juntos. Por lo tanto, unidades de compilación pueden depender entre sí, posiblemente, de manera circular.

El *using_directive*s una unidad de compilación pueden afectar a la *global_attributes* y *namespace_member_declaration*s de esa unidad de compilación, pero no tienen ningún efecto otras unidades de compilación.

El *global_attributes* ([atributos](attributes.md)) de una unidad de compilación permiten la especificación de atributos para el ensamblado de destino y el módulo. Ensamblados y módulos actúan como contenedores físicos de tipos. Un ensamblado puede constar de varios módulos separados físicamente.

El *namespace_member_declaration*s de cada unidad de compilación de un programa contribuyen a los miembros a un espacio de declaración única denominado el espacio de nombres global. Por ejemplo:

Archivo `A.cs`:
```csharp
class A {}
```

Archivo `B.cs`:
```csharp
class B {}
```

Las dos unidades de compilación contribuyen al espacio de nombres global único, en este caso declarar dos clases con los nombres completos `A` y `B`. Dado que las dos unidades de compilación contribuyen al mismo espacio de declaración, habría sido un error si cada una contiene una declaración de un miembro con el mismo nombre.

## <a name="namespace-declarations"></a>Declaraciones de espacio de nombres

Un *namespace_declaration* consta de la palabra clave `namespace`, seguido de un espacio de nombres y un cuerpo, seguido opcionalmente por un punto y coma.

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

Un *namespace_declaration* puede producirse como una declaración de nivel superior en un *compilation_unit* o como una declaración de miembro dentro de otra *namespace_declaration*. Cuando un *namespace_declaration* se produce como una declaración de nivel superior en un *compilation_unit*, el espacio de nombres se convierte en miembro del espacio de nombres global. Cuando un *namespace_declaration* se produce dentro de otra *namespace_declaration*, el espacio de nombres interno se convierte en un miembro del espacio de nombres exterior. En cualquier caso, el nombre de un espacio de nombres debe ser único dentro del espacio de nombres contenedor.

Los espacios de nombres son implícitamente `public` y la declaración de un espacio de nombres no puede incluir modificadores de acceso.

Dentro de un *namespace_body*, opcional *using_directive*s importar los nombres de otros espacios de nombres, tipos y miembros, lo que permite hacer referencia a él directamente en lugar de a través de los nombres completos. El elemento opcional *namespace_member_declaration*s contribuyen a los miembros del espacio de declaración de espacio de nombres. Tenga en cuenta que todos los *using_directive*s deben aparecer antes que cualquier declaración de miembro.

El *qualified_identifier* de un *namespace_declaration* puede ser un identificador único o una secuencia de identificadores separados por "`.`" tokens. La segunda forma permite que un programa para definir un espacio de nombres anidado sin anidar léxicamente varias declaraciones de espacio de nombres. Por ejemplo,

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
es semánticamente equivalente a
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

Los espacios de nombres son abiertas y dos declaraciones de espacio de nombres con el mismo nombre completo contribuyen al mismo espacio de declaración ([declaraciones](basic-concepts.md#declarations)). En el ejemplo
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
Las dos declaraciones de espacio de nombres anteriores contribuyen al mismo espacio de declaración, en este caso declarar dos clases con los nombres completos `N1.N2.A` y `N1.N2.B`. Dado que las dos declaraciones contribuyen al mismo espacio de declaración, habría sido un error si cada uno contiene una declaración de un miembro con el mismo nombre.

## <a name="extern-aliases"></a>Alias externos

Un *extern_alias_directive* presenta un identificador que actúa como un alias para un espacio de nombres. La especificación del espacio de nombres con alias es externa al código fuente del programa y se aplica también a los espacios de nombres anidados del espacio de nombres con alias.

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

El ámbito de un *extern_alias_directive* se extiende a través de la *using_directive*s, *global_attributes* y *namespace_member_declaration*s de su cuerpo contenedor inmediato de espacio de nombres o la unidad de compilación.

Dentro de un cuerpo de espacio de nombres o la unidad de compilación que contiene un *extern_alias_directive*, el identificador definido por el *extern_alias_directive* puede usarse para hacer referencia el espacio de nombres con alias. Es un error en tiempo de compilación para la *identificador* debe ser la palabra `global`.

Un *extern_alias_directive* ofrece un alias dentro de un cuerpo de espacio de nombres o la unidad de compilación concreta, pero no aporta ningún miembro nuevo al espacio de declaración subyacente. En otras palabras, un *extern_alias_directive* no es transitiva, pero, en su lugar, sólo afecta a la compilación unidad o el espacio de nombres de cuerpo en el que se produce.

El programa siguiente declara y usa dos alias externos, `X` y `Y`, cada uno de los que representan la raíz de una jerarquía de espacio de nombres distinto:
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

El programa declara la existencia de la extern alias `X` y `Y`, pero las definiciones actuales de los alias son externas al programa. Con el mismo nombre `N.B` ahora se pueden hacer referencia a las clases como `X.N.B` y `Y.N.B`, o bien, mediante el calificador de alias de espacio de nombres, `X::N.B` y `Y::N.B`. Se produce un error si un programa declara un alias externo para el que no se proporciona ninguna definición externa.

## <a name="using-directives"></a>directivas using

***Directivas using*** facilitan el uso de espacios de nombres y tipos definidos en otros espacios de nombres. Mediante el impacto de las directivas en el proceso de resolución de nombres de *namespace_or_type_name*s ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)) y *simple_name*s ([nombres simples ](expressions.md#simple-names)), pero a diferencia de las declaraciones, las directivas using no contribuyen nuevos miembros a los espacios de declaración subyacentes de las unidades de compilación o espacios de nombres en el que se usan.

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

Un *using_alias_directive* ([directivas Using alias](namespaces.md#using-alias-directives)) introduce un alias para un espacio de nombres o tipo.

Un *using_namespace_directive* ([directivas de espacio de nombres Using](namespaces.md#using-namespace-directives)) importa los miembros de tipo de un espacio de nombres.

Un *using_static_directive* ([directivas Using static](namespaces.md#using-static-directives)) importa los tipos anidados y miembros estáticos de un tipo.

El ámbito de un *using_directive* se extiende a través de la *namespace_member_declaration*s de su cuerpo contenedor inmediato de espacio de nombres o la unidad de compilación. El ámbito de un *using_directive* específicamente no incluye su interlocutor *using_directive*s. Por lo tanto, del mismo nivel *using_directive*s no afectan a entre sí y el orden en el que se escriben es insignificante.

### <a name="using-alias-directives"></a>Directivas de alias using

Un *using_alias_directive* presenta un identificador que actúa como un alias para un espacio de nombres o tipo dentro del cuerpo de espacio de nombres o la unidad de compilación inmediatamente envolvente.

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

Dentro de las declaraciones de miembros en un cuerpo de espacio de nombres o la unidad de compilación que contiene un *using_alias_directive*, el identificador definido por el *using_alias_directive* puede usarse para hacer referencia a la determinada espacio de nombres o tipo. Por ejemplo:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

Anterior, dentro de las declaraciones de miembros en el `N3` espacio de nombres, `A` es un alias para `N1.N2.A`y, por tanto, la clase `N3.B` se deriva de la clase `N1.N2.A`. El mismo efecto que puede obtenerse mediante la creación de un alias `R` para `N1.N2` y, a continuación, hacer referencia a `R.A`:
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

El *identificador* de un *using_alias_directive* debe ser único dentro del espacio de declaración de la unidad de compilación o espacio de nombres que contiene inmediatamente la *using_alias_directive* . Por ejemplo:
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

Anterior, `N3` ya contiene un miembro `A`, por lo que es un error en tiempo de compilación para un *using_alias_directive* utilice ese identificador. Del mismo modo, es un error en tiempo de compilación para dos o más *using_alias_directive*s en el cuerpo mismo de espacio de nombres o la unidad de compilación para declarar los alias con el mismo nombre.

Un *using_alias_directive* ofrece un alias dentro de un cuerpo de espacio de nombres o la unidad de compilación concreta, pero no aporta ningún miembro nuevo al espacio de declaración subyacente. En otras palabras, un *using_alias_directive* no es transitiva, pero en su lugar sólo afecta a la compilación unidad o el espacio de nombres de cuerpo en el que se produce. En el ejemplo
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
el ámbito de la *using_alias_directive* que presenta `R` sólo se extiende a las declaraciones de miembros en el cuerpo de espacio de nombres que lo contiene, por lo que `R` es desconocido en la segunda declaración de espacio de nombres. Sin embargo, colocar el *using_alias_directive* en la compilación que lo contiene unidad hace que el alias para que estén disponibles en ambas declaraciones de espacio de nombres:
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

Al igual que los miembros normales, los nombres introducen por *using_alias_directive*s se ocultan los miembros con el mismo nombre en ámbitos anidados. En el ejemplo
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
la referencia a `R.A` en la declaración de `B` genera un error de tiempo de compilación porque `R` hace referencia a `N3.R`, no `N1.N2`.

El orden en que *using_alias_directive*s se escriben no tiene ninguna importancia y la resolución de la *namespace_or_type_name* hace referencia a él un *using_alias_directive*no se ve afectado por la *using_alias_directive* propio o por otro *using_directive*s en el cuerpo de espacio de nombres o la unidad de compilación contenedor inmediato. En otras palabras, el *namespace_or_type_name* de un *using_alias_directive* se resuelve como si el cuerpo de espacio de nombres o la unidad de compilación contenedor inmediato no tuviese *using_directive*s. Un *using_alias_directive* pero puede ser afectado por *extern_alias_directive*s en el cuerpo de espacio de nombres o la unidad de compilación contenedor inmediato. En el ejemplo
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
la última *using_alias_directive* da como resultado un error en tiempo de compilación porque no se ve afectado por la primera *using_alias_directive*. La primera *using_alias_directive* no produce un error desde el ámbito de lo alias externo `E` incluye la *using_alias_directive*.

Un *using_alias_directive* puede crear un alias para cualquier espacio de nombres o tipo, incluido el espacio de nombres en el que aparece y cualquier espacio de nombres o tipo anidado dentro de ese espacio de nombres.

Acceso a un tipo o espacio de nombres a través de un alias produce exactamente el mismo resultado que obtener acceso a dicho espacio de nombres o tipo mediante su nombre declarado. Por ejemplo, dada
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
los nombres `N1.N2.A`, `R1.N2.A`, y `R2.A` son equivalentes y todos hacen referencia a la clase cuyo nombre completo es `N1.N2.A`.

El uso de alias puede nombre de un tipo construido cerrado, pero no el nombre de una declaración de tipo genérico sin enlazar sin proporcionar argumentos de tipo. Por ejemplo:
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a>Espacio de nombres directivas using

Un *using_namespace_directive* importa los tipos contenidos en un espacio de nombres en la compilación cuerpo inmediatamente envolvente unidad o el espacio de nombres, lo que permite el identificador de cada tipo que se puede usar sin calificación.

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

Dentro de las declaraciones de miembros en un cuerpo de espacio de nombres o la unidad de compilación que contiene un *using_namespace_directive*, pueden hacer referencia directamente a los tipos contenidos en el espacio de nombres especificado. Por ejemplo:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

Anterior, dentro de las declaraciones de miembros en el `N3` espacio de nombres, los miembros de tipo de `N1.N2` están directamente disponibles y, por tanto, la clase `N3.B` se deriva de la clase `N1.N2.A`.

Un *using_namespace_directive* importa los tipos contenidos en el espacio de nombres especificado, pero no importa específicamente los espacios de nombres anidados. En el ejemplo
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
el *using_namespace_directive* importa los tipos contenidos en `N1`, pero no los espacios de nombres anidados en `N1`. Por lo tanto, la referencia a `N2.A` en la declaración de `B` da como resultado un error en tiempo de compilación porque ningún miembro llamado `N2` están en el ámbito.

A diferencia de un *using_alias_directive*, un *using_namespace_directive* puede importar tipos cuyos identificadores ya están definidos dentro del cuerpo de espacio de nombres o la unidad de compilación envolvente. De hecho, los nombres importan por un *using_namespace_directive* están ocultos por miembros con el mismo nombre en el cuerpo de espacio de nombres o la unidad de compilación envolvente. Por ejemplo:
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

Aquí, en las declaraciones de miembros en el `N3` espacio de nombres, `A` hace referencia a `N3.A` lugar `N1.N2.A`.

Cuando más de un espacio de nombres o tipo importado por *using_namespace_directive*s o *using_static_directive*s en el mismo cuerpo de espacio de nombres o la unidad de compilación contiene tipos para el mismo nombre, las referencias a ese nombre como un *type_name* se consideran ambiguos. En el ejemplo
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
ambos `N1` y `N2` contienen un miembro `A`y porque `N3` importa ambos, que hacen referencia a `A` en `N3` es un error en tiempo de compilación. En esta situación, el conflicto se puede resolver mediante la calificación de referencias a `A`, o bien introduciendo un *using_alias_directive* que toma un determinado `A`. Por ejemplo:
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

Además, cuando más de un espacio de nombres o tipo importado por *using_namespace_directive*s o *using_static_directive*s en el mismo cuerpo de espacio de nombres o la unidad de compilación contiene tipos o miembros mediante el mismo nombre, las referencias a ese nombre como un *simple_name* se consideran ambiguos. En el ejemplo
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
`N1` contiene un miembro de tipo `A`, y `C` contiene un campo estático `A`y porque `N2` importa ambos, que hacen referencia a `A` como un *simple_name* es ambiguo y un tiempo de compilación error. 

Al igual que un *using_alias_directive*, un *using_namespace_directive* no contribuye con ningún miembro nuevo al espacio de declaración subyacente de la unidad de compilación o espacio de nombres, sino que afecta a solo el compilación cuerpo unidad o el espacio de nombres en el que aparece.

El *namespace_name* hace referencia a él un *using_namespace_directive* se resuelve en la misma manera que el *namespace_or_type_name* hace referencia a él un  *using_alias_directive*. Por lo tanto, *using_namespace_directive*s en el mismo cuerpo de espacio de nombres o la unidad de compilación no afectan a entre sí y pueden escribirse en cualquier orden.

### <a name="using-static-directives"></a>Directivas using static

Un *using_static_directive* importa los tipos anidados y miembros estáticos contenidos directamente en una declaración de tipo en el cuerpo inmediatamente envolvente compilación unidad o el espacio de nombres, lo que permite el identificador de cada miembro y tipo que utilizar sin calificación.

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

Dentro de las declaraciones de miembros en un cuerpo de espacio de nombres o la unidad de compilación que contiene un *using_static_directive*, el accesible anidar tipos y miembros estáticos (excepto los métodos de extensión) contenidos directamente en la declaración de la tipo especificado se puede hacer referencia directamente. Por ejemplo:

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

Anterior, dentro de las declaraciones de miembros en el `N2` espacio de nombres, los miembros estáticos y los tipos anidados de `N1.A` están directamente disponibles y, por tanto, el método `N` es capaz de hacer referencia tanto la `B` y `M` los miembros de `N1.A`.

Un *using_static_directive* específicamente no importa los métodos de extensión directamente como métodos estáticos, pero hace que estén disponibles para la invocación del método de extensión ([las invocaciones de método de extensión](expressions.md#extension-method-invocations)). En el ejemplo

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
el *using_static_directive* importa el método de extensión `M` contenidos en `N1.A`, sino únicamente como un método de extensión. Por lo tanto, la primera referencia a `M` en el cuerpo de `B.N` da como resultado un error en tiempo de compilación porque ningún miembro llamado `M` están en el ámbito.

Un *using_static_directive* sólo importa los miembros y tipos declarados directamente en el tipo especificado, no a los tipos y miembros declaran en las clases base.

TODO: Ejemplo

Las ambigüedades entre varias *using_namespace_directives* y *using_static_directives* se tratan en [mediante directivas de espacio de nombres](namespaces.md#using-namespace-directives).

## <a name="namespace-members"></a>Miembros de Namespace

Un *namespace_member_declaration* sea un *namespace_declaration* ([declaraciones Namespace](namespaces.md#namespace-declarations)) o un *type_declaration* () [Declaraciones de tipo](namespaces.md#type-declarations)).

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

Una unidad de compilación o un cuerpo de espacio de nombres puede contener *namespace_member_declaration*s y dichas declaraciones contribuyen nuevos miembros al espacio de declaración subyacente del cuerpo de espacio de nombres o la unidad de compilación que contiene.

## <a name="type-declarations"></a>Declaraciones de tipos

Un *type_declaration* es un *class_declaration* ([declaraciones de clase](classes.md#class-declarations)), un *struct_declaration* ([Struct las declaraciones](structs.md#struct-declarations)), un *interface_declaration* ([declaraciones de interfaz](interfaces.md#interface-declarations)), un *enum_declaration* ([Enum las declaraciones](enums.md#enum-declarations)), o un *delegate_declaration* ([declaraciones de delegado](delegates.md#delegate-declarations)).

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

Un *type_declaration* puede ocurrir como una declaración de nivel superior en una unidad de compilación o como una declaración de miembro dentro de un espacio de nombres, clase o struct.

Cuando una declaración de tipos para un tipo `T` se produce como una declaración de nivel superior en una unidad de compilación, el nombre completo del nuevo tipo declarado es simplemente `T`. Cuando una declaración de tipos para un tipo `T` se produce dentro de un espacio de nombres, clase o struct, el nombre completo del nuevo tipo declarado es `N.T`, donde `N` es el nombre completo del espacio de nombres, clase o estructura contenedora.

Un tipo declarado dentro de una clase o struct se denomina tipo anidado ([tipos anidados](classes.md#nested-types)).

Los modificadores de acceso permitido y el acceso predeterminado para una declaración de tipo dependen del contexto en el que tiene lugar la declaración ([accesibilidad declarada](basic-concepts.md#declared-accessibility)):

*  Pueden tener tipos declarados en unidades de compilación o espacios de nombres `public` o `internal` acceso. El valor predeterminado es `internal` acceso.
*  Los tipos declarados en las clases pueden tener `public`, `protected internal`, `protected`, `internal`, o `private` acceso. El valor predeterminado es `private` acceso.
*  Los tipos declarados en estructuras pueden tener `public`, `internal`, o `private` acceso. El valor predeterminado es `private` acceso.

## <a name="namespace-alias-qualifiers"></a>Calificadores de alias Namespace

El ***calificador de alias de espacio de nombres*** `::` hace posible garantizar que las búsquedas de nombres de tipo no se ven afectadas por la introducción de nuevos tipos y miembros. El calificador de alias de espacio de nombres siempre aparece entre dos identificadores que se conoce como los identificadores de la izquierda y derecha. A diferencia de las tarifas `.` calificador, el identificador izquierdo de la `::` calificador se busca sólo como un alias de using o extern.

Un *qualified_alias_member* se define como sigue:

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

Un *qualified_alias_member* puede usarse como un *namespace_or_type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)) o como el operando izquierdo de un *member_access* ([Acceso a miembros](expressions.md#member-access)).

Un *qualified_alias_member* tiene uno de dos formas:

*  `N::I<A1, ..., Ak>`, donde `N` y `I` representan identificadores, y `<A1, ..., Ak>` es una lista de argumentos de tipo. (`K` siempre es al menos uno.)
*  `N::I`, donde `N` y `I` representan identificadores. (En este caso, `K` se considera igual a cero.)

Usa esta notación, el significado de un *qualified_alias_member* se determina como sigue:

*  Si `N` es el identificador `global`, a continuación, se busca el espacio de nombres global `I`:
   * Si el espacio de nombres global contiene un espacio de nombres denominado `I` y `K` es cero, el *qualified_alias_member* hace referencia a ese espacio de nombres.
   * En caso contrario, si el espacio de nombres global contiene un tipo no genérico denominado `I` y `K` es cero, el *qualified_alias_member* hace referencia a ese tipo.
   * En caso contrario, si el espacio de nombres global contiene un tipo denominado `I` cuya `K`  parámetros de tipo, el *qualified_alias_member* hace referencia a ese tipo construido con los argumentos de tipo especificado.
   * En caso contrario, el *qualified_alias_member* es no definido y se produce un error de tiempo de compilación.

*  En caso contrario, empezando con la declaración de espacio de nombres ([declaraciones Namespace](namespaces.md#namespace-declarations)) inmediatamente que contiene el *qualified_alias_member* (si existe), continuando con cada declaración de espacio de nombres envolvente (si existe) y termina con la unidad de compilación que contiene el *qualified_alias_member*, se evalúan los pasos siguientes hasta que se encuentra una entidad:

   * Si la unidad de compilación o de declaración de espacio de nombres contiene un *using_alias_directive* que asocia `N` con un tipo, el *qualified_alias_member* no está definido y un tiempo de compilación se produce un error.
   * En caso contrario, si la unidad de compilación o de declaración de espacio de nombres contiene un *extern_alias_directive* o *using_alias_directive* que asocia `N` con un espacio de nombres, a continuación:
     * Si el espacio de nombres asociado `N` contiene un espacio de nombres denominado `I` y `K` es cero, el *qualified_alias_member* hace referencia a ese espacio de nombres.
     * En caso contrario, si el espacio de nombres asociado `N` contiene un tipo no genérico denominado `I` y `K` es cero, el *qualified_alias_member* hace referencia a ese tipo.
     * En caso contrario, si el espacio de nombres asociado `N` contiene un tipo denominado `I` cuya `K`  parámetros de tipo, el *qualified_alias_member* hace referencia a dicho tipo construido con los argumentos de tipo especificado.
     * En caso contrario, el *qualified_alias_member* es no definido y se produce un error de tiempo de compilación.
*  En caso contrario, el *qualified_alias_member* es no definido y se produce un error de tiempo de compilación.

Tenga en cuenta que uso el calificador de alias de espacio de nombres con un alias que hace referencia a un tipo produce un error de tiempo de compilación. Observe también que si el identificador `N` es `global`, a continuación, se realiza una búsqueda en el espacio de nombres global, incluso si hay un alias using que asocia `global` con un tipo o espacio de nombres.

### <a name="uniqueness-of-aliases"></a>Unicidad de los alias

Cada cuerpo de unidad y el espacio de nombres de compilación tiene un espacio de declaración independiente para los alias externos y el uso de alias. Por lo tanto, aunque el nombre de alias using o un alias externo debe ser único dentro del conjunto de alias externos y el uso de alias declarados en el cuerpo de espacio de nombres o la unidad de compilación contenedor inmediato, un alias puede tener el mismo nombre que un tipo o espacio de nombres siempre y cuando t solo se usa con el `::` calificador.

En el ejemplo
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
el nombre `A` tiene dos significados posibles en el cuerpo de espacio de nombres de segundo, porque la clase `A` y el uso de alias `A` están en el ámbito. Para ello, el uso de `A` en el nombre completo `A.Stream` es ambigua y produce un error de tiempo de compilación que se produzca. Sin embargo, usar de `A` con el `::` calificador no es un error porque `A` se busca sólo como un alias de espacio de nombres.
