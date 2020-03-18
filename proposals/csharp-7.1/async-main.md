---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483755"
---
# <a name="async-main"></a>Async principal

* [x] propuesto
* [] Prototipo
* [] Implementación
* [] Especificación

## <a name="summary"></a>Resumen
[summary]: #summary

Permita el uso de `await` en el método principal o de punto de entrada de una aplicación permitiendo que el punto de entrada devuelva `Task` / `Task<int>` y se marque `async`.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Es muy común cuando se aprende C#, cuando se escriben utilidades basadas en consola y cuando se escriben aplicaciones de prueba pequeñas en las que se desea llamar a y `await` métodos de `async` desde Main.  Hoy en día, agregamos un nivel de complejidad al forzar que este tipo de `await`se realice en un método asincrónico independiente, lo que hace que los desarrolladores tengan que escribir elementos reutilizables como el siguiente para empezar:

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

Podemos eliminar la necesidad de este texto reutilizable y facilitar la introducción al uso de la propia tarea principal para que `async` se pueda usar `await`s.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Actualmente se permiten las siguientes firmas entryPoints:

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

Ampliamos la lista de entryPoints permitidos para incluir:

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

Para evitar riesgos de compatibilidad, estas nuevas firmas solo se considerarán como entryPoints válidas si no hay sobrecargas del conjunto anterior.
El lenguaje/compilador no requerirá que el punto de entrada esté marcado como `async`, aunque esperamos que la mayoría de los usos se marque como tal.

Cuando uno de ellos se identifica como el punto de entrada, el compilador sintetizará un método de punto de entrada real que llama a uno de estos métodos codificados:
- ```static Task Main()``` provocará que el compilador emita el equivalente de ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task Main(string[])``` provocará que el compilador emita el equivalente de ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```
- ```static Task<int> Main()``` provocará que el compilador emita el equivalente de ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task<int> Main(string[])``` provocará que el compilador emita el equivalente de ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```

Ejemplo de uso:

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

La principal desventaja es simplemente la complejidad adicional de admitir firmas de punto de entrada adicionales.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Otras variantes consideradas:

Permitir `async void`.  Necesitamos mantener la semántica igual para que el código la llame directamente, lo que a continuación dificultaría que un punto de entrada generado lo llamara (no se devuelve ninguna tarea).  Podríamos resolver esto generando otros dos métodos, por ejemplo,

```csharp
public static async void Main()
{
   ... // await code
}
```

se convierte en

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

También hay dudas acerca de cómo alentar el uso de `async void`.

Usar "MainAsync" en lugar de "Main" como nombre.  Aunque se recomienda el sufijo Async para los métodos que devuelven tareas, principalmente sobre la funcionalidad de la biblioteca, que Main no es, y que admiten nombres de punto de entrada adicionales más allá de "Main" no merece la pena.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

N/D

## <a name="design-meetings"></a>Design Meetings

N/D
