---
ms.openlocfilehash: 8f9551b9e7f70379836c23a60f0d37dc02f8e18e
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488812"
---
# <a name="statements"></a><span data-ttu-id="6780e-101">Instrucciones</span><span class="sxs-lookup"><span data-stu-id="6780e-101">Statements</span></span>

<span data-ttu-id="6780e-102">C# proporciona una serie de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-102">C# provides a variety of statements.</span></span> <span data-ttu-id="6780e-103">La mayoría de estas instrucciones le resultarán familiar a los desarrolladores que se han programado en C y C++.</span><span class="sxs-lookup"><span data-stu-id="6780e-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

<span data-ttu-id="6780e-104">El *embedded_statement* no terminal se usa para las instrucciones que aparecen dentro de otras instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="6780e-105">El uso de *embedded_statement* lugar *instrucción* excluye el uso de instrucciones de declaración e instrucciones con etiquetas en estos contextos.</span><span class="sxs-lookup"><span data-stu-id="6780e-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="6780e-106">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="6780e-107">se produce un error en tiempo de compilación porque un `if` instrucción requiere un *embedded_statement* en lugar de un *instrucción* para Sí si rama.</span><span class="sxs-lookup"><span data-stu-id="6780e-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="6780e-108">Si se permite este código, a continuación, la variable `i` se declararía, pero nunca se puede usar.</span><span class="sxs-lookup"><span data-stu-id="6780e-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="6780e-109">Sin embargo, tenga en cuenta que al colocar `i`de declaración de un bloque, el ejemplo es válido.</span><span class="sxs-lookup"><span data-stu-id="6780e-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="6780e-110">Puntos finales y alcance</span><span class="sxs-lookup"><span data-stu-id="6780e-110">End points and reachability</span></span>

<span data-ttu-id="6780e-111">Cada instrucción tiene un ***punto final***.</span><span class="sxs-lookup"><span data-stu-id="6780e-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="6780e-112">En términos intuitivos, el punto final de una instrucción es la ubicación que sigue inmediatamente a la instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="6780e-113">Las reglas de ejecución de instrucciones compuestas (instrucciones que contienen instrucciones incrustadas) especifican la acción que se realiza cuando el control alcanza el punto final de una instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="6780e-114">Por ejemplo, cuando el control alcanza el punto final de una instrucción en un bloque, el control se transfiere a la siguiente instrucción del bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="6780e-115">Si una instrucción, posiblemente, se puede acceder mediante la ejecución, la instrucción se dice que ***accesible***.</span><span class="sxs-lookup"><span data-stu-id="6780e-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="6780e-116">Por el contrario, si no hay ninguna posibilidad de que se ejecuta una instrucción, la instrucción se dice que ***inaccesible***.</span><span class="sxs-lookup"><span data-stu-id="6780e-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="6780e-117">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="6780e-118">la segunda invocación de `Console.WriteLine` no es accesible porque no hay ninguna posibilidad de que se ejecutará la instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="6780e-119">Se notifica una advertencia si el compilador determina que una instrucción es inaccesible.</span><span class="sxs-lookup"><span data-stu-id="6780e-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="6780e-120">Es específicamente no es un error de una instrucción sea inaccesible.</span><span class="sxs-lookup"><span data-stu-id="6780e-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="6780e-121">Para determinar que si una determinada instrucción o el punto de conexión está accesible, el compilador realiza análisis de flujo según las reglas de disponibilidad definidas para cada instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="6780e-122">El análisis de flujo tiene en cuenta los valores de las expresiones constantes ([expresiones constantes](expressions.md#constant-expressions)) que controlan el comportamiento de instrucciones, pero no se consideran los posibles valores de expresiones no constantes.</span><span class="sxs-lookup"><span data-stu-id="6780e-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="6780e-123">En otras palabras, para fines de análisis de flujo de control, una expresión no constante de un tipo determinado se considera que tiene cualquier valor posible de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="6780e-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="6780e-124">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="6780e-125">la expresión booleana de la `if` instrucción es una expresión constante porque ambos operandos de la `==` operador son constantes.</span><span class="sxs-lookup"><span data-stu-id="6780e-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="6780e-126">La expresión constante se evalúa en tiempo de compilación, que genera el valor `false`, el `Console.WriteLine` invocación se considera inaccesible.</span><span class="sxs-lookup"><span data-stu-id="6780e-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="6780e-127">Sin embargo, si `i` se cambia para ser una variable local</span><span class="sxs-lookup"><span data-stu-id="6780e-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="6780e-128">el `Console.WriteLine` llamada se considera alcanzable, aunque en realidad, nunca se ejecutará.</span><span class="sxs-lookup"><span data-stu-id="6780e-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="6780e-129">El *bloque* de una función miembro siempre se considera alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="6780e-130">Evaluando sucesivamente las reglas de disponibilidad de cada instrucción en un bloque, se puede determinar el alcance de cualquier instrucción determinada.</span><span class="sxs-lookup"><span data-stu-id="6780e-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="6780e-131">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="6780e-132">la accesibilidad de la segunda `Console.WriteLine` se determina como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="6780e-133">La primera `Console.WriteLine` instrucción de expresión es accesible porque el bloque de la `F` método es accesible.</span><span class="sxs-lookup"><span data-stu-id="6780e-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="6780e-134">El punto final de la primera `Console.WriteLine` instrucción de expresión es accesible porque esa instrucción es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="6780e-135">El `if` instrucción es alcanzable porque el punto final de la primera `Console.WriteLine` expresión instrucción es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="6780e-136">El segundo `Console.WriteLine` instrucción de expresión es accesible porque la expresión booleana de la `if` instrucción no tiene el valor constante `false`.</span><span class="sxs-lookup"><span data-stu-id="6780e-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="6780e-137">Hay dos situaciones en las que es un error en tiempo de compilación para el punto final de una instrucción sea accesible:</span><span class="sxs-lookup"><span data-stu-id="6780e-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="6780e-138">Dado que el `switch` instrucción no permite una sección switch "pasen" a la siguiente sección, es un error en tiempo de compilación para el punto final de la lista de instrucciones de una sección switch sea alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="6780e-139">Si se produce este error, normalmente es una indicación de que un `break` falta la instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="6780e-140">Es un error en tiempo de compilación para el punto final del bloque de un miembro de función que calcula un valor para que sea accesible.</span><span class="sxs-lookup"><span data-stu-id="6780e-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="6780e-141">Si se produce este error, normalmente es un valor que indica que un `return` falta la instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="6780e-142">Bloques</span><span class="sxs-lookup"><span data-stu-id="6780e-142">Blocks</span></span>

