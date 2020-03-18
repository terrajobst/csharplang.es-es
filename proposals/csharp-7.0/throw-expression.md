---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483539"
---
# <a name="throw-expression"></a><span data-ttu-id="edf5a-101">Expresión Throw</span><span class="sxs-lookup"><span data-stu-id="edf5a-101">Throw expression</span></span>

<span data-ttu-id="edf5a-102">Se extiende el conjunto de formularios de expresión que se va a incluir</span><span class="sxs-lookup"><span data-stu-id="edf5a-102">We extend the set of expression forms to include</span></span>

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

<span data-ttu-id="edf5a-103">Las reglas de tipo son las siguientes:</span><span class="sxs-lookup"><span data-stu-id="edf5a-103">The type rules are as follows:</span></span>

- <span data-ttu-id="edf5a-104">Un *throw_expression* no tiene ningún tipo.</span><span class="sxs-lookup"><span data-stu-id="edf5a-104">A *throw_expression* has no type.</span></span>
- <span data-ttu-id="edf5a-105">Un *throw_expression* se pueda convertir en cada tipo mediante una conversión implícita.</span><span class="sxs-lookup"><span data-stu-id="edf5a-105">A *throw_expression* is convertible to every type by an implicit conversion.</span></span>

<span data-ttu-id="edf5a-106">Una *expresión Throw* inicia el valor generado al evaluar el *null_coalescing_expression*, que debe indicar un valor del tipo de clase `System.Exception`, de un tipo de clase que se deriva de `System.Exception` o de un tipo de parámetro de tipo que tiene `System.Exception` (o una subclase de ella) como su clase base efectiva.</span><span class="sxs-lookup"><span data-stu-id="edf5a-106">A *throw expression* throws the value produced by evaluating the *null_coalescing_expression*, which must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="edf5a-107">Si la evaluación de la expresión produce `null`, se produce una `System.NullReferenceException` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="edf5a-107">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="edf5a-108">El comportamiento en tiempo de ejecución de la evaluación de una *expresión Throw* es el mismo [que el especificado para una *instrucción throw*](../../spec/statements.md#the-throw-statement).</span><span class="sxs-lookup"><span data-stu-id="edf5a-108">The behavior at runtime of the evaluation of a *throw expression* is the same [as specified for a *throw statement*](../../spec/statements.md#the-throw-statement).</span></span>

<span data-ttu-id="edf5a-109">Las reglas de análisis de flujo son las siguientes:</span><span class="sxs-lookup"><span data-stu-id="edf5a-109">The flow-analysis rules are as follows:</span></span>

- <span data-ttu-id="edf5a-110">Para cada variable *v*, *v* se asigna definitivamente antes de la *null_coalescing_expression* de un *throw_expression* iff que se asigna definitivamente antes de la *throw_expression*.</span><span class="sxs-lookup"><span data-stu-id="edf5a-110">For every variable *v*, *v* is definitely assigned before the *null_coalescing_expression* of a *throw_expression* iff it is definitely assigned before the *throw_expression*.</span></span>
- <span data-ttu-id="edf5a-111">Para *cada variable, v se*asigna definitivamente *después* de *throw_expression*.</span><span class="sxs-lookup"><span data-stu-id="edf5a-111">For every variable *v*, *v* is definitely assigned after *throw_expression*.</span></span>

<span data-ttu-id="edf5a-112">Se permite una *expresión Throw* solo en los siguientes contextos sintácticos:</span><span class="sxs-lookup"><span data-stu-id="edf5a-112">A *throw expression* is permitted in only the following syntactic contexts:</span></span>
- <span data-ttu-id="edf5a-113">Como segundo o tercer operando de un operador condicional ternario `?:`</span><span class="sxs-lookup"><span data-stu-id="edf5a-113">As the second or third operand of a ternary conditional operator `?:`</span></span>
- <span data-ttu-id="edf5a-114">Como segundo operando de un operador de uso combinado de NULL `??`</span><span class="sxs-lookup"><span data-stu-id="edf5a-114">As the second operand of a null coalescing operator `??`</span></span>
- <span data-ttu-id="edf5a-115">Como cuerpo de un método o una expresión lambda con forma de expresión.</span><span class="sxs-lookup"><span data-stu-id="edf5a-115">As the body of an expression-bodied lambda or method.</span></span>
