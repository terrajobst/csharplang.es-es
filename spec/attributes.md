---
ms.openlocfilehash: a8ad8a8b3eda1d00fa745bd92e4371eacc36b79f
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229739"
---
# <a name="attributes"></a><span data-ttu-id="0f896-101">Atributos</span><span class="sxs-lookup"><span data-stu-id="0f896-101">Attributes</span></span>

<span data-ttu-id="0f896-102">Gran parte del lenguaje C# permite al programador especificar información declarativa sobre las entidades definidas en el programa.</span><span class="sxs-lookup"><span data-stu-id="0f896-102">Much of the C# language enables the programmer to specify declarative information about the entities defined in the program.</span></span> <span data-ttu-id="0f896-103">Por ejemplo, la accesibilidad de un método en una clase se especifica al decorarlo con el *method_modifier*s `public`, `protected`, `internal`, y `private`.</span><span class="sxs-lookup"><span data-stu-id="0f896-103">For example, the accessibility of a method in a class is specified by decorating it with the *method_modifier*s `public`, `protected`, `internal`, and `private`.</span></span>

<span data-ttu-id="0f896-104">C# permite a los programadores inventar nuevas clases de información declarativa, denominadas ***atributos***.</span><span class="sxs-lookup"><span data-stu-id="0f896-104">C# enables programmers to invent new kinds of declarative information, called ***attributes***.</span></span> <span data-ttu-id="0f896-105">Los programadores, a continuación, pueden asociar los atributos a varias entidades de programa y recuperar información de atributos en un entorno de tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="0f896-105">Programmers can then attach attributes to various program entities, and retrieve attribute information in a run-time environment.</span></span> <span data-ttu-id="0f896-106">Por ejemplo, podría definir un marco de trabajo un `HelpAttribute` atributo que se puede colocar en determinados elementos de programa (por ejemplo, clases y métodos) para proporcionar una asignación de los elementos de programa a su documentación.</span><span class="sxs-lookup"><span data-stu-id="0f896-106">For instance, a framework might define a `HelpAttribute` attribute that can be placed on certain program elements (such as classes and methods) to provide a mapping from those program elements to their documentation.</span></span>

