---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483545"
---
# <a name="simplified-null-argument-checking"></a><span data-ttu-id="82dd2-101">Comprobación de argumentos null simplificada</span><span class="sxs-lookup"><span data-stu-id="82dd2-101">Simplified Null Argument Checking</span></span>

## <a name="summary"></a><span data-ttu-id="82dd2-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="82dd2-102">Summary</span></span>
<span data-ttu-id="82dd2-103">Esta propuesta proporciona una sintaxis simplificada para validar que los argumentos de método no se `null` y producir `ArgumentNullException` adecuadamente.</span><span class="sxs-lookup"><span data-stu-id="82dd2-103">This proposal provides a simplified syntax for validating method arguments are not `null` and throwing `ArgumentNullException` appropriately.</span></span>

## <a name="motivation"></a><span data-ttu-id="82dd2-104">Motivación</span><span class="sxs-lookup"><span data-stu-id="82dd2-104">Motivation</span></span>
<span data-ttu-id="82dd2-105">El trabajo de diseño de tipos de referencia que aceptan valores NULL ha provocado el examen del código necesario para `null` la validación de los argumentos.</span><span class="sxs-lookup"><span data-stu-id="82dd2-105">The work on designing nullable reference types has caused us to examine the code necessary for `null` argument validation.</span></span> <span data-ttu-id="82dd2-106">Dado que NRT no afecta a los desarrolladores de ejecución de código, todavía debe agregar `if (arg is null) throw` código de placa de la caldera incluso en los proyectos que están totalmente `null` limpios.</span><span class="sxs-lookup"><span data-stu-id="82dd2-106">Given that NRT doesn't affect code execution developers still must add `if (arg is null) throw` boiler plate code even in projects which are fully `null` clean.</span></span> <span data-ttu-id="82dd2-107">Esto nos dio el deseo de explorar una sintaxis mínima para la validación de argumentos `null` en el lenguaje.</span><span class="sxs-lookup"><span data-stu-id="82dd2-107">This gave us the desire to explore a minimal syntax for argument `null` validation in the language.</span></span> 

<span data-ttu-id="82dd2-108">Aunque se espera que esta sintaxis de validación de parámetros `null` se empareje con frecuencia con NRT, la propuesta es totalmente independiente.</span><span class="sxs-lookup"><span data-stu-id="82dd2-108">While this `null` parameter validation syntax is expected to pair frequently with NRT the proposal is fully independent of it.</span></span> <span data-ttu-id="82dd2-109">La sintaxis se puede usar independientemente de las directivas de `#nullable`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-109">The syntax can be used independent of `#nullable` directives.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="82dd2-110">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="82dd2-110">Detailed Design</span></span> 

### <a name="null-validation-parameter-syntax"></a><span data-ttu-id="82dd2-111">Sintaxis de parámetro de validación null</span><span class="sxs-lookup"><span data-stu-id="82dd2-111">Null validation parameter syntax</span></span>
<span data-ttu-id="82dd2-112">El operador de signo de exclamación, `!`, se puede colocar después de un nombre de parámetro en una C# lista de parámetros y esto hará que el compilador emita el código de comprobación de `null` estándar para ese parámetro.</span><span class="sxs-lookup"><span data-stu-id="82dd2-112">The bang operator, `!`, can be positioned after a parameter name in a parameter list and this will cause the C# compiler to emit standard `null` checking code for that parameter.</span></span> <span data-ttu-id="82dd2-113">Esto se conoce como `null` sintaxis de parámetros de validación.</span><span class="sxs-lookup"><span data-stu-id="82dd2-113">This is referred to as `null` validation parameter syntax.</span></span> <span data-ttu-id="82dd2-114">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="82dd2-114">For example:</span></span>

``` csharp
void M(string name!) {
    ...
}
```

<span data-ttu-id="82dd2-115">Se traducirá en:</span><span class="sxs-lookup"><span data-stu-id="82dd2-115">Will be translated into:</span></span>

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

<span data-ttu-id="82dd2-116">La comprobación de `null` generada se realizará antes que cualquier código creado por el desarrollador en el método.</span><span class="sxs-lookup"><span data-stu-id="82dd2-116">The generated `null` check will occur before any developer authored code in the method.</span></span> <span data-ttu-id="82dd2-117">Cuando varios parámetros contienen el operador `!`, las comprobaciones se producirán en el mismo orden en que se declaran los parámetros.</span><span class="sxs-lookup"><span data-stu-id="82dd2-117">When multiple parameters contain the `!` operator then the checks will occur in the same order as the parameters are declared.</span></span>

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

