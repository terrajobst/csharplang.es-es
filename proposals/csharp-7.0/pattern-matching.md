---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484073"
---
# <a name="pattern-matching-for-c-7"></a><span data-ttu-id="e9d33-101">Coincidencia de patrones C# para 7</span><span class="sxs-lookup"><span data-stu-id="e9d33-101">Pattern Matching for C# 7</span></span>

<span data-ttu-id="e9d33-102">Las extensiones de coincidencia C# de patrones para permiten muchas de las ventajas de los tipos de datos algebraicos y la coincidencia de patrones de los lenguajes funcionales, pero de una manera que se integra sin problemas con la sensación del lenguaje subyacente.</span><span class="sxs-lookup"><span data-stu-id="e9d33-102">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="e9d33-103">Las características básicas son: [tipos de registros](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), que son tipos cuyo significado semántico se describe mediante la forma de los datos. y la coincidencia de patrones, que es un nuevo formulario de expresión que permite la descomposición de varios niveles de estos tipos de datos.</span><span class="sxs-lookup"><span data-stu-id="e9d33-103">The basic features are: [record types](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), which are types whose semantic meaning is described by the shape of the data; and pattern matching, which is a new expression form that enables extremely concise multilevel decomposition of these data types.</span></span> <span data-ttu-id="e9d33-104">Los elementos de este enfoque están inspirados en las características relacionadas en [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Coincidencia de patrones extensible a través de un lenguaje ligero") los lenguajes de programación y [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Coincidencia de objetos con patrones").</span><span class="sxs-lookup"><span data-stu-id="e9d33-104">Elements of this approach are inspired by related features in the programming languages [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Matching Objects With Patterns").</span></span>

## <a name="is-expression"></a><span data-ttu-id="e9d33-105">Expresión is</span><span class="sxs-lookup"><span data-stu-id="e9d33-105">Is expression</span></span>

<span data-ttu-id="e9d33-106">El operador `is` se extiende para probar una expresión con un *patrón*.</span><span class="sxs-lookup"><span data-stu-id="e9d33-106">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="e9d33-107">Esta forma de *relational_expression* se suma a los formularios existentes en la C# especificación.</span><span class="sxs-lookup"><span data-stu-id="e9d33-107">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="e9d33-108">Es un error en tiempo de compilación si el *relational_expression* a la izquierda del token de `is` no designa un valor o no tiene un tipo.</span><span class="sxs-lookup"><span data-stu-id="e9d33-108">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="e9d33-109">Cada *identificador* del patrón introduce una nueva variable local que se *asigna definitivamente* después de que el operador de `is` sea `true` (es decir, se *asigna definitivamente cuando es true*).</span><span class="sxs-lookup"><span data-stu-id="e9d33-109">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="e9d33-110">Nota: técnicamente existe una ambigüedad entre el *tipo* en un `is-expression` y *constant_pattern*, cualquiera de los cuales puede ser un análisis válido de un identificador calificado.</span><span class="sxs-lookup"><span data-stu-id="e9d33-110">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="e9d33-111">Intentamos enlazarlo como un tipo para la compatibilidad con versiones anteriores del lenguaje; solo si se produce un error, se resuelve como hacemos en otros contextos, lo primero que se encontró (que debe ser una constante o un tipo).</span><span class="sxs-lookup"><span data-stu-id="e9d33-111">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="e9d33-112">Esta ambigüedad solo está presente en la parte derecha de una expresión de `is`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-112">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

## <a name="patterns"></a><span data-ttu-id="e9d33-113">Patrones</span><span class="sxs-lookup"><span data-stu-id="e9d33-113">Patterns</span></span>

<span data-ttu-id="e9d33-114">Los patrones se utilizan en el operador `is` y en un *switch_statement* para expresar la forma de los datos con los que se van a comparar los datos entrantes.</span><span class="sxs-lookup"><span data-stu-id="e9d33-114">Patterns are used in the `is` operator and in a *switch_statement* to express the shape of data against which incoming data is to be compared.</span></span> <span data-ttu-id="e9d33-115">Los patrones pueden ser recursivos para que se puedan comparar partes de los datos con subpatrones.</span><span class="sxs-lookup"><span data-stu-id="e9d33-115">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> <span data-ttu-id="e9d33-116">Nota: técnicamente existe una ambigüedad entre el *tipo* en un `is-expression` y *constant_pattern*, cualquiera de los cuales puede ser un análisis válido de un identificador calificado.</span><span class="sxs-lookup"><span data-stu-id="e9d33-116">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="e9d33-117">Intentamos enlazarlo como un tipo para la compatibilidad con versiones anteriores del lenguaje; solo si se produce un error, se resuelve como hacemos en otros contextos, lo primero que se encontró (que debe ser una constante o un tipo).</span><span class="sxs-lookup"><span data-stu-id="e9d33-117">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="e9d33-118">Esta ambigüedad solo está presente en la parte derecha de una expresión de `is`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-118">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="declaration-pattern"></a><span data-ttu-id="e9d33-119">Modelo de declaración</span><span class="sxs-lookup"><span data-stu-id="e9d33-119">Declaration pattern</span></span>

