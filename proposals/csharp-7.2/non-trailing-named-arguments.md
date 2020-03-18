---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483647"
---
# <a name="non-trailing-named-arguments"></a><span data-ttu-id="b0b7a-101">Argumentos con nombre no finales</span><span class="sxs-lookup"><span data-stu-id="b0b7a-101">Non-trailing named arguments</span></span>

## <a name="summary"></a><span data-ttu-id="b0b7a-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="b0b7a-102">Summary</span></span>
[summary]: #summary
<span data-ttu-id="b0b7a-103">Permita que los argumentos con nombre se utilicen en la posición no final, siempre que se usen en su posición correcta.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-103">Allow named arguments to be used in non-trailing position, as long as they are used in their correct position.</span></span> <span data-ttu-id="b0b7a-104">Por ejemplo: `DoSomething(isEmployed:true, name, age);`.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-104">For example: `DoSomething(isEmployed:true, name, age);`.</span></span>

## <a name="motivation"></a><span data-ttu-id="b0b7a-105">Motivación</span><span class="sxs-lookup"><span data-stu-id="b0b7a-105">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="b0b7a-106">La principal motivación es evitar escribir información redundante.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-106">The main motivation is to avoid typing redundant information.</span></span> <span data-ttu-id="b0b7a-107">Es habitual asignar un nombre a un argumento que sea un literal (como `null`, `true`) con el fin de aclarar el código, en lugar de pasar argumentos desordenados.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-107">It is common to name an argument that is a literal (such as `null`, `true`) for the purpose of clarifying the code, rather than of passing arguments out-of-order.</span></span>
<span data-ttu-id="b0b7a-108">Actualmente no se permite (`CS1738`) a menos que todos los argumentos siguientes también se denominen.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-108">That is currently disallowed (`CS1738`) unless all the following arguments are also named.</span></span>

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

<span data-ttu-id="b0b7a-109">Algunos ejemplos adicionales:</span><span class="sxs-lookup"><span data-stu-id="b0b7a-109">Some additional examples:</span></span>
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

<span data-ttu-id="b0b7a-110">Esto también funcionaría con los parámetros:</span><span class="sxs-lookup"><span data-stu-id="b0b7a-110">This would also work with params:</span></span>
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a><span data-ttu-id="b0b7a-111">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="b0b7a-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="b0b7a-112">En la sección § 7.5.1 (listas de argumentos), la especificación indica actualmente:</span><span class="sxs-lookup"><span data-stu-id="b0b7a-112">In §7.5.1 (Argument lists), the spec currently says:</span></span>
> <span data-ttu-id="b0b7a-113">Un *argumento* con un *argumento-Name* se conoce como argumento con __nombre__, mientras que un *argumento* sin *el argumento-Name* es un __argumento posicional__.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-113">An *argument* with an *argument-name* is referred to as a __named argument__, whereas an *argument* without an *argument-name* is a __positional argument__.</span></span> <span data-ttu-id="b0b7a-114">Es un error que un argumento posicional aparezca después de un argumento con nombre en una *lista de argumentos*.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-114">It is an error for a positional argument to appear after a named argument in an *argument-list*.</span></span>

<span data-ttu-id="b0b7a-115">La propuesta es quitar este error y actualizar las reglas para buscar el parámetro correspondiente para un argumento (§ 7.5.1.1):</span><span class="sxs-lookup"><span data-stu-id="b0b7a-115">The proposal is to remove this error and update the rules for finding the corresponding parameter for an argument (§7.5.1.1):</span></span>

<span data-ttu-id="b0b7a-116">Argumentos en la lista de argumentos de constructores de instancia, métodos, indizadores y delegados:</span><span class="sxs-lookup"><span data-stu-id="b0b7a-116">Arguments in the argument-list of instance constructors, methods, indexers and delegates:</span></span>
- <span data-ttu-id="b0b7a-117">[reglas existentes]</span><span class="sxs-lookup"><span data-stu-id="b0b7a-117">[existing rules]</span></span>
- <span data-ttu-id="b0b7a-118">Un argumento sin nombre corresponde a ningún parámetro cuando está después de un argumento con nombre fuera de posición o un argumento params con nombre.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-118">An unnamed argument corresponds to no parameter when it is after an out-of-position named argument or a named params argument.</span></span>

