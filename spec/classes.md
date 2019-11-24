---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703979"
---
# <a name="classes"></a><span data-ttu-id="7db9f-101">Clases</span><span class="sxs-lookup"><span data-stu-id="7db9f-101">Classes</span></span>

<span data-ttu-id="7db9f-102">Una clase es una estructura de datos que puede contener miembros de datos (constantes y campos), miembros de función (métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos) y tipos anidados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="7db9f-103">Los tipos de clase admiten la herencia, un mecanismo mediante el cual una clase derivada puede extender y especializar una clase base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="7db9f-104">Declaraciones de clase</span><span class="sxs-lookup"><span data-stu-id="7db9f-104">Class declarations</span></span>

<span data-ttu-id="7db9f-105">Una *class_declaration* es una *type_declaration* ([declaraciones de tipos](namespaces.md#type-declarations)) que declara una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="7db9f-106">Un *class_declaration* está compuesto de un conjunto opcional *de atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *class_modifier*s ([modificadores de clase](classes.md#class-modifiers)), seguido de un modificador de `partial` opcional, seguido de la palabra clave `class` y un *identificador* que nombra la clase, seguido de un *type_parameter_list* opcional (parámetros de[tipo](classes.md#type-parameters)), seguido de una especificación de *class_base* opcional (especificación de[clase base](classes.md#class-base-specification)), por un conjunto opcional de *type_parameter_constraints_clause*s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), seguido de un *class_body* ([cuerpo de clase](classes.md#class-body)), opcionalmente seguido de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="7db9f-107">Una declaración de clase no puede proporcionar *type_parameter_constraints_clause*s a menos que también proporcione un *type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="7db9f-108">Una declaración de clase que proporciona un *type_parameter_list* es una ***declaración de clase genérica***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="7db9f-109">Además, cualquier clase anidada dentro de una declaración de clase genérica o una declaración de estructura genérica es una declaración de clase genérica, ya que se deben proporcionar los parámetros de tipo para el tipo contenedor para crear un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="7db9f-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="7db9f-110">Modificadores de clase</span><span class="sxs-lookup"><span data-stu-id="7db9f-110">Class modifiers</span></span>

<span data-ttu-id="7db9f-111">Un *class_declaration* puede incluir opcionalmente una secuencia de modificadores de clase:</span><span class="sxs-lookup"><span data-stu-id="7db9f-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="7db9f-112">Es un error en tiempo de compilación que el mismo modificador aparezca varias veces en una declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="7db9f-113">Se permite el modificador `new` en las clases anidadas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="7db9f-114">Especifica que la clase oculta un miembro heredado con el mismo nombre, tal y como se describe en [el modificador New](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="7db9f-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="7db9f-115">Es un error en tiempo de compilación que el modificador `new` aparezca en una declaración de clase que no es una declaración de clase anidada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="7db9f-116">Los modificadores `public`, `protected`, `internal`y `private` controlan la accesibilidad de la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="7db9f-117">Dependiendo del contexto en el que se produce la declaración de clase, es posible que algunos de estos modificadores no estén permitidos (se[declare accesibilidad](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="7db9f-118">Los modificadores `abstract`, `sealed` y `static` se describen en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="7db9f-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="7db9f-119">Clases abstractas</span><span class="sxs-lookup"><span data-stu-id="7db9f-119">Abstract classes</span></span>

<span data-ttu-id="7db9f-120">El modificador `abstract` se usa para indicar que una clase está incompleta y que está destinada a usarse solo como una clase base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="7db9f-121">Una clase abstracta es distinta de una clase no abstracta de las siguientes maneras:</span><span class="sxs-lookup"><span data-stu-id="7db9f-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="7db9f-122">No se puede crear una instancia de una clase abstracta directamente y es un error en tiempo de compilación usar el operador `new` en una clase abstracta.</span><span class="sxs-lookup"><span data-stu-id="7db9f-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="7db9f-123">Aunque es posible tener variables y valores cuyos tipos en tiempo de compilación sean abstractos, tales variables y valores serán necesariamente `null` o contengan referencias a instancias de clases no abstractas derivadas de los tipos abstractos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="7db9f-124">Se permite que una clase abstracta contenga miembros abstractos (aunque no es necesario).</span><span class="sxs-lookup"><span data-stu-id="7db9f-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="7db9f-125">Una clase abstracta no puede ser sellada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="7db9f-126">Cuando una clase no abstracta se deriva de una clase abstracta, la clase no abstracta debe incluir las implementaciones reales de todos los miembros abstractos heredados, con lo que se reemplazan los miembros abstractos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="7db9f-127">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-127">In the example</span></span>
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
<span data-ttu-id="7db9f-128">la clase abstracta `A` presenta un método abstracto `F`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="7db9f-129">La clase `B` introduce un método adicional `G`, pero dado que no proporciona una implementación de `F`, `B` también debe declararse como abstracta.</span><span class="sxs-lookup"><span data-stu-id="7db9f-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="7db9f-130">La clase `C` invalida `F` y proporciona una implementación real.</span><span class="sxs-lookup"><span data-stu-id="7db9f-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="7db9f-131">Dado que no hay miembros abstractos en `C`, se permite que `C` (pero no es necesario) sea no abstracto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="7db9f-132">Clases selladas</span><span class="sxs-lookup"><span data-stu-id="7db9f-132">Sealed classes</span></span>

<span data-ttu-id="7db9f-133">El modificador `sealed` se utiliza para evitar la derivación de una clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="7db9f-134">Se produce un error en tiempo de compilación si se especifica una clase sellada como la clase base de otra clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="7db9f-135">Una clase sellada no puede ser también una clase abstracta.</span><span class="sxs-lookup"><span data-stu-id="7db9f-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="7db9f-136">El modificador `sealed` se utiliza principalmente para evitar la derivación no deseada, pero también permite ciertas optimizaciones en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7db9f-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="7db9f-137">En concreto, dado que se sabe que una clase sellada nunca tiene clases derivadas, es posible transformar las invocaciones de miembros de función virtual en instancias de clase selladas en invocaciones no virtuales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="7db9f-138">Clases estáticas</span><span class="sxs-lookup"><span data-stu-id="7db9f-138">Static classes</span></span>

<span data-ttu-id="7db9f-139">El modificador `static` se usa para marcar la clase que se declara como una ***clase estática***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="7db9f-140">No se puede crear una instancia de una clase estática, no se puede usar como un tipo y solo puede contener miembros estáticos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="7db9f-141">Solo una clase estática puede contener declaraciones de métodos de extensión ([métodos de extensión](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="7db9f-142">Una declaración de clase estática está sujeta a las siguientes restricciones:</span><span class="sxs-lookup"><span data-stu-id="7db9f-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="7db9f-143">Una clase estática no puede incluir un modificador `sealed` o `abstract`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="7db9f-144">Sin embargo, tenga en cuenta que, puesto que no se pueden crear instancias de una clase estática ni derivarse de, se comporta como si fuera Sealed y Abstract.</span><span class="sxs-lookup"><span data-stu-id="7db9f-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="7db9f-145">Una clase estática no puede incluir una especificación de *class_base* ([clase base Specification](classes.md#class-base-specification)) y no puede especificar explícitamente una clase base o una lista de interfaces implementadas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="7db9f-146">Una clase estática hereda implícitamente del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="7db9f-147">Una clase estática solo puede contener miembros estáticos ([miembros estáticos y de instancia](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="7db9f-148">Tenga en cuenta que las constantes y los tipos anidados se clasifican como miembros estáticos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="7db9f-149">Una clase estática no puede tener miembros con `protected` o `protected internal` la accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="7db9f-150">Es un error en tiempo de compilación infringir cualquiera de estas restricciones.</span><span class="sxs-lookup"><span data-stu-id="7db9f-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="7db9f-151">Una clase estática no tiene ningún constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-151">A static class has no instance constructors.</span></span> <span data-ttu-id="7db9f-152">No es posible declarar un constructor de instancia en una clase estática, y no se proporciona ningún constructor de instancia predeterminado ([constructores predeterminados](classes.md#default-constructors)) para una clase estática.</span><span class="sxs-lookup"><span data-stu-id="7db9f-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="7db9f-153">Los miembros de una clase estática no son estáticos automáticamente y las declaraciones de miembros deben incluir explícitamente un modificador `static` (excepto para las constantes y los tipos anidados).</span><span class="sxs-lookup"><span data-stu-id="7db9f-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="7db9f-154">Cuando una clase está anidada dentro de una clase externa estática, la clase anidada no es una clase estática a menos que incluya explícitamente un modificador `static`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="7db9f-155">__Referencia a tipos de clase estáticos__</span><span class="sxs-lookup"><span data-stu-id="7db9f-155">__Referencing static class types__</span></span>

<span data-ttu-id="7db9f-156">Se permite que un *namespace_or_type_name* ([espacio de nombres y nombres de tipo](basic-concepts.md#namespace-and-type-names)) haga referencia a una clase estática Si</span><span class="sxs-lookup"><span data-stu-id="7db9f-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="7db9f-157">El *namespace_or_type_name* es el `T` en un *namespace_or_type_name* del formulario `T.I`, o</span><span class="sxs-lookup"><span data-stu-id="7db9f-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="7db9f-158">El *namespace_or_type_name* es el `T` en un *typeof_expression* ([listas de argumentos](expressions.md#argument-lists)1) del formulario `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="7db9f-159">Se permite que un *primary_expression* ([miembros de función](expressions.md#function-members)) haga referencia a una clase estática Si</span><span class="sxs-lookup"><span data-stu-id="7db9f-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="7db9f-160">El *primary_expression* es el `E` en un *member_access* ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) del formulario `E.I`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="7db9f-161">En cualquier otro contexto, se trata de un error en tiempo de compilación para hacer referencia a una clase estática.</span><span class="sxs-lookup"><span data-stu-id="7db9f-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="7db9f-162">Por ejemplo, es un error que una clase estática se use como una clase base, un tipo constituyente ([tipos anidados](classes.md#nested-types)) de un miembro, un argumento de tipo genérico o una restricción de parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="7db9f-163">Del mismo modo, una clase estática no se puede usar en un tipo de matriz, un tipo de puntero, una expresión de `new`, una expresión de conversión, una expresión de `is`, una expresión de `as`, una expresión de `sizeof` o una expresión de valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="7db9f-164">Unmodifier (modificador)</span><span class="sxs-lookup"><span data-stu-id="7db9f-164">Partial modifier</span></span>

<span data-ttu-id="7db9f-165">El modificador `partial` se utiliza para indicar que este *class_declaration* es una declaración de tipos parciales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="7db9f-166">Varias declaraciones de tipos parciales con el mismo nombre dentro de una declaración de tipo o espacio de nombres envolvente se combinan para formar una declaración de tipos, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="7db9f-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="7db9f-167">Tener la declaración de una clase distribuida sobre segmentos independientes del texto del programa puede ser útil si estos segmentos se producen o mantienen en contextos diferentes.</span><span class="sxs-lookup"><span data-stu-id="7db9f-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="7db9f-168">Por ejemplo, una parte de una declaración de clase puede ser generada por el equipo, mientras que la otra se crea manualmente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="7db9f-169">La separación textual de los dos impide que las actualizaciones se realicen en conflicto con las actualizaciones del otro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="7db9f-170">Parámetros de tipo</span><span class="sxs-lookup"><span data-stu-id="7db9f-170">Type parameters</span></span>

<span data-ttu-id="7db9f-171">Un parámetro de tipo es un identificador simple que denota un marcador de posición para un argumento de tipo proporcionado para crear un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="7db9f-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="7db9f-172">Un parámetro de tipo es un marcador de posición formal para un tipo que se proporcionará más adelante.</span><span class="sxs-lookup"><span data-stu-id="7db9f-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="7db9f-173">Por el contrario, un argumento de tipo ([argumentos de tipo](types.md#type-arguments)) es el tipo real que se sustituye por el parámetro de tipo cuando se crea un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="7db9f-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="7db9f-174">Cada parámetro de tipo de una declaración de clase define un nombre en el espacio de declaración ([declaraciones](basic-concepts.md#declarations)) de esa clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="7db9f-175">Por lo tanto, no puede tener el mismo nombre que otro parámetro de tipo o un miembro declarado en esa clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="7db9f-176">Un parámetro de tipo no puede tener el mismo nombre que el propio tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="7db9f-177">Especificación de clase base</span><span class="sxs-lookup"><span data-stu-id="7db9f-177">Class base specification</span></span>

<span data-ttu-id="7db9f-178">Una declaración de clase puede incluir una especificación de *class_base* , que define la clase base directa de la clase y las interfaces ([interfaces](interfaces.md)) implementadas directamente por la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="7db9f-179">La clase base especificada en una declaración de clase puede ser un tipo de clase construido ([tipos construidos](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="7db9f-180">Una clase base no puede ser un parámetro de tipo por su cuenta, aunque puede incluir los parámetros de tipo que se encuentran en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="7db9f-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="7db9f-181">Clases base</span><span class="sxs-lookup"><span data-stu-id="7db9f-181">Base classes</span></span>

<span data-ttu-id="7db9f-182">Cuando se incluye un *class_type* en el *class_base*, especifica la clase base directa de la clase que se está declarando.</span><span class="sxs-lookup"><span data-stu-id="7db9f-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="7db9f-183">Si una declaración de clase no tiene *class_base*, o si el *class_base* solo enumera los tipos de interfaz, se supone que la clase base directa se `object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="7db9f-184">Una clase hereda los miembros de su clase base directa, como se describe en [herencia](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="7db9f-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="7db9f-185">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="7db9f-186">la clase `A` se dice que es la clase base directa de `B`y `B` se dice que se deriva de `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="7db9f-187">Puesto que `A` no especifica explícitamente una clase base directa, su clase base directa se `object`implícitamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="7db9f-188">Para un tipo de clase construido, si se especifica una clase base en la declaración de clase genérica, la clase base del tipo construido se obtiene sustituyendo, por cada *type_parameter* en la declaración de clase base, el *type_argument* correspondiente del tipo construido.</span><span class="sxs-lookup"><span data-stu-id="7db9f-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="7db9f-189">Dadas las declaraciones de clase genéricas</span><span class="sxs-lookup"><span data-stu-id="7db9f-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="7db9f-190">la clase base del tipo construido `G<int>` sería `B<string,int[]>`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="7db9f-191">La clase base directa de un tipo de clase debe ser al menos igual de accesible que el propio tipo de clase ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="7db9f-192">Por ejemplo, se trata de un error en tiempo de compilación para que una clase `public` derive de una clase `private` o `internal`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="7db9f-193">La clase base directa de un tipo de clase no debe ser ninguno de los siguientes tipos: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`o `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="7db9f-194">Además, una declaración de clase genérica no puede usar `System.Attribute` como una clase base directa o indirecta.</span><span class="sxs-lookup"><span data-stu-id="7db9f-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="7db9f-195">A la hora de determinar el significado de la especificación de clase base directa `A` de una `B`de clase, se supone que la clase base directa de `B` está `object`da de forma temporal.</span><span class="sxs-lookup"><span data-stu-id="7db9f-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="7db9f-196">Intuitivamente esto garantiza que el significado de una especificación de clase base no puede depender recursivamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="7db9f-197">En el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="7db9f-198">es un error porque en la especificación de clase base `A<C.B>` la clase base directa de `C` se considera `object`y, por lo tanto, las reglas de [nombres de espacio de nombres y de tipo](basic-concepts.md#namespace-and-type-names), `C` no se considera que tiene un miembro `B`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="7db9f-199">Las clases base de un tipo de clase son la clase base directa y sus clases base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="7db9f-200">En otras palabras, el conjunto de clases base es el cierre transitivo de la relación de clase base directa.</span><span class="sxs-lookup"><span data-stu-id="7db9f-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="7db9f-201">En el ejemplo anterior, las clases base de `B` son `A` y `object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="7db9f-202">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="7db9f-203">las clases base de `D<int>` son `C<int[]>`, `B<IComparable<int[]>>`, `A`y `object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="7db9f-204">Excepto en el caso de la clase `object`, cada tipo de clase tiene exactamente una clase base directa.</span><span class="sxs-lookup"><span data-stu-id="7db9f-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="7db9f-205">La clase `object` no tiene ninguna clase base directa y es la última clase base de todas las demás clases.</span><span class="sxs-lookup"><span data-stu-id="7db9f-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="7db9f-206">Cuando una clase `B` deriva de una `A`de clase, es un error en tiempo de compilación para que `A` dependa de `B`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="7db9f-207">Una clase ***depende directamente de*** su clase base directa (si existe) y ***depende directamente de*** la clase en la que se anida inmediatamente (si existe).</span><span class="sxs-lookup"><span data-stu-id="7db9f-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="7db9f-208">Dada esta definición, el conjunto completo de clases de las que depende una clase es el cierre reflexivo y transitivo de la relación ***directamente depende de*** .</span><span class="sxs-lookup"><span data-stu-id="7db9f-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="7db9f-209">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="7db9f-210">es erróneo porque la clase depende de sí misma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="7db9f-211">Del mismo modo, el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="7db9f-212">es un error porque las clases dependen circularmente por sí mismas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="7db9f-213">Por último, el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="7db9f-214">produce un error en tiempo de compilación porque `A` depende de `B.C` (su clase base directa), que depende de `B` (su clase envolvente inmediata), que depende circularmente de `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="7db9f-215">Tenga en cuenta que una clase no depende de las clases anidadas en ella.</span><span class="sxs-lookup"><span data-stu-id="7db9f-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="7db9f-216">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="7db9f-217">`B` depende de `A` (porque `A` es tanto su clase base directa como su clase envolvente inmediata), pero `A` no depende de `B` (puesto que `B` no es una clase base ni una clase envolvente de `A`).</span><span class="sxs-lookup"><span data-stu-id="7db9f-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="7db9f-218">Por lo tanto, el ejemplo es válido.</span><span class="sxs-lookup"><span data-stu-id="7db9f-218">Thus, the example is valid.</span></span>

<span data-ttu-id="7db9f-219">No es posible derivar de una clase `sealed`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="7db9f-220">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="7db9f-221">la `B` de clase tiene un error porque intenta derivar de la clase `sealed` `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="7db9f-222">Implementaciones de interfaces</span><span class="sxs-lookup"><span data-stu-id="7db9f-222">Interface implementations</span></span>

<span data-ttu-id="7db9f-223">Una especificación de *class_base* puede incluir una lista de tipos de interfaz, en cuyo caso se dice que la clase implementa directamente los tipos de interfaz especificados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="7db9f-224">Las implementaciones de interfaz se tratan en las [implementaciones de interfaz](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="7db9f-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="7db9f-225">Restricciones de parámetros de tipo</span><span class="sxs-lookup"><span data-stu-id="7db9f-225">Type parameter constraints</span></span>

<span data-ttu-id="7db9f-226">Las declaraciones de tipos y métodos genéricos pueden especificar opcionalmente restricciones de parámetros de tipo incluyendo *type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="7db9f-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="7db9f-227">Cada *type_parameter_constraints_clause* consta del `where`de token, seguido del nombre de un parámetro de tipo, seguido de dos puntos y la lista de restricciones para ese parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="7db9f-228">Puede haber como máximo una cláusula `where` para cada parámetro de tipo y las cláusulas `where` se pueden enumerar en cualquier orden.</span><span class="sxs-lookup"><span data-stu-id="7db9f-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="7db9f-229">Al igual que los tokens de `get` y `set` en un descriptor de acceso de propiedad, el token de `where` no es una palabra clave.</span><span class="sxs-lookup"><span data-stu-id="7db9f-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="7db9f-230">La lista de restricciones dadas en una cláusula `where` puede incluir cualquiera de los componentes siguientes, en este orden: una restricción Primary única, una o varias restricciones secundarias y la restricción de constructor, `new()`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="7db9f-231">Una restricción Primary puede ser un tipo de clase o la ***restricción de tipo de referencia*** `class` o la restricción de tipo de ***valor*** `struct`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="7db9f-232">Una restricción secundaria puede ser una *type_parameter* o *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="7db9f-233">La restricción de tipo de referencia especifica que un argumento de tipo usado para el parámetro de tipo debe ser un tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="7db9f-234">Todos los tipos de clase, tipos de interfaz, tipos de delegado, tipos de matriz y parámetros de tipo que se sabe que son un tipo de referencia (como se define a continuación) satisfacen esta restricción.</span><span class="sxs-lookup"><span data-stu-id="7db9f-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="7db9f-235">La restricción de tipo de valor especifica que un argumento de tipo usado para el parámetro de tipo debe ser un tipo de valor que no acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="7db9f-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="7db9f-236">Todos los tipos de struct que no aceptan valores NULL, tipos de enumeración y parámetros de tipo con la restricción de tipo de valor cumplen esta restricción.</span><span class="sxs-lookup"><span data-stu-id="7db9f-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="7db9f-237">Tenga en cuenta que aunque se clasifique como un tipo de valor, un tipo que acepta valores NULL ([tipos que aceptan valores NULL](types.md#nullable-types)) no satisface la restricción de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="7db9f-238">Un parámetro de tipo que tiene la restricción de tipo de valor no puede tener también el *constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="7db9f-239">Los tipos de puntero nunca pueden ser argumentos de tipo y no se tienen en cuenta para satisfacer las restricciones de tipo de valor o tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="7db9f-240">Si una restricción es un tipo de clase, un tipo de interfaz o un parámetro de tipo, ese tipo especifica un "tipo base" mínimo que cada argumento de tipo utilizado para ese parámetro de tipo debe admitir.</span><span class="sxs-lookup"><span data-stu-id="7db9f-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="7db9f-241">Siempre que se usa un tipo construido o un método genérico, el argumento de tipo se compara con las restricciones del parámetro de tipo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="7db9f-242">El argumento de tipo proporcionado debe cumplir las condiciones descritas en satisfacción de las [restricciones](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="7db9f-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="7db9f-243">Una restricción *class_type* debe cumplir las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="7db9f-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="7db9f-244">El tipo debe ser un tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-244">The type must be a class type.</span></span>
*  <span data-ttu-id="7db9f-245">El tipo no se debe `sealed`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="7db9f-246">El tipo no debe ser uno de los tipos siguientes: `System.Array`, `System.Delegate`, `System.Enum`o `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="7db9f-247">El tipo no se debe `object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-247">The type must not be `object`.</span></span> <span data-ttu-id="7db9f-248">Dado que todos los tipos se derivan de `object`, este tipo de restricción no tendría ningún efecto si se permitiera.</span><span class="sxs-lookup"><span data-stu-id="7db9f-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="7db9f-249">Como máximo, una restricción para un parámetro de tipo determinado puede ser un tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="7db9f-250">Un tipo especificado como restricción *interface_type* debe cumplir las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="7db9f-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="7db9f-251">El tipo debe ser un tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="7db9f-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="7db9f-252">Un tipo no debe especificarse más de una vez en una cláusula `where` determinada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="7db9f-253">En cualquier caso, la restricción puede incluir cualquiera de los parámetros de tipo del tipo o la declaración de método asociados como parte de un tipo construido, y puede implicar el tipo que se declara.</span><span class="sxs-lookup"><span data-stu-id="7db9f-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="7db9f-254">Cualquier tipo de clase o interfaz especificado como restricción de parámetro de tipo debe ser al menos igual de accesible ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)) que el tipo o método genérico que se está declarando.</span><span class="sxs-lookup"><span data-stu-id="7db9f-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="7db9f-255">Un tipo especificado como restricción *type_parameter* debe cumplir las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="7db9f-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="7db9f-256">El tipo debe ser un parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="7db9f-257">Un tipo no debe especificarse más de una vez en una cláusula `where` determinada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="7db9f-258">Además, no debe haber ningún ciclo en el gráfico de dependencias de los parámetros de tipo, donde Dependency es una relación transitiva definida por:</span><span class="sxs-lookup"><span data-stu-id="7db9f-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="7db9f-259">Si se usa un parámetro de tipo `T` como una restricción para el parámetro de tipo `S` `S` ***depende de*** `T`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="7db9f-260">Si un parámetro de tipo `S` depende de un parámetro de tipo `T` y `T` depende de un parámetro de tipo `U`, a continuación, `S` ***depende de*** `U`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="7db9f-261">Dada esta relación, se trata de un error en tiempo de compilación para que un parámetro de tipo dependa de sí mismo (directa o indirectamente).</span><span class="sxs-lookup"><span data-stu-id="7db9f-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="7db9f-262">Cualquier restricción debe ser coherente entre los parámetros de tipo dependiente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="7db9f-263">Si el parámetro de tipo `S` depende del parámetro de tipo `T` entonces:</span><span class="sxs-lookup"><span data-stu-id="7db9f-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="7db9f-264">`T` no deben tener la restricción de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="7db9f-265">De lo contrario, `T` se sella de forma eficaz, de modo que `S` se forzaría a ser del mismo tipo que `T`, lo que elimina la necesidad de dos parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="7db9f-266">Si `S` tiene la restricción de tipo de valor, `T` no debe tener una restricción de *class_type* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="7db9f-267">Si `S` tiene una restricción *class_type* `A` y `T` tiene una restricción *class_type* `B`, debe haber una conversión de identidad o una conversión de referencia implícita de `A` a `B` o una conversión de referencia implícita de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="7db9f-268">Si `S` también depende del parámetro de tipo `U` y `U` tiene una restricción *class_type* `A` y `T` tiene una restricción *class_type* `B`, debe haber una conversión de identidad o una conversión de referencia implícita de `A` a `B` o una conversión de referencia implícita de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="7db9f-269">Es válido para que `S` tenga la restricción de tipo de valor y `T` para tener la restricción de tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="7db9f-270">De hecho, esto limita `T` a los tipos `System.Object`, `System.ValueType`, `System.Enum`y cualquier tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="7db9f-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="7db9f-271">Si la cláusula `where` para un parámetro de tipo incluye una restricción de constructor (que tiene el formato `new()`), es posible utilizar el operador `new` para crear instancias del tipo ([expresiones de creación de objetos](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="7db9f-272">Cualquier argumento de tipo utilizado para un parámetro de tipo con una restricción de constructor debe tener un constructor sin parámetros público (este constructor existe implícitamente para cualquier tipo de valor) o ser un parámetro de tipo con la restricción de tipo de valor o la restricción de constructor (vea [restricciones de parámetro de tipo](classes.md#type-parameter-constraints) para obtener más detalles).</span><span class="sxs-lookup"><span data-stu-id="7db9f-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="7db9f-273">A continuación se muestran ejemplos de restricciones:</span><span class="sxs-lookup"><span data-stu-id="7db9f-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="7db9f-274">El ejemplo siguiente es erróneo porque provoca una circularidad en el gráfico de dependencias de los parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="7db9f-275">En los siguientes ejemplos se muestran situaciones no válidas adicionales:</span><span class="sxs-lookup"><span data-stu-id="7db9f-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="7db9f-276">La ***clase base efectiva*** de un parámetro de tipo `T` se define de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="7db9f-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="7db9f-277">Si `T` no tiene restricciones PRIMARY o restricciones de parámetro de tipo, se `object`su clase base efectiva.</span><span class="sxs-lookup"><span data-stu-id="7db9f-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="7db9f-278">Si `T` tiene la restricción de tipo de valor, se `System.ValueType`su clase base efectiva.</span><span class="sxs-lookup"><span data-stu-id="7db9f-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="7db9f-279">Si `T` tiene una restricción *class_type* `C` pero no *type_parameter* restricciones, se `C`su clase base efectiva.</span><span class="sxs-lookup"><span data-stu-id="7db9f-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="7db9f-280">Si `T` no tiene ninguna restricción de *class_type* pero tiene una o más restricciones de *type_parameter* , su clase base efectiva es el tipo más abarcado ([operadores de conversión de elevación](conversions.md#lifted-conversion-operators)) en el conjunto de clases base eficaces de sus restricciones de *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="7db9f-281">Las reglas de coherencia garantizan que exista un tipo más abarcado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="7db9f-282">Si `T` tiene una restricción de *class_type* y una o varias restricciones de *type_parameter* , su clase base efectiva es el tipo más abarcado (operadores de[conversión levantada](conversions.md#lifted-conversion-operators)) del conjunto que consta de la restricción *class_type* de `T` y las clases base eficaces de sus restricciones de *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="7db9f-283">Las reglas de coherencia garantizan que exista un tipo más abarcado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="7db9f-284">Si `T` tiene la restricción de tipo de referencia pero no *class_type* restricciones, se `object`su clase base efectiva.</span><span class="sxs-lookup"><span data-stu-id="7db9f-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="7db9f-285">En el caso de estas reglas, si T tiene una restricción `V` que es una *value_type*, use en su lugar el tipo base más específico de `V` que sea un *class_type*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="7db9f-286">Esto nunca puede ocurrir en una restricción explícitamente especificada, pero puede producirse cuando las restricciones de un método genérico se heredan implícitamente mediante una declaración de método de reemplazo o una implementación explícita de un método de interfaz.</span><span class="sxs-lookup"><span data-stu-id="7db9f-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="7db9f-287">Estas reglas garantizan que la clase base efectiva siempre sea una *class_type*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="7db9f-288">El ***conjunto de interfaces efectivo*** de un parámetro de tipo `T` se define de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="7db9f-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="7db9f-289">Si `T` no tiene *secondary_constraints*, su conjunto de interfaces efectivo está vacío.</span><span class="sxs-lookup"><span data-stu-id="7db9f-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="7db9f-290">Si `T` tiene *interface_type* restricciones pero no *type_parameter* restricciones, su conjunto de interfaces efectivo es su conjunto de restricciones de *interface_type* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="7db9f-291">Si `T` no tiene restricciones de *interface_type* pero tiene *type_parameter* restricciones, su conjunto de interfaces efectivo es la Unión de los conjuntos de interfaces eficaces de sus restricciones de *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="7db9f-292">Si `T` tiene restricciones de *interface_type* y restricciones de *type_parameter* , su conjunto de interfaces efectivo es la Unión de su conjunto de restricciones de *interface_type* y los conjuntos de interfaces eficaces de sus restricciones de *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="7db9f-293">Se sabe que un parámetro de tipo es ***un tipo de referencia*** si tiene la restricción de tipo de referencia o su clase base efectiva no es `object` o `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="7db9f-294">Los valores de un tipo de parámetro de tipo restringido se pueden utilizar para tener acceso a los miembros de instancia que implican las restricciones.</span><span class="sxs-lookup"><span data-stu-id="7db9f-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="7db9f-295">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-295">In the example</span></span>
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
<span data-ttu-id="7db9f-296">los métodos de `IPrintable` se pueden invocar directamente en `x` porque `T` está restringido a implementar siempre `IPrintable`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="7db9f-297">Cuerpo de clase</span><span class="sxs-lookup"><span data-stu-id="7db9f-297">Class body</span></span>

<span data-ttu-id="7db9f-298">El *class_body* de una clase define los miembros de esa clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="7db9f-299">Tipos parciales</span><span class="sxs-lookup"><span data-stu-id="7db9f-299">Partial types</span></span>

<span data-ttu-id="7db9f-300">Una declaración de tipos se puede dividir en varias ***declaraciones de tipos parciales***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="7db9f-301">La declaración de tipos se construye a partir de sus elementos siguiendo las reglas de esta sección, en la que se trata como una única declaración durante el resto del procesamiento en tiempo de compilación y en tiempo de ejecución del programa.</span><span class="sxs-lookup"><span data-stu-id="7db9f-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="7db9f-302">Un *class_declaration*, *struct_declaration* o *interface_declaration* representa una declaración de tipos parciales si incluye un modificador `partial`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="7db9f-303">`partial` no es una palabra clave y solo actúa como modificador si aparece inmediatamente antes de que una de las palabras clave `class`, `struct` o `interface` en una declaración de tipos o antes del tipo `void` en una declaración de método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="7db9f-304">En otros contextos, se puede usar como un identificador normal.</span><span class="sxs-lookup"><span data-stu-id="7db9f-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="7db9f-305">Cada parte de una declaración de tipo parcial debe incluir un modificador `partial`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="7db9f-306">Debe tener el mismo nombre y estar declarado en el mismo espacio de nombres o declaración de tipos que las demás partes.</span><span class="sxs-lookup"><span data-stu-id="7db9f-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="7db9f-307">El modificador `partial` indica que pueden existir partes adicionales de la declaración de tipos en otro lugar, pero la existencia de tales partes adicionales no es un requisito; es válido para un tipo con una única declaración para incluir el modificador `partial`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="7db9f-308">Todas las partes de un tipo parcial se deben compilar de forma que se puedan combinar las partes en tiempo de compilación en una única declaración de tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="7db9f-309">Los tipos parciales no permiten que se extiendan los tipos ya compilados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="7db9f-310">Los tipos anidados se pueden declarar en varias partes mediante el modificador `partial`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="7db9f-311">Normalmente, el tipo contenedor se declara utilizando también `partial`, y cada parte del tipo anidado se declara en una parte diferente del tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="7db9f-312">No se permite el modificador `partial` en las declaraciones de delegado o de enumeración.</span><span class="sxs-lookup"><span data-stu-id="7db9f-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="7db9f-313">Atributos</span><span class="sxs-lookup"><span data-stu-id="7db9f-313">Attributes</span></span>

<span data-ttu-id="7db9f-314">Los atributos de un tipo parcial se determinan mediante la combinación, en un orden no especificado, con los atributos de cada uno de los elementos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="7db9f-315">Si un atributo se coloca en varias partes, es equivalente a especificar el atributo varias veces en el tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="7db9f-316">Por ejemplo, las dos partes:</span><span class="sxs-lookup"><span data-stu-id="7db9f-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="7db9f-317">son equivalentes a una declaración como:</span><span class="sxs-lookup"><span data-stu-id="7db9f-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="7db9f-318">Los atributos de los parámetros de tipo se combinan de manera similar.</span><span class="sxs-lookup"><span data-stu-id="7db9f-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="7db9f-319">Modificadores</span><span class="sxs-lookup"><span data-stu-id="7db9f-319">Modifiers</span></span>

<span data-ttu-id="7db9f-320">Cuando una declaración de tipos parciales incluye una especificación de accesibilidad (los modificadores `public`, `protected`, `internal`y `private`) debe coincidir con todas las demás partes que incluyen una especificación de accesibilidad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="7db9f-321">Si ninguna parte de un tipo parcial incluye una especificación de accesibilidad, se proporciona al tipo la accesibilidad predeterminada adecuada (la[accesibilidad declarada](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="7db9f-322">Si una o varias declaraciones parciales de un tipo anidado incluyen un modificador `new`, no se genera ninguna advertencia si el tipo anidado oculta un miembro heredado ([ocultando a través](basic-concepts.md#hiding-through-inheritance)de la herencia).</span><span class="sxs-lookup"><span data-stu-id="7db9f-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="7db9f-323">Si una o varias declaraciones parciales de una clase incluyen un modificador `abstract`, la clase se considera abstracta ([clases abstractas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="7db9f-324">De lo contrario, la clase se considera no abstracta.</span><span class="sxs-lookup"><span data-stu-id="7db9f-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="7db9f-325">Si una o varias declaraciones parciales de una clase incluyen un modificador `sealed`, la clase se considera Sealed ([clases selladas](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="7db9f-326">De lo contrario, se considera que la clase no está sellada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="7db9f-327">Tenga en cuenta que una clase no puede ser Abstract y Sealed.</span><span class="sxs-lookup"><span data-stu-id="7db9f-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="7db9f-328">Cuando el modificador `unsafe` se usa en una declaración de tipo parcial, solo esa parte concreta se considera un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="7db9f-329">Parámetros de tipo y restricciones</span><span class="sxs-lookup"><span data-stu-id="7db9f-329">Type parameters and constraints</span></span>

<span data-ttu-id="7db9f-330">Si un tipo genérico se declara en varias partes, cada elemento debe indicar los parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="7db9f-331">Cada elemento debe tener el mismo número de parámetros de tipo y el mismo nombre para cada parámetro de tipo, en orden.</span><span class="sxs-lookup"><span data-stu-id="7db9f-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="7db9f-332">Cuando una declaración de tipos genéricos parciales incluye restricciones (cláusulas`where`), las restricciones deben coincidir con todas las demás partes que incluyen restricciones.</span><span class="sxs-lookup"><span data-stu-id="7db9f-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="7db9f-333">En concreto, cada parte que incluye restricciones debe tener restricciones para el mismo conjunto de parámetros de tipo y, para cada parámetro de tipo, los conjuntos de restricciones PRIMARY, Secondary y constructor deben ser equivalentes.</span><span class="sxs-lookup"><span data-stu-id="7db9f-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="7db9f-334">Dos conjuntos de restricciones son equivalentes si contienen los mismos miembros.</span><span class="sxs-lookup"><span data-stu-id="7db9f-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="7db9f-335">Si ninguna parte de un tipo genérico parcial especifica las restricciones de parámetro de tipo, los parámetros de tipo se consideran no restringidos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="7db9f-336">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-336">The example</span></span>
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
<span data-ttu-id="7db9f-337">es correcto porque las partes que incluyen restricciones (las dos primeras) especifican de forma eficaz el mismo conjunto de restricciones principales, secundarias y constructores para el mismo conjunto de parámetros de tipo, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="7db9f-338">Clase base</span><span class="sxs-lookup"><span data-stu-id="7db9f-338">Base class</span></span>

<span data-ttu-id="7db9f-339">Cuando una declaración de clase parcial incluye una especificación de clase base, debe coincidir con todas las demás partes que incluyen una especificación de clase base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="7db9f-340">Si ninguna parte de una clase parcial incluye una especificación de clase base, la clase base se convierte en `System.Object` ([clases base](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="7db9f-341">Interfaces base</span><span class="sxs-lookup"><span data-stu-id="7db9f-341">Base interfaces</span></span>

<span data-ttu-id="7db9f-342">El conjunto de interfaces base para un tipo declarado en varias partes es la Unión de las interfaces base especificadas en cada parte.</span><span class="sxs-lookup"><span data-stu-id="7db9f-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="7db9f-343">Solo se puede asignar un nombre a una interfaz base determinada una vez en cada parte, pero se permite que varias partes denominen a las mismas interfaces base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="7db9f-344">Solo debe haber una implementación de los miembros de una interfaz base determinada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="7db9f-345">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="7db9f-346">el conjunto de interfaces base para `C` de clase es `IA`, `IB`y `IC`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="7db9f-347">Normalmente, cada elemento proporciona una implementación de las interfaces declaradas en esa parte; sin embargo, esto no es un requisito.</span><span class="sxs-lookup"><span data-stu-id="7db9f-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="7db9f-348">Un elemento puede proporcionar la implementación de una interfaz declarada en una parte diferente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="7db9f-349">Miembros</span><span class="sxs-lookup"><span data-stu-id="7db9f-349">Members</span></span>

<span data-ttu-id="7db9f-350">A excepción de los métodos parciales ([métodos parciales](classes.md#partial-methods)), el conjunto de miembros de un tipo declarado en varias partes es simplemente la Unión del conjunto de miembros declarado en cada parte.</span><span class="sxs-lookup"><span data-stu-id="7db9f-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="7db9f-351">Los cuerpos de todas las partes de la declaración de tipos comparten el mismo espacio de declaración ([declaraciones](basic-concepts.md#declarations)), y el ámbito de cada miembro ([ámbitos](basic-concepts.md#scopes)) se extiende a los cuerpos de todas las partes.</span><span class="sxs-lookup"><span data-stu-id="7db9f-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="7db9f-352">El dominio de accesibilidad de cualquier miembro siempre incluye todas las partes del tipo envolvente; un miembro de `private` declarado en una parte es accesible libremente desde otra parte.</span><span class="sxs-lookup"><span data-stu-id="7db9f-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="7db9f-353">Es un error en tiempo de compilación declarar el mismo miembro en más de una parte del tipo, a menos que ese miembro sea un tipo con el modificador `partial`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="7db9f-354">El orden de los miembros dentro de un tipo rara vez C# es significativo para el código, pero puede ser importante al interactuar con otros lenguajes y entornos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="7db9f-355">En estos casos, el orden de los miembros dentro de un tipo declarado en varias partes es indefinido.</span><span class="sxs-lookup"><span data-stu-id="7db9f-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="7db9f-356">Métodos parciales</span><span class="sxs-lookup"><span data-stu-id="7db9f-356">Partial methods</span></span>

<span data-ttu-id="7db9f-357">Los métodos parciales se pueden definir en una parte de una declaración de tipos e implementarse en otro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="7db9f-358">La implementación es opcional. Si ninguna parte implementa el método parcial, la declaración de método parcial y todas las llamadas a ella se quitan de la declaración de tipos resultante de la combinación de los elementos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="7db9f-359">Los métodos parciales no pueden definir modificadores de acceso, pero implícitamente `private`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="7db9f-360">Su tipo de valor devuelto debe ser `void`y sus parámetros no pueden tener el modificador `out`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="7db9f-361">El identificador `partial` se reconoce como una palabra clave especial en una declaración de método solo si aparece justo antes del tipo de `void`; de lo contrario, se puede usar como un identificador normal.</span><span class="sxs-lookup"><span data-stu-id="7db9f-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="7db9f-362">Un método parcial no puede implementar explícitamente métodos de interfaz.</span><span class="sxs-lookup"><span data-stu-id="7db9f-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="7db9f-363">Hay dos tipos de declaraciones de método parcial: Si el cuerpo de la declaración del método es un punto y coma, se dice que la declaración es una ***declaración de método parcial de definición***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="7db9f-364">Si el cuerpo se proporciona como un *bloque*, se dice que la declaración es una ***declaración de método parcial de implementación***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="7db9f-365">En las partes de una declaración de tipos solo puede haber una declaración de método parcial de definición con una firma determinada, y solo puede haber una declaración de método parcial de implementación con una firma determinada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="7db9f-366">Si se proporciona una declaración de método parcial de implementación, debe existir una declaración de método parcial de definición correspondiente, y las declaraciones deben coincidir como se especifica en lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="7db9f-367">Las declaraciones deben tener los mismos modificadores (aunque no necesariamente en el mismo orden), el nombre del método, el número de parámetros de tipo y el número de parámetros.</span><span class="sxs-lookup"><span data-stu-id="7db9f-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="7db9f-368">Los parámetros correspondientes en las declaraciones deben tener los mismos modificadores (aunque no necesariamente en el mismo orden) y los mismos tipos (diferencias de módulo en los nombres de parámetros de tipo).</span><span class="sxs-lookup"><span data-stu-id="7db9f-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="7db9f-369">Los parámetros de tipo correspondientes en las declaraciones deben tener las mismas restricciones (diferencias de módulo en los nombres de parámetros de tipo).</span><span class="sxs-lookup"><span data-stu-id="7db9f-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="7db9f-370">Una declaración de método parcial de implementación puede aparecer en la misma parte que la declaración de método parcial de definición correspondiente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="7db9f-371">Solo un método parcial de definición participa en la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="7db9f-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="7db9f-372">Por lo tanto, tanto si se proporciona una declaración de implementación como si no, las expresiones de invocación pueden resolverse en invocaciones del método parcial.</span><span class="sxs-lookup"><span data-stu-id="7db9f-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="7db9f-373">Dado que un método parcial siempre devuelve `void`, estas expresiones de invocación siempre serán instrucciones de expresión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="7db9f-374">Además, dado que un método parcial se `private`implícitamente, estas instrucciones siempre se producirán dentro de una de las partes de la declaración de tipos en la que se declara el método parcial.</span><span class="sxs-lookup"><span data-stu-id="7db9f-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="7db9f-375">Si ninguna parte de una declaración de tipo parcial contiene una declaración de implementación para un método parcial determinado, cualquier instrucción de expresión que lo invoque simplemente se quita de la declaración de tipos combinados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="7db9f-376">Por lo tanto, la expresión de invocación, incluidas las expresiones constituyentes, no tiene ningún efecto en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7db9f-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="7db9f-377">También se quita el método parcial en sí y no será miembro de la declaración de tipos combinados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="7db9f-378">Si existe una declaración de implementación para un método parcial determinado, se conservan las invocaciones de los métodos parciales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="7db9f-379">El método parcial da lugar a una declaración de método similar a la declaración de método parcial de implementación, excepto para lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="7db9f-380">No se incluye el modificador `partial`</span><span class="sxs-lookup"><span data-stu-id="7db9f-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="7db9f-381">Los atributos de la declaración del método resultante son los atributos combinados de la definición de y la declaración de método parcial de implementación en un orden no especificado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="7db9f-382">No se quitan los duplicados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="7db9f-383">Los atributos de los parámetros de la declaración de método resultante son los atributos combinados de los parámetros correspondientes de la definición de y la declaración de método parcial de implementación en un orden no especificado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="7db9f-384">No se quitan los duplicados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-384">Duplicates are not removed.</span></span>

<span data-ttu-id="7db9f-385">Si se proporciona una declaración de definición pero no una declaración de implementación para un método parcial M, se aplican las restricciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="7db9f-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="7db9f-386">Es un error en tiempo de compilación crear un delegado para el método ([expresiones de creación de delegado](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="7db9f-387">Es un error en tiempo de compilación hacer referencia a `M` dentro de una función anónima que se convierte en un tipo de árbol[de expresión (evaluación de conversiones de funciones anónimas a tipos de árbol de expresión](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="7db9f-388">Las expresiones que se producen como parte de una invocación de `M` no afectan al estado de asignación definitiva ([asignación definitiva](variables.md#definite-assignment)), lo que puede provocar errores en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="7db9f-389">`M` no puede ser el punto de entrada de una aplicación (inicio de la[aplicación](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="7db9f-390">Los métodos parciales son útiles para permitir que una parte de una declaración de tipos Personalice el comportamiento de otra parte, por ejemplo, una generada por una herramienta.</span><span class="sxs-lookup"><span data-stu-id="7db9f-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="7db9f-391">Considere la siguiente declaración de clase parcial:</span><span class="sxs-lookup"><span data-stu-id="7db9f-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="7db9f-392">Si esta clase se compila sin ningún otro elemento, se quitarán las declaraciones de método parcial de definición y sus invocaciones, y la declaración de clase combinada resultante será equivalente a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="7db9f-393">No obstante, supongamos que se proporciona otra parte, que proporciona declaraciones de implementación de los métodos parciales:</span><span class="sxs-lookup"><span data-stu-id="7db9f-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="7db9f-394">A continuación, la declaración de clase combinada resultante será equivalente a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="7db9f-395">Enlace de nombre</span><span class="sxs-lookup"><span data-stu-id="7db9f-395">Name binding</span></span>

<span data-ttu-id="7db9f-396">Aunque cada parte de un tipo extensible se debe declarar dentro del mismo espacio de nombres, las partes se escriben normalmente en diferentes declaraciones de espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="7db9f-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="7db9f-397">Por lo tanto, pueden estar presentes directivas de `using` diferentes ([directivas de uso](namespaces.md#using-directives)) para cada parte.</span><span class="sxs-lookup"><span data-stu-id="7db9f-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="7db9f-398">Al interpretar nombres simples ([inferencia de tipos](expressions.md#type-inference)) dentro de una parte, solo se tienen en cuenta las directivas de `using` de las declaraciones de espacio de nombres que la forman.</span><span class="sxs-lookup"><span data-stu-id="7db9f-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="7db9f-399">Esto puede dar lugar a que el mismo identificador tenga significados diferentes en distintas partes:</span><span class="sxs-lookup"><span data-stu-id="7db9f-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="7db9f-400">Miembros de la clase</span><span class="sxs-lookup"><span data-stu-id="7db9f-400">Class members</span></span>

<span data-ttu-id="7db9f-401">Los miembros de una clase se componen de los miembros introducidos por su *class_member_declaration*s y los miembros heredados de la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="7db9f-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="7db9f-402">Los miembros de un tipo de clase se dividen en las siguientes categorías:</span><span class="sxs-lookup"><span data-stu-id="7db9f-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="7db9f-403">Constantes, que representan valores constantes asociados a la clase ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="7db9f-404">Campos, que son las variables de la clase ([campos](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="7db9f-405">Los métodos, que implementan los cálculos y las acciones que puede realizar la clase ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="7db9f-406">Propiedades, que definen las características con nombre y las acciones asociadas a la lectura y escritura de esas características ([propiedades](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="7db9f-407">Eventos, que definen las notificaciones que puede generar la clase ([eventos](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="7db9f-408">Indexadores, que permiten indizar las instancias de la clase de la misma manera (sintácticamente) que las matrices ([indizadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="7db9f-409">Operadores, que definen los operadores de expresión que se pueden aplicar a las instancias de la clase ([operadores](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="7db9f-410">Constructores de instancias, que implementan las acciones necesarias para inicializar las instancias de la clase ([constructores de instancia](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="7db9f-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="7db9f-411">Destructores, que implementan las acciones que se deben realizar antes de que las instancias de la clase se descartan de forma permanente ([destructores](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="7db9f-412">Constructores estáticos, que implementan las acciones necesarias para inicializar la propia clase ([constructores estáticos](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="7db9f-413">Tipos, que representan los tipos locales de la clase ([tipos anidados](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="7db9f-414">Los miembros que pueden contener código ejecutable se conocen colectivamente como *miembros de función* del tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="7db9f-415">Los miembros de función de un tipo de clase son los métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos de ese tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="7db9f-416">Un *class_declaration* crea un nuevo espacio de declaración ([declaraciones](basic-concepts.md#declarations)) y el *class_member_declaration*s que contiene inmediatamente el *class_declaration* introduce nuevos miembros en este espacio de declaración.</span><span class="sxs-lookup"><span data-stu-id="7db9f-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="7db9f-417">Las siguientes reglas se aplican a *class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="7db9f-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="7db9f-418">Los constructores de instancias, destructores y constructores estáticos deben tener el mismo nombre que la clase de inclusión inmediata.</span><span class="sxs-lookup"><span data-stu-id="7db9f-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="7db9f-419">Todos los demás miembros deben tener nombres distintos del nombre de la clase de inclusión inmediata.</span><span class="sxs-lookup"><span data-stu-id="7db9f-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="7db9f-420">El nombre de una constante, un campo, una propiedad, un evento o un tipo debe ser distinto de los nombres de todos los demás miembros declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="7db9f-421">El nombre de un método debe ser distinto de los nombres de todos los demás métodos que no se declaran en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="7db9f-422">Además, la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un método debe ser diferente de las firmas de todos los demás métodos declarados en la misma clase, y dos métodos declarados en la misma clase no pueden tener firmas que solo se diferencien por `ref` y `out`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="7db9f-423">La firma de un constructor de instancia debe ser diferente de las firmas de todos los demás constructores de instancia declaradas en la misma clase, y dos constructores declarados en la misma clase no pueden tener firmas que solo difieran en `ref` y `out`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="7db9f-424">La firma de un indizador debe ser diferente de las firmas de todos los demás indexadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="7db9f-425">La firma de un operador debe ser diferente de las firmas de todos los demás operadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="7db9f-426">Los miembros heredados de un tipo de clase ([herencia](classes.md#inheritance)) no forman parte del espacio de declaración de una clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="7db9f-427">Por lo tanto, una clase derivada puede declarar un miembro con el mismo nombre o signatura que un miembro heredado (que en efecto oculta el miembro heredado).</span><span class="sxs-lookup"><span data-stu-id="7db9f-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="7db9f-428">El tipo de instancia</span><span class="sxs-lookup"><span data-stu-id="7db9f-428">The instance type</span></span>

<span data-ttu-id="7db9f-429">Cada declaración de clase tiene un tipo enlazado asociado ([tipos enlazados y sin enlazar](types.md#bound-and-unbound-types)), el ***tipo de instancia***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="7db9f-430">En el caso de una declaración de clase genérica, el tipo de instancia se forma creando un tipo construido ([tipos construidos](types.md#constructed-types)) a partir de la declaración de tipos, y cada uno de los argumentos de tipo proporcionados es el parámetro de tipo correspondiente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="7db9f-431">Dado que el tipo de instancia utiliza los parámetros de tipo, solo se puede usar cuando los parámetros de tipo están en el ámbito; es decir, dentro de la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="7db9f-432">El tipo de instancia es el tipo de `this` para el código escrito dentro de la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="7db9f-433">En el caso de las clases no genéricas, el tipo de instancia es simplemente la clase declarada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="7db9f-434">A continuación se muestran varias declaraciones de clase junto con sus tipos de instancia:</span><span class="sxs-lookup"><span data-stu-id="7db9f-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="7db9f-435">Miembros de tipos construidos</span><span class="sxs-lookup"><span data-stu-id="7db9f-435">Members of constructed types</span></span>

<span data-ttu-id="7db9f-436">Los miembros no heredados de un tipo construido se obtienen sustituyendo, por cada *type_parameter* en la declaración de miembro, el *type_argument* correspondiente del tipo construido.</span><span class="sxs-lookup"><span data-stu-id="7db9f-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="7db9f-437">El proceso de sustitución se basa en el significado semántico de las declaraciones de tipos y no es simplemente una sustitución textual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="7db9f-438">Por ejemplo, dada la declaración de clase genérica</span><span class="sxs-lookup"><span data-stu-id="7db9f-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="7db9f-439">el tipo construido `Gen<int[],IComparable<string>>` tiene los siguientes miembros:</span><span class="sxs-lookup"><span data-stu-id="7db9f-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="7db9f-440">El tipo del miembro `a` en la declaración de clase genérica `Gen` es "matriz bidimensional de `T`", por lo que el tipo del miembro `a` en el tipo construido anterior es "matriz bidimensional de matriz unidimensional de `int`" o `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="7db9f-441">Dentro de los miembros de la función de instancia, el tipo de `this` es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la declaración contenedora.</span><span class="sxs-lookup"><span data-stu-id="7db9f-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="7db9f-442">Todos los miembros de una clase genérica pueden usar parámetros de tipo de cualquier clase envolvente, ya sea directamente o como parte de un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="7db9f-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="7db9f-443">Cuando se usa un tipo construido cerrado determinado ([tipos abiertos y cerrados](types.md#open-and-closed-types)) en tiempo de ejecución, cada uso de un parámetro de tipo se reemplaza por el argumento de tipo real proporcionado al tipo construido.</span><span class="sxs-lookup"><span data-stu-id="7db9f-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="7db9f-444">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="7db9f-445">Herencia</span><span class="sxs-lookup"><span data-stu-id="7db9f-445">Inheritance</span></span>

<span data-ttu-id="7db9f-446">Una clase ***hereda*** los miembros de su tipo de clase base directa.</span><span class="sxs-lookup"><span data-stu-id="7db9f-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="7db9f-447">La herencia significa que una clase contiene implícitamente todos los miembros de su tipo de clase base directa, a excepción de los constructores de instancias, destructores y constructores estáticos de la clase base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="7db9f-448">Algunos aspectos importantes de la herencia son:</span><span class="sxs-lookup"><span data-stu-id="7db9f-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="7db9f-449">La herencia es transitiva.</span><span class="sxs-lookup"><span data-stu-id="7db9f-449">Inheritance is transitive.</span></span> <span data-ttu-id="7db9f-450">Si `C` se deriva de `B`y `B` se deriva de `A`, `C` hereda los miembros declarados en `B` así como los miembros declarados en `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="7db9f-451">Una clase derivada extiende su clase base directa.</span><span class="sxs-lookup"><span data-stu-id="7db9f-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="7db9f-452">Una clase derivada puede agregar nuevos miembros a aquellos de los que hereda, pero no puede quitar la definición de un miembro heredado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="7db9f-453">Los constructores de instancias, destructores y constructores estáticos no se heredan, pero todos los demás miembros son, independientemente de su accesibilidad declarada ([acceso a miembros](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="7db9f-454">Sin embargo, en función de la accesibilidad declarada, es posible que los miembros heredados no estén accesibles en una clase derivada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="7db9f-455">Una clase derivada puede ***ocultar*** ([ocultar a través](basic-concepts.md#hiding-through-inheritance)de la herencia) los miembros heredados declarando nuevos miembros con el mismo nombre o signatura.</span><span class="sxs-lookup"><span data-stu-id="7db9f-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="7db9f-456">Sin embargo, tenga en cuenta que ocultar un miembro heredado no quita ese miembro; simplemente hace que ese miembro no sea accesible directamente a través de la clase derivada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="7db9f-457">Una instancia de una clase contiene un conjunto de todos los campos de instancia declarados en la clase y sus clases base, y existe una conversión implícita ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions)) de un tipo de clase derivada a cualquiera de sus tipos de clase base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="7db9f-458">Por lo tanto, una referencia a una instancia de alguna clase derivada se puede tratar como una referencia a una instancia de cualquiera de sus clases base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="7db9f-459">Una clase puede declarar métodos virtuales, propiedades e indizadores, y las clases derivadas pueden invalidar la implementación de estos miembros de función.</span><span class="sxs-lookup"><span data-stu-id="7db9f-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="7db9f-460">Esto permite que las clases muestren un comportamiento polimórfico en el que las acciones realizadas por una invocación de miembro de función varían en función del tipo en tiempo de ejecución de la instancia a través de la cual se invoca a ese miembro de función.</span><span class="sxs-lookup"><span data-stu-id="7db9f-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="7db9f-461">El miembro heredado de un tipo de clase construido son los miembros del tipo de clase base inmediato ([clases base](classes.md#base-classes)), que se encuentra sustituyendo los argumentos de tipo del tipo construido por cada aparición de los parámetros de tipo correspondientes en la especificación *class_base* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="7db9f-462">Estos miembros, a su vez, se transforman sustituyendo, por cada *type_parameter* en la declaración de miembro, el *type_argument* correspondiente de la especificación de *class_base* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="7db9f-463">En el ejemplo anterior, el tipo construido `D<int>` tiene un miembro no heredado `public int G(string s)` obtener sustituyendo el argumento de tipo `int` por el parámetro de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="7db9f-464">`D<int>` también tiene un miembro heredado de la declaración de clase `B`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="7db9f-465">Este miembro heredado se determina determinando primero el tipo de clase base `B<int[]>` de `D<int>` sustituyendo `int` por `T` en la especificación de clase base `B<T[]>`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="7db9f-466">A continuación, como un argumento de tipo para `B`, `int[]` se sustituye por `U` en `public U F(long index)`, lo que produce el miembro heredado `public int[] F(long index)`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="7db9f-467">El nuevo modificador</span><span class="sxs-lookup"><span data-stu-id="7db9f-467">The new modifier</span></span>

<span data-ttu-id="7db9f-468">Un *class_member_declaration* puede declarar un miembro con el mismo nombre o signatura que un miembro heredado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="7db9f-469">Cuando esto ocurre, se dice que el miembro de la clase derivada ***oculta*** el miembro de la clase base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="7db9f-470">Ocultar un miembro heredado no se considera un error, pero hace que el compilador emita una advertencia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="7db9f-471">Para suprimir la advertencia, la declaración del miembro de la clase derivada puede incluir un modificador `new` para indicar que el miembro derivado está pensado para ocultar el miembro base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="7db9f-472">Este tema se describe con más detalle en [ocultarse a través](basic-concepts.md#hiding-through-inheritance)de la herencia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="7db9f-473">Si se incluye un modificador `new` en una declaración que no oculta un miembro heredado, se emite una advertencia para ese efecto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="7db9f-474">Esta advertencia se suprime quitando el modificador `new`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="7db9f-475">Modificadores de acceso</span><span class="sxs-lookup"><span data-stu-id="7db9f-475">Access modifiers</span></span>

<span data-ttu-id="7db9f-476">Un *class_member_declaration* puede tener cualquiera de los cinco tipos posibles de accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`o `private`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="7db9f-477">A excepción de la combinación de `protected internal`, se trata de un error en tiempo de compilación para especificar más de un modificador de acceso.</span><span class="sxs-lookup"><span data-stu-id="7db9f-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="7db9f-478">Cuando una *class_member_declaration* no incluye modificadores de acceso, se supone `private`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="7db9f-479">Tipos constituyentes</span><span class="sxs-lookup"><span data-stu-id="7db9f-479">Constituent types</span></span>

<span data-ttu-id="7db9f-480">Los tipos que se usan en la declaración de un miembro se denominan tipos constituyentes de ese miembro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="7db9f-481">Los tipos constituyentes posibles son el tipo de una constante, un campo, una propiedad, un evento o un indizador, el tipo de valor devuelto de un método o un operador, y los tipos de parámetro de un método, indizador, operador o constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="7db9f-482">Los tipos constituyentes de un miembro deben ser al menos tan accesibles como el propio miembro ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="7db9f-483">Miembros estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="7db9f-483">Static and instance members</span></span>

<span data-ttu-id="7db9f-484">Los miembros de una clase son ***miembros estáticos*** o ***miembros de instancia***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="7db9f-485">En general, resulta útil pensar en que los miembros estáticos pertenecen a los tipos de clase y a los miembros de instancia como pertenecientes a objetos (instancias de tipos de clase).</span><span class="sxs-lookup"><span data-stu-id="7db9f-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="7db9f-486">Cuando un campo, método, propiedad, evento, operador o declaración de constructor incluye un modificador `static`, declara un miembro estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="7db9f-487">Además, una declaración de constante o de tipo declara implícitamente un miembro estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="7db9f-488">Los miembros estáticos tienen las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="7db9f-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="7db9f-489">Cuando se hace referencia a un miembro estático `M` en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, `E` debe indicar un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="7db9f-490">Se trata de un error en tiempo de compilación para que `E` denote una instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="7db9f-491">Un campo estático identifica exactamente una ubicación de almacenamiento que compartirán todas las instancias de un tipo de clase cerrada determinado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="7db9f-492">Independientemente del número de instancias de un tipo de clase cerrada determinado que se creen, solo hay una copia de un campo estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="7db9f-493">Un miembro de función estático (método, propiedad, evento, operador o constructor) no funciona en una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en este tipo de miembro de función.</span><span class="sxs-lookup"><span data-stu-id="7db9f-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="7db9f-494">Cuando un campo, un método, una propiedad, un evento, un indexador, un constructor o una declaración de destructor no incluye un modificador `static`, declara un miembro de instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="7db9f-495">(Un miembro de instancia se denomina a veces miembro no estático). Los miembros de instancia tienen las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="7db9f-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="7db9f-496">Cuando se hace referencia a un miembro de instancia `M` en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, `E` debe indicar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="7db9f-497">Es un error en tiempo de enlace para que `E` denote un tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="7db9f-498">Cada instancia de una clase contiene un conjunto independiente de todos los campos de instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="7db9f-499">Un miembro de función de instancia (método, propiedad, indexador, constructor de instancia o destructor) opera en una instancia determinada de la clase y se puede tener acceso a esta instancia como `this` ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="7db9f-500">En el ejemplo siguiente se muestran las reglas para tener acceso a los miembros estáticos y de instancia:</span><span class="sxs-lookup"><span data-stu-id="7db9f-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="7db9f-501">El método `F` muestra que en un miembro de función de instancia, se puede usar un *simple_name* ([nombres simples](expressions.md#simple-names)) para tener acceso a los miembros de instancia y a los miembros estáticos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="7db9f-502">El método `G` muestra que en un miembro de función estático, es un error en tiempo de compilación para tener acceso a un miembro de instancia a través de un *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="7db9f-503">El método `Main` muestra que en un *member_access* ([acceso a miembros](expressions.md#member-access)), se debe tener acceso a los miembros de instancia a través de instancias de y se debe tener acceso a los miembros estáticos a través de los tipos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="7db9f-504">Tipos anidados</span><span class="sxs-lookup"><span data-stu-id="7db9f-504">Nested types</span></span>

<span data-ttu-id="7db9f-505">Un tipo declarado dentro de una declaración de clase o struct se denomina ***tipo anidado***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="7db9f-506">Un tipo que se declara dentro de una unidad de compilación o espacio de nombres se denomina ***tipo no anidado***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="7db9f-507">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-507">In the example</span></span>
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
<span data-ttu-id="7db9f-508">la clase `B` es un tipo anidado porque se declara dentro de la clase `A`y la clase `A` es un tipo no anidado porque se declara dentro de una unidad de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="7db9f-509">Nombre completo</span><span class="sxs-lookup"><span data-stu-id="7db9f-509">Fully qualified name</span></span>

<span data-ttu-id="7db9f-510">El nombre completo ([nombres](basic-concepts.md#fully-qualified-names)completos) de un tipo anidado es `S.N` donde `S` es el nombre completo del tipo en el que se declara `N` de tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="7db9f-511">Accesibilidad declarada</span><span class="sxs-lookup"><span data-stu-id="7db9f-511">Declared accessibility</span></span>

<span data-ttu-id="7db9f-512">Los tipos no anidados pueden tener `public` o `internal` la accesibilidad declarada y tienen `internal` accesibilidad declarada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="7db9f-513">Los tipos anidados también pueden tener estas formas de accesibilidad declarada, además de una o varias formas adicionales de accesibilidad declarada, dependiendo de si el tipo contenedor es una clase o un struct:</span><span class="sxs-lookup"><span data-stu-id="7db9f-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="7db9f-514">Un tipo anidado que se declara en una clase puede tener cualquiera de las cinco formas de accesibilidad declarada (`public`, `protected internal`, `protected`, `internal`o `private`) y, al igual que otros miembros de clase, tiene como valor predeterminado `private` accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="7db9f-515">Un tipo anidado que se declara en un struct puede tener cualquiera de las tres formas de accesibilidad declarada (`public`, `internal`o `private`) y, al igual que otros miembros de struct, tiene como valor predeterminado `private` accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="7db9f-516">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-516">The example</span></span>
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
<span data-ttu-id="7db9f-517">declara una clase anidada privada `Node`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="7db9f-518">Conde</span><span class="sxs-lookup"><span data-stu-id="7db9f-518">Hiding</span></span>

<span data-ttu-id="7db9f-519">Un tipo anidado puede ocultar ([ocultar nombres](basic-concepts.md#name-hiding)) un miembro base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="7db9f-520">El modificador `new` se permite en las declaraciones de tipos anidados para que la ocultación se pueda expresar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="7db9f-521">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-521">The example</span></span>
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
<span data-ttu-id="7db9f-522">muestra una `M` de clase anidada que oculta el método `M` definido en `Base`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="7db9f-523">Este acceso</span><span class="sxs-lookup"><span data-stu-id="7db9f-523">this access</span></span>

<span data-ttu-id="7db9f-524">Un tipo anidado y su tipo contenedor no tienen una relación especial con respecto a *this_access* ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="7db9f-525">En concreto, `this` dentro de un tipo anidado no se puede usar para hacer referencia a los miembros de instancia del tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="7db9f-526">En los casos en los que un tipo anidado necesita tener acceso a los miembros de instancia de su tipo contenedor, se puede proporcionar acceso proporcionando el `this` para la instancia del tipo contenedor como argumento de constructor para el tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="7db9f-527">En el ejemplo siguiente</span><span class="sxs-lookup"><span data-stu-id="7db9f-527">The following example</span></span>
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
<span data-ttu-id="7db9f-528">muestra esta técnica.</span><span class="sxs-lookup"><span data-stu-id="7db9f-528">shows this technique.</span></span> <span data-ttu-id="7db9f-529">Una instancia de `C` crea una instancia de `Nested` y pasa su propia `this` al constructor de `Nested`para proporcionar el acceso posterior a los miembros de la instancia de `C`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="7db9f-530">Acceso a los miembros privados y protegidos del tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="7db9f-531">Un tipo anidado tiene acceso a todos los miembros a los que se puede tener acceso a su tipo contenedor, incluidos los miembros del tipo contenedor que tienen `private` y `protected` la accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="7db9f-532">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-532">The example</span></span>
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
<span data-ttu-id="7db9f-533">muestra una clase `C` que contiene una clase anidada `Nested`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="7db9f-534">Dentro de `Nested`, el método `G` llama al método estático `F` definido en `C`y `F` tiene una accesibilidad declarada privada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="7db9f-535">Un tipo anidado también puede tener acceso a los miembros protegidos definidos en un tipo base de su tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="7db9f-536">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-536">In the example</span></span>
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
<span data-ttu-id="7db9f-537">la clase anidada `Derived.Nested` tiene acceso al método protegido `F` definido en la clase base de `Derived`, `Base`, llamando a a través de una instancia de `Derived`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="7db9f-538">Tipos anidados en clases genéricas</span><span class="sxs-lookup"><span data-stu-id="7db9f-538">Nested types in generic classes</span></span>

<span data-ttu-id="7db9f-539">Una declaración de clase genérica puede contener declaraciones de tipos anidados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="7db9f-540">Los parámetros de tipo de la clase envolvente se pueden usar dentro de los tipos anidados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="7db9f-541">Una declaración de tipos anidados puede contener parámetros de tipo adicionales que solo se aplican al tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="7db9f-542">Cada declaración de tipos contenida dentro de una declaración de clase genérica es implícitamente una declaración de tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="7db9f-543">Al escribir una referencia a un tipo anidado dentro de un tipo genérico, el tipo que contiene el tipo construido, incluidos sus argumentos de tipo, debe denominarse.</span><span class="sxs-lookup"><span data-stu-id="7db9f-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="7db9f-544">Sin embargo, desde dentro de la clase externa, el tipo anidado se puede usar sin calificación; el tipo de instancia de la clase externa se puede usar implícitamente al construir el tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="7db9f-545">En el ejemplo siguiente se muestran tres formas correctas de hacer referencia a un tipo construido creado a partir de `Inner`; los dos primeros son equivalentes:</span><span class="sxs-lookup"><span data-stu-id="7db9f-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="7db9f-546">Aunque el estilo de programación es incorrecto, un parámetro de tipo de un tipo anidado puede ocultar un miembro o un parámetro de tipo declarado en el tipo externo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="7db9f-547">Nombres de miembros reservados</span><span class="sxs-lookup"><span data-stu-id="7db9f-547">Reserved member names</span></span>

<span data-ttu-id="7db9f-548">Para facilitar la implementación C# subyacente en tiempo de ejecución, para cada declaración de miembro de origen que sea una propiedad, un evento o un indexador, la implementación debe reservar dos firmas de método basadas en el tipo de declaración de miembro, su nombre y su tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="7db9f-549">Se trata de un error en tiempo de compilación para que un programa declare un miembro cuya firma coincide con una de estas firmas reservadas, aunque la implementación de tiempo de ejecución subyacente no use estas reservas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="7db9f-550">Los nombres reservados no presentan declaraciones, por lo que no participan en la búsqueda de miembros.</span><span class="sxs-lookup"><span data-stu-id="7db9f-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="7db9f-551">Sin embargo, las signaturas de método reservado asociadas a una declaración participan en la herencia ([herencia](classes.md#inheritance)) y se pueden ocultar con el modificador `new` ([el nuevo modificador](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="7db9f-552">La reserva de estos nombres sirve para tres propósitos:</span><span class="sxs-lookup"><span data-stu-id="7db9f-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="7db9f-553">Para permitir que la implementación subyacente use un identificador ordinario como un nombre de método para obtener o establecer el acceso a C# la característica de lenguaje.</span><span class="sxs-lookup"><span data-stu-id="7db9f-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="7db9f-554">Para permitir que otros lenguajes interoperen usando un identificador ordinario como un nombre de método para obtener o establecer el C# acceso a la característica de lenguaje.</span><span class="sxs-lookup"><span data-stu-id="7db9f-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="7db9f-555">Para asegurarse de que el origen aceptado por un compilador compatible lo acepta otro, haciendo que los detalles de los nombres de miembro reservados sean coherentes C# en todas las implementaciones.</span><span class="sxs-lookup"><span data-stu-id="7db9f-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="7db9f-556">La declaración de un destructor ([destructores) también](classes.md#destructors)hace que se Reserve una firma ([los nombres de miembro están reservados para los destructores](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="7db9f-557">Nombres de miembro reservados para propiedades</span><span class="sxs-lookup"><span data-stu-id="7db9f-557">Member names reserved for properties</span></span>

<span data-ttu-id="7db9f-558">En el caso de una propiedad `P` ([propiedades](classes.md#properties)) de tipo `T`, se reservan las siguientes firmas:</span><span class="sxs-lookup"><span data-stu-id="7db9f-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="7db9f-559">Ambas firmas están reservadas, aunque la propiedad sea de solo lectura o de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="7db9f-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="7db9f-560">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-560">In the example</span></span>
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
<span data-ttu-id="7db9f-561">una clase `A` define una propiedad de solo lectura `P`, de modo que se reservan firmas para los métodos `get_P` y `set_P`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="7db9f-562">Una clase `B` deriva de `A` y oculta ambas firmas reservadas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="7db9f-563">En el ejemplo se genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="7db9f-563">The example produces the output:</span></span>
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="7db9f-564">Nombres de miembro reservados para eventos</span><span class="sxs-lookup"><span data-stu-id="7db9f-564">Member names reserved for events</span></span>

<span data-ttu-id="7db9f-565">Para un `E` de eventos ([eventos](classes.md#events)) de tipo delegado `T`, se reservan las siguientes firmas:</span><span class="sxs-lookup"><span data-stu-id="7db9f-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="7db9f-566">Nombres de miembro reservados para los indizadores</span><span class="sxs-lookup"><span data-stu-id="7db9f-566">Member names reserved for indexers</span></span>

<span data-ttu-id="7db9f-567">Para un indizador ([indexadores](classes.md#indexers)) de tipo `T` con `L`de lista de parámetros, se reservan las siguientes firmas:</span><span class="sxs-lookup"><span data-stu-id="7db9f-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="7db9f-568">Ambas firmas están reservadas, aunque el indizador sea de solo lectura o de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="7db9f-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="7db9f-569">Además, el nombre de miembro `Item` está reservado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="7db9f-570">Nombres de miembro reservados para destructores</span><span class="sxs-lookup"><span data-stu-id="7db9f-570">Member names reserved for destructors</span></span>

<span data-ttu-id="7db9f-571">Para una clase que contiene un destructor[(](classes.md#destructors)destructores), se reserva la firma siguiente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="7db9f-572">Constantes</span><span class="sxs-lookup"><span data-stu-id="7db9f-572">Constants</span></span>

<span data-ttu-id="7db9f-573">Una ***constante*** es un miembro de clase que representa un valor constante: un valor que se puede calcular en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="7db9f-574">Una *constant_declaration* introduce una o más constantes de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="7db9f-575">Un *constant_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), un modificador `new` ([el nuevo modificador](classes.md#the-new-modifier)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="7db9f-576">Los atributos y modificadores se aplican a todos los miembros declarados por el *constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="7db9f-577">Aunque las constantes se consideran miembros estáticos, una *constant_declaration* no requiere ni permite un modificador `static`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="7db9f-578">Es un error que el mismo modificador aparezca varias veces en una declaración de constante.</span><span class="sxs-lookup"><span data-stu-id="7db9f-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="7db9f-579">El *tipo* de un *constant_declaration* especifica el tipo de los miembros introducidos por la declaración.</span><span class="sxs-lookup"><span data-stu-id="7db9f-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="7db9f-580">El tipo va seguido de una lista de *constant_declarator*s, cada uno de los cuales incorpora un nuevo miembro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="7db9f-581">Un *constant_declarator* consta de un *identificador* que nombra el miembro, seguido de un token "`=`", seguido de un *constant_expression* ([expresiones constantes](expressions.md#constant-expressions)) que proporciona el valor del miembro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="7db9f-582">El *tipo* especificado en una declaración de constante debe ser `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *enum_type*o *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="7db9f-583">Cada *constant_expression* debe producir un valor del tipo de destino o de un tipo que se pueda convertir al tipo de destino mediante una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="7db9f-584">El *tipo* de una constante debe ser al menos tan accesible como la propia constante ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="7db9f-585">El valor de una constante se obtiene en una expresión usando un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="7db9f-586">Una constante puede participar en una *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="7db9f-587">Por lo tanto, se puede utilizar una constante en cualquier construcción que requiera una *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="7db9f-588">Algunos ejemplos de estas construcciones son `case` etiquetas, `goto case` instrucciones, `enum` declaraciones de miembros, atributos y otras declaraciones de constantes.</span><span class="sxs-lookup"><span data-stu-id="7db9f-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="7db9f-589">Como se describe en [expresiones constantes](expressions.md#constant-expressions), una *constant_expression* es una expresión que se puede evaluar por completo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="7db9f-590">Dado que la única forma de crear un valor que no sea NULL de una *reference_type* distinta de `string` es aplicar el operador `new` y, dado que no se permite el operador `new` en una *constant_expression*, el único valor posible para las constantes de *reference_type*s distinto de `string` es `null`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="7db9f-591">Cuando se desea un nombre simbólico para un valor constante, pero cuando el tipo de ese valor no se permite en una declaración de constante, o cuando el valor no se puede calcular en tiempo de compilación mediante un *constant_expression*, se puede usar en su lugar un campo `readonly` ([campos de solo lectura](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="7db9f-592">Una declaración de constante que declara varias constantes es equivalente a varias declaraciones de constantes únicas con los mismos atributos, modificadores y tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="7db9f-593">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="7db9f-594">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="7db9f-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="7db9f-595">Las constantes pueden depender de otras constantes en el mismo programa, siempre que las dependencias no sean de naturaleza circular.</span><span class="sxs-lookup"><span data-stu-id="7db9f-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="7db9f-596">El compilador se organiza automáticamente para evaluar las declaraciones de constantes en el orden adecuado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="7db9f-597">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-597">In the example</span></span>
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
<span data-ttu-id="7db9f-598">en primer lugar, el compilador evalúa `A.Y`, evalúa `B.Z`y, por último, evalúa `A.X`y genera los valores `10`, `11`y `12`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="7db9f-599">Las declaraciones de constantes pueden depender de constantes de otros programas, pero dichas dependencias solo son posibles en una dirección.</span><span class="sxs-lookup"><span data-stu-id="7db9f-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="7db9f-600">Si se hace referencia al ejemplo anterior, si `A` y `B` se declararon en programas independientes, sería posible que `A.X` dependa de `B.Z`, pero `B.Z` podría no depender simultáneamente de `A.Y`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="7db9f-601">Campos</span><span class="sxs-lookup"><span data-stu-id="7db9f-601">Fields</span></span>

<span data-ttu-id="7db9f-602">Un ***campo*** es un miembro que representa una variable asociada con un objeto o una clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="7db9f-603">Un *field_declaration* introduce uno o más campos de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="7db9f-604">Un *field_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), un modificador `new` ([el nuevo modificador](classes.md#the-new-modifier)), una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)) y un modificador `static` ([campos estáticos y de instancia](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="7db9f-605">Además, un *field_declaration* puede incluir un modificador `readonly` ([campos ReadOnly](classes.md#readonly-fields)) o un modificador `volatile` ([campos volátiles](classes.md#volatile-fields)), pero no ambos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="7db9f-606">Los atributos y modificadores se aplican a todos los miembros declarados por el *field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="7db9f-607">Es un error que el mismo modificador aparezca varias veces en una declaración de campo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="7db9f-608">El *tipo* de un *field_declaration* especifica el tipo de los miembros introducidos por la declaración.</span><span class="sxs-lookup"><span data-stu-id="7db9f-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="7db9f-609">El tipo va seguido de una lista de *variable_declarator*s, cada uno de los cuales incorpora un nuevo miembro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="7db9f-610">Un *variable_declarator* consta de un *identificador* que asigna un nombre a ese miembro, seguido opcionalmente por un token "`=`" y un *variable_initializer* ([inicializadores de variables](classes.md#variable-initializers)) que proporciona el valor inicial de ese miembro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="7db9f-611">El *tipo* de un campo debe ser al menos igual de accesible que el propio campo ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="7db9f-612">El valor de un campo se obtiene en una expresión usando un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="7db9f-613">El valor de un campo que no es de solo lectura se modifica mediante una *asignación* ([operadores de asignación](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="7db9f-614">El valor de un campo que no es de solo lectura se puede obtener y modificar mediante operadores de incremento y decremento postfijos ([operadores de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)postfijo) y operadores de incremento y decremento prefijos ([operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="7db9f-615">Una declaración de campo que declara varios campos es equivalente a varias declaraciones de campos únicos con los mismos atributos, modificadores y tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="7db9f-616">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="7db9f-617">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="7db9f-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="7db9f-618">Campos estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="7db9f-618">Static and instance fields</span></span>

<span data-ttu-id="7db9f-619">Cuando una declaración de campo incluye un modificador `static`, los campos introducidos por la declaración son ***campos estáticos***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="7db9f-620">Cuando no hay ningún modificador `static`, los campos introducidos por la declaración son ***campos de instancia***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="7db9f-621">Los campos estáticos y los campos de instancia son dos de los diversos tipos de variables ([Variables](variables.md)) que admite C# y, en ocasiones, se hace referencia a ellas como ***variables estáticas*** y ***variables de instancia***, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="7db9f-622">Los campos estáticos no forman parte de una instancia específica; en su lugar, se comparte entre todas las instancias de un tipo cerrado ([tipos abiertos y cerrados](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="7db9f-623">Independientemente del número de instancias de un tipo de clase cerrada, solo hay una copia de un campo estático para el dominio de aplicación asociado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="7db9f-624">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-624">For example:</span></span>
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

<span data-ttu-id="7db9f-625">Un campo de instancia pertenece a una instancia de.</span><span class="sxs-lookup"><span data-stu-id="7db9f-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="7db9f-626">En concreto, cada instancia de una clase contiene un conjunto independiente de todos los campos de instancia de esa clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="7db9f-627">Cuando se hace referencia a un campo en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, si `M` es un campo estático, `E` debe indicar un tipo que contiene `M`y, si `M` es un campo de instancia, E debe indicar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="7db9f-628">Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="7db9f-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="7db9f-629">Campos de solo lectura</span><span class="sxs-lookup"><span data-stu-id="7db9f-629">Readonly fields</span></span>

<span data-ttu-id="7db9f-630">Cuando una *field_declaration* incluye un modificador `readonly`, los campos introducidos por la declaración son ***campos de solo lectura***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="7db9f-631">Las asignaciones directas a campos de solo lectura solo se pueden producir como parte de la declaración o en un constructor de instancia o en un constructor estático de la misma clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="7db9f-632">(Un campo de solo lectura se puede asignar a varias veces en estos contextos). En concreto, las asignaciones directas a un campo `readonly` solo se permiten en los contextos siguientes:</span><span class="sxs-lookup"><span data-stu-id="7db9f-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="7db9f-633">En el *variable_declarator* que presenta el campo (incluyendo un *variable_initializer* en la declaración).</span><span class="sxs-lookup"><span data-stu-id="7db9f-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="7db9f-634">Para un campo de instancia, en los constructores de instancia de la clase que contiene la declaración de campo; para un campo estático, en el constructor estático de la clase que contiene la declaración de campo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="7db9f-635">Estos son también los únicos contextos en los que es válido pasar un campo de `readonly` como parámetro `out` o `ref`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="7db9f-636">Un error en tiempo de compilación intenta asignar a un campo de `readonly` o pasarlo como un parámetro `out` o `ref` en cualquier otro contexto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="7db9f-637">Usar campos de solo lectura estáticos para constantes</span><span class="sxs-lookup"><span data-stu-id="7db9f-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="7db9f-638">Un campo `static readonly` es útil cuando se desea un nombre simbólico para un valor constante, pero cuando el tipo del valor no está permitido en una declaración de `const` o cuando el valor no se puede calcular en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="7db9f-639">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-639">In the example</span></span>
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
<span data-ttu-id="7db9f-640">los miembros `Black`, `White`, `Red`, `Green`y `Blue` no se pueden declarar como miembros de `const` porque sus valores no se pueden calcular en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="7db9f-641">Sin embargo, si se declaran `static readonly` en su lugar, se produce el mismo efecto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="7db9f-642">Control de versiones de constantes y campos de solo lectura estáticos</span><span class="sxs-lookup"><span data-stu-id="7db9f-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="7db9f-643">Las constantes y los campos de solo lectura tienen una semántica de versiones binaria diferente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="7db9f-644">Cuando una expresión hace referencia a una constante, el valor de la constante se obtiene en tiempo de compilación, pero cuando una expresión hace referencia a un campo de solo lectura, el valor del campo no se obtiene hasta el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7db9f-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="7db9f-645">Considere una aplicación que consta de dos programas independientes:</span><span class="sxs-lookup"><span data-stu-id="7db9f-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="7db9f-646">Los espacios de nombres `Program1` y `Program2` denotan dos programas que se compilan por separado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="7db9f-647">Dado que `Program1.Utils.X` se declara como un campo estático de solo lectura, la salida del valor de la instrucción `Console.WriteLine` no se conoce en tiempo de compilación, sino que se obtiene en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7db9f-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="7db9f-648">Por lo tanto, si se cambia el valor de `X` y `Program1` se vuelve a compilar, la instrucción `Console.WriteLine` generará el nuevo valor incluso si `Program2` no se vuelve a compilar.</span><span class="sxs-lookup"><span data-stu-id="7db9f-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="7db9f-649">Sin embargo, si `X` era una constante, el valor de `X` se habría obtenido en el momento en que se compiló `Program2` y no se vería afectado por los cambios en `Program1` hasta que `Program2` se vuelva a compilar.</span><span class="sxs-lookup"><span data-stu-id="7db9f-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="7db9f-650">Campos volátiles</span><span class="sxs-lookup"><span data-stu-id="7db9f-650">Volatile fields</span></span>

<span data-ttu-id="7db9f-651">Cuando una *field_declaration* incluye un modificador `volatile`, los campos introducidos por esa declaración son ***campos volátiles***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="7db9f-652">En el caso de los campos no volátiles, las técnicas de optimización que reordenan las instrucciones pueden provocar resultados inesperados e imprevisibles en programas multiproceso que tienen acceso a campos sin sincronización, como los que proporciona el *lock_statement* ([la instrucción lock](statements.md#the-lock-statement)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="7db9f-653">Estas optimizaciones las puede realizar el compilador, el sistema en tiempo de ejecución o el hardware.</span><span class="sxs-lookup"><span data-stu-id="7db9f-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="7db9f-654">En el caso de los campos volátiles, las optimizaciones de reordenación están restringidas:</span><span class="sxs-lookup"><span data-stu-id="7db9f-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="7db9f-655">Una lectura de un campo volátil se denomina ***lectura volátil***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="7db9f-656">Una lectura volátil tiene la "semántica de adquisición"; es decir, se garantiza que se produce antes de las referencias a la memoria que se producen después de ella en la secuencia de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="7db9f-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="7db9f-657">La escritura de un campo volátil se denomina ***escritura volátil***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="7db9f-658">Una escritura volátil tiene "semántica de versión"; es decir, se garantiza que se produce después de cualquier referencia de memoria antes de la instrucción de escritura en la secuencia de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="7db9f-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="7db9f-659">Estas restricciones aseguran que todos los subprocesos observarán las operaciones de escritura volátiles realizadas por cualquier otro subproceso en el orden en que se realizaron.</span><span class="sxs-lookup"><span data-stu-id="7db9f-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="7db9f-660">No se requiere una implementación compatible para proporcionar una ordenación total única de escrituras volátiles, como se aprecia en todos los subprocesos de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7db9f-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="7db9f-661">El tipo de un campo volátil debe ser uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="7db9f-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="7db9f-662">*Reference_type*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-662">A *reference_type*.</span></span>
*  <span data-ttu-id="7db9f-663">Tipo `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`o `System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="7db9f-664">*Enum_type* que tiene un tipo base de enumeración de `byte`, `sbyte`, `short`, `ushort`, `int`o `uint`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="7db9f-665">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-665">The example</span></span>
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
<span data-ttu-id="7db9f-666">genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="7db9f-666">produces the output:</span></span>
```console
result = 143
```

<span data-ttu-id="7db9f-667">En este ejemplo, el método `Main` inicia un nuevo subproceso que ejecuta el método `Thread2`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="7db9f-668">Este método almacena un valor en un campo no volátil denominado `result`y, a continuación, almacena `true` en el campo volatile `finished`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="7db9f-669">El subproceso principal espera a que el campo `finished` se establezca en `true`y, a continuación, lee el campo `result`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="7db9f-670">Como `finished` se ha declarado `volatile`, el subproceso principal debe leer el valor `143` de la `result`de campo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="7db9f-671">Si el `finished` de campo no se hubiera declarado `volatile`, se permitiría que el almacén `result` fuera visible en el subproceso principal después del almacén para `finished`y, por lo tanto, para que el subproceso principal Lea el valor `0` del campo `result`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="7db9f-672">Al declarar `finished` como un campo `volatile` se evita cualquier incoherencia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="7db9f-673">Inicialización de campos</span><span class="sxs-lookup"><span data-stu-id="7db9f-673">Field initialization</span></span>

<span data-ttu-id="7db9f-674">El valor inicial de un campo, ya sea un campo estático o un campo de instancia, es el valor predeterminado ([valores predeterminados](variables.md#default-values)) del tipo de campo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="7db9f-675">No es posible observar el valor de un campo antes de que se haya producido esta inicialización predeterminada y, por tanto, un campo nunca es "no inicializado".</span><span class="sxs-lookup"><span data-stu-id="7db9f-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="7db9f-676">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-676">The example</span></span>
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
<span data-ttu-id="7db9f-677">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="7db9f-677">produces the output</span></span>
```console
b = False, i = 0
```
<span data-ttu-id="7db9f-678">Dado que `b` y `i` se inicializan automáticamente en los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="7db9f-679">Inicializadores de variables</span><span class="sxs-lookup"><span data-stu-id="7db9f-679">Variable initializers</span></span>

<span data-ttu-id="7db9f-680">Las declaraciones de campo pueden incluir *variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="7db9f-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="7db9f-681">En los campos estáticos, los inicializadores de variables corresponden a las instrucciones de asignación que se ejecutan durante la inicialización de la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="7db9f-682">En el caso de los campos de instancia, los inicializadores de variables corresponden a las instrucciones de asignación que se ejecutan cuando se crea una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="7db9f-683">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-683">The example</span></span>
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
<span data-ttu-id="7db9f-684">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="7db9f-684">produces the output</span></span>
```console
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="7db9f-685">Dado que una asignación a `x` tiene lugar cuando se ejecutan inicializadores de campo estáticos y las asignaciones a `i` y `s` se producen cuando se ejecutan los inicializadores de campo de instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="7db9f-686">La inicialización del valor predeterminado que se describe en [inicialización de campos](classes.md#field-initialization) se produce para todos los campos, incluidos los campos que tienen inicializadores variables.</span><span class="sxs-lookup"><span data-stu-id="7db9f-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="7db9f-687">Por lo tanto, cuando se inicializa una clase, todos los campos estáticos de esa clase se inicializan primero a sus valores predeterminados y, a continuación, los inicializadores de campo estáticos se ejecutan en orden textual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="7db9f-688">Del mismo modo, cuando se crea una instancia de una clase, todos los campos de instancia de esa instancia se inicializan por primera vez con sus valores predeterminados y, a continuación, los inicializadores de campo de instancia se ejecutan en orden textual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="7db9f-689">Es posible que se respeten los campos estáticos con inicializadores variables en su estado de valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="7db9f-690">Sin embargo, esto no se recomienda como cuestión de estilo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="7db9f-691">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-691">The example</span></span>
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
<span data-ttu-id="7db9f-692">exhibe este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-692">exhibits this behavior.</span></span> <span data-ttu-id="7db9f-693">A pesar de las definiciones circulares de a y b, el programa es válido.</span><span class="sxs-lookup"><span data-stu-id="7db9f-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="7db9f-694">Da como resultado la salida</span><span class="sxs-lookup"><span data-stu-id="7db9f-694">It results in the output</span></span>
```console
a = 1, b = 2
```
<span data-ttu-id="7db9f-695">Dado que los campos estáticos `a` y `b` se inicializan en `0` (el valor predeterminado para `int`) antes de que se ejecuten sus inicializadores.</span><span class="sxs-lookup"><span data-stu-id="7db9f-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="7db9f-696">Cuando se ejecuta el inicializador para `a`, el valor de `b` es cero y, por tanto, se inicializa `a` en `1`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="7db9f-697">Cuando se ejecuta el inicializador para `b`, el valor de `a` ya está `1`y, por tanto, `b` se inicializa en `2`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="7db9f-698">Inicialización de campos estáticos</span><span class="sxs-lookup"><span data-stu-id="7db9f-698">Static field initialization</span></span>

<span data-ttu-id="7db9f-699">Los inicializadores de variables de campo estáticos de una clase corresponden a una secuencia de asignaciones que se ejecutan en el orden textual en el que aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="7db9f-700">Si un constructor estático ([constructores estáticos](classes.md#static-constructors)) existe en la clase, la ejecución de los inicializadores de campo estáticos se produce inmediatamente antes de ejecutar ese constructor estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="7db9f-701">De lo contrario, los inicializadores de campo estáticos se ejecutan en un momento dependiente de la implementación antes del primer uso de un campo estático de esa clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="7db9f-702">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-702">The example</span></span>
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
<span data-ttu-id="7db9f-703">puede producir la salida:</span><span class="sxs-lookup"><span data-stu-id="7db9f-703">might produce either the output:</span></span>
```console
Init A
Init B
1 1
```
<span data-ttu-id="7db9f-704">o la salida:</span><span class="sxs-lookup"><span data-stu-id="7db9f-704">or the output:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="7db9f-705">Dado que la ejecución del inicializador de `X`y del inicializador de `Y`puede producirse en cualquier orden; solo se restringen a las referencias a esos campos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="7db9f-706">Sin embargo, en el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-706">However, in the example:</span></span>
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
<span data-ttu-id="7db9f-707">la salida debe ser:</span><span class="sxs-lookup"><span data-stu-id="7db9f-707">the output must be:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="7db9f-708">Dado que las reglas para cuando los constructores estáticos se ejecutan (como se definen en [constructores estáticos](classes.md#static-constructors)), proporcionan el constructor estático de `B`(y, por tanto, los inicializadores de campo estáticos de `B`) se deben ejecutar antes de `A`constructores estáticos y los inicializadores de campo de.</span><span class="sxs-lookup"><span data-stu-id="7db9f-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="7db9f-709">Inicialización de campos de instancia</span><span class="sxs-lookup"><span data-stu-id="7db9f-709">Instance field initialization</span></span>

<span data-ttu-id="7db9f-710">Los inicializadores de variable de campo de instancia de una clase corresponden a una secuencia de asignaciones que se ejecutan inmediatamente después de la entrada a cualquiera de los constructores de instancia ([inicializadores de constructor](classes.md#constructor-initializers)) de esa clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="7db9f-711">Los inicializadores de variable se ejecutan en el orden textual en el que aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="7db9f-712">El proceso de creación e inicialización de instancias de clase se describe con más detalle en [constructores de instancias](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="7db9f-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="7db9f-713">Un inicializador de variable para un campo de instancia no puede hacer referencia a la instancia que se está creando.</span><span class="sxs-lookup"><span data-stu-id="7db9f-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="7db9f-714">Por lo tanto, se trata de un error en tiempo de compilación para hacer referencia a `this` en un inicializador de variable, ya que se trata de un error en tiempo de compilación para que un inicializador de variable haga referencia a un miembro de instancia a través de un *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="7db9f-715">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="7db9f-716">el inicializador de variable para `y` produce un error en tiempo de compilación porque hace referencia a un miembro de la instancia que se va a crear.</span><span class="sxs-lookup"><span data-stu-id="7db9f-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="7db9f-717">Métodos</span><span class="sxs-lookup"><span data-stu-id="7db9f-717">Methods</span></span>

<span data-ttu-id="7db9f-718">Un ***método*** es un miembro que implementa un cálculo o una acción que puede realizar un objeto o una clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="7db9f-719">Los métodos se declaran mediante *method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="7db9f-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="7db9f-720">Un *method_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso[(modificadores de acceso](classes.md#access-modifiers)), el `new` ([modificador nuevo](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` (métodos de[invalidación](classes.md#override-methods)), `sealed` (métodos[sellados](classes.md#sealed-methods)), `abstract` (métodos[abstractos](classes.md#abstract-methods)) y modificadores `extern` ([métodos](classes.md#external-methods)</span><span class="sxs-lookup"><span data-stu-id="7db9f-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="7db9f-721">Una declaración tiene una combinación válida de modificadores si se cumplen todas las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="7db9f-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="7db9f-722">La Declaración incluye una combinación válida de modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="7db9f-723">La declaración no incluye el mismo modificador varias veces.</span><span class="sxs-lookup"><span data-stu-id="7db9f-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="7db9f-724">La Declaración incluye como máximo uno de los siguientes modificadores: `static`, `virtual`y `override`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="7db9f-725">La Declaración incluye como máximo uno de los siguientes modificadores: `new` y `override`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="7db9f-726">Si la Declaración incluye el modificador `abstract`, la declaración no incluye ninguno de los siguientes modificadores: `static`, `virtual`, `sealed` o `extern`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="7db9f-727">Si la Declaración incluye el modificador `private`, la declaración no incluye ninguno de los siguientes modificadores: `virtual`, `override`o `abstract`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="7db9f-728">Si la Declaración incluye el modificador `sealed`, la Declaración también incluye el modificador `override`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="7db9f-729">Si la Declaración incluye el modificador `partial`, no incluye ninguno de los siguientes modificadores: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`o `extern`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="7db9f-730">Un método que tiene el modificador `async` es una función asincrónica y sigue las reglas descritas en [funciones asincrónicas](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="7db9f-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="7db9f-731">El *return_type* de una declaración de método especifica el tipo del valor calculado y devuelto por el método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="7db9f-732">El *return_type* se `void` si el método no devuelve un valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="7db9f-733">Si la Declaración incluye el modificador `partial`, el tipo de valor devuelto debe ser `void`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="7db9f-734">El *member_name* especifica el nombre del método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="7db9f-735">A menos que el método sea una implementación explícita de un miembro de interfaz ([implementaciones explícitas de miembros de interfaz](interfaces.md#explicit-interface-member-implementations)), el *member_name* es simplemente un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="7db9f-736">En el caso de una implementación explícita de un miembro de interfaz, el *member_name* se compone de un *interface_type* seguido de un "`.`" y un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="7db9f-737">El *type_parameter_list* opcional especifica los parámetros de tipo del método ([parámetros de tipo](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="7db9f-738">Si se especifica un *type_parameter_list* el método es un ***método genérico***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="7db9f-739">Si el método tiene un modificador `extern`, no se puede especificar un *type_parameter_list* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="7db9f-740">El *formal_parameter_list* opcional especifica los parámetros del método ([parámetros de método](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="7db9f-741">Los *type_parameter_constraints_clause*opcionales especifican restricciones en parámetros de tipo individuales ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) y solo se pueden especificar si también se proporciona un *type_parameter_list* y el método no tiene un modificador `override`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="7db9f-742">El *return_type* y cada uno de los tipos a los que se hace referencia en el *formal_parameter_list* de un método deben ser al menos tan accesibles como el propio método ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="7db9f-743">El *method_body* es un punto y coma, un ***cuerpo de instrucción*** o un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="7db9f-744">Un cuerpo de instrucción consta de un *bloque*, que especifica las instrucciones que se ejecutarán cuando se invoque el método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="7db9f-745">Un cuerpo de expresión consta de `=>` seguido de una *expresión* y un punto y coma, y denota una expresión única que se debe realizar cuando se invoca el método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="7db9f-746">En el caso de los métodos `abstract` y `extern`, el *method_body* consta simplemente de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="7db9f-747">Por `partial` métodos, el *method_body* puede constar de un punto y coma, un cuerpo de bloque o un cuerpo de expresión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="7db9f-748">En el caso de todos los demás métodos, el *method_body* es un cuerpo de bloque o un cuerpo de expresión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="7db9f-749">Si el *method_body* consta de un punto y coma, la declaración no puede incluir el modificador `async`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="7db9f-750">El nombre, la lista de parámetros de tipo y la lista de parámetros formales de un método definen la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) del método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="7db9f-751">En concreto, la firma de un método consta de su nombre, el número de parámetros de tipo y el número, modificadores y tipos de sus parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="7db9f-752">Para estos propósitos, cualquier parámetro de tipo del método que se produce en el tipo de un parámetro formal se identifica no por su nombre, sino por su posición ordinal en la lista de argumentos de tipo del método. El tipo de valor devuelto no forma parte de la signatura de un método, ni tampoco los nombres de los parámetros de tipo o los parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="7db9f-753">El nombre de un método debe ser distinto de los nombres de todos los demás métodos que no se declaran en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="7db9f-754">Además, la firma de un método debe ser diferente de las firmas de todos los demás métodos declarados en la misma clase y dos métodos declarados en la misma clase no pueden tener firmas que difieran únicamente en `ref` y `out`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="7db9f-755">Los *type_parameter*s del método están en el ámbito de la *method_declaration*y se pueden usar para formar tipos en ese ámbito en *return_type*, *method_body*y *type_parameter_constraints_clause*s, pero no en *atributos*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="7db9f-756">Todos los parámetros formales y los parámetros de tipo deben tener nombres diferentes.</span><span class="sxs-lookup"><span data-stu-id="7db9f-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="7db9f-757">Parámetros de método</span><span class="sxs-lookup"><span data-stu-id="7db9f-757">Method parameters</span></span>

<span data-ttu-id="7db9f-758">Los parámetros de un método, si los hay, se declaran mediante el *formal_parameter_list*del método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="7db9f-759">La lista de parámetros formales está formada por uno o varios parámetros separados por comas, de los cuales solo el último puede ser un *parameter_array*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="7db9f-760">Un *fixed_parameter* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), un modificador `ref`, `out` o `this` opcional, un *tipo*, un *identificador* y un *default_argument*opcional.</span><span class="sxs-lookup"><span data-stu-id="7db9f-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="7db9f-761">Cada *fixed_parameter* declara un parámetro del tipo especificado con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="7db9f-762">El modificador `this` designa el método como un método de extensión y solo se permite en el primer parámetro de un método estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="7db9f-763">Los métodos de extensión se describen con más detalle en [métodos de extensión](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="7db9f-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="7db9f-764">Un *fixed_parameter* con un *default_argument* se conoce como ***parámetro opcional***, mientras que un *fixed_parameter* sin *default_argument* es un ***parámetro necesario***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="7db9f-765">Es posible que un parámetro necesario no aparezca después de un parámetro opcional en un *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="7db9f-766">Un parámetro `ref` o `out` no puede tener una *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="7db9f-767">La *expresión* de un *default_argument* debe ser una de las siguientes:</span><span class="sxs-lookup"><span data-stu-id="7db9f-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="7db9f-768">a *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="7db9f-768">a *constant_expression*</span></span>
*  <span data-ttu-id="7db9f-769">una expresión con el formato `new S()` donde `S` es un tipo de valor</span><span class="sxs-lookup"><span data-stu-id="7db9f-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="7db9f-770">una expresión con el formato `default(S)` donde `S` es un tipo de valor</span><span class="sxs-lookup"><span data-stu-id="7db9f-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="7db9f-771">La *expresión* debe poder convertirse implícitamente mediante una identidad o una conversión que acepte valores NULL en el tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="7db9f-772">Si se producen parámetros opcionales en una declaración de método parcial de implementación ([métodos parciales](classes.md#partial-methods)), una implementación explícita de un miembro de interfaz ([implementaciones de miembros de interfaz explícitos](interfaces.md#explicit-interface-member-implementations)) o en una declaración de indexador de un solo parámetro ([indizadores](classes.md#indexers)), el compilador debe proporcionar una advertencia, ya que estos miembros nunca se pueden invocar de forma que permita omitir los argumentos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="7db9f-773">Un *parameter_array* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), un modificador de `params`, un *array_type*y un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="7db9f-774">Una matriz de parámetros declara un parámetro único del tipo de matriz especificado con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="7db9f-775">El *array_type* de una matriz de parámetros debe ser un tipo de matriz unidimensional ([tipos de matriz](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="7db9f-776">En una invocación de método, una matriz de parámetros permite especificar un único argumento del tipo de matriz especificado o permite que se especifiquen cero o más argumentos del tipo de elemento de matriz.</span><span class="sxs-lookup"><span data-stu-id="7db9f-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="7db9f-777">Las matrices de parámetros se describen con más detalle en [matrices de parámetros](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="7db9f-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="7db9f-778">Una *parameter_array* puede producirse después de un parámetro opcional, pero no puede tener un valor predeterminado, la omisión de los argumentos de un *parameter_array* daría lugar a la creación de una matriz vacía.</span><span class="sxs-lookup"><span data-stu-id="7db9f-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="7db9f-779">En el ejemplo siguiente se muestran diferentes tipos de parámetros:</span><span class="sxs-lookup"><span data-stu-id="7db9f-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="7db9f-780">En el *formal_parameter_list* de `M`, `i` es un parámetro ref necesario, `d` es un parámetro de valor obligatorio, `b`, `s`, `o` y `t` son parámetros de valor opcionales y `a` es una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="7db9f-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="7db9f-781">Una declaración de método crea un espacio de declaración independiente para los parámetros, los parámetros de tipo y las variables locales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="7db9f-782">Los nombres se introducen en este espacio de declaración mediante la lista de parámetros de tipo y la lista de parámetros formales del método y las declaraciones de variables locales en el *bloque* del método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="7db9f-783">Es un error que dos miembros de un espacio de declaración de método tengan el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="7db9f-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="7db9f-784">Es un error que el espacio de declaración de método y el espacio de declaración de variable local de un espacio de declaración anidado contengan elementos con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="7db9f-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="7db9f-785">Una invocación de método ([invocaciones de método](expressions.md#method-invocations)) crea una copia, específica de dicha invocación, de los parámetros formales y las variables locales del método, y la lista de argumentos de la invocación asigna valores o referencias a variables a los parámetros formales recién creados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="7db9f-786">Dentro del *bloque* de un método, se puede hacer referencia a los parámetros formales por sus identificadores en expresiones de *Simple_name* ([nombres simples](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="7db9f-787">Hay cuatro tipos de parámetros formales:</span><span class="sxs-lookup"><span data-stu-id="7db9f-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="7db9f-788">Parámetros de valor, que se declaran sin ningún modificador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="7db9f-789">Parámetros de referencia, que se declaran con el modificador `ref`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="7db9f-790">Parámetros de salida, que se declaran con el modificador `out`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="7db9f-791">Matrices de parámetros, que se declaran con el modificador `params`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="7db9f-792">Como se describe en [firmas y sobrecarga](basic-concepts.md#signatures-and-overloading), los modificadores `ref` y `out` forman parte de la firma de un método, pero el modificador `params` no lo es.</span><span class="sxs-lookup"><span data-stu-id="7db9f-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="7db9f-793">Parámetros de valor</span><span class="sxs-lookup"><span data-stu-id="7db9f-793">Value parameters</span></span>

<span data-ttu-id="7db9f-794">Un parámetro declarado sin modificadores es un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="7db9f-795">Un parámetro de valor corresponde a una variable local que obtiene su valor inicial del argumento correspondiente proporcionado en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="7db9f-796">Cuando un parámetro formal es un parámetro de valor, el argumento correspondiente en una invocación de método debe ser una expresión que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="7db9f-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="7db9f-797">Un método puede asignar nuevos valores a un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="7db9f-798">Dichas asignaciones solo afectan a la ubicación de almacenamiento local representada por el parámetro de valor; no tienen ningún efecto en el argumento real proporcionado en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="7db9f-799">Parámetros de referencia</span><span class="sxs-lookup"><span data-stu-id="7db9f-799">Reference parameters</span></span>

<span data-ttu-id="7db9f-800">Un parámetro declarado con un modificador `ref` es un parámetro de referencia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="7db9f-801">A diferencia de un parámetro de valor, un parámetro de referencia no crea una nueva ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="7db9f-802">En su lugar, un parámetro de referencia representa la misma ubicación de almacenamiento que la variable proporcionada como argumento en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="7db9f-803">Cuando un parámetro formal es un parámetro de referencia, el argumento correspondiente en una invocación de método debe constar de la palabra clave `ref` seguido de un *variable_reference* ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) del mismo tipo que el parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="7db9f-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="7db9f-804">Una variable debe estar asignada definitivamente antes de que se pueda pasar como parámetro de referencia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="7db9f-805">Dentro de un método, un parámetro de referencia se considera siempre asignado definitivamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="7db9f-806">Un método declarado como iterador ([iteradores](classes.md#iterators)) no puede tener parámetros de referencia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="7db9f-807">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-807">The example</span></span>
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
<span data-ttu-id="7db9f-808">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="7db9f-808">produces the output</span></span>
```console
i = 2, j = 1
```

<span data-ttu-id="7db9f-809">Para la invocación de `Swap` en `Main`, `x` representa `i` y `y` representa `j`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="7db9f-810">Por lo tanto, la invocación tiene el efecto de intercambiar los valores de `i` y `j`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="7db9f-811">En un método que toma parámetros de referencia, es posible que varios nombres representen la misma ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="7db9f-812">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-812">In the example</span></span>
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
<span data-ttu-id="7db9f-813">la invocación de `F` en `G` pasa una referencia a `s` para `a` y `b`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="7db9f-814">Por lo tanto, para esa invocación, los nombres `s`, `a`y `b` hacen referencia a la misma ubicación de almacenamiento, y las tres asignaciones modifican el campo de instancia `s`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="7db9f-815">Parámetros de salida</span><span class="sxs-lookup"><span data-stu-id="7db9f-815">Output parameters</span></span>

<span data-ttu-id="7db9f-816">Un parámetro declarado con un modificador `out` es un parámetro de salida.</span><span class="sxs-lookup"><span data-stu-id="7db9f-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="7db9f-817">De forma similar a un parámetro de referencia, un parámetro de salida no crea una nueva ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="7db9f-818">En su lugar, un parámetro de salida representa la misma ubicación de almacenamiento que la variable proporcionada como argumento en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="7db9f-819">Cuando un parámetro formal es un parámetro de salida, el argumento correspondiente en una invocación de método debe constar de la palabra clave `out` seguido de un *variable_reference* ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) del mismo tipo que el parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="7db9f-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="7db9f-820">No es necesario asignar definitivamente una variable antes de que se pueda pasar como parámetro de salida, pero después de una invocación en la que se pasó una variable como parámetro de salida, la variable se considera definitivamente asignada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="7db9f-821">Dentro de un método, al igual que una variable local, un parámetro de salida se considera inicialmente sin asignar y debe asignarse definitivamente antes de usar su valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="7db9f-822">Todos los parámetros de salida de un método se deben asignar definitivamente antes de que el método devuelva.</span><span class="sxs-lookup"><span data-stu-id="7db9f-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="7db9f-823">Un método declarado como método parcial ([métodos parciales](classes.md#partial-methods)) o un iterador ([iteradores](classes.md#iterators)) no puede tener parámetros de salida.</span><span class="sxs-lookup"><span data-stu-id="7db9f-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="7db9f-824">Los parámetros de salida se usan normalmente en métodos que producen varios valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="7db9f-825">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-825">For example:</span></span>
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

<span data-ttu-id="7db9f-826">En el ejemplo se genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="7db9f-826">The example produces the output:</span></span>
```console
c:\Windows\System\
hello.txt
```

<span data-ttu-id="7db9f-827">Tenga en cuenta que las variables `dir` y `name` se pueden desasignar antes de que se pasen a `SplitPath`y que se consideran definitivamente asignadas después de la llamada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="7db9f-828">Matrices de parámetros</span><span class="sxs-lookup"><span data-stu-id="7db9f-828">Parameter arrays</span></span>

<span data-ttu-id="7db9f-829">Un parámetro declarado con un modificador `params` es una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="7db9f-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="7db9f-830">Si una lista de parámetros formales incluye una matriz de parámetros, debe ser el último parámetro de la lista y debe ser de un tipo de matriz unidimensional.</span><span class="sxs-lookup"><span data-stu-id="7db9f-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="7db9f-831">Por ejemplo, los tipos `string[]` y `string[][]` se pueden usar como el tipo de una matriz de parámetros, pero el tipo `string[,]` no puede.</span><span class="sxs-lookup"><span data-stu-id="7db9f-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="7db9f-832">No es posible combinar el modificador `params` con los modificadores `ref` y `out`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="7db9f-833">Una matriz de parámetros permite especificar argumentos de una de estas dos maneras en una invocación de método:</span><span class="sxs-lookup"><span data-stu-id="7db9f-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="7db9f-834">El argumento dado para una matriz de parámetros puede ser una expresión única que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="7db9f-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="7db9f-835">En este caso, la matriz de parámetros actúa exactamente como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="7db9f-836">Como alternativa, la invocación puede especificar cero o más argumentos para la matriz de parámetros, donde cada argumento es una expresión que se puede convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de elemento de la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="7db9f-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="7db9f-837">En este caso, la invocación crea una instancia del tipo de matriz de parámetros con una longitud que corresponde al número de argumentos, inicializa los elementos de la instancia de la matriz con los valores de argumento especificados y usa la instancia de matriz recién creada como el actual. argument.</span><span class="sxs-lookup"><span data-stu-id="7db9f-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="7db9f-838">A excepción de permitir un número variable de argumentos en una invocación, una matriz de parámetros es exactamente equivalente a un parámetro de valor ([parámetros de valor](classes.md#value-parameters)) del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="7db9f-839">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-839">The example</span></span>
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
<span data-ttu-id="7db9f-840">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="7db9f-840">produces the output</span></span>
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="7db9f-841">La primera invocación de `F` simplemente pasa el `a` de la matriz como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="7db9f-842">La segunda invocación de `F` crea automáticamente un `int[]` de cuatro elementos con los valores de elemento especificados y pasa esa instancia de la matriz como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="7db9f-843">Del mismo modo, la tercera invocación de `F` crea un `int[]` de elementos cero y pasa esa instancia como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="7db9f-844">Las invocaciones segunda y tercera son exactamente equivalentes a la escritura:</span><span class="sxs-lookup"><span data-stu-id="7db9f-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="7db9f-845">Al realizar la resolución de sobrecarga, un método con una matriz de parámetros puede ser aplicable en su forma normal o en su forma expandida ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="7db9f-846">La forma expandida de un método solo está disponible si la forma normal del método no es aplicable y solo si un método aplicable con la misma signatura que la forma expandida no se ha declarado en el mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="7db9f-847">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-847">The example</span></span>
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
<span data-ttu-id="7db9f-848">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="7db9f-848">produces the output</span></span>
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="7db9f-849">En el ejemplo, dos de las formas expandidas posibles del método con una matriz de parámetros ya están incluidas en la clase como métodos normales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="7db9f-850">Por lo tanto, estos formularios expandidos no se tienen en cuenta al realizar la resolución de sobrecarga, y las invocaciones del método primero y tercer, por tanto, seleccionan los métodos normales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="7db9f-851">Cuando una clase declara un método con una matriz de parámetros, no es raro incluir también algunos de los formularios expandidos como métodos normales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="7db9f-852">Al hacerlo, es posible evitar la asignación de una instancia de matriz que se produce cuando se invoca una forma expandida de un método con una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="7db9f-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="7db9f-853">Cuando se `object[]`el tipo de una matriz de parámetros, se produce una ambigüedad potencial entre la forma normal del método y la forma desgastada para un solo parámetro `object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="7db9f-854">La razón de la ambigüedad es que una `object[]` es implícitamente convertible al tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="7db9f-855">Sin embargo, la ambigüedad no presenta ningún problema, ya que se puede resolver mediante la inserción de una conversión si es necesario.</span><span class="sxs-lookup"><span data-stu-id="7db9f-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="7db9f-856">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-856">The example</span></span>
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
<span data-ttu-id="7db9f-857">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="7db9f-857">produces the output</span></span>
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="7db9f-858">En la primera y última invocación de `F`, la forma normal de `F` es aplicable porque existe una conversión implícita desde el tipo de argumento al tipo de parámetro (ambos son del tipo `object[]`).</span><span class="sxs-lookup"><span data-stu-id="7db9f-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="7db9f-859">Por lo tanto, la resolución de sobrecarga selecciona la forma normal de `F`y el argumento se pasa como un parámetro de valor normal.</span><span class="sxs-lookup"><span data-stu-id="7db9f-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="7db9f-860">En la segunda y tercera invocación, la forma normal de `F` no es aplicable porque no existe ninguna conversión implícita del tipo de argumento al tipo de parámetro (el tipo `object` no se puede convertir implícitamente al tipo `object[]`).</span><span class="sxs-lookup"><span data-stu-id="7db9f-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="7db9f-861">Sin embargo, la forma expandida de `F` es aplicable, por lo que se selecciona mediante la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="7db9f-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="7db9f-862">Como resultado, la invocación crea un `object[]` de un elemento y el único elemento de la matriz se inicializa con el valor de argumento dado (que a su vez es una referencia a un `object[]`).</span><span class="sxs-lookup"><span data-stu-id="7db9f-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="7db9f-863">Métodos estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="7db9f-863">Static and instance methods</span></span>

<span data-ttu-id="7db9f-864">Cuando una declaración de método incluye un modificador `static`, se dice que se trata de un método estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="7db9f-865">Cuando no existe ningún modificador `static`, se dice que el método es un método de instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="7db9f-866">Un método estático no funciona en una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en un método estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="7db9f-867">Un método de instancia funciona en una instancia determinada de una clase y se puede tener acceso a esa instancia como `this` ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="7db9f-868">Cuando se hace referencia a un método en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`, si `M` es un método estático, `E` debe indicar un tipo que contiene `M`y, si `M` es un método de instancia, `E` debe indicar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="7db9f-869">Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="7db9f-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="7db9f-870">Métodos virtuales</span><span class="sxs-lookup"><span data-stu-id="7db9f-870">Virtual methods</span></span>

<span data-ttu-id="7db9f-871">Cuando una declaración de método de instancia incluye un modificador `virtual`, se dice que se trata de un método virtual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="7db9f-872">Cuando no existe ningún modificador `virtual`, se dice que el método es un método no virtual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="7db9f-873">La implementación de un método no virtual es invariable: la implementación es la misma si se invoca el método en una instancia de la clase en la que se declara o una instancia de una clase derivada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="7db9f-874">Por el contrario, las clases derivadas pueden reemplazar la implementación de un método virtual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="7db9f-875">El proceso de reemplazar la implementación de un método virtual heredado se conoce como ***invalidar*** ese método ([métodos de invalidación](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="7db9f-876">En una invocación de método virtual, el ***tipo en tiempo de ejecución*** de la instancia para la que tiene lugar la invocación determina la implementación de método real que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="7db9f-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="7db9f-877">En una invocación de método no virtual, el ***tipo en tiempo de compilación*** de la instancia es el factor determinante.</span><span class="sxs-lookup"><span data-stu-id="7db9f-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="7db9f-878">En términos precisos, cuando se invoca un método denominado `N` con una lista de argumentos `A` en una instancia con un tipo en tiempo de compilación `C` y un `R` de tipo en tiempo de ejecución (donde `R` es `C` o una clase derivada de `C`), la invocación se procesa de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="7db9f-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="7db9f-879">En primer lugar, se aplica la resolución de sobrecarga a `C`, `N`y `A`para seleccionar un método concreto `M` del conjunto de métodos declarados en y heredados por `C`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="7db9f-880">Esto se describe en las [invocaciones de método](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="7db9f-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="7db9f-881">A continuación, si `M` es un método no virtual, se invoca `M`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="7db9f-882">De lo contrario, `M` es un método virtual y se invoca la implementación más derivada de `M` con respecto a `R`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="7db9f-883">Para cada método virtual declarado en o heredado por una clase, existe una ***implementación más derivada*** del método con respecto a esa clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="7db9f-884">La implementación más derivada de un método virtual `M` con respecto a una clase `R` se determina de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="7db9f-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="7db9f-885">Si `R` contiene la declaración de introducción `virtual` de `M`, ésta es la implementación más derivada de `M`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="7db9f-886">De lo contrario, si `R` contiene un `override` de `M`, se trata de la implementación más derivada de `M`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="7db9f-887">De lo contrario, la implementación más derivada de `M` con respecto a `R` es la misma que la implementación más derivada de `M` con respecto a la clase base directa de `R`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="7db9f-888">En el ejemplo siguiente se muestran las diferencias entre los métodos virtuales y los no virtuales:</span><span class="sxs-lookup"><span data-stu-id="7db9f-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="7db9f-889">En el ejemplo, `A` presenta un método no virtual `F` y un método virtual `G`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="7db9f-890">La clase `B` introduce un nuevo método no virtual `F`, lo que oculta el `F`heredado, y también invalida el `G`de método heredado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="7db9f-891">En el ejemplo se genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="7db9f-891">The example produces the output:</span></span>
```console
A.F
B.F
B.G
B.G
```

<span data-ttu-id="7db9f-892">Observe que la instrucción `a.G()` invoca `B.G`, no `A.G`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="7db9f-893">Esto se debe a que el tipo en tiempo de ejecución de la instancia (que es `B`), no el tipo en tiempo de compilación de la instancia (que es `A`), determina la implementación de método real que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="7db9f-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="7db9f-894">Dado que los métodos pueden ocultar métodos heredados, es posible que una clase contenga varios métodos virtuales con la misma firma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="7db9f-895">Esto no presenta un problema de ambigüedad, ya que todo menos el método más derivado está oculto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="7db9f-896">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-896">In the example</span></span>
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
<span data-ttu-id="7db9f-897">las clases `C` y `D` contienen dos métodos virtuales con la misma firma: los introducidos por `A` y los introducidos por `C`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="7db9f-898">El método introducido por `C` oculta el método heredado de `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="7db9f-899">Por lo tanto, la declaración de invalidación en `D` invalida el método introducido por `C`y no es posible que `D` invalide el método introducido por `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="7db9f-900">En el ejemplo se genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="7db9f-900">The example produces the output:</span></span>
```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="7db9f-901">Tenga en cuenta que es posible invocar el método virtual oculto mediante el acceso a una instancia de `D` a través de un tipo menos derivado en el que el método no está oculto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="7db9f-902">Métodos de invalidación</span><span class="sxs-lookup"><span data-stu-id="7db9f-902">Override methods</span></span>

<span data-ttu-id="7db9f-903">Cuando una declaración de método de instancia incluye un modificador `override`, se dice que el método es un ***método de invalidación***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="7db9f-904">Un método de invalidación invalida un método virtual heredado con la misma firma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="7db9f-905">Mientras que una declaración de método virtual introduce un método nuevo, una declaración de método de reemplazo especializa un método virtual heredado existente proporcionando una nueva implementación de ese método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="7db9f-906">El método invalidado por una declaración de `override` se conoce como el ***método base invalidado***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="7db9f-907">Para un método de invalidación `M` declara en una `C`de clase, el método base invalidado se determina examinando cada tipo de clase base de `C`, empezando por el tipo de clase base directa de `C` y continuando con cada tipo de clase base directo sucesivo, hasta que en un tipo de clase base determinado se encuentre un método accesible que tenga la misma firma que `M` después de la sustitución de</span><span class="sxs-lookup"><span data-stu-id="7db9f-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="7db9f-908">A la hora de localizar el método base invalidado, se considera que un método es accesible si se `public`, si se `protected`, si está `protected internal`, o si se `internal` y se declara en el mismo programa que `C`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="7db9f-909">Se produce un error en tiempo de compilación a menos que se cumplan todas las condiciones siguientes para una declaración de invalidación:</span><span class="sxs-lookup"><span data-stu-id="7db9f-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="7db9f-910">Un método base invalidado se puede encontrar como se describió anteriormente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="7db9f-911">Hay exactamente un método base invalidado de este tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="7db9f-912">Esta restricción solo tiene efecto si el tipo de clase base es un tipo construido en el que la sustitución de los argumentos de tipo hace que la firma de dos métodos sea la misma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="7db9f-913">El método base invalidado es un método virtual, abstracto o de invalidación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="7db9f-914">En otras palabras, el método base invalidado no puede ser estático o no virtual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="7db9f-915">El método base invalidado no es un método sellado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="7db9f-916">El método de invalidación y el método base invalidado tienen el mismo tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="7db9f-917">La declaración de invalidación y el método base invalidado tienen la misma accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="7db9f-918">En otras palabras, una declaración de invalidación no puede cambiar la accesibilidad del método virtual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="7db9f-919">Sin embargo, si el método base invalidado es interno protegido y se declara en un ensamblado diferente al del ensamblado que contiene el método de invalidación, se debe proteger la accesibilidad declarada del método de invalidación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="7db9f-920">La declaración de invalidación no especifica las cláusulas-de-parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="7db9f-921">En su lugar, las restricciones se heredan del método base invalidado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="7db9f-922">Tenga en cuenta que las restricciones que son parámetros de tipo en el método invalidado se pueden reemplazar por argumentos de tipo en la restricción heredada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="7db9f-923">Esto puede dar lugar a restricciones que no son válidas cuando se especifican explícitamente, como tipos de valor o tipos sellados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="7db9f-924">En el ejemplo siguiente se muestra cómo funcionan las reglas de reemplazo para las clases genéricas:</span><span class="sxs-lookup"><span data-stu-id="7db9f-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="7db9f-925">Una declaración de invalidación puede tener acceso al método base invalidado mediante un *base_access* ([acceso base](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="7db9f-926">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-926">In the example</span></span>
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
<span data-ttu-id="7db9f-927">la invocación de `base.PrintFields()` en `B` invoca el método `PrintFields` declarado en `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="7db9f-928">Un *base_access* deshabilita el mecanismo de invocación virtual y simplemente trata el método base como un método no virtual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="7db9f-929">Una vez que se ha escrito la invocación en `B` `((A)this).PrintFields()`, invocaría de forma recursiva el método `PrintFields` declarado en `B`, no el declarado en `A`, ya que `PrintFields` es virtual y se `((A)this)` el tipo en tiempo de ejecución de `B`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="7db9f-930">Solo si se incluye un modificador `override`, un método puede reemplazar a otro método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="7db9f-931">En todos los demás casos, un método con la misma signatura que un método heredado simplemente oculta el método heredado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="7db9f-932">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-932">In the example</span></span>
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
<span data-ttu-id="7db9f-933">el método `F` de `B` no incluye un modificador `override` y, por tanto, no invalida el método `F` de `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="7db9f-934">En lugar de ello, el método `F` en `B` oculta el método en `A`y se indica una advertencia porque la declaración no incluye un modificador `new`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="7db9f-935">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-935">In the example</span></span>
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
<span data-ttu-id="7db9f-936">el método `F` de `B` oculta el método de `F` virtual heredado de `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="7db9f-937">Dado que el nuevo `F` en `B` tiene acceso privado, su ámbito solo incluye el cuerpo de la clase de `B` y no se extiende a `C`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="7db9f-938">Por lo tanto, la declaración de `F` en `C` puede invalidar el `F` heredado de `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="7db9f-939">Métodos sellados</span><span class="sxs-lookup"><span data-stu-id="7db9f-939">Sealed methods</span></span>

<span data-ttu-id="7db9f-940">Cuando una declaración de método de instancia incluye un modificador `sealed`, se dice que se trata de un método ***sellado***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="7db9f-941">Si una declaración de método de instancia incluye el modificador `sealed`, también debe incluir el modificador `override`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="7db9f-942">El uso del modificador `sealed` impide que una clase derivada Reemplace el método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="7db9f-943">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-943">In the example</span></span>
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
<span data-ttu-id="7db9f-944">la clase `B` proporciona dos métodos de invalidación: un método de `F` que tiene el modificador `sealed` y un método `G` que no lo hace.</span><span class="sxs-lookup"><span data-stu-id="7db9f-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="7db9f-945">`B`el uso de la `modifier` sellada impide que `C` Reemplace `F`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="7db9f-946">Métodos abstractos</span><span class="sxs-lookup"><span data-stu-id="7db9f-946">Abstract methods</span></span>

<span data-ttu-id="7db9f-947">Cuando una declaración de método de instancia incluye un modificador `abstract`, se dice que ese método es un ***método abstracto***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="7db9f-948">Aunque un método abstracto también es implícitamente un método virtual, no puede tener el modificador `virtual`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="7db9f-949">Una declaración de método abstracto presenta un nuevo método virtual, pero no proporciona una implementación de ese método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="7db9f-950">En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación mediante la invalidación de ese método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="7db9f-951">Dado que un método abstracto no proporciona ninguna implementación real, el *method_body* de un método abstracto simplemente consiste en un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="7db9f-952">Las declaraciones de método abstracto solo se permiten en clases abstractas ([clases abstractas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="7db9f-953">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-953">In the example</span></span>
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
<span data-ttu-id="7db9f-954">la clase `Shape` define la noción abstracta de un objeto de forma geométrica que puede dibujarse a sí mismo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="7db9f-955">El método `Paint` es abstracto porque no hay ninguna implementación predeterminada significativa.</span><span class="sxs-lookup"><span data-stu-id="7db9f-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="7db9f-956">Las clases `Ellipse` y `Box` son implementaciones concretas de `Shape`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="7db9f-957">Dado que estas clases no son abstractas, son necesarias para invalidar el método `Paint` y proporcionar una implementación real.</span><span class="sxs-lookup"><span data-stu-id="7db9f-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="7db9f-958">Se trata de un error en tiempo de compilación para que un *base_access* ([acceso base](expressions.md#base-access)) haga referencia a un método abstracto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="7db9f-959">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-959">In the example</span></span>
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
<span data-ttu-id="7db9f-960">se ha detectado un error en tiempo de compilación para la invocación de `base.F()` porque hace referencia a un método abstracto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="7db9f-961">Se permite que una declaración de método abstracto invalide un método virtual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="7db9f-962">Esto permite a una clase abstracta forzar la reimplementación del método en las clases derivadas y hace que la implementación original del método no esté disponible.</span><span class="sxs-lookup"><span data-stu-id="7db9f-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="7db9f-963">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-963">In the example</span></span>
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
<span data-ttu-id="7db9f-964">la clase `A` declara un método virtual, la clase `B` invalida este método con un método abstracto y la clase `C` invalida el método abstracto para proporcionar su propia implementación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="7db9f-965">Métodos externos</span><span class="sxs-lookup"><span data-stu-id="7db9f-965">External methods</span></span>

<span data-ttu-id="7db9f-966">Cuando una declaración de método incluye un modificador `extern`, se dice que se trata de un ***método externo***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="7db9f-967">Los métodos externos se implementan externamente, normalmente con un lenguaje C#distinto de.</span><span class="sxs-lookup"><span data-stu-id="7db9f-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="7db9f-968">Dado que una declaración de método externo no proporciona ninguna implementación real, el *method_body* de un método externo consiste simplemente en un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="7db9f-969">Es posible que un método externo no sea genérico.</span><span class="sxs-lookup"><span data-stu-id="7db9f-969">An external method may not be generic.</span></span>

<span data-ttu-id="7db9f-970">El modificador `extern` se utiliza normalmente junto con un atributo `DllImport` ([interoperación con componentes com y Win32](attributes.md#interoperation-with-com-and-win32-components)), permitiendo que los métodos externos se implementen mediante archivos DLL (bibliotecas de vínculos dinámicos).</span><span class="sxs-lookup"><span data-stu-id="7db9f-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="7db9f-971">El entorno de ejecución puede admitir otros mecanismos en los que se puedan proporcionar implementaciones de métodos externos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="7db9f-972">Cuando un método externo incluye un atributo `DllImport`, la declaración del método también debe incluir un modificador `static`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="7db9f-973">En este ejemplo se muestra el uso del modificador `extern` y el atributo `DllImport`:</span><span class="sxs-lookup"><span data-stu-id="7db9f-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="7db9f-974">Métodos parciales (Resumen)</span><span class="sxs-lookup"><span data-stu-id="7db9f-974">Partial methods (recap)</span></span>

<span data-ttu-id="7db9f-975">Cuando una declaración de método incluye un modificador `partial`, se dice que se trata de un ***método parcial***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="7db9f-976">Los métodos parciales solo se pueden declarar como miembros de tipos parciales ([tipos parciales](classes.md#partial-types)) y están sujetos a una serie de restricciones.</span><span class="sxs-lookup"><span data-stu-id="7db9f-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="7db9f-977">Los métodos parciales se describen con más detalle en [métodos parciales](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="7db9f-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="7db9f-978">Métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="7db9f-978">Extension methods</span></span>

<span data-ttu-id="7db9f-979">Cuando el primer parámetro de un método incluye el modificador `this`, se dice que se trata de un ***método de extensión***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="7db9f-980">Los métodos de extensión solo se pueden declarar en clases estáticas no anidadas no genéricas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="7db9f-981">El primer parámetro de un método de extensión no puede tener modificadores que no sean `this`y el tipo de parámetro no puede ser un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="7db9f-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="7db9f-982">El siguiente es un ejemplo de una clase estática que declara dos métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="7db9f-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="7db9f-983">Un método de extensión es un método estático normal.</span><span class="sxs-lookup"><span data-stu-id="7db9f-983">An extension method is a regular static method.</span></span> <span data-ttu-id="7db9f-984">Además, cuando la clase estática envolvente está en el ámbito, se puede invocar un método de extensión mediante la sintaxis de invocación de método de instancia ([invocaciones de método de extensión](expressions.md#extension-method-invocations)), mediante la expresión de receptor como primer argumento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="7db9f-985">El programa siguiente utiliza los métodos de extensión declarados anteriormente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="7db9f-986">El método `Slice` está disponible en el `string[]`y el método `ToInt32` está disponible en `string`, porque se han declarado como métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="7db9f-987">El significado del programa es el mismo que el que se indica a continuación, mediante llamadas ordinarias al método estático:</span><span class="sxs-lookup"><span data-stu-id="7db9f-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="7db9f-988">Cuerpo del método</span><span class="sxs-lookup"><span data-stu-id="7db9f-988">Method body</span></span>

<span data-ttu-id="7db9f-989">El *method_body* de una declaración de método consta de un cuerpo de bloque, un cuerpo de expresión o un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="7db9f-990">El ***tipo de resultado*** de un método es `void` si el tipo de valor devuelto es `void`, o si el método es Async y el tipo de valor devuelto es `System.Threading.Tasks.Task`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="7db9f-991">De lo contrario, el tipo de resultado de un método no asincrónico es el tipo de valor devuelto y el tipo de resultado de un método asincrónico con el tipo de valor devuelto `System.Threading.Tasks.Task<T>` es `T`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="7db9f-992">Cuando un método tiene un tipo de resultado `void` y un cuerpo de bloque, `return` instrucciones ([la instrucción return](statements.md#the-return-statement)) del bloque no pueden especificar una expresión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="7db9f-993">Si la ejecución del bloque de un método void se completa normalmente (es decir, el control fluye fuera del final del cuerpo del método), ese método simplemente vuelve a su llamador actual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="7db9f-994">Cuando un método tiene un resultado `void` y un cuerpo de expresión, la expresión `E` debe ser un *statement_expression*y el cuerpo es exactamente equivalente a un cuerpo de bloque del formulario `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="7db9f-995">Cuando un método tiene un tipo de resultado no void y un cuerpo de bloque, cada instrucción `return` del bloque debe especificar una expresión que se pueda convertir implícitamente al tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="7db9f-996">No se puede tener acceso al extremo de un cuerpo de bloque de un método que devuelve un valor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="7db9f-997">Es decir, en un método que devuelve valores con un cuerpo de bloque, el control no puede fluir fuera del final del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="7db9f-998">Cuando un método tiene un tipo de resultado no void y un cuerpo de expresión, la expresión se debe poder convertir implícitamente al tipo de resultado y el cuerpo es exactamente equivalente a un cuerpo de bloque del formulario `{ return E; }`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="7db9f-999">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-999">In the example</span></span>
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
<span data-ttu-id="7db9f-1000">el método `F` que devuelve el valor produce un error en tiempo de compilación porque el control puede fluir fuera del final del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="7db9f-1001">Los métodos `G` y `H` son correctos porque todas las rutas de acceso de ejecución posibles terminan en una instrucción return que especifica un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="7db9f-1002">El método `I` es correcto porque su cuerpo es equivalente a un bloque de instrucciones que solo contiene una instrucción return.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="7db9f-1003">Sobrecarga de métodos</span><span class="sxs-lookup"><span data-stu-id="7db9f-1003">Method overloading</span></span>

<span data-ttu-id="7db9f-1004">Las reglas de resolución de sobrecarga de método se describen en [inferencia de tipos](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="7db9f-1005">Propiedades</span><span class="sxs-lookup"><span data-stu-id="7db9f-1005">Properties</span></span>

<span data-ttu-id="7db9f-1006">Una ***propiedad*** es un miembro que proporciona acceso a una característica de un objeto o una clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="7db9f-1007">Entre los ejemplos de propiedades se incluyen la longitud de una cadena, el tamaño de una fuente, el título de una ventana, el nombre de un cliente, etc.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="7db9f-1008">Las propiedades son una extensión natural de los campos: ambos son miembros con nombre con tipos asociados y la sintaxis para tener acceso a campos y propiedades es la misma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="7db9f-1009">Sin embargo, a diferencia de los campos, las propiedades no denotan ubicaciones de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="7db9f-1010">Las propiedades tienen ***descriptores de acceso*** que especifican las instrucciones que se ejecutan cuando se leen o escriben sus valores.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="7db9f-1011">Por tanto, las propiedades proporcionan un mecanismo para asociar acciones con la lectura y escritura de los atributos de un objeto; Además, permiten calcular tales atributos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="7db9f-1012">Las propiedades se declaran mediante *property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="7db9f-1013">Un *property_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso[(modificadores de acceso](classes.md#access-modifiers)), el `new` ([modificador nuevo](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` (métodos de[invalidación](classes.md#override-methods)), `sealed` (métodos[sellados](classes.md#sealed-methods)), `abstract` (métodos[abstractos](classes.md#abstract-methods)) y modificadores `extern` ([métodos](classes.md#external-methods)</span><span class="sxs-lookup"><span data-stu-id="7db9f-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="7db9f-1014">Las declaraciones de propiedad están sujetas a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="7db9f-1015">El *tipo* de una declaración de propiedad especifica el tipo de la propiedad introducida por la declaración y el *member_name* especifica el nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="7db9f-1016">A menos que la propiedad sea una implementación explícita de un miembro de interfaz, el *member_name* es simplemente un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="7db9f-1017">En el caso de una implementación explícita de un miembro de interfaz ([implementaciones de miembros de interfaz explícitos](interfaces.md#explicit-interface-member-implementations)), el *member_name* consta de un *interface_type* seguido de un "`.`" y un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="7db9f-1018">El *tipo* de una propiedad debe ser al menos igual de accesible que la propiedad en sí ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="7db9f-1019">Un *property_body* puede consistir en un ***cuerpo de descriptor de acceso*** o en un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="7db9f-1020">En el cuerpo de un descriptor de acceso, *accessor_declarations*, que se deben incluir en los tokens "`{`" y "`}`", declare los descriptores de acceso ([descriptores de acceso](classes.md#accessors)) de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="7db9f-1021">Los descriptores de acceso especifican las instrucciones ejecutables asociadas a la lectura y la escritura de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="7db9f-1022">Un cuerpo de expresión que consta de `=>` seguido de una *expresión* `E` y un punto y coma es exactamente equivalente al cuerpo de la instrucción `{ get { return E; } }`y, por lo tanto, solo se puede usar para especificar las propiedades de solo captador en las que una sola expresión proporciona el resultado del captador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="7db9f-1023">Solo se puede proporcionar un *property_initializer* para una propiedad implementada automáticamente ([propiedades implementadas automáticamente](classes.md#automatically-implemented-properties)) y provoca la inicialización del campo subyacente de dichas propiedades con el valor especificado por la *expresión*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="7db9f-1024">Aunque la sintaxis para tener acceso a una propiedad es la misma que la de un campo, una propiedad no se clasifica como una variable.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="7db9f-1025">Por lo tanto, no es posible pasar una propiedad como `ref` o `out` argumento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="7db9f-1026">Cuando una declaración de propiedad incluye un modificador `extern`, se dice que la propiedad es una ***propiedad externa***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="7db9f-1027">Dado que una declaración de propiedad externa no proporciona ninguna implementación real, cada una de sus *accessor_declarations* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="7db9f-1028">Propiedades estáticas y de instancia</span><span class="sxs-lookup"><span data-stu-id="7db9f-1028">Static and instance properties</span></span>

<span data-ttu-id="7db9f-1029">Cuando una declaración de propiedad incluye un modificador `static`, se dice que la propiedad es una ***propiedad estática***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="7db9f-1030">Cuando no existe ningún modificador `static`, se dice que la propiedad es una ***propiedad de instancia***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="7db9f-1031">Una propiedad estática no está asociada a una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en los descriptores de acceso de una propiedad estática.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="7db9f-1032">Una propiedad de instancia está asociada a una instancia determinada de una clase y se puede tener acceso a esa instancia como `this` ([este acceso](expressions.md#this-access)) en los descriptores de acceso de esa propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="7db9f-1033">Cuando se hace referencia a una propiedad en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`, si `M` es una propiedad estática, `E` debe indicar un tipo que contiene `M`y, si `M` es una propiedad de instancia, E debe denotar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="7db9f-1034">Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="7db9f-1035">Descriptores</span><span class="sxs-lookup"><span data-stu-id="7db9f-1035">Accessors</span></span>

<span data-ttu-id="7db9f-1036">El *accessor_declarations* de una propiedad especifica las instrucciones ejecutables asociadas a la lectura y escritura de esa propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="7db9f-1037">Las declaraciones de descriptor de acceso constan de un *get_accessor_declaration*, un *set_accessor_declaration*o ambos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="7db9f-1038">Cada declaración de descriptor de acceso está formada por el token `get` o `set` seguido de un *accessor_modifier* opcional y un *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="7db9f-1039">El uso de *accessor_modifier*s se rige por las siguientes restricciones:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="7db9f-1040">Un *accessor_modifier* no se puede usar en una interfaz o en una implementación explícita de un miembro de interfaz.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="7db9f-1041">En el caso de una propiedad o un indizador que no tiene ningún modificador `override`, solo se permite una *accessor_modifier* si la propiedad o indizador tiene un descriptor de acceso `get` y `set` y, a continuación, solo se permite en uno de esos descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="7db9f-1042">En el caso de una propiedad o un indizador que incluya un modificador `override`, un descriptor de acceso debe coincidir con el *accessor_modifier*, si existe, del descriptor de acceso que se va a invalidar.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="7db9f-1043">El *accessor_modifier* debe declarar una accesibilidad que sea estrictamente más restrictiva que la accesibilidad declarada de la propiedad o el propio indexador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="7db9f-1044">Para ser precisos:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1044">To be precise:</span></span>
   * <span data-ttu-id="7db9f-1045">Si la propiedad o indizador tiene una accesibilidad declarada de `public`, el *accessor_modifier* puede ser `protected internal`, `internal`, `protected`o `private`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="7db9f-1046">Si la propiedad o indizador tiene una accesibilidad declarada de `protected internal`, el *accessor_modifier* puede ser `internal`, `protected`o `private`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="7db9f-1047">Si la propiedad o indizador tiene una accesibilidad declarada de `internal` o `protected`, se debe `private`la *accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="7db9f-1048">Si la propiedad o indizador tiene una accesibilidad declarada de `private`, no se puede usar ningún *accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="7db9f-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="7db9f-1049">En el caso de las propiedades `abstract` y `extern`, el *accessor_body* para cada descriptor de acceso especificado es simplemente un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="7db9f-1050">Una propiedad no abstracta y no externa puede tener cada *accessor_body* ser un punto y coma, en cuyo caso es una ***propiedad implementada automáticamente*** ([propiedades implementadas automáticamente](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="7db9f-1051">Una propiedad implementada automáticamente debe tener al menos un descriptor de acceso get.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="7db9f-1052">En el caso de los descriptores de acceso de cualquier otra propiedad no abstracta no externa, el *accessor_body* es un *bloque* que especifica las instrucciones que se van a ejecutar cuando se invoca el descriptor de acceso correspondiente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="7db9f-1053">Un descriptor de acceso `get` corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="7db9f-1054">Excepto como destino de una asignación, cuando se hace referencia a una propiedad en una expresión, el descriptor de acceso `get` de la propiedad se invoca para calcular el valor de la propiedad ([valores de las expresiones](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="7db9f-1055">El cuerpo de un descriptor de acceso `get` debe ajustarse a las reglas de los métodos que devuelven valores descritos en el [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="7db9f-1056">En concreto, todas las instrucciones `return` en el cuerpo de un descriptor de acceso `get` deben especificar una expresión que se pueda convertir implícitamente al tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="7db9f-1057">Además, el punto de conexión de un descriptor de acceso `get` no debe ser accesible.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="7db9f-1058">Un descriptor de acceso `set` corresponde a un método con un parámetro de valor único del tipo de propiedad y un tipo de valor devuelto `void`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="7db9f-1059">El parámetro implícito de un descriptor de acceso `set` siempre se denomina `value`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="7db9f-1060">Cuando se hace referencia a una propiedad como el destino de una asignación[(operadores de asignación](expressions.md#assignment-operators)), o como operando de `++` o `--` (operadores de[incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)de [postfijo, operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)), el descriptor de acceso de `set` se invoca con un argumento (cuyo valor es el del lado derecho de la asignación o el operando del operador `++` o `--`) que proporciona el nuevo valor ([asignación simple](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="7db9f-1061">El cuerpo de un descriptor de acceso `set` debe ajustarse a las reglas de `void` métodos descritos en el [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="7db9f-1062">En concreto, `return` instrucciones del cuerpo del descriptor de acceso `set` no pueden especificar una expresión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="7db9f-1063">Dado que un descriptor de acceso `set` tiene implícitamente un parámetro denominado `value`, se trata de un error en tiempo de compilación para que una declaración de constante o variable local de un descriptor de acceso de `set` tenga ese nombre.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="7db9f-1064">En función de la presencia o ausencia de los descriptores de acceso `get` y `set`, se clasifica una propiedad de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="7db9f-1065">Una propiedad que incluye tanto un descriptor de acceso `get` como un descriptor de acceso de `set` se dice que es una propiedad ***de lectura y escritura*** .</span><span class="sxs-lookup"><span data-stu-id="7db9f-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="7db9f-1066">Una propiedad que solo tiene un descriptor de acceso `get` se dice que es una propiedad ***de solo lectura*** .</span><span class="sxs-lookup"><span data-stu-id="7db9f-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="7db9f-1067">Es un error en tiempo de compilación que una propiedad de solo lectura sea el destino de una asignación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="7db9f-1068">Se dice que una propiedad que solo tiene un descriptor de acceso `set` es una propiedad ***de solo escritura*** .</span><span class="sxs-lookup"><span data-stu-id="7db9f-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="7db9f-1069">Excepto como destino de una asignación, se trata de un error en tiempo de compilación para hacer referencia a una propiedad de solo escritura en una expresión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="7db9f-1070">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1070">In the example</span></span>
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
<span data-ttu-id="7db9f-1071">el control `Button` declara una propiedad de `Caption` pública.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="7db9f-1072">El descriptor de acceso `get` de la propiedad `Caption` devuelve la cadena almacenada en el campo de `caption` privada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="7db9f-1073">El descriptor de acceso `set` comprueba si el nuevo valor es diferente del valor actual y, en ese caso, almacena el nuevo valor y vuelve a dibujar el control.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="7db9f-1074">Las propiedades suelen seguir el patrón mostrado anteriormente: el descriptor de acceso `get` simplemente devuelve un valor almacenado en un campo privado y el descriptor de acceso `set` modifica ese campo privado y, a continuación, realiza las acciones adicionales necesarias para actualizar por completo el estado del objeto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="7db9f-1075">Dado el `Button` clase anterior, el siguiente es un ejemplo de uso de la propiedad `Caption`:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="7db9f-1076">En este caso, el descriptor de acceso `set` se invoca asignando un valor a la propiedad y el descriptor de acceso `get` se invoca haciendo referencia a la propiedad en una expresión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="7db9f-1077">Los descriptores de acceso `get` y `set` de una propiedad no son miembros distintos y no es posible declarar los descriptores de acceso de una propiedad por separado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="7db9f-1078">Como tal, no es posible que los dos descriptores de acceso de una propiedad de lectura y escritura tengan una accesibilidad diferente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="7db9f-1079">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1079">The example</span></span>
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
<span data-ttu-id="7db9f-1080">no declara una única propiedad de lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="7db9f-1081">En su lugar, declara dos propiedades con el mismo nombre, una de solo lectura y otra de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="7db9f-1082">Dado que dos miembros declarados en la misma clase no pueden tener el mismo nombre, en el ejemplo se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="7db9f-1083">Cuando una clase derivada declara una propiedad con el mismo nombre que una propiedad heredada, la propiedad derivada oculta la propiedad heredada con respecto a la lectura y la escritura.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="7db9f-1084">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1084">In the example</span></span>
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
<span data-ttu-id="7db9f-1085">la propiedad `P` de `B` oculta la propiedad `P` en `A` con respecto a la lectura y la escritura.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="7db9f-1086">Por lo tanto, en las instrucciones</span><span class="sxs-lookup"><span data-stu-id="7db9f-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="7db9f-1087">la asignación a `b.P` produce un error en tiempo de compilación, ya que la propiedad `P` de solo lectura de `B` oculta la propiedad `P` de solo escritura en `A`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="7db9f-1088">Sin embargo, tenga en cuenta que se puede usar una conversión para tener acceso a la propiedad Hidden `P`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="7db9f-1089">A diferencia de los campos públicos, las propiedades proporcionan una separación entre el estado interno de un objeto y su interfaz pública.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="7db9f-1090">Considere el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1090">Consider the example:</span></span>
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

<span data-ttu-id="7db9f-1091">En este caso, la clase `Label` usa dos campos `int`, `x` y `y`, para almacenar su ubicación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="7db9f-1092">La ubicación se expone públicamente como un `X` y una propiedad `Y` y como una propiedad `Location` de tipo `Point`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="7db9f-1093">Si, en una versión futura de `Label`, resulta más cómodo almacenar la ubicación como `Point` internamente, el cambio se puede realizar sin que afecte a la interfaz pública de la clase:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="7db9f-1094">Si `x` y `y` en su lugar se `public readonly` campos, habría sido imposible realizar este cambio en la clase de `Label`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="7db9f-1095">Exponer el estado a través de las propiedades no es necesariamente menos eficaz que exponer los campos directamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="7db9f-1096">En concreto, cuando una propiedad no es virtual y contiene solo una pequeña cantidad de código, el entorno de ejecución puede reemplazar las llamadas a los descriptores de acceso con el código real de los descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="7db9f-1097">Este proceso se ***conoce como inserción y hace***que el acceso a la propiedad sea tan eficaz como el acceso al campo, pero conserva la mayor flexibilidad de las propiedades.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="7db9f-1098">Dado que la invocación de un descriptor de acceso `get` es conceptualmente equivalente a leer el valor de un campo, se considera un estilo de programación incorrecto para que los descriptores de acceso `get` tengan efectos secundarios observables.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="7db9f-1099">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="7db9f-1100">el valor de la propiedad `Next` depende del número de veces que se ha accedido previamente a la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="7db9f-1101">Por lo tanto, el acceso a la propiedad produce un efecto secundario observable y la propiedad debe implementarse como un método en su lugar.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="7db9f-1102">La Convención "sin efectos secundarios" para los descriptores de acceso `get` no significa que los descriptores de acceso de `get` siempre se deben escribir para devolver los valores almacenados en los campos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="7db9f-1103">En realidad, los descriptores de acceso `get` suelen calcular el valor de una propiedad mediante el acceso a varios campos o la invocación de métodos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="7db9f-1104">Sin embargo, un descriptor de acceso de `get` diseñado correctamente no realiza ninguna acción que produzca cambios observables en el estado del objeto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="7db9f-1105">Las propiedades se pueden usar para retrasar la inicialización de un recurso hasta el momento en el que se hace referencia a él por primera vez.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="7db9f-1106">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1106">For example:</span></span>
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

<span data-ttu-id="7db9f-1107">La clase `Console` contiene tres propiedades, `In`, `Out`y `Error`, que representan los dispositivos de entrada, salida y error estándar, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="7db9f-1108">Al exponer estos miembros como propiedades, la clase `Console` puede retrasar su inicialización hasta que se usen realmente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="7db9f-1109">Por ejemplo, al hacer referencia por primera vez a la propiedad `Out`, como en</span><span class="sxs-lookup"><span data-stu-id="7db9f-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="7db9f-1110">se crea el `TextWriter` subyacente para el dispositivo de salida.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="7db9f-1111">Pero si la aplicación no hace referencia a las propiedades `In` y `Error`, no se crea ningún objeto para esos dispositivos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="7db9f-1112">Propiedades implementadas automáticamente</span><span class="sxs-lookup"><span data-stu-id="7db9f-1112">Automatically implemented properties</span></span>

<span data-ttu-id="7db9f-1113">Una propiedad implementada automáticamente (o ***propiedad automática*** para abreviar) es una propiedad no abstracta no abstracta con cuerpos de descriptor de acceso solo de punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="7db9f-1114">Las propiedades automáticas deben tener un descriptor de acceso get y, opcionalmente, tener un descriptor de acceso set.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="7db9f-1115">Cuando se especifica una propiedad como una propiedad implementada automáticamente, un campo de respaldo oculto está disponible automáticamente para la propiedad y los descriptores de acceso se implementan para leer y escribir en ese campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="7db9f-1116">Si la propiedad automática no tiene ningún descriptor de acceso set, el campo de respaldo se considera `readonly` ([campos de solo lectura](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="7db9f-1117">Al igual que un campo de `readonly`, también se puede asignar una propiedad automática de solo captador en el cuerpo de un constructor de la clase envolvente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="7db9f-1118">Este tipo de asignación asigna directamente al campo de respaldo de solo lectura de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="7db9f-1119">Una propiedad automática puede tener opcionalmente un *property_initializer*, que se aplica directamente al campo de respaldo como un *variable_initializer* ([inicializadores de variables](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="7db9f-1120">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="7db9f-1121">es equivalente a la siguiente declaración:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="7db9f-1122">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="7db9f-1123">es equivalente a la siguiente declaración:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="7db9f-1124">Observe que las asignaciones al campo de solo lectura son válidas, ya que se producen dentro del constructor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="7db9f-1125">Accesibilidad</span><span class="sxs-lookup"><span data-stu-id="7db9f-1125">Accessibility</span></span>

<span data-ttu-id="7db9f-1126">Si un descriptor de acceso tiene un *accessor_modifier*, el dominio de accesibilidad ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)) del descriptor de acceso se determina mediante la accesibilidad declarada de la *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="7db9f-1127">Si un descriptor de acceso no tiene un *accessor_modifier*, el dominio de accesibilidad del descriptor de acceso se determina a partir de la accesibilidad declarada de la propiedad o del indizador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="7db9f-1128">La presencia de un *accessor_modifier* nunca afecta a la búsqueda de miembros ([operadores](expressions.md#operators)) o a la resolución de sobrecarga ([resolución de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="7db9f-1129">Los modificadores de la propiedad o el indizador siempre determinan la propiedad o el indizador que está enlazado, independientemente del contexto del acceso.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="7db9f-1130">Una vez que se ha seleccionado una propiedad o un indizador determinados, se usan los dominios de accesibilidad de los descriptores de acceso específicos implicados para determinar si ese uso es válido:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="7db9f-1131">Si el uso es como un valor ([valores de expresiones](expressions.md#values-of-expressions)), el descriptor de acceso `get` debe existir y ser accesible.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="7db9f-1132">Si el uso es como el destino de una asignación simple ([asignación simple](expressions.md#simple-assignment)), el descriptor de acceso `set` debe existir y ser accesible.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="7db9f-1133">Si el uso es como destino de la asignación compuesta ([asignación compuesta](expressions.md#compound-assignment)) o como destino de los operadores `++` o `--` (miembros de[función](expressions.md#function-members)0,9, [expresiones de invocación](expressions.md#invocation-expressions)), los descriptores de acceso `get` y `set` deben existir y ser accesibles.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="7db9f-1134">En el ejemplo siguiente, el `B.Text`de propiedad oculta la propiedad `A.Text`, incluso en contextos donde solo se llama al descriptor de acceso `set`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="7db9f-1135">Por el contrario, la propiedad `B.Count` no es accesible para el `M`de clase, por lo que en su lugar se usa la propiedad accesible `A.Count`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="7db9f-1136">Un descriptor de acceso que se utiliza para implementar una interfaz no puede tener un *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="7db9f-1137">Si solo se usa un descriptor de acceso para implementar una interfaz, el otro descriptor de acceso se puede declarar con un *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="7db9f-1138">Descriptores de acceso de propiedad virtual, Sealed, override y Abstract</span><span class="sxs-lookup"><span data-stu-id="7db9f-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="7db9f-1139">Una declaración de propiedad `virtual` especifica que los descriptores de acceso de la propiedad son virtuales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="7db9f-1140">El modificador `virtual` se aplica a ambos descriptores de acceso de una propiedad de lectura y escritura, no es posible que solo un descriptor de acceso de una propiedad de lectura y escritura sea virtual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="7db9f-1141">Una declaración de propiedad `abstract` especifica que los descriptores de acceso de la propiedad son virtuales, pero no proporciona una implementación real de los descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="7db9f-1142">En su lugar, se requiere que las clases derivadas no abstractas proporcionen su propia implementación para los descriptores de acceso mediante la invalidación de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="7db9f-1143">Dado que un descriptor de acceso de una declaración de propiedad abstracta no proporciona ninguna implementación real, su *accessor_body* simplemente consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="7db9f-1144">Una declaración de propiedad que incluye los modificadores `abstract` y `override` especifica que la propiedad es abstracta e invalida una propiedad base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="7db9f-1145">Los descriptores de acceso de dicha propiedad también son abstractos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="7db9f-1146">Las declaraciones de propiedad abstracta solo se permiten en clases abstractas ([clases abstractas](classes.md#abstract-classes)). Los descriptores de acceso de una propiedad virtual heredada se pueden invalidar en una clase derivada mediante la inclusión de una declaración de propiedad que especifique una directiva de `override`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="7db9f-1147">Esto se conoce como una ***declaración de propiedad de reemplazo***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="7db9f-1148">Una declaración de propiedad de reemplazo no declara una nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="7db9f-1149">En su lugar, simplemente especializa las implementaciones de los descriptores de acceso de una propiedad virtual existente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="7db9f-1150">Una declaración de propiedad de reemplazo debe especificar exactamente los mismos modificadores de accesibilidad, tipo y nombre que la propiedad heredada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="7db9f-1151">Si la propiedad heredada solo tiene un descriptor de acceso (es decir, si la propiedad heredada es de solo lectura o de solo escritura), la propiedad de reemplazo solo debe incluir ese descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="7db9f-1152">Si la propiedad heredada incluye ambos descriptores de acceso (es decir, si la propiedad heredada es de lectura y escritura), la propiedad de reemplazo puede incluir un solo descriptor de acceso o ambos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="7db9f-1153">Una declaración de propiedad de reemplazo puede incluir el modificador `sealed`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="7db9f-1154">El uso de este modificador evita que una clase derivada Reemplace la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="7db9f-1155">Los descriptores de acceso de una propiedad sellada también están sellados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="7db9f-1156">A excepción de las diferencias en la sintaxis de declaración e invocación, los descriptores de acceso virtual, sellado, invalidación y Abstract se comportan exactamente igual que los métodos virtuales, sellados, de invalidación y abstractos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="7db9f-1157">En concreto, las reglas descritas en [métodos virtuales](classes.md#virtual-methods), [métodos de invalidación](classes.md#override-methods), [métodos sellados](classes.md#sealed-methods)y [métodos abstractos](classes.md#abstract-methods) se aplican como si los descriptores de acceso fueran métodos de una forma correspondiente:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="7db9f-1158">Un descriptor de acceso `get` corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad y los mismos modificadores que la propiedad que lo contiene.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="7db9f-1159">Un descriptor de acceso `set` corresponde a un método con un parámetro de valor único del tipo de propiedad, un tipo de valor devuelto `void` y los mismos modificadores que la propiedad que lo contiene.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="7db9f-1160">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1160">In the example</span></span>
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
<span data-ttu-id="7db9f-1161">`X` es una propiedad virtual de solo lectura, `Y` es una propiedad de lectura y escritura virtual y `Z` es una propiedad de lectura y escritura abstracta.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="7db9f-1162">Dado que `Z` es abstracto, la clase contenedora `A` también debe declararse como abstracta.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="7db9f-1163">A continuación se muestra una clase que deriva de `A`:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="7db9f-1164">Aquí, las declaraciones de `X`, `Y`y `Z` invalidan las declaraciones de propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="7db9f-1165">Cada declaración de propiedad coincide exactamente con los modificadores de accesibilidad, el tipo y el nombre de la propiedad heredada correspondiente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="7db9f-1166">El descriptor de acceso `get` de `X` y el descriptor de acceso `set` de `Y` utilizan la palabra clave `base` para tener acceso a los descriptores de acceso heredados</span><span class="sxs-lookup"><span data-stu-id="7db9f-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="7db9f-1167">La declaración de `Z` invalida ambos descriptores de acceso abstractos; por tanto, no hay miembros de función abstracta pendientes en `B`y se permite que `B` sea una clase no abstracta.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="7db9f-1168">Cuando una propiedad se declara como un `override`, cualquier descriptor de acceso invalidado debe ser accesible para el código de invalidación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="7db9f-1169">Además, la accesibilidad declarada de la propiedad o del indexador, y de los descriptores de acceso, debe coincidir con la del miembro y los descriptores de acceso invalidados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="7db9f-1170">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="7db9f-1171">Eventos</span><span class="sxs-lookup"><span data-stu-id="7db9f-1171">Events</span></span>

<span data-ttu-id="7db9f-1172">Un ***evento*** es un miembro que permite a un objeto o una clase proporcionar notificaciones.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="7db9f-1173">Los clientes pueden adjuntar código ejecutable para eventos proporcionando ***controladores de eventos***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="7db9f-1174">Los eventos se declaran mediante *event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="7db9f-1175">Un *event_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso[(modificadores de acceso](classes.md#access-modifiers)), el `new` ([modificador nuevo](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` (métodos de[invalidación](classes.md#override-methods)), `sealed` (métodos[sellados](classes.md#sealed-methods)), `abstract` (métodos[abstractos](classes.md#abstract-methods)) y modificadores `extern` ([métodos](classes.md#external-methods)</span><span class="sxs-lookup"><span data-stu-id="7db9f-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="7db9f-1176">Las declaraciones de eventos están sujetas a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="7db9f-1177">El *tipo* de una declaración de evento debe ser un *delegate_type* ([tipos de referencia](types.md#reference-types)) y ese *delegate_type* debe ser al menos igual de accesible que el propio evento (restricciones de[accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="7db9f-1178">Una declaración de evento puede incluir *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="7db9f-1179">Sin embargo, si no, para los eventos no abstractos y no externos, el compilador los proporciona automáticamente ([eventos similares a los campos](classes.md#field-like-events)); en el caso de los eventos extern, los descriptores de acceso se proporcionan externamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="7db9f-1180">Una declaración de evento que omite *event_accessor_declarations* define uno o más eventos, uno para cada una de las *variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="7db9f-1181">Los atributos y modificadores se aplican a todos los miembros declarados por este tipo de *event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="7db9f-1182">Se trata de un error en tiempo de compilación para que una *event_declaration* incluya tanto el modificador `abstract` como *event_accessor_declarations*delimitado por llaves.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="7db9f-1183">Cuando una declaración de evento incluye un modificador `extern`, se dice que se trata de un ***evento externo***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="7db9f-1184">Dado que una declaración de evento externo no proporciona ninguna implementación real, es un error que incluye el modificador `extern` y el *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="7db9f-1185">Se trata de un error en tiempo de compilación para una *variable_declarator* de una declaración de evento con un modificador `abstract` o `external` para incluir un *variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="7db9f-1186">Un evento se puede usar como operando izquierdo de los operadores `+=` y `-=` (asignación de[eventos](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="7db9f-1187">Estos operadores se utilizan, respectivamente, para adjuntar controladores de eventos a o para quitar controladores de eventos de un evento, y los modificadores de acceso del evento controlan los contextos en los que se permiten esas operaciones.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="7db9f-1188">Como `+=` y `-=` son las únicas operaciones que se permiten en un evento fuera del tipo que declara el evento, el código externo puede Agregar y quitar controladores para un evento, pero no puede obtener o modificar la lista subyacente de controladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="7db9f-1189">En una operación con el formato `x += y` o `x -= y`, cuando `x` es un evento y la referencia tiene lugar fuera del tipo que contiene la declaración de `x`, el resultado de la operación tiene el tipo `void` (en lugar de tener el tipo de `x`, con el valor de `x` después de la asignación).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="7db9f-1190">Esta regla prohíbe que el código externo examine indirectamente el delegado subyacente de un evento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="7db9f-1191">En el ejemplo siguiente se muestra cómo se adjuntan los controladores de eventos a las instancias de la clase `Button`:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="7db9f-1192">En este caso, el constructor de instancia de `LoginDialog` crea dos instancias de `Button` y asocia controladores de eventos a los eventos `Click`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="7db9f-1193">Eventos similares a campos</span><span class="sxs-lookup"><span data-stu-id="7db9f-1193">Field-like events</span></span>

<span data-ttu-id="7db9f-1194">En el texto del programa de la clase o el struct que contiene la declaración de un evento, se pueden usar determinados eventos como campos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="7db9f-1195">Para usarse de esta manera, un evento no debe estar `abstract` ni `extern`y no debe incluir explícitamente *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="7db9f-1196">Este tipo de evento se puede utilizar en cualquier contexto que permita un campo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="7db9f-1197">El campo contiene un delegado ([delegados](delegates.md)) que hace referencia a la lista de controladores de eventos que se han agregado al evento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="7db9f-1198">Si no se ha agregado ningún controlador de eventos, el campo contiene `null`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="7db9f-1199">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1199">In the example</span></span>
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
<span data-ttu-id="7db9f-1200">`Click` se utiliza como un campo dentro de la clase `Button`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="7db9f-1201">Como muestra el ejemplo, el campo se puede examinar, modificar y usar en expresiones de invocación de delegado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="7db9f-1202">El método `OnClick` de la clase `Button` "genera" el evento `Click`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="7db9f-1203">La noción de generar un evento es equivalente exactamente a invocar el delegado representado por el evento; por lo tanto, no hay ninguna construcción especial de lenguaje para generar eventos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="7db9f-1204">Tenga en cuenta que la invocación del delegado está precedida por una comprobación que garantiza que el delegado no es NULL.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="7db9f-1205">Fuera de la declaración de la clase `Button`, el miembro de `Click` solo se puede usar en el lado izquierdo de los operadores `+=` y `-=`, como en</span><span class="sxs-lookup"><span data-stu-id="7db9f-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="7db9f-1206">que anexa un delegado a la lista de invocaciones del evento `Click`, y</span><span class="sxs-lookup"><span data-stu-id="7db9f-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="7db9f-1207">que quita un delegado de la lista de invocaciones del evento `Click`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="7db9f-1208">Al compilar un evento como un campo, el compilador crea automáticamente el almacenamiento para contener el delegado y crea los descriptores de acceso para el evento que agregan o quitan controladores de eventos para el campo delegado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="7db9f-1209">Las operaciones de adición y eliminación son seguras para subprocesos y puede (pero no es necesario que) se realicen mientras se mantiene el bloqueo ([la instrucción lock](statements.md#the-lock-statement)) en el objeto contenedor para un evento de instancia o el objeto de tipo ([expresiones de creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions)) para un evento estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="7db9f-1210">Por lo tanto, una declaración de evento de instancia con el formato:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="7db9f-1211">se compilará en algo equivalente a:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="7db9f-1212">Dentro de la `X`de la clase, las referencias a `Ev` en el lado izquierdo de los operadores `+=` y `-=` provocan que se invoquen los descriptores de acceso add y Remove.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="7db9f-1213">Todas las demás referencias a `Ev` se compilan para hacer referencia al campo oculto `__Ev` en su lugar ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="7db9f-1214">El nombre "`__Ev`" es arbitrario; el campo oculto puede tener cualquier nombre o ningún nombre.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="7db9f-1215">Descriptores de acceso de un evento</span><span class="sxs-lookup"><span data-stu-id="7db9f-1215">Event accessors</span></span>

<span data-ttu-id="7db9f-1216">Las declaraciones de evento normalmente omiten *event_accessor_declarations*, como en el ejemplo de `Button` anterior.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="7db9f-1217">Una situación para hacerlo implica el caso en el que el costo de almacenamiento de un campo por evento no es aceptable.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="7db9f-1218">En tales casos, una clase puede incluir *event_accessor_declarations* y utilizar un mecanismo privado para almacenar la lista de controladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="7db9f-1219">En el *event_accessor_declarations* de un evento se especifican las instrucciones ejecutables asociadas a la adición y eliminación de controladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="7db9f-1220">Las declaraciones de descriptor de acceso constan de un *add_accessor_declaration* y un *remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="7db9f-1221">Cada declaración de descriptor de acceso está formada por el token `add` o `remove` seguido de un *bloque*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="7db9f-1222">El *bloque* asociado a un *add_accessor_declaration* especifica las instrucciones que se ejecutarán cuando se agregue un controlador de eventos y el *bloque* asociado a un *remove_accessor_declaration* especifica las instrucciones que se ejecutarán cuando se quite un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="7db9f-1223">Cada *add_accessor_declaration* y *remove_accessor_declaration* corresponde a un método con un parámetro de valor único del tipo de evento y un tipo de valor devuelto `void`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="7db9f-1224">El parámetro implícito de un descriptor de acceso de eventos se denomina `value`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="7db9f-1225">Cuando se utiliza un evento en una asignación de eventos, se usa el descriptor de acceso de eventos adecuado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="7db9f-1226">En concreto, si el operador de asignación es `+=`, se usa el descriptor de acceso add y, si el operador de asignación es `-=`, se usa el descriptor de acceso Remove.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="7db9f-1227">En cualquier caso, el operando derecho del operador de asignación se utiliza como argumento para el descriptor de acceso de eventos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="7db9f-1228">El bloque de una *add_accessor_declaration* o un *remove_accessor_declaration* debe ajustarse a las reglas de los métodos de `void` descritos en el [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="7db9f-1229">En concreto, `return` instrucciones de este tipo de bloque no pueden especificar una expresión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="7db9f-1230">Dado que un descriptor de acceso de eventos tiene implícitamente un parámetro denominado `value`, es un error en tiempo de compilación para una variable local o una constante declarada en un descriptor de acceso de eventos para que tenga ese nombre.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="7db9f-1231">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1231">In the example</span></span>
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
<span data-ttu-id="7db9f-1232">la clase `Control` implementa un mecanismo de almacenamiento interno para los eventos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="7db9f-1233">El método `AddEventHandler` asocia un valor de delegado a una clave, el método `GetEventHandler` devuelve el delegado asociado actualmente a una clave y el método `RemoveEventHandler` quita un delegado como controlador de eventos para el evento especificado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="7db9f-1234">Presumiblemente, el mecanismo de almacenamiento subyacente está diseñado de forma que no hay ningún costo por asociar un `null` valor de delegado con una clave y, por lo tanto, los eventos no controlados no consumen almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="7db9f-1235">Eventos estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="7db9f-1235">Static and instance events</span></span>

<span data-ttu-id="7db9f-1236">Cuando una declaración de evento incluye un modificador `static`, se dice que se trata de un ***evento estático***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="7db9f-1237">Cuando no hay ningún modificador `static`, se dice que el evento es un ***evento de instancia***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="7db9f-1238">Un evento estático no está asociado a una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en los descriptores de acceso de un evento estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="7db9f-1239">Un evento de instancia está asociado a una instancia determinada de una clase y se puede tener acceso a esta instancia como `this` ([este acceso](expressions.md#this-access)) en los descriptores de acceso de ese evento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="7db9f-1240">Cuando se hace referencia a un evento en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`, si `M` es un evento estático, `E` debe indicar un tipo que contiene `M`y, si `M` es un evento de instancia, E debe denotar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="7db9f-1241">Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="7db9f-1242">Descriptores de acceso de eventos virtuales, sellados, de invalidación y abstractos</span><span class="sxs-lookup"><span data-stu-id="7db9f-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="7db9f-1243">Una declaración de evento `virtual` especifica que los descriptores de acceso de ese evento son virtuales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="7db9f-1244">El modificador `virtual` se aplica a ambos descriptores de acceso de un evento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="7db9f-1245">Una declaración de evento `abstract` especifica que los descriptores de acceso del evento son virtuales, pero no proporciona una implementación real de los descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="7db9f-1246">En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación para los descriptores de acceso invalidando el evento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="7db9f-1247">Dado que una declaración de evento abstracto no proporciona ninguna implementación real, no puede proporcionar *event_accessor_declarations*delimitados por llaves.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="7db9f-1248">Una declaración de evento que incluye los modificadores `abstract` y `override` especifica que el evento es abstracto e invalida un evento base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="7db9f-1249">Los descriptores de acceso de este tipo de evento también son abstractos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="7db9f-1250">Las declaraciones de eventos abstractos solo se permiten en clases abstractas ([clases abstractas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="7db9f-1251">Los descriptores de acceso de un evento virtual heredado se pueden invalidar en una clase derivada mediante la inclusión de una declaración de evento que especifique un modificador `override`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="7db9f-1252">Esto se conoce como una ***declaración de evento de reemplazo***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="7db9f-1253">Una declaración de evento de reemplazo no declara un nuevo evento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="7db9f-1254">En su lugar, simplemente especializa las implementaciones de los descriptores de acceso de un evento virtual existente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="7db9f-1255">Una declaración de evento de reemplazo debe especificar exactamente los mismos modificadores de accesibilidad, tipo y nombre que el evento invalidado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="7db9f-1256">Una declaración de evento de reemplazo puede incluir el modificador `sealed`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="7db9f-1257">El uso de este modificador evita que una clase derivada Reemplace el evento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="7db9f-1258">Los descriptores de acceso de un evento sellado también están sellados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="7db9f-1259">Es un error en tiempo de compilación que una declaración de evento de reemplazo incluya un modificador `new`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="7db9f-1260">A excepción de las diferencias en la sintaxis de declaración e invocación, los descriptores de acceso virtual, sellado, invalidación y Abstract se comportan exactamente igual que los métodos virtuales, sellados, de invalidación y abstractos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="7db9f-1261">En concreto, las reglas descritas en [métodos virtuales](classes.md#virtual-methods), [métodos de invalidación](classes.md#override-methods), [métodos sellados](classes.md#sealed-methods)y [métodos abstractos](classes.md#abstract-methods) se aplican como si los descriptores de acceso fueran métodos de un formulario correspondiente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="7db9f-1262">Cada descriptor de acceso corresponde a un método con un parámetro de valor único del tipo de evento, un tipo de valor devuelto `void` y los mismos modificadores que el evento que lo contiene.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="7db9f-1263">Indizadores</span><span class="sxs-lookup"><span data-stu-id="7db9f-1263">Indexers</span></span>

<span data-ttu-id="7db9f-1264">Un ***indexador*** es un miembro que permite indizar un objeto de la misma manera que una matriz.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="7db9f-1265">Los indizadores se declaran mediante *indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="7db9f-1266">Un *indexer_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)), el `new` ([modificador nuevo](classes.md#the-new-modifier)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` ([métodos de invalidación](classes.md#override-methods)), `sealed` ([métodos sellados](classes.md#sealed-methods)), `abstract` (métodos[abstractos](classes.md#abstract-methods)) y modificadores `extern` ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="7db9f-1267">Las declaraciones de indexador están sujetas a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores, con la única excepción de que el modificador static no se permite en una declaración de indexador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="7db9f-1268">Los modificadores `virtual`, `override`y `abstract` se excluyen mutuamente, excepto en un caso.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="7db9f-1269">Los modificadores `abstract` y `override` se pueden usar juntos para que un indizador abstracto pueda reemplazar uno virtual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="7db9f-1270">El *tipo* de una declaración de indexador especifica el tipo de elemento del indizador introducido por la declaración.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="7db9f-1271">A menos que el indizador sea una implementación explícita de un miembro de interfaz, el *tipo* va seguido de la palabra clave `this`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="7db9f-1272">Para una implementación explícita de un miembro de interfaz, el *tipo* va seguido de un *interface_type*, un "`.`" y la palabra clave `this`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="7db9f-1273">A diferencia de otros miembros, los indizadores no tienen nombres definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="7db9f-1274">En el *formal_parameter_list* se especifican los parámetros del indexador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="7db9f-1275">La lista de parámetros formales de un indizador corresponde a la de un método ([parámetros de método](classes.md#method-parameters)), salvo que se debe especificar al menos un parámetro y que no se permiten los modificadores de parámetro `ref` y `out`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="7db9f-1276">El *tipo* de un indexador y cada uno de los tipos a los que se hace referencia en el *formal_parameter_list* deben ser al menos tan accesibles como el propio indizador ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="7db9f-1277">Un *indexer_body* puede consistir en un ***cuerpo de descriptor de acceso*** o en un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="7db9f-1278">En el cuerpo de un descriptor de acceso, *accessor_declarations*, que se deben incluir en los tokens "`{`" y "`}`", declare los descriptores de acceso ([descriptores de acceso](classes.md#accessors)) de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="7db9f-1279">Los descriptores de acceso especifican las instrucciones ejecutables asociadas a la lectura y la escritura de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="7db9f-1280">Un cuerpo de expresión que consiste en "`=>`" seguido de una expresión `E` y un punto y coma es exactamente equivalente al cuerpo de la instrucción `{ get { return E; } }`y, por tanto, solo se puede usar para especificar indizadores de solo captador en los que una sola expresión proporciona el resultado del captador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="7db9f-1281">Aunque la sintaxis para tener acceso a un elemento de indexador es la misma que para un elemento de matriz, un elemento de indexador no se clasifica como una variable.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="7db9f-1282">Por lo tanto, no es posible pasar un elemento indexador como `ref` o `out` argumento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="7db9f-1283">La lista de parámetros formales de un indexador define la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) del indexador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="7db9f-1284">En concreto, la firma de un indexador consta del número y los tipos de sus parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="7db9f-1285">El tipo de elemento y los nombres de los parámetros formales no forman parte de la firma de un indexador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="7db9f-1286">La firma de un indizador debe ser diferente de las firmas de todos los demás indexadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="7db9f-1287">Los indexadores y las propiedades son muy similares en concepto, pero difieren de las siguientes maneras:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="7db9f-1288">Una propiedad se identifica por su nombre, mientras que un indexador se identifica mediante su firma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="7db9f-1289">Se tiene acceso a una propiedad a través de un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)), mientras que se tiene acceso a un elemento de indexador a través de un *element_access* ([acceso a indexador](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="7db9f-1290">Una propiedad puede ser un miembro de `static`, mientras que un indexador siempre es un miembro de instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="7db9f-1291">Un descriptor de acceso de `get` de una propiedad corresponde a un método sin parámetros, mientras que un descriptor de acceso de `get` de un indizador corresponde a un método con la misma lista de parámetros formales que el indexador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="7db9f-1292">Un descriptor de acceso de `set` de una propiedad corresponde a un método con un parámetro único denominado `value`, mientras que un descriptor de acceso de `set` de un indizador corresponde a un método con la misma lista de parámetros formales que el indexador, más un parámetro adicional denominado `value`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="7db9f-1293">Es un error en tiempo de compilación que un descriptor de acceso de indexador declare una variable local con el mismo nombre que un parámetro de indizador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="7db9f-1294">En una declaración de propiedad de reemplazo, se obtiene acceso a la propiedad heredada mediante la sintaxis `base.P`, donde `P` es el nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="7db9f-1295">En una declaración de indizador de reemplazo, se tiene acceso al indizador heredado mediante la sintaxis `base[E]`, donde `E` es una lista de expresiones separadas por comas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="7db9f-1296">No hay ningún concepto de "indexador implementado automáticamente".</span><span class="sxs-lookup"><span data-stu-id="7db9f-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="7db9f-1297">Es un error tener un indexador no externo no abstracto con descriptores de acceso de punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="7db9f-1298">Aparte de estas diferencias, todas las reglas definidas en los [descriptores de acceso](classes.md#accessors) y [las propiedades implementadas automáticamente](classes.md#automatically-implemented-properties) se aplican a los descriptores de acceso del indizador y a los descriptores de acceso de propiedades.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="7db9f-1299">Cuando una declaración de indexador incluye un modificador `extern`, se dice que se trata de un ***indizador externo***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="7db9f-1300">Dado que una declaración de indexador externa no proporciona ninguna implementación real, cada una de sus *accessor_declarations* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="7db9f-1301">En el ejemplo siguiente se declara una clase `BitArray` que implementa un indizador para tener acceso a los bits individuales de la matriz de bits.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="7db9f-1302">Una instancia de la clase `BitArray` consume bastante menos memoria que una `bool[]` correspondiente (puesto que cada valor de la antigua solo ocupa un bit en lugar de un byte), pero permite las mismas operaciones que una `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="7db9f-1303">En la siguiente clase de `CountPrimes` se usa un `BitArray` y el algoritmo clásico "tamiz" para calcular el número de primas entre 1 y un máximo determinado:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="7db9f-1304">Tenga en cuenta que la sintaxis para tener acceso a los elementos de la `BitArray` es precisamente la misma que para un `bool[]`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="7db9f-1305">En el ejemplo siguiente se muestra una clase de cuadrícula de 26 \* 10 que tiene un indizador con dos parámetros.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="7db9f-1306">El primer parámetro debe ser una letra mayúscula o minúscula en el intervalo A-Z, y el segundo debe ser un entero en el intervalo 0-9.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="7db9f-1307">Sobrecarga del indexador</span><span class="sxs-lookup"><span data-stu-id="7db9f-1307">Indexer overloading</span></span>

<span data-ttu-id="7db9f-1308">Las reglas de resolución de sobrecarga de indexador se describen en [inferencia de tipos](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="7db9f-1309">Operadores</span><span class="sxs-lookup"><span data-stu-id="7db9f-1309">Operators</span></span>

<span data-ttu-id="7db9f-1310">Un ***operador*** es un miembro que define el significado de un operador de expresión que se puede aplicar a las instancias de la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="7db9f-1311">Los operadores se declaran mediante *operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1311">Operators are declared using *operator_declaration*s:</span></span>

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

<span data-ttu-id="7db9f-1312">Hay tres categorías de Operadores sobrecargables: operadores unarios ([operadores unarios](classes.md#unary-operators)), operadores binarios ([operadores binarios](classes.md#binary-operators)) y operadores de conversión ([operadores de conversión](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="7db9f-1313">El *operator_body* es un punto y coma, un ***cuerpo de instrucción*** o un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="7db9f-1314">Un cuerpo de instrucción consta de un *bloque*, que especifica las instrucciones que se ejecutarán cuando se invoque el operador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="7db9f-1315">El *bloque* debe cumplir las reglas de los métodos que devuelven valores descritos en el [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="7db9f-1316">Un cuerpo de expresión consta de `=>` seguido de una expresión y un punto y coma, y denota una expresión única que se debe realizar cuando se invoca el operador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="7db9f-1317">Para los operadores de `extern`, el *operator_body* consta simplemente de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="7db9f-1318">En el caso de todos los demás operadores, el *operator_body* es un cuerpo de bloque o un cuerpo de expresión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="7db9f-1319">Las siguientes reglas se aplican a todas las declaraciones de operador:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="7db9f-1320">Una declaración de operador debe incluir un `public` y un modificador `static`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="7db9f-1321">Los parámetros de un operador deben ser parámetros de valor (parámetros de[valor](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="7db9f-1322">Es un error en tiempo de compilación que una declaración de operador especifique `ref` o `out` parámetros.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="7db9f-1323">La firma de un operador ([operadores unarios](classes.md#unary-operators), [operadores binarios](classes.md#binary-operators)y [operadores de conversión](classes.md#conversion-operators)) debe ser diferente de las firmas de todos los demás operadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="7db9f-1324">Todos los tipos a los que se hace referencia en una declaración de operador deben ser al menos tan accesibles como el propio operador ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="7db9f-1325">Es un error que el mismo modificador aparezca varias veces en una declaración de operador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="7db9f-1326">Cada categoría de operador impone restricciones adicionales, tal y como se describe en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="7db9f-1327">Al igual que otros miembros, las clases derivadas heredan los operadores declarados en una clase base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="7db9f-1328">Dado que las declaraciones de operador siempre requieren la clase o estructura en la que se declara que el operador participa en la firma del operador, no es posible que un operador declarado en una clase derivada oculte un operador declarado en una clase base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="7db9f-1329">Por lo tanto, el modificador `new` nunca es necesario y, por lo tanto, nunca se permite en una declaración de operador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="7db9f-1330">Puede encontrar información adicional sobre operadores unarios y binarios en [operadores](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="7db9f-1331">Puede encontrar información adicional sobre los operadores de conversión en [conversiones definidas por el usuario](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="7db9f-1332">Operadores unarios</span><span class="sxs-lookup"><span data-stu-id="7db9f-1332">Unary operators</span></span>

<span data-ttu-id="7db9f-1333">Las siguientes reglas se aplican a las declaraciones de operadores unarios, donde `T` denota el tipo de instancia de la clase o el struct que contiene la declaración del operador:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="7db9f-1334">Un operador unario `+`, `-`, `!`o `~` debe tomar un parámetro único de tipo `T` o `T?` y puede devolver cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="7db9f-1335">Un operador unario `++` o `--` debe tomar un parámetro único de tipo `T` o `T?` y debe devolver el mismo tipo o un tipo derivado de él.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="7db9f-1336">Un operador unario `true` o `false` debe tomar un parámetro único de tipo `T` o `T?` y debe devolver el tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="7db9f-1337">La firma de un operador unario consta del símbolo (token) del operador (`+`, `-`, `!`, `~`, `++`, `--`, `true`o `false`) y el tipo del único parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="7db9f-1338">El tipo de valor devuelto no forma parte de la firma de un operador unario, ni tampoco el nombre del parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="7db9f-1339">Los operadores unarios `true` y `false` requieren una declaración de pares.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="7db9f-1340">Se produce un error en tiempo de compilación si una clase declara uno de estos operadores sin declarar también el otro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="7db9f-1341">Los operadores `true` y `false` se describen con más detalle en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators) y en [Expresiones booleanas](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="7db9f-1342">En el ejemplo siguiente se muestra una implementación y el uso posterior de `operator ++` para una clase de vector entero:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="7db9f-1343">Observe cómo el método de operador devuelve el valor generado agregando 1 al operando, al igual que los operadores de incremento y decremento de postfijo[(operadores de incremento y](expressions.md#postfix-increment-and-decrement-operators)decremento postfijo) y los operadores de incremento y decremento de prefijo (operadores de incremento y decremento de[prefijo](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="7db9f-1344">A diferencia de C++en, este método no necesita modificar directamente el valor de su operando.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="7db9f-1345">De hecho, la modificación del valor de operando infringiría la semántica estándar del operador de incremento de postfijo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="7db9f-1346">Operadores binarios</span><span class="sxs-lookup"><span data-stu-id="7db9f-1346">Binary operators</span></span>

<span data-ttu-id="7db9f-1347">Las siguientes reglas se aplican a las declaraciones de operadores binarios, donde `T` denota el tipo de instancia de la clase o el struct que contiene la declaración de operador:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="7db9f-1348">Un operador binario sin desplazamiento debe tomar dos parámetros, al menos uno de los cuales debe tener el tipo `T` o `T?`y puede devolver cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="7db9f-1349">Un operador binario `<<` o `>>` debe tomar dos parámetros, el primero debe tener el tipo `T` o `T?` y el segundo debe tener el tipo `int` o `int?`y puede devolver cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="7db9f-1350">La firma de un operador binario consta del símbolo (token) del operador (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`o `<=`) y los tipos de los dos parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="7db9f-1351">El tipo de valor devuelto y los nombres de los parámetros formales no forman parte de la firma de un operador binario.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="7db9f-1352">Ciertos operadores binarios requieren la declaración de pares.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="7db9f-1353">Para cada declaración de cualquiera de los operadores de un par, debe haber una declaración coincidente del otro operador del par.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="7db9f-1354">Dos declaraciones de operador coinciden cuando tienen el mismo tipo de valor devuelto y el mismo tipo para cada parámetro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="7db9f-1355">Los operadores siguientes requieren una declaración de pares:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="7db9f-1356">`operator ==` y `operator !=`</span><span class="sxs-lookup"><span data-stu-id="7db9f-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="7db9f-1357">`operator >` y `operator <`</span><span class="sxs-lookup"><span data-stu-id="7db9f-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="7db9f-1358">`operator >=` y `operator <=`</span><span class="sxs-lookup"><span data-stu-id="7db9f-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="7db9f-1359">Operadores de conversión</span><span class="sxs-lookup"><span data-stu-id="7db9f-1359">Conversion operators</span></span>

<span data-ttu-id="7db9f-1360">Una declaración de operador de conversión introduce una ***conversión definida por el usuario*** ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)) que aumenta las conversiones implícitas y explícitas predefinidas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="7db9f-1361">Una declaración de operador de conversión que incluye la palabra clave `implicit` presenta una conversión implícita definida por el usuario.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="7db9f-1362">Las conversiones implícitas pueden producirse en diversas situaciones, incluidas las invocaciones de miembros de función, las expresiones de conversión y las asignaciones.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="7db9f-1363">Esto se describe con más detalle en [conversiones implícitas](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="7db9f-1364">Una declaración de operador de conversión que incluye la palabra clave `explicit` presenta una conversión explícita definida por el usuario.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="7db9f-1365">Las conversiones explícitas pueden producirse en expresiones de conversión y se describen con más detalle en [conversiones explícitas](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="7db9f-1366">Un operador de conversión convierte de un tipo de origen, indicado por el tipo de parámetro del operador de conversión, en un tipo de destino, indicado por el tipo de valor devuelto del operador de conversión.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="7db9f-1367">Para un tipo de origen determinado `S` y el tipo de destino `T`, si `S` o `T` son tipos que aceptan valores NULL, permita que `S0` y `T0` hagan referencia a sus tipos subyacentes; de lo contrario, `S0` y `T0` son iguales a `S` y `T` respectivamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="7db9f-1368">Una clase o struct puede declarar una conversión de un tipo de origen `S` a un tipo de destino `T` solo si se cumplen todas las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="7db9f-1369">`S0` y `T0` son tipos diferentes.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="7db9f-1370">`S0` o `T0` es el tipo de clase o estructura en el que tiene lugar la declaración del operador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="7db9f-1371">Ni `S0` ni `T0` es una *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="7db9f-1372">Sin incluir las conversiones definidas por el usuario, no existe una conversión de `S` a `T` o de `T` a `S`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="7db9f-1373">En el caso de estas reglas, cualquier parámetro de tipo asociado a `S` o `T` se considera que son tipos únicos que no tienen ninguna relación de herencia con otros tipos y se omiten las restricciones en esos parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="7db9f-1374">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="7db9f-1375">las dos primeras declaraciones de operador están permitidas porque, con fines de [indexadores](classes.md#indexers). 3, `T` y `int` y `string` respectivamente se consideran tipos únicos sin relación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="7db9f-1376">Sin embargo, el tercer operador es un error porque `C<T>` es la clase base de `D<T>`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="7db9f-1377">En la segunda regla, se sigue que un operador de conversión debe convertir a o desde el tipo de clase o estructura en el que se declara el operador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="7db9f-1378">Por ejemplo, es posible que un tipo de clase o struct `C` defina una conversión de `C` a `int` y de `int` a `C`, pero no de `int` a `bool`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="7db9f-1379">No es posible volver a definir directamente una conversión predefinida.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="7db9f-1380">Por lo tanto, no se permite a los operadores de conversión convertir de o a `object` porque ya existen conversiones implícitas y explícitas entre `object` y todos los demás tipos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="7db9f-1381">Del mismo modo, ni los tipos de origen ni de destino de una conversión pueden ser un tipo base del otro, ya que una conversión ya existirá.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="7db9f-1382">Sin embargo, es posible declarar operadores en tipos genéricos que, para argumentos de tipo concretos, especifiquen conversiones que ya existan como conversiones predefinidas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="7db9f-1383">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="7db9f-1384">Cuando el tipo `object` se especifica como un argumento de tipo para `T`, el segundo operador declara una conversión que ya existe (una conversión implícita y, por tanto, también existe una conversión explícita de cualquier tipo al tipo `object`).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="7db9f-1385">En los casos en los que existe una conversión predefinida entre dos tipos, se omiten las conversiones definidas por el usuario entre esos tipos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="7db9f-1386">De manera específica:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1386">Specifically:</span></span>

*  <span data-ttu-id="7db9f-1387">Si existe una conversión implícita predefinida ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo `S` al tipo `T`, se omiten todas las conversiones definidas por el usuario (implícitas o explícitas) de `S` a `T`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="7db9f-1388">Si existe una conversión explícita predefinida ([conversiones explícitas](conversions.md#explicit-conversions)) del tipo `S` al tipo `T`, se omiten las conversiones explícitas definidas por el usuario de `S` a `T`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="7db9f-1389">Asimismo,</span><span class="sxs-lookup"><span data-stu-id="7db9f-1389">Furthermore:</span></span>

<span data-ttu-id="7db9f-1390">Si `T` es un tipo de interfaz, se omiten las conversiones implícitas definidas por el usuario de `S` a `T`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="7db9f-1391">De lo contrario, las conversiones implícitas definidas por el usuario de `S` a `T` se siguen considerando.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="7db9f-1392">En todos los tipos pero `object`, los operadores declarados por el tipo de `Convertible<T>` anterior no entran en conflicto con las conversiones predefinidas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="7db9f-1393">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="7db9f-1394">Sin embargo, para el tipo `object`, las conversiones predefinidas ocultan las conversiones definidas por el usuario en todos los casos, pero una:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="7db9f-1395">No se permiten conversiones definidas por el usuario para convertir de o a *interface_type*s.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="7db9f-1396">En concreto, esta restricción garantiza que no se produzcan transformaciones definidas por el usuario al realizar la conversión a un *interface_type*y que una conversión a una *interface_type* se realice correctamente solo si el objeto que se está convirtiendo implementa realmente el *interface_type*especificado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="7db9f-1397">La firma de un operador de conversión está formada por el tipo de origen y el tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="7db9f-1398">(Tenga en cuenta que esta es la única forma de miembro para la que participa el tipo de valor devuelto en la firma.) La clasificación `implicit` o `explicit` de un operador de conversión no forma parte de la firma del operador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="7db9f-1399">Por lo tanto, una clase o struct no puede declarar un `implicit` y un operador de conversión `explicit` con los mismos tipos de origen y de destino.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="7db9f-1400">En general, las conversiones implícitas definidas por el usuario deben diseñarse para que nunca se produzcan excepciones y nunca se pierda información.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="7db9f-1401">Si una conversión definida por el usuario puede dar lugar a excepciones (por ejemplo, porque el argumento de origen está fuera del intervalo) o la pérdida de información (como descartar los bits de orden superior), esa conversión debe definirse como una conversión explícita.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="7db9f-1402">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1402">In the example</span></span>
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
<span data-ttu-id="7db9f-1403">la conversión de `Digit` a `byte` es implícita porque nunca produce excepciones o pierde información, pero la conversión de `byte` a `Digit` es explícita, ya que `Digit` solo puede representar un subconjunto de los valores posibles de una `byte`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="7db9f-1404">Constructores de instancias</span><span class="sxs-lookup"><span data-stu-id="7db9f-1404">Instance constructors</span></span>

<span data-ttu-id="7db9f-1405">Un ***constructor de instancia*** es un miembro que implementa las acciones necesarias para inicializar una instancia de una clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="7db9f-1406">Los constructores de instancias se declaran mediante *constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="7db9f-1407">Un *constructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)) y un modificador `extern` ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="7db9f-1408">No se permite que una declaración de constructor incluya el mismo modificador varias veces.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="7db9f-1409">El *identificador* de un *constructor_declarator* debe nombrar la clase en la que se declara el constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="7db9f-1410">Si se especifica cualquier otro nombre, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="7db9f-1411">El *formal_parameter_list* opcional de un constructor de instancia está sujeto a las mismas reglas que la *formal_parameter_list* de un método ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="7db9f-1412">La lista de parámetros formales define la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un constructor de instancia y rige el proceso por el cual la resolución de sobrecarga ([inferencia de tipos](expressions.md#type-inference)) selecciona un constructor de instancia determinado en una invocación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="7db9f-1413">Cada uno de los tipos a los que se hace referencia en el *formal_parameter_list* de un constructor de instancia debe ser al menos igual de accesible que el propio constructor ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="7db9f-1414">El *constructor_initializer* opcional especifica otro constructor de instancia que se va a invocar antes de ejecutar las instrucciones proporcionadas en el *constructor_body* de este constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="7db9f-1415">Esto se describe con más detalle en [inicializadores de constructor](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="7db9f-1416">Cuando una declaración de constructor incluye un modificador `extern`, se dice que el constructor es un ***constructor externo***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="7db9f-1417">Dado que una declaración de constructor externo no proporciona ninguna implementación real, su *constructor_body* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="7db9f-1418">En el caso de todos los demás constructores, el *constructor_body* se compone de un *bloque* que especifica las instrucciones para inicializar una nueva instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="7db9f-1419">Esto corresponde exactamente al *bloque* de un método de instancia con un tipo de valor devuelto `void` ([cuerpo del método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="7db9f-1420">No se heredan los constructores de instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="7db9f-1421">Por lo tanto, una clase no tiene ningún constructor de instancia que no sea el declarado realmente en la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="7db9f-1422">Si una clase no contiene ninguna declaración de constructor de instancia, se proporciona automáticamente un constructor de instancia predeterminado ([constructores predeterminados](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="7db9f-1423">Los constructores de instancias se invocan mediante *object_creation_expression*s ([expresiones de creación de objetos](expressions.md#object-creation-expressions)) y a través de *constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="7db9f-1424">Inicializadores del constructor</span><span class="sxs-lookup"><span data-stu-id="7db9f-1424">Constructor initializers</span></span>

<span data-ttu-id="7db9f-1425">Todos los constructores de instancia (excepto los de la clase `object`) incluyen implícitamente una invocación de otro constructor de instancia inmediatamente antes de la *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="7db9f-1426">El constructor para invocar implícitamente está determinado por el *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="7db9f-1427">Un inicializador de constructor de instancia con el formato `base(argument_list)` o `base()` hace que se invoque un constructor de instancia de la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="7db9f-1428">Ese constructor se selecciona utilizando *argument_list* , si está presente y las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="7db9f-1429">El conjunto de constructores de instancias candidatas consta de todos los constructores de instancia accesibles contenidos en la clase base directa, o el constructor predeterminado ([constructores predeterminados](classes.md#default-constructors)), si no se declara ningún constructor de instancia en la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="7db9f-1430">Si este conjunto está vacío, o si no se puede identificar un único constructor de instancia mejor, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="7db9f-1431">Un inicializador de constructor de instancia con el formato `this(argument-list)` o `this()` hace que se invoque un constructor de instancia de la propia clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="7db9f-1432">El constructor se selecciona utilizando *argument_list* , si está presente y las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="7db9f-1433">El conjunto de constructores de instancias candidatas se compone de todos los constructores de instancia accesibles declarados en la propia clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="7db9f-1434">Si este conjunto está vacío, o si no se puede identificar un único constructor de instancia mejor, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="7db9f-1435">Si una declaración de constructor de instancia incluye un inicializador de constructor que invoca al propio constructor, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="7db9f-1436">Si un constructor de instancia no tiene ningún inicializador de constructor, se proporciona implícitamente un inicializador de constructor con el formato `base()`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="7db9f-1437">Por lo tanto, una declaración de constructor de instancia con el formato</span><span class="sxs-lookup"><span data-stu-id="7db9f-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="7db9f-1438">es exactamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="7db9f-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="7db9f-1439">El ámbito de los parámetros proporcionados por la *formal_parameter_list* de una declaración de constructor de instancia incluye el inicializador de constructor de esa declaración.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="7db9f-1440">Por lo tanto, se permite a un inicializador de constructor tener acceso a los parámetros del constructor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="7db9f-1441">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1441">For example:</span></span>
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

<span data-ttu-id="7db9f-1442">Un inicializador de constructor de instancia no puede tener acceso a la instancia que se está creando.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="7db9f-1443">Por lo tanto, se trata de un error en tiempo de compilación para hacer referencia a `this` en una expresión de argumento del inicializador de constructor, como es un error en tiempo de compilación para que una expresión de argumento haga referencia a un miembro de instancia a través de un *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="7db9f-1444">Inicializadores de variables de instancia</span><span class="sxs-lookup"><span data-stu-id="7db9f-1444">Instance variable initializers</span></span>

<span data-ttu-id="7db9f-1445">Cuando un constructor de instancia no tiene ningún inicializador de constructor o tiene un inicializador de constructor con el formato `base(...)`, ese constructor realiza implícitamente las inicializaciones especificadas por los *variable_initializer*s de los campos de instancia declarados en su clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="7db9f-1446">Esto corresponde a una secuencia de asignaciones que se ejecutan inmediatamente después de la entrada al constructor y antes de la invocación implícita del constructor de clase base directo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="7db9f-1447">Los inicializadores de variable se ejecutan en el orden textual en el que aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="7db9f-1448">Ejecución del constructor</span><span class="sxs-lookup"><span data-stu-id="7db9f-1448">Constructor execution</span></span>

<span data-ttu-id="7db9f-1449">Los inicializadores de variables se transforman en instrucciones de asignación, y estas instrucciones de asignación se ejecutan antes de la invocación del constructor de instancia de clase base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="7db9f-1450">Este orden garantiza que todos los campos de instancia se inicializan mediante sus inicializadores de variable antes de que se ejecute cualquier instrucción que tenga acceso a esa instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="7db9f-1451">Dado el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1451">Given the example</span></span>
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
<span data-ttu-id="7db9f-1452">Cuando se usa `new B()` para crear una instancia de `B`, se genera el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```console
x = 1, y = 0
```

<span data-ttu-id="7db9f-1453">El valor de `x` es 1 porque el inicializador de variable se ejecuta antes de que se invoque el constructor de instancia de clase base.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="7db9f-1454">Sin embargo, el valor de `y` es 0 (el valor predeterminado de un `int`) porque la asignación a `y` no se ejecuta hasta que el constructor de clase base devuelve.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="7db9f-1455">Resulta útil pensar en los inicializadores de variables de instancia y en los inicializadores de constructor como instrucciones que se insertan automáticamente antes de la *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="7db9f-1456">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1456">The example</span></span>
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
<span data-ttu-id="7db9f-1457">contiene varios inicializadores variables; también contiene inicializadores de constructor de ambos formularios (`base` y `this`).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="7db9f-1458">El ejemplo corresponde al código que se muestra a continuación, donde cada comentario indica una instrucción insertada automáticamente (la sintaxis utilizada para las invocaciones de constructor insertadas automáticamente no es válida, pero simplemente sirve para ilustrar el mecanismo).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="7db9f-1459">Constructores predeterminados</span><span class="sxs-lookup"><span data-stu-id="7db9f-1459">Default constructors</span></span>

<span data-ttu-id="7db9f-1460">Si una clase no contiene ninguna declaración de constructor de instancia, se proporciona automáticamente un constructor de instancia predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="7db9f-1461">Ese constructor predeterminado simplemente invoca el constructor sin parámetros de la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="7db9f-1462">Si la clase es abstracta, se protege la accesibilidad declarada para el constructor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="7db9f-1463">De lo contrario, la accesibilidad declarada para el constructor predeterminado es Public.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="7db9f-1464">Por lo tanto, el constructor predeterminado siempre tiene el formato</span><span class="sxs-lookup"><span data-stu-id="7db9f-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="7db9f-1465">o</span><span class="sxs-lookup"><span data-stu-id="7db9f-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="7db9f-1466">donde `C` es el nombre de la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="7db9f-1467">Si la resolución de sobrecarga no puede determinar un mejor candidato único para el inicializador de constructor de clase base, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="7db9f-1468">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="7db9f-1469">se proporciona un constructor predeterminado porque la clase no contiene ninguna declaración de constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="7db9f-1470">Por lo tanto, el ejemplo es exactamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="7db9f-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="7db9f-1471">Constructores privados</span><span class="sxs-lookup"><span data-stu-id="7db9f-1471">Private constructors</span></span>

<span data-ttu-id="7db9f-1472">Cuando una clase `T` declara solo constructores de instancia privados, no es posible que las clases fuera del texto del programa de `T` deriven de `T` o para crear directamente instancias de `T`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="7db9f-1473">Por lo tanto, si una clase solo contiene miembros estáticos y no está pensado para que se creen instancias, agregar un constructor de instancia privado vacío impedirá la creación de instancias.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="7db9f-1474">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1474">For example:</span></span>
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

<span data-ttu-id="7db9f-1475">La clase `Trig` agrupa los métodos relacionados y las constantes, pero no está diseñado para crear instancias.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="7db9f-1476">Por lo tanto, declara un único constructor de instancia privado vacío.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="7db9f-1477">Se debe declarar al menos un constructor de instancia para suprimir la generación automática de un constructor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="7db9f-1478">Parámetros de constructor de instancia opcionales</span><span class="sxs-lookup"><span data-stu-id="7db9f-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="7db9f-1479">La forma `this(...)` del inicializador de constructor se utiliza normalmente junto con la sobrecarga para implementar parámetros de constructor de instancia opcionales.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="7db9f-1480">en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1480">In the example</span></span>
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
<span data-ttu-id="7db9f-1481">los dos primeros constructores de instancia simplemente proporcionan los valores predeterminados para los argumentos que faltan.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="7db9f-1482">Ambos utilizan un inicializador de constructor `this(...)` para invocar el tercer constructor de instancia, que realmente realiza el trabajo de inicializar la nueva instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="7db9f-1483">El efecto es el de los parámetros de constructor opcionales:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="7db9f-1484">Constructores estáticos</span><span class="sxs-lookup"><span data-stu-id="7db9f-1484">Static constructors</span></span>

<span data-ttu-id="7db9f-1485">Un ***constructor estático*** es un miembro que implementa las acciones necesarias para inicializar un tipo de clase cerrada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="7db9f-1486">Los constructores estáticos se declaran mediante *static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="7db9f-1487">Un *static_constructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y un modificador `extern` ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="7db9f-1488">El *identificador* de un *static_constructor_declaration* debe nombrar la clase en la que se declara el constructor estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="7db9f-1489">Si se especifica cualquier otro nombre, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="7db9f-1490">Cuando una declaración de constructor estático incluye un modificador `extern`, se dice que el constructor estático es un ***constructor estático externo***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="7db9f-1491">Dado que una declaración de constructor estático externo no proporciona ninguna implementación real, su *static_constructor_body* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="7db9f-1492">Para todas las demás declaraciones de constructores estáticos, la *static_constructor_body* está formada por un *bloque* que especifica las instrucciones que se ejecutan para inicializar la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="7db9f-1493">Se corresponde exactamente con la *method_body* de un método estático con un tipo de valor devuelto `void` ([cuerpo del método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="7db9f-1494">Los constructores estáticos no se heredan y no se pueden llamar directamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="7db9f-1495">El constructor estático de un tipo de clase cerrada se ejecuta como máximo una vez en un dominio de aplicación determinado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="7db9f-1496">La ejecución de un constructor estático lo desencadena el primero de los siguientes eventos para que se produzca dentro de un dominio de aplicación:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="7db9f-1497">Se crea una instancia del tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="7db9f-1498">Se hace referencia a cualquiera de los miembros estáticos del tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="7db9f-1499">Si una clase contiene el método `Main` ([Inicio](basic-concepts.md#application-startup)de la aplicación) en el que comienza la ejecución, el constructor estático de esa clase se ejecuta antes de que se llame al método `Main`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="7db9f-1500">Para inicializar un nuevo tipo de clase cerrada, primero se crea un nuevo conjunto de campos estáticos ([campos estáticos y de instancia](classes.md#static-and-instance-fields)) para ese tipo cerrado en particular.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="7db9f-1501">Cada uno de los campos estáticos se inicializa en su valor predeterminado ([valores predeterminados](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="7db9f-1502">A continuación, se ejecutan los inicializadores de campo estáticos ([inicialización de campos estáticos](classes.md#static-field-initialization)) para esos campos estáticos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="7db9f-1503">Por último, se ejecuta el constructor estático.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="7db9f-1504">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1504">The example</span></span>
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
<span data-ttu-id="7db9f-1505">debe generar la salida:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1505">must produce the output:</span></span>
```console
Init A
A.F
Init B
B.F
```
<span data-ttu-id="7db9f-1506">Dado que la llamada a `A.F`desencadena la ejecución del constructor estático de `A`, y la llamada a `B.F`desencadena la ejecución del constructor estático de `B`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="7db9f-1507">Es posible crear dependencias circulares que permitan observar los campos estáticos con inicializadores variables en su estado de valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="7db9f-1508">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1508">The example</span></span>
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
<span data-ttu-id="7db9f-1509">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="7db9f-1509">produces the output</span></span>
```console
X = 1, Y = 2
```

<span data-ttu-id="7db9f-1510">Para ejecutar el método `Main`, el sistema primero ejecuta el inicializador para `B.Y`, antes del constructor estático de la clase `B`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="7db9f-1511">el inicializador de `Y`hace que se ejecute el constructor estático de `A`porque se hace referencia al valor de `A.X`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="7db9f-1512">A su vez, el constructor estático de `A` continúa para calcular el valor de `X`y, al hacerlo, recupera el valor predeterminado de `Y`, que es cero.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="7db9f-1513">por tanto, `A.X` se inicializa en 1.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="7db9f-1514">El proceso de ejecución de los inicializadores de campo estáticos y el constructor estático de `A`se completa y vuelve al cálculo del valor inicial de `Y`, cuyo resultado se convierte en 2.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="7db9f-1515">Dado que el constructor estático se ejecuta exactamente una vez para cada tipo de clase construido cerrado, es un lugar cómodo para aplicar comprobaciones en tiempo de ejecución en el parámetro de tipo que no se puede comprobar en tiempo de compilación a través de restricciones ([restricciones de parámetros de tipo](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="7db9f-1516">Por ejemplo, el tipo siguiente usa un constructor estático para exigir que el argumento de tipo sea una enumeración:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="7db9f-1517">Destructores</span><span class="sxs-lookup"><span data-stu-id="7db9f-1517">Destructors</span></span>

<span data-ttu-id="7db9f-1518">Un ***destructor*** es un miembro que implementa las acciones necesarias para destruir una instancia de una clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="7db9f-1519">Un destructor se declara mediante un *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="7db9f-1520">Un *destructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="7db9f-1521">El *identificador* de un *destructor_declaration* debe nombrar la clase en la que se declara el destructor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="7db9f-1522">Si se especifica cualquier otro nombre, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="7db9f-1523">Cuando una declaración de destructor incluye un modificador `extern`, se dice que se trata de un ***destructor externo***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="7db9f-1524">Dado que una declaración de destructor externo no proporciona ninguna implementación real, su *destructor_body* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="7db9f-1525">En el caso de todos los demás destructores, el *destructor_body* consta de un *bloque* que especifica las instrucciones que se van a ejecutar para destruir una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="7db9f-1526">Un *destructor_body* corresponde exactamente al *method_body* de un método de instancia con un tipo de valor devuelto `void` ([cuerpo del método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="7db9f-1527">Los destructores no se heredan.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1527">Destructors are not inherited.</span></span> <span data-ttu-id="7db9f-1528">Por lo tanto, una clase no tiene ningún destructor que no sea el que se puede declarar en esa clase.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="7db9f-1529">Dado que un destructor debe tener ningún parámetro, no se puede sobrecargar, por lo que una clase puede tener, como máximo, un destructor.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="7db9f-1530">Los destructores se invocan automáticamente y no se pueden invocar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="7db9f-1531">Una instancia pasa a ser válida para su destrucción cuando ya no es posible para cualquier código utilizar esa instancia.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="7db9f-1532">La ejecución del destructor para la instancia puede producirse en cualquier momento después de que la instancia sea válida para la destrucción.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="7db9f-1533">Cuando se destruye una instancia, se llama a los destructores de la cadena de herencia de la instancia, en orden, de la más derivada a la menos derivada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="7db9f-1534">Se puede ejecutar un destructor en cualquier subproceso.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="7db9f-1535">Para obtener más información sobre las reglas que rigen cuándo y cómo se ejecuta un destructor, consulte [Administración automática](basic-concepts.md#automatic-memory-management)de la memoria.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="7db9f-1536">La salida del ejemplo</span><span class="sxs-lookup"><span data-stu-id="7db9f-1536">The output of the example</span></span>
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
<span data-ttu-id="7db9f-1537">estará</span><span class="sxs-lookup"><span data-stu-id="7db9f-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="7db9f-1538">Dado que se llama a los destructores en una cadena de herencia en orden, desde la más derivada hasta la menos derivada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="7db9f-1539">Los destructores se implementan invalidando el método virtual `Finalize` en `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="7db9f-1540">C#no se permite a los programas invalidar este método ni llamarlo (o invalidarlo) directamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="7db9f-1541">Por ejemplo, el programa</span><span class="sxs-lookup"><span data-stu-id="7db9f-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="7db9f-1542">contiene dos errores.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1542">contains two errors.</span></span>

<span data-ttu-id="7db9f-1543">El compilador se comporta como si este método, y los reemplaza, no existen en absoluto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="7db9f-1544">Por lo tanto, este programa:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="7db9f-1545">es válido y el método mostrado oculta el método de `Finalize` de `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="7db9f-1546">Para obtener una explicación del comportamiento cuando se produce una excepción desde un destructor, vea [cómo se controlan las excepciones](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="7db9f-1547">Iteradores</span><span class="sxs-lookup"><span data-stu-id="7db9f-1547">Iterators</span></span>

<span data-ttu-id="7db9f-1548">Un miembro de función ([miembros de función](expressions.md#function-members)) implementado mediante un bloque de iteradores ([bloques](statements.md#blocks)) se denomina ***iterador***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="7db9f-1549">Un bloque de iterador se puede usar como cuerpo de un miembro de función siempre que el tipo de valor devuelto del miembro de función correspondiente sea una de las interfaces de enumerador ([interfaces de enumerador](classes.md#enumerator-interfaces)) o una de las interfaces enumerables ([interfaces enumerables](classes.md#enumerable-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="7db9f-1550">Puede producirse como *method_body*, *operator_body* o *accessor_body*, mientras que los eventos, constructores de instancias, constructores estáticos y destructores no se pueden implementar como iteradores.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="7db9f-1551">Cuando un miembro de función se implementa mediante un bloque de iteradores, se trata de un error en tiempo de compilación para la lista de parámetros formales del miembro de función a fin de especificar los parámetros `ref` o `out`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="7db9f-1552">Interfaces de enumerador</span><span class="sxs-lookup"><span data-stu-id="7db9f-1552">Enumerator interfaces</span></span>

<span data-ttu-id="7db9f-1553">Las ***interfaces de enumerador*** son la interfaz no genérica `System.Collections.IEnumerator` y todas las creaciones de instancias de la interfaz genérica `System.Collections.Generic.IEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="7db9f-1554">Por motivos de brevedad, en este capítulo se hace referencia a estas interfaces como `IEnumerator` y `IEnumerator<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="7db9f-1555">Interfaces enumerables</span><span class="sxs-lookup"><span data-stu-id="7db9f-1555">Enumerable interfaces</span></span>

<span data-ttu-id="7db9f-1556">Las ***interfaces enumerables*** son la interfaz no genérica `System.Collections.IEnumerable` y todas las creaciones de instancias de la interfaz genérica `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="7db9f-1557">Por motivos de brevedad, en este capítulo se hace referencia a estas interfaces como `IEnumerable` y `IEnumerable<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="7db9f-1558">Tipo yield</span><span class="sxs-lookup"><span data-stu-id="7db9f-1558">Yield type</span></span>

<span data-ttu-id="7db9f-1559">Un iterador genera una secuencia de valores, todo el mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="7db9f-1560">Este tipo se denomina ***tipo yield*** del iterador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="7db9f-1561">El tipo yield de un iterador que devuelve `IEnumerator` o `IEnumerable` es `object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="7db9f-1562">El tipo yield de un iterador que devuelve `IEnumerator<T>` o `IEnumerable<T>` es `T`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="7db9f-1563">Objetos Enumerator</span><span class="sxs-lookup"><span data-stu-id="7db9f-1563">Enumerator objects</span></span>

<span data-ttu-id="7db9f-1564">Cuando un miembro de función que devuelve un tipo de interfaz de enumerador se implementa mediante un bloque de iteradores, al invocar al miembro de función no se ejecuta inmediatamente el código en el bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="7db9f-1565">En su lugar, se crea y se devuelve un ***objeto de enumerador*** .</span><span class="sxs-lookup"><span data-stu-id="7db9f-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="7db9f-1566">Este objeto encapsula el código especificado en el bloque de iteradores y la ejecución del código en el bloque de iterador se produce cuando se invoca el método de `MoveNext` del objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="7db9f-1567">Un objeto de enumerador tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="7db9f-1568">Implementa `IEnumerator` y `IEnumerator<T>`, donde `T` es el tipo yield del iterador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="7db9f-1569">Implementa `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="7db9f-1570">Se inicializa con una copia de los valores de argumento (si existen) y el valor de instancia pasado al miembro de función.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="7db9f-1571">Tiene cuatro Estados posibles, ***antes***, en ***ejecución***, ***suspendido***y ***después***, y está inicialmente en el estado ***antes*** .</span><span class="sxs-lookup"><span data-stu-id="7db9f-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="7db9f-1572">Un objeto de enumerador suele ser una instancia de una clase de enumerador generada por el compilador que encapsula el código en el bloque de iteradores e implementa las interfaces del enumerador, pero otros métodos de implementación son posibles.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="7db9f-1573">Si el compilador genera una clase de enumerador, esa clase se anidará, directa o indirectamente, en la clase que contiene el miembro de función, tendrá accesibilidad privada y tendrá un nombre reservado para uso del compilador ([identificadores](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="7db9f-1574">Un objeto de enumerador puede implementar más interfaces de las especificadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="7db9f-1575">En las secciones siguientes se describe el comportamiento exacto de los miembros `MoveNext`, `Current`y `Dispose` de las implementaciones de interfaz `IEnumerable` y `IEnumerable<T>` proporcionadas por un objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="7db9f-1576">Tenga en cuenta que los objetos de enumerador no admiten el método `IEnumerator.Reset`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="7db9f-1577">Al invocar este método, se produce una `System.NotSupportedException`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="7db9f-1578">El método MoveNext</span><span class="sxs-lookup"><span data-stu-id="7db9f-1578">The MoveNext method</span></span>

<span data-ttu-id="7db9f-1579">El método `MoveNext` de un objeto enumerador encapsula el código de un bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="7db9f-1580">Al invocar el método `MoveNext` se ejecuta código en el bloque de iterador y se establece la propiedad `Current` del objeto de enumerador según corresponda.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="7db9f-1581">La acción precisa realizada por `MoveNext` depende del estado del objeto de enumerador cuando se invoca `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="7db9f-1582">Si el estado del objeto de enumerador es ***anterior***, al invocar `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="7db9f-1583">Cambia el estado a en ***ejecución***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="7db9f-1584">Inicializa los parámetros (incluido `this`) del bloque de iteradores en los valores de argumento y el valor de instancia guardados cuando se inicializó el objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="7db9f-1585">Ejecuta el bloque de iterador desde el principio hasta que se interrumpe la ejecución (como se describe a continuación).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="7db9f-1586">Si el estado del objeto de enumerador se está ***ejecutando***, no se especifica el resultado de invocar `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="7db9f-1587">Si el estado del objeto de enumerador es ***suspendido***, al invocar `MoveNext`:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="7db9f-1588">Cambia el estado a en ***ejecución***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="7db9f-1589">Restaura los valores de todas las variables locales y los parámetros (incluido este) en los valores guardados cuando la ejecución del bloque de iterador se suspendió por última vez.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="7db9f-1590">Tenga en cuenta que el contenido de los objetos a los que hacen referencia estas variables puede haber cambiado desde la llamada anterior a MoveNext.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="7db9f-1591">Reanuda la ejecución del bloque de iterador inmediatamente después de la instrucción `yield return` que causó la suspensión de la ejecución y continúa hasta que se interrumpe la ejecución (como se describe a continuación).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="7db9f-1592">Si el estado del objeto de enumerador es ***posterior***, al invocar `MoveNext` se devuelve `false`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="7db9f-1593">Cuando `MoveNext` ejecuta el bloque de iterador, la ejecución se puede interrumpir de cuatro maneras: mediante una instrucción de `yield return`, mediante una instrucción `yield break`, al encontrar el final del bloque de iterador y una excepción que se produce y se propaga fuera del bloque de iterador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="7db9f-1594">Cuando se encuentra una instrucción `yield return` ([la instrucción yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="7db9f-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="7db9f-1595">La expresión dada en la instrucción se evalúa, se convierte implícitamente al tipo Yield y se asigna a la propiedad `Current` del objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="7db9f-1596">Se suspende la ejecución del cuerpo del iterador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="7db9f-1597">Se guardan los valores de todas las variables locales y los parámetros (incluido `this`), al igual que la ubicación de esta instrucción `yield return`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="7db9f-1598">Si la instrucción `yield return` está en uno o varios bloques de `try`, los bloques de `finally` asociados no se ejecutan en este momento.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="7db9f-1599">El estado del objeto de enumerador se cambia a ***Suspended***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="7db9f-1600">El método `MoveNext` devuelve `true` a su llamador, lo que indica que la iteración se ha avanzado correctamente al valor siguiente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="7db9f-1601">Cuando se encuentra una instrucción `yield break` ([la instrucción yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="7db9f-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="7db9f-1602">Si la instrucción `yield break` está dentro de uno o más bloques `try`, se ejecutan los bloques de `finally` asociados.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="7db9f-1603">El estado del objeto de enumerador se cambia a ***después***de.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="7db9f-1604">El método `MoveNext` devuelve `false` a su llamador, lo que indica que la iteración ha finalizado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="7db9f-1605">Cuando se encuentra el final del cuerpo del iterador:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="7db9f-1606">El estado del objeto de enumerador se cambia a ***después***de.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="7db9f-1607">El método `MoveNext` devuelve `false` a su llamador, lo que indica que la iteración ha finalizado.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="7db9f-1608">Cuando se produce una excepción y se propaga fuera del bloque de iteradores:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="7db9f-1609">La propagación de excepciones habrá ejecutado los bloques de `finally` adecuados en el cuerpo del iterador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="7db9f-1610">El estado del objeto de enumerador se cambia a ***después***de.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="7db9f-1611">La propagación de excepciones continúa hasta el autor de la llamada del método `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="7db9f-1612">Propiedad actual</span><span class="sxs-lookup"><span data-stu-id="7db9f-1612">The Current property</span></span>

<span data-ttu-id="7db9f-1613">La propiedad `Current` de un objeto de enumerador se ve afectada por `yield return` instrucciones del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="7db9f-1614">Cuando un objeto de enumerador se encuentra en estado ***suspendido*** , el valor de `Current` es el valor establecido por la llamada anterior a `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="7db9f-1615">Cuando un objeto de enumerador está en los Estados ***Before***, ***Running***o ***After*** , no se especifica el resultado de obtener acceso a `Current`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="7db9f-1616">En el caso de un iterador con un tipo de Yield distinto de `object`, el resultado de tener acceso a `Current` a través de la implementación de `IEnumerable` del objeto de enumerador corresponde al acceso a `Current` a través de la `IEnumerator<T>` de la implementación del objeto de enumerador y convertir el resultado a `object`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="7db9f-1617">El método Dispose</span><span class="sxs-lookup"><span data-stu-id="7db9f-1617">The Dispose method</span></span>

<span data-ttu-id="7db9f-1618">El método `Dispose` se utiliza para limpiar la iteración, para lo que coloca el objeto enumerador en el estado ***después*** .</span><span class="sxs-lookup"><span data-stu-id="7db9f-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="7db9f-1619">Si el estado del objeto de enumerador es ***anterior***, al invocar `Dispose` cambia el estado a ***después***de.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="7db9f-1620">Si el estado del objeto de enumerador se está ***ejecutando***, no se especifica el resultado de invocar `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="7db9f-1621">Si el estado del objeto de enumerador es ***suspendido***, al invocar `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="7db9f-1622">Cambia el estado a en ***ejecución***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="7db9f-1623">Ejecuta cualquier bloque Finally como si la última instrucción `yield return` ejecuta fuera una instrucción `yield break`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="7db9f-1624">Si esto hace que se produzca una excepción y se propague fuera del cuerpo del iterador, el estado del objeto de enumerador se establece en ***después*** de y la excepción se propaga al llamador del método `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="7db9f-1625">Cambia el estado a ***después***de.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="7db9f-1626">Si el estado del objeto de enumerador es ***posterior***, la invocación de `Dispose` no tiene ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="7db9f-1627">Objetos enumerables</span><span class="sxs-lookup"><span data-stu-id="7db9f-1627">Enumerable objects</span></span>

<span data-ttu-id="7db9f-1628">Cuando un miembro de función que devuelve un tipo de interfaz enumerable se implementa mediante un bloque de iteradores, al invocar al miembro de función no se ejecuta inmediatamente el código en el bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="7db9f-1629">En su lugar, se crea y se devuelve un ***objeto enumerable*** .</span><span class="sxs-lookup"><span data-stu-id="7db9f-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="7db9f-1630">El método `GetEnumerator` del objeto enumerable devuelve un objeto de enumerador que encapsula el código especificado en el bloque de iterador y la ejecución del código en el bloque de iterador se produce cuando se invoca el método de `MoveNext` del objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="7db9f-1631">Un objeto enumerable tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="7db9f-1632">Implementa `IEnumerable` y `IEnumerable<T>`, donde `T` es el tipo yield del iterador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="7db9f-1633">Se inicializa con una copia de los valores de argumento (si existen) y el valor de instancia pasado al miembro de función.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="7db9f-1634">Un objeto enumerable es normalmente una instancia de una clase Enumerable generada por el compilador que encapsula el código en el bloque de iteradores e implementa las interfaces enumerables, pero otros métodos de implementación son posibles.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="7db9f-1635">Si el compilador genera una clase Enumerable, esa clase se anidará, directa o indirectamente, en la clase que contiene el miembro de función, tendrá accesibilidad privada y tendrá un nombre reservado para uso del compilador ([identificadores](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="7db9f-1636">Un objeto enumerable puede implementar más interfaces de las especificadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="7db9f-1637">En concreto, un objeto enumerable también puede implementar `IEnumerator` y `IEnumerator<T>`, lo que permite que sirva como un enumerador y un enumerador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="7db9f-1638">En ese tipo de implementación, la primera vez que se invoca un método `GetEnumerator` del objeto enumerable, se devuelve el propio objeto Enumerable.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="7db9f-1639">Las siguientes invocaciones del `GetEnumerator`del objeto enumerable, si las hay, devuelven una copia del objeto Enumerable.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="7db9f-1640">Por lo tanto, cada enumerador devuelto tiene su propio estado y los cambios en un enumerador no afectarán a otro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="7db9f-1641">El método GetEnumerator</span><span class="sxs-lookup"><span data-stu-id="7db9f-1641">The GetEnumerator method</span></span>

<span data-ttu-id="7db9f-1642">Un objeto enumerable proporciona una implementación de los métodos `GetEnumerator` de las interfaces `IEnumerable` y `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="7db9f-1643">Los dos métodos `GetEnumerator` comparten una implementación común que adquiere y devuelve un objeto de enumerador disponible.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="7db9f-1644">El objeto de enumerador se inicializa con los valores de argumento y el valor de instancia guardados cuando se inicializó el objeto enumerable, pero de lo contrario el objeto de enumerador funciona como se describe en [objetos de enumerador](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="7db9f-1645">Ejemplo de implementación</span><span class="sxs-lookup"><span data-stu-id="7db9f-1645">Implementation example</span></span>

<span data-ttu-id="7db9f-1646">En esta sección se describe una posible implementación de iteradores en términos C# de construcciones estándar.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="7db9f-1647">La implementación que se describe aquí se basa en los mismos principios utilizados por C# el compilador de Microsoft, pero no es una implementación asignada o la única que sea posible.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="7db9f-1648">La siguiente clase de `Stack<T>` implementa su método de `GetEnumerator` mediante un iterador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="7db9f-1649">El iterador enumera los elementos de la pila en orden descendente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="7db9f-1650">El método `GetEnumerator` se puede convertir en una instancia de una clase de enumerador generada por el compilador que encapsula el código en el bloque de iteradores, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="7db9f-1651">En la traducción anterior, el código del bloque de iterador se convierte en una máquina de Estados y se coloca en el método `MoveNext` de la clase de enumerador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="7db9f-1652">Además, la variable local `i` se convierte en un campo en el objeto de enumerador para que pueda seguir existiendo en las invocaciones de `MoveNext`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="7db9f-1653">En el ejemplo siguiente se imprime una tabla de multiplicación simple de los enteros comprendidos entre 1 y 10.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="7db9f-1654">El método `FromTo` del ejemplo devuelve un objeto enumerable y se implementa mediante un iterador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="7db9f-1655">El método `FromTo` se puede convertir en una instancia de una clase Enumerable generada por el compilador que encapsula el código en el bloque de iteradores, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="7db9f-1656">La clase Enumerable implementa las interfaces enumerables y las interfaces de enumerador, lo que permite que sirva como un enumerador y un enumerador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="7db9f-1657">La primera vez que se invoca el método de `GetEnumerator`, se devuelve el propio objeto Enumerable.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="7db9f-1658">Las siguientes invocaciones del `GetEnumerator`del objeto enumerable, si las hay, devuelven una copia del objeto Enumerable.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="7db9f-1659">Por lo tanto, cada enumerador devuelto tiene su propio estado y los cambios en un enumerador no afectarán a otro.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="7db9f-1660">El método `Interlocked.CompareExchange` se usa para garantizar la operación segura para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="7db9f-1661">Los parámetros `from` y `to` se convierten en campos de la clase Enumerable.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="7db9f-1662">Dado que `from` se modifica en el bloque de iterador, se introduce un campo de `__from` adicional para contener el valor inicial dado a `from` en cada enumerador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="7db9f-1663">El método `MoveNext` produce una `InvalidOperationException` si se llama cuando se `0``__state`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="7db9f-1664">Esto protege contra el uso del objeto enumerable como un objeto de enumerador sin llamar primero a `GetEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="7db9f-1665">En el ejemplo siguiente se muestra una clase de árbol simple.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="7db9f-1666">La clase `Tree<T>` implementa su método de `GetEnumerator` mediante un iterador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="7db9f-1667">El iterador enumera los elementos del árbol en orden infijo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="7db9f-1668">El método `GetEnumerator` se puede convertir en una instancia de una clase de enumerador generada por el compilador que encapsula el código en el bloque de iteradores, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="7db9f-1669">El compilador generó objetos temporales utilizado en las instrucciones `foreach` se elevan a los campos `__left` y `__right` del objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="7db9f-1670">El campo `__state` del objeto de enumerador se actualiza cuidadosamente para que el método de `Dispose()` correcto se llame correctamente si se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="7db9f-1671">Tenga en cuenta que no es posible escribir el código traducido con instrucciones de `foreach` simples.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="7db9f-1672">Funciones asincrónicas</span><span class="sxs-lookup"><span data-stu-id="7db9f-1672">Async functions</span></span>

<span data-ttu-id="7db9f-1673">Un método ([métodos](classes.md#methods)) o una función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) con el modificador `async` se denomina ***función asincrónica***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="7db9f-1674">En general, el término ***Async*** se usa para describir cualquier tipo de función que tenga el modificador `async`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="7db9f-1675">Se trata de un error en tiempo de compilación para la lista de parámetros formales de una función asincrónica con el fin de especificar los parámetros `ref` o `out`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="7db9f-1676">El *return_type* de un método asincrónico debe ser `void` o un ***tipo de tarea***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="7db9f-1677">Los tipos de tarea son `System.Threading.Tasks.Task` y los tipos construidos a partir de `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="7db9f-1678">Por motivos de brevedad, en este capítulo se hace referencia a estos tipos como `Task` y `Task<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="7db9f-1679">Se dice que un método asincrónico que devuelve un tipo de tarea es el que devuelve la tarea.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="7db9f-1680">La definición exacta de los tipos de tarea es la implementación definida, pero desde el punto de vista del idioma, un tipo de tarea se encuentra en uno de los Estados incompleto, correcto o erróneo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="7db9f-1681">Una tarea con errores registra una excepción pertinente.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="7db9f-1682">Un `Task<T>` correctamente registra un resultado de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="7db9f-1683">Los tipos de tarea son de espera y, por tanto, pueden ser los operandos de las expresiones Await ([expresiones Await](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="7db9f-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="7db9f-1684">Una invocación de función asincrónica tiene la capacidad de suspender la evaluación por medio de expresiones Await ([expresiones Await](expressions.md#await-expressions)) en su cuerpo.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="7db9f-1685">La evaluación se puede reanudar posteriormente en el punto de la expresión Await de suspensión por medio de un ***delegado de reanudación***.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="7db9f-1686">El delegado de reanudación es de tipo `System.Action`y, cuando se invoca, la evaluación de la invocación de la función asincrónica se reanudará desde la expresión Await en la que se quedó.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="7db9f-1687">El ***llamador actual*** de una invocación de función asincrónica es el autor de la llamada original si la invocación de la función nunca se ha suspendido o el llamador más reciente del delegado de reanudación en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="7db9f-1688">Evaluación de una función asincrónica que devuelve una tarea</span><span class="sxs-lookup"><span data-stu-id="7db9f-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="7db9f-1689">La invocación de una función asincrónica que devuelve una tarea hace que se genere una instancia del tipo de tarea devuelto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="7db9f-1690">Esto se denomina la ***tarea devuelta*** de la función Async.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="7db9f-1691">La tarea está inicialmente en un estado incompleto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="7db9f-1692">A continuación, se evalúa el cuerpo de la función asincrónica hasta que se suspenda (al llegar a una expresión Await) o finalice, donde el control de punto se devuelve al autor de la llamada, junto con la tarea devuelta.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="7db9f-1693">Cuando finaliza el cuerpo de la función asincrónica, la tarea devuelta se saca del estado incompleto:</span><span class="sxs-lookup"><span data-stu-id="7db9f-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="7db9f-1694">Si el cuerpo de la función finaliza como resultado de alcanzar una instrucción return o el final del cuerpo, se registra cualquier valor de resultado en la tarea devuelta, que se pone en un estado correcto.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="7db9f-1695">Si el cuerpo de la función finaliza como resultado de una excepción no detectada ([la instrucción throw](statements.md#the-throw-statement)), la excepción se registra en la tarea devuelta que se coloca en un estado de error.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="7db9f-1696">Evaluación de una función asincrónica que devuelve void</span><span class="sxs-lookup"><span data-stu-id="7db9f-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="7db9f-1697">Si el tipo de valor devuelto de la función Async es `void`, la evaluación difiere de la anterior de la siguiente manera: dado que no se devuelve ninguna tarea, la función comunica en su lugar la finalización y las excepciones al ***contexto de sincronización***del subproceso actual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="7db9f-1698">La definición exacta del contexto de sincronización depende de la implementación, pero es una representación de "Dónde" se está ejecutando el subproceso actual.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="7db9f-1699">El contexto de sincronización se notifica cuando se inicia la evaluación de una función asincrónica que devuelve void, se completa correctamente o provoca que se produzca una excepción no detectada.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="7db9f-1700">Esto permite que el contexto realice un seguimiento del número de funciones asincrónicas que devuelven void que se están ejecutando en él y de decidir cómo propagar las excepciones que salen de ellas.</span><span class="sxs-lookup"><span data-stu-id="7db9f-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
