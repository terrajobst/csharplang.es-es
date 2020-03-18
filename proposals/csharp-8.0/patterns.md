---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483983"
---
# <a name="recursive-pattern-matching"></a><span data-ttu-id="2c549-101">Coincidencia de patrones recursivos</span><span class="sxs-lookup"><span data-stu-id="2c549-101">Recursive Pattern Matching</span></span>

## <a name="summary"></a><span data-ttu-id="2c549-102">Resumen</span><span class="sxs-lookup"><span data-stu-id="2c549-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="2c549-103">Las extensiones de coincidencia C# de patrones para permiten muchas de las ventajas de los tipos de datos algebraicos y la coincidencia de patrones de los lenguajes funcionales, pero de una manera que se integra sin problemas con la sensación del lenguaje subyacente.</span><span class="sxs-lookup"><span data-stu-id="2c549-103">Pattern matching extensions for C# enable many of the benefits of algebraic data types and pattern matching from functional languages, but in a way that smoothly integrates with the feel of the underlying language.</span></span> <span data-ttu-id="2c549-104">Los elementos de este enfoque están inspirados en las características relacionadas en [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Coincidencia de patrones extensible a través de un lenguaje ligero") los lenguajes de programación y [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Coincidencia de objetos con patrones, página 273").</span><span class="sxs-lookup"><span data-stu-id="2c549-104">Elements of this approach are inspired by related features in the programming languages [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Extensible Pattern Matching Via a Lightweight Language") and [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Matching Objects With Patterns, page 273").</span></span>

## <a name="detailed-design"></a><span data-ttu-id="2c549-105">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="2c549-105">Detailed design</span></span>
[design]: #detailed-design

### <a name="is-expression"></a><span data-ttu-id="2c549-106">Expresión is</span><span class="sxs-lookup"><span data-stu-id="2c549-106">Is Expression</span></span>

<span data-ttu-id="2c549-107">El operador `is` se extiende para probar una expresión con un *patrón*.</span><span class="sxs-lookup"><span data-stu-id="2c549-107">The `is` operator is extended to test an expression against a *pattern*.</span></span>

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

<span data-ttu-id="2c549-108">Esta forma de *relational_expression* se suma a los formularios existentes en la C# especificación.</span><span class="sxs-lookup"><span data-stu-id="2c549-108">This form of *relational_expression* is in addition to the existing forms in the C# specification.</span></span> <span data-ttu-id="2c549-109">Es un error en tiempo de compilación si el *relational_expression* a la izquierda del token de `is` no designa un valor o no tiene un tipo.</span><span class="sxs-lookup"><span data-stu-id="2c549-109">It is a compile-time error if the *relational_expression* to the left of the `is` token does not designate a value or does not have a type.</span></span>

<span data-ttu-id="2c549-110">Cada *identificador* del patrón introduce una nueva variable local que se *asigna definitivamente* después de que el operador de `is` sea `true` (es decir, se *asigna definitivamente cuando es true*).</span><span class="sxs-lookup"><span data-stu-id="2c549-110">Every *identifier* of the pattern introduces a new local variable that is *definitely assigned* after the `is` operator is `true` (i.e. *definitely assigned when true*).</span></span>

> <span data-ttu-id="2c549-111">Nota: técnicamente existe una ambigüedad entre el *tipo* en un `is-expression` y *constant_pattern*, cualquiera de los cuales puede ser un análisis válido de un identificador calificado.</span><span class="sxs-lookup"><span data-stu-id="2c549-111">Note: There is technically an ambiguity between *type* in an `is-expression` and *constant_pattern*, either of which might be a valid parse of a qualified identifier.</span></span> <span data-ttu-id="2c549-112">Intentamos enlazarlo como un tipo para la compatibilidad con versiones anteriores del lenguaje; solo si se produce un error, lo resolvemos como hacemos una expresión en otros contextos, lo primero que se encontró (que debe ser una constante o un tipo).</span><span class="sxs-lookup"><span data-stu-id="2c549-112">We try to bind it as a type for compatibility with previous versions of the language; only if that fails do we resolve it as we do an expression in other contexts, to the first thing found (which must be either a constant or a type).</span></span> <span data-ttu-id="2c549-113">Esta ambigüedad solo está presente en la parte derecha de una expresión de `is`.</span><span class="sxs-lookup"><span data-stu-id="2c549-113">This ambiguity is only present on the right-hand-side of an `is` expression.</span></span>

### <a name="patterns"></a><span data-ttu-id="2c549-114">Patrones</span><span class="sxs-lookup"><span data-stu-id="2c549-114">Patterns</span></span>

