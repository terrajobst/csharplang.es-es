---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483863"
---
# <a name="expression-variables-in-initializers"></a><span data-ttu-id="7ebd2-101">Variables de expresión en inicializadores</span><span class="sxs-lookup"><span data-stu-id="7ebd2-101">Expression variables in initializers</span></span>

## <a name="summary"></a><span data-ttu-id="7ebd2-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="7ebd2-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="7ebd2-103">Se amplían las características introducidas en C# 7 para permitir expresiones que contengan variables de expresión (modelos de declaración y declaraciones de variables de salida) en inicializadores de campo, inicializadores de propiedad, constructores de inicializadores y cláusulas de consulta.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-103">We extend the features introduced in C# 7 to permit expressions containing expression variables (out variable declarations and declaration patterns) in field initializers, property initializers, ctor-initializers, and query clauses.</span></span>

## <a name="motivation"></a><span data-ttu-id="7ebd2-104">Motivación</span><span class="sxs-lookup"><span data-stu-id="7ebd2-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="7ebd2-105">Esto completa un par de bordes aproximados que quedan en el C# idioma debido a la falta de tiempo.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-105">This completes a couple of the rough edges left in the C# language due to lack of time.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="7ebd2-106">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="7ebd2-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="7ebd2-107">Quitamos la restricción que impide la declaración de variables de expresión (declaraciones de variables de salida y modelos de declaración) en un inicializador de constructor.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-107">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a ctor-initializer.</span></span> <span data-ttu-id="7ebd2-108">Este tipo de variable declarado está en el ámbito de todo el cuerpo del constructor.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-108">Such a declared variable is in scope throughout the body of the constructor.</span></span>

<span data-ttu-id="7ebd2-109">Quitamos la restricción que impide la declaración de variables de expresión (declaraciones de variables y modelos de declaración) en un inicializador de campo o propiedad.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-109">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a field or property initializer.</span></span> <span data-ttu-id="7ebd2-110">Este tipo de variable declarado está en el ámbito de la expresión de inicialización.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-110">Such a declared variable is in scope throughout the initializing expression.</span></span>

<span data-ttu-id="7ebd2-111">Quitamos la restricción que impide la declaración de variables de expresión (declaraciones de variables de salida y modelos de declaración) en una cláusula de expresión de consulta que se traduce en el cuerpo de una expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-111">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a query expression clause that is translated into the body of a lambda.</span></span> <span data-ttu-id="7ebd2-112">Este tipo de variable declarado está en el ámbito de esa expresión de la cláusula de consulta.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-112">Such a declared variable is in scope throughout that expression of the query clause.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="7ebd2-113">Desventajas</span><span class="sxs-lookup"><span data-stu-id="7ebd2-113">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="7ebd2-114">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-114">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="7ebd2-115">Alternativas</span><span class="sxs-lookup"><span data-stu-id="7ebd2-115">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="7ebd2-116">El ámbito adecuado para las variables de expresión declaradas en estos contextos no es obvio y merece más información sobre el LDM.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-116">The appropriate scope for expression variables declared in these contexts is not obvious, and deserves further LDM discussion.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="7ebd2-117">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="7ebd2-117">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="7ebd2-118">[] ¿Cuál es el ámbito adecuado para estas variables?</span><span class="sxs-lookup"><span data-stu-id="7ebd2-118">[ ] What is the appropriate scope for these variables?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="7ebd2-119">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="7ebd2-119">Design meetings</span></span>

<span data-ttu-id="7ebd2-120">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="7ebd2-120">None.</span></span>
