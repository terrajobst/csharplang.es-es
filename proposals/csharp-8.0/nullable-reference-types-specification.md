---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2020
ms.locfileid: "79484109"
---
# <a name="nullable-reference-types-specification"></a><span data-ttu-id="1e1a5-101">Especificación de tipos de referencia que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="1e1a5-101">Nullable Reference Types Specification</span></span>

<span data-ttu-id="1e1a5-102">Se trata de un trabajo en curso; faltan varias partes o están incompletas.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-102">\*\*\* This is a work in progress - several parts are missing or incomplete.</span></span> ***

## <a name="syntax"></a><span data-ttu-id="1e1a5-103">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="1e1a5-103">Syntax</span></span>

### <a name="nullable-reference-types"></a><span data-ttu-id="1e1a5-104">Tipos de referencia que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="1e1a5-104">Nullable reference types</span></span>

<span data-ttu-id="1e1a5-105">Los tipos de referencia que aceptan valores NULL tienen la misma sintaxis `T?` que la forma abreviada de tipos de valor que aceptan valores NULL, pero no tienen una forma larga correspondiente.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-105">Nullable reference types have the same syntax `T?` as the short form of nullable value types, but do not have a corresponding long form.</span></span>

<span data-ttu-id="1e1a5-106">Para los fines de la especificación, se cambia el nombre de la producción de `nullable_type` actual a `nullable_value_type`y se agrega una `nullable_reference_type` producción:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-106">For the purposes of the specification, the current `nullable_type` production is renamed to `nullable_value_type`, and a `nullable_reference_type` production is added:</span></span>

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

<span data-ttu-id="1e1a5-107">El `non_nullable_reference_type` en un `nullable_reference_type` debe ser un tipo de referencia que no acepte valores NULL (clase, interfaz, delegado o matriz) o un parámetro de tipo restringido para ser un tipo de referencia que no acepta valores NULL (a través de la restricción `class` o una clase distinta de `object`).</span><span class="sxs-lookup"><span data-stu-id="1e1a5-107">The `non_nullable_reference_type` in a `nullable_reference_type` must be a non-nullable reference type (class, interface, delegate or array), or a type parameter that is constrained to be a non-nullable reference type (through the `class` constraint, or a class other than `object`).</span></span>

<span data-ttu-id="1e1a5-108">Los tipos de referencia que aceptan valores NULL no pueden aparecer en las siguientes posiciones:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-108">Nullable reference types cannot occur in the following positions:</span></span>

- <span data-ttu-id="1e1a5-109">como una clase base o una interfaz</span><span class="sxs-lookup"><span data-stu-id="1e1a5-109">as a base class or interface</span></span>
- <span data-ttu-id="1e1a5-110">como receptor de una `member_access`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-110">as the receiver of a `member_access`</span></span>
- <span data-ttu-id="1e1a5-111">como `type` en un `object_creation_expression`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-111">as the `type` in an `object_creation_expression`</span></span>
- <span data-ttu-id="1e1a5-112">como `delegate_type` en un `delegate_creation_expression`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-112">as the `delegate_type` in a `delegate_creation_expression`</span></span>
- <span data-ttu-id="1e1a5-113">como `type` en un `is_expression`, un `catch_clause` o un `type_pattern`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-113">as the `type` in an `is_expression`, a `catch_clause` or a `type_pattern`</span></span>
- <span data-ttu-id="1e1a5-114">como `interface` en un nombre de miembro de interfaz completo</span><span class="sxs-lookup"><span data-stu-id="1e1a5-114">as the `interface` in a fully qualified interface member name</span></span>

<span data-ttu-id="1e1a5-115">Se proporciona una advertencia en un `nullable_reference_type` donde se deshabilita el contexto de anotación que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-115">A warning is given on a `nullable_reference_type` where the nullable annotation context is disabled.</span></span>

### <a name="nullable-class-constraint"></a><span data-ttu-id="1e1a5-116">Restricción de clase que acepta valores NULL</span><span class="sxs-lookup"><span data-stu-id="1e1a5-116">Nullable class constraint</span></span>

<span data-ttu-id="1e1a5-117">La restricción `class` tiene un homólogo que acepta valores NULL `class?`:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-117">The `class` constraint has a nullable counterpart `class?`:</span></span>

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a><span data-ttu-id="1e1a5-118">El operador null-permisivo</span><span class="sxs-lookup"><span data-stu-id="1e1a5-118">The null-forgiving operator</span></span>

<span data-ttu-id="1e1a5-119">El operador de `!` posterior a la corrección se denomina operador permisivo nulo.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-119">The post-fix `!` operator is called the null-forgiving operator.</span></span>

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

<span data-ttu-id="1e1a5-120">El `primary_expression` debe ser de un tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-120">The `primary_expression` must be of a reference type.</span></span>  

<span data-ttu-id="1e1a5-121">El operador de `!` postfijo no tiene ningún efecto en tiempo de ejecución, sino que se evalúa como el resultado de la expresión subyacente.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-121">The postfix `!` operator has no runtime effect - it evaluates to the result of the underlying expression.</span></span> <span data-ttu-id="1e1a5-122">Su único rol es cambiar el estado null de la expresión y limitar las advertencias que se dan en su uso.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-122">Its only role is to change the null state of the expression, and to limit warnings given on its use.</span></span>

### <a name="nullable-implicitly-typed-local-variables"></a><span data-ttu-id="1e1a5-123">variables locales con tipo implícito que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="1e1a5-123">nullable implicitly typed local variables</span></span>

<span data-ttu-id="1e1a5-124">`var` deduce un tipo anotado para los tipos de referencia.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-124">`var` infers an annotated type for reference types.</span></span>
<span data-ttu-id="1e1a5-125">Por ejemplo, en `var s = "";` el `var` se deduce como `string?`.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-125">For instance, in `var s = "";` the `var` is inferred as `string?`.</span></span>

### <a name="nullable-compiler-directives"></a><span data-ttu-id="1e1a5-126">Directivas de compilador que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="1e1a5-126">Nullable compiler directives</span></span>

<span data-ttu-id="1e1a5-127">las directivas de `#nullable` controlan la anotación y los contextos de advertencia que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-127">`#nullable` directives control the nullable annotation and warning contexts.</span></span>

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

<span data-ttu-id="1e1a5-128">las directivas de `#pragma warning` se expanden para permitir cambiar el contexto de advertencia que acepta valores NULL y permitir que se habiliten advertencias individuales en incluso cuando están deshabilitadas de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-128">`#pragma warning` directives are expanded to allow changing the nullable warning context, and to allow individual warnings to be enabled on even when they're disabled by default:</span></span>

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

<span data-ttu-id="1e1a5-129">Tenga en cuenta que el nuevo formulario de `pragma_warning_body` usa `nullable_action`, no `warning_action`.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-129">Note that the new form of `pragma_warning_body` uses `nullable_action`, not `warning_action`.</span></span>

## <a name="nullable-contexts"></a><span data-ttu-id="1e1a5-130">Contextos que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="1e1a5-130">Nullable contexts</span></span>

