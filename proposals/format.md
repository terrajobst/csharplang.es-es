---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "79483941"
---
# <a name="efficient-params-and-string-formatting"></a><span data-ttu-id="54372-101">Parámetros eficientes y formato de cadena</span><span class="sxs-lookup"><span data-stu-id="54372-101">Efficient Params and String Formatting</span></span>

## <a name="summary"></a><span data-ttu-id="54372-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="54372-102">Summary</span></span>
<span data-ttu-id="54372-103">Esta combinación de características aumentará la eficacia del formato `string` valores y el paso de los argumentos de estilo `params`.</span><span class="sxs-lookup"><span data-stu-id="54372-103">This combination of features will increase the efficiency of formatting `string` values and passing of `params` style arguments.</span></span>

## <a name="motivation"></a><span data-ttu-id="54372-104">Motivación</span><span class="sxs-lookup"><span data-stu-id="54372-104">Motivation</span></span>
<span data-ttu-id="54372-105">La sobrecarga de asignación del formato `string` valores puede dominar el rendimiento de muchas aplicaciones basadas en texto: a partir de la penalización Boxing de los tipos de `struct`, la asignación de `object[]` para `params` y las asignaciones de `string` intermedias durante `string.Format` llamadas.</span><span class="sxs-lookup"><span data-stu-id="54372-105">The allocation overhead of formatting `string` values can dominate the performance of many text based applications: from the boxing penalty of `struct` types, the `object[]` allocation for `params` and the intermediate `string` allocations during `string.Format` calls.</span></span> <span data-ttu-id="54372-106">Con el fin de mantener la eficacia, estas aplicaciones suelen necesitar abandonar características de productividad, como `params` y `string` interpolación y pasar a soluciones codificadas no estándar.</span><span class="sxs-lookup"><span data-stu-id="54372-106">In order to maintain efficiency such applications often need to abandon productivity features such as `params` and `string` interpolation and move to non-standard, hand coded solutions.</span></span> 

<span data-ttu-id="54372-107">Considere MSBuild como ejemplo.</span><span class="sxs-lookup"><span data-stu-id="54372-107">Consider MSBuild as an example.</span></span> <span data-ttu-id="54372-108">Esto se escribe con muchas características modernas C# de los desarrolladores que son conscientes del rendimiento.</span><span class="sxs-lookup"><span data-stu-id="54372-108">This is written using a lot of modern C# features by developers who are conscious of performance.</span></span> <span data-ttu-id="54372-109">Sin embargo, en un ejemplo de compilación representativo MSBuild generará 262MB de asignación `string` con un nivel de detalle mínimo.</span><span class="sxs-lookup"><span data-stu-id="54372-109">Yet in one representative build sample MSBuild will generate 262MB of `string` allocation using minimal verbosity.</span></span> <span data-ttu-id="54372-110">De que 1/2 de las asignaciones son asignaciones de corta duración dentro de `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="54372-110">Of that 1/2 of the allocations are short lived allocations inside `string.Format`.</span></span> <span data-ttu-id="54372-111">Estas características quitarían gran parte de eso en el escritorio de .NET y la bajamos a casi cero en .NET Core debido a la disponibilidad de `Span<T>`</span><span class="sxs-lookup"><span data-stu-id="54372-111">These features would remove much of that on .NET Desktop and get it down to nearly zero on .NET Core due to the availability of `Span<T>`</span></span>

<span data-ttu-id="54372-112">El conjunto de características del lenguaje que se describe aquí permitirá a las aplicaciones seguir usando estas características, con muy poca o ninguna actividad en la base de código de la aplicación, al tiempo que se elimina la sobrecarga de asignación no deseada en la mayoría de los casos.</span><span class="sxs-lookup"><span data-stu-id="54372-112">The set of language features described here will enable applications to continue using these features, with very little or no churn to their application code base, while removing the unintended allocation overhead in the majority of cases.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="54372-113">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="54372-113">Detailed Design</span></span> 
<span data-ttu-id="54372-114">Hay un conjunto de características que se utilizarán aquí para lograr estos resultados:</span><span class="sxs-lookup"><span data-stu-id="54372-114">There are a set of features that will be used here to achieve these results:</span></span>

- <span data-ttu-id="54372-115">Expandir `params` para admitir un conjunto más amplio de tipos de colección.</span><span class="sxs-lookup"><span data-stu-id="54372-115">Expanding `params` to support a broader set of collection types.</span></span>
- <span data-ttu-id="54372-116">Permite a los desarrolladores personalizar cómo se consigue `string` interpolación.</span><span class="sxs-lookup"><span data-stu-id="54372-116">Allowing for developers to customize how `string` interpolation is achieved.</span></span> 
- <span data-ttu-id="54372-117">Permite que los `string` interpolados se enlacen a sobrecargas de `string.Format` más eficaces.</span><span class="sxs-lookup"><span data-stu-id="54372-117">Allowing for interpolated `string` to bind to more efficient `string.Format` overloads.</span></span>

