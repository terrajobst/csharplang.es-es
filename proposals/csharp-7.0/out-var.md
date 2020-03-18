---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483509"
---
# <a name="out-variable-declarations"></a><span data-ttu-id="f350e-101">Declaraciones de variable out</span><span class="sxs-lookup"><span data-stu-id="f350e-101">Out variable declarations</span></span>

<span data-ttu-id="f350e-102">La característica de *declaración de variable out* permite declarar una variable en la ubicación en la que se pasa como un argumento `out`.</span><span class="sxs-lookup"><span data-stu-id="f350e-102">The *out variable declaration* feature enables a variable to be declared at the location that it is being passed as an `out` argument.</span></span>

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

<span data-ttu-id="f350e-103">Una variable declarada de esta manera se denomina una *variable out*.</span><span class="sxs-lookup"><span data-stu-id="f350e-103">A variable declared this way is called an *out variable*.</span></span> <span data-ttu-id="f350e-104">Puede usar la palabra clave contextual `var` para el tipo de la variable.</span><span class="sxs-lookup"><span data-stu-id="f350e-104">You may use the contextual keyword `var` for the variable's type.</span></span> <span data-ttu-id="f350e-105">El ámbito será el mismo que para una *variable de patrón* introducida a través de la coincidencia de patrones.</span><span class="sxs-lookup"><span data-stu-id="f350e-105">The scope will be the same as for a *pattern-variable* introduced via pattern-matching.</span></span>

<span data-ttu-id="f350e-106">Según la especificación del lenguaje (sección acceso al elemento 7.6.7), la lista de argumentos de un acceso a elementos (expresión de indización) no contiene argumentos Ref o out.</span><span class="sxs-lookup"><span data-stu-id="f350e-106">According to Language Specification (section 7.6.7 Element access) the argument-list of an element-access (indexing expression) does not contain ref or out arguments.</span></span> <span data-ttu-id="f350e-107">Sin embargo, las permite el compilador para varios escenarios, por ejemplo, los indizadores declarados en metadatos que aceptan `out`.</span><span class="sxs-lookup"><span data-stu-id="f350e-107">However, they are permitted by the compiler for various scenarios, for example indexers declared in metadata that accept `out`.</span></span>

<span data-ttu-id="f350e-108">Dentro del ámbito de una variable local introducida por un argument_value, es un error en tiempo de compilación hacer referencia a esa variable local en una posición textual que precede a su declaración.</span><span class="sxs-lookup"><span data-stu-id="f350e-108">Within the scope of a local variable introduced by an argument_value, it is a compile-time error to refer to that local variable in a textual position that precedes its declaration.</span></span>

<span data-ttu-id="f350e-109">También es un error hacer referencia a una variable out de tipo implícito (§ 8.5.1) en la misma lista de argumentos que contiene inmediatamente su declaración.</span><span class="sxs-lookup"><span data-stu-id="f350e-109">It is also an error to reference an implicitly-typed (§8.5.1) out variable in the same argument list that immediately contains its declaration.</span></span>

<span data-ttu-id="f350e-110">La resolución de sobrecarga se modifica de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="f350e-110">Overload resolution is modified as follows:</span></span>

<span data-ttu-id="f350e-111">Agregamos una nueva conversión:</span><span class="sxs-lookup"><span data-stu-id="f350e-111">We add a new conversion:</span></span>

> <span data-ttu-id="f350e-112">Existe una *conversión de la expresión* de una declaración de variable out implícitamente a cada tipo.</span><span class="sxs-lookup"><span data-stu-id="f350e-112">There is a *conversion from expression* from an implicitly-typed out variable declaration to every type.</span></span>

<span data-ttu-id="f350e-113">Asimismo</span><span class="sxs-lookup"><span data-stu-id="f350e-113">Also</span></span>

> <span data-ttu-id="f350e-114">El tipo de un argumento de variable out explícitamente escrito es el tipo declarado.</span><span class="sxs-lookup"><span data-stu-id="f350e-114">The type of an explicitly-typed out variable argument is the declared type.</span></span>

<span data-ttu-id="f350e-115">y</span><span class="sxs-lookup"><span data-stu-id="f350e-115">and</span></span>

> <span data-ttu-id="f350e-116">Un argumento de variable out con tipo implícito no tiene ningún tipo.</span><span class="sxs-lookup"><span data-stu-id="f350e-116">An implicitly-typed out variable argument has no type.</span></span>

<span data-ttu-id="f350e-117">La *conversión de la expresión* de una declaración de variable out implícitamente no se considera mejor que cualquier otra *conversión de la expresión*.</span><span class="sxs-lookup"><span data-stu-id="f350e-117">The *conversion from expression* from an implicitly-typed out variable declaration is not considered better than any other *conversion from expression*.</span></span>

<span data-ttu-id="f350e-118">El tipo de una variable de salida con tipo implícito es el tipo del parámetro correspondiente en la firma del método seleccionado por la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="f350e-118">The type of an implicitly-typed out variable is the type of the corresponding parameter in the signature of the method selected by overload resolution.</span></span>

<span data-ttu-id="f350e-119">Se agrega el nuevo nodo de sintaxis `DeclarationExpressionSyntax` para representar la declaración en un argumento de salida var.</span><span class="sxs-lookup"><span data-stu-id="f350e-119">The new syntax node `DeclarationExpressionSyntax` is added to represent the declaration in an out var argument.</span></span>
