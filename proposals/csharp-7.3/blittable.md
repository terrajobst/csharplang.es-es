---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483605"
---
# <a name="unmanaged-type-constraint"></a>Restricción de tipo no administrado

## <a name="summary"></a>Resumen
[summary]: #summary

La característica de restricción no administrada proporcionará el cumplimiento del lenguaje a la clase de tipos conocidos como "tipos no administrados C# " en la especificación del lenguaje.  Esto se define en la sección 18,2 como un tipo que no es un tipo de referencia y no contiene campos de tipo de referencia en ningún nivel de anidamiento.  

## <a name="motivation"></a>Motivación
[motivation]: #motivation

La motivación principal es facilitar la creación de código de interoperabilidad de bajo C#nivel en. Los tipos no administrados son uno de los bloques de creación principales del código de interoperabilidad, pero la falta de compatibilidad en genéricos hace que sea imposible crear rutinas reutilizables en todos los tipos no administrados. En su lugar, se obliga a los desarrolladores a crear el mismo código de placa de caldera para cada tipo no administrado en su biblioteca:

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

Para habilitar este tipo de escenario, el lenguaje introducirá una nueva restricción:

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

Esta restricción solo se puede cumplir con los tipos que caben en la definición de tipo no administrado C# en la especificación del lenguaje. Otra manera de examinarlo es que un tipo satisface la restricción no administrada IFF; también se puede usar como un puntero. 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

Los parámetros de tipo con la restricción no administrada pueden usar todas las características disponibles para los tipos no administrados: punteros, fijos, etc... 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

Esta restricción también hará posible tener conversiones eficaces entre los datos estructurados y los flujos de bytes. Se trata de una operación que es habitual en las pilas de red y en las capas de serialización:

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

Estas rutinas son ventajosas porque son provably seguras en tiempo de compilación y la asignación es gratuita.  Los autores de interoperabilidad hoy no pueden hacerlo (aunque se encuentra en una capa en la que el rendimiento es crítico).  En su lugar, deben confiar en la asignación de rutinas que tienen comprobaciones en tiempo de ejecución costosas para comprobar que los valores se han desadministrado correctamente.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

El lenguaje introducirá una nueva restricción denominada `unmanaged`. Para satisfacer esta restricción, un tipo debe ser una estructura y todos los campos del tipo deben corresponder a una de las siguientes categorías:

- Tienen el tipo `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` o `UIntPtr`.
- Ser cualquier tipo de `enum`.
- Ser un tipo de puntero.
- Ser un struct definido por el usuario que satsifies la restricción `unmanaged`.

Los campos de instancia generados por el compilador, como los que respaldan las propiedades implementadas automáticamente, también deben cumplir estas restricciones. 

Por ejemplo:

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

La restricción `unmanaged` no puede combinarse con `struct`, `class` o `new()`. Esta restricción se deriva del hecho de que `unmanaged` implica `struct` por lo que las otras restricciones no tienen sentido.

CLR no aplica la restricción `unmanaged`, solo en el lenguaje. Para evitar que otros lenguajes lo utilicen, los métodos que tienen esta restricción estarán protegidos por mod-req. Esto impedirá que otros lenguajes usen argumentos de tipo que no son tipos no administrados.

El token `unmanaged` en la restricción no es una palabra clave ni una palabra clave contextual. En su lugar, es como `var` en que se evalúa en esa ubicación y:

- Enlazar a un tipo definido por el usuario o al que se hace referencia llamado `unmanaged`: se tratará como cualquier otra restricción de tipo con nombre que se trate. 
- Enlazar a ningún tipo: se interpretará como la restricción `unmanaged`.

En el caso de que haya un tipo denominado `unmanaged` y esté disponible sin calificación en el contexto actual, no habrá forma de usar la restricción `unmanaged`. Esto se asemeja a las reglas que rodean el `var` de características y los tipos definidos por el usuario del mismo nombre. 

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

El principal inconveniente de esta característica es que sirve a un pequeño número de desarrolladores: normalmente los autores o marcos de la biblioteca de bajo nivel.  Por lo tanto, se pierde un tiempo de lenguaje precioso para un pequeño número de desarrolladores. 

Sin embargo, estos marcos de trabajo suelen ser la base de la mayoría de las aplicaciones .NET.  Por lo tanto, el rendimiento y la exactitud de WINS en este nivel pueden tener un efecto de rizo en el ecosistema de .NET.  Esto hace que la característica merezca la pena considerar incluso con la audiencia limitada.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Hay un par de alternativas a tener en cuenta:

- El estado quo: la característica no está justificada por sus propios méritos y los desarrolladores siguen usando el comportamiento de participación implícita.

## <a name="questions"></a>Preguntas
[quesions]: #questions

### <a name="metadata-representation"></a>Representación de metadatos

El F# idioma codifica la restricción en el archivo de signatura, lo que C# significa que no puede volver a usar su representación. Se debe elegir un nuevo atributo para esta restricción. Además, un método que tiene esta restricción debe estar protegido por mod-req.

### <a name="blittable-vs-unmanaged"></a>Bytes entre bits y no administrados
El F# lenguaje tiene una [característica muy similar](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) que usa la palabra clave Unmanaged. El nombre que se va a representar como bits/bytes proviene del uso en Midori.  Puede que le interese buscar la precedencia aquí y usar en su lugar no administrada. 

**Solución** de El lenguaje que decide usar no administrado 

### <a name="verifier"></a>Comprobador

¿Es necesario actualizar el comprobador/tiempo de ejecución para comprender el uso de punteros a parámetros de tipo genérico?  ¿O puede simplemente trabajar como está sin cambios?

**Solución** de No se necesitan cambios. Todos los tipos de puntero son simplemente no comprobables. 

## <a name="design-meetings"></a>Design Meetings

N/D