<span data-ttu-id="e9d33-120">El *declaration_pattern* ambos comprueba que una expresión es de un tipo determinado y la convierte a ese tipo si la prueba se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="e9d33-120">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="e9d33-121">Si el *simple_designation* es un identificador, introduce una variable local del tipo especificado denominado por el identificador especificado.</span><span class="sxs-lookup"><span data-stu-id="e9d33-121">If the *simple_designation* is an identifier, it introduces a local variable of the given type named by the given identifier.</span></span> <span data-ttu-id="e9d33-122">Esa variable local se *asigna definitivamente* cuando el resultado de la operación de coincidencia de patrones es true.</span><span class="sxs-lookup"><span data-stu-id="e9d33-122">That local variable is *definitely assigned* when the result of the pattern-matching operation is true.</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="e9d33-123">La semántica en tiempo de ejecución de esta expresión es que prueba el tipo en tiempo de ejecución del operando del lado izquierdo *relational_expression* en el *tipo* del patrón.</span><span class="sxs-lookup"><span data-stu-id="e9d33-123">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span> <span data-ttu-id="e9d33-124">Si es de ese tipo en tiempo de ejecución (o algún subtipo), el resultado del `is operator` es `true`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-124">If it is of that runtime type (or some subtype), the result of the `is operator` is `true`.</span></span> <span data-ttu-id="e9d33-125">Declara una nueva variable local denominada por el *identificador* al que se asigna el valor del operando izquierdo cuando el resultado es `true`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-125">It declares a new local variable named by the *identifier* that is assigned the value of the left-hand operand when the result is `true`.</span></span>

<span data-ttu-id="e9d33-126">Ciertas combinaciones de tipo estático del lado izquierdo y del tipo especificado se consideran incompatibles y producen un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="e9d33-126">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="e9d33-127">Se dice que un valor de tipo estático `E` es *compatible* con el tipo `T` si existe una conversión de identidad, una conversión de referencia implícita, una conversión boxing, una conversión de referencia explícita o una conversión unboxing de `E` a `T`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-127">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`.</span></span> <span data-ttu-id="e9d33-128">Es un error en tiempo de compilación si una expresión de tipo `E` no es compatible con el tipo en un patrón de tipo con el que se encuentra una coincidencia.</span><span class="sxs-lookup"><span data-stu-id="e9d33-128">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

> <span data-ttu-id="e9d33-129">Nota: [en C# 7,1 se amplía esto](../csharp-7.1/generics-pattern-match.md) para permitir una operación de coincidencia de patrones si el tipo de entrada o el tipo `T` es un tipo abierto.</span><span class="sxs-lookup"><span data-stu-id="e9d33-129">Note: [In C# 7.1 we extend this](../csharp-7.1/generics-pattern-match.md) to permit a pattern-matching operation if either the input type or the type `T` is an open type.</span></span> <span data-ttu-id="e9d33-130">Este párrafo se sustituye por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e9d33-130">This paragraph is replaced by the following:</span></span>
> 
> <span data-ttu-id="e9d33-131">Ciertas combinaciones de tipo estático del lado izquierdo y del tipo especificado se consideran incompatibles y producen un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="e9d33-131">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="e9d33-132">Se dice que un valor de tipo estático `E` es *compatible* con el tipo `T` si existe una conversión de identidad, una conversión de referencia implícita, una conversión boxing, una conversión de referencia explícita o una conversión unboxing de `E` a `T`, **o bien, si `E` o `T` es un tipo abierto**.</span><span class="sxs-lookup"><span data-stu-id="e9d33-132">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, **or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="e9d33-133">Es un error en tiempo de compilación si una expresión de tipo `E` no es compatible con el tipo en un patrón de tipo con el que se encuentra una coincidencia.</span><span class="sxs-lookup"><span data-stu-id="e9d33-133">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

<span data-ttu-id="e9d33-134">El modelo de declaración es útil para realizar pruebas de tipo en tiempo de ejecución de tipos de referencia y reemplaza la expresión</span><span class="sxs-lookup"><span data-stu-id="e9d33-134">The declaration pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

<span data-ttu-id="e9d33-135">Con el poco más conciso</span><span class="sxs-lookup"><span data-stu-id="e9d33-135">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v }
```

<span data-ttu-id="e9d33-136">Es un error si el *tipo* es un tipo de valor que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="e9d33-136">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="e9d33-137">El modelo de declaración se puede usar para probar valores de tipos que aceptan valores NULL: un valor de tipo `Nullable<T>` (o un `T`con conversión boxing) coincide con un patrón de tipo `T2 id` si el valor no es NULL y el tipo de `T2` es `T`, o algún tipo base o interfaz de `T`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-137">The declaration pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="e9d33-138">Por ejemplo, en el fragmento de código</span><span class="sxs-lookup"><span data-stu-id="e9d33-138">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

<span data-ttu-id="e9d33-139">La condición de la instrucción `if` es `true` en tiempo de ejecución y la variable `v` contiene el valor `3` de tipo `int` dentro del bloque.</span><span class="sxs-lookup"><span data-stu-id="e9d33-139">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span>

### <a name="constant-pattern"></a><span data-ttu-id="e9d33-140">Patrón de constante</span><span class="sxs-lookup"><span data-stu-id="e9d33-140">Constant pattern</span></span>

```antlr
constant_pattern
    : shift_expression
    ;
