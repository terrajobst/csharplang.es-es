---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "79484025"
---
# <a name="readonly-references"></a><span data-ttu-id="ff17f-101">Referencias readonly</span><span class="sxs-lookup"><span data-stu-id="ff17f-101">Readonly references</span></span>

* <span data-ttu-id="ff17f-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="ff17f-102">[x] Proposed</span></span>
* <span data-ttu-id="ff17f-103">[x] prototipo</span><span class="sxs-lookup"><span data-stu-id="ff17f-103">[x] Prototype</span></span>
* <span data-ttu-id="ff17f-104">[x] implementación: iniciada</span><span class="sxs-lookup"><span data-stu-id="ff17f-104">[x] Implementation: Started</span></span>
* <span data-ttu-id="ff17f-105">[] Especificación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="ff17f-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="ff17f-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="ff17f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="ff17f-107">La característica "referencias de solo lectura" es realmente un grupo de características que aprovechan la eficacia de pasar variables por referencia, pero sin exponer los datos a modificaciones:</span><span class="sxs-lookup"><span data-stu-id="ff17f-107">The "readonly references" feature is actually a group of features that leverage the efficiency of passing variables by reference, but without exposing the data to modifications:</span></span>  
- <span data-ttu-id="ff17f-108">`in` de parámetros</span><span class="sxs-lookup"><span data-stu-id="ff17f-108">`in` parameters</span></span>
- <span data-ttu-id="ff17f-109">Devoluciones de `ref readonly`</span><span class="sxs-lookup"><span data-stu-id="ff17f-109">`ref readonly` returns</span></span>
- <span data-ttu-id="ff17f-110">Structs `readonly`</span><span class="sxs-lookup"><span data-stu-id="ff17f-110">`readonly` structs</span></span>
- <span data-ttu-id="ff17f-111">`ref`/métodos de extensión `in`</span><span class="sxs-lookup"><span data-stu-id="ff17f-111">`ref`/`in` extension methods</span></span>
- <span data-ttu-id="ff17f-112">`ref readonly` variables locales</span><span class="sxs-lookup"><span data-stu-id="ff17f-112">`ref readonly` locals</span></span>
- <span data-ttu-id="ff17f-113">`ref` de expresiones condicionales</span><span class="sxs-lookup"><span data-stu-id="ff17f-113">`ref` conditional expressions</span></span>

## <a name="passing-arguments-as-readonly-references"></a><span data-ttu-id="ff17f-114">Pasar argumentos como referencias de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="ff17f-114">Passing arguments as readonly references.</span></span>

<span data-ttu-id="ff17f-115">Existe una propuesta existente que toca este tema https://github.com/dotnet/roslyn/issues/115 como un caso especial de parámetros de solo lectura sin entrar en muchos detalles.</span><span class="sxs-lookup"><span data-stu-id="ff17f-115">There is an existing proposal that touches this topic https://github.com/dotnet/roslyn/issues/115 as a special case of readonly parameters without going into many details.</span></span>
<span data-ttu-id="ff17f-116">Aquí solo quiero reconocer que la idea por sí misma no es muy nueva.</span><span class="sxs-lookup"><span data-stu-id="ff17f-116">Here I just want to acknowledge that the idea by itself is not very new.</span></span>

### <a name="motivation"></a><span data-ttu-id="ff17f-117">Motivación</span><span class="sxs-lookup"><span data-stu-id="ff17f-117">Motivation</span></span>

