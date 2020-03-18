---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483575"
---
# <a name="fixed-sized-buffers"></a>Búferes de tamaño fijo

* [x] propuesto
* [] Prototipo: no iniciado
* [] Implementación: no iniciada
* [] Especificación: no iniciada

## <a name="summary"></a>Resumen
[summary]: #summary

Proporcionan un mecanismo de uso general y seguro para declarar búferes de tamaño fijo en el C# lenguaje.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Hoy en día, los usuarios tienen la capacidad de crear búferes de tamaño fijo en un contexto no seguro. Sin embargo, esto requiere que el usuario trate con punteros, realice comprobaciones de límites manualmente y solo admita un conjunto limitado de tipos (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`y `double`).

La queja más común es que los búferes de tamaño fijo no se pueden indizar en código seguro. La imposibilidad de usar más tipos es el segundo.

Con unos pocos ajustes menores, podríamos proporcionar búferes de tamaño fijo de uso general que admitan cualquier tipo, se puede usar en un contexto seguro y hacer que se realice una comprobación automática de los límites.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Uno declararía un búfer de tamaño fijo seguro a través de lo siguiente:

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

El compilador traducirá la declaración en una representación interna similar a la siguiente

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

Dado que estos búferes de tamaño fijo ya no requieren el uso de `fixed`, tiene sentido permitir cualquier tipo de elemento.  

> Nota: se seguirá admitiendo `fixed`, pero solo si el tipo de elemento es `blittable`

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

* Podría haber algunos desafíos con la compatibilidad con versiones anteriores, pero dado que los búferes de tamaño fijo existentes solo funcionan con una selección de tipos primitivos, es posible que el compilador continúe "Just-Working" si el usuario trata el búfer fijo como un puntero.
* Las construcciones incompatibles pueden necesitar usar una codificación de `v2` ligeramente diferente para ocultar los campos del compilador anterior.
* El empaquetado no está bien definido en la especificación de IL para tipos genéricos. Aunque el enfoque debería funcionar, se aplicará un borde a un comportamiento no documentado. Debemos hacer esto documentado y asegurarnos de que otros JITs como mono tienen el mismo comportamiento.
* Si se especifica un tipo independiente para cada longitud (posiblemente otro para los campos `readonly`, si se admiten), afectará a los metadatos. Estará limitado por el número de matrices de distintos tamaños en la aplicación especificada.
* `ref` Math no es comprobable formalmente (ya que no es seguro). Necesitamos encontrar una manera de actualizar las reglas de comprobación para saber que nuestro uso es correcto.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Declare manualmente las estructuras y use código no seguro para construir indexadores.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- ¿se debe permitir `readonly`?  (con indizador de solo lectura)
- ¿se deben permitir los inicializadores de matriz?
- ¿es `fixed` palabra clave necesaria?
- `foreach`?
- ¿solo campos de instancia en Structs?

## <a name="design-meetings"></a>Design Meetings

Vínculo a las notas de diseño que afectan a esta propuesta y describa en una oración cada uno de los cambios que han producido.