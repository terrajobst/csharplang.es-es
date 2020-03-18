---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483491"
---
# <a name="suppress-emitting-of-localsinit-flag"></a><span data-ttu-id="69a48-101">Suprimir la emisión de `localsinit` marca.</span><span class="sxs-lookup"><span data-stu-id="69a48-101">Suppress emitting of `localsinit` flag.</span></span>

* <span data-ttu-id="69a48-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="69a48-102">[x] Proposed</span></span>
* <span data-ttu-id="69a48-103">[] Prototipo: no iniciado</span><span class="sxs-lookup"><span data-stu-id="69a48-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="69a48-104">[] Implementación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="69a48-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="69a48-105">[] Especificación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="69a48-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="69a48-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="69a48-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="69a48-107">Permite suprimir la emisión de `localsinit` marca mediante `SkipLocalsInitAttribute` atributo.</span><span class="sxs-lookup"><span data-stu-id="69a48-107">Allow suppressing emit of `localsinit` flag via `SkipLocalsInitAttribute` attribute.</span></span> 

## <a name="motivation"></a><span data-ttu-id="69a48-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="69a48-108">Motivation</span></span>
[motivation]: #motivation


### <a name="background"></a><span data-ttu-id="69a48-109">Información previa</span><span class="sxs-lookup"><span data-stu-id="69a48-109">Background</span></span>
<span data-ttu-id="69a48-110">Por las variables locales de especificación de CLR que no contienen referencias no se inicializan en un valor determinado por la máquina virtual o JIT.</span><span class="sxs-lookup"><span data-stu-id="69a48-110">Per CLR spec local variables that do not contain references are not initialized to a particular value by the VM/JIT.</span></span> <span data-ttu-id="69a48-111">La lectura de estas variables sin inicialización tiene seguridad de tipos, pero, de lo contrario, el comportamiento es indefinido y específico de la implementación.</span><span class="sxs-lookup"><span data-stu-id="69a48-111">Reading from such variables without initialization is type-safe, but otherwise the behavior is undefined and implementation specific.</span></span> <span data-ttu-id="69a48-112">Normalmente, las variables locales sin inicializar contienen los valores que se dejaron en la memoria que ocupa ahora el marco de pila.</span><span class="sxs-lookup"><span data-stu-id="69a48-112">Typically uninitialized locals contain whatever values were left in the memory that is now occupied by the stack frame.</span></span> <span data-ttu-id="69a48-113">Esto podría provocar un comportamiento no determinista y difícil reproducir errores.</span><span class="sxs-lookup"><span data-stu-id="69a48-113">That could lead to nondeterministic behavior and hard to reproduce bugs.</span></span> 