<span data-ttu-id="ff17f-118">Antes de esta característica C# no tenía una manera eficaz de expresar el deseo de pasar variables de struct a llamadas a métodos con fines de solo lectura sin intención de modificar.</span><span class="sxs-lookup"><span data-stu-id="ff17f-118">Prior to this feature C# did not have an efficient way of expressing a desire to pass struct variables into method calls for readonly purposes with no intention of modifying.</span></span> <span data-ttu-id="ff17f-119">El paso normal de los argumentos por valor implica la copia, lo que agrega costos innecesarios.</span><span class="sxs-lookup"><span data-stu-id="ff17f-119">Regular by-value argument passing implies copying, which adds unnecessary costs.</span></span>  <span data-ttu-id="ff17f-120">Que dirige a los usuarios para que usen el paso de argumentos por referencia y que se basen en comentarios o documentación para indicar que no se supone que el destinatario de la llamada modifique los datos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-120">That drives users to use by-ref argument passing and rely on comments/documentation to indicate that the data is not supposed to be mutated by the callee.</span></span> <span data-ttu-id="ff17f-121">No es una buena solución por muchas razones.</span><span class="sxs-lookup"><span data-stu-id="ff17f-121">It is not a good solution for many reasons.</span></span>  
<span data-ttu-id="ff17f-122">Los ejemplos son numerosos operadores matemáticos-vector/Matrix en las bibliotecas de gráficos, como [XNA](https://msdn.microsoft.com/library/bb194944.aspx) , se sabe que tienen operandos Ref únicamente debido a consideraciones de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ff17f-122">The examples are numerous - vector/matrix math operators in graphics libraries like [XNA](https://msdn.microsoft.com/library/bb194944.aspx) are known to have ref operands purely because of performance considerations.</span></span> <span data-ttu-id="ff17f-123">Hay código en el propio compilador Roslyn que usa estructuras para evitar asignaciones y, a continuación, los pasa por referencia para evitar la copia de costos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-123">There is code in Roslyn compiler itself that uses structs to avoid allocations and then passes them by reference to avoid copying costs.</span></span>

### <a name="solution-in-parameters"></a><span data-ttu-id="ff17f-124">Solución (parámetros de`in`)</span><span class="sxs-lookup"><span data-stu-id="ff17f-124">Solution (`in` parameters)</span></span>

<span data-ttu-id="ff17f-125">Del mismo modo que los parámetros de `out`, `in` parámetros se pasan como referencias administradas con garantías adicionales del destinatario.</span><span class="sxs-lookup"><span data-stu-id="ff17f-125">Similarly to the `out` parameters, `in` parameters are passed as managed references with additional guarantees from the callee.</span></span>  
<span data-ttu-id="ff17f-126">A diferencia de `out` parámetros que _debe_ asignar el destinatario antes de cualquier otro uso, el destinatario no puede asignar los parámetros `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-126">Unlike `out` parameters which _must_ be assigned by the callee before any other use, `in` parameters cannot be assigned by the callee at all.</span></span>

<span data-ttu-id="ff17f-127">Como resultado `in` los parámetros permiten la eficacia del paso de argumentos indirectos sin exponer argumentos a mutaciones por parte del destinatario.</span><span class="sxs-lookup"><span data-stu-id="ff17f-127">As a result `in` parameters allow for effectiveness of indirect argument passing without exposing arguments to mutations by the callee.</span></span>

### <a name="declaring-in-parameters"></a><span data-ttu-id="ff17f-128">Declaración de parámetros `in`</span><span class="sxs-lookup"><span data-stu-id="ff17f-128">Declaring `in` parameters</span></span>

<span data-ttu-id="ff17f-129">`in` parámetros se declaran mediante `in` palabra clave como un modificador en la Signatura del parámetro.</span><span class="sxs-lookup"><span data-stu-id="ff17f-129">`in` parameters are declared by using `in` keyword as a modifier in the parameter signature.</span></span>

<span data-ttu-id="ff17f-130">Para todos los propósitos, el parámetro `in` se trata como una variable `readonly`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-130">For all purposes the `in` parameter is treated as a `readonly` variable.</span></span> <span data-ttu-id="ff17f-131">La mayoría de las restricciones en el uso de los parámetros de `in` dentro del método son las mismas que con los campos de `readonly`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-131">Most of the restrictions on the use of `in` parameters inside the method are the same as with `readonly` fields.</span></span>

> <span data-ttu-id="ff17f-132">En realidad, un parámetro `in` puede representar un campo `readonly`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-132">Indeed an `in` parameter may represent a `readonly` field.</span></span> <span data-ttu-id="ff17f-133">La similitud de las restricciones no es una coincidencia.</span><span class="sxs-lookup"><span data-stu-id="ff17f-133">Similarity of restrictions is not a coincidence.</span></span>

<span data-ttu-id="ff17f-134">Por ejemplo, los campos de un parámetro `in` que tiene un tipo de estructura se clasifican de forma recursiva como variables de `readonly`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-134">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- <span data-ttu-id="ff17f-135">`in` parámetros se permiten en cualquier lugar donde se permitan los parámetros de ByVal normales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-135">`in` parameters are allowed anywhere where ordinary byval parameters are allowed.</span></span> <span data-ttu-id="ff17f-136">Esto incluye indizadores, operadores (incluidas las conversiones), delegados, expresiones lambda, funciones locales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-136">This includes indexers, operators (including conversions), delegates, lambdas, local functions.</span></span>

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- <span data-ttu-id="ff17f-137">`in` no se permite en combinación con `out` o con cualquier cosa con la que `out` no se combina.</span><span class="sxs-lookup"><span data-stu-id="ff17f-137">`in` is not allowed in combination with `out` or with anything that `out` does not combine with.</span></span>

- <span data-ttu-id="ff17f-138">No se puede sobrecargar en `ref`/`out``in` /diferencias.</span><span class="sxs-lookup"><span data-stu-id="ff17f-138">It is not permitted to overload on `ref`/`out`/`in` differences.</span></span>

- <span data-ttu-id="ff17f-139">Se permite sobrecargar las diferencias ordinarias de ByVal y `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-139">It is permitted to overload on ordinary byval and `in` differences.</span></span>

- <span data-ttu-id="ff17f-140">Para el propósito de OHI (sobrecargar, ocultar, implementar), `in` se comporta de forma similar a un parámetro `out`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-140">For the purpose of OHI (Overloading, Hiding, Implementing), `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="ff17f-141">Se aplican las mismas reglas.</span><span class="sxs-lookup"><span data-stu-id="ff17f-141">All the same rules apply.</span></span>
<span data-ttu-id="ff17f-142">Por ejemplo, el método de reemplazo tendrá que coincidir con `in` parámetros con `in` parámetros de un tipo que se pueda convertir en identidades.</span><span class="sxs-lookup"><span data-stu-id="ff17f-142">For example the overriding method will have to match `in` parameters with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="ff17f-143">Para el propósito de las conversiones de grupo de delegado/lambda/método, `in` se comporta de forma similar a un parámetro de `out`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-143">For the purpose of delegate/lambda/method group conversions, `in` behaves similarly to an `out` parameter.</span></span>
<span data-ttu-id="ff17f-144">Las expresiones lambda y los candidatos de conversión de grupos de métodos aplicables tendrán que coincidir con `in` parámetros del delegado de destino con `in` parámetros de un tipo que se pueda convertir en identidades.</span><span class="sxs-lookup"><span data-stu-id="ff17f-144">Lambdas and applicable method group conversion candidates will have to match `in` parameters of the target delegate with `in` parameters of an identity-convertible type.</span></span>

- <span data-ttu-id="ff17f-145">Para el propósito de la varianza genérica, `in` parámetros no son variantes.</span><span class="sxs-lookup"><span data-stu-id="ff17f-145">For the purpose of generic variance, `in` parameters are nonvariant.</span></span>

> <span data-ttu-id="ff17f-146">Nota: no hay advertencias en `in` parámetros que tengan tipos de referencia o primitivos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-146">NOTE: There are no warnings on `in` parameters that have reference or primitives types.</span></span>
<span data-ttu-id="ff17f-147">Puede que no sea puntual en general, pero en algunos casos el usuario debe/desea pasar primitivos como `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-147">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="ff17f-148">Ejemplos: invalidar un método genérico como `Method(in T param)` cuando `T` se sustituye por `int`, o cuando tiene métodos como `Volatile.Read(in int location)`</span><span class="sxs-lookup"><span data-stu-id="ff17f-148">Examples - overriding a generic method like `Method(in T param)` when `T` was substituted to be `int`, or when having methods like `Volatile.Read(in int location)`</span></span>
>
> <span data-ttu-id="ff17f-149">Es posible tener un analizador que advierte en casos de uso ineficaz de los parámetros de `in`, pero las reglas para dicho análisis serían demasiado aproximados para formar parte de una especificación de lenguaje.</span><span class="sxs-lookup"><span data-stu-id="ff17f-149">It is conceivable to have an analyzer that warns in cases of inefficient use of `in` parameters, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="use-of-in-at-call-sites-in-arguments"></a><span data-ttu-id="ff17f-150">Uso de `in` en sitios de llamada.</span><span class="sxs-lookup"><span data-stu-id="ff17f-150">Use of `in` at call sites.</span></span> <span data-ttu-id="ff17f-151">(argumentos`in`)</span><span class="sxs-lookup"><span data-stu-id="ff17f-151">(`in` arguments)</span></span>

<span data-ttu-id="ff17f-152">Hay dos maneras de pasar argumentos a `in` parámetros.</span><span class="sxs-lookup"><span data-stu-id="ff17f-152">There are two ways to pass arguments to `in` parameters.</span></span>

#### <a name="in-arguments-can-match-in-parameters"></a><span data-ttu-id="ff17f-153">`in` argumentos pueden coincidir con `in` parámetros:</span><span class="sxs-lookup"><span data-stu-id="ff17f-153">`in` arguments can match `in` parameters:</span></span>

<span data-ttu-id="ff17f-154">Un argumento con un modificador `in` en el sitio de llamada puede coincidir con `in` parámetros.</span><span class="sxs-lookup"><span data-stu-id="ff17f-154">An argument with an `in` modifier at the call site can match `in` parameters.</span></span>

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- <span data-ttu-id="ff17f-155">`in` argumento debe ser un valor l (\*) _legible_ .</span><span class="sxs-lookup"><span data-stu-id="ff17f-155">`in` argument must be a _readable_ LValue(\*).</span></span>
<span data-ttu-id="ff17f-156">Ejemplo: `M1(in 42)` no es válido</span><span class="sxs-lookup"><span data-stu-id="ff17f-156">Example: `M1(in 42)` is invalid</span></span>

> <span data-ttu-id="ff17f-157">(\*) La noción de valor [l/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) varía entre los lenguajes.</span><span class="sxs-lookup"><span data-stu-id="ff17f-157">(\*) The notion of [LValue/RValue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) vary between languages.</span></span>  
<span data-ttu-id="ff17f-158">Aquí, por valor l I significa una expresión que representa una ubicación a la que se puede hacer referencia directamente.</span><span class="sxs-lookup"><span data-stu-id="ff17f-158">Here, by LValue I mean an expression that represent a location that can be referred to directly.</span></span>
<span data-ttu-id="ff17f-159">Y RValue significa una expresión que produce un resultado temporal que no se conserva por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="ff17f-159">And RValue means an expression that yields a temporary result which does not persist on its own.</span></span>  

- <span data-ttu-id="ff17f-160">En concreto, es válido pasar `readonly` campos, `in` parámetros u otras variables `readonly` de forma formal como argumentos de `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-160">In particular it is valid to pass `readonly` fields, `in` parameters or other formally `readonly` variables as `in` arguments.</span></span>
<span data-ttu-id="ff17f-161">Ejemplo: `dictionary[in Guid.Empty]` es legal.</span><span class="sxs-lookup"><span data-stu-id="ff17f-161">Example: `dictionary[in Guid.Empty]` is legal.</span></span> <span data-ttu-id="ff17f-162">`Guid.Empty` es un campo de solo lectura estático.</span><span class="sxs-lookup"><span data-stu-id="ff17f-162">`Guid.Empty` is a static readonly field.</span></span>

- <span data-ttu-id="ff17f-163">`in` argumento debe tener la _identidad_ del tipo que se pueda convertir al tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="ff17f-163">`in` argument must have type _identity-convertible_ to the type of the parameter.</span></span>
<span data-ttu-id="ff17f-164">Ejemplo: `M1<object>(in Guid.Empty)` no es válido.</span><span class="sxs-lookup"><span data-stu-id="ff17f-164">Example: `M1<object>(in Guid.Empty)` is invalid.</span></span> <span data-ttu-id="ff17f-165">`Guid.Empty` no es _convertible en una identidad_ en `object`</span><span class="sxs-lookup"><span data-stu-id="ff17f-165">`Guid.Empty` is not _identity-convertible_ to `object`</span></span>

<span data-ttu-id="ff17f-166">La motivación de las reglas anteriores es que `in` argumentos garantizan el _alias_ de la variable de argumento.</span><span class="sxs-lookup"><span data-stu-id="ff17f-166">The motivation for the above rules is that `in` arguments guarantee _aliasing_ of the argument variable.</span></span> <span data-ttu-id="ff17f-167">El destinatario siempre recibe una referencia directa a la misma ubicación que representa el argumento.</span><span class="sxs-lookup"><span data-stu-id="ff17f-167">The callee always receives a direct reference to the same location as represented by the argument.</span></span>

- <span data-ttu-id="ff17f-168">en situaciones excepcionales en las que los argumentos de `in` deben estar desbordados por pila debido a las expresiones de `await` que se usan como operandos de la misma llamada, el comportamiento es el mismo que con los argumentos `out` y `ref`: Si la variable no se puede desbordar de manera referencial y transparente, se muestra un error.</span><span class="sxs-lookup"><span data-stu-id="ff17f-168">in rare situations when `in` arguments must be stack-spilled due to `await` expressions used as operands of the same call, the behavior is the same as with `out` and `ref` arguments - if the variable cannot be spilled in referentially-transparent manner, an error is reported.</span></span>

<span data-ttu-id="ff17f-169">Ejemplos:</span><span class="sxs-lookup"><span data-stu-id="ff17f-169">Examples:</span></span>
1. <span data-ttu-id="ff17f-170">`M1(in staticField, await SomethingAsync())` es válido.</span><span class="sxs-lookup"><span data-stu-id="ff17f-170">`M1(in staticField, await SomethingAsync())`  is valid.</span></span>
<span data-ttu-id="ff17f-171">`staticField` es un campo estático al que se puede tener acceso más de una vez sin efectos secundarios observables.</span><span class="sxs-lookup"><span data-stu-id="ff17f-171">`staticField` is a static field which can be accessed more than once without observable side effects.</span></span> <span data-ttu-id="ff17f-172">Por lo tanto, se pueden proporcionar el orden de los efectos secundarios y los requisitos de alias.</span><span class="sxs-lookup"><span data-stu-id="ff17f-172">Therefore both the order of side effects and aliasing requirements can be provided.</span></span>
2. <span data-ttu-id="ff17f-173">`M1(in RefReturningMethod(), await SomethingAsync())` producirá un error.</span><span class="sxs-lookup"><span data-stu-id="ff17f-173">`M1(in RefReturningMethod(), await SomethingAsync())`  will produce an error.</span></span>
<span data-ttu-id="ff17f-174">`RefReturningMethod()` es un método que devuelve `ref`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-174">`RefReturningMethod()` is a `ref` returning method.</span></span> <span data-ttu-id="ff17f-175">Una llamada al método puede tener efectos secundarios observables, por lo que debe evaluarse antes que el operando `SomethingAsync()`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-175">A method call may have observable side effects, therefore it must be evaluated before the `SomethingAsync()` operand.</span></span> <span data-ttu-id="ff17f-176">Sin embargo, el resultado de la invocación es una referencia que no se puede conservar en el punto de suspensión de `await`, lo que hace imposible el requisito de referencia directa.</span><span class="sxs-lookup"><span data-stu-id="ff17f-176">However the result of the invocation is a reference that cannot be preserved across the `await` suspension point which make the direct reference requirement impossible.</span></span>

> <span data-ttu-id="ff17f-177">Nota: los errores de desbordamiento de pila se consideran limitaciones específicas de la implementación.</span><span class="sxs-lookup"><span data-stu-id="ff17f-177">NOTE: the stack spilling errors are considered to be implementation-specific limitations.</span></span> <span data-ttu-id="ff17f-178">Por lo tanto, no tienen efecto en la resolución de sobrecarga o la inferencia lambda.</span><span class="sxs-lookup"><span data-stu-id="ff17f-178">Therefore they do not have effect on overload resolution or lambda inference.</span></span>

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a><span data-ttu-id="ff17f-179">Los argumentos ByVal normales pueden coincidir `in` parámetros:</span><span class="sxs-lookup"><span data-stu-id="ff17f-179">Ordinary byval arguments can match `in` parameters:</span></span>

<span data-ttu-id="ff17f-180">Los argumentos normales sin modificadores pueden coincidir con los parámetros de `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-180">Regular arguments without modifiers can match `in` parameters.</span></span> <span data-ttu-id="ff17f-181">En tal caso, los argumentos tienen las mismas restricciones distendidas que los argumentos ByVal normales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-181">In such case the arguments have the same relaxed constraints as an ordinary byval arguments would have.</span></span>

<span data-ttu-id="ff17f-182">La motivación para este escenario es que `in` parámetros de las API pueden dar lugar a inconvenientes para el usuario cuando los argumentos no se pueden pasar como una referencia directa: por ejemplo, los literales, los resultados calculados o `await`-Ed o los argumentos que tienen tipos más específicos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-182">The motivation for this scenario is that `in` parameters in APIs may result in inconveniences for the user when arguments cannot be passed as a direct reference - ex: literals, computed or `await`-ed results or arguments that happen to have more specific types.</span></span>  
<span data-ttu-id="ff17f-183">Todos estos casos tienen una solución trivial de almacenar el valor del argumento en una local temporal del tipo adecuado y pasar ese local como un argumento `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-183">All these cases have a trivial solution of storing the argument value in a temporary local of appropriate type and passing that local as an `in` argument.</span></span>  
<span data-ttu-id="ff17f-184">Para reducir la necesidad de este compilador de código reutilizable puede realizar la misma transformación, si es necesario, cuando `in` modificador no está presente en el sitio de llamada.</span><span class="sxs-lookup"><span data-stu-id="ff17f-184">To reduce the need for such boilerplate code compiler can perform the same transformation, if needed, when `in` modifier is not present at the call site.</span></span>  

<span data-ttu-id="ff17f-185">Además, en algunos casos, como la invocación de operadores o `in` métodos de extensión, no hay ninguna manera sintáctica de especificar `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-185">In addition, in some cases, such as invocation of operators, or `in` extension methods, there is no syntactical way to specify `in` at all.</span></span> <span data-ttu-id="ff17f-186">Esto solo requiere especificar el comportamiento de los argumentos ByVal normales cuando coinciden `in` parámetros.</span><span class="sxs-lookup"><span data-stu-id="ff17f-186">That alone requires specifying the behavior of ordinary byval arguments when they match `in` parameters.</span></span>

<span data-ttu-id="ff17f-187">En concreto:</span><span class="sxs-lookup"><span data-stu-id="ff17f-187">In particular:</span></span>

- <span data-ttu-id="ff17f-188">es válido para pasar RValues.</span><span class="sxs-lookup"><span data-stu-id="ff17f-188">it is valid to pass RValues.</span></span>
<span data-ttu-id="ff17f-189">En tal caso, se pasa una referencia a un temporal.</span><span class="sxs-lookup"><span data-stu-id="ff17f-189">A reference to a temporary is passed in such case.</span></span>
<span data-ttu-id="ff17f-190">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ff17f-190">Example:</span></span>
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- <span data-ttu-id="ff17f-191">se permiten las conversiones implícitas.</span><span class="sxs-lookup"><span data-stu-id="ff17f-191">implicit conversions are allowed.</span></span>

> <span data-ttu-id="ff17f-192">En realidad, se trata de un caso especial de pasar un valor r</span><span class="sxs-lookup"><span data-stu-id="ff17f-192">This is actually a special case of passing an RValue</span></span>  

<span data-ttu-id="ff17f-193">En ese caso, se pasa una referencia a un valor convertido temporal que contiene.</span><span class="sxs-lookup"><span data-stu-id="ff17f-193">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="ff17f-194">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ff17f-194">Example:</span></span>
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- <span data-ttu-id="ff17f-195">en un caso de un receptor de un método de extensión `in` (en lugar de `ref` métodos de extensión), se permiten las _conversiones_ de RValues o implícitas.</span><span class="sxs-lookup"><span data-stu-id="ff17f-195">in a case of a receiver of an `in` extension method (as opposed to `ref` extension methods), RValues or implicit _this-argument-conversions_ are allowed.</span></span>
<span data-ttu-id="ff17f-196">En ese caso, se pasa una referencia a un valor convertido temporal que contiene.</span><span class="sxs-lookup"><span data-stu-id="ff17f-196">A reference to a temporary holding converted value is passed in such case.</span></span>
<span data-ttu-id="ff17f-197">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ff17f-197">Example:</span></span>
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
<span data-ttu-id="ff17f-198">En este documento se proporciona más información sobre `ref`/métodos de extensión `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-198">More information on `ref`/`in` extension methods is provided further in this document.</span></span>

- <span data-ttu-id="ff17f-199">la desbordación de argumentos debido a `await` operandos podría sobrepasar "por valor", si es necesario.</span><span class="sxs-lookup"><span data-stu-id="ff17f-199">argument spilling due to `await` operands could spill "by-value", if necessary.</span></span>
<span data-ttu-id="ff17f-200">En escenarios en los que no es posible proporcionar una referencia directa al argumento debido a que intervienen `await` se vuelca una copia del valor del argumento.</span><span class="sxs-lookup"><span data-stu-id="ff17f-200">In scenarios where providing a direct reference to the argument is not possible due to intervening `await` a copy of the argument's value is spilled instead.</span></span>  
<span data-ttu-id="ff17f-201">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ff17f-201">Example:</span></span>
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
<span data-ttu-id="ff17f-202">Dado que el resultado de una invocación de efectos secundarios es una referencia que no se puede conservar en `await` suspensión, se conservará en su lugar un temporal que contenga el valor real (tal como lo haría en un caso de parámetro ByVal normal).</span><span class="sxs-lookup"><span data-stu-id="ff17f-202">Since the result of a side-effecting invocation is a reference that cannot be preserved across `await` suspension, a temporary containing the actual value will be preserved instead (as it would in an ordinary byval parameter case).</span></span>

#### <a name="omitted-optional-arguments"></a><span data-ttu-id="ff17f-203">Argumentos opcionales omitidos</span><span class="sxs-lookup"><span data-stu-id="ff17f-203">Omitted optional arguments</span></span>

<span data-ttu-id="ff17f-204">Se permite que un parámetro `in` especifique un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ff17f-204">It is permitted for an `in` parameter to specify a default value.</span></span> <span data-ttu-id="ff17f-205">Esto hace que el argumento correspondiente sea opcional.</span><span class="sxs-lookup"><span data-stu-id="ff17f-205">That makes the corresponding argument optional.</span></span>

<span data-ttu-id="ff17f-206">Si se omite el argumento opcional en el sitio de llamada, se pasa el valor predeterminado a través de un temporal.</span><span class="sxs-lookup"><span data-stu-id="ff17f-206">Omitting optional argument at the call site results in passing the default value via a temporary.</span></span>

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a><span data-ttu-id="ff17f-207">Comportamiento de alias en general</span><span class="sxs-lookup"><span data-stu-id="ff17f-207">Aliasing behavior in general</span></span>

<span data-ttu-id="ff17f-208">Al igual que las variables `ref` y `out`, las variables de `in` son referencias o alias a las ubicaciones existentes.</span><span class="sxs-lookup"><span data-stu-id="ff17f-208">Just like `ref` and `out` variables, `in` variables are references/aliases to existing locations.</span></span>

<span data-ttu-id="ff17f-209">Aunque no se permite que el destinatario escriba en ellos, leer un parámetro `in` puede observar valores diferentes como un efecto secundario de otras evaluaciones.</span><span class="sxs-lookup"><span data-stu-id="ff17f-209">While callee is not allowed to write into them, reading an `in` parameter can observe different values as a side effect of other evaluations.</span></span>

<span data-ttu-id="ff17f-210">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ff17f-210">Example:</span></span>

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a><span data-ttu-id="ff17f-211">`in` parámetros y la captura de variables locales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-211">`in` parameters and capturing of local variables.</span></span>  
<span data-ttu-id="ff17f-212">Para la captura de lambda/Async `in` parámetros se comportan igual que los parámetros `out` y `ref`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-212">For the purpose of lambda/async capturing `in` parameters behave the same as `out` and `ref` parameters.</span></span>

- <span data-ttu-id="ff17f-213">`in` parámetros no se pueden capturar en un cierre</span><span class="sxs-lookup"><span data-stu-id="ff17f-213">`in` parameters cannot be captured in a closure</span></span>
- <span data-ttu-id="ff17f-214">no se permiten parámetros `in` en métodos de iterador</span><span class="sxs-lookup"><span data-stu-id="ff17f-214">`in` parameters are not allowed in iterator methods</span></span>
- <span data-ttu-id="ff17f-215">no se permiten parámetros `in` en métodos Async</span><span class="sxs-lookup"><span data-stu-id="ff17f-215">`in` parameters are not allowed in async methods</span></span>

### <a name="temporary-variables"></a><span data-ttu-id="ff17f-216">Variables temporales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-216">Temporary variables.</span></span>  
<span data-ttu-id="ff17f-217">Algunos usos de `in` paso de parámetros pueden requerir el uso indirecto de una variable local temporal:</span><span class="sxs-lookup"><span data-stu-id="ff17f-217">Some uses of `in` parameter passing may require indirect use of a temporary local variable:</span></span>  
- <span data-ttu-id="ff17f-218">`in` argumentos se pasan siempre como alias directos cuando Call-site utiliza `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-218">`in` arguments are always passed as direct aliases when call-site uses `in`.</span></span> <span data-ttu-id="ff17f-219">No se usa nunca en este caso.</span><span class="sxs-lookup"><span data-stu-id="ff17f-219">Temporary is never used in such case.</span></span>
- <span data-ttu-id="ff17f-220">`in` argumentos no deben ser alias directos cuando Call-site no usa `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-220">`in` arguments are not required to be direct aliases when call-site does not use `in`.</span></span> <span data-ttu-id="ff17f-221">Si el argumento no es un valor l, se puede usar un temporal.</span><span class="sxs-lookup"><span data-stu-id="ff17f-221">When argument is not an LValue, a temporary may be used.</span></span>
- <span data-ttu-id="ff17f-222">`in` parámetro puede tener un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ff17f-222">`in` parameter may have default value.</span></span> <span data-ttu-id="ff17f-223">Cuando se omite el argumento correspondiente en el sitio de la llamada, el valor predeterminado se pasa a través de un temporal.</span><span class="sxs-lookup"><span data-stu-id="ff17f-223">When corresponding argument is omitted at the call site, the default value are passed via a temporary.</span></span>
- <span data-ttu-id="ff17f-224">`in` argumentos pueden tener conversiones implícitas, incluidas las que no conservan la identidad.</span><span class="sxs-lookup"><span data-stu-id="ff17f-224">`in` arguments may have implicit conversions, including those that do not preserve identity.</span></span> <span data-ttu-id="ff17f-225">En esos casos se usa un temporal.</span><span class="sxs-lookup"><span data-stu-id="ff17f-225">A temporary is used in those cases.</span></span>
- <span data-ttu-id="ff17f-226">no se puede escribir en los receptores de llamadas de struct normales LValues (**caso existente**).</span><span class="sxs-lookup"><span data-stu-id="ff17f-226">receivers of ordinary struct calls may not be writeable LValues (**existing case!**).</span></span> <span data-ttu-id="ff17f-227">En esos casos se usa un temporal.</span><span class="sxs-lookup"><span data-stu-id="ff17f-227">A temporary is used in those cases.</span></span>

<span data-ttu-id="ff17f-228">La duración del argumento objetos temporales coincide con el ámbito más cercano del sitio de llamada.</span><span class="sxs-lookup"><span data-stu-id="ff17f-228">The life time of the argument temporaries matches the closest encompassing scope of the call-site.</span></span>

<span data-ttu-id="ff17f-229">La duración formal de las variables temporales es semánticamente significativa en escenarios que implican el análisis de escape de las variables devueltas por referencia.</span><span class="sxs-lookup"><span data-stu-id="ff17f-229">The formal life time of temporary variables is semantically significant in scenarios involving escape analysis of variables returned by reference.</span></span>

### <a name="metadata-representation-of-in-parameters"></a><span data-ttu-id="ff17f-230">Representación de metadatos de parámetros de `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-230">Metadata representation of `in` parameters.</span></span>
<span data-ttu-id="ff17f-231">Cuando se aplica `System.Runtime.CompilerServices.IsReadOnlyAttribute` a un parámetro ByRef, significa que el parámetro es un parámetro `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-231">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a byref parameter, it means that the parameter is an `in` parameter.</span></span>

<span data-ttu-id="ff17f-232">Además, si el método es *abstracto* o *virtual*, la firma de estos parámetros (y solo estos parámetros) debe tener `modreq[System.Runtime.InteropServices.InAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-232">In addition, if the method is *abstract* or *virtual*, then the signature of such parameters (and only such parameters) must have `modreq[System.Runtime.InteropServices.InAttribute]`.</span></span>

<span data-ttu-id="ff17f-233">**Motivación**: esto se hace para asegurarse de que, en un caso de que el método invalide e implemente los parámetros de `in` coincidan.</span><span class="sxs-lookup"><span data-stu-id="ff17f-233">**Motivation**: this is done to ensure that in a case of method overriding/implementing the `in` parameters match.</span></span>

<span data-ttu-id="ff17f-234">Los mismos requisitos se aplican a los métodos de `Invoke` en los delegados.</span><span class="sxs-lookup"><span data-stu-id="ff17f-234">Same requirements apply to `Invoke` methods in delegates.</span></span>

<span data-ttu-id="ff17f-235">**Motivación**: esto es para asegurarse de que los compiladores existentes no pueden simplemente omitir `readonly` al crear o asignar delegados.</span><span class="sxs-lookup"><span data-stu-id="ff17f-235">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when creating or assigning delegates.</span></span>

## <a name="returning-by-readonly-reference"></a><span data-ttu-id="ff17f-236">Devolución por referencia de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="ff17f-236">Returning by readonly reference.</span></span>

### <a name="motivation"></a><span data-ttu-id="ff17f-237">Motivación</span><span class="sxs-lookup"><span data-stu-id="ff17f-237">Motivation</span></span>
<span data-ttu-id="ff17f-238">La motivación de esta subcaracterística es aproximadamente simétrica a las razones del `in` los parámetros, evitando la copia, pero en el lado devuelto.</span><span class="sxs-lookup"><span data-stu-id="ff17f-238">The motivation for this sub-feature is roughly symmetrical to the reasons for the `in` parameters - avoiding copying, but on the returning side.</span></span> <span data-ttu-id="ff17f-239">Antes de esta característica, un método o un indexador tenían dos opciones: 1) devuelven por referencia y se exponen a posibles mutaciones o 2) devuelven por valor, lo que da como resultado la copia.</span><span class="sxs-lookup"><span data-stu-id="ff17f-239">Prior to this feature, a method or an indexer had two options: 1) return by reference and be exposed to possible mutations or 2) return by value which results in copying.</span></span>

