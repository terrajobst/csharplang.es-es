---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483641"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a><span data-ttu-id="664f4-101">Atributos de destino de campo de propiedades implementadas automáticamente</span><span class="sxs-lookup"><span data-stu-id="664f4-101">Auto-Implemented Property Field-Targeted Attributes</span></span>

## <a name="summary"></a><span data-ttu-id="664f4-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="664f4-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="664f4-103">Esta característica intenta permitir a los desarrolladores aplicar atributos directamente a los campos de respaldo de las propiedades implementadas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="664f4-103">This feature intends to allow developers to apply attributes directly to the backing fields of auto-implemented properties.</span></span>

## <a name="motivation"></a><span data-ttu-id="664f4-104">Motivación</span><span class="sxs-lookup"><span data-stu-id="664f4-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="664f4-105">Actualmente no es posible aplicar atributos a los campos de respaldo de las propiedades implementadas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="664f4-105">Currently it is not possible to apply attributes to the backing fields of auto-implemented properties.</span></span>  <span data-ttu-id="664f4-106">En aquellos casos en los que el desarrollador debe utilizar un atributo de destino de campo, se les obliga a declarar el campo manualmente y usar la sintaxis de propiedad más detallada.</span><span class="sxs-lookup"><span data-stu-id="664f4-106">In those cases where the developer must use a field-targeting attribute they are forced to declare the field manually and use the more verbose property syntax.</span></span>  <span data-ttu-id="664f4-107">Dado que C# siempre se admiten los atributos de destino de campo en el campo de respaldo generado para los eventos, tiene sentido extender la misma funcionalidad a su más familiar.</span><span class="sxs-lookup"><span data-stu-id="664f4-107">Given that C# has always supported field-targeted attributes on the generated backing field for events it makes sense to extend the same functionality to their property kin.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="664f4-108">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="664f4-108">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="664f4-109">En Resumen, lo siguiente sería legal C# y no produciría una advertencia:</span><span class="sxs-lookup"><span data-stu-id="664f4-109">In short, the following would be legal C# and not produce a warning:</span></span>

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

<span data-ttu-id="664f4-110">Esto daría lugar a la aplicación de los atributos de destino de campo al campo de respaldo generado por el compilador:</span><span class="sxs-lookup"><span data-stu-id="664f4-110">This would result in the field-targeted attributes being applied to the compiler-generated backing field:</span></span>

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

<span data-ttu-id="664f4-111">Como se mencionó, esto proporciona la paridad con la C# sintaxis de eventos desde 1,0, ya que lo siguiente es válido y se comporta como se esperaba:</span><span class="sxs-lookup"><span data-stu-id="664f4-111">As mentioned, this brings parity with event syntax from C# 1.0 as the following is already legal and behaves as expected:</span></span>

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a><span data-ttu-id="664f4-112">Desventajas</span><span class="sxs-lookup"><span data-stu-id="664f4-112">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="664f4-113">Hay dos posibles inconvenientes en la implementación de este cambio:</span><span class="sxs-lookup"><span data-stu-id="664f4-113">There are two potential drawbacks to implementing this change:</span></span>

1. <span data-ttu-id="664f4-114">Al intentar aplicar un atributo al campo de una propiedad implementada automáticamente, se genera una advertencia del compilador que indica que los atributos de ese bloque se omitirán.</span><span class="sxs-lookup"><span data-stu-id="664f4-114">Attempting to apply an attribute to the field of an auto-implemented property produces a compiler warning that the attributes in that block will be ignored.</span></span>  <span data-ttu-id="664f4-115">Si el compilador se ha cambiado para admitir esos atributos, se les aplicaría al campo de respaldo en una recompilación posterior que podría modificar el comportamiento del programa en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="664f4-115">If the compiler were changed to support those attributes they would be applied to the backing field on a subsequent recompilation which could alter the behavior of the program at runtime.</span></span>
1. <span data-ttu-id="664f4-116">El compilador no valida actualmente los destinos de AttributeUsage de los atributos al intentar aplicarlos al campo de la propiedad implementada automáticamente.</span><span class="sxs-lookup"><span data-stu-id="664f4-116">The compiler does not currently validate the AttributeUsage targets of the attributes when attempting to apply them to the field of the auto-implemented property.</span></span>  <span data-ttu-id="664f4-117">Si el compilador se ha cambiado para admitir atributos dirigidos a campos y el atributo en cuestión no se puede aplicar a un campo, el compilador emitirá un error en lugar de una advertencia, interrumpiendo la compilación.</span><span class="sxs-lookup"><span data-stu-id="664f4-117">If the compiler were changed to support field-targeted attributes and the attribute in question cannot be applied to a field the compiler would emit an error instead of a warning, breaking the build.</span></span>

## <a name="alternatives"></a><span data-ttu-id="664f4-118">Alternativas</span><span class="sxs-lookup"><span data-stu-id="664f4-118">Alternatives</span></span>
[alternatives]: #alternatives

## <a name="unresolved-questions"></a><span data-ttu-id="664f4-119">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="664f4-119">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="664f4-120">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="664f4-120">Design meetings</span></span>