<span data-ttu-id="b0b7a-119">En concreto, esto impide que se invoque `void M(bool a = true, bool b = true, bool c = true, );` con `M(c: false, valueB);`.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-119">In particular, this prevents invoking `void M(bool a = true, bool b = true, bool c = true, );` with `M(c: false, valueB);`.</span></span> <span data-ttu-id="b0b7a-120">El primer argumento se usa fuera de la posición (el argumento se usa en la primera posición, pero el parámetro denominado "c" está en la tercera posición), por lo que los siguientes argumentos deben tener nombre.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-120">The first argument is used out-of-position (the argument is used in first position, but the parameter named "c" is in third position), so the following arguments should be named.</span></span>

<span data-ttu-id="b0b7a-121">En otras palabras, los argumentos con nombre no finales solo se permiten cuando el nombre y la posición dan como resultado el mismo parámetro correspondiente.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-121">In other words, non-trailing named arguments are only allowed when the name and the position result in finding the same corresponding parameter.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="b0b7a-122">Desventajas</span><span class="sxs-lookup"><span data-stu-id="b0b7a-122">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="b0b7a-123">Esta propuesta agrava los matices existentes con argumentos con nombre en la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-123">This proposal exacerbates existing subtleties with named arguments in overload resolution.</span></span> <span data-ttu-id="b0b7a-124">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b0b7a-124">For instance:</span></span>

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

<span data-ttu-id="b0b7a-125">Puede obtener esta situación hoy mismo intercambiando los parámetros:</span><span class="sxs-lookup"><span data-stu-id="b0b7a-125">You could get this situation today by swapping the parameters:</span></span>

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

<span data-ttu-id="b0b7a-126">Del mismo modo, si tiene dos métodos `void M(int a, int b)` y `void M(int x, string y)`, el `M(x: 1, 2)` de invocación errónea producirá un diagnóstico basado en la segunda sobrecarga ("no se puede convertir de ' int ' a ' String '").</span><span class="sxs-lookup"><span data-stu-id="b0b7a-126">Similarly, if you have two methods `void M(int a, int b)` and `void M(int x, string y)`, the mistaken invocation `M(x: 1, 2)` will produce a diagnostic based on the second overload ("cannot convert from 'int' to 'string'").</span></span> <span data-ttu-id="b0b7a-127">Este problema ya existe cuando el argumento con nombre se usa en una posición final.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-127">This problem already exists when the named argument is used in a trailing position.</span></span>

## <a name="alternatives"></a><span data-ttu-id="b0b7a-128">Alternativas</span><span class="sxs-lookup"><span data-stu-id="b0b7a-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="b0b7a-129">Hay un par de alternativas a tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="b0b7a-129">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="b0b7a-130">El estado quo</span><span class="sxs-lookup"><span data-stu-id="b0b7a-130">The status quo</span></span>
- <span data-ttu-id="b0b7a-131">Proporcionar asistencia IDE para rellenar todos los nombres de los argumentos finales cuando se escribe un nombre específico en el centro.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-131">Providing IDE assistance to fill-in all the names of trailing arguments when you type specific a name in the middle.</span></span>

<span data-ttu-id="b0b7a-132">Ambos sufren un mayor nivel de detalle, ya que introducen varios argumentos con nombre, incluso si solo se necesita un nombre de literal al principio de la lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-132">Both of those suffer from more verbosity, as they introduce multiple named arguments even if you just need one name of a literal at the beginning of the argument list.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="b0b7a-133">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="b0b7a-133">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="b0b7a-134">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="b0b7a-134">Design meetings</span></span>
[ldm]: #ldm
<span data-ttu-id="b0b7a-135">La característica se analizó brevemente en LDM el 16 de mayo de 2017, con aprobación en principio (Aceptar para pasar a propuesta/prototipo).</span><span class="sxs-lookup"><span data-stu-id="b0b7a-135">The feature was briefly discussed in LDM on May 16th 2017, with approval in principle (ok to move to proposal/prototype).</span></span> <span data-ttu-id="b0b7a-136">También se analizó brevemente el 28 de junio de 2017.</span><span class="sxs-lookup"><span data-stu-id="b0b7a-136">It was also briefly discussed on June 28th 2017.</span></span>

<span data-ttu-id="b0b7a-137">Está relacionado con el análisis inicial https://github.com/dotnet/csharplang/issues/518 está relacionado con el problema experto https://github.com/dotnet/csharplang/issues/570</span><span class="sxs-lookup"><span data-stu-id="b0b7a-137">Relates to initial discussion https://github.com/dotnet/csharplang/issues/518 Relates to championed issue https://github.com/dotnet/csharplang/issues/570</span></span>
