---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704027"
---
# <a name="structs"></a><span data-ttu-id="99c51-101">Estructuras</span><span class="sxs-lookup"><span data-stu-id="99c51-101">Structs</span></span>

<span data-ttu-id="99c51-102">Las estructuras son similares a las clases en que representan las estructuras de datos que pueden contener miembros de datos y miembros de función.</span><span class="sxs-lookup"><span data-stu-id="99c51-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="99c51-103">Sin embargo, a diferencia de las clases, las estructuras son tipos de valor y no requieren la asignación del montón.</span><span class="sxs-lookup"><span data-stu-id="99c51-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="99c51-104">Una variable de un tipo de estructura contiene directamente los datos del struct, mientras que una variable de un tipo de clase contiene una referencia a los datos, la última conocida como un objeto.</span><span class="sxs-lookup"><span data-stu-id="99c51-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="99c51-105">Los structs son particularmente útiles para estructuras de datos pequeñas que tengan semánticas de valor.</span><span class="sxs-lookup"><span data-stu-id="99c51-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="99c51-106">Los números complejos, los puntos de un sistema de coordenadas o los pares clave-valor de un diccionario son buenos ejemplos de structs.</span><span class="sxs-lookup"><span data-stu-id="99c51-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="99c51-107">La clave de estas estructuras de datos es que tienen pocos miembros de datos, que no requieren el uso de la herencia o la identidad referencial, y que se pueden implementar de forma cómoda mediante la semántica de valores donde la asignación copia el valor en lugar de la referencia.</span><span class="sxs-lookup"><span data-stu-id="99c51-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="99c51-108">Tal y como se describe en [tipos simples](types.md#simple-types), los tipos C#simples que proporciona, como `int`, `double`y `bool`, son en realidad todos los tipos de estructura.</span><span class="sxs-lookup"><span data-stu-id="99c51-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="99c51-109">Del mismo modo que estos tipos predefinidos son Structs, también es posible usar estructuras y sobrecarga de operadores para implementar nuevos tipos "primitivos" en el C# lenguaje.</span><span class="sxs-lookup"><span data-stu-id="99c51-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="99c51-110">Al final de este capítulo ([ejemplos de struct](structs.md#struct-examples)) se proporcionan dos ejemplos de estos tipos.</span><span class="sxs-lookup"><span data-stu-id="99c51-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="99c51-111">Declaraciones de struct</span><span class="sxs-lookup"><span data-stu-id="99c51-111">Struct declarations</span></span>

<span data-ttu-id="99c51-112">Una *struct_declaration* es una *type_declaration* ([declaraciones de tipos](namespaces.md#type-declarations)) que declara un nuevo struct:</span><span class="sxs-lookup"><span data-stu-id="99c51-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="99c51-113">Un *struct_declaration* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *struct_modifier*s ([modificadores de struct](structs.md#struct-modifiers)), seguido de un modificador de `partial` opcional, seguido de la palabra clave `struct` y un *identificador* que nombra el struct, seguido de una especificación de *type_parameter_list* opcional ([parámetros de tipo](classes.md#type-parameters)), seguida de una especificación de *struct_interfaces* opcional ([modificador parcial ](structs.md#partial-modifier)), seguido de una especificación de *type_parameter_constraints_clause*s opcional ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), seguida de un *struct_body* ([cuerpo de estructura](structs.md#struct-body)), opcionalmente seguido de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="99c51-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="99c51-114">Modificadores de struct</span><span class="sxs-lookup"><span data-stu-id="99c51-114">Struct modifiers</span></span>

<span data-ttu-id="99c51-115">Un *struct_declaration* puede incluir opcionalmente una secuencia de modificadores de struct:</span><span class="sxs-lookup"><span data-stu-id="99c51-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

<span data-ttu-id="99c51-116">Es un error en tiempo de compilación que el mismo modificador aparezca varias veces en una declaración de estructura.</span><span class="sxs-lookup"><span data-stu-id="99c51-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="99c51-117">Los modificadores de una declaración de struct tienen el mismo significado que los de una declaración de clase ([declaraciones de clase](classes.md#class-declarations)).</span><span class="sxs-lookup"><span data-stu-id="99c51-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="99c51-118">Unmodifier (modificador)</span><span class="sxs-lookup"><span data-stu-id="99c51-118">Partial modifier</span></span>

<span data-ttu-id="99c51-119">El modificador `partial` indica que este *struct_declaration* es una declaración de tipos parciales.</span><span class="sxs-lookup"><span data-stu-id="99c51-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="99c51-120">Varias declaraciones de struct parciales con el mismo nombre dentro de una declaración de tipo o espacio de nombres envolvente se combinan para formar una declaración de struct, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="99c51-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="99c51-121">Interfaces de struct</span><span class="sxs-lookup"><span data-stu-id="99c51-121">Struct interfaces</span></span>

<span data-ttu-id="99c51-122">Una declaración de estructura puede incluir una especificación de *struct_interfaces* , en cuyo caso se dice que el struct implementa directamente los tipos de interfaz especificados.</span><span class="sxs-lookup"><span data-stu-id="99c51-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="99c51-123">Las implementaciones de interfaz se tratan en las [implementaciones de interfaz](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="99c51-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="99c51-124">Cuerpo del struct</span><span class="sxs-lookup"><span data-stu-id="99c51-124">Struct body</span></span>

<span data-ttu-id="99c51-125">El *struct_body* de un struct define los miembros del struct.</span><span class="sxs-lookup"><span data-stu-id="99c51-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="99c51-126">Miembros de struct</span><span class="sxs-lookup"><span data-stu-id="99c51-126">Struct members</span></span>

<span data-ttu-id="99c51-127">Los miembros de una estructura se componen de los miembros introducidos por su *struct_member_declaration*s y los miembros heredados del tipo `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="99c51-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

<span data-ttu-id="99c51-128">A excepción de las diferencias que se indican en [diferencias entre clases y estructuras](structs.md#class-and-struct-differences), las descripciones de los miembros de clase proporcionados en [miembros de clase](classes.md#class-members) a través de [iteradores](classes.md#iterators) también se aplican a los miembros de estructura.</span><span class="sxs-lookup"><span data-stu-id="99c51-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="99c51-129">Diferencias entre clases y estructuras</span><span class="sxs-lookup"><span data-stu-id="99c51-129">Class and struct differences</span></span>

<span data-ttu-id="99c51-130">Los Structs difieren de las clases de varias maneras importantes:</span><span class="sxs-lookup"><span data-stu-id="99c51-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="99c51-131">Los Structs son tipos de valor ([semántica de valores](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="99c51-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="99c51-132">Todos los tipos de struct se heredan implícitamente de la clase `System.ValueType` ([herencia](structs.md#inheritance)).</span><span class="sxs-lookup"><span data-stu-id="99c51-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="99c51-133">La asignación a una variable de un tipo de struct crea una copia del valor que se asigna ([asignación](structs.md#assignment)).</span><span class="sxs-lookup"><span data-stu-id="99c51-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="99c51-134">El valor predeterminado de un struct es el valor generado al establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en `null` ([valores predeterminados](structs.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="99c51-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="99c51-135">Las operaciones de conversión boxing y unboxing se utilizan para realizar la conversión entre un tipo de struct y `object` ([conversión boxing y unboxing](structs.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="99c51-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="99c51-136">El significado de `this` es diferente para los Structs ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="99c51-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="99c51-137">Las declaraciones de campo de instancia de un struct no pueden incluir inicializadores variables ([inicializadores de campo](structs.md#field-initializers)).</span><span class="sxs-lookup"><span data-stu-id="99c51-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="99c51-138">Un struct no puede declarar un constructor de instancia sin parámetros ([constructores](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="99c51-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="99c51-139">Un struct no puede declarar un[destructor (](structs.md#destructors)destructores).</span><span class="sxs-lookup"><span data-stu-id="99c51-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="99c51-140">Semántica de valores</span><span class="sxs-lookup"><span data-stu-id="99c51-140">Value semantics</span></span>

<span data-ttu-id="99c51-141">Los Structs son tipos de valor ([tipos de valor](types.md#value-types)) y se dice que tienen semántica de valor.</span><span class="sxs-lookup"><span data-stu-id="99c51-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="99c51-142">Por otro lado, las clases son tipos de referencia ([tipos de referencia](types.md#reference-types)) y se dice que tienen semántica de referencia.</span><span class="sxs-lookup"><span data-stu-id="99c51-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="99c51-143">Una variable de un tipo de estructura contiene directamente los datos del struct, mientras que una variable de un tipo de clase contiene una referencia a los datos, la última conocida como un objeto.</span><span class="sxs-lookup"><span data-stu-id="99c51-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="99c51-144">Cuando un struct `B` contiene un campo de instancia de tipo `A` y `A` es un tipo de estructura, se trata de un error en tiempo de compilación para que `A` dependa de `B` o de un tipo construido a partir de `B`.</span><span class="sxs-lookup"><span data-stu-id="99c51-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="99c51-145">Un struct `X` ***depende directamente de*** un struct `Y` si `X` contiene un campo de instancia de tipo `Y`.</span><span class="sxs-lookup"><span data-stu-id="99c51-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="99c51-146">Dada esta definición, el conjunto completo de estructuras de las que depende un struct es el cierre transitivo de la relación ***directamente depende de*** .</span><span class="sxs-lookup"><span data-stu-id="99c51-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="99c51-147">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="99c51-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="99c51-148">es un error porque `Node` contiene un campo de instancia de su propio tipo.</span><span class="sxs-lookup"><span data-stu-id="99c51-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="99c51-149">Otro ejemplo</span><span class="sxs-lookup"><span data-stu-id="99c51-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="99c51-150">es un error porque cada uno de los tipos `A`, `B`y `C` dependen entre sí.</span><span class="sxs-lookup"><span data-stu-id="99c51-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="99c51-151">Con las clases, es posible que dos variables hagan referencia al mismo objeto y, por lo tanto, las operaciones en una variable afecten al objeto al que hace referencia la otra variable.</span><span class="sxs-lookup"><span data-stu-id="99c51-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="99c51-152">Con las estructuras, cada variable tiene su propia copia de los datos (excepto en el caso de las variables de parámetro `ref` y `out`) y no es posible que las operaciones en una afecten a la otra.</span><span class="sxs-lookup"><span data-stu-id="99c51-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="99c51-153">Además, dado que los Structs no son tipos de referencia, no es posible que se `null`los valores de un tipo struct.</span><span class="sxs-lookup"><span data-stu-id="99c51-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="99c51-154">Dada la declaración</span><span class="sxs-lookup"><span data-stu-id="99c51-154">Given the declaration</span></span>
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="99c51-155">fragmento de código</span><span class="sxs-lookup"><span data-stu-id="99c51-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="99c51-156">genera el valor `10`.</span><span class="sxs-lookup"><span data-stu-id="99c51-156">outputs the value `10`.</span></span> <span data-ttu-id="99c51-157">La asignación de `a` a `b` crea una copia del valor y, por tanto, `b` no se ve afectada por la asignación a `a.x`.</span><span class="sxs-lookup"><span data-stu-id="99c51-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="99c51-158">Si `Point` se hubieran declarado como una clase, el resultado sería `100` porque `a` y `b` harían referencia al mismo objeto.</span><span class="sxs-lookup"><span data-stu-id="99c51-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="99c51-159">Herencia</span><span class="sxs-lookup"><span data-stu-id="99c51-159">Inheritance</span></span>

<span data-ttu-id="99c51-160">Todos los tipos de struct se heredan implícitamente de la clase `System.ValueType`, que, a su vez, hereda de la clase `object`.</span><span class="sxs-lookup"><span data-stu-id="99c51-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="99c51-161">Una declaración de estructura puede especificar una lista de interfaces implementadas, pero no es posible que una declaración de struct especifique una clase base.</span><span class="sxs-lookup"><span data-stu-id="99c51-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="99c51-162">Los tipos de struct nunca son abstractos y siempre están sellados implícitamente.</span><span class="sxs-lookup"><span data-stu-id="99c51-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="99c51-163">Por lo tanto, los modificadores `abstract` y `sealed` no se permiten en una declaración de estructura.</span><span class="sxs-lookup"><span data-stu-id="99c51-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="99c51-164">Puesto que no se admite la herencia para Structs, la accesibilidad declarada de un miembro de struct no puede ser `protected` ni `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="99c51-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="99c51-165">Los miembros de función de una estructura no se pueden `abstract` ni `virtual`, y el modificador `override` solo se permite para reemplazar métodos heredados de `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="99c51-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="99c51-166">Asignación</span><span class="sxs-lookup"><span data-stu-id="99c51-166">Assignment</span></span>

<span data-ttu-id="99c51-167">La asignación a una variable de un tipo de struct crea una copia del valor que se va a asignar.</span><span class="sxs-lookup"><span data-stu-id="99c51-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="99c51-168">Esto difiere de la asignación a una variable de un tipo de clase, que copia la referencia pero no el objeto identificado por la referencia.</span><span class="sxs-lookup"><span data-stu-id="99c51-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="99c51-169">De forma similar a una asignación, cuando se pasa un struct como parámetro de valor o se devuelve como resultado de un miembro de función, se crea una copia de la estructura.</span><span class="sxs-lookup"><span data-stu-id="99c51-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="99c51-170">Un struct se puede pasar por referencia a un miembro de función mediante un parámetro `ref` o `out`.</span><span class="sxs-lookup"><span data-stu-id="99c51-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="99c51-171">Cuando una propiedad o un indizador de una estructura es el destino de una asignación, la expresión de instancia asociada con el acceso a la propiedad o indizador debe estar clasificada como una variable.</span><span class="sxs-lookup"><span data-stu-id="99c51-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="99c51-172">Si la expresión de instancia se clasifica como un valor, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="99c51-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="99c51-173">Esto se describe con más detalle en [asignación simple](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="99c51-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="99c51-174">Valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="99c51-174">Default values</span></span>

<span data-ttu-id="99c51-175">Tal y como se describe en [valores predeterminados](variables.md#default-values), varios tipos de variables se inicializan automáticamente en su valor predeterminado cuando se crean.</span><span class="sxs-lookup"><span data-stu-id="99c51-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="99c51-176">En el caso de las variables de tipos de clase y otros tipos de referencia, este valor predeterminado es `null`.</span><span class="sxs-lookup"><span data-stu-id="99c51-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="99c51-177">Sin embargo, dado que los Structs son tipos de valor que no se pueden `null`, el valor predeterminado de un struct es el valor generado al establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en `null`.</span><span class="sxs-lookup"><span data-stu-id="99c51-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="99c51-178">En el ejemplo se hace referencia al `Point` struct declarado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="99c51-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="99c51-179">Inicializa cada `Point` de la matriz con el valor generado al establecer los campos `x` y `y` en cero.</span><span class="sxs-lookup"><span data-stu-id="99c51-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="99c51-180">El valor predeterminado de un struct corresponde al valor devuelto por el constructor predeterminado de la estructura ([constructores predeterminados](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="99c51-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="99c51-181">A diferencia de una clase, un struct no puede declarar un constructor de instancia sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="99c51-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="99c51-182">En su lugar, cada struct tiene implícitamente un constructor de instancia sin parámetros que siempre devuelve el valor resultante de establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en `null`.</span><span class="sxs-lookup"><span data-stu-id="99c51-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="99c51-183">Los Structs se deben diseñar para considerar el estado de inicialización predeterminado como un estado válido.</span><span class="sxs-lookup"><span data-stu-id="99c51-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="99c51-184">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="99c51-184">In the example</span></span>
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
<span data-ttu-id="99c51-185">el constructor de instancia definido por el usuario protege solo los valores NULL cuando se llama explícitamente.</span><span class="sxs-lookup"><span data-stu-id="99c51-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="99c51-186">En los casos en los que una variable `KeyValuePair` está sujeta a la inicialización de valores predeterminados, los campos `key` y `value` serán NULL y el struct debe estar preparado para controlar este estado.</span><span class="sxs-lookup"><span data-stu-id="99c51-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="99c51-187">Conversión boxing y conversión unboxing</span><span class="sxs-lookup"><span data-stu-id="99c51-187">Boxing and unboxing</span></span>

<span data-ttu-id="99c51-188">Un valor de un tipo de clase se puede convertir al tipo `object` o a un tipo de interfaz implementado por la clase simplemente tratando la referencia como otro tipo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="99c51-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="99c51-189">Del mismo modo, un valor de tipo `object` o un valor de un tipo de interfaz se puede volver a convertir a un tipo de clase sin cambiar la referencia (pero, por supuesto, se requiere una comprobación de tipo en tiempo de ejecución en este caso).</span><span class="sxs-lookup"><span data-stu-id="99c51-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="99c51-190">Dado que los Structs no son tipos de referencia, estas operaciones se implementan de forma diferente para los tipos de struct.</span><span class="sxs-lookup"><span data-stu-id="99c51-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="99c51-191">Cuando un valor de un tipo struct se convierte al tipo `object` o a un tipo de interfaz implementado por el struct, se realiza una operación de conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="99c51-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="99c51-192">Del mismo modo, cuando un valor de tipo `object` o un valor de un tipo de interfaz se vuelve a convertir a un tipo de estructura, se produce una operación de conversión unboxing.</span><span class="sxs-lookup"><span data-stu-id="99c51-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="99c51-193">Una diferencia clave de las mismas operaciones en los tipos de clase es que la conversión boxing y la conversión unboxing copia el valor de la estructura dentro o fuera de la instancia de conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="99c51-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="99c51-194">Por lo tanto, después de una operación de conversión boxing o unboxing, los cambios realizados en la estructura desempaquetada no se reflejan en la estructura con conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="99c51-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="99c51-195">Cuando un tipo de struct invalida un método virtual heredado de `System.Object` (como `Equals`, `GetHashCode`o `ToString`), la invocación del método virtual a través de una instancia del tipo struct no produce la conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="99c51-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="99c51-196">Esto es así incluso cuando el struct se utiliza como parámetro de tipo y la invocación se produce a través de una instancia del tipo de parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="99c51-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="99c51-197">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="99c51-197">For example:</span></span>
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="99c51-198">La salida del programa es:</span><span class="sxs-lookup"><span data-stu-id="99c51-198">The output of the program is:</span></span>
```console
1
2
3
```

<span data-ttu-id="99c51-199">Aunque el estilo no es válido para que `ToString` tenga efectos secundarios, en el ejemplo se muestra que no se ha producido ninguna conversión boxing para las tres invocaciones de `x.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="99c51-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="99c51-200">Del mismo modo, la conversión boxing nunca se produce implícitamente cuando se obtiene acceso a un miembro en un parámetro de tipo restringido.</span><span class="sxs-lookup"><span data-stu-id="99c51-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="99c51-201">Por ejemplo, supongamos que una interfaz `ICounter` contiene un método `Increment` que se puede utilizar para modificar un valor.</span><span class="sxs-lookup"><span data-stu-id="99c51-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="99c51-202">Si `ICounter` se utiliza como una restricción, se llama a la implementación del método `Increment` con una referencia a la variable en la que se llamó `Increment`, nunca a una copia con conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="99c51-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

<span data-ttu-id="99c51-203">La primera llamada a `Increment` modifica el valor de la variable `x`.</span><span class="sxs-lookup"><span data-stu-id="99c51-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="99c51-204">Esto no es equivalente a la segunda llamada a `Increment`, que modifica el valor de una copia con conversión boxing de `x`.</span><span class="sxs-lookup"><span data-stu-id="99c51-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="99c51-205">Por lo tanto, la salida del programa es:</span><span class="sxs-lookup"><span data-stu-id="99c51-205">Thus, the output of the program is:</span></span>
```console
0
1
1
```

<span data-ttu-id="99c51-206">Para obtener más información sobre las conversiones boxing y unboxing, consulte [Boxing y unboxing](types.md#boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="99c51-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="99c51-207">Significado de este</span><span class="sxs-lookup"><span data-stu-id="99c51-207">Meaning of this</span></span>

<span data-ttu-id="99c51-208">Dentro de un constructor de instancia o un miembro de función de instancia de una clase, `this` se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="99c51-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="99c51-209">Por lo tanto, aunque se puede usar `this` para hacer referencia a la instancia para la que se invocó el miembro de función, no es posible asignar a `this` en un miembro de función de una clase.</span><span class="sxs-lookup"><span data-stu-id="99c51-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="99c51-210">Dentro de un constructor de instancia de un struct, `this` corresponde a un parámetro `out` del tipo struct y dentro de un miembro de función de instancia de un struct, `this` corresponde a un parámetro `ref` del tipo struct.</span><span class="sxs-lookup"><span data-stu-id="99c51-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="99c51-211">En ambos casos, `this` se clasifica como una variable y es posible modificar la estructura completa para la que se invocó el miembro de función mediante la asignación a `this` o pasando esto como un parámetro de `ref` o `out`.</span><span class="sxs-lookup"><span data-stu-id="99c51-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="99c51-212">Inicializadores de campo</span><span class="sxs-lookup"><span data-stu-id="99c51-212">Field initializers</span></span>

<span data-ttu-id="99c51-213">Como se describe en [valores predeterminados](structs.md#default-values), el valor predeterminado de un struct consta del valor que se obtiene al establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en `null`.</span><span class="sxs-lookup"><span data-stu-id="99c51-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="99c51-214">Por esta razón, un struct no permite que las declaraciones de campo de instancia incluyan inicializadores variables.</span><span class="sxs-lookup"><span data-stu-id="99c51-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="99c51-215">Esta restricción solo se aplica a los campos de instancia.</span><span class="sxs-lookup"><span data-stu-id="99c51-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="99c51-216">Los campos estáticos de un struct pueden incluir inicializadores variables.</span><span class="sxs-lookup"><span data-stu-id="99c51-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="99c51-217">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="99c51-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="99c51-218">es un error porque las declaraciones de campo de instancia incluyen inicializadores variables.</span><span class="sxs-lookup"><span data-stu-id="99c51-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="99c51-219">Constructores</span><span class="sxs-lookup"><span data-stu-id="99c51-219">Constructors</span></span>

<span data-ttu-id="99c51-220">A diferencia de una clase, un struct no puede declarar un constructor de instancia sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="99c51-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="99c51-221">En su lugar, cada struct tiene implícitamente un constructor de instancia sin parámetros que siempre devuelve el valor que se obtiene al establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en null ([constructores predeterminados](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="99c51-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="99c51-222">Un struct puede declarar constructores de instancia que tengan parámetros.</span><span class="sxs-lookup"><span data-stu-id="99c51-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="99c51-223">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="99c51-223">For example</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

<span data-ttu-id="99c51-224">Dada la declaración anterior, las instrucciones</span><span class="sxs-lookup"><span data-stu-id="99c51-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="99c51-225">ambos crean un `Point` con `x` y `y` inicializado en cero.</span><span class="sxs-lookup"><span data-stu-id="99c51-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="99c51-226">Un constructor de instancia de struct no puede incluir un inicializador de constructor con el formato `base(...)`.</span><span class="sxs-lookup"><span data-stu-id="99c51-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="99c51-227">Si el constructor de instancia de struct no especifica un inicializador de constructor, la variable `this` corresponde a un parámetro `out` del tipo struct y similar a un parámetro `out`, `this` debe estar asignado definitivamente ([asignación definitiva](variables.md#definite-assignment)) en cada ubicación donde el constructor devuelva.</span><span class="sxs-lookup"><span data-stu-id="99c51-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="99c51-228">Si el constructor de instancia de struct especifica un inicializador de constructor, la variable `this` corresponde a un parámetro `ref` del tipo struct y similar a un parámetro `ref`, `this` se considera definitivamente asignada en la entrada al cuerpo del constructor.</span><span class="sxs-lookup"><span data-stu-id="99c51-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="99c51-229">Considere la implementación del constructor de instancia a continuación:</span><span class="sxs-lookup"><span data-stu-id="99c51-229">Consider the instance constructor implementation below:</span></span>
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

<span data-ttu-id="99c51-230">No se puede llamar a ninguna función miembro de instancia (incluidos los descriptores de acceso set para las propiedades `X` y `Y`) hasta que todos los campos del struct que se está construyendo se hayan asignado definitivamente.</span><span class="sxs-lookup"><span data-stu-id="99c51-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="99c51-231">La única excepción implica las propiedades implementadas automáticamente ([propiedades implementadas automáticamente](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="99c51-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="99c51-232">Las reglas de asignación definitiva ([expresiones de asignación simple](variables.md#simple-assignment-expressions)) excluyen específicamente la asignación a una propiedad automática de un tipo de estructura dentro de un constructor de instancia de ese tipo de estructura: una asignación se considera una asignación definitiva del campo oculto de respaldo de la propiedad automática.</span><span class="sxs-lookup"><span data-stu-id="99c51-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="99c51-233">Por lo tanto, se permite lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="99c51-233">Thus, the following is allowed:</span></span>

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a><span data-ttu-id="99c51-234">Destructores</span><span class="sxs-lookup"><span data-stu-id="99c51-234">Destructors</span></span>

<span data-ttu-id="99c51-235">Un struct no puede declarar un destructor.</span><span class="sxs-lookup"><span data-stu-id="99c51-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="99c51-236">Constructores estáticos</span><span class="sxs-lookup"><span data-stu-id="99c51-236">Static constructors</span></span>

<span data-ttu-id="99c51-237">Los constructores estáticos para Structs siguen la mayoría de las mismas reglas que para las clases.</span><span class="sxs-lookup"><span data-stu-id="99c51-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="99c51-238">La ejecución de un constructor estático para un tipo de struct lo desencadena el primero de los siguientes eventos para que se produzca dentro de un dominio de aplicación:</span><span class="sxs-lookup"><span data-stu-id="99c51-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="99c51-239">Se hace referencia A un miembro estático del tipo struct.</span><span class="sxs-lookup"><span data-stu-id="99c51-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="99c51-240">Se llama a un constructor declarado explícitamente del tipo de estructura.</span><span class="sxs-lookup"><span data-stu-id="99c51-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="99c51-241">La creación de valores predeterminados ([valores predeterminados](structs.md#default-values)) de tipos struct no desencadena el constructor estático.</span><span class="sxs-lookup"><span data-stu-id="99c51-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="99c51-242">(Un ejemplo de esto es el valor inicial de los elementos de una matriz).</span><span class="sxs-lookup"><span data-stu-id="99c51-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="99c51-243">Ejemplos de struct</span><span class="sxs-lookup"><span data-stu-id="99c51-243">Struct examples</span></span>

<span data-ttu-id="99c51-244">A continuación se muestran dos ejemplos significativos del uso de `struct` tipos para crear tipos que se pueden usar de forma similar a los tipos predefinidos del lenguaje, pero con semántica modificada.</span><span class="sxs-lookup"><span data-stu-id="99c51-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="99c51-245">Tipo entero de base de datos</span><span class="sxs-lookup"><span data-stu-id="99c51-245">Database integer type</span></span>

<span data-ttu-id="99c51-246">El `DBInt` struct siguiente implementa un tipo entero que puede representar el conjunto completo de valores del tipo `int`, además de un estado adicional que indica un valor desconocido.</span><span class="sxs-lookup"><span data-stu-id="99c51-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="99c51-247">Un tipo con estas características se utiliza normalmente en las bases de datos.</span><span class="sxs-lookup"><span data-stu-id="99c51-247">A type with these characteristics is commonly used in databases.</span></span>

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a><span data-ttu-id="99c51-248">Tipo booleano de base de datos</span><span class="sxs-lookup"><span data-stu-id="99c51-248">Database boolean type</span></span>

<span data-ttu-id="99c51-249">El `DBBool` struct siguiente implementa un tipo lógico de tres valores.</span><span class="sxs-lookup"><span data-stu-id="99c51-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="99c51-250">Los valores posibles de este tipo son `DBBool.True`, `DBBool.False`y `DBBool.Null`, donde el miembro `Null` indica un valor desconocido.</span><span class="sxs-lookup"><span data-stu-id="99c51-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="99c51-251">Estos tipos lógicos de tres valores se utilizan normalmente en las bases de datos de.</span><span class="sxs-lookup"><span data-stu-id="99c51-251">Such three-valued logical types are commonly used in databases.</span></span>

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
