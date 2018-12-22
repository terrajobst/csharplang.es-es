# <a name="delegates"></a><span data-ttu-id="e649e-101">Delegados</span><span class="sxs-lookup"><span data-stu-id="e649e-101">Delegates</span></span>

<span data-ttu-id="e649e-102">Los delegados permiten escenarios que otros lenguajes, como C++, Pascal y Modula--han resuelto con punteros de función.</span><span class="sxs-lookup"><span data-stu-id="e649e-102">Delegates enable scenarios that other languages—such as C++, Pascal, and Modula -- have addressed with function pointers.</span></span> <span data-ttu-id="e649e-103">A diferencia de los punteros de función de C++, sin embargo, los delegados son completamente orientada a objetos y, a diferencia de los punteros a funciones miembro de C++, los delegados encapsulan una instancia de objeto y un método.</span><span class="sxs-lookup"><span data-stu-id="e649e-103">Unlike C++ function pointers, however, delegates are fully object oriented, and unlike C++ pointers to member functions, delegates encapsulate both an object instance and a method.</span></span>

<span data-ttu-id="e649e-104">Una declaración de delegado define una clase que se deriva de la clase `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="e649e-104">A delegate declaration defines a class that is derived from the class `System.Delegate`.</span></span> <span data-ttu-id="e649e-105">Una instancia de delegado encapsula una lista de invocación, que es una lista de uno o varios métodos, cada uno de los cuales se conoce como una entidad que se puede llamar.</span><span class="sxs-lookup"><span data-stu-id="e649e-105">A delegate instance encapsulates an invocation list, which is a list of one or more methods, each of which is referred to as a callable entity.</span></span> <span data-ttu-id="e649e-106">Por ejemplo, métodos, una entidad que se puede llamar consta de una instancia y un método en esa instancia.</span><span class="sxs-lookup"><span data-stu-id="e649e-106">For instance methods, a callable entity consists of an instance and a method on that instance.</span></span> <span data-ttu-id="e649e-107">Para los métodos estáticos, una entidad que se puede llamar consta de un solo método.</span><span class="sxs-lookup"><span data-stu-id="e649e-107">For static methods, a callable entity consists of just a method.</span></span> <span data-ttu-id="e649e-108">Invoca una instancia de delegado con un conjunto adecuado de argumentos, cada una de las entidades en que se puede llamar que se invocan con el conjunto especificado de argumentos del delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-108">Invoking a delegate instance with an appropriate set of arguments causes each of the delegate's callable entities to be invoked with the given set of arguments.</span></span>

<span data-ttu-id="e649e-109">Una propiedad interesante y útil de una instancia del delegado es que no conoce ni interesan las clases de los métodos que encapsula; lo único que importa es que esos métodos sean compatibles ([declaraciones de delegado](delegates.md#delegate-declarations)) con el tipo del delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-109">An interesting and useful property of a delegate instance is that it does not know or care about the classes of the methods it encapsulates; all that matters is that those methods be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with the delegate's type.</span></span> <span data-ttu-id="e649e-110">Esto hace que los delegados sean perfectamente adecuados para la invocación "anonymous".</span><span class="sxs-lookup"><span data-stu-id="e649e-110">This makes delegates perfectly suited for "anonymous" invocation.</span></span>

## <a name="delegate-declarations"></a><span data-ttu-id="e649e-111">Declaraciones de delegados</span><span class="sxs-lookup"><span data-stu-id="e649e-111">Delegate declarations</span></span>

<span data-ttu-id="e649e-112">Un *delegate_declaration* es un *type_declaration* ([declaraciones de tipo](namespaces.md#type-declarations)) que declara un nuevo tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-112">A *delegate_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new delegate type.</span></span>

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

<span data-ttu-id="e649e-113">Es un error en tiempo de compilación para el mismo modificador aparezca varias veces en una declaración de delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-113">It is a compile-time error for the same modifier to appear multiple times in a delegate declaration.</span></span>

<span data-ttu-id="e649e-114">El `new` modificador solo se permite en los delegados se declaran dentro de otro tipo, en cuyo caso especifica dicho delegado oculta un miembro heredado con el mismo nombre, como se describe en [el nuevo modificador](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="e649e-114">The `new` modifier is only permitted on delegates declared within another type, in which case it specifies that such a delegate hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="e649e-115">El `public`, `protected`, `internal`, y `private` modificadores controlan la accesibilidad del tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-115">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the delegate type.</span></span> <span data-ttu-id="e649e-116">Dependiendo del contexto en el que se produce la declaración de delegado, algunos de estos modificadores pueden no estar permitidos ([accesibilidad declarada](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="e649e-116">Depending on the context in which the delegate declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="e649e-117">Es el nombre del tipo delegado *identificador*.</span><span class="sxs-lookup"><span data-stu-id="e649e-117">The delegate's type name is *identifier*.</span></span>

<span data-ttu-id="e649e-118">El elemento opcional *formal_parameter_list* especifica los parámetros del delegado, y *return_type* indica el tipo de valor devuelto del delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-118">The optional *formal_parameter_list* specifies the parameters of the delegate, and *return_type* indicates the return type of the delegate.</span></span>

<span data-ttu-id="e649e-119">El elemento opcional *variant_type_parameter_list* ([listas de parámetros de tipo variante](interfaces.md#variant-type-parameter-lists)) especifica los parámetros de tipo para el delegado en Sí.</span><span class="sxs-lookup"><span data-stu-id="e649e-119">The optional *variant_type_parameter_list* ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)) specifies the type parameters to the delegate itself.</span></span>

<span data-ttu-id="e649e-120">El tipo de valor devuelto de un tipo de delegado debe ser `void`, o con seguridad de salida ([seguridad varianza](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="e649e-120">The return type of a delegate type must be either `void`, or output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span>

<span data-ttu-id="e649e-121">Todos los tipos de parámetros formales de un tipo de delegado deben ser con seguridad de entrada.</span><span class="sxs-lookup"><span data-stu-id="e649e-121">All the formal parameter types of a delegate type must be input-safe.</span></span> <span data-ttu-id="e649e-122">Asimismo, cualquier `out` o `ref` tipos de parámetro también deben ser seguro para salida.</span><span class="sxs-lookup"><span data-stu-id="e649e-122">Additionally, any `out` or `ref` parameter types must also be output-safe.</span></span> <span data-ttu-id="e649e-123">Tenga en cuenta que, incluso `out` parámetros son necesarios para tener seguridad de entrada, debido a una limitación de la plataforma subyacente de la ejecución.</span><span class="sxs-lookup"><span data-stu-id="e649e-123">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="e649e-124">Tipos de delegados en C# son el nombre equivalente, no es estructuralmente equivalente.</span><span class="sxs-lookup"><span data-stu-id="e649e-124">Delegate types in C# are name equivalent, not structurally equivalent.</span></span> <span data-ttu-id="e649e-125">En concreto, dos tipos de delegado distintos que tienen el mismo parámetro se enumeran y devolver el tipo se consideran tipos de delegado distintos.</span><span class="sxs-lookup"><span data-stu-id="e649e-125">Specifically, two different delegate types that have the same parameter lists and return type are considered different delegate types.</span></span> <span data-ttu-id="e649e-126">Sin embargo, pueden comparar las instancias de dos tipos de delegado distintos pero equivalentes estructuralmente igual ([delegar los operadores de igualdad](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="e649e-126">However, instances of two distinct but structurally equivalent delegate types may compare as equal ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="e649e-127">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e649e-127">For example:</span></span>

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

<span data-ttu-id="e649e-128">Los métodos `A.M1` y `B.M1 `son compatibles con ambos tipos de delegado `D1` y `D2` , puesto que tienen el mismo tipo y lista de parámetros de devolver; sin embargo, estos tipos de delegado son dos tipos diferentes, por lo que no son intercambiables.</span><span class="sxs-lookup"><span data-stu-id="e649e-128">The methods `A.M1` and `B.M1 `are compatible with both the delegate types `D1` and `D2` , since they have the same return type and parameter list; however, these delegate types are two different types, so they are not interchangeable.</span></span> <span data-ttu-id="e649e-129">Los métodos `B.M2`, `B.M3`, y `B.M4` no son compatibles con los tipos de delegado `D1` y `D2`, puesto que tienen diferentes tipos de valor devueltos o listas de parámetros.</span><span class="sxs-lookup"><span data-stu-id="e649e-129">The methods `B.M2`, `B.M3`, and `B.M4` are incompatible with the delegate types `D1` and `D2`, since they have different return types or parameter lists.</span></span>

<span data-ttu-id="e649e-130">Al igual que otras declaraciones de tipo genérico, se deben proporcionar argumentos de tipo para crear un tipo de delegado construido.</span><span class="sxs-lookup"><span data-stu-id="e649e-130">Like other generic type declarations, type arguments must be given to create a constructed delegate type.</span></span> <span data-ttu-id="e649e-131">Los tipos de parámetro y el tipo de valor devuelto de un tipo de delegado construido se crean sustituyendo, para cada parámetro de tipo en la declaración de delegado, el argumento de tipo correspondiente del tipo de delegado construido.</span><span class="sxs-lookup"><span data-stu-id="e649e-131">The parameter types and return type of a constructed delegate type are created by substituting, for each type parameter in the delegate declaration, the corresponding type argument of the constructed delegate type.</span></span> <span data-ttu-id="e649e-132">El tipo de valor devuelto resultante y los tipos de parámetro se usan para determinar qué métodos son compatibles con un tipo de delegado construido.</span><span class="sxs-lookup"><span data-stu-id="e649e-132">The resulting return type and parameter types are used in determining what methods are compatible with a constructed delegate type.</span></span> <span data-ttu-id="e649e-133">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e649e-133">For example:</span></span>

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

<span data-ttu-id="e649e-134">El método `X.F` es compatible con el tipo de delegado `Predicate<int>` y el método `X.G` es compatible con el tipo de delegado `Predicate<string>` .</span><span class="sxs-lookup"><span data-stu-id="e649e-134">The method `X.F` is compatible with the delegate type `Predicate<int>` and the method `X.G` is compatible with the delegate type `Predicate<string>` .</span></span>

<span data-ttu-id="e649e-135">La única manera de declarar un tipo delegado es a través de un *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="e649e-135">The only way to declare a delegate type is via a *delegate_declaration*.</span></span> <span data-ttu-id="e649e-136">Un tipo de delegado es un tipo de clase que se deriva de `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="e649e-136">A delegate type is a class type that is derived from `System.Delegate`.</span></span> <span data-ttu-id="e649e-137">Tipos de delegados son implícitamente `sealed`, por lo que no permite derivar cualquier tipo de un tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-137">Delegate types are implicitly `sealed`, so it is not permissible to derive any type from a delegate type.</span></span> <span data-ttu-id="e649e-138">También no es permitida para derivar un tipo de clase no delegado de `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="e649e-138">It is also not permissible to derive a non-delegate class type from `System.Delegate`.</span></span> <span data-ttu-id="e649e-139">Tenga en cuenta que `System.Delegate` es no escriba un delegado; es un tipo de clase que se derivan todos los tipos de delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-139">Note that `System.Delegate` is not itself a delegate type; it is a class type from which all delegate types are derived.</span></span>

