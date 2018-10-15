# <a name="namespaces"></a><span data-ttu-id="0c042-101">Espacios de nombres</span><span class="sxs-lookup"><span data-stu-id="0c042-101">Namespaces</span></span>

<span data-ttu-id="0c042-102">Programas de C# se organizan mediante espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-102">C# programs are organized using namespaces.</span></span> <span data-ttu-id="0c042-103">Los espacios de nombres se utilizan como un sistema de organización "interno" para un programa y como un sistema de organización "external": una forma de presentar los elementos de programa que se exponen a otros programas.</span><span class="sxs-lookup"><span data-stu-id="0c042-103">Namespaces are used both as an "internal" organization system for a program, and as an "external" organization system—a way of presenting program elements that are exposed to other programs.</span></span>

<span data-ttu-id="0c042-104">Las directivas using ([directivas Using](namespaces.md#using-directives)) se proporcionan para facilitar el uso de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-104">Using directives ([Using directives](namespaces.md#using-directives)) are provided to facilitate the use of namespaces.</span></span>

## <a name="compilation-units"></a><span data-ttu-id="0c042-105">Unidades de compilación</span><span class="sxs-lookup"><span data-stu-id="0c042-105">Compilation units</span></span>

<span data-ttu-id="0c042-106">Un *compilation_unit* define la estructura general de un archivo de origen.</span><span class="sxs-lookup"><span data-stu-id="0c042-106">A *compilation_unit* defines the overall structure of a source file.</span></span> <span data-ttu-id="0c042-107">Una unidad de compilación se compone de cero o más *using_directive*s seguido de cero o más *global_attributes* seguido de cero o más *namespace_member_declaration*s .</span><span class="sxs-lookup"><span data-stu-id="0c042-107">A compilation unit consists of zero or more *using_directive*s followed by zero or more *global_attributes* followed by zero or more *namespace_member_declaration*s.</span></span>

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

<span data-ttu-id="0c042-108">Un programa de C# consta de uno o más unidades de compilación, contenidos todos ellos en un archivo de código fuente independiente.</span><span class="sxs-lookup"><span data-stu-id="0c042-108">A C# program consists of one or more compilation units, each contained in a separate source file.</span></span> <span data-ttu-id="0c042-109">Cuando se compila un programa de C#, todas las unidades de compilación se procesan juntos.</span><span class="sxs-lookup"><span data-stu-id="0c042-109">When a C# program is compiled, all of the compilation units are processed together.</span></span> <span data-ttu-id="0c042-110">Por lo tanto, unidades de compilación pueden depender entre sí, posiblemente, de manera circular.</span><span class="sxs-lookup"><span data-stu-id="0c042-110">Thus, compilation units can depend on each other, possibly in a circular fashion.</span></span>

<span data-ttu-id="0c042-111">El *using_directive*s una unidad de compilación pueden afectar a la *global_attributes* y *namespace_member_declaration*s de esa unidad de compilación, pero no tienen ningún efecto otras unidades de compilación.</span><span class="sxs-lookup"><span data-stu-id="0c042-111">The *using_directive*s of a compilation unit affect the *global_attributes* and *namespace_member_declaration*s of that compilation unit, but have no effect on other compilation units.</span></span>

<span data-ttu-id="0c042-112">El *global_attributes* ([atributos](attributes.md)) de una unidad de compilación permiten la especificación de atributos para el ensamblado de destino y el módulo.</span><span class="sxs-lookup"><span data-stu-id="0c042-112">The *global_attributes* ([Attributes](attributes.md)) of a compilation unit permit the specification of attributes for the target assembly and module.</span></span> <span data-ttu-id="0c042-113">Ensamblados y módulos actúan como contenedores físicos de tipos.</span><span class="sxs-lookup"><span data-stu-id="0c042-113">Assemblies and modules act as physical containers for types.</span></span> <span data-ttu-id="0c042-114">Un ensamblado puede constar de varios módulos separados físicamente.</span><span class="sxs-lookup"><span data-stu-id="0c042-114">An assembly may consist of several physically separate modules.</span></span>

<span data-ttu-id="0c042-115">El *namespace_member_declaration*s de cada unidad de compilación de un programa contribuyen a los miembros a un espacio de declaración única denominado el espacio de nombres global.</span><span class="sxs-lookup"><span data-stu-id="0c042-115">The *namespace_member_declaration*s of each compilation unit of a program contribute members to a single declaration space called the global namespace.</span></span> <span data-ttu-id="0c042-116">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c042-116">For example:</span></span>

<span data-ttu-id="0c042-117">Archivo `A.cs`:</span><span class="sxs-lookup"><span data-stu-id="0c042-117">File `A.cs`:</span></span>
```csharp
class A {}
```

<span data-ttu-id="0c042-118">Archivo `B.cs`:</span><span class="sxs-lookup"><span data-stu-id="0c042-118">File `B.cs`:</span></span>
```csharp
class B {}
```

<span data-ttu-id="0c042-119">Las dos unidades de compilación contribuyen al espacio de nombres global único, en este caso declarar dos clases con los nombres completos `A` y `B`.</span><span class="sxs-lookup"><span data-stu-id="0c042-119">The two compilation units contribute to the single global namespace, in this case declaring two classes with the fully qualified names `A` and `B`.</span></span> <span data-ttu-id="0c042-120">Dado que las dos unidades de compilación contribuyen al mismo espacio de declaración, habría sido un error si cada una contiene una declaración de un miembro con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="0c042-120">Because the two compilation units contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="namespace-declarations"></a><span data-ttu-id="0c042-121">Declaraciones de espacio de nombres</span><span class="sxs-lookup"><span data-stu-id="0c042-121">Namespace declarations</span></span>

<span data-ttu-id="0c042-122">Un *namespace_declaration* consta de la palabra clave `namespace`, seguido de un espacio de nombres y un cuerpo, seguido opcionalmente por un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="0c042-122">A *namespace_declaration* consists of the keyword `namespace`, followed by a namespace name and body, optionally followed by a semicolon.</span></span>

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

<span data-ttu-id="0c042-123">Un *namespace_declaration* puede producirse como una declaración de nivel superior en un *compilation_unit* o como una declaración de miembro dentro de otra *namespace_declaration*.</span><span class="sxs-lookup"><span data-stu-id="0c042-123">A *namespace_declaration* may occur as a top-level declaration in a *compilation_unit* or as a member declaration within another *namespace_declaration*.</span></span> <span data-ttu-id="0c042-124">Cuando un *namespace_declaration* se produce como una declaración de nivel superior en un *compilation_unit*, el espacio de nombres se convierte en miembro del espacio de nombres global.</span><span class="sxs-lookup"><span data-stu-id="0c042-124">When a *namespace_declaration* occurs as a top-level declaration in a *compilation_unit*, the namespace becomes a member of the global namespace.</span></span> <span data-ttu-id="0c042-125">Cuando un *namespace_declaration* se produce dentro de otra *namespace_declaration*, el espacio de nombres interno se convierte en un miembro del espacio de nombres exterior.</span><span class="sxs-lookup"><span data-stu-id="0c042-125">When a *namespace_declaration* occurs within another *namespace_declaration*, the inner namespace becomes a member of the outer namespace.</span></span> <span data-ttu-id="0c042-126">En cualquier caso, el nombre de un espacio de nombres debe ser único dentro del espacio de nombres contenedor.</span><span class="sxs-lookup"><span data-stu-id="0c042-126">In either case, the name of a namespace must be unique within the containing namespace.</span></span>

<span data-ttu-id="0c042-127">Los espacios de nombres son implícitamente `public` y la declaración de un espacio de nombres no puede incluir modificadores de acceso.</span><span class="sxs-lookup"><span data-stu-id="0c042-127">Namespaces are implicitly `public` and the declaration of a namespace cannot include any access modifiers.</span></span>

<span data-ttu-id="0c042-128">Dentro de un *namespace_body*, opcional *using_directive*s importar los nombres de otros espacios de nombres, tipos y miembros, lo que permite hacer referencia a él directamente en lugar de a través de los nombres completos.</span><span class="sxs-lookup"><span data-stu-id="0c042-128">Within a *namespace_body*, the optional *using_directive*s import the names of other namespaces, types and members, allowing them to be referenced directly instead of through qualified names.</span></span> <span data-ttu-id="0c042-129">El elemento opcional *namespace_member_declaration*s contribuyen a los miembros del espacio de declaración de espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-129">The optional *namespace_member_declaration*s contribute members to the declaration space of the namespace.</span></span> <span data-ttu-id="0c042-130">Tenga en cuenta que todos los *using_directive*s deben aparecer antes que cualquier declaración de miembro.</span><span class="sxs-lookup"><span data-stu-id="0c042-130">Note that all *using_directive*s must appear before any member declarations.</span></span>

<span data-ttu-id="0c042-131">El *qualified_identifier* de un *namespace_declaration* puede ser un identificador único o una secuencia de identificadores separados por "`.`" tokens.</span><span class="sxs-lookup"><span data-stu-id="0c042-131">The *qualified_identifier* of a *namespace_declaration* may be a single identifier or a sequence of identifiers separated by "`.`" tokens.</span></span> <span data-ttu-id="0c042-132">La segunda forma permite que un programa para definir un espacio de nombres anidado sin anidar léxicamente varias declaraciones de espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-132">The latter form permits a program to define a nested namespace without lexically nesting several namespace declarations.</span></span> <span data-ttu-id="0c042-133">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="0c042-133">For example,</span></span>

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
<span data-ttu-id="0c042-134">es semánticamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="0c042-134">is semantically equivalent to</span></span>
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

<span data-ttu-id="0c042-135">Los espacios de nombres son abiertas y dos declaraciones de espacio de nombres con el mismo nombre completo contribuyen al mismo espacio de declaración ([declaraciones](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="0c042-135">Namespaces are open-ended, and two namespace declarations with the same fully qualified name contribute to the same declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="0c042-136">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c042-136">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
<span data-ttu-id="0c042-137">Las dos declaraciones de espacio de nombres anteriores contribuyen al mismo espacio de declaración, en este caso declarar dos clases con los nombres completos `N1.N2.A` y `N1.N2.B`.</span><span class="sxs-lookup"><span data-stu-id="0c042-137">the two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `N1.N2.A` and `N1.N2.B`.</span></span> <span data-ttu-id="0c042-138">Dado que las dos declaraciones contribuyen al mismo espacio de declaración, habría sido un error si cada uno contiene una declaración de un miembro con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="0c042-138">Because the two declarations contribute to the same declaration space, it would have been an error if each contained a declaration of a member with the same name.</span></span>

## <a name="extern-aliases"></a><span data-ttu-id="0c042-139">Alias externos</span><span class="sxs-lookup"><span data-stu-id="0c042-139">Extern aliases</span></span>

<span data-ttu-id="0c042-140">Un *extern_alias_directive* presenta un identificador que actúa como un alias para un espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-140">An *extern_alias_directive* introduces an identifier that serves as an alias for a namespace.</span></span> <span data-ttu-id="0c042-141">La especificación del espacio de nombres con alias es externa al código fuente del programa y se aplica también a los espacios de nombres anidados del espacio de nombres con alias.</span><span class="sxs-lookup"><span data-stu-id="0c042-141">The specification of the aliased namespace is external to the source code of the program and applies also to nested namespaces of the aliased namespace.</span></span>

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

<span data-ttu-id="0c042-142">El ámbito de un *extern_alias_directive* se extiende a través de la *using_directive*s, *global_attributes* y *namespace_member_declaration*s de su cuerpo contenedor inmediato de espacio de nombres o la unidad de compilación.</span><span class="sxs-lookup"><span data-stu-id="0c042-142">The scope of an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span>

<span data-ttu-id="0c042-143">Dentro de un cuerpo de espacio de nombres o la unidad de compilación que contiene un *extern_alias_directive*, el identificador definido por el *extern_alias_directive* puede usarse para hacer referencia el espacio de nombres con alias.</span><span class="sxs-lookup"><span data-stu-id="0c042-143">Within a compilation unit or namespace body that contains an *extern_alias_directive*, the identifier introduced by the *extern_alias_directive* can be used to reference the aliased namespace.</span></span> <span data-ttu-id="0c042-144">Es un error en tiempo de compilación para la *identificador* debe ser la palabra `global`.</span><span class="sxs-lookup"><span data-stu-id="0c042-144">It is a compile-time error for the *identifier* to be the word `global`.</span></span>

<span data-ttu-id="0c042-145">Un *extern_alias_directive* ofrece un alias dentro de un cuerpo de espacio de nombres o la unidad de compilación concreta, pero no aporta ningún miembro nuevo al espacio de declaración subyacente.</span><span class="sxs-lookup"><span data-stu-id="0c042-145">An *extern_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="0c042-146">En otras palabras, un *extern_alias_directive* no es transitiva, pero, en su lugar, sólo afecta a la compilación unidad o el espacio de nombres de cuerpo en el que se produce.</span><span class="sxs-lookup"><span data-stu-id="0c042-146">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>

<span data-ttu-id="0c042-147">El programa siguiente declara y usa dos alias externos, `X` y `Y`, cada uno de los que representan la raíz de una jerarquía de espacio de nombres distinto:</span><span class="sxs-lookup"><span data-stu-id="0c042-147">The following program declares and uses two extern aliases, `X` and `Y`, each of which represent the root of a distinct namespace hierarchy:</span></span>
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

<span data-ttu-id="0c042-148">El programa declara la existencia de la extern alias `X` y `Y`, pero las definiciones actuales de los alias son externas al programa.</span><span class="sxs-lookup"><span data-stu-id="0c042-148">The program declares the existence of the extern aliases `X` and `Y`, but the actual definitions of the aliases are external to the program.</span></span> <span data-ttu-id="0c042-149">Con el mismo nombre `N.B` ahora se pueden hacer referencia a las clases como `X.N.B` y `Y.N.B`, o bien, mediante el calificador de alias de espacio de nombres, `X::N.B` y `Y::N.B`.</span><span class="sxs-lookup"><span data-stu-id="0c042-149">The identically named `N.B` classes can now be referenced as `X.N.B` and `Y.N.B`, or, using the namespace alias qualifier, `X::N.B` and `Y::N.B`.</span></span> <span data-ttu-id="0c042-150">Se produce un error si un programa declara un alias externo para el que no se proporciona ninguna definición externa.</span><span class="sxs-lookup"><span data-stu-id="0c042-150">An error occurs if a program declares an extern alias for which no external definition is provided.</span></span>

## <a name="using-directives"></a><span data-ttu-id="0c042-151">directivas using</span><span class="sxs-lookup"><span data-stu-id="0c042-151">Using directives</span></span>

<span data-ttu-id="0c042-152">***Directivas using*** facilitan el uso de espacios de nombres y tipos definidos en otros espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-152">***Using directives*** facilitate the use of namespaces and types defined in other namespaces.</span></span> <span data-ttu-id="0c042-153">Mediante el impacto de las directivas en el proceso de resolución de nombres de *namespace_or_type_name*s ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)) y *simple_name*s ([nombres simples ](expressions.md#simple-names)), pero a diferencia de las declaraciones, las directivas using no contribuyen nuevos miembros a los espacios de declaración subyacentes de las unidades de compilación o espacios de nombres en el que se usan.</span><span class="sxs-lookup"><span data-stu-id="0c042-153">Using directives impact the name resolution process of *namespace_or_type_name*s ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) and *simple_name*s ([Simple names](expressions.md#simple-names)), but unlike declarations, using directives do not contribute new members to the underlying declaration spaces of the compilation units or namespaces within which they are used.</span></span>

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

<span data-ttu-id="0c042-154">Un *using_alias_directive* ([directivas Using alias](namespaces.md#using-alias-directives)) introduce un alias para un espacio de nombres o tipo.</span><span class="sxs-lookup"><span data-stu-id="0c042-154">A *using_alias_directive* ([Using alias directives](namespaces.md#using-alias-directives)) introduces an alias for a namespace or type.</span></span>

<span data-ttu-id="0c042-155">Un *using_namespace_directive* ([directivas de espacio de nombres Using](namespaces.md#using-namespace-directives)) importa los miembros de tipo de un espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-155">A *using_namespace_directive* ([Using namespace directives](namespaces.md#using-namespace-directives)) imports the type members of a namespace.</span></span>

<span data-ttu-id="0c042-156">Un *using_static_directive* ([directivas Using static](namespaces.md#using-static-directives)) importa los tipos anidados y miembros estáticos de un tipo.</span><span class="sxs-lookup"><span data-stu-id="0c042-156">A *using_static_directive* ([Using static directives](namespaces.md#using-static-directives)) imports the nested types and static members of a type.</span></span>

<span data-ttu-id="0c042-157">El ámbito de un *using_directive* se extiende a través de la *namespace_member_declaration*s de su cuerpo contenedor inmediato de espacio de nombres o la unidad de compilación.</span><span class="sxs-lookup"><span data-stu-id="0c042-157">The scope of a *using_directive* extends over the *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="0c042-158">El ámbito de un *using_directive* específicamente no incluye su interlocutor *using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="0c042-158">The scope of a *using_directive* specifically does not include its peer *using_directive*s.</span></span> <span data-ttu-id="0c042-159">Por lo tanto, del mismo nivel *using_directive*s no afectan a entre sí y el orden en el que se escriben es insignificante.</span><span class="sxs-lookup"><span data-stu-id="0c042-159">Thus, peer *using_directive*s do not affect each other, and the order in which they are written is insignificant.</span></span>

### <a name="using-alias-directives"></a><span data-ttu-id="0c042-160">Directivas de alias using</span><span class="sxs-lookup"><span data-stu-id="0c042-160">Using alias directives</span></span>

<span data-ttu-id="0c042-161">Un *using_alias_directive* presenta un identificador que actúa como un alias para un espacio de nombres o tipo dentro del cuerpo de espacio de nombres o la unidad de compilación inmediatamente envolvente.</span><span class="sxs-lookup"><span data-stu-id="0c042-161">A *using_alias_directive* introduces an identifier that serves as an alias for a namespace or type within the immediately enclosing compilation unit or namespace body.</span></span>

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

<span data-ttu-id="0c042-162">Dentro de las declaraciones de miembros en un cuerpo de espacio de nombres o la unidad de compilación que contiene un *using_alias_directive*, el identificador definido por el *using_alias_directive* puede usarse para hacer referencia a la determinada espacio de nombres o tipo.</span><span class="sxs-lookup"><span data-stu-id="0c042-162">Within member declarations in a compilation unit or namespace body that contains a *using_alias_directive*, the identifier introduced by the *using_alias_directive* can be used to reference the given namespace or type.</span></span> <span data-ttu-id="0c042-163">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c042-163">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

<span data-ttu-id="0c042-164">Anterior, dentro de las declaraciones de miembros en el `N3` espacio de nombres, `A` es un alias para `N1.N2.A`y, por tanto, la clase `N3.B` se deriva de la clase `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="0c042-164">Above, within member declarations in the `N3` namespace, `A` is an alias for `N1.N2.A`, and thus class `N3.B` derives from class `N1.N2.A`.</span></span> <span data-ttu-id="0c042-165">El mismo efecto que puede obtenerse mediante la creación de un alias `R` para `N1.N2` y, a continuación, hacer referencia a `R.A`:</span><span class="sxs-lookup"><span data-stu-id="0c042-165">The same effect can be obtained by creating an alias `R` for `N1.N2` and then referencing `R.A`:</span></span>
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

<span data-ttu-id="0c042-166">El *identificador* de un *using_alias_directive* debe ser único dentro del espacio de declaración de la unidad de compilación o espacio de nombres que contiene inmediatamente la *using_alias_directive* .</span><span class="sxs-lookup"><span data-stu-id="0c042-166">The *identifier* of a *using_alias_directive* must be unique within the declaration space of the compilation unit or namespace that immediately contains the *using_alias_directive*.</span></span> <span data-ttu-id="0c042-167">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c042-167">For example:</span></span>
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

<span data-ttu-id="0c042-168">Anterior, `N3` ya contiene un miembro `A`, por lo que es un error en tiempo de compilación para un *using_alias_directive* utilice ese identificador.</span><span class="sxs-lookup"><span data-stu-id="0c042-168">Above, `N3` already contains a member `A`, so it is a compile-time error for a *using_alias_directive* to use that identifier.</span></span> <span data-ttu-id="0c042-169">Del mismo modo, es un error en tiempo de compilación para dos o más *using_alias_directive*s en el cuerpo mismo de espacio de nombres o la unidad de compilación para declarar los alias con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="0c042-169">Likewise, it is a compile-time error for two or more *using_alias_directive*s in the same compilation unit or namespace body to declare aliases by the same name.</span></span>

<span data-ttu-id="0c042-170">Un *using_alias_directive* ofrece un alias dentro de un cuerpo de espacio de nombres o la unidad de compilación concreta, pero no aporta ningún miembro nuevo al espacio de declaración subyacente.</span><span class="sxs-lookup"><span data-stu-id="0c042-170">A *using_alias_directive* makes an alias available within a particular compilation unit or namespace body, but it does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="0c042-171">En otras palabras, un *using_alias_directive* no es transitiva, pero en su lugar sólo afecta a la compilación unidad o el espacio de nombres de cuerpo en el que se produce.</span><span class="sxs-lookup"><span data-stu-id="0c042-171">In other words, a *using_alias_directive* is not transitive but rather affects only the compilation unit or namespace body in which it occurs.</span></span> <span data-ttu-id="0c042-172">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c042-172">In the example</span></span>
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
<span data-ttu-id="0c042-173">el ámbito de la *using_alias_directive* que presenta `R` sólo se extiende a las declaraciones de miembros en el cuerpo de espacio de nombres que lo contiene, por lo que `R` es desconocido en la segunda declaración de espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-173">the scope of the *using_alias_directive* that introduces `R` only extends to member declarations in the namespace body in which it is contained, so `R` is unknown in the second namespace declaration.</span></span> <span data-ttu-id="0c042-174">Sin embargo, colocar el *using_alias_directive* en la compilación que lo contiene unidad hace que el alias para que estén disponibles en ambas declaraciones de espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="0c042-174">However, placing the *using_alias_directive* in the containing compilation unit causes the alias to become available within both namespace declarations:</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

<span data-ttu-id="0c042-175">Al igual que los miembros normales, los nombres introducen por *using_alias_directive*s se ocultan los miembros con el mismo nombre en ámbitos anidados.</span><span class="sxs-lookup"><span data-stu-id="0c042-175">Just like regular members, names introduced by *using_alias_directive*s are hidden by similarly named members in nested scopes.</span></span> <span data-ttu-id="0c042-176">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c042-176">In the example</span></span>
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
<span data-ttu-id="0c042-177">la referencia a `R.A` en la declaración de `B` genera un error de tiempo de compilación porque `R` hace referencia a `N3.R`, no `N1.N2`.</span><span class="sxs-lookup"><span data-stu-id="0c042-177">the reference to `R.A` in the declaration of `B` causes a compile-time error because `R` refers to `N3.R`, not `N1.N2`.</span></span>

<span data-ttu-id="0c042-178">El orden en que *using_alias_directive*s se escriben no tiene ninguna importancia y la resolución de la *namespace_or_type_name* hace referencia a él un *using_alias_directive*no se ve afectado por la *using_alias_directive* propio o por otro *using_directive*s en el cuerpo de espacio de nombres o la unidad de compilación contenedor inmediato.</span><span class="sxs-lookup"><span data-stu-id="0c042-178">The order in which *using_alias_directive*s are written has no significance, and resolution of the *namespace_or_type_name* referenced by a *using_alias_directive* is not affected by the *using_alias_directive* itself or by other *using_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="0c042-179">En otras palabras, el *namespace_or_type_name* de un *using_alias_directive* se resuelve como si el cuerpo de espacio de nombres o la unidad de compilación contenedor inmediato no tuviese *using_directive*s.</span><span class="sxs-lookup"><span data-stu-id="0c042-179">In other words, the *namespace_or_type_name* of a *using_alias_directive* is resolved as if the immediately containing compilation unit or namespace body had no *using_directive*s.</span></span> <span data-ttu-id="0c042-180">Un *using_alias_directive* pero puede ser afectado por *extern_alias_directive*s en el cuerpo de espacio de nombres o la unidad de compilación contenedor inmediato.</span><span class="sxs-lookup"><span data-stu-id="0c042-180">A *using_alias_directive* may however be affected by *extern_alias_directive*s in the immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="0c042-181">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c042-181">In the example</span></span>
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
<span data-ttu-id="0c042-182">la última *using_alias_directive* da como resultado un error en tiempo de compilación porque no se ve afectado por la primera *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="0c042-182">the last *using_alias_directive* results in a compile-time error because it is not affected by the first *using_alias_directive*.</span></span> <span data-ttu-id="0c042-183">La primera *using_alias_directive* no produce un error desde el ámbito de lo alias externo `E` incluye la *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="0c042-183">The first *using_alias_directive* does not result in an error since the scope of the extern alias `E` includes the *using_alias_directive*.</span></span>

<span data-ttu-id="0c042-184">Un *using_alias_directive* puede crear un alias para cualquier espacio de nombres o tipo, incluido el espacio de nombres en el que aparece y cualquier espacio de nombres o tipo anidado dentro de ese espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-184">A *using_alias_directive* can create an alias for any namespace or type, including the namespace within which it appears and any namespace or type nested within that namespace.</span></span>

<span data-ttu-id="0c042-185">Acceso a un tipo o espacio de nombres a través de un alias produce exactamente el mismo resultado que obtener acceso a dicho espacio de nombres o tipo mediante su nombre declarado.</span><span class="sxs-lookup"><span data-stu-id="0c042-185">Accessing a namespace or type through an alias yields exactly the same result as accessing that namespace or type through its declared name.</span></span> <span data-ttu-id="0c042-186">Por ejemplo, dada</span><span class="sxs-lookup"><span data-stu-id="0c042-186">For example, given</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
<span data-ttu-id="0c042-187">los nombres `N1.N2.A`, `R1.N2.A`, y `R2.A` son equivalentes y todos hacen referencia a la clase cuyo nombre completo es `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="0c042-187">the names `N1.N2.A`, `R1.N2.A`, and `R2.A` are equivalent and all refer to the class whose fully qualified name is `N1.N2.A`.</span></span>

<span data-ttu-id="0c042-188">El uso de alias puede nombre de un tipo construido cerrado, pero no el nombre de una declaración de tipo genérico sin enlazar sin proporcionar argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="0c042-188">Using aliases can name a closed constructed type, but cannot name an unbound generic type declaration without supplying type arguments.</span></span> <span data-ttu-id="0c042-189">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c042-189">For example:</span></span>
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a><span data-ttu-id="0c042-190">Espacio de nombres directivas using</span><span class="sxs-lookup"><span data-stu-id="0c042-190">Using namespace directives</span></span>

<span data-ttu-id="0c042-191">Un *using_namespace_directive* importa los tipos contenidos en un espacio de nombres en la compilación cuerpo inmediatamente envolvente unidad o el espacio de nombres, lo que permite el identificador de cada tipo que se puede usar sin calificación.</span><span class="sxs-lookup"><span data-stu-id="0c042-191">A *using_namespace_directive* imports the types contained in a namespace into the immediately enclosing compilation unit or namespace body, enabling the identifier of each type to be used without qualification.</span></span>

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

<span data-ttu-id="0c042-192">Dentro de las declaraciones de miembros en un cuerpo de espacio de nombres o la unidad de compilación que contiene un *using_namespace_directive*, pueden hacer referencia directamente a los tipos contenidos en el espacio de nombres especificado.</span><span class="sxs-lookup"><span data-stu-id="0c042-192">Within member declarations in a compilation unit or namespace body that contains a *using_namespace_directive*, the types contained in the given namespace can be referenced directly.</span></span> <span data-ttu-id="0c042-193">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c042-193">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

<span data-ttu-id="0c042-194">Anterior, dentro de las declaraciones de miembros en el `N3` espacio de nombres, los miembros de tipo de `N1.N2` están directamente disponibles y, por tanto, la clase `N3.B` se deriva de la clase `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="0c042-194">Above, within member declarations in the `N3` namespace, the type members of `N1.N2` are directly available, and thus class `N3.B` derives from class `N1.N2.A`.</span></span>

<span data-ttu-id="0c042-195">Un *using_namespace_directive* importa los tipos contenidos en el espacio de nombres especificado, pero no importa específicamente los espacios de nombres anidados.</span><span class="sxs-lookup"><span data-stu-id="0c042-195">A *using_namespace_directive* imports the types contained in the given namespace, but specifically does not import nested namespaces.</span></span> <span data-ttu-id="0c042-196">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c042-196">In the example</span></span>
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
<span data-ttu-id="0c042-197">el *using_namespace_directive* importa los tipos contenidos en `N1`, pero no los espacios de nombres anidados en `N1`.</span><span class="sxs-lookup"><span data-stu-id="0c042-197">the *using_namespace_directive* imports the types contained in `N1`, but not the namespaces nested in `N1`.</span></span> <span data-ttu-id="0c042-198">Por lo tanto, la referencia a `N2.A` en la declaración de `B` da como resultado un error en tiempo de compilación porque ningún miembro llamado `N2` están en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="0c042-198">Thus, the reference to `N2.A` in the declaration of `B` results in a compile-time error because no members named `N2` are in scope.</span></span>

<span data-ttu-id="0c042-199">A diferencia de un *using_alias_directive*, un *using_namespace_directive* puede importar tipos cuyos identificadores ya están definidos dentro del cuerpo de espacio de nombres o la unidad de compilación envolvente.</span><span class="sxs-lookup"><span data-stu-id="0c042-199">Unlike a *using_alias_directive*, a *using_namespace_directive* may import types whose identifiers are already defined within the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="0c042-200">De hecho, los nombres importan por un *using_namespace_directive* están ocultos por miembros con el mismo nombre en el cuerpo de espacio de nombres o la unidad de compilación envolvente.</span><span class="sxs-lookup"><span data-stu-id="0c042-200">In effect, names imported by a *using_namespace_directive* are hidden by similarly named members in the enclosing compilation unit or namespace body.</span></span> <span data-ttu-id="0c042-201">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c042-201">For example:</span></span>
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

<span data-ttu-id="0c042-202">Aquí, en las declaraciones de miembros en el `N3` espacio de nombres, `A` hace referencia a `N3.A` lugar `N1.N2.A`.</span><span class="sxs-lookup"><span data-stu-id="0c042-202">Here, within member declarations in the `N3` namespace, `A` refers to `N3.A` rather than `N1.N2.A`.</span></span>

<span data-ttu-id="0c042-203">Cuando más de un espacio de nombres o tipo importado por *using_namespace_directive*s o *using_static_directive*s en el mismo cuerpo de espacio de nombres o la unidad de compilación contiene tipos para el mismo nombre, las referencias a ese nombre como un *type_name* se consideran ambiguos.</span><span class="sxs-lookup"><span data-stu-id="0c042-203">When more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types by the same name, references to that name as a *type_name* are considered ambiguous.</span></span> <span data-ttu-id="0c042-204">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c042-204">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
<span data-ttu-id="0c042-205">ambos `N1` y `N2` contienen un miembro `A`y porque `N3` importa ambos, que hacen referencia a `A` en `N3` es un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0c042-205">both `N1` and `N2` contain a member `A`, and because `N3` imports both, referencing `A` in `N3` is a compile-time error.</span></span> <span data-ttu-id="0c042-206">En esta situación, el conflicto se puede resolver mediante la calificación de referencias a `A`, o bien introduciendo un *using_alias_directive* que toma un determinado `A`.</span><span class="sxs-lookup"><span data-stu-id="0c042-206">In this situation, the conflict can be resolved either through qualification of references to `A`, or by introducing a *using_alias_directive* that picks a particular `A`.</span></span> <span data-ttu-id="0c042-207">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c042-207">For example:</span></span>
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

<span data-ttu-id="0c042-208">Además, cuando más de un espacio de nombres o tipo importado por *using_namespace_directive*s o *using_static_directive*s en el mismo cuerpo de espacio de nombres o la unidad de compilación contiene tipos o miembros mediante el mismo nombre, las referencias a ese nombre como un *simple_name* se consideran ambiguos.</span><span class="sxs-lookup"><span data-stu-id="0c042-208">Furthermore, when more than one namespace or type imported by *using_namespace_directive*s or *using_static_directive*s in the same compilation unit or namespace body contain types or members by the same name, references to that name as a *simple_name* are considered ambiguous.</span></span> <span data-ttu-id="0c042-209">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c042-209">In the example</span></span>
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
<span data-ttu-id="0c042-210">`N1` contiene un miembro de tipo `A`, y `C` contiene un método estático `A`y porque `N2` importa ambos, que hacen referencia a `A` como un *simple_name* es ambiguo y un tiempo de compilación error.</span><span class="sxs-lookup"><span data-stu-id="0c042-210">`N1` contains a type member `A`, and `C` contains a static method `A`, and because `N2` imports both, referencing `A` as a *simple_name* is ambiguous and a compile-time error.</span></span> 

<span data-ttu-id="0c042-211">Al igual que un *using_alias_directive*, un *using_namespace_directive* no contribuye con ningún miembro nuevo al espacio de declaración subyacente de la unidad de compilación o espacio de nombres, sino que afecta a solo el compilación cuerpo unidad o el espacio de nombres en el que aparece.</span><span class="sxs-lookup"><span data-stu-id="0c042-211">Like a *using_alias_directive*, a *using_namespace_directive* does not contribute any new members to the underlying declaration space of the compilation unit or namespace, but rather affects only the compilation unit or namespace body in which it appears.</span></span>

<span data-ttu-id="0c042-212">El *namespace_name* hace referencia a él un *using_namespace_directive* se resuelve en la misma manera que el *namespace_or_type_name* hace referencia a él un  *using_alias_directive*.</span><span class="sxs-lookup"><span data-stu-id="0c042-212">The *namespace_name* referenced by a *using_namespace_directive* is resolved in the same way as the *namespace_or_type_name* referenced by a *using_alias_directive*.</span></span> <span data-ttu-id="0c042-213">Por lo tanto, *using_namespace_directive*s en el mismo cuerpo de espacio de nombres o la unidad de compilación no afectan a entre sí y pueden escribirse en cualquier orden.</span><span class="sxs-lookup"><span data-stu-id="0c042-213">Thus, *using_namespace_directive*s in the same compilation unit or namespace body do not affect each other and can be written in any order.</span></span>

### <a name="using-static-directives"></a><span data-ttu-id="0c042-214">Directivas using static</span><span class="sxs-lookup"><span data-stu-id="0c042-214">Using static directives</span></span>

<span data-ttu-id="0c042-215">Un *using_static_directive* importa los tipos anidados y miembros estáticos contenidos directamente en una declaración de tipo en el cuerpo inmediatamente envolvente compilación unidad o el espacio de nombres, lo que permite el identificador de cada miembro y tipo que utilizar sin calificación.</span><span class="sxs-lookup"><span data-stu-id="0c042-215">A *using_static_directive* imports the nested types and static members contained directly in a type declaration into the immediately enclosing compilation unit or namespace body, enabling the identifier of each member and type to be used without qualification.</span></span>

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

<span data-ttu-id="0c042-216">Dentro de las declaraciones de miembros en un cuerpo de espacio de nombres o la unidad de compilación que contiene un *using_static_directive*, el accesible anidar tipos y miembros estáticos (excepto los métodos de extensión) contenidos directamente en la declaración de la tipo especificado se puede hacer referencia directamente.</span><span class="sxs-lookup"><span data-stu-id="0c042-216">Within member declarations in a compilation unit or namespace body that contains a *using_static_directive*, the accessible nested types and static members (except extension methods) contained directly in the declaration of the given type can be referenced directly.</span></span> <span data-ttu-id="0c042-217">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c042-217">For example:</span></span>

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

<span data-ttu-id="0c042-218">Anterior, dentro de las declaraciones de miembros en el `N2` espacio de nombres, los miembros estáticos y los tipos anidados de `N1.A` están directamente disponibles y, por tanto, el método `N` es capaz de hacer referencia tanto la `B` y `M` los miembros de `N1.A`.</span><span class="sxs-lookup"><span data-stu-id="0c042-218">Above, within member declarations in the `N2` namespace, the static members and nested types of `N1.A` are directly available, and thus the method `N` is able to reference both the `B` and `M` members of `N1.A`.</span></span>

<span data-ttu-id="0c042-219">Un *using_static_directive* específicamente no importa los métodos de extensión directamente como métodos estáticos, pero hace que estén disponibles para la invocación del método de extensión ([las invocaciones de método de extensión](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="0c042-219">A *using_static_directive* specifically does not import extension methods directly as static methods, but makes them available for extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="0c042-220">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c042-220">In the example</span></span>

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
<span data-ttu-id="0c042-221">el *using_static_directive* importa el método de extensión `M` contenidos en `N1.A`, sino únicamente como un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="0c042-221">the *using_static_directive* imports the extension method `M` contained in `N1.A`, but only as an extension method.</span></span> <span data-ttu-id="0c042-222">Por lo tanto, la primera referencia a `M` en el cuerpo de `B.N` da como resultado un error en tiempo de compilación porque ningún miembro llamado `M` están en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="0c042-222">Thus, the first reference to `M` in the body of `B.N` results in a compile-time error because no members named `M` are in scope.</span></span>

<span data-ttu-id="0c042-223">Un *using_static_directive* sólo importa los miembros y tipos declarados directamente en el tipo especificado, no a los tipos y miembros declaran en las clases base.</span><span class="sxs-lookup"><span data-stu-id="0c042-223">A *using_static_directive* only imports members and types declared directly in the given type, not members and types declared in base classes.</span></span>

<span data-ttu-id="0c042-224">TODO: ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c042-224">TODO: Example</span></span>

<span data-ttu-id="0c042-225">Las ambigüedades entre varias *using_namespace_directives* y *using_static_directives* se tratan en [mediante directivas de espacio de nombres](namespaces.md#using-namespace-directives).</span><span class="sxs-lookup"><span data-stu-id="0c042-225">Ambiguities between multiple *using_namespace_directives* and *using_static_directives* are discussed in [Using namespace directives](namespaces.md#using-namespace-directives).</span></span>

## <a name="namespace-members"></a><span data-ttu-id="0c042-226">Miembros de Namespace</span><span class="sxs-lookup"><span data-stu-id="0c042-226">Namespace members</span></span>

<span data-ttu-id="0c042-227">Un *namespace_member_declaration* sea un *namespace_declaration* ([declaraciones Namespace](namespaces.md#namespace-declarations)) o un *type_declaration* () [Declaraciones de tipo](namespaces.md#type-declarations)).</span><span class="sxs-lookup"><span data-stu-id="0c042-227">A *namespace_member_declaration* is either a *namespace_declaration* ([Namespace declarations](namespaces.md#namespace-declarations)) or a *type_declaration* ([Type declarations](namespaces.md#type-declarations)).</span></span>

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

<span data-ttu-id="0c042-228">Una unidad de compilación o un cuerpo de espacio de nombres puede contener *namespace_member_declaration*s y dichas declaraciones contribuyen nuevos miembros al espacio de declaración subyacente del cuerpo de espacio de nombres o la unidad de compilación que contiene.</span><span class="sxs-lookup"><span data-stu-id="0c042-228">A compilation unit or a namespace body can contain *namespace_member_declaration*s, and such declarations contribute new members to the underlying declaration space of the containing compilation unit or namespace body.</span></span>

## <a name="type-declarations"></a><span data-ttu-id="0c042-229">Declaraciones de tipos</span><span class="sxs-lookup"><span data-stu-id="0c042-229">Type declarations</span></span>

<span data-ttu-id="0c042-230">Un *type_declaration* es un *class_declaration* ([declaraciones de clase](classes.md#class-declarations)), un *struct_declaration* ([Struct las declaraciones](structs.md#struct-declarations)), un *interface_declaration* ([declaraciones de interfaz](interfaces.md#interface-declarations)), un *enum_declaration* ([Enum las declaraciones](enums.md#enum-declarations)), o un *delegate_declaration* ([declaraciones de delegado](delegates.md#delegate-declarations)).</span><span class="sxs-lookup"><span data-stu-id="0c042-230">A *type_declaration* is a *class_declaration* ([Class declarations](classes.md#class-declarations)), a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)), an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)), an *enum_declaration* ([Enum declarations](enums.md#enum-declarations)), or a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)).</span></span>

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

<span data-ttu-id="0c042-231">Un *type_declaration* puede ocurrir como una declaración de nivel superior en una unidad de compilación o como una declaración de miembro dentro de un espacio de nombres, clase o struct.</span><span class="sxs-lookup"><span data-stu-id="0c042-231">A *type_declaration* can occur as a top-level declaration in a compilation unit or as a member declaration within a namespace, class, or struct.</span></span>

<span data-ttu-id="0c042-232">Cuando una declaración de tipos para un tipo `T` se produce como una declaración de nivel superior en una unidad de compilación, el nombre completo del nuevo tipo declarado es simplemente `T`.</span><span class="sxs-lookup"><span data-stu-id="0c042-232">When a type declaration for a type `T` occurs as a top-level declaration in a compilation unit, the fully qualified name of the newly declared type is simply `T`.</span></span> <span data-ttu-id="0c042-233">Cuando una declaración de tipos para un tipo `T` se produce dentro de un espacio de nombres, clase o struct, el nombre completo del nuevo tipo declarado es `N.T`, donde `N` es el nombre completo del espacio de nombres, clase o estructura contenedora.</span><span class="sxs-lookup"><span data-stu-id="0c042-233">When a type declaration for a type `T` occurs within a namespace, class, or struct, the fully qualified name of the newly declared type is `N.T`, where `N` is the fully qualified name of the containing namespace, class, or struct.</span></span>

<span data-ttu-id="0c042-234">Un tipo declarado dentro de una clase o struct se denomina tipo anidado ([tipos anidados](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="0c042-234">A type declared within a class or struct is called a nested type ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="0c042-235">Los modificadores de acceso permitido y el acceso predeterminado para una declaración de tipo dependen del contexto en el que tiene lugar la declaración ([accesibilidad declarada](basic-concepts.md#declared-accessibility)):</span><span class="sxs-lookup"><span data-stu-id="0c042-235">The permitted access modifiers and the default access for a type declaration depend on the context in which the declaration takes place ([Declared accessibility](basic-concepts.md#declared-accessibility)):</span></span>

*  <span data-ttu-id="0c042-236">Pueden tener tipos declarados en unidades de compilación o espacios de nombres `public` o `internal` acceso.</span><span class="sxs-lookup"><span data-stu-id="0c042-236">Types declared in compilation units or namespaces can have `public` or `internal` access.</span></span> <span data-ttu-id="0c042-237">El valor predeterminado es `internal` acceso.</span><span class="sxs-lookup"><span data-stu-id="0c042-237">The default is `internal` access.</span></span>
*  <span data-ttu-id="0c042-238">Los tipos declarados en las clases pueden tener `public`, `protected internal`, `protected`, `internal`, o `private` acceso.</span><span class="sxs-lookup"><span data-stu-id="0c042-238">Types declared in classes can have `public`, `protected internal`, `protected`, `internal`, or `private` access.</span></span> <span data-ttu-id="0c042-239">El valor predeterminado es `private` acceso.</span><span class="sxs-lookup"><span data-stu-id="0c042-239">The default is `private` access.</span></span>
*  <span data-ttu-id="0c042-240">Los tipos declarados en estructuras pueden tener `public`, `internal`, o `private` acceso.</span><span class="sxs-lookup"><span data-stu-id="0c042-240">Types declared in structs can have `public`, `internal`, or `private` access.</span></span> <span data-ttu-id="0c042-241">El valor predeterminado es `private` acceso.</span><span class="sxs-lookup"><span data-stu-id="0c042-241">The default is `private` access.</span></span>

## <a name="namespace-alias-qualifiers"></a><span data-ttu-id="0c042-242">Calificadores de alias Namespace</span><span class="sxs-lookup"><span data-stu-id="0c042-242">Namespace alias qualifiers</span></span>

<span data-ttu-id="0c042-243">El ***calificador de alias de espacio de nombres*** `::` hace posible garantizar que las búsquedas de nombres de tipo no se ven afectadas por la introducción de nuevos tipos y miembros.</span><span class="sxs-lookup"><span data-stu-id="0c042-243">The ***namespace alias qualifier*** `::` makes it possible to guarantee that type name lookups are unaffected by the introduction of new types and members.</span></span> <span data-ttu-id="0c042-244">El calificador de alias de espacio de nombres siempre aparece entre dos identificadores que se conoce como los identificadores de la izquierda y derecha.</span><span class="sxs-lookup"><span data-stu-id="0c042-244">The namespace alias qualifier always appears between two identifiers referred to as the left-hand and right-hand identifiers.</span></span> <span data-ttu-id="0c042-245">A diferencia de las tarifas `.` calificador, el identificador izquierdo de la `::` calificador se busca sólo como un alias de using o extern.</span><span class="sxs-lookup"><span data-stu-id="0c042-245">Unlike the regular `.` qualifier, the left-hand identifier of the `::` qualifier is looked up only as an extern or using alias.</span></span>

<span data-ttu-id="0c042-246">Un *qualified_alias_member* se define como sigue:</span><span class="sxs-lookup"><span data-stu-id="0c042-246">A *qualified_alias_member* is defined as follows:</span></span>

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

<span data-ttu-id="0c042-247">Un *qualified_alias_member* puede usarse como un *namespace_or_type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)) o como el operando izquierdo de un *member_access* ([Acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="0c042-247">A *qualified_alias_member* can be used as a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) or as the left operand in a *member_access* ([Member access](expressions.md#member-access)).</span></span>

<span data-ttu-id="0c042-248">Un *qualified_alias_member* tiene uno de dos formas:</span><span class="sxs-lookup"><span data-stu-id="0c042-248">A *qualified_alias_member* has one of two forms:</span></span>

*  <span data-ttu-id="0c042-249">`N::I<A1, ..., Ak>`, donde `N` y `I` representan identificadores, y `<A1, ..., Ak>` es una lista de argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="0c042-249">`N::I<A1, ..., Ak>`, where `N` and `I` represent identifiers, and `<A1, ..., Ak>` is a type argument list.</span></span> <span data-ttu-id="0c042-250">(`K` siempre es al menos uno.)</span><span class="sxs-lookup"><span data-stu-id="0c042-250">(`K` is always at least one.)</span></span>
*  <span data-ttu-id="0c042-251">`N::I`, donde `N` y `I` representan identificadores.</span><span class="sxs-lookup"><span data-stu-id="0c042-251">`N::I`, where `N` and `I` represent identifiers.</span></span> <span data-ttu-id="0c042-252">(En este caso, `K` se considera igual a cero.)</span><span class="sxs-lookup"><span data-stu-id="0c042-252">(In this case, `K` is considered to be zero.)</span></span>

<span data-ttu-id="0c042-253">Usa esta notación, el significado de un *qualified_alias_member* se determina como sigue:</span><span class="sxs-lookup"><span data-stu-id="0c042-253">Using this notation, the meaning of a *qualified_alias_member* is determined as follows:</span></span>

*  <span data-ttu-id="0c042-254">Si `N` es el identificador `global`, a continuación, se busca el espacio de nombres global `I`:</span><span class="sxs-lookup"><span data-stu-id="0c042-254">If `N` is the identifier `global`, then the global namespace is searched for `I`:</span></span>
   * <span data-ttu-id="0c042-255">Si el espacio de nombres global contiene un espacio de nombres denominado `I` y `K` es cero, el *qualified_alias_member* hace referencia a ese espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-255">If the global namespace contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
   * <span data-ttu-id="0c042-256">En caso contrario, si el espacio de nombres global contiene un tipo no genérico denominado `I` y `K` es cero, el *qualified_alias_member* hace referencia a ese tipo.</span><span class="sxs-lookup"><span data-stu-id="0c042-256">Otherwise, if the global namespace contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
   * <span data-ttu-id="0c042-257">En caso contrario, si el espacio de nombres global contiene un tipo denominado `I` cuya `K` parámetros de tipo, el *qualified_alias_member* hace referencia a ese tipo construido con los argumentos de tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="0c042-257">Otherwise, if the global namespace contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
   * <span data-ttu-id="0c042-258">En caso contrario, el *qualified_alias_member* es no definido y se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0c042-258">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

*  <span data-ttu-id="0c042-259">En caso contrario, empezando con la declaración de espacio de nombres ([declaraciones Namespace](namespaces.md#namespace-declarations)) inmediatamente que contiene el *qualified_alias_member* (si existe), continuando con cada declaración de espacio de nombres envolvente (si existe) y termina con la unidad de compilación que contiene el *qualified_alias_member*, se evalúan los pasos siguientes hasta que se encuentra una entidad:</span><span class="sxs-lookup"><span data-stu-id="0c042-259">Otherwise, starting with the namespace declaration ([Namespace declarations](namespaces.md#namespace-declarations)) immediately containing the *qualified_alias_member* (if any), continuing with each enclosing namespace declaration (if any), and ending with the compilation unit containing the *qualified_alias_member*, the following steps are evaluated until an entity is located:</span></span>

   * <span data-ttu-id="0c042-260">Si la unidad de compilación o de declaración de espacio de nombres contiene un *using_alias_directive* que asocia `N` con un tipo, el *qualified_alias_member* no está definido y un tiempo de compilación se produce un error.</span><span class="sxs-lookup"><span data-stu-id="0c042-260">If the namespace declaration or compilation unit contains a *using_alias_directive* that associates `N` with a type, then the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
   * <span data-ttu-id="0c042-261">En caso contrario, si la unidad de compilación o de declaración de espacio de nombres contiene un *extern_alias_directive* o *using_alias_directive* que asocia `N` con un espacio de nombres, a continuación:</span><span class="sxs-lookup"><span data-stu-id="0c042-261">Otherwise, if the namespace declaration or compilation unit contains an *extern_alias_directive* or *using_alias_directive* that associates `N` with a namespace, then:</span></span>
     * <span data-ttu-id="0c042-262">Si el espacio de nombres asociado `N` contiene un espacio de nombres denominado `I` y `K` es cero, el *qualified_alias_member* hace referencia a ese espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-262">If the namespace associated with `N` contains a namespace named `I` and `K` is zero, then the *qualified_alias_member* refers to that namespace.</span></span>
     * <span data-ttu-id="0c042-263">En caso contrario, si el espacio de nombres asociado `N` contiene un tipo no genérico denominado `I` y `K` es cero, el *qualified_alias_member* hace referencia a ese tipo.</span><span class="sxs-lookup"><span data-stu-id="0c042-263">Otherwise, if the namespace associated with `N` contains a non-generic type named `I` and `K` is zero, then the *qualified_alias_member* refers to that type.</span></span>
     * <span data-ttu-id="0c042-264">En caso contrario, si el espacio de nombres asociado `N` contiene un tipo denominado `I` cuya `K` parámetros de tipo, el *qualified_alias_member* hace referencia a ese tipo construido con el tipo especificado argumentos.</span><span class="sxs-lookup"><span data-stu-id="0c042-264">Otherwise, if the namespace associated with `N` contains a type named `I` that has `K` type parameters, then the *qualified_alias_member* refers to that type constructed with the given type arguments.</span></span>
     * <span data-ttu-id="0c042-265">En caso contrario, el *qualified_alias_member* es no definido y se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0c042-265">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="0c042-266">En caso contrario, el *qualified_alias_member* es no definido y se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0c042-266">Otherwise, the *qualified_alias_member* is undefined and a compile-time error occurs.</span></span>

<span data-ttu-id="0c042-267">Tenga en cuenta que uso el calificador de alias de espacio de nombres con un alias que hace referencia a un tipo produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="0c042-267">Note that using the namespace alias qualifier with an alias that references a type causes a compile-time error.</span></span> <span data-ttu-id="0c042-268">Observe también que si el identificador `N` es `global`, a continuación, se realiza una búsqueda en el espacio de nombres global, incluso si hay un alias using que asocia `global` con un tipo o espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-268">Also note that if the identifier `N` is `global`, then lookup is performed in the global namespace, even if there is a using alias associating `global` with a type or namespace.</span></span>

### <a name="uniqueness-of-aliases"></a><span data-ttu-id="0c042-269">Unicidad de los alias</span><span class="sxs-lookup"><span data-stu-id="0c042-269">Uniqueness of aliases</span></span>

<span data-ttu-id="0c042-270">Cada cuerpo de unidad y el espacio de nombres de compilación tiene un espacio de declaración independiente para los alias externos y el uso de alias.</span><span class="sxs-lookup"><span data-stu-id="0c042-270">Each compilation unit and namespace body has a separate declaration space for extern aliases and using aliases.</span></span> <span data-ttu-id="0c042-271">Por lo tanto, aunque el nombre de alias using o un alias externo debe ser único dentro del conjunto de alias externos y el uso de alias declarados en el cuerpo de espacio de nombres o la unidad de compilación contenedor inmediato, un alias puede tener el mismo nombre que un tipo o espacio de nombres siempre y cuando t solo se usa con el `::` calificador.</span><span class="sxs-lookup"><span data-stu-id="0c042-271">Thus, while the name of an extern alias or using alias must be unique within the set of extern aliases and using aliases declared in the immediately containing compilation unit or namespace body, an alias is permitted to have the same name as a type or namespace as long as it is used only with the `::` qualifier.</span></span>

<span data-ttu-id="0c042-272">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0c042-272">In the example</span></span>
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
<span data-ttu-id="0c042-273">el nombre `A` tiene dos significados posibles en el cuerpo de espacio de nombres de segundo, porque la clase `A` y el uso de alias `A` están en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="0c042-273">the name `A` has two possible meanings in the second namespace body because both the class `A` and the using alias `A` are in scope.</span></span> <span data-ttu-id="0c042-274">Para ello, el uso de `A` en el nombre completo `A.Stream` es ambigua y produce un error de tiempo de compilación que se produzca.</span><span class="sxs-lookup"><span data-stu-id="0c042-274">For this reason, use of `A` in the qualified name `A.Stream` is ambiguous and causes a compile-time error to occur.</span></span> <span data-ttu-id="0c042-275">Sin embargo, usar de `A` con el `::` calificador no es un error porque `A` se busca sólo como un alias de espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c042-275">However, use of `A` with the `::` qualifier is not an error because `A` is looked up only as a namespace alias.</span></span>
