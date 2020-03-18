---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483479"
---
# <a name="static-delegates"></a>Delegados estáticos

* [x] propuesto
* [] Prototipo: no iniciado
* [] Implementación: no iniciada
* [] Especificación: no iniciada

## <a name="summary"></a>Resumen
[summary]: #summary

Proporcionar una funcionalidad de devolución de llamada ligera y de uso C# general al lenguaje.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Hoy en día, los usuarios tienen la capacidad de crear devoluciones de llamada mediante el tipo de `System.Delegate`. Sin embargo, son bastante pesadas (como requerir una asignación de montón y siempre tener el control para encadenar las devoluciones de llamada juntas).

Además, `System.Delegate` no proporciona la mejor interoperabilidad con punteros de función no administrados, es decir, debido a que no se pueden representar como bits/bytes y requerir la serialización cada vez que atraviesa el límite administrado o no administrado.

Con unos pocos ajustes menores, podríamos proporcionar un nuevo tipo de delegado que sea ligero, de uso general y de interoperabilidad con código nativo.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Uno declararía un delegado estático mediante lo siguiente:

```C#
static delegate int Func()
```

También podría atribuir la declaración con algo similar a `System.Runtime.InteropServices.UnmanagedFunctionPointer` de modo que se pueda controlar la Convención de llamada, la serialización de cadenas y el comportamiento del último error. Nota: el uso de `System.Runtime.InteropServices.UnmanagedFunctionPointer` mismo no funcionará, ya que solo se puede usar en los delegados.

El compilador traducirá la declaración en una representación interna similar a la siguiente

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

Es decir, se representa internamente mediante un struct que tiene un único miembro de tipo `IntPtr` (este tipo de estructura es un elemento que no se tiene en cuenta y no tiene ninguna asignación de montón):
* El miembro contiene la dirección de la función que va a ser la devolución de llamada.
* El tipo declara un método que coincide con la firma del método de la devolución de llamada.
* El nombre del struct no debe ser constructor por el usuario (como hacemos con otras estructuras de respaldo generadas internamente).
 * Por ejemplo: los búferes de tamaño fijo generan un struct con un nombre con el formato `<name>e__FixedBuffer` (`<` y `>` forman parte del identificador y hacen que el identificador no se pueda construir en C#, pero todavía se puede seguir usando en Il).
* El nombre de la declaración del método debe ser un nombre conocido que se utiliza en todos los tipos de delegado estáticos (esto permite que el compilador conozca el nombre que se debe buscar al determinar la firma).

El valor del delegado estático solo se puede enlazar a un método estático que coincida con la firma de la devolución de llamada.

No se admite el encadenamiento de devoluciones de llamada juntas.

La invocación de la devolución de llamada la implementaría la instrucción `calli`.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Los delegados estáticos no funcionarían con las API existentes que usan delegados normales (es necesario ajustar dicho delegado estático en un delegado normal de la misma firma).
* Dado que `System.Delegate` se representa internamente como un conjunto de campos de `object` y `IntPtr` (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), sería posible permitir la conversión implícita de un delegado estático a una `System.Delegate` que tenga una firma de método coincidente. También sería posible proporcionar una conversión explícita en la dirección opuesta, siempre que el `System.Delegate` se ajuste a todos los requisitos de ser un delegado estático.

Se necesitará trabajo adicional para que el delegado estático pueda usarse fácilmente en el marco principal.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

TBD

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

¿Qué partes del diseño siguen siendo TBD?

## <a name="design-meetings"></a>Design Meetings

Vínculo a las notas de diseño que afectan a esta propuesta y describa en una oración cada uno de los cambios que han producido.