<span data-ttu-id="2c549-115">Los patrones se usan en el operador *is_pattern* , en un *switch_statement*y en un *switch_expression* para expresar la forma de los datos en los que se van a comparar los datos entrantes (que llamamos al valor de entrada).</span><span class="sxs-lookup"><span data-stu-id="2c549-115">Patterns are used in the *is_pattern* operator, in a *switch_statement*, and in a *switch_expression* to express the shape of data against which incoming data  (which we call the input value) is to be compared.</span></span> <span data-ttu-id="2c549-116">Los patrones pueden ser recursivos para que se puedan comparar partes de los datos con subpatrones.</span><span class="sxs-lookup"><span data-stu-id="2c549-116">Patterns may be recursive so that parts of the data may be matched against sub-patterns.</span></span>

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a><span data-ttu-id="2c549-117">Modelo de declaración</span><span class="sxs-lookup"><span data-stu-id="2c549-117">Declaration Pattern</span></span>

```antlr
declaration_pattern
    : type simple_designation
    ;
```

<span data-ttu-id="2c549-118">El *declaration_pattern* ambos comprueba que una expresión es de un tipo determinado y la convierte a ese tipo si la prueba se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="2c549-118">The *declaration_pattern* both tests that an expression is of a given type and casts it to that type if the test succeeds.</span></span> <span data-ttu-id="2c549-119">Esto puede introducir una variable local del tipo especificado denominado por el identificador dado, si la designación es una *single_variable_designation*.</span><span class="sxs-lookup"><span data-stu-id="2c549-119">This may introduce a local variable of the given type named by the given identifier, if the designation is a *single_variable_designation*.</span></span> <span data-ttu-id="2c549-120">Esa variable local se *asigna definitivamente* cuando se `true`el resultado de la operación de coincidencia de patrones.</span><span class="sxs-lookup"><span data-stu-id="2c549-120">That local variable is *definitely assigned* when the result of the pattern-matching operation is `true`.</span></span>

<span data-ttu-id="2c549-121">La semántica en tiempo de ejecución de esta expresión es que prueba el tipo en tiempo de ejecución del operando del lado izquierdo *relational_expression* en el *tipo* del patrón.</span><span class="sxs-lookup"><span data-stu-id="2c549-121">The runtime semantic of this expression is that it tests the runtime type of the left-hand *relational_expression* operand against the *type* in the pattern.</span></span>  <span data-ttu-id="2c549-122">Si es de ese tipo en tiempo de ejecución (o algún subtipo) y no `null`, el resultado de la `is operator` es `true`.</span><span class="sxs-lookup"><span data-stu-id="2c549-122">If it is of that runtime type (or some subtype) and not `null`, the result of the `is operator` is `true`.</span></span>