### <a name="solution-ref-readonly-returns"></a><span data-ttu-id="ff17f-240">Solución (`ref readonly` devuelve)</span><span class="sxs-lookup"><span data-stu-id="ff17f-240">Solution (`ref readonly` returns)</span></span>  
<span data-ttu-id="ff17f-241">La característica permite que un miembro devuelva variables por referencia sin exponerlas a mutaciones.</span><span class="sxs-lookup"><span data-stu-id="ff17f-241">The feature allows a member to return variables by reference without exposing them to mutations.</span></span>

### <a name="declaring-ref-readonly-returning-members"></a><span data-ttu-id="ff17f-242">Declarar `ref readonly` devolver miembros</span><span class="sxs-lookup"><span data-stu-id="ff17f-242">Declaring `ref readonly` returning members</span></span>

<span data-ttu-id="ff17f-243">Una combinación de modificadores `ref readonly` en la firma devuelta se utiliza para indicar que el miembro devuelve una referencia de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="ff17f-243">A combination of modifiers `ref readonly` on the return signature is used to to indicate that the member returns a readonly reference.</span></span>

<span data-ttu-id="ff17f-244">Para todos los propósitos, un miembro de `ref readonly` se trata como una variable `readonly`, similar a `readonly` campos y `in` parámetros.</span><span class="sxs-lookup"><span data-stu-id="ff17f-244">For all purposes a `ref readonly` member is treated as a `readonly` variable - similar to `readonly` fields and `in` parameters.</span></span>

