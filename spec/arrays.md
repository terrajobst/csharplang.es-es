# <a name="arrays"></a><span data-ttu-id="7bd1e-101">Matrices</span><span class="sxs-lookup"><span data-stu-id="7bd1e-101">Arrays</span></span>

<span data-ttu-id="7bd1e-102">Una matriz es una estructura de datos que contiene una serie de variables que se accede mediante índices calculados.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-102">An array is a data structure that contains a number of variables which are accessed through computed indices.</span></span> <span data-ttu-id="7bd1e-103">Las variables contenidas en una matriz, también conocida como elementos de la matriz, son todas del mismo tipo, y este tipo se denomina tipo de elemento de la matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-103">The variables contained in an array, also called the elements of the array, are all of the same type, and this type is called the element type of the array.</span></span>

<span data-ttu-id="7bd1e-104">Una matriz tiene un rango que determina el número de índices asociados a cada elemento de matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-104">An array has a rank which determines the number of indices associated with each array element.</span></span> <span data-ttu-id="7bd1e-105">El rango de matriz se conoce también como las dimensiones de la matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-105">The rank of an array is also referred to as the dimensions of the array.</span></span> <span data-ttu-id="7bd1e-106">Una matriz con un rango de uno se denomina un ***matriz unidimensional***.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-106">An array with a rank of one is called a ***single-dimensional array***.</span></span> <span data-ttu-id="7bd1e-107">Una matriz con un rango mayor que uno se denomina un ***matriz multidimensional***.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-107">An array with a rank greater than one is called a ***multi-dimensional array***.</span></span> <span data-ttu-id="7bd1e-108">Matrices multidimensionales de tamaño específicas a menudo se conocen como las matrices bidimensionales, las matrices tridimensionales y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-108">Specific sized multi-dimensional arrays are often referred to as two-dimensional arrays, three-dimensional arrays, and so on.</span></span>

<span data-ttu-id="7bd1e-109">Cada dimensión de una matriz tiene una longitud asociada que es un número entero mayor o igual que cero.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-109">Each dimension of an array has an associated length which is an integral number greater than or equal to zero.</span></span> <span data-ttu-id="7bd1e-110">Las longitudes de dimensión no forman parte del tipo de la matriz, pero se establecen cuando se crea una instancia del tipo de matriz en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-110">The dimension lengths are not part of the type of the array, but rather are established when an instance of the array type is created at run-time.</span></span> <span data-ttu-id="7bd1e-111">La longitud de una dimensión determina el intervalo válido de índices para esa dimensión: Para una dimensión de longitud `N`, los índices pueden oscilar entre `0` a `N - 1` inclusive.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-111">The length of a dimension determines the valid range of indices for that dimension: For a dimension of length `N`, indices can range from `0` to `N - 1` inclusive.</span></span> <span data-ttu-id="7bd1e-112">El número total de elementos de una matriz es el producto de las longitudes de cada dimensión de la matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-112">The total number of elements in an array is the product of the lengths of each dimension in the array.</span></span> <span data-ttu-id="7bd1e-113">Si una o varias de las dimensiones de la matriz tienen una longitud de cero, se dice que la matriz estar vacío.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-113">If one or more of the dimensions of an array have a length of zero, the array is said to be empty.</span></span>

<span data-ttu-id="7bd1e-114">El tipo de elemento de una matriz puede ser cualquiera, incluido un tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-114">The element type of an array can be any type, including an array type.</span></span>

## <a name="array-types"></a><span data-ttu-id="7bd1e-115">Tipos de matriz</span><span class="sxs-lookup"><span data-stu-id="7bd1e-115">Array types</span></span>

<span data-ttu-id="7bd1e-116">Un tipo de matriz se escribe como un *non_array_type* seguido de uno o varios *rank_specifier*s:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-116">An array type is written as a *non_array_type* followed by one or more *rank_specifier*s:</span></span>

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

<span data-ttu-id="7bd1e-117">Un *non_array_type* es cualquier *tipo* que es no sí un *array_type*.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-117">A *non_array_type* is any *type* that is not itself an *array_type*.</span></span>

<span data-ttu-id="7bd1e-118">El rango de un tipo de matriz viene dado por el extremo izquierdo *rank_specifier* en el *array_type*: Un *rank_specifier* indica que la matriz es una matriz con un rango de uno más el número de "`,`" tokens en el *rank_specifier*.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-118">The rank of an array type is given by the leftmost *rank_specifier* in the *array_type*: A *rank_specifier* indicates that the array is an array with a rank of one plus the number of "`,`" tokens in the *rank_specifier*.</span></span>

