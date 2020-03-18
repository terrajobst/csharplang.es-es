---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2019
ms.locfileid: "79484061"
---
# <a name="lambda-discard-parameters"></a><span data-ttu-id="d3752-101">Parámetros de descarte de lambda</span><span class="sxs-lookup"><span data-stu-id="d3752-101">Lambda discard parameters</span></span>

## <a name="summary"></a><span data-ttu-id="d3752-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="d3752-102">Summary</span></span>

<span data-ttu-id="d3752-103">Permite el uso de Descartes (`_`) como parámetros de expresiones lambda y métodos anónimos.</span><span class="sxs-lookup"><span data-stu-id="d3752-103">Allow discards (`_`) to be used as parameters of lambdas and anonymous methods.</span></span>
<span data-ttu-id="d3752-104">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d3752-104">For example:</span></span>
- <span data-ttu-id="d3752-105">lambdas: `(_, _) => 0`, `(int _, int _) => 0`</span><span class="sxs-lookup"><span data-stu-id="d3752-105">lambdas: `(_, _) => 0`, `(int _, int _) => 0`</span></span>
- <span data-ttu-id="d3752-106">métodos anónimos: `delegate(int _, int _) { return 0; }`</span><span class="sxs-lookup"><span data-stu-id="d3752-106">anonymous methods: `delegate(int _, int _) { return 0; }`</span></span>

## <a name="motivation"></a><span data-ttu-id="d3752-107">Motivación</span><span class="sxs-lookup"><span data-stu-id="d3752-107">Motivation</span></span>

<span data-ttu-id="d3752-108">No es necesario que los parámetros no usados tengan nombre.</span><span class="sxs-lookup"><span data-stu-id="d3752-108">Unused parameters do not need to be named.</span></span> <span data-ttu-id="d3752-109">La intención de los descartes es clara, es decir, no se usan ni se descartan.</span><span class="sxs-lookup"><span data-stu-id="d3752-109">The intent of discards is clear, i.e. they are unused/discarded.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d3752-110">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="d3752-110">Detailed design</span></span>

<span data-ttu-id="d3752-111">[Parámetros de método](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) En la lista de parámetros de una expresión lambda o un método anónimo con más de un parámetro denominado `_`, estos parámetros son descartar parámetros.</span><span class="sxs-lookup"><span data-stu-id="d3752-111">[Method parameters](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) In the parameter list of a lambda or anonymous method with more than one parameter named `_`, such parameters are discard parameters.</span></span>
<span data-ttu-id="d3752-112">Nota: Si un solo parámetro se denomina `_`, es un parámetro normal por motivos de compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="d3752-112">Note: if a single parameter is named `_` then it is a regular parameter for backwards compatibility reasons.</span></span>

<span data-ttu-id="d3752-113">Los parámetros discard no introducen ningún nombre en los ámbitos.</span><span class="sxs-lookup"><span data-stu-id="d3752-113">Discard parameters do not introduce any names to any scopes.</span></span>
<span data-ttu-id="d3752-114">Tenga en cuenta que esto implica que no se ocultan los nombres de `_` (subrayado).</span><span class="sxs-lookup"><span data-stu-id="d3752-114">Note this implies they do not cause any `_` (underscore) names to be hidden.</span></span>

<span data-ttu-id="d3752-115">[Nombres simples](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) Si `K` es cero y el *simple_name* aparece dentro de un *bloque* y si el espacio de declaración de variable local del *bloque*[(o](basic-concepts.md#declarations)el de un *bloque*de inclusión) contiene una variable local, un parámetro (con la excepción de los parámetros de descarte) o una constante con el nombre `I`, el *simple_name* hace referencia a esa variable local, parámetro o constante y se clasifica como una variable o valor.</span><span class="sxs-lookup"><span data-stu-id="d3752-115">[Simple names](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter (with the exception of discard parameters) or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>

<span data-ttu-id="d3752-116">[Ámbitos](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) de A excepción de los parámetros de descarte, el ámbito de un parámetro declarado en una *lambda_expression* ([expresiones de función anónimas](expressions.md#anonymous-function-expressions)) es el *anonymous_function_body* de ese *lambda_expression* con la excepción de los parámetros de descarte, el ámbito de un parámetro declarado en una *anonymous_method_expression* ([expresiones de función anónimas](expressions.md#anonymous-function-expressions)) es el *bloque* de ese *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="d3752-116">[Scopes](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) With the exception of discard parameters, the scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression* With the exception of discard parameters, the scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>

## <a name="related-spec-sections"></a><span data-ttu-id="d3752-117">Secciones de especificaciones relacionadas</span><span class="sxs-lookup"><span data-stu-id="d3752-117">Related spec sections</span></span>
- [<span data-ttu-id="d3752-118">Parámetros correspondientes</span><span class="sxs-lookup"><span data-stu-id="d3752-118">Corresponding parameters</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
