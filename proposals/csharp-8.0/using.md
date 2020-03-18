---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483977"
---
# <a name="pattern-based-using-and-using-declarations"></a>"basado en patrones usando" y "usar declaraciones"

## <a name="summary"></a>Resumen

El lenguaje agregará dos nuevas capacidades en torno a la instrucción `using` para simplificar la administración de recursos: `using` debe reconocer un patrón descartable además de `IDisposable` y agregar una declaración de `using` al idioma.

## <a name="motivation"></a>Motivación

La instrucción `using` es una herramienta eficaz para la administración de recursos hoy en día, pero requiere bastante ceremonia. Los métodos que tienen una serie de recursos para administrar se pueden bloquear sintácticamente con una serie de instrucciones `using`. Esta carga de sintaxis es suficiente para que la mayoría de las instrucciones de estilo de codificación tengan una excepción en torno a las llaves para este escenario. 

La declaración de `using` elimina gran parte de la ceremonia aquí C# y se obtiene en su par con otros lenguajes que incluyen bloques de administración de recursos. Además, el `using` basado en patrones permite a los desarrolladores expandir el conjunto de tipos que pueden participar aquí. En muchos casos, quitar la necesidad de crear tipos de contenedor que solo existen para permitir un uso de valores en una instrucción `using`. 

Juntas, estas características permiten a los desarrolladores simplificar y expandir los escenarios en los que se pueden aplicar `using`.

## <a name="detailed-design"></a>Diseño detallado 

### <a name="using-declaration"></a>declaración using

El lenguaje permitirá agregar `using` a una declaración de variable local. Este tipo de declaración tendrá el mismo efecto que declarar la variable en una instrucción `using` en la misma ubicación.

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

La duración de un `using` local se extenderá hasta el final del ámbito en el que se declara. El `using` variables locales se eliminará en el orden inverso en el que se declaran. 

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

No hay ninguna restricción en torno a `goto`o cualquier otra construcción de flujo de control en el aspecto de una declaración de `using`. En su lugar, el código actúa igual que para la instrucción `using` equivalente:

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

Un local declarado en una `using` declaración local será implícitamente de solo lectura. Esto coincide con el comportamiento de las variables locales declaradas en una instrucción `using`. 

La gramática de lenguaje para las declaraciones de `using` será la siguiente:

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

Restricciones en torno a la declaración de `using`:

- No puede aparecer directamente dentro de un `case` etiqueta, sino que debe estar dentro de un bloque dentro de la etiqueta de `case`.
- No puede aparecer como parte de una declaración de variable de `out`. 
- Debe tener un inicializador para cada declarador.
- El tipo local se debe poder convertir implícitamente en `IDisposable` o completar el patrón de `using`.

### <a name="pattern-based-using"></a>basado en patrones con

El lenguaje agregará la noción de un patrón descartable: es decir, un tipo que tiene un método de instancia accesible `Dispose`. Los tipos que se ajustan al patrón descartable pueden participar en una instrucción o declaración `using` sin necesidad de implementar `IDisposable`. 

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

Esto permitirá a los desarrolladores aprovechar `using` en una serie de escenarios nuevos:

- `ref struct`: estos tipos no pueden implementar interfaces hoy en día y, por lo tanto, no pueden participar en `using` instrucciones.
- Los métodos de extensión permitirán a los desarrolladores aumentar los tipos de otros ensamblados para participar en `using` instrucciones.

En la situación en la que un tipo puede convertirse implícitamente en `IDisposable` y también se ajusta al patrón descartable, se preferirá `IDisposable`. Aunque esto supone el enfoque opuesto de `foreach` (patrón preferido sobre la interfaz), es necesario para la compatibilidad con versiones anteriores.

Aquí también se aplican las mismas restricciones de una instrucción de `using` tradicional: las variables locales declaradas en el `using` son de solo lectura, un valor de `null` no provocará que se produzca una excepción, etc. La generación de código será diferente solo en que no se convierta en `IDisposable` antes de llamar a Dispose:

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

Para ajustarse al patrón descartable, el método `Dispose` debe ser accesible, sin parámetros y tener un tipo de valor devuelto `void`. No hay otras restricciones. Esto significa explícitamente que los métodos de extensión se pueden usar aquí.

## <a name="considerations"></a>Consideraciones

### <a name="case-labels-without-blocks"></a>etiquetas Case sin bloques

Un `using declaration` no es válido directamente dentro de una etiqueta de `case` debido a las complicaciones en torno a su duración real. Una posible solución es simplemente asignarle la misma duración que una `out var` en la misma ubicación. Se consideró la complejidad adicional de la implementación de características y la facilidad del trabajo (solo tiene que agregar un bloque a la etiqueta de `case`) no justificaba la toma de esta ruta.

## <a name="future-expansions"></a>Futuras expansiones

### <a name="fixed-locals"></a>variables locales fijas

Una instrucción `fixed` tiene todas las propiedades de `using` instrucciones que motivaron la capacidad de tener `using` variables locales. Se debe tener en cuenta que puede extender esta característica a `fixed` variables locales. Las reglas de duración y ordenación deben aplicarse igualmente bien para `using` y `fixed` aquí.
