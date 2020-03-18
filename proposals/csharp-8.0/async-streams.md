---
ms.openlocfilehash: 9ed1aa75d581cecbf754a84d1f523c6334b8c0ac
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2020
ms.locfileid: "79484151"
---
# <a name="async-streams"></a>Secuencias asincrónicas

* [x] propuesto
* [x] prototipo
* [] Implementación
* [] Especificación

## <a name="summary"></a>Resumen
[summary]: #summary

C#admite métodos de iterador y métodos asincrónicos, pero no admite un método que sea un iterador y un método asincrónico.  Debemos rectificar esto permitiendo que `await` se use en una nueva forma de `async` iterador, uno que devuelva un `IAsyncEnumerable<T>` o `IAsyncEnumerator<T>` en lugar de un `IEnumerable<T>` o `IEnumerator<T>`, con `IAsyncEnumerable<T>` consumible en una nueva `await foreach`.  También se usa una interfaz de `IAsyncDisposable` para habilitar la limpieza asincrónica.

## <a name="related-discussion"></a>Discusión relacionada
- https://github.com/dotnet/roslyn/issues/261
- https://github.com/dotnet/roslyn/issues/114

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

## <a name="interfaces"></a>Interfaces

### <a name="iasyncdisposable"></a>IAsyncDisposable

