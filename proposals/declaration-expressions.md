---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483569"
---
# <a name="declaration-expressions"></a><span data-ttu-id="805b7-101">Expresiones de declaración</span><span class="sxs-lookup"><span data-stu-id="805b7-101">Declaration expressions</span></span>

<span data-ttu-id="805b7-102">Admitir las asignaciones de declaraciones como expresiones.</span><span class="sxs-lookup"><span data-stu-id="805b7-102">Support declaration assignments as expressions.</span></span>

## <a name="motivation"></a><span data-ttu-id="805b7-103">Motivación</span><span class="sxs-lookup"><span data-stu-id="805b7-103">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="805b7-104">Permita la inicialización en el punto de declaración en más casos, simplifique el código y permita el uso de `var`.</span><span class="sxs-lookup"><span data-stu-id="805b7-104">Allow initialization at the point of declaration in more cases, simplifying code, and allowing `var` to be used.</span></span>

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

<span data-ttu-id="805b7-105">Permita las declaraciones de `ref` argumentos, similar a `out var`.</span><span class="sxs-lookup"><span data-stu-id="805b7-105">Allow declarations for `ref` arguments, similar to `out var`.</span></span>

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a><span data-ttu-id="805b7-106">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="805b7-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="805b7-107">Las expresiones se extienden para incluir la asignación de declaración.</span><span class="sxs-lookup"><span data-stu-id="805b7-107">Expressions are extended to include declaration assignment.</span></span> <span data-ttu-id="805b7-108">La prioridad es la misma que la asignación.</span><span class="sxs-lookup"><span data-stu-id="805b7-108">Precedence is the same as assignment.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

<span data-ttu-id="805b7-109">La asignación de declaración es de un solo local.</span><span class="sxs-lookup"><span data-stu-id="805b7-109">The declaration assignment is of a single local.</span></span>

<span data-ttu-id="805b7-110">El tipo de una expresión de asignación de declaración es el tipo de la declaración.</span><span class="sxs-lookup"><span data-stu-id="805b7-110">The type of a declaration assignment expression is the type of the declaration.</span></span>
<span data-ttu-id="805b7-111">Si el tipo es `var`, el tipo deducido es el tipo de la expresión de inicialización.</span><span class="sxs-lookup"><span data-stu-id="805b7-111">If the type is `var`, the inferred type is the type of the initializing expression.</span></span> 

<span data-ttu-id="805b7-112">La expresión de asignación de declaración puede ser un valor l, para `ref` valores de argumento en particular.</span><span class="sxs-lookup"><span data-stu-id="805b7-112">The declaration assignment expression may be an l-value, for `ref` argument values in particular.</span></span>

<span data-ttu-id="805b7-113">Si la expresión de asignación de declaración declara un tipo de valor y la expresión es un valor r, el valor de la expresión es una copia.</span><span class="sxs-lookup"><span data-stu-id="805b7-113">If the declaration assignment expression declares a value type, and the expression is an r-value, the value of the expression is a copy.</span></span>

<span data-ttu-id="805b7-114">La expresión de asignación de declaración puede declarar un `ref` local.</span><span class="sxs-lookup"><span data-stu-id="805b7-114">The declaration assignment expression may declare a `ref` local.</span></span>
<span data-ttu-id="805b7-115">Existe una ambigüedad cuando se usa `ref` para una expresión de declaración en un argumento de `ref`.</span><span class="sxs-lookup"><span data-stu-id="805b7-115">There is an ambiguity when `ref` is used for a declaration expression in a `ref` argument.</span></span>
<span data-ttu-id="805b7-116">El inicializador de variable local determina si la declaración es una `ref` local.</span><span class="sxs-lookup"><span data-stu-id="805b7-116">The local variable initializer determines whether the declaration is a `ref` local.</span></span>

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

<span data-ttu-id="805b7-117">El ámbito de las variables locales declaradas en expresiones de asignación de declaración es el mismo que el ámbito de las expresiones de declaración correspondientes de C # 7.0.</span><span class="sxs-lookup"><span data-stu-id="805b7-117">The scope of locals declared in declaration assignment expressions is the same the scope of corresponding declaration expressions from C#7.0.</span></span>

<span data-ttu-id="805b7-118">Es un error en tiempo de compilación hacer referencia a una variable local en el texto que precede a la expresión de declaración.</span><span class="sxs-lookup"><span data-stu-id="805b7-118">It is a compile time error to refer to a local in text preceding the declaration expression.</span></span>

## <a name="alternatives"></a><span data-ttu-id="805b7-119">Alternativas</span><span class="sxs-lookup"><span data-stu-id="805b7-119">Alternatives</span></span>
[alternatives]: #alternatives
<span data-ttu-id="805b7-120">Sin cambios.</span><span class="sxs-lookup"><span data-stu-id="805b7-120">No change.</span></span> <span data-ttu-id="805b7-121">Esta característica es simplemente sintácticamente breve después de todo.</span><span class="sxs-lookup"><span data-stu-id="805b7-121">This feature is just syntactic shorthand after all.</span></span>

<span data-ttu-id="805b7-122">Expresiones de secuencias más generales: vea [#377](https://github.com/dotnet/csharplang/issues/377).</span><span class="sxs-lookup"><span data-stu-id="805b7-122">More general sequence expressions: see [#377](https://github.com/dotnet/csharplang/issues/377).</span></span>

<span data-ttu-id="805b7-123">Para permitir el uso de `var` en más casos, permita una declaración y asignación independientes de `var` variables locales y deduzca el tipo de las asignaciones de todas las rutas de acceso de código.</span><span class="sxs-lookup"><span data-stu-id="805b7-123">To allow use of `var` in more cases, allow separate declaration and assignment of `var` locals, and infer the type from assignments from all code paths.</span></span>

## <a name="see-also"></a><span data-ttu-id="805b7-124">Consulte también</span><span class="sxs-lookup"><span data-stu-id="805b7-124">See also</span></span>
[see-also]: #see-also
<span data-ttu-id="805b7-125">Vea expresión de declaración básica en [#595](https://github.com/dotnet/csharplang/issues/595).</span><span class="sxs-lookup"><span data-stu-id="805b7-125">See Basic Declaration Expression in [#595](https://github.com/dotnet/csharplang/issues/595).</span></span>

<span data-ttu-id="805b7-126">Vea declaración de desconstrucción en la característica de [desconstrucción](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) .</span><span class="sxs-lookup"><span data-stu-id="805b7-126">See Deconstruction Declaration in the [deconstruction](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) feature.</span></span>