<span data-ttu-id="82dd2-118">La comprobación será específica para la igualdad de referencia en `null`, no invoca `==` ni ningún operador definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="82dd2-118">The check will be specifically for reference equality to `null`, it does not invoke `==` or any user defined operators.</span></span> <span data-ttu-id="82dd2-119">Esto también significa que el operador `!` solo se puede Agregar a los parámetros cuyo tipo se puede probar para determinar si son iguales en `null`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-119">This also means the `!` operator can only be added to parameters whose type can be tested for equality against `null`.</span></span> <span data-ttu-id="82dd2-120">Esto significa que no se puede usar en un parámetro cuyo tipo se sabe que es un tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="82dd2-120">This means it can't be used on a parameter whose type is known to be a value type.</span></span>

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

<span data-ttu-id="82dd2-121">En el caso de un constructor, la validación de `null` se realizará antes que cualquier otro código del constructor.</span><span class="sxs-lookup"><span data-stu-id="82dd2-121">In the case of a constructor the `null` validation will occur before any other code in the constructor.</span></span> <span data-ttu-id="82dd2-122">Esto incluye:</span><span class="sxs-lookup"><span data-stu-id="82dd2-122">That includes:</span></span> 

- <span data-ttu-id="82dd2-123">Encadenar a otros constructores con `this` o `base`</span><span class="sxs-lookup"><span data-stu-id="82dd2-123">Chaining to other constructors with `this` or `base`</span></span> 
- <span data-ttu-id="82dd2-124">Inicializadores de campo que se producen implícitamente en el constructor</span><span class="sxs-lookup"><span data-stu-id="82dd2-124">Field initializers which implicitly occur in the constructor</span></span>

<span data-ttu-id="82dd2-125">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="82dd2-125">For example:</span></span>

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

<span data-ttu-id="82dd2-126">Se traducirá aproximadamente a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="82dd2-126">Will be roughly translated into the following:</span></span>

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

<span data-ttu-id="82dd2-127">Nota: este no es un C# código válido, sino solo una aproximación de lo que hace la implementación.</span><span class="sxs-lookup"><span data-stu-id="82dd2-127">Note: this is not legal C# code but instead just an approximation of what the implementation does.</span></span> 

<span data-ttu-id="82dd2-128">La sintaxis del parámetro de validación `null` también será válida en las listas de parámetros lambda.</span><span class="sxs-lookup"><span data-stu-id="82dd2-128">The `null` validation parameter syntax will also be valid on lambda parameter lists.</span></span> <span data-ttu-id="82dd2-129">Esto es válido incluso en la sintaxis de parámetro único que carece de paréntesis.</span><span class="sxs-lookup"><span data-stu-id="82dd2-129">This is valid even in the single parameter syntax that lacks parens.</span></span>

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

<span data-ttu-id="82dd2-130">La sintaxis también es válida en los parámetros de los métodos de iterador.</span><span class="sxs-lookup"><span data-stu-id="82dd2-130">The syntax is also valid on parameters to iterator methods.</span></span> <span data-ttu-id="82dd2-131">A diferencia de otro código en el iterador, la `null` validación se producirá cuando se invoque el método iterador, no cuando se recorre el enumerador subyacente.</span><span class="sxs-lookup"><span data-stu-id="82dd2-131">Unlike other code in the iterator the `null` validation will occur when the iterator method is invoked, not when the underlying enumerator is walked.</span></span> <span data-ttu-id="82dd2-132">Esto se aplica a los iteradores tradicionales o `async`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-132">This is true for traditional or `async` iterators.</span></span>

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

<span data-ttu-id="82dd2-133">El operador `!` solo se puede usar para las listas de parámetros que tienen un cuerpo de método asociado.</span><span class="sxs-lookup"><span data-stu-id="82dd2-133">The `!` operator can only be used for parameter lists which have an associated method body.</span></span> <span data-ttu-id="82dd2-134">Esto significa que no se puede utilizar en una definición de método `abstract`, `interface`, `delegate` o `partial`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-134">This means it cannot be used in an `abstract` method, `interface`, `delegate` or `partial` method definition.</span></span>

