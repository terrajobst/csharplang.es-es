---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122946"
---
# <a name="ranges"></a><span data-ttu-id="267af-101">Intervalos</span><span class="sxs-lookup"><span data-stu-id="267af-101">Ranges</span></span>

## <a name="summary"></a><span data-ttu-id="267af-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="267af-102">Summary</span></span>

<span data-ttu-id="267af-103">Esta característica trata sobre la entrega de dos nuevos operadores que permiten construir `System.Index` y `System.Range` objetos y usarlos para indexar y segmentar colecciones en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="267af-103">This feature is about delivering two new operators that allow constructing `System.Index` and `System.Range` objects, and using them to index/slice collections at runtime.</span></span>

## <a name="overview"></a><span data-ttu-id="267af-104">Información general</span><span class="sxs-lookup"><span data-stu-id="267af-104">Overview</span></span>

### <a name="well-known-types-and-members"></a><span data-ttu-id="267af-105">Tipos y miembros conocidos</span><span class="sxs-lookup"><span data-stu-id="267af-105">Well-known types and members</span></span>

<span data-ttu-id="267af-106">Para usar las nuevas formas sintácticas para `System.Index` y `System.Range`, es posible que se necesiten nuevos tipos y miembros conocidos, en función de las formas sintácticas que se utilicen.</span><span class="sxs-lookup"><span data-stu-id="267af-106">To use the new syntactic forms for `System.Index` and `System.Range`, new well-known types and members may be necessary, depending on which syntactic forms are used.</span></span>

<span data-ttu-id="267af-107">Para usar el operador "Hat" (`^`), se requiere lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="267af-107">To use the "hat" operator (`^`), the following is required</span></span>

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

<span data-ttu-id="267af-108">Para usar el tipo de `System.Index` como argumento en un acceso de elemento de matriz, se requiere el siguiente miembro:</span><span class="sxs-lookup"><span data-stu-id="267af-108">To use the `System.Index` type as an argument in an array element access, the following member is required:</span></span>

```csharp
int System.Index.GetOffset(int length);
```

<span data-ttu-id="267af-109">La sintaxis de `..` para `System.Range` requerirá el tipo de `System.Range`, así como uno o varios de los miembros siguientes:</span><span class="sxs-lookup"><span data-stu-id="267af-109">The `..` syntax for `System.Range` will require the `System.Range` type, as well as one or more of the following members:</span></span>

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

<span data-ttu-id="267af-110">La sintaxis de `..` permite que estén ausentes, ambos o ninguno de sus argumentos.</span><span class="sxs-lookup"><span data-stu-id="267af-110">The `..` syntax allows for either, both, or none of its arguments to be absent.</span></span> <span data-ttu-id="267af-111">Independientemente del número de argumentos, el constructor de `Range` siempre es suficiente para utilizar la sintaxis de `Range`.</span><span class="sxs-lookup"><span data-stu-id="267af-111">Regardless of the number of arguments, the `Range` constructor is always sufficient for using the `Range` syntax.</span></span> <span data-ttu-id="267af-112">Sin embargo, si alguno de los demás miembros está presente y faltan uno o varios de los argumentos `..`, se puede sustituir el miembro adecuado.</span><span class="sxs-lookup"><span data-stu-id="267af-112">However, if any of the other members are present and one or more of the `..` arguments are missing, the appropriate member may be substituted.</span></span>

<span data-ttu-id="267af-113">Por último, para un valor de tipo `System.Range` que se va a usar en una expresión de acceso de elemento de matriz, debe existir el siguiente miembro:</span><span class="sxs-lookup"><span data-stu-id="267af-113">Finally, for a value of type `System.Range` to be used in an array element access expression, the following member must be present:</span></span>

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a><span data-ttu-id="267af-114">System. index</span><span class="sxs-lookup"><span data-stu-id="267af-114">System.Index</span></span>

<span data-ttu-id="267af-115">C#no tiene forma de indizar una colección desde el final, sino que la mayoría de los indizadores usan la noción "desde Start" o una expresión "length-i".</span><span class="sxs-lookup"><span data-stu-id="267af-115">C# has no way of indexing a collection from the end, but rather most indexers use the "from start" notion, or do a "length - i" expression.</span></span> <span data-ttu-id="267af-116">Se presenta una nueva expresión de índice que significa "desde el final".</span><span class="sxs-lookup"><span data-stu-id="267af-116">We introduce a new Index expression that means "from the end".</span></span> <span data-ttu-id="267af-117">La característica introducirá un nuevo operador unario de prefijo "Hat".</span><span class="sxs-lookup"><span data-stu-id="267af-117">The feature will introduce a new unary prefix "hat" operator.</span></span> <span data-ttu-id="267af-118">Su único operando debe ser convertible a `System.Int32`.</span><span class="sxs-lookup"><span data-stu-id="267af-118">Its single operand must be convertible to `System.Int32`.</span></span> <span data-ttu-id="267af-119">Se reducirá en el `System.Index` adecuado Factory Method llamada.</span><span class="sxs-lookup"><span data-stu-id="267af-119">It will be lowered into the appropriate `System.Index` factory method call.</span></span>

<span data-ttu-id="267af-120">Aumentamos la gramática de *unary_expression* con el siguiente formato de sintaxis adicional:</span><span class="sxs-lookup"><span data-stu-id="267af-120">We augment the grammar for *unary_expression* with the following additional syntax form:</span></span>