Ha habido mucho análisis de `IAsyncDisposable` (por ejemplo, https://github.com/dotnet/roslyn/issues/114) y si es una buena idea.  Sin embargo, es un concepto necesario para agregar compatibilidad con iteradores asincrónicos.  Como los bloques de `finally` pueden contener `await`s y, como los bloques de `finally` deben ejecutarse como parte de la eliminación de iteradores, se necesita la eliminación asincrónica.  También es muy útil en general cuando la limpieza de recursos puede tardar cualquier período de tiempo, por ejemplo, cerrar los archivos (que requieren vaciados), anular el registro de las devoluciones de llamada y proporcionar una manera de saber cuándo se ha completado el registro, etc.

La siguiente interfaz se agrega a las bibliotecas básicas de .NET (por ejemplo, System. Private. CoreLib/System. Runtime):
```csharp
namespace System
{
    public interface IAsyncDisposable
    {
        ValueTask DisposeAsync();
    }
}
```
Al igual que con `Dispose`, la invocación de `DisposeAsync` varias veces es aceptable, y las invocaciones subsiguientes después de la primera se deben tratar como Nops, devolviendo una tarea completada correctamente completada (`DisposeAsync` no necesita ser segura para subprocesos, sin embargo, y no es necesario que admitan la invocación simultánea).  Además, los tipos pueden implementar `IDisposable` y `IAsyncDisposable`y, si lo hacen, es igualmente aceptable invocar `Dispose` y, a continuación, `DisposeAsync` o viceversa, pero solo el primero debe ser significativo y las invocaciones subsiguientes de deben ser una NOP.  Como tal, si un tipo implementa ambos, se anima a los consumidores a llamar una vez y solo una vez al método más relevante basado en el contexto, `Dispose` en contextos sincrónicos y `DisposeAsync` en los asincrónicos.

(Estoy saliendo del modo en que `IAsyncDisposable` interactúa con `using` a una discusión independiente.  Y la cobertura de cómo interactúa con `foreach` se trata más adelante en esta propuesta).

Alternativas consideradas:
- _`DisposeAsync` aceptar un `CancellationToken`_ : aunque en teoría tiene sentido que se puede cancelar cualquier elemento asincrónico, la eliminación es sobre la limpieza, el cierre de las cosas, los recursos de free'ing, etc., que normalmente no es algo que se debe cancelar. la limpieza todavía es importante para el trabajo que se cancela.  El mismo `CancellationToken` que causó el trabajo real que se va a cancelar sería el mismo token que se pasó a `DisposeAsync`, por lo que no `DisposeAsync` merecente porque la cancelación del trabajo haría que `DisposeAsync` fuera una instrucción NOP.  Si alguien desea evitar que se bloquee a la espera de la eliminación, puede evitar la espera en el `ValueTask`resultante o esperar solo durante un período de tiempo.
- _`DisposeAsync` devolver una `Task`_ : ahora que existe un `ValueTask` no genérico y se puede construir a partir de una `IValueTaskSource`, al devolver `ValueTask` de `DisposeAsync` se permite que un objeto existente se reutilice como promesa que representa la finalización asincrónica eventual de `DisposeAsync`, guardando una asignación `Task` en caso de que `DisposeAsync` se complete de forma asincrónica.
- _Configuración de `DisposeAsync` con una `bool continueOnCapturedContext` (`ConfigureAwait`)_ : aunque puede haber problemas relacionados con el modo en que este concepto se expone a `using`, `foreach`y otras construcciones de lenguaje que consumen esto, desde una perspectiva de interfaz, no está realizando realmente ninguna `await`y no hay nada que configurar... los consumidores del `ValueTask` pueden utilizarlo sin embargo.
- _`IAsyncDisposable` heredar `IDisposable`_ : dado que solo se debe usar una u otra, no tiene sentido forzar los tipos para implementar ambos.
- _`IDisposableAsync` en lugar de `IAsyncDisposable`_ : hemos seguido el nombre de los elementos y tipos que son "Async algo", mientras que las operaciones son "Async Async", por lo que los tipos tienen "Async" como prefijo y los métodos tienen "Async" como sufijo.

### <a name="iasyncenumerable--iasyncenumerator"></a>IAsyncEnumerable / IAsyncEnumerator

Se agregan dos interfaces a las bibliotecas .NET básicas:

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

El consumo típico (sin características adicionales del lenguaje) sería similar al siguiente:

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

Opciones descartadas que se consideran:
- _`Task<bool> MoveNextAsync(); T current { get; }`_ : el uso de `Task<bool>` admitiría el uso de un objeto de tarea almacenado en memoria caché para representar llamadas sincrónicas `MoveNextAsync` correctas, pero todavía se requeriría una asignación para la finalización asincrónica.  Al devolver `ValueTask<bool>`, se habilita el objeto de enumerador para que se implemente `IValueTaskSource<bool>` y se use como respaldo para el `ValueTask<bool>` devuelto de `MoveNextAsync`, lo que a su vez permite una sobrecargas significativamente reducidas.
- _`ValueTask<(bool, T)> MoveNextAsync();`_ : no solo es más difícil de consumir, pero significa que `T` ya no puede ser covariante.
- _`ValueTask<T?> TryMoveNextAsync();`_ : no covariante.
- _`Task<T?> TryMoveNextAsync();`_ : no covariantes, asignaciones en cada llamada, etc.
- _`ITask<T?> TryMoveNextAsync();`_ : no covariantes, asignaciones en cada llamada, etc.
- _`ITask<(bool,T)> TryMoveNextAsync();`_ : no covariantes, asignaciones en cada llamada, etc.
- _`Task<bool> TryMoveNextAsync(out T result);`_ : el resultado de la `out` debe establecerse cuando la operación se devuelve sincrónicamente, no cuando completa de forma asincrónica la tarea potencialmente en el futuro, momento en el que no había forma de comunicar el resultado.
- _`IAsyncEnumerator<T>` no implementar `IAsyncDisposable`_ : podríamos optar por separarlas.  Sin embargo, Esto complica algunas otras áreas de la propuesta, ya que el código debe ser capaz de tratar con la posibilidad de que un enumerador no proporcione la eliminación, lo que dificulta la escritura de aplicaciones auxiliares basadas en patrones.  Además, es habitual que los enumeradores tengan una necesidad de eliminación (por ejemplo, cualquier iterador asincrónico que tenga un bloque Finally, la mayoría de las C# cosas que enumeran datos de una conexión de red, etc.) y, si no es así, es sencillo implementar el método únicamente como `public ValueTask DisposeAsync() => default(ValueTask);` con una mínima sobrecarga adicional.
- _ `IAsyncEnumerator<T> GetAsyncEnumerator()`: no hay ningún parámetro de token de cancelación.

#### <a name="viable-alternative"></a>Alternativa viable:

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

`TryGetNext` se usa en un bucle interno para consumir elementos con una única llamada de interfaz, siempre y cuando estén disponibles de forma sincrónica.  Cuando el siguiente elemento no se puede recuperar sincrónicamente, devuelve false y cada vez que devuelve false, un llamador debe invocar posteriormente `WaitForNextAsync` para esperar a que el siguiente elemento esté disponible o para determinar que nunca habrá otro elemento. El consumo típico (sin características adicionales del lenguaje) sería similar al siguiente:

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

La ventaja de esto es dos veces, una secundaria y una principal:
- _Minor: permite que un enumerador admita varios consumidores_. Puede haber escenarios en los que sea valioso que un enumerador admita varios consumidores simultáneos.  Esto no se puede lograr cuando `MoveNextAsync` y `Current` son independientes para que una implementación no pueda hacer que su uso sea atómico.  En cambio, este enfoque proporciona un método único `TryGetNext` que admite la inserción del enumerador hacia delante y la obtención del elemento siguiente, por lo que el enumerador puede habilitar la atomicidad si se desea.  Sin embargo, es probable que estos escenarios también se puedan habilitar si se asigna a cada consumidor su propio enumerador de un Enumerable compartido.  Además, no queremos exigir que todos los enumeradores admitan el uso simultáneo, ya que esto agregaría sobrecargas no triviales al caso de mayoría que no lo requiera, lo que significa que un consumidor de la interfaz no podía confiar normalmente de este modo.
- _Principal: rendimiento_. El enfoque de `Current` de /de `MoveNextAsync`requiere dos llamadas de interfaz por operación, mientras que el mejor caso para `WaitForNextAsync`/`TryGetNext` es que la mayoría de las iteraciones se completan sincrónicamente, lo que permite un bucle interno estrecho con `TryGetNext`, de modo que solo tenemos una llamada de interfaz por operación.  Esto puede tener un impacto mensurable en situaciones en las que la interfaz llama a dominar el cálculo.

Sin embargo, hay desventajas no triviales, incluida la complejidad significativamente mayor cuando se usan de forma manual y una mayor probabilidad de que se presenten errores al usarlas.  Mientras que las ventajas de rendimiento se muestran en microbenchmarks, no creemos que se verán afectadas por la inmensa mayoría del uso real.  En caso de que sea así, podemos introducir un segundo conjunto de interfaces de manera clara.

Opciones descartadas que se consideran:
- `ValueTask<bool> WaitForNextAsync(); bool TryGetNext(out T result);`: los parámetros de `out` no pueden ser covariantes.  También hay un pequeño impacto aquí (un problema con el patrón try en general) que probablemente incurrirá en una barrera de escritura en tiempo de ejecución para los resultados de tipo de referencia.

#### <a name="cancellation"></a>Cancelación

Existen varios enfoques posibles para admitir la cancelación:
1. `IAsyncEnumerable<T>`/`IAsyncEnumerator<T>` son independientes de la cancelación: `CancellationToken` no aparece en ninguna parte.  La cancelación se logra mediante la conversión lógica del `CancellationToken` en el enumerador y/o de la manera adecuada, por ejemplo, cuando se llama a un iterador, pasando el `CancellationToken` como argumento al método iterador y usando en el cuerpo del iterador, como se hace con cualquier otro parámetro.
2. `IAsyncEnumerator<T>.GetAsyncEnumerator(CancellationToken)`: pasa un `CancellationToken` a `GetAsyncEnumerator`y las operaciones de `MoveNextAsync` subsiguientes lo respetan.
3. `IAsyncEnumerator<T>.MoveNextAsync(CancellationToken)`: se pasa un `CancellationToken` a cada llamada de `MoveNextAsync` individual.
4. 1 & & 2: incrusta `CancellationToken`s en el enumerador o enumerable y pasar `CancellationToken`s a `GetAsyncEnumerator`.
5. 1 & & 3: incrusta `CancellationToken`s en el enumerador o enumerable y pasar `CancellationToken`s a `MoveNextAsync`.

Desde una perspectiva puramente teórica, (5) es la más sólida, en la que (a) `MoveNextAsync` aceptar un `CancellationToken` permite el control más preciso sobre lo que se cancela y (b) `CancellationToken` es solo cualquier otro tipo que se pueda pasar como argumento en iteradores, incrustados en tipos arbitrarios, etc.

Sin embargo, hay varios problemas con este enfoque:
- ¿Cómo se pasa un `CancellationToken` a `GetAsyncEnumerator` convertirlo en el cuerpo del iterador?  Podríamos exponer una nueva palabra clave `iterator` de la que podría dejar punto fuera de para obtener acceso a los `CancellationToken` pasados a `GetEnumerator`pero a) eso es una gran cantidad de maquinaria adicional, b) lo estamos haciendo un ciudadano de primera clase y c) el caso 99% sería el mismo código que llama a un iterador y llama a `GetAsyncEnumerator` en él, en cuyo caso puede simplemente pasar el `CancellationToken` como un argumento en el método.
- ¿Cómo se pasa un `CancellationToken` a `MoveNextAsync` entrar en el cuerpo del método?  Esto es incluso peor, ya que si se expone fuera de un `iterator` objeto local, su valor podría cambiar entre las esperas, lo que significa que cualquier código que se haya registrado con el token tendría que anular el registro de este antes de la espera y volver a registrarse después de; también puede resultar bastante caro tener que hacer esto registrando y anulando el registro en cada llamada `MoveNextAsync`, independientemente de si el compilador lo implementa en un iterador o un desarrollador manualmente.
- ¿Cómo cancela un desarrollador un bucle `foreach`?  Si se realiza mediante la asignación de un `CancellationToken` a un enumerador o un enumerador, se necesita compatibilidad con `foreach`de los enumeradores. lo que les da lugar a ser ciudadanos de primera clase y ahora debe empezar a pensar en un ecosistema creado en torno a los enumeradores (por ejemplo, los métodos LINQ) o b) necesitamos incrustar el `CancellationToken` en el enumerable de todas formas si tenemos algún método de extensión `WithCancellation` de `IAsyncEnumerable<T>` que almacenaría el token proporcionado y, a continuación, lo pasaría a la `GetAsyncEnumerator` de enumeración `GetAsyncEnumerator` se invoca (se omite ese token).  O bien, solo puede usar el `CancellationToken` que tenga en el cuerpo de la instrucción foreach.
- Si se admiten las comprensión de las consultas, ¿cómo se proporciona el `CancellationToken` a `GetEnumerator` o `MoveNextAsync` pasar a cada cláusula?  La manera más sencilla sería simplemente para que la cláusula la Capture, en cuyo punto el token se pasa a `GetAsyncEnumerator`/`MoveNextAsync` se pasa por alto.

