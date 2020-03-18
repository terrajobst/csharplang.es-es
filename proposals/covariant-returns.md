---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483461"
---
# <a name="covariant-return-types"></a>tipos devueltos de covariante

* [x] propuesto
* [] Prototipo: no iniciado
* [] Implementación: no iniciada
* [] Especificación: no iniciada

## <a name="summary"></a>Resumen
[summary]: #summary

Admitir _tipos de valor devuelto covariante_. En concreto, permita que un método de reemplazo tenga un tipo de referencia más derivado que el método que reemplaza.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Es un patrón común en el código que los distintos nombres de método deben inventar para solucionar la restricción de lenguaje que las invalidaciones deben devolver el mismo tipo que el método invalidado. A continuación se muestra un ejemplo de la base de código Roslyn.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Admitir _tipos de valor devuelto covariante_. En concreto, permita que un método de reemplazo tenga un tipo de referencia más derivado que el método que reemplaza. Esto se aplica a los métodos y propiedades, y se admite en clases e interfaces.

Esto sería útil en el patrón de fábrica. Por ejemplo, en la base de código Roslyn deberíamos tener

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

La implementación de esto sería para que el compilador emita el método de reemplazo como un método virtual "nuevo" que oculta el método de la clase base, junto con un _método de puente_ que implementa el método de clase base con una llamada al método de la clase derivada.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

- [] Todos los cambios de idioma deben pagarse por sí mismos.
- [] Debemos asegurarnos de que el rendimiento sea razonable, incluso en el caso de las jerarquías de herencia profunda.
- [] Debemos asegurarnos de que los artefactos de la estrategia de traslación no afecten a la semántica del lenguaje, incluso cuando se utiliza un nuevo IL de compiladores antiguos.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Podríamos relajar las reglas de idioma ligeramente para permitir, en origen,

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- [] ¿Cómo funcionarán las API que se han compilado para usar esta característica en versiones anteriores del lenguaje?

## <a name="design-meetings"></a>Design Meetings

Ninguno todavía. Se ha realizado algún debate en <https://github.com/dotnet/roslyn/issues/357>.