### <a name="extending-params"></a><span data-ttu-id="54372-118">Extender los parámetros</span><span class="sxs-lookup"><span data-stu-id="54372-118">Extending params</span></span>
<span data-ttu-id="54372-119">El lenguaje permitirá que `params` en una signatura de método tenga los tipos `Span<T>`, `ReadOnlySpan<T>` y `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="54372-119">The language will allow for `params` in a method signature to have the types `Span<T>`, `ReadOnlySpan<T>` and `IEnumerable<T>`.</span></span> <span data-ttu-id="54372-120">Las mismas reglas de invocación se aplicarán a estos nuevos tipos que se aplican a `params T[]`:</span><span class="sxs-lookup"><span data-stu-id="54372-120">The same rules for invocation will apply to these new types that apply to `params T[]`:</span></span>

- <span data-ttu-id="54372-121">No se puede sobrecargar cuando la única diferencia es una palabra clave de `params`.</span><span class="sxs-lookup"><span data-stu-id="54372-121">Can't overload where the only difference is a `params` keyword.</span></span>
- <span data-ttu-id="54372-122">Puede invocar pasando una serie de argumentos que se pueden convertir implícitamente en `T` o en un único `Span<T>` / 
`ReadOnlySpan<T>` / argumento.`IEnumerable<T>`</span><span class="sxs-lookup"><span data-stu-id="54372-122">Can invoke by passing a series of arguments that are implicitly convertible to `T` or a single `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` argument.</span></span>
- <span data-ttu-id="54372-123">Debe ser el último parámetro de una firma de método.</span><span class="sxs-lookup"><span data-stu-id="54372-123">Must be the last parameter in a method signature.</span></span>
- <span data-ttu-id="54372-124">Etc...</span><span class="sxs-lookup"><span data-stu-id="54372-124">Etc ...</span></span> 

<span data-ttu-id="54372-125">Se hará referencia a las variantes `Span<T>` y `ReadOnlySpan<T>` como `Span<T>` por motivos de simplicidad.</span><span class="sxs-lookup"><span data-stu-id="54372-125">The `Span<T>` and `ReadOnlySpan<T>` variants will be referred to as `Span<T>` below for simplicity.</span></span> <span data-ttu-id="54372-126">En los casos en los que el comportamiento de `ReadOnlySpan<T>` difiere, se llamará explícitamente.</span><span class="sxs-lookup"><span data-stu-id="54372-126">In cases where the behavior of `ReadOnlySpan<T>` differs it will be explicitly called out.</span></span> 

<span data-ttu-id="54372-127">La ventaja que proporcionan las variantes `Span<T>` de `params` es que proporciona al compilador una gran flexibilidad en cuanto a cómo asigna el almacenamiento de seguridad para el valor de `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="54372-127">The advantage the `Span<T>` variants of `params` provides is it gives the compiler great flexibility in how it allocates the backing storage for the `Span<T>` value.</span></span> <span data-ttu-id="54372-128">Con un `params T[]` el compilador debe asignar un nuevo `T[]` para cada invocación de un método `params`.</span><span class="sxs-lookup"><span data-stu-id="54372-128">With a `params T[]` the compiler must allocate a new `T[]` for every invocation of a `params` method.</span></span> <span data-ttu-id="54372-129">La reutilización no es posible porque debe asumir que el destinatario de la llamada almacenó y volvió a usar el parámetro.</span><span class="sxs-lookup"><span data-stu-id="54372-129">Re-use is not possible because it must assume the callee stored and reused the parameter.</span></span> <span data-ttu-id="54372-130">Esto puede dar lugar a una gran ineficacia en métodos con muchas de las invocaciones de `params`.</span><span class="sxs-lookup"><span data-stu-id="54372-130">This can lead to a large inefficiency in methods with lots of `params` invocations.</span></span>

