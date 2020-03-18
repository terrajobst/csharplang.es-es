---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "79484121"
---
# <a name="simple-programs"></a><span data-ttu-id="c9dfd-101">Programas simples</span><span class="sxs-lookup"><span data-stu-id="c9dfd-101">Simple programs</span></span>

* <span data-ttu-id="c9dfd-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="c9dfd-102">[x] Proposed</span></span>
* <span data-ttu-id="c9dfd-103">[x] prototipo: iniciado</span><span class="sxs-lookup"><span data-stu-id="c9dfd-103">[x] Prototype: Started</span></span>
* <span data-ttu-id="c9dfd-104">[] Implementación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="c9dfd-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="c9dfd-105">[] Especificación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="c9dfd-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="c9dfd-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="c9dfd-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="c9dfd-107">Permita que una secuencia de *instrucciones* se produzca justo antes de la *namespace_member_declaration*de un *compilation_unit* (es decir, un archivo de código fuente).</span><span class="sxs-lookup"><span data-stu-id="c9dfd-107">Allow a sequence of *statements* to occur right before the *namespace_member_declaration*s of a *compilation_unit* (i.e. source file).</span></span>

<span data-ttu-id="c9dfd-108">La semántica es que, si dicha secuencia de *instrucciones* está presente, se emitirá la siguiente declaración de tipos, módulo el nombre de tipo real y el nombre del método:</span><span class="sxs-lookup"><span data-stu-id="c9dfd-108">The semantics are that if such a sequence of *statements* is present, the following type declaration, modulo the actual type name and the method name, would be emitted:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

<span data-ttu-id="c9dfd-109">Consulte también https://github.com/dotnet/csharplang/issues/3117.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-109">See also https://github.com/dotnet/csharplang/issues/3117.</span></span>

## <a name="motivation"></a><span data-ttu-id="c9dfd-110">Motivación</span><span class="sxs-lookup"><span data-stu-id="c9dfd-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="c9dfd-111">Hay una determinada cantidad de reutilización que se encuentra en la parte más sencilla de los programas, debido a la necesidad de un método `Main` explícito.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-111">There's a certain amount of boilerplate surrounding even the simplest of programs, because of the need for an explicit `Main` method.</span></span> <span data-ttu-id="c9dfd-112">Esto parece ser un aprendizaje lingüístico y una claridad de los programas.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-112">This seems to get in the way of language learning and program clarity.</span></span> <span data-ttu-id="c9dfd-113">Por lo tanto, el objetivo principal de la característica C# es permitir programas sin reutilizaciones innecesarias en torno a ellos, por lo que los aprendidos y la claridad del código.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-113">The primary goal of the feature therefore is to allow C# programs without unnecessary boilerplate around them, for the sake of learners and the clarity of code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="c9dfd-114">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="c9dfd-114">Detailed design</span></span>
[design]: #detailed-design

### <a name="syntax"></a><span data-ttu-id="c9dfd-115">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="c9dfd-115">Syntax</span></span>

<span data-ttu-id="c9dfd-116">La única sintaxis adicional es permitir una secuencia de *instrucciones*en una unidad de compilación, justo antes de la *namespace_member_declaration*s:</span><span class="sxs-lookup"><span data-stu-id="c9dfd-116">The only additional syntax is allowing a sequence of *statement*s in a compilation unit, just before the *namespace_member_declaration*s:</span></span>

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

<span data-ttu-id="c9dfd-117">En todo menos uno *compilation_unit* la *instrucción*s debe ser declaraciones de función local.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-117">In all but one *compilation_unit* the *statement*s must all be local function declarations.</span></span> 

<span data-ttu-id="c9dfd-118">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c9dfd-118">Example:</span></span>