<span data-ttu-id="7bd1e-119">El tipo de elemento de un tipo de matriz es el tipo resultante de eliminar el extremo izquierdo *rank_specifier*:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-119">The element type of an array type is the type that results from deleting the leftmost *rank_specifier*:</span></span>

*  <span data-ttu-id="7bd1e-120">Un tipo de matriz del formulario `T[R]` es una matriz con rango `R` y un tipo de elemento que no son de matriz `T`.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-120">An array type of the form `T[R]` is an array with rank `R` and a non-array element type `T`.</span></span>
*  <span data-ttu-id="7bd1e-121">Un tipo de matriz del formulario `T[R][R1]...[Rn]` es una matriz con rango `R` y un tipo de elemento `T[R1]...[Rn]`.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-121">An array type of the form `T[R][R1]...[Rn]` is an array with rank `R` and an element type `T[R1]...[Rn]`.</span></span>

<span data-ttu-id="7bd1e-122">De hecho, el *rank_specifier*s se leen de izquierda a derecha antes del tipo de elemento final que no son de matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-122">In effect, the *rank_specifier*s are read from left to right before the final non-array element type.</span></span> <span data-ttu-id="7bd1e-123">El tipo `int[][,,][,]` es una matriz unidimensional de las matrices tridimensionales de matrices bidimensionales de `int`.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-123">The type `int[][,,][,]` is a single-dimensional array of three-dimensional arrays of two-dimensional arrays of `int`.</span></span>

<span data-ttu-id="7bd1e-124">En tiempo de ejecución, puede ser un valor de un tipo de matriz `null` o una referencia a una instancia de ese tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-124">At run-time, a value of an array type can be `null` or a reference to an instance of that array type.</span></span>

### <a name="the-systemarray-type"></a><span data-ttu-id="7bd1e-125">El tipo System.Array</span><span class="sxs-lookup"><span data-stu-id="7bd1e-125">The System.Array type</span></span>

<span data-ttu-id="7bd1e-126">El tipo `System.Array` es el tipo base abstracto de todos los tipos de matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-126">The type `System.Array` is the abstract base type of all array types.</span></span> <span data-ttu-id="7bd1e-127">Una conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) existe desde cualquier tipo de matriz a `System.Array`y una conversión explícita de referencia ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) existe desde `System.Array` a cualquier tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-127">An implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from any array type to `System.Array`, and an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `System.Array` to any array type.</span></span> <span data-ttu-id="7bd1e-128">Tenga en cuenta que `System.Array` no es un *array_type*.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-128">Note that `System.Array` is not itself an *array_type*.</span></span> <span data-ttu-id="7bd1e-129">Se trata de un *class_type* de los cuales *array_type*se derivan.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-129">Rather, it is a *class_type* from which all *array_type*s are derived.</span></span>

<span data-ttu-id="7bd1e-130">En tiempo de ejecución, un valor de tipo `System.Array` puede ser `null` o una referencia a una instancia de cualquier tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-130">At run-time, a value of type `System.Array` can be `null` or a reference to an instance of any array type.</span></span>

### <a name="arrays-and-the-generic-ilist-interface"></a><span data-ttu-id="7bd1e-131">Matrices y la interfaz IList genérica</span><span class="sxs-lookup"><span data-stu-id="7bd1e-131">Arrays and the generic IList interface</span></span>

