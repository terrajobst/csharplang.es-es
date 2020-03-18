---
ms.openlocfilehash: 77c9ffda3a59cc5f3dcc3ec9848600c6c5e03eed
ms.sourcegitcommit: 27487fa0294f4cdb7e5f2478884149e05314fd8a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "79483905"
---
# <a name="primary-constructors"></a><span data-ttu-id="86891-101">Constructores principales</span><span class="sxs-lookup"><span data-stu-id="86891-101">Primary constructors</span></span>

* <span data-ttu-id="86891-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="86891-102">[x] Proposed</span></span>
* <span data-ttu-id="86891-103">[] Prototipo: no iniciado</span><span class="sxs-lookup"><span data-stu-id="86891-103">[ ] Prototype: Not started</span></span>
* <span data-ttu-id="86891-104">[] Implementación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="86891-104">[ ] Implementation: Not started</span></span>
* <span data-ttu-id="86891-105">[] Especificación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="86891-105">[ ] Specification: Not started</span></span>

## <a name="summary"></a><span data-ttu-id="86891-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="86891-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="86891-107">Las clases pueden tener una lista de parámetros y, cuando lo hacen, su especificación de clase base puede tener una lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="86891-107">Classes can have a parameter list, and when they do, their base class specification can have an argument list.</span></span>
<span data-ttu-id="86891-108">Los parámetros del constructor principal están en el ámbito de la declaración de clase y, si los captura un miembro de función o una función anónima, se almacenan como campos privados en la clase.</span><span class="sxs-lookup"><span data-stu-id="86891-108">Primary constructor parameters are in scope throughout the class declaration, and if they are captured by a function member or anonymous function, they are stored as private fields in the class.</span></span>

## <a name="motivation"></a><span data-ttu-id="86891-109">Motivación</span><span class="sxs-lookup"><span data-stu-id="86891-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="86891-110">Es habitual tener muchos reutilizables en el código de inicialización del programa.</span><span class="sxs-lookup"><span data-stu-id="86891-110">It is common to have a lot of boilerplate in program initialization code.</span></span> <span data-ttu-id="86891-111">En el caso general, una parte determinada de `x` de datos se menciona muchas veces:</span><span class="sxs-lookup"><span data-stu-id="86891-111">In the general case, a given piece of data `x` is mentioned many times:</span></span>

- <span data-ttu-id="86891-112">Como un campo privado `_x`</span><span class="sxs-lookup"><span data-stu-id="86891-112">As a private field `_x`</span></span>
- <span data-ttu-id="86891-113">Como parámetro `x` a un constructor</span><span class="sxs-lookup"><span data-stu-id="86891-113">As a parameter `x` to a constructor</span></span>
- <span data-ttu-id="86891-114">En un `_x = x;` de asignación del campo del parámetro del constructor</span><span class="sxs-lookup"><span data-stu-id="86891-114">In an assignment `_x = x;` of the field from the parameter in the constructor</span></span>
- <span data-ttu-id="86891-115">Como una propiedad `X`</span><span class="sxs-lookup"><span data-stu-id="86891-115">As a property `X`</span></span>
- <span data-ttu-id="86891-116">En el establecedor de propiedad `x = value;`</span><span class="sxs-lookup"><span data-stu-id="86891-116">In the property setter `x = value;`</span></span>
- <span data-ttu-id="86891-117">En el `return x;` captador de propiedad</span><span class="sxs-lookup"><span data-stu-id="86891-117">In the property getter `return x;`</span></span>

``` c#
class C
{
    private string _x;
    
    public C(string x)
    {
        _x = x;
    }
    public string X
    {
        get => _x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); _x = value; }
    }
}
```

<span data-ttu-id="86891-118">En el caso de las propiedades que no requieren validación o cálculo, el tediosas tareas se puede reducir mediante propiedades automáticas, lo que reduce la necesidad de declarar un campo de respaldo explícito para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="86891-118">For properties that don't require validation or computation, the tedium can be reduced using auto-properties, thus cutting out the need to declare an explicit backing field for the property.</span></span> <span data-ttu-id="86891-119">Pero si la propiedad requiere cualquier tipo de lógica más allá de lo que proporciona una propiedad automática, lo mejor es lo que hace.</span><span class="sxs-lookup"><span data-stu-id="86891-119">But if your property requires any sort of logic beyond what an auto-property provides, the above is the best you an do.</span></span>

