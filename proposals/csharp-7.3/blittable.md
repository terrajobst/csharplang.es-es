---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483605"
---
# <a name="unmanaged-type-constraint"></a><span data-ttu-id="f4330-101">Restricción de tipo no administrado</span><span class="sxs-lookup"><span data-stu-id="f4330-101">Unmanaged type constraint</span></span>

## <a name="summary"></a><span data-ttu-id="f4330-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="f4330-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="f4330-103">La característica de restricción no administrada proporcionará el cumplimiento del lenguaje a la clase de tipos conocidos como "tipos no administrados C# " en la especificación del lenguaje.  Esto se define en la sección 18,2 como un tipo que no es un tipo de referencia y no contiene campos de tipo de referencia en ningún nivel de anidamiento.</span><span class="sxs-lookup"><span data-stu-id="f4330-103">The unmanaged constraint feature will give language enforcement to the class of types known as "unmanaged types" in the C# language spec.  This is defined in section 18.2 as a type which is not a reference type and doesn't contain reference type fields at any level of nesting.</span></span>  

## <a name="motivation"></a><span data-ttu-id="f4330-104">Motivación</span><span class="sxs-lookup"><span data-stu-id="f4330-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="f4330-105">La motivación principal es facilitar la creación de código de interoperabilidad de bajo C#nivel en.</span><span class="sxs-lookup"><span data-stu-id="f4330-105">The primary motivation is to make it easier to author low level interop code in C#.</span></span> <span data-ttu-id="f4330-106">Los tipos no administrados son uno de los bloques de creación principales del código de interoperabilidad, pero la falta de compatibilidad en genéricos hace que sea imposible crear rutinas reutilizables en todos los tipos no administrados.</span><span class="sxs-lookup"><span data-stu-id="f4330-106">Unmanaged types are one of the core building blocks for interop code, yet the lack of support in generics makes it impossible to create re-usable routines across all unmanaged types.</span></span> <span data-ttu-id="f4330-107">En su lugar, se obliga a los desarrolladores a crear el mismo código de placa de caldera para cada tipo no administrado en su biblioteca:</span><span class="sxs-lookup"><span data-stu-id="f4330-107">Instead developers are forced to author the same boiler plate code for every unmanaged type in their library:</span></span>

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

<span data-ttu-id="f4330-108">Para habilitar este tipo de escenario, el lenguaje introducirá una nueva restricción:</span><span class="sxs-lookup"><span data-stu-id="f4330-108">To enable this type of scenario the language will be introducing a new constraint: unmanaged:</span></span>

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

<span data-ttu-id="f4330-109">Esta restricción solo se puede cumplir con los tipos que caben en la definición de tipo no administrado C# en la especificación del lenguaje. Otra manera de examinarlo es que un tipo satisface la restricción no administrada IFF; también se puede usar como un puntero.</span><span class="sxs-lookup"><span data-stu-id="f4330-109">This constraint can only be met by types which fit into the unmanaged type definition in the C# language spec. Another way of looking at it is that a type satisfies the unmanaged constraint iff it can also be used as a pointer.</span></span> 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

<span data-ttu-id="f4330-110">Los parámetros de tipo con la restricción no administrada pueden usar todas las características disponibles para los tipos no administrados: punteros, fijos, etc...</span><span class="sxs-lookup"><span data-stu-id="f4330-110">Type parameters with the unmanaged constraint can use all the features available to unmanaged types: pointers, fixed, etc ...</span></span> 

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

<span data-ttu-id="f4330-111">Esta restricción también hará posible tener conversiones eficaces entre los datos estructurados y los flujos de bytes.</span><span class="sxs-lookup"><span data-stu-id="f4330-111">This constraint will also make it possible to have efficient conversions between structured data and streams of bytes.</span></span> <span data-ttu-id="f4330-112">Se trata de una operación que es habitual en las pilas de red y en las capas de serialización:</span><span class="sxs-lookup"><span data-stu-id="f4330-112">This is an operation that is common in networking stacks and serialization layers:</span></span>

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