```

<span data-ttu-id="e9d33-141">Un patrón de constante prueba el valor de una expresión con respecto a un valor constante.</span><span class="sxs-lookup"><span data-stu-id="e9d33-141">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="e9d33-142">La constante puede ser cualquier expresión constante, como un literal, el nombre de una variable `const` declarada, una constante de enumeración o una expresión `typeof`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-142">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant, or a `typeof` expression.</span></span>

<span data-ttu-id="e9d33-143">Si tanto *e* como *c* son de tipos enteros, se considera que el patrón coincide si el resultado de la expresión `e == c` es `true`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-143">If both *e* and *c* are of integral types, the pattern is considered matched if the result of the expression `e == c` is `true`.</span></span>

<span data-ttu-id="e9d33-144">De lo contrario, se considera que el patrón coincide si `object.Equals(e, c)` devuelve `true`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-144">Otherwise the pattern is considered matching if `object.Equals(e, c)` returns `true`.</span></span> <span data-ttu-id="e9d33-145">En este caso, se trata de un error en tiempo de compilación si el tipo estático de *e* no es *compatible* con el tipo de la constante.</span><span class="sxs-lookup"><span data-stu-id="e9d33-145">In this case it is a compile-time error if the static type of *e* is not *pattern compatible* with the type of the constant.</span></span>

### <a name="var-pattern"></a><span data-ttu-id="e9d33-146">Patrón var</span><span class="sxs-lookup"><span data-stu-id="e9d33-146">Var pattern</span></span>

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

<span data-ttu-id="e9d33-147">Una expresión *e* coincide con un *var_pattern* siempre.</span><span class="sxs-lookup"><span data-stu-id="e9d33-147">An expression *e* matches a *var_pattern* always.</span></span> <span data-ttu-id="e9d33-148">En otras palabras, una coincidencia con un *patrón var* siempre se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="e9d33-148">In other words, a match to a *var pattern* always succeeds.</span></span> <span data-ttu-id="e9d33-149">Si el *simple_designation* es un identificador, en tiempo de ejecución el valor de *e* se enlaza a una variable local recién introducida.</span><span class="sxs-lookup"><span data-stu-id="e9d33-149">If the *simple_designation* is an identifier, then at runtime the value of *e* is bound to a newly introduced local variable.</span></span> <span data-ttu-id="e9d33-150">El tipo de la variable local es el tipo estático de *e*.</span><span class="sxs-lookup"><span data-stu-id="e9d33-150">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="e9d33-151">Es un error si el nombre `var` enlaza a un tipo.</span><span class="sxs-lookup"><span data-stu-id="e9d33-151">It is an error if the name `var` binds to a type.</span></span>

## <a name="switch-statement"></a><span data-ttu-id="e9d33-152">Instrucción switch</span><span class="sxs-lookup"><span data-stu-id="e9d33-152">Switch statement</span></span>

<span data-ttu-id="e9d33-153">La instrucción `switch` se extiende para seleccionar la ejecución del primer bloque que tiene un patrón asociado que coincide con la *expresión switch*.</span><span class="sxs-lookup"><span data-stu-id="e9d33-153">The `switch` statement is extended to select for execution the first block having an associated pattern that matches the *switch expression*.</span></span>

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

<span data-ttu-id="e9d33-154">No se define el orden en el que se buscan coincidencias con los patrones.</span><span class="sxs-lookup"><span data-stu-id="e9d33-154">The order in which patterns are matched is not defined.</span></span> <span data-ttu-id="e9d33-155">Se permite que un compilador coincida con los patrones desordenados y reutilizar los resultados de los patrones ya coincidentes para calcular el resultado de la coincidencia de otros patrones.</span><span class="sxs-lookup"><span data-stu-id="e9d33-155">A compiler is permitted to match patterns out of order, and to reuse the results of already matched patterns to compute the result of matching of other patterns.</span></span>

<span data-ttu-id="e9d33-156">Si hay una *protección de casos* , su expresión es de tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-156">If a *case-guard* is present, its expression is of type `bool`.</span></span> <span data-ttu-id="e9d33-157">Se evalúa como una condición adicional que se debe satisfacer para que el caso se considere satisfecho.</span><span class="sxs-lookup"><span data-stu-id="e9d33-157">It is evaluated as an additional condition that must be satisfied for the case to be considered satisfied.</span></span>

<span data-ttu-id="e9d33-158">Es un error si un *switch_label* puede no tener ningún efecto en el tiempo de ejecución porque su patrón está en los casos anteriores.</span><span class="sxs-lookup"><span data-stu-id="e9d33-158">It is an error if a *switch_label* can have no effect at runtime because its pattern is subsumed by previous cases.</span></span> <span data-ttu-id="e9d33-159">[TODO: deberíamos ser más precisos sobre las técnicas que el compilador necesita usar para alcanzar esta resolución.]</span><span class="sxs-lookup"><span data-stu-id="e9d33-159">[TODO: We should be more precise about the techniques the compiler is required to use to reach this judgment.]</span></span>

<span data-ttu-id="e9d33-160">Una variable de patrón declarada en un *switch_label* se asigna definitivamente en su bloque case si y solo si ese bloque Case contiene exactamente un *switch_label*.</span><span class="sxs-lookup"><span data-stu-id="e9d33-160">A pattern variable declared in a *switch_label* is definitely assigned in its case block if and only if that case block contains precisely one *switch_label*.</span></span>

<span data-ttu-id="e9d33-161">[TODO: deberíamos especificar Cuándo se puede obtener acceso a un *bloque switch* .]</span><span class="sxs-lookup"><span data-stu-id="e9d33-161">[TODO: We should specify when a *switch block* is reachable.]</span></span>

### <a name="scope-of-pattern-variables"></a><span data-ttu-id="e9d33-162">Ámbito de las variables de patrón</span><span class="sxs-lookup"><span data-stu-id="e9d33-162">Scope of pattern variables</span></span>

<span data-ttu-id="e9d33-163">El ámbito de una variable declarada en un patrón es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="e9d33-163">The scope of a variable declared in a pattern is as follows:</span></span>

- <span data-ttu-id="e9d33-164">Si el patrón es una etiqueta de caso, el ámbito de la variable es el *bloque de casos*.</span><span class="sxs-lookup"><span data-stu-id="e9d33-164">If the pattern is a case label, then the scope of the variable is the *case block*.</span></span>

<span data-ttu-id="e9d33-165">En caso contrario, la variable se declara en una expresión *is_pattern* y su ámbito se basa en la construcción que incluye inmediatamente la expresión que contiene la expresión *is_pattern* de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="e9d33-165">Otherwise the variable is declared in an *is_pattern* expression, and its scope is based on the construct immediately enclosing the expression containing the *is_pattern* expression as follows:</span></span>

- <span data-ttu-id="e9d33-166">Si la expresión se encuentra en una expresión lambda con cuerpo de expresión, su ámbito es el cuerpo de la lambda.</span><span class="sxs-lookup"><span data-stu-id="e9d33-166">If the expression is in an expression-bodied lambda, its scope is the body of the lambda.</span></span>
- <span data-ttu-id="e9d33-167">Si la expresión se encuentra en un método o propiedad con forma de expresión, su ámbito es el cuerpo del método o propiedad.</span><span class="sxs-lookup"><span data-stu-id="e9d33-167">If the expression is in an expression-bodied method or property, its scope is the body of the method or property.</span></span>
- <span data-ttu-id="e9d33-168">Si la expresión está en una cláusula `when` de una cláusula `catch`, su ámbito es la cláusula `catch`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-168">If the expression is in a `when` clause of a `catch` clause, its scope is that `catch` clause.</span></span>
- <span data-ttu-id="e9d33-169">Si la expresión se encuentra en un *iteration_statement*, su ámbito es simplemente esa instrucción.</span><span class="sxs-lookup"><span data-stu-id="e9d33-169">If the expression is in an *iteration_statement*, its scope is just that statement.</span></span>
- <span data-ttu-id="e9d33-170">De lo contrario, si la expresión está en otra forma de instrucción, su ámbito es el ámbito que contiene la instrucción.</span><span class="sxs-lookup"><span data-stu-id="e9d33-170">Otherwise if the expression is in some other statement form, its scope is the scope containing the statement.</span></span>

<span data-ttu-id="e9d33-171">Con el fin de determinar el ámbito, se considera que una *embedded_statement* está en su propio ámbito.</span><span class="sxs-lookup"><span data-stu-id="e9d33-171">For the purpose of determining the scope, an *embedded_statement* is considered to be in its own scope.</span></span> <span data-ttu-id="e9d33-172">Por ejemplo, la gramática de una *if_statement* es</span><span class="sxs-lookup"><span data-stu-id="e9d33-172">For example, the grammar for an *if_statement* is</span></span>

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="e9d33-173">Por tanto, si la instrucción controlada de un *if_statement* declara una variable de patrón, su ámbito está restringido a ese *embedded_statement*:</span><span class="sxs-lookup"><span data-stu-id="e9d33-173">So if the controlled statement of an *if_statement* declares a pattern variable, its scope is restricted to that *embedded_statement*:</span></span>

```csharp
if (x) M(y is var z);
```

<span data-ttu-id="e9d33-174">En este caso, el ámbito de `z` es la instrucción incrustada `M(y is var z);`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-174">In this case the scope of `z` is the embedded statement `M(y is var z);`.</span></span>

<span data-ttu-id="e9d33-175">Otros casos son errores por otros motivos (por ejemplo, en el valor predeterminado de un parámetro o en un atributo, ambos son un error porque esos contextos requieren una expresión constante).</span><span class="sxs-lookup"><span data-stu-id="e9d33-175">Other cases are errors for other reasons (e.g. in a parameter's default value or an attribute, both of which are an error because those contexts require a constant expression).</span></span>

> <span data-ttu-id="e9d33-176">[En C# 7,3 hemos agregado los siguientes contextos](../csharp-7.3/expression-variables-in-initializers.md) en los que se puede declarar una variable de patrón:</span><span class="sxs-lookup"><span data-stu-id="e9d33-176">[In C# 7.3 we added the following contexts](../csharp-7.3/expression-variables-in-initializers.md) in which a pattern variable may be declared:</span></span>
> - <span data-ttu-id="e9d33-177">Si la expresión se encuentra en un *inicializador de constructor*, su ámbito es el *inicializador del constructor* y el cuerpo del constructor.</span><span class="sxs-lookup"><span data-stu-id="e9d33-177">If the expression is in a *constructor initializer*, its scope is the *constructor initializer* and the constructor's body.</span></span>
> - <span data-ttu-id="e9d33-178">Si la expresión se encuentra en un inicializador de campo, su ámbito es el *equals_value_clause* en el que aparece.</span><span class="sxs-lookup"><span data-stu-id="e9d33-178">If the expression is in a field initializer, its scope is the *equals_value_clause* in which it appears.</span></span>
> - <span data-ttu-id="e9d33-179">Si la expresión se encuentra en una cláusula de consulta que se especifica para traducirse en el cuerpo de una expresión lambda, su ámbito es simplemente esa expresión.</span><span class="sxs-lookup"><span data-stu-id="e9d33-179">If the expression is in a query clause that is specified to be translated into the body of a lambda, its scope is just that expression.</span></span>

## <a name="changes-to-syntactic-disambiguation"></a><span data-ttu-id="e9d33-180">Cambios en la desambiguación sintáctica</span><span class="sxs-lookup"><span data-stu-id="e9d33-180">Changes to syntactic disambiguation</span></span>

<span data-ttu-id="e9d33-181">Existen situaciones en las que se incluyen los genéricos en los que la C# gramática es ambigua y en la especificación del lenguaje se indica cómo resolver esas ambigüedades:</span><span class="sxs-lookup"><span data-stu-id="e9d33-181">There are situations involving generics where the C# grammar is ambiguous, and the language spec says how to resolve those ambiguities:</span></span>

> #### <a name="7652-grammar-ambiguities"></a><span data-ttu-id="e9d33-182">7.6.5.2 ambigüedades de la gramática</span><span class="sxs-lookup"><span data-stu-id="e9d33-182">7.6.5.2 Grammar ambiguities</span></span>
> <span data-ttu-id="e9d33-183">Las producciones de *nombre simple* (§ 7.6.3) y *acceso a miembros* (§ 7.6.5) pueden dar lugar a ambigüedades en la gramática de expresiones.</span><span class="sxs-lookup"><span data-stu-id="e9d33-183">The productions for *simple-name* (§7.6.3) and *member-access* (§7.6.5) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="e9d33-184">Por ejemplo, la instrucción:</span><span class="sxs-lookup"><span data-stu-id="e9d33-184">For example, the statement:</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="e9d33-185">podría interpretarse como una llamada a `F` con dos argumentos, `G < A` y `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-185">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="e9d33-186">Como alternativa, podría interpretarse como una llamada a `F` con un argumento, que es una llamada a un método genérico `G` con dos argumentos de tipo y un argumento normal.</span><span class="sxs-lookup"><span data-stu-id="e9d33-186">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

