---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703966"
---
# <a name="enums"></a><span data-ttu-id="91c66-101">Enumeraciones</span><span class="sxs-lookup"><span data-stu-id="91c66-101">Enums</span></span>

<span data-ttu-id="91c66-102">Un ***tipo de enumeración*** es un tipo de valor distinto ([tipos de valor](types.md#value-types)) que declara un conjunto de constantes con nombre.</span><span class="sxs-lookup"><span data-stu-id="91c66-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="91c66-103">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="91c66-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="91c66-104">declara un tipo de enumeración denominado `Color` con miembros `Red`, `Green`y `Blue`.</span><span class="sxs-lookup"><span data-stu-id="91c66-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="91c66-105">Declaraciones de enumeración</span><span class="sxs-lookup"><span data-stu-id="91c66-105">Enum declarations</span></span>

<span data-ttu-id="91c66-106">Una declaración de enumeración declara un nuevo tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="91c66-107">Una declaración de enumeración comienza con la palabra clave `enum`, y define el nombre, la accesibilidad, el tipo subyacente y los miembros de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="91c66-108">Cada tipo de enumeración tiene un tipo entero correspondiente denominado el tipo ***subyacente*** del tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="91c66-109">Este tipo subyacente debe ser capaz de representar todos los valores de enumerador definidos en la enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="91c66-110">Una declaración de enumeración puede declarar explícitamente un tipo subyacente de `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` o `ulong`.</span><span class="sxs-lookup"><span data-stu-id="91c66-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="91c66-111">Tenga en cuenta que `char` no se puede usar como un tipo subyacente.</span><span class="sxs-lookup"><span data-stu-id="91c66-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="91c66-112">Una declaración de enumeración que no declara explícitamente un tipo subyacente tiene un tipo subyacente de `int`.</span><span class="sxs-lookup"><span data-stu-id="91c66-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="91c66-113">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="91c66-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="91c66-114">declara una enumeración con un tipo subyacente de `long`.</span><span class="sxs-lookup"><span data-stu-id="91c66-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="91c66-115">Un programador puede optar por usar un tipo subyacente de `long`, como en el ejemplo, para habilitar el uso de valores que se encuentran en el intervalo de `long` pero no en el intervalo de `int`, o para conservar esta opción para el futuro.</span><span class="sxs-lookup"><span data-stu-id="91c66-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="91c66-116">Modificadores de enumeración</span><span class="sxs-lookup"><span data-stu-id="91c66-116">Enum modifiers</span></span>

<span data-ttu-id="91c66-117">Un *enum_declaration* puede incluir opcionalmente una secuencia de modificadores de enumeración:</span><span class="sxs-lookup"><span data-stu-id="91c66-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="91c66-118">Es un error en tiempo de compilación que el mismo modificador aparezca varias veces en una declaración de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="91c66-119">Los modificadores de una declaración de enumeración tienen el mismo significado que los de una declaración de clase ([modificadores de clase](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="91c66-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="91c66-120">Sin embargo, tenga en cuenta que no se permiten los modificadores `abstract` y `sealed` en una declaración de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="91c66-121">Las enumeraciones no pueden ser abstractas y no permiten la derivación.</span><span class="sxs-lookup"><span data-stu-id="91c66-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="91c66-122">Enumerar miembros</span><span class="sxs-lookup"><span data-stu-id="91c66-122">Enum members</span></span>

<span data-ttu-id="91c66-123">El cuerpo de una declaración de tipo enum define cero o más miembros de enumeración, que son las constantes con nombre del tipo enum.</span><span class="sxs-lookup"><span data-stu-id="91c66-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="91c66-124">Dos miembros de enumeración no pueden tener el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="91c66-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="91c66-125">Cada miembro de la enumeración tiene un valor constante asociado.</span><span class="sxs-lookup"><span data-stu-id="91c66-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="91c66-126">El tipo de este valor es el tipo subyacente de la enumeración contenedora.</span><span class="sxs-lookup"><span data-stu-id="91c66-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="91c66-127">El valor constante para cada miembro de la enumeración debe estar en el intervalo del tipo subyacente de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="91c66-128">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="91c66-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="91c66-129">produce un error en tiempo de compilación porque los valores constantes `-1`, `-2`y `-3` no están en el intervalo del `uint`de tipo entero subyacente.</span><span class="sxs-lookup"><span data-stu-id="91c66-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="91c66-130">Varios miembros de enumeración pueden compartir el mismo valor asociado.</span><span class="sxs-lookup"><span data-stu-id="91c66-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="91c66-131">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="91c66-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="91c66-132">muestra una enumeración en la que dos miembros de enumeración (`Blue` y `Max`) tienen el mismo valor asociado.</span><span class="sxs-lookup"><span data-stu-id="91c66-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="91c66-133">El valor asociado de un miembro de enumeración se asigna de forma implícita o explícita.</span><span class="sxs-lookup"><span data-stu-id="91c66-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="91c66-134">Si la declaración del miembro de enumeración tiene un inicializador *constant_expression* , el valor de esa expresión constante, convertido implícitamente en el tipo subyacente de la enumeración, es el valor asociado del miembro de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="91c66-135">Si la declaración del miembro de enumeración no tiene inicializador, su valor asociado se establece implícitamente, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="91c66-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="91c66-136">Si el miembro de la enumeración es el primer miembro de la enumeración declarado en el tipo de enumeración, su valor asociado es cero.</span><span class="sxs-lookup"><span data-stu-id="91c66-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="91c66-137">De lo contrario, el valor asociado del miembro de enumeración se obtiene aumentando el valor asociado del miembro de enumeración anterior textualmente en uno.</span><span class="sxs-lookup"><span data-stu-id="91c66-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="91c66-138">Este aumento de valor debe estar dentro del intervalo de valores que se puede representar mediante el tipo subyacente; de lo contrario, se producirá un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="91c66-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="91c66-139">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="91c66-139">The example</span></span>

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

<span data-ttu-id="91c66-140">imprime los nombres de miembro de enumeración y sus valores asociados.</span><span class="sxs-lookup"><span data-stu-id="91c66-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="91c66-141">El resultado es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="91c66-141">The output is:</span></span>

```console
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="91c66-142">por los siguientes motivos:</span><span class="sxs-lookup"><span data-stu-id="91c66-142">for the following reasons:</span></span>

*  <span data-ttu-id="91c66-143">al miembro de enumeración `Red` se le asigna automáticamente el valor cero (ya que no tiene inicializador y es el primer miembro de la enumeración);</span><span class="sxs-lookup"><span data-stu-id="91c66-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="91c66-144">al miembro de enumeración `Green` se le asigna explícitamente el valor `10`;</span><span class="sxs-lookup"><span data-stu-id="91c66-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="91c66-145">y al miembro de enumeración `Blue` se le asigna automáticamente el valor uno mayor que el miembro que lo precede textualmente.</span><span class="sxs-lookup"><span data-stu-id="91c66-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="91c66-146">El valor asociado de un miembro de enumeración no puede, directa ni indirectamente, usar el valor de su propio miembro de enumeración asociado.</span><span class="sxs-lookup"><span data-stu-id="91c66-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="91c66-147">Aparte de esta restricción de circularidad, los inicializadores de miembros de enumeración pueden hacer referencia libremente a otros inicializadores de miembro de enumeración, independientemente de su posición textual.</span><span class="sxs-lookup"><span data-stu-id="91c66-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="91c66-148">Dentro de un inicializador de miembro de enumeración, los valores de otros miembros de enumeración siempre se tratan como si tuvieran el tipo de su tipo subyacente, por lo que no es necesario realizar conversiones cuando se hace referencia a otros miembros de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="91c66-149">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="91c66-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="91c66-150">produce un error en tiempo de compilación porque las declaraciones de `A` y `B` son circulares.</span><span class="sxs-lookup"><span data-stu-id="91c66-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="91c66-151">`A` depende de `B` explícitamente y `B` depende de `A` implícitamente.</span><span class="sxs-lookup"><span data-stu-id="91c66-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="91c66-152">Los miembros de enumeración tienen un nombre y tienen el ámbito de una manera exactamente análoga a los campos de las clases.</span><span class="sxs-lookup"><span data-stu-id="91c66-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="91c66-153">El ámbito de un miembro de enumeración es el cuerpo de su tipo de enumeración contenedor.</span><span class="sxs-lookup"><span data-stu-id="91c66-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="91c66-154">Dentro de ese ámbito, se puede hacer referencia a los miembros de enumeración por su nombre simple.</span><span class="sxs-lookup"><span data-stu-id="91c66-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="91c66-155">Desde el resto del código, el nombre de un miembro de enumeración debe estar calificado con el nombre de su tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="91c66-156">Los miembros de enumeración no tienen ninguna accesibilidad declarada: se puede acceder a un miembro de enumeración si se puede tener acceso a su tipo de enumeración contenedor.</span><span class="sxs-lookup"><span data-stu-id="91c66-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="91c66-157">Tipo System. Enum</span><span class="sxs-lookup"><span data-stu-id="91c66-157">The System.Enum type</span></span>

<span data-ttu-id="91c66-158">El tipo `System.Enum` es la clase base abstracta de todos los tipos de enumeración (es distinto y diferente del tipo subyacente del tipo de enumeración) y los miembros heredados de `System.Enum` están disponibles en cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="91c66-159">Existe una conversión boxing ([conversiones boxing](types.md#boxing-conversions)) de cualquier tipo de enumeración a `System.Enum`y existe una conversión unboxing ([conversiones unboxing](types.md#unboxing-conversions)) de `System.Enum` a cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="91c66-160">Tenga en cuenta que `System.Enum` no es en sí misma un *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="91c66-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="91c66-161">En su lugar, es una *class_type* de la que se derivan todos los *enum_type*s.</span><span class="sxs-lookup"><span data-stu-id="91c66-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="91c66-162">El tipo `System.Enum` hereda del tipo `System.ValueType` ([el tipo System. ValueType](types.md#the-systemvaluetype-type)), que, a su vez, hereda del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="91c66-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="91c66-163">En tiempo de ejecución, un valor de tipo `System.Enum` puede ser `null` o una referencia a un valor de conversión boxing de cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="91c66-164">Valores de enumeración y operaciones</span><span class="sxs-lookup"><span data-stu-id="91c66-164">Enum values and operations</span></span>

<span data-ttu-id="91c66-165">Cada tipo de enumeración define un tipo distinto; se requiere una conversión de enumeración explícita ([conversiones explícitas de enumeración](conversions.md#explicit-enumeration-conversions)) para realizar la conversión entre un tipo de enumeración y un tipo entero, o entre dos tipos de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="91c66-166">El conjunto de valores que puede tomar un tipo de enumeración no está limitado por sus miembros de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="91c66-167">En concreto, cualquier valor del tipo subyacente de una enumeración se puede convertir al tipo de enumeración y es un valor válido distinto de ese tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="91c66-168">Los miembros de enumeración tienen el tipo de su tipo de enumeración contenedor (excepto en otros inicializadores de miembro de enumeración: vea [miembros de enumeración](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="91c66-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="91c66-169">El valor de un miembro de enumeración declarado en el tipo de enumeración `E` con el valor asociado `v` es `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="91c66-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="91c66-170">Los operadores siguientes se pueden usar en valores de tipos de enumeración: `==`, `!=`, `<`, `>`, `<=`, `>=` ([operadores de comparación de enumeraciones](expressions.md#enumeration-comparison-operators)), binary `+` ([operador de suma](expressions.md#addition-operator)), binario `-` (operador de[sustracción](expressions.md#subtraction-operator)), `^``&`, `|` ([operadores lógicos de enumeración](expressions.md#enumeration-logical-operators)), `~` ([operador de complemento bit a bit](expressions.md#bitwise-complement-operator)), [Operadores de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) [postfijo y operadores de incremento y decremento prefijos](expressions.md#prefix-increment-and-decrement-operators)).`++``--` </span><span class="sxs-lookup"><span data-stu-id="91c66-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="91c66-171">Cada tipo de enumeración se deriva automáticamente de la clase `System.Enum` (que, a su vez, deriva de `System.ValueType` y `object`).</span><span class="sxs-lookup"><span data-stu-id="91c66-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="91c66-172">Por lo tanto, los métodos y las propiedades heredados de esta clase se pueden utilizar en los valores de un tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="91c66-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