<span data-ttu-id="7bd1e-132">Una matriz unidimensional `T[]` implementa la interfaz `System.Collections.Generic.IList<T>` (`IList<T>` para abreviar) y sus interfaces base.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-132">A one-dimensional array `T[]` implements the interface `System.Collections.Generic.IList<T>` (`IList<T>` for short) and its base interfaces.</span></span> <span data-ttu-id="7bd1e-133">En consecuencia, hay una conversión implícita de `T[]` a `IList<T>` y sus interfaces base.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-133">Accordingly, there is an implicit conversion from `T[]` to `IList<T>` and its base interfaces.</span></span> <span data-ttu-id="7bd1e-134">Además, si hay una conversión implícita de referencia de `S` a `T` , a continuación, `S[]` implementa `IList<T>` y hay una conversión implícita de referencia de `S[]` a `IList<T>` y sus interfaces base () [Conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="7bd1e-134">In addition, if there is an implicit reference conversion from `S` to `T` then `S[]` implements `IList<T>` and there is an implicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Implicit reference conversions](conversions.md#implicit-reference-conversions)).</span></span> <span data-ttu-id="7bd1e-135">Si no hay una conversión explícita de referencia de `S` a `T` , a continuación, hay una conversión explícita de referencia de `S[]` a `IList<T>` y sus interfaces base ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="7bd1e-135">If there is an explicit reference conversion from `S` to `T` then there is an explicit reference conversion from `S[]` to `IList<T>` and its base interfaces ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span> <span data-ttu-id="7bd1e-136">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-136">For example:</span></span>
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

<span data-ttu-id="7bd1e-137">La asignación `lst2 = oa1` genera un error en tiempo de compilación desde la conversión de `object[]` a `IList<string>` es una conversión explícita, no implícita.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-137">The assignment `lst2 = oa1` generates a compile-time error since the conversion from `object[]` to `IList<string>` is an explicit conversion, not implicit.</span></span> <span data-ttu-id="7bd1e-138">La conversión `(IList<string>)oa1` provocará una excepción en tiempo de ejecución desde `oa1` referencias una `object[]` y no un `string[]`.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-138">The cast `(IList<string>)oa1` will cause an exception to be thrown at run-time since `oa1` references an `object[]` and not a `string[]`.</span></span> <span data-ttu-id="7bd1e-139">Sin embargo la conversión `(IList<string>)oa2` no provocará una excepción que se produzca desde `oa2` referencias una `string[]`.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-139">However the cast `(IList<string>)oa2` will not cause an exception to be thrown since `oa2` references a `string[]`.</span></span>

<span data-ttu-id="7bd1e-140">Cada vez que hay una conversión implícita o explícita de referencia de `S[]` a `IList<T>`, también hay una conversión explícita de referencia de `IList<T>` y sus interfaces base a `S[]` ([referencia explícita las conversiones](conversions.md#explicit-reference-conversions)).</span><span class="sxs-lookup"><span data-stu-id="7bd1e-140">Whenever there is an implicit or explicit reference conversion from `S[]` to `IList<T>`, there is also an explicit reference conversion from `IList<T>` and its base interfaces to `S[]` ([Explicit reference conversions](conversions.md#explicit-reference-conversions)).</span></span>

<span data-ttu-id="7bd1e-141">Cuando un tipo de matriz `S[]` implementa `IList<T>`, algunos de los miembros de la interfaz implementada pueden producir excepciones.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-141">When an array type `S[]` implements `IList<T>`, some of the members of the implemented interface may throw exceptions.</span></span> <span data-ttu-id="7bd1e-142">El comportamiento exacto de la implementación de la interfaz queda fuera del ámbito de esta especificación.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-142">The precise behavior of the implementation of the interface is beyond the scope of this specification.</span></span>

## <a name="array-creation"></a><span data-ttu-id="7bd1e-143">Creación de una matriz</span><span class="sxs-lookup"><span data-stu-id="7bd1e-143">Array creation</span></span>

<span data-ttu-id="7bd1e-144">Se crean instancias de la matriz por *array_creation_expression*s ([expresiones de creación de matriz](expressions.md#array-creation-expressions)) o por el campo o declaraciones de variable local que incluyen un *array_initializer*([Inicializadores de matriz](arrays.md#array-initializers)).</span><span class="sxs-lookup"><span data-stu-id="7bd1e-144">Array instances are created by *array_creation_expression*s ([Array creation expressions](expressions.md#array-creation-expressions)) or by field or local variable declarations that include an *array_initializer* ([Array initializers](arrays.md#array-initializers)).</span></span>

<span data-ttu-id="7bd1e-145">Cuando se crea una instancia de matriz, el rango y la longitud de cada dimensión se establecen y, a continuación, permanecen constantes para toda la duración de la instancia.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-145">When an array instance is created, the rank and length of each dimension are established and then remain constant for the entire lifetime of the instance.</span></span> <span data-ttu-id="7bd1e-146">En otras palabras, no es posible cambiar el rango de una instancia existente de la matriz, ni es posible cambiar el tamaño de sus dimensiones.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-146">In other words, it is not possible to change the rank of an existing array instance, nor is it possible to resize its dimensions.</span></span>

<span data-ttu-id="7bd1e-147">Una instancia de matriz siempre es un tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-147">An array instance is always of an array type.</span></span> <span data-ttu-id="7bd1e-148">El `System.Array` tipo es un tipo abstracto que no pueden crearse instancias.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-148">The `System.Array` type is an abstract type that cannot be instantiated.</span></span>

<span data-ttu-id="7bd1e-149">Elementos de las matrices creadas por *array_creation_expression*s siempre se inicializan en sus valores predeterminados ([los valores predeterminados](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="7bd1e-149">Elements of arrays created by *array_creation_expression*s are always initialized to their default value ([Default values](variables.md#default-values)).</span></span>

## <a name="array-element-access"></a><span data-ttu-id="7bd1e-150">Acceso de elemento de matriz</span><span class="sxs-lookup"><span data-stu-id="7bd1e-150">Array element access</span></span>

<span data-ttu-id="7bd1e-151">Elementos de la matriz son accesibles mediante *element_access* expresiones ([matriz acceso](expressions.md#array-access)) del formulario `A[I1, I2, ..., In]`, donde `A` es una expresión de un tipo de matriz y cada `Ix` es un expresión de tipo `int`, `uint`, `long`, `ulong`, o se pueden convertir implícitamente a uno o varios de estos tipos.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-151">Array elements are accessed using *element_access* expressions ([Array access](expressions.md#array-access)) of the form `A[I1, I2, ..., In]`, where `A` is an expression of an array type and each `Ix` is an expression of type `int`, `uint`, `long`, `ulong`, or can be implicitly converted to one or more of these types.</span></span> <span data-ttu-id="7bd1e-152">El resultado de un acceso de elemento de matriz es una variable, es decir, el elemento de matriz seleccionado por los índices.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-152">The result of an array element access is a variable, namely the array element selected by the indices.</span></span>

<span data-ttu-id="7bd1e-153">Se pueden enumerar los elementos de una matriz mediante un `foreach` instrucción ([la instrucción foreach](statements.md#the-foreach-statement)).</span><span class="sxs-lookup"><span data-stu-id="7bd1e-153">The elements of an array can be enumerated using a `foreach` statement ([The foreach statement](statements.md#the-foreach-statement)).</span></span>

## <a name="array-members"></a><span data-ttu-id="7bd1e-154">Miembros de la matriz</span><span class="sxs-lookup"><span data-stu-id="7bd1e-154">Array members</span></span>

<span data-ttu-id="7bd1e-155">Cada tipo de matriz hereda los miembros declarados por el `System.Array` tipo.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-155">Every array type inherits the members declared by the `System.Array` type.</span></span>

## <a name="array-covariance"></a><span data-ttu-id="7bd1e-156">Covarianza de matrices</span><span class="sxs-lookup"><span data-stu-id="7bd1e-156">Array covariance</span></span>

<span data-ttu-id="7bd1e-157">Para dos *reference_type*s `A` y `B`, si una conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) o una conversión explícita de referencia ([ Conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) existe desde `A` a `B`, a continuación, también existe la misma conversión de referencia del tipo de matriz `A[R]` para el tipo de matriz `B[R]`, donde `R` es cualquier determinado *rank_specifier* (pero el mismo para ambos tipos de matriz).</span><span class="sxs-lookup"><span data-stu-id="7bd1e-157">For any two *reference_type*s `A` and `B`, if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) or explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from `A` to `B`, then the same reference conversion also exists from the array type `A[R]` to the array type `B[R]`, where `R` is any given *rank_specifier* (but the same for both array types).</span></span> <span data-ttu-id="7bd1e-158">Esta relación se conoce como ***covarianza de matrices***.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-158">This relationship is known as ***array covariance***.</span></span> <span data-ttu-id="7bd1e-159">Covarianza de matrices significa que en concreto, un valor de un tipo de matriz `A[R]` realmente puede ser una referencia a una instancia de un tipo de matriz `B[R]`, siempre existe una conversión implícita de referencia de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-159">Array covariance in particular means that a value of an array type `A[R]` may actually be a reference to an instance of an array type `B[R]`, provided an implicit reference conversion exists from `B` to `A`.</span></span>

<span data-ttu-id="7bd1e-160">Debido a la covarianza de matrices, las asignaciones a elementos de matrices de tipos de referencia incluyen una comprobación en tiempo de ejecución que garantiza que el valor asignado al elemento de matriz es realmente de un tipo permitido ([asignación Simple](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="7bd1e-160">Because of array covariance, assignments to elements of reference type arrays include a run-time check which ensures that the value being assigned to the array element is actually of a permitted type ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="7bd1e-161">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-161">For example:</span></span>
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

<span data-ttu-id="7bd1e-162">La asignación a `array[i]` en el `Fill` método incluye implícitamente una comprobación en tiempo de ejecución que garantiza que el objeto al que hace referencia `value` sea `null` o una instancia que es compatible con el tipo de elemento real de `array`.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-162">The assignment to `array[i]` in the `Fill` method implicitly includes a run-time check which ensures that the object referenced by `value` is either `null` or an instance that is compatible with the actual element type of `array`.</span></span> <span data-ttu-id="7bd1e-163">En `Main`, las dos primeras invocaciones de `Fill` se realiza correctamente, pero las causas de invocación tercer un `System.ArrayTypeMismatchException` que se produzca una vez ejecutada la primera asignación a `array[i]`.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-163">In `Main`, the first two invocations of `Fill` succeed, but the third invocation causes a `System.ArrayTypeMismatchException` to be thrown upon executing the first assignment to `array[i]`.</span></span> <span data-ttu-id="7bd1e-164">La excepción se produce porque una conversión boxing `int` no pueden almacenarse en un `string` matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-164">The exception occurs because a boxed `int` cannot be stored in a `string` array.</span></span>

<span data-ttu-id="7bd1e-165">Covarianza de matrices específicamente no se extiende a las matrices de *value_type*s.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-165">Array covariance specifically does not extend to arrays of *value_type*s.</span></span> <span data-ttu-id="7bd1e-166">Por ejemplo, no existe ninguna conversión que permite realizar un `int[]` a tratarse como un `object[]`.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-166">For example, no conversion exists that permits an `int[]` to be treated as an `object[]`.</span></span>

## <a name="array-initializers"></a><span data-ttu-id="7bd1e-167">Inicializadores de matriz</span><span class="sxs-lookup"><span data-stu-id="7bd1e-167">Array initializers</span></span>

<span data-ttu-id="7bd1e-168">Los inicializadores de matriz se pueden especificar en las declaraciones de campo ([campos](classes.md#fields)), las declaraciones de variable locales ([declaraciones de variable Local](statements.md#local-variable-declarations)) y las expresiones de creación de matriz ([creación de matrices las expresiones](expressions.md#array-creation-expressions)):</span><span class="sxs-lookup"><span data-stu-id="7bd1e-168">Array initializers may be specified in field declarations ([Fields](classes.md#fields)), local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), and array creation expressions ([Array creation expressions](expressions.md#array-creation-expressions)):</span></span>

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="7bd1e-169">Un inicializador de matriz consta de una secuencia de inicializadores de variables, delimitadas por "`{`"y"`}`"tokens y separados por"`,`" tokens.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-169">An array initializer consists of a sequence of variable initializers, enclosed by "`{`" and "`}`" tokens and separated by "`,`" tokens.</span></span> <span data-ttu-id="7bd1e-170">Cada inicializador de variable es una expresión o, en el caso de una matriz multidimensional, un inicializador de matriz anidados.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-170">Each variable initializer is an expression or, in the case of a multi-dimensional array, a nested array initializer.</span></span>

<span data-ttu-id="7bd1e-171">El contexto en el que se usa un inicializador de matriz determina el tipo de la matriz que se está inicializando.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-171">The context in which an array initializer is used determines the type of the array being initialized.</span></span> <span data-ttu-id="7bd1e-172">En una expresión de creación de matriz, el tipo de matriz inmediatamente precede al inicializador o se deduce de las expresiones de inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-172">In an array creation expression, the array type immediately precedes the initializer, or is inferred from the expressions in the array initializer.</span></span> <span data-ttu-id="7bd1e-173">En un campo o una declaración de variable, el tipo de matriz es el tipo del campo o variable que se declara.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-173">In a field or variable declaration, the array type is the type of the field or variable being declared.</span></span> <span data-ttu-id="7bd1e-174">Cuando se usa un inicializador de matriz en un campo o una declaración de variable, como:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-174">When an array initializer is used in a field or variable declaration, such as:</span></span>
```csharp
int[] a = {0, 2, 4, 6, 8};
```
<span data-ttu-id="7bd1e-175">es simplemente la versión abreviada de una expresión de creación de matriz equivalente:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-175">it is simply shorthand for an equivalent array creation expression:</span></span>
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

<span data-ttu-id="7bd1e-176">En una matriz unidimensional, el inicializador de matriz debe constar de una secuencia de expresiones que son compatibles con asignación del tipo de elemento de la matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-176">For a single-dimensional array, the array initializer must consist of a sequence of expressions that are assignment compatible with the element type of the array.</span></span> <span data-ttu-id="7bd1e-177">Las expresiones de inicializan los elementos de matriz en orden ascendente, empezando por el elemento en el índice cero.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-177">The expressions initialize array elements in increasing order, starting with the element at index zero.</span></span> <span data-ttu-id="7bd1e-178">El número de expresiones en el inicializador de matriz determina la longitud de la instancia de matriz que se va a crear.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-178">The number of expressions in the array initializer determines the length of the array instance being created.</span></span> <span data-ttu-id="7bd1e-179">Por ejemplo, el inicializador de matriz anterior crea un `int[]` instancia de longitud 5 y, a continuación, se inicializa la instancia con los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-179">For example, the array initializer above creates an `int[]` instance of length 5 and then initializes the instance with the following values:</span></span>
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

<span data-ttu-id="7bd1e-180">Para una matriz multidimensional, el inicializador de matriz debe tener tantos niveles de anidamiento como dimensiones de la matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-180">For a multi-dimensional array, the array initializer must have as many levels of nesting as there are dimensions in the array.</span></span> <span data-ttu-id="7bd1e-181">El nivel de anidamiento superior corresponde a la dimensión del extremo izquierdo y el nivel de anidamiento inferior se corresponde con la dimensión situada más.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-181">The outermost nesting level corresponds to the leftmost dimension and the innermost nesting level corresponds to the rightmost dimension.</span></span> <span data-ttu-id="7bd1e-182">La longitud de cada dimensión de la matriz viene determinada por el número de elementos en el nivel de anidamiento correspondiente en el inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-182">The length of each dimension of the array is determined by the number of elements at the corresponding nesting level in the array initializer.</span></span> <span data-ttu-id="7bd1e-183">Cada inicializador de matriz anidados, el número de elementos debe ser igual que los otros inicializadores de matriz del mismo nivel.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-183">For each nested array initializer, the number of elements must be the same as the other array initializers at the same level.</span></span> <span data-ttu-id="7bd1e-184">El ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-184">The example:</span></span>
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
<span data-ttu-id="7bd1e-185">crea una matriz bidimensional con una longitud de cinco para la dimensión del extremo izquierdo y una longitud de dos para la dimensión situada más:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-185">creates a two-dimensional array with a length of five for the leftmost dimension and a length of two for the rightmost dimension:</span></span>
```csharp
int[,] b = new int[5, 2];
```
<span data-ttu-id="7bd1e-186">y, a continuación, se inicializa la matriz de instancia con los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-186">and then initializes the array instance with the following values:</span></span>
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

<span data-ttu-id="7bd1e-187">Si una dimensión que no sea la más a la derecha no se especifica con longitud cero, se supone que las dimensiones subsiguientes también tiene longitud cero.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-187">If a dimension other than the rightmost is given with length zero, the subsequent dimensions are assumed to also have length zero.</span></span> <span data-ttu-id="7bd1e-188">El ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-188">The example:</span></span>
```csharp
int[,] c = {};
```
<span data-ttu-id="7bd1e-189">crea una matriz bidimensional con una longitud de cero para el extremo izquierdo y la dimensión situada más:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-189">creates a two-dimensional array with a length of zero for both the leftmost and the rightmost dimension:</span></span>
```csharp
int[,] c = new int[0, 0];
```

<span data-ttu-id="7bd1e-190">Cuando una expresión de creación de matriz incluye tanto longitudes de dimensión explícitas como un inicializador de matriz, las longitudes deben ser expresiones constantes y el número de elementos en cada nivel de anidamiento debe coincidir con la longitud de la dimensión correspondiente.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-190">When an array creation expression includes both explicit dimension lengths and an array initializer, the lengths must be constant expressions and the number of elements at each nesting level must match the corresponding dimension length.</span></span> <span data-ttu-id="7bd1e-191">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="7bd1e-191">Here are some examples:</span></span>
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

<span data-ttu-id="7bd1e-192">Aquí, el inicializador para `y` da como resultado un error en tiempo de compilación porque la expresión de longitud de la dimensión no es una constante y el inicializador para `z` da como resultado un error en tiempo de compilación porque la longitud y el número de elementos en el inicializador no coinciden.</span><span class="sxs-lookup"><span data-stu-id="7bd1e-192">Here, the initializer for `y` results in a compile-time error because the dimension length expression is not a constant, and the initializer for `z` results in a compile-time error because the length and the number of elements in the initializer do not agree.</span></span>