```antlr
unary_expression
    : '^' unary_expression
    ;
```

<span data-ttu-id="267af-121">Se llama al operador *index from end* .</span><span class="sxs-lookup"><span data-stu-id="267af-121">We call this the *index from end* operator.</span></span> <span data-ttu-id="267af-122">El índice predefinido de los operadores *finales* es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="267af-122">The predefined *index from end* operators are as follows:</span></span>

```csharp
    System.Index operator ^(int fromEnd);
```

<span data-ttu-id="267af-123">El comportamiento de este operador solo se define para los valores de entrada mayores o iguales que cero.</span><span class="sxs-lookup"><span data-stu-id="267af-123">The behavior of this operator is only defined for input values greater than or equal to zero.</span></span>

<span data-ttu-id="267af-124">Ejemplos:</span><span class="sxs-lookup"><span data-stu-id="267af-124">Examples:</span></span>

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a><span data-ttu-id="267af-125">System. Range</span><span class="sxs-lookup"><span data-stu-id="267af-125">System.Range</span></span>

<span data-ttu-id="267af-126">C#no tiene ninguna forma sintáctica de tener acceso a "Ranges" o "slices" de las colecciones.</span><span class="sxs-lookup"><span data-stu-id="267af-126">C# has no syntactic way to access "ranges" or "slices" of collections.</span></span> <span data-ttu-id="267af-127">Normalmente, los usuarios se ven obligados a implementar estructuras complejas para filtrar o operar en segmentos de memoria, o bien recurrir a métodos LINQ como `list.Skip(5).Take(2)`.</span><span class="sxs-lookup"><span data-stu-id="267af-127">Usually users are forced to implement complex structures to filter/operate on slices of memory, or resort to LINQ methods like `list.Skip(5).Take(2)`.</span></span> <span data-ttu-id="267af-128">Con la adición de `System.Span<T>` y otros tipos similares, es más importante que este tipo de operación se admita en un nivel más profundo en el lenguaje o en tiempo de ejecución, y que la interfaz sea unificada.</span><span class="sxs-lookup"><span data-stu-id="267af-128">With the addition of `System.Span<T>` and other similar types, it becomes more important to have this kind of operation supported on a deeper level in the language/runtime, and have the interface unified.</span></span>

<span data-ttu-id="267af-129">El lenguaje introducirá un nuevo operador de intervalo `x..y`.</span><span class="sxs-lookup"><span data-stu-id="267af-129">The language will introduce a new range operator `x..y`.</span></span> <span data-ttu-id="267af-130">Es un operador binario de infijo que acepta dos expresiones.</span><span class="sxs-lookup"><span data-stu-id="267af-130">It is a binary infix operator that accepts two expressions.</span></span> <span data-ttu-id="267af-131">Se puede omitir cualquiera de los operandos (ejemplos siguientes) y deben poder convertirse en `System.Index`.</span><span class="sxs-lookup"><span data-stu-id="267af-131">Either operand can be omitted (examples below), and they have to be convertible to `System.Index`.</span></span> <span data-ttu-id="267af-132">Se reducirá al `System.Range` adecuado Factory Method llamada.</span><span class="sxs-lookup"><span data-stu-id="267af-132">It will be lowered to the appropriate `System.Range` factory method call.</span></span>

<span data-ttu-id="267af-133">Reemplazamos las C# reglas de gramática de *multiplicative_expression* por lo siguiente (para introducir un nuevo nivel de prioridad):</span><span class="sxs-lookup"><span data-stu-id="267af-133">We replace the C# grammar rules for *multiplicative_expression* with the following (in order to introduce a new precedence level):</span></span>

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

<span data-ttu-id="267af-134">Todas las formas del *operador Range* tienen la misma precedencia.</span><span class="sxs-lookup"><span data-stu-id="267af-134">All forms of the *range operator* have the same precedence.</span></span> <span data-ttu-id="267af-135">Este nuevo grupo de precedencia es inferior a los *operadores unarios* y superior a los *operadores aritméticos mulitiplicative*.</span><span class="sxs-lookup"><span data-stu-id="267af-135">This new precedence group is lower than the *unary operators* and higher than the *mulitiplicative arithmetic operators*.</span></span>

<span data-ttu-id="267af-136">Llamamos al operador de `..` el *operador de intervalo*.</span><span class="sxs-lookup"><span data-stu-id="267af-136">We call the `..` operator the *range operator*.</span></span> <span data-ttu-id="267af-137">El operador de intervalo integrado se puede entender aproximadamente para que se corresponda con la invocación de un operador integrado de este formulario:</span><span class="sxs-lookup"><span data-stu-id="267af-137">The built-in range operator can roughly be understood to correspond to the invocation of a built-in operator of this form:</span></span>

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

<span data-ttu-id="267af-138">Ejemplos:</span><span class="sxs-lookup"><span data-stu-id="267af-138">Examples:</span></span>

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

<span data-ttu-id="267af-139">Además, `System.Index` debe tener una conversión implícita de `System.Int32`, por lo que no es necesario sobrecargar para mezclar enteros e índices en firmas multidimensionales.</span><span class="sxs-lookup"><span data-stu-id="267af-139">Moreover, `System.Index` should have an implicit conversion from `System.Int32`, in order not to need to overload for mixing integers and indexes over multi-dimensional signatures.</span></span>