Se recomienda una versión anterior de este documento (1), pero hemos cambiado a (4).

Los dos problemas principales de (1):
- los productores de enumerables cancelables tienen que implementar algunos reutilizables y solo pueden aprovechar la compatibilidad del compilador para que los iteradores asincrónicos implementen un método `IAsyncEnumerator<T> GetAsyncEnumerator(CancellationToken)`.
- es probable que muchos productores pudieran sentirse tentados de agregar un `CancellationToken` parámetro a su firma Async-Enumerable en su lugar, lo que impedirá que los consumidores pasen el token de cancelación que deseen cuando se les proporcione un tipo `IAsyncEnumerable`.

Hay dos escenarios de consumo principales:
1. `await foreach (var i in GetData(token)) ...` donde el consumidor llama al método Async-iterator,
2. `await foreach (var i in givenIAsyncEnumerable.WithCancellation(token)) ...` donde el consumidor trabaja con una instancia `IAsyncEnumerable` determinada.

Encontramos un compromiso razonable de admitir ambos escenarios de una manera adecuada para los productores y consumidores de flujos asincrónicos es usar un parámetro anotado especialmente en el método Async-Iterator. El atributo `[EnumeratorCancellation]` se utiliza para este propósito. Al colocar este atributo en un parámetro, se indica al compilador que, si se pasa un token al método `GetAsyncEnumerator`, se debe usar ese token en lugar del valor pasado originalmente para el parámetro.

