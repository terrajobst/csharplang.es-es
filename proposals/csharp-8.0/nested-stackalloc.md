---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2019
ms.locfileid: "79484049"
---
# <a name="permit-stackalloc-in-nested-contexts"></a><span data-ttu-id="3a710-101">Permitir `stackalloc` en contextos anidados</span><span class="sxs-lookup"><span data-stu-id="3a710-101">Permit `stackalloc` in nested contexts</span></span>

### <a name="stack-allocation"></a><span data-ttu-id="3a710-102">Asignación de la pila</span><span class="sxs-lookup"><span data-stu-id="3a710-102">Stack allocation</span></span>

<span data-ttu-id="3a710-103">Se modifica la sección [*asignación*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) de la pila C# de la especificación del lenguaje para relajar los lugares en los que puede aparecer una expresión `stackalloc`.</span><span class="sxs-lookup"><span data-stu-id="3a710-103">We modify the section [*Stack allocation*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) of the C# language specification to relax the places when a `stackalloc` expression may appear.</span></span> <span data-ttu-id="3a710-104">Eliminamos</span><span class="sxs-lookup"><span data-stu-id="3a710-104">We delete</span></span>

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

<span data-ttu-id="3a710-105">y reemplácelas por</span><span class="sxs-lookup"><span data-stu-id="3a710-105">and replace them with</span></span>

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

<span data-ttu-id="3a710-106">Tenga en cuenta que la adición de una *array_initializer* a *stackalloc_initializer* (y que la expresión de índice es opcional) era una [extensión en C# 7,3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) y no se describe aquí.</span><span class="sxs-lookup"><span data-stu-id="3a710-106">Note that the addition of an *array_initializer* to *stackalloc_initializer* (and making the index expression optional) was an [extension in C# 7.3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) and is not described here.</span></span>

<span data-ttu-id="3a710-107">El *tipo de elemento* de la expresión `stackalloc` es el *unmanaged_type* denominado en la expresión stackalloc, si existe, o el tipo común entre los elementos de la *array_initializer* en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="3a710-107">The *element type* of the `stackalloc` expression is the *unmanaged_type* named in the stackalloc expression, if any, or the common type among the elements of the *array_initializer* otherwise.</span></span>

<span data-ttu-id="3a710-108">El tipo de *stackalloc_initializer* con el *tipo de elemento* `K` depende de su contexto sintáctico:</span><span class="sxs-lookup"><span data-stu-id="3a710-108">The type of the *stackalloc_initializer* with *element type* `K` depends on its syntactic context:</span></span>
- <span data-ttu-id="3a710-109">Si el *stackalloc_initializer* aparece directamente como *local_variable_initializer* de una instrucción de *local_variable_declaration* o un *for_initializer*, su tipo es `K*`.</span><span class="sxs-lookup"><span data-stu-id="3a710-109">If the *stackalloc_initializer* appears directly as the *local_variable_initializer* of a *local_variable_declaration* statement or a *for_initializer*, then its type is `K*`.</span></span>
- <span data-ttu-id="3a710-110">De lo contrario, su tipo es `System.Span<K>`.</span><span class="sxs-lookup"><span data-stu-id="3a710-110">Otherwise its type is `System.Span<K>`.</span></span>

### <a name="stackalloc-conversion"></a><span data-ttu-id="3a710-111">Conversión stackalloc</span><span class="sxs-lookup"><span data-stu-id="3a710-111">Stackalloc Conversion</span></span>

<span data-ttu-id="3a710-112">La *conversión stackalloc* es una nueva conversión implícita integrada de la expresión.</span><span class="sxs-lookup"><span data-stu-id="3a710-112">The *stackalloc conversion* is a new built-in implicit conversion from expression.</span></span> <span data-ttu-id="3a710-113">Cuando se `K*`el tipo de una *stackalloc_initializer* , hay una *conversión stackalloc* implícita desde el *stackalloc_initializer* al `System.Span<K>`de tipo.</span><span class="sxs-lookup"><span data-stu-id="3a710-113">When the type of a *stackalloc_initializer* is `K*`, there is an implicit *stackalloc conversion* from the *stackalloc_initializer* to the type `System.Span<K>`.</span></span>
