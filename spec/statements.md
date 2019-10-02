---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704041"
---
# <a name="statements"></a><span data-ttu-id="82896-101">Instrucciones</span><span class="sxs-lookup"><span data-stu-id="82896-101">Statements</span></span>

<span data-ttu-id="82896-102">C#proporciona una serie de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="82896-102">C# provides a variety of statements.</span></span> <span data-ttu-id="82896-103">La mayoría de estas instrucciones resultará familiar a los programadores que hayan programado C++en C y.</span><span class="sxs-lookup"><span data-stu-id="82896-103">Most of these statements will be familiar to developers who have programmed in C and C++.</span></span>

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

<span data-ttu-id="82896-104">*Embedded_statement* nonterminal se utiliza para las instrucciones que aparecen dentro de otras instrucciones.</span><span class="sxs-lookup"><span data-stu-id="82896-104">The *embedded_statement* nonterminal is used for statements that appear within other statements.</span></span> <span data-ttu-id="82896-105">El uso de *embedded_statement* en lugar de *Statement* excluye el uso de las instrucciones de declaración y las instrucciones con etiquetas en estos contextos.</span><span class="sxs-lookup"><span data-stu-id="82896-105">The use of *embedded_statement* rather than *statement* excludes the use of declaration statements and labeled statements in these contexts.</span></span> <span data-ttu-id="82896-106">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-106">The example</span></span>
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
<span data-ttu-id="82896-107">produce un error en tiempo de compilación porque una instrucción `if` requiere un *embedded_statement* en lugar de una *instrucción* para su bifurcación if.</span><span class="sxs-lookup"><span data-stu-id="82896-107">results in a compile-time error because an `if` statement requires an *embedded_statement* rather than a *statement* for its if branch.</span></span> <span data-ttu-id="82896-108">Si se permitía este código, la variable `i` se declararía, pero nunca se podía usar.</span><span class="sxs-lookup"><span data-stu-id="82896-108">If this code were permitted, then the variable `i` would be declared, but it could never be used.</span></span> <span data-ttu-id="82896-109">Tenga en cuenta, sin embargo, `i`que si coloca la declaración de en un bloque, el ejemplo es válido.</span><span class="sxs-lookup"><span data-stu-id="82896-109">Note, however, that by placing `i`'s declaration in a block, the example is valid.</span></span>

## <a name="end-points-and-reachability"></a><span data-ttu-id="82896-110">Extremos y disponibilidad</span><span class="sxs-lookup"><span data-stu-id="82896-110">End points and reachability</span></span>

<span data-ttu-id="82896-111">Cada instrucción tiene un ***punto final***.</span><span class="sxs-lookup"><span data-stu-id="82896-111">Every statement has an ***end point***.</span></span> <span data-ttu-id="82896-112">En términos intuitivos, el punto final de una instrucción es la ubicación que sigue inmediatamente a la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-112">In intuitive terms, the end point of a statement is the location that immediately follows the statement.</span></span> <span data-ttu-id="82896-113">Las reglas de ejecución para las instrucciones compuestas (instrucciones que contienen instrucciones incrustadas) especifican la acción que se realiza cuando el control alcanza el punto final de una instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="82896-113">The execution rules for composite statements (statements that contain embedded statements) specify the action that is taken when control reaches the end point of an embedded statement.</span></span> <span data-ttu-id="82896-114">Por ejemplo, cuando el control alcanza el punto final de una instrucción en un bloque, el control se transfiere a la siguiente instrucción del bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-114">For example, when control reaches the end point of a statement in a block, control is transferred to the next statement in the block.</span></span>

<span data-ttu-id="82896-115">Si la ejecución puede alcanzar una instrucción, se dice que la instrucción es ***accesible***.</span><span class="sxs-lookup"><span data-stu-id="82896-115">If a statement can possibly be reached by execution, the statement is said to be ***reachable***.</span></span> <span data-ttu-id="82896-116">Por el contrario, si no hay ninguna posibilidad de que se ejecute una instrucción, se dice que la instrucción no es ***accesible***.</span><span class="sxs-lookup"><span data-stu-id="82896-116">Conversely, if there is no possibility that a statement will be executed, the statement is said to be ***unreachable***.</span></span>

<span data-ttu-id="82896-117">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-117">In the example</span></span>
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
<span data-ttu-id="82896-118">la segunda invocación de `Console.WriteLine` es inaccesible porque no existe ninguna posibilidad de que se ejecute la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-118">the second invocation of `Console.WriteLine` is unreachable because there is no possibility that the statement will be executed.</span></span>

<span data-ttu-id="82896-119">Se genera una advertencia si el compilador determina que no se puede tener acceso a una instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-119">A warning is reported if the compiler determines that a statement is unreachable.</span></span> <span data-ttu-id="82896-120">No se trata de un error específico para que no se pueda tener acceso a una instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-120">It is specifically not an error for a statement to be unreachable.</span></span>

<span data-ttu-id="82896-121">Para determinar si una instrucción o un extremo concreto es accesible, el compilador realiza el análisis de flujo según las reglas de disponibilidad definidas para cada instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-121">To determine whether a particular statement or end point is reachable, the compiler performs flow analysis according to the reachability rules defined for each statement.</span></span> <span data-ttu-id="82896-122">El análisis de flujo tiene en cuenta los valores de expresiones constantes ([expresiones constantes](expressions.md#constant-expressions)) que controlan el comportamiento de las instrucciones, pero no se tienen en cuenta los valores posibles de las expresiones que no son constantes.</span><span class="sxs-lookup"><span data-stu-id="82896-122">The flow analysis takes into account the values of constant expressions ([Constant expressions](expressions.md#constant-expressions)) that control the behavior of statements, but the possible values of non-constant expressions are not considered.</span></span> <span data-ttu-id="82896-123">En otras palabras, a efectos del análisis de flujo de control, se considera que una expresión no constante de un tipo determinado tiene cualquier valor posible de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="82896-123">In other words, for purposes of control flow analysis, a non-constant expression of a given type is considered to have any possible value of that type.</span></span>

<span data-ttu-id="82896-124">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-124">In the example</span></span>
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
<span data-ttu-id="82896-125">la expresión booleana de la `if` instrucción es una expresión constante porque ambos operandos `==` del operador son constantes.</span><span class="sxs-lookup"><span data-stu-id="82896-125">the boolean expression of the `if` statement is a constant expression because both operands of the `==` operator are constants.</span></span> <span data-ttu-id="82896-126">Como la expresión constante se evalúa en tiempo de compilación, lo que genera `false`el valor `Console.WriteLine` , se considera que la invocación no es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-126">As the constant expression is evaluated at compile-time, producing the value `false`, the `Console.WriteLine` invocation is considered unreachable.</span></span> <span data-ttu-id="82896-127">Sin embargo, `i` si se cambia para ser una variable local</span><span class="sxs-lookup"><span data-stu-id="82896-127">However, if `i` is changed to be a local variable</span></span>
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
<span data-ttu-id="82896-128">la `Console.WriteLine` invocación se considera accesible, aunque, en realidad, nunca se ejecutará.</span><span class="sxs-lookup"><span data-stu-id="82896-128">the `Console.WriteLine` invocation is considered reachable, even though, in reality, it will never be executed.</span></span>

<span data-ttu-id="82896-129">El *bloque* de un miembro de función siempre se considera alcanzable.</span><span class="sxs-lookup"><span data-stu-id="82896-129">The *block* of a function member is always considered reachable.</span></span> <span data-ttu-id="82896-130">Al evaluar sucesivamente las reglas de disponibilidad de cada instrucción en un bloque, se puede determinar la disponibilidad de cualquier instrucción determinada.</span><span class="sxs-lookup"><span data-stu-id="82896-130">By successively evaluating the reachability rules of each statement in a block, the reachability of any given statement can be determined.</span></span>

<span data-ttu-id="82896-131">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-131">In the example</span></span>
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
<span data-ttu-id="82896-132">la disponibilidad de la segunda `Console.WriteLine` se determina de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-132">the reachability of the second `Console.WriteLine` is determined as follows:</span></span>

*  <span data-ttu-id="82896-133">La primera `Console.WriteLine` instrucción `F` de expresión es accesible porque el bloque del método es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-133">The first `Console.WriteLine` expression statement is reachable because the block of the `F` method is reachable.</span></span>
*  <span data-ttu-id="82896-134">El punto final de la primera `Console.WriteLine` instrucción de expresión es accesible porque esa instrucción es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-134">The end point of the first `Console.WriteLine` expression statement is reachable because that statement is reachable.</span></span>
*  <span data-ttu-id="82896-135">La `if` instrucción es accesible porque el punto final de la primera `Console.WriteLine` instrucción de expresión es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-135">The `if` statement is reachable because the end point of the first `Console.WriteLine` expression statement is reachable.</span></span>
*  <span data-ttu-id="82896-136">La segunda `Console.WriteLine` instrucción de expresión es accesible porque la expresión booleana de la `if` instrucción no tiene el valor `false`constante.</span><span class="sxs-lookup"><span data-stu-id="82896-136">The second `Console.WriteLine` expression statement is reachable because the boolean expression of the `if` statement does not have the constant value `false`.</span></span>

<span data-ttu-id="82896-137">Hay dos situaciones en las que se trata de un error en tiempo de compilación para que el punto final de una instrucción sea alcanzable:</span><span class="sxs-lookup"><span data-stu-id="82896-137">There are two situations in which it is a compile-time error for the end point of a statement to be reachable:</span></span>

*  <span data-ttu-id="82896-138">Dado que `switch` la instrucción no permite que una sección switch pase a la siguiente sección switch, se trata de un error en tiempo de compilación para que el punto final de la lista de instrucciones de una sección switch sea accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-138">Because the `switch` statement does not permit a switch section to "fall through" to the next switch section, it is a compile-time error for the end point of the statement list of a switch section to be reachable.</span></span> <span data-ttu-id="82896-139">Si se produce este error, normalmente es una indicación de que `break` falta una instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-139">If this error occurs, it is typically an indication that a `break` statement is missing.</span></span>
*  <span data-ttu-id="82896-140">Es un error en tiempo de compilación el punto final del bloque de un miembro de función que calcula un valor al que se puede tener acceso.</span><span class="sxs-lookup"><span data-stu-id="82896-140">It is a compile-time error for the end point of the block of a function member that computes a value to be reachable.</span></span> <span data-ttu-id="82896-141">Si se produce este error, normalmente es una indicación de que `return` falta una instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-141">If this error occurs, it typically is an indication that a `return` statement is missing.</span></span>

## <a name="blocks"></a><span data-ttu-id="82896-142">Bloques</span><span class="sxs-lookup"><span data-stu-id="82896-142">Blocks</span></span>

<span data-ttu-id="82896-143">Un *bloque* permite que se escriban varias instrucciones en contextos donde se permite una única instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-143">A *block* permits multiple statements to be written in contexts where a single statement is allowed.</span></span>

```antlr
block
    : '{' statement_list? '}'
    ;
```

