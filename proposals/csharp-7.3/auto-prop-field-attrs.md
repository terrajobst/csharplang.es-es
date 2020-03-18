---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483641"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a>Atributos de destino de campo de propiedades implementadas automáticamente

## <a name="summary"></a>Resumen
[summary]: #summary

Esta característica intenta permitir a los desarrolladores aplicar atributos directamente a los campos de respaldo de las propiedades implementadas automáticamente.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Actualmente no es posible aplicar atributos a los campos de respaldo de las propiedades implementadas automáticamente.  En aquellos casos en los que el desarrollador debe utilizar un atributo de destino de campo, se les obliga a declarar el campo manualmente y usar la sintaxis de propiedad más detallada.  Dado que C# siempre se admiten los atributos de destino de campo en el campo de respaldo generado para los eventos, tiene sentido extender la misma funcionalidad a su más familiar.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

En Resumen, lo siguiente sería legal C# y no produciría una advertencia:

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

Esto daría lugar a la aplicación de los atributos de destino de campo al campo de respaldo generado por el compilador:

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

Como se mencionó, esto proporciona la paridad con la C# sintaxis de eventos desde 1,0, ya que lo siguiente es válido y se comporta como se esperaba:

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Hay dos posibles inconvenientes en la implementación de este cambio:

1. Al intentar aplicar un atributo al campo de una propiedad implementada automáticamente, se genera una advertencia del compilador que indica que los atributos de ese bloque se omitirán.  Si el compilador se ha cambiado para admitir esos atributos, se les aplicaría al campo de respaldo en una recompilación posterior que podría modificar el comportamiento del programa en tiempo de ejecución.
1. El compilador no valida actualmente los destinos de AttributeUsage de los atributos al intentar aplicarlos al campo de la propiedad implementada automáticamente.  Si el compilador se ha cambiado para admitir atributos dirigidos a campos y el atributo en cuestión no se puede aplicar a un campo, el compilador emitirá un error en lugar de una advertencia, interrumpiendo la compilación.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Design Meetings