<span data-ttu-id="6780e-143">Un *bloque* permite que se escriban varias instrucciones en contextos donde se permite una única instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="6780e-144">Un *bloque* consta de un elemento opcional *statement_list* ([instrucción enumera](statements.md#statement-lists)), encerradas entre llaves.</span><span class="sxs-lookup"><span data-stu-id="6780e-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="6780e-145">Si se omite la lista de instrucciones, se dice que el bloque de estar vacío.</span><span class="sxs-lookup"><span data-stu-id="6780e-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="6780e-146">Un bloque puede contener instrucciones de declaración ([las instrucciones de declaración](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="6780e-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="6780e-147">El ámbito de una variable o constante local declarada en un bloque es el bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="6780e-148">Un bloque se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="6780e-149">Si el bloque está vacío, el control se transfiere al punto final del bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="6780e-150">Si el bloque no está vacío, el control se transfiere a la lista de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="6780e-151">Cuando el control alcanza el punto final de la lista de instrucciones, se transfiere al punto final del bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="6780e-152">La lista de instrucciones de un bloque es alcanzable si el propio bloque es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="6780e-153">El punto final de un bloque es accesible si el bloque está vacío o si el punto final de la lista de instrucciones es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="6780e-154">Un *bloque* que contiene uno o varios `yield` instrucciones ([la instrucción yield](statements.md#the-yield-statement)) se llama a un bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="6780e-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="6780e-155">Bloques del iterador se usan para implementar miembros de función como iteradores ([iteradores](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="6780e-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="6780e-156">Algunas restricciones adicionales se aplican a los bloques del iterador:</span><span class="sxs-lookup"><span data-stu-id="6780e-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="6780e-157">Es un error en tiempo de compilación para un `return` instrucción que aparezca en un bloque de iteradores (pero `yield return` se permiten instrucciones).</span><span class="sxs-lookup"><span data-stu-id="6780e-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="6780e-158">Es un error en tiempo de compilación para un bloque de iteradores para que contenga un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="6780e-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="6780e-159">Un bloque de iteradores siempre define un contexto seguro, incluso cuando su declaración está anidada en un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="6780e-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="6780e-160">Listas de instrucciones</span><span class="sxs-lookup"><span data-stu-id="6780e-160">Statement lists</span></span>

<span data-ttu-id="6780e-161">Un ***lista de instrucciones*** consta de uno o más instrucciones escritas en secuencia.</span><span class="sxs-lookup"><span data-stu-id="6780e-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="6780e-162">Las listas de instrucciones que se producen en *bloque*s ([bloques](statements.md#blocks)) y en *switch_block*s ([la instrucción switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="6780e-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="6780e-163">Transfiere el control a la primera instrucción ejecuta una lista de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="6780e-164">Cuando el control alcanza el punto final de una instrucción, se transfiere a la siguiente instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="6780e-165">Cuando el control alcanza el punto final de la última instrucción, se transfiere al punto final de la lista de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="6780e-166">Una instrucción en una lista de instrucciones es alcanzable si se cumple al menos uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="6780e-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="6780e-167">La instrucción es la primera instrucción y la propia lista de la instrucción es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="6780e-168">El punto final de la instrucción anterior es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="6780e-169">La instrucción es una instrucción con etiqueta y la etiqueta se hace referencia mediante un accesible `goto` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="6780e-170">El punto final de una lista de instrucciones es accesible si el punto final de la última instrucción en la lista es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="6780e-171">Instrucción vacía</span><span class="sxs-lookup"><span data-stu-id="6780e-171">The empty statement</span></span>

<span data-ttu-id="6780e-172">Un *empty_statement* no hace nada.</span><span class="sxs-lookup"><span data-stu-id="6780e-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="6780e-173">Cuando hay ninguna operación para realizar en un contexto donde se requiere una instrucción, se usa una instrucción vacía.</span><span class="sxs-lookup"><span data-stu-id="6780e-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="6780e-174">Ejecución de una instrucción vacía simplemente transfiere el control al punto final de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="6780e-175">Por lo tanto, el punto final de una instrucción vacía es alcanzable si la instrucción vacía es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="6780e-176">Se puede usar una instrucción vacía al escribir un `while` instrucción con un cuerpo null:</span><span class="sxs-lookup"><span data-stu-id="6780e-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="6780e-177">Además, se puede usar una instrucción vacía para declarar una etiqueta justo antes de cerrar "`}`" de un bloque:</span><span class="sxs-lookup"><span data-stu-id="6780e-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="6780e-178">Instrucciones con etiqueta</span><span class="sxs-lookup"><span data-stu-id="6780e-178">Labeled statements</span></span>

<span data-ttu-id="6780e-179">Un *labeled_statement* permite ir precedida por una etiqueta de una instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="6780e-180">Labeled (instrucciones) se permiten en bloques, pero no están permitidas como instrucciones incrustadas.</span><span class="sxs-lookup"><span data-stu-id="6780e-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="6780e-181">Una instrucción con etiqueta declara una etiqueta con el nombre proporcionado por el *identificador*.</span><span class="sxs-lookup"><span data-stu-id="6780e-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="6780e-182">El ámbito de una etiqueta es el bloque completo en el que se declara, incluyendo los bloques anidados.</span><span class="sxs-lookup"><span data-stu-id="6780e-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="6780e-183">Es un error en tiempo de compilación para las dos etiquetas con el mismo nombre para tener ámbitos que se superponen.</span><span class="sxs-lookup"><span data-stu-id="6780e-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="6780e-184">Se puede hacer referencia a una etiqueta de `goto` instrucciones ([la instrucción goto](statements.md#the-goto-statement)) dentro del ámbito de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6780e-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="6780e-185">Esto significa que `goto` instrucciones pueden transferir el control dentro y fuera de los bloques, pero nunca en bloques.</span><span class="sxs-lookup"><span data-stu-id="6780e-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="6780e-186">Las etiquetas tienen su propio espacio de declaración y no interfieren con otros identificadores.</span><span class="sxs-lookup"><span data-stu-id="6780e-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="6780e-187">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="6780e-188">es válido y utiliza el nombre `x` como un parámetro y una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6780e-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="6780e-189">Ejecución de una instrucción con etiqueta corresponde exactamente con la ejecución de la instrucción siguiente a la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6780e-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="6780e-190">Además de la disponibilidad proporcionado por el flujo de control normal, una instrucción con etiqueta estará accesible si se hace referencia a la etiqueta por un accesible `goto` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="6780e-191">(Excepción: Si un `goto` instrucción está dentro de un `try` que incluye un `finally` bloque y la instrucción con etiqueta está fuera de la `try`y el punto final de la `finally` bloque es inaccesible, entonces no es accesible desde la instrucción con etiqueta que `goto` instrucción.)</span><span class="sxs-lookup"><span data-stu-id="6780e-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="6780e-192">Instrucciones de declaración</span><span class="sxs-lookup"><span data-stu-id="6780e-192">Declaration statements</span></span>

<span data-ttu-id="6780e-193">Un *declaration_statement* declara una variable local o una constante.</span><span class="sxs-lookup"><span data-stu-id="6780e-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="6780e-194">Las instrucciones de declaración se permiten en bloques, pero no están permitidas como instrucciones incrustadas.</span><span class="sxs-lookup"><span data-stu-id="6780e-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="6780e-195">Declaraciones de variable locales</span><span class="sxs-lookup"><span data-stu-id="6780e-195">Local variable declarations</span></span>

<span data-ttu-id="6780e-196">Un *local_variable_declaration* declara uno o más variables locales.</span><span class="sxs-lookup"><span data-stu-id="6780e-196">A *local_variable_declaration* declares one or more local variables.</span></span>

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

<span data-ttu-id="6780e-197">El *local_variable_type* de un *local_variable_declaration* directa especifica el tipo de las variables que aparecen en la declaración, o indica con el identificador `var` que el tipo debe deducirse basándose en un inicializador.</span><span class="sxs-lookup"><span data-stu-id="6780e-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="6780e-198">El tipo es seguido por una lista de *local_variable_declarator*s, cada uno de los cuales incluye una nueva variable.</span><span class="sxs-lookup"><span data-stu-id="6780e-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="6780e-199">Un *local_variable_declarator* consta de un *identificador* que los nombres de la variable, seguida opcionalmente por una "`=`" token y un *local_variable_initializer* que proporciona el valor inicial de la variable.</span><span class="sxs-lookup"><span data-stu-id="6780e-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="6780e-200">En el contexto de una declaración de variable local, el identificador var actúa como una palabra clave contextual ([palabras clave](lexical-structure.md#keywords)). Cuando el *local_variable_type* se especifica como `var` y ningún tipo denominado `var` está en el ámbito, la declaración es un ***declaración de variable local con tipo implícito***, cuyo tipo es deduce el tipo de la expresión de inicializador asociado.</span><span class="sxs-lookup"><span data-stu-id="6780e-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="6780e-201">Las declaraciones de variable locales tipificadas implícitamente son las siguientes restricciones:</span><span class="sxs-lookup"><span data-stu-id="6780e-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="6780e-202">El *local_variable_declaration* no se puede incluir varios *local_variable_declarator*s.</span><span class="sxs-lookup"><span data-stu-id="6780e-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="6780e-203">El *local_variable_declarator* debe incluir un *local_variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="6780e-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="6780e-204">El *local_variable_initializer* debe ser un *expresión*.</span><span class="sxs-lookup"><span data-stu-id="6780e-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="6780e-205">El inicializador *expresión* debe tener un tipo de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="6780e-206">El inicializador *expresión* no puede hacer referencia a la variable declarada en el propio</span><span class="sxs-lookup"><span data-stu-id="6780e-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="6780e-207">Los siguientes son ejemplos de declaraciones de variable local tipificadas implícitamente incorrectos:</span><span class="sxs-lookup"><span data-stu-id="6780e-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="6780e-208">Se obtiene el valor de una variable local en una expresión que utiliza un *simple_name* ([nombres simples](expressions.md#simple-names)), y se modifica el valor de una variable local con un *asignación* () [Operadores de asignación](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="6780e-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="6780e-209">Una variable local debe estar previamente asignada ([asignación definitiva](variables.md#definite-assignment)) en cada ubicación donde se obtiene su valor.</span><span class="sxs-lookup"><span data-stu-id="6780e-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="6780e-210">El ámbito de una variable local declarada en un *local_variable_declaration* es el bloque en que se produce la declaración.</span><span class="sxs-lookup"><span data-stu-id="6780e-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="6780e-211">Es un error para hacer referencia a una variable local en una posición textual que precede a la *local_variable_declarator* de la variable local.</span><span class="sxs-lookup"><span data-stu-id="6780e-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="6780e-212">Dentro del ámbito de una variable local, es un error en tiempo de compilación para declarar otra variable o constante con el mismo nombre local.</span><span class="sxs-lookup"><span data-stu-id="6780e-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="6780e-213">Una declaración de variable local que declara varias variables equivale a varias declaraciones de variables únicas con el mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="6780e-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="6780e-214">Además, un inicializador de variable en una declaración de variable local se corresponde exactamente a una instrucción de asignación que se inserta inmediatamente después de la declaración.</span><span class="sxs-lookup"><span data-stu-id="6780e-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="6780e-215">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="6780e-216">corresponde exactamente a</span><span class="sxs-lookup"><span data-stu-id="6780e-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="6780e-217">En una declaración de variable local tipificada implícitamente, se toma el tipo de la variable local que se declara para ser el mismo que el tipo de la expresión utilizada para inicializar la variable.</span><span class="sxs-lookup"><span data-stu-id="6780e-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="6780e-218">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6780e-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="6780e-219">El implícito declaraciones de variable local anteriores son exactamente equivalentes a las siguientes declaraciones con tipo explícito:</span><span class="sxs-lookup"><span data-stu-id="6780e-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="6780e-220">Declaraciones de constante local</span><span class="sxs-lookup"><span data-stu-id="6780e-220">Local constant declarations</span></span>

<span data-ttu-id="6780e-221">Un *local_constant_declaration* declara uno o más constantes locales.</span><span class="sxs-lookup"><span data-stu-id="6780e-221">A *local_constant_declaration* declares one or more local constants.</span></span>

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

<span data-ttu-id="6780e-222">El *tipo* de un *local_constant_declaration* especifica el tipo de las constantes que se incluyen en la declaración.</span><span class="sxs-lookup"><span data-stu-id="6780e-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="6780e-223">El tipo es seguido por una lista de *constant_declarator*s, cada uno de los cuales incluye una nueva constante.</span><span class="sxs-lookup"><span data-stu-id="6780e-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="6780e-224">Un *constant_declarator* consta de un *identificador* que nombres de la constante, seguido por un "`=`" símbolo (token), seguido de un *constant_expression* ([ Expresiones constantes](expressions.md#constant-expressions)) que proporciona el valor de la constante.</span><span class="sxs-lookup"><span data-stu-id="6780e-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="6780e-225">El *tipo* y *constant_expression* de una declaración de constante local deben seguir las mismas reglas que los de una declaración de miembro de constante ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="6780e-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="6780e-226">Se obtiene el valor de una constante local en una expresión que utiliza un *simple_name* ([nombres simples](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="6780e-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="6780e-227">El ámbito de una constante local es el bloque en que se produce la declaración.</span><span class="sxs-lookup"><span data-stu-id="6780e-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="6780e-228">Es un error para hacer referencia a una constante local en una posición textual que precede a su *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="6780e-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="6780e-229">Dentro del ámbito de una constante local, es un error en tiempo de compilación para declarar otra variable o constante con el mismo nombre local.</span><span class="sxs-lookup"><span data-stu-id="6780e-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="6780e-230">Una declaración de constante local que declara varias constantes equivale a varias declaraciones de una sola constante con el mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="6780e-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="6780e-231">Instrucciones de expresión</span><span class="sxs-lookup"><span data-stu-id="6780e-231">Expression statements</span></span>

<span data-ttu-id="6780e-232">Un *expression_statement* evalúa una expresión determinada.</span><span class="sxs-lookup"><span data-stu-id="6780e-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="6780e-233">El valor calculado por la expresión, si existe, se descarta.</span><span class="sxs-lookup"><span data-stu-id="6780e-233">The value computed by the expression, if any, is discarded.</span></span>

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

<span data-ttu-id="6780e-234">No todas las expresiones pueden ser instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="6780e-235">En concreto, expresiones como `x + y` y `x == 1` que simplemente calcular un valor (que se descartarán), no se permiten que las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="6780e-236">Ejecución de un *expression_statement* evalúa la expresión contenida y, a continuación, transfiere el control al punto final de la *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="6780e-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="6780e-237">El punto final de un *expression_statement* estará accesible si ese *expression_statement* es accesible.</span><span class="sxs-lookup"><span data-stu-id="6780e-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="6780e-238">Instrucciones de selección</span><span class="sxs-lookup"><span data-stu-id="6780e-238">Selection statements</span></span>

<span data-ttu-id="6780e-239">Instrucciones de selección seleccione uno de una serie de instrucciones posibles para la ejecución en función del valor de alguna expresión.</span><span class="sxs-lookup"><span data-stu-id="6780e-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="6780e-240">If instrucción</span><span class="sxs-lookup"><span data-stu-id="6780e-240">The if statement</span></span>

<span data-ttu-id="6780e-241">El `if` instrucción selecciona una instrucción para ejecutarla en función del valor de una expresión booleana.</span><span class="sxs-lookup"><span data-stu-id="6780e-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="6780e-242">Un `else` parte está asociada con la instrucción anterior más cercana `if` permitido por la sintaxis.</span><span class="sxs-lookup"><span data-stu-id="6780e-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="6780e-243">Por lo tanto, un `if` instrucción del formulario</span><span class="sxs-lookup"><span data-stu-id="6780e-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="6780e-244">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="6780e-244">is equivalent to</span></span>
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

<span data-ttu-id="6780e-245">Un `if` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-246">El *boolean_expression* ([expresiones booleanas](expressions.md#boolean-expressions)) se evalúa.</span><span class="sxs-lookup"><span data-stu-id="6780e-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="6780e-247">Si la expresión booleana devuelve `true`, el control se transfiere a la primera instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="6780e-248">Cuando el control alcanza el punto final de dicha instrucción, se transfiere al punto final de la `if` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="6780e-249">Si la expresión booleana devuelve `false` y si un `else` parte está presente, el control se transfiere a la segunda instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="6780e-250">Cuando el control alcanza el punto final de dicha instrucción, se transfiere al punto final de la `if` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="6780e-251">Si la expresión booleana devuelve `false` y si un `else` parte no está presente, el control se transfiere al punto final de la `if` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="6780e-252">La primera instrucción de incrustada una `if` instrucción es alcanzable si el `if` instrucción es alcanzable y la expresión booleana no tiene el valor constante `false`.</span><span class="sxs-lookup"><span data-stu-id="6780e-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="6780e-253">La segunda instrucción de incrustada una `if` instrucción, si está presente, es accesible si el `if` instrucción es alcanzable y la expresión booleana no tiene el valor constante `true`.</span><span class="sxs-lookup"><span data-stu-id="6780e-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="6780e-254">El punto final de un `if` instrucción es alcanzable si el punto final de al menos uno de sus instrucciones incrustadas es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="6780e-255">Además, el punto final de un `if` instrucción sin ningún `else` parte es accesible si el `if` instrucción es alcanzable y la expresión booleana no tiene el valor constante `true`.</span><span class="sxs-lookup"><span data-stu-id="6780e-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="6780e-256">La instrucción switch</span><span class="sxs-lookup"><span data-stu-id="6780e-256">The switch statement</span></span>

<span data-ttu-id="6780e-257">La instrucción switch, selecciona para la ejecución de una lista de instrucciones con una etiqueta de conmutador asociado que se corresponde con el valor de la expresión switch.</span><span class="sxs-lookup"><span data-stu-id="6780e-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

<span data-ttu-id="6780e-258">Un *switch_statement* consta de la palabra clave `switch`, seguido de una expresión entre paréntesis (denominada la expresión switch), seguido por un *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="6780e-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="6780e-259">El *switch_block* consta de cero o más *switch_section*, encerradas entre llaves.</span><span class="sxs-lookup"><span data-stu-id="6780e-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="6780e-260">Cada *switch_section* consta de uno o varios *switch_label*s seguido por un *statement_list* ([instrucción enumera](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="6780e-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="6780e-261">El ***que rigen tipo*** de un `switch` instrucción se establece mediante la expresión switch.</span><span class="sxs-lookup"><span data-stu-id="6780e-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="6780e-262">Si el tipo de la expresión switch es `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, o un *enum_type*, o si es el tipo que acepta valores NULL correspondiente a uno de estos tipos, que es el que rigen el tipo de la `switch` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="6780e-263">En caso contrario, exactamente uno definido por el usuario conversión implícita ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)) debe existir en el tipo de la expresión switch a uno de los posibles tipos aplicables: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, o bien, un tipo que acepta valores NULL correspondiente a uno de esos tipos.</span><span class="sxs-lookup"><span data-stu-id="6780e-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="6780e-264">En caso contrario, si no existe una conversión implícita, o si existe más de una conversión implícita, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="6780e-265">La expresión constante de cada `case` etiqueta debe indicar un valor que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo aplicable el `switch` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="6780e-266">Se produce un error de tiempo de compilación si dos o más `case` etiquetas en el mismo `switch` instrucción especificar el mismo valor constante.</span><span class="sxs-lookup"><span data-stu-id="6780e-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="6780e-267">Puede haber a lo sumo un `default` etiqueta en una instrucción switch.</span><span class="sxs-lookup"><span data-stu-id="6780e-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="6780e-268">Un `switch` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-269">La expresión switch se evalúa y se convierte al tipo aplicable.</span><span class="sxs-lookup"><span data-stu-id="6780e-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="6780e-270">Si no especifica una de las constantes en un `case` etiqueta en la misma `switch` instrucción es igual al valor de la expresión switch, el control se transfiere a la lista de la instrucción siguiente correspondiente `case` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6780e-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="6780e-271">Si ninguna de las constantes especificadas en `case` etiquetas en el mismo `switch` instrucción es igual al valor de la expresión switch y si un `default` etiqueta está presente, el control se transfiere a la instrucción que sigue lista la `default` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6780e-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="6780e-272">Si ninguna de las constantes especificadas en `case` etiquetas en el mismo `switch` instrucción es igual al valor de la expresión switch y si no hay ningún `default` etiqueta está presente, el control se transfiere al punto final de la `switch` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="6780e-273">Si el punto final de la lista de instrucciones de una sección switch es accesible, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="6780e-274">Esto se conoce como la regla "sin paso explícito".</span><span class="sxs-lookup"><span data-stu-id="6780e-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="6780e-275">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-275">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
<span data-ttu-id="6780e-276">es válido porque no hay ninguna sección switch tiene un punto de conexión alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="6780e-277">A diferencia de C y C++, la ejecución de una sección switch no permite el "paso explícito" a la siguiente sección switch y el ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
<span data-ttu-id="6780e-278">genera un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-278">results in a compile-time error.</span></span> <span data-ttu-id="6780e-279">Cuando la ejecución de una sección switch es seguido de ejecución de otra sección switch, explícita `goto case` o `goto default` se debe usar la instrucción:</span><span class="sxs-lookup"><span data-stu-id="6780e-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

<span data-ttu-id="6780e-280">Se permiten varias etiquetas en un *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="6780e-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="6780e-281">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-281">The example</span></span>
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
<span data-ttu-id="6780e-282">es válido.</span><span class="sxs-lookup"><span data-stu-id="6780e-282">is valid.</span></span> <span data-ttu-id="6780e-283">El ejemplo no infringir la regla "sin paso explícito" porque las etiquetas `case 2:` y `default:` forman parte del mismo *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="6780e-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="6780e-284">La regla "sin paso explícito" evita que una clase común de errores que se producen en C y C++ cuando `break` accidentalmente se omiten las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="6780e-285">Además, debido a esta regla, las secciones switch de una `switch` instrucción se puede reorganizar arbitrariamente sin afectar al comportamiento de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="6780e-286">Por ejemplo, las secciones de la `switch` se puede invertir la instrucción anterior sin afectar al comportamiento de la instrucción:</span><span class="sxs-lookup"><span data-stu-id="6780e-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

<span data-ttu-id="6780e-287">La lista de instrucciones de una sección switch normalmente termina en un `break`, `goto case`, o `goto default` se permite la instrucción, pero ninguna construcción que representa el punto final de la lista de instrucciones inaccesible.</span><span class="sxs-lookup"><span data-stu-id="6780e-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="6780e-288">Por ejemplo, un `while` instrucción controlada por la expresión booleana `true` se sabe que nunca alcance su punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="6780e-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="6780e-289">Del mismo modo, un `throw` o `return` instrucción siempre transfiere el control en otro lugar y nunca llega a su punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="6780e-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="6780e-290">Por lo tanto, el ejemplo siguiente es válido:</span><span class="sxs-lookup"><span data-stu-id="6780e-290">Thus, the following example is valid:</span></span>
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

<span data-ttu-id="6780e-291">El tipo aplicable una `switch` instrucción puede ser del tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="6780e-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="6780e-292">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6780e-292">For example:</span></span>
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

<span data-ttu-id="6780e-293">Al igual que los operadores de igualdad de cadenas ([operadores de igualdad de cadenas](expressions.md#string-equality-operators)), el `switch` instrucción distingue mayúsculas de minúsculas y ejecutará una determinada sección sólo si la cadena de expresión switch coincide exactamente con un `case` etiqueta constante.</span><span class="sxs-lookup"><span data-stu-id="6780e-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="6780e-294">Cuando el control de tipo de un `switch` instrucción es `string`, el valor `null` se permite como una constante de etiqueta case.</span><span class="sxs-lookup"><span data-stu-id="6780e-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="6780e-295">El *statement_list*s de una *switch_block* pueden contener instrucciones de declaración ([las instrucciones de declaración](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="6780e-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="6780e-296">El ámbito de una variable o constante local declarada en un bloque switch es el bloque switch.</span><span class="sxs-lookup"><span data-stu-id="6780e-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="6780e-297">La lista de instrucciones de una sección switch determinada es accesible si el `switch` instrucción es alcanzable y al menos uno de los siguientes es true:</span><span class="sxs-lookup"><span data-stu-id="6780e-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="6780e-298">La expresión switch es un valor no constante.</span><span class="sxs-lookup"><span data-stu-id="6780e-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="6780e-299">La expresión switch es un valor constante que coincide con un `case` etiqueta en la sección switch.</span><span class="sxs-lookup"><span data-stu-id="6780e-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="6780e-300">La expresión switch es un valor constante que no coincide con ninguna `case` etiqueta y la sección switch contiene la `default` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6780e-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="6780e-301">Se hace referencia a una etiqueta de switch de la sección switch mediante una accesible `goto case` o `goto default` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="6780e-302">El punto final de un `switch` instrucción es alcanzable si se cumple al menos uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="6780e-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="6780e-303">El `switch` instrucción contiene una accesible `break` instrucción que se cierra el `switch` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="6780e-304">El `switch` instrucción es alcanzable, la expresión switch es un valor no constante y no `default` etiqueta está presente.</span><span class="sxs-lookup"><span data-stu-id="6780e-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="6780e-305">El `switch` instrucción es alcanzable, la expresión switch es un valor constante que no coincide con ninguna `case` etiqueta y ningún `default` etiqueta está presente.</span><span class="sxs-lookup"><span data-stu-id="6780e-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="6780e-306">Instrucciones de iteración</span><span class="sxs-lookup"><span data-stu-id="6780e-306">Iteration statements</span></span>

<span data-ttu-id="6780e-307">Instrucciones de iteración repetidamente ejecutan una instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="6780e-308">La instrucción while</span><span class="sxs-lookup"><span data-stu-id="6780e-308">The while statement</span></span>

<span data-ttu-id="6780e-309">El `while` instrucción condicionalmente ejecuta una instrucción incrustada cero o más veces.</span><span class="sxs-lookup"><span data-stu-id="6780e-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="6780e-310">Un `while` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-311">El *boolean_expression* ([expresiones booleanas](expressions.md#boolean-expressions)) se evalúa.</span><span class="sxs-lookup"><span data-stu-id="6780e-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="6780e-312">Si la expresión booleana devuelve `true`, el control se transfiere a la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="6780e-313">Cuando y si el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de un `continue` instrucción), el control se transfiere al principio de la `while` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="6780e-314">Si la expresión booleana devuelve `false`, el control se transfiere al punto final de la `while` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="6780e-315">Dentro de la instrucción incrustada de un `while` (instrucción), un `break` instrucción ([la instrucción break](statements.md#the-break-statement)) puede utilizarse para transferir el control al punto final de la `while` instrucción (terminando así la iteración de los datos incrustados instrucción) y un `continue` instrucción ([la instrucción continue](statements.md#the-continue-statement)) puede utilizarse para transferir el control al punto final de la instrucción incrustada (otra iteración de esta forma se realizará la `while` instrucción).</span><span class="sxs-lookup"><span data-stu-id="6780e-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="6780e-316">La instrucción incrustada de un `while` instrucción es alcanzable si el `while` instrucción es alcanzable y la expresión booleana no tiene el valor constante `false`.</span><span class="sxs-lookup"><span data-stu-id="6780e-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="6780e-317">El punto final de un `while` instrucción es alcanzable si se cumple al menos uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="6780e-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="6780e-318">El `while` instrucción contiene una accesible `break` instrucción que se cierra el `while` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="6780e-319">El `while` instrucción es alcanzable y la expresión booleana no tiene el valor constante `true`.</span><span class="sxs-lookup"><span data-stu-id="6780e-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="6780e-320">La instrucción do</span><span class="sxs-lookup"><span data-stu-id="6780e-320">The do statement</span></span>

<span data-ttu-id="6780e-321">El `do` instrucción condicionalmente ejecuta una instrucción incrustada una o varias veces.</span><span class="sxs-lookup"><span data-stu-id="6780e-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="6780e-322">Un `do` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-323">El control se transfiere a la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="6780e-324">Cuando y si el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de un `continue` instrucción), el *boolean_expression* ([expresiones booleanas](expressions.md#boolean-expressions)) se evalúa.</span><span class="sxs-lookup"><span data-stu-id="6780e-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="6780e-325">Si la expresión booleana devuelve `true`, el control se transfiere al principio de la `do` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="6780e-326">En caso contrario, el control se transfiere al punto final de la `do` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="6780e-327">Dentro de la instrucción incrustada de un `do` (instrucción), un `break` instrucción ([la instrucción break](statements.md#the-break-statement)) puede utilizarse para transferir el control al punto final de la `do` instrucción (terminando así la iteración de los datos incrustados instrucción) y un `continue` instrucción ([la instrucción continue](statements.md#the-continue-statement)) puede utilizarse para transferir el control al punto final de la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="6780e-328">La instrucción incrustada de un `do` instrucción es alcanzable si el `do` instrucción es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="6780e-329">El punto final de un `do` instrucción es alcanzable si se cumple al menos uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="6780e-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="6780e-330">El `do` instrucción contiene una accesible `break` instrucción que se cierra el `do` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="6780e-331">El punto final de la instrucción incrustada es alcanzable y la expresión booleana no tiene el valor constante `true`.</span><span class="sxs-lookup"><span data-stu-id="6780e-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="6780e-332">La instrucción for</span><span class="sxs-lookup"><span data-stu-id="6780e-332">The for statement</span></span>

<span data-ttu-id="6780e-333">El `for` instrucción se evalúa como una secuencia de expresiones de inicialización y, a continuación, mientras una condición es true, repetidamente ejecuta una instrucción incrustada y se evalúa como una secuencia de expresiones de iteración.</span><span class="sxs-lookup"><span data-stu-id="6780e-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

<span data-ttu-id="6780e-334">El *for_initializer*, si está presente, consta de un *local_variable_declaration* ([declaraciones de variable Local](statements.md#local-variable-declarations)) o una lista de *statement_ expresión*s ([las instrucciones de expresión](statements.md#expression-statements)) separados por comas.</span><span class="sxs-lookup"><span data-stu-id="6780e-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="6780e-335">El ámbito de una variable local declarada por un *for_initializer* comienza en el *local_variable_declarator* para la variable y se extiende hasta el final de la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="6780e-336">El ámbito incluye la *for_condition* y *for_iterator*.</span><span class="sxs-lookup"><span data-stu-id="6780e-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="6780e-337">El *for_condition*, si está presente, debe ser un *boolean_expression* ([expresiones booleanas](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="6780e-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="6780e-338">El *for_iterator*, si está presente, consta de una lista de *statement_expression*s ([las instrucciones de expresión](statements.md#expression-statements)) separados por comas.</span><span class="sxs-lookup"><span data-stu-id="6780e-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="6780e-339">Una instrucción for se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-340">Si un *for_initializer* existe, los inicializadores de variables o expresiones de instrucción se ejecutan en el orden en que se escriben.</span><span class="sxs-lookup"><span data-stu-id="6780e-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="6780e-341">Este paso se realiza solo una vez.</span><span class="sxs-lookup"><span data-stu-id="6780e-341">This step is only performed once.</span></span>
*  <span data-ttu-id="6780e-342">Si un *for_condition* está presente, se evalúa.</span><span class="sxs-lookup"><span data-stu-id="6780e-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="6780e-343">Si el *for_condition* no está presente o si se produce la evaluación `true`, el control se transfiere a la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="6780e-344">Cuando y si el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de un `continue` instrucción), las expresiones de la *for_iterator*, si existe, se evalúa en secuencia y, a continuación, otra iteración es realiza empezando por la evaluación de la *for_condition* en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="6780e-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="6780e-345">Si el *for_condition* está presente, la evaluación produce `false`, el control se transfiere al punto final de la `for` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="6780e-346">Dentro de la instrucción incrustada de un `for` (instrucción), un `break` instrucción ([la instrucción break](statements.md#the-break-statement)) puede utilizarse para transferir el control al punto final de la `for` instrucción (terminando así la iteración de los datos incrustados instrucción) y un `continue` instrucción ([la instrucción continue](statements.md#the-continue-statement)) puede utilizarse para transferir el control al punto final de la instrucción incrustada (ejecutándolos el *for_iterator* y realizar otra iteración de la `for` instrucción, empezando por el *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="6780e-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="6780e-347">La instrucción incrustada de un `for` instrucción es alcanzable si se cumple una de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="6780e-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="6780e-348">El `for` instrucción es alcanzable y no *for_condition* está presente.</span><span class="sxs-lookup"><span data-stu-id="6780e-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="6780e-349">El `for` instrucción es alcanzable y un *for_condition* está presente y no tiene el valor constante `false`.</span><span class="sxs-lookup"><span data-stu-id="6780e-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="6780e-350">El punto final de un `for` instrucción es alcanzable si se cumple al menos uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="6780e-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="6780e-351">El `for` instrucción contiene una accesible `break` instrucción que se cierra el `for` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="6780e-352">El `for` instrucción es alcanzable y un *for_condition* está presente y no tiene el valor constante `true`.</span><span class="sxs-lookup"><span data-stu-id="6780e-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="6780e-353">La instrucción foreach</span><span class="sxs-lookup"><span data-stu-id="6780e-353">The foreach statement</span></span>

<span data-ttu-id="6780e-354">El `foreach` instrucción enumera los elementos de una colección y ejecuta una instrucción incrustada para cada elemento de la colección.</span><span class="sxs-lookup"><span data-stu-id="6780e-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="6780e-355">El *tipo* y *identificador* de un `foreach` instrucción declara el ***variable de iteración*** de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="6780e-356">Si el `var` identificador se expresa como el *local_variable_type*y ningún tipo denominado `var` está en el ámbito, la variable de iteración se dice que un ***variable de iteración implícito***, y su tipo se considera que es el tipo de elemento de la `foreach` instrucción, como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="6780e-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="6780e-357">La variable de iteración corresponde a una variable local de solo lectura con un ámbito que se extiende a través de la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="6780e-358">Durante la ejecución de un `foreach` instrucción, la variable de iteración representa el elemento de colección para el que se está realizando una iteración.</span><span class="sxs-lookup"><span data-stu-id="6780e-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="6780e-359">Se produce un error de tiempo de compilación si la instrucción incrustada intenta modificar la variable de iteración (a través de la asignación o el `++` y `--` operadores) o pasar la variable de iteración como un `ref` o `out` parámetro.</span><span class="sxs-lookup"><span data-stu-id="6780e-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="6780e-360">A continuación, para mayor brevedad, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` y `IEnumerator<T>` hacen referencia a los tipos correspondientes en los espacios de nombres `System.Collections` y `System.Collections.Generic`.</span><span class="sxs-lookup"><span data-stu-id="6780e-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="6780e-361">El procesamiento en tiempo de compilación de una instrucción foreach se determina en primer lugar el ***tipo de colección***, ***tipo de enumerador*** y ***tipo de elemento*** de la expresión.</span><span class="sxs-lookup"><span data-stu-id="6780e-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="6780e-362">Esta determinación se realiza como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="6780e-363">Si el tipo `X` de *expresión* es un tipo de matriz, no hay una conversión implícita de referencia de `X` a la `IEnumerable` interfaz (puesto que `System.Array` implementa esta interfaz).</span><span class="sxs-lookup"><span data-stu-id="6780e-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="6780e-364">El ***tipo de colección*** es el `IEnumerable` interfaz, el ***tipo de enumerador*** es el `IEnumerator` interfaz y la ***tipo de elemento*** es el tipo de elemento de la tipo de matriz `X`.</span><span class="sxs-lookup"><span data-stu-id="6780e-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="6780e-365">Si el tipo `X` de *expresión* es `dynamic` , a continuación, hay una conversión implícita de *expresión* a la `IEnumerable` interfaz ([dinámicos implícita las conversiones](conversions.md#implicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="6780e-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="6780e-366">El ***tipo de colección*** es el `IEnumerable` interfaz y la ***tipo de enumerador*** es el `IEnumerator` interfaz.</span><span class="sxs-lookup"><span data-stu-id="6780e-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="6780e-367">Si el `var` identificador se expresa como el *local_variable_type* el ***tipo de elemento*** es `dynamic`, de lo contrario es `object`.</span><span class="sxs-lookup"><span data-stu-id="6780e-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="6780e-368">En caso contrario, determinar si el tipo `X` tiene un adecuado `GetEnumerator` método:</span><span class="sxs-lookup"><span data-stu-id="6780e-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="6780e-369">Realizar la búsqueda de miembros en el tipo `X` con el identificador de `GetEnumerator` y sin argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="6780e-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="6780e-370">Como se describe a continuación, compruebe si la búsqueda de miembros no produce una coincidencia, o que produce una ambigüedad, o produzca a una coincidencia que no es un grupo de métodos, para una interfaz enumerable.</span><span class="sxs-lookup"><span data-stu-id="6780e-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="6780e-371">Se recomienda que se emite una advertencia si la búsqueda de miembros genera nada excepto un grupo de métodos o ninguna coincidencia.</span><span class="sxs-lookup"><span data-stu-id="6780e-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="6780e-372">Realizar la resolución de sobrecarga con el grupo de métodos resultantes y una lista de argumentos vacía.</span><span class="sxs-lookup"><span data-stu-id="6780e-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="6780e-373">Si la resolución de sobrecarga tiene como resultado ningún método aplicable, da como resultado una ambigüedad o da como resultado un solo método mejor, pero ese método es estático o no público, busque una interfaz enumerable, como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="6780e-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="6780e-374">Se recomienda que se emite una advertencia si la resolución de sobrecarga genera nada excepto un método de instancia pública ambiguo o no hay métodos aplicables.</span><span class="sxs-lookup"><span data-stu-id="6780e-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="6780e-375">Si el tipo de devolución `E` de la `GetEnumerator` método no es una clase, struct o tipo de interfaz, un error se genera y no se toma ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="6780e-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="6780e-376">Se realiza la búsqueda de miembros en `E` con el identificador `Current` y sin argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="6780e-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="6780e-377">Si la búsqueda de miembros no produce a ninguna coincidencia, el resultado es un error o el resultado es salvo para una propiedad de instancia pública que permite lectura, se produce un error y no se toma ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="6780e-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="6780e-378">Se realiza la búsqueda de miembros en `E` con el identificador `MoveNext` y sin argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="6780e-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="6780e-379">Si la búsqueda de miembros no produce a ninguna coincidencia, el resultado es un error o el resultado es salvo para un grupo de métodos, se produce un error y no se toma ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="6780e-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="6780e-380">Se realiza la resolución de sobrecarga en el grupo de métodos con una lista de argumentos vacía.</span><span class="sxs-lookup"><span data-stu-id="6780e-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="6780e-381">Si los resultados de la resolución de sobrecarga en ningún métodos aplicables, los resultados en una ambigüedad o los resultados en un único método mejor, pero ese método es estático o no público, o su tipo de valor devuelto no es `bool`, se genera un error y no se toma ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="6780e-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="6780e-382">El ***tipo de colección*** es `X`, el ***tipo de enumerador*** es `E`y el ***tipo de elemento*** es el tipo de la `Current` propiedad.</span><span class="sxs-lookup"><span data-stu-id="6780e-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="6780e-383">En caso contrario, busque una interfaz enumerable:</span><span class="sxs-lookup"><span data-stu-id="6780e-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="6780e-384">If entre todos los tipos de `Ti` para que no hay una conversión implícita de `X` a `IEnumerable<Ti>`, hay un único tipo `T` que `T` no `dynamic` y para todos los demás `Ti` hay una conversión implícita de `IEnumerable<T>` a `IEnumerable<Ti>`, el ***tipo de colección*** es la interfaz `IEnumerable<T>`, ***tipo de enumerador*** es la interfaz `IEnumerator<T>`y el ***tipo de elemento*** es `T`.</span><span class="sxs-lookup"><span data-stu-id="6780e-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="6780e-385">En caso contrario, si hay más de uno de estos tipos `T`, a continuación, se produce un error y no se toma ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="6780e-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="6780e-386">En caso contrario, si hay una conversión implícita de `X` a la `System.Collections.IEnumerable` interfaz, el ***tipo de colección*** es esta interfaz, el ***tipo de enumerador*** es la interfaz `System.Collections.IEnumerator`y el ***tipo de elemento*** es `object`.</span><span class="sxs-lookup"><span data-stu-id="6780e-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="6780e-387">En caso contrario, se produce un error y no se toma ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="6780e-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="6780e-388">Los pasos anteriores, si se realiza correctamente, sin ambigüedades generan un tipo de colección `C`, tipo de enumerador `E` y tipo de elemento `T`.</span><span class="sxs-lookup"><span data-stu-id="6780e-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="6780e-389">Una instrucción foreach del formulario</span><span class="sxs-lookup"><span data-stu-id="6780e-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="6780e-390">a continuación, se expande en:</span><span class="sxs-lookup"><span data-stu-id="6780e-390">is then expanded to:</span></span>
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

<span data-ttu-id="6780e-391">La variable `e` no es visible o accesible a la expresión `x` o la instrucción incrustada o cualquier otro código fuente del programa.</span><span class="sxs-lookup"><span data-stu-id="6780e-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="6780e-392">La variable `v` es de solo lectura en la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="6780e-393">Si no hay una conversión explícita ([las conversiones explícitas](conversions.md#explicit-conversions)) desde `T` (el tipo de elemento) para `V` (el *local_variable_type* en la instrucción foreach), se produce un error y no se realiza ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="6780e-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="6780e-394">Si `x` tiene el valor `null`, un `System.NullReferenceException` se produce en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="6780e-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="6780e-395">Se permite una implementación para implementar una determinada instrucción foreach de manera diferente, por ejemplo, por motivos de rendimiento, siempre y cuando el comportamiento es coherente con la expansión de la anterior.</span><span class="sxs-lookup"><span data-stu-id="6780e-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="6780e-396">La colocación de `v` dentro del tiempo es importante para cómo se capturan por cualquier función anónima que se producen en bucle la *embedded_statement*.</span><span class="sxs-lookup"><span data-stu-id="6780e-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="6780e-397">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6780e-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="6780e-398">Si `v` se ha declarado fuera while bucle, se pueden compartir entre todas las iteraciones y su valor después de la para bucle sería el valor final, `13`, que es lo que la invocación de `f` imprimía.</span><span class="sxs-lookup"><span data-stu-id="6780e-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="6780e-399">En su lugar, porque cada iteración tiene su propia variable `v`, la captura `f` en la primera iteración continuará que contenga el valor `7`, que es lo que se van a imprimir.</span><span class="sxs-lookup"><span data-stu-id="6780e-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="6780e-400">(Nota: las versiones anteriores de C# declaran `v` bucle exterior de while.)</span><span class="sxs-lookup"><span data-stu-id="6780e-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="6780e-401">El cuerpo de la, por último, se construye el bloque según los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="6780e-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="6780e-402">Si no hay una conversión implícita de `E` a la `System.IDisposable` interfaz, a continuación,</span><span class="sxs-lookup"><span data-stu-id="6780e-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="6780e-403">Si `E` es un tipo de valor distinto de null, la finalmente cláusula se expande en el equivalente semántico de:</span><span class="sxs-lookup"><span data-stu-id="6780e-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="6780e-404">De lo contrario el finalmente cláusula se expande en el equivalente semántico de:</span><span class="sxs-lookup"><span data-stu-id="6780e-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="6780e-405">salvo que si `E` es un tipo de valor o un parámetro de tipo que se crea una instancia de un tipo de valor y, después, la conversión de `e` a `System.IDisposable` no hará que la conversión boxing que se produzca.</span><span class="sxs-lookup"><span data-stu-id="6780e-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="6780e-406">De lo contrario, si `E` es un tipo sellado, la, por último, se expande la cláusula en un bloque vacío:</span><span class="sxs-lookup"><span data-stu-id="6780e-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="6780e-407">En caso contrario, el, por último, se expande la cláusula para:</span><span class="sxs-lookup"><span data-stu-id="6780e-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="6780e-408">La variable local `d` no es visible o accesible a cualquier código de usuario.</span><span class="sxs-lookup"><span data-stu-id="6780e-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="6780e-409">En concreto, no está en conflicto con cualquier otra variable cuyo ámbito incluye el bloque finally.</span><span class="sxs-lookup"><span data-stu-id="6780e-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="6780e-410">El orden en que `foreach` recorre los elementos de una matriz, es como sigue: Para los elementos de matrices unidimensionales se recorren en orden de índice ascendente, empezando por el índice `0` y terminando con el índice `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="6780e-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="6780e-411">Las matrices multidimensionales, se recorren los elementos que tanto los índices de la dimensión más a la derecha se incrementan en primer lugar, a continuación, en la siguiente dimensión izquierda, y así sucesivamente hacia la izquierda.</span><span class="sxs-lookup"><span data-stu-id="6780e-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="6780e-412">El ejemplo siguiente se imprime cada valor en una matriz bidimensional, en orden de los elementos:</span><span class="sxs-lookup"><span data-stu-id="6780e-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
<span data-ttu-id="6780e-413">El resultado es como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-413">The output produced is as follows:</span></span>
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="6780e-414">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="6780e-415">el tipo de `n` se infiere para ser `int`, el tipo de elemento de `numbers`.</span><span class="sxs-lookup"><span data-stu-id="6780e-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="6780e-416">Instrucciones de salto</span><span class="sxs-lookup"><span data-stu-id="6780e-416">Jump statements</span></span>

<span data-ttu-id="6780e-417">Instrucciones de salto incondicional transferir el control.</span><span class="sxs-lookup"><span data-stu-id="6780e-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="6780e-418">Se llama a la ubicación a la que una instrucción de salto transfiere el control del ***destino*** de la instrucción de salto.</span><span class="sxs-lookup"><span data-stu-id="6780e-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="6780e-419">Cuando se produce una instrucción de salto dentro de un bloque y el destino de esa instrucción de salto está fuera de ese bloque, se dice que la instrucción de salto a ***salir*** el bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="6780e-420">Mientras una instrucción de salto puede transferir el control fuera de un bloque, nunca puede transferir el control en un bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="6780e-421">Ejecución de instrucciones de salto se complica por la presencia de intermedia `try` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="6780e-422">En ausencia de estos `try` instrucciones, una instrucción de salto transfiere incondicionalmente el control de la instrucción de salto a su destino.</span><span class="sxs-lookup"><span data-stu-id="6780e-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="6780e-423">En presencia de este tipo intermedia `try` instrucciones, la ejecución es más compleja.</span><span class="sxs-lookup"><span data-stu-id="6780e-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="6780e-424">Si la instrucción de salto sale de uno o varios `try` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="6780e-425">Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="6780e-426">Este proceso se repite hasta que el `finally` bloques de todos los que intervienen `try` se han ejecutado las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="6780e-427">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-427">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
<span data-ttu-id="6780e-428">el `finally` bloques asociados con dos `try` las instrucciones se ejecutan antes de que el control se transfiere al destino de la instrucción de salto.</span><span class="sxs-lookup"><span data-stu-id="6780e-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="6780e-429">El resultado es como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-429">The output produced is as follows:</span></span>
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="6780e-430">Break (instrucción)</span><span class="sxs-lookup"><span data-stu-id="6780e-430">The break statement</span></span>

<span data-ttu-id="6780e-431">El `break` instrucción sale envolvente `switch`, `while`, `do`, `for`, o `foreach` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="6780e-432">El destino de un `break` instrucción es el punto final de envolvente `switch`, `while`, `do`, `for`, o `foreach` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="6780e-433">Si un `break` instrucción no se incluye un `switch`, `while`, `do`, `for`, o `foreach` instrucción, que se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="6780e-434">Cuando varios `switch`, `while`, `do`, `for`, o `foreach` instrucciones están anidadas dentro de otros, un `break` instrucción solo se aplica a la instrucción más interna.</span><span class="sxs-lookup"><span data-stu-id="6780e-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="6780e-435">Para transferir el control a través de varios niveles de anidamiento, un `goto` instrucción ([la instrucción goto](statements.md#the-goto-statement)) se debe usar.</span><span class="sxs-lookup"><span data-stu-id="6780e-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="6780e-436">Un `break` instrucción no puede salir una `finally` bloque ([la instrucción try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="6780e-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="6780e-437">Cuando un `break` instrucción se produce en un `finally` bloquea, el destino de la `break` instrucción debe ser dentro del mismo `finally` bloquear; en caso contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="6780e-438">Un `break` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-439">Si el `break` instrucción sale de uno o varios `try` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="6780e-440">Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="6780e-441">Este proceso se repite hasta que el `finally` bloques de todos los que intervienen `try` se han ejecutado las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="6780e-442">El control se transfiere al destino de la `break` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="6780e-443">Dado que un `break` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `break` instrucción nunca es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="6780e-444">La instrucción continue</span><span class="sxs-lookup"><span data-stu-id="6780e-444">The continue statement</span></span>

<span data-ttu-id="6780e-445">El `continue` instrucción inicia una nueva iteración de envolvente `while`, `do`, `for`, o `foreach` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="6780e-446">El destino de un `continue` instrucción es el punto final de la instrucción incrustada de envolvente `while`, `do`, `for`, o `foreach` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="6780e-447">Si un `continue` instrucción no se incluye un `while`, `do`, `for`, o `foreach` instrucción, que se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="6780e-448">Cuando varios `while`, `do`, `for`, o `foreach` instrucciones están anidadas dentro de otros, un `continue` instrucción solo se aplica a la instrucción más interna.</span><span class="sxs-lookup"><span data-stu-id="6780e-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="6780e-449">Para transferir el control a través de varios niveles de anidamiento, un `goto` instrucción ([la instrucción goto](statements.md#the-goto-statement)) se debe usar.</span><span class="sxs-lookup"><span data-stu-id="6780e-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="6780e-450">Un `continue` instrucción no puede salir una `finally` bloque ([la instrucción try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="6780e-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="6780e-451">Cuando un `continue` instrucción se produce en un `finally` bloquea, el destino de la `continue` instrucción debe ser dentro del mismo `finally` bloquear; de lo contrario se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="6780e-452">Un `continue` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-453">Si el `continue` instrucción sale de uno o varios `try` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="6780e-454">Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="6780e-455">Este proceso se repite hasta que el `finally` bloques de todos los que intervienen `try` se han ejecutado las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="6780e-456">El control se transfiere al destino de la `continue` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="6780e-457">Dado que un `continue` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `continue` instrucción nunca es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="6780e-458">La instrucción goto</span><span class="sxs-lookup"><span data-stu-id="6780e-458">The goto statement</span></span>

<span data-ttu-id="6780e-459">El `goto` instrucción transfiere el control a una instrucción que está marcada con una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6780e-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="6780e-460">El destino de un `goto` *identificador* es la instrucción con etiqueta con la etiqueta especificada.</span><span class="sxs-lookup"><span data-stu-id="6780e-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="6780e-461">Si una etiqueta con el nombre especificado no existe en el miembro de función actual, o si el `goto` instrucción no se está dentro del ámbito de la etiqueta, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="6780e-462">Esta regla permite el uso de un `goto` instrucción para transferir el control fuera de un ámbito anidado, pero no en un ámbito anidado.</span><span class="sxs-lookup"><span data-stu-id="6780e-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="6780e-463">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-463">In the example</span></span>
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
<span data-ttu-id="6780e-464">un `goto` instrucción se usa para transferir el control fuera de un ámbito anidado.</span><span class="sxs-lookup"><span data-stu-id="6780e-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="6780e-465">El destino de un `goto case` instrucción es la lista de instrucciones en la envolvente inmediato `switch` instrucción ([la instrucción switch](statements.md#the-switch-statement)), que contiene un `case` etiqueta con el valor constante especificado.</span><span class="sxs-lookup"><span data-stu-id="6780e-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="6780e-466">Si el `goto case` instrucción no se incluye un `switch` instrucción, si la *constant_expression* no es implícitamente convertible ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo aplicable de la más cercano envolvente `switch` instrucción, o si envolvente `switch` instrucción no contenga un `case` etiquetar con el valor constante especificado, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="6780e-467">El destino de un `goto default` instrucción es la lista de instrucciones en la envolvente inmediato `switch` instrucción ([la instrucción switch](statements.md#the-switch-statement)), que contiene un `default` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="6780e-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="6780e-468">Si el `goto default` instrucción no se incluye un `switch` instrucción, o si envolvente `switch` instrucción no contenga un `default` etiqueta, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="6780e-469">Un `goto` instrucción no puede salir una `finally` bloque ([la instrucción try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="6780e-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="6780e-470">Cuando un `goto` instrucción se produce en un `finally` bloquea, el destino de la `goto` instrucción debe ser dentro del mismo `finally` bloque, o en caso contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="6780e-471">Un `goto` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-472">Si el `goto` instrucción sale de uno o varios `try` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="6780e-473">Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="6780e-474">Este proceso se repite hasta que el `finally` bloques de todos los que intervienen `try` se han ejecutado las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="6780e-475">El control se transfiere al destino de la `goto` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="6780e-476">Dado que un `goto` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `goto` instrucción nunca es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="6780e-477">La instrucción return</span><span class="sxs-lookup"><span data-stu-id="6780e-477">The return statement</span></span>

<span data-ttu-id="6780e-478">El `return` instrucción devuelve el control al llamador actual de la función en el que el `return` aparece la instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="6780e-479">Un `return` instrucción sin una expresión puede utilizarse únicamente en un miembro de función que no calcula un valor, es decir, un método con el tipo de resultado ([cuerpo del método](classes.md#method-body)) `void`, el `set` descriptor de acceso de una propiedad o indizador, el `add` y `remove` descriptores de acceso de un evento, un constructor de instancia, un constructor estático o un destructor.</span><span class="sxs-lookup"><span data-stu-id="6780e-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="6780e-480">Un `return` instrucción con una expresión solo se puede usar en un miembro de función que calcula un valor, es decir, un método con un tipo de resultado distinto de void el `get` descriptor de acceso de una propiedad o indizador o un operador definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="6780e-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="6780e-481">Una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) debe existir en el tipo de la expresión para el tipo de valor devuelto del miembro de función que lo contiene.</span><span class="sxs-lookup"><span data-stu-id="6780e-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="6780e-482">Devolver instrucciones también se pueden utilizar en el cuerpo de expresiones de función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) y participar en determinar qué conversiones existen para esas funciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="6780e-483">Es un error en tiempo de compilación para un `return` instrucción que aparezca en un `finally` bloque ([la instrucción try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="6780e-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="6780e-484">Un `return` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-485">Si el `return` instrucción especifica una expresión, la expresión se evalúa y el valor resultante se convierte en el tipo de valor devuelto de la función que lo contiene mediante una conversión implícita.</span><span class="sxs-lookup"><span data-stu-id="6780e-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="6780e-486">El resultado de la conversión se convierte en el valor del resultado producido por la función.</span><span class="sxs-lookup"><span data-stu-id="6780e-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="6780e-487">Si el `return` instrucción está dentro de uno o varios `try` o `catch` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="6780e-488">Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="6780e-489">Este proceso se repite hasta que el `finally` bloques de inclusión de todos los `try` se han ejecutado las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="6780e-490">Si la función contenedora no es una función asincrónica, el control se devuelve al autor de llamada de la función contenedora junto con el valor del resultado, si existe.</span><span class="sxs-lookup"><span data-stu-id="6780e-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="6780e-491">Si la función contenedora es una función asincrónica, el control se devuelve al llamador actual y el valor del resultado, si los hay, se registra en la tarea devuelta como se describe en ([interfaces de enumerador](classes.md#enumerator-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="6780e-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="6780e-492">Dado que un `return` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `return` instrucción nunca es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="6780e-493">La instrucción throw</span><span class="sxs-lookup"><span data-stu-id="6780e-493">The throw statement</span></span>

<span data-ttu-id="6780e-494">El `throw` instrucción produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="6780e-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="6780e-495">Un `throw` instrucción con una expresión produce el valor generado al evaluar la expresión.</span><span class="sxs-lookup"><span data-stu-id="6780e-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="6780e-496">La expresión debe denotar un valor del tipo de clase `System.Exception`, de un tipo de clase que derive de `System.Exception` o de un tipo de parámetro de tipo que tiene `System.Exception` (o una subclase de ella) como su clase base efectiva.</span><span class="sxs-lookup"><span data-stu-id="6780e-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="6780e-497">Si la evaluación de la expresión genera `null`, un `System.NullReferenceException` se produce en su lugar.</span><span class="sxs-lookup"><span data-stu-id="6780e-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="6780e-498">Un `throw` instrucción sin una expresión puede utilizarse únicamente en un `catch` bloquear, en cuyo caso esa instrucción vuelve a inicia la excepción que se está controlando actualmente por el que `catch` bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="6780e-499">Dado que un `throw` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `throw` instrucción nunca es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="6780e-500">Cuando se produce una excepción, el control se transfiere a la primera `catch` cláusula en envolvente `try` instrucción que pueda controlar la excepción.</span><span class="sxs-lookup"><span data-stu-id="6780e-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="6780e-501">El proceso que tiene lugar desde el punto de la excepción producida en el punto de transferencia de control a un controlador de excepciones adecuado se conoce como ***propagación de excepciones***.</span><span class="sxs-lookup"><span data-stu-id="6780e-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="6780e-502">Propagación de una excepción consiste en evaluar repetidamente los pasos siguientes hasta que un `catch` cláusula que coincida con la excepción se encuentra.</span><span class="sxs-lookup"><span data-stu-id="6780e-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="6780e-503">En esta descripción, el ***throw punto*** inicialmente es la ubicación en la que se produce la excepción.</span><span class="sxs-lookup"><span data-stu-id="6780e-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="6780e-504">En el miembro de función actual, cada `try` se examina la instrucción que encierra el punto de inicio.</span><span class="sxs-lookup"><span data-stu-id="6780e-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="6780e-505">Para cada instrucción `S`, empezando por el más interno `try` instrucción y finaliza con el exterior `try` instrucción, se evalúan los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="6780e-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="6780e-506">Si el `try` block de `S` encierra el punto de inicio y S tiene uno o varios `catch` cláusulas, la `catch` cláusulas se examinan por orden de aparición para buscar un controlador adecuado para la excepción, según las reglas especificadas en Sección [la instrucción try](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="6780e-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="6780e-507">Si una coincidencia `catch` cláusula se encuentra, se completa la propagación de excepciones mediante la transferencia de control al bloque de que `catch` cláusula.</span><span class="sxs-lookup"><span data-stu-id="6780e-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="6780e-508">De lo contrario, si la `try` bloque o una `catch` block de `S` encierra el punto de inicio y si `S` tiene un `finally` bloque, el control se transfiere a la `finally` bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="6780e-509">Si el `finally` bloque inicia otra excepción, se termina el procesamiento de la excepción actual.</span><span class="sxs-lookup"><span data-stu-id="6780e-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="6780e-510">En caso contrario, cuando el control alcanza el punto final de la `finally` bloque, continúa el procesamiento de la excepción actual.</span><span class="sxs-lookup"><span data-stu-id="6780e-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="6780e-511">Si un controlador de excepciones no se encuentra en la invocación de función actual, finaliza la invocación de función y se produce uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="6780e-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="6780e-512">Si la función actual es que no es asincrónico, se repiten los pasos anteriores para que el llamador de la función con un punto de inicio correspondiente a la instrucción desde el que se invoca el miembro de función.</span><span class="sxs-lookup"><span data-stu-id="6780e-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="6780e-513">Si la función actual es asincrónico y devolución de tarea, la excepción se registra en la tarea devuelta, que se coloca en un estado cancelado o con errores, como se describe en [interfaces de enumerador](classes.md#enumerator-interfaces).</span><span class="sxs-lookup"><span data-stu-id="6780e-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="6780e-514">Si la función actual es asincrónico y devuelve void, el contexto de sincronización del subproceso actual se notifica como se describe en [interfaces enumerables](classes.md#enumerable-interfaces).</span><span class="sxs-lookup"><span data-stu-id="6780e-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="6780e-515">Si el procesamiento de la excepción finaliza todas las invocaciones de miembro de función en el subproceso actual, que indica que el subproceso no tiene ningún controlador para la excepción, a continuación, el subproceso está finaliza.</span><span class="sxs-lookup"><span data-stu-id="6780e-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="6780e-516">El impacto de cuando dicha terminación es definido por la implementación.</span><span class="sxs-lookup"><span data-stu-id="6780e-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="6780e-517">La instrucción try</span><span class="sxs-lookup"><span data-stu-id="6780e-517">The try statement</span></span>

<span data-ttu-id="6780e-518">El `try` instrucción proporciona un mecanismo para capturar las excepciones que se producen durante la ejecución de un bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="6780e-519">Además, el `try` instrucción proporciona la capacidad de especificar un bloque de código que siempre se ejecuta cuando el control abandona el `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

<span data-ttu-id="6780e-520">Hay tres formas posibles de `try` instrucciones:</span><span class="sxs-lookup"><span data-stu-id="6780e-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="6780e-521">Un `try` bloque seguido de uno o varios `catch` bloques.</span><span class="sxs-lookup"><span data-stu-id="6780e-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="6780e-522">Un `try` bloque seguido por un `finally` bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="6780e-523">Un `try` bloque seguido de uno o varios `catch` bloques seguido por un `finally` bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="6780e-524">Cuando un `catch` cláusula especifica un *exception_specifier*, el tipo debe ser `System.Exception`, un tipo que derive de `System.Exception` o un parámetro de tipo que tiene `System.Exception` (o una subclase de ella) como su efectivo clase base.</span><span class="sxs-lookup"><span data-stu-id="6780e-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="6780e-525">Cuando un `catch` cláusula especifica tanto un *exception_specifier* con un *identificador*, un ***variable de excepción*** del nombre especificado y el tipo se declara.</span><span class="sxs-lookup"><span data-stu-id="6780e-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="6780e-526">La variable de excepción corresponde a una variable local con un ámbito que se extiende a través de la `catch` cláusula.</span><span class="sxs-lookup"><span data-stu-id="6780e-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="6780e-527">Durante la ejecución de la *exception_filter* y *bloque*, la variable de excepción representa la excepción que se controla actualmente.</span><span class="sxs-lookup"><span data-stu-id="6780e-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="6780e-528">Para fines de comprobación de asignación definitiva, la variable de excepción se considera asignado definitivamente en todo su ámbito.</span><span class="sxs-lookup"><span data-stu-id="6780e-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="6780e-529">A menos que un `catch` cláusula incluye un nombre de variable de excepción, es imposible obtener acceso al objeto de excepción en el filtro y `catch` bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="6780e-530">Un `catch` cláusula que no especifican un *exception_specifier* se denomina general `catch` cláusula.</span><span class="sxs-lookup"><span data-stu-id="6780e-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="6780e-531">Algunos lenguajes de programación pueden admitir las excepciones que no se puede representables como un objeto derivado de `System.Exception`, aunque dichas excepciones nunca podrían generar código de C#.</span><span class="sxs-lookup"><span data-stu-id="6780e-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="6780e-532">General `catch` cláusula puede utilizarse para detectar estas excepciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="6780e-533">Por lo tanto, un general `catch` es semánticamente diferente desde el que especifica el tipo de cláusula `System.Exception`, ya que la primera también puede detectar las excepciones de otros lenguajes.</span><span class="sxs-lookup"><span data-stu-id="6780e-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="6780e-534">Para encontrar un controlador para una excepción, `catch` cláusulas se examinan en orden léxico.</span><span class="sxs-lookup"><span data-stu-id="6780e-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="6780e-535">Si un `catch` cláusula especifica un tipo, pero ningún filtro de excepción, es un error en tiempo de compilación para una versión posterior `catch` cláusula en la misma `try` instrucción para especificar un tipo que es igual o se deriva y que escriba.</span><span class="sxs-lookup"><span data-stu-id="6780e-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="6780e-536">Si un `catch` cláusula especifica ningún tipo y no hay ningún filtro, debe ser el último `catch` cláusula para que `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="6780e-537">Dentro de un `catch` bloque, un `throw` instrucción ([la instrucción throw](statements.md#the-throw-statement)) sin una expresión puede utilizarse para volver a producir la excepción que ha sido detectada por la `catch` bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="6780e-538">Las asignaciones a una variable de excepción no modifican la excepción que se vuelve a producir.</span><span class="sxs-lookup"><span data-stu-id="6780e-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="6780e-539">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="6780e-539">In the example</span></span>
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
<span data-ttu-id="6780e-540">el método `F` detecta una excepción, escribe información de diagnóstico en la consola, modifica la variable de excepción y vuelve a iniciar la excepción.</span><span class="sxs-lookup"><span data-stu-id="6780e-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="6780e-541">La excepción que se vuelve a producir es la excepción original, por lo que el resultado es:</span><span class="sxs-lookup"><span data-stu-id="6780e-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="6780e-542">Si el primer bloque catch iniciara `e` en lugar de volver a producir la excepción actual, el resultado sería como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```csharp
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="6780e-543">Es un error en tiempo de compilación para un `break`, `continue`, o `goto` instrucción para transferir el control fuera de un `finally` bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="6780e-544">Cuando un `break`, `continue`, o `goto` instrucción se produce en un `finally` bloque, el destino de la instrucción debe estar dentro del mismo `finally` bloque, o en caso contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="6780e-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="6780e-545">Es un error en tiempo de compilación para un `return` instrucción que se produzca en un `finally` bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="6780e-546">Un `try` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-547">El control se transfiere a la `try` bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="6780e-548">Cuando y si el control alcanza el punto final de la `try` bloque:</span><span class="sxs-lookup"><span data-stu-id="6780e-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="6780e-549">Si el `try` instrucción tiene un `finally` bloque, el `finally` bloque se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="6780e-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="6780e-550">El control se transfiere al punto final de la `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="6780e-551">Si una excepción se propaga a la `try` instrucción durante la ejecución de la `try` bloque:</span><span class="sxs-lookup"><span data-stu-id="6780e-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="6780e-552">El `catch` cláusulas, si existe, se examinan en orden de aparición para buscar un controlador adecuado para la excepción.</span><span class="sxs-lookup"><span data-stu-id="6780e-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="6780e-553">Si un `catch` no especifica un tipo de cláusula, o especifica el tipo de excepción o un tipo base del tipo de excepción:</span><span class="sxs-lookup"><span data-stu-id="6780e-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="6780e-554">Si el `catch` cláusula declara una variable de excepción, el objeto de excepción se asigna a la variable de excepción.</span><span class="sxs-lookup"><span data-stu-id="6780e-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="6780e-555">Si el `catch` cláusula declara un filtro de excepción, se evalúa el filtro.</span><span class="sxs-lookup"><span data-stu-id="6780e-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="6780e-556">Si se evalúa como `false`, la cláusula catch no es una coincidencia y la búsqueda continúa a través de cualquier posteriores `catch` cláusulas para un controlador adecuado.</span><span class="sxs-lookup"><span data-stu-id="6780e-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="6780e-557">En caso contrario, el `catch` cláusula se considera una coincidencia, y el control se transfiere a la coincidencia `catch` bloque.</span><span class="sxs-lookup"><span data-stu-id="6780e-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="6780e-558">Cuando y si el control alcanza el punto final de la `catch` bloque:</span><span class="sxs-lookup"><span data-stu-id="6780e-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="6780e-559">Si el `try` instrucción tiene un `finally` bloque, el `finally` bloque se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="6780e-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="6780e-560">El control se transfiere al punto final de la `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="6780e-561">Si una excepción se propaga a la `try` instrucción durante la ejecución de la `catch` bloque:</span><span class="sxs-lookup"><span data-stu-id="6780e-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="6780e-562">Si el `try` instrucción tiene un `finally` bloque, el `finally` bloque se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="6780e-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="6780e-563">La excepción se propaga a la siguiente inclusión `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="6780e-564">Si el `try` instrucción no tiene ningún `catch` cláusulas o si no hay ningún `catch` cláusula coincide con la excepción:</span><span class="sxs-lookup"><span data-stu-id="6780e-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="6780e-565">Si el `try` instrucción tiene un `finally` bloque, el `finally` bloque se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="6780e-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="6780e-566">La excepción se propaga a la siguiente inclusión `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="6780e-567">Las instrucciones de un `finally` bloque siempre se ejecutan cuando el control abandona un `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="6780e-568">Esto es cierto si la transferencia de control se produce como resultado de una ejecución normal, como resultado de ejecutar un `break`, `continue`, `goto`, o `return` instrucción, o como resultado de la propagación de una excepción fuera de la `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="6780e-569">Si se produce una excepción durante la ejecución de un `finally` bloquea y no se captura dentro del mismo bloque finally, la excepción se propaga a la siguiente inclusión `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="6780e-570">Si fue otra excepción en el proceso de propagación, esa excepción se pierde.</span><span class="sxs-lookup"><span data-stu-id="6780e-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="6780e-571">El proceso de propagación de una excepción se explica con más detalle en la descripción de la `throw` instrucción ([la instrucción throw](statements.md#the-throw-statement)).</span><span class="sxs-lookup"><span data-stu-id="6780e-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="6780e-572">El `try` block de un `try` instrucción es alcanzable si el `try` instrucción es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="6780e-573">Un `catch` block de un `try` instrucción es alcanzable si el `try` instrucción es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="6780e-574">El `finally` block de un `try` instrucción es alcanzable si el `try` instrucción es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="6780e-575">El punto final de un `try` instrucción es alcanzable si se cumplen dos de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="6780e-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="6780e-576">El punto final de la `try` bloque es alcanzable o el punto final de al menos un `catch` bloque es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="6780e-577">Si un `finally` bloque está presente, el punto final de la `finally` bloque es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="6780e-578">Las instrucciones checked y unchecked</span><span class="sxs-lookup"><span data-stu-id="6780e-578">The checked and unchecked statements</span></span>

<span data-ttu-id="6780e-579">El `checked` y `unchecked` instrucciones sirven para controlar la ***contexto de comprobación de desbordamiento*** para conversiones y operaciones aritméticas de tipo integral.</span><span class="sxs-lookup"><span data-stu-id="6780e-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="6780e-580">El `checked` instrucción hace que todas las expresiones de la *bloque* se evalúan en un contexto comprobado y el `unchecked` instrucción hace que todas las expresiones de la *bloque* se evalúan en un un contexto no comprobado.</span><span class="sxs-lookup"><span data-stu-id="6780e-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="6780e-581">El `checked` y `unchecked` son exactamente equivalentes a las instrucciones del `checked` y `unchecked` operadores ([los operadores checked y unchecked](expressions.md#the-checked-and-unchecked-operators)), excepto en que operan en bloques en lugar de expresiones .</span><span class="sxs-lookup"><span data-stu-id="6780e-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="6780e-582">La instrucción lock</span><span class="sxs-lookup"><span data-stu-id="6780e-582">The lock statement</span></span>

<span data-ttu-id="6780e-583">El `lock` instrucción obtiene el bloqueo de exclusión mutua para un objeto determinado, ejecuta una instrucción y, a continuación, libera el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="6780e-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="6780e-584">La expresión de un `lock` instrucción debe denotar un valor de un tipo conocido como un *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="6780e-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="6780e-585">No hay conversión boxing implícita ([conversiones Boxing](conversions.md#boxing-conversions)) nunca se realiza para la expresión de un `lock` instrucción y, por lo tanto es un error de tiempo de compilación de la expresión denota un valor de un *value_type*.</span><span class="sxs-lookup"><span data-stu-id="6780e-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="6780e-586">Un `lock` instrucción del formulario</span><span class="sxs-lookup"><span data-stu-id="6780e-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="6780e-587">donde `x` es una expresión de un *reference_type*, es equivalente a</span><span class="sxs-lookup"><span data-stu-id="6780e-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
<span data-ttu-id="6780e-588">salvo que `x` solo se evalúa una vez.</span><span class="sxs-lookup"><span data-stu-id="6780e-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="6780e-589">Mientras se mantiene un bloqueo de exclusión mutua, código que se ejecuta en el mismo subproceso de ejecución también puede obtener y liberar el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="6780e-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="6780e-590">Sin embargo, se bloquea código que se ejecuta en otros subprocesos obtengan el bloqueo hasta que se libere el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="6780e-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="6780e-591">Bloqueo `System.Type` objetos con el fin de sincronizar el acceso a los datos estáticos no se recomienda.</span><span class="sxs-lookup"><span data-stu-id="6780e-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="6780e-592">Puede bloquear otro código en el mismo tipo, que puede producir un interbloqueo.</span><span class="sxs-lookup"><span data-stu-id="6780e-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="6780e-593">Un mejor enfoque es sincronizar el acceso a los datos estáticos mediante el bloqueo de un objeto estático privado.</span><span class="sxs-lookup"><span data-stu-id="6780e-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="6780e-594">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6780e-594">For example:</span></span>
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a><span data-ttu-id="6780e-595">La instrucción using</span><span class="sxs-lookup"><span data-stu-id="6780e-595">The using statement</span></span>

<span data-ttu-id="6780e-596">El `using` instrucción obtiene uno o más recursos, ejecuta una instrucción y, a continuación, elimina el recurso.</span><span class="sxs-lookup"><span data-stu-id="6780e-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="6780e-597">Un ***recursos*** es una clase o struct que implemente `System.IDisposable`, que incluye un único método sin parámetros denominado `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="6780e-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="6780e-598">Puede llamar código que usa un recurso `Dispose` para indicar que el recurso ya no es necesario.</span><span class="sxs-lookup"><span data-stu-id="6780e-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="6780e-599">Si `Dispose` no se llama disposición automática finalmente se produce como consecuencia de la recolección de elementos.</span><span class="sxs-lookup"><span data-stu-id="6780e-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="6780e-600">Si el formulario de *resource_acquisition* es *local_variable_declaration* , a continuación, el tipo de la *local_variable_declaration* debe ser `dynamic` o un tipo que se pueda convertir implícitamente a `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="6780e-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="6780e-601">Si el formulario de *resource_acquisition* es *expresión* , a continuación, esta expresión debe ser implícitamente convertible a `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="6780e-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="6780e-602">Las variables locales declaradas en un *resource_acquisition* son de solo lectura y debe incluir un inicializador.</span><span class="sxs-lookup"><span data-stu-id="6780e-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="6780e-603">Se produce un error de tiempo de compilación si la instrucción incrustada intenta modificar estas variables locales (por medio de asignación o el `++` y `--` operadores), tomar la dirección de ellos o pasarlos como `ref` o `out` parámetros.</span><span class="sxs-lookup"><span data-stu-id="6780e-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="6780e-604">Un `using` instrucción se traduce en tres partes: adquisición, uso y eliminación.</span><span class="sxs-lookup"><span data-stu-id="6780e-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="6780e-605">Uso del recurso está incluido implícitamente en un `try` instrucción que incluye un `finally` cláusula.</span><span class="sxs-lookup"><span data-stu-id="6780e-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="6780e-606">Esto `finally` cláusula elimina el recurso.</span><span class="sxs-lookup"><span data-stu-id="6780e-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="6780e-607">Si un `null` se adquiere el recurso, a continuación, ninguna llamada a `Dispose` se realiza, y se produce ninguna excepción.</span><span class="sxs-lookup"><span data-stu-id="6780e-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="6780e-608">Si el recurso es de tipo `dynamic` dinámicamente se convierte a través de una conversión implícita dinámica ([conversiones implícitas de dinámicas](conversions.md#implicit-dynamic-conversions)) a `IDisposable` durante la adquisición para asegurarse de que la conversión es finalizar correctamente para el uso y la eliminación.</span><span class="sxs-lookup"><span data-stu-id="6780e-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="6780e-609">Un `using` instrucción del formulario</span><span class="sxs-lookup"><span data-stu-id="6780e-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="6780e-610">corresponde a uno de tres posibles expansiones.</span><span class="sxs-lookup"><span data-stu-id="6780e-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="6780e-611">Cuando `ResourceType` es un tipo de valor distinto de null, la expansión es:</span><span class="sxs-lookup"><span data-stu-id="6780e-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="6780e-612">En caso contrario, cuando `ResourceType` es un tipo de valor que acepta valores NULL o un tipo de referencia distinto `dynamic`, la expansión es:</span><span class="sxs-lookup"><span data-stu-id="6780e-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

<span data-ttu-id="6780e-613">En caso contrario, cuando `ResourceType` es `dynamic`, la expansión es:</span><span class="sxs-lookup"><span data-stu-id="6780e-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

<span data-ttu-id="6780e-614">En cualquiera de las expansiones, el `resource` variable es de solo lectura en la instrucción incrustada y el `d` variable es accesible en el e invisible a la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="6780e-615">Se permite una implementación para implementar una determinada instrucción using de manera diferente, por ejemplo, por motivos de rendimiento, siempre y cuando el comportamiento es coherente con la expansión de la anterior.</span><span class="sxs-lookup"><span data-stu-id="6780e-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="6780e-616">Un `using` instrucción del formulario</span><span class="sxs-lookup"><span data-stu-id="6780e-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="6780e-617">tiene las mismas tres posibles expansiones.</span><span class="sxs-lookup"><span data-stu-id="6780e-617">has the same three possible expansions.</span></span> <span data-ttu-id="6780e-618">En este caso `ResourceType` es implícitamente el tipo de tiempo de compilación de la `expression`, si tiene uno.</span><span class="sxs-lookup"><span data-stu-id="6780e-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="6780e-619">En caso contrario, la interfaz `IDisposable` propio se utiliza como el `ResourceType`.</span><span class="sxs-lookup"><span data-stu-id="6780e-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="6780e-620">El `resource` variable es accesible en el e invisible a la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="6780e-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="6780e-621">Cuando un *resource_acquisition* adopta la forma de un *local_variable_declaration*, es posible adquirir varios recursos de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="6780e-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="6780e-622">Un `using` instrucción del formulario</span><span class="sxs-lookup"><span data-stu-id="6780e-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="6780e-623">se anida con precisión equivalente a una secuencia de `using` instrucciones:</span><span class="sxs-lookup"><span data-stu-id="6780e-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="6780e-624">El ejemplo siguiente crea un archivo denominado `log.txt` y escribe dos líneas de texto en el archivo.</span><span class="sxs-lookup"><span data-stu-id="6780e-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="6780e-625">El ejemplo, a continuación, abre el mismo archivo para lectura y copia las líneas de texto independientes en la consola.</span><span class="sxs-lookup"><span data-stu-id="6780e-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

<span data-ttu-id="6780e-626">Puesto que la `TextWriter` y `TextReader` clases implementan la `IDisposable` interfaz, puede usar el ejemplo `using` instrucciones para asegurarse de que el archivo subyacente se cierra correctamente después de la escritura o las operaciones de lectura.</span><span class="sxs-lookup"><span data-stu-id="6780e-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="6780e-627">La instrucción yield</span><span class="sxs-lookup"><span data-stu-id="6780e-627">The yield statement</span></span>

<span data-ttu-id="6780e-628">El `yield` instrucción se utiliza en un bloque de iteradores ([bloques](statements.md#blocks)) para producir un valor al objeto enumerador ([objetos enumerador](classes.md#enumerator-objects)) o un objeto enumerable ([objetos enumerables](classes.md#enumerable-objects)) de un iterador o para indicar el final de la iteración.</span><span class="sxs-lookup"><span data-stu-id="6780e-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="6780e-629">`yield` no es una palabra reservada; tiene un significado especial solo cuando se usa inmediatamente antes un `return` o `break` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="6780e-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="6780e-630">En otros contextos, `yield` puede usarse como identificador.</span><span class="sxs-lookup"><span data-stu-id="6780e-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="6780e-631">Existen varias restricciones sobre dónde un `yield` instrucción puede aparecer como se describe en la siguiente.</span><span class="sxs-lookup"><span data-stu-id="6780e-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="6780e-632">Es un error en tiempo de compilación para un `yield` (informe de cualquiera de las formas) a aparecer fuera de un *cuerpoMétodo*, *operator_body* o *accessor_body*</span><span class="sxs-lookup"><span data-stu-id="6780e-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="6780e-633">Es un error en tiempo de compilación para un `yield` (informe de cualquiera de las formas) a aparecer dentro de una función anónima.</span><span class="sxs-lookup"><span data-stu-id="6780e-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="6780e-634">Es un error en tiempo de compilación para un `yield` (informe de cualquiera de las formas) aparezca en el `finally` cláusula de una `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="6780e-635">Es un error en tiempo de compilación para un `yield return` instrucción para aparecer en cualquier lugar en un `try` instrucción que incluye cualquier `catch` cláusulas.</span><span class="sxs-lookup"><span data-stu-id="6780e-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="6780e-636">El ejemplo siguiente muestra algunos usos válidos y no válidos de `yield` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

<span data-ttu-id="6780e-637">Una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) debe existir el tipo de la expresión en el `yield return` instrucción al tipo yield ([tipo Yield](classes.md#yield-type)) del iterador.</span><span class="sxs-lookup"><span data-stu-id="6780e-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="6780e-638">Un `yield return` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-639">La expresión proporcionada en la instrucción se evalúa implícitamente convierte al tipo yield y asignada a la `Current` propiedad del objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="6780e-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="6780e-640">Se suspende la ejecución del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="6780e-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="6780e-641">Si el `yield return` instrucción está dentro de uno o varios `try` bloquea asociado `finally` bloques no se ejecutan en este momento.</span><span class="sxs-lookup"><span data-stu-id="6780e-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="6780e-642">El `MoveNext` método del objeto enumerador devuelve `true` a su llamador, que indica que el objeto de enumerador avanza correctamente al elemento siguiente.</span><span class="sxs-lookup"><span data-stu-id="6780e-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="6780e-643">La siguiente llamada para el objeto de enumerador `MoveNext` método reanuda la ejecución del bloque de iteradores desde donde se suspendió por última vez.</span><span class="sxs-lookup"><span data-stu-id="6780e-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="6780e-644">Un `yield break` instrucción se ejecuta como sigue:</span><span class="sxs-lookup"><span data-stu-id="6780e-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="6780e-645">Si el `yield break` instrucción está dentro de uno o varios `try` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="6780e-646">Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="6780e-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="6780e-647">Este proceso se repite hasta que el `finally` bloques de inclusión de todos los `try` se han ejecutado las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="6780e-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="6780e-648">Control se devuelve al llamador del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="6780e-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="6780e-649">Puede ser el `MoveNext` método o `Dispose` método del objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="6780e-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="6780e-650">Dado que un `yield break` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `yield break` instrucción nunca es alcanzable.</span><span class="sxs-lookup"><span data-stu-id="6780e-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
