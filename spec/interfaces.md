---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229740"
---
# <a name="interfaces"></a><span data-ttu-id="80ea2-101">Interfaces</span><span class="sxs-lookup"><span data-stu-id="80ea2-101">Interfaces</span></span>

<span data-ttu-id="80ea2-102">Una interfaz define un contrato.</span><span class="sxs-lookup"><span data-stu-id="80ea2-102">An interface defines a contract.</span></span> <span data-ttu-id="80ea2-103">Una clase o estructura que implementa una interfaz debe adherirse a su contrato.</span><span class="sxs-lookup"><span data-stu-id="80ea2-103">A class or struct that implements an interface must adhere to its contract.</span></span> <span data-ttu-id="80ea2-104">Una interfaz puede heredar de varias interfaces base, y una clase o struct puede implementar varias interfaces.</span><span class="sxs-lookup"><span data-stu-id="80ea2-104">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="80ea2-105">Las interfaces pueden contener métodos, propiedades, eventos e indizadores.</span><span class="sxs-lookup"><span data-stu-id="80ea2-105">Interfaces can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="80ea2-106">La propia interfaz no proporciona implementaciones para los miembros que define.</span><span class="sxs-lookup"><span data-stu-id="80ea2-106">The interface itself does not provide implementations for the members that it defines.</span></span> <span data-ttu-id="80ea2-107">La interfaz sólo especifica a los miembros que se deben proporcionar mediante clases o structs que implementan la interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-107">The interface merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

## <a name="interface-declarations"></a><span data-ttu-id="80ea2-108">Declaraciones de interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-108">Interface declarations</span></span>