> <span data-ttu-id="e9d33-187">Si una secuencia de tokens se puede analizar (en contexto) como un *nombre simple* (§ 7.6.3), *acceso a miembro* (§ 7.6.5) o *puntero-miembro-acceso* (§ 18.5.2) que termina con una *lista de argumentos de tipo* (§ 4.4.1), se examina el token que sigue inmediatamente al cierre `>` token.</span><span class="sxs-lookup"><span data-stu-id="e9d33-187">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="e9d33-188">Si es uno de</span><span class="sxs-lookup"><span data-stu-id="e9d33-188">If it is one of</span></span>
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> <span data-ttu-id="e9d33-189">a continuación, la *lista de argumentos de tipo* se conserva como parte del acceso *simple*, de *acceso a miembros* o de miembro de *puntero* , y se descarta cualquier otro posible análisis de la secuencia de tokens.</span><span class="sxs-lookup"><span data-stu-id="e9d33-189">then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="e9d33-190">De lo contrario, la *lista de argumentos de tipo* no se considera parte del *nombre simple*, *acceso a miembros* o > puntero- *miembro-acceso*, incluso si no hay otro posible análisis de la secuencia de tokens.</span><span class="sxs-lookup"><span data-stu-id="e9d33-190">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or > *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="e9d33-191">Tenga en cuenta que estas reglas no se aplican al analizar una *lista de argumentos de tipo* en un nombre de espacio de *nombres o-tipo* (§ 3,8).</span><span class="sxs-lookup"><span data-stu-id="e9d33-191">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span> <span data-ttu-id="e9d33-192">La instrucción</span><span class="sxs-lookup"><span data-stu-id="e9d33-192">The statement</span></span>
> ```csharp
> F(G<A,B>(7));
> ```
> <span data-ttu-id="e9d33-193">, según esta regla, se interpretará como una llamada a `F` con un argumento, que es una llamada a un método genérico `G` con dos argumentos de tipo y un argumento normal.</span><span class="sxs-lookup"><span data-stu-id="e9d33-193">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="e9d33-194">Las instrucciones</span><span class="sxs-lookup"><span data-stu-id="e9d33-194">The statements</span></span>
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> <span data-ttu-id="e9d33-195">cada uno de ellos se interpretará como una llamada a `F` con dos argumentos.</span><span class="sxs-lookup"><span data-stu-id="e9d33-195">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="e9d33-196">La instrucción</span><span class="sxs-lookup"><span data-stu-id="e9d33-196">The statement</span></span>
> ```csharp
> x = F < A > +y;
> ```
> <span data-ttu-id="e9d33-197">se interpretará como un operador menor que, mayor que y unario más, como si la instrucción se hubiera escrito `x = (F < A) > (+y)`, en lugar de como un *nombre simple* con una *lista de argumentos de tipo* seguido de un operador binario Plus.</span><span class="sxs-lookup"><span data-stu-id="e9d33-197">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple-name* with a *type-argument-list* followed by a binary plus operator.</span></span> <span data-ttu-id="e9d33-198">En la instrucción</span><span class="sxs-lookup"><span data-stu-id="e9d33-198">In the statement</span></span>
> ```csharp
> x = y is C<T> + z;
> ```
> <span data-ttu-id="e9d33-199">los tokens `C<T>` se interpretan como un *espacio de nombres o un nombre de tipo* con una *lista de argumentos de tipo*.</span><span class="sxs-lookup"><span data-stu-id="e9d33-199">the tokens `C<T>` are interpreted as a *namespace-or-type-name* with a *type-argument-list*.</span></span>

