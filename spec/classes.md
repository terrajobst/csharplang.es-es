---
ms.openlocfilehash: af7af574814dc04ee3ece0396b7ae5f86b3ec8eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488898"
---
# <a name="classes"></a><span data-ttu-id="98cd8-101">Clases</span><span class="sxs-lookup"><span data-stu-id="98cd8-101">Classes</span></span>

<span data-ttu-id="98cd8-102">Una clase es una estructura de datos que puede contener miembros de datos (constantes y campos), miembros de función (métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos) y tipos anidados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="98cd8-103">Tipos de clase admiten la herencia, un mecanismo mediante el cual una clase derivada puede extender y especializar una clase base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="98cd8-104">Declaraciones de clase</span><span class="sxs-lookup"><span data-stu-id="98cd8-104">Class declarations</span></span>

<span data-ttu-id="98cd8-105">Un *class_declaration* es un *type_declaration* ([declaraciones de tipo](namespaces.md#type-declarations)) que declara una clase nueva.</span><span class="sxs-lookup"><span data-stu-id="98cd8-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="98cd8-106">Un *class_declaration* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *class_modifier*s ([modificadores de clase](classes.md#class-modifiers)), seguido de un elemento opcional `partial` modificador, seguido por la palabra clave `class` y un *identificador* que los nombres de la clase, seguida por un opcional *type_parameter_list* ([parámetros de tipo](classes.md#type-parameters)), seguido de un elemento opcional *class_base* especificación ([base clase especificación de](classes.md#class-base-specification)), seguido de un conjunto opcional de *type_parameter_constraints_clause*s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), seguido de un *class_body*  ([Cuerpo de la clase](classes.md#class-body)), seguido opcionalmente de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="98cd8-107">No se puede proporcionar una declaración de clase *type_parameter_constraints_clause*s a menos que también proporciona un *type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="98cd8-108">Una declaración de clase que proporciona un *type_parameter_list* es un ***declaración de clase genérica***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="98cd8-109">Además, cualquier clase anidada dentro de una declaración de clase genérica o una declaración de struct genérico es una declaración de clase genérica, puesto que se deben proporcionar parámetros de tipo para el tipo de contenedor para crear un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="98cd8-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="98cd8-110">Modificadores de clase</span><span class="sxs-lookup"><span data-stu-id="98cd8-110">Class modifiers</span></span>

<span data-ttu-id="98cd8-111">Un *class_declaration* puede incluir opcionalmente una secuencia de modificadores de clase:</span><span class="sxs-lookup"><span data-stu-id="98cd8-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

<span data-ttu-id="98cd8-112">Es un error en tiempo de compilación para el mismo modificador aparezca varias veces en una declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="98cd8-113">El `new` modificador está permitido en clases anidadas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="98cd8-114">Especifica que la clase oculta un miembro heredado con el mismo nombre, como se describe en [el nuevo modificador](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="98cd8-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="98cd8-115">Es un error en tiempo de compilación para el `new` modificador aparezca en una declaración de clase que no es una declaración de clase anidada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="98cd8-116">El `public`, `protected`, `internal`, y `private` modificadores controlan el acceso de la clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="98cd8-117">Dependiendo del contexto en el que se produce la declaración de clase, algunos de estos modificadores pueden no estar permitidos ([accesibilidad declarada](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="98cd8-118">El `abstract`, `sealed` y `static` modificadores se tratan en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="98cd8-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="98cd8-119">Clases abstractas</span><span class="sxs-lookup"><span data-stu-id="98cd8-119">Abstract classes</span></span>

<span data-ttu-id="98cd8-120">El `abstract` modificador se usa para indicar que una clase está incompleta y que está pensada para usarse solo como una clase base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="98cd8-121">Una clase abstracta difiere de una clase no abstracta de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="98cd8-122">Una clase abstracta no se pueden crear instancias directamente, y es un error en tiempo de compilación para usar el `new` operador en una clase abstracta.</span><span class="sxs-lookup"><span data-stu-id="98cd8-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="98cd8-123">Aunque es posible tener variables y valores cuyos tipos en tiempo de compilación son abstractos, tales variables y valores necesariamente será `null` o contendrán referencias a instancias de clases no abstractas derivadas de los tipos abstractos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="98cd8-124">Una clase abstracta se permiten (aunque no es necesario) para contener miembros abstractos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="98cd8-125">Una clase abstracta no puede estar sellada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="98cd8-126">Cuando una clase no abstracta se deriva de una clase abstracta, la clase no abstracta debe incluir implementaciones reales de todos los miembros abstractos heredados, anulando a esos miembros abstractos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="98cd8-127">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-127">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
<span data-ttu-id="98cd8-128">la clase abstracta `A` presenta un método abstracto `F`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="98cd8-129">Clase `B` presenta un método adicional `G`, pero proporciona una implementación de `F`, `B` debe declararse como abstracta.</span><span class="sxs-lookup"><span data-stu-id="98cd8-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="98cd8-130">Clase `C` invalida `F` y proporciona una implementación real.</span><span class="sxs-lookup"><span data-stu-id="98cd8-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="98cd8-131">Puesto que no hay ningún miembro abstracto en `C`, `C` se permitía (aunque no es necesario) para ser no abstracta.</span><span class="sxs-lookup"><span data-stu-id="98cd8-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="98cd8-132">Clases selladas</span><span class="sxs-lookup"><span data-stu-id="98cd8-132">Sealed classes</span></span>

<span data-ttu-id="98cd8-133">El `sealed` modificador se usa para impedir la derivación de una clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="98cd8-134">Si se especifica una clase sellada como clase base de otra clase, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="98cd8-135">Una clase sellada no puede ser también una clase abstracta.</span><span class="sxs-lookup"><span data-stu-id="98cd8-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="98cd8-136">El `sealed` modificador se usa principalmente para evitar una derivación imprevista, pero también permite ciertas optimizaciones en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="98cd8-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="98cd8-137">En concreto, dado que una clase sellada no puede tener clases derivadas, es posible transformar las invocaciones de miembros de función virtual en instancias de una clase sellada en llamadas no virtuales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="98cd8-138">Clases estáticas</span><span class="sxs-lookup"><span data-stu-id="98cd8-138">Static classes</span></span>

<span data-ttu-id="98cd8-139">El `static` modificador se usa para marcar la clase que se declara como un ***clase estática***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="98cd8-140">Una clase estática no pueden crearse instancias, no se puede usar como un tipo y puede contener a solo miembros estáticos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="98cd8-141">Sólo una clase estática puede contener declaraciones de métodos de extensión ([métodos de extensión](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="98cd8-142">Una declaración de clase estática está sujeto a las restricciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="98cd8-143">Una clase estática no puede incluir un `sealed` o `abstract` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="98cd8-144">Sin embargo, tenga en cuenta que puesto que no se crea una instancia o derivada de una clase estática, se comporta como si fuese sellada y abstracta.</span><span class="sxs-lookup"><span data-stu-id="98cd8-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="98cd8-145">Una clase estática no puede incluir un *class_base* especificación ([clase Especificación base](classes.md#class-base-specification)) y no se puede especificar explícitamente una clase base o una lista de interfaces implementadas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="98cd8-146">Una clase estática hereda implícitamente de tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="98cd8-147">Una clase estática solo puede contener miembros estáticos ([miembros estáticos y de instancia](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="98cd8-148">Tenga en cuenta que las constantes y tipos anidados se clasifican como miembros estáticos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="98cd8-149">Una clase estática no puede tener miembros con `protected` o `protected internal` accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="98cd8-150">Es un error en tiempo de compilación que infringe cualquiera de estas restricciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="98cd8-151">Una clase estática no tiene ningún constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-151">A static class has no instance constructors.</span></span> <span data-ttu-id="98cd8-152">No es posible declarar un constructor de instancia en una clase estática y ningún constructor de instancia predeterminado ([constructores predeterminados](classes.md#default-constructors)) se proporciona para una clase estática.</span><span class="sxs-lookup"><span data-stu-id="98cd8-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="98cd8-153">Los miembros de una clase estática no son automáticamente estáticos y las declaraciones de miembro deben incluir explícitamente una `static` modificador (excepto las constantes y tipos anidados).</span><span class="sxs-lookup"><span data-stu-id="98cd8-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="98cd8-154">Cuando una clase se anida dentro de una clase estática externa, la clase anidada no es una clase estática a menos que incluya explícitamente un `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="98cd8-155">__Hacer referencia a tipos de clase estática__</span><span class="sxs-lookup"><span data-stu-id="98cd8-155">__Referencing static class types__</span></span>

<span data-ttu-id="98cd8-156">Un *namespace_or_type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)) tiene permiso para hacer referencia a una clase estática si</span><span class="sxs-lookup"><span data-stu-id="98cd8-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="98cd8-157">El *namespace_or_type_name* es el `T` en un *namespace_or_type_name* del formulario `T.I`, o</span><span class="sxs-lookup"><span data-stu-id="98cd8-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="98cd8-158">El *namespace_or_type_name* es el `T` en un *typeof_expression* ([listas de argumentos](expressions.md#argument-lists)1) del formulario `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="98cd8-159">Un *primary_expression* ([miembros de función](expressions.md#function-members)) tiene permiso para hacer referencia a una clase estática si</span><span class="sxs-lookup"><span data-stu-id="98cd8-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="98cd8-160">El *primary_expression* es el `E` en un *member_access* ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) del formulario `E.I`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="98cd8-161">En cualquier otro contexto es un error en tiempo de compilación para hacer referencia a una clase estática.</span><span class="sxs-lookup"><span data-stu-id="98cd8-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="98cd8-162">Por ejemplo, es un error para una clase estática que se usará como una clase base, un tipo constituyente ([tipos anidados](classes.md#nested-types)) de un miembro, un argumento de tipo genérico o una restricción de parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="98cd8-163">Del mismo modo, no se puede usar una clase estática en un tipo de matriz, un tipo de puntero, un `new` expresión, una expresión de conversión, un `is` expresión, un `as` expresión, un `sizeof` expresión o una expresión de valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="98cd8-164">Modificador parcial</span><span class="sxs-lookup"><span data-stu-id="98cd8-164">Partial modifier</span></span>

<span data-ttu-id="98cd8-165">El `partial` modificador se usa para indicar que este *class_declaration* es una declaración de tipo parcial.</span><span class="sxs-lookup"><span data-stu-id="98cd8-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="98cd8-166">Combinan varias declaraciones parciales de tipos con el mismo nombre dentro de una declaración de espacio de nombres o tipo envolvente a la declaración de un tipo de formulario, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="98cd8-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="98cd8-167">Tener la declaración de una clase que se distribuye en segmentos independientes del texto del programa puede ser útil si son producidos o mantenerse en contextos diferentes estos segmentos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="98cd8-168">Por ejemplo, una parte de una declaración de clase puede ser generan automáticamente, mientras que la otra se crea manualmente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="98cd8-169">Separación textual de los dos impide las actualizaciones en uno respecto a entrar en conflicto con las actualizaciones de otros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="98cd8-170">Parámetros de tipo</span><span class="sxs-lookup"><span data-stu-id="98cd8-170">Type parameters</span></span>

<span data-ttu-id="98cd8-171">Un parámetro de tipo es un identificador simple que denota un marcador de posición para un argumento de tipo proporcionado para crear un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="98cd8-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="98cd8-172">Un parámetro de tipo es un marcador de posición formal para un tipo que se proporcionarán más adelante.</span><span class="sxs-lookup"><span data-stu-id="98cd8-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="98cd8-173">Por el contrario, un argumento de tipo ([argumentos de tipo](types.md#type-arguments)) es el tipo real que se sustituye por el parámetro de tipo cuando se crea un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="98cd8-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

<span data-ttu-id="98cd8-174">Cada parámetro de tipo en una declaración de clase define un nombre en el espacio de declaración ([declaraciones](basic-concepts.md#declarations)) de esa clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="98cd8-175">Por lo tanto, no puede tener el mismo nombre que otro parámetro de tipo o un miembro declarado en esa clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="98cd8-176">Un parámetro de tipo no puede tener el mismo nombre que el propio tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="98cd8-177">Especificación de clase base</span><span class="sxs-lookup"><span data-stu-id="98cd8-177">Class base specification</span></span>

<span data-ttu-id="98cd8-178">Una declaración de clase puede incluir un *class_base* especificación, que define la clase base directa de la clase y las interfaces ([Interfaces](interfaces.md)) directamente implementada por la clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

<span data-ttu-id="98cd8-179">La clase base especificada en una declaración de clase puede ser un tipo de clase construida ([construido tipos](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="98cd8-180">Una clase base no puede ser un parámetro de tipo por sí mismo, aunque puede implicar a los parámetros de tipo que están en ámbito.</span><span class="sxs-lookup"><span data-stu-id="98cd8-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="98cd8-181">Clases base</span><span class="sxs-lookup"><span data-stu-id="98cd8-181">Base classes</span></span>

<span data-ttu-id="98cd8-182">Cuando un *class_type* se incluye en el *class_base*, especifica la clase base directa de la clase que se declara.</span><span class="sxs-lookup"><span data-stu-id="98cd8-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="98cd8-183">Si una declaración de clase no tiene ningún *class_base*, o si el *class_base* listas sólo los tipos de interfaz, se supone que la clase base directa se `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="98cd8-184">Una clase hereda los miembros de su clase base directa, como se describe en [herencia](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="98cd8-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="98cd8-185">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="98cd8-186">clase `A` se dice que la clase base directa de `B`, y `B` se dice que se derive de `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="98cd8-187">Puesto que `A` does explícitamente no especificar una clase base directa, su clase base directa es implícitamente `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="98cd8-188">Para un tipo de clase construida, si una clase base se especifica en la declaración de clase genérica, se obtiene la clase base del tipo construido sustituyendo, para cada *tipo_parámetro* en la declaración de clase base correspondiente *type_argument* del tipo construido.</span><span class="sxs-lookup"><span data-stu-id="98cd8-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="98cd8-189">Dadas las declaraciones de clase genérica</span><span class="sxs-lookup"><span data-stu-id="98cd8-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="98cd8-190">la clase base del tipo construido `G<int>` sería `B<string,int[]>`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="98cd8-191">La clase base directa de un tipo de clase debe ser al menos tan accesible como el propio tipo de clase ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="98cd8-192">Por ejemplo, es un error en tiempo de compilación para un `public` clase derive de una `private` o `internal` clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="98cd8-193">La clase base directa de un tipo de clase no debe ser cualquiera de los siguientes tipos: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, o `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="98cd8-194">Además, no se puede usar una declaración de clase genérica `System.Attribute` como una clase base directa o indirecta.</span><span class="sxs-lookup"><span data-stu-id="98cd8-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="98cd8-195">Al determinar el significado de la especificación de la clase base directa `A` de una clase `B`, la clase base directa de `B` temporalmente se supone que es `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="98cd8-196">Intuitivamente Esto garantiza que el significado de una especificación de clase base no pueden hacerlo de forma recursiva depender de sí misma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="98cd8-197">El ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="98cd8-198">es un error desde en la especificación de la clase base `A<C.B>` la clase base directa de `C` se considera `object`y por lo tanto, (según las reglas del [Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)) `C` no se considera tener un miembro `B`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="98cd8-199">Las clases base de un tipo de clase son la clase base directa y sus clases base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="98cd8-200">En otras palabras, el conjunto de clases bases es el cierre transitivo de la relación de clase base directa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="98cd8-201">Que hace referencia en el ejemplo anterior, las clases base de `B` son `A` y `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="98cd8-202">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="98cd8-203">las clases base de `D<int>` son `C<int[]>`, `B<IComparable<int[]>>`, `A`, y `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="98cd8-204">Excepto para la clase `object`, cada tipo de clase tiene exactamente una clase base directa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="98cd8-205">La `object` clase no tiene ninguna clase base directa y es la clase base fundamental de todas las demás clases.</span><span class="sxs-lookup"><span data-stu-id="98cd8-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="98cd8-206">Cuando una clase `B` deriva una clase `A`, es un error de tiempo de compilación de `A` depender `B`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="98cd8-207">Una clase ***depende directamente de*** su clase base directa (si existe) y ***depende directamente de*** la clase en el que está inmediatamente anidada (si existe).</span><span class="sxs-lookup"><span data-stu-id="98cd8-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="98cd8-208">Dada esta definición, el conjunto completo de las clases de los que depende una clase es el cierre reflexivo y transitivo de la ***depende directamente*** relación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="98cd8-209">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="98cd8-210">es incorrecto porque depende de la clase en sí mismo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="98cd8-211">Del mismo modo, el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="98cd8-212">es un error porque las clases tienen una dependencia circular entre ellas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="98cd8-213">Por último, el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="98cd8-214">se produce un error en tiempo de compilación porque `A` depende `B.C` (su clase base directa), que depende de `B` (su clase inmediatamente envolvente), que depende de forma circular `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="98cd8-215">Tenga en cuenta que una clase no depende de las clases que están anidadas dentro de él.</span><span class="sxs-lookup"><span data-stu-id="98cd8-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="98cd8-216">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="98cd8-217">`B` depende de `A` (porque `A` es su clase base directa y su clase inmediatamente envolvente), pero `A` no depende de `B` (puesto que `B` no es una clase base ni una clase contenedora de `A` ).</span><span class="sxs-lookup"><span data-stu-id="98cd8-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="98cd8-218">Por lo tanto, el ejemplo es válido.</span><span class="sxs-lookup"><span data-stu-id="98cd8-218">Thus, the example is valid.</span></span>

<span data-ttu-id="98cd8-219">No es posible derivar un `sealed` clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="98cd8-220">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="98cd8-221">clase `B` es un error porque intenta derivar el `sealed` clase `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="98cd8-222">Implementaciones de interfaces</span><span class="sxs-lookup"><span data-stu-id="98cd8-222">Interface implementations</span></span>

<span data-ttu-id="98cd8-223">Un *class_base* especificación puede incluir una lista de tipos de interfaz, en cuyo caso la clase se dice implementar directamente los tipos de interfaz determinado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="98cd8-224">Implementaciones de interfaz se tratan con más detalle en [implementaciones de interfaz](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="98cd8-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="98cd8-225">Restricciones de parámetro de tipo</span><span class="sxs-lookup"><span data-stu-id="98cd8-225">Type parameter constraints</span></span>

<span data-ttu-id="98cd8-226">Las declaraciones de tipo y método genéricas pueden especificar las restricciones del parámetro mediante la inclusión de *type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="98cd8-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

<span data-ttu-id="98cd8-227">Cada *type_parameter_constraints_clause* consta del token `where`, seguido del nombre de un parámetro de tipo, seguido de dos puntos y la lista de restricciones para ese parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="98cd8-228">Puede haber a lo sumo un `where` cláusula para cada parámetro de tipo y el `where` cláusulas pueden aparecer en cualquier orden.</span><span class="sxs-lookup"><span data-stu-id="98cd8-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="98cd8-229">Al igual que el `get` y `set` tokens en un descriptor de acceso de propiedad, el `where` símbolo (token) no es una palabra clave.</span><span class="sxs-lookup"><span data-stu-id="98cd8-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="98cd8-230">La lista de restricciones en un `where` cláusula puede incluir cualquiera de los siguientes componentes, en este orden: una restricción principal única, una o más restricciones secundarias y la restricción de constructor `new()`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="98cd8-231">Una restricción principal puede ser un tipo de clase o el ***hacen referencia a la restricción de tipo*** `class` o ***restricción de tipo de valor*** `struct`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="98cd8-232">Una restricción secundaria puede ser un *tipo_parámetro* o *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="98cd8-233">La restricción de tipo de referencia especifica que un argumento de tipo utilizado para el parámetro de tipo debe ser un tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="98cd8-234">Todos los tipos de clase, tipos de interfaz, tipos de delegado, los tipos de matriz y los parámetros de tipo que se sabe que es un tipo de referencia (como se define a continuación) se aplica esta restricción.</span><span class="sxs-lookup"><span data-stu-id="98cd8-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="98cd8-235">La restricción de tipo de valor especifica que un argumento de tipo utilizado para el parámetro de tipo debe ser un tipo de valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="98cd8-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="98cd8-236">Todos los tipos de estructura que no aceptan valores NULL, los tipos de enumeración y los parámetros de tipo que tiene la restricción de tipo de valor se aplica esta restricción.</span><span class="sxs-lookup"><span data-stu-id="98cd8-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="98cd8-237">Tenga en cuenta que aunque se clasifica como un tipo de valor, un tipo que acepta valores null ([tipos que aceptan valores NULL](types.md#nullable-types)) no satisface la restricción de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="98cd8-238">Un parámetro de tipo que tiene la restricción de tipo de valor no puede tener también el *constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="98cd8-239">Tipos de puntero nunca pueden ser argumentos de tipo y no se consideran para satisfacer cualquier las referencia valor o tipo de restricciones de tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="98cd8-240">Si una restricción es un tipo de clase, un tipo de interfaz o un parámetro de tipo, dicho tipo especifica un tipo base"mínimo" que debe ser compatibles con cada argumento de tipo utilizado para ese parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="98cd8-241">Cada vez que se utiliza un tipo construido o método genérico, el argumento de tipo se compara con las restricciones en el parámetro de tipo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="98cd8-242">El argumento de tipo proporcionado debe cumplir las condiciones descritas en [que satisfacen las restricciones](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="98cd8-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="98cd8-243">Un *class_type* restricción debe cumplir las reglas siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="98cd8-244">El tipo debe ser un tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-244">The type must be a class type.</span></span>
*  <span data-ttu-id="98cd8-245">El tipo no debe ser `sealed`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="98cd8-246">El tipo no debe ser uno de los siguientes tipos: `System.Array`, `System.Delegate`, `System.Enum`, o `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="98cd8-247">El tipo no debe ser `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-247">The type must not be `object`.</span></span> <span data-ttu-id="98cd8-248">Dado que todos los tipos que derivan de `object`, este tipo de restricción tendría ningún efecto si se permitiera.</span><span class="sxs-lookup"><span data-stu-id="98cd8-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="98cd8-249">A lo sumo una restricción para un parámetro de tipo especificado puede ser un tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="98cd8-250">Un tipo especificado como un *interface_type* restricción debe cumplir las reglas siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="98cd8-251">El tipo debe ser un tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="98cd8-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="98cd8-252">Un tipo no debe especificarse más de una vez en un determinado `where` cláusula.</span><span class="sxs-lookup"><span data-stu-id="98cd8-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="98cd8-253">En cualquier caso, la restricción puede incluir cualquiera de los parámetros de tipo del tipo asociado o declaración de método como parte de un tipo construido y puede implicar al tipo que se declara.</span><span class="sxs-lookup"><span data-stu-id="98cd8-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="98cd8-254">Cualquier tipo de clase o interfaz especificado como una restricción de parámetro de tipo debe ser al menos tan accesible ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)) como el tipo o método genérico que se declara.</span><span class="sxs-lookup"><span data-stu-id="98cd8-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="98cd8-255">Un tipo especificado como un *tipo_parámetro* restricción debe cumplir las reglas siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="98cd8-256">El tipo debe ser un parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="98cd8-257">Un tipo no debe especificarse más de una vez en un determinado `where` cláusula.</span><span class="sxs-lookup"><span data-stu-id="98cd8-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="98cd8-258">Además no debe haber ningún ciclo en el gráfico de dependencias de parámetros de tipo, donde dependencia es una relación transitiva definida por:</span><span class="sxs-lookup"><span data-stu-id="98cd8-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="98cd8-259">Si un parámetro de tipo `T` se utiliza como una restricción de parámetro de tipo `S` , a continuación, `S` ***depende*** `T`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="98cd8-260">Si un parámetro de tipo `S` depende de un parámetro de tipo `T` y `T` depende de un parámetro de tipo `U` , a continuación, `S` ***depende*** `U`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="98cd8-261">Dada a esta relación, es un error en tiempo de compilación de un parámetro de tipo a depender de sí misma (directa o indirectamente).</span><span class="sxs-lookup"><span data-stu-id="98cd8-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="98cd8-262">Las restricciones deben ser coherentes entre los parámetros de tipo dependiente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="98cd8-263">Si el parámetro de tipo `S` depende del parámetro de tipo `T` a continuación:</span><span class="sxs-lookup"><span data-stu-id="98cd8-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="98cd8-264">`T` no se debe tener la restricción de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="98cd8-265">En caso contrario, `T` está sellado eficazmente lo `S` hubiera estado obligado a ser el mismo tipo que `T`, eliminando la necesidad de dos parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="98cd8-266">Si `S` tiene la restricción de tipo de valor, a continuación, `T` no debe tener un *class_type* restricción.</span><span class="sxs-lookup"><span data-stu-id="98cd8-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="98cd8-267">Si `S` tiene un *class_type* restricción `A` y `T` tiene un *class_type* restricción `B` , a continuación, debe haber una conversión de identidad o implícita hacer referencia a la conversión de `A` a `B` o una conversión implícita de referencia de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="98cd8-268">Si `S` también depende del parámetro de tipo `U` y `U` tiene un *class_type* restricción `A` y `T` tiene un *class_type* restricción `B` , a continuación, debe haber una conversión de identidad o una conversión implícita de referencia de `A` a `B` o una conversión implícita de referencia de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="98cd8-269">Es válido para `S` tener la restricción de tipo de valor y `T` a tiene la restricción de tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="98cd8-270">Esto limita `T` a los tipos de `System.Object`, `System.ValueType`, `System.Enum`y cualquier tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="98cd8-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="98cd8-271">Si el `where` cláusula para un parámetro de tipo incluye una restricción de constructor (que tiene el formato `new()`), es posible utilizar el `new` operador para crear instancias del tipo ([lasexpresionesdecreacióndeobjetos](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="98cd8-272">Cualquier tipo de argumento utilizado para un parámetro de tipo con una restricción de constructor debe tener un constructor público sin parámetros (este constructor existe de manera implícita para cualquier tipo de valor) o ser un parámetro de tipo con la restricción de tipo de valor o la restricción de constructor (consulte la [Restricciones de parámetro de tipo](classes.md#type-parameter-constraints) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="98cd8-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="98cd8-273">Los siguientes son ejemplos de restricciones:</span><span class="sxs-lookup"><span data-stu-id="98cd8-273">The following are examples of constraints:</span></span>
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

<span data-ttu-id="98cd8-274">El ejemplo siguiente es error, porque provoca una circularidad en el gráfico de dependencias de los parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="98cd8-275">Los ejemplos siguientes muestran otras situaciones no válidas:</span><span class="sxs-lookup"><span data-stu-id="98cd8-275">The following examples illustrate additional invalid situations:</span></span>
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

<span data-ttu-id="98cd8-276">El ***clase base efectiva*** de un parámetro de tipo `T` se define como sigue:</span><span class="sxs-lookup"><span data-stu-id="98cd8-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="98cd8-277">Si `T` no tiene ninguna restricción principal o restricciones de parámetro de tipo, su clase base efectiva es `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="98cd8-278">Si `T` tiene la restricción de tipo de valor, su clase base efectiva es `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="98cd8-279">Si `T` tiene un *class_type* restricción `C` pero no *tipo_parámetro* restricciones, su clase base efectiva es `C`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="98cd8-280">Si `T` no tiene ningún *class_type* restricción, pero tiene uno o varios *tipo_parámetro* restricciones, su clase base efectiva es el tipo más abarcado ([eleva la conversión operadores](conversions.md#lifted-conversion-operators)) en el conjunto de clases base efectivas de su *tipo_parámetro* restricciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="98cd8-281">Las reglas de coherencia Asegúrese de que existe un tipo más abarcado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="98cd8-282">Si `T` tiene tanto un *class_type* restricción y uno o más *tipo_parámetro* restricciones, su clase base efectiva es el tipo más abarcado ([eleva la conversión operadores](conversions.md#lifted-conversion-operators)) en el conjunto formado por el *class_type* restricción de `T` y las clases base efectiva de su *tipo_parámetro* restricciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="98cd8-283">Las reglas de coherencia Asegúrese de que existe un tipo más abarcado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="98cd8-284">Si `T` tiene la restricción de tipo de referencia, pero no *class_type* restricciones, su clase base efectiva es `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="98cd8-285">Con el propósito de estas reglas, si T tiene una restricción `V` que es un *value_type*, utilice en su lugar el tipo más específico de base de `V` que es un *class_type*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="98cd8-286">Esto no puede ocurrir nunca en una restricción dada explícitamente, pero puede producirse cuando una declaración de método de reemplazo o una implementación explícita de un método de interfaz implícitamente heredan las restricciones de un método genérico.</span><span class="sxs-lookup"><span data-stu-id="98cd8-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="98cd8-287">Estas reglas garantizan que la clase base efectiva es siempre un *class_type*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="98cd8-288">El ***conjunto de interfaces efectivas*** de un parámetro de tipo `T` se define como sigue:</span><span class="sxs-lookup"><span data-stu-id="98cd8-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="98cd8-289">Si `T` no tiene ningún *secondary_constraints*, su conjunto de interfaces efectivas está vacío.</span><span class="sxs-lookup"><span data-stu-id="98cd8-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="98cd8-290">Si `T` tiene *interface_type* restricciones, pero no *tipo_parámetro* restricciones, su conjunto de interfaces efectivas es su conjunto de *interface_type* restricciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="98cd8-291">Si `T` no tiene ningún *interface_type* , pero tiene restricciones *tipo_parámetro* restricciones, su conjunto de interfaces efectiva es la unión de los conjuntos de interfaz eficaz de su *tipo_ parámetro* restricciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="98cd8-292">Si `T` tiene ambos *interface_type* restricciones y *tipo_parámetro* restricciones, su conjunto de interfaces efectiva es la unión de su conjunto de *interface_type* las restricciones y los conjuntos de interfaz eficaz de su *tipo_parámetro* restricciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="98cd8-293">Es un parámetro de tipo ***sabe que es un tipo de referencia*** si tiene la restricción de tipo de referencia o no es de su clase base efectiva `object` o `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="98cd8-294">Los valores de un tipo de parámetro de tipo restringido pueden utilizarse para tener acceso a los miembros de instancia implicados en las restricciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="98cd8-295">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-295">In the example</span></span>
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
<span data-ttu-id="98cd8-296">los métodos de `IPrintable` se puede invocar directamente en `x` porque `T` está restringido a implementar siempre `IPrintable`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="98cd8-297">Cuerpo de la clase</span><span class="sxs-lookup"><span data-stu-id="98cd8-297">Class body</span></span>

<span data-ttu-id="98cd8-298">El *class_body* de una clase define los miembros de esa clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="98cd8-299">Tipos parciales</span><span class="sxs-lookup"><span data-stu-id="98cd8-299">Partial types</span></span>

<span data-ttu-id="98cd8-300">Una declaración de tipos se puede dividir entre varios ***declaraciones parciales de tipos***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="98cd8-301">La declaración de tipos se construye a partir de sus partes siguiendo las reglas en esta sección, con lo que se trata como una sola declaración durante el resto del procesamiento de tiempo de compilación y tiempo de ejecución del programa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="98cd8-302">Un *class_declaration*, *struct_declaration* o *interface_declaration* representa una declaración de tipo parcial si incluye un `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="98cd8-303">`partial` no es una palabra clave y sólo actúa como un modificador si aparece inmediatamente antes de una de las palabras clave `class`, `struct` o `interface` en una declaración de tipo, o antes del tipo `void` en una declaración de método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="98cd8-304">En otros contextos, se puede usar como un identificador normal.</span><span class="sxs-lookup"><span data-stu-id="98cd8-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="98cd8-305">Cada parte de una declaración de tipo parcial debe incluir un `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="98cd8-306">Debe tener el mismo nombre y declararse en el mismo espacio de nombres o en otras partes de la declaración de tipos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="98cd8-307">El `partial` modificador indica que las partes adicionales de la declaración de tipos pueden existir en otra parte, pero la existencia de tales elementos adicionales no es un requisito; es válida para un tipo con una sola declaración incluir el `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="98cd8-308">Todas las partes de un tipo parcial se deben compilar juntos tal que las partes se pueden combinar en tiempo de compilación en una declaración de tipo único.</span><span class="sxs-lookup"><span data-stu-id="98cd8-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="98cd8-309">Tipos parciales no permiten ampliar los tipos ya compilados en concreto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="98cd8-310">Los tipos anidados se pueden declarar en varias partes mediante el uso de la `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="98cd8-311">Normalmente, el tipo contenedor se declara mediante `partial` bien y cada parte del tipo anidado que se declara en una parte distinta del tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="98cd8-312">El `partial` modificador no está permitido en declaraciones de delegado o enumeración.</span><span class="sxs-lookup"><span data-stu-id="98cd8-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="98cd8-313">Atributos</span><span class="sxs-lookup"><span data-stu-id="98cd8-313">Attributes</span></span>

<span data-ttu-id="98cd8-314">Los atributos de un tipo parcial se determinan mediante la combinación, en un orden no especificado, los atributos de cada una de las partes.</span><span class="sxs-lookup"><span data-stu-id="98cd8-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="98cd8-315">Si se coloca un atributo en varias partes, es equivalente a especificar el atributo varias veces en el tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="98cd8-316">Por ejemplo, las dos partes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="98cd8-317">son equivalentes a una declaración de tales como:</span><span class="sxs-lookup"><span data-stu-id="98cd8-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="98cd8-318">Atributos de tipos de parámetros se combinan de manera similar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="98cd8-319">Modificadores</span><span class="sxs-lookup"><span data-stu-id="98cd8-319">Modifiers</span></span>

<span data-ttu-id="98cd8-320">Cuando una declaración de tipo parcial incluye una especificación de accesibilidad (el `public`, `protected`, `internal`, y `private` modificadores) debe coincidir con todas las otras partes que incluyen una especificación de accesibilidad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="98cd8-321">Si ninguna parte de un tipo parcial incluye una especificación de accesibilidad, el tipo tiene la accesibilidad predeterminada apropiada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="98cd8-322">Si uno o más declaraciones parciales de un tipo anidado incluyen un `new` modificador, no se notifica ninguna advertencia si el tipo anidado oculta un miembro heredado ([ocultar mediante herencia](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="98cd8-323">Si uno o más declaraciones parciales de una clase incluyen un `abstract` modificador, la clase se considera abstracta ([clases abstractas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="98cd8-324">En caso contrario, la clase se considera no abstractas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="98cd8-325">Si uno o más declaraciones parciales de una clase incluyen un `sealed` modificador, la clase se considera sellado ([clases Sealed](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="98cd8-326">En caso contrario, se considera la clase no sellada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="98cd8-327">Tenga en cuenta que una clase no puede ser abstract y sealed.</span><span class="sxs-lookup"><span data-stu-id="98cd8-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="98cd8-328">Cuando el `unsafe` modificador se usa en una declaración de tipo parcial, solo esa parte concreta se considera un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="98cd8-329">Parámetros de tipo y restricciones</span><span class="sxs-lookup"><span data-stu-id="98cd8-329">Type parameters and constraints</span></span>

<span data-ttu-id="98cd8-330">Si un tipo genérico se declara en varias partes, cada parte debe declarar los parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="98cd8-331">Cada parte debe tener el mismo número de parámetros de tipo y el mismo nombre para cada parámetro de tipo, en orden.</span><span class="sxs-lookup"><span data-stu-id="98cd8-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="98cd8-332">Cuando una declaración de tipo genérico parcial incluye restricciones (`where` cláusulas), las restricciones deben coincidir con todas las otras partes que incluyen restricciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="98cd8-333">En concreto, cada elemento que incluya restricciones debe tener restricciones para el mismo conjunto de parámetros de tipo, y para cada parámetro de tipo de los conjuntos de principal, secundaria y las restricciones de constructor deben ser equivalentes.</span><span class="sxs-lookup"><span data-stu-id="98cd8-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="98cd8-334">Dos conjuntos de restricciones son equivalentes si contienen a los mismos miembros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="98cd8-335">Si ninguna parte de un tipo genérico parcial especifica las restricciones del parámetro, el tipo se consideran parámetros sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="98cd8-336">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-336">The example</span></span>
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
<span data-ttu-id="98cd8-337">es correcto porque aquellas partes que incluyen restricciones (los dos primeros) de forma eficaz especifican el mismo conjunto de principal, secundaria y las restricciones de constructor para el mismo conjunto de parámetros de tipo, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="98cd8-338">Clase base</span><span class="sxs-lookup"><span data-stu-id="98cd8-338">Base class</span></span>

<span data-ttu-id="98cd8-339">Cuando una declaración de clase parcial incluye una especificación de clase base, debe coincidir con todas las otras partes que incluyen una especificación de clase base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="98cd8-340">Si ninguna parte de una clase parcial incluye una especificación de clase base, se convierte en la clase base `System.Object` ([clases Base](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="98cd8-341">Interfaces base</span><span class="sxs-lookup"><span data-stu-id="98cd8-341">Base interfaces</span></span>

<span data-ttu-id="98cd8-342">El conjunto de interfaces base para un tipo declarado en varias partes es la unión de las interfaces base especificadas en cada parte.</span><span class="sxs-lookup"><span data-stu-id="98cd8-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="98cd8-343">Una interfaz base concreta solo puede llamarse una vez en cada parte, pero se permite varias partes de un nombre de las mismas interfaces bases.</span><span class="sxs-lookup"><span data-stu-id="98cd8-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="98cd8-344">Solo debe haber una implementación de los miembros de cualquier interfaz base determinada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="98cd8-345">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="98cd8-346">el conjunto de interfaces base para la clase `C` es `IA`, `IB`, y `IC`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="98cd8-347">Normalmente, cada parte proporciona una implementación de las interfaces declaradas en esa parte; Sin embargo, esto no es un requisito.</span><span class="sxs-lookup"><span data-stu-id="98cd8-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="98cd8-348">Una parte puede proporcionar la implementación para una interfaz declarada en una parte distinta:</span><span class="sxs-lookup"><span data-stu-id="98cd8-348">A part may provide the implementation for an interface declared on a different part:</span></span>
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a><span data-ttu-id="98cd8-349">Miembros</span><span class="sxs-lookup"><span data-stu-id="98cd8-349">Members</span></span>

<span data-ttu-id="98cd8-350">A excepción de los métodos parciales ([métodos parciales](classes.md#partial-methods)), el conjunto de miembros de un tipo declarado en varias partes es simplemente la unión del conjunto de miembros declarados en cada parte.</span><span class="sxs-lookup"><span data-stu-id="98cd8-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="98cd8-351">Los cuerpos de todas las partes de la declaración de tipo comparten el mismo espacio de declaración ([declaraciones](basic-concepts.md#declarations)) y el ámbito de cada miembro ([ámbitos](basic-concepts.md#scopes)) se extiende a los cuerpos de todas las partes.</span><span class="sxs-lookup"><span data-stu-id="98cd8-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="98cd8-352">El dominio de accesibilidad de cualquier miembro siempre incluye todas las partes del tipo envolvente. un `private` miembro declarado en una parte es libremente accesible desde otra parte.</span><span class="sxs-lookup"><span data-stu-id="98cd8-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="98cd8-353">Es un error en tiempo de compilación para declarar el mismo miembro en más de una parte del tipo, a menos que ese miembro es un tipo con el `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

<span data-ttu-id="98cd8-354">El orden de los miembros dentro de un tipo es rara vez significativo al código de C#, pero puede ser importante cuando se interactúa con otros lenguajes y entornos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="98cd8-355">En estos casos, el orden de los miembros dentro de un tipo declarado en varias partes es indefinido.</span><span class="sxs-lookup"><span data-stu-id="98cd8-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="98cd8-356">Métodos parciales</span><span class="sxs-lookup"><span data-stu-id="98cd8-356">Partial methods</span></span>

<span data-ttu-id="98cd8-357">Métodos parciales pueden definirse en una parte de una declaración de tipo y se implementa en otro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="98cd8-358">La implementación es opcional; Si ninguna parte implementa el método parcial, la declaración de método parcial y todas las llamadas a la se quitan de la declaración del tipo resultante de la combinación de las partes.</span><span class="sxs-lookup"><span data-stu-id="98cd8-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="98cd8-359">Los métodos parciales no se puede definir los modificadores de acceso, pero son implícitamente `private`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="98cd8-360">Tipo de valor devuelto debe ser `void`, y no pueden tener sus parámetros la `out` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="98cd8-361">El identificador `partial` se reconoce como palabra clave especial en una declaración de método sólo si aparece justo antes de que el `void` tipo; en caso contrario, se puede usar como un identificador normal.</span><span class="sxs-lookup"><span data-stu-id="98cd8-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="98cd8-362">Un método parcial no puede implementar explícitamente los métodos de interfaz.</span><span class="sxs-lookup"><span data-stu-id="98cd8-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="98cd8-363">Hay dos tipos de declaraciones de método parcial: Si el cuerpo de la declaración del método es un punto y coma, la declaración se dice que un ***definir la declaración de método parcial***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="98cd8-364">Si el cuerpo se expresa como un *bloque*, la declaración se dice que un ***implementar la declaración de método parcial***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="98cd8-365">Entre las partes de una declaración de tipo puede haber solo una declaración de método parcial con una firma determinada de definición y puede haber solo una implementación de la declaración de método parcial con una firma dada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="98cd8-366">Si tiene una declaración de método parcial de implementación, una definición de la declaración de método parcial correspondiente debe existir y deben coincidir con las declaraciones como se especifica en la siguiente:</span><span class="sxs-lookup"><span data-stu-id="98cd8-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="98cd8-367">Las declaraciones deben tener los mismos modificadores (aunque no necesariamente en el mismo orden), nombre del método, el número de parámetros de tipo y el número de parámetros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="98cd8-368">Los parámetros correspondientes en las declaraciones deben tener los mismos modificadores (aunque no necesariamente en el mismo orden) y los mismos tipos (módulo las diferencias en los nombres de parámetro de tipo).</span><span class="sxs-lookup"><span data-stu-id="98cd8-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="98cd8-369">Parámetros de tipo correspondientes en las declaraciones de deben tener las mismas restricciones (módulo las diferencias en los nombres de parámetro de tipo).</span><span class="sxs-lookup"><span data-stu-id="98cd8-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="98cd8-370">Una declaración de método parcial de implementación puede aparecer en el mismo elemento como la declaración de método parcial de definición correspondiente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="98cd8-371">Solo un método parcial definición participa en la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="98cd8-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="98cd8-372">Por lo tanto, si se proporciona una declaración de implementación, expresiones de invocación pueden resolver a las invocaciones del método parcial.</span><span class="sxs-lookup"><span data-stu-id="98cd8-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="98cd8-373">Dado que siempre devuelve un método parcial `void`, estas expresiones de invocación siempre será las instrucciones de expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="98cd8-374">Además, dado que un método parcial es implícitamente `private`, siempre se produzcan tales instrucciones dentro de una de las partes de la declaración de tipo en el que se declara el método parcial.</span><span class="sxs-lookup"><span data-stu-id="98cd8-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="98cd8-375">Si ninguna parte de una declaración de tipo parcial contiene una declaración de implementación para un determinado método parcial, cualquier instrucción de expresión que realiza la llamada simplemente se quita de la declaración de tipos combinados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="98cd8-376">Por tanto, la expresión de invocación, incluidas las expresiones constituyentes, tiene ningún efecto en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="98cd8-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="98cd8-377">El método parcial propio también se quita y no será un miembro de la declaración de tipos combinados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="98cd8-378">Si existe una declaración de implementación de un método parcial dado, se conservan las invocaciones de métodos parciales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="98cd8-379">El método parcial da lugar a una declaración de método similar a la declaración de implementación de método parcial, excepto los siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="98cd8-380">El `partial` modificador no se incluye</span><span class="sxs-lookup"><span data-stu-id="98cd8-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="98cd8-381">Los atributos de la declaración del método resultante son los atributos de la definición y la declaración de método parcial implementación combinados en un orden no especificado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="98cd8-382">No se quitan los duplicados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="98cd8-383">Los atributos de los parámetros de la declaración del método resultante son los atributos combinados de los parámetros correspondientes de la definición y la declaración de método parcial de implementación en un orden no especificado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="98cd8-384">No se quitan los duplicados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-384">Duplicates are not removed.</span></span>

<span data-ttu-id="98cd8-385">Si se especifica, pero no una declaración de implementación de una declaración de definición para un método parcial M, se aplican las restricciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="98cd8-386">Es un error en tiempo de compilación para crear un delegado al método ([expresiones de creación de delegados](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="98cd8-387">Es un error en tiempo de compilación para hacer referencia a `M` dentro de una función anónima que se convierte en un tipo de árbol de expresión ([evaluación de función anónima conversiones a tipos de árbol de expresión](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="98cd8-388">Las expresiones que se producen como parte de una invocación de `M` no afectan el estado de asignación definitiva a ([asignación definitiva](variables.md#definite-assignment)), lo que potencialmente puede provocar errores en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="98cd8-389">`M` no puede ser el punto de entrada para una aplicación ([inicio de la aplicación](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="98cd8-390">Los métodos parciales son útiles para permitir que una parte de una declaración de tipo para personalizar el comportamiento de la otra parte, por ejemplo, uno generado por una herramienta.</span><span class="sxs-lookup"><span data-stu-id="98cd8-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="98cd8-391">Considere la siguiente declaración de clase parcial:</span><span class="sxs-lookup"><span data-stu-id="98cd8-391">Consider the following partial class declaration:</span></span>
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

<span data-ttu-id="98cd8-392">Si esta clase se compila sin cualquier otra parte, se quitarán las declaraciones de método parcial definición y sus invocaciones y la declaración de clase combinado resultante será equivalente a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="98cd8-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

<span data-ttu-id="98cd8-393">Suponga que otra parte se proporciona, sin embargo, que proporciona las declaraciones de implementación de los métodos parciales:</span><span class="sxs-lookup"><span data-stu-id="98cd8-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

<span data-ttu-id="98cd8-394">A continuación, la declaración de clase combinado resultante será equivalente a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="98cd8-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a><span data-ttu-id="98cd8-395">Enlace de nombres</span><span class="sxs-lookup"><span data-stu-id="98cd8-395">Name binding</span></span>

<span data-ttu-id="98cd8-396">Aunque cada parte de un tipo extensible debe declararse dentro del mismo espacio de nombres, las partes se suelen escribir en las declaraciones de espacio de nombres diferente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="98cd8-397">Por lo tanto, diferentes `using` directivas ([directivas Using](namespaces.md#using-directives)) pueden estar presentes para cada parte.</span><span class="sxs-lookup"><span data-stu-id="98cd8-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="98cd8-398">Al interpretar los nombres simples ([inferencia](expressions.md#type-inference)) dentro de una sola parte, el `using` se consideran las directivas de las declaraciones de espacio de nombres envolvente esa parte.</span><span class="sxs-lookup"><span data-stu-id="98cd8-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="98cd8-399">Esto puede producir el mismo identificador con diferentes significados en diferentes partes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-399">This may result in the same identifier having different meanings in different parts:</span></span>
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a><span data-ttu-id="98cd8-400">Miembros de la clase</span><span class="sxs-lookup"><span data-stu-id="98cd8-400">Class members</span></span>

<span data-ttu-id="98cd8-401">Los miembros de una clase se componen de los miembros introducidos por su *class_member_declaration*s y los miembros heredan de la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

<span data-ttu-id="98cd8-402">Los miembros de un tipo de clase se dividen en las siguientes categorías:</span><span class="sxs-lookup"><span data-stu-id="98cd8-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="98cd8-403">Constantes, que representan valores constantes asociados a la clase ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="98cd8-404">Los campos, que son las variables de la clase ([campos](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="98cd8-405">Métodos, que implementan los cálculos y acciones que pueden realizarse mediante la clase ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="98cd8-406">Propiedades, que definen características con nombre y las acciones asociadas a la lectura y escritura de esas características ([propiedades](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="98cd8-407">Eventos, que definen las notificaciones que se pueden generar mediante la clase ([eventos](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="98cd8-408">Indizadores, que permiten a las instancias de la clase que se debe indexar de la misma manera (sintácticamente) que las matrices ([indizadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="98cd8-409">Operadores, que definen los operadores de expresión que se pueden aplicar a las instancias de la clase ([operadores](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="98cd8-410">Constructores de instancias, que implementan las acciones necesarias para inicializar instancias de la clase ([constructores de instancias](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="98cd8-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="98cd8-411">Los destructores, que implementan las acciones antes de instancias de la clase se descarten de forma permanente ([destructores](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="98cd8-412">Constructores estáticos, que implementan las acciones necesarias para inicializar la propia clase ([constructores estáticos](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="98cd8-413">Tipos que representan los tipos que son locales a la clase ([tipos anidados](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="98cd8-414">Los miembros que pueden contener código ejecutable se conocen colectivamente como la *miembros de función* del tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="98cd8-415">Los miembros de función de un tipo de clase son los métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos de ese tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="98cd8-416">Un *class_declaration* crea un nuevo espacio de declaración ([declaraciones](basic-concepts.md#declarations)) y el *class_member_declaration*s contenidas inmediatamente en la *clase _declaration* introducir nuevos miembros en este espacio de declaración.</span><span class="sxs-lookup"><span data-stu-id="98cd8-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="98cd8-417">Las siguientes reglas se aplican a *class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="98cd8-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="98cd8-418">Constructores de instancias, destructores y constructores estáticos deben tener el mismo nombre que la clase inmediatamente envolvente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="98cd8-419">Todos los demás miembros deben tener nombres que difieren del nombre de la clase inmediatamente envolvente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="98cd8-420">El nombre de constante, campo, propiedad, evento o tipo debe ser diferente de los nombres de todos los demás miembros declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="98cd8-421">El nombre de un método debe ser diferente de los nombres de todos los demás de que no son métodos declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="98cd8-422">Además, la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un método debe ser diferentes de las firmas de todos los demás métodos declarados en la misma clase, y dos métodos declarados en la misma clase no pueden tener signaturas que se diferencien únicamente por `ref` y `out`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="98cd8-423">La firma de un constructor de instancia debe ser diferentes de las firmas de todos los demás constructores de instancia declarados en la misma clase, y dos constructores declarados en la misma clase no pueden tener firmas que se diferencien únicamente por `ref` y `out`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="98cd8-424">La firma de un indizador debe ser diferente de las firmas de todos los demás indexadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="98cd8-425">La firma de un operador debe ser diferente de las firmas de todos los demás operadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="98cd8-426">Los miembros heredados de un tipo de clase ([herencia](classes.md#inheritance)) no forman parte del espacio de declaración de una clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="98cd8-427">Por lo tanto, una clase derivada puede declarar a un miembro con el mismo nombre o signatura que un miembro heredado (que en realidad oculta al miembro heredado).</span><span class="sxs-lookup"><span data-stu-id="98cd8-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="98cd8-428">El tipo de instancia</span><span class="sxs-lookup"><span data-stu-id="98cd8-428">The instance type</span></span>

<span data-ttu-id="98cd8-429">Cada declaración de clase tiene un tipo enlace asociado ([dependientes e independientes tipos](types.md#bound-and-unbound-types)), el ***tipo de instancia***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="98cd8-430">Para una declaración de clase genérica, el tipo de instancia se forma mediante la creación de un tipo construido ([construido tipos](types.md#constructed-types)) de la declaración de tipo, con cada uno del tipo suministrado argumentos es el correspondiente parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="98cd8-431">Puesto que el tipo de instancia utiliza los parámetros de tipo, que puede usarse únicamente donde los parámetros de tipo están en ámbito; es decir, dentro de la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="98cd8-432">El tipo de instancia es el tipo de `this` para el código escrito dentro de la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="98cd8-433">Para las clases no genéricas, el tipo de instancia es simplemente la clase declarada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="98cd8-434">A continuación muestra varias declaraciones de clase junto con sus tipos de instancia:</span><span class="sxs-lookup"><span data-stu-id="98cd8-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="98cd8-435">Miembros de tipos construidos</span><span class="sxs-lookup"><span data-stu-id="98cd8-435">Members of constructed types</span></span>

<span data-ttu-id="98cd8-436">Los miembros no heredados de un tipo construido se obtienen sustituyendo, para cada *tipo_parámetro* en la declaración de miembro correspondiente *type_argument* del tipo construido.</span><span class="sxs-lookup"><span data-stu-id="98cd8-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="98cd8-437">El proceso de sustitución se basa en el significado semántico de declaraciones de tipos y no es una sustitución simplemente textual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="98cd8-438">Por ejemplo, dada la declaración de clase genérica</span><span class="sxs-lookup"><span data-stu-id="98cd8-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="98cd8-439">el tipo construido `Gen<int[],IComparable<string>>` tiene los siguientes miembros:</span><span class="sxs-lookup"><span data-stu-id="98cd8-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="98cd8-440">El tipo del miembro `a` en la declaración de clase genérica `Gen` es "matriz bidimensional de `T`", por lo que el tipo del miembro `a` en el tipo construido anterior es "matriz bidimensional de una matriz unidimensional de `int`", o `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="98cd8-441">Dentro de los miembros de la función de instancia, el tipo de `this` es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la declaración del contenedor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="98cd8-442">Todos los miembros de una clase genérica pueden usar parámetros de tipo de cualquier clase envolvente, ya sea directamente o como parte de un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="98cd8-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="98cd8-443">Cuando se cierra un determinado tipo construido ([abierto y cerrado tipos](types.md#open-and-closed-types)) se usa en tiempo de ejecución, cada uso de un parámetro de tipo es reemplazado por el argumento de tipo real proporcionado para el tipo construido.</span><span class="sxs-lookup"><span data-stu-id="98cd8-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="98cd8-444">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-444">For example:</span></span>
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a><span data-ttu-id="98cd8-445">Herencia</span><span class="sxs-lookup"><span data-stu-id="98cd8-445">Inheritance</span></span>

<span data-ttu-id="98cd8-446">Una clase ***hereda*** los miembros de su tipo de clase base directa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="98cd8-447">La herencia significa que una clase contiene implícitamente todos los miembros de su tipo de clase base directa, excepto los constructores de instancias, destructores y constructores estáticos de la clase base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="98cd8-448">Algunos aspectos importantes de herencia son:</span><span class="sxs-lookup"><span data-stu-id="98cd8-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="98cd8-449">La herencia es transitiva.</span><span class="sxs-lookup"><span data-stu-id="98cd8-449">Inheritance is transitive.</span></span> <span data-ttu-id="98cd8-450">Si `C` se deriva de `B`, y `B` se deriva de `A`, a continuación, `C` hereda los miembros declarados en `B` , así como los miembros declarados en `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="98cd8-451">Una clase derivada extiende su clase base directa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="98cd8-452">Una clase derivada puede agregar nuevos miembros a aquellos de los que hereda, pero no puede quitar la definición de un miembro heredado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="98cd8-453">Constructores de instancias, destructores y constructores estáticos no se heredan, pero todos los demás miembros, independientemente de su accesibilidad declarada ([acceso a miembros](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="98cd8-454">Sin embargo, dependiendo de su accesibilidad declarada, es posible que los miembros heredados no esté accesibles en una clase derivada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="98cd8-455">Una clase derivada puede ***ocultar*** ([ocultar mediante herencia](basic-concepts.md#hiding-through-inheritance)) los miembros heredados mediante la declaración de nuevos miembros con el mismo nombre o signatura.</span><span class="sxs-lookup"><span data-stu-id="98cd8-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="98cd8-456">Tenga en cuenta sin embargo que ocultar un miembro heredado no elimina dicho miembro, simplemente hace que el miembro accesible directamente a través de la clase derivada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="98cd8-457">Una instancia de una clase contiene un conjunto de todos los campos de instancia declarados en la clase y sus clases base y una conversión implícita ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) existe desde un tipo de clase derivada a cualquiera de sus tipos de clase base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="98cd8-458">Por lo tanto, una referencia a una instancia de alguna clase derivada puede tratarse como una referencia a una instancia de cualquiera de sus clases base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="98cd8-459">Una clase puede declarar indizadores, propiedades y métodos virtuales y las clases derivadas pueden invalidar la implementación de estos miembros de función.</span><span class="sxs-lookup"><span data-stu-id="98cd8-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="98cd8-460">Esto permite que las clases muestren un comportamiento polimórfico ya que las acciones realizadas por una llamada a función miembro varían según el tipo de tiempo de ejecución de la instancia a través del cual se invoca ese miembro de función.</span><span class="sxs-lookup"><span data-stu-id="98cd8-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="98cd8-461">El miembro heredado de un tipo de clase construida son los miembros del tipo de clase base inmediata ([clases Base](classes.md#base-classes)), que se encuentra, sustituyendo los argumentos de tipo para cada repetición del tipo correspondiente del tipo construido los parámetros en el *class_base* especificación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="98cd8-462">Estos miembros, se transforman a su vez, sustituyendo, para cada *tipo_parámetro* en la declaración de miembro correspondiente *type_argument* de la *class_base* especificación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

<span data-ttu-id="98cd8-463">En el ejemplo anterior, el tipo construido `D<int>` tiene un miembro no heredado `public int G(string s)` obtiene sustituyendo el argumento de tipo `int` para el parámetro de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="98cd8-464">`D<int>` También tiene un miembro heredado de la declaración de clase `B`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="98cd8-465">Este miembro heredado se determina, determine primero el tipo de clase base `B<int[]>` de `D<int>` sustituyendo `int` para `T` en la especificación de la clase base `B<T[]>`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="98cd8-466">A continuación, como un argumento de tipo `B`, `int[]` se sustituye por `U` en `public U F(long index)`, produciendo el miembro heredado `public int[] F(long index)`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="98cd8-467">El nuevo modificador</span><span class="sxs-lookup"><span data-stu-id="98cd8-467">The new modifier</span></span>

<span data-ttu-id="98cd8-468">Un *class_member_declaration* se pueden declarar un miembro con el mismo nombre o signatura que un miembro heredado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="98cd8-469">Cuando esto ocurre, se dice que el miembro de clase derivada a ***ocultar*** el miembro de clase base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="98cd8-470">Ocultar a un miembro heredado no se considera un error, pero hace que el compilador emita una advertencia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="98cd8-471">Para suprimir la advertencia, la declaración del miembro de clase derivada puede incluir un `new` modificador para indicar que el miembro derivado está diseñado para ocultar el miembro base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="98cd8-472">En este tema se describe con más detalle en [ocultar mediante herencia](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="98cd8-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="98cd8-473">Si un `new` modificador se incluye en una declaración que no oculta un miembro heredado, se emite una advertencia a tal efecto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="98cd8-474">Se puede suprimir esta advertencia mediante la eliminación de la `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="98cd8-475">Modificadores de acceso</span><span class="sxs-lookup"><span data-stu-id="98cd8-475">Access modifiers</span></span>

<span data-ttu-id="98cd8-476">Un *class_member_declaration* puede tener uno de los cinco posibles tipos de accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , o `private`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="98cd8-477">Excepto para el `protected internal` combinación, es un error en tiempo de compilación para especificar más de un modificador de acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="98cd8-478">Cuando un *class_member_declaration* no incluye ningún modificador de acceso, `private` se da por hecho.</span><span class="sxs-lookup"><span data-stu-id="98cd8-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="98cd8-479">Tipos constituyentes</span><span class="sxs-lookup"><span data-stu-id="98cd8-479">Constituent types</span></span>

<span data-ttu-id="98cd8-480">Tipos que se usan en la declaración de un miembro se denominan tipos constituyentes de ese miembro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="98cd8-481">Los posibles tipos constituyentes son el tipo de una constante, campo, propiedad, indizador o evento, el tipo de valor devuelto de un método u operador y los tipos de parámetro de un método, indizador, operador o constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="98cd8-482">Los tipos constituyentes de un miembro deben ser al menos tan accesibles como el propio miembro ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="98cd8-483">Miembros estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="98cd8-483">Static and instance members</span></span>

<span data-ttu-id="98cd8-484">Son miembros de una clase ***miembros estáticos*** o ***miembros de instancia***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="98cd8-485">Por lo general, resulta útil pensar en los miembros estáticos pertenecen a tipos de clases y miembros de instancia como que pertenecen a objetos (instancias de tipos de clase).</span><span class="sxs-lookup"><span data-stu-id="98cd8-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="98cd8-486">Cuando una declaración de campo, método, propiedad, evento, operador o constructor incluye un `static` modificador, declara un miembro estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="98cd8-487">Además, una declaración de constante o tipo declara implícitamente un miembro estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="98cd8-488">Los miembros estáticos tienen las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="98cd8-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="98cd8-489">Cuando un miembro estático `M` se hace referencia en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`, `E` debe denotar un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="98cd8-490">Es un error en tiempo de compilación para `E` para denotar una instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="98cd8-491">Un campo estático identifica exactamente una ubicación de almacenamiento para ser compartidos por todas las instancias de un tipo determinado de clases cerrado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="98cd8-492">Independientemente del número de instancias de un tipo de clase cerrado determinado se crea, hay solo una copia de un campo estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="98cd8-493">Un miembro de función estático (método, propiedad, evento, operador o constructor) no funciona en una instancia concreta, y es un error en tiempo de compilación para hacer referencia a `this` en dicho miembro de función.</span><span class="sxs-lookup"><span data-stu-id="98cd8-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="98cd8-494">Cuando una declaración de campo, método, propiedad, evento, indizador, constructor o destructor no incluye un `static` modificador, declara un miembro de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="98cd8-495">(Un miembro de instancia a veces se denomina a un miembro no estático.) Los miembros de instancia tienen las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="98cd8-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="98cd8-496">Cuando un miembro de instancia `M` se hace referencia en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`, `E` debe denotar una instancia de un tipo que contiene `M`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="98cd8-497">Es un error en tiempo de enlace para `E` para denotar un tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="98cd8-498">Todas las instancias de una clase contiene un conjunto independiente de todos los campos de instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="98cd8-499">Un miembro de función de instancia (método, propiedad, indizador, constructor de instancia o destructor) funciona en una instancia determinada de la clase, y puede tener acceso a esta instancia como `this` ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="98cd8-500">El ejemplo siguiente muestra las reglas para tener acceso a estáticas y miembros de instancia:</span><span class="sxs-lookup"><span data-stu-id="98cd8-500">The following example illustrates the rules for accessing static and instance members:</span></span>
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

<span data-ttu-id="98cd8-501">El `F` método que muestra en un miembro de función de instancia, un *simple_name* ([nombres simples](expressions.md#simple-names)) puede utilizarse para tener acceso a los miembros de instancia y miembros estáticos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="98cd8-502">El `G` método muestra que en un miembro de función estático, se produce un error en tiempo de compilación para tener acceso a un miembro de instancia a través de un *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="98cd8-503">El `Main` método muestra que en un *member_access* ([acceso a miembros](expressions.md#member-access)), deben tener acceso a los miembros de instancia a través de instancias, y deben tener acceso a los miembros estáticos a través de tipos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="98cd8-504">Tipos anidados</span><span class="sxs-lookup"><span data-stu-id="98cd8-504">Nested types</span></span>

<span data-ttu-id="98cd8-505">Se llama a un tipo declarado dentro de una declaración de clase o struct un ***tipo anidado***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="98cd8-506">Un tipo que se declara dentro de una unidad de compilación o espacio de nombres se denomina un ***tipo no anidado***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="98cd8-507">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-507">In the example</span></span>
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
<span data-ttu-id="98cd8-508">clase `B` es un tipo anidado porque se declara dentro de la clase `A`y la clase `A` es un tipo no anidado porque se declara dentro de una unidad de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="98cd8-509">Nombre completo</span><span class="sxs-lookup"><span data-stu-id="98cd8-509">Fully qualified name</span></span>

<span data-ttu-id="98cd8-510">El nombre completo ([nombres completos](basic-concepts.md#fully-qualified-names)) para un tipo anidado es `S.N` donde `S` es el nombre completo del tipo en el tipo `N` se declara.</span><span class="sxs-lookup"><span data-stu-id="98cd8-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="98cd8-511">Accesibilidad declarada</span><span class="sxs-lookup"><span data-stu-id="98cd8-511">Declared accessibility</span></span>

<span data-ttu-id="98cd8-512">Los tipos no anidados pueden tener `public` o `internal` accesibilidad declarada y tener `internal` accesibilidad declarada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="98cd8-513">Los tipos anidados pueden tener estas formas de accesibilidad declarada demasiado, además de uno o más formas adicionales de accesibilidad declarada, dependiendo de si el tipo contenedor es una clase o struct:</span><span class="sxs-lookup"><span data-stu-id="98cd8-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="98cd8-514">Un tipo anidado que se declara en una clase puede tener cualquiera de las cinco formas de accesibilidad declarada (`public`, `protected internal`, `protected`, `internal`, o `private`) y, al igual que otros miembros de clase, el valor predeterminado es `private` declarado accesibilidad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="98cd8-515">Un tipo anidado que se declara en un struct puede tener cualquiera de las tres formas de accesibilidad declarada (`public`, `internal`, o `private`) y, al igual que otros miembros de struct, el valor predeterminado es `private` accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="98cd8-516">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-516">The example</span></span>
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
<span data-ttu-id="98cd8-517">declara una clase anidada privada `Node`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="98cd8-518">Ocultar</span><span class="sxs-lookup"><span data-stu-id="98cd8-518">Hiding</span></span>

<span data-ttu-id="98cd8-519">Puede ocultar un tipo anidado ([ocultación de nombres](basic-concepts.md#name-hiding)) un miembro base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="98cd8-520">El `new` modificador está permitido en declaraciones de tipo anidado para que se puede expresar explícitamente si se oculta.</span><span class="sxs-lookup"><span data-stu-id="98cd8-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="98cd8-521">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-521">The example</span></span>
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
<span data-ttu-id="98cd8-522">muestra una clase anidada `M` que oculta el método `M` definido en `Base`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="98cd8-523">Este acceso</span><span class="sxs-lookup"><span data-stu-id="98cd8-523">this access</span></span>

<span data-ttu-id="98cd8-524">Un tipo anidado y su tipo contenedor no tiene una relación especial con respecto a *this_access* ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="98cd8-525">En concreto, `this` dentro de un tipo anidado no puede usarse para hacer referencia a los miembros de instancia del tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="98cd8-526">En casos donde un tipo anidado tiene acceso a los miembros de instancia de su tipo contenedor, se puede proporcionar acceso proporcionando el `this` para la instancia del tipo contenedor como un argumento de constructor para el tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="98cd8-527">El ejemplo siguiente</span><span class="sxs-lookup"><span data-stu-id="98cd8-527">The following example</span></span>
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
<span data-ttu-id="98cd8-528">se muestra esta técnica.</span><span class="sxs-lookup"><span data-stu-id="98cd8-528">shows this technique.</span></span> <span data-ttu-id="98cd8-529">Una instancia de `C` crea una instancia de `Nested` y pasa su propio `this` a `Nested`del constructor con el fin de proporcionar acceso posterior a `C`de miembros de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="98cd8-530">Acceso a los miembros privados y protegidos del tipo contenedor</span><span class="sxs-lookup"><span data-stu-id="98cd8-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="98cd8-531">Un tipo anidado tiene acceso a todos los miembros que son accesibles para su tipo contenedor, incluidos los miembros del tipo contenedor que tienen `private` y `protected` accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="98cd8-532">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-532">The example</span></span>
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
<span data-ttu-id="98cd8-533">se muestra una clase `C` que contiene una clase anidada `Nested`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="98cd8-534">Dentro de `Nested`, el método `G` llama al método estático `F` definido en `C`, y `F` declarado privado accesibilidad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="98cd8-535">Un tipo anidado también puede tener acceso a miembros protegidos que se definen en un tipo base de su tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="98cd8-536">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-536">In the example</span></span>
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
<span data-ttu-id="98cd8-537">la clase anidada `Derived.Nested` tiene acceso al método protegido `F` definido en `Derived`de la clase base `Base`, por una llamada a través de una instancia de `Derived`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="98cd8-538">Tipos anidados en clases genéricas</span><span class="sxs-lookup"><span data-stu-id="98cd8-538">Nested types in generic classes</span></span>

<span data-ttu-id="98cd8-539">Una declaración de clase genérica puede contener declaraciones de tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="98cd8-540">Los parámetros de tipo de la clase envolvente pueden utilizarse dentro de los tipos anidados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="98cd8-541">Una declaración de tipo anidado puede contener parámetros de tipo adicionales que se aplican solo al tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="98cd8-542">Cada declaración de tipos contenido dentro de una declaración de clase genérica es implícitamente una declaración de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="98cd8-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="98cd8-543">Cuando escribe una referencia a un tipo anidado dentro de un tipo genérico, el tipo construido contenedor, incluidos sus argumentos de tipo, debe tener nombres.</span><span class="sxs-lookup"><span data-stu-id="98cd8-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="98cd8-544">Sin embargo, desde dentro de la clase externa, el tipo anidado puede utilizar sin calificación; el tipo de instancia de la clase externa puede utilizarse de forma implícita al construir el tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="98cd8-545">El ejemplo siguiente muestra tres maneras diferentes para hacer referencia a un tipo construido creado a partir de `Inner`; los dos primeros son equivalentes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

<span data-ttu-id="98cd8-546">Aunque es recomendable hacerla programación estilo, un parámetro de tipo en un tipo anidado puede ocultar a un miembro o escribir parámetro declarado en el tipo externo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="98cd8-547">Nombres de miembro reservado</span><span class="sxs-lookup"><span data-stu-id="98cd8-547">Reserved member names</span></span>

<span data-ttu-id="98cd8-548">Para facilitar la subyacente implementación C# tiempo de ejecución, para cada declaración de miembro de origen es una propiedad, evento o indizador, la implementación debe reservar dos firmas de método según el tipo de la declaración de miembro, su nombre y su tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="98cd8-549">Es un error en tiempo de compilación para un programa declarar un miembro cuya firma coincida con una de estas firmas reservadas, incluso si la implementación subyacente de tiempo de ejecución no hace uso de estas reservas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="98cd8-550">Los nombres reservados no presentan declaraciones, por lo tanto no participan en la búsqueda de miembros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="98cd8-551">Sin embargo, una declaración asociada al método reservado firmas participan en la herencia ([herencia](classes.md#inheritance)) y puede estar oculto con el `new` modificador ([el nuevo modificador](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="98cd8-552">La reserva de estos nombres desempeña tres funciones:</span><span class="sxs-lookup"><span data-stu-id="98cd8-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="98cd8-553">Para permitir la implementación subyacente utilizar un identificador normal como un nombre de método para obtener o establecer el acceso a la característica del lenguaje C#.</span><span class="sxs-lookup"><span data-stu-id="98cd8-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="98cd8-554">Para permitir que otros lenguajes interoperar con un identificador normal como un nombre de método para obtener o establecer el acceso a la característica del lenguaje C#.</span><span class="sxs-lookup"><span data-stu-id="98cd8-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="98cd8-555">Para ayudar a garantizar que el origen aceptado por un compilador adecuado es aceptado por el otro, mediante la realización de las particularidades del miembro reservado nombres coherentes en todas las implementaciones de C#.</span><span class="sxs-lookup"><span data-stu-id="98cd8-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="98cd8-556">La declaración de un destructor ([destructores](classes.md#destructors)) también hace que una firma reservar ([nombres de miembros reservados para los destructores](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="98cd8-557">Nombres de miembro reservados para las propiedades</span><span class="sxs-lookup"><span data-stu-id="98cd8-557">Member names reserved for properties</span></span>

<span data-ttu-id="98cd8-558">Para una propiedad `P` ([propiedades](classes.md#properties)) de tipo `T`, las firmas siguientes están reservadas:</span><span class="sxs-lookup"><span data-stu-id="98cd8-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="98cd8-559">Tanto las firmas están reservadas, incluso si la propiedad es de solo lectura o solo escritura.</span><span class="sxs-lookup"><span data-stu-id="98cd8-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="98cd8-560">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-560">In the example</span></span>
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
<span data-ttu-id="98cd8-561">una clase `A` define una propiedad de solo lectura `P`, reservando así las firmas para `get_P` y `set_P` métodos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="98cd8-562">Una clase `B` deriva `A` y oculta ambas de estas firmas reservadas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="98cd8-563">El ejemplo genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="98cd8-563">The example produces the output:</span></span>
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="98cd8-564">Nombres de miembros reservados para los eventos</span><span class="sxs-lookup"><span data-stu-id="98cd8-564">Member names reserved for events</span></span>

<span data-ttu-id="98cd8-565">Para un evento `E` ([eventos](classes.md#events)) del tipo de delegado `T`, las firmas siguientes están reservadas:</span><span class="sxs-lookup"><span data-stu-id="98cd8-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="98cd8-566">Nombres de miembros reservados para los indizadores</span><span class="sxs-lookup"><span data-stu-id="98cd8-566">Member names reserved for indexers</span></span>

<span data-ttu-id="98cd8-567">Para un indizador ([indizadores](classes.md#indexers)) de tipo `T` con lista de parámetros `L`, las firmas siguientes están reservadas:</span><span class="sxs-lookup"><span data-stu-id="98cd8-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="98cd8-568">Tanto las firmas están reservadas, incluso si el indizador es de solo lectura o solo escritura.</span><span class="sxs-lookup"><span data-stu-id="98cd8-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="98cd8-569">Además del nombre de miembro `Item` está reservado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="98cd8-570">Nombres de miembros reservados para los destructores</span><span class="sxs-lookup"><span data-stu-id="98cd8-570">Member names reserved for destructors</span></span>

<span data-ttu-id="98cd8-571">Para una clase que contiene un destructor ([destructores](classes.md#destructors)), se reserva la firma siguiente:</span><span class="sxs-lookup"><span data-stu-id="98cd8-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="98cd8-572">Constantes</span><span class="sxs-lookup"><span data-stu-id="98cd8-572">Constants</span></span>

<span data-ttu-id="98cd8-573">Un ***constante*** es un miembro de clase que representa un valor constante: un valor que se puede calcular en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="98cd8-574">Un *constant_declaration* presenta una o varias constantes de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="98cd8-575">Un *constant_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), un `new` modificador ([el nuevo modificador](classes.md#the-new-modifier)), y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="98cd8-576">Los atributos y modificadores se aplican a todos los miembros declarados por el *constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="98cd8-577">Aunque las constantes se consideran miembros estáticos, un *constant_declaration* no requiere ni permite un `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="98cd8-578">Es un error para el mismo modificador aparezca varias veces en una declaración de constante.</span><span class="sxs-lookup"><span data-stu-id="98cd8-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="98cd8-579">El *tipo* de un *constant_declaration* especifica el tipo de los miembros introducidos por la declaración.</span><span class="sxs-lookup"><span data-stu-id="98cd8-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="98cd8-580">El tipo es seguido por una lista de *constant_declarator*s, cada uno de los cuales incluye un nuevo miembro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="98cd8-581">Un *constant_declarator* consta de un *identificador* que designa el miembro, seguido de un "`=`" símbolo (token), seguido de un *constant_expression* ([ Expresiones constantes](expressions.md#constant-expressions)) que proporciona el valor del miembro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="98cd8-582">El *tipo* especificado en una constante debe ser declaración `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, un *enum_type*, o un *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="98cd8-583">Cada *constant_expression* debe dar un valor del tipo de destino o de un tipo que pueda convertirse al tipo de destino mediante una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="98cd8-584">El *tipo* de una constante debe ser al menos tan accesibles como la propia constante ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="98cd8-585">Se obtiene el valor de una constante en una expresión que utiliza un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="98cd8-586">Una constante puede participar en una *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="98cd8-587">Por lo tanto, se puede utilizar una constante en cualquier constructor que requiere un *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="98cd8-588">Ejemplos de tales construcciones `case` etiquetas, `goto case` instrucciones, `enum` las declaraciones de miembros, atributos y otras declaraciones de constantes.</span><span class="sxs-lookup"><span data-stu-id="98cd8-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="98cd8-589">Como se describe en [expresiones constantes](expressions.md#constant-expressions), un *constant_expression* es una expresión que se puede evaluar por completo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="98cd8-590">La única forma para crear un valor distinto de null de un *reference_type* distinto `string` consiste en aplicar el `new` operador y, puesto que el `new` operador no está permitido en un *constant_ expresión*, el único valor posible para las constantes de *reference_type*s distinto `string` es `null`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="98cd8-591">Cuando se desea un nombre simbólico para un valor constante, pero cuando el tipo de ese valor no se permite en una declaración de constante o cuando no se puede calcular el valor en tiempo de compilación por una *constant_expression*, un `readonly` campo () [Campos de solo lectura](classes.md#readonly-fields)) se puede usar en su lugar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="98cd8-592">Una declaración de constante que declara varias constantes equivale a varias declaraciones de una sola constante con los mismos atributos, los modificadores y tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="98cd8-593">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="98cd8-594">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="98cd8-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="98cd8-595">Las constantes pueden depender de otras constantes dentro del mismo programa siempre y cuando las dependencias no son de naturaleza circular.</span><span class="sxs-lookup"><span data-stu-id="98cd8-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="98cd8-596">El compilador organiza automáticamente evaluar las declaraciones de constante en el orden adecuado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="98cd8-597">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-597">In the example</span></span>
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
<span data-ttu-id="98cd8-598">el compilador evalúa primero `A.Y`, a continuación, se evalúa como `B.Z`y, por último, se evalúa como `A.X`, produciendo los valores `10`, `11`, y `12`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="98cd8-599">Las declaraciones de constantes pueden depender de constantes de otros programas, pero dichas dependencias sólo son posibles en una dirección.</span><span class="sxs-lookup"><span data-stu-id="98cd8-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="98cd8-600">Que hace referencia en el ejemplo anterior, si `A` y `B` han sido declarados en programas independientes, sería posible para `A.X` depender `B.Z`, pero `B.Z` , a continuación, podrían depender no de forma simultánea en `A.Y`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="98cd8-601">Campos</span><span class="sxs-lookup"><span data-stu-id="98cd8-601">Fields</span></span>

<span data-ttu-id="98cd8-602">Un ***campo*** es un miembro que representa una variable asociada con un objeto o clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="98cd8-603">Un *field_declaration* presenta uno o varios campos de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

<span data-ttu-id="98cd8-604">Un *field_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), un `new` modificador ([el nuevo modificador](classes.md#the-new-modifier)), un una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)) y un `static` modificador ([campos estáticos y de instancia](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="98cd8-605">Además, un *field_declaration* puede incluir un `readonly` modificador ([campos de solo lectura](classes.md#readonly-fields)) o un `volatile` modificador ([campos volátiles](classes.md#volatile-fields)), pero no ambos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="98cd8-606">Los atributos y modificadores se aplican a todos los miembros declarados por el *field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="98cd8-607">Es un error para el mismo modificador aparezca varias veces en una declaración de campo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="98cd8-608">El *tipo* de un *field_declaration* especifica el tipo de los miembros introducidos por la declaración.</span><span class="sxs-lookup"><span data-stu-id="98cd8-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="98cd8-609">El tipo es seguido por una lista de *variable_declarator*s, cada uno de los cuales incluye un nuevo miembro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="98cd8-610">Un *variable_declarator* consta de un *identificador* que los nombres de ese miembro, seguido opcionalmente por una "`=`" token y un *variable_initializer* ([ Inicializadores de variables](classes.md#variable-initializers)) que proporciona el valor inicial de ese miembro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="98cd8-611">El *tipo* de un campo debe ser al menos tan accesibles como el propio campo ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="98cd8-612">Se obtiene el valor de un campo en una expresión que utiliza un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="98cd8-613">El valor de un campo que no son de sólo lectura se modifica mediante una *asignación* ([operadores de asignación](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="98cd8-614">El valor de un campo que no son de sólo lectura puede ser obtener y modificar utilizando postfijo de incremento y decremento operadores ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)) y el incremento de prefijo y operadores de decremento ([prefijo operadores de incremento y decremento](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="98cd8-615">Una declaración de campo que declara varios campos equivale a varias declaraciones de campos solo con los mismos atributos, los modificadores y tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="98cd8-616">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="98cd8-617">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="98cd8-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="98cd8-618">Campos estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="98cd8-618">Static and instance fields</span></span>

<span data-ttu-id="98cd8-619">Cuando una declaración de campo incluye un `static` son los campos introducidos por la declaración de modificador, ***campos estáticos***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="98cd8-620">Cuando no hay ninguna `static` modificador está presente, los campos introducidos por la declaración son ***campos de instancia***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="98cd8-621">Los campos estáticos y los campos de instancia son dos de los distintos tipos de variables ([Variables](variables.md)) compatible con C# y, a veces se conocen como ***variables estáticas*** y ***las variables de instancia*** , respectivamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="98cd8-622">Un campo estático no forma parte de una instancia concreta; en su lugar, se comparte entre todas las instancias de un tipo cerrado ([abierto y cerrado tipos](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="98cd8-623">Independientemente del número de instancias de un tipo de clase cerrado se crea, hay solo una copia de un campo estático para el dominio de aplicación asociada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="98cd8-624">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-624">For example:</span></span>
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

<span data-ttu-id="98cd8-625">Un campo de instancia pertenece a una instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="98cd8-626">En concreto, todas las instancias de una clase contiene un conjunto independiente de todos los campos de instancia de esa clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="98cd8-627">Cuando se hace referencia a un campo en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`si `M` es un campo estático, `E` debe denotar un tipo que contenga `M` y si `M` es un campo de instancia, E debe denotar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="98cd8-628">Las diferencias entre estático y los miembros de instancia se tratan con más detalle en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="98cd8-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="98cd8-629">Campos de solo lectura</span><span class="sxs-lookup"><span data-stu-id="98cd8-629">Readonly fields</span></span>

<span data-ttu-id="98cd8-630">Cuando un *field_declaration* incluye un `readonly` son los campos introducidos por la declaración de modificador, ***campos de solo lectura***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="98cd8-631">Las asignaciones directas a campos de sólo lectura sólo pueden producirse como parte de la declaración o en un constructor de instancia o un constructor estático de la misma clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="98cd8-632">(Un campo de solo lectura puede asignarse a varias veces en estos contextos.) En concreto, las asignaciones directas a una `readonly` campo solo se permiten en los contextos siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="98cd8-633">En el *variable_declarator* que presenta el campo (incluyendo un *variable_initializer* en la declaración).</span><span class="sxs-lookup"><span data-stu-id="98cd8-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="98cd8-634">Para un campo de instancia, en los constructores de instancia de la clase que contiene la declaración de campo; para un campo estático, en el constructor estático de la clase que contiene la declaración de campo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="98cd8-635">Estos también son los únicos contextos en los que es válido pasar un `readonly` campo como un `out` o `ref` parámetro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="98cd8-636">Intenta asignar a un `readonly` campo o pasarla como un `out` o `ref` parámetro en cualquier otro contexto es un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="98cd8-637">Uso de campos de solo lectura estáticos para constantes</span><span class="sxs-lookup"><span data-stu-id="98cd8-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="98cd8-638">Un `static readonly` campo resulta útil cuando se desea un nombre simbólico para un valor constante, pero cuando el tipo del valor no se permite en un `const` declaración, o cuando no se puede calcular el valor en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="98cd8-639">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-639">In the example</span></span>
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
<span data-ttu-id="98cd8-640">el `Black`, `White`, `Red`, `Green`, y `Blue` miembros no pueden declararse como `const` miembros porque sus valores no se puede calcular en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="98cd8-641">Sin embargo, declararlas `static readonly` produce el mismo efecto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="98cd8-642">Control de versiones de constantes y campos de solo lectura estático</span><span class="sxs-lookup"><span data-stu-id="98cd8-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="98cd8-643">Constantes y campos de solo lectura tienen semántica de control de versiones binarias diferentes.</span><span class="sxs-lookup"><span data-stu-id="98cd8-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="98cd8-644">Cuando una expresión hace referencia a una constante, se obtiene el valor de la constante en tiempo de compilación, pero cuando una expresión hace referencia a un campo de solo lectura, no se obtiene el valor del campo hasta el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="98cd8-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="98cd8-645">Considere la posibilidad de una aplicación que consta de dos programas independientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-645">Consider an application that consists of two separate programs:</span></span>
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

<span data-ttu-id="98cd8-646">El `Program1` y `Program2` espacios de nombres indican dos programas que están compilados por separado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="98cd8-647">Dado que `Program1.Utils.X` se declara como un campo estático de solo lectura, el resultado de la `Console.WriteLine` instrucción no se conoce en tiempo de compilación, sino que se obtiene en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="98cd8-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="98cd8-648">Por lo tanto, si el valor de `X` se cambia y `Program1` se vuelve a compilar, el `Console.WriteLine` instrucción dará como resultado el nuevo valor incluso si `Program2` no se vuelven a compilar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="98cd8-649">Sin embargo, tenía `X` sido una constante, el valor de `X` habría obtenido en el momento `Program2` se compiló y no se verán afectadas por cambios en `Program1` hasta `Program2` se vuelve a compilar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="98cd8-650">Campos volátiles</span><span class="sxs-lookup"><span data-stu-id="98cd8-650">Volatile fields</span></span>

<span data-ttu-id="98cd8-651">Cuando un *field_declaration* incluye un `volatile` son los campos que aparecen en la declaración de modificador, ***campos volátiles***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="98cd8-652">Para los campos volátiles, técnicas de optimización que reordenan las instrucciones pueden provocar resultados inesperados e impredecibles en programas multiproceso que tienen acceso a los campos sin sincronización como la proporcionada por el *lock_statement*  ([La instrucción lock](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="98cd8-653">Estas optimizaciones pueden ser realizadas por el compilador, el sistema de tiempo de ejecución o el hardware.</span><span class="sxs-lookup"><span data-stu-id="98cd8-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="98cd8-654">Para los campos volátiles, dichas optimizaciones de reordenación están restringidos:</span><span class="sxs-lookup"><span data-stu-id="98cd8-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="98cd8-655">Se llama a una lectura de un campo volátil un ***lectura volátil***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="98cd8-656">Una lectura volátil tiene "semántica de adquisición"; es decir, se garantiza que se produzca antes de cualquier referencia a la memoria que se produce después de él en la secuencia de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="98cd8-657">Se llama a una operación de escritura de un campo volátil un ***escritura volátil***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="98cd8-658">Una escritura volátil tiene "semántica de liberación"; es decir, se garantiza que producirse después de todas las referencias de memoria antes de la instrucción de escritura en la secuencia de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="98cd8-659">Estas restricciones aseguran que todos los subprocesos observarán las operaciones de escritura volátiles realizadas por cualquier otro subproceso en el orden en que se realizaron.</span><span class="sxs-lookup"><span data-stu-id="98cd8-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="98cd8-660">Una implementación compatible no es necesario para proporcionar una ordenación total única de las escrituras volátiles como se muestra en todos los subprocesos de ejecución.</span><span class="sxs-lookup"><span data-stu-id="98cd8-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="98cd8-661">El tipo de un campo volátil debe ser uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="98cd8-662">Un *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-662">A *reference_type*.</span></span>
*  <span data-ttu-id="98cd8-663">El tipo `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, o` System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or` System.UIntPtr`.</span></span>
*  <span data-ttu-id="98cd8-664">Un *enum_type* con un tipo de base de enumeración de `byte`, `sbyte`, `short`, `ushort`, `int`, o `uint`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="98cd8-665">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-665">The example</span></span>
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
<span data-ttu-id="98cd8-666">genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="98cd8-666">produces the output:</span></span>
```
result = 143
```

<span data-ttu-id="98cd8-667">En este ejemplo, el método `Main` inicia un nuevo subproceso que ejecuta el método `Thread2`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="98cd8-668">Este método almacena un valor en un campo no volátil denominado `result`, a continuación, almacena `true` en el campo volátil `finished`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="98cd8-669">El subproceso principal espera para el campo `finished` para establecerse en `true`, a continuación, lee el campo `result`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="98cd8-670">Puesto que `finished` se ha declarado `volatile`, el subproceso principal debe leer el valor `143` desde el campo `result`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="98cd8-671">Si el campo `finished` no se declararon `volatile`, estaría permitida para el almacén de `result` sea visible para el subproceso principal después de la tienda a `finished`y por lo tanto, para el subproceso principal leer el valor `0` desde el campo `result`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="98cd8-672">Declarar `finished` como un `volatile` campo impide tales incoherencias.</span><span class="sxs-lookup"><span data-stu-id="98cd8-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="98cd8-673">Inicialización de campos</span><span class="sxs-lookup"><span data-stu-id="98cd8-673">Field initialization</span></span>

<span data-ttu-id="98cd8-674">El valor inicial de un campo, ya sea en un campo estático o un campo de instancia, es el valor predeterminado ([los valores predeterminados](variables.md#default-values)) del tipo del campo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="98cd8-675">No es posible observar el valor de un campo antes de que se ha producido esta inicialización predeterminada y un campo, por tanto, nunca es "no inicializado".</span><span class="sxs-lookup"><span data-stu-id="98cd8-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="98cd8-676">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-676">The example</span></span>
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
<span data-ttu-id="98cd8-677">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="98cd8-677">produces the output</span></span>
```
b = False, i = 0
```
<span data-ttu-id="98cd8-678">Dado que `b` y `i` se inicializan automáticamente en valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="98cd8-679">Inicializadores de variables</span><span class="sxs-lookup"><span data-stu-id="98cd8-679">Variable initializers</span></span>

<span data-ttu-id="98cd8-680">Pueden incluir declaraciones de campo *variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="98cd8-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="98cd8-681">Para los campos estáticos, los inicializadores de variables corresponden a instrucciones de asignación que se ejecutan durante la inicialización de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="98cd8-682">Por ejemplo, campos, los inicializadores de variables corresponden a instrucciones de asignación que se ejecutan cuando se crea una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="98cd8-683">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-683">The example</span></span>
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
<span data-ttu-id="98cd8-684">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="98cd8-684">produces the output</span></span>
```
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="98cd8-685">Dado que una asignación a `x` se produce cuando se ejecutan los inicializadores de campo estáticos y las asignaciones a `i` y `s` se producen al ejecutan los inicializadores de campo de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="98cd8-686">La inicialización de un valor predeterminado se describe en [inicialización de campos](classes.md#field-initialization) se produce para todos los campos, incluidos los campos que tienen los inicializadores de variables.</span><span class="sxs-lookup"><span data-stu-id="98cd8-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="98cd8-687">Por lo tanto, cuando se inicializa una clase, todos los campos estáticos en esa clase primero se inicializan a sus valores predeterminados y, a continuación, los inicializadores de campo estático se ejecutan en orden textual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="98cd8-688">Del mismo modo, cuando se crea una instancia de una clase, todos los campos de instancia de dicha instancia se inicializan por primera vez en sus valores predeterminados y, a continuación, los inicializadores de campo de instancia se ejecutan en orden textual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="98cd8-689">Es posible que los campos estáticos con inicializadores de variables que se deben observar en su estado de valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="98cd8-690">Sin embargo, esto no se recomienda como una cuestión de estilo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="98cd8-691">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-691">The example</span></span>
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
<span data-ttu-id="98cd8-692">muestra este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-692">exhibits this behavior.</span></span> <span data-ttu-id="98cd8-693">A pesar de las definiciones circulares de un y b, el programa es válido.</span><span class="sxs-lookup"><span data-stu-id="98cd8-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="98cd8-694">Genera la salida</span><span class="sxs-lookup"><span data-stu-id="98cd8-694">It results in the output</span></span>
```
a = 1, b = 2
```
<span data-ttu-id="98cd8-695">Dado que los campos estáticos `a` y `b` se inicializan en `0` (el valor predeterminado de `int`) antes de que se ejecutan sus inicializadores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="98cd8-696">Cuando el inicializador para `a` se ejecuta, el valor de `b` es cero de modo que `a` se inicializa en `1`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="98cd8-697">Cuando el inicializador para `b` se ejecuta, el valor de `a` ya está `1`de modo que `b` se inicializa en `2`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="98cd8-698">Inicialización de campos estáticos</span><span class="sxs-lookup"><span data-stu-id="98cd8-698">Static field initialization</span></span>

<span data-ttu-id="98cd8-699">Los inicializadores de variable de campo estático de una clase corresponden a una secuencia de asignaciones que se ejecutan en el orden textual en que aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="98cd8-700">Si un constructor estático ([constructores estáticos](classes.md#static-constructors)) existe en la clase, la ejecución de los inicializadores de campo estático, se produce inmediatamente antes de ejecutar ese constructor estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="98cd8-701">En caso contrario, los inicializadores de campo estático se ejecutan en el momento antes del primer uso de un campo estático de esa clase dependen de la implementación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="98cd8-702">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-702">The example</span></span>
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="98cd8-703">podría producir el resultado:</span><span class="sxs-lookup"><span data-stu-id="98cd8-703">might produce either the output:</span></span>
```
Init A
Init B
1 1
```
<span data-ttu-id="98cd8-704">o la salida:</span><span class="sxs-lookup"><span data-stu-id="98cd8-704">or the output:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="98cd8-705">Dado que la ejecución de `X`del inicializador y `Y`del inicializador podría producir en cualquier orden; sólo están restringidos que se produzca antes de las referencias a esos campos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="98cd8-706">Sin embargo, en el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-706">However, in the example:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
<span data-ttu-id="98cd8-707">el resultado debe ser:</span><span class="sxs-lookup"><span data-stu-id="98cd8-707">the output must be:</span></span>
```
Init B
Init A
1 1
```
<span data-ttu-id="98cd8-708">Dado que las reglas de ejecución de los constructores estáticos (tal como se define en [constructores estáticos](classes.md#static-constructors)) que proporcionan `B`del constructor estático (y por lo tanto, `B`del inicializadores de campo estático) debe ejecutar antes de `A`del constructor estático y los inicializadores de campo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="98cd8-709">Inicialización de campos de instancia</span><span class="sxs-lookup"><span data-stu-id="98cd8-709">Instance field initialization</span></span>

<span data-ttu-id="98cd8-710">Los inicializadores de variable de un campo de instancia de una clase se corresponden con una secuencia de asignaciones que se ejecutan inmediatamente después de la entrada a cualquiera de los constructores de instancias ([inicializadores de Constructor](classes.md#constructor-initializers)) de esa clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="98cd8-711">Los inicializadores de variable se ejecutan en el orden textual en que aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="98cd8-712">El proceso de creación e inicialización de la instancia de clase se describe más detalladamente en [constructores de instancias](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="98cd8-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="98cd8-713">Un inicializador de variable para un campo de instancia no puede hacer referencia a la instancia que se está creando.</span><span class="sxs-lookup"><span data-stu-id="98cd8-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="98cd8-714">Por lo tanto, es un error en tiempo de compilación para hacer referencia a `this` en un inicializador de variable, tal como se produce un error de tiempo de compilación de un inicializador de variable hacer referencia a cualquier miembro de instancia a través de un *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="98cd8-715">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="98cd8-716">el inicializador de variable para `y` da como resultado un error en tiempo de compilación porque hace referencia a un miembro de la instancia que se va a crear.</span><span class="sxs-lookup"><span data-stu-id="98cd8-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="98cd8-717">Métodos</span><span class="sxs-lookup"><span data-stu-id="98cd8-717">Methods</span></span>

<span data-ttu-id="98cd8-718">Un ***método*** es un miembro que implementa un cálculo o una acción que puede realizar un objeto o una clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="98cd8-719">Los métodos se declaran mediante *method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="98cd8-719">Methods are declared using *method_declaration*s:</span></span>

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="98cd8-720">Un *method_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso ](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` ([Invalidar métodos](classes.md#override-methods)), `sealed` ([sellado métodos](classes.md#sealed-methods)), `abstract` ([métodos abstractos](classes.md#abstract-methods)), y `extern` ([Métodos externos](classes.md#external-methods)) modificadores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="98cd8-721">Una declaración tiene una combinación válida de modificadores si se cumplen todos los elementos siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="98cd8-722">La declaración incluye una combinación válida de modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="98cd8-723">La declaración no incluye el mismo modificador varias veces.</span><span class="sxs-lookup"><span data-stu-id="98cd8-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="98cd8-724">La declaración incluye al menos uno de los siguientes modificadores: `static`, `virtual`, y `override`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="98cd8-725">La declaración incluye al menos uno de los siguientes modificadores: `new` y `override`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="98cd8-726">Si la declaración incluye el `abstract` modificador, a continuación, en la declaración no incluye ninguno de los siguientes modificadores: `static`, `virtual`, `sealed` o `extern`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="98cd8-727">Si la declaración incluye el `private` modificador, a continuación, en la declaración no incluye ninguno de los siguientes modificadores: `virtual`, `override`, o `abstract`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="98cd8-728">Si la declaración incluye el `sealed` modificador, a continuación, en la declaración también incluye el `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="98cd8-729">Si la declaración incluye el `partial` modificador, a continuación, que no incluye ninguno de los siguientes modificadores: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, o `extern`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="98cd8-730">Un método que tiene el `async` modificador es una función asincrónica y sigue las reglas descritas en [funciones asincrónicas](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="98cd8-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="98cd8-731">El *return_type* declaración especifica el tipo del valor calculado y devuelto por el método de un método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="98cd8-732">El *return_type* es `void` si el método no devuelve un valor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="98cd8-733">Si la declaración incluye el `partial` modificador, el tipo de valor devuelto debe ser `void`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="98cd8-734">El *member_name* especifica el nombre del método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="98cd8-735">A menos que el método es una implementación de miembro de interfaz explícita ([implementaciones de miembros de interfaz explícita](interfaces.md#explicit-interface-member-implementations)), el *member_name* es simplemente un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="98cd8-736">Para obtener una implementación de miembro de interfaz explícita, el *member_name* consta de un *interface_type* seguido de un "`.`" y un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="98cd8-737">El elemento opcional *type_parameter_list* especifica los parámetros de tipo del método ([parámetros de tipo](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="98cd8-738">Si un *type_parameter_list* se especifica el método es un ***método genérico***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="98cd8-739">Si el método tiene un `extern` modificador, un *type_parameter_list* no se puede especificar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="98cd8-740">El elemento opcional *formal_parameter_list* especifica los parámetros del método ([parámetros del método](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="98cd8-741">El elemento opcional *type_parameter_constraints_clause*s especificar restricciones en parámetros de tipo individuales ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) y solo se puede especificar si una *type_parameter_ lista* también se proporciona, y el método no tiene un `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="98cd8-742">El *return_type* y cada uno de los tipos que se hace referenciados en el *formal_parameter_list* de un método debe ser al menos tan accesibles como el propio método ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="98cd8-743">El *cuerpoMétodo* ya sea un punto y coma, un ***cuerpo de instrucción*** o un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="98cd8-744">Consta de un cuerpo de instrucción de un *bloque*, que especifica las instrucciones que se ejecutará cuando se invoca el método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="98cd8-745">Un cuerpo de expresión consta de `=>` seguido por un *expresión* y un punto y coma y denota una expresión única para realizar cuando se invoca el método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="98cd8-746">Para `abstract` y `extern` métodos, el *method_body* consiste simplemente en un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="98cd8-747">Para `partial` métodos el *method_body* puede constar de un punto y coma, un cuerpo de bloques o un cuerpo de expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="98cd8-748">Para todos los demás métodos, el *method_body* es un cuerpo de bloques o un cuerpo de expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="98cd8-749">Si el *cuerpoMétodo* consta de un punto y coma, a continuación, la declaración no puede incluir el `async` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="98cd8-750">El nombre, la lista de parámetros de tipo y la lista de parámetros formales de un método de definen la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) del método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="98cd8-751">En concreto, la firma de un método está formada por su nombre, el número de parámetros de tipo y el número, modificadores y tipos de sus parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="98cd8-752">Para estos fines, se identifica cualquier parámetro de tipo del método que se produce en el tipo de un parámetro formal no por su nombre, pero por su posición ordinal en la lista de argumentos de tipo del método. El tipo de valor devuelto no forma parte de una firma de método ni es los nombres de los parámetros de tipo o los parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="98cd8-753">El nombre de un método debe ser diferente de los nombres de todos los demás de que no son métodos declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="98cd8-754">Además, la firma de un método debe ser diferentes de las firmas de todos los demás métodos declarados en la misma clase, y dos métodos declarados en la misma clase no pueden tener firmas que se diferencien únicamente por `ref` y `out`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="98cd8-755">El método *tipo_parámetro*están en el ámbito de la *method_declaration*y se puede usar para tipos de formulario a lo largo de ese ámbito en *return_type*, *cuerpoMétodo*, y *type_parameter_constraints_clause*s, pero no en *atributos*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="98cd8-756">Todos los parámetros formales y parámetros de tipo deben tener nombres diferentes.</span><span class="sxs-lookup"><span data-stu-id="98cd8-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="98cd8-757">Parámetros de método</span><span class="sxs-lookup"><span data-stu-id="98cd8-757">Method parameters</span></span>

<span data-ttu-id="98cd8-758">Los parámetros de un método, si existe, se declaran en el método *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

<span data-ttu-id="98cd8-759">La lista de parámetros formales consta de uno o más parámetros separados por comas de los cuales sólo el último puede ser un *parameter_array*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="98cd8-760">Un *fixed_parameter* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), opcional `ref`, `out` o `this` modificador, un *tipo*, un *identificador* opcional *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="98cd8-761">Cada *fixed_parameter* declara un parámetro del tipo especificado con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="98cd8-762">El `this` modificador designa el método como método de extensión y solo se permite en el primer parámetro de un método estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="98cd8-763">Métodos de extensión se describen con más detalle en [métodos de extensión](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="98cd8-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="98cd8-764">Un *fixed_parameter* con un *default_argument* se conoce como un ***parámetro opcional***, mientras que un *fixed_parameter* sin un *default_argument* es un ***parámetro necesario***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="98cd8-765">Un parámetro obligatorio no puede aparecer después de un parámetro opcional en un *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="98cd8-766">Un `ref` o `out` parámetro no puede tener un *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="98cd8-767">El *expresión* en un *default_argument* debe ser uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="98cd8-768">a *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="98cd8-768">a *constant_expression*</span></span>
*  <span data-ttu-id="98cd8-769">una expresión de formato `new S()` donde `S` es un tipo de valor</span><span class="sxs-lookup"><span data-stu-id="98cd8-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="98cd8-770">una expresión de formato `default(S)` donde `S` es un tipo de valor</span><span class="sxs-lookup"><span data-stu-id="98cd8-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="98cd8-771">El *expresión* debe poder convertirse implícitamente por una identidad o una conversión que acepta valores NULL al tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="98cd8-772">Si los parámetros opcionales se producen en una declaración de método parcial de implementación ([métodos parciales](classes.md#partial-methods)), una implementación de miembro de interfaz explícita ([implementaciones de miembros de interfaz explícita](interfaces.md#explicit-interface-member-implementations)) o en un declaración de indexador único parámetro ([indizadores](classes.md#indexers)) el compilador debe mostrar una advertencia, ya que estos miembros nunca se pueden invocar de forma que permite argumentos que se omitirán.</span><span class="sxs-lookup"><span data-stu-id="98cd8-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="98cd8-773">Un *parameter_array* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), un `params` modificador, un *array_type*, y un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="98cd8-774">Una matriz de parámetros declara un único parámetro del tipo de matriz determinado con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="98cd8-775">El *array_type* de un parámetro de matriz debe ser un tipo de matriz unidimensional ([tipos de matriz](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="98cd8-776">En una invocación de método, una matriz de parámetros permite que un único argumento del tipo de matriz determinado que se especifique o permite cero o más argumentos de tipo de elemento de matriz que se especifique.</span><span class="sxs-lookup"><span data-stu-id="98cd8-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="98cd8-777">Las matrices de parámetros se describen más detalladamente en [las matrices de parámetros](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="98cd8-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="98cd8-778">Un *parameter_array* pueden aparecer después de un parámetro opcional, pero no puede tener un valor predeterminado, la omisión de argumentos para una *parameter_array* en su lugar, daría como resultado la creación de una matriz vacía.</span><span class="sxs-lookup"><span data-stu-id="98cd8-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="98cd8-779">El ejemplo siguiente muestra los diferentes tipos de parámetros:</span><span class="sxs-lookup"><span data-stu-id="98cd8-779">The following example illustrates different kinds of parameters:</span></span>
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

<span data-ttu-id="98cd8-780">En el *formal_parameter_list* para `M`, `i` es un parámetro ref necesarios, `d` es un parámetro de valor requerido, `b`, `s`, `o` y `t` parámetros de valor opcional y `a` es una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="98cd8-781">Una declaración de método crea un espacio de declaración independiente para los parámetros, parámetros de tipo y las variables locales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="98cd8-782">Los nombres se incluyen en este espacio de declaración mediante la lista de parámetros de tipo y la lista de parámetros formales del método y las declaraciones de variable locales en el *bloque* del método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="98cd8-783">Es un error para dos miembros de un espacio de declaración de método tienen el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="98cd8-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="98cd8-784">Es un error para el espacio de declaración de método y el espacio de declaración de variable local de un espacio de declaración anidado contengan elementos con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="98cd8-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="98cd8-785">Una invocación de método ([las invocaciones de método](expressions.md#method-invocations)) crea una copia, específica de esa invocación, de los parámetros formales y variables locales del método y la lista de argumentos de la invocación asigna valores o referencias a variables a la acaba de crearse parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="98cd8-786">Dentro de la *bloque* de un método, se pueden hacer referencia a parámetros formales mediante sus identificadores en *simple_name* expresiones ([nombres simples](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="98cd8-787">Hay cuatro tipos de parámetros formales:</span><span class="sxs-lookup"><span data-stu-id="98cd8-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="98cd8-788">Parámetros de valor, que se declaran sin modificadores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="98cd8-789">Parámetros de referencia, que se declaran con la `ref` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="98cd8-790">Los parámetros de salida, que se declaran con la `out` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="98cd8-791">Las matrices de parámetros, que se declaran con la `params` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="98cd8-792">Como se describe en [firmas y sobrecarga](basic-concepts.md#signatures-and-overloading), `ref` y `out` modificadores forman parte de una firma de método, pero la `params` modificador no es.</span><span class="sxs-lookup"><span data-stu-id="98cd8-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="98cd8-793">Parámetros de valor</span><span class="sxs-lookup"><span data-stu-id="98cd8-793">Value parameters</span></span>

<span data-ttu-id="98cd8-794">Un parámetro declarado sin modificadores es un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="98cd8-795">Un parámetro de valor corresponde a una variable local que obtiene su valor inicial del argumento correspondiente proporcionado en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="98cd8-796">Cuando un parámetro formal es un parámetro de valor, el argumento correspondiente en una invocación de método debe ser una expresión que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo de parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="98cd8-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="98cd8-797">Se permite un método para asignar nuevos valores a un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="98cd8-798">Estas asignaciones sólo afectan a la ubicación de almacenamiento local representada por el valor del parámetro, no tienen ningún efecto en el argumento real definido en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="98cd8-799">Parámetros de referencia</span><span class="sxs-lookup"><span data-stu-id="98cd8-799">Reference parameters</span></span>

<span data-ttu-id="98cd8-800">Un parámetro declarado con un `ref` modificador es un parámetro de referencia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="98cd8-801">A diferencia de un parámetro de valor, un parámetro de referencia no crea una nueva ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="98cd8-802">En su lugar, un parámetro de referencia representa la misma ubicación de almacenamiento que la variable especificada como argumento en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="98cd8-803">Cuando un parámetro formal es un parámetro de referencia, el argumento correspondiente en una invocación de método debe constar de la palabra clave `ref` seguido por un *variable_reference* ([reglas precisas para determinar asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) del mismo tipo del parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="98cd8-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="98cd8-804">Una variable debe estar asignada definitivamente antes de que se puede pasar como un parámetro de referencia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="98cd8-805">Dentro de un método, un parámetro de referencia siempre se considera asignado definitivamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="98cd8-806">Un método declarado como iterador ([iteradores](classes.md#iterators)) no puede tener parámetros de referencia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="98cd8-807">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-807">The example</span></span>
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
<span data-ttu-id="98cd8-808">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="98cd8-808">produces the output</span></span>
```
i = 2, j = 1
```

<span data-ttu-id="98cd8-809">Para la invocación de `Swap` en `Main`, `x` representa `i` y `y` representa `j`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="98cd8-810">Por lo tanto, la invocación tiene el efecto de intercambiar los valores de `i` y `j`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="98cd8-811">En un método que toma parámetros de referencia, es posible que varios nombres representar la misma ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="98cd8-812">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-812">In the example</span></span>
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
<span data-ttu-id="98cd8-813">la invocación de `F` en `G` pasa una referencia al `s` para ambos `a` y `b`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="98cd8-814">Por lo tanto, para esa invocación, los nombres `s`, `a`, y `b` todas hacen referencia a la misma ubicación de almacenamiento y las tres asignaciones modifican el campo de instancia `s`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="98cd8-815">Parámetros de salida</span><span class="sxs-lookup"><span data-stu-id="98cd8-815">Output parameters</span></span>

<span data-ttu-id="98cd8-816">Un parámetro declarado con un `out` modificador es un parámetro de salida.</span><span class="sxs-lookup"><span data-stu-id="98cd8-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="98cd8-817">Al igual que un parámetro de referencia, un parámetro de salida no crea una nueva ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="98cd8-818">En su lugar, un parámetro de salida representa la misma ubicación de almacenamiento que la variable especificada como argumento en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="98cd8-819">Cuando un parámetro formal es un parámetro de salida, el argumento correspondiente en una invocación de método debe constar de la palabra clave `out` seguido por un *variable_reference* ([reglas precisas para determinar asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) del mismo tipo del parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="98cd8-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="98cd8-820">Una variable no debe estar asignada definitivamente antes de que se puede pasar como un parámetro de salida, pero después de una invocación donde se pasó una variable como un parámetro de salida, la variable se considera asignada definitivamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="98cd8-821">Dentro de un método, al igual que una variable local variable, un parámetro de salida se considera inicialmente sin asignar y debe estar previamente asignada antes de que se utiliza su valor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="98cd8-822">Cada parámetro de salida de un método debe asignarse definitivamente antes de que el método devuelve.</span><span class="sxs-lookup"><span data-stu-id="98cd8-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="98cd8-823">Un método declarado como un método parcial ([métodos parciales](classes.md#partial-methods)) o un iterador ([iteradores](classes.md#iterators)) no puede tener parámetros de salida.</span><span class="sxs-lookup"><span data-stu-id="98cd8-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="98cd8-824">Los parámetros de salida se usan normalmente en los métodos que devuelven varios valores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="98cd8-825">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-825">For example:</span></span>
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

<span data-ttu-id="98cd8-826">El ejemplo genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="98cd8-826">The example produces the output:</span></span>
```
c:\Windows\System\
hello.txt
```

<span data-ttu-id="98cd8-827">Tenga en cuenta que el `dir` y `name` pueden estar sin asignadas variables antes de pasarlos a `SplitPath`, y que se considera asignados definitivamente posterior a la llamada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="98cd8-828">Matrices de parámetros</span><span class="sxs-lookup"><span data-stu-id="98cd8-828">Parameter arrays</span></span>

<span data-ttu-id="98cd8-829">Un parámetro declarado con un `params` modificador es una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="98cd8-830">Si una lista de parámetros formales incluye una matriz de parámetros, debe ser el último parámetro en la lista y debe ser de un tipo de matriz unidimensional.</span><span class="sxs-lookup"><span data-stu-id="98cd8-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="98cd8-831">Por ejemplo, los tipos `string[]` y `string[][]` puede usarse como el tipo de una matriz de parámetros, pero el tipo `string[,]` no puede.</span><span class="sxs-lookup"><span data-stu-id="98cd8-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="98cd8-832">No es posible combinar la `params` modificador con los modificadores `ref` y `out`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="98cd8-833">Una matriz de parámetros permite argumentos que se especifique en uno de dos formas de una invocación de método:</span><span class="sxs-lookup"><span data-stu-id="98cd8-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="98cd8-834">El argumento especificado para una matriz de parámetros puede ser una sola expresión que es implícitamente convertible ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo de matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="98cd8-835">En este caso, la matriz de parámetros actúa exactamente como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="98cd8-836">Como alternativa, la invocación puede especificar cero o más argumentos para la matriz de parámetros, donde cada argumento es una expresión que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo de elemento de la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="98cd8-837">En este caso, la invocación crea una instancia del parámetro de tipo de matriz con una longitud correspondiente al número de argumentos, inicializa los elementos de la instancia de matriz con los valores de argumento especificado y usa la instancia de matriz recién creado como real argumento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="98cd8-838">Excepto para permitir un número variable de argumentos en una invocación, una matriz de parámetros equivale exactamente a un parámetro de valor ([parámetros del valor](classes.md#value-parameters)) del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="98cd8-839">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-839">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
<span data-ttu-id="98cd8-840">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="98cd8-840">produces the output</span></span>
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="98cd8-841">La primera invocación de `F` simplemente pasa la matriz `a` como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="98cd8-842">La segunda invocación de `F` crea automáticamente un elemento de cuatro `int[]` con los valores de elemento especificados y pasa ese instancia como un parámetro de valor de matriz.</span><span class="sxs-lookup"><span data-stu-id="98cd8-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="98cd8-843">Del mismo modo, la tercera invocación de `F` crea un elemento de cero `int[]` y pasa esa instancia como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="98cd8-844">Las invocaciones de segunda y terceros son exactamente equivalentes a escribir:</span><span class="sxs-lookup"><span data-stu-id="98cd8-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="98cd8-845">Al realizar la resolución de sobrecarga, un método con una matriz de parámetros puede ser aplicable en su forma normal o en su forma expandida ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="98cd8-846">La forma expandida de un método está disponible sólo si la forma normal del método no es aplicable, y solo si un método aplicable con la misma firma que la forma expandida ya no está declarado en el mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="98cd8-847">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-847">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
<span data-ttu-id="98cd8-848">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="98cd8-848">produces the output</span></span>
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="98cd8-849">En el ejemplo, dos de las posibles formas expandidas del método con una matriz de parámetros ya están incluidas en la clase con métodos periódicas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="98cd8-850">Estas formas expandidas no por lo tanto, se consideran al realizar la resolución de sobrecarga y las invocaciones de método primero y tercero, por tanto, seleccionan los métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="98cd8-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="98cd8-851">Cuando una clase declara un método con una matriz de parámetros, no es raro que incluir también algunas de las formas expandidas como métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="98cd8-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="98cd8-852">Por lo que es posible evitar la asignación de una matriz se invoca la instancia que se produce cuando una forma expandida de un método con una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="98cd8-853">Cuando el tipo de una matriz de parámetros es `object[]`, puede producirse una ambigüedad entre la forma normal del método y la forma expandida para un único `object` parámetro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="98cd8-854">La razón de la ambigüedad es que un `object[]` es implícitamente convertible al tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="98cd8-855">La ambigüedad no presenta ningún problema, sin embargo, puesto que se puede resolver mediante la inserción de una conversión explícita si es necesario.</span><span class="sxs-lookup"><span data-stu-id="98cd8-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="98cd8-856">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-856">The example</span></span>
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
<span data-ttu-id="98cd8-857">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="98cd8-857">produces the output</span></span>
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="98cd8-858">En la primera y última invocación de `F`, la forma normal de `F` es aplicable porque existe una conversión implícita desde el tipo de argumento para el tipo de parámetro (ambos son de tipo `object[]`).</span><span class="sxs-lookup"><span data-stu-id="98cd8-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="98cd8-859">Por lo tanto, la resolución de sobrecarga selecciona la forma normal de `F`, y el argumento se pasa como un parámetro de valor normales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="98cd8-860">En las invocaciones segunda y terceros, la forma normal de `F` no es aplicable porque no existe ninguna conversión implícita desde el tipo de argumento para el tipo de parámetro (tipo `object` no se puede convertir implícitamente al tipo `object[]`).</span><span class="sxs-lookup"><span data-stu-id="98cd8-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="98cd8-861">Sin embargo, la forma expandida de `F` es aplicable, por lo que se selecciona mediante la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="98cd8-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="98cd8-862">Como resultado, un solo elemento `object[]` se crea mediante la invocación, y el único elemento de la matriz se inicializa con el valor del argumento especificado (que a su vez es una referencia a un `object[]`).</span><span class="sxs-lookup"><span data-stu-id="98cd8-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="98cd8-863">Métodos estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="98cd8-863">Static and instance methods</span></span>

<span data-ttu-id="98cd8-864">Cuando una declaración de método incluye un `static` modificador, que el método se dice que es un método estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="98cd8-865">Cuando no hay ninguna `static` modificador está presente, se dice que el método es un método de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="98cd8-866">Un método estático no opera en una instancia concreta, y es un error en tiempo de compilación para hacer referencia a `this` en un método estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="98cd8-867">Un método de instancia opera en una instancia determinada de una clase, y puede tener acceso a esa instancia como `this` ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="98cd8-868">Cuando se hace referencia a un método en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`si `M` es un método estático, `E` debe denotar un tipo que contiene `M`y si `M` es un método de instancia, `E` debe denotar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="98cd8-869">Las diferencias entre estático y los miembros de instancia se tratan con más detalle en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="98cd8-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="98cd8-870">Métodos virtuales</span><span class="sxs-lookup"><span data-stu-id="98cd8-870">Virtual methods</span></span>

<span data-ttu-id="98cd8-871">Cuando una declaración de método de instancia incluye un `virtual` modificador, que el método se dice que es un método virtual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="98cd8-872">Cuando no hay ninguna `virtual` modificador está presente, se dice que el método es un método no virtual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="98cd8-873">La implementación de un método no virtual es invariable: La implementación es la misma, si se invoca el método en una instancia de la clase en que se declaró o una instancia de una clase derivada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="98cd8-874">En cambio, las clases derivadas pueden reemplazar la implementación de un método virtual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="98cd8-875">El proceso de sustitución de la implementación de un método virtual heredado se conoce como ***reemplazar*** ese método ([invalidar métodos](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="98cd8-876">En una invocación de método virtual, el ***tipo en tiempo de ejecución*** de la instancia para el que tiene esa invocación lugar determina la implementación del método real a invocar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="98cd8-877">En una invocación de método no virtual, el ***tipo en tiempo de compilación*** de la instancia es el factor determinante.</span><span class="sxs-lookup"><span data-stu-id="98cd8-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="98cd8-878">En términos precisos, cuando un método denominado `N` se invoca con una lista de argumentos `A` en una instancia con un tipo de tiempo de compilación `C` y un tipo de tiempo de ejecución `R` (donde `R` sea `C` o una clase derivada desde `C`), la invocación se procesa como sigue:</span><span class="sxs-lookup"><span data-stu-id="98cd8-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="98cd8-879">En primer lugar, se aplica la resolución de sobrecarga a `C`, `N`, y `A`para seleccionar un método específico `M` desde el conjunto de métodos declarada en y heredadas por `C`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="98cd8-880">Esto se describe en [las invocaciones de método](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="98cd8-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="98cd8-881">A continuación, si `M` es un método no virtual, `M` se invoca.</span><span class="sxs-lookup"><span data-stu-id="98cd8-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="98cd8-882">En caso contrario, `M` es un método virtual y la implementación más derivada de `M` con respecto a `R` se invoca.</span><span class="sxs-lookup"><span data-stu-id="98cd8-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="98cd8-883">Para cada método virtual declarado en o heredado por una clase, existe una ***implementación más derivada*** del método con respecto a esa clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="98cd8-884">La implementación más derivada de un método virtual `M` con respecto a una clase `R` se determina como sigue:</span><span class="sxs-lookup"><span data-stu-id="98cd8-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="98cd8-885">Si `R` contiene la presentación de `virtual` declaración de `M`, a continuación, se trata de la implementación más derivada de `M`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="98cd8-886">De lo contrario, si `R` contiene un `override` de `M`, a continuación, se trata de la implementación más derivada de `M`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="98cd8-887">La mayor parte de lo contrario, la implementación de derivada `M` con respecto a `R` es igual que la implementación más derivada de `M` con respecto a la clase base directa de `R`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="98cd8-888">En el ejemplo siguiente se ilustra las diferencias entre los métodos virtuales y no virtuales:</span><span class="sxs-lookup"><span data-stu-id="98cd8-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

<span data-ttu-id="98cd8-889">En el ejemplo, `A` presenta un método no virtual `F` y un método virtual `G`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="98cd8-890">La clase `B` presenta un nuevo método no virtual `F`, ocultando así las heredadas `F`y también invalida el método heredado `G`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="98cd8-891">El ejemplo genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="98cd8-891">The example produces the output:</span></span>
```
A.F
B.F
B.G
B.G
```

<span data-ttu-id="98cd8-892">Tenga en cuenta que la instrucción `a.G()` invoca `B.G`, no `A.G`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="98cd8-893">Esto es porque el tipo de tiempo de ejecución de la instancia (que es `B`), no el tipo de tiempo de compilación de la instancia (que es `A`), determina la implementación del método real a invocar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="98cd8-894">Dado que se permiten métodos para ocultar los métodos heredados, es posible que una clase que contiene varios métodos virtuales con la misma firma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="98cd8-895">Esto no presenta un problema de ambigüedad, puesto que se ocultan todos excepto el método más derivado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="98cd8-896">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-896">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
<span data-ttu-id="98cd8-897">el `C` y `D` clases contienen dos métodos virtuales con la misma firma: El introducidos por `A` y otro introducido por `C`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="98cd8-898">El método introducido por `C` oculta el método heredado de `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="98cd8-899">Por lo tanto, la declaración de reemplazo en `D` invalida el método introducido por `C`, y no es posible que `D` para invalidar el método introducido por `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="98cd8-900">El ejemplo genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="98cd8-900">The example produces the output:</span></span>
```
B.F
B.F
D.F
D.F
```

<span data-ttu-id="98cd8-901">Tenga en cuenta que es posible invocar el método virtual oculto mediante el acceso a una instancia de `D` a través de una menor derivada tipo en el que el método no está oculto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="98cd8-902">Invalidar métodos</span><span class="sxs-lookup"><span data-stu-id="98cd8-902">Override methods</span></span>

<span data-ttu-id="98cd8-903">Cuando una declaración de método de instancia incluye un `override` modificador, se dice que el método sea un ***invalidar el método***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="98cd8-904">Un método de invalidación invalida un método virtual heredado con la misma firma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="98cd8-905">Mientras que una declaración de método virtual introduce un método nuevo, una declaración de método de reemplazo especializa un método virtual heredado existente proporcionando una nueva implementación de ese método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="98cd8-906">El método reemplazado por una `override` declaración se conoce como el ***se invalida el método base***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="98cd8-907">Para un método de invalidación `M` declarado en una clase `C`, el método base reemplazado se determina examinando cada tipo de clase base del `C`, empezando por el tipo de clase base directa de `C` y continuando con cada sucesivas tipo de clase base directa, hasta en un tipo de clase base determinada, al menos uno es el método accesible encuentra que tiene la misma firma que `M` después de la sustitución de argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="98cd8-908">Para los fines de buscar el método base reemplazado, se considera accesible si es un método `public`, si es `protected`, si es `protected internal`, o si es `internal` y se ha declarado en el mismo programa como `C`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="98cd8-909">Se produce un error de tiempo de compilación a menos que todos los elementos siguientes son true para una declaración de reemplazo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="98cd8-910">Un método base invalidado puede encontrarse como se describió anteriormente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="98cd8-911">No hay exactamente un tal método base invalidado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="98cd8-912">Esta restricción tiene efecto solo si el tipo de clase base es un tipo construido donde la sustitución de argumentos de tipo hace que la firma de los dos métodos de la misma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="98cd8-913">El método base reemplazado es un virtual, abstract o invalidar el método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="98cd8-914">En otras palabras, el método base reemplazado no puede ser estático o no virtual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="98cd8-915">El método base reemplazado no es un método sellado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="98cd8-916">El método de reemplazo y el método base reemplazado tienen el mismo tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="98cd8-917">La declaración de reemplazo y el método base reemplazado tienen la misma accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="98cd8-918">En otras palabras, una declaración de reemplazo no puede cambiar la accesibilidad del método virtual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="98cd8-919">Sin embargo, si el método base reemplazado es protected internal y se declara en un ensamblado diferente que el ensamblado que contiene el método de invalidación, a continuación, el método de invalidación declarado accesibilidad debe estar protegida.</span><span class="sxs-lookup"><span data-stu-id="98cd8-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="98cd8-920">La declaración de reemplazo no especifica el tipo de parámetro restricciones cláusulas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="98cd8-921">En su lugar, las restricciones se heredan del método base invalidado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="98cd8-922">Tenga en cuenta que las restricciones que son parámetros de tipo en el método invalidado pueden reemplazarse por argumentos de tipo en la restricción heredada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="98cd8-923">Esto puede conducir a las restricciones que no sean válidas cuando se especifica explícitamente, como tipos de valor o tipos sellados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="98cd8-924">El ejemplo siguiente muestra cómo funcionan las reglas de reemplazo para las clases genéricas:</span><span class="sxs-lookup"><span data-stu-id="98cd8-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

<span data-ttu-id="98cd8-925">Puede tener acceso a una declaración de reemplazo del método base invalidado utilizando un *base_access* ([Base acceso](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="98cd8-926">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-926">In the example</span></span>
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
<span data-ttu-id="98cd8-927">el `base.PrintFields()` invocación en `B` invoca el `PrintFields` método declarado en `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="98cd8-928">Un *base_access* deshabilita el mecanismo de llamada virtual y simplemente se trata del método base como un método no virtual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="98cd8-929">Tenían la invocación de `B` sido escritos `((A)this).PrintFields()`, lo haría de forma recursiva invocar el `PrintFields` método declarado en `B`, no uno que se declara en `A`, puesto que `PrintFields` es virtual y el tipo de tiempo de ejecución de `((A)this)` es `B`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="98cd8-930">Mediante la inclusión de un `override` can modificador un método invalidan otro método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="98cd8-931">En todos los demás casos, un método con la misma firma que un método heredado simplemente oculta el método heredado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="98cd8-932">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-932">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
<span data-ttu-id="98cd8-933">el `F` método `B` no incluye un `override` modificador y, por tanto, no reemplaza el `F` método `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="98cd8-934">En su lugar, el `F` método `B` oculta el método en `A`, y se notifica una advertencia porque la declaración no incluye un `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="98cd8-935">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-935">In the example</span></span>
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
<span data-ttu-id="98cd8-936">el `F` método `B` oculta virtual `F` método hereda `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="98cd8-937">Desde el nuevo `F` en `B` tiene acceso privado, su ámbito sólo incluye el cuerpo de la clase de `B` y no se extiende a `C`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="98cd8-938">Por lo tanto, la declaración de `F` en `C` se permite reemplazar la `F` hereda `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="98cd8-939">Métodos sellados</span><span class="sxs-lookup"><span data-stu-id="98cd8-939">Sealed methods</span></span>

<span data-ttu-id="98cd8-940">Cuando una declaración de método de instancia incluye un `sealed` modificador, que el método se dice que un ***sellado método***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="98cd8-941">Si una declaración de método de instancia incluye la `sealed` modificador, también debe incluir el `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="98cd8-942">El uso de la `sealed` modificador impide que una clase derivada siga reemplazando el método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="98cd8-943">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-943">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
<span data-ttu-id="98cd8-944">la clase `B` proporciona dos métodos de reemplazo: un `F` método que tiene el `sealed` modificador y un `G` método que no lo hace.</span><span class="sxs-lookup"><span data-stu-id="98cd8-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="98cd8-945">`B`uso del sellado `modifier` impide `C` invaliden aún más `F`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="98cd8-946">Métodos abstractos</span><span class="sxs-lookup"><span data-stu-id="98cd8-946">Abstract methods</span></span>

<span data-ttu-id="98cd8-947">Cuando una declaración de método de instancia incluye un `abstract` modificador, que el método se dice que un ***método abstracto***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="98cd8-948">Aunque un método abstracto es también implícitamente un método virtual, no puede tener el modificador `virtual`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="98cd8-949">Una declaración de método abstracto introduce un nuevo método virtual pero no proporciona una implementación de ese método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="98cd8-950">En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación invalidando este método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="98cd8-951">Dado que un método abstracto no proporciona ninguna implementación real, el *cuerpoMétodo* de un método abstracto consiste simplemente en un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="98cd8-952">Solo se permiten declaraciones de métodos abstractos en clases abstractas ([clases abstractas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="98cd8-953">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-953">In the example</span></span>
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
<span data-ttu-id="98cd8-954">la `Shape` clase define el concepto abstracto de un objeto de forma geométrica que puede dibujarse a sí mismo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="98cd8-955">El `Paint` método es abstracto, porque no hay ninguna implementación predeterminada significativa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="98cd8-956">El `Ellipse` y `Box` clases son concretas `Shape` implementaciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="98cd8-957">Dado que estas clases no son abstractas, es necesario reemplazar el `Paint` método y proporcionar una implementación real.</span><span class="sxs-lookup"><span data-stu-id="98cd8-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="98cd8-958">Es un error en tiempo de compilación para un *base_access* ([Base acceso](expressions.md#base-access)) para hacer referencia a un método abstracto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="98cd8-959">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-959">In the example</span></span>
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
<span data-ttu-id="98cd8-960">se notifica un error en tiempo de compilación para el `base.F()` invocación porque hace referencia a un método abstracto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="98cd8-961">Una declaración de método abstracto se permite reemplazar un método virtual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="98cd8-962">Esto permite que una clase abstracta fuerce una nueva implementación del método en las clases derivadas y hace que la implementación original del método no está disponible.</span><span class="sxs-lookup"><span data-stu-id="98cd8-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="98cd8-963">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-963">In the example</span></span>
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
<span data-ttu-id="98cd8-964">clase `A` declara un método virtual, la clase `B` invalida este método con un método abstracto y una clase `C` reemplaza el método abstracto para proporcionar su propia implementación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="98cd8-965">Métodos externos</span><span class="sxs-lookup"><span data-stu-id="98cd8-965">External methods</span></span>

<span data-ttu-id="98cd8-966">Cuando una declaración de método incluye un `extern` modificador, que el método se dice que un ***método externo***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="98cd8-967">Métodos externos se implementan de forma externa, normalmente mediante un lenguaje distinto de C#.</span><span class="sxs-lookup"><span data-stu-id="98cd8-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="98cd8-968">Dado que una declaración de método externo no proporciona ninguna implementación real, el *cuerpoMétodo* de un método externo consiste simplemente en un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="98cd8-969">Un método externo no puede ser genérico.</span><span class="sxs-lookup"><span data-stu-id="98cd8-969">An external method may not be generic.</span></span>

<span data-ttu-id="98cd8-970">El `extern` modificador se usa normalmente junto con un `DllImport` atributo ([interoperabilidad con componentes COM y Win32](attributes.md#interoperation-with-com-and-win32-components)), lo que permite métodos externos que se implementa en los archivos DLL (bibliotecas de vínculos dinámicos).</span><span class="sxs-lookup"><span data-stu-id="98cd8-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="98cd8-971">El entorno de ejecución puede admitir otros mecanismos mediante el cual se pueden proporcionar implementaciones de métodos externos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="98cd8-972">Cuando un método externo incluye un `DllImport` atributo, la declaración del método también debe incluir un `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="98cd8-973">En este ejemplo se muestra el uso de la `extern` modificador y `DllImport` atributo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a><span data-ttu-id="98cd8-974">Métodos parciales (resumen)</span><span class="sxs-lookup"><span data-stu-id="98cd8-974">Partial methods (recap)</span></span>

<span data-ttu-id="98cd8-975">Cuando una declaración de método incluye un `partial` modificador, que el método se dice que un ***método parcial***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="98cd8-976">Los métodos parciales solo se pueden declarar como miembros de tipos parciales ([tipos parciales](classes.md#partial-types)) y están sujetos a un número de restricciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="98cd8-977">Los métodos parciales se describen con más detalle en [métodos parciales](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="98cd8-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="98cd8-978">Métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="98cd8-978">Extension methods</span></span>

<span data-ttu-id="98cd8-979">Cuando el primer parámetro de un método incluye el `this` modificador, que el método se dice que un ***método de extensión***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="98cd8-980">Métodos de extensión solo pueden declararse en clases estáticas no genérica y no anidados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="98cd8-981">El primer parámetro de un método de extensión no puede tener ningún modificador distinto `this`, y el tipo de parámetro no puede ser un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="98cd8-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="98cd8-982">El siguiente es un ejemplo de una clase estática que declara dos métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="98cd8-982">The following is an example of a static class that declares two extension methods:</span></span>
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

<span data-ttu-id="98cd8-983">Un método de extensión es un método estático normal.</span><span class="sxs-lookup"><span data-stu-id="98cd8-983">An extension method is a regular static method.</span></span> <span data-ttu-id="98cd8-984">Además, cuando su clase estática envolvente en el ámbito, un método de extensión se puede invocar mediante sintaxis de invocación de método de instancia ([las invocaciones de método de extensión](expressions.md#extension-method-invocations)), mediante la expresión del receptor como primer argumento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="98cd8-985">El siguiente programa utiliza los métodos de extensión declarados anteriormente:</span><span class="sxs-lookup"><span data-stu-id="98cd8-985">The following program uses the extension methods declared above:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

<span data-ttu-id="98cd8-986">El `Slice` método está disponible en el `string[]`y el `ToInt32` método está disponible en `string`, porque se ha declarado como métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="98cd8-987">El significado del programa es igual que las llamadas de método estático normal siguiente, mediante:</span><span class="sxs-lookup"><span data-stu-id="98cd8-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a><span data-ttu-id="98cd8-988">Cuerpo del método</span><span class="sxs-lookup"><span data-stu-id="98cd8-988">Method body</span></span>

<span data-ttu-id="98cd8-989">El *cuerpoMétodo* de un método de declaración consta de un cuerpo de bloques, un cuerpo de expresión o un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="98cd8-990">El ***tipo de resultado*** de un método es `void` si el tipo de valor devuelto es `void`, o si el método es asincrónico y el tipo de valor devuelto es `System.Threading.Tasks.Task`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="98cd8-991">En caso contrario, el tipo de resultado de un método que no sea asincrónico es su tipo de valor devuelto y el tipo de resultado con tipo de valor devuelto de un método asincrónico `System.Threading.Tasks.Task<T>` es `T`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="98cd8-992">Cuando un método que tiene un `void` como resultado de tipo y un cuerpo de bloques, `return` instrucciones ([la instrucción return](statements.md#the-return-statement)) no se permiten en el bloque al especificar una expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="98cd8-993">Si la ejecución del bloque de un método void termina normalmente (es decir, el control llega al final del cuerpo del método), método regresa a su llamador actual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="98cd8-994">Cuando un método que tiene un `void` resultado y un cuerpo de expresión, la expresión `E` debe ser un *statement_expression*, y el cuerpo es equivalente exactamente a un cuerpo de bloques del formulario `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="98cd8-995">Cuando un método tiene un tipo de resultado distinto de void y un bloque de cuerpo, cada uno de ellos `return` instrucción del bloque debe especificar una expresión que es implícitamente convertible al tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="98cd8-996">El punto de conexión de un cuerpo de bloques de un método devuelve un valor no debe ser accesible.</span><span class="sxs-lookup"><span data-stu-id="98cd8-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="98cd8-997">En otras palabras, en un método devuelve un valor con un cuerpo de bloques, no se permite que el control al alcanzar el final del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="98cd8-998">Cuando un método que tiene un tipo de resultado distinto de void y un cuerpo de expresión, la expresión debe poder convertirse implícitamente al tipo de resultado y el cuerpo equivale exactamente a un cuerpo de bloques del formulario `{ return E; }`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="98cd8-999">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-999">In the example</span></span>
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
<span data-ttu-id="98cd8-1000">el valor de devolución `F` método produce un error de tiempo de compilación porque el control puede alcanzar el final del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="98cd8-1001">El `G` y `H` métodos son correctos, ya que todas las rutas de ejecución posibles terminan en una instrucción return que especifica un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="98cd8-1002">El `I` método es correcto, porque el cuerpo es equivalente a un bloque de instrucciones con una sola instrucción return en ella.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="98cd8-1003">Sobrecarga de métodos</span><span class="sxs-lookup"><span data-stu-id="98cd8-1003">Method overloading</span></span>

<span data-ttu-id="98cd8-1004">Se describen las reglas de resolución de sobrecarga del método en [inferencia de tipo](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="98cd8-1005">Propiedades</span><span class="sxs-lookup"><span data-stu-id="98cd8-1005">Properties</span></span>

<span data-ttu-id="98cd8-1006">Un ***propiedad*** es un miembro que proporciona acceso a una característica de un objeto o una clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="98cd8-1007">Ejemplos de propiedades incluyen la longitud de una cadena, el tamaño de una fuente, el título de una ventana, el nombre de un cliente y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="98cd8-1008">Las propiedades son una extensión natural de los campos, ambos son miembros denominados con tipos asociados, y la sintaxis para tener acceso a los campos y propiedades es la misma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="98cd8-1009">Sin embargo, a diferencia de los campos, las propiedades no denotan ubicaciones de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="98cd8-1010">Las propiedades tienen ***descriptores de acceso*** que especifican las instrucciones que se ejecutan cuando se leen o escriben sus valores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="98cd8-1011">Las propiedades proporcionan un mecanismo para asociar las acciones con la lectura y escritura de los atributos del objeto; Además, permiten esos atributos que se va a calcular.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="98cd8-1012">Las propiedades se declaran mediante *property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1012">Properties are declared using *property_declaration*s:</span></span>

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

<span data-ttu-id="98cd8-1013">Un *property_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso ](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` ([Invalidar métodos](classes.md#override-methods)), `sealed` ([sellado métodos](classes.md#sealed-methods)), `abstract` ([métodos abstractos](classes.md#abstract-methods)), y `extern` ([Métodos externos](classes.md#external-methods)) modificadores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="98cd8-1014">Las declaraciones de propiedad están sujetos a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="98cd8-1015">El *tipo* de una propiedad de la declaración especifica el tipo de la propiedad que se incluyen en la declaración y el *member_name* especifica el nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="98cd8-1016">A menos que la propiedad es una implementación de miembro de interfaz explícita, el *member_name* es simplemente un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="98cd8-1017">Para obtener una implementación de miembro de interfaz explícita ([implementaciones de miembros de interfaz explícita](interfaces.md#explicit-interface-member-implementations)), el *member_name* consta de un *interface_type* seguido de un " `.`"y un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="98cd8-1018">El *tipo* de una propiedad debe ser al menos tan accesibles como la propiedad propiamente dicha ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="98cd8-1019">Un *property_body* puede constar de un ***cuerpo del descriptor de acceso*** o un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="98cd8-1020">En el cuerpo de un descriptor de acceso, *accessor_declarations*, que debe incluirse en "`{`"y"`}`" tokens, declare los descriptores de acceso ([descriptores de acceso](classes.md#accessors)) de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="98cd8-1021">Los descriptores de acceso especifican las instrucciones ejecutables asociadas con la lectura y escritura de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="98cd8-1022">Un cuerpo de expresión formada por `=>` seguido por un *expresión* `E` y un punto y coma es igual que en el cuerpo de instrucción `{ get { return E; } }`y por lo tanto, sólo puede utilizarse para especificar solo captador propiedades donde se indica el resultado de Get por una sola expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="98cd8-1023">Un *property_initializer* sólo puede asignarse una propiedad implementada automáticamente ([implementa automáticamente propiedades](classes.md#automatically-implemented-properties)) y hace que la inicialización del campo subyacente de estos las propiedades con el valor especificado por el *expresión*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="98cd8-1024">Aunque la sintaxis para tener acceso a una propiedad es el mismo que para un campo, una propiedad no está clasificada como una variable.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="98cd8-1025">Por lo tanto, no es posible pasar una propiedad como un `ref` o `out` argumento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="98cd8-1026">Cuando una declaración de propiedad incluye un `extern` modificador, la propiedad se dice que un ***propiedad externa***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="98cd8-1027">Dado que una declaración de propiedad externa no proporciona ninguna implementación real, cada uno de sus *accessor_declarations* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="98cd8-1028">Propiedades de instancia y estáticos</span><span class="sxs-lookup"><span data-stu-id="98cd8-1028">Static and instance properties</span></span>

<span data-ttu-id="98cd8-1029">Cuando una declaración de propiedad incluye un `static` modificador, la propiedad se dice que un ***propiedad estática***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="98cd8-1030">Cuando no hay ninguna `static` modificador está presente, la propiedad se dice que un ***propiedad de instancia***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="98cd8-1031">Una propiedad estática no está asociada con una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en los descriptores de acceso de una propiedad estática.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="98cd8-1032">Una propiedad de instancia está asociada con una instancia determinada de una clase, y puede tener acceso a esa instancia como `this` ([este acceso](expressions.md#this-access)) en los descriptores de acceso de esa propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="98cd8-1033">Cuando se hace referencia a una propiedad en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`si `M` es una propiedad estática, `E` debe denotar un tipo que contiene `M`y si `M` es una propiedad de instancia, E debe denotar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="98cd8-1034">Las diferencias entre estático y los miembros de instancia se tratan con más detalle en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="98cd8-1035">Descriptores de acceso</span><span class="sxs-lookup"><span data-stu-id="98cd8-1035">Accessors</span></span>

<span data-ttu-id="98cd8-1036">El *accessor_declarations* de una propiedad, especifique las instrucciones ejecutables asociadas con la lectura y escritura de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="98cd8-1037">Las declaraciones de descriptor de acceso que constan de un *get_accessor_declaration*, un *set_accessor_declaration*, o ambos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="98cd8-1038">Cada declaración de descriptor de acceso está formado por el token `get` o `set` seguido de un elemento opcional *accessor_modifier* y un *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="98cd8-1039">El uso de *accessor_modifier*s se rige por las siguientes restricciones:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="98cd8-1040">Un *accessor_modifier* no puede usarse en una interfaz o en una implementación de miembro de interfaz explícita.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="98cd8-1041">Para una propiedad o indizador que no tiene ningún `override` modificador, un *accessor_modifier* sólo está permitido si la propiedad o indizador tiene tanto un `get` y `set` descriptor de acceso y, a continuación, sólo se permite en una de ellas descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="98cd8-1042">Para una propiedad o indizador que incluye un `override` modificador, un descriptor de acceso debe coincidir con el *accessor_modifier*, si lo hay, del descriptor de acceso que se va a invalidar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="98cd8-1043">El *accessor_modifier* debe declarar una accesibilidad que es estrictamente más restrictiva que la accesibilidad declarada de la propiedad o el propio indexador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="98cd8-1044">Para ser precisos:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1044">To be precise:</span></span>
   * <span data-ttu-id="98cd8-1045">Si la propiedad o indizador tiene una accesibilidad declarada de `public`, *accessor_modifier* pueden ser `protected internal`, `internal`, `protected`, o `private`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="98cd8-1046">Si la propiedad o indizador tiene una accesibilidad declarada de `protected internal`, *accessor_modifier* pueden ser `internal`, `protected`, o `private`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="98cd8-1047">Si la propiedad o indizador tiene una accesibilidad declarada de `internal` o `protected`, *accessor_modifier* debe ser `private`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="98cd8-1048">Si la propiedad o indizador tiene una accesibilidad declarada de `private`, no *accessor_modifier* puede utilizarse.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="98cd8-1049">Para `abstract` y `extern` propiedades, el *accessor_body* para cada descriptor de acceso especificado es simplemente un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="98cd8-1050">Puede tener una propiedad no abstracta, no extern cada *accessor_body* sea un punto y coma, en cuyo caso es un ***propiedad implementada automáticamente*** ([implementa automáticamente propiedades ](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="98cd8-1051">Una propiedad implementada automáticamente debe tener al menos un descriptor de acceso get.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="98cd8-1052">Para los descriptores de acceso de otra propiedad no abstracta, no externos, el *accessor_body* es un *bloque* que especifica las instrucciones que se ejecuta cuando se invoca el descriptor de acceso correspondiente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="98cd8-1053">Un `get` descriptor de acceso corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="98cd8-1054">Excepto como destino de una asignación, cuando se hace referencia a una propiedad en una expresión, el `get` descriptor de acceso de la propiedad se invoca para calcular el valor de la propiedad ([valores de expresiones](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="98cd8-1055">El cuerpo de un `get` descriptor de acceso debe ajustarse a las reglas de devolución por valor métodos descritos en [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="98cd8-1056">En concreto, todos los `return` instrucciones en el cuerpo de un `get` descriptor de acceso debe especificar una expresión que es implícitamente convertible al tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="98cd8-1057">Además, el punto de conexión de un `get` descriptor de acceso no debe ser accesible.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="98cd8-1058">Un `set` descriptor de acceso corresponde a un método con un parámetro de valor único del tipo de propiedad y un `void` tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="98cd8-1059">El parámetro implícito de un `set` descriptor de acceso siempre se denomina `value`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="98cd8-1060">Cuando se hace referencia a una propiedad como el destino de una asignación ([operadores de asignación](expressions.md#assignment-operators)), o como el operando de `++` o `--` ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators), [ Prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)), el `set` descriptor de acceso se invoca con un argumento (cuyo valor es el de la derecha de la asignación o el operando de la `++` o `--` operador) que proporciona el nuevo valor ([asignación Simple](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="98cd8-1061">El cuerpo de un `set` descriptor de acceso debe cumplir las reglas de `void` métodos descritos en [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="98cd8-1062">En concreto, `return` instrucciones en el `set` cuerpo del descriptor de acceso no se permiten especificar una expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="98cd8-1063">Puesto que un `set` descriptor de acceso tiene implícitamente un parámetro denominado `value`, es un error de tiempo de compilación de una declaración de variable o una constante local en un `set` descriptor de acceso de ese nombre.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="98cd8-1064">Según la presencia o ausencia de la `get` y `set` descriptores de acceso, una propiedad se clasifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="98cd8-1065">Una propiedad que incluye tanto una `get` descriptor de acceso y un `set` descriptor de acceso se dice que un ***lectura-escritura*** propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="98cd8-1066">Una propiedad que tiene solo un `get` descriptor de acceso se dice que un ***de sólo lectura*** propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="98cd8-1067">Es un error en tiempo de compilación para una propiedad de solo lectura a ser el destino de una asignación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="98cd8-1068">Una propiedad que tiene solo un `set` descriptor de acceso se dice que un ***de solo escritura*** propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="98cd8-1069">Excepto como destino de una asignación, es un error en tiempo de compilación para hacer referencia a una propiedad de solo escritura en una expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="98cd8-1070">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1070">In the example</span></span>
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
<span data-ttu-id="98cd8-1071">el `Button` control declara una pública `Caption` propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="98cd8-1072">El `get` descriptor de acceso de la `Caption` propiedad devuelve la cadena almacenada en privado `caption` campo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="98cd8-1073">El `set` comprueba si el nuevo valor es diferente del valor actual; en caso afirmativo, almacena el nuevo valor de descriptor de acceso y vuelve a dibujar el control.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="98cd8-1074">Las propiedades a menudo siguen el patrón anterior: El `get` descriptor de acceso simplemente devuelve un valor almacenado en un campo privado y el `set` modifica el campo privado de descriptor de acceso y, a continuación, realiza acciones adicionales necesarias para actualizar completamente el estado del objeto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="98cd8-1075">Dada la `Button` clase anterior, el siguiente es un ejemplo de uso de la `Caption` propiedad:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="98cd8-1076">En este caso, el `set` se invoca el descriptor de acceso asignando un valor a la propiedad y el `get` descriptor de acceso se invoca haciendo referencia a la propiedad en una expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="98cd8-1077">El `get` y `set` descriptores de acceso de una propiedad no son miembros distintos, y no es posible declarar los descriptores de acceso de una propiedad por separado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="98cd8-1078">Por lo tanto, no es posible que los dos descriptores de acceso de una propiedad de lectura y escritura tener una accesibilidad diferente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="98cd8-1079">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1079">The example</span></span>
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
<span data-ttu-id="98cd8-1080">no se declara una sola propiedad de lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="98cd8-1081">En su lugar, declaran dos propiedades con el mismo nombre, una de sólo lectura y otra de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="98cd8-1082">Dado que dos miembros declarados en la misma clase no pueden tener el mismo nombre, el ejemplo hace que se produzca un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="98cd8-1083">Cuando una clase derivada declara una propiedad con el mismo nombre que una propiedad heredada, la propiedad derivada oculta la propiedad heredada con respecto a las operaciones de lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="98cd8-1084">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1084">In the example</span></span>
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
<span data-ttu-id="98cd8-1085">el `P` propiedad `B` oculta la `P` propiedad `A` con respecto a las operaciones de lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="98cd8-1086">Por lo tanto, en las instrucciones</span><span class="sxs-lookup"><span data-stu-id="98cd8-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="98cd8-1087">la asignación a `b.P` provoca un error de tiempo de compilación notificará desde sólo lectura `P` propiedad `B` oculta solo escritura `P` propiedad en `A`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="98cd8-1088">Sin embargo, tenga en cuenta que puede usarse una conversión para tener acceso a la oculta `P` propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="98cd8-1089">A diferencia de los campos públicos, las propiedades proporcionan una separación entre el estado interno de un objeto y su interfaz pública.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="98cd8-1090">Considere el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1090">Consider the example:</span></span>
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="98cd8-1091">En este caso, el `Label` clase utiliza dos `int` campos, `x` y `y`, para almacenar su ubicación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="98cd8-1092">La ubicación es públicamente expuestos como un `X` y un `Y` propiedad y, como un `Location` propiedad de tipo `Point`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="98cd8-1093">Si se encuentra, en una versión futura de `Label`, resulta más conveniente almacenar la ubicación como una `Point` internamente, el cambio se puede realizar sin que afecte a la interfaz pública de la clase:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

<span data-ttu-id="98cd8-1094">Tenía `x` y `y` en su lugar se ha sido `public readonly` campos, habría sido imposible realizar este cambio a la `Label` clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="98cd8-1095">Expone el estado a través de propiedades no es necesariamente menos eficiente que exponer directamente los campos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="98cd8-1096">En concreto, cuando una propiedad no es virtual y contiene sólo una pequeña cantidad de código, el entorno de ejecución puede reemplazar las llamadas a los descriptores de acceso con el código real de los descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="98cd8-1097">Este proceso se conoce como ***inserción***, y facilita el acceso a la propiedad tan eficaz como el acceso a campos al mismo, conserva la mayor flexibilidad de las propiedades.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="98cd8-1098">Desde la invocación de un `get` es conceptualmente equivalente a leer el valor de un campo de descriptor de acceso, se considera mal estilo de programación para `get` descriptores de acceso tienen efectos observables.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="98cd8-1099">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="98cd8-1100">el valor de la `Next` propiedad depende del número de veces que previamente ha accedido a la propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="98cd8-1101">Por lo tanto, el acceso a la propiedad produce un efecto secundario observable y la propiedad debe implementarse como un método en su lugar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="98cd8-1102">La convención "sin efectos secundarios" para `get` descriptores de acceso no significa que `get` siempre se deben escribir los descriptores de acceso para devolver los valores almacenados en campos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="98cd8-1103">De hecho, `get` descriptores de acceso de proceso a menudo el valor de una propiedad mediante el acceso a varios campos o invocar métodos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="98cd8-1104">Sin embargo, diseñado correctamente `get` descriptor de acceso realiza ninguna acción que provocan cambios observables en el estado del objeto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="98cd8-1105">Las propiedades se pueden usar para retrasar la inicialización de un recurso hasta el momento en que se hace referencia en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="98cd8-1106">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1106">For example:</span></span>
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

<span data-ttu-id="98cd8-1107">El `Console` clase contiene tres propiedades `In`, `Out`, y `Error`, que representan la entrada estándar, salida y error de dispositivos, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="98cd8-1108">Mediante la exposición de estos miembros como propiedades, la `Console` clase puede retrasar su inicialización hasta que se usan realmente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="98cd8-1109">Por ejemplo, en la primera referencia a la `Out` propiedad, como en</span><span class="sxs-lookup"><span data-stu-id="98cd8-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="98cd8-1110">subyacente `TextWriter` para crear el dispositivo de salida.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="98cd8-1111">Sin embargo, si la aplicación no hace referencia a la `In` y `Error` propiedades y, a continuación, no hay ningún objeto que se crea para esos dispositivos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="98cd8-1112">Propiedades implementadas automáticamente</span><span class="sxs-lookup"><span data-stu-id="98cd8-1112">Automatically implemented properties</span></span>

<span data-ttu-id="98cd8-1113">Una propiedad implementada automáticamente (o ***autopropiedad*** para abreviar), es una propiedad de no abstracta no extern con cuerpos de descriptor de acceso de sólo punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="98cd8-1114">Propiedades automáticas deben tener un descriptor de acceso get y, opcionalmente, pueden tener un descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="98cd8-1115">Cuando se especifica una propiedad como una propiedad implementada automáticamente, un campo de respaldo oculto está automáticamente disponible para la propiedad y se implementan los descriptores de acceso para leer y escribir en ese campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="98cd8-1116">Si la propiedad automática no tiene ningún descriptor de acceso set, el campo de respaldo se considera `readonly` ([campos de solo lectura](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="98cd8-1117">Al igual que un `readonly` campo, una propiedad automática solo captador puede asignarse a en el cuerpo de un constructor de la clase envolvente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="98cd8-1118">Esta asignación se asigna directamente en el campo de respaldo de solo lectura de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="98cd8-1119">Una propiedad automática podría tener opcionalmente un *property_initializer*, que se aplica directamente en el campo de respaldo como un *variable_initializer* ([inicializadores de variables](classes.md#variable-initializers)) .</span><span class="sxs-lookup"><span data-stu-id="98cd8-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="98cd8-1120">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="98cd8-1121">es equivalente a la siguiente declaración:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="98cd8-1122">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="98cd8-1123">es equivalente a la siguiente declaración:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1123">is equivalent to the following declaration:</span></span>
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

<span data-ttu-id="98cd8-1124">Tenga en cuenta que las asignaciones para el campo de solo lectura son válidas, porque se producen dentro del constructor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="98cd8-1125">Accesibilidad</span><span class="sxs-lookup"><span data-stu-id="98cd8-1125">Accessibility</span></span>

<span data-ttu-id="98cd8-1126">Si tiene un descriptor de acceso una *accessor_modifier*, el dominio de accesibilidad ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)) del descriptor de acceso se determina mediante la accesibilidad declarada de la *accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="98cd8-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="98cd8-1127">Si no tiene un descriptor de acceso una *accessor_modifier*, el dominio de accesibilidad del descriptor de acceso viene determinada por la accesibilidad declarada de la propiedad o indizador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="98cd8-1128">La presencia de un *accessor_modifier* nunca afecta a la búsqueda de miembros ([operadores](expressions.md#operators)) o de resolución de sobrecarga ([de resolución de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="98cd8-1129">Los modificadores de la propiedad o indizador siempre determinan qué propiedad o indizador está enlazado, independientemente del contexto del acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="98cd8-1130">Una vez que se ha seleccionado una propiedad o indizador concreto, los dominios de accesibilidad de los descriptores de acceso específicos implicados se utilizan para determinar si el uso es válido:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="98cd8-1131">Si el uso es como un valor ([valores de expresiones](expressions.md#values-of-expressions)), el `get` descriptor de acceso debe existir y ser accesible.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="98cd8-1132">Si el uso es como el destino de una asignación simple ([asignación Simple](expressions.md#simple-assignment)), el `set` descriptor de acceso debe existir y ser accesible.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="98cd8-1133">Si el uso es como el destino de asignación compuesta ([asignación compuesta](expressions.md#compound-assignment)), o como destino de la `++` o `--` operadores ([miembros de función](expressions.md#function-members).9, [ Expresiones de invocación](expressions.md#invocation-expressions)), tanto el `get` descriptores de acceso y el `set` descriptor de acceso debe existir y ser accesible.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="98cd8-1134">En el ejemplo siguiente, la propiedad `A.Text` está oculto por la propiedad `B.Text`, incluso en contextos en los que sólo el `set` se denomina descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="98cd8-1135">En cambio, la propiedad `B.Count` no es accesible para la clase `M`, por lo que la propiedad accesible `A.Count` se usa en su lugar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

<span data-ttu-id="98cd8-1136">Un descriptor de acceso que se usa para implementar una interfaz no puede tener un *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="98cd8-1137">Si solo un descriptor de acceso se utiliza para implementar una interfaz, el otro descriptor de acceso se puede declarar con un *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="98cd8-1138">Virtual, sellado, de reemplazo y descriptores de acceso de propiedad abstracta</span><span class="sxs-lookup"><span data-stu-id="98cd8-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="98cd8-1139">Un `virtual` declaración de propiedad especifica que los descriptores de acceso de la propiedad son virtuales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="98cd8-1140">El `virtual` modificador se aplica a ambos descriptores de acceso de una propiedad de lectura y escritura, no es posible que solo un descriptor de acceso de una propiedad de lectura y escritura sea virtual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="98cd8-1141">Un `abstract` declaración de propiedad especifica que los descriptores de acceso de la propiedad son virtuales, pero no proporciona una implementación real de los descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="98cd8-1142">En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación para los descriptores de acceso mediante la invalidación de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="98cd8-1143">Dado que un descriptor de acceso para una declaración de propiedad abstracta no proporciona ninguna implementación real, su *accessor_body* consiste simplemente en un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="98cd8-1144">Una declaración de propiedad que incluya tanto el `abstract` y `override` modificadores especifica que la propiedad es abstracta y reemplaza una propiedad de base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="98cd8-1145">Los descriptores de acceso de esa propiedad también son abstractas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="98cd8-1146">Solo se permiten declaraciones de propiedad abstracta en clases abstractas ([clases abstractas](classes.md#abstract-classes)). Los descriptores de acceso de una propiedad virtual heredada se pueden invalidar en una clase derivada incluyendo una declaración de propiedad que especifica un `override` directiva.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="98cd8-1147">Esto se conoce como un ***reemplazar la declaración de propiedad***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="98cd8-1148">Una declaración de propiedad reemplazada no declara una nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="98cd8-1149">En su lugar, simplemente especializa las implementaciones de los descriptores de acceso de una propiedad virtual existente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="98cd8-1150">Una declaración de propiedad reemplazada debe especificar los mismos modificadores de accesibilidad exacto, tipo y nombre que la propiedad heredada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="98cd8-1151">Si la propiedad heredada tiene solo un descriptor de acceso (por ejemplo, si la propiedad heredada es de solo lectura o solo escritura), la propiedad de reemplazo debe incluir sólo ese descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="98cd8-1152">Si la propiedad heredada incluye ambos descriptores de acceso (es decir, si la propiedad heredada es de lectura y escritura), la propiedad de reemplazo puede incluir un descriptor de acceso o ambos descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="98cd8-1153">Puede incluir una declaración de propiedad reemplazada la `sealed` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="98cd8-1154">Uso de este modificador impide que una clase derivada siga reemplazando la propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="98cd8-1155">Los descriptores de acceso de una propiedad sellada también están sellados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="98cd8-1156">Salvo por diferencias en la declaración e invocación sintaxis, virtual, sellado, de reemplazo y abstractos descriptores de acceso se comportan exactamente igual virtual, sellado, de reemplazo y métodos abstractos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="98cd8-1157">En concreto, las reglas se describen en [métodos virtuales](classes.md#virtual-methods), [invalidar métodos](classes.md#override-methods), [sellado métodos](classes.md#sealed-methods), y [métodos abstractos](classes.md#abstract-methods) aplicar como si los descriptores de acceso fueran métodos de la forma correspondiente:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="98cd8-1158">Un `get` descriptor de acceso corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad y los mismos modificadores que la propiedad contenedora.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="98cd8-1159">Un `set` descriptor de acceso corresponde a un método con un parámetro de valor único del tipo de propiedad, un `void` devolver el tipo y los mismos modificadores que la propiedad contenedora.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="98cd8-1160">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1160">In the example</span></span>
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
<span data-ttu-id="98cd8-1161">`X` es una propiedad de solo lectura virtual `Y` es una propiedad de lectura y escritura virtual, y `Z` es una propiedad abstracta de lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="98cd8-1162">Dado que `Z` es abstracto, la clase contenedora `A` debe declararse como abstracta.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="98cd8-1163">Una clase que derive de `A` es muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1163">A class that derives from `A` is show below:</span></span>
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

<span data-ttu-id="98cd8-1164">Aquí, las declaraciones de `X`, `Y`, y `Z` invalida las declaraciones de propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="98cd8-1165">Cada declaración de propiedad coincide exactamente con los modificadores de accesibilidad, tipo y nombre de la propiedad heredada correspondiente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="98cd8-1166">El `get` descriptor de acceso de `X` y `set` descriptor de acceso de `Y` utilizar el `base` palabra clave para tener acceso a los descriptores de acceso heredadas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="98cd8-1167">La declaración de `Z` reemplaza ambos descriptores de acceso, por lo tanto, no hay ningún miembro de función abstracta pendientes en `B`, y `B` puede ser una clase no abstracta.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="98cd8-1168">Cuando se declara una propiedad como un `override`, los descriptores de acceso invalidados deben ser accesibles para el código de invalidación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="98cd8-1169">Además, la accesibilidad declarada de la propiedad o el indexador y de los descriptores de acceso, debe coincidir con el miembro reemplazado y descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="98cd8-1170">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1170">For example:</span></span>
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a><span data-ttu-id="98cd8-1171">Eventos</span><span class="sxs-lookup"><span data-stu-id="98cd8-1171">Events</span></span>

<span data-ttu-id="98cd8-1172">Un ***eventos*** es un miembro que permite a un objeto o clase para proporcionar notificaciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="98cd8-1173">Los clientes pueden adjuntar código ejecutable para eventos proporcionando ***controladores de eventos***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="98cd8-1174">Los eventos se declaran mediante *event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1174">Events are declared using *event_declaration*s:</span></span>

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

<span data-ttu-id="98cd8-1175">Un *event_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso ](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` ([Invalidar métodos](classes.md#override-methods)), `sealed` ([sellado métodos](classes.md#sealed-methods)), `abstract` ([métodos abstractos](classes.md#abstract-methods)), y `extern` ([Métodos externos](classes.md#external-methods)) modificadores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="98cd8-1176">Declaraciones de eventos están sujetos a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="98cd8-1177">El *tipo* de un evento debe especificarse un *delegate_type* ([hacen referencia a tipos](types.md#reference-types)) y que *delegate_type* debe ser al menos como accesible como el propio evento ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="98cd8-1178">Puede incluir una declaración de evento *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="98cd8-1179">Sin embargo, si no es así, para que no son externos y no abstractas eventos, el compilador proporciona automáticamente ([eventos como campos](classes.md#field-like-events)); para los eventos externos, los descriptores de acceso se proporcionan externamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="98cd8-1180">Una declaración de evento que omite *event_accessor_declarations* define uno o varios eventos: uno para cada uno de los *variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="98cd8-1181">Los atributos y modificadores se aplican a todos los miembros que se declara como un *event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="98cd8-1182">Es un error en tiempo de compilación para un *event_declaration* para incluir tanto la `abstract` modificador y delimitado por llaves *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="98cd8-1183">Cuando una declaración de evento incluye un `extern` modificador, el evento se dice que un ***evento externo***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="98cd8-1184">Dado que una declaración de evento externo no proporciona ninguna implementación real, es un error para que incluya tanto el `extern` modificador y *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="98cd8-1185">Es un error en tiempo de compilación para un *variable_declarator* de una declaración de evento con un `abstract` o `external` modificador debe incluir un *variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="98cd8-1186">Un evento puede utilizarse como operando izquierdo de la `+=` y `-=` operadores ([asignación de evento](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="98cd8-1187">Estos operadores se utilizan, respectivamente, para adjuntar controladores de eventos a o quitar controladores de eventos de un evento, y los modificadores de acceso del evento controlan los contextos en los que se permiten estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="98cd8-1188">Puesto que `+=` y `-=` son las únicas operaciones permitidas en un evento fuera del tipo que declara el evento, el código externo pueden agregar y quitar controladores de evento, pero no en ninguna otra forma de obtener o modificar la lista subyacente del evento controladores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="98cd8-1189">En una operación del formulario `x += y` o `x -= y`, cuando `x` es un evento y la referencia tiene lugar fuera del tipo que contiene la declaración de `x`, el resultado de la operación tiene el tipo `void` (en lugar de tener el tipo de `x`, con el valor de `x` después de la asignación).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="98cd8-1190">Esta regla evita que código externo examine indirectamente el delegado subyacente de un evento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="98cd8-1191">El ejemplo siguiente muestra cómo se adjuntan los controladores de eventos a instancias de la `Button` clase:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

<span data-ttu-id="98cd8-1192">En este caso, el `LoginDialog` constructor de instancia, crea dos `Button` instancias y adjunta los controladores de eventos para el `Click` eventos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="98cd8-1193">Eventos como si fueran campos</span><span class="sxs-lookup"><span data-stu-id="98cd8-1193">Field-like events</span></span>

<span data-ttu-id="98cd8-1194">En el texto de programa de la clase o estructura que contiene la declaración de un evento, ciertos eventos pueden usarse como campos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="98cd8-1195">Para su uso en este modo, no debe ser un evento `abstract` o `extern`y no debe incluir explícitamente *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="98cd8-1196">Este evento puede utilizarse en cualquier contexto que permite que un campo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="98cd8-1197">El campo contiene un delegado ([delegados](delegates.md)) que hace referencia a la lista de controladores de eventos que se han agregado al evento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="98cd8-1198">Si no hay controladores de eventos se han agregado, el campo contiene `null`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="98cd8-1199">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1199">In the example</span></span>
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
<span data-ttu-id="98cd8-1200">`Click` se utiliza como un campo dentro de la `Button` clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="98cd8-1201">Como se muestra en el ejemplo, el campo puede examinar, modificar y utilizado en expresiones de invocación del delegado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="98cd8-1202">El `OnClick` método en el `Button` clase "genera" el `Click` eventos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="98cd8-1203">La noción de generar un evento es equivalente exactamente a invocar el delegado representado por el evento; por lo tanto, no hay ninguna construcción especial de lenguaje para generar eventos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="98cd8-1204">Tenga en cuenta que la invocación del delegado está precedida por una comprobación que garantiza que el delegado no es null.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="98cd8-1205">Fuera de la declaración de la `Button` (clase), el `Click` miembro solo puede usarse en el lado izquierdo de la `+=` y `-=` operadores, como en</span><span class="sxs-lookup"><span data-stu-id="98cd8-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="98cd8-1206">que anexa un delegado a la lista de invocación de la `Click` eventos, y</span><span class="sxs-lookup"><span data-stu-id="98cd8-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="98cd8-1207">que quita un delegado de la lista de invocaciones del `Click` eventos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="98cd8-1208">Cuando se compila un campo como un evento, el compilador crea el almacenamiento para almacenar al delegado y crea los descriptores de acceso para el evento que agregan o quitan controladores de eventos para el campo de delegado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="98cd8-1209">Las operaciones de adición y eliminación son subprocesos y puede (pero no es necesarias) se haya hecho mientras mantiene el bloqueo ([la instrucción lock](statements.md#the-lock-statement)) en el objeto contenedor para un evento de instancia o el objeto de tipo ([anónimo expresiones de creación de objetos](expressions.md#anonymous-object-creation-expressions)) para un evento estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="98cd8-1210">Por lo tanto, una instancia declaración de evento del formulario:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="98cd8-1211">se compilará en un valor equivalente al:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1211">will be compiled to something equivalent to:</span></span>
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
<span data-ttu-id="98cd8-1212">Dentro de la clase `X`, las referencias a `Ev` en el lado izquierdo de la `+=` y `-=` operadores ocasionar el agregar y quitar los descriptores de acceso que se debe invocar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="98cd8-1213">Todas las referencias a `Ev` se compilan para hacer referencia al campo oculto `__Ev` en su lugar ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="98cd8-1214">El nombre "`__Ev`" es arbitrario; el campo oculto no puede tener cualquier nombre o en absoluto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="98cd8-1215">Descriptores de acceso de un evento</span><span class="sxs-lookup"><span data-stu-id="98cd8-1215">Event accessors</span></span>

<span data-ttu-id="98cd8-1216">Declaraciones de eventos normalmente se omiten *event_accessor_declarations*, como en el `Button` ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="98cd8-1217">Una situación para hacerlo es en el caso en que el costo de almacenamiento de un campo por el evento no es aceptable.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="98cd8-1218">En tales casos, puede incluir una clase *event_accessor_declarations* y utilizar un mecanismo privado para almacenar la lista de controladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="98cd8-1219">El *event_accessor_declarations* de un evento, especifique las instrucciones ejecutables asociadas con la adición y eliminación de controladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="98cd8-1220">Las declaraciones de descriptor de acceso que constan de un *add_accessor_declaration* y un *remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="98cd8-1221">Cada declaración de descriptor de acceso está formado por el token `add` o `remove` seguido por un *bloque*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="98cd8-1222">El *bloque* asociado con un *add_accessor_declaration* especifica las instrucciones que se ejecutará cuando se agrega un controlador de eventos y el *bloque* asociado con un *remove_accessor_declaration* especifica las instrucciones que se ejecutará cuando se quita un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="98cd8-1223">Cada *add_accessor_declaration* y *remove_accessor_declaration* corresponde a un método con un parámetro de valor único del tipo de evento y un `void` tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="98cd8-1224">El parámetro implícito de un descriptor de acceso de eventos se denomina `value`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="98cd8-1225">Cuando se usa un evento en una asignación de evento, se utiliza el descriptor de acceso de eventos adecuado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="98cd8-1226">En concreto, si el operador de asignación es `+=` , a continuación, se utiliza el descriptor de acceso add y si el operador de asignación es `-=` , a continuación, se utiliza el descriptor de acceso remove.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="98cd8-1227">En cualquier caso, el operando derecho del operador de asignación se usa como argumento para el descriptor de acceso de eventos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="98cd8-1228">El bloque de un *add_accessor_declaration* o un *remove_accessor_declaration* debe cumplir las reglas de `void` métodos descritos en [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="98cd8-1229">En concreto, `return` no se permiten las instrucciones de dicho bloque al especificar una expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="98cd8-1230">Puesto que un descriptor de acceso de eventos tiene implícitamente un parámetro denominado `value`, es un error en tiempo de compilación para una variable o constante local declarados en un descriptor de acceso de eventos para que ese nombre.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="98cd8-1231">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1231">In the example</span></span>
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
<span data-ttu-id="98cd8-1232">la `Control` clase implementa un mecanismo de almacenamiento interno para los eventos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="98cd8-1233">El `AddEventHandler` método asocia un valor de delegado con una clave, el `GetEventHandler` método devuelve el delegado asociado actualmente a una clave y el `RemoveEventHandler` método quita un delegado como un controlador de eventos para el evento especificado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="98cd8-1234">Presumiblemente, el mecanismo de almacenamiento subyacente está diseñado, como que no hay ningún costo para asociar un `null` delegado de valor con una clave y, por tanto, los eventos no controlados no consumen almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="98cd8-1235">Eventos estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="98cd8-1235">Static and instance events</span></span>

<span data-ttu-id="98cd8-1236">Cuando una declaración de evento incluye un `static` modificador, el evento se dice que un ***evento estático***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="98cd8-1237">Cuando no hay ninguna `static` modificador está presente, el evento se dice que un ***eventos de instancia***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="98cd8-1238">Un evento estático no está asociado a una instancia concreta, y es un error en tiempo de compilación para hacer referencia a `this` en los descriptores de acceso de un evento estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="98cd8-1239">Un evento de instancia está asociado con una instancia determinada de una clase, y puede tener acceso a esta instancia como `this` ([este acceso](expressions.md#this-access)) en los descriptores de acceso de ese evento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="98cd8-1240">Cuando se hace referencia a un evento en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`si `M` es un evento estático, `E` debe denotar un tipo que contiene `M`y si `M` es un evento de instancia, E debe denotar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="98cd8-1241">Las diferencias entre estático y los miembros de instancia se tratan con más detalle en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="98cd8-1242">Virtual, sellado, de reemplazo y descriptores de acceso de un evento abstracto</span><span class="sxs-lookup"><span data-stu-id="98cd8-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="98cd8-1243">Un `virtual` declaración de evento especifica que los descriptores de acceso de ese evento son virtuales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="98cd8-1244">El `virtual` modificador se aplica a ambos descriptores de acceso de un evento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="98cd8-1245">Un `abstract` declaración de evento especifica que los descriptores de acceso del evento son virtuales, pero no proporciona una implementación real de los descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="98cd8-1246">En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación para los descriptores de acceso invalidando el evento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="98cd8-1247">Dado que una declaración de evento abstracto no proporciona una implementación, no puede proporcionar delimitado por llaves *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="98cd8-1248">Una declaración de evento que incluye tanto el `abstract` y `override` modificadores especifica que el evento es abstracto y reemplaza un evento base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="98cd8-1249">Los descriptores de acceso de este evento también son abstractas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="98cd8-1250">Solo se permiten declaraciones de eventos abstractos en clases abstractas ([clases abstractas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="98cd8-1251">Los descriptores de acceso de un evento virtual heredado pueden invalidarse en una clase derivada incluyendo una declaración de evento que especifica un `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="98cd8-1252">Esto se conoce como un ***reemplazar la declaración de evento***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="98cd8-1253">Una declaración de evento reemplazado no declara un nuevo evento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="98cd8-1254">En su lugar, simplemente especializa las implementaciones de los descriptores de acceso de un evento virtual existente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="98cd8-1255">Una declaración de evento reemplazado debe especificar los mismos modificadores de accesibilidad exacto, tipo y nombre que el evento reemplazado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="98cd8-1256">Puede incluir una declaración de evento reemplazado el `sealed` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="98cd8-1257">Uso de este modificador impide que una clase derivada siga reemplazando el evento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="98cd8-1258">Los descriptores de acceso de un evento sellado también están sellados.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="98cd8-1259">Es un error en tiempo de compilación para una declaración de evento reemplazado incluir un `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="98cd8-1260">Salvo por diferencias en la declaración e invocación sintaxis, virtual, sellado, de reemplazo y abstractos descriptores de acceso se comportan exactamente igual virtual, sellado, de reemplazo y métodos abstractos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="98cd8-1261">En concreto, las reglas se describen en [métodos virtuales](classes.md#virtual-methods), [invalidar métodos](classes.md#override-methods), [sellado métodos](classes.md#sealed-methods), y [métodos abstractos](classes.md#abstract-methods) aplicar como si los descriptores de acceso fueran métodos de la forma correspondiente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="98cd8-1262">Cada descriptor de acceso corresponde a un método con un parámetro de valor único del tipo de evento, un `void` devolver el tipo y los mismos modificadores que el evento que lo contiene.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="98cd8-1263">Indizadores</span><span class="sxs-lookup"><span data-stu-id="98cd8-1263">Indexers</span></span>

<span data-ttu-id="98cd8-1264">Un ***indizador*** es un miembro que permite que un objeto que se debe indexar en la misma manera que una matriz.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="98cd8-1265">Los indizadores se declaran mediante *indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1265">Indexers are declared using *indexer_declaration*s:</span></span>

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

<span data-ttu-id="98cd8-1266">Un *indexer_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso ](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` ([invalidar métodos](classes.md#override-methods) ), `sealed` ([Sellado métodos](classes.md#sealed-methods)), `abstract` ([métodos abstractos](classes.md#abstract-methods)), y `extern` ([métodos externos](classes.md#external-methods)) modificadores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="98cd8-1267">Las declaraciones de indizador están sujetos a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores, con la única excepción es que el modificador "static" no se permite en una declaración de indizador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="98cd8-1268">Los modificadores `virtual`, `override`, y `abstract` se excluyen mutuamente, excepto en un caso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="98cd8-1269">El `abstract` y `override` modificadores pueden usarse conjuntamente para que un indizador abstracto puede reemplazar a uno virtual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="98cd8-1270">El *tipo* declaración especifica el tipo de elemento del indizador que se incluyen en la declaración de un indizador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="98cd8-1271">A menos que el indizador es una implementación de miembro de interfaz explícita, el *tipo* va seguido de la palabra clave `this`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="98cd8-1272">Para obtener una implementación de miembro de interfaz explícita, el *tipo* va seguido de un *interface_type*, un "`.`" y la palabra clave `this`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="98cd8-1273">A diferencia de otros miembros, los indizadores no tienen nombres definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="98cd8-1274">El *formal_parameter_list* especifica los parámetros del indizador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="98cd8-1275">La lista de parámetros formales de un indizador corresponde a la de un método ([parámetros del método](classes.md#method-parameters)), excepto en que se debe especificar al menos un parámetro y que la `ref` y `out` no se permiten modificadores de parámetro .</span><span class="sxs-lookup"><span data-stu-id="98cd8-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="98cd8-1276">El *tipo* de un indizador y cada uno de los tipos que se hace referenciados en el *formal_parameter_list* deben ser al menos tan accesibles como el propio indexador ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="98cd8-1277">Un *indexer_body* puede constar de un ***cuerpo del descriptor de acceso*** o un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="98cd8-1278">En el cuerpo de un descriptor de acceso, *accessor_declarations*, que debe incluirse en "`{`"y"`}`" tokens, declare los descriptores de acceso ([descriptores de acceso](classes.md#accessors)) de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="98cd8-1279">Los descriptores de acceso especifican las instrucciones ejecutables asociadas con la lectura y escritura de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="98cd8-1280">Un cuerpo de expresión que consta de "`=>`" seguido de una expresión `E` y un punto y coma es igual que en el cuerpo de instrucción `{ get { return E; } }`y por lo tanto, sólo puede utilizarse para especificar indizadores de solo captador donde es el resultado de Get proporcionado por una sola expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="98cd8-1281">Aunque la sintaxis para tener acceso a un elemento de indizador es el mismo que para un elemento de matriz, un elemento de indizador no está clasificado como una variable.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="98cd8-1282">Por lo tanto, no es posible pasar un elemento de indexador como un `ref` o `out` argumento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="98cd8-1283">La lista de parámetros formales de un indizador define la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) del indizador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="98cd8-1284">En concreto, la firma de un indexador consta del número y tipos de sus parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="98cd8-1285">El tipo de elemento y los nombres de los parámetros formales no forman parte de la firma de un indizador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="98cd8-1286">La firma de un indizador debe ser diferente de las firmas de todos los demás indexadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="98cd8-1287">Los indizadores y propiedades son muy similares en concepto, pero difieren en lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="98cd8-1288">Una propiedad se identifica por su nombre, mientras que un indizador se identifica por su firma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="98cd8-1289">Se accede a una propiedad a través de un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)), mientras que un indizador elemento que se accede a través de un *element_access* ([acceso al indizador](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="98cd8-1290">Una propiedad puede ser un `static` miembro, mientras que un indizador siempre es un miembro de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="98cd8-1291">Un `get` descriptor de acceso de una propiedad que corresponde a un método sin parámetros, mientras que un `get` descriptor de acceso de un indizador corresponde a un método con la misma lista de parámetros formales que el indizador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="98cd8-1292">Un `set` descriptor de acceso de una propiedad que corresponde a un método con un solo parámetro denominado `value`, mientras que un `set` descriptor de acceso de un indizador corresponde a un método con la misma lista de parámetros formales que el indizador, además de un parámetro adicional denominado `value`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="98cd8-1293">Es un error en tiempo de compilación para un descriptor de acceso de indexador declarar una variable local con el mismo nombre que un parámetro del indizador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="98cd8-1294">En una declaración de propiedad reemplazada, la propiedad heredada se accede mediante la sintaxis `base.P`, donde `P` es el nombre de propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="98cd8-1295">En una declaración de indexador de reemplazo, el indizador heredado se tiene acceso a la sintaxis `base[E]`, donde `E` es una lista separada por comas de expresiones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="98cd8-1296">No hay ningún concepto de "un indizador implementado automáticamente".</span><span class="sxs-lookup"><span data-stu-id="98cd8-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="98cd8-1297">Es un error tener un indizador no abstracta, no externo con descriptores de acceso de punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="98cd8-1298">Aparte de estas diferencias, todas las reglas se definen en [descriptores de acceso](classes.md#accessors) y [implementa automáticamente propiedades](classes.md#automatically-implemented-properties) se aplican a los descriptores de acceso, así como para los descriptores de acceso de propiedad.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="98cd8-1299">Cuando se incluye una declaración de indexador una `extern` modificador, el indizador se dice que un ***indizador externo***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="98cd8-1300">Dado que la declaración de un indizador externo no proporciona ninguna implementación real, cada uno de sus *accessor_declarations* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="98cd8-1301">El ejemplo siguiente declara un `BitArray` clase que implementa un indizador para tener acceso a los bits individuales de la matriz de bits.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

<span data-ttu-id="98cd8-1302">Una instancia de la `BitArray` clase consume mucha menos memoria que correspondiente `bool[]` (ya que cada valor de la primera ocupa un solo bit en lugar de la última de un byte), pero permite las mismas operaciones que un `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="98cd8-1303">La siguiente `CountPrimes` clase utiliza un `BitArray` y el algoritmo de "criba" clásica para calcular el número de primos entre 1 y un máximo determinado:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

<span data-ttu-id="98cd8-1304">Tenga en cuenta que la sintaxis para tener acceso a los elementos de la `BitArray` es precisamente la misma que para un `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="98cd8-1305">El ejemplo siguiente muestra una clase de 26 \* 10 cuadrícula que tiene un indizador con dos parámetros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="98cd8-1306">El primer parámetro debe ser una mayúscula o minúscula en el intervalo A-z y el segundo debe ser un entero en el intervalo 0-9.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a><span data-ttu-id="98cd8-1307">Sobrecarga de indizadores</span><span class="sxs-lookup"><span data-stu-id="98cd8-1307">Indexer overloading</span></span>

<span data-ttu-id="98cd8-1308">Se describen las reglas de resolución de sobrecarga de indizador en [inferencia de tipo](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="98cd8-1309">Operadores</span><span class="sxs-lookup"><span data-stu-id="98cd8-1309">Operators</span></span>

<span data-ttu-id="98cd8-1310">Un ***operador*** es un miembro que define el significado de un operador de expresión que se puede aplicar a las instancias de la clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="98cd8-1311">Los operadores se declaran mediante *operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1311">Operators are declared using *operator_declaration*s:</span></span>

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

<span data-ttu-id="98cd8-1312">Hay tres categorías de operadores sobrecargables: Operadores unarios ([operadores unarios](classes.md#unary-operators)), los operadores binarios ([operadores binarios](classes.md#binary-operators)) y los operadores de conversión ([operadores de conversión](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="98cd8-1313">El *operator_body* ya sea un punto y coma, un ***cuerpo de instrucción*** o un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="98cd8-1314">Consta de un cuerpo de instrucción de un *bloque*, que especifica las instrucciones que se ejecutará cuando se invoca el operador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="98cd8-1315">El *bloque* debe cumplir las reglas de devolución por valor métodos descritos en [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="98cd8-1316">Un cuerpo de expresión consta de `=>` seguido de una expresión y un punto y coma y denota una expresión única para realizar cuando se invoca el operador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="98cd8-1317">Para `extern` operadores, el *operator_body* consiste simplemente en un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="98cd8-1318">Para todos los demás operadores, el *operator_body* es un cuerpo de bloques o un cuerpo de expresión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="98cd8-1319">Las siguientes reglas se aplican a todas las declaraciones de operador:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="98cd8-1320">Una declaración de un operador debe incluir tanto un `public` y un `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="98cd8-1321">Los parámetros de un operador deben ser parámetros de valor ([parámetros del valor](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="98cd8-1322">Es un error en tiempo de compilación para una declaración de un operador especificar `ref` o `out` parámetros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="98cd8-1323">La firma de un operador ([operadores unarios](classes.md#unary-operators), [operadores binarios](classes.md#binary-operators), [operadores de conversión](classes.md#conversion-operators)) deben ser diferentes de las firmas de todos los demás operadores declarados en el misma clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="98cd8-1324">Todos los tipos que se hace referencia en una declaración de un operador deben ser al menos tan accesibles como el propio operador ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="98cd8-1325">Es un error para el mismo modificador aparece varias veces en una declaración de operador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="98cd8-1326">Cada categoría de operador impone restricciones adicionales, como se describe en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="98cd8-1327">Al igual que otros miembros, las clases derivadas heredan los operadores declarados en una clase base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="98cd8-1328">Dado que las declaraciones de operador siempre requieren la clase o estructura en la que se declara el operador para participar en la firma del operador, no es posible que un operador declarado en una clase derivada ocultar un operador declarado en una clase base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="98cd8-1329">Por lo tanto, el `new` modificador nunca es necesaria y, por lo tanto, no se permite en una declaración de operador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="98cd8-1330">Encontrará información adicional sobre los operadores unarios y binarios en [operadores](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="98cd8-1331">Se puede encontrar información adicional sobre operadores de conversión en [conversiones definidas por el usuario](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="98cd8-1332">Operadores unarios</span><span class="sxs-lookup"><span data-stu-id="98cd8-1332">Unary operators</span></span>

<span data-ttu-id="98cd8-1333">Las reglas siguientes se aplican a las declaraciones de operador unario, donde `T` denota el tipo de instancia de la clase o estructura que contiene la declaración del operador:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="98cd8-1334">Unario `+`, `-`, `!`, o `~` operador debe tomar un único parámetro de tipo `T` o `T?` y puede devolver cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="98cd8-1335">Unario `++` o `--` operador debe tomar un único parámetro de tipo `T` o `T?` y debe devolver que mismo tipo o un tipo derivado de éste.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="98cd8-1336">Unario `true` o `false` operador debe tomar un único parámetro de tipo `T` o `T?` y debe devolver un tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="98cd8-1337">La firma de un operador unario consiste en el símbolo de operador (`+`, `-`, `!`, `~`, `++`, `--`, `true`, o `false`) y el tipo de parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="98cd8-1338">El tipo de valor devuelto no es parte de la firma de un operador unario, ni es el nombre del parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="98cd8-1339">El `true` y `false` operadores unarios requieren declaración par a par.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="98cd8-1340">Si una clase declara uno de estos operadores sin declarar el otro, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="98cd8-1341">El `true` y `false` operadores se describen más detalladamente en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators) y [expresiones booleanas](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="98cd8-1342">El ejemplo siguiente muestra una implementación y el uso posterior de `operator ++` para una clase de vector de enteros:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

<span data-ttu-id="98cd8-1343">Tenga en cuenta cómo el método de operador devuelve el valor producido por sumar 1 al operando, al igual que el incremento de postfijo y operadores de decremento ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)) y el prefijo de incremento y decremento operadores ([prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="98cd8-1344">A diferencia de en C++, este método necesita no modifica el valor de su operando directamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="98cd8-1345">De hecho, modificar el valor del operando infringiría la semántica estándar del operador de incremento de postfijo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="98cd8-1346">Operadores binarios</span><span class="sxs-lookup"><span data-stu-id="98cd8-1346">Binary operators</span></span>

<span data-ttu-id="98cd8-1347">Las reglas siguientes se aplican a las declaraciones de operador binario, donde `T` denota el tipo de instancia de la clase o estructura que contiene la declaración del operador:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="98cd8-1348">Un operador de desplazamiento que no sea binario debe tomar dos parámetros, al menos uno de los cuales debe tener tipo `T` o `T?`y puede devolver cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="98cd8-1349">Un archivo binario `<<` o `>>` operador debe tomar dos parámetros, el primero de los cuales debe tener tipo `T` o `T?` y el segundo de los cuales debe tener tipo `int` o `int?`y puede devolver cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="98cd8-1350">La firma de un operador binario consiste en el símbolo de operador (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, o `<=`) y los tipos de los dos parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="98cd8-1351">El tipo de valor devuelto y los nombres de los parámetros formales no forman parte de la firma de un operador binario.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="98cd8-1352">Algunos operadores binarios requieren declaración par a par.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="98cd8-1353">Para todas las declaraciones de cualquier operador de un par, debe haber una declaración del operador del par coincidente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="98cd8-1354">Coincide con dos declaraciones de operador cuando tienen el mismo tipo de valor devuelto y el mismo tipo para cada parámetro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="98cd8-1355">Los siguientes operadores requieren declaración por pares:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="98cd8-1356">`operator ==` y `operator !=`</span><span class="sxs-lookup"><span data-stu-id="98cd8-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="98cd8-1357">`operator >` y `operator <`</span><span class="sxs-lookup"><span data-stu-id="98cd8-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="98cd8-1358">`operator >=` y `operator <=`</span><span class="sxs-lookup"><span data-stu-id="98cd8-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="98cd8-1359">Operadores de conversión</span><span class="sxs-lookup"><span data-stu-id="98cd8-1359">Conversion operators</span></span>

<span data-ttu-id="98cd8-1360">Una declaración de operador de conversión introduce un ***conversión definida por el usuario*** ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)) que aumenta las conversiones explícitas e implícitas predefinidas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="98cd8-1361">Una declaración de operador de conversión que incluye la `implicit` palabra clave define una conversión implícita de definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="98cd8-1362">Conversiones implícitas pueden ocurrir en una variedad de situaciones, incluidas las llamadas a funciones miembro, expresiones de conversión y asignaciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="98cd8-1363">Esto se describe más detalladamente en [conversiones implícitas](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="98cd8-1364">Una declaración de operador de conversión que incluye la `explicit` palabra clave define una conversión explícita de definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="98cd8-1365">Las conversiones explícitas pueden producir en expresiones de conversión y se describe más detalladamente en [las conversiones explícitas](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="98cd8-1366">Un operador de conversión convierte de un tipo de origen, indicado por el tipo de parámetro del operador de conversión a un tipo de destino indicado por el tipo de valor devuelto del operador de conversión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="98cd8-1367">Para un tipo de origen dado `S` y tipo de destino `T`si `S` o `T` son tipos que aceptan valores NULL, permiten `S0` y `T0` hacen referencia a sus tipos subyacentes, de lo contrario `S0` y `T0` son igual que `S` y `T` respectivamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="98cd8-1368">Una clase o struct se puede declarar una conversión de un tipo de origen `S` a un tipo de destino `T` sólo si se cumplen todas las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="98cd8-1369">`S0` y `T0` son tipos diferentes.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="98cd8-1370">Ya sea `S0` o `T0` es el tipo de clase o estructura en la que realiza la declaración del operador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="98cd8-1371">Ni `S0` ni `T0` es un *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="98cd8-1372">Excluyendo las conversiones definidas por el usuario, no existe una conversión desde `S` a `T` o desde `T` a `S`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="98cd8-1373">Para los fines de estas reglas, cualquier tipo de parámetros asociados con `S` o `T` se consideran tipos únicos que tienen ninguna relación de herencia con otros tipos y todas las restricciones de tipo de esos parámetros se omiten.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="98cd8-1374">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="98cd8-1375">se permiten las declaraciones de dos operador primera porque, para los fines de [indizadores](classes.md#indexers).3, `T` y `int` y `string` respectivamente se consideran tipos únicos sin ninguna relación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="98cd8-1376">Sin embargo, el tercer operador es un error porque `C<T>` es la clase base de `D<T>`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="98cd8-1377">Desde la segunda regla que sigue un operador de conversión debe convertir hacia o desde el tipo de clase o estructura en la que se declara el operador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="98cd8-1378">Por ejemplo, es posible que un tipo de clase o struct `C` para definir una conversión de `C` a `int` y desde `int` a `C`, pero no desde `int` a `bool`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="98cd8-1379">No es posible volver a definir directamente una conversión predefinida.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="98cd8-1380">Por lo tanto, no se permiten los operadores de conversión para convertir de o a `object` porque ya existen conversiones implícitas y explícitas entre `object` y todos los demás tipos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="98cd8-1381">Del mismo modo, ni el origen ni los tipos de destino de una conversión pueden ser un tipo base del otro, ya que entonces ya existiría una conversión.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="98cd8-1382">Sin embargo, es posible declarar los operadores en tipos genéricos que, para los argumentos de tipo determinado, especifican las conversiones que ya existen como conversiones predefinidas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="98cd8-1383">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="98cd8-1384">Cuando escriba `object` se especifica como un argumento de tipo para `T`, el segundo operador declara una conversión que ya existe (implícita y por lo tanto también explícito, no existe conversión de cualquier tipo `object`).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="98cd8-1385">En casos donde no existe una conversión predefinida entre dos tipos, se omiten cualquier conversiones definidas por el usuario entre esos tipos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="98cd8-1386">De manera específica:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1386">Specifically:</span></span>

*  <span data-ttu-id="98cd8-1387">Si una conversión implícita predefinida ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde tipo `S` al tipo `T`, todos los definidos por el usuario conversiones (implícitas o explícitas) de `S` a `T` se omiten.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="98cd8-1388">Si una conversión explícita predefinida ([las conversiones explícitas](conversions.md#explicit-conversions)) existe desde tipo `S` al tipo `T`, las conversiones explícitas definidas por el usuario de `S` a `T` se omiten.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="98cd8-1389">Furthermore:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1389">Furthermore:</span></span>

<span data-ttu-id="98cd8-1390">Si `T` es un tipo de interfaz definido por el usuario conversiones implícitas de `S` a `T` se omiten.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="98cd8-1391">De lo contrario, definido por el usuario conversiones implícitas de `S` a `T` todavía se consideran.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="98cd8-1392">Para todos los tipos, pero `object`, los operadores declarados por el `Convertible<T>` tipo anterior no entren en conflicto con las conversiones predefinidas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="98cd8-1393">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="98cd8-1394">Sin embargo, para el tipo `object`, conversiones predefinidas ocultar las conversiones definidas por el usuario en todos los casos, pero uno:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="98cd8-1395">No se permiten conversiones definidas por el usuario para convertir de o a *interface_type*s.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="98cd8-1396">En concreto, esta restricción garantiza que ninguna transformación definido por el usuario se produce cuando se convierte en un *interface_type*y que una conversión a un *interface_type* se realizará correctamente solo si el objeto que se está convirtiendo implementa realmente especificado *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="98cd8-1397">La firma de un operador de conversión está formada por el tipo de origen y el tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="98cd8-1398">(Tenga en cuenta que esta es la única forma de miembro para el que el tipo de valor devuelto participa en la firma.) El `implicit` o `explicit` clasificación de un operador de conversión no forma parte de la firma del operador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="98cd8-1399">Por lo tanto, una clase o struct no puede declarar tanto un `implicit` y un `explicit` operador de conversión con los mismos tipos de origen y de destino.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="98cd8-1400">En general, las conversiones implícitas definido por el usuario deben diseñarse para nunca produzcan excepciones ni pérdida de información.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="98cd8-1401">Si una conversión definida por el usuario puede dar lugar a excepciones (por ejemplo, porque el argumento de origen está fuera del intervalo) o pérdida de información (por ejemplo, descartar los bits de orden superior), dicha conversión debe definirse como una conversión explícita.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="98cd8-1402">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1402">In the example</span></span>
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
<span data-ttu-id="98cd8-1403">la conversión de `Digit` a `byte` es implícita porque nunca produce excepciones o pierde información, pero la conversión de `byte` a `Digit` es explícito desde `Digit` sólo se puede representar un subconjunto de los posibles los valores de un `byte`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="98cd8-1404">Constructores de instancias</span><span class="sxs-lookup"><span data-stu-id="98cd8-1404">Instance constructors</span></span>

<span data-ttu-id="98cd8-1405">Un ***constructor de instancia*** es un miembro que implementa las acciones necesarias para inicializar una instancia de una clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="98cd8-1406">Constructores de instancia se declaran mediante *constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="98cd8-1407">Un *constructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso ](classes.md#access-modifiers)) y un `extern` ([métodos externos](classes.md#external-methods)) modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="98cd8-1408">No se permite una declaración de constructor para incluir el mismo modificador varias veces.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="98cd8-1409">El *identificador* de un *constructor_declarator* debe nombrar la clase en el que se declara el constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="98cd8-1410">Si se especifica ningún otro nombre, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="98cd8-1411">El elemento opcional *formal_parameter_list* de una instancia del constructor está sujeto a las mismas reglas que el *formal_parameter_list* de un método ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="98cd8-1412">La lista de parámetros formales define la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un constructor de instancia y se controla mediante el cual el proceso de resolución de sobrecarga ([inferencia](expressions.md#type-inference)) selecciona un determinado constructor de instancia en una invocación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="98cd8-1413">Cada uno de los tipos que se hace referenciados en el *formal_parameter_list* de una instancia del constructor debe ser al menos tan accesible como el propio constructor ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="98cd8-1414">El elemento opcional *constructor_initializer* especifica otro constructor de instancia para invocar antes de ejecutar las instrucciones que aparecen la *constructor_body* de este constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="98cd8-1415">Esto se describe más detalladamente en [inicializadores de Constructor](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="98cd8-1416">Cuando una declaración de constructor incluye un `extern` modificador, el constructor se dice que un ***constructor externo***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="98cd8-1417">Dado que una declaración de constructor externo no proporciona ninguna implementación real, su *constructor_body* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="98cd8-1418">Para todos los demás constructores, el *constructor_body* consta de un *bloque* que especifica las instrucciones para inicializar una nueva instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="98cd8-1419">Esto corresponde exactamente a la *bloque* de un método de instancia con un `void` tipo de valor devuelto ([cuerpo del método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="98cd8-1420">No se heredan los constructores de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="98cd8-1421">Por lo tanto, una clase no tiene ningún constructor de instancia distinto de aquéllos declarados realmente en la clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="98cd8-1422">Si una clase no contiene ninguna declaración de constructor de instancia, automáticamente se proporciona un constructor de instancia predeterminado ([constructores predeterminados](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="98cd8-1423">Los constructores de instancia se invocan mediante *object_creation_expression*s ([expresiones de creación de objetos](expressions.md#object-creation-expressions)) y *constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="98cd8-1424">Inicializadores del constructor</span><span class="sxs-lookup"><span data-stu-id="98cd8-1424">Constructor initializers</span></span>

<span data-ttu-id="98cd8-1425">Todos los constructores de instancias (los de la clase excepto `object`) incluyen implícitamente una invocación de otro constructor de instancia inmediatamente antes del *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="98cd8-1426">El constructor debe invocar implícitamente viene determinada por la *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="98cd8-1427">Un inicializador de constructor de instancia del formulario `base(argument_list)` o `base()` hace que un constructor de instancia de la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="98cd8-1428">Este constructor se selecciona mediante *argument_list* si existe y las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="98cd8-1429">El conjunto de constructores de instancias de candidato consta de todos los constructores de instancia accesible contenidos en la clase base directa o el constructor predeterminado ([constructores predeterminados](classes.md#default-constructors)), si se declara ningún constructor de instancia en el clase base directa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="98cd8-1430">Si este conjunto está vacío, o si no se puede identificar un mejor constructor de instancia, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="98cd8-1431">Un inicializador de constructor de instancia del formulario `this(argument-list)` o `this()` hace que un constructor de instancia de la clase en sí que se debe invocar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="98cd8-1432">El constructor se selecciona utilizando *argument_list* si existe y las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="98cd8-1433">El conjunto de constructores de instancias de candidato consta de todos los constructores de instancia accesible declarados en la propia clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="98cd8-1434">Si este conjunto está vacío, o si no se puede identificar un mejor constructor de instancia, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="98cd8-1435">Si una declaración de constructor de instancia incluye a un inicializador de constructor que invoca el constructor en Sí, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="98cd8-1436">Si un constructor de instancia no tiene ningún inicializador de constructor, un inicializador de constructor del formulario `base()` se proporciona implícitamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="98cd8-1437">Por lo tanto, una declaración de constructor de instancia del formulario</span><span class="sxs-lookup"><span data-stu-id="98cd8-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="98cd8-1438">equivale exactamente a</span><span class="sxs-lookup"><span data-stu-id="98cd8-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="98cd8-1439">El ámbito de los parámetros proporcionados por el *formal_parameter_list* de un constructor de instancia declaración incluye el inicializador de constructor de dicha declaración.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="98cd8-1440">Por lo tanto, se permite un inicializador de constructor para tener acceso a los parámetros del constructor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="98cd8-1441">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1441">For example:</span></span>
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

<span data-ttu-id="98cd8-1442">Un inicializador de constructor de instancia no puede tener acceso a la instancia que se está creando.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="98cd8-1443">Por lo tanto, es un error en tiempo de compilación para hacer referencia a `this` en una expresión de argumento del inicializador del constructor, como es un error en tiempo de compilación para una expresión de argumento hacer referencia a cualquier miembro de instancia a través de un *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="98cd8-1444">Inicializadores de variables de instancia</span><span class="sxs-lookup"><span data-stu-id="98cd8-1444">Instance variable initializers</span></span>

<span data-ttu-id="98cd8-1445">Cuando un constructor de instancia no tiene ningún inicializador de constructor, o tiene un inicializador de constructor del formulario `base(...)`, implícitamente el constructor realiza las inicializaciones especificadas por el *variable_initializer*s de los campos de instancia declaran en su clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="98cd8-1446">Esto corresponde a una secuencia de asignaciones que se ejecutan inmediatamente al entrar en el constructor y antes de la invocación del constructor de clase base directa implícita.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="98cd8-1447">Los inicializadores de variable se ejecutan en el orden textual en que aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="98cd8-1448">Ejecución de un constructor</span><span class="sxs-lookup"><span data-stu-id="98cd8-1448">Constructor execution</span></span>

<span data-ttu-id="98cd8-1449">Inicializadores de variables se transforman en instrucciones de asignación y se ejecutan estas instrucciones de asignación antes de la invocación del constructor de instancia de clase base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="98cd8-1450">Este orden garantiza que todos los campos de instancia se inicializan por sus inicializadores de variable antes de que se ejecutan las instrucciones que tienen acceso a esa instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="98cd8-1451">Dado el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1451">Given the example</span></span>
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
<span data-ttu-id="98cd8-1452">Cuando `new B()` se utiliza para crear una instancia de `B`, se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```
x = 1, y = 0
```

<span data-ttu-id="98cd8-1453">El valor de `x` es 1 porque el inicializador de variable se ejecuta antes de invoca el constructor de instancia de la clase base.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="98cd8-1454">Sin embargo, el valor de `y` es 0 (el valor predeterminado de un `int`) porque la asignación a `y` no se ejecuta hasta que el constructor de clase base devuelve.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="98cd8-1455">Resulta útil pensar en los inicializadores de variables de instancia y los inicializadores de constructor como instrucciones que se insertan automáticamente antes de la *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="98cd8-1456">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1456">The example</span></span>
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
<span data-ttu-id="98cd8-1457">contiene a varios inicializadores de variables; También contiene los inicializadores de constructor de ambas formas (`base` y `this`).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="98cd8-1458">El ejemplo se corresponde con el código se muestra a continuación, donde cada comentario indica una instrucción que se inserta automáticamente (la sintaxis utilizada para invocar el constructor insertado automáticamente no es válido, pero sirve para ilustrar el mecanismo).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a><span data-ttu-id="98cd8-1459">Constructores predeterminados</span><span class="sxs-lookup"><span data-stu-id="98cd8-1459">Default constructors</span></span>

<span data-ttu-id="98cd8-1460">Si una clase no contiene ninguna declaración de constructor de instancia, se proporciona automáticamente un constructor de instancia predeterminado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="98cd8-1461">El constructor predeterminado simplemente invoca el constructor sin parámetros de la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="98cd8-1462">Si la clase es abstracta está protegida la accesibilidad declarada para el constructor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="98cd8-1463">En caso contrario, la accesibilidad declarada para el constructor predeterminado es pública.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="98cd8-1464">Por lo tanto, es siempre el constructor predeterminado del formulario</span><span class="sxs-lookup"><span data-stu-id="98cd8-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="98cd8-1465">o</span><span class="sxs-lookup"><span data-stu-id="98cd8-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="98cd8-1466">donde `C` es el nombre de la clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="98cd8-1467">Si la resolución de sobrecarga no puede determinar a un único mejores candidatos para el inicializador de constructor de clase base, a continuación, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="98cd8-1468">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="98cd8-1469">se proporciona un constructor predeterminado porque la clase no contiene ninguna declaración de constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="98cd8-1470">Por lo tanto, el ejemplo es equivalente a</span><span class="sxs-lookup"><span data-stu-id="98cd8-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="98cd8-1471">Constructores privados</span><span class="sxs-lookup"><span data-stu-id="98cd8-1471">Private constructors</span></span>

<span data-ttu-id="98cd8-1472">Cuando una clase `T` declara sólo los constructores de instancia privados, no es posible para las clases fuera el texto del programa `T` derivar `T` o crear directamente instancias de `T`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="98cd8-1473">Por lo tanto, si una clase contiene a sólo miembros estáticos y no está diseñada para ejecutarse, agregar un constructor de instancia privada vacía impedirá creación de instancias.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="98cd8-1474">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1474">For example:</span></span>
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

<span data-ttu-id="98cd8-1475">La `Trig` clase grupos de constantes y métodos relacionados, pero no está pensada para ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="98cd8-1476">Por lo tanto, declara un constructor de instancia única de privado vacío.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="98cd8-1477">Debe declararse al menos un constructor de instancia para suprimir la generación automática de un constructor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="98cd8-1478">Parámetros del constructor de instancia opcional</span><span class="sxs-lookup"><span data-stu-id="98cd8-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="98cd8-1479">El `this(...)` forma de inicializador de constructor se suele utilizar junto con la sobrecarga para implementar los parámetros del constructor de instancia opcional.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="98cd8-1480">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1480">In the example</span></span>
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
<span data-ttu-id="98cd8-1481">los primeros constructores de instancia de dos simplemente proporcionan los valores predeterminados para los argumentos que faltan.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="98cd8-1482">Ambos usan un `this(...)` inicializador de constructor para invocar el tercer constructor de instancia, lo que realmente se encarga de inicializar la nueva instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="98cd8-1483">El efecto es el de los parámetros del constructor opcional:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="98cd8-1484">Constructores estáticos</span><span class="sxs-lookup"><span data-stu-id="98cd8-1484">Static constructors</span></span>

<span data-ttu-id="98cd8-1485">Un ***constructor estático*** es un miembro que implementa las acciones necesarias para inicializar un tipo de clase cerrado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="98cd8-1486">Los constructores estáticos se declaran mediante *static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="98cd8-1487">Un *static_constructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y un `extern` modificador ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="98cd8-1488">El *identificador* de un *static_constructor_declaration* debe nombrar la clase en el que se declara el constructor estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="98cd8-1489">Si se especifica ningún otro nombre, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="98cd8-1490">Cuando una declaración de constructor estático incluye un `extern` modificador, el constructor estático se dice que un ***constructor estático***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="98cd8-1491">Dado que una declaración de constructor estático no proporciona ninguna implementación real, su *static_constructor_body* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="98cd8-1492">Para todas las declaraciones de constructor estático, el *static_constructor_body* consta de un *bloque* que especifica las instrucciones que se ejecutarán para inicializar la clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="98cd8-1493">Esto corresponde exactamente a la *cuerpoMétodo* de un método estático con un `void` tipo de valor devuelto ([cuerpo del método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="98cd8-1494">Los constructores estáticos no se heredan y no se puede llamar directamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="98cd8-1495">El constructor estático de un tipo de clase cerrado como máximo una vez se ejecuta en un dominio de aplicación determinado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="98cd8-1496">El primero de los siguientes eventos que se producen dentro de un dominio de aplicación, se desencadena la ejecución de un constructor estático:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="98cd8-1497">Se crea una instancia del tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="98cd8-1498">Se hace referencia a cualquiera de los miembros estáticos del tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="98cd8-1499">Si una clase contiene la `Main` método ([inicio de la aplicación](basic-concepts.md#application-startup)) en que comienza la ejecución, el constructor estático para esa clase se ejecuta antes que la `Main` se llama al método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="98cd8-1500">Para inicializar un nuevo tipo de clase cerrados, en primer lugar un nuevo conjunto de campos estáticos ([campos estáticos y de instancia](classes.md#static-and-instance-fields)) para ese tipo cerrado determinado se ha creado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="98cd8-1501">Cada uno de los campos estáticos se inicializa en su valor predeterminado ([los valores predeterminados](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="98cd8-1502">A continuación, los inicializadores de campo estático ([inicialización de campos estáticos](classes.md#static-field-initialization)) se ejecutan para esos campos estáticos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="98cd8-1503">Por último, se ejecuta el constructor estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="98cd8-1504">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1504">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
<span data-ttu-id="98cd8-1505">el resultado debe ser:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1505">must produce the output:</span></span>
```
Init A
A.F
Init B
B.F
```
<span data-ttu-id="98cd8-1506">Dado que la ejecución de `A`del constructor estático se desencadena por la llamada a `A.F`como la ejecución de `B`del constructor estático se desencadena por la llamada a `B.F`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="98cd8-1507">Es posible construir dependencias circulares que permiten a los campos estáticos con inicializadores de variables que se deben observar en su estado de valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="98cd8-1508">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1508">The example</span></span>
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
<span data-ttu-id="98cd8-1509">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="98cd8-1509">produces the output</span></span>
```
X = 1, Y = 2
```

<span data-ttu-id="98cd8-1510">Para ejecutar el `Main` método, el sistema ejecuta por primera vez el inicializador para `B.Y`, antes de la clase `B`del constructor estático.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="98cd8-1511">`Y`del inicializador hace `A`del constructor estático para que se puede ejecutar porque el valor de `A.X` se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="98cd8-1512">El constructor estático de `A` a su vez va a calcular el valor de `X`y hacer búsquedas por lo que el valor predeterminado de `Y`, que es cero.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="98cd8-1513">`A.X` por lo tanto se inicializa a 1.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="98cd8-1514">El proceso de ejecución `A`inicializadores de campo estático de constructor estático y, a continuación, finaliza, volviendo al cálculo del valor inicial de `Y`, el resultado se convierte en 2.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="98cd8-1515">Dado que el constructor estático se ejecuta exactamente una vez para cada tipo de clase construida de cerrado, es un lugar conveniente para exigir comprobaciones de tiempo de ejecución en el parámetro de tipo que no se puede comprobar en tiempo de compilación a través de restricciones ([parámetro de tipo restricciones](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="98cd8-1516">Por ejemplo, el siguiente tipo utiliza un constructor estático para exigir que el argumento de tipo es una enumeración:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a><span data-ttu-id="98cd8-1517">Destructores</span><span class="sxs-lookup"><span data-stu-id="98cd8-1517">Destructors</span></span>

<span data-ttu-id="98cd8-1518">Un ***destructor*** es un miembro que implementa las acciones necesarias para destruir una instancia de una clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="98cd8-1519">Se declara un destructor con un *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1519">A destructor is declared using a *destructor_declaration*:</span></span>

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

<span data-ttu-id="98cd8-1520">Un *destructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="98cd8-1521">El *identificador* de un *destructor_declaration* debe nombrar la clase en el que se declara el destructor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="98cd8-1522">Si se especifica ningún otro nombre, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="98cd8-1523">Cuando una declaración de destructor incluye un `extern` modificador, el destructor se dice que un ***destructor externo***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="98cd8-1524">Dado que una declaración de destructor externo no proporciona ninguna implementación real, su *destructor_body* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="98cd8-1525">Para el resto de los destructores, la *destructor_body* consta de un *bloque* que especifica las instrucciones que se ejecutarán para destruir una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="98cd8-1526">Un *destructor_body* corresponde exactamente a la *cuerpoMétodo* de un método de instancia con un `void` tipo de valor devuelto ([cuerpo del método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="98cd8-1527">No se heredan los destructores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1527">Destructors are not inherited.</span></span> <span data-ttu-id="98cd8-1528">Por lo tanto, una clase tiene el destructor que se puede declarar en esa clase.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="98cd8-1529">Puesto que un destructor debe no tener ningún parámetro, no puede sobrecargarse, por lo que puede tener una clase, como máximo, un destructor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="98cd8-1530">Los destructores se invocan automáticamente y no se puede invocar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="98cd8-1531">Una instancia se vuelve apta para su destrucción cuando ya no es posible que cualquier código para que use esa instancia.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="98cd8-1532">La ejecución del destructor para la instancia puede producirse en cualquier momento después de la instancia se vuelve apta para su destrucción.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="98cd8-1533">Cuando se destruye una instancia, se llaman a los destructores de cadena de herencia de la instancia en orden, de la más derivada a la menos derivada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="98cd8-1534">Un destructor se puede ejecutar en cualquier subproceso.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="98cd8-1535">Para obtener más información sobre las reglas que rigen cuándo y cómo se ejecuta un destructor, vea [administración de memoria automática](basic-concepts.md#automatic-memory-management).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="98cd8-1536">La salida del ejemplo</span><span class="sxs-lookup"><span data-stu-id="98cd8-1536">The output of the example</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
<span data-ttu-id="98cd8-1537">is</span><span class="sxs-lookup"><span data-stu-id="98cd8-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="98cd8-1538">Puesto que los destructores de una cadena de herencia se llaman en orden, de la más derivada a la menos derivada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="98cd8-1539">Los destructores se implementan mediante la invalidación del método virtual `Finalize` en `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="98cd8-1540">Programas de C# no pueden invalidar este método o llame al (o invalidaciones del mismo) directamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="98cd8-1541">Por ejemplo, el programa</span><span class="sxs-lookup"><span data-stu-id="98cd8-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="98cd8-1542">contiene dos errores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1542">contains two errors.</span></span>

<span data-ttu-id="98cd8-1543">El compilador se comporta como si este método y reemplazos, no existen en absoluto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="98cd8-1544">Por lo tanto, este programa:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="98cd8-1545">es válido, y muestra el método oculta `System.Object`del `Finalize` método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="98cd8-1546">Para obtener una explicación del comportamiento cuando se produce una excepción desde un destructor, vea [cómo se controlan las excepciones](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="98cd8-1547">Iterators</span><span class="sxs-lookup"><span data-stu-id="98cd8-1547">Iterators</span></span>

<span data-ttu-id="98cd8-1548">Un miembro de función ([miembros de función](expressions.md#function-members)) implementa mediante un bloque de iteradores ([bloques](statements.md#blocks)) se denomina un ***iterador***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="98cd8-1549">Un bloque de iteradores puede utilizarse como el cuerpo de un miembro de función como el tipo de valor devuelto del miembro de función correspondiente es una de las interfaces de enumerador ([interfaces de enumerador](classes.md#enumerator-interfaces)) o una de las interfaces enumerables ([Interfaces enumerables](classes.md#enumerable-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="98cd8-1550">Se puede producir como un *cuerpoMétodo*, *operator_body* o *accessor_body*, mientras que los eventos, constructores de instancias, los constructores estáticos y destructores no pueden ser se implementa como iteradores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="98cd8-1551">Cuando un miembro de función se implementa mediante un bloque de iteradores, es un error en tiempo de compilación para la lista de parámetros formales del miembro de función para especificar cualquier `ref` o `out` parámetros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="98cd8-1552">Interfaces de enumerador</span><span class="sxs-lookup"><span data-stu-id="98cd8-1552">Enumerator interfaces</span></span>

<span data-ttu-id="98cd8-1553">El ***interfaces de enumerador*** son la interfaz no genérica `System.Collections.IEnumerator` y todas las instancias de la interfaz genérica `System.Collections.Generic.IEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="98cd8-1554">Para mayor brevedad, en este capítulo estas interfaces se denominarán `IEnumerator` y `IEnumerator<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="98cd8-1555">Interfaces enumerables</span><span class="sxs-lookup"><span data-stu-id="98cd8-1555">Enumerable interfaces</span></span>

<span data-ttu-id="98cd8-1556">El ***interfaces enumerables*** son la interfaz no genérica `System.Collections.IEnumerable` y todas las instancias de la interfaz genérica `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="98cd8-1557">Para mayor brevedad, en este capítulo estas interfaces se denominarán `IEnumerable` y `IEnumerable<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="98cd8-1558">Tipo yield</span><span class="sxs-lookup"><span data-stu-id="98cd8-1558">Yield type</span></span>

<span data-ttu-id="98cd8-1559">Un iterador genera una secuencia de valores, todos del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="98cd8-1560">Este tipo se denomina el ***tipo yield*** del iterador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="98cd8-1561">El tipo de rendimiento de un iterador que devuelve `IEnumerator` o `IEnumerable` es `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="98cd8-1562">El tipo de rendimiento de un iterador que devuelve `IEnumerator<T>` o `IEnumerable<T>` es `T`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="98cd8-1563">Objetos del enumerador</span><span class="sxs-lookup"><span data-stu-id="98cd8-1563">Enumerator objects</span></span>

<span data-ttu-id="98cd8-1564">Cuando un miembro de función devuelve un enumerador de tipo de interfaz se implementa mediante un bloque de iteradores, invocar al miembro de función no se ejecuta inmediatamente el código del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="98cd8-1565">En su lugar, un ***objeto enumerador*** se crea y devuelve.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="98cd8-1566">Este objeto encapsula el código especificado en el bloque de iteradores y la ejecución del código del bloque de iteradores se produce cuando el objeto de enumerador `MoveNext` se invoca el método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="98cd8-1567">Un objeto de enumerador tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="98cd8-1568">Implementa `IEnumerator` y `IEnumerator<T>`, donde `T` es el tipo yield del iterador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="98cd8-1569">Implementa `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="98cd8-1570">Se inicializa con una copia de los valores de argumento (si existe) y pasa el valor de instancia para el miembro de función.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="98cd8-1571">Tiene cuatro estados posibles, ***antes***, ***ejecutando***, ***suspendido***, y ***después***y está inicialmente en el ***antes***  estado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="98cd8-1572">Un objeto de enumerador es normalmente una instancia de una clase de enumerador generado por el compilador que encapsula el código del bloque de iteradores e implementa las interfaces de enumerador, pero son posibles otros métodos de implementación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="98cd8-1573">Si el compilador genera una clase de enumerador, esa clase se anidarán, directa o indirectamente, en la clase que contiene el miembro de función, tendrá accesibilidad privada, y tendrá un nombre reservado para uso del compilador ([identificadores ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="98cd8-1574">Un objeto de enumerador puede implementar más interfaces que las especificadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="98cd8-1575">Las secciones siguientes describen el comportamiento exacto de la `MoveNext`, `Current`, y `Dispose` los miembros de la `IEnumerable` y `IEnumerable<T>` proporcionadas por un objeto de enumerador de implementaciones de interfaz.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="98cd8-1576">Tenga en cuenta que los objetos del enumerador no admiten la `IEnumerator.Reset` método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="98cd8-1577">Invocar este método hace que un `System.NotSupportedException` que se produzca.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="98cd8-1578">El método MoveNext</span><span class="sxs-lookup"><span data-stu-id="98cd8-1578">The MoveNext method</span></span>

<span data-ttu-id="98cd8-1579">El `MoveNext` método de un objeto de enumerador encapsula el código de un bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="98cd8-1580">Invocar el `MoveNext` método ejecuta código en el bloque de iteradores y establece el `Current` propiedad del objeto enumerador según corresponda.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="98cd8-1581">La acción exacta realizada por `MoveNext` depende del estado del objeto del enumerador cuando `MoveNext` se invoca:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="98cd8-1582">Si el estado del objeto del enumerador es ***antes***, al invocar `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="98cd8-1583">Cambia el estado a ***ejecutando***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="98cd8-1584">Inicializa los parámetros (incluido `this`) del bloque de iteradores para los valores de argumento y el valor de instancia guarda cuando se inicializó el objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="98cd8-1585">Ejecuta el bloque de iteradores desde el principio hasta que se interrumpe la ejecución (como se describe a continuación).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="98cd8-1586">Si el estado del objeto del enumerador es ***ejecutando***, el resultado de invocar `MoveNext` no se ha especificado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="98cd8-1587">Si el estado del objeto del enumerador es ***suspendido***, al invocar `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="98cd8-1588">Cambia el estado a ***ejecutando***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="98cd8-1589">Restaura los valores de todas las variables locales y parámetros (incluido this) a los valores guardados cuando por última vez se suspende la ejecución del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="98cd8-1590">Tenga en cuenta que el contenido de todos los objetos que hacen referencia estas variables puede haber cambiado desde la anterior llamada a MoveNext.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="98cd8-1591">Reanuda la ejecución del bloque de iteradores inmediatamente después de la `yield return` instrucción que causó la suspensión de la ejecución y continúa hasta que se interrumpe la ejecución (como se describe a continuación).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="98cd8-1592">Si el estado del objeto del enumerador es ***después***, al invocar `MoveNext` devuelve `false`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="98cd8-1593">Cuando `MoveNext` ejecuta el bloque de iteradores, puede interrumpir la ejecución de cuatro maneras: Por un `yield return` instrucción, por un `yield break` instrucción cuando se encuentra al final del bloque de iteradores y debido a una excepción que se produce y se propaga fuera del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="98cd8-1594">Cuando un `yield return` se encuentra la instrucción ([la instrucción yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="98cd8-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="98cd8-1595">La expresión proporcionada en la instrucción se evalúa implícitamente convierte al tipo yield y asignada a la `Current` propiedad del objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="98cd8-1596">Se suspende la ejecución del cuerpo del iterador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="98cd8-1597">Los valores de todas las variables locales y parámetros (incluidos `this`) se guardan, tal como es la ubicación de este `yield return` instrucción.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="98cd8-1598">Si el `yield return` instrucción está dentro de uno o varios `try` bloquea asociado `finally` bloques no se ejecutan en este momento.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="98cd8-1599">Se cambia el estado del objeto del enumerador a ***suspendido***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="98cd8-1600">El `MoveNext` devuelve del método `true` a su llamador, que indica que la iteración avanzó correctamente al siguiente valor.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="98cd8-1601">Cuando un `yield break` se encuentra la instrucción ([la instrucción yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="98cd8-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="98cd8-1602">Si el `yield break` instrucción está dentro de uno o varios `try` bloquea asociado `finally` bloques se ejecutan.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="98cd8-1603">Se cambia el estado del objeto del enumerador a ***después***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="98cd8-1604">El `MoveNext` devuelve del método `false` a su llamador, que indica que la iteración está completa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="98cd8-1605">Cuando se encuentra el final del cuerpo del iterador:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="98cd8-1606">Se cambia el estado del objeto del enumerador a ***después***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="98cd8-1607">El `MoveNext` devuelve del método `false` a su llamador, que indica que la iteración está completa.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="98cd8-1608">Cuando se produce una excepción y se propaga fuera del bloque de iteradores:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="98cd8-1609">Adecuado `finally` bloques en el cuerpo del iterador habrá ejecutados por la propagación de excepciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="98cd8-1610">Se cambia el estado del objeto del enumerador a ***después***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="98cd8-1611">Continúa la propagación de excepciones al llamador de la `MoveNext` método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="98cd8-1612">La propiedad actual</span><span class="sxs-lookup"><span data-stu-id="98cd8-1612">The Current property</span></span>

<span data-ttu-id="98cd8-1613">Un objeto de enumerador `Current` propiedad se ve afectada por `yield return` las instrucciones del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="98cd8-1614">Cuando un objeto de enumerador está en el ***suspendido*** de estado, el valor de `Current` es el valor establecido por la llamada anterior a `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="98cd8-1615">Cuando un objeto de enumerador está en el ***antes***, ***ejecutando***, o ***después*** indica, el resultado del acceso a `Current` no se ha especificado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="98cd8-1616">Para un iterador con un rendimiento escriba distinto `object`, el resultado del acceso a `Current` a través del objeto de enumerador `IEnumerable` implementación corresponde al acceso a `Current` a través del objeto de enumerador `IEnumerator<T>` implementación y convirtiendo el resultado a `object`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="98cd8-1617">El método Dispose</span><span class="sxs-lookup"><span data-stu-id="98cd8-1617">The Dispose method</span></span>

<span data-ttu-id="98cd8-1618">El `Dispose` método se utiliza para limpiar la iteración al traer el objeto de enumerador a la ***después*** estado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="98cd8-1619">Si el estado del objeto del enumerador es ***antes***, al invocar `Dispose` cambia el estado a ***después***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="98cd8-1620">Si el estado del objeto del enumerador es ***ejecutando***, el resultado de invocar `Dispose` no se ha especificado.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="98cd8-1621">Si el estado del objeto del enumerador es ***suspendido***, al invocar `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="98cd8-1622">Cambia el estado a ***ejecutando***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="98cd8-1623">Ejecute cualquiera, por último, bloques como si ejecuta la última `yield return` instrucción fuera un `yield break` instrucción.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="98cd8-1624">Si esto produce una excepción se produce y se propaga fuera del cuerpo del iterador, el estado del objeto de enumerador se establece en ***después*** y la excepción se propaga al llamador de la `Dispose` método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="98cd8-1625">Cambia el estado a ***después***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="98cd8-1626">Si el estado del objeto del enumerador es ***después***, al invocar `Dispose` no tiene ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="98cd8-1627">Objetos enumerables</span><span class="sxs-lookup"><span data-stu-id="98cd8-1627">Enumerable objects</span></span>

<span data-ttu-id="98cd8-1628">Cuando un miembro de función devuelve un tipo de interfaz enumerable se implementa mediante un bloque de iteradores, invocar al miembro de función no se ejecuta inmediatamente el código del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="98cd8-1629">En su lugar, un ***objeto enumerable*** se crea y devuelve.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="98cd8-1630">El objeto enumerable `GetEnumerator` método devuelve un objeto de enumerador que encapsula el código especificado en el bloque de iteradores y la ejecución del código del bloque de iteradores se produce cuando el objeto de enumerador `MoveNext` se invoca el método.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="98cd8-1631">Un objeto enumerable tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="98cd8-1632">Implementa `IEnumerable` y `IEnumerable<T>`, donde `T` es el tipo yield del iterador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="98cd8-1633">Se inicializa con una copia de los valores de argumento (si existe) y pasa el valor de instancia para el miembro de función.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="98cd8-1634">Un objeto enumerable normalmente es una instancia de una clase enumerable generado por el compilador que encapsula el código del bloque de iteradores e implementa las interfaces enumerables, pero son posibles otros métodos de implementación.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="98cd8-1635">Si el compilador genera una clase enumerable, esa clase se anidarán, directa o indirectamente, en la clase que contiene el miembro de función, tendrá accesibilidad privada, y tendrá un nombre reservado para uso del compilador ([identificadores ](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="98cd8-1636">Un objeto enumerable puede implementar más interfaces que las especificadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="98cd8-1637">En concreto, también puede implementar un objeto enumerable `IEnumerator` y `IEnumerator<T>`, habilitarla para que actúe como un objeto enumerable y un enumerador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="98cd8-1638">En ese tipo de implementación, la primera vez que un objeto enumerable `GetEnumerator` se invoca al método, se devuelve el mismo objeto enumerable.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="98cd8-1639">Las siguientes invocaciones del objeto enumerable `GetEnumerator`, si existe, devuelve una copia del objeto enumerable.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="98cd8-1640">Por lo tanto, cada uno devuelve enumerador tiene su propio estado y los cambios de un enumerador no afectarán a otro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="98cd8-1641">El método GetEnumerator</span><span class="sxs-lookup"><span data-stu-id="98cd8-1641">The GetEnumerator method</span></span>

<span data-ttu-id="98cd8-1642">Un objeto enumerable proporciona una implementación de la `GetEnumerator` métodos de la `IEnumerable` y `IEnumerable<T>` interfaces.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="98cd8-1643">Los dos `GetEnumerator` métodos comparten una implementación común que adquiere y devuelve un objeto de enumerador disponible.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="98cd8-1644">El objeto de enumerador se inicializa con los valores de argumento y la instancia valor guardado cuando el objeto enumerable que se ha inicializado, pero en caso contrario, las funciones del objeto de enumerador tal como se describe en [objetos enumerador](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="98cd8-1645">Ejemplo de implementación</span><span class="sxs-lookup"><span data-stu-id="98cd8-1645">Implementation example</span></span>

<span data-ttu-id="98cd8-1646">Esta sección describe una posible implementación de iteradores en cuanto a las construcciones C# estándar.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="98cd8-1647">La implementación que se describen aquí se basa en los mismos principios utilizados por el compilador de C# de Microsoft, pero en ningún caso es una implementación obligatoria o el único posible.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="98cd8-1648">La siguiente `Stack<T>` la clase implementa su `GetEnumerator` método mediante un iterador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="98cd8-1649">El iterador enumera los elementos de la pila en orden descendente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

<span data-ttu-id="98cd8-1650">El `GetEnumerator` método se puede traducir en una instancia de una clase de enumerador generado por el compilador que encapsula el código en el bloque de iteradores, tal como se muestra en la siguiente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="98cd8-1651">En la traducción anterior, se convierte en un equipo de estado y se coloca en el código del bloque de iteradores el `MoveNext` método de la clase de enumerador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="98cd8-1652">Además, la variable local `i` se convierte en un campo en el objeto de enumerador para que pueda seguir existen entre las distintas invocaciones de `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="98cd8-1653">El ejemplo siguiente imprime una sencilla tabla de multiplicación de enteros del 1 al 10.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="98cd8-1654">El `FromTo` método en el ejemplo devuelve un objeto enumerable y se implementa mediante un iterador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

<span data-ttu-id="98cd8-1655">El `FromTo` método se puede traducir en una instancia de una clase enumerable generado por el compilador que encapsula el código del bloque de iteradores, tal como se muestra en la siguiente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="98cd8-1656">La clase enumerable implementa las interfaces enumerables y las interfaces de enumerador, habilitarla para que actúe como un objeto enumerable y un enumerador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="98cd8-1657">La primera vez el `GetEnumerator` se invoca al método, se devuelve el mismo objeto enumerable.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="98cd8-1658">Las siguientes invocaciones del objeto enumerable `GetEnumerator`, si existe, devuelve una copia del objeto enumerable.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="98cd8-1659">Por lo tanto, cada uno devuelve enumerador tiene su propio estado y los cambios de un enumerador no afectarán a otro.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="98cd8-1660">El `Interlocked.CompareExchange` método se utiliza para garantizar un funcionamiento seguro para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="98cd8-1661">El `from` y `to` parámetros se convierten en campos de la clase enumerable.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="98cd8-1662">Dado que `from` se modifica en el bloque de iteradores adicional `__from` campo se introdujo para contener el valor inicial dado a `from` en cada enumerador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="98cd8-1663">El `MoveNext` método produce una `InvalidOperationException` si se llama cuando `__state` es `0`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="98cd8-1664">Esto protege contra el uso del objeto enumerable como un objeto de enumerador sin llamar primero a `GetEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="98cd8-1665">El ejemplo siguiente muestra una clase simple de árbol.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="98cd8-1666">El `Tree<T>` la clase implementa su `GetEnumerator` método mediante un iterador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="98cd8-1667">El iterador enumera los elementos del árbol en orden infijo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

<span data-ttu-id="98cd8-1668">El `GetEnumerator` método se puede traducir en una instancia de una clase de enumerador generado por el compilador que encapsula el código en el bloque de iteradores, tal como se muestra en la siguiente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

<span data-ttu-id="98cd8-1669">Los objetos temporales generados por el compilador usados en el `foreach` se levantan instrucciones en el `__left` y `__right` campos del objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="98cd8-1670">El `__state` cuidadosamente se actualiza el campo del objeto enumerador para que el valor correcto `Dispose()` se llamará al método correctamente si se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="98cd8-1671">Tenga en cuenta que no es posible escribir el código traducido con simple `foreach` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="98cd8-1672">Funciones asincrónicas</span><span class="sxs-lookup"><span data-stu-id="98cd8-1672">Async functions</span></span>

<span data-ttu-id="98cd8-1673">Un método ([métodos](classes.md#methods)) o una función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) con el `async` modificador se denomina un ***función asincrónica***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="98cd8-1674">En general, el término ***async*** se usa para describir cualquier tipo de función que tiene el `async` modificador.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="98cd8-1675">Es un error en tiempo de compilación para la lista de parámetros formales de una función asincrónica para especificar cualquier `ref` o `out` parámetros.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="98cd8-1676">El *return_type* Async método debe ser `void` o un ***tipo de tarea***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="98cd8-1677">Los tipos de tarea son `System.Threading.Tasks.Task` y tipos construyan desde `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="98cd8-1678">Por brevedad, en este capítulo estos tipos se denominan `Task` y `Task<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="98cd8-1679">Un método asincrónico devuelve un tipo de tarea se dice que devuelven tareas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="98cd8-1680">La definición exacta de los tipos de tarea es implementación definida, pero desde la perspectiva del lenguaje, un tipo de tarea está en uno de los Estados incompletos, se ha realizado correctamente o con errores.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="98cd8-1681">Una tarea con error registra una excepción pertinente.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="98cd8-1682">Una correcta `Task<T>` registra un resultado de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="98cd8-1683">Tipos de tareas son esperables y puede ser, por tanto, los operandos de expresiones await ([expresiones Await](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="98cd8-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="98cd8-1684">Una invocación de función async tiene la capacidad para suspender la evaluación por medio de expresiones await ([expresiones Await](expressions.md#await-expressions)) en su cuerpo.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="98cd8-1685">Más adelante se puede reanudar la evaluación en el punto de suspensión de la expresión por medio de await un ***delegado de reanudación***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="98cd8-1686">El delegado de reanudación es de tipo `System.Action`, y cuando se invoca, evaluación de la invocación de función async se reanudará desde la expresión await, donde se quedó.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="98cd8-1687">El ***llamador actual*** de una función asincrónica invocación es el llamador original si nunca se ha suspendido la invocación de función o el llamador del delegado de reanudación más reciente, en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="98cd8-1688">Evaluación de una función asincrónica de devolución de tarea</span><span class="sxs-lookup"><span data-stu-id="98cd8-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="98cd8-1689">Invocación de una función asincrónica de devolución de tarea hace que una instancia del tipo de tarea devuelta se genere.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="98cd8-1690">Esto se denomina la ***task devuelto*** de la función async.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="98cd8-1691">La tarea está inicialmente en un estado incompleto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="98cd8-1692">El cuerpo de la función async, a continuación, se evalúa hasta que se suspende (que se alcanza una expresión await) o se finaliza, en el que se devuelve punto de control al llamador, junto con la tarea devuelto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="98cd8-1693">Cuando el cuerpo de la función asincrónica termina, la tarea devuelta se mueve fuera del estado incompleto:</span><span class="sxs-lookup"><span data-stu-id="98cd8-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="98cd8-1694">Si el cuerpo de la función termina como resultado de alcanzar una instrucción return o al final del cuerpo, cualquier valor de resultado se registra en la tarea devuelta, que se coloca en un estado correcto.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="98cd8-1695">Si el cuerpo de la función termina como resultado de una excepción no detectada ([la instrucción throw](statements.md#the-throw-statement)) se grabó la excepción en la tarea devuelta que se coloca en un estado de error.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="98cd8-1696">Evaluación de una función asincrónica devuelve void</span><span class="sxs-lookup"><span data-stu-id="98cd8-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="98cd8-1697">Si el tipo de valor devuelto de la función async es `void`, evaluación difiere de los pasos anteriores en la siguiente manera: Dado que no se devuelve ninguna tarea, la función comunica en su lugar excepciones en el subproceso actual y finalización ***contexto de sincronización***.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="98cd8-1698">La definición exacta del contexto de sincronización depende de la implementación, pero es una representación de "donde" se está ejecutando el subproceso actual.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="98cd8-1699">El contexto de sincronización se notifica cuando comienza la evaluación de una función asincrónica devuelve void, se completa correctamente o provoca que se produzca una excepción no detectada.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="98cd8-1700">Esto permite que el contexto para realizar un seguimiento de cuántas funciones async devuelven void se ejecutan en él y decidir cómo propagar las excepciones que salen de ellas.</span><span class="sxs-lookup"><span data-stu-id="98cd8-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
