# <a name="unsafe-code"></a><span data-ttu-id="0851d-101">Código no seguro</span><span class="sxs-lookup"><span data-stu-id="0851d-101">Unsafe code</span></span>

<span data-ttu-id="0851d-102">El núcleo del lenguaje C#, tal como se define en los capítulos anteriores, difiere notablemente de C y C++ en su omisión de punteros como un tipo de datos.</span><span class="sxs-lookup"><span data-stu-id="0851d-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="0851d-103">En su lugar, C# proporciona referencias y la capacidad para crear objetos que son administrados por un recolector de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="0851d-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="0851d-104">Este diseño, junto con otras características, hace que C# un lenguaje mucho más seguro que C o C++.</span><span class="sxs-lookup"><span data-stu-id="0851d-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="0851d-105">En el lenguaje básico C# no es posible tener una variable no inicializada, un puntero "pendiente" o una expresión que indiza una matriz más allá de sus límites.</span><span class="sxs-lookup"><span data-stu-id="0851d-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="0851d-106">Categorías completas de errores que suelen contaminen C y, por tanto, se eliminan los programas de C++.</span><span class="sxs-lookup"><span data-stu-id="0851d-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="0851d-107">Aunque prácticamente cualquier construcción del tipo de puntero en C o C++ tiene un tipo de referencia equivalente en C#, sin embargo, hay situaciones donde el acceso a los tipos de puntero se convierte en una necesidad.</span><span class="sxs-lookup"><span data-stu-id="0851d-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="0851d-108">Por ejemplo, interactuar con el sistema operativo subyacente, obtener acceso a un dispositivo asignado a la memoria o implementar un algoritmo puntuales puede no ser posible ni práctico sin acceso a punteros.</span><span class="sxs-lookup"><span data-stu-id="0851d-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="0851d-109">Para abordar esta necesidad, C# ofrece la posibilidad de escribir ***código no seguro***.</span><span class="sxs-lookup"><span data-stu-id="0851d-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="0851d-110">En código no seguro es posible declarar y operar con punteros, realizar conversiones entre los punteros y los tipos enteros, para tomar la dirección de las variables y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="0851d-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="0851d-111">En cierto sentido, escribir código no seguro es muy parecido a escribir código de C dentro de un programa de C#.</span><span class="sxs-lookup"><span data-stu-id="0851d-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="0851d-112">De hecho, el código no seguro es una característica "segura" desde la perspectiva de los desarrolladores y usuarios.</span><span class="sxs-lookup"><span data-stu-id="0851d-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="0851d-113">Código no seguro debe marcarse con claridad con el modificador `unsafe`, por lo que los desarrolladores no puedan usar características no seguras por accidente, y el motor de ejecución funciona para asegurarse de que no se puede ejecutar código no seguro en un entorno de confianza.</span><span class="sxs-lookup"><span data-stu-id="0851d-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="0851d-114">Contextos no seguros</span><span class="sxs-lookup"><span data-stu-id="0851d-114">Unsafe contexts</span></span>

<span data-ttu-id="0851d-115">Las características de C# no seguras solo están disponibles en contextos no seguros.</span><span class="sxs-lookup"><span data-stu-id="0851d-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="0851d-116">Se introduce un contexto no seguro mediante la inclusión de un `unsafe` modificador en la declaración de un tipo o miembro, o bien mediante un *unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="0851d-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="0851d-117">Una declaración de clase, struct, interfaz o delegado puede incluir un `unsafe` modificador, en cuyo caso toda la extensión textual de la declaración (incluido el cuerpo de la clase, estructura o interfaz) se considera un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="0851d-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="0851d-118">Una declaración de un campo, método, propiedad, evento, indizador, operador, constructor de instancia, un destructor o constructor estático puede incluir un `unsafe` modificador, en cuyo caso toda la extensión textual de la declaración del miembro se considera un no seguro contexto.</span><span class="sxs-lookup"><span data-stu-id="0851d-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="0851d-119">Un *unsafe_statement* habilita el uso de un contexto no seguro dentro de un *bloque*.</span><span class="sxs-lookup"><span data-stu-id="0851d-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="0851d-120">Toda la extensión textual del asociado *bloque* se considera un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="0851d-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="0851d-121">Las producciones gramaticales asociados se muestran a continuación.</span><span class="sxs-lookup"><span data-stu-id="0851d-121">The associated grammar productions are shown below.</span></span>

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

<span data-ttu-id="0851d-122">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0851d-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="0851d-123">el `unsafe` modificador especificado en la declaración de estructura hace que toda la extensión textual de la declaración de struct para convertirse en un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="0851d-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="0851d-124">Por lo tanto, es posible declarar el `Left` y `Right` campos de un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="0851d-125">El ejemplo anterior también podría escribirse</span><span class="sxs-lookup"><span data-stu-id="0851d-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="0851d-126">En este caso, el `unsafe` modificadores en declaraciones de campo, hacer que estas declaraciones se consideren contextos no seguros.</span><span class="sxs-lookup"><span data-stu-id="0851d-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="0851d-127">Aparte de establecer un contexto no seguro, lo que permite el uso de tipos de puntero, el `unsafe` modificador no tiene ningún efecto en un tipo o miembro.</span><span class="sxs-lookup"><span data-stu-id="0851d-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="0851d-128">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0851d-128">In the example</span></span>

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

<span data-ttu-id="0851d-129">el `unsafe` modificador en el `F` método `A` simplemente hace que la extensión textual del `F` para convertirse en un contexto no seguro en el que se pueden usar las características del lenguaje no seguras.</span><span class="sxs-lookup"><span data-stu-id="0851d-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="0851d-130">En el reemplazo de `F` en `B`, no es necesario volver a especificar la `unsafe` modificador--a menos que, por supuesto, el `F` método `B` sí necesita tener acceso a características no seguras.</span><span class="sxs-lookup"><span data-stu-id="0851d-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="0851d-131">La situación es ligeramente diferente de un tipo de puntero es parte de la firma del método</span><span class="sxs-lookup"><span data-stu-id="0851d-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

<span data-ttu-id="0851d-132">En este caso, porque `F`de firma incluye un tipo de puntero, solo se puede escribir en un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="0851d-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="0851d-133">Sin embargo, el contexto no seguro se puede introducir convirtiendo toda la clase no seguro, como sucede en `A`, o mediante la inclusión de un `unsafe` modificador en la declaración de método, como es el caso en `B`.</span><span class="sxs-lookup"><span data-stu-id="0851d-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="0851d-134">Tipos de puntero</span><span class="sxs-lookup"><span data-stu-id="0851d-134">Pointer types</span></span>

