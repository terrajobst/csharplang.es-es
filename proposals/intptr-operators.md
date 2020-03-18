---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483551"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a><span data-ttu-id="f4a32-101">Los operadores deben exponerse para `System.IntPtr` y `System.UIntPtr`</span><span class="sxs-lookup"><span data-stu-id="f4a32-101">Operators should be exposed for `System.IntPtr` and `System.UIntPtr`</span></span>

* <span data-ttu-id="f4a32-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="f4a32-102">[x] Proposed</span></span>
* <span data-ttu-id="f4a32-103">[] Prototipo: no iniciado</span><span class="sxs-lookup"><span data-stu-id="f4a32-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="f4a32-104">[] Implementación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="f4a32-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="f4a32-105">[] Especificación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="f4a32-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="f4a32-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="f4a32-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="f4a32-107">CLR admite un conjunto de operadores para los tipos `System.IntPtr` y `System.UIntPtr` (`native int`).</span><span class="sxs-lookup"><span data-stu-id="f4a32-107">The CLR supports a set of operators for the `System.IntPtr` and `System.UIntPtr` types (`native int`).</span></span> <span data-ttu-id="f4a32-108">Estos operadores se pueden observar en `III.1.5` de la especificación de Common Language Infrastructure (`ECMA-335`).</span><span class="sxs-lookup"><span data-stu-id="f4a32-108">These operators can be seen in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="f4a32-109">Sin embargo, estos operadores no se admiten en C#.</span><span class="sxs-lookup"><span data-stu-id="f4a32-109">However, these operators are not supported by C#.</span></span>

<span data-ttu-id="f4a32-110">Se debe proporcionar compatibilidad con el lenguaje para el conjunto completo de operadores admitidos por `System.IntPtr` y `System.UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="f4a32-110">Language support should be provided for the full set of operators supported by `System.IntPtr` and `System.UIntPtr`.</span></span> <span data-ttu-id="f4a32-111">Estos operadores son: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft``ShiftRight`.</span><span class="sxs-lookup"><span data-stu-id="f4a32-111">These operators are: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span></span>

## <a name="motivation"></a><span data-ttu-id="f4a32-112">Motivación</span><span class="sxs-lookup"><span data-stu-id="f4a32-112">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="f4a32-113">En la actualidad, los usuarios C# pueden escribir fácilmente aplicaciones destinadas a varias plataformas mediante diversas herramientas y marcos de trabajo, como `Xamarin`, `.NET Core`, `Mono`, etc.</span><span class="sxs-lookup"><span data-stu-id="f4a32-113">Today, users can easily write C# applications targeting multiple platforms using various tools and frameworks, such as: `Xamarin`, `.NET Core`, `Mono`, etc...</span></span>

<span data-ttu-id="f4a32-114">Al escribir código multiplataforma, a menudo es necesario escribir código de interoperabilidad que interactúe con una plataforma de destino concreta de una manera determinada.</span><span class="sxs-lookup"><span data-stu-id="f4a32-114">When writing cross-platform code, it is often necessary to write interop code that interacts with a particular target platform in a specific manner.</span></span> <span data-ttu-id="f4a32-115">Esto podría incluir la escritura de código de gráficos, la llamada a algunas API del sistema o la interacción con una biblioteca nativa existente.</span><span class="sxs-lookup"><span data-stu-id="f4a32-115">This could include writing graphics code, calling some System API, or interacting with an existing native library.</span></span>

<span data-ttu-id="f4a32-116">A menudo, este código de interoperabilidad tiene que tratar con los identificadores, la memoria no administrada o incluso los enteros con un tamaño específico de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="f4a32-116">This interop code often has to deal with handles, unmanaged memory, or even just platform-specific sized integers.</span></span>