### <a name="extending-is-null"></a><span data-ttu-id="82dd2-135">La extensión es null</span><span class="sxs-lookup"><span data-stu-id="82dd2-135">Extending is null</span></span>
<span data-ttu-id="82dd2-136">Los tipos para los que la expresión `is null` es válida se extenderán para incluir parámetros de tipo sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="82dd2-136">The types for which the expression `is null` is valid will be extended to include unconstrained type parameters.</span></span> <span data-ttu-id="82dd2-137">Esto le permitirá rellenar el intento de comprobar `null` en todos los tipos que sean válidos para una comprobación de `null`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-137">This will allow it to fill the intent of checking for `null` on all types which a `null` check is valid.</span></span> <span data-ttu-id="82dd2-138">En concreto, los tipos que no se conocen definitivamente como tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="82dd2-138">Specifically that is types which are not definitely known to be value types.</span></span> <span data-ttu-id="82dd2-139">Por ejemplo, los parámetros de tipo que están restringidos a `struct` no se pueden usar con esta sintaxis.</span><span class="sxs-lookup"><span data-stu-id="82dd2-139">For example Type parameters which are constrained to `struct` cannot be used with this syntax.</span></span>

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

<span data-ttu-id="82dd2-140">El comportamiento de `is null` en un parámetro de tipo será el mismo que el de `== null` hoy.</span><span class="sxs-lookup"><span data-stu-id="82dd2-140">The behavior of `is null` on a type parameter will be the same as `== null` today.</span></span> <span data-ttu-id="82dd2-141">En los casos en los que se crea una instancia del parámetro de tipo como un tipo de valor, el código se evaluará como `false`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-141">In the cases where the type parameter is instantiated as a value type the code will be evaluated as `false`.</span></span> <span data-ttu-id="82dd2-142">En los casos en los que se trata de un tipo de referencia, el código realizará una comprobación de `is null` adecuada.</span><span class="sxs-lookup"><span data-stu-id="82dd2-142">For cases where it is a reference type the code will do a proper `is null` check.</span></span>

### <a name="intersection-with-nullable-reference-types"></a><span data-ttu-id="82dd2-143">Intersección con tipos de referencia que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="82dd2-143">Intersection with Nullable Reference Types</span></span>
<span data-ttu-id="82dd2-144">Cualquier parámetro que tenga un operador de `!` aplicado a su nombre comenzará con el estado que acepta valores NULL que no se `null`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-144">Any parameter which has a `!` operator applied to it's name will start with the nullable state being not `null`.</span></span> <span data-ttu-id="82dd2-145">Esto es así incluso si el tipo del propio parámetro es potencialmente `null`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-145">This is true even if the type of the parameter itself is potentially `null`.</span></span> <span data-ttu-id="82dd2-146">Esto puede ocurrir con un tipo que acepta valores NULL explícitamente, como por ejemplo `string?`, o con un parámetro de tipo sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="82dd2-146">That can occur with an explicitly nullable type, such as say `string?`, or with an unconstrained type parameter.</span></span> 

<span data-ttu-id="82dd2-147">Cuando una sintaxis de `!` en parámetros se combina con un tipo que acepta valores NULL explícitamente en el parámetro, el compilador emitirá una advertencia:</span><span class="sxs-lookup"><span data-stu-id="82dd2-147">When a `!` syntax on parameters is combined with an explicitly nullable type on the parameter then a warning will be issued by the compiler:</span></span>

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a><span data-ttu-id="82dd2-148">Problemas abiertos</span><span class="sxs-lookup"><span data-stu-id="82dd2-148">Open Issues</span></span>
<span data-ttu-id="82dd2-149">None</span><span class="sxs-lookup"><span data-stu-id="82dd2-149">None</span></span>

## <a name="considerations"></a><span data-ttu-id="82dd2-150">Consideraciones</span><span class="sxs-lookup"><span data-stu-id="82dd2-150">Considerations</span></span>

