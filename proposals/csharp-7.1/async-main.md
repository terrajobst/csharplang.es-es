---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483755"
---
# <a name="async-main"></a><span data-ttu-id="d114f-101">Async principal</span><span class="sxs-lookup"><span data-stu-id="d114f-101">Async Main</span></span>

* <span data-ttu-id="d114f-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="d114f-102">[x] Proposed</span></span>
* <span data-ttu-id="d114f-103">[] Prototipo</span><span class="sxs-lookup"><span data-stu-id="d114f-103">[ ] Prototype</span></span>
* <span data-ttu-id="d114f-104">[] Implementación</span><span class="sxs-lookup"><span data-stu-id="d114f-104">[ ] Implementation</span></span>
* <span data-ttu-id="d114f-105">[] Especificación</span><span class="sxs-lookup"><span data-stu-id="d114f-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="d114f-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="d114f-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d114f-107">Permita el uso de `await` en el método principal o de punto de entrada de una aplicación permitiendo que el punto de entrada devuelva `Task` / `Task<int>` y se marque `async`.</span><span class="sxs-lookup"><span data-stu-id="d114f-107">Allow `await` to be used in an application's Main / entrypoint method by allowing the entrypoint to return `Task` / `Task<int>` and be marked `async`.</span></span>

## <a name="motivation"></a><span data-ttu-id="d114f-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="d114f-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d114f-109">Es muy común cuando se aprende C#, cuando se escriben utilidades basadas en consola y cuando se escriben aplicaciones de prueba pequeñas en las que se desea llamar a y `await` métodos de `async` desde Main.</span><span class="sxs-lookup"><span data-stu-id="d114f-109">It is very common when learning C#, when writing console-based utilities, and when writing small test apps to want to call and `await` `async` methods from Main.</span></span>  <span data-ttu-id="d114f-110">Hoy en día, agregamos un nivel de complejidad al forzar que este tipo de `await`se realice en un método asincrónico independiente, lo que hace que los desarrolladores tengan que escribir elementos reutilizables como el siguiente para empezar:</span><span class="sxs-lookup"><span data-stu-id="d114f-110">Today we add a level of complexity here by forcing such `await`'ing to be done in a separate async method, which causes developers to need to write boilerplate like the following just to get started:</span></span>

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

<span data-ttu-id="d114f-111">Podemos eliminar la necesidad de este texto reutilizable y facilitar la introducción al uso de la propia tarea principal para que `async` se pueda usar `await`s.</span><span class="sxs-lookup"><span data-stu-id="d114f-111">We can remove the need for this boilerplate and make it easier to get started simply by allowing Main itself to be `async` such that `await`s can be used in it.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d114f-112">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="d114f-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d114f-113">Actualmente se permiten las siguientes firmas entryPoints:</span><span class="sxs-lookup"><span data-stu-id="d114f-113">The following signatures are currently allowed entrypoints:</span></span>

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

<span data-ttu-id="d114f-114">Ampliamos la lista de entryPoints permitidos para incluir:</span><span class="sxs-lookup"><span data-stu-id="d114f-114">We extend the list of allowed entrypoints to include:</span></span>

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

<span data-ttu-id="d114f-115">Para evitar riesgos de compatibilidad, estas nuevas firmas solo se considerarán como entryPoints válidas si no hay sobrecargas del conjunto anterior.</span><span class="sxs-lookup"><span data-stu-id="d114f-115">To avoid compatibility risks, these new signatures will only be considered as valid entrypoints if no overloads of the previous set are present.</span></span>
<span data-ttu-id="d114f-116">El lenguaje/compilador no requerirá que el punto de entrada esté marcado como `async`, aunque esperamos que la mayoría de los usos se marque como tal.</span><span class="sxs-lookup"><span data-stu-id="d114f-116">The language / compiler will not require that the entrypoint be marked as `async`, though we expect the vast majority of uses will be marked as such.</span></span>

