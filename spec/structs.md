# <a name="structs"></a><span data-ttu-id="24c9d-101">Estructuras</span><span class="sxs-lookup"><span data-stu-id="24c9d-101">Structs</span></span>

<span data-ttu-id="24c9d-102">Los structs son similares a las clases que representan las estructuras de datos que pueden contener miembros de datos y miembros de función.</span><span class="sxs-lookup"><span data-stu-id="24c9d-102">Structs are similar to classes in that they represent data structures that can contain data members and function members.</span></span> <span data-ttu-id="24c9d-103">Sin embargo, a diferencia de las clases, structs son tipos de valor y no requieren asignación del montón.</span><span class="sxs-lookup"><span data-stu-id="24c9d-103">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="24c9d-104">Una variable de un tipo de estructura contiene directamente los datos de la estructura, mientras que una variable de un tipo de clase contiene una referencia a los datos, que se conoce como un objeto.</span><span class="sxs-lookup"><span data-stu-id="24c9d-104">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span>

<span data-ttu-id="24c9d-105">Los structs son particularmente útiles para estructuras de datos pequeñas que tengan semánticas de valor.</span><span class="sxs-lookup"><span data-stu-id="24c9d-105">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="24c9d-106">Los números complejos, los puntos de un sistema de coordenadas o los pares clave-valor de un diccionario son buenos ejemplos de structs.</span><span class="sxs-lookup"><span data-stu-id="24c9d-106">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="24c9d-107">Clave de estas estructuras de datos es que tienen pocos miembros de datos, que no requieren el uso de herencia o identidad referencial, y que pueden ser implementadas convenientemente utilizando la semántica de valor donde la asignación copia el valor en lugar de la referencia.</span><span class="sxs-lookup"><span data-stu-id="24c9d-107">Key to these data structures is that they have few data members, that they do not require use of inheritance or referential identity, and that they can be conveniently implemented using value semantics where assignment copies the value instead of the reference.</span></span>