### <a name="constructors"></a><span data-ttu-id="82dd2-151">Constructores</span><span class="sxs-lookup"><span data-stu-id="82dd2-151">Constructors</span></span>
<span data-ttu-id="82dd2-152">La generación de código para constructores significa que hay un cambio de comportamiento pequeño pero observable al pasar de la validación de `null` estándar hoy y la sintaxis del parámetro de validación `null` (`!`).</span><span class="sxs-lookup"><span data-stu-id="82dd2-152">The code generation for constructors means there is a small, but observable, behavior change when moving from standard `null` validation today and the `null` validation parameter syntax (`!`).</span></span> <span data-ttu-id="82dd2-153">La comprobación de `null` en la validación estándar se produce después de los inicializadores de campo y de las llamadas a `base` o `this`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-153">The `null` check in standard validation occurs after both field initializers and any `base` or `this` calls.</span></span> <span data-ttu-id="82dd2-154">Esto significa que un desarrollador no puede migrar necesariamente el 100% de la validación de `null` a la nueva sintaxis.</span><span class="sxs-lookup"><span data-stu-id="82dd2-154">This means a developer can't necessarily migrate 100% of their `null` validation to the new syntax.</span></span> <span data-ttu-id="82dd2-155">Los constructores requieren al menos alguna inspección.</span><span class="sxs-lookup"><span data-stu-id="82dd2-155">Constructors at least require some inspection.</span></span>

<span data-ttu-id="82dd2-156">Después de la explicación, se decidió que es muy poco probable que se produzcan problemas de adopción significativos.</span><span class="sxs-lookup"><span data-stu-id="82dd2-156">After discussion though it was decided that this is very unlikely to cause any significant adoption issues.</span></span> <span data-ttu-id="82dd2-157">Es más lógico que la comprobación de `null` se ejecute antes que cualquier lógica del constructor.</span><span class="sxs-lookup"><span data-stu-id="82dd2-157">It's more logical that the `null` check run before any logic in the constructor does.</span></span> <span data-ttu-id="82dd2-158">Puede volver a visitar si se detectan problemas de compatibilidad significativos.</span><span class="sxs-lookup"><span data-stu-id="82dd2-158">Can revisit if significant compat issues are discovered.</span></span>

### <a name="warning-when-mixing--and-"></a><span data-ttu-id="82dd2-159">¿ADVERTENCIA al mezclar?</span><span class="sxs-lookup"><span data-stu-id="82dd2-159">Warning when mixing ?</span></span> <span data-ttu-id="82dd2-160">etc!</span><span class="sxs-lookup"><span data-stu-id="82dd2-160">and !</span></span>
<span data-ttu-id="82dd2-161">Hubo una explicación larga sobre si se debe emitir o no una advertencia cuando se aplica la sintaxis de `!` a un parámetro que se escribe explícitamente en un tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="82dd2-161">There was a lengthy discussion on whether or not a warning should be issued when the `!` syntax is applied to a parameter which is explicitly typed to a nullable type.</span></span> <span data-ttu-id="82dd2-162">En la superficie, parece una declaración de absurdo del desarrollador, pero hay casos en los que las jerarquías de tipos podrían obligar a los desarrolladores a realizar esta situación.</span><span class="sxs-lookup"><span data-stu-id="82dd2-162">On the surface it seems like a nonsensical declaration by the developer but there are cases where type hierarchies could force developers into such a situation.</span></span> 

<span data-ttu-id="82dd2-163">Considere la siguiente jerarquía de clase en una serie de ensamblados (suponiendo que todos se compilan con la comprobación de `null` habilitada):</span><span class="sxs-lookup"><span data-stu-id="82dd2-163">Consider the following class hierarchy across a series of assemblies (assuming all are compiled with `null` checking enabled):</span></span>

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

<span data-ttu-id="82dd2-164">Aquí el autor de `C3` decidió agregar la validación de `null` al `o`de parámetros.</span><span class="sxs-lookup"><span data-stu-id="82dd2-164">Here the author of `C3` decided to add `null` validation to the parameter `o`.</span></span> <span data-ttu-id="82dd2-165">Esto está completamente en línea con la finalidad de usar la característica.</span><span class="sxs-lookup"><span data-stu-id="82dd2-165">This is completely in line with how the feature is intended to be used.</span></span>

<span data-ttu-id="82dd2-166">Ahora Imagine en una fecha posterior, el autor de Assembly2 decide agregar la siguiente invalidación:</span><span class="sxs-lookup"><span data-stu-id="82dd2-166">Now imagine at a later date the author of Assembly2 decides to add the following override:</span></span>

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