<span data-ttu-id="1e1a5-131">Cada línea de código fuente tiene un *contexto de anotación que acepta valores NULL* y un *contexto de advertencia que acepta valores NULL*.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-131">Every line of source code has a *nullable annotation context* and a *nullable warning context*.</span></span> <span data-ttu-id="1e1a5-132">Estos controlan si las anotaciones que aceptan valores NULL tienen efecto y si se proporcionan advertencias de nulabilidad.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-132">These control whether nullable annotations have effect, and whether nullability warnings are given.</span></span> <span data-ttu-id="1e1a5-133">El contexto de anotación de una línea determinada está *deshabilitado* o *habilitado*.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-133">The annotation context of a given line is either *disabled* or *enabled*.</span></span> <span data-ttu-id="1e1a5-134">El contexto de advertencia de una línea determinada está *deshabilitado* o *habilitado*.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-134">The warning context of a given line is either *disabled* or *enabled*.</span></span>

<span data-ttu-id="1e1a5-135">Ambos contextos se pueden especificar en el nivel de proyecto (fuera C# del código fuente) o en cualquier lugar dentro de un archivo de código fuente a través de `#nullable` directivas de preprocesador.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-135">Both contexts can be specified at the project level (outside of C# source code), or anywhere within a source file via `#nullable` pre-processor directives.</span></span> <span data-ttu-id="1e1a5-136">Si no se proporciona ninguna configuración de nivel de proyecto, el valor predeterminado es que ambos contextos estén *deshabilitados*.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-136">If no project level settings are provided the default is for both contexts to be *disabled*.</span></span>

<span data-ttu-id="1e1a5-137">La Directiva `#nullable` controla la anotación y los contextos de advertencia dentro del texto de origen y tienen prioridad sobre la configuración de nivel de proyecto.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-137">The `#nullable` directive controls the annotation and warning contexts within the source text, and take precedence over the project-level settings.</span></span>

<span data-ttu-id="1e1a5-138">Una Directiva establece los contextos que controla para las siguientes líneas de código, hasta que otra directiva la invalida o hasta el final del archivo de código fuente.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-138">A directive sets the context(s) it controls for subsequent lines of code, until another directive overrides it, or until the end of the source file.</span></span>

<span data-ttu-id="1e1a5-139">El efecto de las directivas es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-139">The effect of the directives is as follows:</span></span>

- <span data-ttu-id="1e1a5-140">`#nullable disable`: establece la anotación y los contextos de advertencia que aceptan valores NULL en *deshabilitado* .</span><span class="sxs-lookup"><span data-stu-id="1e1a5-140">`#nullable disable`: Sets the nullable annotation and warning contexts to *disabled*</span></span>
- <span data-ttu-id="1e1a5-141">`#nullable enable`: establece la anotación y los contextos de advertencia que aceptan valores NULL en *habilitado*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-141">`#nullable enable`: Sets the nullable annotation and warning contexts to *enabled*</span></span>
- <span data-ttu-id="1e1a5-142">`#nullable restore`: restaura los contextos de advertencia y anotación que aceptan valores NULL a la configuración del proyecto.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-142">`#nullable restore`: Restores the nullable annotation and warning contexts to project settings</span></span>
- <span data-ttu-id="1e1a5-143">`#nullable disable annotations`: establece el contexto de anotación que acepta valores NULL en *deshabilitado*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-143">`#nullable disable annotations`: Sets the nullable annotation context to *disabled*</span></span>
- <span data-ttu-id="1e1a5-144">`#nullable enable annotations`: establece el contexto de anotación que acepta valores NULL en *habilitado*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-144">`#nullable enable annotations`: Sets the nullable annotation context to *enabled*</span></span>
- <span data-ttu-id="1e1a5-145">`#nullable restore annotations`: restaura el contexto de anotación que acepta valores NULL a la configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="1e1a5-145">`#nullable restore annotations`: Restores the nullable annotation context to project settings</span></span>
- <span data-ttu-id="1e1a5-146">`#nullable disable warnings`: establece el contexto de advertencia que acepta valores NULL en *deshabilitado*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-146">`#nullable disable warnings`: Sets the nullable warning context to *disabled*</span></span>
- <span data-ttu-id="1e1a5-147">`#nullable enable warnings`: establece el contexto de advertencia que acepta valores NULL en *habilitado*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-147">`#nullable enable warnings`: Sets the nullable warning context to *enabled*</span></span>
- <span data-ttu-id="1e1a5-148">`#nullable restore warnings`: restaura el contexto de advertencia que acepta valores NULL a la configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="1e1a5-148">`#nullable restore warnings`: Restores the nullable warning context to project settings</span></span>

## <a name="nullability-of-types"></a><span data-ttu-id="1e1a5-149">Nulabilidad de tipos</span><span class="sxs-lookup"><span data-stu-id="1e1a5-149">Nullability of types</span></span>

<span data-ttu-id="1e1a5-150">Un tipo determinado puede tener uno de cuatro nullabilities: *desconocen*, not *Nullable*, *Nullable* y *Unknown*.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-150">A given type can have one of four nullabilities: *Oblivious*, *nonnullable*, *nullable* and *unknown*.</span></span> 

<span data-ttu-id="1e1a5-151">Los tipos que no *aceptan valores NULL* y los *desconocidos* pueden producir advertencias si se les asigna un valor de `null` potencial.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-151">*Nonnullable* and *unknown* types may cause warnings if a potential `null` value is assigned to them.</span></span> <span data-ttu-id="1e1a5-152">Sin embargo, los tipos *desconocen* y que *aceptan valores NULL* son "*asignable null*" y pueden tener `null` valores asignados sin advertencias.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-152">*Oblivious* and *nullable* types, however, are "*null-assignable*" and can have `null` values assigned to them without warnings.</span></span> 

<span data-ttu-id="1e1a5-153">Los tipos *desconocen* y que no *aceptan valores NULL* se pueden desreferenciar o asignar sin advertencias.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-153">*Oblivious* and *nonnullable* types can be dereferenced or assigned without warnings.</span></span> <span data-ttu-id="1e1a5-154">Sin embargo, los valores de los tipos que *aceptan valores NULL* y los *desconocidos* son "*produjeron valores NULL*" y pueden provocar advertencias al desreferenciarse o asignarse sin una comprobación nula adecuada.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-154">Values of *nullable* and *unknown* types, however, are "*null-yielding*" and may cause warnings when dereferenced or assigned without proper null checking.</span></span> 

<span data-ttu-id="1e1a5-155">El *Estado null predeterminado* de un tipo de rendimiento nulo es "quizás null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-155">The *default null state* of a null-yielding type is "maybe null".</span></span> <span data-ttu-id="1e1a5-156">El estado Null predeterminado de un tipo de rendimiento que no es NULL es "not null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-156">The default null state of a non-null-yielding type is "not null".</span></span>

<span data-ttu-id="1e1a5-157">El tipo de tipo y el contexto de anotación que acepta valores NULL que tiene lugar en determina su nulabilidad:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-157">The kind of type and the nullable annotation context it occurs in determine its nullability:</span></span>

- <span data-ttu-id="1e1a5-158">Un tipo de valor que no acepta valores NULL `S` siempre es no *acepta valores NULL*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-158">A nonnullable value type `S` is always *nonnullable*</span></span>
- <span data-ttu-id="1e1a5-159">Un tipo de valor que acepta valores NULL `S?` siempre *admite valores NULL*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-159">A nullable value type `S?` is always *nullable*</span></span>
- <span data-ttu-id="1e1a5-160">Un tipo de referencia sin anotar `C` en un contexto de anotación *deshabilitado* es *desconocen*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-160">An unannotated reference type `C` in a *disabled* annotation context is *oblivious*</span></span>
- <span data-ttu-id="1e1a5-161">Un tipo de referencia sin anotar `C` en un contexto de anotación *habilitado* no *admite valores NULL*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-161">An unannotated reference type `C` in an *enabled* annotation context is *nonnullable*</span></span>
- <span data-ttu-id="1e1a5-162">Un tipo de referencia que acepta valores NULL `C?` *admite valores NULL* (pero se puede producir una advertencia en un contexto de anotación *deshabilitado* )</span><span class="sxs-lookup"><span data-stu-id="1e1a5-162">A nullable reference type `C?` is *nullable* (but a warning may be yielded in a *disabled* annotation context)</span></span>

<span data-ttu-id="1e1a5-163">Los parámetros de tipo también tienen en cuenta las restricciones:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-163">Type parameters additionally take their constraints into account:</span></span>

- <span data-ttu-id="1e1a5-164">Un parámetro de tipo `T` en el que todas las restricciones (si existen) son tipos que producen valores NULL (*aceptan valores NULL* y son *desconocidos*) o se *desconoce* la restricción `class?`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-164">A type parameter `T` where all constraints (if any) are either null-yielding types (*nullable* and *unknown*) or the `class?` constraint is *unknown*</span></span>
- <span data-ttu-id="1e1a5-165">Un parámetro de tipo `T` donde al menos una restricción es *desconocen* o que no *admite valores NULL* , o una de las restricciones de `struct` o `class` es</span><span class="sxs-lookup"><span data-stu-id="1e1a5-165">A type parameter `T` where at least one constraint is either *oblivious* or *nonnullable* or one of the `struct` or `class` constraints is</span></span>
    - <span data-ttu-id="1e1a5-166">*desconocen* en un contexto de anotación *deshabilitado*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-166">*oblivious* in a *disabled* annotation context</span></span>
    - <span data-ttu-id="1e1a5-167">no *acepta valores NULL* en un contexto de anotación *habilitado*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-167">*nonnullable* in an *enabled* annotation context</span></span>
- <span data-ttu-id="1e1a5-168">Un parámetro de tipo que acepta valores NULL `T?` donde al menos una de las restricciones de `T`es *desconocen* o que no *admite valores NULL* , o una de las restricciones `struct` o `class`, es</span><span class="sxs-lookup"><span data-stu-id="1e1a5-168">A nullable type parameter `T?` where at least one of `T`'s constraints is *oblivious* or *nonnullable* or one of the `struct` or `class` constraints, is</span></span>
    - <span data-ttu-id="1e1a5-169">*acepta valores NULL* en un contexto de anotación *deshabilitado* (pero se produce una advertencia)</span><span class="sxs-lookup"><span data-stu-id="1e1a5-169">*nullable* in a *disabled* annotation context (but a warning is yielded)</span></span>
    - <span data-ttu-id="1e1a5-170">*acepta valores NULL* en un contexto de anotación *habilitado*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-170">*nullable* in an *enabled* annotation context</span></span>

<span data-ttu-id="1e1a5-171">Para un parámetro de tipo `T`, `T?` solo se permite si se sabe que `T` es un tipo de valor o que se sabe que es un tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-171">For a type parameter `T`, `T?` is only allowed if `T` is known to be a value type or known to be a reference type.</span></span>

### <a name="oblivious-vs-nonnullable"></a><span data-ttu-id="1e1a5-172">Desconocen frente a no Nullable</span><span class="sxs-lookup"><span data-stu-id="1e1a5-172">Oblivious vs nonnullable</span></span>

<span data-ttu-id="1e1a5-173">Se considera que un `type` se produce en un contexto de anotación determinado cuando el último token del tipo está dentro de ese contexto.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-173">A `type` is deemed to occur in a given annotation context when the last token of the type is within that context.</span></span>

<span data-ttu-id="1e1a5-174">Que un tipo de referencia determinado `C` en el código fuente se interprete como desconocen o que no admita valores NULL, depende del contexto de anotación de ese código fuente.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-174">Whether a given reference type `C` in source code is interpreted as oblivious or nonnullable depends on the annotation context of that source code.</span></span> <span data-ttu-id="1e1a5-175">Pero una vez establecido, se considera parte de ese tipo y "viaja con él", por ejemplo, durante la sustitución de los argumentos de tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-175">But once established, it is considered part of that type, and "travels with it" e.g. during substitution of generic type arguments.</span></span> <span data-ttu-id="1e1a5-176">Es como si hubiera una anotación como `?` en el tipo, pero invisible.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-176">It is as if there is an annotation like `?` on the type, but invisible.</span></span>

## <a name="constraints"></a><span data-ttu-id="1e1a5-177">Restricciones</span><span class="sxs-lookup"><span data-stu-id="1e1a5-177">Constraints</span></span>

<span data-ttu-id="1e1a5-178">Los tipos de referencia que aceptan valores NULL se pueden usar como restricciones genéricas.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-178">Nullable reference types can be used as generic constraints.</span></span> <span data-ttu-id="1e1a5-179">Además `object` es válido ahora como una restricción explícita.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-179">Furthermore `object` is now valid as an explicit constraint.</span></span> <span data-ttu-id="1e1a5-180">La ausencia de una restricción es ahora equivalente a una restricción `object?` (en lugar de `object`), pero (a diferencia de `object` antes) `object?` no está prohibida como restricción explícita.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-180">Absence of a constraint is now equivalent to an `object?` constraint (instead of `object`), but (unlike `object` before) `object?` is not prohibited as an explicit constraint.</span></span>

<span data-ttu-id="1e1a5-181">`class?` es una nueva restricción que denota "posible tipo de referencia que acepta valores NULL", mientras que `class` denota "tipo de referencia que no acepta valores NULL".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-181">`class?` is a new constraint denoting "possibly nullable reference type", whereas `class` denotes "nonnullable reference type".</span></span>

<span data-ttu-id="1e1a5-182">La nulabilidad de un argumento de tipo o de una restricción no afecta a si el tipo satisface la restricción, excepto en el caso de que ya sea el caso hoy (los tipos de valor que aceptan valores NULL no satisfacen la restricción `struct`).</span><span class="sxs-lookup"><span data-stu-id="1e1a5-182">The nullability of a type argument or of a constraint does not impact whether the type satisfies the constraint, except where that is already the case today (nullable value types do not satisfy the `struct` constraint).</span></span> <span data-ttu-id="1e1a5-183">Sin embargo, si el argumento de tipo no satisface los requisitos de nulabilidad de la restricción, se puede proporcionar una advertencia.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-183">However, if the type argument does not satisfy the nullability requirements of the constraint, a warning may be given.</span></span>

## <a name="null-state-and-null-tracking"></a><span data-ttu-id="1e1a5-184">Estado NULL y seguimiento nulo</span><span class="sxs-lookup"><span data-stu-id="1e1a5-184">Null state and null tracking</span></span>

<span data-ttu-id="1e1a5-185">Cada expresión de una ubicación de origen determinada tiene un *Estado null*, que indica si se considera que puede evaluarse como null.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-185">Every expression in a given source location has a *null state*, which indicated whether it is believed to potentially evaluate to null.</span></span> <span data-ttu-id="1e1a5-186">El estado NULL es "not null" o "maybe null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-186">The null state is either "not null" or "maybe null".</span></span> <span data-ttu-id="1e1a5-187">El estado NULL se usa para determinar si se debe proporcionar una advertencia sobre las conversiones y desreferenciaciones no seguras de NULL.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-187">The null state is used to determine whether a warning should be given about null-unsafe conversions and dereferences.</span></span>

### <a name="null-tracking-for-variables"></a><span data-ttu-id="1e1a5-188">Seguimiento de valores NULL para variables</span><span class="sxs-lookup"><span data-stu-id="1e1a5-188">Null tracking for variables</span></span>

<span data-ttu-id="1e1a5-189">En el caso de ciertas expresiones que indican variables o propiedades, se realiza un seguimiento del estado Null entre las repeticiones, en función de las asignaciones a ellos, las pruebas realizadas en ellos y el flujo de control entre ellos.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-189">For certain expressions denoting variables or properties, the null state is tracked between occurrences, based on assignments to them, tests performed on them and the control flow between them.</span></span> <span data-ttu-id="1e1a5-190">Esto es similar a la forma en que se realiza el seguimiento de la asignación definitiva para las variables.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-190">This is similar to how definite assignment is tracked for variables.</span></span> <span data-ttu-id="1e1a5-191">Las expresiones de las que se ha realizado un seguimiento son las siguientes:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-191">The tracked expressions are the ones of the following form:</span></span>

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

<span data-ttu-id="1e1a5-192">Donde los identificadores denotan campos o propiedades.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-192">Where the identifiers denote fields or properties.</span></span>

<span data-ttu-id="1e1a5-193">***Describir las transiciones de estado Null similares a las asignaciones definitivas***</span><span class="sxs-lookup"><span data-stu-id="1e1a5-193">***Describe null state transitions similar to definite assignment***</span></span>

### <a name="null-state-for-expressions"></a><span data-ttu-id="1e1a5-194">Estado null para expresiones</span><span class="sxs-lookup"><span data-stu-id="1e1a5-194">Null state for expressions</span></span>

<span data-ttu-id="1e1a5-195">El estado null de una expresión se deriva de su forma y tipo, y del estado null de las variables implicadas en él.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-195">The null state of an expression is derived from its form and type, and from the null state of variables involved in it.</span></span>

### <a name="literals"></a><span data-ttu-id="1e1a5-196">Literales</span><span class="sxs-lookup"><span data-stu-id="1e1a5-196">Literals</span></span>

<span data-ttu-id="1e1a5-197">El estado null de un literal `null` es "maybe null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-197">The null state of a `null` literal is "maybe null".</span></span> <span data-ttu-id="1e1a5-198">El estado null de un literal `default` que se está convirtiendo en un tipo que se sabe que no es un tipo de valor que no acepta valores NULL es "quizás null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-198">The null state of a `default` literal that is being converted to a type that is known not to be a nonnullable value type is "maybe null".</span></span> <span data-ttu-id="1e1a5-199">El estado null de cualquier otro literal es "not null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-199">The null state of any other literal is "not null".</span></span>

### <a name="simple-names"></a><span data-ttu-id="1e1a5-200">Nombres simples</span><span class="sxs-lookup"><span data-stu-id="1e1a5-200">Simple names</span></span>

<span data-ttu-id="1e1a5-201">Si un `simple_name` no se clasifica como un valor, su estado NULL es "not null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-201">If a `simple_name` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="1e1a5-202">En caso contrario, se trata de una expresión de la que se realiza un seguimiento y su estado NULL es el estado NULL del que se realiza el seguimiento en esta ubicación de origen.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-202">Otherwise it is a tracked expression, and its null state is its tracked null state at this source location.</span></span>

### <a name="member-access"></a><span data-ttu-id="1e1a5-203">Acceso a miembros</span><span class="sxs-lookup"><span data-stu-id="1e1a5-203">Member access</span></span>

<span data-ttu-id="1e1a5-204">Si un `member_access` no se clasifica como un valor, su estado NULL es "not null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-204">If a `member_access` is not classified as a value, its null state is "not null".</span></span> <span data-ttu-id="1e1a5-205">De lo contrario, si se trata de una expresión de la que se realiza un seguimiento, su estado NULL es el estado NULL del que se realiza el seguimiento en esta ubicación de origen.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-205">Otherwise, if it is a tracked expression, its null state is its tracked null state at this source location.</span></span> <span data-ttu-id="1e1a5-206">De lo contrario, su estado NULL es el estado Null predeterminado de su tipo.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-206">Otherwise its null state is the default null state of its type.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="1e1a5-207">Expresiones de invocación</span><span class="sxs-lookup"><span data-stu-id="1e1a5-207">Invocation expressions</span></span>

<span data-ttu-id="1e1a5-208">Si un `invocation_expression` invoca a un miembro declarado con uno o varios atributos para un comportamiento especial nulo, el estado NULL se determina por esos atributos.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-208">If an `invocation_expression` invokes a member that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="1e1a5-209">De lo contrario, el estado null de la expresión es el estado Null predeterminado de su tipo.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-209">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="element-access"></a><span data-ttu-id="1e1a5-210">Acceso a elementos</span><span class="sxs-lookup"><span data-stu-id="1e1a5-210">Element access</span></span>

<span data-ttu-id="1e1a5-211">Si un `element_access` invoca un indexador que se declara con uno o más atributos para un comportamiento especial nulo, el estado NULL se determina por esos atributos.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-211">If an `element_access` invokes an indexer that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="1e1a5-212">De lo contrario, el estado null de la expresión es el estado Null predeterminado de su tipo.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-212">Otherwise the null state of the expression is the default null state of its type.</span></span>

### <a name="base-access"></a><span data-ttu-id="1e1a5-213">Acceso base</span><span class="sxs-lookup"><span data-stu-id="1e1a5-213">Base access</span></span>

<span data-ttu-id="1e1a5-214">Si `B` denota el tipo base del tipo envolvente, `base.I` tiene el mismo estado null que `((B)this).I` y `base[E]` tiene el mismo estado null que `((B)this)[E]`.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-214">If `B` denotes the base type of the enclosing type, `base.I` has the same null state as `((B)this).I` and `base[E]` has the same null state as `((B)this)[E]`.</span></span>

### <a name="default-expressions"></a><span data-ttu-id="1e1a5-215">Expresiones predeterminadas</span><span class="sxs-lookup"><span data-stu-id="1e1a5-215">Default expressions</span></span>

<span data-ttu-id="1e1a5-216">`default(T)` tiene el estado null "nonnull" si se sabe que `T` es un tipo de valor que no acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-216">`default(T)` has the null state "non-null" if `T` is known to be a nonnullable value type.</span></span> <span data-ttu-id="1e1a5-217">En caso contrario, tiene el estado null "maybe null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-217">Otherwise it has the null state "maybe null".</span></span>

### <a name="null-conditional-expressions"></a><span data-ttu-id="1e1a5-218">Expresiones condicionales null</span><span class="sxs-lookup"><span data-stu-id="1e1a5-218">Null-conditional expressions</span></span>

<span data-ttu-id="1e1a5-219">Una `null_conditional_expression` tiene el estado null "maybe null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-219">A `null_conditional_expression` has the null state "maybe null".</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="1e1a5-220">Expresiones de conversión</span><span class="sxs-lookup"><span data-stu-id="1e1a5-220">Cast expressions</span></span>

<span data-ttu-id="1e1a5-221">Si una expresión de conversión `(T)E` invoca una conversión definida por el usuario, el estado null de la expresión es el estado Null predeterminado para su tipo.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-221">If a cast expression `(T)E` invokes a user-defined conversion, then the null state of the expression is the default null state for its type.</span></span> <span data-ttu-id="1e1a5-222">De lo contrario, si `T` tiene un rendimiento nulo (que*acepta valores* null o es *desconocido*), el estado NULL es "maybe null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-222">Otherwise, if `T` is null-yielding (*nullable* or *unknown*) then the null state is "maybe null".</span></span> <span data-ttu-id="1e1a5-223">En caso contrario, el estado NULL es el mismo que el estado null de `E`.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-223">Otherwise the null state is the same as the null state of `E`.</span></span>

### <a name="await-expressions"></a><span data-ttu-id="1e1a5-224">Expresiones Await</span><span class="sxs-lookup"><span data-stu-id="1e1a5-224">Await expressions</span></span>

<span data-ttu-id="1e1a5-225">El estado null de `await E` es el estado Null predeterminado de su tipo.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-225">The null state of `await E` is the default null state of its type.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="1e1a5-226">El operador `as`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-226">The `as` operator</span></span>

<span data-ttu-id="1e1a5-227">Una expresión `as` tiene el estado null "maybe null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-227">An `as` expression has the null state "maybe null".</span></span>

### <a name="the-null-coalescing-operator"></a><span data-ttu-id="1e1a5-228">Operador de uso combinado de null</span><span class="sxs-lookup"><span data-stu-id="1e1a5-228">The null-coalescing operator</span></span>

<span data-ttu-id="1e1a5-229">`E1 ?? E2` tiene el mismo estado null que `E2`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-229">`E1 ?? E2` has the same null state as `E2`</span></span>

### <a name="the-conditional-operator"></a><span data-ttu-id="1e1a5-230">El operador condicional</span><span class="sxs-lookup"><span data-stu-id="1e1a5-230">The conditional operator</span></span>

<span data-ttu-id="1e1a5-231">El estado null de `E1 ? E2 : E3` es "not null" si el estado null de `E2` y `E3` es "not null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-231">The null state of `E1 ? E2 : E3` is "not null" if the null state of both `E2` and `E3` are "not null".</span></span> <span data-ttu-id="1e1a5-232">En caso contrario, es "quizás null".</span><span class="sxs-lookup"><span data-stu-id="1e1a5-232">Otherwise it is "maybe null".</span></span>

### <a name="query-expressions"></a><span data-ttu-id="1e1a5-233">Expresiones de consulta</span><span class="sxs-lookup"><span data-stu-id="1e1a5-233">Query expressions</span></span>

<span data-ttu-id="1e1a5-234">El estado null de una expresión de consulta es el estado Null predeterminado de su tipo.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-234">The null state of a query expression is the default null state of its type.</span></span>

### <a name="assignment-operators"></a><span data-ttu-id="1e1a5-235">Operadores de asignación</span><span class="sxs-lookup"><span data-stu-id="1e1a5-235">Assignment operators</span></span>

<span data-ttu-id="1e1a5-236">`E1 = E2` y `E1 op= E2` tienen el mismo estado null que `E2` una vez aplicadas las conversiones implícitas.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-236">`E1 = E2` and `E1 op= E2` have the same null state as `E2` after any implicit conversions have been applied.</span></span>

### <a name="unary-and-binary-operators"></a><span data-ttu-id="1e1a5-237">Operadores unarios y binarios</span><span class="sxs-lookup"><span data-stu-id="1e1a5-237">Unary and binary operators</span></span>

<span data-ttu-id="1e1a5-238">Si un operador unario o binario invoca a un operador definido por el usuario que se declara con uno o más atributos para un comportamiento especial nulo, los atributos determinan el estado null.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-238">If a unary or binary operator invokes an user-defined operator that is declared with one or more attributes for special null behavior, the null state is determined by those attributes.</span></span> <span data-ttu-id="1e1a5-239">De lo contrario, el estado null de la expresión es el estado Null predeterminado de su tipo.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-239">Otherwise the null state of the expression is the default null state of its type.</span></span>

<span data-ttu-id="1e1a5-240">***¿Algo especial de hacer para `+` binarios en cadenas y delegados?***</span><span class="sxs-lookup"><span data-stu-id="1e1a5-240">***Something special to do for binary `+` over strings and delegates?***</span></span>

### <a name="expressions-that-propagate-null-state"></a><span data-ttu-id="1e1a5-241">Expresiones que propagan el estado null</span><span class="sxs-lookup"><span data-stu-id="1e1a5-241">Expressions that propagate null state</span></span>

<span data-ttu-id="1e1a5-242">`(E)`, `checked(E)` y `unchecked(E)` tienen el mismo estado null que `E`.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-242">`(E)`, `checked(E)` and `unchecked(E)` all have the same null state as `E`.</span></span>

### <a name="expressions-that-are-never-null"></a><span data-ttu-id="1e1a5-243">Expresiones que nunca son NULL</span><span class="sxs-lookup"><span data-stu-id="1e1a5-243">Expressions that are never null</span></span>

<span data-ttu-id="1e1a5-244">El estado null de los siguientes formatos de expresión siempre es "not null":</span><span class="sxs-lookup"><span data-stu-id="1e1a5-244">The null state of the following expression forms is always "not null":</span></span>

- <span data-ttu-id="1e1a5-245">`this` access</span><span class="sxs-lookup"><span data-stu-id="1e1a5-245">`this` access</span></span>
- <span data-ttu-id="1e1a5-246">cadenas interpoladas</span><span class="sxs-lookup"><span data-stu-id="1e1a5-246">interpolated strings</span></span>
- <span data-ttu-id="1e1a5-247">Expresiones `new` (expresiones de objeto, delegado, objeto anónimo y creación de matriz)</span><span class="sxs-lookup"><span data-stu-id="1e1a5-247">`new` expressions (object, delegate, anonymous object and array creation expressions)</span></span>
- <span data-ttu-id="1e1a5-248">Expresiones `typeof`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-248">`typeof` expressions</span></span>
- <span data-ttu-id="1e1a5-249">Expresiones `nameof`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-249">`nameof` expressions</span></span>
- <span data-ttu-id="1e1a5-250">funciones anónimas (métodos anónimos y expresiones lambda)</span><span class="sxs-lookup"><span data-stu-id="1e1a5-250">anonymous functions (anonymous methods and lambda expressions)</span></span>
- <span data-ttu-id="1e1a5-251">Expresiones permisivo nulas</span><span class="sxs-lookup"><span data-stu-id="1e1a5-251">null-forgiving expressions</span></span>
- <span data-ttu-id="1e1a5-252">Expresiones `is`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-252">`is` expressions</span></span>

## <a name="type-inference"></a><span data-ttu-id="1e1a5-253">Inferencia de tipos</span><span class="sxs-lookup"><span data-stu-id="1e1a5-253">Type inference</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="1e1a5-254">Inferencia de tipos para `var`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-254">Type inference for `var`</span></span>

<span data-ttu-id="1e1a5-255">El tipo deducido para las variables locales declaradas con `var` es informado por el estado null de la expresión de inicialización.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-255">The type inferred for local variables declared with `var` is informed by the null state of the initializing expression.</span></span>

```csharp
var x = E;
```

<span data-ttu-id="1e1a5-256">Si el tipo de `E` es un tipo de referencia que acepta valores NULL `C?` y el estado null de `E` es "not null", el tipo deducido para `x` es `C`.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-256">If the type of `E` is a nullable reference type `C?` and the null state of `E` is "not null" then the type inferred for `x` is `C`.</span></span> <span data-ttu-id="1e1a5-257">De lo contrario, el tipo deducido es el tipo de `E`.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-257">Otherwise, the inferred type is the type of `E`.</span></span>

<span data-ttu-id="1e1a5-258">La nulabilidad del tipo deducido para `x` se determina como se describió anteriormente, basándose en el contexto de la anotación del `var`, como si el tipo se hubiera proporcionado explícitamente en esa posición.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-258">The nullability of the type inferred for `x` is determined as described above, based on the annotation context of the `var`, just as if the type had been given explicitly in that position.</span></span>

### <a name="type-inference-for-var"></a><span data-ttu-id="1e1a5-259">Inferencia de tipos para `var?`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-259">Type inference for `var?`</span></span>

<span data-ttu-id="1e1a5-260">El tipo deducido para las variables locales declaradas con `var?` es independiente del estado null de la expresión de inicialización.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-260">The type inferred for local variables declared with `var?` is independent of the null state of the initializing expression.</span></span>

```csharp
var? x = E;
```

<span data-ttu-id="1e1a5-261">Si el tipo `T` de `E` es un tipo de valor que acepta valores NULL o un tipo de referencia que acepta valores NULL, se `T`el tipo deducido para `x`.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-261">If the type `T` of `E` is a nullable value type or a nullable reference type then the type inferred for `x` is `T`.</span></span> <span data-ttu-id="1e1a5-262">De lo contrario, si `T` es un tipo de valor que no acepta valores NULL `S` se `S?`el tipo deducido.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-262">Otherwise, if `T` is a nonnullable value type `S` the type inferred is `S?`.</span></span> <span data-ttu-id="1e1a5-263">De lo contrario, si `T` es un tipo de referencia que no admite valores NULL `C` se `C?`el tipo deducido.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-263">Otherwise, if `T` is a nonnullable reference type `C` the type inferred is `C?`.</span></span> <span data-ttu-id="1e1a5-264">De lo contrario, la declaración no es válida.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-264">Otherwise, the declaration is illegal.</span></span>

<span data-ttu-id="1e1a5-265">La nulabilidad del tipo deducido para `x` siempre *admite valores NULL*.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-265">The nullability of the type inferred for `x` is always *nullable*.</span></span>

### <a name="generic-type-inference"></a><span data-ttu-id="1e1a5-266">Inferencia de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="1e1a5-266">Generic type inference</span></span>

<span data-ttu-id="1e1a5-267">La inferencia de tipos genéricos se ha mejorado para ayudar a decidir si los tipos de referencia deducidos deben admitir valores NULL o no.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-267">Generic type inference is enhanced to help decide whether inferred reference types should be nullable or not.</span></span> <span data-ttu-id="1e1a5-268">Se trata de un mejor esfuerzo y no produce advertencias en sí mismo, pero puede dar lugar a advertencias que aceptan valores NULL cuando los tipos deducidos de la sobrecarga seleccionada se aplican a los argumentos.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-268">This is a best effort, and does not in and of itself yield warnings, but may lead to nullable warnings when the inferred types of the selected overload are applied to the arguments.</span></span>

<span data-ttu-id="1e1a5-269">La inferencia de tipos no se basa en el contexto de anotación de los tipos entrantes.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-269">The type inference does not rely on the annotation context of incoming types.</span></span> <span data-ttu-id="1e1a5-270">En su lugar, se deduce una `type` que adquiere su propio contexto de anotación desde donde "habría sido" si se hubiera expresado explícitamente.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-270">Instead a `type` is inferred which acquires its own annotation context from where it "would have been" if it had been expressed explicitly.</span></span> <span data-ttu-id="1e1a5-271">Esto subraya el rol de inferencia de tipos por comodidad para lo que podría haber escrito.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-271">This underscores the role of type inference as a convenience for what you could have written yourself.</span></span>

<span data-ttu-id="1e1a5-272">Más concretamente, el contexto de anotación para un argumento de tipo deducido es el contexto del token que habría seguido de la `<...>` lista de parámetros de tipo, si hubiera sido uno; es decir, el nombre del método genérico que se va a llamar.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-272">More precisely, the annotation context for an inferred type argument is the context of the token that would have been followed by the `<...>` type parameter list, had there been one; i.e. the name of the generic method being called.</span></span> <span data-ttu-id="1e1a5-273">En el caso de las expresiones de consulta que se traducen en llamadas de este tipo, el contexto se toma de la palabra clave contextual inicial de la cláusula de consulta a partir de la cual se genera la llamada.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-273">For query expressions that translate to such calls, the context is taken from the initial contextual keyword of the query clause from which the call is generated.</span></span>

### <a name="the-first-phase"></a><span data-ttu-id="1e1a5-274">La primera fase</span><span class="sxs-lookup"><span data-stu-id="1e1a5-274">The first phase</span></span>

<span data-ttu-id="1e1a5-275">Los tipos de referencia que aceptan valores NULL fluyen en los límites de las expresiones iniciales, como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-275">Nullable reference types flow into the bounds from the initial expressions, as described below.</span></span> <span data-ttu-id="1e1a5-276">Además, se introducen dos nuevos tipos de límites, es decir, `null` y `default`.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-276">In addition, two new kinds of bounds, namely `null` and `default` are introduced.</span></span> <span data-ttu-id="1e1a5-277">Su finalidad es llevar a cabo las repeticiones de `null` o `default` en las expresiones de entrada, lo que puede hacer que un tipo deducido acepte valores NULL, incluso cuando no sea así.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-277">Their purpose is to carry through occurrences of `null` or `default` in the input expressions, which may cause an inferred type to be nullable, even when it otherwise wouldn't.</span></span> <span data-ttu-id="1e1a5-278">Esto funciona incluso para los tipos de *valor* que aceptan valores NULL, que se han mejorado para recoger la "nulación" en el proceso de inferencia.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-278">This works even for nullable *value* types, which are enhanced to pick up "nullness" in the inference process.</span></span>

<span data-ttu-id="1e1a5-279">La determinación de los límites que se van a agregar en la primera fase se ha mejorado como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-279">The determination of what bounds to add in the first phase are enhanced as follows:</span></span>

<span data-ttu-id="1e1a5-280">Si un `Ei` de argumento tiene un tipo de referencia, el tipo `U` utilizado para la inferencia depende del estado null de `Ei` así como de su tipo declarado:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-280">If an argument `Ei` has a reference type, the type `U` used for inference depends on the null state of `Ei` as well as its declared type:</span></span>
- <span data-ttu-id="1e1a5-281">Si el tipo declarado es un tipo de referencia que no admite valores NULL `U0` o un tipo de referencia que acepta valores NULL `U0?` entonces</span><span class="sxs-lookup"><span data-stu-id="1e1a5-281">If the declared type is a nonnullable reference type `U0` or a nullable reference type `U0?` then</span></span>
    - <span data-ttu-id="1e1a5-282">Si el estado null de `Ei` es "not null", `U` se `U0`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-282">if the null state of `Ei` is "not null" then `U` is `U0`</span></span>
    - <span data-ttu-id="1e1a5-283">Si el estado nulo de `Ei` es "quizás null", `U` se `U0?`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-283">if the null state of `Ei` is "maybe null" then `U` is `U0?`</span></span>
- <span data-ttu-id="1e1a5-284">De lo contrario, si `Ei` tiene un tipo declarado, `U` es ese tipo.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-284">Otherwise if `Ei` has a declared type, `U` is that type</span></span>
- <span data-ttu-id="1e1a5-285">De lo contrario, si se `null` `Ei`, `U` es el enlace especial `null`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-285">Otherwise if `Ei` is `null` then `U` is the special bound `null`</span></span>
- <span data-ttu-id="1e1a5-286">De lo contrario, si se `default` `Ei`, `U` es el enlace especial `default`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-286">Otherwise if `Ei` is `default` then `U` is the special bound `default`</span></span>
- <span data-ttu-id="1e1a5-287">En caso contrario, no se realiza ninguna inferencia.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-287">Otherwise no inference is made.</span></span>

### <a name="exact-upper-bound-and-lower-bound-inferences"></a><span data-ttu-id="1e1a5-288">Inferencias exactas, de límite superior y de límite inferior</span><span class="sxs-lookup"><span data-stu-id="1e1a5-288">Exact, upper-bound and lower-bound inferences</span></span>

<span data-ttu-id="1e1a5-289">En las inferencias *del* tipo `U` *al* tipo `V`, si `V` es un tipo de referencia que acepta valores NULL `V0?`, se usa `V0` en lugar de `V` en las cláusulas siguientes.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-289">In inferences *from* the type `U` *to* the type `V`, if `V` is a nullable reference type `V0?`, then `V0` is used instead of `V` in the following clauses.</span></span>
- <span data-ttu-id="1e1a5-290">Si `V` es una de las variables de tipo sin corregir, `U` se agrega como un límite inferior, superior o inferior como antes.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-290">If `V` is one of the unfixed type variables, `U` is added as an exact, upper or lower bound as before</span></span>
- <span data-ttu-id="1e1a5-291">De lo contrario, si `U` es `null` o `default`, no se realiza ninguna inferencia.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-291">Otherwise, if `U` is `null` or `default`, no inference is made</span></span>
- <span data-ttu-id="1e1a5-292">De lo contrario, si `U` es un tipo de referencia que acepta valores NULL `U0?`, se usa `U0` en lugar de `U` en las cláusulas posteriores.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-292">Otherwise, if `U` is a nullable reference type `U0?`, then `U0` is used instead of `U` in the subsequent clauses.</span></span>

<span data-ttu-id="1e1a5-293">La esencia es que la nulabilidad que pertenece directamente a una de las variables de tipo sin Fixed se conserva en sus límites.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-293">The essence is that nullability that pertains directly to one of the unfixed type variables is preserved into its bounds.</span></span> <span data-ttu-id="1e1a5-294">Por otra parte, para las inferencias que se recorren más en los tipos de origen y de destino, se omite la nulabilidad.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-294">For the inferences that recurse further into the source and target types, on the other hand, nullability is ignored.</span></span> <span data-ttu-id="1e1a5-295">Puede o no coincidir, pero si no es así, se emitirá una advertencia más adelante si se elige la sobrecarga y se aplica.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-295">It may or may not match, but if it doesn't, a warning will be issued later if the overload is chosen and applied.</span></span>

### <a name="fixing"></a><span data-ttu-id="1e1a5-296">Corrección de</span><span class="sxs-lookup"><span data-stu-id="1e1a5-296">Fixing</span></span>

<span data-ttu-id="1e1a5-297">Actualmente, la especificación no realiza un buen trabajo de describir lo que sucede cuando varios límites son convertibles entre sí, pero son diferentes.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-297">The spec currently does not do a good job of describing what happens when multiple bounds are identity convertible to each other, but are different.</span></span> <span data-ttu-id="1e1a5-298">Esto puede ocurrir entre `object` y `dynamic`, entre tipos de tupla que solo difieren en los nombres de elemento, entre los tipos construidos en él y ahora también entre `C` y `C?` para los tipos de referencia.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-298">This may happen between `object` and `dynamic`, between tuple types that differ only in element names, between types constructed thereof and now also between `C` and `C?` for reference types.</span></span>

<span data-ttu-id="1e1a5-299">Además, debemos propagar la "nulación" de las expresiones de entrada al tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-299">In addition we need to propagate "nullness" from the input expressions to the result type.</span></span> 

<span data-ttu-id="1e1a5-300">Para controlar estos agregamos más fases para corregir, que ahora es:</span><span class="sxs-lookup"><span data-stu-id="1e1a5-300">To handle these we add more phases to fixing, which is now:</span></span>

1. <span data-ttu-id="1e1a5-301">Recopile todos los tipos de todos los límites como candidatos, quitando `?` de todos los tipos de referencia que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="1e1a5-301">Gather all the types in all the bounds as candidates, removing `?` from all that are nullable reference types</span></span>
2. <span data-ttu-id="1e1a5-302">Eliminación de candidatos en función de los requisitos de los límites exactos, inferiores y superiores (manteniendo `null` y `default` límites)</span><span class="sxs-lookup"><span data-stu-id="1e1a5-302">Eliminate candidates based on requirements of exact, lower and upper bounds (keeping `null` and `default` bounds)</span></span>
3. <span data-ttu-id="1e1a5-303">Eliminación de candidatos que no tienen una conversión implícita a todos los demás candidatos</span><span class="sxs-lookup"><span data-stu-id="1e1a5-303">Eliminate candidates that do not have an implicit conversion to all the other candidates</span></span>
4. <span data-ttu-id="1e1a5-304">Si los candidatos restantes no tienen conversiones de identidad entre sí, se produce un error en la inferencia de tipos</span><span class="sxs-lookup"><span data-stu-id="1e1a5-304">If the remaining candidates do not all have identity conversions to one another, then type inference fails</span></span>
5. <span data-ttu-id="1e1a5-305">*Combine* los candidatos restantes como se describe a continuación</span><span class="sxs-lookup"><span data-stu-id="1e1a5-305">*Merge* the remaining candidates as described below</span></span>
6. <span data-ttu-id="1e1a5-306">Si el candidato resultante es un tipo de referencia o un tipo de valor que no acepta valores NULL y *todos* los límites exactos o *alguno* de los límites inferiores son tipos de valor que aceptan valores NULL, tipos de referencia que aceptan valores null, `null` o `default`, se agrega `?` al candidato resultante, convirtiéndolo en un tipo de valor que acepta valores NULL o un tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-306">If the resulting candidate is a reference type or a nonnullable value type and *all* of the exact bounds or *any* of the lower bounds are nullable value types, nullable reference types, `null` or `default`, then `?` is added to the resulting candidate, making it a nullable value type or reference type.</span></span>

<span data-ttu-id="1e1a5-307">La *combinación* se describe entre dos tipos candidatos.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-307">*Merging* is described between two candidate types.</span></span> <span data-ttu-id="1e1a5-308">Es transitiva y conmutable, por lo que los candidatos se pueden combinar en cualquier orden con el mismo resultado final.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-308">It is transitive and commutative, so the candidates can be merged in any order with the same ultimate result.</span></span> <span data-ttu-id="1e1a5-309">No se define si los dos tipos candidatos no son una identidad convertible entre sí.</span><span class="sxs-lookup"><span data-stu-id="1e1a5-309">It is undefined if the two candidate types are not identity convertible to each other.</span></span>

<span data-ttu-id="1e1a5-310">La función *Merge* toma dos tipos candidatos y una dirección ( *+* o *-* ):</span><span class="sxs-lookup"><span data-stu-id="1e1a5-310">The *Merge* function takes two candidate types and a direction (*+* or *-*):</span></span>

- <span data-ttu-id="1e1a5-311">*Merge*(`T`, `T`, *d*) = t</span><span class="sxs-lookup"><span data-stu-id="1e1a5-311">*Merge*(`T`, `T`, *d*) = T</span></span>
- <span data-ttu-id="1e1a5-312">*Merge*(`S`, `T?`, *+* ) = *Merge*(`S?`, `T`, *+* ) = *Merge*(`S`, `T`, *+* )`?`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-312">*Merge*(`S`, `T?`, *+*) = *Merge*(`S?`, `T`, *+*) = *Merge*(`S`, `T`, *+*)`?`</span></span>
- <span data-ttu-id="1e1a5-313">*Merge*(`S`, `T?`, *-* ) = *Merge*(`S?`, `T`, *-* ) = *Merge*(`S`, `T`, *-* )</span><span class="sxs-lookup"><span data-stu-id="1e1a5-313">*Merge*(`S`, `T?`, *-*) = *Merge*(`S?`, `T`, *-*) = *Merge*(`S`, `T`, *-*)</span></span>
- <span data-ttu-id="1e1a5-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+* ) = `C<`*combinar*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *DN*)`>`, *donde*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-314">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="1e1a5-315">`di` =  *+* si el parámetro de tipo "th" `i`de `C<...>` es covariante</span><span class="sxs-lookup"><span data-stu-id="1e1a5-315">`di` = *+* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="1e1a5-316">`di` =  *-* si el parámetro de tipo "th" `i`de `C<...>` es compensa-o invariable</span><span class="sxs-lookup"><span data-stu-id="1e1a5-316">`di` = *-* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="1e1a5-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-* ) = `C<`*combinar*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *DN*)`>`, *donde*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-317">*Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-*) = `C<`*Merge*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *dn*)`>`, *where*</span></span>
    - <span data-ttu-id="1e1a5-318">`di` =  *-* si el parámetro de tipo "th" `i`de `C<...>` es covariante</span><span class="sxs-lookup"><span data-stu-id="1e1a5-318">`di` = *-* if the `i`'th type parameter of `C<...>` is covariant</span></span>
    - <span data-ttu-id="1e1a5-319">`di` =  *+* si el parámetro de tipo "th" `i`de `C<...>` es compensa-o invariable</span><span class="sxs-lookup"><span data-stu-id="1e1a5-319">`di` = *+* if the `i`'th type parameter of `C<...>` is contra- or invariant</span></span>
