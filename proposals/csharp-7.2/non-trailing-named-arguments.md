---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483647"
---
# <a name="non-trailing-named-arguments"></a>Argumentos con nombre no finales

## <a name="summary"></a>Resumen
[summary]: #summary
Permita que los argumentos con nombre se utilicen en la posición no final, siempre que se usen en su posición correcta. Por ejemplo: `DoSomething(isEmployed:true, name, age);`.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

La principal motivación es evitar escribir información redundante. Es habitual asignar un nombre a un argumento que sea un literal (como `null`, `true`) con el fin de aclarar el código, en lugar de pasar argumentos desordenados.
Actualmente no se permite (`CS1738`) a menos que todos los argumentos siguientes también se denominen.

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

Algunos ejemplos adicionales:
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

Esto también funcionaría con los parámetros:
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

En la sección § 7.5.1 (listas de argumentos), la especificación indica actualmente:
> Un *argumento* con un *argumento-Name* se conoce como argumento con __nombre__, mientras que un *argumento* sin *el argumento-Name* es un __argumento posicional__. Es un error que un argumento posicional aparezca después de un argumento con nombre en una *lista de argumentos*.

La propuesta es quitar este error y actualizar las reglas para buscar el parámetro correspondiente para un argumento (§ 7.5.1.1):

Argumentos en la lista de argumentos de constructores de instancia, métodos, indizadores y delegados:
- [reglas existentes]
- Un argumento sin nombre corresponde a ningún parámetro cuando está después de un argumento con nombre fuera de posición o un argumento params con nombre.

En concreto, esto impide que se invoque `void M(bool a = true, bool b = true, bool c = true, );` con `M(c: false, valueB);`. El primer argumento se usa fuera de la posición (el argumento se usa en la primera posición, pero el parámetro denominado "c" está en la tercera posición), por lo que los siguientes argumentos deben tener nombre.

En otras palabras, los argumentos con nombre no finales solo se permiten cuando el nombre y la posición dan como resultado el mismo parámetro correspondiente.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Esta propuesta agrava los matices existentes con argumentos con nombre en la resolución de sobrecarga. Por ejemplo:

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

Puede obtener esta situación hoy mismo intercambiando los parámetros:

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

Del mismo modo, si tiene dos métodos `void M(int a, int b)` y `void M(int x, string y)`, el `M(x: 1, 2)` de invocación errónea producirá un diagnóstico basado en la segunda sobrecarga ("no se puede convertir de ' int ' a ' String '"). Este problema ya existe cuando el argumento con nombre se usa en una posición final.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Hay un par de alternativas a tener en cuenta:

- El estado quo
- Proporcionar asistencia IDE para rellenar todos los nombres de los argumentos finales cuando se escribe un nombre específico en el centro.

Ambos sufren un mayor nivel de detalle, ya que introducen varios argumentos con nombre, incluso si solo se necesita un nombre de literal al principio de la lista de argumentos.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Design Meetings
[ldm]: #ldm
La característica se analizó brevemente en LDM el 16 de mayo de 2017, con aprobación en principio (Aceptar para pasar a propuesta/prototipo). También se analizó brevemente el 28 de junio de 2017.

Está relacionado con el análisis inicial https://github.com/dotnet/csharplang/issues/518 está relacionado con el problema experto https://github.com/dotnet/csharplang/issues/570
