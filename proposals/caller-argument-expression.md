---
ms.openlocfilehash: fa3326bf69c83b6042b1db7b5567fd5c28d6f81a
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483521"
---
# <a name="callerargumentexpression"></a><span data-ttu-id="8ada4-101">CallerArgumentExpression</span><span class="sxs-lookup"><span data-stu-id="8ada4-101">CallerArgumentExpression</span></span>

* <span data-ttu-id="8ada4-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="8ada4-102">[x] Proposed</span></span>
* <span data-ttu-id="8ada4-103">[] Prototipo: no iniciado</span><span class="sxs-lookup"><span data-stu-id="8ada4-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="8ada4-104">[] Implementación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="8ada4-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="8ada4-105">[] Especificación: no iniciada</span><span class="sxs-lookup"><span data-stu-id="8ada4-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="8ada4-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="8ada4-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="8ada4-107">Permite a los desarrolladores capturar las expresiones que se pasan a un método para habilitar mejores mensajes de error en las API de diagnóstico y prueba y reducir las pulsaciones de teclas.</span><span class="sxs-lookup"><span data-stu-id="8ada4-107">Allow developers to capture the expressions passed to a method, to enable better error messages in diagnostic/testing APIs and reduce keystrokes.</span></span>

## <a name="motivation"></a><span data-ttu-id="8ada4-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="8ada4-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="8ada4-109">Cuando se produce un error en la validación de una aserción o un argumento, el desarrollador desea saber lo máximo posible sobre dónde y por qué se produjo el error.</span><span class="sxs-lookup"><span data-stu-id="8ada4-109">When an assertion or argument validation fails, the developer wants to know as much as possible about where and why it failed.</span></span> <span data-ttu-id="8ada4-110">Sin embargo, las API de diagnóstico de hoy en día no lo facilitan totalmente.</span><span class="sxs-lookup"><span data-stu-id="8ada4-110">However, today's diagnostic APIs do not fully facilitate this.</span></span> <span data-ttu-id="8ada4-111">Tenga en cuenta el método siguiente:</span><span class="sxs-lookup"><span data-stu-id="8ada4-111">Consider the following method:</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null);
    Debug.Assert(array.Length == 1);

    return array[0];
}
```

<span data-ttu-id="8ada4-112">Cuando se produce un error en una de las aserciones, solo se proporcionarán el nombre de archivo, el número de línea y el nombre del método en el seguimiento de la pila.</span><span class="sxs-lookup"><span data-stu-id="8ada4-112">When one of the asserts fail, only the filename, line number, and method name will be provided in the stack trace.</span></span> <span data-ttu-id="8ada4-113">El desarrollador no podrá saber qué aserción produjo un error en esta información: (s) tendrá que abrir el archivo y navegar al número de línea proporcionado para ver qué salió mal.</span><span class="sxs-lookup"><span data-stu-id="8ada4-113">The developer will not be able to tell which assert failed from this information-- (s)he will have to open the file and navigate to the provided line number to see what went wrong.</span></span>

<span data-ttu-id="8ada4-114">Este es también el motivo por el que los marcos de pruebas deben proporcionar una variedad de métodos de aserción.</span><span class="sxs-lookup"><span data-stu-id="8ada4-114">This is also the reason testing frameworks have to provide a variety of assert methods.</span></span> <span data-ttu-id="8ada4-115">Con xUnit, no se usan con frecuencia `Assert.True` y `Assert.False` porque no proporcionan suficiente contexto sobre el error.</span><span class="sxs-lookup"><span data-stu-id="8ada4-115">With xUnit, `Assert.True` and `Assert.False` are not frequently used because they do not provide enough context about what failed.</span></span>

<span data-ttu-id="8ada4-116">Aunque la situación es un poco mejor para la validación de argumentos porque los nombres de argumentos no válidos se muestran al desarrollador, el desarrollador debe pasar estos nombres a las excepciones manualmente.</span><span class="sxs-lookup"><span data-stu-id="8ada4-116">While the situation is a bit better for argument validation because the names of invalid arguments are shown to the developer, the developer must pass these names to exceptions manually.</span></span> <span data-ttu-id="8ada4-117">Si el ejemplo anterior se reescribió para usar la validación de argumentos tradicional en lugar de `Debug.Assert`, tendría el aspecto siguiente.</span><span class="sxs-lookup"><span data-stu-id="8ada4-117">If the above example were rewritten to use traditional argument validation instead of `Debug.Assert`, it would look like</span></span>

```csharp
T Single<T>(this T[] array)
{
    if (array == null)
    {
        throw new ArgumentNullException(nameof(array));
    }

    if (array.Length != 1)
    {
        throw new ArgumentException("Array must contain a single element.", nameof(array));
    }

    return array[0];
}
```

<span data-ttu-id="8ada4-118">Observe que `nameof(array)` se debe pasar a cada excepción, aunque ya está claro del contexto qué argumento no es válido.</span><span class="sxs-lookup"><span data-stu-id="8ada4-118">Notice that `nameof(array)` must be passed to each exception, although it's already clear from context which argument is invalid.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="8ada4-119">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="8ada4-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="8ada4-120">En los ejemplos anteriores, incluir la cadena `"array != null"` o `"array.Length == 1"` en el mensaje de aserción ayudaría al desarrollador a determinar el error.</span><span class="sxs-lookup"><span data-stu-id="8ada4-120">In the above examples, including the string `"array != null"` or `"array.Length == 1"` in the assert message would help the developer determine what failed.</span></span> <span data-ttu-id="8ada4-121">Escriba `CallerArgumentExpression`: es un atributo que el marco de trabajo puede usar para obtener la cadena asociada a un argumento de método determinado.</span><span class="sxs-lookup"><span data-stu-id="8ada4-121">Enter `CallerArgumentExpression`: it's an attribute the framework can use to obtain the string associated with a particular method argument.</span></span> <span data-ttu-id="8ada4-122">Lo agregaremos a `Debug.Assert` similar</span><span class="sxs-lookup"><span data-stu-id="8ada4-122">We would add it to `Debug.Assert` like so</span></span>

```csharp
public static class Debug
{
    public static void Assert(bool condition, [CallerArgumentExpression("condition")] string message = null);
}
```

<span data-ttu-id="8ada4-123">El código fuente del ejemplo anterior seguiría siendo el mismo.</span><span class="sxs-lookup"><span data-stu-id="8ada4-123">The source code in the above example would stay the same.</span></span> <span data-ttu-id="8ada4-124">Sin embargo, el código que el compilador emite realmente correspondería a</span><span class="sxs-lookup"><span data-stu-id="8ada4-124">However, the code the compiler actually emits would correspond to</span></span>

```csharp
T Single<T>(this T[] array)
{
    Debug.Assert(array != null, "array != null");
    Debug.Assert(array.Length == 1, "array.Length == 1");

    return array[0];
}
```

<span data-ttu-id="8ada4-125">El compilador reconoce especialmente el atributo en `Debug.Assert`.</span><span class="sxs-lookup"><span data-stu-id="8ada4-125">The compiler specially recognizes the attribute on `Debug.Assert`.</span></span> <span data-ttu-id="8ada4-126">Pasa la cadena asociada al argumento al que se hace referencia en el constructor del atributo (en este caso, `condition`) en el sitio de llamada.</span><span class="sxs-lookup"><span data-stu-id="8ada4-126">It passes the string associated with the argument referred to in the attribute's constructor (in this case, `condition`) at the call site.</span></span> <span data-ttu-id="8ada4-127">Cuando se produce un error en una aserción, se muestra al desarrollador la condición falsa y sabrá cuál produjo un error.</span><span class="sxs-lookup"><span data-stu-id="8ada4-127">When either assert fails, the developer will be shown the condition that was false and will know which one failed.</span></span>

<span data-ttu-id="8ada4-128">Para la validación de argumentos, el atributo no se puede usar directamente, pero se puede usar a través de una clase auxiliar:</span><span class="sxs-lookup"><span data-stu-id="8ada4-128">For argument validation, the attribute cannot be used directly, but can be made use of through a helper class:</span></span>

```csharp
public static class Verify
{
    public static void Argument(bool condition, string message, [CallerArgumentExpression("condition")] string conditionExpression = null)
    {
        if (!condition) throw new ArgumentException(message: message, paramName: conditionExpression);
    }