<span data-ttu-id="86891-120">En su lugar, los constructores principales reducen la sobrecarga mediante la colocación de los argumentos del constructor directamente en el ámbito de la clase, lo que vuelve a obviar la necesidad de declarar explícitamente un campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="86891-120">Primary constructors instead reduce the overhead by putting constructor arguments directly in scope throughout the class, again obviating the need to explicitly declare a backing field.</span></span> <span data-ttu-id="86891-121">Por lo tanto, el ejemplo anterior sería:</span><span class="sxs-lookup"><span data-stu-id="86891-121">Thus, the above example would become:</span></span>

``` c#
class C(string x)
{
    public string X
    {
        get => x;
        set { if (value == null) throw new NullArgumentException(nameof(X)); x = value; }
    }
}
```

<span data-ttu-id="86891-122">En este ejemplo, el constructor principal reduce el número de entidades con nombre para `x` de tres a dos, obviando el campo de respaldo `_x`.</span><span class="sxs-lookup"><span data-stu-id="86891-122">In this example, the primary constructor reduces the number of named entities for `x` from three to two, obviating the `_x` backing field.</span></span> <span data-ttu-id="86891-123">Quita dos declaraciones de miembro de tres (manteniendo solo la propia declaración de propiedad) y reduce el número total de menciones de `x`/`_x`/`X` de ocho a cinco.</span><span class="sxs-lookup"><span data-stu-id="86891-123">It removes two out of three member declarations (keeping only the property declaration itself), and reduces the total number of mentions of `x`/`_x`/`X` from eight to five.</span></span>


## <a name="detailed-design"></a><span data-ttu-id="86891-124">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="86891-124">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="86891-125">Las clases pueden tener una lista de parámetros:</span><span class="sxs-lookup"><span data-stu-id="86891-125">Classes can have a parameter list:</span></span>

``` c#
public class C(int i, string s)
{
    ...
}
```

<span data-ttu-id="86891-126">La lista de parámetros hace que se declare implícitamente un constructor para la clase, con la misma accesibilidad que la propia clase.</span><span class="sxs-lookup"><span data-stu-id="86891-126">The parameter list causes a constructor to be implicitly declared for the class, with the same accessibility as the class itself.</span></span>

``` c#
new C(5, "Hello");
```

<span data-ttu-id="86891-127">Los parámetros del constructor principal están en el ámbito de todo el cuerpo de la clase.</span><span class="sxs-lookup"><span data-stu-id="86891-127">Primary constructor parameters are in scope throughout the class body.</span></span> <span data-ttu-id="86891-128">Si se capturan mediante un miembro de función o una función anónima, se almacenan como campos privados en la clase.</span><span class="sxs-lookup"><span data-stu-id="86891-128">If they are captured by a function member or anonymous function, they become stored as private fields in the class.</span></span> <span data-ttu-id="86891-129">Si solo se utilizan durante la inicialización, no se almacenarán en el objeto.</span><span class="sxs-lookup"><span data-stu-id="86891-129">If they are only used during initialization they will not be stored in the object.</span></span>

``` c#
public class C(int i, string s)
{
    int[] a = new int[i]; // i not captured
    public int S => s;    // s captured
}
```