``` c#
// File 1 - any statements
if (args.Length == 0
    || !int.TryParse(args[0], out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

// File 2 - only local functions
(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a><span data-ttu-id="c9dfd-119">Semántica</span><span class="sxs-lookup"><span data-stu-id="c9dfd-119">Semantics</span></span>

<span data-ttu-id="c9dfd-120">Si las instrucciones de nivel superior están presentes en cualquier unidad de compilación del programa, el significado es como si se combinaran en el cuerpo del bloque de un método `Main` de una clase `Program` en el espacio de nombres global, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="c9dfd-120">If any top-level statements are present in any compilation unit of the program, the meaning is as if they were combined in the block body of a `Main` method of a `Program` class in the global namespace, as follows:</span></span>

``` c#
static class Program
{
    static async Task Main()
    {
        // File 1 statements
        // File 2 local functions
        // ...
    }
}
```

<span data-ttu-id="c9dfd-121">Tenga en cuenta que los nombres "Program" y "Main" se usan solo con fines ilustrativos, los nombres reales usados por el compilador dependen de la implementación y no se puede hacer referencia al método por el nombre del código fuente.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-121">Note that the names "Program" and "Main" are used only for illustrations purposes, actual names used by compiler are implementation dependent and neither the type, nor the method can be referenced by name from source code.</span></span>

<span data-ttu-id="c9dfd-122">El método se designa como el punto de entrada del programa.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-122">The method is designated as the entry point of the program.</span></span> <span data-ttu-id="c9dfd-123">Los métodos declarados explícitamente que por Convención se pueden considerar como candidatos de punto de entrada se omiten.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-123">Explicitly declared methods that by convention could be considered as an entry point candidates are ignored.</span></span> <span data-ttu-id="c9dfd-124">Cuando esto sucede, se genera una advertencia.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-124">A warning is reported when that happens.</span></span> <span data-ttu-id="c9dfd-125">Es un error especificar `-main:<type>` modificador del compilador.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-125">It is an error to specify `-main:<type>` compiler switch.</span></span>

<span data-ttu-id="c9dfd-126">Si alguna de las unidades de compilación tiene instrucciones que no son declaraciones de funciones locales, las instrucciones de esa unidad de compilación se producen primero.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-126">If any one compilation unit has statements other than local function declarations, statements from that compilation unit occur first.</span></span> <span data-ttu-id="c9dfd-127">Esto hace que sea válido para las funciones locales de un archivo para hacer referencia a variables locales en otro.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-127">This causes it to be legal for local functions in one file to reference local variables in another.</span></span> <span data-ttu-id="c9dfd-128">El orden de las contribuciones de instrucciones (que serían todas las funciones locales) de otras unidades de compilación es indefinido.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-128">The order of statement contributions (which would all be local functions) from other compilation units is undefined.</span></span>

<span data-ttu-id="c9dfd-129">Las operaciones asincrónicas se permiten en las instrucciones de nivel superior hasta el grado en que se permiten en las instrucciones de un método de punto de entrada asincrónico normal.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-129">Async operations are allowed in top-level statements to the degree they are allowed in statements within a regular async entry point method.</span></span> <span data-ttu-id="c9dfd-130">Sin embargo, no son necesarios si `await` expresiones y otras operaciones asincrónicas se omiten, no se genera ninguna advertencia.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-130">However, they are not required, if `await` expressions and other async operations are omitted, no warning is produced.</span></span> <span data-ttu-id="c9dfd-131">En su lugar, la firma del método de punto de entrada generado es equivalente a</span><span class="sxs-lookup"><span data-stu-id="c9dfd-131">Instead the signature of the generated entry point method is equivalent to</span></span> 
``` c#
    static void Main()
```

<span data-ttu-id="c9dfd-132">En el ejemplo anterior se produciría la siguiente declaración de método `$Main`:</span><span class="sxs-lookup"><span data-stu-id="c9dfd-132">The example above would yield the following `$Main` method declaration:</span></span>

``` c#
static class $Program
{
    static void $Main()
    {
        // Statements from File 1
        if (args.Length == 0
            || !int.TryParse(args[0], out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        // Local functions from File 2
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

<span data-ttu-id="c9dfd-133">Al mismo tiempo, un ejemplo como este:</span><span class="sxs-lookup"><span data-stu-id="c9dfd-133">At the same time an example like this:</span></span>
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

<span data-ttu-id="c9dfd-134">produciría:</span><span class="sxs-lookup"><span data-stu-id="c9dfd-134">would  yield:</span></span>
``` c#
static class $Program
{
    static async Task $Main()
    {
        // Statements from File 1
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a><span data-ttu-id="c9dfd-135">Ámbito de las variables locales de nivel superior y las funciones locales</span><span class="sxs-lookup"><span data-stu-id="c9dfd-135">Scope of top-level local variables and local functions</span></span>

<span data-ttu-id="c9dfd-136">Aunque las funciones y las variables locales de nivel superior se "encapsulan" en el método de punto de entrada generado, deben seguir en el ámbito de todo el programa.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-136">Even though top-level local variables and functions are "wrapped" into the generated entry point method, they should still be in scope throughout the program.</span></span>
<span data-ttu-id="c9dfd-137">Para la evaluación de nombre simple, una vez que se alcanza el espacio de nombres global:</span><span class="sxs-lookup"><span data-stu-id="c9dfd-137">For the purpose of simple-name evaluation, once the global namespace is reached:</span></span>
- <span data-ttu-id="c9dfd-138">En primer lugar, se realiza un intento de evaluar el nombre dentro del método de punto de entrada generado y solo si se produce un error en este intento.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-138">First, an attempt is made to evaluate the name within the generated entry point method and only if this attempt fails</span></span> 
- <span data-ttu-id="c9dfd-139">Se realiza la evaluación "normal" dentro de la declaración del espacio de nombres global.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-139">The "regular" evaluation within the global namespace declaration is performed.</span></span> 

<span data-ttu-id="c9dfd-140">Esto podría dar lugar a la sombra de nombres de espacios de nombres y tipos declarados en el espacio de nombres global, así como a la sombra de nombres importados.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-140">This could lead to name shadowing of namespaces and types declared within the global namespace as well as to shadowing of imported names.</span></span>

<span data-ttu-id="c9dfd-141">Si la evaluación del nombre simple se produce fuera de las instrucciones de nivel superior y la evaluación produce una variable o función local de nivel superior, esto debe producir un error.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-141">If the simple name evaluation occurs outside of the top-level statements and the evaluation yields a top-level local variable or function, that should lead to an error.</span></span>

<span data-ttu-id="c9dfd-142">De esta forma, se protege nuestra futura capacidad para abordar mejor las "funciones de nivel superior" (escenario 2 en https://github.com/dotnet/csharplang/issues/3117)y se pueden proporcionar diagnósticos útiles a los usuarios que no creen que se admitan por equivocación.</span><span class="sxs-lookup"><span data-stu-id="c9dfd-142">In this way we protect our future ability to better address "Top-level functions" (scenario 2 in https://github.com/dotnet/csharplang/issues/3117), and are able to give useful diagnostics to users who mistakenly believe them to be supported.</span></span>