    public static void InRange(int argument, int low, int high,
        [CallerArgumentExpression("argument")] string argumentExpression = null,
        [CallerArgumentExpression("low")] string lowExpression = null,
        [CallerArgumentExpression("high")] string highExpression = null)
    {
        if (argument < low)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be less than {lowExpression} ({low}).");
        }

        if (argument > high)
        {
            throw new ArgumentOutOfRangeException(paramName: argumentExpression,
                message: $"{argumentExpression} ({argument}) cannot be greater than {highExpression} ({high}).");
        }
    }

    public static void NotNull<T>(T argument, [CallerArgumentExpression("argument")] string argumentExpression = null)
        where T : class
    {
        if (argument == null) throw new ArgumentNullException(paramName: argumentExpression);
    }
}

T Single<T>(this T[] array)
{
    Verify.NotNull(array); // paramName: "array"
    Verify.Argument(array.Length == 1, "Array must contain a single element."); // paramName: "array.Length == 1"

    return array[0];
}

T ElementAt(this T[] array, int index)
{
    Verify.NotNull(array); // paramName: "array"
    // paramName: "index"
    // message: "index (-1) cannot be less than 0 (0).", or
    //          "index (6) cannot be greater than array.Length - 1 (5)."
    Verify.InRange(index, 0, array.Length - 1);

    return array[index];
}
```

<span data-ttu-id="8ada4-129">Se está llevando a cabo una propuesta para agregar una clase auxiliar a la plataforma en https://github.com/dotnet/corefx/issues/17068.</span><span class="sxs-lookup"><span data-stu-id="8ada4-129">A proposal to add such a helper class to the framework is underway at https://github.com/dotnet/corefx/issues/17068.</span></span> <span data-ttu-id="8ada4-130">Si se ha implementado esta característica de lenguaje, la propuesta podría actualizarse para aprovecharse de esta característica.</span><span class="sxs-lookup"><span data-stu-id="8ada4-130">If this language feature was implemented, the proposal could be updated to take advantage of this feature.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="8ada4-131">Métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="8ada4-131">Extension methods</span></span>

<span data-ttu-id="8ada4-132">`CallerArgumentExpression`puede hacer referencia al parámetro `this` de un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="8ada4-132">The `this` parameter in an extension method may be referenced by `CallerArgumentExpression`.</span></span> <span data-ttu-id="8ada4-133">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8ada4-133">For example:</span></span>

```csharp
public static void ShouldBe<T>(this T @this, T expected, [CallerArgumentExpression("this")] string thisExpression = null) {}