<span data-ttu-id="24c9d-108">Como se describe en [tipos simples](types.md#simple-types), los tipos simples que proporciona C#, tales como `int`, `double`, y `bool`, son en realidad todos los tipos de struct.</span><span class="sxs-lookup"><span data-stu-id="24c9d-108">As described in [Simple types](types.md#simple-types), the simple types provided by C#, such as `int`, `double`, and `bool`, are in fact all struct types.</span></span> <span data-ttu-id="24c9d-109">Como estos tipos predefinidos son estructuras, también es posible utilizar estructuras y sobrecarga de operadores para implementar nuevos tipos "primitivos" en el lenguaje C#.</span><span class="sxs-lookup"><span data-stu-id="24c9d-109">Just as these predefined types are structs, it is also possible to use structs and operator overloading to implement new "primitive" types in the C# language.</span></span> <span data-ttu-id="24c9d-110">Se proporcionan dos ejemplos de estos tipos al final de este capítulo ([ejemplos de estructuras](structs.md#struct-examples)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-110">Two examples of such types are given at the end of this chapter ([Struct examples](structs.md#struct-examples)).</span></span>

## <a name="struct-declarations"></a><span data-ttu-id="24c9d-111">Declaraciones de estructura</span><span class="sxs-lookup"><span data-stu-id="24c9d-111">Struct declarations</span></span>

<span data-ttu-id="24c9d-112">Un *struct_declaration* es un *type_declaration* ([declaraciones de tipo](namespaces.md#type-declarations)) que declara una nueva estructura:</span><span class="sxs-lookup"><span data-stu-id="24c9d-112">A *struct_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new struct:</span></span>

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

<span data-ttu-id="24c9d-113">Un *struct_declaration* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *struct_modifier*s ([Struct modificadores](structs.md#struct-modifiers)), seguido de un elemento opcional `partial` modificador, seguido por la palabra clave `struct` y un *identificador* que denomina el struct, seguido por un opcional *type_parameter_list* especificación ([parámetros de tipo](classes.md#type-parameters)), seguido de un elemento opcional *struct_interfaces* especificación ([Modificador parcial](structs.md#partial-modifier))), seguido de un elemento opcional *type_parameter_constraints_clause*especificación s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), seguido de un *struct_body* ([cuerpo Struct](structs.md#struct-body)), seguido opcionalmente de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="24c9d-113">A *struct_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *struct_modifier*s ([Struct modifiers](structs.md#struct-modifiers)), followed by an optional `partial` modifier, followed by the keyword `struct` and an *identifier* that names the struct, followed by an optional *type_parameter_list* specification ([Type parameters](classes.md#type-parameters)), followed by an optional *struct_interfaces* specification ([Partial modifier](structs.md#partial-modifier)) ), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *struct_body* ([Struct body](structs.md#struct-body)), optionally followed by a semicolon.</span></span>

### <a name="struct-modifiers"></a><span data-ttu-id="24c9d-114">Modificadores de estructura</span><span class="sxs-lookup"><span data-stu-id="24c9d-114">Struct modifiers</span></span>

<span data-ttu-id="24c9d-115">Un *struct_declaration* puede incluir opcionalmente una secuencia de modificadores de estructura:</span><span class="sxs-lookup"><span data-stu-id="24c9d-115">A *struct_declaration* may optionally include a sequence of struct modifiers:</span></span>

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

<span data-ttu-id="24c9d-116">Es un error en tiempo de compilación para el mismo modificador aparezca varias veces en una declaración de struct.</span><span class="sxs-lookup"><span data-stu-id="24c9d-116">It is a compile-time error for the same modifier to appear multiple times in a struct declaration.</span></span>

<span data-ttu-id="24c9d-117">Los modificadores de una declaración de estructura tienen el mismo significado que los de una declaración de clase ([declaraciones de clase](classes.md#class-declarations)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-117">The modifiers of a struct declaration have the same meaning as those of a class declaration ([Class declarations](classes.md#class-declarations)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="24c9d-118">Modificador parcial</span><span class="sxs-lookup"><span data-stu-id="24c9d-118">Partial modifier</span></span>

<span data-ttu-id="24c9d-119">El `partial` modificador indica que esto *struct_declaration* es una declaración de tipo parcial.</span><span class="sxs-lookup"><span data-stu-id="24c9d-119">The `partial` modifier indicates that this *struct_declaration* is a partial type declaration.</span></span> <span data-ttu-id="24c9d-120">Varias declaraciones de estructura parcial con el mismo nombre dentro de una declaración de espacio de nombres o tipo envolvente se combinan para formar una declaración de struct, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="24c9d-120">Multiple partial struct declarations with the same name within an enclosing namespace or type declaration combine to form one struct declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="struct-interfaces"></a><span data-ttu-id="24c9d-121">Interfaces de struct</span><span class="sxs-lookup"><span data-stu-id="24c9d-121">Struct interfaces</span></span>

<span data-ttu-id="24c9d-122">Puede incluir una declaración de struct un *struct_interfaces* especificación, en cuyo caso el struct se dice implementar directamente los tipos de interfaz determinado.</span><span class="sxs-lookup"><span data-stu-id="24c9d-122">A struct declaration may include a *struct_interfaces* specification, in which case the struct is said to directly implement the given interface types.</span></span>

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

<span data-ttu-id="24c9d-123">Implementaciones de interfaz se tratan con más detalle en [implementaciones de interfaz](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="24c9d-123">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="struct-body"></a><span data-ttu-id="24c9d-124">Cuerpo de estructura</span><span class="sxs-lookup"><span data-stu-id="24c9d-124">Struct body</span></span>

<span data-ttu-id="24c9d-125">El *struct_body* de una estructura define los miembros de struct.</span><span class="sxs-lookup"><span data-stu-id="24c9d-125">The *struct_body* of a struct defines the members of the struct.</span></span>

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a><span data-ttu-id="24c9d-126">Miembros de estructura</span><span class="sxs-lookup"><span data-stu-id="24c9d-126">Struct members</span></span>

<span data-ttu-id="24c9d-127">Los miembros de un struct se componen de los miembros introducidos por su *struct_member_declaration*s y los miembros heredan del tipo `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-127">The members of a struct consist of the members introduced by its *struct_member_declaration*s and the members inherited from the type `System.ValueType`.</span></span>

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

<span data-ttu-id="24c9d-128">Salvo por las diferencias que anotó en [diferencias entre clase y estructura](structs.md#class-and-struct-differences), las descripciones de los miembros de clase proporcionado en [los miembros de clase](classes.md#class-members) a través de [iteradores](classes.md#iterators) se aplican a struct miembros también.</span><span class="sxs-lookup"><span data-stu-id="24c9d-128">Except for the differences noted in [Class and struct differences](structs.md#class-and-struct-differences), the descriptions of class members provided in [Class members](classes.md#class-members) through [Iterators](classes.md#iterators) apply to struct members as well.</span></span>

## <a name="class-and-struct-differences"></a><span data-ttu-id="24c9d-129">Diferencias entre clase y estructura</span><span class="sxs-lookup"><span data-stu-id="24c9d-129">Class and struct differences</span></span>

<span data-ttu-id="24c9d-130">Structs difieren de las clases en varios aspectos importantes:</span><span class="sxs-lookup"><span data-stu-id="24c9d-130">Structs differ from classes in several important ways:</span></span>

*  <span data-ttu-id="24c9d-131">Los structs son tipos de valor ([semántica de valores](structs.md#value-semantics)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-131">Structs are value types ([Value semantics](structs.md#value-semantics)).</span></span>
*  <span data-ttu-id="24c9d-132">Todos los tipos de struct se heredan implícitamente de la clase `System.ValueType` ([herencia](structs.md#inheritance)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-132">All struct types implicitly inherit from the class `System.ValueType` ([Inheritance](structs.md#inheritance)).</span></span>
*  <span data-ttu-id="24c9d-133">La asignación a una variable de un tipo de estructura crea una copia del valor asignado ([asignación](structs.md#assignment)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-133">Assignment to a variable of a struct type creates a copy of the value being assigned ([Assignment](structs.md#assignment)).</span></span>
*  <span data-ttu-id="24c9d-134">El valor predeterminado de un struct es el valor generado al establecer todos los campos de tipo de valor en sus valores predeterminados y referencia de todos los campos de tipo a `null` ([los valores predeterminados](structs.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-134">The default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null` ([Default values](structs.md#default-values)).</span></span>
*  <span data-ttu-id="24c9d-135">Operaciones de conversión boxing y unboxing se utilizan para convertir entre un tipo struct y `object` ([conversiones Boxing y unboxing](structs.md#boxing-and-unboxing)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-135">Boxing and unboxing operations are used to convert between a struct type and `object` ([Boxing and unboxing](structs.md#boxing-and-unboxing)).</span></span>
*  <span data-ttu-id="24c9d-136">El significado de `this` es diferente para los structs ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-136">The meaning of `this` is different for structs ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="24c9d-137">Las declaraciones de campo de instancia para una estructura no pueden incluir inicializadores de variables ([inicializadores de campo](structs.md#field-initializers)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-137">Instance field declarations for a struct are not permitted to include variable initializers ([Field initializers](structs.md#field-initializers)).</span></span>
*  <span data-ttu-id="24c9d-138">Un struct no puede declarar un constructor de instancia sin parámetros ([constructores](structs.md#constructors)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-138">A struct is not permitted to declare a parameterless instance constructor ([Constructors](structs.md#constructors)).</span></span>
*  <span data-ttu-id="24c9d-139">Un struct no puede declarar un destructor ([destructores](structs.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-139">A struct is not permitted to declare a destructor ([Destructors](structs.md#destructors)).</span></span>

### <a name="value-semantics"></a><span data-ttu-id="24c9d-140">Semántica de valores</span><span class="sxs-lookup"><span data-stu-id="24c9d-140">Value semantics</span></span>

<span data-ttu-id="24c9d-141">Los structs son tipos de valor ([los tipos de valor](types.md#value-types)) y se dice que tienen semántica de valores.</span><span class="sxs-lookup"><span data-stu-id="24c9d-141">Structs are value types ([Value types](types.md#value-types)) and are said to have value semantics.</span></span> <span data-ttu-id="24c9d-142">Las clases, por otro lado, son tipos de referencia ([hacen referencia a tipos](types.md#reference-types)) y se dice que tienen semántica de referencia.</span><span class="sxs-lookup"><span data-stu-id="24c9d-142">Classes, on the other hand, are reference types ([Reference types](types.md#reference-types)) and are said to have reference semantics.</span></span>

<span data-ttu-id="24c9d-143">Una variable de un tipo de estructura contiene directamente los datos de la estructura, mientras que una variable de un tipo de clase contiene una referencia a los datos, que se conoce como un objeto.</span><span class="sxs-lookup"><span data-stu-id="24c9d-143">A variable of a struct type directly contains the data of the struct, whereas a variable of a class type contains a reference to the data, the latter known as an object.</span></span> <span data-ttu-id="24c9d-144">Cuando un struct `B` contiene un campo de instancia del tipo `A` y `A` es un tipo struct, es un error de tiempo de compilación de `A` depender `B` o un tipo construido a partir de `B`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-144">When a struct `B` contains an instance field of type `A` and `A` is a struct type, it is a compile-time error for `A` to depend on `B` or a type constructed from `B`.</span></span> <span data-ttu-id="24c9d-145">Un struct `X` ***depende directamente de*** un struct `Y` si `X` contiene un campo de instancia del tipo `Y`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-145">A struct `X` ***directly depends on*** a struct `Y` if `X` contains an instance field of type `Y`.</span></span> <span data-ttu-id="24c9d-146">Dada esta definición, el conjunto completo de las estructuras de los que depende un struct es el cierre transitivo de los ***depende directamente*** relación.</span><span class="sxs-lookup"><span data-stu-id="24c9d-146">Given this definition, the complete set of structs upon which a struct depends is the transitive closure of the ***directly depends on*** relationship.</span></span>  <span data-ttu-id="24c9d-147">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="24c9d-147">For example</span></span>
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
<span data-ttu-id="24c9d-148">es un error porque `Node` contiene un campo de instancia de su propio tipo.</span><span class="sxs-lookup"><span data-stu-id="24c9d-148">is an error because `Node` contains an instance field of its own type.</span></span>  <span data-ttu-id="24c9d-149">Otro ejemplo</span><span class="sxs-lookup"><span data-stu-id="24c9d-149">Another example</span></span>
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
<span data-ttu-id="24c9d-150">es un error porque cada uno de los tipos `A`, `B`, y `C` dependen entre sí.</span><span class="sxs-lookup"><span data-stu-id="24c9d-150">is an error because each of the types `A`, `B`, and `C` depend on each other.</span></span>

<span data-ttu-id="24c9d-151">Con las clases, es posible que dos variables hagan referencia al mismo objeto y, por tanto, las operaciones en una variable afecten al objeto al que hace referencia la otra variable.</span><span class="sxs-lookup"><span data-stu-id="24c9d-151">With classes, it is possible for two variables to reference the same object, and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="24c9d-152">Estructuras, cada variable tiene su propia copia de los datos (excepto en el caso de `ref` y `out` las variables de parámetro), y no es posible que las operaciones en una variable afecten a la otra.</span><span class="sxs-lookup"><span data-stu-id="24c9d-152">With structs, the variables each have their own copy of the data (except in the case of `ref` and `out` parameter variables), and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="24c9d-153">Además, dado que las estructuras no son tipos de referencia, no es posible para los valores de un tipo struct sea `null`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-153">Furthermore, because structs are not reference types, it is not possible for values of a struct type to be `null`.</span></span>

<span data-ttu-id="24c9d-154">Dada la declaración</span><span class="sxs-lookup"><span data-stu-id="24c9d-154">Given the declaration</span></span>
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
<span data-ttu-id="24c9d-155">el fragmento de código</span><span class="sxs-lookup"><span data-stu-id="24c9d-155">the code fragment</span></span>
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
<span data-ttu-id="24c9d-156">presenta el valor `10`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-156">outputs the value `10`.</span></span> <span data-ttu-id="24c9d-157">La asignación de `a` a `b` crea una copia del valor, y `b` , por tanto, no se ve afectado por la asignación a `a.x`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-157">The assignment of `a` to `b` creates a copy of the value, and `b` is thus unaffected by the assignment to `a.x`.</span></span> <span data-ttu-id="24c9d-158">Tenía `Point` en su lugar se ha sido declarado como una clase, el resultado sería `100` porque `a` y `b` haría referencia al mismo objeto.</span><span class="sxs-lookup"><span data-stu-id="24c9d-158">Had `Point` instead been declared as a class, the output would be `100` because `a` and `b` would reference the same object.</span></span>

### <a name="inheritance"></a><span data-ttu-id="24c9d-159">Herencia</span><span class="sxs-lookup"><span data-stu-id="24c9d-159">Inheritance</span></span>

<span data-ttu-id="24c9d-160">Todos los tipos de struct se heredan implícitamente de la clase `System.ValueType`, que, a su vez, hereda de la clase `object`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-160">All struct types implicitly inherit from the class `System.ValueType`, which, in turn, inherits from class `object`.</span></span> <span data-ttu-id="24c9d-161">Una declaración de estructura puede especificar una lista de interfaces implementadas, pero no es posible que una declaración de estructura especificar una clase base.</span><span class="sxs-lookup"><span data-stu-id="24c9d-161">A struct declaration may specify a list of implemented interfaces, but it is not possible for a struct declaration to specify a base class.</span></span>

<span data-ttu-id="24c9d-162">Tipos de estructura nunca son abstractos y son siempre sellados implícitamente.</span><span class="sxs-lookup"><span data-stu-id="24c9d-162">Struct types are never abstract and are always implicitly sealed.</span></span> <span data-ttu-id="24c9d-163">El `abstract` y `sealed` modificadores, por tanto, no se permiten en una declaración de struct.</span><span class="sxs-lookup"><span data-stu-id="24c9d-163">The `abstract` and `sealed` modifiers are therefore not permitted in a struct declaration.</span></span>

<span data-ttu-id="24c9d-164">Puesto que no se admite la herencia para los structs, la accesibilidad declarada de un miembro de estructura no puede ser `protected` o `protected internal`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-164">Since inheritance isn't supported for structs, the declared accessibility of a struct member cannot be `protected` or `protected internal`.</span></span>

<span data-ttu-id="24c9d-165">Miembros de función en un struct no pueden ser `abstract` o `virtual`y el `override` modificador sólo se permite para invalidar métodos heredados de `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-165">Function members in a struct cannot be `abstract` or `virtual`, and the `override` modifier is allowed only to override methods inherited from `System.ValueType`.</span></span>

### <a name="assignment"></a><span data-ttu-id="24c9d-166">Asignación</span><span class="sxs-lookup"><span data-stu-id="24c9d-166">Assignment</span></span>

<span data-ttu-id="24c9d-167">La asignación a una variable de un tipo de estructura crea una copia del valor asignado.</span><span class="sxs-lookup"><span data-stu-id="24c9d-167">Assignment to a variable of a struct type creates a copy of the value being assigned.</span></span> <span data-ttu-id="24c9d-168">Esto difiere de la asignación a una variable de un tipo de clase, que copia la referencia pero no el objeto identificado por la referencia.</span><span class="sxs-lookup"><span data-stu-id="24c9d-168">This differs from assignment to a variable of a class type, which copies the reference but not the object identified by the reference.</span></span>

<span data-ttu-id="24c9d-169">Al igual que una asignación, cuando un struct se pasa como un parámetro de valor o se devuelve como resultado de un miembro de función, se crea una copia de la estructura.</span><span class="sxs-lookup"><span data-stu-id="24c9d-169">Similar to an assignment, when a struct is passed as a value parameter or returned as the result of a function member, a copy of the struct is created.</span></span> <span data-ttu-id="24c9d-170">Un struct puede pasarse por referencia a un miembro de función con un `ref` o `out` parámetro.</span><span class="sxs-lookup"><span data-stu-id="24c9d-170">A struct may be passed by reference to a function member using a `ref` or `out` parameter.</span></span>

<span data-ttu-id="24c9d-171">Cuando una propiedad o indizador de una estructura es el destino de una asignación, la expresión de instancia asociada con el acceso de propiedad o indizador debe estar clasificada como una variable.</span><span class="sxs-lookup"><span data-stu-id="24c9d-171">When a property or indexer of a struct is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="24c9d-172">Si la expresión de instancia se clasifica como un valor, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="24c9d-172">If the instance expression is classified as a value, a compile-time error occurs.</span></span> <span data-ttu-id="24c9d-173">Esto se describe con más detalle en [asignación Simple](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="24c9d-173">This is described in further detail in [Simple assignment](expressions.md#simple-assignment).</span></span>

### <a name="default-values"></a><span data-ttu-id="24c9d-174">Valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="24c9d-174">Default values</span></span>

<span data-ttu-id="24c9d-175">Como se describe en [los valores predeterminados](variables.md#default-values), varios tipos de variables se inicializan automáticamente en su valor predeterminado cuando se crean.</span><span class="sxs-lookup"><span data-stu-id="24c9d-175">As described in [Default values](variables.md#default-values), several kinds of variables are automatically initialized to their default value when they are created.</span></span> <span data-ttu-id="24c9d-176">Para las variables de tipos de clase y otros tipos de referencia, este valor predeterminado es `null`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-176">For variables of class types and other reference types, this default value is `null`.</span></span> <span data-ttu-id="24c9d-177">Sin embargo, dado que los structs son tipos de valor no pueden ser `null`, el valor predeterminado de un struct es el valor generado al establecer todos los campos de tipo de valor en sus valores predeterminados y referencia de todos los campos de tipo a `null`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-177">However, since structs are value types that cannot be `null`, the default value of a struct is the value produced by setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="24c9d-178">Que hace referencia a la `Point` estructura declarada anteriormente, en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="24c9d-178">Referring to the `Point` struct declared above, the example</span></span>
```csharp
Point[] a = new Point[100];
```
<span data-ttu-id="24c9d-179">Inicializa cada `Point` en la matriz en el valor generado al establecer el `x` y `y` campos en cero.</span><span class="sxs-lookup"><span data-stu-id="24c9d-179">initializes each `Point` in the array to the value produced by setting the `x` and `y` fields to zero.</span></span>

<span data-ttu-id="24c9d-180">El valor predeterminado de un struct se corresponde con el valor devuelto por el constructor predeterminado del struct ([constructores predeterminados](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-180">The default value of a struct corresponds to the value returned by the default constructor of the struct ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="24c9d-181">A diferencia de una clase, un struct no se permite declarar un constructor de instancia sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="24c9d-181">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="24c9d-182">En su lugar, cada estructura tiene implícitamente un constructor de instancia sin parámetros que siempre devuelve el valor que es el resultado de establecer todos los campos de tipo de valor a sus valores predeterminados y referencia de todos los campos de tipo a `null`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-182">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span>

<span data-ttu-id="24c9d-183">Las estructuras deben diseñarse a tener en cuenta el estado de inicialización predeterminado con un estado válido.</span><span class="sxs-lookup"><span data-stu-id="24c9d-183">Structs should be designed to consider the default initialization state a valid state.</span></span> <span data-ttu-id="24c9d-184">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="24c9d-184">In the example</span></span>
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
<span data-ttu-id="24c9d-185">el constructor de instancia definido por el usuario protege contra los valores null solo cuando es llamado explícitamente.</span><span class="sxs-lookup"><span data-stu-id="24c9d-185">the user-defined instance constructor protects against null values only where it is explicitly called.</span></span> <span data-ttu-id="24c9d-186">En casos donde un `KeyValuePair` variable está sujeta a la inicialización de un valor predeterminado, el `key` y `value` campos será nulos y la estructura debe estar preparada para controlar este estado.</span><span class="sxs-lookup"><span data-stu-id="24c9d-186">In cases where a `KeyValuePair` variable is subject to default value initialization, the `key` and `value` fields will be null, and the struct must be prepared to handle this state.</span></span>

### <a name="boxing-and-unboxing"></a><span data-ttu-id="24c9d-187">Conversión boxing y conversión unboxing</span><span class="sxs-lookup"><span data-stu-id="24c9d-187">Boxing and unboxing</span></span>

<span data-ttu-id="24c9d-188">Un valor de un tipo de clase se puede convertir al tipo `object` o a un tipo de interfaz que es implementado por la clase simplemente tratando la referencia como otro tipo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="24c9d-188">A value of a class type can be converted to type `object` or to an interface type that is implemented by the class simply by treating the reference as another type at compile-time.</span></span> <span data-ttu-id="24c9d-189">Del mismo modo, un valor de tipo `object` o un valor de un tipo de interfaz se puede convertir a un tipo de clase sin cambiar la referencia (pero evidentemente una comprobación de tipo de tiempo de ejecución es necesaria en este caso).</span><span class="sxs-lookup"><span data-stu-id="24c9d-189">Likewise, a value of type `object` or a value of an interface type can be converted back to a class type without changing the reference (but of course a run-time type check is required in this case).</span></span>

<span data-ttu-id="24c9d-190">Dado que las estructuras no son tipos de referencia, estas operaciones se implementan de forma diferente para tipos de estructura.</span><span class="sxs-lookup"><span data-stu-id="24c9d-190">Since structs are not reference types, these operations are implemented differently for struct types.</span></span> <span data-ttu-id="24c9d-191">Cuando un valor de un tipo de estructura se convierte al tipo `object` o a un tipo de interfaz implementada por la estructura, realiza una operación de conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="24c9d-191">When a value of a struct type is converted to type `object` or to an interface type that is implemented by the struct, a boxing operation takes place.</span></span> <span data-ttu-id="24c9d-192">Del mismo modo, cuando un valor de tipo `object` o un valor de un tipo de interfaz se convierte a un tipo de estructura, realiza una operación de conversión unboxing.</span><span class="sxs-lookup"><span data-stu-id="24c9d-192">Likewise, when a value of type `object` or a value of an interface type is converted back to a struct type, an unboxing operation takes place.</span></span> <span data-ttu-id="24c9d-193">Una diferencia fundamental con respecto a las mismas operaciones en tipos de clase es que conversión boxing y unboxing copian el valor de struct, ya sea dentro o fuera de la instancia de la conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="24c9d-193">A key difference from the same operations on class types is that boxing and unboxing copies the struct value either into or out of the boxed instance.</span></span> <span data-ttu-id="24c9d-194">Por lo tanto, después de una operación de conversión boxing o conversión unboxing, los cambios realizados en la estructura de conversión unboxing no se reflejan en la estructura de conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="24c9d-194">Thus, following a boxing or unboxing operation, changes made to the unboxed struct are not reflected in the boxed struct.</span></span>

<span data-ttu-id="24c9d-195">Cuando un tipo struct invalida un método virtual heredado de `System.Object` (como `Equals`, `GetHashCode`, o `ToString`), la invocación del método virtual a través de una instancia del tipo struct no provoca la conversión boxing se producen.</span><span class="sxs-lookup"><span data-stu-id="24c9d-195">When a struct type overrides a virtual method inherited from `System.Object` (such as `Equals`, `GetHashCode`, or `ToString`), invocation of the virtual method through an instance of the struct type does not cause boxing to occur.</span></span> <span data-ttu-id="24c9d-196">Esto es cierto incluso cuando se utiliza la estructura como un parámetro de tipo y la invocación se produce a través de una instancia del tipo de parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="24c9d-196">This is true even when the struct is used as a type parameter and the invocation occurs through an instance of the type parameter type.</span></span> <span data-ttu-id="24c9d-197">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="24c9d-197">For example:</span></span>
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

<span data-ttu-id="24c9d-198">La salida del programa es:</span><span class="sxs-lookup"><span data-stu-id="24c9d-198">The output of the program is:</span></span>
```
1
2
3
```

<span data-ttu-id="24c9d-199">Aunque es poco recomendable `ToString` para tener efectos secundarios, el ejemplo se muestra que se ha producido ninguna conversión boxing para las tres invocaciones de `x.ToString()`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-199">Although it is bad style for `ToString` to have side effects, the example demonstrates that no boxing occurred for the three invocations of `x.ToString()`.</span></span>

<span data-ttu-id="24c9d-200">De forma similar, la conversión boxing implícitamente nunca se produce cuando el acceso a un miembro en un parámetro de tipo restringido.</span><span class="sxs-lookup"><span data-stu-id="24c9d-200">Similarly, boxing never implicitly occurs when accessing a member on a constrained type parameter.</span></span> <span data-ttu-id="24c9d-201">Por ejemplo, supongamos que una interfaz `ICounter` contiene un método `Increment` que puede usarse para modificar un valor.</span><span class="sxs-lookup"><span data-stu-id="24c9d-201">For example, suppose an interface `ICounter` contains a method `Increment` which can be used to modify a value.</span></span> <span data-ttu-id="24c9d-202">Si `ICounter` se utiliza como una restricción, la implementación de la `Increment` se llama al método con una referencia a la variable que `Increment` se llamó en nunca una copia con conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="24c9d-202">If `ICounter` is used as a constraint, the implementation of the `Increment` method is called with a reference to the variable that `Increment` was called on, never a boxed copy.</span></span>

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

<span data-ttu-id="24c9d-203">La primera llamada a `Increment` modifica el valor en la variable `x`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-203">The first call to `Increment` modifies the value in the variable `x`.</span></span> <span data-ttu-id="24c9d-204">Esto no es equivalente a la segunda llamada a `Increment`, que modifica el valor de una copia encuadrada de `x`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-204">This is not equivalent to the second call to `Increment`, which modifies the value in a boxed copy of `x`.</span></span> <span data-ttu-id="24c9d-205">Por lo tanto, la salida del programa es:</span><span class="sxs-lookup"><span data-stu-id="24c9d-205">Thus, the output of the program is:</span></span>
```
0
1
1
```

<span data-ttu-id="24c9d-206">Para obtener más información sobre las conversiones boxing y unboxing, consulte [conversiones Boxing y unboxing](types.md#boxing-and-unboxing).</span><span class="sxs-lookup"><span data-stu-id="24c9d-206">For further details on boxing and unboxing, see [Boxing and unboxing](types.md#boxing-and-unboxing).</span></span>

### <a name="meaning-of-this"></a><span data-ttu-id="24c9d-207">Significado de esto</span><span class="sxs-lookup"><span data-stu-id="24c9d-207">Meaning of this</span></span>

<span data-ttu-id="24c9d-208">Dentro de un constructor de instancia o un miembro de función de la instancia de una clase, `this` se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="24c9d-208">Within an instance constructor or instance function member of a class, `this` is classified as a value.</span></span> <span data-ttu-id="24c9d-209">Por lo tanto, mientras `this` puede usarse para hacer referencia a la instancia para que se invoca el miembro de función, no es posible asignar a `this` en un miembro de función de una clase.</span><span class="sxs-lookup"><span data-stu-id="24c9d-209">Thus, while `this` can be used to refer to the instance for which the function member was invoked, it is not possible to assign to `this` in a function member of a class.</span></span>

<span data-ttu-id="24c9d-210">Dentro de un constructor de instancia de un struct, `this` corresponde a un `out` parámetro del tipo struct y dentro de un miembro de función de la instancia de un struct, `this` corresponde a un `ref` parámetro del tipo struct.</span><span class="sxs-lookup"><span data-stu-id="24c9d-210">Within an instance constructor of a struct, `this` corresponds to an `out` parameter of the struct type, and within an instance function member of a struct, `this` corresponds to a `ref` parameter of the struct type.</span></span> <span data-ttu-id="24c9d-211">En ambos casos, `this` se clasifica como una variable, y es posible modificar el struct completo para el que el miembro de función se invocó mediante la asignación a `this` o si se pasa como un `ref` o `out` parámetro.</span><span class="sxs-lookup"><span data-stu-id="24c9d-211">In both cases, `this` is classified as a variable, and it is possible to modify the entire struct for which the function member was invoked by assigning to `this` or by passing this as a `ref` or `out` parameter.</span></span>

### <a name="field-initializers"></a><span data-ttu-id="24c9d-212">Inicializadores de campo</span><span class="sxs-lookup"><span data-stu-id="24c9d-212">Field initializers</span></span>

<span data-ttu-id="24c9d-213">Como se describe en [los valores predeterminados](structs.md#default-values), el valor predeterminado de una estructura formada por el valor que es el resultado de establecer todos los campos de tipo de valor a sus valores predeterminados y referencia de todos los campos de tipo a `null`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-213">As described in [Default values](structs.md#default-values), the default value of a struct consists of the value that results from setting all value type fields to their default value and all reference type fields to `null`.</span></span> <span data-ttu-id="24c9d-214">Por este motivo, un struct no permite que las declaraciones de campo de instancia para incluir a inicializadores de variables.</span><span class="sxs-lookup"><span data-stu-id="24c9d-214">For this reason, a struct does not permit instance field declarations to include variable initializers.</span></span> <span data-ttu-id="24c9d-215">Esta restricción se aplica solo a los campos de instancia.</span><span class="sxs-lookup"><span data-stu-id="24c9d-215">This restriction applies only to instance fields.</span></span> <span data-ttu-id="24c9d-216">Los campos estáticos de una estructura pueden incluir a inicializadores de variables.</span><span class="sxs-lookup"><span data-stu-id="24c9d-216">Static fields of a struct are permitted to include variable initializers.</span></span>

<span data-ttu-id="24c9d-217">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="24c9d-217">The example</span></span>
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
<span data-ttu-id="24c9d-218">es un error porque las declaraciones de campo de instancia incluyen a inicializadores de variables.</span><span class="sxs-lookup"><span data-stu-id="24c9d-218">is in error because the instance field declarations include variable initializers.</span></span>

### <a name="constructors"></a><span data-ttu-id="24c9d-219">Constructores</span><span class="sxs-lookup"><span data-stu-id="24c9d-219">Constructors</span></span>

<span data-ttu-id="24c9d-220">A diferencia de una clase, un struct no se permite declarar un constructor de instancia sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="24c9d-220">Unlike a class, a struct is not permitted to declare a parameterless instance constructor.</span></span> <span data-ttu-id="24c9d-221">En su lugar, cada estructura tiene implícitamente un constructor de instancia sin parámetros que siempre devuelve el valor que se obtiene al establecer todos los campos de tipo de valor a sus valores predeterminados y referencia de todos los campos de tipo null ([constructorespredeterminados](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-221">Instead, every struct implicitly has a parameterless instance constructor which always returns the value that results from setting all value type fields to their default value and all reference type fields to null ([Default constructors](types.md#default-constructors)).</span></span> <span data-ttu-id="24c9d-222">Un struct puede declarar constructores de instancia con parámetros.</span><span class="sxs-lookup"><span data-stu-id="24c9d-222">A struct can declare instance constructors having parameters.</span></span> <span data-ttu-id="24c9d-223">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="24c9d-223">For example</span></span>
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

<span data-ttu-id="24c9d-224">Dada la declaración anterior, las instrucciones</span><span class="sxs-lookup"><span data-stu-id="24c9d-224">Given the above declaration, the statements</span></span>
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
<span data-ttu-id="24c9d-225">ambos crean un `Point` con `x` y `y` inicializadas en cero.</span><span class="sxs-lookup"><span data-stu-id="24c9d-225">both create a `Point` with `x` and `y` initialized to zero.</span></span>

<span data-ttu-id="24c9d-226">Un constructor de instancia de struct no puede incluir un inicializador de constructor del formulario `base(...)`.</span><span class="sxs-lookup"><span data-stu-id="24c9d-226">A struct instance constructor is not permitted to include a constructor initializer of the form `base(...)`.</span></span>

<span data-ttu-id="24c9d-227">Si el constructor de instancia de struct no especifica un inicializador de constructor, el `this` variable corresponde a un `out` parámetro de tipo de estructura y otros similares a un `out` parámetro, `this` debe asignarlo definitivamente () [Asignación definitiva](variables.md#definite-assignment)) en todas las ubicaciones donde se devuelve el constructor.</span><span class="sxs-lookup"><span data-stu-id="24c9d-227">If the struct instance constructor doesn't specify a constructor initializer, the `this` variable corresponds to an `out` parameter of the struct type, and similar to an `out` parameter, `this` must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at every location where the constructor returns.</span></span> <span data-ttu-id="24c9d-228">Si el constructor de instancia de struct especifica un inicializador de constructor, el `this` variable corresponde a un `ref` parámetro de tipo de estructura y otros similares a un `ref` parámetro, `this` se considera asignado definitivamente en entrada en el cuerpo del constructor.</span><span class="sxs-lookup"><span data-stu-id="24c9d-228">If the struct instance constructor specifies a constructor initializer, the `this` variable corresponds to a `ref` parameter of the struct type, and similar to a `ref` parameter, `this` is considered definitely assigned on entry to the constructor body.</span></span> <span data-ttu-id="24c9d-229">Tenga en cuenta la siguiente implementación de constructor de instancia:</span><span class="sxs-lookup"><span data-stu-id="24c9d-229">Consider the instance constructor implementation below:</span></span>
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

<span data-ttu-id="24c9d-230">Ninguna función miembro de instancia (incluidos los descriptores de acceso set de las propiedades `X` y `Y`) se puede llamar hasta que se han asignado definitivamente todos los campos de la estructura que se está construyendo.</span><span class="sxs-lookup"><span data-stu-id="24c9d-230">No instance member function (including the set accessors for the properties `X` and `Y`) can be called until all fields of the struct being constructed have been definitely assigned.</span></span> <span data-ttu-id="24c9d-231">La única excepción implica las propiedades implementadas automáticamente ([implementa automáticamente propiedades](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="24c9d-231">The only exception involves automatically implemented properties ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="24c9d-232">Las reglas de asignación definitiva ([expresiones de asignación Simple](variables.md#simple-assignment-expressions)) excluir específicamente la asignación a una propiedad automática de un tipo de estructura dentro de un constructor de instancia de ese tipo de struct: esta asignación se considera un definitiva asignación del campo oculto de respaldo de la propiedad automática.</span><span class="sxs-lookup"><span data-stu-id="24c9d-232">The definite assignment rules ([Simple assignment expressions](variables.md#simple-assignment-expressions)) specifically exempt assignment to an auto-property of a struct type within an instance constructor of that struct type: such an assignment is considered a definite assignment of the hidden backing field of the auto-property.</span></span> <span data-ttu-id="24c9d-233">Por lo tanto, se permite lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="24c9d-233">Thus, the following is allowed:</span></span>

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

### <a name="destructors"></a><span data-ttu-id="24c9d-234">Destructores</span><span class="sxs-lookup"><span data-stu-id="24c9d-234">Destructors</span></span>

<span data-ttu-id="24c9d-235">Un struct no se permite declarar un destructor.</span><span class="sxs-lookup"><span data-stu-id="24c9d-235">A struct is not permitted to declare a destructor.</span></span>

### <a name="static-constructors"></a><span data-ttu-id="24c9d-236">Constructores estáticos</span><span class="sxs-lookup"><span data-stu-id="24c9d-236">Static constructors</span></span>

<span data-ttu-id="24c9d-237">Los constructores estáticos para las estructuras siguen la mayoría de las mismas reglas que las clases.</span><span class="sxs-lookup"><span data-stu-id="24c9d-237">Static constructors for structs follow most of the same rules as for classes.</span></span> <span data-ttu-id="24c9d-238">El primero de los siguientes eventos que se producen dentro de un dominio de aplicación, se desencadena la ejecución de un constructor estático para un tipo struct:</span><span class="sxs-lookup"><span data-stu-id="24c9d-238">The execution of a static constructor for a struct type is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="24c9d-239">Se hace referencia a un miembro estático del tipo struct.</span><span class="sxs-lookup"><span data-stu-id="24c9d-239">A static member of the struct type is referenced.</span></span>
*  <span data-ttu-id="24c9d-240">Se llama a un constructor declarado explícitamente del tipo struct.</span><span class="sxs-lookup"><span data-stu-id="24c9d-240">An explicitly declared constructor of the struct type is called.</span></span>

<span data-ttu-id="24c9d-241">La creación de los valores predeterminados ([los valores predeterminados](structs.md#default-values)) tipos de struct no desencadena el constructor estático.</span><span class="sxs-lookup"><span data-stu-id="24c9d-241">The creation of default values ([Default values](structs.md#default-values)) of struct types does not trigger the static constructor.</span></span> <span data-ttu-id="24c9d-242">(Un ejemplo de esto es el valor inicial de elementos de matriz).</span><span class="sxs-lookup"><span data-stu-id="24c9d-242">(An example of this is the initial value of elements in an array.)</span></span>

## <a name="struct-examples"></a><span data-ttu-id="24c9d-243">Ejemplos de estructuras</span><span class="sxs-lookup"><span data-stu-id="24c9d-243">Struct examples</span></span>

<span data-ttu-id="24c9d-244">Lo siguiente muestra dos ejemplos importantes de usar `struct` tipos para crear tipos que pueden usarse de forma similar a los tipos predefinidos del lenguaje, pero con semántica modificada.</span><span class="sxs-lookup"><span data-stu-id="24c9d-244">The following shows two significant examples of using `struct` types to create types that can be used similarly to the predefined types of the language, but with modified semantics.</span></span>

### <a name="database-integer-type"></a><span data-ttu-id="24c9d-245">Tipo de entero de base de datos</span><span class="sxs-lookup"><span data-stu-id="24c9d-245">Database integer type</span></span>

<span data-ttu-id="24c9d-246">El `DBInt` estructura siguiente implementa un tipo entero que puede representar el conjunto completo de valores de la `int` tipo, además de un estado adicional que indica un valor desconocido.</span><span class="sxs-lookup"><span data-stu-id="24c9d-246">The `DBInt` struct below implements an integer type that can represent the complete set of values of the `int` type, plus an additional state that indicates an unknown value.</span></span> <span data-ttu-id="24c9d-247">Un tipo con estas características se usa habitualmente en las bases de datos.</span><span class="sxs-lookup"><span data-stu-id="24c9d-247">A type with these characteristics is commonly used in databases.</span></span>

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

### <a name="database-boolean-type"></a><span data-ttu-id="24c9d-248">Tipo booleano de la base de datos</span><span class="sxs-lookup"><span data-stu-id="24c9d-248">Database boolean type</span></span>

<span data-ttu-id="24c9d-249">El `DBBool` estructura siguiente implementa un tipo lógico de tres valores.</span><span class="sxs-lookup"><span data-stu-id="24c9d-249">The `DBBool` struct below implements a three-valued logical type.</span></span> <span data-ttu-id="24c9d-250">Los valores posibles de este tipo son `DBBool.True`, `DBBool.False`, y `DBBool.Null`, donde el `Null` miembro indica un valor desconocido.</span><span class="sxs-lookup"><span data-stu-id="24c9d-250">The possible values of this type are `DBBool.True`, `DBBool.False`, and `DBBool.Null`, where the `Null` member indicates an unknown value.</span></span> <span data-ttu-id="24c9d-251">Estos tipos lógicos de tres valores se usan habitualmente en las bases de datos.</span><span class="sxs-lookup"><span data-stu-id="24c9d-251">Such three-valued logical types are commonly used in databases.</span></span>

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