<span data-ttu-id="82dd2-167">Esto lo permiten los tipos de referencia que aceptan valores NULL, ya que es válido para que el contrato sea más flexible para las posiciones de entrada.</span><span class="sxs-lookup"><span data-stu-id="82dd2-167">This is allowed by nullable reference types as it's legal to make the contract more flexible for input positions.</span></span> <span data-ttu-id="82dd2-168">En general, la característica NRT permite una co/contravarianza razonable en el parámetro o la nulabilidad.</span><span class="sxs-lookup"><span data-stu-id="82dd2-168">The NRT feature in general allows for reasonable co/contravariance on parameter / return nullability.</span></span> <span data-ttu-id="82dd2-169">Sin embargo, el lenguaje realiza la comprobación de la co/contravarianza basándose en la invalidación más específica, no en la declaración original.</span><span class="sxs-lookup"><span data-stu-id="82dd2-169">However the language does the co/contravariance checking based on the most specific override, not the original declaration.</span></span> <span data-ttu-id="82dd2-170">Esto significa que el autor de Assembly3 recibirá una advertencia sobre el tipo de `o` no coincidente y tendrá que cambiar la firma a lo siguiente para eliminarla:</span><span class="sxs-lookup"><span data-stu-id="82dd2-170">This means the author of Assembly3 will get a warning about the type of `o` not matching and will need to change the signature to the following to eliminate it:</span></span> 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

<span data-ttu-id="82dd2-171">En este momento, el autor de Assembly3 tiene algunas opciones:</span><span class="sxs-lookup"><span data-stu-id="82dd2-171">At this point the author of Assembly3 has a few choices:</span></span>

- <span data-ttu-id="82dd2-172">Pueden aceptar o suprimir la advertencia sobre `object?` y `object` no coinciden.</span><span class="sxs-lookup"><span data-stu-id="82dd2-172">They can accept / suppress the warning about `object?` and `object` mismatch.</span></span>
- <span data-ttu-id="82dd2-173">Pueden aceptar o suprimir la advertencia sobre `object?` y `!` no coinciden.</span><span class="sxs-lookup"><span data-stu-id="82dd2-173">They can accept / suppress the warning about `object?` and `!` mismatch.</span></span>
- <span data-ttu-id="82dd2-174">Solo pueden quitar la comprobación de validación del `null` (eliminar `!` y realizar la comprobación explícita)</span><span class="sxs-lookup"><span data-stu-id="82dd2-174">They can just remove the `null` validation check (delete `!` and do explicit checking)</span></span>

<span data-ttu-id="82dd2-175">Este es un escenario real, pero por ahora la idea es avanzar con la advertencia.</span><span class="sxs-lookup"><span data-stu-id="82dd2-175">This is a real scenario but for now the idea is to move forward with the warning.</span></span> <span data-ttu-id="82dd2-176">Si la advertencia se produce con más frecuencia de lo previsto, podemos quitarla más adelante (el valor inverso no es cierto).</span><span class="sxs-lookup"><span data-stu-id="82dd2-176">If it turns out the warning happens more frequently than we anticipate then we can remove it later (the reverse is not true).</span></span>

### <a name="implicit-property-setter-arguments"></a><span data-ttu-id="82dd2-177">Argumentos del establecedor de propiedad implícito</span><span class="sxs-lookup"><span data-stu-id="82dd2-177">Implicit property setter arguments</span></span>
<span data-ttu-id="82dd2-178">El argumento `value` de un parámetro es implícito y no aparece en ninguna lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="82dd2-178">The `value` argument of a parameter is implicit and does not appear in any parameter list.</span></span> <span data-ttu-id="82dd2-179">Esto significa que no puede ser un destino de esta característica.</span><span class="sxs-lookup"><span data-stu-id="82dd2-179">That means it cannot be a target of this feature.</span></span> <span data-ttu-id="82dd2-180">La sintaxis del establecedor de propiedad se puede extender para incluir una lista de parámetros que permita aplicar el operador `!`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-180">The property setter syntax could be extended to include a parameter list to allow the `!` operator to be applied.</span></span> <span data-ttu-id="82dd2-181">Pero esto reduce la idea de esta característica, lo que simplifica la validación de `null`.</span><span class="sxs-lookup"><span data-stu-id="82dd2-181">But that cuts against the idea of this feature making `null` validation simpler.</span></span> <span data-ttu-id="82dd2-182">Como tal, el argumento `value` implícito no funcionará con esta característica.</span><span class="sxs-lookup"><span data-stu-id="82dd2-182">As such the implicit `value` argument just won't work with this feature.</span></span>

## <a name="future-considerations"></a><span data-ttu-id="82dd2-183">Consideraciones futuras</span><span class="sxs-lookup"><span data-stu-id="82dd2-183">Future Considerations</span></span>
<span data-ttu-id="82dd2-184">None</span><span class="sxs-lookup"><span data-stu-id="82dd2-184">None</span></span>
