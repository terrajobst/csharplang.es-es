---
ms.openlocfilehash: 922353d043653ddb651534a01f3fb98f85cd756e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483485"
---
# <a name="readonly-locals-and-parameters"></a>variables locales y parámetros de solo lectura

* [x] propuesto
* [] Prototipo
* [] Implementación
* [] Especificación

## <a name="summary"></a>Resumen
[summary]: #summary

Permita anotar variables locales y parámetros como de solo lectura para evitar la mutación superficial de esas variables locales y parámetros.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

En la actualidad, la palabra clave `readonly` se puede aplicar a los campos; Esto tiene el efecto de asegurarse de que un campo solo se puede escribir en durante la construcción (construcción estática en el caso de un campo estático o una construcción de instancia en el caso de un campo de instancia), lo que ayuda a los desarrolladores a evitar errores al sobrescribir accidentalmente el estado que no debe modificarse. Pero los campos no son los únicos lugares en los que los desarrolladores quieren asegurarse de que los valores no están mutados. En concreto, es habitual crear una variable local para almacenar el estado temporal y actualizar accidentalmente ese estado temporal puede producir cálculos erróneos y otros errores, especialmente cuando estas "variables locales" se capturan en lambdas, momento en el que se elevan a campos, pero actualmente no hay ninguna manera de marcar tales campos de elevación como `readonly`.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Las variables locales también se pueden anotar como `readonly`, con el compilador que garantiza que solo se establecen en el momento de la declaración C# (ciertas variables locales en ya son implícitamente de solo lectura, como la variable de iteración en un bucle ' foreach ' o la variable usada en un bloque ' Using ', pero actualmente un desarrollador no tiene la capacidad de marcar otras variables locales como `readonly`) `readonly` variables locales deben tener un inicializador:

```csharp
readonly long maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

Y como abreviatura para `readonly var`, se puede usar la palabra clave contextual existente `let`, por ejemplo,

```csharp
let maxBytesToDelete = (stream.LimitBytes - stream.MaxBytes) / 10;
...
maxBytesToDelete = 0; // Error: can't assign to readonly locals outside of declaration
```

No hay ninguna restricción especial para lo que puede ser el inicializador y puede ser cualquier elemento que sea válido actualmente como inicializador para variables locales, por ejemplo,

```csharp
readonly T data = arg1 ?? arg2;
```

`readonly` en variables locales es especialmente útil cuando se trabaja con expresiones lambda y cierres. Cuando un método anónimo o una expresión lambda accede al estado local desde el ámbito de inclusión, el compilador, que se representa mediante una "clase de presentación", lo captura en una clausura.  Cada "local" que se captura es un campo de esta clase, aunque el compilador está generando este campo en su nombre, no tiene la oportunidad de anotarlo como `readonly` para evitar que la expresión lambda escriba erróneamente en el "local" (entre comillas porque realmente no es local, al menos no en el MSIL resultante). Con `readonly` variables locales, el compilador puede impedir que la expresión lambda se escriba en una variable local, lo que es especialmente útil en escenarios que implican multithreading, donde una escritura errónea podría producir un error de simultaneidad peligroso pero poco frecuente y difícil de encontrar.

```csharp
readonly long index = ...;
Parallel.ForEach(data, item => {
    T element = item[index];
    index = 0; // Error: can't assign to readonly locals outside of declaration
});
```

Como forma especial de local, los parámetros también se pueden anotar como `readonly`. Esto no tendría ningún efecto en lo que el llamador del método puede pasar al parámetro (de la misma forma que no hay ninguna restricción sobre los valores que se pueden almacenar en un campo de `readonly`), pero como con cualquier `readonly` local, el compilador prohibiría que el código escribiera en el parámetro después de la declaración, lo que significa que el cuerpo del método no puede escribir en el parámetro

```csharp
public void Update(readonly int index = 0) // Default values are ok though not required
{
    ...
    index = 0; // Error: can't assign to readonly parameters
    ...
}
```

`readonly` parámetros no afectan a la firma o los metadatos emitidos por el compilador para ese método y simplemente afectan a la forma en que el compilador controla la compilación del cuerpo del método. Por lo tanto, por ejemplo, un método virtual base podría tener un parámetro `readonly`, y ese parámetro podría ser grabable en una invalidación.

Al igual que con los campos, `readonly` para variables locales y parámetros es superficial, lo que afecta a la ubicación de almacenamiento, pero no afecta de forma transitiva al gráfico de objetos. Sin embargo, al igual que con los campos, llamar a un método en un `readonly` struct local/parámetro realizará realmente una copia de la estructura y llamará al método en la copia, con el fin de evitar la mutación interna de `this`.

`readonly` variables locales y parámetros no se pueden pasar como argumentos `ref` o `out`, a menos que también se admita `ref readonly`.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

- `val` podría usarse como una forma abreviada alternativa a `let`.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- `readonly ref` / `ref readonly` / `readonly ref readonly`: he dejado la pregunta sobre cómo administrar `ref readonly` como independiente de esta propuesta.
- Esta propuesta no aborda Structs de solo lectura ni tipos inmutables. Se deja para una propuesta independiente.

## <a name="design-meetings"></a>Design Meetings

- Se describe brevemente el 21 de enero de 2015 (<https://github.com/dotnet/roslyn/issues/98>)