<span data-ttu-id="ff17f-245">Por ejemplo, los campos de `ref readonly` miembro que tiene un tipo de estructura se clasifican de forma recursiva como variables de `readonly`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-245">For example fields of `ref readonly` member which has a struct type are all recursively classified as `readonly` variables.</span></span> <span data-ttu-id="ff17f-246">-Se permite pasarlos como argumentos `in`, pero no como argumentos `ref` o `out`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-246">- It is permitted to pass them as `in` arguments, but not as `ref` or `out` arguments.</span></span>

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- <span data-ttu-id="ff17f-247">se permiten `ref readonly` devoluciones en los mismos lugares en los que se permiten `ref` devoluciones.</span><span class="sxs-lookup"><span data-stu-id="ff17f-247">`ref readonly` returns are allowed in the same places were `ref` returns are allowed.</span></span>
<span data-ttu-id="ff17f-248">Esto incluye indexadores, delegados, expresiones lambda y funciones locales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-248">This includes indexers, delegates, lambdas, local functions.</span></span>

- <span data-ttu-id="ff17f-249">No se puede sobrecargar en `ref`/de `ref readonly`/diferencias.</span><span class="sxs-lookup"><span data-stu-id="ff17f-249">It is not permitted to overload on `ref`/`ref readonly` /  differences.</span></span>

- <span data-ttu-id="ff17f-250">Se permite sobrecargar los valores de ByVal y `ref readonly` devuelven diferencias normales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-250">It is permitted to overload on ordinary byval and `ref readonly` return differences.</span></span>

- <span data-ttu-id="ff17f-251">Para el propósito de OHI (sobrecargar, ocultar, implementar), `ref readonly` es similar, pero distinto de `ref`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-251">For the purpose of OHI (Overloading, Hiding, Implementing), `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="ff17f-252">Por ejemplo, un método que invalide `ref readonly` uno, debe ser `ref readonly` y tener un tipo que se pueda convertir en identidades.</span><span class="sxs-lookup"><span data-stu-id="ff17f-252">For example the a method that overrides `ref readonly` one, must itself be `ref readonly` and have identity-convertible type.</span></span>

- <span data-ttu-id="ff17f-253">En el caso de las conversiones de grupo de delegado/lambda/método, `ref readonly` es similar, pero distinto de `ref`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-253">For the purpose of delegate/lambda/method group conversions, `ref readonly` is similar but distinct from `ref`.</span></span>
<span data-ttu-id="ff17f-254">Las expresiones lambda y los candidatos de conversión de grupos de métodos aplicables deben coincidir `ref readonly` devolución del delegado de destino con `ref readonly` devolución del tipo que se puede convertir en identidades.</span><span class="sxs-lookup"><span data-stu-id="ff17f-254">Lambdas and applicable method group conversion candidates have to match `ref readonly` return of the target delegate with `ref readonly` return of the type that is identity-convertible.</span></span>

- <span data-ttu-id="ff17f-255">Para el propósito de la varianza genérica, `ref readonly` devuelve son no variantes.</span><span class="sxs-lookup"><span data-stu-id="ff17f-255">For the purpose of generic variance, `ref readonly` returns are nonvariant.</span></span>

> <span data-ttu-id="ff17f-256">Nota: no hay ninguna advertencia en `ref readonly` devoluciones que tengan tipos de referencia o primitivos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-256">NOTE: There are no warnings on `ref readonly` returns that have reference or primitives types.</span></span>
<span data-ttu-id="ff17f-257">Puede que no sea puntual en general, pero en algunos casos el usuario debe/desea pasar primitivos como `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-257">It may be pointless in general, but in some cases user must/want to pass primitives as `in`.</span></span> <span data-ttu-id="ff17f-258">Ejemplos: invalidar un método genérico como `ref readonly T Method()` cuando `T` se sustituyó por `int`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-258">Examples - overriding a generic method like `ref readonly T Method()` when `T` was substituted to be `int`.</span></span>
>
><span data-ttu-id="ff17f-259">Es posible tener un analizador que advierte en casos de uso ineficaz de `ref readonly` devuelve, pero las reglas para dicho análisis serían demasiado aproximadas para formar parte de una especificación de lenguaje.</span><span class="sxs-lookup"><span data-stu-id="ff17f-259">It is conceivable to have an analyzer that warns in cases of inefficient use of `ref readonly` returns, but the rules for such analysis would be too fuzzy to be a part of a language specification.</span></span>

### <a name="returning-from-ref-readonly-members"></a><span data-ttu-id="ff17f-260">Devolver desde `ref readonly` miembros</span><span class="sxs-lookup"><span data-stu-id="ff17f-260">Returning from `ref readonly` members</span></span>
<span data-ttu-id="ff17f-261">Dentro del cuerpo del método, la sintaxis es la misma que con las devoluciones de referencia normales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-261">Inside the method body the syntax is the same as with regular ref returns.</span></span> <span data-ttu-id="ff17f-262">El `readonly` se deduce del método contenedor.</span><span class="sxs-lookup"><span data-stu-id="ff17f-262">The `readonly` will be inferred from the containing method.</span></span>

<span data-ttu-id="ff17f-263">La motivación es que `return ref readonly <expression>` es una duración innecesaria y solo permite discrepancias en la parte `readonly` que siempre daría lugar a errores.</span><span class="sxs-lookup"><span data-stu-id="ff17f-263">The motivation is that `return ref readonly <expression>` is unnecessary long and only allows for mismatches on the `readonly` part that would always result in errors.</span></span>
<span data-ttu-id="ff17f-264">Sin embargo, el `ref` es necesario para la coherencia con otros escenarios en los que se pasa algo a través de un alias estricto frente a un valor.</span><span class="sxs-lookup"><span data-stu-id="ff17f-264">The `ref` is, however, required for consistency with other scenarios where something is passed via strict aliasing vs. by value.</span></span>

> <span data-ttu-id="ff17f-265">A diferencia de lo que ocurre con los parámetros de `in`, `ref readonly` devuelve nunca a través de una copia local.</span><span class="sxs-lookup"><span data-stu-id="ff17f-265">Unlike the case with `in` parameters, `ref readonly` returns never return via a local copy.</span></span> <span data-ttu-id="ff17f-266">Es posible que la copia deje de existir inmediatamente después de que se devuelva este procedimiento.</span><span class="sxs-lookup"><span data-stu-id="ff17f-266">Considering that the copy would cease to exist immediately upon returning such practice would be pointless and dangerous.</span></span> <span data-ttu-id="ff17f-267">Por consiguiente `ref readonly` Returns son siempre referencias directas.</span><span class="sxs-lookup"><span data-stu-id="ff17f-267">Therefore `ref readonly` returns are always direct references.</span></span>

<span data-ttu-id="ff17f-268">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ff17f-268">Example:</span></span>

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- <span data-ttu-id="ff17f-269">Un argumento de `return ref` debe ser un valor l (**regla existente**)</span><span class="sxs-lookup"><span data-stu-id="ff17f-269">An argument of `return ref` must be an LValue (**existing rule**)</span></span>
- <span data-ttu-id="ff17f-270">Un argumento de `return ref` debe ser "Safe to Return" (**regla existente**)</span><span class="sxs-lookup"><span data-stu-id="ff17f-270">An argument of `return ref` must be "safe to return" (**existing rule**)</span></span>
- <span data-ttu-id="ff17f-271">En un miembro de `ref readonly`, _no es necesario que_ el argumento de `return ref` sea grabable.</span><span class="sxs-lookup"><span data-stu-id="ff17f-271">In a `ref readonly` member an argument of `return ref` is _not required to be writeable_ .</span></span>
<span data-ttu-id="ff17f-272">Por ejemplo, este miembro puede devolver una referencia a un campo de solo lectura o a uno de sus parámetros de `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-272">For example such member can ref-return a readonly field or one of its `in` parameters.</span></span>

