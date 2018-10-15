# <a name="enums"></a><span data-ttu-id="e0a4c-101">Enumeraciones</span><span class="sxs-lookup"><span data-stu-id="e0a4c-101">Enums</span></span>

<span data-ttu-id="e0a4c-102">Un ***tipo enum*** es un tipo de valor distinto ([los tipos de valor](types.md#value-types)) que declara un conjunto de constantes con nombre.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-102">An ***enum type*** is a distinct value type ([Value types](types.md#value-types)) that declares a set of named constants.</span></span>

<span data-ttu-id="e0a4c-103">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="e0a4c-103">The example</span></span>

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="e0a4c-104">declara un tipo de enumeración denominado `Color` con miembros `Red`, `Green`, y `Blue`.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-104">declares an enum type named `Color` with members `Red`, `Green`, and `Blue`.</span></span>

## <a name="enum-declarations"></a><span data-ttu-id="e0a4c-105">Declaraciones de enumeración</span><span class="sxs-lookup"><span data-stu-id="e0a4c-105">Enum declarations</span></span>

<span data-ttu-id="e0a4c-106">Una declaración de enumeración declara un nuevo tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-106">An enum declaration declares a new enum type.</span></span> <span data-ttu-id="e0a4c-107">Una declaración de enumeración comienza con la palabra clave `enum`y define el nombre, accesibilidad, tipo subyacente y los miembros de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-107">An enum declaration begins with the keyword `enum`, and defines the name, accessibility, underlying type, and members of the enum.</span></span>

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

<span data-ttu-id="e0a4c-108">Cada tipo de enumeración tiene un tipo integral correspondiente denominado el ***tipo subyacente*** del tipo enum.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-108">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="e0a4c-109">Este tipo subyacente debe ser capaz de representar todos los valores del enumerador definidos en la enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-109">This underlying type must be able to represent all the enumerator values defined in the enumeration.</span></span> <span data-ttu-id="e0a4c-110">Una declaración de enumeración puede declarar explícitamente un tipo subyacente de `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` o `ulong`.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-110">An enum declaration may explicitly declare an underlying type of `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` or `ulong`.</span></span> <span data-ttu-id="e0a4c-111">Tenga en cuenta que `char` no puede usarse como un tipo subyacente.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-111">Note that `char` cannot be used as an underlying type.</span></span> <span data-ttu-id="e0a4c-112">Una declaración de enumeración que no declara explícitamente un tipo subyacente tiene un tipo subyacente de `int`.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-112">An enum declaration that does not explicitly declare an underlying type has an underlying type of `int`.</span></span>

<span data-ttu-id="e0a4c-113">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="e0a4c-113">The example</span></span>

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

<span data-ttu-id="e0a4c-114">declara una enumeración con un tipo subyacente de `long`.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-114">declares an enum with an underlying type of `long`.</span></span> <span data-ttu-id="e0a4c-115">Un desarrollador puede optar por usar un tipo subyacente de `long`, como en el ejemplo, para habilitar el uso de valores que se encuentran en el intervalo de `long` , pero no en el intervalo de `int`, o para conservar esta opción para el futuro.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-115">A developer might choose to use an underlying type of `long`, as in the example, to enable the use of values that are in the range of `long` but not in the range of `int`, or to preserve this option for the future.</span></span>

## <a name="enum-modifiers"></a><span data-ttu-id="e0a4c-116">Modificadores de enumeración</span><span class="sxs-lookup"><span data-stu-id="e0a4c-116">Enum modifiers</span></span>

<span data-ttu-id="e0a4c-117">Un *enum_declaration* también puede incluir una secuencia de modificadores de enumeración:</span><span class="sxs-lookup"><span data-stu-id="e0a4c-117">An *enum_declaration* may optionally include a sequence of enum modifiers:</span></span>

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

<span data-ttu-id="e0a4c-118">Es un error en tiempo de compilación para el mismo modificador aparezca varias veces en una declaración de enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-118">It is a compile-time error for the same modifier to appear multiple times in an enum declaration.</span></span>

<span data-ttu-id="e0a4c-119">Los modificadores de una declaración de enumeración tienen el mismo significado que los de una declaración de clase ([modificadores de clase](classes.md#class-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="e0a4c-119">The modifiers of an enum declaration have the same meaning as those of a class declaration ([Class modifiers](classes.md#class-modifiers)).</span></span> <span data-ttu-id="e0a4c-120">Sin embargo, tenga en cuenta que el `abstract` y `sealed` modificadores no están permitidos en una declaración de enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-120">Note, however, that the `abstract` and `sealed` modifiers are not permitted in an enum declaration.</span></span> <span data-ttu-id="e0a4c-121">Las enumeraciones no pueden ser abstractas y no permite la derivación.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-121">Enums cannot be abstract and do not permit derivation.</span></span>

## <a name="enum-members"></a><span data-ttu-id="e0a4c-122">Miembros de enumeración</span><span class="sxs-lookup"><span data-stu-id="e0a4c-122">Enum members</span></span>

<span data-ttu-id="e0a4c-123">El cuerpo de una declaración de tipo de enumeración define a cero o más miembros de enumeración, que son las constantes con nombre del tipo enum.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-123">The body of an enum type declaration defines zero or more enum members, which are the named constants of the enum type.</span></span> <span data-ttu-id="e0a4c-124">No hay dos miembros de enumeración pueden tener el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-124">No two enum members can have the same name.</span></span>

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

<span data-ttu-id="e0a4c-125">Cada miembro de enumeración tiene un valor constante asociado.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-125">Each enum member has an associated constant value.</span></span> <span data-ttu-id="e0a4c-126">El tipo de este valor es el tipo subyacente de la enumeración contenedora.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-126">The type of this value is the underlying type for the containing enum.</span></span> <span data-ttu-id="e0a4c-127">El valor constante para cada miembro de enumeración debe ser en el intervalo del tipo subyacente para la enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-127">The constant value for each enum member must be in the range of the underlying type for the enum.</span></span> <span data-ttu-id="e0a4c-128">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="e0a4c-128">The example</span></span>

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

<span data-ttu-id="e0a4c-129">se produce un error en tiempo de compilación porque los valores constantes `-1`, `-2`, y `-3` no están en el intervalo del tipo entero subyacente `uint`.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-129">results in a compile-time error because the constant values `-1`, `-2`, and `-3` are not in the range of the underlying integral type `uint`.</span></span>

<span data-ttu-id="e0a4c-130">Varios miembros de enumeración pueden compartir el mismo valor asociado.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-130">Multiple enum members may share the same associated value.</span></span> <span data-ttu-id="e0a4c-131">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="e0a4c-131">The example</span></span>

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

<span data-ttu-id="e0a4c-132">muestra una enumeración en la que dos miembros-- `Blue` y `Max` --tienen el mismo valor asociado.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-132">shows an enum in which two enum members -- `Blue` and `Max` -- have the same associated value.</span></span>

<span data-ttu-id="e0a4c-133">El valor asociado de un miembro de enumeración se asigna de forma implícita o explícita.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-133">The associated value of an enum member is assigned either implicitly or explicitly.</span></span> <span data-ttu-id="e0a4c-134">Si la declaración del miembro de enumeración tiene un *constant_expression* inicializador, el valor de esa expresión constante, se convierte implícitamente al tipo subyacente de la enumeración, es el valor del miembro de enumeración asociado.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-134">If the declaration of the enum member has a *constant_expression* initializer, the value of that constant expression, implicitly converted to the underlying type of the enum, is the associated value of the enum member.</span></span> <span data-ttu-id="e0a4c-135">Si la declaración del miembro de enumeración no tiene inicializador, su valor asociado se establece implícitamente, como sigue:</span><span class="sxs-lookup"><span data-stu-id="e0a4c-135">If the declaration of the enum member has no initializer, its associated value is set implicitly, as follows:</span></span>

*  <span data-ttu-id="e0a4c-136">Si el miembro de enumeración es el primer miembro de enumeración declarado en el tipo enum, su valor asociado es cero.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-136">If the enum member is the first enum member declared in the enum type, its associated value is zero.</span></span>
*  <span data-ttu-id="e0a4c-137">En caso contrario, se obtiene el valor del miembro de enumeración asociado aumentando el valor del miembro de enumeración precedente asociado en uno.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-137">Otherwise, the associated value of the enum member is obtained by increasing the associated value of the textually preceding enum member by one.</span></span> <span data-ttu-id="e0a4c-138">Este valor aumentado debe estar dentro del intervalo de valores que se puede representar el tipo subyacente, de lo contrario se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-138">This increased value must be within the range of values that can be represented by the underlying type, otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="e0a4c-139">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="e0a4c-139">The example</span></span>

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

<span data-ttu-id="e0a4c-140">Imprime los nombres de miembro de enumeración y sus valores asociados.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-140">prints out the enum member names and their associated values.</span></span> <span data-ttu-id="e0a4c-141">El resultado es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="e0a4c-141">The output is:</span></span>

```
Red = 0
Green = 10
Blue = 11
```

<span data-ttu-id="e0a4c-142">por las razones siguientes:</span><span class="sxs-lookup"><span data-stu-id="e0a4c-142">for the following reasons:</span></span>

*  <span data-ttu-id="e0a4c-143">el miembro de enumeración `Red` se asigna automáticamente el valor cero (puesto que no tiene inicializador y es el primer miembro de enumeración);</span><span class="sxs-lookup"><span data-stu-id="e0a4c-143">the enum member `Red` is automatically assigned the value zero (since it has no initializer and is the first enum member);</span></span>
*  <span data-ttu-id="e0a4c-144">el miembro de enumeración `Green` explícitamente se le asigna el valor `10`;</span><span class="sxs-lookup"><span data-stu-id="e0a4c-144">the enum member `Green` is explicitly given the value `10`;</span></span>
*  <span data-ttu-id="e0a4c-145">y el miembro de enumeración `Blue` se asigna automáticamente el valor de una unidad mayor que el miembro que le precede.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-145">and the enum member `Blue` is automatically assigned the value one greater than the member that textually precedes it.</span></span>

<span data-ttu-id="e0a4c-146">El valor asociado de un miembro de enumeración no puede, directa o indirectamente, use el valor de su propio miembro de enumeración asociado.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-146">The associated value of an enum member may not, directly or indirectly, use the value of its own associated enum member.</span></span> <span data-ttu-id="e0a4c-147">Aparte de esta restricción circularidad, inicializadores de miembro de enumeración pueden llamar libremente a otros inicializadores de miembro de enumeración, independientemente de su posición textual.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-147">Other than this circularity restriction, enum member initializers may freely refer to other enum member initializers, regardless of their textual position.</span></span> <span data-ttu-id="e0a4c-148">Dentro de un inicializador de miembro de enumeración, los valores de otros miembros de enumeración siempre se tratan como si tuviera el tipo de su tipo subyacente, por lo que las conversiones no son necesarias cuando se hace referencia a otros miembros de enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-148">Within an enum member initializer, values of other enum members are always treated as having the type of their underlying type, so that casts are not necessary when referring to other enum members.</span></span>

<span data-ttu-id="e0a4c-149">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="e0a4c-149">The example</span></span>

```csharp
enum Circular
{
    A = B,
    B
}
```

<span data-ttu-id="e0a4c-150">se produce un error en tiempo de compilación porque las declaraciones de `A` y `B` son circulares.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-150">results in a compile-time error because the declarations of `A` and `B` are circular.</span></span> <span data-ttu-id="e0a4c-151">`A` depende de `B` explícitamente, y `B` depende `A` implícitamente.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-151">`A` depends on `B` explicitly, and `B` depends on `A` implicitly.</span></span>

<span data-ttu-id="e0a4c-152">Los miembros de enumeración son denominados y ámbito de una manera exactamente análoga a los campos dentro de las clases.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-152">Enum members are named and scoped in a manner exactly analogous to fields within classes.</span></span> <span data-ttu-id="e0a4c-153">El ámbito de un miembro de enumeración es el cuerpo de su tipo de enumeración que contiene.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-153">The scope of an enum member is the body of its containing enum type.</span></span> <span data-ttu-id="e0a4c-154">Dentro de ese ámbito, pueden hacer referencia a miembros de enumeración por su nombre simple.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-154">Within that scope, enum members can be referred to by their simple name.</span></span> <span data-ttu-id="e0a4c-155">Desde otro de código, el nombre de un miembro de enumeración debe calificarse con el nombre de su tipo enum.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-155">From all other code, the name of an enum member must be qualified with the name of its enum type.</span></span> <span data-ttu-id="e0a4c-156">Los miembros de enumeración no tiene ninguna accesibilidad declarada: un miembro de enumeración es accesible si su tipo de enumeración que contiene es accesible.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-156">Enum members do not have any declared accessibility -- an enum member is accessible if its containing enum type is accessible.</span></span>

## <a name="the-systemenum-type"></a><span data-ttu-id="e0a4c-157">Tipo System.Enum</span><span class="sxs-lookup"><span data-stu-id="e0a4c-157">The System.Enum type</span></span>

<span data-ttu-id="e0a4c-158">El tipo `System.Enum` es la clase base abstracta de todos los tipos de enumeración (que es distinto del tipo subyacente del tipo enum) y los miembros heredados de `System.Enum` están disponibles en cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-158">The type `System.Enum` is the abstract base class of all enum types (this is distinct and different from the underlying type of the enum type), and the members inherited from `System.Enum` are available in any enum type.</span></span> <span data-ttu-id="e0a4c-159">Una conversión boxing ([conversiones Boxing](types.md#boxing-conversions)) existe desde cualquier tipo de enumeración para `System.Enum`y una conversión unboxing ([Conversiones Unboxing](types.md#unboxing-conversions)) existe desde `System.Enum` a cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-159">A boxing conversion ([Boxing conversions](types.md#boxing-conversions)) exists from any enum type to `System.Enum`, and an unboxing conversion ([Unboxing conversions](types.md#unboxing-conversions)) exists from `System.Enum` to any enum type.</span></span>

<span data-ttu-id="e0a4c-160">Tenga en cuenta que `System.Enum` no es un *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-160">Note that `System.Enum` is not itself an *enum_type*.</span></span> <span data-ttu-id="e0a4c-161">Se trata de un *class_type* de los cuales *enum_type*se derivan.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-161">Rather, it is a *class_type* from which all *enum_type*s are derived.</span></span> <span data-ttu-id="e0a4c-162">El tipo `System.Enum` hereda del tipo `System.ValueType` ([System.ValueType el tipo](types.md#the-systemvaluetype-type)), que, a su vez, hereda del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-162">The type `System.Enum` inherits from the type `System.ValueType` ([The System.ValueType type](types.md#the-systemvaluetype-type)), which, in turn, inherits from type `object`.</span></span> <span data-ttu-id="e0a4c-163">En tiempo de ejecución, un valor de tipo `System.Enum` puede ser `null` o una referencia a un valor con conversión boxing de cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-163">At run-time, a value of type `System.Enum` can be `null` or a reference to a boxed value of any enum type.</span></span>

## <a name="enum-values-and-operations"></a><span data-ttu-id="e0a4c-164">Las operaciones y los valores de enumeración</span><span class="sxs-lookup"><span data-stu-id="e0a4c-164">Enum values and operations</span></span>

<span data-ttu-id="e0a4c-165">Cada tipo de enumeración define un tipo distinto; una conversión explícita de enumeración ([conversiones explícitas de enumeración](conversions.md#explicit-enumeration-conversions)) es necesaria para convertir entre un tipo de enumeración y un tipo entero, o entre dos tipos de enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-165">Each enum type defines a distinct type; an explicit enumeration conversion ([Explicit enumeration conversions](conversions.md#explicit-enumeration-conversions)) is required to convert between an enum type and an integral type, or between two enum types.</span></span> <span data-ttu-id="e0a4c-166">El conjunto de valores que puede tomar un tipo de enumeración no está limitado por sus miembros de enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-166">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="e0a4c-167">En concreto, cualquier valor del tipo subyacente de una enumeración se puede convertir al tipo enum y es un valor válido distinto de ese tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-167">In particular, any value of the underlying type of an enum can be cast to the enum type, and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="e0a4c-168">Miembros de enumeración tienen el tipo de su tipo enum contenedor (excepto dentro de otros inicializadores de miembro de enumeración: vea [miembros de enumeración](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="e0a4c-168">Enum members have the type of their containing enum type (except within other enum member initializers: see [Enum members](enums.md#enum-members)).</span></span> <span data-ttu-id="e0a4c-169">El valor de un miembro de enumeración declarado en el tipo de enumeración `E` con el valor asociado `v` es `(E)v`.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-169">The value of an enum member declared in enum type `E` with associated value `v` is `(E)v`.</span></span>

<span data-ttu-id="e0a4c-170">Los siguientes operadores pueden utilizarse en los valores de los tipos de enumeración: `==`, `!=`, `<`, `>`, `<=`, `>=` ([operadores de comparación de la enumeración](expressions.md#enumeration-comparison-operators)), binario `+` ([Operador de suma](expressions.md#addition-operator)) y binario `-` ([operador de resta](expressions.md#subtraction-operator)), `^`, `&`, `|` ([lógico (enumeración) operadores](expressions.md#enumeration-logical-operators)), `~` ([operador de complemento bit a bit](expressions.md#bitwise-complement-operator)), `++` y `--` ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) y [ Prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="e0a4c-170">The following operators can be used on values of enum types: `==`, `!=`, `<`, `>`, `<=`, `>=` ([Enumeration comparison operators](expressions.md#enumeration-comparison-operators)), binary `+` ([Addition operator](expressions.md#addition-operator)), binary `-` ([Subtraction operator](expressions.md#subtraction-operator)), `^`, `&`, `|` ([Enumeration logical operators](expressions.md#enumeration-logical-operators)), `~` ([Bitwise complement operator](expressions.md#bitwise-complement-operator)), `++` and `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="e0a4c-171">Cada tipo de enumeración se deriva automáticamente de la clase `System.Enum` (que, a su vez, deriva `System.ValueType` y `object`).</span><span class="sxs-lookup"><span data-stu-id="e0a4c-171">Every enum type automatically derives from the class `System.Enum` (which, in turn, derives from `System.ValueType` and `object`).</span></span> <span data-ttu-id="e0a4c-172">Por lo tanto, los métodos heredados y propiedades de esta clase pueden utilizarse en los valores de un tipo enum.</span><span class="sxs-lookup"><span data-stu-id="e0a4c-172">Thus, inherited methods and properties of this class can be used on values of an enum type.</span></span>