<span data-ttu-id="0851d-135">En un contexto no seguro, un *tipo* ([tipos](types.md)) puede ser un *tipo_de_puntero* , así como un *value_type* o un *reference_type* .</span><span class="sxs-lookup"><span data-stu-id="0851d-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="0851d-136">Sin embargo, un *tipo_de_puntero* también se puede usar en un `typeof` expresión ([expresiones de creación de objeto anónimo](expressions.md#anonymous-object-creation-expressions)) fuera de un contexto no seguro como tal uso no es seguro.</span><span class="sxs-lookup"><span data-stu-id="0851d-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="0851d-137">Un *tipo_de_puntero* se escribe como un *unmanaged_type* o la palabra clave `void`, seguido de un `*` token:</span><span class="sxs-lookup"><span data-stu-id="0851d-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="0851d-138">El tipo especificado antes del `*` en un puntero de tipo se denomina el ***tipo referente*** del tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="0851d-139">Representa el tipo de la variable a la que apunta un valor del tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="0851d-140">A diferencia de las referencias (valores de tipos de referencia), punteros no se realiza el seguimiento por el recolector de elementos no utilizados, el recolector de elementos no utilizados no tiene conocimiento de los punteros y los datos a los que señalan.</span><span class="sxs-lookup"><span data-stu-id="0851d-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="0851d-141">Por este motivo, un puntero no se permite para que apunte a una referencia o a una estructura que contenga referencias, y debe ser el tipo referente de un puntero un *unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="0851d-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="0851d-142">Un *unmanaged_type* es cualquier tipo que no sea un *reference_type* o tipo construido y no contiene *reference_type* o construye los campos de tipo en cualquier nivel de anidamiento.</span><span class="sxs-lookup"><span data-stu-id="0851d-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="0851d-143">En otras palabras, un *unmanaged_type* es uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="0851d-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="0851d-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, o `bool`.</span><span class="sxs-lookup"><span data-stu-id="0851d-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="0851d-145">Cualquier *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="0851d-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="0851d-146">Cualquier *tipo_de_puntero*.</span><span class="sxs-lookup"><span data-stu-id="0851d-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="0851d-147">Ninguno definido por el usuario *struct_type* que no es un tipo construido y contiene los campos de *unmanaged_type*s solo.</span><span class="sxs-lookup"><span data-stu-id="0851d-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="0851d-148">La regla intuitiva para mezclar punteros y referencias es que pueden contener punteros referentes de las referencias (objetos), pero referentes de punteros no pueden contener referencias.</span><span class="sxs-lookup"><span data-stu-id="0851d-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="0851d-149">En la tabla siguiente figuran algunos ejemplos de tipos de puntero:</span><span class="sxs-lookup"><span data-stu-id="0851d-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="0851d-150">__Ejemplo__</span><span class="sxs-lookup"><span data-stu-id="0851d-150">__Example__</span></span> | <span data-ttu-id="0851d-151">__Descripción__</span><span class="sxs-lookup"><span data-stu-id="0851d-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="0851d-152">puntero a `byte`</span><span class="sxs-lookup"><span data-stu-id="0851d-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="0851d-153">puntero a `char`</span><span class="sxs-lookup"><span data-stu-id="0851d-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="0851d-154">Puntero a puntero a `int`</span><span class="sxs-lookup"><span data-stu-id="0851d-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="0851d-155">Matriz unidimensional de punteros a `int`</span><span class="sxs-lookup"><span data-stu-id="0851d-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="0851d-156">Puntero a un tipo desconocido</span><span class="sxs-lookup"><span data-stu-id="0851d-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="0851d-157">Para una implementación determinada, todos los tipos de puntero deben tener el mismo tamaño y la representación.</span><span class="sxs-lookup"><span data-stu-id="0851d-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="0851d-158">A diferencia de C y C++, cuando se declaran varios punteros en la misma declaración, en C# el `*` se escribe junto con el tipo subyacente únicamente, no como una puntuación de prefijo en cada nombre de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="0851d-159">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="0851d-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="0851d-160">El valor de un puntero de tipo `T*` representa la dirección de una variable de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="0851d-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="0851d-161">El operador de direccionamiento indirecto del puntero `*` ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)) puede utilizarse para tener acceso a esta variable.</span><span class="sxs-lookup"><span data-stu-id="0851d-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="0851d-162">Por ejemplo, dada una variable `P` typu `int*`, la expresión `*P` denota el `int` se encontró en la dirección contenida en la variable `P`.</span><span class="sxs-lookup"><span data-stu-id="0851d-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="0851d-163">Al igual que una referencia de objeto, puede ser un puntero `null`.</span><span class="sxs-lookup"><span data-stu-id="0851d-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="0851d-164">Aplicar el operador de direccionamiento indirecto a un `null` puntero da como resultado un comportamiento definido por la implementación.</span><span class="sxs-lookup"><span data-stu-id="0851d-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="0851d-165">Un puntero con el valor `null` viene dado por todos los bits cero.</span><span class="sxs-lookup"><span data-stu-id="0851d-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="0851d-166">El `void*` tipo representa un puntero a un tipo desconocido.</span><span class="sxs-lookup"><span data-stu-id="0851d-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="0851d-167">Dado que el tipo referente es desconocido, no se puede aplicar el operador de direccionamiento indirecto a un puntero de tipo `void*`, ni puede realizarse ninguna operación aritmética en un puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="0851d-168">Sin embargo, un puntero de tipo `void*` puede convertirse a cualquier otro tipo de puntero (y viceversa).</span><span class="sxs-lookup"><span data-stu-id="0851d-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="0851d-169">Tipos de puntero son una categoría de tipos independiente.</span><span class="sxs-lookup"><span data-stu-id="0851d-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="0851d-170">A diferencia de los tipos de referencia y tipos de valor, tipos de puntero no heredan de `object` y no existe ninguna conversión entre tipos de puntero y `object`.</span><span class="sxs-lookup"><span data-stu-id="0851d-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="0851d-171">En concreto, la conversión boxing y unboxing ([conversiones Boxing y unboxing](types.md#boxing-and-unboxing)) no son compatibles con punteros.</span><span class="sxs-lookup"><span data-stu-id="0851d-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="0851d-172">Sin embargo, se permiten las conversiones entre diferentes tipos de puntero y entre tipos de puntero y los tipos enteros.</span><span class="sxs-lookup"><span data-stu-id="0851d-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="0851d-173">Esto se describe en [conversiones de puntero](unsafe-code.md#pointer-conversions).</span><span class="sxs-lookup"><span data-stu-id="0851d-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="0851d-174">Un *tipo_de_puntero* no se puede usar como un argumento de tipo ([construye los tipos](types.md#constructed-types)) e inferencia ([inferencia de tipo](expressions.md#type-inference)) se produce un error en las llamadas de método genérico que habría deduce un tipo de argumento sea un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="0851d-175">Un *tipo_de_puntero* puede utilizarse como el tipo de un campo volátil ([campos volátiles](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="0851d-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="0851d-176">Aunque se pueden pasar punteros como `ref` o `out` parámetros, si lo hace, por lo que puede provocar un comportamiento indefinido, ya que el puntero puede bien establecerse para que apunte a una variable local que ya no existe cuando se devuelve el método llamado, o el objeto fijo para la TI usa para apuntar, ya no es fijo.</span><span class="sxs-lookup"><span data-stu-id="0851d-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="0851d-177">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0851d-177">For example:</span></span>

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

<span data-ttu-id="0851d-178">Un método puede devolver un valor de algún tipo, y ese tipo puede ser un puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="0851d-179">Por ejemplo, cuando recibe un puntero a una secuencia contigua de `int`s, número de elementos de la secuencia y algunos otros `int` valor, el método siguiente devuelve la dirección de ese valor en la secuencia, si se produce una coincidencia; de lo contrario, devuelve `null`:</span><span class="sxs-lookup"><span data-stu-id="0851d-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

<span data-ttu-id="0851d-180">En un contexto no seguro, existen varias construcciones para operar con punteros:</span><span class="sxs-lookup"><span data-stu-id="0851d-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="0851d-181">El `*` operador puede usarse para realizar el direccionamiento indirecto del puntero ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)).</span><span class="sxs-lookup"><span data-stu-id="0851d-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="0851d-182">El `->` operador puede utilizarse para tener acceso a un miembro de un struct a través de un puntero ([acceso a miembros de puntero](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="0851d-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="0851d-183">El `[]` operador puede utilizarse para indizar un puntero ([acceso de elemento de puntero](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="0851d-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="0851d-184">El `&` operador se puede utilizar para obtener la dirección de una variable ([el operador address-of](unsafe-code.md#the-address-of-operator)).</span><span class="sxs-lookup"><span data-stu-id="0851d-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="0851d-185">El `++` y `--` operadores pueden utilizarse para incrementar y disminuir punteros ([puntero incremento y decremento](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="0851d-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="0851d-186">El `+` y `-` operadores se pueden utilizar para realizar la aritmética de puntero ([aritmética de puntero](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="0851d-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="0851d-187">El `==`, `!=`, `<`, `>`, `<=`, y `=>` operadores pueden utilizarse para comparar punteros ([comparación de punteros](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="0851d-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="0851d-188">El `stackalloc` operador se puede utilizar para asignar memoria de la pila de llamadas ([búferes de tamaño fijo](unsafe-code.md#fixed-size-buffers)).</span><span class="sxs-lookup"><span data-stu-id="0851d-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="0851d-189">El `fixed` instrucción puede usarse para fijar temporalmente una variable se puede obtener su dirección ([la instrucción fixed](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="0851d-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="0851d-190">Variables fijas y móviles</span><span class="sxs-lookup"><span data-stu-id="0851d-190">Fixed and moveable variables</span></span>

<span data-ttu-id="0851d-191">El operador address-of ([el operador address-of](unsafe-code.md#the-address-of-operator)) y el `fixed` instrucción ([la instrucción fixed](unsafe-code.md#the-fixed-statement)) dividen las variables en dos categorías: ***Se ha corregido variables*** y ***variables móviles***.</span><span class="sxs-lookup"><span data-stu-id="0851d-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="0851d-192">Variables fijas residen en ubicaciones de almacenamiento que se ven afectadas por la operación del recolector de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="0851d-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="0851d-193">(Ejemplos de variables fijas incluyen variables locales, parámetros de valor y las variables creadas por desreferenciar punteros). Por otro lado, variables móviles residen en ubicaciones de almacenamiento que están sujetos a reubicación o eliminación por el recolector de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="0851d-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="0851d-194">(Ejemplos de variables móviles incluyen campos de objetos y elementos de matrices).</span><span class="sxs-lookup"><span data-stu-id="0851d-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="0851d-195">El `&` operador ([el operador address-of](unsafe-code.md#the-address-of-operator)) permite obtener la dirección de una variable fija sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="0851d-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="0851d-196">Sin embargo, dado que una variable móvil está sujeta a reubicación o eliminación por el recolector de elementos no utilizados, la dirección de una variable móvil solo se puede obtener mediante una `fixed` instrucción ([la instrucción fixed](unsafe-code.md#the-fixed-statement)) y esa dirección solo es válido durante el tiempo que dure que `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="0851d-197">En términos precisos, una variable fija es uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="0851d-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="0851d-198">Una variable resultante de un *simple_name* ([nombres simples](expressions.md#simple-names)) que hace referencia a una variable local o un parámetro de valor, a menos que la variable capturada por una función anónima.</span><span class="sxs-lookup"><span data-stu-id="0851d-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="0851d-199">Una variable resultante de un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `V.I`, donde `V` es una variable fija de un *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="0851d-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="0851d-200">Una variable resultante de un *pointer_indirection_expression* ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)) del formulario `*P`, un *pointer_member_access* ([Acceso a miembros de puntero](unsafe-code.md#pointer-member-access)) del formulario `P->I`, o un *pointer_element_access* ([acceso de elemento de puntero](unsafe-code.md#pointer-element-access)) del formulario `P[E]`.</span><span class="sxs-lookup"><span data-stu-id="0851d-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="0851d-201">Todas las demás variables se clasifican como variables móviles.</span><span class="sxs-lookup"><span data-stu-id="0851d-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="0851d-202">Tenga en cuenta que un campo estático se clasifica como una variable móvil.</span><span class="sxs-lookup"><span data-stu-id="0851d-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="0851d-203">Tenga en cuenta también que un `ref` o `out` parámetro se clasifica como una variable móvil, incluso si el argumento proporcionado para el parámetro es una variable fija.</span><span class="sxs-lookup"><span data-stu-id="0851d-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="0851d-204">Por último, tenga en cuenta que una variable generada por desreferencia un puntero siempre se clasifica como una variable fija.</span><span class="sxs-lookup"><span data-stu-id="0851d-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="0851d-205">Conversiones de puntero</span><span class="sxs-lookup"><span data-stu-id="0851d-205">Pointer conversions</span></span>

<span data-ttu-id="0851d-206">En un contexto no seguro, el conjunto de conversiones implícitas disponibles ([conversiones implícitas](conversions.md#implicit-conversions)) se extiende para incluir las siguientes conversiones implícitas de puntero:</span><span class="sxs-lookup"><span data-stu-id="0851d-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="0851d-207">Desde cualquier *tipo_de_puntero* al tipo `void*`.</span><span class="sxs-lookup"><span data-stu-id="0851d-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="0851d-208">Desde el `null` literal a cualquier *tipo_de_puntero*.</span><span class="sxs-lookup"><span data-stu-id="0851d-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="0851d-209">Además, en un contexto no seguro, el conjunto de conversiones explícitas disponibles ([las conversiones explícitas](conversions.md#explicit-conversions)) se extiende para incluir las siguientes conversiones explícitas de puntero:</span><span class="sxs-lookup"><span data-stu-id="0851d-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="0851d-210">Desde cualquier *tipo_de_puntero* a cualquier otro *tipo_de_puntero*.</span><span class="sxs-lookup"><span data-stu-id="0851d-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="0851d-211">Desde `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, o `ulong` a cualquier *tipo_de_puntero*.</span><span class="sxs-lookup"><span data-stu-id="0851d-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="0851d-212">Desde cualquier *tipo_de_puntero* a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, o `ulong`.</span><span class="sxs-lookup"><span data-stu-id="0851d-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="0851d-213">Por último, en un contexto no seguro, el conjunto de conversiones implícitas estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) incluye la siguiente conversión de puntero:</span><span class="sxs-lookup"><span data-stu-id="0851d-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="0851d-214">Desde cualquier *tipo_de_puntero* al tipo `void*`.</span><span class="sxs-lookup"><span data-stu-id="0851d-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="0851d-215">Conversiones entre los dos tipos de puntero nunca cambian el valor de puntero real.</span><span class="sxs-lookup"><span data-stu-id="0851d-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="0851d-216">En otras palabras, una conversión de un tipo de puntero a la otra tiene ningún efecto en la dirección subyacente dada por el puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="0851d-217">Cuando un tipo de puntero se convierte a otro, si aún no está correctamente alineado para el tipo al que apunta el puntero resultante, el comportamiento es indefinido si se desreferencia el resultado.</span><span class="sxs-lookup"><span data-stu-id="0851d-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="0851d-218">En general, el concepto "correctamente alineado" es transitivo: si un puntero al tipo `A` está correctamente alineado para que un puntero al tipo `B`, que, a su vez, está correctamente alineado para que un puntero al tipo `C`, a continuación, un puntero a tipo `A`está correctamente alineado para que un puntero al tipo `C`.</span><span class="sxs-lookup"><span data-stu-id="0851d-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="0851d-219">Tenga en cuenta lo siguiente en el que se tiene acceso a una variable de un tipo determinado a través de un puntero a un tipo diferente:</span><span class="sxs-lookup"><span data-stu-id="0851d-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="0851d-220">Cuando un tipo de puntero se convierte en un puntero a byte, el resultado apunta al byte dirigido más bajo de la variable.</span><span class="sxs-lookup"><span data-stu-id="0851d-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="0851d-221">Incrementos sucesivos del resultado, hasta alcanzar el tamaño de la variable, dan como resultado punteros a los bytes restantes de esa variable.</span><span class="sxs-lookup"><span data-stu-id="0851d-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="0851d-222">Por ejemplo, el siguiente método muestra cada uno de los ocho bytes en un valor doble como un valor hexadecimal:</span><span class="sxs-lookup"><span data-stu-id="0851d-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="0851d-223">Por supuesto, el resultado producido depende endian.</span><span class="sxs-lookup"><span data-stu-id="0851d-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="0851d-224">Las asignaciones entre los punteros y los enteros son definido por la implementación.</span><span class="sxs-lookup"><span data-stu-id="0851d-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="0851d-225">Sin embargo, en 32 \* y arquitecturas de CPU de 64 bits con un espacio de direcciones lineales, conversiones de punteros a o desde tipos integrales normalmente se comportan exactamente igual que las conversiones de `uint` o `ulong` valores, respectivamente, a o desde esos tipos integrales.</span><span class="sxs-lookup"><span data-stu-id="0851d-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="0851d-226">Matrices de puntero</span><span class="sxs-lookup"><span data-stu-id="0851d-226">Pointer arrays</span></span>

<span data-ttu-id="0851d-227">En un contexto no seguro, se pueden construir matrices de punteros.</span><span class="sxs-lookup"><span data-stu-id="0851d-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="0851d-228">Sólo algunas de las conversiones que se aplican a otros tipos de matriz se permiten en las matrices de puntero:</span><span class="sxs-lookup"><span data-stu-id="0851d-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="0851d-229">La conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) desde cualquier *array_type* a `System.Array` y las interfaces que implementa también se aplica a las matrices de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="0851d-230">Sin embargo, cualquier intento de obtener acceso a los elementos de matriz a través de `System.Array` o las interfaces que implementa, se producirán una excepción en tiempo de ejecución, como tipos de puntero no son convertibles a `object`.</span><span class="sxs-lookup"><span data-stu-id="0851d-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="0851d-231">Las conversiones de referencia de implícitas y explícitas ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions), [conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) de un tipo de matriz unidimensional `S[]` a `System.Collections.Generic.IList<T>` y sus interfaces base genéricos nunca se aplican a las matrices de puntero, ya que los tipos de puntero no se puede usar como argumentos de tipo y no hay ninguna conversión entre tipos de puntero a tipos que no son de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="0851d-232">La conversión de referencia explícita ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) desde `System.Array` y las interfaces que implementa a cualquier *array_type* se aplica a las matrices de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="0851d-233">Las conversiones de referencia explícita ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) desde `System.Collections.Generic.IList<S>` y sus interfaces base a un tipo de matriz unidimensional `T[]` nunca se aplica a las matrices de puntero, ya que no pueden ser tipos de puntero usar como argumentos de tipo y no hay ninguna conversión entre tipos de puntero a tipos que no son de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="0851d-234">Estas restricciones indican que la expansión para el `foreach` instrucción a través de las matrices se describe en [la instrucción foreach](statements.md#the-foreach-statement) no se puede aplicar a las matrices de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="0851d-235">En su lugar, una instrucción foreach del formulario</span><span class="sxs-lookup"><span data-stu-id="0851d-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="0851d-236">donde el tipo de `x` es un tipo de matriz del formulario `T[,,...,]`, `N` es el número de dimensiones menos 1 y `T` o `V` es un tipo de puntero, se expande con bucles for anidados como sigue:</span><span class="sxs-lookup"><span data-stu-id="0851d-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

<span data-ttu-id="0851d-237">Las variables `a`, `i0`, `i1`,..., `iN` no está visible o accesible a `x` o *embedded_statement* o cualquier otro código fuente del programa.</span><span class="sxs-lookup"><span data-stu-id="0851d-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="0851d-238">La variable `v` es de solo lectura en la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="0851d-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="0851d-239">Si no hay una conversión explícita ([conversiones de puntero](unsafe-code.md#pointer-conversions)) desde `T` (el tipo de elemento) para `V`, se genera un error y no se toma ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="0851d-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="0851d-240">Si `x` tiene el valor `null`, un `System.NullReferenceException` se produce en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="0851d-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="0851d-241">Punteros en expresiones</span><span class="sxs-lookup"><span data-stu-id="0851d-241">Pointers in expressions</span></span>

<span data-ttu-id="0851d-242">En un contexto no seguro, una expresión puede producir un resultado de un tipo de puntero, pero fuera de un contexto no seguro es un error en tiempo de compilación para una expresión de un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="0851d-243">En términos precisos, fuera de un contexto no seguro durante la compilación se produce un error si cualquier *simple_name* ([nombres simples](expressions.md#simple-names)), *member_access* ([acceso a miembros ](expressions.md#member-access)), *invocation_expression* ([expresiones de invocación](expressions.md#invocation-expressions)), o *element_access* ([acceso de elemento](expressions.md#element-access)) es un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="0851d-244">En un contexto no seguro, el *primary_no_array_creation_expression* ([expresiones primarias](expressions.md#primary-expressions)) y *unary_expression* ([losoperadoresunarios](expressions.md#unary-operators)) producciones permiten las siguientes construcciones adicionales:</span><span class="sxs-lookup"><span data-stu-id="0851d-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

<span data-ttu-id="0851d-245">Estas construcciones se describen en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="0851d-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="0851d-246">La prioridad y asociatividad de los operadores no seguros está implícito en la gramática.</span><span class="sxs-lookup"><span data-stu-id="0851d-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="0851d-247">Direccionamiento indirecto del puntero</span><span class="sxs-lookup"><span data-stu-id="0851d-247">Pointer indirection</span></span>

<span data-ttu-id="0851d-248">Un *pointer_indirection_expression* consta de un asterisco (`*`) seguido por un *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="0851d-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="0851d-249">El operador unario `*` operador denota el direccionamiento indirecto del puntero y se utiliza para obtener la variable a la que señala un puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="0851d-250">El resultado de evaluar `*P`, donde `P` es una expresión de un tipo de puntero `T*`, es una variable de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="0851d-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="0851d-251">Es un error en tiempo de compilación para aplicar el operador unario `*` operador en una expresión de tipo `void*` o en una expresión que no sea de un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="0851d-252">El efecto de aplicar el operador unario `*` operador para un `null` puntero está definido por la implementación.</span><span class="sxs-lookup"><span data-stu-id="0851d-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="0851d-253">En concreto, no hay ninguna garantía de que esta operación produce un `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="0851d-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="0851d-254">Si se ha asignado un valor no válido para el puntero, el comportamiento de unario `*` operador no está definido.</span><span class="sxs-lookup"><span data-stu-id="0851d-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="0851d-255">Entre los valores no válidos para desreferenciar un puntero unario `*` operadores tienen una dirección incorrectamente alineada para el tipo señalado (vea el ejemplo de [conversiones de puntero](unsafe-code.md#pointer-conversions)) y la dirección de una variable después de la final de su duración.</span><span class="sxs-lookup"><span data-stu-id="0851d-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="0851d-256">Para fines de análisis de asignación definitiva, una variable producida al evaluar una expresión de formato `*P` se considera asignado inicialmente ([variables asignadas inicialmente](variables.md#initially-assigned-variables)).</span><span class="sxs-lookup"><span data-stu-id="0851d-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="0851d-257">Acceso a miembros de puntero</span><span class="sxs-lookup"><span data-stu-id="0851d-257">Pointer member access</span></span>

<span data-ttu-id="0851d-258">Un *pointer_member_access* consta de un *primary_expression*, seguido de un "`->`" símbolo (token), seguido de un *identificador* y un opcional*type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="0851d-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="0851d-259">En un acceso de miembro de puntero del formulario `P->I`, `P` debe ser una expresión de un tipo de puntero distinto `void*`, y `I` debe denotar un miembro accesible del tipo al que `P` puntos.</span><span class="sxs-lookup"><span data-stu-id="0851d-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="0851d-260">Un acceso de miembro de puntero de la forma `P->I` se evalúa exactamente como `(*P).I`.</span><span class="sxs-lookup"><span data-stu-id="0851d-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="0851d-261">Para obtener una descripción del operador de direccionamiento indirecto del puntero (`*`), consulte [direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="0851d-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="0851d-262">Para obtener una descripción del operador de acceso a miembros (`.`), consulte [acceso a miembros](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="0851d-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="0851d-263">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0851d-263">In the example</span></span>

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

<span data-ttu-id="0851d-264">el `->` operador se usa para tener acceso a campos e invocar un método de un struct a través de un puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="0851d-265">Dado que la operación `P->I` equivale exactamente a `(*P).I`, el `Main` método podría igual de bien se han escrito:</span><span class="sxs-lookup"><span data-stu-id="0851d-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a><span data-ttu-id="0851d-266">Acceso de elemento de puntero</span><span class="sxs-lookup"><span data-stu-id="0851d-266">Pointer element access</span></span>

<span data-ttu-id="0851d-267">Un *pointer_element_access* consta de un *primary_no_array_creation_expression* seguido de una expresión encerrada entre "`[`"y"`]`".</span><span class="sxs-lookup"><span data-stu-id="0851d-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="0851d-268">En un acceso de elemento de puntero de la forma `P[E]`, `P` debe ser una expresión de un tipo de puntero distinto `void*`, y `E` debe ser una expresión que se puede convertir implícitamente a `int`, `uint`, `long`, o `ulong`.</span><span class="sxs-lookup"><span data-stu-id="0851d-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="0851d-269">Un acceso de elemento de puntero de la forma `P[E]` se evalúa exactamente como `*(P + E)`.</span><span class="sxs-lookup"><span data-stu-id="0851d-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="0851d-270">Para obtener una descripción del operador de direccionamiento indirecto del puntero (`*`), consulte [direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="0851d-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="0851d-271">Para obtener una descripción del operador de suma de puntero (`+`), consulte [aritmética de puntero](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="0851d-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="0851d-272">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0851d-272">In the example</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

<span data-ttu-id="0851d-273">un acceso de elemento de puntero se usa para inicializar el búfer de caracteres en un `for` bucle.</span><span class="sxs-lookup"><span data-stu-id="0851d-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="0851d-274">Dado que la operación `P[E]` equivale exactamente a `*(P + E)`, en el ejemplo se podría igual de bien se han escrito:</span><span class="sxs-lookup"><span data-stu-id="0851d-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

<span data-ttu-id="0851d-275">El operador de acceso de elemento de puntero no comprueba fuera de los límites de errores y el comportamiento al obtener acceso a un fuera de los límites elemento no está definido.</span><span class="sxs-lookup"><span data-stu-id="0851d-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="0851d-276">Esto es lo mismo que C y C++.</span><span class="sxs-lookup"><span data-stu-id="0851d-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="0851d-277">El operador address-of</span><span class="sxs-lookup"><span data-stu-id="0851d-277">The address-of operator</span></span>

<span data-ttu-id="0851d-278">Un *addressof_expression* consta de una y comercial (`&`) seguido por un *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="0851d-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="0851d-279">Dada una expresión `E` que es de un tipo `T` y se clasifica como una variable fija ([fijos y variables móviles](unsafe-code.md#fixed-and-moveable-variables)), la construcción `&E` calcula la dirección de la variable proporcionada por `E`.</span><span class="sxs-lookup"><span data-stu-id="0851d-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="0851d-280">El tipo del resultado es `T*` y se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="0851d-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="0851d-281">Se produce un error de tiempo de compilación si `E` no está clasificado como una variable, si `E` se clasifica como una variable local de solo lectura, o si `E` denota una variable móvil.</span><span class="sxs-lookup"><span data-stu-id="0851d-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="0851d-282">En el último caso, una instrucción fixed ([la instrucción fixed](unsafe-code.md#the-fixed-statement)) se puede usar para "corregir" temporalmente la variable antes de obtener su dirección.</span><span class="sxs-lookup"><span data-stu-id="0851d-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="0851d-283">Como se indicó en [acceso a miembros](expressions.md#member-access), fuera de un constructor de instancia o un constructor estático de una clase o estructura que define un `readonly` campo, dicho campo se considera un valor, no una variable.</span><span class="sxs-lookup"><span data-stu-id="0851d-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="0851d-284">Por lo tanto, no se pueden tomar su dirección.</span><span class="sxs-lookup"><span data-stu-id="0851d-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="0851d-285">De forma similar, no se pueden tomar la dirección de una constante.</span><span class="sxs-lookup"><span data-stu-id="0851d-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="0851d-286">El `&` operador no requiere que esté asignado definitivamente, pero después su argumento una `&` operación, la variable al que se aplica el operador se considera asignada definitivamente en la ruta de acceso de ejecución en el que se produce la operación.</span><span class="sxs-lookup"><span data-stu-id="0851d-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="0851d-287">Es responsabilidad del programador asegurarse de que la inicialización correcta de la variable en realidad tienen lugar en esta situación.</span><span class="sxs-lookup"><span data-stu-id="0851d-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="0851d-288">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0851d-288">In the example</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

<span data-ttu-id="0851d-289">`i` se considera asignado definitivamente siguiendo el `&i` operación utilizada para inicializar `p`.</span><span class="sxs-lookup"><span data-stu-id="0851d-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="0851d-290">La asignación a `*p` vigente inicializa `i`, pero la inclusión de esta inicialización es responsabilidad del programador y no se producirá ningún error de tiempo de compilación si se quitó la asignación.</span><span class="sxs-lookup"><span data-stu-id="0851d-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="0851d-291">Las reglas de asignación definitiva para el `&` operador existe tal que se puede evitar la inicialización redundante de variables locales.</span><span class="sxs-lookup"><span data-stu-id="0851d-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="0851d-292">Por ejemplo, muchas de las API externas toman un puntero a una estructura que se rellena mediante la API.</span><span class="sxs-lookup"><span data-stu-id="0851d-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="0851d-293">Las llamadas a estas API suelen pasan la dirección de una variable local, y sin la regla, se requeriría una inicialización redundante de la variable.</span><span class="sxs-lookup"><span data-stu-id="0851d-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="0851d-294">Puntero incremento y decremento</span><span class="sxs-lookup"><span data-stu-id="0851d-294">Pointer increment and decrement</span></span>

<span data-ttu-id="0851d-295">En un contexto no seguro, el `++` y `--` operadores ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) y [prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)) se pueden aplicar a puntero las variables de todos los tipos excepto `void*`.</span><span class="sxs-lookup"><span data-stu-id="0851d-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="0851d-296">Por lo tanto, para cada tipo de puntero `T*`, implícitamente se definen los siguientes operadores:</span><span class="sxs-lookup"><span data-stu-id="0851d-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="0851d-297">Los operadores producen los mismos resultados que `x + 1` y `x - 1`, respectivamente ([aritmética de puntero](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="0851d-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="0851d-298">En otras palabras, para una variable de puntero de tipo `T*`, el `++` operador agrega `sizeof(T)` a la dirección contenida en la variable y el `--` operador resta `sizeof(T)` desde la dirección contenida en la variable.</span><span class="sxs-lookup"><span data-stu-id="0851d-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="0851d-299">Si un puntero de incremento o decremento operación desborda el dominio del tipo de puntero, el resultado es definido por la implementación, pero no se producen excepciones.</span><span class="sxs-lookup"><span data-stu-id="0851d-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="0851d-300">Aritmética de puntero</span><span class="sxs-lookup"><span data-stu-id="0851d-300">Pointer arithmetic</span></span>

<span data-ttu-id="0851d-301">En un contexto no seguro, el `+` y `-` operadores ([operador de suma](expressions.md#addition-operator) y [operador de resta](expressions.md#subtraction-operator)) se pueden aplicar a los valores de todos los tipos de puntero excepto `void*`.</span><span class="sxs-lookup"><span data-stu-id="0851d-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="0851d-302">Por lo tanto, para cada tipo de puntero `T*`, implícitamente se definen los siguientes operadores:</span><span class="sxs-lookup"><span data-stu-id="0851d-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

<span data-ttu-id="0851d-303">Dada una expresión `P` de un tipo de puntero `T*` y una expresión `N` typu `int`, `uint`, `long`, o `ulong`, las expresiones `P + N` y `N + P` calcular el valor de puntero de tipo `T*` resultante de agregar `N * sizeof(T)` a la dirección proporcionada por `P`.</span><span class="sxs-lookup"><span data-stu-id="0851d-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="0851d-304">Del mismo modo, la expresión `P - N` calcula el valor de puntero de tipo `T*` que da como resultado de restar `N * sizeof(T)` desde la dirección proporcionada por `P`.</span><span class="sxs-lookup"><span data-stu-id="0851d-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="0851d-305">Dadas dos expresiones, `P` y `Q`, de un tipo de puntero `T*`, la expresión `P - Q` calcula la diferencia entre las direcciones proporcionadas por `P` y `Q` y, a continuación, divide la diferencia por `sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="0851d-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="0851d-306">El tipo del resultado siempre es `long`.</span><span class="sxs-lookup"><span data-stu-id="0851d-306">The type of the result is always `long`.</span></span> <span data-ttu-id="0851d-307">De hecho, `P - Q` se calcula como `((long)(P) - (long)(Q)) / sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="0851d-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="0851d-308">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0851d-308">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

<span data-ttu-id="0851d-309">lo que genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="0851d-309">which produces the output:</span></span>

```
p - q = -14
q - p = 14
```

<span data-ttu-id="0851d-310">Si una operación aritmética de puntero desborda el dominio del tipo de puntero, el resultado se trunca de forma definido por la implementación, pero no se producen excepciones.</span><span class="sxs-lookup"><span data-stu-id="0851d-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="0851d-311">Comparación de punteros</span><span class="sxs-lookup"><span data-stu-id="0851d-311">Pointer comparison</span></span>

<span data-ttu-id="0851d-312">En un contexto no seguro, el `==`, `!=`, `<`, `>`, `<=`, y `=>` operadores ([operadores de comprobación de tipos y relacionales](expressions.md#relational-and-type-testing-operators)) se pueden aplicar a los valores de todos los tipos de puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="0851d-313">Los operadores de comparación de puntero son:</span><span class="sxs-lookup"><span data-stu-id="0851d-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="0851d-314">Ya existe una conversión implícita de cualquier tipo de puntero a la `void*` se puedan comparar el tipo, los operandos de cualquier tipo de puntero mediante estos operadores.</span><span class="sxs-lookup"><span data-stu-id="0851d-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="0851d-315">Los operadores de comparación comparan las direcciones proporcionadas por los dos operandos, como si fueran enteros sin signo.</span><span class="sxs-lookup"><span data-stu-id="0851d-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="0851d-316">El operador sizeof</span><span class="sxs-lookup"><span data-stu-id="0851d-316">The sizeof operator</span></span>

<span data-ttu-id="0851d-317">El `sizeof` operador devuelve el número de bytes ocupados por una variable de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="0851d-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="0851d-318">El tipo como operando para `sizeof` debe ser un *unmanaged_type* ([tipos de puntero](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="0851d-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="0851d-319">El resultado de la `sizeof` operador es un valor de tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="0851d-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="0851d-320">Predefinidos para determinados tipos, el `sizeof` operador produce un valor constante, como se muestra en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="0851d-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="0851d-321">__Expresión__</span><span class="sxs-lookup"><span data-stu-id="0851d-321">__Expression__</span></span>   | <span data-ttu-id="0851d-322">__Resultado__</span><span class="sxs-lookup"><span data-stu-id="0851d-322">__Result__</span></span> |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

<span data-ttu-id="0851d-323">Para los demás tipos, el resultado de la `sizeof` operador define la implementación y se clasifica como un valor, no como una constante.</span><span class="sxs-lookup"><span data-stu-id="0851d-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="0851d-324">No se especifica el orden en que los miembros se empaquetan en un struct.</span><span class="sxs-lookup"><span data-stu-id="0851d-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="0851d-325">Para la alineación, puede haber relleno al principio de una estructura, dentro de una estructura y al final de la estructura sin nombre.</span><span class="sxs-lookup"><span data-stu-id="0851d-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="0851d-326">El contenido de los bits utilizados como relleno es indeterminado.</span><span class="sxs-lookup"><span data-stu-id="0851d-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="0851d-327">Cuando se aplica a un operando que tiene el tipo de estructura, el resultado es el número total de bytes en una variable de ese tipo, incluido el relleno.</span><span class="sxs-lookup"><span data-stu-id="0851d-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="0851d-328">La instrucción fixed</span><span class="sxs-lookup"><span data-stu-id="0851d-328">The fixed statement</span></span>

<span data-ttu-id="0851d-329">En un contexto no seguro, el *embedded_statement* ([instrucciones](statements.md)) producción permite una construcción adicional, la `fixed` instrucción, que se utiliza una variable móvil "corregir" que su dirección permanece constante para la duración de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

<span data-ttu-id="0851d-330">Cada *fixed_pointer_declarator* declara una variable local de la dada *tipo_de_puntero* e inicializa la variable local con la dirección calculada por el correspondiente *fixed_ pointer_initializer*.</span><span class="sxs-lookup"><span data-stu-id="0851d-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="0851d-331">Una variable local declarada en un `fixed` es accesible en cualquier instrucción *fixed_pointer_initializer*s que se producen a la derecha de la declaración de variable y, en el *embedded_statement* de la `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="0851d-332">Una variable local declarada por un `fixed` instrucción se considera de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="0851d-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="0851d-333">Se produce un error de tiempo de compilación si la instrucción incrustada intenta modificar esta variable local (a través de la asignación o el `++` y `--` operadores) o pasarla como un `ref` o `out` parámetro.</span><span class="sxs-lookup"><span data-stu-id="0851d-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="0851d-334">Un *fixed_pointer_initializer* puede ser uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="0851d-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="0851d-335">El token "`&`" seguido por un *variable_reference* ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) a una variable móvil ([fijos y variables móviles](unsafe-code.md#fixed-and-moveable-variables)) de un tipo no administrado `T`, siempre que el tipo `T*` es implícitamente convertible al tipo de puntero proporcionado el `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="0851d-336">En este caso, el inicializador calcula la dirección de la variable dada, y se garantiza que la variable va a permanecer en una dirección fija durante la duración de la `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="0851d-337">Una expresión de un *array_type* con elementos de un tipo no administrado `T`, siempre que el tipo `T*` es implícitamente convertible al tipo de puntero proporcionado el `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="0851d-338">En este caso, el inicializador calcula la dirección del primer elemento de la matriz, y se garantiza que toda la matriz permanecerá en una dirección fija durante el tiempo que dure la `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="0851d-339">Si la expresión de matriz es null o si la matriz tiene cero elementos, el inicializador calcula un igual de dirección a cero.</span><span class="sxs-lookup"><span data-stu-id="0851d-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="0851d-340">Una expresión de tipo `string`, siempre que el tipo `char*` es implícitamente convertible al tipo de puntero proporcionado el `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="0851d-341">En este caso, el inicializador calcula la dirección del primer carácter en la cadena y se garantiza que toda la cadena permanecerá en una dirección fija durante el tiempo que dure la `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="0851d-342">El comportamiento de la `fixed` instrucción se define en la implementación si la expresión de cadena es null.</span><span class="sxs-lookup"><span data-stu-id="0851d-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="0851d-343">Un *simple_name* o *member_access* que hace referencia a un miembro de búfer de tamaño fijo de una variable móvil, siempre que el tipo del miembro de búfer de tamaño fijo es implícitamente convertible al tipo de puntero proporcionado en el `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="0851d-344">En este caso, el inicializador calcula un puntero al primer elemento del búfer de tamaño fijo ([búferes de tamaño fijo en expresiones](unsafe-code.md#fixed-size-buffers-in-expressions)), y se garantiza que el búfer de tamaño fijo permanecerá en una dirección fija durante el tiempo que dure la `fixed`instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="0851d-345">Para cada dirección calculada por un *fixed_pointer_initializer* el `fixed` instrucción garantiza que la variable al que hace referencia la dirección no está sujeta a reubicación o eliminación por el recolector de elementos no utilizados para la duración de la `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="0851d-346">Por ejemplo, si la dirección calculada por un *fixed_pointer_initializer* hace referencia a un campo de un objeto o un elemento de una instancia de matriz, el `fixed` instrucción garantiza que la instancia del objeto contenedor no se reubica o eliminado durante la vigencia de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="0851d-347">Es responsabilidad del programador garantizar que los punteros crean por `fixed` instrucciones no permanecen tras la ejecución de esas instrucciones.</span><span class="sxs-lookup"><span data-stu-id="0851d-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="0851d-348">Por ejemplo, cuando punteros crean por `fixed` instrucciones se pasan a las API externas, es responsabilidad del programador asegurarse de que las API no retienen memoria de estos punteros.</span><span class="sxs-lookup"><span data-stu-id="0851d-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="0851d-349">Objetos fijos pueden provocar la fragmentación del montón (porque no se pueden mover).</span><span class="sxs-lookup"><span data-stu-id="0851d-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="0851d-350">Por ese motivo, se deben corregir los objetos solo cuando sea absolutamente necesario y, a continuación, solo por el menor tiempo posible.</span><span class="sxs-lookup"><span data-stu-id="0851d-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="0851d-351">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="0851d-351">The example</span></span>

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

<span data-ttu-id="0851d-352">muestra varios usos de la `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="0851d-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="0851d-353">La primera instrucción fija y obtiene la dirección de un campo estático, la segunda instrucción fija y obtiene la dirección de un campo de instancia y la tercera instrucción fija y obtiene la dirección de un elemento de matriz.</span><span class="sxs-lookup"><span data-stu-id="0851d-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="0851d-354">En cada caso habría sido un error usar el normal `&` operador dado que las variables se clasifican como variables móviles.</span><span class="sxs-lookup"><span data-stu-id="0851d-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="0851d-355">El cuarto `fixed` instrucción en el ejemplo anterior genera un resultado similar a la tercera.</span><span class="sxs-lookup"><span data-stu-id="0851d-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="0851d-356">En este ejemplo de la `fixed` instrucción usa `string`:</span><span class="sxs-lookup"><span data-stu-id="0851d-356">This example of the `fixed` statement uses `string`:</span></span>

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

<span data-ttu-id="0851d-357">En un contexto no seguro se almacenan los elementos de la matriz de matrices unidimensionales en orden creciente de índice, empezando por el índice `0` y terminando con el índice `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="0851d-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="0851d-358">Para matrices multidimensionales, la matriz se almacenan los elementos que aumentan en primer lugar, los índices de la dimensión situada más a continuación, la próxima deja la dimensión, y así sucesivamente hacia la izquierda.</span><span class="sxs-lookup"><span data-stu-id="0851d-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="0851d-359">Dentro de un `fixed` instrucción que obtiene un puntero `p` a una instancia de matriz `a`, los valores de puntero que abarcan desde `p` a `p + a.Length - 1` representan las direcciones de los elementos de la matriz.</span><span class="sxs-lookup"><span data-stu-id="0851d-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="0851d-360">Del mismo modo, las variables que van desde `p[0]` a `p[a.Length - 1]` representan los elementos de matriz real.</span><span class="sxs-lookup"><span data-stu-id="0851d-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="0851d-361">Dada la manera en que se almacenan las matrices, se puede tratar una matriz de cualquier dimensión como si fuese lineal.</span><span class="sxs-lookup"><span data-stu-id="0851d-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="0851d-362">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0851d-362">For example:</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

<span data-ttu-id="0851d-363">lo que genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="0851d-363">which produces the output:</span></span>

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="0851d-364">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0851d-364">In the example</span></span>

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

<span data-ttu-id="0851d-365">un `fixed` instrucción se utiliza para resolver una matriz para que su dirección se puede pasar a un método que toma un puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="0851d-366">En el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0851d-366">In the example:</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

<span data-ttu-id="0851d-367">una instrucción fixed se utiliza para resolver un búfer de tamaño fijo de un struct para que su dirección puede usarse como un puntero.</span><span class="sxs-lookup"><span data-stu-id="0851d-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="0851d-368">Un `char*` valor generado mediante la corrección de una instancia de cadena siempre apunta a una cadena terminada en null.</span><span class="sxs-lookup"><span data-stu-id="0851d-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="0851d-369">Dentro de una instrucción fixed que obtiene un puntero `p` a una instancia de cadena `s`, los valores de puntero que abarcan desde `p` a `p + s.Length - 1` representan las direcciones de los caracteres en la cadena y el valor de puntero `p + s.Length` siempre apunta a un carácter nulo (el carácter con el valor `'\0'`).</span><span class="sxs-lookup"><span data-stu-id="0851d-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="0851d-370">Modificación de objetos de tipo administrado mediante punteros fijos puede tener como resultado un comportamiento indefinido.</span><span class="sxs-lookup"><span data-stu-id="0851d-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="0851d-371">Por ejemplo, dado que las cadenas son inmutables, es responsabilidad del programador asegurarse de que no se modifican los caracteres que se hace referencia a un puntero a una cadena fija.</span><span class="sxs-lookup"><span data-stu-id="0851d-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="0851d-372">La terminación null automática de cadenas es especialmente conveniente al llamar a las API externas que esperan cadenas de "estilo C".</span><span class="sxs-lookup"><span data-stu-id="0851d-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="0851d-373">Sin embargo, tenga en cuenta que una instancia de la cadena puede contener caracteres null.</span><span class="sxs-lookup"><span data-stu-id="0851d-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="0851d-374">Si existen dichos caracteres null, la cadena aparecerá truncada cuando se trata como terminada en null `char*`.</span><span class="sxs-lookup"><span data-stu-id="0851d-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="0851d-375">Búferes de tamaño fijo</span><span class="sxs-lookup"><span data-stu-id="0851d-375">Fixed size buffers</span></span>

<span data-ttu-id="0851d-376">Búferes de tamaño fijo se utilizan para declarar matrices en línea de "Estilo C" como miembros de estructuras y son útiles principalmente para interactuar con las API no administradas.</span><span class="sxs-lookup"><span data-stu-id="0851d-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="0851d-377">Declaraciones de búfer de tamaño fijo</span><span class="sxs-lookup"><span data-stu-id="0851d-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="0851d-378">Un ***búfer de tamaño fijo*** es un miembro que representa el almacenamiento para un búfer de longitud fija de variables de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="0851d-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="0851d-379">Una declaración de búfer de tamaño fijo introduce uno o varios búferes de tamaño fijo de un tipo de elemento especificado.</span><span class="sxs-lookup"><span data-stu-id="0851d-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="0851d-380">Búferes de tamaño fijo solo se permiten en declaraciones de estructura y solo puede aparecer en contextos no seguros ([contextos no seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="0851d-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

<span data-ttu-id="0851d-381">Una declaración de búfer de tamaño fijo puede incluir un conjunto de atributos ([atributos](attributes.md)), un `new` modificador ([modificadores](classes.md#modifiers)), una combinación válida de los cuatro modificadores de acceso ([tipo parámetros y restricciones](classes.md#type-parameters-and-constraints)) y un `unsafe` modificador ([contextos no seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="0851d-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="0851d-382">Los atributos y modificadores se aplican a todos los miembros declarados en la declaración del búfer de tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="0851d-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="0851d-383">Es un error para el mismo modificador aparezca varias veces en una declaración de búfer de tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="0851d-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="0851d-384">No se permite una declaración de búfer de tamaño fijo para incluir el `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="0851d-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="0851d-385">El tipo de elemento de búfer de una declaración de búfer de tamaño fijo especifica el tipo de elemento de los búferes introducidos por la declaración.</span><span class="sxs-lookup"><span data-stu-id="0851d-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="0851d-386">El tipo de elemento de búfer debe ser uno de los tipos predefinidos `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, o `bool`.</span><span class="sxs-lookup"><span data-stu-id="0851d-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="0851d-387">El tipo de elemento de búfer es seguido por una lista de declaradores de búfer de tamaño fijo, cada uno de los cuales incluye a un nuevo miembro.</span><span class="sxs-lookup"><span data-stu-id="0851d-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="0851d-388">Un declarador de búfer de tamaño fijo consta de un identificador que designa el miembro, seguido de una expresión constante entre `[` y `]` tokens.</span><span class="sxs-lookup"><span data-stu-id="0851d-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="0851d-389">La expresión constante denota el número de elementos en el miembro introducidos por esa declarador de búfer de tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="0851d-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="0851d-390">El tipo de la expresión constante debe ser implícitamente convertible al tipo `int`, y el valor debe ser un entero positivo distinto de cero.</span><span class="sxs-lookup"><span data-stu-id="0851d-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="0851d-391">Se garantiza que los elementos de un búfer de tamaño fijo se disponen secuencialmente en la memoria.</span><span class="sxs-lookup"><span data-stu-id="0851d-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="0851d-392">Una declaración de búfer de tamaño fijo que declara varios búferes de tamaño fijo es equivalente a varias declaraciones de una declaración de búfer de tamaño fijo solo con los mismos atributos y tipos de elemento.</span><span class="sxs-lookup"><span data-stu-id="0851d-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="0851d-393">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="0851d-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="0851d-394">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="0851d-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="0851d-395">Búferes de tamaño fijo en expresiones</span><span class="sxs-lookup"><span data-stu-id="0851d-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="0851d-396">Búsqueda de miembros ([operadores](expressions.md#operators)) de un tamaño fijo, miembro de búfer procede exactamente igual que la búsqueda de miembros de un campo.</span><span class="sxs-lookup"><span data-stu-id="0851d-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="0851d-397">Se puede hacer referencia a un búfer de tamaño fijo en una expresión que utiliza un *simple_name* ([inferencia](expressions.md#type-inference)) o un *member_access* ([comprobación de tiempo de compilación la resolución de sobrecarga dinámica](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="0851d-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="0851d-398">Cuando se hace referencia a un miembro de búfer de tamaño fijo como un nombre simple, el efecto es el mismo que un acceso de miembro del formulario `this.I`, donde `I` es el miembro de búfer de tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="0851d-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="0851d-399">En un acceso de miembro del formulario `E.I`si `E` es de un tipo struct y una búsqueda de miembros de `I` en que el tipo de estructura identifica un miembro de tamaño fijo, a continuación, `E.I` es evalúa y clasifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="0851d-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="0851d-400">Si la expresión `E.I` no se produce en un contexto no seguro, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0851d-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="0851d-401">Si `E` se clasifica como un valor, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0851d-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="0851d-402">De lo contrario, si `E` es una variable móvil ([fijos y variables móviles](unsafe-code.md#fixed-and-moveable-variables)) y la expresión `E.I` no es un *fixed_pointer_initializer* ([fijo instrucción](unsafe-code.md#the-fixed-statement)), se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0851d-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="0851d-403">En caso contrario, `E` hace referencia a una variable fija y el resultado de la expresión es un puntero al primer elemento del miembro de búfer de tamaño fijo `I` en `E`.</span><span class="sxs-lookup"><span data-stu-id="0851d-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="0851d-404">El resultado es de tipo `S*`, donde `S` es el tipo de elemento de `I`y se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="0851d-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="0851d-405">Pueden tener acceso a los elementos subsiguientes del búfer de tamaño fijo con operaciones de puntero desde el primer elemento.</span><span class="sxs-lookup"><span data-stu-id="0851d-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="0851d-406">A diferencia de tener acceso a las matrices, acceso a los elementos de un búfer de tamaño fijo es una operación no segura y no está activada de intervalo.</span><span class="sxs-lookup"><span data-stu-id="0851d-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="0851d-407">El ejemplo siguiente declara y utiliza una estructura con un miembro de búfer de tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="0851d-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a><span data-ttu-id="0851d-408">Comprobación de asignación definitiva</span><span class="sxs-lookup"><span data-stu-id="0851d-408">Definite assignment checking</span></span>

<span data-ttu-id="0851d-409">Búferes de tamaño fijo no están sujetos a comprobación de asignación definitiva ([asignación definitiva](variables.md#definite-assignment)), y se omiten los miembros de búfer de tamaño fijo para los fines de comprobación de las variables de tipo de estructura de asignación definitiva.</span><span class="sxs-lookup"><span data-stu-id="0851d-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="0851d-410">Cuando la variable de estructura contenedora más externo de un miembro de búfer de tamaño fijo es una variable estática, una variable de instancia de una instancia de clase o un elemento de matriz, los elementos del búfer de tamaño fijo se inicializan automáticamente en sus valores predeterminados ([Los valores predeterminados](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="0851d-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="0851d-411">En todos los demás casos, el contenido inicial de un búfer de tamaño fijo es indefinido.</span><span class="sxs-lookup"><span data-stu-id="0851d-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="0851d-412">Asignación de pila</span><span class="sxs-lookup"><span data-stu-id="0851d-412">Stack allocation</span></span>

<span data-ttu-id="0851d-413">En un contexto no seguro, una declaración de variable local ([declaraciones de variable Local](statements.md#local-variable-declarations)) puede incluir un inicializador de asignación de pila que asigne memoria de la pila de llamadas.</span><span class="sxs-lookup"><span data-stu-id="0851d-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="0851d-414">El *unmanaged_type* indica el tipo de los elementos que se almacenarán en la ubicación recién asignada, y el *expresión* indica el número de estos elementos.</span><span class="sxs-lookup"><span data-stu-id="0851d-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="0851d-415">En conjunto, estos especifican el tamaño de asignación necesarios.</span><span class="sxs-lookup"><span data-stu-id="0851d-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="0851d-416">Puesto que el tamaño de una asignación de pila no puede ser negativo, es un error en tiempo de compilación para especificar el número de elementos como un *constant_expression* que se evalúa como un valor negativo.</span><span class="sxs-lookup"><span data-stu-id="0851d-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="0851d-417">Un inicializador de asignación de pila del formulario `stackalloc T[E]` requiere `T` sea un tipo no administrado ([tipos de puntero](unsafe-code.md#pointer-types)) y `E` sea una expresión de tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="0851d-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="0851d-418">La construcción asigna `E * sizeof(T)` bytes a partir de la llamada de la pila y devuelve un puntero de tipo `T*`, en el bloque recién asignado.</span><span class="sxs-lookup"><span data-stu-id="0851d-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="0851d-419">Si `E` es un valor negativo, el comportamiento es indefinido.</span><span class="sxs-lookup"><span data-stu-id="0851d-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="0851d-420">Si `E` es cero, a continuación, no se realiza ninguna asignación, y el puntero devuelto está definido por la implementación.</span><span class="sxs-lookup"><span data-stu-id="0851d-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="0851d-421">Si no hay suficiente memoria disponible para asignar un bloque de tamaño determinado, un `System.StackOverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="0851d-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="0851d-422">El contenido de la memoria recién asignada es indefinido.</span><span class="sxs-lookup"><span data-stu-id="0851d-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="0851d-423">No se permiten inicializadores de asignación de pila en `catch` o `finally` bloques ([la instrucción try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="0851d-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="0851d-424">No hay ninguna manera de liberar explícitamente la memoria asignada mediante `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="0851d-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="0851d-425">Todos los bloques de memoria asignado a la pila creados durante la ejecución de un miembro de función se descartan automáticamente cuando se devuelve ese miembro de función.</span><span class="sxs-lookup"><span data-stu-id="0851d-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="0851d-426">Esto corresponde a la `alloca` (función), una extensión que se suelen encontrar en las implementaciones de C y C++.</span><span class="sxs-lookup"><span data-stu-id="0851d-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="0851d-427">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0851d-427">In the example</span></span>

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

<span data-ttu-id="0851d-428">un `stackalloc` inicializador se utiliza en el `IntToString` método para asignar un búfer de 16 caracteres en la pila.</span><span class="sxs-lookup"><span data-stu-id="0851d-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="0851d-429">El búfer se descarta automáticamente cuando el método devuelve.</span><span class="sxs-lookup"><span data-stu-id="0851d-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="0851d-430">Asignación de memoria dinámica</span><span class="sxs-lookup"><span data-stu-id="0851d-430">Dynamic memory allocation</span></span>

<span data-ttu-id="0851d-431">Excepto para el `stackalloc` operador, C# no proporciona construcciones predefinidas para la administración de memoria de elementos no utilizados no recopilado.</span><span class="sxs-lookup"><span data-stu-id="0851d-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="0851d-432">Dichos servicios normalmente proporciona bibliotecas de clases auxiliares o importar directamente desde el sistema operativo subyacente.</span><span class="sxs-lookup"><span data-stu-id="0851d-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="0851d-433">Por ejemplo, el `Memory` clase siguiente muestra cómo se pueden tener acceso a las funciones del montón de un sistema operativo subyacente de C#:</span><span class="sxs-lookup"><span data-stu-id="0851d-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

<span data-ttu-id="0851d-434">Un ejemplo que utiliza el `Memory` clase se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="0851d-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

<span data-ttu-id="0851d-435">El ejemplo asigna 256 bytes de memoria a través de `Memory.Alloc` e inicializa el bloque de memoria con valores entre 0 y 255.</span><span class="sxs-lookup"><span data-stu-id="0851d-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="0851d-436">A continuación, asigna una matriz de bytes de 256 elementos y utiliza `Memory.Copy` para copiar el contenido del bloque de memoria en la matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="0851d-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="0851d-437">Por último, se libera el bloque de memoria mediante `Memory.Free` y salida en la consola el contenido de la matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="0851d-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