<span data-ttu-id="86891-130">Si una clase con un constructor principal tiene una especificación de clase base, ésta puede tener una lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="86891-130">If a class with a primary constructor has a base class specification, that one can have an argument list.</span></span> <span data-ttu-id="86891-131">Actúa como la lista de argumentos para un `base(...)` inicializador del constructor declarado implícitamente.</span><span class="sxs-lookup"><span data-stu-id="86891-131">This serves as the argument list to a `base(...)` initializer of the implicitly declared constructor.</span></span> <span data-ttu-id="86891-132">Si no se proporciona ninguna lista de argumentos, se supone que es una vacía.</span><span class="sxs-lookup"><span data-stu-id="86891-132">If no argument list is provided, an empty one is assumed.</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    ...
}
```
<span data-ttu-id="86891-133">La clase también puede tener constructores definidos explícitamente, pero todos ellos tienen que utilizar un inicializador de `this(...)`.</span><span class="sxs-lookup"><span data-stu-id="86891-133">The class can have explicitly defined constructors as well, but those all have to use a `this(...)` initializer.</span></span> <span data-ttu-id="86891-134">Esto garantiza que siempre se llama al constructor principal cuando se crea una nueva instancia.</span><span class="sxs-lookup"><span data-stu-id="86891-134">This ensures that the primary constructor is always called when a new instance is constructed.</span></span>

<span data-ttu-id="86891-135">Todos los inicializadores del cuerpo de la clase se convertirán en asignaciones en el constructor generado.</span><span class="sxs-lookup"><span data-stu-id="86891-135">All initializers in the class body will become assignments in the generated constructor.</span></span> <span data-ttu-id="86891-136">Esto significa que, a diferencia de otras clases, los inicializadores se ejecutarán *después* de que se haya invocado el constructor base, no antes de.</span><span class="sxs-lookup"><span data-stu-id="86891-136">This means that, unlike other classes, initializers will run *after* the base constructor has been invoked, not before.</span></span> <span data-ttu-id="86891-137">Además, la clase generada contendrá asignaciones para inicializar los campos privados que se generaron para almacenar los parámetros de constructor primario capturados por los miembros.</span><span class="sxs-lookup"><span data-stu-id="86891-137">In addition, the generated class will contain assignments to initialize any private fields that were generated to store primary constructor parameters that were captured by members.</span></span> <span data-ttu-id="86891-138">Estos miembros se reescriben para utilizar el campo privado en lugar del parámetro de una manera similar a los cierres de las expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="86891-138">Those members are rewritten to use the private field instead of the parameter in a manner similar to closures for lambda expressions.</span></span> <span data-ttu-id="86891-139">Los campos principales generados se inicializan primero y, a continuación, las asignaciones generadas por el inicializador se ejecutan en el orden de aparición en la clase.</span><span class="sxs-lookup"><span data-stu-id="86891-139">The generated primary fields are initialized first, and then the initializer-generated assignments are executed in the order of appearance in the class.</span></span>

<span data-ttu-id="86891-140">En el ejemplo anterior, el efecto de la declaración de clase es como si se reescribira de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="86891-140">For the above example, the effect of the class declaration is as if rewritten like this:</span></span>

``` c#
public class C : B
{
    public C(int i, string s) : base(s)
    {
        __s = s;        // store parameter s for captured use
        a = new int[i]; // initialize a
    }
    int __s; // generated field for capture of s
    