contestant.Points.ShouldBe(1337); // thisExpression: "contestant.Points"
```

<span data-ttu-id="8ada4-134">`thisExpression` recibirá la expresión correspondiente al objeto antes del punto.</span><span class="sxs-lookup"><span data-stu-id="8ada4-134">`thisExpression` will receive the expression corresponding to the object before the dot.</span></span> <span data-ttu-id="8ada4-135">Si se llama con sintaxis de método estático, por ejemplo `Ext.ShouldBe(contestant.Points, 1337)`, se comportará como si el primer parámetro no estuviera marcado `this`.</span><span class="sxs-lookup"><span data-stu-id="8ada4-135">If it's called with static method syntax, e.g. `Ext.ShouldBe(contestant.Points, 1337)`, it will behave as if first parameter wasn't marked `this`.</span></span>

<span data-ttu-id="8ada4-136">Siempre debe haber una expresión correspondiente al parámetro `this`.</span><span class="sxs-lookup"><span data-stu-id="8ada4-136">There should always be an expression corresponding to the `this` parameter.</span></span> <span data-ttu-id="8ada4-137">Incluso si una instancia de una clase llama a un método de extensión en sí mismo, por ejemplo, `this.Single()` desde dentro de un tipo de colección, el compilador exige el `this` para que `"this"` se pase.</span><span class="sxs-lookup"><span data-stu-id="8ada4-137">Even if an instance of a class calls an extension method on itself, e.g. `this.Single()` from inside a collection type, the `this` is mandated by the compiler so `"this"` will get passed.</span></span> <span data-ttu-id="8ada4-138">Si se cambia esta regla en el futuro, se puede considerar la posibilidad de pasar `null` o la cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="8ada4-138">If this rule is changed in the future, we can consider passing `null` or the empty string.</span></span>

### <a name="extra-details"></a><span data-ttu-id="8ada4-139">Detalles adicionales</span><span class="sxs-lookup"><span data-stu-id="8ada4-139">Extra details</span></span>

- <span data-ttu-id="8ada4-140">Al igual que los demás atributos de `Caller*`, como `CallerMemberName`, este atributo solo se puede usar en parámetros con valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="8ada4-140">Like the other `Caller*` attributes, such as `CallerMemberName`, this attribute may only be used on parameters with default values.</span></span>
- <span data-ttu-id="8ada4-141">Se permiten varios parámetros marcados con `CallerArgumentExpression`, como se muestra anteriormente.</span><span class="sxs-lookup"><span data-stu-id="8ada4-141">Multiple parameters marked with `CallerArgumentExpression` are permitted, as shown above.</span></span>
- <span data-ttu-id="8ada4-142">El espacio de nombres del atributo se `System.Runtime.CompilerServices`.</span><span class="sxs-lookup"><span data-stu-id="8ada4-142">The attribute's namespace will be `System.Runtime.CompilerServices`.</span></span>
- <span data-ttu-id="8ada4-143">Si se proporciona `null` o una cadena que no es un nombre de parámetro (por ejemplo, `"notAParameterName"`), el compilador pasará una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="8ada4-143">If `null` or a string that is not a parameter name (e.g. `"notAParameterName"`) is provided, the compiler will pass in an empty string.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="8ada4-144">Desventajas</span><span class="sxs-lookup"><span data-stu-id="8ada4-144">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="8ada4-145">Las personas que sepan cómo usar los descompiladores podrán ver parte del código fuente en los sitios de llamada para los métodos marcados con este atributo.</span><span class="sxs-lookup"><span data-stu-id="8ada4-145">People who know how to use decompilers will be able to see some of the source code at call sites for methods marked with this attribute.</span></span> <span data-ttu-id="8ada4-146">Esto puede no ser deseable o inesperado para el software de código fuente cerrado.</span><span class="sxs-lookup"><span data-stu-id="8ada4-146">This may be undesirable/unexpected for closed-source software.</span></span>

- <span data-ttu-id="8ada4-147">Aunque esto no es un error en la propia característica, una fuente de preocupación puede ser que existe una API `Debug.Assert` hoy que solo toma un `bool`.</span><span class="sxs-lookup"><span data-stu-id="8ada4-147">Although this is not a flaw in the feature itself, a source of concern may be that there exists a `Debug.Assert` API today that only takes a `bool`.</span></span> <span data-ttu-id="8ada4-148">Incluso si la sobrecarga que toma un mensaje tenía su segundo parámetro marcado con este atributo y se hizo opcional, el compilador seguiría seleccionando el no-Message en la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="8ada4-148">Even if the overload taking a message had its second parameter marked with this attribute and made optional, the compiler would still pick the no-message one in overload resolution.</span></span> <span data-ttu-id="8ada4-149">Por lo tanto, la sobrecarga sin mensaje tendría que quitarse para aprovecharse de esta característica, lo que sería un cambio de interrupción binario (aunque no de origen).</span><span class="sxs-lookup"><span data-stu-id="8ada4-149">Therefore, the no-message overload would have to be removed to take advantage of this feature, which would be a binary (although not source) breaking change.</span></span>

## <a name="alternatives"></a><span data-ttu-id="8ada4-150">Alternativas</span><span class="sxs-lookup"><span data-stu-id="8ada4-150">Alternatives</span></span>
[alternatives]: #alternatives

- <span data-ttu-id="8ada4-151">Si es capaz de ver el código fuente en los sitios de llamada para los métodos que usan este atributo se considera un problema, podemos hacer que los efectos del atributo participen.</span><span class="sxs-lookup"><span data-stu-id="8ada4-151">If being able to see source code at call sites for methods that use this attribute proves to be a problem, we can make the attribute's effects opt-in.</span></span> <span data-ttu-id="8ada4-152">Los desarrolladores lo habilitarán a través de un atributo de `[assembly: EnableCallerArgumentExpression]` de todo el ensamblado que colocan en `AssemblyInfo.cs`.</span><span class="sxs-lookup"><span data-stu-id="8ada4-152">Developers will enable it through an assembly-wide `[assembly: EnableCallerArgumentExpression]` attribute they put in `AssemblyInfo.cs`.</span></span>
  - <span data-ttu-id="8ada4-153">En el caso de que los efectos del atributo no estén habilitados, llamar a los métodos marcados con el atributo no sería un error, para permitir que los métodos existentes utilicen el atributo y mantengan la compatibilidad con el origen.</span><span class="sxs-lookup"><span data-stu-id="8ada4-153">In the case the attribute's effects are not enabled, calling methods marked with the attribute would not be an error, to allow existing methods to use the attribute and maintain source compatibility.</span></span> <span data-ttu-id="8ada4-154">Sin embargo, se omitirá el atributo y se llamará al método con el valor predeterminado proporcionado.</span><span class="sxs-lookup"><span data-stu-id="8ada4-154">However, the attribute would be ignored and the method would be called with whatever default value was provided.</span></span>

```csharp
// Assembly1