<span data-ttu-id="e9d33-200">Hay una serie de cambios que se introducen C# en 7 que hacen que estas reglas de desambiguación dejen de ser suficientes para controlar la complejidad del lenguaje.</span><span class="sxs-lookup"><span data-stu-id="e9d33-200">There are a number of changes being introduced in C# 7 that make these disambiguation rules no longer sufficient to handle the complexity of the language.</span></span>

### <a name="out-variable-declarations"></a><span data-ttu-id="e9d33-201">Declaraciones de variable out</span><span class="sxs-lookup"><span data-stu-id="e9d33-201">Out variable declarations</span></span>

<span data-ttu-id="e9d33-202">Ahora es posible declarar una variable en un argumento out:</span><span class="sxs-lookup"><span data-stu-id="e9d33-202">It is now possible to declare a variable in an out argument:</span></span>

```csharp
M(out Type name);
```

<span data-ttu-id="e9d33-203">Sin embargo, el tipo puede ser genérico:</span><span class="sxs-lookup"><span data-stu-id="e9d33-203">However, the type may be generic:</span></span> 

```csharp
M(out A<B> name);
```

<span data-ttu-id="e9d33-204">Dado que la gramática de idioma para el argumento usa *Expression*, este contexto está sujeto a la regla de desambiguación.</span><span class="sxs-lookup"><span data-stu-id="e9d33-204">Since the language grammar for the argument uses *expression*, this context is subject to the disambiguation rule.</span></span> <span data-ttu-id="e9d33-205">En este caso, el `>` de cierre va seguido de un *identificador*, que no es uno de los tokens que permite que se trate como una *lista de argumentos de tipo*.</span><span class="sxs-lookup"><span data-stu-id="e9d33-205">In this case the closing `>` is followed by an *identifier*, which is not one of the tokens that permits it to be treated as a *type-argument-list*.</span></span> <span data-ttu-id="e9d33-206">Por lo tanto, se propone **Agregar el *identificador* al conjunto de tokens que desencadena la desambiguación en una *lista de argumentos de tipo*.**</span><span class="sxs-lookup"><span data-stu-id="e9d33-206">I therefore propose to **add *identifier* to the set of tokens that triggers the disambiguation to a *type-argument-list*.**</span></span>

