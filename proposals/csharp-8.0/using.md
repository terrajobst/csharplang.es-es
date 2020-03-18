---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483977"
---
# <a name="pattern-based-using-and-using-declarations"></a><span data-ttu-id="29363-101">"basado en patrones usando" y "usar declaraciones"</span><span class="sxs-lookup"><span data-stu-id="29363-101">"pattern-based using" and "using declarations"</span></span>

## <a name="summary"></a><span data-ttu-id="29363-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="29363-102">Summary</span></span>

<span data-ttu-id="29363-103">El lenguaje agregará dos nuevas capacidades en torno a la instrucción `using` para simplificar la administración de recursos: `using` debe reconocer un patrón descartable además de `IDisposable` y agregar una declaración de `using` al idioma.</span><span class="sxs-lookup"><span data-stu-id="29363-103">The language will add two new capabilities around the `using` statement in order to make resource management simpler: `using` should recognize a disposable pattern in addition to `IDisposable` and add a `using` declaration to the language.</span></span>

## <a name="motivation"></a><span data-ttu-id="29363-104">Motivación</span><span class="sxs-lookup"><span data-stu-id="29363-104">Motivation</span></span>

<span data-ttu-id="29363-105">La instrucción `using` es una herramienta eficaz para la administración de recursos hoy en día, pero requiere bastante ceremonia.</span><span class="sxs-lookup"><span data-stu-id="29363-105">The `using` statement is an effective tool for resource management today but it requires quite a bit of ceremony.</span></span> <span data-ttu-id="29363-106">Los métodos que tienen una serie de recursos para administrar se pueden bloquear sintácticamente con una serie de instrucciones `using`.</span><span class="sxs-lookup"><span data-stu-id="29363-106">Methods that have a number of resources to manage can get syntactically bogged down with a series of `using` statements.</span></span> <span data-ttu-id="29363-107">Esta carga de sintaxis es suficiente para que la mayoría de las instrucciones de estilo de codificación tengan una excepción en torno a las llaves para este escenario.</span><span class="sxs-lookup"><span data-stu-id="29363-107">This syntax burden is enough that most coding style guidelines explicitly have an exception around braces for this scenario.</span></span> 

<span data-ttu-id="29363-108">La declaración de `using` elimina gran parte de la ceremonia aquí C# y se obtiene en su par con otros lenguajes que incluyen bloques de administración de recursos.</span><span class="sxs-lookup"><span data-stu-id="29363-108">The `using` declaration removes much of the ceremony here and gets C# on par with other languages that include resource management blocks.</span></span> <span data-ttu-id="29363-109">Además, el `using` basado en patrones permite a los desarrolladores expandir el conjunto de tipos que pueden participar aquí.</span><span class="sxs-lookup"><span data-stu-id="29363-109">Additionally the pattern-based `using` lets developers expand the set of types that can participate here.</span></span> <span data-ttu-id="29363-110">En muchos casos, quitar la necesidad de crear tipos de contenedor que solo existen para permitir un uso de valores en una instrucción `using`.</span><span class="sxs-lookup"><span data-stu-id="29363-110">In many cases removing the need to create wrapper types that only exist to allow for a values use in a `using` statement.</span></span> 

<span data-ttu-id="29363-111">Juntas, estas características permiten a los desarrolladores simplificar y expandir los escenarios en los que se pueden aplicar `using`.</span><span class="sxs-lookup"><span data-stu-id="29363-111">Together these features allow developers to simplify and expand the scenarios where `using` can be applied.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="29363-112">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="29363-112">Detailed Design</span></span> 

### <a name="using-declaration"></a><span data-ttu-id="29363-113">declaración using</span><span class="sxs-lookup"><span data-stu-id="29363-113">using declaration</span></span>

<span data-ttu-id="29363-114">El lenguaje permitirá agregar `using` a una declaración de variable local.</span><span class="sxs-lookup"><span data-stu-id="29363-114">The language will allow for `using` to be added to a local variable declaration.</span></span> <span data-ttu-id="29363-115">Este tipo de declaración tendrá el mismo efecto que declarar la variable en una instrucción `using` en la misma ubicación.</span><span class="sxs-lookup"><span data-stu-id="29363-115">Such a declaration will have the same effect as declaring the variable in a `using` statement at the same location.</span></span>

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

