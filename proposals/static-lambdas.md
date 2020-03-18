---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "79484001"
---
# <a name="static-lambdas"></a><span data-ttu-id="6c8e1-101">Expresiones lambda estáticas</span><span class="sxs-lookup"><span data-stu-id="6c8e1-101">Static lambdas</span></span>

## <a name="summary"></a><span data-ttu-id="6c8e1-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="6c8e1-102">Summary</span></span>

<span data-ttu-id="6c8e1-103">Admite lambdas que no permiten capturar el estado del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-103">Support lambdas that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="6c8e1-104">Motivación</span><span class="sxs-lookup"><span data-stu-id="6c8e1-104">Motivation</span></span>

<span data-ttu-id="6c8e1-105">Evite capturar involuntariamente el estado del contexto envolvente.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-105">Avoid unintentionally capturing state from the enclosing context.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="6c8e1-106">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="6c8e1-106">Detailed design</span></span>

<span data-ttu-id="6c8e1-107">Una expresión lambda con `static` no puede capturar el estado del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-107">A lambdas with `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="6c8e1-108">Como resultado, las variables locales, los parámetros y `this` del ámbito de inclusión no están disponibles dentro de una expresión lambda `static`.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-108">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` lambda.</span></span>

<span data-ttu-id="6c8e1-109">Una expresión lambda de `static` no puede hacer referencia a miembros de instancia desde una `this` implícita o explícita o una referencia `base`.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-109">A `static` lambda cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="6c8e1-110">Una expresión lambda `static` puede hacer referencia a `static` miembros del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-110">A `static` lambda may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="6c8e1-111">Una expresión lambda `static` puede hacer referencia a definiciones de `constant` del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-111">A `static` lambda may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="6c8e1-112">`nameof()` en una expresión lambda `static` puede hacer referencia a variables locales, parámetros o `this` o `base` del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-112">`nameof()` in a `static` lambda may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="6c8e1-113">Las reglas de accesibilidad para `private` miembros del ámbito de inclusión son las mismas para las expresiones lambda `static` y`static`.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-113">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` lambdas.</span></span>

<span data-ttu-id="6c8e1-114">No se garantiza que se emita una `static` definición lambda como método de `static` en los metadatos.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-114">No guarantee is made as to whether a `static` lambda definition is emitted as a `static` method in metadata.</span></span> <span data-ttu-id="6c8e1-115">Esto se deja hasta la implementación del compilador para optimizar.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-115">This is left up to the compiler implementation to optimize.</span></span>

<span data-ttu-id="6c8e1-116">Una función local o lambda que no sea`static` puede capturar el estado de una expresión lambda `static`, pero no puede capturar el estado fuera de la expresión lambda `static` envolvente.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-116">A non-`static` local function or lambda can capture state from an enclosing `static` lambda but cannot capture state outside the enclosing `static` lambda.</span></span>

<span data-ttu-id="6c8e1-117">Una expresión lambda de `static` se puede usar en un árbol de expresión.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-117">A `static` lambda can be used in an expression tree.</span></span>

<span data-ttu-id="6c8e1-118">Al quitar el modificador `static` de una expresión lambda en un programa válido, no se cambia el significado del programa.</span><span class="sxs-lookup"><span data-stu-id="6c8e1-118">Removing the `static` modifier from a lambda in a valid program does not change the meaning of the program.</span></span>