<span data-ttu-id="54372-131">Determinadas variantes de `Span<T>` se `ref struct` el destinatario no puede almacenar el argumento.</span><span class="sxs-lookup"><span data-stu-id="54372-131">Given `Span<T>` variants are `ref struct` the callee cannot store the argument.</span></span> <span data-ttu-id="54372-132">Por lo tanto, el compilador puede optimizar los sitios de llamada realizando acciones como volver a usar el argumento.</span><span class="sxs-lookup"><span data-stu-id="54372-132">Hence the compiler can optimize the call sites by taking actions like re-using the argument.</span></span> <span data-ttu-id="54372-133">Esto puede hacer que las invocaciones repetidas sean muy eficaces en comparación con `T[]`.</span><span class="sxs-lookup"><span data-stu-id="54372-133">This can make repeated invocations very efficient as compared to `T[]`.</span></span> <span data-ttu-id="54372-134">Sin embargo, el lenguaje no realizará ninguna garantía específica sobre cómo se optimizan estos sitios.</span><span class="sxs-lookup"><span data-stu-id="54372-134">The language though will make no specific guarantees about how such callsites are optimized.</span></span> <span data-ttu-id="54372-135">Tenga en cuenta solo que el compilador es libre de usar valores distintos de `T[]` al invocar un método `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="54372-135">Only note that the compiler is free to use values other than `T[]` when invoking a `params Span<T>` method.</span></span> 

<span data-ttu-id="54372-136">Una de estas posibles implementaciones es la siguiente.</span><span class="sxs-lookup"><span data-stu-id="54372-136">One such potential implementation is the following.</span></span> <span data-ttu-id="54372-137">Considere todas `params` invocación en un cuerpo de método.</span><span class="sxs-lookup"><span data-stu-id="54372-137">Consider all `params` invocation in a method body.</span></span> <span data-ttu-id="54372-138">El compilador podría asignar una matriz que tiene un tamaño igual a la invocación de `params` más grande y usarla para todas las invocaciones creando instancias de `Span<T>` de tamaño adecuado en la matriz.</span><span class="sxs-lookup"><span data-stu-id="54372-138">The compiler could allocate an array which has a size equal to the largest `params` invocation and use that for all of the invocations by creating appropriately sized `Span<T>` instances over the array.</span></span> <span data-ttu-id="54372-139">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="54372-139">For example:</span></span>

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

<span data-ttu-id="54372-140">El compilador podría elegir emitir el cuerpo de `Go` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="54372-140">The compiler could choose to emit the body of `Go` as follows:</span></span>

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

<span data-ttu-id="54372-141">Esto puede reducir significativamente el número de matrices asignadas en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="54372-141">This can significantly reduce the number of arrays allocated in an application.</span></span> <span data-ttu-id="54372-142">Las asignaciones se pueden reducir aún más si el tiempo de ejecución proporciona utilidades para la asignación de matrices más inteligente.</span><span class="sxs-lookup"><span data-stu-id="54372-142">Allocations can be even further reduced if the runtime provides utilities for smarter stack allocation of arrays.</span></span>

<span data-ttu-id="54372-143">Sin embargo, esta optimización no siempre se puede aplicar.</span><span class="sxs-lookup"><span data-stu-id="54372-143">This optimization cannot always be applied though.</span></span> <span data-ttu-id="54372-144">Aunque el destinatario no puede capturar el argumento `params`, todavía se puede capturar en el llamador cuando hay un `ref` o un parámetro `out / ref` que es en sí mismo un `ref struct` tipo.</span><span class="sxs-lookup"><span data-stu-id="54372-144">Even though the callee cannot capture the `params` argument it can still be captured in the caller when there is a `ref` or a `out / ref` parameter that is itself a `ref struct` type.</span></span> 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

<span data-ttu-id="54372-145">Sin embargo, estos casos se detectan estáticamente.</span><span class="sxs-lookup"><span data-stu-id="54372-145">These cases are statically detectable though.</span></span> <span data-ttu-id="54372-146">Puede producirse siempre que haya un `ref` devolver o un parámetro `ref struct` pasado por `out` o `ref`.</span><span class="sxs-lookup"><span data-stu-id="54372-146">It potentially occurs whenever there is a `ref` return or a `ref struct` parameter passed by `out` or `ref`.</span></span> <span data-ttu-id="54372-147">En tal caso, el compilador debe asignar un `T[]` nuevo para cada invocación.</span><span class="sxs-lookup"><span data-stu-id="54372-147">In such a case the compiler must allocate a fresh `T[]` for every invocation.</span></span> 

<span data-ttu-id="54372-148">Al final de este documento se describen otras estrategias de optimización potenciales.</span><span class="sxs-lookup"><span data-stu-id="54372-148">Several other potential optimization strategies are discussed at the end of this document.</span></span>

<span data-ttu-id="54372-149">La variante `IEnumerable<T>` es una simple sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="54372-149">The `IEnumerable<T>` variant is a merely a convenience overload.</span></span> <span data-ttu-id="54372-150">Es útil en escenarios que tienen usos frecuentes de `IEnumerable<T>` pero también tienen muchos `params` uso.</span><span class="sxs-lookup"><span data-stu-id="54372-150">It's useful in scenarios which have frequent uses of `IEnumerable<T>` but also have lots of `params` usage.</span></span> <span data-ttu-id="54372-151">Cuando se invoca en `T` forma de argumento, el almacenamiento de respaldo se asignará como un `T[]` igual que `params T[]` se realiza hoy.</span><span class="sxs-lookup"><span data-stu-id="54372-151">When invoked in `T` argument form the backing storage will be allocated as a `T[]` just as `params T[]` is done today.</span></span>

### <a name="params-overload-resolution-changes"></a><span data-ttu-id="54372-152">cambios en la resolución de sobrecarga de params</span><span class="sxs-lookup"><span data-stu-id="54372-152">params overload resolution changes</span></span>
<span data-ttu-id="54372-153">Esta propuesta significa que el lenguaje tiene ahora cuatro variantes de `params` donde antes tenía una.</span><span class="sxs-lookup"><span data-stu-id="54372-153">This proposal means the language now has four variants of `params` where before it had one.</span></span> <span data-ttu-id="54372-154">Es sensato que los métodos definan sobrecargas de métodos que solo difieren en el tipo de una `params` declaraciones.</span><span class="sxs-lookup"><span data-stu-id="54372-154">It is sensible for methods to define overloads of methods that differ only on the type of a `params` declarations.</span></span> 

<span data-ttu-id="54372-155">Tenga en cuenta que `StringBuilder.AppendFormat` ciertamente agregaría una sobrecarga de `params ReadOnlySpan<object>` además de la `params object[]`.</span><span class="sxs-lookup"><span data-stu-id="54372-155">Consider that `StringBuilder.AppendFormat` would certainly add a `params ReadOnlySpan<object>` overload in addition to the `params object[]`.</span></span> <span data-ttu-id="54372-156">Esto permitiría mejorar considerablemente el rendimiento al reducir las asignaciones de colección sin necesidad de realizar cambios en el código de llamada.</span><span class="sxs-lookup"><span data-stu-id="54372-156">This would allow it to substantially improve performance by reducing collection allocations without requiring any changes to the calling code.</span></span> 

<span data-ttu-id="54372-157">Para facilitar este procedimiento, se presentará la siguiente regla de interrupción de la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="54372-157">To facilitate this the language will introduce the following overload resolution tie breaking rule.</span></span> <span data-ttu-id="54372-158">Cuando los métodos candidatos solo difieren en el parámetro `params`, se preferirán los candidatos en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="54372-158">When the candidate methods differ only by the `params` parameter then the candidates will be preferred in the following order:</span></span>

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

<span data-ttu-id="54372-159">Este orden es el menos eficaz para el caso general.</span><span class="sxs-lookup"><span data-stu-id="54372-159">This order is the most to the least efficient for the general case.</span></span>

### <a name="variant"></a><span data-ttu-id="54372-160">Variant</span><span class="sxs-lookup"><span data-stu-id="54372-160">Variant</span></span>
<span data-ttu-id="54372-161">CoreFX está prototipo de un nuevo tipo administrado denominado [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span><span class="sxs-lookup"><span data-stu-id="54372-161">CoreFX is prototyping a new managed type named [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span></span> <span data-ttu-id="54372-162">Este tipo está pensado para usarse en las API que esperan valores heterogéneos, pero no desea que la sobrecarga que se lleva a cabo mediante `object` como parámetro.</span><span class="sxs-lookup"><span data-stu-id="54372-162">This type is meant to be used in APIs which expect heterogeneous values but don't want the overhead brought on by using `object` as the parameter.</span></span> <span data-ttu-id="54372-163">El tipo de `Variant` proporciona almacenamiento universal, pero evita la asignación de conversión boxing para los tipos de uso más frecuente.</span><span class="sxs-lookup"><span data-stu-id="54372-163">The `Variant` type provides universal storage but avoids the boxing allocation for the most commonly used types.</span></span> <span data-ttu-id="54372-164">El uso de este tipo en las API como `string.Format` puede eliminar la sobrecarga de conversión boxing en la mayoría de los casos.</span><span class="sxs-lookup"><span data-stu-id="54372-164">Using this type in APIs like `string.Format` can eliminate the boxing overhead in the majority of cases.</span></span>

<span data-ttu-id="54372-165">Este tipo no es necesariamente especial en el lenguaje.</span><span class="sxs-lookup"><span data-stu-id="54372-165">This type itself is not necessarily special to the language.</span></span> <span data-ttu-id="54372-166">No obstante, se introduce en este documento por separado, ya que se trata de un detalle de implementación de otras partes de la propuesta.</span><span class="sxs-lookup"><span data-stu-id="54372-166">It is being introduced in this document separately though as it becomes an implementation detail of other parts of the proposal.</span></span> 

### <a name="efficient-interpolated-strings"></a><span data-ttu-id="54372-167">Cadenas interpoladas eficaces</span><span class="sxs-lookup"><span data-stu-id="54372-167">Efficient interpolated strings</span></span>
<span data-ttu-id="54372-168">Las cadenas interpoladas son una característica popular que todavía no C#es eficaz en.</span><span class="sxs-lookup"><span data-stu-id="54372-168">Interpolated strings are a popular yet inefficient feature in C#.</span></span> <span data-ttu-id="54372-169">La sintaxis más común, utilizando un `string` interpolado como `string`, se convierte en una llamada a `string.Format(string, params object[])`.</span><span class="sxs-lookup"><span data-stu-id="54372-169">The most common syntax, using an interpolated `string` as a `string`, translates into a `string.Format(string, params object[])` call.</span></span> <span data-ttu-id="54372-170">Esto incurrirá en las asignaciones de conversión boxing para todos los tipos de valor, `string` las asignaciones intermedias a medida que la implementación usa en gran medida `object.ToString` para el formato, así como las asignaciones de matriz una vez que el número de argumentos supera la cantidad de parámetros de las sobrecargas "rápidas" de `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="54372-170">That will incur boxing allocations for all value types, intermediate `string` allocations as the implementation largely uses `object.ToString` for formatting as well as array allocations once the number of arguments exceeds the amount of parameters on the "fast" overloads of `string.Format`.</span></span> 

