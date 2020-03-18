---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484151"
---
# <a name="async-streams"></a><span data-ttu-id="fcc3d-101">Secuencias asincrónicas</span><span class="sxs-lookup"><span data-stu-id="fcc3d-101">Async Streams</span></span>

* <span data-ttu-id="fcc3d-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="fcc3d-102">[x] Proposed</span></span>
* <span data-ttu-id="fcc3d-103">[x] prototipo</span><span class="sxs-lookup"><span data-stu-id="fcc3d-103">[x] Prototype</span></span>
* <span data-ttu-id="fcc3d-104">[] Implementación</span><span class="sxs-lookup"><span data-stu-id="fcc3d-104">[ ] Implementation</span></span>
* <span data-ttu-id="fcc3d-105">[] Especificación</span><span class="sxs-lookup"><span data-stu-id="fcc3d-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="fcc3d-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="fcc3d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="fcc3d-107">C#admite métodos de iterador y métodos asincrónicos, pero no admite un método que sea un iterador y un método asincrónico.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-107">C# has support for iterator methods and async methods, but no support for a method that is both an iterator and an async method.</span></span>  <span data-ttu-id="fcc3d-108">Debemos rectificar esto permitiendo que `await` se use en una nueva forma de `async` iterador, uno que devuelva un `IAsyncEnumerable<T>` o `IAsyncEnumerator<T>` en lugar de un `IEnumerable<T>` o `IEnumerator<T>`, con `IAsyncEnumerable<T>` consumible en una nueva `await foreach`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-108">We should rectify this by allowing for `await` to be used in a new form of `async` iterator, one that returns an `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` rather than an `IEnumerable<T>` or `IEnumerator<T>`, with `IAsyncEnumerable<T>` consumable in a new `await foreach`.</span></span>  <span data-ttu-id="fcc3d-109">También se usa una interfaz de `IAsyncDisposable` para habilitar la limpieza asincrónica.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-109">An `IAsyncDisposable` interface is also used to enable asynchronous cleanup.</span></span>

## <a name="related-discussion"></a><span data-ttu-id="fcc3d-110">Discusión relacionada</span><span class="sxs-lookup"><span data-stu-id="fcc3d-110">Related discussion</span></span>
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a><span data-ttu-id="fcc3d-111">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="fcc3d-111">Detailed design</span></span>
[design]: #detailed-design

## <a name="interfaces"></a><span data-ttu-id="fcc3d-112">Interfaces</span><span class="sxs-lookup"><span data-stu-id="fcc3d-112">Interfaces</span></span>

### <a name="iasyncdisposable"></a><span data-ttu-id="fcc3d-113">IAsyncDisposable</span><span class="sxs-lookup"><span data-stu-id="fcc3d-113">IAsyncDisposable</span></span>

