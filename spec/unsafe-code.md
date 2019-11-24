---
ms.openlocfilehash: dbea611280a644adc25247b9887986e129c59b68
ms.sourcegitcommit: a5e393b018b04dfa55aae0000290ca087b508495
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2019
ms.locfileid: "72310359"
---
# <a name="unsafe-code"></a><span data-ttu-id="d9237-101">Código no seguro</span><span class="sxs-lookup"><span data-stu-id="d9237-101">Unsafe code</span></span>

<span data-ttu-id="d9237-102">El lenguaje C# principal, tal como se define en los capítulos anteriores, difiere principalmente de C++ C y en su omisión de punteros como un tipo de datos.</span><span class="sxs-lookup"><span data-stu-id="d9237-102">The core C# language, as defined in the preceding chapters, differs notably from C and C++ in its omission of pointers as a data type.</span></span> <span data-ttu-id="d9237-103">En su lugar C# , proporciona referencias y la capacidad de crear objetos administrados por un recolector de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="d9237-103">Instead, C# provides references and the ability to create objects that are managed by a garbage collector.</span></span> <span data-ttu-id="d9237-104">Este diseño, junto con otras características, proporciona C# un lenguaje mucho más seguro que C C++o.</span><span class="sxs-lookup"><span data-stu-id="d9237-104">This design, coupled with other features, makes C# a much safer language than C or C++.</span></span> <span data-ttu-id="d9237-105">En el lenguaje C# principal, simplemente no es posible tener una variable no inicializada, un puntero "pendiente" o una expresión que indexa una matriz más allá de sus límites.</span><span class="sxs-lookup"><span data-stu-id="d9237-105">In the core C# language it is simply not possible to have an uninitialized variable, a "dangling" pointer, or an expression that indexes an array beyond its bounds.</span></span> <span data-ttu-id="d9237-106">Por lo tanto, se eliminan todas las categorías C++ de errores que normalmente plagan C y programas.</span><span class="sxs-lookup"><span data-stu-id="d9237-106">Whole categories of bugs that routinely plague C and C++ programs are thus eliminated.</span></span>

<span data-ttu-id="d9237-107">Aunque prácticamente todas las construcciones de tipo de puntero C++ de C o tienen un tipo C#de referencia homólogo en, sin embargo, hay situaciones en las que el acceso a los tipos de puntero se convierte en una necesidad.</span><span class="sxs-lookup"><span data-stu-id="d9237-107">While practically every pointer type construct in C or C++ has a reference type counterpart in C#, nonetheless, there are situations where access to pointer types becomes a necessity.</span></span> <span data-ttu-id="d9237-108">Por ejemplo, la interacción con el sistema operativo subyacente, el acceso a un dispositivo asignado a la memoria o la implementación de un algoritmo crítico en el tiempo puede no ser posible ni práctico sin acceso a los punteros.</span><span class="sxs-lookup"><span data-stu-id="d9237-108">For example, interfacing with the underlying operating system, accessing a memory-mapped device, or implementing a time-critical algorithm may not be possible or practical without access to pointers.</span></span> <span data-ttu-id="d9237-109">Para satisfacer esta necesidad, C# proporciona la capacidad de escribir ***código no seguro***.</span><span class="sxs-lookup"><span data-stu-id="d9237-109">To address this need, C# provides the ability to write ***unsafe code***.</span></span>

<span data-ttu-id="d9237-110">En código no seguro es posible declarar y operar en punteros, para realizar conversiones entre punteros y tipos enteros, para tomar la dirección de las variables, etc.</span><span class="sxs-lookup"><span data-stu-id="d9237-110">In unsafe code it is possible to declare and operate on pointers, to perform conversions between pointers and integral types, to take the address of variables, and so forth.</span></span> <span data-ttu-id="d9237-111">En cierto sentido, escribir código no seguro es muy similar a escribir código de C C# dentro de un programa.</span><span class="sxs-lookup"><span data-stu-id="d9237-111">In a sense, writing unsafe code is much like writing C code within a C# program.</span></span>

<span data-ttu-id="d9237-112">En realidad, el código no seguro es una característica "segura" desde la perspectiva de los desarrolladores y los usuarios.</span><span class="sxs-lookup"><span data-stu-id="d9237-112">Unsafe code is in fact a "safe" feature from the perspective of both developers and users.</span></span> <span data-ttu-id="d9237-113">El código no seguro debe estar claramente marcado con el modificador `unsafe`, por lo que los desarrolladores no pueden usar las características no seguras accidentalmente, y el motor de ejecución funciona para asegurarse de que el código no seguro no se puede ejecutar en un entorno que no es de confianza.</span><span class="sxs-lookup"><span data-stu-id="d9237-113">Unsafe code must be clearly marked with the modifier `unsafe`, so developers can't possibly use unsafe features accidentally, and the execution engine works to ensure that unsafe code cannot be executed in an untrusted environment.</span></span>

## <a name="unsafe-contexts"></a><span data-ttu-id="d9237-114">Contextos no seguros</span><span class="sxs-lookup"><span data-stu-id="d9237-114">Unsafe contexts</span></span>

<span data-ttu-id="d9237-115">Las características no seguras de C# solo están disponibles en contextos no seguros.</span><span class="sxs-lookup"><span data-stu-id="d9237-115">The unsafe features of C# are available only in unsafe contexts.</span></span> <span data-ttu-id="d9237-116">Un contexto no seguro se introduce mediante la inclusión de un modificador `unsafe` en la declaración de un tipo o miembro, o mediante la utilización de un *unsafe_statement*:</span><span class="sxs-lookup"><span data-stu-id="d9237-116">An unsafe context is introduced by including an `unsafe` modifier in the declaration of a type or member, or by employing an *unsafe_statement*:</span></span>

*  <span data-ttu-id="d9237-117">Una declaración de una clase, un struct, una interfaz o un delegado puede incluir un modificador `unsafe`, en cuyo caso toda la extensión textual de esa declaración de tipos (incluido el cuerpo de la clase, el struct o la interfaz) se considera un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="d9237-117">A declaration of a class, struct, interface, or delegate may include an `unsafe` modifier, in which case the entire textual extent of that type declaration (including the body of the class, struct, or interface) is considered an unsafe context.</span></span>
*  <span data-ttu-id="d9237-118">Una declaración de un campo, un método, una propiedad, un evento, un indexador, un operador, un constructor de instancia, un destructor o un constructor estático puede incluir un modificador `unsafe`, en cuyo caso toda la extensión textual de dicha declaración de miembro se considera un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="d9237-118">A declaration of a field, method, property, event, indexer, operator, instance constructor, destructor, or static constructor may include an `unsafe` modifier, in which case the entire textual extent of that member declaration is considered an unsafe context.</span></span>
*  <span data-ttu-id="d9237-119">Un *unsafe_statement* habilita el uso de un contexto no seguro dentro de un *bloque*.</span><span class="sxs-lookup"><span data-stu-id="d9237-119">An *unsafe_statement* enables the use of an unsafe context within a *block*.</span></span> <span data-ttu-id="d9237-120">Toda la extensión textual del *bloque* asociado se considera un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="d9237-120">The entire textual extent of the associated *block* is considered an unsafe context.</span></span>

<span data-ttu-id="d9237-121">A continuación se muestran las producciones de gramática asociadas.</span><span class="sxs-lookup"><span data-stu-id="d9237-121">The associated grammar productions are shown below.</span></span>

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

<span data-ttu-id="d9237-122">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9237-122">In the example</span></span>

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

<span data-ttu-id="d9237-123">el modificador `unsafe` especificado en la declaración de struct hace que toda la extensión textual de la declaración de struct se convierta en un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="d9237-123">the `unsafe` modifier specified in the struct declaration causes the entire textual extent of the struct declaration to become an unsafe context.</span></span> <span data-ttu-id="d9237-124">Por lo tanto, es posible declarar los campos `Left` y `Right` para que sean de un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-124">Thus, it is possible to declare the `Left` and `Right` fields to be of a pointer type.</span></span> <span data-ttu-id="d9237-125">También se puede escribir el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="d9237-125">The example above could also be written</span></span>

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

<span data-ttu-id="d9237-126">Aquí, los modificadores de `unsafe` en las declaraciones de campo hacen que esas declaraciones se consideren contextos no seguros.</span><span class="sxs-lookup"><span data-stu-id="d9237-126">Here, the `unsafe` modifiers in the field declarations cause those declarations to be considered unsafe contexts.</span></span>

<span data-ttu-id="d9237-127">Además de establecer un contexto no seguro, lo que permite el uso de tipos de puntero, el modificador `unsafe` no tiene ningún efecto en un tipo o miembro.</span><span class="sxs-lookup"><span data-stu-id="d9237-127">Other than establishing an unsafe context, thus permitting the use of pointer types, the `unsafe` modifier has no effect on a type or a member.</span></span> <span data-ttu-id="d9237-128">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9237-128">In the example</span></span>

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

<span data-ttu-id="d9237-129">el modificador `unsafe` en el método `F` de `A` simplemente hace que la extensión textual de `F` se convierta en un contexto no seguro en el que se pueden usar las características no seguras del lenguaje.</span><span class="sxs-lookup"><span data-stu-id="d9237-129">the `unsafe` modifier on the `F` method in `A` simply causes the textual extent of `F` to become an unsafe context in which the unsafe features of the language can be used.</span></span> <span data-ttu-id="d9237-130">En la invalidación de `F` en `B`, no es necesario volver a especificar el modificador `unsafe`, a menos que, por supuesto, el método `F` de `B` mismo necesite tener acceso a características no seguras.</span><span class="sxs-lookup"><span data-stu-id="d9237-130">In the override of `F` in `B`, there is no need to re-specify the `unsafe` modifier -- unless, of course, the `F` method in `B` itself needs access to unsafe features.</span></span>

<span data-ttu-id="d9237-131">La situación es ligeramente diferente cuando un tipo de puntero forma parte de la Signatura del método.</span><span class="sxs-lookup"><span data-stu-id="d9237-131">The situation is slightly different when a pointer type is part of the method's signature</span></span>

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

<span data-ttu-id="d9237-132">Aquí, como la firma de `F`incluye un tipo de puntero, solo se puede escribir en un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="d9237-132">Here, because `F`'s signature includes a pointer type, it can only be written in an unsafe context.</span></span> <span data-ttu-id="d9237-133">Sin embargo, el contexto no seguro se puede introducir haciendo que la clase completa sea no segura, como es el caso de `A`, o incluyendo un modificador `unsafe` en la declaración del método, como es el caso en `B`.</span><span class="sxs-lookup"><span data-stu-id="d9237-133">However, the unsafe context can be introduced by either making the entire class unsafe, as is the case in `A`, or by including an `unsafe` modifier in the method declaration, as is the case in `B`.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="d9237-134">Tipos de puntero</span><span class="sxs-lookup"><span data-stu-id="d9237-134">Pointer types</span></span>

<span data-ttu-id="d9237-135">En un contexto no seguro, un *tipo* ([tipos](types.md)) puede ser un *pointer_type* , así como un *value_type* o un *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="d9237-135">In an unsafe context, a *type* ([Types](types.md)) may be a *pointer_type* as well as a *value_type* or a *reference_type*.</span></span> <span data-ttu-id="d9237-136">Sin embargo, un *pointer_type* también puede usarse en una expresión de `typeof` ([expresiones de creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions)) fuera de un contexto no seguro, ya que dicho uso no es seguro.</span><span class="sxs-lookup"><span data-stu-id="d9237-136">However, a *pointer_type* may also be used in a `typeof` expression ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) outside of an unsafe context as such usage is not unsafe.</span></span>

```antlr
type_unsafe
    : pointer_type
    ;
```

<span data-ttu-id="d9237-137">Un *pointer_type* se escribe como un *unmanaged_type* o la palabra clave `void`, seguido de un token de `*`:</span><span class="sxs-lookup"><span data-stu-id="d9237-137">A *pointer_type* is written as an *unmanaged_type* or the keyword `void`, followed by a `*` token:</span></span>

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

<span data-ttu-id="d9237-138">El tipo especificado antes del `*` en un tipo de puntero se denomina ***tipo referente*** del tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-138">The type specified before the `*` in a pointer type is called the ***referent type*** of the pointer type.</span></span> <span data-ttu-id="d9237-139">Representa el tipo de la variable a la que apunta un valor del tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-139">It represents the type of the variable to which a value of the pointer type points.</span></span>

<span data-ttu-id="d9237-140">A diferencia de las referencias (valores de tipos de referencia), el recolector de elementos no utilizados no realiza un seguimiento de los punteros; el recolector de elementos no utilizados no tiene conocimiento de los punteros y los datos a los que apuntan.</span><span class="sxs-lookup"><span data-stu-id="d9237-140">Unlike references (values of reference types), pointers are not tracked by the garbage collector -- the garbage collector has no knowledge of pointers and the data to which they point.</span></span> <span data-ttu-id="d9237-141">Por esta razón, no se permite que un puntero señale a una referencia o a un struct que contenga referencias, y el tipo referente de un puntero debe ser un *unmanaged_type*.</span><span class="sxs-lookup"><span data-stu-id="d9237-141">For this reason a pointer is not permitted to point to a reference or to a struct that contains references, and the referent type of a pointer must be an *unmanaged_type*.</span></span>

<span data-ttu-id="d9237-142">Un *unmanaged_type* es cualquier tipo que no sea un tipo *reference_type* o construido y que no contenga campos de tipo *reference_type* o construido en ningún nivel de anidamiento.</span><span class="sxs-lookup"><span data-stu-id="d9237-142">An *unmanaged_type* is any type that isn't a *reference_type* or constructed type, and doesn't contain *reference_type* or constructed type fields at any level of nesting.</span></span> <span data-ttu-id="d9237-143">En otras palabras, una *unmanaged_type* es una de las siguientes:</span><span class="sxs-lookup"><span data-stu-id="d9237-143">In other words, an *unmanaged_type* is one of the following:</span></span>