### <a name="safe-to-return-rules"></a><span data-ttu-id="ff17f-273">Safe para devolver reglas.</span><span class="sxs-lookup"><span data-stu-id="ff17f-273">Safe to Return rules.</span></span>
<span data-ttu-id="ff17f-274">La seguridad normal de las reglas de devoluciones para las referencias se aplicará también a las referencias de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="ff17f-274">Normal safe to return rules for references will apply to readonly references as well.</span></span>

<span data-ttu-id="ff17f-275">Tenga en cuenta que se puede obtener un `ref readonly` a partir de un `ref` normal/parámetro/Return, pero no al revés.</span><span class="sxs-lookup"><span data-stu-id="ff17f-275">Note that a `ref readonly` can be obtained from a regular `ref` local/parameter/return, but not the other way around.</span></span> <span data-ttu-id="ff17f-276">De lo contrario, la seguridad de las devoluciones de `ref readonly` se deduce de la misma manera que para las devoluciones normales de `ref`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-276">Otherwise the safety of `ref readonly` returns is inferred the same way as for regular `ref` returns.</span></span>

<span data-ttu-id="ff17f-277">Teniendo en cuenta que RValues se puede pasar como `in` parámetro y devolverse como `ref readonly` necesitamos una regla más: **RValues no es seguro para devolver por referencia**.</span><span class="sxs-lookup"><span data-stu-id="ff17f-277">Considering that RValues can be passed as `in` parameter and returned as `ref readonly` we need one more rule - **RValues are not safe-to-return by reference**.</span></span>

> <span data-ttu-id="ff17f-278">Considere la situación en la que se pasa un valor r a un parámetro `in` a través de una copia y, a continuación, se devuelve en forma de un `ref readonly`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-278">Consider the situation when an RValue is passed to an `in` parameter via a copy and then returned back in a form of a `ref readonly`.</span></span> <span data-ttu-id="ff17f-279">En el contexto del llamador, el resultado de dicha invocación es una referencia a los datos locales y, como tal, no es seguro devolver.</span><span class="sxs-lookup"><span data-stu-id="ff17f-279">In the context of the caller the result of such invocation is a reference to local data and as such is unsafe to return.</span></span>
> <span data-ttu-id="ff17f-280">Una vez que RValues no se pueden devolver de forma segura, la regla existente `#6` ya controla este caso.</span><span class="sxs-lookup"><span data-stu-id="ff17f-280">Once RValues are not safe to return, the existing rule `#6` already handles this case.</span></span>

<span data-ttu-id="ff17f-281">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ff17f-281">Example:</span></span>
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

<span data-ttu-id="ff17f-282">Reglas de `safe to return` actualizadas:</span><span class="sxs-lookup"><span data-stu-id="ff17f-282">Updated `safe to return` rules:</span></span>

1.  <span data-ttu-id="ff17f-283">**las referencias a variables en el montón se pueden devolver de forma segura**</span><span class="sxs-lookup"><span data-stu-id="ff17f-283">**refs to variables on the heap are safe to return**</span></span>
2.  <span data-ttu-id="ff17f-284">**los parámetros Ref/in son seguros para devolver**
`in` los parámetros de forma natural solo se pueden devolver como ReadOnly.</span><span class="sxs-lookup"><span data-stu-id="ff17f-284">**ref/in parameters are safe to return**
`in` parameters naturally can only be returned as readonly.</span></span>
3.  <span data-ttu-id="ff17f-285">**los parámetros out son seguros para devolver** (pero se deben asignar definitivamente, ya que ya es el caso hoy)</span><span class="sxs-lookup"><span data-stu-id="ff17f-285">**out parameters are safe to return** (but must be definitely assigned, as is already the case today)</span></span>
4.  <span data-ttu-id="ff17f-286">**los campos de struct de instancia se pueden devolver de forma segura siempre que el receptor sea seguro para devolver**</span><span class="sxs-lookup"><span data-stu-id="ff17f-286">**instance struct fields are safe to return as long as the receiver is safe to return**</span></span>
5.  <span data-ttu-id="ff17f-287">**' this ' no es seguro para devolver los miembros de struct**</span><span class="sxs-lookup"><span data-stu-id="ff17f-287">**'this' is not safe to return from struct members**</span></span>
6.  <span data-ttu-id="ff17f-288">**una referencia, devuelta desde otro método, es segura para devolver si todas las referencias que se pasan a ese método como parámetros formales eran seguras de devolver.** 
*específicamente es irrelevante si el receptor es seguro para devolver, independientemente de si el receptor es un struct, una clase o un tipo de parámetro de tipo genérico.*</span><span class="sxs-lookup"><span data-stu-id="ff17f-288">**a ref, returned from another method is safe to return if all refs/outs passed to that method as formal parameters were safe to return.**
*Specifically it is irrelevant if receiver is safe to return, regardless whether receiver is a struct, class or typed as a generic type parameter.*</span></span>
7.  <span data-ttu-id="ff17f-289">**No es seguro devolver RValues por referencia.** 
*específicamente RValues se pueden pasar como parámetros in.*</span><span class="sxs-lookup"><span data-stu-id="ff17f-289">**RValues are not safe to return by reference.**
*Specifically RValues are safe to pass as in parameters.*</span></span>

> <span data-ttu-id="ff17f-290">Nota: existen otras reglas relacionadas con la seguridad de las devoluciones que entran en juego cuando se implican tipos de referencia y reasignaciones de referencia.</span><span class="sxs-lookup"><span data-stu-id="ff17f-290">NOTE: There are additional rules regarding safety of returns that come into play when ref-like types and ref-reassignments are involved.</span></span>
> <span data-ttu-id="ff17f-291">Las reglas se aplican igualmente a los miembros `ref` y `ref readonly` y, por lo tanto, no se mencionan aquí.</span><span class="sxs-lookup"><span data-stu-id="ff17f-291">The rules equally apply to `ref` and `ref readonly` members and therefore are not mentioned here.</span></span>

### <a name="aliasing-behavior"></a><span data-ttu-id="ff17f-292">Comportamiento de alias.</span><span class="sxs-lookup"><span data-stu-id="ff17f-292">Aliasing behavior.</span></span>
<span data-ttu-id="ff17f-293">`ref readonly` miembros proporcionan el mismo comportamiento de alias que los miembros de `ref` ordinarios (excepto para ser de solo lectura).</span><span class="sxs-lookup"><span data-stu-id="ff17f-293">`ref readonly` members provide the same aliasing behavior as ordinary `ref` members (except for being readonly).</span></span>
<span data-ttu-id="ff17f-294">Por lo tanto, para la captura en lambdas, Async, iteradores, desbordamiento de pila, etc. se aplican las mismas restricciones.</span><span class="sxs-lookup"><span data-stu-id="ff17f-294">Therefore for the purpose of capturing in lambdas, async, iterators, stack spilling etc... the same restrictions apply.</span></span> <span data-ttu-id="ff17f-295">es decir,.</span><span class="sxs-lookup"><span data-stu-id="ff17f-295">- I.E.</span></span> <span data-ttu-id="ff17f-296">debido a la incapacidad de capturar las referencias reales y debido a la naturaleza de efectos secundarios de la evaluación de miembros, tales escenarios no se permiten.</span><span class="sxs-lookup"><span data-stu-id="ff17f-296">due to inability to capture the actual references and due to side-effecting nature of member evaluation such scenarios are disallowed.</span></span>

> <span data-ttu-id="ff17f-297">Se permite y es necesario realizar una copia cuando `ref readonly` Return es un receptor de métodos struct normales, que toman `this` como una referencia ordinaria que se puede escribir.</span><span class="sxs-lookup"><span data-stu-id="ff17f-297">It is permitted and required to make a copy when `ref readonly` return is a receiver of regular struct methods, which take `this` as an ordinary writeable reference.</span></span> <span data-ttu-id="ff17f-298">Históricamente, en todos los casos en los que estas invocaciones se aplican a la variable de solo lectura, se realiza una copia local.</span><span class="sxs-lookup"><span data-stu-id="ff17f-298">Historically in all cases where such invocations are applied to readonly variable a local copy is made.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="ff17f-299">Representación de metadatos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-299">Metadata representation.</span></span>
<span data-ttu-id="ff17f-300">Cuando se aplica `System.Runtime.CompilerServices.IsReadOnlyAttribute` a la devolución de un método que devuelve ByRef, significa que el método devuelve una referencia de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="ff17f-300">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to the return of a byref returning method, it means that the method returns a readonly reference.</span></span>

<span data-ttu-id="ff17f-301">Además, la firma de resultados de estos métodos (y solo esos métodos) debe tener `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-301">In addition, the result signature of such methods (and only those methods) must have `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`.</span></span>

<span data-ttu-id="ff17f-302">**Motivación**: esto es para asegurarse de que los compiladores existentes no pueden simplemente omitir `readonly` al invocar métodos con `ref readonly` devuelve.</span><span class="sxs-lookup"><span data-stu-id="ff17f-302">**Motivation**: this is to ensure that existing compilers cannot simply ignore `readonly` when invoking methods with `ref readonly` returns</span></span>

## <a name="readonly-structs"></a><span data-ttu-id="ff17f-303">Structs de solo lectura</span><span class="sxs-lookup"><span data-stu-id="ff17f-303">Readonly structs</span></span>
<span data-ttu-id="ff17f-304">En Resumen, es una característica que hace `this` parámetro de todos los miembros de instancia de un struct, excepto para los constructores, un parámetro `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-304">In short - a feature that makes `this` parameter of all instance members of a struct, except for constructors, an `in` parameter.</span></span>

### <a name="motivation"></a><span data-ttu-id="ff17f-305">Motivación</span><span class="sxs-lookup"><span data-stu-id="ff17f-305">Motivation</span></span>
<span data-ttu-id="ff17f-306">El compilador debe asumir que cualquier llamada al método en una instancia de struct puede modificar la instancia.</span><span class="sxs-lookup"><span data-stu-id="ff17f-306">Compiler must assume that any method call on a struct instance may modify the instance.</span></span> <span data-ttu-id="ff17f-307">En realidad, se pasa una referencia grabable al método como `this` parámetro y habilita completamente este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="ff17f-307">Indeed a writeable reference is passed to the method as `this` parameter and fully enables this behavior.</span></span> <span data-ttu-id="ff17f-308">Para permitir tales invocaciones en `readonly` variables, las invocaciones se aplican a las copias temporales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-308">To allow such invocations on `readonly` variables, the invocations are applied to temp copies.</span></span> <span data-ttu-id="ff17f-309">Esto podría ser poco intuitivo y a veces obliga a los usuarios a abandonar `readonly` por motivos de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="ff17f-309">That could be unintuitive and sometimes forces people to abandon `readonly` for performance reasons.</span></span>  
<span data-ttu-id="ff17f-310">Ejemplo: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span><span class="sxs-lookup"><span data-stu-id="ff17f-310">Example: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/</span></span>

