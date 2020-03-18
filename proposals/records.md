---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "79483881"
---
# <a name="records"></a><span data-ttu-id="3f0f5-101">records</span><span class="sxs-lookup"><span data-stu-id="3f0f5-101">records</span></span>

* <span data-ttu-id="3f0f5-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="3f0f5-102">[x] Proposed</span></span>
* <span data-ttu-id="3f0f5-103">[] Prototipo: [completo](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="3f0f5-103">[ ] Prototype: [Complete](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="3f0f5-104">[] Implementación: [en curso](https://github.com/dotnet/roslyn/BRANCH_NAME)</span><span class="sxs-lookup"><span data-stu-id="3f0f5-104">[ ] Implementation: [In Progress](https://github.com/dotnet/roslyn/BRANCH_NAME)</span></span>
* <span data-ttu-id="3f0f5-105">[] Specification: especificación de borrador entre comillas</span><span class="sxs-lookup"><span data-stu-id="3f0f5-105">[ ] Specification: Draft specification enclosed</span></span>

## <a name="summary"></a><span data-ttu-id="3f0f5-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="3f0f5-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="3f0f5-107">Los registros son un formulario de declaración nuevo y C# simplificado para tipos de clase y struct que combinan las ventajas de una serie de características más sencillas.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-107">Records are a new, simplified declaration form for C# class and struct types that combine the benefits of a number of simpler features.</span></span> <span data-ttu-id="3f0f5-108">Se describen las nuevas características (parámetros del receptor llamador y *with-Expression*s), se proporcionan la sintaxis y la semántica para las declaraciones de registro y, a continuación, se proporcionan algunos ejemplos.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-108">We describe the new features (caller-receiver parameters and *with-expression*s), give the syntax and semantics for record declarations, and then provide some examples.</span></span>


## <a name="motivation"></a><span data-ttu-id="3f0f5-109">Motivación</span><span class="sxs-lookup"><span data-stu-id="3f0f5-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="3f0f5-110">Un número significativo de declaraciones de tipo en C# son poco más que las colecciones de datos de tipo.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-110">A significant number of type declarations in C# are little more than aggregate collections of typed data.</span></span> <span data-ttu-id="3f0f5-111">Desafortunadamente, la declaración de estos tipos requiere una gran cantidad de código reutilizable.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-111">Unfortunately, declaring such types requires a great deal of boilerplate code.</span></span> <span data-ttu-id="3f0f5-112">*Los registros* de proporcionan un mecanismo para declarar un tipo de de de. para ello, se describen los miembros del agregado junto con el código adicional o las desviaciones del texto reutilizable habitual, si hay alguno.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-112">*Records* provide a mechanism for declaring a datatype by describing the members of the aggregate along with additional code or deviations from the usual boilerplate, if any.</span></span>

<span data-ttu-id="3f0f5-113">Vea los [ejemplos](#examples)siguientes.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-113">See [Examples](#examples), below.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3f0f5-114">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="3f0f5-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a><span data-ttu-id="3f0f5-115">parámetros del autor de la llamada</span><span class="sxs-lookup"><span data-stu-id="3f0f5-115">caller-receiver parameters</span></span>

<span data-ttu-id="3f0f5-116">Actualmente *, el argumento predeterminado* de un parámetro de método debe ser</span><span class="sxs-lookup"><span data-stu-id="3f0f5-116">Currently a method parameter's *default-argument* must be</span></span> 
- <span data-ttu-id="3f0f5-117">*expresión constante*; de</span><span class="sxs-lookup"><span data-stu-id="3f0f5-117">a *constant-expression*; or</span></span>
- <span data-ttu-id="3f0f5-118">una expresión con el formato `new S()` donde `S` es un tipo de valor; de</span><span class="sxs-lookup"><span data-stu-id="3f0f5-118">an expression of the form `new S()` where `S` is a value type; or</span></span>
- <span data-ttu-id="3f0f5-119">una expresión con el formato `default(S)` donde `S` es un tipo de valor</span><span class="sxs-lookup"><span data-stu-id="3f0f5-119">an expression of the form `default(S)` where `S` is a value type</span></span>

<span data-ttu-id="3f0f5-120">Lo ampliamos para agregar lo siguiente</span><span class="sxs-lookup"><span data-stu-id="3f0f5-120">We extend this to add the following</span></span>
- <span data-ttu-id="3f0f5-121">una expresión con el formato `this.Identifier`</span><span class="sxs-lookup"><span data-stu-id="3f0f5-121">an expression of the form `this.Identifier`</span></span>

<span data-ttu-id="3f0f5-122">Este nuevo formulario se denomina *argumento predeterminado del receptor del autor*de la llamada y solo se permite si se cumplen todos los elementos siguientes.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-122">This new form is called a *caller-receiver default-argument*, and is allowed only if all of the following are satisfied</span></span>
- <span data-ttu-id="3f0f5-123">El método en el que aparece es un método de instancia; etc</span><span class="sxs-lookup"><span data-stu-id="3f0f5-123">The method in which it appears is an instance method; and</span></span>
- <span data-ttu-id="3f0f5-124">La expresión `this.Identifier` enlaza a un miembro de instancia del tipo envolvente, que debe ser un campo o una propiedad; etc</span><span class="sxs-lookup"><span data-stu-id="3f0f5-124">The expression `this.Identifier` binds to an instance member of the enclosing type, which must be either a field or a property; and</span></span>
- <span data-ttu-id="3f0f5-125">El miembro al que se enlaza (y el descriptor de acceso `get`, si es una propiedad) es al menos tan accesible como el método; etc</span><span class="sxs-lookup"><span data-stu-id="3f0f5-125">The member to which it binds (and the `get` accessor, if it is a property) is at least as accessible as the method; and</span></span>
- <span data-ttu-id="3f0f5-126">El tipo de `this.Identifier` es convertible implícitamente por una identidad o conversión que acepta valores NULL en el tipo del parámetro (se trata de una restricción existente en el *argumento predeterminado*).</span><span class="sxs-lookup"><span data-stu-id="3f0f5-126">The type of `this.Identifier` is implicitly convertible by an identity or nullable conversion to the type of the parameter (this is an existing constraint on *default-argument*).</span></span>

<span data-ttu-id="3f0f5-127">Cuando se omite un argumento de una invocación de un miembro de función para un parámetro opcional correspondiente con un *argumento predeterminado del receptor del autor*de la llamada, se pasa implícitamente el valor del miembro del receptor.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-127">When an argument is omitted from an invocation of a function member for a corresponding optional parameter with a *caller-receiver default-argument*, the value of the receiver's member is implicitly passed.</span></span> 

> <span data-ttu-id="3f0f5-128">**Notas de diseño**: la razón principal del parámetro Caller-Receiver es admitir la *expresión with*.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-128">**Design Notes**: the main reason for the caller-receiver parameter is to support the *with-expression*.</span></span> <span data-ttu-id="3f0f5-129">La idea es que puede declarar un método como este.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-129">The idea is that you can declare a method like this</span></span>
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> <span data-ttu-id="3f0f5-130">y, a continuación, úsela como esta</span><span class="sxs-lookup"><span data-stu-id="3f0f5-130">and then use it like this</span></span>
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> <span data-ttu-id="3f0f5-131">Para crear un nuevo `Point` igual que una `Point` existente pero con el valor de `X` cambiado.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-131">To create a new `Point` just like an existing `Point` but with the value of `X` changed.</span></span>
> 
> <span data-ttu-id="3f0f5-132">Es una pregunta abierta si merece la pena agregar o no la forma sintáctica de la *expresión with* , ya que se admiten los parámetros del receptor del llamador, por lo que es posible hacerlo en *lugar de* hacerlo *además* de *with-Expression*.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-132">It is an open question whether or not the syntactic form of the *with-expression* is worth adding once we have support for caller-receiver parameters, so it is possible we would do this *instead of* rather than *in addition to* the *with-expression*.</span></span>

- <span data-ttu-id="3f0f5-133">[] **Problema abierto**: ¿Cuál es el orden en el que se evalúa un *argumento-receptor predeterminado del autor* de la llamada con respecto a otros argumentos?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-133">[ ] **Open issue**: What is the order in which a *caller-receiver default-argument* is evaluated with respect to other arguments?</span></span> <span data-ttu-id="3f0f5-134">¿Debemos decir que no está especificado?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-134">Should we say that it is unspecified?</span></span>

### <a name="with-expressions"></a><span data-ttu-id="3f0f5-135">Expresiones with</span><span class="sxs-lookup"><span data-stu-id="3f0f5-135">with-expressions</span></span>

<span data-ttu-id="3f0f5-136">Se propone un nuevo formulario de expresión:</span><span class="sxs-lookup"><span data-stu-id="3f0f5-136">A new expression form is proposed:</span></span>

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

<span data-ttu-id="3f0f5-137">El token `with` es una nueva palabra clave contextual.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-137">The token `with` is a new context-sensitive keyword.</span></span>

<span data-ttu-id="3f0f5-138">Cada *identificador* a la izquierda de un *with_initializer* debe enlazarse a un campo o propiedad de instancia accesible del tipo del *primary_expression* de la *with_expression*.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-138">Each *identifier* on the left of a *with_initializer* must bind to an accessible instance field or property of the type of the *primary_expression* of the *with_expression*.</span></span> <span data-ttu-id="3f0f5-139">Es posible que no haya ningún nombre duplicado entre estos identificadores de un *with_expression*determinado.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-139">There may be no duplicated name among these identifiers of a given *with_expression*.</span></span>

<span data-ttu-id="3f0f5-140">*With_expression* del formulario</span><span class="sxs-lookup"><span data-stu-id="3f0f5-140">A *with_expression* of the form</span></span>

> <span data-ttu-id="3f0f5-141">*e1* `with` *identificador* de `{` = *e2*,... `}`</span><span class="sxs-lookup"><span data-stu-id="3f0f5-141">*e1* `with` `{` *identifier* = *e2*, ... `}`</span></span>

<span data-ttu-id="3f0f5-142">se trata como una invocación del formulario</span><span class="sxs-lookup"><span data-stu-id="3f0f5-142">is treated as an invocation of the form</span></span>

> <span data-ttu-id="3f0f5-143">*e1*`.With(`*identificador2*`:` *e2*,...`)`</span><span class="sxs-lookup"><span data-stu-id="3f0f5-143">*e1*`.With(`*identifier2*`:` *e2*, ...`)`</span></span>

<span data-ttu-id="3f0f5-144">En el caso de cada método denominado `With` que sea un miembro de instancia accesible de *E1*, se selecciona *identificador2* como el nombre del primer parámetro de ese método que tiene un parámetro de receptor llamador que es el mismo miembro que el campo de instancia o la propiedad enlazada a *Identifier*.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-144">Where, for each method named `With` that is an accessible instance member of *e1*, we select *identifier2* as the name of the first parameter in that method that has a caller-receiver parameter that is the same member as the instance field or property bound to *identifier*.</span></span> <span data-ttu-id="3f0f5-145">Si no se puede identificar este parámetro, no se debe tener en cuenta el método.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-145">If no such parameter can be identified that method is eliminated from consideration.</span></span> <span data-ttu-id="3f0f5-146">El método que se va a invocar se selecciona entre los candidatos restantes por resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-146">The method to be invoked is selected from among the remaining candidates by overload resolution.</span></span>

> <span data-ttu-id="3f0f5-147">**Notas de diseño**: determinados parámetros del receptor de la llamada, muchas de las ventajas de la *expresión with* están disponibles sin esta forma de sintaxis especial.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-147">**Design Notes**: Given caller-receiver parameters, many of the benefits of the *with-expression* are available without this special syntax form.</span></span> <span data-ttu-id="3f0f5-148">Por lo tanto, estamos considerando si es necesario o no.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-148">We are therefore considering whether or not it is needed.</span></span> <span data-ttu-id="3f0f5-149">Su principal ventaja es permitir que uno programe en función de los nombres de campos y propiedades, en lugar de los nombres de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-149">Its main benefit is allowing one to program in terms of the names of fields and properties, rather than in terms of the names of parameters.</span></span> <span data-ttu-id="3f0f5-150">De esta manera, se mejora la legibilidad y la calidad de las herramientas (por ejemplo, la ir a la definición en el identificador de un *with_expression* se navegaría a la propiedad en lugar de a un parámetro de método).</span><span class="sxs-lookup"><span data-stu-id="3f0f5-150">In this way we improve both readability and the quality of tooling (e.g. go-to-definition on the identifier of a *with_expression* would navigate to the property rather than to a method parameter).</span></span>

- <span data-ttu-id="3f0f5-151">[] **Problema abierto**: esta descripción debe modificarse para admitir métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-151">[ ] **Open issue**: This description should be modified to support extension methods.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="3f0f5-152">coincidencia de patrones</span><span class="sxs-lookup"><span data-stu-id="3f0f5-152">pattern-matching</span></span>

<span data-ttu-id="3f0f5-153">Vea la [especificación de coincidencia de patrones](csharp-8.0/patterns.md#positional-pattern) para obtener una especificación de `Deconstruct` y su relación con la coincidencia de patrones.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-153">See the [Pattern Matching Specification](csharp-8.0/patterns.md#positional-pattern) for a specification of `Deconstruct` and its relationship to pattern-matching.</span></span>

> <span data-ttu-id="3f0f5-154">**Notas de diseño**: en virtud del `Deconstruct` generado por el compilador tal y como se especifica aquí, y la especificación para la coincidencia de patrones, una declaración de registro</span><span class="sxs-lookup"><span data-stu-id="3f0f5-154">**Design Notes**: By virtue of the compiler-generated `Deconstruct` as specified herein, and the specification for pattern-matching, a record declaration</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="3f0f5-155">admitirá la coincidencia de patrones posicionales como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-155">will support positional pattern-matching as follows</span></span>
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a><span data-ttu-id="3f0f5-156">declaraciones de tipos de registro</span><span class="sxs-lookup"><span data-stu-id="3f0f5-156">record type declarations</span></span>

<span data-ttu-id="3f0f5-157">La sintaxis de una declaración de `class` o `struct` se extiende para admitir parámetros de valor; los parámetros se convierten en propiedades del tipo:</span><span class="sxs-lookup"><span data-stu-id="3f0f5-157">The syntax for a `class` or `struct` declaration is extended to support value parameters; the parameters become properties of the type:</span></span>

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> <span data-ttu-id="3f0f5-158">**Notas de diseño**: dado que los tipos de registro suelen ser útiles sin necesidad de que los miembros se declaren explícitamente en un cuerpo de clase, se modifica la sintaxis de la declaración para permitir que un cuerpo sea simplemente un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-158">**Design Notes**: Because record types are often useful without the need for any members explicitly declared in a class-body, we modify the syntax of the declaration to allow a body to be simply a semicolon.</span></span>

<span data-ttu-id="3f0f5-159">Una clase (struct) declarada con los *parámetros de registro* se denomina *clase de registro* (*struct record*), cualquiera de los cuales es un *tipo de registro*.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-159">A class (struct) declared with the *record-parameters* is called a *record class* (*record struct*), either of which is a *record type*.</span></span>

- <span data-ttu-id="3f0f5-160">[] **Problema abierto**: necesitamos incluir *primary_constructor_body* en la gramática para que pueda aparecer dentro de una declaración de tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-160">[ ] **Open issue**: We need to include *primary_constructor_body* in the grammar so that it can appear inside a record type declaration.</span></span>
- <span data-ttu-id="3f0f5-161">[] **Problema abierto**: ¿Cuáles son las reglas de conflicto de nombres para los nombres de parámetro?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-161">[ ] **Open issue**: What are the name conflict rules for the parameter names?</span></span> <span data-ttu-id="3f0f5-162">Presumiblemente uno no puede entrar en conflicto con un parámetro de tipo u otro *parámetro de registro*.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-162">Presumably one is not allowed to conflict with a type parameter or another *record-parameter*.</span></span>
- <span data-ttu-id="3f0f5-163">[] **Problema abierto**: es necesario especificar el ámbito de los parámetros de registro.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-163">[ ] **Open issue**: We need to specify the scope of the record-parameters.</span></span> <span data-ttu-id="3f0f5-164">¿Dónde se pueden usar?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-164">Where can they be used?</span></span> <span data-ttu-id="3f0f5-165">Presumiblemente dentro de los inicializadores de campo de instancia y *primary_constructor_body* al menos.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-165">Presumably within instance field initializers and *primary_constructor_body* at least.</span></span>
- <span data-ttu-id="3f0f5-166">[] **Problema abierto**: ¿una declaración de tipo de registro es parcial?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-166">[ ] **Open issue**: Can a record type declaration be partial?</span></span> <span data-ttu-id="3f0f5-167">Si es así, ¿deben repetirse los parámetros en cada parte?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-167">If so, must the parameters be repeated on each part?</span></span>

#### <a name="members-of-a-record-type"></a><span data-ttu-id="3f0f5-168">Miembros de un tipo de registro</span><span class="sxs-lookup"><span data-stu-id="3f0f5-168">Members of a record type</span></span>

<span data-ttu-id="3f0f5-169">Además de los miembros declarados en el *cuerpo de clase*, un tipo de registro tiene los siguientes miembros adicionales:</span><span class="sxs-lookup"><span data-stu-id="3f0f5-169">In addition to the members declared in the *class-body*, a record type has the following additional members:</span></span>

##### <a name="primary-constructor"></a><span data-ttu-id="3f0f5-170">Constructor principal</span><span class="sxs-lookup"><span data-stu-id="3f0f5-170">Primary Constructor</span></span>

<span data-ttu-id="3f0f5-171">Un tipo de registro tiene un constructor de `public` cuya firma corresponde a los parámetros de valor de la declaración de tipos.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-171">A record type has a `public` constructor whose signature corresponds to the value parameters of the type declaration.</span></span> <span data-ttu-id="3f0f5-172">Esto se denomina *constructor principal* para el tipo y hace que se suprima el *constructor predeterminado* declarado implícitamente.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-172">This is called the *primary constructor* for the type, and causes the implicitly declared *default constructor* to be suppressed.</span></span>

<span data-ttu-id="3f0f5-173">En tiempo de ejecución del constructor principal</span><span class="sxs-lookup"><span data-stu-id="3f0f5-173">At runtime the primary constructor</span></span>

* <span data-ttu-id="3f0f5-174">Inicializa los campos de respaldo generados por el compilador para las propiedades correspondientes a los parámetros de valor (si estas propiedades son proporcionadas por el compilador; [vea 1.1.2](#1.1.2)); a</span><span class="sxs-lookup"><span data-stu-id="3f0f5-174">initializes compiler-generated backing fields for the properties corresponding to the value parameters (if these properties are compiler-provided; [see 1.1.2](#1.1.2)); then</span></span>
* <span data-ttu-id="3f0f5-175">ejecuta los inicializadores de campo de instancia que aparecen en el *cuerpo de la clase*; y después</span><span class="sxs-lookup"><span data-stu-id="3f0f5-175">executes the instance field initializers appearing in the *class-body*; and then</span></span>
* <span data-ttu-id="3f0f5-176">invoca un constructor de clase base:</span><span class="sxs-lookup"><span data-stu-id="3f0f5-176">invokes a base class constructor:</span></span>
    * <span data-ttu-id="3f0f5-177">Si hay argumentos en el *record_base_arguments*, se invoca un constructor base seleccionado por la resolución de sobrecarga con estos argumentos;</span><span class="sxs-lookup"><span data-stu-id="3f0f5-177">If there are arguments in the *record_base_arguments*, a base constructor selected by overload resolution with these arguments is invoked;</span></span>
    * <span data-ttu-id="3f0f5-178">De lo contrario, se invoca un constructor base sin argumentos.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-178">Otherwise a base constructor is invoked with no arguments.</span></span>
* <span data-ttu-id="3f0f5-179">ejecuta el cuerpo de cada *primary_constructor_body*, si existe, en el orden de origen.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-179">executes the body of each *primary_constructor_body*, if any, in source order.</span></span>

- <span data-ttu-id="3f0f5-180">[] **Problema abierto**: es necesario especificar ese orden, especialmente en las unidades de compilación de las parciales.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-180">[ ] **Open issue**: We need to specify that order, particularly across compilation units for partials.</span></span>
- <span data-ttu-id="3f0f5-181">[] **Problema abierto**: es necesario especificar que cada constructor declarado explícitamente debe encadenarse al constructor principal.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-181">[ ] **Open Issue**: We need to specify that every explicitly declared constructor must chain to the primary constructor.</span></span>
- <span data-ttu-id="3f0f5-182">[] **Problema abierto**: ¿se debe permitir cambiar el modificador de acceso en el constructor principal?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-182">[ ] **Open issue**: Should it be allowed to change the access modifier on the primary constructor?</span></span>
- <span data-ttu-id="3f0f5-183">[] **Problema abierto**: en una estructura de registro, es un error que no haya ningún parámetro de registro?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-183">[ ] **Open issue**: In a record struct, it is an error for there to be no record parameters?</span></span>

##### <a name="primary-constructor-body"></a><span data-ttu-id="3f0f5-184">Cuerpo del constructor principal</span><span class="sxs-lookup"><span data-stu-id="3f0f5-184">Primary constructor body</span></span>

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

<span data-ttu-id="3f0f5-185">Un *primary_constructor_body* solo se puede usar dentro de una declaración de tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-185">A *primary_constructor_body* may only be used within a record type declaration.</span></span> <span data-ttu-id="3f0f5-186">El *identificador* de un *primary_constructor_body* debe nombrar el tipo de registro en el que se declara.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-186">The *identifier* of a *primary_constructor_body* shall name the record type in which it is declared.</span></span>

<span data-ttu-id="3f0f5-187">El *primary_constructor_body* no declara un miembro por sí mismo, pero es una forma de que el programador proporcione atributos para y especifica el acceso de un constructor principal de un tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-187">The *primary_constructor_body* does not declare a member on its own, but is a way for the programmer to provide attributes for, and specify the access of, a record type's primary constructor.</span></span> <span data-ttu-id="3f0f5-188">También permite al programador proporcionar código adicional que se ejecutará cuando se construya una instancia del tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-188">It also enables the programmer to provide additional code that will be executed when an instance of the record type is constructed.</span></span>

- <span data-ttu-id="3f0f5-189">[] **Problema abierto**: debemos tener en cuenta que un constructor predeterminado de struct lo omite.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-189">[ ] **Open issue**: We should note that a struct default constructor bypasses this.</span></span>
- <span data-ttu-id="3f0f5-190">[] **Problema abierto**: deberíamos especificar el orden de ejecución de la inicialización.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-190">[ ] **Open issue**: We should specify the execution order of initialization.</span></span>
- <span data-ttu-id="3f0f5-191">[] **Problema abierto**: ¿se debe permitir algo como un *primary_constructor_body* (supuestamente sin atributos y modificadores) en una declaración de tipo no de registro y tratarlo como se haría con el código de un inicializador de campo de instancia?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-191">[ ] **Open issue**: Should we allow something like a *primary_constructor_body* (presumably without attributes and modifiers) in a non-record type declaration, and treat it like we would the code of an instance field initializer?</span></span>

##### <a name="properties"></a><span data-ttu-id="3f0f5-192">Propiedades</span><span class="sxs-lookup"><span data-stu-id="3f0f5-192">Properties</span></span>

<span data-ttu-id="3f0f5-193">Para cada parámetro de registro de una declaración de tipo de registro, hay un miembro de propiedad `public` correspondiente cuyo nombre y tipo se toman de la declaración del parámetro value.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-193">For each record parameter of a record type declaration there is a corresponding `public` property member whose name and type are taken from the value parameter declaration.</span></span> <span data-ttu-id="3f0f5-194">Su nombre es el *identificador* de la *record_property_name*, si está presente, o el *identificador* de la *record_parameter* en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-194">Its name is the *identifier* of the *record_property_name*, if present, or the *identifier* of the *record_parameter* otherwise.</span></span> <span data-ttu-id="3f0f5-195">Si no hay ninguna propiedad pública concreta (es decir, no abstracta) con un descriptor de acceso `get` y con este nombre y tipo declarados o heredados explícitamente, el compilador los genera de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="3f0f5-195">If no concrete (i.e. non-abstract) public property with a `get` accessor and with this name and type is explicitly declared or inherited, it is produced by the compiler as follows:</span></span>

* <span data-ttu-id="3f0f5-196">Para un *struct de registro* o una *clase de registro*de `sealed`:</span><span class="sxs-lookup"><span data-stu-id="3f0f5-196">For a *record struct* or a `sealed` *record class*:</span></span>
 * <span data-ttu-id="3f0f5-197">Un `private` campo `readonly` se genera como un campo de respaldo para una propiedad `readonly`.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-197">A `private` `readonly` field is produced as a backing field for a `readonly` property.</span></span> <span data-ttu-id="3f0f5-198">Su valor se inicializa durante la construcción con el valor del parámetro del constructor principal correspondiente.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-198">Its value is initialized during construction with the value of the corresponding primary constructor parameter.</span></span>
 * <span data-ttu-id="3f0f5-199">El descriptor de acceso `get` de la propiedad se implementa para devolver el valor del campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-199">The property's `get` accessor is implemented to return the value of the backing field.</span></span>
 * <span data-ttu-id="3f0f5-200">Los descriptores de acceso `get` de la propiedad virtual heredada "matching" se reemplazan.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-200">Each "matching" inherited virtual property's `get` accessor is overridden.</span></span>

> <span data-ttu-id="3f0f5-201">**Notas de diseño**: en otras palabras, si extiende una clase base o implementa una interfaz que declara una propiedad abstracta pública con el mismo nombre y tipo que un parámetro de registro, esa propiedad se invalida o se implementa.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-201">**Design notes**: In other words, if you extend a base class or implement an interface that declares a public abstract property with the same name and type as a record parameter, that property is overridden or implemented.</span></span>

- <span data-ttu-id="3f0f5-202">[] **Problema abierto**: ¿es posible cambiar el modificador de acceso en una propiedad cuando se declara explícitamente?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-202">[ ] **Open issue**: Should it be possible to change the access modifier on a property when it is explicitly declared?</span></span>
- <span data-ttu-id="3f0f5-203">[] **Problema abierto**: ¿debería ser posible sustituir un campo por una propiedad?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-203">[ ] **Open issue**: Should it be possible to substitute a field for a property?</span></span>

##### <a name="object-methods"></a><span data-ttu-id="3f0f5-204">Métodos de objeto</span><span class="sxs-lookup"><span data-stu-id="3f0f5-204">Object Methods</span></span>

<span data-ttu-id="3f0f5-205">Para un *struct de registro* o una *clase de registro*de `sealed`, el compilador produce implementaciones de los métodos `object.GetHashCode()` y `object.Equals(object)` a menos que el usuario lo proporcione.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-205">For a *record struct* or a `sealed` *record class*, implementations of the methods `object.GetHashCode()` and `object.Equals(object)` are produced by the compiler unless provided by the user.</span></span>

- <span data-ttu-id="3f0f5-206">[] **Problema abierto**: debemos especificar con precisión su implementación.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-206">[ ] **Open issue**: We should precisely specify their implementation.</span></span>
- <span data-ttu-id="3f0f5-207">[] **Problema abierto**: también debemos agregar la interfaz `IEquatable<T>` para el tipo de registro y especificar que se proporcionen las implementaciones.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-207">[ ] **Open issue**: We should also add the interface `IEquatable<T>` for the record type and specify that implementations are provided.</span></span>
- <span data-ttu-id="3f0f5-208">[] **Problema abierto**: también se debe especificar que se implementen todos los `IEquatable<T>.Equals`.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-208">[ ] **Open issue**: We should also specify that we implement every `IEquatable<T>.Equals`.</span></span>
- <span data-ttu-id="3f0f5-209">[] **Problema abierto**: debemos especificar con exactitud cómo se resuelve el problema de Equals en la herencia de registros: en concreto, cómo se generan métodos de igualdad, de modo que son simétricos, transitivos, reflexivos, etc.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-209">[ ] **Open issue**: We should specify precisely how we solve the problem of Equals in the face of record inheritance: specifically how we generate equality methods such that they are symmetric, transitive, reflexive, etc.</span></span>
- <span data-ttu-id="3f0f5-210">[] **Problema abierto**: se ha propuesto que implementemos `operator ==` y `operator !=` para tipos de registro.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-210">[ ] **Open issue**: It has been proposed that we implement `operator ==` and `operator !=` for record types.</span></span>
- <span data-ttu-id="3f0f5-211">[] **Problema abierto**: ¿se debe generar automáticamente una implementación de `object.ToString`?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-211">[ ] **Open issue**: Should we auto-generate an implementation of `object.ToString`?</span></span>

##### `Deconstruct`

<span data-ttu-id="3f0f5-212">Un tipo de registro tiene un método `public` generado por el compilador `void Deconstruct` a menos que el usuario proporcione uno con cualquier firma.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-212">A record type has a compiler-generated `public` method `void Deconstruct` unless one with any signature is provided by the user.</span></span> <span data-ttu-id="3f0f5-213">Cada parámetro es un parámetro `out` con el mismo nombre y tipo que el parámetro correspondiente del tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-213">Each parameter is an `out` parameter of the same name and type as the corresponding parameter of the record type.</span></span> <span data-ttu-id="3f0f5-214">La implementación proporcionada por el compilador de este método asignará cada `out` parámetro con el valor de la propiedad correspondiente.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-214">The compiler-provided implementation of this method shall assign each `out` parameter with the value of the corresponding property.</span></span>

<span data-ttu-id="3f0f5-215">Vea [la especificación de coincidencia de patrones](csharp-8.0/patterns.md#positional-pattern) para la semántica de `Deconstruct`.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-215">See [the pattern-matching specification](csharp-8.0/patterns.md#positional-pattern) for the semantics of `Deconstruct`.</span></span>

##### <a name="with-method"></a><span data-ttu-id="3f0f5-216">Método`With`</span><span class="sxs-lookup"><span data-stu-id="3f0f5-216">`With` method</span></span>

<span data-ttu-id="3f0f5-217">A menos que haya un miembro declarado por el usuario denominado `With` declarado, un tipo de registro tiene un método proporcionado por el compilador denominado `With` cuyo tipo de valor devuelto es el propio tipo de registro y que contiene un parámetro de valor que corresponde a cada *parámetro de registro* en el mismo orden en que aparecen en la declaración de tipo de registro.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-217">Unless there is a user-declared member named `With` declared, a record type has a compiler-provided method named `With` whose return type is the record type itself, and containing one value parameter corresponding to each *record-parameter* in the same order that these parameters appear in the record type declaration.</span></span> <span data-ttu-id="3f0f5-218">Cada parámetro debe tener un *argumento predeterminado del receptor del llamador* de la propiedad correspondiente.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-218">Each parameter shall have a *caller-receiver default-argument* of the corresponding property.</span></span>

<span data-ttu-id="3f0f5-219">En una clase de registro de `abstract`, el método de `With` proporcionado por el compilador es abstracto.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-219">In an `abstract` record class, the compiler-provided `With` method is abstract.</span></span> <span data-ttu-id="3f0f5-220">En una estructura de registro o una clase de registro sellada, el método de `With` proporcionado por el compilador es `sealed`.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-220">In a record struct, or a sealed record class, the compiler-provided `With` method is `sealed`.</span></span> <span data-ttu-id="3f0f5-221">De lo contrario, el método de `With` proporcionado por el compilador es ' virtual y su implementación devolverá una nueva instancia de generada al invocar el constructor principal con los parámetros como argumentos para crear una nueva instancia a partir de los parámetros y devolver esa nueva instancia.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-221">Otherwise the compiler-provided `With` method is \`virtual and its implementation shall return a new instance produced by invoking the primary constructor with the parameters as arguments to create a new instance from the parameters, and return that new instance.</span></span>

- <span data-ttu-id="3f0f5-222">[] **Problema abierto**: también debemos especificar en qué condiciones invalidamos o implementamos métodos `With` virtuales heredados o métodos de `With` de interfaces implementadas.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-222">[ ] **Open issue**: We should also specify under what conditions we override or implement inherited virtual `With` methods or `With` methods from implemented interfaces.</span></span>
- <span data-ttu-id="3f0f5-223">[] **Problema abierto**: deberíamos indicar lo que sucede cuando se hereda un método de `With` no virtual.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-223">[ ] **Open issue**: We should say what happens when we inherit a non-virtual `With` method.</span></span>

> <span data-ttu-id="3f0f5-224">**Notas de diseño**: dado que los tipos de registro son inmutables de forma predeterminada, el método `With` proporciona una forma de crear una nueva instancia de que es igual que una instancia existente, pero con las propiedades seleccionadas en función de los nuevos valores.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-224">**Design notes**: Because record types are by default immutable, the `With` method provides a way of creating a new instance that is the same as an existing instance but with selected properties given new values.</span></span> <span data-ttu-id="3f0f5-225">Por ejemplo, dado</span><span class="sxs-lookup"><span data-stu-id="3f0f5-225">For example, given</span></span>
> ```cs
> public class Point(int X, int Y);
> ```
> <span data-ttu-id="3f0f5-226">existe un miembro proporcionado por el compilador</span><span class="sxs-lookup"><span data-stu-id="3f0f5-226">there is a compiler-provided member</span></span>
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> <span data-ttu-id="3f0f5-227">Que habilita una variable del tipo de registro</span><span class="sxs-lookup"><span data-stu-id="3f0f5-227">Which enables an variable of the record type</span></span>
> ```cs
> var p = new Point(3, 4);
> ```
> <span data-ttu-id="3f0f5-228">para reemplazar por una instancia que tiene una o varias propiedades diferentes</span><span class="sxs-lookup"><span data-stu-id="3f0f5-228">to be replaced with an instance that has one or more properties different</span></span>
> ```cs
>     p = p.With(X: 5);
> ```
> <span data-ttu-id="3f0f5-229">Esto también se puede expresar mediante el *with_expression*:</span><span class="sxs-lookup"><span data-stu-id="3f0f5-229">This can also be expressed using the *with_expression*:</span></span>
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a><span data-ttu-id="3f0f5-230">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="3f0f5-230">Examples</span></span>

#### <a name="compatibility-of-record-types"></a><span data-ttu-id="3f0f5-231">Compatibilidad de tipos de registro</span><span class="sxs-lookup"><span data-stu-id="3f0f5-231">Compatibility of record types</span></span>

<span data-ttu-id="3f0f5-232">Dado que el programador puede Agregar miembros a una declaración de tipo de registro, a menudo es posible cambiar el conjunto de elementos de registro sin que ello afecte a los clientes existentes.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-232">Because the programmer can add members to a record type declaration, it is often possible to change the set of record elements without affecting existing clients.</span></span> <span data-ttu-id="3f0f5-233">Por ejemplo, dada una versión inicial de un tipo de registro</span><span class="sxs-lookup"><span data-stu-id="3f0f5-233">For example, given an initial version of a record type</span></span>

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

<span data-ttu-id="3f0f5-234">Un nuevo elemento del tipo de registro puede agregarse de compatibilidad en la siguiente revisión del tipo sin afectar a la compatibilidad binaria o de origen:</span><span class="sxs-lookup"><span data-stu-id="3f0f5-234">A new element of the record type can be compatibly added in the next revision of the type without affecting binary or source compatibility:</span></span>

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a><span data-ttu-id="3f0f5-235">ejemplo de struct de registro</span><span class="sxs-lookup"><span data-stu-id="3f0f5-235">record struct example</span></span>

<span data-ttu-id="3f0f5-236">Este struct de registro</span><span class="sxs-lookup"><span data-stu-id="3f0f5-236">This record struct</span></span>

```cs
public struct Pair(object First, object Second);
```

<span data-ttu-id="3f0f5-237">se convierte en este código</span><span class="sxs-lookup"><span data-stu-id="3f0f5-237">is translated to this code</span></span>

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- <span data-ttu-id="3f0f5-238">[] **Problema abierto**: ¿la implementación de es igual a (otro par) ser un miembro público de pair?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-238">[ ] **Open issue**: should the implementation of Equals(Pair other) be a public member of Pair?</span></span>
- <span data-ttu-id="3f0f5-239">[] **Problema abierto**: esta implementación de `Equals` no es simétrica en el aspecto de la herencia.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-239">[ ] **Open issue**: This implementation of `Equals` is not symmetric in the face of inheritance.</span></span>
- <span data-ttu-id="3f0f5-240">[] **Problema abierto**: ¿debe un registro declarar `operator ==` y `operator !=`?</span><span class="sxs-lookup"><span data-stu-id="3f0f5-240">[ ] **Open issue**: Should a record declare `operator ==` and `operator !=`?</span></span>

> <span data-ttu-id="3f0f5-241">**Notas de diseño**: dado que un tipo de registro puede heredar de otro y esta implementación de `Equals` no sería simétrica en ese caso, no es correcto.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-241">**Design notes**: Because one record type can inherit from another, and this implementation of `Equals` would not be symmetric in that case, it is not correct.</span></span> <span data-ttu-id="3f0f5-242">Se propone implementar la igualdad de esta manera:</span><span class="sxs-lookup"><span data-stu-id="3f0f5-242">We propose to implement equality this way:</span></span>
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> <span data-ttu-id="3f0f5-243">Los registros derivados `override EqualityContract`.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-243">Derived records would `override EqualityContract`.</span></span> <span data-ttu-id="3f0f5-244">La alternativa menos atractiva es restringir la herencia.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-244">The less attractive alternative is to restrict inheritance.</span></span>

#### <a name="sealed-record-example"></a><span data-ttu-id="3f0f5-245">ejemplo de registro sellado</span><span class="sxs-lookup"><span data-stu-id="3f0f5-245">sealed record example</span></span>

<span data-ttu-id="3f0f5-246">Esta clase de registro sellado</span><span class="sxs-lookup"><span data-stu-id="3f0f5-246">This sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa);
```

<span data-ttu-id="3f0f5-247">se convierte en este código</span><span class="sxs-lookup"><span data-stu-id="3f0f5-247">is translated into this code</span></span>

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a><span data-ttu-id="3f0f5-248">ejemplo de clase de registro abstracta</span><span class="sxs-lookup"><span data-stu-id="3f0f5-248">abstract record class example</span></span>

<span data-ttu-id="3f0f5-249">Esta clase de registro abstracto</span><span class="sxs-lookup"><span data-stu-id="3f0f5-249">This abstract record class</span></span>

```cs
public abstract class Person(string Name);
```

<span data-ttu-id="3f0f5-250">se convierte en este código</span><span class="sxs-lookup"><span data-stu-id="3f0f5-250">is translated into this code</span></span>

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a><span data-ttu-id="3f0f5-251">combinación de registros abstractos y sellados</span><span class="sxs-lookup"><span data-stu-id="3f0f5-251">combining abstract and sealed records</span></span>

<span data-ttu-id="3f0f5-252">Dada la clase de registro Abstract `Person` anterior, esta clase de registro sellado</span><span class="sxs-lookup"><span data-stu-id="3f0f5-252">Given the abstract record class `Person` above, this sealed record class</span></span>

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

<span data-ttu-id="3f0f5-253">se convierte en este código</span><span class="sxs-lookup"><span data-stu-id="3f0f5-253">is translated into this code</span></span>

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a><span data-ttu-id="3f0f5-254">Desventajas</span><span class="sxs-lookup"><span data-stu-id="3f0f5-254">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="3f0f5-255">Al igual que con cualquier característica de lenguaje, debemos cuestionar si se reembolsa la complejidad adicional en el idioma en la claridad adicional que se C# ofrece al cuerpo de los programas que se beneficiarían de la característica.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-255">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="3f0f5-256">Alternativas</span><span class="sxs-lookup"><span data-stu-id="3f0f5-256">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="3f0f5-257">Consideramos agregar *constructores principales* en C# 6.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-257">We considered adding *primary constructors* in C# 6.</span></span> <span data-ttu-id="3f0f5-258">Aunque ocupan la misma superficie sintáctica que esta propuesta, descubrimos que no eran las ventajas que ofrecen los registros.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-258">Although they occupy the same syntactic surface as this proposal, we found that they fell short of the advantages offered by records.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="3f0f5-259">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="3f0f5-259">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="3f0f5-260">Las preguntas abiertas aparecen en el cuerpo de la propuesta.</span><span class="sxs-lookup"><span data-stu-id="3f0f5-260">Open questions appear in the body of the proposal.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="3f0f5-261">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="3f0f5-261">Design meetings</span></span>

<span data-ttu-id="3f0f5-262">TBD</span><span class="sxs-lookup"><span data-stu-id="3f0f5-262">TBD</span></span>
