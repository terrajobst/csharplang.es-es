---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483563"
---
# <a name="lambda-attributes"></a><span data-ttu-id="100e4-101">Atributos lambda</span><span class="sxs-lookup"><span data-stu-id="100e4-101">Lambda Attributes</span></span>

* <span data-ttu-id="100e4-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="100e4-102">[x] Proposed</span></span>
* <span data-ttu-id="100e4-103">[] Prototipo</span><span class="sxs-lookup"><span data-stu-id="100e4-103">[ ] Prototype</span></span>
* <span data-ttu-id="100e4-104">[] Implementación</span><span class="sxs-lookup"><span data-stu-id="100e4-104">[ ] Implementation</span></span>
* <span data-ttu-id="100e4-105">[] Especificación</span><span class="sxs-lookup"><span data-stu-id="100e4-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="100e4-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="100e4-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="100e4-107">Permite que los atributos se apliquen a las expresiones lambda (y métodos anónimos) y a los parámetros de método lambda/anónimo, ya que pueden estar en métodos regulares.</span><span class="sxs-lookup"><span data-stu-id="100e4-107">Allow attributes to be applied to lambdas (and anonymous methods) and to lambda / anonymous method parameters, as they can be on regular methods.</span></span>

## <a name="motivation"></a><span data-ttu-id="100e4-108">Motivación</span><span class="sxs-lookup"><span data-stu-id="100e4-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="100e4-109">Dos motivaciones principales:</span><span class="sxs-lookup"><span data-stu-id="100e4-109">Two primary motivations:</span></span>

1. <span data-ttu-id="100e4-110">Para proporcionar metadatos visibles para los analizadores en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="100e4-110">To provide metadata visible to analyzers at compile-time.</span></span>
2. <span data-ttu-id="100e4-111">Para proporcionar metadatos visibles para la reflexión y las herramientas en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="100e4-111">To provide metadata visible to reflection and tooling at run-time.</span></span>

<span data-ttu-id="100e4-112">Como ejemplo de (1): en el caso de código que tiene en cuenta el rendimiento, resulta útil poder tener un analizador que se marque cuando se asignen cierres y delegados para las expresiones lambda que se cierran con el estado.</span><span class="sxs-lookup"><span data-stu-id="100e4-112">As an example of (1): For performance-sensitive code, it is helpful to be able to have an analyzer that flags when closures and delegates are being allocated for lambdas that close over state.</span></span>  <span data-ttu-id="100e4-113">A menudo, un desarrollador de este código sale de su forma de evitar la captura de cualquier Estado, de modo que el compilador puede generar un método estático y un delegado que se puede almacenar en caché para el método, o bien el desarrollador se asegurará de que el único Estado que se cierra es `this`, lo que permite al compilador al menos asignar un objeto de cierre.</span><span class="sxs-lookup"><span data-stu-id="100e4-113">Often a developer of such code will go out of his or her way to avoid capturing any state, so that the compiler can generate a static method and a cacheable delegate for the method, or the developer will ensure that the only state being closed over is `this`, allowing the compiler at least to avoid allocating a closure object.</span></span>  <span data-ttu-id="100e4-114">Pero sin la compatibilidad de idioma para limitar lo que se puede capturar, es demasiado fácil cerrar accidentalmente el estado.</span><span class="sxs-lookup"><span data-stu-id="100e4-114">But, without language support for limiting what may be captured, it is all too easy to accidentally close over state.</span></span>  <span data-ttu-id="100e4-115">Sería útil si un desarrollador pudiera anotar lambdas con atributos para indicar el estado con el que se pueden cerrar, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="100e4-115">It would be valuable if a developer could annotate lambdas with attributes to indicate what state they're allowed to close over, for example:</span></span>

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

<span data-ttu-id="100e4-116">Después, se puede escribir un analizador para que se marque cuando el estado se Capture incorrectamente, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="100e4-116">Then an analyzer can be written to flag when state is captured incorrectly, for example:</span></span>

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a><span data-ttu-id="100e4-117">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="100e4-117">Detailed design</span></span>
[design]: #detailed-design

- <span data-ttu-id="100e4-118">Con la misma sintaxis de atributo que en los métodos normales, los atributos se pueden aplicar al principio de una expresión lambda o un método anónimo, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="100e4-118">Using the same attribute syntax as on normal methods, attributes may be applied at the beginning of a lambda or anonymous method, for example:</span></span>

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- <span data-ttu-id="100e4-119">Para evitar la ambigüedad en lo que se refiere a si un atributo se aplica al método lambda o a uno de los argumentos, los atributos solo se pueden usar cuando se usan paréntesis en torno a cualquier argumento, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="100e4-119">To avoid ambiguity as to whether an attribute applies to the lambda method or to one of the arguments, attributes may only be used when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- <span data-ttu-id="100e4-120">Con métodos anónimos, no se necesitan paréntesis para aplicar un atributo al método antes de la palabra clave `delegate`, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="100e4-120">With anonymous methods, parens are not needed in order to apply an attribute to the method before the `delegate` keyword, for example:</span></span>

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- <span data-ttu-id="100e4-121">Se pueden aplicar varios atributos, ya sea a través de la sintaxis delimitada por comas estándar o a través de la sintaxis de atributo completo, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="100e4-121">Multiple attributes may be applied, either via standard comma-delimited syntax or via full-attribute syntax, for example:</span></span>

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- <span data-ttu-id="100e4-122">Los atributos se pueden aplicar a los parámetros a un método anónimo o una expresión lambda, pero solo cuando se usan paréntesis alrededor de cualquier argumento, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="100e4-122">Attributes may be applied to the parameters to an anonymous method or lambda, but only when parens are used around any arguments, for example:</span></span>

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- <span data-ttu-id="100e4-123">Se pueden aplicar varios atributos a los parámetros de un método anónimo o una expresión lambda, mediante la sintaxis de atributos completos o delimitados por comas, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="100e4-123">Multiple attributes may be applied to the parameters of an anonymous method or lambda, using either the comma-delimited or full-attribute syntax, for example:</span></span>

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- <span data-ttu-id="100e4-124">los atributos de destino de `return`también se pueden utilizar en lambdas, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="100e4-124">`return`-targeted attributes may also be used on lambdas, for example:</span></span>

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- <span data-ttu-id="100e4-125">El compilador genera los atributos en el método generado y los argumentos para esos métodos, tal y como lo haría para cualquier otro método.</span><span class="sxs-lookup"><span data-stu-id="100e4-125">The compiler outputs the attributes onto the generated method and arguments to those methods as it would for any other method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="100e4-126">Desventajas</span><span class="sxs-lookup"><span data-stu-id="100e4-126">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="100e4-127">N/D</span><span class="sxs-lookup"><span data-stu-id="100e4-127">n/a</span></span>

## <a name="alternatives"></a><span data-ttu-id="100e4-128">Alternativas</span><span class="sxs-lookup"><span data-stu-id="100e4-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="100e4-129">N/D</span><span class="sxs-lookup"><span data-stu-id="100e4-129">n/a</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="100e4-130">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="100e4-130">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="100e4-131">N/D</span><span class="sxs-lookup"><span data-stu-id="100e4-131">n/a</span></span>

## <a name="design-meetings"></a><span data-ttu-id="100e4-132">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="100e4-132">Design meetings</span></span>

<span data-ttu-id="100e4-133">N/D</span><span class="sxs-lookup"><span data-stu-id="100e4-133">n/a</span></span>