<span data-ttu-id="f4a32-117">El tiempo de ejecución proporciona compatibilidad para esto definiendo un conjunto de operadores que se pueden utilizar en los tipos primitivos `native int` (`System.IntPtr`) y `native unsigned int` (`System.UIntPtr`).</span><span class="sxs-lookup"><span data-stu-id="f4a32-117">The runtime provides support for this by defining a set of operators that can be used on the `native int` (`System.IntPtr`) and `native unsigned int` (`System.UIntPtr`) primitive types.</span></span>

<span data-ttu-id="f4a32-118">C#nunca ha admitido estos operadores y, por tanto, los usuarios deben solucionar el problema.</span><span class="sxs-lookup"><span data-stu-id="f4a32-118">C# has never supported these operators and so users have to work around the issue.</span></span> <span data-ttu-id="f4a32-119">Esto a menudo aumenta la complejidad del código y reduce la facilidad de mantenimiento del código.</span><span class="sxs-lookup"><span data-stu-id="f4a32-119">This often increases code complexity and lowers code maintainability.</span></span>

<span data-ttu-id="f4a32-120">Como tal, el lenguaje debe empezar a admitir estos operadores para ayudar a avanzar en el lenguaje para admitir mejor estos requisitos.</span><span class="sxs-lookup"><span data-stu-id="f4a32-120">As such, the language should begin to support these operators to help advance the language to better support these requirements.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f4a32-121">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="f4a32-121">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="f4a32-122">El conjunto completo de operadores admitidos se define en `III.1.5` de la especificación de Common Language Infrastructure (`ECMA-335`).</span><span class="sxs-lookup"><span data-stu-id="f4a32-122">The full set of operators supported are defined in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="f4a32-123">La especificación está disponible aquí: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span><span class="sxs-lookup"><span data-stu-id="f4a32-123">The specification is available here: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span></span>

