---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281949"
---
# <a name="simple-programs"></a><span data-ttu-id="5b211-101">Programas simples</span><span class="sxs-lookup"><span data-stu-id="5b211-101">Simple programs</span></span>

* <span data-ttu-id="5b211-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="5b211-102">[x] Proposed</span></span>
* <span data-ttu-id="5b211-103">[x] prototipo: iniciado</span><span class="sxs-lookup"><span data-stu-id="5b211-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="5b211-104">[] Implementación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="5b211-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="5b211-105">[] Especificación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="5b211-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="5b211-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="5b211-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="5b211-107">Permita que una secuencia de *instrucciones* se produzca justo antes de la *namespace_member_declaration*de un *compilation_unit* (es decir, un archivo de código fuente).</span><span class="sxs-lookup"><span data-stu-id="5b211-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="5b211-108">La semántica es que, si dicha secuencia de *instrucciones* está presente, se emitirá la siguiente declaración de tipos, módulo el nombre de tipo real y el nombre del método:</span><span class="sxs-lookup"><span data-stu-id="5b211-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="5b211-109">Consulte también https://github.com/dotnet/csharplang/issues/3117.</span><span class="sxs-lookup"><span data-stu-id="5b211-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="5b211-110">Motivación</span><span class="sxs-lookup"><span data-stu-id="5b211-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="5b211-111">Hay una determinada cantidad de reutilización que se encuentra en la parte más sencilla de los programas, debido a la necesidad de un método `Main` explícito.</span><span class="sxs-lookup"><span data-stu-id="5b211-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="5b211-112">Esto parece ser un aprendizaje lingüístico y una claridad de los programas.</span><span class="sxs-lookup"><span data-stu-id="5b211-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="5b211-113">Por lo tanto, el objetivo principal de la característica C# es permitir programas sin reutilizaciones innecesarias en torno a ellos, por lo que los aprendidos y la claridad del código.</span><span class="sxs-lookup"><span data-stu-id="5b211-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="5b211-114">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="5b211-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="5b211-115">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="5b211-115">Syntax</span></span>

<span data-ttu-id="5b211-116">La única sintaxis adicional es permitir una secuencia de *instrucciones*en una unidad de compilación, justo antes de la *namespace_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="5b211-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="5b211-117">Solo una *compilation_unit* puede tener *instrucciones*.</span><span class="sxs-lookup"><span data-stu-id="5b211-117">Only one *compilation_unit* is allowed to have *statement*s.</span></span> 

<span data-ttu-id="5b211-118">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5b211-118">Example:</span></span>

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="5b211-119">Semántica</span><span class="sxs-lookup"><span data-stu-id="5b211-119">Semantics</span></span>

<span data-ttu-id="5b211-120">Si las instrucciones de nivel superior están presentes en cualquier unidad de compilación del programa, el significado es como si se combinaran en el cuerpo del bloque de un método `Main` de una clase `Program` en el espacio de nombres global, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="5b211-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="5b211-121">Tenga en cuenta que los nombres "Program" y "Main" se usan solo con fines ilustrativos, los nombres reales usados por el compilador dependen de la implementación y no se puede hacer referencia al método por el nombre del código fuente.</span><span class="sxs-lookup"><span data-stu-id="5b211-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="5b211-122">El método se designa como el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="5b211-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="5b211-123">Los métodos declarados explícitamente que por Convención se pueden considerar como candidatos de punto de entrada se omiten.</span><span class="sxs-lookup"><span data-stu-id="5b211-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="5b211-124">Cuando esto sucede, se genera una advertencia.</span><span class="sxs-lookup"><span data-stu-id="5b211-124">A warning is reported when that happens.</span></span> <span data-ttu-id="5b211-125">Es un error especificar `-main:<type>` modificador del compilador cuando hay instrucciones de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="5b211-125">It is an error to specify `-main:<type>` compiler switch when there are top-level statements.</span></span>

<span data-ttu-id="5b211-126">Las operaciones asincrónicas se permiten en las instrucciones de nivel superior hasta el grado en que se permiten en las instrucciones de un método de punto de entrada asincrónico normal.</span><span class="sxs-lookup"><span data-stu-id="5b211-126">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="5b211-127">Sin embargo, no son necesarios si `await` expresiones y otras operaciones asincrónicas se omiten, no se genera ninguna advertencia.</span><span class="sxs-lookup"><span data-stu-id="5b211-127">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="5b211-128">En su lugar, la firma del método de punto de entrada generado es equivalente a</span><span class="sxs-lookup"><span data-stu-id="5b211-128">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="5b211-129">En el ejemplo anterior se produciría la siguiente declaración de método `$Main`:</span><span class="sxs-lookup"><span data-stu-id="5b211-129">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="5b211-130">Al mismo tiempo, un ejemplo como este:</span><span class="sxs-lookup"><span data-stu-id="5b211-130">At the same time an example like this:</span></span>
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="5b211-131">produciría:</span><span class="sxs-lookup"><span data-stu-id="5b211-131">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="5b211-132">Ámbito de las variables locales de nivel superior y las funciones locales</span><span class="sxs-lookup"><span data-stu-id="5b211-132">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="5b211-133">Aunque las funciones y las variables locales de nivel superior se "encapsulan" en el método de punto de entrada generado, deben seguir en el ámbito de todo el programa en cada unidad de compilación.</span><span class="sxs-lookup"><span data-stu-id="5b211-133">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program in every compilation unit.</span></span>
<span data-ttu-id="5b211-134">Para la evaluación de nombre simple, una vez que se alcanza el espacio de nombres global:</span><span class="sxs-lookup"><span data-stu-id="5b211-134">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="5b211-135">En primer lugar, se realiza un intento de evaluar el nombre dentro del método de punto de entrada generado y solo si se produce un error en este intento.</span><span class="sxs-lookup"><span data-stu-id="5b211-135">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="5b211-136">Se realiza la evaluación "normal" dentro de la declaración del espacio de nombres global.</span><span class="sxs-lookup"><span data-stu-id="5b211-136">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="5b211-137">Esto podría dar lugar a la sombra de nombres de espacios de nombres y tipos declarados en el espacio de nombres global, así como a la sombra de nombres importados.</span><span class="sxs-lookup"><span data-stu-id="5b211-137">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="5b211-138">Si la evaluación del nombre simple se produce fuera de las instrucciones de nivel superior y la evaluación produce una variable o función local de nivel superior, esto debe producir un error.</span><span class="sxs-lookup"><span data-stu-id="5b211-138">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="5b211-139">De esta forma, se protege nuestra futura capacidad para abordar mejor las "funciones de nivel superior" (escenario 2 en https://github.com/dotnet/csharplang/issues/3117)y se pueden proporcionar diagnósticos útiles a los usuarios que no creen que se admitan por equivocación.</span><span class="sxs-lookup"><span data-stu-id="5b211-139">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