    int[] a;
    public int S => __s; // s replaced with captured __s
}
```

<span data-ttu-id="86891-141">La captura tiene restricciones similares a la captura de variables locales mediante expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="86891-141">The capture has similar restrictions to the capture of local variables by lambda expressions.</span></span> <span data-ttu-id="86891-142">Por ejemplo, los parámetros `ref` y `out` se permiten en los constructores principales, pero no se pueden capturar en los cuerpos de los miembros.</span><span class="sxs-lookup"><span data-stu-id="86891-142">For instance, `ref` and `out` parameters are allowed in primary constructors, but cannot be captured my member bodies.</span></span>


## <a name="drawbacks"></a><span data-ttu-id="86891-143">Desventajas</span><span class="sxs-lookup"><span data-stu-id="86891-143">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="86891-144">En orden aproximado de importancia.</span><span class="sxs-lookup"><span data-stu-id="86891-144">In rough order of significance.</span></span>

* <span data-ttu-id="86891-145">La propuesta utiliza la sintaxis que también se ha propuesto para los registros posicionales.</span><span class="sxs-lookup"><span data-stu-id="86891-145">The proposal uses syntax that has also been proposed for positional records.</span></span> <span data-ttu-id="86891-146">Si deseamos ambas características, se requiere algún ajuste.</span><span class="sxs-lookup"><span data-stu-id="86891-146">If we desire both features, some accommodation is required.</span></span> <span data-ttu-id="86891-147">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="86891-147">E.g.</span></span> <span data-ttu-id="86891-148">se ha propuesto un modificador `data` en los registros.</span><span class="sxs-lookup"><span data-stu-id="86891-148">a `data` modifier on records has been proposed.</span></span>
* <span data-ttu-id="86891-149">El tamaño de asignación de los objetos construidos es menos obvio, ya que el compilador determina si se debe asignar un campo para un parámetro de constructor principal basado en el texto completo de la clase.</span><span class="sxs-lookup"><span data-stu-id="86891-149">The allocation size of constructed objects is less obvious, as the compiler determines whether to allocate a field for a primary constructor parameter based on the full text of the class.</span></span> <span data-ttu-id="86891-150">Este riesgo es similar a la captura implícita de variables mediante expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="86891-150">This risk is similar to the implicit capture of variables by lambda expressions.</span></span>
* <span data-ttu-id="86891-151">Una tentación común (o un patrón accidental) podría ser capturar el "mismo" parámetro en varios niveles de herencia a medida que se pasa la cadena del constructor en lugar de allotting explícitamente un campo protegido en la clase base, lo que conduce a asignaciones duplicadas. para los mismos datos en los objetos.</span><span class="sxs-lookup"><span data-stu-id="86891-151">A common temptation (or accidental pattern) might be to capture the "same" parameter at multiple levels of inheritance as it is passed up the constructor chain instead of explicitly allotting it a protected field at the base class, leading to duplicated allocations for the same data in objects.</span></span> <span data-ttu-id="86891-152">Esto es muy similar al riesgo actual de invalidar propiedades automáticas con propiedades automáticas.</span><span class="sxs-lookup"><span data-stu-id="86891-152">This is very similar to today's risk of overriding auto-properties with auto-properties.</span></span> 
* <span data-ttu-id="86891-153">Tal y como se ha propuesto anteriormente, no hay ningún lugar para la lógica adicional que normalmente se expresaría en los cuerpos del constructor.</span><span class="sxs-lookup"><span data-stu-id="86891-153">As proposed above, there is no place for additional logic that might usually expressed in constructor bodies.</span></span> <span data-ttu-id="86891-154">La extensión "cuerpos del constructor principal" que se encuentra debajo de direcciones.</span><span class="sxs-lookup"><span data-stu-id="86891-154">The "Primary constructor bodies" extension below addresses that.</span></span>
* <span data-ttu-id="86891-155">Como se propone, la semántica de orden de ejecución es ligeramente diferente de la de los constructores ordinarios.</span><span class="sxs-lookup"><span data-stu-id="86891-155">As proposed, execution order semantics are subtly different than with ordinary constructors.</span></span> <span data-ttu-id="86891-156">Probablemente, esto podría ser solucionado, pero a costa de algunas de las propuestas de extensión (en particular, "cuerpos de constructor primario").</span><span class="sxs-lookup"><span data-stu-id="86891-156">This could probably be remedied, but at the cost of some of the extension proposals (notably "Primary constructor bodies").</span></span>
* <span data-ttu-id="86891-157">La propuesta solo funciona cuando un solo constructor puede designarse como principal.</span><span class="sxs-lookup"><span data-stu-id="86891-157">The proposal only works when a single constructor can be designated primary.</span></span>
* <span data-ttu-id="86891-158">No hay ninguna manera de tener accesibilidad independiente de la clase y del constructor principal.</span><span class="sxs-lookup"><span data-stu-id="86891-158">There is no way to have separate accessibility of the class and the primary constructor.</span></span> <span data-ttu-id="86891-159">Por ejemplo, en situaciones en las que todos los constructores públicos se delegan a un constructor privado "Build-It-All" que sería necesario.</span><span class="sxs-lookup"><span data-stu-id="86891-159">For instance, in situations where public constructors all delegate to one private "build-it-all" constructor that would be needed.</span></span> <span data-ttu-id="86891-160">Si es necesario, la sintaxis podría proponerse más adelante.</span><span class="sxs-lookup"><span data-stu-id="86891-160">If necessary, syntax could be proposed for that later.</span></span>


## <a name="alternatives"></a><span data-ttu-id="86891-161">Alternativas</span><span class="sxs-lookup"><span data-stu-id="86891-161">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="86891-162">Los registros posicionales completos pueden ser una alternativa, o pueden coexistir con constructores principales, en función de los detalles.</span><span class="sxs-lookup"><span data-stu-id="86891-162">Full-on positional records may be an alternative, or may coexist with primary constructors, depending on the specifics.</span></span> <span data-ttu-id="86891-163">Permiten *más* abreviaturas en un *menor* número de escenarios.</span><span class="sxs-lookup"><span data-stu-id="86891-163">They would allow for *more* abbreviation in a *smaller* number of scenarios.</span></span> <span data-ttu-id="86891-164">Por tanto, los dos son potencialmente útiles, pero tener ambos pueden ser exagerados, a menos que se integren perfectamente entre sí.</span><span class="sxs-lookup"><span data-stu-id="86891-164">So both are potentially useful, but having both may be overkill, unless they can be somewhat neatly integrated with each other.</span></span>


## <a name="possible-extensions"></a><span data-ttu-id="86891-165">Posibles extensiones</span><span class="sxs-lookup"><span data-stu-id="86891-165">Possible extensions</span></span>
[extensions]: #possible-extensions

<span data-ttu-id="86891-166">Se trata de variaciones o adiciones a la propuesta de núcleo que se pueden considerar en conjunción con ella o en una fase posterior, si se considera útil.</span><span class="sxs-lookup"><span data-stu-id="86891-166">These are variations or additions to the core proposal that may be considered in conjunction with it, or at a later stage if deemed useful.</span></span>

### <a name="primary-constructor-bodies"></a><span data-ttu-id="86891-167">Cuerpos del constructor principal</span><span class="sxs-lookup"><span data-stu-id="86891-167">Primary constructor bodies</span></span>

<span data-ttu-id="86891-168">A menudo, los constructores contienen lógica de validación de parámetros u otro código de inicialización no trivial que no se puede expresar como inicializadores.</span><span class="sxs-lookup"><span data-stu-id="86891-168">Constructors themselves often contain parameter validation logic or other nontrivial initialization code that cannot be expressed as initializers.</span></span>

<span data-ttu-id="86891-169">Los constructores principales se pueden extender para permitir que los bloques de instrucciones aparezcan directamente en el cuerpo de la clase.</span><span class="sxs-lookup"><span data-stu-id="86891-169">Primary constructors could be extended to allow statement blocks to appear directly in the class body.</span></span> <span data-ttu-id="86891-170">Esas instrucciones se insertarían en el constructor generado en el punto en el que aparecen entre las asignaciones de inicialización y, por tanto, se ejecutaría intercalada con inicializadores.</span><span class="sxs-lookup"><span data-stu-id="86891-170">Those statements would be inserted in the generated constructor at the point where they appear between initializing assignments, and would thus be executed interspersed with initializers.</span></span> <span data-ttu-id="86891-171">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="86891-171">For instance:</span></span>

``` c#
public class C(int i, string s) : B(s)
{
    {
        if (i < 0) throw new ArgumentOutOfRangeException(nameof(i));
    }
    int[] a = new int[i];
    public int S => s;
}
```

### <a name="initializer-fields-and-initializer-functions"></a><span data-ttu-id="86891-172">Campos de inicializador y funciones de inicializador</span><span class="sxs-lookup"><span data-stu-id="86891-172">Initializer fields and initializer functions</span></span>

<span data-ttu-id="86891-173">En una clase con un constructor principal, podríamos considerar las declaraciones de campo y método sin modificadores de accesibilidad para que sean más similares a las variables locales y las funciones locales:</span><span class="sxs-lookup"><span data-stu-id="86891-173">In a class with a primary constructor we could consider field and method declarations without accessibility modifiers to be more like local variables and local functions:</span></span>

* <span data-ttu-id="86891-174">Al igual que los parámetros de constructor principal, los "campos de inicializador" solo se capturan en un campo privado real si se usaron en miembros de función.</span><span class="sxs-lookup"><span data-stu-id="86891-174">Just like primary constructor parameters the "initializer fields" would only be captured into an actual private field if they were used in function members.</span></span>
* <span data-ttu-id="86891-175">Las "funciones de inicializador" solo se considerarían para capturar los parámetros del constructor principal y los campos de inicializador si se utilizaban en otros miembros de función.</span><span class="sxs-lookup"><span data-stu-id="86891-175">The "initializer functions" would only be considered to capture primary constructor parameters and initializer fields if they were themselves used in other function members.</span></span> <span data-ttu-id="86891-176">Si no se capturan, podrían generarse de manera más óptima, como las funciones locales.</span><span class="sxs-lookup"><span data-stu-id="86891-176">If not captured, they could be generated in a more optimal fashion, like local functions.</span></span>
* <span data-ttu-id="86891-177">Al igual que los parámetros del constructor principal, no estarán disponibles a través del acceso a miembros, sino solo como un nombre simple.</span><span class="sxs-lookup"><span data-stu-id="86891-177">Just like primary constructor parameters they would not be available via member access, but only as a simple name.</span></span>

<span data-ttu-id="86891-178">Se puede usar para las variables temporales y las funciones auxiliares que solo son relevantes para la inicialización:</span><span class="sxs-lookup"><span data-stu-id="86891-178">This could be used for temporary variables and helper functions that are only relevant to initialization:</span></span>

``` c#
public class C(string s)
{
    int size = s.Length;             // not captured
    int[] Create() => new int[size]; // not captured
    int[] a = Create();
    ...
}
```

<span data-ttu-id="86891-179">Esto puede ser demasiado sutil, especialmente porque la ausencia de modificadores de accesibilidad en otro lugar simplemente significa `private`.</span><span class="sxs-lookup"><span data-stu-id="86891-179">This may be too subtle, especially since the absence of accessibility modifiers elsewhere simply means `private`.</span></span> 

### <a name="initializer-statements"></a><span data-ttu-id="86891-180">Instrucciones de inicializador</span><span class="sxs-lookup"><span data-stu-id="86891-180">Initializer statements</span></span>

<span data-ttu-id="86891-181">Una combinación radical de las extensiones anteriores sería permitir simplemente instrucciones directamente en el cuerpo de la clase.</span><span class="sxs-lookup"><span data-stu-id="86891-181">A radical combination of the above to extensions would be to simply allow statements directly in the class body.</span></span> <span data-ttu-id="86891-182">Estas instrucciones son exactamente iguales que los cuerpos de constructor intercalados propuestos anteriormente, salvo que no es necesario que se incluyan en `{ }`.</span><span class="sxs-lookup"><span data-stu-id="86891-182">Such statements are exactly as the interspersed constructor bodies proposed above, except they don't need to be enclosed in `{ }`.</span></span> <span data-ttu-id="86891-183">Para que esto sea suficientemente útil, las variables y las funciones auxiliares "locales" también tendrían que ser expresables en el nivel superior de la clase, de la manera que se explora en la extensión "campos de inicializador y funciones de inicializador" anterior.</span><span class="sxs-lookup"><span data-stu-id="86891-183">For this to be sufficiently useful, "local" variables and helper functions would need to also be expressible at the top level of the class, in the manner explored in the extension "Initializer fields and initializer functions" above.</span></span>


### <a name="member-access"></a><span data-ttu-id="86891-184">Acceso a miembros</span><span class="sxs-lookup"><span data-stu-id="86891-184">Member access</span></span>

<span data-ttu-id="86891-185">La propuesta principal trata los parámetros del constructor principal como parámetros a los que solo se puede hacer referencia como nombres simples.</span><span class="sxs-lookup"><span data-stu-id="86891-185">The core proposal treats primary constructor parameters as parameters that can only be referred as simple names.</span></span> <span data-ttu-id="86891-186">Una alternativa consiste en permitir que se haga referencia a ellas como si fueran campos privados, es decir, con un acceso a miembros, *aunque* a veces no se generen como campos.</span><span class="sxs-lookup"><span data-stu-id="86891-186">An alternative is to allow them to be referenced as if they were private fields, i.e. with a member access, *even* if they are sometimes not generated as fields.</span></span> <span data-ttu-id="86891-187">Esto permitiría hacer referencia a ellas como `this.x` cuando las prevalecen las variables locales y se podía obtener acceso a ellas desde una instancia diferente como `other.x`.</span><span class="sxs-lookup"><span data-stu-id="86891-187">This would allow them to be referenced as `this.x` when shadowed by local variables, and accessed from a different instance as `other.x`.</span></span>

<span data-ttu-id="86891-188">Si se aplica a la extensión "campos de inicializador y funciones de inicializador", esto también reduciría el grado en el que eran diferentes de los miembros privados normales.</span><span class="sxs-lookup"><span data-stu-id="86891-188">If applied to the "initializer fields and initializer functions" extension this would also reduce the degree to which those were different from ordinary private members.</span></span> <span data-ttu-id="86891-189">La única diferencia sería que el compilador es libre de elide desde el objeto si solo se utiliza durante la inicialización.</span><span class="sxs-lookup"><span data-stu-id="86891-189">The only difference would then be that the compiler is free to elide them from the object if only used during initialization.</span></span>