<span data-ttu-id="0f896-107">Los atributos se definen mediante la declaración de clases de atributos ([clases de atributo](attributes.md#attribute-classes)), que puede tener parámetros posicionales y con nombre ([posicional y parámetros con nombre](attributes.md#positional-and-named-parameters)).</span><span class="sxs-lookup"><span data-stu-id="0f896-107">Attributes are defined through the declaration of attribute classes ([Attribute classes](attributes.md#attribute-classes)), which may have positional and named parameters ([Positional and named parameters](attributes.md#positional-and-named-parameters)).</span></span> <span data-ttu-id="0f896-108">Los atributos están asociados a las entidades de un programa de C# mediante las especificaciones de atributo ([especificación de atributos](attributes.md#attribute-specification)) y se pueden recuperar en tiempo de ejecución como instancias de atributo ([instancias de atributos](attributes.md#attribute-instances)).</span><span class="sxs-lookup"><span data-stu-id="0f896-108">Attributes are attached to entities in a C# program using attribute specifications ([Attribute specification](attributes.md#attribute-specification)), and can be retrieved at run-time as attribute instances ([Attribute instances](attributes.md#attribute-instances)).</span></span>

## <a name="attribute-classes"></a><span data-ttu-id="0f896-109">Clases de atributos</span><span class="sxs-lookup"><span data-stu-id="0f896-109">Attribute classes</span></span>

<span data-ttu-id="0f896-110">Una clase que derive de la clase abstracta `System.Attribute`, ya sea directa o indirectamente, es un ***clase de atributos***.</span><span class="sxs-lookup"><span data-stu-id="0f896-110">A class that derives from the abstract class `System.Attribute`, whether directly or indirectly, is an ***attribute class***.</span></span> <span data-ttu-id="0f896-111">La declaración de una clase de atributo define un nuevo tipo de ***atributo*** que puede colocarse en una declaración.</span><span class="sxs-lookup"><span data-stu-id="0f896-111">The declaration of an attribute class defines a new kind of ***attribute*** that can be placed on a declaration.</span></span> <span data-ttu-id="0f896-112">Por convención, las clases de atributos se denominan con el sufijo `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="0f896-112">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="0f896-113">Usos de un atributo pueden incluir u omitir este sufijo.</span><span class="sxs-lookup"><span data-stu-id="0f896-113">Uses of an attribute may either include or omit this suffix.</span></span>

### <a name="attribute-usage"></a><span data-ttu-id="0f896-114">Uso de atributos</span><span class="sxs-lookup"><span data-stu-id="0f896-114">Attribute usage</span></span>

<span data-ttu-id="0f896-115">El atributo `AttributeUsage` ([atributo AttributeUsage The](attributes.md#the-attributeusage-attribute)) se usa para describir cómo se puede usar una clase de atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-115">The attribute `AttributeUsage` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)) is used to describe how an attribute class can be used.</span></span>

<span data-ttu-id="0f896-116">`AttributeUsage` tiene un parámetro posicional ([posicional y parámetros con nombre](attributes.md#positional-and-named-parameters)) que permite que una clase de atributo especificar los tipos de declaraciones en el que se puede usar.</span><span class="sxs-lookup"><span data-stu-id="0f896-116">`AttributeUsage` has a positional parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) that enables an attribute class to specify the kinds of declarations on which it can be used.</span></span> <span data-ttu-id="0f896-117">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-117">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

<span data-ttu-id="0f896-118">define una clase de atributo denominada `SimpleAttribute` que puede colocarse en *class_declaration*s y *interface_declaration*s solo.</span><span class="sxs-lookup"><span data-stu-id="0f896-118">defines an attribute class named `SimpleAttribute` that can be placed on *class_declaration*s and *interface_declaration*s only.</span></span> <span data-ttu-id="0f896-119">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-119">The example</span></span>

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

<span data-ttu-id="0f896-120">se muestran varios usos de la `Simple` atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-120">shows several uses of the `Simple` attribute.</span></span> <span data-ttu-id="0f896-121">Aunque este atributo se define con el nombre `SimpleAttribute`, cuando se utiliza este atributo, el `Attribute` sufijo puede omitirse, lo que resulta en el nombre corto `Simple`.</span><span class="sxs-lookup"><span data-stu-id="0f896-121">Although this attribute is defined with the name `SimpleAttribute`, when this attribute is used, the `Attribute` suffix may be omitted, resulting in the short name `Simple`.</span></span> <span data-ttu-id="0f896-122">Por lo tanto, el ejemplo anterior es semánticamente equivalente a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="0f896-122">Thus, the example above is semantically equivalent to the following:</span></span>

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

<span data-ttu-id="0f896-123">`AttributeUsage` tiene un parámetro con nombre ([posicional y parámetros con nombre](attributes.md#positional-and-named-parameters)) llamado `AllowMultiple`, lo que indica si el atributo se puede especificar más de una vez para una entidad determinada.</span><span class="sxs-lookup"><span data-stu-id="0f896-123">`AttributeUsage` has a named parameter ([Positional and named parameters](attributes.md#positional-and-named-parameters)) called `AllowMultiple`, which indicates whether the attribute can be specified more than once for a given entity.</span></span> <span data-ttu-id="0f896-124">Si `AllowMultiple` para un atributo de clase es true, entonces esa clase de atributo es un ***clase de atributos multiuso***y se puede especificar más de una vez en una entidad.</span><span class="sxs-lookup"><span data-stu-id="0f896-124">If `AllowMultiple` for an attribute class is true, then that attribute class is a ***multi-use attribute class***, and can be specified more than once on an entity.</span></span> <span data-ttu-id="0f896-125">Si `AllowMultiple` para un atributo de clase es false o no se especifica y, a continuación, esa clase de atributo es un ***clase de atributo de uso único***y se puede especificar como máximo una vez en una entidad.</span><span class="sxs-lookup"><span data-stu-id="0f896-125">If `AllowMultiple` for an attribute class is false or it is unspecified, then that attribute class is a ***single-use attribute class***, and can be specified at most once on an entity.</span></span>

<span data-ttu-id="0f896-126">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-126">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

<span data-ttu-id="0f896-127">define una clase de atributo de uso múltiple denominada `AuthorAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0f896-127">defines a multi-use attribute class named `AuthorAttribute`.</span></span> <span data-ttu-id="0f896-128">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-128">The example</span></span>

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

<span data-ttu-id="0f896-129">muestra una declaración de clase con dos usos de la `Author` atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-129">shows a class declaration with two uses of the `Author` attribute.</span></span>

<span data-ttu-id="0f896-130">`AttributeUsage` tiene otro parámetro con nombre denominado `Inherited`, lo que indica si el atributo, cuando se especifican en una clase base, también es heredado por las clases que derivan de esa clase base.</span><span class="sxs-lookup"><span data-stu-id="0f896-130">`AttributeUsage` has another named parameter called `Inherited`, which indicates whether the attribute, when specified on a base class, is also inherited by classes that derive from that base class.</span></span> <span data-ttu-id="0f896-131">Si `Inherited` para un atributo de clase es true, entonces ese atributo es heredado.</span><span class="sxs-lookup"><span data-stu-id="0f896-131">If `Inherited` for an attribute class is true, then that attribute is inherited.</span></span> <span data-ttu-id="0f896-132">Si `Inherited` para un atributo de clase es false, no se hereda ese atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-132">If `Inherited` for an attribute class is false then that attribute is not inherited.</span></span> <span data-ttu-id="0f896-133">Si no se especifica, su valor predeterminado es true.</span><span class="sxs-lookup"><span data-stu-id="0f896-133">If it is unspecified, its default value is true.</span></span>

<span data-ttu-id="0f896-134">Una clase de atributo `X` no tener un `AttributeUsage` atributo asociado a él, como en</span><span class="sxs-lookup"><span data-stu-id="0f896-134">An attribute class `X` not having an `AttributeUsage` attribute attached to it, as in</span></span>

```csharp
using System;

class X: Attribute {...}
```

<span data-ttu-id="0f896-135">es equivalente a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="0f896-135">is equivalent to the following:</span></span>

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a><span data-ttu-id="0f896-136">Parámetros posicionales y con nombre</span><span class="sxs-lookup"><span data-stu-id="0f896-136">Positional and named parameters</span></span>

<span data-ttu-id="0f896-137">Las clases de atributo pueden tener ***parámetros posicionales*** y ***parámetros con nombre***.</span><span class="sxs-lookup"><span data-stu-id="0f896-137">Attribute classes can have ***positional parameters*** and ***named parameters***.</span></span> <span data-ttu-id="0f896-138">Cada constructor de instancia pública de una clase de atributo define una secuencia válida de parámetros posicionales para esa clase de atributos.</span><span class="sxs-lookup"><span data-stu-id="0f896-138">Each public instance constructor for an attribute class defines a valid sequence of positional parameters for that attribute class.</span></span> <span data-ttu-id="0f896-139">Cada campo de lectura y escritura público no estático y la propiedad para una clase de atributo definen un parámetro con nombre para la clase de atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-139">Each non-static public read-write field and property for an attribute class defines a named parameter for the attribute class.</span></span>

<span data-ttu-id="0f896-140">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-140">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

<span data-ttu-id="0f896-141">define una clase de atributo denominada `HelpAttribute` que tiene un parámetro posicional, `url`y un parámetro con nombre, `Topic`.</span><span class="sxs-lookup"><span data-stu-id="0f896-141">defines an attribute class named `HelpAttribute` that has one positional parameter, `url`, and one named parameter, `Topic`.</span></span> <span data-ttu-id="0f896-142">Aunque es no estático y público, la propiedad `Url` no define un parámetro con nombre, ya que no es de lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="0f896-142">Although it is non-static and public, the property `Url` does not define a named parameter, since it is not read-write.</span></span>

<span data-ttu-id="0f896-143">Esta clase de atributo se puede usar como sigue:</span><span class="sxs-lookup"><span data-stu-id="0f896-143">This attribute class might be used as follows:</span></span>

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a><span data-ttu-id="0f896-144">Tipos de parámetro de atributo</span><span class="sxs-lookup"><span data-stu-id="0f896-144">Attribute parameter types</span></span>

<span data-ttu-id="0f896-145">Los tipos de parámetros con nombre y posicionales para una clase de atributo se limitan a la ***tipos de parámetro de atributo***, que son:</span><span class="sxs-lookup"><span data-stu-id="0f896-145">The types of positional and named parameters for an attribute class are limited to the ***attribute parameter types***, which are:</span></span>

*  <span data-ttu-id="0f896-146">Uno de los siguientes tipos: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span><span class="sxs-lookup"><span data-stu-id="0f896-146">One of the following types: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.</span></span>
*  <span data-ttu-id="0f896-147">El tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="0f896-147">The type `object`.</span></span>
*  <span data-ttu-id="0f896-148">El tipo `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="0f896-148">The type `System.Type`.</span></span>
*  <span data-ttu-id="0f896-149">Proporcionó un tipo de enumeración, tiene accesibilidad pública y los tipos en la que está anidado (si existe) también tienen accesibilidad pública ([especificación de atributos](attributes.md#attribute-specification)).</span><span class="sxs-lookup"><span data-stu-id="0f896-149">An enum type, provided it has public accessibility and the types in which it is nested (if any) also have public accessibility ([Attribute specification](attributes.md#attribute-specification)).</span></span>
*  <span data-ttu-id="0f896-150">Matrices unidimensionales de los tipos anteriores.</span><span class="sxs-lookup"><span data-stu-id="0f896-150">Single-dimensional arrays of the above types.</span></span>
*  <span data-ttu-id="0f896-151">Un argumento de constructor o un campo público que no tiene uno de estos tipos, no se puede usar como un parámetro posicional o con nombre en una especificación de atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-151">A constructor argument or public field which does not have one of these types, cannot be used as a positional or named parameter in an attribute specification.</span></span>

## <a name="attribute-specification"></a><span data-ttu-id="0f896-152">Especificación de atributo</span><span class="sxs-lookup"><span data-stu-id="0f896-152">Attribute specification</span></span>

<span data-ttu-id="0f896-153">***Especificación de atributos*** es la aplicación de un atributo definido previamente en una declaración.</span><span class="sxs-lookup"><span data-stu-id="0f896-153">***Attribute specification*** is the application of a previously defined attribute to a declaration.</span></span> <span data-ttu-id="0f896-154">Un atributo es un fragmento de información declarativa adicional que se especifica para una declaración.</span><span class="sxs-lookup"><span data-stu-id="0f896-154">An attribute is a piece of additional declarative information that is specified for a declaration.</span></span> <span data-ttu-id="0f896-155">Atributos que se pueden especificar en el ámbito global (para especificar atributos en el ensamblado o módulo que lo contiene) y para *type_declaration*s ([declaraciones de tipo](namespaces.md#type-declarations)), *class_member_declaration* s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([miembros de interfaz](interfaces.md#interface-members)), *struct_member _declaration*s ([los miembros de Struct](structs.md#struct-members)), *enum_member_declaration*s ([miembros de enumeración](enums.md#enum-members)), *accessor_declarations*  ([Descriptores de acceso](classes.md#accessors)), *event_accessor_declarations* ([eventos como campos](classes.md#field-like-events)), y *formal_parameter_list*s ([parámetros del método](classes.md#method-parameters)).</span><span class="sxs-lookup"><span data-stu-id="0f896-155">Attributes can be specified at global scope (to specify attributes on the containing assembly or module) and for *type_declaration*s ([Type declarations](namespaces.md#type-declarations)), *class_member_declaration*s ([Type parameter constraints](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([Interface members](interfaces.md#interface-members)), *struct_member_declaration*s ([Struct members](structs.md#struct-members)), *enum_member_declaration*s ([Enum members](enums.md#enum-members)), *accessor_declarations* ([Accessors](classes.md#accessors)), *event_accessor_declarations* ([Field-like events](classes.md#field-like-events)), and *formal_parameter_list*s ([Method parameters](classes.md#method-parameters)).</span></span>

<span data-ttu-id="0f896-156">Los atributos se especifican en ***atributo secciones***.</span><span class="sxs-lookup"><span data-stu-id="0f896-156">Attributes are specified in ***attribute sections***.</span></span> <span data-ttu-id="0f896-157">Una sección de atributos consta de un par de corchetes que encierran una lista separada por comas de uno o varios atributos.</span><span class="sxs-lookup"><span data-stu-id="0f896-157">An attribute section consists of a pair of square brackets, which surround a comma-separated list of one or more attributes.</span></span> <span data-ttu-id="0f896-158">El orden en el que se especifican los atributos en la lista y se organizan el orden de las secciones adjuntas a la misma entidad de programa, no es significativo.</span><span class="sxs-lookup"><span data-stu-id="0f896-158">The order in which attributes are specified in such a list, and the order in which sections attached to the same program entity are arranged, is not significant.</span></span> <span data-ttu-id="0f896-159">Por ejemplo, las especificaciones del atributo `[A][B]`, `[B][A]`, `[A,B]`, y `[B,A]` son equivalentes.</span><span class="sxs-lookup"><span data-stu-id="0f896-159">For instance, the attribute specifications `[A][B]`, `[B][A]`, `[A,B]`, and `[B,A]` are equivalent.</span></span>

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

<span data-ttu-id="0f896-160">Un atributo consta de un *attribute_name* y una lista opcional de argumentos posicionales y con nombre.</span><span class="sxs-lookup"><span data-stu-id="0f896-160">An attribute consists of an *attribute_name* and an optional list of positional and named arguments.</span></span> <span data-ttu-id="0f896-161">Los argumentos posicionales (si existe) preceden a los argumentos con nombre.</span><span class="sxs-lookup"><span data-stu-id="0f896-161">The positional arguments (if any) precede the named arguments.</span></span> <span data-ttu-id="0f896-162">Un argumento posicional se compone de un *attribute_argument_expression*; un argumento con nombre consta de un nombre, seguido por un signo igual, seguido por un *attribute_argument_expression*, que, juntos , están restringidas por las mismas reglas de asignación simple.</span><span class="sxs-lookup"><span data-stu-id="0f896-162">A positional argument consists of an *attribute_argument_expression*; a named argument consists of a name, followed by an equal sign, followed by an *attribute_argument_expression*, which, together, are constrained by the same rules as simple assignment.</span></span> <span data-ttu-id="0f896-163">El orden de los argumentos con nombre no es significativo.</span><span class="sxs-lookup"><span data-stu-id="0f896-163">The order of named arguments is not significant.</span></span>

<span data-ttu-id="0f896-164">El *attribute_name* identifica una clase de atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-164">The *attribute_name* identifies an attribute class.</span></span> <span data-ttu-id="0f896-165">Si el formulario de *attribute_name* es *type_name* , a continuación, este nombre debe hacer referencia a una clase de atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-165">If the form of *attribute_name* is *type_name* then this name must refer to an attribute class.</span></span> <span data-ttu-id="0f896-166">De lo contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0f896-166">Otherwise, a compile-time error occurs.</span></span> <span data-ttu-id="0f896-167">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-167">The example</span></span>

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

<span data-ttu-id="0f896-168">da como resultado un error en tiempo de compilación porque intenta utilizar `Class1` como un atributo de clase cuando `Class1` no es una clase de atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-168">results in a compile-time error because it attempts to use `Class1` as an attribute class when `Class1` is not an attribute class.</span></span>

<span data-ttu-id="0f896-169">Algunos contextos permiten la especificación de un atributo en más de un destino.</span><span class="sxs-lookup"><span data-stu-id="0f896-169">Certain contexts permit the specification of an attribute on more than one target.</span></span> <span data-ttu-id="0f896-170">Un programa puede especificar explícitamente el destino mediante la inclusión de un *attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="0f896-170">A program can explicitly specify the target by including an *attribute_target_specifier*.</span></span> <span data-ttu-id="0f896-171">Cuando se coloca un atributo en el nivel global, un *global_attribute_target_specifier* es necesario.</span><span class="sxs-lookup"><span data-stu-id="0f896-171">When an attribute is placed at the global level, a *global_attribute_target_specifier* is required.</span></span> <span data-ttu-id="0f896-172">En todas las demás ubicaciones, se aplica un valor predeterminado razonable, pero un *attribute_target_specifier* puede usarse para afirmar o reemplazar el valor predeterminado en ciertos casos ambiguos (o sólo para afirmar el valor predeterminado en los casos que no sean ambiguas).</span><span class="sxs-lookup"><span data-stu-id="0f896-172">In all other locations, a reasonable default is applied, but an *attribute_target_specifier* can be used to affirm or override the default in certain ambiguous cases (or to just affirm the default in non-ambiguous cases).</span></span> <span data-ttu-id="0f896-173">Por lo tanto, normalmente, *attribute_target_specifier*s puede omitirse, excepto en el nivel global.</span><span class="sxs-lookup"><span data-stu-id="0f896-173">Thus, typically, *attribute_target_specifier*s can be omitted except at the global level.</span></span> <span data-ttu-id="0f896-174">Los contextos potencialmente ambiguos se resuelven como sigue:</span><span class="sxs-lookup"><span data-stu-id="0f896-174">The potentially ambiguous contexts are resolved as follows:</span></span>

*  <span data-ttu-id="0f896-175">Puede aplicar un atributo especificado en el ámbito global en el ensamblado de destino o en el módulo de destino.</span><span class="sxs-lookup"><span data-stu-id="0f896-175">An attribute specified at global scope can apply either to the target assembly or the target module.</span></span> <span data-ttu-id="0f896-176">Existe ningún valor predeterminado para este contexto, por lo tanto un *attribute_target_specifier* siempre es necesaria en este contexto.</span><span class="sxs-lookup"><span data-stu-id="0f896-176">No default exists for this context, so an *attribute_target_specifier* is always required in this context.</span></span> <span data-ttu-id="0f896-177">La presencia de la `assembly` *attribute_target_specifier* indica que el atributo se aplica al destino ensamblado; la presencia de la `module` *attribute_target_specifier* indica que el atributo se aplica al módulo de destino.</span><span class="sxs-lookup"><span data-stu-id="0f896-177">The presence of the `assembly` *attribute_target_specifier* indicates that the attribute applies to the target assembly; the presence of the `module` *attribute_target_specifier* indicates that the attribute applies to the target module.</span></span>
*  <span data-ttu-id="0f896-178">Puede aplicar un atributo especificado en una declaración de delegado para el delegado que se declara o a su valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0f896-178">An attribute specified on a delegate declaration can apply either to the delegate being declared or to its return value.</span></span> <span data-ttu-id="0f896-179">En ausencia de un *attribute_target_specifier*, el atributo se aplica al delegado.</span><span class="sxs-lookup"><span data-stu-id="0f896-179">In the absence of an *attribute_target_specifier*, the attribute applies to the delegate.</span></span> <span data-ttu-id="0f896-180">La presencia de la `type` *attribute_target_specifier* indica que el atributo se aplica al delegado; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0f896-180">The presence of the `type` *attribute_target_specifier* indicates that the attribute applies to the delegate; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="0f896-181">Puede aplicar un atributo especificado en una declaración de método para el método que se declara o a su valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0f896-181">An attribute specified on a method declaration can apply either to the method being declared or to its return value.</span></span> <span data-ttu-id="0f896-182">En ausencia de un *attribute_target_specifier*, el atributo se aplica al método.</span><span class="sxs-lookup"><span data-stu-id="0f896-182">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="0f896-183">La presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica al método; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0f896-183">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="0f896-184">Puede aplicar un atributo especificado en una declaración de operador para el operador que se declara o a su valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0f896-184">An attribute specified on an operator declaration can apply either to the operator being declared or to its return value.</span></span> <span data-ttu-id="0f896-185">En ausencia de un *attribute_target_specifier*, el atributo se aplica al operador.</span><span class="sxs-lookup"><span data-stu-id="0f896-185">In the absence of an *attribute_target_specifier*, the attribute applies to the operator.</span></span> <span data-ttu-id="0f896-186">La presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica al operador; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0f896-186">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the operator; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="0f896-187">Un atributo especificado en una declaración de evento que omite los descriptores de acceso de eventos puede aplicar al evento que se declara, al campo asociado (si el evento no es abstracto) o el asociado agregar y quitar métodos.</span><span class="sxs-lookup"><span data-stu-id="0f896-187">An attribute specified on an event declaration that omits event accessors can apply to the event being declared, to the associated field (if the event is not abstract), or to the associated add and remove methods.</span></span> <span data-ttu-id="0f896-188">En ausencia de un *attribute_target_specifier*, el atributo se aplica al evento.</span><span class="sxs-lookup"><span data-stu-id="0f896-188">In the absence of an *attribute_target_specifier*, the attribute applies to the event.</span></span> <span data-ttu-id="0f896-189">La presencia de la `event` *attribute_target_specifier* indica que el atributo se aplica al evento; la presencia de la `field` *attribute_target_specifier* indica que el atributo se aplica al campo; y la presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica a los métodos.</span><span class="sxs-lookup"><span data-stu-id="0f896-189">The presence of the `event` *attribute_target_specifier* indicates that the attribute applies to the event; the presence of the `field` *attribute_target_specifier* indicates that the attribute applies to the field; and the presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the methods.</span></span>
*  <span data-ttu-id="0f896-190">Puede aplicar un atributo especificado en una declaración de descriptor de acceso get de una declaración de propiedad o indizador al método asociado o a su valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0f896-190">An attribute specified on a get accessor declaration for a property or indexer declaration can apply either to the associated method or to its return value.</span></span> <span data-ttu-id="0f896-191">En ausencia de un *attribute_target_specifier*, el atributo se aplica al método.</span><span class="sxs-lookup"><span data-stu-id="0f896-191">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="0f896-192">La presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica al método; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0f896-192">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="0f896-193">Puede aplicar un atributo especificado en un descriptor de acceso para una declaración de propiedad o indizador al método asociado o a su parámetro implícito.</span><span class="sxs-lookup"><span data-stu-id="0f896-193">An attribute specified on a set accessor for a property or indexer declaration can apply either to the associated method or to its lone implicit parameter.</span></span> <span data-ttu-id="0f896-194">En ausencia de un *attribute_target_specifier*, el atributo se aplica al método.</span><span class="sxs-lookup"><span data-stu-id="0f896-194">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="0f896-195">La presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica al método; la presencia de la `param` *attribute_target_specifier* indica que el atributo se aplica al parámetro; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0f896-195">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>
*  <span data-ttu-id="0f896-196">Un atributo especificado en una declaración de descriptor de acceso add o remove para una declaración de evento se aplica al método asociado o a su parámetro.</span><span class="sxs-lookup"><span data-stu-id="0f896-196">An attribute specified on an add or remove accessor declaration for an event declaration can apply either to the associated method or to its lone parameter.</span></span> <span data-ttu-id="0f896-197">En ausencia de un *attribute_target_specifier*, el atributo se aplica al método.</span><span class="sxs-lookup"><span data-stu-id="0f896-197">In the absence of an *attribute_target_specifier*, the attribute applies to the method.</span></span> <span data-ttu-id="0f896-198">La presencia de la `method` *attribute_target_specifier* indica que el atributo se aplica al método; la presencia de la `param` *attribute_target_specifier* indica que el atributo se aplica al parámetro; la presencia de la `return` *attribute_target_specifier* indica que el atributo se aplica al valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0f896-198">The presence of the `method` *attribute_target_specifier* indicates that the attribute applies to the method; the presence of the `param` *attribute_target_specifier* indicates that the attribute applies to the parameter; the presence of the `return` *attribute_target_specifier* indicates that the attribute applies to the return value.</span></span>

<span data-ttu-id="0f896-199">En otros contextos, la inclusión de un *attribute_target_specifier* está permitido pero innecesarios.</span><span class="sxs-lookup"><span data-stu-id="0f896-199">In other contexts, inclusion of an *attribute_target_specifier* is permitted but unnecessary.</span></span> <span data-ttu-id="0f896-200">Por ejemplo, puede incluir u omitir el especificador de una declaración de clase `type`:</span><span class="sxs-lookup"><span data-stu-id="0f896-200">For instance, a class declaration may either include or omit the specifier `type`:</span></span>

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

<span data-ttu-id="0f896-201">Es un error especificar no es válido *attribute_target_specifier*.</span><span class="sxs-lookup"><span data-stu-id="0f896-201">It is an error to specify an invalid *attribute_target_specifier*.</span></span> <span data-ttu-id="0f896-202">Por ejemplo, el especificador de `param` no se puede usar en una declaración de clase:</span><span class="sxs-lookup"><span data-stu-id="0f896-202">For instance, the specifier `param` cannot be used on a class declaration:</span></span>

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

<span data-ttu-id="0f896-203">Por convención, las clases de atributos se denominan con el sufijo `Attribute`.</span><span class="sxs-lookup"><span data-stu-id="0f896-203">By convention, attribute classes are named with a suffix of `Attribute`.</span></span> <span data-ttu-id="0f896-204">Un *attribute_name* del formulario *type_name* puede incluir u omitir este sufijo.</span><span class="sxs-lookup"><span data-stu-id="0f896-204">An *attribute_name* of the form *type_name* may either include or omit this suffix.</span></span> <span data-ttu-id="0f896-205">Si se encuentra una clase de atributos con y sin este sufijo, una ambigüedad está presente y se producirá un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0f896-205">If an attribute class is found both with and without this suffix, an ambiguity is present, and a compile-time error results.</span></span> <span data-ttu-id="0f896-206">Si el *attribute_name* está escrito tal que su derecha *identificador* es un identificador textual ([identificadores](lexical-structure.md#identifiers)), a continuación, se asocia a solo un atributo sin un sufijo, lo que permite que se resuelva una ambigüedad.</span><span class="sxs-lookup"><span data-stu-id="0f896-206">If the *attribute_name* is spelled such that its right-most *identifier* is a verbatim identifier ([Identifiers](lexical-structure.md#identifiers)), then only an attribute without a suffix is matched, thus enabling such an ambiguity to be resolved.</span></span> <span data-ttu-id="0f896-207">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-207">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

<span data-ttu-id="0f896-208">muestra dos clases de atributos denominados `X` y `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0f896-208">shows two attribute classes named `X` and `XAttribute`.</span></span> <span data-ttu-id="0f896-209">El atributo `[X]` es ambiguo, porque podría referirse a cualquiera `X` o `XAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0f896-209">The attribute `[X]` is ambiguous, since it could refer to either `X` or `XAttribute`.</span></span> <span data-ttu-id="0f896-210">El uso de un identificador textual permite la intención exacta que se especifique en estos casos poco frecuentes.</span><span class="sxs-lookup"><span data-stu-id="0f896-210">Using a verbatim identifier allows the exact intent to be specified in such rare cases.</span></span> <span data-ttu-id="0f896-211">El atributo `[XAttribute]` no es ambiguo (aunque lo sería si se ha producido una clase de atributo denominada `XAttributeAttribute`!).</span><span class="sxs-lookup"><span data-stu-id="0f896-211">The attribute `[XAttribute]` is not ambiguous (although it would be if there was an attribute class named `XAttributeAttribute`!).</span></span> <span data-ttu-id="0f896-212">Si la declaración de clase `X` se quita, a continuación, ambos atributos hacen referencia a la clase de atributo denominada `XAttribute`, como sigue:</span><span class="sxs-lookup"><span data-stu-id="0f896-212">If the declaration for class `X` is removed, then both attributes refer to the attribute class named `XAttribute`, as follows:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

<span data-ttu-id="0f896-213">Es un error en tiempo de compilación para usar una clase de atributo de uso único más de una vez en la misma entidad.</span><span class="sxs-lookup"><span data-stu-id="0f896-213">It is a compile-time error to use a single-use attribute class more than once on the same entity.</span></span> <span data-ttu-id="0f896-214">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-214">The example</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

<span data-ttu-id="0f896-215">da como resultado un error en tiempo de compilación porque intenta utilizar `HelpString`, que es una clase de atributo de uso único, más de una vez en la declaración de `Class1`.</span><span class="sxs-lookup"><span data-stu-id="0f896-215">results in a compile-time error because it attempts to use `HelpString`, which is a single-use attribute class, more than once on the declaration of `Class1`.</span></span>

<span data-ttu-id="0f896-216">Una expresión `E` es un *attribute_argument_expression* si se cumplen todas las instrucciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0f896-216">An expression `E` is an *attribute_argument_expression* if all of the following statements are true:</span></span>

*  <span data-ttu-id="0f896-217">El tipo de `E` es un tipo de parámetro de atributo ([tipos de parámetro de atributo](attributes.md#attribute-parameter-types)).</span><span class="sxs-lookup"><span data-stu-id="0f896-217">The type of `E` is an attribute parameter type ([Attribute parameter types](attributes.md#attribute-parameter-types)).</span></span>
*  <span data-ttu-id="0f896-218">En tiempo de compilación, el valor de `E` puede resolverse en uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="0f896-218">At compile-time, the value of `E` can be resolved to one of the following:</span></span>
   * <span data-ttu-id="0f896-219">Un valor constante.</span><span class="sxs-lookup"><span data-stu-id="0f896-219">A constant value.</span></span>
   * <span data-ttu-id="0f896-220">Objeto `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="0f896-220">A `System.Type` object.</span></span>
   * <span data-ttu-id="0f896-221">Una matriz unidimensional de *attribute_argument_expression*s.</span><span class="sxs-lookup"><span data-stu-id="0f896-221">A one-dimensional array of *attribute_argument_expression*s.</span></span>

<span data-ttu-id="0f896-222">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0f896-222">For example:</span></span>

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

<span data-ttu-id="0f896-223">Un *typeof_expression* ([el operador typeof](expressions.md#the-typeof-operator)) utiliza como una expresión de argumento de atributo puede hacer referencia a un tipo no genérico, un tipo construido cerrado o un tipo genérico sin enlazar, pero no se puede hacer referencia a un Abra el tipo.</span><span class="sxs-lookup"><span data-stu-id="0f896-223">A *typeof_expression* ([The typeof operator](expressions.md#the-typeof-operator)) used as an attribute argument expression can reference a non-generic type, a closed constructed type, or an unbound generic type, but it cannot reference an open type.</span></span> <span data-ttu-id="0f896-224">Esto es para asegurarse de que la expresión se puede resolver en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0f896-224">This is to ensure that the expression can be resolved at compile-time.</span></span>

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a><span data-ttu-id="0f896-225">Instancias de atributo</span><span class="sxs-lookup"><span data-stu-id="0f896-225">Attribute instances</span></span>

<span data-ttu-id="0f896-226">Un ***instancia de atributo*** es una instancia que representa un atributo en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="0f896-226">An ***attribute instance*** is an instance that represents an attribute at run-time.</span></span> <span data-ttu-id="0f896-227">Un atributo está había definido con una clase de atributos, los argumentos posicionales y argumentos con nombre.</span><span class="sxs-lookup"><span data-stu-id="0f896-227">An attribute is defined with an attribute class, positional arguments, and named arguments.</span></span> <span data-ttu-id="0f896-228">Una instancia de atributo es una instancia de la clase de atributo que se inicializa con los argumentos posicionales y con nombre.</span><span class="sxs-lookup"><span data-stu-id="0f896-228">An attribute instance is an instance of the attribute class that is initialized with the positional and named arguments.</span></span>

<span data-ttu-id="0f896-229">Recuperación de una instancia de atributo implica el procesamiento de tiempo de compilación y tiempo de ejecución, como se describe en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="0f896-229">Retrieval of an attribute instance involves both compile-time and run-time processing, as described in the following sections.</span></span>

### <a name="compilation-of-an-attribute"></a><span data-ttu-id="0f896-230">Compilación de un atributo</span><span class="sxs-lookup"><span data-stu-id="0f896-230">Compilation of an attribute</span></span>

<span data-ttu-id="0f896-231">La compilación de un *atributo* con la clase de atributo `T`, *positional_argument_list* `P` y *named_argument_list* `N`, consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="0f896-231">The compilation of an *attribute* with attribute class `T`, *positional_argument_list* `P` and *named_argument_list* `N`, consists of the following steps:</span></span>

*  <span data-ttu-id="0f896-232">Siga los pasos de procesamiento en tiempo de compilación para compilar un *object_creation_expression* del formulario `new T(P)`.</span><span class="sxs-lookup"><span data-stu-id="0f896-232">Follow the compile-time processing steps for compiling an *object_creation_expression* of the form `new T(P)`.</span></span> <span data-ttu-id="0f896-233">Estos pasos dan lugar a un error en tiempo de compilación o determinar un constructor de instancia `C` en `T` que se puede invocar en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="0f896-233">These steps either result in a compile-time error, or determine an instance constructor `C` on `T` that can be invoked at run-time.</span></span>
*  <span data-ttu-id="0f896-234">Si `C` no tiene accesibilidad pública y, a continuación, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0f896-234">If `C` does not have public accessibility, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="0f896-235">Para cada *named_argument* `Arg` en `N`:</span><span class="sxs-lookup"><span data-stu-id="0f896-235">For each *named_argument* `Arg` in `N`:</span></span>
   * <span data-ttu-id="0f896-236">Permiten `Name` ser el *identificador* de la *named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="0f896-236">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span>
   * <span data-ttu-id="0f896-237">`Name` debe identificar un campo público de lectura y escritura no estático o una propiedad en `T`.</span><span class="sxs-lookup"><span data-stu-id="0f896-237">`Name` must identify a non-static read-write public field or property on `T`.</span></span> <span data-ttu-id="0f896-238">Si `T` tiene no existe ese campo o propiedad, a continuación, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0f896-238">If `T` has no such field or property, then a compile-time error occurs.</span></span>
*  <span data-ttu-id="0f896-239">Recuerde la siguiente información para la creación de instancias de tiempo de ejecución del atributo: la clase de atributo `T`, el constructor de instancia `C` en `T`, *positional_argument_list* `P` y el *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="0f896-239">Keep the following information for run-time instantiation of the attribute: the attribute class `T`, the instance constructor `C` on `T`, the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

### <a name="run-time-retrieval-of-an-attribute-instance"></a><span data-ttu-id="0f896-240">Recuperación de tiempo de ejecución de una instancia de atributo</span><span class="sxs-lookup"><span data-stu-id="0f896-240">Run-time retrieval of an attribute instance</span></span>

<span data-ttu-id="0f896-241">Compilación de un *atributo* da como resultado una clase de atributo `T`, un constructor de instancia `C` en `T`, un *positional_argument_list* `P`y un *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="0f896-241">Compilation of an *attribute* yields an attribute class `T`, an instance constructor `C` on `T`, a *positional_argument_list* `P`, and a *named_argument_list* `N`.</span></span> <span data-ttu-id="0f896-242">Dada esta información, se puede recuperar una instancia de atributo en tiempo de ejecución mediante los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="0f896-242">Given this information, an attribute instance can be retrieved at run-time using the following steps:</span></span>

*  <span data-ttu-id="0f896-243">Siga los pasos de procesamiento en tiempo de ejecución para ejecutar un *object_creation_expression* del formulario `new T(P)`, utilizando el constructor de instancia `C` como se determina en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0f896-243">Follow the run-time processing steps for executing an *object_creation_expression* of the form `new T(P)`, using the instance constructor `C` as determined at compile-time.</span></span> <span data-ttu-id="0f896-244">Estos pasos dan lugar a una excepción, o generar una instancia `O` de `T`.</span><span class="sxs-lookup"><span data-stu-id="0f896-244">These steps either result in an exception, or produce an instance `O` of `T`.</span></span>
*  <span data-ttu-id="0f896-245">Para cada *named_argument* `Arg` en `N`, en orden:</span><span class="sxs-lookup"><span data-stu-id="0f896-245">For each *named_argument* `Arg` in `N`, in order:</span></span>
   * <span data-ttu-id="0f896-246">Permiten `Name` ser el *identificador* de la *named_argument* `Arg`.</span><span class="sxs-lookup"><span data-stu-id="0f896-246">Let `Name` be the *identifier* of the *named_argument* `Arg`.</span></span> <span data-ttu-id="0f896-247">Si `Name` no identifica un campo de lectura y escritura público no estático o una propiedad en `O`, a continuación, se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="0f896-247">If `Name` does not identify a non-static public read-write field or property on `O`, then an exception is thrown.</span></span>
   * <span data-ttu-id="0f896-248">Permiten `Value` ser el resultado de evaluar la *attribute_argument_expression* de `Arg`.</span><span class="sxs-lookup"><span data-stu-id="0f896-248">Let `Value` be the result of evaluating the *attribute_argument_expression* of `Arg`.</span></span>
   * <span data-ttu-id="0f896-249">Si `Name` identifica un campo en `O`, a continuación, establezca este campo en `Value`.</span><span class="sxs-lookup"><span data-stu-id="0f896-249">If `Name` identifies a field on `O`, then set this field to `Value`.</span></span>
   * <span data-ttu-id="0f896-250">En caso contrario, `Name` identifica una propiedad en `O`.</span><span class="sxs-lookup"><span data-stu-id="0f896-250">Otherwise, `Name` identifies a property on `O`.</span></span> <span data-ttu-id="0f896-251">Establezca esta propiedad en `Value`.</span><span class="sxs-lookup"><span data-stu-id="0f896-251">Set this property to `Value`.</span></span>
   * <span data-ttu-id="0f896-252">El resultado es `O`, una instancia de la clase de atributo `T` que se ha inicializado con el *positional_argument_list* `P` y *named_argument_list* `N`.</span><span class="sxs-lookup"><span data-stu-id="0f896-252">The result is `O`, an instance of the attribute class `T` that has been initialized with the *positional_argument_list* `P` and the *named_argument_list* `N`.</span></span>

## <a name="reserved-attributes"></a><span data-ttu-id="0f896-253">Atributos reservados</span><span class="sxs-lookup"><span data-stu-id="0f896-253">Reserved attributes</span></span>

<span data-ttu-id="0f896-254">Un pequeño número de atributos afecta al lenguaje de alguna manera.</span><span class="sxs-lookup"><span data-stu-id="0f896-254">A small number of attributes affect the language in some way.</span></span> <span data-ttu-id="0f896-255">Estos atributos incluyen:</span><span class="sxs-lookup"><span data-stu-id="0f896-255">These attributes include:</span></span>

*  <span data-ttu-id="0f896-256">`System.AttributeUsageAttribute` ([Atributo AttributeUsage the](attributes.md#the-attributeusage-attribute)), que se usa para describir las formas en que se puede usar una clase de atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-256">`System.AttributeUsageAttribute` ([The AttributeUsage attribute](attributes.md#the-attributeusage-attribute)), which is used to describe the ways in which an attribute class can be used.</span></span>
*  <span data-ttu-id="0f896-257">`System.Diagnostics.ConditionalAttribute` ([El atributo Conditional](attributes.md#the-conditional-attribute)), que se usa para definir métodos condicionales.</span><span class="sxs-lookup"><span data-stu-id="0f896-257">`System.Diagnostics.ConditionalAttribute` ([The Conditional attribute](attributes.md#the-conditional-attribute)), which is used to define conditional methods.</span></span>
*  <span data-ttu-id="0f896-258">`System.ObsoleteAttribute` ([Obsoleto el atributo](attributes.md#the-obsolete-attribute)), que se usa para marcar un miembro como obsoleto.</span><span class="sxs-lookup"><span data-stu-id="0f896-258">`System.ObsoleteAttribute` ([The Obsolete attribute](attributes.md#the-obsolete-attribute)), which is used to mark a member as obsolete.</span></span>
*  <span data-ttu-id="0f896-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` y `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([atributos de información del llamador](attributes.md#caller-info-attributes)), que se usan para proporcionar información sobre el contexto de llamada a los parámetros opcionales.</span><span class="sxs-lookup"><span data-stu-id="0f896-259">`System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` and `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Caller info attributes](attributes.md#caller-info-attributes)), which are used to supply information about the calling context to optional parameters.</span></span>

### <a name="the-attributeusage-attribute"></a><span data-ttu-id="0f896-260">El atributo AttributeUsage</span><span class="sxs-lookup"><span data-stu-id="0f896-260">The AttributeUsage attribute</span></span>

<span data-ttu-id="0f896-261">El atributo `AttributeUsage` se usa para describir la manera en que se puede usar la clase de atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-261">The attribute `AttributeUsage` is used to describe the manner in which the attribute class can be used.</span></span>

<span data-ttu-id="0f896-262">Una clase decorada con el `AttributeUsage` atributo debe derivar de `System.Attribute`, directa o indirectamente.</span><span class="sxs-lookup"><span data-stu-id="0f896-262">A class that is decorated with the `AttributeUsage` attribute must derive from `System.Attribute`, either directly or indirectly.</span></span> <span data-ttu-id="0f896-263">De lo contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0f896-263">Otherwise, a compile-time error occurs.</span></span>

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a><span data-ttu-id="0f896-264">El atributo Conditional</span><span class="sxs-lookup"><span data-stu-id="0f896-264">The Conditional attribute</span></span>

<span data-ttu-id="0f896-265">El atributo `Conditional` permite la definición de ***métodos condicionales*** y ***clases de atributos condicionales***.</span><span class="sxs-lookup"><span data-stu-id="0f896-265">The attribute `Conditional` enables the definition of ***conditional methods*** and ***conditional attribute classes***.</span></span>

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a><span data-ttu-id="0f896-266">Métodos condicionales</span><span class="sxs-lookup"><span data-stu-id="0f896-266">Conditional methods</span></span>

<span data-ttu-id="0f896-267">Un método decorado con el `Conditional` atributo es un método condicional.</span><span class="sxs-lookup"><span data-stu-id="0f896-267">A method decorated with the `Conditional` attribute is a conditional method.</span></span> <span data-ttu-id="0f896-268">El `Conditional` atributo indica una condición mediante la comprobación de un símbolo de compilación condicional.</span><span class="sxs-lookup"><span data-stu-id="0f896-268">The `Conditional` attribute indicates a condition by testing a conditional compilation symbol.</span></span> <span data-ttu-id="0f896-269">Las llamadas a un método condicional se incluyen o se omiten en función de si se define este símbolo en el momento de la llamada.</span><span class="sxs-lookup"><span data-stu-id="0f896-269">Calls to a conditional method are either included or omitted depending on whether this symbol is defined at the point of the call.</span></span> <span data-ttu-id="0f896-270">Si el símbolo está definido, la llamada se incluye; en caso contrario, se omite la llamada (incluida la evaluación del receptor y los parámetros de la llamada).</span><span class="sxs-lookup"><span data-stu-id="0f896-270">If the symbol is defined, the call is included; otherwise, the call (including evaluation of the receiver and parameters of the call) is omitted.</span></span>

<span data-ttu-id="0f896-271">Un método condicional está sujeto a las restricciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0f896-271">A conditional method is subject to the following restrictions:</span></span>

*  <span data-ttu-id="0f896-272">El método condicional debe ser un método en un *class_declaration* o *struct_declaration*.</span><span class="sxs-lookup"><span data-stu-id="0f896-272">The conditional method must be a method in a *class_declaration* or *struct_declaration*.</span></span> <span data-ttu-id="0f896-273">Se produce un error de tiempo de compilación si el `Conditional` se especifica el atributo en un método en una declaración de interfaz.</span><span class="sxs-lookup"><span data-stu-id="0f896-273">A compile-time error occurs if the `Conditional` attribute is specified on a method in an interface declaration.</span></span>
*  <span data-ttu-id="0f896-274">El método condicional debe tener un tipo de valor devuelto de `void`.</span><span class="sxs-lookup"><span data-stu-id="0f896-274">The conditional method must have a return type of `void`.</span></span>
*  <span data-ttu-id="0f896-275">El método condicional no se debe marcar con el `override` modificador.</span><span class="sxs-lookup"><span data-stu-id="0f896-275">The conditional method must not be marked with the `override` modifier.</span></span> <span data-ttu-id="0f896-276">Un método condicional puede marcarse con el `virtual` modificador, sin embargo.</span><span class="sxs-lookup"><span data-stu-id="0f896-276">A conditional method may be marked with the `virtual` modifier, however.</span></span> <span data-ttu-id="0f896-277">Invalidaciones de este método son implícitamente condicionales y no deben marcarse explícitamente con un `Conditional` atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-277">Overrides of such a method are implicitly conditional, and must not be explicitly marked with a `Conditional` attribute.</span></span>
*  <span data-ttu-id="0f896-278">El método condicional no debe ser una implementación de un método de interfaz.</span><span class="sxs-lookup"><span data-stu-id="0f896-278">The conditional method must not be an implementation of an interface method.</span></span> <span data-ttu-id="0f896-279">De lo contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0f896-279">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0f896-280">Además, se produce un error de tiempo de compilación si un método condicional se usa en un *delegate_creation_expression*.</span><span class="sxs-lookup"><span data-stu-id="0f896-280">In addition, a compile-time error occurs if a conditional method is used in a *delegate_creation_expression*.</span></span> <span data-ttu-id="0f896-281">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-281">The example</span></span>

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

<span data-ttu-id="0f896-282">declara `Class1.M` como un método condicional.</span><span class="sxs-lookup"><span data-stu-id="0f896-282">declares `Class1.M` as a conditional method.</span></span> <span data-ttu-id="0f896-283">`Class2`del `Test` método llama a este método.</span><span class="sxs-lookup"><span data-stu-id="0f896-283">`Class2`'s `Test` method calls this method.</span></span> <span data-ttu-id="0f896-284">Desde el símbolo de compilación condicional `DEBUG` se define, si `Class2.Test` es llamado, llamará a `M`.</span><span class="sxs-lookup"><span data-stu-id="0f896-284">Since the conditional compilation symbol `DEBUG` is defined, if `Class2.Test` is called, it will call `M`.</span></span> <span data-ttu-id="0f896-285">Si el símbolo `DEBUG` no estaba definido, a continuación, `Class2.Test` no llamaría a `Class1.M`.</span><span class="sxs-lookup"><span data-stu-id="0f896-285">If the symbol `DEBUG` had not been defined, then `Class2.Test` would not call `Class1.M`.</span></span>

<span data-ttu-id="0f896-286">Es importante tener en cuenta que la inclusión o exclusión de una llamada a un método condicional se controla mediante los símbolos de compilación condicional en el momento de la llamada.</span><span class="sxs-lookup"><span data-stu-id="0f896-286">It is important to note that the inclusion or exclusion of a call to a conditional method is controlled by the conditional compilation symbols at the point of the call.</span></span> <span data-ttu-id="0f896-287">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-287">In the example</span></span>

<span data-ttu-id="0f896-288">Archivo `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="0f896-288">File `class1.cs`:</span></span>

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

<span data-ttu-id="0f896-289">Archivo `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="0f896-289">File `class2.cs`:</span></span>

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

<span data-ttu-id="0f896-290">Archivo `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="0f896-290">File `class3.cs`:</span></span>

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

<span data-ttu-id="0f896-291">las clases `Class2` y `Class3` cada uno contiene las llamadas al método condicional `Class1.F`, que es condicional o no según `DEBUG` está definido.</span><span class="sxs-lookup"><span data-stu-id="0f896-291">the classes `Class2` and `Class3` each contain calls to the conditional method `Class1.F`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="0f896-292">Puesto que este símbolo se define en el contexto de `Class2` pero no `Class3`, la llamada a `F` en `Class2` se incluye, mientras que la llamada a `F` en `Class3` se omite.</span><span class="sxs-lookup"><span data-stu-id="0f896-292">Since this symbol is defined in the context of `Class2` but not `Class3`, the call to `F` in `Class2` is included, while the call to `F` in `Class3` is omitted.</span></span>

<span data-ttu-id="0f896-293">El uso de métodos condicionales en una cadena de herencia puede resultar confuso.</span><span class="sxs-lookup"><span data-stu-id="0f896-293">The use of conditional methods in an inheritance chain can be confusing.</span></span> <span data-ttu-id="0f896-294">Las llamadas realizadas a un método condicional mediante `base`, del formulario `base.M`, están sujetos a las reglas de llamada de método condicional normal.</span><span class="sxs-lookup"><span data-stu-id="0f896-294">Calls made to a conditional method through `base`, of the form `base.M`, are subject to the normal conditional method call rules.</span></span> <span data-ttu-id="0f896-295">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-295">In the example</span></span>

<span data-ttu-id="0f896-296">Archivo `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="0f896-296">File `class1.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

<span data-ttu-id="0f896-297">Archivo `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="0f896-297">File `class2.cs`:</span></span>

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

<span data-ttu-id="0f896-298">Archivo `class3.cs`:</span><span class="sxs-lookup"><span data-stu-id="0f896-298">File `class3.cs`:</span></span>

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

<span data-ttu-id="0f896-299">`Class2` incluye una llamada a la `M` definido en su clase base.</span><span class="sxs-lookup"><span data-stu-id="0f896-299">`Class2` includes a call to the `M` defined in its base class.</span></span> <span data-ttu-id="0f896-300">Esta llamada se omite porque el método base es condicional basándose en la presencia del símbolo `DEBUG`, que no está definido.</span><span class="sxs-lookup"><span data-stu-id="0f896-300">This call is omitted because the base method is conditional based on the presence of the symbol `DEBUG`, which is undefined.</span></span> <span data-ttu-id="0f896-301">Por lo tanto, el método escribe en la consola "`Class2.M executed`" sólo.</span><span class="sxs-lookup"><span data-stu-id="0f896-301">Thus, the method writes to the console "`Class2.M executed`" only.</span></span> <span data-ttu-id="0f896-302">Un uso razonable de *pp_declaration*s puede eliminar tales problemas.</span><span class="sxs-lookup"><span data-stu-id="0f896-302">Judicious use of *pp_declaration*s can eliminate such problems.</span></span>

#### <a name="conditional-attribute-classes"></a><span data-ttu-id="0f896-303">Clases de atributo Conditional</span><span class="sxs-lookup"><span data-stu-id="0f896-303">Conditional attribute classes</span></span>

<span data-ttu-id="0f896-304">Una clase de atributos ([clases de atributo](attributes.md#attribute-classes)) decorada con uno o varios `Conditional` atributos es un ***clase de atributo condicional***.</span><span class="sxs-lookup"><span data-stu-id="0f896-304">An attribute class ([Attribute classes](attributes.md#attribute-classes)) decorated with one or more `Conditional` attributes is a ***conditional attribute class***.</span></span> <span data-ttu-id="0f896-305">Una clase de atributo condicional, por tanto, se asocia con los símbolos de compilación condicional declarados en sus `Conditional` atributos.</span><span class="sxs-lookup"><span data-stu-id="0f896-305">A conditional attribute class is thus associated with the conditional compilation symbols declared in its `Conditional` attributes.</span></span> <span data-ttu-id="0f896-306">En este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0f896-306">This example:</span></span>

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

<span data-ttu-id="0f896-307">declara `TestAttribute` como una clase de atributos condicionales asociada con los símbolos de compilaciones condicionales `ALPHA` y `BETA`.</span><span class="sxs-lookup"><span data-stu-id="0f896-307">declares `TestAttribute` as a conditional attribute class associated with the conditional compilations symbols `ALPHA` and `BETA`.</span></span>

<span data-ttu-id="0f896-308">Las especificaciones del atributo ([especificación de atributos](attributes.md#attribute-specification)) de un atributo conditional se incluyen si uno o varios de sus símbolos de compilación condicional asociado se definen en el punto de especificación, en caso contrario, el atributo se omite la especificación.</span><span class="sxs-lookup"><span data-stu-id="0f896-308">Attribute specifications ([Attribute specification](attributes.md#attribute-specification)) of a conditional attribute are included if one or more of its associated conditional compilation symbols is defined at the point of specification, otherwise the attribute specification is omitted.</span></span>

<span data-ttu-id="0f896-309">Es importante tener en cuenta que la inclusión o exclusión de una especificación de atributo de una clase de atributo conditional se controla mediante los símbolos de compilación condicional en el momento de la especificación.</span><span class="sxs-lookup"><span data-stu-id="0f896-309">It is important to note that the inclusion or exclusion of an attribute specification of a conditional attribute class is controlled by the conditional compilation symbols at the point of the specification.</span></span> <span data-ttu-id="0f896-310">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-310">In the example</span></span>

<span data-ttu-id="0f896-311">Archivo `test.cs`:</span><span class="sxs-lookup"><span data-stu-id="0f896-311">File `test.cs`:</span></span>

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

<span data-ttu-id="0f896-312">Archivo `class1.cs`:</span><span class="sxs-lookup"><span data-stu-id="0f896-312">File `class1.cs`:</span></span>

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

<span data-ttu-id="0f896-313">Archivo `class2.cs`:</span><span class="sxs-lookup"><span data-stu-id="0f896-313">File `class2.cs`:</span></span>

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

<span data-ttu-id="0f896-314">las clases `Class1` y `Class2` son decoradas con el atributo `Test`, que es condicional o no según `DEBUG` está definido.</span><span class="sxs-lookup"><span data-stu-id="0f896-314">the classes `Class1` and `Class2` are each decorated with attribute `Test`, which is conditional based on whether or not `DEBUG` is defined.</span></span> <span data-ttu-id="0f896-315">Puesto que este símbolo se define en el contexto de `Class1` pero no `Class2`, la especificación de la `Test` atributo `Class1` se incluye, mientras la especificación de la `Test` atributo `Class2` se omite.</span><span class="sxs-lookup"><span data-stu-id="0f896-315">Since this symbol is defined in the context of `Class1` but not `Class2`, the specification of the `Test` attribute on `Class1` is included, while the specification of the `Test` attribute on `Class2` is omitted.</span></span>

### <a name="the-obsolete-attribute"></a><span data-ttu-id="0f896-316">El atributo Obsolete</span><span class="sxs-lookup"><span data-stu-id="0f896-316">The Obsolete attribute</span></span>

<span data-ttu-id="0f896-317">El atributo `Obsolete` se usa para marcar los tipos y miembros de tipos que ya no se deben usar.</span><span class="sxs-lookup"><span data-stu-id="0f896-317">The attribute `Obsolete` is used to mark types and members of types that should no longer be used.</span></span>

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

<span data-ttu-id="0f896-318">Si un programa usa un tipo o miembro que está decorada con el `Obsolete` atributo, el compilador emite una advertencia o un error.</span><span class="sxs-lookup"><span data-stu-id="0f896-318">If a program uses a type or member that is decorated with the `Obsolete` attribute, the compiler issues a warning or an error.</span></span> <span data-ttu-id="0f896-319">En concreto, el compilador emite una advertencia si no se proporciona ningún parámetro de error, o si el parámetro de error se proporciona y tiene el valor `false`.</span><span class="sxs-lookup"><span data-stu-id="0f896-319">Specifically, the compiler issues a warning if no error parameter is provided, or if the error parameter is provided and has the value `false`.</span></span> <span data-ttu-id="0f896-320">El compilador emite un error si el parámetro de error especificado y tiene el valor `true`.</span><span class="sxs-lookup"><span data-stu-id="0f896-320">The compiler issues an error if the error parameter is specified and has the value `true`.</span></span>

<span data-ttu-id="0f896-321">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0f896-321">In the example</span></span>

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

<span data-ttu-id="0f896-322">la clase `A` está decorada con el `Obsolete` atributo.</span><span class="sxs-lookup"><span data-stu-id="0f896-322">the class `A` is decorated with the `Obsolete` attribute.</span></span> <span data-ttu-id="0f896-323">Cada uso de `A` en `Main` produce una advertencia que incluye el mensaje especificado, "esta clase está obsoleta; Use la clase B en su lugar."</span><span class="sxs-lookup"><span data-stu-id="0f896-323">Each use of `A` in `Main` results in a warning that includes the specified message, "This class is obsolete; use class B instead."</span></span>

### <a name="caller-info-attributes"></a><span data-ttu-id="0f896-324">Atributos de información del llamador</span><span class="sxs-lookup"><span data-stu-id="0f896-324">Caller info attributes</span></span>

<span data-ttu-id="0f896-325">Para fines como registros e informes, a veces resulta útil para un miembro de función obtener determinada información de tiempo de compilación sobre el código de llamada.</span><span class="sxs-lookup"><span data-stu-id="0f896-325">For purposes such as logging and reporting, it is sometimes useful for a function member to obtain certain compile-time information about the calling code.</span></span> <span data-ttu-id="0f896-326">Los atributos de información de llamador proporcionan una manera de pasar esa información de forma transparente.</span><span class="sxs-lookup"><span data-stu-id="0f896-326">The caller info attributes provide a way to pass such information transparently.</span></span>

<span data-ttu-id="0f896-327">Si un parámetro opcional está anotado con uno de los atributos de información del llamador, si se omite el argumento correspondiente en una llamada no hace que necesariamente el valor de parámetro predeterminado que se sustituirá.</span><span class="sxs-lookup"><span data-stu-id="0f896-327">When an optional parameter is annotated with one of the caller info attributes, omitting the corresponding argument in a call does not necessarily cause the default parameter value to be substituted.</span></span> <span data-ttu-id="0f896-328">En su lugar, si está disponible la información especificada sobre el contexto de llamada, esa información se pasarán como el valor del argumento.</span><span class="sxs-lookup"><span data-stu-id="0f896-328">Instead, if the specified information about the calling context is available, that information will be passed as the argument value.</span></span>

<span data-ttu-id="0f896-329">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0f896-329">For example:</span></span>

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

<span data-ttu-id="0f896-330">Una llamada a `Log()` sin argumentos imprimía la línea número y la ruta de la llamada, así como el nombre del miembro en el que se ha producido la llamada.</span><span class="sxs-lookup"><span data-stu-id="0f896-330">A call to `Log()` with no arguments would print the line number and file path of the call, as well as the name of the member within which the call occurred.</span></span>

<span data-ttu-id="0f896-331">Atributos de información del autor de la llamada pueden producirse en los parámetros opcionales en cualquier lugar, incluido en las declaraciones de delegado.</span><span class="sxs-lookup"><span data-stu-id="0f896-331">Caller info attributes can occur on optional parameters anywhere, including in delegate declarations.</span></span> <span data-ttu-id="0f896-332">Sin embargo, los atributos de información de llamador concreto tengan restricciones en los tipos de los parámetros que puede atribuir, por lo que siempre habrá una conversión implícita de un valor sustituido para el tipo de parámetro.</span><span class="sxs-lookup"><span data-stu-id="0f896-332">However, the specific caller info attributes have restrictions on the types of the parameters they can attribute, so that there will always be an implicit conversion from a substituted value to the parameter type.</span></span>

<span data-ttu-id="0f896-333">Es un error con el mismo atributo de información del llamador en un parámetro de tanto la definición e implementación de parte de una declaración de método parcial.</span><span class="sxs-lookup"><span data-stu-id="0f896-333">It is an error to have the same caller info attribute on a parameter of both the defining and implementing part of a partial method declaration.</span></span> <span data-ttu-id="0f896-334">Se aplican sólo atributos de información de llamador en la parte de la definición, mientras que se omiten los atributos de información del autor de llamada que se producen solo en la parte de la implementación.</span><span class="sxs-lookup"><span data-stu-id="0f896-334">Only caller info attributes in the defining part are applied, whereas caller info attributes occurring only in the implementing part are ignored.</span></span>

<span data-ttu-id="0f896-335">Información del llamador no afecta a la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="0f896-335">Caller information does not affect overload resolution.</span></span> <span data-ttu-id="0f896-336">Como todavía se omiten los parámetros opcionales con atributos del código fuente del llamador, la resolución de sobrecarga ignora los parámetros de la misma manera omite otros parámetros opcionales se omite ([de resolución de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="0f896-336">As the attributed optional parameters are still omitted from the source code of the caller, overload resolution ignores those parameters in the same way it ignores other omitted optional parameters ([Overload resolution](expressions.md#overload-resolution)).</span></span>

<span data-ttu-id="0f896-337">Información del llamador se sustituye solo cuando una función se invoca explícitamente en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="0f896-337">Caller information is only substituted when a function is explicitly invoked in source code.</span></span> <span data-ttu-id="0f896-338">Invocaciones implícitas como llamadas de constructor de la ventana primaria implícita no tiene una ubicación de origen y no sustituirá la información del llamador.</span><span class="sxs-lookup"><span data-stu-id="0f896-338">Implicit invocations such as implicit parent constructor calls do not have a source location and will not substitute caller information.</span></span> <span data-ttu-id="0f896-339">Además, las llamadas que se enlazan dinámicamente no sustituirá a la información del llamador.</span><span class="sxs-lookup"><span data-stu-id="0f896-339">Also, calls that are dynamically bound will not substitute caller information.</span></span> <span data-ttu-id="0f896-340">Cuando una información del llamador con atributos de parámetro se omite en tales casos, el valor predeterminado especificado del parámetro se usa en su lugar.</span><span class="sxs-lookup"><span data-stu-id="0f896-340">When a caller info attributed parameter is omitted in such cases, the specified default value of the parameter is used instead.</span></span>

<span data-ttu-id="0f896-341">Una excepción son las expresiones de consulta.</span><span class="sxs-lookup"><span data-stu-id="0f896-341">One exception is query-expressions.</span></span> <span data-ttu-id="0f896-342">Se consideran las expansiones sintácticas y si las llamadas se expanden para omitir los parámetros opcionales con atributos de información del llamador, se sustituirá la información del llamador.</span><span class="sxs-lookup"><span data-stu-id="0f896-342">These are considered syntactic expansions, and if the calls they expand to omit optional parameters with caller info attributes, caller information will be substituted.</span></span> <span data-ttu-id="0f896-343">La ubicación utilizada es la ubicación de la cláusula de consulta que generó la llamada.</span><span class="sxs-lookup"><span data-stu-id="0f896-343">The location used is the location of the query clause which the call was generated from.</span></span>

<span data-ttu-id="0f896-344">Si se especifica más de un atributo de información del llamador en un parámetro determinado, están la preferida en el siguiente orden: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span><span class="sxs-lookup"><span data-stu-id="0f896-344">If more than one caller info attribute is specified on a given parameter, they are preferred in the following order: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.</span></span>

#### <a name="the-callerlinenumber-attribute"></a><span data-ttu-id="0f896-345">El atributo CallerLineNumber</span><span class="sxs-lookup"><span data-stu-id="0f896-345">The CallerLineNumber attribute</span></span>

<span data-ttu-id="0f896-346">El `System.Runtime.CompilerServices.CallerLineNumberAttribute` se permite en parámetros opcionales cuando no hay una conversión implícita estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) desde el valor constante `int.MaxValue` para el tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="0f896-346">The `System.Runtime.CompilerServices.CallerLineNumberAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from the constant value `int.MaxValue` to the parameter's type.</span></span> <span data-ttu-id="0f896-347">Esto garantiza que se puede pasar cualquier número de línea no negativo hasta ese valor sin errores.</span><span class="sxs-lookup"><span data-stu-id="0f896-347">This ensures that any non-negative line number up to that value can be passed without error.</span></span>

<span data-ttu-id="0f896-348">En caso de una invocación de función desde una ubicación en el código fuente omite un parámetro opcional con el `CallerLineNumberAttribute`, un literal numérico que representa el número de línea de la ubicación se utiliza como argumento a la invocación en lugar del valor de parámetro predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0f896-348">If a function invocation from a location in source code omits an optional parameter with the `CallerLineNumberAttribute`, then a numeric literal representing that location's line number is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="0f896-349">Si la invocación abarca varias líneas, la línea elegida es depende de la implementación.</span><span class="sxs-lookup"><span data-stu-id="0f896-349">If the invocation spans multiple lines, the line chosen is implementation-dependent.</span></span>

<span data-ttu-id="0f896-350">Tenga en cuenta que el número de línea puede verse afectado por `#line` directivas ([línea directivas](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="0f896-350">Note that the line number may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callerfilepath-attribute"></a><span data-ttu-id="0f896-351">El atributo CallerFilePath</span><span class="sxs-lookup"><span data-stu-id="0f896-351">The CallerFilePath attribute</span></span>

<span data-ttu-id="0f896-352">El `System.Runtime.CompilerServices.CallerFilePathAttribute` se permite en parámetros opcionales cuando no hay una conversión implícita estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) desde `string` para el tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="0f896-352">The `System.Runtime.CompilerServices.CallerFilePathAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="0f896-353">En caso de una invocación de función desde una ubicación en el código fuente omite un parámetro opcional con el `CallerFilePathAttribute`, a continuación, se usa un literal de cadena que representa la ruta de la ubicación del archivo como un argumento a la invocación en lugar del valor de parámetro predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0f896-353">If a function invocation from a location in source code omits an optional parameter with the `CallerFilePathAttribute`, then a string literal representing that location's file path is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="0f896-354">El formato de la ruta de acceso de archivo es depende de la implementación.</span><span class="sxs-lookup"><span data-stu-id="0f896-354">The format of the file path is implementation-dependent.</span></span>

<span data-ttu-id="0f896-355">Tenga en cuenta que la ruta de acceso puede verse afectado por `#line` directivas ([línea directivas](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="0f896-355">Note that the file path may be affected by `#line` directives ([Line directives](lexical-structure.md#line-directives)).</span></span>

#### <a name="the-callermembername-attribute"></a><span data-ttu-id="0f896-356">El atributo CallerMemberName</span><span class="sxs-lookup"><span data-stu-id="0f896-356">The CallerMemberName attribute</span></span>

<span data-ttu-id="0f896-357">El `System.Runtime.CompilerServices.CallerMemberNameAttribute` se permite en parámetros opcionales cuando no hay una conversión implícita estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) desde `string` para el tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="0f896-357">The `System.Runtime.CompilerServices.CallerMemberNameAttribute` is allowed on optional parameters when there is a standard implicit conversion ([Standard implicit conversions](conversions.md#standard-implicit-conversions)) from `string` to the parameter's type.</span></span>

<span data-ttu-id="0f896-358">Si una invocación de función desde una ubicación dentro del cuerpo de un miembro de función o dentro de un atributo aplicado al miembro de función propio o su tipo de valor devuelto, los parámetros o parámetros de tipo en el código fuente omite un parámetro opcional con el `CallerMemberNameAttribute`, un literal de cadena que representa el nombre de ese miembro se usa como argumento a la invocación en lugar del valor de parámetro predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0f896-358">If a function invocation from a location within the body of a function member or within an attribute applied to the function member itself or its return type, parameters or type parameters in source code omits an optional parameter with the `CallerMemberNameAttribute`, then a string literal representing the name of that member is used as an argument to the invocation instead of the default parameter value.</span></span>

<span data-ttu-id="0f896-359">Para las llamadas que se producen dentro de los métodos genéricos, se usa solo el nombre del método propio, sin la lista de parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="0f896-359">For invocations that occur within generic methods, only the method name itself is used, without the type parameter list.</span></span>

<span data-ttu-id="0f896-360">Para las llamadas que se producen dentro de las implementaciones de miembros de interfaz explícita, se usa solo el nombre del método propio, sin la calificación de interfaz anterior.</span><span class="sxs-lookup"><span data-stu-id="0f896-360">For invocations that occur within explicit interface member implementations, only the method name itself is used, without the preceding interface qualification.</span></span>

<span data-ttu-id="0f896-361">Para invocaciones que se producen dentro de los descriptores de acceso de propiedad o evento, el nombre de miembro utilizado es de la propiedad o el propio evento.</span><span class="sxs-lookup"><span data-stu-id="0f896-361">For invocations that occur within property or event accessors, the member name used is that of the property or event itself.</span></span>

<span data-ttu-id="0f896-362">Para las llamadas que se producen dentro de los descriptores de acceso, el nombre de miembro utilizado es que proporcionada por un `IndexerNameAttribute` ([el atributo IndexerName](attributes.md#the-indexername-attribute)) en el miembro de indizador, si está presente, o el nombre predeterminado `Item` en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="0f896-362">For invocations that occur within indexer accessors, the member name used is that supplied by an `IndexerNameAttribute` ([The IndexerName attribute](attributes.md#the-indexername-attribute)) on the indexer member, if present, or the default name `Item` otherwise.</span></span>

<span data-ttu-id="0f896-363">Para las llamadas que se producen dentro de las declaraciones de constructores de instancias, los constructores estáticos, los destructores y operadores miembro nombre utilizado es depende de la implementación.</span><span class="sxs-lookup"><span data-stu-id="0f896-363">For invocations that occur within declarations of instance constructors, static constructors, destructors and operators the member name used is implementation-dependent.</span></span>

## <a name="attributes-for-interoperation"></a><span data-ttu-id="0f896-364">Atributos para la interoperación</span><span class="sxs-lookup"><span data-stu-id="0f896-364">Attributes for Interoperation</span></span>

<span data-ttu-id="0f896-365">Nota: En esta sección solo es aplicable a la implementación de Microsoft .NET C#.</span><span class="sxs-lookup"><span data-stu-id="0f896-365">Note: This section is applicable only to the Microsoft .NET implementation of C#.</span></span>

### <a name="interoperation-with-com-and-win32-components"></a><span data-ttu-id="0f896-366">Interoperabilidad con componentes COM y Win32</span><span class="sxs-lookup"><span data-stu-id="0f896-366">Interoperation with COM and Win32 components</span></span>

<span data-ttu-id="0f896-367">El tiempo de ejecución .NET proporciona un gran número de atributos que permiten que los programas de C# interoperar con componentes escritos con COM y las DLL de Win32.</span><span class="sxs-lookup"><span data-stu-id="0f896-367">The .NET run-time provides a large number of attributes that enable C# programs to interoperate with components written using COM and Win32 DLLs.</span></span> <span data-ttu-id="0f896-368">Por ejemplo, el `DllImport` atributo se puede usar en un `static extern` método para indicar que la implementación del método es que se encuentren en un archivo DLL de Win32.</span><span class="sxs-lookup"><span data-stu-id="0f896-368">For example, the `DllImport` attribute can be used on a `static extern` method to indicate that the implementation of the method is to be found in a Win32 DLL.</span></span> <span data-ttu-id="0f896-369">Estos atributos se encuentran en el `System.Runtime.InteropServices` espacio de nombres y documentación detallada acerca de estos atributos se encuentra en la documentación de .NET en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="0f896-369">These attributes are found in the `System.Runtime.InteropServices` namespace, and detailed documentation for these attributes is found in the .NET runtime documentation.</span></span>

### <a name="interoperation-with-other-net-languages"></a><span data-ttu-id="0f896-370">Interoperación con otros lenguajes de .NET</span><span class="sxs-lookup"><span data-stu-id="0f896-370">Interoperation with other .NET languages</span></span>

#### <a name="the-indexername-attribute"></a><span data-ttu-id="0f896-371">El atributo IndexerName</span><span class="sxs-lookup"><span data-stu-id="0f896-371">The IndexerName attribute</span></span>

<span data-ttu-id="0f896-372">Los indizadores se implementan en .NET mediante las propiedades indizadas y tengan un nombre en los metadatos de .NET.</span><span class="sxs-lookup"><span data-stu-id="0f896-372">Indexers are implemented in .NET using indexed properties, and have a name in the .NET metadata.</span></span> <span data-ttu-id="0f896-373">Si no hay ningún `IndexerName` está presente para un indizador, a continuación, el nombre del atributo `Item` se usa de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0f896-373">If no `IndexerName` attribute is present for an indexer, then the name `Item` is used by default.</span></span> <span data-ttu-id="0f896-374">El `IndexerName` atributo permite que un desarrollador al invalidar este comportamiento predeterminado y especifique un nombre diferente.</span><span class="sxs-lookup"><span data-stu-id="0f896-374">The `IndexerName` attribute enables a developer to override this default and specify a different name.</span></span>

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
