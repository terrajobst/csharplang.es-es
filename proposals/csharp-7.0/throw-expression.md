---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483539"
---
# <a name="throw-expression"></a>Expresión Throw

Se extiende el conjunto de formularios de expresión que se va a incluir

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

Las reglas de tipo son las siguientes:

- Un *throw_expression* no tiene ningún tipo.
- Un *throw_expression* se pueda convertir en cada tipo mediante una conversión implícita.

Una *expresión Throw* inicia el valor generado al evaluar el *null_coalescing_expression*, que debe indicar un valor del tipo de clase `System.Exception`, de un tipo de clase que se deriva de `System.Exception` o de un tipo de parámetro de tipo que tiene `System.Exception` (o una subclase de ella) como su clase base efectiva. Si la evaluación de la expresión produce `null`, se produce una `System.NullReferenceException` en su lugar.

El comportamiento en tiempo de ejecución de la evaluación de una *expresión Throw* es el mismo [que el especificado para una *instrucción throw*](../../spec/statements.md#the-throw-statement).

Las reglas de análisis de flujo son las siguientes:

- Para cada variable *v*, *v* se asigna definitivamente antes de la *null_coalescing_expression* de un *throw_expression* iff que se asigna definitivamente antes de la *throw_expression*.
- Para *cada variable, v se*asigna definitivamente *después* de *throw_expression*.

Se permite una *expresión Throw* solo en los siguientes contextos sintácticos:
- Como segundo o tercer operando de un operador condicional ternario `?:`
- Como segundo operando de un operador de uso combinado de NULL `??`
- Como cuerpo de un método o una expresión lambda con forma de expresión.
