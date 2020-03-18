---
ms.openlocfilehash: aa0c233e7739ae7a0a7752251aae6f89df18ba93
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483515"
---
# <a name="c-language-proposals"></a>C#Propuestas de idioma

Las propuestas de lenguaje son documentos vivos que describen el pensamiento actual sobre una característica de lenguaje determinado.

Las propuestas pueden ser *activas*, *inactivas*, *rechazadas* o *realizadas*. Las propuestas *activas* se almacenan directamente en la carpeta de propuestas, las propuestas *inactivas* y *rechazadas* se almacenan en las subcarpetas [inactivas](proposals/inactive) y [rechazadas](proposals/rejected) , y las propuestas *realizadas* se archivan en una carpeta correspondiente a la versión de idioma de la que forman parte.

## <a name="lifetime-of-a-proposal"></a>Duración de una propuesta

Una propuesta comienza su vida cuando el equipo de diseño de lenguaje decide que puede ser una buena adición al idioma en un día. Normalmente, se empezará a estar *activo*, pero si queremos capturar una idea sin querer trabajar en ella ahora, una propuesta también puede empezar en la subcarpeta *inactiva* . Las propuestas pueden incluso comenzar directamente en el estado *rechazado* , si queremos realizar un registro de algo que no pretendemos hacer. Por ejemplo, si no es posible implementar una solicitud frecuente y periódica, podemos capturarla como una propuesta rechazada.

Es posible que la propuesta comience como una idea en un [problema de discusión](https://github.com/dotnet/csharplang/labels/Discussion), o bien que proceda de debates en la reunión de diseño del lenguaje o llegue desde muchos otros frentes. Lo principal es que el equipo de diseño piensa que debe realizarse y que hay alguien que está dispuesto a escribirlo. Normalmente, un miembro del equipo de diseño de lenguaje se asignará como un experto para la característica, cuyo seguimiento se realizará a través de un [problema del experto](https://github.com/dotnet/csharplang/labels/Proposal%20champion). El campeón es responsable de mover la propuesta a través del proceso de diseño.

Una propuesta está *activa* Si avanza por el diseño y la implementación hacia una próxima versión. Una vez *finalizada*, es decir, una implementación se ha combinado en una versión y se ha especificado la característica, se mueve a un subdirectorio correspondiente a su versión.

Si una característica no es probable que sea el idioma, por ejemplo, porque no es factible, no parece agregar un valor suficiente o simplemente no es adecuado para el idioma, se [rechazará](proposals/rejected)el mismo. Si una característica tiene una promesa razonable, pero no se está dando prioridad a la que se está usando para trabajar, se puede declarar [inactiva](proposals/inactive) para evitar la confusión de la carpeta principal. Es perfectamente adecuado para que el trabajo se produzca en las propuestas inactivas o rechazadas y para que se restablezcan más adelante. Las categorías están allí para reflejar la intención del diseño actual.

## <a name="nature-of-a-proposal"></a>Naturaleza de una propuesta

Una propuesta debe seguir la [plantilla de propuesta](proposal-template.md). Una buena propuesta debe:

- Cabe en el espíritu general y estética del lenguaje.
- No introducir sintaxis sutilmente alternativa para las características existentes.
- Agregue un gran número de valores para un conjunto de usuarios claros.
- No agregue significativamente a la complejidad del lenguaje, especialmente para los nuevos usuarios.  

## <a name="discussion-of-proposals"></a>Discusión de propuestas

Los comentarios y los debates se producen en los [problemas de discusión](https://github.com/dotnet/csharplang/labels/Discussion). Cuando se agrega una nueva propuesta a la carpeta de propuestas, se debe anunciar en un problema de discusión del experto o el autor de la propuesta. 

 