<span data-ttu-id="ff17f-311">Después de agregar compatibilidad para `in` parámetros y `ref readonly` devuelve el problema de la copia defensiva se verá peor, ya que las variables ReadOnly serán más comunes.</span><span class="sxs-lookup"><span data-stu-id="ff17f-311">After adding support for `in` parameters and `ref readonly` returns the problem of defensive copying will get worse since readonly variables will become more common.</span></span>

### <a name="solution"></a><span data-ttu-id="ff17f-312">Solución</span><span class="sxs-lookup"><span data-stu-id="ff17f-312">Solution</span></span>
<span data-ttu-id="ff17f-313">Permita `readonly` modificador en las declaraciones de struct, lo que daría lugar a `this` tratarse como un parámetro de `in` en todos los métodos de instancia de estructura, salvo para los constructores.</span><span class="sxs-lookup"><span data-stu-id="ff17f-313">Allow `readonly` modifier on struct declarations which would result in `this` being treated as `in` parameter on all struct instance methods except for constructors.</span></span>

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a><span data-ttu-id="ff17f-314">Restricciones en miembros de struct de solo lectura</span><span class="sxs-lookup"><span data-stu-id="ff17f-314">Restrictions on members of readonly struct</span></span>
- <span data-ttu-id="ff17f-315">Los campos de instancia de un struct de solo lectura deben ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="ff17f-315">Instance fields of a readonly struct must be readonly.</span></span>  
<span data-ttu-id="ff17f-316">**Motivación:** solo se puede escribir externamente, pero no a través de miembros.</span><span class="sxs-lookup"><span data-stu-id="ff17f-316">**Motivation:** can only be written to externally, but not through members.</span></span>
- <span data-ttu-id="ff17f-317">Las autopropiedades de instancia de un struct de solo lectura deben ser Get-only.</span><span class="sxs-lookup"><span data-stu-id="ff17f-317">Instance autoproperties of a readonly struct must be get-only.</span></span>  
<span data-ttu-id="ff17f-318">**Motivación:** consecuencia de la restricción en los campos de instancia.</span><span class="sxs-lookup"><span data-stu-id="ff17f-318">**Motivation:** consequence of restriction on instance fields.</span></span>
- <span data-ttu-id="ff17f-319">La estructura ReadOnly no puede declarar eventos similares a campos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-319">Readonly struct may not declare field-like events.</span></span>  
<span data-ttu-id="ff17f-320">**Motivación:** consecuencia de la restricción en los campos de instancia.</span><span class="sxs-lookup"><span data-stu-id="ff17f-320">**Motivation:** consequence of restriction on instance fields.</span></span>

### <a name="metadata-representation"></a><span data-ttu-id="ff17f-321">Representación de metadatos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-321">Metadata representation.</span></span>
<span data-ttu-id="ff17f-322">Cuando `System.Runtime.CompilerServices.IsReadOnlyAttribute` se aplica a un tipo de valor, significa que el tipo es un `readonly struct`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-322">When `System.Runtime.CompilerServices.IsReadOnlyAttribute` is applied to a value type, it means that the type is a `readonly struct`.</span></span>

<span data-ttu-id="ff17f-323">En concreto:</span><span class="sxs-lookup"><span data-stu-id="ff17f-323">In particular:</span></span>
-  <span data-ttu-id="ff17f-324">La identidad del tipo `IsReadOnlyAttribute` no es importante.</span><span class="sxs-lookup"><span data-stu-id="ff17f-324">The identity of the `IsReadOnlyAttribute` type is unimportant.</span></span> <span data-ttu-id="ff17f-325">De hecho, el compilador puede incrustarlo en el ensamblado contenedor si es necesario.</span><span class="sxs-lookup"><span data-stu-id="ff17f-325">In fact it can be embedded by the compiler in the containing assembly if needed.</span></span>

## <a name="refin-extension-methods"></a><span data-ttu-id="ff17f-326">`ref`/métodos de extensión `in`</span><span class="sxs-lookup"><span data-stu-id="ff17f-326">`ref`/`in` extension methods</span></span>
<span data-ttu-id="ff17f-327">En realidad, existe una propuesta (https://github.com/dotnet/roslyn/issues/165) y la solicitud de incorporación de prototipo correspondiente (https://github.com/dotnet/roslyn/pull/15650).</span><span class="sxs-lookup"><span data-stu-id="ff17f-327">There is actually an existing proposal (https://github.com/dotnet/roslyn/issues/165) and corresponding prototype PR (https://github.com/dotnet/roslyn/pull/15650).</span></span>
<span data-ttu-id="ff17f-328">Simplemente deseo confirmar que esta idea no es totalmente nueva.</span><span class="sxs-lookup"><span data-stu-id="ff17f-328">I just want to acknowledge that this idea is not entirely new.</span></span> <span data-ttu-id="ff17f-329">Sin embargo, es relevante aquí, ya que `ref readonly` quita de forma elegante el problema más polémico sobre tales métodos: Qué hacer con los receptores de valor r.</span><span class="sxs-lookup"><span data-stu-id="ff17f-329">It is, however, relevant here since `ref readonly` elegantly removes the most contentious issue about such methods - what to do with RValue receivers.</span></span>

<span data-ttu-id="ff17f-330">La idea general es permitir que los métodos de extensión tomen el parámetro `this` por referencia, siempre que se sepa que el tipo es un tipo de estructura.</span><span class="sxs-lookup"><span data-stu-id="ff17f-330">The general idea is allowing extension methods to take the `this` parameter by reference, as long as the type is known to be a struct type.</span></span>

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

<span data-ttu-id="ff17f-331">Los motivos para escribir estos métodos de extensión son principalmente:</span><span class="sxs-lookup"><span data-stu-id="ff17f-331">The reasons for writing such extension methods are primarily:</span></span>  
1.  <span data-ttu-id="ff17f-332">Evitar la copia cuando el receptor es un struct grande</span><span class="sxs-lookup"><span data-stu-id="ff17f-332">Avoid copying when receiver is a large struct</span></span>
2.  <span data-ttu-id="ff17f-333">Permitir métodos de extensión mutables en Structs</span><span class="sxs-lookup"><span data-stu-id="ff17f-333">Allow mutating extension methods on structs</span></span>

<span data-ttu-id="ff17f-334">Los motivos por los que no queremos permitir esto en las clases</span><span class="sxs-lookup"><span data-stu-id="ff17f-334">The reasons why we do not want to allow this on classes</span></span>  
1.  <span data-ttu-id="ff17f-335">Tendría una finalidad muy limitada.</span><span class="sxs-lookup"><span data-stu-id="ff17f-335">It would be of very limited purpose.</span></span>
2.  <span data-ttu-id="ff17f-336">Interrumpiría la invariante de larga duración que una llamada al método no puede convertir un receptor que no sea de`null` para que se `null` después de la invocación.</span><span class="sxs-lookup"><span data-stu-id="ff17f-336">It would break long standing invariant that a method call cannot turn non-`null` receiver to become `null` after invocation.</span></span>
> <span data-ttu-id="ff17f-337">De hecho, actualmente no se puede convertir en una variable que no sea de`null` `null` a menos que `ref` o `out`no la asigne o pase _explícitamente_ .</span><span class="sxs-lookup"><span data-stu-id="ff17f-337">In fact, currently a non-`null` variable cannot become `null` unless _explicitly_ assigned or passed by `ref` or `out`.</span></span>
> <span data-ttu-id="ff17f-338">Esto ayuda en gran medida a la legibilidad u otras formas de análisis "puede ser un valor nulo aquí".</span><span class="sxs-lookup"><span data-stu-id="ff17f-338">That greatly aids readability or other forms of "can this be a null here" analysis.</span></span>
3.  <span data-ttu-id="ff17f-339">Sería difícil conciliar con la semántica "evaluar una vez" de los accesos condicionales null.</span><span class="sxs-lookup"><span data-stu-id="ff17f-339">It would be hard to reconcile with "evaluate once" semantics of null-conditional accesses.</span></span>
<span data-ttu-id="ff17f-340">Ejemplo: `obj.stringField?.RefExtension(...)`-necesita capturar una copia de `stringField` para que la comprobación de valores NULL sea significativa, pero las asignaciones a `this` dentro de RefExtension no se reflejarán de nuevo en el campo.</span><span class="sxs-lookup"><span data-stu-id="ff17f-340">Example: `obj.stringField?.RefExtension(...)` - need to capture a copy of `stringField` to make the null check meaningful, but then assignments to `this` inside RefExtension would not be reflected back to the field.</span></span>

<span data-ttu-id="ff17f-341">Una capacidad para declarar métodos de extensión en **Structs** que toman el primer argumento por referencia era una solicitud de larga duración.</span><span class="sxs-lookup"><span data-stu-id="ff17f-341">An ability to declare extension methods on **structs** that take the first argument by reference was a long-standing request.</span></span> <span data-ttu-id="ff17f-342">Una de las consideraciones de bloqueo fue "¿Qué ocurre si el receptor no es un valor l?".</span><span class="sxs-lookup"><span data-stu-id="ff17f-342">One of the blocking consideration was "what happens if receiver is not an LValue?".</span></span>

- <span data-ttu-id="ff17f-343">Existe un precedente que también se podría llamar a cualquier método de extensión como método estático (a veces es la única manera de resolver la ambigüedad).</span><span class="sxs-lookup"><span data-stu-id="ff17f-343">There is a precedent that any extension method could also be called as a static method (sometimes it is the only way to resolve ambiguity).</span></span> <span data-ttu-id="ff17f-344">Esto indicaría que no se permiten los receptores de valor r.</span><span class="sxs-lookup"><span data-stu-id="ff17f-344">It would dictate that RValue receivers should be disallowed.</span></span>
- <span data-ttu-id="ff17f-345">Por otro lado, existe la posibilidad de efectuar la invocación en una copia en situaciones similares cuando intervienen los métodos de instancia de struct.</span><span class="sxs-lookup"><span data-stu-id="ff17f-345">On the other hand there is a practice of making invocation on a copy in similar situations when struct instance methods are involved.</span></span>

<span data-ttu-id="ff17f-346">La razón por la que existe la "copia implícita" es que la mayoría de los métodos struct no modifican realmente el struct mientras no es capaz de indicarlo.</span><span class="sxs-lookup"><span data-stu-id="ff17f-346">The reason why the "implicit copying" exists is because the majority of struct methods do not actually modify the struct while not being able to indicate that.</span></span> <span data-ttu-id="ff17f-347">Por lo tanto, la solución más práctica era simplemente hacer la invocación en una copia, pero esta práctica se conoce para perjudicar el rendimiento y provocar errores.</span><span class="sxs-lookup"><span data-stu-id="ff17f-347">Therefore the most practical solution was to just make the invocation on a copy, but this practice is known for harming performance and causing bugs.</span></span>

<span data-ttu-id="ff17f-348">Ahora, con la disponibilidad de `in` parámetros, es posible que una extensión señale el intento.</span><span class="sxs-lookup"><span data-stu-id="ff17f-348">Now, with availability of `in` parameters, it is possible for an extension to signal the intent.</span></span> <span data-ttu-id="ff17f-349">Por lo tanto, el dilema se puede resolver mediante la exigencia de que las extensiones de `ref` se llamen con receptores grabables, mientras que las extensiones `in` permiten la copia implícita si es necesario.</span><span class="sxs-lookup"><span data-stu-id="ff17f-349">Therefore the conundrum can be resolved by requiring `ref` extensions to be called with writeable receivers while `in` extensions permit implicit copying if necessary.</span></span>

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a><span data-ttu-id="ff17f-350">`in` extensiones y genéricos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-350">`in` extensions and generics.</span></span>
<span data-ttu-id="ff17f-351">El propósito de `ref` métodos de extensión es mutar el receptor directamente o invocar a los miembros mutantes.</span><span class="sxs-lookup"><span data-stu-id="ff17f-351">The purpose of `ref` extension methods is to mutate the receiver directly or by invoking mutating members.</span></span> <span data-ttu-id="ff17f-352">Por lo tanto, se permiten las extensiones `ref this T` siempre que `T` esté restringido a un struct.</span><span class="sxs-lookup"><span data-stu-id="ff17f-352">Therefore `ref this T` extensions are allowed as long as `T` is constrained to be a struct.</span></span>

<span data-ttu-id="ff17f-353">Por otro lado `in` métodos de extensión existen específicamente para reducir la copia implícita.</span><span class="sxs-lookup"><span data-stu-id="ff17f-353">On the other hand `in` extension methods exist specifically to reduce implicit copying.</span></span> <span data-ttu-id="ff17f-354">Sin embargo, cualquier uso de un parámetro `in T` tendrá que realizarse a través de un miembro de interfaz.</span><span class="sxs-lookup"><span data-stu-id="ff17f-354">However any use of an `in T` parameter will have to be done through an interface member.</span></span> <span data-ttu-id="ff17f-355">Dado que todos los miembros de interfaz se consideran mutables, cualquier uso de este tipo requeriría una copia.</span><span class="sxs-lookup"><span data-stu-id="ff17f-355">Since all interface members are considered mutating, any such use would require a copy.</span></span> <span data-ttu-id="ff17f-356">-En lugar de reducir la copia, el efecto sería el contrario.</span><span class="sxs-lookup"><span data-stu-id="ff17f-356">- Instead of reducing copying, the effect would be the opposite.</span></span> <span data-ttu-id="ff17f-357">Por lo tanto, no se permite `in this T` cuando `T` es un parámetro de tipo genérico, independientemente de las restricciones.</span><span class="sxs-lookup"><span data-stu-id="ff17f-357">Therefore `in this T` is not allowed when `T` is a generic type parameter regardless of constraints.</span></span>

### <a name="valid-kinds-of-extension-methods-recap"></a><span data-ttu-id="ff17f-358">Tipos válidos de métodos de extensión (Resumen):</span><span class="sxs-lookup"><span data-stu-id="ff17f-358">Valid kinds of extension methods (recap):</span></span>
<span data-ttu-id="ff17f-359">Ahora se permiten las siguientes formas de declaración de `this` en un método de extensión:</span><span class="sxs-lookup"><span data-stu-id="ff17f-359">The following forms of `this` declaration in an extension method are now allowed:</span></span>
1) <span data-ttu-id="ff17f-360">`this T arg`: extensión ByVal normal.</span><span class="sxs-lookup"><span data-stu-id="ff17f-360">`this T arg` - regular byval extension.</span></span> <span data-ttu-id="ff17f-361">(**caso existente**)</span><span class="sxs-lookup"><span data-stu-id="ff17f-361">(**existing case**)</span></span>
- <span data-ttu-id="ff17f-362">T puede ser cualquier tipo, incluidos los tipos de referencia o los parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="ff17f-362">T can be any type, including reference types or type parameters.</span></span>
<span data-ttu-id="ff17f-363">La instancia será la misma variable después de la llamada.</span><span class="sxs-lookup"><span data-stu-id="ff17f-363">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="ff17f-364">Permite conversiones implícitas de este tipo de _conversión de argumentos_ .</span><span class="sxs-lookup"><span data-stu-id="ff17f-364">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="ff17f-365">Se puede llamar a en RValues.</span><span class="sxs-lookup"><span data-stu-id="ff17f-365">Can be called on RValues.</span></span>