<span data-ttu-id="29363-116">La duración de un `using` local se extenderá hasta el final del ámbito en el que se declara.</span><span class="sxs-lookup"><span data-stu-id="29363-116">The lifetime of a `using` local will extend to the end of the scope in which it is declared.</span></span> <span data-ttu-id="29363-117">El `using` variables locales se eliminará en el orden inverso en el que se declaran.</span><span class="sxs-lookup"><span data-stu-id="29363-117">The `using` locals will then be disposed in the reverse order in which they are declared.</span></span> 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

<span data-ttu-id="29363-118">No hay ninguna restricción en torno a `goto`o cualquier otra construcción de flujo de control en el aspecto de una declaración de `using`.</span><span class="sxs-lookup"><span data-stu-id="29363-118">There are no restrictions around `goto`, or any other control flow construct in the face of a `using` declaration.</span></span> <span data-ttu-id="29363-119">En su lugar, el código actúa igual que para la instrucción `using` equivalente:</span><span class="sxs-lookup"><span data-stu-id="29363-119">Instead the code acts just as it would for the equivalent `using` statement:</span></span>

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

<span data-ttu-id="29363-120">Un local declarado en una `using` declaración local será implícitamente de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="29363-120">A local declared in a `using` local declaration will be implicitly read-only.</span></span> <span data-ttu-id="29363-121">Esto coincide con el comportamiento de las variables locales declaradas en una instrucción `using`.</span><span class="sxs-lookup"><span data-stu-id="29363-121">This matches the behavior of locals declared in a `using` statement.</span></span> 

<span data-ttu-id="29363-122">La gramática de lenguaje para las declaraciones de `using` será la siguiente:</span><span class="sxs-lookup"><span data-stu-id="29363-122">The language grammar for `using` declarations will be the following:</span></span>

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

<span data-ttu-id="29363-123">Restricciones en torno a la declaración de `using`:</span><span class="sxs-lookup"><span data-stu-id="29363-123">Restrictions around `using` declaration:</span></span>

- <span data-ttu-id="29363-124">No puede aparecer directamente dentro de un `case` etiqueta, sino que debe estar dentro de un bloque dentro de la etiqueta de `case`.</span><span class="sxs-lookup"><span data-stu-id="29363-124">May not appear directly inside a `case` label but instead must be within a block inside the `case` label.</span></span>
- <span data-ttu-id="29363-125">No puede aparecer como parte de una declaración de variable de `out`.</span><span class="sxs-lookup"><span data-stu-id="29363-125">May not appear as part of an `out` variable declaration.</span></span> 
- <span data-ttu-id="29363-126">Debe tener un inicializador para cada declarador.</span><span class="sxs-lookup"><span data-stu-id="29363-126">Must have an initializer for each declarator.</span></span>
- <span data-ttu-id="29363-127">El tipo local se debe poder convertir implícitamente en `IDisposable` o completar el patrón de `using`.</span><span class="sxs-lookup"><span data-stu-id="29363-127">The local type must be implicitly convertible to `IDisposable` or fulfill the `using` pattern.</span></span>

### <a name="pattern-based-using"></a><span data-ttu-id="29363-128">basado en patrones con</span><span class="sxs-lookup"><span data-stu-id="29363-128">pattern-based using</span></span>

<span data-ttu-id="29363-129">El lenguaje agregará la noción de un patrón descartable: es decir, un tipo que tiene un método de instancia accesible `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="29363-129">The language will add the notion of a disposable pattern: that is a type which has an accessible `Dispose` instance method.</span></span> <span data-ttu-id="29363-130">Los tipos que se ajustan al patrón descartable pueden participar en una instrucción o declaración `using` sin necesidad de implementar `IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="29363-130">Types which fit the disposable pattern can participate in a `using` statement or declaration without being required to implement `IDisposable`.</span></span> 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

<span data-ttu-id="29363-131">Esto permitirá a los desarrolladores aprovechar `using` en una serie de escenarios nuevos:</span><span class="sxs-lookup"><span data-stu-id="29363-131">This will allow developers to leverage `using` in a number of new scenarios:</span></span>

- <span data-ttu-id="29363-132">`ref struct`: estos tipos no pueden implementar interfaces hoy en día y, por lo tanto, no pueden participar en `using` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="29363-132">`ref struct`: These types can't implement interfaces today and hence can't participate in `using` statements.</span></span>
- <span data-ttu-id="29363-133">Los métodos de extensión permitirán a los desarrolladores aumentar los tipos de otros ensamblados para participar en `using` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="29363-133">Extension methods will allow developers to augment types in other assemblies to participate in `using` statements.</span></span>

