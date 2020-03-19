---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509496"
---
# <a name="attributes-on-local-functions"></a><span data-ttu-id="f3628-101">Atributos en funciones locales</span><span class="sxs-lookup"><span data-stu-id="f3628-101">Attributes on local functions</span></span>

## <a name="attributes"></a><span data-ttu-id="f3628-102">Atributos</span><span class="sxs-lookup"><span data-stu-id="f3628-102">Attributes</span></span>

<span data-ttu-id="f3628-103">Ahora se permite que las declaraciones de función local tengan [atributos](../spec/attributes.md).</span><span class="sxs-lookup"><span data-stu-id="f3628-103">Local function declarations are now permitted to have [attributes](../spec/attributes.md).</span></span> <span data-ttu-id="f3628-104">Los parámetros y los parámetros de tipo de las funciones locales también pueden tener atributos.</span><span class="sxs-lookup"><span data-stu-id="f3628-104">Parameters and type parameters on local functions are also allowed to have attributes.</span></span>

<span data-ttu-id="f3628-105">Los atributos con un significado especificado cuando se aplican a un método, sus parámetros o sus parámetros de tipo tendrán el mismo significado cuando se aplican a una función local, sus parámetros o sus parámetros de tipo, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="f3628-105">Attributes with a specified meaning when applied to a method, its parameters, or its type parameters will have the same meaning when applied to a local function, its parameters, or its type parameters, respectively.</span></span>

<span data-ttu-id="f3628-106">Una función local puede hacerse condicional en el mismo sentido que un [método condicional](../spec/attributes.md#the-conditional-attribute) al decorarla con un `[ConditionalAttribute]`.</span><span class="sxs-lookup"><span data-stu-id="f3628-106">A local function can be made conditional in the same sense as a [conditional method](../spec/attributes.md#the-conditional-attribute) by decorating it with a `[ConditionalAttribute]`.</span></span> <span data-ttu-id="f3628-107">También se debe `static`una función local condicional.</span><span class="sxs-lookup"><span data-stu-id="f3628-107">A conditional local function must also be `static`.</span></span> <span data-ttu-id="f3628-108">Todas las restricciones de los métodos condicionales también se aplican a las funciones locales condicionales, lo que incluye que el tipo de valor devuelto debe ser `void`.</span><span class="sxs-lookup"><span data-stu-id="f3628-108">All restrictions on conditional methods also apply to conditional local functions, including that the return type must be `void`.</span></span>

## <a name="extern"></a><span data-ttu-id="f3628-109">Externas</span><span class="sxs-lookup"><span data-stu-id="f3628-109">Extern</span></span>

<span data-ttu-id="f3628-110">Ahora se permite el modificador `extern` en funciones locales.</span><span class="sxs-lookup"><span data-stu-id="f3628-110">The `extern` modifier is now permitted on local functions.</span></span> <span data-ttu-id="f3628-111">Esto hace que la función local sea externa en el mismo sentido que un [método externo](../spec/classes.md#external-methods).</span><span class="sxs-lookup"><span data-stu-id="f3628-111">This makes the local function external in the same sense as an [external method](../spec/classes.md#external-methods).</span></span>

<span data-ttu-id="f3628-112">De forma similar a un método externo, el cuerpo de la *función local* de una función local externa debe ser un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="f3628-112">Similarly to an external method, the *local-function-body* of an external local function must be a semicolon.</span></span> <span data-ttu-id="f3628-113">Solo se permite un *cuerpo de función local* de punto y coma en una función local externa.</span><span class="sxs-lookup"><span data-stu-id="f3628-113">A semicolon *local-function-body* is only permitted on an external local function.</span></span> 

<span data-ttu-id="f3628-114">También se debe `static`una función local externa.</span><span class="sxs-lookup"><span data-stu-id="f3628-114">An external local function must also be `static`.</span></span>

## <a name="syntax"></a><span data-ttu-id="f3628-115">Sintaxis</span><span class="sxs-lookup"><span data-stu-id="f3628-115">Syntax</span></span>

<span data-ttu-id="f3628-116">La [gramática de funciones locales](csharp-7.0/local-functions.md#syntax-grammar) se modifica de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="f3628-116">The [local functions grammar](csharp-7.0/local-functions.md#syntax-grammar) is modified as follows:</span></span>
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
