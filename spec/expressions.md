---
ms.openlocfilehash: 75454072a5137b3044f78bb896317fd88a29e336
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "49640917"
---
# <a name="expressions"></a><span data-ttu-id="d7bba-101">Expresiones</span><span class="sxs-lookup"><span data-stu-id="d7bba-101">Expressions</span></span>

<span data-ttu-id="d7bba-102">Una expresión es una secuencia de operadores y operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="d7bba-103">Este capítulo define la sintaxis, el orden de evaluación de operandos y operadores y el significado de las expresiones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="d7bba-104">Clasificaciones de expresiones</span><span class="sxs-lookup"><span data-stu-id="d7bba-104">Expression classifications</span></span>

<span data-ttu-id="d7bba-105">Una expresión se clasifica de las siguientes formas:</span><span class="sxs-lookup"><span data-stu-id="d7bba-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="d7bba-106">Un valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-106">A value.</span></span> <span data-ttu-id="d7bba-107">Todos los valores tienen un tipo asociado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="d7bba-108">Una variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-108">A variable.</span></span> <span data-ttu-id="d7bba-109">Cada variable tiene un tipo asociado, es decir, el tipo declarado de la variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="d7bba-110">Espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="d7bba-110">A namespace.</span></span> <span data-ttu-id="d7bba-111">Una expresión con esta clasificación solo puede aparecer como el lado izquierdo de un *member_access* ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="d7bba-112">En cualquier otro contexto, una expresión que se clasifica como un espacio de nombres produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="d7bba-113">Un tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-113">A type.</span></span> <span data-ttu-id="d7bba-114">Una expresión con esta clasificación solo puede aparecer como el lado izquierdo de un *member_access* ([acceso a miembros](expressions.md#member-access)), o como operando para el `as` operador ([el operador as ](expressions.md#the-as-operator)), el `is` operador ([el operador is](expressions.md#the-is-operator)), o la `typeof` operador ([el operador typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="d7bba-115">En cualquier otro contexto, una expresión que se clasifica como un tipo produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="d7bba-116">Un grupo de métodos, que es un conjunto de métodos sobrecargados procedente de una búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="d7bba-117">Un grupo de métodos puede tener una expresión de instancia asociada y una lista de argumentos de tipo asociado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="d7bba-118">Cuando se invoca un método de instancia, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="d7bba-119">Se permite un grupo de métodos en un *invocation_expression* ([expresiones de invocación](expressions.md#invocation-expressions)), un *delegate_creation_expression* ([delegar la creación expresiones](expressions.md#delegate-creation-expressions)) y como el lado izquierdo de un operador y se puede convertir implícitamente a un tipo delegado compatible ([conversiones de métodos de grupo](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="d7bba-120">En cualquier otro contexto, una expresión que se clasifica como un grupo de métodos produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="d7bba-121">Un literal null.</span><span class="sxs-lookup"><span data-stu-id="d7bba-121">A null literal.</span></span> <span data-ttu-id="d7bba-122">Una expresión con esta clasificación se puede convertir implícitamente a un tipo de referencia o tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="d7bba-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="d7bba-123">Una función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-123">An anonymous function.</span></span> <span data-ttu-id="d7bba-124">Una expresión con esta clasificación puede convertirse implícitamente en un tipo de árbol de expresión o delegado compatible.</span><span class="sxs-lookup"><span data-stu-id="d7bba-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="d7bba-125">Acceso a una propiedad.</span><span class="sxs-lookup"><span data-stu-id="d7bba-125">A property access.</span></span> <span data-ttu-id="d7bba-126">Cada acceso de propiedad tiene un tipo asociado, el tipo declarado de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="d7bba-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="d7bba-127">Además, el acceso a una propiedad puede tener una expresión de instancia asociada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="d7bba-128">Cuando un descriptor de acceso (la `get` o `set` bloque) de una instancia se invoca el acceso a la propiedad, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="d7bba-129">Acceso de un evento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-129">An event access.</span></span> <span data-ttu-id="d7bba-130">Cada acceso de eventos tiene un tipo asociado, es decir, el tipo del evento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="d7bba-131">Además, un acceso de eventos puede tener una expresión de instancia asociada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="d7bba-132">Un acceso a evento puede aparecer como el operando izquierdo de la `+=` y `-=` operadores ([asignación de evento](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="d7bba-133">En cualquier otro contexto, una expresión que se clasifica como un acceso a evento produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="d7bba-134">Acceso de un indizador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-134">An indexer access.</span></span> <span data-ttu-id="d7bba-135">Cada acceso al indizador tiene un tipo asociado, es decir, el tipo de elemento del indizador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="d7bba-136">Además, un acceso de indizador tiene una expresión de instancia asociada y una lista de argumentos asociada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="d7bba-137">Cuando un descriptor de acceso (la `get` o `set` bloque) de un indizador se invoca el acceso, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)) y el resultado de evaluación de la lista de argumentos se convierte en la lista de parámetros de la invocación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="d7bba-138">No hay nada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-138">Nothing.</span></span> <span data-ttu-id="d7bba-139">Esto se produce cuando la expresión es una invocación de un método con un tipo de valor devuelto de `void`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="d7bba-140">Una expresión que se clasifica como nada sólo es válida en el contexto de un *statement_expression* ([las instrucciones de expresión](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="d7bba-141">El resultado final de una expresión nunca es un espacio de nombres, tipo, grupo de métodos o acceso de eventos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="d7bba-142">En su lugar, como se mencionó anteriormente, estas categorías de expresiones son construcciones intermedias que solo se permiten en ciertos contextos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="d7bba-143">Un acceso de propiedad o indizador siempre es reclasificar como un valor mediante la realización de una invocación de la *descriptor de acceso get* o *descriptor de acceso set*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="d7bba-144">El descriptor de acceso concreto viene determinada por el contexto del acceso de la propiedad o indizador: Si el acceso es el destino de una asignación, el *descriptor de acceso set* se invoca para asignar un nuevo valor ([asignación Simple](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="d7bba-145">En caso contrario, el *descriptor de acceso get* se invoca para obtener el valor actual ([valores de expresiones](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="d7bba-146">Valores de expresiones</span><span class="sxs-lookup"><span data-stu-id="d7bba-146">Values of expressions</span></span>

<span data-ttu-id="d7bba-147">La mayoría de las construcciones que implican una expresión requiere que en última instancia, la expresión para denotar un ***valor***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="d7bba-148">En tales casos, si la expresión real denota un espacio de nombres, un tipo, un grupo de métodos o nada, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="d7bba-149">Sin embargo, si la expresión denota un acceso de propiedad, un acceso de indizador o una variable, se sustituye implícitamente el valor de la propiedad, indizador o variable:</span><span class="sxs-lookup"><span data-stu-id="d7bba-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="d7bba-150">El valor de una variable es simplemente el valor almacenado actualmente en la ubicación de almacenamiento identificada por la variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="d7bba-151">Una variable se considera asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) antes de que se puede obtener su valor o, de lo contrario se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-152">El valor de una expresión de acceso de propiedad se obtiene al invocar el *descriptor de acceso get* de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="d7bba-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="d7bba-153">Si la propiedad no tiene ningún *descriptor de acceso get*, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="d7bba-154">En caso contrario, una invocación de miembros de función ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se realiza, y el resultado de la invocación se convierte en el valor de la expresión de acceso de propiedad.</span><span class="sxs-lookup"><span data-stu-id="d7bba-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="d7bba-155">El valor de una expresión de acceso de indizador se obtiene al invocar el *descriptor de acceso get* del indizador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="d7bba-156">Si el indizador no tiene ningún *descriptor de acceso get*, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="d7bba-157">En caso contrario, una invocación de miembros de función ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se realiza con el argumento de lista asociada con la expresión de acceso de indizador, y el resultado de la invocación se convierte en el valor de la expresión de acceso de indizador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="d7bba-158">Enlaces estáticos y dinámicos</span><span class="sxs-lookup"><span data-stu-id="d7bba-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="d7bba-159">El proceso de determinar el significado de una operación basada en el tipo o valor de expresiones constituyentes (argumentos, operandos, receptores) a menudo se conoce como ***enlace***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="d7bba-160">Por ejemplo el significado de una llamada al método se determina según el tipo de los argumentos y el receptor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="d7bba-161">El significado de un operador se determina según el tipo de sus operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="d7bba-162">En el significado de una operación normalmente se determina en tiempo de compilación, en función del tipo de tiempo de compilación de sus expresiones constituyentes de C#.</span><span class="sxs-lookup"><span data-stu-id="d7bba-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="d7bba-163">Del mismo modo, si una expresión contiene un error, el error se ha detectado y notificado por el compilador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="d7bba-164">Este enfoque se conoce como ***enlace estático***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="d7bba-165">Sin embargo, si una expresión es una expresión dinámica (es decir, tiene el tipo `dynamic`) indica que cualquier enlace que participa en debe basarse en su tipo en tiempo de ejecución (es decir, el tipo real del objeto indica en tiempo de ejecución) en lugar del tipo tiene en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="d7bba-166">Por lo tanto, el enlace de este tipo de operación se aplaza hasta el momento en que la operación es que se ejecutará durante la ejecución del programa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="d7bba-167">Esto se conoce como ***enlace dinámico***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="d7bba-168">Cuando una operación se enlaza dinámicamente, poca o ninguna comprobación se realiza por el compilador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="d7bba-169">En su lugar, si se produce un error en el enlace en tiempo de ejecución, se notifican errores como excepciones en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d7bba-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="d7bba-170">Las siguientes operaciones en C# están sujetos a enlace:</span><span class="sxs-lookup"><span data-stu-id="d7bba-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="d7bba-171">Acceso a miembros: `e.M`</span><span class="sxs-lookup"><span data-stu-id="d7bba-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="d7bba-172">Invocación de método: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="d7bba-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="d7bba-173">Invocación del delegado:`e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="d7bba-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="d7bba-174">Acceso de elemento: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="d7bba-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="d7bba-175">Creación de objetos: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="d7bba-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="d7bba-176">Sobrecargar operadores unarios: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="d7bba-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="d7bba-177">Sobrecargar operadores binarios: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span><span class="sxs-lookup"><span data-stu-id="d7bba-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="d7bba-178">Operadores de asignación: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span><span class="sxs-lookup"><span data-stu-id="d7bba-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="d7bba-179">Conversiones implícitas y explícitas</span><span class="sxs-lookup"><span data-stu-id="d7bba-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="d7bba-180">Cuando no hay implicada ninguna expresión dinámica, C# tiene como valor predeterminado para el enlace estático, lo que significa que los tipos de tiempo de compilación de expresiones constituyentes se usan en el proceso de selección.</span><span class="sxs-lookup"><span data-stu-id="d7bba-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="d7bba-181">Sin embargo, cuando una de las expresiones constituyentes en las operaciones enumeradas anteriormente es una expresión dinámica, la operación en su lugar se enlaza dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="d7bba-182">Tiempo de enlace</span><span class="sxs-lookup"><span data-stu-id="d7bba-182">Binding-time</span></span>

<span data-ttu-id="d7bba-183">Enlace estático lleva a cabo en tiempo de compilación, mientras que el enlace dinámico realiza en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d7bba-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="d7bba-184">En las secciones siguientes, el término ***tiempo de enlace*** se refiere al tiempo de compilación o tiempo de ejecución, dependiendo de cuándo realiza el enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="d7bba-185">El ejemplo siguiente muestra las nociones de enlaces estáticos y dinámicos y de tiempo de enlace:</span><span class="sxs-lookup"><span data-stu-id="d7bba-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="d7bba-186">Las dos primeras llamadas están enlazadas estáticamente: la sobrecarga de `Console.WriteLine` se selecciona basándose en el tipo de tiempo de compilación de su argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="d7bba-187">Por lo tanto, el tiempo de enlace es el tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="d7bba-188">La tercera llamada se enlaza dinámicamente: la sobrecarga de `Console.WriteLine` se selecciona basándose en el tipo de tiempo de ejecución de su argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="d7bba-189">Esto sucede porque el argumento es una expresión dinámica, que es su tipo en tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="d7bba-190">Por lo tanto, el tiempo de enlace para la tercera llamada es el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d7bba-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="d7bba-191">Enlace dinámico</span><span class="sxs-lookup"><span data-stu-id="d7bba-191">Dynamic binding</span></span>

<span data-ttu-id="d7bba-192">El propósito de enlace dinámico es permitir que los programas de C# interactuar con ***objetos dinámicos***, es decir, sistema de tipos de objetos que no siguen las reglas normales de C#.</span><span class="sxs-lookup"><span data-stu-id="d7bba-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="d7bba-193">Objetos dinámicos pueden ser objetos de otros lenguajes de programación con los sistemas de tipos diferentes, o pueden ser objetos que se configuran para implementar su propia semántica de enlace de distintas operaciones de mediante programación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="d7bba-194">El mecanismo por el que un objeto dinámico implementa su propia semántica es implementación definida.</span><span class="sxs-lookup"><span data-stu-id="d7bba-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="d7bba-195">Una interfaz determinada--nuevo implementación definida, se implementa mediante objetos dinámicos para señalar a la C#-tiempo de ejecución que tienen una semántica especial.</span><span class="sxs-lookup"><span data-stu-id="d7bba-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="d7bba-196">Por lo tanto, siempre que las operaciones en un objeto dinámico se enlazan dinámicamente, su propia semántica de enlace, en lugar de los de C#, tal como se especifica en este documento, asumir.</span><span class="sxs-lookup"><span data-stu-id="d7bba-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="d7bba-197">Aunque el propósito de enlace dinámico es permitir la interoperación con objetos dinámicos, C# permite a enlace dinámico en todos los objetos, ya sean dinámicos o no.</span><span class="sxs-lookup"><span data-stu-id="d7bba-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="d7bba-198">Esto permite una integración más fluida de los objetos dinámicos, como los resultados de operaciones en ellos no ellos mismos pueden ser objetos dinámicos, pero son todavía de un tipo desconocido para el programador en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="d7bba-199">También enlace dinámico puede ayudar a eliminar código basado en reflexión propensas a errores incluso cuando no hay objetos implicados son objetos dinámicos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="d7bba-200">Las secciones siguientes describen para cada construcción en el lenguaje exactamente cuando enlace dinámico se aplica, lo que compile la comprobación del tiempo: si se aplica, y es lo que la clasificación de resultados y la expresión de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="d7bba-201">Tipos de expresiones constituyentes</span><span class="sxs-lookup"><span data-stu-id="d7bba-201">Types of constituent expressions</span></span>

<span data-ttu-id="d7bba-202">Cuando una operación estáticamente está enlazada, el tipo de una expresión que lo constituyen (por ejemplo, un receptor y argumento, un índice o un operando) siempre se considera el tipo de tiempo de compilación de dicha expresión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, and argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="d7bba-203">Cuando una operación se enlaza dinámicamente, se determina el tipo de una expresión constituyente de maneras diferentes según el tipo de tiempo de compilación de la expresión que lo constituyen:</span><span class="sxs-lookup"><span data-stu-id="d7bba-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="d7bba-204">Una expresión de tipo en tiempo de compilación constituyente `dynamic` se considera que tiene el tipo del valor real que la expresión se evalúa como en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="d7bba-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="d7bba-205">Una expresión que lo constituyen cuyo tipo de tiempo de compilación es un parámetro de tipo se considera que tiene el tipo que está enlazado el parámetro de tipo en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="d7bba-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="d7bba-206">En caso contrario, la expresión constituyente se considera que tiene su tipo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="d7bba-207">Operadores</span><span class="sxs-lookup"><span data-stu-id="d7bba-207">Operators</span></span>

<span data-ttu-id="d7bba-208">Las expresiones se construyen a partir ***operandos*** y ***operadores***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="d7bba-209">Los operadores de una expresión indican qué operaciones se aplican a los operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="d7bba-210">Ejemplos de operadores incluyen `+`, `-`, `*`, `/` y `new`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="d7bba-211">Algunos ejemplos de operandos son literales, campos, variables locales y expresiones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="d7bba-212">Hay tres tipos de operadores:</span><span class="sxs-lookup"><span data-stu-id="d7bba-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="d7bba-213">Operadores unarios.</span><span class="sxs-lookup"><span data-stu-id="d7bba-213">Unary operators.</span></span> <span data-ttu-id="d7bba-214">Los operadores unarios toman un operando y usar la notación de prefijo (como `--x`) o la notación de postfijo (como `x++`).</span><span class="sxs-lookup"><span data-stu-id="d7bba-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="d7bba-215">Operadores binarios.</span><span class="sxs-lookup"><span data-stu-id="d7bba-215">Binary operators.</span></span> <span data-ttu-id="d7bba-216">Los operadores binarios tienen dos operandos y utilizan una notación infija (como `x + y`).</span><span class="sxs-lookup"><span data-stu-id="d7bba-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="d7bba-217">operador ternario.</span><span class="sxs-lookup"><span data-stu-id="d7bba-217">Ternary operator.</span></span> <span data-ttu-id="d7bba-218">Solo un operador ternario, `?:`, existe; toma tres operandos y utiliza notación infija (`c ? x : y`).</span><span class="sxs-lookup"><span data-stu-id="d7bba-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="d7bba-219">El orden de evaluación de los operadores en una expresión viene determinada por la ***prioridad*** y ***asociatividad*** de los operadores ([prioridad y asociatividad](expressions.md#operator-precedence-and-associativity)) .</span><span class="sxs-lookup"><span data-stu-id="d7bba-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="d7bba-220">Los operandos de una expresión se evalúan de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="d7bba-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="d7bba-221">Por ejemplo, en `F(i) + G(i++) * H(i)`, método `F` se denomina con el valor antiguo de `i`, método, a continuación, `G` se llama con el valor antiguo de `i`y, por último, el método `H` se denomina con el nuevo valor de `i`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="d7bba-222">Esto es independiente y no relacionados a la prioridad de operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="d7bba-223">Pueden ser determinados operadores ***sobrecargado***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="d7bba-224">Sobrecarga de operador permite implementaciones de operadores definidos por el usuario se puede especificar para las operaciones en el que uno o ambos operandos son de un tipo de clase o struct definido por el usuario ([la sobrecarga de operadores](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="d7bba-225">Prioridad y asociatividad de los operadores</span><span class="sxs-lookup"><span data-stu-id="d7bba-225">Operator precedence and associativity</span></span>

<span data-ttu-id="d7bba-226">Cuando una expresión contiene varios operadores, la ***precedencia*** de los operadores controla el orden en que se evalúan los operadores individuales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="d7bba-227">Por ejemplo, la expresión `x + y * z` se evalúa como `x + (y * z)` porque el `*` operador tiene mayor prioridad que el archivo binario `+` operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="d7bba-228">La prioridad de un operador se establece mediante la definición de su producción gramatical asociada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="d7bba-229">Por ejemplo, un *additive_expression* consta de una secuencia de *multiplicative_expression*s separados por `+` o `-` operadores, lo que da a la `+` y `-` operadores menor prioridad que el `*`, `/`, y `%` operadores.</span><span class="sxs-lookup"><span data-stu-id="d7bba-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="d7bba-230">En la tabla siguiente se resume todos los operadores en orden de prioridad de mayor a menor:</span><span class="sxs-lookup"><span data-stu-id="d7bba-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="d7bba-231">__Sección__</span><span class="sxs-lookup"><span data-stu-id="d7bba-231">__Section__</span></span>                                                                                   | <span data-ttu-id="d7bba-232">__Categoría__</span><span class="sxs-lookup"><span data-stu-id="d7bba-232">__Category__</span></span>                | <span data-ttu-id="d7bba-233">__Operadores__</span><span class="sxs-lookup"><span data-stu-id="d7bba-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="d7bba-234">Expresiones primarias</span><span class="sxs-lookup"><span data-stu-id="d7bba-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="d7bba-235">Principal</span><span class="sxs-lookup"><span data-stu-id="d7bba-235">Primary</span></span>                     | <span data-ttu-id="d7bba-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="d7bba-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="d7bba-237">Operadores unarios</span><span class="sxs-lookup"><span data-stu-id="d7bba-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="d7bba-238">Unario</span><span class="sxs-lookup"><span data-stu-id="d7bba-238">Unary</span></span>                       | <span data-ttu-id="d7bba-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="d7bba-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="d7bba-240">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="d7bba-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="d7bba-241">Multiplicativo</span><span class="sxs-lookup"><span data-stu-id="d7bba-241">Multiplicative</span></span>              | <span data-ttu-id="d7bba-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="d7bba-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="d7bba-243">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="d7bba-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="d7bba-244">Aditivo</span><span class="sxs-lookup"><span data-stu-id="d7bba-244">Additive</span></span>                    | <span data-ttu-id="d7bba-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="d7bba-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="d7bba-246">Operadores de desplazamiento</span><span class="sxs-lookup"><span data-stu-id="d7bba-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="d7bba-247">Shift</span><span class="sxs-lookup"><span data-stu-id="d7bba-247">Shift</span></span>                       | <span data-ttu-id="d7bba-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="d7bba-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="d7bba-249">Operadores de comprobación de tipos y relacionales</span><span class="sxs-lookup"><span data-stu-id="d7bba-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="d7bba-250">Comprobación de tipos y relacional</span><span class="sxs-lookup"><span data-stu-id="d7bba-250">Relational and type testing</span></span> | <span data-ttu-id="d7bba-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="d7bba-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="d7bba-252">Operadores de comprobación de tipos y relacionales</span><span class="sxs-lookup"><span data-stu-id="d7bba-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="d7bba-253">Igualdad</span><span class="sxs-lookup"><span data-stu-id="d7bba-253">Equality</span></span>                    | <span data-ttu-id="d7bba-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="d7bba-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="d7bba-255">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="d7bba-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="d7bba-256">AND lógico</span><span class="sxs-lookup"><span data-stu-id="d7bba-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="d7bba-257">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="d7bba-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="d7bba-258">XOR lógico</span><span class="sxs-lookup"><span data-stu-id="d7bba-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="d7bba-259">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="d7bba-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="d7bba-260">OR lógico</span><span class="sxs-lookup"><span data-stu-id="d7bba-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="d7bba-261">Operadores lógicos condicionales</span><span class="sxs-lookup"><span data-stu-id="d7bba-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="d7bba-262">AND condicional</span><span class="sxs-lookup"><span data-stu-id="d7bba-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="d7bba-263">Operadores lógicos condicionales</span><span class="sxs-lookup"><span data-stu-id="d7bba-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="d7bba-264">OR condicional</span><span class="sxs-lookup"><span data-stu-id="d7bba-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="d7bba-265">Operador de uso combinado de NULL</span><span class="sxs-lookup"><span data-stu-id="d7bba-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="d7bba-266">Uso combinado de NULL</span><span class="sxs-lookup"><span data-stu-id="d7bba-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="d7bba-267">Operador condicional</span><span class="sxs-lookup"><span data-stu-id="d7bba-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="d7bba-268">Condicional</span><span class="sxs-lookup"><span data-stu-id="d7bba-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="d7bba-269">[Operadores de asignación](expressions.md#assignment-operators), [expresiones de función anónima](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="d7bba-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="d7bba-270">Expresión de asignación y lambda</span><span class="sxs-lookup"><span data-stu-id="d7bba-270">Assignment and lambda expression</span></span> | <span data-ttu-id="d7bba-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="d7bba-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="d7bba-272">Cuando un operando se encuentra entre dos operadores con la misma prioridad, la asociatividad de los operadores controla el orden en que se realizan las operaciones:</span><span class="sxs-lookup"><span data-stu-id="d7bba-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="d7bba-273">Excepto los operadores de asignación y el operador de uso combinado de null, todos los operadores binarios son ***asociativos a la izquierda***, lo que significa que las operaciones se realizan de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="d7bba-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="d7bba-274">Por ejemplo, `x + y + z` se evalúa como `(x + y) + z`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="d7bba-275">Los operadores de asignación, el operador de uso combinado de null y el operador condicional (`?:`) son ***asociativos a la derecha***, lo que significa que las operaciones se realizan de derecha a izquierda.</span><span class="sxs-lookup"><span data-stu-id="d7bba-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="d7bba-276">Por ejemplo, `x = y = z` se evalúa como `x = (y = z)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="d7bba-277">La precedencia y la asociatividad pueden controlarse mediante paréntesis.</span><span class="sxs-lookup"><span data-stu-id="d7bba-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="d7bba-278">Por ejemplo, `x + y * z` primero multiplica `y` por `z` y luego suma el resultado a `x`, pero `(x + y) * z` primero suma `x` y `y` y luego multiplica el resultado por `z`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="d7bba-279">Sobrecarga de operadores</span><span class="sxs-lookup"><span data-stu-id="d7bba-279">Operator overloading</span></span>

<span data-ttu-id="d7bba-280">Todos los operadores unarios y binarios tienen implementaciones predefinidas que están disponibles automáticamente en cualquier expresión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="d7bba-281">Además de las implementaciones predefinidas, pueden introducirse implementaciones definidas por el usuario mediante la inclusión de `operator` las declaraciones de clases y structs ([operadores](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="d7bba-282">Las implementaciones de operador definido por el usuario siempre tienen prioridad sobre las implementaciones de operador predefinidas: Solo cuando no existen implementaciones de ningún operador definido por el usuario correspondiente se consideran las implementaciones de operador predefinido, como se describe en [de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution) y [sobrecarga de operador binario resolución](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d7bba-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="d7bba-283">El ***operadores unarios sobrecargables*** son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="d7bba-284">Aunque `true` y `false` explícitamente no se utilizan en expresiones (y, por tanto, no se incluyen en la tabla de prioridad de [prioridad y asociatividad](expressions.md#operator-precedence-and-associativity)), se consideran operadores porque son invoca en varios contextos de expresión: expresiones booleanas ([expresiones booleanas](expressions.md#boolean-expressions)) y expresiones que implican el condicional ([operador condicional](expressions.md#conditional-operator)) y la lógica condicional operadores ([operadores lógicos condicionales](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="d7bba-285">El ***puede sobrecargables operadores binarios*** son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="d7bba-286">Solo los operadores enumerados anteriormente se pueden sobrecargar.</span><span class="sxs-lookup"><span data-stu-id="d7bba-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="d7bba-287">En concreto, no es posible sobrecargar el acceso de miembro, la invocación de método, o la `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, y `is` operadores.</span><span class="sxs-lookup"><span data-stu-id="d7bba-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="d7bba-288">Cuando se sobrecarga un operador binario, el operador de asignación correspondiente, si lo hay, también se sobrecarga de modo implícito.</span><span class="sxs-lookup"><span data-stu-id="d7bba-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="d7bba-289">Por ejemplo, una sobrecarga del operador `*` también es una sobrecarga del operador `*=`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="d7bba-290">Esto se describe más detalladamente en [asignación compuesta](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="d7bba-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="d7bba-291">Tenga en cuenta que el propio operador de asignación (`=`) no se puede sobrecargar.</span><span class="sxs-lookup"><span data-stu-id="d7bba-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="d7bba-292">Una asignación siempre realiza una simple copia bit a bit de un valor en una variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="d7bba-293">Convertir las operaciones, tales como `(T)x`, están sobrecargados, ya que proporciona las conversiones definidas por el usuario ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="d7bba-294">Acceso de elemento, como `a[x]`, no se considera un operador sobrecargable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="d7bba-295">En su lugar, se admite la indización definido por el usuario a través de los indizadores ([indizadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="d7bba-296">En las expresiones, se hace referencia a operadores mediante la notación de operador y, en las declaraciones, los operadores se hace referencia mediante la notación funcional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="d7bba-297">En la tabla siguiente muestra la relación entre las notaciones funcionales para los operadores unarios y binarios y de operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="d7bba-298">En la primera entrada, *op* denota cualquier operador unario sobrecargable de prefijo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="d7bba-299">En la segunda entrada, *op* denota el postfijo unario `++` y `--` operadores.</span><span class="sxs-lookup"><span data-stu-id="d7bba-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="d7bba-300">En la tercera entrada, *op* denota cualquier operador binario sobrecargable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="d7bba-301">__Notación de operador__</span><span class="sxs-lookup"><span data-stu-id="d7bba-301">__Operator notation__</span></span> | <span data-ttu-id="d7bba-302">__Notación funcional__</span><span class="sxs-lookup"><span data-stu-id="d7bba-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="d7bba-303">Las declaraciones de operador definido por el usuario siempre requieren al menos uno de los parámetros para ser del tipo de clase o estructura que contiene la declaración del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="d7bba-304">Por lo tanto, no es posible que un operador definido por el usuario tener la misma firma que un operador predefinido.</span><span class="sxs-lookup"><span data-stu-id="d7bba-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="d7bba-305">Las declaraciones de operador definido por el usuario no pueden modificar la sintaxis, prioridad o asociatividad de un operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="d7bba-306">Por ejemplo, el `/` operador siempre es un operador binario, siempre se el nivel de prioridad especificado en [prioridad y asociatividad](expressions.md#operator-precedence-and-associativity)y siempre es asociativo a la izquierda.</span><span class="sxs-lookup"><span data-stu-id="d7bba-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="d7bba-307">Aunque es posible que un operador definido por el usuario realizar cualquier cálculo que le interese, se desaconseja las implementaciones que generan resultados distintos a los que se esperan de manera intuitiva.</span><span class="sxs-lookup"><span data-stu-id="d7bba-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="d7bba-308">Por ejemplo, una implementación de `operator ==` debe comparar la igualdad de los dos operandos y devolver un adecuado `bool` resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="d7bba-309">Las descripciones de los operadores individuales en [expresiones primarias](expressions.md#primary-expressions) a través de [operadores lógicos condicionales](expressions.md#conditional-logical-operators) especifican las implementaciones de los operadores y las reglas adicionales que se aplican predefinidas para cada operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="d7bba-310">Las descripciones de realizar el uso de los términos ***de resolución de sobrecarga de operador unario***, ***de resolución de sobrecarga de operador binario***, y ***promoción numérica***, definiciones de las cuales se se encuentra en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="d7bba-311">Resolución de sobrecarga de operador unario</span><span class="sxs-lookup"><span data-stu-id="d7bba-311">Unary operator overload resolution</span></span>

<span data-ttu-id="d7bba-312">Una operación del formulario `op x` o `x op`, donde `op` es un operador unario sobrecargable, y `x` es una expresión de tipo `X`, se procesa como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="d7bba-313">El conjunto de operadores definidos por el usuario candidatos suministrados por `X` para la operación `operator op(x)` se determina mediante las reglas de [operadores definidos por el usuario de candidatos](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="d7bba-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="d7bba-314">Si el conjunto de operadores candidatos definidos por el usuario no está vacío, se convierte en el conjunto de operadores candidatos para la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="d7bba-315">En caso contrario, el operador unario predefinido `operator op` implementaciones, incluidas sus formatos de elevación, se convierten en el conjunto de operadores candidatos para la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="d7bba-316">Las implementaciones predefinidas de un operador determinado se especifican en la descripción del operador ([expresiones primarias](expressions.md#primary-expressions) y [operadores unarios](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="d7bba-317">Las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution) se aplican al conjunto de operadores candidatos para seleccionar el mejor operador con respecto a la lista de argumentos `(x)`, y este operador es el resultado de la sobrecarga proceso de resolución.</span><span class="sxs-lookup"><span data-stu-id="d7bba-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="d7bba-318">Si se produce un error en la resolución de sobrecarga seleccionar un operador único idóneo, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="d7bba-319">Resolución de sobrecarga de operador binario</span><span class="sxs-lookup"><span data-stu-id="d7bba-319">Binary operator overload resolution</span></span>

<span data-ttu-id="d7bba-320">Una operación del formulario `x op y`, donde `op` es un operador binario sobrecargable, `x` es una expresión de tipo `X`, y `y` es una expresión de tipo `Y`, se procesa como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="d7bba-321">El conjunto de operadores definidos por el usuario candidatos suministrados por `X` y `Y` para la operación `operator op(x,y)` viene determinada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="d7bba-322">El conjunto consta de la unión de los operadores candidatos suministrados por `X` y los operadores candidatos suministrados por `Y`, cada uno de ellos determinado utilizando las reglas de [operadores definidos por el usuario de candidatos](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="d7bba-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="d7bba-323">Si `X` y `Y` son del mismo tipo, o si `X` y `Y` se derivan de un tipo base común, los operadores candidatos compartidos solo se producen en el conjunto combinado una vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="d7bba-324">Si el conjunto de operadores candidatos definidos por el usuario no está vacío, se convierte en el conjunto de operadores candidatos para la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="d7bba-325">En caso contrario, el archivo binario predefinido `operator op` implementaciones, incluidas sus formatos de elevación, se convierten en el conjunto de operadores candidatos para la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="d7bba-326">Las implementaciones predefinidas de un operador determinado se especifican en la descripción del operador ([operadores aritméticos](expressions.md#arithmetic-operators) a través de [operadores lógicos condicionales](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="d7bba-327">Para los operadores predefinidos de enumeración y delegado, los operadores solo considerados son los definidos por un tipo de enumeración o delegado que es el tipo en tiempo de enlace de uno de los operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="d7bba-328">Las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution) se aplican al conjunto de operadores candidatos para seleccionar el mejor operador con respecto a la lista de argumentos `(x,y)`, y este operador es el resultado de la sobrecarga proceso de resolución.</span><span class="sxs-lookup"><span data-stu-id="d7bba-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="d7bba-329">Si se produce un error en la resolución de sobrecarga seleccionar un operador único idóneo, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="d7bba-330">Operadores definidos por el usuario de candidatos</span><span class="sxs-lookup"><span data-stu-id="d7bba-330">Candidate user-defined operators</span></span>

<span data-ttu-id="d7bba-331">Dado un tipo `T` y una operación `operator op(A)`, donde `op` es un operador sobrecargable y `A` es una lista de argumentos, el conjunto de candidatos a operadores definidos por el usuario proporcionados por `T` para `operator op(A)` viene la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="d7bba-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="d7bba-332">Determinar el tipo `T0`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-332">Determine the type `T0`.</span></span> <span data-ttu-id="d7bba-333">Si `T` es un tipo que acepta valores NULL, `T0` es su tipo subyacente, de lo contrario, `T0` es igual a `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="d7bba-334">Para todos los `operator op` declaraciones en `T0` y todos los formatos de elevación de operadores de este tipo, si es aplicable al menos un operador ([miembro de función aplicable](expressions.md#applicable-function-member)) con respecto a la lista de argumentos `A`, a continuación, el conjunto de operadores candidatos consta de todos estos operadores aplicable en `T0`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="d7bba-335">De lo contrario, si `T0` es `object`, el conjunto de operadores candidatos está vacío.</span><span class="sxs-lookup"><span data-stu-id="d7bba-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="d7bba-336">En caso contrario, proporciona el conjunto de operadores candidatos `T0` es el conjunto de operadores candidatos suministrado por la clase base directa de `T0`, o la clase base efectiva de `T0` si `T0` es un parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="d7bba-337">Promociones numéricas</span><span class="sxs-lookup"><span data-stu-id="d7bba-337">Numeric promotions</span></span>

<span data-ttu-id="d7bba-338">La promoción numérica consiste en llevar automáticamente determinadas conversiones implícitas de los operandos de los predefinidos unario y binarios operadores numéricos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="d7bba-339">Promoción numérica no es un mecanismo distinto, sino más bien un efecto de aplicar la resolución de sobrecarga a los operadores predefinidos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="d7bba-340">Promoción numérica específicamente no afectan a la evaluación de operadores definidos por el usuario, aunque los operadores definidos por el usuario se pueden implementar para que presenten efectos similares.</span><span class="sxs-lookup"><span data-stu-id="d7bba-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="d7bba-341">Como ejemplo de promoción numérica, considere la posibilidad de las implementaciones predefinidas del binario `*` operador:</span><span class="sxs-lookup"><span data-stu-id="d7bba-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="d7bba-342">Cuando las reglas de resolución de sobrecarga ([de resolución de sobrecarga](expressions.md#overload-resolution)) se aplican a este conjunto de operadores, el efecto es seleccionar el primero de los operadores para las que existen las conversiones implícitas de los tipos de operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="d7bba-343">Por ejemplo, para la operación `b * s`, donde `b` es un `byte` y `s` es un `short`, selecciona la resolución de sobrecarga `operator *(int,int)` como el mejor operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="d7bba-344">Por lo tanto, el efecto es que `b` y `s` se convierten en `int`, y el tipo del resultado es `int`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="d7bba-345">Del mismo modo, para la operación `i * d`, donde `i` es un `int` y `d` es un `double`, selecciona la resolución de sobrecarga `operator *(double,double)` como el mejor operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="d7bba-346">Promociones numéricas unarias</span><span class="sxs-lookup"><span data-stu-id="d7bba-346">Unary numeric promotions</span></span>

<span data-ttu-id="d7bba-347">Promoción numérica unaria se produce para los operandos de predefinido `+`, `-`, y `~` operadores unarios.</span><span class="sxs-lookup"><span data-stu-id="d7bba-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="d7bba-348">Promoción numérica unaria simplemente consiste en convertir los operandos de tipo `sbyte`, `byte`, `short`, `ushort`, o `char` escriba `int`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="d7bba-349">Además, para el operador unario `-` operador, la promoción numérica unaria convierte operandos de tipo `uint` escriba `long`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="d7bba-350">Promociones numéricas binarias</span><span class="sxs-lookup"><span data-stu-id="d7bba-350">Binary numeric promotions</span></span>

<span data-ttu-id="d7bba-351">Se produce una promoción numérica binaria para los operandos de predefinido `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, y `<=` operadores binarios.</span><span class="sxs-lookup"><span data-stu-id="d7bba-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="d7bba-352">Promoción numérica binaria convierte implícitamente ambos operandos a un tipo común que, en el caso de los operadores no relacionales, también se convierte en el tipo de resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="d7bba-353">Promoción numérica binaria consta de aplicar las reglas siguientes, en el orden que aparecen a continuación:</span><span class="sxs-lookup"><span data-stu-id="d7bba-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="d7bba-354">Si alguno de los operandos es de tipo `decimal`, el otro operando se convierte al tipo `decimal`, o se produce un error en tiempo de enlace si el otro operando es de tipo `float` o `double`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="d7bba-355">En caso contrario, si alguno de los operandos es de tipo `double`, el otro operando se convierte al tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="d7bba-356">En caso contrario, si alguno de los operandos es de tipo `float`, el otro operando se convierte al tipo `float`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="d7bba-357">En caso contrario, si alguno de los operandos es de tipo `ulong`, el otro operando se convierte al tipo `ulong`, o se produce un error en tiempo de enlace si el otro operando es de tipo `sbyte`, `short`, `int`, o `long`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="d7bba-358">En caso contrario, si alguno de los operandos es de tipo `long`, el otro operando se convierte al tipo `long`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="d7bba-359">En caso contrario, si alguno de los operandos es de tipo `uint` y el otro operando es de tipo `sbyte`, `short`, o `int`, ambos operandos se convierten al tipo `long`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="d7bba-360">En caso contrario, si alguno de los operandos es de tipo `uint`, el otro operando se convierte al tipo `uint`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="d7bba-361">En caso contrario, ambos operandos se convierten al tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="d7bba-362">Tenga en cuenta que la primera regla no permite las operaciones que combinan la `decimal` tipo con el `double` y `float` tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="d7bba-363">Sigue la regla del hecho de que no hay ninguna conversión implícita entre el `decimal` tipo y el `double` y `float` tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="d7bba-364">Tenga en cuenta también que no es posible que un operando de tipo `ulong` cuando el otro operando es de un tipo entero con signo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="d7bba-365">El motivo es que existe ningún tipo de entero que puede representar el intervalo completo de `ulong` , así como los tipos enteros con signo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="d7bba-366">En ambos de los casos anteriores, se puede usar una expresión de conversión para convertir explícitamente un operando a un tipo que es compatible con el otro operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="d7bba-367">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="d7bba-368">se produce un error en tiempo de enlace porque un `decimal` no se puede multiplicar por un `double`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="d7bba-369">El error se resuelve mediante la conversión explícita del segundo operando `decimal`, como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="d7bba-370">Operadores de elevación</span><span class="sxs-lookup"><span data-stu-id="d7bba-370">Lifted operators</span></span>

<span data-ttu-id="d7bba-371">***Operadores de elevación*** permitir que los operadores predefinidos y definidos por el usuario que funcionan en tipos de valor distinto de NULL para utilizarse también con los formularios de esos tipos que aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="d7bba-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="d7bba-372">Operadores de elevación se construyen a partir de los operadores predefinidos y definidos por el usuario que cumplen ciertos requisitos, como se describe en la siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7bba-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="d7bba-373">Para los operadores unarios</span><span class="sxs-lookup"><span data-stu-id="d7bba-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="d7bba-374">un formato de elevación de un operador existe si los tipos de operando y el resultado son ambos tipos de valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="d7bba-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="d7bba-375">El formato de elevación se construye mediante la adición de una sola `?` modificador a los tipos de operando y el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="d7bba-376">El operador de elevación genera un valor null si el operando es null.</span><span class="sxs-lookup"><span data-stu-id="d7bba-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="d7bba-377">En caso contrario, el operador de elevación desajusta el operando, aplica el operador subyacente y ajusta el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="d7bba-378">Para los operadores binarios</span><span class="sxs-lookup"><span data-stu-id="d7bba-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="d7bba-379">un formato de elevación de un operador existe si los tipos de operando y el resultado son todos los tipos de valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="d7bba-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="d7bba-380">El formato de elevación se construye mediante la adición de una sola `?` modificador a cada tipo de operando y el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="d7bba-381">El operador de elevación genera un valor null si uno o ambos operandos son nulos (una excepción que se va a la `&` y `|` operadores de la `bool?` type, tal como se describe en [operadores lógicos Boolean](expressions.md#boolean-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="d7bba-382">En caso contrario, el operador de elevación desajusta los operandos, aplica el operador subyacente y ajusta el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="d7bba-383">Para los operadores de igualdad</span><span class="sxs-lookup"><span data-stu-id="d7bba-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="d7bba-384">existe un formulario de elevación de un operador si los tipos de operando son tipos de valor que no aceptan valores NULL y si el tipo de resultado es `bool`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="d7bba-385">El formato de elevación se construye mediante la adición de una sola `?` modificador a cada tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="d7bba-386">El operador de elevación considera que los dos valores null son iguales y un valor null no es igual a cualquier valor distinto de null.</span><span class="sxs-lookup"><span data-stu-id="d7bba-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="d7bba-387">Si ambos operandos son distintos de null, el operador de elevación desajusta los operandos y aplica el operador subyacente para generar el `bool` resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="d7bba-388">Para los operadores relacionales</span><span class="sxs-lookup"><span data-stu-id="d7bba-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="d7bba-389">existe un formulario de elevación de un operador si los tipos de operando son tipos de valor que no aceptan valores NULL y si el tipo de resultado es `bool`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="d7bba-390">El formato de elevación se construye mediante la adición de una sola `?` modificador a cada tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="d7bba-391">El operador de elevación genera el valor `false` si uno o ambos operandos son nulos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="d7bba-392">En caso contrario, el operador de elevación desajusta los operandos y aplica el operador subyacente para generar el `bool` resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="d7bba-393">Búsqueda de miembros</span><span class="sxs-lookup"><span data-stu-id="d7bba-393">Member lookup</span></span>

<span data-ttu-id="d7bba-394">Una búsqueda de miembros es el proceso mediante el cual se determina el significado de un nombre en el contexto de un tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="d7bba-395">Puede producirse una búsqueda de miembros como parte de la evaluación de un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)) en un expresión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="d7bba-396">Si el *simple_name* o *member_access* se produce como el *primary_expression* de un *invocation_expression* ([ Las invocaciones de método](expressions.md#method-invocations)), se dice que se invoca el miembro.</span><span class="sxs-lookup"><span data-stu-id="d7bba-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="d7bba-397">Si un miembro es un método o evento, o si es una constante, campo o propiedad de un tipo de delegado ([delegados](delegates.md)) o el tipo `dynamic` ([el tipo dinámico](types.md#the-dynamic-type)), a continuación, se dice que el miembro es *invocable*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="d7bba-398">Búsqueda de miembros se considera que no sólo el nombre de un miembro, pero también el número de parámetros de tipo que el miembro tiene y si el miembro es accesible.</span><span class="sxs-lookup"><span data-stu-id="d7bba-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="d7bba-399">Para los fines de búsqueda de miembros, los métodos genéricos y tipos genéricos anidados tienen el número de parámetros de tipo indicado en sus respectivas declaraciones y todos los demás miembros tienen cero parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="d7bba-400">Una búsqueda de miembros de un nombre `N` con `K`  parámetros de tipo en un tipo `T` se procesa como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="d7bba-401">Primero, un conjunto de miembros accesibles denominado `N` viene determinada:</span><span class="sxs-lookup"><span data-stu-id="d7bba-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="d7bba-402">Si `T` es un parámetro de tipo, el conjunto es la unión de los conjuntos de miembros accesibles denominado `N` en cada uno de los tipos especificados como una restricción principal o secundaria restricción ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) para  `T`, junto con el conjunto de miembros accesibles denominado `N` en `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="d7bba-403">En caso contrario, el conjunto formado disponible ([acceso a miembros](basic-concepts.md#member-access)) miembros denominados `N` en `T`, incluidos los miembros heredados y los miembros accesibles denominados `N` en `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="d7bba-404">Si `T` es un tipo construido, el conjunto de miembros se obtiene mediante la sustitución de argumentos de tipo como se describe en [los miembros de tipos construidos](classes.md#members-of-constructed-types).</span><span class="sxs-lookup"><span data-stu-id="d7bba-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="d7bba-405">Los miembros que incluyen un `override` modificador se excluyen del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="d7bba-406">A continuación, si `K` es cero, se quitan los tipos cuyas declaraciones incluyen parámetros de tipo anidado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="d7bba-407">Si `K` no es cero, todos los miembros con un número diferente de se quitan los parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="d7bba-408">Tenga en cuenta que, cuando `K` es cero, los métodos que tienen el tipo no se quitan los parámetros desde el proceso de inferencia de tipo ([inferencia](expressions.md#type-inference)) podría ser capaz de inferir los argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="d7bba-409">A continuación, si el miembro es *invoca*, todos los no-*invocable* se quitan los miembros del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="d7bba-410">A continuación, se quitan los miembros que están ocultos por otros miembros del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="d7bba-411">Para cada miembro `S.M` en el conjunto, donde `S` es el tipo en el que el miembro `M` se declara, se aplican las reglas siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="d7bba-412">Si `M` es una constante, campo, propiedad, evento o miembro de enumeración, entonces todos los miembros declaran en un tipo base de `S` se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="d7bba-413">Si `M` es una declaración de tipo y, a continuación, todos los que no son tipos declarados en un tipo base del `S` se quitan del conjunto, y todas las declaraciones con el mismo número de parámetros de tipo como de tipo `M` declarados en un tipo base del `S` se quitan desde el conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="d7bba-414">Si `M` es un método, entonces todos los miembros que no son métodos declaran en un tipo base de `S` se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="d7bba-415">A continuación, se quitan los miembros de interfaz que están ocultos por miembros de clase del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="d7bba-416">Este paso sólo tiene efecto si `T` es un parámetro de tipo y `T` tiene una clase base eficaz distinto `object` y un conjunto de interfaces efectivas vacío ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="d7bba-417">Para cada miembro `S.M` en el conjunto, donde `S` es el tipo en el que el miembro `M` se declara las siguientes reglas se aplican si `S` es una declaración de clase que no sean `object`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="d7bba-418">Si `M` es constante, campo, propiedad, evento, miembro de enumeración o declaración de tipos, a continuación, se quitan todos los miembros declarados en una declaración de interfaz del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="d7bba-419">Si `M` es un método, a continuación, se quitan todos los miembros no son métodos declarados en una declaración de interfaz del conjunto y todos los métodos con la misma firma que `M` declarado en una interfaz de declaración se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="d7bba-420">Por último, quitados los miembros ocultos, se determina el resultado de la búsqueda:</span><span class="sxs-lookup"><span data-stu-id="d7bba-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="d7bba-421">Si el conjunto consta de un único miembro que no es un método, este miembro es el resultado de la búsqueda.</span><span class="sxs-lookup"><span data-stu-id="d7bba-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="d7bba-422">En caso contrario, si el conjunto contiene solo los métodos, este grupo de métodos es el resultado de la búsqueda.</span><span class="sxs-lookup"><span data-stu-id="d7bba-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="d7bba-423">En caso contrario, la búsqueda es ambigua y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-424">Para búsquedas de miembros en tipos distintos de los parámetros de tipo y las interfaces y búsquedas de miembros en las interfaces que son estrictamente de herencia simple (cada interfaz en la cadena de herencia tiene exactamente cero o una interfaz base directa), el efecto de las reglas de búsqueda es solo tiene que derivar miembros ocultar miembros base con el mismo nombre o signatura.</span><span class="sxs-lookup"><span data-stu-id="d7bba-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="d7bba-425">Este tipo de búsquedas de herencia simple nunca es ambigua.</span><span class="sxs-lookup"><span data-stu-id="d7bba-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="d7bba-426">Las ambigüedades que pueden surgir de búsquedas de miembros en interfaces de herencia múltiple se describen en [acceso a miembros de la interfaz](interfaces.md#interface-member-access).</span><span class="sxs-lookup"><span data-stu-id="d7bba-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="d7bba-427">Tipos base</span><span class="sxs-lookup"><span data-stu-id="d7bba-427">Base types</span></span>

<span data-ttu-id="d7bba-428">A efectos de la búsqueda de miembros de un tipo `T` se considera que tiene los siguientes tipos base:</span><span class="sxs-lookup"><span data-stu-id="d7bba-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="d7bba-429">Si `T` es `object`, a continuación, `T` no tiene ningún tipo base.</span><span class="sxs-lookup"><span data-stu-id="d7bba-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="d7bba-430">Si `T` es un *enum_type*, los tipos base del `T` son los tipos de clase `System.Enum`, `System.ValueType`, y `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="d7bba-431">Si `T` es un *struct_type*, los tipos base del `T` son los tipos de clase `System.ValueType` y `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="d7bba-432">Si `T` es un *class_type*, los tipos base del `T` son las clases base de `T`, incluido el tipo de clase `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="d7bba-433">Si `T` es un *interface_type*, los tipos base del `T` son las interfaces base de `T` y el tipo de clase `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="d7bba-434">Si `T` es un *array_type*, los tipos base del `T` son los tipos de clase `System.Array` y `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="d7bba-435">Si `T` es un *delegate_type*, los tipos base del `T` son los tipos de clase `System.Delegate` y `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="d7bba-436">Miembros de función</span><span class="sxs-lookup"><span data-stu-id="d7bba-436">Function members</span></span>

<span data-ttu-id="d7bba-437">Los miembros de función son los miembros que contienen instrucciones ejecutables.</span><span class="sxs-lookup"><span data-stu-id="d7bba-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="d7bba-438">Miembros de función siempre son miembros de tipos y no pueden ser miembros de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="d7bba-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="d7bba-439">C# define las siguientes categorías de miembros de función:</span><span class="sxs-lookup"><span data-stu-id="d7bba-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="d7bba-440">Métodos</span><span class="sxs-lookup"><span data-stu-id="d7bba-440">Methods</span></span>
*  <span data-ttu-id="d7bba-441">Propiedades</span><span class="sxs-lookup"><span data-stu-id="d7bba-441">Properties</span></span>
*  <span data-ttu-id="d7bba-442">Eventos</span><span class="sxs-lookup"><span data-stu-id="d7bba-442">Events</span></span>
*  <span data-ttu-id="d7bba-443">Indizadores</span><span class="sxs-lookup"><span data-stu-id="d7bba-443">Indexers</span></span>
*  <span data-ttu-id="d7bba-444">Operadores definidos por el usuario</span><span class="sxs-lookup"><span data-stu-id="d7bba-444">User-defined operators</span></span>
*  <span data-ttu-id="d7bba-445">Constructores de instancias</span><span class="sxs-lookup"><span data-stu-id="d7bba-445">Instance constructors</span></span>
*  <span data-ttu-id="d7bba-446">Constructores estáticos</span><span class="sxs-lookup"><span data-stu-id="d7bba-446">Static constructors</span></span>
*  <span data-ttu-id="d7bba-447">Destructores</span><span class="sxs-lookup"><span data-stu-id="d7bba-447">Destructors</span></span>

<span data-ttu-id="d7bba-448">Excepto para los destructores y los constructores estáticos (que no se puede invocar explícitamente), las instrucciones incluidas en los miembros de función se ejecutan a través de las llamadas a funciones miembro.</span><span class="sxs-lookup"><span data-stu-id="d7bba-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="d7bba-449">La sintaxis para escribir una invocación de miembros de función depende de la categoría de miembro de función determinada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="d7bba-450">La lista de argumentos ([listas de argumentos](expressions.md#argument-lists)) de un miembro de función invocación proporciona los valores reales o referencias de variables para los parámetros del miembro de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="d7bba-451">Las invocaciones de métodos genéricos pueden emplear la inferencia de tipos para determinar el conjunto de argumentos de tipo para pasar al método.</span><span class="sxs-lookup"><span data-stu-id="d7bba-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="d7bba-452">Este proceso se describe en [inferencia de tipo](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="d7bba-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="d7bba-453">Las llamadas de métodos, indizadores, operadores y constructores de instancias utilizan la resolución de sobrecarga para determinar qué parte de un conjunto candidato de miembros de función para invocar.</span><span class="sxs-lookup"><span data-stu-id="d7bba-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="d7bba-454">Este proceso se describe en [de resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d7bba-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="d7bba-455">Una vez que se ha identificado un miembro de función determinada en tiempo de enlace, posiblemente a través de la resolución de sobrecarga, el proceso de tiempo de ejecución real de invocar el miembro de función se describe en [comprobación de la resolución de sobrecarga dinámicadetiempodecompilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d7bba-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="d7bba-456">En la tabla siguiente se resume el procesamiento que tiene lugar en las construcciones que implican las seis categorías de miembros de función que se pueden invocar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="d7bba-457">En la tabla, `e`, `x`, `y`, y `value` indican expresiones clasificadas como variables o valores, `T` indica una expresión que se clasifica como un tipo, `F` es el nombre sencillo de un método y `P` es el nombre sencillo de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="d7bba-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="d7bba-458">__Construcción__</span><span class="sxs-lookup"><span data-stu-id="d7bba-458">__Construct__</span></span>     | <span data-ttu-id="d7bba-459">__Ejemplo__</span><span class="sxs-lookup"><span data-stu-id="d7bba-459">__Example__</span></span>    | <span data-ttu-id="d7bba-460">__Descripción__</span><span class="sxs-lookup"><span data-stu-id="d7bba-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="d7bba-461">Invocación del método.</span><span class="sxs-lookup"><span data-stu-id="d7bba-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="d7bba-462">Se aplica la resolución de sobrecarga para seleccionar el mejor método `F` en la clase o estructura contenedora.</span><span class="sxs-lookup"><span data-stu-id="d7bba-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="d7bba-463">El método se invoca con la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="d7bba-464">Si el método no es `static`, la expresión de instancia es `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="d7bba-465">Se aplica la resolución de sobrecarga para seleccionar el mejor método `F` en la clase o struct `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="d7bba-466">Se produce un error en tiempo de enlace si el método no es `static`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="d7bba-467">El método se invoca con la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="d7bba-468">Se aplica la resolución de sobrecarga para seleccionar el mejor método F en la clase, estructura o interfaz dada por el tipo de `e`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="d7bba-469">Se produce un error en tiempo de enlace si el método es `static`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="d7bba-470">El método se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="d7bba-471">Property Access</span><span class="sxs-lookup"><span data-stu-id="d7bba-471">Property access</span></span>   | `P`            | <span data-ttu-id="d7bba-472">El `get` descriptor de acceso de la propiedad `P` se invoca en la clase o estructura contenedora.</span><span class="sxs-lookup"><span data-stu-id="d7bba-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="d7bba-473">Se produce un error de tiempo de compilación si `P` es de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="d7bba-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="d7bba-474">Si `P` no `static`, la expresión de instancia es `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="d7bba-475">El `set` descriptor de acceso de la propiedad `P` en la clase o estructura contenedora se invoca con la lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="d7bba-476">Se produce un error de tiempo de compilación si `P` es de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="d7bba-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="d7bba-477">Si `P` no `static`, la expresión de instancia es `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="d7bba-478">El `get` descriptor de acceso de la propiedad `P` en la clase o struct `T` se invoca.</span><span class="sxs-lookup"><span data-stu-id="d7bba-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="d7bba-479">Se produce un error de tiempo de compilación si `P` no `static` o si `P` es de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="d7bba-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="d7bba-480">El `set` descriptor de acceso de la propiedad `P` en la clase o struct `T` se invoca con la lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="d7bba-481">Se produce un error de tiempo de compilación si `P` no `static` o si `P` es de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="d7bba-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="d7bba-482">El `get` descriptor de acceso de la propiedad `P` en la clase, estructura o interfaz dada por el tipo de `e` se invoca con la expresión de instancia `e`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="d7bba-483">Si se produce un error de enlace `P` es `static` o si `P` es de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="d7bba-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="d7bba-484">El `set` descriptor de acceso de la propiedad `P` en la clase, estructura o interfaz dada por el tipo de `e` se invoca con la expresión de instancia `e` y la lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="d7bba-485">Si se produce un error de enlace `P` es `static` o si `P` es de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="d7bba-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="d7bba-486">Acceso a eventos</span><span class="sxs-lookup"><span data-stu-id="d7bba-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="d7bba-487">El `add` descriptor de acceso del evento `E` se invoca en la clase o estructura contenedora.</span><span class="sxs-lookup"><span data-stu-id="d7bba-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="d7bba-488">Si `E` es no estático, la expresión de instancia es `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="d7bba-489">El `remove` descriptor de acceso del evento `E` se invoca en la clase o estructura contenedora.</span><span class="sxs-lookup"><span data-stu-id="d7bba-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="d7bba-490">Si `E` es no estático, la expresión de instancia es `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="d7bba-491">El `add` descriptor de acceso del evento `E` en la clase o struct `T` se invoca.</span><span class="sxs-lookup"><span data-stu-id="d7bba-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="d7bba-492">Se produce un error en tiempo de enlace si `E` no es estático.</span><span class="sxs-lookup"><span data-stu-id="d7bba-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="d7bba-493">El `remove` descriptor de acceso del evento `E` en la clase o struct `T` se invoca.</span><span class="sxs-lookup"><span data-stu-id="d7bba-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="d7bba-494">Se produce un error en tiempo de enlace si `E` no es estático.</span><span class="sxs-lookup"><span data-stu-id="d7bba-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="d7bba-495">El `add` descriptor de acceso del evento `E` en la clase, estructura o interfaz dada por el tipo de `e` se invoca con la expresión de instancia `e`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="d7bba-496">Se produce un error en tiempo de enlace si `E` es estático.</span><span class="sxs-lookup"><span data-stu-id="d7bba-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="d7bba-497">El `remove` descriptor de acceso del evento `E` en la clase, estructura o interfaz dada por el tipo de `e` se invoca con la expresión de instancia `e`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="d7bba-498">Se produce un error en tiempo de enlace si `E` es estático.</span><span class="sxs-lookup"><span data-stu-id="d7bba-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="d7bba-499">Acceso al indizador</span><span class="sxs-lookup"><span data-stu-id="d7bba-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="d7bba-500">Se aplica la resolución de sobrecarga para seleccionar el mejor indizador de la clase, estructura o interfaz dada por el tipo de e.</span><span class="sxs-lookup"><span data-stu-id="d7bba-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="d7bba-501">El `get` descriptor de acceso del indizador se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="d7bba-502">Si el indizador es de solo escritura, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="d7bba-503">Se aplica la resolución de sobrecarga para seleccionar el mejor indizador de la clase, estructura o interfaz dada por el tipo de `e`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="d7bba-504">El `set` descriptor de acceso del indizador se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y,value)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="d7bba-505">Si el indizador es de solo lectura, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="d7bba-506">Invocación de operador</span><span class="sxs-lookup"><span data-stu-id="d7bba-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="d7bba-507">Se aplica la resolución de sobrecarga para seleccionar el operador unario mejor en la clase o struct dada por el tipo de `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="d7bba-508">Se invoca el operador seleccionado con la lista de argumentos `(x)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="d7bba-509">Se aplica la resolución de sobrecarga para seleccionar el mejor operador binario de las clases o structs proporcionados por los tipos de `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="d7bba-510">Se invoca el operador seleccionado con la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="d7bba-511">Invocación del constructor de instancia</span><span class="sxs-lookup"><span data-stu-id="d7bba-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="d7bba-512">Se aplica la resolución de sobrecarga para seleccionar el mejor constructor de instancia de la clase o estructura `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="d7bba-513">Se invoca el constructor de instancia con la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="d7bba-514">Listas de argumentos</span><span class="sxs-lookup"><span data-stu-id="d7bba-514">Argument lists</span></span>

<span data-ttu-id="d7bba-515">Cada invocación de miembros y el delegado de función incluye una lista de argumentos que proporciona los valores reales o referencias de variables para los parámetros del miembro de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="d7bba-516">La sintaxis para especificar la lista de argumentos de una invocación de miembros de función depende de la categoría de miembro de función:</span><span class="sxs-lookup"><span data-stu-id="d7bba-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="d7bba-517">Por ejemplo los constructores, métodos, indizadores y delegados, los argumentos se especifican como un *argument_list*, tal y como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="d7bba-518">Para los indizadores, al invocar el `set` descriptor de acceso, la lista de argumentos incluye además la expresión especificada como el operando derecho del operador de asignación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="d7bba-519">Para las propiedades, la lista de argumentos está vacía cuando se invoca el `get` descriptor de acceso y consta de la expresión especificada como el operando derecho del operador de asignación al invocar el `set` descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="d7bba-520">Para los eventos, la lista de argumentos consta de la expresión especificada como el operando derecho de la `+=` o `-=` operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="d7bba-521">Para los operadores definidos por el usuario, la lista de argumentos está formado por el único operando del operador unario o los dos operandos del operador binario.</span><span class="sxs-lookup"><span data-stu-id="d7bba-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="d7bba-522">Los argumentos de propiedades ([propiedades](classes.md#properties)), los eventos ([eventos](classes.md#events)) y los operadores definidos por el usuario ([operadores](classes.md#operators)) siempre se pasan como parámetros de valor ([ Parámetros del valor](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="d7bba-523">Los argumentos de indizadores ([indizadores](classes.md#indexers)) siempre se pasan como parámetros de valor ([parámetros del valor](classes.md#value-parameters)) o matrices de parámetros ([las matrices de parámetros](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="d7bba-524">No se admiten parámetros de referencia y de salida para estas categorías de miembros de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="d7bba-525">Los argumentos de una invocación de constructor, método, indizador o delegado de la instancia se especifican como un *argument_list*:</span><span class="sxs-lookup"><span data-stu-id="d7bba-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

<span data-ttu-id="d7bba-526">Un *argument_list* consta de uno o varios *argumento*s, separados por comas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="d7bba-527">Cada argumento consta de un elemento opcional *argument_name* seguido por un *argument_value*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="d7bba-528">Un *argumento* con un *argument_name* se conoce como un ***argumento con nombre***, mientras que un *argumento* sin un  *argument_name* es un ***argumento posicional***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="d7bba-529">Es un error para un argumento posicional en aparecer después de un argumento con nombre en un *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="d7bba-530">El *argument_value* puede tomar uno de los formatos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="d7bba-531">Un *expresión*, que indica que el argumento se pasa como un parámetro de valor ([parámetros del valor](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="d7bba-532">La palabra clave `ref` seguido por un *variable_reference* ([referencias de variables](variables.md#variable-references)), que indica que el argumento se pasa como un parámetro de referencia ([hacen referencia a parámetros ](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="d7bba-533">Una variable debe asignarlo definitivamente ([asignación definitiva](variables.md#definite-assignment)) antes de que se puede pasar como un parámetro de referencia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="d7bba-534">La palabra clave `out` seguido por un *variable_reference* ([referencias de variables](variables.md#variable-references)), que indica que el argumento se pasa como un parámetro de salida ([parámetrosdesalida](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="d7bba-535">Una variable se considera asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) siguiendo una invocación de miembros de función en el que la variable se pasa como parámetro de salida.</span><span class="sxs-lookup"><span data-stu-id="d7bba-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="d7bba-536">Parámetros correspondientes</span><span class="sxs-lookup"><span data-stu-id="d7bba-536">Corresponding parameters</span></span>

<span data-ttu-id="d7bba-537">Para cada argumento de una lista de argumentos debe haber un parámetro correspondiente en el miembro de función o un delegado que se invoca.</span><span class="sxs-lookup"><span data-stu-id="d7bba-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="d7bba-538">La lista de parámetros utilizada en el siguiente ejemplo se determina como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="d7bba-539">Para los métodos virtuales y los indizadores definidos en las clases, la lista de parámetros seleccionado de la declaración más específica o reemplazar el miembro de función, empezando por el tipo estático del receptor y buscar a través de sus clases base.</span><span class="sxs-lookup"><span data-stu-id="d7bba-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="d7bba-540">Para los métodos de interfaz y los indizadores, se toma la lista de parámetros forman la definición más específica del miembro, empezando por el tipo de interfaz y la búsqueda a través de las interfaces base.</span><span class="sxs-lookup"><span data-stu-id="d7bba-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="d7bba-541">Si no se encuentra ninguna lista de parámetros únicos, se construye una lista de parámetros con nombres accesibles y no existen parámetros opcionales, por lo que no se pueden usar parámetros con nombre o se omiten los argumentos opcionales invocaciones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="d7bba-542">Para los métodos parciales, se usa la lista de parámetros de la declaración de método parcial de definición.</span><span class="sxs-lookup"><span data-stu-id="d7bba-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="d7bba-543">No hay sólo una lista de parámetros único, que es el que se usa para todos los demás miembros de función y delegados.</span><span class="sxs-lookup"><span data-stu-id="d7bba-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="d7bba-544">La posición de un argumento o parámetro se define como el número de argumentos o parámetros que le precede en la lista de argumentos o la lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="d7bba-545">Los parámetros correspondientes para los argumentos de función miembro se establecen como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="d7bba-546">Argumentos de la *argument_list* de constructores de instancias, métodos, indizadores y delegados:</span><span class="sxs-lookup"><span data-stu-id="d7bba-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="d7bba-547">Un argumento de posición donde se produce un parámetro fijo en la misma posición en la lista de parámetros que se corresponde a ese parámetro.</span><span class="sxs-lookup"><span data-stu-id="d7bba-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="d7bba-548">Un argumento posicional de un miembro de función con una matriz de parámetros que se invoca en su forma normal corresponde a la matriz de parámetros, que debe aparecer en la misma posición en la lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="d7bba-549">Un argumento posicional de un miembro de función con una matriz de parámetros que se invoca en su forma expandida, donde ningún parámetro fijo se produce en la misma posición en la lista de parámetros, corresponde a un elemento de la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="d7bba-550">Un argumento con nombre se corresponde con el parámetro del mismo nombre en la lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="d7bba-551">Para los indizadores, al invocar el `set` descriptor de acceso, la expresión especificada como el operando derecho del operador de asignación corresponde a implícito `value` parámetro de la `set` declaración del descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="d7bba-552">Para las propiedades, al invocar el `get` no son de descriptor de acceso no existe ningún argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="d7bba-553">Al invocar el `set` descriptor de acceso, la expresión especificada como el operando derecho del operador de asignación corresponde a implícito `value` parámetro de la `set` declaración del descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="d7bba-554">Para los operadores unarios definido por el usuario (incluidas las conversiones), el único operando corresponde al parámetro único de la declaración del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="d7bba-555">Para los operadores binarios definidos por el usuario, el operando izquierdo corresponde al primer parámetro y el operando derecho se corresponde con el segundo parámetro de la declaración del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="d7bba-556">Evaluación de tiempo de ejecución de las listas de argumentos</span><span class="sxs-lookup"><span data-stu-id="d7bba-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="d7bba-557">Durante el procesamiento de tiempo de ejecución de una invocación de miembros de función ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), las expresiones o referencias a variables de una lista de argumentos se evalúan en orden, de izquierda a derecha, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="d7bba-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="d7bba-558">Para un parámetro de valor, se evalúa la expresión de argumento y una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) al parámetro correspondiente del tipo que se realiza.</span><span class="sxs-lookup"><span data-stu-id="d7bba-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="d7bba-559">El valor resultante se convierte en el valor inicial del parámetro de valor en la invocación de miembros de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="d7bba-560">Para un parámetro de referencia o de salida, se evalúa la referencia de variable y la ubicación de almacenamiento resultante se convierte en la ubicación de almacenamiento representada por el parámetro de la invocación de miembros de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="d7bba-561">Si la referencia de variable como un parámetro de referencia o de salida es un elemento de matriz de un *reference_type*, se realiza una comprobación de tiempo de ejecución para asegurarse de que el tipo de elemento de la matriz es idéntico al tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="d7bba-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="d7bba-562">Si se produce un error en esta comprobación, un `System.ArrayTypeMismatchException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="d7bba-563">Los métodos, indizadores y constructores de instancias pueden declarar su parámetro de derecha a ser una matriz de parámetros ([las matrices de parámetros](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="d7bba-564">Estos miembros de función se invocan en su forma normal o en su forma expandida, dependiendo de lo que es aplicable ([miembro de función aplicable](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="d7bba-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="d7bba-565">Cuando se invoca un miembro de función con una matriz de parámetros en su forma normal, el argumento especificado para la matriz de parámetros debe ser una sola expresión que es implícitamente convertible ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo de matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="d7bba-566">En este caso, la matriz de parámetros actúa exactamente como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="d7bba-567">Cuando se invoca un miembro de función con una matriz de parámetros en su forma expandida, la invocación debe especificar cero o más argumentos posicionales para la matriz de parámetros, donde cada argumento es una expresión que se pueda convertir implícitamente ([implícitas las conversiones](conversions.md#implicit-conversions)) para el tipo de elemento de la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="d7bba-568">En este caso, la invocación crea una instancia del parámetro de tipo de matriz con una longitud correspondiente al número de argumentos, inicializa los elementos de la instancia de matriz con los valores de argumento especificado y usa la instancia de matriz recién creado como real argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="d7bba-569">Las expresiones de una lista de argumentos siempre se evalúan en el orden en que se escriben.</span><span class="sxs-lookup"><span data-stu-id="d7bba-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="d7bba-570">Por lo tanto, el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-570">Thus, the example</span></span>
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
<span data-ttu-id="d7bba-571">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="d7bba-571">produces the output</span></span>
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="d7bba-572">Las reglas de covarianza de matriz ([covarianza de matrices](arrays.md#array-covariance)) permiten que un valor de un tipo de matriz `A[]` sea una referencia a una instancia de un tipo de matriz `B[]`, siempre existe una conversión implícita de referencia de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="d7bba-573">Debido a estas reglas, cuando un elemento de matriz de un *reference_type* se pasa como un parámetro de referencia o de salida, una comprobación en tiempo de ejecución no es necesaria para asegurarse de que el tipo de elemento real de la matriz es idéntico del parámetro.</span><span class="sxs-lookup"><span data-stu-id="d7bba-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="d7bba-574">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-574">In the example</span></span>
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
<span data-ttu-id="d7bba-575">la segunda invocación de `F` hace que un `System.ArrayTypeMismatchException` se produce porque el tipo de elemento real de `b` es `string` y no `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="d7bba-576">Cuando se invoca un miembro de función con una matriz de parámetros en su forma expandida, la invocación se procesa exactamente como si una expresión de creación de matriz con un inicializador de matriz ([expresiones de creación de matriz](expressions.md#array-creation-expressions)) se ha insertado en torno a la parámetros expandidos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="d7bba-577">Por ejemplo, dada la declaración</span><span class="sxs-lookup"><span data-stu-id="d7bba-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="d7bba-578">las siguientes invocaciones de la forma expandida del método</span><span class="sxs-lookup"><span data-stu-id="d7bba-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="d7bba-579">corresponden exactamente a</span><span class="sxs-lookup"><span data-stu-id="d7bba-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="d7bba-580">En concreto, tenga en cuenta que se crea una matriz vacía cuando no hay ningún argumento para la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="d7bba-581">Cuando se omiten los argumentos de un miembro de función con parámetros opcionales correspondientes, implícitamente se pasan los argumentos predeterminados de la declaración de miembro de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="d7bba-582">Dado que siempre son constantes, la evaluación no afectará el orden de evaluación de los argumentos restantes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="d7bba-583">Inferencia de tipos</span><span class="sxs-lookup"><span data-stu-id="d7bba-583">Type inference</span></span>

<span data-ttu-id="d7bba-584">Cuando se llama a un método genérico sin especificar argumentos de tipo, un ***inferencia*** proceso intenta inferir los argumentos de tipo para la llamada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="d7bba-585">La presencia de inferencia de tipos permite una sintaxis más cómoda para usarse para llamar a un método genérico y permite al programador evitar especificar información de tipo redundante.</span><span class="sxs-lookup"><span data-stu-id="d7bba-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="d7bba-586">Por ejemplo, dada la declaración de método:</span><span class="sxs-lookup"><span data-stu-id="d7bba-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="d7bba-587">es posible invocar la `Choose` método sin especificar explícitamente un argumento de tipo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="d7bba-588">A través de la inferencia de tipos, los argumentos de tipo `int` y `string` se determinan a partir de los argumentos al método.</span><span class="sxs-lookup"><span data-stu-id="d7bba-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="d7bba-589">Inferencia de tipos se produce como parte del procesamiento de tiempo de enlace de una invocación de método ([las invocaciones de método](expressions.md#method-invocations)) y tiene lugar antes de los pasos de resolución de sobrecarga de la invocación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="d7bba-590">Cuando se especifica un grupo de método concreto en una invocación de método y se especifica ningún argumento de tipo como parte de la invocación del método, la inferencia de tipos se aplica a cada método genérico en el grupo de métodos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="d7bba-591">Si la inferencia de tipos se realiza correctamente, se usan los argumentos de tipo inferidos para determinar los tipos de argumentos para la resolución de sobrecarga subsiguiente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="d7bba-592">Si la resolución de sobrecarga, elige un método genérico al invocar, los argumentos de tipo se usan como argumentos de tipo real de la invocación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="d7bba-593">Si se produce un error en la inferencia de tipos para un método concreto, ese método no participa en la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="d7bba-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="d7bba-594">El error de inferencia de tipos, de por sí, no provoca un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="d7bba-595">Sin embargo, a menudo lo conduce a un error en tiempo de enlace, cuando la resolución de sobrecarga, a continuación, no se encontró ningún método aplicable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="d7bba-596">Si el número de argumentos proporcionado es diferente del número de parámetros del método, a continuación, inferencia inmediatamente se produce un error.</span><span class="sxs-lookup"><span data-stu-id="d7bba-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="d7bba-597">En caso contrario, se supone que el método genérico tiene la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="d7bba-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="d7bba-598">Con una llamada al método del formulario `M(E1...Em)` la tarea de inferencia de tipos es encontrar los argumentos de tipo único `S1...Sn` para cada uno de los parámetros de tipo `X1...Xn` para que la llamada `M<S1...Sn>(E1...Em)` pasa a ser válido.</span><span class="sxs-lookup"><span data-stu-id="d7bba-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="d7bba-599">Durante el proceso de inferencia de cada parámetro de tipo `Xi` sea *fijo* a un tipo determinado `Si` o *tipo unfixed* con un conjunto de asociados *límites*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="d7bba-600">Cada uno de los límites es algún tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="d7bba-601">Cada variable de tipo inicialmente `Xi` es de tipo unfixed con un conjunto vacío de límites.</span><span class="sxs-lookup"><span data-stu-id="d7bba-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="d7bba-602">Inferencia de tipos realiza en fases.</span><span class="sxs-lookup"><span data-stu-id="d7bba-602">Type inference takes place in phases.</span></span> <span data-ttu-id="d7bba-603">Cada fase intentará deducir los argumentos de tipo para las variables de tipo más según las conclusiones de la fase anterior.</span><span class="sxs-lookup"><span data-stu-id="d7bba-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="d7bba-604">La primera fase hace algunos inferencias iniciales de los límites indicados, mientras que la segunda fase corrige las variables de tipo a tipos específicos y deduce aún más los límites.</span><span class="sxs-lookup"><span data-stu-id="d7bba-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="d7bba-605">Puede tener la segunda fase se repite un número de veces.</span><span class="sxs-lookup"><span data-stu-id="d7bba-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="d7bba-606">*Nota:* Tipo inferencia se lleva a cabo no solo cuando se llama a un método genérico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="d7bba-607">Inferencia de tipos para la conversión de grupos de métodos se describe en [inferencia de tipos para la conversión de grupos de métodos](expressions.md#type-inference-for-conversion-of-method-groups) y encontrar el mejor tipo común de un conjunto de expresiones se describe en [encontrar el mejor tipo común de un conjunto expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span><span class="sxs-lookup"><span data-stu-id="d7bba-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="d7bba-608">La primera fase</span><span class="sxs-lookup"><span data-stu-id="d7bba-608">The first phase</span></span>

<span data-ttu-id="d7bba-609">Para cada uno de los argumentos del método `Ei`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="d7bba-610">Si `Ei` es una función anónima, un *inferencia de tipos de parámetro explícito* ([inferencias de tipo de parámetro explícito](expressions.md#explicit-parameter-type-inferences)) se realiza desde `Ei` a `Ti`</span><span class="sxs-lookup"><span data-stu-id="d7bba-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="d7bba-611">De lo contrario, si `Ei` tiene un tipo `U` y `xi` es un parámetro de valor un *inferencia de límite inferior* estará *desde* `U` *a* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="d7bba-612">De lo contrario, si `Ei` tiene un tipo `U` y `xi` es un `ref` o `out` parámetro una *exacta inferencia* se realiza *desde* `U` *a* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="d7bba-613">En caso contrario, no se realiza ninguna inferencia para este argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="d7bba-614">La segunda fase</span><span class="sxs-lookup"><span data-stu-id="d7bba-614">The second phase</span></span>

<span data-ttu-id="d7bba-615">La segunda fase procede como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="d7bba-616">Todos los *tipo unfixed* variables de tipo `Xi` cuáles no *dependen* ([dependencia](expressions.md#dependence)) cualquier `Xj` son fijos ([fijación](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="d7bba-617">Si no existe estas variables de tipo, todos *tipo unfixed* variables de tipo `Xi` son *fijo* para el que se mantenga todos los elementos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="d7bba-618">Hay al menos una variable de tipo `Xj` que depende `Xi`</span><span class="sxs-lookup"><span data-stu-id="d7bba-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="d7bba-619">`Xi` tiene un conjunto no vacío de límites</span><span class="sxs-lookup"><span data-stu-id="d7bba-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="d7bba-620">Si no existen estas variables de tipo y todavía hay *tipo unfixed* variables de tipo, se produce un error de inferencia de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="d7bba-621">En caso contrario, si ningún otro *tipo unfixed* existen variables de tipo, la inferencia de tipos se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="d7bba-622">En caso contrario, para todos los argumentos `Ei` con el tipo de parámetro correspondiente `Ti` donde el *tipos de salida* ([tipos de salida](expressions.md#output-types)) contienen *tipo unfixed* variables de tipo `Xj` pero la *tipos de entrada* ([tipos de entrada](expressions.md#input-types)) no lo hace, un *inferencia de salida* ([inferencias de tipo de salida ](expressions.md#output-type-inferences)) se realiza *desde* `Ei` *a* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="d7bba-623">A continuación, se repite la segunda fase.</span><span class="sxs-lookup"><span data-stu-id="d7bba-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="d7bba-624">Tipos de entrada</span><span class="sxs-lookup"><span data-stu-id="d7bba-624">Input types</span></span>

<span data-ttu-id="d7bba-625">Si `E` es un grupo de método o función anónima con tipo implícito y `T` es un delegado de tipo o tipo de árbol de expresión, a continuación, todos los tipos de parámetro de `T` son *tipos de entrada* de `E` *con tipo* `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="d7bba-626">Tipos de salida</span><span class="sxs-lookup"><span data-stu-id="d7bba-626">Output types</span></span>

<span data-ttu-id="d7bba-627">Si `E` es un grupo de métodos o una función anónima y `T` es un delegado de tipo o tipo de árbol de expresión y el tipo de valor devuelto de `T` es un *tipo de salida* `E` *con tipo*  `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="d7bba-628">Dependencia</span><span class="sxs-lookup"><span data-stu-id="d7bba-628">Dependence</span></span>

<span data-ttu-id="d7bba-629">Un *tipo unfixed* variable de tipo `Xi` *depende directamente* una variable de tipo unfixed tipo `Xj` if para algunos argumentos `Ek` con tipo `Tk` `Xj` se produce en un *tipo de entrada* de `Ek` con tipo `Tk` y `Xi` se produce en un *tipo de salida* de `Ek` con tipo `Tk`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="d7bba-630">`Xj` *depende de* `Xi` si `Xj` *depende directamente* `Xi` o si `Xi` *depende directamente* `Xk` y `Xk` *depende* `Xj`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="d7bba-631">Por lo tanto "depende" es el cierre transitivo pero no reflexivo de "depende directamente".</span><span class="sxs-lookup"><span data-stu-id="d7bba-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="d7bba-632">Inferencias de tipo de salida</span><span class="sxs-lookup"><span data-stu-id="d7bba-632">Output type inferences</span></span>

<span data-ttu-id="d7bba-633">Un *inferencia de salida* estará *desde* una expresión `E` *a* un tipo `T` de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7bba-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="d7bba-634">Si `E` es una función anónima con el tipo de valor devuelto deducido `U` ([tipo de valor devuelto deducido](expressions.md#inferred-return-type)) y `T` es un tipo de delegado o el tipo de árbol de expresión con tipo de valor devuelto `Tb`, a continuación, un *inferencia de límite inferior* ([inferencias de límite inferior](expressions.md#lower-bound-inferences)) se realiza *desde* `U` *a* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="d7bba-635">De lo contrario, si `E` es un grupo de métodos y `T` es un tipo de delegado o el tipo de árbol de expresión con tipos de parámetro `T1...Tk` y tipo de valor devuelto `Tb`y la resolución de sobrecarga de `E` con los tipos de `T1...Tk` da como resultado una método con tipo de valor devuelto único `U`, un *inferencia de límite inferior* estará *desde* `U` *a* `Tb`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="d7bba-636">De lo contrario, si `E` es una expresión con tipo `U`, un *inferencia de límite inferior* estará *desde* `U` *a* `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="d7bba-637">En caso contrario, no se realizan inferencias.</span><span class="sxs-lookup"><span data-stu-id="d7bba-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="d7bba-638">Inferencias de tipo de parámetro explícito</span><span class="sxs-lookup"><span data-stu-id="d7bba-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="d7bba-639">Un *inferencia de tipos de parámetro explícito* estará *desde* una expresión `E` *a* un tipo `T` de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7bba-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="d7bba-640">Si `E` es una función anónima con tipo explícito con tipos de parámetro `U1...Uk` y `T` es un tipo de delegado o el tipo de árbol de expresión con tipos de parámetro `V1...Vk` , a continuación, para cada `Ui` un *exacta inferencia* ([exacta inferencias](expressions.md#exact-inferences)) se realiza *desde* `Ui` *a* correspondiente `Vi`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="d7bba-641">Inferencias exactas</span><span class="sxs-lookup"><span data-stu-id="d7bba-641">Exact inferences</span></span>

<span data-ttu-id="d7bba-642">Un *exacta inferencia* *desde* un tipo `U` *a* un tipo `V` se realiza como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="d7bba-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="d7bba-643">Si `V` es uno de los *tipo unfixed* `Xi` , a continuación, `U` se agrega al conjunto de límites exactos para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="d7bba-644">En caso contrario, establece `V1...Vk` y `U1...Uk` se determinan mediante la comprobación de si se aplica cualquiera de los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="d7bba-645">`V` es un tipo de matriz `V1[...]` y `U` es un tipo de matriz `U1[...]` del mismo rango</span><span class="sxs-lookup"><span data-stu-id="d7bba-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="d7bba-646">`V` es el tipo `V1?` y `U` es el tipo `U1?`</span><span class="sxs-lookup"><span data-stu-id="d7bba-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="d7bba-647">`V` es un tipo construido `C<V1...Vk>`y `U` es un tipo construido `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="d7bba-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="d7bba-648">Si cualquiera de estos casos, a continuación, aplicar un *exacta inferencia* se realiza *desde* cada `Ui` *a* correspondiente `Vi`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="d7bba-649">En caso contrario, no se realizan inferencias.</span><span class="sxs-lookup"><span data-stu-id="d7bba-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="d7bba-650">Límite inferior inferencias</span><span class="sxs-lookup"><span data-stu-id="d7bba-650">Lower-bound inferences</span></span>

<span data-ttu-id="d7bba-651">Un *inferencia de límite inferior* *desde* un tipo `U` *a* un tipo `V` se realiza como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="d7bba-652">Si `V` es uno de los *tipo unfixed* `Xi` , a continuación, `U` se agrega al conjunto de límites inferiores `Xi`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="d7bba-653">De lo contrario, si `V` es el tipo `V1?`y `U` es el tipo `U1?` , a continuación, se realiza una inferencia de límite inferior desde `U1` a `V1`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="d7bba-654">En caso contrario, establece `U1...Uk` y `V1...Vk` se determinan mediante la comprobación de si se aplica cualquiera de los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="d7bba-655">`V` es un tipo de matriz `V1[...]` y `U` es un tipo de matriz `U1[...]` (o un parámetro de tipo cuyo tipo base eficaz es `U1[...]`) del mismo rango</span><span class="sxs-lookup"><span data-stu-id="d7bba-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="d7bba-656">`V` es uno de `IEnumerable<V1>`, `ICollection<V1>` o `IList<V1>` y `U` es un tipo de matriz unidimensional `U1[]`(o un parámetro de tipo cuyo tipo base eficaz es `U1[]`)</span><span class="sxs-lookup"><span data-stu-id="d7bba-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="d7bba-657">`V` es un tipo construido class, struct, interfaz o delegado `C<V1...Vk>` y hay un único tipo `C<U1...Uk>` que `U` (o, si `U` es un parámetro de tipo, su clase base efectivo o cualquier miembro de su conjunto de interfaces efectivas) es es idéntica a hereda (directa o indirectamente) o implementa (directa o indirectamente) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="d7bba-658">(La restricción "unicidad" significa que, en la interfaz case `C<T> {} class U: C<X>, C<Y> {}`, entonces no se realiza ninguna inferencia al deducir de `U` a `C<T>` porque `U1` podría ser `X` o `Y`.)</span><span class="sxs-lookup"><span data-stu-id="d7bba-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="d7bba-659">Si se aplica cualquiera de estos casos, a continuación, se realiza una inferencia *desde* cada `Ui` *a* correspondiente `Vi` como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="d7bba-660">Si `Ui` no se sabe que es un tipo de referencia una *exacta inferencia* se realiza</span><span class="sxs-lookup"><span data-stu-id="d7bba-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="d7bba-661">De lo contrario, si `U` es un tipo de matriz, a continuación, un *inferencia de límite inferior* se realiza</span><span class="sxs-lookup"><span data-stu-id="d7bba-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="d7bba-662">De lo contrario, si `V` es `C<V1...Vk>` , a continuación, inferencia depende del parámetro de tipo i-ésimo de `C`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="d7bba-663">Si es covariante un *inferencia de límite inferior* se realiza.</span><span class="sxs-lookup"><span data-stu-id="d7bba-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="d7bba-664">Si es contravariante una *inferencia de límite superior* se realiza.</span><span class="sxs-lookup"><span data-stu-id="d7bba-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="d7bba-665">Si es invariable una *exacta inferencia* se realiza.</span><span class="sxs-lookup"><span data-stu-id="d7bba-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="d7bba-666">En caso contrario, no se realizan inferencias.</span><span class="sxs-lookup"><span data-stu-id="d7bba-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="d7bba-667">Límite superior inferencias</span><span class="sxs-lookup"><span data-stu-id="d7bba-667">Upper-bound inferences</span></span>

<span data-ttu-id="d7bba-668">Un *inferencia de límite superior* *desde* un tipo `U` *a* un tipo `V` se realiza como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="d7bba-669">Si `V` es uno de los *tipo unfixed* `Xi` , a continuación, `U` se agrega al conjunto de límites superiores `Xi`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="d7bba-670">En caso contrario, establece `V1...Vk` y `U1...Uk` se determinan mediante la comprobación de si se aplica cualquiera de los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="d7bba-671">`U` es un tipo de matriz `U1[...]` y `V` es un tipo de matriz `V1[...]` del mismo rango</span><span class="sxs-lookup"><span data-stu-id="d7bba-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="d7bba-672">`U` es uno de `IEnumerable<Ue>`, `ICollection<Ue>` o `IList<Ue>` y `V` es un tipo de matriz unidimensional `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="d7bba-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="d7bba-673">`U` es el tipo `U1?` y `V` es el tipo `V1?`</span><span class="sxs-lookup"><span data-stu-id="d7bba-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="d7bba-674">`U` es la clase construida, struct, interfaz o delegado tipo `C<U1...Uk>` y `V` es una clase, struct, interfaz o delegado de tipo que es idéntica, hereda (directa o indirectamente) o implementa (directa o indirectamente) un tipo único `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="d7bba-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="d7bba-675">(La restricción "unicidad" significa que si tenemos `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, a continuación, no se realiza ninguna inferencia al deducir de `C<U1>` a `V<Q>`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="d7bba-676">No se realizan deducciones de `U1` como `X<Q>` o `Y<Q>`.)</span><span class="sxs-lookup"><span data-stu-id="d7bba-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="d7bba-677">Si se aplica cualquiera de estos casos, a continuación, se realiza una inferencia *desde* cada `Ui` *a* correspondiente `Vi` como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="d7bba-678">Si `Ui` no se sabe que es un tipo de referencia una *exacta inferencia* se realiza</span><span class="sxs-lookup"><span data-stu-id="d7bba-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="d7bba-679">De lo contrario, si `V` es un tipo de matriz una *inferencia de límite superior* se realiza</span><span class="sxs-lookup"><span data-stu-id="d7bba-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="d7bba-680">De lo contrario, si `U` es `C<U1...Uk>` , a continuación, inferencia depende del parámetro de tipo i-ésimo de `C`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="d7bba-681">Si es covariante una *inferencia de límite superior* se realiza.</span><span class="sxs-lookup"><span data-stu-id="d7bba-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="d7bba-682">Si es contravariante un *inferencia de límite inferior* se realiza.</span><span class="sxs-lookup"><span data-stu-id="d7bba-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="d7bba-683">Si es invariable una *exacta inferencia* se realiza.</span><span class="sxs-lookup"><span data-stu-id="d7bba-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="d7bba-684">En caso contrario, no se realizan inferencias.</span><span class="sxs-lookup"><span data-stu-id="d7bba-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="d7bba-685">Corregir</span><span class="sxs-lookup"><span data-stu-id="d7bba-685">Fixing</span></span>

<span data-ttu-id="d7bba-686">Un *tipo unfixed* variable de tipo `Xi` con un conjunto de límites es *fijo* como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="d7bba-687">El conjunto de *tipos candidatos* `Uj` comienza como el conjunto de todos los tipos en el conjunto de límites para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="d7bba-688">A continuación, examinaremos cada límite para `Xi` a su vez: Para cada límite exacto `U` de `Xi` todos los tipos de `Uj` que no son idénticas a `U` se quitan del conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="d7bba-689">Para cada límite inferior `U` de `Xi` todos los tipos de `Uj` al cual se está *no* una conversión implícita de `U` se quitan del conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="d7bba-690">Para cada límite superior `U` de `Xi` todos los tipos de `Uj` desde los cuales hay es *no* una conversión implícita a `U` se quitan del conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="d7bba-691">If entre los tipos de candidatos restantes `Uj` hay un único tipo `V` de que hay una conversión implícita a todos los demás tipos de candidato, a continuación, `Xi` está fija en `V`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="d7bba-692">En caso contrario, se produce un error en la inferencia de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="d7bba-693">Tipo de valor devuelto deducido</span><span class="sxs-lookup"><span data-stu-id="d7bba-693">Inferred return type</span></span>

<span data-ttu-id="d7bba-694">Deducidos tipo de valor devuelto de una función anónima `F` se usa durante la resolución de sobrecarga y de inferencia de tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="d7bba-695">Solo se puede determinar el tipo de valor devuelto deducido para una función anónima donde todos los parámetros se conocen los tipos, ya sea porque se les asigna explícitamente, proporcionan a través de una conversión de función anónima o deduce durante la inferencia de tipos en envolvente genérico invocación del método.</span><span class="sxs-lookup"><span data-stu-id="d7bba-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="d7bba-696">El ***deduce el tipo de resultado*** se determina como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="d7bba-697">Si el cuerpo de `F` es un *expresión* que tiene un tipo y, a continuación, el tipo de resultado deducido `F` es el tipo de dicha expresión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="d7bba-698">Si el cuerpo de `F` es un *bloque* y el conjunto de expresiones en el bloque `return` instrucciones tiene un tipo común mejor `T` ([encontrar el mejor tipo común de un conjunto de expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), a continuación, el tipo de resultado deducido `F` es `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="d7bba-699">En caso contrario, no se puede inferir un tipo de resultado para `F`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="d7bba-700">El ***deduce el tipo de valor devuelto*** se determina como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="d7bba-701">Si `F` es asincrónico y el cuerpo de `F` es una expresión se clasifica como nada ([clasificaciones de expresiones](expressions.md#expression-classifications)), o un bloque de instrucciones donde ninguna instrucción return tiene expresiones, deducidos devuelven es de tipo `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="d7bba-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="d7bba-702">Si `F` es asincrónico y tiene un tipo de resultado deducido `T`, deducidos devuelven es de tipo `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="d7bba-703">Si `F` es no asincrónicas y tiene un tipo de resultado deducido `T`, deducidos devuelven es de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="d7bba-704">En caso contrario, no se puede inferir un tipo de valor devuelto para `F`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="d7bba-705">Consideremos un ejemplo de inferencia de tipos que implican las funciones anónimas, el `Select` declara el método de extensión en el `System.Linq.Enumerable` clase:</span><span class="sxs-lookup"><span data-stu-id="d7bba-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

<span data-ttu-id="d7bba-706">Suponiendo que el `System.Linq` se ha importado el espacio de nombres con un `using` cláusula y dada una clase `Customer` con un `Name` propiedad de tipo `string`, el `Select` método puede utilizarse para seleccionar los nombres de una lista de clientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="d7bba-707">La invocación del método de extensión ([las invocaciones de método de extensión](expressions.md#extension-method-invocations)) de `Select` se procesa al reescribir la invocación a una invocación de método estático:</span><span class="sxs-lookup"><span data-stu-id="d7bba-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="d7bba-708">Puesto que los argumentos de tipo no se especifican explícitamente, inferencia de tipo se usa para inferir los argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="d7bba-709">En primer lugar, el `customers` argumento está relacionado con la `source` parámetro, inferir `T` sea `Customer`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="d7bba-710">A continuación, utilizando la función anónima escriba se ha descrito anteriormente, el proceso de inferencia `c` tiene tipo `Customer`y la expresión `c.Name` está relacionado con el tipo de valor devuelto de la `selector` parámetro, deducir `S` sea `string`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="d7bba-711">Por lo tanto, es equivalente a la invocación</span><span class="sxs-lookup"><span data-stu-id="d7bba-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="d7bba-712">y el resultado es de tipo `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="d7bba-713">El ejemplo siguiente muestra el tipo de función anónima cómo esta característica permite la información de tipo "fluir" entre los argumentos en una invocación de método genérico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="d7bba-714">Dado que el método:</span><span class="sxs-lookup"><span data-stu-id="d7bba-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="d7bba-715">Inferencia de tipos para la invocación:</span><span class="sxs-lookup"><span data-stu-id="d7bba-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="d7bba-716">continúa como sigue: Primero, el argumento `"1:15:30"` está relacionado con la `value` parámetro, inferir `X` sea `string`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="d7bba-717">A continuación, el parámetro de la primera función anónima, `s`, se proporciona el tipo deducido `string`y la expresión `TimeSpan.Parse(s)` está relacionado con el tipo de valor devuelto de `f1`, deducir `Y` sea `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="d7bba-718">Por último, el parámetro de la segunda función anónima, `t`, se proporciona el tipo deducido `System.TimeSpan`y la expresión `t.TotalSeconds` está relacionado con el tipo de valor devuelto de `f2`, deducir `Z` sea `double`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="d7bba-719">Por lo tanto, el resultado de la invocación es de tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="d7bba-720">Inferencia de tipos para la conversión de grupos de métodos</span><span class="sxs-lookup"><span data-stu-id="d7bba-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="d7bba-721">Al igual que las llamadas de métodos genéricos, inferencia de tipos también se debe aplicar cuando un grupo de métodos `M` que contiene un método genérico se convierte en un tipo de delegado dado `D` ([conversiones de métodos de grupo](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="d7bba-722">Dado un método</span><span class="sxs-lookup"><span data-stu-id="d7bba-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="d7bba-723">y el grupo de métodos `M` que se asigna al tipo de delegado `D` la tarea de inferencia de tipos es encontrar los argumentos de tipo `S1...Sn` para que la expresión:</span><span class="sxs-lookup"><span data-stu-id="d7bba-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="d7bba-724">se vuelve compatible ([declaraciones de delegado](delegates.md#delegate-declarations)) con `D`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="d7bba-725">A diferencia del algoritmo de inferencia de tipo para las llamadas de método genérico, en este caso hay sólo argumento *tipos*, ningún argumento *expresiones*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="d7bba-726">En concreto, no hay ninguna función anónima y, por tanto, sin necesidad de varias fases de inferencia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="d7bba-727">En su lugar, todos los `Xi` se consideran *tipo unfixed*y un *inferencia de límite inferior* estará *desde* cada tipo de argumento `Uj` de `D` *a* correspondiente tipo de parámetro `Tj` de `M`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="d7bba-728">If para cualquiera de los `Xi` no se encontraron límites, se produce un error en la inferencia de tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="d7bba-729">De lo contrario, todos los `Xi` son *fijo* correspondiente `Si`, que son el resultado de la inferencia de tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="d7bba-730">Buscar el mejor tipo común de un conjunto de expresiones</span><span class="sxs-lookup"><span data-stu-id="d7bba-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="d7bba-731">En algunos casos, tiene que deducir un conjunto de expresiones de un tipo común.</span><span class="sxs-lookup"><span data-stu-id="d7bba-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="d7bba-732">En particular, los tipos de elemento de matrices con tipo implícito y los tipos devueltos de funciones anónimas con *bloque* se encuentran los cuerpos de esta manera.</span><span class="sxs-lookup"><span data-stu-id="d7bba-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="d7bba-733">De manera intuitiva, dado un conjunto de expresiones `E1...Em` este inferencia debe ser equivalente a llamar a un método</span><span class="sxs-lookup"><span data-stu-id="d7bba-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="d7bba-734">con el `Ei` como argumentos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="d7bba-735">Más concretamente, la inferencia comienza con un *tipo unfixed* variable de tipo `X`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="d7bba-736">*Salida tipo inferencias* , a continuación, se realizan *desde* cada `Ei` *a* `X`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="d7bba-737">Por último, `X` es *fijo* y, si se realiza correctamente, el resultado escriba `S` es el mejor tipo común resultante para las expresiones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="d7bba-738">Si no existe ese `S` existe, las expresiones no tienen ningún tipo común mejor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="d7bba-739">Resolución de sobrecarga</span><span class="sxs-lookup"><span data-stu-id="d7bba-739">Overload resolution</span></span>

<span data-ttu-id="d7bba-740">La resolución de sobrecarga es un mecanismo de enlace de tiempo para seleccionar el mejor miembro de función que puede invocarse dados una lista de argumentos y un conjunto de miembros de función candidatos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="d7bba-741">La resolución de sobrecarga selecciona el miembro de función para invocar en los siguientes contextos distintos dentro de C#:</span><span class="sxs-lookup"><span data-stu-id="d7bba-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="d7bba-742">Invocación de un método llamado en un *invocation_expression* ([las invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="d7bba-743">Invocación de un constructor de instancia con nombre en un *object_creation_expression* ([expresiones de creación de objeto](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="d7bba-744">Invocación de un descriptor de acceso de indexador a través de un *element_access* ([acceso de elemento](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="d7bba-745">Invocación de un operador predefinido o definidos por el usuario al que hace referencia en una expresión ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution) y [de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="d7bba-746">Cada uno de estos contextos define el conjunto de miembros de función candidatos y la lista de argumentos en su propia manera única, como se describe en detalle en las secciones anteriores.</span><span class="sxs-lookup"><span data-stu-id="d7bba-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="d7bba-747">Por ejemplo, el conjunto de candidatos para una invocación de método no incluye los métodos marcados `override` ([búsqueda de miembros](expressions.md#member-lookup)), y los métodos en una clase base no son candidatos cualquier método en una clase derivada que se apliquen ([ Las invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="d7bba-748">Una vez que se han identificado los miembros de función candidatos y la lista de argumentos, la selección del mejor miembro de función es el mismo en todos los casos:</span><span class="sxs-lookup"><span data-stu-id="d7bba-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="d7bba-749">Dado el conjunto de miembros de función candidatos aplicables, el mejor miembro de función conjunto se encuentra.</span><span class="sxs-lookup"><span data-stu-id="d7bba-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="d7bba-750">Si el conjunto contiene a sólo un miembro de función, ese miembro de función es el mejor miembro de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="d7bba-751">En caso contrario, el mejor miembro de función es el miembro de una función que es mejor que todos los demás miembros de función con respecto a la lista de argumentos determinada, siempre que cada miembro de función se compara con todos los demás miembros de función mediante las reglas de [ Mejor miembro de función](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="d7bba-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="d7bba-752">Si no hay exactamente un miembro de función que es mejor que todos los demás miembros de función, a continuación, la invocación de miembros de función es ambigua y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-753">Las secciones siguientes definen el significado exacto de los términos ***miembro de función aplicable*** y ***mejor miembro de función***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="d7bba-754">Miembro de función aplicable</span><span class="sxs-lookup"><span data-stu-id="d7bba-754">Applicable function member</span></span>

<span data-ttu-id="d7bba-755">Un miembro de función se dice que un ***miembro de función aplicable*** con respecto a una lista de argumentos `A` cuando todas las opciones siguientes son verdaderas:</span><span class="sxs-lookup"><span data-stu-id="d7bba-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="d7bba-756">Cada argumento de `A` corresponde a un parámetro en la declaración de miembro de función, como se describe en [correspondientes parámetros](expressions.md#corresponding-parameters), y cualquier parámetro a la que se corresponde ningún argumento es un parámetro opcional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="d7bba-757">Para cada argumento de `A`, el modo del argumento de paso de parámetros (es decir, el valor, `ref`, o `out`) es idéntico al modo de pasar parámetros del parámetro correspondiente, y</span><span class="sxs-lookup"><span data-stu-id="d7bba-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="d7bba-758">para un parámetro de valor o una matriz de parámetros, una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde el argumento al tipo del parámetro correspondiente, o</span><span class="sxs-lookup"><span data-stu-id="d7bba-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="d7bba-759">para un `ref` o `out` parámetro, el tipo del argumento es idéntico al tipo del parámetro correspondiente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="d7bba-760">Después de todo, un `ref` o `out` parámetro es un alias para el argumento pasado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="d7bba-761">Para un miembro de función que incluye una matriz de parámetros, el miembro de función que se apliquen las reglas anteriores, se dice que sean aplicables a su ***forma normal***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="d7bba-762">Si un miembro de función que incluye una matriz de parámetros no es aplicable en su forma normal, el miembro de función puede ser aplicable en su ***expandido formulario***:</span><span class="sxs-lookup"><span data-stu-id="d7bba-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="d7bba-763">La forma expandida se construye mediante la sustitución de la matriz de parámetros en la declaración de miembro de función con cero o más parámetros de valor del tipo de elemento del parámetro de matriz, que el número de argumentos en la lista de argumentos `A` coincide con el total número de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="d7bba-764">Si `A` tiene menos argumentos que el número de parámetros fijos en la declaración de miembro de función, no se puede construir de la forma expandida del miembro de función y, por tanto, no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="d7bba-765">En caso contrario, es aplicable si para cada argumento de la forma expandida `A` el modo de pasar parámetros del argumento es idéntico al modo de pasar parámetros del parámetro correspondiente, y</span><span class="sxs-lookup"><span data-stu-id="d7bba-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="d7bba-766">para un parámetro de valor fijo o un parámetro de valor creados por la expansión, una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde el tipo del argumento al tipo del parámetro correspondiente, o</span><span class="sxs-lookup"><span data-stu-id="d7bba-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="d7bba-767">para un `ref` o `out` parámetro, el tipo del argumento es idéntico al tipo del parámetro correspondiente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="d7bba-768">Mejor miembro de función</span><span class="sxs-lookup"><span data-stu-id="d7bba-768">Better function member</span></span>

<span data-ttu-id="d7bba-769">Para los fines de determinar al mejor miembro de función, una lista de argumentos reducida A se construye que contiene solo las expresiones de argumentos a sí mismos en el orden en que aparecen en la lista de argumentos original.</span><span class="sxs-lookup"><span data-stu-id="d7bba-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="d7bba-770">Listas de parámetros para cada una de las funciones candidatas se construyen los miembros de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7bba-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="d7bba-771">Si el miembro de función se aplica sólo en la forma expandida, se usa la forma expandida.</span><span class="sxs-lookup"><span data-stu-id="d7bba-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="d7bba-772">Parámetros opcionales con ningún argumento correspondiente se quitan de la lista de parámetros</span><span class="sxs-lookup"><span data-stu-id="d7bba-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="d7bba-773">Se puede reordenar los parámetros para que se produzcan en la misma posición que el argumento correspondiente en la lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="d7bba-774">Dada una lista de argumentos `A` con un conjunto de expresiones de argumento `{E1, E2, ..., En}` y dos miembros de función aplicable `Mp` y `Mq` con tipos de parámetro `{P1, P2, ..., Pn}` y `{Q1, Q2, ..., Qn}`, `Mp` se define como un ***mejor miembro de función*** que `Mq` si</span><span class="sxs-lookup"><span data-stu-id="d7bba-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="d7bba-775">para cada argumento, la conversión implícita de `Ex` a `Qx` no es mejor que la conversión implícita de `Ex` a `Px`, y</span><span class="sxs-lookup"><span data-stu-id="d7bba-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="d7bba-776">para al menos un argumento, la conversión de `Ex` a `Px` es mejor que la conversión de `Ex` a `Qx`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="d7bba-777">Al realizar esta evaluación, si `Mp` o `Mq` es aplicable en su forma expandida, a continuación, `Px` o `Qx` hace referencia a un parámetro en la forma expandida de la lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="d7bba-778">En caso de que el parámetro de tipo secuencias `{P1, P2, ..., Pn}` y `{Q1, Q2, ..., Qn}` son equivalentes (es decir, cada uno `Pi` tiene una conversión de identidad correspondiente `Qi`), se aplican las siguientes reglas de desempate, en orden, para determinar la mejor miembro de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="d7bba-779">Si `Mp` es un método no genérico y `Mq` es un método genérico, a continuación, `Mp` es mejor que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="d7bba-780">De lo contrario, si `Mp` es aplicable en su forma normal y `Mq` tiene un `params` de matriz y, a continuación, se aplica sólo en su forma expandida, `Mp` es mejor que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="d7bba-781">De lo contrario, si `Mp` ha declarado más parámetros que `Mq`, a continuación, `Mp` es mejor que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="d7bba-782">Esto puede ocurrir si ambos métodos tienen `params` matrices y se aplica sólo en sus formas expandidas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="d7bba-783">En caso contrario si todos los parámetros de `Mp` tiene un argumento correspondiente, mientras que los argumentos predeterminados deben sustituirse por al menos un parámetro opcional en `Mq` , a continuación, `Mp` es mejor que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="d7bba-784">De lo contrario, si `Mp` tiene tipos de parámetro más específicos que `Mq`, a continuación, `Mp` es mejor que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="d7bba-785">Permiten `{R1, R2, ..., Rn}` y `{S1, S2, ..., Sn}` representan los tipos de parámetros no expandidos y sin instancias de `Mp` y `Mq`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="d7bba-786">`Mp`de tipos de parámetro son más específicos que `Mq`de if, para cada parámetro, `Rx` no es menos específico que `Sx`y, para al menos un parámetro `Rx` es más específico que `Sx`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="d7bba-787">Un parámetro de tipo es menos específico que un parámetro sin tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="d7bba-788">De forma recursiva, un tipo construido es más específico que otro tipo construido (con el mismo número de argumentos de tipo) si al menos un argumento de tipo es más específico y ningún argumento de tipo es menos específico que el argumento de tipo correspondiente en el otro.</span><span class="sxs-lookup"><span data-stu-id="d7bba-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="d7bba-789">Un tipo de matriz es más específico que otro tipo de matriz (con el mismo número de dimensiones) si el tipo de elemento de la primera es más específico que el tipo de elemento del segundo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="d7bba-790">En caso contrario, si un miembro es un operador no elevado y el otro es un operador de elevación, el no elevado es mejor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="d7bba-791">De lo contrario, ningún miembro de función es mejor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="d7bba-792">Mejor conversión de expresión</span><span class="sxs-lookup"><span data-stu-id="d7bba-792">Better conversion from expression</span></span>

<span data-ttu-id="d7bba-793">Dada una conversión implícita `C1` que convierte a partir de una expresión `E` a un tipo `T1`y una conversión implícita `C2` que convierte a partir de una expresión `E` a un tipo `T2`, `C1` es un ***mejor conversión*** que `C2` si `E` coincide exactamente `T2` y suspensiones de al menos uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="d7bba-794">`E` coincide exactamente con `T1` ([que coincidan exactamente con la expresión](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="d7bba-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="d7bba-795">`T1` es un destino de conversión mejor que `T2` ([mejor destino de la conversión](expressions.md#better-conversion-target))</span><span class="sxs-lookup"><span data-stu-id="d7bba-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="d7bba-796">Expresión que coincide exactamente</span><span class="sxs-lookup"><span data-stu-id="d7bba-796">Exactly matching Expression</span></span>

<span data-ttu-id="d7bba-797">Dada una expresión `E` y un tipo `T`, `E` coincide exactamente con `T` si uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="d7bba-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="d7bba-798">`E` tiene un tipo `S`, y existe una conversión de identidad de `S` a `T`</span><span class="sxs-lookup"><span data-stu-id="d7bba-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="d7bba-799">`E` es una función anónima, `T` es un tipo de delegado `D` o un tipo de árbol de expresión `Expression<D>` y suspensiones de uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="d7bba-800">Un tipo de valor devuelto deducido `X` existe para `E` en el contexto de la lista de parámetros `D` ([tipo de valor devuelto deducido](expressions.md#inferred-return-type)), y existe una conversión de identidad de `X` para el tipo de valor devuelto `D`</span><span class="sxs-lookup"><span data-stu-id="d7bba-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="d7bba-801">Cualquier `E` es que no es asincrónico y `D` tiene un tipo de valor devuelto `Y` o `E` es asincrónico y `D` tiene un tipo de valor devuelto `Task<Y>`, y uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="d7bba-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="d7bba-802">El cuerpo de `E` es una expresión que coincida exactamente con `Y`</span><span class="sxs-lookup"><span data-stu-id="d7bba-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="d7bba-803">El cuerpo de `E` es un bloque de instrucciones donde cada instrucción return devuelve una expresión que coincida exactamente con `Y`</span><span class="sxs-lookup"><span data-stu-id="d7bba-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="d7bba-804">Destino de conversión mejor</span><span class="sxs-lookup"><span data-stu-id="d7bba-804">Better conversion target</span></span>

<span data-ttu-id="d7bba-805">Dados dos tipos diferentes `T1` y `T2`, `T1` es un destino de conversión mejor que `T2` si ninguna conversión implícita de `T2` a `T1` existe, y al menos uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="d7bba-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="d7bba-806">Una conversión implícita de `T1` a `T2` existe</span><span class="sxs-lookup"><span data-stu-id="d7bba-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="d7bba-807">`T1` es un tipo de delegado `D1` o un tipo de árbol de expresión `Expression<D1>`, `T2` es un tipo de delegado `D2` o un tipo de árbol de expresión `Expression<D2>`, `D1` tiene un tipo de valor devuelto `S1` y uno de los contiene las siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="d7bba-808">`D2` Devuelve void</span><span class="sxs-lookup"><span data-stu-id="d7bba-808">`D2` is void returning</span></span>
   * <span data-ttu-id="d7bba-809">`D2` tiene un tipo de valor devuelto `S2`, y `S1` es un destino de conversión mejor que `S2`</span><span class="sxs-lookup"><span data-stu-id="d7bba-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="d7bba-810">`T1` es `Task<S1>`, `T2` es `Task<S2>`, y `S1` es un destino de conversión mejor que `S2`</span><span class="sxs-lookup"><span data-stu-id="d7bba-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="d7bba-811">`T1` es `S1` o `S1?` donde `S1` es un tipo entero con signo, y `T2` es `S2` o `S2?` donde `S2` es un tipo entero sin signo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="d7bba-812">De manera específica:</span><span class="sxs-lookup"><span data-stu-id="d7bba-812">Specifically:</span></span>
   * <span data-ttu-id="d7bba-813">`S1` es `sbyte` y `S2` es `byte`, `ushort`, `uint`, o `ulong`</span><span class="sxs-lookup"><span data-stu-id="d7bba-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="d7bba-814">`S1` es `short` y `S2` es `ushort`, `uint`, o `ulong`</span><span class="sxs-lookup"><span data-stu-id="d7bba-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="d7bba-815">`S1` es `int` y `S2` es `uint`, o `ulong`</span><span class="sxs-lookup"><span data-stu-id="d7bba-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="d7bba-816">`S1` es `long` y `S2` es `ulong`</span><span class="sxs-lookup"><span data-stu-id="d7bba-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="d7bba-817">Sobrecarga en clases genéricas</span><span class="sxs-lookup"><span data-stu-id="d7bba-817">Overloading in generic classes</span></span>

<span data-ttu-id="d7bba-818">Mientras que las firmas declaradas deben ser únicas, es posible que la sustitución de argumentos de tipo da lugar a firmas idénticas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="d7bba-819">En tales casos, las reglas de desempate de la resolución de sobrecarga anterior seleccionará al miembro más específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="d7bba-820">Los ejemplos siguientes muestran las sobrecargas que son válidos y no válidos según esta regla:</span><span class="sxs-lookup"><span data-stu-id="d7bba-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="d7bba-821">Comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación</span><span class="sxs-lookup"><span data-stu-id="d7bba-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="d7bba-822">Para las operaciones enlazadas más dinámicamente el conjunto de posibles candidatos para la resolución es desconocido en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="d7bba-823">Sin embargo en algunos casos, el conjunto de candidatos se conoce en tiempo de compilación:</span><span class="sxs-lookup"><span data-stu-id="d7bba-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="d7bba-824">Llamadas a métodos estáticos con argumentos dinámicos</span><span class="sxs-lookup"><span data-stu-id="d7bba-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="d7bba-825">Llamadas de método de instancia donde el receptor no es una expresión dinámica</span><span class="sxs-lookup"><span data-stu-id="d7bba-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="d7bba-826">Llamadas de indizador donde el receptor no es una expresión dinámica</span><span class="sxs-lookup"><span data-stu-id="d7bba-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="d7bba-827">Llamadas de constructor con argumentos dinámicos</span><span class="sxs-lookup"><span data-stu-id="d7bba-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="d7bba-828">En estos casos se realiza una comprobación de tiempo de compilación limitada para que cada candidato ver si alguno de ellos, posiblemente, podría aplicar en tiempo de ejecución. Esta comprobación se compone de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="d7bba-829">Inferencia de tipo parcial: Cualquier tipo de argumento que no dependen directa o indirectamente un argumento de tipo `dynamic` se deduce usando las reglas de [inferencia de tipo](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="d7bba-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="d7bba-830">Los argumentos de tipo restantes son desconocidos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="d7bba-831">Comprobación de aplicabilidad parcial: Se comprueba la aplicabilidad según [miembro de función aplicable](expressions.md#applicable-function-member), pero se omiten los parámetros cuyos tipos son desconocidas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="d7bba-832">Si ningún candidato pasa esta prueba, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="d7bba-833">Invocación de función miembro</span><span class="sxs-lookup"><span data-stu-id="d7bba-833">Function member invocation</span></span>

<span data-ttu-id="d7bba-834">En esta sección se describe el proceso que tiene lugar en tiempo de ejecución para invocar a un miembro de función determinada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="d7bba-835">Se supone que un proceso en tiempo de enlace ya ha determinado el miembro que se invoca, posiblemente mediante la aplicación a un conjunto de miembros de función candidatos de resolución de sobrecarga determinado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="d7bba-836">Para fines de describir el proceso de invocación, los miembros de función se dividen en dos categorías:</span><span class="sxs-lookup"><span data-stu-id="d7bba-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="d7bba-837">Miembros de la función estática.</span><span class="sxs-lookup"><span data-stu-id="d7bba-837">Static function members.</span></span> <span data-ttu-id="d7bba-838">Estos son los constructores de instancias, los métodos estáticos, los descriptores de acceso de propiedad estática y los operadores definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="d7bba-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="d7bba-839">Miembros de función estáticos siempre son no virtuales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="d7bba-840">Miembros de función de instancia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-840">Instance function members.</span></span> <span data-ttu-id="d7bba-841">Estos son los métodos de instancia, los descriptores de acceso de propiedad de instancia y descriptores de acceso de indizador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="d7bba-842">Miembros de función de instancia son no virtuales o virtual y siempre se invocan en una instancia determinada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="d7bba-843">La instancia se calcula mediante una expresión de instancia, y se convierte en accesible dentro del miembro de función como `this` ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="d7bba-844">El procesamiento en tiempo de ejecución de una invocación de miembros de función consta de los pasos siguientes, donde `M` es el miembro de función y, si `M` es un miembro de instancia, `E` es la expresión de instancia:</span><span class="sxs-lookup"><span data-stu-id="d7bba-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="d7bba-845">Si `M` es un miembro de función estático:</span><span class="sxs-lookup"><span data-stu-id="d7bba-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="d7bba-846">La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="d7bba-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="d7bba-847">`M` se invoca.</span><span class="sxs-lookup"><span data-stu-id="d7bba-847">`M` is invoked.</span></span>

*  <span data-ttu-id="d7bba-848">Si `M` se declara un miembro de función de la instancia en un *value_type*:</span><span class="sxs-lookup"><span data-stu-id="d7bba-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="d7bba-849">`E` se evalúa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-849">`E` is evaluated.</span></span> <span data-ttu-id="d7bba-850">Si esta evaluación provoca una excepción, no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="d7bba-851">Si `E` no está clasificado como una variable y, a continuación, una variable local temporal de `E`del tipo se crea y el valor de `E` se asigna a esa variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="d7bba-852">`E` a continuación, se reclasifica como una referencia a la variable local temporal.</span><span class="sxs-lookup"><span data-stu-id="d7bba-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="d7bba-853">La variable temporal es accesible como `this` en `M`, pero no en ninguna otra forma.</span><span class="sxs-lookup"><span data-stu-id="d7bba-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="d7bba-854">Por lo tanto, solo cuando `E` es una variable true permite al llamador observar los cambios que `M` hace `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="d7bba-855">La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="d7bba-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="d7bba-856">`M` se invoca.</span><span class="sxs-lookup"><span data-stu-id="d7bba-856">`M` is invoked.</span></span> <span data-ttu-id="d7bba-857">La variable al que hace referencia `E` se convierte en la variable al que hace referencia `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="d7bba-858">Si `M` se declara un miembro de función de la instancia en un *reference_type*:</span><span class="sxs-lookup"><span data-stu-id="d7bba-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="d7bba-859">`E` se evalúa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-859">`E` is evaluated.</span></span> <span data-ttu-id="d7bba-860">Si esta evaluación provoca una excepción, no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="d7bba-861">La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="d7bba-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="d7bba-862">Si el tipo de `E` es un *value_type*, una conversión boxing ([conversiones Boxing](types.md#boxing-conversions)) se realiza para convertir `E` escriba `object`, y `E` se considera para ser de tipo `object` en los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="d7bba-863">En este caso, `M` sólo podría ser un miembro de `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="d7bba-864">El valor de `E` se comprueba para ser válido.</span><span class="sxs-lookup"><span data-stu-id="d7bba-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="d7bba-865">Si el valor de `E` es `null`, un `System.NullReferenceException` se inicia y no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="d7bba-866">Se determina la implementación del miembro de función para invocar:</span><span class="sxs-lookup"><span data-stu-id="d7bba-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="d7bba-867">Si el enlace de tipo en tiempo de `E` es una interfaz, el miembro de función para invocar es la implementación de `M` proporcionada por el tipo de tiempo de ejecución de la instancia que hace referencia `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="d7bba-868">Este miembro de función se determina aplicando las reglas de asignación de interfaz ([asignación de interfaz](interfaces.md#interface-mapping)) para determinar la implementación de `M` proporcionada por el tipo de tiempo de ejecución de la instancia que hace referencia `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="d7bba-869">De lo contrario, si `M` es un miembro de función virtual, el miembro de función para invocar es la implementación de `M` proporcionada por el tipo de tiempo de ejecución de la instancia que hace referencia `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="d7bba-870">Este miembro de función se determina aplicando las reglas para determinar la implementación más derivada ([métodos virtuales](classes.md#virtual-methods)) de `M` en relación con el tipo de tiempo de ejecución de la instancia que hace referencia `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="d7bba-871">En caso contrario, `M` es un miembro de función no virtual, y es el miembro de función para invocar `M` propio.</span><span class="sxs-lookup"><span data-stu-id="d7bba-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="d7bba-872">Se invoca la implementación del miembro de función determinada en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="d7bba-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="d7bba-873">El objeto al que hace referencia `E` se convierte en el objeto al que hace referencia `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="d7bba-874">Invocaciones en instancias de conversión boxing</span><span class="sxs-lookup"><span data-stu-id="d7bba-874">Invocations on boxed instances</span></span>

<span data-ttu-id="d7bba-875">Un miembro de función se implementa en un *value_type* pueden invocarse a través de una instancia con conversión boxing de los que *value_type* en las situaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="d7bba-876">Cuando el miembro de función es un `override` de un método heredado de tipo `object` y se invoca a través de una expresión de instancia de tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="d7bba-877">Cuando el miembro de función es una implementación de un miembro de función de la interfaz y se invoca a través de una expresión de instancia de un *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="d7bba-878">Cuando se invoca el miembro de función a través de un delegado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="d7bba-879">En estas situaciones, se considera la instancia de la conversión boxing para contener una variable de la *value_type*, y esta variable se convierte en la variable al que hace referencia `this` dentro de la invocación de miembros de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="d7bba-880">En concreto, esto significa que cuando un miembro de función se invoca en una instancia de la conversión boxing, es posible que el miembro de función modificar el valor contenido en la instancia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="d7bba-881">Expresiones primarias</span><span class="sxs-lookup"><span data-stu-id="d7bba-881">Primary expressions</span></span>

<span data-ttu-id="d7bba-882">Expresiones primarias incluyen las formas más sencillas de expresiones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-882">Primary expressions include the simplest forms of expressions.</span></span>

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

<span data-ttu-id="d7bba-883">Expresiones primarias se dividen entre *array_creation_expression*s y *primary_no_array_creation_expression*s.</span><span class="sxs-lookup"><span data-stu-id="d7bba-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="d7bba-884">Tratamiento de expresión de creación de matriz de esta manera, en lugar de enumerarlas junto con las otras formas de expresión simple, permite que la gramática para no permitir código potencialmente confuso, como</span><span class="sxs-lookup"><span data-stu-id="d7bba-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="d7bba-885">lo que de lo contrario se interpretaría como</span><span class="sxs-lookup"><span data-stu-id="d7bba-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="d7bba-886">Literales</span><span class="sxs-lookup"><span data-stu-id="d7bba-886">Literals</span></span>

<span data-ttu-id="d7bba-887">Un *primary_expression* que consta de un *literal* ([literales](lexical-structure.md#literals)) se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="d7bba-888">Cadenas interpoladas</span><span class="sxs-lookup"><span data-stu-id="d7bba-888">Interpolated strings</span></span>

<span data-ttu-id="d7bba-889">Un *interpolated_string_expression* consta de un `$` signo seguida de una cadena literal, normal o textual en agujeros, delimitadas por `{` y `}`, incluya expresiones y formato especificaciones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="d7bba-890">Una expresión de cadena interpolada es el resultado de una *interpolated_string_literal* que se ha dividido en tokens individuales, como se describe en [interpoladas literales de cadena](lexical-structure.md#interpolated-string-literals).</span><span class="sxs-lookup"><span data-stu-id="d7bba-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

<span data-ttu-id="d7bba-891">El *constant_expression* en una interpolación debe tener una conversión implícita a `int`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="d7bba-892">Un *interpolated_string_expression* se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="d7bba-893">Si se convierte inmediatamente en `System.IFormattable` o `System.FormattableString` con una conversión implícita de cadena interpolada ([implícitas interpolan conversiones de cadenas](conversions.md#implicit-interpolated-string-conversions)), la expresión de cadena interpolada tiene ese tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="d7bba-894">En caso contrario, tiene el tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="d7bba-895">Si el tipo de una cadena interpolada es `System.IFormattable` o `System.FormattableString`, el significado es una llamada a `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="d7bba-896">Si el tipo es `string`, el significado de la expresión es una llamada a `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="d7bba-897">En ambos casos, la lista de argumentos de la llamada consta de una cadena de formato literal con marcadores de posición para cada interpolación y un argumento para cada expresión correspondiente a los marcadores de posición.</span><span class="sxs-lookup"><span data-stu-id="d7bba-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="d7bba-898">El literal de cadena de formato se construye como sigue, donde `N` es el número de interpolaciones en el *interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="d7bba-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="d7bba-899">Si un *interpolated_regular_string_whole* o un *interpolated_verbatim_string_whole* sigue el `$` iniciar sesión, a continuación, el literal de cadena de formato es ese token.</span><span class="sxs-lookup"><span data-stu-id="d7bba-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="d7bba-900">En caso contrario, el literal de cadena de formato consta de:</span><span class="sxs-lookup"><span data-stu-id="d7bba-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="d7bba-901">Primera la *interpolated_regular_string_start* o *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="d7bba-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="d7bba-902">A continuación, para cada número `I` desde `0` a `N-1`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="d7bba-903">La representación decimal de `I`</span><span class="sxs-lookup"><span data-stu-id="d7bba-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="d7bba-904">A continuación, si el correspondiente *interpolación* tiene un *constant_expression*, un `,` (coma) seguido de la representación decimal del valor de la *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="d7bba-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="d7bba-905">El *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* o *interpolated_ verbatim_string_end* inmediatamente después de la interpolación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="d7bba-906">Los argumentos subsiguientes son simplemente el *expresiones* desde el *interpolaciones* (si existe), en orden.</span><span class="sxs-lookup"><span data-stu-id="d7bba-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="d7bba-907">TODO: ejemplos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="d7bba-908">Nombres simples</span><span class="sxs-lookup"><span data-stu-id="d7bba-908">Simple names</span></span>

<span data-ttu-id="d7bba-909">Un *simple_name* consta de un identificador, seguido opcionalmente por una lista de argumentos de tipo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="d7bba-910">Un *simple_name* sea del formulario `I` o tiene el formato `I<A1,...,Ak>`, donde `I` es un identificador único y `<A1,...,Ak>` es una función opcional *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="d7bba-911">Cuando no hay ninguna *type_argument_list* está especificado, considere la posibilidad de `K` sea cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="d7bba-912">El *simple_name* se evalúa y clasifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="d7bba-913">Si `K` es cero y el *simple_name* aparece dentro de un *bloque* y si el *bloque*de (o envolvente *bloque*del) local espacio de declaración de variable ([declaraciones](basic-concepts.md#declarations)) contiene una variable local, parámetro o constante con nombre `I`, el *simple_name* hace referencia a la variable local, parámetro o una constante y se clasifica como una variable o un valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="d7bba-914">Si `K` es cero y el *simple_name* aparece dentro del cuerpo de una declaración de método genérico y si dicha declaración incluye un parámetro de tipo con nombre `I`, el *simple_name*hace referencia a ese parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="d7bba-915">En caso contrario, para cada tipo de instancia `T` ([el tipo de instancia](classes.md#the-instance-type)), empezando por el tipo de instancia de la declaración de tipo envolvente inmediato y continuando con el tipo de instancia de cada clase o estructura envolvente declaración (si existe):</span><span class="sxs-lookup"><span data-stu-id="d7bba-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="d7bba-916">Si `K` es cero y la declaración de `T` incluye un parámetro de tipo con nombre `I`, el *simple_name* hace referencia a ese parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="d7bba-917">En caso contrario, si una búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)) de `I` en `T` con `K`  argumentos de tipo produce una coincidencia:</span><span class="sxs-lookup"><span data-stu-id="d7bba-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="d7bba-918">Si `T` es el tipo de instancia del tipo de clase o estructura envolvente inmediata y la búsqueda identifica uno o más métodos, el resultado es un grupo de métodos con una expresión de instancia asociada de `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="d7bba-919">Si se ha especificado una lista de argumentos de tipo, se utiliza en una llamada a un método genérico ([las invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="d7bba-920">De lo contrario, si `T` es el tipo de instancia del tipo struct o clase inmediatamente envolvente, si la búsqueda identifica un miembro de instancia, y si se produce la referencia dentro del cuerpo de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia, el resultado es el mismo que un acceso a miembros ([acceso a miembros](expressions.md#member-access)) del formulario `this.I`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="d7bba-921">Esto solo puede ocurrir cuando `K` es cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="d7bba-922">En caso contrario, el resultado es el mismo que un acceso a miembros ([acceso a miembros](expressions.md#member-access)) del formulario `T.I` o `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="d7bba-923">En este caso, es un error en tiempo de enlace para el *simple_name* para hacer referencia a un miembro de instancia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="d7bba-924">En caso contrario, para cada espacio de nombres `N`, empezando por el espacio de nombres en el que el *simple_name* se produce, continúe con cada uno de los espacios de nombres (si existe) envolventes y terminando con el espacio de nombres global, son los pasos siguientes evalúa hasta que se encuentra una entidad:</span><span class="sxs-lookup"><span data-stu-id="d7bba-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="d7bba-925">Si `K` es cero y `I` es el nombre de un espacio de nombres en `N`, a continuación:</span><span class="sxs-lookup"><span data-stu-id="d7bba-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="d7bba-926">Si la ubicación donde el *simple_name* se produce entre una declaración de espacio de nombres para `N` y la declaración de espacio de nombres contiene un *extern_alias_directive* o  *using_alias_directive* que asocia el nombre `I` con un espacio de nombres o tipo, el *simple_name* es ambiguo y se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="d7bba-927">En caso contrario, el *simple_name* hace referencia al espacio de nombres denominado `I` en `N`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="d7bba-928">De lo contrario, si `N` contiene un tipo accesible con nombre `I` y `K`  parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="d7bba-929">Si `K` es cero y la ubicación donde el *simple_name* se produce entre una declaración de espacio de nombres para `N` y la declaración de espacio de nombres contiene un *extern_alias_directive*o *using_alias_directive* que asocia el nombre `I` con un espacio de nombres o tipo, el *simple_name* es ambiguo y se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="d7bba-930">En caso contrario, el *namespace_or_type_name* hace referencia al tipo construido con los argumentos de tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="d7bba-931">De lo contrario, si la ubicación donde el *simple_name* se produce entre una declaración de espacio de nombres para `N`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="d7bba-932">Si `K` es cero y la declaración de espacio de nombres contiene un *extern_alias_directive* o *using_alias_directive* que asocia el nombre `I` con un espacio de nombres importado o tipo, el *simple_name* hace referencia a dicho espacio de nombres o tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="d7bba-933">En caso contrario, si las declaraciones de espacios de nombres y el tipo importan por el *using_namespace_directive*s y *using_static_directive*s de la declaración de espacio de nombres contiene exactamente un tipo accesible o con el nombre de miembro estático sin extensión `I` y `K`  parámetros de tipo, el *simple_name* hace referencia a ese tipo o miembro construido con los argumentos de tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="d7bba-934">En caso contrario, si los tipos y espacios de nombres importan por el *using_namespace_directive*s de la declaración de espacio de nombres contiene más de un tipo accesible o método de extensión que no son miembro estático con el nombre `I` y `K`  parámetros de tipo, el *simple_name* es ambiguo y se produce un error.</span><span class="sxs-lookup"><span data-stu-id="d7bba-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="d7bba-935">Tenga en cuenta que este paso es paralelo exactamente con el paso correspondiente en el procesamiento de un *namespace_or_type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="d7bba-936">En caso contrario, el *simple_name* es no definido y se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="d7bba-937">Expresiones entre paréntesis</span><span class="sxs-lookup"><span data-stu-id="d7bba-937">Parenthesized expressions</span></span>

<span data-ttu-id="d7bba-938">Un *parenthesized_expression* consta de un *expresión* entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="d7bba-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="d7bba-939">Un *parenthesized_expression* se evalúa mediante la evaluación de la *expresión* entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="d7bba-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="d7bba-940">Si el *expresión* entre paréntesis denota un espacio de nombres o tipo, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="d7bba-941">En caso contrario, el resultado de la *parenthesized_expression* es el resultado de la evaluación de los contenidos *expresión*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="d7bba-942">Acceso a miembros</span><span class="sxs-lookup"><span data-stu-id="d7bba-942">Member access</span></span>

<span data-ttu-id="d7bba-943">Un *member_access* consta de un *primary_expression*, un *predefined_type*, o un *qualified_alias_member*, seguido de un"`.`"símbolo (token), seguido de un *identificador*, seguido opcionalmente un *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

<span data-ttu-id="d7bba-944">El *qualified_alias_member* producción se define en [calificadores de alias Namespace](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="d7bba-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="d7bba-945">Un *member_access* sea del formulario `E.I` o tiene el formato `E.I<A1, ..., Ak>`, donde `E` es una expresión primaria, `I` es un identificador único y `<A1, ..., Ak>` es una función opcional  *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="d7bba-946">Cuando no hay ninguna *type_argument_list* está especificado, considere la posibilidad de `K` sea cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="d7bba-947">Un *member_access* con un *primary_expression* typu `dynamic` se enlazan dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="d7bba-948">En este caso el compilador clasifica el acceso a miembros como acceso a una propiedad de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="d7bba-949">Las reglas siguientes para determinar el significado de la *member_access* , a continuación, se aplican en tiempo de ejecución, mediante el tipo de tiempo de ejecución en lugar del tipo de tiempo de compilación de la *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="d7bba-950">Si esta clasificación de tiempo de ejecución da lugar a un grupo de métodos, el acceso a miembros debe ser el *primary_expression* de un *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="d7bba-951">El *member_access* se evalúa y clasifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="d7bba-952">Si `K` es cero y `E` es un espacio de nombres y `E` contiene un espacio de nombres anidado con nombre `I`, el resultado será ese espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="d7bba-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="d7bba-953">De lo contrario, si `E` es un espacio de nombres y `E` contiene un tipo accesible con nombre `I` y `K`  parámetros de tipo, el resultado será ese tipo construido con los argumentos de tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="d7bba-954">Si `E` es un *predefined_type* o un *primary_expression* clasificada como un tipo, si `E` no es un parámetro de tipo y si una búsqueda de miembros ([debúsquedademiembros](expressions.md#member-lookup)) de `I` en `E` con `K`  parámetros de tipo produce una coincidencia, a continuación, `E.I` se evalúa y clasifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="d7bba-955">Si `I` identifica un tipo, el resultado será ese tipo construido con los argumentos de tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="d7bba-956">Si `I` identifica uno o varios métodos, entonces el resultado es un grupo de métodos sin expresión de instancia asociada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="d7bba-957">Si se ha especificado una lista de argumentos de tipo, se utiliza en una llamada a un método genérico ([las invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="d7bba-958">Si `I` identifica un `static` propiedad, el resultado es un acceso de propiedad sin expresión de instancia asociada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="d7bba-959">Si `I` identifica un `static` campo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="d7bba-960">Si el campo es `readonly` y la referencia se produce fuera del constructor estático de la clase o estructura en la que se declara el campo, entonces el resultado es un valor, es decir, el valor del campo estático `I` en `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="d7bba-961">En caso contrario, el resultado es una variable, es decir, el campo estático `I` en `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="d7bba-962">Si `I` identifica un `static` eventos:</span><span class="sxs-lookup"><span data-stu-id="d7bba-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="d7bba-963">Si la referencia se produce dentro de la clase o estructura en el que se declara el evento y el evento se ha declarado sin *event_accessor_declarations* ([eventos](classes.md#events)), a continuación, `E.I` se procesa exactamente como si `I` fuera un campo estático.</span><span class="sxs-lookup"><span data-stu-id="d7bba-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="d7bba-964">En caso contrario, el resultado es un acceso de eventos sin expresión de instancia asociada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="d7bba-965">Si `I` identifica una constante, entonces el resultado es un valor, es decir, el valor de la constante.</span><span class="sxs-lookup"><span data-stu-id="d7bba-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="d7bba-966">Si `I` identifica un miembro de enumeración, entonces el resultado es un valor, es decir, el valor de ese miembro de enumeración.</span><span class="sxs-lookup"><span data-stu-id="d7bba-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="d7bba-967">En caso contrario, `E.I` es una referencia de miembro no válido, y se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-968">Si `E` es acceso a la propiedad, indizador, variable o valor, el tipo de los cuales es `T`y una búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)) de `I` en `T` con `K`  argumentos de tipo produce una coincidencia, a continuación, `E.I` se evalúa y clasifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="d7bba-969">Primero, if `E` es una propiedad o indizador, el valor de la propiedad o se obtiene acceso al indizador ([valores de expresiones](expressions.md#values-of-expressions)) y `E` es reclasificar como un valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="d7bba-970">Si `I` identifica uno o varios métodos, entonces el resultado es un grupo de métodos con una expresión de instancia asociada de `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="d7bba-971">Si se ha especificado una lista de argumentos de tipo, se utiliza en una llamada a un método genérico ([las invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="d7bba-972">Si `I` identifica una propiedad de instancia</span><span class="sxs-lookup"><span data-stu-id="d7bba-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="d7bba-973">Si `E` es `this`, `I` identifica una propiedad implementada automáticamente ([implementa automáticamente propiedades](classes.md#automatically-implemented-properties)) sin un establecedor y la referencia ocurre dentro de un constructor de instancia para un tipo de clase o struct `T`, entonces el resultado es una variable, es decir, el campo de respaldo oculto para la propiedad automática proporcionada por `I` en la instancia de `T` proporcionado por `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="d7bba-974">En caso contrario, el resultado es un acceso de propiedad con una expresión de instancia asociada de `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="d7bba-975">Si `T` es un *class_type* y `I` identifica un campo de instancia de ese *class_type*:</span><span class="sxs-lookup"><span data-stu-id="d7bba-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="d7bba-976">Si el valor de `E` es `null`, un `System.NullReferenceException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="d7bba-977">En caso contrario, si el campo es `readonly` y la referencia se produce fuera de un constructor de instancia de la clase en el que se declara el campo, entonces el resultado es un valor, es decir, el valor del campo `I` en el objeto al que hace referencia `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="d7bba-978">En caso contrario, el resultado es una variable, es decir, el campo `I` en el objeto al que hace referencia `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="d7bba-979">Si `T` es un *struct_type* y `I` identifica un campo de instancia de ese *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="d7bba-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="d7bba-980">Si `E` es un valor, o si el campo es `readonly` y la referencia se produce fuera de un constructor de instancia de la estructura en el que se declara el campo, entonces el resultado es un valor, es decir, el valor del campo `I` en la instancia de estructura proporcionada por  `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="d7bba-981">En caso contrario, el resultado es una variable, es decir, el campo `I` en la instancia de estructura proporcionada por `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="d7bba-982">Si `I` identifica un evento de instancia:</span><span class="sxs-lookup"><span data-stu-id="d7bba-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="d7bba-983">Si la referencia se produce dentro de la clase o estructura en el que se declara el evento y el evento se ha declarado sin *event_accessor_declarations* ([eventos](classes.md#events)), y la referencia no se realizan como la lado izquierdo de un `+=` o `-=` operador y, a continuación, `E.I` se procesa exactamente como si `I` era un campo de instancia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="d7bba-984">En caso contrario, el resultado es un acceso de eventos con una expresión de instancia asociada de `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="d7bba-985">En caso contrario, se realiza un intento para procesar `E.I` como una invocación de método de extensión ([las invocaciones de método de extensión](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="d7bba-986">Si se produce un error, `E.I` es una referencia de miembro no válido, y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="d7bba-987">Idénticos nombres simples y nombres de tipo</span><span class="sxs-lookup"><span data-stu-id="d7bba-987">Identical simple names and type names</span></span>

<span data-ttu-id="d7bba-988">En un acceso de miembro del formulario `E.I`si `E` es un identificador único y si el significado de `E` como un *simple_name* ([nombres simples](expressions.md#simple-names)) es una constante, campo, propiedad, variable local o un parámetro con el mismo tipo que el significado de `E` como un *type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)), a continuación, dos significados posibles de `E` son permitido.</span><span class="sxs-lookup"><span data-stu-id="d7bba-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="d7bba-989">Los dos significados posibles de `E.I` nunca son ambiguos, desde `I` necesariamente debe ser miembro del tipo `E` en ambos casos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="d7bba-990">En otras palabras, la regla sencillamente permite el acceso a los miembros estáticos y los tipos anidados de `E` en tiempo de compilación en caso contrario, habría producido un error.</span><span class="sxs-lookup"><span data-stu-id="d7bba-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="d7bba-991">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-991">For example:</span></span>
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a><span data-ttu-id="d7bba-992">Ambigüedades de gramática</span><span class="sxs-lookup"><span data-stu-id="d7bba-992">Grammar ambiguities</span></span>

<span data-ttu-id="d7bba-993">Las producciones de *simple_name* ([nombres simples](expressions.md#simple-names)) y *member_access* ([acceso a miembros](expressions.md#member-access)) puede dar lugar a ambigüedades en la gramática de expresiones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="d7bba-994">Por ejemplo, la instrucción:</span><span class="sxs-lookup"><span data-stu-id="d7bba-994">For example, the statement:</span></span>
```
F(G<A,B>(7));
```
<span data-ttu-id="d7bba-995">se puede interpretar como una llamada a `F` con dos argumentos, `G < A` y `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="d7bba-996">Como alternativa, podría interpretarse como una llamada a `F` con un argumento, que es una llamada a un método genérico `G` con dos argumentos de tipo y un argumento normal.</span><span class="sxs-lookup"><span data-stu-id="d7bba-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="d7bba-997">Si se puede analizar una secuencia de tokens (en contexto) como un *simple_name* ([nombres simples](expressions.md#simple-names)), *member_access* ([acceso a miembros](expressions.md#member-access)), o *pointer_member_access* ([acceso a miembros de puntero](unsafe-code.md#pointer-member-access)) termina con un *type_argument_list* ([argumentos de tipo](types.md#type-arguments)), el símbolo (token) inmediatamente después del cierre `>` se examina el token.</span><span class="sxs-lookup"><span data-stu-id="d7bba-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="d7bba-998">Si es uno de</span><span class="sxs-lookup"><span data-stu-id="d7bba-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="d7bba-999">el *type_argument_list* se conserva como parte de la *simple_name*, *member_access* o *pointer_member_access* y cualquier otro posible análisis de la secuencia de tokens se descartan.</span><span class="sxs-lookup"><span data-stu-id="d7bba-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="d7bba-1000">En caso contrario, el *type_argument_list* no se considera parte de la *simple_name*, *member_access* o *pointer_member_access*, incluso si no hay ningún otro análisis posible de la secuencia de tokens.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="d7bba-1001">Tenga en cuenta que no son de estas reglas se aplica al analizar un *type_argument_list* en un *namespace_or_type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="d7bba-1002">La instrucción</span><span class="sxs-lookup"><span data-stu-id="d7bba-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="d7bba-1003">según esta regla, se interpretará como una llamada a `F` con un argumento, que es una llamada a un método genérico `G` con dos argumentos de tipo y un argumento normal.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="d7bba-1004">Las instrucciones</span><span class="sxs-lookup"><span data-stu-id="d7bba-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="d7bba-1005">cada uno se interpretará como una llamada a `F` con dos argumentos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="d7bba-1006">La instrucción</span><span class="sxs-lookup"><span data-stu-id="d7bba-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="d7bba-1007">se interpretará como un operador menor que, mayor que el operador y operador unario, como si hubiese escrita la instrucción `x = (F < A) > (+y)`, en lugar de como un *simple_name* con un *type_argument_list* seguido de un operador más binario.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="d7bba-1008">En la instrucción</span><span class="sxs-lookup"><span data-stu-id="d7bba-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="d7bba-1009">los tokens `C<T>` se interpretan como un *namespace_or_type_name* con un *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="d7bba-1010">Expresiones de invocación</span><span class="sxs-lookup"><span data-stu-id="d7bba-1010">Invocation expressions</span></span>

<span data-ttu-id="d7bba-1011">Un *invocation_expression* se utiliza para invocar un método.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="d7bba-1012">Un *invocation_expression* se enlazan dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) si la contiene al menos uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="d7bba-1013">El *primary_expression* tiene el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="d7bba-1014">Al menos un argumento de opcional *argument_list* tiene el tipo de tiempo de compilación `dynamic` y *primary_expression* no tiene un tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="d7bba-1015">En este caso el compilador clasifica el *invocation_expression* como un valor de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="d7bba-1016">Las reglas siguientes para determinar el significado de la *invocation_expression* , a continuación, se aplican en tiempo de ejecución, mediante el tipo de tiempo de ejecución en lugar del tipo de tiempo de compilación de los de la *primary_expression* y argumentos que tienen el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="d7bba-1017">Si el *primary_expression* no tiene el tipo de tiempo de compilación `dynamic`, a continuación, la invocación del método se somete a una comprobación en tiempo de compilación limitada según se describe en [comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="d7bba-1018">El *primary_expression* de un *invocation_expression* debe ser un grupo de métodos o un valor de un *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="d7bba-1019">Si el *primary_expression* es un grupo de métodos, el *invocation_expression* es una invocación de método ([las invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="d7bba-1020">Si el *primary_expression* es un valor de un *delegate_type*, *invocation_expression* es una invocación de delegado ([invocacionesdedelegado](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="d7bba-1021">Si el *primary_expression* no es un grupo de métodos ni un valor de un *delegate_type*, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-1022">El elemento opcional *argument_list* ([listas de argumentos](expressions.md#argument-lists)) proporciona valores o referencias de variables para los parámetros del método.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="d7bba-1023">El resultado de evaluar una *invocation_expression* se clasifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="d7bba-1024">Si el *invocation_expression* invoca un método o delegado que devuelve `void`, el resultado es nothing.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="d7bba-1025">Una expresión que se clasifica como nada sólo se permite en el contexto de un *statement_expression* ([las instrucciones de expresión](statements.md#expression-statements)) o como el cuerpo de un *lambda_expression*([Expresiones de función anónima](expressions.md#anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="d7bba-1026">En caso contrario, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-1027">En caso contrario, el resultado es un valor del tipo devuelto por el método o delegado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="d7bba-1028">Invocaciones de método</span><span class="sxs-lookup"><span data-stu-id="d7bba-1028">Method invocations</span></span>

<span data-ttu-id="d7bba-1029">Para una invocación de método, el *primary_expression* de la *invocation_expression* debe ser un grupo de métodos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="d7bba-1030">El grupo de métodos identifica el método que se invoca o el conjunto de métodos sobrecargados para elegir un método específico para invocar.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="d7bba-1031">En el último caso, la determinación del método concreto que se invoca se basa en el contexto proporcionado por los tipos de los argumentos de la *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="d7bba-1032">El procesamiento en tiempo de enlace de una invocación de método del formulario `M(A)`, donde `M` es un grupo de métodos (incluidos posiblemente un *type_argument_list*), y `A` es una función opcional *argument_ lista*, consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="d7bba-1033">Se construye el conjunto de métodos de candidatos para la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="d7bba-1034">Para cada método `F` asociado con el grupo de métodos `M`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="d7bba-1035">Si `F` es genérica, `F` es un candidato cuando:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="d7bba-1036">`M` no tiene ninguna lista de argumentos de tipo, y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="d7bba-1037">`F` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="d7bba-1038">Si `F` es genérico y `M` no tiene ninguna lista de argumentos de tipo `F` es un candidato cuando:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="d7bba-1039">Inferencia de tipos ([inferencia](expressions.md#type-inference)) se realiza correctamente, deduciendo una lista de argumentos de tipo para la llamada, y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="d7bba-1040">Una vez que los argumentos de tipo deducido se sustituyen por los parámetros de tipo del método correspondiente, todos los tipos construidos en la lista de parámetros de F cumplen sus restricciones ([que satisfacen las restricciones](types.md#satisfying-constraints)) y la lista de parámetros de `F` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="d7bba-1041">Si `F` es genérico y `M` incluye una lista de argumentos de tipo `F` es un candidato cuando:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="d7bba-1042">`F` tiene el mismo número de parámetros de tipo de método que se proporcionaron en la lista de argumentos de tipo, y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="d7bba-1043">Una vez que los argumentos de tipo se sustituyen por los parámetros de tipo del método correspondiente, todos los tipos construidos en la lista de parámetros de F cumplen sus restricciones ([que satisfacen las restricciones](types.md#satisfying-constraints)) y la lista de parámetros `F` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="d7bba-1044">El conjunto de métodos candidatos se reduce para que contenga solo los métodos de los tipos más derivados: Para cada método `C.F` en el conjunto, donde `C` es el tipo en el que el método `F` se declara, todos los métodos declaran en un tipo base de `C` se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="d7bba-1045">Además, si `C` es distinto de un tipo de clase `object`, se quitan todos los métodos declarados en un tipo de interfaz del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="d7bba-1046">(Esta última regla sólo tiene efecto cuando el grupo de métodos fue el resultado de una búsqueda de miembros en un parámetro de tipo que tiene una clase base efectiva que no sean de objeto y un conjunto de interfaces efectivas vacío).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="d7bba-1047">Si el conjunto de métodos candidatos resultante está vacío, se abandona el procesamiento posterior a lo largo de los pasos siguientes y, en su lugar, se realiza un intento para procesar la invocación de una invocación de método de extensión ([invocaciones de método de extensión](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="d7bba-1048">Si se produce un error, a continuación, no existen métodos aplicables, y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-1049">El mejor método para el conjunto de métodos candidatos se identifica mediante las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="d7bba-1050">Si no se puede identificar un método mejor, la invocación del método es ambigua y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="d7bba-1051">Al realizar la resolución de sobrecarga, se consideran los parámetros de un método genérico después de sustituir los argumentos de tipo (suministrados o inferidos) para los parámetros de tipo del método correspondiente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="d7bba-1052">Se realiza una validación final del método elegido mejor:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="d7bba-1053">El método se valida en el contexto del grupo de método: Si el método mejor es un método estático, el grupo de métodos debe haberse obtenido de un *simple_name* o un *member_access* a través de un tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="d7bba-1054">Si el método mejor es un método de instancia, el grupo de métodos debe haberse obtenido de un *simple_name*, un *member_access* a través de una variable o un valor, o un *base_access*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="d7bba-1055">Si ninguno de estos requisitos es true, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="d7bba-1056">Si el método mejor es un método genérico, los argumentos de tipo (suministrados o inferidos) se comprueban con las restricciones ([que satisfacen las restricciones](types.md#satisfying-constraints)) declarados en el método genérico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="d7bba-1057">Si cualquier argumento de tipo no satisface las restricciones correspondientes en el parámetro de tipo, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-1058">Una vez que ha seleccionado un método y validado en tiempo de enlace por los pasos anteriores, la invocación de tiempo de ejecución real se procesa según las reglas de invocación de miembros de función se describe en [comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="d7bba-1059">El efecto intuitivo de las reglas de resolución que se ha descrito anteriormente es como sigue: Para buscar el método concreto invocado por una invocación de método, comience con el tipo indicado por la invocación del método y se continúa en la cadena de herencia hasta que se encuentra la declaración de al menos un método aplicable, accesible y no de reemplazo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="d7bba-1060">A continuación, realizar inferencia de tipos y en el conjunto de métodos aplicables, accesibles y no de reemplazo declarados en dicho tipo de resolución de sobrecarga e invocar el método, por tanto, se selecciona.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="d7bba-1061">Si se encontró ningún método, pruebe en su lugar procesar la invocación de una invocación de método de extensión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="d7bba-1062">Invocaciones de método de extensión</span><span class="sxs-lookup"><span data-stu-id="d7bba-1062">Extension method invocations</span></span>

<span data-ttu-id="d7bba-1063">En una invocación de método ([invocaciones en instancias de conversión boxing](expressions.md#invocations-on-boxed-instances)) de uno de los formularios</span><span class="sxs-lookup"><span data-stu-id="d7bba-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="d7bba-1064">Si el procesamiento normal de la invocación encuentra ningún método aplicable, se realiza un intento para procesar la construcción de una invocación de método de extensión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="d7bba-1065">Si *expr* o cualquiera de los *args* tiene el tipo de tiempo de compilación `dynamic`, no se aplicarán métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="d7bba-1066">El objetivo es encontrar el mejor *type_name* `C`, de modo que la invocación del método estático correspondiente puede tener lugar:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="d7bba-1067">Un método de extensión `Ci.Mj` es ***aptos*** si:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="d7bba-1068">`Ci` es una clase genérica, no anidadas</span><span class="sxs-lookup"><span data-stu-id="d7bba-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="d7bba-1069">El nombre de `Mj` es *identificador*</span><span class="sxs-lookup"><span data-stu-id="d7bba-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="d7bba-1070">`Mj` es accesible y es aplicable cuando se aplica a los argumentos como un método estático, como se muestra arriba</span><span class="sxs-lookup"><span data-stu-id="d7bba-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="d7bba-1071">Existe una identidad implícitos, referencia o una conversión boxing desde *expr* para el tipo del primer parámetro de `Mj`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="d7bba-1072">La búsqueda de `C` continúa como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="d7bba-1073">A partir de la declaración espacio de nombres envolvente más cercana, continuando con cada declaración de espacio de nombres envolvente y termina con la unidad de compilación que lo contiene, se realizan intentos sucesivos para buscar un conjunto de candidatos de métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="d7bba-1074">Si la unidad de compilación o espacio de nombres determinada directamente contiene las declaraciones de tipo no genérico `Ci` con métodos de extensión aptos `Mj`, a continuación, el conjunto de estos métodos de extensión es el conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="d7bba-1075">Si tipos `Ci` importados por *using_static_declarations* y declara directamente en los espacios de nombres importados por *using_namespace_directive*s en la compilación o espacio de nombres de unidad especificada directamente contiene métodos de extensión aptos `Mj`, a continuación, el conjunto de estos métodos de extensión es el conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="d7bba-1076">Si no se encuentra ningún conjunto de candidato en cualquier unidad de compilación o de declaración de espacio de nombres envolvente, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-1077">En caso contrario, se aplica la resolución de sobrecarga como se describe en la versión candidata ([de resolución de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="d7bba-1078">Si no se encuentra ningún método mejor único, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-1079">`C` es el tipo dentro del cual el mejor método se declara como un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="d7bba-1080">Uso de `C` como un destino, la llamada al método, a continuación, se procesa como una invocación de método estático ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="d7bba-1081">Las reglas anteriores significan que los métodos de instancia tienen prioridad sobre los métodos de extensión, que los métodos de extensión disponibles en las declaraciones de espacio de nombres interna tienen prioridad sobre los métodos de extensión disponibles en las declaraciones de espacio de nombres exterior y esa extensión los métodos declarados directamente en un espacio de nombres tienen prioridad sobre los métodos de extensión que se importan en ese mismo espacio de nombres con el uso de una directiva de espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="d7bba-1082">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1082">For example:</span></span>
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

<span data-ttu-id="d7bba-1083">En el ejemplo, `B`del método tiene prioridad sobre el primer método de extensión, y `C`del método tiene prioridad sobre ambos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

<span data-ttu-id="d7bba-1084">El resultado de este ejemplo es:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1084">The output of this example is:</span></span>
```
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="d7bba-1085">`D.G` tiene prioridad sobre `C.G`, y `E.F` tiene prioridad sobre ambos `D.F` y `C.F`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="d7bba-1086">Invocaciones del delegado</span><span class="sxs-lookup"><span data-stu-id="d7bba-1086">Delegate invocations</span></span>

<span data-ttu-id="d7bba-1087">Para una invocación de delegado, la *primary_expression* de la *invocation_expression* debe ser un valor de un *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="d7bba-1088">Además, teniendo en cuenta la *delegate_type* sea un miembro de función con la misma lista de parámetros como el *delegate_type*, el *delegate_type* debe ser aplicable () [Miembro de función aplicable](expressions.md#applicable-function-member)) con respecto a la *argument_list* de la *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="d7bba-1089">El procesamiento en tiempo de ejecución de una invocación de delegado del formulario `D(A)`, donde `D` es un *primary_expression* de un *delegate_type* y `A` es un opcional*argument_list*, consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="d7bba-1090">`D` se evalúa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1090">`D` is evaluated.</span></span> <span data-ttu-id="d7bba-1091">Si esta evaluación, produce una excepción, no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="d7bba-1092">El valor de `D` se comprueba para ser válido.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="d7bba-1093">Si el valor de `D` es `null`, un `System.NullReferenceException` se inicia y no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="d7bba-1094">En caso contrario, `D` es una referencia a una instancia de delegado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="d7bba-1095">Invocaciones de miembro de función ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se realizan en cada una de las entidades que se puede llamar en la lista de invocaciones del delegado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="d7bba-1096">Para las entidades que se puede llamar que consta de una instancia y el método de instancia, la instancia para la invocación es la instancia de la entidad que se puede llamar.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="d7bba-1097">Acceso de elemento</span><span class="sxs-lookup"><span data-stu-id="d7bba-1097">Element access</span></span>

<span data-ttu-id="d7bba-1098">Un *element_access* consta de un *primary_no_array_creation_expression*, seguido de un "`[`" símbolo (token), seguido de un *argument_list*, seguido de un " `]`"token.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="d7bba-1099">El *argument_list* consta de uno o varios *argumento*s, separados por comas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="d7bba-1100">El *argument_list* de un *element_access* no puede contener `ref` o `out` argumentos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="d7bba-1101">Un *element_access* se enlazan dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) si la contiene al menos uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="d7bba-1102">El *primary_no_array_creation_expression* tiene el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="d7bba-1103">Al menos una expresión de la *argument_list* tiene el tipo de tiempo de compilación `dynamic` y *primary_no_array_creation_expression* no tiene un tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="d7bba-1104">En este caso el compilador clasifica el *element_access* como un valor de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="d7bba-1105">Las reglas siguientes para determinar el significado de la *element_access* , a continuación, se aplican en tiempo de ejecución, mediante el tipo de tiempo de ejecución en lugar del tipo de tiempo de compilación de los de la *primary_no_array_creation_expression*y *argument_list* expresiones que tienen el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="d7bba-1106">Si el *primary_no_array_creation_expression* no tiene el tipo de tiempo de compilación `dynamic`, a continuación, el acceso de elemento se somete a una comprobación en tiempo de compilación limitada según se describe en [comprobación dinámico en tiempo de compilación resolución de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="d7bba-1107">Si el *primary_no_array_creation_expression* de un *element_access* es un valor de un *array_type*, *element_access* es un acceso de la matriz ([matriz acceso](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="d7bba-1108">En caso contrario, el *primary_no_array_creation_expression* debe ser una variable o un valor de una clase, struct o tipo de interfaz que tiene uno o más miembros de indizador, en cuyo caso el *element_access* es un acceso de indizador ([acceso al indizador](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="d7bba-1109">Acceso a la matriz</span><span class="sxs-lookup"><span data-stu-id="d7bba-1109">Array access</span></span>

<span data-ttu-id="d7bba-1110">Para un acceso a la matriz, el *primary_no_array_creation_expression* de la *element_access* debe ser un valor de un *array_type*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="d7bba-1111">Además, el *argument_list* de una matriz no se permite el acceso que contiene los argumentos con nombre. El número de expresiones en el *argument_list* debe ser el mismo que el rango de la *array_type*, y cada expresión debe ser de tipo `int`, `uint`, `long`, `ulong`, o debe poder convertirse implícitamente a uno o varios de estos tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="d7bba-1112">El resultado de evaluar un acceso a la matriz es una variable del tipo de elemento de la matriz, es decir, el elemento de matriz seleccionado por los valores de las expresiones de la *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="d7bba-1113">El procesamiento en tiempo de ejecución de un acceso a la matriz del formulario `P[A]`, donde `P` es un *primary_no_array_creation_expression* de un *array_type* y `A` es un *argument_list*, consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="d7bba-1114">`P` se evalúa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1114">`P` is evaluated.</span></span> <span data-ttu-id="d7bba-1115">Si esta evaluación, produce una excepción, no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="d7bba-1116">Las expresiones de índice de la *argument_list* se evalúan en orden, de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="d7bba-1117">Después de la evaluación de cada expresión de índice, una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) se realiza en uno de los siguientes tipos: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="d7bba-1118">Se elige el primer tipo de esta lista para el que existe una conversión implícita.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="d7bba-1119">Por ejemplo, si la expresión de índice es de tipo `short` , a continuación, una conversión implícita a `int` se realiza desde conversiones implícitas de `short` a `int` y desde `short` a `long` son posibles.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="d7bba-1120">Si la evaluación de una expresión de índice o la conversión implícita posterior causa una excepción, no hay otras expresiones de índice se evalúan y ninguna otra se ejecutan los pasos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="d7bba-1121">El valor de `P` se comprueba para ser válido.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="d7bba-1122">Si el valor de `P` es `null`, un `System.NullReferenceException` se inicia y no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="d7bba-1123">El valor de cada expresión en el *argument_list* se compara con los límites reales de cada dimensión de la instancia de matriz al que hace referencia `P`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="d7bba-1124">Si uno o más valores que están fuera del intervalo, un `System.IndexOutOfRangeException` se inicia y no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="d7bba-1125">Se calcula la ubicación del elemento de matriz especificado por las expresiones de índice, y esta ubicación es el resultado de acceso a la matriz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="d7bba-1126">Acceso al indizador</span><span class="sxs-lookup"><span data-stu-id="d7bba-1126">Indexer access</span></span>

<span data-ttu-id="d7bba-1127">Para un acceso de indizador, el *primary_no_array_creation_expression* de la *element_access* debe ser una variable o un valor de una clase, estructura o tipo de interfaz, y este tipo debe implementar uno o más los indizadores que se aplican con respecto a la *argument_list* de la *element_access*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="d7bba-1128">El procesamiento en tiempo de enlace de un acceso de indizador del formulario `P[A]`, donde `P` es un *primary_no_array_creation_expression* de una clase, estructura o tipo de interfaz `T`, y `A` es un *argument_list*, consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="d7bba-1129">El conjunto de indizadores suministrados por `T` se construye.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="d7bba-1130">El conjunto formado por todos los indizadores declarados en `T` o de un tipo base `T` que no son `override` declaraciones y son accesibles en el contexto actual ([acceso a miembros](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="d7bba-1131">El conjunto se reduce a aquellos indizadores que son aplicables y no ocultos por otros indizadores.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="d7bba-1132">Las siguientes reglas se aplican a cada indizador `S.I` en el conjunto, donde `S` es el tipo en el que el indizador `I` se ha declarado:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="d7bba-1133">Si `I` no es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)), a continuación, `I` se quita del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="d7bba-1134">Si `I` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)), a continuación, todos los indizadores se declaran en un tipo base de `S` se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="d7bba-1135">Si `I` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)) y `S` es distinto de un tipo de clase `object`, se quitan todos los indizadores declarados en una interfaz del conjunto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="d7bba-1136">Si el conjunto resultante de indizadores candidatos está vacío, a continuación, no existen indizadores aplicables, y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-1137">El mejor indizador del conjunto de indizadores candidatos se identifica mediante las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="d7bba-1138">Si no se puede identificar un único indexador mejor, el acceso de indizador es ambiguo y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-1139">Las expresiones de índice de la *argument_list* se evalúan en orden, de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="d7bba-1140">El resultado de procesar el acceso de indizador es una expresión que se clasifica como un acceso de indizador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="d7bba-1141">La expresión de acceso de indizador hace referencia al indizador determinado en el paso anterior y tiene una expresión de instancia asociada de `P` y una lista de argumentos de `A`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="d7bba-1142">Dependiendo del contexto en el que se utiliza, un acceso de indizador hace que la invocación de uno de ellos la *descriptor de acceso get* o *descriptor de acceso set* del indizador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="d7bba-1143">Si el acceso de indizador es el destino de una asignación, el *descriptor de acceso set* se invoca para asignar un nuevo valor ([asignación Simple](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="d7bba-1144">En todos los demás casos, el *descriptor de acceso get* se invoca para obtener el valor actual ([valores de expresiones](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="d7bba-1145">Este acceso</span><span class="sxs-lookup"><span data-stu-id="d7bba-1145">This access</span></span>

<span data-ttu-id="d7bba-1146">Un *this_access* consta de una palabra reservada `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="d7bba-1147">Un *this_access* se permite únicamente en el *bloque* de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="d7bba-1148">Tiene uno de los siguientes significados:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="d7bba-1149">Cuando `this` se usa en un *primary_expression* dentro de un constructor de instancia de una clase, se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="d7bba-1150">El tipo del valor es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la clase dentro del cual se produce el uso y el valor es una referencia al objeto que se está construyendo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="d7bba-1151">Cuando `this` se usa en un *primary_expression* dentro de un método de instancia o un descriptor de acceso de instancia de una clase, se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="d7bba-1152">El tipo del valor es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la clase dentro del cual se produce el uso y el valor es una referencia al objeto para el que se invocó el método o el descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="d7bba-1153">Cuando `this` se usa en un *primary_expression* dentro de un constructor de instancia de una estructura, se clasifica como una variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="d7bba-1154">El tipo de la variable es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la estructura dentro del cual se produce el uso y la variable representa la estructura que se está construyendo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="d7bba-1155">El `this` variable de un constructor de instancia de una estructura comporta exactamente igual que un `out` parámetro del tipo struct, en concreto, esto significa que la variable debe asignarlo definitivamente en cada ruta de acceso de ejecución de la instancia constructor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="d7bba-1156">Cuando `this` se usa en un *primary_expression* dentro de un método de instancia o un descriptor de acceso de instancia de una estructura, se clasifica como una variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="d7bba-1157">El tipo de la variable es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la estructura dentro del cual se produce el uso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="d7bba-1158">Si el método o el descriptor de acceso no es un iterador ([iteradores](classes.md#iterators)), el `this` variable representa la estructura para el que el método o el descriptor de acceso se invocó y se comporta exactamente igual que un `ref` parámetro del tipo struct.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="d7bba-1159">Si el método o el descriptor de acceso es un iterador, la `this` variable representa una copia de la estructura para el que el método o el descriptor de acceso se invocó y se comporta exactamente igual que un parámetro de valor del tipo struct.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="d7bba-1160">El uso de `this` en un *primary_expression* en un contexto distinto de los mencionados anteriormente, es un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="d7bba-1161">En concreto, no es posible hacer referencia a `this` en un método estático, un descriptor de acceso de propiedad estática, o en un *variable_initializer* de una declaración de campo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="d7bba-1162">Acceso base</span><span class="sxs-lookup"><span data-stu-id="d7bba-1162">Base access</span></span>

<span data-ttu-id="d7bba-1163">Un *base_access* consta de una palabra reservada `base` seguida un "`.`" token y un identificador o un *argument_list* entre corchetes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="d7bba-1164">Un *base_access* se usa para tener acceso a miembros de clase base que están ocultos de forma similar a los miembros con nombre en la clase actual o el struct.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="d7bba-1165">Un *base_access* se permite únicamente en el *bloque* de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="d7bba-1166">Cuando `base.I` se produce en una clase o struct, `I` debe denotar un miembro de la clase base de esa clase o struct.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="d7bba-1167">Del mismo modo, cuando `base[E]` se produce en una clase, debe existir un indizador aplicable en la clase base.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="d7bba-1168">En tiempo de enlace, *base_access* expresiones del formulario `base.I` y `base[E]` se evalúan exactamente como si se escribieron `((B)this).I` y `((B)this)[E]`, donde `B` es la clase de la clase base o el struct en el que se produce la construcción.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="d7bba-1169">Por lo tanto, `base.I` y `base[E]` corresponden a `this.I` y `this[E]`, excepto `this` se considera como una instancia de la clase base.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="d7bba-1170">Cuando un *base_access* hace referencia a un miembro de función virtual (método, propiedad o indizador), la determinación de los cuales función miembro que desea invocar en tiempo de ejecución ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se cambia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="d7bba-1171">El miembro de función que se invoca se determina mediante la búsqueda de la implementación más derivada ([métodos virtuales](classes.md#virtual-methods)) del miembro de función con respecto a `B` (en lugar de en relación con el tipo de tiempo de ejecución de `this`, como es habitual en un acceso no son de base).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="d7bba-1172">Por lo tanto, dentro de un `override` de un `virtual` miembro de función, un *base_access* puede usarse para invocar la implementación heredada del miembro de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="d7bba-1173">Si el miembro de función al que hace referencia un *base_access* es abstracto, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="d7bba-1174">Operadores postfijos de incremento y decremento</span><span class="sxs-lookup"><span data-stu-id="d7bba-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="d7bba-1175">El operando de un postfijo de incremento o decremento operación debe ser una expresión que se clasifica como una variable, el acceso a una propiedad o un acceso de indizador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="d7bba-1176">El resultado de la operación es un valor del mismo tipo como operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="d7bba-1177">Si el *primary_expression* tiene el tipo de tiempo de compilación `dynamic` , a continuación, el operador se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)), el *post_increment_expression*o *post_decrement_expression* tiene el tipo de tiempo de compilación `dynamic` y las reglas siguientes se aplican en tiempo de ejecución utilizando el tipo de tiempo de ejecución de la *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="d7bba-1178">Si el operando de un sufijo incrementa u operación de decremento es una propiedad o acceso de indizador, la propiedad o indizador debe tener tanto un `get` y un `set` descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="d7bba-1179">Si esto no es el caso, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-1180">Resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-1181">Predefinidos `++` y `--` operadores existen para los siguientes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`y cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="d7bba-1182">Predefinido `++` operadores devuelven el valor generado al sumar 1 al operando y predefinido `--` operadores devuelven el valor generado al restar 1 del operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="d7bba-1183">En un `checked` contexto, si el resultado de esta suma o resta está fuera del intervalo del tipo del resultado y el tipo de resultado es un tipo entero o un tipo de enumeración, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="d7bba-1184">El procesamiento en tiempo de ejecución de un incremento de postfijo o disminuir la operación de la forma `x++` o `x--` consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="d7bba-1185">Si `x` se clasifica como una variable:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="d7bba-1186">`x` se evalúa para producir la variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="d7bba-1187">El valor de `x` se guarda.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="d7bba-1188">Se invoca el operador seleccionado con el valor guardado de `x` como su argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="d7bba-1189">El valor devuelto por el operador se almacena en la ubicación dada por la evaluación de `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="d7bba-1190">El valor guardado de `x` se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="d7bba-1191">Si `x` se clasifica como un acceso de propiedad o indizador:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="d7bba-1192">La expresión de instancia (si `x` no `static`) y la lista de argumentos (si `x` es un acceso de indizador) asociadas con `x` se evalúan, y los resultados se usan en la siguiente `get` y `set` invocaciones de descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="d7bba-1193">El `get` descriptor de acceso de `x` se invoca y se guarda el valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="d7bba-1194">Se invoca el operador seleccionado con el valor guardado de `x` como su argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="d7bba-1195">El `set` descriptor de acceso de `x` se invoca con el valor devuelto por el operador como su `value` argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="d7bba-1196">El valor guardado de `x` se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="d7bba-1197">El `++` y `--` operadores también admiten la notación de prefijo ([prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="d7bba-1198">Normalmente, el resultado de `x++` o `x--` es el valor de `x` antes de la operación, mientras que el resultado de `++x` o `--x` es el valor de `x` después de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="d7bba-1199">En cualquier caso, `x` sí tiene el mismo valor después de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="d7bba-1200">Un `operator ++` o `operator --` implementación puede invocarse mediante la notación de prefijo o sufijo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="d7bba-1201">No es posible tener implementaciones de operador independientes para las dos notaciones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="d7bba-1202">Operador new</span><span class="sxs-lookup"><span data-stu-id="d7bba-1202">The new operator</span></span>

<span data-ttu-id="d7bba-1203">El `new` operador se usa para crear nuevas instancias de tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="d7bba-1204">Hay tres formas de `new` expresiones:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="d7bba-1205">Expresiones de creación de objetos se utilizan para crear nuevas instancias de tipos de clases y tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="d7bba-1206">Expresiones de creación de matriz se utilizan para crear nuevas instancias de tipos de matriz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="d7bba-1207">Expresiones de creación de delegado se utilizan para crear nuevas instancias del delegado de tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="d7bba-1208">El `new` implica la creación de una instancia de un tipo de operador, pero no implica necesariamente una asignación dinámica de memoria.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="d7bba-1209">En concreto, las instancias de tipos de valor no requieren más memoria más allá de las variables en el que residen y no hay asignaciones dinámicas se producen cuando `new` se usa para crear instancias de tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="d7bba-1210">Expresiones de creación de objetos</span><span class="sxs-lookup"><span data-stu-id="d7bba-1210">Object creation expressions</span></span>

<span data-ttu-id="d7bba-1211">Un *object_creation_expression* se utiliza para crear una nueva instancia de un *class_type* o un *value_type*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

<span data-ttu-id="d7bba-1212">El *tipo* de un *object_creation_expression* debe ser un *class_type*, un *value_type* o un *tipo_parámetro* .</span><span class="sxs-lookup"><span data-stu-id="d7bba-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="d7bba-1213">El *tipo* no puede ser un `abstract` *class_type*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="d7bba-1214">Opcional *argument_list* ([listas de argumentos](expressions.md#argument-lists)) sólo está permitida si el *tipo* es un *class_type* o un *struct_ tipo*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="d7bba-1215">Una expresión de creación de objeto puede omitir la lista de argumentos de constructor y envolventes paréntesis proporcionan incluye un inicializador de objeto o un inicializador de colección.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="d7bba-1216">Si se omite la lista de argumentos de constructor e incluya entre paréntesis son equivalente a especificar una lista de argumentos vacía.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="d7bba-1217">Procesamiento de una expresión de creación de objetos que incluye un inicializador de objeto o un inicializador de colección consta de procesamiento en primer lugar el constructor de instancia y después procesando las inicializaciones de miembro o el elemento especificadas por el inicializador de objeto ([ Inicializadores de objeto](expressions.md#object-initializers)) o un inicializador de colección ([inicializadores de colección](expressions.md#collection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="d7bba-1218">Si alguno de los argumentos en el elemento opcional *argument_list* tiene el tipo de tiempo de compilación `dynamic` el *object_creation_expression* se enlazan dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) y las reglas siguientes se aplican en tiempo de ejecución con el tipo de tiempo de ejecución de esos argumentos de la *argument_list* que tienen el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="d7bba-1219">Sin embargo, la creación del objeto se somete a una comprobación en tiempo de compilación limitada según se describe en [comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="d7bba-1220">El procesamiento en tiempo de enlace de un *object_creation_expression* del formulario `new T(A)`, donde `T` es un *class_type* o un *value_type* y `A` es una función opcional *argument_list*, consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="d7bba-1221">Si `T` es un *value_type* y `A` no está presente:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="d7bba-1222">El *object_creation_expression* es una invocación del constructor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="d7bba-1223">El resultado de la *object_creation_expression* es un valor de tipo `T`, es decir, el valor predeterminado de `T` tal como se define en [System.ValueType el tipo](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="d7bba-1224">De lo contrario, si `T` es un *tipo_parámetro* y `A` no está presente:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="d7bba-1225">Si ninguna restricción de tipo de valor o una restricción de constructor ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) se ha especificado para `T`, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="d7bba-1226">El resultado de la *object_creation_expression* es un valor del tipo de tiempo de ejecución que se ha enlazado el parámetro de tipo, es decir, el resultado de invocar el constructor predeterminado de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="d7bba-1227">El tipo de tiempo de ejecución puede ser un tipo de referencia o un tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="d7bba-1228">De lo contrario, si `T` es un *class_type* o un *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="d7bba-1229">Si `T` es un `abstract` *class_type*, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="d7bba-1230">Para invocar el constructor de instancia se determina mediante las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="d7bba-1231">El conjunto de constructores de instancias de candidato consta de todos los constructores de instancia accesible declarados en `T` que son aplicables con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="d7bba-1232">Si el conjunto de constructores de instancias de candidato está vacío, o si no se puede identificar un mejor constructor de instancia, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="d7bba-1233">El resultado de la *object_creation_expression* es un valor de tipo `T`, es decir, el valor generado al invocar el constructor de instancia determinado en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="d7bba-1234">En caso contrario, el *object_creation_expression* no es válido, y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-1235">Incluso si la *object_creation_expression* dinámicamente está enlazado, el tipo de tiempo de compilación sigue siendo `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="d7bba-1236">El procesamiento en tiempo de ejecución de un *object_creation_expression* del formulario `new T(A)`, donde `T` es *class_type* o un *struct_type* y `A` es una función opcional *argument_list*, consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="d7bba-1237">Si `T` es un *class_type*:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="d7bba-1238">Una nueva instancia de la clase `T` está asignada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="d7bba-1239">Si no hay suficiente memoria disponible para asignar la nueva instancia, un `System.OutOfMemoryException` se inicia y no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="d7bba-1240">Todos los campos de la nueva instancia se inicializan en sus valores predeterminados ([los valores predeterminados](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="d7bba-1241">Se invoca el constructor de instancia según las reglas de invocación de función miembro ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="d7bba-1242">Una referencia a la instancia recién asignada automáticamente se pasa al constructor de instancia y la instancia se puede acceder desde dentro de ese constructor como `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="d7bba-1243">Si `T` es un *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="d7bba-1244">Una instancia del tipo `T` se crea mediante la asignación de una variable local temporal.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="d7bba-1245">Desde un constructor de instancia de un *struct_type* es necesario para asignar definitivamente un valor para cada campo de la instancia que se va a crear, no es necesaria ninguna inicialización de la variable temporal.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="d7bba-1246">Se invoca el constructor de instancia según las reglas de invocación de función miembro ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="d7bba-1247">Una referencia a la instancia recién asignada automáticamente se pasa al constructor de instancia y la instancia se puede acceder desde dentro de ese constructor como `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="d7bba-1248">Inicializadores de objeto</span><span class="sxs-lookup"><span data-stu-id="d7bba-1248">Object initializers</span></span>

<span data-ttu-id="d7bba-1249">Un ***inicializador de objeto*** especifica los valores de cero o más campos, propiedades o elementos indizados de un objeto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

<span data-ttu-id="d7bba-1250">Un inicializador de objeto se compone de una secuencia de inicializadores de miembro, delimitadas por `{` y `}` tokens y separados por comas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="d7bba-1251">Cada *member_initializer* designa un destino para la inicialización.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="d7bba-1252">Un *identificador* debe nombrar un campo accesible o una propiedad del objeto que se está inicializando, mientras que un *argument_list* entre corchetes debe especificar los argumentos de un indizador accesible en el objeto que se está inicializando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="d7bba-1253">Es un error en un inicializador de objeto incluir a más de un inicializador de miembro para el mismo campo o propiedad.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="d7bba-1254">Cada *initializer_target* va seguido de un signo igual y una expresión, un inicializador de objeto o un inicializador de colección.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="d7bba-1255">No es posible que las expresiones en el inicializador de objeto para hacer referencia al objeto recién creado que se está inicializando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="d7bba-1256">Un inicializador de miembro que se especifica una expresión después del signo igual se procesa en la misma manera que una asignación ([asignación Simple](expressions.md#simple-assignment)) al destino.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="d7bba-1257">Un inicializador de miembro que especifica un inicializador de objeto después del signo igual es un ***inicializador de objeto anidado***, es decir, una inicialización de un objeto incrustado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="d7bba-1258">En lugar de asignar un nuevo valor al campo o propiedad, las asignaciones en el inicializador de objeto anidado se tratan como asignaciones a los miembros del campo o propiedad.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="d7bba-1259">Inicializadores de objeto anidado no pueden aplicarse a las propiedades con un tipo de valor, o a los campos de solo lectura con un tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="d7bba-1260">Un inicializador de miembro que especifica a un inicializador de colección después del signo igual es una inicialización de una recopilación incrustada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="d7bba-1261">En lugar de asignar una nueva colección en el campo de destino, propiedad o indizador, los elementos contenidos en el inicializador se agregan a la colección al que hace referencia el destino.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="d7bba-1262">El destino debe ser de un tipo de colección que cumple los requisitos especificados en [inicializadores de colección](expressions.md#collection-initializers).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="d7bba-1263">Los argumentos de un inicializador de índice siempre se evaluará una sola vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="d7bba-1264">Por lo tanto, incluso si los argumentos terminan nunca acostumbrando (por ejemplo, debido a un inicializador vacío anidado), se evaluará para sus efectos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="d7bba-1265">La clase siguiente representa un punto con dos coordenadas:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="d7bba-1266">Una instancia de `Point` pueden crearse y se inicializan como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="d7bba-1267">que tiene el mismo efecto que</span><span class="sxs-lookup"><span data-stu-id="d7bba-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="d7bba-1268">donde `__a` es una variable temporal en caso contrario invisible y accesible.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="d7bba-1269">La clase siguiente representa un rectángulo que se crea a partir de dos puntos:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="d7bba-1270">Una instancia de `Rectangle` pueden crearse y se inicializan como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="d7bba-1271">que tiene el mismo efecto que</span><span class="sxs-lookup"><span data-stu-id="d7bba-1271">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
<span data-ttu-id="d7bba-1272">donde `__r`, `__p1` y `__p2` son variables temporales que de otro modo invisible y no son accesibles.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="d7bba-1273">Si `Rectangle`del constructor asigna los dos incrustados `Point` instancias</span><span class="sxs-lookup"><span data-stu-id="d7bba-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="d7bba-1274">la construcción siguiente puede usarse para inicializar el objeto incrustado `Point` instancias en lugar de asignar nuevas instancias:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="d7bba-1275">que tiene el mismo efecto que</span><span class="sxs-lookup"><span data-stu-id="d7bba-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="d7bba-1276">Dada una definición adecuada de C, en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1276">Given an appropriate definition of C, the following example:</span></span>
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
<span data-ttu-id="d7bba-1277">es equivalente a esta serie de asignaciones:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1277">is equivalent to this series of assignments:</span></span>
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
<span data-ttu-id="d7bba-1278">donde `__c`, etc., son variables generadas que son inaccesibles al código fuente y invisible.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="d7bba-1279">Tenga en cuenta que los argumentos para `[0,0]` son evalúa solo una vez y los argumentos para `[1,2]` se evalúan una vez, aunque nunca se usan.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="d7bba-1280">Inicializadores de colección</span><span class="sxs-lookup"><span data-stu-id="d7bba-1280">Collection initializers</span></span>

<span data-ttu-id="d7bba-1281">Un inicializador de colección especifica los elementos de una colección.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1281">A collection initializer specifies the elements of a collection.</span></span>

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

<span data-ttu-id="d7bba-1282">Un inicializador de colección consta de una secuencia de inicializadores de elemento, delimitadas por `{` y `}` tokens y separados por comas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="d7bba-1283">Cada inicializador de elemento especifica un elemento que se va a agregarse al objeto de colección que se está inicializando y consta de una lista de expresiones delimitadas por `{` y `}` tokens y separados por comas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="d7bba-1284">Un inicializador de elemento de expresión simple se puede escribir sin llaves, pero, a continuación, no puede ser una expresión de asignación, para evitar la ambigüedad con inicializadores de miembro.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="d7bba-1285">El *non_assignment_expression* producción se define en [expresión](expressions.md#expression).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="d7bba-1286">El siguiente es un ejemplo de una expresión de creación de objetos que incluye a un inicializador de colección:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="d7bba-1287">El objeto de colección al que se aplica un inicializador de colección debe ser de un tipo que implementa `System.Collections.IEnumerable` o se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="d7bba-1288">Para cada uno especifica el elemento en orden, el inicializador de colección invoca un `Add` método en el destino con la lista de expresiones del inicializador de elementos de objeto como lista de argumentos, aplicación de búsqueda de miembros normales y para cada invocación de resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="d7bba-1289">Por lo tanto, el objeto de colección debe tener un método de instancia o extensión aplicable con el nombre `Add` cada inicializador de elemento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="d7bba-1290">La clase siguiente representa un contacto con un nombre y una lista de números de teléfono:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="d7bba-1291">Un `List<Contact>` pueden crearse y se inicializan como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
<span data-ttu-id="d7bba-1292">que tiene el mismo efecto que</span><span class="sxs-lookup"><span data-stu-id="d7bba-1292">which has the same effect as</span></span>
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
<span data-ttu-id="d7bba-1293">donde `__clist`, `__c1` y `__c2` son variables temporales que de otro modo invisible y no son accesibles.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="d7bba-1294">Expresiones de creación de matriz</span><span class="sxs-lookup"><span data-stu-id="d7bba-1294">Array creation expressions</span></span>

<span data-ttu-id="d7bba-1295">Un *array_creation_expression* se utiliza para crear una nueva instancia de un *array_type*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="d7bba-1296">Una expresión de creación de matriz del primer formulario asigna una instancia de matriz del tipo que es el resultado de eliminar cada una de las expresiones individuales de la lista de expresiones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="d7bba-1297">Por ejemplo, la expresión de creación de matriz `new int[10,20]` genera una instancia de la matriz de tipo `int[,]`y la expresión de creación de matriz `new int[10][,]` genera una matriz de tipo `int[][,]`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="d7bba-1298">Cada expresión en la lista de expresiones debe ser de tipo `int`, `uint`, `long`, o `ulong`, o se pueden convertir implícitamente a uno o varios de estos tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="d7bba-1299">El valor de cada expresión determina la longitud de la dimensión correspondiente en la instancia de matriz recién asignado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="d7bba-1300">Puesto que la longitud de una dimensión de matriz debe ser no negativa, es un error en tiempo de compilación para tener un *constant_expression* con un valor negativo en la lista de expresiones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="d7bba-1301">Excepto en un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)), el diseño de las matrices no se especifica.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="d7bba-1302">Si una expresión de creación de matriz del primer formulario incluye a un inicializador de matriz, cada expresión en la lista de expresiones debe ser una constante y las longitudes de rango y dimensión especificadas por la lista de expresiones deben coincidir con los del inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="d7bba-1303">En una expresión de creación de matriz del segundo o tercer formulario, el rango del especificador de tipo o rango en la matriz especificada debe coincidir con del inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="d7bba-1304">Las longitudes de dimensión individuales se infieren del número de elementos en cada uno de los niveles de anidamiento correspondientes del inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="d7bba-1305">Por lo tanto, la expresión</span><span class="sxs-lookup"><span data-stu-id="d7bba-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="d7bba-1306">corresponde exactamente a</span><span class="sxs-lookup"><span data-stu-id="d7bba-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="d7bba-1307">Una expresión de creación de matriz del tercer formulario se conoce como un ***expresión de creación de matriz con tipo implícito***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="d7bba-1308">Es similar a la segunda forma, salvo que el tipo de elemento de la matriz no es da explícitamente, pero se determina como el tipo más común ([encontrar el mejor tipo común de un conjunto de expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) del conjunto de expresiones de la matriz inicializador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="d7bba-1309">Para una matriz multidimensional, es decir, donde el *rank_specifier* contiene al menos una coma, este conjunto comprende todos *expresión*s encontrado en anidados *array_initializer*s.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="d7bba-1310">Se describe con más detalle en los inicializadores de matriz [inicializadores de matriz](arrays.md#array-initializers).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="d7bba-1311">El resultado de evaluar una expresión de creación de matriz se clasifica como un valor, es decir, una referencia a la instancia de matriz recién asignado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="d7bba-1312">El procesamiento en tiempo de ejecución de una expresión de creación de matriz consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="d7bba-1313">Las expresiones de longitud de la dimensión de la *expression_list* se evalúan en orden, de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="d7bba-1314">Después de la evaluación de cada expresión, una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) se realiza en uno de los siguientes tipos: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="d7bba-1315">Se elige el primer tipo de esta lista para el que existe una conversión implícita.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="d7bba-1316">Si la evaluación de una expresión o la conversión implícita posterior causa una excepción, no hay otras expresiones se evalúan y no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="d7bba-1317">Los valores calculados para las longitudes de dimensión se validan como sigue.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="d7bba-1318">Si uno o varios de los valores son inferiores a cero, una `System.OverflowException` se inicia y no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="d7bba-1319">Se asigna una instancia de matriz con las longitudes de dimensión determinada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="d7bba-1320">Si no hay suficiente memoria disponible para asignar la nueva instancia, un `System.OutOfMemoryException` se inicia y no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="d7bba-1321">Todos los elementos de la nueva instancia de la matriz se inicializan en sus valores predeterminados ([los valores predeterminados](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="d7bba-1322">Si la expresión de creación de matriz contiene a un inicializador de matriz, a continuación, cada expresión de inicializador de matriz se evalúan y se asignan a su correspondiente elemento de matriz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="d7bba-1323">Las evaluaciones y asignaciones se realizan en el orden de las expresiones se escriben en el inicializador de matriz, en otras palabras, los elementos se inicializan en sentido ascendente índice, con la dimensión situada más a la derecha.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="d7bba-1324">Si la evaluación de una expresión determinada o la asignación posterior al elemento de matriz correspondiente causa una excepción, no hay más elementos se inicializan (y los elementos restantes, por tanto, tendrán sus valores predeterminados).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="d7bba-1325">Una expresión de creación de matriz permite crear una instancia de una matriz con elementos de un tipo de matriz, pero se deben inicializar manualmente los elementos de este tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="d7bba-1326">Por ejemplo, la instrucción</span><span class="sxs-lookup"><span data-stu-id="d7bba-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="d7bba-1327">crea una matriz unidimensional con 100 elementos de tipo `int[]`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="d7bba-1328">El valor inicial de cada elemento es `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="d7bba-1329">No es posible que la misma expresión de creación de la matriz también crear instancias de las submatrices y la instrucción</span><span class="sxs-lookup"><span data-stu-id="d7bba-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="d7bba-1330">genera un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1330">results in a compile-time error.</span></span> <span data-ttu-id="d7bba-1331">Creación de instancias de las submatrices en su lugar, debe realizarse manualmente, como en</span><span class="sxs-lookup"><span data-stu-id="d7bba-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="d7bba-1332">Cuando una matriz de matrices tiene forma "rectangular", es decir, cuando las submatrices tienen todos la misma longitud, es más eficaz utilizar una matriz multidimensional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="d7bba-1333">En el ejemplo anterior, la creación de instancias de la matriz de matrices crea 101 objetos, una matriz externa y 100 submatrices.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="d7bba-1334">En cambio,</span><span class="sxs-lookup"><span data-stu-id="d7bba-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="d7bba-1335">crea un único objeto, una matriz bidimensional y se lleva a cabo la asignación en una sola instrucción.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="d7bba-1336">Los siguientes son ejemplos de expresiones de creación de matriz con tipo implícito:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="d7bba-1337">La última expresión genera un error de tiempo de compilación porque ni `int` ni `string` es implícitamente convertible a otro y, por lo que es común no mejor escriba.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="d7bba-1338">Una expresión de creación de matriz con tipo explícito se debe usar en este caso, por ejemplo si se especifica el tipo es `object[]`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="d7bba-1339">Como alternativa, se puede convertir uno de los elementos en un tipo base común, que se convierte entonces en el tipo de elemento deducido.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="d7bba-1340">Expresiones de creación de matriz con tipo implícito se pueden combinar con inicializadores de objeto anónimo ([expresiones de creación de objeto anónimo](expressions.md#anonymous-object-creation-expressions)) crear de forma anónima con el tipo de estructuras de datos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="d7bba-1341">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1341">For example:</span></span>
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="d7bba-1342">Expresiones de creación de delegado</span><span class="sxs-lookup"><span data-stu-id="d7bba-1342">Delegate creation expressions</span></span>

<span data-ttu-id="d7bba-1343">Un *delegate_creation_expression* se utiliza para crear una nueva instancia de un *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="d7bba-1344">El argumento de una expresión de creación de delegado debe ser un grupo de métodos, una función anónima o un valor del tipo de tiempo de compilación `dynamic` o un *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="d7bba-1345">Si el argumento es un grupo de métodos, identifica el método y, para un método de instancia, el objeto que se va a crear un delegado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="d7bba-1346">Si el argumento es una función anónima directamente define los parámetros y el cuerpo del método de destino del delegado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="d7bba-1347">Si el argumento es un valor identifica una instancia de delegado que se va a crear una copia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="d7bba-1348">Si el *expresión* tiene el tipo de tiempo de compilación `dynamic`, *delegate_creation_expression* se enlazan dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) y las reglas siguientes se aplican en tiempo de ejecución utilizando el tipo de tiempo de ejecución de la *expresión*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="d7bba-1349">En caso contrario, las reglas se aplican en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="d7bba-1350">El procesamiento en tiempo de enlace de un *delegate_creation_expression* del formulario `new D(E)`, donde `D` es un *delegate_type* y `E` es un *expresión* , consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="d7bba-1351">Si `E` es un grupo de métodos, la expresión de creación de delegado se procesa en la misma manera que la conversión de grupos de un método ([conversiones de grupo de métodos](conversions.md#method-group-conversions)) desde `E` a `D`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="d7bba-1352">Si `E` es una función anónima, la expresión de creación de delegado se procesa en la misma manera que una conversión de función anónima ([conversiones de función anónima](conversions.md#anonymous-function-conversions)) desde `E` a `D`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="d7bba-1353">Si `E` es un valor, `E` deben ser compatibles ([declaraciones de delegado](delegates.md#delegate-declarations)) con `D`, y el resultado es una referencia a un delegado recién creado del tipo `D` que hace referencia a la invocación del misma lista como `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="d7bba-1354">Si `E` no es compatible con `D`, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="d7bba-1355">El procesamiento en tiempo de ejecución de un *delegate_creation_expression* del formulario `new D(E)`, donde `D` es un *delegate_type* y `E` es un *expresión* , consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="d7bba-1356">Si `E` es un grupo de métodos, la expresión de creación de delegado se evalúa como la conversión de grupos de un método ([conversiones de grupo de métodos](conversions.md#method-group-conversions)) desde `E` a `D`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="d7bba-1357">Si `E` es una función anónima, la creación del delegado se evalúa como una conversión de función anónima de `E` a `D` ([conversiones de función anónima](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="d7bba-1358">Si `E` es un valor de un *delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="d7bba-1359">`E` se evalúa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1359">`E` is evaluated.</span></span> <span data-ttu-id="d7bba-1360">Si esta evaluación, produce una excepción, no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="d7bba-1361">Si el valor de `E` es `null`, un `System.NullReferenceException` se inicia y no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="d7bba-1362">Una nueva instancia del tipo de delegado `D` está asignada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="d7bba-1363">Si no hay suficiente memoria disponible para asignar la nueva instancia, un `System.OutOfMemoryException` se inicia y no se ejecuta ningún paso adicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="d7bba-1364">La nueva instancia de delegado se inicializa con la misma lista de invocación que la instancia del delegado proporcionada por `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="d7bba-1365">La lista de invocaciones de un delegado se determina cuando el delegado se crea una instancia y, a continuación, se mantiene constante durante toda la duración del delegado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="d7bba-1366">En otras palabras, no es posible cambiar las entidades de destino que se puede llamar de un delegado, una vez que se ha creado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="d7bba-1367">Cuando se combinan dos delegados o se quita uno de otro ([declaraciones de delegado](delegates.md#delegate-declarations)), se produce un nuevo delegado; cambia el contenido no tiene ningún delegado existente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="d7bba-1368">No es posible crear a un delegado que hace referencia a una propiedad, el indizador, operador definido por el usuario, el constructor de instancia, un destructor o constructor estático.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="d7bba-1369">Como se describió anteriormente, cuando se crea un delegado de un grupo de métodos, la lista de parámetros formales y tipo de valor devuelto del delegado determinar cuál de los métodos sobrecargados para seleccionar.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="d7bba-1370">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-1370">In the example</span></span>
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
<span data-ttu-id="d7bba-1371">el `A.f` campo se inicializa con un delegado que hace referencia a la segunda `Square` método porque ese método coincide exactamente con la lista de parámetros formales y el tipo de valor devuelto de `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="d7bba-1372">Tenía el segundo `Square` método no estado presente, se habría producido un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="d7bba-1373">Expresiones de creación de objeto anónimo</span><span class="sxs-lookup"><span data-stu-id="d7bba-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="d7bba-1374">Un *anonymous_object_creation_expression* se utiliza para crear un objeto de un tipo anónimo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

<span data-ttu-id="d7bba-1375">Un inicializador de objeto anónimo declara un tipo anónimo y devuelve una instancia de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="d7bba-1376">Un tipo anónimo es un tipo sin nombre de clase que hereda directamente de `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="d7bba-1377">Los miembros de un tipo anónimo son una secuencia de propiedades de solo lectura que se deducen del inicializador de objeto anónimo usado para crear una instancia del tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="d7bba-1378">En concreto, un inicializador de objeto anónimo del formulario</span><span class="sxs-lookup"><span data-stu-id="d7bba-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="d7bba-1379">declara un tipo anónimo del formulario</span><span class="sxs-lookup"><span data-stu-id="d7bba-1379">declares an anonymous type of the form</span></span>
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
<span data-ttu-id="d7bba-1380">donde cada `Tx` es el tipo de la expresión correspondiente `ex`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="d7bba-1381">La expresión utilizada en un *member_declarator* debe tener un tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="d7bba-1382">Por lo tanto, es un error en tiempo de compilación para una expresión en un *member_declarator* a ser null ni una función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="d7bba-1383">También es un error en tiempo de compilación para que la expresión tiene un tipo no seguro.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="d7bba-1384">Los nombres de un tipo anónimo y del parámetro para su `Equals` método generados automáticamente por el compilador y no puede hacer referencia en el texto del programa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="d7bba-1385">Dentro del mismo programa, dos inicializadores de objeto anónimos que especifican una secuencia de propiedades de los mismos nombres y tipos de tiempo de compilación en el mismo orden producirán instancias del mismo tipo anónimo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="d7bba-1386">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="d7bba-1387">se permite la asignación en la última línea porque `p1` y `p2` son del mismo tipo anónimo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="d7bba-1388">El `Equals` y `GetHashcode` métodos en tipos anónimos invalidación los métodos heredados de `object`y se definen en términos de la `Equals` y `GetHashcode` de las propiedades, para que dos instancias del mismo tipo anónimo son iguales si y sólo si todas sus propiedades son iguales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="d7bba-1389">Un declarador de miembro puede abreviarse con un nombre simple ([inferencia](expressions.md#type-inference)), un acceso a miembros ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), un acceso base ([Base acceso](expressions.md#base-access)) o un acceso a miembros condicional null ([expresiones condicionales nulos como los inicializadores de proyección](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="d7bba-1390">Esto se denomina un ***inicializadores de proyección*** y es una abreviatura de una declaración de y la asignación a una propiedad con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="d7bba-1391">En concreto, los declaradores de miembros de los formularios</span><span class="sxs-lookup"><span data-stu-id="d7bba-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="d7bba-1392">son exactamente equivalentes a lo siguiente, respectivamente:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="d7bba-1393">Por lo tanto, en un inicializador de proyección el *identificador* selecciona el valor y el campo o propiedad al que se asigna el valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="d7bba-1394">De manera intuitiva, un inicializador de proyección proyectos no solo un valor, sino también el nombre del valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="d7bba-1395">El operador typeof</span><span class="sxs-lookup"><span data-stu-id="d7bba-1395">The typeof operator</span></span>

<span data-ttu-id="d7bba-1396">El `typeof` operador se usa para obtener el `System.Type` un tipo de objeto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

<span data-ttu-id="d7bba-1397">El primer formulario del *typeof_expression* consta de un `typeof` palabra clave seguido por un entre paréntesis *tipo*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="d7bba-1398">El resultado de una expresión de esta forma es el `System.Type` objeto para el tipo indicado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="d7bba-1399">Solo hay un `System.Type` objeto para un tipo dado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="d7bba-1400">Esto significa que para un tipo `T`, `typeof(T) == typeof(T)` siempre es true.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="d7bba-1401">El *tipo* no puede ser `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="d7bba-1402">La segunda forma de *typeof_expression* consta de un `typeof` palabra clave seguido por un entre paréntesis *unbound_type_name*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="d7bba-1403">Un *unbound_type_name* es muy similar a un *type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)), salvo que un *unbound_type_name* contiene *generic_dimension_specifier*s donde un *type_name* contiene *type_argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="d7bba-1404">Cuando el operando de un *typeof_expression* es una secuencia de tokens que satisface las gramáticas de ambos *unbound_type_name* y *type_name*, es decir, cuando no contiene ni un *generic_dimension_specifier* ni un *type_argument_list*, la secuencia de tokens se considera un *type_name*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="d7bba-1405">El significado de un *unbound_type_name* se determina como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="d7bba-1406">Convertir la secuencia de tokens para una *type_name* reemplazando cada *generic_dimension_specifier* con un *type_argument_list* con el mismo número de comas y el palabra clave `object` cada *type_argument*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="d7bba-1407">Evaluar resultante *type_name*, mientras se les ignora todas las restricciones de parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="d7bba-1408">El *unbound_type_name* resuelve el tipo genérico sin enlazar asociado con el tipo construido resultante ([dependientes e independientes tipos](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="d7bba-1409">El resultado de la *typeof_expression* es la `System.Type` objeto resultante sin delimitar tipo genérico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="d7bba-1410">La tercera forma de *typeof_expression* consta de un `typeof` palabra clave seguido por un entre paréntesis `void` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="d7bba-1411">El resultado de una expresión de esta forma es el `System.Type` objeto que representa la ausencia de un tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="d7bba-1412">El objeto de tipo devuelto por `typeof(void)` es distinto del objeto del tipo devuelto por cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="d7bba-1413">Este objeto de tipo especial es útil en las bibliotecas de clases que permiten la reflexión en métodos en el lenguaje, donde dichos métodos desean tener una manera de representar el tipo de valor devuelto de cualquier método, incluidos los métodos void, con una instancia de `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="d7bba-1414">El `typeof` operador puede usarse en un parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="d7bba-1415">El resultado es el `System.Type` objeto para el tipo de tiempo de ejecución que se enlazó con el parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="d7bba-1416">El `typeof` operador también puede usarse en un tipo construido o un tipo genérico sin enlazar ([dependientes e independientes tipos](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="d7bba-1417">El `System.Type` de objeto para un tipo genérico sin enlazar no es el mismo que el `System.Type` objeto del tipo de instancia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="d7bba-1418">El tipo de instancia es siempre un tipo construido cerrado en tiempo de ejecución para su `System.Type` objeto depende de los argumentos de tipo de tiempo de ejecución en uso, mientras que el tipo genérico sin enlazar no tiene ningún argumento de tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="d7bba-1419">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-1419">The example</span></span>
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
<span data-ttu-id="d7bba-1420">genera el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1420">produces the following output:</span></span>
```
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

<span data-ttu-id="d7bba-1421">Tenga en cuenta que `int` y `System.Int32` son del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="d7bba-1422">Tenga en cuenta también que el resultado de `typeof(X<>)` no depende del argumento de tipo, pero el resultado de `typeof(X<T>)` does.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="d7bba-1423">Los operadores checked y unchecked</span><span class="sxs-lookup"><span data-stu-id="d7bba-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="d7bba-1424">El `checked` y `unchecked` operadores se utilizan para controlar la ***contexto de comprobación de desbordamiento*** para conversiones y operaciones aritméticas de tipo integral.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="d7bba-1425">El `checked` operador evalúa la expresión contenida en un contexto comprobado y el `unchecked` operador evalúa la expresión contenida en un contexto no comprobado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="d7bba-1426">Un *checked_expression* o *unchecked_expression* corresponde exactamente a un *parenthesized_expression* ([lasexpresionesentreparéntesis](expressions.md#parenthesized-expressions)), excepto en que se evalúa la expresión contenida en el contexto de comprobación de desbordamiento dado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="d7bba-1427">También se puede controlar el contexto de comprobación de desbordamiento mediante la `checked` y `unchecked` instrucciones ([las instrucciones checked y unchecked](statements.md#the-checked-and-unchecked-statements)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="d7bba-1428">Las operaciones siguientes se ven afectadas por el contexto establecido por la comprobación de desbordamiento el `checked` y `unchecked` operadores e instrucciones:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="d7bba-1429">Predefinido `++` y `--` operadores unarios ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) y [prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)), cuando el operando es de un entero tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="d7bba-1430">Predefinido `-` operador unario ([operador unario menos](expressions.md#unary-minus-operator)), cuando el operando es de tipo entero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="d7bba-1431">Predefinido `+`, `-`, `*`, y `/` operadores binarios ([operadores aritméticos](expressions.md#arithmetic-operators)), cuando ambos operandos son de tipos enteros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="d7bba-1432">Conversiones numéricas explícitas ([conversiones numéricas explícitas](conversions.md#explicit-numeric-conversions)) de un tipo integral a otro tipo entero o de `float` o `double` a un tipo entero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="d7bba-1433">Cuando una de las operaciones anteriores se producirá un resultado que es demasiado grande para representarlo en el tipo de destino, el contexto en que la operación se realiza controles el comportamiento resultante:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="d7bba-1434">En un `checked` contexto, si la operación es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)), se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="d7bba-1435">En caso contrario, cuando la operación se realiza en tiempo de ejecución, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="d7bba-1436">En un `unchecked` contexto, el resultado se trunca al descartar los bits de orden superior que no caben en el tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="d7bba-1437">Para expresiones no constantes (expresiones que se evalúan en tiempo de ejecución) que no están incluidas por `checked` o `unchecked` operadores o instrucciones, el contexto de comprobación de desbordamiento predeterminado es `unchecked` a menos que los factores externos (por ejemplo, el compilador llamar conmutadores y configuración del entorno de ejecución) para `checked` evaluación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="d7bba-1438">Las expresiones constantes (expresiones que se pueden evaluar por completo en tiempo de compilación), el contexto de comprobación de desbordamiento predeterminado siempre es `checked`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="d7bba-1439">A menos que explícitamente se coloca una expresión constante en una `unchecked` contexto, desbordamientos que ocurren durante la evaluación de tiempo de compilación de la expresión siempre causan errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="d7bba-1440">El cuerpo de una función anónima no se ve afectado por `checked` o `unchecked` contextos en los que se produce la función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="d7bba-1441">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-1441">In the example</span></span>
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
<span data-ttu-id="d7bba-1442">Puesto que ninguna de las expresiones se pueden evaluar en tiempo de compilación, se notifica ningún error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="d7bba-1443">En tiempo de ejecución, el `F` método produce una `System.OverflowException`y el `G` método devuelve-727379968 (los 32 bits menores del resultado fuera de intervalo).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="d7bba-1444">El comportamiento de la `H` depende del método en el contexto para la compilación de comprobación de desbordamiento predeterminado, pero es el mismo que `F` o igual que `G`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="d7bba-1445">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-1445">In the example</span></span>
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
<span data-ttu-id="d7bba-1446">los desbordamientos que se producen al evaluar las expresiones constantes en `F` y `H` provocan errores de tiempo de compilación notificará porque las expresiones se evalúan en un `checked` contexto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="d7bba-1447">También se produce un desbordamiento al evaluar la expresión constante en `G`, pero dado que la evaluación realiza un `unchecked` contexto, no se notifica el desbordamiento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="d7bba-1448">El `checked` y `unchecked` sólo afectan a los operadores del contexto de las operaciones que están contenidas textualmente en comprobación de desbordamiento el "`(`"y"`)`" tokens.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="d7bba-1449">Los operadores no tienen ningún efecto en los miembros de función que se invocan como resultado de evaluar la expresión contenida.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="d7bba-1450">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-1450">In the example</span></span>
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
<span data-ttu-id="d7bba-1451">el uso de `checked` en `F` no afecta a la evaluación de `x * y` en `Multiply`, por lo que `x * y` se evalúa en el contexto de comprobación de desbordamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="d7bba-1452">El `unchecked` operador es conveniente al escribir constantes de los tipos enteros con signo en notación hexadecimal.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="d7bba-1453">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="d7bba-1454">Dos de las constantes hexadecimales anteriores son de tipo `uint`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="d7bba-1455">Dado que las constantes están fuera de la `int` intervalo, sin el `unchecked` operador, las conversiones a `int` producirían errores de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="d7bba-1456">El `checked` y `unchecked` operadores e instrucciones permiten a los programadores controlar ciertos aspectos de algunos cálculos numéricos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="d7bba-1457">Sin embargo, el comportamiento de algunos operadores numéricos depende de los tipos de datos de sus operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="d7bba-1458">Por ejemplo, la multiplicación de dos decimales siempre produce una excepción en caso de desbordamiento incluso dentro un explícitamente `unchecked` construir.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="d7bba-1459">De forma similar, multiplicar dos flota nunca los resultados de una excepción en caso de desbordamiento incluso dentro un explícitamente `checked` construir.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="d7bba-1460">Además, los otros operadores nunca se ven afectados por el modo de comprobación, ya sea predeterminada o explícita.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="d7bba-1461">Expresiones de valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="d7bba-1461">Default value expressions</span></span>

<span data-ttu-id="d7bba-1462">Una expresión de valor predeterminado se utiliza para obtener el valor predeterminado ([los valores predeterminados](variables.md#default-values)) de un tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="d7bba-1463">Normalmente se utiliza una expresión de valor predeterminado para los parámetros de tipo, ya que no puede conocerse si el parámetro de tipo es un tipo de valor o un tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="d7bba-1464">(No existe ninguna conversión desde el `null` literal a un parámetro de tipo a menos que el parámetro de tipo se conoce como un tipo de referencia.)</span><span class="sxs-lookup"><span data-stu-id="d7bba-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="d7bba-1465">Si el *tipo* en un *default_value_expression* evalúa en tiempo de ejecución a un tipo de referencia, el resultado es `null` convertir a ese tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="d7bba-1466">Si el *tipo* en un *default_value_expression* evalúa en tiempo de ejecución a un tipo de valor, el resultado es el *value_type*del valor predeterminado ([predeterminada constructores](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="d7bba-1467">Un *default_value_expression* es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)) si el tipo es un tipo de referencia o un parámetro de tipo que se sabe que es un tipo de referencia ([parámetro de tipo restricciones](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="d7bba-1468">Además, un *default_value_expression* es una expresión constante si el tipo es uno de los siguientes tipos de valor: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, o cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="d7bba-1469">Expresiones Nameof</span><span class="sxs-lookup"><span data-stu-id="d7bba-1469">Nameof expressions</span></span>

<span data-ttu-id="d7bba-1470">Un *nameof_expression* se usa para obtener el nombre de una entidad de programa como una constante de cadena.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

<span data-ttu-id="d7bba-1471">Gramaticalmente hablando, la *named_entity* operando siempre es una expresión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="d7bba-1472">Dado que `nameof` no es una palabra reservada, siempre es sintácticamente ambigua con una invocación del nombre sencillo de una expresión nameof `nameof`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="d7bba-1473">Por motivos de compatibilidad, si una búsqueda de nombres ([nombres simples](expressions.md#simple-names)) del nombre `nameof` se realiza correctamente, la expresión se trata como un *invocation_expression* , independientemente de si es la invocación legal.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="d7bba-1474">En caso contrario, es un *nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="d7bba-1475">El significado de la *named_entity* de un *nameof_expression* es el significado de la misma que una expresión; es decir, ya sea como un *simple_name*, un *base_access*  o un *member_access*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="d7bba-1476">Sin embargo, donde se describe la búsqueda en [nombres simples](expressions.md#simple-names) y [acceso a miembros](expressions.md#member-access) produce un error porque se encontró un miembro de instancia en un contexto estático, un *nameof_expression*no produce ningún error de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="d7bba-1477">Es un error en tiempo de compilación para un *named_entity* designar un grupo de métodos para tener un *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="d7bba-1478">Es un error en tiempo de compilación para un *named_entity_target* que el tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="d7bba-1479">Un *nameof_expression* es una expresión constante de tipo `string`, y no tiene ningún efecto en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="d7bba-1480">En concreto, su *named_entity* no se evalúa y se omite para los fines de análisis de asignación definitiva ([reglas generales para las expresiones simples](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="d7bba-1481">Su valor es el último identificador de la *named_entity* antes de la última opcional *type_argument_list*transformado en la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="d7bba-1482">El prefijo "`@`", si se usa, se quita.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="d7bba-1483">Cada *unicode_escape_sequence* se transforma en su carácter Unicode correspondiente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="d7bba-1484">Cualquier *formatting_characters* se quitan.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="d7bba-1485">Estas son las mismas transformaciones que se aplican en [identificadores](lexical-structure.md#identifiers) al probar la igualdad entre los identificadores.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="d7bba-1486">TODO: ejemplos</span><span class="sxs-lookup"><span data-stu-id="d7bba-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="d7bba-1487">Expresiones de métodos anónimos</span><span class="sxs-lookup"><span data-stu-id="d7bba-1487">Anonymous method expressions</span></span>

<span data-ttu-id="d7bba-1488">Un *anonymous_method_expression* es uno de dos maneras de definir una función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="d7bba-1489">Estos se describen en [expresiones de función anónima](expressions.md#anonymous-function-expressions).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="d7bba-1490">Operadores unarios</span><span class="sxs-lookup"><span data-stu-id="d7bba-1490">Unary operators</span></span>

<span data-ttu-id="d7bba-1491">El `?`, `+`, `-`, `!`, `~`, `++`, `--`, convierta, y `await` se denominan los operadores unarios.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

<span data-ttu-id="d7bba-1492">Si el operando de un *unary_expression* tiene el tipo de tiempo de compilación `dynamic`, se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="d7bba-1493">En este caso se escriba el tiempo de compilación de la *unary_expression* es `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución del operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="d7bba-1494">Operador condicional null</span><span class="sxs-lookup"><span data-stu-id="d7bba-1494">Null-conditional operator</span></span>

<span data-ttu-id="d7bba-1495">El operador condicional de null solo aplica una lista de operaciones para su operando si dicho operando no es null.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="d7bba-1496">En caso contrario, es el resultado de aplicar el operador `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1496">Otherwise the result of applying the operator is `null`.</span></span>

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="d7bba-1497">Puede incluir la lista de las operaciones de acceso a miembros y las operaciones de acceso de elemento (que pueden estar condicional de null), así como de invocación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="d7bba-1498">Por ejemplo, la expresión `a.b?[0]?.c()` es un *null_conditional_expression* con un *primary_expression* `a.b` y *null_conditional_operations* `?[0]` (acceso de elemento condicional de null), `?.c` (acceso condicional de null miembro) y `()` (invocación).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="d7bba-1499">Para un *null_conditional_expression* `E` con un *primary_expression* `P`, permiten `E0` la expresión obtenida extrayendo textualmente el interlineado `?`de cada uno de los *null_conditional_operations* de `E` que tiene uno.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="d7bba-1500">Conceptualmente, `E0` es la expresión que se evaluará si ninguna de las comprobaciones de null que representa el `?`s encontrar un `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="d7bba-1501">También, permiten `E1` la expresión obtenido quitando textualmente el interlineado `?` desde solamente el primero del *null_conditional_operations* en `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="d7bba-1502">Esto puede dar lugar a un *expresión primaria* (si hubiera una sola `?`) o a otro *null_conditional_expression*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="d7bba-1503">Por ejemplo, si `E` es la expresión `a.b?[0]?.c()`, a continuación, `E0` es la expresión `a.b[0].c()` y `E1` es la expresión `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="d7bba-1504">Si `E0` se clasifica como nada, a continuación, `E` se clasifica como nada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="d7bba-1505">En caso contrario, E está clasificado como un valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="d7bba-1506">`E0` y `E1` se usan para determinar el significado de `E`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="d7bba-1507">Si `E` se produce como un *statement_expression* el significado de `E` es el mismo que la instrucción</span><span class="sxs-lookup"><span data-stu-id="d7bba-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="d7bba-1508">salvo que P se evalúa solo una vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="d7bba-1509">De lo contrario, si `E0` se clasifica como nada se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="d7bba-1510">De lo contrario, deje que `T0` ser el tipo de `E0`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="d7bba-1511">Si `T0` es un parámetro de tipo que no se conoce como un tipo de referencia o un tipo de valor distinto de null, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="d7bba-1512">Si `T0` es un tipo de valor distinto de null, entonces el tipo de `E` es `T0?`y el significado de `E` es igual que</span><span class="sxs-lookup"><span data-stu-id="d7bba-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="d7bba-1513">salvo que `P` se evalúa solo una vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="d7bba-1514">En caso contrario, el tipo de E es T0, y el significado de E es igual a</span><span class="sxs-lookup"><span data-stu-id="d7bba-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="d7bba-1515">salvo que `P` se evalúa solo una vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="d7bba-1516">Si `E1` es en sí mismo un *null_conditional_expression*, a continuación, estas reglas se aplican de nuevo, el anidamiento de las pruebas para `null` hasta que no haya ningún otro `?`de, y se ha reducido la expresión hacia abajo en la expresión primaria `E0`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="d7bba-1517">Por ejemplo, si la expresión `a.b?[0]?.c()` se produce como una expresión de instrucción, como se muestra en la instrucción:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="d7bba-1518">es equivalente a su significado:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="d7bba-1519">nuevo que es equivalente a:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="d7bba-1520">Salvo que `a.b` y `a.b[0]` se evalúan una sola vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="d7bba-1521">Si se produce en un contexto donde se utiliza su valor, como en:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="d7bba-1522">y suponiendo que el tipo de la llamada final no es un tipo de valor distinto de null, es equivalente a su significado:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="d7bba-1523">Salvo que `a.b` y `a.b[0]` se evalúan una sola vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="d7bba-1524">Expresiones condicionales nulos como los inicializadores de proyección</span><span class="sxs-lookup"><span data-stu-id="d7bba-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="d7bba-1525">Solo se permite una expresión condicional de null como un *member_declarator* en un *anonymous_object_creation_expression* ([expresiones de creación de objeto anónimo](expressions.md#anonymous-object-creation-expressions)) si finaliza con un acceso de miembro (opcionalmente condicional de null).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="d7bba-1526">Gramaticalmente, este requisito puede expresarse como:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="d7bba-1527">Se trata de un caso especial de la gramática de *null_conditional_expression* anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="d7bba-1528">El entorno de producción *member_declarator* en [expresiones de creación de objeto anónimo](expressions.md#anonymous-object-creation-expressions) , a continuación, se incluye solo *null_conditional_member_access*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="d7bba-1529">Expresiones como expresiones de instrucción condicional de null</span><span class="sxs-lookup"><span data-stu-id="d7bba-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="d7bba-1530">Solo se permite una expresión condicional de null como un *statement_expression* ([las instrucciones de expresión](statements.md#expression-statements)) si termina con una invocación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="d7bba-1531">Gramaticalmente, este requisito puede expresarse como:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="d7bba-1532">Se trata de un caso especial de la gramática de *null_conditional_expression* anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="d7bba-1533">El entorno de producción *statement_expression* en [las instrucciones de expresión](statements.md#expression-statements) , a continuación, se incluye solo *null_conditional_invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="d7bba-1534">Operador unario más</span><span class="sxs-lookup"><span data-stu-id="d7bba-1534">Unary plus operator</span></span>

<span data-ttu-id="d7bba-1535">Para una operación del formulario `+x`, resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-1536">El operando se convierte al tipo de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="d7bba-1537">El unario más predefinida operadores son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="d7bba-1538">Para cada uno de estos operadores, el resultado es simplemente el valor del operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="d7bba-1539">Operador unario menos</span><span class="sxs-lookup"><span data-stu-id="d7bba-1539">Unary minus operator</span></span>

<span data-ttu-id="d7bba-1540">Para una operación del formulario `-x`, resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-1541">El operando se convierte al tipo de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="d7bba-1542">Los operadores de negación predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="d7bba-1543">Negación entero:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="d7bba-1544">El resultado se calcula restando `x` desde cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="d7bba-1545">Si el valor de `x` es el valor más pequeño que se puede representar el tipo de operando (-2 ^ 31 para `int` o -2 ^ 63 para `long`), a continuación, la negación matemática de `x` no es que se puede representar en el tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1545">If the value of of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="d7bba-1546">Si esto ocurre dentro de un `checked` contexto, un `System.OverflowException` se inicia; si se produce dentro de un `unchecked` contexto, el resultado es el valor del operando y no se notifica el desbordamiento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="d7bba-1547">Si el operando del operador de negación es de tipo `uint`, se convierte al tipo `long`, y el tipo del resultado es `long`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="d7bba-1548">Una excepción es la regla que permite el `int` valor entre -2147483648 (-2 ^ 31) se deben escribir como un literal entero decimal ([literales enteros](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="d7bba-1549">Si el operando del operador de negación es de tipo `ulong`, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="d7bba-1550">Una excepción es la regla que permite el `long` valor -9223372036854775808 (-2 ^ 63) se deben escribir como un literal entero decimal ([literales enteros](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="d7bba-1551">Negación de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="d7bba-1552">El resultado es el valor de `x` con su signo invertido.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="d7bba-1553">Si `x` es NaN, el resultado también es NaN.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="d7bba-1554">Negación decimal:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="d7bba-1555">El resultado se calcula restando `x` desde cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="d7bba-1556">La negación decimal equivale a usar el operador unario menos de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="d7bba-1557">Operador lógico de negación</span><span class="sxs-lookup"><span data-stu-id="d7bba-1557">Logical negation operator</span></span>

<span data-ttu-id="d7bba-1558">Para una operación del formulario `!x`, resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-1559">El operando se convierte al tipo de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="d7bba-1560">Existe solo un operador de negación lógica predefinidos:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="d7bba-1561">Este operador calcula la negación lógica del operando: Si el operando es `true`, el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="d7bba-1562">Si el operando es `false`, el resultado es `true`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="d7bba-1563">Operador de complemento bit a bit</span><span class="sxs-lookup"><span data-stu-id="d7bba-1563">Bitwise complement operator</span></span>

<span data-ttu-id="d7bba-1564">Para una operación del formulario `~x`, resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-1565">El operando se convierte al tipo de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="d7bba-1566">Los operadores de complemento bit a bit predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="d7bba-1567">Para cada uno de estos operadores, el resultado de la operación es el complemento bit a bit de `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="d7bba-1568">Cada tipo de enumeración `E` proporciona implícitamente el siguiente operador de complemento bit a bit:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="d7bba-1569">El resultado de evaluar `~x`, donde `x` es una expresión de un tipo de enumeración `E` con un tipo subyacente `U`, es exactamente el mismo que el de evaluar `(E)(~(U)x)`, salvo que la conversión a `E` es siempre se realiza como si se encuentra en un `unchecked` contexto ([los operadores checked y unchecked](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="d7bba-1570">Prefijo de incremento y decremento de operadores</span><span class="sxs-lookup"><span data-stu-id="d7bba-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="d7bba-1571">El operando de un prefijo de incremento o decremento operación debe ser una expresión que se clasifica como una variable, el acceso a una propiedad o un acceso de indizador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="d7bba-1572">El resultado de la operación es un valor del mismo tipo como operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="d7bba-1573">Si el operando de un prefijo de incremento o decremento operación es una propiedad o acceso de indizador, la propiedad o indizador debe tenerlos un `get` y un `set` descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="d7bba-1574">Si esto no es el caso, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-1575">Resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-1576">Predefinidos `++` y `--` operadores existen para los siguientes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`y cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="d7bba-1577">Predefinido `++` operadores devuelven el valor generado al sumar 1 al operando y predefinido `--` operadores devuelven el valor generado al restar 1 del operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="d7bba-1578">En un `checked` contexto, si el resultado de esta suma o resta está fuera del intervalo del tipo del resultado y el tipo de resultado es un tipo entero o un tipo de enumeración, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="d7bba-1579">El procesamiento en tiempo de ejecución de un prefijo de incremento o decremento de la operación de la forma `++x` o `--x` consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="d7bba-1580">Si `x` se clasifica como una variable:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="d7bba-1581">`x` se evalúa para producir la variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="d7bba-1582">Se invoca el operador seleccionado con el valor de `x` como su argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="d7bba-1583">El valor devuelto por el operador se almacena en la ubicación dada por la evaluación de `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="d7bba-1584">El valor devuelto por el operador se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="d7bba-1585">Si `x` se clasifica como un acceso de propiedad o indizador:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="d7bba-1586">La expresión de instancia (si `x` no `static`) y la lista de argumentos (si `x` es un acceso de indizador) asociadas con `x` se evalúan, y los resultados se usan en la siguiente `get` y `set` invocaciones de descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="d7bba-1587">El `get` descriptor de acceso de `x` se invoca.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="d7bba-1588">Se invoca el operador seleccionado con el valor devuelto por la `get` descriptor de acceso como su argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="d7bba-1589">El `set` descriptor de acceso de `x` se invoca con el valor devuelto por el operador como su `value` argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="d7bba-1590">El valor devuelto por el operador se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="d7bba-1591">El `++` y `--` operadores también admiten postfijo notación ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="d7bba-1592">Normalmente, el resultado de `x++` o `x--` es el valor de `x` antes de la operación, mientras que el resultado de `++x` o `--x` es el valor de `x` después de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="d7bba-1593">En cualquier caso, `x` sí tiene el mismo valor después de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="d7bba-1594">Un `operator++` o `operator--` implementación puede invocarse mediante la notación de prefijo o sufijo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="d7bba-1595">No es posible tener implementaciones de operador independientes para las dos notaciones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="d7bba-1596">Expresiones de conversión</span><span class="sxs-lookup"><span data-stu-id="d7bba-1596">Cast expressions</span></span>

<span data-ttu-id="d7bba-1597">Un *cast_expression* se usa para convertir explícitamente una expresión a un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="d7bba-1598">Un *cast_expression* del formulario `(T)E`, donde `T` es un *tipo* y `E` es un *unary_expression*, realiza una explícita conversión ([las conversiones explícitas](conversions.md#explicit-conversions)) del valor de `E` escriba `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="d7bba-1599">Si no existe ninguna conversión explícita de `E` a `T`, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="d7bba-1600">En caso contrario, el resultado es el valor producido por la conversión explícita.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="d7bba-1601">El resultado siempre se clasifica como un valor, aunque `E` denota una variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="d7bba-1602">La gramática de una *cast_expression* conduce a ciertas ambigüedades sintácticas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="d7bba-1603">Por ejemplo, la expresión `(x)-y` se cualquiera puede interpretar como un *cast_expression* (una conversión de `-y` escriba `x`) o como un *additive_expression* combinado con un *parenthesized_expression* (que calcula el valor `x - y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="d7bba-1604">Para resolver *cast_expression* ambigüedades, la siguiente regla existe: Una secuencia de uno o varios *token*s ([espacio en blanco](lexical-structure.md#white-space)) entre paréntesis se considera el principio de un *cast_expression* sólo si al menos uno de los siguientes es verdaderas:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="d7bba-1605">La secuencia de símbolos es correcta gramaticalmente para un *tipo*, pero no para un *expresión*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="d7bba-1606">La secuencia de símbolos es correcta gramaticalmente para un *tipo*, y el token sigue inmediatamente al paréntesis de cierre es el token "`~`", el token "`!`", el token "`(`", un  *identificador* ([secuencias de escape de caracteres Unicode](lexical-structure.md#unicode-character-escape-sequences)), un *literal* ([literales](lexical-structure.md#literals)), o cualquier *palabra clave*([Palabras clave](lexical-structure.md#keywords)) excepto `as` y `is`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="d7bba-1607">El término "gramaticalmente correcto" significa que la secuencia de tokens debe ajustarse a la producción gramatical particular.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="d7bba-1608">En concreto no considera el significado real de sus identificadores constituyentes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="d7bba-1609">Por ejemplo, si `x` y `y` son identificadores, a continuación, `x.y` es gramática correcto para un tipo, incluso si `x.y` realmente no denota un tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="d7bba-1610">Desde la regla de desambiguación se deduce que, si `x` y `y` son identificadores, `(x)y`, `(x)(y)`, y `(x)(-y)` son *cast_expression*s, pero `(x)-y` no lo es, incluso si `x` identifica un tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="d7bba-1611">Sin embargo, si `x` es una palabra clave que identifica un tipo predefinido (como `int`), a continuación, las cuatro formas son *cast_expression*s (porque esta palabra clave no podría ser una expresión por sí mismo).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="d7bba-1612">Expresiones await</span><span class="sxs-lookup"><span data-stu-id="d7bba-1612">Await expressions</span></span>

<span data-ttu-id="d7bba-1613">Se usa el operador await para suspender la evaluación de la función async envolvente hasta que se ha completado la operación asincrónica representada por el operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="d7bba-1614">Un *await_expression* solo se permite en el cuerpo de una función asincrónica ([iteradores](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="d7bba-1615">En la envolvente más próximo función asincrónica, un *await_expression* puede no producirse en estos lugares:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="d7bba-1616">Dentro de una función anónima (no-asincrónico anidadas)</span><span class="sxs-lookup"><span data-stu-id="d7bba-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="d7bba-1617">Dentro del bloque de un *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="d7bba-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="d7bba-1618">En un contexto no seguro</span><span class="sxs-lookup"><span data-stu-id="d7bba-1618">In an unsafe context</span></span>

<span data-ttu-id="d7bba-1619">Tenga en cuenta que un *await_expression* no puede aparecer en la mayoría de los lugares dentro de un *expresión_de_consulta*, porque los sintácticamente se transforman para usar las expresiones lambda no son asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="d7bba-1620">Dentro de una función asincrónica, `await` no se puede usar como identificador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="d7bba-1621">Por lo tanto, no hay ninguna ambigüedad sintáctica entre expresiones await y diversas expresiones que implican los identificadores.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="d7bba-1622">Fuera de las funciones asincrónicas, `await` actúa como un identificador normal.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="d7bba-1623">El operando de un *await_expression* se denomina el ***tarea***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="d7bba-1624">Representa una operación asincrónica que puede o no estar completa en el momento en el *await_expression* se evalúa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="d7bba-1625">Es el propósito del operador await suspender la ejecución de la función async envolvente hasta que se complete la tarea esperada y, a continuación, obtener su resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="d7bba-1626">Expresiones que admite await</span><span class="sxs-lookup"><span data-stu-id="d7bba-1626">Awaitable expressions</span></span>

<span data-ttu-id="d7bba-1627">La tarea de una expresión await tiene que ser ***esperable***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="d7bba-1628">Una expresión `t` es esperable si uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="d7bba-1629">`t` es de tipo en tiempo de compilación `dynamic`</span><span class="sxs-lookup"><span data-stu-id="d7bba-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="d7bba-1630">`t` tiene un método de extensión o de instancia accesible denominado `GetAwaiter` con un tipo de valor devuelto y ningún parámetro de tipo y sin parámetros `A` para el que se mantenga todos los elementos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="d7bba-1631">`A` implementa la interfaz `System.Runtime.CompilerServices.INotifyCompletion` (aquí en adelante conocidas como `INotifyCompletion` por razones de brevedad)</span><span class="sxs-lookup"><span data-stu-id="d7bba-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="d7bba-1632">`A` tiene una propiedad de instancia accesible y legible `IsCompleted` de tipo `bool`</span><span class="sxs-lookup"><span data-stu-id="d7bba-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="d7bba-1633">`A` tiene un método de instancia accesible `GetResult` sin parámetros ni ningún parámetro de tipo</span><span class="sxs-lookup"><span data-stu-id="d7bba-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="d7bba-1634">El propósito de la `GetAwaiter` método consiste en obtener una ***awaiter*** para la tarea.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="d7bba-1635">El tipo `A` se denomina el ***tipo awaiter*** para la expresión await.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="d7bba-1636">El propósito de la `IsCompleted` propiedad consiste en determinar si la tarea ya está completa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="d7bba-1637">Si es así, no es necesario para suspender la evaluación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="d7bba-1638">El propósito de la `INotifyCompletion.OnCompleted` método consiste en registrarse "continuación" a la tarea; es decir, un delegado (de tipo `System.Action`) que se invocará una vez completada la tarea.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="d7bba-1639">El propósito de la `GetResult` método consiste en obtener el resultado de la tarea cuando haya terminado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="d7bba-1640">Este resultado puede ser la finalización correcta, posiblemente con un valor de resultado, o puede ser una excepción que produce el `GetResult` método.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="d7bba-1641">Clasificación de expresiones await</span><span class="sxs-lookup"><span data-stu-id="d7bba-1641">Classification of await expressions</span></span>

<span data-ttu-id="d7bba-1642">La expresión `await t` se clasifican del mismo modo que la expresión `(t).GetAwaiter().GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="d7bba-1643">Por lo tanto, si el tipo de valor devuelto de `GetResult` es `void`, *await_expression* se clasifica como nada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="d7bba-1644">Si tiene un tipo de valor devuelto distinto de void `T`, *await_expression* se clasifica como un valor de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="d7bba-1645">En tiempo de ejecución evaluación de expresiones await</span><span class="sxs-lookup"><span data-stu-id="d7bba-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="d7bba-1646">En tiempo de ejecución, la expresión `await t` se evalúa como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="d7bba-1647">Un awaiter `a` se obtiene al evaluar la expresión `(t).GetAwaiter()`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="d7bba-1648">Un `bool` `b` se obtiene al evaluar la expresión `(a).IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="d7bba-1649">Si `b` es `false` , a continuación, evaluación depende de si `a` implementa la interfaz `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (aquí en adelante conocidas como `ICriticalNotifyCompletion` por razones de brevedad).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="d7bba-1650">Esta comprobación se realiza en tiempo; de enlace es decir, en tiempo de ejecución si `a` tiene el tipo de tiempo de compilación `dynamic`y en tiempo de compilación en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="d7bba-1651">Permiten `r` denotan el delegado de reanudación ([iteradores](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="d7bba-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="d7bba-1652">Si `a` no implementa `ICriticalNotifyCompletion`, a continuación, la expresión `(a as (INotifyCompletion)).OnCompleted(r)` se evalúa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="d7bba-1653">Si `a` neimplementuje `ICriticalNotifyCompletion`, a continuación, la expresión `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` se evalúa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="d7bba-1654">A continuación, se suspende la evaluación y control se devuelve al llamador actual de la función async.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="d7bba-1655">Ya sea inmediatamente después de (si `b` era `true`), o al invocar el delegado de reanudación posterior (si `b` era `false`), la expresión `(a).GetResult()` se evalúa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="d7bba-1656">Si devuelve un valor, ese valor es el resultado de la *await_expression*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="d7bba-1657">En caso contrario, el resultado es nothing.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="d7bba-1658">Implementación de un factor en espera de los métodos de interfaz `INotifyCompletion.OnCompleted` y `ICriticalNotifyCompletion.UnsafeOnCompleted` provocaría que el delegado `r` invocarse como máximo una vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="d7bba-1659">En caso contrario, el comportamiento de la función envolvente de async es indefinido.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="d7bba-1660">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="d7bba-1660">Arithmetic operators</span></span>

<span data-ttu-id="d7bba-1661">El `*`, `/`, `%`, `+`, y `-` se denominan los operadores aritméticos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

<span data-ttu-id="d7bba-1662">Si un operando de un operador aritmético tiene el tipo de tiempo de compilación `dynamic`, a continuación, la expresión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="d7bba-1663">En este caso es el tipo de tiempo de compilación de la expresión `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución de los operandos que tienen el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="d7bba-1664">Operador de multiplicación</span><span class="sxs-lookup"><span data-stu-id="d7bba-1664">Multiplication operator</span></span>

<span data-ttu-id="d7bba-1665">Para una operación del formulario `x * y`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-1666">Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="d7bba-1667">A continuación, se enumeran los operadores de multiplicación predefinidos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="d7bba-1668">Todos los operadores calculan el producto de `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="d7bba-1669">Multiplicación de enteros:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="d7bba-1670">En un `checked` contexto, si el producto está fuera del intervalo del tipo del resultado, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="d7bba-1671">En un `unchecked` contexto, no se notifican los desbordamientos y se descartan los bits de orden superior significativa fuera del intervalo del tipo del resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="d7bba-1672">Multiplicación de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="d7bba-1673">El producto se calcula según las reglas aritméticas de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="d7bba-1674">En la tabla siguiente se muestra los resultados de todas las posibles combinaciones de valores finitos distinto de cero, ceros, valores infinitos y NaN.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="d7bba-1675">En la tabla, `x` y `y` son los valores positivos finitos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="d7bba-1676">`z` es el resultado de `x * y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="d7bba-1677">Si el resultado es demasiado grande para el tipo de destino, `z` es infinito.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="d7bba-1678">Si el resultado es demasiado pequeño para el tipo de destino, `z` es cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="d7bba-1679">+ y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1679">+y</span></span>   | <span data-ttu-id="d7bba-1680">-y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1680">-y</span></span>   | <span data-ttu-id="d7bba-1681">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1681">+0</span></span>  | <span data-ttu-id="d7bba-1682">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1682">-0</span></span>  | <span data-ttu-id="d7bba-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1683">+inf</span></span> | <span data-ttu-id="d7bba-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1684">-inf</span></span> | <span data-ttu-id="d7bba-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1685">NaN</span></span> | 
   | <span data-ttu-id="d7bba-1686">+x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1686">+x</span></span>   | <span data-ttu-id="d7bba-1687">+z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1687">+z</span></span>   | <span data-ttu-id="d7bba-1688">-z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1688">-z</span></span>   | <span data-ttu-id="d7bba-1689">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1689">+0</span></span>  | <span data-ttu-id="d7bba-1690">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1690">-0</span></span>  | <span data-ttu-id="d7bba-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1691">+inf</span></span> | <span data-ttu-id="d7bba-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1692">-inf</span></span> | <span data-ttu-id="d7bba-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1693">NaN</span></span> | 
   | <span data-ttu-id="d7bba-1694">-x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1694">-x</span></span>   | <span data-ttu-id="d7bba-1695">-z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1695">-z</span></span>   | <span data-ttu-id="d7bba-1696">+z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1696">+z</span></span>   | <span data-ttu-id="d7bba-1697">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1697">-0</span></span>  | <span data-ttu-id="d7bba-1698">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1698">+0</span></span>  | <span data-ttu-id="d7bba-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1699">-inf</span></span> | <span data-ttu-id="d7bba-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1700">+inf</span></span> | <span data-ttu-id="d7bba-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1701">NaN</span></span> | 
   | <span data-ttu-id="d7bba-1702">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1702">+0</span></span>   | <span data-ttu-id="d7bba-1703">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1703">+0</span></span>   | <span data-ttu-id="d7bba-1704">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1704">-0</span></span>   | <span data-ttu-id="d7bba-1705">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1705">+0</span></span>  | <span data-ttu-id="d7bba-1706">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1706">-0</span></span>  | <span data-ttu-id="d7bba-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1707">NaN</span></span>  | <span data-ttu-id="d7bba-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1708">NaN</span></span>  | <span data-ttu-id="d7bba-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1709">NaN</span></span> | 
   | <span data-ttu-id="d7bba-1710">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1710">-0</span></span>   | <span data-ttu-id="d7bba-1711">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1711">-0</span></span>   | <span data-ttu-id="d7bba-1712">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1712">+0</span></span>   | <span data-ttu-id="d7bba-1713">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1713">-0</span></span>  | <span data-ttu-id="d7bba-1714">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1714">+0</span></span>  | <span data-ttu-id="d7bba-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1715">NaN</span></span>  | <span data-ttu-id="d7bba-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1716">NaN</span></span>  | <span data-ttu-id="d7bba-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1717">NaN</span></span> | 
   | <span data-ttu-id="d7bba-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1718">+inf</span></span> | <span data-ttu-id="d7bba-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1719">+inf</span></span> | <span data-ttu-id="d7bba-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1720">-inf</span></span> | <span data-ttu-id="d7bba-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1721">NaN</span></span> | <span data-ttu-id="d7bba-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1722">NaN</span></span> | <span data-ttu-id="d7bba-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1723">+inf</span></span> | <span data-ttu-id="d7bba-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1724">-inf</span></span> | <span data-ttu-id="d7bba-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1725">NaN</span></span> | 
   | <span data-ttu-id="d7bba-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1726">-inf</span></span> | <span data-ttu-id="d7bba-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1727">-inf</span></span> | <span data-ttu-id="d7bba-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1728">+inf</span></span> | <span data-ttu-id="d7bba-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1729">NaN</span></span> | <span data-ttu-id="d7bba-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1730">NaN</span></span> | <span data-ttu-id="d7bba-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1731">-inf</span></span> | <span data-ttu-id="d7bba-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1732">+inf</span></span> | <span data-ttu-id="d7bba-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1733">NaN</span></span> | 
   | <span data-ttu-id="d7bba-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1734">NaN</span></span>  | <span data-ttu-id="d7bba-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1735">NaN</span></span>  | <span data-ttu-id="d7bba-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1736">NaN</span></span>  | <span data-ttu-id="d7bba-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1737">NaN</span></span> | <span data-ttu-id="d7bba-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1738">NaN</span></span> | <span data-ttu-id="d7bba-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1739">NaN</span></span>  | <span data-ttu-id="d7bba-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1740">NaN</span></span>  | <span data-ttu-id="d7bba-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1741">NaN</span></span> | 

*  <span data-ttu-id="d7bba-1742">Multiplicación de decimales:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="d7bba-1743">Si el valor resultante es demasiado grande para representarlo en el `decimal` formato, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="d7bba-1744">Si el valor del resultado es demasiado pequeño para representarlo en el `decimal` formato, el resultado es cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="d7bba-1745">La escala del resultado, antes de cualquier operación de redondeo, es la suma de las escalas de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="d7bba-1746">La multiplicación de decimales es equivalente a usar el operador de multiplicación de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="d7bba-1747">Operador de división</span><span class="sxs-lookup"><span data-stu-id="d7bba-1747">Division operator</span></span>

<span data-ttu-id="d7bba-1748">Para una operación del formulario `x / y`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-1749">Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="d7bba-1750">A continuación, se enumeran los operadores de división predefinidos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="d7bba-1751">Todos los operadores calculan el cociente de `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="d7bba-1752">División de enteros:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="d7bba-1753">Si el valor del operando derecho es cero, una `System.DivideByZeroException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="d7bba-1754">La división redondea el resultado hacia cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="d7bba-1755">Por lo tanto, el valor absoluto del resultado es el entero más grande posible que sea menor o igual al valor absoluto del cociente de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="d7bba-1756">El resultado es cero o positivo cuando los dos operandos tienen el mismo signo y cero o negativo si los dos operandos tienen signos opuestos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="d7bba-1757">Si el operando izquierdo es el más pequeño que se puede representar `int` o `long` valor y el operando derecho es `-1`, se produce un desbordamiento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="d7bba-1758">En un `checked` contexto, esto hace que un `System.ArithmeticException` (o una subclase de ella) que se produzca.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="d7bba-1759">En un `unchecked` contexto está definido por la implementación si un `System.ArithmeticException` (o una subclase de ella) se inicia o se informa del desbordamiento con el valor resultante es que el del operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="d7bba-1760">División de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="d7bba-1761">Se calcula el cociente según las reglas aritméticas de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="d7bba-1762">En la tabla siguiente se muestra los resultados de todas las posibles combinaciones de valores finitos distinto de cero, ceros, valores infinitos y NaN.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="d7bba-1763">En la tabla, `x` y `y` son los valores positivos finitos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="d7bba-1764">`z` es el resultado de `x / y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="d7bba-1765">Si el resultado es demasiado grande para el tipo de destino, `z` es infinito.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="d7bba-1766">Si el resultado es demasiado pequeño para el tipo de destino, `z` es cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="d7bba-1767">+ y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1767">+y</span></span>   | <span data-ttu-id="d7bba-1768">-y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1768">-y</span></span>   | <span data-ttu-id="d7bba-1769">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1769">+0</span></span>   | <span data-ttu-id="d7bba-1770">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1770">-0</span></span>   | <span data-ttu-id="d7bba-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1771">+inf</span></span> | <span data-ttu-id="d7bba-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1772">-inf</span></span> | <span data-ttu-id="d7bba-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1773">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1774">+x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1774">+x</span></span>   | <span data-ttu-id="d7bba-1775">+z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1775">+z</span></span>   | <span data-ttu-id="d7bba-1776">-z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1776">-z</span></span>   | <span data-ttu-id="d7bba-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1777">+inf</span></span> | <span data-ttu-id="d7bba-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1778">-inf</span></span> | <span data-ttu-id="d7bba-1779">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1779">+0</span></span>   | <span data-ttu-id="d7bba-1780">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1780">-0</span></span>   | <span data-ttu-id="d7bba-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1781">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1782">-x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1782">-x</span></span>   | <span data-ttu-id="d7bba-1783">-z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1783">-z</span></span>   | <span data-ttu-id="d7bba-1784">+z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1784">+z</span></span>   | <span data-ttu-id="d7bba-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1785">-inf</span></span> | <span data-ttu-id="d7bba-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1786">+inf</span></span> | <span data-ttu-id="d7bba-1787">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1787">-0</span></span>   | <span data-ttu-id="d7bba-1788">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1788">+0</span></span>   | <span data-ttu-id="d7bba-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1789">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1790">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1790">+0</span></span>   | <span data-ttu-id="d7bba-1791">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1791">+0</span></span>   | <span data-ttu-id="d7bba-1792">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1792">-0</span></span>   | <span data-ttu-id="d7bba-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1793">NaN</span></span>  | <span data-ttu-id="d7bba-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1794">NaN</span></span>  | <span data-ttu-id="d7bba-1795">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1795">+0</span></span>   | <span data-ttu-id="d7bba-1796">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1796">-0</span></span>   | <span data-ttu-id="d7bba-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1797">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1798">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1798">-0</span></span>   | <span data-ttu-id="d7bba-1799">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1799">-0</span></span>   | <span data-ttu-id="d7bba-1800">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1800">+0</span></span>   | <span data-ttu-id="d7bba-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1801">NaN</span></span>  | <span data-ttu-id="d7bba-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1802">NaN</span></span>  | <span data-ttu-id="d7bba-1803">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1803">-0</span></span>   | <span data-ttu-id="d7bba-1804">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1804">+0</span></span>   | <span data-ttu-id="d7bba-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1805">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1806">+inf</span></span> | <span data-ttu-id="d7bba-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1807">+inf</span></span> | <span data-ttu-id="d7bba-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1808">-inf</span></span> | <span data-ttu-id="d7bba-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1809">+inf</span></span> | <span data-ttu-id="d7bba-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1810">-inf</span></span> | <span data-ttu-id="d7bba-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1811">NaN</span></span>  | <span data-ttu-id="d7bba-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1812">NaN</span></span>  | <span data-ttu-id="d7bba-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1813">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1814">-inf</span></span> | <span data-ttu-id="d7bba-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1815">-inf</span></span> | <span data-ttu-id="d7bba-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1816">+inf</span></span> | <span data-ttu-id="d7bba-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1817">-inf</span></span> | <span data-ttu-id="d7bba-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1818">+inf</span></span> | <span data-ttu-id="d7bba-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1819">NaN</span></span>  | <span data-ttu-id="d7bba-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1820">NaN</span></span>  | <span data-ttu-id="d7bba-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1821">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1822">NaN</span></span>  | <span data-ttu-id="d7bba-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1823">NaN</span></span>  | <span data-ttu-id="d7bba-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1824">NaN</span></span>  | <span data-ttu-id="d7bba-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1825">NaN</span></span>  | <span data-ttu-id="d7bba-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1826">NaN</span></span>  | <span data-ttu-id="d7bba-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1827">NaN</span></span>  | <span data-ttu-id="d7bba-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1828">NaN</span></span>  | <span data-ttu-id="d7bba-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1829">NaN</span></span>  | 

*  <span data-ttu-id="d7bba-1830">División decimal:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="d7bba-1831">Si el valor del operando derecho es cero, una `System.DivideByZeroException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="d7bba-1832">Si el valor resultante es demasiado grande para representarlo en el `decimal` formato, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="d7bba-1833">Si el valor del resultado es demasiado pequeño para representarlo en el `decimal` formato, el resultado es cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="d7bba-1834">La escala del resultado es el menor escala que conserve un resultado igual que el más próximo al valor decimal que se puede representar en el resultado matemático es true.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="d7bba-1835">División decimal es equivalente a usar el operador de división de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="d7bba-1836">Operador de resto</span><span class="sxs-lookup"><span data-stu-id="d7bba-1836">Remainder operator</span></span>

<span data-ttu-id="d7bba-1837">Para una operación del formulario `x % y`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-1838">Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="d7bba-1839">Los operadores predefinidos resto se enumeran a continuación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="d7bba-1840">Todos los operadores compute el resto de la división entre `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="d7bba-1841">Resto entero:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="d7bba-1842">El resultado de `x % y` es el valor producido por `x - (x / y) * y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="d7bba-1843">Si `y` es cero, una `System.DivideByZeroException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="d7bba-1844">Si el operando izquierdo es el más pequeño `int` o `long` valor y el operando derecho es `-1`, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="d7bba-1845">En ningún caso hace `x % y` produce una excepción donde `x / y` no produciría una excepción.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="d7bba-1846">Resto de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="d7bba-1847">En la tabla siguiente se muestra los resultados de todas las posibles combinaciones de valores finitos distinto de cero, ceros, valores infinitos y NaN.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="d7bba-1848">En la tabla, `x` y `y` son los valores positivos finitos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="d7bba-1849">`z` es el resultado de `x % y` y se calcula como `x - n * y`, donde `n` es el entero más grande posible que sea menor o igual que `x / y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="d7bba-1850">Este método para calcular el resto es análogo a la utilizada para los operandos enteros, pero difiere de la definición de IEEE 754 (en el que `n` es el entero más cercano al `x / y`).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="d7bba-1851">+ y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1851">+y</span></span>   | <span data-ttu-id="d7bba-1852">-y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1852">-y</span></span>   | <span data-ttu-id="d7bba-1853">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1853">+0</span></span>   | <span data-ttu-id="d7bba-1854">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1854">-0</span></span>   | <span data-ttu-id="d7bba-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1855">+inf</span></span> | <span data-ttu-id="d7bba-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1856">-inf</span></span> | <span data-ttu-id="d7bba-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1857">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1858">+x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1858">+x</span></span>   | <span data-ttu-id="d7bba-1859">+z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1859">+z</span></span>   | <span data-ttu-id="d7bba-1860">+z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1860">+z</span></span>   | <span data-ttu-id="d7bba-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1861">NaN</span></span>  | <span data-ttu-id="d7bba-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1862">NaN</span></span>  | <span data-ttu-id="d7bba-1863">x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1863">x</span></span>    | <span data-ttu-id="d7bba-1864">x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1864">x</span></span>    | <span data-ttu-id="d7bba-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1865">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1866">-x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1866">-x</span></span>   | <span data-ttu-id="d7bba-1867">-z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1867">-z</span></span>   | <span data-ttu-id="d7bba-1868">-z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1868">-z</span></span>   | <span data-ttu-id="d7bba-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1869">NaN</span></span>  | <span data-ttu-id="d7bba-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1870">NaN</span></span>  | <span data-ttu-id="d7bba-1871">-x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1871">-x</span></span>   | <span data-ttu-id="d7bba-1872">-x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1872">-x</span></span>   | <span data-ttu-id="d7bba-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1873">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1874">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1874">+0</span></span>   | <span data-ttu-id="d7bba-1875">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1875">+0</span></span>   | <span data-ttu-id="d7bba-1876">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1876">+0</span></span>   | <span data-ttu-id="d7bba-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1877">NaN</span></span>  | <span data-ttu-id="d7bba-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1878">NaN</span></span>  | <span data-ttu-id="d7bba-1879">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1879">+0</span></span>   | <span data-ttu-id="d7bba-1880">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1880">+0</span></span>   | <span data-ttu-id="d7bba-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1881">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1882">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1882">-0</span></span>   | <span data-ttu-id="d7bba-1883">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1883">-0</span></span>   | <span data-ttu-id="d7bba-1884">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1884">-0</span></span>   | <span data-ttu-id="d7bba-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1885">NaN</span></span>  | <span data-ttu-id="d7bba-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1886">NaN</span></span>  | <span data-ttu-id="d7bba-1887">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1887">-0</span></span>   | <span data-ttu-id="d7bba-1888">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1888">-0</span></span>   | <span data-ttu-id="d7bba-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1889">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1890">+inf</span></span> | <span data-ttu-id="d7bba-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1891">NaN</span></span>  | <span data-ttu-id="d7bba-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1892">NaN</span></span>  | <span data-ttu-id="d7bba-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1893">NaN</span></span>  | <span data-ttu-id="d7bba-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1894">NaN</span></span>  | <span data-ttu-id="d7bba-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1895">NaN</span></span>  | <span data-ttu-id="d7bba-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1896">NaN</span></span>  | <span data-ttu-id="d7bba-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1897">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1898">-inf</span></span> | <span data-ttu-id="d7bba-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1899">NaN</span></span>  | <span data-ttu-id="d7bba-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1900">NaN</span></span>  | <span data-ttu-id="d7bba-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1901">NaN</span></span>  | <span data-ttu-id="d7bba-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1902">NaN</span></span>  | <span data-ttu-id="d7bba-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1903">NaN</span></span>  | <span data-ttu-id="d7bba-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1904">NaN</span></span>  | <span data-ttu-id="d7bba-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1905">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1906">NaN</span></span>  | <span data-ttu-id="d7bba-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1907">NaN</span></span>  | <span data-ttu-id="d7bba-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1908">NaN</span></span>  | <span data-ttu-id="d7bba-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1909">NaN</span></span>  | <span data-ttu-id="d7bba-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1910">NaN</span></span>  | <span data-ttu-id="d7bba-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1911">NaN</span></span>  | <span data-ttu-id="d7bba-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1912">NaN</span></span>  | <span data-ttu-id="d7bba-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1913">NaN</span></span>  | 

*  <span data-ttu-id="d7bba-1914">Resto decimal:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="d7bba-1915">Si el valor del operando derecho es cero, una `System.DivideByZeroException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="d7bba-1916">La escala del resultado, antes de cualquier operación de redondeo, es el mayor de las escalas de los dos operandos y el signo del resultado, si es distinto de cero, es el mismo que el de `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="d7bba-1917">El resto decimal es equivalente a usar el operador de resto de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="d7bba-1918">Operador de suma</span><span class="sxs-lookup"><span data-stu-id="d7bba-1918">Addition operator</span></span>

<span data-ttu-id="d7bba-1919">Para una operación del formulario `x + y`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-1920">Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="d7bba-1921">A continuación, se muestran los operadores de suma predefinidos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="d7bba-1922">Para los tipos numéricos y de enumeración, los operadores de suma predefinidos calculan la suma de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="d7bba-1923">Cuando uno o ambos operandos son de tipo cadena, los operadores de suma predefinidos concatenan la representación de cadena de los operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="d7bba-1924">Adición de números enteros:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="d7bba-1925">En un `checked` contexto, si la suma está fuera del intervalo del tipo del resultado, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="d7bba-1926">En un `unchecked` contexto, no se notifican los desbordamientos y se descartan los bits de orden superior significativa fuera del intervalo del tipo del resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="d7bba-1927">Adición de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="d7bba-1928">La suma se calcula según las reglas aritméticas de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="d7bba-1929">En la tabla siguiente se muestra los resultados de todas las posibles combinaciones de valores finitos distinto de cero, ceros, valores infinitos y NaN.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="d7bba-1930">En la tabla, `x` y `y` son valores finitos distinto de cero, y `z` es el resultado de `x + y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="d7bba-1931">Si `x` y `y` tienen la misma magnitud pero signos opuestos, `z` es cero positivo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="d7bba-1932">Si `x + y` es demasiado grande para representarlo en el tipo de destino, `z` es infinito con el mismo signo que `x + y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="d7bba-1933">y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1933">y</span></span>    | <span data-ttu-id="d7bba-1934">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1934">+0</span></span>   | <span data-ttu-id="d7bba-1935">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1935">-0</span></span>   | <span data-ttu-id="d7bba-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1936">+inf</span></span> | <span data-ttu-id="d7bba-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1937">-inf</span></span> | <span data-ttu-id="d7bba-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1938">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1939">x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1939">x</span></span>    | <span data-ttu-id="d7bba-1940">z</span><span class="sxs-lookup"><span data-stu-id="d7bba-1940">z</span></span>    | <span data-ttu-id="d7bba-1941">x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1941">x</span></span>    | <span data-ttu-id="d7bba-1942">x</span><span class="sxs-lookup"><span data-stu-id="d7bba-1942">x</span></span>    | <span data-ttu-id="d7bba-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1943">+inf</span></span> | <span data-ttu-id="d7bba-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1944">-inf</span></span> | <span data-ttu-id="d7bba-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1945">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1946">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1946">+0</span></span>   | <span data-ttu-id="d7bba-1947">y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1947">y</span></span>    | <span data-ttu-id="d7bba-1948">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1948">+0</span></span>   | <span data-ttu-id="d7bba-1949">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1949">+0</span></span>   | <span data-ttu-id="d7bba-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1950">+inf</span></span> | <span data-ttu-id="d7bba-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1951">-inf</span></span> | <span data-ttu-id="d7bba-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1952">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1953">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1953">-0</span></span>   | <span data-ttu-id="d7bba-1954">y</span><span class="sxs-lookup"><span data-stu-id="d7bba-1954">y</span></span>    | <span data-ttu-id="d7bba-1955">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1955">+0</span></span>   | <span data-ttu-id="d7bba-1956">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-1956">-0</span></span>   | <span data-ttu-id="d7bba-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1957">+inf</span></span> | <span data-ttu-id="d7bba-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1958">-inf</span></span> | <span data-ttu-id="d7bba-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1959">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1960">+inf</span></span> | <span data-ttu-id="d7bba-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1961">+inf</span></span> | <span data-ttu-id="d7bba-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1962">+inf</span></span> | <span data-ttu-id="d7bba-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1963">+inf</span></span> | <span data-ttu-id="d7bba-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1964">+inf</span></span> | <span data-ttu-id="d7bba-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1965">NaN</span></span>  | <span data-ttu-id="d7bba-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1966">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1967">-inf</span></span> | <span data-ttu-id="d7bba-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1968">-inf</span></span> | <span data-ttu-id="d7bba-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1969">-inf</span></span> | <span data-ttu-id="d7bba-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1970">-inf</span></span> | <span data-ttu-id="d7bba-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1971">NaN</span></span>  | <span data-ttu-id="d7bba-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-1972">-inf</span></span> | <span data-ttu-id="d7bba-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1973">NaN</span></span>  | 
   | <span data-ttu-id="d7bba-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1974">NaN</span></span>  | <span data-ttu-id="d7bba-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1975">NaN</span></span>  | <span data-ttu-id="d7bba-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1976">NaN</span></span>  | <span data-ttu-id="d7bba-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1977">NaN</span></span>  | <span data-ttu-id="d7bba-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1978">NaN</span></span>  | <span data-ttu-id="d7bba-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1979">NaN</span></span>  | <span data-ttu-id="d7bba-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-1980">NaN</span></span>  | 

*  <span data-ttu-id="d7bba-1981">Suma de decimales:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="d7bba-1982">Si el valor resultante es demasiado grande para representarlo en el `decimal` formato, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="d7bba-1983">La escala del resultado, antes de cualquier operación de redondeo, es el mayor de las escalas de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="d7bba-1984">La suma de decimales es equivalente a usar el operador de suma de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="d7bba-1985">Adición de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1985">Enumeration addition.</span></span> <span data-ttu-id="d7bba-1986">Cada tipo de enumeración proporciona implícitamente predefinidos de los siguientes operadores, donde `E` es el tipo enum y `U` es el tipo subyacente de `E`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="d7bba-1987">En tiempo de ejecución, estos operadores se evalúan exactamente como `(E)((U)x + (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="d7bba-1988">Concatenación de cadenas:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="d7bba-1989">Estas sobrecargas del binario `+` realizar de operador de concatenación de cadenas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="d7bba-1990">Si es un operando de la concatenación de cadenas `null`, se sustituye una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="d7bba-1991">De lo contrario, cualquier argumento que no son de cadena se convierte en su representación de cadena mediante la invocación virtual `ToString` método hereda del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="d7bba-1992">Si `ToString` devuelve `null`, se sustituye una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   El resultado del operador de concatenación de cadena es una cadena que consta de los caracteres del operando izquierdo seguidos por los caracteres del operando derecho. El operador de concatenación de cadenas nunca devuelve un `null` valor. <span data-ttu-id="d7bba-1995">Un `System.OutOfMemoryException` se puede producir si no hay suficiente memoria disponible para asignar la cadena resultante.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="d7bba-1996">Combinación de delegados.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1996">Delegate combination.</span></span> <span data-ttu-id="d7bba-1997">Cada tipo de delegado proporciona implícitamente el siguiente operador predefinido, donde `D` es el tipo de delegado:</span><span class="sxs-lookup"><span data-stu-id="d7bba-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="d7bba-1998">El archivo binario `+` operador realiza la combinación de delegados cuando ambos operandos son de un tipo delegado `D`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="d7bba-1999">(Si los operandos tienen tipos de delegado distintos, se produce un error en tiempo de enlace.) Si el primer operando es `null`, el resultado de la operación es el valor del segundo operando (aunque eso es también `null`).</span><span class="sxs-lookup"><span data-stu-id="d7bba-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="d7bba-2000">En caso contrario, si el segundo operando es `null`, entonces el resultado de la operación es el valor del primer operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="d7bba-2001">En caso contrario, el resultado de la operación es un nueva instancia de delegado que, cuando se invoca, invoca el primer operando y, a continuación, invoca el segundo operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="d7bba-2002">Para obtener ejemplos de combinación de delegados, vea [operador de resta](expressions.md#subtraction-operator) y [invocación de delegado](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="d7bba-2003">Puesto que `System.Delegate` no es un tipo de delegado, `operator`  `+` no está definido para ella.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="d7bba-2004">Operador de resta</span><span class="sxs-lookup"><span data-stu-id="d7bba-2004">Subtraction operator</span></span>

<span data-ttu-id="d7bba-2005">Para una operación del formulario `x - y`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-2006">Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="d7bba-2007">Los operadores de resta predefinidos se enumeran a continuación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="d7bba-2008">Los operadores todos restar `y` desde `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="d7bba-2009">Resta de enteros:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="d7bba-2010">En un `checked` contexto, si la diferencia está fuera del intervalo del tipo del resultado, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="d7bba-2011">En un `unchecked` contexto, no se notifican los desbordamientos y se descartan los bits de orden superior significativa fuera del intervalo del tipo del resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="d7bba-2012">Resta de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="d7bba-2013">La diferencia se calcula según las reglas aritméticas de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="d7bba-2014">En la tabla siguiente se muestra los resultados de todas las posibles combinaciones de valores finitos distinto de cero, ceros, valores infinitos y NaN.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="d7bba-2015">En la tabla, `x` y `y` son valores finitos distinto de cero, y `z` es el resultado de `x - y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="d7bba-2016">Si `x` y `y` son iguales, `z` es cero positivo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="d7bba-2017">Si `x - y` es demasiado grande para representarlo en el tipo de destino, `z` es infinito con el mismo signo que `x - y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   | <span data-ttu-id="d7bba-2018">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2018">NaN</span></span>  | <span data-ttu-id="d7bba-2019">y</span><span class="sxs-lookup"><span data-stu-id="d7bba-2019">y</span></span>    | <span data-ttu-id="d7bba-2020">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-2020">+0</span></span>   | <span data-ttu-id="d7bba-2021">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-2021">-0</span></span>   | <span data-ttu-id="d7bba-2022">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2022">+inf</span></span> | <span data-ttu-id="d7bba-2023">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2023">-inf</span></span> | <span data-ttu-id="d7bba-2024">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2024">NaN</span></span> | 
   | <span data-ttu-id="d7bba-2025">x</span><span class="sxs-lookup"><span data-stu-id="d7bba-2025">x</span></span>    | <span data-ttu-id="d7bba-2026">z</span><span class="sxs-lookup"><span data-stu-id="d7bba-2026">z</span></span>    | <span data-ttu-id="d7bba-2027">x</span><span class="sxs-lookup"><span data-stu-id="d7bba-2027">x</span></span>    | <span data-ttu-id="d7bba-2028">x</span><span class="sxs-lookup"><span data-stu-id="d7bba-2028">x</span></span>    | <span data-ttu-id="d7bba-2029">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2029">-inf</span></span> | <span data-ttu-id="d7bba-2030">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2030">+inf</span></span> | <span data-ttu-id="d7bba-2031">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2031">NaN</span></span> | 
   | <span data-ttu-id="d7bba-2032">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-2032">+0</span></span>   | <span data-ttu-id="d7bba-2033">-y</span><span class="sxs-lookup"><span data-stu-id="d7bba-2033">-y</span></span>   | <span data-ttu-id="d7bba-2034">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-2034">+0</span></span>   | <span data-ttu-id="d7bba-2035">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-2035">+0</span></span>   | <span data-ttu-id="d7bba-2036">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2036">-inf</span></span> | <span data-ttu-id="d7bba-2037">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2037">+inf</span></span> | <span data-ttu-id="d7bba-2038">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2038">NaN</span></span> | 
   | <span data-ttu-id="d7bba-2039">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-2039">-0</span></span>   | <span data-ttu-id="d7bba-2040">-y</span><span class="sxs-lookup"><span data-stu-id="d7bba-2040">-y</span></span>   | <span data-ttu-id="d7bba-2041">-0</span><span class="sxs-lookup"><span data-stu-id="d7bba-2041">-0</span></span>   | <span data-ttu-id="d7bba-2042">+0</span><span class="sxs-lookup"><span data-stu-id="d7bba-2042">+0</span></span>   | <span data-ttu-id="d7bba-2043">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2043">-inf</span></span> | <span data-ttu-id="d7bba-2044">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2044">+inf</span></span> | <span data-ttu-id="d7bba-2045">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2045">NaN</span></span> | 
   | <span data-ttu-id="d7bba-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2046">+inf</span></span> | <span data-ttu-id="d7bba-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2047">+inf</span></span> | <span data-ttu-id="d7bba-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2048">+inf</span></span> | <span data-ttu-id="d7bba-2049">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2049">+inf</span></span> | <span data-ttu-id="d7bba-2050">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2050">NaN</span></span>  | <span data-ttu-id="d7bba-2051">+inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2051">+inf</span></span> | <span data-ttu-id="d7bba-2052">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2052">NaN</span></span> | 
   | <span data-ttu-id="d7bba-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2053">-inf</span></span> | <span data-ttu-id="d7bba-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2054">-inf</span></span> | <span data-ttu-id="d7bba-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2055">-inf</span></span> | <span data-ttu-id="d7bba-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2056">-inf</span></span> | <span data-ttu-id="d7bba-2057">-inf</span><span class="sxs-lookup"><span data-stu-id="d7bba-2057">-inf</span></span> | <span data-ttu-id="d7bba-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2058">NaN</span></span>  | <span data-ttu-id="d7bba-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2059">NaN</span></span> | 
   | <span data-ttu-id="d7bba-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2060">NaN</span></span>  | <span data-ttu-id="d7bba-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2061">NaN</span></span>  | <span data-ttu-id="d7bba-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2062">NaN</span></span>  | <span data-ttu-id="d7bba-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2063">NaN</span></span>  | <span data-ttu-id="d7bba-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2064">NaN</span></span>  | <span data-ttu-id="d7bba-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2065">NaN</span></span>  | <span data-ttu-id="d7bba-2066">NaN</span><span class="sxs-lookup"><span data-stu-id="d7bba-2066">NaN</span></span> | 

*  <span data-ttu-id="d7bba-2067">Resta decimal:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2067">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="d7bba-2068">Si el valor resultante es demasiado grande para representarlo en el `decimal` formato, un `System.OverflowException` se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2068">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="d7bba-2069">La escala del resultado, antes de cualquier operación de redondeo, es el mayor de las escalas de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2069">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="d7bba-2070">Resta decimal es equivalente a usar el operador de resta de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2070">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="d7bba-2071">Resta de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2071">Enumeration subtraction.</span></span> <span data-ttu-id="d7bba-2072">Cada tipo de enumeración proporciona implícitamente el siguiente operador predefinido, donde `E` es el tipo enum y `U` es el tipo subyacente de `E`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2072">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="d7bba-2073">Este operador se evalúa exactamente como `(U)((U)x - (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2073">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="d7bba-2074">En otras palabras, el operador calcula la diferencia entre los valores ordinales de `x` y `y`, y el tipo del resultado es el tipo subyacente de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2074">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="d7bba-2075">Este operador se evalúa exactamente como `(E)((U)x - y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2075">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="d7bba-2076">En otras palabras, el operador de resta un valor de tipo subyacente de la enumeración, que produce un valor de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2076">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="d7bba-2077">Eliminación de delegados.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2077">Delegate removal.</span></span> <span data-ttu-id="d7bba-2078">Cada tipo de delegado proporciona implícitamente el siguiente operador predefinido, donde `D` es el tipo de delegado:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2078">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="d7bba-2079">El archivo binario `-` operador realiza la eliminación de delegados cuando ambos operandos son de un tipo delegado `D`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2079">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="d7bba-2080">Si los operandos tienen tipos de delegado distintos, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2080">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="d7bba-2081">Si el primer operando es `null`, el resultado de la operación es `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2081">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="d7bba-2082">En caso contrario, si el segundo operando es `null`, entonces el resultado de la operación es el valor del primer operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2082">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="d7bba-2083">En caso contrario, ambos operandos representan listas de invocación ([declaraciones de delegado](delegates.md#delegate-declarations)) que tiene una o más entradas y el resultado es una nueva lista de invocación que consta de lista del primer operando con entradas del segundo operando eliminadas proporciona la lista del segundo operando es una sublista apropiada contigua de la primera.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2083">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="d7bba-2084">(Para determinar la igualdad de sublista, las entradas correspondientes se comparan en cuanto el operador de igualdad del delegado ([delegar los operadores de igualdad](expressions.md#delegate-equality-operators)).) En caso contrario, el resultado es el valor del operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2084">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="d7bba-2085">Ninguna de las listas de los operandos se cambia en el proceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2085">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="d7bba-2086">Si la lista del segundo operando coincide con varias sublistas de entradas contiguas de la lista del primer operando, se quita la sublista coincidente más a la derecha de las entradas contiguas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2086">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="d7bba-2087">Si la eliminación se produce en una lista vacía, el resultado es `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2087">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="d7bba-2088">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2088">For example:</span></span>

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a><span data-ttu-id="d7bba-2089">Operadores de desplazamiento</span><span class="sxs-lookup"><span data-stu-id="d7bba-2089">Shift operators</span></span>

<span data-ttu-id="d7bba-2090">El `<<` y `>>` operadores se utilizan para realizar operaciones de desplazamiento de bits.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2090">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="d7bba-2091">Si un operando de un *shift_expression* tiene el tipo de tiempo de compilación `dynamic`, a continuación, la expresión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2091">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="d7bba-2092">En este caso es el tipo de tiempo de compilación de la expresión `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución de los operandos que tienen el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2092">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="d7bba-2093">Para una operación del formulario `x << count` o `x >> count`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2093">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-2094">Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2094">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="d7bba-2095">Al declarar un operador de desplazamiento sobrecargado, el tipo del primer operando debe ser siempre la clase o estructura que contiene la declaración del operador y el tipo del segundo operando debe ser siempre `int`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2095">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="d7bba-2096">A continuación, se enumeran los operadores de desplazamiento predefinidos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2096">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="d7bba-2097">Desplazamiento a la izquierda:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2097">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="d7bba-2098">El `<<` operador turnos `x` izquierda un número de bits se calcula como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2098">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="d7bba-2099">Los bits de orden superior fuera del intervalo del tipo de resultado `x` son descartados, los bits restantes se desplazan a la izquierda y las posiciones de bits vacíos de orden inferior se establecen en cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2099">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="d7bba-2100">Desplazamiento a la derecha:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2100">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="d7bba-2101">El `>>` operador turnos `x` derecha un número de bits se calcula como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2101">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="d7bba-2102">Cuando `x` es de tipo `int` o `long`, los bits de orden inferior de `x` son descartan, los bits restantes se desplazan a la derecha y las posiciones de bits vacíos de orden superior se establecen en cero si `x` es no negativo y establecido en uno si `x` es negativo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2102">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="d7bba-2103">Cuando `x` es de tipo `uint` o `ulong`, los bits de orden inferior de `x` son descartados, los bits restantes se desplazan a la derecha y las posiciones de bits vacíos de orden superior se establecen en cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2103">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="d7bba-2104">Para los operadores predefinidos, el número de bits de desplazamiento se calcula como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2104">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="d7bba-2105">Cuando el tipo de `x` es `int` o `uint`, el valor de desplazamiento viene dado por los cinco bits de orden inferior de `count`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2105">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="d7bba-2106">En otras palabras, se calcula el valor de desplazamiento de `count & 0x1F`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2106">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="d7bba-2107">Cuando el tipo de `x` es `long` o `ulong`, el valor de desplazamiento viene dado por los seis bits de orden inferior de `count`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2107">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="d7bba-2108">En otras palabras, se calcula el valor de desplazamiento de `count & 0x3F`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2108">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="d7bba-2109">Si el valor de desplazamiento resultante es cero, los operadores de desplazamiento simplemente devuelven el valor de `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2109">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="d7bba-2110">Operaciones de desplazamiento nunca producen desbordamientos y producen el mismo resultado en `checked` y `unchecked` contextos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2110">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="d7bba-2111">Cuando el operando izquierdo de la `>>` operador es de un tipo entero con signo, el operador realiza un desplazamiento aritmético a la derecha, en la que el valor del bit más significativo (el bit de signo) del operando se propaga a las posiciones de bits vacíos de orden superior.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2111">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="d7bba-2112">Cuando el operando izquierdo de la `>>` operador es de tipo entero sin signo, el operador realiza un desplazamiento lógico directamente en la que las posiciones de bits vacíos de orden superior siempre se establecen en cero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2112">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="d7bba-2113">Para realizar la operación contraria del deriva del tipo de operando, se pueden usar conversiones explícitas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2113">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="d7bba-2114">Por ejemplo, si `x` es una variable de tipo `int`, la operación `unchecked((int)((uint)x >> y))` realiza un desplazamiento lógico derecha de `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2114">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="d7bba-2115">Operadores de comprobación de tipos y relacionales</span><span class="sxs-lookup"><span data-stu-id="d7bba-2115">Relational and type-testing operators</span></span>

<span data-ttu-id="d7bba-2116">El `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` y `as` se denominan los operadores relacionales y comprobación de tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2116">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

<span data-ttu-id="d7bba-2117">El `is` operador se describe en [el operador is](expressions.md#the-is-operator) y el `as` operador se describe en [el operador as](expressions.md#the-as-operator).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2117">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="d7bba-2118">El `==`, `!=`, `<`, `>`, `<=` y `>=` operadores son ***operadores de comparación***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2118">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="d7bba-2119">Si un operando de un operador de comparación con el tipo de tiempo de compilación `dynamic`, a continuación, la expresión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2119">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="d7bba-2120">En este caso es el tipo de tiempo de compilación de la expresión `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución de los operandos que tienen el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2120">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="d7bba-2121">Para una operación del formulario `x` *op* `y`, donde *op* es un operador de comparación, la resolución de sobrecarga ([deresolucióndesobrecargadeoperadorbinario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2121">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-2122">Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2122">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="d7bba-2123">Los operadores de comparación predefinidos se describen en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2123">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="d7bba-2124">Todos los operadores de comparación predefinidos devuelven un resultado de tipo `bool`, tal y como se describe en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2124">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="d7bba-2125">__Operación__</span><span class="sxs-lookup"><span data-stu-id="d7bba-2125">__Operation__</span></span> | <span data-ttu-id="d7bba-2126">__Resultado__</span><span class="sxs-lookup"><span data-stu-id="d7bba-2126">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="d7bba-2127">`true` Si `x` es igual a `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2127">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="d7bba-2128">`true` Si `x` no es igual a `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2128">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="d7bba-2129">`true` Si `x` es menor que `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2129">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="d7bba-2130">`true` Si `x` es mayor que `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2130">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="d7bba-2131">`true` Si `x` es menor o igual que `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2131">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="d7bba-2132">`true` Si `x` es mayor o igual a `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2132">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="d7bba-2133">Operadores de comparación de enteros</span><span class="sxs-lookup"><span data-stu-id="d7bba-2133">Integer comparison operators</span></span>

<span data-ttu-id="d7bba-2134">Los operadores de comparación de enteros predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2134">The predefined integer comparison operators are:</span></span>
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

<span data-ttu-id="d7bba-2135">Cada uno de estos operadores compara los valores numéricos de los dos operandos enteros y devuelve un `bool` valor que indica si la relación concreta es `true` o `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2135">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="d7bba-2136">Operadores de comparación de punto flotante</span><span class="sxs-lookup"><span data-stu-id="d7bba-2136">Floating-point comparison operators</span></span>

<span data-ttu-id="d7bba-2137">Los operadores de comparación de punto flotante predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2137">The predefined floating-point comparison operators are:</span></span>
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

<span data-ttu-id="d7bba-2138">Los operadores comparan los operandos según las reglas del estándar IEEE 754:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2138">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="d7bba-2139">Si alguno de los operandos es NaN, el resultado es `false` para todos los operadores excepto `!=`, para que el resultado es `true`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2139">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="d7bba-2140">Para los dos operandos, `x != y` siempre genera el mismo resultado que `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2140">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="d7bba-2141">Sin embargo, cuando uno o ambos operandos son NaN, la `<`, `>`, `<=`, y `>=` operadores no generan los mismos resultados que la negación lógica del operador opuesto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2141">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="d7bba-2142">Por ejemplo, si alguno de `x` y `y` es NaN, a continuación, `x < y` es `false`, pero `!(x >= y)` es `true`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2142">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="d7bba-2143">Cuando ninguno de los operandos es NaN, los operadores comparan los valores de los dos operandos de punto flotante con respecto al orden</span><span class="sxs-lookup"><span data-stu-id="d7bba-2143">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="d7bba-2144">donde `min` y `max` son los valores mayor y menor positivos finitos que pueden representarse en el formato de punto flotante especificado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2144">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="d7bba-2145">Efectos importantes de esta ordenación son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2145">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="d7bba-2146">Ceros negativos y positivos se consideran iguales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2146">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="d7bba-2147">Un infinito negativo se considera menor que todos los demás valores, pero igual que otro infinito negativo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2147">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="d7bba-2148">Se considera un infinito positivo mayor que todos los demás valores, pero igual que otro infinito positivo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2148">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="d7bba-2149">Operadores de comparación decimal</span><span class="sxs-lookup"><span data-stu-id="d7bba-2149">Decimal comparison operators</span></span>

<span data-ttu-id="d7bba-2150">Los operadores de comparación de decimales predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2150">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="d7bba-2151">Cada uno de estos operadores compara los valores numéricos de los dos operandos decimales y devuelve un `bool` valor que indica si la relación concreta es `true` o `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2151">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="d7bba-2152">Cada comparación decimal equivale al uso del operador de igualdad de tipo o correspondiente relacional `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2152">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="d7bba-2153">Operadores de igualdad booleana</span><span class="sxs-lookup"><span data-stu-id="d7bba-2153">Boolean equality operators</span></span>

<span data-ttu-id="d7bba-2154">Los operadores de igualdad booleana predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2154">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="d7bba-2155">El resultado de `==` es `true` si ambos `x` y `y` son `true` o si ambos `x` y `y` son `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2155">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="d7bba-2156">De lo contrario, el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2156">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="d7bba-2157">El resultado de `!=` es `false` si ambos `x` y `y` son `true` o si ambos `x` y `y` son `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2157">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="d7bba-2158">De lo contrario, el resultado es `true`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2158">Otherwise, the result is `true`.</span></span> <span data-ttu-id="d7bba-2159">Cuando los operandos son de tipo `bool`, `!=` operador genera el mismo resultado que el `^` operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2159">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="d7bba-2160">Operadores de comparación (enumeración)</span><span class="sxs-lookup"><span data-stu-id="d7bba-2160">Enumeration comparison operators</span></span>

<span data-ttu-id="d7bba-2161">Cada tipo de enumeración implícitamente proporciona los siguientes operadores de comparación predefinidos:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2161">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="d7bba-2162">El resultado de evaluar `x op y`, donde `x` y `y` son expresiones de un tipo de enumeración `E` con un tipo subyacente `U`, y `op` es uno de los operadores de comparación, es exactamente igual que evaluar `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2162">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="d7bba-2163">En otras palabras, los operadores de comparación de tipo de enumeración simplemente comparan los valores enteros subyacentes de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2163">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="d7bba-2164">Operadores de igualdad de tipo de referencia</span><span class="sxs-lookup"><span data-stu-id="d7bba-2164">Reference type equality operators</span></span>

<span data-ttu-id="d7bba-2165">Los operadores de igualdad de tipo de referencia predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2165">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="d7bba-2166">Los operadores devuelven el resultado de comparar las dos referencias de igualdad o desigualdad.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2166">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="d7bba-2167">Dado que los operadores de igualdad de referencia predefinidos tipo aceptan operandos de tipo `object`, se aplican a todos los tipos que no declaran aplicable `operator ==` y `operator !=` miembros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2167">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="d7bba-2168">Por el contrario, los operadores de igualdad definido por el usuario aplicables ocultan eficazmente los operadores de igualdad de tipo de referencia predefinidos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2168">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="d7bba-2169">Los operadores de igualdad de tipo de referencia predefinidos requieren uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2169">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="d7bba-2170">Ambos operandos son un valor de un tipo conocido como un *reference_type* o el literal `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2170">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="d7bba-2171">Además, una conversión explícita de referencia ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) existe desde el tipo de uno de los operandos al tipo del otro operando.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2171">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="d7bba-2172">Un operando es un valor de tipo `T` donde `T` es un *tipo_parámetro* y el otro operando es el literal `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2172">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="d7bba-2173">Además `T` no tiene la restricción de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2173">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="d7bba-2174">A menos que una de estas condiciones son true, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2174">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="d7bba-2175">Implicaciones importantes de estas reglas son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2175">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="d7bba-2176">Es un error en tiempo de enlace a usar los operadores de igualdad de tipo de referencia predefinidos para comparar dos referencias que se sabe que son diferentes en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2176">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="d7bba-2177">Por ejemplo, si los tipos en tiempo de enlace de los operandos son dos tipos de clase `A` y `B`y si no `A` ni `B` se deriva de otra, sería imposible que los dos operandos para hacer referencia al mismo objeto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2177">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="d7bba-2178">Por lo tanto, la operación se considera un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2178">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="d7bba-2179">Los operadores de igualdad de tipo de referencia predefinidos no permiten valor operandos de tipo va a comparar.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2179">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="d7bba-2180">Por lo tanto, a menos que un tipo de estructura declara sus propios operadores de igualdad, no es posible comparar valores de ese tipo de estructura.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2180">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="d7bba-2181">Los operadores de igualdad de tipo de referencia predefinidos nunca producen las operaciones de conversión boxing se producen para sus operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2181">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="d7bba-2182">Tendría sentido para realizar estas operaciones de conversión boxing, puesto que las referencias a las instancias de conversión boxing recién asignadas necesariamente podrían diferir de todas las demás referencias.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2182">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="d7bba-2183">Si un operando de un parámetro de tipo `T` se compara con `null`y el tipo de tiempo de ejecución de `T` es un tipo de valor, el resultado de la comparación es `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2183">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="d7bba-2184">El ejemplo siguiente se comprueba si un argumento de un tipo de parámetro de tipo sin restricciones es `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2184">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="d7bba-2185">El `x == null` , aunque se permite la construcción `T` podría representar un tipo de valor y el resultado se define simplemente como `false` cuando `T` es un tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2185">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="d7bba-2186">Para una operación del formulario `x == y` o `x != y`, si procede cualquier `operator ==` o `operator !=` existe, la resolución de sobrecarga de operador ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) las reglas que seleccionará operador en lugar del operador de igualdad de tipo de referencia predefinidos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2186">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="d7bba-2187">Sin embargo, siempre es posible seleccionar el operador de igualdad de referencia predefinidos tipo convirtiendo de forma explícita uno o ambos operandos al tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2187">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="d7bba-2188">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2188">The example</span></span>
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
<span data-ttu-id="d7bba-2189">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="d7bba-2189">produces the output</span></span>
```
True
False
False
False
```

<span data-ttu-id="d7bba-2190">El `s` y `t` variables hacen referencia a dos distintos `string` instancias que contienen los mismos caracteres.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2190">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="d7bba-2191">La primera comparación da como resultado `True` porque el operador de igualdad de cadenas predefinido ([operadores de igualdad de cadenas](expressions.md#string-equality-operators)) está activada cuando ambos operandos son del tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2191">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="d7bba-2192">Salida de todas las comparaciones restantes `False` porque se ha seleccionado el operador de igualdad de tipo de referencia predefinidos cuando uno o ambos operandos son del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2192">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="d7bba-2193">Tenga en cuenta que la técnica anterior no es significativa para los tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2193">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="d7bba-2194">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2194">The example</span></span>
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
<span data-ttu-id="d7bba-2195">da como resultado `False` porque las conversiones de tipos para crear referencias a dos instancias independientes de conversión boxing `int` valores.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2195">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="d7bba-2196">Operadores de igualdad de cadenas</span><span class="sxs-lookup"><span data-stu-id="d7bba-2196">String equality operators</span></span>

<span data-ttu-id="d7bba-2197">Los operadores de igualdad de cadenas predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2197">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="d7bba-2198">Dos `string` valores se consideran iguales cuando se cumple una de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2198">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="d7bba-2199">Los dos valores son `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2199">Both values are `null`.</span></span>
*  <span data-ttu-id="d7bba-2200">Ambos valores son distintos de null referencias a instancias de cadenas que tienen una longitud idéntica y caracteres idénticos en cada posición de carácter.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2200">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="d7bba-2201">Los operadores de igualdad de cadenas comparan los valores de cadena en lugar de las referencias de cadena.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2201">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="d7bba-2202">Cuando dos instancias de cadena distintas contienen exactamente la misma secuencia de caracteres, los valores de las cadenas son iguales, pero las referencias son diferentes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2202">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="d7bba-2203">Como se describe en [operadores de igualdad de referencia tipo](expressions.md#reference-type-equality-operators), los operadores de igualdad de tipo de referencia pueden usarse para comparar las referencias de cadena en lugar de los valores de cadena.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2203">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="d7bba-2204">Operadores de igualdad de delegado</span><span class="sxs-lookup"><span data-stu-id="d7bba-2204">Delegate equality operators</span></span>

<span data-ttu-id="d7bba-2205">Cada tipo de delegado implícitamente proporciona los siguientes operadores de comparación predefinidos:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2205">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="d7bba-2206">Delegado de dos instancias se consideran iguales como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2206">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="d7bba-2207">Si se da alguna de las instancias de delegado `null`, son iguales solo si ambos son `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2207">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="d7bba-2208">Si los delegados tienen diferentes tipos en tiempo de ejecución nunca son iguales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2208">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="d7bba-2209">Si ambas de las instancias de delegado tienen una lista de invocación ([declaraciones de delegado](delegates.md#delegate-declarations)), las instancias son iguales si y solo si sus listas de invocaciones tienen la misma longitud, y cada entrada en la lista de invocación es igual (tal y como se define a continuación) a la entrada correspondiente, en orden, en la lista de invocaciones de la otra.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2209">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="d7bba-2210">Las siguientes reglas determinan la igualdad de las entradas de la lista de invocación:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2210">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="d7bba-2211">Si dos invocación entradas de la lista ambos hacen referencia al mismo estático método, a continuación, las entradas son iguales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2211">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="d7bba-2212">Si dos invocación entradas de la lista ambos hacen referencia al mismo método no estático en el mismo objeto de destino (como se define por los operadores de igualdad de referencia), a continuación, las entradas son iguales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2212">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="d7bba-2213">Genera entradas de la lista de invocación de la evaluación de semánticamente idéntico *anonymous_method_expression*s o *lambda_expression*s con el mismo conjunto (posiblemente vacío) de la variable externa capturada las instancias se permiten (aunque no es necesario) para que sea igual.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2213">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="d7bba-2214">NULL y los operadores de igualdad</span><span class="sxs-lookup"><span data-stu-id="d7bba-2214">Equality operators and null</span></span>

<span data-ttu-id="d7bba-2215">El `==` y `!=` operadores permiten un operando tenga un valor de un tipo que acepta valores NULL y el otro sea el `null` literal, aunque no exista ningún operador predefinido o definidos por el usuario (en no sea de elevación o formato de elevación) para la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2215">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="d7bba-2216">Para una operación de uno de los formularios</span><span class="sxs-lookup"><span data-stu-id="d7bba-2216">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="d7bba-2217">donde `x` es una expresión de un tipo que acepta valores NULL, si el operador de resolución de sobrecarga ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) no se puede encontrar un operador aplicable, el resultado en su lugar, se calcula a partir del `HasValue` propiedad de `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2217">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="d7bba-2218">En concreto, se traducen los dos primeros formularios `!x.HasValue`, y se traducen los dos últimos formularios `x.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2218">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="d7bba-2219">El operador is</span><span class="sxs-lookup"><span data-stu-id="d7bba-2219">The is operator</span></span>

<span data-ttu-id="d7bba-2220">El `is` operador se usa para comprobar si el tipo de tiempo de ejecución de un objeto es compatible con un tipo determinado de manera dinámica.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2220">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="d7bba-2221">El resultado de la operación `E is T`, donde `E` es una expresión y `T` es un tipo, es un valor booleano que indica el valor si `E` correctamente puede convertirse al tipo `T` mediante una conversión de referencia, una conversión boxing conversión, o una conversión unboxing.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2221">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="d7bba-2222">La operación se evalúa como sigue, después de argumentos de tipo se sustituyeron por todos los parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2222">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="d7bba-2223">Si `E` es una función anónima, se produce un error de tiempo de compilación</span><span class="sxs-lookup"><span data-stu-id="d7bba-2223">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="d7bba-2224">Si `E` es un grupo de métodos o `null` literales de if el tipo de `E` es un tipo de referencia o un tipo que acepta valores NULL y el valor de `E` es null, el resultado es false.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2224">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="d7bba-2225">De lo contrario, deje que `D` representan el tipo dinámico de `E` como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2225">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="d7bba-2226">Si el tipo de `E` es un tipo de referencia, `D` es el tipo de tiempo de ejecución de la referencia de instancia por `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2226">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="d7bba-2227">Si el tipo de `E` es un tipo que acepta valores NULL, `D` es el tipo subyacente de ese tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2227">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="d7bba-2228">Si el tipo de `E` es un tipo de valor distinto de null, `D` es el tipo de `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2228">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="d7bba-2229">El resultado de la operación depende `D` y `T` como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2229">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="d7bba-2230">Si `T` es un tipo de referencia, el resultado es true si `D` y `T` son del mismo tipo, si `D` es un tipo de referencia y una conversión implícita de referencia de `D` a `T` existe, o si `D` es un tipo de valor y una conversión boxing de `D` a `T` existe.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2230">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="d7bba-2231">Si `T` es un tipo que acepta valores NULL, el resultado es true si `D` es el tipo subyacente de `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2231">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="d7bba-2232">Si `T` es un tipo de valor distinto de null, el resultado es true si `D` y `T` son del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2232">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="d7bba-2233">En caso contrario, el resultado es false.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2233">Otherwise, the result is false.</span></span>

<span data-ttu-id="d7bba-2234">Tenga en cuenta que las conversiones definidas por el usuario, no tiene en cuenta el `is` operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2234">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="d7bba-2235">El operador as</span><span class="sxs-lookup"><span data-stu-id="d7bba-2235">The as operator</span></span>

<span data-ttu-id="d7bba-2236">El `as` operador se usa para convertir explícitamente un valor a un tipo que acepta valores NULL o un tipo de referencia especificada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2236">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="d7bba-2237">A diferencia de una expresión de conversión ([expresiones de conversión](expressions.md#cast-expressions)), el `as` operador nunca produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2237">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="d7bba-2238">En su lugar, si la conversión indicada no es posible, el valor resultante es `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2238">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="d7bba-2239">En una operación del formulario `E as T`, `E` debe ser una expresión y `T` debe ser un tipo de referencia, un parámetro de tipo que se sabe que es un tipo de referencia o un tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2239">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="d7bba-2240">Además, al menos uno de los siguientes debe ser true o en caso contrario, se produce un error en tiempo de compilación:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2240">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="d7bba-2241">Una identidad ([conversión de identidad](conversions.md#identity-conversion)), implícita que acepta valores null ([las conversiones implícitas que acepta valores NULL](conversions.md#implicit-nullable-conversions)), una referencia implícita ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)), la conversión boxing ([ Las conversiones boxing](conversions.md#boxing-conversions)) explícito que acepta valores null ([conversiones que aceptan valores NULL explícitas](conversions.md#explicit-nullable-conversions)), una referencia explícita ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)), unboxing o ([Conversiones Unboxing](conversions.md#unboxing-conversions)) no existe conversión de `E` a `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2241">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="d7bba-2242">El tipo de `E` o `T` es un tipo abierto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2242">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="d7bba-2243">`E` es el `null` literal.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2243">`E` is the `null` literal.</span></span>

<span data-ttu-id="d7bba-2244">Si el tipo de tiempo de compilación de `E` no `dynamic`, la operación `E as T` produce el mismo resultado que</span><span class="sxs-lookup"><span data-stu-id="d7bba-2244">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="d7bba-2245">salvo que `E` solo se evalúa una vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2245">except that `E` is only evaluated once.</span></span> <span data-ttu-id="d7bba-2246">Cabe esperar que el compilador optimice `E as T` para realizar a lo sumo una comprobación de tipo dinámico en lugar de las dos comprobaciones de tipo dinámico implicadas en la expansión de la anterior.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2246">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="d7bba-2247">Si el tipo de tiempo de compilación de `E` es `dynamic`, a diferencia del operador de conversión el `as` operador no se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2247">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="d7bba-2248">Por lo tanto, la expansión en este caso es:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2248">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="d7bba-2249">Tenga en cuenta que algunas conversiones, como las conversiones definidas por el usuario no son posibles con la `as` operador y debe realizarse mediante expresiones de conversión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2249">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="d7bba-2250">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2250">In the example</span></span>
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
<span data-ttu-id="d7bba-2251">el parámetro de tipo `T` de `G` se sabe que es un tipo de referencia, porque tiene la restricción de clase.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2251">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="d7bba-2252">El parámetro de tipo `U` de `H` no es cambio; por lo tanto, el uso de la `as` operador en `H` no está permitida.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2252">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="d7bba-2253">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="d7bba-2253">Logical operators</span></span>

<span data-ttu-id="d7bba-2254">El `&`, `^`, y `|` se denominan los operadores lógicos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2254">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

<span data-ttu-id="d7bba-2255">Si un operando de un operador lógico tiene el tipo de tiempo de compilación `dynamic`, a continuación, la expresión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2255">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="d7bba-2256">En este caso es el tipo de tiempo de compilación de la expresión `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución de los operandos que tienen el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2256">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="d7bba-2257">Para una operación del formulario `x op y`, donde `op` es uno de los operadores lógicos, la resolución de sobrecarga ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2257">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="d7bba-2258">Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2258">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="d7bba-2259">Los operadores lógicos predefinidos se describen en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2259">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="d7bba-2260">Operadores lógicos enteros</span><span class="sxs-lookup"><span data-stu-id="d7bba-2260">Integer logical operators</span></span>

<span data-ttu-id="d7bba-2261">Los operadores lógicos enteros predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2261">The predefined integer logical operators are:</span></span>
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

<span data-ttu-id="d7bba-2262">El `&` operador calcula bit a bit lógica `AND` de los dos operandos, el `|` operador calcula bit a bit lógica `OR` de los dos operandos y el `^` operador calcula lógica exclusiva bit a bit `OR` de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2262">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="d7bba-2263">Los desbordamientos no son posibles en estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2263">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="d7bba-2264">Operadores lógicos de enumeración</span><span class="sxs-lookup"><span data-stu-id="d7bba-2264">Enumeration logical operators</span></span>

<span data-ttu-id="d7bba-2265">Cada tipo de enumeración `E` proporcionan de forma implícita predefinida de los siguientes operadores lógicos:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2265">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="d7bba-2266">El resultado de evaluar `x op y`, donde `x` y `y` son expresiones de un tipo de enumeración `E` con un tipo subyacente `U`, y `op` es uno de los operadores lógicos, es exactamente igual que evaluar `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2266">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="d7bba-2267">En otras palabras, los operadores lógicos de tipo de enumeración simplemente realizan la operación lógica en el tipo subyacente de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2267">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="d7bba-2268">Operadores lógicos booleanos</span><span class="sxs-lookup"><span data-stu-id="d7bba-2268">Boolean logical operators</span></span>

<span data-ttu-id="d7bba-2269">Los operadores lógicos boolean predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2269">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="d7bba-2270">El resultado de `x & y` es `true` si tanto `x` como `y` son `true`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2270">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="d7bba-2271">De lo contrario, el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2271">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="d7bba-2272">El resultado de `x | y` es `true` si `x` o `y` es `true`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2272">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="d7bba-2273">De lo contrario, el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2273">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="d7bba-2274">El resultado de `x ^ y` es `true` si `x` es `true` y `y` es `false`, o `x` es `false` y `y` es `true`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2274">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="d7bba-2275">De lo contrario, el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2275">Otherwise, the result is `false`.</span></span> <span data-ttu-id="d7bba-2276">Cuando los operandos son de tipo `bool`, `^` operador calcula el mismo resultado que el `!=` operador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2276">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="d7bba-2277">Operadores lógicos de tipo boolean que acepta valores null</span><span class="sxs-lookup"><span data-stu-id="d7bba-2277">Nullable boolean logical operators</span></span>

<span data-ttu-id="d7bba-2278">El tipo booleano que acepta valores NULL `bool?` puede representar los tres valores, `true`, `false`, y `null`y es conceptualmente similar al tipo de tres valores utilizado por expresiones booleanas en SQL.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2278">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="d7bba-2279">Para asegurarse de que los resultados producidos por el `&` y `|` operadores para `bool?` son coherentes con la lógica de tres valores de SQL, se proporcionan los siguientes operadores predefinidos:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2279">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="d7bba-2280">En la tabla siguiente se enumera los resultados generados por estos operadores para todas las combinaciones de los valores de `true`, `false`, y `null`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2280">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a><span data-ttu-id="d7bba-2281">Operadores lógicos condicionales</span><span class="sxs-lookup"><span data-stu-id="d7bba-2281">Conditional logical operators</span></span>

<span data-ttu-id="d7bba-2282">El `&&` y `||` operadores se denominan los operadores lógicos condicionales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2282">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="d7bba-2283">También se denominan los operadores lógicos de "cortocircuito".</span><span class="sxs-lookup"><span data-stu-id="d7bba-2283">They are also called the "short-circuiting" logical operators.</span></span>

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

<span data-ttu-id="d7bba-2284">El `&&` y `||` operadores son versiones condicionales de la `&` y `|` operadores:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2284">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="d7bba-2285">La operación `x && y` corresponde a la operación `x & y`, salvo que `y` solo se evalúa si `x` no `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2285">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="d7bba-2286">La operación `x || y` corresponde a la operación `x | y`, salvo que `y` solo se evalúa si `x` no `true`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2286">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="d7bba-2287">Si un operando de un operador lógico condicional tiene el tipo de tiempo de compilación `dynamic`, a continuación, la expresión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2287">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="d7bba-2288">En este caso es el tipo de tiempo de compilación de la expresión `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución de los operandos que tienen el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2288">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="d7bba-2289">Una operación del formulario `x && y` o `x || y` se procesa mediante la aplicación de la resolución de sobrecarga ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) como si la operación se ha escrito `x & y` o `x | y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2289">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="d7bba-2290">A continuación,</span><span class="sxs-lookup"><span data-stu-id="d7bba-2290">Then,</span></span>

*  <span data-ttu-id="d7bba-2291">Si se produce un error en la resolución de sobrecarga buscar un único operador mejor, o si la resolución de sobrecarga selecciona uno de los operadores lógicos enteros predefinidos, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2291">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-2292">En caso contrario, si el operador seleccionado es uno de los operadores lógicos booleanos predefinidos ([operadores lógicos Boolean](expressions.md#boolean-logical-operators)) o en operadores lógicos de tipo boolean que acepta valores null ([operadores lógicos de tipo boolean que acepta valores NULL](expressions.md#nullable-boolean-logical-operators)), el se ha procesado la operación tal como se describe en [booleano operadores lógicos condicionales](expressions.md#boolean-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2292">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="d7bba-2293">En caso contrario, el operador seleccionado es un operador definido por el usuario y la operación se procesa como se describe en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2293">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="d7bba-2294">No es posible sobrecargar directamente los operadores lógicos condicionales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2294">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="d7bba-2295">Sin embargo, dado que los operadores lógicos condicionales se evalúan en cuanto a los operadores lógicos normales, las sobrecargas de los operadores lógicos normales, con algunas restricciones, también consideran las sobrecargas de los operadores lógicos condicionales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2295">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="d7bba-2296">Esto se describe más detalladamente en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2296">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="d7bba-2297">Operadores lógicos condicionales booleanos</span><span class="sxs-lookup"><span data-stu-id="d7bba-2297">Boolean conditional logical operators</span></span>

<span data-ttu-id="d7bba-2298">Cuando los operandos de `&&` o `||` son del tipo `bool`, o cuando los operandos son de tipos que no definen un aplicable `operator &` o `operator |`, pero definir conversiones implícitas a `bool`, es la operación procesado como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2298">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="d7bba-2299">La operación `x && y` se evalúa como `x ? y : false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2299">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="d7bba-2300">En otras palabras, `x` se evalúa primero y se puede convertir al tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2300">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="d7bba-2301">A continuación, si `x` es `true`, `y` se evalúa y se convierte al tipo `bool`, y esto se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2301">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="d7bba-2302">En caso contrario, el resultado de la operación es `false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2302">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="d7bba-2303">La operación `x || y` se evalúa como `x ? true : y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2303">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="d7bba-2304">En otras palabras, `x` se evalúa primero y se puede convertir al tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2304">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="d7bba-2305">A continuación, si `x` es `true`, el resultado de la operación es `true`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2305">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="d7bba-2306">En caso contrario, `y` se evalúa y se convierte al tipo `bool`, y esto se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2306">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="d7bba-2307">Operadores lógicos condicionales definidos por el usuario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2307">User-defined conditional logical operators</span></span>

<span data-ttu-id="d7bba-2308">Cuando los operandos de `&&` o `||` son de tipos que declaran aplicable definido por el usuario `operator &` o `operator |`, ambos de los siguientes deben ser verdaderas, donde `T` es el tipo en el que se declara el operador seleccionado:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2308">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="d7bba-2309">El tipo de valor devuelto y el tipo de cada parámetro del operador seleccionado deben ser `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2309">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="d7bba-2310">En otras palabras, el operador debe calcular la lógica `AND` o lógico `OR` de dos operandos de tipo `T`y debe devolver un resultado de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2310">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="d7bba-2311">`T` debe incluir declaraciones de `operator true` y `operator false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2311">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="d7bba-2312">Si no se cumple alguno de estos requisitos, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2312">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="d7bba-2313">En caso contrario, el `&&` o `||` operación se evalúa mediante la combinación de definido por el usuario `operator true` o `operator false` con el operador definido por el usuario seleccionado:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2313">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="d7bba-2314">La operación `x && y` se evalúa como `T.false(x) ? x : T.&(x, y)`, donde `T.false(x)` es una invocación de la `operator false` declarado en `T`, y `T.&(x, y)` es una invocación del seleccionado `operator &`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2314">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="d7bba-2315">En otras palabras, `x` se evalúa primero y `operator false` se invoca en el resultado para determinar si `x` es definitivamente false.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2315">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="d7bba-2316">A continuación, si `x` es definitivamente false, el resultado de la operación es el valor previamente calculado para `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2316">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="d7bba-2317">En caso contrario, `y` se evalúa y seleccionado `operator &` se invoca en el valor previamente calculado para `x` y el valor calculado para `y` para generar el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2317">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="d7bba-2318">La operación `x || y` se evalúa como `T.true(x) ? x : T.|(x, y)`, donde `T.true(x)` es una invocación de la `operator true` declarado en `T`, y `T.|(x,y)` es una invocación del seleccionado `operator|`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2318">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="d7bba-2319">En otras palabras, `x` se evalúa primero y `operator true` se invoca en el resultado para determinar si `x` es definitivamente true.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2319">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="d7bba-2320">A continuación, si `x` es definitivamente es true, el resultado de la operación es el valor previamente calculado para `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2320">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="d7bba-2321">En caso contrario, `y` se evalúa y seleccionado `operator |` se invoca en el valor previamente calculado para `x` y el valor calculado para `y` para generar el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2321">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="d7bba-2322">En cualquiera de estas operaciones, la expresión proporcionada por `x` es solo evalúa una vez y la expresión proporcionada por `y` no se evalúa o evaluar exactamente una vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2322">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="d7bba-2323">Para obtener un ejemplo de un tipo que implementa `operator true` y `operator false`, consulte [base de datos de tipo booleano](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2323">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="d7bba-2324">El operador de uso combinado de null</span><span class="sxs-lookup"><span data-stu-id="d7bba-2324">The null coalescing operator</span></span>

<span data-ttu-id="d7bba-2325">El `??` operador se denomina operador de fusión null.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2325">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="d7bba-2326">Una expresión de fusión null del formulario `a ?? b` requiere `a` a ser de un tipo de referencia o de tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2326">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="d7bba-2327">Si `a` es distinto de null, el resultado de `a ?? b` es `a`; en caso contrario, el resultado es `b`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2327">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="d7bba-2328">La operación evalúa `b` solo si `a` es null.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2328">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="d7bba-2329">El operador de fusión null es asociativo a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2329">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="d7bba-2330">Por ejemplo, una expresión de formato `a ?? b ?? c` se evalúa como `a ?? (b ?? c)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2330">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="d7bba-2331">Términos en general, una expresión de formato `E1 ?? E2 ?? ... ?? En` devuelve el primero de los operandos que es distinto de null, o null si todos los operandos son nulos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2331">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="d7bba-2332">El tipo de la expresión `a ?? b` depende de qué conversiones implícitas están disponibles en los operandos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2332">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="d7bba-2333">En el orden de preferencia, el tipo de `a ?? b` es `A0`, `A`, o `B`, donde `A` es el tipo de `a` (siempre que `a` tiene un tipo), `B` es el tipo de `b` () siempre que `b` tiene un tipo), y `A0` es el tipo subyacente de `A` si `A` es un tipo que acepta valores NULL, o `A` en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2333">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="d7bba-2334">En concreto, `a ?? b` se procesa como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2334">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="d7bba-2335">Si `A` existe y no es un tipo que acepta valores NULL o un tipo de referencia, se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2335">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-2336">Si `b` es una expresión dinámica, el tipo de resultado es `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2336">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="d7bba-2337">En tiempo de ejecución, `a` se evalúa primero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2337">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="d7bba-2338">Si `a` no es null, `a` se convierte en dinámico, y esto se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2338">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="d7bba-2339">En caso contrario, `b` se evalúa, y esto se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2339">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="d7bba-2340">De lo contrario, si `A` existe y es un tipo que acepta valores NULL y existe una conversión implícita de `b` a `A0`, el tipo de resultado es `A0`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2340">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="d7bba-2341">En tiempo de ejecución, `a` se evalúa primero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2341">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="d7bba-2342">Si `a` no es null, `a` se desempaqueta para escriba `A0`, y esto se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2342">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="d7bba-2343">En caso contrario, `b` se evalúa y se convierte al tipo `A0`, y esto se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2343">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="d7bba-2344">De lo contrario, si `A` existe y no existe una conversión implícita de `b` a `A`, el tipo de resultado es `A`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2344">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="d7bba-2345">En tiempo de ejecución, `a` se evalúa primero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2345">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="d7bba-2346">Si `a` no es null, `a` se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2346">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="d7bba-2347">En caso contrario, `b` se evalúa y se convierte al tipo `A`, y esto se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2347">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="d7bba-2348">De lo contrario, si `b` tiene un tipo `B` y existe una conversión implícita de `a` a `B`, el tipo de resultado es `B`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2348">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="d7bba-2349">En tiempo de ejecución, `a` se evalúa primero.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2349">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="d7bba-2350">Si `a` no es null, `a` se desempaqueta para escriba `A0` (si `A` existe y que acepta valores NULL) y convertir al tipo `B`, y esto se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2350">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="d7bba-2351">En caso contrario, `b` se evalúa y se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2351">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="d7bba-2352">En caso contrario, `a` y `b` son incompatibles y un error en tiempo de compilación se produce.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2352">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="d7bba-2353">Operador condicional</span><span class="sxs-lookup"><span data-stu-id="d7bba-2353">Conditional operator</span></span>

<span data-ttu-id="d7bba-2354">El `?:` operador se denomina operador condicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2354">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="d7bba-2355">A veces también se denomina el operador ternario.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2355">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="d7bba-2356">Una expresión condicional del formulario `b ? x : y` primero evalúa la condición `b`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2356">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="d7bba-2357">A continuación, si `b` es `true`, `x` se evalúa y se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2357">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="d7bba-2358">En caso contrario, `y` se evalúa y se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2358">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="d7bba-2359">Una expresión condicional nunca evalúa ambos `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2359">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="d7bba-2360">El operador condicional es asociativo a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2360">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="d7bba-2361">Por ejemplo, una expresión de formato `a ? b : c ? d : e` se evalúa como `a ? b : (c ? d : e)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2361">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="d7bba-2362">El primer operando de la `?:` operador debe ser una expresión que se puede convertir implícitamente a `bool`, o una expresión de un tipo que implementa `operator true`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2362">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="d7bba-2363">Si se cumple ninguno de estos requisitos, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2363">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="d7bba-2364">Los operandos segundo y tercero, `x` y `y`, de la `?:` operador controlar el tipo de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2364">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="d7bba-2365">Si `x` tiene tipo `X` y `y` tiene tipo `Y` , a continuación,</span><span class="sxs-lookup"><span data-stu-id="d7bba-2365">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="d7bba-2366">Si una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde `X` a `Y`, pero no desde `Y` a `X`, a continuación, `Y` es el tipo de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="d7bba-2367">Si una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde `Y` a `X`, pero no desde `X` a `Y`, a continuación, `X` es el tipo de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2367">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="d7bba-2368">En caso contrario, no se puede determinar ningún tipo de expresión y se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2368">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="d7bba-2369">Si solo uno de `x` y `y` tiene un tipo y ambos `x` y `y`, de son implícitamente convertible a ese tipo, que es el tipo de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2369">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="d7bba-2370">En caso contrario, no se puede determinar ningún tipo de expresión y se produce un error de tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2370">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="d7bba-2371">El procesamiento en tiempo de ejecución de una expresión condicional del formulario `b ? x : y` consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2371">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="d7bba-2372">Primero, `b` se evalúa y la `bool` valor `b` viene determinada:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2372">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="d7bba-2373">Si una conversión implícita del tipo de `b` a `bool` existe, se realiza esta conversión implícita para producir un `bool` valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2373">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="d7bba-2374">En caso contrario, el `operator true` definido por el tipo de `b` se invoca para generar un `bool` valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2374">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="d7bba-2375">Si el `bool` es el valor generado por el paso anterior `true`, a continuación, `x` se evalúa y se convierte al tipo de la expresión condicional, y esto se convierte en el resultado de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2375">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="d7bba-2376">En caso contrario, `y` se evalúa y se convierte al tipo de la expresión condicional, y esto se convierte en el resultado de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2376">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="d7bba-2377">Expresiones de función anónima</span><span class="sxs-lookup"><span data-stu-id="d7bba-2377">Anonymous function expressions</span></span>

<span data-ttu-id="d7bba-2378">Un ***función anónima*** es una expresión que representa una definición de método "en línea".</span><span class="sxs-lookup"><span data-stu-id="d7bba-2378">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="d7bba-2379">Una función anónima no tiene un valor o el tipo de por sí, pero puede convertirse en un tipo de árbol de expresión o delegado compatible.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2379">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="d7bba-2380">La evaluación de una conversión de función anónima depende del tipo de destino de la conversión: Si es un tipo de delegado, la conversión se evalúa como un valor de delegado, hacer referencia al método que define la función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2380">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="d7bba-2381">Si es un tipo de árbol de expresión, la conversión se evalúa como un árbol de expresión que representa la estructura del método como una estructura de objeto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2381">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="d7bba-2382">Por motivos históricos hay dos variantes sintácticas de funciones anónimas, es decir, *lambda_expression*s y *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2382">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="d7bba-2383">Para casi todos los propósitos, *lambda_expression*s son más concisas y expresivas que *anonymous_method_expression*s, que permanecen en el idioma para compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2383">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

<span data-ttu-id="d7bba-2384">El `=>` operador tiene la misma prioridad que la asignación (`=`) y es asociativo a la derecha.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2384">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="d7bba-2385">Una función anónima con el `async` modificador es una función asincrónica y sigue las reglas descritas en [iteradores](classes.md#iterators).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2385">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="d7bba-2386">Los parámetros de una función anónima en forma de un *lambda_expression* puede estar escrito explícitamente o implícitamente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2386">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="d7bba-2387">En una lista de parámetros con tipo explícito, se indica explícitamente el tipo de cada parámetro.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2387">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="d7bba-2388">En una lista de parámetros con tipo implícito se deducen los tipos de los parámetros de contexto en el que se produce la función anónima; en concreto, cuando la función anónima se convierte en un tipo delegado compatible o el tipo de árbol de expresión, que proporciona el tipo los tipos de parámetros ([conversiones de función anónima](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2388">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="d7bba-2389">En una función anónima con un parámetro único, implícito, se pueden omitir los paréntesis de la lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2389">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="d7bba-2390">En otras palabras, una función anónima del formulario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2390">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="d7bba-2391">se puede abreviar a</span><span class="sxs-lookup"><span data-stu-id="d7bba-2391">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="d7bba-2392">La lista de parámetros de una función anónima en forma de un *anonymous_method_expression* es opcional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2392">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="d7bba-2393">Si no especifica, los parámetros de tipo deben establecerse explícitamente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2393">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="d7bba-2394">Si no, la función anónima es convertible a un delegado con cualquier parámetro de lista que no contenga `out` parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2394">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="d7bba-2395">Un *bloque* cuerpo de una función anónima es accesible ([puntos finales y alcance](statements.md#end-points-and-reachability)) a menos que la función anónima que se produce dentro de una instrucción inaccesible.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2395">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="d7bba-2396">Algunos ejemplos de funciones anónimas siguen siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2396">Some examples of anonymous functions follow below:</span></span>

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

<span data-ttu-id="d7bba-2397">El comportamiento de *lambda_expression*s y *anonymous_method_expression*s es el mismo, excepto los siguientes puntos:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2397">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="d7bba-2398">*anonymous_method_expression*s permiten la lista de parámetros para omitir por completo, lo que produce la conversión de varianza para los tipos de cualquier lista de parámetros con valores de delegado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2398">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="d7bba-2399">*lambda_expression*s permite a los tipos de parámetro se omite y se deduce, mientras que *anonymous_method_expression*s requieren tipos de parámetro que se indique explícitamente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2399">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="d7bba-2400">El cuerpo de un *lambda_expression* puede ser una expresión o un bloque de instrucciones mientras que el cuerpo de un *anonymous_method_expression* debe ser un bloque de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2400">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="d7bba-2401">Solo *lambda_expression*s tienen conversiones a tipos de árbol de expresión compatible ([tipos de árbol de expresión](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2401">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="d7bba-2402">Firmas de función anónima</span><span class="sxs-lookup"><span data-stu-id="d7bba-2402">Anonymous function signatures</span></span>

<span data-ttu-id="d7bba-2403">El elemento opcional *anonymous_function_signature* de una función anónima define los nombres y, opcionalmente, los tipos de los parámetros formales para la función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2403">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="d7bba-2404">El ámbito de los parámetros de la función anónima es el *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2404">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="d7bba-2405">([Ámbitos](basic-concepts.md#scopes)) junto con la lista de parámetros (si se indica) anónimo-method-body constituye un espacio de declaración ([declaraciones](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2405">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="d7bba-2406">Por tanto, es un error en tiempo de compilación para el nombre de un parámetro de la función anónima para que coincida con el nombre de una variable local, constante local o parámetro cuyo ámbito incluye el *anonymous_method_expression* o *lambda_ expresión*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2406">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="d7bba-2407">Si tiene una función anónima un *explicit_anonymous_function_signature*, a continuación, el conjunto de tipos de delegado compatibles y los tipos de árbol de expresión está restringido a los que tienen los mismos tipos de parámetros y modificadores en el mismo orden.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2407">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="d7bba-2408">A diferencia de las conversiones de grupo de métodos ([conversiones de métodos de grupo](conversions.md#method-group-conversions)), no se admite la contravarianza de tipos de parámetro de función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2408">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="d7bba-2409">Si no dispone de una función anónima un *anonymous_function_signature*, a continuación, el conjunto de tipos de delegado compatibles y los tipos de árbol de expresión está restringido a aquellos que no tienen ningún `out` parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2409">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="d7bba-2410">Tenga en cuenta que un *anonymous_function_signature* no puede incluir atributos o una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2410">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="d7bba-2411">No obstante, un *anonymous_function_signature* puede ser compatible con un tipo de delegado cuya lista de parámetros contiene una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2411">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="d7bba-2412">Tenga en cuenta también que la conversión a un tipo de árbol de expresión, incluso si compatible, todavía puede producir un error en tiempo de compilación ([tipos de árbol de expresión](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2412">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="d7bba-2413">Cuerpos de función anónima</span><span class="sxs-lookup"><span data-stu-id="d7bba-2413">Anonymous function bodies</span></span>

<span data-ttu-id="d7bba-2414">El cuerpo (*expresión* o *bloque*) de una función anónima está sujeto a las reglas siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2414">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="d7bba-2415">Si la función anónima incluye una firma, están disponibles en el cuerpo de los parámetros especificados en la firma.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2415">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="d7bba-2416">Si la función anónima no tiene una firma se puede convertir a un tipo de delegado o expresión con parámetros ([conversiones de función anónima](conversions.md#anonymous-function-conversions)), pero no se puede acceder a los parámetros en el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2416">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="d7bba-2417">Excepto para `ref` o `out` los parámetros especificados en la firma (si existe) de la envolvente más próximo función anónima, es un error en tiempo de compilación para el cuerpo tener acceso a un `ref` o `out` parámetro.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2417">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="d7bba-2418">Cuando el tipo de `this` es un tipo struct, es un error en tiempo de compilación para el cuerpo tener acceso a `this`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2418">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="d7bba-2419">Esto es cierto si el acceso es explícito (como en `this.x`) o implícita (como en `x` donde `x` es un miembro de instancia de la estructura).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2419">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="d7bba-2420">Esta regla simplemente prohíbe dicho acceso y no afecta a si los resultados de búsqueda de miembros en un miembro de la estructura.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2420">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="d7bba-2421">El cuerpo tiene acceso a las variables externas ([Outer variables](expressions.md#outer-variables)) de la función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2421">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="d7bba-2422">Acceso de una variable externa hará referencia a la instancia de la variable que está activa en el momento del *lambda_expression* o *anonymous_method_expression* se evalúa ([evaluación de las expresiones de función anónima](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2422">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="d7bba-2423">Es un error en tiempo de compilación para el cuerpo contenga un `goto` instrucción, `break` instrucción, o `continue` instrucción cuyo destino es fuera del cuerpo o en el cuerpo de una función anónima contenida.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2423">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="d7bba-2424">Un `return` instrucción del cuerpo devuelve el control de una invocación de envolvente función anónima, no desde el miembro de función envolvente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2424">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="d7bba-2425">Una expresión especificada en un `return` instrucción debe poder convertirse implícitamente al tipo de valor devuelto del tipo de delegado o tipo de árbol de expresión al que envolvente *lambda_expression* o *anonymous_ method_expression* se convierte ([conversiones de función anónima](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2425">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="d7bba-2426">No se especifica explícitamente si hay alguna forma de ejecutar el bloque de una función anónima distinto a través de evaluación y la invocación de la *lambda_expression* o *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2426">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="d7bba-2427">En concreto, el compilador puede optar por implementar una función anónima si sintetiza uno o más con el nombre de los métodos o tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2427">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="d7bba-2428">Los nombres de esos elementos sintetizados deben ser de un formulario reservado para uso del compilador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2428">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="d7bba-2429">La resolución de sobrecarga y las funciones anónimas</span><span class="sxs-lookup"><span data-stu-id="d7bba-2429">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="d7bba-2430">Las funciones anónimas en una lista de argumentos participan en la inferencia de tipos y resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2430">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="d7bba-2431">Consulte [inferencia](expressions.md#type-inference) y [de resolución de sobrecarga](expressions.md#overload-resolution) para ver las reglas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2431">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="d7bba-2432">El ejemplo siguiente muestra el efecto de las funciones anónimas en la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2432">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

<span data-ttu-id="d7bba-2433">El `ItemList<T>` clase tiene dos `Sum` métodos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2433">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="d7bba-2434">Cada uno toma una `selector` argumento, que extrae el valor a suma a través de un elemento de lista.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2434">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="d7bba-2435">El valor extraído puede ser un `int` o un `double` y la suma resultante sea del mismo modo un `int` o `double`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2435">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="d7bba-2436">El `Sum` métodos por ejemplo podrían utilizarse para calcular las sumas de una lista de líneas de detalles en un orden.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2436">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

<span data-ttu-id="d7bba-2437">En la primera invocación de `orderDetails.Sum`, ambos `Sum` métodos son aplicables porque la función anónima `d => d. UnitCount` es compatible con ambos `Func<Detail,int>` y `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2437">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="d7bba-2438">Sin embargo, la resolución de sobrecarga selecciona la primera `Sum` método porque la conversión a `Func<Detail,int>` es mejor que la conversión a `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2438">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="d7bba-2439">En la segunda invocación de `orderDetails.Sum`, solo el segundo `Sum` método es aplicable porque la función anónima `d => d.UnitPrice * d.UnitCount` genera un valor de tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2439">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="d7bba-2440">Por lo tanto, la sobrecarga resolución elige la segunda `Sum` método para esa invocación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2440">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="d7bba-2441">Funciones anónimas y el enlace dinámico</span><span class="sxs-lookup"><span data-stu-id="d7bba-2441">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="d7bba-2442">Una función anónima no puede ser un receptor, un argumento o un operando de una operación dinámica enlazada.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2442">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="d7bba-2443">Variables externas</span><span class="sxs-lookup"><span data-stu-id="d7bba-2443">Outer variables</span></span>

<span data-ttu-id="d7bba-2444">Cualquier variable local, el parámetro de valor o la matriz de parámetros cuyo ámbito incluye el *lambda_expression* o *anonymous_method_expression* se denomina un ***variable externa*** de la función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2444">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="d7bba-2445">En un miembro de función de la instancia de una clase, el `this` valor se considera un parámetro de valor y es una variable externa de cualquier función anónima contenida dentro del miembro de función.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2445">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="d7bba-2446">Captura de variables externas</span><span class="sxs-lookup"><span data-stu-id="d7bba-2446">Captured outer variables</span></span>

<span data-ttu-id="d7bba-2447">Cuando se hace referencia a una variable externa por una función anónima, se dice que la variable externa han sido ***capturan*** por la función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2447">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="d7bba-2448">Normalmente, la duración de una variable local está limitada por la ejecución del bloque o instrucción que está asociada ([variables locales](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2448">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="d7bba-2449">Sin embargo, la duración de una variable externa capturada se extiende al menos hasta que el delegado o árbol de expresión que se crea a partir de la función anónima se vuelve apto para la recolección.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2449">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="d7bba-2450">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2450">In the example</span></span>
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
<span data-ttu-id="d7bba-2451">la variable local `x` capturada por la función anónima y la duración de `x` se extiende al menos hasta que el delegado devuelto por `F` se convierte en apto para la recolección de elementos no utilizados (que no ocurre hasta el final de el programa).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2451">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="d7bba-2452">Puesto que cada invocación de la función anónima opera en la misma instancia de `x`, el resultado del ejemplo es:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2452">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```
1
2
3
```

<span data-ttu-id="d7bba-2453">Cuando se captura una variable local o un parámetro de valor mediante una función anónima, el parámetro o variable local ya no se considera que una variable fija ([fijos y variables móviles](unsafe-code.md#fixed-and-moveable-variables)), pero en su lugar, se considera un moveable variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2453">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="d7bba-2454">Por lo tanto, cualquier `unsafe` en primer lugar debe usar el código que toma la dirección de una variable externa capturada el `fixed` instrucción para fijar la variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2454">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="d7bba-2455">Tenga en cuenta que a diferencia de una variable no capturada, una variable local capturada se puede exponer simultáneamente a varios subprocesos de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2455">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="d7bba-2456">Creación de instancias de las variables locales</span><span class="sxs-lookup"><span data-stu-id="d7bba-2456">Instantiation of local variables</span></span>

<span data-ttu-id="d7bba-2457">Se considera una variable local ***crea una instancia*** cuando entra la ejecución en el ámbito de la variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2457">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="d7bba-2458">Por ejemplo, cuando se invoca el método siguiente, la variable local `x` se crea una instancia e inicializa tres veces, una vez para cada iteración del bucle.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2458">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="d7bba-2459">Sin embargo, mueva la declaración de `x` fuera de los resultados de bucle en una única instancia de `x`:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2459">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="d7bba-2460">Cuando no se capturan, no hay ninguna manera de observar exactamente, ¿con qué frecuencia se crea una instancia de una variable local, porque la duración de la creación de instancias no es contiguas, es posible que cada instancia simplemente utilizar la misma ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2460">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="d7bba-2461">Sin embargo, cuando una función anónima captura una variable local, los efectos de la creación de instancias se hacen evidentes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2461">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="d7bba-2462">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2462">The example</span></span>
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
<span data-ttu-id="d7bba-2463">genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2463">produces the output:</span></span>
```
1
3
5
```

<span data-ttu-id="d7bba-2464">Sin embargo, cuando la declaración de `x` se mueve fuera del bucle:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2464">However, when the declaration of `x` is moved outside the loop:</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
<span data-ttu-id="d7bba-2465">El resultado es:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2465">the output is:</span></span>
```
5
5
5
```

<span data-ttu-id="d7bba-2466">Si un bucle for declara una variable de iteración, esa variable en sí se considera que se declara fuera del bucle.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2466">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="d7bba-2467">Por lo tanto, si se cambia el ejemplo para capturar la propia variable de iteración:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2467">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="d7bba-2468">se captura solo una instancia de la variable de iteración, lo que genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2468">only one instance of the iteration variable is captured, which produces the output:</span></span>
```
3
3
3
```

<span data-ttu-id="d7bba-2469">Es posible que los delegados de función anónima compartir algunas de las variables capturadas aún tiene instancias separadas de otras.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2469">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="d7bba-2470">Por ejemplo, si `F` se cambia a</span><span class="sxs-lookup"><span data-stu-id="d7bba-2470">For example, if `F` is changed to</span></span>
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
<span data-ttu-id="d7bba-2471">la misma instancia de capturan de los tres delegados `x` pero instancias independientes de `y`, y el resultado es:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2471">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```
1 1
2 1
3 1
```

<span data-ttu-id="d7bba-2472">Funciones anónimas independientes pueden capturar la misma instancia de una variable externa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2472">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="d7bba-2473">En el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2473">In the example:</span></span>
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
<span data-ttu-id="d7bba-2474">las dos funciones anónimas capturan la misma instancia de la variable local `x`, y se puede, por tanto, "comunican" a través de esa variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2474">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="d7bba-2475">El resultado del ejemplo es:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2475">The output of the example is:</span></span>
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="d7bba-2476">Evaluación de expresiones de función anónima</span><span class="sxs-lookup"><span data-stu-id="d7bba-2476">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="d7bba-2477">Una función anónima `F` siempre se deben convertir a un tipo delegado `D` o un tipo de árbol de expresión `E`, ya sea directamente o a través de la ejecución de una expresión de creación de delegado `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2477">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="d7bba-2478">Esta conversión determina el resultado de la función anónima, tal como se describe en [conversiones de función anónima](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2478">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="d7bba-2479">Expresiones de consulta</span><span class="sxs-lookup"><span data-stu-id="d7bba-2479">Query expressions</span></span>

<span data-ttu-id="d7bba-2480">***Las expresiones de consulta*** proporcionan una sintaxis de lenguaje integrado para las consultas que es similar a lenguajes de consulta jerárquica y relacional como SQL y XQuery.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2480">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

<span data-ttu-id="d7bba-2481">Una expresión de consulta comienza con un `from` cláusula y finaliza con ya sea un `select` o `group` cláusula.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2481">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="d7bba-2482">Inicial `from` cláusula puede ir seguida de cero o más `from`, `let`, `where`, `join` o `orderby` cláusulas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2482">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="d7bba-2483">Cada `from` cláusula es un generador de introducir un ***variable de rango*** cuyos intervalos van a través de los elementos de un ***secuencia***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2483">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="d7bba-2484">Cada `let` cláusula introduce una variable de rango que representa un valor calculado por medio de variables de rango anteriores.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2484">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="d7bba-2485">Cada `where` cláusula es un filtro que excluye los elementos del resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2485">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="d7bba-2486">Cada `join` cláusula compara las claves especificadas de la secuencia de origen con las claves de otra secuencia, produciendo los pares coincidentes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2486">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="d7bba-2487">Cada `orderby` cláusula reordena los elementos según los criterios especificados. El último `select` o `group` cláusula especifica la forma del resultado en cuanto a las variables de rango.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2487">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="d7bba-2488">Por último, un `into` cláusula se puede usar para "unir" consultas tratando los resultados de una consulta como un generador en una consulta posterior.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2488">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="d7bba-2489">Ambigüedades en expresiones de consulta</span><span class="sxs-lookup"><span data-stu-id="d7bba-2489">Ambiguities in query expressions</span></span>

<span data-ttu-id="d7bba-2490">Las expresiones de consulta contienen un número de "palabras clave contextuales", es decir, los identificadores que tienen un significado especial en un contexto determinado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2490">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="d7bba-2491">En concreto, estos son `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` y `by`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2491">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="d7bba-2492">Con el fin de evitar la ambigüedad en las expresiones de consulta causadas por el uso combinado de estos identificadores como palabras clave o nombres simples, estos identificadores se consideran palabras clave al que se producen en cualquier lugar dentro de una expresión de consulta.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2492">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="d7bba-2493">Para ello, una expresión de consulta es cualquier expresión que comienza por "`from identifier`"seguido de cualquier token excepto"`;`","`=`"o"`,`".</span><span class="sxs-lookup"><span data-stu-id="d7bba-2493">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="d7bba-2494">Para poder usar estas palabras como identificadores dentro de una expresión de consulta, pueden agregarse como prefijo "`@`" ([identificadores](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2494">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="d7bba-2495">Conversión de expresiones de consulta</span><span class="sxs-lookup"><span data-stu-id="d7bba-2495">Query expression translation</span></span>

<span data-ttu-id="d7bba-2496">El lenguaje C# no especifica la semántica de ejecución de expresiones de consulta.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2496">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="d7bba-2497">En su lugar, las expresiones de consulta se traducen las invocaciones de métodos que se adhieren a la *patrón de expresión de consulta* ([el patrón de expresión de consulta](expressions.md#the-query-expression-pattern)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2497">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="d7bba-2498">En concreto, las expresiones de consulta se traducen las invocaciones de métodos denominados `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, y `Cast`. Estos métodos se esperan que tengan firmas determinadas y tipos de resultados, como se describe en [el patrón de expresión de consulta](expressions.md#the-query-expression-pattern).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2498">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="d7bba-2499">Estos métodos pueden ser métodos de instancia del objeto que se va a consultar o métodos de extensión que son externos al objeto, y que implementan la ejecución real de la consulta.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2499">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="d7bba-2500">La traducción de expresiones de consulta a las invocaciones de método es una asignación sintáctica que tiene lugar antes de cualquier tipo de enlace o se ha realizado la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2500">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="d7bba-2501">La traducción se garantiza que sea sintácticamente correcto, pero no se garantiza para generar código de C# semánticamente correcto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2501">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="d7bba-2502">Siguiendo la traducción de expresiones de consulta, se procesan las invocaciones de método resultante como invocaciones de método normal y, a su vez, esto puede revelar errores, por ejemplo si no existen los métodos, si los argumentos tienen tipos incorrectos, o si los métodos son genéricos y se produce un error en la inferencia de tipos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2502">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="d7bba-2503">Una expresión de consulta se procesa mediante la aplicación varias veces las traducciones siguientes hasta que ninguna reducciones adicionales.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2503">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="d7bba-2504">Las traducciones se enumeran en orden de aplicación: cada sección se da por supuesto que las traducciones en las secciones anteriores se han realizado de forma exhaustiva y una vez agotado, una sección de no más adelante se revisará en el procesamiento de la misma expresión de consulta.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2504">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="d7bba-2505">No se permite la asignación a variables de rango en expresiones de consulta.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2505">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="d7bba-2506">Sin embargo se permite una implementación de C# no siempre exigir esta restricción, ya que esto a veces no sea posible con el esquema de traducción sintáctica presentado aquí.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2506">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="d7bba-2507">Ciertas traducciones insertan las variables de rango con identificadores transparentes denotados por `*`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2507">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="d7bba-2508">Las propiedades especiales de identificadores transparentes se tratan con más detalle en [identificadores transparentes](expressions.md#transparent-identifiers).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2508">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="d7bba-2509">Cláusulas SELECT y groupby con continuaciones</span><span class="sxs-lookup"><span data-stu-id="d7bba-2509">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="d7bba-2510">Una expresión de consulta con una continuación</span><span class="sxs-lookup"><span data-stu-id="d7bba-2510">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="d7bba-2511">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2511">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="d7bba-2512">Las traducciones en las secciones siguientes se suponen que las consultas tienen no `into` continuaciones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2512">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="d7bba-2513">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2513">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="d7bba-2514">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2514">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="d7bba-2515">la traducción de final de los cuales es</span><span class="sxs-lookup"><span data-stu-id="d7bba-2515">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="d7bba-2516">Tipos de variable de rango explícito</span><span class="sxs-lookup"><span data-stu-id="d7bba-2516">Explicit range variable types</span></span>

<span data-ttu-id="d7bba-2517">Un `from` cláusula que especifica explícitamente un tipo de variable de rango</span><span class="sxs-lookup"><span data-stu-id="d7bba-2517">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="d7bba-2518">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2518">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="d7bba-2519">Un `join` cláusula que especifica explícitamente un tipo de variable de rango</span><span class="sxs-lookup"><span data-stu-id="d7bba-2519">A `join` clause that explicitly specifies a range variable type</span></span>
```
join T x in e on k1 equals k2
```
<span data-ttu-id="d7bba-2520">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2520">is translated into</span></span>
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="d7bba-2521">Las traducciones en las secciones siguientes se suponen que las consultas no tienen ningún tipo de variable de rango explícito.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2521">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="d7bba-2522">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2522">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="d7bba-2523">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2523">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="d7bba-2524">la traducción de final de los cuales es</span><span class="sxs-lookup"><span data-stu-id="d7bba-2524">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="d7bba-2525">Tipos de variable de rango explícitas son útiles para consultar las colecciones que implementan la no genérica `IEnumerable` interfaz, pero no genérico `IEnumerable<T>` interfaz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2525">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="d7bba-2526">En el ejemplo anterior, esto sería el caso si `customers` eran de tipo `ArrayList`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2526">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="d7bba-2527">Expresiones de consulta degenerado</span><span class="sxs-lookup"><span data-stu-id="d7bba-2527">Degenerate query expressions</span></span>

<span data-ttu-id="d7bba-2528">Una expresión de consulta del formulario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2528">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="d7bba-2529">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2529">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="d7bba-2530">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2530">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="d7bba-2531">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2531">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="d7bba-2532">Una expresión de consulta degenerado es aquella que selecciona los elementos del origen de forma trivial.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2532">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="d7bba-2533">Una fase posterior de la traducción quita degeneradas consultas introducidas por otros pasos de traducción al reemplazarlos con su origen.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2533">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="d7bba-2534">Es importante sin embargo, para asegurarse de que el resultado de una consulta de expresión nunca es el objeto de origen, ya que podría revelar el tipo y la identidad del origen al cliente de la consulta.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2534">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="d7bba-2535">Por lo tanto, este paso protege degeneradas consultas escritas directamente en el código fuente llamando explícitamente a `Select` en el origen.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2535">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="d7bba-2536">Es decisión los implementadores de `Select` y otros operadores de consulta para asegurarse de que estos métodos no devuelven nunca el propio objeto de origen.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2536">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="d7bba-2537">Desde, permiten, where, cláusulas join y orderby</span><span class="sxs-lookup"><span data-stu-id="d7bba-2537">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="d7bba-2538">Una expresión de consulta con un segundo `from` cláusula seguido por un `select` cláusula</span><span class="sxs-lookup"><span data-stu-id="d7bba-2538">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="d7bba-2539">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2539">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="d7bba-2540">Una expresión de consulta con un segundo `from` cláusula seguido de algo que no sea un `select` cláusula:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2540">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="d7bba-2541">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2541">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="d7bba-2542">Una expresión de consulta con un `let` cláusula</span><span class="sxs-lookup"><span data-stu-id="d7bba-2542">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="d7bba-2543">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2543">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="d7bba-2544">Una expresión de consulta con un `where` cláusula</span><span class="sxs-lookup"><span data-stu-id="d7bba-2544">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="d7bba-2545">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2545">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="d7bba-2546">Una expresión de consulta con un `join` cláusula sin un `into` seguido por un `select` cláusula</span><span class="sxs-lookup"><span data-stu-id="d7bba-2546">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="d7bba-2547">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2547">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="d7bba-2548">Una expresión de consulta con un `join` cláusula sin un `into` seguido de algo que no sea un `select` cláusula</span><span class="sxs-lookup"><span data-stu-id="d7bba-2548">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="d7bba-2549">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2549">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="d7bba-2550">Una expresión de consulta con un `join` cláusula con una `into` seguido por un `select` cláusula</span><span class="sxs-lookup"><span data-stu-id="d7bba-2550">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="d7bba-2551">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2551">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="d7bba-2552">Una expresión de consulta con un `join` cláusula con una `into` seguido de algo que no sea un `select` cláusula</span><span class="sxs-lookup"><span data-stu-id="d7bba-2552">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="d7bba-2553">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2553">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="d7bba-2554">Una expresión de consulta con un `orderby` cláusula</span><span class="sxs-lookup"><span data-stu-id="d7bba-2554">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="d7bba-2555">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2555">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="d7bba-2556">Si una ordenación cláusula especifica un `descending` indicador de dirección, una invocación de `OrderByDescending` o `ThenByDescending` se produce en su lugar.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2556">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="d7bba-2557">Las traducciones siguientes se suponen que no hay ningún `let`, `where`, `join` o `orderby` cláusulas y no más de una inicial `from` cláusula en cada expresión de consulta.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2557">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="d7bba-2558">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2558">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="d7bba-2559">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2559">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="d7bba-2560">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2560">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="d7bba-2561">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2561">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="d7bba-2562">la traducción de final de los cuales es</span><span class="sxs-lookup"><span data-stu-id="d7bba-2562">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="d7bba-2563">donde `x` es un identificador generado por el compilador que en caso contrario, no es visible y accesible.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2563">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="d7bba-2564">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2564">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="d7bba-2565">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2565">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="d7bba-2566">la traducción de final de los cuales es</span><span class="sxs-lookup"><span data-stu-id="d7bba-2566">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="d7bba-2567">donde `x` es un identificador generado por el compilador que en caso contrario, no es visible y accesible.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2567">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="d7bba-2568">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2568">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="d7bba-2569">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2569">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="d7bba-2570">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2570">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="d7bba-2571">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2571">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="d7bba-2572">la traducción de final de los cuales es</span><span class="sxs-lookup"><span data-stu-id="d7bba-2572">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="d7bba-2573">donde `x` y `y` son los identificadores generados por el compilador que de otro modo invisible y no son accesibles.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2573">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="d7bba-2574">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2574">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="d7bba-2575">tiene la traducción final</span><span class="sxs-lookup"><span data-stu-id="d7bba-2575">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="d7bba-2576">Cláusulas SELECT</span><span class="sxs-lookup"><span data-stu-id="d7bba-2576">Select clauses</span></span>

<span data-ttu-id="d7bba-2577">Una expresión de consulta del formulario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2577">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="d7bba-2578">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2578">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="d7bba-2579">excepto cuando v es el identificador x, es simplemente la traducción</span><span class="sxs-lookup"><span data-stu-id="d7bba-2579">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="d7bba-2580">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2580">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="d7bba-2581">simplemente se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2581">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="d7bba-2582">Agrupar cláusulas</span><span class="sxs-lookup"><span data-stu-id="d7bba-2582">Groupby clauses</span></span>

<span data-ttu-id="d7bba-2583">Una expresión de consulta del formulario</span><span class="sxs-lookup"><span data-stu-id="d7bba-2583">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="d7bba-2584">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2584">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="d7bba-2585">excepto cuando v es el identificador x, que es la traducción</span><span class="sxs-lookup"><span data-stu-id="d7bba-2585">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="d7bba-2586">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2586">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="d7bba-2587">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2587">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="d7bba-2588">Identificadores transparentes</span><span class="sxs-lookup"><span data-stu-id="d7bba-2588">Transparent identifiers</span></span>

<span data-ttu-id="d7bba-2589">Ciertas traducciones insertan las variables de rango con ***identificadores transparentes*** denotado por `*`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2589">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="d7bba-2590">Identificadores transparentes no son una característica del lenguaje adecuado; Existen sólo como un paso intermedio en el proceso de traducción de la expresión de consulta.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2590">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="d7bba-2591">Cuando una traducción de la consulta inyecta un identificador transparente, aún más pasos de traducción propagan el identificador transparente a las funciones anónimas y los inicializadores de objeto anónimo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2591">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="d7bba-2592">En esos contextos, identificadores transparentes tienen el siguiente comportamiento:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2592">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="d7bba-2593">Cuando se produce un identificador transparente como un parámetro en una función anónima, los miembros del tipo anónimo asociado son automáticamente en el ámbito en el cuerpo de la función anónima.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2593">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="d7bba-2594">Cuando un miembro con un identificador transparente está en el ámbito, los miembros de ese miembro están en ámbito también.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2594">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="d7bba-2595">Cuando se produce un identificador transparente como un declarador de miembro en un inicializador de objeto anónimo, presenta a un miembro con un identificador transparente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2595">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="d7bba-2596">En los pasos de traducción que se ha descrito anteriormente, siempre se introducen los identificadores transparentes junto con los tipos anónimos, con la intención de capturar varias variables de rango como miembros de un solo objeto.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2596">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="d7bba-2597">Una implementación de C# se puede usar un mecanismo diferente que los tipos anónimos para agrupar varias variables de rango.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2597">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="d7bba-2598">Los ejemplos de traducción siguientes se supone que se usan tipos anónimos y mostrar los identificadores de transparencia se puede traducir inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2598">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="d7bba-2599">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2599">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="d7bba-2600">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2600">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="d7bba-2601">es más que traducen en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2601">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="d7bba-2602">que, cuando se borran los identificadores transparentes, es equivalente a</span><span class="sxs-lookup"><span data-stu-id="d7bba-2602">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="d7bba-2603">donde `x` es un identificador generado por el compilador que en caso contrario, no es visible y accesible.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2603">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="d7bba-2604">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2604">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="d7bba-2605">se traduce en</span><span class="sxs-lookup"><span data-stu-id="d7bba-2605">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="d7bba-2606">que es más reducción a</span><span class="sxs-lookup"><span data-stu-id="d7bba-2606">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="d7bba-2607">la traducción de final de los cuales es</span><span class="sxs-lookup"><span data-stu-id="d7bba-2607">the final translation of which is</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
<span data-ttu-id="d7bba-2608">donde `x`, `y`, y `z` son los identificadores generados por el compilador que de otro modo invisible y no son accesibles.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2608">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="d7bba-2609">El patrón de expresión de consulta</span><span class="sxs-lookup"><span data-stu-id="d7bba-2609">The query expression pattern</span></span>

<span data-ttu-id="d7bba-2610">El ***patrón de expresión de consulta*** establece un patrón de métodos de tipos pueden implementar para admitir las expresiones de consulta.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2610">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="d7bba-2611">Dado que las expresiones de consulta se traducen a las invocaciones de método por medio de una asignación sintáctica, tipos tienen una flexibilidad considerable en cómo implementan el patrón de expresión de consulta.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2611">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="d7bba-2612">Por ejemplo, los métodos del patrón pueden implementarse como métodos de instancia o como métodos de extensión porque los dos tienen la misma sintaxis de invocación, y pueden solicitar los métodos delegados o árboles de expresión porque se pueden convertibles a ambas funciones anónimas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2612">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="d7bba-2613">La forma recomendada de un tipo genérico `C<T>` que admite el patrón de expresión de consulta se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2613">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="d7bba-2614">Un tipo genérico sirve para ilustrar las relaciones entre los tipos de parámetro y el resultado correctas, pero es posible implementar el patrón para tipos no genéricos también.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2614">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

<span data-ttu-id="d7bba-2615">Los métodos anteriores usan los tipos de delegado genérico `Func<T1,R>` y `Func<T1,T2,R>`, pero éstos podrían igualmente bien usado otros tipos de árbol de expresión o delegado con las mismas relaciones en los tipos de parámetro y resultado.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2615">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="d7bba-2616">Tenga en cuenta la relación entre recomendada `C<T>` y `O<T>` lo que garantiza que el `ThenBy` y `ThenByDescending` métodos sólo están disponibles en el resultado de una `OrderBy` o `OrderByDescending`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2616">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="d7bba-2617">Tenga en cuenta también la forma recomendada de resultado de `GroupBy` --una secuencia de secuencias, donde cada secuencia interna tiene más `Key` propiedad.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2617">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="d7bba-2618">El `System.Linq` espacio de nombres proporciona una implementación del patrón de operador de consulta para cualquier tipo que implementa el `System.Collections.Generic.IEnumerable<T>` interfaz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2618">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="d7bba-2619">Operadores de asignación</span><span class="sxs-lookup"><span data-stu-id="d7bba-2619">Assignment operators</span></span>

<span data-ttu-id="d7bba-2620">Los operadores de asignación asignación un nuevo valor a una variable, una propiedad, un evento o un elemento del indizador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2620">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

<span data-ttu-id="d7bba-2621">El operando izquierdo de una asignación debe ser una expresión que se clasifica como una variable, un acceso de propiedad, un acceso de indizador o un acceso a evento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2621">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="d7bba-2622">El `=` se denomina operador la ***operador de asignación simple***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2622">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="d7bba-2623">El valor del operando derecho asigna al elemento de variable, propiedad o indizador dado por el operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2623">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="d7bba-2624">El operando izquierdo del operador de asignación simple no puede ser un acceso a evento (excepto como se describe en [eventos como campos](classes.md#field-like-events)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2624">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="d7bba-2625">Se describe el operador de asignación simple en [asignación Simple](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2625">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="d7bba-2626">Los operadores de asignación no sea el `=` operador se denominan el ***operadores de asignación compuesta***.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2626">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="d7bba-2627">Estos operadores realizan la operación indicada en los dos operandos y, a continuación, asignan el valor resultante para el elemento de variable, propiedad o indizador dado por el operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2627">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="d7bba-2628">Se describen los operadores de asignación compuesta en [asignación compuesta](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2628">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="d7bba-2629">El `+=` y `-=` operadores con una expresión de acceso de eventos como el operando izquierdo se denominan el *operadores de asignación de evento*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2629">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="d7bba-2630">Ningún otro operador de asignación es válido con acceso de un evento como el operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2630">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="d7bba-2631">Los operadores de asignación de eventos se describen en [asignación de eventos](expressions.md#event-assignment).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2631">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="d7bba-2632">Los operadores de asignación son asociativos a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2632">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="d7bba-2633">Por ejemplo, una expresión de formato `a = b = c` se evalúa como `a = (b = c)`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2633">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="d7bba-2634">Asignación simple</span><span class="sxs-lookup"><span data-stu-id="d7bba-2634">Simple assignment</span></span>

<span data-ttu-id="d7bba-2635">El `=` operador se denomina operador de asignación simple.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2635">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="d7bba-2636">Si el operando izquierdo de una asignación simple tiene el formato `E.P` o `E[Ei]` donde `E` tiene el tipo de tiempo de compilación `dynamic`, a continuación, se enlaza dinámicamente la asignación ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2636">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="d7bba-2637">En este caso es el tipo de tiempo de compilación de la expresión de asignación `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución según el tipo de tiempo de ejecución de `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2637">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="d7bba-2638">En una asignación simple, el operando derecho debe ser una expresión que es implícitamente convertible al tipo del operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2638">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="d7bba-2639">La operación asigna el valor del operando derecho para el elemento de variable, propiedad o indizador dado por el operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2639">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="d7bba-2640">El resultado de una expresión de asignación simple es el valor asignado al operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2640">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="d7bba-2641">El resultado tiene el mismo tipo que el operando izquierdo y siempre se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2641">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="d7bba-2642">Si el operando izquierdo es un acceso de propiedad o indizador, la propiedad o indizador debe tener un `set` descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2642">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="d7bba-2643">Si esto no es el caso, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2643">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-2644">El procesamiento en tiempo de ejecución de una asignación simple del formulario `x = y` consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2644">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="d7bba-2645">Si `x` se clasifica como una variable:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2645">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="d7bba-2646">`x` se evalúa para producir la variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2646">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="d7bba-2647">`y` se evalúa y, si es necesario, puede convertir al tipo de `x` a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2647">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="d7bba-2648">Si la variable proporcionada por `x` es un elemento de matriz de un *reference_type*, se realiza una comprobación de tiempo de ejecución para asegurarse de que el valor calculado para `y` es compatible con la instancia de la matriz de los cuales `x` es un elemento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2648">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="d7bba-2649">La comprobación se realiza correctamente si `y` es `null`, o si una conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) existe desde el tipo real de la instancia que hace referencia `y` al tipo de elemento real de la instancia de matriz que contiene `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2649">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="d7bba-2650">De lo contrario, se produce una excepción `System.ArrayTypeMismatchException`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2650">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="d7bba-2651">El valor resultante de la evaluación y la conversión de `y` se almacena en la ubicación proporcionada por la evaluación de `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2651">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="d7bba-2652">Si `x` se clasifica como un acceso de propiedad o indizador:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2652">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="d7bba-2653">La expresión de instancia (si `x` no `static`) y la lista de argumentos (si `x` es un acceso de indizador) asociado con `x` se evalúan, y los resultados se usan en la siguiente `set` invocación del descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2653">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="d7bba-2654">`y` se evalúa y, si es necesario, puede convertir al tipo de `x` a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2654">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="d7bba-2655">El `set` descriptor de acceso de `x` se invoca con el valor calculado para `y` como su `value` argumento.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2655">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="d7bba-2656">Las reglas de covarianza de matriz ([covarianza de matrices](arrays.md#array-covariance)) permiten que un valor de un tipo de matriz `A[]` sea una referencia a una instancia de un tipo de matriz `B[]`, siempre existe una conversión implícita de referencia de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2656">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="d7bba-2657">Debido a estas reglas, la asignación a un elemento de matriz de un *reference_type* requiere una comprobación en tiempo de ejecución para asegurarse de que el valor asignado es compatible con la instancia de matriz.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2657">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="d7bba-2658">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2658">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="d7bba-2659">la última asignación produce un `System.ArrayTypeMismatchException` debido a una instancia de `ArrayList` no pueden almacenarse en un elemento de un `string[]`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2659">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="d7bba-2660">Cuando una propiedad o indizador declarado en un *struct_type* es el destino de una asignación, la expresión de instancia asociado con la propiedad o indizador debe estar clasificada como una variable.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2660">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="d7bba-2661">Si la expresión de instancia se clasifica como un valor, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2661">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="d7bba-2662">Debido [acceso a miembros](expressions.md#member-access), la misma regla también se aplica a los campos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2662">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="d7bba-2663">Dadas las declaraciones:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2663">Given the declarations:</span></span>
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
<span data-ttu-id="d7bba-2664">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2664">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="d7bba-2665">las asignaciones a `p.X`, `p.Y`, `r.A`, y `r.B` se permiten porque `p` y `r` son variables.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2665">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="d7bba-2666">Sin embargo, en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2666">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="d7bba-2667">las asignaciones no son válidas, desde `r.A` y `r.B` no son variables.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2667">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="d7bba-2668">Asignación compuesta</span><span class="sxs-lookup"><span data-stu-id="d7bba-2668">Compound assignment</span></span>

<span data-ttu-id="d7bba-2669">Si el operando izquierdo de una asignación compuesta tiene el formato `E.P` o `E[Ei]` donde `E` tiene el tipo de tiempo de compilación `dynamic`, a continuación, se enlaza dinámicamente la asignación ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2669">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="d7bba-2670">En este caso es el tipo de tiempo de compilación de la expresión de asignación `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución según el tipo de tiempo de ejecución de `E`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2670">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="d7bba-2671">Una operación del formulario `x op= y` se procesa mediante la aplicación de la resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) como si la operación se ha escrito `x op y`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2671">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="d7bba-2672">A continuación,</span><span class="sxs-lookup"><span data-stu-id="d7bba-2672">Then,</span></span>

*  <span data-ttu-id="d7bba-2673">Si el tipo de valor devuelto del operador seleccionado es implícitamente convertible al tipo de `x`, la operación se evalúa como `x = x op y`, salvo que `x` se evalúa solo una vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2673">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="d7bba-2674">En caso contrario, si el operador seleccionado es un operador predefinido, si el tipo de valor devuelto del operador seleccionado es convertible explícitamente al tipo de `x`y si `y` es implícitamente convertible al tipo de `x` o el operador es un operador, de desplazamiento, a continuación, la operación se evalúa como `x = (T)(x op y)`, donde `T` es el tipo de `x`, salvo que `x` se evalúa solo una vez.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2674">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="d7bba-2675">En caso contrario, la asignación compuesta no es válida y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2675">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-2676">El término "se evalúa solo una vez" significa que, en la evaluación de `x op y`, los resultados de cualquier expresión constituyentes de `x` se guarda temporalmente y, a continuación, volver a usar al realizar la asignación a `x`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2676">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="d7bba-2677">Por ejemplo, en la asignación `A()[B()] += C()`, donde `A` es un método que devuelve `int[]`, y `B` y `C` son métodos que devuelven `int`, se invocan los métodos de una sola vez, en el orden `A`, `B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2677">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="d7bba-2678">Cuando el operando izquierdo de una asignación compuesta es un acceso a la propiedad o indizador, la propiedad o indizador debe tener tanto un `get` descriptor de acceso y un `set` descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2678">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="d7bba-2679">Si esto no es el caso, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2679">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-2680">La segunda regla mencionada permite `x op= y` se evalúen como `x = (T)(x op y)` en ciertos contextos.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2680">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="d7bba-2681">La regla existe de forma que los operadores predefinidos pueden usarse como operadores compuestos cuando el operando izquierdo es de tipo `sbyte`, `byte`, `short`, `ushort`, o `char`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2681">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="d7bba-2682">Incluso cuando ambos argumentos son de uno de esos tipos, los operadores predefinidos producen un resultado de tipo `int`, tal y como se describe en [Promociones numéricas binarias](expressions.md#binary-numeric-promotions).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2682">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="d7bba-2683">Por lo tanto, sin una conversión no sería posible asignar el resultado al operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2683">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="d7bba-2684">El efecto intuitivo de la regla para los operadores predefinidos es simplemente que `x op= y` está permitido si ambos de `x op y` y `x = y` están permitidas.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2684">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="d7bba-2685">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2685">In the example</span></span>
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
<span data-ttu-id="d7bba-2686">el motivo intuitivo de cada error es que una asignación simple correspondiente también habría sido un error.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2686">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="d7bba-2687">Esto también significa que admiten operaciones de asignación compuesta eleva las operaciones.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2687">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="d7bba-2688">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d7bba-2688">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="d7bba-2689">el operador de elevación `+(int?,int?)` se utiliza.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2689">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="d7bba-2690">Asignación de eventos</span><span class="sxs-lookup"><span data-stu-id="d7bba-2690">Event assignment</span></span>

<span data-ttu-id="d7bba-2691">Si el operando izquierdo de un `+=` o `-=` operador se clasifica como un acceso de eventos, a continuación, la expresión se evalúa como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2691">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="d7bba-2692">La expresión de instancia, si los hay, de acceso a lo eventos se evalúa.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2692">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="d7bba-2693">El operando derecho de la `+=` o `-=` operador se evalúa y, si es necesario, puede convertir al tipo del operando izquierdo a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2693">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="d7bba-2694">Se invoca un descriptor de acceso de eventos del evento, con la lista de argumentos que se compone del operando derecho, después de la evaluación y, si es necesario, conversión.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2694">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="d7bba-2695">Si el operador era `+=`, el `add` se invoca el descriptor de acceso; si el operador era `-=`, el `remove` se invoca al descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2695">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="d7bba-2696">Una expresión de evento de asignación no producen un valor.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2696">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="d7bba-2697">Por lo tanto, una expresión de asignación de eventos solo es válida en el contexto de un *statement_expression* ([las instrucciones de expresión](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2697">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="d7bba-2698">Expresión</span><span class="sxs-lookup"><span data-stu-id="d7bba-2698">Expression</span></span>

<span data-ttu-id="d7bba-2699">Un *expresión* sea un *non_assignment_expression* o un *asignación*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2699">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a><span data-ttu-id="d7bba-2700">Expresiones de constante</span><span class="sxs-lookup"><span data-stu-id="d7bba-2700">Constant expressions</span></span>

<span data-ttu-id="d7bba-2701">Un *constant_expression* es una expresión que se puede evaluar por completo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2701">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="d7bba-2702">Una expresión constante debe ser el `null` literal o un valor con uno de los siguientes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, o cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2702">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="d7bba-2703">Las siguientes construcciones de sólo se permiten en expresiones constantes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2703">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="d7bba-2704">Literales (incluido el `null` literal).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2704">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="d7bba-2705">Las referencias a `const` miembros de tipos de clase y estructura.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2705">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="d7bba-2706">Referencias a los miembros de tipos de enumeración.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2706">References to members of enumeration types.</span></span>
*  <span data-ttu-id="d7bba-2707">Las referencias a `const` parámetros o variables locales</span><span class="sxs-lookup"><span data-stu-id="d7bba-2707">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="d7bba-2708">Subexpresiones entre paréntesis, que son expresiones constantes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2708">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="d7bba-2709">Expresiones de conversión, siempre que el tipo de destino es uno de los tipos enumerados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2709">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="d7bba-2710">`checked` y `unchecked` expresiones</span><span class="sxs-lookup"><span data-stu-id="d7bba-2710">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="d7bba-2711">Expresiones de valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="d7bba-2711">Default value expressions</span></span>
*  <span data-ttu-id="d7bba-2712">Expresiones Nameof</span><span class="sxs-lookup"><span data-stu-id="d7bba-2712">Nameof expressions</span></span>
*  <span data-ttu-id="d7bba-2713">Predefinido `+`, `-`, `!`, y `~` operadores unarios.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2713">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="d7bba-2714">Predefinido `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, y `>=` proporcionado de operadores binarios, cada operando es de tipo enumerado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2714">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="d7bba-2715">El `?:` operador condicional.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2715">The `?:` conditional operator.</span></span>

<span data-ttu-id="d7bba-2716">Las conversiones siguientes se permiten en expresiones constantes:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2716">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="d7bba-2717">Conversiones de identidad</span><span class="sxs-lookup"><span data-stu-id="d7bba-2717">Identity conversions</span></span>
*  <span data-ttu-id="d7bba-2718">Conversiones numéricas</span><span class="sxs-lookup"><span data-stu-id="d7bba-2718">Numeric conversions</span></span>
*  <span data-ttu-id="d7bba-2719">Conversiones de enumeración</span><span class="sxs-lookup"><span data-stu-id="d7bba-2719">Enumeration conversions</span></span>
*  <span data-ttu-id="d7bba-2720">Conversiones de expresión constante</span><span class="sxs-lookup"><span data-stu-id="d7bba-2720">Constant expression conversions</span></span>
*  <span data-ttu-id="d7bba-2721">Conversiones implícitas y explícitas de referencia, siempre que el origen de las conversiones de una expresión constante que se evalúa como el valor null.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2721">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="d7bba-2722">Otras conversiones, incluidas la conversión boxing, unboxing e implícita las conversiones de referencia de valores distintos de null no se permiten en expresiones constantes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2722">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="d7bba-2723">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2723">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="d7bba-2724">la inicialización de i es un error porque se requiere una conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2724">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="d7bba-2725">La inicialización de str es un error porque se requiere una conversión implícita de referencia de un valor distinto de null.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2725">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="d7bba-2726">Cada vez que una expresión que cumple los requisitos descritos anteriormente, la expresión se evalúa en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2726">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="d7bba-2727">Esto es cierto incluso si la expresión es una subexpresión de una expresión mayor que contiene construcciones no constante.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2727">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="d7bba-2728">La evaluación de tiempo de compilación de expresiones constantes utiliza las mismas reglas como tiempo de ejecución de evaluación de expresiones no constantes, salvo que en evaluación en tiempo de ejecución se habría iniciado una excepción, la evaluación de tiempo de compilación produce un error de tiempo de compilación que se produzca.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2728">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="d7bba-2729">A menos que explícitamente se coloca una expresión constante en una `unchecked` contexto, los desbordamientos que se producen en las conversiones y operaciones aritméticas de tipo integral durante la evaluación de tiempo de compilación de la expresión siempre provocan errores de tiempo de compilación ([Expresiones constantes](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2729">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="d7bba-2730">Expresiones constantes que se producen en los contextos siguientes.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2730">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="d7bba-2731">En estos contextos, se produce un error de tiempo de compilación si una expresión no se puede evaluar por completo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2731">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="d7bba-2732">Las declaraciones de constantes ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2732">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="d7bba-2733">Las declaraciones de miembros de enumeración ([miembros de enumeración](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2733">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="d7bba-2734">Argumentos de las listas de parámetros formales predeterminados ([parámetros del método](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="d7bba-2734">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="d7bba-2735">`case` las etiquetas de un `switch` instrucción ([la instrucción switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2735">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="d7bba-2736">`goto case` instrucciones ([la instrucción goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2736">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="d7bba-2737">Longitudes de dimensión en una expresión de creación de matriz ([expresiones de creación de matriz](expressions.md#array-creation-expressions)) que incluye un inicializador.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2737">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="d7bba-2738">Atributos ([atributos](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="d7bba-2738">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="d7bba-2739">Una conversión implícita de expresión constante ([conversiones implícitas de expresión constante](conversions.md#implicit-constant-expression-conversions)) permite una expresión constante de tipo `int` va a convertir en `sbyte`, `byte`, `short`, `ushort`, `uint`, o `ulong`, siempre que el valor de la expresión constante esté dentro del intervalo del tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2739">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="d7bba-2740">expresiones booleanas</span><span class="sxs-lookup"><span data-stu-id="d7bba-2740">Boolean expressions</span></span>

<span data-ttu-id="d7bba-2741">Un *boolean_expression* es una expresión que da como resultado un tipo `bool`; ya sea directamente o a través de la aplicación de `operator true` en ciertos contextos, como se especifica en la siguiente.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2741">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="d7bba-2742">La expresión condicional de control de un *if_statement* ([if instrucción](statements.md#the-if-statement)), *while_statement* ([la instrucción while](statements.md#the-while-statement)), *do_statement* ([la instrucción do](statements.md#the-do-statement)), o *for_statement* ([la instrucción for](statements.md#the-for-statement)) es un *boolean_ expresión*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2742">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="d7bba-2743">La expresión condicional de control de la `?:` operador ([operador condicional](expressions.md#conditional-operator)) sigue las mismas reglas que un *boolean_expression*, pero por motivos de operador se clasifica prioridad como un *conditional_or_expression*.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2743">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="d7bba-2744">Un *boolean_expression* `E` debe ser capaz de generar un valor de tipo `bool`, como sigue:</span><span class="sxs-lookup"><span data-stu-id="d7bba-2744">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="d7bba-2745">Si `E` es implícitamente convertible a `bool` , a continuación, en tiempo de ejecución se aplica esa conversión implícita.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2745">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="d7bba-2746">En caso contrario, resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se utiliza para buscar una única mejor implementación del operador `true` en `E`, y que la implementación se aplica en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2746">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="d7bba-2747">Si no se encuentra ningún operador de este tipo, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2747">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="d7bba-2748">El `DBBool` tipo struct en [base de datos de tipo booleano](structs.md#database-boolean-type) proporciona un ejemplo de un tipo que implementa `operator true` y `operator false`.</span><span class="sxs-lookup"><span data-stu-id="d7bba-2748">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