## <a name="adding-index-and-range-support-to-existing-library-types"></a><span data-ttu-id="267af-140">Agregar compatibilidad con índices y rangos a tipos de biblioteca existentes</span><span class="sxs-lookup"><span data-stu-id="267af-140">Adding Index and Range support to existing library types</span></span>

### <a name="implicit-index-support"></a><span data-ttu-id="267af-141">Compatibilidad con índices implícitos</span><span class="sxs-lookup"><span data-stu-id="267af-141">Implicit Index support</span></span>

<span data-ttu-id="267af-142">El lenguaje proporcionará un miembro de indexador de instancia con un parámetro único de tipo `Index` para los tipos que cumplen los criterios siguientes:</span><span class="sxs-lookup"><span data-stu-id="267af-142">The language will provide an instance indexer member with a single parameter of type `Index` for types which meet the following criteria:</span></span>

- <span data-ttu-id="267af-143">El tipo es recuento.</span><span class="sxs-lookup"><span data-stu-id="267af-143">The type is Countable.</span></span>
- <span data-ttu-id="267af-144">El tipo tiene un indizador de instancia accesible que toma un único `int` como argumento.</span><span class="sxs-lookup"><span data-stu-id="267af-144">The type has an accessible instance indexer which takes a single `int` as the argument.</span></span>
- <span data-ttu-id="267af-145">El tipo no tiene un indizador de instancia accesible que toma un `Index` como primer parámetro.</span><span class="sxs-lookup"><span data-stu-id="267af-145">The type does not have an accessible instance indexer which takes an `Index` as the first parameter.</span></span> <span data-ttu-id="267af-146">El `Index` debe ser el único parámetro o los parámetros restantes deben ser opcionales.</span><span class="sxs-lookup"><span data-stu-id="267af-146">The `Index` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="267af-147">Un tipo se puede ***contar*** si tiene una propiedad denominada `Length` o `Count` con un captador accesible y un tipo de valor devuelto de `int`.</span><span class="sxs-lookup"><span data-stu-id="267af-147">A type is ***Countable*** if it  has a property named `Length` or `Count` with an accessible getter and a return type of `int`.</span></span> <span data-ttu-id="267af-148">El lenguaje puede hacer uso de esta propiedad para convertir una expresión de tipo `Index` en un `int` en el punto de la expresión sin necesidad de usar el tipo `Index` en absoluto.</span><span class="sxs-lookup"><span data-stu-id="267af-148">The language can make use of this property to convert an expression of type `Index` into an `int` at the point of the expression without the need to use the type `Index` at all.</span></span> <span data-ttu-id="267af-149">En caso de que existan `Length` y `Count`, se preferirá `Length`.</span><span class="sxs-lookup"><span data-stu-id="267af-149">In case both `Length` and `Count` are present, `Length` will be preferred.</span></span> <span data-ttu-id="267af-150">Para facilitar la simplicidad, la propuesta usará el nombre `Length` para representar `Count` o `Length`.</span><span class="sxs-lookup"><span data-stu-id="267af-150">For simplicity going forward, the proposal will use the name `Length` to represent `Count` or `Length`.</span></span>

<span data-ttu-id="267af-151">Para estos tipos, el lenguaje actuará como si hubiera un miembro de indexador con el formato `T this[Index index]` donde `T` es el tipo de valor devuelto del indexador basado en `int`, incluidas las anotaciones de estilo de `ref`.</span><span class="sxs-lookup"><span data-stu-id="267af-151">For such types, the language will act as if there is an indexer member of the form `T this[Index index]` where `T` is the return type of the `int` based indexer including any `ref` style annotations.</span></span> <span data-ttu-id="267af-152">El nuevo miembro tendrá el mismo `get` y `set` miembros con accesibilidad coincidente que el indizador `int`.</span><span class="sxs-lookup"><span data-stu-id="267af-152">The new member will have the same `get` and `set` members with matching accessibility as the `int` indexer.</span></span> 

<span data-ttu-id="267af-153">El nuevo indexador se implementará mediante la conversión del argumento de tipo `Index` en un `int` y la emisión de una llamada al indizador basado en `int`.</span><span class="sxs-lookup"><span data-stu-id="267af-153">The new indexer will be implemented by converting the argument of type `Index` into an `int` and emitting a call to the `int` based indexer.</span></span> <span data-ttu-id="267af-154">A efectos de discusión, vamos a usar el ejemplo de `receiver[expr]`.</span><span class="sxs-lookup"><span data-stu-id="267af-154">For discussion purposes, let's use the example of `receiver[expr]`.</span></span> <span data-ttu-id="267af-155">La conversión de `expr` en `int` se realizará de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="267af-155">The conversion of `expr` to `int` will occur as follows:</span></span>