Fíjese en `IAsyncEnumerable<int> GetData([EnumeratorCancellation] CancellationToken token = default)`. El implementador de este método puede simplemente usar el parámetro en el cuerpo del método. El consumidor puede usar cualquiera de los patrones de consumo anteriores:
1. Si usa `GetData(token)`, el token se guarda en Async-enumerable y se usará en la iteración.
2. Si usa `givenIAsyncEnumerable.WithCancellation(token)`, el token que se pasa a `GetAsyncEnumerator` reemplazará cualquier token guardado en Async-Enumerable.

## <a name="foreach"></a>foreach

`foreach` se aumentará para admitir `IAsyncEnumerable<T>` además de la compatibilidad existente con `IEnumerable<T>`.  Y será compatible con el equivalente de `IAsyncEnumerable<T>` como patrón si los miembros pertinentes se exponen públicamente, recurriendo al uso de la interfaz directamente si no, para habilitar las extensiones basadas en struct que evitan la asignación y el uso de awaitables alternativos como el tipo de valor devuelto de `MoveNextAsync` y `DisposeAsync`.

### <a name="syntax"></a>Sintaxis

Usar la sintaxis:

```csharp
foreach (var i in enumerable)
```

C#seguirá tratando `enumerable` como un Enumerable sincrónico, de modo que incluso si expone las API pertinentes para los enumerables asincrónicos (exponiendo el patrón o implementando la interfaz), solo tendrá en cuenta las API sincrónicas.

