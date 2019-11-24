---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703966"
---
# <a name="enums"></a>Enumeraciones

Un ***tipo de enumeración*** es un tipo de valor distinto ([tipos de valor](types.md#value-types)) que declara un conjunto de constantes con nombre.

El ejemplo

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

declara un tipo de enumeración denominado `Color` con miembros `Red`, `Green`y `Blue`.

## <a name="enum-declarations"></a>Declaraciones de enumeración

Una declaración de enumeración declara un nuevo tipo de enumeración. Una declaración de enumeración comienza con la palabra clave `enum`, y define el nombre, la accesibilidad, el tipo subyacente y los miembros de la enumeración.

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

Cada tipo de enumeración tiene un tipo entero correspondiente denominado el tipo ***subyacente*** del tipo de enumeración. Este tipo subyacente debe ser capaz de representar todos los valores de enumerador definidos en la enumeración. Una declaración de enumeración puede declarar explícitamente un tipo subyacente de `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` o `ulong`. Tenga en cuenta que `char` no se puede usar como un tipo subyacente. Una declaración de enumeración que no declara explícitamente un tipo subyacente tiene un tipo subyacente de `int`.

El ejemplo

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

declara una enumeración con un tipo subyacente de `long`. Un programador puede optar por usar un tipo subyacente de `long`, como en el ejemplo, para habilitar el uso de valores que se encuentran en el intervalo de `long` pero no en el intervalo de `int`, o para conservar esta opción para el futuro.

## <a name="enum-modifiers"></a>Modificadores de enumeración

Un *enum_declaration* puede incluir opcionalmente una secuencia de modificadores de enumeración:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Es un error en tiempo de compilación que el mismo modificador aparezca varias veces en una declaración de enumeración.

Los modificadores de una declaración de enumeración tienen el mismo significado que los de una declaración de clase ([modificadores de clase](classes.md#class-modifiers)). Sin embargo, tenga en cuenta que no se permiten los modificadores `abstract` y `sealed` en una declaración de enumeración. Las enumeraciones no pueden ser abstractas y no permiten la derivación.

## <a name="enum-members"></a>Enumerar miembros

El cuerpo de una declaración de tipo enum define cero o más miembros de enumeración, que son las constantes con nombre del tipo enum. Dos miembros de enumeración no pueden tener el mismo nombre.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Cada miembro de la enumeración tiene un valor constante asociado. El tipo de este valor es el tipo subyacente de la enumeración contenedora. El valor constante para cada miembro de la enumeración debe estar en el intervalo del tipo subyacente de la enumeración. El ejemplo

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

produce un error en tiempo de compilación porque los valores constantes `-1`, `-2`y `-3` no están en el intervalo del `uint`de tipo entero subyacente.

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

muestra una enumeración en la que dos miembros de enumeración (`Blue` y `Max`) tienen el mismo valor asociado.

El valor asociado de un miembro de enumeración se asigna de forma implícita o explícita. Si la declaración del miembro de enumeración tiene un inicializador *constant_expression* , el valor de esa expresión constante, convertido implícitamente en el tipo subyacente de la enumeración, es el valor asociado del miembro de enumeración. Si la declaración del miembro de enumeración no tiene inicializador, su valor asociado se establece implícitamente, como se indica a continuación:

*  Si el miembro de la enumeración es el primer miembro de la enumeración declarado en el tipo de enumeración, su valor asociado es cero.
*  De lo contrario, el valor asociado del miembro de enumeración se obtiene aumentando el valor asociado del miembro de enumeración anterior textualmente en uno. Este aumento de valor debe estar dentro del intervalo de valores que se puede representar mediante el tipo subyacente; de lo contrario, se producirá un error en tiempo de compilación.

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

imprime los nombres de miembro de enumeración y sus valores asociados. El resultado es el siguiente:

```console
Red = 0
Green = 10
Blue = 11
```

por los siguientes motivos:

*  al miembro de enumeración `Red` se le asigna automáticamente el valor cero (ya que no tiene inicializador y es el primer miembro de la enumeración);
*  al miembro de enumeración `Green` se le asigna explícitamente el valor `10`;
*  y al miembro de enumeración `Blue` se le asigna automáticamente el valor uno mayor que el miembro que lo precede textualmente.

El valor asociado de un miembro de enumeración no puede, directa ni indirectamente, usar el valor de su propio miembro de enumeración asociado. Aparte de esta restricción de circularidad, los inicializadores de miembros de enumeración pueden hacer referencia libremente a otros inicializadores de miembro de enumeración, independientemente de su posición textual. Dentro de un inicializador de miembro de enumeración, los valores de otros miembros de enumeración siempre se tratan como si tuvieran el tipo de su tipo subyacente, por lo que no es necesario realizar conversiones cuando se hace referencia a otros miembros de enumeración.

El ejemplo

```csharp
enum Circular
{
    A = B,
    B
}
```

produce un error en tiempo de compilación porque las declaraciones de `A` y `B` son circulares. `A` depende de `B` explícitamente y `B` depende de `A` implícitamente.

Los miembros de enumeración tienen un nombre y tienen el ámbito de una manera exactamente análoga a los campos de las clases. El ámbito de un miembro de enumeración es el cuerpo de su tipo de enumeración contenedor. Dentro de ese ámbito, se puede hacer referencia a los miembros de enumeración por su nombre simple. Desde el resto del código, el nombre de un miembro de enumeración debe estar calificado con el nombre de su tipo de enumeración. Los miembros de enumeración no tienen ninguna accesibilidad declarada: se puede acceder a un miembro de enumeración si se puede tener acceso a su tipo de enumeración contenedor.

## <a name="the-systemenum-type"></a>Tipo System. Enum

El tipo `System.Enum` es la clase base abstracta de todos los tipos de enumeración (es distinto y diferente del tipo subyacente del tipo de enumeración) y los miembros heredados de `System.Enum` están disponibles en cualquier tipo de enumeración. Existe una conversión boxing ([conversiones boxing](types.md#boxing-conversions)) de cualquier tipo de enumeración a `System.Enum`y existe una conversión unboxing ([conversiones unboxing](types.md#unboxing-conversions)) de `System.Enum` a cualquier tipo de enumeración.

Tenga en cuenta que `System.Enum` no es en sí misma un *enum_type*. En su lugar, es una *class_type* de la que se derivan todos los *enum_type*s. El tipo `System.Enum` hereda del tipo `System.ValueType` ([el tipo System. ValueType](types.md#the-systemvaluetype-type)), que, a su vez, hereda del tipo `object`. En tiempo de ejecución, un valor de tipo `System.Enum` puede ser `null` o una referencia a un valor de conversión boxing de cualquier tipo de enumeración.

## <a name="enum-values-and-operations"></a>Valores de enumeración y operaciones

Cada tipo de enumeración define un tipo distinto; se requiere una conversión de enumeración explícita ([conversiones explícitas de enumeración](conversions.md#explicit-enumeration-conversions)) para realizar la conversión entre un tipo de enumeración y un tipo entero, o entre dos tipos de enumeración. El conjunto de valores que puede tomar un tipo de enumeración no está limitado por sus miembros de enumeración. En concreto, cualquier valor del tipo subyacente de una enumeración se puede convertir al tipo de enumeración y es un valor válido distinto de ese tipo de enumeración.

Los miembros de enumeración tienen el tipo de su tipo de enumeración contenedor (excepto en otros inicializadores de miembro de enumeración: vea [miembros de enumeración](enums.md#enum-members)). El valor de un miembro de enumeración declarado en el tipo de enumeración `E` con el valor asociado `v` es `(E)v`.

Los operadores siguientes se pueden usar en valores de tipos de enumeración: `==`, `!=`, `<`, `>`, `<=`, `>=` ([operadores de comparación de enumeraciones](expressions.md#enumeration-comparison-operators)), binary `+` ([operador de suma](expressions.md#addition-operator)), binario `-` (operador de[sustracción](expressions.md#subtraction-operator)), `^``&`, `|` ([operadores lógicos de enumeración](expressions.md#enumeration-logical-operators)), `~` ([operador de complemento bit a bit](expressions.md#bitwise-complement-operator)), [Operadores de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) [postfijo y operadores de incremento y decremento prefijos](expressions.md#prefix-increment-and-decrement-operators)).`++``--` 

Cada tipo de enumeración se deriva automáticamente de la clase `System.Enum` (que, a su vez, deriva de `System.ValueType` y `object`). Por lo tanto, los métodos y las propiedades heredados de esta clase se pueden utilizar en los valores de un tipo de enumeración.