<span data-ttu-id="fcc3d-114">Ha habido mucho análisis de `IAsyncDisposable` (por ejemplo, https://github.com/dotnet/roslyn/issues/114) y si es una buena idea.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-114">There has been much discussion of `IAsyncDisposable` (e.g. https://github.com/dotnet/roslyn/issues/114) and whether it's a good idea.</span></span>  <span data-ttu-id="fcc3d-115">Sin embargo, es un concepto necesario para agregar compatibilidad con iteradores asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-115">However, it's a required concept to add in support of async iterators.</span></span>  <span data-ttu-id="fcc3d-116">Como los bloques de `finally` pueden contener `await`s y, como los bloques de `finally` deben ejecutarse como parte de la eliminación de iteradores, se necesita la eliminación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-116">Since `finally` blocks may contain `await`s, and since `finally` blocks need to be run as part of disposing of iterators, we need async disposal.</span></span>  <span data-ttu-id="fcc3d-117">También es muy útil en general cuando la limpieza de recursos puede tardar cualquier período de tiempo, por ejemplo, cerrar los archivos (que requieren vaciados), anular el registro de las devoluciones de llamada y proporcionar una manera de saber cuándo se ha completado el registro, etc.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-117">It's also just generally useful any time cleaning up of resources might take any period of time, e.g. closing files (requiring flushes), deregistering callbacks and providing a way to know when deregistration has completed, etc.</span></span>

<span data-ttu-id="fcc3d-118">La siguiente interfaz se agrega a las bibliotecas básicas de .NET (por ejemplo, System. Private. CoreLib/System. Runtime):</span><span class="sxs-lookup"><span data-stu-id="fcc3d-118">The following interface is added to the core .NET libraries (e.g. System.Private.CoreLib / System.Runtime):</span></span>
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
<span data-ttu-id="fcc3d-119">Al igual que con `Dispose`, la invocación de `DisposeAsync` varias veces es aceptable, y las invocaciones subsiguientes después de la primera se deben tratar como Nops, devolviendo una tarea completada correctamente completada (`DisposeAsync` no necesita ser segura para subprocesos, sin embargo, y no es necesario que admitan la invocación simultánea).</span><span class="sxs-lookup"><span data-stu-id="fcc3d-119">As with `Dispose`, invoking `DisposeAsync` multiple times is acceptable, and subsequent invocations after the first should be treated as nops, returning a synchronously completed successful task (`DisposeAsync` need not be thread-safe, though, and need not support concurrent invocation).</span></span>  <span data-ttu-id="fcc3d-120">Además, los tipos pueden implementar `IDisposable` y `IAsyncDisposable`y, si lo hacen, es igualmente aceptable invocar `Dispose` y, a continuación, `DisposeAsync` o viceversa, pero solo el primero debe ser significativo y las invocaciones subsiguientes de deben ser una NOP.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-120">Further, types may implement both `IDisposable` and `IAsyncDisposable`, and if they do, it's similarly acceptable to invoke `Dispose` and then `DisposeAsync` or vice versa, but only the first should be meaningful and subsequent invocations of either should be a nop.</span></span>  <span data-ttu-id="fcc3d-121">Como tal, si un tipo implementa ambos, se anima a los consumidores a llamar una vez y solo una vez al método más relevante basado en el contexto, `Dispose` en contextos sincrónicos y `DisposeAsync` en los asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-121">As such, if a type does implement both, consumers are encouraged to call once and only once the more relevant method based on the context, `Dispose` in synchronous contexts and `DisposeAsync` in asynchronous ones.</span></span>

<span data-ttu-id="fcc3d-122">(Estoy saliendo del modo en que `IAsyncDisposable` interactúa con `using` a una discusión independiente.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-122">(I'm leaving discussion of how `IAsyncDisposable` interacts with `using` to a separate discussion.</span></span>  <span data-ttu-id="fcc3d-123">Y la cobertura de cómo interactúa con `foreach` se trata más adelante en esta propuesta).</span><span class="sxs-lookup"><span data-stu-id="fcc3d-123">And coverage of how it interacts with `foreach` is handled later in this proposal.)</span></span>

<span data-ttu-id="fcc3d-124">Alternativas consideradas:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-124">Alternatives considered:</span></span>
- <span data-ttu-id="fcc3d-125">_`DisposeAsync` aceptar un `CancellationToken`_ : aunque en teoría tiene sentido que se puede cancelar cualquier elemento asincrónico, la eliminación es sobre la limpieza, el cierre de las cosas, los recursos de free'ing, etc., que normalmente no es algo que se debe cancelar. la limpieza todavía es importante para el trabajo que se cancela.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-125">_`DisposeAsync` accepting a `CancellationToken`_: while in theory it makes sense that anything async can be canceled, disposal is about cleanup, closing things out, free'ing resources, etc., which is generally not something that should be canceled; cleanup is still important for work that's canceled.</span></span>  <span data-ttu-id="fcc3d-126">El mismo `CancellationToken` que causó el trabajo real que se va a cancelar sería el mismo token que se pasó a `DisposeAsync`, por lo que no `DisposeAsync` merecente porque la cancelación del trabajo haría que `DisposeAsync` fuera una instrucción NOP.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-126">The same `CancellationToken` that caused the actual work to be canceled would typically be the same token passed to `DisposeAsync`, making `DisposeAsync` worthless because cancellation of the work would cause `DisposeAsync` to be a nop.</span></span>  <span data-ttu-id="fcc3d-127">Si alguien desea evitar que se bloquee a la espera de la eliminación, puede evitar la espera en el `ValueTask`resultante o esperar solo durante un período de tiempo.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-127">If someone wants to avoid being blocked waiting for disposal, they can avoid waiting on the resulting `ValueTask`, or wait on it only for some period of time.</span></span>
- <span data-ttu-id="fcc3d-128">_`DisposeAsync` devolver una `Task`_ : ahora que existe un `ValueTask` no genérico y se puede construir a partir de una `IValueTaskSource`, al devolver `ValueTask` de `DisposeAsync` se permite que un objeto existente se reutilice como promesa que representa la finalización asincrónica eventual de `DisposeAsync`, guardando una asignación `Task` en caso de que `DisposeAsync` se complete de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-128">_`DisposeAsync` returning a `Task`_: Now that a non-generic `ValueTask` exists and can be constructed from an `IValueTaskSource`, returning `ValueTask` from `DisposeAsync` allows an existing object to be reused as the promise representing the eventual async completion of `DisposeAsync`, saving a `Task` allocation in the case where `DisposeAsync` completes asynchronously.</span></span>
- <span data-ttu-id="fcc3d-129">_Configuración de `DisposeAsync` con una `bool continueOnCapturedContext` (`ConfigureAwait`)_ : aunque puede haber problemas relacionados con el modo en que este concepto se expone a `using`, `foreach`y otras construcciones de lenguaje que consumen esto, desde una perspectiva de interfaz, no está realizando realmente ninguna `await`y no hay nada que configurar... los consumidores del `ValueTask` pueden utilizarlo sin embargo.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-129">_Configuring `DisposeAsync` with a `bool continueOnCapturedContext` (`ConfigureAwait`)_: While there may be issues related to how such a concept is exposed to `using`, `foreach`, and other language constructs that consume this, from an interface perspective it's not actually doing any `await`'ing and there's nothing to configure... consumers of the `ValueTask` can consume it however they wish.</span></span>
- <span data-ttu-id="fcc3d-130">_`IAsyncDisposable` heredar `IDisposable`_ : dado que solo se debe usar una u otra, no tiene sentido forzar los tipos para implementar ambos.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-130">_`IAsyncDisposable` inheriting `IDisposable`_:  Since only one or the other should be used, it doesn't make sense to force types to implement both.</span></span>
- <span data-ttu-id="fcc3d-131">_`IDisposableAsync` en lugar de `IAsyncDisposable`_ : hemos seguido el nombre de los elementos y tipos que son "Async algo", mientras que las operaciones son "Async Async", por lo que los tipos tienen "Async" como prefijo y los métodos tienen "Async" como sufijo.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-131">_`IDisposableAsync` instead of `IAsyncDisposable`_: We've been following the naming that things/types are an "async something" whereas operations are "done async", so types have "Async" as a prefix and methods have "Async" as a suffix.</span></span>

### <a name="iasyncenumerable--iasyncenumerator"></a><span data-ttu-id="fcc3d-132">IAsyncEnumerable / IAsyncEnumerator</span><span class="sxs-lookup"><span data-stu-id="fcc3d-132">IAsyncEnumerable / IAsyncEnumerator</span></span>

<span data-ttu-id="fcc3d-133">Se agregan dos interfaces a las bibliotecas .NET básicas:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-133">Two interfaces are added to the core .NET libraries:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken cancellationToken = default);
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> MoveNextAsync();
        T Current { get; }
    }
}
```

<span data-ttu-id="fcc3d-134">El consumo típico (sin características adicionales del lenguaje) sería similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-134">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
        Use(enumerator.Current);
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="fcc3d-135">Opciones descartadas que se consideran:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-135">Discarded options considered:</span></span>
- <span data-ttu-id="fcc3d-136">_`Task<bool> MoveNextAsync(); T current { get; }`_ : el uso de `Task<bool>` admitiría el uso de un objeto de tarea almacenado en memoria caché para representar llamadas sincrónicas `MoveNextAsync` correctas, pero todavía se requeriría una asignación para la finalización asincrónica.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-136">_`Task<bool> MoveNextAsync(); T current { get; }`_: Using `Task<bool>` would support using a cached task object to represent synchronous, successful `MoveNextAsync` calls, but an allocation would still be required for asynchronous completion.</span></span>  <span data-ttu-id="fcc3d-137">Al devolver `ValueTask<bool>`, se habilita el objeto de enumerador para que se implemente `IValueTaskSource<bool>` y se use como respaldo para el `ValueTask<bool>` devuelto de `MoveNextAsync`, lo que a su vez permite una sobrecargas significativamente reducidas.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-137">By returning `ValueTask<bool>`, we enable the enumerator object to itself implement `IValueTaskSource<bool>` and be used as the backing for the `ValueTask<bool>` returned from `MoveNextAsync`, which in turn allows for significantly reduced overheads.</span></span>
- <span data-ttu-id="fcc3d-138">_`ValueTask<(bool, T)> MoveNextAsync();`_ : no solo es más difícil de consumir, pero significa que `T` ya no puede ser covariante.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-138">_`ValueTask<(bool, T)> MoveNextAsync();`_: It's not only harder to consume, but it means that `T` can no longer be covariant.</span></span>
- <span data-ttu-id="fcc3d-139">_`ValueTask<T?> TryMoveNextAsync();`_ : no covariante.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-139">_`ValueTask<T?> TryMoveNextAsync();`_: Not covariant.</span></span>
- <span data-ttu-id="fcc3d-140">_`Task<T?> TryMoveNextAsync();`_ : no covariantes, asignaciones en cada llamada, etc.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-140">_`Task<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="fcc3d-141">_`ITask<T?> TryMoveNextAsync();`_ : no covariantes, asignaciones en cada llamada, etc.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-141">_`ITask<T?> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="fcc3d-142">_`ITask<(bool,T)> TryMoveNextAsync();`_ : no covariantes, asignaciones en cada llamada, etc.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-142">_`ITask<(bool,T)> TryMoveNextAsync();`_: Not covariant, allocations on every call, etc.</span></span>
- <span data-ttu-id="fcc3d-143">_`Task<bool> TryMoveNextAsync(out T result);`_ : el resultado de la `out` debe establecerse cuando la operación se devuelve sincrónicamente, no cuando completa de forma asincrónica la tarea potencialmente en el futuro, momento en el que no había forma de comunicar el resultado.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-143">_`Task<bool> TryMoveNextAsync(out T result);`_: The `out` result would need to be set when the operation returns synchronously, not when it asynchronously completes the task potentially sometime long in the future, at which point there'd be no way to communicate the result.</span></span>
- <span data-ttu-id="fcc3d-144">_`IAsyncEnumerator<T>` no implementar `IAsyncDisposable`_ : podríamos optar por separarlas.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-144">_`IAsyncEnumerator<T>` not implementing `IAsyncDisposable`_: We could choose to separate these.</span></span>  <span data-ttu-id="fcc3d-145">Sin embargo, Esto complica algunas otras áreas de la propuesta, ya que el código debe ser capaz de tratar con la posibilidad de que un enumerador no proporcione la eliminación, lo que dificulta la escritura de aplicaciones auxiliares basadas en patrones.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-145">However, doing so complicates certain other areas of the proposal, as code must then be able to deal with the possibility that an enumerator doesn't provide disposal, which makes it difficult to write pattern-based helpers.</span></span>  <span data-ttu-id="fcc3d-146">Además, es habitual que los enumeradores tengan una necesidad de eliminación (por ejemplo, cualquier iterador asincrónico que tenga un bloque Finally, la mayoría de las C# cosas que enumeran datos de una conexión de red, etc.) y, si no es así, es sencillo implementar el método únicamente como `public ValueTask DisposeAsync() => default(ValueTask);` con una mínima sobrecarga adicional.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-146">Further, it will be common for enumerators to have a need for disposal (e.g. any C# async iterator that has a finally block, most things enumerating data from a network connection, etc.), and if one doesn't, it is simple to implement the method purely as `public ValueTask DisposeAsync() => default(ValueTask);` with minimal additional overhead.</span></span>
- <span data-ttu-id="fcc3d-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: no hay ningún parámetro de token de cancelación.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-147">_ `IAsyncEnumerator<T> GetAsyncEnumerator()`: No cancellation token parameter.</span></span>