Para forzar que `foreach` en su lugar solo tenga en cuenta las API asincrónicas, `await` se inserta de la siguiente manera:

```csharp
await foreach (var i in enumerable)
```

No se proporcionará ninguna sintaxis que admita el uso de las API Async o Sync. el desarrollador debe elegir según la sintaxis utilizada.

Opciones descartadas que se consideran:
- _`foreach (var i in await enumerable)`_ : esta sintaxis ya es válida y cambiar su significado sería un cambio importante.  Esto significa `await` la `enumerable`, obtener algo que se pueda iterar sincrónicamente y, a continuación, recorrer en iteración sincrónicamente.
- _`foreach (var i await in enumerable)`, `foreach (var await i in enumerable)``foreach (await var i in enumerable)`_ : todos ellos sugieren que estamos esperando el siguiente elemento, pero hay otras esperas implicadas en foreach, en particular, si el Enumerable es una `IAsyncDisposable`, se `await`"gracias a su eliminación asincrónica".  Ese Await es como el ámbito de foreach en lugar de para cada elemento individual y, por tanto, la palabra clave `await` merece estar en el nivel de `foreach`.  Además, si se asocia con el `foreach` nos proporciona una manera de describir el `foreach` con un término diferente, por ejemplo, "Await foreach".  Pero, lo que es más importante, tiene el valor de considerar `foreach` sintaxis al mismo tiempo que `using` sintaxis, de modo que sigan siendo coherentes entre sí y `using (await ...)` ya sea una sintaxis válida.
- _`foreach await (var i in enumerable)`_

Todavía debe tener en cuenta lo siguiente:
- en la actualidad, `foreach` no admite la iteración a través de un enumerador.  Esperamos que sea más común tener `IAsyncEnumerator<T>`se han entregado y, por lo tanto, es tentador admitir `await foreach` con `IAsyncEnumerable<T>` y `IAsyncEnumerator<T>`.  Pero una vez que se agrega dicha compatibilidad, se presenta la pregunta de si `IAsyncEnumerator<T>` es un ciudadano de primera clase y si es necesario tener sobrecargas de los combinadores que operan en los enumeradores además de los enumerables.    ¿Queremos alentar métodos para que devuelvan enumeradores en lugar de enumerables? Debemos seguir discutiendo esto.  Si decidimos que no queremos admitirlo, es posible que deseemos introducir un método de extensión `public static IAsyncEnumerable<T> AsEnumerable<T>(this IAsyncEnumerator<T> enumerator);` que permita que un enumerador siga `foreach`.  Si decidimos que queremos admitirlo, también tendremos que decidir si el `await foreach` sería el responsable de llamar a `DisposeAsync` en el enumerador, y es probable que la respuesta sea "no, el control de la eliminación debe ser controlado por quien se llama `GetEnumerator`".

### <a name="pattern-based-compilation"></a>Compilación basada en patrones

El compilador se enlazará a las API basadas en el patrón, si existen, preparando el uso de la interfaz (el patrón puede estar satisfecho con métodos de instancia o métodos de extensión).  Los requisitos para el patrón son:
- El Enumerable debe exponer un método `GetAsyncEnumerator` al que se pueda llamar sin argumentos y que devuelva un enumerador que cumpla el patrón pertinente.
- El enumerador debe exponer un método `MoveNextAsync` al que se pueda llamar sin argumentos y que devuelva algo que pueda ser `await`Ed y cuyo `GetResult()` devuelva un `bool`.
- El enumerador también debe exponer `Current` propiedad cuyo captador devuelve un `T` que representa el tipo de datos que se enumeran.
- El enumerador puede exponer opcionalmente un método `DisposeAsync` que se pueda invocar sin argumentos y que devuelva algo que pueda ser `await`Ed y cuyo `GetResult()` devuelva `void`.

