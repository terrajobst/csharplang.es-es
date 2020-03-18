---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483611"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a>La indización de los campos `fixed` no debe requerir el anclaje, independientemente del contexto movible/unmovible. #

El cambio tiene el tamaño de una corrección de errores. Puede estar en 7,3 y no entra en conflicto con cualquier dirección que se tome más.
Este cambio es solo para permitir que el siguiente escenario funcione aunque se pueda mover `s`. Ya es válido cuando `s` no se va a mover. 

Nota: en cualquier caso, todavía requiere `unsafe` contexto. Es posible leer datos no inicializados o incluso fuera del intervalo. Que no cambia.

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

El "desafío" principal que veo aquí es cómo explicar la flexibilización en las especificaciones. En concreto, dado que lo siguiente seguiría necesitando el anclaje. (dado que `s` es movible y usamos explícitamente el campo como puntero)

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

Un motivo por el que se requiere el anclaje del destino cuando es movible es el artefacto de nuestra estrategia de generación de código,-siempre se convierte en un puntero no administrado y, por tanto, se obliga al usuario a anclar a través de `fixed` instrucción. Sin embargo, la conversión a no administrada no es necesaria cuando se realiza la indexación. La misma expresión matemática de puntero no seguro es igualmente aplicable cuando tenemos el receptor en forma de puntero administrado. Si lo hacemos, la referencia intermedia se administra (seguimiento de GC) y el anclaje no es necesario.

El cambio https://github.com/dotnet/roslyn/pull/24966 es una PR de prototipo que relaja este requisito.