*  <span data-ttu-id="d9237-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`o `bool`.</span><span class="sxs-lookup"><span data-stu-id="d9237-144">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`.</span></span>
*  <span data-ttu-id="d9237-145">Cualquier *enum_type*.</span><span class="sxs-lookup"><span data-stu-id="d9237-145">Any *enum_type*.</span></span>
*  <span data-ttu-id="d9237-146">Cualquier *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="d9237-146">Any *pointer_type*.</span></span>
*  <span data-ttu-id="d9237-147">Cualquier *struct_type* definido por el usuario que no sea un tipo construido y que contenga campos de *unmanaged_type*s solamente.</span><span class="sxs-lookup"><span data-stu-id="d9237-147">Any user-defined *struct_type* that is not a constructed type and contains fields of *unmanaged_type*s only.</span></span>

<span data-ttu-id="d9237-148">La regla intuitiva para la combinación de punteros y referencias es que los referentes de referencias (objetos) pueden contener punteros, pero los referentes de punteros no pueden contener referencias.</span><span class="sxs-lookup"><span data-stu-id="d9237-148">The intuitive rule for mixing of pointers and references is that referents of references (objects) are permitted to contain pointers, but referents of pointers are not permitted to contain references.</span></span>

<span data-ttu-id="d9237-149">En la tabla siguiente se proporcionan algunos ejemplos de tipos de puntero:</span><span class="sxs-lookup"><span data-stu-id="d9237-149">Some examples of pointer types are given in the table below:</span></span>

| <span data-ttu-id="d9237-150">__Ejemplo__</span><span class="sxs-lookup"><span data-stu-id="d9237-150">__Example__</span></span> | <span data-ttu-id="d9237-151">__Descripción__</span><span class="sxs-lookup"><span data-stu-id="d9237-151">__Description__</span></span>                               |
|-------------|-----------------------------------------------|
| `byte*`     | <span data-ttu-id="d9237-152">Puntero a `byte`</span><span class="sxs-lookup"><span data-stu-id="d9237-152">Pointer to `byte`</span></span>                             |
| `char*`     | <span data-ttu-id="d9237-153">Puntero a `char`</span><span class="sxs-lookup"><span data-stu-id="d9237-153">Pointer to `char`</span></span>                             |
| `int**`     | <span data-ttu-id="d9237-154">Puntero al puntero a `int`</span><span class="sxs-lookup"><span data-stu-id="d9237-154">Pointer to pointer to `int`</span></span>                   |
| `int*[]`    | <span data-ttu-id="d9237-155">Matriz unidimensional de punteros a `int`</span><span class="sxs-lookup"><span data-stu-id="d9237-155">Single-dimensional array of pointers to `int`</span></span> |
| `void*`     | <span data-ttu-id="d9237-156">Puntero a un tipo desconocido</span><span class="sxs-lookup"><span data-stu-id="d9237-156">Pointer to unknown type</span></span>                       |

<span data-ttu-id="d9237-157">Para una implementación determinada, todos los tipos de puntero deben tener el mismo tamaño y representación.</span><span class="sxs-lookup"><span data-stu-id="d9237-157">For a given implementation, all pointer types must have the same size and representation.</span></span>

<span data-ttu-id="d9237-158">A diferencia de C C++y, cuando se declaran varios punteros en la misma C# declaración, en el `*` se escribe solo con el tipo subyacente, no como un signo de puntuación de prefijo en cada nombre de puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-158">Unlike C and C++, when multiple pointers are declared in the same declaration, in C# the `*` is written along with the underlying type only, not as a prefix punctuator on each pointer name.</span></span> <span data-ttu-id="d9237-159">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9237-159">For example</span></span>

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

