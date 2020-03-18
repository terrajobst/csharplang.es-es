---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483635"
---
# <a name="pattern-based-fixed-statement"></a>Instrucción `fixed` basada en patrones

## <a name="summary"></a>Resumen
[summary]: #summary

Introduzca un patrón que permita a los tipos participar en `fixed` instrucciones. 

## <a name="motivation"></a>Motivación
[motivation]: #motivation

El lenguaje proporciona un mecanismo para anclar datos administrados y obtener un puntero nativo al búfer subyacente.

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

El conjunto de tipos que pueden participar en `fixed` se codifica y se limita a las matrices y `System.String`. Los tipos "especiales" de codificar no se escalan cuando se introducen nuevos primitivos como `ImmutableArray<T>`, `Span<T>``Utf8String`. 

Además, la solución actual para `System.String` se basa en una API bastante rígida. La forma de la API implica que `System.String` es un objeto contiguo que incrusta los datos codificados en UTF16 en un desplazamiento fijo del encabezado de objeto. Este enfoque se ha encontrado problemático en varias propuestas que podrían requerir cambios en el diseño subyacente. Sería conveniente poder cambiar a algo más flexible que desacople `System.String` objeto de su representación interna con el fin de interoperabilidad no administrada. 

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

## <a name="pattern"></a>*Patrón* ##
Un "fijo" viable basado en patrones debe:
-   Proporcione las referencias administradas para anclar la instancia e inicializar el puntero (preferiblemente es la misma referencia).
-   Transmite de forma inequívoca el tipo del elemento no administrado (es decir, "Char" para "String").
-   Prescribe el comportamiento en caso "vacío" cuando no hay nada al que hacer referencia. 
-   No debe enviar a los creadores de la API hacia las decisiones de diseño que perjudiquen el uso del tipo fuera de `fixed`.

Creo que lo anterior se podría satisfacer reconociendo un miembro de devolución especial con nombre: `ref [readonly] T GetPinnableReference()`.

Para que la instrucción `fixed` la use, deben cumplirse las siguientes condiciones:

1. Solo se proporciona un miembro de este tipo para un tipo.
1. Devuelve por `ref` o `ref readonly`. (`readonly` se permite para que los autores de tipos inmutables y de solo lectura puedan implementar el patrón sin agregar la API grabable que podría usarse en código seguro)
1. T es un tipo no administrado.
(dado que `T*` se convierte en el tipo de puntero. La restricción se expande de forma natural si se expande la noción de "no administrada")
1. Devuelve `nullptr` administrados cuando no hay datos para anclar, probablemente la manera más barata de transmitir Emptiness.
(tenga en cuenta que "" cadena devuelve una referencia a "\ 0", ya que las cadenas terminan en null)

Como alternativa, en el `#3` podemos permitir que el resultado en casos vacíos sea no definido o específico de la implementación. Sin embargo, eso puede hacer que la API sea más peligrosa y propenso a abusos y cargas de compatibilidad no intencionadas. 

## <a name="translation"></a>*Traducción* ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

se convierte en el pseudocódigo siguiente (no todos los elementos C#que se van a expresar en)

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

- GetPinnableReference está pensado para usarse solo en `fixed`, pero nada impide su uso en código seguro, por lo que el implementador debe tenerlo en cuenta.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Los usuarios pueden introducir GetPinnableReference o un miembro similar y usarlo como
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

No hay ninguna solución para `System.String` si se desea una solución alternativa.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- [] Comportamiento en estado "vacío".¿ - `nullptr` o `undefined`? 
- [] ¿Deben tenerse en cuenta los métodos de extensión? 
- [] Si se detecta un patrón en `System.String`, ¿debe ganarse? 

## <a name="design-meetings"></a>Design Meetings

Ninguno todavía. 