void Foo(string bar); // V1
void Foo(string bar, string barExpression = "not provided"); // V2
void Foo(string bar, [CallerArgumentExpression("bar")] string barExpression = "not provided"); // V3

// Assembly2

Foo(a); // V1: Compiles to Foo(a), V2, V3: Compiles to Foo(a, "not provided")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")

// Assembly3

[assembly: EnableCallerArgumentExpression]

Foo(a); // V1: Compiles to Foo(a), V2: Compiles to Foo(a, "not provided"), V3: Compiles to Foo(a, "a")
Foo(a, "provided"); // V2, V3: Compiles to Foo(a, "provided")
```

- <span data-ttu-id="8ada4-155">Para evitar que se produzca el[ drawbacks] [problema de compatibilidad binaria] cada vez que queremos agregar nueva información de autor de llamada a `Debug.Assert`, una solución alternativa sería agregar un `CallerInfo` struct al marco de trabajo que contenga toda la información necesaria sobre el autor de la llamada.</span><span class="sxs-lookup"><span data-stu-id="8ada4-155">To prevent the [binary compatibility problem][drawbacks] from occurring every time we want to add new caller info to `Debug.Assert`, an alternative solution would be to add a `CallerInfo` struct to the framework that contains all the necessary information about the caller.</span></span>

```csharp
struct CallerInfo
{
    public string MemberName { get; set; }
    public string TypeName { get; set; }
    public string Namespace { get; set; }
    public string FullTypeName { get; set; }
    public string FilePath { get; set; }
    public int LineNumber { get; set; }
    public int ColumnNumber { get; set; }
    public Type Type { get; set; }
    public MethodBase Method { get; set; }
    public string[] ArgumentExpressions { get; set; }
}

