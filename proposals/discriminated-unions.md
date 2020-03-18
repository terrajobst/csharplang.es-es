---
ms.openlocfilehash: 2e4a3a32def6900797c151264c984378b09b4988
ms.sourcegitcommit: 5983461e05be62f39c77383cb7857539518cb04f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "79484037"
---

# <a name="discriminated-unions--enum-class"></a><span data-ttu-id="efa44-101">Uniones/`enum class` discriminadas</span><span class="sxs-lookup"><span data-stu-id="efa44-101">Discriminated unions / `enum class`</span></span>

<span data-ttu-id="efa44-102">`enum class`es un nuevo tipo de declaración de tipos, que a veces se denomina uniones discriminadas, donde cada instancia posible del tipo se muestra y cada instancia no se superpone.</span><span class="sxs-lookup"><span data-stu-id="efa44-102">`enum class`es are a new kind of type declaration, sometimes referred to as discriminated unions, where each every possible instance the type is listed, and each instance is non-overlapping.</span></span>

<span data-ttu-id="efa44-103">Un `enum class` se define con la sintaxis siguiente:</span><span class="sxs-lookup"><span data-stu-id="efa44-103">An `enum class` is defined using the following syntax:</span></span>

```antlr
enum_class
    : 'partial'? 'enum class' identifier type_parameter_list? type_parameter_constraints_clause* 
      '{' enum_class_body '}'
    ;

enum_class_body
    : enum_class_cases?
    | enum_class_cases ','
    ;

enum_class_cases
    : enum_class_case
    | enum_class_case ',' enum_class_cases
    ;

enum_class_case
    : enum_class
    | class_declaration
    | identifier type_parameter_list? '(' formal_parameter_list? ')'
    | identifier
    ;

```

