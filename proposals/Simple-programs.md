---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "79484121"
---
# <a name="simple-programs"></a>Programas simples

* [x] propuesto
* [x] prototipo: iniciado
* [] Implementación: no iniciada
* [] Especificación: no iniciada

## <a name="summary"></a>Resumen
[summary]: #summary

Permita que una secuencia de *instrucciones* se produzca justo antes de la *namespace_member_declaration*de un *compilation_unit* (es decir, un archivo de código fuente).

La semántica es que, si dicha secuencia de *instrucciones* está presente, se emitirá la siguiente declaración de tipos, módulo el nombre de tipo real y el nombre del método:

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

Consulte también https://github.com/dotnet/csharplang/issues/3117.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Hay una determinada cantidad de reutilización que se encuentra en la parte más sencilla de los programas, debido a la necesidad de un método `Main` explícito. Esto parece ser un aprendizaje lingüístico y una claridad de los programas. Por lo tanto, el objetivo principal de la característica C# es permitir programas sin reutilizaciones innecesarias en torno a ellos, por lo que los aprendidos y la claridad del código.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

### <a name="syntax"></a>Sintaxis

La única sintaxis adicional es permitir una secuencia de *instrucciones*en una unidad de compilación, justo antes de la *namespace_member_declaration*s:

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

En todo menos uno *compilation_unit* la *instrucción*s debe ser declaraciones de función local. 

Ejemplo:

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

### <a name="semantics"></a>Semántica

Si las instrucciones de nivel superior están presentes en cualquier unidad de compilación del programa, el significado es como si se combinaran en el cuerpo del bloque de un método `Main` de una clase `Program` en el espacio de nombres global, como se indica a continuación:

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

Tenga en cuenta que los nombres "Program" y "Main" se usan solo con fines ilustrativos, los nombres reales usados por el compilador dependen de la implementación y no se puede hacer referencia al método por el nombre del código fuente.

El método se designa como el punto de entrada del programa. Los métodos declarados explícitamente que por Convención se pueden considerar como candidatos de punto de entrada se omiten. Cuando esto sucede, se genera una advertencia. Es un error especificar `-main:<type>` modificador del compilador.

Si alguna de las unidades de compilación tiene instrucciones que no son declaraciones de funciones locales, las instrucciones de esa unidad de compilación se producen primero. Esto hace que sea válido para las funciones locales de un archivo para hacer referencia a variables locales en otro. El orden de las contribuciones de instrucciones (que serían todas las funciones locales) de otras unidades de compilación es indefinido.

Las operaciones asincrónicas se permiten en las instrucciones de nivel superior hasta el grado en que se permiten en las instrucciones de un método de punto de entrada asincrónico normal. Sin embargo, no son necesarios si `await` expresiones y otras operaciones asincrónicas se omiten, no se genera ninguna advertencia. En su lugar, la firma del método de punto de entrada generado es equivalente a 
``` c#
    static void Main()
```

En el ejemplo anterior se produciría la siguiente declaración de método `$Main`:

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

Al mismo tiempo, un ejemplo como este:
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

produciría:
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

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>Ámbito de las variables locales de nivel superior y las funciones locales

Aunque las funciones y las variables locales de nivel superior se "encapsulan" en el método de punto de entrada generado, deben seguir en el ámbito de todo el programa.
Para la evaluación de nombre simple, una vez que se alcanza el espacio de nombres global:
- En primer lugar, se realiza un intento de evaluar el nombre dentro del método de punto de entrada generado y solo si se produce un error en este intento. 
- Se realiza la evaluación "normal" dentro de la declaración del espacio de nombres global. 

Esto podría dar lugar a la sombra de nombres de espacios de nombres y tipos declarados en el espacio de nombres global, así como a la sombra de nombres importados.

Si la evaluación del nombre simple se produce fuera de las instrucciones de nivel superior y la evaluación produce una variable o función local de nivel superior, esto debe producir un error.

De esta forma, se protege nuestra futura capacidad para abordar mejor las "funciones de nivel superior" (escenario 2 en https://github.com/dotnet/csharplang/issues/3117)y se pueden proporcionar diagnósticos útiles a los usuarios que no creen que se admitan por equivocación.