- <span data-ttu-id="ff17f-366">`in this T self`extensión de `in` de  - .</span><span class="sxs-lookup"><span data-stu-id="ff17f-366">`in this T self` - `in` extension.</span></span>
<span data-ttu-id="ff17f-367">T debe ser un tipo de struct real.</span><span class="sxs-lookup"><span data-stu-id="ff17f-367">T must be an actual struct type.</span></span>
<span data-ttu-id="ff17f-368">La instancia será la misma variable después de la llamada.</span><span class="sxs-lookup"><span data-stu-id="ff17f-368">Instance will be the same variable after the call.</span></span>
<span data-ttu-id="ff17f-369">Permite conversiones implícitas de este tipo de _conversión de argumentos_ .</span><span class="sxs-lookup"><span data-stu-id="ff17f-369">Allows implicit conversions of _this-argument-conversion_ kind.</span></span>
<span data-ttu-id="ff17f-370">Se puede llamar a en RValues (se puede invocar en un Temp si es necesario).</span><span class="sxs-lookup"><span data-stu-id="ff17f-370">Can be called on RValues (may be invoked on a temp if needed).</span></span>

- <span data-ttu-id="ff17f-371">`ref this T self`extensión de `ref` de  - .</span><span class="sxs-lookup"><span data-stu-id="ff17f-371">`ref this T self` - `ref` extension.</span></span>
<span data-ttu-id="ff17f-372">T debe ser un tipo de estructura o un parámetro de tipo genérico restringido para ser un struct.</span><span class="sxs-lookup"><span data-stu-id="ff17f-372">T must be a struct type or a generic type parameter constrained to be a struct.</span></span>
<span data-ttu-id="ff17f-373">La invocación puede escribir en la instancia.</span><span class="sxs-lookup"><span data-stu-id="ff17f-373">Instance may be written to by the invocation.</span></span>
<span data-ttu-id="ff17f-374">Solo permite conversiones de identidad.</span><span class="sxs-lookup"><span data-stu-id="ff17f-374">Allows only identity conversions.</span></span>
<span data-ttu-id="ff17f-375">Se debe llamar al método LValue grabable.</span><span class="sxs-lookup"><span data-stu-id="ff17f-375">Must be called on writeable LValue.</span></span> <span data-ttu-id="ff17f-376">(nunca se invoca a través de un Temp).</span><span class="sxs-lookup"><span data-stu-id="ff17f-376">(never invoked via a temp).</span></span>

## <a name="readonly-ref-locals"></a><span data-ttu-id="ff17f-377">Variables locales de referencia de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="ff17f-377">Readonly ref locals.</span></span>

### <a name="motivation"></a><span data-ttu-id="ff17f-378">Razones.</span><span class="sxs-lookup"><span data-stu-id="ff17f-378">Motivation.</span></span>
<span data-ttu-id="ff17f-379">Una vez que se introdujeron `ref readonly` miembros, era evidente que se debían emparejar con el tipo de local adecuado.</span><span class="sxs-lookup"><span data-stu-id="ff17f-379">Once `ref readonly` members were introduced, it was clear from the use that they need to be paired with appropriate kind of local.</span></span> <span data-ttu-id="ff17f-380">La evaluación de un miembro puede producir o observar efectos secundarios, por lo tanto, si el resultado se debe usar más de una vez, debe almacenarse.</span><span class="sxs-lookup"><span data-stu-id="ff17f-380">Evaluation of a member may produce or observe side effects, therefore if the result must be used more than once, it needs to be stored.</span></span> <span data-ttu-id="ff17f-381">Las variables locales de `ref` normales no ayudan aquí, ya que no se les puede asignar una referencia `readonly`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-381">Ordinary `ref` locals do not help here since they cannot be assigned a `readonly` reference.</span></span>   

### <a name="solution"></a><span data-ttu-id="ff17f-382">Solución.</span><span class="sxs-lookup"><span data-stu-id="ff17f-382">Solution.</span></span>
<span data-ttu-id="ff17f-383">Permitir declarar `ref readonly` variables locales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-383">Allow declaring `ref readonly` locals.</span></span> <span data-ttu-id="ff17f-384">Se trata de un nuevo tipo de `ref` variables locales que no se van a escribir.</span><span class="sxs-lookup"><span data-stu-id="ff17f-384">This is a new kind of `ref` locals that is not writeable.</span></span> <span data-ttu-id="ff17f-385">Como resultado `ref readonly` las variables locales pueden aceptar referencias a variables de solo lectura sin exponer estas variables a Escrituras.</span><span class="sxs-lookup"><span data-stu-id="ff17f-385">As a result `ref readonly` locals can accept references to readonly variables without exposing these variables to writes.</span></span>

### <a name="declaring-and-using-ref-readonly-locals"></a><span data-ttu-id="ff17f-386">Declarar y usar `ref readonly` variables locales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-386">Declaring and using `ref readonly` locals.</span></span>

<span data-ttu-id="ff17f-387">La sintaxis de tales variables locales usa modificadores `ref readonly` en el sitio de la declaración (en ese orden específico).</span><span class="sxs-lookup"><span data-stu-id="ff17f-387">The syntax of such locals uses `ref readonly` modifiers at declaration site (in that specific order).</span></span> <span data-ttu-id="ff17f-388">De forma similar a las variables locales de `ref` normales, las variables locales de `ref readonly` se deben inicializar con referencia a la declaración.</span><span class="sxs-lookup"><span data-stu-id="ff17f-388">Similarly to ordinary `ref` locals, `ref readonly` locals must be ref-initialized at declaration.</span></span> <span data-ttu-id="ff17f-389">A diferencia de las variables locales de `ref` estándar, las variables locales de `ref readonly` pueden hacer referencia a `readonly` LValues como `in` parámetros, `readonly` campos `ref readonly` métodos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-389">Unlike regular `ref` locals, `ref readonly` locals can refer to `readonly` LValues like `in` parameters, `readonly` fields, `ref readonly` methods.</span></span>

<span data-ttu-id="ff17f-390">Para todos los propósitos, una `ref readonly` local se trata como una variable `readonly`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-390">For all purposes a `ref readonly` local is treated as a `readonly` variable.</span></span> <span data-ttu-id="ff17f-391">La mayoría de las restricciones en el uso son las mismas que con `readonly` campos o parámetros de `in`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-391">Most of the restrictions on the use are the same as with `readonly` fields or `in` parameters.</span></span>