Este código:

```csharp
var enumerable = ...;
await foreach (T item in enumerable)
{
   ...
}
```

se traduce al equivalente de:

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

Si el tipo iterado no expone el patrón de la derecha, se usarán las interfaces.

### <a name="configureawait"></a>ConfigureAwait

Esta compilación basada en patrones permitirá el uso de `ConfigureAwait` en todas las esperas, a través de un método de extensión `ConfigureAwait`:

```csharp
await foreach (T item in enumerable.ConfigureAwait(false))
{
   ...
}
```

Esto se basará en los tipos que agregaremos también a .NET, probablemente a System. Threading. Tasks. Extensions. dll:

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

Tenga en cuenta que este enfoque no permitirá el uso de `ConfigureAwait` con enumerables basadas en patrones. pero, de nuevo, es el caso de que el `ConfigureAwait` solo se exponga como una extensión en `Task`/`Task<T>`/`ValueTask`/`ValueTask<T>` y no se pueda aplicar a elementos de espera arbitrarios, ya que solo tiene sentido cuando se aplica a las tareas (controla un comportamiento implementado en la compatibilidad de continuación de la tarea) y, por tanto, no tiene sentido cuando se usa un patrón en el que  Cualquier persona que devuelva cosas que puedan esperar puede proporcionar su propio comportamiento personalizado en escenarios avanzados.

(Si podemos encontrar alguna manera de admitir una solución `ConfigureAwait` de nivel de ensamblado o ámbito, esto no será necesario).

## <a name="async-iterators"></a>Iteradores Async

El lenguaje/compilador admitirá la generación de `IAsyncEnumerable<T>`s y `IAsyncEnumerator<T>`s además de consumirlos. En la actualidad, el lenguaje admite la escritura de un iterador como:

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

pero `await` no se pueden usar en el cuerpo de estos iteradores.  Agregaremos ese soporte.

### <a name="syntax"></a>Sintaxis

La compatibilidad de lenguajes existente para los iteradores deduce la naturaleza del iterador del método en función de si contiene `yield`s.  Lo mismo se aplicará a los iteradores asincrónicos.  Estos iteradores asincrónicos se delimitarán y se diferenciarán de los iteradores sincrónicos mediante la adición de `async` a la firma y, a continuación, deberán tener también `IAsyncEnumerable<T>` o `IAsyncEnumerator<T>` como su tipo de valor devuelto.  Por ejemplo, el ejemplo anterior podría escribirse como un iterador Async de la siguiente manera:

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

Alternativas consideradas:
- _No usar `async` en la firma_: es probable que el compilador necesite técnicamente el uso de `async`, ya que lo utiliza para determinar si `await` es válido en ese contexto.  Pero incluso si no es necesario, hemos establecido que `await` solo se puede usar en métodos marcados como `async`y parece importante mantener la coherencia.
- _Habilitar generadores personalizados para `IAsyncEnumerable<T>`_ : es algo que podríamos echar en el futuro, pero la maquinaria es complicada y no es compatible con los homólogos sincrónicos.
- _Tener una palabra clave `iterator` en la firma_: Los iteradores asincrónicos usarían `async iterator` en la firma y `yield` solo se podrían usar en métodos de `async` que incluían `iterator`; a continuación, `iterator` se haría opcional en iteradores sincrónicos.  En función de su perspectiva, esto tiene la ventaja de que resulta muy evidente por la firma del método si se permite `yield` y si el método está pensado realmente para devolver instancias de tipo `IAsyncEnumerable<T>` en lugar de la fabricación del compilador en función de si el código usa `yield` o no.  Pero es diferente de los iteradores sincrónicos, que no y no se pueden establecer para que requieran uno.  Además, algunos desarrolladores no le gustan la sintaxis adicional.  Si estamos diseñando desde cero, probablemente lo haría necesario, pero en este momento hay mucho más valor en mantener los iteradores asincrónicos cerca de los iteradores de sincronización.

## <a name="linq"></a>LINQ

