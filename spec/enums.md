# <a name="enums"></a>Enumeraciones

Un ***tipo enum*** es un tipo de valor distinto ([los tipos de valor](types.md#value-types)) que declara un conjunto de constantes con nombre.

El ejemplo

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

declara un tipo de enumeración denominado `Color` con miembros `Red`, `Green`, y `Blue`.

## <a name="enum-declarations"></a>Declaraciones de enumeración

Una declaración de enumeración declara un nuevo tipo de enumeración. Una declaración de enumeración comienza con la palabra clave `enum`y define el nombre, accesibilidad, tipo subyacente y los miembros de la enumeración.

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

Cada tipo de enumeración tiene un tipo integral correspondiente denominado el ***tipo subyacente*** del tipo enum. Este tipo subyacente debe ser capaz de representar todos los valores del enumerador definidos en la enumeración. Una declaración de enumeración puede declarar explícitamente un tipo subyacente de `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` o `ulong`. Tenga en cuenta que `char` no puede usarse como un tipo subyacente. Una declaración de enumeración que no declara explícitamente un tipo subyacente tiene un tipo subyacente de `int`.

El ejemplo

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

declara una enumeración con un tipo subyacente de `long`. Un desarrollador puede optar por usar un tipo subyacente de `long`, como en el ejemplo, para habilitar el uso de valores que se encuentran en el intervalo de `long` , pero no en el intervalo de `int`, o para conservar esta opción para el futuro.

## <a name="enum-modifiers"></a>Modificadores de enumeración

Un *enum_declaration* también puede incluir una secuencia de modificadores de enumeración:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Es un error en tiempo de compilación para el mismo modificador aparezca varias veces en una declaración de enumeración.

Los modificadores de una declaración de enumeración tienen el mismo significado que los de una declaración de clase ([modificadores de clase](classes.md#class-modifiers)). Sin embargo, tenga en cuenta que el `abstract` y `sealed` modificadores no están permitidos en una declaración de enumeración. Las enumeraciones no pueden ser abstractas y no permite la derivación.

## <a name="enum-members"></a>Miembros de enumeración

El cuerpo de una declaración de tipo de enumeración define a cero o más miembros de enumeración, que son las constantes con nombre del tipo enum. No hay dos miembros de enumeración pueden tener el mismo nombre.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Cada miembro de enumeración tiene un valor constante asociado. El tipo de este valor es el tipo subyacente de la enumeración contenedora. El valor constante para cada miembro de enumeración debe ser en el intervalo del tipo subyacente para la enumeración. El ejemplo

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

se produce un error en tiempo de compilación porque los valores constantes `-1`, `-2`, y `-3` no están en el intervalo del tipo entero subyacente `uint`.

Varios miembros de enumeración pueden compartir el mismo valor asociado. El ejemplo

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

muestra una enumeración en la que dos miembros-- `Blue` y `Max` --tienen el mismo valor asociado.

El valor asociado de un miembro de enumeración se asigna de forma implícita o explícita. Si la declaración del miembro de enumeración tiene un *constant_expression* inicializador, el valor de esa expresión constante, se convierte implícitamente al tipo subyacente de la enumeración, es el valor del miembro de enumeración asociado. Si la declaración del miembro de enumeración no tiene inicializador, su valor asociado se establece implícitamente, como sigue:

*  Si el miembro de enumeración es el primer miembro de enumeración declarado en el tipo enum, su valor asociado es cero.
*  En caso contrario, se obtiene el valor del miembro de enumeración asociado aumentando el valor del miembro de enumeración precedente asociado en uno. Este valor aumentado debe estar dentro del intervalo de valores que se puede representar el tipo subyacente, de lo contrario se produce un error en tiempo de compilación.

El ejemplo

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

Imprime los nombres de miembro de enumeración y sus valores asociados. El resultado es el siguiente:

```
Red = 0
Green = 10
Blue = 11
```

por las razones siguientes:

*  el miembro de enumeración `Red` se asigna automáticamente el valor cero (puesto que no tiene inicializador y es el primer miembro de enumeración);
*  el miembro de enumeración `Green` explícitamente se le asigna el valor `10`;
*  y el miembro de enumeración `Blue` se asigna automáticamente el valor de una unidad mayor que el miembro que le precede.

El valor asociado de un miembro de enumeración no puede, directa o indirectamente, use el valor de su propio miembro de enumeración asociado. Aparte de esta restricción circularidad, inicializadores de miembro de enumeración pueden llamar libremente a otros inicializadores de miembro de enumeración, independientemente de su posición textual. Dentro de un inicializador de miembro de enumeración, los valores de otros miembros de enumeración siempre se tratan como si tuviera el tipo de su tipo subyacente, por lo que las conversiones no son necesarias cuando se hace referencia a otros miembros de enumeración.

El ejemplo

```csharp
enum Circular
{
    A = B,
    B
}
```

se produce un error en tiempo de compilación porque las declaraciones de `A` y `B` son circulares. `A` depende de `B` explícitamente, y `B` depende `A` implícitamente.

Los miembros de enumeración son denominados y ámbito de una manera exactamente análoga a los campos dentro de las clases. El ámbito de un miembro de enumeración es el cuerpo de su tipo de enumeración que contiene. Dentro de ese ámbito, pueden hacer referencia a miembros de enumeración por su nombre simple. Desde otro de código, el nombre de un miembro de enumeración debe calificarse con el nombre de su tipo enum. Los miembros de enumeración no tiene ninguna accesibilidad declarada: un miembro de enumeración es accesible si su tipo de enumeración que contiene es accesible.

## <a name="the-systemenum-type"></a>Tipo System.Enum

El tipo `System.Enum` es la clase base abstracta de todos los tipos de enumeración (que es distinto del tipo subyacente del tipo enum) y los miembros heredados de `System.Enum` están disponibles en cualquier tipo de enumeración. Una conversión boxing ([conversiones Boxing](types.md#boxing-conversions)) existe desde cualquier tipo de enumeración para `System.Enum`y una conversión unboxing ([Conversiones Unboxing](types.md#unboxing-conversions)) existe desde `System.Enum` a cualquier tipo de enumeración.

Tenga en cuenta que `System.Enum` no es un *enum_type*. Se trata de un *class_type* de los cuales *enum_type*se derivan. El tipo `System.Enum` hereda del tipo `System.ValueType` ([System.ValueType el tipo](types.md#the-systemvaluetype-type)), que, a su vez, hereda del tipo `object`. En tiempo de ejecución, un valor de tipo `System.Enum` puede ser `null` o una referencia a un valor con conversión boxing de cualquier tipo de enumeración.

## <a name="enum-values-and-operations"></a>Las operaciones y los valores de enumeración

Cada tipo de enumeración define un tipo distinto; una conversión explícita de enumeración ([conversiones explícitas de enumeración](conversions.md#explicit-enumeration-conversions)) es necesaria para convertir entre un tipo de enumeración y un tipo entero, o entre dos tipos de enumeración. El conjunto de valores que puede tomar un tipo de enumeración no está limitado por sus miembros de enumeración. En concreto, cualquier valor del tipo subyacente de una enumeración se puede convertir al tipo enum y es un valor válido distinto de ese tipo de enumeración.

Miembros de enumeración tienen el tipo de su tipo enum contenedor (excepto dentro de otros inicializadores de miembro de enumeración: vea [miembros de enumeración](enums.md#enum-members)). El valor de un miembro de enumeración declarado en el tipo de enumeración `E` con el valor asociado `v` es `(E)v`.

Los siguientes operadores pueden utilizarse en los valores de los tipos de enumeración: `==`, `!=`, `<`, `>`, `<=`, `>=` ([operadores de comparación de la enumeración](expressions.md#enumeration-comparison-operators)), binario `+` ([Operador de suma](expressions.md#addition-operator)) y binario `-` ([operador de resta](expressions.md#subtraction-operator)), `^`, `&`, `|` ([lógico (enumeración) operadores](expressions.md#enumeration-logical-operators)), `~` ([operador de complemento bit a bit](expressions.md#bitwise-complement-operator)), `++` y `--` ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) y [ Prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)).

Cada tipo de enumeración se deriva automáticamente de la clase `System.Enum` (que, a su vez, deriva `System.ValueType` y `object`). Por lo tanto, los métodos heredados y propiedades de esta clase pueden utilizarse en los valores de un tipo enum.