#### <a name="viable-alternative"></a><span data-ttu-id="fcc3d-148">Alternativa viable:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-148">Viable alternative:</span></span>

```csharp
namespace System.Collections.Generic
{
    public interface IAsyncEnumerable<out T>
    {
        IAsyncEnumerator<T> GetAsyncEnumerator();
    }

    public interface IAsyncEnumerator<out T> : IAsyncDisposable
    {
        ValueTask<bool> WaitForNextAsync();
        T TryGetNext(out bool success);
    }
}
```

<span data-ttu-id="fcc3d-149">`TryGetNext` se usa en un bucle interno para consumir elementos con una única llamada de interfaz, siempre y cuando estén disponibles de forma sincrónica.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-149">`TryGetNext` is used in an inner loop to consume items with a single interface call as long as they're available synchronously.</span></span>  <span data-ttu-id="fcc3d-150">Cuando el siguiente elemento no se puede recuperar sincrónicamente, devuelve false y cada vez que devuelve false, un llamador debe invocar posteriormente `WaitForNextAsync` para esperar a que el siguiente elemento esté disponible o para determinar que nunca habrá otro elemento.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-150">When the next item can't be retrieved synchronously, it returns false, and any time it returns false, a caller must subsequently invoke `WaitForNextAsync` to either wait for the next item to be available or to determine that there will never be another item.</span></span> <span data-ttu-id="fcc3d-151">El consumo típico (sin características adicionales del lenguaje) sería similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-151">Typical consumption (without additional language features) would look like:</span></span>

```csharp
IAsyncEnumerator<T> enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.WaitForNextAsync())
    {
        while (true)
        {
            int item = enumerator.TryGetNext(out bool success);
            if (!success) break;
            Use(item);
        }
    }
}
finally { await enumerator.DisposeAsync(); }
```

<span data-ttu-id="fcc3d-152">La ventaja de esto es dos veces, una secundaria y una principal:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-152">The advantage of this is two-fold, one minor and one major:</span></span>
- <span data-ttu-id="fcc3d-153">_Minor: permite que un enumerador admita varios consumidores_.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-153">_Minor: Allows for an enumerator to support multiple consumers_.</span></span> <span data-ttu-id="fcc3d-154">Puede haber escenarios en los que sea valioso que un enumerador admita varios consumidores simultáneos.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-154">There may be scenarios where it's valuable for an enumerator to support multiple concurrent consumers.</span></span>  <span data-ttu-id="fcc3d-155">Esto no se puede lograr cuando `MoveNextAsync` y `Current` son independientes para que una implementación no pueda hacer que su uso sea atómico.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-155">That can't be achieved when `MoveNextAsync` and `Current` are separate such that an implementation can't make their usage atomic.</span></span>  <span data-ttu-id="fcc3d-156">En cambio, este enfoque proporciona un método único `TryGetNext` que admite la inserción del enumerador hacia delante y la obtención del elemento siguiente, por lo que el enumerador puede habilitar la atomicidad si se desea.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-156">In contrast, this approach provides a single method `TryGetNext` that supports pushing the enumerator forward and getting the next item, so the enumerator can enable atomicity if desired.</span></span>  <span data-ttu-id="fcc3d-157">Sin embargo, es probable que estos escenarios también se puedan habilitar si se asigna a cada consumidor su propio enumerador de un Enumerable compartido.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-157">However, it's likely that such scenarios could also be enabled by giving each consumer its own enumerator from a shared enumerable.</span></span>  <span data-ttu-id="fcc3d-158">Además, no queremos exigir que todos los enumeradores admitan el uso simultáneo, ya que esto agregaría sobrecargas no triviales al caso de mayoría que no lo requiera, lo que significa que un consumidor de la interfaz no podía confiar normalmente de este modo.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-158">Further, we don't want to enforce that every enumerator support concurrent usage, as that would add non-trivial overheads to the majority case that doesn't require it, which means a consumer of the interface generally couldn't rely on this any way.</span></span>
- <span data-ttu-id="fcc3d-159">_Principal: rendimiento_.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-159">_Major: Performance_.</span></span> <span data-ttu-id="fcc3d-160">El enfoque de `Current` de /de `MoveNextAsync`requiere dos llamadas de interfaz por operación, mientras que el mejor caso para `WaitForNextAsync`/`TryGetNext` es que la mayoría de las iteraciones se completan sincrónicamente, lo que permite un bucle interno estrecho con `TryGetNext`, de modo que solo tenemos una llamada de interfaz por operación.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-160">The `MoveNextAsync`/`Current` approach requires two interface calls per operation, whereas the best case for `WaitForNextAsync`/`TryGetNext` is that most iterations complete synchronously, enabling a tight inner loop with `TryGetNext`, such that we only have one interface call per operation.</span></span>  <span data-ttu-id="fcc3d-161">Esto puede tener un impacto mensurable en situaciones en las que la interfaz llama a dominar el cálculo.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-161">This can have a measurable impact in situations where the interface calls dominate the computation.</span></span>

<span data-ttu-id="fcc3d-162">Sin embargo, hay desventajas no triviales, incluida la complejidad significativamente mayor cuando se usan de forma manual y una mayor probabilidad de que se presenten errores al usarlas.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-162">However, there are non-trivial downsides, including significantly increased complexity when consuming these manually, and an increased chance of introducing bugs when using them.</span></span>  <span data-ttu-id="fcc3d-163">Mientras que las ventajas de rendimiento se muestran en microbenchmarks, no creemos que se verán afectadas por la inmensa mayoría del uso real.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-163">And while the performance benefits show up in microbenchmarks, we don't believe they'll be impactful in the vast majority of real usage.</span></span>  <span data-ttu-id="fcc3d-164">En caso de que sea así, podemos introducir un segundo conjunto de interfaces de manera clara.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-164">If it turns out they are, we can introduce a second set of interfaces in a light-up fashion.</span></span>

<span data-ttu-id="fcc3d-165">Opciones descartadas que se consideran:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-165">Discarded options considered:</span></span>
- <span data-ttu-id="fcc3d-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: los parámetros de `out` no pueden ser covariantes.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-166">`ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: `out` parameters can't be covariant.</span></span>  <span data-ttu-id="fcc3d-167">También hay un pequeño impacto aquí (un problema con el patrón try en general) que probablemente incurrirá en una barrera de escritura en tiempo de ejecución para los resultados de tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-167">There's also a small impact here (an issue with the try pattern in general) that this likely incurs a runtime write barrier for reference type results.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="fcc3d-168">Cancelación</span><span class="sxs-lookup"><span data-stu-id="fcc3d-168">Cancellation</span></span>

<span data-ttu-id="fcc3d-169">Existen varios enfoques posibles para admitir la cancelación:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-169">There are several possible approaches to supporting cancellation:</span></span>
1. <span data-ttu-id="fcc3d-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` son independientes de la cancelación: `CancellationToken` no aparece en ninguna parte.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-170">`IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` are cancellation-agnostic: `CancellationToken` doesn't appear anywhere.</span></span>  <span data-ttu-id="fcc3d-171">La cancelación se logra mediante la conversión lógica del `CancellationToken` en el enumerador y/o de la manera adecuada, por ejemplo, cuando se llama a un iterador, pasando el `CancellationToken` como argumento al método iterador y usando en el cuerpo del iterador, como se hace con cualquier otro parámetro.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-171">Cancellation is achieved by logically baking the `CancellationToken` into the enumerable and/or enumerator in whatever manner is appropriate, e.g. when calling an iterator, passing the `CancellationToken` as an argument to the iterator method and using it in the body of the iterator, as is done with any other parameter.</span></span>
2. <span data-ttu-id="fcc3d-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: pasa un `CancellationToken` a `GetAsyncEnumerator`y las operaciones de `MoveNextAsync` subsiguientes lo respetan.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-172">`IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: You pass a `CancellationToken` to `GetAsyncEnumerator`, and subsequent `MoveNextAsync` operations respect it however it can.</span></span>
3. <span data-ttu-id="fcc3d-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: se pasa un `CancellationToken` a cada llamada de `MoveNextAsync` individual.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-173">`IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: You pass a `CancellationToken` to each individual `MoveNextAsync` call.</span></span>
4. <span data-ttu-id="fcc3d-174">1 & & 2: incrusta `CancellationToken`s en el enumerador o enumerable y pasar `CancellationToken`s a `GetAsyncEnumerator`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-174">1 && 2: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `GetAsyncEnumerator`.</span></span>
5. <span data-ttu-id="fcc3d-175">1 & & 3: incrusta `CancellationToken`s en el enumerador o enumerable y pasar `CancellationToken`s a `MoveNextAsync`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-175">1 && 3: You both embed `CancellationToken`s into your enumerable/enumerator and pass `CancellationToken`s into `MoveNextAsync`.</span></span>