<span data-ttu-id="82896-144">Un *bloque* consta de un *statement_list* opcional ([listas de instrucciones](statements.md#statement-lists)), entre llaves.</span><span class="sxs-lookup"><span data-stu-id="82896-144">A *block* consists of an optional *statement_list* ([Statement lists](statements.md#statement-lists)), enclosed in braces.</span></span> <span data-ttu-id="82896-145">Si se omite la lista de instrucciones, se dice que el bloque está vacío.</span><span class="sxs-lookup"><span data-stu-id="82896-145">If the statement list is omitted, the block is said to be empty.</span></span>

<span data-ttu-id="82896-146">Un bloque puede contener instrucciones de declaración ([instrucciones de declaración](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="82896-146">A block may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="82896-147">El ámbito de una variable local o constante declarada en un bloque es el bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-147">The scope of a local variable or constant declared in a block is the block.</span></span>

<span data-ttu-id="82896-148">Un bloque se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-148">A block is executed as follows:</span></span>

*  <span data-ttu-id="82896-149">Si el bloque está vacío, el control se transfiere al punto final del bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-149">If the block is empty, control is transferred to the end point of the block.</span></span>
*  <span data-ttu-id="82896-150">Si el bloque no está vacío, el control se transfiere a la lista de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="82896-150">If the block is not empty, control is transferred to the statement list.</span></span> <span data-ttu-id="82896-151">Cuando y si el control alcanza el punto final de la lista de instrucciones, el control se transfiere al punto final del bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-151">When and if control reaches the end point of the statement list, control is transferred to the end point of the block.</span></span>

<span data-ttu-id="82896-152">La lista de instrucciones de un bloque es accesible si el propio bloque es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-152">The statement list of a block is reachable if the block itself is reachable.</span></span>

<span data-ttu-id="82896-153">El punto final de un bloque es accesible si el bloque está vacío o si el punto final de la lista de instrucciones es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-153">The end point of a block is reachable if the block is empty or if the end point of the statement list is reachable.</span></span>

<span data-ttu-id="82896-154">Un *bloque* que contiene una o más `yield` instrucciones ([la instrucción yield](statements.md#the-yield-statement)) se denomina bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="82896-154">A *block* that contains one or more `yield` statements ([The yield statement](statements.md#the-yield-statement)) is called an iterator block.</span></span> <span data-ttu-id="82896-155">Los bloques de iterador se usan para implementar miembros de función como iteradores ([iteradores](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="82896-155">Iterator blocks are used to implement function members as iterators ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="82896-156">Se aplican algunas restricciones adicionales a los bloques de iteradores:</span><span class="sxs-lookup"><span data-stu-id="82896-156">Some additional restrictions apply to iterator blocks:</span></span>

*  <span data-ttu-id="82896-157">Se trata de un error en tiempo de compilación `return` para que una instrucción aparezca en un bloque de `yield return` iteradores (pero se permiten las instrucciones).</span><span class="sxs-lookup"><span data-stu-id="82896-157">It is a compile-time error for a `return` statement to appear in an iterator block (but `yield return` statements are permitted).</span></span>
*  <span data-ttu-id="82896-158">Es un error en tiempo de compilación que un bloque de iteradores contenga un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)).</span><span class="sxs-lookup"><span data-stu-id="82896-158">It is a compile-time error for an iterator block to contain an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)).</span></span> <span data-ttu-id="82896-159">Un bloque de iterador siempre define un contexto seguro, incluso cuando su declaración está anidada en un contexto no seguro.</span><span class="sxs-lookup"><span data-stu-id="82896-159">An iterator block always defines a safe context, even when its declaration is nested in an unsafe context.</span></span>

### <a name="statement-lists"></a><span data-ttu-id="82896-160">Listas de instrucciones</span><span class="sxs-lookup"><span data-stu-id="82896-160">Statement lists</span></span>

<span data-ttu-id="82896-161">Una ***lista de instrucciones*** consta de una o varias instrucciones escritas en secuencia.</span><span class="sxs-lookup"><span data-stu-id="82896-161">A ***statement list*** consists of one or more statements written in sequence.</span></span> <span data-ttu-id="82896-162">Las listas de instrucciones aparecen en *bloque*s ([bloques](statements.md#blocks)) y en *switch_block*s ([la instrucción switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="82896-162">Statement lists occur in *block*s ([Blocks](statements.md#blocks)) and in *switch_block*s ([The switch statement](statements.md#the-switch-statement)).</span></span>

```antlr
statement_list
    : statement+
    ;
```

<span data-ttu-id="82896-163">Una lista de instrucciones se ejecuta mediante la transferencia de control a la primera instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-163">A statement list is executed by transferring control to the first statement.</span></span> <span data-ttu-id="82896-164">Cuando y si el control alcanza el punto final de una instrucción, el control se transfiere a la siguiente instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-164">When and if control reaches the end point of a statement, control is transferred to the next statement.</span></span> <span data-ttu-id="82896-165">Cuando el control alcanza el punto final de la última instrucción, se transfiere el control al punto final de la lista de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="82896-165">When and if control reaches the end point of the last statement, control is transferred to the end point of the statement list.</span></span>

<span data-ttu-id="82896-166">Se puede tener acceso a una instrucción de una lista de instrucciones si se cumple al menos una de las siguientes condiciones:</span><span class="sxs-lookup"><span data-stu-id="82896-166">A statement in a statement list is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="82896-167">La instrucción es la primera instrucción y la propia lista de instrucciones es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-167">The statement is the first statement and the statement list itself is reachable.</span></span>
*  <span data-ttu-id="82896-168">El punto final de la instrucción anterior es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-168">The end point of the preceding statement is reachable.</span></span>
*  <span data-ttu-id="82896-169">La instrucción es una instrucción con etiqueta y una `goto` instrucción accesible hace referencia a la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-169">The statement is a labeled statement and the label is referenced by a reachable `goto` statement.</span></span>

<span data-ttu-id="82896-170">El punto final de una lista de instrucciones es accesible si el punto final de la última instrucción de la lista es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-170">The end point of a statement list is reachable if the end point of the last statement in the list is reachable.</span></span>

## <a name="the-empty-statement"></a><span data-ttu-id="82896-171">Instrucción vacía</span><span class="sxs-lookup"><span data-stu-id="82896-171">The empty statement</span></span>

<span data-ttu-id="82896-172">Un *empty_statement* no hace nada.</span><span class="sxs-lookup"><span data-stu-id="82896-172">An *empty_statement* does nothing.</span></span>

```antlr
empty_statement
    : ';'
    ;
```

<span data-ttu-id="82896-173">Se utiliza una instrucción vacía cuando no hay ninguna operación que realizar en un contexto donde se requiere una instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-173">An empty statement is used when there are no operations to perform in a context where a statement is required.</span></span>

<span data-ttu-id="82896-174">La ejecución de una instrucción vacía simplemente transfiere el control al punto final de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-174">Execution of an empty statement simply transfers control to the end point of the statement.</span></span> <span data-ttu-id="82896-175">Por lo tanto, el punto final de una instrucción vacía es accesible si la instrucción vacía es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-175">Thus, the end point of an empty statement is reachable if the empty statement is reachable.</span></span>

<span data-ttu-id="82896-176">Se puede usar una instrucción vacía al escribir una `while` instrucción con un cuerpo nulo:</span><span class="sxs-lookup"><span data-stu-id="82896-176">An empty statement can be used when writing a `while` statement with a null body:</span></span>
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

<span data-ttu-id="82896-177">Además, se puede usar una instrucción vacía para declarar una etiqueta justo antes del cierre "`}`" de un bloque:</span><span class="sxs-lookup"><span data-stu-id="82896-177">Also, an empty statement can be used to declare a label just before the closing "`}`" of a block:</span></span>
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a><span data-ttu-id="82896-178">Instrucciones con etiqueta</span><span class="sxs-lookup"><span data-stu-id="82896-178">Labeled statements</span></span>

<span data-ttu-id="82896-179">Un *labeled_statement* permite a una instrucción ir precedida por una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-179">A *labeled_statement* permits a statement to be prefixed by a label.</span></span> <span data-ttu-id="82896-180">Las instrucciones con etiqueta se permiten en bloques, pero no se permiten como instrucciones incrustadas.</span><span class="sxs-lookup"><span data-stu-id="82896-180">Labeled statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

<span data-ttu-id="82896-181">Una instrucción con etiqueta declara una etiqueta con el nombre proporcionado por el *identificador*.</span><span class="sxs-lookup"><span data-stu-id="82896-181">A labeled statement declares a label with the name given by the *identifier*.</span></span> <span data-ttu-id="82896-182">El ámbito de una etiqueta es el bloque entero en el que se declara la etiqueta, incluidos los bloques anidados.</span><span class="sxs-lookup"><span data-stu-id="82896-182">The scope of a label is the whole block in which the label is declared, including any nested blocks.</span></span> <span data-ttu-id="82896-183">Es un error en tiempo de compilación que dos etiquetas con el mismo nombre tienen ámbitos superpuestos.</span><span class="sxs-lookup"><span data-stu-id="82896-183">It is a compile-time error for two labels with the same name to have overlapping scopes.</span></span>

<span data-ttu-id="82896-184">Se puede hacer referencia a una etiqueta `goto` desde instrucciones ([instrucción Goto](statements.md#the-goto-statement)) dentro del ámbito de la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-184">A label can be referenced from `goto` statements ([The goto statement](statements.md#the-goto-statement)) within the scope of the label.</span></span> <span data-ttu-id="82896-185">Esto significa que `goto` las instrucciones pueden transferir el control dentro de los bloques y fuera de los bloques, pero nunca a los bloques.</span><span class="sxs-lookup"><span data-stu-id="82896-185">This means that `goto` statements can transfer control within blocks and out of blocks, but never into blocks.</span></span>

<span data-ttu-id="82896-186">Las etiquetas tienen su propio espacio de declaración y no interfieren con otros identificadores.</span><span class="sxs-lookup"><span data-stu-id="82896-186">Labels have their own declaration space and do not interfere with other identifiers.</span></span> <span data-ttu-id="82896-187">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-187">The example</span></span>
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
<span data-ttu-id="82896-188">es válido y usa el nombre `x` como un parámetro y una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-188">is valid and uses the name `x` as both a parameter and a label.</span></span>

<span data-ttu-id="82896-189">La ejecución de una instrucción con etiqueta corresponde exactamente a la ejecución de la instrucción que sigue a la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-189">Execution of a labeled statement corresponds exactly to execution of the statement following the label.</span></span>

<span data-ttu-id="82896-190">Además de la disponibilidad proporcionada por el flujo de control normal, se puede tener acceso a una instrucción con etiqueta si se hace referencia a la etiqueta mediante una `goto` instrucción alcanzable.</span><span class="sxs-lookup"><span data-stu-id="82896-190">In addition to the reachability provided by normal flow of control, a labeled statement is reachable if the label is referenced by a reachable `goto` statement.</span></span> <span data-ttu-id="82896-191">Excepcional Si una `goto` instrucción está dentro de `try` un que incluye `finally` un bloque y la instrucción con etiqueta está fuera `finally` de `try`, y no se puede tener acceso al punto final del bloque, la instrucción con etiqueta no es accesible desde esa `goto` instrucción).</span><span class="sxs-lookup"><span data-stu-id="82896-191">(Exception: If a `goto` statement is inside a `try` that includes a `finally` block, and the labeled statement is outside the `try`, and the end point of the `finally` block is unreachable, then the labeled statement is not reachable from that `goto` statement.)</span></span>

## <a name="declaration-statements"></a><span data-ttu-id="82896-192">Instrucciones de declaración</span><span class="sxs-lookup"><span data-stu-id="82896-192">Declaration statements</span></span>

<span data-ttu-id="82896-193">Un *declaration_statement* declara una variable o constante local.</span><span class="sxs-lookup"><span data-stu-id="82896-193">A *declaration_statement* declares a local variable or constant.</span></span> <span data-ttu-id="82896-194">Las instrucciones de declaración se permiten en bloques, pero no se permiten como instrucciones incrustadas.</span><span class="sxs-lookup"><span data-stu-id="82896-194">Declaration statements are permitted in blocks, but are not permitted as embedded statements.</span></span>

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a><span data-ttu-id="82896-195">Declaraciones de variables locales</span><span class="sxs-lookup"><span data-stu-id="82896-195">Local variable declarations</span></span>

<span data-ttu-id="82896-196">Un *local_variable_declaration* declara una o varias variables locales.</span><span class="sxs-lookup"><span data-stu-id="82896-196">A *local_variable_declaration* declares one or more local variables.</span></span>

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

<span data-ttu-id="82896-197">El *local_variable_type* de un *local_variable_declaration* especifica directamente el tipo de las variables introducidas por la declaración, o indica con el identificador `var` que el tipo se debe inferir en función de un inicializador.</span><span class="sxs-lookup"><span data-stu-id="82896-197">The *local_variable_type* of a *local_variable_declaration* either directly specifies the type of the variables introduced by the declaration, or indicates with the identifier `var` that the type should be inferred based on an initializer.</span></span> <span data-ttu-id="82896-198">El tipo va seguido de una lista de *local_variable_declarator*s, cada uno de los cuales introduce una nueva variable.</span><span class="sxs-lookup"><span data-stu-id="82896-198">The type is followed by a list of *local_variable_declarator*s, each of which introduces a new variable.</span></span> <span data-ttu-id="82896-199">Un *local_variable_declarator* consta de un *identificador* que asigna un nombre a la variable, seguido opcionalmente por un token "`=`" y un *local_variable_initializer* que proporciona el valor inicial de la variable.</span><span class="sxs-lookup"><span data-stu-id="82896-199">A *local_variable_declarator* consists of an *identifier* that names the variable, optionally followed by an "`=`" token and a *local_variable_initializer* that gives the initial value of the variable.</span></span>

<span data-ttu-id="82896-200">En el contexto de una declaración de variable local, el identificador var actúa como una palabra clave contextual ([palabras clave](lexical-structure.md#keywords)). Cuando *local_variable_type* se especifica como `var` y ningún tipo denominado `var` está en el ámbito, la declaración es una ***declaración de variable local con tipo implícito***, cuyo tipo se deduce del tipo de la expresión de inicializador asociada.</span><span class="sxs-lookup"><span data-stu-id="82896-200">In the context of a local variable declaration, the identifier var acts as a contextual keyword ([Keywords](lexical-structure.md#keywords)).When the *local_variable_type* is specified as `var` and no type named `var` is in scope, the declaration is an ***implicitly typed local variable declaration***, whose type is inferred from the type of the associated initializer expression.</span></span> <span data-ttu-id="82896-201">Las declaraciones de variables locales con tipo implícito están sujetas a las siguientes restricciones:</span><span class="sxs-lookup"><span data-stu-id="82896-201">Implicitly typed local variable declarations are subject to the following restrictions:</span></span>

*  <span data-ttu-id="82896-202">*Local_variable_declaration* no puede incluir varios *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="82896-202">The *local_variable_declaration* cannot include multiple *local_variable_declarator*s.</span></span>
*  <span data-ttu-id="82896-203">*Local_variable_declarator* debe incluir un *local_variable_initializer*.</span><span class="sxs-lookup"><span data-stu-id="82896-203">The *local_variable_declarator* must include a *local_variable_initializer*.</span></span>
*  <span data-ttu-id="82896-204">*Local_variable_initializer* debe ser una *expresión*.</span><span class="sxs-lookup"><span data-stu-id="82896-204">The *local_variable_initializer* must be an *expression*.</span></span>
*  <span data-ttu-id="82896-205">La *expresión* de inicializador debe tener un tipo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-205">The initializer *expression* must have a compile-time type.</span></span>
*  <span data-ttu-id="82896-206">La *expresión* de inicializador no puede hacer referencia a la variable declarada en sí misma</span><span class="sxs-lookup"><span data-stu-id="82896-206">The initializer *expression* cannot refer to the declared variable itself</span></span>

<span data-ttu-id="82896-207">A continuación se muestran ejemplos de declaraciones de variables locales con tipo implícito incorrectas:</span><span class="sxs-lookup"><span data-stu-id="82896-207">The following are examples of incorrect implicitly typed local variable declarations:</span></span>

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

<span data-ttu-id="82896-208">El valor de una variable local se obtiene en una expresión usando *simple_name* ([nombres simples](expressions.md#simple-names)) y el valor de una variable local se modifica mediante una *asignación* (operadores de[asignación](expressions.md#assignment-operators)).</span><span class="sxs-lookup"><span data-stu-id="82896-208">The value of a local variable is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)), and the value of a local variable is modified using an *assignment* ([Assignment operators](expressions.md#assignment-operators)).</span></span> <span data-ttu-id="82896-209">Una variable local debe estar asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) en cada ubicación donde se obtiene su valor.</span><span class="sxs-lookup"><span data-stu-id="82896-209">A local variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) at each location where its value is obtained.</span></span>

<span data-ttu-id="82896-210">El ámbito de una variable local declarada en un *local_variable_declaration* es el bloque en el que se produce la declaración.</span><span class="sxs-lookup"><span data-stu-id="82896-210">The scope of a local variable declared in a *local_variable_declaration* is the block in which the declaration occurs.</span></span> <span data-ttu-id="82896-211">Es un error hacer referencia a una variable local en una posición textual que precede al *local_variable_declarator* de la variable local.</span><span class="sxs-lookup"><span data-stu-id="82896-211">It is an error to refer to a local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="82896-212">Dentro del ámbito de una variable local, se trata de un error en tiempo de compilación para declarar otra variable o constante local con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="82896-212">Within the scope of a local variable, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="82896-213">Una declaración de variable local que declara varias variables es equivalente a varias declaraciones de variables únicas con el mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="82896-213">A local variable declaration that declares multiple variables is equivalent to multiple declarations of single variables with the same type.</span></span> <span data-ttu-id="82896-214">Además, un inicializador de variable en una declaración de variable local se corresponde exactamente con una instrucción de asignación que se inserta inmediatamente después de la declaración.</span><span class="sxs-lookup"><span data-stu-id="82896-214">Furthermore, a variable initializer in a local variable declaration corresponds exactly to an assignment statement that is inserted immediately after the declaration.</span></span>

<span data-ttu-id="82896-215">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-215">The example</span></span>
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
<span data-ttu-id="82896-216">corresponde exactamente a</span><span class="sxs-lookup"><span data-stu-id="82896-216">corresponds exactly to</span></span>
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

<span data-ttu-id="82896-217">En una declaración de variable local con tipo implícito, el tipo de la variable local que se está declarando se toma como el mismo tipo de la expresión que se usa para inicializar la variable.</span><span class="sxs-lookup"><span data-stu-id="82896-217">In an implicitly typed local variable declaration, the type of the local variable being declared is taken to be the same as the type of the expression used to initialize the variable.</span></span> <span data-ttu-id="82896-218">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="82896-218">For example:</span></span>
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

<span data-ttu-id="82896-219">Las declaraciones de variables locales con tipo implícito anterior son exactamente equivalentes a las siguientes declaraciones con tipo explícito:</span><span class="sxs-lookup"><span data-stu-id="82896-219">The implicitly typed local variable declarations above are precisely equivalent to the following explicitly typed declarations:</span></span>
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a><span data-ttu-id="82896-220">Declaraciones de constantes locales</span><span class="sxs-lookup"><span data-stu-id="82896-220">Local constant declarations</span></span>

<span data-ttu-id="82896-221">Un *local_constant_declaration* declara una o más constantes locales.</span><span class="sxs-lookup"><span data-stu-id="82896-221">A *local_constant_declaration* declares one or more local constants.</span></span>

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

<span data-ttu-id="82896-222">El *tipo* de *local_constant_declaration* especifica el tipo de las constantes introducidas por la declaración.</span><span class="sxs-lookup"><span data-stu-id="82896-222">The *type* of a *local_constant_declaration* specifies the type of the constants introduced by the declaration.</span></span> <span data-ttu-id="82896-223">El tipo va seguido de una lista de *constant_declarator*s, cada uno de los cuales introduce una nueva constante.</span><span class="sxs-lookup"><span data-stu-id="82896-223">The type is followed by a list of *constant_declarator*s, each of which introduces a new constant.</span></span> <span data-ttu-id="82896-224">Un *constant_declarator* consta de un *identificador* que nombra la constante, seguido de un token "`=`", seguido de un *constant_expression* ([expresiones constantes](expressions.md#constant-expressions)) que proporciona el valor de la constante.</span><span class="sxs-lookup"><span data-stu-id="82896-224">A *constant_declarator* consists of an *identifier* that names the constant, followed by an "`=`" token, followed by a *constant_expression* ([Constant expressions](expressions.md#constant-expressions)) that gives the value of the constant.</span></span>

<span data-ttu-id="82896-225">El *tipo* y *constant_expression* de una declaración de constante local deben seguir las mismas reglas que las de una declaración de miembro constante ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="82896-225">The *type* and *constant_expression* of a local constant declaration must follow the same rules as those of a constant member declaration ([Constants](classes.md#constants)).</span></span>

<span data-ttu-id="82896-226">El valor de una constante local se obtiene en una expresión usando *simple_name* ([nombres simples](expressions.md#simple-names)).</span><span class="sxs-lookup"><span data-stu-id="82896-226">The value of a local constant is obtained in an expression using a *simple_name* ([Simple names](expressions.md#simple-names)).</span></span>

<span data-ttu-id="82896-227">El ámbito de una constante local es el bloque en el que se produce la declaración.</span><span class="sxs-lookup"><span data-stu-id="82896-227">The scope of a local constant is the block in which the declaration occurs.</span></span> <span data-ttu-id="82896-228">Es un error hacer referencia a una constante local en una posición textual que precede a su *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="82896-228">It is an error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span> <span data-ttu-id="82896-229">Dentro del ámbito de una constante local, es un error en tiempo de compilación declarar otra variable o constante local con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="82896-229">Within the scope of a local constant, it is a compile-time error to declare another local variable or constant with the same name.</span></span>

<span data-ttu-id="82896-230">Una declaración de constante local que declara varias constantes es equivalente a varias declaraciones de constantes únicas con el mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="82896-230">A local constant declaration that declares multiple constants is equivalent to multiple declarations of single constants with the same type.</span></span>

## <a name="expression-statements"></a><span data-ttu-id="82896-231">Instrucciones de expresión</span><span class="sxs-lookup"><span data-stu-id="82896-231">Expression statements</span></span>

<span data-ttu-id="82896-232">Un *expression_statement* evalúa una expresión determinada.</span><span class="sxs-lookup"><span data-stu-id="82896-232">An *expression_statement* evaluates a given expression.</span></span> <span data-ttu-id="82896-233">El valor calculado por la expresión, si existe, se descarta.</span><span class="sxs-lookup"><span data-stu-id="82896-233">The value computed by the expression, if any, is discarded.</span></span>

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

<span data-ttu-id="82896-234">No todas las expresiones se permiten como instrucciones.</span><span class="sxs-lookup"><span data-stu-id="82896-234">Not all expressions are permitted as statements.</span></span> <span data-ttu-id="82896-235">En concreto, las expresiones `x + y` como y `x == 1` que simplemente calculan un valor (que se descartarán) no se permiten como instrucciones.</span><span class="sxs-lookup"><span data-stu-id="82896-235">In particular, expressions such as `x + y` and `x == 1` that merely compute a value (which will be discarded), are not permitted as statements.</span></span>

<span data-ttu-id="82896-236">La ejecución de un *expression_statement* evalúa la expresión contenida y, a continuación, transfiere el control al punto final de *expression_statement*.</span><span class="sxs-lookup"><span data-stu-id="82896-236">Execution of an *expression_statement* evaluates the contained expression and then transfers control to the end point of the *expression_statement*.</span></span> <span data-ttu-id="82896-237">El punto final de un *expression_statement* es accesible si ese *expression_statement* es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-237">The end point of an *expression_statement* is reachable if that *expression_statement* is reachable.</span></span>

## <a name="selection-statements"></a><span data-ttu-id="82896-238">Instrucciones de selección</span><span class="sxs-lookup"><span data-stu-id="82896-238">Selection statements</span></span>

<span data-ttu-id="82896-239">Las instrucciones de selección seleccionan una de varias instrucciones posibles para su ejecución según el valor de alguna expresión.</span><span class="sxs-lookup"><span data-stu-id="82896-239">Selection statements select one of a number of possible statements for execution based on the value of some expression.</span></span>

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a><span data-ttu-id="82896-240">La instrucción If</span><span class="sxs-lookup"><span data-stu-id="82896-240">The if statement</span></span>

<span data-ttu-id="82896-241">La `if` instrucción selecciona una instrucción para su ejecución basada en el valor de una expresión booleana.</span><span class="sxs-lookup"><span data-stu-id="82896-241">The `if` statement selects a statement for execution based on the value of a boolean expression.</span></span>

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

<span data-ttu-id="82896-242">Un `else` elemento está asociado a la parte anterior `if` léxicamente más cercana permitida por la sintaxis.</span><span class="sxs-lookup"><span data-stu-id="82896-242">An `else` part is associated with the lexically nearest preceding `if` that is allowed by the syntax.</span></span> <span data-ttu-id="82896-243">Por lo tanto `if` , una instrucción con el formato</span><span class="sxs-lookup"><span data-stu-id="82896-243">Thus, an `if` statement of the form</span></span>
```csharp
if (x) if (y) F(); else G();
```
<span data-ttu-id="82896-244">es equivalente a</span><span class="sxs-lookup"><span data-stu-id="82896-244">is equivalent to</span></span>
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

<span data-ttu-id="82896-245">Una `if` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-245">An `if` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-246">Se evalúa *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="82896-246">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="82896-247">Si la expresión booleana produce `true`, el control se transfiere a la primera instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="82896-247">If the boolean expression yields `true`, control is transferred to the first embedded statement.</span></span> <span data-ttu-id="82896-248">Cuando y si el control alcanza el punto final de la instrucción, el control se transfiere al punto final de `if` la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-248">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="82896-249">Si la expresión booleana produce `false` y si un `else` elemento está presente, el control se transfiere a la segunda instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="82896-249">If the boolean expression yields `false` and if an `else` part is present, control is transferred to the second embedded statement.</span></span> <span data-ttu-id="82896-250">Cuando y si el control alcanza el punto final de la instrucción, el control se transfiere al punto final de `if` la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-250">When and if control reaches the end point of that statement, control is transferred to the end point of the `if` statement.</span></span>
*  <span data-ttu-id="82896-251">Si la expresión booleana produce `false` y si un `else` elemento no está presente, el control se transfiere al punto final de la `if` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-251">If the boolean expression yields `false` and if an `else` part is not present, control is transferred to the end point of the `if` statement.</span></span>

<span data-ttu-id="82896-252">La primera instrucción incrustada de `if` una instrucción es accesible si la `if` instrucción es accesible y la expresión booleana no tiene el valor `false`constante.</span><span class="sxs-lookup"><span data-stu-id="82896-252">The first embedded statement of an `if` statement is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="82896-253">La segunda instrucción incrustada de `if` una instrucción, si está presente, es accesible si `if` la instrucción es accesible y la expresión booleana no tiene el valor `true`constante.</span><span class="sxs-lookup"><span data-stu-id="82896-253">The second embedded statement of an `if` statement, if present, is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

<span data-ttu-id="82896-254">El punto final de una `if` instrucción es accesible si el punto final de al menos una de sus instrucciones incrustadas es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-254">The end point of an `if` statement is reachable if the end point of at least one of its embedded statements is reachable.</span></span> <span data-ttu-id="82896-255">Además, el `if` punto final de una instrucción sin ninguna `else` parte es accesible si la `if` instrucción es accesible y la expresión booleana no tiene el valor `true`constante.</span><span class="sxs-lookup"><span data-stu-id="82896-255">In addition, the end point of an `if` statement with no `else` part is reachable if the `if` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-switch-statement"></a><span data-ttu-id="82896-256">La instrucción switch</span><span class="sxs-lookup"><span data-stu-id="82896-256">The switch statement</span></span>

<span data-ttu-id="82896-257">La instrucción switch selecciona la ejecución de una lista de instrucciones que tiene una etiqueta de conmutador asociada que corresponde al valor de la expresión switch.</span><span class="sxs-lookup"><span data-stu-id="82896-257">The switch statement selects for execution a statement list having an associated switch label that corresponds to the value of the switch expression.</span></span>

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

<span data-ttu-id="82896-258">Un *switch_statement* consta de la palabra clave `switch`, seguido de una expresión entre paréntesis (denominada expresión switch), seguida de un *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="82896-258">A *switch_statement* consists of the keyword `switch`, followed by a parenthesized expression (called the switch expression), followed by a *switch_block*.</span></span> <span data-ttu-id="82896-259">*Switch_block* consta de cero o más *switch_section*s, entre llaves.</span><span class="sxs-lookup"><span data-stu-id="82896-259">The *switch_block* consists of zero or more *switch_section*s, enclosed in braces.</span></span> <span data-ttu-id="82896-260">Cada *switch_section* consta de una o más *switch_label*s seguidos de una *statement_list* ([listas de instrucciones](statements.md#statement-lists)).</span><span class="sxs-lookup"><span data-stu-id="82896-260">Each *switch_section* consists of one or more *switch_label*s followed by a *statement_list* ([Statement lists](statements.md#statement-lists)).</span></span>

<span data-ttu-id="82896-261">El ***tipo de control*** de `switch` una instrucción se establece mediante la expresión switch.</span><span class="sxs-lookup"><span data-stu-id="82896-261">The ***governing type*** of a `switch` statement is established by the switch expression.</span></span>

*  <span data-ttu-id="82896-262">Si el tipo de la expresión switch es `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, 0 o un *enum_type*, o si es el tipo que acepta valores NULL correspondiente a uno de estos tipos , que es el tipo de control de la instrucción 2.</span><span class="sxs-lookup"><span data-stu-id="82896-262">If the type of the switch expression is `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, or an *enum_type*, or if it is the nullable type corresponding to one of these types, then that is the governing type of the `switch` statement.</span></span>
*  <span data-ttu-id="82896-263">De lo contrario, debe existir exactamente una conversión implícita definida por el usuario ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)) del tipo de la expresión switch a uno de los siguientes tipos de `sbyte`control `byte`posibles: `ushort` ,, `short`, , `int`, `uint`, ,`long` ,`char`,o, un tipo que acepta valores NULL correspondiente a uno de esos tipos. `ulong` `string`</span><span class="sxs-lookup"><span data-stu-id="82896-263">Otherwise, exactly one user-defined implicit conversion ([User-defined conversions](conversions.md#user-defined-conversions)) must exist from the type of the switch expression to one of the following possible governing types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, or,  a nullable type corresponding to one of those types.</span></span>
*  <span data-ttu-id="82896-264">De lo contrario, si no existe ninguna conversión implícita, o si existe más de una conversión implícita de este tipo, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-264">Otherwise, if no such implicit conversion exists, or if more than one such implicit conversion exists, a compile-time error occurs.</span></span>

<span data-ttu-id="82896-265">La expresión constante de cada `case` etiqueta debe indicar un valor que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo aplicable de la `switch` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-265">The constant expression of each `case` label must denote a value that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the `switch` statement.</span></span> <span data-ttu-id="82896-266">Se produce un error en tiempo de compilación si dos `case` o más etiquetas de `switch` la misma instrucción especifican el mismo valor constante.</span><span class="sxs-lookup"><span data-stu-id="82896-266">A compile-time error occurs if two or more `case` labels in the same `switch` statement specify the same constant value.</span></span>

<span data-ttu-id="82896-267">Puede haber como máximo `default` una etiqueta en una instrucción switch.</span><span class="sxs-lookup"><span data-stu-id="82896-267">There can be at most one `default` label in a switch statement.</span></span>

<span data-ttu-id="82896-268">Una `switch` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-268">A `switch` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-269">La expresión switch se evalúa y se convierte al tipo aplicable.</span><span class="sxs-lookup"><span data-stu-id="82896-269">The switch expression is evaluated and converted to the governing type.</span></span>
*  <span data-ttu-id="82896-270">Si una de las constantes especificadas en una `case` etiqueta en la misma `switch` instrucción es igual al valor de la expresión switch, el control se transfiere a la `case` lista de instrucciones que sigue a la etiqueta coincidente.</span><span class="sxs-lookup"><span data-stu-id="82896-270">If one of the constants specified in a `case` label in the same `switch` statement is equal to the value of the switch expression, control is transferred to the statement list following the matched `case` label.</span></span>
*  <span data-ttu-id="82896-271">Si ninguna de las constantes especificadas en `case` las etiquetas de la `switch` misma instrucción es igual al valor de la expresión switch y, si hay `default` una etiqueta, el control se transfiere a la lista de instrucciones que `default` sigue a la rótulo.</span><span class="sxs-lookup"><span data-stu-id="82896-271">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if a `default` label is present, control is transferred to the statement list following the `default` label.</span></span>
*  <span data-ttu-id="82896-272">Si ninguna de las constantes especificadas en `case` las etiquetas de la `switch` misma instrucción es igual al valor de la expresión switch y no hay ninguna `default` etiqueta, el control se transfiere al punto final de la `switch` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-272">If none of the constants specified in `case` labels in the same `switch` statement is equal to the value of the switch expression, and if no `default` label is present, control is transferred to the end point of the `switch` statement.</span></span>

<span data-ttu-id="82896-273">Si el punto final de la lista de instrucciones de una sección switch es accesible, se producirá un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-273">If the end point of the statement list of a switch section is reachable, a compile-time error occurs.</span></span> <span data-ttu-id="82896-274">Esto se conoce como la regla "no pasar de un paso".</span><span class="sxs-lookup"><span data-stu-id="82896-274">This is known as the "no fall through" rule.</span></span> <span data-ttu-id="82896-275">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-275">The example</span></span>
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
<span data-ttu-id="82896-276">es válido porque ninguna sección del modificador tiene un punto final alcanzable.</span><span class="sxs-lookup"><span data-stu-id="82896-276">is valid because no switch section has a reachable end point.</span></span> <span data-ttu-id="82896-277">A diferencia de C C++y, no se permite la ejecución de una sección switch a la siguiente sección switch y el ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-277">Unlike C and C++, execution of a switch section is not permitted to "fall through" to the next switch section, and the example</span></span>
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
<span data-ttu-id="82896-278">produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-278">results in a compile-time error.</span></span> <span data-ttu-id="82896-279">Cuando la ejecución de una sección switch va seguida de la ejecución de otra sección switch, se debe `goto case` usar `goto default` una instrucción or explícita:</span><span class="sxs-lookup"><span data-stu-id="82896-279">When execution of a switch section is to be followed by execution of another switch section, an explicit `goto case` or `goto default` statement must be used:</span></span>
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

<span data-ttu-id="82896-280">Se permiten varias etiquetas en un *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="82896-280">Multiple labels are permitted in a *switch_section*.</span></span> <span data-ttu-id="82896-281">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-281">The example</span></span>
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
<span data-ttu-id="82896-282">es válido.</span><span class="sxs-lookup"><span data-stu-id="82896-282">is valid.</span></span> <span data-ttu-id="82896-283">En el ejemplo no se infringe la regla "no hay que pasar" porque las etiquetas `case 2:` y `default:` forman parte del mismo *switch_section*.</span><span class="sxs-lookup"><span data-stu-id="82896-283">The example does not violate the "no fall through" rule because the labels `case 2:` and `default:` are part of the same *switch_section*.</span></span>

<span data-ttu-id="82896-284">La regla "no pasar" evita una clase común de errores que se producen en C y C++ cuando `break` las instrucciones se omiten accidentalmente.</span><span class="sxs-lookup"><span data-stu-id="82896-284">The "no fall through" rule prevents a common class of bugs that occur in C and C++ when `break` statements are accidentally omitted.</span></span> <span data-ttu-id="82896-285">Además, debido a esta regla, las secciones switch de una `switch` instrucción se pueden reorganizar arbitrariamente sin afectar al comportamiento de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-285">In addition, because of this rule, the switch sections of a `switch` statement can be arbitrarily rearranged without affecting the behavior of the statement.</span></span> <span data-ttu-id="82896-286">Por ejemplo, las secciones de la `switch` instrucción anterior se pueden revertir sin afectar al comportamiento de la instrucción:</span><span class="sxs-lookup"><span data-stu-id="82896-286">For example, the sections of the `switch` statement above can be reversed without affecting the behavior of the statement:</span></span>
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

<span data-ttu-id="82896-287">La lista de instrucciones de una sección switch normalmente finaliza en `break`una `goto case`instrucción, `goto default` o, pero se permite cualquier construcción que representa el punto final de la lista de instrucciones inaccesible.</span><span class="sxs-lookup"><span data-stu-id="82896-287">The statement list of a switch section typically ends in a `break`, `goto case`, or `goto default` statement, but any construct that renders the end point of the statement list unreachable is permitted.</span></span> <span data-ttu-id="82896-288">Por ejemplo, se `while` sabe que una instrucción controlada por `true` la expresión booleana nunca alcanza su punto final.</span><span class="sxs-lookup"><span data-stu-id="82896-288">For example, a `while` statement controlled by the boolean expression `true` is known to never reach its end point.</span></span> <span data-ttu-id="82896-289">Del mismo modo `throw` , `return` una instrucción o siempre transfiere el control en otro lugar y nunca alcanza su punto final.</span><span class="sxs-lookup"><span data-stu-id="82896-289">Likewise, a `throw` or `return` statement always transfers control elsewhere and never reaches its end point.</span></span> <span data-ttu-id="82896-290">Por lo tanto, el ejemplo siguiente es válido:</span><span class="sxs-lookup"><span data-stu-id="82896-290">Thus, the following example is valid:</span></span>
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

<span data-ttu-id="82896-291">El tipo de control de `switch` una instrucción puede ser el `string`tipo.</span><span class="sxs-lookup"><span data-stu-id="82896-291">The governing type of a `switch` statement may be the type `string`.</span></span> <span data-ttu-id="82896-292">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="82896-292">For example:</span></span>
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

<span data-ttu-id="82896-293">Al igual que los operadores de igualdad de cadena (operadores de `switch` igualdad de[cadena](expressions.md#string-equality-operators)), la instrucción distingue entre mayúsculas y minúsculas y ejecutará una sección de `case` modificador determinada solo si la cadena de expresión de modificador coincide exactamente con una constante de etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-293">Like the string equality operators ([String equality operators](expressions.md#string-equality-operators)), the `switch` statement is case sensitive and will execute a given switch section only if the switch expression string exactly matches a `case` label constant.</span></span>

<span data-ttu-id="82896-294">Cuando el tipo de control de `switch` una instrucción `string`es, se `null` permite el valor como una constante de etiqueta de caso.</span><span class="sxs-lookup"><span data-stu-id="82896-294">When the governing type of a `switch` statement is `string`, the value `null` is permitted as a case label constant.</span></span>

<span data-ttu-id="82896-295">*Statement_list*s de un *switch_block* puede contener instrucciones de declaración ([instrucciones de declaración](statements.md#declaration-statements)).</span><span class="sxs-lookup"><span data-stu-id="82896-295">The *statement_list*s of a *switch_block* may contain declaration statements ([Declaration statements](statements.md#declaration-statements)).</span></span> <span data-ttu-id="82896-296">El ámbito de una variable local o constante declarada en un bloque switch es el bloque switch.</span><span class="sxs-lookup"><span data-stu-id="82896-296">The scope of a local variable or constant declared in a switch block is the switch block.</span></span>

<span data-ttu-id="82896-297">La lista de instrucciones de una sección de modificador determinada es accesible `switch` si la instrucción es accesible y se cumple al menos una de las siguientes condiciones:</span><span class="sxs-lookup"><span data-stu-id="82896-297">The statement list of a given switch section is reachable if the `switch` statement is reachable and at least one of the following is true:</span></span>

*  <span data-ttu-id="82896-298">La expresión switch es un valor que no es constante.</span><span class="sxs-lookup"><span data-stu-id="82896-298">The switch expression is a non-constant value.</span></span>
*  <span data-ttu-id="82896-299">La expresión switch es un valor constante que coincide con `case` una etiqueta en la sección switch.</span><span class="sxs-lookup"><span data-stu-id="82896-299">The switch expression is a constant value that matches a `case` label in the switch section.</span></span>
*  <span data-ttu-id="82896-300">La expresión switch es un valor constante que no coincide con `case` ninguna etiqueta y la sección switch contiene la `default` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-300">The switch expression is a constant value that doesn't match any `case` label, and the switch section contains the `default` label.</span></span>
*  <span data-ttu-id="82896-301">Se hace referencia a una etiqueta switch de la sección switch mediante una instrucción `goto case` o `goto default` alcanzable.</span><span class="sxs-lookup"><span data-stu-id="82896-301">A switch label of the switch section is referenced by a reachable `goto case` or `goto default` statement.</span></span>

<span data-ttu-id="82896-302">El punto final de una `switch` instrucción es accesible si se cumple al menos una de las siguientes condiciones:</span><span class="sxs-lookup"><span data-stu-id="82896-302">The end point of a `switch` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="82896-303">La `switch` instrucción contiene una `break` instrucción accesible que sale de la `switch` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-303">The `switch` statement contains a reachable `break` statement that exits the `switch` statement.</span></span>
*  <span data-ttu-id="82896-304">La `switch` instrucción es accesible, la expresión switch es un valor no constante y no hay ninguna `default` etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-304">The `switch` statement is reachable, the switch expression is a non-constant value, and no `default` label is present.</span></span>
*  <span data-ttu-id="82896-305">La `switch` instrucción es accesible, la expresión switch es un valor constante que no coincide con ninguna `case` etiqueta y no hay `default` ninguna etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-305">The `switch` statement is reachable, the switch expression is a constant value that doesn't match any `case` label, and no `default` label is present.</span></span>

## <a name="iteration-statements"></a><span data-ttu-id="82896-306">Instrucciones de iteración</span><span class="sxs-lookup"><span data-stu-id="82896-306">Iteration statements</span></span>

<span data-ttu-id="82896-307">Las instrucciones de iteración ejecutan repetidamente una instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="82896-307">Iteration statements repeatedly execute an embedded statement.</span></span>

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a><span data-ttu-id="82896-308">La instrucción while</span><span class="sxs-lookup"><span data-stu-id="82896-308">The while statement</span></span>

<span data-ttu-id="82896-309">La `while` instrucción ejecuta condicionalmente una instrucción incrustada cero o más veces.</span><span class="sxs-lookup"><span data-stu-id="82896-309">The `while` statement conditionally executes an embedded statement zero or more times.</span></span>

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

<span data-ttu-id="82896-310">Una `while` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-310">A `while` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-311">Se evalúa *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="82896-311">The *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span>
*  <span data-ttu-id="82896-312">Si la expresión booleana produce `true`, el control se transfiere a la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="82896-312">If the boolean expression yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="82896-313">Cuando el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de `continue` una instrucción), el control se transfiere al principio de `while` la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-313">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), control is transferred to the beginning of the `while` statement.</span></span>
*  <span data-ttu-id="82896-314">Si la expresión booleana produce `false`, el control se transfiere al punto final de la `while` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-314">If the boolean expression yields `false`, control is transferred to the end point of the `while` statement.</span></span>

<span data-ttu-id="82896-315">Dentro de la instrucción incrustada `while` de una instrucción `break` , se puede usar una instrucción ([la instrucción break](statements.md#the-break-statement)) para transferir el control al punto final `while` de la instrucción (por lo tanto, finalizar la iteración de la instrucción incrustada) y un `continue` la instrucción ([la instrucción continue](statements.md#the-continue-statement)) se puede utilizar para transferir el control al punto final de la instrucción incrustada (con lo que se `while` realiza otra iteración de la instrucción).</span><span class="sxs-lookup"><span data-stu-id="82896-315">Within the embedded statement of a `while` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `while` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus performing another iteration of the `while` statement).</span></span>

<span data-ttu-id="82896-316">La instrucción incrustada de `while` una instrucción es accesible si la `while` instrucción es accesible y la expresión booleana no tiene el valor `false`constante.</span><span class="sxs-lookup"><span data-stu-id="82896-316">The embedded statement of a `while` statement is reachable if the `while` statement is reachable and the boolean expression does not have the constant value `false`.</span></span>

<span data-ttu-id="82896-317">El punto final de una `while` instrucción es accesible si se cumple al menos una de las siguientes condiciones:</span><span class="sxs-lookup"><span data-stu-id="82896-317">The end point of a `while` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="82896-318">La `while` instrucción contiene una `break` instrucción accesible que sale de la `while` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-318">The `while` statement contains a reachable `break` statement that exits the `while` statement.</span></span>
*  <span data-ttu-id="82896-319">La `while` instrucción es accesible y la expresión booleana no tiene el valor `true`constante.</span><span class="sxs-lookup"><span data-stu-id="82896-319">The `while` statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-do-statement"></a><span data-ttu-id="82896-320">La instrucción do</span><span class="sxs-lookup"><span data-stu-id="82896-320">The do statement</span></span>

<span data-ttu-id="82896-321">La `do` instrucción ejecuta condicionalmente una instrucción incrustada una o varias veces.</span><span class="sxs-lookup"><span data-stu-id="82896-321">The `do` statement conditionally executes an embedded statement one or more times.</span></span>

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

<span data-ttu-id="82896-322">Una `do` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-322">A `do` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-323">El control se transfiere a la instrucción insertada.</span><span class="sxs-lookup"><span data-stu-id="82896-323">Control is transferred to the embedded statement.</span></span>
*  <span data-ttu-id="82896-324">Cuando el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de una instrucción `continue`), se evalúa la *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="82896-324">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)) is evaluated.</span></span> <span data-ttu-id="82896-325">Si la expresión booleana produce `true`, el control se transfiere al principio de la `do` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-325">If the boolean expression yields `true`, control is transferred to the beginning of the `do` statement.</span></span> <span data-ttu-id="82896-326">De lo contrario, el control se transfiere al punto final `do` de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-326">Otherwise, control is transferred to the end point of the `do` statement.</span></span>

<span data-ttu-id="82896-327">Dentro de la instrucción incrustada `do` de una instrucción `break` , se puede usar una instrucción ([la instrucción break](statements.md#the-break-statement)) para transferir el control al punto final `do` de la instrucción (por lo tanto, finalizar la iteración de la instrucción incrustada) y un `continue` la instrucción ([la instrucción continue](statements.md#the-continue-statement)) se puede utilizar para transferir el control al punto final de la instrucción insertada.</span><span class="sxs-lookup"><span data-stu-id="82896-327">Within the embedded statement of a `do` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `do` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement.</span></span>

<span data-ttu-id="82896-328">La instrucción insertada de `do` una instrucción es accesible si la `do` instrucción es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-328">The embedded statement of a `do` statement is reachable if the `do` statement is reachable.</span></span>

<span data-ttu-id="82896-329">El punto final de una `do` instrucción es accesible si se cumple al menos una de las siguientes condiciones:</span><span class="sxs-lookup"><span data-stu-id="82896-329">The end point of a `do` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="82896-330">La `do` instrucción contiene una `break` instrucción accesible que sale de la `do` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-330">The `do` statement contains a reachable `break` statement that exits the `do` statement.</span></span>
*  <span data-ttu-id="82896-331">El punto final de la instrucción incrustada es accesible y la expresión booleana no tiene el valor `true`constante.</span><span class="sxs-lookup"><span data-stu-id="82896-331">The end point of the embedded statement is reachable and the boolean expression does not have the constant value `true`.</span></span>

### <a name="the-for-statement"></a><span data-ttu-id="82896-332">La instrucción for</span><span class="sxs-lookup"><span data-stu-id="82896-332">The for statement</span></span>

<span data-ttu-id="82896-333">La `for` instrucción evalúa una secuencia de expresiones de inicialización y, a continuación, mientras una condición es true, ejecuta repetidamente una instrucción incrustada y evalúa una secuencia de expresiones de iteración.</span><span class="sxs-lookup"><span data-stu-id="82896-333">The `for` statement evaluates a sequence of initialization expressions and then, while a condition is true, repeatedly executes an embedded statement and evaluates a sequence of iteration expressions.</span></span>

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

<span data-ttu-id="82896-334">El *for_initializer*, si está presente, consta de un *local_variable_declaration* ([declaraciones de variables locales](statements.md#local-variable-declarations)) o una lista de *statement_expression*s ([instrucciones de expresión](statements.md#expression-statements)) separadas por comas.</span><span class="sxs-lookup"><span data-stu-id="82896-334">The *for_initializer*, if present, consists of either a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) or a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span> <span data-ttu-id="82896-335">El ámbito de una variable local declarada por un *for_initializer* comienza en el *local_variable_declarator* para la variable y se extiende hasta el final de la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="82896-335">The scope of a local variable declared by a *for_initializer* starts at the *local_variable_declarator* for the variable and extends to the end of the embedded statement.</span></span> <span data-ttu-id="82896-336">El ámbito incluye *for_condition* y *for_iterator*.</span><span class="sxs-lookup"><span data-stu-id="82896-336">The scope includes the *for_condition* and the *for_iterator*.</span></span>

<span data-ttu-id="82896-337">El *for_condition*, si está presente, debe ser una *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)).</span><span class="sxs-lookup"><span data-stu-id="82896-337">The *for_condition*, if present, must be a *boolean_expression* ([Boolean expressions](expressions.md#boolean-expressions)).</span></span>

<span data-ttu-id="82896-338">*For_iterator*, si está presente, consta de una lista de *statement_expression*s ([instrucciones de expresión](statements.md#expression-statements)) separadas por comas.</span><span class="sxs-lookup"><span data-stu-id="82896-338">The *for_iterator*, if present, consists of a list of *statement_expression*s ([Expression statements](statements.md#expression-statements)) separated by commas.</span></span>

<span data-ttu-id="82896-339">Una instrucción for se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-339">A for statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-340">Si hay un *for_initializer* , los inicializadores variables o las expresiones de instrucción se ejecutan en el orden en que se escriben.</span><span class="sxs-lookup"><span data-stu-id="82896-340">If a *for_initializer* is present, the variable initializers or statement expressions are executed in the order they are written.</span></span> <span data-ttu-id="82896-341">Este paso solo se realiza una vez.</span><span class="sxs-lookup"><span data-stu-id="82896-341">This step is only performed once.</span></span>
*  <span data-ttu-id="82896-342">Si un *for_condition* está presente, se evalúa.</span><span class="sxs-lookup"><span data-stu-id="82896-342">If a *for_condition* is present, it is evaluated.</span></span>
*  <span data-ttu-id="82896-343">Si *for_condition* no está presente o si la evaluación produce `true`, el control se transfiere a la instrucción insertada.</span><span class="sxs-lookup"><span data-stu-id="82896-343">If the *for_condition* is not present or if the evaluation yields `true`, control is transferred to the embedded statement.</span></span> <span data-ttu-id="82896-344">Cuando el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de una instrucción `continue`), las expresiones de *for_iterator*, si hay alguna, se evalúan en secuencia y, a continuación, se realiza otra iteración, empezando por evaluación de *for_condition* en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="82896-344">When and if control reaches the end point of the embedded statement (possibly from execution of a `continue` statement), the expressions of the *for_iterator*, if any, are evaluated in sequence, and then another iteration is performed, starting with evaluation of the *for_condition* in the step above.</span></span>
*  <span data-ttu-id="82896-345">Si el *for_condition* está presente y la evaluación produce `false`, el control se transfiere al punto final de la instrucción `for`.</span><span class="sxs-lookup"><span data-stu-id="82896-345">If the *for_condition* is present and the evaluation yields `false`, control is transferred to the end point of the `for` statement.</span></span>

<span data-ttu-id="82896-346">Dentro de la instrucción incrustada de una instrucción `for`, se puede usar una instrucción `break` ([instrucción break](statements.md#the-break-statement)) para transferir el control al punto final de la instrucción `for` (por lo tanto, finalizar la iteración de la instrucción incrustada) y una instrucción `continue` ([ La instrucción continue](statements.md#the-continue-statement)) se puede usar para transferir el control al punto final de la instrucción incrustada (con lo que se ejecuta *for_iterator* y se realiza otra iteración de la instrucción `for`, comenzando por *for_condition*).</span><span class="sxs-lookup"><span data-stu-id="82896-346">Within the embedded statement of a `for` statement, a `break` statement ([The break statement](statements.md#the-break-statement)) may be used to transfer control to the end point of the `for` statement (thus ending iteration of the embedded statement), and a `continue` statement ([The continue statement](statements.md#the-continue-statement)) may be used to transfer control to the end point of the embedded statement (thus executing the *for_iterator* and performing another iteration of the `for` statement, starting with the *for_condition*).</span></span>

<span data-ttu-id="82896-347">La instrucción insertada de `for` una instrucción es accesible si se cumple una de las siguientes condiciones:</span><span class="sxs-lookup"><span data-stu-id="82896-347">The embedded statement of a `for` statement is reachable if one of the following is true:</span></span>

*  <span data-ttu-id="82896-348">La instrucción `for` es accesible y no hay *for_condition* .</span><span class="sxs-lookup"><span data-stu-id="82896-348">The `for` statement is reachable and no *for_condition* is present.</span></span>
*  <span data-ttu-id="82896-349">La instrucción `for` es accesible y está presente un *for_condition* y no tiene el valor constante `false`.</span><span class="sxs-lookup"><span data-stu-id="82896-349">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `false`.</span></span>

<span data-ttu-id="82896-350">El punto final de una `for` instrucción es accesible si se cumple al menos una de las siguientes condiciones:</span><span class="sxs-lookup"><span data-stu-id="82896-350">The end point of a `for` statement is reachable if at least one of the following is true:</span></span>

*  <span data-ttu-id="82896-351">La `for` instrucción contiene una `break` instrucción accesible que sale de la `for` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-351">The `for` statement contains a reachable `break` statement that exits the `for` statement.</span></span>
*  <span data-ttu-id="82896-352">La instrucción `for` es accesible y está presente un *for_condition* y no tiene el valor constante `true`.</span><span class="sxs-lookup"><span data-stu-id="82896-352">The `for` statement is reachable and a *for_condition* is present and does not have the constant value `true`.</span></span>

### <a name="the-foreach-statement"></a><span data-ttu-id="82896-353">La instrucción foreach</span><span class="sxs-lookup"><span data-stu-id="82896-353">The foreach statement</span></span>

<span data-ttu-id="82896-354">La `foreach` instrucción enumera los elementos de una colección y ejecuta una instrucción incrustada para cada elemento de la colección.</span><span class="sxs-lookup"><span data-stu-id="82896-354">The `foreach` statement enumerates the elements of a collection, executing an embedded statement for each element of the collection.</span></span>

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

<span data-ttu-id="82896-355">El *tipo* y el *identificador* de una `foreach` instrucción declaran la ***variable de iteración*** de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-355">The *type* and *identifier* of a `foreach` statement declare the ***iteration variable*** of the statement.</span></span> <span data-ttu-id="82896-356">Si se proporciona el identificador `var` como *local_variable_type*y no hay ningún tipo denominado `var` en el ámbito, se dice que la variable de iteración es una ***variable de iteración con tipo implícito***y se considera que su tipo es el tipo de elemento de `foreach`. tal y como se especifica a continuación.</span><span class="sxs-lookup"><span data-stu-id="82896-356">If the `var` identifier is given as the *local_variable_type*, and no type named `var` is in scope, the iteration variable is said to be an ***implicitly typed iteration variable***, and its type is taken to be the element type of the `foreach` statement, as specified below.</span></span> <span data-ttu-id="82896-357">La variable de iteración corresponde a una variable local de solo lectura con un ámbito que se extiende a través de la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="82896-357">The iteration variable corresponds to a read-only local variable with a scope that extends over the embedded statement.</span></span> <span data-ttu-id="82896-358">Durante la ejecución de `foreach` una instrucción, la variable de iteración representa el elemento de colección para el que se está realizando actualmente una iteración.</span><span class="sxs-lookup"><span data-stu-id="82896-358">During execution of a `foreach` statement, the iteration variable represents the collection element for which an iteration is currently being performed.</span></span> <span data-ttu-id="82896-359">Se produce un error en tiempo de compilación si la instrucción incrustada intenta modificar la variable de iteración ( `++` a `--` través de la asignación o los operadores y) `ref` o `out` pasar la variable de iteración como un parámetro o.</span><span class="sxs-lookup"><span data-stu-id="82896-359">A compile-time error occurs if the embedded statement attempts to modify the iteration variable (via assignment or the `++` and `--` operators) or pass the iteration variable as a `ref` or `out` parameter.</span></span>

<span data-ttu-id="82896-360">En el siguiente, por motivos de `IEnumerable`brevedad `IEnumerator`, `IEnumerable<T>` , `IEnumerator<T>` y hacen referencia a los tipos correspondientes de los `System.Collections` espacios `System.Collections.Generic`de nombres y.</span><span class="sxs-lookup"><span data-stu-id="82896-360">In the following, for brevity, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` and `IEnumerator<T>` refer to the corresponding types in the namespaces `System.Collections` and `System.Collections.Generic`.</span></span>

<span data-ttu-id="82896-361">El procesamiento en tiempo de compilación de una instrucción foreach primero determina el ***tipo de colección***, el tipo de ***enumerador*** y el ***tipo de elemento*** de la expresión.</span><span class="sxs-lookup"><span data-stu-id="82896-361">The compile-time processing of a foreach statement first determines the ***collection type***, ***enumerator type*** and ***element type*** of the expression.</span></span> <span data-ttu-id="82896-362">Esta determinación se realiza de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-362">This determination proceeds as follows:</span></span>

*  <span data-ttu-id="82896-363">Si el tipo `X` de *expresión* es un tipo de matriz, hay una conversión de referencia implícita `X` de a `IEnumerable` la interfaz ( `System.Array` puesto que implementa esta interfaz).</span><span class="sxs-lookup"><span data-stu-id="82896-363">If the type `X` of *expression* is an array type then there is an implicit reference conversion from `X` to the `IEnumerable` interface (since `System.Array` implements this interface).</span></span> <span data-ttu-id="82896-364">El ***tipo de colección*** es `IEnumerable` la interfaz, el ***tipo de enumerador*** es `IEnumerator` la interfaz ***y el tipo de elemento es*** el tipo de `X`elemento del tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="82896-364">The ***collection type*** is the `IEnumerable` interface, the ***enumerator type*** is the `IEnumerator` interface and the ***element type*** is the element type of the array type `X`.</span></span>
*  <span data-ttu-id="82896-365">Si el tipo `X` de *expresión* es `dynamic` , hay una conversión implícita de la *expresión* a la `IEnumerable` interfaz ([conversiones dinámicas implícitas](conversions.md#implicit-dynamic-conversions)).</span><span class="sxs-lookup"><span data-stu-id="82896-365">If the type `X` of *expression* is `dynamic` then there is an implicit conversion from *expression* to the `IEnumerable` interface ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)).</span></span> <span data-ttu-id="82896-366">El ***tipo de colección*** es `IEnumerable` la interfaz y el ***tipo de enumerador*** es la `IEnumerator` interfaz.</span><span class="sxs-lookup"><span data-stu-id="82896-366">The ***collection type*** is the `IEnumerable` interface and the ***enumerator type*** is the `IEnumerator` interface.</span></span> <span data-ttu-id="82896-367">Si se proporciona el identificador `var` como *local_variable_type* , el tipo de ***elemento*** es `dynamic`, de lo contrario es `object`.</span><span class="sxs-lookup"><span data-stu-id="82896-367">If the `var` identifier is given as the *local_variable_type* then the ***element type*** is `dynamic`, otherwise it is `object`.</span></span>
*  <span data-ttu-id="82896-368">De lo contrario, determine `X` si el tipo `GetEnumerator` tiene un método adecuado:</span><span class="sxs-lookup"><span data-stu-id="82896-368">Otherwise, determine whether the type `X` has an appropriate `GetEnumerator` method:</span></span>
   * <span data-ttu-id="82896-369">Realiza la búsqueda de miembros en `X` el tipo con `GetEnumerator` el identificador y ningún argumento de tipo.</span><span class="sxs-lookup"><span data-stu-id="82896-369">Perform member lookup on the type `X` with identifier `GetEnumerator` and no type arguments.</span></span> <span data-ttu-id="82896-370">Si la búsqueda de miembros no produce una coincidencia, o produce una ambigüedad, o produce una coincidencia que no es un grupo de métodos, busque una interfaz enumerable como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="82896-370">If the member lookup does not produce a match, or it produces an ambiguity, or produces a match that is not a method group, check for an enumerable interface as described below.</span></span> <span data-ttu-id="82896-371">Se recomienda emitir una advertencia si la búsqueda de miembros produce cualquier cosa, excepto un grupo de métodos o ninguna coincidencia.</span><span class="sxs-lookup"><span data-stu-id="82896-371">It is recommended that a warning be issued if member lookup produces anything except a method group or no match.</span></span>
   * <span data-ttu-id="82896-372">Realizar la resolución de sobrecarga utilizando el grupo de métodos resultante y una lista de argumentos vacía.</span><span class="sxs-lookup"><span data-stu-id="82896-372">Perform overload resolution using the resulting method group and an empty argument list.</span></span> <span data-ttu-id="82896-373">Si la resolución de sobrecarga da como resultado ningún método aplicable, da como resultado una ambigüedad o tiene como resultado un único método mejor pero ese método es estático o no público, busque una interfaz enumerable como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="82896-373">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, check for an enumerable interface as described below.</span></span> <span data-ttu-id="82896-374">Se recomienda emitir una advertencia si la resolución de sobrecarga produce todo excepto un método de instancia pública inequívoco o ningún método aplicable.</span><span class="sxs-lookup"><span data-stu-id="82896-374">It is recommended that a warning be issued if overload resolution produces anything except an unambiguous public instance method or no applicable methods.</span></span>
   * <span data-ttu-id="82896-375">Si el tipo `E` `GetEnumerator` de valor devuelto del método no es una clase, una estructura o un tipo de interfaz, se genera un error y no se realiza ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="82896-375">If the return type `E` of the `GetEnumerator` method is not a class, struct or interface type, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="82896-376">La búsqueda de miembros se `E` realiza en con el `Current` identificador y sin argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="82896-376">Member lookup is performed on `E` with the identifier `Current` and no type arguments.</span></span> <span data-ttu-id="82896-377">Si la búsqueda de miembros no produce ninguna coincidencia, el resultado es un error o el resultado es cualquier cosa excepto una propiedad de instancia pública que permita la lectura, se genera un error y no se realiza ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="82896-377">If the member lookup produces no match, the result is an error, or the result is anything except a public instance property that permits reading, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="82896-378">La búsqueda de miembros se `E` realiza en con el `MoveNext` identificador y sin argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="82896-378">Member lookup is performed on `E` with the identifier `MoveNext` and no type arguments.</span></span> <span data-ttu-id="82896-379">Si la búsqueda de miembros no produce ninguna coincidencia, el resultado es un error o el resultado es cualquier cosa excepto un grupo de métodos, se genera un error y no se realiza ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="82896-379">If the member lookup produces no match, the result is an error, or the result is anything except a method group, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="82896-380">La resolución de sobrecarga se realiza en el grupo de métodos con una lista de argumentos vacía.</span><span class="sxs-lookup"><span data-stu-id="82896-380">Overload resolution is performed on the method group with an empty argument list.</span></span> <span data-ttu-id="82896-381">Si la resolución de sobrecarga da como resultado ningún método aplicable, da como resultado una ambigüedad o tiene como resultado un único método mejor pero ese método es estático o no público, o su tipo de `bool`valor devuelto no es, se genera un error y no se realiza ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="82896-381">If overload resolution results in no applicable methods, results in an ambiguity, or results in a single best method but that method is either static or not public, or its return type is not `bool`, an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="82896-382">El ***tipo*** de colección `X`es, el tipo de `E` ***enumerador*** es y el tipo de ***elemento*** es el `Current` tipo de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="82896-382">The ***collection type*** is `X`, the ***enumerator type*** is `E`, and the ***element type*** is the type of the `Current` property.</span></span>

*  <span data-ttu-id="82896-383">De lo contrario, compruebe si hay una interfaz enumerable:</span><span class="sxs-lookup"><span data-stu-id="82896-383">Otherwise, check for an enumerable interface:</span></span>
   * <span data-ttu-id="82896-384">Si entre todos los tipos `Ti` para los que hay una conversión implícita de `X` a `IEnumerable<Ti>`, hay un tipo `T` único, de modo `T` que no `dynamic` es y para todos los `Ti` demás hay un conversión implícita de `IEnumerable<T>` a `IEnumerable<Ti>`, el ***tipo de colección*** es la interfaz `IEnumerable<T>`, el ***tipo de enumerador*** es `IEnumerator<T>`la interfaz y el ***tipo de elemento*** es `T`.</span><span class="sxs-lookup"><span data-stu-id="82896-384">If among all the types `Ti` for which there is an implicit conversion from `X` to `IEnumerable<Ti>`, there is a unique type `T` such that `T` is not `dynamic` and for all the other `Ti` there is an implicit conversion from `IEnumerable<T>` to `IEnumerable<Ti>`, then the ***collection type*** is the interface `IEnumerable<T>`, the ***enumerator type*** is the interface `IEnumerator<T>`, and the ***element type*** is `T`.</span></span>
   * <span data-ttu-id="82896-385">De lo contrario, si hay más de un tipo `T`de este tipo, se genera un error y no se realiza ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="82896-385">Otherwise, if there is more than one such type `T`, then an error is produced and no further steps are taken.</span></span>
   * <span data-ttu-id="82896-386">De lo contrario, si hay una conversión implícita `X` de en `System.Collections.IEnumerable` la interfaz, el ***tipo de colección*** es esta interfaz, el ***tipo de enumerador*** es la interfaz `System.Collections.IEnumerator`y el ***tipo de elemento*** es `object`.</span><span class="sxs-lookup"><span data-stu-id="82896-386">Otherwise, if there is an implicit conversion from `X` to the `System.Collections.IEnumerable` interface, then the ***collection type*** is this interface, the ***enumerator type*** is the interface `System.Collections.IEnumerator`, and the ***element type*** is `object`.</span></span>
   * <span data-ttu-id="82896-387">De lo contrario, se genera un error y no se realiza ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="82896-387">Otherwise, an error is produced and no further steps are taken.</span></span>

<span data-ttu-id="82896-388">Los pasos anteriores, si son correctos, producen de forma inequívoca `C`un tipo de `E` colección, un `T`tipo de enumerador y un tipo de elemento.</span><span class="sxs-lookup"><span data-stu-id="82896-388">The above steps, if successful, unambiguously produce a collection type `C`, enumerator type `E` and element type `T`.</span></span> <span data-ttu-id="82896-389">Una instrucción foreach con el formato</span><span class="sxs-lookup"><span data-stu-id="82896-389">A foreach statement of the form</span></span>
```csharp
foreach (V v in x) embedded_statement
```
<span data-ttu-id="82896-390">se expande a continuación hasta:</span><span class="sxs-lookup"><span data-stu-id="82896-390">is then expanded to:</span></span>
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

<span data-ttu-id="82896-391">La variable `e` no es visible o accesible para la expresión `x` o la instrucción incrustada ni cualquier otro código fuente del programa.</span><span class="sxs-lookup"><span data-stu-id="82896-391">The variable `e` is not visible to or accessible to the expression `x` or the embedded statement or any other source code of the program.</span></span> <span data-ttu-id="82896-392">La variable `v` es de solo lectura en la instrucción insertada.</span><span class="sxs-lookup"><span data-stu-id="82896-392">The variable `v` is read-only in the embedded statement.</span></span> <span data-ttu-id="82896-393">Si no hay una conversión explícita ([conversiones explícitas](conversions.md#explicit-conversions)) de `T` (el tipo de elemento) a `V` ( *local_variable_type* en la instrucción foreach), se genera un error y no se realiza ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="82896-393">If there is not an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) from `T` (the element type) to `V` (the *local_variable_type* in the foreach statement), an error is produced and no further steps are taken.</span></span> <span data-ttu-id="82896-394">Si `x` tiene el valor `null`, se `System.NullReferenceException` produce una excepción en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="82896-394">If `x` has the value `null`, a `System.NullReferenceException` is thrown at run-time.</span></span>

<span data-ttu-id="82896-395">Una implementación puede implementar una instrucción foreach determinada de forma diferente, por ejemplo, por motivos de rendimiento, siempre que el comportamiento sea coherente con la expansión anterior.</span><span class="sxs-lookup"><span data-stu-id="82896-395">An implementation is permitted to implement a given foreach-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="82896-396">La colocación de `v` dentro del bucle while es importante para que lo Capture cualquier función anónima que se produzca en el *embedded_statement*.</span><span class="sxs-lookup"><span data-stu-id="82896-396">The placement of `v` inside the while loop is important for how it is captured by any anonymous function occurring in the *embedded_statement*.</span></span>

<span data-ttu-id="82896-397">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="82896-397">For example:</span></span>
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
<span data-ttu-id="82896-398">Si `v` se declaró fuera del bucle while, se compartirá entre todas las iteraciones y su valor después del bucle for sería el valor final, `13`, que es lo que la invocación de `f` imprimiría.</span><span class="sxs-lookup"><span data-stu-id="82896-398">If `v` was declared outside of the while loop, it would be shared among all iterations, and its value after the for loop would be the final value, `13`, which is what the invocation of `f` would print.</span></span> <span data-ttu-id="82896-399">En su lugar, dado que cada iteración tiene `v`su propia variable, la `f` que captura en la primera iteración seguirá conservando `7`el valor, que es lo que se va a imprimir.</span><span class="sxs-lookup"><span data-stu-id="82896-399">Instead, because each iteration has its own variable `v`, the one captured by `f` in the first iteration will continue to hold the value `7`, which is what will be printed.</span></span> <span data-ttu-id="82896-400">(Nota: versiones anteriores de C# declaradas `v` fuera del bucle while).</span><span class="sxs-lookup"><span data-stu-id="82896-400">(Note: earlier versions of C# declared `v` outside of the while loop.)</span></span>

<span data-ttu-id="82896-401">El cuerpo del bloque finally se construye según los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="82896-401">The body of the finally block is constructed according to the following steps:</span></span>

*  <span data-ttu-id="82896-402">Si hay una conversión implícita de `E` en la `System.IDisposable` interfaz,</span><span class="sxs-lookup"><span data-stu-id="82896-402">If there is an implicit conversion from `E` to the `System.IDisposable` interface, then</span></span>
   *  <span data-ttu-id="82896-403">Si `E` es un tipo de valor que no acepta valores NULL, la cláusula finally se expande al equivalente semántico de:</span><span class="sxs-lookup"><span data-stu-id="82896-403">If `E` is a non-nullable value type then the finally clause is expanded to the semantic equivalent  of:</span></span>

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  <span data-ttu-id="82896-404">En caso contrario, la cláusula finally se expande al equivalente semántico de:</span><span class="sxs-lookup"><span data-stu-id="82896-404">Otherwise the finally clause is expanded to the semantic equivalent of:</span></span>

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   <span data-ttu-id="82896-405">salvo que si `E` es un tipo de valor o un parámetro de tipo al que se ha creado una instancia de un tipo `e` de `System.IDisposable` valor, la conversión de a no hará que se produzca la conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="82896-405">except that if `E` is a value type, or a type parameter instantiated to a value type, then the cast of `e` to `System.IDisposable` will not cause boxing to occur.</span></span>

*  <span data-ttu-id="82896-406">De lo contrario `E` , si es un tipo sellado, la cláusula finally se expande a un bloque vacío:</span><span class="sxs-lookup"><span data-stu-id="82896-406">Otherwise, if `E` is a sealed type, the finally clause is expanded to an empty block:</span></span>

   ```csharp
   finally {
   }
   ```

*  <span data-ttu-id="82896-407">De lo contrario, la cláusula finally se expande a:</span><span class="sxs-lookup"><span data-stu-id="82896-407">Otherwise, the finally clause is expanded to:</span></span>

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   <span data-ttu-id="82896-408">La variable `d` local no es visible o accesible para cualquier código de usuario.</span><span class="sxs-lookup"><span data-stu-id="82896-408">The local variable `d` is not visible to or accessible to any user code.</span></span> <span data-ttu-id="82896-409">En concreto, no entra en conflicto con ninguna otra variable cuyo ámbito incluya el bloque Finally.</span><span class="sxs-lookup"><span data-stu-id="82896-409">In particular, it does not conflict with any other variable whose scope includes the finally block.</span></span>

<span data-ttu-id="82896-410">El orden en el `foreach` que recorre los elementos de una matriz es el siguiente: En el caso de las matrices unidimensionales, los elementos se recorren en orden `0` creciente de índice, `Length - 1`empezando por el índice y terminando por el índice.</span><span class="sxs-lookup"><span data-stu-id="82896-410">The order in which `foreach` traverses the elements of an array, is as follows: For single-dimensional arrays elements are traversed in increasing index order, starting with index `0` and ending with index `Length - 1`.</span></span> <span data-ttu-id="82896-411">En el caso de las matrices multidimensionales, los elementos se recorren de forma que los índices de la dimensión situada más a la derecha aumenten primero, la dimensión izquierda siguiente y así sucesivamente hacia la izquierda.</span><span class="sxs-lookup"><span data-stu-id="82896-411">For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left.</span></span>

<span data-ttu-id="82896-412">En el ejemplo siguiente se imprime cada valor en una matriz bidimensional, en el orden de los elementos:</span><span class="sxs-lookup"><span data-stu-id="82896-412">The following example prints out each value in a two-dimensional array, in element order:</span></span>
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
<span data-ttu-id="82896-413">La salida generada es la siguiente:</span><span class="sxs-lookup"><span data-stu-id="82896-413">The output produced is as follows:</span></span>
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

<span data-ttu-id="82896-414">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-414">In the example</span></span>
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
<span data-ttu-id="82896-415">el tipo de `n` se deduce `int`como, el tipo de elemento de `numbers`.</span><span class="sxs-lookup"><span data-stu-id="82896-415">the type of `n` is inferred to be `int`, the element type of `numbers`.</span></span>

## <a name="jump-statements"></a><span data-ttu-id="82896-416">Instrucciones de salto</span><span class="sxs-lookup"><span data-stu-id="82896-416">Jump statements</span></span>

<span data-ttu-id="82896-417">Las instrucciones de salto transfieren el control incondicionalmente.</span><span class="sxs-lookup"><span data-stu-id="82896-417">Jump statements unconditionally transfer control.</span></span>

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

<span data-ttu-id="82896-418">La ubicación a la que una instrucción de salto transfiere el control se denomina ***destino*** de la instrucción de salto.</span><span class="sxs-lookup"><span data-stu-id="82896-418">The location to which a jump statement transfers control is called the ***target*** of the jump statement.</span></span>

<span data-ttu-id="82896-419">Cuando una instrucción de salto se produce dentro de un bloque y el destino de la instrucción de salto está fuera del bloque, se dice que la instrucción de salto ***sale*** del bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-419">When a jump statement occurs within a block, and the target of that jump statement is outside that block, the jump statement is said to ***exit*** the block.</span></span> <span data-ttu-id="82896-420">Aunque una instrucción de salto puede transferir el control fuera de un bloque, nunca puede transferir el control a un bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-420">While a jump statement may transfer control out of a block, it can never transfer control into a block.</span></span>

<span data-ttu-id="82896-421">La ejecución de instrucciones de salto es complicada por la presencia de `try` instrucciones intermedias.</span><span class="sxs-lookup"><span data-stu-id="82896-421">Execution of jump statements is complicated by the presence of intervening `try` statements.</span></span> <span data-ttu-id="82896-422">En ausencia de estas `try` instrucciones, una instrucción de salto transfiere incondicionalmente el control de la instrucción de salto a su destino.</span><span class="sxs-lookup"><span data-stu-id="82896-422">In the absence of such `try` statements, a jump statement unconditionally transfers control from the jump statement to its target.</span></span> <span data-ttu-id="82896-423">En presencia de dichas `try` instrucciones intermedias, la ejecución es más compleja.</span><span class="sxs-lookup"><span data-stu-id="82896-423">In the presence of such intervening `try` statements, execution is more complex.</span></span> <span data-ttu-id="82896-424">Si la instrucción de salto sale de uno o `try` más bloques con `finally` bloques asociados, el `finally` control se transfiere inicialmente al bloque de la `try` instrucción más interna.</span><span class="sxs-lookup"><span data-stu-id="82896-424">If the jump statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82896-425">Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión.</span><span class="sxs-lookup"><span data-stu-id="82896-425">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82896-426">Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones que intervienen.</span><span class="sxs-lookup"><span data-stu-id="82896-426">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>

<span data-ttu-id="82896-427">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-427">In the example</span></span>
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
<span data-ttu-id="82896-428">los `finally` bloques asociados a dos `try` instrucciones se ejecutan antes de que el control se transfiera al destino de la instrucción de salto.</span><span class="sxs-lookup"><span data-stu-id="82896-428">the `finally` blocks associated with two `try` statements are executed before control is transferred to the target of the jump statement.</span></span>

<span data-ttu-id="82896-429">La salida generada es la siguiente:</span><span class="sxs-lookup"><span data-stu-id="82896-429">The output produced is as follows:</span></span>
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a><span data-ttu-id="82896-430">Instrucción break</span><span class="sxs-lookup"><span data-stu-id="82896-430">The break statement</span></span>

<span data-ttu-id="82896-431">La `break` instrucción sale de la instrucción de inclusión `switch`, `while` `do` `for`,, o `foreach` más cercana.</span><span class="sxs-lookup"><span data-stu-id="82896-431">The `break` statement exits the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
break_statement
    : 'break' ';'
    ;
```

<span data-ttu-id="82896-432">El destino de una `break` instrucción es el punto final de la instrucción envolvente `switch`, `while`, `do` `for`, o `foreach` más cercana.</span><span class="sxs-lookup"><span data-stu-id="82896-432">The target of a `break` statement is the end point of the nearest enclosing `switch`, `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="82896-433">Si una `break` instrucción no está delimitada por `switch`una `while`instrucción `do`, `for`,, `foreach` o, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-433">If a `break` statement is not enclosed by a `switch`, `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="82896-434">Cuando varias `switch`instrucciones `while` `break` , `do` ,,`foreach` o se anidan entre sí, una instrucción solo se aplica a la instrucción más interna. `for`</span><span class="sxs-lookup"><span data-stu-id="82896-434">When multiple `switch`, `while`, `do`, `for`, or `foreach` statements are nested within each other, a `break` statement applies only to the innermost statement.</span></span> <span data-ttu-id="82896-435">Para transferir el control entre varios niveles de anidamiento, `goto` se debe usar una instrucción ([instrucción Goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="82896-435">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="82896-436">Una `break` instrucción no puede salir `finally` de un bloque ([la instrucción try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="82896-436">A `break` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="82896-437">Cuando una `break` instrucción aparece dentro de `finally` un bloque, el destino de `break` la instrucción debe estar dentro del `finally` mismo bloque; de lo contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-437">When a `break` statement occurs within a `finally` block, the target of the `break` statement must be within the same `finally` block; otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="82896-438">Una `break` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-438">A `break` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-439">Si la `break` instrucción sale de uno o más `try` bloques con `finally` bloques `finally` asociados, el control se transfiere inicialmente al bloque de la instrucción `try` más interna.</span><span class="sxs-lookup"><span data-stu-id="82896-439">If the `break` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82896-440">Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión.</span><span class="sxs-lookup"><span data-stu-id="82896-440">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82896-441">Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones que intervienen.</span><span class="sxs-lookup"><span data-stu-id="82896-441">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="82896-442">El control se transfiere al destino de la `break` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-442">Control is transferred to the target of the `break` statement.</span></span>

<span data-ttu-id="82896-443">Dado que `break` una instrucción transfiere el control incondicionalmente a otra parte, el `break` punto final de una instrucción nunca es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-443">Because a `break` statement unconditionally transfers control elsewhere, the end point of a `break` statement is never reachable.</span></span>

### <a name="the-continue-statement"></a><span data-ttu-id="82896-444">La instrucción continue</span><span class="sxs-lookup"><span data-stu-id="82896-444">The continue statement</span></span>

<span data-ttu-id="82896-445">La `continue` instrucción inicia una nueva iteración de la instrucción envolvente `while`, `do`, `for`o `foreach` más cercana.</span><span class="sxs-lookup"><span data-stu-id="82896-445">The `continue` statement starts a new iteration of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span>

```antlr
continue_statement
    : 'continue' ';'
    ;
```

<span data-ttu-id="82896-446">El destino de una `continue` instrucción es el punto final de la instrucción incrustada de la instrucción envolvente `while`, `do`, `for`o `foreach` más cercana.</span><span class="sxs-lookup"><span data-stu-id="82896-446">The target of a `continue` statement is the end point of the embedded statement of the nearest enclosing `while`, `do`, `for`, or `foreach` statement.</span></span> <span data-ttu-id="82896-447">Si una `continue` instrucción no está delimitada por `while`una `do`instrucción `for`,, `foreach` o, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-447">If a `continue` statement is not enclosed by a `while`, `do`, `for`, or `foreach` statement, a compile-time error occurs.</span></span>

<span data-ttu-id="82896-448">Cuando varias `while`instrucciones `do`, `for`, `continue` o `foreach` se anidan entre sí, una instrucción solo se aplica a la instrucción más interna.</span><span class="sxs-lookup"><span data-stu-id="82896-448">When multiple `while`, `do`, `for`, or `foreach` statements are nested within each other, a `continue` statement applies only to the innermost statement.</span></span> <span data-ttu-id="82896-449">Para transferir el control entre varios niveles de anidamiento, `goto` se debe usar una instrucción ([instrucción Goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="82896-449">To transfer control across multiple nesting levels, a `goto` statement ([The goto statement](statements.md#the-goto-statement)) must be used.</span></span>

<span data-ttu-id="82896-450">Una `continue` instrucción no puede salir `finally` de un bloque ([la instrucción try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="82896-450">A `continue` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="82896-451">Cuando una `continue` instrucción aparece dentro de `finally` un bloque, el destino de `continue` la instrucción debe estar dentro del `finally` mismo bloque; de lo contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-451">When a `continue` statement occurs within a `finally` block, the target of the `continue` statement must be within the same `finally` block; otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="82896-452">Una `continue` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-452">A `continue` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-453">Si la `continue` instrucción sale de uno o más `try` bloques con `finally` bloques `finally` asociados, el control se transfiere inicialmente al bloque de la instrucción `try` más interna.</span><span class="sxs-lookup"><span data-stu-id="82896-453">If the `continue` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82896-454">Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión.</span><span class="sxs-lookup"><span data-stu-id="82896-454">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82896-455">Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones que intervienen.</span><span class="sxs-lookup"><span data-stu-id="82896-455">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="82896-456">El control se transfiere al destino de la `continue` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-456">Control is transferred to the target of the `continue` statement.</span></span>

<span data-ttu-id="82896-457">Dado que `continue` una instrucción transfiere el control incondicionalmente a otra parte, el `continue` punto final de una instrucción nunca es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-457">Because a `continue` statement unconditionally transfers control elsewhere, the end point of a `continue` statement is never reachable.</span></span>

### <a name="the-goto-statement"></a><span data-ttu-id="82896-458">La instrucción goto</span><span class="sxs-lookup"><span data-stu-id="82896-458">The goto statement</span></span>

<span data-ttu-id="82896-459">La `goto` instrucción transfiere el control a una instrucción marcada por una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-459">The `goto` statement transfers control to a statement that is marked by a label.</span></span>

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

<span data-ttu-id="82896-460">El destino de una `goto` instrucción *Identifier* es la instrucción con etiqueta con la etiqueta especificada.</span><span class="sxs-lookup"><span data-stu-id="82896-460">The target of a `goto` *identifier* statement is the labeled statement with the given label.</span></span> <span data-ttu-id="82896-461">Si no existe una etiqueta con el nombre especificado en el miembro de función actual, o si la `goto` instrucción no está dentro del ámbito de la etiqueta, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-461">If a label with the given name does not exist in the current function member, or if the `goto` statement is not within the scope of the label, a compile-time error occurs.</span></span> <span data-ttu-id="82896-462">Esta regla permite el uso de una `goto` instrucción para transferir el control fuera de un ámbito anidado, pero no a un ámbito anidado.</span><span class="sxs-lookup"><span data-stu-id="82896-462">This rule permits the use of a `goto` statement to transfer control out of a nested scope, but not into a nested scope.</span></span> <span data-ttu-id="82896-463">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-463">In the example</span></span>
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
<span data-ttu-id="82896-464">una `goto` instrucción se usa para transferir el control fuera de un ámbito anidado.</span><span class="sxs-lookup"><span data-stu-id="82896-464">a `goto` statement is used to transfer control out of a nested scope.</span></span>

<span data-ttu-id="82896-465">El destino de una `goto case` instrucción es la lista de instrucciones de la instrucción de `switch` inclusión inmediata ([la instrucción switch](statements.md#the-switch-statement)), que contiene `case` una etiqueta con el valor constante especificado.</span><span class="sxs-lookup"><span data-stu-id="82896-465">The target of a `goto case` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `case` label with the given constant value.</span></span> <span data-ttu-id="82896-466">Si la instrucción `goto case` no está delimitada por una instrucción `switch`, si *constant_expression* no se pueden convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) al tipo de control de la instrucción envolvente de `switch` más cercana, o si el contenedor más cercano la instrucción `switch` no contiene una etiqueta `case` con el valor constante especificado, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-466">If the `goto case` statement is not enclosed by a `switch` statement, if the *constant_expression* is not implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the governing type of the nearest enclosing `switch` statement, or if the nearest enclosing `switch` statement does not contain a `case` label with the given constant value, a compile-time error occurs.</span></span>

<span data-ttu-id="82896-467">El destino de una `goto default` instrucción es la lista de instrucciones de la instrucción de `switch` inclusión inmediata ([la instrucción switch](statements.md#the-switch-statement)), que contiene `default` una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="82896-467">The target of a `goto default` statement is the statement list in the immediately enclosing `switch` statement ([The switch statement](statements.md#the-switch-statement)), which contains a `default` label.</span></span> <span data-ttu-id="82896-468">Si la `goto default` instrucción no está delimitada por `switch` una instrucción, o si la instrucción de `switch` inclusión más cercana no contiene `default` una etiqueta, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-468">If the `goto default` statement is not enclosed by a `switch` statement, or if the nearest enclosing `switch` statement does not contain a `default` label, a compile-time error occurs.</span></span>

<span data-ttu-id="82896-469">Una `goto` instrucción no puede salir `finally` de un bloque ([la instrucción try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="82896-469">A `goto` statement cannot exit a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span> <span data-ttu-id="82896-470">Cuando una `goto` instrucción aparece dentro de `finally` un bloque, el destino de `goto` la instrucción debe estar dentro del `finally` mismo bloque o, de lo contrario, se producirá un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-470">When a `goto` statement occurs within a `finally` block, the target of the `goto` statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="82896-471">Una `goto` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-471">A `goto` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-472">Si la `goto` instrucción sale de uno o más `try` bloques con `finally` bloques `finally` asociados, el control se transfiere inicialmente al bloque de la instrucción `try` más interna.</span><span class="sxs-lookup"><span data-stu-id="82896-472">If the `goto` statement exits one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82896-473">Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión.</span><span class="sxs-lookup"><span data-stu-id="82896-473">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82896-474">Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones que intervienen.</span><span class="sxs-lookup"><span data-stu-id="82896-474">This process is repeated until the `finally` blocks of all intervening `try` statements have been executed.</span></span>
*  <span data-ttu-id="82896-475">El control se transfiere al destino de la `goto` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-475">Control is transferred to the target of the `goto` statement.</span></span>

<span data-ttu-id="82896-476">Dado que `goto` una instrucción transfiere el control incondicionalmente a otra parte, el `goto` punto final de una instrucción nunca es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-476">Because a `goto` statement unconditionally transfers control elsewhere, the end point of a `goto` statement is never reachable.</span></span>

### <a name="the-return-statement"></a><span data-ttu-id="82896-477">La instrucción return</span><span class="sxs-lookup"><span data-stu-id="82896-477">The return statement</span></span>

<span data-ttu-id="82896-478">La `return` instrucción devuelve el control al llamador actual de la función en la que `return` aparece la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-478">The `return` statement returns control to the current caller of the function in which the `return` statement appears.</span></span>

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

<span data-ttu-id="82896-479">Una `return` instrucción sin expresión solo se puede usar en un miembro de función que no calcule un valor, es decir, un método con el tipo de resultado ([cuerpo del método](classes.md#method-body)) `void`, el `set` descriptor de acceso de una propiedad o `add` un indizador, el los `remove` descriptores de acceso de un evento, un constructor de instancia, un constructor estático o un destructor.</span><span class="sxs-lookup"><span data-stu-id="82896-479">A `return` statement with no expression can be used only in a function member that does not compute a value, that is, a method with the result type ([Method body](classes.md#method-body)) `void`, the `set` accessor of a property or indexer, the `add` and `remove` accessors of an event, an instance constructor, a static constructor, or a destructor.</span></span>

<span data-ttu-id="82896-480">Una `return` instrucción con una expresión solo se puede usar en un miembro de función que calcula un valor, es decir, un método con un tipo de resultado no void, el `get` descriptor de acceso de una propiedad o un indizador, o un operador definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="82896-480">A `return` statement with an expression can only be used in a function member that computes a value, that is, a method with a non-void result type, the `get` accessor of a property or indexer, or a user-defined operator.</span></span> <span data-ttu-id="82896-481">Debe existir una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo de la expresión al tipo de valor devuelto del miembro de función contenedora.</span><span class="sxs-lookup"><span data-stu-id="82896-481">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression to the return type of the containing function member.</span></span>

<span data-ttu-id="82896-482">Las instrucciones Return también se pueden utilizar en el cuerpo de expresiones de función anónimas ([expresiones de función anónimas](expressions.md#anonymous-function-expressions)) y participar en la determinación de las conversiones que existen para esas funciones.</span><span class="sxs-lookup"><span data-stu-id="82896-482">Return statements can also be used in the body of anonymous function expressions ([Anonymous function expressions](expressions.md#anonymous-function-expressions)), and participate in determining which conversions exist for those functions.</span></span>

<span data-ttu-id="82896-483">Se trata de un error en tiempo de compilación `return` para que una instrucción aparezca `finally` en un bloque ([la instrucción try](statements.md#the-try-statement)).</span><span class="sxs-lookup"><span data-stu-id="82896-483">It is a compile-time error for a `return` statement to appear in a `finally` block ([The try statement](statements.md#the-try-statement)).</span></span>

<span data-ttu-id="82896-484">Una `return` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-484">A `return` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-485">Si la `return` instrucción especifica una expresión, se evalúa la expresión y el valor resultante se convierte al tipo de valor devuelto de la función que la contiene mediante una conversión implícita.</span><span class="sxs-lookup"><span data-stu-id="82896-485">If the `return` statement specifies an expression, the expression is evaluated and the resulting value is converted to the return type of the containing function by an implicit conversion.</span></span> <span data-ttu-id="82896-486">El resultado de la conversión se convierte en el valor de resultado producido por la función.</span><span class="sxs-lookup"><span data-stu-id="82896-486">The result of the conversion becomes the result value produced by the function.</span></span>
*  <span data-ttu-id="82896-487">Si la `return` instrucción está incluida en uno o varios `try` bloques `catch` o con bloques `finally` asociados, el `finally` control se transfiere inicialmente al bloque de la instrucción `try` más interna.</span><span class="sxs-lookup"><span data-stu-id="82896-487">If the `return` statement is enclosed by one or more `try` or `catch` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82896-488">Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión.</span><span class="sxs-lookup"><span data-stu-id="82896-488">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82896-489">Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones envolventes.</span><span class="sxs-lookup"><span data-stu-id="82896-489">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="82896-490">Si la función contenedora no es una función asincrónica, el control se devuelve al autor de la llamada de la función contenedora junto con el valor del resultado, si existe.</span><span class="sxs-lookup"><span data-stu-id="82896-490">If the containing function is not an async function, control is returned to the caller of the containing function along with the result value, if any.</span></span>
*  <span data-ttu-id="82896-491">Si la función contenedora es una función asincrónica, el control se devuelve al llamador actual y el valor del resultado, si existe, se registra en la tarea devuelta tal y como se describe en ([interfaces del enumerador](classes.md#enumerator-interfaces)).</span><span class="sxs-lookup"><span data-stu-id="82896-491">If the containing function is an async function, control is returned to the current caller, and the result value, if any, is recorded in the return task as described in ([Enumerator interfaces](classes.md#enumerator-interfaces)).</span></span>

<span data-ttu-id="82896-492">Dado que `return` una instrucción transfiere el control incondicionalmente a otra parte, el `return` punto final de una instrucción nunca es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-492">Because a `return` statement unconditionally transfers control elsewhere, the end point of a `return` statement is never reachable.</span></span>

### <a name="the-throw-statement"></a><span data-ttu-id="82896-493">La instrucción throw</span><span class="sxs-lookup"><span data-stu-id="82896-493">The throw statement</span></span>

<span data-ttu-id="82896-494">La `throw` instrucción produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="82896-494">The `throw` statement throws an exception.</span></span>

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

<span data-ttu-id="82896-495">Una `throw` instrucción con una expresión inicia el valor generado al evaluar la expresión.</span><span class="sxs-lookup"><span data-stu-id="82896-495">A `throw` statement with an expression throws the value produced by evaluating the expression.</span></span> <span data-ttu-id="82896-496">La expresión debe indicar un valor del tipo `System.Exception`de clase, de un tipo de clase que se deriva de o de un tipo de parámetro de tipo que tiene `System.Exception` (o una subclase de `System.Exception` ella) como su clase base efectiva.</span><span class="sxs-lookup"><span data-stu-id="82896-496">The expression must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="82896-497">Si la evaluación de la expresión `null`produce, `System.NullReferenceException` se produce una excepción en su lugar.</span><span class="sxs-lookup"><span data-stu-id="82896-497">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="82896-498">Una `throw` instrucción sin expresión solo se puede usar en un `catch` bloque, en cuyo caso la instrucción vuelve a iniciar la excepción que está controlando actualmente ese `catch` bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-498">A `throw` statement with no expression can be used only in a `catch` block, in which case that statement re-throws the exception that is currently being handled by that `catch` block.</span></span>

<span data-ttu-id="82896-499">Dado que `throw` una instrucción transfiere el control incondicionalmente a otra parte, el `throw` punto final de una instrucción nunca es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-499">Because a `throw` statement unconditionally transfers control elsewhere, the end point of a `throw` statement is never reachable.</span></span>

<span data-ttu-id="82896-500">Cuando se produce una excepción, el control se transfiere a la `catch` primera cláusula de una instrucción `try` de inclusión que puede controlar la excepción.</span><span class="sxs-lookup"><span data-stu-id="82896-500">When an exception is thrown, control is transferred to the first `catch` clause in an enclosing `try` statement that can handle the exception.</span></span> <span data-ttu-id="82896-501">El proceso que tiene lugar desde el punto de la excepción que se está iniciando hasta el punto de transferir el control a un controlador de excepciones adecuado se conoce como ***propagación de excepciones***.</span><span class="sxs-lookup"><span data-stu-id="82896-501">The process that takes place from the point of the exception being thrown to the point of transferring control to a suitable exception handler is known as ***exception propagation***.</span></span> <span data-ttu-id="82896-502">La propagación de una excepción consiste en evaluar repetidamente los siguientes pasos hasta `catch` que se encuentre una cláusula que coincida con la excepción.</span><span class="sxs-lookup"><span data-stu-id="82896-502">Propagation of an exception consists of repeatedly evaluating the following steps until a `catch` clause that matches the exception is found.</span></span> <span data-ttu-id="82896-503">En esta descripción, el ***punto de inicio*** es inicialmente la ubicación en la que se produce la excepción.</span><span class="sxs-lookup"><span data-stu-id="82896-503">In this description, the ***throw point*** is initially the location at which the exception is thrown.</span></span>

*  <span data-ttu-id="82896-504">En el miembro de función actual, `try` se examina cada instrucción que incluye el punto de inicio.</span><span class="sxs-lookup"><span data-stu-id="82896-504">In the current function member, each `try` statement that encloses the throw point is examined.</span></span> <span data-ttu-id="82896-505">Para cada instrucción `S`, empezando por la instrucción `try` más interna y finalizando con `try` la instrucción externa, se evalúan los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="82896-505">For each statement `S`, starting with the innermost `try` statement and ending with the outermost `try` statement, the following steps are evaluated:</span></span>

   * <span data-ttu-id="82896-506">Si el `try` bloque de `S` incluye el punto de inicio y si S tiene una o más `catch` cláusulas, las `catch` cláusulas se examinan en orden de aparición para encontrar un controlador adecuado para la excepción, de acuerdo con las reglas especificadas en. Sección [de la instrucción try](statements.md#the-try-statement).</span><span class="sxs-lookup"><span data-stu-id="82896-506">If the `try` block of `S` encloses the throw point and if S has one or more `catch` clauses, the `catch` clauses are examined in order of appearance to locate a suitable handler for the exception, according to the rules specified in Section [The try statement](statements.md#the-try-statement).</span></span> <span data-ttu-id="82896-507">Si se encuentra `catch` una cláusula coincidente, la propagación de excepciones se completa mediante la transferencia del control al bloque de esa `catch` cláusula.</span><span class="sxs-lookup"><span data-stu-id="82896-507">If a matching `catch` clause is located, the exception propagation is completed by transferring control to the block of that `catch` clause.</span></span>

   * <span data-ttu-id="82896-508">De lo contrario, `try` si el bloque `catch` o un `S` bloque de incluye el punto de inicio y `S` si tiene `finally` un bloque, el `finally` control se transfiere al bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-508">Otherwise, if the `try` block or a `catch` block of `S` encloses the throw point and if `S` has a `finally` block, control is transferred to the `finally` block.</span></span> <span data-ttu-id="82896-509">Si el `finally` bloque produce otra excepción, finaliza el procesamiento de la excepción actual.</span><span class="sxs-lookup"><span data-stu-id="82896-509">If the `finally` block throws another exception, processing of the current exception is terminated.</span></span> <span data-ttu-id="82896-510">De lo contrario, cuando el control alcanza el punto `finally` final del bloque, continúa el procesamiento de la excepción actual.</span><span class="sxs-lookup"><span data-stu-id="82896-510">Otherwise, when control reaches the end point of the `finally` block, processing of the current exception is continued.</span></span>

*  <span data-ttu-id="82896-511">Si no se encuentra un controlador de excepciones en la invocación de función actual, se termina la invocación de la función y se produce uno de los siguientes casos:</span><span class="sxs-lookup"><span data-stu-id="82896-511">If an exception handler was not located in the current function invocation, the function invocation is terminated, and one of the following occurs:</span></span>

   * <span data-ttu-id="82896-512">Si la función actual no es asincrónica, los pasos anteriores se repiten para el llamador de la función con un punto de inicio correspondiente a la instrucción de la que se invocó el miembro de función.</span><span class="sxs-lookup"><span data-stu-id="82896-512">If the current function is non-async, the steps above are repeated for the caller of the function with a throw point corresponding to the statement from which the function member was invoked.</span></span>

   * <span data-ttu-id="82896-513">Si la función actual es asincrónica y devuelve la tarea, la excepción se registra en la tarea devuelta, que se coloca en un estado de error o cancelado, tal y como se describe en [interfaces de enumerador](classes.md#enumerator-interfaces).</span><span class="sxs-lookup"><span data-stu-id="82896-513">If the current function is async and task-returning, the exception is recorded in the return task, which is put into a faulted or cancelled state as described in [Enumerator interfaces](classes.md#enumerator-interfaces).</span></span>

   * <span data-ttu-id="82896-514">Si la función actual es asincrónica y devuelve void, el contexto de sincronización del subproceso actual se notifica como se describe en [interfaces enumerables](classes.md#enumerable-interfaces).</span><span class="sxs-lookup"><span data-stu-id="82896-514">If the current function is async and void-returning, the synchronization context of the current thread is notified as described in [Enumerable interfaces](classes.md#enumerable-interfaces).</span></span>

*  <span data-ttu-id="82896-515">Si el procesamiento de excepciones finaliza todas las invocaciones de miembros de función en el subproceso actual, lo que indica que el subproceso no tiene ningún controlador para la excepción, el subproceso se termina.</span><span class="sxs-lookup"><span data-stu-id="82896-515">If the exception processing terminates all function member invocations in the current thread, indicating that the thread has no handler for the exception, then the thread is itself terminated.</span></span> <span data-ttu-id="82896-516">El impacto de dicha terminación está definido por la implementación.</span><span class="sxs-lookup"><span data-stu-id="82896-516">The impact of such termination is implementation-defined.</span></span>

## <a name="the-try-statement"></a><span data-ttu-id="82896-517">Instrucción try</span><span class="sxs-lookup"><span data-stu-id="82896-517">The try statement</span></span>

<span data-ttu-id="82896-518">La `try` instrucción proporciona un mecanismo para detectar las excepciones que se producen durante la ejecución de un bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-518">The `try` statement provides a mechanism for catching exceptions that occur during execution of a block.</span></span> <span data-ttu-id="82896-519">Además, la `try` instrucción proporciona la capacidad de especificar un bloque de código que siempre se ejecuta cuando el control sale `try` de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-519">Furthermore, the `try` statement provides the ability to specify a block of code that is always executed when control leaves the `try` statement.</span></span>

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

<span data-ttu-id="82896-520">Hay tres formas posibles de `try` instrucciones:</span><span class="sxs-lookup"><span data-stu-id="82896-520">There are three possible forms of `try` statements:</span></span>

*  <span data-ttu-id="82896-521">Un `try` bloque seguido de uno o varios `catch` bloques.</span><span class="sxs-lookup"><span data-stu-id="82896-521">A `try` block followed by one or more `catch` blocks.</span></span>
*  <span data-ttu-id="82896-522">Un `try` bloque seguido de un `finally` bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-522">A `try` block followed by a `finally` block.</span></span>
*  <span data-ttu-id="82896-523">Un `try` bloque seguido de uno o varios `catch` bloques seguidos de `finally` un bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-523">A `try` block followed by one or more `catch` blocks followed by a `finally` block.</span></span>

<span data-ttu-id="82896-524">Cuando una cláusula `catch` especifica un *exception_specifier*, el tipo debe ser `System.Exception`, un tipo que se deriva de `System.Exception` o un tipo de parámetro de tipo que tiene `System.Exception` (o una subclase de ella) como su clase base efectiva.</span><span class="sxs-lookup"><span data-stu-id="82896-524">When a `catch` clause specifies an *exception_specifier*, the type must be `System.Exception`, a type that derives from `System.Exception` or a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span>

<span data-ttu-id="82896-525">Cuando una cláusula `catch` especifica un *exception_specifier* con un *identificador*, se declara una ***variable de excepción*** con el nombre y el tipo especificados.</span><span class="sxs-lookup"><span data-stu-id="82896-525">When a `catch` clause specifies both an *exception_specifier* with an *identifier*, an ***exception variable*** of the given name and type is declared.</span></span> <span data-ttu-id="82896-526">La variable de excepción corresponde a una variable local con un ámbito que se extiende `catch` por encima de la cláusula.</span><span class="sxs-lookup"><span data-stu-id="82896-526">The exception variable corresponds to a local variable with a scope that extends over the `catch` clause.</span></span> <span data-ttu-id="82896-527">Durante la ejecución del *exception_filter* y el *bloque*, la variable de excepción representa la excepción que se está controlando actualmente.</span><span class="sxs-lookup"><span data-stu-id="82896-527">During execution of the *exception_filter* and *block*, the exception variable represents the exception currently being handled.</span></span> <span data-ttu-id="82896-528">A efectos de la comprobación de asignación definitiva, la variable de excepción se considera asignada definitivamente en todo el ámbito.</span><span class="sxs-lookup"><span data-stu-id="82896-528">For purposes of definite assignment checking, the exception variable is considered definitely assigned in its entire scope.</span></span>

<span data-ttu-id="82896-529">A menos `catch` que una cláusula incluya un nombre de variable de excepción, no es posible tener acceso al objeto de `catch` excepción en el filtro y bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-529">Unless a `catch` clause includes an exception variable name, it is impossible to access the exception object in the filter and `catch` block.</span></span>

<span data-ttu-id="82896-530">Una cláusula `catch` que no especifica un *exception_specifier* se denomina cláusula general de @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="82896-530">A `catch` clause that does not specify an *exception_specifier* is called a general `catch` clause.</span></span>

<span data-ttu-id="82896-531">Algunos lenguajes de programación pueden admitir excepciones que no se pueden representar como un objeto derivado `System.Exception`de, aunque estas excepciones nunca se pueden generar C# mediante código.</span><span class="sxs-lookup"><span data-stu-id="82896-531">Some programming languages may support exceptions that are not representable as an object derived from `System.Exception`, although such exceptions could never be generated by C# code.</span></span> <span data-ttu-id="82896-532">Se puede `catch` usar una cláusula general para detectar tales excepciones.</span><span class="sxs-lookup"><span data-stu-id="82896-532">A general `catch` clause may be used to catch such exceptions.</span></span> <span data-ttu-id="82896-533">Por lo tanto, `catch` una cláusula general es semánticamente diferente de una que especifica `System.Exception`el tipo, en que la primera también puede detectar excepciones de otros lenguajes.</span><span class="sxs-lookup"><span data-stu-id="82896-533">Thus, a general `catch` clause is semantically different from one that specifies the type `System.Exception`, in that the former may also catch exceptions from other languages.</span></span>

<span data-ttu-id="82896-534">Para buscar un controlador de una excepción, `catch` las cláusulas se examinan en orden léxico.</span><span class="sxs-lookup"><span data-stu-id="82896-534">In order to locate a handler for an exception, `catch` clauses are examined in lexical order.</span></span> <span data-ttu-id="82896-535">Si una `catch` cláusula especifica un tipo pero no un filtro de excepción, se trata de un error en tiempo de `catch` compilación para una cláusula `try` posterior en la misma instrucción para especificar un tipo que sea igual o derivado de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="82896-535">If a `catch` clause specifies a type but no exception filter, it is a compile-time error for a later `catch` clause in the same `try` statement to specify a type that is the same as, or is derived from, that type.</span></span> <span data-ttu-id="82896-536">Si una `catch` cláusula no especifica ningún tipo y no hay ningún filtro, debe ser `catch` la última cláusula `try` para esa instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-536">If a `catch` clause specifies no type and no filter, it must be the last `catch` clause for that `try` statement.</span></span>

<span data-ttu-id="82896-537">Dentro de `catch` un bloque, `throw` se puede usar una instrucción ([instrucción throw](statements.md#the-throw-statement)) sin expresión para volver a producir la excepción detectada por el `catch` bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-537">Within a `catch` block, a `throw` statement ([The throw statement](statements.md#the-throw-statement)) with no expression can be used to re-throw the exception that was caught by the `catch` block.</span></span> <span data-ttu-id="82896-538">Las asignaciones a una variable de excepción no modifican la excepción que se vuelve a iniciar.</span><span class="sxs-lookup"><span data-stu-id="82896-538">Assignments to an exception variable do not alter the exception that is re-thrown.</span></span>

<span data-ttu-id="82896-539">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="82896-539">In the example</span></span>
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
<span data-ttu-id="82896-540">el método `F` detecta una excepción, escribe información de diagnóstico en la consola, modifica la variable de excepción y vuelve a producir la excepción.</span><span class="sxs-lookup"><span data-stu-id="82896-540">the method `F` catches an exception, writes some diagnostic information to the console, alters the exception variable, and re-throws the exception.</span></span> <span data-ttu-id="82896-541">La excepción que se vuelve a producir es la excepción original, por lo que la salida generada es:</span><span class="sxs-lookup"><span data-stu-id="82896-541">The exception that is re-thrown is the original exception, so the output produced is:</span></span>
```console
Exception in F: G
Exception in Main: G
```

<span data-ttu-id="82896-542">Si se produjo `e` el primer bloque catch en lugar de volver a producir la excepción actual, la salida generada sería la siguiente:</span><span class="sxs-lookup"><span data-stu-id="82896-542">If the first catch block had thrown `e` instead of rethrowing the current exception, the output produced would be as follows:</span></span>
```console
Exception in F: G
Exception in Main: F
```

<span data-ttu-id="82896-543">Es un error en tiempo de compilación para una `break`instrucción `continue`, o `goto` para transferir el control fuera de un `finally` bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-543">It is a compile-time error for a `break`, `continue`, or `goto` statement to transfer control out of a `finally` block.</span></span> <span data-ttu-id="82896-544">Cuando una `break`instrucción `continue`, o `goto` aparece en un `finally` bloque, el destino de la instrucción debe estar dentro del mismo `finally` bloque o, de lo contrario, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="82896-544">When a `break`, `continue`, or `goto` statement occurs in a `finally` block, the target of the statement must be within the same `finally` block, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="82896-545">Se trata de un error en tiempo de compilación `return` para que una instrucción se `finally` produzca en un bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-545">It is a compile-time error for a `return` statement to occur in a `finally` block.</span></span>

<span data-ttu-id="82896-546">Una `try` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-546">A `try` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-547">El `try` control se transfiere al bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-547">Control is transferred to the `try` block.</span></span>
*  <span data-ttu-id="82896-548">When y si el control alcanza el punto final del `try` bloque:</span><span class="sxs-lookup"><span data-stu-id="82896-548">When and if control reaches the end point of the `try` block:</span></span>
   *  <span data-ttu-id="82896-549">Si la `try` instrucción tiene un `finally` bloque, se `finally` ejecuta el bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-549">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
   *  <span data-ttu-id="82896-550">El control se transfiere al punto final de la `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-550">Control is transferred to the end point of the `try` statement.</span></span>

*  <span data-ttu-id="82896-551">Si una excepción se propaga a la `try` instrucción durante la `try` ejecución del bloque:</span><span class="sxs-lookup"><span data-stu-id="82896-551">If an exception is propagated to the `try` statement during execution of the `try` block:</span></span>
   *  <span data-ttu-id="82896-552">Las `catch` cláusulas, si las hay, se examinan en orden de aparición para encontrar un controlador adecuado para la excepción.</span><span class="sxs-lookup"><span data-stu-id="82896-552">The `catch` clauses, if any, are examined in order of appearance to locate a suitable handler for the exception.</span></span> <span data-ttu-id="82896-553">Si una `catch` cláusula no especifica un tipo, o especifica el tipo de excepción o un tipo base del tipo de excepción:</span><span class="sxs-lookup"><span data-stu-id="82896-553">If a `catch` clause does not specify a type, or specifies the exception type or a base type of the exception type:</span></span>
      *  <span data-ttu-id="82896-554">Si la `catch` cláusula declara una variable de excepción, el objeto de excepción se asigna a la variable de excepción.</span><span class="sxs-lookup"><span data-stu-id="82896-554">If the `catch` clause declares an exception variable, the exception object is assigned to the exception variable.</span></span>
      *  <span data-ttu-id="82896-555">Si la `catch` cláusula declara un filtro de excepción, se evalúa el filtro.</span><span class="sxs-lookup"><span data-stu-id="82896-555">If the `catch` clause declares an exception filter, the filter is evaluated.</span></span> <span data-ttu-id="82896-556">Si se evalúa como `false`, la cláusula catch no es una coincidencia y la búsqueda continúa a través de las cláusulas posteriores `catch` de un controlador adecuado.</span><span class="sxs-lookup"><span data-stu-id="82896-556">If it evaluates to `false`, the catch clause is not a match, and the search continues through any subsequent `catch` clauses for a suitable handler.</span></span>
      *  <span data-ttu-id="82896-557">De lo contrario `catch` , la cláusula se considera una coincidencia y el control se transfiere al `catch` bloque coincidente.</span><span class="sxs-lookup"><span data-stu-id="82896-557">Otherwise, the `catch` clause is considered a match, and control is transferred to the matching `catch` block.</span></span>
      *  <span data-ttu-id="82896-558">When y si el control alcanza el punto final del `catch` bloque:</span><span class="sxs-lookup"><span data-stu-id="82896-558">When and if control reaches the end point of the `catch` block:</span></span>
         * <span data-ttu-id="82896-559">Si la `try` instrucción tiene un `finally` bloque, se `finally` ejecuta el bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-559">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         * <span data-ttu-id="82896-560">El control se transfiere al punto final de la `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-560">Control is transferred to the end point of the `try` statement.</span></span>
      *  <span data-ttu-id="82896-561">Si una excepción se propaga a la `try` instrucción durante la `catch` ejecución del bloque:</span><span class="sxs-lookup"><span data-stu-id="82896-561">If an exception is propagated to the `try` statement during execution of the `catch` block:</span></span>
         *  <span data-ttu-id="82896-562">Si la `try` instrucción tiene un `finally` bloque, se `finally` ejecuta el bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-562">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
         *  <span data-ttu-id="82896-563">La excepción se propaga a la siguiente instrucción de inclusión `try` .</span><span class="sxs-lookup"><span data-stu-id="82896-563">The exception is propagated to the next enclosing `try` statement.</span></span>
   *  <span data-ttu-id="82896-564">Si la `try` instrucción no `catch` tiene cláusulas o si ninguna `catch` cláusula coincide con la excepción:</span><span class="sxs-lookup"><span data-stu-id="82896-564">If the `try` statement has no `catch` clauses or if no `catch` clause matches the exception:</span></span>
      *  <span data-ttu-id="82896-565">Si la `try` instrucción tiene un `finally` bloque, se `finally` ejecuta el bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-565">If the `try` statement has a `finally` block, the `finally` block is executed.</span></span>
      *  <span data-ttu-id="82896-566">La excepción se propaga a la siguiente instrucción de inclusión `try` .</span><span class="sxs-lookup"><span data-stu-id="82896-566">The exception is propagated to the next enclosing `try` statement.</span></span>

<span data-ttu-id="82896-567">Las instrucciones de un `finally` bloque siempre se ejecutan cuando el control `try` sale de una instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-567">The statements of a `finally` block are always executed when control leaves a `try` statement.</span></span> <span data-ttu-id="82896-568">Esto es cierto si la transferencia de control se produce como resultado de la ejecución normal, como resultado de la ejecución `break`de `continue`una `goto`instrucción, `return` , o, o como resultado de propagar una excepción fuera de la `try` privacidad.</span><span class="sxs-lookup"><span data-stu-id="82896-568">This is true whether the control transfer occurs as a result of normal execution, as a result of executing a `break`, `continue`, `goto`, or `return` statement, or as a result of propagating an exception out of the `try` statement.</span></span>

<span data-ttu-id="82896-569">Si se produce una excepción durante la ejecución de `finally` un bloque y no se detecta en el mismo bloque Finally, la excepción se propaga a la siguiente instrucción envolvente `try` .</span><span class="sxs-lookup"><span data-stu-id="82896-569">If an exception is thrown during execution of a `finally` block, and is not caught within the same finally block, the exception is propagated to the next enclosing `try` statement.</span></span> <span data-ttu-id="82896-570">Si hay otra excepción en el proceso de propagación, se perderá esa excepción.</span><span class="sxs-lookup"><span data-stu-id="82896-570">If another exception was in the process of being propagated, that exception is lost.</span></span> <span data-ttu-id="82896-571">El proceso de propagación de una excepción se describe con más detalle en la descripción `throw` de la instrucción ([la instrucción throw](statements.md#the-throw-statement)).</span><span class="sxs-lookup"><span data-stu-id="82896-571">The process of propagating an exception is discussed further in the description of the `throw` statement ([The throw statement](statements.md#the-throw-statement)).</span></span>

<span data-ttu-id="82896-572">El `try` bloque de una `try` instrucción es accesible si la `try` instrucción es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-572">The `try` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="82896-573">Se `catch` puede tener acceso `try` a un bloque de una instrucción `try` si la instrucción es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-573">A `catch` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="82896-574">El `finally` bloque de una `try` instrucción es accesible si la `try` instrucción es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-574">The `finally` block of a `try` statement is reachable if the `try` statement is reachable.</span></span>

<span data-ttu-id="82896-575">El punto final de una `try` instrucción es accesible si se cumplen las dos condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="82896-575">The end point of a `try` statement is reachable if both of the following are true:</span></span>

*  <span data-ttu-id="82896-576">El punto final del `try` bloque es accesible o se puede tener acceso al punto final de al menos un `catch` bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-576">The end point of the `try` block is reachable or the end point of at least one `catch` block is reachable.</span></span>
*  <span data-ttu-id="82896-577">Si hay `finally` un bloque, se puede tener acceso al punto `finally` final del bloque.</span><span class="sxs-lookup"><span data-stu-id="82896-577">If a `finally` block is present, the end point of the `finally` block is reachable.</span></span>

## <a name="the-checked-and-unchecked-statements"></a><span data-ttu-id="82896-578">Las instrucciones checked y unchecked</span><span class="sxs-lookup"><span data-stu-id="82896-578">The checked and unchecked statements</span></span>

<span data-ttu-id="82896-579">Las `checked` instrucciones `unchecked` y se utilizan para controlar el ***contexto de comprobación de desbordamiento*** para conversiones y operaciones aritméticas de tipo entero.</span><span class="sxs-lookup"><span data-stu-id="82896-579">The `checked` and `unchecked` statements are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

<span data-ttu-id="82896-580">La `checked` instrucción hace que todas las expresiones del *bloque* se evalúen en un contexto comprobado, `unchecked` y la instrucción hace que todas las expresiones del *bloque* se evalúen en un contexto no comprobado.</span><span class="sxs-lookup"><span data-stu-id="82896-580">The `checked` statement causes all expressions in the *block* to be evaluated in a checked context, and the `unchecked` statement causes all expressions in the *block* to be evaluated in an unchecked context.</span></span>

<span data-ttu-id="82896-581">Las `checked` instrucciones `unchecked` y son exactamente equivalentes a `checked` los `unchecked` operadores y ([los operadores Checked y unchecked](expressions.md#the-checked-and-unchecked-operators)), salvo que operan en bloques en lugar de en expresiones.</span><span class="sxs-lookup"><span data-stu-id="82896-581">The `checked` and `unchecked` statements are precisely equivalent to the `checked` and `unchecked` operators ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)), except that they operate on blocks instead of expressions.</span></span>

## <a name="the-lock-statement"></a><span data-ttu-id="82896-582">La instrucción lock</span><span class="sxs-lookup"><span data-stu-id="82896-582">The lock statement</span></span>

<span data-ttu-id="82896-583">La `lock` instrucción obtiene el bloqueo de exclusión mutua para un objeto determinado, ejecuta una instrucción y, a continuación, libera el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="82896-583">The `lock` statement obtains the mutual-exclusion lock for a given object, executes a statement, and then releases the lock.</span></span>

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

<span data-ttu-id="82896-584">La expresión de una instrucción `lock` debe indicar un valor de un tipo conocido como *reference_type*.</span><span class="sxs-lookup"><span data-stu-id="82896-584">The expression of a `lock` statement must denote a value of a type known to be a *reference_type*.</span></span> <span data-ttu-id="82896-585">No se realiza ninguna conversión boxing implícita ([conversiones boxing](conversions.md#boxing-conversions)) en la expresión de una instrucción `lock` y, por lo tanto, es un error en tiempo de compilación para que la expresión denote un valor de *value_type*.</span><span class="sxs-lookup"><span data-stu-id="82896-585">No implicit boxing conversion ([Boxing conversions](conversions.md#boxing-conversions)) is ever performed for the expression of a `lock` statement, and thus it is a compile-time error for the expression to denote a value of a *value_type*.</span></span>

<span data-ttu-id="82896-586">Una `lock` instrucción con el formato</span><span class="sxs-lookup"><span data-stu-id="82896-586">A `lock` statement of the form</span></span>
```csharp
lock (x) ...
```
<span data-ttu-id="82896-587">donde `x` es una expresión de *reference_type*, es exactamente equivalente a</span><span class="sxs-lookup"><span data-stu-id="82896-587">where `x` is an expression of a *reference_type*, is precisely equivalent to</span></span>
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
<span data-ttu-id="82896-588">salvo que `x` solo se evalúa una vez.</span><span class="sxs-lookup"><span data-stu-id="82896-588">except that `x` is only evaluated once.</span></span>

<span data-ttu-id="82896-589">Mientras se mantiene un bloqueo de exclusión mutua, el código que se ejecuta en el mismo subproceso de ejecución también puede obtener y liberar el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="82896-589">While a mutual-exclusion lock is held, code executing in the same execution thread can also obtain and release the lock.</span></span> <span data-ttu-id="82896-590">Sin embargo, el código que se ejecuta en otros subprocesos no obtiene el bloqueo hasta que se libera el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="82896-590">However, code executing in other threads is blocked from obtaining the lock until the lock is released.</span></span>

<span data-ttu-id="82896-591">No `System.Type` se recomienda el bloqueo de objetos para sincronizar el acceso a los datos estáticos.</span><span class="sxs-lookup"><span data-stu-id="82896-591">Locking `System.Type` objects in order to synchronize access to static data is not recommended.</span></span> <span data-ttu-id="82896-592">Otro código podría bloquearse en el mismo tipo, lo que puede provocar un interbloqueo.</span><span class="sxs-lookup"><span data-stu-id="82896-592">Other code might lock on the same type, which can result in deadlock.</span></span> <span data-ttu-id="82896-593">Un mejor enfoque es sincronizar el acceso a los datos estáticos bloqueando un objeto estático privado.</span><span class="sxs-lookup"><span data-stu-id="82896-593">A better approach is to synchronize access to static data by locking a private static object.</span></span> <span data-ttu-id="82896-594">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="82896-594">For example:</span></span>
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

## <a name="the-using-statement"></a><span data-ttu-id="82896-595">La instrucción using</span><span class="sxs-lookup"><span data-stu-id="82896-595">The using statement</span></span>

<span data-ttu-id="82896-596">La `using` instrucción obtiene uno o más recursos, ejecuta una instrucción y, a continuación, desecha el recurso.</span><span class="sxs-lookup"><span data-stu-id="82896-596">The `using` statement obtains one or more resources, executes a statement, and then disposes of the resource.</span></span>

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

<span data-ttu-id="82896-597">Un ***recurso*** es una clase o estructura que implementa `System.IDisposable`, que incluye un único método sin parámetros denominado. `Dispose`</span><span class="sxs-lookup"><span data-stu-id="82896-597">A ***resource*** is a class or struct that implements `System.IDisposable`, which includes a single parameterless method named `Dispose`.</span></span> <span data-ttu-id="82896-598">El código que usa un recurso puede llamar `Dispose` a para indicar que el recurso ya no es necesario.</span><span class="sxs-lookup"><span data-stu-id="82896-598">Code that is using a resource can call `Dispose` to indicate that the resource is no longer needed.</span></span> <span data-ttu-id="82896-599">Si `Dispose` no se llama a, la eliminación automática se produce finalmente como consecuencia de la recolección de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="82896-599">If `Dispose` is not called, then automatic disposal eventually occurs as a consequence of garbage collection.</span></span>

<span data-ttu-id="82896-600">Si el formato de *resource_acquisition* es *local_variable_declaration* , el tipo de *local_variable_declaration* debe ser `dynamic` o un tipo que se pueda convertir implícitamente a `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="82896-600">If the form of *resource_acquisition* is *local_variable_declaration* then the type of the *local_variable_declaration* must be either `dynamic` or a type that can be implicitly converted to `System.IDisposable`.</span></span> <span data-ttu-id="82896-601">Si el formato de *resource_acquisition* es *Expression* , esta expresión debe poder convertirse implícitamente a `System.IDisposable`.</span><span class="sxs-lookup"><span data-stu-id="82896-601">If the form of *resource_acquisition* is *expression* then this expression must be implicitly convertible to `System.IDisposable`.</span></span>

<span data-ttu-id="82896-602">Las variables locales declaradas en un *resource_acquisition* son de solo lectura y deben incluir un inicializador.</span><span class="sxs-lookup"><span data-stu-id="82896-602">Local variables declared in a *resource_acquisition* are read-only, and must include an initializer.</span></span> <span data-ttu-id="82896-603">Se produce un error en tiempo de compilación si la instrucción incrustada intenta modificar estas variables locales (a través `++` de `--` la asignación o los operadores y), tomar la dirección de ellas `ref` o `out` pasarlas como parámetros o.</span><span class="sxs-lookup"><span data-stu-id="82896-603">A compile-time error occurs if the embedded statement attempts to modify these local variables (via assignment or the `++` and `--` operators) , take the address of them, or pass them as `ref` or `out` parameters.</span></span>

<span data-ttu-id="82896-604">Una `using` instrucción se traduce en tres partes: adquisición, uso y eliminación.</span><span class="sxs-lookup"><span data-stu-id="82896-604">A `using` statement is translated into three parts: acquisition, usage, and disposal.</span></span> <span data-ttu-id="82896-605">El uso del recurso se adjunta implícitamente en una `try` instrucción que incluye una `finally` cláusula.</span><span class="sxs-lookup"><span data-stu-id="82896-605">Usage of the resource is implicitly enclosed in a `try` statement that includes a `finally` clause.</span></span> <span data-ttu-id="82896-606">Esta `finally` cláusula desecha el recurso.</span><span class="sxs-lookup"><span data-stu-id="82896-606">This `finally` clause disposes of the resource.</span></span> <span data-ttu-id="82896-607">Si se `null` adquiere un recurso, no se realiza ninguna `Dispose` llamada a y no se produce ninguna excepción.</span><span class="sxs-lookup"><span data-stu-id="82896-607">If a `null` resource is acquired, then no call to `Dispose` is made, and no exception is thrown.</span></span> <span data-ttu-id="82896-608">Si el recurso es de tipo `dynamic` , se convierte dinámicamente a través de una conversión dinámica implícita ([conversiones dinámicas implícitas](conversions.md#implicit-dynamic-conversions)) en `IDisposable` durante la adquisición para asegurarse de que la conversión se realiza correctamente antes del uso y elimina.</span><span class="sxs-lookup"><span data-stu-id="82896-608">If the resource is of type `dynamic` it is dynamically converted through an implicit dynamic conversion ([Implicit dynamic conversions](conversions.md#implicit-dynamic-conversions)) to `IDisposable` during acquisition in order to ensure that the conversion is successful before the usage and disposal.</span></span>

<span data-ttu-id="82896-609">Una `using` instrucción con el formato</span><span class="sxs-lookup"><span data-stu-id="82896-609">A `using` statement of the form</span></span>
```csharp
using (ResourceType resource = expression) statement
```
<span data-ttu-id="82896-610">corresponde a una de las tres expansiones posibles.</span><span class="sxs-lookup"><span data-stu-id="82896-610">corresponds to one of three possible expansions.</span></span> <span data-ttu-id="82896-611">Cuando `ResourceType` es un tipo de valor que no acepta valores NULL, la expansión es</span><span class="sxs-lookup"><span data-stu-id="82896-611">When `ResourceType` is a non-nullable value type, the expansion is</span></span>
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

<span data-ttu-id="82896-612">De lo contrario `ResourceType` , cuando es un tipo de valor que acepta valores NULL o `dynamic`un tipo de referencia distinto de, la expansión es</span><span class="sxs-lookup"><span data-stu-id="82896-612">Otherwise, when `ResourceType` is a nullable value type or a reference type other than `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="82896-613">De lo contrario `ResourceType` , `dynamic`cuando es, la expansión es</span><span class="sxs-lookup"><span data-stu-id="82896-613">Otherwise, when `ResourceType` is `dynamic`, the expansion is</span></span>
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

<span data-ttu-id="82896-614">En cualquier expansión, la `resource` variable es de solo lectura en la instrucción incrustada, y `d` no se puede obtener acceso a la variable en la instrucción incrustada y no es visible para ella.</span><span class="sxs-lookup"><span data-stu-id="82896-614">In either expansion, the `resource` variable is read-only in the embedded statement, and the `d` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="82896-615">Una implementación puede implementar una instrucción using determinada de forma diferente, por ejemplo, por motivos de rendimiento, siempre que el comportamiento sea coherente con la expansión anterior.</span><span class="sxs-lookup"><span data-stu-id="82896-615">An implementation is permitted to implement a given using-statement differently, e.g. for performance reasons, as long as the behavior is consistent with the above expansion.</span></span>

<span data-ttu-id="82896-616">Una `using` instrucción con el formato</span><span class="sxs-lookup"><span data-stu-id="82896-616">A `using` statement of the form</span></span>
```csharp
using (expression) statement
```
<span data-ttu-id="82896-617">tiene las mismas tres expansiones posibles.</span><span class="sxs-lookup"><span data-stu-id="82896-617">has the same three possible expansions.</span></span> <span data-ttu-id="82896-618">En este caso `ResourceType` `expression`, es implícitamente el tipo en tiempo de compilación de, si tiene uno.</span><span class="sxs-lookup"><span data-stu-id="82896-618">In this case `ResourceType` is implicitly the compile-time type of the `expression`, if it has one.</span></span> <span data-ttu-id="82896-619">En caso contrario `IDisposable` , la propia interfaz se `ResourceType`utiliza como.</span><span class="sxs-lookup"><span data-stu-id="82896-619">Otherwise the interface `IDisposable` itself is used as the `ResourceType`.</span></span> <span data-ttu-id="82896-620">No `resource` se puede obtener acceso a la variable en y no es visible para la instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="82896-620">The `resource` variable is inaccessible in, and invisible to, the embedded statement.</span></span>

<span data-ttu-id="82896-621">Cuando un *resource_acquisition* toma la forma de un *local_variable_declaration*, es posible adquirir varios recursos de un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="82896-621">When a *resource_acquisition* takes the form of a *local_variable_declaration*, it is possible to acquire multiple resources of a given type.</span></span> <span data-ttu-id="82896-622">Una `using` instrucción con el formato</span><span class="sxs-lookup"><span data-stu-id="82896-622">A `using` statement of the form</span></span>
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
<span data-ttu-id="82896-623">es exactamente equivalente a una secuencia de `using` instrucciones anidadas:</span><span class="sxs-lookup"><span data-stu-id="82896-623">is precisely equivalent to a sequence of nested `using` statements:</span></span>
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

<span data-ttu-id="82896-624">En el ejemplo siguiente se crea un `log.txt` archivo denominado y se escriben dos líneas de texto en el archivo.</span><span class="sxs-lookup"><span data-stu-id="82896-624">The example below creates a file named `log.txt` and writes two lines of text to the file.</span></span> <span data-ttu-id="82896-625">A continuación, el ejemplo abre el mismo archivo para leer y copia las líneas de texto contenidas en la consola.</span><span class="sxs-lookup"><span data-stu-id="82896-625">The example then opens that same file for reading and copies the contained lines of text to the console.</span></span>
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

<span data-ttu-id="82896-626">Dado que `TextWriter` las `TextReader` clases y implementan la `IDisposable` interfaz, el ejemplo `using` puede utilizar instrucciones para asegurarse de que el archivo subyacente está cerrado correctamente después de las operaciones de escritura o lectura.</span><span class="sxs-lookup"><span data-stu-id="82896-626">Since the `TextWriter` and `TextReader` classes implement the `IDisposable` interface, the example can use `using` statements to ensure that the underlying file is properly closed following the write or read operations.</span></span>

## <a name="the-yield-statement"></a><span data-ttu-id="82896-627">La instrucción yield</span><span class="sxs-lookup"><span data-stu-id="82896-627">The yield statement</span></span>

<span data-ttu-id="82896-628">La `yield` instrucción se usa en un bloque de iteradores ([bloques](statements.md#blocks)) para obtener un valor para el objeto enumerador ([objetos enumerador](classes.md#enumerator-objects)) o el objeto enumerable ([objetos enumerables](classes.md#enumerable-objects)) de un iterador o para señalar el final de la iteración.</span><span class="sxs-lookup"><span data-stu-id="82896-628">The `yield` statement is used in an iterator block ([Blocks](statements.md#blocks)) to yield a value to the enumerator object ([Enumerator objects](classes.md#enumerator-objects)) or enumerable object ([Enumerable objects](classes.md#enumerable-objects)) of an iterator or to signal the end of the iteration.</span></span>

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

<span data-ttu-id="82896-629">`yield`no es una palabra reservada; tiene un significado especial solo cuando se usa inmediatamente antes `return` de `break` una palabra clave o.</span><span class="sxs-lookup"><span data-stu-id="82896-629">`yield` is not a reserved word; it has special meaning only when used immediately before a `return` or `break` keyword.</span></span> <span data-ttu-id="82896-630">En otros contextos, `yield` se puede usar como identificador.</span><span class="sxs-lookup"><span data-stu-id="82896-630">In other contexts, `yield` can be used as an identifier.</span></span>

<span data-ttu-id="82896-631">Hay varias restricciones sobre dónde puede aparecer `yield` una instrucción, tal y como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="82896-631">There are several restrictions on where a `yield` statement can appear, as described in the following.</span></span>

*  <span data-ttu-id="82896-632">Es un error en tiempo de compilación que una instrucción `yield` (de cualquier forma) aparezca fuera de *method_body*, *operator_body* o *accessor_body*</span><span class="sxs-lookup"><span data-stu-id="82896-632">It is a compile-time error for a `yield` statement (of either form) to appear outside a *method_body*, *operator_body* or *accessor_body*</span></span>
*  <span data-ttu-id="82896-633">Es un error en tiempo de compilación para una `yield` instrucción (de cualquier forma) que aparezca dentro de una función anónima.</span><span class="sxs-lookup"><span data-stu-id="82896-633">It is a compile-time error for a `yield` statement (of either form) to appear inside an anonymous function.</span></span>
*  <span data-ttu-id="82896-634">Es un error en tiempo de compilación para una `yield` instrucción (de cualquier forma) que aparezca en la `finally` cláusula de una `try` instrucción.</span><span class="sxs-lookup"><span data-stu-id="82896-634">It is a compile-time error for a `yield` statement (of either form) to appear in the `finally` clause of a `try` statement.</span></span>
*  <span data-ttu-id="82896-635">Se trata de un error en tiempo de compilación `yield return` para que una instrucción aparezca en `try` cualquier parte de una `catch` instrucción que contenga cláusulas.</span><span class="sxs-lookup"><span data-stu-id="82896-635">It is a compile-time error for a `yield return` statement to appear anywhere in a `try` statement that contains any `catch` clauses.</span></span>

<span data-ttu-id="82896-636">En el ejemplo siguiente se muestran algunos usos válidos `yield` y no válidos de las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="82896-636">The following example shows some valid and invalid uses of `yield` statements.</span></span>

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

<span data-ttu-id="82896-637">Debe existir una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo de la expresión en la `yield return` instrucción al tipo yield ([yield Type](classes.md#yield-type)) del iterador.</span><span class="sxs-lookup"><span data-stu-id="82896-637">An implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) must exist from the type of the expression in the `yield return` statement to the yield type ([Yield type](classes.md#yield-type)) of the iterator.</span></span>

<span data-ttu-id="82896-638">Una `yield return` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-638">A `yield return` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-639">La expresión dada en la instrucción se evalúa, se convierte implícitamente al tipo Yield y se asigna a la `Current` propiedad del objeto enumerador.</span><span class="sxs-lookup"><span data-stu-id="82896-639">The expression given in the statement is evaluated, implicitly converted to the yield type, and assigned to the `Current` property of the enumerator object.</span></span>
*  <span data-ttu-id="82896-640">La ejecución del bloque de iterador está suspendida.</span><span class="sxs-lookup"><span data-stu-id="82896-640">Execution of the iterator block is suspended.</span></span> <span data-ttu-id="82896-641">Si la `yield return` instrucción está dentro de uno o `try` más bloques, los `finally` bloques asociados no se ejecutan en este momento.</span><span class="sxs-lookup"><span data-stu-id="82896-641">If the `yield return` statement is within one or more `try` blocks, the associated `finally` blocks are not executed at this time.</span></span>
*  <span data-ttu-id="82896-642">El `MoveNext` método del objeto de enumerador `true` vuelve a su llamador, lo que indica que el objeto de enumerador avanzó correctamente al siguiente elemento.</span><span class="sxs-lookup"><span data-stu-id="82896-642">The `MoveNext` method of the enumerator object returns `true` to its caller, indicating that the enumerator object successfully advanced to the next item.</span></span>

<span data-ttu-id="82896-643">La siguiente llamada al método del `MoveNext` objeto de enumerador reanuda la ejecución del bloque de iterador desde donde se suspendió por última vez.</span><span class="sxs-lookup"><span data-stu-id="82896-643">The next call to the enumerator object's `MoveNext` method resumes execution of the iterator block from where it was last suspended.</span></span>

<span data-ttu-id="82896-644">Una `yield break` instrucción se ejecuta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="82896-644">A `yield break` statement is executed as follows:</span></span>

*  <span data-ttu-id="82896-645">Si la `yield break` instrucción está delimitada por uno o `try` más bloques con `finally` bloques asociados, el `finally` control se transfiere inicialmente al bloque de la `try` instrucción más interna.</span><span class="sxs-lookup"><span data-stu-id="82896-645">If the `yield break` statement is enclosed by one or more `try` blocks with associated `finally` blocks, control is initially transferred to the `finally` block of the innermost `try` statement.</span></span> <span data-ttu-id="82896-646">Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión.</span><span class="sxs-lookup"><span data-stu-id="82896-646">When and if control reaches the end point of a `finally` block, control is transferred to the `finally` block of the next enclosing `try` statement.</span></span> <span data-ttu-id="82896-647">Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones envolventes.</span><span class="sxs-lookup"><span data-stu-id="82896-647">This process is repeated until the `finally` blocks of all enclosing `try` statements have been executed.</span></span>
*  <span data-ttu-id="82896-648">El control se devuelve al llamador del bloque de iteradores.</span><span class="sxs-lookup"><span data-stu-id="82896-648">Control is returned to the caller of the iterator block.</span></span> <span data-ttu-id="82896-649">Este es el `MoveNext` método o `Dispose` el método del objeto de enumerador.</span><span class="sxs-lookup"><span data-stu-id="82896-649">This is either the `MoveNext` method or `Dispose` method of the enumerator object.</span></span>

<span data-ttu-id="82896-650">Dado que `yield break` una instrucción transfiere el control incondicionalmente a otra parte, el `yield break` punto final de una instrucción nunca es accesible.</span><span class="sxs-lookup"><span data-stu-id="82896-650">Because a `yield break` statement unconditionally transfers control elsewhere, the end point of a `yield break` statement is never reachable.</span></span>