* <span data-ttu-id="f4a32-124">A continuación se proporciona un resumen de los operadores para mayor comodidad.</span><span class="sxs-lookup"><span data-stu-id="f4a32-124">A summary of the operators is provided below for convenience.</span></span>
* <span data-ttu-id="f4a32-125">Los operadores no comprobables definidos por la especificación de la CLI no aparecen en la lista y actualmente no forman parte de esta propuesta (aunque puede que merezca la pena considerar esto también).</span><span class="sxs-lookup"><span data-stu-id="f4a32-125">The unverifiable operators defined by the CLI spec are not listed and are not currently part of this proposal (although it may be worth considering these as well).</span></span>
* <span data-ttu-id="f4a32-126">Proporcionar una palabra clave (como `nint` y `nuint`) ni proporcionar una manera de declarar los literales para `System.IntPtr` y `System.UIntPtr` (como 0n () no forma parte de esta propuesta (aunque puede que merezca la pena considerar esto también).</span><span class="sxs-lookup"><span data-stu-id="f4a32-126">Providing a keyword (such as `nint` and `nuint`) nor providing a way to for literals to be declared for `System.IntPtr` and `System.UIntPtr` (such as 0n) is not part of this proposal (although it may be worth considering these as well).</span></span>

### <a name="unary-plus-operator"></a><span data-ttu-id="f4a32-127">Operador unario más</span><span class="sxs-lookup"><span data-stu-id="f4a32-127">Unary Plus Operator</span></span>

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a><span data-ttu-id="f4a32-128">Operador unario menos</span><span class="sxs-lookup"><span data-stu-id="f4a32-128">Unary Minus Operator</span></span>

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a><span data-ttu-id="f4a32-129">Operador de complemento bit a bit</span><span class="sxs-lookup"><span data-stu-id="f4a32-129">Bitwise Complement Operator</span></span>

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a><span data-ttu-id="f4a32-130">Operadores de conversión</span><span class="sxs-lookup"><span data-stu-id="f4a32-130">Cast Operators</span></span>

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

### <a name="multiplication-operator"></a><span data-ttu-id="f4a32-131">Operador de multiplicación</span><span class="sxs-lookup"><span data-stu-id="f4a32-131">Multiplication Operator</span></span>

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

### <a name="division-operator"></a><span data-ttu-id="f4a32-132">Operador de división</span><span class="sxs-lookup"><span data-stu-id="f4a32-132">Division Operator</span></span>

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

### <a name="remainder-operator"></a><span data-ttu-id="f4a32-133">Resto (operador)</span><span class="sxs-lookup"><span data-stu-id="f4a32-133">Remainder Operator</span></span>

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

### <a name="addition-operator"></a><span data-ttu-id="f4a32-134">Operador de suma</span><span class="sxs-lookup"><span data-stu-id="f4a32-134">Addition Operator</span></span>

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

### <a name="subtraction-operator"></a><span data-ttu-id="f4a32-135">Operador de resta</span><span class="sxs-lookup"><span data-stu-id="f4a32-135">Subtraction Operator</span></span>

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

### <a name="shift-operators"></a><span data-ttu-id="f4a32-136">Operadores de desplazamiento</span><span class="sxs-lookup"><span data-stu-id="f4a32-136">Shift Operators</span></span>

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a><span data-ttu-id="f4a32-137">Operadores de comparación de enteros</span><span class="sxs-lookup"><span data-stu-id="f4a32-137">Integer Comparison Operators</span></span>

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

### <a name="integer-logical-operators"></a><span data-ttu-id="f4a32-138">Operadores lógicos enteros</span><span class="sxs-lookup"><span data-stu-id="f4a32-138">Integer Logical Operators</span></span>

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

## <a name="drawbacks"></a><span data-ttu-id="f4a32-139">Desventajas</span><span class="sxs-lookup"><span data-stu-id="f4a32-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="f4a32-140">El uso real de estos operadores puede ser pequeño y estar limitado a los usuarios finales que escriben bibliotecas de nivel inferior o código de interoperabilidad.</span><span class="sxs-lookup"><span data-stu-id="f4a32-140">The actual use of these operators may be small and limited to end-users who are writing lower level libraries or interop code.</span></span> <span data-ttu-id="f4a32-141">La mayoría de los usuarios finales probablemente estaban consumiendo estas bibliotecas de nivel inferior, que tendrían los enteros, identificadores y códigos de interoperabilidad de tamaño nativo abstraedos.</span><span class="sxs-lookup"><span data-stu-id="f4a32-141">Most end-users would likely be consuming these lower level libraries themselves which would have the native sized integers, handles, and interop code abstracted away.</span></span> <span data-ttu-id="f4a32-142">Por lo tanto, no necesitarían los propios operadores.</span><span class="sxs-lookup"><span data-stu-id="f4a32-142">As such, they would not have need of the operators themselves.</span></span>

## <a name="alternatives"></a><span data-ttu-id="f4a32-143">Alternativas</span><span class="sxs-lookup"><span data-stu-id="f4a32-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="f4a32-144">Haga que el marco de trabajo implemente los operadores necesarios escribiéndolo directamente en IL.</span><span class="sxs-lookup"><span data-stu-id="f4a32-144">Have the framework implement the required operators by writing them directly in IL.</span></span> <span data-ttu-id="f4a32-145">Además, el tiempo de ejecución podría proporcionar compatibilidad intrínseca para los operadores definidos por el marco de trabajo, para optimizar mejor el rendimiento final.</span><span class="sxs-lookup"><span data-stu-id="f4a32-145">Additionally, the runtime could provide intrinsic support for the operators defined by the framework, so as to better optimize the end performance.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="f4a32-146">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="f4a32-146">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="f4a32-147">¿Qué partes del diseño siguen siendo TBD?</span><span class="sxs-lookup"><span data-stu-id="f4a32-147">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="f4a32-148">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="f4a32-148">Design meetings</span></span>

<span data-ttu-id="f4a32-149">Vínculo a las notas de diseño que afectan a esta propuesta y describa en una oración cada uno de los cambios que han producido.</span><span class="sxs-lookup"><span data-stu-id="f4a32-149">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>