---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483551"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a>Los operadores deben exponerse para `System.IntPtr` y `System.UIntPtr`

* [x] propuesto
* [] Prototipo: no iniciado
* [] Implementación: no iniciada
* [] Especificación: no iniciada

## <a name="summary"></a>Resumen
[summary]: #summary

CLR admite un conjunto de operadores para los tipos `System.IntPtr` y `System.UIntPtr` (`native int`). Estos operadores se pueden observar en `III.1.5` de la especificación de Common Language Infrastructure (`ECMA-335`). Sin embargo, estos operadores no se admiten en C#.

Se debe proporcionar compatibilidad con el lenguaje para el conjunto completo de operadores admitidos por `System.IntPtr` y `System.UIntPtr`. Estos operadores son: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft``ShiftRight`.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

En la actualidad, los usuarios C# pueden escribir fácilmente aplicaciones destinadas a varias plataformas mediante diversas herramientas y marcos de trabajo, como `Xamarin`, `.NET Core`, `Mono`, etc.

Al escribir código multiplataforma, a menudo es necesario escribir código de interoperabilidad que interactúe con una plataforma de destino concreta de una manera determinada. Esto podría incluir la escritura de código de gráficos, la llamada a algunas API del sistema o la interacción con una biblioteca nativa existente.

A menudo, este código de interoperabilidad tiene que tratar con los identificadores, la memoria no administrada o incluso los enteros con un tamaño específico de la plataforma.

El tiempo de ejecución proporciona compatibilidad para esto definiendo un conjunto de operadores que se pueden utilizar en los tipos primitivos `native int` (`System.IntPtr`) y `native unsigned int` (`System.UIntPtr`).

C#nunca ha admitido estos operadores y, por tanto, los usuarios deben solucionar el problema. Esto a menudo aumenta la complejidad del código y reduce la facilidad de mantenimiento del código.

Como tal, el lenguaje debe empezar a admitir estos operadores para ayudar a avanzar en el lenguaje para admitir mejor estos requisitos.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

El conjunto completo de operadores admitidos se define en `III.1.5` de la especificación de Common Language Infrastructure (`ECMA-335`). La especificación está disponible aquí: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).

* A continuación se proporciona un resumen de los operadores para mayor comodidad.
* Los operadores no comprobables definidos por la especificación de la CLI no aparecen en la lista y actualmente no forman parte de esta propuesta (aunque puede que merezca la pena considerar esto también).
* Proporcionar una palabra clave (como `nint` y `nuint`) ni proporcionar una manera de declarar los literales para `System.IntPtr` y `System.UIntPtr` (como 0n () no forma parte de esta propuesta (aunque puede que merezca la pena considerar esto también).

### <a name="unary-plus-operator"></a>Operador unario más

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a>Operador unario menos

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a>Operador de complemento bit a bit

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a>Operadores de conversión

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a>Operador de multiplicación

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a>Operador de división

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a>Resto (operador)

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a>Operador de suma

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a>Operador de resta

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a>Operadores de desplazamiento

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a>Operadores de comparación de enteros

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a>Operadores lógicos enteros

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

El uso real de estos operadores puede ser pequeño y estar limitado a los usuarios finales que escriben bibliotecas de nivel inferior o código de interoperabilidad. La mayoría de los usuarios finales probablemente estaban consumiendo estas bibliotecas de nivel inferior, que tendrían los enteros, identificadores y códigos de interoperabilidad de tamaño nativo abstraedos. Por lo tanto, no necesitarían los propios operadores.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Haga que el marco de trabajo implemente los operadores necesarios escribiéndolo directamente en IL. Además, el tiempo de ejecución podría proporcionar compatibilidad intrínseca para los operadores definidos por el marco de trabajo, para optimizar mejor el rendimiento final.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

¿Qué partes del diseño siguen siendo TBD?

## <a name="design-meetings"></a>Design Meetings

Vínculo a las notas de diseño que afectan a esta propuesta y describa en una oración cada uno de los cambios que han producido.