<span data-ttu-id="80ea2-109">Un *interface_declaration* es un *type_declaration* ([declaraciones de tipo](namespaces.md#type-declarations)) que declara un nuevo tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-109">An *interface_declaration* is a *type_declaration* ([Type declarations](namespaces.md#type-declarations)) that declares a new interface type.</span></span>

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

<span data-ttu-id="80ea2-110">Un *interface_declaration* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *interface_modifier*s ([modificadores de interfaz](interfaces.md#interface-modifiers)), seguido de un elemento opcional `partial` modificador, seguido por la palabra clave `interface` y un *identificador* que los nombres de la interfaz, seguido de un elemento opcional *variant_type_parameter_list* especificación ([listas de parámetros de tipo variante](interfaces.md#variant-type-parameter-lists)), seguido de un elemento opcional *interface_base* especificación ([interfaces Base](interfaces.md#base-interfaces)), seguido de un elemento opcional *type_parameter_constraints_clause*especificación s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) , seguido por un *interface_body* ([cuerpo de la interfaz](interfaces.md#interface-body)), seguido opcionalmente de un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="80ea2-110">An *interface_declaration* consists of an optional set of *attributes* ([Attributes](attributes.md)), followed by an optional set of *interface_modifier*s ([Interface modifiers](interfaces.md#interface-modifiers)), followed by an optional `partial` modifier, followed by the keyword `interface` and an *identifier* that names the interface, followed by an optional *variant_type_parameter_list* specification ([Variant type parameter lists](interfaces.md#variant-type-parameter-lists)), followed by an optional *interface_base* specification ([Base interfaces](interfaces.md#base-interfaces)), followed by an optional *type_parameter_constraints_clause*s specification ([Type parameter constraints](classes.md#type-parameter-constraints)), followed by an *interface_body* ([Interface body](interfaces.md#interface-body)), optionally followed by a semicolon.</span></span>

### <a name="interface-modifiers"></a><span data-ttu-id="80ea2-111">Modificadores de interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-111">Interface modifiers</span></span>

<span data-ttu-id="80ea2-112">Un *interface_declaration* puede incluir opcionalmente una secuencia de modificadores de interfaz:</span><span class="sxs-lookup"><span data-stu-id="80ea2-112">An *interface_declaration* may optionally include a sequence of interface modifiers:</span></span>

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

<span data-ttu-id="80ea2-113">Es un error en tiempo de compilación para el mismo modificador aparezca varias veces en una declaración de interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-113">It is a compile-time error for the same modifier to appear multiple times in an interface declaration.</span></span>

<span data-ttu-id="80ea2-114">El `new` modificador sólo se permite en interfaces definidas dentro de una clase.</span><span class="sxs-lookup"><span data-stu-id="80ea2-114">The `new` modifier is only permitted on interfaces defined within a class.</span></span> <span data-ttu-id="80ea2-115">Especifica que la interfaz oculta un miembro heredado con el mismo nombre, como se describe en [el nuevo modificador](classes.md#the-new-modifier).</span><span class="sxs-lookup"><span data-stu-id="80ea2-115">It specifies that the interface hides an inherited member by the same name, as described in [The new modifier](classes.md#the-new-modifier).</span></span>

<span data-ttu-id="80ea2-116">El `public`, `protected`, `internal`, y `private` modificadores controlan el acceso de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-116">The `public`, `protected`, `internal`, and `private` modifiers control the accessibility of the interface.</span></span> <span data-ttu-id="80ea2-117">Dependiendo del contexto en el que se produce la declaración de interfaz, pueden permitir solo algunos de estos modificadores ([accesibilidad declarada](basic-concepts.md#declared-accessibility)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-117">Depending on the context in which the interface declaration occurs, only some of these modifiers may be permitted ([Declared accessibility](basic-concepts.md#declared-accessibility)).</span></span>

### <a name="partial-modifier"></a><span data-ttu-id="80ea2-118">Modificador parcial</span><span class="sxs-lookup"><span data-stu-id="80ea2-118">Partial modifier</span></span>

<span data-ttu-id="80ea2-119">El `partial` modificador indica que esto *interface_declaration* es una declaración de tipo parcial.</span><span class="sxs-lookup"><span data-stu-id="80ea2-119">The `partial` modifier indicates that this *interface_declaration* is a partial type declaration.</span></span> <span data-ttu-id="80ea2-120">Combinan varias declaraciones parciales de interfaz con el mismo nombre dentro de una declaración de espacio de nombres o tipo envolvente a la declaración de una interfaz de formulario, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).</span><span class="sxs-lookup"><span data-stu-id="80ea2-120">Multiple partial interface declarations with the same name within an enclosing namespace or type declaration combine to form one interface declaration, following the rules specified in [Partial types](classes.md#partial-types).</span></span>

### <a name="variant-type-parameter-lists"></a><span data-ttu-id="80ea2-121">Listas de parámetros de tipo variante</span><span class="sxs-lookup"><span data-stu-id="80ea2-121">Variant type parameter lists</span></span>

<span data-ttu-id="80ea2-122">Listas de parámetros de tipo de variante solo pueden realizarse con tipos de interfaz y delegado.</span><span class="sxs-lookup"><span data-stu-id="80ea2-122">Variant type parameter lists can only occur on interface and delegate types.</span></span> <span data-ttu-id="80ea2-123">La diferencia de normal *type_parameter_list*s es opcional *variance_annotation* en cada parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="80ea2-123">The difference from ordinary *type_parameter_list*s is the optional *variance_annotation* on each type parameter.</span></span>

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

<span data-ttu-id="80ea2-124">Si la anotación de varianza es `out`, se dice que el parámetro de tipo se ***covariante***.</span><span class="sxs-lookup"><span data-stu-id="80ea2-124">If the variance annotation is `out`, the type parameter is said to be ***covariant***.</span></span> <span data-ttu-id="80ea2-125">Si la anotación de varianza es `in`, se dice que el parámetro de tipo se ***contravariante***.</span><span class="sxs-lookup"><span data-stu-id="80ea2-125">If the variance annotation is `in`, the type parameter is said to be ***contravariant***.</span></span> <span data-ttu-id="80ea2-126">Si no hay ninguna anotación de varianza, se dice que el parámetro de tipo sea ***invariable***.</span><span class="sxs-lookup"><span data-stu-id="80ea2-126">If there is no variance annotation, the type parameter is said to be ***invariant***.</span></span>

<span data-ttu-id="80ea2-127">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-127">In the example</span></span>
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
<span data-ttu-id="80ea2-128">`X` es covariante, `Y` es contravariante y `Z` es invariable.</span><span class="sxs-lookup"><span data-stu-id="80ea2-128">`X` is covariant, `Y` is contravariant and `Z` is invariant.</span></span>

#### <a name="variance-safety"></a><span data-ttu-id="80ea2-129">Seguridad de variación</span><span class="sxs-lookup"><span data-stu-id="80ea2-129">Variance safety</span></span>

<span data-ttu-id="80ea2-130">La aparición de las anotaciones de varianza en la lista de parámetros de tipo de un tipo restringe los lugares donde pueden producirse tipos dentro de la declaración de tipos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-130">The occurrence of variance annotations in the type parameter list of a type restricts the places where types can occur within the type declaration.</span></span>

<span data-ttu-id="80ea2-131">Un tipo `T` es ***salida unsafe*** si uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="80ea2-131">A type `T` is ***output-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="80ea2-132">`T` es un parámetro de tipo contravariante</span><span class="sxs-lookup"><span data-stu-id="80ea2-132">`T` is a contravariant type parameter</span></span>
*  <span data-ttu-id="80ea2-133">`T` es un tipo de matriz con un tipo de elemento de salida no seguros</span><span class="sxs-lookup"><span data-stu-id="80ea2-133">`T` is an array type with an output-unsafe element type</span></span>
*  <span data-ttu-id="80ea2-134">`T` es un tipo de interfaz o delegado `S<A1,...,Ak>` construido a partir de un tipo genérico `S<X1,...,Xk>` where para al menos una `Ai` uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="80ea2-134">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="80ea2-135">`Xi` es covariante o invariable y `Ai` no es seguro para la salida.</span><span class="sxs-lookup"><span data-stu-id="80ea2-135">`Xi` is covariant or invariant and `Ai` is output-unsafe.</span></span>
   * <span data-ttu-id="80ea2-136">`Xi` es contravariante o invariable y `Ai` tiene seguridad de entrada.</span><span class="sxs-lookup"><span data-stu-id="80ea2-136">`Xi` is contravariant or invariant and `Ai` is input-safe.</span></span>
   
<span data-ttu-id="80ea2-137">Un tipo `T` es ***entrada unsafe*** si uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="80ea2-137">A type `T` is ***input-unsafe*** if one of the following holds:</span></span>

*  <span data-ttu-id="80ea2-138">`T` es un parámetro de tipo covariante</span><span class="sxs-lookup"><span data-stu-id="80ea2-138">`T` is a covariant type parameter</span></span>
*  <span data-ttu-id="80ea2-139">`T` es un tipo de matriz con un tipo de elemento de entrada no seguros</span><span class="sxs-lookup"><span data-stu-id="80ea2-139">`T` is an array type with an input-unsafe element type</span></span>
*  <span data-ttu-id="80ea2-140">`T` es un tipo de interfaz o delegado `S<A1,...,Ak>` construido a partir de un tipo genérico `S<X1,...,Xk>` where para al menos una `Ai` uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="80ea2-140">`T` is an interface or delegate type `S<A1,...,Ak>` constructed from a generic type `S<X1,...,Xk>` where for at least one `Ai` one of the following holds:</span></span>
   * <span data-ttu-id="80ea2-141">`Xi` es covariante o invariable y `Ai` no es seguro para la entrada.</span><span class="sxs-lookup"><span data-stu-id="80ea2-141">`Xi` is covariant or invariant and `Ai` is input-unsafe.</span></span>
   * <span data-ttu-id="80ea2-142">`Xi` es contravariante o invariable y `Ai` no es seguro para la salida.</span><span class="sxs-lookup"><span data-stu-id="80ea2-142">`Xi` is contravariant or invariant and `Ai` is output-unsafe.</span></span>

<span data-ttu-id="80ea2-143">De manera intuitiva, un tipo de salida no seguros está prohibido en una posición de salida y un tipo de entrada no seguros está prohibido en una posición de entrada.</span><span class="sxs-lookup"><span data-stu-id="80ea2-143">Intuitively, an output-unsafe type is prohibited in an output position, and an input-unsafe type is prohibited in an input position.</span></span>

<span data-ttu-id="80ea2-144">Es un tipo ***con seguridad de salida*** si no es segura de salida, y ***con seguridad de entrada*** si no no es seguro para la entrada.</span><span class="sxs-lookup"><span data-stu-id="80ea2-144">A type is ***output-safe*** if it is not output-unsafe, and ***input-safe*** if it is not input-unsafe.</span></span>

#### <a name="variance-conversion"></a><span data-ttu-id="80ea2-145">Conversión de varianza</span><span class="sxs-lookup"><span data-stu-id="80ea2-145">Variance conversion</span></span>

<span data-ttu-id="80ea2-146">El propósito de las anotaciones de varianza es proporcionar conversiones más flexible (pero todavía seguridad de tipos) en tipos de interfaz y delegado.</span><span class="sxs-lookup"><span data-stu-id="80ea2-146">The purpose of variance annotations is to provide for more lenient (but still type safe) conversions to interface and delegate types.</span></span> <span data-ttu-id="80ea2-147">A este fin las definiciones de implícito ([conversiones implícitas](conversions.md#implicit-conversions)) y las conversiones explícitas ([las conversiones explícitas](conversions.md#explicit-conversions)) asegúrese de usar de la noción de varianza, que se define como sigue:</span><span class="sxs-lookup"><span data-stu-id="80ea2-147">To this end the definitions of implicit ([Implicit conversions](conversions.md#implicit-conversions)) and explicit conversions ([Explicit conversions](conversions.md#explicit-conversions)) make use of the notion of variance-convertibility, which is defined as follows:</span></span>

<span data-ttu-id="80ea2-148">Un tipo `T<A1,...,An>` es convertible de varianza a un tipo `T<B1,...,Bn>` si `T` se declara una interfaz o un tipo de delegado con los parámetros de tipo de variante `T<X1,...,Xn>`y para cada parámetro de tipo de variante `Xi` uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="80ea2-148">A type `T<A1,...,An>` is variance-convertible to a type `T<B1,...,Bn>` if `T` is either an interface or a delegate type declared with the variant type parameters `T<X1,...,Xn>`, and for each variant type parameter `Xi` one of the following holds:</span></span>

*  <span data-ttu-id="80ea2-149">`Xi` es covariante y existe una conversión implícita de referencia o identidad de `Ai` a `Bi`</span><span class="sxs-lookup"><span data-stu-id="80ea2-149">`Xi` is covariant and an implicit reference or identity conversion exists from `Ai` to `Bi`</span></span>
*  <span data-ttu-id="80ea2-150">`Xi` es contravariante y una referencia implícita o no existe conversión de identidad de `Bi` a `Ai`</span><span class="sxs-lookup"><span data-stu-id="80ea2-150">`Xi` is contravariant and an implicit reference or identity conversion exists from `Bi` to `Ai`</span></span>
*  <span data-ttu-id="80ea2-151">`Xi` es invariable y una identidad no existe conversión de `Ai` a `Bi`</span><span class="sxs-lookup"><span data-stu-id="80ea2-151">`Xi` is invariant and an identity conversion exists from `Ai` to `Bi`</span></span>

### <a name="base-interfaces"></a><span data-ttu-id="80ea2-152">Interfaces base</span><span class="sxs-lookup"><span data-stu-id="80ea2-152">Base interfaces</span></span>

<span data-ttu-id="80ea2-153">Una interfaz puede heredar de cero o más tipos de interfaz, que se denominan el ***interfaces base explícitas*** de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-153">An interface can inherit from zero or more interface types, which are called the ***explicit base interfaces*** of the interface.</span></span> <span data-ttu-id="80ea2-154">Cuando una interfaz tiene una o varias interfaces base explícitas, a continuación, en la declaración de dicha interfaz, el identificador de interfaz es seguido por dos puntos y comas separados por la lista de tipos de interfaz base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-154">When an interface has one or more explicit base interfaces, then in the declaration of that interface, the interface identifier is followed by a colon and a comma separated list of base interface types.</span></span>

```antlr
interface_base
    : ':' interface_type_list
    ;
```

<span data-ttu-id="80ea2-155">Para un tipo de interfaz construida, las interfaces base explícitas se forman tomando las declaraciones de interfaz base explícita en la declaración de tipo genérico y sustituyendo, para cada *tipo_parámetro* en la interfaz base declaración, la correspondiente *type_argument* del tipo construido.</span><span class="sxs-lookup"><span data-stu-id="80ea2-155">For a constructed interface type, the explicit base interfaces are formed by taking the explicit base interface declarations on the generic type declaration, and substituting, for each *type_parameter* in the base interface declaration, the corresponding *type_argument* of the constructed type.</span></span>

<span data-ttu-id="80ea2-156">Las interfaces base explícitas de una interfaz deben ser al menos tan accesibles como la propia interfaz ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-156">The explicit base interfaces of an interface must be at least as accessible as the interface itself ([Accessibility constraints](basic-concepts.md#accessibility-constraints)).</span></span> <span data-ttu-id="80ea2-157">Por ejemplo, es un error en tiempo de compilación para especificar un `private` o `internal` interfaz en el *interface_base* de un `public` interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-157">For example, it is a compile-time error to specify a `private` or `internal` interface in the *interface_base* of a `public` interface.</span></span>

<span data-ttu-id="80ea2-158">Es un error en tiempo de compilación para una interfaz para heredar directa o indirectamente de sí misma.</span><span class="sxs-lookup"><span data-stu-id="80ea2-158">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>

<span data-ttu-id="80ea2-159">El ***interfaces base*** de una interfaz son las interfaces base explícitas y sus interfaces bases.</span><span class="sxs-lookup"><span data-stu-id="80ea2-159">The ***base interfaces*** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="80ea2-160">En otras palabras, el conjunto de interfaces base es el cierre transitivo completando de las interfaces base explícitas, sus interfaces base explícitas y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="80ea2-160">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span> <span data-ttu-id="80ea2-161">Una interfaz hereda a todos los miembros de sus interfaces base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-161">An interface inherits all members of its base interfaces.</span></span> <span data-ttu-id="80ea2-162">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-162">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="80ea2-163">las interfaces base de `IComboBox` son `IControl`, `ITextBox`, y `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-163">the base interfaces of `IComboBox` are `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="80ea2-164">En otras palabras, el `IComboBox` anterior de la interfaz hereda miembros `SetText` y `SetItems` como `Paint`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-164">In other words, the `IComboBox` interface above inherits members `SetText` and `SetItems` as well as `Paint`.</span></span>

<span data-ttu-id="80ea2-165">Cada interfaz de la base de una interfaz debe tener la seguridad de salida ([seguridad varianza](interfaces.md#variance-safety)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-165">Every base interface of an interface must be output-safe ([Variance safety](interfaces.md#variance-safety)).</span></span> <span data-ttu-id="80ea2-166">Una clase o estructura que implementa una interfaz implícitamente también implementa todas las interfaces de la interfaz base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-166">A class or struct that implements an interface also implicitly implements all of the interface's base interfaces.</span></span>

### <a name="interface-body"></a><span data-ttu-id="80ea2-167">Cuerpo de la interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-167">Interface body</span></span>

<span data-ttu-id="80ea2-168">El *interface_body* de una interfaz define los miembros de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-168">The *interface_body* of an interface defines the members of the interface.</span></span>

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a><span data-ttu-id="80ea2-169">Miembros de interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-169">Interface members</span></span>

<span data-ttu-id="80ea2-170">Los miembros de una interfaz son los miembros heredados de las interfaces base y los miembros declaran por la propia interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-170">The members of an interface are the members inherited from the base interfaces and the members declared by the interface itself.</span></span>

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

<span data-ttu-id="80ea2-171">Una declaración de interfaz puede declarar a cero o más miembros.</span><span class="sxs-lookup"><span data-stu-id="80ea2-171">An interface declaration may declare zero or more members.</span></span> <span data-ttu-id="80ea2-172">Los miembros de una interfaz deben ser métodos, propiedades, eventos o indizadores.</span><span class="sxs-lookup"><span data-stu-id="80ea2-172">The members of an interface must be methods, properties, events, or indexers.</span></span> <span data-ttu-id="80ea2-173">Una interfaz no puede contener constantes, campos, operadores, constructores de instancias, destructores o tipos, ni puede una interfaz contener a miembros estáticos de ningún tipo.</span><span class="sxs-lookup"><span data-stu-id="80ea2-173">An interface cannot contain constants, fields, operators, instance constructors, destructors, or types, nor can an interface contain static members of any kind.</span></span>

<span data-ttu-id="80ea2-174">Todos los miembros de interfaz tienen implícitamente acceso público.</span><span class="sxs-lookup"><span data-stu-id="80ea2-174">All interface members implicitly have public access.</span></span> <span data-ttu-id="80ea2-175">Es un error en tiempo de compilación para las declaraciones de miembros de interfaz incluir los modificadores.</span><span class="sxs-lookup"><span data-stu-id="80ea2-175">It is a compile-time error for interface member declarations to include any modifiers.</span></span> <span data-ttu-id="80ea2-176">En concreto, los miembros de interfaces no se pueden declarar con los modificadores `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, o `static`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-176">In particular, interfaces members cannot be declared with the modifiers `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="80ea2-177">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-177">The example</span></span>
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
<span data-ttu-id="80ea2-178">declara una interfaz que contiene uno de los posibles tipos de miembros: Un método, una propiedad, un evento y un indizador.</span><span class="sxs-lookup"><span data-stu-id="80ea2-178">declares an interface that contains one each of the possible kinds of members: A method, a property, an event, and an indexer.</span></span>

<span data-ttu-id="80ea2-179">Un *interface_declaration* crea un nuevo espacio de declaración ([declaraciones](basic-concepts.md#declarations)) y el *interface_member_declaration*s contenidas inmediatamente en el *interface_declaration* introducir nuevos miembros en este espacio de declaración.</span><span class="sxs-lookup"><span data-stu-id="80ea2-179">An *interface_declaration* creates a new declaration space ([Declarations](basic-concepts.md#declarations)), and the *interface_member_declaration*s immediately contained by the *interface_declaration* introduce new members into this declaration space.</span></span> <span data-ttu-id="80ea2-180">Las siguientes reglas se aplican a *interface_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="80ea2-180">The following rules apply to *interface_member_declaration*s:</span></span>

*  <span data-ttu-id="80ea2-181">El nombre de un método debe ser diferente de los nombres de todas las propiedades y los eventos declarados en la misma interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-181">The name of a method must differ from the names of all properties and events declared in the same interface.</span></span> <span data-ttu-id="80ea2-182">Además, la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un método debe ser diferentes de las firmas de todos los demás métodos declarados en la misma interfaz, y dos métodos declarados en la misma interfaz no pueden tener firmas que se diferencian solamente por `ref` y `out`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-182">In addition, the signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)) of a method must differ from the signatures of all other methods declared in the same interface, and two methods declared in the same interface may not have signatures that differ solely by `ref` and `out`.</span></span>
*  <span data-ttu-id="80ea2-183">El nombre de una propiedad o evento debe ser diferente de los nombres de todos los demás miembros declarados en la misma interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-183">The name of a property or event must differ from the names of all other members declared in the same interface.</span></span>
*  <span data-ttu-id="80ea2-184">La firma de un indexador debe ser diferente de las firmas de los demás indexadores declarados en la misma interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-184">The signature of an indexer must differ from the signatures of all other indexers declared in the same interface.</span></span>

<span data-ttu-id="80ea2-185">Los miembros heredados de una interfaz específicamente no forman parte del espacio de declaración de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-185">The inherited members of an interface are specifically not part of the declaration space of the interface.</span></span> <span data-ttu-id="80ea2-186">Por lo tanto, una interfaz puede declarar a un miembro con el mismo nombre o signatura que un miembro heredado.</span><span class="sxs-lookup"><span data-stu-id="80ea2-186">Thus, an interface is allowed to declare a member with the same name or signature as an inherited member.</span></span> <span data-ttu-id="80ea2-187">Cuando esto ocurre, se dice que el miembro de interfaz derivada oculta al miembro de interfaz base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-187">When this occurs, the derived interface member is said to hide the base interface member.</span></span> <span data-ttu-id="80ea2-188">Ocultar a un miembro heredado no se considera un error, pero hace que el compilador emita una advertencia.</span><span class="sxs-lookup"><span data-stu-id="80ea2-188">Hiding an inherited member is not considered an error, but it does cause the compiler to issue a warning.</span></span> <span data-ttu-id="80ea2-189">Para suprimir la advertencia, la declaración del miembro de interfaz derivado debe incluir un `new` modificador para indicar que el miembro derivado está diseñado para ocultar el miembro base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-189">To suppress the warning, the declaration of the derived interface member must include a `new` modifier to indicate that the derived member is intended to hide the base member.</span></span> <span data-ttu-id="80ea2-190">En este tema se describe con más detalle en [ocultar mediante herencia](basic-concepts.md#hiding-through-inheritance).</span><span class="sxs-lookup"><span data-stu-id="80ea2-190">This topic is discussed further in [Hiding through inheritance](basic-concepts.md#hiding-through-inheritance).</span></span>

<span data-ttu-id="80ea2-191">Si un `new` modificador se incluye en una declaración que no oculta un miembro heredado, se emite una advertencia a tal efecto.</span><span class="sxs-lookup"><span data-stu-id="80ea2-191">If a `new` modifier is included in a declaration that doesn't hide an inherited member, a warning is issued to that effect.</span></span> <span data-ttu-id="80ea2-192">Se puede suprimir esta advertencia mediante la eliminación de la `new` modificador.</span><span class="sxs-lookup"><span data-stu-id="80ea2-192">This warning is suppressed by removing the `new` modifier.</span></span>

<span data-ttu-id="80ea2-193">Tenga en cuenta que los miembros de clase `object` no son estrictamente hablando, los miembros de cualquier interfaz ([miembros de interfaz](interfaces.md#interface-members)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-193">Note that the members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="80ea2-194">Sin embargo, los miembros de clase `object` están disponibles a través de la búsqueda de miembros en cualquier tipo de interfaz ([búsqueda de miembros](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-194">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="interface-methods"></a><span data-ttu-id="80ea2-195">Métodos de interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-195">Interface methods</span></span>

<span data-ttu-id="80ea2-196">Los métodos de interfaz se declaran mediante *interface_method_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="80ea2-196">Interface methods are declared using *interface_method_declaration*s:</span></span>

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

<span data-ttu-id="80ea2-197">El *atributos*, *return_type*, *identificador*, y *formal_parameter_list* de una declaración de método de interfaz tienen el mismo lo que significa que los de una declaración de método en una clase ([métodos](classes.md#methods)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-197">The *attributes*, *return_type*, *identifier*, and *formal_parameter_list* of an interface method declaration have the same meaning as those of a method declaration in a class ([Methods](classes.md#methods)).</span></span> <span data-ttu-id="80ea2-198">No se permite una declaración de método de interfaz para especificar un cuerpo de método y la declaración, por tanto, siempre termina con un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="80ea2-198">An interface method declaration is not permitted to specify a method body, and the declaration therefore always ends with a semicolon.</span></span>

<span data-ttu-id="80ea2-199">Cada tipo de parámetros formales de un método de interfaz debe ser seguro para entrada ([seguridad varianza](interfaces.md#variance-safety)), y el tipo de valor devuelto debe ser `void` o seguridad de salida.</span><span class="sxs-lookup"><span data-stu-id="80ea2-199">Each formal parameter type of an interface method must be input-safe ([Variance safety](interfaces.md#variance-safety)), and the return type must be either `void` or output-safe.</span></span> <span data-ttu-id="80ea2-200">Además, cada restricción de tipo de clase, la restricción de tipo de interfaz y la restricción de parámetro de tipo en cualquier parámetro de tipo del método deben ser con seguridad de entrada.</span><span class="sxs-lookup"><span data-stu-id="80ea2-200">Furthermore, each class type constraint, interface type constraint and type parameter constraint on any type parameter of the method must be input-safe.</span></span>

<span data-ttu-id="80ea2-201">Estas reglas garantizan que cualquier covariante o contravariante uso de la interfaz mantiene la seguridad de tipos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-201">These rules ensure that any covariant or contravariant usage of the interface remains type-safe.</span></span> <span data-ttu-id="80ea2-202">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="80ea2-202">For example,</span></span>
```csharp
interface I<out T> { void M<U>() where U : T; }
```
<span data-ttu-id="80ea2-203">no es válido porque el uso de `T` como una restricción de parámetro de tipo en `U` no tiene seguridad de entrada.</span><span class="sxs-lookup"><span data-stu-id="80ea2-203">is illegal because the usage of `T` as a type parameter constraint on `U` is not input-safe.</span></span>

<span data-ttu-id="80ea2-204">Había esta restricción no sería posible infracción de seguridad de tipos de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="80ea2-204">Were this restriction not in place it would be possible to violate type safety in the following manner:</span></span>
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
<span data-ttu-id="80ea2-205">Esto es realmente una llamada a `C.M<E>`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-205">This is actually a call to `C.M<E>`.</span></span> <span data-ttu-id="80ea2-206">Pero esa llamada requiere que `E` derivan `D`, por lo que aquí se infringiría la seguridad de tipos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-206">But that call requires that `E` derive from `D`, so type safety would be violated here.</span></span>

### <a name="interface-properties"></a><span data-ttu-id="80ea2-207">Propiedades de la interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-207">Interface properties</span></span>

<span data-ttu-id="80ea2-208">Propiedades de la interfaz se declaran mediante *interface_property_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="80ea2-208">Interface properties are declared using *interface_property_declaration*s:</span></span>

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

<span data-ttu-id="80ea2-209">El *atributos*, *tipo*, y *identificador* de una declaración de propiedad de interfaz tienen el mismo significado que los de una declaración de propiedad en una clase ([ Propiedades](classes.md#properties)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-209">The *attributes*, *type*, and *identifier* of an interface property declaration have the same meaning as those of a property declaration in a class ([Properties](classes.md#properties)).</span></span>

<span data-ttu-id="80ea2-210">Los descriptores de acceso de una declaración de propiedad de interfaz corresponden a los descriptores de acceso de una declaración de propiedad de clase ([descriptores de acceso](classes.md#accessors)), salvo que el cuerpo del descriptor de acceso siempre debe ser un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="80ea2-210">The accessors of an interface property declaration correspond to the accessors of a class property declaration ([Accessors](classes.md#accessors)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="80ea2-211">Por lo tanto, los descriptores de acceso es indican si la propiedad es de lectura y escritura, de solo lectura o solo escritura.</span><span class="sxs-lookup"><span data-stu-id="80ea2-211">Thus, the accessors simply indicate whether the property is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="80ea2-212">El tipo de una propiedad de interfaz debe ser seguro para la salida si hay un descriptor de acceso get y debe ser seguro para la entrada si hay un descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="80ea2-212">The type of an interface property must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-events"></a><span data-ttu-id="80ea2-213">Eventos de interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-213">Interface events</span></span>

<span data-ttu-id="80ea2-214">Eventos de interfaz se declaran mediante *interface_event_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="80ea2-214">Interface events are declared using *interface_event_declaration*s:</span></span>

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

<span data-ttu-id="80ea2-215">El *atributos*, *tipo*, y *identificador* de una declaración de evento de interfaz tienen el mismo significado que los de una declaración de evento en una clase ([eventos ](classes.md#events)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-215">The *attributes*, *type*, and *identifier* of an interface event declaration have the same meaning as those of an event declaration in a class ([Events](classes.md#events)).</span></span>

<span data-ttu-id="80ea2-216">El tipo de un evento de interfaz debe ser con seguridad de entrada.</span><span class="sxs-lookup"><span data-stu-id="80ea2-216">The type of an interface event must be input-safe.</span></span>

### <a name="interface-indexers"></a><span data-ttu-id="80ea2-217">Indizadores en interfaces</span><span class="sxs-lookup"><span data-stu-id="80ea2-217">Interface indexers</span></span>

<span data-ttu-id="80ea2-218">Indizadores en interfaces se declaran mediante *interface_indexer_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="80ea2-218">Interface indexers are declared using *interface_indexer_declaration*s:</span></span>

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

<span data-ttu-id="80ea2-219">El *atributos*, *tipo*, y *formal_parameter_list* una declaración de interfaz indizador tienen el mismo significado que los de una declaración de indizador en una clase ([ Los indizadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-219">The *attributes*, *type*, and *formal_parameter_list* of an interface indexer declaration have the same meaning as those of an indexer declaration in a class ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="80ea2-220">Los descriptores de acceso de una declaración de indexador de interfaz corresponden a los descriptores de acceso de una declaración de indizador de clase ([indizadores](classes.md#indexers)), salvo que el cuerpo del descriptor de acceso siempre debe ser un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="80ea2-220">The accessors of an interface indexer declaration correspond to the accessors of a class indexer declaration ([Indexers](classes.md#indexers)), except that the accessor body must always be a semicolon.</span></span> <span data-ttu-id="80ea2-221">Por lo tanto, los descriptores de acceso es indican si el indizador es de lectura y escritura, de solo lectura o solo escritura.</span><span class="sxs-lookup"><span data-stu-id="80ea2-221">Thus, the accessors simply indicate whether the indexer is read-write, read-only, or write-only.</span></span>

<span data-ttu-id="80ea2-222">Todos los tipos de parámetros formales de un indizador de interfaz deben ser con seguridad de entrada.</span><span class="sxs-lookup"><span data-stu-id="80ea2-222">All the formal parameter types of an interface indexer must be input-safe .</span></span> <span data-ttu-id="80ea2-223">Además, cualquier `out` o `ref` tipos de parámetros formales también deben ser seguro para salida.</span><span class="sxs-lookup"><span data-stu-id="80ea2-223">In addition, any `out` or `ref` formal parameter types must also be output-safe.</span></span> <span data-ttu-id="80ea2-224">Tenga en cuenta que, incluso `out` parámetros son necesarios para tener seguridad de entrada, debido a una limitación de la plataforma subyacente de la ejecución.</span><span class="sxs-lookup"><span data-stu-id="80ea2-224">Note that even `out` parameters are required to be input-safe, due to a limitation of the underlying execution platform.</span></span>

<span data-ttu-id="80ea2-225">El tipo de un indizador de interfaz debe ser seguro para la salida si hay un descriptor de acceso get y debe ser seguro para la entrada si hay un descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="80ea2-225">The type of an interface indexer must be output-safe if there is a get accessor, and must be input-safe if there is a set accessor.</span></span>

### <a name="interface-member-access"></a><span data-ttu-id="80ea2-226">Acceso a miembros de interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-226">Interface member access</span></span>

<span data-ttu-id="80ea2-227">Se tiene acceso a los miembros de interfaz a través de acceso a miembros ([acceso a miembros](expressions.md#member-access)) y el acceso de indizador ([acceso al indizador](expressions.md#indexer-access)) expresiones de la forma `I.M` y `I[A]`, donde `I` es un tipo de interfaz, `M` es un método, propiedad o evento de ese tipo de interfaz, y `A` es una lista de argumentos del indizador.</span><span class="sxs-lookup"><span data-stu-id="80ea2-227">Interface members are accessed through member access ([Member access](expressions.md#member-access)) and indexer access ([Indexer access](expressions.md#indexer-access)) expressions of the form `I.M` and `I[A]`, where `I` is an interface type, `M` is a method, property, or event of that interface type, and `A` is an indexer argument list.</span></span>

<span data-ttu-id="80ea2-228">Para las interfaces que son estrictamente de herencia simple (cada interfaz en la cadena de herencia tiene exactamente cero o una interfaz base directa), los efectos de la búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)), la invocación de método ([ Las invocaciones de método](expressions.md#method-invocations)) y el acceso de indizador ([acceso al indizador](expressions.md#indexer-access)) las reglas son exactamente los mismos que para las clases y estructuras: Ocultar miembros más derivados de menos miembros derivados con el mismo nombre o la firma.</span><span class="sxs-lookup"><span data-stu-id="80ea2-228">For interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effects of the member lookup ([Member lookup](expressions.md#member-lookup)), method invocation ([Method invocations](expressions.md#method-invocations)), and indexer access ([Indexer access](expressions.md#indexer-access)) rules are exactly the same as for classes and structs: More derived members hide less derived members with the same name or signature.</span></span> <span data-ttu-id="80ea2-229">Sin embargo, para las interfaces de herencia múltiple, las ambigüedades que pueden producirse cuando dos o más interfaces base no relacionadas declaran a miembros con el mismo nombre o signatura.</span><span class="sxs-lookup"><span data-stu-id="80ea2-229">However, for multiple-inheritance interfaces, ambiguities can occur when two or more unrelated base interfaces declare members with the same name or signature.</span></span> <span data-ttu-id="80ea2-230">En esta sección se muestra varios ejemplos de tales situaciones.</span><span class="sxs-lookup"><span data-stu-id="80ea2-230">This section shows several examples of such situations.</span></span> <span data-ttu-id="80ea2-231">En todos los casos, se pueden usar conversiones explícitas para resolver las ambigüedades.</span><span class="sxs-lookup"><span data-stu-id="80ea2-231">In all cases, explicit casts can be used to resolve the ambiguities.</span></span>

<span data-ttu-id="80ea2-232">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-232">In the example</span></span>
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
<span data-ttu-id="80ea2-233">las dos primeras instrucciones provocar errores en tiempo de compilación porque la búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)) de `Count` en `IListCounter` es ambiguo.</span><span class="sxs-lookup"><span data-stu-id="80ea2-233">the first two statements cause compile-time errors because the member lookup ([Member lookup](expressions.md#member-lookup)) of `Count` in `IListCounter` is ambiguous.</span></span> <span data-ttu-id="80ea2-234">Como se muestra en el ejemplo, la ambigüedad se resuelve mediante la conversión `x` para el tipo de interfaz base adecuada.</span><span class="sxs-lookup"><span data-stu-id="80ea2-234">As illustrated by the example, the ambiguity is resolved by casting `x` to the appropriate base interface type.</span></span> <span data-ttu-id="80ea2-235">Tales conversiones no tienen ningún costo de tiempo de ejecución, simplemente consisten en ver la instancia como un tipo menos derivado en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="80ea2-235">Such casts have no run-time costs—they merely consist of viewing the instance as a less derived type at compile-time.</span></span>

<span data-ttu-id="80ea2-236">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-236">In the example</span></span>
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
<span data-ttu-id="80ea2-237">la invocación `n.Add(1)` selecciona `IInteger.Add` aplicando las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="80ea2-237">the invocation `n.Add(1)` selects `IInteger.Add` by applying the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="80ea2-238">De forma similar a la invocación `n.Add(1.0)` selecciona `IDouble.Add`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-238">Similarly the invocation `n.Add(1.0)` selects `IDouble.Add`.</span></span> <span data-ttu-id="80ea2-239">Cuando se insertan conversiones explícitas, hay solo un candidato método y, por tanto, no hay ambigüedad.</span><span class="sxs-lookup"><span data-stu-id="80ea2-239">When explicit casts are inserted, there is only one candidate method, and thus no ambiguity.</span></span>

<span data-ttu-id="80ea2-240">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-240">In the example</span></span>
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
<span data-ttu-id="80ea2-241">el `IBase.F` miembro está oculto por la `ILeft.F` miembro.</span><span class="sxs-lookup"><span data-stu-id="80ea2-241">the `IBase.F` member is hidden by the `ILeft.F` member.</span></span> <span data-ttu-id="80ea2-242">La invocación `d.F(1)` , por tanto, se selecciona `ILeft.F`, aunque `IBase.F` parece no estar oculto en la ruta de acceso que le guíe por `IRight`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-242">The invocation `d.F(1)` thus selects `ILeft.F`, even though `IBase.F` appears to not be hidden in the access path that leads through `IRight`.</span></span>

<span data-ttu-id="80ea2-243">La regla intuitiva para la ocultación en interfaces de herencia múltiple es simplemente el siguiente: Si un miembro está oculto en cualquier ruta de acceso, se oculta en todas las rutas de acceso.</span><span class="sxs-lookup"><span data-stu-id="80ea2-243">The intuitive rule for hiding in multiple-inheritance interfaces is simply this: If a member is hidden in any access path, it is hidden in all access paths.</span></span> <span data-ttu-id="80ea2-244">Dado que la ruta de acceso de `IDerived` a `ILeft` a `IBase` oculta `IBase.F`, también se oculta el miembro en la ruta de acceso de `IDerived` a `IRight` a `IBase`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-244">Because the access path from `IDerived` to `ILeft` to `IBase` hides `IBase.F`, the member is also hidden in the access path from `IDerived` to `IRight` to `IBase`.</span></span>

## <a name="fully-qualified-interface-member-names"></a><span data-ttu-id="80ea2-245">Nombres de miembro de interfaz completo</span><span class="sxs-lookup"><span data-stu-id="80ea2-245">Fully qualified interface member names</span></span>

<span data-ttu-id="80ea2-246">Un miembro de interfaz a veces se conoce por su ***nombre completo***.</span><span class="sxs-lookup"><span data-stu-id="80ea2-246">An interface member is sometimes referred to by its ***fully qualified name***.</span></span> <span data-ttu-id="80ea2-247">El nombre completo de un miembro de interfaz consta del nombre de la interfaz en la que el miembro se declara, seguido por un punto, seguido del nombre del miembro.</span><span class="sxs-lookup"><span data-stu-id="80ea2-247">The fully qualified name of an interface member consists of the name of the interface in which the member is declared, followed by a dot, followed by the name of the member.</span></span> <span data-ttu-id="80ea2-248">El nombre completo de un miembro hace referencia a la interfaz en la que se declara el miembro.</span><span class="sxs-lookup"><span data-stu-id="80ea2-248">The fully qualified name of a member references the interface in which the member is declared.</span></span> <span data-ttu-id="80ea2-249">Por ejemplo, dadas las declaraciones</span><span class="sxs-lookup"><span data-stu-id="80ea2-249">For example, given the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
<span data-ttu-id="80ea2-250">el nombre completo de `Paint` es `IControl.Paint` y el nombre completo de `SetText` es `ITextBox.SetText`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-250">the fully qualified name of `Paint` is `IControl.Paint` and the fully qualified name of `SetText` is `ITextBox.SetText`.</span></span>

<span data-ttu-id="80ea2-251">En el ejemplo anterior, no es posible hacer referencia a `Paint` como `ITextBox.Paint`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-251">In the example above, it is not possible to refer to `Paint` as `ITextBox.Paint`.</span></span>

<span data-ttu-id="80ea2-252">Cuando una interfaz es parte de un espacio de nombres, el nombre completo de un miembro de interfaz incluye el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="80ea2-252">When an interface is part of a namespace, the fully qualified name of an interface member includes the namespace name.</span></span> <span data-ttu-id="80ea2-253">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-253">For example</span></span>
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

<span data-ttu-id="80ea2-254">Aquí, el nombre completo de la `Clone` método es `System.ICloneable.Clone`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-254">Here, the fully qualified name of the `Clone` method is `System.ICloneable.Clone`.</span></span>

## <a name="interface-implementations"></a><span data-ttu-id="80ea2-255">Implementaciones de interfaces</span><span class="sxs-lookup"><span data-stu-id="80ea2-255">Interface implementations</span></span>

<span data-ttu-id="80ea2-256">Interfaces se pueden implementar mediante clases y structs.</span><span class="sxs-lookup"><span data-stu-id="80ea2-256">Interfaces may be implemented by classes and structs.</span></span> <span data-ttu-id="80ea2-257">Para indicar que una clase o estructura implementa directamente una interfaz, el identificador de interfaz se incluye en la lista de clases base de la clase o struct.</span><span class="sxs-lookup"><span data-stu-id="80ea2-257">To indicate that a class or struct directly implements an interface, the interface identifier is included in the base class list of the class or struct.</span></span> <span data-ttu-id="80ea2-258">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="80ea2-258">For example:</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

<span data-ttu-id="80ea2-259">Una clase o estructura que implementa directamente una interfaz también directamente implementa todas las interfaces de base de la interfaz de forma implícita.</span><span class="sxs-lookup"><span data-stu-id="80ea2-259">A class or struct that directly implements an interface also directly implements all of the interface's base interfaces implicitly.</span></span> <span data-ttu-id="80ea2-260">Esto es cierto incluso si la clase o estructura no muestre explícitamente todas las interfaces base en la lista de clases base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-260">This is true even if the class or struct doesn't explicitly list all base interfaces in the base class list.</span></span> <span data-ttu-id="80ea2-261">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="80ea2-261">For example:</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

<span data-ttu-id="80ea2-262">En este caso, la clase `TextBox` implementa tanto `IControl` y `ITextBox`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-262">Here, class `TextBox` implements both `IControl` and `ITextBox`.</span></span>

<span data-ttu-id="80ea2-263">Cuando una clase `C` directamente implementa una interfaz, todas las clases derivadas de C también implementa la interfaz implícitamente.</span><span class="sxs-lookup"><span data-stu-id="80ea2-263">When a class `C` directly implements an interface, all classes derived from C also implement the interface implicitly.</span></span> <span data-ttu-id="80ea2-264">Las interfaces base especificadas en una declaración de clase pueden ser tipos de interfaz construida ([construido tipos](types.md#constructed-types)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-264">The base interfaces specified in a class declaration can be constructed interface types ([Constructed types](types.md#constructed-types)).</span></span> <span data-ttu-id="80ea2-265">Una interfaz base no puede ser un parámetro de tipo por sí mismo, aunque puede implicar a los parámetros de tipo que están en ámbito.</span><span class="sxs-lookup"><span data-stu-id="80ea2-265">A base interface cannot be a type parameter on its own, though it can involve the type parameters that are in scope.</span></span> <span data-ttu-id="80ea2-266">El código siguiente muestra cómo una clase puede implementar y ampliar los tipos construidos:</span><span class="sxs-lookup"><span data-stu-id="80ea2-266">The following code illustrates how a class can implement and extend constructed types:</span></span>
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

<span data-ttu-id="80ea2-267">Las interfaces base de una declaración de clase genérica deben cumplir la regla de unicidad se describe en [exclusividad de interfaces implementadas](interfaces.md#uniqueness-of-implemented-interfaces).</span><span class="sxs-lookup"><span data-stu-id="80ea2-267">The base interfaces of a generic class declaration must satisfy the uniqueness rule described in [Uniqueness of implemented interfaces](interfaces.md#uniqueness-of-implemented-interfaces).</span></span>

### <a name="explicit-interface-member-implementations"></a><span data-ttu-id="80ea2-268">Implementaciones de miembros de interfaz explícita</span><span class="sxs-lookup"><span data-stu-id="80ea2-268">Explicit interface member implementations</span></span>

<span data-ttu-id="80ea2-269">Para fines de implementación de interfaces, puede declarar una clase o struct ***implementaciones de miembros de interfaz explícitas***.</span><span class="sxs-lookup"><span data-stu-id="80ea2-269">For purposes of implementing interfaces, a class or struct may declare ***explicit interface member implementations***.</span></span> <span data-ttu-id="80ea2-270">Una implementación de miembro de interfaz explícita es una declaración de método, propiedad, evento o indizador que hace referencia a un nombre de miembro de interfaz completo.</span><span class="sxs-lookup"><span data-stu-id="80ea2-270">An explicit interface member implementation is a method, property, event, or indexer declaration that references a fully qualified interface member name.</span></span> <span data-ttu-id="80ea2-271">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-271">For example</span></span>
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

<span data-ttu-id="80ea2-272">Aquí `IDictionary<int,T>.this` y `IDictionary<int,T>.Add` es implementaciones de miembros de interfaz explícita.</span><span class="sxs-lookup"><span data-stu-id="80ea2-272">Here `IDictionary<int,T>.this` and `IDictionary<int,T>.Add` are explicit interface member implementations.</span></span>

<span data-ttu-id="80ea2-273">En algunos casos, el nombre de un miembro de interfaz no puede ser adecuado para la clase de implementación en el que caso el miembro de interfaz puede implementarse mediante la implementación del miembro de interfaz explícita.</span><span class="sxs-lookup"><span data-stu-id="80ea2-273">In some cases, the name of an interface member may not be appropriate for the implementing class, in which case the interface member may be implemented using explicit interface member implementation.</span></span> <span data-ttu-id="80ea2-274">Una clase que implementa una abstracción de archivo, por ejemplo, es probable que implementaría una `Close` función miembro que tiene el efecto de liberar el recurso de archivo e implemente el `Dispose` método de la `IDisposable` interfaz explícita de interfaz implementación del miembro:</span><span class="sxs-lookup"><span data-stu-id="80ea2-274">A class implementing a file abstraction, for example, would likely implement a `Close` member function that has the effect of releasing the file resource, and implement the `Dispose` method of the `IDisposable` interface using explicit interface member implementation:</span></span>
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

<span data-ttu-id="80ea2-275">No es posible tener acceso a una implementación de miembro de interfaz explícita a través de su nombre completo en una invocación de método, propiedad o acceso de indizador.</span><span class="sxs-lookup"><span data-stu-id="80ea2-275">It is not possible to access an explicit interface member implementation through its fully qualified name in a method invocation, property access, or indexer access.</span></span> <span data-ttu-id="80ea2-276">Una implementación de miembro de interfaz explícita solo puede obtenerse a través de una instancia de interfaz y se hace referencia en ese caso simplemente por su nombre de miembro.</span><span class="sxs-lookup"><span data-stu-id="80ea2-276">An explicit interface member implementation can only be accessed through an interface instance, and is in that case referenced simply by its member name.</span></span>

<span data-ttu-id="80ea2-277">Es un error en tiempo de compilación para una implementación de miembro de interfaz explícita incluir los modificadores de acceso, y es un error en tiempo de compilación para incluir los modificadores `abstract`, `virtual`, `override`, o `static`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-277">It is a compile-time error for an explicit interface member implementation to include access modifiers, and it is a compile-time error to include the modifiers `abstract`, `virtual`, `override`, or `static`.</span></span>

<span data-ttu-id="80ea2-278">Implementaciones de miembros de interfaz explícitas tienen características de accesibilidad diferente a otros miembros.</span><span class="sxs-lookup"><span data-stu-id="80ea2-278">Explicit interface member implementations have different accessibility characteristics than other members.</span></span> <span data-ttu-id="80ea2-279">Dado que las implementaciones de miembros de interfaz explícita nunca son accesibles a través de su nombre completo en una invocación de método o acceso a una propiedad, son en cierto sentido privadas.</span><span class="sxs-lookup"><span data-stu-id="80ea2-279">Because explicit interface member implementations are never accessible through their fully qualified name in a method invocation or a property access, they are in a sense private.</span></span> <span data-ttu-id="80ea2-280">Sin embargo, puesto que puede tener acceso a través de una instancia de interfaz, están en un sentido públicas.</span><span class="sxs-lookup"><span data-stu-id="80ea2-280">However, since they can be accessed through an interface instance, they are in a sense also public.</span></span>

<span data-ttu-id="80ea2-281">Implementaciones de miembros de interfaz explícitas tienen dos propósitos principales:</span><span class="sxs-lookup"><span data-stu-id="80ea2-281">Explicit interface member implementations serve two primary purposes:</span></span>

*  <span data-ttu-id="80ea2-282">Dado que las implementaciones de miembros de interfaz explícita no son accesibles a través de las instancias de clase o struct, permiten implementaciones de interfaz que se excluirán de la interfaz pública de una clase o struct.</span><span class="sxs-lookup"><span data-stu-id="80ea2-282">Because explicit interface member implementations are not accessible through class or struct instances, they allow interface implementations to be excluded from the public interface of a class or struct.</span></span> <span data-ttu-id="80ea2-283">Esto es especialmente útil cuando una clase o estructura implementa una interfaz interna que no es de interés a un consumidor de esa clase o struct.</span><span class="sxs-lookup"><span data-stu-id="80ea2-283">This is particularly useful when a class or struct implements an internal interface that is of no interest to a consumer of that class or struct.</span></span>
*  <span data-ttu-id="80ea2-284">Implementaciones de miembros de interfaz explícita permiten anulación de ambigüedades de los miembros de interfaz con la misma firma.</span><span class="sxs-lookup"><span data-stu-id="80ea2-284">Explicit interface member implementations allow disambiguation of interface members with the same signature.</span></span> <span data-ttu-id="80ea2-285">Sin implementaciones de miembros de interfaz explícita sería imposible para una clase o struct que diferentes implementaciones de miembros con la misma firma de interfaz y el tipo de valor devuelto, como lo sería imposible que una clase o struct que cualquier implementación en todos los miembros de interfaz con la misma firma pero con diferentes tipos de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="80ea2-285">Without explicit interface member implementations it would be impossible for a class or struct to have different implementations of interface members with the same signature and return type, as would it be impossible for a class or struct to have any implementation at all of interface members with the same signature but with different return types.</span></span>

<span data-ttu-id="80ea2-286">Para que una implementación de miembro de interfaz explícita ser válido, la clase o estructura debe nombrar una interfaz en su lista de clases base que contiene a un miembro cuyo nombre completo, tipo y tipos de parámetros coincidan con los del miembro de interfaz explícita implementación de.</span><span class="sxs-lookup"><span data-stu-id="80ea2-286">For an explicit interface member implementation to be valid, the class or struct must name an interface in its base class list that contains a member whose fully qualified name, type, and parameter types exactly match those of the explicit interface member implementation.</span></span> <span data-ttu-id="80ea2-287">Por lo tanto, en la siguiente clase</span><span class="sxs-lookup"><span data-stu-id="80ea2-287">Thus, in the following class</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
<span data-ttu-id="80ea2-288">la declaración de `IComparable.CompareTo` da como resultado un error en tiempo de compilación porque `IComparable` no aparece en la lista de clases base `Shape` y no es una interfaz base de `ICloneable`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-288">the declaration of `IComparable.CompareTo` results in a compile-time error because `IComparable` is not listed in the base class list of `Shape` and is not a base interface of `ICloneable`.</span></span> <span data-ttu-id="80ea2-289">Del mismo modo, en las declaraciones</span><span class="sxs-lookup"><span data-stu-id="80ea2-289">Likewise, in the declarations</span></span>
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
<span data-ttu-id="80ea2-290">la declaración de `ICloneable.Clone` en `Ellipse` da como resultado un error en tiempo de compilación porque `ICloneable` no aparecen enumerados en la lista de clases base `Ellipse`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-290">the declaration of `ICloneable.Clone` in `Ellipse` results in a compile-time error because `ICloneable` is not explicitly listed in the base class list of `Ellipse`.</span></span>

<span data-ttu-id="80ea2-291">El nombre completo de un miembro de interfaz debe hacer referencia a la interfaz en el que se declara el miembro.</span><span class="sxs-lookup"><span data-stu-id="80ea2-291">The fully qualified name of an interface member must reference the interface in which the member was declared.</span></span> <span data-ttu-id="80ea2-292">Por lo tanto, en las declaraciones</span><span class="sxs-lookup"><span data-stu-id="80ea2-292">Thus, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
<span data-ttu-id="80ea2-293">la implementación de miembro de interfaz explícita de `Paint` debe escribirse como `IControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-293">the explicit interface member implementation of `Paint` must be written as `IControl.Paint`.</span></span>

### <a name="uniqueness-of-implemented-interfaces"></a><span data-ttu-id="80ea2-294">Unicidad de interfaces implementadas</span><span class="sxs-lookup"><span data-stu-id="80ea2-294">Uniqueness of implemented interfaces</span></span>

<span data-ttu-id="80ea2-295">Las interfaces implementadas por una declaración de tipo genérico deben ser únicas para todos los posibles tipos construidos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-295">The interfaces implemented by a generic type declaration must remain unique for all possible constructed types.</span></span> <span data-ttu-id="80ea2-296">Sin esta regla, sería imposible determinar el método correcto para llamar a determinados tipos construidos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-296">Without this rule, it would be impossible to determine the correct method to call for certain constructed types.</span></span> <span data-ttu-id="80ea2-297">Por ejemplo, supongamos que se permite una declaración de clase genérica que escribirse como sigue:</span><span class="sxs-lookup"><span data-stu-id="80ea2-297">For example, suppose a generic class declaration were permitted to be written as follows:</span></span>
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

<span data-ttu-id="80ea2-298">Eran permitiese, sería imposible determinar qué código se ejecute en el caso siguiente:</span><span class="sxs-lookup"><span data-stu-id="80ea2-298">Were this permitted, it would be impossible to determine which code to execute in the following case:</span></span>
```csharp
I<int> x = new X<int,int>();
x.F();
```

<span data-ttu-id="80ea2-299">Para determinar si la lista de interfaces de una declaración de tipo genérico es válida, se realizan los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="80ea2-299">To determine if the interface list of a generic type declaration is valid, the following steps are performed:</span></span>

*  <span data-ttu-id="80ea2-300">Permiten `L` sea la lista de interfaces de especificar directamente en una clase genérica, estructura o declaración de interfaz `C`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-300">Let `L` be the list of interfaces directly specified in a generic class, struct, or interface declaration `C`.</span></span>
*  <span data-ttu-id="80ea2-301">Agregar a `L` cualquier base de las interfaces de las interfaces ya existente en `L`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-301">Add to `L` any base interfaces of the interfaces already in `L`.</span></span>
*  <span data-ttu-id="80ea2-302">Quite los duplicados de `L`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-302">Remove any duplicates from `L`.</span></span>
*  <span data-ttu-id="80ea2-303">Si cualquier posible construido creado a partir del tipo `C` después de que se sustituyen los argumentos de tipo en `L`, hacer que las dos interfaces de `L` que ser idénticos y, a continuación, la declaración de `C` no es válido.</span><span class="sxs-lookup"><span data-stu-id="80ea2-303">If any possible constructed type created from `C` would, after type arguments are substituted into `L`, cause two interfaces in `L` to be identical, then the declaration of `C` is invalid.</span></span> <span data-ttu-id="80ea2-304">Las declaraciones de restricción no se consideran al determinar todos los tipos construidos posibles.</span><span class="sxs-lookup"><span data-stu-id="80ea2-304">Constraint declarations are not considered when determining all possible constructed types.</span></span>

<span data-ttu-id="80ea2-305">En la declaración de clase `X` anteriormente, la lista de interfaces `L` consta de `I<U>` y `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-305">In the class declaration `X` above, the interface list `L` consists of `I<U>` and `I<V>`.</span></span> <span data-ttu-id="80ea2-306">La declaración no es válida porque cualquier tipo con construido `U` y `V` del mismo tipo haría que estas dos interfaces sean tipos idénticos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-306">The declaration is invalid because any constructed type with `U` and `V` being the same type would cause these two interfaces to be identical types.</span></span>

<span data-ttu-id="80ea2-307">Es posible que las interfaces especificadas en los niveles de herencia diferente para unificar:</span><span class="sxs-lookup"><span data-stu-id="80ea2-307">It is possible for interfaces specified at different inheritance levels to unify:</span></span>
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

<span data-ttu-id="80ea2-308">Este código es válido aunque `Derived<U,V>` implementa tanto `I<U>` y `I<V>`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-308">This code is valid even though `Derived<U,V>` implements both `I<U>` and `I<V>`.</span></span> <span data-ttu-id="80ea2-309">El código</span><span class="sxs-lookup"><span data-stu-id="80ea2-309">The code</span></span>
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
<span data-ttu-id="80ea2-310">invoca el método de `Derived`, puesto que `Derived<int,int>` vuelve a implementar eficazmente `I<int>` ([volver a implementar la interfaz](interfaces.md#interface-re-implementation)).</span><span class="sxs-lookup"><span data-stu-id="80ea2-310">invokes the method in `Derived`, since `Derived<int,int>` effectively re-implements `I<int>` ([Interface re-implementation](interfaces.md#interface-re-implementation)).</span></span>

### <a name="implementation-of-generic-methods"></a><span data-ttu-id="80ea2-311">Implementación de métodos genéricos</span><span class="sxs-lookup"><span data-stu-id="80ea2-311">Implementation of generic methods</span></span>

<span data-ttu-id="80ea2-312">Cuando un método genérico implementa de manera implícita un método de interfaz, las restricciones proporcionadas para cada parámetro de tipo de método de donde debe ser equivalente en ambas declaraciones (después de cualquier tipo de interfaz parámetros se reemplazan con los argumentos de tipo adecuado), método parámetros de tipo se identifican mediante posiciones ordinales, de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="80ea2-312">When a generic method implicitly implements an interface method, the constraints given for each method type parameter must be equivalent in both declarations (after any interface type parameters are replaced with the appropriate type arguments), where method type parameters are identified by ordinal positions, left to right.</span></span>

<span data-ttu-id="80ea2-313">Cuando un método genérico implementa explícitamente un método de interfaz, sin embargo, se permiten sin restricciones en el método de implementación.</span><span class="sxs-lookup"><span data-stu-id="80ea2-313">When a generic method explicitly implements an interface method, however, no constraints are allowed on the implementing method.</span></span> <span data-ttu-id="80ea2-314">En su lugar, las restricciones se heredan del método de interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-314">Instead, the constraints are inherited from the interface method</span></span>

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

<span data-ttu-id="80ea2-315">El método `C.F<T>` implementa de manera implícita `I<object,C,string>.F<T>`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-315">The method `C.F<T>` implicitly implements `I<object,C,string>.F<T>`.</span></span> <span data-ttu-id="80ea2-316">En este caso, `C.F<T>` no se requiere (ni permiten) para especificar la restricción `T:object` porque `object` es una restricción en todos los parámetros de tipo implícita.</span><span class="sxs-lookup"><span data-stu-id="80ea2-316">In this case, `C.F<T>` is not required (nor permitted) to specify the constraint `T:object` since `object` is an implicit constraint on all type parameters.</span></span> <span data-ttu-id="80ea2-317">El método `C.G<T>` implementa de manera implícita `I<object,C,string>.G<T>` porque las restricciones coinciden con los de la interfaz, después de los parámetros de tipo de interfaz se reemplazan por los argumentos de tipo correspondiente.</span><span class="sxs-lookup"><span data-stu-id="80ea2-317">The method `C.G<T>` implicitly implements `I<object,C,string>.G<T>` because the constraints match those in the interface, after the interface type parameters are replaced with the corresponding type arguments.</span></span> <span data-ttu-id="80ea2-318">La restricción para el método `C.H<T>` es un error porque los tipos sellados (`string` en este caso) no se puede usar como restricciones.</span><span class="sxs-lookup"><span data-stu-id="80ea2-318">The constraint for method `C.H<T>` is an error because sealed types (`string` in this case) cannot be used as constraints.</span></span> <span data-ttu-id="80ea2-319">Si se omite la restricción también sería un error ya que deben coincidir con las restricciones de las implementaciones de método de interfaz implícita.</span><span class="sxs-lookup"><span data-stu-id="80ea2-319">Omitting the constraint would also be an error since constraints of implicit interface method implementations are required to match.</span></span> <span data-ttu-id="80ea2-320">Por lo tanto, no podrá implementar de manera implícita `I<object,C,string>.H<T>`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-320">Thus, it is impossible to implicitly implement `I<object,C,string>.H<T>`.</span></span> <span data-ttu-id="80ea2-321">Este método de interfaz solo puede implementarse mediante una implementación de miembro de interfaz explícita:</span><span class="sxs-lookup"><span data-stu-id="80ea2-321">This interface method can only be implemented using an explicit interface member implementation:</span></span>
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

<span data-ttu-id="80ea2-322">En este ejemplo, la implementación de miembro de interfaz explícita invoca un método público tener restricciones estrictamente más débiles.</span><span class="sxs-lookup"><span data-stu-id="80ea2-322">In this example, the explicit interface member implementation invokes a public method having strictly weaker constraints.</span></span> <span data-ttu-id="80ea2-323">Tenga en cuenta que la asignación de `t` a `s` es válido desde `T` hereda una restricción de `T:string`, aunque esta restricción no se pueden expresar en código fuente.</span><span class="sxs-lookup"><span data-stu-id="80ea2-323">Note that the assignment from `t` to `s` is valid since `T` inherits a constraint of `T:string`, even though this constraint is not expressible in source code.</span></span>

### <a name="interface-mapping"></a><span data-ttu-id="80ea2-324">Asignación de interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-324">Interface mapping</span></span>

<span data-ttu-id="80ea2-325">Una clase o estructura debe proporcionar implementaciones de todos los miembros de las interfaces que se muestran en la lista de clases base de la clase o struct.</span><span class="sxs-lookup"><span data-stu-id="80ea2-325">A class or struct must provide implementations of all members of the interfaces that are listed in the base class list of the class or struct.</span></span> <span data-ttu-id="80ea2-326">El proceso de buscar las implementaciones de miembros de interfaz en una clase o struct se conoce como ***asignación de interfaz***.</span><span class="sxs-lookup"><span data-stu-id="80ea2-326">The process of locating implementations of interface members in an implementing class or struct is known as ***interface mapping***.</span></span>

<span data-ttu-id="80ea2-327">Asignación de interfaz para una clase o struct `C` localiza una implementación para cada miembro de cada interfaz especificada en la lista de clases base `C`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-327">Interface mapping for a class or struct `C` locates an implementation for each member of each interface specified in the base class list of `C`.</span></span> <span data-ttu-id="80ea2-328">La implementación de un miembro de interfaz determinado `I.M`, donde `I` es la interfaz en la que el miembro `M` se declara, se determina examinando cada clase o struct `S`, comenzando por `C` y repetir para cada clase base sucesiva de `C`, hasta que se encuentra una coincidencia:</span><span class="sxs-lookup"><span data-stu-id="80ea2-328">The implementation of a particular interface member `I.M`, where `I` is the interface in which the member `M` is declared, is determined by examining each class or struct `S`, starting with `C` and repeating for each successive base class of `C`, until a match is located:</span></span>

*  <span data-ttu-id="80ea2-329">Si `S` contiene una declaración de una implementación de miembro de interfaz explícita que coincida con `I` y `M`, a continuación, este miembro es la implementación de `I.M`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-329">If `S` contains a declaration of an explicit interface member implementation that matches `I` and `M`, then this member is the implementation of `I.M`.</span></span>
*  <span data-ttu-id="80ea2-330">De lo contrario, si `S` contiene una declaración de un miembro público no estático que coincide con `M`, a continuación, este miembro es la implementación de `I.M`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-330">Otherwise, if `S` contains a declaration of a non-static public member that matches `M`, then this member is the implementation of `I.M`.</span></span> <span data-ttu-id="80ea2-331">Si más de un miembro de la coincidencia, no se especifica qué miembro es la implementación de `I.M`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-331">If more than one member matches, it is unspecified which member is the implementation of `I.M`.</span></span> <span data-ttu-id="80ea2-332">Esta situación solo puede producirse si `S` es un tipo construido, donde los dos miembros, como se declara en el tipo genérico tienen firmas diferentes, pero los argumentos de tipo que sea idéntico sus firmas.</span><span class="sxs-lookup"><span data-stu-id="80ea2-332">This situation can only occur if `S` is a constructed type where the two members as declared in the generic type have different signatures, but the type arguments make their signatures identical.</span></span>

<span data-ttu-id="80ea2-333">Se produce un error de tiempo de compilación si las implementaciones no se encuentra para todos los miembros de todas las interfaces especificadas en la lista de clases base `C`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-333">A compile-time error occurs if implementations cannot be located for all members of all interfaces specified in the base class list of `C`.</span></span> <span data-ttu-id="80ea2-334">Tenga en cuenta que los miembros de una interfaz incluyen a aquellos miembros que se heredan de las interfaces base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-334">Note that the members of an interface include those members that are inherited from base interfaces.</span></span>

<span data-ttu-id="80ea2-335">Para los fines de asignación de interfaz, un miembro de clase `A` coincide con un miembro de interfaz `B` cuando:</span><span class="sxs-lookup"><span data-stu-id="80ea2-335">For purposes of interface mapping, a class member `A` matches an interface member `B` when:</span></span>

*  <span data-ttu-id="80ea2-336">`A` y `B` son métodos y el nombre, tipo y listas de parámetros formales de `A` y `B` son idénticos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-336">`A` and `B` are methods, and the name, type, and formal parameter lists of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="80ea2-337">`A` y `B` son propiedades, el nombre y tipo de `A` y `B` son idénticos, y `A` tiene el mismos descriptores de acceso como `B` (`A` puede tener los descriptores de acceso adicionales si no es una interfaz explícita implementación de miembro).</span><span class="sxs-lookup"><span data-stu-id="80ea2-337">`A` and `B` are properties, the name and type of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>
*  <span data-ttu-id="80ea2-338">`A` y `B` son eventos y el nombre y tipo de `A` y `B` son idénticos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-338">`A` and `B` are events, and the name and type of `A` and `B` are identical.</span></span>
*  <span data-ttu-id="80ea2-339">`A` y `B` son los indizadores, el tipo y listas de parámetros formales de `A` y `B` son idénticos, y `A` tiene el mismos descriptores de acceso como `B` (`A` puede tener los descriptores de acceso adicionales si no es un implementación de miembro de interfaz explícita).</span><span class="sxs-lookup"><span data-stu-id="80ea2-339">`A` and `B` are indexers, the type and formal parameter lists of `A` and `B` are identical, and `A` has the same accessors as `B` (`A` is permitted to have additional accessors if it is not an explicit interface member implementation).</span></span>

<span data-ttu-id="80ea2-340">Implicaciones importantes del algoritmo de asignación de interfaz son:</span><span class="sxs-lookup"><span data-stu-id="80ea2-340">Notable implications of the interface mapping algorithm are:</span></span>

*  <span data-ttu-id="80ea2-341">Implementaciones de miembros de interfaz explícitos tienen prioridad sobre otros miembros en la misma clase o struct al determinar al miembro de clase o estructura que implementa a un miembro de interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-341">Explicit interface member implementations take precedence over other members in the same class or struct when determining the class or struct member that implements an interface member.</span></span>
*  <span data-ttu-id="80ea2-342">Los miembros no públicos ni estáticos participan en la asignación de interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-342">Neither non-public nor static members participate in interface mapping.</span></span>

<span data-ttu-id="80ea2-343">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-343">In the example</span></span>
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
<span data-ttu-id="80ea2-344">el `ICloneable.Clone` miembro de `C` se convierte en la implementación de `Clone` en `ICloneable` porque las implementaciones de miembros de interfaz explícitos tienen prioridad sobre los demás miembros.</span><span class="sxs-lookup"><span data-stu-id="80ea2-344">the `ICloneable.Clone` member of `C` becomes the implementation of `Clone` in `ICloneable` because explicit interface member implementations take precedence over other members.</span></span>

<span data-ttu-id="80ea2-345">Si una clase o estructura implementa dos o más interfaces que contiene a un miembro con el mismo nombre, tipo y tipos de parámetros, es posible asignar cada uno de esos miembros de interfaz a un único miembro de clase o struct.</span><span class="sxs-lookup"><span data-stu-id="80ea2-345">If a class or struct implements two or more interfaces containing a member with the same name, type, and parameter types, it is possible to map each of those interface members onto a single class or struct member.</span></span> <span data-ttu-id="80ea2-346">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-346">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

<span data-ttu-id="80ea2-347">En este caso, el `Paint` métodos de ambos `IControl` y `IForm` se asignan a la `Paint` método `Page`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-347">Here, the `Paint` methods of both `IControl` and `IForm` are mapped onto the `Paint` method in `Page`.</span></span> <span data-ttu-id="80ea2-348">Por supuesto, también es posible tener implementaciones de miembros de interfaz explícita independiente para los dos métodos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-348">It is of course also possible to have separate explicit interface member implementations for the two methods.</span></span>

<span data-ttu-id="80ea2-349">Si una clase o estructura implementa una interfaz que contiene a los miembros ocultos, necesariamente deben implementarse algunos miembros a través de implementaciones de miembros de interfaz explícita.</span><span class="sxs-lookup"><span data-stu-id="80ea2-349">If a class or struct implements an interface that contains hidden members, then some members must necessarily be implemented through explicit interface member implementations.</span></span> <span data-ttu-id="80ea2-350">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-350">For example</span></span>
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

<span data-ttu-id="80ea2-351">Una implementación de esta interfaz requeriría al menos una implementación de miembro de interfaz explícita y tomaría una de las siguientes formas</span><span class="sxs-lookup"><span data-stu-id="80ea2-351">An implementation of this interface would require at least one explicit interface member implementation, and would take one of the following forms</span></span>
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

<span data-ttu-id="80ea2-352">Cuando una clase implementa varias interfaces que tienen la misma interfaz base, puede haber solo una implementación de la interfaz base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-352">When a class implements multiple interfaces that have the same base interface, there can be only one implementation of the base interface.</span></span> <span data-ttu-id="80ea2-353">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-353">In the example</span></span>
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
<span data-ttu-id="80ea2-354">no es posible tener implementaciones independientes para la `IControl` mencionada en la lista de clases base, el `IControl` heredan `ITextBox`y el `IControl` heredan `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-354">it is not possible to have separate implementations for the `IControl` named in the base class list, the `IControl` inherited by `ITextBox`, and the `IControl` inherited by `IListBox`.</span></span> <span data-ttu-id="80ea2-355">De hecho, no hay ninguna noción de una identidad diferente para estas interfaces.</span><span class="sxs-lookup"><span data-stu-id="80ea2-355">Indeed, there is no notion of a separate identity for these interfaces.</span></span> <span data-ttu-id="80ea2-356">En su lugar, las implementaciones de `ITextBox` y `IListBox` comparten la misma implementación de `IControl`, y `ComboBox` simplemente se considera implementar tres interfaces `IControl`, `ITextBox`, y `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-356">Rather, the implementations of `ITextBox` and `IListBox` share the same implementation of `IControl`, and `ComboBox` is simply considered to implement three interfaces, `IControl`, `ITextBox`, and `IListBox`.</span></span>

<span data-ttu-id="80ea2-357">Los miembros de una clase base participan en la asignación de interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-357">The members of a base class participate in interface mapping.</span></span> <span data-ttu-id="80ea2-358">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-358">In the example</span></span>
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
<span data-ttu-id="80ea2-359">el método `F` en `Class1` se usa en `Class2`de implementación de `Interface1`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-359">the method `F` in `Class1` is used in `Class2`'s implementation of `Interface1`.</span></span>

### <a name="interface-implementation-inheritance"></a><span data-ttu-id="80ea2-360">Herencia de la implementación de interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-360">Interface implementation inheritance</span></span>

<span data-ttu-id="80ea2-361">Una clase hereda todas las implementaciones de interfaz proporcionadas por sus clases base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-361">A class inherits all interface implementations provided by its base classes.</span></span>

<span data-ttu-id="80ea2-362">Sin explícitamente ***volver a implementar*** una interfaz, una clase derivada no puede alterar de ningún modo las asignaciones de interfaz hereda de sus clases base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-362">Without explicitly ***re-implementing*** an interface, a derived class cannot in any way alter the interface mappings it inherits from its base classes.</span></span> <span data-ttu-id="80ea2-363">Por ejemplo, en las declaraciones</span><span class="sxs-lookup"><span data-stu-id="80ea2-363">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
<span data-ttu-id="80ea2-364">el `Paint` método `TextBox` oculta la `Paint` método `Control`, pero no modifica la asignación de `Control.Paint` en `IControl.Paint`y las llamadas a `Paint` a través de la clase de instancias y las instancias de la interfaz le tiene los siguientes efectos</span><span class="sxs-lookup"><span data-stu-id="80ea2-364">the `Paint` method in `TextBox` hides the `Paint` method in `Control`, but it does not alter the mapping of `Control.Paint` onto `IControl.Paint`, and calls to `Paint` through class instances and interface instances will have the following effects</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

<span data-ttu-id="80ea2-365">Sin embargo, cuando un método de interfaz se asigna a un método virtual en una clase, es posible que las clases derivadas reemplazar el método virtual y modificar la implementación de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-365">However, when an interface method is mapped onto a virtual method in a class, it is possible for derived classes to override the virtual method and alter the implementation of the interface.</span></span> <span data-ttu-id="80ea2-366">Por ejemplo, volver a escribir las declaraciones anteriores a</span><span class="sxs-lookup"><span data-stu-id="80ea2-366">For example, rewriting the declarations above to</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
<span data-ttu-id="80ea2-367">Ahora se tendrán en cuenta los siguientes efectos</span><span class="sxs-lookup"><span data-stu-id="80ea2-367">the following effects will now be observed</span></span>
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

<span data-ttu-id="80ea2-368">Puesto que no se pueden declarar implementaciones de interfaz explícita miembros virtuales, no es posible invalidar una implementación de miembro de interfaz explícita.</span><span class="sxs-lookup"><span data-stu-id="80ea2-368">Since explicit interface member implementations cannot be declared virtual, it is not possible to override an explicit interface member implementation.</span></span> <span data-ttu-id="80ea2-369">Sin embargo, es perfectamente válido para una implementación de miembro de interfaz explícita llamar a otro método, y que otro método puede declararse virtual para permitir que las clases derivadas deben invalidarla.</span><span class="sxs-lookup"><span data-stu-id="80ea2-369">However, it is perfectly valid for an explicit interface member implementation to call another method, and that other method can be declared virtual to allow derived classes to override it.</span></span> <span data-ttu-id="80ea2-370">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-370">For example</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

<span data-ttu-id="80ea2-371">En este caso, las clases derivadas de `Control` puede especializar la implementación de `IControl.Paint` invalidando el `PaintControl` método.</span><span class="sxs-lookup"><span data-stu-id="80ea2-371">Here, classes derived from `Control` can specialize the implementation of `IControl.Paint` by overriding the `PaintControl` method.</span></span>

### <a name="interface-re-implementation"></a><span data-ttu-id="80ea2-372">Reimplementación de interfaz</span><span class="sxs-lookup"><span data-stu-id="80ea2-372">Interface re-implementation</span></span>

<span data-ttu-id="80ea2-373">Se permite una clase que hereda de una implementación de interfaz ***volver a implementar*** la interfaz mediante su inclusión en la lista de clases base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-373">A class that inherits an interface implementation is permitted to ***re-implement*** the interface by including it in the base class list.</span></span>

<span data-ttu-id="80ea2-374">Una reimplementación de una interfaz sigue exactamente la misma interfaz reglas de asignación como una implementación inicial de una interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-374">A re-implementation of an interface follows exactly the same interface mapping rules as an initial implementation of an interface.</span></span> <span data-ttu-id="80ea2-375">Por lo tanto, la asignación de interfaz heredada no tiene efecto alguno en la asignación de interfaz establecido para la reimplementación de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="80ea2-375">Thus, the inherited interface mapping has no effect whatsoever on the interface mapping established for the re-implementation of the interface.</span></span> <span data-ttu-id="80ea2-376">Por ejemplo, en las declaraciones</span><span class="sxs-lookup"><span data-stu-id="80ea2-376">For example, in the declarations</span></span>
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
<span data-ttu-id="80ea2-377">el hecho de que `Control` asigna `IControl.Paint` en `Control.IControl.Paint` no afecta a la reimplementación en `MyControl`, que se asigna `IControl.Paint` en `MyControl.Paint`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-377">the fact that `Control` maps `IControl.Paint` onto `Control.IControl.Paint` doesn't affect the re-implementation in `MyControl`, which maps `IControl.Paint` onto `MyControl.Paint`.</span></span>

<span data-ttu-id="80ea2-378">Hereda las declaraciones de miembro público y miembro de interfaz explícita heredada declaraciones participan en el proceso de asignación de interfaz para las interfaces implementadas.</span><span class="sxs-lookup"><span data-stu-id="80ea2-378">Inherited public member declarations and inherited explicit interface member declarations participate in the interface mapping process for re-implemented interfaces.</span></span> <span data-ttu-id="80ea2-379">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-379">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

<span data-ttu-id="80ea2-380">Aquí, la implementación de `IMethods` en `Derived` asigna los métodos de interfaz en `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, y `Base.I`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-380">Here, the implementation of `IMethods` in `Derived` maps the interface methods onto `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, and `Base.I`.</span></span>

<span data-ttu-id="80ea2-381">Cuando una clase implementa una interfaz, implícitamente también implementa todas las interfaces de base de la interfaz de.</span><span class="sxs-lookup"><span data-stu-id="80ea2-381">When a class implements an interface, it implicitly also implements all of that interface's base interfaces.</span></span> <span data-ttu-id="80ea2-382">Del mismo modo, una reimplementación de una interfaz también es implícitamente una reimplementación de todas las interfaces de la interfaz base.</span><span class="sxs-lookup"><span data-stu-id="80ea2-382">Likewise, a re-implementation of an interface is also implicitly a re-implementation of all of the interface's base interfaces.</span></span> <span data-ttu-id="80ea2-383">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-383">For example</span></span>
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

<span data-ttu-id="80ea2-384">Aquí, la reimplementación de `IDerived` también vuelve a implementar `IBase`, la asignación `IBase.F` en `D.F`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-384">Here, the re-implementation of `IDerived` also re-implements `IBase`, mapping `IBase.F` onto `D.F`.</span></span>

### <a name="abstract-classes-and-interfaces"></a><span data-ttu-id="80ea2-385">Interfaces y clases abstractas</span><span class="sxs-lookup"><span data-stu-id="80ea2-385">Abstract classes and interfaces</span></span>

<span data-ttu-id="80ea2-386">Al igual que una clase no abstracta, una clase abstracta debe proporcionar implementaciones de todos los miembros de las interfaces que se muestran en la lista de clases base de la clase.</span><span class="sxs-lookup"><span data-stu-id="80ea2-386">Like a non-abstract class, an abstract class must provide implementations of all members of the interfaces that are listed in the base class list of the class.</span></span> <span data-ttu-id="80ea2-387">Sin embargo, se permite una clase abstracta para asignar los métodos de interfaz a métodos abstractos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-387">However, an abstract class is permitted to map interface methods onto abstract methods.</span></span> <span data-ttu-id="80ea2-388">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-388">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

<span data-ttu-id="80ea2-389">Aquí, la implementación de `IMethods` asigna `F` y `G` a métodos abstractos, que debe invalidarse en clases no abstractas que derivan de `C`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-389">Here, the implementation of `IMethods` maps `F` and `G` onto abstract methods, which must be overridden in non-abstract classes that derive from `C`.</span></span>

<span data-ttu-id="80ea2-390">Tenga en cuenta que las implementaciones de miembros de interfaz explícita no pueden ser abstractas, pero obviamente se permiten las implementaciones de miembros de interfaz explícita para llamar a métodos abstractos.</span><span class="sxs-lookup"><span data-stu-id="80ea2-390">Note that explicit interface member implementations cannot be abstract, but explicit interface member implementations are of course permitted to call abstract methods.</span></span> <span data-ttu-id="80ea2-391">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="80ea2-391">For example</span></span>
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

<span data-ttu-id="80ea2-392">Aquí, las clases no abstractas que derivan de `C` sería necesario para invalidar `FF` y `GG`, lo que proporciona la implementación real de `IMethods`.</span><span class="sxs-lookup"><span data-stu-id="80ea2-392">Here, non-abstract classes that derive from `C` would be required to override `FF` and `GG`, thus providing the actual implementation of `IMethods`.</span></span>