<span data-ttu-id="fcc3d-176">Desde una perspectiva puramente teórica, (5) es la más sólida, en la que (a) `MoveNextAsync` aceptar un `CancellationToken` permite el control más preciso sobre lo que se cancela y (b) `CancellationToken` es solo cualquier otro tipo que se pueda pasar como argumento en iteradores, incrustados en tipos arbitrarios, etc.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-176">From a purely theoretical perspective, (5) is the most robust, in that (a) `MoveNextAsync` accepting a `CancellationToken` enables the most fine-grained control over what's canceled, and (b) `CancellationToken` is just any other type that can passed as an argument into iterators, embedded in arbitrary types, etc.</span></span>

<span data-ttu-id="fcc3d-177">Sin embargo, hay varios problemas con este enfoque:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-177">However, there are multiple problems with that approach:</span></span>
- <span data-ttu-id="fcc3d-178">¿Cómo se pasa un `CancellationToken` a `GetAsyncEnumerator` convertirlo en el cuerpo del iterador?</span><span class="sxs-lookup"><span data-stu-id="fcc3d-178">How does a `CancellationToken` passed to `GetAsyncEnumerator` make it into the body of the iterator?</span></span>  <span data-ttu-id="fcc3d-179">Podríamos exponer una nueva palabra clave `iterator` de la que podría dejar punto fuera de para obtener acceso a los `CancellationToken` pasados a `GetEnumerator`pero a) eso es una gran cantidad de maquinaria adicional, b) lo estamos haciendo un ciudadano de primera clase y c) el caso 99% sería el mismo código que llama a un iterador y llama a `GetAsyncEnumerator` en él, en cuyo caso puede simplemente pasar el `CancellationToken` como un argumento en el método.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-179">We could expose a new `iterator` keyword that you could dot off of to get access to the `CancellationToken` passed to `GetEnumerator`, but a) that's a lot of additional machinery, b) we're making it a very first-class citizen, and c) the 99% case would seem to be the same code both calling an iterator and calling `GetAsyncEnumerator` on it, in which case it can just pass the `CancellationToken` as an argument into the method.</span></span>
- <span data-ttu-id="fcc3d-180">¿Cómo se pasa un `CancellationToken` a `MoveNextAsync` entrar en el cuerpo del método?</span><span class="sxs-lookup"><span data-stu-id="fcc3d-180">How does a `CancellationToken` passed to `MoveNextAsync` get into the body of the method?</span></span>  <span data-ttu-id="fcc3d-181">Esto es incluso peor, ya que si se expone fuera de un `iterator` objeto local, su valor podría cambiar entre las esperas, lo que significa que cualquier código que se haya registrado con el token tendría que anular el registro de este antes de la espera y volver a registrarse después de; también puede resultar bastante caro tener que hacer esto registrando y anulando el registro en cada llamada `MoveNextAsync`, independientemente de si el compilador lo implementa en un iterador o un desarrollador manualmente.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-181">This is even worse, as if it's exposed off of an `iterator` local object, its value could change across awaits, which means any code that registered with the token would need to unregister from it prior to awaits and then re-register after; it's also potentially quite expensive to need to do such registering and unregistering in every `MoveNextAsync` call, regardless of whether implemented by the compiler in an iterator or by a developer manually.</span></span>
- <span data-ttu-id="fcc3d-182">¿Cómo cancela un desarrollador un bucle `foreach`?</span><span class="sxs-lookup"><span data-stu-id="fcc3d-182">How does a developer cancel a `foreach` loop?</span></span>  <span data-ttu-id="fcc3d-183">Si se realiza mediante la asignación de un `CancellationToken` a un enumerador o un enumerador, se necesita compatibilidad con `foreach`de los enumeradores. lo que les da lugar a ser ciudadanos de primera clase y ahora debe empezar a pensar en un ecosistema creado en torno a los enumeradores (por ejemplo, los métodos LINQ) o b) necesitamos incrustar el `CancellationToken` en el enumerable de todas formas si tenemos algún método de extensión `WithCancellation` de `IAsyncEnumerable<T>` que almacenaría el token proporcionado y, a continuación, lo pasaría a la `GetAsyncEnumerator` de enumeración `GetAsyncEnumerator` se invoca (se omite ese token).</span><span class="sxs-lookup"><span data-stu-id="fcc3d-183">If it's done by giving a `CancellationToken` to an enumerable/enumerator, then either a) we need to support `foreach`'ing over enumerators, which raises them to being first-class citizens, and now you need to start thinking about an ecosystem built up around enumerators (e.g. LINQ methods) or b) we need to embed the `CancellationToken` in the enumerable anyway by having some `WithCancellation` extension method off of `IAsyncEnumerable<T>` that would store the provided token and then pass it into  the wrapped enumerable's `GetAsyncEnumerator` when the `GetAsyncEnumerator` on the returned struct is invoked (ignoring that token).</span></span>  <span data-ttu-id="fcc3d-184">O bien, solo puede usar el `CancellationToken` que tenga en el cuerpo de la instrucción foreach.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-184">Or, you can just use the `CancellationToken` you have in the body of the foreach.</span></span>
- <span data-ttu-id="fcc3d-185">Si se admiten las comprensión de las consultas, ¿cómo se proporciona el `CancellationToken` a `GetEnumerator` o `MoveNextAsync` pasar a cada cláusula?</span><span class="sxs-lookup"><span data-stu-id="fcc3d-185">If/when query comprehensions are supported, how would the `CancellationToken` supplied to `GetEnumerator` or `MoveNextAsync` be passed into each clause?</span></span>  <span data-ttu-id="fcc3d-186">La manera más sencilla sería simplemente para que la cláusula la Capture, en cuyo punto el token se pasa a `GetAsyncEnumerator`/`MoveNextAsync` se pasa por alto.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-186">The easiest way would simply be for the clause to capture it, at which point whatever token is passed to `GetAsyncEnumerator`/`MoveNextAsync` is ignored.</span></span>

<span data-ttu-id="fcc3d-187">Se recomienda una versión anterior de este documento (1), pero hemos cambiado a (4).</span><span class="sxs-lookup"><span data-stu-id="fcc3d-187">An earlier version of this document recommended (1), but we since switched to (4).</span></span>

<span data-ttu-id="fcc3d-188">Los dos problemas principales de (1):</span><span class="sxs-lookup"><span data-stu-id="fcc3d-188">The two main problems with (1):</span></span>
- <span data-ttu-id="fcc3d-189">los productores de enumerables cancelables tienen que implementar algunos reutilizables y solo pueden aprovechar la compatibilidad del compilador para que los iteradores asincrónicos implementen un método `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-189">producers of cancellable enumerables have to implement some boilerplate, and can only leverage the compiler's support for async-iterators to implement a `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)` method.</span></span>
- <span data-ttu-id="fcc3d-190">es probable que muchos productores pudieran sentirse tentados de agregar un `CancellationToken` parámetro a su firma Async-Enumerable en su lugar, lo que impedirá que los consumidores pasen el token de cancelación que deseen cuando se les proporcione un tipo `IAsyncEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-190">it is likely that many producers would be tempted to just add a `CancellationToken` parameter to their async-enumerable signature instead, which will prevent consumers from passing the cancellation token they want when they are given an `IAsyncEnumerable` type.</span></span>