[Flags]
enum CallerInfoOptions
{
    MemberName = 1, TypeName = 2, ...
}

public static class Debug
{
    public static void Assert(bool condition,
        // If a flag is not set here, the corresponding CallerInfo member is not populated by the caller, so it's
        // pay-for-play friendly.
        [CallerInfo(CallerInfoOptions.FilePath | CallerInfoOptions.Method | CallerInfoOptions.ArgumentExpressions)] CallerInfo callerInfo = default(CallerInfo))
    {
        string filePath = callerInfo.FilePath;
        MethodBase method = callerInfo.Method;
        string conditionExpression = callerInfo.ArgumentExpressions[0];
        ...
    }
}

class Bar
{
    void Foo()
    {
        Debug.Assert(false);

        // Translates to:

        var callerInfo = new CallerInfo();
        callerInfo.FilePath = @"C:\Bar.cs";
        callerInfo.Method = MethodBase.GetCurrentMethod();
        callerInfo.ArgumentExpressions = new string[] { "false" };
        Debug.Assert(false, callerInfo);
    }
}
```

<span data-ttu-id="8ada4-156">Esto se propuso originalmente en https://github.com/dotnet/csharplang/issues/87.</span><span class="sxs-lookup"><span data-stu-id="8ada4-156">This was originally proposed at https://github.com/dotnet/csharplang/issues/87.</span></span>

<span data-ttu-id="8ada4-157">Este enfoque tiene algunas desventajas:</span><span class="sxs-lookup"><span data-stu-id="8ada4-157">There are a few disadvantages of this approach:</span></span>

- <span data-ttu-id="8ada4-158">A pesar de ser muy descriptivos al permitirle especificar qué propiedades necesita, todavía podría perjudicar el rendimiento mediante la asignación de una matriz para las expresiones/llamadas `MethodBase.GetCurrentMethod` incluso cuando se pasa la aserción.</span><span class="sxs-lookup"><span data-stu-id="8ada4-158">Despite being pay-for-play friendly by allowing you to specify which properties you need, it could still hurt perf significantly by allocating an array for the expressions/calling `MethodBase.GetCurrentMethod` even when the assert passes.</span></span>

- <span data-ttu-id="8ada4-159">Además, aunque pasar una nueva marca al atributo `CallerInfo` no será un cambio importante, `Debug.Assert` no se garantiza que reciba realmente ese nuevo parámetro de sitios de llamada que se compilaron con una versión anterior del método.</span><span class="sxs-lookup"><span data-stu-id="8ada4-159">Additionally, while passing a new flag to the `CallerInfo` attribute won't be a breaking change, `Debug.Assert` won't be guaranteed to actually receive that new parameter from call sites that compiled against an old version of the method.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="8ada4-160">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="8ada4-160">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="8ada4-161">TBD</span><span class="sxs-lookup"><span data-stu-id="8ada4-161">TBD</span></span>

## <a name="design-meetings"></a><span data-ttu-id="8ada4-162">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="8ada4-162">Design meetings</span></span>

<span data-ttu-id="8ada4-163">N/D</span><span class="sxs-lookup"><span data-stu-id="8ada4-163">N/A</span></span>