Hay más de ~ 200 sobrecargas de métodos en la clase `System.Linq.Enumerable`, que funcionan en términos de `IEnumerable<T>`; algunos de estos aceptan `IEnumerable<T>`, algunos de ellos generan `IEnumerable<T>`y muchos de ellos.  La adición de compatibilidad con LINQ para `IAsyncEnumerable<T>` podría suponer la duplicación de todas estas sobrecargas para el mismo, por otro ~ 200.  Y como es probable que `IAsyncEnumerator<T>` sea más común como una entidad independiente en el mundo asincrónico que `IEnumerator<T>` se encuentra en el mundo sincrónico, podríamos necesitar otras sobrecargas de ~ 200 que funcionen con `IAsyncEnumerator<T>`.  Además, un gran número de sobrecargas se ocupan de los predicados (por ejemplo, `Where` que toma un `Func<T, bool>`) y puede ser conveniente tener sobrecargas basadas en `IAsyncEnumerable<T>`que se ocupen de los predicados sincrónicos y asincrónicos (por ejemplo, `Func<T, ValueTask<bool>>` además de `Func<T, bool>`).  Aunque esto no es aplicable a todas las sobrecargas ahora ~ 400 nuevas, un cálculo aproximado es que sería aplicable a la mitad, lo que significa que hay otras sobrecargas de ~ 200, para un total de ~ 600 nuevos métodos.

Es un número escalonado de API, con la posibilidad de incluso más cuando se tienen en cuenta las bibliotecas de extensiones como las extensiones interactivas (IX).  Pero IX ya tiene una implementación de muchos de ellos y no parece ser una buena razón para duplicar el trabajo. en su lugar, deberíamos ayudar a la comunidad a mejorar IX y recomendarlo para cuando los desarrolladores quieran usar LINQ con `IAsyncEnumerable<T>`.

También hay el problema de la sintaxis de la comprensión de las consultas.  La naturaleza basada en patrones de las comprensión de las consultas permitiría "simplemente trabajar" con algunos operadores, por ejemplo, si IX proporciona los métodos siguientes:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> func);
public static IAsyncEnumerable<T> Where(this IAsyncEnumerable<T> source, Func<T, bool> func);
```

a continuación C# , este código "solo funcionará":

```csharp
IAsyncEnumerable<int> enumerable = ...;
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select item * 2;
```

Sin embargo, no hay ninguna sintaxis de comprensión de consulta que admita el uso de `await` en las cláusulas, por lo que, si se agrega IX, por ejemplo:

```csharp
public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, ValueTask<TResult>> func);
```

entonces, esto haría "simplemente trabajar":

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

pero no habría ninguna manera de escribirlo con la `await` insertada en la cláusula `select`.  Como un esfuerzo independiente, podríamos considerar agregar expresiones `async { ... }` al lenguaje, en cuyo momento podríamos permitir que se usen en las comprensión de las consultas y, en su lugar, se pudiera escribir lo siguiente:

```csharp
IAsyncEnumerable<int> result = from item in enumerable
                               where item % 2 == 0
                               select async
                               {
                                   await Task.Yield();
                                   return item * 2;
                               };
```

o bien, para permitir el uso de `await` directamente en expresiones, por ejemplo, para admitir la `async from`.  Sin embargo, no es probable que un diseño aquí afecte al resto del conjunto de características o a la otra, y esto no es una cuestión especialmente importante para invertir en este momento, por lo que la propuesta consiste en no hacer nada más en este momento.

## <a name="integration-with-other-asynchronous-frameworks"></a>Integración con otros marcos asincrónicos

La integración con `IObservable<T>` y otros marcos asincrónicos (por ejemplo, secuencias reactivas) se harían en el nivel de biblioteca en lugar de en el nivel de lenguaje.  Por ejemplo, todos los datos de un `IAsyncEnumerator<T>` pueden publicarse en un `IObserver<T>` simplemente `await foreach`"por encima del enumerador" y `OnNext`que los datos al observador, por lo que es posible un método de extensión `AsObservable<T>`.  El consumo de una `IObservable<T>` en un `await foreach` requiere almacenar en búfer los datos (en caso de que se inserte otro elemento mientras el elemento anterior todavía se está procesando), pero se puede implementar fácilmente un adaptador de extracción de inserción para permitir que una `IObservable<T>` se extraiga de un `IAsyncEnumerator<T>`.  Otros.  RX/IX ya proporcionan prototipos de estas implementaciones, y las bibliotecas como https://github.com/dotnet/corefx/tree/master/src/System.Threading.Channels proporcionan varios tipos de estructuras de datos de almacenamiento en búfer.  No es necesario que el lenguaje esté implicado en esta fase.
