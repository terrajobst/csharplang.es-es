---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483689"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a>Inferir nombres de tupla (también conocidos como inicializadores de proyección de tupla)

## <a name="summary"></a>Resumen
[summary]: #summary

En una serie de casos comunes, esta característica permite omitir los nombres de elemento de tupla y, en su lugar, deducirlos. Por ejemplo, en lugar de escribir `(f1: x.f1, f2: x?.f2)`, los nombres de elemento "F1" y "F2" se pueden inferir de `(x.f1, x?.f2)`.

Esto es paralelo al comportamiento de los tipos anónimos, que permiten deducir los nombres de miembro durante la creación. Por ejemplo, `new { x.f1, y?.f2 }` declara los miembros "F1" y "F2".

Esto es especialmente útil cuando se usan tuplas en LINQ:

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

Hay dos partes del cambio:

1.  Intente inferir un nombre de candidato para cada elemento de tupla que no tenga un nombre explícito:
    -   Usar las mismas reglas que la inferencia de nombres para los tipos anónimos.
        - En C#, esto permite tres casos: `y` (identificador), `x.y` (acceso a miembros simple) y `x?.y` (acceso condicional).
        - En VB, esto permite casos adicionales, como `x.y()`.
    -   Rechazar nombres de tupla reservados (distingue mayúsculas de C#minúsculas en, sin distinción de mayúsculas y minúsculas en VB), ya que están prohibidos o ya están implícitos. Por ejemplo, como `ItemN`, `Rest`y `ToString`.
    -   Si los nombres de candidatos son duplicados (con distinción de C#mayúsculas y minúsculas en, sin distinción de mayúsculas y minúsculas en VB) en toda la tupla, se quitan esos candidatos.
2.  Durante las conversiones (que comprueban y avisan sobre la eliminación de nombres de los literales de tupla), los nombres inferidos no generarán ninguna advertencia. Esto evita que se interrumpa el código de tupla existente.

Tenga en cuenta que la regla para administrar duplicados es diferente de la de los tipos anónimos. Por ejemplo, `new { x.f1, x.f1 }` produce un error, pero todavía se permite el `(x.f1, x.f1)` (solo sin nombres inferidos). Esto evita que se interrumpa el código de tupla existente.

Por coherencia, lo mismo se aplica a las tuplas generadas por las asignaciones de desconstrucción ( C#en):

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

Lo mismo se aplica también a las tuplas de VB, mediante las reglas específicas de VB para deducir el nombre de las comparaciones de expresiones y de nombres que no distinguen mayúsculas de minúsculas.

Al usar el C# compilador 7,1 (o posterior) con la versión de idioma "7,0", los nombres de los elementos se deducen (a pesar de que la característica no esté disponible), pero se producirá un error de uso del sitio para intentar tener acceso a ellos. Esto limitará las adiciones de código nuevo que posteriormente se verán ante el problema de compatibilidad (que se describe a continuación).

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

La principal desventaja es que esto presenta una interrupción de compatibilidad C# de 7,0:

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

El Consejo de compatibilidad encontró este salto aceptable, dado que es limitado y el período de tiempo desde que se enviaron C# tuplas (en 7,0) es corto.

## <a name="references"></a>Referencias
- [LDM, 4 de abril de 2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- [Debate en github](https://github.com/dotnet/csharplang/issues/370) (gracias @alrz para llevar a cabo este problema)
- [Diseño de tuplas](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