- <span data-ttu-id="1e1a5-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*combinar*(`S1`, `T1`, *d*)`n1,...,`*combinar*(`Sn`, `Tn`, *d*) `nn)`, *donde*</span><span class="sxs-lookup"><span data-stu-id="1e1a5-320">*Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*Merge*(`S1`, `T1`, *d*)`n1,...,`*Merge*(`Sn`, `Tn`, *d*) `nn)`, *where*</span></span>
    - <span data-ttu-id="1e1a5-321">`ni` no está presente si `si` y `ti` difieren, o si ambos están ausentes</span><span class="sxs-lookup"><span data-stu-id="1e1a5-321">`ni` is absent if `si` and `ti` differ, or if both are absent</span></span>
    - <span data-ttu-id="1e1a5-322">`ni` es `si` si `si` y `ti` son iguales</span><span class="sxs-lookup"><span data-stu-id="1e1a5-322">`ni` is `si` if `si` and `ti` are the same</span></span>
- <span data-ttu-id="1e1a5-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span><span class="sxs-lookup"><span data-stu-id="1e1a5-323">*Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`</span></span>

## <a name="warnings"></a><span data-ttu-id="1e1a5-324">Advertencias</span><span class="sxs-lookup"><span data-stu-id="1e1a5-324">Warnings</span></span>

### <a name="potential-null-assignment"></a><span data-ttu-id="1e1a5-325">Posible asignación null</span><span class="sxs-lookup"><span data-stu-id="1e1a5-325">Potential null assignment</span></span>

### <a name="potential-null-dereference"></a><span data-ttu-id="1e1a5-326">Desreferenciación potencial null</span><span class="sxs-lookup"><span data-stu-id="1e1a5-326">Potential null dereference</span></span>

### <a name="constraint-nullability-mismatch"></a><span data-ttu-id="1e1a5-327">No coincide la nulabilidad de restricciones</span><span class="sxs-lookup"><span data-stu-id="1e1a5-327">Constraint nullability mismatch</span></span>

### <a name="nullable-types-in-disabled-annotation-context"></a><span data-ttu-id="1e1a5-328">Tipos que aceptan valores NULL en el contexto de anotación deshabilitado</span><span class="sxs-lookup"><span data-stu-id="1e1a5-328">Nullable types in disabled annotation context</span></span>

## <a name="attributes-for-special-null-behavior"></a><span data-ttu-id="1e1a5-329">Atributos para un comportamiento especial nulo</span><span class="sxs-lookup"><span data-stu-id="1e1a5-329">Attributes for special null behavior</span></span>