<span data-ttu-id="fcc3d-191">Hay dos escenarios de consumo principales:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-191">There are two main consumption scenarios:</span></span>
1. <span data-ttu-id="fcc3d-192">`await foreach (var i in GetData(token)) ...` donde el consumidor llama al método Async-iterator,</span><span class="sxs-lookup"><span data-stu-id="fcc3d-192">`await foreach (var i in GetData(token)) ...` where the consumer calls the async-iterator method,</span></span>
2. <span data-ttu-id="fcc3d-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` donde el consumidor trabaja con una instancia `IAsyncEnumerable` determinada.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-193">`await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` where the consumer deals with a given `IAsyncEnumerable` instance.</span></span>

<span data-ttu-id="fcc3d-194">Encontramos un compromiso razonable de admitir ambos escenarios de una manera adecuada para los productores y consumidores de flujos asincrónicos es usar un parámetro anotado especialmente en el método Async-Iterator.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-194">We find that a reasonable compromise to support both scenarios in a way that is convenient for both producers and consumers of async-streams is to use a specially annotated parameter in the async-iterator method.</span></span> <span data-ttu-id="fcc3d-195">El atributo `[EnumeratorCancellation]` se utiliza para este propósito.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-195">The `[EnumeratorCancellation]` attribute is used for this purpose.</span></span> <span data-ttu-id="fcc3d-196">Al colocar este atributo en un parámetro, se indica al compilador que, si se pasa un token al método `GetAsyncEnumerator`, se debe usar ese token en lugar del valor pasado originalmente para el parámetro.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-196">Placing this attribute on a parameter tells the compiler that if a token is passed to the `GetAsyncEnumerator` method, that token should be used instead of the value originally passed for the parameter.</span></span>

<span data-ttu-id="fcc3d-197">Fíjese en `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-197">Consider `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`.</span></span> <span data-ttu-id="fcc3d-198">El implementador de este método puede simplemente usar el parámetro en el cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-198">The implementer of this method can simply use the parameter in the method body.</span></span> <span data-ttu-id="fcc3d-199">El consumidor puede usar cualquiera de los patrones de consumo anteriores:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-199">The consumer can use either consumption patterns above:</span></span>
1. <span data-ttu-id="fcc3d-200">Si usa `GetData(token)`, el token se guarda en Async-enumerable y se usará en la iteración.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-200">if you use `GetData(token)`, then the token is saved into the async-enumerable and will be used in iteration,</span></span>
2. <span data-ttu-id="fcc3d-201">Si usa `givenIAsyncEnumerable.WithCancellation(token)`, el token que se pasa a `GetAsyncEnumerator` reemplazará cualquier token guardado en Async-Enumerable.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-201">if you use `givenIAsyncEnumerable.WithCancellation(token)`, then the token passed to `GetAsyncEnumerator` will supersede any token saved in the async-enumerable.</span></span>

## <a name="foreach"></a><span data-ttu-id="fcc3d-202">foreach</span><span class="sxs-lookup"><span data-stu-id="fcc3d-202">foreach</span></span>

<span data-ttu-id="fcc3d-203">`foreach` se aumentará para admitir `IAsyncEnumerable<T>` además de la compatibilidad existente con `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-203">`foreach` will be augmented to support `IAsyncEnumerable<T>` in addition to its existing support for `IEnumerable<T>`.</span></span>  <span data-ttu-id="fcc3d-204">Y será compatible con el equivalente de `IAsyncEnumerable<T>` como patrón si los miembros pertinentes se exponen públicamente, recurriendo al uso de la interfaz directamente si no, para habilitar las extensiones basadas en struct que evitan la asignación y el uso de awaitables alternativos como el tipo de valor devuelto de `MoveNextAsync` y `DisposeAsync`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-204">And it will support the equivalent of `IAsyncEnumerable<T>` as a pattern if the relevant members are exposed publicly, falling back to using the interface directly if not, in order to enable struct-based extensions that avoid allocating as well as using alternative awaitables as the return type of `MoveNextAsync` and `DisposeAsync`.</span></span>

### <a name="syntax"></a><span data-ttu-id="fcc3d-205">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="fcc3d-205">Syntax</span></span>

<span data-ttu-id="fcc3d-206">Usar la sintaxis:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-206">Using the syntax:</span></span>

```csharp
foreach (var i in enumerable)
```

<span data-ttu-id="fcc3d-207">C#seguirá tratando `enumerable` como un Enumerable sincrónico, de modo que incluso si expone las API pertinentes para los enumerables asincrónicos (exponiendo el patrón o implementando la interfaz), solo tendrá en cuenta las API sincrónicas.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-207">C# will continue to treat `enumerable` as a synchronous enumerable, such that even if it exposes the relevant APIs for async enumerables (exposing the pattern or implementing the interface), it will only consider the synchronous APIs.</span></span>

<span data-ttu-id="fcc3d-208">Para forzar que `foreach` en su lugar solo tenga en cuenta las API asincrónicas, `await` se inserta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-208">To force `foreach` to instead only consider the asynchronous APIs, `await` is inserted as follows:</span></span>

```csharp
await foreach (var i in enumerable)
```

<span data-ttu-id="fcc3d-209">No se proporcionará ninguna sintaxis que admita el uso de las API Async o Sync. el desarrollador debe elegir según la sintaxis utilizada.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-209">No syntax would be provided that would support using either the async or the sync APIs; the developer must choose based on the syntax used.</span></span>

<span data-ttu-id="fcc3d-210">Opciones descartadas que se consideran:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-210">Discarded options considered:</span></span>
- <span data-ttu-id="fcc3d-211">_`foreach (var i in await enumerable)`_ : esta sintaxis ya es válida y cambiar su significado sería un cambio importante.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-211">_`foreach (var i in await enumerable)`_: This is already valid syntax, and changing its meaning would be a breaking change.</span></span>  <span data-ttu-id="fcc3d-212">Esto significa `await` la `enumerable`, obtener algo que se pueda iterar sincrónicamente y, a continuación, recorrer en iteración sincrónicamente.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-212">This means to `await` the `enumerable`, get back something synchronously iterable from it, and then synchronously iterate through that.</span></span>
- <span data-ttu-id="fcc3d-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)``foreach (await var i in enumerable)`_ : todos ellos sugieren que estamos esperando el siguiente elemento, pero hay otras esperas implicadas en foreach, en particular, si el Enumerable es una `IAsyncDisposable`, se `await`"gracias a su eliminación asincrónica".</span><span class="sxs-lookup"><span data-stu-id="fcc3d-213">_`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)`, `foreach (await var i in enumerable)`_: These all suggest that we're awaiting the next item, but there are other awaits involved in foreach, in particular if the enumerable is an `IAsyncDisposable`, we will be `await`'ing its async disposal.</span></span>  <span data-ttu-id="fcc3d-214">Ese Await es como el ámbito de foreach en lugar de para cada elemento individual y, por tanto, la palabra clave `await` merece estar en el nivel de `foreach`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-214">That await is as the scope of the foreach rather than for each individual element, and thus the `await` keyword deserves to be at the `foreach` level.</span></span>  <span data-ttu-id="fcc3d-215">Además, si se asocia con el `foreach` nos proporciona una manera de describir el `foreach` con un término diferente, por ejemplo, "Await foreach".</span><span class="sxs-lookup"><span data-stu-id="fcc3d-215">Further, having it associated with the `foreach` gives us a way to describe the `foreach` with a different term, e.g. a "await foreach".</span></span>  <span data-ttu-id="fcc3d-216">Pero, lo que es más importante, tiene el valor de considerar `foreach` sintaxis al mismo tiempo que `using` sintaxis, de modo que sigan siendo coherentes entre sí y `using (await ...)` ya sea una sintaxis válida.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-216">But more importantly, there's value in considering `foreach` syntax at the same time as `using` syntax, so that they remain consistent with each other, and `using (await ...)` is already valid syntax.</span></span>
- _`foreach await (var i in enumerable)`_