<span data-ttu-id="69a48-114">Hay dos maneras de "asignar" una variable local:</span><span class="sxs-lookup"><span data-stu-id="69a48-114">There are two ways to "assign" a local variable:</span></span> 
- <span data-ttu-id="69a48-115">almacenando un valor o</span><span class="sxs-lookup"><span data-stu-id="69a48-115">by storing a value or</span></span> 
- <span data-ttu-id="69a48-116">al especificar `localsinit` marca que fuerza a todo lo que se asigna, el grupo de memoria local debe inicializarse con ceros Nota: Esto incluye variables locales y datos de `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="69a48-116">by specifying `localsinit` flag which forces everything that is allocated form the local memory pool to be zero-initialized NOTE: this includes both local variables and `stackalloc` data.</span></span>    

<span data-ttu-id="69a48-117">No se recomienda el uso de datos no inicializados y no se permite en código comprobable.</span><span class="sxs-lookup"><span data-stu-id="69a48-117">Use of uninitialized data is discouraged and is not allowed in verifiable code.</span></span> <span data-ttu-id="69a48-118">Aunque podría ser posible demostrar que, por medio del análisis de flujo, se permite que el algoritmo de comprobación sea conservador y simplemente se requiere que se establezca `localsinit`.</span><span class="sxs-lookup"><span data-stu-id="69a48-118">While it might be possible to prove that by the means of flow analysis, it is permitted for the verification algorithm to be conservative and simply require that `localsinit` is set.</span></span>

<span data-ttu-id="69a48-119">Históricamente C# , el compilador emite `localsinit` marca en todos los métodos que declaran variables locales.</span><span class="sxs-lookup"><span data-stu-id="69a48-119">Historically C# compiler emits `localsinit` flag on all methods that declare locals.</span></span>

<span data-ttu-id="69a48-120">Aunque C# emplea el análisis de asignación desfinita que es más estricto que el que requeriría la especificaciónC# de CLR (también debe considerar el ámbito de las variables locales), no se garantiza estrictamente que el código resultante se pueda comprobar formalmente:</span><span class="sxs-lookup"><span data-stu-id="69a48-120">While C# employs definite-assignment analysis which is more strict than what CLR spec would require (C# also needs to consider scoping of locals), it is not strictly guaranteed that the resulting code would be formally verifiable:</span></span>
- <span data-ttu-id="69a48-121">Es posible C# que las reglas y CLR no estén de acuerdo en si pasar un argumento local como `out` es un `use`.</span><span class="sxs-lookup"><span data-stu-id="69a48-121">CLR and C# rules may not agree on whether passing a local as `out` argument is a `use`.</span></span>
- <span data-ttu-id="69a48-122">Es posible C# que las reglas y CLR no concuerden en el tratamiento de ramas condicionales cuando se conocen las condiciones (propagación constante).</span><span class="sxs-lookup"><span data-stu-id="69a48-122">CLR and C# rules may not agree on treatment of conditional branches when conditions are known (constant propagation).</span></span>
- <span data-ttu-id="69a48-123">CLR podría simplemente requerir `localinits`, ya que se permite.</span><span class="sxs-lookup"><span data-stu-id="69a48-123">CLR could as well simply require `localinits`, since that is permitted.</span></span>  

### <a name="problem"></a><span data-ttu-id="69a48-124">Problema</span><span class="sxs-lookup"><span data-stu-id="69a48-124">Problem</span></span>
<span data-ttu-id="69a48-125">En las aplicaciones de alto rendimiento, el costo de la inicialización de cero forzada podría ser apreciable.</span><span class="sxs-lookup"><span data-stu-id="69a48-125">In high-performance application the cost of forced zero-initialization could be noticeable.</span></span> <span data-ttu-id="69a48-126">Es especialmente evidente cuando se usa `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="69a48-126">It is particularly noticeable when `stackalloc` is used.</span></span>