<span data-ttu-id="e649e-140">C# proporciona una sintaxis especial de delegado y llamada de instancias.</span><span class="sxs-lookup"><span data-stu-id="e649e-140">C# provides special syntax for delegate instantiation and invocation.</span></span> <span data-ttu-id="e649e-141">Excepto para la creación de instancias, cualquier operación que se puede aplicar a una clase o instancia de la clase también puede aplicarse a una clase de delegado o una instancia, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="e649e-141">Except for instantiation, any operation that can be applied to a class or class instance can also be applied to a delegate class or instance, respectively.</span></span> <span data-ttu-id="e649e-142">En concreto, es posible acceder a los miembros de la `System.Delegate` tipo a través de la sintaxis de acceso de miembro habitual.</span><span class="sxs-lookup"><span data-stu-id="e649e-142">In particular, it is possible to access members of the `System.Delegate` type via the usual member access syntax.</span></span>

<span data-ttu-id="e649e-143">El conjunto de métodos encapsulados por una instancia de delegado se denomina una lista de invocación.</span><span class="sxs-lookup"><span data-stu-id="e649e-143">The set of methods encapsulated by a delegate instance is called an invocation list.</span></span> <span data-ttu-id="e649e-144">Cuando se crea una instancia de delegado ([compatibilidad de delegado](delegates.md#delegate-compatibility)) desde un único método, encapsula ese método y su lista de invocación contiene sólo una entrada.</span><span class="sxs-lookup"><span data-stu-id="e649e-144">When a delegate instance is created ([Delegate compatibility](delegates.md#delegate-compatibility)) from a single method, it encapsulates that method, and its invocation list contains only one entry.</span></span> <span data-ttu-id="e649e-145">Sin embargo, cuando se combinan dos instancias de delegado no null, se concatenan sus listas de invocación--en el orden en el operando izquierdo operando derecho--para formar una nueva lista de invocación, que contiene dos o más entradas.</span><span class="sxs-lookup"><span data-stu-id="e649e-145">However, when two non-null delegate instances are combined, their invocation lists are concatenated -- in the order left operand then right operand -- to form a new invocation list, which contains two or more entries.</span></span>

<span data-ttu-id="e649e-146">Los delegados se combinan mediante el archivo binario `+` ([operador de suma](expressions.md#addition-operator)) y `+=` operadores ([asignación compuesta](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="e649e-146">Delegates are combined using the binary `+` ([Addition operator](expressions.md#addition-operator)) and `+=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="e649e-147">Se puede quitar un delegado de una combinación de delegados utilizando el archivo binario `-` ([operador de resta](expressions.md#subtraction-operator)) y `-=` operadores ([asignación compuesta](expressions.md#compound-assignment)).</span><span class="sxs-lookup"><span data-stu-id="e649e-147">A delegate can be removed from a combination of delegates, using the binary `-` ([Subtraction operator](expressions.md#subtraction-operator)) and `-=` operators ([Compound assignment](expressions.md#compound-assignment)).</span></span> <span data-ttu-id="e649e-148">Los delegados se pueden comparar la igualdad ([delegar los operadores de igualdad](expressions.md#delegate-equality-operators)).</span><span class="sxs-lookup"><span data-stu-id="e649e-148">Delegates can be compared for equality ([Delegate equality operators](expressions.md#delegate-equality-operators)).</span></span>

<span data-ttu-id="e649e-149">En el ejemplo siguiente se muestra la creación de instancias de un número de delegados y sus correspondientes listas de llamadas:</span><span class="sxs-lookup"><span data-stu-id="e649e-149">The following example shows the instantiation of a number of delegates, and their corresponding invocation lists:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

<span data-ttu-id="e649e-150">Cuando `cd1` y `cd2` son instancias, cada uno de ellos encapsular un método.</span><span class="sxs-lookup"><span data-stu-id="e649e-150">When `cd1` and `cd2` are instantiated, they each encapsulate one method.</span></span> <span data-ttu-id="e649e-151">Cuando `cd3` es creación de instancias, tiene una lista de invocaciones de dos métodos, `M1` y `M2`, en ese orden.</span><span class="sxs-lookup"><span data-stu-id="e649e-151">When `cd3` is instantiated, it has an invocation list of two methods, `M1` and `M2`, in that order.</span></span> <span data-ttu-id="e649e-152">`cd4`de contiene la lista de invocaciones `M1`, `M2`, y `M1`, en ese orden.</span><span class="sxs-lookup"><span data-stu-id="e649e-152">`cd4`'s invocation list contains `M1`, `M2`, and `M1`, in that order.</span></span> <span data-ttu-id="e649e-153">Por último, `cd5`de contiene la lista de invocaciones `M1`, `M2`, `M1`, `M1`, y `M2`, en ese orden.</span><span class="sxs-lookup"><span data-stu-id="e649e-153">Finally, `cd5`'s invocation list contains `M1`, `M2`, `M1`, `M1`, and `M2`, in that order.</span></span> <span data-ttu-id="e649e-154">Para obtener más ejemplos de cómo combinar delegados (así como eliminar), consulte [invocación de delegado](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="e649e-154">For more examples of combining (as well as removing) delegates, see [Delegate invocation](delegates.md#delegate-invocation).</span></span>

## <a name="delegate-compatibility"></a><span data-ttu-id="e649e-155">Compatibilidad de delegado</span><span class="sxs-lookup"><span data-stu-id="e649e-155">Delegate compatibility</span></span>

<span data-ttu-id="e649e-156">Un método o delegado `M` es ***compatible*** con un tipo de delegado `D` si se cumplen todas las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="e649e-156">A method or delegate `M` is ***compatible*** with a delegate type `D` if all of the following are true:</span></span>

*  <span data-ttu-id="e649e-157">`D` y `M` tienen el mismo número de parámetros y cada parámetro de `D` tiene el mismo `ref` o `out` modificadores como el parámetro correspondiente en `M`.</span><span class="sxs-lookup"><span data-stu-id="e649e-157">`D` and `M` have the same number of parameters, and each parameter in `D` has the same `ref` or `out` modifiers as the corresponding parameter in `M`.</span></span>
*  <span data-ttu-id="e649e-158">Para cada parámetro de valor (un parámetro no `ref` o `out` modificador), una conversión de identidad ([conversión de identidad](conversions.md#identity-conversion)) o una conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) existe el tipo de parámetro en `D` al tipo de parámetro correspondiente en `M`.</span><span class="sxs-lookup"><span data-stu-id="e649e-158">For each value parameter (a parameter with no `ref` or `out` modifier), an identity conversion ([Identity conversion](conversions.md#identity-conversion)) or implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the parameter type in `D` to the corresponding parameter type in `M`.</span></span>
*  <span data-ttu-id="e649e-159">Para cada `ref` o `out` parámetro, escriba el parámetro `D` es el mismo que el tipo de parámetro en `M`.</span><span class="sxs-lookup"><span data-stu-id="e649e-159">For each `ref` or `out` parameter, the parameter type in `D` is the same as the parameter type in `M`.</span></span>
*  <span data-ttu-id="e649e-160">No existe una identidad o una conversión implícita de referencia de tipo de valor devuelto de `M` para el tipo de valor devuelto de `D`.</span><span class="sxs-lookup"><span data-stu-id="e649e-160">An identity or implicit reference conversion exists from the return type of `M` to the return type of `D`.</span></span>

## <a name="delegate-instantiation"></a><span data-ttu-id="e649e-161">Creación de instancias de delegado</span><span class="sxs-lookup"><span data-stu-id="e649e-161">Delegate instantiation</span></span>

<span data-ttu-id="e649e-162">Se crea una instancia de un delegado mediante un *delegate_creation_expression* ([expresiones de creación de delegados](expressions.md#delegate-creation-expressions)) o una conversión a un tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-162">An instance of a delegate is created by a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) or a conversion to a delegate type.</span></span> <span data-ttu-id="e649e-163">La instancia del delegado recién creado, a continuación, hace referencia a cualquiera:</span><span class="sxs-lookup"><span data-stu-id="e649e-163">The newly created delegate instance then refers to either:</span></span>

*  <span data-ttu-id="e649e-164">El método estático al que hace referencia en el *delegate_creation_expression*, o</span><span class="sxs-lookup"><span data-stu-id="e649e-164">The static method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="e649e-165">El objeto de destino (que no puede ser `null`) y la instancia de método al que hace referencia en el *delegate_creation_expression*, o</span><span class="sxs-lookup"><span data-stu-id="e649e-165">The target object (which cannot be `null`) and instance method referenced in the *delegate_creation_expression*, or</span></span>
*  <span data-ttu-id="e649e-166">Otro delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-166">Another delegate.</span></span>

<span data-ttu-id="e649e-167">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e649e-167">For example:</span></span>

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

<span data-ttu-id="e649e-168">Una vez que crea una instancia, instancias de delegado consulte siempre el mismo objeto de destino y método.</span><span class="sxs-lookup"><span data-stu-id="e649e-168">Once instantiated, delegate instances always refer to the same target object and method.</span></span> <span data-ttu-id="e649e-169">Recuerde que, cuando se combinan dos delegados o se quita uno de otro, un nuevo resultados delegado con su propia lista de invocaciones; las listas de invocaciones de los delegados combinados o eliminados permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="e649e-169">Remember, when two delegates are combined, or one is removed from another, a new delegate results with its own invocation list; the invocation lists of the delegates combined or removed remain unchanged.</span></span>

## <a name="delegate-invocation"></a><span data-ttu-id="e649e-170">Invocación de delegado</span><span class="sxs-lookup"><span data-stu-id="e649e-170">Delegate invocation</span></span>

<span data-ttu-id="e649e-171">C# proporciona una sintaxis especial para invocar a un delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-171">C# provides special syntax for invoking a delegate.</span></span> <span data-ttu-id="e649e-172">Cuando se invoca una instancia de delegado no null cuya lista de invocaciones contiene una entrada, invoca el método con los mismos argumentos que se ha proporcionado y devuelve el mismo valor que el que se hace referencia al método.</span><span class="sxs-lookup"><span data-stu-id="e649e-172">When a non-null delegate instance whose invocation list contains one entry is invoked, it invokes the one method with the same arguments it was given, and returns the same value as the referred to method.</span></span> <span data-ttu-id="e649e-173">(Consulte [invocaciones de delegado](expressions.md#delegate-invocations) para obtener información detallada sobre la invocación de delegado.) Si se produce una excepción durante la invocación de un delegado y no se detecta esa excepción dentro del método invocado, la búsqueda de una cláusula de excepción catch continúa en el método que llama al delegado, como si ese método hubiera llamado directamente el método al que delegar la que hace referencia.</span><span class="sxs-lookup"><span data-stu-id="e649e-173">(See [Delegate invocations](expressions.md#delegate-invocations) for detailed information on delegate invocation.) If an exception occurs during the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, as if that method had directly called the method to which that delegate referred.</span></span>

<span data-ttu-id="e649e-174">La invocación de una instancia de delegado cuya lista de invocaciones contiene varias entradas continúa llamando a cada uno de los métodos en la lista de invocaciones, de forma sincrónica, en orden.</span><span class="sxs-lookup"><span data-stu-id="e649e-174">Invocation of a delegate instance whose invocation list contains multiple entries proceeds by invoking each of the methods in the invocation list, synchronously, in order.</span></span> <span data-ttu-id="e649e-175">Cada método llamado por lo que se pasa el mismo conjunto de argumentos que se asignó a la instancia del delegado.</span><span class="sxs-lookup"><span data-stu-id="e649e-175">Each method so called is passed the same set of arguments as was given to the delegate instance.</span></span> <span data-ttu-id="e649e-176">Si la invocación de delegado incluye parámetros de referencia ([hacen referencia a parámetros](classes.md#reference-parameters)), se producirá cada invocación de método con una referencia a la misma variable; serán los cambios en esa variable mediante un método en la lista de invocación visible para los métodos más abajo en la lista de invocación.</span><span class="sxs-lookup"><span data-stu-id="e649e-176">If such a delegate invocation includes reference parameters ([Reference parameters](classes.md#reference-parameters)), each method invocation will occur with a reference to the same variable; changes to that variable by one method in the invocation list will be visible to methods further down the invocation list.</span></span> <span data-ttu-id="e649e-177">Si la invocación del delegado incluye parámetros de salida o valor devuelto, su valor final procederán de la invocación del último delegado en la lista.</span><span class="sxs-lookup"><span data-stu-id="e649e-177">If the delegate invocation includes output parameters or a return value, their final value will come from the invocation of the last delegate in the list.</span></span>

<span data-ttu-id="e649e-178">Si se produce una excepción durante el procesamiento de la invocación de un delegado y no se detecta esa excepción dentro del método invocado, la búsqueda de una cláusula de excepción catch continúa en el método que llama al delegado, y todos los métodos más abajo no se invoca la lista de invocación.</span><span class="sxs-lookup"><span data-stu-id="e649e-178">If an exception occurs during processing of the invocation of such a delegate, and that exception is not caught within the method that was invoked, the search for an exception catch clause continues in the method that called the delegate, and any methods further down the invocation list are not invoked.</span></span>

<span data-ttu-id="e649e-179">Intento de invocar una instancia de delegado cuyo valor es null provoca una excepción de tipo `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="e649e-179">Attempting to invoke a delegate instance whose value is null results in an exception of type `System.NullReferenceException`.</span></span>

<span data-ttu-id="e649e-180">El ejemplo siguiente muestra cómo crear una instancia, combinar, quitar e invocar a delegados:</span><span class="sxs-lookup"><span data-stu-id="e649e-180">The following example shows how to instantiate, combine, remove, and invoke delegates:</span></span>

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

<span data-ttu-id="e649e-181">Como se muestra en la instrucción `cd3 += cd1;`, un delegado puede estar presente en una lista de invocaciones varias veces.</span><span class="sxs-lookup"><span data-stu-id="e649e-181">As shown in the statement `cd3 += cd1;`, a delegate can be present in an invocation list multiple times.</span></span> <span data-ttu-id="e649e-182">En este caso, se le llama una vez por cada instancia.</span><span class="sxs-lookup"><span data-stu-id="e649e-182">In this case, it is simply invoked once per occurrence.</span></span> <span data-ttu-id="e649e-183">En la lista de invocación como esta, cuando se quita ese delegado, la última aparición en la lista de invocación es lo que se quita en realidad.</span><span class="sxs-lookup"><span data-stu-id="e649e-183">In an invocation list such as this, when that delegate is removed, the last occurrence in the invocation list is the one actually removed.</span></span>

<span data-ttu-id="e649e-184">Inmediatamente antes de la ejecución de la instrucción final, `cd3 -= cd1;`, el delegado `cd3` hace referencia a una lista de invocaciones vacío.</span><span class="sxs-lookup"><span data-stu-id="e649e-184">Immediately prior to the execution of the final statement, `cd3 -= cd1;`, the delegate `cd3` refers to an empty invocation list.</span></span> <span data-ttu-id="e649e-185">No es un error al intentar quitar un delegado de una lista vacía (o para quitar a un delegado que no existe en una lista de valores no vacíos).</span><span class="sxs-lookup"><span data-stu-id="e649e-185">Attempting to remove a delegate from an empty list (or to remove a non-existent delegate from a non-empty list) is not an error.</span></span>

<span data-ttu-id="e649e-186">El resultado es:</span><span class="sxs-lookup"><span data-stu-id="e649e-186">The output produced is:</span></span>

```
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