<span data-ttu-id="efa44-104">Sintaxis de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="efa44-104">Sample syntax:</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius),
}
```

## <a name="semantics"></a><span data-ttu-id="efa44-105">Semántica</span><span class="sxs-lookup"><span data-stu-id="efa44-105">Semantics</span></span>

<span data-ttu-id="efa44-106">Una definición de `enum class` define un tipo raíz, que es una clase abstracta del mismo nombre que la declaración de `enum class`, y un conjunto de miembros, cada uno de los cuales tiene un tipo que es un subtipo del tipo raíz.</span><span class="sxs-lookup"><span data-stu-id="efa44-106">An `enum class` definition defines a root type, which is an abstract class of the same name as the `enum class` declaration, and a set of members, each of which has a type which is a subtype of the root type.</span></span> <span data-ttu-id="efa44-107">Si hay varias definiciones de `partial enum class`, todos los miembros se considerarán miembros de la definición de clase enum.</span><span class="sxs-lookup"><span data-stu-id="efa44-107">If there are multiple `partial enum class` definitions, all members will be considered members of the enum class definition.</span></span> <span data-ttu-id="efa44-108">A diferencia de una definición de clase abstracta definida por el usuario, el tipo de raíz `enum class` es parcial de forma predeterminada y se define para tener un constructor *privado* predeterminado sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="efa44-108">Unlike a user-defined abstract class definition, the `enum class` root type is partial by default and defined to have a default *private* parameter-less constructor.</span></span>

<span data-ttu-id="efa44-109">Tenga en cuenta que, dado que el tipo raíz se define como una clase abstracta parcial, también se pueden agregar definiciones parciales del *tipo raíz* , donde se permiten los formatos de sintaxis estándar para un cuerpo de clase.</span><span class="sxs-lookup"><span data-stu-id="efa44-109">Note that, since the root type is defined to be a partial abstract class, partial definitions of the *root type* may also be added, where standard syntax forms for a class body are allowed.</span></span>
<span data-ttu-id="efa44-110">Sin embargo, ningún tipo puede heredar directamente del tipo raíz en cualquier declaración, aparte de los especificados como miembros de `enum class`.</span><span class="sxs-lookup"><span data-stu-id="efa44-110">However, no types may directly inherit from the root type in any declaration, aside from those specified as `enum class` members.</span></span> <span data-ttu-id="efa44-111">Además, no se permiten constructores definidos por el usuario para el tipo raíz.</span><span class="sxs-lookup"><span data-stu-id="efa44-111">In addition, no user-defined constructors are permitted for the root type.</span></span>

<span data-ttu-id="efa44-112">Hay cuatro tipos de declaraciones de miembros de `enum class`:</span><span class="sxs-lookup"><span data-stu-id="efa44-112">There are four kinds of `enum class` member declarations:</span></span>

* <span data-ttu-id="efa44-113">Miembros de clase simples</span><span class="sxs-lookup"><span data-stu-id="efa44-113">simple class members</span></span>

* <span data-ttu-id="efa44-114">miembros de clase complejos</span><span class="sxs-lookup"><span data-stu-id="efa44-114">complex class members</span></span>

* <span data-ttu-id="efa44-115">miembros de `enum class`</span><span class="sxs-lookup"><span data-stu-id="efa44-115">`enum class` members</span></span>

* <span data-ttu-id="efa44-116">miembros de valor.</span><span class="sxs-lookup"><span data-stu-id="efa44-116">value members.</span></span>

### <a name="simple-class-members"></a><span data-ttu-id="efa44-117">Miembros de clase simples</span><span class="sxs-lookup"><span data-stu-id="efa44-117">Simple class members</span></span>

<span data-ttu-id="efa44-118">Una declaración de miembro de clase simple define una nueva clase de "registro" anidada (no definida intencionadamente en este documento) con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="efa44-118">A simple class member declaration defines a new nested "record" class (intentionally left undefined in this document) with the same name.</span></span> <span data-ttu-id="efa44-119">La clase anidada hereda del tipo raíz.</span><span class="sxs-lookup"><span data-stu-id="efa44-119">The nested class inherits from the root type.</span></span>

<span data-ttu-id="efa44-120">Dado el código de ejemplo anterior,</span><span class="sxs-lookup"><span data-stu-id="efa44-120">Given the sample code above,</span></span>

```C#
enum class Shape
{
    Rectangle(float Width, float Length),
    Circle(float Radius)
}
```

<span data-ttu-id="efa44-121">la declaración de `enum class` tiene una semántica equivalente a la siguiente declaración</span><span class="sxs-lookup"><span data-stu-id="efa44-121">the `enum class` declaration has semantics equivalent to the following declaration</span></span>

```C#
partial abstract class Shape
{
    public data class Rectangle(float Width, float Length) : Shape,
    public data class Circle(float Radius) : Shape
}
```

### <a name="complex-class-members"></a><span data-ttu-id="efa44-122">miembros de clase complejos</span><span class="sxs-lookup"><span data-stu-id="efa44-122">Complex class members</span></span>

<span data-ttu-id="efa44-123">También puede anidar una declaración de clase completa debajo de una declaración de `enum class`.</span><span class="sxs-lookup"><span data-stu-id="efa44-123">You can also nest an entire class declaration below an `enum class` declaration.</span></span> <span data-ttu-id="efa44-124">Se tratará como una clase anidada del tipo raíz.</span><span class="sxs-lookup"><span data-stu-id="efa44-124">It will be treated as a nested class of the root type.</span></span> <span data-ttu-id="efa44-125">La sintaxis permite cualquier declaración de clase, pero es necesaria para que el miembro de clase compleja herede de la declaración de `enum class` contenedora directa.</span><span class="sxs-lookup"><span data-stu-id="efa44-125">The syntax allows any class declaration, but it is required for the complex class member to inherit from the direct containing `enum class` declaration.</span></span> 

### <a name="enum-class-members"></a><span data-ttu-id="efa44-126">miembros de `enum class`</span><span class="sxs-lookup"><span data-stu-id="efa44-126">`enum class` members</span></span>

<span data-ttu-id="efa44-127">`enum classes` se pueden anidar entre sí, por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="efa44-127">`enum classes` can be nested under each other, e.g.</span></span>

```C#
enum class Expr
{
    enum class Binary
    {
        Addition(Expr left, Expr right),
        Multiplication(Expr left, Expr right)
    }
}
```

<span data-ttu-id="efa44-128">Esto es casi idéntico a la semántica de un `enum class`de nivel superior, salvo que la clase de enumeración anidada define un tipo de raíz anidada y todo lo que está debajo de la clase de enumeración anidada es un subtipo del tipo de raíz anidado, en lugar del tipo raíz de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="efa44-128">This is almost identical to the semantics of a top-level `enum class`, except that the nested enum class defines a nested root type, and everything below the nested enum class is a subtype of the nested root type, instead of the top-level root type.</span></span>

```C#
partial abstract class Expr
{
    partial abstract class Binary : Expr
    {
        public data class Addition(Expr left, Expr right) : Binary,
        public data class Multiplication(Expr left, Expr right) : Binary
    }
}
```

### <a name="value-members"></a><span data-ttu-id="efa44-129">Miembros de valor</span><span class="sxs-lookup"><span data-stu-id="efa44-129">Value members</span></span>

<span data-ttu-id="efa44-130">`enum classes` también puede contener miembros de valor.</span><span class="sxs-lookup"><span data-stu-id="efa44-130">`enum classes` can also contain value members.</span></span> <span data-ttu-id="efa44-131">Los miembros de valor definen propiedades estáticas Get-Only públicas en el tipo raíz que también devuelven el tipo raíz, por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="efa44-131">Value members define public get-only static properties on the root type that also return the root type, e.g.</span></span>

```C#
enum class Color
{
    Red,
    Green
}
```

<span data-ttu-id="efa44-132">tiene propiedades equivalentes a</span><span class="sxs-lookup"><span data-stu-id="efa44-132">has properties equivalent to</span></span>

```C#
partial abstract class Color
{
    public static Color Red => ...;
    public static Color Green => ...;
}
```

<span data-ttu-id="efa44-133">La semántica completa se considera un detalle de implementación, pero se garantiza que se devolverá una instancia única para cada propiedad, y se devolverá la misma instancia en las invocaciones repetidas.</span><span class="sxs-lookup"><span data-stu-id="efa44-133">The complete semantics are considered an implementation detail, but it is guaranteed that one unique instance will be returned for each property, and the same instance will be returned on repeated invocations.</span></span>


### <a name="switch-expression-and-patterns"></a><span data-ttu-id="efa44-134">Cambiar expresión y patrones</span><span class="sxs-lookup"><span data-stu-id="efa44-134">Switch expression and patterns</span></span>

<span data-ttu-id="efa44-135">Hay algunos ajustes propuestos para la coincidencia de patrones y la expresión switch para controlar `enum classes`.</span><span class="sxs-lookup"><span data-stu-id="efa44-135">There are some proposed adjustments to pattern matching and the switch expression to handle `enum classes`.</span></span> <span data-ttu-id="efa44-136">Las expresiones switch ya pueden coincidir con tipos a través del patrón variable, pero para actualmente para tipos de referencia, no se considera que ningún conjunto de brazos de conmutador de la expresión switch está completo, salvo para la coincidencia con el tipo estático del argumento o un subtipo.</span><span class="sxs-lookup"><span data-stu-id="efa44-136">Switch expressions can already match types through the variable pattern, but for currently for reference types, no set of switch arms in the switch expression are considered complete, except for matching against the static type of the argument, or a subtype.</span></span>

<span data-ttu-id="efa44-137">Las expresiones switch se cambiarán de modo que, si el tipo raíz de un `enum class` es el tipo estático del argumento de la expresión switch, y hay un conjunto de patrones que coinciden con todos los miembros de la enumeración, el modificador se considerará exhaustivo.</span><span class="sxs-lookup"><span data-stu-id="efa44-137">Switch expressions would be changed such that, if the root type of an `enum class` is the static type of the argument to the switch expression, and there is a set of patterns matching all members of the enum, then the switch will be considered exhaustive.</span></span>

<span data-ttu-id="efa44-138">Dado que los miembros de valor no son constantes y no definen nuevos tipos estáticos, actualmente no pueden coincidir con el patrón.</span><span class="sxs-lookup"><span data-stu-id="efa44-138">Since value members are not constants and do not define new static types, they currently cannot be matched by pattern.</span></span> <span data-ttu-id="efa44-139">Para que esto sea posible, se agregará un nuevo patrón con la sintaxis de patrón de constante para permitir la coincidencia con `enum class` miembros de valor.</span><span class="sxs-lookup"><span data-stu-id="efa44-139">To make this possible, a new pattern using the constant pattern syntax will be added to allow match against `enum class` value members.</span></span> <span data-ttu-id="efa44-140">La coincidencia se define para que se realice correctamente si y solo si el argumento de la coincidencia de patrón y el valor devuelto por el miembro de valor de `enum class` serían iguales a la referencia, aunque la implementación no es necesaria para realizar esta comprobación.</span><span class="sxs-lookup"><span data-stu-id="efa44-140">The match is defined to succeed if and only if the argument to the pattern match and the value returned by the `enum class` value member would be reference equal, although the implementation is not required to perform this check.</span></span>


## <a name="open-questions"></a><span data-ttu-id="efa44-141">Preguntas abiertas</span><span class="sxs-lookup"><span data-stu-id="efa44-141">Open questions</span></span>

- <span data-ttu-id="efa44-142">[] ¿Qué indica el algoritmo de tipo común sobre `enum class` miembros?</span><span class="sxs-lookup"><span data-stu-id="efa44-142">[ ] What does the common type algorithm say about `enum class` members?</span></span> <span data-ttu-id="efa44-143">¿Es este código válido?</span><span class="sxs-lookup"><span data-stu-id="efa44-143">Is this valid code?</span></span>
    * `var x = b ? new Shape.Rectangle(...) : new Shape.Circle(...)`

- <span data-ttu-id="efa44-144">[] Agregar un nuevo patrón solo para los miembros de valor parece pesado.</span><span class="sxs-lookup"><span data-stu-id="efa44-144">[ ] Adding a new pattern just for value members seems heavy handed.</span></span> <span data-ttu-id="efa44-145">¿Hay una construcción de versión más general que tenga sentido?</span><span class="sxs-lookup"><span data-stu-id="efa44-145">Is there a more general version construction that makes sense?</span></span>
    - <span data-ttu-id="efa44-146">[] Los miembros de valor tampoco se asignan correctamente a una construcción de clase anidada paralela debido a esto</span><span class="sxs-lookup"><span data-stu-id="efa44-146">[ ] Value members also do not map well to a parallel nested class construction because of this</span></span>

- <span data-ttu-id="efa44-147">[] Se está cambiando a un argumento con un tipo estático `enum class` que se garantiza que sea constante.</span><span class="sxs-lookup"><span data-stu-id="efa44-147">[ ] Is switching against an argument with an `enum class` static type guaranteed to be constant-time?</span></span>

- <span data-ttu-id="efa44-148">[] ¿Hay una manera de hacer que `enum class`no se consideren completos en la expresión switch?</span><span class="sxs-lookup"><span data-stu-id="efa44-148">[ ] Should there be a way to make `enum class`es not be considered complete in the switch expression?</span></span> <span data-ttu-id="efa44-149">¿Tiene `virtual`?</span><span class="sxs-lookup"><span data-stu-id="efa44-149">Prefix with `virtual`?</span></span>

- <span data-ttu-id="efa44-150">[] ¿Qué modificadores se deben permitir en `enum class`?</span><span class="sxs-lookup"><span data-stu-id="efa44-150">[ ] What modifiers should be permitted on `enum class`?</span></span>