### <a name="tuples-and-deconstruction-declarations"></a><span data-ttu-id="e9d33-207">Tuplas y declaraciones de desconstrucción</span><span class="sxs-lookup"><span data-stu-id="e9d33-207">Tuples and deconstruction declarations</span></span>

<span data-ttu-id="e9d33-208">Un literal de tupla se ejecuta exactamente en el mismo problema.</span><span class="sxs-lookup"><span data-stu-id="e9d33-208">A tuple literal runs into exactly the same issue.</span></span> <span data-ttu-id="e9d33-209">Considere la expresión de tupla.</span><span class="sxs-lookup"><span data-stu-id="e9d33-209">Consider the tuple expression</span></span>

```csharp
(A < B, C > D, E < F, G > H)
```

<span data-ttu-id="e9d33-210">En las 6 C# reglas anteriores para analizar una lista de argumentos, esto se analizaría como una tupla con cuatro elementos, empezando por `A < B` como primer.</span><span class="sxs-lookup"><span data-stu-id="e9d33-210">Under the old C# 6 rules for parsing an argument list, this would parse as a tuple with four elements, starting with `A < B` as the first.</span></span> <span data-ttu-id="e9d33-211">Sin embargo, cuando esto aparece a la izquierda de una desconstrucción, queremos que la desambiguación se desencadene por el token del *identificador* como se describió anteriormente:</span><span class="sxs-lookup"><span data-stu-id="e9d33-211">However, when this appears on the left of a deconstruction, we want the disambiguation triggered by the *identifier* token as described above:</span></span>

```csharp
(A<B,C> D, E<F,G> H) = e;
```

<span data-ttu-id="e9d33-212">Se trata de una declaración de desconstrucción que declara dos variables, la primera de las cuales es de tipo `A<B,C>` y con el nombre `D`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-212">This is a deconstruction declaration which declares two variables, the first of which is of type `A<B,C>` and named `D`.</span></span> <span data-ttu-id="e9d33-213">En otras palabras, el literal de tupla contiene dos expresiones, cada una de las cuales es una expresión de declaración.</span><span class="sxs-lookup"><span data-stu-id="e9d33-213">In other words, the tuple literal contains two expressions, each of which is a declaration expression.</span></span>

<span data-ttu-id="e9d33-214">Para simplificar la especificación y el compilador, propongo que este literal de tupla se analice como una tupla de dos elementos dondequiera que aparezca (ya sea o no aparece en el lado izquierdo de una asignación).</span><span class="sxs-lookup"><span data-stu-id="e9d33-214">For simplicity of the specification and compiler, I propose that this tuple literal be parsed as a two-element tuple wherever it appears (whether or not it appears on the left-hand-side of an assignment).</span></span> <span data-ttu-id="e9d33-215">Esto sería un resultado natural de la desambiguación descrita en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="e9d33-215">That would be a natural result of the disambiguation described in the previous section.</span></span>

### <a name="pattern-matching"></a><span data-ttu-id="e9d33-216">Coincidencia de patrones</span><span class="sxs-lookup"><span data-stu-id="e9d33-216">Pattern-matching</span></span>

<span data-ttu-id="e9d33-217">La coincidencia de patrones introduce un nuevo contexto en el que se produce la ambigüedad del tipo de expresión.</span><span class="sxs-lookup"><span data-stu-id="e9d33-217">Pattern matching introduces a new context where the expression-type ambiguity arises.</span></span> <span data-ttu-id="e9d33-218">Anteriormente, el lado derecho de un operador `is` era un tipo.</span><span class="sxs-lookup"><span data-stu-id="e9d33-218">Previously the right-hand-side of an `is` operator was a type.</span></span> <span data-ttu-id="e9d33-219">Ahora puede ser un tipo o una expresión, y si es un tipo, puede ir seguido de un identificador.</span><span class="sxs-lookup"><span data-stu-id="e9d33-219">Now it can be a type or expression, and if it is a type it may be followed by an identifier.</span></span> <span data-ttu-id="e9d33-220">Técnicamente, esto puede cambiar el significado del código existente:</span><span class="sxs-lookup"><span data-stu-id="e9d33-220">This can, technically, change the meaning of existing code:</span></span>

```csharp
var x = e is T < A > B;
```

<span data-ttu-id="e9d33-221">Esto podría analizarse en reglas de C# 6 como</span><span class="sxs-lookup"><span data-stu-id="e9d33-221">This could be parsed under C#6 rules as</span></span>

```csharp
var x = ((e is T) < A) > B;
```

<span data-ttu-id="e9d33-222">pero bajo bajo las reglas de C# 7 (con la desambiguación propuesta anteriormente) se analizaría como</span><span class="sxs-lookup"><span data-stu-id="e9d33-222">but under under C#7 rules (with the disambiguation proposed above) would be parsed as</span></span>

```csharp
var x = e is T<A> B;
```

<span data-ttu-id="e9d33-223">que declara una variable `B` de tipo `T<A>`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-223">which declares a variable `B` of type `T<A>`.</span></span> <span data-ttu-id="e9d33-224">Afortunadamente, los compiladores nativo y Roslyn tienen un error por el que dan un error de sintaxis en el código de C# 6.</span><span class="sxs-lookup"><span data-stu-id="e9d33-224">Fortunately, the native and Roslyn compilers have a bug whereby they give a syntax error on the C#6 code.</span></span> <span data-ttu-id="e9d33-225">Por lo tanto, este cambio importante concreto no es un problema.</span><span class="sxs-lookup"><span data-stu-id="e9d33-225">Therefore this particular breaking change is not a concern.</span></span>