<span data-ttu-id="54372-171">El idioma cambiará su interpolación disminuyendo para considerar sobrecargas alternativas de `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="54372-171">The language will change its interpolation lowering to consider alternate overloads of `string.Format`.</span></span> <span data-ttu-id="54372-172">Tendrá en cuenta todas las formas de `string.Format(string, params)` y elegirá la sobrecarga "Best" que satisfaga los tipos de argumento.</span><span class="sxs-lookup"><span data-stu-id="54372-172">It will consider all forms of `string.Format(string, params)` and pick the "best" overload which satisfies the argument types.</span></span>
<span data-ttu-id="54372-173">La "mejor" `params` sobrecarga se determinará según las reglas descritas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="54372-173">The "best" `params` overload will be determined by the rules discussed above.</span></span> <span data-ttu-id="54372-174">Esto significa que los `string` interpolados ahora se pueden enlazar a sobrecargas muy eficaces como `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span><span class="sxs-lookup"><span data-stu-id="54372-174">This means interpolated `string` can now bind to very efficient overloads like `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span></span> <span data-ttu-id="54372-175">En muchos casos, se quitarán todas las asignaciones intermedias.</span><span class="sxs-lookup"><span data-stu-id="54372-175">In many cases this will remove all intermediate allocations.</span></span>

### <a name="customizable-interpolated-strings"></a><span data-ttu-id="54372-176">Cadenas interpoladas personalizables</span><span class="sxs-lookup"><span data-stu-id="54372-176">Customizable interpolated strings</span></span>
<span data-ttu-id="54372-177">Los desarrolladores pueden personalizar el comportamiento de las cadenas interpoladas con `FormattableString`.</span><span class="sxs-lookup"><span data-stu-id="54372-177">Developers are able to customize the behavior of interpolated strings with `FormattableString`.</span></span> <span data-ttu-id="54372-178">Contiene los datos que entran en una cadena interpolada: el formato `string` y los argumentos como una matriz.</span><span class="sxs-lookup"><span data-stu-id="54372-178">This contains the data which goes into an interpolated string: the format `string` and the arguments as an array.</span></span> <span data-ttu-id="54372-179">Aunque todavía tiene la conversión boxing y la asignación de la matriz de argumentos, así como la asignación para `FormattableString` (es un `abstract class`).</span><span class="sxs-lookup"><span data-stu-id="54372-179">This though still has the boxing and argument array allocation as well as the allocation for `FormattableString` (it's an `abstract class`).</span></span> <span data-ttu-id="54372-180">Por lo tanto, es de poco uso en las aplicaciones que se asignan con gran cantidad de `string` formato.</span><span class="sxs-lookup"><span data-stu-id="54372-180">Hence it's of little use to applications which are allocation heavy in `string` formatting.</span></span>

<span data-ttu-id="54372-181">Para que el formato de cadena interpolada sea eficaz, el lenguaje reconocerá un nuevo tipo: `System.ValueFormattableString`.</span><span class="sxs-lookup"><span data-stu-id="54372-181">To make interpolated string formatting efficient the language will recognize a new type: `System.ValueFormattableString`.</span></span> <span data-ttu-id="54372-182">Todas las cadenas interpoladas tendrán una conversión de tipo de destino a este tipo.</span><span class="sxs-lookup"><span data-stu-id="54372-182">All interpolated strings will have a target type conversion to this type.</span></span> <span data-ttu-id="54372-183">Esto se implementará mediante la conversión de la cadena interpolada en la llamada `ValueFormattableString.Create` tal como se hace en `FormattableString.Create` hoy.</span><span class="sxs-lookup"><span data-stu-id="54372-183">This will be implemented by translating the interpolated string into the call `ValueFormattableString.Create` exactly as is done for `FormattableString.Create` today.</span></span> <span data-ttu-id="54372-184">El lenguaje admitirá todas las opciones de `params` descritas en este documento al buscar el método de `ValueFormattableString.Create` más adecuado.</span><span class="sxs-lookup"><span data-stu-id="54372-184">The language will support all `params` options described in this document when looking for the most suitable `ValueFormattableString.Create` method.</span></span> 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

<span data-ttu-id="54372-185">Las reglas de resolución de sobrecarga se cambiarán para preferir `ValueFormattableString` sobre `string` cuando el argumento sea una cadena interpolada.</span><span class="sxs-lookup"><span data-stu-id="54372-185">Overload resolution rules will be changed to prefer `ValueFormattableString` over `string` when the argument is an interpolated string.</span></span> <span data-ttu-id="54372-186">Esto significa que será útil tener sobrecargas que solo difieran en `string` y `ValueFormattableString`.</span><span class="sxs-lookup"><span data-stu-id="54372-186">This means it will be valuable to have overloads which differ only on `string` and `ValueFormattableString`.</span></span> <span data-ttu-id="54372-187">Hoy en día, este tipo de sobrecarga con `FormattableString` no es valioso, ya que el compilador siempre preferirá la versión `string` (a menos que el desarrollador use una conversión explícita).</span><span class="sxs-lookup"><span data-stu-id="54372-187">Such an overload today with `FormattableString` is not valuable as the compiler will always prefer the `string` version (unless the developer uses an explicit cast).</span></span> 

## <a name="open-issues"></a><span data-ttu-id="54372-188">Problemas abiertos</span><span class="sxs-lookup"><span data-stu-id="54372-188">Open Issues</span></span>

### <a name="valueformattablestring-breaking-change"></a><span data-ttu-id="54372-189">ValueFormattableString cambio de interrupción</span><span class="sxs-lookup"><span data-stu-id="54372-189">ValueFormattableString breaking change</span></span>
<span data-ttu-id="54372-190">El cambio que prefiere `ValueFormattableString` durante la resolución de sobrecargas en `string` es un cambio importante.</span><span class="sxs-lookup"><span data-stu-id="54372-190">The change to prefer `ValueFormattableString` during overload resolution over `string` is a breaking change.</span></span> <span data-ttu-id="54372-191">Es posible que un desarrollador haya definido un tipo llamado `ValueFormattableString` hoy y usarlo en sobrecargas de método con `string`.</span><span class="sxs-lookup"><span data-stu-id="54372-191">It is possible for a developer to have defined a type called `ValueFormattableString` today and use it in method overloads with `string`.</span></span> <span data-ttu-id="54372-192">Este cambio propuesto haría que el compilador seleccionara una sobrecarga diferente una vez implementado este conjunto de características.</span><span class="sxs-lookup"><span data-stu-id="54372-192">This proposed change would cause the compiler to pick a different overload once this set of features was implemented.</span></span> 

<span data-ttu-id="54372-193">La posibilidad de esto parece razonablemente baja.</span><span class="sxs-lookup"><span data-stu-id="54372-193">The possibility of this seems reasonably low.</span></span> <span data-ttu-id="54372-194">El tipo necesitaría el nombre completo `System.ValueFormattableString` y tendría que tener `static` métodos denominados `Create`.</span><span class="sxs-lookup"><span data-stu-id="54372-194">The type would need the full name `System.ValueFormattableString` and it would need to have `static` methods named `Create`.</span></span> <span data-ttu-id="54372-195">Dado que se recomienda encarecidamente que los desarrolladores puedan definir cualquier tipo en el espacio de nombres `System` este salto parece un riesgo razonable.</span><span class="sxs-lookup"><span data-stu-id="54372-195">Given that developers are strongly discouraged from defining any type in the `System` namespace this break seems like a reasonable compromise.</span></span>

### <a name="expanding-to-more-types"></a><span data-ttu-id="54372-196">Expandir hasta más tipos</span><span class="sxs-lookup"><span data-stu-id="54372-196">Expanding to more types</span></span>
<span data-ttu-id="54372-197">Dado que estamos en el área, deberíamos considerar agregar `IList<T>`, `ICollection<T>` y `IReadOnlyList<T>` al conjunto de colecciones para el que se admite `params`.</span><span class="sxs-lookup"><span data-stu-id="54372-197">Given we're in the area we should consider adding `IList<T>`, `ICollection<T>` and `IReadOnlyList<T>` to the set of collections for which `params` is supported.</span></span> <span data-ttu-id="54372-198">En lo que respecta a la implementación, el costo es una pequeña cantidad sobre el otro trabajo aquí.</span><span class="sxs-lookup"><span data-stu-id="54372-198">In terms of implementation it will cost a small amount over the other work here.</span></span>

<span data-ttu-id="54372-199">Sin embargo, LDM debe decidir si merece la pena para el lenguaje.</span><span class="sxs-lookup"><span data-stu-id="54372-199">LDM needs to decide if the complication to the language is worth it though.</span></span> <span data-ttu-id="54372-200">La adición de `IEnumerable<T>` quita un punto de fricción muy específico.</span><span class="sxs-lookup"><span data-stu-id="54372-200">The addition of `IEnumerable<T>` removes a very specific friction point.</span></span> <span data-ttu-id="54372-201">Si falta esta solución `params` muchos clientes tenían que asignar `T[]` de un `IEnumerable<T>` al llamar a un método de `params`.</span><span class="sxs-lookup"><span data-stu-id="54372-201">Lacking this `params` solution many customers were forced to allocate `T[]` from an `IEnumerable<T>` when calling a `params` method.</span></span> <span data-ttu-id="54372-202">No obstante, la adición de `IEnumerable<T>` se corrige.</span><span class="sxs-lookup"><span data-stu-id="54372-202">The addition of `IEnumerable<T>` fixes this though.</span></span> <span data-ttu-id="54372-203">No hay ningún punto de fricción específico que las otras interfaces corrijan aquí.</span><span class="sxs-lookup"><span data-stu-id="54372-203">There is no specific friction point that the other interfaces fix here.</span></span> 

## <a name="considerations"></a><span data-ttu-id="54372-204">Consideraciones</span><span class="sxs-lookup"><span data-stu-id="54372-204">Considerations</span></span>

### <a name="variant2-and-variant3"></a><span data-ttu-id="54372-205">Variant2 y Variant3</span><span class="sxs-lookup"><span data-stu-id="54372-205">Variant2 and Variant3</span></span>
<span data-ttu-id="54372-206">El equipo de CoreFX también tiene un conjunto de tipos de almacenamiento sin asignación para un máximo de tres argumentos de `Variant`.</span><span class="sxs-lookup"><span data-stu-id="54372-206">The CoreFX team also has a non-allocating set of storage types for up to three `Variant` arguments.</span></span> <span data-ttu-id="54372-207">Se trata de una única `Variant`, `Variant2` y `Variant3`.</span><span class="sxs-lookup"><span data-stu-id="54372-207">These are a single `Variant`, `Variant2` and `Variant3`.</span></span> <span data-ttu-id="54372-208">Todos tienen un par de métodos para obtener una asignación gratuita `Span<Variant>` de ellas: `CreateSpan` y `KeepAlive`.</span><span class="sxs-lookup"><span data-stu-id="54372-208">All have a pair of methods for getting an allocation free `Span<Variant>` off of them: `CreateSpan` and `KeepAlive`.</span></span> <span data-ttu-id="54372-209">Esto significa que para una `params Span<Variant>` de hasta tres argumentos, el sitio de la llamada puede ser totalmente gratuito.</span><span class="sxs-lookup"><span data-stu-id="54372-209">This means for a `params Span<Variant>` of up to three arguments the call site can be entirely allocation free.</span></span>

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="54372-210">El método `Go` se puede reducir a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="54372-210">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

<span data-ttu-id="54372-211">Esto requiere muy poco trabajo en la parte superior de la propuesta para volver a usar `T[]` entre llamadas a `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="54372-211">This requires very little work on top of the proposal to re-use `T[]` between `params Span<T>` calls.</span></span> <span data-ttu-id="54372-212">El compilador ya necesita administrar un temporal por llamada y realizar una limpieza de trabajo después de (aunque en un caso es simplemente marcar una Temp interna como disponible).</span><span class="sxs-lookup"><span data-stu-id="54372-212">The compiler already needs to manage a temporary per call and do clean up work after (even if in one case it's just marking an internal temp as free).</span></span> 

<span data-ttu-id="54372-213">Nota: la función `KeepAlive` solo es necesaria en el escritorio.</span><span class="sxs-lookup"><span data-stu-id="54372-213">Note: the `KeepAlive` function is only necessary on desktop.</span></span> <span data-ttu-id="54372-214">En .NET Core, el método no estará disponible y, por lo tanto, el compilador no emitirá una llamada a él.</span><span class="sxs-lookup"><span data-stu-id="54372-214">On .NET Core the method will not be available and hence the compiler won't emit a call to it.</span></span>

### <a name="clr-stack-allocation-helpers"></a><span data-ttu-id="54372-215">Aplicaciones auxiliares de asignación de pila de CLR</span><span class="sxs-lookup"><span data-stu-id="54372-215">CLR stack allocation helpers</span></span>
<span data-ttu-id="54372-216">CLR solo proporciona solamente [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) para la asignación de la memoria contigua de la pila.</span><span class="sxs-lookup"><span data-stu-id="54372-216">The CLR only provides only [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) for stack allocation of contiguous memory.</span></span> <span data-ttu-id="54372-217">Esta instrucción está limitada, ya que solo funciona para tipos de `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="54372-217">This instruction is limited in that it only works for `unmanaged` types.</span></span> <span data-ttu-id="54372-218">Esto significa que no se puede usar como una solución universal para asignar eficazmente el almacenamiento de copia de seguridad para `params 
 Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="54372-218">This means it can't be used as a universal solution for efficiently allocating the backing storage for `params 
 Span<T>`.</span></span> 

<span data-ttu-id="54372-219">Sin embargo, esta limitación no es una restricción fundamental, sino que, en su lugar, se trata de un artefacto de historial.</span><span class="sxs-lookup"><span data-stu-id="54372-219">This limitation is not some fundamental restriction though but instead more an artifact of history.</span></span> <span data-ttu-id="54372-220">CLR podría optar por agregar nuevos códigos de operación o intrínsecos que proporcionan la asignación de la pila universal.</span><span class="sxs-lookup"><span data-stu-id="54372-220">The CLR could choose to add new op codes / intrinsics which provide universal stack allocation.</span></span> <span data-ttu-id="54372-221">A continuación, se pueden usar para asignar el almacenamiento de respaldo para la mayoría de las llamadas `params Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="54372-221">These could then be used to allocate the backing storage for most `params Span<T>` calls.</span></span>

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

<span data-ttu-id="54372-222">El método `Go` se puede reducir a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="54372-222">The `Go` method can be lowered to the following:</span></span>

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

<span data-ttu-id="54372-223">Aunque este enfoque es muy eficaz para el montón, produce un uso adicional de la pila.</span><span class="sxs-lookup"><span data-stu-id="54372-223">While this approach is very heap efficient it does cause extra stack usage.</span></span> <span data-ttu-id="54372-224">En un algoritmo que tiene una pila profunda y una gran cantidad de `params` uso, es posible que se genere un `StackOverflowException` en el que una sencilla asignación de `T[]` se realizara correctamente.</span><span class="sxs-lookup"><span data-stu-id="54372-224">In an algorithm which has a deep stack and lots of `params` usage it's possible this could cause a `StackOverflowException` to be generated where a simple `T[]` allocation would succeed.</span></span> 

<span data-ttu-id="54372-225">Desafortunadamente C# , no está configurado para el tipo de análisis entre métodos, en el que podría tomar una determinación fundada de si la llamada debe usar la asignación de la pila o el montón de `params`.</span><span class="sxs-lookup"><span data-stu-id="54372-225">Unfortunately C# is not set up for the type of inter-method analysis where it could make an educated determination of whether or not call should use stack or heap allocation of `params`.</span></span> <span data-ttu-id="54372-226">En realidad, solo puede considerar cada llamada por sí misma.</span><span class="sxs-lookup"><span data-stu-id="54372-226">It can only really consider each call on its own.</span></span>

<span data-ttu-id="54372-227">CLR es la mejor configuración para crear este tipo de determinación en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="54372-227">The CLR is best setup for making this type of determination at runtime.</span></span> <span data-ttu-id="54372-228">Por lo tanto, es probable que el tiempo de ejecución proporcione dos métodos para la asignación de la pila universal:</span><span class="sxs-lookup"><span data-stu-id="54372-228">Hence we'd likely have the runtime provide two methods for universal stack allocation:</span></span>

1. <span data-ttu-id="54372-229">`Span<T> StackAlloc<T>(int length)`: esto tiene los mismos comportamientos y limitaciones de `localloc`, salvo que puede funcionar en cualquier tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="54372-229">`Span<T> StackAlloc<T>(int length)`: this has the same behaviors and limitations of `localloc` except it can work on any type `T`.</span></span> 
1. <span data-ttu-id="54372-230">`Span<T> MaybeStackAlloc<T>(int length)`: este Runtime puede optar por implementar esto mediante una pila o una asignación de montón.</span><span class="sxs-lookup"><span data-stu-id="54372-230">`Span<T> MaybeStackAlloc<T>(int length)`: this runtime can choose to implement this by doing a stack or heap allocation.</span></span> <span data-ttu-id="54372-231">Después, el tiempo de ejecución puede utilizar el contexto de ejecución en el que se llama para determinar cómo se asigna el `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="54372-231">The runtime can then use the execution context in which it's called to determine how the `Span<T>` is allocated.</span></span> <span data-ttu-id="54372-232">Sin embargo, el autor de la llamada siempre lo tratará como si estuviera asignada a la pila.</span><span class="sxs-lookup"><span data-stu-id="54372-232">The caller though will always treat it as if it were stack allocated.</span></span>

<span data-ttu-id="54372-233">En casos muy sencillos, como uno o dos argumentos, C# el compilador siempre podría usar `StackAlloc<T>` Variant.</span><span class="sxs-lookup"><span data-stu-id="54372-233">For very simple cases, like one to two arguments, the C# compiler could always use `StackAlloc<T>` variant.</span></span> <span data-ttu-id="54372-234">No es probable que contribuya significativamente al agotamiento de la pila en la mayoría de los casos.</span><span class="sxs-lookup"><span data-stu-id="54372-234">This is unlikely to significantly contribute to stack exhaustion in most cases.</span></span> <span data-ttu-id="54372-235">En otros casos, el compilador podría optar por usar `MaybeStackAlloc<T>` en su lugar y dejar que el Runtime realice la llamada.</span><span class="sxs-lookup"><span data-stu-id="54372-235">For other cases the compiler could choose to use `MaybeStackAlloc<T>` instead and let the runtime make the call.</span></span>

<span data-ttu-id="54372-236">La forma de elegir probablemente requerirá una investigación y un examen más profundos de las aplicaciones del mundo real.</span><span class="sxs-lookup"><span data-stu-id="54372-236">How we choose will likely require a deeper investigation and examination of real world apps.</span></span> <span data-ttu-id="54372-237">Pero si estas nuevas funciones intrínsecas están disponibles, nos proporcionará este tipo de flexibilidad.</span><span class="sxs-lookup"><span data-stu-id="54372-237">But if these new intrinsics are available then it will give us this type of flexibility.</span></span>

### <a name="why-not-varargs"></a><span data-ttu-id="54372-238">¿Por qué no varargs?</span><span class="sxs-lookup"><span data-stu-id="54372-238">Why not varargs?</span></span> 
<span data-ttu-id="54372-239">La característica [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) existente se consideró aquí como una posible solución.</span><span class="sxs-lookup"><span data-stu-id="54372-239">The existing [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) feature was considered here as a possible solution.</span></span> <span data-ttu-id="54372-240">Aunque esta característica está pensada principalmente C++para escenarios de/CLI y tiene lagunas conocidas para otros escenarios.</span><span class="sxs-lookup"><span data-stu-id="54372-240">This feature though is meant primarily for C++/CLI scenarios and has known holes for other scenarios.</span></span> <span data-ttu-id="54372-241">Además, hay un costo significativo en la migración de esto a Unix.</span><span class="sxs-lookup"><span data-stu-id="54372-241">Additionally there is significant cost in porting this to Unix.</span></span> <span data-ttu-id="54372-242">Por lo tanto, no se considera una solución viable.</span><span class="sxs-lookup"><span data-stu-id="54372-242">Hence it wasn't seen as a viable solution.</span></span>

## <a name="related-issues"></a><span data-ttu-id="54372-243">Problemas relacionados</span><span class="sxs-lookup"><span data-stu-id="54372-243">Related Issues</span></span>
<span data-ttu-id="54372-244">Esta especificación está relacionada con los siguientes problemas:</span><span class="sxs-lookup"><span data-stu-id="54372-244">This spec is related to the following issues:</span></span> 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