<span data-ttu-id="2c549-123">Ciertas combinaciones de tipo estático del lado izquierdo y del tipo especificado se consideran incompatibles y producen un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="2c549-123">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="2c549-124">Se dice que un valor de tipo estático `E` es *compatible* con el patrón con un tipo `T` si existe una conversión de identidad, una conversión de referencia implícita, una conversión boxing, una conversión de referencia explícita o una conversión unboxing de `E` a `T`, o si uno de esos tipos es un tipo abierto.</span><span class="sxs-lookup"><span data-stu-id="2c549-124">A value of static type `E` is said to be *pattern-compatible* with a type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`, or if one of those types is an open type.</span></span> <span data-ttu-id="2c549-125">Es un error en tiempo de compilación si una entrada de tipo `E` no es *compatible* con el patrón con el *tipo* de un patrón de tipo con el que se encuentra una coincidencia.</span><span class="sxs-lookup"><span data-stu-id="2c549-125">It is a compile-time error if an input of type `E` is not *pattern-compatible* with the *type* in a type pattern that it is matched with.</span></span>

<span data-ttu-id="2c549-126">El patrón de tipo es útil para realizar pruebas de tipo en tiempo de ejecución de tipos de referencia y reemplaza la expresión</span><span class="sxs-lookup"><span data-stu-id="2c549-126">The type pattern is useful for performing run-time type tests of reference types, and replaces the idiom</span></span>

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

<span data-ttu-id="2c549-127">Con el poco más conciso</span><span class="sxs-lookup"><span data-stu-id="2c549-127">With the slightly more concise</span></span>

```csharp
if (expr is Type v) { // code using v
```

<span data-ttu-id="2c549-128">Es un error si el *tipo* es un tipo de valor que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="2c549-128">It is an error if *type* is a nullable value type.</span></span>

<span data-ttu-id="2c549-129">El patrón de tipo se puede usar para probar valores de tipos que aceptan valores NULL: un valor de tipo `Nullable<T>` (o un `T`con conversión boxing) coincide con un patrón de tipo `T2 id` si el valor no es NULL y el tipo de `T2` es `T`, o algún tipo base o interfaz de `T`.</span><span class="sxs-lookup"><span data-stu-id="2c549-129">The type pattern can be used to test values of nullable types: a value of type `Nullable<T>` (or a boxed `T`) matches a type pattern `T2 id` if the value is non-null and the type of `T2` is `T`, or some base type or interface of `T`.</span></span> <span data-ttu-id="2c549-130">Por ejemplo, en el fragmento de código</span><span class="sxs-lookup"><span data-stu-id="2c549-130">For example, in the code fragment</span></span>

```csharp
int? x = 3;
if (x is int v) { // code using v
```

<span data-ttu-id="2c549-131">La condición de la instrucción `if` es `true` en tiempo de ejecución y la variable `v` contiene el valor `3` de tipo `int` dentro del bloque.</span><span class="sxs-lookup"><span data-stu-id="2c549-131">The condition of the `if` statement is `true` at runtime and the variable `v` holds the value `3` of type `int` inside the block.</span></span> <span data-ttu-id="2c549-132">Después del bloqueo, la variable `v` está en el ámbito pero no se asigna de forma definitiva.</span><span class="sxs-lookup"><span data-stu-id="2c549-132">After the block the variable `v` is in scope but not definitely assigned.</span></span>

#### <a name="constant-pattern"></a><span data-ttu-id="2c549-133">Patrón de constante</span><span class="sxs-lookup"><span data-stu-id="2c549-133">Constant Pattern</span></span>

```antlr
constant_pattern
    : constant_expression
    ;
```

<span data-ttu-id="2c549-134">Un patrón de constante prueba el valor de una expresión con respecto a un valor constante.</span><span class="sxs-lookup"><span data-stu-id="2c549-134">A constant pattern tests the value of an expression against a constant value.</span></span> <span data-ttu-id="2c549-135">La constante puede ser cualquier expresión constante, como un literal, el nombre de una variable `const` declarada o una constante de enumeración.</span><span class="sxs-lookup"><span data-stu-id="2c549-135">The constant may be any constant expression, such as a literal, the name of a declared `const` variable, or an enumeration constant.</span></span> <span data-ttu-id="2c549-136">Cuando el valor de entrada no es un tipo abierto, la expresión constante se convierte implícitamente al tipo de la expresión coincidente; Si el tipo del valor de entrada no es *compatible* con el patrón con el tipo de la expresión constante, la operación de coincidencia de patrones es un error.</span><span class="sxs-lookup"><span data-stu-id="2c549-136">When the input value is not an open type, the constant expression is implicitly converted to the type of the matched expression; if the type of the input value is not *pattern-compatible* with the type of the constant expression, the pattern-matching operation is an error.</span></span>

<span data-ttu-id="2c549-137">El patrón *c* se considera que coincide con el valor de entrada convertido *e* si `object.Equals(c, e)` devolvería `true`.</span><span class="sxs-lookup"><span data-stu-id="2c549-137">The pattern *c* is considered matching the converted input value *e* if `object.Equals(c, e)` would return `true`.</span></span>

<span data-ttu-id="2c549-138">Esperamos ver `e is null` como la manera más común de comprobar `null` en el código recién escrito, ya que no puede invocar un `operator==`definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="2c549-138">We expect to see `e is null` as the most common way to test for `null` in newly written code, as it cannot invoke a user-defined `operator==`.</span></span>

#### <a name="var-pattern"></a><span data-ttu-id="2c549-139">Patrón var</span><span class="sxs-lookup"><span data-stu-id="2c549-139">Var Pattern</span></span>

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

<span data-ttu-id="2c549-140">Si la *designación* es una *simple_designation*, una expresión *e* coincide con el patrón.</span><span class="sxs-lookup"><span data-stu-id="2c549-140">If the *designation* is a *simple_designation*, an expression *e* matches the pattern.</span></span> <span data-ttu-id="2c549-141">En otras palabras, una coincidencia con un *patrón var* siempre se realiza correctamente con una *simple_designation*.</span><span class="sxs-lookup"><span data-stu-id="2c549-141">In other words, a match to a *var pattern* always succeeds with a *simple_designation*.</span></span> <span data-ttu-id="2c549-142">Si el *simple_designation* es un *single_variable_designation*, el valor de *e* se enlaza a una variable local recién introducida.</span><span class="sxs-lookup"><span data-stu-id="2c549-142">If the *simple_designation* is a *single_variable_designation*, the value of *e* is bounds to a newly introduced local variable.</span></span> <span data-ttu-id="2c549-143">El tipo de la variable local es el tipo estático de *e*.</span><span class="sxs-lookup"><span data-stu-id="2c549-143">The type of the local variable is the static type of *e*.</span></span>

<span data-ttu-id="2c549-144">Si la *designación* es una *tuple_designation*, el patrón es equivalente a un *positional_pattern* con el formato `(var` *designación*,... `)` donde la *designación*son las que se encuentran en el *tuple_designation*.</span><span class="sxs-lookup"><span data-stu-id="2c549-144">If the *designation* is a *tuple_designation*, then the pattern is equivalent to a *positional_pattern* of the form `(var` *designation*, ... `)` where the *designation*s are those found within the *tuple_designation*.</span></span>  <span data-ttu-id="2c549-145">Por ejemplo, el patrón `var (x, (y, z))` es equivalente a `(var x, (var y, var z))`.</span><span class="sxs-lookup"><span data-stu-id="2c549-145">For example, the pattern `var (x, (y, z))` is equivalent to `(var x, (var y, var z))`.</span></span>

<span data-ttu-id="2c549-146">Es un error si el nombre `var` enlaza a un tipo.</span><span class="sxs-lookup"><span data-stu-id="2c549-146">It is an error if the name `var` binds to a type.</span></span>

#### <a name="discard-pattern"></a><span data-ttu-id="2c549-147">Patrón de descartar</span><span class="sxs-lookup"><span data-stu-id="2c549-147">Discard Pattern</span></span>

```antlr
discard_pattern
    : '_'
    ;
```

<span data-ttu-id="2c549-148">Una expresión *e* coincide con el patrón `_` siempre.</span><span class="sxs-lookup"><span data-stu-id="2c549-148">An expression *e* matches the pattern `_` always.</span></span> <span data-ttu-id="2c549-149">En otras palabras, cada expresión coincide con el patrón de descarte.</span><span class="sxs-lookup"><span data-stu-id="2c549-149">In other words, every expression matches the discard pattern.</span></span>

<span data-ttu-id="2c549-150">Un patrón de descarte no se puede usar como el patrón de una *is_pattern_expression*.</span><span class="sxs-lookup"><span data-stu-id="2c549-150">A discard pattern may not be used as the pattern of an *is_pattern_expression*.</span></span>

#### <a name="positional-pattern"></a><span data-ttu-id="2c549-151">Patrón posicional</span><span class="sxs-lookup"><span data-stu-id="2c549-151">Positional Pattern</span></span>

<span data-ttu-id="2c549-152">Un patrón posicional comprueba que el valor de entrada no se `null`, invoca un método `Deconstruct` adecuado y realiza una mayor coincidencia de patrones en los valores resultantes.</span><span class="sxs-lookup"><span data-stu-id="2c549-152">A positional pattern checks that the input value is not `null`, invokes an appropriate `Deconstruct` method, and performs further pattern matching on the resulting values.</span></span>  <span data-ttu-id="2c549-153">También admite una sintaxis de modelo de tipo tupla (sin el tipo que se proporciona) cuando el tipo del valor de entrada es el mismo que el tipo que contiene `Deconstruct`, o si el tipo del valor de entrada es un tipo de tupla, o si el tipo del valor de entrada es `object` o `ITuple` y el tipo en tiempo de ejecución de la expresión implementa `ITuple`.</span><span class="sxs-lookup"><span data-stu-id="2c549-153">It also supports a tuple-like pattern syntax (without the type being provided) when the type of the input value is the same as the type containing `Deconstruct`, or if the type of the input value is a tuple type, or if the type of the input value is `object` or `ITuple` and the runtime type of the expression implements `ITuple`.</span></span>

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

<span data-ttu-id="2c549-154">Si se omite el *tipo* , se toma como el tipo estático del valor de entrada.</span><span class="sxs-lookup"><span data-stu-id="2c549-154">If the *type* is omitted, we take it to be the static type of the input value.</span></span>

<span data-ttu-id="2c549-155">Dada una coincidencia de un valor de entrada con el *tipo* de patrón `(` *subpattern_list* `)`, se selecciona un método buscando en el *tipo* para las declaraciones accesibles de `Deconstruct` y seleccionando uno entre ellos con las mismas reglas que para la declaración de desconstrucción.</span><span class="sxs-lookup"><span data-stu-id="2c549-155">Given a match of an input value to the pattern *type* `(` *subpattern_list* `)`, a method is selected by searching in *type* for accessible declarations of `Deconstruct` and selecting one among them using the same rules as for the deconstruction declaration.</span></span>

<span data-ttu-id="2c549-156">Es un error si una *positional_pattern* omite el tipo, tiene un único *subpatrón* sin un *identificador*, no tiene *property_subpattern* y no tiene ningún *simple_designation*.</span><span class="sxs-lookup"><span data-stu-id="2c549-156">It is an error if a *positional_pattern* omits the type, has a single *subpattern* without an *identifier*, has no *property_subpattern* and has no *simple_designation*.</span></span> <span data-ttu-id="2c549-157">Esta anulación de una *constant_pattern* entre paréntesis y una *positional_pattern*.</span><span class="sxs-lookup"><span data-stu-id="2c549-157">This disambiguates between a *constant_pattern* that is parenthesized and a *positional_pattern*.</span></span>

<span data-ttu-id="2c549-158">Para extraer los valores para que coincidan con los patrones de la lista,</span><span class="sxs-lookup"><span data-stu-id="2c549-158">In order to extract the values to match against the patterns in the list,</span></span>
- <span data-ttu-id="2c549-159">Si se omite el *tipo* y el tipo del valor de entrada es un tipo de tupla, el número de subpatrones debe ser el mismo que la cardinalidad de la tupla.</span><span class="sxs-lookup"><span data-stu-id="2c549-159">If *type* was omitted and the input value's type is a tuple type, then the number of subpatterns is required to be the same as the cardinality of the tuple.</span></span> <span data-ttu-id="2c549-160">Cada elemento de tupla coincide con el *subpatrón*correspondiente y la coincidencia se realiza correctamente si todos ellos se realizan correctamente.</span><span class="sxs-lookup"><span data-stu-id="2c549-160">Each tuple element is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="2c549-161">Si algún *subpatrón* tiene un *identificador*, debe denominar un elemento de tupla en la posición correspondiente en el tipo de tupla.</span><span class="sxs-lookup"><span data-stu-id="2c549-161">If any *subpattern* has an *identifier*, then that must name a tuple element at the corresponding position in the tuple type.</span></span>
- <span data-ttu-id="2c549-162">De lo contrario, si un `Deconstruct` adecuado existe como miembro de *tipo*, es un error en tiempo de compilación si el tipo del valor de entrada no es compatible con el *patrón* con el *tipo*.</span><span class="sxs-lookup"><span data-stu-id="2c549-162">Otherwise, if a suitable `Deconstruct` exists as a member of *type*, it is a compile-time error if the type of the input value is not *pattern-compatible* with *type*.</span></span> <span data-ttu-id="2c549-163">En tiempo de ejecución, el valor de entrada se prueba con el *tipo*.</span><span class="sxs-lookup"><span data-stu-id="2c549-163">At runtime the input value is tested against *type*.</span></span> <span data-ttu-id="2c549-164">Si se produce un error, se produce un error en la coincidencia del patrón posicional.</span><span class="sxs-lookup"><span data-stu-id="2c549-164">If this fails then the positional pattern match fails.</span></span> <span data-ttu-id="2c549-165">Si se realiza correctamente, el valor de entrada se convierte a este tipo y `Deconstruct` se invoca con variables nuevas generadas por el compilador para recibir los parámetros de `out`.</span><span class="sxs-lookup"><span data-stu-id="2c549-165">If it succeeds,  the input value is converted to this type and `Deconstruct` is invoked with fresh compiler-generated variables to receive the `out` parameters.</span></span> <span data-ttu-id="2c549-166">Cada valor recibido se compara con el *subpatrón*correspondiente y la coincidencia se realiza correctamente si todos ellos se realizan correctamente.</span><span class="sxs-lookup"><span data-stu-id="2c549-166">Each value that was received is matched against the corresponding *subpattern*, and the match succeeds if all of these succeed.</span></span> <span data-ttu-id="2c549-167">Si algún *subpatrón* tiene un *identificador*, debe asignar un nombre a un parámetro en la posición correspondiente de `Deconstruct`.</span><span class="sxs-lookup"><span data-stu-id="2c549-167">If any *subpattern* has an *identifier*, then that must name a parameter at the corresponding position of `Deconstruct`.</span></span>
- <span data-ttu-id="2c549-168">De lo contrario, si se omitió el *tipo* y el valor de entrada es de tipo `object` o `ITuple` o algún tipo que se pueda convertir a `ITuple` por una conversión de referencia implícita, y no aparece ningún *identificador* entre los subpatróns, coincidirá con `ITuple`.</span><span class="sxs-lookup"><span data-stu-id="2c549-168">Otherwise if *type* was omitted, and the input value is of type `object` or `ITuple` or some type that can be converted to `ITuple` by an implicit reference conversion, and no *identifier* appears among the subpatterns, then we match using `ITuple`.</span></span>
- <span data-ttu-id="2c549-169">De lo contrario, el patrón es un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="2c549-169">Otherwise the pattern is a compile-time error.</span></span>

<span data-ttu-id="2c549-170">El orden en el que se buscan coincidencias con los subpatróns en tiempo de ejecución no se especifica y es posible que no se intente coincidir con todos los subpatróns.</span><span class="sxs-lookup"><span data-stu-id="2c549-170">The order in which subpatterns are matched at runtime is unspecified, and a failed match may not attempt to match all subpatterns.</span></span>

##### <a name="example"></a><span data-ttu-id="2c549-171">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="2c549-171">Example</span></span>

<span data-ttu-id="2c549-172">En este ejemplo se usan muchas de las características descritas en esta especificación.</span><span class="sxs-lookup"><span data-stu-id="2c549-172">This example uses many of the features described in this specification</span></span>

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a><span data-ttu-id="2c549-173">Patrón de propiedad</span><span class="sxs-lookup"><span data-stu-id="2c549-173">Property Pattern</span></span>

<span data-ttu-id="2c549-174">Un patrón de propiedades comprueba que el valor de entrada no es `null` y coincide de forma recursiva con los valores extraídos por el uso de propiedades o campos accesibles.</span><span class="sxs-lookup"><span data-stu-id="2c549-174">A property pattern checks that the input value is not `null` and recursively matches values extracted by the use of accessible properties or fields.</span></span>

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

<span data-ttu-id="2c549-175">Es un error si algún _subpatrón_ de una _property_pattern_ no contiene un _identificador_ (debe ser de la segunda forma, que tiene un _identificador_).</span><span class="sxs-lookup"><span data-stu-id="2c549-175">It is an error if any _subpattern_ of a _property_pattern_ does not contain an _identifier_ (it must be of the second form, which has an _identifier_).</span></span>  <span data-ttu-id="2c549-176">Una coma final después del último subpatrón es opcional.</span><span class="sxs-lookup"><span data-stu-id="2c549-176">A trailing comma after the last subpattern is optional.</span></span>

<span data-ttu-id="2c549-177">Tenga en cuenta que un patrón de comprobación de valores NULL cae de un patrón de propiedad trivial.</span><span class="sxs-lookup"><span data-stu-id="2c549-177">Note that a null-checking pattern falls out of a trivial property pattern.</span></span> <span data-ttu-id="2c549-178">Para comprobar si el `s` de la cadena no es null, puede escribir cualquiera de las siguientes formas:</span><span class="sxs-lookup"><span data-stu-id="2c549-178">To check if the string `s` is non-null, you can write any of the following forms</span></span>

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

<span data-ttu-id="2c549-179">Dada una coincidencia de una expresión *e* con el *tipo* de patrón `{` *property_pattern_list* `}`, se trata de un error en tiempo de compilación si la expresión *e* no es compatible con el *patrón* con el tipo *t* designado por el *tipo*.</span><span class="sxs-lookup"><span data-stu-id="2c549-179">Given a match of an expression *e* to the pattern *type* `{` *property_pattern_list* `}`, it is a compile-time error if the expression *e* is not *pattern-compatible* with the type *T* designated by *type*.</span></span> <span data-ttu-id="2c549-180">Si el tipo está ausente, se toma como el tipo estático de *e*.</span><span class="sxs-lookup"><span data-stu-id="2c549-180">If the type is absent, we take it to be the static type of *e*.</span></span> <span data-ttu-id="2c549-181">Si el *identificador* está presente, declara una variable de patrón de tipo *Type*.</span><span class="sxs-lookup"><span data-stu-id="2c549-181">If the *identifier* is present, it declares a pattern variable of type *type*.</span></span> <span data-ttu-id="2c549-182">Cada uno de los identificadores que aparecen en el lado izquierdo de su *property_pattern_list* debe designar una propiedad o campo legible accesible de *T*. Si el *simple_designation* del *property_pattern* está presente, define una variable de patrón de tipo *T*.</span><span class="sxs-lookup"><span data-stu-id="2c549-182">Each of the identifiers appearing on the left-hand-side of its *property_pattern_list* must designate an accessible readable property or field of *T*. If the *simple_designation* of the *property_pattern* is present, it defines a pattern variable of type *T*.</span></span>

<span data-ttu-id="2c549-183">En tiempo de ejecución, la expresión se prueba en *T*. Si se produce un error, se produce un error en la coincidencia del patrón de propiedad y el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="2c549-183">At runtime, the expression is tested against *T*. If this fails then the property pattern match fails and the result is `false`.</span></span> <span data-ttu-id="2c549-184">Si se realiza correctamente, cada *property_subpattern* campo o propiedad se lee y su valor coincide con su patrón correspondiente.</span><span class="sxs-lookup"><span data-stu-id="2c549-184">If it succeeds, then each *property_subpattern* field or property is read and its value matched against its corresponding pattern.</span></span> <span data-ttu-id="2c549-185">El resultado de la coincidencia completa es `false` solo si el resultado de cualquiera de ellos es `false`.</span><span class="sxs-lookup"><span data-stu-id="2c549-185">The result of the whole match is `false` only if the result of any of these is `false`.</span></span> <span data-ttu-id="2c549-186">No se especifica el orden en el que coinciden los subpatrones y es posible que no coincidan todos los subpatróns en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="2c549-186">The order in which subpatterns are matched is not specified, and a failed match may not match all subpatterns at runtime.</span></span> <span data-ttu-id="2c549-187">Si la coincidencia se realiza correctamente y el *simple_designation* del *property_pattern* es un *single_variable_designation*, define una variable de tipo *T* que tiene asignado el valor coincidente.</span><span class="sxs-lookup"><span data-stu-id="2c549-187">If the match succeeds and the *simple_designation* of the *property_pattern* is a *single_variable_designation*, it defines a variable of type *T* that is assigned the matched value.</span></span>

> <span data-ttu-id="2c549-188">Nota: el patrón de propiedad se puede usar para la coincidencia de patrones con tipos anónimos.</span><span class="sxs-lookup"><span data-stu-id="2c549-188">Note: The property pattern can be used to pattern-match with anonymous types.</span></span>

##### <a name="example"></a><span data-ttu-id="2c549-189">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="2c549-189">Example</span></span>

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a><span data-ttu-id="2c549-190">Cambiar expresión</span><span class="sxs-lookup"><span data-stu-id="2c549-190">Switch Expression</span></span>

<span data-ttu-id="2c549-191">Se agrega un *switch_expression* para admitir la semántica de tipo `switch`para un contexto de expresión.</span><span class="sxs-lookup"><span data-stu-id="2c549-191">A *switch_expression* is added to support `switch`-like semantics for an expression context.</span></span>

<span data-ttu-id="2c549-192">La C# sintaxis del lenguaje se amplía con las siguientes producciones sintácticas:</span><span class="sxs-lookup"><span data-stu-id="2c549-192">The C# language syntax is augmented with the following syntactic productions:</span></span>

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

<span data-ttu-id="2c549-193">No se permite el *switch_expression* como *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="2c549-193">The *switch_expression* is not permitted as an *expression_statement*.</span></span>

> <span data-ttu-id="2c549-194">Estamos examinando este aspecto en una revisión futura.</span><span class="sxs-lookup"><span data-stu-id="2c549-194">We are looking at relaxing this in a future revision.</span></span>

<span data-ttu-id="2c549-195">El tipo del *switch_expression* es el [*mejor tipo común*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) de las expresiones que aparecen a la derecha de los tokens de `=>` de la *switch_expression_arm*s si este tipo existe y la expresión en cada brazo de la expresión switch se puede convertir implícitamente a ese tipo.</span><span class="sxs-lookup"><span data-stu-id="2c549-195">The type of the *switch_expression* is the [*best common type*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) of the expressions appearing to the right of the `=>` tokens of the *switch_expression_arm*s if such a type exists and the expression in every arm of the switch expression can be implicitly converted to that type.</span></span>  <span data-ttu-id="2c549-196">Además, se agrega una nueva *conversión de expresión switch*, que es una conversión implícita predefinida de una expresión switch a cada tipo `T` para el que existe una conversión implícita de cada expresión de arm en `T`.</span><span class="sxs-lookup"><span data-stu-id="2c549-196">In addition, we add a new *switch expression conversion*, which is a predefined implicit conversion from a switch expression to every type `T` for which there exists an implicit conversion from each arm's expression to `T`.</span></span>

<span data-ttu-id="2c549-197">Es un error si algún patrón de *switch_expression_arm*no puede afectar al resultado porque algún patrón y una protección anteriores siempre coincidirán.</span><span class="sxs-lookup"><span data-stu-id="2c549-197">It is an error if some *switch_expression_arm*'s pattern cannot affect the result because some previous pattern and guard will always match.</span></span>

<span data-ttu-id="2c549-198">Se dice que una expresión switch es *exhaustiva* si algún brazo de la expresión switch controla cada valor de su entrada.</span><span class="sxs-lookup"><span data-stu-id="2c549-198">A switch expression is said to be *exhaustive* if some arm of the switch expression handles every value of its input.</span></span>  <span data-ttu-id="2c549-199">El compilador generará una advertencia si una expresión switch no es *exhaustiva*.</span><span class="sxs-lookup"><span data-stu-id="2c549-199">The compiler shall produce a warning if a switch expression is not *exhaustive*.</span></span>

<span data-ttu-id="2c549-200">En tiempo de ejecución, el resultado de la *switch_expression* es el valor de la *expresión* de la primera *switch_expression_arm* para la que la expresión del lado izquierdo del *switch_expression* coincide con el patrón del *switch_expression_arm*y para el que el *case_guard* de la *switch_expression_arm*, si está presente, se evalúa como `true`.</span><span class="sxs-lookup"><span data-stu-id="2c549-200">At runtime, the result of the *switch_expression* is the value of the *expression* of the first *switch_expression_arm* for which the expression on the left-hand-side of the *switch_expression* matches the *switch_expression_arm*'s pattern, and for which the *case_guard* of the *switch_expression_arm*, if present, evaluates to `true`.</span></span> <span data-ttu-id="2c549-201">Si no existe tal *switch_expression_arm*, el *switch_expression* inicia una instancia de la excepción `System.Runtime.CompilerServices.SwitchExpressionException`.</span><span class="sxs-lookup"><span data-stu-id="2c549-201">If there is no such *switch_expression_arm*, the *switch_expression* throws an instance of the exception `System.Runtime.CompilerServices.SwitchExpressionException`.</span></span>

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a><span data-ttu-id="2c549-202">Paréntesis opcionales al cambiar un literal de tupla</span><span class="sxs-lookup"><span data-stu-id="2c549-202">Optional parens when switching on a tuple literal</span></span>

<span data-ttu-id="2c549-203">Para cambiar a un literal de tupla mediante el *switch_statement*, tiene que escribir lo que parece ser un paréntesis redundante</span><span class="sxs-lookup"><span data-stu-id="2c549-203">In order to switch on a tuple literal using the *switch_statement*, you have to write what appear to be redundant parens</span></span>

```csharp
switch ((a, b))
{
```

<span data-ttu-id="2c549-204">Para permitir</span><span class="sxs-lookup"><span data-stu-id="2c549-204">To permit</span></span>

```csharp
switch (a, b)
{
```

<span data-ttu-id="2c549-205">los paréntesis de la instrucción switch son opcionales cuando la expresión que se está cambiando es un literal de tupla.</span><span class="sxs-lookup"><span data-stu-id="2c549-205">the parentheses of the switch statement are optional when the expression being switched on is a tuple literal.</span></span>

### <a name="order-of-evaluation-in-pattern-matching"></a><span data-ttu-id="2c549-206">Orden de evaluación en coincidencia de patrones</span><span class="sxs-lookup"><span data-stu-id="2c549-206">Order of evaluation in pattern-matching</span></span>

<span data-ttu-id="2c549-207">Ofrecer la flexibilidad del compilador al reordenar las operaciones ejecutadas durante la coincidencia de patrones puede permitir la flexibilidad que se puede utilizar para mejorar la eficacia de la coincidencia de patrones.</span><span class="sxs-lookup"><span data-stu-id="2c549-207">Giving the compiler flexibility in reordering the operations executed during pattern-matching can permit flexibility that can be used to improve the efficiency of pattern-matching.</span></span> <span data-ttu-id="2c549-208">El requisito (no exigido) sería que las propiedades a las que se tiene acceso en un patrón y los métodos de deconstrucción deben ser "puras" (sin efecto secundario, idempotente, etc.).</span><span class="sxs-lookup"><span data-stu-id="2c549-208">The (unenforced) requirement would be that properties accessed in a pattern, and the Deconstruct methods, are required to be "pure" (side-effect free, idempotent, etc).</span></span> <span data-ttu-id="2c549-209">Eso no significa que se agregue la pureza como concepto de lenguaje, solo que se permitiría la flexibilidad del compilador en las operaciones de reordenación.</span><span class="sxs-lookup"><span data-stu-id="2c549-209">That doesn't mean that we would add purity as a language concept, only that we would allow the compiler flexibility in reordering operations.</span></span>

<span data-ttu-id="2c549-210">**Resolución 2018-04-04 LDM**: confirmada: el compilador tiene permiso para reordenar las llamadas a `Deconstruct`, accesos de propiedad e invocaciones de métodos en `ITuple`, y puede suponer que los valores devueltos son los mismos desde varias llamadas.</span><span class="sxs-lookup"><span data-stu-id="2c549-210">**Resolution 2018-04-04 LDM**: confirmed: the compiler is permitted to reorder calls to `Deconstruct`, property accesses, and invocations of methods in `ITuple`, and may assume that returned values are the same from multiple calls.</span></span> <span data-ttu-id="2c549-211">El compilador no debe invocar las funciones que no pueden afectar al resultado, por lo que tendremos mucho cuidado antes de realizar cambios en el orden generado por el compilador de la evaluación en el futuro.</span><span class="sxs-lookup"><span data-stu-id="2c549-211">The compiler should not invoke functions that cannot affect the result, and we will be very careful before making any changes to the compiler-generated order of evaluation in the future.</span></span>

### <a name="some-possible-optimizations"></a><span data-ttu-id="2c549-212">Algunas optimizaciones posibles</span><span class="sxs-lookup"><span data-stu-id="2c549-212">Some Possible Optimizations</span></span>

<span data-ttu-id="2c549-213">La compilación de la coincidencia de patrones puede aprovechar las partes comunes de los patrones.</span><span class="sxs-lookup"><span data-stu-id="2c549-213">The compilation of pattern matching can take advantage of common parts of patterns.</span></span> <span data-ttu-id="2c549-214">Por ejemplo, si la prueba de tipo de nivel superior de dos patrones sucesivos de un *switch_statement* es del mismo tipo, el código generado puede omitir la prueba de tipo para el segundo patrón.</span><span class="sxs-lookup"><span data-stu-id="2c549-214">For example, if the top-level type test of two successive patterns in a *switch_statement* is the same type, the generated code can skip the type test for the second pattern.</span></span>

<span data-ttu-id="2c549-215">Cuando algunos de los patrones son enteros o cadenas, el compilador puede generar el mismo tipo de código que genera para una instrucción switch en versiones anteriores del lenguaje.</span><span class="sxs-lookup"><span data-stu-id="2c549-215">When some of the patterns are integers or strings, the compiler can generate the same kind of code it generates for a switch-statement in earlier versions of the language.</span></span>

<span data-ttu-id="2c549-216">Para obtener más información sobre estos tipos de optimizaciones, vea [[Scott and Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "¿Cuándo se importa la heurística de compilaciones de coincidencia?").</span><span class="sxs-lookup"><span data-stu-id="2c549-216">For more on these kinds of optimizations, see [[Scott and Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "When Do Match-Compilation Heuristics Matter?").</span></span>