<span data-ttu-id="d114f-117">Cuando uno de ellos se identifica como el punto de entrada, el compilador sintetizará un método de punto de entrada real que llama a uno de estos métodos codificados:</span><span class="sxs-lookup"><span data-stu-id="d114f-117">When one of these is identified as the entrypoint, the compiler will synthesize an actual entrypoint method that calls one of these coded methods:</span></span>
- <span data-ttu-id="d114f-118">```static Task Main()``` provocará que el compilador emita el equivalente de ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="d114f-118">```static Task Main()``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="d114f-119">```static Task Main(string[])``` provocará que el compilador emita el equivalente de ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="d114f-119">```static Task Main(string[])``` will result in the compiler emitting the equivalent of ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="d114f-120">```static Task<int> Main()``` provocará que el compilador emita el equivalente de ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="d114f-120">```static Task<int> Main()``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```</span></span>
- <span data-ttu-id="d114f-121">```static Task<int> Main(string[])``` provocará que el compilador emita el equivalente de ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span><span class="sxs-lookup"><span data-stu-id="d114f-121">```static Task<int> Main(string[])``` will result in the compiler emitting the equivalent of ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```</span></span>

<span data-ttu-id="d114f-122">Ejemplo de uso:</span><span class="sxs-lookup"><span data-stu-id="d114f-122">Example usage:</span></span>

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a><span data-ttu-id="d114f-123">Desventajas</span><span class="sxs-lookup"><span data-stu-id="d114f-123">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d114f-124">La principal desventaja es simplemente la complejidad adicional de admitir firmas de punto de entrada adicionales.</span><span class="sxs-lookup"><span data-stu-id="d114f-124">The main drawback is simply the additional complexity of supporting additional entrypoint signatures.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d114f-125">Alternativas</span><span class="sxs-lookup"><span data-stu-id="d114f-125">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d114f-126">Otras variantes consideradas:</span><span class="sxs-lookup"><span data-stu-id="d114f-126">Other variants considered:</span></span>

<span data-ttu-id="d114f-127">Permitir `async void`.</span><span class="sxs-lookup"><span data-stu-id="d114f-127">Allowing `async void`.</span></span>  <span data-ttu-id="d114f-128">Necesitamos mantener la semántica igual para que el código la llame directamente, lo que a continuación dificultaría que un punto de entrada generado lo llamara (no se devuelve ninguna tarea).</span><span class="sxs-lookup"><span data-stu-id="d114f-128">We need to keep the semantics the same for code calling it directly, which would then make it difficult for a generated entrypoint to call it (no Task returned).</span></span>  <span data-ttu-id="d114f-129">Podríamos resolver esto generando otros dos métodos, por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="d114f-129">We could solve this by generating two other methods, e.g.</span></span>

```csharp
public static async void Main()
{
   ... // await code
}
```

<span data-ttu-id="d114f-130">se convierte en</span><span class="sxs-lookup"><span data-stu-id="d114f-130">becomes</span></span>

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

<span data-ttu-id="d114f-131">También hay dudas acerca de cómo alentar el uso de `async void`.</span><span class="sxs-lookup"><span data-stu-id="d114f-131">There are also concerns around encouraging usage of `async void`.</span></span>

<span data-ttu-id="d114f-132">Usar "MainAsync" en lugar de "Main" como nombre.</span><span class="sxs-lookup"><span data-stu-id="d114f-132">Using "MainAsync" instead of "Main" as the name.</span></span>  <span data-ttu-id="d114f-133">Aunque se recomienda el sufijo Async para los métodos que devuelven tareas, principalmente sobre la funcionalidad de la biblioteca, que Main no es, y que admiten nombres de punto de entrada adicionales más allá de "Main" no merece la pena.</span><span class="sxs-lookup"><span data-stu-id="d114f-133">While the async suffix is recommended for Task-returning methods, that's primarily about library functionality, which Main is not, and supporting additional entrypoint names beyond "Main" is not worth it.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d114f-134">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="d114f-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="d114f-135">N/D</span><span class="sxs-lookup"><span data-stu-id="d114f-135">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d114f-136">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="d114f-136">Design meetings</span></span>

<span data-ttu-id="d114f-137">N/D</span><span class="sxs-lookup"><span data-stu-id="d114f-137">n/a</span></span>
