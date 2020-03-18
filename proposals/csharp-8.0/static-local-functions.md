---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "79484013"
---
# <a name="static-local-functions"></a><span data-ttu-id="3d5e6-101">Funciones locales estáticas</span><span class="sxs-lookup"><span data-stu-id="3d5e6-101">Static local functions</span></span>

## <a name="summary"></a><span data-ttu-id="3d5e6-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="3d5e6-102">Summary</span></span>

<span data-ttu-id="3d5e6-103">Admitir funciones locales que no permiten capturar el estado del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-103">Support local functions that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="3d5e6-104">Motivación</span><span class="sxs-lookup"><span data-stu-id="3d5e6-104">Motivation</span></span>

<span data-ttu-id="3d5e6-105">Evite capturar involuntariamente el estado del contexto envolvente.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-105">Avoid unintentionally capturing state from the enclosing context.</span></span>
<span data-ttu-id="3d5e6-106">Permite usar funciones locales en escenarios en los que se requiere un método `static`.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-106">Allow local functions to be used in scenarios where a `static` method is required.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3d5e6-107">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="3d5e6-107">Detailed design</span></span>

<span data-ttu-id="3d5e6-108">Una función local declarada `static` no puede capturar el estado del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-108">A local function declared `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="3d5e6-109">Como resultado, las variables locales, los parámetros y `this` del ámbito de inclusión no están disponibles dentro de una función `static` local.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-109">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` local function.</span></span>

<span data-ttu-id="3d5e6-110">Una función local `static` no puede hacer referencia a miembros de instancia desde una `this` implícita o explícita o una referencia `base`.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-110">A `static` local function cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="3d5e6-111">Una función local `static` puede hacer referencia a `static` miembros del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-111">A `static` local function may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="3d5e6-112">Una función local `static` puede hacer referencia a definiciones de `constant` del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-112">A `static` local function may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="3d5e6-113">`nameof()` en una función local `static` puede hacer referencia a variables locales, parámetros o `this` o `base` del ámbito de inclusión.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-113">`nameof()` in a `static` local function may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="3d5e6-114">Las reglas de accesibilidad para `private` miembros del ámbito de inclusión son las mismas para las funciones locales `static` y`static`.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-114">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` local functions.</span></span>

<span data-ttu-id="3d5e6-115">Un `static` definición de función local se emite como un método de `static` en los metadatos, incluso si solo se usa en un delegado.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-115">A `static` local function definition is emitted as a `static` method in metadata, even if only used in a delegate.</span></span>

<span data-ttu-id="3d5e6-116">Una función local o lambda que no sea`static` puede capturar el estado de una función local `static`, pero no puede capturar el estado fuera de la función local envolvente `static`.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-116">A non-`static` local function or lambda can capture state from an enclosing `static` local function but cannot capture state outside the enclosing `static` local function.</span></span>

<span data-ttu-id="3d5e6-117">No se puede invocar una función local `static` en un árbol de expresión.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-117">A `static` local function cannot be invoked in an expression tree.</span></span>

<span data-ttu-id="3d5e6-118">Una llamada a una función local se emite como `call` en lugar de `callvirt`, independientemente de si la función local está `static`.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-118">A call to a local function is emitted as `call` rather than `callvirt`, regardless of whether the local function is `static`.</span></span>

<span data-ttu-id="3d5e6-119">Resolución de sobrecarga de una llamada en una función local que no se ve afectada por si la función local está `static`.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-119">Overload resolution of a call within a local function not affected by whether the local function is `static`.</span></span>

<span data-ttu-id="3d5e6-120">Al quitar el modificador `static` de una función local en un programa válido, no se cambia el significado del programa.</span><span class="sxs-lookup"><span data-stu-id="3d5e6-120">Removing the `static` modifier from a local function in a valid program does not change the meaning of the program.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="3d5e6-121">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="3d5e6-121">Design meetings</span></span>

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