<span data-ttu-id="ff17f-392">Por ejemplo, los campos de un parámetro `in` que tiene un tipo de estructura se clasifican de forma recursiva como variables de `readonly`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-392">For example fields of an `in` parameter which has a struct type are all recursively classified as `readonly` variables .</span></span>   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a><span data-ttu-id="ff17f-393">Restricciones en el uso de `ref readonly` variables locales</span><span class="sxs-lookup"><span data-stu-id="ff17f-393">Restrictions on use of `ref readonly` locals</span></span>
<span data-ttu-id="ff17f-394">Salvo por su naturaleza `readonly`, `ref readonly` variables locales se comportan como `ref` variables locales normales y están sujetas a las mismas restricciones.</span><span class="sxs-lookup"><span data-stu-id="ff17f-394">Except for their `readonly` nature, `ref readonly` locals behave like ordinary `ref` locals and are subject to exactly same restrictions.</span></span>  
<span data-ttu-id="ff17f-395">Por ejemplo, las restricciones relacionadas con la captura en cierres, la declaración en `async` métodos o el análisis de `safe-to-return` se aplican igualmente a `ref readonly` variables locales.</span><span class="sxs-lookup"><span data-stu-id="ff17f-395">For example restrictions related to capturing in closures, declaring in `async` methods or the `safe-to-return` analysis equally applies to `ref readonly` locals.</span></span>

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a><span data-ttu-id="ff17f-396">Expresiones de `ref` ternarios.</span><span class="sxs-lookup"><span data-stu-id="ff17f-396">Ternary `ref` expressions.</span></span> <span data-ttu-id="ff17f-397">(también conocido como "LValues condicional")</span><span class="sxs-lookup"><span data-stu-id="ff17f-397">(aka "Conditional LValues")</span></span>

### <a name="motivation"></a><span data-ttu-id="ff17f-398">Motivación</span><span class="sxs-lookup"><span data-stu-id="ff17f-398">Motivation</span></span>
<span data-ttu-id="ff17f-399">El uso de `ref` y `ref readonly` variables locales expone una necesidad de inicializar las variables locales con una u otra variable de destino basada en una condición.</span><span class="sxs-lookup"><span data-stu-id="ff17f-399">Use of `ref` and `ref readonly` locals exposed a need to ref-initialize such locals with one or another target variable based on a condition.</span></span>

<span data-ttu-id="ff17f-400">Una solución habitual es introducir un método como:</span><span class="sxs-lookup"><span data-stu-id="ff17f-400">A typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="ff17f-401">Tenga en cuenta que `Choice` no es un reemplazo exacto de un ternario, ya que _todos los_ argumentos deben evaluarse en el sitio de llamada, lo que ha provocado errores y comportamientos inintuitivos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-401">Note that `Choice` is not an exact replacement of a ternary since _all_ arguments must be evaluated at the call site, which was leading to unintuitive behavior and bugs.</span></span>

<span data-ttu-id="ff17f-402">Lo siguiente no funcionará según lo esperado:</span><span class="sxs-lookup"><span data-stu-id="ff17f-402">The following will not work as expected:</span></span>

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a><span data-ttu-id="ff17f-403">Solución</span><span class="sxs-lookup"><span data-stu-id="ff17f-403">Solution</span></span>
<span data-ttu-id="ff17f-404">Permite un tipo especial de expresión condicional que se evalúa como una referencia a uno de los argumentos LValue basados en una condición.</span><span class="sxs-lookup"><span data-stu-id="ff17f-404">Allow special kind of conditional expression that evaluates to a reference to one of LValue argument based on a condition.</span></span>

### <a name="using-ref-ternary-expression"></a><span data-ttu-id="ff17f-405">Usar `ref` expresión ternaria.</span><span class="sxs-lookup"><span data-stu-id="ff17f-405">Using `ref` ternary expression.</span></span>

<span data-ttu-id="ff17f-406">La sintaxis del tipo de `ref` de una expresión condicional es `<condition> ? ref <consequence> : ref <alternative>;`</span><span class="sxs-lookup"><span data-stu-id="ff17f-406">The syntax for the `ref` flavor of a conditional expression is `<condition> ? ref <consequence> : ref <alternative>;`</span></span>

<span data-ttu-id="ff17f-407">Al igual que con la expresión condicional ordinaria, solo `<consequence>` o `<alternative>` se evalúa según el resultado de la expresión de condición booleana.</span><span class="sxs-lookup"><span data-stu-id="ff17f-407">Just like with the ordinary conditional expression only `<consequence>` or `<alternative>` is evaluated depending on result of the boolean condition expression.</span></span>

<span data-ttu-id="ff17f-408">A diferencia de la expresión condicional ordinaria, `ref` expresión condicional:</span><span class="sxs-lookup"><span data-stu-id="ff17f-408">Unlike ordinary conditional expression, `ref` conditional expression:</span></span>
- <span data-ttu-id="ff17f-409">requiere que `<consequence>` y `<alternative>` sean LValues.</span><span class="sxs-lookup"><span data-stu-id="ff17f-409">requires that `<consequence>` and `<alternative>` are LValues.</span></span>
- <span data-ttu-id="ff17f-410">`ref` expresión condicional es un valor l y</span><span class="sxs-lookup"><span data-stu-id="ff17f-410">`ref` conditional expression itself is an LValue and</span></span>
- <span data-ttu-id="ff17f-411">`ref` expresión condicional es grabable si tanto `<consequence>` como `<alternative>` son grabables LValues</span><span class="sxs-lookup"><span data-stu-id="ff17f-411">`ref` conditional expression is writeable if both `<consequence>` and `<alternative>` are writeable LValues</span></span>

<span data-ttu-id="ff17f-412">Ejemplos:</span><span class="sxs-lookup"><span data-stu-id="ff17f-412">Examples:</span></span>  
<span data-ttu-id="ff17f-413">`ref` ternario es un valor l y, como tal, se puede pasar, asignar o devolver por referencia;</span><span class="sxs-lookup"><span data-stu-id="ff17f-413">`ref` ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="ff17f-414">Ser un valor l; también se puede asignar a.</span><span class="sxs-lookup"><span data-stu-id="ff17f-414">Being an LValue, it can also be assigned to.</span></span>
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

<span data-ttu-id="ff17f-415">Se puede usar como receptor de una llamada al método y omitir la copia si es necesario.</span><span class="sxs-lookup"><span data-stu-id="ff17f-415">Can be used as a receiver of a method call and skip copying if necessary.</span></span>
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

<span data-ttu-id="ff17f-416">`ref` ternario se puede usar también en un contexto normal (no Ref).</span><span class="sxs-lookup"><span data-stu-id="ff17f-416">`ref` ternary can be used in a regular (not ref) context as well.</span></span>
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a><span data-ttu-id="ff17f-417">Desventajas</span><span class="sxs-lookup"><span data-stu-id="ff17f-417">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="ff17f-418">Puedo ver dos argumentos principales contra la compatibilidad mejorada para referencias y referencias de solo lectura:</span><span class="sxs-lookup"><span data-stu-id="ff17f-418">I can see two major arguments against enhanced support for references and readonly references:</span></span>

1) <span data-ttu-id="ff17f-419">Los problemas que se solucionan aquí son muy antiguos.</span><span class="sxs-lookup"><span data-stu-id="ff17f-419">The problems that are solved here are very old.</span></span> <span data-ttu-id="ff17f-420">¿Por qué solucionarlos repentinamente ahora, especialmente porque no ayudaría el código existente?</span><span class="sxs-lookup"><span data-stu-id="ff17f-420">Why suddenly solve them now, especially since it would not help existing code?</span></span>

<span data-ttu-id="ff17f-421">Como encontramos C# y .net se usan en los dominios nuevos, algunos problemas están más destacados.</span><span class="sxs-lookup"><span data-stu-id="ff17f-421">As we find C# and .Net used in new domains, some problems become more prominent.</span></span>  
<span data-ttu-id="ff17f-422">Como ejemplos de entornos que son más críticos que el promedio de las sobrecargas de cálculo, puedo Mostrar</span><span class="sxs-lookup"><span data-stu-id="ff17f-422">As examples of environments that are more critical than average about computation overheads, I can list</span></span>

* <span data-ttu-id="ff17f-423">escenarios de Cloud/Datacenter en los que se factura el cálculo y la capacidad de respuesta es una ventaja competitiva.</span><span class="sxs-lookup"><span data-stu-id="ff17f-423">cloud/datacenter scenarios where computation is billed for and responsiveness is a competitive advantage.</span></span>
* <span data-ttu-id="ff17f-424">Juegos/VR/AR con requisitos de tiempo real en latencia</span><span class="sxs-lookup"><span data-stu-id="ff17f-424">Games/VR/AR with soft-realtime requirements on latencies</span></span>     

<span data-ttu-id="ff17f-425">Esta característica no sacrifica ninguno de los puntos fuertes existentes, como la seguridad de tipos, a la vez que permite reducir las sobrecargas en algunos escenarios comunes.</span><span class="sxs-lookup"><span data-stu-id="ff17f-425">This feature does not sacrifice any of the existing strengths such as type-safety, while allowing to lower overheads in some common scenarios.</span></span>

2) <span data-ttu-id="ff17f-426">¿Podemos garantizar razonablemente que el destinatario de la llamada reproducirá las reglas cuando opte por `readonly` contratos?</span><span class="sxs-lookup"><span data-stu-id="ff17f-426">Can we reasonably guarantee that the callee will play by the rules when it opts into `readonly` contracts?</span></span>

<span data-ttu-id="ff17f-427">Tenemos confianza similar cuando se usa `out`.</span><span class="sxs-lookup"><span data-stu-id="ff17f-427">We have similar trust when using `out`.</span></span> <span data-ttu-id="ff17f-428">Una implementación incorrecta de `out` puede provocar un comportamiento no especificado, pero en realidad apenas sucede.</span><span class="sxs-lookup"><span data-stu-id="ff17f-428">Incorrect implementation of `out` can cause unspecified behavior, but in reality it rarely happens.</span></span>  

<span data-ttu-id="ff17f-429">El hecho de que las reglas de comprobación formal estén familiarizadas con `ref readonly` mitigaría aún más el problema de confianza.</span><span class="sxs-lookup"><span data-stu-id="ff17f-429">Making the formal verification rules familiar with `ref readonly` would further mitigate the trust issue.</span></span>

### <a name="alternatives"></a><span data-ttu-id="ff17f-430">Alternativas</span><span class="sxs-lookup"><span data-stu-id="ff17f-430">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="ff17f-431">En realidad, el diseño de la competencia principal es "no hacer nada".</span><span class="sxs-lookup"><span data-stu-id="ff17f-431">The main competing design is really "do nothing".</span></span>

### <a name="unresolved-questions"></a><span data-ttu-id="ff17f-432">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="ff17f-432">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a><span data-ttu-id="ff17f-433">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="ff17f-433">Design meetings</span></span>

<span data-ttu-id="ff17f-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span><span class="sxs-lookup"><span data-stu-id="ff17f-434">https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md</span></span>
