---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483563"
---
# <a name="lambda-attributes"></a>Atributos lambda

* [x] propuesto
* [] Prototipo
* [] Implementación
* [] Especificación

## <a name="summary"></a>Resumen
[summary]: #summary

Permite que los atributos se apliquen a las expresiones lambda (y métodos anónimos) y a los parámetros de método lambda/anónimo, ya que pueden estar en métodos regulares.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Dos motivaciones principales:

1. Para proporcionar metadatos visibles para los analizadores en tiempo de compilación.
2. Para proporcionar metadatos visibles para la reflexión y las herramientas en tiempo de ejecución.

Como ejemplo de (1): en el caso de código que tiene en cuenta el rendimiento, resulta útil poder tener un analizador que se marque cuando se asignen cierres y delegados para las expresiones lambda que se cierran con el estado.  A menudo, un desarrollador de este código sale de su forma de evitar la captura de cualquier Estado, de modo que el compilador puede generar un método estático y un delegado que se puede almacenar en caché para el método, o bien el desarrollador se asegurará de que el único Estado que se cierra es `this`, lo que permite al compilador al menos asignar un objeto de cierre.  Pero sin la compatibilidad de idioma para limitar lo que se puede capturar, es demasiado fácil cerrar accidentalmente el estado.  Sería útil si un desarrollador pudiera anotar lambdas con atributos para indicar el estado con el que se pueden cerrar, por ejemplo:

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

Después, se puede escribir un analizador para que se marque cuando el estado se Capture incorrectamente, por ejemplo:

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

- Con la misma sintaxis de atributo que en los métodos normales, los atributos se pueden aplicar al principio de una expresión lambda o un método anónimo, por ejemplo:

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- Para evitar la ambigüedad en lo que se refiere a si un atributo se aplica al método lambda o a uno de los argumentos, los atributos solo se pueden usar cuando se usan paréntesis en torno a cualquier argumento, por ejemplo:

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- Con métodos anónimos, no se necesitan paréntesis para aplicar un atributo al método antes de la palabra clave `delegate`, por ejemplo:

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- Se pueden aplicar varios atributos, ya sea a través de la sintaxis delimitada por comas estándar o a través de la sintaxis de atributo completo, por ejemplo:

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- Los atributos se pueden aplicar a los parámetros a un método anónimo o una expresión lambda, pero solo cuando se usan paréntesis alrededor de cualquier argumento, por ejemplo:

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- Se pueden aplicar varios atributos a los parámetros de un método anónimo o una expresión lambda, mediante la sintaxis de atributos completos o delimitados por comas, por ejemplo:

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- los atributos de destino de `return`también se pueden utilizar en lambdas, por ejemplo:

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- El compilador genera los atributos en el método generado y los argumentos para esos métodos, tal y como lo haría para cualquier otro método.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

N/D

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

N/D

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

N/D

## <a name="design-meetings"></a>Design Meetings

N/D