<span data-ttu-id="f4330-113">Estas rutinas son ventajosas porque son provably seguras en tiempo de compilación y la asignación es gratuita.</span><span class="sxs-lookup"><span data-stu-id="f4330-113">Such routines are advantageous because they are provably safe at compile time and allocation free.</span></span>  <span data-ttu-id="f4330-114">Los autores de interoperabilidad hoy no pueden hacerlo (aunque se encuentra en una capa en la que el rendimiento es crítico).</span><span class="sxs-lookup"><span data-stu-id="f4330-114">Interop authors today can not do this (even though it's at a layer where perf is critical).</span></span>  <span data-ttu-id="f4330-115">En su lugar, deben confiar en la asignación de rutinas que tienen comprobaciones en tiempo de ejecución costosas para comprobar que los valores se han desadministrado correctamente.</span><span class="sxs-lookup"><span data-stu-id="f4330-115">Instead they need to rely on allocating routines that have expensive runtime checks to verify values are correctly unmanaged.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="f4330-116">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="f4330-116">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="f4330-117">El lenguaje introducirá una nueva restricción denominada `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="f4330-117">The language will introduce a new constraint named `unmanaged`.</span></span> <span data-ttu-id="f4330-118">Para satisfacer esta restricción, un tipo debe ser una estructura y todos los campos del tipo deben corresponder a una de las siguientes categorías:</span><span class="sxs-lookup"><span data-stu-id="f4330-118">In order to satisfy this constraint a type must be a struct and all the fields of the type must fall into one of the following categories:</span></span>

- <span data-ttu-id="f4330-119">Tienen el tipo `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` o `UIntPtr`.</span><span class="sxs-lookup"><span data-stu-id="f4330-119">Have the type `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` or `UIntPtr`.</span></span>
- <span data-ttu-id="f4330-120">Ser cualquier tipo de `enum`.</span><span class="sxs-lookup"><span data-stu-id="f4330-120">Be any `enum` type.</span></span>
- <span data-ttu-id="f4330-121">Ser un tipo de puntero.</span><span class="sxs-lookup"><span data-stu-id="f4330-121">Be a pointer type.</span></span>
- <span data-ttu-id="f4330-122">Ser un struct definido por el usuario que satsifies la restricción `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="f4330-122">Be a user defined struct that satsifies the `unmanaged` constraint.</span></span>

<span data-ttu-id="f4330-123">Los campos de instancia generados por el compilador, como los que respaldan las propiedades implementadas automáticamente, también deben cumplir estas restricciones.</span><span class="sxs-lookup"><span data-stu-id="f4330-123">Compiler generated instance fields, such as those backing auto-implemented properties, must also meet these constraints.</span></span> 

<span data-ttu-id="f4330-124">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f4330-124">For example:</span></span>

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

<span data-ttu-id="f4330-125">La restricción `unmanaged` no puede combinarse con `struct`, `class` o `new()`.</span><span class="sxs-lookup"><span data-stu-id="f4330-125">The `unmanaged` constraint cannot be combined with `struct`, `class` or `new()`.</span></span> <span data-ttu-id="f4330-126">Esta restricción se deriva del hecho de que `unmanaged` implica `struct` por lo que las otras restricciones no tienen sentido.</span><span class="sxs-lookup"><span data-stu-id="f4330-126">This restriction derives from the fact that `unmanaged` implies `struct` hence the other constraints do not make sense.</span></span>

<span data-ttu-id="f4330-127">CLR no aplica la restricción `unmanaged`, solo en el lenguaje.</span><span class="sxs-lookup"><span data-stu-id="f4330-127">The `unmanaged` constraint is not enforced by CLR, only by the language.</span></span> <span data-ttu-id="f4330-128">Para evitar que otros lenguajes lo utilicen, los métodos que tienen esta restricción estarán protegidos por mod-req. Esto impedirá que otros lenguajes usen argumentos de tipo que no son tipos no administrados.</span><span class="sxs-lookup"><span data-stu-id="f4330-128">To prevent mis-use by other languages, methods which have this constraint will be protected by a mod-req. This will prevent other languages from using type arguments which are not unmanaged types.</span></span>

<span data-ttu-id="f4330-129">El token `unmanaged` en la restricción no es una palabra clave ni una palabra clave contextual.</span><span class="sxs-lookup"><span data-stu-id="f4330-129">The token `unmanaged` in the constraint is not a keyword, nor a contextual keyword.</span></span> <span data-ttu-id="f4330-130">En su lugar, es como `var` en que se evalúa en esa ubicación y:</span><span class="sxs-lookup"><span data-stu-id="f4330-130">Instead it is like `var` in that it is evaluated at that location and will either:</span></span>

- <span data-ttu-id="f4330-131">Enlazar a un tipo definido por el usuario o al que se hace referencia llamado `unmanaged`: se tratará como cualquier otra restricción de tipo con nombre que se trate.</span><span class="sxs-lookup"><span data-stu-id="f4330-131">Bind to user defined or referenced type named `unmanaged`: This will be treated just as any other named type constraint is treated.</span></span> 
- <span data-ttu-id="f4330-132">Enlazar a ningún tipo: se interpretará como la restricción `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="f4330-132">Bind to no type: This will be interpreted as the `unmanaged` constraint.</span></span>

<span data-ttu-id="f4330-133">En el caso de que haya un tipo denominado `unmanaged` y esté disponible sin calificación en el contexto actual, no habrá forma de usar la restricción `unmanaged`.</span><span class="sxs-lookup"><span data-stu-id="f4330-133">In the case there is a type named `unmanaged` and it is available without qualification in the current context, then there will be no way to use the `unmanaged` constraint.</span></span> <span data-ttu-id="f4330-134">Esto se asemeja a las reglas que rodean el `var` de características y los tipos definidos por el usuario del mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="f4330-134">This parallels the rules surrounding the feature `var` and user defined types of the same name.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="f4330-135">Desventajas</span><span class="sxs-lookup"><span data-stu-id="f4330-135">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="f4330-136">El principal inconveniente de esta característica es que sirve a un pequeño número de desarrolladores: normalmente los autores o marcos de la biblioteca de bajo nivel.</span><span class="sxs-lookup"><span data-stu-id="f4330-136">The primary drawback of this feature is that it serves a small number of developers: typically low level library authors or frameworks.</span></span>  <span data-ttu-id="f4330-137">Por lo tanto, se pierde un tiempo de lenguaje precioso para un pequeño número de desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="f4330-137">Hence it's spending precious language time for a small number of developers.</span></span> 

<span data-ttu-id="f4330-138">Sin embargo, estos marcos de trabajo suelen ser la base de la mayoría de las aplicaciones .NET.</span><span class="sxs-lookup"><span data-stu-id="f4330-138">Yet these frameworks are often the basis for the majority of .NET applications out there.</span></span>  <span data-ttu-id="f4330-139">Por lo tanto, el rendimiento y la exactitud de WINS en este nivel pueden tener un efecto de rizo en el ecosistema de .NET.</span><span class="sxs-lookup"><span data-stu-id="f4330-139">Hence performance / correctness wins at this level can have a ripple effect on the .NET ecosystem.</span></span>  <span data-ttu-id="f4330-140">Esto hace que la característica merezca la pena considerar incluso con la audiencia limitada.</span><span class="sxs-lookup"><span data-stu-id="f4330-140">This makes the feature worth considering even with the limited audience.</span></span>

## <a name="alternatives"></a><span data-ttu-id="f4330-141">Alternativas</span><span class="sxs-lookup"><span data-stu-id="f4330-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="f4330-142">Hay un par de alternativas a tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="f4330-142">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="f4330-143">El estado quo: la característica no está justificada por sus propios méritos y los desarrolladores siguen usando el comportamiento de participación implícita.</span><span class="sxs-lookup"><span data-stu-id="f4330-143">The status quo:  The feature is not justified on its own merits and developers continue to use the implicit opt in behavior.</span></span>

## <a name="questions"></a><span data-ttu-id="f4330-144">Preguntas</span><span class="sxs-lookup"><span data-stu-id="f4330-144">Questions</span></span>
[quesions]: #questions

### <a name="metadata-representation"></a><span data-ttu-id="f4330-145">Representación de metadatos</span><span class="sxs-lookup"><span data-stu-id="f4330-145">Metadata Representation</span></span>

<span data-ttu-id="f4330-146">El F# idioma codifica la restricción en el archivo de signatura, lo que C# significa que no puede volver a usar su representación.</span><span class="sxs-lookup"><span data-stu-id="f4330-146">The F# language encodes the constraint in the signature file which means C# cannot re-use their representation.</span></span> <span data-ttu-id="f4330-147">Se debe elegir un nuevo atributo para esta restricción.</span><span class="sxs-lookup"><span data-stu-id="f4330-147">A new attribute will need to be chosen for this constraint.</span></span> <span data-ttu-id="f4330-148">Además, un método que tiene esta restricción debe estar protegido por mod-req.</span><span class="sxs-lookup"><span data-stu-id="f4330-148">Additionally a method which has this constraint must be protected by a mod-req.</span></span>

### <a name="blittable-vs-unmanaged"></a><span data-ttu-id="f4330-149">Bytes entre bits y no administrados</span><span class="sxs-lookup"><span data-stu-id="f4330-149">Blittable vs. Unmanaged</span></span>
<span data-ttu-id="f4330-150">El F# lenguaje tiene una [característica muy similar](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) que usa la palabra clave Unmanaged.</span><span class="sxs-lookup"><span data-stu-id="f4330-150">The F# language has a very [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) which uses the keyword unmanaged.</span></span> <span data-ttu-id="f4330-151">El nombre que se va a representar como bits/bytes proviene del uso en Midori.</span><span class="sxs-lookup"><span data-stu-id="f4330-151">The blittable name comes from the use in Midori.</span></span>  <span data-ttu-id="f4330-152">Puede que le interese buscar la precedencia aquí y usar en su lugar no administrada.</span><span class="sxs-lookup"><span data-stu-id="f4330-152">May want to look to precedence here and use unmanaged instead.</span></span> 

<span data-ttu-id="f4330-153">**Solución** de El lenguaje que decide usar no administrado</span><span class="sxs-lookup"><span data-stu-id="f4330-153">**Resolution** The language decide to use unmanaged</span></span> 

### <a name="verifier"></a><span data-ttu-id="f4330-154">Comprobador</span><span class="sxs-lookup"><span data-stu-id="f4330-154">Verifier</span></span>

<span data-ttu-id="f4330-155">¿Es necesario actualizar el comprobador/tiempo de ejecución para comprender el uso de punteros a parámetros de tipo genérico?</span><span class="sxs-lookup"><span data-stu-id="f4330-155">Does the verifier / runtime need to be updated to understand the use of pointers to generic type parameters?</span></span>  <span data-ttu-id="f4330-156">¿O puede simplemente trabajar como está sin cambios?</span><span class="sxs-lookup"><span data-stu-id="f4330-156">Or can it simply work as is without changes?</span></span>

<span data-ttu-id="f4330-157">**Solución** de No se necesitan cambios.</span><span class="sxs-lookup"><span data-stu-id="f4330-157">**Resolution** No changes needed.</span></span> <span data-ttu-id="f4330-158">Todos los tipos de puntero son simplemente no comprobables.</span><span class="sxs-lookup"><span data-stu-id="f4330-158">All pointer types are simply unverifiable.</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="f4330-159">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="f4330-159">Design meetings</span></span>

<span data-ttu-id="f4330-160">N/D</span><span class="sxs-lookup"><span data-stu-id="f4330-160">n/a</span></span>
