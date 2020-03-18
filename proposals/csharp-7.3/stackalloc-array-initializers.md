---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483593"
---
# <a name="stackalloc-array-initializers"></a>Inicializadores de matriz stackalloc

## <a name="summary"></a>Resumen
[summary]: #summary

Permite usar la sintaxis del inicializador de matriz con `stackalloc`.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Las matrices normales pueden inicializar sus elementos en el momento de su creación. Parece razonable permitirlo en `stackalloc` caso.

La pregunta de por qué no se permite esta sintaxis con `stackalloc` se produce con bastante frecuencia.  
Consulte, por ejemplo, [#1112](https://github.com/dotnet/csharplang/issues/1112)

## <a name="detailed-design"></a>Diseño detallado

Las matrices ordinarias se pueden crear mediante la sintaxis siguiente:

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

Deberíamos permitir la creación de matrices asignadas de pila mediante:  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

La semántica de todos los casos es aproximadamente la misma que con las matrices.  
Por ejemplo: en el último caso, el tipo de elemento se deduce del inicializador y debe ser un tipo "no administrado".

Nota: la característica no depende de que el destino sea un `Span<T>`. Es tan aplicable en `T*` caso, por lo que no parece razonable que lo predicase en `Span<T>` caso.  

## <a name="translation"></a>Traducción

La implementación de Naive podría simplemente inicializar la matriz justo después de la creación a través de una serie de asignaciones de elementos.  

Del mismo modo que con las matrices, puede ser posible y deseable detectar los casos en los que todos o la mayoría de los elementos son tipos que pueden transferirse en exceso de bits y usar técnicas más eficaces copiando el estado creado previamente de todos los elementos constantes. 

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Esta es una característica útil. Solo es posible hacer nada.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Design Meetings

Ninguno todavía. 