<span data-ttu-id="e9d33-226">La coincidencia de patrones introduce tokens adicionales que deben impulsar la resolución de ambigüedades al seleccionar un tipo.</span><span class="sxs-lookup"><span data-stu-id="e9d33-226">Pattern-matching introduces additional tokens that should drive the ambiguity resolution toward selecting a type.</span></span> <span data-ttu-id="e9d33-227">Los siguientes ejemplos de código de C# 6 válido existente se interrumpirían sin reglas de desambiguación adicionales:</span><span class="sxs-lookup"><span data-stu-id="e9d33-227">The following examples of existing valid C#6 code would be broken without additional disambiguation rules:</span></span>

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a><span data-ttu-id="e9d33-228">Cambio propuesto en la regla de desambiguación</span><span class="sxs-lookup"><span data-stu-id="e9d33-228">Proposed change to the disambiguation rule</span></span>

<span data-ttu-id="e9d33-229">Propongo revisar la especificación para cambiar la lista de tokens de ambigüedad de</span><span class="sxs-lookup"><span data-stu-id="e9d33-229">I propose to revise the specification to change the list of disambiguating tokens from</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

<span data-ttu-id="e9d33-230">to</span><span class="sxs-lookup"><span data-stu-id="e9d33-230">to</span></span>

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