<span data-ttu-id="29363-134">En la situación en la que un tipo puede convertirse implícitamente en `IDisposable` y también se ajusta al patrón descartable, se preferirá `IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="29363-134">In the situation where a type can be implicitly converted to `IDisposable` and also fits the disposable pattern, then `IDisposable` will be preferred.</span></span> <span data-ttu-id="29363-135">Aunque esto supone el enfoque opuesto de `foreach` (patrón preferido sobre la interfaz), es necesario para la compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="29363-135">While this takes the opposite approach of `foreach` (pattern preferred over interface) it is necessary for backwards compatibility.</span></span>

<span data-ttu-id="29363-136">Aquí también se aplican las mismas restricciones de una instrucción de `using` tradicional: las variables locales declaradas en el `using` son de solo lectura, un valor de `null` no provocará que se produzca una excepción, etc. La generación de código será diferente solo en que no se convierta en `IDisposable` antes de llamar a Dispose:</span><span class="sxs-lookup"><span data-stu-id="29363-136">The same restrictions from a traditional `using` statement apply here as well: local variables declared in the `using` are read-only, a `null` value will not cause an exception to be thrown, etc ... The code generation will be different only in that there will not be a cast to `IDisposable` before calling Dispose:</span></span>

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

<span data-ttu-id="29363-137">Para ajustarse al patrón descartable, el método `Dispose` debe ser accesible, sin parámetros y tener un tipo de valor devuelto `void`.</span><span class="sxs-lookup"><span data-stu-id="29363-137">In order to fit the disposable pattern the `Dispose` method must be accessible, parameterless and have a `void` return type.</span></span> <span data-ttu-id="29363-138">No hay otras restricciones.</span><span class="sxs-lookup"><span data-stu-id="29363-138">There are no other restrictions.</span></span> <span data-ttu-id="29363-139">Esto significa explícitamente que los métodos de extensión se pueden usar aquí.</span><span class="sxs-lookup"><span data-stu-id="29363-139">This explicitly means that extension methods can be used here.</span></span>

## <a name="considerations"></a><span data-ttu-id="29363-140">Consideraciones</span><span class="sxs-lookup"><span data-stu-id="29363-140">Considerations</span></span>

### <a name="case-labels-without-blocks"></a><span data-ttu-id="29363-141">etiquetas Case sin bloques</span><span class="sxs-lookup"><span data-stu-id="29363-141">case labels without blocks</span></span>

<span data-ttu-id="29363-142">Un `using declaration` no es válido directamente dentro de una etiqueta de `case` debido a las complicaciones en torno a su duración real.</span><span class="sxs-lookup"><span data-stu-id="29363-142">A `using declaration` is illegal directly inside a `case` label due to complications around its actual lifetime.</span></span> <span data-ttu-id="29363-143">Una posible solución es simplemente asignarle la misma duración que una `out var` en la misma ubicación.</span><span class="sxs-lookup"><span data-stu-id="29363-143">One potential solution is to simply give it the same lifetime as an `out var` in the same location.</span></span> <span data-ttu-id="29363-144">Se consideró la complejidad adicional de la implementación de características y la facilidad del trabajo (solo tiene que agregar un bloque a la etiqueta de `case`) no justificaba la toma de esta ruta.</span><span class="sxs-lookup"><span data-stu-id="29363-144">It was deemed the extra complexity to the feature implementation and the ease of the work around (just add a block to the `case` label) didn't justify taking this route.</span></span>

## <a name="future-expansions"></a><span data-ttu-id="29363-145">Futuras expansiones</span><span class="sxs-lookup"><span data-stu-id="29363-145">Future Expansions</span></span>

### <a name="fixed-locals"></a><span data-ttu-id="29363-146">variables locales fijas</span><span class="sxs-lookup"><span data-stu-id="29363-146">fixed locals</span></span>

<span data-ttu-id="29363-147">Una instrucción `fixed` tiene todas las propiedades de `using` instrucciones que motivaron la capacidad de tener `using` variables locales.</span><span class="sxs-lookup"><span data-stu-id="29363-147">A `fixed` statement has all of the properties of `using` statements that motivated the ability to have `using` locals.</span></span> <span data-ttu-id="29363-148">Se debe tener en cuenta que puede extender esta característica a `fixed` variables locales.</span><span class="sxs-lookup"><span data-stu-id="29363-148">Consideration should be given to extending this feature to `fixed` locals as well.</span></span> <span data-ttu-id="29363-149">Las reglas de duración y ordenación deben aplicarse igualmente bien para `using` y `fixed` aquí.</span><span class="sxs-lookup"><span data-stu-id="29363-149">The lifetime and ordering rules should apply equally well for `using` and `fixed` here.</span></span>
