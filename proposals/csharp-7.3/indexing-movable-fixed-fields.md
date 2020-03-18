---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483611"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a><span data-ttu-id="fc291-101">La indización de los campos `fixed` no debe requerir el anclaje, independientemente del contexto movible/unmovible.</span><span class="sxs-lookup"><span data-stu-id="fc291-101">Indexing `fixed` fields should not require pinning regardless of the movable/unmovable context.</span></span> #

<span data-ttu-id="fc291-102">El cambio tiene el tamaño de una corrección de errores.</span><span class="sxs-lookup"><span data-stu-id="fc291-102">The change has the size of a bug fix.</span></span> <span data-ttu-id="fc291-103">Puede estar en 7,3 y no entra en conflicto con cualquier dirección que se tome más.</span><span class="sxs-lookup"><span data-stu-id="fc291-103">It can be in 7.3 and does not conflict with whatever direction we take further.</span></span>
<span data-ttu-id="fc291-104">Este cambio es solo para permitir que el siguiente escenario funcione aunque se pueda mover `s`.</span><span class="sxs-lookup"><span data-stu-id="fc291-104">This change is only about allowing the following scenario to work even though `s` is moveable.</span></span> <span data-ttu-id="fc291-105">Ya es válido cuando `s` no se va a mover.</span><span class="sxs-lookup"><span data-stu-id="fc291-105">It is already valid when `s` is not moveable.</span></span> 

<span data-ttu-id="fc291-106">Nota: en cualquier caso, todavía requiere `unsafe` contexto.</span><span class="sxs-lookup"><span data-stu-id="fc291-106">NOTE: in either case, it still requires `unsafe` context.</span></span> <span data-ttu-id="fc291-107">Es posible leer datos no inicializados o incluso fuera del intervalo.</span><span class="sxs-lookup"><span data-stu-id="fc291-107">It is possible to read uninitialized data or even out of range.</span></span> <span data-ttu-id="fc291-108">Que no cambia.</span><span class="sxs-lookup"><span data-stu-id="fc291-108">That is not changing.</span></span>

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

<span data-ttu-id="fc291-109">El "desafío" principal que veo aquí es cómo explicar la flexibilización en las especificaciones. En concreto, dado que lo siguiente seguiría necesitando el anclaje.</span><span class="sxs-lookup"><span data-stu-id="fc291-109">The main “challenge” that I see here is how to explain the relaxation in the spec. In particular, since the following would still need pinning.</span></span> <span data-ttu-id="fc291-110">(dado que `s` es movible y usamos explícitamente el campo como puntero)</span><span class="sxs-lookup"><span data-stu-id="fc291-110">(because `s` is moveable and we explicitly use the field as a pointer)</span></span>

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

<span data-ttu-id="fc291-111">Un motivo por el que se requiere el anclaje del destino cuando es movible es el artefacto de nuestra estrategia de generación de código,-siempre se convierte en un puntero no administrado y, por tanto, se obliga al usuario a anclar a través de `fixed` instrucción.</span><span class="sxs-lookup"><span data-stu-id="fc291-111">One reason why we require pinning of the target when it is movable is the artifact of our code generation strategy, - we always convert to an unmanaged pointer and thus force the user to pin via `fixed` statement.</span></span> <span data-ttu-id="fc291-112">Sin embargo, la conversión a no administrada no es necesaria cuando se realiza la indexación.</span><span class="sxs-lookup"><span data-stu-id="fc291-112">However, conversion to unmanaged is unnecessary when doing indexing.</span></span> <span data-ttu-id="fc291-113">La misma expresión matemática de puntero no seguro es igualmente aplicable cuando tenemos el receptor en forma de puntero administrado.</span><span class="sxs-lookup"><span data-stu-id="fc291-113">The same unsafe pointer math is equally applicable when we have the receiver in the form of a managed pointer.</span></span> <span data-ttu-id="fc291-114">Si lo hacemos, la referencia intermedia se administra (seguimiento de GC) y el anclaje no es necesario.</span><span class="sxs-lookup"><span data-stu-id="fc291-114">If we do that, then the intermediate ref is managed (GC-tracked) and the pinning is unnecessary.</span></span>

<span data-ttu-id="fc291-115">El cambio https://github.com/dotnet/roslyn/pull/24966 es una PR de prototipo que relaja este requisito.</span><span class="sxs-lookup"><span data-stu-id="fc291-115">The change https://github.com/dotnet/roslyn/pull/24966 is a prototype PR that relaxes this requirement.</span></span>