<span data-ttu-id="e9d33-231">Y, en ciertos contextos, tratamos el *identificador* como un token de ambigüedad.</span><span class="sxs-lookup"><span data-stu-id="e9d33-231">And, in certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="e9d33-232">Estos contextos son los lugares en los que la secuencia de los tokens que se van a la ambigüedad está inmediatamente precedida por una de las palabras clave `is`, `case`o `out`, o cuando se analiza el primer elemento de un literal de tupla (en cuyo caso los tokens van precedidos de `(` o `:` y el identificador va seguido de un `,`) o de un elemento subsiguiente de</span><span class="sxs-lookup"><span data-stu-id="e9d33-232">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case`, or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>

### <a name="modified-disambiguation-rule"></a><span data-ttu-id="e9d33-233">Regla de desambiguación modificada</span><span class="sxs-lookup"><span data-stu-id="e9d33-233">Modified disambiguation rule</span></span>

<span data-ttu-id="e9d33-234">La regla de desambiguación revisada sería similar a la siguiente</span><span class="sxs-lookup"><span data-stu-id="e9d33-234">The revised disambiguation rule would be something like this</span></span>

> <span data-ttu-id="e9d33-235">Si se puede analizar una secuencia de tokens (en contexto) como un *nombre simple* (§ 7.6.3), *acceso a miembro* (§ 7.6.5) o *acceso a miembro de puntero* (§ 18.5.2) que termina con una lista de *argumentos de tipo* (§ 4.4.1), se examina el token inmediatamente posterior al cierre `>` token para ver si es</span><span class="sxs-lookup"><span data-stu-id="e9d33-235">If a sequence of tokens can be parsed (in context) as a *simple-name* (§7.6.3), *member-access* (§7.6.5), or *pointer-member-access* (§18.5.2) ending with a *type-argument-list* (§4.4.1), the token immediately following the closing `>` token is examined, to see if it is</span></span>
> - <span data-ttu-id="e9d33-236">Uno de `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; de</span><span class="sxs-lookup"><span data-stu-id="e9d33-236">One of `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; or</span></span>
> - <span data-ttu-id="e9d33-237">Uno de los operadores relacionales `<  >  <=  >=  is as`; de</span><span class="sxs-lookup"><span data-stu-id="e9d33-237">One of the relational operators `<  >  <=  >=  is as`; or</span></span>
> - <span data-ttu-id="e9d33-238">Una palabra clave de consulta contextual que aparece dentro de una expresión de consulta; de</span><span class="sxs-lookup"><span data-stu-id="e9d33-238">A contextual query keyword appearing inside a query expression; or</span></span>
> - <span data-ttu-id="e9d33-239">En ciertos contextos, tratamos el *identificador* como un token de ambigüedad.</span><span class="sxs-lookup"><span data-stu-id="e9d33-239">In certain contexts, we treat *identifier* as a disambiguating token.</span></span> <span data-ttu-id="e9d33-240">Estos contextos son donde la secuencia de tokens que se va a desambigüedadr está inmediatamente precedida por una de las palabras clave `is`, `case` o `out`, o bien al analizar el primer elemento de un literal de tupla (en cuyo caso los tokens van precedidos de `(` o `:` y el identificador va seguido de un `,`) o un elemento subsiguiente de un literal de tupla</span><span class="sxs-lookup"><span data-stu-id="e9d33-240">Those contexts are where the sequence of tokens being disambiguated is immediately preceded by one of the keywords `is`, `case` or `out`, or arises while parsing the first element of a tuple literal (in which case the tokens are preceded by `(` or `:` and the identifier is followed by a `,`) or a subsequent element of a tuple literal.</span></span>
> 
> <span data-ttu-id="e9d33-241">Si el token siguiente se encuentra entre esta lista o un identificador en un contexto de este tipo, la *lista de argumentos de tipo* se conserva como parte del *nombre simple*, acceso a *miembros* o puntero a *miembro de puntero* , y se descarta cualquier otro posible análisis de la secuencia de tokens.</span><span class="sxs-lookup"><span data-stu-id="e9d33-241">If the following token is among this list, or an identifier in such a context, then the *type-argument-list* is retained as part of the *simple-name*, *member-access* or  *pointer-member-access* and any other possible parse of the sequence of tokens is discarded.</span></span>  <span data-ttu-id="e9d33-242">De lo contrario, la *lista de argumentos de tipo* no se considera parte del acceso *simple*, de acceso a *miembros* o de *miembro de puntero*, incluso si no hay otro posible análisis de la secuencia de tokens.</span><span class="sxs-lookup"><span data-stu-id="e9d33-242">Otherwise, the *type-argument-list* is not considered to be part of the *simple-name*, *member-access* or *pointer-member-access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="e9d33-243">Tenga en cuenta que estas reglas no se aplican al analizar una *lista de argumentos de tipo* en un nombre de espacio de *nombres o-tipo* (§ 3,8).</span><span class="sxs-lookup"><span data-stu-id="e9d33-243">Note that these rules are not applied when parsing a *type-argument-list* in a *namespace-or-type-name* (§3.8).</span></span>

### <a name="breaking-changes-due-to-this-proposal"></a><span data-ttu-id="e9d33-244">Cambios importantes debidos a esta propuesta</span><span class="sxs-lookup"><span data-stu-id="e9d33-244">Breaking changes due to this proposal</span></span>

<span data-ttu-id="e9d33-245">No se conoce ningún cambio importante debido a esta regla de desambiguación propuesta.</span><span class="sxs-lookup"><span data-stu-id="e9d33-245">No breaking changes are known due to this proposed disambiguation rule.</span></span>

### <a name="interesting-examples"></a><span data-ttu-id="e9d33-246">Ejemplos interesantes</span><span class="sxs-lookup"><span data-stu-id="e9d33-246">Interesting examples</span></span>

<span data-ttu-id="e9d33-247">Estos son algunos resultados interesantes de estas reglas de desambiguación:</span><span class="sxs-lookup"><span data-stu-id="e9d33-247">Here are some interesting results of these disambiguation rules:</span></span>

<span data-ttu-id="e9d33-248">La expresión `(A < B, C > D)` es una tupla con dos elementos, cada una de las cuales es una comparación.</span><span class="sxs-lookup"><span data-stu-id="e9d33-248">The expression `(A < B, C > D)` is a tuple with two elements, each a comparison.</span></span>

<span data-ttu-id="e9d33-249">La expresión `(A<B,C> D, E)` es una tupla con dos elementos, el primero de los cuales es una expresión de declaración.</span><span class="sxs-lookup"><span data-stu-id="e9d33-249">The expression `(A<B,C> D, E)` is a tuple with two elements, the first of which is a declaration expression.</span></span>

<span data-ttu-id="e9d33-250">La `M(A < B, C > D, E)` de invocación tiene tres argumentos.</span><span class="sxs-lookup"><span data-stu-id="e9d33-250">The invocation `M(A < B, C > D, E)` has three arguments.</span></span>

<span data-ttu-id="e9d33-251">La `M(out A<B,C> D, E)` de invocación tiene dos argumentos, el primero de los cuales es una declaración de `out`.</span><span class="sxs-lookup"><span data-stu-id="e9d33-251">The invocation `M(out A<B,C> D, E)` has two arguments, the first of which is an `out` declaration.</span></span>

<span data-ttu-id="e9d33-252">La expresión `e is A<B> C` utiliza una expresión de declaración.</span><span class="sxs-lookup"><span data-stu-id="e9d33-252">The expression `e is A<B> C` uses a declaration expression.</span></span>

<span data-ttu-id="e9d33-253">La etiqueta Case `case A<B> C:` usa una expresión de declaración.</span><span class="sxs-lookup"><span data-stu-id="e9d33-253">The case label `case A<B> C:` uses a declaration expression.</span></span>

## <a name="some-examples-of-pattern-matching"></a><span data-ttu-id="e9d33-254">Algunos ejemplos de coincidencia de patrones</span><span class="sxs-lookup"><span data-stu-id="e9d33-254">Some examples of pattern matching</span></span>

### <a name="is-as"></a><span data-ttu-id="e9d33-255">Es como</span><span class="sxs-lookup"><span data-stu-id="e9d33-255">Is-As</span></span>

<span data-ttu-id="e9d33-256">Podemos reemplazar la expresión</span><span class="sxs-lookup"><span data-stu-id="e9d33-256">We can replace the idiom</span></span>

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

<span data-ttu-id="e9d33-257">Con el aspecto más conciso y directo</span><span class="sxs-lookup"><span data-stu-id="e9d33-257">With the slightly more concise and direct</span></span>

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a><span data-ttu-id="e9d33-258">Prueba de valores NULL</span><span class="sxs-lookup"><span data-stu-id="e9d33-258">Testing nullable</span></span>

<span data-ttu-id="e9d33-259">Podemos reemplazar la expresión</span><span class="sxs-lookup"><span data-stu-id="e9d33-259">We can replace the idiom</span></span>

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

<span data-ttu-id="e9d33-260">Con el aspecto más conciso y directo</span><span class="sxs-lookup"><span data-stu-id="e9d33-260">With the slightly more concise and direct</span></span>

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a><span data-ttu-id="e9d33-261">Simplificación aritmética</span><span class="sxs-lookup"><span data-stu-id="e9d33-261">Arithmetic simplification</span></span>

<span data-ttu-id="e9d33-262">Supongamos que definimos un conjunto de tipos recursivos para representar expresiones (según una propuesta independiente):</span><span class="sxs-lookup"><span data-stu-id="e9d33-262">Suppose we define a set of recursive types to represent expressions (per a separate proposal):</span></span>

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

<span data-ttu-id="e9d33-263">Ahora se puede definir una función para calcular el derivado (no reducido) de una expresión:</span><span class="sxs-lookup"><span data-stu-id="e9d33-263">Now we can define a function to compute the (unreduced) derivative of an expression:</span></span>

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

<span data-ttu-id="e9d33-264">Un Simplificador de expresiones muestra patrones posicionales:</span><span class="sxs-lookup"><span data-stu-id="e9d33-264">An expression simplifier demonstrates positional patterns:</span></span>

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