<span data-ttu-id="fcc3d-217">Todavía debe tener en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-217">Still to consider:</span></span>
- <span data-ttu-id="fcc3d-218">en la actualidad, `foreach` no admite la iteración a través de un enumerador.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-218">`foreach` today does not support iterating through an enumerator.</span></span>  <span data-ttu-id="fcc3d-219">Esperamos que sea más común tener `IAsyncEnumerator<T>`se han entregado y, por lo tanto, es tentador admitir `await foreach` con `IAsyncEnumerable<T>` y `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-219">We expect it will be more common to have `IAsyncEnumerator<T>`s handed around, and thus it's tempting to support `await foreach` with both `IAsyncEnumerable<T>` and `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="fcc3d-220">Pero una vez que se agrega dicha compatibilidad, se presenta la pregunta de si `IAsyncEnumerator<T>` es un ciudadano de primera clase y si es necesario tener sobrecargas de los combinadores que operan en los enumeradores además de los enumerables.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-220">But once we add such support, it introduces the question of whether `IAsyncEnumerator<T>` is a first-class citizen, and whether we need to have overloads of combinators that operate on enumerators in addition to enumerables?</span></span>    <span data-ttu-id="fcc3d-221">¿Queremos alentar métodos para que devuelvan enumeradores en lugar de enumerables?</span><span class="sxs-lookup"><span data-stu-id="fcc3d-221">Do we want to encourage methods to return enumerators rather than enumerables?</span></span> <span data-ttu-id="fcc3d-222">Debemos seguir discutiendo esto.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-222">We should continue to discuss this.</span></span>  <span data-ttu-id="fcc3d-223">Si decidimos que no queremos admitirlo, es posible que deseemos introducir un método de extensión `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` que permita que un enumerador siga `foreach`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-223">If we decide we don't want to support it, we might want to introduce an extension method `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` that would allow an enumerator to still be `foreach`'d.</span></span>  <span data-ttu-id="fcc3d-224">Si decidimos que queremos admitirlo, también tendremos que decidir si el `await foreach` sería el responsable de llamar a `DisposeAsync` en el enumerador, y es probable que la respuesta sea "no, el control de la eliminación debe ser controlado por quien se llama `GetEnumerator`".</span><span class="sxs-lookup"><span data-stu-id="fcc3d-224">If we decide we do want to support it, we'll need to also decide on whether the `await foreach` would be responsible for calling `DisposeAsync` on the enumerator, and the answer is likely "no, control over disposal should be handled by whoever called `GetEnumerator`."</span></span>

### <a name="pattern-based-compilation"></a><span data-ttu-id="fcc3d-225">Compilación basada en patrones</span><span class="sxs-lookup"><span data-stu-id="fcc3d-225">Pattern-based Compilation</span></span>

<span data-ttu-id="fcc3d-226">El compilador se enlazará a las API basadas en el patrón, si existen, preparando el uso de la interfaz (el patrón puede estar satisfecho con métodos de instancia o métodos de extensión).</span><span class="sxs-lookup"><span data-stu-id="fcc3d-226">The compiler will bind to the pattern-based APIs if they exist, preferring those over using the interface (the pattern may be satisfied with instance methods or extension methods).</span></span>  <span data-ttu-id="fcc3d-227">Los requisitos para el patrón son:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-227">The requirements for the pattern are:</span></span>
- <span data-ttu-id="fcc3d-228">El Enumerable debe exponer un método `GetAsyncEnumerator` al que se pueda llamar sin argumentos y que devuelva un enumerador que cumpla el patrón pertinente.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-228">The enumerable must expose a `GetAsyncEnumerator` method that may be called with no arguments and that returns an enumerator that meets the relevant pattern.</span></span>
- <span data-ttu-id="fcc3d-229">El enumerador debe exponer un método `MoveNextAsync` al que se pueda llamar sin argumentos y que devuelva algo que pueda ser `await`Ed y cuyo `GetResult()` devuelva un `bool`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-229">The enumerator must expose a `MoveNextAsync` method that may be called with no arguments and that returns something which may be `await`ed and whose `GetResult()` returns a `bool`.</span></span>
- <span data-ttu-id="fcc3d-230">El enumerador también debe exponer `Current` propiedad cuyo captador devuelve un `T` que representa el tipo de datos que se enumeran.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-230">The enumerator must also expose `Current` property whose getter returns a `T` representing the kind of data being enumerated.</span></span>
- <span data-ttu-id="fcc3d-231">El enumerador puede exponer opcionalmente un método `DisposeAsync` que se pueda invocar sin argumentos y que devuelva algo que pueda ser `await`Ed y cuyo `GetResult()` devuelva `void`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-231">The enumerator may optionally expose a `DisposeAsync` method that may be invoked with no arguments and that returns something that can be `await`ed and whose `GetResult()` returns `void`.</span></span>

<span data-ttu-id="fcc3d-232">Este código:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-232">This code:</span></span>

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

<span data-ttu-id="fcc3d-233">se traduce al equivalente de:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-233">is translated to the equivalent of:</span></span>

```csharp
var enumerable = ...;
var enumerator = enumerable.GetAsyncEnumerator();
try
{
    while (await enumerator.MoveNextAsync())
    {
       T item = enumerator.Current;
       ...
    }
}
finally
{
    await enumerator.DisposeAsync(); // omitted, along with the try/finally, if the enumerator doesn't expose DisposeAsync
}
```

<span data-ttu-id="fcc3d-234">Si el tipo iterado no expone el patrón de la derecha, se usarán las interfaces.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-234">If the iterated type doesn't expose the right pattern, the interfaces will be used.</span></span>

### <a name="configureawait"></a><span data-ttu-id="fcc3d-235">ConfigureAwait</span><span class="sxs-lookup"><span data-stu-id="fcc3d-235">ConfigureAwait</span></span>

<span data-ttu-id="fcc3d-236">Esta compilación basada en patrones permitirá el uso de `ConfigureAwait` en todas las esperas, a través de un método de extensión `ConfigureAwait`:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-236">This pattern-based compilation will allow `ConfigureAwait` to be used on all of the awaits, via a `ConfigureAwait` extension method:</span></span>

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

<span data-ttu-id="fcc3d-237">Esto se basará en los tipos que agregaremos también a .NET, probablemente a System. Threading. Tasks. Extensions. dll:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-237">This will be based on types we'll add to .NET as well, likely to System.Threading.Tasks.Extensions.dll:</span></span>

```csharp
// Approximate implementation, omitting arg validation and the like
namespace System.Threading.Tasks
{
    public static class AsyncEnumerableExtensions
    {
        public static ConfiguredAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext) =>
            new ConfiguredAsyncEnumerable<T>(enumerable, continueOnCapturedContext);

        public struct ConfiguredAsyncEnumerable<T>
        {
            private readonly IAsyncEnumerable<T> _enumerable;
            private readonly bool _continueOnCapturedContext;

            internal ConfiguredAsyncEnumerable(IAsyncEnumerable<T> enumerable, bool continueOnCapturedContext)
            {
                _enumerable = enumerable;
                _continueOnCapturedContext = continueOnCapturedContext;
            }

            public ConfiguredAsyncEnumerator<T> GetAsyncEnumerator() =>
                new ConfiguredAsyncEnumerator<T>(_enumerable.GetAsyncEnumerator(), _continueOnCapturedContext);

            public struct Enumerator
            {
                private readonly IAsyncEnumerator<T> _enumerator;
                private readonly bool _continueOnCapturedContext;

                internal Enumerator(IAsyncEnumerator<T> enumerator, bool continueOnCapturedContext)
                {
                    _enumerator = enumerator;
                    _continueOnCapturedContext = continueOnCapturedContext;
                }

                public ConfiguredValueTaskAwaitable<bool> MoveNextAsync() =>
                    _enumerator.MoveNextAsync().ConfigureAwait(_continueOnCapturedContext);

                public T Current => _enumerator.Current;

                public ConfiguredValueTaskAwaitable DisposeAsync() =>
                    _enumerator.DisposeAsync().ConfigureAwait(_continueOnCapturedContext);
            }
        }
    }
}
```

