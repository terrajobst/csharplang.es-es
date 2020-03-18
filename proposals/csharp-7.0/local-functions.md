---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483449"
---
# <a name="local-functions"></a><span data-ttu-id="6d27a-101">Funciones locales</span><span class="sxs-lookup"><span data-stu-id="6d27a-101">Local functions</span></span>

<span data-ttu-id="6d27a-102">Ampliamos C# para admitir la declaración de funciones en el ámbito de bloque.</span><span class="sxs-lookup"><span data-stu-id="6d27a-102">We extend C# to support the declaration of functions in block scope.</span></span> <span data-ttu-id="6d27a-103">Las funciones locales pueden utilizar (Capture) variables del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="6d27a-103">Local functions may use (capture) variables from the enclosing scope.</span></span>

<span data-ttu-id="6d27a-104">El compilador usa el análisis de flujo para detectar las variables que usa una función local antes de asignarle un valor.</span><span class="sxs-lookup"><span data-stu-id="6d27a-104">The compiler uses flow analysis to detect which variables a local function uses before assigning it a value.</span></span> <span data-ttu-id="6d27a-105">Cada llamada de la función requiere que estas variables se asignen de forma definitiva.</span><span class="sxs-lookup"><span data-stu-id="6d27a-105">Every call of the function requires such variables to be definitely assigned.</span></span> <span data-ttu-id="6d27a-106">Del mismo modo, el compilador determina qué variables se asignan definitivamente en la devolución.</span><span class="sxs-lookup"><span data-stu-id="6d27a-106">Similarly the compiler determines which variables are definitely assigned on return.</span></span> <span data-ttu-id="6d27a-107">Estas variables se consideran definitivamente asignadas después de invocar la función local.</span><span class="sxs-lookup"><span data-stu-id="6d27a-107">Such variables are considered definitely assigned after the local function is invoked.</span></span>

<span data-ttu-id="6d27a-108">Se puede llamar a las funciones locales desde un punto léxico antes de su definición.</span><span class="sxs-lookup"><span data-stu-id="6d27a-108">Local functions may be called from a lexical point before its definition.</span></span> <span data-ttu-id="6d27a-109">Las instrucciones de declaración de funciones locales no provocan una advertencia cuando no se puede tener acceso a ellas.</span><span class="sxs-lookup"><span data-stu-id="6d27a-109">Local function declaration statements do not cause a warning when they are not reachable.</span></span>

<span data-ttu-id="6d27a-110">TODO: _escribir especificación_</span><span class="sxs-lookup"><span data-stu-id="6d27a-110">TODO: _WRITE SPEC_</span></span>

## <a name="syntax-grammar"></a><span data-ttu-id="6d27a-111">Gramática de sintaxis</span><span class="sxs-lookup"><span data-stu-id="6d27a-111">Syntax grammar</span></span>

<span data-ttu-id="6d27a-112">Esta gramática se representa como una diferencia de la gramática de especificación actual.</span><span class="sxs-lookup"><span data-stu-id="6d27a-112">This grammar is represented as a diff from the current spec grammar.</span></span>

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

<span data-ttu-id="6d27a-113">Las funciones locales pueden usar variables definidas en el ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="6d27a-113">Local functions may use variables defined in the enclosing scope.</span></span> <span data-ttu-id="6d27a-114">La implementación actual requiere que todas las variables leídas dentro de una función local se asignen definitivamente, como si se ejecutara la función local en su punto de definición.</span><span class="sxs-lookup"><span data-stu-id="6d27a-114">The current implementation requires that every variable read inside a local function be definitely assigned, as if executing the local function at its point of definition.</span></span> <span data-ttu-id="6d27a-115">Además, la definición de función local se debe haber "ejecutado" en cualquier punto de uso.</span><span class="sxs-lookup"><span data-stu-id="6d27a-115">Also, the local function definition must have been "executed" at any use point.</span></span>

<span data-ttu-id="6d27a-116">Después de experimentar con ese bit (por ejemplo, no es posible definir dos funciones locales recursivas mutuamente), hemos revisado cómo deseamos que funcione la asignación definitiva.</span><span class="sxs-lookup"><span data-stu-id="6d27a-116">After experimenting with that a bit (for example, it is not possible to define two mutually recursive local functions), we've since revised how we want the definite assignment to work.</span></span> <span data-ttu-id="6d27a-117">La revisión (aún no implementada) es que todas las variables locales leídas en una función local deben asignarse definitivamente en cada invocación de la función local.</span><span class="sxs-lookup"><span data-stu-id="6d27a-117">The revision (not yet implemented) is that all local variables read in a local function must be definitely assigned at each invocation of the local function.</span></span> <span data-ttu-id="6d27a-118">Eso es realmente más sutil que el sonido y hay una gran cantidad de trabajo que queda para que funcione.</span><span class="sxs-lookup"><span data-stu-id="6d27a-118">That's actually more subtle than it sounds, and there is a bunch of work remaining to make it work.</span></span> <span data-ttu-id="6d27a-119">Una vez que lo haya hecho, podrá trasladar las funciones locales al final de su bloque de inclusión.</span><span class="sxs-lookup"><span data-stu-id="6d27a-119">Once it is done you'll be able to move your local functions to the end of its enclosing block.</span></span>

<span data-ttu-id="6d27a-120">Las nuevas reglas de asignación definitiva son incompatibles con la deducción del tipo de valor devuelto de una función local, por lo que es probable que se quite la compatibilidad para deducir el tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="6d27a-120">The new definite assignment rules are incompatible with inferring the return type of a local function, so we'll likely be removing support for inferring the return type.</span></span>

<span data-ttu-id="6d27a-121">A menos que se convierta una función local en un delegado, la captura se realiza en marcos que son tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="6d27a-121">Unless you convert a local function to a delegate, capturing is done into frames that are value types.</span></span> <span data-ttu-id="6d27a-122">Esto significa que no obtiene ninguna presión de GC del uso de funciones locales con captura.</span><span class="sxs-lookup"><span data-stu-id="6d27a-122">That means you don't get any GC pressure from using local functions with capturing.</span></span>

### <a name="reachability"></a><span data-ttu-id="6d27a-123">Accesibilidad</span><span class="sxs-lookup"><span data-stu-id="6d27a-123">Reachability</span></span>

<span data-ttu-id="6d27a-124">Agregamos a la especificación</span><span class="sxs-lookup"><span data-stu-id="6d27a-124">We add to the spec</span></span>

> <span data-ttu-id="6d27a-125">El cuerpo de una expresión lambda con cuerpo de instrucción o función local se considera alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6d27a-125">The body of a statement-bodied lambda expression or local function is considered reachable.</span></span>