<span data-ttu-id="69a48-127">En algunos casos, JIT puede Elide la inicialización cero inicial de las variables locales individuales cuando dicha inicialización se "elimina" mediante asignaciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="69a48-127">In some cases JIT can elide initial zero-initialization of individual locals when such initialization is "killed" by subsequent assignments.</span></span> <span data-ttu-id="69a48-128">No todas JITs lo hacen y esta optimización tiene límites.</span><span class="sxs-lookup"><span data-stu-id="69a48-128">Not all JITs do this and such optimization has limits.</span></span> <span data-ttu-id="69a48-129">No ayuda con `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="69a48-129">It does not help with `stackalloc`.</span></span>

<span data-ttu-id="69a48-130">Para ilustrar que el problema es real, hay un error conocido en el que un método que no contiene ninguna `IL` variables locales no tendría `localsinit` marca.</span><span class="sxs-lookup"><span data-stu-id="69a48-130">To illustrate that the problem is real - there is a known bug where a method not containing any `IL` locals would not have `localsinit` flag.</span></span> <span data-ttu-id="69a48-131">Los usuarios ya están aprovechando el error colocando `stackalloc` en estos métodos, intencionalmente para evitar los costos de inicialización.</span><span class="sxs-lookup"><span data-stu-id="69a48-131">The bug is already being exploited by users by putting `stackalloc` into such methods - intentionally to avoid initialization costs.</span></span> <span data-ttu-id="69a48-132">Es decir, a pesar de que la ausencia de `IL` variables locales es una métrica inestable y puede variar en función de los cambios en la estrategia CODEGEN.</span><span class="sxs-lookup"><span data-stu-id="69a48-132">That is despite the fact that absence of `IL` locals is an unstable metric and may vary depending on changes in codegen strategy.</span></span> <span data-ttu-id="69a48-133">El error debe corregirse y los usuarios deben obtener una forma más documentada y confiable de suprimir la marca.</span><span class="sxs-lookup"><span data-stu-id="69a48-133">The bug should be fixed and users should get a more documented and reliable way of suppressing the flag.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="69a48-134">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="69a48-134">Detailed design</span></span>

<span data-ttu-id="69a48-135">Permite especificar `System.Runtime.CompilerServices.SkipLocalsInitAttribute` como una manera de indicar al compilador que no emita `localsinit` marca.</span><span class="sxs-lookup"><span data-stu-id="69a48-135">Allow specifying `System.Runtime.CompilerServices.SkipLocalsInitAttribute` as a way to tell the compiler to not emit `localsinit` flag.</span></span>
 
<span data-ttu-id="69a48-136">El resultado final de este es que las variables locales no pueden inicializarse en cero mediante el JIT, que en la mayoría de los casos no se puede C#observar en.</span><span class="sxs-lookup"><span data-stu-id="69a48-136">The end result of this will be that the locals may not be zero-initialized by the JIT, which is in most cases unobservable in C#.</span></span>  
<span data-ttu-id="69a48-137">Además de que `stackalloc` datos no se inicializarán en cero.</span><span class="sxs-lookup"><span data-stu-id="69a48-137">In addition to that `stackalloc` data will not be zero-initialized.</span></span> <span data-ttu-id="69a48-138">Eso es claramente observable, pero también es el escenario más motivador.</span><span class="sxs-lookup"><span data-stu-id="69a48-138">That is definitely observable, but also is the most motivating scenario.</span></span>

<span data-ttu-id="69a48-139">Los destinos de atributo permitidos y reconocidos son: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface``Constructor`.</span><span class="sxs-lookup"><span data-stu-id="69a48-139">Permitted and recognized attribute targets are: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`.</span></span> <span data-ttu-id="69a48-140">Sin embargo, el compilador no requerirá que el atributo esté definido con los destinos enumerados, ni será de interés en qué ensamblado está definido el atributo.</span><span class="sxs-lookup"><span data-stu-id="69a48-140">However compiler will not require that attribute is defined with the listed targets nor it will care in which assembly the attribute is defined.</span></span> 

<span data-ttu-id="69a48-141">Cuando se especifica el atributo en un contenedor (`class`, `module`, que contiene el método para un método anidado,...), la marca afecta a todos los métodos contenidos en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="69a48-141">When attribute is specified on a container (`class`, `module`, containing method for a nested method, ...), the flag affects all methods contained within the container.</span></span>

<span data-ttu-id="69a48-142">Los métodos sintetizados "heredan" la marca del contenedor lógico/propietario.</span><span class="sxs-lookup"><span data-stu-id="69a48-142">Synthesized methods "inherit" the flag from the logical container/owner.</span></span> 

<span data-ttu-id="69a48-143">La marca solo afecta a la estrategia CODEGEN para los cuerpos de método reales.</span><span class="sxs-lookup"><span data-stu-id="69a48-143">The flag affects only codegen strategy for actual method bodies.</span></span> <span data-ttu-id="69a48-144">es decir,.</span><span class="sxs-lookup"><span data-stu-id="69a48-144">I.E.</span></span> <span data-ttu-id="69a48-145">la marca no tiene ningún efecto en los métodos abstractos y no se propaga a los métodos de reemplazo/implementación.</span><span class="sxs-lookup"><span data-stu-id="69a48-145">the flag has no effect on abstract methods and is not propagated to overriding/implementing methods.</span></span>

<span data-ttu-id="69a48-146">Esto es explícitamente una **_característica del compilador_** y **_no una característica del lenguaje_** .</span><span class="sxs-lookup"><span data-stu-id="69a48-146">This is explicitly a **_compiler feature_** and **_not a language feature_**.</span></span>  
<span data-ttu-id="69a48-147">Del mismo modo que los modificadores de línea de comandos del compilador, la característica controla los detalles de implementación de una C# estrategia de CODEGEN determinada y no necesita ser necesaria para la especificación.</span><span class="sxs-lookup"><span data-stu-id="69a48-147">Similarly to compiler command line switches the feature controls implementation details of a particular codegen strategy and does not need to be required by the C# spec.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="69a48-148">Desventajas</span><span class="sxs-lookup"><span data-stu-id="69a48-148">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="69a48-149">Es posible que los compiladores anteriores y otros no respeten el atributo.</span><span class="sxs-lookup"><span data-stu-id="69a48-149">Old/other compilers may not honor the attribute.</span></span>
<span data-ttu-id="69a48-150">Omitir el atributo es un comportamiento compatible.</span><span class="sxs-lookup"><span data-stu-id="69a48-150">Ignoring the attribute is compatible behavior.</span></span> <span data-ttu-id="69a48-151">Solo puede dar lugar a una ligera disminución del rendimiento.</span><span class="sxs-lookup"><span data-stu-id="69a48-151">Only may result in a slight perf hit.</span></span>

- <span data-ttu-id="69a48-152">El código sin `localinits` marca puede desencadenar errores de comprobación.</span><span class="sxs-lookup"><span data-stu-id="69a48-152">The code without `localinits` flag may trigger verification failures.</span></span>
<span data-ttu-id="69a48-153">Los usuarios que solicitan esta característica generalmente no se ven preocupadas por la capacidad de comprobación.</span><span class="sxs-lookup"><span data-stu-id="69a48-153">Users that ask for this feature are generally unconcerned with verifiability.</span></span> 
 
- <span data-ttu-id="69a48-154">Aplicar el atributo en niveles superiores a un método individual tiene un efecto no local, que es observable cuando se utiliza `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="69a48-154">Applying the attribute at higher levels than an individual method has nonlocal effect, which is observable when `stackalloc` is used.</span></span> <span data-ttu-id="69a48-155">Aún así, este es el escenario más solicitado.</span><span class="sxs-lookup"><span data-stu-id="69a48-155">Yet, this is the most requested scenario.</span></span>