<span data-ttu-id="fcc3d-238">Tenga en cuenta que este enfoque no permitirá el uso de `ConfigureAwait` con enumerables basadas en patrones. pero, de nuevo, es el caso de que el `ConfigureAwait` solo se exponga como una extensión en `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` y no se pueda aplicar a elementos de espera arbitrarios, ya que solo tiene sentido cuando se aplica a las tareas (controla un comportamiento implementado en la compatibilidad de continuación de la tarea) y, por tanto, no tiene sentido cuando se usa un patrón en el que</span><span class="sxs-lookup"><span data-stu-id="fcc3d-238">Note that this approach will not enable `ConfigureAwait` to be used with pattern-based enumerables, but then again it's already the case that the `ConfigureAwait` is only exposed as an extension on `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` and can't be applied to arbitrary awaitable things, as it only makes sense when applied to Tasks (it controls a behavior implemented in Task's continuation support), and thus doesn't make sense when using a pattern where the awaitable things may not be tasks.</span></span>  <span data-ttu-id="fcc3d-239">Cualquier persona que devuelva cosas que puedan esperar puede proporcionar su propio comportamiento personalizado en escenarios avanzados.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-239">Anyone returning awaitable things can provide their own custom behavior in such advanced scenarios.</span></span>

<span data-ttu-id="fcc3d-240">(Si podemos encontrar alguna manera de admitir una solución `ConfigureAwait` de nivel de ensamblado o ámbito, esto no será necesario).</span><span class="sxs-lookup"><span data-stu-id="fcc3d-240">(If we can come up with some way to support a scope- or assembly-level `ConfigureAwait` solution, then this won't be necessary.)</span></span>

## <a name="async-iterators"></a><span data-ttu-id="fcc3d-241">Iteradores Async</span><span class="sxs-lookup"><span data-stu-id="fcc3d-241">Async Iterators</span></span>

<span data-ttu-id="fcc3d-242">El lenguaje/compilador admitirá la generación de `IAsyncEnumerable<T>`s y `IAsyncEnumerator<T>`s además de consumirlos.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-242">The language / compiler will support producing `IAsyncEnumerable<T>`s and `IAsyncEnumerator<T>`s in addition to consuming them.</span></span> <span data-ttu-id="fcc3d-243">En la actualidad, el lenguaje admite la escritura de un iterador como:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-243">Today the language supports writing an iterator like:</span></span>

```csharp
static IEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            Thread.Sleep(1000);
            yield return i;
        }
    }
    finally
    {
        Thread.Sleep(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="fcc3d-244">pero `await` no se pueden usar en el cuerpo de estos iteradores.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-244">but `await` can't be used in the body of these iterators.</span></span>  <span data-ttu-id="fcc3d-245">Agregaremos ese soporte.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-245">We will add that support.</span></span>

### <a name="syntax"></a><span data-ttu-id="fcc3d-246">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="fcc3d-246">Syntax</span></span>

<span data-ttu-id="fcc3d-247">La compatibilidad de lenguajes existente para los iteradores deduce la naturaleza del iterador del método en función de si contiene `yield`s.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-247">The existing language support for iterators infers the iterator nature of the method based on whether it contains any `yield`s.</span></span>  <span data-ttu-id="fcc3d-248">Lo mismo se aplicará a los iteradores asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-248">The same will be true for async iterators.</span></span>  <span data-ttu-id="fcc3d-249">Estos iteradores asincrónicos se delimitarán y se diferenciarán de los iteradores sincrónicos mediante la adición de `async` a la firma y, a continuación, deberán tener también `IAsyncEnumerable<T>` o `IAsyncEnumerator<T>` como su tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-249">Such async iterators will be demarcated and differentiated from synchronous iterators via adding `async` to the signature, and must then also have either `IAsyncEnumerable<T>` or `IAsyncEnumerator<T>` as its return type.</span></span>  <span data-ttu-id="fcc3d-250">Por ejemplo, el ejemplo anterior podría escribirse como un iterador Async de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-250">For example, the above example could be written as an async iterator as follows:</span></span>

```csharp
static async IAsyncEnumerable<int> MyIterator()
{
    try
    {
        for (int i = 0; i < 100; i++)
        {
            await Task.Delay(1000);
            yield return i;
        }
    }
    finally
    {
        await Task.Delay(200);
        Console.WriteLine("finally");
    }
}
```

<span data-ttu-id="fcc3d-251">Alternativas consideradas:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-251">Alternatives considered:</span></span>
- <span data-ttu-id="fcc3d-252">_No usar `async` en la firma_: es probable que el compilador necesite técnicamente el uso de `async`, ya que lo utiliza para determinar si `await` es válido en ese contexto.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-252">_Not using `async` in the signature_: Using `async` is likely technically required by the compiler, as it uses it to determine whether `await` is valid in that context.</span></span>  <span data-ttu-id="fcc3d-253">Pero incluso si no es necesario, hemos establecido que `await` solo se puede usar en métodos marcados como `async`y parece importante mantener la coherencia.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-253">But even if it's not required, we've established that `await` may only be used in methods marked as `async`, and it seems important to keep the consistency.</span></span>
- <span data-ttu-id="fcc3d-254">_Habilitar generadores personalizados para `IAsyncEnumerable<T>`_ : es algo que podríamos echar en el futuro, pero la maquinaria es complicada y no es compatible con los homólogos sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-254">_Enabling custom builders for `IAsyncEnumerable<T>`_:  That's something we could look at for the future, but the machinery is complicated and we don't support that for the synchronous counterparts.</span></span>
- <span data-ttu-id="fcc3d-255">_Tener una palabra clave `iterator` en la firma_: Los iteradores asincrónicos usarían `async iterator` en la firma y `yield` solo se podrían usar en métodos de `async` que incluían `iterator`; a continuación, `iterator` se haría opcional en iteradores sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-255">_Having an `iterator` keyword in the signature_: Async iterators would use `async iterator` in the signature, and `yield` could only be used in `async` methods that included `iterator`; `iterator` would then be made optional on synchronous iterators.</span></span>  <span data-ttu-id="fcc3d-256">En función de su perspectiva, esto tiene la ventaja de que resulta muy evidente por la firma del método si se permite `yield` y si el método está pensado realmente para devolver instancias de tipo `IAsyncEnumerable<T>` en lugar de la fabricación del compilador en función de si el código usa `yield` o no.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-256">Depending on your perspective, this has the benefit of making it very clear by the signature of the method whether `yield` is allowed and whether the method is actually meant to return instances of type `IAsyncEnumerable<T>` rather than the compiler manufacturing one based on whether the code uses `yield` or not.</span></span>  <span data-ttu-id="fcc3d-257">Pero es diferente de los iteradores sincrónicos, que no y no se pueden establecer para que requieran uno.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-257">But it is different from synchronous iterators, which don't and can't be made to require one.</span></span>  <span data-ttu-id="fcc3d-258">Además, algunos desarrolladores no le gustan la sintaxis adicional.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-258">Plus some developers don't like the extra syntax.</span></span>  <span data-ttu-id="fcc3d-259">Si estamos diseñando desde cero, probablemente lo haría necesario, pero en este momento hay mucho más valor en mantener los iteradores asincrónicos cerca de los iteradores de sincronización.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-259">If we were designing it from scratch, we'd probably make this required, but at this point there's much more value in keeping async iterators close to sync iterators.</span></span>

## <a name="linq"></a><span data-ttu-id="fcc3d-260">LINQ</span><span class="sxs-lookup"><span data-stu-id="fcc3d-260">LINQ</span></span>

<span data-ttu-id="fcc3d-261">Hay más de ~ 200 sobrecargas de métodos en la clase `System.Linq.Enumerable`, que funcionan en términos de `IEnumerable<T>`; algunos de estos aceptan `IEnumerable<T>`, algunos de ellos generan `IEnumerable<T>`y muchos de ellos.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-261">There are over ~200 overloads of methods on the `System.Linq.Enumerable` class, all of which work in terms of `IEnumerable<T>`; some of these accept `IEnumerable<T>`, some of them produce `IEnumerable<T>`, and many do both.</span></span>  <span data-ttu-id="fcc3d-262">La adición de compatibilidad con LINQ para `IAsyncEnumerable<T>` podría suponer la duplicación de todas estas sobrecargas para el mismo, por otro ~ 200.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-262">Adding LINQ support for `IAsyncEnumerable<T>` would likely entail duplicating all of these overloads for it, for another ~200.</span></span>  <span data-ttu-id="fcc3d-263">Y como es probable que `IAsyncEnumerator<T>` sea más común como una entidad independiente en el mundo asincrónico que `IEnumerator<T>` se encuentra en el mundo sincrónico, podríamos necesitar otras sobrecargas de ~ 200 que funcionen con `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-263">And since `IAsyncEnumerator<T>` is likely to be more common as a standalone entity in the asynchronous world than `IEnumerator<T>` is in the synchronous world, we could potentially need another ~200 overloads that work with `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="fcc3d-264">Además, un gran número de sobrecargas se ocupan de los predicados (por ejemplo, `Where` que toma un `Func<T, bool>`) y puede ser conveniente tener sobrecargas basadas en `IAsyncEnumerable<T>`que se ocupen de los predicados sincrónicos y asincrónicos (por ejemplo, `Func<T, ValueTask<bool>>` además de `Func<T, bool>`).</span><span class="sxs-lookup"><span data-stu-id="fcc3d-264">Plus, a large number of the overloads deal with predicates (e.g. `Where` that takes a `Func<T, bool>`), and it may be desirable to have `IAsyncEnumerable<T>`-based overloads that deal with both synchronous and asynchronous predicates (e.g. `Func<T, ValueTask<bool>>` in addition to `Func<T, bool>`).</span></span>  <span data-ttu-id="fcc3d-265">Aunque esto no es aplicable a todas las sobrecargas ahora ~ 400 nuevas, un cálculo aproximado es que sería aplicable a la mitad, lo que significa que hay otras sobrecargas de ~ 200, para un total de ~ 600 nuevos métodos.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-265">While this isn't applicable to all of the now ~400 new overloads, a rough calculation is that it'd be applicable to half, which means another ~200 overloads, for a total of ~600 new methods.</span></span>

<span data-ttu-id="fcc3d-266">Es un número escalonado de API, con la posibilidad de incluso más cuando se tienen en cuenta las bibliotecas de extensiones como las extensiones interactivas (IX).</span><span class="sxs-lookup"><span data-stu-id="fcc3d-266">That is a staggering number of APIs, with the potential for even more when extension libraries like Interactive Extensions (Ix) are considered.</span></span>  <span data-ttu-id="fcc3d-267">Pero IX ya tiene una implementación de muchos de ellos y no parece ser una buena razón para duplicar el trabajo. en su lugar, deberíamos ayudar a la comunidad a mejorar IX y recomendarlo para cuando los desarrolladores quieran usar LINQ con `IAsyncEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-267">But Ix already has an implementation of many of these, and there doesn't seem to be a great reason to duplicate that work; we should instead help the community improve Ix and recommend it for when developers want to use LINQ with `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="fcc3d-268">También hay el problema de la sintaxis de la comprensión de las consultas.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-268">There is also the issue of query comprehension syntax.</span></span>  <span data-ttu-id="fcc3d-269">La naturaleza basada en patrones de las comprensión de las consultas permitiría "simplemente trabajar" con algunos operadores, por ejemplo, si IX proporciona los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-269">The pattern-based nature of query comprehensions would allow them to "just work" with some operators, e.g. if Ix provides the following methods:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

<span data-ttu-id="fcc3d-270">a continuación C# , este código "solo funcionará":</span><span class="sxs-lookup"><span data-stu-id="fcc3d-270">then this C# code will "just work":</span></span>

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

<span data-ttu-id="fcc3d-271">Sin embargo, no hay ninguna sintaxis de comprensión de consulta que admita el uso de `await` en las cláusulas, por lo que, si se agrega IX, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-271">However, there is no query comprehension syntax that supports using `await` in the clauses, so if Ix added, for example:</span></span>

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

<span data-ttu-id="fcc3d-272">entonces, esto haría "simplemente trabajar":</span><span class="sxs-lookup"><span data-stu-id="fcc3d-272">then this would "just work":</span></span>

```csharp
IAsyncEnumerable<string> result = from url in urls
                                  where item % 2 == 0
                                  select SomeAsyncMethod(item);

async ValueTask<int> SomeAsyncMethod(int item)
{
    await Task.Yield();
    return item * 2;
}
```

<span data-ttu-id="fcc3d-273">pero no habría ninguna manera de escribirlo con la `await` insertada en la cláusula `select`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-273">but there'd be no way to write it with the `await` inline in the `select` clause.</span></span>  <span data-ttu-id="fcc3d-274">Como un esfuerzo independiente, podríamos considerar agregar expresiones `async { ... }` al lenguaje, en cuyo momento podríamos permitir que se usen en las comprensión de las consultas y, en su lugar, se pudiera escribir lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="fcc3d-274">As a separate effort, we could look into adding `async { ... }` expressions to the language, at which point we could allow them to be used in query comprehensions and the above could instead be written as:</span></span>

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

<span data-ttu-id="fcc3d-275">o bien, para permitir el uso de `await` directamente en expresiones, por ejemplo, para admitir la `async from`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-275">or to enabling `await` to be used directly in expressions, such as by supporting `async from`.</span></span>  <span data-ttu-id="fcc3d-276">Sin embargo, no es probable que un diseño aquí afecte al resto del conjunto de características o a la otra, y esto no es una cuestión especialmente importante para invertir en este momento, por lo que la propuesta consiste en no hacer nada más en este momento.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-276">However, it's unlikely a design here would impact the rest of the feature set one way or the other, and this isn't a particularly high-value thing to invest in right now, so the proposal is to do nothing additional here right now.</span></span>

## <a name="integration-with-other-asynchronous-frameworks"></a><span data-ttu-id="fcc3d-277">Integración con otros marcos asincrónicos</span><span class="sxs-lookup"><span data-stu-id="fcc3d-277">Integration with other asynchronous frameworks</span></span>

<span data-ttu-id="fcc3d-278">La integración con `IObservable<T>` y otros marcos asincrónicos (por ejemplo, secuencias reactivas) se harían en el nivel de biblioteca en lugar de en el nivel de lenguaje.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-278">Integration with `IObservable<T>` and other asynchronous frameworks (e.g. reactive streams) would be done at the library level rather than at the language level.</span></span>  <span data-ttu-id="fcc3d-279">Por ejemplo, todos los datos de un `IAsyncEnumerator<T>` pueden publicarse en un `IObserver<T>` simplemente `await foreach`"por encima del enumerador" y `OnNext`que los datos al observador, por lo que es posible un método de extensión `AsObservable<T>`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-279">For example, all of the data from an `IAsyncEnumerator<T>` can be published to an `IObserver<T>` simply by `await foreach`'ing over the enumerator and `OnNext`'ing the data to the observer, so an `AsObservable<T>` extension method is possible.</span></span>  <span data-ttu-id="fcc3d-280">El consumo de una `IObservable<T>` en un `await foreach` requiere almacenar en búfer los datos (en caso de que se inserte otro elemento mientras el elemento anterior todavía se está procesando), pero se puede implementar fácilmente un adaptador de extracción de inserción para permitir que una `IObservable<T>` se extraiga de un `IAsyncEnumerator<T>`.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-280">Consuming an `IObservable<T>` in a `await foreach` requires buffering the data (in case another item is pushed while the previous item is still being processing), but such a push-pull adapter can easily be implemented to enable an `IObservable<T>` to be pulled from with an `IAsyncEnumerator<T>`.</span></span>  <span data-ttu-id="fcc3d-281">Otros.  RX/IX ya proporcionan prototipos de estas implementaciones, y las bibliotecas como https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels proporcionan varios tipos de estructuras de datos de almacenamiento en búfer.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-281">Etc.  Rx/Ix already provide prototypes of such implementations, and libraries like https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels provide various kinds of buffering data structures.</span></span>  <span data-ttu-id="fcc3d-282">No es necesario que el lenguaje esté implicado en esta fase.</span><span class="sxs-lookup"><span data-stu-id="fcc3d-282">The language need not be involved at this stage.</span></span>
