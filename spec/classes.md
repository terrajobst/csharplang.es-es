---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703979"
---
# <a name="classes"></a><span data-ttu-id="84145-101">Clases</span><span class="sxs-lookup"><span data-stu-id="84145-101">Classes</span></span>

<span data-ttu-id="84145-102">Una clase es una estructura de datos que puede contener miembros de datos (constantes y campos), miembros de función (métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos) y tipos anidados.</span><span class="sxs-lookup"><span data-stu-id="84145-102">A class is a data structure that may contain data members (constants and fields), function members (methods, properties, events, indexers, operators, instance constructors, destructors and static constructors), and nested types.</span></span> <span data-ttu-id="84145-103">Los tipos de clase admiten la herencia, un mecanismo mediante el cual una clase derivada puede extender y especializar una clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-103">Class types support inheritance, a mechanism whereby a derived class can extend and specialize a base class.</span></span>

## <a name="class-declarations"></a><span data-ttu-id="84145-104">Declaraciones de clase</span><span class="sxs-lookup"><span data-stu-id="84145-104">Class declarations</span></span>

<span data-ttu-id="84145-105">Un *class_declaration* es un *type_declaration* ([declaraciones de tipos](namespaces.md#type-declarations)) que declara una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="84145-105">A *class_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new class.</span></span>

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

<span data-ttu-id="84145-106">Un *class_declaration* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *class_modifier*s ([modificadores de clase](classes.md#class-modifiers)), seguido de un modificador `partial` opcional, seguido de la palabra clave. `class` y un *identificador* que nombra la clase, seguido de un *type_parameter_list* opcional ([parámetros de tipo](classes.md#type-parameters)), seguido de una especificación de *class_base* opcional ([especificación de clase base](classes.md#class-base-specification)), seguida de un conjunto opcional de *type_parameter_constraints_clause*s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), seguido de un *class_body* ([cuerpo de clase](classes.md#class-body)), opcionalmente seguido de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-106">A *class_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *class_modifier*s ([Class modifiers](classes.md#class-modifiers)), followed by an optional `partial` modifier, followed by the keyword `class` and an *identifier* that names the class, followed by an optional *type_parameter_list* ([Type parameters](classes.md#type-parameters)), followed by an optional *class_base* specification ([Class base specification](classes.md#class-base-specification)) , followed by an optional set of *type_parameter_constraints_clause*s ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by a *class_body* ([Class body](classes.md#class-body)), optionally followed by a semicolon.</span></span>

<span data-ttu-id="84145-107">Una declaración de clase no puede proporcionar *type_parameter_constraints_clause*s a menos que también proporcione una *type_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="84145-107">A class declaration cannot supply *type_parameter_constraints_clause*s unless it also supplies a *type_parameter_list*.</span></span>

<span data-ttu-id="84145-108">Una declaración de clase que proporciona un *type_parameter_list* es una ***declaración de clase genérica***.</span><span class="sxs-lookup"><span data-stu-id="84145-108">A class declaration that supplies a *type_parameter_list* is a ***generic class declaration***.</span></span> <span data-ttu-id="84145-109">Además, cualquier clase anidada dentro de una declaración de clase genérica o una declaración de estructura genérica es una declaración de clase genérica, ya que se deben proporcionar los parámetros de tipo para el tipo contenedor para crear un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="84145-109">Additionally, any class nested inside a generic class declaration or a generic struct declaration is itself a generic class declaration, since type parameters for the containing type must be supplied to create a constructed type.</span></span>

### <a name="class-modifiers"></a><span data-ttu-id="84145-110">Modificadores de clase</span><span class="sxs-lookup"><span data-stu-id="84145-110">Class modifiers</span></span>

<span data-ttu-id="84145-111">Un *class_declaration* puede incluir opcionalmente una secuencia de modificadores de clase:</span><span class="sxs-lookup"><span data-stu-id="84145-111">A *class_declaration* may optionally include a sequence of class modifiers:</span></span>

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

<span data-ttu-id="84145-112">Es un error en tiempo de compilación que el mismo modificador aparezca varias veces en una declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-112">It is a compile-time error for the same modifier to appear multiple times in a class declaration.</span></span>

<span data-ttu-id="84145-113">El `new` modificador se permite en las clases anidadas.</span><span class="sxs-lookup"><span data-stu-id="84145-113">The `new` modifier is permitted on nested classes.</span></span> <span data-ttu-id="84145-114">Especifica que la clase oculta un miembro heredado con el mismo nombre, tal y como se describe en [el modificador New](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="84145-114">It specifies that the class hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span> <span data-ttu-id="84145-115">Se trata de un error en tiempo de compilación `new` para que el modificador aparezca en una declaración de clase que no es una declaración de clase anidada.</span><span class="sxs-lookup"><span data-stu-id="84145-115">It is a compile-time error for the `new` modifier to appear on a class declaration that is not a nested class declaration.</span></span>

<span data-ttu-id="84145-116">Los `public`modificadores `internal`, `protected`, y`private` controlan la accesibilidad de la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the class.</span></span> <span data-ttu-id="84145-117">Dependiendo del contexto en el que se produce la declaración de clase, es posible que algunos de estos modificadores no estén permitidos (se[declare accesibilidad](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="84145-117">Depending on the context in which the class declaration occurs, some of these modifiers may not be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="84145-118">Los `abstract`modificadores `static` , `sealed` y se describen en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="84145-118">The `abstract`, `sealed` and `static` modifiers are discussed in the following sections.</span></span>

#### <a name="abstract-classes"></a><span data-ttu-id="84145-119">Clases abstractas</span><span class="sxs-lookup"><span data-stu-id="84145-119">Abstract classes</span></span>

<span data-ttu-id="84145-120">El `abstract` modificador se usa para indicar que una clase está incompleta y que está destinada a usarse solo como una clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-120">The `abstract` modifier is used to indicate that a class is incomplete and that it is intended to be used only as a base class.</span></span> <span data-ttu-id="84145-121">Una clase abstracta es distinta de una clase no abstracta de las siguientes maneras:</span><span class="sxs-lookup"><span data-stu-id="84145-121">An abstract class differs from a non-abstract class in the following ways:</span></span>

*  <span data-ttu-id="84145-122">No se puede crear una instancia de una clase abstracta directamente y es un error en tiempo de compilación utilizar `new` el operador en una clase abstracta.</span><span class="sxs-lookup"><span data-stu-id="84145-122">An abstract class cannot be instantiated directly, and it is a compile-time error to use the `new` operator on an abstract class.</span></span> <span data-ttu-id="84145-123">Aunque es posible tener variables y valores cuyos tipos en tiempo de compilación sean abstractos, tales variables y valores serán necesariamente `null` o contendrán referencias a instancias de clases no abstractas derivadas de los tipos abstractos.</span><span class="sxs-lookup"><span data-stu-id="84145-123">While it is possible to have variables and values whose compile-time types are abstract, such variables and values will necessarily either be `null` or contain references to instances of non-abstract classes derived from the abstract types.</span></span>
*  <span data-ttu-id="84145-124">Se permite que una clase abstracta contenga miembros abstractos (aunque no es necesario).</span><span class="sxs-lookup"><span data-stu-id="84145-124">An abstract class is permitted (but not required) to contain abstract members.</span></span>
*  <span data-ttu-id="84145-125">Una clase abstracta no puede ser sellada.</span><span class="sxs-lookup"><span data-stu-id="84145-125">An abstract class cannot be sealed.</span></span>

<span data-ttu-id="84145-126">Cuando una clase no abstracta se deriva de una clase abstracta, la clase no abstracta debe incluir las implementaciones reales de todos los miembros abstractos heredados, con lo que se reemplazan los miembros abstractos.</span><span class="sxs-lookup"><span data-stu-id="84145-126">When a non-abstract class is derived from an abstract class, the non-abstract class must include actual implementations of all inherited abstract members, thereby overriding those abstract members.</span></span> <span data-ttu-id="84145-127">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-127">In the example</span></span>
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
<span data-ttu-id="84145-128">la clase `A` abstracta presenta un método `F`abstracto.</span><span class="sxs-lookup"><span data-stu-id="84145-128">the abstract class `A` introduces an abstract method `F`.</span></span> <span data-ttu-id="84145-129">La `B` clase introduce un método `G`adicional, pero como no proporciona una implementación de `F`, `B` también debe declararse como Abstract.</span><span class="sxs-lookup"><span data-stu-id="84145-129">Class `B` introduces an additional method `G`, but since it doesn't provide an implementation of `F`, `B` must also be declared abstract.</span></span> <span data-ttu-id="84145-130">La `C` clase`F` invalida y proporciona una implementación real.</span><span class="sxs-lookup"><span data-stu-id="84145-130">Class `C` overrides `F` and provides an actual implementation.</span></span> <span data-ttu-id="84145-131">Dado que no hay miembros abstractos `C`en `C` , se permite (pero no es necesario) que no sea abstracto.</span><span class="sxs-lookup"><span data-stu-id="84145-131">Since there are no abstract members in `C`, `C` is permitted (but not required) to be non-abstract.</span></span>

#### <a name="sealed-classes"></a><span data-ttu-id="84145-132">Clases selladas</span><span class="sxs-lookup"><span data-stu-id="84145-132">Sealed classes</span></span>

<span data-ttu-id="84145-133">El `sealed` modificador se usa para evitar la derivación de una clase.</span><span class="sxs-lookup"><span data-stu-id="84145-133">The `sealed` modifier is used to prevent derivation from a class.</span></span> <span data-ttu-id="84145-134">Se produce un error en tiempo de compilación si se especifica una clase sellada como la clase base de otra clase.</span><span class="sxs-lookup"><span data-stu-id="84145-134">A compile-time error occurs if a sealed class is specified as the base class of another class.</span></span>

<span data-ttu-id="84145-135">Una clase sellada no puede ser también una clase abstracta.</span><span class="sxs-lookup"><span data-stu-id="84145-135">A sealed class cannot also be an abstract class.</span></span>

<span data-ttu-id="84145-136">El `sealed` modificador se usa principalmente para evitar la derivación no deseada, pero también permite ciertas optimizaciones en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="84145-136">The `sealed` modifier is primarily used to prevent unintended derivation, but it also enables certain run-time optimizations.</span></span> <span data-ttu-id="84145-137">En concreto, dado que se sabe que una clase sellada nunca tiene clases derivadas, es posible transformar las invocaciones de miembros de función virtual en instancias de clase selladas en invocaciones no virtuales.</span><span class="sxs-lookup"><span data-stu-id="84145-137">In particular, because a sealed class is known to never have any derived classes, it is possible to transform virtual function member invocations on sealed class instances into non-virtual invocations.</span></span>

#### <a name="static-classes"></a><span data-ttu-id="84145-138">Clases estáticas</span><span class="sxs-lookup"><span data-stu-id="84145-138">Static classes</span></span>

<span data-ttu-id="84145-139">El `static` modificador se usa para marcar la clase que se declara como una ***clase estática***.</span><span class="sxs-lookup"><span data-stu-id="84145-139">The `static` modifier is used to mark the class being declared as a ***static class***.</span></span> <span data-ttu-id="84145-140">No se puede crear una instancia de una clase estática, no se puede usar como un tipo y solo puede contener miembros estáticos.</span><span class="sxs-lookup"><span data-stu-id="84145-140">A static class cannot be instantiated, cannot be used as a type and can contain only static members.</span></span> <span data-ttu-id="84145-141">Solo una clase estática puede contener declaraciones de métodos de extensión ([métodos de extensión](classes.md#extension-methods)).</span><span class="sxs-lookup"><span data-stu-id="84145-141">Only a static class can contain declarations of extension methods ([Extension methods](classes.md#extension-methods)).</span></span>

<span data-ttu-id="84145-142">Una declaración de clase estática está sujeta a las siguientes restricciones:</span><span class="sxs-lookup"><span data-stu-id="84145-142">A static class declaration is subject to the following restrictions:</span></span>

*  <span data-ttu-id="84145-143">Una clase estática no puede incluir un `sealed` modificador o `abstract` .</span><span class="sxs-lookup"><span data-stu-id="84145-143">A static class may not include a `sealed` or `abstract` modifier.</span></span> <span data-ttu-id="84145-144">Sin embargo, tenga en cuenta que, puesto que no se pueden crear instancias de una clase estática ni derivarse de, se comporta como si fuera Sealed y Abstract.</span><span class="sxs-lookup"><span data-stu-id="84145-144">Note, however, that since a static class cannot be instantiated or derived from, it behaves as if it was both sealed and abstract.</span></span>
*  <span data-ttu-id="84145-145">Una clase estática no puede incluir una especificación *class_base* ([especificación de clase base](classes.md#class-base-specification)) y no puede especificar explícitamente una clase base o una lista de interfaces implementadas.</span><span class="sxs-lookup"><span data-stu-id="84145-145">A static class may not include a *class_base* specification ([Class base specification](classes.md#class-base-specification)) and cannot explicitly specify a base class or a list of implemented interfaces.</span></span> <span data-ttu-id="84145-146">Una clase estática hereda implícitamente del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="84145-146">A static class implicitly inherits from type `object`.</span></span>
*  <span data-ttu-id="84145-147">Una clase estática solo puede contener miembros estáticos ([miembros estáticos y de instancia](classes.md#static-and-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="84145-147">A static class can only contain static members ([Static and instance members](classes.md#static-and-instance-members)).</span></span> <span data-ttu-id="84145-148">Tenga en cuenta que las constantes y los tipos anidados se clasifican como miembros estáticos.</span><span class="sxs-lookup"><span data-stu-id="84145-148">Note that constants and nested types are classified as static members.</span></span>
*  <span data-ttu-id="84145-149">Una clase estática no puede tener miembros `protected` con `protected internal` o declarar accesibilidad.</span><span class="sxs-lookup"><span data-stu-id="84145-149">A static class cannot have members with `protected` or `protected internal` declared accessibility.</span></span>

<span data-ttu-id="84145-150">Es un error en tiempo de compilación infringir cualquiera de estas restricciones.</span><span class="sxs-lookup"><span data-stu-id="84145-150">It is a compile-time error to violate any of these restrictions.</span></span>

<span data-ttu-id="84145-151">Una clase estática no tiene ningún constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-151">A static class has no instance constructors.</span></span> <span data-ttu-id="84145-152">No es posible declarar un constructor de instancia en una clase estática, y no se proporciona ningún constructor de instancia predeterminado ([constructores predeterminados](classes.md#default-constructors)) para una clase estática.</span><span class="sxs-lookup"><span data-stu-id="84145-152">It is not possible to declare an instance constructor in a static class, and no default instance constructor ([Default constructors](classes.md#default-constructors)) is provided for a static class.</span></span>

<span data-ttu-id="84145-153">Los miembros de una clase estática no son estáticos automáticamente y las declaraciones de miembros deben incluir explícitamente `static` un modificador (excepto para las constantes y los tipos anidados).</span><span class="sxs-lookup"><span data-stu-id="84145-153">The members of a static class are not automatically static, and the member declarations must explicitly include a `static` modifier (except for constants and nested types).</span></span> <span data-ttu-id="84145-154">Cuando una clase está anidada dentro de una clase externa estática, la clase anidada no es una clase estática a menos que incluya `static` explícitamente un modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-154">When a class is nested within a static outer class, the nested class is not a static class unless it explicitly includes a `static` modifier.</span></span>

<span data-ttu-id="84145-155">__Referencia a tipos de clase estáticos__</span><span class="sxs-lookup"><span data-stu-id="84145-155">__Referencing static class types__</span></span>

<span data-ttu-id="84145-156">Se permite que un *namespace_or_type_name* ([espacio de nombres y nombres de tipo](basic-concepts.md#namespace-and-type-names)) haga referencia a una clase estática Si</span><span class="sxs-lookup"><span data-stu-id="84145-156">A *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="84145-157">*Namespace_or_type_name* es el `T` en un *namespace_or_type_name* con el formato `T.I`, o bien</span><span class="sxs-lookup"><span data-stu-id="84145-157">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="84145-158">*Namespace_or_type_name* es el `T` en *typeof_expression* (listas de[argumentos](expressions.md#argument-lists)1) con el formato `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="84145-158">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

<span data-ttu-id="84145-159">Se permite que un *primary_expression* ([miembros de función](expressions.md#function-members)) haga referencia a una clase estática Si</span><span class="sxs-lookup"><span data-stu-id="84145-159">A *primary_expression* ([Function members](expressions.md#function-members)) is permitted to reference a static class if</span></span>

*  <span data-ttu-id="84145-160">*Primary_expression* es el `E` en *member_access* ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) con el formato `E.I`.</span><span class="sxs-lookup"><span data-stu-id="84145-160">The *primary_expression* is the `E` in a *member_access* ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) of the form `E.I`.</span></span>

<span data-ttu-id="84145-161">En cualquier otro contexto, se trata de un error en tiempo de compilación para hacer referencia a una clase estática.</span><span class="sxs-lookup"><span data-stu-id="84145-161">In any other context it is a compile-time error to reference a static class.</span></span> <span data-ttu-id="84145-162">Por ejemplo, es un error que una clase estática se use como una clase base, un tipo constituyente ([tipos anidados](classes.md#nested-types)) de un miembro, un argumento de tipo genérico o una restricción de parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-162">For example, it is an error for a static class to be used as a base class, a constituent type ([Nested types](classes.md#nested-types)) of a member, a generic type argument, or a type parameter constraint.</span></span> <span data-ttu-id="84145-163">Del mismo modo, una clase estática no se puede usar en un tipo de matriz, un `new` tipo de puntero, una expresión, `is` una expresión de `as` conversión, una `sizeof` expresión, una expresión, una expresión o una expresión de valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="84145-163">Likewise, a static class cannot be used in an array type, a pointer type, a `new` expression, a cast expression, an `is` expression, an `as` expression, a `sizeof` expression, or a default value expression.</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="84145-164">Unmodifier (modificador)</span><span class="sxs-lookup"><span data-stu-id="84145-164">Partial modifier</span></span>

<span data-ttu-id="84145-165">El modificador `partial` se usa para indicar que este *class_declaration* es una declaración de tipo parcial.</span><span class="sxs-lookup"><span data-stu-id="84145-165">The `partial` modifier is used to indicate that this *class_declaration* is a partial type declaration.</span></span> <span data-ttu-id="84145-166">Varias declaraciones de tipos parciales con el mismo nombre dentro de una declaración de tipo o espacio de nombres envolvente se combinan para formar una declaración de tipos, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="84145-166">Multiple partial type declarations with the same name within an enclosing namespace or type declaration combine to form one type declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

<span data-ttu-id="84145-167">Tener la declaración de una clase distribuida sobre segmentos independientes del texto del programa puede ser útil si estos segmentos se producen o mantienen en contextos diferentes.</span><span class="sxs-lookup"><span data-stu-id="84145-167">Having the declaration of a class distributed over separate segments of program text can be useful if these segments are produced or maintained in different contexts.</span></span> <span data-ttu-id="84145-168">Por ejemplo, una parte de una declaración de clase puede ser generada por el equipo, mientras que la otra se crea manualmente.</span><span class="sxs-lookup"><span data-stu-id="84145-168">For instance, one part of a class declaration may be machine generated, whereas the other is manually authored.</span></span> <span data-ttu-id="84145-169">La separación textual de los dos impide que las actualizaciones se realicen en conflicto con las actualizaciones del otro.</span><span class="sxs-lookup"><span data-stu-id="84145-169">Textual separation of the two prevents updates by one from conflicting with updates by the other.</span></span>

### <a name="type-parameters"></a><span data-ttu-id="84145-170">Parámetros de tipo</span><span class="sxs-lookup"><span data-stu-id="84145-170">Type parameters</span></span>

<span data-ttu-id="84145-171">Un parámetro de tipo es un identificador simple que denota un marcador de posición para un argumento de tipo proporcionado para crear un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="84145-171">A type parameter is a simple identifier that denotes a placeholder for a type argument supplied to create a constructed type.</span></span> <span data-ttu-id="84145-172">Un parámetro de tipo es un marcador de posición formal para un tipo que se proporcionará más adelante.</span><span class="sxs-lookup"><span data-stu-id="84145-172">A type parameter is a formal placeholder for a type that will be supplied later.</span></span> <span data-ttu-id="84145-173">Por el contrario, un argumento de tipo ([argumentos de tipo](types.md#type-arguments)) es el tipo real que se sustituye por el parámetro de tipo cuando se crea un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="84145-173">By contrast, a type argument ([Type arguments](types.md#type-arguments)) is the actual type that is substituted for the type parameter when a constructed type is created.</span></span>

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

<span data-ttu-id="84145-174">Cada parámetro de tipo de una declaración de clase define un nombre en el espacio de declaración ([declaraciones](basic-concepts.md#declarations)) de esa clase.</span><span class="sxs-lookup"><span data-stu-id="84145-174">Each type parameter in a class declaration defines a name in the declaration space ([Declarations](basic-concepts.md#declarations)) of that class.</span></span> <span data-ttu-id="84145-175">Por lo tanto, no puede tener el mismo nombre que otro parámetro de tipo o un miembro declarado en esa clase.</span><span class="sxs-lookup"><span data-stu-id="84145-175">Thus, it cannot have the same name as another type parameter or a member declared in that class.</span></span> <span data-ttu-id="84145-176">Un parámetro de tipo no puede tener el mismo nombre que el propio tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-176">A type parameter cannot have the same name as the type itself.</span></span>

### <a name="class-base-specification"></a><span data-ttu-id="84145-177">Especificación de clase base</span><span class="sxs-lookup"><span data-stu-id="84145-177">Class base specification</span></span>

<span data-ttu-id="84145-178">Una declaración de clase puede incluir una especificación de *class_base* , que define la clase base directa de la clase y las interfaces ([interfaces](interfaces.md)) implementadas directamente por la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-178">A class declaration may include a *class_base* specification, which defines the direct base class of the class and the interfaces ([Interfaces](interfaces.md)) directly implemented by the class.</span></span>

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

<span data-ttu-id="84145-179">La clase base especificada en una declaración de clase puede ser un tipo de clase construido ([tipos construidos](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="84145-179">The base class specified in a class declaration can be a constructed class type ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="84145-180">Una clase base no puede ser un parámetro de tipo por su cuenta, aunque puede incluir los parámetros de tipo que se encuentran en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="84145-180">A base class cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span>

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a><span data-ttu-id="84145-181">Clases base</span><span class="sxs-lookup"><span data-stu-id="84145-181">Base classes</span></span>

<span data-ttu-id="84145-182">Cuando se incluye un *class_type* en el *class_base*, especifica la clase base directa de la clase que se declara.</span><span class="sxs-lookup"><span data-stu-id="84145-182">When a *class_type* is included in the *class_base*, it specifies the direct base class of the class being declared.</span></span> <span data-ttu-id="84145-183">Si una declaración de clase no tiene *class_base*, o si *class_base* solo muestra los tipos de interfaz, se supone que la clase base directa es `object`.</span><span class="sxs-lookup"><span data-stu-id="84145-183">If a class declaration has no *class_base*, or if the *class_base* lists only interface types, the direct base class is assumed to be `object`.</span></span> <span data-ttu-id="84145-184">Una clase hereda los miembros de su clase base directa, como se describe en [herencia](classes.md#inheritance).</span><span class="sxs-lookup"><span data-stu-id="84145-184">A class inherits members from its direct base class, as described in [Inheritance](classes.md#inheritance).</span></span>

<span data-ttu-id="84145-185">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-185">In the example</span></span>
```csharp
class A {}

class B: A {}
```
<span data-ttu-id="84145-186">se `A` dice que la clase es la clase base directa `B`de y `B` se dice que se deriva de `A`.</span><span class="sxs-lookup"><span data-stu-id="84145-186">class `A` is said to be the direct base class of `B`, and `B` is said to be derived from `A`.</span></span> <span data-ttu-id="84145-187">Puesto `A` que no especifica explícitamente una clase base directa, su clase base directa es `object`implícitamente.</span><span class="sxs-lookup"><span data-stu-id="84145-187">Since `A` does not explicitly specify a direct base class, its direct base class is implicitly `object`.</span></span>

<span data-ttu-id="84145-188">Para un tipo de clase construido, si se especifica una clase base en la declaración de clase genérica, la clase base del tipo construido se obtiene sustituyendo, para cada *type_parameter* en la declaración de clase base, el *type_argument correspondiente* del tipo construido.</span><span class="sxs-lookup"><span data-stu-id="84145-188">For a constructed class type, if a base class is specified in the generic class declaration, the base class of the constructed type is obtained by substituting, for each *type_parameter* in the base class declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="84145-189">Dadas las declaraciones de clase genéricas</span><span class="sxs-lookup"><span data-stu-id="84145-189">Given the generic class declarations</span></span>
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
<span data-ttu-id="84145-190">la clase base del tipo `G<int>` construido `B<string,int[]>`sería.</span><span class="sxs-lookup"><span data-stu-id="84145-190">the base class of the constructed type `G<int>` would be `B<string,int[]>`.</span></span>

<span data-ttu-id="84145-191">La clase base directa de un tipo de clase debe ser al menos igual de accesible que el propio tipo de clase ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)).</span><span class="sxs-lookup"><span data-stu-id="84145-191">The direct base class of a class type must be at least as accessible as the class type itself ([Accessibility domains](basic-concepts.md#accessibility-domains)).</span></span> <span data-ttu-id="84145-192">Por ejemplo, se trata de un error en tiempo de compilación `public` para que una clase se `private` derive `internal` de una clase o.</span><span class="sxs-lookup"><span data-stu-id="84145-192">For example, it is a compile-time error for a `public` class to derive from a `private` or `internal` class.</span></span>

<span data-ttu-id="84145-193">La clase base directa de un tipo de clase no debe ser ninguno de los siguientes tipos `System.Array`: `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, o `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="84145-193">The direct base class of a class type must not be any of the following types: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, or `System.ValueType`.</span></span> <span data-ttu-id="84145-194">Además, una declaración de clase genérica no `System.Attribute` puede usar como una clase base directa o indirecta.</span><span class="sxs-lookup"><span data-stu-id="84145-194">Furthermore, a generic class declaration cannot use `System.Attribute` as a direct or indirect base class.</span></span>

<span data-ttu-id="84145-195">A la hora de determinar el significado de la especificación `A` de clase base `B`directa de una clase, se `B` supone que `object`la clase base directa de es.</span><span class="sxs-lookup"><span data-stu-id="84145-195">While determining the meaning of the direct base class specification `A` of a class `B`, the direct base class of `B` is temporarily assumed to be `object`.</span></span> <span data-ttu-id="84145-196">Intuitivamente esto garantiza que el significado de una especificación de clase base no puede depender recursivamente.</span><span class="sxs-lookup"><span data-stu-id="84145-196">Intuitively this ensures that the meaning of a base class specification cannot recursively depend on itself.</span></span> <span data-ttu-id="84145-197">En el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-197">The example:</span></span>
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
<span data-ttu-id="84145-198">es un error porque `A<C.B>` `object`, en la especificación de clase base, la clase `C` base directa de se considera y, por tanto, `C` las reglas de [nombres de espacio de nombres y de tipo](basic-concepts.md#namespace-and-type-names)no se considera que tiene un miembro. `B`.</span><span class="sxs-lookup"><span data-stu-id="84145-198">is in error since in the base class specification `A<C.B>` the direct base class of `C` is considered to be `object`, and hence (by the rules of [Namespace and type names](basic-concepts.md#namespace-and-type-names))  `C` is not considered to have a member `B`.</span></span>

<span data-ttu-id="84145-199">Las clases base de un tipo de clase son la clase base directa y sus clases base.</span><span class="sxs-lookup"><span data-stu-id="84145-199">The base classes of a class type are the direct base class and its base classes.</span></span> <span data-ttu-id="84145-200">En otras palabras, el conjunto de clases base es el cierre transitivo de la relación de clase base directa.</span><span class="sxs-lookup"><span data-stu-id="84145-200">In other words, the set of base classes is the transitive closure of the direct base class relationship.</span></span> <span data-ttu-id="84145-201">En el ejemplo anterior, las clases base de `B` son `A` y `object`.</span><span class="sxs-lookup"><span data-stu-id="84145-201">Referring to the example above, the base classes of `B` are `A` and `object`.</span></span> <span data-ttu-id="84145-202">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-202">In the example</span></span>
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
<span data-ttu-id="84145-203">las clases base de `D<int>` son `C<int[]>`, `B<IComparable<int[]>>`, `A`y. `object`</span><span class="sxs-lookup"><span data-stu-id="84145-203">the base classes of `D<int>` are `C<int[]>`, `B<IComparable<int[]>>`, `A`, and `object`.</span></span>

<span data-ttu-id="84145-204">A excepción de `object`la clase, cada tipo de clase tiene exactamente una clase base directa.</span><span class="sxs-lookup"><span data-stu-id="84145-204">Except for class `object`, every class type has exactly one direct base class.</span></span> <span data-ttu-id="84145-205">La `object` clase no tiene ninguna clase base directa y es la última clase base de todas las demás clases.</span><span class="sxs-lookup"><span data-stu-id="84145-205">The `object` class has no direct base class and is the ultimate base class of all other classes.</span></span>

<span data-ttu-id="84145-206">Cuando una clase `B` se deriva de una clase `A`, es un error en tiempo de compilación para `A` que dependa `B`de.</span><span class="sxs-lookup"><span data-stu-id="84145-206">When a class `B` derives from a class `A`, it is a compile-time error for `A` to depend on `B`.</span></span> <span data-ttu-id="84145-207">Una clase ***depende directamente de*** su clase base directa (si existe) y ***depende directamente de*** la clase en la que se anida inmediatamente (si existe).</span><span class="sxs-lookup"><span data-stu-id="84145-207">A class ***directly depends on*** its direct base class (if any) and ***directly depends on*** the class within which it is immediately nested (if any).</span></span> <span data-ttu-id="84145-208">Dada esta definición, el conjunto completo de clases de las que depende una clase es el cierre reflexivo y transitivo de la relación ***directamente depende de*** .</span><span class="sxs-lookup"><span data-stu-id="84145-208">Given this definition, the complete set of classes upon which a class depends is the reflexive and transitive closure of the ***directly depends on*** relationship.</span></span>

<span data-ttu-id="84145-209">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-209">The example</span></span>
```csharp
class A: A {}
```
<span data-ttu-id="84145-210">es erróneo porque la clase depende de sí misma.</span><span class="sxs-lookup"><span data-stu-id="84145-210">is erroneous because the class depends on itself.</span></span> <span data-ttu-id="84145-211">Del mismo modo, el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-211">Likewise, the example</span></span>
```csharp
class A: B {}
class B: C {}
class C: A {}
```
<span data-ttu-id="84145-212">es un error porque las clases dependen circularmente por sí mismas.</span><span class="sxs-lookup"><span data-stu-id="84145-212">is in error because the classes circularly depend on themselves.</span></span> <span data-ttu-id="84145-213">Por último, el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-213">Finally, the example</span></span>
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
<span data-ttu-id="84145-214">produce un error en tiempo de compilación porque `A` depende de `B.C` (su clase base directa), que depende de `B` (su clase envolvente inmediata), que depende circularmente de `A`.</span><span class="sxs-lookup"><span data-stu-id="84145-214">results in a compile-time error because `A` depends on `B.C` (its direct base class), which depends on `B` (its immediately enclosing class), which circularly depends on `A`.</span></span>

<span data-ttu-id="84145-215">Tenga en cuenta que una clase no depende de las clases anidadas en ella.</span><span class="sxs-lookup"><span data-stu-id="84145-215">Note that a class does not depend on the classes that are nested within it.</span></span> <span data-ttu-id="84145-216">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-216">In the example</span></span>
```csharp
class A
{
    class B: A {}
}
```
<span data-ttu-id="84145-217">`B`depende de `A` (porque `A` es tanto su clase base directa como su clase envolvente inmediata), pero `A` no depende de `B` (puesto que `B` no es una clase base ni una clase envolvente de `A`).</span><span class="sxs-lookup"><span data-stu-id="84145-217">`B` depends on `A` (because `A` is both its direct base class and its immediately enclosing class), but `A` does not depend on `B` (since `B` is neither a base class nor an enclosing class of `A`).</span></span> <span data-ttu-id="84145-218">Por lo tanto, el ejemplo es válido.</span><span class="sxs-lookup"><span data-stu-id="84145-218">Thus, the example is valid.</span></span>

<span data-ttu-id="84145-219">No se puede derivar de una `sealed` clase.</span><span class="sxs-lookup"><span data-stu-id="84145-219">It is not possible to derive from a `sealed` class.</span></span> <span data-ttu-id="84145-220">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-220">In the example</span></span>
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
<span data-ttu-id="84145-221">la `B` clase tiene un error porque intenta derivar de la `sealed` clase `A`.</span><span class="sxs-lookup"><span data-stu-id="84145-221">class `B` is in error because it attempts to derive from the `sealed` class `A`.</span></span>

#### <a name="interface-implementations"></a><span data-ttu-id="84145-222">Implementaciones de interfaces</span><span class="sxs-lookup"><span data-stu-id="84145-222">Interface implementations</span></span>

<span data-ttu-id="84145-223">Una especificación de *class_base* puede incluir una lista de tipos de interfaz, en cuyo caso se dice que la clase implementa directamente los tipos de interfaz especificados.</span><span class="sxs-lookup"><span data-stu-id="84145-223">A *class_base* specification may include a list of interface types, in which case the class is said to directly implement the given interface types.</span></span> <span data-ttu-id="84145-224">Las implementaciones de interfaz se tratan en las [implementaciones de interfaz](interfaces.md#interface-implementations).</span><span class="sxs-lookup"><span data-stu-id="84145-224">Interface implementations are discussed further in [Interface implementations](interfaces.md#interface-implementations).</span></span>

### <a name="type-parameter-constraints"></a><span data-ttu-id="84145-225">Restricciones de parámetros de tipo</span><span class="sxs-lookup"><span data-stu-id="84145-225">Type parameter constraints</span></span>

<span data-ttu-id="84145-226">Las declaraciones de tipos y métodos genéricos pueden especificar opcionalmente restricciones de parámetros de tipo mediante la inclusión de *type_parameter_constraints_clause*s.</span><span class="sxs-lookup"><span data-stu-id="84145-226">Generic type and method declarations can optionally specify type parameter constraints by including *type_parameter_constraints_clause*s.</span></span>

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

<span data-ttu-id="84145-227">Cada *type_parameter_constraints_clause* consta del token `where`, seguido del nombre de un parámetro de tipo, seguido de dos puntos y la lista de restricciones para ese parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-227">Each *type_parameter_constraints_clause* consists of the token `where`, followed by the name of a type parameter, followed by a colon and the list of constraints for that type parameter.</span></span> <span data-ttu-id="84145-228">Puede haber como máximo una `where` cláusula para cada parámetro de tipo y las `where` cláusulas se pueden enumerar en cualquier orden.</span><span class="sxs-lookup"><span data-stu-id="84145-228">There can be at most one `where` clause for each type parameter, and the `where` clauses can be listed in any order.</span></span> <span data-ttu-id="84145-229">Al igual `get` que `set` los tokens y en un descriptor de acceso de propiedad, el `where` token no es una palabra clave.</span><span class="sxs-lookup"><span data-stu-id="84145-229">Like the `get` and `set` tokens in a property accessor, the `where` token is not a keyword.</span></span>

<span data-ttu-id="84145-230">La lista de restricciones dadas en una `where` cláusula puede incluir cualquiera de los componentes siguientes, en este orden: una restricción Primary única, una o más restricciones secundarias y la restricción de constructor,. `new()`</span><span class="sxs-lookup"><span data-stu-id="84145-230">The list of constraints given in a `where` clause can include any of the following components, in this order: a single primary constraint, one or more secondary constraints, and the constructor constraint, `new()`.</span></span>

<span data-ttu-id="84145-231">Una restricción Primary puede ser un tipo de clase o la restricción de tipo de ***referencia*** `class` o la ***restricción*** `struct`de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-231">A primary constraint can be a class type or the ***reference type constraint*** `class` or the ***value type constraint*** `struct`.</span></span> <span data-ttu-id="84145-232">Una restricción secundaria puede ser un *type_parameter* o *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="84145-232">A secondary constraint can be a *type_parameter* or *interface_type*.</span></span>

<span data-ttu-id="84145-233">La restricción de tipo de referencia especifica que un argumento de tipo usado para el parámetro de tipo debe ser un tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="84145-233">The reference type constraint specifies that a type argument used for the type parameter must be a reference type.</span></span> <span data-ttu-id="84145-234">Todos los tipos de clase, tipos de interfaz, tipos de delegado, tipos de matriz y parámetros de tipo que se sabe que son un tipo de referencia (como se define a continuación) satisfacen esta restricción.</span><span class="sxs-lookup"><span data-stu-id="84145-234">All class types, interface types, delegate types, array types, and type parameters known to be a reference type (as defined below) satisfy this constraint.</span></span>

<span data-ttu-id="84145-235">La restricción de tipo de valor especifica que un argumento de tipo usado para el parámetro de tipo debe ser un tipo de valor que no acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="84145-235">The value type constraint specifies that a type argument used for the type parameter must be a non-nullable value type.</span></span> <span data-ttu-id="84145-236">Todos los tipos de struct que no aceptan valores NULL, tipos de enumeración y parámetros de tipo con la restricción de tipo de valor cumplen esta restricción.</span><span class="sxs-lookup"><span data-stu-id="84145-236">All non-nullable struct types, enum types, and type parameters having the value type constraint satisfy this constraint.</span></span> <span data-ttu-id="84145-237">Tenga en cuenta que aunque se clasifique como un tipo de valor, un tipo que acepta valores NULL ([tipos que aceptan valores NULL](types.md#nullable-types)) no satisface la restricción de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-237">Note that although classified as a value type, a nullable type ([Nullable types](types.md#nullable-types)) does not satisfy the value type constraint.</span></span> <span data-ttu-id="84145-238">Un parámetro de tipo que tiene la restricción de tipo de valor no puede tener también *constructor_constraint*.</span><span class="sxs-lookup"><span data-stu-id="84145-238">A type parameter having the value type constraint cannot also have the *constructor_constraint*.</span></span>

<span data-ttu-id="84145-239">Los tipos de puntero nunca pueden ser argumentos de tipo y no se tienen en cuenta para satisfacer las restricciones de tipo de valor o tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="84145-239">Pointer types are never allowed to be type arguments and are not considered to satisfy either the reference type or value type constraints.</span></span>

<span data-ttu-id="84145-240">Si una restricción es un tipo de clase, un tipo de interfaz o un parámetro de tipo, ese tipo especifica un "tipo base" mínimo que cada argumento de tipo utilizado para ese parámetro de tipo debe admitir.</span><span class="sxs-lookup"><span data-stu-id="84145-240">If a constraint is a class type, an interface type, or a type parameter, that type specifies a minimal "base type" that every type argument used for that type parameter must support.</span></span> <span data-ttu-id="84145-241">Siempre que se usa un tipo construido o un método genérico, el argumento de tipo se compara con las restricciones del parámetro de tipo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-241">Whenever a constructed type or generic method is used, the type argument is checked against the constraints on the type parameter at compile-time.</span></span> <span data-ttu-id="84145-242">El argumento de tipo proporcionado debe cumplir las condiciones descritas en satisfacción de las [restricciones](types.md#satisfying-constraints).</span><span class="sxs-lookup"><span data-stu-id="84145-242">The type argument supplied must satisfy the conditions described in [Satisfying constraints](types.md#satisfying-constraints).</span></span>

<span data-ttu-id="84145-243">Una restricción *class_type* debe cumplir las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="84145-243">A *class_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="84145-244">El tipo debe ser un tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-244">The type must be a class type.</span></span>
*  <span data-ttu-id="84145-245">El tipo no debe ser `sealed`.</span><span class="sxs-lookup"><span data-stu-id="84145-245">The type must not be `sealed`.</span></span>
*  <span data-ttu-id="84145-246">El tipo no debe ser uno de los tipos siguientes: `System.Array`, `System.Delegate`, `System.Enum`o `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="84145-246">The type must not be one of the following types: `System.Array`, `System.Delegate`, `System.Enum`, or `System.ValueType`.</span></span>
*  <span data-ttu-id="84145-247">El tipo no debe ser `object`.</span><span class="sxs-lookup"><span data-stu-id="84145-247">The type must not be `object`.</span></span> <span data-ttu-id="84145-248">Dado que todos los tipos `object`se derivan de, este tipo de restricción no tendría ningún efecto si se permitiera.</span><span class="sxs-lookup"><span data-stu-id="84145-248">Because all types derive from `object`, such a constraint would have no effect if it were permitted.</span></span>
*  <span data-ttu-id="84145-249">Como máximo, una restricción para un parámetro de tipo determinado puede ser un tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-249">At most one constraint for a given type parameter can be a class type.</span></span>

<span data-ttu-id="84145-250">Un tipo especificado como una restricción *interface_type* debe cumplir las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="84145-250">A type specified as an *interface_type* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="84145-251">El tipo debe ser un tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="84145-251">The type must be an interface type.</span></span>
*  <span data-ttu-id="84145-252">Un tipo no debe especificarse más de una vez en una `where` cláusula determinada.</span><span class="sxs-lookup"><span data-stu-id="84145-252">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="84145-253">En cualquier caso, la restricción puede incluir cualquiera de los parámetros de tipo del tipo o la declaración de método asociados como parte de un tipo construido, y puede implicar el tipo que se declara.</span><span class="sxs-lookup"><span data-stu-id="84145-253">In either case, the constraint can involve any of the type parameters of the associated type or method declaration as part of a constructed type, and can involve the type being declared.</span></span>

<span data-ttu-id="84145-254">Cualquier tipo de clase o interfaz especificado como restricción de parámetro de tipo debe ser al menos igual de accesible ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)) que el tipo o método genérico que se está declarando.</span><span class="sxs-lookup"><span data-stu-id="84145-254">Any class or interface type specified as a type parameter constraint must be at least as accessible ([Accessibility constraints](basic-concepts.md#accessibility-constraints)) as the generic type or method being declared.</span></span>

<span data-ttu-id="84145-255">Un tipo especificado como restricción *type_parameter* debe cumplir las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="84145-255">A type specified as a *type_parameter* constraint must satisfy the following rules:</span></span>

*  <span data-ttu-id="84145-256">El tipo debe ser un parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-256">The type must be a type parameter.</span></span>
*  <span data-ttu-id="84145-257">Un tipo no debe especificarse más de una vez en una `where` cláusula determinada.</span><span class="sxs-lookup"><span data-stu-id="84145-257">A type must not be specified more than once in a given `where` clause.</span></span>

<span data-ttu-id="84145-258">Además, no debe haber ningún ciclo en el gráfico de dependencias de los parámetros de tipo, donde Dependency es una relación transitiva definida por:</span><span class="sxs-lookup"><span data-stu-id="84145-258">In addition there must be no cycles in the dependency graph of type parameters, where dependency is a transitive relation defined by:</span></span>

*  <span data-ttu-id="84145-259">Si se usa un `T` parámetro de tipo como una restricción para el `S` parámetro `S` de tipo, ***depende de*** `T`.</span><span class="sxs-lookup"><span data-stu-id="84145-259">If a type parameter `T` is used as a constraint for type parameter `S` then `S` ***depends on*** `T`.</span></span>
*  <span data-ttu-id="84145-260">Si un parámetro de `S` tipo depende de un parámetro `T` de `T` tipo y depende de un `U` parámetro `S` de tipo, ***depende de*** `U`.</span><span class="sxs-lookup"><span data-stu-id="84145-260">If a type parameter `S` depends on a type parameter `T` and `T` depends on a type parameter `U` then `S` ***depends on*** `U`.</span></span>

<span data-ttu-id="84145-261">Dada esta relación, se trata de un error en tiempo de compilación para que un parámetro de tipo dependa de sí mismo (directa o indirectamente).</span><span class="sxs-lookup"><span data-stu-id="84145-261">Given this relation, it is a compile-time error for a type parameter to depend on itself (directly or indirectly).</span></span>

<span data-ttu-id="84145-262">Cualquier restricción debe ser coherente entre los parámetros de tipo dependiente.</span><span class="sxs-lookup"><span data-stu-id="84145-262">Any constraints must be consistent among dependent type parameters.</span></span> <span data-ttu-id="84145-263">Si el parámetro `S` de tipo depende del `T` parámetro de tipo, entonces:</span><span class="sxs-lookup"><span data-stu-id="84145-263">If type parameter `S` depends on type parameter `T` then:</span></span>

*  <span data-ttu-id="84145-264">`T`no debe tener la restricción de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-264">`T` must not have the value type constraint.</span></span> <span data-ttu-id="84145-265">De lo `T` contrario, se sella `S` de forma eficaz, de modo que se forzaría que sea del mismo tipo que, lo que `T`elimina la necesidad de dos parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-265">Otherwise, `T` is effectively sealed so `S` would be forced to be the same type as `T`, eliminating the need for two type parameters.</span></span>
*  <span data-ttu-id="84145-266">Si `S` tiene la restricción de tipo de valor, `T` no debe tener una restricción *class_type* .</span><span class="sxs-lookup"><span data-stu-id="84145-266">If `S` has the value type constraint then `T` must not have a *class_type* constraint.</span></span>
*  <span data-ttu-id="84145-267">Si `S` tiene una restricción *class_type* `A` y `T` tiene una restricción *class_type* `B`, debe haber una conversión de identidad o una conversión de referencia implícita de `A` a `B` o una conversión de referencia implícita de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="84145-267">If `S` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>
*  <span data-ttu-id="84145-268">Si `S` también depende del parámetro de tipo `U` y `U` tiene una restricción *class_type* `A` y `T` tiene una restricción *class_type* `B`, debe haber una conversión de identidad o una conversión de referencia implícita de `A` para `B` o una conversión de referencia implícita de 0 a 1.</span><span class="sxs-lookup"><span data-stu-id="84145-268">If `S` also depends on type parameter `U` and `U` has a *class_type* constraint `A` and `T` has a *class_type* constraint `B` then there must be an identity conversion or implicit reference conversion from `A` to `B` or an implicit reference conversion from `B` to `A`.</span></span>

<span data-ttu-id="84145-269">Es válido para `S` que tenga la restricción de tipo de valor `T` y para que tenga la restricción de tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="84145-269">It is valid for `S` to have the value type constraint and `T` to have the reference type constraint.</span></span> <span data-ttu-id="84145-270">De hecho, `T` esto limita a `System.Object`los `System.ValueType`tipos `System.Enum`,, y a cualquier tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="84145-270">Effectively this limits `T` to the types `System.Object`, `System.ValueType`, `System.Enum`, and any interface type.</span></span>

<span data-ttu-id="84145-271">Si la `where` cláusula de un parámetro de tipo incluye una restricción de constructor (que tiene `new()`el formato), es posible utilizar el `new` operador para crear instancias del tipo ([expresiones de creación de objetos](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="84145-271">If the `where` clause for a type parameter includes a constructor constraint (which has the form `new()`), it is possible to use the `new` operator to create instances of the type ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span> <span data-ttu-id="84145-272">Cualquier argumento de tipo utilizado para un parámetro de tipo con una restricción de constructor debe tener un constructor sin parámetros público (este constructor existe implícitamente para cualquier tipo de valor) o ser un parámetro de tipo con la restricción de tipo de valor o la restricción de constructor (vea [Restricciones de parámetro de tipo](classes.md#type-parameter-constraints) para obtener más información).</span><span class="sxs-lookup"><span data-stu-id="84145-272">Any type argument used for a type parameter with a constructor constraint must have a public parameterless constructor (this constructor implicitly exists for any value type) or be a type parameter having the value type constraint or constructor constraint (see [Type parameter constraints](classes.md#type-parameter-constraints) for details).</span></span>

<span data-ttu-id="84145-273">A continuación se muestran ejemplos de restricciones:</span><span class="sxs-lookup"><span data-stu-id="84145-273">The following are examples of constraints:</span></span>
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

<span data-ttu-id="84145-274">El ejemplo siguiente es erróneo porque provoca una circularidad en el gráfico de dependencias de los parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="84145-274">The following example is in error because it causes a circularity in the dependency graph of the type parameters:</span></span>
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

<span data-ttu-id="84145-275">En los siguientes ejemplos se muestran situaciones no válidas adicionales:</span><span class="sxs-lookup"><span data-stu-id="84145-275">The following examples illustrate additional invalid situations:</span></span>
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

<span data-ttu-id="84145-276">La ***clase base efectiva*** de un parámetro `T` de tipo se define de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="84145-276">The ***effective base class*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="84145-277">Si `T` no tiene restricciones PRIMARY o restricciones de parámetro de tipo, su clase base efectiva es `object`.</span><span class="sxs-lookup"><span data-stu-id="84145-277">If `T` has no primary constraints or type parameter constraints, its effective base class is `object`.</span></span>
*  <span data-ttu-id="84145-278">Si `T` tiene la restricción de tipo de valor, su clase base `System.ValueType`efectiva es.</span><span class="sxs-lookup"><span data-stu-id="84145-278">If `T` has the value type constraint, its effective base class is `System.ValueType`.</span></span>
*  <span data-ttu-id="84145-279">Si `T` tiene una restricción *class_type* `C` pero no hay restricciones *type_parameter* , su clase base efectiva es `C`.</span><span class="sxs-lookup"><span data-stu-id="84145-279">If `T` has a *class_type* constraint `C` but no *type_parameter* constraints, its effective base class is `C`.</span></span>
*  <span data-ttu-id="84145-280">Si `T` no tiene ninguna restricción *class_type* pero tiene una o más restricciones *type_parameter* , su clase base efectiva es el tipo más abarcado (operadores de[conversión elevada](conversions.md#lifted-conversion-operators)) en el conjunto de clases base eficaces de su *tipo* restricciones de parámetros.</span><span class="sxs-lookup"><span data-stu-id="84145-280">If `T` has no *class_type* constraint but has one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set of effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="84145-281">Las reglas de coherencia garantizan que exista un tipo más abarcado.</span><span class="sxs-lookup"><span data-stu-id="84145-281">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="84145-282">Si `T` tiene una restricción *class_type* y una o más restricciones *type_parameter* , su clase base efectiva es el tipo más abarcado ([operadores de conversión levantada](conversions.md#lifted-conversion-operators)) del conjunto que consta de *class_type* restricción de `T` y las clases base eficaces de sus restricciones *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="84145-282">If `T` has both a *class_type* constraint and one or more *type_parameter* constraints, its effective base class is the most encompassed type ([Lifted conversion operators](conversions.md#lifted-conversion-operators)) in the set consisting of the *class_type* constraint of `T` and the effective base classes of its *type_parameter* constraints.</span></span> <span data-ttu-id="84145-283">Las reglas de coherencia garantizan que exista un tipo más abarcado.</span><span class="sxs-lookup"><span data-stu-id="84145-283">The consistency rules ensure that such a most encompassed type exists.</span></span>
*  <span data-ttu-id="84145-284">Si `T` tiene la restricción de tipo de referencia pero no tiene restricciones *class_type* , su clase base efectiva es `object`.</span><span class="sxs-lookup"><span data-stu-id="84145-284">If `T` has the reference type constraint but no *class_type* constraints, its effective base class is `object`.</span></span>

<span data-ttu-id="84145-285">En el caso de estas reglas, si T tiene una restricción `V` que es un *value_type*, use en su lugar el tipo base más específico de `V` que es un *class_type*.</span><span class="sxs-lookup"><span data-stu-id="84145-285">For the purpose of these rules, if T has a constraint `V` that is a *value_type*, use instead the most specific base type of `V` that is a *class_type*.</span></span> <span data-ttu-id="84145-286">Esto nunca puede ocurrir en una restricción explícitamente especificada, pero puede producirse cuando las restricciones de un método genérico se heredan implícitamente mediante una declaración de método de reemplazo o una implementación explícita de un método de interfaz.</span><span class="sxs-lookup"><span data-stu-id="84145-286">This can never happen in an explicitly given constraint, but may occur when the constraints of a generic method are implicitly inherited by an overriding method declaration or an explicit implementation of an interface method.</span></span>

<span data-ttu-id="84145-287">Estas reglas garantizan que la clase base vigente sea siempre *class_type*.</span><span class="sxs-lookup"><span data-stu-id="84145-287">These rules ensure that the effective base class is always a *class_type*.</span></span>

<span data-ttu-id="84145-288">El ***conjunto de interfaces efectivo*** de un parámetro `T` de tipo se define de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="84145-288">The ***effective interface set*** of a type parameter `T` is defined as follows:</span></span>

*  <span data-ttu-id="84145-289">Si `T` no tiene *secondary_constraints*, su conjunto de interfaces efectivo está vacío.</span><span class="sxs-lookup"><span data-stu-id="84145-289">If `T` has no *secondary_constraints*, its effective interface set is empty.</span></span>
*  <span data-ttu-id="84145-290">Si `T` tiene restricciones *interface_type* pero no *type_parameter* restricciones, su conjunto de interfaces efectivo es su conjunto de restricciones *interface_type* .</span><span class="sxs-lookup"><span data-stu-id="84145-290">If `T` has *interface_type* constraints but no *type_parameter* constraints, its effective interface set is its set of *interface_type* constraints.</span></span>
*  <span data-ttu-id="84145-291">Si `T` no tiene restricciones *interface_type* pero tiene restricciones *type_parameter* , su conjunto de interfaces efectivo es la Unión de los conjuntos de interfaces eficaces de sus restricciones *type_parameter* .</span><span class="sxs-lookup"><span data-stu-id="84145-291">If `T` has no *interface_type* constraints but has *type_parameter* constraints, its effective interface set is the union of the effective interface sets of its *type_parameter* constraints.</span></span>
*  <span data-ttu-id="84145-292">Si `T` tiene restricciones *interface_type* y *type_parameter* , su conjunto de interfaces efectivo es la Unión de su conjunto de restricciones *interface_type* y los conjuntos de interfaces efectivos de su *type_parameter* restricciones.</span><span class="sxs-lookup"><span data-stu-id="84145-292">If `T` has both *interface_type* constraints and *type_parameter* constraints, its effective interface set is the union of its set of *interface_type* constraints and the effective interface sets of its *type_parameter* constraints.</span></span>

<span data-ttu-id="84145-293">Se sabe que un parámetro de tipo es ***un tipo de referencia*** si tiene la restricción de tipo de referencia o su clase base `object` efectiva `System.ValueType`no es o.</span><span class="sxs-lookup"><span data-stu-id="84145-293">A type parameter is ***known to be a reference type*** if it has the reference type constraint or its effective base class is not `object` or `System.ValueType`.</span></span>

<span data-ttu-id="84145-294">Los valores de un tipo de parámetro de tipo restringido se pueden utilizar para tener acceso a los miembros de instancia que implican las restricciones.</span><span class="sxs-lookup"><span data-stu-id="84145-294">Values of a constrained type parameter type can be used to access the instance members implied by the constraints.</span></span> <span data-ttu-id="84145-295">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-295">In the example</span></span>
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
<span data-ttu-id="84145-296">los métodos de `IPrintable` se pueden invocar directamente en `x` porque `T` está restringido a implementar `IPrintable`siempre.</span><span class="sxs-lookup"><span data-stu-id="84145-296">the methods of `IPrintable` can be invoked directly on `x` because `T` is constrained to always implement `IPrintable`.</span></span>

### <a name="class-body"></a><span data-ttu-id="84145-297">Cuerpo de clase</span><span class="sxs-lookup"><span data-stu-id="84145-297">Class body</span></span>

<span data-ttu-id="84145-298">El *class_body* de una clase define los miembros de esa clase.</span><span class="sxs-lookup"><span data-stu-id="84145-298">The *class_body* of a class defines the members of that class.</span></span>

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a><span data-ttu-id="84145-299">Tipos parciales</span><span class="sxs-lookup"><span data-stu-id="84145-299">Partial types</span></span>

<span data-ttu-id="84145-300">Una declaración de tipos se puede dividir en varias ***declaraciones de tipos parciales***.</span><span class="sxs-lookup"><span data-stu-id="84145-300">A type declaration can be split across multiple ***partial type declarations***.</span></span> <span data-ttu-id="84145-301">La declaración de tipos se construye a partir de sus elementos siguiendo las reglas de esta sección, en la que se trata como una única declaración durante el resto del procesamiento en tiempo de compilación y en tiempo de ejecución del programa.</span><span class="sxs-lookup"><span data-stu-id="84145-301">The type declaration is constructed from its parts by following the rules in this section, whereupon it is treated as a single declaration during the remainder of the compile-time and run-time processing of the program.</span></span>

<span data-ttu-id="84145-302">*Class_declaration*, *struct_declaration* o *interface_declaration* representa una declaración de tipos parciales si incluye un modificador `partial`.</span><span class="sxs-lookup"><span data-stu-id="84145-302">A *class_declaration*, *struct_declaration* or *interface_declaration* represents a partial type declaration if it includes a `partial` modifier.</span></span> <span data-ttu-id="84145-303">`partial`no es una palabra clave y solo actúa como modificador si aparece inmediatamente antes de una de las palabras clave `class`, `struct` o `interface` en una declaración de tipo o antes del tipo `void` en una declaración de método.</span><span class="sxs-lookup"><span data-stu-id="84145-303">`partial` is not a keyword, and only acts as a modifier if it appears immediately before one of the keywords `class`, `struct` or `interface` in a type declaration, or before the type `void` in a method declaration.</span></span> <span data-ttu-id="84145-304">En otros contextos, se puede usar como un identificador normal.</span><span class="sxs-lookup"><span data-stu-id="84145-304">In other contexts it can be used as a normal identifier.</span></span>

<span data-ttu-id="84145-305">Cada parte de una declaración de tipo parcial debe incluir `partial` un modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-305">Each part of a partial type declaration must include a `partial` modifier.</span></span> <span data-ttu-id="84145-306">Debe tener el mismo nombre y estar declarado en el mismo espacio de nombres o declaración de tipos que las demás partes.</span><span class="sxs-lookup"><span data-stu-id="84145-306">It must have the same name  and be declared in the same namespace or type declaration as the other parts.</span></span> <span data-ttu-id="84145-307">El `partial` modificador indica que pueden existir partes adicionales de la declaración de tipos en otro lugar, pero la existencia de tales partes adicionales no es un requisito; es válido para un tipo con una sola declaración que `partial` incluya el modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-307">The `partial` modifier indicates that additional parts of the type declaration may exist elsewhere, but the existence of such additional parts is not a requirement; it is valid for a type with a single declaration to include the `partial` modifier.</span></span>

<span data-ttu-id="84145-308">Todas las partes de un tipo parcial se deben compilar de forma que se puedan combinar las partes en tiempo de compilación en una única declaración de tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-308">All parts of a partial type must be compiled together such that the parts can be merged at compile-time into a single type declaration.</span></span> <span data-ttu-id="84145-309">Los tipos parciales no permiten que se extiendan los tipos ya compilados.</span><span class="sxs-lookup"><span data-stu-id="84145-309">Partial types specifically do not allow already compiled types to be extended.</span></span>

<span data-ttu-id="84145-310">Los tipos anidados se pueden declarar en varias partes mediante el `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-310">Nested types may be declared in multiple parts by using the `partial` modifier.</span></span> <span data-ttu-id="84145-311">Normalmente, el tipo contenedor se declara utilizando `partial` también y cada parte del tipo anidado se declara en una parte diferente del tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="84145-311">Typically, the containing type is declared using `partial` as well, and each part of the nested type is declared in a different part of the containing type.</span></span>

<span data-ttu-id="84145-312">No `partial` se permite el modificador en las declaraciones de delegado o enumeración.</span><span class="sxs-lookup"><span data-stu-id="84145-312">The `partial` modifier is not permitted on delegate or enum declarations.</span></span>

### <a name="attributes"></a><span data-ttu-id="84145-313">Atributos</span><span class="sxs-lookup"><span data-stu-id="84145-313">Attributes</span></span>

<span data-ttu-id="84145-314">Los atributos de un tipo parcial se determinan mediante la combinación, en un orden no especificado, con los atributos de cada uno de los elementos.</span><span class="sxs-lookup"><span data-stu-id="84145-314">The attributes of a partial type are determined by combining, in an unspecified order, the attributes of each of the parts.</span></span> <span data-ttu-id="84145-315">Si un atributo se coloca en varias partes, es equivalente a especificar el atributo varias veces en el tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-315">If an attribute is placed on multiple parts, it is equivalent to specifying the attribute multiple times on the type.</span></span> <span data-ttu-id="84145-316">Por ejemplo, las dos partes:</span><span class="sxs-lookup"><span data-stu-id="84145-316">For example, the two parts:</span></span>

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
<span data-ttu-id="84145-317">son equivalentes a una declaración como:</span><span class="sxs-lookup"><span data-stu-id="84145-317">are equivalent to a declaration such as:</span></span>
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

<span data-ttu-id="84145-318">Los atributos de los parámetros de tipo se combinan de manera similar.</span><span class="sxs-lookup"><span data-stu-id="84145-318">Attributes on type parameters combine in a similar fashion.</span></span>

### <a name="modifiers"></a><span data-ttu-id="84145-319">Modificadores</span><span class="sxs-lookup"><span data-stu-id="84145-319">Modifiers</span></span>

<span data-ttu-id="84145-320">Cuando una declaración de tipos parciales incluye una especificación de accesibilidad `public`( `protected`los `internal`modificadores `private` ,, y), debe coincidir con todas las demás partes que incluyen una especificación de accesibilidad.</span><span class="sxs-lookup"><span data-stu-id="84145-320">When a partial type declaration includes an accessibility specification (the `public`, `protected`, `internal`, and `private` modifiers) it must agree with all other parts that include an accessibility specification.</span></span> <span data-ttu-id="84145-321">Si ninguna parte de un tipo parcial incluye una especificación de accesibilidad, se proporciona al tipo la accesibilidad predeterminada adecuada (la[accesibilidad declarada](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="84145-321">If no part of a partial type includes an accessibility specification, the type is given the appropriate default accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

<span data-ttu-id="84145-322">Si una o varias declaraciones parciales de un tipo anidado incluyen un `new` modificador, no se genera ninguna advertencia si el tipo anidado oculta un miembro heredado ([ocultando a través](basic-concepts.md#hiding-through-inheritance)de la herencia).</span><span class="sxs-lookup"><span data-stu-id="84145-322">If one or more partial declarations of a nested type include a `new` modifier, no warning is reported if the nested type hides an inherited member ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

<span data-ttu-id="84145-323">Si una o varias declaraciones parciales de una clase incluyen un `abstract` modificador, la clase se considera abstracta ([clases abstractas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="84145-323">If one or more partial declarations of a class include an `abstract` modifier, the class is considered abstract ([Abstract classes](classes.md#abstract-classes)).</span></span> <span data-ttu-id="84145-324">De lo contrario, la clase se considera no abstracta.</span><span class="sxs-lookup"><span data-stu-id="84145-324">Otherwise, the class is considered non-abstract.</span></span>

<span data-ttu-id="84145-325">Si una o varias declaraciones parciales de una clase incluyen un `sealed` modificador, la clase se considera Sealed ([clases selladas](classes.md#sealed-classes)).</span><span class="sxs-lookup"><span data-stu-id="84145-325">If one or more partial declarations of a class include a `sealed` modifier, the class is considered sealed ([Sealed classes](classes.md#sealed-classes)).</span></span> <span data-ttu-id="84145-326">De lo contrario, se considera que la clase no está sellada.</span><span class="sxs-lookup"><span data-stu-id="84145-326">Otherwise, the class is considered unsealed.</span></span>

<span data-ttu-id="84145-327">Tenga en cuenta que una clase no puede ser Abstract y Sealed.</span><span class="sxs-lookup"><span data-stu-id="84145-327">Note that a class cannot be both abstract and sealed.</span></span>

<span data-ttu-id="84145-328">Cuando el `unsafe` modificador se usa en una declaración de tipo parcial, solo esa parte concreta se considera un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="84145-328">When the `unsafe` modifier is used on a partial type declaration, only that particular part is considered an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span>

### <a name="type-parameters-and-constraints"></a><span data-ttu-id="84145-329">Parámetros de tipo y restricciones</span><span class="sxs-lookup"><span data-stu-id="84145-329">Type parameters and constraints</span></span>

<span data-ttu-id="84145-330">Si un tipo genérico se declara en varias partes, cada elemento debe indicar los parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-330">If a generic type is declared in multiple parts, each part must state the type parameters.</span></span> <span data-ttu-id="84145-331">Cada elemento debe tener el mismo número de parámetros de tipo y el mismo nombre para cada parámetro de tipo, en orden.</span><span class="sxs-lookup"><span data-stu-id="84145-331">Each part must have the same number of type parameters, and the same name for each type parameter, in order.</span></span>

<span data-ttu-id="84145-332">Cuando una declaración de tipos genéricos parciales incluye restricciones (`where` cláusulas), las restricciones deben coincidir con todas las demás partes que incluyen restricciones.</span><span class="sxs-lookup"><span data-stu-id="84145-332">When a partial generic type declaration includes constraints (`where` clauses), the constraints must agree with all other parts that include constraints.</span></span> <span data-ttu-id="84145-333">En concreto, cada parte que incluye restricciones debe tener restricciones para el mismo conjunto de parámetros de tipo y, para cada parámetro de tipo, los conjuntos de restricciones PRIMARY, Secondary y constructor deben ser equivalentes.</span><span class="sxs-lookup"><span data-stu-id="84145-333">Specifically, each part that includes constraints must have constraints for the same set of type parameters, and for each type parameter the sets of primary, secondary, and constructor constraints must be equivalent.</span></span> <span data-ttu-id="84145-334">Dos conjuntos de restricciones son equivalentes si contienen los mismos miembros.</span><span class="sxs-lookup"><span data-stu-id="84145-334">Two sets of constraints are equivalent if they contain the same members.</span></span> <span data-ttu-id="84145-335">Si ninguna parte de un tipo genérico parcial especifica las restricciones de parámetro de tipo, los parámetros de tipo se consideran no restringidos.</span><span class="sxs-lookup"><span data-stu-id="84145-335">If no part of a partial generic type specifies type parameter constraints, the type parameters are considered unconstrained.</span></span>

<span data-ttu-id="84145-336">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-336">The example</span></span>
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
<span data-ttu-id="84145-337">es correcto porque las partes que incluyen restricciones (las dos primeras) especifican de forma eficaz el mismo conjunto de restricciones principales, secundarias y constructores para el mismo conjunto de parámetros de tipo, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="84145-337">is correct because those parts that include constraints (the first two) effectively specify the same set of primary, secondary, and constructor constraints for the same set of type parameters, respectively.</span></span>

### <a name="base-class"></a><span data-ttu-id="84145-338">Clase base</span><span class="sxs-lookup"><span data-stu-id="84145-338">Base class</span></span>

<span data-ttu-id="84145-339">Cuando una declaración de clase parcial incluye una especificación de clase base, debe coincidir con todas las demás partes que incluyen una especificación de clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-339">When a partial class declaration includes a base class specification it must agree with all other parts that include a base class specification.</span></span> <span data-ttu-id="84145-340">Si ninguna parte de una clase parcial incluye una especificación de clase base, la clase base `System.Object` se convierte en ([clases base](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="84145-340">If no part of a partial class includes a base class specification, the base class becomes `System.Object` ([Base classes](classes.md#base-classes)).</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="84145-341">Interfaces base</span><span class="sxs-lookup"><span data-stu-id="84145-341">Base interfaces</span></span>

<span data-ttu-id="84145-342">El conjunto de interfaces base para un tipo declarado en varias partes es la Unión de las interfaces base especificadas en cada parte.</span><span class="sxs-lookup"><span data-stu-id="84145-342">The set of base interfaces for a type declared in multiple parts is the union of the base interfaces specified on each part.</span></span> <span data-ttu-id="84145-343">Solo se puede asignar un nombre a una interfaz base determinada una vez en cada parte, pero se permite que varias partes denominen a las mismas interfaces base.</span><span class="sxs-lookup"><span data-stu-id="84145-343">A particular base interface may only be named once on each part, but it is permitted for multiple parts to name the same base interface(s).</span></span> <span data-ttu-id="84145-344">Solo debe haber una implementación de los miembros de una interfaz base determinada.</span><span class="sxs-lookup"><span data-stu-id="84145-344">There must only be one implementation of the members of any given base interface.</span></span>

<span data-ttu-id="84145-345">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-345">In the example</span></span>
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
<span data-ttu-id="84145-346">el conjunto de interfaces base para la `C` clase `IA`es `IB`, y `IC`.</span><span class="sxs-lookup"><span data-stu-id="84145-346">the set of base interfaces for class `C` is `IA`, `IB`, and `IC`.</span></span>

<span data-ttu-id="84145-347">Normalmente, cada elemento proporciona una implementación de las interfaces declaradas en esa parte; sin embargo, esto no es un requisito.</span><span class="sxs-lookup"><span data-stu-id="84145-347">Typically, each part provides an implementation of the interface(s) declared on that part; however, this is not a requirement.</span></span> <span data-ttu-id="84145-348">Un elemento puede proporcionar la implementación de una interfaz declarada en una parte diferente:</span><span class="sxs-lookup"><span data-stu-id="84145-348">A part may provide the implementation for an interface declared on a different part:</span></span>
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

### <a name="members"></a><span data-ttu-id="84145-349">Miembros</span><span class="sxs-lookup"><span data-stu-id="84145-349">Members</span></span>

<span data-ttu-id="84145-350">A excepción de los métodos parciales ([métodos parciales](classes.md#partial-methods)), el conjunto de miembros de un tipo declarado en varias partes es simplemente la Unión del conjunto de miembros declarado en cada parte.</span><span class="sxs-lookup"><span data-stu-id="84145-350">With the exception of partial methods ([Partial methods](classes.md#partial-methods)), the set of members of a type declared in multiple parts is simply the union of the set of members declared in each part.</span></span> <span data-ttu-id="84145-351">Los cuerpos de todas las partes de la declaración de tipos comparten el mismo espacio de declaración ([declaraciones](basic-concepts.md#declarations)), y el ámbito de cada miembro ([ámbitos](basic-concepts.md#scopes)) se extiende a los cuerpos de todas las partes.</span><span class="sxs-lookup"><span data-stu-id="84145-351">The bodies of all parts of the type declaration share the same declaration space ([Declarations](basic-concepts.md#declarations)), and the scope of each member ([Scopes](basic-concepts.md#scopes)) extends to the bodies of all the parts.</span></span> <span data-ttu-id="84145-352">El dominio de accesibilidad de cualquier miembro siempre incluye todas las partes del tipo envolvente; un `private` miembro declarado en una parte está disponible libremente desde otra parte.</span><span class="sxs-lookup"><span data-stu-id="84145-352">The accessibility domain of any member always includes all the parts of the enclosing type; a `private` member declared in one part is freely accessible from another part.</span></span> <span data-ttu-id="84145-353">Es un error en tiempo de compilación declarar el mismo miembro en más de una parte del tipo, a menos que ese miembro sea un tipo con el `partial` modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-353">It is a compile-time error to declare the same member in more than one part of the type, unless that member is a type with the `partial` modifier.</span></span>

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

<span data-ttu-id="84145-354">El orden de los miembros dentro de un tipo rara vez C# es significativo para el código, pero puede ser importante al interactuar con otros lenguajes y entornos.</span><span class="sxs-lookup"><span data-stu-id="84145-354">The ordering of members within a type is rarely significant to C# code, but may be significant when interfacing with other languages and environments.</span></span> <span data-ttu-id="84145-355">En estos casos, el orden de los miembros dentro de un tipo declarado en varias partes es indefinido.</span><span class="sxs-lookup"><span data-stu-id="84145-355">In these cases, the ordering of members within a type declared in multiple parts is undefined.</span></span>

### <a name="partial-methods"></a><span data-ttu-id="84145-356">Métodos parciales</span><span class="sxs-lookup"><span data-stu-id="84145-356">Partial methods</span></span>

<span data-ttu-id="84145-357">Los métodos parciales se pueden definir en una parte de una declaración de tipos e implementarse en otro.</span><span class="sxs-lookup"><span data-stu-id="84145-357">Partial methods can be defined in one part of a type declaration and implemented in another.</span></span> <span data-ttu-id="84145-358">La implementación es opcional. Si ninguna parte implementa el método parcial, la declaración de método parcial y todas las llamadas a ella se quitan de la declaración de tipos resultante de la combinación de los elementos.</span><span class="sxs-lookup"><span data-stu-id="84145-358">The implementation is optional; if no part implements the partial method, the partial method declaration and all calls to it are removed from the type declaration resulting from the combination of the parts.</span></span>

<span data-ttu-id="84145-359">Los métodos parciales no pueden definir modificadores de acceso, pero `private`son implícitamente.</span><span class="sxs-lookup"><span data-stu-id="84145-359">Partial methods cannot define access modifiers, but are implicitly `private`.</span></span> <span data-ttu-id="84145-360">Su tipo de valor devuelto debe ser `void`, y sus parámetros no pueden tener el `out` modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-360">Their return type must be `void`, and their parameters cannot have the `out` modifier.</span></span> <span data-ttu-id="84145-361">El identificador `partial` se reconoce como una palabra clave especial en una declaración de método solo si aparece justo antes del `void` tipo; en caso contrario, se puede usar como un identificador normal.</span><span class="sxs-lookup"><span data-stu-id="84145-361">The identifier `partial` is recognized as a special keyword in a method declaration only if it appears right before the `void` type; otherwise it can be used as a normal identifier.</span></span> <span data-ttu-id="84145-362">Un método parcial no puede implementar explícitamente métodos de interfaz.</span><span class="sxs-lookup"><span data-stu-id="84145-362">A partial method cannot explicitly implement interface methods.</span></span>

<span data-ttu-id="84145-363">Hay dos tipos de declaraciones de método parcial: Si el cuerpo de la declaración del método es un punto y coma, se dice que la declaración es una ***declaración de método parcial de definición***.</span><span class="sxs-lookup"><span data-stu-id="84145-363">There are two kinds of partial method declarations: If the body of the method declaration is a semicolon, the declaration is said to be a ***defining partial method declaration***.</span></span> <span data-ttu-id="84145-364">Si el cuerpo se proporciona como un *bloque*, se dice que la declaración es una ***declaración de método parcial de implementación***.</span><span class="sxs-lookup"><span data-stu-id="84145-364">If the body is given as a *block*, the declaration is said to be an ***implementing partial method declaration***.</span></span> <span data-ttu-id="84145-365">En las partes de una declaración de tipos solo puede haber una declaración de método parcial de definición con una firma determinada, y solo puede haber una declaración de método parcial de implementación con una firma determinada.</span><span class="sxs-lookup"><span data-stu-id="84145-365">Across the parts of a type declaration there can be only one defining partial method declaration with a given signature, and there can be only one implementing partial method declaration with a given signature.</span></span> <span data-ttu-id="84145-366">Si se proporciona una declaración de método parcial de implementación, debe existir una declaración de método parcial de definición correspondiente, y las declaraciones deben coincidir como se especifica en lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="84145-366">If an implementing partial method declaration is given, a corresponding defining partial method declaration must exist, and the declarations must match as specified in the following:</span></span>

* <span data-ttu-id="84145-367">Las declaraciones deben tener los mismos modificadores (aunque no necesariamente en el mismo orden), el nombre del método, el número de parámetros de tipo y el número de parámetros.</span><span class="sxs-lookup"><span data-stu-id="84145-367">The declarations must have the same modifiers (although not necessarily in the same order), method name, number of type parameters and number of parameters.</span></span>
* <span data-ttu-id="84145-368">Los parámetros correspondientes en las declaraciones deben tener los mismos modificadores (aunque no necesariamente en el mismo orden) y los mismos tipos (diferencias de módulo en los nombres de parámetros de tipo).</span><span class="sxs-lookup"><span data-stu-id="84145-368">Corresponding parameters in the declarations must have the same modifiers (although not necessarily in the same order) and the same types (modulo differences in type parameter names).</span></span>
* <span data-ttu-id="84145-369">Los parámetros de tipo correspondientes en las declaraciones deben tener las mismas restricciones (diferencias de módulo en los nombres de parámetros de tipo).</span><span class="sxs-lookup"><span data-stu-id="84145-369">Corresponding type parameters in the declarations must have the same constraints (modulo differences in type parameter names).</span></span>

<span data-ttu-id="84145-370">Una declaración de método parcial de implementación puede aparecer en la misma parte que la declaración de método parcial de definición correspondiente.</span><span class="sxs-lookup"><span data-stu-id="84145-370">An implementing partial method declaration can appear in the same part as the corresponding defining partial method declaration.</span></span>

<span data-ttu-id="84145-371">Solo un método parcial de definición participa en la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="84145-371">Only a defining partial method participates in overload resolution.</span></span> <span data-ttu-id="84145-372">Por lo tanto, tanto si se proporciona una declaración de implementación como si no, las expresiones de invocación pueden resolverse en invocaciones del método parcial.</span><span class="sxs-lookup"><span data-stu-id="84145-372">Thus, whether or not an implementing declaration is given, invocation expressions may resolve to invocations of the partial method.</span></span> <span data-ttu-id="84145-373">Dado que un método parcial siempre `void`devuelve, estas expresiones de invocación siempre serán instrucciones de expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-373">Because a partial method always returns `void`, such invocation expressions will always be expression statements.</span></span> <span data-ttu-id="84145-374">Además, dado que un método parcial es implícitamente `private`, estas instrucciones siempre se producirán dentro de una de las partes de la declaración de tipos en la que se declara el método parcial.</span><span class="sxs-lookup"><span data-stu-id="84145-374">Furthermore, because a partial method is implicitly `private`, such statements will always occur within one of the parts of the type declaration within which the partial method is declared.</span></span>

<span data-ttu-id="84145-375">Si ninguna parte de una declaración de tipo parcial contiene una declaración de implementación para un método parcial determinado, cualquier instrucción de expresión que lo invoque simplemente se quita de la declaración de tipos combinados.</span><span class="sxs-lookup"><span data-stu-id="84145-375">If no part of a partial type declaration contains an implementing declaration for a given partial method, any expression statement invoking it is simply removed from the combined type declaration.</span></span> <span data-ttu-id="84145-376">Por lo tanto, la expresión de invocación, incluidas las expresiones constituyentes, no tiene ningún efecto en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="84145-376">Thus the invocation expression, including any constituent expressions, has no effect at run-time.</span></span> <span data-ttu-id="84145-377">También se quita el método parcial en sí y no será miembro de la declaración de tipos combinados.</span><span class="sxs-lookup"><span data-stu-id="84145-377">The partial method itself is also removed and will not be a member of the combined type declaration.</span></span>

<span data-ttu-id="84145-378">Si existe una declaración de implementación para un método parcial determinado, se conservan las invocaciones de los métodos parciales.</span><span class="sxs-lookup"><span data-stu-id="84145-378">If an implementing declaration exist for a given partial method, the invocations of the partial methods are retained.</span></span> <span data-ttu-id="84145-379">El método parcial da lugar a una declaración de método similar a la declaración de método parcial de implementación, excepto para lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="84145-379">The partial method gives rise to a method declaration similar to the implementing partial method declaration except for the following:</span></span>

* <span data-ttu-id="84145-380">El `partial` modificador no está incluido</span><span class="sxs-lookup"><span data-stu-id="84145-380">The `partial` modifier is not included</span></span>
* <span data-ttu-id="84145-381">Los atributos de la declaración del método resultante son los atributos combinados de la definición de y la declaración de método parcial de implementación en un orden no especificado.</span><span class="sxs-lookup"><span data-stu-id="84145-381">The attributes in the resulting method declaration are the combined attributes of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="84145-382">No se quitan los duplicados.</span><span class="sxs-lookup"><span data-stu-id="84145-382">Duplicates are not removed.</span></span>
* <span data-ttu-id="84145-383">Los atributos de los parámetros de la declaración de método resultante son los atributos combinados de los parámetros correspondientes de la definición de y la declaración de método parcial de implementación en un orden no especificado.</span><span class="sxs-lookup"><span data-stu-id="84145-383">The attributes on the parameters of the resulting method declaration are the combined attributes of the corresponding parameters of the defining and the implementing partial method declaration in unspecified order.</span></span> <span data-ttu-id="84145-384">No se quitan los duplicados.</span><span class="sxs-lookup"><span data-stu-id="84145-384">Duplicates are not removed.</span></span>

<span data-ttu-id="84145-385">Si se proporciona una declaración de definición pero no una declaración de implementación para un método parcial M, se aplican las restricciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="84145-385">If a defining declaration but not an implementing declaration is given for a partial method M, the following restrictions apply:</span></span>

* <span data-ttu-id="84145-386">Es un error en tiempo de compilación crear un delegado para el método ([expresiones de creación de delegado](expressions.md#delegate-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="84145-386">It is a compile-time error to create a delegate to method ([Delegate creation expressions](expressions.md#delegate-creation-expressions)).</span></span>
* <span data-ttu-id="84145-387">Es un error en tiempo de compilación hacer referencia a `M` dentro de una función anónima que se convierte en un tipo de árbol[de expresión (evaluación de conversiones de funciones anónimas a tipos de árbol de expresión](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="84145-387">It is a compile-time error to refer to `M` inside an anonymous function that is converted to an expression tree type ([Evaluation of anonymous function conversions to expression tree types](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).</span></span>
* <span data-ttu-id="84145-388">Las expresiones que se producen como parte de una `M` invocación de no afectan al estado de asignación definitiva ([asignación definitiva](variables.md#definite-assignment)), lo que puede provocar errores en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-388">Expressions occurring as part of an invocation of `M` do not affect the definite assignment state ([Definite assignment](variables.md#definite-assignment)), which can potentially lead to compile-time errors.</span></span>
* <span data-ttu-id="84145-389">`M`no puede ser el punto de entrada de una aplicación (inicio de la[aplicación](basic-concepts.md#application-startup)).</span><span class="sxs-lookup"><span data-stu-id="84145-389">`M` cannot be the entry point for an application ([Application Startup](basic-concepts.md#application-startup)).</span></span>

<span data-ttu-id="84145-390">Los métodos parciales son útiles para permitir que una parte de una declaración de tipos Personalice el comportamiento de otra parte, por ejemplo, una generada por una herramienta.</span><span class="sxs-lookup"><span data-stu-id="84145-390">Partial methods are useful for allowing one part of a type declaration to customize the behavior of another part, e.g., one that is generated by a tool.</span></span> <span data-ttu-id="84145-391">Considere la siguiente declaración de clase parcial:</span><span class="sxs-lookup"><span data-stu-id="84145-391">Consider the following partial class declaration:</span></span>
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

<span data-ttu-id="84145-392">Si esta clase se compila sin ningún otro elemento, se quitarán las declaraciones de método parcial de definición y sus invocaciones, y la declaración de clase combinada resultante será equivalente a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="84145-392">If this class is compiled without any other parts, the defining partial method declarations and their invocations will be removed, and the resulting combined class declaration will be equivalent to the following:</span></span>
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

<span data-ttu-id="84145-393">No obstante, supongamos que se proporciona otra parte, que proporciona declaraciones de implementación de los métodos parciales:</span><span class="sxs-lookup"><span data-stu-id="84145-393">Assume that another part is given, however, which provides implementing declarations of the partial methods:</span></span>
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

<span data-ttu-id="84145-394">A continuación, la declaración de clase combinada resultante será equivalente a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="84145-394">Then the resulting combined class declaration will be equivalent to the following:</span></span>
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

### <a name="name-binding"></a><span data-ttu-id="84145-395">Enlace de nombre</span><span class="sxs-lookup"><span data-stu-id="84145-395">Name binding</span></span>

<span data-ttu-id="84145-396">Aunque cada parte de un tipo extensible se debe declarar dentro del mismo espacio de nombres, las partes se escriben normalmente en diferentes declaraciones de espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="84145-396">Although each part of an extensible type must be declared within the same namespace, the parts are typically written within different namespace declarations.</span></span> <span data-ttu-id="84145-397">Por lo tanto `using` , pueden estar presentes directivas diferentes ([mediante directivas](namespaces.md#using-directives)) para cada parte.</span><span class="sxs-lookup"><span data-stu-id="84145-397">Thus, different `using` directives ([Using directives](namespaces.md#using-directives)) may be present for each part.</span></span> <span data-ttu-id="84145-398">Al interpretar nombres simples ([inferencia de tipos](expressions.md#type-inference)) dentro de una parte, solo `using` se tienen en cuenta las directivas de las declaraciones de espacio de nombres que la forman.</span><span class="sxs-lookup"><span data-stu-id="84145-398">When interpreting simple names ([Type inference](expressions.md#type-inference)) within one part, only the `using` directives of the namespace declaration(s) enclosing that part are considered.</span></span> <span data-ttu-id="84145-399">Esto puede dar lugar a que el mismo identificador tenga significados diferentes en distintas partes:</span><span class="sxs-lookup"><span data-stu-id="84145-399">This may result in the same identifier having different meanings in different parts:</span></span>
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

## <a name="class-members"></a><span data-ttu-id="84145-400">Miembros de la clase</span><span class="sxs-lookup"><span data-stu-id="84145-400">Class members</span></span>

<span data-ttu-id="84145-401">Los miembros de una clase se componen de los miembros introducidos por sus *class_member_declaration*s y los miembros heredados de la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="84145-401">The members of a class consist of the members introduced by its *class_member_declaration*s and the members inherited from the direct base class.</span></span>

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

<span data-ttu-id="84145-402">Los miembros de un tipo de clase se dividen en las siguientes categorías:</span><span class="sxs-lookup"><span data-stu-id="84145-402">The members of a class type are divided into the following categories:</span></span>

*  <span data-ttu-id="84145-403">Constantes, que representan valores constantes asociados a la clase ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="84145-403">Constants, which represent constant values associated with the class ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="84145-404">Campos, que son las variables de la clase ([campos](classes.md#fields)).</span><span class="sxs-lookup"><span data-stu-id="84145-404">Fields, which are the variables of the class ([Fields](classes.md#fields)).</span></span>
*  <span data-ttu-id="84145-405">Los métodos, que implementan los cálculos y las acciones que puede realizar la clase ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="84145-405">Methods, which implement the computations and actions that can be performed by the class ([Methods](classes.md#methods)).</span></span>
*  <span data-ttu-id="84145-406">Propiedades, que definen las características con nombre y las acciones asociadas a la lectura y escritura de esas características ([propiedades](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="84145-406">Properties, which define named characteristics and the actions associated with reading and writing those characteristics ([Properties](classes.md#properties)).</span></span>
*  <span data-ttu-id="84145-407">Eventos, que definen las notificaciones que puede generar la clase ([eventos](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="84145-407">Events, which define notifications that can be generated by the class ([Events](classes.md#events)).</span></span>
*  <span data-ttu-id="84145-408">Indexadores, que permiten indizar las instancias de la clase de la misma manera (sintácticamente) que las matrices ([indizadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="84145-408">Indexers, which permit instances of the class to be indexed in the same way (syntactically) as arrays ([Indexers](classes.md#indexers)).</span></span>
*  <span data-ttu-id="84145-409">Operadores, que definen los operadores de expresión que se pueden aplicar a las instancias de la clase ([operadores](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="84145-409">Operators, which define the expression operators that can be applied to instances of the class ([Operators](classes.md#operators)).</span></span>
*  <span data-ttu-id="84145-410">Constructores de instancias, que implementan las acciones necesarias para inicializar las instancias de la clase ([constructores de instancia](classes.md#instance-constructors))</span><span class="sxs-lookup"><span data-stu-id="84145-410">Instance constructors, which implement the actions required to initialize instances of the class ([Instance constructors](classes.md#instance-constructors))</span></span>
*  <span data-ttu-id="84145-411">Destructores, que implementan las acciones que se deben realizar antes de que las instancias de la clase se descartan de forma permanente ([destructores](classes.md#destructors)).</span><span class="sxs-lookup"><span data-stu-id="84145-411">Destructors, which implement the actions to be performed before instances of the class are permanently discarded ([Destructors](classes.md#destructors)).</span></span>
*  <span data-ttu-id="84145-412">Constructores estáticos, que implementan las acciones necesarias para inicializar la propia clase ([constructores estáticos](classes.md#static-constructors)).</span><span class="sxs-lookup"><span data-stu-id="84145-412">Static constructors, which implement the actions required to initialize the class itself ([Static constructors](classes.md#static-constructors)).</span></span>
*  <span data-ttu-id="84145-413">Tipos, que representan los tipos locales de la clase ([tipos anidados](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="84145-413">Types, which represent the types that are local to the class ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="84145-414">Los miembros que pueden contener código ejecutable se conocen colectivamente como *miembros de función* del tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-414">Members that can contain executable code are collectively known as the *function members* of the class type.</span></span> <span data-ttu-id="84145-415">Los miembros de función de un tipo de clase son los métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos de ese tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-415">The function members of a class type are the methods, properties, events, indexers, operators, instance constructors,  destructors, and static constructors of that class type.</span></span>

<span data-ttu-id="84145-416">Un *class_declaration* crea un nuevo espacio de declaración ([declaraciones](basic-concepts.md#declarations)) y los *class_member_declaration*de elementos que contiene inmediatamente *class_declaration* introducen nuevos miembros en este espacio de declaración.</span><span class="sxs-lookup"><span data-stu-id="84145-416">A *class_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *class_member_declaration*s immediately contained by the *class_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="84145-417">Las siguientes reglas se aplican a *class_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="84145-417">The following rules apply to *class_member_declaration*s:</span></span>

*  <span data-ttu-id="84145-418">Los constructores de instancias, destructores y constructores estáticos deben tener el mismo nombre que la clase de inclusión inmediata.</span><span class="sxs-lookup"><span data-stu-id="84145-418">Instance constructors, destructors and static constructors must have the same name as the immediately enclosing class.</span></span> <span data-ttu-id="84145-419">Todos los demás miembros deben tener nombres distintos del nombre de la clase de inclusión inmediata.</span><span class="sxs-lookup"><span data-stu-id="84145-419">All other members must have names that differ from the name of the immediately enclosing class.</span></span>
*  <span data-ttu-id="84145-420">El nombre de una constante, un campo, una propiedad, un evento o un tipo debe ser distinto de los nombres de todos los demás miembros declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="84145-420">The name of a constant, field, property, event, or type must differ from the names of all other members declared in the same class.</span></span>
*  <span data-ttu-id="84145-421">El nombre de un método debe ser distinto de los nombres de todos los demás métodos que no se declaran en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="84145-421">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="84145-422">Además, la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un método debe ser diferente de las firmas de todos los demás métodos declarados en la misma clase y dos métodos declarados en la misma clase no pueden tener firmas que solo difieran `ref` en y `out`.</span><span class="sxs-lookup"><span data-stu-id="84145-422">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="84145-423">La firma de un constructor de instancia debe ser diferente de las firmas de todos los demás constructores de instancia declaradas en la misma clase y dos constructores declarados en la misma clase no pueden tener `ref` firmas `out`que solo difieran en y.</span><span class="sxs-lookup"><span data-stu-id="84145-423">The signature of an instance constructor must differ from the signatures of all other instance constructors declared in the same class, and two constructors declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="84145-424">La firma de un indizador debe ser diferente de las firmas de todos los demás indexadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="84145-424">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>
*  <span data-ttu-id="84145-425">La firma de un operador debe ser diferente de las firmas de todos los demás operadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="84145-425">The signature of an operator must differ from the signatures of all other operators declared in the same class.</span></span>

<span data-ttu-id="84145-426">Los miembros heredados de un tipo de clase ([herencia](classes.md#inheritance)) no forman parte del espacio de declaración de una clase.</span><span class="sxs-lookup"><span data-stu-id="84145-426">The inherited members of a class type ([Inheritance](classes.md#inheritance)) are not part of the declaration space of a class.</span></span> <span data-ttu-id="84145-427">Por lo tanto, una clase derivada puede declarar un miembro con el mismo nombre o signatura que un miembro heredado (que en efecto oculta el miembro heredado).</span><span class="sxs-lookup"><span data-stu-id="84145-427">Thus, a derived class is allowed to declare a member with the same name or signature as an inherited member (which in effect hides the inherited member).</span></span>

### <a name="the-instance-type"></a><span data-ttu-id="84145-428">El tipo de instancia</span><span class="sxs-lookup"><span data-stu-id="84145-428">The instance type</span></span>

<span data-ttu-id="84145-429">Cada declaración de clase tiene un tipo enlazado asociado ([tipos enlazados y sin enlazar](types.md#bound-and-unbound-types)), el ***tipo de instancia***.</span><span class="sxs-lookup"><span data-stu-id="84145-429">Each class declaration has an associated bound type ([Bound and unbound types](types.md#bound-and-unbound-types)), the ***instance type***.</span></span> <span data-ttu-id="84145-430">En el caso de una declaración de clase genérica, el tipo de instancia se forma creando un tipo construido ([tipos construidos](types.md#constructed-types)) a partir de la declaración de tipos, y cada uno de los argumentos de tipo proporcionados es el parámetro de tipo correspondiente.</span><span class="sxs-lookup"><span data-stu-id="84145-430">For a generic class declaration, the instance type is formed by creating a constructed type ([Constructed types](types.md#constructed-types)) from the type declaration, with each of the supplied type arguments being the corresponding type parameter.</span></span> <span data-ttu-id="84145-431">Dado que el tipo de instancia utiliza los parámetros de tipo, solo se puede usar cuando los parámetros de tipo están en el ámbito; es decir, dentro de la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-431">Since the instance type uses the type parameters, it can only be used where the type parameters are in scope; that is, inside the class declaration.</span></span> <span data-ttu-id="84145-432">El tipo de instancia es el tipo `this` de para el código escrito dentro de la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-432">The instance type is the type of `this` for code written inside the class declaration.</span></span> <span data-ttu-id="84145-433">En el caso de las clases no genéricas, el tipo de instancia es simplemente la clase declarada.</span><span class="sxs-lookup"><span data-stu-id="84145-433">For non-generic classes, the instance type is simply the declared class.</span></span> <span data-ttu-id="84145-434">A continuación se muestran varias declaraciones de clase junto con sus tipos de instancia:</span><span class="sxs-lookup"><span data-stu-id="84145-434">The following shows several class declarations along with their instance types:</span></span> 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a><span data-ttu-id="84145-435">Miembros de tipos construidos</span><span class="sxs-lookup"><span data-stu-id="84145-435">Members of constructed types</span></span>

<span data-ttu-id="84145-436">Los miembros no heredados de un tipo construido se obtienen sustituyendo, por cada *type_parameter* en la declaración de miembro, el *type_argument* correspondiente del tipo construido.</span><span class="sxs-lookup"><span data-stu-id="84145-436">The non-inherited members of a constructed type are obtained by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the constructed type.</span></span> <span data-ttu-id="84145-437">El proceso de sustitución se basa en el significado semántico de las declaraciones de tipos y no es simplemente una sustitución textual.</span><span class="sxs-lookup"><span data-stu-id="84145-437">The substitution process is based on the semantic meaning of type declarations, and is not simply textual substitution.</span></span>

<span data-ttu-id="84145-438">Por ejemplo, dada la declaración de clase genérica</span><span class="sxs-lookup"><span data-stu-id="84145-438">For example, given the generic class declaration</span></span>
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
<span data-ttu-id="84145-439">el tipo `Gen<int[],IComparable<string>>` construido tiene los siguientes miembros:</span><span class="sxs-lookup"><span data-stu-id="84145-439">the constructed type `Gen<int[],IComparable<string>>` has the following members:</span></span>
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

<span data-ttu-id="84145-440">`a` El tipo del miembro en la declaración `Gen` de clase genérica es la "matriz bidimensional de `T`", por lo que el tipo del `a` miembro en el tipo construido anterior es "matriz bidimensional de matriz unidimensional de `int`"o `int[,][]`.</span><span class="sxs-lookup"><span data-stu-id="84145-440">The type of the member `a` in the generic class declaration `Gen` is "two-dimensional array of `T`", so the type of the member `a` in the constructed type above is "two-dimensional array of one-dimensional array of `int`", or `int[,][]`.</span></span>

<span data-ttu-id="84145-441">Dentro de los miembros de la función de `this` instancia, el tipo de es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la declaración contenedora.</span><span class="sxs-lookup"><span data-stu-id="84145-441">Within instance function members, the type of `this` is the instance type ([The instance type](classes.md#the-instance-type)) of the containing declaration.</span></span>

<span data-ttu-id="84145-442">Todos los miembros de una clase genérica pueden usar parámetros de tipo de cualquier clase envolvente, ya sea directamente o como parte de un tipo construido.</span><span class="sxs-lookup"><span data-stu-id="84145-442">All members of a generic class can use type parameters from any enclosing class, either directly or as part of a constructed type.</span></span> <span data-ttu-id="84145-443">Cuando se usa un tipo construido cerrado determinado ([tipos abiertos y cerrados](types.md#open-and-closed-types)) en tiempo de ejecución, cada uso de un parámetro de tipo se reemplaza por el argumento de tipo real proporcionado al tipo construido.</span><span class="sxs-lookup"><span data-stu-id="84145-443">When a particular closed constructed type ([Open and closed types](types.md#open-and-closed-types)) is used at run-time, each use of a type parameter is replaced with the actual type argument supplied to the constructed type.</span></span> <span data-ttu-id="84145-444">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-444">For example:</span></span>
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

### <a name="inheritance"></a><span data-ttu-id="84145-445">Herencia</span><span class="sxs-lookup"><span data-stu-id="84145-445">Inheritance</span></span>

<span data-ttu-id="84145-446">Una clase ***hereda*** los miembros de su tipo de clase base directa.</span><span class="sxs-lookup"><span data-stu-id="84145-446">A class ***inherits*** the members of its direct base class type.</span></span> <span data-ttu-id="84145-447">La herencia significa que una clase contiene implícitamente todos los miembros de su tipo de clase base directa, a excepción de los constructores de instancias, destructores y constructores estáticos de la clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-447">Inheritance means that a class implicitly contains all members of its direct base class type, except for the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="84145-448">Algunos aspectos importantes de la herencia son:</span><span class="sxs-lookup"><span data-stu-id="84145-448">Some important aspects of inheritance are:</span></span>

*  <span data-ttu-id="84145-449">La herencia es transitiva.</span><span class="sxs-lookup"><span data-stu-id="84145-449">Inheritance is transitive.</span></span> <span data-ttu-id="84145-450">Si `C` se deriva de `B`y `B` se deriva de `A`, `C` hereda los miembros declarados en `B` , así como los miembros declarados `A`en.</span><span class="sxs-lookup"><span data-stu-id="84145-450">If `C` is derived from `B`, and `B` is derived from `A`, then `C` inherits the members declared in `B` as well as the members declared in `A`.</span></span>
*  <span data-ttu-id="84145-451">Una clase derivada extiende su clase base directa.</span><span class="sxs-lookup"><span data-stu-id="84145-451">A derived class extends its direct base class.</span></span> <span data-ttu-id="84145-452">Una clase derivada puede agregar nuevos miembros a aquellos de los que hereda, pero no puede quitar la definición de un miembro heredado.</span><span class="sxs-lookup"><span data-stu-id="84145-452">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span>
*  <span data-ttu-id="84145-453">Los constructores de instancias, destructores y constructores estáticos no se heredan, pero todos los demás miembros son, independientemente de su accesibilidad declarada ([acceso a miembros](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="84145-453">Instance constructors, destructors, and static constructors are not inherited, but all other members are, regardless of their declared accessibility ([Member access](basic-concepts.md#member-access)).</span></span> <span data-ttu-id="84145-454">Sin embargo, en función de la accesibilidad declarada, es posible que los miembros heredados no estén accesibles en una clase derivada.</span><span class="sxs-lookup"><span data-stu-id="84145-454">However, depending on their declared accessibility, inherited members might not be accessible in a derived class.</span></span>
*  <span data-ttu-id="84145-455">Una clase derivada puede ***ocultar*** ([ocultar a través](basic-concepts.md#hiding-through-inheritance)de la herencia) los miembros heredados declarando nuevos miembros con el mismo nombre o signatura.</span><span class="sxs-lookup"><span data-stu-id="84145-455">A derived class can ***hide*** ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)) inherited members by declaring new members with the same name or signature.</span></span> <span data-ttu-id="84145-456">Sin embargo, tenga en cuenta que ocultar un miembro heredado no quita ese miembro; simplemente hace que ese miembro no sea accesible directamente a través de la clase derivada.</span><span class="sxs-lookup"><span data-stu-id="84145-456">Note however that hiding an inherited member does not remove that member—it merely makes that member inaccessible directly through the derived class.</span></span>
*  <span data-ttu-id="84145-457">Una instancia de una clase contiene un conjunto de todos los campos de instancia declarados en la clase y sus clases base, y existe una conversión implícita ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions)) de un tipo de clase derivada a cualquiera de sus tipos de clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-457">An instance of a class contains a set of all instance fields declared in the class and its base classes, and an implicit conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from a derived class type to any of its base class types.</span></span> <span data-ttu-id="84145-458">Por lo tanto, una referencia a una instancia de alguna clase derivada se puede tratar como una referencia a una instancia de cualquiera de sus clases base.</span><span class="sxs-lookup"><span data-stu-id="84145-458">Thus, a reference to an instance of some derived class can be treated as a reference to an instance of any of its base classes.</span></span>
*  <span data-ttu-id="84145-459">Una clase puede declarar métodos virtuales, propiedades e indizadores, y las clases derivadas pueden invalidar la implementación de estos miembros de función.</span><span class="sxs-lookup"><span data-stu-id="84145-459">A class can declare virtual methods, properties, and indexers, and derived classes can override the implementation of these function members.</span></span> <span data-ttu-id="84145-460">Esto permite que las clases muestren un comportamiento polimórfico en el que las acciones realizadas por una invocación de miembro de función varían en función del tipo en tiempo de ejecución de la instancia a través de la cual se invoca a ese miembro de función.</span><span class="sxs-lookup"><span data-stu-id="84145-460">This enables classes to exhibit polymorphic behavior wherein the actions performed by a function member invocation varies depending on the run-time type of the instance through which that function member is invoked.</span></span>

<span data-ttu-id="84145-461">El miembro heredado de un tipo de clase construido son los miembros del tipo de clase base inmediato ([clases base](classes.md#base-classes)), que se encuentra sustituyendo los argumentos de tipo del tipo construido por cada aparición de los parámetros de tipo correspondientes en elespecificación de class_base.</span><span class="sxs-lookup"><span data-stu-id="84145-461">The inherited member of a constructed class type are the members of the immediate base class type ([Base classes](classes.md#base-classes)), which is found by substituting the type arguments of the constructed type for each occurrence of the corresponding type parameters in the *class_base* specification.</span></span> <span data-ttu-id="84145-462">Estos miembros, a su vez, se transforman sustituyendo, por cada *type_parameter* en la declaración de miembro, el *type_argument* correspondiente de la especificación *class_base* .</span><span class="sxs-lookup"><span data-stu-id="84145-462">These members, in turn, are transformed by substituting, for each *type_parameter* in the member declaration, the corresponding *type_argument* of the *class_base* specification.</span></span>

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

<span data-ttu-id="84145-463">En el ejemplo anterior, el `D<int>` tipo construido tiene un miembro `public int G(string s)` no heredado que se obtiene sustituyendo el argumento `int` de tipo para el `T`parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-463">In the above example, the constructed type `D<int>` has a non-inherited member `public int G(string s)` obtained by substituting the type argument `int` for the type parameter `T`.</span></span> <span data-ttu-id="84145-464">`D<int>`también tiene un miembro heredado de la declaración `B`de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-464">`D<int>` also has an inherited member from the class declaration `B`.</span></span> <span data-ttu-id="84145-465">Este miembro heredado se determina determinando en primer lugar `B<int[]>` el `D<int>` tipo de clase base `T` de sustituyendo `int` por en `B<T[]>`la especificación de clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-465">This inherited member is determined by first determining the base class type `B<int[]>` of `D<int>` by substituting `int` for `T` in the base class specification `B<T[]>`.</span></span> <span data-ttu-id="84145-466">A continuación, como un argumento de `B`tipo `int[]` en, se sustituye `U` por `public U F(long index)`en, lo que produce el `public int[] F(long index)`miembro heredado.</span><span class="sxs-lookup"><span data-stu-id="84145-466">Then, as a type argument to `B`, `int[]` is substituted for `U` in `public U F(long index)`, yielding the inherited member `public int[] F(long index)`.</span></span>

### <a name="the-new-modifier"></a><span data-ttu-id="84145-467">El nuevo modificador</span><span class="sxs-lookup"><span data-stu-id="84145-467">The new modifier</span></span>

<span data-ttu-id="84145-468">Un *class_member_declaration* puede declarar un miembro con el mismo nombre o signatura que un miembro heredado.</span><span class="sxs-lookup"><span data-stu-id="84145-468">A *class_member_declaration* is permitted to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="84145-469">Cuando esto ocurre, se dice que el miembro de la clase derivada ***oculta*** el miembro de la clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-469">When this occurs, the derived class member is said to ***hide*** the base class member.</span></span> <span data-ttu-id="84145-470">Ocultar un miembro heredado no se considera un error, pero hace que el compilador emita una advertencia.</span><span class="sxs-lookup"><span data-stu-id="84145-470">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="84145-471">Para suprimir la advertencia, la declaración del miembro de la clase derivada puede `new` incluir un modificador para indicar que el miembro derivado está pensado para ocultar el miembro base.</span><span class="sxs-lookup"><span data-stu-id="84145-471">To suppress the warning, the declaration of the derived class member can include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="84145-472">Este tema se describe con más detalle en [ocultarse a través](basic-concepts.md#hiding-through-inheritance)de la herencia.</span><span class="sxs-lookup"><span data-stu-id="84145-472">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="84145-473">Si se `new` incluye un modificador en una declaración que no oculta un miembro heredado, se emite una advertencia para ese efecto.</span><span class="sxs-lookup"><span data-stu-id="84145-473">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning to that effect is issued.</span></span> <span data-ttu-id="84145-474">Esta advertencia se suprime quitando el `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-474">This warning is suppressed by removing the `new` modifier.</span></span>

### <a name="access-modifiers"></a><span data-ttu-id="84145-475">Modificadores de acceso</span><span class="sxs-lookup"><span data-stu-id="84145-475">Access modifiers</span></span>

<span data-ttu-id="84145-476">Un *class_member_declaration* puede tener cualquiera de los cinco tipos posibles de accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` o `private`.</span><span class="sxs-lookup"><span data-stu-id="84145-476">A *class_member_declaration* can have any one of the five possible kinds of declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`, or `private`.</span></span> <span data-ttu-id="84145-477">A excepción de `protected internal` la combinación, se trata de un error en tiempo de compilación para especificar más de un modificador de acceso.</span><span class="sxs-lookup"><span data-stu-id="84145-477">Except for the `protected internal` combination, it is a compile-time error to specify more than one access modifier.</span></span> <span data-ttu-id="84145-478">Cuando un *class_member_declaration* no incluye ningún modificador de acceso, se supone `private`.</span><span class="sxs-lookup"><span data-stu-id="84145-478">When a *class_member_declaration* does not include any access modifiers, `private` is assumed.</span></span>

### <a name="constituent-types"></a><span data-ttu-id="84145-479">Tipos constituyentes</span><span class="sxs-lookup"><span data-stu-id="84145-479">Constituent types</span></span>

<span data-ttu-id="84145-480">Los tipos que se usan en la declaración de un miembro se denominan tipos constituyentes de ese miembro.</span><span class="sxs-lookup"><span data-stu-id="84145-480">Types that are used in the declaration of a member are called the constituent types of that member.</span></span> <span data-ttu-id="84145-481">Los tipos constituyentes posibles son el tipo de una constante, un campo, una propiedad, un evento o un indizador, el tipo de valor devuelto de un método o un operador, y los tipos de parámetro de un método, indizador, operador o constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-481">Possible constituent types are the type of a constant, field, property, event, or indexer, the return type of a method or operator, and the parameter types of a method, indexer, operator, or instance constructor.</span></span> <span data-ttu-id="84145-482">Los tipos constituyentes de un miembro deben ser al menos tan accesibles como el propio miembro ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="84145-482">The constituent types of a member must be at least as accessible as that member itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

### <a name="static-and-instance-members"></a><span data-ttu-id="84145-483">Miembros estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="84145-483">Static and instance members</span></span>

<span data-ttu-id="84145-484">Los miembros de una clase son ***miembros estáticos*** o ***miembros de instancia***.</span><span class="sxs-lookup"><span data-stu-id="84145-484">Members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="84145-485">En general, resulta útil pensar en que los miembros estáticos pertenecen a los tipos de clase y a los miembros de instancia como pertenecientes a objetos (instancias de tipos de clase).</span><span class="sxs-lookup"><span data-stu-id="84145-485">Generally speaking, it is useful to think of static members as belonging to class types and instance members as belonging to objects (instances of class types).</span></span>

<span data-ttu-id="84145-486">Cuando un campo, método, propiedad, evento, operador o declaración de constructor incluye un `static` modificador, declara un miembro estático.</span><span class="sxs-lookup"><span data-stu-id="84145-486">When a field, method, property, event, operator, or constructor declaration includes a `static` modifier, it declares a static member.</span></span> <span data-ttu-id="84145-487">Además, una declaración de constante o de tipo declara implícitamente un miembro estático.</span><span class="sxs-lookup"><span data-stu-id="84145-487">In addition, a constant or type declaration implicitly declares a static member.</span></span> <span data-ttu-id="84145-488">Los miembros estáticos tienen las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="84145-488">Static members have the following characteristics:</span></span>

*  <span data-ttu-id="84145-489">Cuando se hace referencia a un miembro estático `M` en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, `E` debe indicar un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="84145-489">When a static member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote a type containing `M`.</span></span> <span data-ttu-id="84145-490">Se trata de un error en tiempo de `E` compilación para que denote una instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-490">It is a compile-time error for `E` to denote an instance.</span></span>
*  <span data-ttu-id="84145-491">Un campo estático identifica exactamente una ubicación de almacenamiento que compartirán todas las instancias de un tipo de clase cerrada determinado.</span><span class="sxs-lookup"><span data-stu-id="84145-491">A static field identifies exactly one storage location to be shared by all instances of a given closed class type.</span></span> <span data-ttu-id="84145-492">Independientemente del número de instancias de un tipo de clase cerrada determinado que se creen, solo hay una copia de un campo estático.</span><span class="sxs-lookup"><span data-stu-id="84145-492">No matter how many instances of a given closed class type are created, there is only ever one copy of a static field.</span></span>
*  <span data-ttu-id="84145-493">Un miembro de función estático (método, propiedad, evento, operador o constructor) no funciona en una instancia específica y es un error en tiempo de compilación para hacer referencia `this` a en este tipo de miembro de función.</span><span class="sxs-lookup"><span data-stu-id="84145-493">A static function member (method, property, event, operator, or constructor) does not operate on a specific instance, and it is a compile-time error to refer to `this` in such a function member.</span></span>

<span data-ttu-id="84145-494">Cuando un campo, un método, una propiedad, un evento, un indexador, un constructor o una declaración `static` de destructor no incluye un modificador, declara un miembro de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-494">When a field, method, property, event, indexer, constructor, or destructor declaration does not include a `static` modifier, it declares an instance member.</span></span> <span data-ttu-id="84145-495">(Un miembro de instancia se denomina a veces miembro no estático). Los miembros de instancia tienen las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="84145-495">(An instance member is sometimes called a non-static member.) Instance members have the following characteristics:</span></span>

*  <span data-ttu-id="84145-496">Cuando se hace referencia a un miembro de instancia `M` en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, `E` debe indicar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="84145-496">When an instance member `M` is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, `E` must denote an instance of a type containing `M`.</span></span> <span data-ttu-id="84145-497">Es un error en tiempo de enlace para `E` que denote un tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-497">It is a binding-time error for `E` to denote a type.</span></span>
*  <span data-ttu-id="84145-498">Cada instancia de una clase contiene un conjunto independiente de todos los campos de instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-498">Every instance of a class contains a separate set of all instance fields of the class.</span></span>
*  <span data-ttu-id="84145-499">Un miembro de función de instancia (método, propiedad, indizador, constructor de instancia o destructor) opera en una instancia determinada de la clase y se puede tener acceso a esta `this` instancia como ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="84145-499">An instance function member (method, property, indexer, instance constructor, or destructor) operates on a given instance of the class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="84145-500">En el ejemplo siguiente se muestran las reglas para tener acceso a los miembros estáticos y de instancia:</span><span class="sxs-lookup"><span data-stu-id="84145-500">The following example illustrates the rules for accessing static and instance members:</span></span>
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

<span data-ttu-id="84145-501">El método `F` muestra que en un miembro de función de instancia, se puede usar *simple_name* ([nombres simples](expressions.md#simple-names)) para tener acceso a los miembros de instancia y a los miembros estáticos.</span><span class="sxs-lookup"><span data-stu-id="84145-501">The `F` method shows that in an instance function member, a *simple_name* ([Simple names](expressions.md#simple-names)) can be used to access both instance members and static members.</span></span> <span data-ttu-id="84145-502">El método `G` muestra que en un miembro de función estático, es un error en tiempo de compilación para tener acceso a un miembro de instancia a través de un *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="84145-502">The `G` method shows that in a static function member, it is a compile-time error to access an instance member through a *simple_name*.</span></span> <span data-ttu-id="84145-503">El método `Main` muestra que, en un *member_access* de[acceso a miembros](expressions.md#member-access), se debe tener acceso a los miembros de instancia a través de instancias de y se debe tener acceso a los miembros estáticos a través de los tipos.</span><span class="sxs-lookup"><span data-stu-id="84145-503">The `Main` method shows that in a *member_access* ([Member access](expressions.md#member-access)), instance members must be accessed through instances, and static members must be accessed through types.</span></span>

### <a name="nested-types"></a><span data-ttu-id="84145-504">Tipos anidados</span><span class="sxs-lookup"><span data-stu-id="84145-504">Nested types</span></span>

<span data-ttu-id="84145-505">Un tipo declarado dentro de una declaración de clase o struct se denomina ***tipo anidado***.</span><span class="sxs-lookup"><span data-stu-id="84145-505">A type declared within a class or struct declaration is called a ***nested type***.</span></span> <span data-ttu-id="84145-506">Un tipo que se declara dentro de una unidad de compilación o espacio de nombres se denomina ***tipo no anidado***.</span><span class="sxs-lookup"><span data-stu-id="84145-506">A type that is declared within a compilation unit or namespace is called a ***non-nested type***.</span></span>

<span data-ttu-id="84145-507">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-507">In the example</span></span>
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
<span data-ttu-id="84145-508">la `B` clase es un tipo anidado porque se declara dentro de `A`la clase y `A` la clase es un tipo no anidado porque se declara dentro de una unidad de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-508">class `B` is a nested type because it is declared within class `A`, and class `A` is a non-nested type because it is declared within a compilation unit.</span></span>

#### <a name="fully-qualified-name"></a><span data-ttu-id="84145-509">Nombre completo</span><span class="sxs-lookup"><span data-stu-id="84145-509">Fully qualified name</span></span>

<span data-ttu-id="84145-510">El nombre completo ([nombres](basic-concepts.md#fully-qualified-names)completos) de un tipo anidado es `S.N` , donde `S` es el nombre completo del tipo en el que se declara el `N` tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-510">The fully qualified name ([Fully qualified names](basic-concepts.md#fully-qualified-names)) for a nested type is `S.N` where `S` is the fully qualified name of the type in which type `N` is declared.</span></span>

#### <a name="declared-accessibility"></a><span data-ttu-id="84145-511">Accesibilidad declarada</span><span class="sxs-lookup"><span data-stu-id="84145-511">Declared accessibility</span></span>

<span data-ttu-id="84145-512">Los tipos no anidados pueden tener `public` o `internal` declarar accesibilidad y `internal` declarar accesibilidad de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="84145-512">Non-nested types can have `public` or `internal` declared accessibility and have `internal` declared accessibility by default.</span></span> <span data-ttu-id="84145-513">Los tipos anidados también pueden tener estas formas de accesibilidad declarada, además de una o varias formas adicionales de accesibilidad declarada, dependiendo de si el tipo contenedor es una clase o un struct:</span><span class="sxs-lookup"><span data-stu-id="84145-513">Nested types can have these forms of declared accessibility too, plus one or more additional forms of declared accessibility, depending on whether the containing type is a class or struct:</span></span>

*  <span data-ttu-id="84145-514">Un tipo anidado que se declara en una clase puede tener cualquiera de las cinco formas de accesibilidad declarada `protected`( `internal``public`, `protected internal`, `private`, o) y, al igual que `private` otros miembros de clase, el valor predeterminado es declarado. Consejos.</span><span class="sxs-lookup"><span data-stu-id="84145-514">A nested type that is declared in a class can have any of five forms of declared accessibility (`public`, `protected internal`, `protected`, `internal`, or `private`) and, like other class members, defaults to `private` declared accessibility.</span></span>
*  <span data-ttu-id="84145-515">Un tipo anidado que se declara en un struct puede tener cualquiera de las`public`tres formas de accesibilidad declarada ( `private`, `internal`o) y, al igual que `private` otros miembros de struct, tiene como valor predeterminado la accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="84145-515">A nested type that is declared in a struct can have any of three forms of declared accessibility (`public`, `internal`, or `private`) and, like other struct members, defaults to `private` declared accessibility.</span></span>

<span data-ttu-id="84145-516">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-516">The example</span></span>
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
<span data-ttu-id="84145-517">declara una clase `Node`anidada privada.</span><span class="sxs-lookup"><span data-stu-id="84145-517">declares a private nested class `Node`.</span></span>

#### <a name="hiding"></a><span data-ttu-id="84145-518">Conde</span><span class="sxs-lookup"><span data-stu-id="84145-518">Hiding</span></span>

<span data-ttu-id="84145-519">Un tipo anidado puede ocultar ([ocultar nombres](basic-concepts.md#name-hiding)) un miembro base.</span><span class="sxs-lookup"><span data-stu-id="84145-519">A nested type may hide ([Name hiding](basic-concepts.md#name-hiding)) a base member.</span></span> <span data-ttu-id="84145-520">Se `new` permite el modificador en las declaraciones de tipos anidados para que la ocultación se pueda expresar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="84145-520">The `new` modifier is permitted on nested type declarations so that hiding can be expressed explicitly.</span></span> <span data-ttu-id="84145-521">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-521">The example</span></span>
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
<span data-ttu-id="84145-522">muestra una clase `M` anidada que oculta el método `M` definido en `Base`.</span><span class="sxs-lookup"><span data-stu-id="84145-522">shows a nested class `M` that hides the method `M` defined in `Base`.</span></span>

#### <a name="this-access"></a><span data-ttu-id="84145-523">este acceso</span><span class="sxs-lookup"><span data-stu-id="84145-523">this access</span></span>

<span data-ttu-id="84145-524">Un tipo anidado y su tipo contenedor no tienen una relación especial con respecto a *this_access* ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="84145-524">A nested type and its containing type do not have a special relationship with regard to *this_access* ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="84145-525">En concreto `this` , dentro de un tipo anidado no se puede usar para hacer referencia a los miembros de instancia del tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="84145-525">Specifically, `this` within a nested type cannot be used to refer to instance members of the containing type.</span></span> <span data-ttu-id="84145-526">En los casos en los que un tipo anidado necesite tener acceso a los miembros de instancia de su tipo contenedor, se puede `this` proporcionar acceso proporcionando para la instancia del tipo contenedor como argumento de constructor para el tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="84145-526">In cases where a nested type needs access to the instance members of its containing type, access can be provided by providing the `this` for the instance of the containing type as a constructor argument for the nested type.</span></span> <span data-ttu-id="84145-527">En el ejemplo siguiente</span><span class="sxs-lookup"><span data-stu-id="84145-527">The following example</span></span>
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
<span data-ttu-id="84145-528">muestra esta técnica.</span><span class="sxs-lookup"><span data-stu-id="84145-528">shows this technique.</span></span> <span data-ttu-id="84145-529">Una instancia de `C` crea una instancia de `Nested` y pasa su propia `this` al `Nested`constructor de para proporcionar el acceso posterior a `C`los miembros de instancia de.</span><span class="sxs-lookup"><span data-stu-id="84145-529">An instance of `C` creates an instance of `Nested` and passes its own `this` to `Nested`'s constructor in order to provide subsequent access to `C`'s instance members.</span></span>

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a><span data-ttu-id="84145-530">Acceso a los miembros privados y protegidos del tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="84145-530">Access to private and protected members of the containing type</span></span>

<span data-ttu-id="84145-531">Un tipo anidado tiene acceso a todos los miembros a los que se puede tener acceso a su tipo contenedor, incluidos los miembros del tipo `private` contenedor `protected` que tienen y declaran la accesibilidad.</span><span class="sxs-lookup"><span data-stu-id="84145-531">A nested type has access to all of the members that are accessible to its containing type, including members of the containing type that have `private` and `protected` declared accessibility.</span></span> <span data-ttu-id="84145-532">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-532">The example</span></span>
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
<span data-ttu-id="84145-533">muestra una clase `C` que contiene una clase `Nested`anidada.</span><span class="sxs-lookup"><span data-stu-id="84145-533">shows a class `C` that contains a nested class `Nested`.</span></span> <span data-ttu-id="84145-534">Dentro `Nested`de, el `G` método llama al método `F` estático definido `C`en y `F` tiene una accesibilidad declarada privada.</span><span class="sxs-lookup"><span data-stu-id="84145-534">Within `Nested`, the method `G` calls the static method `F` defined in `C`, and `F` has private declared accessibility.</span></span>

<span data-ttu-id="84145-535">Un tipo anidado también puede tener acceso a los miembros protegidos definidos en un tipo base de su tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="84145-535">A nested type also may access protected members defined in a base type of its containing type.</span></span> <span data-ttu-id="84145-536">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-536">In the example</span></span>
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
<span data-ttu-id="84145-537">la `Derived.Nested` clase anidada tiene acceso al método `Derived` `F` protegido definido en la clase base, `Base`, llamando a a través de una `Derived`instancia de.</span><span class="sxs-lookup"><span data-stu-id="84145-537">the nested class `Derived.Nested` accesses the protected method `F` defined in `Derived`'s base class, `Base`, by calling through an instance of `Derived`.</span></span>

#### <a name="nested-types-in-generic-classes"></a><span data-ttu-id="84145-538">Tipos anidados en clases genéricas</span><span class="sxs-lookup"><span data-stu-id="84145-538">Nested types in generic classes</span></span>

<span data-ttu-id="84145-539">Una declaración de clase genérica puede contener declaraciones de tipos anidados.</span><span class="sxs-lookup"><span data-stu-id="84145-539">A generic class declaration can contain nested type declarations.</span></span> <span data-ttu-id="84145-540">Los parámetros de tipo de la clase envolvente se pueden usar dentro de los tipos anidados.</span><span class="sxs-lookup"><span data-stu-id="84145-540">The type parameters of the enclosing class can be used within the nested types.</span></span> <span data-ttu-id="84145-541">Una declaración de tipos anidados puede contener parámetros de tipo adicionales que solo se aplican al tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="84145-541">A nested type declaration can contain additional type parameters that apply only to the nested type.</span></span>

<span data-ttu-id="84145-542">Cada declaración de tipos contenida dentro de una declaración de clase genérica es implícitamente una declaración de tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="84145-542">Every type declaration contained within a generic class declaration is implicitly a generic type declaration.</span></span> <span data-ttu-id="84145-543">Al escribir una referencia a un tipo anidado dentro de un tipo genérico, el tipo que contiene el tipo construido, incluidos sus argumentos de tipo, debe denominarse.</span><span class="sxs-lookup"><span data-stu-id="84145-543">When writing a reference to a type nested within a generic type, the containing constructed type, including its type arguments, must be named.</span></span> <span data-ttu-id="84145-544">Sin embargo, desde dentro de la clase externa, el tipo anidado se puede usar sin calificación; el tipo de instancia de la clase externa se puede usar implícitamente al construir el tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="84145-544">However, from within the outer class, the nested type can be used without qualification; the instance type of the outer class can be implicitly used when constructing the nested type.</span></span> <span data-ttu-id="84145-545">En el ejemplo siguiente se muestran tres formas correctas de hacer referencia a un tipo construido `Inner`creado desde; los dos primeros son equivalentes:</span><span class="sxs-lookup"><span data-stu-id="84145-545">The following example shows three different correct ways to refer to a constructed type created from `Inner`; the first two are equivalent:</span></span>
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

<span data-ttu-id="84145-546">Aunque el estilo de programación es incorrecto, un parámetro de tipo de un tipo anidado puede ocultar un miembro o un parámetro de tipo declarado en el tipo externo:</span><span class="sxs-lookup"><span data-stu-id="84145-546">Although it is bad programming style, a type parameter in a nested type can hide a member or type parameter declared in the outer type:</span></span>
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a><span data-ttu-id="84145-547">Nombres de miembros reservados</span><span class="sxs-lookup"><span data-stu-id="84145-547">Reserved member names</span></span>

<span data-ttu-id="84145-548">Para facilitar la implementación C# subyacente en tiempo de ejecución, para cada declaración de miembro de origen que sea una propiedad, un evento o un indexador, la implementación debe reservar dos firmas de método basadas en el tipo de declaración de miembro, su nombre y su tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-548">To facilitate the underlying C# run-time implementation, for each source member declaration that is a property, event, or indexer, the implementation must reserve two method signatures based on the kind of the member declaration, its name, and its type.</span></span> <span data-ttu-id="84145-549">Se trata de un error en tiempo de compilación para que un programa declare un miembro cuya firma coincide con una de estas firmas reservadas, aunque la implementación de tiempo de ejecución subyacente no use estas reservas.</span><span class="sxs-lookup"><span data-stu-id="84145-549">It is a compile-time error for a program to declare a member whose signature matches one of these reserved signatures, even if the underlying run-time implementation does not make use of these reservations.</span></span>

<span data-ttu-id="84145-550">Los nombres reservados no presentan declaraciones, por lo que no participan en la búsqueda de miembros.</span><span class="sxs-lookup"><span data-stu-id="84145-550">The reserved names do not introduce declarations, thus they do not participate in member lookup.</span></span> <span data-ttu-id="84145-551">Sin embargo, las signaturas de método reservado asociadas a una declaración participan en la herencia ([herencia](classes.md#inheritance)) y se pueden ocultar `new` con el modificador ([el nuevo modificador](classes.md#the-new-modifier)).</span><span class="sxs-lookup"><span data-stu-id="84145-551">However, a declaration's associated reserved method signatures do participate in inheritance ([Inheritance](classes.md#inheritance)), and can be hidden with the `new` modifier ([The new modifier](classes.md#the-new-modifier)).</span></span>

<span data-ttu-id="84145-552">La reserva de estos nombres sirve para tres propósitos:</span><span class="sxs-lookup"><span data-stu-id="84145-552">The reservation of these names serves three purposes:</span></span>

*  <span data-ttu-id="84145-553">Para permitir que la implementación subyacente use un identificador ordinario como un nombre de método para obtener o establecer el acceso a C# la característica de lenguaje.</span><span class="sxs-lookup"><span data-stu-id="84145-553">To allow the underlying implementation to use an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="84145-554">Para permitir que otros lenguajes interoperen usando un identificador ordinario como un nombre de método para obtener o establecer el C# acceso a la característica de lenguaje.</span><span class="sxs-lookup"><span data-stu-id="84145-554">To allow other languages to interoperate using an ordinary identifier as a method name for get or set access to the C# language feature.</span></span>
*  <span data-ttu-id="84145-555">Para asegurarse de que el origen aceptado por un compilador compatible lo acepta otro, haciendo que los detalles de los nombres de miembro reservados sean coherentes C# en todas las implementaciones.</span><span class="sxs-lookup"><span data-stu-id="84145-555">To help ensure that the source accepted by one conforming compiler is accepted by another, by making the specifics of reserved member names consistent across all C# implementations.</span></span>

<span data-ttu-id="84145-556">La declaración de un destructor ([destructores) también](classes.md#destructors)hace que se Reserve una firma ([los nombres de miembro están reservados para los destructores](classes.md#member-names-reserved-for-destructors)).</span><span class="sxs-lookup"><span data-stu-id="84145-556">The declaration of a destructor ([Destructors](classes.md#destructors)) also causes a signature to be reserved ([Member names reserved for destructors](classes.md#member-names-reserved-for-destructors)).</span></span>

#### <a name="member-names-reserved-for-properties"></a><span data-ttu-id="84145-557">Nombres de miembro reservados para propiedades</span><span class="sxs-lookup"><span data-stu-id="84145-557">Member names reserved for properties</span></span>

<span data-ttu-id="84145-558">En el caso `P` de una propiedad ([propiedades](classes.md#properties)) de tipo `T`, se reservan las siguientes firmas:</span><span class="sxs-lookup"><span data-stu-id="84145-558">For a property `P` ([Properties](classes.md#properties)) of type `T`, the following signatures are reserved:</span></span>

```csharp
T get_P();
void set_P(T value);
```

<span data-ttu-id="84145-559">Ambas firmas están reservadas, aunque la propiedad sea de solo lectura o de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="84145-559">Both signatures are reserved, even if the property is read-only or write-only.</span></span>

<span data-ttu-id="84145-560">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-560">In the example</span></span>
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
<span data-ttu-id="84145-561">una clase `A` define una propiedad `P`de solo lectura, de modo que se reservan `get_P` firmas `set_P` para los métodos y.</span><span class="sxs-lookup"><span data-stu-id="84145-561">a class `A` defines a read-only property `P`, thus reserving signatures for `get_P` and `set_P` methods.</span></span> <span data-ttu-id="84145-562">Una clase `B` se deriva de `A` y oculta ambas firmas reservadas.</span><span class="sxs-lookup"><span data-stu-id="84145-562">A class `B` derives from `A` and hides both of these reserved signatures.</span></span> <span data-ttu-id="84145-563">En el ejemplo se genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="84145-563">The example produces the output:</span></span>
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a><span data-ttu-id="84145-564">Nombres de miembro reservados para eventos</span><span class="sxs-lookup"><span data-stu-id="84145-564">Member names reserved for events</span></span>

<span data-ttu-id="84145-565">Para un evento `E` ([eventos](classes.md#events)) de tipo `T`delegado, se reservan las siguientes firmas:</span><span class="sxs-lookup"><span data-stu-id="84145-565">For an event `E` ([Events](classes.md#events)) of delegate type `T`, the following signatures are reserved:</span></span>
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a><span data-ttu-id="84145-566">Nombres de miembro reservados para los indizadores</span><span class="sxs-lookup"><span data-stu-id="84145-566">Member names reserved for indexers</span></span>

<span data-ttu-id="84145-567">En el caso de un indexador ([indexadores](classes.md#indexers)) de tipo `T` con `L`la lista de parámetros, se reservan las siguientes firmas:</span><span class="sxs-lookup"><span data-stu-id="84145-567">For an indexer ([Indexers](classes.md#indexers)) of type `T` with parameter-list `L`, the following signatures are reserved:</span></span>
```csharp
T get_Item(L);
void set_Item(L, T value);
```

<span data-ttu-id="84145-568">Ambas firmas están reservadas, aunque el indizador sea de solo lectura o de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="84145-568">Both signatures are reserved, even if the indexer is read-only or write-only.</span></span>

<span data-ttu-id="84145-569">Además, el nombre `Item` del miembro está reservado.</span><span class="sxs-lookup"><span data-stu-id="84145-569">Furthermore the member name `Item` is reserved.</span></span>

#### <a name="member-names-reserved-for-destructors"></a><span data-ttu-id="84145-570">Nombres de miembro reservados para destructores</span><span class="sxs-lookup"><span data-stu-id="84145-570">Member names reserved for destructors</span></span>

<span data-ttu-id="84145-571">Para una clase que contiene un destructor[(](classes.md#destructors)destructores), se reserva la firma siguiente:</span><span class="sxs-lookup"><span data-stu-id="84145-571">For a class containing a destructor ([Destructors](classes.md#destructors)), the following signature is reserved:</span></span>
```csharp
void Finalize();
```

## <a name="constants"></a><span data-ttu-id="84145-572">Constantes</span><span class="sxs-lookup"><span data-stu-id="84145-572">Constants</span></span>

<span data-ttu-id="84145-573">Una ***constante*** es un miembro de clase que representa un valor constante: un valor que se puede calcular en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-573">A ***constant*** is a class member that represents a constant value: a value that can be computed at compile-time.</span></span> <span data-ttu-id="84145-574">Un *constant_declaration* introduce una o más constantes de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="84145-574">A *constant_declaration* introduces one or more constants of a given type.</span></span>

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

<span data-ttu-id="84145-575">Un *constant_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), un modificador `new` ([el nuevo modificador](classes.md#the-new-modifier)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="84145-575">A *constant_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span> <span data-ttu-id="84145-576">Los atributos y modificadores se aplican a todos los miembros declarados por *constant_declaration*.</span><span class="sxs-lookup"><span data-stu-id="84145-576">The attributes and modifiers apply to all of the members declared by the *constant_declaration*.</span></span> <span data-ttu-id="84145-577">Aunque las constantes se consideran miembros estáticos, una *constant_declaration* no requiere ni permite un modificador `static`.</span><span class="sxs-lookup"><span data-stu-id="84145-577">Even though constants are considered static members, a *constant_declaration* neither requires nor allows a `static` modifier.</span></span> <span data-ttu-id="84145-578">Es un error que el mismo modificador aparezca varias veces en una declaración de constante.</span><span class="sxs-lookup"><span data-stu-id="84145-578">It is an error for the same modifier to appear multiple times in a constant declaration.</span></span>

<span data-ttu-id="84145-579">El *tipo* de *constant_declaration* especifica el tipo de los miembros introducidos por la declaración.</span><span class="sxs-lookup"><span data-stu-id="84145-579">The *type* of a *constant_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="84145-580">El tipo va seguido de una lista de *constant_declarator*s, cada uno de los cuales incorpora un nuevo miembro.</span><span class="sxs-lookup"><span data-stu-id="84145-580">The type is followed by a list of *constant_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="84145-581">Un *constant_declarator* consta de un *identificador* que denomina al miembro, seguido de un token "`=`", seguido de un *constant_expression* ([expresiones constantes](expressions.md#constant-expressions)) que proporciona el valor del miembro.</span><span class="sxs-lookup"><span data-stu-id="84145-581">A *constant_declarator* consists of an *identifier* that names the member, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the member.</span></span>

<span data-ttu-id="84145-582">El *tipo* especificado en una declaración de constante debe ser `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3, 4, *enum_type*o *reference_ Escriba*.</span><span class="sxs-lookup"><span data-stu-id="84145-582">The *type* specified in a constant declaration must be `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, an *enum_type*, or a *reference_type*.</span></span> <span data-ttu-id="84145-583">Cada *constant_expression* debe producir un valor del tipo de destino o de un tipo que se pueda convertir al tipo de destino mediante una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="84145-583">Each *constant_expression* must yield a value of the target type or of a type that can be converted to the target type by an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>

<span data-ttu-id="84145-584">El *tipo* de una constante debe ser al menos tan accesible como la propia constante ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="84145-584">The *type* of a constant must be at least as accessible as the constant itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="84145-585">El valor de una constante se obtiene en una expresión usando *simple_name* ([nombres simples](expressions.md#simple-names)) o *member_access* ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="84145-585">The value of a constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="84145-586">Una constante puede participar en un *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="84145-586">A constant can itself participate in a *constant_expression*.</span></span> <span data-ttu-id="84145-587">Por lo tanto, se puede utilizar una constante en cualquier construcción que requiera un *constant_expression*.</span><span class="sxs-lookup"><span data-stu-id="84145-587">Thus, a constant may be used in any construct that requires a *constant_expression*.</span></span> <span data-ttu-id="84145-588">Entre los ejemplos de estas construcciones `case` se incluyen `goto case` etiquetas, `enum` instrucciones, declaraciones de miembros, atributos y otras declaraciones de constantes.</span><span class="sxs-lookup"><span data-stu-id="84145-588">Examples of such constructs include `case` labels, `goto case` statements, `enum` member declarations, attributes, and other constant declarations.</span></span>

<span data-ttu-id="84145-589">Como se describe en [expresiones constantes](expressions.md#constant-expressions), un *constant_expression* es una expresión que se puede evaluar por completo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-589">As described in [Constant expressions](expressions.md#constant-expressions), a *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span> <span data-ttu-id="84145-590">Dado que la única forma de crear un valor que no sea NULL de un *reference_type* distinto de `string` es aplicar el operador `new` y, puesto que no se permite el operador `new` en un *constant_expression*, el único valor posible para las constantes de  *reference_type*s excepto `string` es `null`.</span><span class="sxs-lookup"><span data-stu-id="84145-590">Since the only way to create a non-null value of a *reference_type* other than `string` is to apply the `new` operator, and since the `new` operator is not permitted in a *constant_expression*, the only possible value for constants of *reference_type*s other than `string` is `null`.</span></span>

<span data-ttu-id="84145-591">Cuando se desea un nombre simbólico para un valor constante, pero cuando el tipo de ese valor no se permite en una declaración de constante, o cuando el valor no se puede calcular en tiempo de compilación mediante un *constant_expression*, un campo `readonly` ([campos de solo lectura](classes.md#readonly-fields)) puede en su lugar, use.</span><span class="sxs-lookup"><span data-stu-id="84145-591">When a symbolic name for a constant value is desired, but when the type of that value is not permitted in a constant declaration, or when the value cannot be computed at compile-time by a *constant_expression*, a `readonly` field ([Readonly fields](classes.md#readonly-fields)) may be used instead.</span></span>

<span data-ttu-id="84145-592">Una declaración de constante que declara varias constantes es equivalente a varias declaraciones de constantes únicas con los mismos atributos, modificadores y tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-592">A constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="84145-593">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-593">For example</span></span>
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
<span data-ttu-id="84145-594">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="84145-594">is equivalent to</span></span>
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

<span data-ttu-id="84145-595">Las constantes pueden depender de otras constantes en el mismo programa, siempre que las dependencias no sean de naturaleza circular.</span><span class="sxs-lookup"><span data-stu-id="84145-595">Constants are permitted to depend on other constants within the same program as long as the dependencies are not of a circular nature.</span></span> <span data-ttu-id="84145-596">El compilador se organiza automáticamente para evaluar las declaraciones de constantes en el orden adecuado.</span><span class="sxs-lookup"><span data-stu-id="84145-596">The compiler automatically arranges to evaluate the constant declarations in the appropriate order.</span></span> <span data-ttu-id="84145-597">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-597">In the example</span></span>
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
<span data-ttu-id="84145-598">en primer `A.Y`lugar, el compilador evalúa `B.Z`, evalúa y, por `A.X`último, evalúa y `10`genera `11`los valores `12`, y.</span><span class="sxs-lookup"><span data-stu-id="84145-598">the compiler first evaluates `A.Y`, then evaluates `B.Z`, and finally evaluates `A.X`, producing the values `10`, `11`, and `12`.</span></span> <span data-ttu-id="84145-599">Las declaraciones de constantes pueden depender de constantes de otros programas, pero dichas dependencias solo son posibles en una dirección.</span><span class="sxs-lookup"><span data-stu-id="84145-599">Constant declarations may depend on constants from other programs, but such dependencies are only possible in one direction.</span></span> <span data-ttu-id="84145-600">Tomando como referencia el ejemplo anterior, `A` si `B` y se declararon en programas independientes, sería posible `A.X` que dependa `B.Z`de, `B.Z` pero no puede depender `A.Y`simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="84145-600">Referring to the example above, if `A` and `B` were declared in separate programs, it would be possible for `A.X` to depend on `B.Z`, but `B.Z` could then not simultaneously depend on `A.Y`.</span></span>

## <a name="fields"></a><span data-ttu-id="84145-601">Campos</span><span class="sxs-lookup"><span data-stu-id="84145-601">Fields</span></span>

<span data-ttu-id="84145-602">Un ***campo*** es un miembro que representa una variable asociada con un objeto o una clase.</span><span class="sxs-lookup"><span data-stu-id="84145-602">A ***field*** is a member that represents a variable associated with an object or class.</span></span> <span data-ttu-id="84145-603">Un *field_declaration* introduce uno o más campos de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="84145-603">A *field_declaration* introduces one or more fields of a given type.</span></span>

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

<span data-ttu-id="84145-604">Un *field_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), un modificador `new` ([el nuevo modificador](classes.md#the-new-modifier)), una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)) y un modificador `static` ([ Campos estáticos y de instancia](classes.md#static-and-instance-fields)).</span><span class="sxs-lookup"><span data-stu-id="84145-604">A *field_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a `new` modifier ([The new modifier](classes.md#the-new-modifier)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and a `static` modifier ([Static and instance fields](classes.md#static-and-instance-fields)).</span></span> <span data-ttu-id="84145-605">Además, un *field_declaration* puede incluir un modificador `readonly` ([campos de solo lectura](classes.md#readonly-fields)) o un modificador `volatile` ([campos volátiles](classes.md#volatile-fields)), pero no ambos.</span><span class="sxs-lookup"><span data-stu-id="84145-605">In addition, a *field_declaration* may include a `readonly` modifier ([Readonly fields](classes.md#readonly-fields)) or a `volatile` modifier ([Volatile fields](classes.md#volatile-fields)) but not both.</span></span> <span data-ttu-id="84145-606">Los atributos y modificadores se aplican a todos los miembros declarados por *field_declaration*.</span><span class="sxs-lookup"><span data-stu-id="84145-606">The attributes and modifiers apply to all of the members declared by the *field_declaration*.</span></span> <span data-ttu-id="84145-607">Es un error que el mismo modificador aparezca varias veces en una declaración de campo.</span><span class="sxs-lookup"><span data-stu-id="84145-607">It is an error for the same modifier to appear multiple times in a field declaration.</span></span>

<span data-ttu-id="84145-608">El *tipo* de *field_declaration* especifica el tipo de los miembros introducidos por la declaración.</span><span class="sxs-lookup"><span data-stu-id="84145-608">The *type* of a *field_declaration* specifies the type of the members introduced by the declaration.</span></span> <span data-ttu-id="84145-609">El tipo va seguido de una lista de *variable_declarator*s, cada uno de los cuales incorpora un nuevo miembro.</span><span class="sxs-lookup"><span data-stu-id="84145-609">The type is followed by a list of *variable_declarator*s, each of which introduces a new member.</span></span> <span data-ttu-id="84145-610">Un *variable_declarator* consta de un *identificador* que asigna un nombre a ese miembro, seguido opcionalmente por un token "`=`" y un *variable_initializer* ([inicializadores de variables](classes.md#variable-initializers)) que proporciona el valor inicial de ese miembro.</span><span class="sxs-lookup"><span data-stu-id="84145-610">A *variable_declarator* consists of an *identifier* that names that member, optionally followed by an "`=`" token and a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)) that gives the initial value of that member.</span></span>

<span data-ttu-id="84145-611">El *tipo* de un campo debe ser al menos igual de accesible que el propio campo ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="84145-611">The *type* of a field must be at least as accessible as the field itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="84145-612">El valor de un campo se obtiene en una expresión usando *simple_name* ([nombres simples](expressions.md#simple-names)) o *member_access* ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="84145-612">The value of a field is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="84145-613">El valor de un campo que no es de solo lectura se modifica mediante una *asignación* ([operadores de asignación](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="84145-613">The value of a non-readonly field is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="84145-614">El valor de un campo que no es de solo lectura se puede obtener y modificar mediante operadores de incremento y decremento postfijos ([operadores de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)postfijo) y operadores de incremento y decremento prefijos ([incremento y decremento de prefijos operadores](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="84145-614">The value of a non-readonly field can be both obtained and modified using postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)) and prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span>

<span data-ttu-id="84145-615">Una declaración de campo que declara varios campos es equivalente a varias declaraciones de campos únicos con los mismos atributos, modificadores y tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-615">A field declaration that declares multiple fields is equivalent to multiple declarations of single fields with the same attributes, modifiers, and type.</span></span> <span data-ttu-id="84145-616">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-616">For example</span></span>
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
<span data-ttu-id="84145-617">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="84145-617">is equivalent to</span></span>
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a><span data-ttu-id="84145-618">Campos estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="84145-618">Static and instance fields</span></span>

<span data-ttu-id="84145-619">Cuando una declaración de campo incluye `static` un modificador, los campos introducidos por la declaración son ***campos estáticos***.</span><span class="sxs-lookup"><span data-stu-id="84145-619">When a field declaration includes a `static` modifier, the fields introduced by the declaration are ***static fields***.</span></span> <span data-ttu-id="84145-620">Cuando no `static` hay ningún modificador, los campos introducidos por la declaración son ***campos de instancia***.</span><span class="sxs-lookup"><span data-stu-id="84145-620">When no `static` modifier is present, the fields introduced by the declaration are ***instance fields***.</span></span> <span data-ttu-id="84145-621">Los campos estáticos y los campos de instancia son dos de los diversos tipos de variables ([Variables](variables.md)) que admite C# y, en ocasiones, se hace referencia a ellas como ***variables estáticas*** y ***variables de instancia***, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="84145-621">Static fields and instance fields are two of the several kinds of variables ([Variables](variables.md)) supported by C#, and at times they are referred to as ***static variables*** and ***instance variables***, respectively.</span></span>

<span data-ttu-id="84145-622">Los campos estáticos no forman parte de una instancia específica; en su lugar, se comparte entre todas las instancias de un tipo cerrado ([tipos abiertos y cerrados](types.md#open-and-closed-types)).</span><span class="sxs-lookup"><span data-stu-id="84145-622">A static field is not part of a specific instance; instead, it is shared amongst all instances of a closed type ([Open and closed types](types.md#open-and-closed-types)).</span></span> <span data-ttu-id="84145-623">Independientemente del número de instancias de un tipo de clase cerrada, solo hay una copia de un campo estático para el dominio de aplicación asociado.</span><span class="sxs-lookup"><span data-stu-id="84145-623">No matter how many instances of a closed class type are created, there is only ever one copy of a static field for the associated application domain.</span></span>

<span data-ttu-id="84145-624">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-624">For example:</span></span>
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

<span data-ttu-id="84145-625">Un campo de instancia pertenece a una instancia de.</span><span class="sxs-lookup"><span data-stu-id="84145-625">An instance field belongs to an instance.</span></span> <span data-ttu-id="84145-626">En concreto, cada instancia de una clase contiene un conjunto independiente de todos los campos de instancia de esa clase.</span><span class="sxs-lookup"><span data-stu-id="84145-626">Specifically, every instance of a class contains a separate set of all the instance fields of that class.</span></span>

<span data-ttu-id="84145-627">Cuando se hace referencia a un campo en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, si `M` es un campo estático, `E` debe indicar un tipo que contiene `M` y, si `M` es un campo de instancia, E debe denotar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="84145-627">When a field is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static field, `E` must denote a type containing `M`, and if `M` is an instance field, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="84145-628">Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="84145-628">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="readonly-fields"></a><span data-ttu-id="84145-629">Campos de solo lectura</span><span class="sxs-lookup"><span data-stu-id="84145-629">Readonly fields</span></span>

<span data-ttu-id="84145-630">Cuando un *field_declaration* incluye un modificador `readonly`, los campos introducidos por la declaración son ***campos de solo lectura***.</span><span class="sxs-lookup"><span data-stu-id="84145-630">When a *field_declaration* includes a `readonly` modifier, the fields introduced by the declaration are ***readonly fields***.</span></span> <span data-ttu-id="84145-631">Las asignaciones directas a campos de solo lectura solo se pueden producir como parte de la declaración o en un constructor de instancia o en un constructor estático de la misma clase.</span><span class="sxs-lookup"><span data-stu-id="84145-631">Direct assignments to readonly fields can only occur as part of that declaration or in an instance constructor or static constructor in the same class.</span></span> <span data-ttu-id="84145-632">(Un campo de solo lectura se puede asignar a varias veces en estos contextos). En concreto, las asignaciones directas a un `readonly` campo solo se permiten en los contextos siguientes:</span><span class="sxs-lookup"><span data-stu-id="84145-632">(A readonly field can be assigned to multiple times in these contexts.) Specifically, direct assignments to a `readonly` field are permitted only in the following contexts:</span></span>

*  <span data-ttu-id="84145-633">En el *variable_declarator* que introduce el campo (mediante la inclusión de un *variable_initializer* en la declaración).</span><span class="sxs-lookup"><span data-stu-id="84145-633">In the *variable_declarator* that introduces the field (by including a *variable_initializer* in the declaration).</span></span>
*  <span data-ttu-id="84145-634">Para un campo de instancia, en los constructores de instancia de la clase que contiene la declaración de campo; para un campo estático, en el constructor estático de la clase que contiene la declaración de campo.</span><span class="sxs-lookup"><span data-stu-id="84145-634">For an instance field, in the instance constructors of the class that contains the field declaration; for a static field, in the static constructor of the class that contains the field declaration.</span></span> <span data-ttu-id="84145-635">Estos son también los únicos contextos en los que es válido pasar un `readonly` campo como un `out` parámetro o `ref` .</span><span class="sxs-lookup"><span data-stu-id="84145-635">These are also the only contexts in which it is valid to pass a `readonly` field as an `out` or `ref` parameter.</span></span>

<span data-ttu-id="84145-636">Si se intenta asignar a un `readonly` campo o pasarlo como un `out` parámetro `ref` o en cualquier otro contexto, se trata de un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-636">Attempting to assign to a `readonly` field or pass it as an `out` or `ref` parameter in any other context is a compile-time error.</span></span>

#### <a name="using-static-readonly-fields-for-constants"></a><span data-ttu-id="84145-637">Usar campos de solo lectura estáticos para constantes</span><span class="sxs-lookup"><span data-stu-id="84145-637">Using static readonly fields for constants</span></span>

<span data-ttu-id="84145-638">Un `static readonly` campo es útil cuando se desea un nombre simbólico para un valor constante, pero cuando no se permite el tipo del valor en una `const` declaración, o cuando el valor no se puede calcular en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-638">A `static readonly` field is useful when a symbolic name for a constant value is desired, but when the type of the value is not permitted in a `const` declaration, or when the value cannot be computed at compile-time.</span></span> <span data-ttu-id="84145-639">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-639">In the example</span></span>
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
<span data-ttu-id="84145-640">los `Black`miembros `White` `const` ,, ,`Green`y no`Blue` se pueden declarar como miembros porque sus valores no se pueden calcular en tiempo de compilación. `Red`</span><span class="sxs-lookup"><span data-stu-id="84145-640">the `Black`, `White`, `Red`, `Green`, and `Blue` members cannot be declared as `const` members because their values cannot be computed at compile-time.</span></span> <span data-ttu-id="84145-641">Sin embargo, si se `static readonly` declaran en su lugar, se produce el mismo efecto.</span><span class="sxs-lookup"><span data-stu-id="84145-641">However, declaring them `static readonly` instead has much the same effect.</span></span>

#### <a name="versioning-of-constants-and-static-readonly-fields"></a><span data-ttu-id="84145-642">Control de versiones de constantes y campos de solo lectura estáticos</span><span class="sxs-lookup"><span data-stu-id="84145-642">Versioning of constants and static readonly fields</span></span>

<span data-ttu-id="84145-643">Las constantes y los campos de solo lectura tienen una semántica de versiones binaria diferente.</span><span class="sxs-lookup"><span data-stu-id="84145-643">Constants and readonly fields have different binary versioning semantics.</span></span> <span data-ttu-id="84145-644">Cuando una expresión hace referencia a una constante, el valor de la constante se obtiene en tiempo de compilación, pero cuando una expresión hace referencia a un campo de solo lectura, el valor del campo no se obtiene hasta el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="84145-644">When an expression references a constant, the value of the constant is obtained at compile-time, but when an expression references a readonly field, the value of the field is not obtained until run-time.</span></span> <span data-ttu-id="84145-645">Considere una aplicación que consta de dos programas independientes:</span><span class="sxs-lookup"><span data-stu-id="84145-645">Consider an application that consists of two separate programs:</span></span>
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

<span data-ttu-id="84145-646">Los `Program1` espacios `Program2` de nombres y denotan dos programas que se compilan por separado.</span><span class="sxs-lookup"><span data-stu-id="84145-646">The `Program1` and `Program2` namespaces denote two programs that are compiled separately.</span></span> <span data-ttu-id="84145-647">Dado `Program1.Utils.X` que se declara como un campo estático de solo lectura, la salida `Console.WriteLine` del valor de la instrucción no se conoce en tiempo de compilación, sino que se obtiene en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="84145-647">Because `Program1.Utils.X` is declared as a static readonly field, the value output by the `Console.WriteLine` statement is not known at compile-time, but rather is obtained at run-time.</span></span> <span data-ttu-id="84145-648">Por lo tanto, si el `X` valor de se cambia y se vuelve a `Console.WriteLine` compilar, la `Program2` instrucción generará el nuevo valor aunque no se vuelva a `Program1` compilar.</span><span class="sxs-lookup"><span data-stu-id="84145-648">Thus, if the value of `X` is changed and `Program1` is recompiled, the `Console.WriteLine` statement will output the new value even if `Program2` isn't recompiled.</span></span> <span data-ttu-id="84145-649">Sin embargo, `X` había sido una constante, el valor `X` de se habría obtenido en el momento `Program2` en que se compiló y no se vería afectado por los `Program1` cambios `Program2` en hasta que se vuelva a compilar.</span><span class="sxs-lookup"><span data-stu-id="84145-649">However, had `X` been a constant, the value of `X` would have been obtained at the time `Program2` was compiled, and would remain unaffected by changes in `Program1` until `Program2` is recompiled.</span></span>

### <a name="volatile-fields"></a><span data-ttu-id="84145-650">Campos volátiles</span><span class="sxs-lookup"><span data-stu-id="84145-650">Volatile fields</span></span>

<span data-ttu-id="84145-651">Cuando un *field_declaration* incluye un modificador `volatile`, los campos introducidos por esa declaración son ***campos volátiles***.</span><span class="sxs-lookup"><span data-stu-id="84145-651">When a *field_declaration* includes a `volatile` modifier, the fields introduced by that declaration are ***volatile fields***.</span></span>

<span data-ttu-id="84145-652">En el caso de los campos no volátiles, las técnicas de optimización que reordenan las instrucciones pueden provocar resultados inesperados e imprevisibles en programas multiproceso que tienen acceso a campos sin sincronización, como los que proporciona *lock_statement* ([el Lock (instrucción](statements.md#the-lock-statement))).</span><span class="sxs-lookup"><span data-stu-id="84145-652">For non-volatile fields, optimization techniques that reorder instructions can lead to unexpected and unpredictable results in multi-threaded programs that access fields without synchronization such as that provided by the *lock_statement* ([The lock statement](statements.md#the-lock-statement)).</span></span> <span data-ttu-id="84145-653">Estas optimizaciones las puede realizar el compilador, el sistema en tiempo de ejecución o el hardware.</span><span class="sxs-lookup"><span data-stu-id="84145-653">These optimizations can be performed by the compiler, by the run-time system, or by hardware.</span></span> <span data-ttu-id="84145-654">En el caso de los campos volátiles, las optimizaciones de reordenación están restringidas:</span><span class="sxs-lookup"><span data-stu-id="84145-654">For volatile fields, such reordering optimizations are restricted:</span></span>

*  <span data-ttu-id="84145-655">Una lectura de un campo volátil se denomina ***lectura volátil***.</span><span class="sxs-lookup"><span data-stu-id="84145-655">A read of a volatile field is called a ***volatile read***.</span></span> <span data-ttu-id="84145-656">Una lectura volátil tiene la "semántica de adquisición"; es decir, se garantiza que se produce antes de las referencias a la memoria que se producen después de ella en la secuencia de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="84145-656">A volatile read has "acquire semantics"; that is, it is guaranteed to occur prior to any references to memory that occur after it in the instruction sequence.</span></span>
*  <span data-ttu-id="84145-657">La escritura de un campo volátil se denomina ***escritura volátil***.</span><span class="sxs-lookup"><span data-stu-id="84145-657">A write of a volatile field is called a ***volatile write***.</span></span> <span data-ttu-id="84145-658">Una escritura volátil tiene "semántica de versión"; es decir, se garantiza que se produce después de cualquier referencia de memoria antes de la instrucción de escritura en la secuencia de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="84145-658">A volatile write has "release semantics"; that is, it is guaranteed to happen after any memory references prior to the write instruction in the instruction sequence.</span></span>

<span data-ttu-id="84145-659">Estas restricciones aseguran que todos los subprocesos observarán las operaciones de escritura volátiles realizadas por cualquier otro subproceso en el orden en que se realizaron.</span><span class="sxs-lookup"><span data-stu-id="84145-659">These restrictions ensure that all threads will observe volatile writes performed by any other thread in the order in which they were performed.</span></span> <span data-ttu-id="84145-660">No se requiere una implementación compatible para proporcionar una ordenación total única de escrituras volátiles, como se aprecia en todos los subprocesos de ejecución.</span><span class="sxs-lookup"><span data-stu-id="84145-660">A conforming implementation is not required to provide a single total ordering of volatile writes as seen from all threads of execution.</span></span> <span data-ttu-id="84145-661">El tipo de un campo volátil debe ser uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="84145-661">The type of a volatile field must be one of the following:</span></span>

*  <span data-ttu-id="84145-662">Un *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="84145-662">A *reference_type*.</span></span>
*  <span data-ttu-id="84145-663">`byte`Tipo, ,`System.IntPtr`,,,,,,, o`System.UIntPtr`. `char` `uint` `ushort` `int` `sbyte` `short` `float` `bool`</span><span class="sxs-lookup"><span data-stu-id="84145-663">The type `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, or `System.UIntPtr`.</span></span>
*  <span data-ttu-id="84145-664">Un *enum_type* que tiene un tipo base enum de `byte`, `sbyte`, `short`, `ushort`, `int` o `uint`.</span><span class="sxs-lookup"><span data-stu-id="84145-664">An *enum_type* having an enum base type of `byte`, `sbyte`, `short`, `ushort`, `int`, or `uint`.</span></span>

<span data-ttu-id="84145-665">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-665">The example</span></span>
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
<span data-ttu-id="84145-666">genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="84145-666">produces the output:</span></span>
```console
result = 143
```

<span data-ttu-id="84145-667">En este ejemplo, el método `Main` inicia un nuevo subproceso que ejecuta el `Thread2`método.</span><span class="sxs-lookup"><span data-stu-id="84145-667">In this example, the method `Main` starts a new thread that runs the method `Thread2`.</span></span> <span data-ttu-id="84145-668">Este método almacena un valor en un campo no volátil denominado `result`y, a continuación, almacena `true` en el `finished`campo volatile.</span><span class="sxs-lookup"><span data-stu-id="84145-668">This method stores a value into a non-volatile field called `result`, then stores `true` in the volatile field `finished`.</span></span> <span data-ttu-id="84145-669">El subproceso principal espera a que el `finished` campo se establezca en `true`y, a continuación, `result`lee el campo.</span><span class="sxs-lookup"><span data-stu-id="84145-669">The main thread waits for the field `finished` to be set to `true`, then reads the field `result`.</span></span> <span data-ttu-id="84145-670">Dado `finished` que se ha `volatile`declarado, el subproceso principal debe leer `143` el valor del `result`campo.</span><span class="sxs-lookup"><span data-stu-id="84145-670">Since `finished` has been declared `volatile`, the main thread must read the value `143` from the field `result`.</span></span> <span data-ttu-id="84145-671">Si no se `finished` ha declarado `volatile`el campo, se permite que el almacén `result` sea visible para el subproceso principal después del almacén en `finished`y, por lo tanto, para que el subproceso principal Lea el valor `0` de la campo `result`.</span><span class="sxs-lookup"><span data-stu-id="84145-671">If the field `finished` had not been declared `volatile`, then it would be permissible for the store to `result` to be visible to the main thread after the store to `finished`, and hence for the main thread to read the value `0` from the field `result`.</span></span> <span data-ttu-id="84145-672">Declarar como un `volatile` campo evita cualquier incoherencia. `finished`</span><span class="sxs-lookup"><span data-stu-id="84145-672">Declaring `finished` as a `volatile` field prevents any such inconsistency.</span></span>

### <a name="field-initialization"></a><span data-ttu-id="84145-673">Inicialización de campos</span><span class="sxs-lookup"><span data-stu-id="84145-673">Field initialization</span></span>

<span data-ttu-id="84145-674">El valor inicial de un campo, ya sea un campo estático o un campo de instancia, es el valor predeterminado ([valores predeterminados](variables.md#default-values)) del tipo de campo.</span><span class="sxs-lookup"><span data-stu-id="84145-674">The initial value of a field, whether it be a static field or an instance field, is the default value ([Default values](variables.md#default-values)) of the field's type.</span></span> <span data-ttu-id="84145-675">No es posible observar el valor de un campo antes de que se haya producido esta inicialización predeterminada y, por tanto, un campo nunca es "no inicializado".</span><span class="sxs-lookup"><span data-stu-id="84145-675">It is not possible to observe the value of a field before this default initialization has occurred, and a field is thus never "uninitialized".</span></span> <span data-ttu-id="84145-676">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-676">The example</span></span>
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
<span data-ttu-id="84145-677">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="84145-677">produces the output</span></span>
```console
b = False, i = 0
```
<span data-ttu-id="84145-678">dado `b` que `i` y se inicializan automáticamente en los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="84145-678">because `b` and `i` are both automatically initialized to default values.</span></span>

### <a name="variable-initializers"></a><span data-ttu-id="84145-679">Inicializadores de variables</span><span class="sxs-lookup"><span data-stu-id="84145-679">Variable initializers</span></span>

<span data-ttu-id="84145-680">Las declaraciones de campo pueden incluir *variable_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="84145-680">Field declarations may include *variable_initializer*s.</span></span> <span data-ttu-id="84145-681">En los campos estáticos, los inicializadores de variables corresponden a las instrucciones de asignación que se ejecutan durante la inicialización de la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-681">For static fields, variable initializers correspond to assignment statements that are executed during class initialization.</span></span> <span data-ttu-id="84145-682">En el caso de los campos de instancia, los inicializadores de variables corresponden a las instrucciones de asignación que se ejecutan cuando se crea una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-682">For instance fields, variable initializers correspond to assignment statements that are executed when an instance of the class is created.</span></span>

<span data-ttu-id="84145-683">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-683">The example</span></span>
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
<span data-ttu-id="84145-684">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="84145-684">produces the output</span></span>
```console
x = 1.4142135623731, i = 100, s = Hello
```
<span data-ttu-id="84145-685">Dado que `x` una asignación tiene lugar cuando los inicializadores de campo estáticos ejecutan y `s` se asignan a `i` y se producen cuando se ejecutan los inicializadores de campo de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-685">because an assignment to `x` occurs when static field initializers execute and assignments to `i` and `s` occur when the instance field initializers execute.</span></span>

<span data-ttu-id="84145-686">La inicialización del valor predeterminado que se describe en [inicialización de campos](classes.md#field-initialization) se produce para todos los campos, incluidos los campos que tienen inicializadores variables.</span><span class="sxs-lookup"><span data-stu-id="84145-686">The default value initialization described in [Field initialization](classes.md#field-initialization) occurs for all fields, including fields that have variable initializers.</span></span> <span data-ttu-id="84145-687">Por lo tanto, cuando se inicializa una clase, todos los campos estáticos de esa clase se inicializan primero a sus valores predeterminados y, a continuación, los inicializadores de campo estáticos se ejecutan en orden textual.</span><span class="sxs-lookup"><span data-stu-id="84145-687">Thus, when a class is initialized, all static fields in that class are first initialized to their default values, and then the static field initializers are executed in textual order.</span></span> <span data-ttu-id="84145-688">Del mismo modo, cuando se crea una instancia de una clase, todos los campos de instancia de esa instancia se inicializan por primera vez con sus valores predeterminados y, a continuación, los inicializadores de campo de instancia se ejecutan en orden textual.</span><span class="sxs-lookup"><span data-stu-id="84145-688">Likewise, when an instance of a class is created, all instance fields in that instance are first initialized to their default values, and then the instance field initializers are executed in textual order.</span></span>

<span data-ttu-id="84145-689">Es posible que se respeten los campos estáticos con inicializadores variables en su estado de valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="84145-689">It is possible for static fields with variable initializers to be observed in their default value state.</span></span> <span data-ttu-id="84145-690">Sin embargo, esto no se recomienda como cuestión de estilo.</span><span class="sxs-lookup"><span data-stu-id="84145-690">However, this is strongly discouraged as a matter of style.</span></span> <span data-ttu-id="84145-691">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-691">The example</span></span>
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
<span data-ttu-id="84145-692">exhibe este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="84145-692">exhibits this behavior.</span></span> <span data-ttu-id="84145-693">A pesar de las definiciones circulares de a y b, el programa es válido.</span><span class="sxs-lookup"><span data-stu-id="84145-693">Despite the circular definitions of a and b, the program is valid.</span></span> <span data-ttu-id="84145-694">Da como resultado la salida</span><span class="sxs-lookup"><span data-stu-id="84145-694">It results in the output</span></span>
```console
a = 1, b = 2
```
<span data-ttu-id="84145-695">Dado que los campos `a` estáticos y `b` se inicializan en `0` (el valor `int`predeterminado para) antes de que se ejecuten sus inicializadores.</span><span class="sxs-lookup"><span data-stu-id="84145-695">because the static fields `a` and `b` are initialized to `0` (the default value for `int`) before their initializers are executed.</span></span> <span data-ttu-id="84145-696">Cuando se ejecuta `a` el inicializador de, el valor de `b` es cero y, `a` por tanto, se `1`inicializa en.</span><span class="sxs-lookup"><span data-stu-id="84145-696">When the initializer for `a` runs, the value of `b` is zero, and so `a` is initialized to `1`.</span></span> <span data-ttu-id="84145-697">Cuando el inicializador de `b` se ejecuta, el valor `a` de ya `1` `b` es y se inicializa en `2`.</span><span class="sxs-lookup"><span data-stu-id="84145-697">When the initializer for `b` runs, the value of `a` is already `1`, and so `b` is initialized to `2`.</span></span>

#### <a name="static-field-initialization"></a><span data-ttu-id="84145-698">Inicialización de campos estáticos</span><span class="sxs-lookup"><span data-stu-id="84145-698">Static field initialization</span></span>

<span data-ttu-id="84145-699">Los inicializadores de variables de campo estáticos de una clase corresponden a una secuencia de asignaciones que se ejecutan en el orden textual en el que aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-699">The static field variable initializers of a class correspond to a sequence of assignments that are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="84145-700">Si un constructor estático ([constructores estáticos](classes.md#static-constructors)) existe en la clase, la ejecución de los inicializadores de campo estáticos se produce inmediatamente antes de ejecutar ese constructor estático.</span><span class="sxs-lookup"><span data-stu-id="84145-700">If a static constructor ([Static constructors](classes.md#static-constructors)) exists in the class, execution of the static field initializers occurs immediately prior to executing that static constructor.</span></span> <span data-ttu-id="84145-701">De lo contrario, los inicializadores de campo estáticos se ejecutan en un momento dependiente de la implementación antes del primer uso de un campo estático de esa clase.</span><span class="sxs-lookup"><span data-stu-id="84145-701">Otherwise, the static field initializers are executed at an implementation-dependent time prior to the first use of a static field of that class.</span></span> <span data-ttu-id="84145-702">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-702">The example</span></span>
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
<span data-ttu-id="84145-703">puede producir la salida:</span><span class="sxs-lookup"><span data-stu-id="84145-703">might produce either the output:</span></span>
```console
Init A
Init B
1 1
```
<span data-ttu-id="84145-704">o la salida:</span><span class="sxs-lookup"><span data-stu-id="84145-704">or the output:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="84145-705">Dado que la ejecución `X`del inicializador de `Y`y del inicializador de se puede producir en cualquier orden; solo se restringen para que se hagan referencia a esos campos.</span><span class="sxs-lookup"><span data-stu-id="84145-705">because the execution of `X`'s initializer and `Y`'s initializer could occur in either order; they are only constrained to occur before the references to those fields.</span></span> <span data-ttu-id="84145-706">Sin embargo, en el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-706">However, in the example:</span></span>
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
<span data-ttu-id="84145-707">la salida debe ser:</span><span class="sxs-lookup"><span data-stu-id="84145-707">the output must be:</span></span>
```console
Init B
Init A
1 1
```
<span data-ttu-id="84145-708">Dado que las reglas para cuando los constructores estáticos se ejecutan (como se definen en [constructores estáticos](classes.md#static-constructors)) proporcionan `B`el constructor estático de (y, por tanto `A`, los inicializadores de campo estáticos) se deben ejecutar antes que `B`el estático de inicializadores de constructores y campos.</span><span class="sxs-lookup"><span data-stu-id="84145-708">because the rules for when static constructors execute (as defined in [Static constructors](classes.md#static-constructors)) provide that `B`'s static constructor (and hence `B`'s static field initializers) must run before `A`'s static constructor and field initializers.</span></span>

#### <a name="instance-field-initialization"></a><span data-ttu-id="84145-709">Inicialización de campos de instancia</span><span class="sxs-lookup"><span data-stu-id="84145-709">Instance field initialization</span></span>

<span data-ttu-id="84145-710">Los inicializadores de variable de campo de instancia de una clase corresponden a una secuencia de asignaciones que se ejecutan inmediatamente después de la entrada a cualquiera de los constructores de instancia ([inicializadores de constructor](classes.md#constructor-initializers)) de esa clase.</span><span class="sxs-lookup"><span data-stu-id="84145-710">The instance field variable initializers of a class correspond to a sequence of assignments that are executed immediately upon entry to any one of the instance constructors ([Constructor initializers](classes.md#constructor-initializers)) of that class.</span></span> <span data-ttu-id="84145-711">Los inicializadores de variable se ejecutan en el orden textual en el que aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-711">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="84145-712">El proceso de creación e inicialización de instancias de clase se describe con más detalle en [constructores de instancias](classes.md#instance-constructors).</span><span class="sxs-lookup"><span data-stu-id="84145-712">The class instance creation and initialization process is described further in [Instance constructors](classes.md#instance-constructors).</span></span>

<span data-ttu-id="84145-713">Un inicializador de variable para un campo de instancia no puede hacer referencia a la instancia que se está creando.</span><span class="sxs-lookup"><span data-stu-id="84145-713">A variable initializer for an instance field cannot reference the instance being created.</span></span> <span data-ttu-id="84145-714">Por lo tanto, se trata de un error en tiempo de compilación para hacer referencia a `this` en un inicializador de variable, ya que se trata de un error en tiempo de compilación para que un inicializador de variable haga referencia a un miembro de instancia a través de un *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="84145-714">Thus, it is a compile-time error to reference `this` in a variable initializer, as it is a compile-time error for a variable initializer to reference any instance member through a *simple_name*.</span></span> <span data-ttu-id="84145-715">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-715">In the example</span></span>
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
<span data-ttu-id="84145-716">el inicializador de variable `y` para genera un error en tiempo de compilación porque hace referencia a un miembro de la instancia que se va a crear.</span><span class="sxs-lookup"><span data-stu-id="84145-716">the variable initializer for `y` results in a compile-time error because it references a member of the instance being created.</span></span>

## <a name="methods"></a><span data-ttu-id="84145-717">Métodos</span><span class="sxs-lookup"><span data-stu-id="84145-717">Methods</span></span>

<span data-ttu-id="84145-718">Un ***método*** es un miembro que implementa un cálculo o una acción que puede realizar un objeto o una clase.</span><span class="sxs-lookup"><span data-stu-id="84145-718">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="84145-719">Los métodos se declaran mediante *method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="84145-719">Methods are declared using *method_declaration*s:</span></span>

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

<span data-ttu-id="84145-720">Un *method_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `static` ([estático y de instancia). métodos](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), 0 ([métodos de invalidación](classes.md#override-methods)), 2 ([métodos sellados](classes.md#sealed-methods)), 4 ([métodos abstractos](classes.md#abstract-methods)) y modificadores 6 ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="84145-720">A *method_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="84145-721">Una declaración tiene una combinación válida de modificadores si se cumplen todas las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="84145-721">A declaration has a valid combination of modifiers if all of the following are true:</span></span>

*  <span data-ttu-id="84145-722">La Declaración incluye una combinación válida de modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)).</span><span class="sxs-lookup"><span data-stu-id="84145-722">The declaration includes a valid combination of access modifiers ([Access modifiers](classes.md#access-modifiers)).</span></span>
*  <span data-ttu-id="84145-723">La declaración no incluye el mismo modificador varias veces.</span><span class="sxs-lookup"><span data-stu-id="84145-723">The declaration does not include the same modifier multiple times.</span></span>
*  <span data-ttu-id="84145-724">La Declaración incluye como máximo uno de los siguientes modificadores: `static`, `virtual`y `override`.</span><span class="sxs-lookup"><span data-stu-id="84145-724">The declaration includes at most one of the following modifiers: `static`, `virtual`, and `override`.</span></span>
*  <span data-ttu-id="84145-725">La Declaración incluye como máximo uno de los siguientes modificadores: `new` y `override`.</span><span class="sxs-lookup"><span data-stu-id="84145-725">The declaration includes at most one of the following modifiers: `new` and `override`.</span></span>
*  <span data-ttu-id="84145-726">Si la Declaración incluye el `abstract` modificador, la declaración no incluye ninguno de los modificadores siguientes: `static`, `virtual` `sealed` o `extern`.</span><span class="sxs-lookup"><span data-stu-id="84145-726">If the declaration includes the `abstract` modifier, then the declaration does not include any of the following modifiers: `static`, `virtual`, `sealed` or `extern`.</span></span>
*  <span data-ttu-id="84145-727">Si la Declaración incluye el `private` modificador, la declaración no incluye ninguno de los modificadores siguientes: `virtual`, `override`o `abstract`.</span><span class="sxs-lookup"><span data-stu-id="84145-727">If the declaration includes the `private` modifier, then the declaration does not include any of the following modifiers: `virtual`, `override`, or `abstract`.</span></span>
*  <span data-ttu-id="84145-728">Si la Declaración incluye el `sealed` modificador, la Declaración también incluye el `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-728">If the declaration includes the `sealed` modifier, then the declaration also includes the `override` modifier.</span></span>
*  <span data-ttu-id="84145-729">Si la Declaración incluye el `partial` modificador, no incluye ninguno de los siguientes modificadores: `new`, `public`, `protected`, `internal`, `private`, `virtual`,, `override` `sealed` , `abstract`o .`extern`</span><span class="sxs-lookup"><span data-stu-id="84145-729">If the declaration includes the `partial` modifier, then it does not include any of the following modifiers: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`, or `extern`.</span></span>

<span data-ttu-id="84145-730">Un método que tiene el `async` modificador es una función asincrónica y sigue las reglas descritas en [funciones asincrónicas](classes.md#async-functions).</span><span class="sxs-lookup"><span data-stu-id="84145-730">A method that has the `async` modifier is an async function and follows the rules described in [Async functions](classes.md#async-functions).</span></span>

<span data-ttu-id="84145-731">El tipo de una declaración de método especifica el tipo del valor calculado y devuelto *por el método* .</span><span class="sxs-lookup"><span data-stu-id="84145-731">The *return_type* of a method declaration specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="84145-732">El valor devuelto es `void` *si el método* no devuelve un valor.</span><span class="sxs-lookup"><span data-stu-id="84145-732">The *return_type* is `void` if the method does not return a value.</span></span> <span data-ttu-id="84145-733">Si la Declaración incluye el `partial` modificador, el tipo de valor devuelto `void`debe ser.</span><span class="sxs-lookup"><span data-stu-id="84145-733">If the declaration includes the `partial` modifier, then the return type must be `void`.</span></span>

<span data-ttu-id="84145-734">*Member_name* especifica el nombre del método.</span><span class="sxs-lookup"><span data-stu-id="84145-734">The *member_name* specifies the name of the method.</span></span> <span data-ttu-id="84145-735">A menos que el método sea una implementación explícita de un miembro de interfaz ([implementaciones explícitas de miembros de interfaz](interfaces.md#explicit-interface-member-implementations)), *member_name* es simplemente un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="84145-735">Unless the method is an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="84145-736">En el caso de una implementación explícita de un miembro de interfaz, *member_name* consta de un *interface_type* seguido de un "`.`" y un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="84145-736">For an explicit interface member implementation, the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="84145-737">El *type_parameter_list* opcional especifica los parámetros de tipo del método ([parámetros de tipo](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="84145-737">The optional *type_parameter_list* specifies the type parameters of the method ([Type parameters](classes.md#type-parameters)).</span></span> <span data-ttu-id="84145-738">Si se especifica un *type_parameter_list* , el método es un ***método genérico***.</span><span class="sxs-lookup"><span data-stu-id="84145-738">If a *type_parameter_list* is specified the method is a ***generic method***.</span></span> <span data-ttu-id="84145-739">Si el método tiene un modificador `extern`, no se puede especificar *type_parameter_list* .</span><span class="sxs-lookup"><span data-stu-id="84145-739">If the method has an `extern` modifier, a *type_parameter_list* cannot be specified.</span></span>

<span data-ttu-id="84145-740">El *formal_parameter_list* opcional especifica los parámetros del método ([parámetros de método](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="84145-740">The optional *formal_parameter_list* specifies the parameters of the method ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="84145-741">Las *type_parameter_constraints_clause*s opcionales especifican restricciones en parámetros de tipo individuales ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) y solo se pueden especificar si se proporciona un *type_parameter_list* y el método no tiene un modificador `override`.</span><span class="sxs-lookup"><span data-stu-id="84145-741">The optional *type_parameter_constraints_clause*s specify constraints on individual type parameters ([Type parameter constraints](classes.md#type-parameter-constraints)) and may only be specified if a *type_parameter_list* is also supplied, and the method does not have an `override` modifier.</span></span>

<span data-ttu-id="84145-742">El *tipo y cada* uno de los tipos a los que se hace referencia en el *formal_parameter_list* de un método deben ser al menos tan accesibles como el propio método ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="84145-742">The *return_type* and each of the types referenced in the *formal_parameter_list* of a method must be at least as accessible as the method itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="84145-743">*Method_body* es un punto y coma, un ***cuerpo de instrucción*** o un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="84145-743">The *method_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="84145-744">Un cuerpo de instrucción consta de un *bloque*, que especifica las instrucciones que se ejecutarán cuando se invoque el método.</span><span class="sxs-lookup"><span data-stu-id="84145-744">A statement body consists of a *block*, which specifies the statements to execute when the method is invoked.</span></span> <span data-ttu-id="84145-745">Un cuerpo de expresión consta `=>` de seguido de una *expresión* y un punto y coma, y denota una expresión única que se debe realizar cuando se invoca el método.</span><span class="sxs-lookup"><span data-stu-id="84145-745">An expression body consists of `=>` followed by an *expression* and a semicolon, and denotes a single expression to perform when the method is invoked.</span></span> 

<span data-ttu-id="84145-746">En el caso de los métodos `abstract` y `extern`, *method_body* consta simplemente de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-746">For `abstract` and `extern` methods, the *method_body* consists simply of a semicolon.</span></span> <span data-ttu-id="84145-747">En el caso de los métodos `partial`, *method_body* puede constar de un punto y coma, un cuerpo de bloque o un cuerpo de expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-747">For `partial` methods the *method_body* may consist of either a semicolon, a block body or an expression body.</span></span> <span data-ttu-id="84145-748">En el caso de todos los demás métodos, *method_body* es un cuerpo de bloque o un cuerpo de expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-748">For all other methods, the *method_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="84145-749">Si *method_body* consta de un punto y coma, la declaración no puede incluir el modificador `async`.</span><span class="sxs-lookup"><span data-stu-id="84145-749">If the *method_body* consists of a semicolon, then the declaration may not include the `async` modifier.</span></span>

<span data-ttu-id="84145-750">El nombre, la lista de parámetros de tipo y la lista de parámetros formales de un método definen la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) del método.</span><span class="sxs-lookup"><span data-stu-id="84145-750">The name, the type parameter list and the formal parameter list of a method define the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the method.</span></span> <span data-ttu-id="84145-751">En concreto, la firma de un método consta de su nombre, el número de parámetros de tipo y el número, modificadores y tipos de sus parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="84145-751">Specifically, the signature of a method consists of its name, the number of type parameters and the number, modifiers, and types of its formal parameters.</span></span> <span data-ttu-id="84145-752">Para estos propósitos, cualquier parámetro de tipo del método que se produce en el tipo de un parámetro formal se identifica no por su nombre, sino por su posición ordinal en la lista de argumentos de tipo del método. El tipo de valor devuelto no forma parte de la signatura de un método, ni tampoco los nombres de los parámetros de tipo o los parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="84145-752">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.The return type is not part of a method's signature, nor are the names of the type parameters or the formal parameters.</span></span>

<span data-ttu-id="84145-753">El nombre de un método debe ser distinto de los nombres de todos los demás métodos que no se declaran en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="84145-753">The name of a method must differ from the names of all other non-methods declared in the same class.</span></span> <span data-ttu-id="84145-754">Además, la firma de un método debe ser diferente de las firmas de todos los demás métodos declarados en la misma clase y dos métodos declarados en la misma clase no pueden tener firmas que solo `ref` difieran en y `out`.</span><span class="sxs-lookup"><span data-stu-id="84145-754">In addition, the signature of a method must differ from the signatures of all other methods declared in the same class, and two methods declared in the same class may not have signatures that differ solely by `ref` and `out`.</span></span>

<span data-ttu-id="84145-755">Los *type_parameter*s del método están en el ámbito de la *method_declaration*y se pueden usar para formar tipos en ese ámbito en el *campo de tipo*, *method_body*y *type_parameter_constraints_clause*s, pero no en en *atributos*.</span><span class="sxs-lookup"><span data-stu-id="84145-755">The method's *type_parameter*s are in scope throughout the *method_declaration*, and can be used to form types throughout that scope in *return_type*, *method_body*, and *type_parameter_constraints_clause*s but not in *attributes*.</span></span>

<span data-ttu-id="84145-756">Todos los parámetros formales y los parámetros de tipo deben tener nombres diferentes.</span><span class="sxs-lookup"><span data-stu-id="84145-756">All formal parameters and type parameters must have different names.</span></span>

### <a name="method-parameters"></a><span data-ttu-id="84145-757">Parámetros de método</span><span class="sxs-lookup"><span data-stu-id="84145-757">Method parameters</span></span>

<span data-ttu-id="84145-758">Los parámetros de un método, si los hay, se declaran mediante la *formal_parameter_list*del método.</span><span class="sxs-lookup"><span data-stu-id="84145-758">The parameters of a method, if any, are declared by the method's *formal_parameter_list*.</span></span>

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

<span data-ttu-id="84145-759">La lista de parámetros formales está formada por uno o varios parámetros separados por comas, de los cuales solo el último puede ser un *parameter_array*.</span><span class="sxs-lookup"><span data-stu-id="84145-759">The formal parameter list consists of one or more comma-separated parameters of which only the last may be a *parameter_array*.</span></span>

<span data-ttu-id="84145-760">Un *fixed_parameter* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), un modificador opcional `ref`, `out` o `this`, un *tipo*, un *identificador* y un *default_argument*opcional.</span><span class="sxs-lookup"><span data-stu-id="84145-760">A *fixed_parameter* consists of an optional set of *attributes* ([Attributes](attributes.md)), an optional `ref`, `out` or `this` modifier, a *type*, an *identifier* and an optional *default_argument*.</span></span> <span data-ttu-id="84145-761">Cada *fixed_parameter* declara un parámetro del tipo especificado con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="84145-761">Each *fixed_parameter* declares a parameter of the given type with the given name.</span></span> <span data-ttu-id="84145-762">El `this` modificador designa el método como un método de extensión y solo se permite en el primer parámetro de un método estático.</span><span class="sxs-lookup"><span data-stu-id="84145-762">The `this` modifier designates the method as an extension method and is only allowed on the first parameter of a static method.</span></span> <span data-ttu-id="84145-763">Los métodos de extensión se describen con más detalle en [métodos de extensión](classes.md#extension-methods).</span><span class="sxs-lookup"><span data-stu-id="84145-763">Extension methods are further described in [Extension methods](classes.md#extension-methods).</span></span>

<span data-ttu-id="84145-764">Un *fixed_parameter* con un *default_argument* se conoce como ***parámetro opcional***, mientras que un *fixed_parameter* sin *default_argument* es un ***parámetro necesario***.</span><span class="sxs-lookup"><span data-stu-id="84145-764">A *fixed_parameter* with a *default_argument* is known as an ***optional parameter***, whereas a *fixed_parameter* without a *default_argument* is a ***required parameter***.</span></span> <span data-ttu-id="84145-765">Es posible que un parámetro necesario no aparezca después de un parámetro opcional en un *formal_parameter_list*.</span><span class="sxs-lookup"><span data-stu-id="84145-765">A required parameter may not appear after an optional parameter in a *formal_parameter_list*.</span></span>

<span data-ttu-id="84145-766">Un parámetro `ref` o `out` no puede tener un *default_argument*.</span><span class="sxs-lookup"><span data-stu-id="84145-766">A `ref` or `out` parameter cannot have a *default_argument*.</span></span> <span data-ttu-id="84145-767">La *expresión* de un *default_argument* debe ser una de las siguientes:</span><span class="sxs-lookup"><span data-stu-id="84145-767">The *expression* in a *default_argument* must be one of the following:</span></span>

*  <span data-ttu-id="84145-768">a *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="84145-768">a *constant_expression*</span></span>
*  <span data-ttu-id="84145-769">una expresión con el formato `new S()` , `S` donde es un tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-769">an expression of the form `new S()` where `S` is a value type</span></span>
*  <span data-ttu-id="84145-770">una expresión con el formato `default(S)` , `S` donde es un tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-770">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="84145-771">La *expresión* debe poder convertirse implícitamente mediante una identidad o una conversión que acepte valores NULL en el tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="84145-771">The *expression* must be implicitly convertible by an identity or nullable conversion to the type of the parameter.</span></span>

<span data-ttu-id="84145-772">Si se producen parámetros opcionales en una declaración de método parcial de implementación ([métodos parciales](classes.md#partial-methods)), una implementación explícita de un miembro de interfaz ([implementaciones de miembros de interfaz explícitos](interfaces.md#explicit-interface-member-implementations)) o en una declaración de indexador de un solo parámetro ([ Indexadores](classes.md#indexers)) el compilador debe proporcionar una advertencia, ya que estos miembros nunca se pueden invocar de forma que permita omitir los argumentos.</span><span class="sxs-lookup"><span data-stu-id="84145-772">If optional parameters occur in an implementing partial method declaration ([Partial methods](classes.md#partial-methods)) , an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)) or in a single-parameter indexer declaration ([Indexers](classes.md#indexers)) the compiler should give a warning, since these members can never be invoked in a way that permits arguments to be omitted.</span></span>

<span data-ttu-id="84145-773">Un *parameter_array* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), un modificador `params`, un *array_type*y un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="84145-773">A *parameter_array* consists of an optional set of *attributes* ([Attributes](attributes.md)), a `params` modifier, an *array_type*, and an *identifier*.</span></span> <span data-ttu-id="84145-774">Una matriz de parámetros declara un parámetro único del tipo de matriz especificado con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="84145-774">A parameter array declares a single parameter of the given array type with the given name.</span></span> <span data-ttu-id="84145-775">El *array_type* de una matriz de parámetros debe ser un tipo de matriz unidimensional ([tipos de matriz](arrays.md#array-types)).</span><span class="sxs-lookup"><span data-stu-id="84145-775">The *array_type* of a parameter array must be a single-dimensional array type ([Array types](arrays.md#array-types)).</span></span> <span data-ttu-id="84145-776">En una invocación de método, una matriz de parámetros permite especificar un único argumento del tipo de matriz especificado o permite que se especifiquen cero o más argumentos del tipo de elemento de matriz.</span><span class="sxs-lookup"><span data-stu-id="84145-776">In a method invocation, a parameter array permits either a single argument of the given array type to be specified, or it permits zero or more arguments of the array element type to be specified.</span></span> <span data-ttu-id="84145-777">Las matrices de parámetros se describen con más detalle en [matrices de parámetros](classes.md#parameter-arrays).</span><span class="sxs-lookup"><span data-stu-id="84145-777">Parameter arrays are described further in [Parameter arrays](classes.md#parameter-arrays).</span></span>

<span data-ttu-id="84145-778">Un *parameter_array* se puede producir después de un parámetro opcional, pero no puede tener un valor predeterminado; la omisión de los argumentos de un *parameter_array* en su lugar podría dar lugar a la creación de una matriz vacía.</span><span class="sxs-lookup"><span data-stu-id="84145-778">A *parameter_array* may occur after an optional parameter, but cannot have a default value -- the omission of arguments for a *parameter_array* would instead result in the creation of an empty array.</span></span>

<span data-ttu-id="84145-779">En el ejemplo siguiente se muestran diferentes tipos de parámetros:</span><span class="sxs-lookup"><span data-stu-id="84145-779">The following example illustrates different kinds of parameters:</span></span>
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

<span data-ttu-id="84145-780">En *formal_parameter_list* para `M`, @no__t 2 es un parámetro ref necesario, `d` es un parámetro de valor obligatorio, `b`, `s`, `o` y `t` son parámetros de valor opcionales y `a` es una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="84145-780">In the *formal_parameter_list* for `M`, `i` is a required ref parameter, `d` is a required value parameter, `b`, `s`, `o` and `t` are optional value parameters and `a` is a parameter array.</span></span>

<span data-ttu-id="84145-781">Una declaración de método crea un espacio de declaración independiente para los parámetros, los parámetros de tipo y las variables locales.</span><span class="sxs-lookup"><span data-stu-id="84145-781">A method declaration creates a separate declaration space for parameters, type parameters and local variables.</span></span> <span data-ttu-id="84145-782">Los nombres se introducen en este espacio de declaración mediante la lista de parámetros de tipo y la lista de parámetros formales del método y las declaraciones de variables locales en el *bloque* del método.</span><span class="sxs-lookup"><span data-stu-id="84145-782">Names are introduced into this declaration space by the type parameter list and the formal parameter list of the method and by local variable declarations in the *block* of the method.</span></span> <span data-ttu-id="84145-783">Es un error que dos miembros de un espacio de declaración de método tengan el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="84145-783">It is an error for two members of a method declaration space to have the same name.</span></span> <span data-ttu-id="84145-784">Es un error que el espacio de declaración de método y el espacio de declaración de variable local de un espacio de declaración anidado contengan elementos con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="84145-784">It is an error for the method declaration space and the local variable declaration space of a nested declaration space to contain elements with the same name.</span></span>

<span data-ttu-id="84145-785">Una invocación de método ([invocaciones de método](expressions.md#method-invocations)) crea una copia, específica de dicha invocación, de los parámetros formales y las variables locales del método, y la lista de argumentos de la invocación asigna valores o referencias de variables al formal creado recientemente. los.</span><span class="sxs-lookup"><span data-stu-id="84145-785">A method invocation ([Method invocations](expressions.md#method-invocations)) creates a copy, specific to that invocation, of the formal parameters and local variables of the method, and the argument list of the invocation assigns values or variable references to the newly created formal parameters.</span></span> <span data-ttu-id="84145-786">Dentro del *bloque* de un método, se puede hacer referencia a los parámetros formales por sus identificadores en expresiones *Simple_name* ([nombres simples](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="84145-786">Within the *block* of a method, formal parameters can be referenced by their identifiers in *simple_name* expressions ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="84145-787">Hay cuatro tipos de parámetros formales:</span><span class="sxs-lookup"><span data-stu-id="84145-787">There are four kinds of formal parameters:</span></span>

*  <span data-ttu-id="84145-788">Parámetros de valor, que se declaran sin ningún modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-788">Value parameters, which are declared without any modifiers.</span></span>
*  <span data-ttu-id="84145-789">Parámetros de referencia, que se declaran con el `ref` modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-789">Reference parameters, which are declared with the `ref` modifier.</span></span>
*  <span data-ttu-id="84145-790">Parámetros de salida, que se declaran con el `out` modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-790">Output parameters, which are declared with the `out` modifier.</span></span>
*  <span data-ttu-id="84145-791">Matrices de parámetros, que se declaran `params` con el modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-791">Parameter arrays, which are declared with the `params` modifier.</span></span>

<span data-ttu-id="84145-792">Como se describe en [firmas y sobrecarga](basic-concepts.md#signatures-and-overloading), los `ref` modificadores y `out` forman parte de la firma de un método, pero el `params` modificador no es.</span><span class="sxs-lookup"><span data-stu-id="84145-792">As described in [Signatures and overloading](basic-concepts.md#signatures-and-overloading), the `ref` and `out` modifiers are part of a method's signature, but the `params` modifier is not.</span></span>

#### <a name="value-parameters"></a><span data-ttu-id="84145-793">Parámetros de valor</span><span class="sxs-lookup"><span data-stu-id="84145-793">Value parameters</span></span>

<span data-ttu-id="84145-794">Un parámetro declarado sin modificadores es un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-794">A parameter declared with no modifiers is a value parameter.</span></span> <span data-ttu-id="84145-795">Un parámetro de valor corresponde a una variable local que obtiene su valor inicial del argumento correspondiente proporcionado en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="84145-795">A value parameter corresponds to a local variable that gets its initial value from the corresponding argument supplied in the method invocation.</span></span>

<span data-ttu-id="84145-796">Cuando un parámetro formal es un parámetro de valor, el argumento correspondiente en una invocación de método debe ser una expresión que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="84145-796">When a formal parameter is a value parameter, the corresponding argument in a method invocation must be an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the formal parameter type.</span></span>

<span data-ttu-id="84145-797">Un método puede asignar nuevos valores a un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-797">A method is permitted to assign new values to a value parameter.</span></span> <span data-ttu-id="84145-798">Dichas asignaciones solo afectan a la ubicación de almacenamiento local representada por el parámetro de valor; no tienen ningún efecto en el argumento real proporcionado en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="84145-798">Such assignments only affect the local storage location represented by the value parameter—they have no effect on the actual argument given in the method invocation.</span></span>

#### <a name="reference-parameters"></a><span data-ttu-id="84145-799">Parámetros de referencia</span><span class="sxs-lookup"><span data-stu-id="84145-799">Reference parameters</span></span>

<span data-ttu-id="84145-800">Un parámetro declarado con un `ref` modificador es un parámetro de referencia.</span><span class="sxs-lookup"><span data-stu-id="84145-800">A parameter declared with a `ref` modifier is a reference parameter.</span></span> <span data-ttu-id="84145-801">A diferencia de un parámetro de valor, un parámetro de referencia no crea una nueva ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="84145-801">Unlike a value parameter, a reference parameter does not create a new storage location.</span></span> <span data-ttu-id="84145-802">En su lugar, un parámetro de referencia representa la misma ubicación de almacenamiento que la variable proporcionada como argumento en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="84145-802">Instead, a reference parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="84145-803">Cuando un parámetro formal es un parámetro de referencia, el argumento correspondiente en una invocación de método debe constar de la palabra clave `ref` seguida de una *variable_reference* ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) del mismo tipo que parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="84145-803">When a formal parameter is a reference parameter, the corresponding argument in a method invocation must consist of the keyword `ref` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="84145-804">Una variable debe estar asignada definitivamente antes de que se pueda pasar como parámetro de referencia.</span><span class="sxs-lookup"><span data-stu-id="84145-804">A variable must be definitely assigned before it can be passed as a reference parameter.</span></span>

<span data-ttu-id="84145-805">Dentro de un método, un parámetro de referencia se considera siempre asignado definitivamente.</span><span class="sxs-lookup"><span data-stu-id="84145-805">Within a method, a reference parameter is always considered definitely assigned.</span></span>

<span data-ttu-id="84145-806">Un método declarado como iterador ([iteradores](classes.md#iterators)) no puede tener parámetros de referencia.</span><span class="sxs-lookup"><span data-stu-id="84145-806">A method declared as an iterator ([Iterators](classes.md#iterators)) cannot have reference parameters.</span></span>

<span data-ttu-id="84145-807">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-807">The example</span></span>
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
<span data-ttu-id="84145-808">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="84145-808">produces the output</span></span>
```console
i = 2, j = 1
```

<span data-ttu-id="84145-809">Para la invocación de `Swap` en `Main`, `x` representa `i` y `y` representa `j`.</span><span class="sxs-lookup"><span data-stu-id="84145-809">For the invocation of `Swap` in `Main`, `x` represents `i` and `y` represents `j`.</span></span> <span data-ttu-id="84145-810">Por lo tanto, la invocación tiene el efecto de intercambiar los valores `i` de `j`y.</span><span class="sxs-lookup"><span data-stu-id="84145-810">Thus, the invocation has the effect of swapping the values of `i` and `j`.</span></span>

<span data-ttu-id="84145-811">En un método que toma parámetros de referencia, es posible que varios nombres representen la misma ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="84145-811">In a method that takes reference parameters it is possible for multiple names to represent the same storage location.</span></span> <span data-ttu-id="84145-812">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-812">In the example</span></span>
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
<span data-ttu-id="84145-813">la invocación de `F` en `G` pasa `s` `b`una referencia a para y.`a`</span><span class="sxs-lookup"><span data-stu-id="84145-813">the invocation of `F` in `G` passes a reference to `s` for both `a` and `b`.</span></span> <span data-ttu-id="84145-814">Por lo tanto, para esa invocación, `s`los `a`nombres, `b` y hacen referencia a la misma ubicación de almacenamiento, y las tres asignaciones modifican el campo `s`de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-814">Thus, for that invocation, the names `s`, `a`, and `b` all refer to the same storage location, and the three assignments all modify the instance field `s`.</span></span>

#### <a name="output-parameters"></a><span data-ttu-id="84145-815">Parámetros de salida</span><span class="sxs-lookup"><span data-stu-id="84145-815">Output parameters</span></span>

<span data-ttu-id="84145-816">Un parámetro declarado con un `out` modificador es un parámetro de salida.</span><span class="sxs-lookup"><span data-stu-id="84145-816">A parameter declared with an `out` modifier is an output parameter.</span></span> <span data-ttu-id="84145-817">De forma similar a un parámetro de referencia, un parámetro de salida no crea una nueva ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="84145-817">Similar to a reference parameter, an output parameter does not create a new storage location.</span></span> <span data-ttu-id="84145-818">En su lugar, un parámetro de salida representa la misma ubicación de almacenamiento que la variable proporcionada como argumento en la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="84145-818">Instead, an output parameter represents the same storage location as the variable given as the argument in the method invocation.</span></span>

<span data-ttu-id="84145-819">Cuando un parámetro formal es un parámetro de salida, el argumento correspondiente en una invocación de método debe constar de la palabra clave `out` seguida de una *variable_reference* ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) del mismo tipo que el parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="84145-819">When a formal parameter is an output parameter, the corresponding argument in a method invocation must consist of the keyword `out` followed by a *variable_reference* ([Precise rules for determining definite assignment](variables.md#precise-rules-for-determining-definite-assignment)) of the same type as the formal parameter.</span></span> <span data-ttu-id="84145-820">No es necesario asignar definitivamente una variable antes de que se pueda pasar como parámetro de salida, pero después de una invocación en la que se pasó una variable como parámetro de salida, la variable se considera definitivamente asignada.</span><span class="sxs-lookup"><span data-stu-id="84145-820">A variable need not be definitely assigned before it can be passed as an output parameter, but following an invocation where a variable was passed as an output parameter, the variable is considered definitely assigned.</span></span>

<span data-ttu-id="84145-821">Dentro de un método, al igual que una variable local, un parámetro de salida se considera inicialmente sin asignar y debe asignarse definitivamente antes de usar su valor.</span><span class="sxs-lookup"><span data-stu-id="84145-821">Within a method, just like a local variable, an output parameter is initially considered unassigned and must be definitely assigned before its value is used.</span></span>

<span data-ttu-id="84145-822">Todos los parámetros de salida de un método se deben asignar definitivamente antes de que el método devuelva.</span><span class="sxs-lookup"><span data-stu-id="84145-822">Every output parameter of a method must be definitely assigned before the method returns.</span></span>

<span data-ttu-id="84145-823">Un método declarado como método parcial ([métodos parciales](classes.md#partial-methods)) o un iterador ([iteradores](classes.md#iterators)) no puede tener parámetros de salida.</span><span class="sxs-lookup"><span data-stu-id="84145-823">A method declared as a partial method ([Partial methods](classes.md#partial-methods)) or an iterator ([Iterators](classes.md#iterators)) cannot have output parameters.</span></span>

<span data-ttu-id="84145-824">Los parámetros de salida se usan normalmente en métodos que producen varios valores devueltos.</span><span class="sxs-lookup"><span data-stu-id="84145-824">Output parameters are typically used in methods that produce multiple return values.</span></span> <span data-ttu-id="84145-825">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-825">For example:</span></span>
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

<span data-ttu-id="84145-826">En el ejemplo se genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="84145-826">The example produces the output:</span></span>
```console
c:\Windows\System\
hello.txt
```

<span data-ttu-id="84145-827">Tenga en cuenta `dir` que `name` las variables y se pueden desasignar antes de que `SplitPath`se pasen a, y que se consideran definitivamente asignadas después de la llamada.</span><span class="sxs-lookup"><span data-stu-id="84145-827">Note that the `dir` and `name` variables can be unassigned before they are passed to `SplitPath`, and that they are considered definitely assigned following the call.</span></span>

#### <a name="parameter-arrays"></a><span data-ttu-id="84145-828">Matrices de parámetros</span><span class="sxs-lookup"><span data-stu-id="84145-828">Parameter arrays</span></span>

<span data-ttu-id="84145-829">Un parámetro declarado con un `params` modificador es una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="84145-829">A parameter declared with a `params` modifier is a parameter array.</span></span> <span data-ttu-id="84145-830">Si una lista de parámetros formales incluye una matriz de parámetros, debe ser el último parámetro de la lista y debe ser de un tipo de matriz unidimensional.</span><span class="sxs-lookup"><span data-stu-id="84145-830">If a formal parameter list includes a parameter array, it must be the last parameter in the list and it must be of a single-dimensional array type.</span></span> <span data-ttu-id="84145-831">Por ejemplo, los tipos `string[]` y `string[][]` se pueden utilizar como el tipo de una matriz de parámetros, pero el `string[,]` tipo no puede.</span><span class="sxs-lookup"><span data-stu-id="84145-831">For example, the types `string[]` and `string[][]` can be used as the type of a parameter array, but the type `string[,]` can not.</span></span> <span data-ttu-id="84145-832">No es posible combinar el `params` modificador con los `ref` modificadores y `out`.</span><span class="sxs-lookup"><span data-stu-id="84145-832">It is not possible to combine the `params` modifier with the modifiers `ref` and `out`.</span></span>

<span data-ttu-id="84145-833">Una matriz de parámetros permite especificar argumentos de una de estas dos maneras en una invocación de método:</span><span class="sxs-lookup"><span data-stu-id="84145-833">A parameter array permits arguments to be specified in one of two ways in a method invocation:</span></span>

*  <span data-ttu-id="84145-834">El argumento dado para una matriz de parámetros puede ser una expresión única que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="84145-834">The argument given for a parameter array can be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="84145-835">En este caso, la matriz de parámetros actúa exactamente como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-835">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="84145-836">Como alternativa, la invocación puede especificar cero o más argumentos para la matriz de parámetros, donde cada argumento es una expresión que se puede convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de elemento de la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="84145-836">Alternatively, the invocation can specify zero or more arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="84145-837">En este caso, la invocación crea una instancia del tipo de matriz de parámetros con una longitud que corresponde al número de argumentos, inicializa los elementos de la instancia de la matriz con los valores de argumento especificados y usa la instancia de matriz recién creada como el actual. argument.</span><span class="sxs-lookup"><span data-stu-id="84145-837">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="84145-838">A excepción de permitir un número variable de argumentos en una invocación, una matriz de parámetros es exactamente equivalente a un parámetro de valor ([parámetros de valor](classes.md#value-parameters)) del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-838">Except for allowing a variable number of arguments in an invocation, a parameter array is precisely equivalent to a value parameter ([Value parameters](classes.md#value-parameters)) of the same type.</span></span>

<span data-ttu-id="84145-839">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-839">The example</span></span>
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
<span data-ttu-id="84145-840">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="84145-840">produces the output</span></span>
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

<span data-ttu-id="84145-841">La primera invocación de `F` simplemente pasa la matriz `a` como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-841">The first invocation of `F` simply passes the array `a` as a value parameter.</span></span> <span data-ttu-id="84145-842">La segunda invocación de `F` crea automáticamente un elemento de cuatro `int[]` elementos con los valores de elemento especificados y pasa esa instancia de la matriz como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-842">The second invocation of `F` automatically creates a four-element `int[]` with the given element values and passes that array instance as a value parameter.</span></span> <span data-ttu-id="84145-843">Del mismo modo, la tercera invocación de `F` crea un elemento `int[]` de cero y pasa esa instancia como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="84145-843">Likewise, the third invocation of `F` creates a zero-element `int[]` and passes that instance as a value parameter.</span></span> <span data-ttu-id="84145-844">Las invocaciones segunda y tercera son exactamente equivalentes a la escritura:</span><span class="sxs-lookup"><span data-stu-id="84145-844">The second and third invocations are precisely equivalent to writing:</span></span>
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

<span data-ttu-id="84145-845">Al realizar la resolución de sobrecarga, un método con una matriz de parámetros puede ser aplicable en su forma normal o en su forma expandida ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="84145-845">When performing overload resolution, a method with a parameter array may be applicable either in its normal form or in its expanded form ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="84145-846">La forma expandida de un método solo está disponible si la forma normal del método no es aplicable y solo si un método aplicable con la misma signatura que la forma expandida no se ha declarado en el mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-846">The expanded form of a method is available only if the normal form of the method is not applicable and only if an applicable method with the same signature as the expanded form is not already declared in the same type.</span></span>

<span data-ttu-id="84145-847">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-847">The example</span></span>
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
<span data-ttu-id="84145-848">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="84145-848">produces the output</span></span>
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

<span data-ttu-id="84145-849">En el ejemplo, dos de las formas expandidas posibles del método con una matriz de parámetros ya están incluidas en la clase como métodos normales.</span><span class="sxs-lookup"><span data-stu-id="84145-849">In the example, two of the possible expanded forms of the method with a parameter array are already included in the class as regular methods.</span></span> <span data-ttu-id="84145-850">Por lo tanto, estos formularios expandidos no se tienen en cuenta al realizar la resolución de sobrecarga, y las invocaciones del método primero y tercer, por tanto, seleccionan los métodos normales.</span><span class="sxs-lookup"><span data-stu-id="84145-850">These expanded forms are therefore not considered when performing overload resolution, and the first and third method invocations thus select the regular methods.</span></span> <span data-ttu-id="84145-851">Cuando una clase declara un método con una matriz de parámetros, no es raro incluir también algunos de los formularios expandidos como métodos normales.</span><span class="sxs-lookup"><span data-stu-id="84145-851">When a class declares a method with a parameter array, it is not uncommon to also include some of the expanded forms as regular methods.</span></span> <span data-ttu-id="84145-852">Al hacerlo, es posible evitar la asignación de una instancia de matriz que se produce cuando se invoca una forma expandida de un método con una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="84145-852">By doing so it is possible to avoid the allocation of an array instance that occurs when an expanded form of a method with a parameter array is invoked.</span></span>

<span data-ttu-id="84145-853">Cuando el tipo de una matriz de parámetros `object[]`es, se produce una ambigüedad potencial entre la forma normal del método y la forma empleada para un solo `object` parámetro.</span><span class="sxs-lookup"><span data-stu-id="84145-853">When the type of a parameter array is `object[]`, a potential ambiguity arises between the normal form of the method and the expended form for a single `object` parameter.</span></span> <span data-ttu-id="84145-854">La razón de la ambigüedad es que `object[]` , a su vez, se pueda convertir implícitamente al tipo. `object`</span><span class="sxs-lookup"><span data-stu-id="84145-854">The reason for the ambiguity is that an `object[]` is itself implicitly convertible to type `object`.</span></span> <span data-ttu-id="84145-855">Sin embargo, la ambigüedad no presenta ningún problema, ya que se puede resolver mediante la inserción de una conversión si es necesario.</span><span class="sxs-lookup"><span data-stu-id="84145-855">The ambiguity presents no problem, however, since it can be resolved by inserting a cast if needed.</span></span>

<span data-ttu-id="84145-856">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-856">The example</span></span>
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
<span data-ttu-id="84145-857">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="84145-857">produces the output</span></span>
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

<span data-ttu-id="84145-858">En la primera y última invocación de `F`, la forma normal de `F` es aplicable porque existe una conversión implícita desde el tipo de argumento al tipo de parámetro (ambos son del tipo `object[]`).</span><span class="sxs-lookup"><span data-stu-id="84145-858">In the first and last invocations of `F`, the normal form of `F` is applicable because an implicit conversion exists from the argument type to the parameter type (both are of type `object[]`).</span></span> <span data-ttu-id="84145-859">Por lo tanto, la resolución de sobrecarga selecciona `F`la forma normal de y el argumento se pasa como un parámetro de valor normal.</span><span class="sxs-lookup"><span data-stu-id="84145-859">Thus, overload resolution selects the normal form of `F`, and the argument is passed as a regular value parameter.</span></span> <span data-ttu-id="84145-860">En la segunda y tercera invocación, la forma normal de `F` no es aplicable porque no existe ninguna conversión implícita desde el tipo de argumento al tipo de parámetro (el tipo `object` no se puede convertir implícitamente al tipo `object[]`).</span><span class="sxs-lookup"><span data-stu-id="84145-860">In the second and third invocations, the normal form of `F` is not applicable because no implicit conversion exists from the argument type to the parameter type (type `object` cannot be implicitly converted to type `object[]`).</span></span> <span data-ttu-id="84145-861">Sin embargo, la forma expandida de es aplicable, por lo que está seleccionada por la resolución de `F` sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="84145-861">However, the expanded form of `F` is applicable, so it is selected by overload resolution.</span></span> <span data-ttu-id="84145-862">Como resultado, la invocación crea un `object[]` elemento, y el único elemento de la matriz se inicializa con el valor de argumento dado (que a su vez es una referencia a un `object[]`).</span><span class="sxs-lookup"><span data-stu-id="84145-862">As a result, a one-element `object[]` is created by the invocation, and the single element of the array is initialized with the given argument value (which itself is a reference to an `object[]`).</span></span>

### <a name="static-and-instance-methods"></a><span data-ttu-id="84145-863">Métodos estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="84145-863">Static and instance methods</span></span>

<span data-ttu-id="84145-864">Cuando una declaración de método incluye `static` un modificador, se dice que se trata de un método estático.</span><span class="sxs-lookup"><span data-stu-id="84145-864">When a method declaration includes a `static` modifier, that method is said to be a static method.</span></span> <span data-ttu-id="84145-865">Cuando no `static` existe ningún modificador, se dice que el método es un método de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-865">When no `static` modifier is present, the method is said to be an instance method.</span></span>

<span data-ttu-id="84145-866">Un método estático no funciona en una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en un método estático.</span><span class="sxs-lookup"><span data-stu-id="84145-866">A static method does not operate on a specific instance, and it is a compile-time error to refer to `this` in a static method.</span></span>

<span data-ttu-id="84145-867">Un método de instancia funciona en una instancia determinada de una clase y se puede tener acceso a esa instancia `this` como ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="84145-867">An instance method operates on a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="84145-868">Cuando se hace referencia a un método en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, si `M` es un método estático, `E` debe indicar un tipo que contiene `M` y, si `M` es un método de instancia, `E` debe indicar una instancia de un tipo. que contiene `M`.</span><span class="sxs-lookup"><span data-stu-id="84145-868">When a method is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static method, `E` must denote a type containing `M`, and if `M` is an instance method, `E` must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="84145-869">Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="84145-869">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-methods"></a><span data-ttu-id="84145-870">Métodos virtuales</span><span class="sxs-lookup"><span data-stu-id="84145-870">Virtual methods</span></span>

<span data-ttu-id="84145-871">Cuando una declaración de método de instancia `virtual` incluye un modificador, se dice que se trata de un método virtual.</span><span class="sxs-lookup"><span data-stu-id="84145-871">When an instance method declaration includes a `virtual` modifier, that method is said to be a virtual method.</span></span> <span data-ttu-id="84145-872">Cuando no `virtual` existe ningún modificador, se dice que el método es un método no virtual.</span><span class="sxs-lookup"><span data-stu-id="84145-872">When no `virtual` modifier is present, the method is said to be a non-virtual method.</span></span>

<span data-ttu-id="84145-873">La implementación de un método no virtual es invariable: La implementación es la misma si se invoca el método en una instancia de la clase en la que se declara o una instancia de una clase derivada.</span><span class="sxs-lookup"><span data-stu-id="84145-873">The implementation of a non-virtual method is invariant: The implementation is the same whether the method is invoked on an instance of the class in which it is declared or an instance of a derived class.</span></span> <span data-ttu-id="84145-874">Por el contrario, las clases derivadas pueden reemplazar la implementación de un método virtual.</span><span class="sxs-lookup"><span data-stu-id="84145-874">In contrast, the implementation of a virtual method can be superseded by derived classes.</span></span> <span data-ttu-id="84145-875">El proceso de reemplazar la implementación de un método virtual heredado se conoce como ***invalidar*** ese método ([métodos de invalidación](classes.md#override-methods)).</span><span class="sxs-lookup"><span data-stu-id="84145-875">The process of superseding the implementation of an inherited virtual method is known as ***overriding*** that method ([Override methods](classes.md#override-methods)).</span></span>

<span data-ttu-id="84145-876">En una invocación de método virtual, el ***tipo en tiempo de ejecución*** de la instancia para la que tiene lugar la invocación determina la implementación de método real que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="84145-876">In a virtual method invocation, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="84145-877">En una invocación de método no virtual, el ***tipo en tiempo de compilación*** de la instancia es el factor determinante.</span><span class="sxs-lookup"><span data-stu-id="84145-877">In a non-virtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span> <span data-ttu-id="84145-878">En términos precisos, cuando se invoca `N` un método denominado con una lista `A` de argumentos en una instancia de con un tipo `C` en tiempo de compilación y un tipo `R` en tiempo `R` de `C` ejecución (donde es o una clase derivada de `C`), la invocación se procesa de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="84145-878">In precise terms, when a method named `N` is invoked with an argument list `A` on an instance with a compile-time type `C` and a run-time type `R` (where `R` is either `C` or a class derived from `C`), the invocation is processed as follows:</span></span>

*  <span data-ttu-id="84145-879">En primer lugar, la resolución de `C`sobrecarga `N`se aplica `A`a, y, para seleccionar `M` un método específico del conjunto de métodos declarados `C`en y heredados por.</span><span class="sxs-lookup"><span data-stu-id="84145-879">First, overload resolution is applied to `C`, `N`, and `A`, to select a specific method `M` from the set of methods declared in and inherited by `C`.</span></span> <span data-ttu-id="84145-880">Esto se describe en las [invocaciones de método](expressions.md#method-invocations).</span><span class="sxs-lookup"><span data-stu-id="84145-880">This is described in [Method invocations](expressions.md#method-invocations).</span></span>
*  <span data-ttu-id="84145-881">Después, si `M` es un método no virtual, `M` se invoca.</span><span class="sxs-lookup"><span data-stu-id="84145-881">Then, if `M` is a non-virtual method, `M` is invoked.</span></span>
*  <span data-ttu-id="84145-882">De lo `M` contrario, es un método virtual y se invoca la implementación `M` más derivada de `R` con respecto a.</span><span class="sxs-lookup"><span data-stu-id="84145-882">Otherwise, `M` is a virtual method, and the most derived implementation of `M` with respect to `R` is invoked.</span></span>

<span data-ttu-id="84145-883">Para cada método virtual declarado en o heredado por una clase, existe una ***implementación más derivada*** del método con respecto a esa clase.</span><span class="sxs-lookup"><span data-stu-id="84145-883">For every virtual method declared in or inherited by a class, there exists a ***most derived implementation*** of the method with respect to that class.</span></span> <span data-ttu-id="84145-884">La implementación más derivada de un método `M` virtual con respecto a una clase `R` se determina de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="84145-884">The most derived implementation of a virtual method `M` with respect to a class `R` is determined as follows:</span></span>

*  <span data-ttu-id="84145-885">Si `R` contiene la declaración `virtual` de introducción `M`de, ésta es la implementación más derivada de `M`.</span><span class="sxs-lookup"><span data-stu-id="84145-885">If `R` contains the introducing `virtual` declaration of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="84145-886">De lo contrario `R` , si `override` contiene `M`una de, esta es la implementación más derivada `M`de.</span><span class="sxs-lookup"><span data-stu-id="84145-886">Otherwise, if `R` contains an `override` of `M`, then this is the most derived implementation of `M`.</span></span>
*  <span data-ttu-id="84145-887">De lo contrario, la implementación más `M` derivada de con `R` respecto a es la misma que la implementación más `M` derivada de con respecto a la clase base `R`directa de.</span><span class="sxs-lookup"><span data-stu-id="84145-887">Otherwise, the most derived implementation of `M` with respect to `R` is the same as the most derived implementation of `M` with respect to the direct base class of `R`.</span></span>

<span data-ttu-id="84145-888">En el ejemplo siguiente se muestran las diferencias entre los métodos virtuales y los no virtuales:</span><span class="sxs-lookup"><span data-stu-id="84145-888">The following example illustrates the differences between virtual and non-virtual methods:</span></span>
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

<span data-ttu-id="84145-889">En el ejemplo, `A` introduce un método `F` no virtual y un método `G`virtual.</span><span class="sxs-lookup"><span data-stu-id="84145-889">In the example, `A` introduces a non-virtual method `F` and a virtual method `G`.</span></span> <span data-ttu-id="84145-890">La clase `B` introduce un nuevo método `F`no virtual, lo que oculta el heredado `F`, y también invalida el método `G`heredado.</span><span class="sxs-lookup"><span data-stu-id="84145-890">The class `B` introduces a new non-virtual method `F`, thus hiding the inherited `F`, and also overrides the inherited method `G`.</span></span> <span data-ttu-id="84145-891">En el ejemplo se genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="84145-891">The example produces the output:</span></span>
```console
A.F
B.F
B.G
B.G
```

<span data-ttu-id="84145-892">Observe que la instrucción `a.G()` `B.G`invoca, no `A.G`.</span><span class="sxs-lookup"><span data-stu-id="84145-892">Notice that the statement `a.G()` invokes `B.G`, not `A.G`.</span></span> <span data-ttu-id="84145-893">Esto se debe a que el tipo en tiempo de ejecución de la instancia `B`(que es), y no el tipo en tiempo de compilación de `A`la instancia (que es), determina la implementación de método real que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="84145-893">This is because the run-time type of the instance (which is `B`), not the compile-time type of the instance (which is `A`), determines the actual method implementation to invoke.</span></span>

<span data-ttu-id="84145-894">Dado que los métodos pueden ocultar métodos heredados, es posible que una clase contenga varios métodos virtuales con la misma firma.</span><span class="sxs-lookup"><span data-stu-id="84145-894">Because methods are allowed to hide inherited methods, it is possible for a class to contain several virtual methods with the same signature.</span></span> <span data-ttu-id="84145-895">Esto no presenta un problema de ambigüedad, ya que todo menos el método más derivado está oculto.</span><span class="sxs-lookup"><span data-stu-id="84145-895">This does not present an ambiguity problem, since all but the most derived method are hidden.</span></span> <span data-ttu-id="84145-896">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-896">In the example</span></span>
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
<span data-ttu-id="84145-897">las `C` clases `D` y contienen dos métodos virtuales con la misma firma: La introducida por `A` y la `C`introducida por.</span><span class="sxs-lookup"><span data-stu-id="84145-897">the `C` and `D` classes contain two virtual methods with the same signature: The one introduced by `A` and the one introduced by `C`.</span></span> <span data-ttu-id="84145-898">El método introducido por `C` oculta el método heredado de `A`.</span><span class="sxs-lookup"><span data-stu-id="84145-898">The method introduced by `C` hides the method inherited from `A`.</span></span> <span data-ttu-id="84145-899">Por lo tanto, la declaración `D` de invalidación en invalida el método `C`introducido por, y no es posible `D` que invalide el método `A`introducido por.</span><span class="sxs-lookup"><span data-stu-id="84145-899">Thus, the override declaration in `D` overrides the method introduced by `C`, and it is not possible for `D` to override the method introduced by `A`.</span></span> <span data-ttu-id="84145-900">En el ejemplo se genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="84145-900">The example produces the output:</span></span>
```console
B.F
B.F
D.F
D.F
```

<span data-ttu-id="84145-901">Tenga en cuenta que es posible invocar el método virtual oculto mediante el acceso a una `D` instancia de a través de un tipo menos derivado en el que el método no está oculto.</span><span class="sxs-lookup"><span data-stu-id="84145-901">Note that it is possible to invoke the hidden virtual method by accessing an instance of `D` through a less derived type in which the method is not hidden.</span></span>

### <a name="override-methods"></a><span data-ttu-id="84145-902">Métodos de invalidación</span><span class="sxs-lookup"><span data-stu-id="84145-902">Override methods</span></span>

<span data-ttu-id="84145-903">Cuando una declaración de método de instancia `override` incluye un modificador, se dice que el método es un ***método de invalidación***.</span><span class="sxs-lookup"><span data-stu-id="84145-903">When an instance method declaration includes an `override` modifier, the method is said to be an ***override method***.</span></span> <span data-ttu-id="84145-904">Un método de invalidación invalida un método virtual heredado con la misma firma.</span><span class="sxs-lookup"><span data-stu-id="84145-904">An override method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="84145-905">Mientras que una declaración de método virtual introduce un método nuevo, una declaración de método de reemplazo especializa un método virtual heredado existente proporcionando una nueva implementación de ese método.</span><span class="sxs-lookup"><span data-stu-id="84145-905">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="84145-906">El método invalidado por `override` una declaración se conoce como el ***método base invalidado***.</span><span class="sxs-lookup"><span data-stu-id="84145-906">The method overridden by an `override` declaration is known as the ***overridden base method***.</span></span> <span data-ttu-id="84145-907">Para un `M` método de invalidación declarado en `C`una clase, el método base invalidado se determina examinando cada tipo de `C`clase base de, empezando por el tipo de `C` clase base directa de y continuando con cada tipo de clase base directa, hasta que en un tipo de clase base determinado se encuentra al menos un método accesible que tiene la `M` misma firma que después de la sustitución de los argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-907">For an override method `M` declared in a class `C`, the overridden base method is determined by examining each base class type of `C`, starting with the direct base class type of `C` and continuing with each successive direct base class type, until in a given base class type at least one accessible method is located which has the same signature as `M` after substitution of type arguments.</span></span> <span data-ttu-id="84145-908">Con el fin de localizar el método base invalidado, se considera que un método es accesible si `public`es `protected` `protected internal`, si es, si es, o si es y se `internal` declara en el mismo programa que `C`.</span><span class="sxs-lookup"><span data-stu-id="84145-908">For the purposes of locating the overridden base method, a method is considered accessible if it is `public`, if it is `protected`, if it is `protected internal`, or if it is `internal` and declared in the same program as `C`.</span></span>

<span data-ttu-id="84145-909">Se produce un error en tiempo de compilación a menos que se cumplan todas las condiciones siguientes para una declaración de invalidación:</span><span class="sxs-lookup"><span data-stu-id="84145-909">A compile-time error occurs unless all of the following are true for an override declaration:</span></span>

*  <span data-ttu-id="84145-910">Un método base invalidado se puede encontrar como se describió anteriormente.</span><span class="sxs-lookup"><span data-stu-id="84145-910">An overridden base method can be located as described above.</span></span>
*  <span data-ttu-id="84145-911">Hay exactamente un método base invalidado de este tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-911">There is exactly one such overridden base method.</span></span> <span data-ttu-id="84145-912">Esta restricción solo tiene efecto si el tipo de clase base es un tipo construido en el que la sustitución de los argumentos de tipo hace que la firma de dos métodos sea la misma.</span><span class="sxs-lookup"><span data-stu-id="84145-912">This restriction has effect only if the base class type is a constructed type where the substitution of type arguments makes the signature of two methods the same.</span></span>
*  <span data-ttu-id="84145-913">El método base invalidado es un método virtual, abstracto o de invalidación.</span><span class="sxs-lookup"><span data-stu-id="84145-913">The overridden base method is a virtual, abstract, or override method.</span></span> <span data-ttu-id="84145-914">En otras palabras, el método base invalidado no puede ser estático o no virtual.</span><span class="sxs-lookup"><span data-stu-id="84145-914">In other words, the overridden base method cannot be static or non-virtual.</span></span>
*  <span data-ttu-id="84145-915">El método base invalidado no es un método sellado.</span><span class="sxs-lookup"><span data-stu-id="84145-915">The overridden base method is not a sealed method.</span></span>
*  <span data-ttu-id="84145-916">El método de invalidación y el método base invalidado tienen el mismo tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="84145-916">The override method and the overridden base method have the same return type.</span></span>
*  <span data-ttu-id="84145-917">La declaración de invalidación y el método base invalidado tienen la misma accesibilidad declarada.</span><span class="sxs-lookup"><span data-stu-id="84145-917">The override declaration and the overridden base method have the same declared accessibility.</span></span> <span data-ttu-id="84145-918">En otras palabras, una declaración de invalidación no puede cambiar la accesibilidad del método virtual.</span><span class="sxs-lookup"><span data-stu-id="84145-918">In other words, an override declaration cannot change the accessibility of the virtual method.</span></span> <span data-ttu-id="84145-919">Sin embargo, si el método base invalidado es interno protegido y se declara en un ensamblado diferente al del ensamblado que contiene el método de invalidación, se debe proteger la accesibilidad declarada del método de invalidación.</span><span class="sxs-lookup"><span data-stu-id="84145-919">However, if the overridden base method is protected internal and it is declared in a different assembly than the assembly containing the override method then the override method's declared accessibility must be protected.</span></span>
*  <span data-ttu-id="84145-920">La declaración de invalidación no especifica las cláusulas-de-parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-920">The override declaration does not specify type-parameter-constraints-clauses.</span></span> <span data-ttu-id="84145-921">En su lugar, las restricciones se heredan del método base invalidado.</span><span class="sxs-lookup"><span data-stu-id="84145-921">Instead the constraints are inherited from the overridden base method.</span></span> <span data-ttu-id="84145-922">Tenga en cuenta que las restricciones que son parámetros de tipo en el método invalidado se pueden reemplazar por argumentos de tipo en la restricción heredada.</span><span class="sxs-lookup"><span data-stu-id="84145-922">Note that constraints that are type parameters in the overridden method may be replaced by type arguments in the inherited constraint.</span></span> <span data-ttu-id="84145-923">Esto puede dar lugar a restricciones que no son válidas cuando se especifican explícitamente, como tipos de valor o tipos sellados.</span><span class="sxs-lookup"><span data-stu-id="84145-923">This can lead to constraints that are not legal when explicitly specified, such as value types or sealed types.</span></span>

<span data-ttu-id="84145-924">En el ejemplo siguiente se muestra cómo funcionan las reglas de reemplazo para las clases genéricas:</span><span class="sxs-lookup"><span data-stu-id="84145-924">The following example demonstrates how the overriding rules work for generic classes:</span></span>
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

<span data-ttu-id="84145-925">Una declaración de invalidación puede tener acceso al método base invalidado mediante una *base_access* ([acceso base](expressions.md#base-access)).</span><span class="sxs-lookup"><span data-stu-id="84145-925">An override declaration can access the overridden base method using a *base_access* ([Base access](expressions.md#base-access)).</span></span> <span data-ttu-id="84145-926">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-926">In the example</span></span>
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
<span data-ttu-id="84145-927">la `base.PrintFields()` invocación de `B` invoca el `PrintFields` método declarado en `A`.</span><span class="sxs-lookup"><span data-stu-id="84145-927">the `base.PrintFields()` invocation in `B` invokes the `PrintFields` method declared in `A`.</span></span> <span data-ttu-id="84145-928">Un *base_access* deshabilita el mecanismo de invocación virtual y simplemente trata el método base como un método no virtual.</span><span class="sxs-lookup"><span data-stu-id="84145-928">A *base_access* disables the virtual invocation mechanism and simply treats the base method as a non-virtual method.</span></span> <span data-ttu-id="84145-929">`B` Una vez escrita `PrintFields` `PrintFields` `A` `B`la invocación, invocaría de forma recursiva el método declarado en, no el declarado en, ya que es virtual y el tipo en tiempo de ejecución de `((A)this).PrintFields()` `((A)this)` es .`B`</span><span class="sxs-lookup"><span data-stu-id="84145-929">Had the invocation in `B` been written `((A)this).PrintFields()`, it would recursively invoke the `PrintFields` method declared in `B`, not the one declared in `A`, since `PrintFields` is virtual and the run-time type of `((A)this)` is `B`.</span></span>

<span data-ttu-id="84145-930">Solo si se incluye `override` un modificador, un método puede reemplazar a otro método.</span><span class="sxs-lookup"><span data-stu-id="84145-930">Only by including an `override` modifier can a method override another method.</span></span> <span data-ttu-id="84145-931">En todos los demás casos, un método con la misma signatura que un método heredado simplemente oculta el método heredado.</span><span class="sxs-lookup"><span data-stu-id="84145-931">In all other cases, a method with the same signature as an inherited method simply hides the inherited method.</span></span> <span data-ttu-id="84145-932">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-932">In the example</span></span>
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
<span data-ttu-id="84145-933">el `F` método de `B` no incluye un `override` modificador y, por tanto, no invalida `F` el método `A`en.</span><span class="sxs-lookup"><span data-stu-id="84145-933">the `F` method in `B` does not include an `override` modifier and therefore does not override the `F` method in `A`.</span></span> <span data-ttu-id="84145-934">En su lugar `F` , el `B` método en oculta el método `A`en y se genera una advertencia porque la declaración no incluye un `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-934">Rather, the `F` method in `B` hides the method in `A`, and a warning is reported because the declaration does not include a `new` modifier.</span></span>

<span data-ttu-id="84145-935">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-935">In the example</span></span>
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
<span data-ttu-id="84145-936">el `F` método en `B` oculta el método virtual `F` heredado de `A`.</span><span class="sxs-lookup"><span data-stu-id="84145-936">the `F` method in `B` hides the virtual `F` method inherited from `A`.</span></span> <span data-ttu-id="84145-937">Dado que el `F` nuevo `B` en tiene acceso privado, su ámbito solo incluye el cuerpo de `B` clase de y no se `C`extiende a.</span><span class="sxs-lookup"><span data-stu-id="84145-937">Since the new `F` in `B` has private access, its scope only includes the class body of `B` and does not extend to `C`.</span></span> <span data-ttu-id="84145-938">Por consiguiente, la declaración `F` de `C` en puede invalidar el `F` heredado `A`de.</span><span class="sxs-lookup"><span data-stu-id="84145-938">Therefore, the declaration of `F` in `C` is permitted to override the `F` inherited from `A`.</span></span>

### <a name="sealed-methods"></a><span data-ttu-id="84145-939">Métodos sellados</span><span class="sxs-lookup"><span data-stu-id="84145-939">Sealed methods</span></span>

<span data-ttu-id="84145-940">Cuando una declaración de método de instancia `sealed` incluye un modificador, se dice que se trata de un ***método sellado***.</span><span class="sxs-lookup"><span data-stu-id="84145-940">When an instance method declaration includes a `sealed` modifier, that method is said to be a ***sealed method***.</span></span> <span data-ttu-id="84145-941">Si una declaración de método de instancia `sealed` incluye el modificador, también debe incluir `override` el modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-941">If an instance method declaration includes the  `sealed` modifier, it must also include the `override` modifier.</span></span> <span data-ttu-id="84145-942">El `sealed` uso del modificador impide que una clase derivada Reemplace el método.</span><span class="sxs-lookup"><span data-stu-id="84145-942">Use of the `sealed` modifier prevents a derived class from further overriding the method.</span></span>

<span data-ttu-id="84145-943">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-943">In the example</span></span>
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
<span data-ttu-id="84145-944">la clase `B` proporciona dos métodos de invalidación `F` : un método que `sealed` tiene el modificador `G` y un método que no lo hace.</span><span class="sxs-lookup"><span data-stu-id="84145-944">the class `B` provides two override methods: an `F` method that has the `sealed` modifier and a `G` method that does not.</span></span> <span data-ttu-id="84145-945">`B`el uso del sellado `modifier` evita que `C` se invalide `F`más.</span><span class="sxs-lookup"><span data-stu-id="84145-945">`B`'s use of the sealed `modifier` prevents `C` from further overriding `F`.</span></span>

### <a name="abstract-methods"></a><span data-ttu-id="84145-946">Métodos abstractos</span><span class="sxs-lookup"><span data-stu-id="84145-946">Abstract methods</span></span>

<span data-ttu-id="84145-947">Cuando una declaración de método de instancia `abstract` incluye un modificador, se dice que el método es un ***método abstracto***.</span><span class="sxs-lookup"><span data-stu-id="84145-947">When an instance method declaration includes an `abstract` modifier, that method is said to be an ***abstract method***.</span></span> <span data-ttu-id="84145-948">Aunque un método abstracto también es implícitamente un método virtual, no puede tener el modificador `virtual`.</span><span class="sxs-lookup"><span data-stu-id="84145-948">Although an abstract method is implicitly also a virtual method, it cannot have the modifier `virtual`.</span></span>

<span data-ttu-id="84145-949">Una declaración de método abstracto presenta un nuevo método virtual, pero no proporciona una implementación de ese método.</span><span class="sxs-lookup"><span data-stu-id="84145-949">An abstract method declaration introduces a new virtual method but does not provide an implementation of that method.</span></span> <span data-ttu-id="84145-950">En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación mediante la invalidación de ese método.</span><span class="sxs-lookup"><span data-stu-id="84145-950">Instead, non-abstract derived classes are required to provide their own implementation by overriding that method.</span></span> <span data-ttu-id="84145-951">Dado que un método abstracto no proporciona ninguna implementación real, el *method_body* de un método abstracto simplemente consiste en un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-951">Because an abstract method provides no actual implementation, the *method_body* of an abstract method simply consists of a semicolon.</span></span>

<span data-ttu-id="84145-952">Las declaraciones de método abstracto solo se permiten en clases abstractas ([clases abstractas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="84145-952">Abstract method declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="84145-953">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-953">In the example</span></span>
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
<span data-ttu-id="84145-954">la `Shape` clase define la noción abstracta de un objeto de forma geométrica que puede dibujarse a sí mismo.</span><span class="sxs-lookup"><span data-stu-id="84145-954">the `Shape` class defines the abstract notion of a geometrical shape object that can paint itself.</span></span> <span data-ttu-id="84145-955">El `Paint` método es abstracto porque no hay ninguna implementación predeterminada significativa.</span><span class="sxs-lookup"><span data-stu-id="84145-955">The `Paint` method is abstract because there is no meaningful default implementation.</span></span> <span data-ttu-id="84145-956">Las `Ellipse` clases `Box` y son implementaciones concretas `Shape` .</span><span class="sxs-lookup"><span data-stu-id="84145-956">The `Ellipse` and `Box` classes are concrete `Shape` implementations.</span></span> <span data-ttu-id="84145-957">Dado que estas clases no son abstractas, son necesarias para invalidar `Paint` el método y proporcionar una implementación real.</span><span class="sxs-lookup"><span data-stu-id="84145-957">Because these classes are non-abstract, they are required to override the `Paint` method and provide an actual implementation.</span></span>

<span data-ttu-id="84145-958">Se trata de un error en tiempo de compilación para que una *base_access* ([acceso base](expressions.md#base-access)) haga referencia a un método abstracto.</span><span class="sxs-lookup"><span data-stu-id="84145-958">It is a compile-time error for a *base_access* ([Base access](expressions.md#base-access)) to reference an abstract method.</span></span> <span data-ttu-id="84145-959">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-959">In the example</span></span>
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
<span data-ttu-id="84145-960">se ha detectado un error en tiempo de compilación `base.F()` para la invocación porque hace referencia a un método abstracto.</span><span class="sxs-lookup"><span data-stu-id="84145-960">a compile-time error is reported for the `base.F()` invocation because it references an abstract method.</span></span>

<span data-ttu-id="84145-961">Se permite que una declaración de método abstracto invalide un método virtual.</span><span class="sxs-lookup"><span data-stu-id="84145-961">An abstract method declaration is permitted to override a virtual method.</span></span> <span data-ttu-id="84145-962">Esto permite a una clase abstracta forzar la reimplementación del método en las clases derivadas y hace que la implementación original del método no esté disponible.</span><span class="sxs-lookup"><span data-stu-id="84145-962">This allows an abstract class to force re-implementation of the method in derived classes, and makes the original implementation of the method unavailable.</span></span> <span data-ttu-id="84145-963">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-963">In the example</span></span>
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
<span data-ttu-id="84145-964">la `A` clase declara un método virtual, la `B` clase invalida este método con un método abstracto y la clase `C` invalida el método abstracto para proporcionar su propia implementación.</span><span class="sxs-lookup"><span data-stu-id="84145-964">class `A` declares a virtual method, class `B` overrides this method with an abstract method, and class `C` overrides the abstract method to provide its own implementation.</span></span>

### <a name="external-methods"></a><span data-ttu-id="84145-965">Métodos externos</span><span class="sxs-lookup"><span data-stu-id="84145-965">External methods</span></span>

<span data-ttu-id="84145-966">Cuando una declaración de método incluye `extern` un modificador, se dice que se trata de un ***método externo***.</span><span class="sxs-lookup"><span data-stu-id="84145-966">When a method declaration includes an `extern` modifier, that method is said to be an ***external method***.</span></span> <span data-ttu-id="84145-967">Los métodos externos se implementan externamente, normalmente con un lenguaje C#distinto de.</span><span class="sxs-lookup"><span data-stu-id="84145-967">External methods are implemented externally, typically using a language other than C#.</span></span> <span data-ttu-id="84145-968">Dado que una declaración de método externo no proporciona ninguna implementación real, el *method_body* de un método externo consiste simplemente en un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-968">Because an external method declaration provides no actual implementation, the *method_body* of an external method simply consists of a semicolon.</span></span> <span data-ttu-id="84145-969">Es posible que un método externo no sea genérico.</span><span class="sxs-lookup"><span data-stu-id="84145-969">An external method may not be generic.</span></span>

<span data-ttu-id="84145-970">El `extern` modificador se usa normalmente junto con un `DllImport` atributo ([interoperación con componentes com y Win32](attributes.md#interoperation-with-com-and-win32-components)), lo que permite que los métodos externos se implementen mediante archivos DLL (bibliotecas de vínculos dinámicos).</span><span class="sxs-lookup"><span data-stu-id="84145-970">The `extern` modifier is typically used in conjunction with a `DllImport` attribute ([Interoperation with COM and Win32 components](attributes.md#interoperation-with-com-and-win32-components)), allowing external methods to be implemented by DLLs (Dynamic Link Libraries).</span></span> <span data-ttu-id="84145-971">El entorno de ejecución puede admitir otros mecanismos en los que se puedan proporcionar implementaciones de métodos externos.</span><span class="sxs-lookup"><span data-stu-id="84145-971">The execution environment may support other mechanisms whereby implementations of external methods can be provided.</span></span>

<span data-ttu-id="84145-972">Cuando un método externo incluye un `DllImport` atributo, la declaración del método también debe incluir `static` un modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-972">When an external method includes a `DllImport` attribute, the method declaration must also include a `static` modifier.</span></span> <span data-ttu-id="84145-973">En este ejemplo se muestra el uso `extern` del modificador y `DllImport` el atributo:</span><span class="sxs-lookup"><span data-stu-id="84145-973">This example demonstrates the use of the `extern` modifier and the `DllImport` attribute:</span></span>
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

### <a name="partial-methods-recap"></a><span data-ttu-id="84145-974">Métodos parciales (Resumen)</span><span class="sxs-lookup"><span data-stu-id="84145-974">Partial methods (recap)</span></span>

<span data-ttu-id="84145-975">Cuando una declaración de método incluye `partial` un modificador, se dice que el método es un ***método parcial***.</span><span class="sxs-lookup"><span data-stu-id="84145-975">When a method declaration includes a `partial` modifier, that method is said to be a ***partial method***.</span></span> <span data-ttu-id="84145-976">Los métodos parciales solo se pueden declarar como miembros de tipos parciales ([tipos parciales](classes.md#partial-types)) y están sujetos a una serie de restricciones.</span><span class="sxs-lookup"><span data-stu-id="84145-976">Partial methods can only be declared as members of partial types ([Partial types](classes.md#partial-types)), and are subject to a number of restrictions.</span></span> <span data-ttu-id="84145-977">Los métodos parciales se describen con más detalle en [métodos parciales](classes.md#partial-methods).</span><span class="sxs-lookup"><span data-stu-id="84145-977">Partial methods are further described in [Partial methods](classes.md#partial-methods).</span></span>

### <a name="extension-methods"></a><span data-ttu-id="84145-978">Métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="84145-978">Extension methods</span></span>

<span data-ttu-id="84145-979">Cuando el primer parámetro de un método incluye el `this` modificador, se dice que se trata de un ***método de extensión***.</span><span class="sxs-lookup"><span data-stu-id="84145-979">When the first parameter of a method includes the `this` modifier, that method is said to be an ***extension method***.</span></span> <span data-ttu-id="84145-980">Los métodos de extensión solo se pueden declarar en clases estáticas no anidadas no genéricas.</span><span class="sxs-lookup"><span data-stu-id="84145-980">Extension methods can only be declared in non-generic, non-nested static classes.</span></span> <span data-ttu-id="84145-981">El primer parámetro de un método de extensión no puede tener modificadores distintos `this`de y el tipo de parámetro no puede ser un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="84145-981">The first parameter of an extension method can have no modifiers other than `this`, and the parameter type cannot be a pointer type.</span></span>

<span data-ttu-id="84145-982">El siguiente es un ejemplo de una clase estática que declara dos métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="84145-982">The following is an example of a static class that declares two extension methods:</span></span>
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

<span data-ttu-id="84145-983">Un método de extensión es un método estático normal.</span><span class="sxs-lookup"><span data-stu-id="84145-983">An extension method is a regular static method.</span></span> <span data-ttu-id="84145-984">Además, cuando la clase estática envolvente está en el ámbito, se puede invocar un método de extensión mediante la sintaxis de invocación de método de instancia ([invocaciones de método de extensión](expressions.md#extension-method-invocations)), mediante la expresión de receptor como primer argumento.</span><span class="sxs-lookup"><span data-stu-id="84145-984">In addition, where its enclosing static class is in scope, an extension method can be invoked using instance method invocation syntax ([Extension method invocations](expressions.md#extension-method-invocations)), using the receiver expression as the first argument.</span></span>

<span data-ttu-id="84145-985">El programa siguiente utiliza los métodos de extensión declarados anteriormente:</span><span class="sxs-lookup"><span data-stu-id="84145-985">The following program uses the extension methods declared above:</span></span>
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

<span data-ttu-id="84145-986">El `Slice` método está disponible `string[]`en, y el `ToInt32` método está disponible en `string`, porque se han declarado como métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="84145-986">The `Slice` method is available on the `string[]`, and the `ToInt32` method is available on `string`, because they have been declared as extension methods.</span></span> <span data-ttu-id="84145-987">El significado del programa es el mismo que el que se indica a continuación, mediante llamadas ordinarias al método estático:</span><span class="sxs-lookup"><span data-stu-id="84145-987">The meaning of the program is the same as the following, using ordinary static method calls:</span></span>
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

### <a name="method-body"></a><span data-ttu-id="84145-988">Cuerpo del método</span><span class="sxs-lookup"><span data-stu-id="84145-988">Method body</span></span>

<span data-ttu-id="84145-989">El *method_body* de una declaración de método consta de un cuerpo de bloque, un cuerpo de expresión o un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-989">The *method_body* of a method declaration consists of either a block body, an expression body or a semicolon.</span></span>

<span data-ttu-id="84145-990">El ***tipo de resultado*** de un método `void` es si el tipo de `void`valor devuelto es, o si el método es Async y `System.Threading.Tasks.Task`el tipo de valor devuelto es.</span><span class="sxs-lookup"><span data-stu-id="84145-990">The ***result type*** of a method is `void` if the return type is `void`, or if the method is async and the return type is `System.Threading.Tasks.Task`.</span></span> <span data-ttu-id="84145-991">De lo contrario, el tipo de resultado de un método no asincrónico es el tipo de valor devuelto y el tipo de resultado de un `System.Threading.Tasks.Task<T>` método `T`asincrónico con el tipo de valor devuelto es.</span><span class="sxs-lookup"><span data-stu-id="84145-991">Otherwise, the result type of a non-async method is its return type, and the result type of an async method with return type `System.Threading.Tasks.Task<T>` is `T`.</span></span>

<span data-ttu-id="84145-992">Cuando un método tiene un `void` tipo de resultado y un cuerpo de `return` bloque, las instrucciones ([la instrucción return](statements.md#the-return-statement)) del bloque no pueden especificar una expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-992">When a method has a `void` result type and a block body, `return` statements ([The return statement](statements.md#the-return-statement)) in the block are not permitted to specify an expression.</span></span> <span data-ttu-id="84145-993">Si la ejecución del bloque de un método void se completa normalmente (es decir, el control fluye fuera del final del cuerpo del método), ese método simplemente vuelve a su llamador actual.</span><span class="sxs-lookup"><span data-stu-id="84145-993">If execution of the block of a void method completes normally (that is, control flows off the end of the method body), that method simply returns to its current caller.</span></span>
    
<span data-ttu-id="84145-994">Cuando un método tiene un resultado `void` y un cuerpo de expresión, la expresión `E` debe ser *statement_expression*y el cuerpo es exactamente equivalente a un cuerpo de bloque con el formato `{ E; }`.</span><span class="sxs-lookup"><span data-stu-id="84145-994">When a method has a `void` result and an expression body, the expression `E` must be a *statement_expression*, and the body is exactly equivalent to a block body of the form `{ E; }`.</span></span>
    
<span data-ttu-id="84145-995">Cuando un método tiene un tipo de resultado no void y un cuerpo de bloque, `return` cada instrucción del bloque debe especificar una expresión que se pueda convertir implícitamente al tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="84145-995">When a method has a non-void result type and a block body, each `return` statement in the block must specify an expression that is implicitly convertible to the result type.</span></span> <span data-ttu-id="84145-996">No se puede tener acceso al extremo de un cuerpo de bloque de un método que devuelve un valor.</span><span class="sxs-lookup"><span data-stu-id="84145-996">The endpoint of a block body of a value-returning method must not be reachable.</span></span> <span data-ttu-id="84145-997">Es decir, en un método que devuelve valores con un cuerpo de bloque, el control no puede fluir fuera del final del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="84145-997">In other words, in a value-returning method with a block body, control is not permitted to flow off the end of the method body.</span></span>
    
<span data-ttu-id="84145-998">Cuando un método tiene un tipo de resultado no void y un cuerpo de expresión, la expresión se debe poder convertir implícitamente al tipo de resultado y el cuerpo es exactamente equivalente a un cuerpo de bloque del `{ return E; }`formulario.</span><span class="sxs-lookup"><span data-stu-id="84145-998">When a method has a non-void result type and an expression body, the expression must be implicitly convertible to the result type, and the body is exactly equivalent to a block body of the form `{ return E; }`.</span></span>
    
<span data-ttu-id="84145-999">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-999">In the example</span></span>
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
<span data-ttu-id="84145-1000">el método que devuelve `F` el valor produce un error en tiempo de compilación porque el control puede fluir fuera del final del cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="84145-1000">the value-returning `F` method results in a compile-time error because control can flow off the end of the method body.</span></span> <span data-ttu-id="84145-1001">Los `G` métodos `H` y son correctos porque todas las rutas de acceso de ejecución posibles terminan en una instrucción return que especifica un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="84145-1001">The `G` and `H` methods are correct because all possible execution paths end in a return statement that specifies a return value.</span></span> <span data-ttu-id="84145-1002">El `I` método es correcto porque su cuerpo es equivalente a un bloque de instrucciones con solo una única instrucción return en él.</span><span class="sxs-lookup"><span data-stu-id="84145-1002">The `I` method is correct, because its body is equivalent to a statement block with just a single return statement in it.</span></span>

### <a name="method-overloading"></a><span data-ttu-id="84145-1003">Sobrecarga de métodos</span><span class="sxs-lookup"><span data-stu-id="84145-1003">Method overloading</span></span>

<span data-ttu-id="84145-1004">Las reglas de resolución de sobrecarga de método se describen en [inferencia de tipos](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="84145-1004">The method overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="properties"></a><span data-ttu-id="84145-1005">Propiedades</span><span class="sxs-lookup"><span data-stu-id="84145-1005">Properties</span></span>

<span data-ttu-id="84145-1006">Una ***propiedad*** es un miembro que proporciona acceso a una característica de un objeto o una clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1006">A ***property*** is a member that provides access to a characteristic of an object or a class.</span></span> <span data-ttu-id="84145-1007">Entre los ejemplos de propiedades se incluyen la longitud de una cadena, el tamaño de una fuente, el título de una ventana, el nombre de un cliente, etc.</span><span class="sxs-lookup"><span data-stu-id="84145-1007">Examples of properties include the length of a string, the size of a font, the caption of a window, the name of a customer, and so on.</span></span> <span data-ttu-id="84145-1008">Las propiedades son una extensión natural de los campos: ambos son miembros con nombre con tipos asociados y la sintaxis para tener acceso a campos y propiedades es la misma.</span><span class="sxs-lookup"><span data-stu-id="84145-1008">Properties are a natural extension of fields—both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="84145-1009">Sin embargo, a diferencia de los campos, las propiedades no denotan ubicaciones de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="84145-1009">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="84145-1010">Las propiedades tienen ***descriptores de acceso*** que especifican las instrucciones que se ejecutan cuando se leen o escriben sus valores.</span><span class="sxs-lookup"><span data-stu-id="84145-1010">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span> <span data-ttu-id="84145-1011">Por tanto, las propiedades proporcionan un mecanismo para asociar acciones con la lectura y escritura de los atributos de un objeto; Además, permiten calcular tales atributos.</span><span class="sxs-lookup"><span data-stu-id="84145-1011">Properties thus provide a mechanism for associating actions with the reading and writing of an object's attributes; furthermore, they permit such attributes to be computed.</span></span>

<span data-ttu-id="84145-1012">Las propiedades se declaran mediante *property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="84145-1012">Properties are declared using *property_declaration*s:</span></span>

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

<span data-ttu-id="84145-1013">Un *property_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `static` ([estático y de instancia). métodos](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), 0 ([métodos de invalidación](classes.md#override-methods)), 2 ([métodos sellados](classes.md#sealed-methods)), 4 ([métodos abstractos](classes.md#abstract-methods)) y modificadores 6 ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="84145-1013">A *property_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="84145-1014">Las declaraciones de propiedad están sujetas a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores.</span><span class="sxs-lookup"><span data-stu-id="84145-1014">Property declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="84145-1015">El *tipo* de una declaración de propiedad especifica el tipo de la propiedad introducida por la declaración y *member_name* especifica el nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1015">The *type* of a property declaration specifies the type of the property introduced by the declaration, and the *member_name* specifies the name of the property.</span></span> <span data-ttu-id="84145-1016">A menos que la propiedad sea una implementación explícita de un miembro de interfaz, *member_name* es simplemente un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="84145-1016">Unless the property is an explicit interface member implementation, the *member_name* is simply an *identifier*.</span></span> <span data-ttu-id="84145-1017">En el caso de una implementación explícita de un miembro de interfaz ([implementaciones de miembros de interfaz explícitos](interfaces.md#explicit-interface-member-implementations)), *member_name* consta de un *interface_type* seguido de un "`.`" y un *identificador*.</span><span class="sxs-lookup"><span data-stu-id="84145-1017">For an explicit interface member implementation ([Explicit interface member implementations](interfaces.md#explicit-interface-member-implementations)), the *member_name* consists of an *interface_type* followed by a "`.`" and an *identifier*.</span></span>

<span data-ttu-id="84145-1018">El *tipo* de una propiedad debe ser al menos igual de accesible que la propiedad en sí ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="84145-1018">The *type* of a property must be at least as accessible as the property itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="84145-1019">Un *property_body* puede estar formado por un ***cuerpo de descriptor de acceso*** o un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="84145-1019">A *property_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="84145-1020">En el cuerpo de un descriptor de acceso, *accessor_declarations*, que se debe incluir en los tokens "`{`" y "`}`", declare los descriptores de acceso ([descriptores de acceso](classes.md#accessors)) de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1020">In an accessor body,  *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="84145-1021">Los descriptores de acceso especifican las instrucciones ejecutables asociadas a la lectura y la escritura de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1021">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="84145-1022">Un cuerpo de expresión que consta `=>` de seguido de una *expresión* `E` y un punto y coma es exactamente equivalente al cuerpo `{ get { return E; } }`de la instrucción y, por tanto, solo se puede usar para especificar propiedades de solo captador en las que el resultado de el captador se proporciona mediante una sola expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-1022">An expression body consisting of `=>` followed by an *expression* `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only properties where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="84145-1023">Un *property_initializer* solo se puede proporcionar para una propiedad implementada automáticamente ([propiedades implementadas automáticamente](classes.md#automatically-implemented-properties)) y provoca la inicialización del campo subyacente de dichas propiedades con el valor dado por la *expresión.* .</span><span class="sxs-lookup"><span data-stu-id="84145-1023">A *property_initializer* may only be given for an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)), and causes the initialization of the underlying field of such properties with the value given by the *expression*.</span></span>

<span data-ttu-id="84145-1024">Aunque la sintaxis para tener acceso a una propiedad es la misma que la de un campo, una propiedad no se clasifica como una variable.</span><span class="sxs-lookup"><span data-stu-id="84145-1024">Even though the syntax for accessing a property is the same as that for a field, a property is not classified as a variable.</span></span> <span data-ttu-id="84145-1025">Por lo tanto, no es posible pasar una propiedad como `ref` argumento o. `out`</span><span class="sxs-lookup"><span data-stu-id="84145-1025">Thus, it is not possible to pass a property as a `ref` or `out` argument.</span></span>

<span data-ttu-id="84145-1026">Cuando una declaración de propiedad incluye `extern` un modificador, se dice que la propiedad es una ***propiedad externa***.</span><span class="sxs-lookup"><span data-stu-id="84145-1026">When a property declaration includes an `extern` modifier, the property is said to be an ***external property***.</span></span> <span data-ttu-id="84145-1027">Dado que una declaración de propiedad externa no proporciona ninguna implementación real, cada una de sus *accessor_declarations* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-1027">Because an external property declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

### <a name="static-and-instance-properties"></a><span data-ttu-id="84145-1028">Propiedades estáticas y de instancia</span><span class="sxs-lookup"><span data-stu-id="84145-1028">Static and instance properties</span></span>

<span data-ttu-id="84145-1029">Cuando una declaración de propiedad incluye `static` un modificador, se dice que la propiedad es una ***propiedad estática***.</span><span class="sxs-lookup"><span data-stu-id="84145-1029">When a property declaration includes a `static` modifier, the property is said to be a ***static property***.</span></span> <span data-ttu-id="84145-1030">Cuando no `static` existe ningún modificador, se dice que la propiedad es una ***propiedad de instancia***.</span><span class="sxs-lookup"><span data-stu-id="84145-1030">When no `static` modifier is present, the property is said to be an ***instance property***.</span></span>

<span data-ttu-id="84145-1031">Una propiedad estática no está asociada a una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en los descriptores de acceso de una propiedad estática.</span><span class="sxs-lookup"><span data-stu-id="84145-1031">A static property is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static property.</span></span>

<span data-ttu-id="84145-1032">Una propiedad de instancia está asociada a una instancia determinada de una clase y se puede tener acceso a esa instancia `this` como ([este acceso](expressions.md#this-access)) en los descriptores de acceso de esa propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1032">An instance property is associated with a given instance of a class, and that instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that property.</span></span>

<span data-ttu-id="84145-1033">Cuando se hace referencia a una propiedad en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, si `M` es una propiedad estática, `E` debe indicar un tipo que contiene `M` y, si `M` es una propiedad de instancia, E debe denotar una instancia de un tipo. que contiene `M`.</span><span class="sxs-lookup"><span data-stu-id="84145-1033">When a property is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static property, `E` must denote a type containing `M`, and if `M` is an instance property, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="84145-1034">Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="84145-1034">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="accessors"></a><span data-ttu-id="84145-1035">Descriptores</span><span class="sxs-lookup"><span data-stu-id="84145-1035">Accessors</span></span>

<span data-ttu-id="84145-1036">El *accessor_declarations* de una propiedad especifica las instrucciones ejecutables asociadas a la lectura y escritura de esa propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1036">The *accessor_declarations* of a property specify the executable statements associated with reading and writing that property.</span></span>

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

<span data-ttu-id="84145-1037">Las declaraciones de descriptor de acceso constan de un *get_accessor_declaration*, un *set_accessor_declaration*o ambos.</span><span class="sxs-lookup"><span data-stu-id="84145-1037">The accessor declarations consist of a *get_accessor_declaration*, a *set_accessor_declaration*, or both.</span></span> <span data-ttu-id="84145-1038">Cada declaración de descriptor de acceso consta del token `get` o `set` seguido de un *accessor_modifier* opcional y un *accessor_body*.</span><span class="sxs-lookup"><span data-stu-id="84145-1038">Each accessor declaration consists of the token `get` or `set` followed by an optional *accessor_modifier* and an *accessor_body*.</span></span>

<span data-ttu-id="84145-1039">El uso de *accessor_modifier*s se rige por las siguientes restricciones:</span><span class="sxs-lookup"><span data-stu-id="84145-1039">The use of *accessor_modifier*s is governed by the following restrictions:</span></span>

*  <span data-ttu-id="84145-1040">Un *accessor_modifier* no se puede usar en una interfaz o en una implementación explícita de un miembro de interfaz.</span><span class="sxs-lookup"><span data-stu-id="84145-1040">An *accessor_modifier* may not be used in an interface or in an explicit interface member implementation.</span></span>
*  <span data-ttu-id="84145-1041">En el caso de una propiedad o un indizador que no tenga un modificador `override`, solo se permite una *accessor_modifier* si la propiedad o indizador tiene un descriptor de acceso `get` y `set` y, a continuación, solo se permite en uno de estos descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="84145-1041">For a property or indexer that has no `override` modifier, an *accessor_modifier* is permitted only if the property or indexer has both a `get` and `set` accessor, and then is permitted only on one of those accessors.</span></span>
*  <span data-ttu-id="84145-1042">En el caso de una propiedad o un indizador que incluya un modificador `override`, un descriptor de acceso debe coincidir con el *accessor_modifier*, si existe, del descriptor de acceso que se va a invalidar.</span><span class="sxs-lookup"><span data-stu-id="84145-1042">For a property or indexer that includes an `override` modifier, an accessor must match the *accessor_modifier*, if any, of the accessor being overridden.</span></span>
*  <span data-ttu-id="84145-1043">*Accessor_modifier* debe declarar una accesibilidad que sea estrictamente más restrictiva que la accesibilidad declarada de la propiedad o el indexador.</span><span class="sxs-lookup"><span data-stu-id="84145-1043">The *accessor_modifier* must declare an accessibility that is strictly more restrictive than the declared accessibility of the property or indexer itself.</span></span> <span data-ttu-id="84145-1044">Para ser precisos:</span><span class="sxs-lookup"><span data-stu-id="84145-1044">To be precise:</span></span>
   * <span data-ttu-id="84145-1045">Si la propiedad o indizador tiene una accesibilidad declarada de `public`, el valor de *accessor_modifier* puede ser `protected internal`, `internal`, `protected` o `private`.</span><span class="sxs-lookup"><span data-stu-id="84145-1045">If the property or indexer has a declared accessibility of `public`, the *accessor_modifier* may be either `protected internal`, `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="84145-1046">Si la propiedad o indizador tiene una accesibilidad declarada de `protected internal`, el valor de *accessor_modifier* puede ser `internal`, `protected` o `private`.</span><span class="sxs-lookup"><span data-stu-id="84145-1046">If the property or indexer has a declared accessibility of `protected internal`, the *accessor_modifier* may be either `internal`, `protected`, or `private`.</span></span>
   * <span data-ttu-id="84145-1047">Si la propiedad o indizador tiene una accesibilidad declarada de `internal` o `protected`, *accessor_modifier* debe ser `private`.</span><span class="sxs-lookup"><span data-stu-id="84145-1047">If the property or indexer has a declared accessibility of `internal` or `protected`, the *accessor_modifier* must be `private`.</span></span>
   * <span data-ttu-id="84145-1048">Si la propiedad o indizador tiene una accesibilidad declarada de `private`, no se puede usar ningún *accessor_modifier* .</span><span class="sxs-lookup"><span data-stu-id="84145-1048">If the property or indexer has a declared accessibility of `private`, no *accessor_modifier* may be used.</span></span>

<span data-ttu-id="84145-1049">En el caso de las propiedades `abstract` y `extern`, el valor de *accessor_body* para cada descriptor de acceso especificado es simplemente un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-1049">For `abstract` and `extern` properties, the *accessor_body* for each accessor specified is simply a semicolon.</span></span> <span data-ttu-id="84145-1050">Una propiedad no abstracta y no externa puede tener cada *accessor_body* en punto y coma, en cuyo caso es una ***propiedad implementada automáticamente*** ([propiedades implementadas automáticamente](classes.md#automatically-implemented-properties)).</span><span class="sxs-lookup"><span data-stu-id="84145-1050">A non-abstract, non-extern property may have each *accessor_body* be a semicolon, in which case it is an ***automatically implemented property*** ([Automatically implemented properties](classes.md#automatically-implemented-properties)).</span></span> <span data-ttu-id="84145-1051">Una propiedad implementada automáticamente debe tener al menos un descriptor de acceso get.</span><span class="sxs-lookup"><span data-stu-id="84145-1051">An automatically implemented property must have at least a get accessor.</span></span> <span data-ttu-id="84145-1052">En el caso de los descriptores de acceso de cualquier otra propiedad no abstracta no externa, *accessor_body* es un *bloque* que especifica las instrucciones que se van a ejecutar cuando se invoca el descriptor de acceso correspondiente.</span><span class="sxs-lookup"><span data-stu-id="84145-1052">For the accessors of any other non-abstract, non-extern property, the *accessor_body* is a *block* which specifies the statements to be executed when the corresponding accessor is invoked.</span></span>

<span data-ttu-id="84145-1053">Un `get` descriptor de acceso corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1053">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="84145-1054">Excepto como destino de una asignación, cuando se hace referencia a una propiedad en una expresión, el `get` descriptor de acceso de la propiedad se invoca para calcular el valor de la propiedad ([valores de las expresiones](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="84145-1054">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property ([Values of expressions](expressions.md#values-of-expressions)).</span></span> <span data-ttu-id="84145-1055">El cuerpo de un `get` descriptor de acceso debe cumplir las reglas de los métodos que devuelven valores descritos en el [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="84145-1055">The body of a `get` accessor must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="84145-1056">En concreto, todas `return` las instrucciones del cuerpo de un `get` descriptor de acceso deben especificar una expresión que se pueda convertir implícitamente al tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1056">In particular, all `return` statements in the body of a `get` accessor must specify an expression that is implicitly convertible to the property type.</span></span> <span data-ttu-id="84145-1057">Además, el punto de conexión `get` de un descriptor de acceso no debe ser accesible.</span><span class="sxs-lookup"><span data-stu-id="84145-1057">Furthermore, the endpoint of a `get` accessor must not be reachable.</span></span>

<span data-ttu-id="84145-1058">Un `set` descriptor de acceso corresponde a un método con un parámetro de valor único del `void` tipo de propiedad y un tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="84145-1058">A `set` accessor corresponds to a method with a single value parameter of the property type and a `void` return type.</span></span> <span data-ttu-id="84145-1059">El parámetro implícito de un `set` descriptor de `value`acceso siempre se denomina.</span><span class="sxs-lookup"><span data-stu-id="84145-1059">The implicit parameter of a `set` accessor is always named `value`.</span></span> <span data-ttu-id="84145-1060">Cuando se hace referencia a una propiedad como el destino de una asignación[(operadores de asignación](expressions.md#assignment-operators)) o como operando `++` de `--` o ([operadores de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) [postfijo, operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)), el el descriptor de acceso se invoca con un argumento (cuyo valor es el del lado derecho de la asignación o el operando `++` del `--` operador o) que proporciona el nuevo valor ([asignación simple](expressions.md#simple-assignment)). `set`</span><span class="sxs-lookup"><span data-stu-id="84145-1060">When a property is referenced as the target of an assignment ([Assignment operators](expressions.md#assignment-operators)), or as the operand of `++` or `--` ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators), [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), the `set` accessor is invoked with an argument (whose value is that of the right-hand side of the assignment or the operand of the `++` or `--` operator) that provides the new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="84145-1061">El cuerpo de un `set` descriptor de acceso debe cumplir `void` las reglas de los métodos descritos en [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="84145-1061">The body of a `set` accessor must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="84145-1062">En concreto, `return` las instrucciones del `set` cuerpo del descriptor de acceso no pueden especificar una expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-1062">In particular, `return` statements in the `set` accessor body are not permitted to specify an expression.</span></span> <span data-ttu-id="84145-1063">Dado que `set` un descriptor de acceso tiene `value`implícitamente un parámetro denominado, se trata de un error en tiempo de compilación para una `set` variable local o una declaración de constante en un descriptor de acceso que tiene ese nombre.</span><span class="sxs-lookup"><span data-stu-id="84145-1063">Since a `set` accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declaration in a `set` accessor to have that name.</span></span>

<span data-ttu-id="84145-1064">En función de la presencia o ausencia de `get` los `set` descriptores de acceso y, una propiedad se clasifica de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="84145-1064">Based on the presence or absence of the `get` and `set` accessors, a property is classified as follows:</span></span>

*  <span data-ttu-id="84145-1065">Se dice que una propiedad que `get` incluye tanto un `set` descriptor de acceso como un descriptor de acceso es una propiedad ***de lectura y escritura*** .</span><span class="sxs-lookup"><span data-stu-id="84145-1065">A property that includes both a `get` accessor and a `set` accessor is said to be a ***read-write*** property.</span></span>
*  <span data-ttu-id="84145-1066">Se dice que una propiedad que `get` solo tiene un descriptor de acceso es una propiedad ***de solo lectura*** .</span><span class="sxs-lookup"><span data-stu-id="84145-1066">A property that has only a `get` accessor is said to be a ***read-only*** property.</span></span> <span data-ttu-id="84145-1067">Es un error en tiempo de compilación que una propiedad de solo lectura sea el destino de una asignación.</span><span class="sxs-lookup"><span data-stu-id="84145-1067">It is a compile-time error for a read-only property to be the target of an assignment.</span></span>
*  <span data-ttu-id="84145-1068">Se dice que una propiedad que `set` solo tiene un descriptor de acceso es una propiedad ***de solo escritura*** .</span><span class="sxs-lookup"><span data-stu-id="84145-1068">A property that has only a `set` accessor is said to be a ***write-only*** property.</span></span> <span data-ttu-id="84145-1069">Excepto como destino de una asignación, se trata de un error en tiempo de compilación para hacer referencia a una propiedad de solo escritura en una expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-1069">Except as the target of an assignment, it is a compile-time error to reference a write-only property in an expression.</span></span>

<span data-ttu-id="84145-1070">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1070">In the example</span></span>
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
<span data-ttu-id="84145-1071">el `Button` control declara una propiedad pública `Caption` .</span><span class="sxs-lookup"><span data-stu-id="84145-1071">the `Button` control declares a public `Caption` property.</span></span> <span data-ttu-id="84145-1072">El `get` descriptor `Caption` de acceso de la propiedad devuelve la cadena `caption` almacenada en el campo privado.</span><span class="sxs-lookup"><span data-stu-id="84145-1072">The `get` accessor of the `Caption` property returns the string stored in the private `caption` field.</span></span> <span data-ttu-id="84145-1073">El `set` descriptor de acceso comprueba si el nuevo valor es diferente del valor actual y, en ese caso, almacena el nuevo valor y vuelve a dibujar el control.</span><span class="sxs-lookup"><span data-stu-id="84145-1073">The `set` accessor checks if the new value is different from the current value, and if so, it stores the new value and repaints the control.</span></span> <span data-ttu-id="84145-1074">Las propiedades suelen seguir el patrón mostrado anteriormente: El `get` descriptor de acceso devuelve simplemente un valor almacenado en un campo `set` privado y el descriptor de acceso modifica ese campo privado y, a continuación, realiza las acciones adicionales necesarias para actualizar por completo el estado del objeto.</span><span class="sxs-lookup"><span data-stu-id="84145-1074">Properties often follow the pattern shown above: The `get` accessor simply returns a value stored in a private field, and the `set` accessor modifies that private field and then performs any additional actions required to fully update the state of the object.</span></span>

<span data-ttu-id="84145-1075">Dada la `Caption` clase anterior, el siguiente es un ejemplo de uso de la propiedad: `Button`</span><span class="sxs-lookup"><span data-stu-id="84145-1075">Given the `Button` class above, the following is an example of use of the `Caption` property:</span></span>
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

<span data-ttu-id="84145-1076">Aquí, el `set` descriptor de acceso se invoca mediante la asignación de un valor a la propiedad `get` , y el descriptor de acceso se invoca mediante la referencia a la propiedad en una expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-1076">Here, the `set` accessor is invoked by assigning a value to the property, and the `get` accessor is invoked by referencing the property in an expression.</span></span>

<span data-ttu-id="84145-1077">Los `get` descriptores de acceso y `set` de una propiedad no son miembros distintos y no es posible declarar los descriptores de acceso de una propiedad por separado.</span><span class="sxs-lookup"><span data-stu-id="84145-1077">The `get` and `set` accessors of a property are not distinct members, and it is not possible to declare the accessors of a property separately.</span></span> <span data-ttu-id="84145-1078">Como tal, no es posible que los dos descriptores de acceso de una propiedad de lectura y escritura tengan una accesibilidad diferente.</span><span class="sxs-lookup"><span data-stu-id="84145-1078">As such, it is not possible for the two accessors of a read-write property to have different accessibility.</span></span> <span data-ttu-id="84145-1079">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1079">The example</span></span>
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
<span data-ttu-id="84145-1080">no declara una única propiedad de lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="84145-1080">does not declare a single read-write property.</span></span> <span data-ttu-id="84145-1081">En su lugar, declara dos propiedades con el mismo nombre, una de solo lectura y otra de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="84145-1081">Rather, it declares two properties with the same name, one read-only and one write-only.</span></span> <span data-ttu-id="84145-1082">Dado que dos miembros declarados en la misma clase no pueden tener el mismo nombre, en el ejemplo se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-1082">Since two members declared in the same class cannot have the same name, the example causes a compile-time error to occur.</span></span>

<span data-ttu-id="84145-1083">Cuando una clase derivada declara una propiedad con el mismo nombre que una propiedad heredada, la propiedad derivada oculta la propiedad heredada con respecto a la lectura y la escritura.</span><span class="sxs-lookup"><span data-stu-id="84145-1083">When a derived class declares a property by the same name as an inherited property, the derived property hides the inherited property with respect to both reading and writing.</span></span> <span data-ttu-id="84145-1084">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1084">In the example</span></span>
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
<span data-ttu-id="84145-1085">la `P` propiedad de `B` oculta la propiedad `P` en `A` con respecto a la lectura y la escritura.</span><span class="sxs-lookup"><span data-stu-id="84145-1085">the `P` property in `B` hides the `P` property in `A` with respect to both reading and writing.</span></span> <span data-ttu-id="84145-1086">Por lo tanto, en las instrucciones</span><span class="sxs-lookup"><span data-stu-id="84145-1086">Thus, in the statements</span></span>
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
<span data-ttu-id="84145-1087">la asignación a `b.P` produce un error en tiempo de compilación, ya que la propiedad de solo `P` lectura de `B` oculta la propiedad de solo `P` escritura en `A`.</span><span class="sxs-lookup"><span data-stu-id="84145-1087">the assignment to `b.P` causes a compile-time error to be reported, since the read-only `P` property in `B` hides the write-only `P` property in `A`.</span></span> <span data-ttu-id="84145-1088">Sin embargo, tenga en cuenta que se puede usar una conversión para tener `P` acceso a la propiedad Hidden.</span><span class="sxs-lookup"><span data-stu-id="84145-1088">Note, however, that a cast can be used to access the hidden `P` property.</span></span>

<span data-ttu-id="84145-1089">A diferencia de los campos públicos, las propiedades proporcionan una separación entre el estado interno de un objeto y su interfaz pública.</span><span class="sxs-lookup"><span data-stu-id="84145-1089">Unlike public fields, properties provide a separation between an object's internal state and its public interface.</span></span> <span data-ttu-id="84145-1090">Considere el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-1090">Consider the example:</span></span>
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

<span data-ttu-id="84145-1091">En este caso `Label` , la clase `int` usa dos `x` campos `y`, y, para almacenar su ubicación.</span><span class="sxs-lookup"><span data-stu-id="84145-1091">Here, the `Label` class uses two `int` fields, `x` and `y`, to store its location.</span></span> <span data-ttu-id="84145-1092">La ubicación se expone `X` públicamente como y una `Y` propiedad y como `Location` propiedad de tipo `Point`.</span><span class="sxs-lookup"><span data-stu-id="84145-1092">The location is publicly exposed both as an `X` and a `Y` property and as a `Location` property of type `Point`.</span></span> <span data-ttu-id="84145-1093">Si, en una versión futura de `Label`, resulta más cómodo almacenar la ubicación `Point` internamente, el cambio se puede realizar sin que afecte a la interfaz pública de la clase:</span><span class="sxs-lookup"><span data-stu-id="84145-1093">If, in a future version of `Label`, it becomes more convenient to store the location as a `Point` internally, the change can be made without affecting the public interface of the class:</span></span>
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

<span data-ttu-id="84145-1094">Los campos `y` tenían `x` y en su lugar `Label` , habría sido imposible realizar este cambio en la clase. `public readonly`</span><span class="sxs-lookup"><span data-stu-id="84145-1094">Had `x` and `y` instead been `public readonly` fields, it would have been impossible to make such a change to the `Label` class.</span></span>

<span data-ttu-id="84145-1095">Exponer el estado a través de las propiedades no es necesariamente menos eficaz que exponer los campos directamente.</span><span class="sxs-lookup"><span data-stu-id="84145-1095">Exposing state through properties is not necessarily any less efficient than exposing fields directly.</span></span> <span data-ttu-id="84145-1096">En concreto, cuando una propiedad no es virtual y contiene solo una pequeña cantidad de código, el entorno de ejecución puede reemplazar las llamadas a los descriptores de acceso con el código real de los descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="84145-1096">In particular, when a property is non-virtual and contains only a small amount of code, the execution environment may replace calls to accessors with the actual code of the accessors.</span></span> <span data-ttu-id="84145-1097">Este proceso se ***conoce como inserción y hace***que el acceso a la propiedad sea tan eficaz como el acceso al campo, pero conserva la mayor flexibilidad de las propiedades.</span><span class="sxs-lookup"><span data-stu-id="84145-1097">This process is known as ***inlining***, and it makes property access as efficient as field access, yet preserves the increased flexibility of properties.</span></span>

<span data-ttu-id="84145-1098">Dado que la invocación de un descriptor de acceso es conceptualmente equivalente a leer el valor de un campo, `get` se considera un `get` estilo de programación incorrecto para que los descriptores de acceso tengan efectos secundarios observables.</span><span class="sxs-lookup"><span data-stu-id="84145-1098">Since invoking a `get` accessor is conceptually equivalent to reading the value of a field, it is considered bad programming style for `get` accessors to have observable side-effects.</span></span> <span data-ttu-id="84145-1099">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1099">In the example</span></span>
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
<span data-ttu-id="84145-1100">el valor de la `Next` propiedad depende del número de veces que se ha accedido previamente a la propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1100">the value of the `Next` property depends on the number of times the property has previously been accessed.</span></span> <span data-ttu-id="84145-1101">Por lo tanto, el acceso a la propiedad produce un efecto secundario observable y la propiedad debe implementarse como un método en su lugar.</span><span class="sxs-lookup"><span data-stu-id="84145-1101">Thus, accessing the property produces an observable side-effect, and the property should be implemented as a method instead.</span></span>

<span data-ttu-id="84145-1102">La Convención "sin efectos secundarios" para los `get` descriptores de acceso `get` no significa que los descriptores de acceso siempre se deben escribir para devolver los valores almacenados en los campos.</span><span class="sxs-lookup"><span data-stu-id="84145-1102">The "no side-effects" convention for `get` accessors doesn't mean that `get` accessors should always be written to simply return values stored in fields.</span></span> <span data-ttu-id="84145-1103">En realidad `get` , los descriptores de acceso suelen calcular el valor de una propiedad mediante el acceso a varios campos o la invocación de métodos.</span><span class="sxs-lookup"><span data-stu-id="84145-1103">Indeed, `get` accessors often compute the value of a property by accessing multiple fields or invoking methods.</span></span> <span data-ttu-id="84145-1104">Sin embargo, un descriptor de acceso diseñado `get` correctamente no realiza ninguna acción que produzca cambios observables en el estado del objeto.</span><span class="sxs-lookup"><span data-stu-id="84145-1104">However, a properly designed `get` accessor performs no actions that cause observable changes in the state of the object.</span></span>

<span data-ttu-id="84145-1105">Las propiedades se pueden usar para retrasar la inicialización de un recurso hasta el momento en el que se hace referencia a él por primera vez.</span><span class="sxs-lookup"><span data-stu-id="84145-1105">Properties can be used to delay initialization of a resource until the moment it is first referenced.</span></span> <span data-ttu-id="84145-1106">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-1106">For example:</span></span>
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

<span data-ttu-id="84145-1107">La `Console` clase contiene tres propiedades `Out`, `In`, y `Error`, que representan los dispositivos de entrada, salida y error estándar, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="84145-1107">The `Console` class contains three properties, `In`, `Out`, and `Error`, that represent the standard input, output, and error devices, respectively.</span></span> <span data-ttu-id="84145-1108">Al exponer estos miembros como propiedades, la `Console` clase puede retrasar su inicialización hasta que se usen realmente.</span><span class="sxs-lookup"><span data-stu-id="84145-1108">By exposing these members as properties, the `Console` class can delay their initialization until they are actually used.</span></span> <span data-ttu-id="84145-1109">Por ejemplo, al hacer referencia por `Out` primera vez a la propiedad, como en</span><span class="sxs-lookup"><span data-stu-id="84145-1109">For example, upon first referencing the `Out` property, as in</span></span>
```csharp
Console.Out.WriteLine("hello, world");
```
<span data-ttu-id="84145-1110">se crea `TextWriter` el subyacente para el dispositivo de salida.</span><span class="sxs-lookup"><span data-stu-id="84145-1110">the underlying `TextWriter` for the output device is created.</span></span> <span data-ttu-id="84145-1111">Pero si la aplicación no hace referencia a las `In` propiedades `Error` y, no se crea ningún objeto para esos dispositivos.</span><span class="sxs-lookup"><span data-stu-id="84145-1111">But if the application makes no reference to the `In` and `Error` properties, then no objects are created for those devices.</span></span>

### <a name="automatically-implemented-properties"></a><span data-ttu-id="84145-1112">Propiedades implementadas automáticamente</span><span class="sxs-lookup"><span data-stu-id="84145-1112">Automatically implemented properties</span></span>

<span data-ttu-id="84145-1113">Una propiedad implementada automáticamente (o ***propiedad automática*** para abreviar) es una propiedad no abstracta no abstracta con cuerpos de descriptor de acceso solo de punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-1113">An automatically implemented property (or ***auto-property*** for short), is a non-abstract non-extern property with semicolon-only accessor bodies.</span></span> <span data-ttu-id="84145-1114">Las propiedades automáticas deben tener un descriptor de acceso get y, opcionalmente, tener un descriptor de acceso set.</span><span class="sxs-lookup"><span data-stu-id="84145-1114">Auto-properties must have a get accessor and can optionally have a set accessor.</span></span>

<span data-ttu-id="84145-1115">Cuando se especifica una propiedad como una propiedad implementada automáticamente, un campo de respaldo oculto está disponible automáticamente para la propiedad y los descriptores de acceso se implementan para leer y escribir en ese campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="84145-1115">When a property is specified as an automatically implemented property, a hidden backing field is automatically available for the property, and the accessors are implemented to read from and write to that backing field.</span></span> <span data-ttu-id="84145-1116">Si la propiedad automática no tiene ningún descriptor de acceso set, se `readonly` considera el campo de respaldo ([campos de solo lectura](classes.md#readonly-fields)).</span><span class="sxs-lookup"><span data-stu-id="84145-1116">If the auto-property has no set accessor, the backing field is considered `readonly` ([Readonly fields](classes.md#readonly-fields)).</span></span> <span data-ttu-id="84145-1117">Al igual que `readonly` un campo, una propiedad automática de solo captador también se puede asignar a en el cuerpo de un constructor de la clase envolvente.</span><span class="sxs-lookup"><span data-stu-id="84145-1117">Just like a `readonly` field, a getter-only auto-property can also be assigned to in the body of a constructor of the enclosing class.</span></span> <span data-ttu-id="84145-1118">Este tipo de asignación asigna directamente al campo de respaldo de solo lectura de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1118">Such an assignment assigns directly to the readonly backing field of the property.</span></span>

<span data-ttu-id="84145-1119">Una propiedad automática puede tener opcionalmente un *property_initializer*, que se aplica directamente al campo de respaldo como un *variable_initializer* ([inicializadores de variables](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="84145-1119">An auto-property may optionally have a *property_initializer*, which is applied directly to the backing field as a *variable_initializer* ([Variable initializers](classes.md#variable-initializers)).</span></span>

<span data-ttu-id="84145-1120">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="84145-1120">The following example:</span></span>
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
<span data-ttu-id="84145-1121">es equivalente a la siguiente declaración:</span><span class="sxs-lookup"><span data-stu-id="84145-1121">is equivalent to the following declaration:</span></span>
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

<span data-ttu-id="84145-1122">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="84145-1122">The following example:</span></span>
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
<span data-ttu-id="84145-1123">es equivalente a la siguiente declaración:</span><span class="sxs-lookup"><span data-stu-id="84145-1123">is equivalent to the following declaration:</span></span>
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

<span data-ttu-id="84145-1124">Observe que las asignaciones al campo de solo lectura son válidas, ya que se producen dentro del constructor.</span><span class="sxs-lookup"><span data-stu-id="84145-1124">Notice that the assignments to the readonly field are legal, because they occur within the constructor.</span></span>


### <a name="accessibility"></a><span data-ttu-id="84145-1125">Accesibilidad</span><span class="sxs-lookup"><span data-stu-id="84145-1125">Accessibility</span></span>

<span data-ttu-id="84145-1126">Si un descriptor de acceso tiene un *accessor_modifier*, el dominio de accesibilidad ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)) del descriptor de acceso se determina mediante la accesibilidad declarada de *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="84145-1126">If an accessor has an *accessor_modifier*, the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the accessor is determined using the declared accessibility of the *accessor_modifier*.</span></span> <span data-ttu-id="84145-1127">Si un descriptor de acceso no tiene un *accessor_modifier*, el dominio de accesibilidad del descriptor de acceso se determina a partir de la accesibilidad declarada de la propiedad o del indizador.</span><span class="sxs-lookup"><span data-stu-id="84145-1127">If an accessor does not have an *accessor_modifier*, the accessibility domain of the accessor is determined from the declared accessibility of the property or indexer.</span></span>

<span data-ttu-id="84145-1128">La presencia de *accessor_modifier* nunca afecta a la búsqueda de miembros ([operadores](expressions.md#operators)) o a la resolución de sobrecarga ([resolución de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="84145-1128">The presence of an *accessor_modifier* never affects member lookup ([Operators](expressions.md#operators)) or overload resolution ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="84145-1129">Los modificadores de la propiedad o el indizador siempre determinan la propiedad o el indizador que está enlazado, independientemente del contexto del acceso.</span><span class="sxs-lookup"><span data-stu-id="84145-1129">The modifiers on the property or indexer always determine which property or indexer is bound to, regardless of the context of the access.</span></span>

<span data-ttu-id="84145-1130">Una vez que se ha seleccionado una propiedad o un indizador determinados, se usan los dominios de accesibilidad de los descriptores de acceso específicos implicados para determinar si ese uso es válido:</span><span class="sxs-lookup"><span data-stu-id="84145-1130">Once a particular property or indexer has been selected, the accessibility domains of the specific accessors involved are used to determine if that usage is valid:</span></span>

*  <span data-ttu-id="84145-1131">Si el uso es como un valor ([valores de expresiones](expressions.md#values-of-expressions)), el `get` descriptor de acceso debe existir y ser accesible.</span><span class="sxs-lookup"><span data-stu-id="84145-1131">If the usage is as a value ([Values of expressions](expressions.md#values-of-expressions)), the `get` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="84145-1132">Si el uso es como el destino de una asignación simple ([asignación simple](expressions.md#simple-assignment)), el `set` descriptor de acceso debe existir y ser accesible.</span><span class="sxs-lookup"><span data-stu-id="84145-1132">If the usage is as the target of a simple assignment ([Simple assignment](expressions.md#simple-assignment)), the `set` accessor must exist and be accessible.</span></span>
*  <span data-ttu-id="84145-1133">Si el uso es como destino de la asignación compuesta ([asignación compuesta](expressions.md#compound-assignment)) o como destino de los `++` operadores o `--` (miembros de[función](expressions.md#function-members)0,9, expresiones de `get` [invocación](expressions.md#invocation-expressions)), ambos descriptores de acceso y el `set` descriptor de acceso debe existir y ser accesible.</span><span class="sxs-lookup"><span data-stu-id="84145-1133">If the usage is as the target of compound assignment ([Compound assignment](expressions.md#compound-assignment)), or as the target of the `++` or `--` operators ([Function members](expressions.md#function-members).9, [Invocation expressions](expressions.md#invocation-expressions)), both the `get` accessors and the `set` accessor must exist and be accessible.</span></span>

<span data-ttu-id="84145-1134">En el ejemplo siguiente, `A.Text` la propiedad oculta `B.Text`la propiedad, incluso en contextos donde solo se llama al `set` descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="84145-1134">In the following example, the property `A.Text` is hidden by the property `B.Text`, even in contexts where only the `set` accessor is called.</span></span> <span data-ttu-id="84145-1135">En cambio, la propiedad `B.Count` no es accesible a la `M`clase, por lo que `A.Count` en su lugar se usa la propiedad accesible.</span><span class="sxs-lookup"><span data-stu-id="84145-1135">In contrast, the property `B.Count` is not accessible to class `M`, so the accessible property `A.Count` is used instead.</span></span>

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

<span data-ttu-id="84145-1136">Un descriptor de acceso que se usa para implementar una interfaz no puede tener un *accessor_modifier*.</span><span class="sxs-lookup"><span data-stu-id="84145-1136">An accessor that is used to implement an interface may not have an *accessor_modifier*.</span></span> <span data-ttu-id="84145-1137">Si solo se usa un descriptor de acceso para implementar una interfaz, el otro descriptor de acceso puede declararse con un *accessor_modifier*:</span><span class="sxs-lookup"><span data-stu-id="84145-1137">If only one accessor is used to implement an interface, the other accessor may be declared with an *accessor_modifier*:</span></span>
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

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a><span data-ttu-id="84145-1138">Descriptores de acceso de propiedad virtual, Sealed, override y Abstract</span><span class="sxs-lookup"><span data-stu-id="84145-1138">Virtual, sealed, override, and abstract property accessors</span></span>

<span data-ttu-id="84145-1139">Una `virtual` declaración de propiedad especifica que los descriptores de acceso de la propiedad son virtuales.</span><span class="sxs-lookup"><span data-stu-id="84145-1139">A `virtual` property declaration specifies that the accessors of the property are virtual.</span></span> <span data-ttu-id="84145-1140">El `virtual` modificador se aplica a ambos descriptores de acceso de una propiedad de lectura y escritura, no es posible que solo un descriptor de acceso de una propiedad de lectura y escritura sea virtual.</span><span class="sxs-lookup"><span data-stu-id="84145-1140">The `virtual` modifier applies to both accessors of a read-write property—it is not possible for only one accessor of a read-write property to be virtual.</span></span>

<span data-ttu-id="84145-1141">Una `abstract` declaración de propiedad especifica que los descriptores de acceso de la propiedad son virtuales, pero no proporciona una implementación real de los descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="84145-1141">An `abstract` property declaration specifies that the accessors of the property are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="84145-1142">En su lugar, se requiere que las clases derivadas no abstractas proporcionen su propia implementación para los descriptores de acceso mediante la invalidación de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1142">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the property.</span></span> <span data-ttu-id="84145-1143">Dado que un descriptor de acceso de una declaración de propiedad abstracta no proporciona ninguna implementación real, su *accessor_body* simplemente consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-1143">Because an accessor for an abstract property declaration provides no actual implementation, its *accessor_body* simply consists of a semicolon.</span></span>

<span data-ttu-id="84145-1144">Una declaración de propiedad que incluye los `abstract` modificadores y `override` especifica que la propiedad es abstracta e invalida una propiedad base.</span><span class="sxs-lookup"><span data-stu-id="84145-1144">A property declaration that includes both the `abstract` and `override` modifiers specifies that the property is abstract and overrides a base property.</span></span> <span data-ttu-id="84145-1145">Los descriptores de acceso de dicha propiedad también son abstractos.</span><span class="sxs-lookup"><span data-stu-id="84145-1145">The accessors of such a property are also abstract.</span></span>

<span data-ttu-id="84145-1146">Las declaraciones de propiedad abstracta solo se permiten en clases abstractas ([clases abstractas](classes.md#abstract-classes)). Los descriptores de acceso de una propiedad virtual heredada se pueden invalidar en una clase derivada mediante la inclusión de `override` una declaración de propiedad que especifica una directiva.</span><span class="sxs-lookup"><span data-stu-id="84145-1146">Abstract property declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).The accessors of an inherited virtual property can be overridden in a derived class by including a property declaration that specifies an `override` directive.</span></span> <span data-ttu-id="84145-1147">Esto se conoce como una ***declaración de propiedad de reemplazo***.</span><span class="sxs-lookup"><span data-stu-id="84145-1147">This is known as an ***overriding property declaration***.</span></span> <span data-ttu-id="84145-1148">Una declaración de propiedad de reemplazo no declara una nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1148">An overriding property declaration does not declare a new property.</span></span> <span data-ttu-id="84145-1149">En su lugar, simplemente especializa las implementaciones de los descriptores de acceso de una propiedad virtual existente.</span><span class="sxs-lookup"><span data-stu-id="84145-1149">Instead, it simply specializes the implementations of the accessors of an existing virtual property.</span></span>

<span data-ttu-id="84145-1150">Una declaración de propiedad de reemplazo debe especificar exactamente los mismos modificadores de accesibilidad, tipo y nombre que la propiedad heredada.</span><span class="sxs-lookup"><span data-stu-id="84145-1150">An overriding property declaration must specify the exact same accessibility modifiers, type, and name as the inherited property.</span></span> <span data-ttu-id="84145-1151">Si la propiedad heredada solo tiene un descriptor de acceso (es decir, si la propiedad heredada es de solo lectura o de solo escritura), la propiedad de reemplazo solo debe incluir ese descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="84145-1151">If the inherited property has only a single accessor (i.e., if the inherited property is read-only or write-only), the overriding property must include only that accessor.</span></span> <span data-ttu-id="84145-1152">Si la propiedad heredada incluye ambos descriptores de acceso (es decir, si la propiedad heredada es de lectura y escritura), la propiedad de reemplazo puede incluir un solo descriptor de acceso o ambos.</span><span class="sxs-lookup"><span data-stu-id="84145-1152">If the inherited property includes both accessors (i.e., if the inherited property is read-write), the overriding property can include either a single accessor or both accessors.</span></span>

<span data-ttu-id="84145-1153">Una declaración de propiedad de reemplazo puede incluir `sealed` el modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-1153">An overriding property declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="84145-1154">El uso de este modificador evita que una clase derivada Reemplace la propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1154">Use of this modifier prevents a derived class from further overriding the property.</span></span> <span data-ttu-id="84145-1155">Los descriptores de acceso de una propiedad sellada también están sellados.</span><span class="sxs-lookup"><span data-stu-id="84145-1155">The accessors of a sealed property are also sealed.</span></span>

<span data-ttu-id="84145-1156">A excepción de las diferencias en la sintaxis de declaración e invocación, los descriptores de acceso virtual, sellado, invalidación y Abstract se comportan exactamente igual que los métodos virtuales, sellados, de invalidación y abstractos.</span><span class="sxs-lookup"><span data-stu-id="84145-1156">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="84145-1157">En concreto, las reglas descritas en [métodos virtuales](classes.md#virtual-methods), [métodos de invalidación](classes.md#override-methods), [métodos sellados](classes.md#sealed-methods)y [métodos abstractos](classes.md#abstract-methods) se aplican como si los descriptores de acceso fueran métodos de una forma correspondiente:</span><span class="sxs-lookup"><span data-stu-id="84145-1157">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form:</span></span>

*  <span data-ttu-id="84145-1158">Un `get` descriptor de acceso corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad y los mismos modificadores que la propiedad que lo contiene.</span><span class="sxs-lookup"><span data-stu-id="84145-1158">A `get` accessor corresponds to a parameterless method with a return value of the property type and the same modifiers as the containing property.</span></span>
*  <span data-ttu-id="84145-1159">Un `set` descriptor de acceso corresponde a un método con un parámetro de valor único del `void` tipo de propiedad, un tipo de valor devuelto y los mismos modificadores que la propiedad que lo contiene.</span><span class="sxs-lookup"><span data-stu-id="84145-1159">A `set` accessor corresponds to a method with a single value parameter of the property type, a `void` return type, and the same modifiers as the containing property.</span></span>

<span data-ttu-id="84145-1160">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1160">In the example</span></span>
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
<span data-ttu-id="84145-1161">`X`es una propiedad virtual de solo lectura, `Y` es una propiedad de lectura y escritura virtual y `Z` es una propiedad de lectura y escritura abstracta.</span><span class="sxs-lookup"><span data-stu-id="84145-1161">`X` is a virtual read-only property, `Y` is a virtual read-write property, and `Z` is an abstract read-write property.</span></span> <span data-ttu-id="84145-1162">Dado `Z` que es abstracto, la `A` clase contenedora también debe declararse como abstracta.</span><span class="sxs-lookup"><span data-stu-id="84145-1162">Because `Z` is abstract, the containing class `A` must also be declared abstract.</span></span>

<span data-ttu-id="84145-1163">Una clase que deriva de `A` se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="84145-1163">A class that derives from `A` is show below:</span></span>
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

<span data-ttu-id="84145-1164">Aquí, las declaraciones de, `X` `Y`y `Z` son las declaraciones de propiedad de reemplazo.</span><span class="sxs-lookup"><span data-stu-id="84145-1164">Here, the declarations of `X`, `Y`, and `Z` are overriding property declarations.</span></span> <span data-ttu-id="84145-1165">Cada declaración de propiedad coincide exactamente con los modificadores de accesibilidad, el tipo y el nombre de la propiedad heredada correspondiente.</span><span class="sxs-lookup"><span data-stu-id="84145-1165">Each property declaration exactly matches the accessibility modifiers, type, and name of the corresponding inherited property.</span></span> <span data-ttu-id="84145-1166">El `get` descriptor de `set` acceso de `Y` `X` y el `base` descriptor de acceso de utilizan la palabra clave para tener acceso a los descriptores de acceso</span><span class="sxs-lookup"><span data-stu-id="84145-1166">The `get` accessor of `X` and the `set` accessor of `Y` use the `base` keyword to access the inherited accessors.</span></span> <span data-ttu-id="84145-1167">La declaración de `Z` invalida ambos descriptores de acceso abstractos; por tanto, no hay miembros de función `B`abstracta pendientes `B` en y se permite que sea una clase no abstracta.</span><span class="sxs-lookup"><span data-stu-id="84145-1167">The declaration of `Z` overrides both abstract accessors—thus, there are no outstanding abstract function members in `B`, and `B` is permitted to be a non-abstract class.</span></span>

<span data-ttu-id="84145-1168">Cuando una propiedad se declara como `override`, cualquier descriptor de acceso invalidado debe ser accesible para el código de invalidación.</span><span class="sxs-lookup"><span data-stu-id="84145-1168">When a property is declared as an `override`, any overridden accessors must be accessible to the overriding code.</span></span> <span data-ttu-id="84145-1169">Además, la accesibilidad declarada de la propiedad o del indexador, y de los descriptores de acceso, debe coincidir con la del miembro y los descriptores de acceso invalidados.</span><span class="sxs-lookup"><span data-stu-id="84145-1169">In addition, the declared accessibility of both the property or indexer itself, and of the accessors, must match that of the overridden member and accessors.</span></span> <span data-ttu-id="84145-1170">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-1170">For example:</span></span>
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

## <a name="events"></a><span data-ttu-id="84145-1171">Events</span><span class="sxs-lookup"><span data-stu-id="84145-1171">Events</span></span>

<span data-ttu-id="84145-1172">Un ***evento*** es un miembro que permite a un objeto o una clase proporcionar notificaciones.</span><span class="sxs-lookup"><span data-stu-id="84145-1172">An ***event*** is a member that enables an object or class to provide notifications.</span></span> <span data-ttu-id="84145-1173">Los clientes pueden adjuntar código ejecutable para eventos proporcionando ***controladores de eventos***.</span><span class="sxs-lookup"><span data-stu-id="84145-1173">Clients can attach executable code for events by supplying ***event handlers***.</span></span>

<span data-ttu-id="84145-1174">Los eventos se declaran mediante *event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="84145-1174">Events are declared using *event_declaration*s:</span></span>

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

<span data-ttu-id="84145-1175">Un *event_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `static` ([estático y de instancia). métodos](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), 0 ([métodos de invalidación](classes.md#override-methods)), 2 ([métodos sellados](classes.md#sealed-methods)), 4 ([métodos abstractos](classes.md#abstract-methods)) y modificadores 6 ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="84145-1175">An *event_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)),  `static` ([Static and instance methods](classes.md#static-and-instance-methods)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="84145-1176">Las declaraciones de eventos están sujetas a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores.</span><span class="sxs-lookup"><span data-stu-id="84145-1176">Event declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers.</span></span>

<span data-ttu-id="84145-1177">El *tipo* de una declaración de evento debe ser *delegate_type* ([tipos de referencia](types.md#reference-types)) y ese *delegate_type* debe ser al menos igual de accesible que el propio evento ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="84145-1177">The *type* of an event declaration must be a *delegate_type* ([Reference types](types.md#reference-types)), and that *delegate_type* must be at least as accessible as the event itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="84145-1178">Una declaración de evento puede incluir *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="84145-1178">An event declaration may include *event_accessor_declarations*.</span></span> <span data-ttu-id="84145-1179">Sin embargo, si no, para los eventos no abstractos y no externos, el compilador los proporciona automáticamente ([eventos similares a los campos](classes.md#field-like-events)); en el caso de los eventos extern, los descriptores de acceso se proporcionan externamente.</span><span class="sxs-lookup"><span data-stu-id="84145-1179">However, if it does not, for non-extern, non-abstract events, the compiler supplies them automatically ([Field-like events](classes.md#field-like-events)); for extern events, the accessors are provided externally.</span></span>

<span data-ttu-id="84145-1180">Una declaración de evento que omite *event_accessor_declarations* define uno o varios eventos, uno para cada uno de los *variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="84145-1180">An event declaration that omits *event_accessor_declarations* defines one or more events—one for each of the *variable_declarator*s.</span></span> <span data-ttu-id="84145-1181">Los atributos y modificadores se aplican a todos los miembros declarados por este tipo de *event_declaration*.</span><span class="sxs-lookup"><span data-stu-id="84145-1181">The attributes and modifiers apply to all of the members declared by such an *event_declaration*.</span></span>

<span data-ttu-id="84145-1182">Se trata de un error en tiempo de compilación para que un *event_declaration* incluya el modificador `abstract` y el *event_accessor_declarations*delimitado por llaves.</span><span class="sxs-lookup"><span data-stu-id="84145-1182">It is a compile-time error for an *event_declaration* to include both the `abstract` modifier and brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="84145-1183">Cuando una declaración de evento incluye `extern` un modificador, se dice que se trata de un ***evento externo***.</span><span class="sxs-lookup"><span data-stu-id="84145-1183">When an event declaration includes an `extern` modifier, the event is said to be an ***external event***.</span></span> <span data-ttu-id="84145-1184">Dado que una declaración de evento externo no proporciona ninguna implementación real, es un error que incluya el modificador `extern` y *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="84145-1184">Because an external event declaration provides no actual implementation, it is an error for it to include both the `extern` modifier and *event_accessor_declarations*.</span></span>

<span data-ttu-id="84145-1185">Se trata de un error en tiempo de compilación para un *variable_declarator* de una declaración de evento con un modificador `abstract` o `external` para incluir un *variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="84145-1185">It is a compile-time error for a *variable_declarator* of an event declaration with an `abstract` or `external` modifier to include a *variable_initializer*.</span></span>

<span data-ttu-id="84145-1186">Un evento se puede usar como operando izquierdo de los `+=` operadores y `-=` (asignación de[eventos](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="84145-1186">An event can be used as the left-hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="84145-1187">Estos operadores se utilizan, respectivamente, para adjuntar controladores de eventos a o para quitar controladores de eventos de un evento, y los modificadores de acceso del evento controlan los contextos en los que se permiten esas operaciones.</span><span class="sxs-lookup"><span data-stu-id="84145-1187">These operators are used, respectively, to attach event handlers to or to remove event handlers from an event, and the access modifiers of the event control the contexts in which such operations are permitted.</span></span>

<span data-ttu-id="84145-1188">Puesto `+=` que `-=` y son las únicas operaciones que se permiten en un evento fuera del tipo que declara el evento, el código externo puede Agregar y quitar controladores para un evento, pero no puede obtener o modificar la lista subyacente de eventos. Controladores.</span><span class="sxs-lookup"><span data-stu-id="84145-1188">Since `+=` and `-=` are the only operations that are permitted on an event outside the type that declares the event, external code can add and remove handlers for an event, but cannot in any other way obtain or modify the underlying list of event handlers.</span></span>

<span data-ttu-id="84145-1189">En una operación de la forma `x += y` o `x -= y`, cuando `x` es un evento y la referencia tiene lugar fuera del tipo que contiene la declaración de `x`, el resultado de la operación tiene el `void` tipo (en lugar de tener tipo de `x`, con el valor de después `x` de la asignación.</span><span class="sxs-lookup"><span data-stu-id="84145-1189">In an operation of the form `x += y` or `x -= y`, when `x` is an event and the reference takes place outside the type that contains the declaration of `x`, the result of the operation has type `void` (as opposed to having the type of `x`, with the value of `x` after the assignment).</span></span> <span data-ttu-id="84145-1190">Esta regla prohíbe que el código externo examine indirectamente el delegado subyacente de un evento.</span><span class="sxs-lookup"><span data-stu-id="84145-1190">This rule prohibits external code from indirectly examining the underlying delegate of an event.</span></span>

<span data-ttu-id="84145-1191">En el ejemplo siguiente se muestra cómo se adjuntan los controladores de `Button` eventos a las instancias de la clase:</span><span class="sxs-lookup"><span data-stu-id="84145-1191">The following example shows how event handlers are attached to instances of the `Button` class:</span></span>
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

<span data-ttu-id="84145-1192">En este caso `LoginDialog` , el constructor de `Button` instancia crea dos instancias y asocia los controladores de `Click` eventos a los eventos.</span><span class="sxs-lookup"><span data-stu-id="84145-1192">Here, the `LoginDialog` instance constructor creates two `Button` instances and attaches event handlers to the `Click` events.</span></span>

### <a name="field-like-events"></a><span data-ttu-id="84145-1193">Eventos similares a campos</span><span class="sxs-lookup"><span data-stu-id="84145-1193">Field-like events</span></span>

<span data-ttu-id="84145-1194">En el texto del programa de la clase o el struct que contiene la declaración de un evento, se pueden usar determinados eventos como campos.</span><span class="sxs-lookup"><span data-stu-id="84145-1194">Within the program text of the class or struct that contains the declaration of an event, certain events can be used like fields.</span></span> <span data-ttu-id="84145-1195">Para usar de esta manera, un evento no debe ser `abstract` ni `extern`, y no debe incluir explícitamente *event_accessor_declarations*.</span><span class="sxs-lookup"><span data-stu-id="84145-1195">To be used in this way, an event must not be `abstract` or `extern`, and must not explicitly include *event_accessor_declarations*.</span></span> <span data-ttu-id="84145-1196">Este tipo de evento se puede utilizar en cualquier contexto que permita un campo.</span><span class="sxs-lookup"><span data-stu-id="84145-1196">Such an event can be used in any context that permits a field.</span></span> <span data-ttu-id="84145-1197">El campo contiene un delegado ([delegados](delegates.md)) que hace referencia a la lista de controladores de eventos que se han agregado al evento.</span><span class="sxs-lookup"><span data-stu-id="84145-1197">The field contains a delegate ([Delegates](delegates.md)) which refers to the list of event handlers that have been added to the event.</span></span> <span data-ttu-id="84145-1198">Si no se ha agregado ningún controlador de eventos, el campo `null`contiene.</span><span class="sxs-lookup"><span data-stu-id="84145-1198">If no event handlers have been added, the field contains `null`.</span></span>

<span data-ttu-id="84145-1199">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1199">In the example</span></span>
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
<span data-ttu-id="84145-1200">`Click`se usa como un campo dentro de `Button` la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1200">`Click` is used as a field within the `Button` class.</span></span> <span data-ttu-id="84145-1201">Como muestra el ejemplo, el campo se puede examinar, modificar y usar en expresiones de invocación de delegado.</span><span class="sxs-lookup"><span data-stu-id="84145-1201">As the example demonstrates, the field can be examined, modified, and used in delegate invocation expressions.</span></span> <span data-ttu-id="84145-1202">El `OnClick` método de la `Button` clase "genera" el `Click` evento.</span><span class="sxs-lookup"><span data-stu-id="84145-1202">The `OnClick` method in the `Button` class "raises" the `Click` event.</span></span> <span data-ttu-id="84145-1203">La noción de generar un evento es equivalente exactamente a invocar el delegado representado por el evento; por lo tanto, no hay ninguna construcción especial de lenguaje para generar eventos.</span><span class="sxs-lookup"><span data-stu-id="84145-1203">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span> <span data-ttu-id="84145-1204">Tenga en cuenta que la invocación del delegado está precedida por una comprobación que garantiza que el delegado no es NULL.</span><span class="sxs-lookup"><span data-stu-id="84145-1204">Note that the delegate invocation is preceded by a check that ensures the delegate is non-null.</span></span>

<span data-ttu-id="84145-1205">Fuera de la declaración de `Button` la clase, `Click` el miembro solo se puede usar en el lado izquierdo de los `+=` operadores y `-=` , como en</span><span class="sxs-lookup"><span data-stu-id="84145-1205">Outside the declaration of the `Button` class, the `Click` member can only be used on the left-hand side of the `+=` and `-=` operators, as in</span></span>
```csharp
b.Click += new EventHandler(...);
```
<span data-ttu-id="84145-1206">que anexa un delegado a la lista de invocaciones del `Click` evento, y</span><span class="sxs-lookup"><span data-stu-id="84145-1206">which appends a delegate to the invocation list of the `Click` event, and</span></span>
```csharp
b.Click -= new EventHandler(...);
```
<span data-ttu-id="84145-1207">que quita un delegado de la lista de invocación del `Click` evento.</span><span class="sxs-lookup"><span data-stu-id="84145-1207">which removes a delegate from the invocation list of the `Click` event.</span></span>

<span data-ttu-id="84145-1208">Al compilar un evento como un campo, el compilador crea automáticamente el almacenamiento para contener el delegado y crea los descriptores de acceso para el evento que agregan o quitan controladores de eventos para el campo delegado.</span><span class="sxs-lookup"><span data-stu-id="84145-1208">When compiling a field-like event, the compiler automatically creates storage to hold the delegate, and creates accessors for the event that add or remove event handlers to the delegate field.</span></span> <span data-ttu-id="84145-1209">Las operaciones de adición y eliminación son seguras para subprocesos y puede (pero no es necesario que) se realicen mientras se mantiene el bloqueo ([la instrucción lock](statements.md#the-lock-statement)) en el objeto contenedor para un evento de instancia o el objeto Type ([expresiones de creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions)). para un evento estático.</span><span class="sxs-lookup"><span data-stu-id="84145-1209">The addition and removal operations are thread safe, and may (but are not required to) be done while holding the lock ([The lock statement](statements.md#the-lock-statement)) on the containing object for an instance event, or the type object ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) for a static event.</span></span>

<span data-ttu-id="84145-1210">Por lo tanto, una declaración de evento de instancia con el formato:</span><span class="sxs-lookup"><span data-stu-id="84145-1210">Thus, an instance event declaration of the form:</span></span>
```csharp
class X
{
    public event D Ev;
}
```
<span data-ttu-id="84145-1211">se compilará en algo equivalente a:</span><span class="sxs-lookup"><span data-stu-id="84145-1211">will be compiled to something equivalent to:</span></span>
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
<span data-ttu-id="84145-1212">Dentro de la `X`clase, las `Ev` referencias a en el lado izquierdo de los `+=` operadores `-=` y hacen que se invoquen los descriptores de acceso add y Remove.</span><span class="sxs-lookup"><span data-stu-id="84145-1212">Within the class `X`, references to `Ev` on the left-hand side of the `+=` and `-=` operators cause the add and remove accessors to be invoked.</span></span> <span data-ttu-id="84145-1213">Todas las demás referencias `Ev` a se compilan para hacer `__Ev` referencia al campo oculto en su lugar ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="84145-1213">All other references to `Ev` are compiled to reference the hidden field `__Ev` instead ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="84145-1214">El nombre "`__Ev`" es arbitrario; el campo oculto puede tener cualquier nombre o ningún nombre.</span><span class="sxs-lookup"><span data-stu-id="84145-1214">The name "`__Ev`" is arbitrary; the hidden field could have any name or no name at all.</span></span>

### <a name="event-accessors"></a><span data-ttu-id="84145-1215">Descriptores de acceso de un evento</span><span class="sxs-lookup"><span data-stu-id="84145-1215">Event accessors</span></span>

<span data-ttu-id="84145-1216">Las declaraciones de evento normalmente omiten *event_accessor_declarations*, como en el ejemplo `Button` anterior.</span><span class="sxs-lookup"><span data-stu-id="84145-1216">Event declarations typically omit *event_accessor_declarations*, as in the `Button` example above.</span></span> <span data-ttu-id="84145-1217">Una situación para hacerlo implica el caso en el que el costo de almacenamiento de un campo por evento no es aceptable.</span><span class="sxs-lookup"><span data-stu-id="84145-1217">One situation for doing so involves the case in which the storage cost of one field per event is not acceptable.</span></span> <span data-ttu-id="84145-1218">En tales casos, una clase puede incluir *event_accessor_declarations* y usar un mecanismo privado para almacenar la lista de controladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="84145-1218">In such cases, a class can include *event_accessor_declarations* and use a private mechanism for storing the list of event handlers.</span></span>

<span data-ttu-id="84145-1219">El *event_accessor_declarations* de un evento especifica las instrucciones ejecutables asociadas a la adición y eliminación de controladores de eventos.</span><span class="sxs-lookup"><span data-stu-id="84145-1219">The *event_accessor_declarations* of an event specify the executable statements associated with adding and removing event handlers.</span></span>

<span data-ttu-id="84145-1220">Las declaraciones de descriptor de acceso constan de un *add_accessor_declaration* y un *remove_accessor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="84145-1220">The accessor declarations consist of an *add_accessor_declaration* and a *remove_accessor_declaration*.</span></span> <span data-ttu-id="84145-1221">Cada declaración de descriptor de `add` acceso `remove` consta del token o va seguido de un *bloque*.</span><span class="sxs-lookup"><span data-stu-id="84145-1221">Each accessor declaration consists of the token `add` or `remove` followed by a *block*.</span></span> <span data-ttu-id="84145-1222">El *bloque* asociado a un *add_accessor_declaration* especifica las instrucciones que se ejecutarán cuando se agregue un controlador de eventos y el *bloque* asociado a *remove_accessor_declaration* especifica las instrucciones que se deben ejecutar. Cuando se quita un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="84145-1222">The *block* associated with an *add_accessor_declaration* specifies the statements to execute when an event handler is added, and the *block* associated with a *remove_accessor_declaration* specifies the statements to execute when an event handler is removed.</span></span>

<span data-ttu-id="84145-1223">Cada *add_accessor_declaration* y *remove_accessor_declaration* corresponde a un método con un parámetro de valor único del tipo de evento y un tipo de valor devuelto `void`.</span><span class="sxs-lookup"><span data-stu-id="84145-1223">Each *add_accessor_declaration* and *remove_accessor_declaration* corresponds to a method with a single value parameter of the event type and a `void` return type.</span></span> <span data-ttu-id="84145-1224">El parámetro implícito de un descriptor de `value`acceso de eventos se denomina.</span><span class="sxs-lookup"><span data-stu-id="84145-1224">The implicit parameter of an event accessor is named `value`.</span></span> <span data-ttu-id="84145-1225">Cuando se utiliza un evento en una asignación de eventos, se usa el descriptor de acceso de eventos adecuado.</span><span class="sxs-lookup"><span data-stu-id="84145-1225">When an event is used in an event assignment, the appropriate event accessor is used.</span></span> <span data-ttu-id="84145-1226">En concreto, si el operador de `+=` asignación es, se usa el descriptor de acceso add y `-=` , si el operador de asignación es, se usa el descriptor de acceso Remove.</span><span class="sxs-lookup"><span data-stu-id="84145-1226">Specifically, if the assignment operator is `+=` then the add accessor is used, and if the assignment operator is `-=` then the remove accessor is used.</span></span> <span data-ttu-id="84145-1227">En cualquier caso, el operando derecho del operador de asignación se utiliza como argumento para el descriptor de acceso de eventos.</span><span class="sxs-lookup"><span data-stu-id="84145-1227">In either case, the right-hand operand of the assignment operator is used as the argument to the event accessor.</span></span> <span data-ttu-id="84145-1228">El bloque de *add_accessor_declaration* o *remove_accessor_declaration* debe cumplir las reglas de los métodos `void` descritos en el [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="84145-1228">The block of an *add_accessor_declaration* or a *remove_accessor_declaration* must conform to the rules for `void` methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="84145-1229">En concreto, `return` las instrucciones de un bloque de este tipo no pueden especificar una expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-1229">In particular, `return` statements in such a block are not permitted to specify an expression.</span></span>

<span data-ttu-id="84145-1230">Dado que un descriptor de acceso de eventos `value`tiene implícitamente un parámetro denominado, se trata de un error en tiempo de compilación para una variable local o una constante declarada en un descriptor de acceso de eventos para que tenga ese nombre.</span><span class="sxs-lookup"><span data-stu-id="84145-1230">Since an event accessor implicitly has a parameter named `value`, it is a compile-time error for a local variable or constant declared in an event accessor to have that name.</span></span>

<span data-ttu-id="84145-1231">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1231">In the example</span></span>
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
<span data-ttu-id="84145-1232">la `Control` clase implementa un mecanismo de almacenamiento interno para los eventos.</span><span class="sxs-lookup"><span data-stu-id="84145-1232">the `Control` class implements an internal storage mechanism for events.</span></span> <span data-ttu-id="84145-1233">El `AddEventHandler` método asocia un valor de delegado a una clave, `GetEventHandler` el método devuelve el delegado asociado actualmente a una clave y el `RemoveEventHandler` método quita un delegado como controlador de eventos para el evento especificado.</span><span class="sxs-lookup"><span data-stu-id="84145-1233">The `AddEventHandler` method associates a delegate value with a key, the `GetEventHandler` method returns the delegate currently associated with a key, and the `RemoveEventHandler` method removes a delegate as an event handler for the specified event.</span></span> <span data-ttu-id="84145-1234">Presumiblemente, el mecanismo de almacenamiento subyacente está diseñado de forma que no hay ningún costo por asociar un `null` valor de delegado con una clave y, por lo tanto, los eventos no controlados no consumen almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="84145-1234">Presumably, the underlying storage mechanism is designed such that there is no cost for associating a `null` delegate value with a key, and thus unhandled events consume no storage.</span></span>

### <a name="static-and-instance-events"></a><span data-ttu-id="84145-1235">Eventos estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="84145-1235">Static and instance events</span></span>

<span data-ttu-id="84145-1236">Cuando una declaración de evento incluye `static` un modificador, se dice que se trata de un ***evento estático***.</span><span class="sxs-lookup"><span data-stu-id="84145-1236">When an event declaration includes a `static` modifier, the event is said to be a ***static event***.</span></span> <span data-ttu-id="84145-1237">Cuando no `static` existe ningún modificador, se dice que se trata de un ***evento de instancia***.</span><span class="sxs-lookup"><span data-stu-id="84145-1237">When no `static` modifier is present, the event is said to be an ***instance event***.</span></span>

<span data-ttu-id="84145-1238">Un evento estático no está asociado a una instancia específica de y es un error en tiempo de compilación para hacer referencia `this` a en los descriptores de acceso de un evento estático.</span><span class="sxs-lookup"><span data-stu-id="84145-1238">A static event is not associated with a specific instance, and it is a compile-time error to refer to `this` in the accessors of a static event.</span></span>

<span data-ttu-id="84145-1239">Un evento de instancia está asociado a una instancia determinada de una clase y se puede tener acceso a esta instancia `this` como ([este acceso](expressions.md#this-access)) en los descriptores de acceso de ese evento.</span><span class="sxs-lookup"><span data-stu-id="84145-1239">An instance event is associated with a given instance of a class, and this instance can be accessed as `this` ([This access](expressions.md#this-access)) in the accessors of that event.</span></span>

<span data-ttu-id="84145-1240">Cuando se hace referencia a un evento en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, si `M` es un evento estático, `E` debe indicar un tipo que contiene `M` y, si `M` es un evento de instancia, E debe denotar una instancia de un tipo que contenga `M`.</span><span class="sxs-lookup"><span data-stu-id="84145-1240">When an event is referenced in a *member_access* ([Member access](expressions.md#member-access)) of the form `E.M`, if `M` is a static event, `E` must denote a type containing `M`, and if `M` is an instance event, E must denote an instance of a type containing `M`.</span></span>

<span data-ttu-id="84145-1241">Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).</span><span class="sxs-lookup"><span data-stu-id="84145-1241">The differences between static and instance members are discussed further in [Static and instance members](classes.md#static-and-instance-members).</span></span>

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a><span data-ttu-id="84145-1242">Descriptores de acceso de eventos virtuales, sellados, de invalidación y abstractos</span><span class="sxs-lookup"><span data-stu-id="84145-1242">Virtual, sealed, override, and abstract event accessors</span></span>

<span data-ttu-id="84145-1243">Una `virtual` declaración de evento especifica que los descriptores de acceso de ese evento son virtuales.</span><span class="sxs-lookup"><span data-stu-id="84145-1243">A `virtual` event declaration specifies that the accessors of that event are virtual.</span></span> <span data-ttu-id="84145-1244">El `virtual` modificador se aplica a ambos descriptores de acceso de un evento.</span><span class="sxs-lookup"><span data-stu-id="84145-1244">The `virtual` modifier applies to both accessors of an event.</span></span>

<span data-ttu-id="84145-1245">Una `abstract` declaración de evento especifica que los descriptores de acceso del evento son virtuales, pero no proporciona una implementación real de los descriptores de acceso.</span><span class="sxs-lookup"><span data-stu-id="84145-1245">An `abstract` event declaration specifies that the accessors of the event are virtual, but does not provide an actual implementation of the accessors.</span></span> <span data-ttu-id="84145-1246">En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación para los descriptores de acceso invalidando el evento.</span><span class="sxs-lookup"><span data-stu-id="84145-1246">Instead, non-abstract derived classes are required to provide their own implementation for the accessors by overriding the event.</span></span> <span data-ttu-id="84145-1247">Dado que una declaración de evento abstracto no proporciona ninguna implementación real, no puede proporcionar *event_accessor_declarations*delimitados por llaves.</span><span class="sxs-lookup"><span data-stu-id="84145-1247">Because an abstract event declaration provides no actual implementation, it cannot provide brace-delimited *event_accessor_declarations*.</span></span>

<span data-ttu-id="84145-1248">Una declaración de evento que incluye los `abstract` modificadores y `override` especifica que el evento es abstracto e invalida un evento base.</span><span class="sxs-lookup"><span data-stu-id="84145-1248">An event declaration that includes both the `abstract` and `override` modifiers specifies that the event is abstract and overrides a base event.</span></span> <span data-ttu-id="84145-1249">Los descriptores de acceso de este tipo de evento también son abstractos.</span><span class="sxs-lookup"><span data-stu-id="84145-1249">The accessors of such an event are also abstract.</span></span>

<span data-ttu-id="84145-1250">Las declaraciones de eventos abstractos solo se permiten en clases abstractas ([clases abstractas](classes.md#abstract-classes)).</span><span class="sxs-lookup"><span data-stu-id="84145-1250">Abstract event declarations are only permitted in abstract classes ([Abstract classes](classes.md#abstract-classes)).</span></span>

<span data-ttu-id="84145-1251">Los descriptores de acceso de un evento virtual heredado se pueden invalidar en una clase derivada mediante la inclusión de `override` una declaración de evento que especifique un modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-1251">The accessors of an inherited virtual event can be overridden in a derived class by including an event declaration that specifies an `override` modifier.</span></span> <span data-ttu-id="84145-1252">Esto se conoce como una ***declaración de evento de reemplazo***.</span><span class="sxs-lookup"><span data-stu-id="84145-1252">This is known as an ***overriding event declaration***.</span></span> <span data-ttu-id="84145-1253">Una declaración de evento de reemplazo no declara un nuevo evento.</span><span class="sxs-lookup"><span data-stu-id="84145-1253">An overriding event declaration does not declare a new event.</span></span> <span data-ttu-id="84145-1254">En su lugar, simplemente especializa las implementaciones de los descriptores de acceso de un evento virtual existente.</span><span class="sxs-lookup"><span data-stu-id="84145-1254">Instead, it simply specializes the implementations of the accessors of an existing virtual event.</span></span>

<span data-ttu-id="84145-1255">Una declaración de evento de reemplazo debe especificar exactamente los mismos modificadores de accesibilidad, tipo y nombre que el evento invalidado.</span><span class="sxs-lookup"><span data-stu-id="84145-1255">An overriding event declaration must specify the exact same accessibility modifiers, type, and name as the overridden event.</span></span>

<span data-ttu-id="84145-1256">Una declaración de evento de reemplazo puede incluir `sealed` el modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-1256">An overriding event declaration may include the `sealed` modifier.</span></span> <span data-ttu-id="84145-1257">El uso de este modificador evita que una clase derivada Reemplace el evento.</span><span class="sxs-lookup"><span data-stu-id="84145-1257">Use of this modifier prevents a derived class from further overriding the event.</span></span> <span data-ttu-id="84145-1258">Los descriptores de acceso de un evento sellado también están sellados.</span><span class="sxs-lookup"><span data-stu-id="84145-1258">The accessors of a sealed event are also sealed.</span></span>

<span data-ttu-id="84145-1259">Es un error en tiempo de compilación que una declaración de evento de reemplazo incluya un `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-1259">It is a compile-time error for an overriding event declaration to include a `new` modifier.</span></span>

<span data-ttu-id="84145-1260">A excepción de las diferencias en la sintaxis de declaración e invocación, los descriptores de acceso virtual, sellado, invalidación y Abstract se comportan exactamente igual que los métodos virtuales, sellados, de invalidación y abstractos.</span><span class="sxs-lookup"><span data-stu-id="84145-1260">Except for differences in declaration and invocation syntax, virtual, sealed, override, and abstract accessors behave exactly like virtual, sealed, override and abstract methods.</span></span> <span data-ttu-id="84145-1261">En concreto, las reglas descritas en [métodos virtuales](classes.md#virtual-methods), [métodos de invalidación](classes.md#override-methods), [métodos sellados](classes.md#sealed-methods)y [métodos abstractos](classes.md#abstract-methods) se aplican como si los descriptores de acceso fueran métodos de un formulario correspondiente.</span><span class="sxs-lookup"><span data-stu-id="84145-1261">Specifically, the rules described in [Virtual methods](classes.md#virtual-methods), [Override methods](classes.md#override-methods), [Sealed methods](classes.md#sealed-methods), and [Abstract methods](classes.md#abstract-methods) apply as if accessors were methods of a corresponding form.</span></span> <span data-ttu-id="84145-1262">Cada descriptor de acceso corresponde a un método con un parámetro de valor único del `void` tipo de evento, un tipo de valor devuelto y los mismos modificadores que el evento que lo contiene.</span><span class="sxs-lookup"><span data-stu-id="84145-1262">Each accessor corresponds to a method with a single value parameter of the event type, a `void` return type, and the same modifiers as the containing event.</span></span>

## <a name="indexers"></a><span data-ttu-id="84145-1263">Indizadores</span><span class="sxs-lookup"><span data-stu-id="84145-1263">Indexers</span></span>

<span data-ttu-id="84145-1264">Un ***indexador*** es un miembro que permite indizar un objeto de la misma manera que una matriz.</span><span class="sxs-lookup"><span data-stu-id="84145-1264">An ***indexer*** is a member that enables an object to be indexed in the same way as an array.</span></span> <span data-ttu-id="84145-1265">Los indexadores se declaran mediante *indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="84145-1265">Indexers are declared using *indexer_declaration*s:</span></span>

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

<span data-ttu-id="84145-1266">Un *indexer_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)), el `new` ([el modificador nuevo](classes.md#the-new-modifier)), `virtual` ([métodos virtuales ](classes.md#virtual-methods)), `override` ([métodos de invalidación](classes.md#override-methods)), 0 ([métodos sellados](classes.md#sealed-methods)), 2 ([métodos abstractos](classes.md#abstract-methods)) y modificadores 4 ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="84145-1266">An *indexer_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), the `new` ([The new modifier](classes.md#the-new-modifier)), `virtual` ([Virtual methods](classes.md#virtual-methods)), `override` ([Override methods](classes.md#override-methods)), `sealed` ([Sealed methods](classes.md#sealed-methods)), `abstract` ([Abstract methods](classes.md#abstract-methods)), and `extern` ([External methods](classes.md#external-methods)) modifiers.</span></span>

<span data-ttu-id="84145-1267">Las declaraciones de indexador están sujetas a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores, con la única excepción de que el modificador static no se permite en una declaración de indexador.</span><span class="sxs-lookup"><span data-stu-id="84145-1267">Indexer declarations are subject to the same rules as method declarations ([Methods](classes.md#methods)) with regard to valid combinations of modifiers, with the one exception being that the static modifier is not permitted on an indexer declaration.</span></span>

<span data-ttu-id="84145-1268">Los modificadores `virtual`, `override`y `abstract` se excluyen mutuamente, excepto en un caso.</span><span class="sxs-lookup"><span data-stu-id="84145-1268">The modifiers `virtual`, `override`, and `abstract` are mutually exclusive except in one case.</span></span> <span data-ttu-id="84145-1269">Los `abstract` modificadores y `override` se pueden usar juntos para que un indexador abstracto pueda reemplazar a uno virtual.</span><span class="sxs-lookup"><span data-stu-id="84145-1269">The `abstract` and `override` modifiers may be used together so that an abstract indexer can override a virtual one.</span></span>

<span data-ttu-id="84145-1270">El *tipo* de una declaración de indexador especifica el tipo de elemento del indizador introducido por la declaración.</span><span class="sxs-lookup"><span data-stu-id="84145-1270">The *type* of an indexer declaration specifies the element type of the indexer introduced by the declaration.</span></span> <span data-ttu-id="84145-1271">A menos que el indizador sea una implementación explícita de un miembro de interfaz, el tipo `this`va seguido de la palabra clave.</span><span class="sxs-lookup"><span data-stu-id="84145-1271">Unless the indexer is an explicit interface member implementation, the *type* is followed by the keyword `this`.</span></span> <span data-ttu-id="84145-1272">Para una implementación explícita de un miembro de interfaz, el *tipo* va seguido de un *interface_type*, un "`.`" y la palabra clave `this`.</span><span class="sxs-lookup"><span data-stu-id="84145-1272">For an explicit interface member implementation, the *type* is followed by an *interface_type*, a "`.`", and the keyword `this`.</span></span> <span data-ttu-id="84145-1273">A diferencia de otros miembros, los indizadores no tienen nombres definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="84145-1273">Unlike other members, indexers do not have user-defined names.</span></span>

<span data-ttu-id="84145-1274">*Formal_parameter_list* especifica los parámetros del indexador.</span><span class="sxs-lookup"><span data-stu-id="84145-1274">The *formal_parameter_list* specifies the parameters of the indexer.</span></span> <span data-ttu-id="84145-1275">La lista de parámetros formales de un indizador corresponde a la de un método ([parámetros de método](classes.md#method-parameters)), salvo que se debe especificar al menos un parámetro y `ref` que `out` no se permiten los modificadores de parámetro y.</span><span class="sxs-lookup"><span data-stu-id="84145-1275">The formal parameter list of an indexer corresponds to that of a method ([Method parameters](classes.md#method-parameters)), except that at least one parameter must be specified, and that the `ref` and `out` parameter modifiers are not permitted.</span></span>

<span data-ttu-id="84145-1276">El *tipo* de un indexador y cada uno de los tipos a los que se hace referencia en *formal_parameter_list* deben ser al menos tan accesibles como el propio indizador ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="84145-1276">The *type* of an indexer and each of the types referenced in the *formal_parameter_list* must be at least as accessible as the indexer itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="84145-1277">Un *indexer_body* puede estar formado por un ***cuerpo de descriptor de acceso*** o un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="84145-1277">An *indexer_body* may either consist of an ***accessor body*** or an ***expression body***.</span></span> <span data-ttu-id="84145-1278">En el cuerpo de un descriptor de acceso, *accessor_declarations*, que se debe incluir en los tokens "`{`" y "`}`", declare los descriptores de acceso ([descriptores de acceso](classes.md#accessors)) de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1278">In an accessor body, *accessor_declarations*, which must be enclosed in "`{`" and "`}`" tokens, declare the accessors ([Accessors](classes.md#accessors)) of the property.</span></span> <span data-ttu-id="84145-1279">Los descriptores de acceso especifican las instrucciones ejecutables asociadas a la lectura y la escritura de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1279">The accessors specify the executable statements associated with reading and writing the property.</span></span>

<span data-ttu-id="84145-1280">Un cuerpo de expresión que consta de`=>`"" seguido de una `E` expresión y un punto y coma es exactamente equivalente al cuerpo `{ get { return E; } }`de la instrucción y, por tanto, solo se puede usar para especificar indizadores de solo captador en los que el resultado del captador es proporcionado por una sola expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-1280">An expression body consisting of "`=>`" followed by an expression `E` and a semicolon is exactly equivalent to the statement body `{ get { return E; } }`, and can therefore only be used to specify getter-only indexers where the result of the getter is given by a single expression.</span></span>

<span data-ttu-id="84145-1281">Aunque la sintaxis para tener acceso a un elemento de indexador es la misma que para un elemento de matriz, un elemento de indexador no se clasifica como una variable.</span><span class="sxs-lookup"><span data-stu-id="84145-1281">Even though the syntax for accessing an indexer element is the same as that for an array element, an indexer element is not classified as a variable.</span></span> <span data-ttu-id="84145-1282">Por lo tanto, no es posible pasar un elemento indexador como `ref` argumento o. `out`</span><span class="sxs-lookup"><span data-stu-id="84145-1282">Thus, it is not possible to pass an indexer element as a `ref` or `out` argument.</span></span>

<span data-ttu-id="84145-1283">La lista de parámetros formales de un indexador define la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) del indexador.</span><span class="sxs-lookup"><span data-stu-id="84145-1283">The formal parameter list of an indexer defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of the indexer.</span></span> <span data-ttu-id="84145-1284">En concreto, la firma de un indexador consta del número y los tipos de sus parámetros formales.</span><span class="sxs-lookup"><span data-stu-id="84145-1284">Specifically, the signature of an indexer consists of the number and types of its formal parameters.</span></span> <span data-ttu-id="84145-1285">El tipo de elemento y los nombres de los parámetros formales no forman parte de la firma de un indexador.</span><span class="sxs-lookup"><span data-stu-id="84145-1285">The element type and names of the formal parameters are not part of an indexer's signature.</span></span>

<span data-ttu-id="84145-1286">La firma de un indizador debe ser diferente de las firmas de todos los demás indexadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1286">The signature of an indexer must differ from the signatures of all other indexers declared in the same class.</span></span>

<span data-ttu-id="84145-1287">Los indexadores y las propiedades son muy similares en concepto, pero difieren de las siguientes maneras:</span><span class="sxs-lookup"><span data-stu-id="84145-1287">Indexers and properties are very similar in concept, but differ in the following ways:</span></span>

*  <span data-ttu-id="84145-1288">Una propiedad se identifica por su nombre, mientras que un indexador se identifica mediante su firma.</span><span class="sxs-lookup"><span data-stu-id="84145-1288">A property is identified by its name, whereas an indexer is identified by its signature.</span></span>
*  <span data-ttu-id="84145-1289">Se tiene acceso a una propiedad a través de *simple_name* ([nombres simples](expressions.md#simple-names)) o de *member_access* ([acceso a miembros](expressions.md#member-access)), mientras que se tiene acceso a un elemento de indexador a través de un *element_access* ([acceso a indexador](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="84145-1289">A property is accessed through a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)), whereas an indexer element is accessed through an *element_access* ([Indexer access](expressions.md#indexer-access)).</span></span>
*  <span data-ttu-id="84145-1290">Una propiedad puede ser un `static` miembro, mientras que un indexador siempre es un miembro de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-1290">A property can be a `static` member, whereas an indexer is always an instance member.</span></span>
*  <span data-ttu-id="84145-1291">Un `get` descriptor de acceso de una propiedad corresponde a un método sin parámetros `get` , mientras que un descriptor de acceso de un indizador corresponde a un método con la misma lista de parámetros formales que el indexador.</span><span class="sxs-lookup"><span data-stu-id="84145-1291">A `get` accessor of a property corresponds to a method with no parameters, whereas a `get` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer.</span></span>
*  <span data-ttu-id="84145-1292">Un `set` descriptor de acceso de una propiedad corresponde a un método con `value`un único parámetro `set` denominado, mientras que un descriptor de acceso de un indizador corresponde a un método con la misma lista de parámetros formales que el indexador, además de un parámetro adicional. denominado `value`.</span><span class="sxs-lookup"><span data-stu-id="84145-1292">A `set` accessor of a property corresponds to a method with a single parameter named `value`, whereas a `set` accessor of an indexer corresponds to a method with the same formal parameter list as the indexer, plus an additional parameter named `value`.</span></span>
*  <span data-ttu-id="84145-1293">Es un error en tiempo de compilación que un descriptor de acceso de indexador declare una variable local con el mismo nombre que un parámetro de indizador.</span><span class="sxs-lookup"><span data-stu-id="84145-1293">It is a compile-time error for an indexer accessor to declare a local variable with the same name as an indexer parameter.</span></span>
*  <span data-ttu-id="84145-1294">En una declaración de propiedad de reemplazo, se tiene acceso a la propiedad heredada `base.P`mediante la `P` sintaxis, donde es el nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="84145-1294">In an overriding property declaration, the inherited property is accessed using the syntax `base.P`, where `P` is the property name.</span></span> <span data-ttu-id="84145-1295">En una declaración de indizador de reemplazo, se tiene acceso al indizador heredado mediante `base[E]`la sintaxis `E` , donde es una lista de expresiones separadas por comas.</span><span class="sxs-lookup"><span data-stu-id="84145-1295">In an overriding indexer declaration, the inherited indexer is accessed using the syntax `base[E]`, where `E` is a comma separated list of expressions.</span></span>
*  <span data-ttu-id="84145-1296">No hay ningún concepto de "indexador implementado automáticamente".</span><span class="sxs-lookup"><span data-stu-id="84145-1296">There is no concept of an "automatically implemented indexer".</span></span> <span data-ttu-id="84145-1297">Es un error tener un indexador no externo no abstracto con descriptores de acceso de punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-1297">It is an error to have a non-abstract, non-external indexer with semicolon accessors.</span></span>

<span data-ttu-id="84145-1298">Aparte de estas diferencias, todas las reglas definidas en los [descriptores de acceso](classes.md#accessors) y [las propiedades implementadas automáticamente](classes.md#automatically-implemented-properties) se aplican a los descriptores de acceso del indizador y a los descriptores de acceso de propiedades.</span><span class="sxs-lookup"><span data-stu-id="84145-1298">Aside from these differences, all rules defined in [Accessors](classes.md#accessors) and [Automatically implemented properties](classes.md#automatically-implemented-properties) apply to indexer accessors as well as to property accessors.</span></span>

<span data-ttu-id="84145-1299">Cuando una declaración de indexador incluye `extern` un modificador, se dice que se trata de un ***indizador externo***.</span><span class="sxs-lookup"><span data-stu-id="84145-1299">When an indexer declaration includes an `extern` modifier, the indexer is said to be an ***external indexer***.</span></span> <span data-ttu-id="84145-1300">Dado que una declaración de indexador externa no proporciona ninguna implementación real, cada una de sus *accessor_declarations* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-1300">Because an external indexer declaration provides no actual implementation, each of its *accessor_declarations* consists of a semicolon.</span></span>

<span data-ttu-id="84145-1301">En el ejemplo siguiente se declara `BitArray` una clase que implementa un indizador para tener acceso a los bits individuales de la matriz de bits.</span><span class="sxs-lookup"><span data-stu-id="84145-1301">The example below declares a `BitArray` class that implements an indexer for accessing the individual bits in the bit array.</span></span>
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

<span data-ttu-id="84145-1302">Una instancia de la `BitArray` clase consume bastante menos memoria que un correspondiente `bool[]` (puesto que cada valor de la antigua solo ocupa un bit en lugar de un byte), pero permite `bool[]`las mismas operaciones que.</span><span class="sxs-lookup"><span data-stu-id="84145-1302">An instance of the `BitArray` class consumes substantially less memory than a corresponding `bool[]` (since each value of the former occupies only one bit instead of the latter's one byte), but it permits the same operations as a `bool[]`.</span></span>

<span data-ttu-id="84145-1303">La clase `CountPrimes` siguiente usa un `BitArray` y el algoritmo clásico "criba" para calcular el número de primos entre 1 y un máximo determinado:</span><span class="sxs-lookup"><span data-stu-id="84145-1303">The following `CountPrimes` class uses a `BitArray` and the classical "sieve" algorithm to compute the number of primes between 1 and a given maximum:</span></span>
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

<span data-ttu-id="84145-1304">Tenga en cuenta que la sintaxis para tener acceso a `BitArray` los elementos de es exactamente igual que `bool[]`para.</span><span class="sxs-lookup"><span data-stu-id="84145-1304">Note that the syntax for accessing elements of the `BitArray` is precisely the same as for a `bool[]`.</span></span>

<span data-ttu-id="84145-1305">En el ejemplo siguiente se muestra una clase de cuadrícula de 26 \* 10 que tiene un indizador con dos parámetros.</span><span class="sxs-lookup"><span data-stu-id="84145-1305">The following example shows a 26 \* 10 grid class that has an indexer with two parameters.</span></span> <span data-ttu-id="84145-1306">El primer parámetro debe ser una letra mayúscula o minúscula en el intervalo A-Z, y el segundo debe ser un entero en el intervalo 0-9.</span><span class="sxs-lookup"><span data-stu-id="84145-1306">The first parameter is required to be an upper- or lowercase letter in the range A-Z, and the second is required to be an integer in the range 0-9.</span></span>

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

### <a name="indexer-overloading"></a><span data-ttu-id="84145-1307">Sobrecarga del indexador</span><span class="sxs-lookup"><span data-stu-id="84145-1307">Indexer overloading</span></span>

<span data-ttu-id="84145-1308">Las reglas de resolución de sobrecarga de indexador se describen en [inferencia de tipos](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="84145-1308">The indexer overload resolution rules are described in [Type inference](expressions.md#type-inference).</span></span>

## <a name="operators"></a><span data-ttu-id="84145-1309">Operadores</span><span class="sxs-lookup"><span data-stu-id="84145-1309">Operators</span></span>

<span data-ttu-id="84145-1310">Un ***operador*** es un miembro que define el significado de un operador de expresión que se puede aplicar a las instancias de la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1310">An ***operator*** is a member that defines the meaning of an expression operator that can be applied to instances of the class.</span></span> <span data-ttu-id="84145-1311">Los operadores se declaran mediante *operator_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="84145-1311">Operators are declared using *operator_declaration*s:</span></span>

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

<span data-ttu-id="84145-1312">Hay tres categorías de Operadores sobrecargables: Operadores unarios ([operadores unarios](classes.md#unary-operators)), operadores binarios (operadores[binarios](classes.md#binary-operators)) y operadores de conversión ([operadores de conversión](classes.md#conversion-operators)).</span><span class="sxs-lookup"><span data-stu-id="84145-1312">There are three categories of overloadable operators: Unary operators ([Unary operators](classes.md#unary-operators)), binary operators ([Binary operators](classes.md#binary-operators)), and conversion operators ([Conversion operators](classes.md#conversion-operators)).</span></span>

<span data-ttu-id="84145-1313">*Operator_body* es un punto y coma, un ***cuerpo de instrucción*** o un ***cuerpo de expresión***.</span><span class="sxs-lookup"><span data-stu-id="84145-1313">The *operator_body* is either a semicolon, a ***statement body*** or an ***expression body***.</span></span> <span data-ttu-id="84145-1314">Un cuerpo de instrucción consta de un *bloque*, que especifica las instrucciones que se ejecutarán cuando se invoque el operador.</span><span class="sxs-lookup"><span data-stu-id="84145-1314">A statement body consists of a *block*, which specifies the statements to execute when the operator is invoked.</span></span> <span data-ttu-id="84145-1315">El *bloque* debe cumplir las reglas de los métodos que devuelven valores descritos en el [cuerpo del método](classes.md#method-body).</span><span class="sxs-lookup"><span data-stu-id="84145-1315">The *block* must conform to the rules for value-returning methods described in [Method body](classes.md#method-body).</span></span> <span data-ttu-id="84145-1316">Un cuerpo de expresión consta `=>` de seguido de una expresión y un punto y coma, y denota una expresión única que se debe realizar cuando se invoca el operador.</span><span class="sxs-lookup"><span data-stu-id="84145-1316">An expression body consists of `=>` followed by an expression and a semicolon, and denotes a single expression to perform when the operator is invoked.</span></span>

<span data-ttu-id="84145-1317">En el caso de los operadores `extern`, el *operator_body* consta simplemente de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-1317">For `extern` operators, the *operator_body* consists simply of a semicolon.</span></span> <span data-ttu-id="84145-1318">En el caso de todos los demás operadores, *operator_body* es un cuerpo de bloque o un cuerpo de expresión.</span><span class="sxs-lookup"><span data-stu-id="84145-1318">For all other operators, the *operator_body* is either a block body or an expression body.</span></span>

<span data-ttu-id="84145-1319">Las siguientes reglas se aplican a todas las declaraciones de operador:</span><span class="sxs-lookup"><span data-stu-id="84145-1319">The following rules apply to all operator declarations:</span></span>

*  <span data-ttu-id="84145-1320">Una declaración de operador debe incluir tanto `public` un `static` modificador como un modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-1320">An operator declaration must include both a `public` and a `static` modifier.</span></span>
*  <span data-ttu-id="84145-1321">Los parámetros de un operador deben ser parámetros de valor (parámetros de[valor](variables.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="84145-1321">The parameter(s) of an operator must be value parameters ([Value parameters](variables.md#value-parameters)).</span></span> <span data-ttu-id="84145-1322">Se trata de un error en tiempo de compilación para que una declaración `ref` de `out` operador Especifique parámetros o.</span><span class="sxs-lookup"><span data-stu-id="84145-1322">It is a compile-time error for an operator declaration to specify `ref` or `out` parameters.</span></span>
*  <span data-ttu-id="84145-1323">La firma de un operador ([operadores unarios](classes.md#unary-operators), [operadores binarios](classes.md#binary-operators)y [operadores de conversión](classes.md#conversion-operators)) debe ser diferente de las firmas de todos los demás operadores declarados en la misma clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1323">The signature of an operator ([Unary operators](classes.md#unary-operators), [Binary operators](classes.md#binary-operators), [Conversion operators](classes.md#conversion-operators)) must differ from the signatures of all other operators declared in the same class.</span></span>
*  <span data-ttu-id="84145-1324">Todos los tipos a los que se hace referencia en una declaración de operador deben ser al menos tan accesibles como el propio operador ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="84145-1324">All types referenced in an operator declaration must be at least as accessible as the operator itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>
*  <span data-ttu-id="84145-1325">Es un error que el mismo modificador aparezca varias veces en una declaración de operador.</span><span class="sxs-lookup"><span data-stu-id="84145-1325">It is an error for the same modifier to appear multiple times in an operator declaration.</span></span>

<span data-ttu-id="84145-1326">Cada categoría de operador impone restricciones adicionales, tal y como se describe en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="84145-1326">Each operator category imposes additional restrictions, as described in the following sections.</span></span>

<span data-ttu-id="84145-1327">Al igual que otros miembros, las clases derivadas heredan los operadores declarados en una clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-1327">Like other members, operators declared in a base class are inherited by derived classes.</span></span> <span data-ttu-id="84145-1328">Dado que las declaraciones de operador siempre requieren la clase o estructura en la que se declara que el operador participa en la firma del operador, no es posible que un operador declarado en una clase derivada oculte un operador declarado en una clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-1328">Because operator declarations always require the class or struct in which the operator is declared to participate in the signature of the operator, it is not possible for an operator declared in a derived class to hide an operator declared in a base class.</span></span> <span data-ttu-id="84145-1329">Por lo tanto `new` , el modificador nunca es necesario y, por lo tanto, nunca se permite en una declaración de operador.</span><span class="sxs-lookup"><span data-stu-id="84145-1329">Thus, the `new` modifier is never required, and therefore never permitted, in an operator declaration.</span></span>

<span data-ttu-id="84145-1330">Puede encontrar información adicional sobre operadores unarios y binarios en [operadores](expressions.md#operators).</span><span class="sxs-lookup"><span data-stu-id="84145-1330">Additional information on unary and binary operators can be found in [Operators](expressions.md#operators).</span></span>

<span data-ttu-id="84145-1331">Puede encontrar información adicional sobre los operadores de conversión en [conversiones definidas por el usuario](conversions.md#user-defined-conversions).</span><span class="sxs-lookup"><span data-stu-id="84145-1331">Additional information on conversion operators can be found in [User-defined conversions](conversions.md#user-defined-conversions).</span></span>

### <a name="unary-operators"></a><span data-ttu-id="84145-1332">Operadores unarios</span><span class="sxs-lookup"><span data-stu-id="84145-1332">Unary operators</span></span>

<span data-ttu-id="84145-1333">Las siguientes reglas se aplican a las declaraciones de operadores `T` unarios, donde denota el tipo de instancia de la clase o el struct que contiene la declaración de operador:</span><span class="sxs-lookup"><span data-stu-id="84145-1333">The following rules apply to unary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="84145-1334">Un operador `+`unario `!`, `-`, `~` o debe tomar un parámetro único de tipo `T` o `T?` y puede devolver cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-1334">A unary `+`, `-`, `!`, or `~` operator must take a single parameter of type `T` or `T?` and can return any type.</span></span>
*  <span data-ttu-id="84145-1335">Un operador `++` unario or `--` debe tomar un único parámetro de `T` tipo `T?` o y debe devolver el mismo tipo o un tipo derivado de él.</span><span class="sxs-lookup"><span data-stu-id="84145-1335">A unary `++` or `--` operator must take a single parameter of type `T` or `T?` and must return that same type or a type derived from it.</span></span>
*  <span data-ttu-id="84145-1336">Un operador `true` unario or `false` debe tomar un parámetro único de `T` tipo `T?` o y debe devolver `bool`el tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-1336">A unary `true` or `false` operator must take a single parameter of type `T` or `T?` and must return type `bool`.</span></span>

<span data-ttu-id="84145-1337">La firma de un operador unario está formada por el token`+`del `-`operador ( `~`, `++`, `--` `!`, `true`,, `false`, o) y el tipo del único parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="84145-1337">The signature of a unary operator consists of the operator token (`+`, `-`, `!`, `~`, `++`, `--`, `true`, or `false`) and the type of the single formal parameter.</span></span> <span data-ttu-id="84145-1338">El tipo de valor devuelto no forma parte de la firma de un operador unario, ni tampoco el nombre del parámetro formal.</span><span class="sxs-lookup"><span data-stu-id="84145-1338">The return type is not part of a unary operator's signature, nor is the name of the formal parameter.</span></span>

<span data-ttu-id="84145-1339">Los `true` operadores `false` unarios y requieren la declaración de pares.</span><span class="sxs-lookup"><span data-stu-id="84145-1339">The `true` and `false` unary operators require pair-wise declaration.</span></span> <span data-ttu-id="84145-1340">Se produce un error en tiempo de compilación si una clase declara uno de estos operadores sin declarar también el otro.</span><span class="sxs-lookup"><span data-stu-id="84145-1340">A compile-time error occurs if a class declares one of these operators without also declaring the other.</span></span> <span data-ttu-id="84145-1341">Los `true` operadores `false` y se describen con más detalle en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators) y en [Expresiones booleanas](expressions.md#boolean-expressions).</span><span class="sxs-lookup"><span data-stu-id="84145-1341">The `true` and `false` operators are described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators) and [Boolean expressions](expressions.md#boolean-expressions).</span></span>

<span data-ttu-id="84145-1342">En el ejemplo siguiente se muestra una implementación y el `operator ++` uso posterior de para una clase de vector entero:</span><span class="sxs-lookup"><span data-stu-id="84145-1342">The following example shows an implementation and subsequent usage of `operator ++` for an integer vector class:</span></span>
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

<span data-ttu-id="84145-1343">Observe cómo el método de operador devuelve el valor generado agregando 1 al operando, al igual que los operadores de incremento y decremento de postfijo ([operadores de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)postfijo) y los operadores de incremento y decremento de prefijo ([Prefijo operadores de incremento y decremento](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="84145-1343">Note how the operator method returns the value produced by adding 1 to the operand, just like the  postfix increment and decrement operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)), and the prefix increment and decrement operators ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="84145-1344">A diferencia de C++en, este método no necesita modificar directamente el valor de su operando.</span><span class="sxs-lookup"><span data-stu-id="84145-1344">Unlike in C++, this method need not modify the value of its operand directly.</span></span> <span data-ttu-id="84145-1345">De hecho, la modificación del valor de operando infringiría la semántica estándar del operador de incremento de postfijo.</span><span class="sxs-lookup"><span data-stu-id="84145-1345">In fact, modifying the operand value would violate the standard semantics of the postfix increment operator.</span></span>

### <a name="binary-operators"></a><span data-ttu-id="84145-1346">Operadores binarios</span><span class="sxs-lookup"><span data-stu-id="84145-1346">Binary operators</span></span>

<span data-ttu-id="84145-1347">Las siguientes reglas se aplican a las declaraciones de operadores `T` binarios, donde denota el tipo de instancia de la clase o el struct que contiene la declaración de operador:</span><span class="sxs-lookup"><span data-stu-id="84145-1347">The following rules apply to binary operator declarations, where `T` denotes the instance type of the class or struct that contains the operator declaration:</span></span>

*  <span data-ttu-id="84145-1348">Un operador binario sin desplazamiento debe tomar dos parámetros, al menos uno de los cuales debe tener `T` el `T?`tipo o y puede devolver cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-1348">A binary non-shift operator must take two parameters, at least one of which must have type `T` or `T?`, and can return any type.</span></span>
*  <span data-ttu-id="84145-1349">Un operador `<<` or `>>` binario debe tomar dos parámetros, el primero debe tener el tipo `T` o `T?` y el segundo de los cuales debe tener `int` el `int?`tipo o, y puede devolver cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-1349">A binary `<<` or `>>` operator must take two parameters, the first of which must have type `T` or `T?` and the second of which must have type `int` or `int?`, and can return any type.</span></span>

<span data-ttu-id="84145-1350">La firma de un operador binario consta del token del operador`+`( `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`,, `==`, ,,`>`, o`>=`) ylostiposdelosdosparámetrosformales`<=`. `!=` `<`</span><span class="sxs-lookup"><span data-stu-id="84145-1350">The signature of a binary operator consists of the operator token (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, or `<=`) and the types of the two formal parameters.</span></span> <span data-ttu-id="84145-1351">El tipo de valor devuelto y los nombres de los parámetros formales no forman parte de la firma de un operador binario.</span><span class="sxs-lookup"><span data-stu-id="84145-1351">The return type and the names of the formal parameters are not part of a binary operator's signature.</span></span>

<span data-ttu-id="84145-1352">Ciertos operadores binarios requieren la declaración de pares.</span><span class="sxs-lookup"><span data-stu-id="84145-1352">Certain binary operators require pair-wise declaration.</span></span> <span data-ttu-id="84145-1353">Para cada declaración de cualquiera de los operadores de un par, debe haber una declaración coincidente del otro operador del par.</span><span class="sxs-lookup"><span data-stu-id="84145-1353">For every declaration of either operator of a pair, there must be a matching declaration of the other operator of the pair.</span></span> <span data-ttu-id="84145-1354">Dos declaraciones de operador coinciden cuando tienen el mismo tipo de valor devuelto y el mismo tipo para cada parámetro.</span><span class="sxs-lookup"><span data-stu-id="84145-1354">Two operator declarations match when they have the same return type and the same type for each parameter.</span></span> <span data-ttu-id="84145-1355">Los operadores siguientes requieren una declaración de pares:</span><span class="sxs-lookup"><span data-stu-id="84145-1355">The following operators require pair-wise declaration:</span></span>

*  <span data-ttu-id="84145-1356">`operator ==` y `operator !=`</span><span class="sxs-lookup"><span data-stu-id="84145-1356">`operator ==` and `operator !=`</span></span>
*  <span data-ttu-id="84145-1357">`operator >` y `operator <`</span><span class="sxs-lookup"><span data-stu-id="84145-1357">`operator >` and `operator <`</span></span>
*  <span data-ttu-id="84145-1358">`operator >=` y `operator <=`</span><span class="sxs-lookup"><span data-stu-id="84145-1358">`operator >=` and `operator <=`</span></span>

### <a name="conversion-operators"></a><span data-ttu-id="84145-1359">Operadores de conversión</span><span class="sxs-lookup"><span data-stu-id="84145-1359">Conversion operators</span></span>

<span data-ttu-id="84145-1360">Una declaración de operador de conversión introduce una ***conversión definida por el usuario*** ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)) que aumenta las conversiones implícitas y explícitas predefinidas.</span><span class="sxs-lookup"><span data-stu-id="84145-1360">A conversion operator declaration introduces a ***user-defined conversion*** ([User-defined conversions](conversions.md#user-defined-conversions)) which augments the pre-defined implicit and explicit conversions.</span></span>

<span data-ttu-id="84145-1361">Una declaración de operador de conversión que `implicit` incluye la palabra clave presenta una conversión implícita definida por el usuario.</span><span class="sxs-lookup"><span data-stu-id="84145-1361">A conversion operator declaration that includes the `implicit` keyword introduces a user-defined implicit conversion.</span></span> <span data-ttu-id="84145-1362">Las conversiones implícitas pueden producirse en diversas situaciones, incluidas las invocaciones de miembros de función, las expresiones de conversión y las asignaciones.</span><span class="sxs-lookup"><span data-stu-id="84145-1362">Implicit conversions can occur in a variety of situations, including function member invocations, cast expressions, and assignments.</span></span> <span data-ttu-id="84145-1363">Esto se describe con más detalle en [conversiones implícitas](conversions.md#implicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="84145-1363">This is described further in [Implicit conversions](conversions.md#implicit-conversions).</span></span>

<span data-ttu-id="84145-1364">Una declaración de operador de conversión que `explicit` incluye la palabra clave presenta una conversión explícita definida por el usuario.</span><span class="sxs-lookup"><span data-stu-id="84145-1364">A conversion operator declaration that includes the `explicit` keyword introduces a user-defined explicit conversion.</span></span> <span data-ttu-id="84145-1365">Las conversiones explícitas pueden producirse en expresiones de conversión y se describen con más detalle en [conversiones explícitas](conversions.md#explicit-conversions).</span><span class="sxs-lookup"><span data-stu-id="84145-1365">Explicit conversions can occur in cast expressions, and are described further in [Explicit conversions](conversions.md#explicit-conversions).</span></span>

<span data-ttu-id="84145-1366">Un operador de conversión convierte de un tipo de origen, indicado por el tipo de parámetro del operador de conversión, en un tipo de destino, indicado por el tipo de valor devuelto del operador de conversión.</span><span class="sxs-lookup"><span data-stu-id="84145-1366">A conversion operator converts from a source type, indicated by the parameter type of the conversion operator, to a target type, indicated by the return type of the conversion operator.</span></span>

<span data-ttu-id="84145-1367">Para un tipo de origen `S` y tipo `T`de destino determinados `S` , `T` si o son tipos que aceptan `S0` valores `T0` null, permiten y hacen referencia a `S0` sus `T0` tipos subyacentes; de lo contrario, y son. igual a `S` y `T` , respectivamente.</span><span class="sxs-lookup"><span data-stu-id="84145-1367">For a given source type `S` and target type `T`, if `S` or `T` are nullable types, let `S0` and `T0` refer to their underlying types, otherwise `S0` and `T0` are equal to `S` and `T` respectively.</span></span> <span data-ttu-id="84145-1368">Una clase o struct puede declarar una conversión de un tipo `S` de origen a un tipo `T` de destino solo si se cumplen todas las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="84145-1368">A class or struct is permitted to declare a conversion from a source type `S` to a target type `T` only if all of the following are true:</span></span>

*  <span data-ttu-id="84145-1369">`S0`y `T0` son tipos diferentes.</span><span class="sxs-lookup"><span data-stu-id="84145-1369">`S0` and `T0` are different types.</span></span>
*  <span data-ttu-id="84145-1370">`S0` O`T0` es el tipo de clase o estructura en el que tiene lugar la declaración del operador.</span><span class="sxs-lookup"><span data-stu-id="84145-1370">Either `S0` or `T0` is the class or struct type in which the operator declaration takes place.</span></span>
*  <span data-ttu-id="84145-1371">Ni `S0` ni `T0` son *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="84145-1371">Neither `S0` nor `T0` is an *interface_type*.</span></span>
*  <span data-ttu-id="84145-1372">Sin incluir las conversiones definidas por el usuario, no existe una conversión `S` de `T` a o `T` de `S`a.</span><span class="sxs-lookup"><span data-stu-id="84145-1372">Excluding user-defined conversions, a conversion does not exist from `S` to `T` or from `T` to `S`.</span></span>

<span data-ttu-id="84145-1373">En el caso de estas reglas, los parámetros de tipo asociados `S` a `T` o se consideran tipos únicos que no tienen ninguna relación de herencia con otros tipos y se omiten las restricciones en esos parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-1373">For the purposes of these rules, any type parameters associated with `S` or `T` are considered to be unique types that have no inheritance relationship with other types, and any constraints on those type parameters are ignored.</span></span>

<span data-ttu-id="84145-1374">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1374">In the example</span></span>
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
<span data-ttu-id="84145-1375">las dos primeras declaraciones de operador se permiten porque, para los fines de los [indizadores](classes.md#indexers). 3 `T` , `int` y `string` y respectivamente se consideran tipos únicos sin relación.</span><span class="sxs-lookup"><span data-stu-id="84145-1375">the first two operator declarations are permitted because, for the purposes of [Indexers](classes.md#indexers).3, `T` and `int` and `string` respectively are considered unique types with no relationship.</span></span> <span data-ttu-id="84145-1376">Sin embargo, el tercer operador es un error `C<T>` porque es la clase base `D<T>`de.</span><span class="sxs-lookup"><span data-stu-id="84145-1376">However, the third operator is an error because `C<T>` is the base class of `D<T>`.</span></span>

<span data-ttu-id="84145-1377">En la segunda regla, se sigue que un operador de conversión debe convertir a o desde el tipo de clase o estructura en el que se declara el operador.</span><span class="sxs-lookup"><span data-stu-id="84145-1377">From the second rule it follows that a conversion operator must convert either to or from the class or struct type in which the operator is declared.</span></span> <span data-ttu-id="84145-1378">Por ejemplo, es `C` posible que un tipo de clase o estructura defina una conversión de `C` a `int` y de `int` a `C`, pero no de `int` a `bool`.</span><span class="sxs-lookup"><span data-stu-id="84145-1378">For example, it is possible for a class or struct type `C` to define a conversion from `C` to `int` and from `int` to `C`, but not from `int` to `bool`.</span></span>

<span data-ttu-id="84145-1379">No es posible volver a definir directamente una conversión predefinida.</span><span class="sxs-lookup"><span data-stu-id="84145-1379">It is not possible to directly redefine a pre-defined conversion.</span></span> <span data-ttu-id="84145-1380">Por lo tanto, no se permite que los operadores de conversión `object` se conviertan de o a porque ya existen `object` conversiones implícitas y explícitas entre y todos los demás tipos.</span><span class="sxs-lookup"><span data-stu-id="84145-1380">Thus, conversion operators are not allowed to convert from or to `object` because implicit and explicit conversions already exist between `object` and all other types.</span></span> <span data-ttu-id="84145-1381">Del mismo modo, ni los tipos de origen ni de destino de una conversión pueden ser un tipo base del otro, ya que una conversión ya existirá.</span><span class="sxs-lookup"><span data-stu-id="84145-1381">Likewise, neither the source nor the target types of a conversion can be a base type of the other, since a conversion would then already exist.</span></span>

<span data-ttu-id="84145-1382">Sin embargo, es posible declarar operadores en tipos genéricos que, para argumentos de tipo concretos, especifiquen conversiones que ya existan como conversiones predefinidas.</span><span class="sxs-lookup"><span data-stu-id="84145-1382">However, it is possible to declare operators on generic types that, for particular type arguments, specify conversions that already exist as pre-defined conversions.</span></span> <span data-ttu-id="84145-1383">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1383">In the example</span></span>
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
<span data-ttu-id="84145-1384">Cuando el `object` tipo se especifica como un argumento de `T`tipo para, el segundo operador declara una conversión que ya existe (una conversión implícita y, por tanto, también una conversión explícita de cualquier tipo `object`al tipo).</span><span class="sxs-lookup"><span data-stu-id="84145-1384">when type `object` is specified as a type argument for `T`, the second operator declares a conversion that already exists (an implicit, and therefore also an explicit, conversion exists from any type to type `object`).</span></span>

<span data-ttu-id="84145-1385">En los casos en los que existe una conversión predefinida entre dos tipos, se omiten las conversiones definidas por el usuario entre esos tipos.</span><span class="sxs-lookup"><span data-stu-id="84145-1385">In cases where a pre-defined conversion exists between two types, any user-defined conversions between those types are ignored.</span></span> <span data-ttu-id="84145-1386">De manera específica:</span><span class="sxs-lookup"><span data-stu-id="84145-1386">Specifically:</span></span>

*  <span data-ttu-id="84145-1387">Si existe una conversión implícita predefinida ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo `S` al tipo `T`, se omiten todas las conversiones definidas por el usuario (implícitas o explícitas `T` ) de `S` a.</span><span class="sxs-lookup"><span data-stu-id="84145-1387">If a pre-defined implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from type `S` to type `T`, all user-defined conversions (implicit or explicit) from `S` to `T` are ignored.</span></span>
*  <span data-ttu-id="84145-1388">Si existe una conversión explícita predefinida ([conversiones explícitas](conversions.md#explicit-conversions)) del tipo `S` al tipo `T`, se omiten las conversiones explícitas definidas por `S` el `T` usuario de a.</span><span class="sxs-lookup"><span data-stu-id="84145-1388">If a pre-defined explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) exists from type `S` to type `T`, any user-defined explicit conversions from `S` to `T` are ignored.</span></span> <span data-ttu-id="84145-1389">Por</span><span class="sxs-lookup"><span data-stu-id="84145-1389">Furthermore:</span></span>

<span data-ttu-id="84145-1390">Si `T` es un tipo de interfaz, se omiten las conversiones implícitas `S` definidas `T` por el usuario de a.</span><span class="sxs-lookup"><span data-stu-id="84145-1390">If `T` is an interface type, user-defined implicit conversions from `S` to `T` are ignored.</span></span>

<span data-ttu-id="84145-1391">De lo contrario, las conversiones implícitas definidas por `S` el `T` usuario de a se siguen teniendo en cuenta.</span><span class="sxs-lookup"><span data-stu-id="84145-1391">Otherwise, user-defined implicit conversions from `S` to `T` are still considered.</span></span>

<span data-ttu-id="84145-1392">En todos los tipos `object`, pero los operadores declarados por el `Convertible<T>` tipo anterior no entran en conflicto con las conversiones predefinidas.</span><span class="sxs-lookup"><span data-stu-id="84145-1392">For all types but `object`, the operators declared by the `Convertible<T>` type above do not conflict with pre-defined conversions.</span></span> <span data-ttu-id="84145-1393">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-1393">For example:</span></span>
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

<span data-ttu-id="84145-1394">Sin embargo, para `object`el tipo, las conversiones predefinidas ocultan las conversiones definidas por el usuario en todos los casos, pero una:</span><span class="sxs-lookup"><span data-stu-id="84145-1394">However, for type `object`, pre-defined conversions hide the user-defined conversions in all cases but one:</span></span>

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

<span data-ttu-id="84145-1395">No se permiten conversiones definidas por el usuario para convertir de o a *interface_type*s.</span><span class="sxs-lookup"><span data-stu-id="84145-1395">User-defined conversions are not allowed to convert from or to *interface_type*s.</span></span> <span data-ttu-id="84145-1396">En concreto, esta restricción garantiza que no se produzcan transformaciones definidas por el usuario al realizar la conversión a un *interface_type*y que una conversión a un *interface_type* se realice correctamente solo si el objeto que se convierte realmente implementa el *interface_type*especificado.</span><span class="sxs-lookup"><span data-stu-id="84145-1396">In particular, this restriction ensures that no user-defined transformations occur when converting to an *interface_type*, and that a conversion to an *interface_type* succeeds only if the object being converted actually implements the specified *interface_type*.</span></span>

<span data-ttu-id="84145-1397">La firma de un operador de conversión está formada por el tipo de origen y el tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="84145-1397">The signature of a conversion operator consists of the source type and the target type.</span></span> <span data-ttu-id="84145-1398">(Tenga en cuenta que esta es la única forma de miembro para la que participa el tipo de valor devuelto en la firma.) La `implicit` clasificación `explicit` o de un operador de conversión no forma parte de la firma del operador.</span><span class="sxs-lookup"><span data-stu-id="84145-1398">(Note that this is the only form of member for which the return type participates in the signature.) The `implicit` or `explicit` classification of a conversion operator is not part of the operator's signature.</span></span> <span data-ttu-id="84145-1399">Por lo tanto, una clase o struct no puede `implicit` declarar un `explicit` operador de conversión y con los mismos tipos de origen y de destino.</span><span class="sxs-lookup"><span data-stu-id="84145-1399">Thus, a class or struct cannot declare both an `implicit` and an `explicit` conversion operator with the same source and target types.</span></span>

<span data-ttu-id="84145-1400">En general, las conversiones implícitas definidas por el usuario deben diseñarse para que nunca se produzcan excepciones y nunca se pierda información.</span><span class="sxs-lookup"><span data-stu-id="84145-1400">In general, user-defined implicit conversions should be designed to never throw exceptions and never lose information.</span></span> <span data-ttu-id="84145-1401">Si una conversión definida por el usuario puede dar lugar a excepciones (por ejemplo, porque el argumento de origen está fuera del intervalo) o la pérdida de información (como descartar los bits de orden superior), esa conversión debe definirse como una conversión explícita.</span><span class="sxs-lookup"><span data-stu-id="84145-1401">If a user-defined conversion can give rise to exceptions (for example, because the source argument is out of range) or loss of information (such as discarding high-order bits), then that conversion should be defined as an explicit conversion.</span></span>

<span data-ttu-id="84145-1402">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1402">In the example</span></span>
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
<span data-ttu-id="84145-1403">la conversión de `Digit` a `byte` es implícita porque nunca produce excepciones o pierde información, pero la conversión de `byte` a `Digit` es explícita, ya que `Digit` solo puede representar un subconjunto del posible valores de `byte`.</span><span class="sxs-lookup"><span data-stu-id="84145-1403">the conversion from `Digit` to `byte` is implicit because it never throws exceptions or loses information, but the conversion from `byte` to `Digit` is explicit since `Digit` can only represent a subset of the possible values of a `byte`.</span></span>

## <a name="instance-constructors"></a><span data-ttu-id="84145-1404">Constructores de instancias</span><span class="sxs-lookup"><span data-stu-id="84145-1404">Instance constructors</span></span>

<span data-ttu-id="84145-1405">Un ***constructor de instancia*** es un miembro que implementa las acciones necesarias para inicializar una instancia de una clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1405">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="84145-1406">Los constructores de instancias se declaran mediante *constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="84145-1406">Instance constructors are declared using *constructor_declaration*s:</span></span>

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

<span data-ttu-id="84145-1407">Un *constructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)) y un modificador `extern` ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="84145-1407">A *constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)), a valid combination of the four access modifiers ([Access modifiers](classes.md#access-modifiers)), and an `extern` ([External methods](classes.md#external-methods)) modifier.</span></span> <span data-ttu-id="84145-1408">No se permite que una declaración de constructor incluya el mismo modificador varias veces.</span><span class="sxs-lookup"><span data-stu-id="84145-1408">A constructor declaration is not permitted to include the same modifier multiple times.</span></span>

<span data-ttu-id="84145-1409">El *identificador* de un *constructor_declarator* debe nombrar la clase en la que se declara el constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-1409">The *identifier* of a *constructor_declarator* must name the class in which the instance constructor is declared.</span></span> <span data-ttu-id="84145-1410">Si se especifica cualquier otro nombre, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-1410">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="84145-1411">El *formal_parameter_list* opcional de un constructor de instancia está sujeto a las mismas reglas que el *formal_parameter_list* de un método ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="84145-1411">The optional *formal_parameter_list* of an instance constructor is subject to the same rules as the *formal_parameter_list* of a method ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="84145-1412">La lista de parámetros formales define la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un constructor de instancia y rige el proceso por el cual la resolución de sobrecarga ([inferencia de tipos](expressions.md#type-inference)) selecciona un constructor de instancia determinado en una invocación.</span><span class="sxs-lookup"><span data-stu-id="84145-1412">The formal parameter list defines the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of an instance constructor and governs the process whereby overload resolution ([Type inference](expressions.md#type-inference)) selects a particular instance constructor in an invocation.</span></span>

<span data-ttu-id="84145-1413">Cada uno de los tipos a los que se hace referencia en *formal_parameter_list* de un constructor de instancia debe ser al menos igual de accesible que el propio constructor ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="84145-1413">Each of the types referenced in the *formal_parameter_list* of an instance constructor must be at least as accessible as the constructor itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span>

<span data-ttu-id="84145-1414">El *constructor_initializer* opcional especifica otro constructor de instancia que se va a invocar antes de ejecutar las instrucciones proporcionadas en el *constructor_body* de este constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-1414">The optional *constructor_initializer* specifies another instance constructor to invoke before executing the statements given in the *constructor_body* of this instance constructor.</span></span> <span data-ttu-id="84145-1415">Esto se describe con más detalle en [inicializadores de constructor](classes.md#constructor-initializers).</span><span class="sxs-lookup"><span data-stu-id="84145-1415">This is described further in [Constructor initializers](classes.md#constructor-initializers).</span></span>

<span data-ttu-id="84145-1416">Cuando una declaración de constructor incluye `extern` un modificador, se dice que el constructor es un ***constructor externo***.</span><span class="sxs-lookup"><span data-stu-id="84145-1416">When a constructor declaration includes an `extern` modifier, the constructor is said to be an ***external constructor***.</span></span> <span data-ttu-id="84145-1417">Dado que una declaración de constructor externo no proporciona ninguna implementación real, su *constructor_body* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-1417">Because an external constructor declaration provides no actual implementation, its *constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="84145-1418">En el caso de todos los demás constructores, *constructor_body* consta de un *bloque* que especifica las instrucciones para inicializar una nueva instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1418">For all other constructors, the *constructor_body* consists of a *block* which specifies the statements to initialize a new instance of the class.</span></span> <span data-ttu-id="84145-1419">Esto corresponde exactamente al *bloque* de un método de instancia con un `void` tipo de valor devuelto ([cuerpo del método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="84145-1419">This corresponds exactly to the *block* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="84145-1420">No se heredan los constructores de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-1420">Instance constructors are not inherited.</span></span> <span data-ttu-id="84145-1421">Por lo tanto, una clase no tiene ningún constructor de instancia que no sea el declarado realmente en la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1421">Thus, a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="84145-1422">Si una clase no contiene ninguna declaración de constructor de instancia, se proporciona automáticamente un constructor de instancia predeterminado ([constructores predeterminados](classes.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="84145-1422">If a class contains no instance constructor declarations, a default instance constructor is automatically provided ([Default constructors](classes.md#default-constructors)).</span></span>

<span data-ttu-id="84145-1423">Los constructores de instancia se invocan mediante *object_creation_expression*s ([expresiones de creación de objetos](expressions.md#object-creation-expressions)) y a través de *constructor_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="84145-1423">Instance constructors are invoked by *object_creation_expression*s ([Object creation expressions](expressions.md#object-creation-expressions)) and through *constructor_initializer*s.</span></span>

### <a name="constructor-initializers"></a><span data-ttu-id="84145-1424">Inicializadores del constructor</span><span class="sxs-lookup"><span data-stu-id="84145-1424">Constructor initializers</span></span>

<span data-ttu-id="84145-1425">Todos los constructores de instancia (excepto los de la clase `object`) incluyen implícitamente una invocación de otro constructor de instancia inmediatamente antes de *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="84145-1425">All instance constructors (except those for class `object`) implicitly include an invocation of another instance constructor immediately before the *constructor_body*.</span></span> <span data-ttu-id="84145-1426">El constructor para invocar implícitamente está determinado por *constructor_initializer*:</span><span class="sxs-lookup"><span data-stu-id="84145-1426">The constructor to implicitly invoke is determined by the *constructor_initializer*:</span></span>

*  <span data-ttu-id="84145-1427">Un inicializador de constructor de instancia con `base(argument_list)` el `base()` formato o hace que se invoque un constructor de instancia de la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="84145-1427">An instance constructor initializer of the form `base(argument_list)` or `base()` causes an instance constructor from the direct base class to be invoked.</span></span> <span data-ttu-id="84145-1428">Ese constructor se selecciona mediante *argument_list* si está presente y las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="84145-1428">That constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="84145-1429">El conjunto de constructores de instancias candidatas consta de todos los constructores de instancia accesibles contenidos en la clase base directa, o el constructor predeterminado ([constructores predeterminados](classes.md#default-constructors)), si no se declara ningún constructor de instancia en la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="84145-1429">The set of candidate instance constructors consists of all accessible instance constructors contained in the direct base class, or the default constructor ([Default constructors](classes.md#default-constructors)), if no instance constructors are declared in the direct base class.</span></span> <span data-ttu-id="84145-1430">Si este conjunto está vacío, o si no se puede identificar un único constructor de instancia mejor, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-1430">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span>
*  <span data-ttu-id="84145-1431">Un inicializador de constructor de instancia con `this(argument-list)` el `this()` formato o hace que se invoque un constructor de instancia de la propia clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1431">An instance constructor initializer of the form `this(argument-list)` or `this()` causes an instance constructor from the class itself to be invoked.</span></span> <span data-ttu-id="84145-1432">El constructor se selecciona mediante *argument_list* si está presente y las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="84145-1432">The constructor is selected using *argument_list* if present and the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="84145-1433">El conjunto de constructores de instancias candidatas se compone de todos los constructores de instancia accesibles declarados en la propia clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1433">The set of candidate instance constructors consists of all accessible instance constructors declared in the class itself.</span></span> <span data-ttu-id="84145-1434">Si este conjunto está vacío, o si no se puede identificar un único constructor de instancia mejor, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-1434">If this set is empty, or if a single best instance constructor cannot be identified, a compile-time error occurs.</span></span> <span data-ttu-id="84145-1435">Si una declaración de constructor de instancia incluye un inicializador de constructor que invoca al propio constructor, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-1435">If an instance constructor declaration includes a constructor initializer that invokes the constructor itself, a compile-time error occurs.</span></span>

<span data-ttu-id="84145-1436">Si un constructor de instancia no tiene ningún inicializador de constructor, se proporciona implícitamente `base()` un inicializador de constructor con el formato.</span><span class="sxs-lookup"><span data-stu-id="84145-1436">If an instance constructor has no constructor initializer, a constructor initializer of the form `base()` is implicitly provided.</span></span> <span data-ttu-id="84145-1437">Por lo tanto, una declaración de constructor de instancia con el formato</span><span class="sxs-lookup"><span data-stu-id="84145-1437">Thus, an instance constructor declaration of the form</span></span>
```csharp
C(...) {...}
```
<span data-ttu-id="84145-1438">es exactamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="84145-1438">is exactly equivalent to</span></span>
```csharp
C(...): base() {...}
```

<span data-ttu-id="84145-1439">El ámbito de los parámetros proporcionados por el *formal_parameter_list* de una declaración de constructor de instancia incluye el inicializador de constructor de esa declaración.</span><span class="sxs-lookup"><span data-stu-id="84145-1439">The scope of the parameters given by the *formal_parameter_list* of an instance constructor declaration includes the constructor initializer of that declaration.</span></span> <span data-ttu-id="84145-1440">Por lo tanto, se permite a un inicializador de constructor tener acceso a los parámetros del constructor.</span><span class="sxs-lookup"><span data-stu-id="84145-1440">Thus, a constructor initializer is permitted to access the parameters of the constructor.</span></span> <span data-ttu-id="84145-1441">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-1441">For example:</span></span>
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

<span data-ttu-id="84145-1442">Un inicializador de constructor de instancia no puede tener acceso a la instancia que se está creando.</span><span class="sxs-lookup"><span data-stu-id="84145-1442">An instance constructor initializer cannot access the instance being created.</span></span> <span data-ttu-id="84145-1443">Por lo tanto, es un error en tiempo de compilación hacer referencia a `this` en una expresión de argumento del inicializador de constructor, como es un error en tiempo de compilación para que una expresión de argumento haga referencia a un miembro de instancia a través de un *simple_name*.</span><span class="sxs-lookup"><span data-stu-id="84145-1443">Therefore it is a compile-time error to reference `this` in an argument expression of the constructor initializer, as is it a compile-time error for an argument expression to reference any instance member through a *simple_name*.</span></span>

### <a name="instance-variable-initializers"></a><span data-ttu-id="84145-1444">Inicializadores de variables de instancia</span><span class="sxs-lookup"><span data-stu-id="84145-1444">Instance variable initializers</span></span>

<span data-ttu-id="84145-1445">Cuando un constructor de instancia no tiene ningún inicializador de constructor o tiene un inicializador de constructor con el formato `base(...)`, ese constructor realiza implícitamente las inicializaciones especificadas por *variable_initializer*s de los campos de instancia declarados en su clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1445">When an instance constructor has no constructor initializer, or it has a constructor initializer of the form `base(...)`, that constructor implicitly performs the initializations specified by the *variable_initializer*s of the instance fields declared in its class.</span></span> <span data-ttu-id="84145-1446">Esto corresponde a una secuencia de asignaciones que se ejecutan inmediatamente después de la entrada al constructor y antes de la invocación implícita del constructor de clase base directo.</span><span class="sxs-lookup"><span data-stu-id="84145-1446">This corresponds to a sequence of assignments that are executed immediately upon entry to the constructor and before the implicit invocation of the direct base class constructor.</span></span> <span data-ttu-id="84145-1447">Los inicializadores de variable se ejecutan en el orden textual en el que aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1447">The variable initializers are executed in the textual order in which they appear in the class declaration.</span></span>

### <a name="constructor-execution"></a><span data-ttu-id="84145-1448">Ejecución del constructor</span><span class="sxs-lookup"><span data-stu-id="84145-1448">Constructor execution</span></span>

<span data-ttu-id="84145-1449">Los inicializadores de variables se transforman en instrucciones de asignación, y estas instrucciones de asignación se ejecutan antes de la invocación del constructor de instancia de clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-1449">Variable initializers are transformed into assignment statements, and these assignment statements are executed before the invocation of the base class instance constructor.</span></span> <span data-ttu-id="84145-1450">Este orden garantiza que todos los campos de instancia se inicializan mediante sus inicializadores de variable antes de que se ejecute cualquier instrucción que tenga acceso a esa instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-1450">This ordering ensures that all instance fields are initialized by their variable initializers before any statements that have access to that instance are executed.</span></span>

<span data-ttu-id="84145-1451">Dado el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1451">Given the example</span></span>
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
<span data-ttu-id="84145-1452">Cuando `new B()` se utiliza para crear una instancia de `B`, se genera el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="84145-1452">when `new B()` is used to create an instance of `B`, the following output is produced:</span></span>
```console
x = 1, y = 0
```

<span data-ttu-id="84145-1453">El valor de `x` es 1 porque el inicializador de variable se ejecuta antes de que se invoque el constructor de instancia de clase base.</span><span class="sxs-lookup"><span data-stu-id="84145-1453">The value of `x` is 1 because the variable initializer is executed before the base class instance constructor is invoked.</span></span> <span data-ttu-id="84145-1454">Sin embargo, el valor `y` de es 0 (el valor predeterminado `int`de) porque la asignación a `y` no se ejecuta hasta que el constructor de clase base devuelve.</span><span class="sxs-lookup"><span data-stu-id="84145-1454">However, the value of `y` is 0 (the default value of an `int`) because the assignment to `y` is not executed until after the base class constructor returns.</span></span>

<span data-ttu-id="84145-1455">Resulta útil pensar en los inicializadores de variables de instancia y en los inicializadores de constructor como instrucciones que se insertan automáticamente antes de *constructor_body*.</span><span class="sxs-lookup"><span data-stu-id="84145-1455">It is useful to think of instance variable initializers and constructor initializers as statements that are automatically inserted before the *constructor_body*.</span></span> <span data-ttu-id="84145-1456">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1456">The example</span></span>
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
<span data-ttu-id="84145-1457">contiene varios inicializadores variables; también contiene inicializadores de constructor de ambos formularios (`base` y `this`).</span><span class="sxs-lookup"><span data-stu-id="84145-1457">contains several variable initializers; it also contains constructor initializers of both forms (`base` and `this`).</span></span> <span data-ttu-id="84145-1458">El ejemplo corresponde al código que se muestra a continuación, donde cada comentario indica una instrucción insertada automáticamente (la sintaxis utilizada para las invocaciones de constructor insertadas automáticamente no es válida, pero simplemente sirve para ilustrar el mecanismo).</span><span class="sxs-lookup"><span data-stu-id="84145-1458">The example corresponds to the code shown below, where each comment indicates an automatically inserted statement (the syntax used for the automatically inserted constructor invocations isn't valid, but merely serves to illustrate the mechanism).</span></span>

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

### <a name="default-constructors"></a><span data-ttu-id="84145-1459">Constructores predeterminados</span><span class="sxs-lookup"><span data-stu-id="84145-1459">Default constructors</span></span>

<span data-ttu-id="84145-1460">Si una clase no contiene ninguna declaración de constructor de instancia, se proporciona automáticamente un constructor de instancia predeterminado.</span><span class="sxs-lookup"><span data-stu-id="84145-1460">If a class contains no instance constructor declarations, a default instance constructor is automatically provided.</span></span> <span data-ttu-id="84145-1461">Ese constructor predeterminado simplemente invoca el constructor sin parámetros de la clase base directa.</span><span class="sxs-lookup"><span data-stu-id="84145-1461">That default constructor simply invokes the parameterless constructor of the direct base class.</span></span> <span data-ttu-id="84145-1462">Si la clase es abstracta, se protege la accesibilidad declarada para el constructor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="84145-1462">If the class is abstract then the declared accessibility for the default constructor is protected.</span></span> <span data-ttu-id="84145-1463">De lo contrario, la accesibilidad declarada para el constructor predeterminado es Public.</span><span class="sxs-lookup"><span data-stu-id="84145-1463">Otherwise, the declared accessibility for the default constructor is public.</span></span> <span data-ttu-id="84145-1464">Por lo tanto, el constructor predeterminado siempre tiene el formato</span><span class="sxs-lookup"><span data-stu-id="84145-1464">Thus, the default constructor is always of the form</span></span>

```csharp
protected C(): base() {}
```
<span data-ttu-id="84145-1465">o</span><span class="sxs-lookup"><span data-stu-id="84145-1465">or</span></span>
```csharp
public C(): base() {}
```
<span data-ttu-id="84145-1466">donde `C` es el nombre de la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1466">where `C` is the name of the class.</span></span> <span data-ttu-id="84145-1467">Si la resolución de sobrecarga no puede determinar un mejor candidato único para el inicializador de constructor de clase base, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-1467">If overload resolution is unable to determine a unique best candidate for the base class constructor initializer then a compile-time error occurs.</span></span>

<span data-ttu-id="84145-1468">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1468">In the example</span></span>
```csharp
class Message
{
    object sender;
    string text;
}
```
<span data-ttu-id="84145-1469">se proporciona un constructor predeterminado porque la clase no contiene ninguna declaración de constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-1469">a default constructor is provided because the class contains no instance constructor declarations.</span></span> <span data-ttu-id="84145-1470">Por lo tanto, el ejemplo es exactamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="84145-1470">Thus, the example is precisely equivalent to</span></span>
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a><span data-ttu-id="84145-1471">Constructores privados</span><span class="sxs-lookup"><span data-stu-id="84145-1471">Private constructors</span></span>

<span data-ttu-id="84145-1472">Cuando una clase `T` declara solo constructores de instancia privados, no es posible que las clases fuera del texto del programa `T` de deriven `T` de o creen directamente instancias de `T`.</span><span class="sxs-lookup"><span data-stu-id="84145-1472">When a class `T` declares only private instance constructors, it is not possible for classes outside the program text of `T` to derive from `T` or to directly create instances of `T`.</span></span> <span data-ttu-id="84145-1473">Por lo tanto, si una clase solo contiene miembros estáticos y no está pensado para que se creen instancias, agregar un constructor de instancia privado vacío impedirá la creación de instancias.</span><span class="sxs-lookup"><span data-stu-id="84145-1473">Thus, if a class contains only static members and isn't intended to be instantiated, adding an empty private instance constructor will prevent instantiation.</span></span> <span data-ttu-id="84145-1474">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84145-1474">For example:</span></span>
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

<span data-ttu-id="84145-1475">La `Trig` clase agrupa los métodos relacionados y las constantes, pero no está previsto que se creen instancias de ellos.</span><span class="sxs-lookup"><span data-stu-id="84145-1475">The `Trig` class groups related methods and constants, but is not intended to be instantiated.</span></span> <span data-ttu-id="84145-1476">Por lo tanto, declara un único constructor de instancia privado vacío.</span><span class="sxs-lookup"><span data-stu-id="84145-1476">Therefore it declares a single empty private instance constructor.</span></span> <span data-ttu-id="84145-1477">Se debe declarar al menos un constructor de instancia para suprimir la generación automática de un constructor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="84145-1477">At least one instance constructor must be declared to suppress the automatic generation of a default constructor.</span></span>

### <a name="optional-instance-constructor-parameters"></a><span data-ttu-id="84145-1478">Parámetros de constructor de instancia opcionales</span><span class="sxs-lookup"><span data-stu-id="84145-1478">Optional instance constructor parameters</span></span>

<span data-ttu-id="84145-1479">La `this(...)` forma de inicializador de constructor se utiliza normalmente junto con la sobrecarga para implementar parámetros de constructor de instancia opcionales.</span><span class="sxs-lookup"><span data-stu-id="84145-1479">The `this(...)` form of constructor initializer is commonly used in conjunction with overloading to implement optional instance constructor parameters.</span></span> <span data-ttu-id="84145-1480">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1480">In the example</span></span>
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
<span data-ttu-id="84145-1481">los dos primeros constructores de instancia simplemente proporcionan los valores predeterminados para los argumentos que faltan.</span><span class="sxs-lookup"><span data-stu-id="84145-1481">the first two instance constructors merely provide the default values for the missing arguments.</span></span> <span data-ttu-id="84145-1482">Ambos usan un `this(...)` inicializador de constructor para invocar el tercer constructor de instancia, que realmente realiza el trabajo de inicializar la nueva instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-1482">Both use a `this(...)` constructor initializer to invoke the third instance constructor, which actually does the work of initializing the new instance.</span></span> <span data-ttu-id="84145-1483">El efecto es el de los parámetros de constructor opcionales:</span><span class="sxs-lookup"><span data-stu-id="84145-1483">The effect is that of optional constructor parameters:</span></span>
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a><span data-ttu-id="84145-1484">Constructores estáticos</span><span class="sxs-lookup"><span data-stu-id="84145-1484">Static constructors</span></span>

<span data-ttu-id="84145-1485">Un ***constructor estático*** es un miembro que implementa las acciones necesarias para inicializar un tipo de clase cerrada.</span><span class="sxs-lookup"><span data-stu-id="84145-1485">A ***static constructor*** is a member that implements the actions required to initialize a closed class type.</span></span> <span data-ttu-id="84145-1486">Los constructores estáticos se declaran mediante *static_constructor_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="84145-1486">Static constructors are declared using *static_constructor_declaration*s:</span></span>

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

<span data-ttu-id="84145-1487">Un *static_constructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y un modificador `extern` ([métodos externos](classes.md#external-methods)).</span><span class="sxs-lookup"><span data-stu-id="84145-1487">A *static_constructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)) and an `extern` modifier ([External methods](classes.md#external-methods)).</span></span>

<span data-ttu-id="84145-1488">El *identificador* de un *static_constructor_declaration* debe nombrar la clase en la que se declara el constructor estático.</span><span class="sxs-lookup"><span data-stu-id="84145-1488">The *identifier* of a *static_constructor_declaration* must name the class in which the static constructor is declared.</span></span> <span data-ttu-id="84145-1489">Si se especifica cualquier otro nombre, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-1489">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="84145-1490">Cuando una declaración de constructor estático incluye `extern` un modificador, se dice que el constructor estático es un ***constructor estático externo***.</span><span class="sxs-lookup"><span data-stu-id="84145-1490">When a static constructor declaration includes an `extern` modifier, the static constructor is said to be an ***external static constructor***.</span></span> <span data-ttu-id="84145-1491">Dado que una declaración de constructor estático externo no proporciona ninguna implementación real, su *static_constructor_body* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-1491">Because an external static constructor declaration provides no actual implementation, its *static_constructor_body* consists of a semicolon.</span></span> <span data-ttu-id="84145-1492">Para todas las demás declaraciones de constructores estáticos, *static_constructor_body* consta de un *bloque* que especifica las instrucciones que se ejecutan para inicializar la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1492">For all other static constructor declarations, the *static_constructor_body* consists of a *block* which specifies the statements to execute in order to initialize the class.</span></span> <span data-ttu-id="84145-1493">Esto se corresponde exactamente con el *method_body* de un método estático con un tipo de valor devuelto `void` ([cuerpo del método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="84145-1493">This corresponds exactly to the *method_body* of a static method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="84145-1494">Los constructores estáticos no se heredan y no se pueden llamar directamente.</span><span class="sxs-lookup"><span data-stu-id="84145-1494">Static constructors are not inherited, and cannot be called directly.</span></span>

<span data-ttu-id="84145-1495">El constructor estático de un tipo de clase cerrada se ejecuta como máximo una vez en un dominio de aplicación determinado.</span><span class="sxs-lookup"><span data-stu-id="84145-1495">The static constructor for a closed class type executes at most once in a given application domain.</span></span> <span data-ttu-id="84145-1496">La ejecución de un constructor estático lo desencadena el primero de los siguientes eventos para que se produzca dentro de un dominio de aplicación:</span><span class="sxs-lookup"><span data-stu-id="84145-1496">The execution of a static constructor is triggered by the first of the following events to occur within an application domain:</span></span>

*  <span data-ttu-id="84145-1497">Se crea una instancia del tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1497">An instance of the class type is created.</span></span>
*  <span data-ttu-id="84145-1498">Se hace referencia a cualquiera de los miembros estáticos del tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1498">Any of the static members of the class type are referenced.</span></span>

<span data-ttu-id="84145-1499">Si una clase contiene el `Main` método ([Inicio](basic-concepts.md#application-startup)de la aplicación) en el que comienza la ejecución, el constructor estático de esa clase `Main` se ejecuta antes de que se llame al método.</span><span class="sxs-lookup"><span data-stu-id="84145-1499">If a class contains the `Main` method ([Application Startup](basic-concepts.md#application-startup)) in which execution begins, the static constructor for that class executes before the `Main` method is called.</span></span>

<span data-ttu-id="84145-1500">Para inicializar un nuevo tipo de clase cerrada, primero se crea un nuevo conjunto de campos estáticos ([campos estáticos y de instancia](classes.md#static-and-instance-fields)) para ese tipo cerrado en particular.</span><span class="sxs-lookup"><span data-stu-id="84145-1500">To initialize a new closed class type, first a new set of static fields ([Static and instance fields](classes.md#static-and-instance-fields)) for that particular closed type is created.</span></span> <span data-ttu-id="84145-1501">Cada uno de los campos estáticos se inicializa en su valor predeterminado ([valores predeterminados](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="84145-1501">Each of the static fields is initialized to its default value ([Default values](variables.md#default-values)).</span></span> <span data-ttu-id="84145-1502">A continuación, se ejecutan los inicializadores de campo estáticos ([inicialización de campos estáticos](classes.md#static-field-initialization)) para esos campos estáticos.</span><span class="sxs-lookup"><span data-stu-id="84145-1502">Next, the static field initializers ([Static field initialization](classes.md#static-field-initialization)) are executed for those static fields.</span></span> <span data-ttu-id="84145-1503">Por último, se ejecuta el constructor estático.</span><span class="sxs-lookup"><span data-stu-id="84145-1503">Finally, the static constructor is executed.</span></span>

<span data-ttu-id="84145-1504">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1504">The example</span></span>
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
<span data-ttu-id="84145-1505">debe generar la salida:</span><span class="sxs-lookup"><span data-stu-id="84145-1505">must produce the output:</span></span>
```console
Init A
A.F
Init B
B.F
```
<span data-ttu-id="84145-1506">Dado que la llamada `A`a `A.F`desencadena la ejecución del constructor estático de, y la llamada a `B.F`desencadena la `B`ejecución del constructor estático de.</span><span class="sxs-lookup"><span data-stu-id="84145-1506">because the execution of `A`'s static constructor is triggered by the call to `A.F`, and the execution of `B`'s static constructor is triggered by the call to `B.F`.</span></span>

<span data-ttu-id="84145-1507">Es posible crear dependencias circulares que permitan observar los campos estáticos con inicializadores variables en su estado de valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="84145-1507">It is possible to construct circular dependencies that allow static fields with variable initializers to be observed in their default value state.</span></span>

<span data-ttu-id="84145-1508">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1508">The example</span></span>
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
<span data-ttu-id="84145-1509">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="84145-1509">produces the output</span></span>
```console
X = 1, Y = 2
```

<span data-ttu-id="84145-1510">Para ejecutar el `Main` método, el sistema primero ejecuta el inicializador para `B.Y`, antes del constructor `B`estático de la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1510">To execute the `Main` method, the system first runs the initializer for `B.Y`, prior to class `B`'s static constructor.</span></span> <span data-ttu-id="84145-1511">`Y`el inicializador de `A`hace que el constructor estático de se ejecute porque se `A.X` hace referencia al valor de.</span><span class="sxs-lookup"><span data-stu-id="84145-1511">`Y`'s initializer causes `A`'s static constructor to be run because the value of `A.X` is referenced.</span></span> <span data-ttu-id="84145-1512">El constructor estático de `A` a su vez continúa para calcular el valor de `X`y, al hacerlo, recupera el valor predeterminado de `Y`, que es cero.</span><span class="sxs-lookup"><span data-stu-id="84145-1512">The static constructor of `A` in turn proceeds to compute the value of `X`, and in doing so fetches the default value of `Y`, which is zero.</span></span> <span data-ttu-id="84145-1513">`A.X`por tanto, se inicializa en 1.</span><span class="sxs-lookup"><span data-stu-id="84145-1513">`A.X` is thus initialized to 1.</span></span> <span data-ttu-id="84145-1514">El proceso de ejecución `A`de los inicializadores de campo estáticos y del constructor estático se completa y vuelve al cálculo del valor inicial `Y`de, cuyo resultado es 2.</span><span class="sxs-lookup"><span data-stu-id="84145-1514">The process of running `A`'s static field initializers and static constructor then completes, returning to the calculation of the initial value of `Y`, the result of which becomes 2.</span></span>

<span data-ttu-id="84145-1515">Dado que el constructor estático se ejecuta exactamente una vez para cada tipo de clase construido cerrado, es un lugar cómodo para aplicar comprobaciones en tiempo de ejecución en el parámetro de tipo que no se puede comprobar en tiempo de compilación a través de restricciones ([restricciones de parámetros de tipo](classes.md#type-parameter-constraints)). .</span><span class="sxs-lookup"><span data-stu-id="84145-1515">Because the static constructor is executed exactly once for each closed constructed class type, it is a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile-time via constraints ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="84145-1516">Por ejemplo, el tipo siguiente usa un constructor estático para exigir que el argumento de tipo sea una enumeración:</span><span class="sxs-lookup"><span data-stu-id="84145-1516">For example, the following type uses a static constructor to enforce that the type argument is an enum:</span></span>
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

## <a name="destructors"></a><span data-ttu-id="84145-1517">Destructores</span><span class="sxs-lookup"><span data-stu-id="84145-1517">Destructors</span></span>

<span data-ttu-id="84145-1518">Un ***destructor*** es un miembro que implementa las acciones necesarias para destruir una instancia de una clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1518">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="84145-1519">Un destructor se declara mediante *destructor_declaration*:</span><span class="sxs-lookup"><span data-stu-id="84145-1519">A destructor is declared using a *destructor_declaration*:</span></span>

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

<span data-ttu-id="84145-1520">Un *destructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="84145-1520">A *destructor_declaration* may include a set of *attributes* ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="84145-1521">El *identificador* de un *destructor_declaration* debe nombrar la clase en la que se declara el destructor.</span><span class="sxs-lookup"><span data-stu-id="84145-1521">The *identifier* of a *destructor_declaration* must name the class in which the destructor is declared.</span></span> <span data-ttu-id="84145-1522">Si se especifica cualquier otro nombre, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="84145-1522">If any other name is specified, a compile-time error occurs.</span></span>

<span data-ttu-id="84145-1523">Cuando una declaración de destructor incluye `extern` un modificador, se dice que es un ***destructor externo***.</span><span class="sxs-lookup"><span data-stu-id="84145-1523">When a destructor declaration includes an `extern` modifier, the destructor is said to be an ***external destructor***.</span></span> <span data-ttu-id="84145-1524">Dado que una declaración de destructor externo no proporciona ninguna implementación real, su *destructor_body* consta de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="84145-1524">Because an external destructor declaration provides no actual implementation, its *destructor_body* consists of a semicolon.</span></span> <span data-ttu-id="84145-1525">En el caso de todos los demás destructores, *destructor_body* consta de un *bloque* que especifica las instrucciones que se van a ejecutar para desestructurar una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1525">For all other destructors, the *destructor_body* consists of a *block* which specifies the statements to execute in order to destruct an instance of the class.</span></span> <span data-ttu-id="84145-1526">Un *destructor_body* se corresponde exactamente con el *method_body* de un método de instancia con un tipo de valor devuelto `void` ([cuerpo del método](classes.md#method-body)).</span><span class="sxs-lookup"><span data-stu-id="84145-1526">A *destructor_body* corresponds exactly to the *method_body* of an instance method with a `void` return type ([Method body](classes.md#method-body)).</span></span>

<span data-ttu-id="84145-1527">Los destructores no se heredan.</span><span class="sxs-lookup"><span data-stu-id="84145-1527">Destructors are not inherited.</span></span> <span data-ttu-id="84145-1528">Por lo tanto, una clase no tiene ningún destructor que no sea el que se puede declarar en esa clase.</span><span class="sxs-lookup"><span data-stu-id="84145-1528">Thus, a class has no destructors other than the one which may be declared in that class.</span></span>

<span data-ttu-id="84145-1529">Dado que un destructor debe tener ningún parámetro, no se puede sobrecargar, por lo que una clase puede tener, como máximo, un destructor.</span><span class="sxs-lookup"><span data-stu-id="84145-1529">Since a destructor is required to have no parameters, it cannot be overloaded, so a class can have, at most, one destructor.</span></span>

<span data-ttu-id="84145-1530">Los destructores se invocan automáticamente y no se pueden invocar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="84145-1530">Destructors are invoked automatically, and cannot be invoked explicitly.</span></span> <span data-ttu-id="84145-1531">Una instancia pasa a ser válida para su destrucción cuando ya no es posible para cualquier código utilizar esa instancia.</span><span class="sxs-lookup"><span data-stu-id="84145-1531">An instance becomes eligible for destruction when it is no longer possible for any code to use that instance.</span></span> <span data-ttu-id="84145-1532">La ejecución del destructor para la instancia puede producirse en cualquier momento después de que la instancia sea válida para la destrucción.</span><span class="sxs-lookup"><span data-stu-id="84145-1532">Execution of the destructor for the instance may occur at any time after the instance becomes eligible for destruction.</span></span> <span data-ttu-id="84145-1533">Cuando se destruye una instancia, se llama a los destructores de la cadena de herencia de la instancia, en orden, de la más derivada a la menos derivada.</span><span class="sxs-lookup"><span data-stu-id="84145-1533">When an instance is destructed, the destructors in that instance's inheritance chain are called, in order, from most derived to least derived.</span></span> <span data-ttu-id="84145-1534">Se puede ejecutar un destructor en cualquier subproceso.</span><span class="sxs-lookup"><span data-stu-id="84145-1534">A destructor may be executed on any thread.</span></span> <span data-ttu-id="84145-1535">Para obtener más información sobre las reglas que rigen cuándo y cómo se ejecuta un destructor, consulte [Administración automática](basic-concepts.md#automatic-memory-management)de la memoria.</span><span class="sxs-lookup"><span data-stu-id="84145-1535">For further discussion of the rules that govern when and how a destructor is executed, see [Automatic memory management](basic-concepts.md#automatic-memory-management).</span></span>

<span data-ttu-id="84145-1536">La salida del ejemplo</span><span class="sxs-lookup"><span data-stu-id="84145-1536">The output of the example</span></span>
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
<span data-ttu-id="84145-1537">is</span><span class="sxs-lookup"><span data-stu-id="84145-1537">is</span></span>
```
B's destructor
A's destructor
```
<span data-ttu-id="84145-1538">Dado que se llama a los destructores en una cadena de herencia en orden, desde la más derivada hasta la menos derivada.</span><span class="sxs-lookup"><span data-stu-id="84145-1538">since destructors in an inheritance chain are called in order, from most derived to least derived.</span></span>

<span data-ttu-id="84145-1539">Los destructores se implementan invalidando el método `Finalize` virtual `System.Object`en.</span><span class="sxs-lookup"><span data-stu-id="84145-1539">Destructors are implemented by overriding the virtual method `Finalize` on `System.Object`.</span></span> <span data-ttu-id="84145-1540">C#no se permite a los programas invalidar este método ni llamarlo (o invalidarlo) directamente.</span><span class="sxs-lookup"><span data-stu-id="84145-1540">C# programs are not permitted to override this method or call it (or overrides of it) directly.</span></span> <span data-ttu-id="84145-1541">Por ejemplo, el programa</span><span class="sxs-lookup"><span data-stu-id="84145-1541">For instance, the program</span></span>
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
<span data-ttu-id="84145-1542">contiene dos errores.</span><span class="sxs-lookup"><span data-stu-id="84145-1542">contains two errors.</span></span>

<span data-ttu-id="84145-1543">El compilador se comporta como si este método, y los reemplaza, no existen en absoluto.</span><span class="sxs-lookup"><span data-stu-id="84145-1543">The compiler behaves as if this method, and overrides of it, do not exist at all.</span></span> <span data-ttu-id="84145-1544">Por lo tanto, este programa:</span><span class="sxs-lookup"><span data-stu-id="84145-1544">Thus, this program:</span></span>
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
<span data-ttu-id="84145-1545">es válido y el método mostrado oculta `System.Object`el método de. `Finalize`</span><span class="sxs-lookup"><span data-stu-id="84145-1545">is valid, and the method shown hides `System.Object`'s `Finalize` method.</span></span>

<span data-ttu-id="84145-1546">Para obtener una explicación del comportamiento cuando se produce una excepción desde un destructor, vea [cómo se controlan las excepciones](exceptions.md#how-exceptions-are-handled).</span><span class="sxs-lookup"><span data-stu-id="84145-1546">For a discussion of the behavior when an exception is thrown from a destructor, see [How exceptions are handled](exceptions.md#how-exceptions-are-handled).</span></span>

## <a name="iterators"></a><span data-ttu-id="84145-1547">Iterators</span><span class="sxs-lookup"><span data-stu-id="84145-1547">Iterators</span></span>

<span data-ttu-id="84145-1548">Un miembro de función ([miembros de función](expressions.md#function-members)) implementado mediante un bloque de iteradores ([bloques](statements.md#blocks)) se denomina ***iterador***.</span><span class="sxs-lookup"><span data-stu-id="84145-1548">A function member ([Function members](expressions.md#function-members)) implemented using an iterator block ([Blocks](statements.md#blocks)) is called an ***iterator***.</span></span>

<span data-ttu-id="84145-1549">Un bloque de iterador se puede usar como cuerpo de un miembro de función siempre que el tipo de valor devuelto del miembro de función correspondiente sea una de las interfaces de enumerador ([interfaces de enumerador](classes.md#enumerator-interfaces)) o una de las interfaces enumerables ([interfaces enumerables](classes.md#enumerable-interfaces)) .</span><span class="sxs-lookup"><span data-stu-id="84145-1549">An iterator block may be used as the body of a function member as long as the return type of the corresponding function member is one of the enumerator interfaces ([Enumerator interfaces](classes.md#enumerator-interfaces)) or one of the enumerable interfaces ([Enumerable interfaces](classes.md#enumerable-interfaces)).</span></span> <span data-ttu-id="84145-1550">Puede producirse como *method_body*, *operator_body* o *accessor_body*, mientras que los eventos, constructores de instancias, constructores estáticos y destructores no se pueden implementar como iteradores.</span><span class="sxs-lookup"><span data-stu-id="84145-1550">It can occur as a *method_body*, *operator_body* or *accessor_body*, whereas events, instance constructors, static constructors and destructors cannot be implemented as iterators.</span></span>

<span data-ttu-id="84145-1551">Cuando un miembro de función se implementa mediante un bloque de iteradores, se trata de un error en tiempo de compilación para que la lista de parámetros formales `out` del miembro de función especifique cualquier `ref` parámetro o.</span><span class="sxs-lookup"><span data-stu-id="84145-1551">When a function member is implemented using an iterator block, it is a compile-time error for the formal parameter list of the function member to specify any `ref` or `out` parameters.</span></span>

### <a name="enumerator-interfaces"></a><span data-ttu-id="84145-1552">Interfaces de enumerador</span><span class="sxs-lookup"><span data-stu-id="84145-1552">Enumerator interfaces</span></span>

<span data-ttu-id="84145-1553">Las ***interfaces de enumerador*** son la interfaz `System.Collections.IEnumerator` no genérica y todas las creaciones de instancias de `System.Collections.Generic.IEnumerator<T>`la interfaz genérica.</span><span class="sxs-lookup"><span data-stu-id="84145-1553">The ***enumerator interfaces*** are the non-generic interface `System.Collections.IEnumerator` and all instantiations of the generic interface `System.Collections.Generic.IEnumerator<T>`.</span></span> <span data-ttu-id="84145-1554">Por motivos de brevedad, en este capítulo se hace referencia a estas interfaces como `IEnumerator` y `IEnumerator<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="84145-1554">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerator` and `IEnumerator<T>`, respectively.</span></span>

### <a name="enumerable-interfaces"></a><span data-ttu-id="84145-1555">Interfaces enumerables</span><span class="sxs-lookup"><span data-stu-id="84145-1555">Enumerable interfaces</span></span>

<span data-ttu-id="84145-1556">Las ***interfaces enumerables*** son la interfaz `System.Collections.IEnumerable` no genérica y todas las creaciones de instancias de la `System.Collections.Generic.IEnumerable<T>`interfaz genérica.</span><span class="sxs-lookup"><span data-stu-id="84145-1556">The ***enumerable interfaces*** are the non-generic interface `System.Collections.IEnumerable` and all instantiations of the generic interface `System.Collections.Generic.IEnumerable<T>`.</span></span> <span data-ttu-id="84145-1557">Por motivos de brevedad, en este capítulo se hace referencia a estas interfaces como `IEnumerable` y `IEnumerable<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="84145-1557">For the sake of brevity, in this chapter these interfaces are referenced as `IEnumerable` and `IEnumerable<T>`, respectively.</span></span>

### <a name="yield-type"></a><span data-ttu-id="84145-1558">Tipo yield</span><span class="sxs-lookup"><span data-stu-id="84145-1558">Yield type</span></span>

<span data-ttu-id="84145-1559">Un iterador genera una secuencia de valores, todo el mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="84145-1559">An iterator produces a sequence of values, all of the same type.</span></span> <span data-ttu-id="84145-1560">Este tipo se denomina ***tipo yield*** del iterador.</span><span class="sxs-lookup"><span data-stu-id="84145-1560">This type is called the ***yield type*** of the iterator.</span></span>

*  <span data-ttu-id="84145-1561">El tipo yield de un iterador que `IEnumerator` devuelve `IEnumerable` o `object`es.</span><span class="sxs-lookup"><span data-stu-id="84145-1561">The yield type of an iterator that returns `IEnumerator` or `IEnumerable` is `object`.</span></span>
*  <span data-ttu-id="84145-1562">El tipo yield de un iterador que `IEnumerator<T>` devuelve `IEnumerable<T>` o `T`es.</span><span class="sxs-lookup"><span data-stu-id="84145-1562">The yield type of an iterator that returns `IEnumerator<T>` or `IEnumerable<T>` is `T`.</span></span>

### <a name="enumerator-objects"></a><span data-ttu-id="84145-1563">Objetos Enumerator</span><span class="sxs-lookup"><span data-stu-id="84145-1563">Enumerator objects</span></span>

<span data-ttu-id="84145-1564">Cuando un miembro de función que devuelve un tipo de interfaz de enumerador se implementa mediante un bloque de iteradores, al invocar al miembro de función no se ejecuta inmediatamente el código en el bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="84145-1564">When a function member returning an enumerator interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="84145-1565">En su lugar, se crea y se devuelve un ***objeto de enumerador*** .</span><span class="sxs-lookup"><span data-stu-id="84145-1565">Instead, an ***enumerator object*** is created and returned.</span></span> <span data-ttu-id="84145-1566">Este objeto encapsula el código especificado en el bloque de iteradores y la ejecución del código en el bloque de iterador se produce cuando se `MoveNext` invoca el método del objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="84145-1566">This object encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="84145-1567">Un objeto de enumerador tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="84145-1567">An enumerator object has the following characteristics:</span></span>

*  <span data-ttu-id="84145-1568">Implementa `IEnumerator` y `IEnumerator<T>`, donde`T` es el tipo yield del iterador.</span><span class="sxs-lookup"><span data-stu-id="84145-1568">It implements `IEnumerator` and `IEnumerator<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="84145-1569">Implementa `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="84145-1569">It implements `System.IDisposable`.</span></span>
*  <span data-ttu-id="84145-1570">Se inicializa con una copia de los valores de argumento (si existen) y el valor de instancia pasado al miembro de función.</span><span class="sxs-lookup"><span data-stu-id="84145-1570">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>
*  <span data-ttu-id="84145-1571">Tiene cuatro Estados posibles, ***antes***, en ***ejecución***, ***suspendido***y ***después***, y está inicialmente en el estado ***antes*** .</span><span class="sxs-lookup"><span data-stu-id="84145-1571">It has four potential states, ***before***, ***running***, ***suspended***, and ***after***, and is initially in the ***before*** state.</span></span>

<span data-ttu-id="84145-1572">Un objeto de enumerador suele ser una instancia de una clase de enumerador generada por el compilador que encapsula el código en el bloque de iteradores e implementa las interfaces del enumerador, pero otros métodos de implementación son posibles.</span><span class="sxs-lookup"><span data-stu-id="84145-1572">An enumerator object is typically an instance of a compiler-generated enumerator class that encapsulates the code in the iterator block and implements the enumerator interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="84145-1573">Si el compilador genera una clase de enumerador, esa clase se anidará, directa o indirectamente, en la clase que contiene el miembro de función, tendrá accesibilidad privada y tendrá un nombre reservado para uso del compilador ([identificadores](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="84145-1573">If an enumerator class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="84145-1574">Un objeto de enumerador puede implementar más interfaces de las especificadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="84145-1574">An enumerator object may implement more interfaces than those specified above.</span></span>

<span data-ttu-id="84145-1575">En las secciones siguientes se describe el comportamiento exacto `MoveNext`de `Current`los miembros `Dispose` , y de `IEnumerable` las `IEnumerable<T>` implementaciones de la interfaz y que proporciona un objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="84145-1575">The following sections describe the exact behavior of the `MoveNext`, `Current`, and `Dispose` members of the `IEnumerable` and `IEnumerable<T>` interface implementations provided by an enumerator object.</span></span>

<span data-ttu-id="84145-1576">Tenga en cuenta que los objetos de enumerador no admiten el `IEnumerator.Reset` método.</span><span class="sxs-lookup"><span data-stu-id="84145-1576">Note that enumerator objects do not support the `IEnumerator.Reset` method.</span></span> <span data-ttu-id="84145-1577">Al invocar este método, `System.NotSupportedException` se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="84145-1577">Invoking this method causes a `System.NotSupportedException` to be thrown.</span></span>

#### <a name="the-movenext-method"></a><span data-ttu-id="84145-1578">El método MoveNext</span><span class="sxs-lookup"><span data-stu-id="84145-1578">The MoveNext method</span></span>

<span data-ttu-id="84145-1579">El `MoveNext` método de un objeto de enumerador encapsula el código de un bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="84145-1579">The `MoveNext` method of an enumerator object encapsulates the code of an iterator block.</span></span> <span data-ttu-id="84145-1580">Al invocar `MoveNext` el método, se ejecuta código en el bloque de iterador y se establece la `Current` propiedad del objeto de enumerador según corresponda.</span><span class="sxs-lookup"><span data-stu-id="84145-1580">Invoking the `MoveNext` method executes code in the iterator block and sets the `Current` property of the enumerator object as appropriate.</span></span> <span data-ttu-id="84145-1581">La acción precisa realizada por `MoveNext` depende del estado del objeto de enumerador cuando `MoveNext` se invoca:</span><span class="sxs-lookup"><span data-stu-id="84145-1581">The precise action performed by `MoveNext` depends on the state of the enumerator object when `MoveNext` is invoked:</span></span>

*  <span data-ttu-id="84145-1582">Si el estado del objeto de enumerador es ***anterior***, al `MoveNext`invocar:</span><span class="sxs-lookup"><span data-stu-id="84145-1582">If the state of the enumerator object is ***before***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="84145-1583">Cambia el estado a en ***ejecución***.</span><span class="sxs-lookup"><span data-stu-id="84145-1583">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="84145-1584">Inicializa los parámetros (incluido `this`) del bloque de iteradores en los valores de argumento y el valor de instancia guardados cuando se inicializó el objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="84145-1584">Initializes the parameters (including `this`) of the iterator block to the argument values and instance value saved when the enumerator object was initialized.</span></span>
   * <span data-ttu-id="84145-1585">Ejecuta el bloque de iterador desde el principio hasta que se interrumpe la ejecución (como se describe a continuación).</span><span class="sxs-lookup"><span data-stu-id="84145-1585">Executes the iterator block from the beginning until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="84145-1586">Si el estado del objeto de enumerador se está ***ejecutando***, no se especifica `MoveNext` el resultado de la invocación.</span><span class="sxs-lookup"><span data-stu-id="84145-1586">If the state of the enumerator object is ***running***, the result of invoking `MoveNext` is unspecified.</span></span>
*  <span data-ttu-id="84145-1587">Si el estado del objeto de enumerador es ***suspendido***, al `MoveNext`invocar:</span><span class="sxs-lookup"><span data-stu-id="84145-1587">If the state of the enumerator object is ***suspended***, invoking `MoveNext`:</span></span>
   * <span data-ttu-id="84145-1588">Cambia el estado a en ***ejecución***.</span><span class="sxs-lookup"><span data-stu-id="84145-1588">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="84145-1589">Restaura los valores de todas las variables locales y los parámetros (incluido este) en los valores guardados cuando la ejecución del bloque de iterador se suspendió por última vez.</span><span class="sxs-lookup"><span data-stu-id="84145-1589">Restores the values of all local variables and parameters (including this) to the values saved when execution of the iterator block was last suspended.</span></span> <span data-ttu-id="84145-1590">Tenga en cuenta que el contenido de los objetos a los que hacen referencia estas variables puede haber cambiado desde la llamada anterior a MoveNext.</span><span class="sxs-lookup"><span data-stu-id="84145-1590">Note that the contents of any objects referenced by these variables may have changed since the previous call to MoveNext.</span></span>
   * <span data-ttu-id="84145-1591">Reanuda la ejecución del bloque de iterador inmediatamente después `yield return` de la instrucción que provocó la suspensión de la ejecución y continúa hasta que se interrumpe la ejecución (como se describe a continuación).</span><span class="sxs-lookup"><span data-stu-id="84145-1591">Resumes execution of the iterator block immediately following the `yield return` statement that caused the suspension of execution and continues until execution is interrupted (as described below).</span></span>
*  <span data-ttu-id="84145-1592">Si el estado del objeto de enumerador es ***después***de, `MoveNext` la `false`llamada a devuelve.</span><span class="sxs-lookup"><span data-stu-id="84145-1592">If the state of the enumerator object is ***after***, invoking `MoveNext` returns `false`.</span></span>


<span data-ttu-id="84145-1593">Cuando `MoveNext` ejecuta el bloque de iterador, la ejecución se puede interrumpir de cuatro maneras: Mediante una `yield return` instrucción, mediante una `yield break` instrucción, al encontrar el final del bloque de iterador y una excepción que se produce y se propaga fuera del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="84145-1593">When `MoveNext` executes the iterator block, execution can be interrupted in four ways: By a `yield return` statement, by a `yield break` statement, by encountering the end of the iterator block, and by an exception being thrown and propagated out of the iterator block.</span></span>

*  <span data-ttu-id="84145-1594">Cuando se `yield return` encuentra una instrucción ([la instrucción yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="84145-1594">When a `yield return` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="84145-1595">La expresión dada en la instrucción se evalúa, se convierte implícitamente al tipo Yield y se asigna a la `Current` propiedad del objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="84145-1595">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
   * <span data-ttu-id="84145-1596">Se suspende la ejecución del cuerpo del iterador.</span><span class="sxs-lookup"><span data-stu-id="84145-1596">Execution of the iterator body is suspended.</span></span> <span data-ttu-id="84145-1597">Se guardan los valores de todas las variables locales `this`y los parámetros (incluido), tal y como `yield return` se encuentra en la ubicación de esta instrucción.</span><span class="sxs-lookup"><span data-stu-id="84145-1597">The values of all local variables and parameters (including `this`) are saved, as is the location of this `yield return` statement.</span></span> <span data-ttu-id="84145-1598">Si la `yield return` instrucción está dentro de uno o `try` más bloques, los `finally` bloques asociados no se ejecutan en este momento.</span><span class="sxs-lookup"><span data-stu-id="84145-1598">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
   * <span data-ttu-id="84145-1599">El estado del objeto de enumerador se cambia a ***Suspended***.</span><span class="sxs-lookup"><span data-stu-id="84145-1599">The state of the enumerator object is changed to ***suspended***.</span></span>
   * <span data-ttu-id="84145-1600">El `MoveNext` método vuelve `true` a su llamador, lo que indica que la iteración se ha avanzado correctamente al valor siguiente.</span><span class="sxs-lookup"><span data-stu-id="84145-1600">The `MoveNext` method returns `true` to its caller, indicating that the iteration successfully advanced to the next value.</span></span>
*  <span data-ttu-id="84145-1601">Cuando se `yield break` encuentra una instrucción ([la instrucción yield](statements.md#the-yield-statement)):</span><span class="sxs-lookup"><span data-stu-id="84145-1601">When a `yield break` statement is encountered ([The yield statement](statements.md#the-yield-statement)):</span></span>
   * <span data-ttu-id="84145-1602">Si la `yield break` instrucción está dentro de uno o `try` más bloques, se `finally` ejecutan los bloques asociados.</span><span class="sxs-lookup"><span data-stu-id="84145-1602">If the `yield break` statement is within one or more `try` blocks, the associated `finally` blocks are executed.</span></span>
   * <span data-ttu-id="84145-1603">El estado del objeto de enumerador se cambia a ***después***de.</span><span class="sxs-lookup"><span data-stu-id="84145-1603">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="84145-1604">El `MoveNext` método vuelve `false` a su llamador, lo que indica que la iteración ha finalizado.</span><span class="sxs-lookup"><span data-stu-id="84145-1604">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="84145-1605">Cuando se encuentra el final del cuerpo del iterador:</span><span class="sxs-lookup"><span data-stu-id="84145-1605">When the end of the iterator body is encountered:</span></span>
   * <span data-ttu-id="84145-1606">El estado del objeto de enumerador se cambia a ***después***de.</span><span class="sxs-lookup"><span data-stu-id="84145-1606">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="84145-1607">El `MoveNext` método vuelve `false` a su llamador, lo que indica que la iteración ha finalizado.</span><span class="sxs-lookup"><span data-stu-id="84145-1607">The `MoveNext` method returns `false` to its caller, indicating that the iteration is complete.</span></span>
*  <span data-ttu-id="84145-1608">Cuando se produce una excepción y se propaga fuera del bloque de iteradores:</span><span class="sxs-lookup"><span data-stu-id="84145-1608">When an exception is thrown and propagated out of the iterator block:</span></span>
   * <span data-ttu-id="84145-1609">La `finally` propagación de excepciones habrá ejecutado los bloques adecuados en el cuerpo del iterador.</span><span class="sxs-lookup"><span data-stu-id="84145-1609">Appropriate `finally` blocks in the iterator body will have been executed by the exception propagation.</span></span>
   * <span data-ttu-id="84145-1610">El estado del objeto de enumerador se cambia a ***después***de.</span><span class="sxs-lookup"><span data-stu-id="84145-1610">The state of the enumerator object is changed to ***after***.</span></span>
   * <span data-ttu-id="84145-1611">La propagación de excepciones continúa hasta el autor de la llamada `MoveNext` del método.</span><span class="sxs-lookup"><span data-stu-id="84145-1611">The exception propagation continues to the caller of the `MoveNext` method.</span></span>

#### <a name="the-current-property"></a><span data-ttu-id="84145-1612">Propiedad actual</span><span class="sxs-lookup"><span data-stu-id="84145-1612">The Current property</span></span>

<span data-ttu-id="84145-1613">La propiedad de un `Current` objeto de enumerador `yield return` se ve afectada por las instrucciones del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="84145-1613">An enumerator object's `Current` property is affected by `yield return` statements in the iterator block.</span></span>

<span data-ttu-id="84145-1614">Cuando un objeto de enumerador se encuentra en estado ***suspendido*** , el `Current` valor de es el valor establecido por la llamada `MoveNext`anterior a.</span><span class="sxs-lookup"><span data-stu-id="84145-1614">When an enumerator object is in the ***suspended*** state, the value of `Current` is the value set by the previous call to `MoveNext`.</span></span> <span data-ttu-id="84145-1615">Cuando un objeto de enumerador está en los Estados ***Before***, ***Running***o ***After*** , `Current` no se especifica el resultado del acceso.</span><span class="sxs-lookup"><span data-stu-id="84145-1615">When an enumerator object is in the ***before***, ***running***, or ***after*** states, the result of accessing `Current` is unspecified.</span></span>

<span data-ttu-id="84145-1616">En el caso de un iterador con un `object`tipo de Yield distinto de, `Current` el resultado del acceso a `IEnumerable` través de la implementación del `Current` objeto de enumerador corresponde `IEnumerator<T>` al acceso a través del objeto del enumerador. implementación y conversión del resultado en `object`.</span><span class="sxs-lookup"><span data-stu-id="84145-1616">For an iterator with a yield type other than `object`, the result of accessing `Current` through the enumerator object's `IEnumerable` implementation corresponds to accessing `Current` through the enumerator object's `IEnumerator<T>` implementation and casting the result to `object`.</span></span>

#### <a name="the-dispose-method"></a><span data-ttu-id="84145-1617">El método Dispose</span><span class="sxs-lookup"><span data-stu-id="84145-1617">The Dispose method</span></span>

<span data-ttu-id="84145-1618">El `Dispose` método se usa para limpiar la iteración y el objeto de enumerador se coloca en el estado ***después*** .</span><span class="sxs-lookup"><span data-stu-id="84145-1618">The `Dispose` method is used to clean up the iteration by bringing the enumerator object to the ***after*** state.</span></span>

*  <span data-ttu-id="84145-1619">Si el estado del objeto de enumerador es ***anterior***, al `Dispose` invocar se cambia el estado a ***después***de.</span><span class="sxs-lookup"><span data-stu-id="84145-1619">If the state of the enumerator object is ***before***, invoking `Dispose` changes the state to ***after***.</span></span>
*  <span data-ttu-id="84145-1620">Si el estado del objeto de enumerador se está ***ejecutando***, no se especifica `Dispose` el resultado de la invocación.</span><span class="sxs-lookup"><span data-stu-id="84145-1620">If the state of the enumerator object is ***running***, the result of invoking `Dispose` is unspecified.</span></span>
*  <span data-ttu-id="84145-1621">Si el estado del objeto de enumerador es ***suspendido***, al `Dispose`invocar:</span><span class="sxs-lookup"><span data-stu-id="84145-1621">If the state of the enumerator object is ***suspended***, invoking `Dispose`:</span></span>
   * <span data-ttu-id="84145-1622">Cambia el estado a en ***ejecución***.</span><span class="sxs-lookup"><span data-stu-id="84145-1622">Changes the state to ***running***.</span></span>
   * <span data-ttu-id="84145-1623">Ejecuta cualquier bloque Finally como si la última instrucción `yield return` ejecutada fuera `yield break` una instrucción.</span><span class="sxs-lookup"><span data-stu-id="84145-1623">Executes any finally blocks as if the last executed `yield return` statement were a `yield break` statement.</span></span> <span data-ttu-id="84145-1624">Si esto hace que se produzca una excepción y se propague fuera del cuerpo del iterador, el estado del objeto de enumerador se establece en ***después*** de y la excepción se propaga al llamador `Dispose` del método.</span><span class="sxs-lookup"><span data-stu-id="84145-1624">If this causes an exception to be thrown and propagated out of the iterator body, the state of the enumerator object is set to ***after*** and the exception is propagated to the caller of the `Dispose` method.</span></span>
   * <span data-ttu-id="84145-1625">Cambia el estado a ***después***de.</span><span class="sxs-lookup"><span data-stu-id="84145-1625">Changes the state to ***after***.</span></span>
*  <span data-ttu-id="84145-1626">Si el estado del objeto de enumerador es ***After***, la `Dispose` invocación de no tiene ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="84145-1626">If the state of the enumerator object is ***after***, invoking `Dispose` has no affect.</span></span>

### <a name="enumerable-objects"></a><span data-ttu-id="84145-1627">Objetos enumerables</span><span class="sxs-lookup"><span data-stu-id="84145-1627">Enumerable objects</span></span>

<span data-ttu-id="84145-1628">Cuando un miembro de función que devuelve un tipo de interfaz enumerable se implementa mediante un bloque de iteradores, al invocar al miembro de función no se ejecuta inmediatamente el código en el bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="84145-1628">When a function member returning an enumerable interface type is implemented using an iterator block, invoking the function member does not immediately execute the code in the iterator block.</span></span> <span data-ttu-id="84145-1629">En su lugar, se crea y se devuelve un ***objeto enumerable*** .</span><span class="sxs-lookup"><span data-stu-id="84145-1629">Instead, an ***enumerable object*** is created and returned.</span></span> <span data-ttu-id="84145-1630">El método del objeto `GetEnumerator` Enumerable devuelve un objeto de enumerador que encapsula el código especificado en el bloque de iterador y la ejecución del código en el bloque de iterador se `MoveNext` produce cuando se invoca el método del objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="84145-1630">The enumerable object's `GetEnumerator` method returns an enumerator object that encapsulates the code specified in the iterator block, and execution of the code in the iterator block occurs when the enumerator object's `MoveNext` method is invoked.</span></span> <span data-ttu-id="84145-1631">Un objeto enumerable tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="84145-1631">An enumerable object has the following characteristics:</span></span>

*  <span data-ttu-id="84145-1632">Implementa `IEnumerable` y `IEnumerable<T>`, donde`T` es el tipo yield del iterador.</span><span class="sxs-lookup"><span data-stu-id="84145-1632">It implements `IEnumerable` and `IEnumerable<T>`, where `T` is the yield type of the iterator.</span></span>
*  <span data-ttu-id="84145-1633">Se inicializa con una copia de los valores de argumento (si existen) y el valor de instancia pasado al miembro de función.</span><span class="sxs-lookup"><span data-stu-id="84145-1633">It is initialized with a copy of the argument values (if any) and instance value passed to the function member.</span></span>

<span data-ttu-id="84145-1634">Un objeto enumerable es normalmente una instancia de una clase Enumerable generada por el compilador que encapsula el código en el bloque de iteradores e implementa las interfaces enumerables, pero otros métodos de implementación son posibles.</span><span class="sxs-lookup"><span data-stu-id="84145-1634">An enumerable object is typically an instance of a compiler-generated enumerable class that encapsulates the code in the iterator block and implements the enumerable interfaces, but other methods of implementation are possible.</span></span> <span data-ttu-id="84145-1635">Si el compilador genera una clase Enumerable, esa clase se anidará, directa o indirectamente, en la clase que contiene el miembro de función, tendrá accesibilidad privada y tendrá un nombre reservado para uso del compilador ([identificadores](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="84145-1635">If an enumerable class is generated by the compiler, that class will be nested, directly or indirectly, in the class containing the function member, it will have private accessibility, and it will have a name reserved for compiler use ([Identifiers](lexical-structure.md#identifiers)).</span></span>

<span data-ttu-id="84145-1636">Un objeto enumerable puede implementar más interfaces de las especificadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="84145-1636">An enumerable object may implement more interfaces than those specified above.</span></span> <span data-ttu-id="84145-1637">En concreto, un objeto enumerable también puede `IEnumerator` implementar `IEnumerator<T>`y, lo que permite que sirva como un enumerador y un enumerador.</span><span class="sxs-lookup"><span data-stu-id="84145-1637">In particular, an enumerable object may also implement `IEnumerator` and `IEnumerator<T>`, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="84145-1638">En ese tipo de implementación, la primera vez que se invoca el `GetEnumerator` método de un objeto enumerable, se devuelve el propio objeto Enumerable.</span><span class="sxs-lookup"><span data-stu-id="84145-1638">In that type of implementation, the first time an enumerable object's `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="84145-1639">Las siguientes invocaciones de la del `GetEnumerator`objeto enumerable, si las hay, devuelven una copia del objeto Enumerable.</span><span class="sxs-lookup"><span data-stu-id="84145-1639">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="84145-1640">Por lo tanto, cada enumerador devuelto tiene su propio estado y los cambios en un enumerador no afectarán a otro.</span><span class="sxs-lookup"><span data-stu-id="84145-1640">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span>

#### <a name="the-getenumerator-method"></a><span data-ttu-id="84145-1641">El método GetEnumerator</span><span class="sxs-lookup"><span data-stu-id="84145-1641">The GetEnumerator method</span></span>

<span data-ttu-id="84145-1642">Un objeto enumerable proporciona una implementación de `GetEnumerator` los métodos de `IEnumerable` las `IEnumerable<T>` interfaces y.</span><span class="sxs-lookup"><span data-stu-id="84145-1642">An enumerable object provides an implementation of the `GetEnumerator` methods of the `IEnumerable` and `IEnumerable<T>` interfaces.</span></span> <span data-ttu-id="84145-1643">Los dos `GetEnumerator` métodos comparten una implementación común que adquiere y devuelve un objeto de enumerador disponible.</span><span class="sxs-lookup"><span data-stu-id="84145-1643">The two `GetEnumerator` methods share a common implementation that acquires and returns an available enumerator object.</span></span> <span data-ttu-id="84145-1644">El objeto de enumerador se inicializa con los valores de argumento y el valor de instancia guardados cuando se inicializó el objeto enumerable, pero de lo contrario el objeto de enumerador funciona como se describe en [objetos de enumerador](classes.md#enumerator-objects).</span><span class="sxs-lookup"><span data-stu-id="84145-1644">The enumerator object is initialized with the argument values and instance value saved when the enumerable object was initialized, but otherwise the enumerator object functions as described in [Enumerator objects](classes.md#enumerator-objects).</span></span>

### <a name="implementation-example"></a><span data-ttu-id="84145-1645">Ejemplo de implementación</span><span class="sxs-lookup"><span data-stu-id="84145-1645">Implementation example</span></span>

<span data-ttu-id="84145-1646">En esta sección se describe una posible implementación de iteradores en términos C# de construcciones estándar.</span><span class="sxs-lookup"><span data-stu-id="84145-1646">This section describes a possible implementation of iterators in terms of standard C# constructs.</span></span> <span data-ttu-id="84145-1647">La implementación que se describe aquí se basa en los mismos principios utilizados por C# el compilador de Microsoft, pero no es una implementación asignada o la única que sea posible.</span><span class="sxs-lookup"><span data-stu-id="84145-1647">The implementation described here is based on the same principles used by the Microsoft C# compiler, but it is by no means a mandated implementation or the only one possible.</span></span>

<span data-ttu-id="84145-1648">La clase `Stack<T>` siguiente implementa su `GetEnumerator` método mediante un iterador.</span><span class="sxs-lookup"><span data-stu-id="84145-1648">The following `Stack<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="84145-1649">El iterador enumera los elementos de la pila en orden descendente.</span><span class="sxs-lookup"><span data-stu-id="84145-1649">The iterator enumerates the elements of the stack in top to bottom order.</span></span>

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

<span data-ttu-id="84145-1650">El `GetEnumerator` método se puede traducir en una instancia de una clase de enumerador generada por el compilador que encapsula el código en el bloque de iteradores, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="84145-1650">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="84145-1651">En la traducción anterior, el código del bloque de iterador se convierte en una máquina de Estados y se `MoveNext` coloca en el método de la clase de enumerador.</span><span class="sxs-lookup"><span data-stu-id="84145-1651">In the preceding translation, the code in the iterator block is turned into a state machine and placed in the `MoveNext` method of the enumerator class.</span></span> <span data-ttu-id="84145-1652">Además, la variable `i` local se convierte en un campo en el objeto de enumerador para que pueda seguir existiendo en las invocaciones de. `MoveNext`</span><span class="sxs-lookup"><span data-stu-id="84145-1652">Furthermore, the local variable `i` is turned into a field in the enumerator object so it can continue to exist across invocations of `MoveNext`.</span></span>

<span data-ttu-id="84145-1653">En el ejemplo siguiente se imprime una tabla de multiplicación simple de los enteros comprendidos entre 1 y 10.</span><span class="sxs-lookup"><span data-stu-id="84145-1653">The following example prints a simple multiplication table of the integers 1 through 10.</span></span> <span data-ttu-id="84145-1654">El `FromTo` método en el ejemplo devuelve un objeto enumerable y se implementa mediante un iterador.</span><span class="sxs-lookup"><span data-stu-id="84145-1654">The `FromTo` method in the example returns an enumerable object and is implemented using an iterator.</span></span>

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

<span data-ttu-id="84145-1655">El `FromTo` método se puede traducir en una instancia de una clase Enumerable generada por el compilador que encapsula el código en el bloque de iteradores, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="84145-1655">The `FromTo` method can be translated into an instantiation of a compiler-generated enumerable class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="84145-1656">La clase Enumerable implementa las interfaces enumerables y las interfaces de enumerador, lo que permite que sirva como un enumerador y un enumerador.</span><span class="sxs-lookup"><span data-stu-id="84145-1656">The enumerable class implements both the enumerable interfaces and the enumerator interfaces, enabling it to serve as both an enumerable and an enumerator.</span></span> <span data-ttu-id="84145-1657">La primera vez que `GetEnumerator` se invoca el método, se devuelve el propio objeto Enumerable.</span><span class="sxs-lookup"><span data-stu-id="84145-1657">The first time the `GetEnumerator` method is invoked, the enumerable object itself is returned.</span></span> <span data-ttu-id="84145-1658">Las siguientes invocaciones de la del `GetEnumerator`objeto enumerable, si las hay, devuelven una copia del objeto Enumerable.</span><span class="sxs-lookup"><span data-stu-id="84145-1658">Subsequent invocations of the enumerable object's `GetEnumerator`, if any, return a copy of the enumerable object.</span></span> <span data-ttu-id="84145-1659">Por lo tanto, cada enumerador devuelto tiene su propio estado y los cambios en un enumerador no afectarán a otro.</span><span class="sxs-lookup"><span data-stu-id="84145-1659">Thus, each returned enumerator has its own state and changes in one enumerator will not affect another.</span></span> <span data-ttu-id="84145-1660">El `Interlocked.CompareExchange` método se usa para garantizar la operación segura para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="84145-1660">The `Interlocked.CompareExchange` method is used to ensure thread-safe operation.</span></span>

<span data-ttu-id="84145-1661">Los `from` parámetros `to` y se convierten en campos en la clase Enumerable.</span><span class="sxs-lookup"><span data-stu-id="84145-1661">The `from` and `to` parameters are turned into fields in the enumerable class.</span></span> <span data-ttu-id="84145-1662">Dado `from` que se modifica en el bloque de iterador `__from` , se introduce un campo adicional para contener el valor `from` inicial dado a en cada enumerador.</span><span class="sxs-lookup"><span data-stu-id="84145-1662">Because `from` is modified in the iterator block, an additional `__from` field is introduced to hold the initial value given to `from` in each enumerator.</span></span>

<span data-ttu-id="84145-1663">El `MoveNext` método produce una `InvalidOperationException` excepción si se llama cuando `__state` es `0`.</span><span class="sxs-lookup"><span data-stu-id="84145-1663">The `MoveNext` method throws an `InvalidOperationException` if it is called when `__state` is `0`.</span></span> <span data-ttu-id="84145-1664">Esto protege contra el uso del objeto enumerable como un objeto de enumerador `GetEnumerator`sin llamar primero a.</span><span class="sxs-lookup"><span data-stu-id="84145-1664">This protects against use of the enumerable object as an enumerator object without first calling `GetEnumerator`.</span></span>

<span data-ttu-id="84145-1665">En el ejemplo siguiente se muestra una clase de árbol simple.</span><span class="sxs-lookup"><span data-stu-id="84145-1665">The following example shows a simple tree class.</span></span> <span data-ttu-id="84145-1666">La `Tree<T>` clase implementa su `GetEnumerator` método mediante un iterador.</span><span class="sxs-lookup"><span data-stu-id="84145-1666">The `Tree<T>` class implements its `GetEnumerator` method using an iterator.</span></span> <span data-ttu-id="84145-1667">El iterador enumera los elementos del árbol en orden infijo.</span><span class="sxs-lookup"><span data-stu-id="84145-1667">The iterator enumerates the elements of the tree in infix order.</span></span>

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

<span data-ttu-id="84145-1668">El `GetEnumerator` método se puede traducir en una instancia de una clase de enumerador generada por el compilador que encapsula el código en el bloque de iteradores, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="84145-1668">The `GetEnumerator` method can be translated into an instantiation of a compiler-generated enumerator class that encapsulates the code in the iterator block, as shown in the following.</span></span>

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

<span data-ttu-id="84145-1669">El compilador generado objetos temporales utilizado `foreach` en las instrucciones se eleva en `__left` los `__right` campos y del objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="84145-1669">The compiler generated temporaries used in the `foreach` statements are lifted into the `__left` and `__right` fields of the enumerator object.</span></span> <span data-ttu-id="84145-1670">El `__state` campo del objeto de enumerador se actualiza cuidadosamente para que se `Dispose()` llame correctamente al método correcto si se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="84145-1670">The `__state` field of the enumerator object is carefully updated so that the correct `Dispose()` method will be called correctly if an exception is thrown.</span></span> <span data-ttu-id="84145-1671">Tenga en cuenta que no es posible escribir el código traducido con instrucciones `foreach` sencillas.</span><span class="sxs-lookup"><span data-stu-id="84145-1671">Note that it is not possible to write the translated code with simple `foreach` statements.</span></span>

## <a name="async-functions"></a><span data-ttu-id="84145-1672">Funciones asincrónicas</span><span class="sxs-lookup"><span data-stu-id="84145-1672">Async functions</span></span>

<span data-ttu-id="84145-1673">Un método ([métodos](classes.md#methods)) o una función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) `async` con el modificador se denomina ***función asincrónica***.</span><span class="sxs-lookup"><span data-stu-id="84145-1673">A method ([Methods](classes.md#methods)) or anonymous function ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) with the `async` modifier is called an ***async function***.</span></span> <span data-ttu-id="84145-1674">En general, el término ***Async*** se usa para describir cualquier tipo de función que tenga el `async` modificador.</span><span class="sxs-lookup"><span data-stu-id="84145-1674">In general, the term ***async*** is used to describe any kind of function that has the `async` modifier.</span></span>

<span data-ttu-id="84145-1675">Es un error en tiempo de compilación para que la lista de parámetros formales de una función asincrónica `ref` especifique `out` cualquier parámetro o.</span><span class="sxs-lookup"><span data-stu-id="84145-1675">It is a compile-time error for the formal parameter list of an async function to specify any `ref` or `out` parameters.</span></span>

<span data-ttu-id="84145-1676">El tipo de un método asincrónico debe ser `void` o un ***tipo de tarea***.</span><span class="sxs-lookup"><span data-stu-id="84145-1676">The *return_type* of an async method must be either `void` or a ***task type***.</span></span> <span data-ttu-id="84145-1677">Los tipos de tarea `System.Threading.Tasks.Task` son y los tipos `System.Threading.Tasks.Task<T>`construidos a partir de.</span><span class="sxs-lookup"><span data-stu-id="84145-1677">The task types are `System.Threading.Tasks.Task` and types constructed from `System.Threading.Tasks.Task<T>`.</span></span> <span data-ttu-id="84145-1678">Por motivos de brevedad, en este capítulo se hace referencia a estos tipos como `Task` y `Task<T>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="84145-1678">For the sake of brevity, in this chapter these types are referenced as `Task` and `Task<T>`, respectively.</span></span> <span data-ttu-id="84145-1679">Se dice que un método asincrónico que devuelve un tipo de tarea es el que devuelve la tarea.</span><span class="sxs-lookup"><span data-stu-id="84145-1679">An async method returning a task type is said to be task-returning.</span></span>

<span data-ttu-id="84145-1680">La definición exacta de los tipos de tarea es la implementación definida, pero desde el punto de vista del idioma, un tipo de tarea se encuentra en uno de los Estados incompleto, correcto o erróneo.</span><span class="sxs-lookup"><span data-stu-id="84145-1680">The exact definition of the task types is implementation defined, but from the language's point of view a task type is in one of the states incomplete, succeeded or faulted.</span></span> <span data-ttu-id="84145-1681">Una tarea con errores registra una excepción pertinente.</span><span class="sxs-lookup"><span data-stu-id="84145-1681">A faulted task records a pertinent exception.</span></span> <span data-ttu-id="84145-1682">Una correcta `Task<T>` registra un resultado de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="84145-1682">A succeeded `Task<T>` records a result of type `T`.</span></span> <span data-ttu-id="84145-1683">Los tipos de tarea son de espera y, por tanto, pueden ser los operandos de las expresiones Await ([expresiones Await](expressions.md#await-expressions)).</span><span class="sxs-lookup"><span data-stu-id="84145-1683">Task types are awaitable, and can therefore be the operands of await expressions ([Await expressions](expressions.md#await-expressions)).</span></span>

<span data-ttu-id="84145-1684">Una invocación de función asincrónica tiene la capacidad de suspender la evaluación por medio de expresiones Await ([expresiones Await](expressions.md#await-expressions)) en su cuerpo.</span><span class="sxs-lookup"><span data-stu-id="84145-1684">An async function invocation has the ability to suspend evaluation by means of await expressions ([Await expressions](expressions.md#await-expressions)) in its body.</span></span> <span data-ttu-id="84145-1685">La evaluación se puede reanudar posteriormente en el punto de la expresión Await de suspensión por medio de un ***delegado de reanudación***.</span><span class="sxs-lookup"><span data-stu-id="84145-1685">Evaluation may later be resumed at the point of the suspending await expression by means of a ***resumption delegate***.</span></span> <span data-ttu-id="84145-1686">El delegado de reanudación es de `System.Action`tipo y, cuando se invoca, la evaluación de la invocación de la función asincrónica se reanudará desde la expresión Await en la que se quedó.</span><span class="sxs-lookup"><span data-stu-id="84145-1686">The resumption delegate is of type `System.Action`, and when it is invoked, evaluation of the async function invocation will resume from the await expression where it left off.</span></span> <span data-ttu-id="84145-1687">El ***llamador actual*** de una invocación de función asincrónica es el autor de la llamada original si la invocación de la función nunca se ha suspendido o el llamador más reciente del delegado de reanudación en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="84145-1687">The ***current caller*** of an async function invocation is the original caller if the function invocation has never been suspended, or the most recent caller of the resumption delegate otherwise.</span></span>

### <a name="evaluation-of-a-task-returning-async-function"></a><span data-ttu-id="84145-1688">Evaluación de una función asincrónica que devuelve una tarea</span><span class="sxs-lookup"><span data-stu-id="84145-1688">Evaluation of a task-returning async function</span></span>

<span data-ttu-id="84145-1689">La invocación de una función asincrónica que devuelve una tarea hace que se genere una instancia del tipo de tarea devuelto.</span><span class="sxs-lookup"><span data-stu-id="84145-1689">Invocation of a task-returning async function causes an instance of the returned task type to be generated.</span></span> <span data-ttu-id="84145-1690">Esto se denomina la ***tarea devuelta*** de la función Async.</span><span class="sxs-lookup"><span data-stu-id="84145-1690">This is called the ***return task*** of the async function.</span></span> <span data-ttu-id="84145-1691">La tarea está inicialmente en un estado incompleto.</span><span class="sxs-lookup"><span data-stu-id="84145-1691">The task is initially in an incomplete state.</span></span>

<span data-ttu-id="84145-1692">A continuación, se evalúa el cuerpo de la función asincrónica hasta que se suspenda (al llegar a una expresión Await) o finalice, donde el control de punto se devuelve al autor de la llamada, junto con la tarea devuelta.</span><span class="sxs-lookup"><span data-stu-id="84145-1692">The async function body is then evaluated until it is either suspended (by reaching an await expression) or terminates, at which point control is returned to the caller, along with the return task.</span></span>

<span data-ttu-id="84145-1693">Cuando finaliza el cuerpo de la función asincrónica, la tarea devuelta se saca del estado incompleto:</span><span class="sxs-lookup"><span data-stu-id="84145-1693">When the body of the async function terminates, the return task is moved out of the incomplete state:</span></span>

*  <span data-ttu-id="84145-1694">Si el cuerpo de la función finaliza como resultado de alcanzar una instrucción return o el final del cuerpo, se registra cualquier valor de resultado en la tarea devuelta, que se pone en un estado correcto.</span><span class="sxs-lookup"><span data-stu-id="84145-1694">If the function body terminates as the result of reaching a return statement or the end of the body, any result value is recorded in the return task, which is put into a succeeded state.</span></span>
*  <span data-ttu-id="84145-1695">Si el cuerpo de la función finaliza como resultado de una excepción no detectada ([la instrucción throw](statements.md#the-throw-statement)), la excepción se registra en la tarea devuelta que se coloca en un estado de error.</span><span class="sxs-lookup"><span data-stu-id="84145-1695">If the function body terminates as the result of an uncaught exception ([The throw statement](statements.md#the-throw-statement)) the exception is recorded in the return task which is put into a faulted state.</span></span>

### <a name="evaluation-of-a-void-returning-async-function"></a><span data-ttu-id="84145-1696">Evaluación de una función asincrónica que devuelve void</span><span class="sxs-lookup"><span data-stu-id="84145-1696">Evaluation of a void-returning async function</span></span>

<span data-ttu-id="84145-1697">Si el tipo de valor devuelto de la `void`función Async es, la evaluación difiere de la anterior de la siguiente manera: Dado que no se devuelve ninguna tarea, la función comunica en su lugar la finalización y las excepciones al ***contexto de sincronización***del subproceso actual.</span><span class="sxs-lookup"><span data-stu-id="84145-1697">If the return type of the async function is `void`, evaluation differs from the above in the following way: Because no task is returned, the function instead communicates completion and exceptions to the current thread's ***synchronization context***.</span></span> <span data-ttu-id="84145-1698">La definición exacta del contexto de sincronización depende de la implementación, pero es una representación de "Dónde" se está ejecutando el subproceso actual.</span><span class="sxs-lookup"><span data-stu-id="84145-1698">The exact definition of synchronization context is implementation-dependent, but is a representation of "where" the current thread is running.</span></span> <span data-ttu-id="84145-1699">El contexto de sincronización se notifica cuando se inicia la evaluación de una función asincrónica que devuelve void, se completa correctamente o provoca que se produzca una excepción no detectada.</span><span class="sxs-lookup"><span data-stu-id="84145-1699">The synchronization context is notified when evaluation of a void-returning async function commences, completes successfully, or causes an uncaught exception to be thrown.</span></span>

<span data-ttu-id="84145-1700">Esto permite que el contexto realice un seguimiento del número de funciones asincrónicas que devuelven void que se están ejecutando en él y de decidir cómo propagar las excepciones que salen de ellas.</span><span class="sxs-lookup"><span data-stu-id="84145-1700">This allows the context to keep track of how many void-returning async functions are running under it, and to decide how to propagate exceptions coming out of them.</span></span>