## <a name="alternatives"></a><span data-ttu-id="69a48-156">Alternativas</span><span class="sxs-lookup"><span data-stu-id="69a48-156">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="69a48-157">omitir `localinits` marca cuando el método se declara en `unsafe` contexto.</span><span class="sxs-lookup"><span data-stu-id="69a48-157">omit `localinits` flag when method is declared in `unsafe` context.</span></span> <span data-ttu-id="69a48-158">Esto podría provocar un cambio de comportamiento silencioso y peligroso de determinista a no determinista en el caso de `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="69a48-158">That could cause silent and dangerous behavior change from deterministic to nondeterministic in a case of `stackalloc`.</span></span>

- <span data-ttu-id="69a48-159">omitir la marca de `localinits` siempre.</span><span class="sxs-lookup"><span data-stu-id="69a48-159">omit `localinits` flag always.</span></span>
<span data-ttu-id="69a48-160">Incluso peor que el anterior.</span><span class="sxs-lookup"><span data-stu-id="69a48-160">Even worse than above.</span></span>

- <span data-ttu-id="69a48-161">omita `localinits` marca a menos que se use `stackalloc` en el cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="69a48-161">omit `localinits` flag unless `stackalloc` is used in the method body.</span></span>
<span data-ttu-id="69a48-162">No aborda el escenario más solicitado y puede desactivar el código sin ninguna opción para revertirlo de nuevo.</span><span class="sxs-lookup"><span data-stu-id="69a48-162">Does not address the most requested scenario and may turn code unverifiable with no option to revert that back.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="69a48-163">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="69a48-163">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="69a48-164">¿Se debe emitir realmente el atributo a los metadatos?</span><span class="sxs-lookup"><span data-stu-id="69a48-164">Should the attribute be actually emitted to metadata?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="69a48-165">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="69a48-165">Design meetings</span></span>

<span data-ttu-id="69a48-166">Ninguno todavía.</span><span class="sxs-lookup"><span data-stu-id="69a48-166">None yet.</span></span> 