- <span data-ttu-id="267af-156">Cuando el argumento tiene el formato `^expr2` y el tipo de `expr2` es `int`, se traducirá a `receiver.Length - expr2`.</span><span class="sxs-lookup"><span data-stu-id="267af-156">When the argument is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="267af-157">De lo contrario, se traducirá como `expr.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="267af-157">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="267af-158">Esto permite a los desarrolladores usar la característica `Index` en tipos existentes sin necesidad de modificarla.</span><span class="sxs-lookup"><span data-stu-id="267af-158">This allows for developers to use the `Index` feature on existing types without the need for modification.</span></span> <span data-ttu-id="267af-159">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="267af-159">For example:</span></span>

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

<span data-ttu-id="267af-160">Las expresiones `receiver` y `Length` se perderán según corresponda para asegurarse de que los efectos secundarios solo se ejecutan una vez.</span><span class="sxs-lookup"><span data-stu-id="267af-160">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="267af-161">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="267af-161">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="267af-162">Este código imprimirá "obtener longitud 3".</span><span class="sxs-lookup"><span data-stu-id="267af-162">This code will print "Get Length 3".</span></span>

### <a name="implicit-range-support"></a><span data-ttu-id="267af-163">Compatibilidad de intervalo implícita</span><span class="sxs-lookup"><span data-stu-id="267af-163">Implicit Range support</span></span>

<span data-ttu-id="267af-164">El lenguaje proporcionará un miembro de indexador de instancia con un parámetro único de tipo `Range` para los tipos que cumplen los criterios siguientes:</span><span class="sxs-lookup"><span data-stu-id="267af-164">The language will provide an instance indexer member with a single parameter of type `Range` for types which meet the following criteria:</span></span>

- <span data-ttu-id="267af-165">El tipo es recuento.</span><span class="sxs-lookup"><span data-stu-id="267af-165">The type is Countable.</span></span>
- <span data-ttu-id="267af-166">El tipo tiene un miembro accesible denominado `Slice` que tiene dos parámetros de tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="267af-166">The type has an accessible member named `Slice` which has two parameters of type `int`.</span></span>
- <span data-ttu-id="267af-167">El tipo no tiene un indizador de instancia que toma un único `Range` como primer parámetro.</span><span class="sxs-lookup"><span data-stu-id="267af-167">The type does not have an instance indexer which takes a single `Range` as the first parameter.</span></span> <span data-ttu-id="267af-168">El `Range` debe ser el único parámetro o los parámetros restantes deben ser opcionales.</span><span class="sxs-lookup"><span data-stu-id="267af-168">The `Range` must be the only parameter or the remaining parameters must be optional.</span></span>

<span data-ttu-id="267af-169">Para estos tipos, el lenguaje se enlazará como si hubiera un miembro de indizador con el formato `T this[Range range]` donde `T` es el tipo de valor devuelto del método `Slice`, incluidas las anotaciones de estilo de `ref`.</span><span class="sxs-lookup"><span data-stu-id="267af-169">For such types, the language will bind as if there is an indexer member of the form `T this[Range range]` where `T` is the return type of the `Slice` method including any `ref` style annotations.</span></span> <span data-ttu-id="267af-170">El nuevo miembro también tendrá accesibilidad coincidente con `Slice`.</span><span class="sxs-lookup"><span data-stu-id="267af-170">The new member will also have matching accessibility with `Slice`.</span></span> 

<span data-ttu-id="267af-171">Cuando el indexador basado en `Range` se enlaza a una expresión llamada `receiver`, se reducirá al convertir la expresión `Range` en dos valores que se pasarán al método `Slice`.</span><span class="sxs-lookup"><span data-stu-id="267af-171">When the `Range` based indexer is bound on an expression named `receiver`, it will be lowered by converting the `Range` expression into two values that are then passed to the `Slice` method.</span></span> <span data-ttu-id="267af-172">A efectos de discusión, vamos a usar el ejemplo de `receiver[expr]`.</span><span class="sxs-lookup"><span data-stu-id="267af-172">For discussion purposes, let's use the example of `receiver[expr]`.</span></span>

<span data-ttu-id="267af-173">El primer argumento de `Slice` se obtendrá convirtiendo la expresión con tipo de intervalo de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="267af-173">The first argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="267af-174">Cuando `expr` tiene el formato `expr1..expr2` (donde `expr2` se puede omitir) y `expr1` tiene el tipo `int`, se emitirá como `expr1`.</span><span class="sxs-lookup"><span data-stu-id="267af-174">When `expr` is of the form `expr1..expr2` (where `expr2` can be omitted) and `expr1` has type `int`, then it will be emitted as `expr1`.</span></span>
- <span data-ttu-id="267af-175">Cuando `expr` tiene el formato `^expr1..expr2` (donde `expr2` se puede omitir), se emitirá como `receiver.Length - expr1`.</span><span class="sxs-lookup"><span data-stu-id="267af-175">When `expr` is of the form `^expr1..expr2` (where `expr2` can be omitted), then it will be emitted as `receiver.Length - expr1`.</span></span>
- <span data-ttu-id="267af-176">Cuando `expr` tiene el formato `..expr2` (donde `expr2` se puede omitir), se emitirá como `0`.</span><span class="sxs-lookup"><span data-stu-id="267af-176">When `expr` is of the form `..expr2` (where `expr2` can be omitted), then it will be emitted as `0`.</span></span>
- <span data-ttu-id="267af-177">De lo contrario, se emitirá como `expr.Start.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="267af-177">Otherwise, it will be emitted as `expr.Start.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="267af-178">Este valor se volverá a usar en el cálculo del segundo argumento `Slice`.</span><span class="sxs-lookup"><span data-stu-id="267af-178">This value will be re-used in the calculation of the second `Slice` argument.</span></span> <span data-ttu-id="267af-179">Al hacerlo, se hará referencia a él como `start`.</span><span class="sxs-lookup"><span data-stu-id="267af-179">When doing so it will be referred to as `start`.</span></span> <span data-ttu-id="267af-180">El segundo argumento de `Slice` se obtendrá convirtiendo la expresión con tipo de intervalo de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="267af-180">The second argument of `Slice` will be obtained by converting the range typed expression in the following way:</span></span>

- <span data-ttu-id="267af-181">Cuando `expr` tiene el formato `expr1..expr2` (donde `expr1` se puede omitir) y `expr2` tiene el tipo `int`, se emitirá como `expr2 - start`.</span><span class="sxs-lookup"><span data-stu-id="267af-181">When `expr` is of the form `expr1..expr2` (where `expr1` can be omitted) and `expr2` has type `int`, then it will be emitted as `expr2 - start`.</span></span>
- <span data-ttu-id="267af-182">Cuando `expr` tiene el formato `expr1..^expr2` (donde `expr1` se puede omitir), se emitirá como `(receiver.Length - expr2) - start`.</span><span class="sxs-lookup"><span data-stu-id="267af-182">When `expr` is of the form `expr1..^expr2` (where `expr1` can be omitted), then it will be emitted as `(receiver.Length - expr2) - start`.</span></span>
- <span data-ttu-id="267af-183">Cuando `expr` tiene el formato `expr1..` (donde `expr1` se puede omitir), se emitirá como `receiver.Length - start`.</span><span class="sxs-lookup"><span data-stu-id="267af-183">When `expr` is of the form `expr1..` (where `expr1` can be omitted), then it will be emitted as `receiver.Length - start`.</span></span>
- <span data-ttu-id="267af-184">De lo contrario, se emitirá como `expr.End.GetOffset(receiver.Length) - start`.</span><span class="sxs-lookup"><span data-stu-id="267af-184">Otherwise, it will be emitted as `expr.End.GetOffset(receiver.Length) - start`.</span></span>

<span data-ttu-id="267af-185">Las expresiones `receiver`, `Length`y `expr` se perderán según corresponda para asegurarse de que los efectos secundarios solo se ejecutan una vez.</span><span class="sxs-lookup"><span data-stu-id="267af-185">The `receiver`, `Length`, and `expr` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="267af-186">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="267af-186">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

<span data-ttu-id="267af-187">Este código imprimirá "obtener longitud 2".</span><span class="sxs-lookup"><span data-stu-id="267af-187">This code will print "Get Length 2".</span></span>

<span data-ttu-id="267af-188">El lenguaje será especial en caso de que se trate de los siguientes tipos conocidos:</span><span class="sxs-lookup"><span data-stu-id="267af-188">The language will special case the following known types:</span></span> 

- <span data-ttu-id="267af-189">`string`: se usará el `Substring` de método en lugar de `Slice`.</span><span class="sxs-lookup"><span data-stu-id="267af-189">`string`: the method `Substring` will be used instead of `Slice`.</span></span>
- <span data-ttu-id="267af-190">`array`: se usará el `System.Reflection.CompilerServices.GetSubArray` de método en lugar de `Slice`.</span><span class="sxs-lookup"><span data-stu-id="267af-190">`array`: the method `System.Reflection.CompilerServices.GetSubArray` will be used instead of `Slice`.</span></span>

## <a name="alternatives"></a><span data-ttu-id="267af-191">Alternativas</span><span class="sxs-lookup"><span data-stu-id="267af-191">Alternatives</span></span>

<span data-ttu-id="267af-192">Los nuevos operadores (`^` y `..`) son el azúcar sintáctico.</span><span class="sxs-lookup"><span data-stu-id="267af-192">The new operators (`^` and `..`) are syntactic sugar.</span></span> <span data-ttu-id="267af-193">La funcionalidad se puede implementar mediante llamadas explícitas a `System.Index` y `System.Range` métodos de generador, pero dará como resultado un código mucho más reutilizable y la experiencia no será intuitiva.</span><span class="sxs-lookup"><span data-stu-id="267af-193">The functionality can be implemented by explicit calls to `System.Index` and `System.Range` factory methods, but it will result in a lot more boilerplate code, and the experience will be unintuitive.</span></span>

## <a name="il-representation"></a><span data-ttu-id="267af-194">Representación de IL</span><span class="sxs-lookup"><span data-stu-id="267af-194">IL Representation</span></span>

<span data-ttu-id="267af-195">Estos dos operadores se reducirán a las llamadas normales de indexador/método, sin cambios en los niveles posteriores del compilador.</span><span class="sxs-lookup"><span data-stu-id="267af-195">These two operators will be lowered to regular indexer/method calls, with no change in subsequent compiler layers.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="267af-196">Comportamiento en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="267af-196">Runtime behavior</span></span>

- <span data-ttu-id="267af-197">El compilador puede optimizar los indizadores para los tipos integrados como matrices y cadenas, y reducir la indización a los métodos existentes correspondientes.</span><span class="sxs-lookup"><span data-stu-id="267af-197">Compiler can optimize indexers for built-in types like arrays and strings, and lower the indexing to the appropriate existing methods.</span></span>
- <span data-ttu-id="267af-198">`System.Index` producirá si se construye con un valor negativo.</span><span class="sxs-lookup"><span data-stu-id="267af-198">`System.Index` will throw if constructed with a negative value.</span></span>
- <span data-ttu-id="267af-199">`^0` no inicia, pero se convierte en la longitud de la colección o Enumerable en la que se proporciona.</span><span class="sxs-lookup"><span data-stu-id="267af-199">`^0` does not throw, but it translates to the length of the collection/enumerable it is supplied to.</span></span>
- <span data-ttu-id="267af-200">`Range.All` es semánticamente equivalente a `0..^0`y se puede desconstruir en estos índices.</span><span class="sxs-lookup"><span data-stu-id="267af-200">`Range.All` is semantically equivalent to `0..^0`, and can be deconstructed to these indices.</span></span>

## <a name="considerations"></a><span data-ttu-id="267af-201">Consideraciones</span><span class="sxs-lookup"><span data-stu-id="267af-201">Considerations</span></span>

### <a name="detect-indexable-based-on-icollection"></a><span data-ttu-id="267af-202">Detectar indexable basada en ICollection</span><span class="sxs-lookup"><span data-stu-id="267af-202">Detect Indexable based on ICollection</span></span>

<span data-ttu-id="267af-203">La inspiración para este comportamiento era inicializadores de colección.</span><span class="sxs-lookup"><span data-stu-id="267af-203">The inspiration for this behavior was collection initializers.</span></span> <span data-ttu-id="267af-204">Usar la estructura de un tipo para indicar que ha participado en una característica.</span><span class="sxs-lookup"><span data-stu-id="267af-204">Using the structure of a type to convey that it had opted into a feature.</span></span> <span data-ttu-id="267af-205">En el caso de los tipos de inicializadores de colección, puede participar en la característica implementando la interfaz `IEnumerable` (no genérica).</span><span class="sxs-lookup"><span data-stu-id="267af-205">In the case of collection initializers types can opt into the feature by implementing the interface `IEnumerable` (non generic).</span></span>

<span data-ttu-id="267af-206">Esta propuesta requiere inicialmente que los tipos implementen `ICollection` para poder calificarse como indexable.</span><span class="sxs-lookup"><span data-stu-id="267af-206">This proposal initially required that types implement `ICollection` in order to qualify as Indexable.</span></span> <span data-ttu-id="267af-207">Sin embargo, esto requería un número de casos especiales:</span><span class="sxs-lookup"><span data-stu-id="267af-207">That required a number of special cases though:</span></span>

- <span data-ttu-id="267af-208">`ref struct`: estos no pueden implementar interfaces todavía tipos como `Span<T>` son ideales para la compatibilidad con índices y rangos.</span><span class="sxs-lookup"><span data-stu-id="267af-208">`ref struct`: these cannot implement interfaces yet types like `Span<T>` are ideal for index / range support.</span></span> 
- <span data-ttu-id="267af-209">`string`: no implementa `ICollection` y agregar ese `interface` tiene un gran costo.</span><span class="sxs-lookup"><span data-stu-id="267af-209">`string`: does not implement `ICollection` and adding that `interface` has a large cost.</span></span>

<span data-ttu-id="267af-210">Esto significa que la compatibilidad con tipos de clave especiales ya es necesaria.</span><span class="sxs-lookup"><span data-stu-id="267af-210">This means to support key types special casing is already needed.</span></span> <span data-ttu-id="267af-211">El uso de mayúsculas y minúsculas especiales de `string` es menos interesante, ya que el lenguaje lo hace en otras áreas (`foreach` bajar, constantes, etc.). El uso de mayúsculas y minúsculas especiales de `ref struct` es más en lo que se refiere a una clase completa de tipos.</span><span class="sxs-lookup"><span data-stu-id="267af-211">The special casing of `string` is less interesting as the language does this in other areas (`foreach` lowering, constants, etc ...). The special casing of `ref struct` is more concerning as it's special casing an entire class of types.</span></span> <span data-ttu-id="267af-212">Se etiquetan como indexables si simplemente tienen una propiedad denominada `Count` con un tipo de valor devuelto de `int`.</span><span class="sxs-lookup"><span data-stu-id="267af-212">They get labeled as Indexable if they simply have a property named `Count` with a return type of `int`.</span></span> 

<span data-ttu-id="267af-213">Después de tener en cuenta que se ha normalizado el diseño para indicar que cualquier tipo que tiene una propiedad `Count` / `Length` con un tipo de valor devuelto de `int` es indexable.</span><span class="sxs-lookup"><span data-stu-id="267af-213">After consideration the design was normalized to say that any type which has a property `Count` / `Length` with a return type of `int` is Indexable.</span></span> <span data-ttu-id="267af-214">Esto quita todas las mayúsculas y minúsculas especiales, incluso para `string` y matrices.</span><span class="sxs-lookup"><span data-stu-id="267af-214">That removes all special casing, even for `string` and arrays.</span></span>

### <a name="detect-just-count"></a><span data-ttu-id="267af-215">Detectar solo recuento</span><span class="sxs-lookup"><span data-stu-id="267af-215">Detect just Count</span></span>

<span data-ttu-id="267af-216">La detección de los nombres de propiedad `Count` o `Length` complica el diseño un poco.</span><span class="sxs-lookup"><span data-stu-id="267af-216">Detecting on the property names `Count` or `Length` does complicate the design a bit.</span></span> <span data-ttu-id="267af-217">La selección de solo una para estandarizar no es suficiente, ya que termina excluyendo un gran número de tipos:</span><span class="sxs-lookup"><span data-stu-id="267af-217">Picking just one to standardize though is not sufficient as it ends up excluding a large number of types:</span></span>

- <span data-ttu-id="267af-218">Use `Length`: excluye prácticamente todas las colecciones de los subespacios de nombres System. Collections y sub-namespaces.</span><span class="sxs-lookup"><span data-stu-id="267af-218">Use `Length`: excludes pretty much every collection in System.Collections and sub-namespaces.</span></span> <span data-ttu-id="267af-219">Tienden a derivarse de `ICollection` y, por tanto, prefieren `Count` de la longitud.</span><span class="sxs-lookup"><span data-stu-id="267af-219">Those tend to derive from `ICollection` and hence prefer `Count` over length.</span></span>
- <span data-ttu-id="267af-220">Usar `Count`: excluye `string`, matrices, `Span<T>` y la mayoría de los tipos basados en `ref struct`</span><span class="sxs-lookup"><span data-stu-id="267af-220">Use `Count`: excludes `string`, arrays, `Span<T>` and most `ref struct` based types</span></span>

<span data-ttu-id="267af-221">La complicación adicional en la detección inicial de los tipos indexables se compensa por su simplificación en otros aspectos.</span><span class="sxs-lookup"><span data-stu-id="267af-221">The extra complication on the initial detection of Indexable types is outweighed by its simplification in other aspects.</span></span>

### <a name="choice-of-slice-as-a-name"></a><span data-ttu-id="267af-222">Elección del segmento como nombre</span><span class="sxs-lookup"><span data-stu-id="267af-222">Choice of Slice as a name</span></span>

<span data-ttu-id="267af-223">Se eligió el nombre `Slice` como es el nombre estándar de facto para las operaciones de estilo de segmento en .NET.</span><span class="sxs-lookup"><span data-stu-id="267af-223">The name `Slice` was chosen as it's the de-facto standard name for slice style operations in .NET.</span></span> <span data-ttu-id="267af-224">A partir de netcoreapp 2.1, todos los tipos de estilo de span usan el nombre `Slice` para las operaciones de segmentación.</span><span class="sxs-lookup"><span data-stu-id="267af-224">Starting with netcoreapp2.1 all span style types use the name `Slice` for slicing operations.</span></span> <span data-ttu-id="267af-225">Antes de netcoreapp 2.1, en realidad no hay ningún ejemplo de segmentación para ver un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="267af-225">Prior to netcoreapp2.1 there really aren't any examples of slicing to look to for an example.</span></span> <span data-ttu-id="267af-226">Los tipos como `List<T>`, `ArraySegment<T>``SortedList<T>` serían ideales para la segmentación, pero el concepto no existía cuando se agregaban tipos.</span><span class="sxs-lookup"><span data-stu-id="267af-226">Types like `List<T>`, `ArraySegment<T>`, `SortedList<T>` would've been ideal for slicing but the concept didn't exist when types were added.</span></span> 

<span data-ttu-id="267af-227">Por lo tanto, `Slice` ser el único ejemplo, se eligió como nombre.</span><span class="sxs-lookup"><span data-stu-id="267af-227">Thus, `Slice` being the sole example, it was chosen as the name.</span></span>

### <a name="index-target-type-conversion"></a><span data-ttu-id="267af-228">Conversión de tipo de destino de índice</span><span class="sxs-lookup"><span data-stu-id="267af-228">Index target type conversion</span></span>

<span data-ttu-id="267af-229">Otra manera de ver el `Index` transformación en una expresión de indexador es como una conversión de tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="267af-229">Another way to view the `Index` transformation in an indexer expression is as a target type conversion.</span></span> <span data-ttu-id="267af-230">En lugar de enlazar como si hubiera un miembro del formulario `return_type this[Index]`, en su lugar, el lenguaje asigna una conversión con tipo de destino a `int`.</span><span class="sxs-lookup"><span data-stu-id="267af-230">Instead of binding as if there is a member of the form `return_type this[Index]`, the language instead assigns a target typed conversion to `int`.</span></span> 

<span data-ttu-id="267af-231">Este concepto podría generalizarse para todos los accesos a miembros en tipos que se pueden contar.</span><span class="sxs-lookup"><span data-stu-id="267af-231">This concept could be generalized to all member access on Countable types.</span></span> <span data-ttu-id="267af-232">Siempre que se utiliza una expresión con el tipo `Index` como argumento para la invocación de un miembro de instancia y el receptor es recuento, la expresión tendrá una conversión de tipo de destino a `int`.</span><span class="sxs-lookup"><span data-stu-id="267af-232">Whenever an expression with type `Index` is used as an argument to an instance member invocation and the receiver is Countable then the expression will have a target type conversion to `int`.</span></span> <span data-ttu-id="267af-233">Las invocaciones de miembro aplicables para esta conversión incluyen métodos, indexadores, propiedades, métodos de extensión, etc. Solo se excluyen los constructores, ya que no tienen ningún receptor.</span><span class="sxs-lookup"><span data-stu-id="267af-233">The member invocations applicable for this conversion include methods, indexers, properties, extension methods, etc ... Only constructors are excluded as they have no receiver.</span></span> 

<span data-ttu-id="267af-234">La conversión de tipo de destino se implementará como se indica a continuación para cualquier expresión que tenga un tipo de `Index`.</span><span class="sxs-lookup"><span data-stu-id="267af-234">The target type conversion will be implemented as follows for any expression which has a type of `Index`.</span></span> <span data-ttu-id="267af-235">Para fines de análisis, use el ejemplo de `receiver[expr]`:</span><span class="sxs-lookup"><span data-stu-id="267af-235">For discussion purposes lets use the example of `receiver[expr]`:</span></span>

- <span data-ttu-id="267af-236">Cuando `expr` tiene el formato `^expr2` y se `int`el tipo de `expr2`, se traducirá a `receiver.Length - expr2`.</span><span class="sxs-lookup"><span data-stu-id="267af-236">When `expr` is of the form `^expr2` and the type of `expr2` is `int`, it will be translated to `receiver.Length - expr2`.</span></span>
- <span data-ttu-id="267af-237">De lo contrario, se traducirá como `expr.GetOffset(receiver.Length)`.</span><span class="sxs-lookup"><span data-stu-id="267af-237">Otherwise, it will be translated as `expr.GetOffset(receiver.Length)`.</span></span>

<span data-ttu-id="267af-238">Las expresiones `receiver` y `Length` se perderán según corresponda para asegurarse de que los efectos secundarios solo se ejecutan una vez.</span><span class="sxs-lookup"><span data-stu-id="267af-238">The `receiver` and `Length` expressions will be spilled as appropriate to ensure any side effects are only executed once.</span></span> <span data-ttu-id="267af-239">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="267af-239">For example:</span></span>

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="267af-240">Este código imprimirá "obtener longitud 3".</span><span class="sxs-lookup"><span data-stu-id="267af-240">This code will print "Get Length 3".</span></span> 

<span data-ttu-id="267af-241">Esta característica sería beneficiosa para cualquier miembro que tuviera un parámetro que representase un índice.</span><span class="sxs-lookup"><span data-stu-id="267af-241">This feature would be beneficial to any member which had a parameter that represented an index.</span></span> <span data-ttu-id="267af-242">Por ejemplo, `List<T>.InsertAt`.</span><span class="sxs-lookup"><span data-stu-id="267af-242">For example `List<T>.InsertAt`.</span></span> <span data-ttu-id="267af-243">Esto también tiene la posibilidad de confusión, ya que el lenguaje no puede proporcionar ninguna orientación sobre si una expresión está pensada para la indización o no.</span><span class="sxs-lookup"><span data-stu-id="267af-243">This also has the potential for confusion as the language can't give any guidance as to whether or not an expression is meant for indexing.</span></span> <span data-ttu-id="267af-244">Todo lo que puede hacer es convertir cualquier expresión `Index` en `int` al invocar un miembro en un tipo que se puede contar.</span><span class="sxs-lookup"><span data-stu-id="267af-244">All it can do is convert any `Index` expression to `int` when invoking a member on a Countable type.</span></span> 

<span data-ttu-id="267af-245">Restricciones:</span><span class="sxs-lookup"><span data-stu-id="267af-245">Restrictions:</span></span>

- <span data-ttu-id="267af-246">Esta conversión solo es aplicable cuando la expresión con el tipo `Index` es directamente un argumento para el miembro.</span><span class="sxs-lookup"><span data-stu-id="267af-246">This conversion is only applicable when the expression with type `Index` is directly an argument to the member.</span></span> <span data-ttu-id="267af-247">No se aplicaría a ninguna expresión anidada.</span><span class="sxs-lookup"><span data-stu-id="267af-247">It would not apply to any nested expressions.</span></span>

## <a name="decisions-made-during-implementation"></a><span data-ttu-id="267af-248">Decisiones tomadas durante la implementación</span><span class="sxs-lookup"><span data-stu-id="267af-248">Decisions made during implementation</span></span>

- <span data-ttu-id="267af-249">Todos los miembros del patrón deben ser miembros de instancia</span><span class="sxs-lookup"><span data-stu-id="267af-249">All members in the pattern must be instance members</span></span>
- <span data-ttu-id="267af-250">Si se encuentra un método de longitud pero tiene un tipo de valor devuelto incorrecto, continúe buscando el recuento.</span><span class="sxs-lookup"><span data-stu-id="267af-250">If a Length method is found but it has the wrong return type, continue looking for Count</span></span>
- <span data-ttu-id="267af-251">El indexador utilizado para el patrón de índice debe tener exactamente un parámetro int</span><span class="sxs-lookup"><span data-stu-id="267af-251">The indexer used for the Index pattern must have exactly one int parameter</span></span>
- <span data-ttu-id="267af-252">El método slice que se usa para el patrón de intervalo debe tener exactamente dos parámetros int</span><span class="sxs-lookup"><span data-stu-id="267af-252">The Slice method used for the Range pattern must have exactly two int parameters</span></span>
- <span data-ttu-id="267af-253">Al buscar los miembros del patrón, buscamos definiciones originales, no miembros construidos</span><span class="sxs-lookup"><span data-stu-id="267af-253">When looking for the pattern members, we look for original definitions, not constructed members</span></span>

## <a name="design-meetings"></a><span data-ttu-id="267af-254">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="267af-254">Design Meetings</span></span>

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a><span data-ttu-id="267af-255">Preguntas</span><span class="sxs-lookup"><span data-stu-id="267af-255">Questions</span></span>