<span data-ttu-id="d9237-160">El valor de un puntero que tiene el tipo `T*` representa la dirección de una variable de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="d9237-160">The value of a pointer having type `T*` represents the address of a variable of type `T`.</span></span> <span data-ttu-id="d9237-161">El operador de direccionamiento indirecto de puntero `*` ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)) se puede usar para tener acceso a esta variable.</span><span class="sxs-lookup"><span data-stu-id="d9237-161">The pointer indirection operator `*` ([Pointer indirection](unsafe-code.md#pointer-indirection)) may be used to access this variable.</span></span> <span data-ttu-id="d9237-162">Por ejemplo, dada una variable `P` de tipo `int*`, la expresión `*P` denota la variable de `int` que se encuentra en la dirección contenida en `P`.</span><span class="sxs-lookup"><span data-stu-id="d9237-162">For example, given a variable `P` of type `int*`, the expression `*P` denotes the `int` variable found at the address contained in `P`.</span></span>

<span data-ttu-id="d9237-163">Al igual que una referencia a un objeto, se puede `null`un puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-163">Like an object reference, a pointer may be `null`.</span></span> <span data-ttu-id="d9237-164">Si se aplica el operador de direccionamiento indirecto a un puntero `null`, se producirá un comportamiento definido por la implementación.</span><span class="sxs-lookup"><span data-stu-id="d9237-164">Applying the indirection operator to a `null` pointer results in implementation-defined behavior.</span></span> <span data-ttu-id="d9237-165">Un puntero con valor `null` se representa mediante All-bits-cero.</span><span class="sxs-lookup"><span data-stu-id="d9237-165">A pointer with value `null` is represented by all-bits-zero.</span></span>

<span data-ttu-id="d9237-166">El tipo de `void*` representa un puntero a un tipo desconocido.</span><span class="sxs-lookup"><span data-stu-id="d9237-166">The `void*` type represents a pointer to an unknown type.</span></span> <span data-ttu-id="d9237-167">Dado que el tipo referente es desconocido, el operador de direccionamiento indirecto no se puede aplicar a un puntero de tipo `void*`, ni se puede realizar ninguna operación aritmética en dicho puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-167">Because the referent type is unknown, the indirection operator cannot be applied to a pointer of type `void*`, nor can any arithmetic be performed on such a pointer.</span></span> <span data-ttu-id="d9237-168">Sin embargo, un puntero de tipo `void*` se puede convertir a cualquier otro tipo de puntero (y viceversa).</span><span class="sxs-lookup"><span data-stu-id="d9237-168">However, a pointer of type `void*` can be cast to any other pointer type (and vice versa).</span></span>

<span data-ttu-id="d9237-169">Los tipos de puntero son una categoría independiente de tipos.</span><span class="sxs-lookup"><span data-stu-id="d9237-169">Pointer types are a separate category of types.</span></span> <span data-ttu-id="d9237-170">A diferencia de los tipos de referencia y los tipos de valor, los tipos de puntero no heredan de `object` y no existen conversiones entre los tipos de puntero y `object`.</span><span class="sxs-lookup"><span data-stu-id="d9237-170">Unlike reference types and value types, pointer types do not inherit from `object` and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="d9237-171">En concreto, no se admiten las conversiones boxing y unboxing ([conversión boxing y unboxing](types.md#boxing-and-unboxing)) para los punteros.</span><span class="sxs-lookup"><span data-stu-id="d9237-171">In particular, boxing and unboxing ([Boxing and unboxing](types.md#boxing-and-unboxing)) are not supported for pointers.</span></span> <span data-ttu-id="d9237-172">Sin embargo, se permiten conversiones entre diferentes tipos de puntero y entre tipos de puntero y tipos enteros.</span><span class="sxs-lookup"><span data-stu-id="d9237-172">However, conversions are permitted between different pointer types and between pointer types and the integral types.</span></span> <span data-ttu-id="d9237-173">Esto se describe en [conversiones de puntero](unsafe-code.md#pointer-conversions).</span><span class="sxs-lookup"><span data-stu-id="d9237-173">This is described in [Pointer conversions](unsafe-code.md#pointer-conversions).</span></span>

<span data-ttu-id="d9237-174">No se puede usar un *pointer_type* como argumento de tipo ([tipos construidos](types.md#constructed-types)) y se produce un error en la inferencia de tipos ([inferencia de tipos](expressions.md#type-inference)) en llamadas a métodos genéricos que habrían deducido un argumento de tipo para que sea un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-174">A *pointer_type* cannot be used as a type argument ([Constructed types](types.md#constructed-types)), and type inference ([Type inference](expressions.md#type-inference)) fails on generic method calls that would have inferred a type argument to be a pointer type.</span></span>

<span data-ttu-id="d9237-175">Un *pointer_type* se puede usar como el tipo de un campo volátil ([campos volátiles](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="d9237-175">A *pointer_type* may be used as the type of a volatile field ([Volatile fields](classes.md#volatile-fields)).</span></span>

<span data-ttu-id="d9237-176">Aunque los punteros se pueden pasar como parámetros `ref` o `out`, esto puede provocar un comportamiento indefinido, ya que el puntero se puede establecer para que apunte a una variable local que ya no existe cuando el método llamado vuelve, o el objeto fijo al que se ha usado para apuntar ya no se ha corregido.</span><span class="sxs-lookup"><span data-stu-id="d9237-176">Although pointers can be passed as `ref` or `out` parameters, doing so can cause undefined behavior, since the pointer may well be set to point to a local variable which no longer exists when the called method returns, or the fixed object to which it used to point, is no longer fixed.</span></span> <span data-ttu-id="d9237-177">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d9237-177">For example:</span></span>

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

<span data-ttu-id="d9237-178">Un método puede devolver un valor de algún tipo y ese tipo puede ser un puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-178">A method can return a value of some type, and that type can be a pointer.</span></span> <span data-ttu-id="d9237-179">Por ejemplo, cuando se proporciona un puntero a una secuencia contigua de `int`s, el recuento de elementos de la secuencia y otros valores de `int`, el método siguiente devuelve la dirección de ese valor en esa secuencia, en caso de que se produzca una coincidencia. en caso contrario, devuelve `null`:</span><span class="sxs-lookup"><span data-stu-id="d9237-179">For example, when given a pointer to a contiguous sequence of `int`s, that sequence's element count, and some other `int` value, the following method returns the address of that value in that sequence, if a match occurs; otherwise it returns `null`:</span></span>

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

<span data-ttu-id="d9237-180">En un contexto no seguro, hay varias construcciones disponibles para trabajar en punteros:</span><span class="sxs-lookup"><span data-stu-id="d9237-180">In an unsafe context, several constructs are available for operating on pointers:</span></span>

*  <span data-ttu-id="d9237-181">El operador `*` se puede utilizar para realizar la direccionamiento indirecto del puntero ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)).</span><span class="sxs-lookup"><span data-stu-id="d9237-181">The `*` operator may be used to perform pointer indirection ([Pointer indirection](unsafe-code.md#pointer-indirection)).</span></span>
*  <span data-ttu-id="d9237-182">El operador `->` se puede utilizar para tener acceso a un miembro de un struct a través de un puntero ([acceso a miembros de puntero](unsafe-code.md#pointer-member-access)).</span><span class="sxs-lookup"><span data-stu-id="d9237-182">The `->` operator may be used to access a member of a struct through a pointer ([Pointer member access](unsafe-code.md#pointer-member-access)).</span></span>
*  <span data-ttu-id="d9237-183">El operador `[]` se puede utilizar para indizar un puntero ([acceso a elementos de puntero](unsafe-code.md#pointer-element-access)).</span><span class="sxs-lookup"><span data-stu-id="d9237-183">The `[]` operator may be used to index a pointer ([Pointer element access](unsafe-code.md#pointer-element-access)).</span></span>
*  <span data-ttu-id="d9237-184">El operador `&` se puede utilizar para obtener la dirección de una variable ([el operador Address-of](unsafe-code.md#the-address-of-operator)).</span><span class="sxs-lookup"><span data-stu-id="d9237-184">The `&` operator may be used to obtain the address of a variable ([The address-of operator](unsafe-code.md#the-address-of-operator)).</span></span>
*  <span data-ttu-id="d9237-185">Los operadores `++` y `--` se pueden usar para aumentar y disminuir punteros ([incremento y decremento del puntero](unsafe-code.md#pointer-increment-and-decrement)).</span><span class="sxs-lookup"><span data-stu-id="d9237-185">The `++` and `--` operators may be used to increment and decrement pointers ([Pointer increment and decrement](unsafe-code.md#pointer-increment-and-decrement)).</span></span>
*  <span data-ttu-id="d9237-186">Los operadores `+` y `-` se pueden usar para realizar operaciones aritméticas de punteros ([aritmética de puntero](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="d9237-186">The `+` and `-` operators may be used to perform pointer arithmetic ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span>
*  <span data-ttu-id="d9237-187">Los operadores `==`, `!=`, `<`, `>`, `<=`y `=>` se pueden usar para comparar los punteros ([comparación de punteros](unsafe-code.md#pointer-comparison)).</span><span class="sxs-lookup"><span data-stu-id="d9237-187">The `==`, `!=`, `<`, `>`, `<=`, and `=>` operators may be used to compare pointers ([Pointer comparison](unsafe-code.md#pointer-comparison)).</span></span>
*  <span data-ttu-id="d9237-188">El operador `stackalloc` se puede usar para asignar memoria de la pila de llamadas ([búferes de tamaño fijo](unsafe-code.md#fixed-size-buffers)).</span><span class="sxs-lookup"><span data-stu-id="d9237-188">The `stackalloc` operator may be used to allocate memory from the call stack ([Fixed size buffers](unsafe-code.md#fixed-size-buffers)).</span></span>
*  <span data-ttu-id="d9237-189">La instrucción `fixed` se puede utilizar para corregir temporalmente una variable para que se pueda obtener su dirección ([la instrucción Fixed](unsafe-code.md#the-fixed-statement)).</span><span class="sxs-lookup"><span data-stu-id="d9237-189">The `fixed` statement may be used to temporarily fix a variable so its address can be obtained ([The fixed statement](unsafe-code.md#the-fixed-statement)).</span></span>

## <a name="fixed-and-moveable-variables"></a><span data-ttu-id="d9237-190">Variables fijas y móviles</span><span class="sxs-lookup"><span data-stu-id="d9237-190">Fixed and moveable variables</span></span>

<span data-ttu-id="d9237-191">El operador Address-of ([el operador Address-of](unsafe-code.md#the-address-of-operator)) y la instrucción `fixed` ([instrucción Fixed](unsafe-code.md#the-fixed-statement)) dividen las variables en dos categorías: ***variables fijas*** y ***variables móviles***.</span><span class="sxs-lookup"><span data-stu-id="d9237-191">The address-of operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) and the `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) divide variables into two categories: ***Fixed variables*** and ***moveable variables***.</span></span>

<span data-ttu-id="d9237-192">Las variables fijas residen en ubicaciones de almacenamiento que no se ven afectadas por el funcionamiento del recolector de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="d9237-192">Fixed variables reside in storage locations that are unaffected by operation of the garbage collector.</span></span> <span data-ttu-id="d9237-193">(Algunos ejemplos de variables fijas incluyen variables locales, parámetros de valor y variables creadas mediante punteros de desreferencia). Por otro lado, las variables móviles residen en ubicaciones de almacenamiento que están sujetas a reubicación o eliminación por parte del recolector de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="d9237-193">(Examples of fixed variables include local variables, value parameters, and variables created by dereferencing pointers.) On the other hand, moveable variables reside in storage locations that are subject to relocation or disposal by the garbage collector.</span></span> <span data-ttu-id="d9237-194">(Ejemplos de variables móviles incluyen campos en objetos y elementos de matrices).</span><span class="sxs-lookup"><span data-stu-id="d9237-194">(Examples of moveable variables include fields in objects and elements of arrays.)</span></span>

<span data-ttu-id="d9237-195">El operador `&` ([el operador Address-of](unsafe-code.md#the-address-of-operator)) permite obtener la dirección de una variable fija sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="d9237-195">The `&` operator ([The address-of operator](unsafe-code.md#the-address-of-operator)) permits the address of a fixed variable to be obtained without restrictions.</span></span> <span data-ttu-id="d9237-196">Sin embargo, dado que una variable móvil está sujeta a reubicación o eliminación por parte del recolector de elementos no utilizados, la dirección de una variable móvil solo se puede obtener mediante una instrucción `fixed` ([la instrucción Fixed](unsafe-code.md#the-fixed-statement)) y esa dirección permanece válida solo mientras dure la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-196">However, because a moveable variable is subject to relocation or disposal by the garbage collector, the address of a moveable variable can only be obtained using a `fixed` statement ([The fixed statement](unsafe-code.md#the-fixed-statement)), and that address remains valid only for the duration of that `fixed` statement.</span></span>

<span data-ttu-id="d9237-197">En términos precisos, una variable fija es una de las siguientes:</span><span class="sxs-lookup"><span data-stu-id="d9237-197">In precise terms, a fixed variable is one of the following:</span></span>

*  <span data-ttu-id="d9237-198">Variable resultante de una *simple_name* ([nombres simples](expressions.md#simple-names)) que hace referencia a una variable local o un parámetro de valor, a menos que una función anónima Capture la variable.</span><span class="sxs-lookup"><span data-stu-id="d9237-198">A variable resulting from a *simple_name* ([Simple names](expressions.md#simple-names)) that refers to a local variable or a value parameter, unless the variable is captured by an anonymous function.</span></span>
*  <span data-ttu-id="d9237-199">Variable resultante de un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `V.I`, donde `V` es una variable fija de un *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="d9237-199">A variable resulting from a *member_access* ([Member access](expressions.md#member-access)) of the form `V.I`, where `V` is a fixed variable of a *struct_type*.</span></span>
*  <span data-ttu-id="d9237-200">Variable que resulta de una *pointer_indirection_expression* ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)) del formulario `*P`, *un pointer_member_access* ([acceso a miembro de puntero](unsafe-code.md#pointer-member-access)) del formulario `P->I`o un *pointer_element_access* ([acceso a un elemento de puntero](unsafe-code.md#pointer-element-access)) del formulario `P[E]`.</span><span class="sxs-lookup"><span data-stu-id="d9237-200">A variable resulting from a *pointer_indirection_expression* ([Pointer indirection](unsafe-code.md#pointer-indirection)) of the form `*P`, a *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) of the form `P->I`, or a *pointer_element_access* ([Pointer element access](unsafe-code.md#pointer-element-access)) of the form `P[E]`.</span></span>

<span data-ttu-id="d9237-201">Todas las demás variables se clasifican como variables móviles.</span><span class="sxs-lookup"><span data-stu-id="d9237-201">All other variables are classified as moveable variables.</span></span>

<span data-ttu-id="d9237-202">Tenga en cuenta que un campo estático se clasifica como una variable móvil.</span><span class="sxs-lookup"><span data-stu-id="d9237-202">Note that a static field is classified as a moveable variable.</span></span> <span data-ttu-id="d9237-203">Tenga en cuenta también que un parámetro `ref` o `out` se clasifica como una variable móvil, incluso si el argumento especificado para el parámetro es una variable fija.</span><span class="sxs-lookup"><span data-stu-id="d9237-203">Also note that a `ref` or `out` parameter is classified as a moveable variable, even if the argument given for the parameter is a fixed variable.</span></span> <span data-ttu-id="d9237-204">Por último, tenga en cuenta que una variable generada al desreferenciar un puntero siempre se clasifica como una variable fija.</span><span class="sxs-lookup"><span data-stu-id="d9237-204">Finally, note that a variable produced by dereferencing a pointer is always classified as a fixed variable.</span></span>

## <a name="pointer-conversions"></a><span data-ttu-id="d9237-205">Conversiones de puntero</span><span class="sxs-lookup"><span data-stu-id="d9237-205">Pointer conversions</span></span>

<span data-ttu-id="d9237-206">En un contexto no seguro, el conjunto de conversiones implícitas disponibles ([conversiones implícitas](conversions.md#implicit-conversions)) se extiende para incluir las siguientes conversiones de puntero implícitas:</span><span class="sxs-lookup"><span data-stu-id="d9237-206">In an unsafe context, the set of available implicit conversions ([Implicit conversions](conversions.md#implicit-conversions)) is extended to include the following implicit pointer conversions:</span></span>

*  <span data-ttu-id="d9237-207">Desde cualquier *pointer_type* al tipo `void*`.</span><span class="sxs-lookup"><span data-stu-id="d9237-207">From any *pointer_type* to the type `void*`.</span></span>
*  <span data-ttu-id="d9237-208">Del literal `null` a cualquier *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="d9237-208">From the `null` literal to any *pointer_type*.</span></span>

<span data-ttu-id="d9237-209">Además, en un contexto no seguro, el conjunto de conversiones explícitas disponibles ([conversiones explícitas](conversions.md#explicit-conversions)) se extiende para incluir las siguientes conversiones de puntero explícitas:</span><span class="sxs-lookup"><span data-stu-id="d9237-209">Additionally, in an unsafe context, the set of available explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) is extended to include the following explicit pointer conversions:</span></span>

*  <span data-ttu-id="d9237-210">Desde cualquier *pointer_type* a cualquier otro *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="d9237-210">From any *pointer_type* to any other *pointer_type*.</span></span>
*  <span data-ttu-id="d9237-211">Desde `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`o `ulong` a cualquier *pointer_type*.</span><span class="sxs-lookup"><span data-stu-id="d9237-211">From `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong` to any *pointer_type*.</span></span>
*  <span data-ttu-id="d9237-212">Desde cualquier *pointer_type* a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`o `ulong`.</span><span class="sxs-lookup"><span data-stu-id="d9237-212">From any *pointer_type* to `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="d9237-213">Por último, en un contexto no seguro, el conjunto de conversiones implícitas estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) incluye la siguiente conversión de puntero:</span><span class="sxs-lookup"><span data-stu-id="d9237-213">Finally, in an unsafe context, the set of standard implicit conversions ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) includes the following pointer conversion:</span></span>

*  <span data-ttu-id="d9237-214">Desde cualquier *pointer_type* al tipo `void*`.</span><span class="sxs-lookup"><span data-stu-id="d9237-214">From any *pointer_type* to the type `void*`.</span></span>

<span data-ttu-id="d9237-215">Las conversiones entre dos tipos de puntero nunca cambian el valor del puntero real.</span><span class="sxs-lookup"><span data-stu-id="d9237-215">Conversions between two pointer types never change the actual pointer value.</span></span> <span data-ttu-id="d9237-216">En otras palabras, una conversión de un tipo de puntero a otro no tiene ningún efecto en la dirección subyacente proporcionada por el puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-216">In other words, a conversion from one pointer type to another has no effect on the underlying address given by the pointer.</span></span>

<span data-ttu-id="d9237-217">Cuando un tipo de puntero se convierte en otro, si el puntero resultante no está alineado correctamente para el tipo señalado, el comportamiento es indefinido si se desreferencia el resultado.</span><span class="sxs-lookup"><span data-stu-id="d9237-217">When one pointer type is converted to another, if the resulting pointer is not correctly aligned for the pointed-to type, the behavior is undefined if the result is dereferenced.</span></span> <span data-ttu-id="d9237-218">En general, el concepto "correctamente alineado" es transitivo: Si un puntero al tipo `A` está alineado correctamente para un puntero al tipo `B`, que, a su vez, está alineado correctamente para un puntero al tipo `C`, un puntero al tipo `A` se alinea correctamente para un puntero al tipo `C`.</span><span class="sxs-lookup"><span data-stu-id="d9237-218">In general, the concept "correctly aligned" is transitive: if a pointer to type `A` is correctly aligned for a pointer to type `B`, which, in turn, is correctly aligned for a pointer to type `C`, then a pointer to type `A` is correctly aligned for a pointer to type `C`.</span></span>

<span data-ttu-id="d9237-219">Considere el siguiente caso en el que se tiene acceso a una variable con un tipo a través de un puntero a un tipo diferente:</span><span class="sxs-lookup"><span data-stu-id="d9237-219">Consider the following case in which a variable having one type is accessed via a pointer to a different type:</span></span>

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

<span data-ttu-id="d9237-220">Cuando un tipo de puntero se convierte en un puntero a byte, el resultado apunta al byte más bajo direccionable de la variable.</span><span class="sxs-lookup"><span data-stu-id="d9237-220">When a pointer type is converted to a pointer to byte, the result points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="d9237-221">Los incrementos sucesivos del resultado, hasta el tamaño de la variable, proporcionan punteros a los bytes restantes de esa variable.</span><span class="sxs-lookup"><span data-stu-id="d9237-221">Successive increments of the result, up to the size of the variable, yield pointers to the remaining bytes of that variable.</span></span> <span data-ttu-id="d9237-222">Por ejemplo, el siguiente método muestra cada uno de los ocho bytes en un tipo Double como un valor hexadecimal:</span><span class="sxs-lookup"><span data-stu-id="d9237-222">For example, the following method displays each of the eight bytes in a double as a hexadecimal value:</span></span>

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

<span data-ttu-id="d9237-223">Por supuesto, la salida generada depende de modos endian.</span><span class="sxs-lookup"><span data-stu-id="d9237-223">Of course, the output produced depends on endianness.</span></span>

<span data-ttu-id="d9237-224">Las asignaciones entre punteros y enteros están definidas por la implementación.</span><span class="sxs-lookup"><span data-stu-id="d9237-224">Mappings between pointers and integers are implementation-defined.</span></span> <span data-ttu-id="d9237-225">Sin embargo, en las arquitecturas de CPU de 32 \* y 64 bits con un espacio de direcciones lineal, las conversiones de punteros a o desde tipos enteros normalmente se comportan exactamente igual que las conversiones de `uint` o `ulong` valores, respectivamente, a o desde esos tipos enteros.</span><span class="sxs-lookup"><span data-stu-id="d9237-225">However, on 32\* and 64-bit CPU architectures with a linear address space, conversions of pointers to or from integral types typically behave exactly like conversions of `uint` or `ulong` values, respectively, to or from those integral types.</span></span>

### <a name="pointer-arrays"></a><span data-ttu-id="d9237-226">Matrices de puntero</span><span class="sxs-lookup"><span data-stu-id="d9237-226">Pointer arrays</span></span>

<span data-ttu-id="d9237-227">En un contexto no seguro, se pueden construir matrices de punteros.</span><span class="sxs-lookup"><span data-stu-id="d9237-227">In an unsafe context, arrays of pointers can be constructed.</span></span> <span data-ttu-id="d9237-228">En las matrices de puntero solo se permiten algunas de las conversiones que se aplican a otros tipos de matriz:</span><span class="sxs-lookup"><span data-stu-id="d9237-228">Only some of the conversions that apply to other array types are allowed on pointer arrays:</span></span>

*  <span data-ttu-id="d9237-229">La conversión de referencia implícita ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions)) de cualquier *array_type* a `System.Array` y las interfaces que implementa también se aplican a las matrices de punteros.</span><span class="sxs-lookup"><span data-stu-id="d9237-229">The implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) from any *array_type* to `System.Array` and the interfaces it implements also applies to pointer arrays.</span></span> <span data-ttu-id="d9237-230">Sin embargo, cualquier intento de obtener acceso a los elementos de la matriz a través de `System.Array` o de las interfaces que implementa producirá una excepción en tiempo de ejecución, ya que los tipos de puntero no se pueden convertir a `object`.</span><span class="sxs-lookup"><span data-stu-id="d9237-230">However, any attempt to access the array elements through `System.Array` or the interfaces it implements will result in an exception at run-time, as pointer types are not convertible to `object`.</span></span>
*  <span data-ttu-id="d9237-231">Las conversiones de referencia implícitas y explícitas (conversiones de[referencia implícita](conversions.md#implicit-reference-conversions), [conversiones de referencia explícitas](conversions.md#explicit-reference-conversions)) de un tipo de matriz unidimensional `S[]` a `System.Collections.Generic.IList<T>` y sus interfaces base genéricas nunca se aplican a las matrices de puntero, ya que los tipos de puntero no se pueden usar como argumentos de tipo y no hay conversiones de tipos de puntero en tipos que no son de puntero</span><span class="sxs-lookup"><span data-stu-id="d9237-231">The implicit and explicit reference conversions ([Implicit reference conversions](conversions.md#implicit-reference-conversions), [Explicit reference conversions](conversions.md#explicit-reference-conversions)) from a single-dimensional array type `S[]` to `System.Collections.Generic.IList<T>` and its generic base interfaces never apply to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>
*  <span data-ttu-id="d9237-232">La conversión de referencia explícita ([conversiones de referencia explícita](conversions.md#explicit-reference-conversions)) de `System.Array` y las interfaces que implementa en cualquier *array_type* se aplican a las matrices de punteros.</span><span class="sxs-lookup"><span data-stu-id="d9237-232">The explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Array` and the interfaces it implements to any *array_type* applies to pointer arrays.</span></span>
*  <span data-ttu-id="d9237-233">Las conversiones explícitas de referencia ([conversiones de referencia explícita](conversions.md#explicit-reference-conversions)) de `System.Collections.Generic.IList<S>` y sus interfaces base a un tipo de matriz unidimensional `T[]` nunca se aplican a las matrices de punteros, ya que los tipos de puntero no se pueden usar como argumentos de tipo y no hay conversiones de tipos de puntero en tipos que no son de puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-233">The explicit reference conversions ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) from `System.Collections.Generic.IList<S>` and its base interfaces to a single-dimensional array type `T[]` never applies to pointer arrays, since pointer types cannot be used as type arguments, and there are no conversions from pointer types to non-pointer types.</span></span>

<span data-ttu-id="d9237-234">Estas restricciones implican que la expansión de la instrucción `foreach` sobre las matrices descritas en [la instrucción foreach](statements.md#the-foreach-statement) no se puede aplicar a las matrices de punteros.</span><span class="sxs-lookup"><span data-stu-id="d9237-234">These restrictions mean that the expansion for the `foreach` statement over arrays described in [The foreach statement](statements.md#the-foreach-statement) cannot be applied to pointer arrays.</span></span> <span data-ttu-id="d9237-235">En su lugar, una instrucción foreach con el formato</span><span class="sxs-lookup"><span data-stu-id="d9237-235">Instead, a foreach statement of the form</span></span>

```csharp
foreach (V v in x) embedded_statement
```

<span data-ttu-id="d9237-236">donde el tipo de `x` es un tipo de matriz con el formato `T[,,...,]`, `N` es el número de dimensiones menos 1 y `T` o `V` es un tipo de puntero, se expande mediante bucles for anidados como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="d9237-236">where the type of `x` is an array type of the form `T[,,...,]`, `N` is the number of dimensions minus 1 and `T` or `V` is a pointer type, is expanded using nested for-loops as follows:</span></span>

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

<span data-ttu-id="d9237-237">Las variables `a`, `i0`, `i1`,..., `iN` no son visibles o son accesibles para `x` o *embedded_statement* o cualquier otro código fuente del programa.</span><span class="sxs-lookup"><span data-stu-id="d9237-237">The variables `a`, `i0`, `i1`, ..., `iN` are not visible to or accessible to `x` or the *embedded_statement* or any other source code of the program.</span></span> <span data-ttu-id="d9237-238">La variable `v` es de solo lectura en la instrucción insertada.</span><span class="sxs-lookup"><span data-stu-id="d9237-238">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="d9237-239">Si no hay una conversión explícita ([conversiones de puntero](unsafe-code.md#pointer-conversions)) de `T` (el tipo de elemento) en `V`, se genera un error y no se realiza ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="d9237-239">If there is not an explicit conversion ([Pointer conversions](unsafe-code.md#pointer-conversions)) from `T` (the element type) to `V`, an error is produced and no further steps are taken.</span></span> <span data-ttu-id="d9237-240">Si `x` tiene el valor `null`, se produce una `System.NullReferenceException` en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d9237-240">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

## <a name="pointers-in-expressions"></a><span data-ttu-id="d9237-241">Punteros en expresiones</span><span class="sxs-lookup"><span data-stu-id="d9237-241">Pointers in expressions</span></span>

<span data-ttu-id="d9237-242">En un contexto no seguro, una expresión puede producir un resultado de un tipo de puntero, pero fuera de un contexto no seguro, es un error en tiempo de compilación para que una expresión sea de un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-242">In an unsafe context, an expression may yield a result of a pointer type, but outside an unsafe context it is a compile-time error for an expression to be of a pointer type.</span></span> <span data-ttu-id="d9237-243">En términos precisos, fuera de un contexto no seguro, se produce un error en tiempo de compilación si algún *simple_name* ([nombres simples](expressions.md#simple-names)), *member_access* ([acceso a miembros](expressions.md#member-access)), *invocation_expression* ([expresiones de invocación](expressions.md#invocation-expressions)) o *element_access* ([acceso a elementos](expressions.md#element-access)) es de un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-243">In precise terms, outside an unsafe context a compile-time error occurs if any *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)), or *element_access* ([Element access](expressions.md#element-access)) is of a pointer type.</span></span>

<span data-ttu-id="d9237-244">En un contexto no seguro, las producciones *primary_no_array_creation_expression* ([expresiones primarias](expressions.md#primary-expressions)) y *unary_expression* ([operadores unarios](expressions.md#unary-operators)) permiten las siguientes construcciones adicionales:</span><span class="sxs-lookup"><span data-stu-id="d9237-244">In an unsafe context, the *primary_no_array_creation_expression* ([Primary expressions](expressions.md#primary-expressions)) and *unary_expression* ([Unary operators](expressions.md#unary-operators)) productions permit the following additional constructs:</span></span>

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

<span data-ttu-id="d9237-245">Estas construcciones se describen en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="d9237-245">These constructs are described in the following sections.</span></span> <span data-ttu-id="d9237-246">La gramática implica la prioridad y la asociatividad de los operadores no seguros.</span><span class="sxs-lookup"><span data-stu-id="d9237-246">The precedence and associativity of the unsafe operators is implied by the grammar.</span></span>

### <a name="pointer-indirection"></a><span data-ttu-id="d9237-247">Direccionamiento indirecto de puntero</span><span class="sxs-lookup"><span data-stu-id="d9237-247">Pointer indirection</span></span>

<span data-ttu-id="d9237-248">Un *pointer_indirection_expression* consta de un asterisco (`*`) seguido de un *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="d9237-248">A *pointer_indirection_expression* consists of an asterisk (`*`) followed by a *unary_expression*.</span></span>

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

<span data-ttu-id="d9237-249">El operador unario `*` denota el direccionamiento indirecto del puntero y se utiliza para obtener la variable a la que señala un puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-249">The unary `*` operator denotes pointer indirection and is used to obtain the variable to which a pointer points.</span></span> <span data-ttu-id="d9237-250">El resultado de evaluar `*P`, donde `P` es una expresión de un tipo de puntero `T*`, es una variable de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="d9237-250">The result of evaluating `*P`, where `P` is an expression of a pointer type `T*`, is a variable of type `T`.</span></span> <span data-ttu-id="d9237-251">Es un error en tiempo de compilación aplicar el operador unario `*` a una expresión de tipo `void*` o a una expresión que no es de un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-251">It is a compile-time error to apply the unary `*` operator to an expression of type `void*` or to an expression that isn't of a pointer type.</span></span>

<span data-ttu-id="d9237-252">El efecto de aplicar el operador unario `*` a un puntero `null` está definido por la implementación.</span><span class="sxs-lookup"><span data-stu-id="d9237-252">The effect of applying the unary `*` operator to a `null` pointer is implementation-defined.</span></span> <span data-ttu-id="d9237-253">En concreto, no hay ninguna garantía de que esta operación produzca una `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="d9237-253">In particular, there is no guarantee that this operation throws a `System.NullReferenceException`.</span></span>

<span data-ttu-id="d9237-254">Si se ha asignado un valor no válido al puntero, el comportamiento del operador unario `*` es indefinido.</span><span class="sxs-lookup"><span data-stu-id="d9237-254">If an invalid value has been assigned to the pointer, the behavior of the unary `*` operator is undefined.</span></span> <span data-ttu-id="d9237-255">Entre los valores no válidos para desreferenciar un puntero por el operador unario `*`, se trata de una dirección incorrectamente alineada para el tipo señalado (vea el ejemplo en [conversiones de puntero](unsafe-code.md#pointer-conversions)) y la dirección de una variable después del final de su período de duración.</span><span class="sxs-lookup"><span data-stu-id="d9237-255">Among the invalid values for dereferencing a pointer by the unary `*` operator are an address inappropriately aligned for the type pointed to (see example in [Pointer conversions](unsafe-code.md#pointer-conversions)), and the address of a variable after the end of its lifetime.</span></span>

<span data-ttu-id="d9237-256">A efectos del análisis de asignación definitiva, una variable generada al evaluar una expresión con el formato `*P` se considera inicialmente asignada ([variables asignadas inicialmente](variables.md#initially-assigned-variables)).</span><span class="sxs-lookup"><span data-stu-id="d9237-256">For purposes of definite assignment analysis, a variable produced by evaluating an expression of the form `*P` is considered initially assigned ([Initially assigned variables](variables.md#initially-assigned-variables)).</span></span>

### <a name="pointer-member-access"></a><span data-ttu-id="d9237-257">Acceso a miembros de puntero</span><span class="sxs-lookup"><span data-stu-id="d9237-257">Pointer member access</span></span>

<span data-ttu-id="d9237-258">Un *pointer_member_access* consta de un *primary_expression*, seguido de un token "`->`", seguido de un *identificador* y un *type_argument_list*opcional.</span><span class="sxs-lookup"><span data-stu-id="d9237-258">A *pointer_member_access* consists of a *primary_expression*, followed by a "`->`" token, followed by an *identifier* and an optional *type_argument_list*.</span></span>

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

<span data-ttu-id="d9237-259">En un acceso de miembro de puntero del formulario `P->I`, `P` debe ser una expresión de un tipo de puntero distinto de `void*`y `I` debe indicar un miembro accesible del tipo al que apunta `P`.</span><span class="sxs-lookup"><span data-stu-id="d9237-259">In a pointer member access of the form `P->I`, `P` must be an expression of a pointer type other than `void*`, and `I` must denote an accessible member of the type to which `P` points.</span></span>

<span data-ttu-id="d9237-260">Un acceso de miembro de puntero del formulario `P->I` se evalúa exactamente como `(*P).I`.</span><span class="sxs-lookup"><span data-stu-id="d9237-260">A pointer member access of the form `P->I` is evaluated exactly as `(*P).I`.</span></span> <span data-ttu-id="d9237-261">Para obtener una descripción del operador de direccionamiento indirecto de puntero (`*`), vea [direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="d9237-261">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="d9237-262">Para obtener una descripción del operador de acceso a miembros (`.`), consulte [acceso a miembros](expressions.md#member-access).</span><span class="sxs-lookup"><span data-stu-id="d9237-262">For a description of the member access operator (`.`), see [Member access](expressions.md#member-access).</span></span>

<span data-ttu-id="d9237-263">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9237-263">In the example</span></span>

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

<span data-ttu-id="d9237-264">el operador `->` se utiliza para tener acceso a los campos e invocar un método de un struct a través de un puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-264">the `->` operator is used to access fields and invoke a method of a struct through a pointer.</span></span> <span data-ttu-id="d9237-265">Dado que la operación `P->I` es precisamente equivalente a `(*P).I`, el método de `Main` se podría haber escrito igualmente correctamente:</span><span class="sxs-lookup"><span data-stu-id="d9237-265">Because the operation `P->I` is precisely equivalent to `(*P).I`, the `Main` method could equally well have been written:</span></span>

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

### <a name="pointer-element-access"></a><span data-ttu-id="d9237-266">Acceso a elementos de puntero</span><span class="sxs-lookup"><span data-stu-id="d9237-266">Pointer element access</span></span>

<span data-ttu-id="d9237-267">Un *pointer_element_access* consta de un *primary_no_array_creation_expression* seguido de una expresión encerrada en "`[`" y "`]`".</span><span class="sxs-lookup"><span data-stu-id="d9237-267">A *pointer_element_access* consists of a *primary_no_array_creation_expression* followed by an expression enclosed in "`[`" and "`]`".</span></span>

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

<span data-ttu-id="d9237-268">En un elemento de puntero acceso del formulario `P[E]`, `P` debe ser una expresión de un tipo de puntero distinto de `void*`y `E` debe ser una expresión que se pueda convertir implícitamente en `int`, `uint`, `long`o `ulong`.</span><span class="sxs-lookup"><span data-stu-id="d9237-268">In a pointer element access of the form `P[E]`, `P` must be an expression of a pointer type other than `void*`, and `E` must be an expression that can be implicitly converted to `int`, `uint`, `long`, or `ulong`.</span></span>

<span data-ttu-id="d9237-269">Un acceso de elemento de puntero del formulario `P[E]` se evalúa exactamente como `*(P + E)`.</span><span class="sxs-lookup"><span data-stu-id="d9237-269">A pointer element access of the form `P[E]` is evaluated exactly as `*(P + E)`.</span></span> <span data-ttu-id="d9237-270">Para obtener una descripción del operador de direccionamiento indirecto de puntero (`*`), vea [direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection).</span><span class="sxs-lookup"><span data-stu-id="d9237-270">For a description of the pointer indirection operator (`*`), see [Pointer indirection](unsafe-code.md#pointer-indirection).</span></span> <span data-ttu-id="d9237-271">Para obtener una descripción del operador de adición de puntero (`+`), vea [aritmética de punteros](unsafe-code.md#pointer-arithmetic).</span><span class="sxs-lookup"><span data-stu-id="d9237-271">For a description of the pointer addition operator (`+`), see [Pointer arithmetic](unsafe-code.md#pointer-arithmetic).</span></span>

<span data-ttu-id="d9237-272">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9237-272">In the example</span></span>

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

<span data-ttu-id="d9237-273">un acceso de elemento de puntero se usa para inicializar el búfer de caracteres en un bucle de `for`.</span><span class="sxs-lookup"><span data-stu-id="d9237-273">a pointer element access is used to initialize the character buffer in a `for` loop.</span></span> <span data-ttu-id="d9237-274">Dado que la operación `P[E]` es precisamente equivalente a `*(P + E)`, el ejemplo se podría haber escrito igualmente correctamente:</span><span class="sxs-lookup"><span data-stu-id="d9237-274">Because the operation `P[E]` is precisely equivalent to `*(P + E)`, the example could equally well have been written:</span></span>

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

<span data-ttu-id="d9237-275">El operador de acceso a elementos de puntero no comprueba los errores fuera de los límites y el comportamiento cuando el acceso a un elemento fuera de los límites es indefinido.</span><span class="sxs-lookup"><span data-stu-id="d9237-275">The pointer element access operator does not check for out-of-bounds errors and the behavior when accessing an out-of-bounds element is undefined.</span></span> <span data-ttu-id="d9237-276">Es lo mismo que C y C++.</span><span class="sxs-lookup"><span data-stu-id="d9237-276">This is the same as C and C++.</span></span>

### <a name="the-address-of-operator"></a><span data-ttu-id="d9237-277">Operador Address-of</span><span class="sxs-lookup"><span data-stu-id="d9237-277">The address-of operator</span></span>

<span data-ttu-id="d9237-278">Un *addressof_expression* consta de una y comercial (`&`) seguida de un *unary_expression*.</span><span class="sxs-lookup"><span data-stu-id="d9237-278">An *addressof_expression* consists of an ampersand (`&`) followed by a *unary_expression*.</span></span>

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

<span data-ttu-id="d9237-279">Dada una expresión `E` que es de un tipo `T` y se clasifica como una variable fija ([variables fijas y móviles](unsafe-code.md#fixed-and-moveable-variables)), la construcción `&E` calcula la dirección de la variable proporcionada por `E`.</span><span class="sxs-lookup"><span data-stu-id="d9237-279">Given an expression `E` which is of a type `T` and is classified as a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), the construct `&E` computes the address of the variable given by `E`.</span></span> <span data-ttu-id="d9237-280">El tipo del resultado es `T*` y se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="d9237-280">The type of the result is `T*` and is classified as a value.</span></span> <span data-ttu-id="d9237-281">Se produce un error en tiempo de compilación si `E` no está clasificado como una variable, si `E` está clasificado como una variable local de solo lectura o si `E` denota una variable móvil.</span><span class="sxs-lookup"><span data-stu-id="d9237-281">A compile-time error occurs if `E` is not classified as a variable, if `E` is classified as a read-only local variable, or if `E` denotes a moveable variable.</span></span> <span data-ttu-id="d9237-282">En el último caso, se puede usar una instrucción fixed ([la instrucción Fixed](unsafe-code.md#the-fixed-statement)) para "corregir" temporalmente la variable antes de obtener su dirección.</span><span class="sxs-lookup"><span data-stu-id="d9237-282">In the last case, a fixed statement ([The fixed statement](unsafe-code.md#the-fixed-statement)) can be used to temporarily "fix" the variable before obtaining its address.</span></span> <span data-ttu-id="d9237-283">Como se indica en [acceso a miembros](expressions.md#member-access), fuera de un constructor de instancia o un constructor estático para una estructura o clase que define un campo de `readonly`, ese campo se considera un valor, no una variable.</span><span class="sxs-lookup"><span data-stu-id="d9237-283">As stated in [Member access](expressions.md#member-access), outside an instance constructor or static constructor for a struct or class that defines a `readonly` field, that field is considered a value, not a variable.</span></span> <span data-ttu-id="d9237-284">Como tal, no se puede tomar su dirección.</span><span class="sxs-lookup"><span data-stu-id="d9237-284">As such, its address cannot be taken.</span></span> <span data-ttu-id="d9237-285">Del mismo modo, no se puede tomar la dirección de una constante.</span><span class="sxs-lookup"><span data-stu-id="d9237-285">Similarly, the address of a constant cannot be taken.</span></span>

<span data-ttu-id="d9237-286">El operador `&` no requiere que el argumento esté asignado definitivamente, pero después de una operación `&`, la variable a la que se aplica el operador se considera definitivamente asignada en la ruta de acceso de ejecución en la que se produce la operación.</span><span class="sxs-lookup"><span data-stu-id="d9237-286">The `&` operator does not require its argument to be definitely assigned, but following an `&` operation, the variable to which the operator is applied is considered definitely assigned in the execution path in which the operation occurs.</span></span> <span data-ttu-id="d9237-287">Es responsabilidad del programador asegurarse de que la inicialización correcta de la variable realmente se realiza en esta situación.</span><span class="sxs-lookup"><span data-stu-id="d9237-287">It is the responsibility of the programmer to ensure that correct initialization of the variable actually does take place in this situation.</span></span>

<span data-ttu-id="d9237-288">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9237-288">In the example</span></span>

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

<span data-ttu-id="d9237-289">`i` se considera definitivamente asignada después de la operación de `&i` que se usa para inicializar `p`.</span><span class="sxs-lookup"><span data-stu-id="d9237-289">`i` is considered definitely assigned following the `&i` operation used to initialize `p`.</span></span> <span data-ttu-id="d9237-290">La asignación a `*p` en vigor inicializa `i`, pero la inclusión de esta inicialización es responsabilidad del programador y no se produciría ningún error en tiempo de compilación si se quitara la asignación.</span><span class="sxs-lookup"><span data-stu-id="d9237-290">The assignment to `*p` in effect initializes `i`, but the inclusion of this initialization is the responsibility of the programmer, and no compile-time error would occur if the assignment was removed.</span></span>

<span data-ttu-id="d9237-291">Las reglas de asignación definitiva para el operador de `&` existen de forma que se pueda evitar la inicialización redundante de variables locales.</span><span class="sxs-lookup"><span data-stu-id="d9237-291">The rules of definite assignment for the `&` operator exist such that redundant initialization of local variables can be avoided.</span></span> <span data-ttu-id="d9237-292">Por ejemplo, muchas API externas toman un puntero a una estructura que se rellena mediante la API.</span><span class="sxs-lookup"><span data-stu-id="d9237-292">For example, many external APIs take a pointer to a structure which is filled in by the API.</span></span> <span data-ttu-id="d9237-293">Las llamadas a estas API normalmente pasan la dirección de una variable de struct local y sin la regla, sería necesaria la inicialización redundante de la variable de estructura.</span><span class="sxs-lookup"><span data-stu-id="d9237-293">Calls to such APIs typically pass the address of a local struct variable, and without the rule, redundant initialization of the struct variable would be required.</span></span>

### <a name="pointer-increment-and-decrement"></a><span data-ttu-id="d9237-294">Incremento y decremento de puntero</span><span class="sxs-lookup"><span data-stu-id="d9237-294">Pointer increment and decrement</span></span>

<span data-ttu-id="d9237-295">En un contexto no seguro, los operadores `++` y `--` (operadores de[incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) de [postfijo y operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)) se pueden aplicar a las variables de puntero de todos los tipos excepto `void*`.</span><span class="sxs-lookup"><span data-stu-id="d9237-295">In an unsafe context, the `++` and `--` operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)) can be applied to pointer variables of all types except `void*`.</span></span> <span data-ttu-id="d9237-296">Por lo tanto, para cada tipo de puntero `T*`, se definen implícitamente los siguientes operadores:</span><span class="sxs-lookup"><span data-stu-id="d9237-296">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

<span data-ttu-id="d9237-297">Los operadores producen los mismos resultados que `x + 1` y `x - 1`, respectivamente ([aritmética de puntero](unsafe-code.md#pointer-arithmetic)).</span><span class="sxs-lookup"><span data-stu-id="d9237-297">The operators produce the same results as `x + 1` and `x - 1`, respectively ([Pointer arithmetic](unsafe-code.md#pointer-arithmetic)).</span></span> <span data-ttu-id="d9237-298">En otras palabras, para una variable de puntero de tipo `T*`, el operador `++` agrega `sizeof(T)` a la dirección contenida en la variable y el operador `--` resta `sizeof(T)` de la dirección incluida en la variable.</span><span class="sxs-lookup"><span data-stu-id="d9237-298">In other words, for a pointer variable of type `T*`, the `++` operator adds `sizeof(T)` to the address contained in the variable, and the `--` operator subtracts `sizeof(T)` from the address contained in the variable.</span></span>

<span data-ttu-id="d9237-299">Si una operación de incremento o decremento de puntero desborda el dominio del tipo de puntero, el resultado se define en la implementación, pero no se genera ninguna excepción.</span><span class="sxs-lookup"><span data-stu-id="d9237-299">If a pointer increment or decrement operation overflows the domain of the pointer type, the result is implementation-defined, but no exceptions are produced.</span></span>

### <a name="pointer-arithmetic"></a><span data-ttu-id="d9237-300">Aritmética de puntero</span><span class="sxs-lookup"><span data-stu-id="d9237-300">Pointer arithmetic</span></span>

<span data-ttu-id="d9237-301">En un contexto no seguro, los operadores `+` y `-` (operador de[suma](expressions.md#addition-operator) y [resta](expressions.md#subtraction-operator)) se pueden aplicar a los valores de todos los tipos de puntero excepto `void*`.</span><span class="sxs-lookup"><span data-stu-id="d9237-301">In an unsafe context, the `+` and `-` operators ([Addition operator](expressions.md#addition-operator) and [Subtraction operator](expressions.md#subtraction-operator)) can be applied to values of all pointer types except `void*`.</span></span> <span data-ttu-id="d9237-302">Por lo tanto, para cada tipo de puntero `T*`, se definen implícitamente los siguientes operadores:</span><span class="sxs-lookup"><span data-stu-id="d9237-302">Thus, for every pointer type `T*`, the following operators are implicitly defined:</span></span>

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

<span data-ttu-id="d9237-303">Dada una expresión `P` de un tipo de puntero `T*` y una expresión `N` de tipo `int`, `uint`, `long`o `ulong`, las expresiones `P + N` y `N + P` calculan el valor de puntero de tipo `T*` que es el resultado de agregar `N * sizeof(T)` a la dirección proporcionada por `P`.</span><span class="sxs-lookup"><span data-stu-id="d9237-303">Given an expression `P` of a pointer type `T*` and an expression `N` of type `int`, `uint`, `long`, or `ulong`, the expressions `P + N` and `N + P` compute the pointer value of type `T*` that results from adding `N * sizeof(T)` to the address given by `P`.</span></span> <span data-ttu-id="d9237-304">Del mismo modo, la expresión `P - N` calcula el valor de puntero de tipo `T*` que es el resultado de restar `N * sizeof(T)` de la dirección proporcionada por `P`.</span><span class="sxs-lookup"><span data-stu-id="d9237-304">Likewise, the expression `P - N` computes the pointer value of type `T*` that results from subtracting `N * sizeof(T)` from the address given by `P`.</span></span>

<span data-ttu-id="d9237-305">Dadas dos expresiones, `P` y `Q`, de un tipo de puntero `T*`, la expresión `P - Q` calcula la diferencia entre las direcciones proporcionadas por `P` y `Q` y, a continuación, divide esa diferencia por `sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="d9237-305">Given two expressions, `P` and `Q`, of a pointer type `T*`, the expression `P - Q` computes the difference between the addresses given by `P` and `Q` and then divides that difference by `sizeof(T)`.</span></span> <span data-ttu-id="d9237-306">El tipo del resultado siempre es `long`.</span><span class="sxs-lookup"><span data-stu-id="d9237-306">The type of the result is always `long`.</span></span> <span data-ttu-id="d9237-307">En efecto, `P - Q` se calcula como `((long)(P) - (long)(Q)) / sizeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="d9237-307">In effect, `P - Q` is computed as `((long)(P) - (long)(Q)) / sizeof(T)`.</span></span>

<span data-ttu-id="d9237-308">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d9237-308">For example:</span></span>

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

<span data-ttu-id="d9237-309">que genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="d9237-309">which produces the output:</span></span>

```console
p - q = -14
q - p = 14
```

<span data-ttu-id="d9237-310">Si una operación aritmética de puntero desborda el dominio del tipo de puntero, el resultado se trunca en un modo definido por la implementación, pero no se genera ninguna excepción.</span><span class="sxs-lookup"><span data-stu-id="d9237-310">If a pointer arithmetic operation overflows the domain of the pointer type, the result is truncated in an implementation-defined fashion, but no exceptions are produced.</span></span>

### <a name="pointer-comparison"></a><span data-ttu-id="d9237-311">Comparación de punteros</span><span class="sxs-lookup"><span data-stu-id="d9237-311">Pointer comparison</span></span>

<span data-ttu-id="d9237-312">En un contexto no seguro, los operadores `==`, `!=`, `<`, `>`, `<=`y `=>` ([operadores relacionales y de prueba de tipos](expressions.md#relational-and-type-testing-operators)) se pueden aplicar a los valores de todos los tipos de puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-312">In an unsafe context, the `==`, `!=`, `<`, `>`, `<=`, and `=>` operators ([Relational and type-testing operators](expressions.md#relational-and-type-testing-operators)) can be applied to values of all pointer types.</span></span> <span data-ttu-id="d9237-313">Los operadores de comparación de punteros son:</span><span class="sxs-lookup"><span data-stu-id="d9237-313">The pointer comparison operators are:</span></span>

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

<span data-ttu-id="d9237-314">Dado que existe una conversión implícita de cualquier tipo de puntero al tipo de `void*`, se pueden comparar los operandos de cualquier tipo de puntero mediante estos operadores.</span><span class="sxs-lookup"><span data-stu-id="d9237-314">Because an implicit conversion exists from any pointer type to the `void*` type, operands of any pointer type can be compared using these operators.</span></span> <span data-ttu-id="d9237-315">Los operadores de comparación comparan las direcciones proporcionadas por los dos operandos como si fueran enteros sin signo.</span><span class="sxs-lookup"><span data-stu-id="d9237-315">The comparison operators compare the addresses given by the two operands as if they were unsigned integers.</span></span>

### <a name="the-sizeof-operator"></a><span data-ttu-id="d9237-316">El operador sizeof</span><span class="sxs-lookup"><span data-stu-id="d9237-316">The sizeof operator</span></span>

<span data-ttu-id="d9237-317">El operador `sizeof` devuelve el número de bytes ocupados por una variable de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="d9237-317">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="d9237-318">El tipo especificado como operando para `sizeof` debe ser un *unmanaged_type* ([tipos de puntero](unsafe-code.md#pointer-types)).</span><span class="sxs-lookup"><span data-stu-id="d9237-318">The type specified as an operand to `sizeof` must be an *unmanaged_type* ([Pointer types](unsafe-code.md#pointer-types)).</span></span>

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

<span data-ttu-id="d9237-319">El resultado del operador `sizeof` es un valor de tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="d9237-319">The result of the `sizeof` operator is a value of type `int`.</span></span> <span data-ttu-id="d9237-320">Para determinados tipos predefinidos, el operador `sizeof` produce un valor constante, tal como se muestra en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="d9237-320">For certain predefined types, the `sizeof` operator yields a constant value as shown in the table below.</span></span>


| <span data-ttu-id="d9237-321">__Expresión__</span><span class="sxs-lookup"><span data-stu-id="d9237-321">__Expression__</span></span>   | <span data-ttu-id="d9237-322">__Resultado__</span><span class="sxs-lookup"><span data-stu-id="d9237-322">__Result__</span></span> |
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

<span data-ttu-id="d9237-323">En el caso de todos los demás tipos, el resultado del operador `sizeof` está definido por la implementación y se clasifica como un valor, no como una constante.</span><span class="sxs-lookup"><span data-stu-id="d9237-323">For all other types, the result of the `sizeof` operator is implementation-defined and is classified as a value, not a constant.</span></span>

<span data-ttu-id="d9237-324">No se especifica el orden en que los miembros se empaquetan en un struct.</span><span class="sxs-lookup"><span data-stu-id="d9237-324">The order in which members are packed into a struct is unspecified.</span></span>

<span data-ttu-id="d9237-325">A efectos de alineación, puede haber un relleno sin nombre al principio de un struct, dentro de un struct y al final de la estructura.</span><span class="sxs-lookup"><span data-stu-id="d9237-325">For alignment purposes, there may be unnamed padding at the beginning of a struct, within a struct, and at the end of the struct.</span></span> <span data-ttu-id="d9237-326">El contenido de los bits usados como relleno es indeterminado.</span><span class="sxs-lookup"><span data-stu-id="d9237-326">The contents of the bits used as padding are indeterminate.</span></span>

<span data-ttu-id="d9237-327">Cuando se aplica a un operando que tiene el tipo de estructura, el resultado es el número total de bytes en una variable de ese tipo, incluido el relleno.</span><span class="sxs-lookup"><span data-stu-id="d9237-327">When applied to an operand that has struct type, the result is the total number of bytes in a variable of that type, including any padding.</span></span>

## <a name="the-fixed-statement"></a><span data-ttu-id="d9237-328">Instrucción Fixed</span><span class="sxs-lookup"><span data-stu-id="d9237-328">The fixed statement</span></span>

<span data-ttu-id="d9237-329">En un contexto no seguro, la producción de *embedded_statement* ([instrucciones](statements.md)) permite una construcción adicional, la instrucción `fixed`, que se usa para "corregir" una variable móvil, de modo que su dirección permanece constante mientras dure la instrucción.</span><span class="sxs-lookup"><span data-stu-id="d9237-329">In an unsafe context, the *embedded_statement* ([Statements](statements.md)) production permits an additional construct, the `fixed` statement, which is used to "fix" a moveable variable such that its address remains constant for the duration of the statement.</span></span>

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

<span data-ttu-id="d9237-330">Cada *fixed_pointer_declarator* declara una variable local del *pointer_type* determinado e inicializa esa variable local con la dirección calculada por el *fixed_pointer_initializer*correspondiente.</span><span class="sxs-lookup"><span data-stu-id="d9237-330">Each *fixed_pointer_declarator* declares a local variable of the given *pointer_type* and initializes that local variable with the address computed by the corresponding *fixed_pointer_initializer*.</span></span> <span data-ttu-id="d9237-331">Una variable local declarada en una instrucción `fixed` es accesible en cualquier *fixed_pointer_initializer*que se encuentra a la derecha de la declaración de esa variable y en el *embedded_statement* de la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-331">A local variable declared in a `fixed` statement is accessible in any *fixed_pointer_initializer*s occurring to the right of that variable's declaration, and in the *embedded_statement* of the `fixed` statement.</span></span> <span data-ttu-id="d9237-332">Una variable local declarada por una instrucción `fixed` se considera de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="d9237-332">A local variable declared by a `fixed` statement is considered read-only.</span></span> <span data-ttu-id="d9237-333">Se produce un error en tiempo de compilación si la instrucción incrustada intenta modificar esta variable local (a través de la asignación o los operadores `++` y `--`) o pasarla como un parámetro de `ref` o `out`.</span><span class="sxs-lookup"><span data-stu-id="d9237-333">A compile-time error occurs if the embedded statement attempts to modify this local variable (via assignment or the `++` and `--` operators) or pass it as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="d9237-334">Una *fixed_pointer_initializer* puede ser una de las siguientes:</span><span class="sxs-lookup"><span data-stu-id="d9237-334">A *fixed_pointer_initializer* can be one of the following:</span></span>

*  <span data-ttu-id="d9237-335">El token "`&`" seguido de un *variable_reference* ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) a una variable móvil ([variables fijas y móviles](unsafe-code.md#fixed-and-moveable-variables)) de un tipo no administrado `T`, siempre que el tipo `T*` se pueda convertir implícitamente al tipo de puntero especificado en la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-335">The token "`&`" followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) to a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="d9237-336">En este caso, el inicializador calcula la dirección de la variable especificada y se garantiza que la variable permanece en una dirección fija mientras dure la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-336">In this case, the initializer computes the address of the given variable, and the variable is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>
*  <span data-ttu-id="d9237-337">Una expresión de un *array_type* con elementos de un tipo no administrado `T`, siempre que el tipo `T*` se pueda convertir implícitamente al tipo de puntero especificado en la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-337">An expression of an *array_type* with elements of an unmanaged type `T`, provided the type `T*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="d9237-338">En este caso, el inicializador calcula la dirección del primer elemento de la matriz y se garantiza que toda la matriz permanece en una dirección fija mientras dure la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-338">In this case, the initializer computes the address of the first element in the array, and the entire array is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="d9237-339">Si la expresión de matriz es null o si la matriz tiene cero elementos, el inicializador calcula una dirección igual a cero.</span><span class="sxs-lookup"><span data-stu-id="d9237-339">If the array expression is null or if the array has zero elements, the initializer computes an address equal to zero.</span></span>
*  <span data-ttu-id="d9237-340">Expresión de tipo `string`, siempre que el tipo `char*` se pueda convertir implícitamente al tipo de puntero proporcionado en la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-340">An expression of type `string`, provided the type `char*` is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="d9237-341">En este caso, el inicializador calcula la dirección del primer carácter de la cadena y se garantiza que toda la cadena permanece en una dirección fija mientras dure la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-341">In this case, the initializer computes the address of the first character in the string, and the entire string is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span> <span data-ttu-id="d9237-342">El comportamiento de la instrucción `fixed` es definido por la implementación si la expresión de cadena es NULL.</span><span class="sxs-lookup"><span data-stu-id="d9237-342">The behavior of the `fixed` statement is implementation-defined if the string expression is null.</span></span>
*  <span data-ttu-id="d9237-343">*Simple_name* o *member_access* que hace referencia a un miembro de búfer de tamaño fijo de una variable móvil, siempre que el tipo del miembro de búfer de tamaño fijo se pueda convertir implícitamente al tipo de puntero proporcionado en la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-343">A *simple_name* or *member_access* that references a fixed size buffer member of a moveable variable, provided the type of the fixed size buffer member is implicitly convertible to the pointer type given in the `fixed` statement.</span></span> <span data-ttu-id="d9237-344">En este caso, el inicializador calcula un puntero al primer elemento del búfer de tamaño fijo ([búferes de tamaño fijo en expresiones](unsafe-code.md#fixed-size-buffers-in-expressions)) y se garantiza que el búfer de tamaño fijo permanece en una dirección fija mientras dure la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-344">In this case, the initializer computes a pointer to the first element of the fixed size buffer ([Fixed size buffers in expressions](unsafe-code.md#fixed-size-buffers-in-expressions)), and the fixed size buffer is guaranteed to remain at a fixed address for the duration of the `fixed` statement.</span></span>

<span data-ttu-id="d9237-345">Para cada dirección calculada por una *fixed_pointer_initializer* la instrucción `fixed` garantiza que la variable a la que hace referencia la dirección no esté sujeta a reubicación o eliminación por parte del recolector de elementos no utilizados mientras dure la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-345">For each address computed by a *fixed_pointer_initializer* the `fixed` statement ensures that the variable referenced by the address is not subject to relocation or disposal by the garbage collector for the duration of the `fixed` statement.</span></span> <span data-ttu-id="d9237-346">Por ejemplo, si la dirección calculada por un *fixed_pointer_initializer* hace referencia a un campo de un objeto o un elemento de una instancia de matriz, la instrucción `fixed` garantiza que la instancia del objeto contenedor no se reubique o se elimine durante la duración de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="d9237-346">For example, if the address computed by a *fixed_pointer_initializer* references a field of an object or an element of an array instance, the `fixed` statement guarantees that the containing object instance is not relocated or disposed of during the lifetime of the statement.</span></span>

<span data-ttu-id="d9237-347">Es responsabilidad del programador garantizar que los punteros creados por `fixed` instrucciones no sobreviven más allá de la ejecución de esas instrucciones.</span><span class="sxs-lookup"><span data-stu-id="d9237-347">It is the programmer's responsibility to ensure that pointers created by `fixed` statements do not survive beyond execution of those statements.</span></span> <span data-ttu-id="d9237-348">Por ejemplo, cuando se pasan punteros creados por `fixed` instrucciones a las API externas, es responsabilidad del programador asegurarse de que las API no conservan memoria de estos punteros.</span><span class="sxs-lookup"><span data-stu-id="d9237-348">For example, when pointers created by `fixed` statements are passed to external APIs, it is the programmer's responsibility to ensure that the APIs retain no memory of these pointers.</span></span>

<span data-ttu-id="d9237-349">Los objetos fijos pueden provocar la fragmentación del montón (porque no se pueden moverse).</span><span class="sxs-lookup"><span data-stu-id="d9237-349">Fixed objects may cause fragmentation of the heap (because they can't be moved).</span></span> <span data-ttu-id="d9237-350">Por ese motivo, los objetos deben corregirse solo cuando sea absolutamente necesario y solo para la cantidad de tiempo más corta posible.</span><span class="sxs-lookup"><span data-stu-id="d9237-350">For that reason, objects should be fixed only when absolutely necessary and then only for the shortest amount of time possible.</span></span>

<span data-ttu-id="d9237-351">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9237-351">The example</span></span>

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

<span data-ttu-id="d9237-352">muestra varios usos de la instrucción `fixed`.</span><span class="sxs-lookup"><span data-stu-id="d9237-352">demonstrates several uses of the `fixed` statement.</span></span> <span data-ttu-id="d9237-353">La primera instrucción corrige y obtiene la dirección de un campo estático, la segunda instrucción corrige y obtiene la dirección de un campo de instancia, y la tercera instrucción corrige y obtiene la dirección de un elemento de matriz.</span><span class="sxs-lookup"><span data-stu-id="d9237-353">The first statement fixes and obtains the address of a static field, the second statement fixes and obtains the address of an instance field, and the third statement fixes and obtains the address of an array element.</span></span> <span data-ttu-id="d9237-354">En cada caso, se habría producido un error al usar el operador de `&` normal, ya que todas las variables se clasifican como variables móviles.</span><span class="sxs-lookup"><span data-stu-id="d9237-354">In each case it would have been an error to use the regular `&` operator since the variables are all classified as moveable variables.</span></span>

<span data-ttu-id="d9237-355">La cuarta instrucción `fixed` del ejemplo anterior genera un resultado similar al tercero.</span><span class="sxs-lookup"><span data-stu-id="d9237-355">The fourth `fixed` statement in the example above produces a similar result to the third.</span></span>

<span data-ttu-id="d9237-356">Este ejemplo de la instrucción `fixed` utiliza `string`:</span><span class="sxs-lookup"><span data-stu-id="d9237-356">This example of the `fixed` statement uses `string`:</span></span>

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

<span data-ttu-id="d9237-357">En un contexto no seguro, los elementos de las matrices unidimensionales se almacenan en un orden de índice creciente, empezando por el índice `0` y finalizando con `Length - 1`de índice.</span><span class="sxs-lookup"><span data-stu-id="d9237-357">In an unsafe context array elements of single-dimensional arrays are stored in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="d9237-358">En el caso de las matrices multidimensionales, los elementos de matriz se almacenan de forma que los índices de la dimensión situada más a la derecha aumenten primero, después la dimensión izquierda siguiente y así sucesivamente hacia la izquierda.</span><span class="sxs-lookup"><span data-stu-id="d9237-358">For multi-dimensional arrays, array elements are stored such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span> <span data-ttu-id="d9237-359">Dentro de una instrucción `fixed` que obtiene un puntero `p` a una instancia de matriz `a`, los valores de puntero que van desde `p` a `p + a.Length - 1` representan direcciones de los elementos de la matriz.</span><span class="sxs-lookup"><span data-stu-id="d9237-359">Within a `fixed` statement that obtains a pointer `p` to an array instance `a`, the pointer values ranging from `p` to `p + a.Length - 1` represent addresses of the elements in the array.</span></span> <span data-ttu-id="d9237-360">Del mismo modo, las variables que van desde `p[0]` a `p[a.Length - 1]` representan los elementos reales de la matriz.</span><span class="sxs-lookup"><span data-stu-id="d9237-360">Likewise, the variables ranging from `p[0]` to `p[a.Length - 1]` represent the actual array elements.</span></span> <span data-ttu-id="d9237-361">Dado el modo en que se almacenan las matrices, podemos tratar una matriz de cualquier dimensión como si fuera lineal.</span><span class="sxs-lookup"><span data-stu-id="d9237-361">Given the way in which arrays are stored, we can treat an array of any dimension as though it were linear.</span></span>

<span data-ttu-id="d9237-362">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d9237-362">For example:</span></span>

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

<span data-ttu-id="d9237-363">que genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="d9237-363">which produces the output:</span></span>

```console
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

<span data-ttu-id="d9237-364">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9237-364">In the example</span></span>

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

<span data-ttu-id="d9237-365">una instrucción `fixed` se usa para corregir una matriz, por lo que su dirección se puede pasar a un método que toma un puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-365">a `fixed` statement is used to fix an array so its address can be passed to a method that takes a pointer.</span></span>

<span data-ttu-id="d9237-366">En el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d9237-366">In the example:</span></span>

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

<span data-ttu-id="d9237-367">una instrucción Fixed se usa para corregir un búfer de tamaño fijo de una estructura, por lo que su dirección se puede usar como puntero.</span><span class="sxs-lookup"><span data-stu-id="d9237-367">a fixed statement is used to fix a fixed size buffer of a struct so its address can be used as a pointer.</span></span>

<span data-ttu-id="d9237-368">Un valor `char*` generado al corregir una instancia de cadena siempre apunta a una cadena terminada en NULL.</span><span class="sxs-lookup"><span data-stu-id="d9237-368">A `char*` value produced by fixing a string instance always points to a null-terminated string.</span></span> <span data-ttu-id="d9237-369">Dentro de una instrucción Fixed que obtiene un puntero `p` a una instancia de cadena `s`, los valores de puntero que van desde `p` a `p + s.Length - 1` representan direcciones de los caracteres de la cadena y el valor del puntero `p + s.Length` apunta siempre a un carácter nulo (el carácter con el valor `'\0'`).</span><span class="sxs-lookup"><span data-stu-id="d9237-369">Within a fixed statement that obtains a pointer `p` to a string instance `s`, the pointer values ranging from `p` to `p + s.Length - 1` represent addresses of the characters in the string, and the pointer value `p + s.Length` always points to a null character (the character with value `'\0'`).</span></span>

<span data-ttu-id="d9237-370">La modificación de objetos de tipo administrado a través de punteros fijos puede tener como resultado un comportamiento indefinido.</span><span class="sxs-lookup"><span data-stu-id="d9237-370">Modifying objects of managed type through fixed pointers can results in undefined behavior.</span></span> <span data-ttu-id="d9237-371">Por ejemplo, dado que las cadenas son inmutables, es responsabilidad del programador asegurarse de que los caracteres a los que hace referencia un puntero a una cadena fija no se modifican.</span><span class="sxs-lookup"><span data-stu-id="d9237-371">For example, because strings are immutable, it is the programmer's responsibility to ensure that the characters referenced by a pointer to a fixed string are not modified.</span></span>

<span data-ttu-id="d9237-372">La terminación automática de las cadenas es especialmente útil cuando se llama a las API externas que esperan cadenas de "estilo C".</span><span class="sxs-lookup"><span data-stu-id="d9237-372">The automatic null-termination of strings is particularly convenient when calling external APIs that expect "C-style" strings.</span></span> <span data-ttu-id="d9237-373">Sin embargo, tenga en cuenta que se permite que una instancia de cadena contenga caracteres nulos.</span><span class="sxs-lookup"><span data-stu-id="d9237-373">Note, however, that a string instance is permitted to contain null characters.</span></span> <span data-ttu-id="d9237-374">Si estos caracteres nulos están presentes, la cadena aparecerá truncada cuando se trate como una `char*`terminada en NULL.</span><span class="sxs-lookup"><span data-stu-id="d9237-374">If such null characters are present, the string will appear truncated when treated as a null-terminated `char*`.</span></span>

## <a name="fixed-size-buffers"></a><span data-ttu-id="d9237-375">Búferes de tamaño fijo</span><span class="sxs-lookup"><span data-stu-id="d9237-375">Fixed size buffers</span></span>

<span data-ttu-id="d9237-376">Los búferes de tamaño fijo se usan para declarar matrices en línea de "estilo C" como miembros de Structs y son principalmente útiles para interactuar con las API no administradas.</span><span class="sxs-lookup"><span data-stu-id="d9237-376">Fixed size buffers are used to declare "C style" in-line arrays as members of structs, and are primarily useful for interfacing with unmanaged APIs.</span></span>

### <a name="fixed-size-buffer-declarations"></a><span data-ttu-id="d9237-377">Declaraciones de búfer de tamaño fijo</span><span class="sxs-lookup"><span data-stu-id="d9237-377">Fixed size buffer declarations</span></span>

<span data-ttu-id="d9237-378">Un ***búfer de tamaño fijo*** es un miembro que representa el almacenamiento para un búfer de longitud fija de variables de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="d9237-378">A ***fixed size buffer*** is a member that represents storage for a fixed length buffer of variables of a given type.</span></span> <span data-ttu-id="d9237-379">Una declaración de búfer de tamaño fijo introduce uno o más búferes de tamaño fijo de un tipo de elemento determinado.</span><span class="sxs-lookup"><span data-stu-id="d9237-379">A fixed size buffer declaration introduces one or more fixed size buffers of a given element type.</span></span> <span data-ttu-id="d9237-380">Los búferes de tamaño fijo solo se permiten en declaraciones de struct y solo se pueden producir en contextos no seguros ([contextos no seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="d9237-380">Fixed size buffers are only permitted in struct declarations and can only occur in unsafe contexts ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

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

<span data-ttu-id="d9237-381">Una declaración de búfer de tamaño fijo puede incluir un conjunto de atributos ([atributos](attributes.md)), un modificador `new` ([Modificadores](classes.md#modifiers)), una combinación válida de los cuatro modificadores de acceso ([parámetros de tipo y restricciones](classes.md#type-parameters-and-constraints)) y un modificador `unsafe` ([contextos no seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="d9237-381">A fixed size buffer declaration may include a set of attributes ([Attributes](attributes.md)), a `new` modifier ([Modifiers](classes.md#modifiers)), a valid combination of the four access modifiers ([Type parameters and constraints](classes.md#type-parameters-and-constraints)) and an `unsafe` modifier ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="d9237-382">Los atributos y modificadores se aplican a todos los miembros declarados por la declaración de búfer de tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="d9237-382">The attributes and modifiers apply to all of the members declared by the fixed size buffer declaration.</span></span> <span data-ttu-id="d9237-383">Es un error que el mismo modificador aparezca varias veces en una declaración de búfer de tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="d9237-383">It is an error for the same modifier to appear multiple times in a fixed size buffer declaration.</span></span>

<span data-ttu-id="d9237-384">No se permite una declaración de búfer de tamaño fijo para incluir el modificador `static`.</span><span class="sxs-lookup"><span data-stu-id="d9237-384">A fixed size buffer declaration is not permitted to include the `static` modifier.</span></span>

<span data-ttu-id="d9237-385">El tipo de elemento de búfer de una declaración de búfer de tamaño fijo especifica el tipo de elemento de los búferes introducidos por la declaración.</span><span class="sxs-lookup"><span data-stu-id="d9237-385">The buffer element type of a fixed size buffer declaration specifies the element type of the buffer(s) introduced by the declaration.</span></span> <span data-ttu-id="d9237-386">El tipo de elemento de búfer debe ser uno de los tipos predefinidos `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`o `bool`.</span><span class="sxs-lookup"><span data-stu-id="d9237-386">The buffer element type must be one of the predefined types `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, or `bool`.</span></span>

<span data-ttu-id="d9237-387">El tipo de elemento de búfer va seguido de una lista de declaradores de búfer de tamaño fijo, cada uno de los cuales incorpora un nuevo miembro.</span><span class="sxs-lookup"><span data-stu-id="d9237-387">The buffer element type is followed by a list of fixed size buffer declarators, each of which introduces a new member.</span></span> <span data-ttu-id="d9237-388">Un declarador de búfer de tamaño fijo consta de un identificador que denomina al miembro, seguido de una expresión constante encerrada en `[` y `]` tokens.</span><span class="sxs-lookup"><span data-stu-id="d9237-388">A fixed size buffer declarator consists of an identifier that names the member, followed by a constant expression enclosed in `[` and `]` tokens.</span></span> <span data-ttu-id="d9237-389">La expresión constante denota el número de elementos del miembro introducido por ese declarador de búfer de tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="d9237-389">The constant expression denotes the number of elements in the member introduced by that fixed size buffer declarator.</span></span> <span data-ttu-id="d9237-390">El tipo de la expresión constante se debe poder convertir implícitamente al tipo `int`y el valor debe ser un entero positivo distinto de cero.</span><span class="sxs-lookup"><span data-stu-id="d9237-390">The type of the constant expression must be implicitly convertible to type `int`, and the value must be a non-zero positive integer.</span></span>

<span data-ttu-id="d9237-391">Se garantiza que los elementos de un búfer de tamaño fijo se organizan secuencialmente en la memoria.</span><span class="sxs-lookup"><span data-stu-id="d9237-391">The elements of a fixed size buffer are guaranteed to be laid out sequentially in memory.</span></span>

<span data-ttu-id="d9237-392">Una declaración de búfer de tamaño fijo que declara varios búferes de tamaño fijo es equivalente a varias declaraciones de una única declaración de búfer de tamaño fijo con los mismos atributos y tipos de elemento.</span><span class="sxs-lookup"><span data-stu-id="d9237-392">A fixed size buffer declaration that declares multiple fixed size buffers is equivalent to multiple declarations of a single fixed size buffer declaration with the same attributes, and element types.</span></span> <span data-ttu-id="d9237-393">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9237-393">For example</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

<span data-ttu-id="d9237-394">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="d9237-394">is equivalent to</span></span>

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a><span data-ttu-id="d9237-395">Búferes de tamaño fijo en expresiones</span><span class="sxs-lookup"><span data-stu-id="d9237-395">Fixed size buffers in expressions</span></span>

<span data-ttu-id="d9237-396">La búsqueda de miembros ([operadores](expressions.md#operators)) de un miembro de búfer de tamaño fijo continúa exactamente como la búsqueda de miembros de un campo.</span><span class="sxs-lookup"><span data-stu-id="d9237-396">Member lookup ([Operators](expressions.md#operators)) of a fixed size buffer member proceeds exactly like member lookup of a field.</span></span>

<span data-ttu-id="d9237-397">Se puede hacer referencia a un búfer de tamaño fijo en una expresión usando un *simple_name* ([inferencia de tipo](expressions.md#type-inference)) o un *member_access* ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="d9237-397">A fixed size buffer can be referenced in an expression using a *simple_name* ([Type inference](expressions.md#type-inference)) or a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="d9237-398">Cuando se hace referencia a un miembro de búfer de tamaño fijo como un nombre simple, el efecto es el mismo que el acceso de miembro del formulario `this.I`, donde `I` es el miembro de búfer de tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="d9237-398">When a fixed size buffer member is referenced as a simple name, the effect is the same as a member access of the form `this.I`, where `I` is the fixed size buffer member.</span></span>

<span data-ttu-id="d9237-399">En un acceso de miembro del formulario `E.I`, si `E` es de un tipo de struct y una búsqueda de miembro de `I` en ese tipo de struct identifica un miembro de tamaño fijo, `E.I` se evalúa como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="d9237-399">In a member access of the form `E.I`, if `E` is of a struct type and a member lookup of `I` in that struct type identifies a fixed size member, then `E.I` is evaluated an classified as follows:</span></span>

*  <span data-ttu-id="d9237-400">Si el `E.I` de la expresión no se produce en un contexto no seguro, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d9237-400">If the expression `E.I` does not occur in an unsafe context, a compile-time error occurs.</span></span>
*  <span data-ttu-id="d9237-401">Si `E` se clasifica como un valor, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d9237-401">If `E` is classified as a value, a compile-time error occurs.</span></span>
*  <span data-ttu-id="d9237-402">De lo contrario, si `E` es una variable móvil ([variables fijas y móviles](unsafe-code.md#fixed-and-moveable-variables)) y la expresión `E.I` no es un *fixed_pointer_initializer* ([la instrucción Fixed](unsafe-code.md#the-fixed-statement)), se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d9237-402">Otherwise, if `E` is a moveable variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)) and the expression `E.I` is not a *fixed_pointer_initializer* ([The fixed statement](unsafe-code.md#the-fixed-statement)), a compile-time error occurs.</span></span>
*  <span data-ttu-id="d9237-403">De lo contrario, `E` hace referencia a una variable fija y el resultado de la expresión es un puntero al primer elemento del miembro de búfer de tamaño fijo `I` en `E`.</span><span class="sxs-lookup"><span data-stu-id="d9237-403">Otherwise, `E` references a fixed variable and the result of the expression is a pointer to the first element of the fixed size buffer member `I` in `E`.</span></span> <span data-ttu-id="d9237-404">El resultado es de tipo `S*`, donde `S` es el tipo de elemento de `I`y se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="d9237-404">The result is of type `S*`, where `S` is the element type of `I`, and is classified as a value.</span></span>

<span data-ttu-id="d9237-405">Se puede tener acceso a los elementos subsiguientes del búfer de tamaño fijo mediante operaciones de puntero desde el primer elemento.</span><span class="sxs-lookup"><span data-stu-id="d9237-405">The subsequent elements of the fixed size buffer can be accessed using pointer operations from the first element.</span></span> <span data-ttu-id="d9237-406">A diferencia del acceso a las matrices, el acceso a los elementos de un búfer de tamaño fijo es una operación no segura y no se comprueba el intervalo.</span><span class="sxs-lookup"><span data-stu-id="d9237-406">Unlike access to arrays, access to the elements of a fixed size buffer is an unsafe operation and is not range checked.</span></span>

<span data-ttu-id="d9237-407">En el ejemplo siguiente se declara y se utiliza un struct con un miembro de búfer de tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="d9237-407">The following example declares and uses a struct with a fixed size buffer member.</span></span>

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

### <a name="definite-assignment-checking"></a><span data-ttu-id="d9237-408">Comprobación de asignación definitiva</span><span class="sxs-lookup"><span data-stu-id="d9237-408">Definite assignment checking</span></span>

<span data-ttu-id="d9237-409">Los búferes de tamaño fijo no están sujetos a la comprobación de asignación definitiva ([asignación definitiva](variables.md#definite-assignment)) y los miembros de búfer de tamaño fijo se omiten para la comprobación de asignación definitiva de variables de tipo struct.</span><span class="sxs-lookup"><span data-stu-id="d9237-409">Fixed size buffers are not subject to definite assignment checking ([Definite assignment](variables.md#definite-assignment)), and fixed size buffer members are ignored for purposes of definite assignment checking of struct type variables.</span></span>

<span data-ttu-id="d9237-410">Cuando la variable de estructura contenedora más externa de un miembro de búfer de tamaño fijo es una variable estática, una variable de instancia de una instancia de clase o un elemento de matriz, los elementos del búfer de tamaño fijo se inicializan automáticamente con sus valores predeterminados ([valores predeterminados](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="d9237-410">When the outermost containing struct variable of a fixed size buffer member is a static variable, an instance variable of a class instance, or an array element, the elements of the fixed size buffer are automatically initialized to their default values ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="d9237-411">En todos los demás casos, el contenido inicial de un búfer de tamaño fijo es indefinido.</span><span class="sxs-lookup"><span data-stu-id="d9237-411">In all other cases, the initial content of a fixed size buffer is undefined.</span></span>

## <a name="stack-allocation"></a><span data-ttu-id="d9237-412">Asignación de la pila</span><span class="sxs-lookup"><span data-stu-id="d9237-412">Stack allocation</span></span>

<span data-ttu-id="d9237-413">En un contexto no seguro, una declaración de variable local ([declaraciones de variables locales](statements.md#local-variable-declarations)) puede incluir un inicializador de asignación de pila que asigna memoria de la pila de llamadas.</span><span class="sxs-lookup"><span data-stu-id="d9237-413">In an unsafe context, a local variable declaration ([Local variable declarations](statements.md#local-variable-declarations)) may include a stack allocation initializer which allocates memory from the call stack.</span></span>

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="d9237-414">El *unmanaged_type* indica el tipo de los elementos que se almacenarán en la ubicación recién asignada y la *expresión* indica el número de estos elementos.</span><span class="sxs-lookup"><span data-stu-id="d9237-414">The *unmanaged_type* indicates the type of the items that will be stored in the newly allocated location, and the *expression* indicates the number of these items.</span></span> <span data-ttu-id="d9237-415">En conjunto, estos especifican el tamaño de asignación requerido.</span><span class="sxs-lookup"><span data-stu-id="d9237-415">Taken together, these specify the required allocation size.</span></span> <span data-ttu-id="d9237-416">Dado que el tamaño de una asignación de pila no puede ser negativo, se trata de un error en tiempo de compilación para especificar el número de elementos como un *constant_expression* que se evalúa como un valor negativo.</span><span class="sxs-lookup"><span data-stu-id="d9237-416">Since the size of a stack allocation cannot be negative, it is a compile-time error to specify the number of items as a *constant_expression* that evaluates to a negative value.</span></span>

<span data-ttu-id="d9237-417">Un inicializador de asignación de pila con el formato `stackalloc T[E]` requiere que `T` sea un tipo no administrado ([tipos de puntero](unsafe-code.md#pointer-types)) y que `E` sea una expresión de tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="d9237-417">A stack allocation initializer of the form `stackalloc T[E]` requires `T` to be an unmanaged type ([Pointer types](unsafe-code.md#pointer-types)) and `E` to be an expression of type `int`.</span></span> <span data-ttu-id="d9237-418">La construcción asigna `E * sizeof(T)` bytes de la pila de llamadas y devuelve un puntero, de tipo `T*`, al bloque recién asignado.</span><span class="sxs-lookup"><span data-stu-id="d9237-418">The construct allocates `E * sizeof(T)` bytes from the call stack and returns a pointer, of type `T*`, to the newly allocated block.</span></span> <span data-ttu-id="d9237-419">Si `E` es un valor negativo, el comportamiento es indefinido.</span><span class="sxs-lookup"><span data-stu-id="d9237-419">If `E` is a negative value, then the behavior is undefined.</span></span> <span data-ttu-id="d9237-420">Si `E` es cero, no se realiza ninguna asignación y el puntero devuelto está definido por la implementación.</span><span class="sxs-lookup"><span data-stu-id="d9237-420">If `E` is zero, then no allocation is made, and the pointer returned is implementation-defined.</span></span> <span data-ttu-id="d9237-421">Si no hay suficiente memoria disponible para asignar un bloque del tamaño determinado, se produce una `System.StackOverflowException`.</span><span class="sxs-lookup"><span data-stu-id="d9237-421">If there is not enough memory available to allocate a block of the given size, a `System.StackOverflowException` is thrown.</span></span>

<span data-ttu-id="d9237-422">El contenido de la memoria recién asignada está sin definir.</span><span class="sxs-lookup"><span data-stu-id="d9237-422">The content of the newly allocated memory is undefined.</span></span>

<span data-ttu-id="d9237-423">No se permiten inicializadores de asignación de pila en `catch` o bloques de `finally` ([la instrucción try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="d9237-423">Stack allocation initializers are not permitted in `catch` or `finally` blocks ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="d9237-424">No hay ninguna manera de liberar explícitamente la memoria asignada mediante `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="d9237-424">There is no way to explicitly free memory allocated using `stackalloc`.</span></span> <span data-ttu-id="d9237-425">Todos los bloques de memoria asignados a la pila creados durante la ejecución de un miembro de función se descartan automáticamente cuando se devuelve ese miembro de función.</span><span class="sxs-lookup"><span data-stu-id="d9237-425">All stack allocated memory blocks created during the execution of a function member are automatically discarded when that function member returns.</span></span> <span data-ttu-id="d9237-426">Esto corresponde a la función `alloca`, una extensión que se encuentra normalmente en C++ C y las implementaciones.</span><span class="sxs-lookup"><span data-stu-id="d9237-426">This corresponds to the `alloca` function, an extension commonly found in C and C++ implementations.</span></span>

<span data-ttu-id="d9237-427">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9237-427">In the example</span></span>

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

<span data-ttu-id="d9237-428">se usa un inicializador `stackalloc` en el método `IntToString` para asignar un búfer de 16 caracteres en la pila.</span><span class="sxs-lookup"><span data-stu-id="d9237-428">a `stackalloc` initializer is used in the `IntToString` method to allocate a buffer of 16 characters on the stack.</span></span> <span data-ttu-id="d9237-429">El búfer se descarta automáticamente cuando el método vuelve.</span><span class="sxs-lookup"><span data-stu-id="d9237-429">The buffer is automatically discarded when the method returns.</span></span>

## <a name="dynamic-memory-allocation"></a><span data-ttu-id="d9237-430">Asignación de memoria dinámica</span><span class="sxs-lookup"><span data-stu-id="d9237-430">Dynamic memory allocation</span></span>

<span data-ttu-id="d9237-431">Excepto en el caso del operador C# `stackalloc`, no proporciona construcciones predefinidas para administrar la memoria de recolección de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="d9237-431">Except for the `stackalloc` operator, C# provides no predefined constructs for managing non-garbage collected memory.</span></span> <span data-ttu-id="d9237-432">Estos servicios se proporcionan normalmente mediante bibliotecas de clases auxiliares o importados directamente desde el sistema operativo subyacente.</span><span class="sxs-lookup"><span data-stu-id="d9237-432">Such services are typically provided by supporting class libraries or imported directly from the underlying operating system.</span></span> <span data-ttu-id="d9237-433">Por ejemplo, la clase `Memory` siguiente ilustra cómo se puede tener acceso a las funciones del montón de un sistema operativo subyacente C#desde:</span><span class="sxs-lookup"><span data-stu-id="d9237-433">For example, the `Memory` class below illustrates how the heap functions of an underlying operating system might be accessed from C#:</span></span>

```csharp
using System;
using System.Runtime.InteropServices;

public static unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    private static readonly IntPtr s_heap = GetProcessHeap();

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size)
    {
        void* result = HeapAlloc(s_heap, HEAP_ZERO_MEMORY, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count)
    {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd)
        {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd)
        {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block)
    {
        if (!HeapFree(s_heap, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size)
    {
        void* result = HeapReAlloc(s_heap, HEAP_ZERO_MEMORY, block, (UIntPtr)size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block)
    {
        int result = (int)HeapSize(s_heap, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    private const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    private static extern IntPtr GetProcessHeap();

    [DllImport("kernel32")]
    private static extern void* HeapAlloc(IntPtr hHeap, int flags, UIntPtr size);

    [DllImport("kernel32")]
    private static extern bool HeapFree(IntPtr hHeap, int flags, void* block);

    [DllImport("kernel32")]
    private static extern void* HeapReAlloc(IntPtr hHeap, int flags, void* block, UIntPtr size);

    [DllImport("kernel32")]
    private static extern UIntPtr HeapSize(IntPtr hHeap, int flags, void* block);
}
```

<span data-ttu-id="d9237-434">A continuación se muestra un ejemplo que utiliza la clase `Memory`:</span><span class="sxs-lookup"><span data-stu-id="d9237-434">An example that uses the `Memory` class is given below:</span></span>

```csharp
class Test
{
    static unsafe void Main()
    {
        byte* buffer = null;
        try
        {
            const int Size = 256;
            buffer = (byte*)Memory.Alloc(Size);
            for (int i = 0; i < Size; i++) buffer[i] = (byte)i;
            byte[] array = new byte[Size];
            fixed (byte* p = array) Memory.Copy(buffer, p, Size);
            for (int i = 0; i < Size; i++) Console.WriteLine(array[i]);
        }
        finally
        {
            if (buffer != null) Memory.Free(buffer);
        }
    }
}
```

<span data-ttu-id="d9237-435">En el ejemplo se asignan 256 bytes de memoria a través de `Memory.Alloc` e inicializa el bloque de memoria con valores que aumentan de 0 a 255.</span><span class="sxs-lookup"><span data-stu-id="d9237-435">The example allocates 256 bytes of memory through `Memory.Alloc` and initializes the memory block with values increasing from 0 to 255.</span></span> <span data-ttu-id="d9237-436">A continuación, asigna una matriz de bytes de elemento 256 y usa `Memory.Copy` para copiar el contenido del bloque de memoria en la matriz de bytes.</span><span class="sxs-lookup"><span data-stu-id="d9237-436">It then allocates a 256 element byte array and uses `Memory.Copy` to copy the contents of the memory block into the byte array.</span></span> <span data-ttu-id="d9237-437">Por último, el bloque de memoria se libera utilizando `Memory.Free` y el contenido de la matriz de bytes se genera en la consola.</span><span class="sxs-lookup"><span data-stu-id="d9237-437">Finally, the memory block is freed using `Memory.Free` and the contents of the byte array are output on the console.</span></span>
