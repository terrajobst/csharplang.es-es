---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704088"
---
# <a name="expressions"></a><span data-ttu-id="212c8-101">Expresiones</span><span class="sxs-lookup"><span data-stu-id="212c8-101">Expressions</span></span>

<span data-ttu-id="212c8-102">Una expresión es una secuencia de operadores y operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-102">An expression is a sequence of operators and operands.</span></span> <span data-ttu-id="212c8-103">En este capítulo se define la sintaxis, el orden de evaluación de los operandos y operadores y el significado de las expresiones.</span><span class="sxs-lookup"><span data-stu-id="212c8-103">This chapter defines the syntax, order of evaluation of operands and operators, and meaning of expressions.</span></span>

## <a name="expression-classifications"></a><span data-ttu-id="212c8-104">Clasificaciones de expresiones</span><span class="sxs-lookup"><span data-stu-id="212c8-104">Expression classifications</span></span>

<span data-ttu-id="212c8-105">Una expresión se clasifica de las siguientes formas:</span><span class="sxs-lookup"><span data-stu-id="212c8-105">An expression is classified as one of the following:</span></span>

*  <span data-ttu-id="212c8-106">Un valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-106">A value.</span></span> <span data-ttu-id="212c8-107">Todos los valores tienen un tipo asociado.</span><span class="sxs-lookup"><span data-stu-id="212c8-107">Every value has an associated type.</span></span>
*  <span data-ttu-id="212c8-108">Una variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-108">A variable.</span></span> <span data-ttu-id="212c8-109">Cada variable tiene un tipo asociado, es decir, el tipo declarado de la variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-109">Every variable has an associated type, namely the declared type of the variable.</span></span>
*  <span data-ttu-id="212c8-110">Espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="212c8-110">A namespace.</span></span> <span data-ttu-id="212c8-111">Una expresión con esta clasificación solo puede aparecer como la parte izquierda de un *member_access* ([acceso a miembros](expressions.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="212c8-111">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)).</span></span> <span data-ttu-id="212c8-112">En cualquier otro contexto, una expresión clasificada como un espacio de nombres produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-112">In any other context, an expression classified as a namespace causes a compile-time error.</span></span>
*  <span data-ttu-id="212c8-113">Un tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-113">A type.</span></span> <span data-ttu-id="212c8-114">Una expresión con esta clasificación solo puede aparecer como el lado izquierdo de un *member_access* ([acceso a miembros](expressions.md#member-access)) o como un operando para el operador `as` ([el operador as](expressions.md#the-as-operator)), el operador `is` ([el operador is](expressions.md#the-is-operator)) o el @no operador __t-6 ([operador typeof](expressions.md#the-typeof-operator)).</span><span class="sxs-lookup"><span data-stu-id="212c8-114">An expression with this classification can only appear as the left hand side of a *member_access* ([Member access](expressions.md#member-access)), or as an operand for the `as` operator ([The as operator](expressions.md#the-as-operator)), the `is` operator ([The is operator](expressions.md#the-is-operator)), or the `typeof` operator ([The typeof operator](expressions.md#the-typeof-operator)).</span></span> <span data-ttu-id="212c8-115">En cualquier otro contexto, una expresión clasificada como un tipo genera un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-115">In any other context, an expression classified as a type causes a compile-time error.</span></span>
*  <span data-ttu-id="212c8-116">Un grupo de métodos, que es un conjunto de métodos sobrecargados que resultan de una búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="212c8-116">A method group, which is a set of overloaded methods resulting from a member lookup ([Member lookup](expressions.md#member-lookup)).</span></span> <span data-ttu-id="212c8-117">Un grupo de métodos puede tener una expresión de instancia asociada y una lista de argumentos de tipo asociada.</span><span class="sxs-lookup"><span data-stu-id="212c8-117">A method group may have an associated instance expression and an associated type argument list.</span></span> <span data-ttu-id="212c8-118">Cuando se invoca un método de instancia, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="212c8-118">When an instance method is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span> <span data-ttu-id="212c8-119">Se permite un grupo de métodos en un *invocation_expression* ([expresiones de invocación](expressions.md#invocation-expressions)), un *delegate_creation_expression* (expresiones de[creación de delegado](expressions.md#delegate-creation-expressions)) y como el lado izquierdo de un operador is y puede ser implícitamente se convierte en un tipo de delegado compatible ([conversiones de grupo de métodos](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-119">A method group is permitted in an *invocation_expression* ([Invocation expressions](expressions.md#invocation-expressions)) , a *delegate_creation_expression* ([Delegate creation expressions](expressions.md#delegate-creation-expressions)) and as the left hand side of an is operator, and can be implicitly converted to a compatible delegate type ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="212c8-120">En cualquier otro contexto, una expresión clasificada como grupo de métodos produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-120">In any other context, an expression classified as a method group causes a compile-time error.</span></span>
*  <span data-ttu-id="212c8-121">Un literal null.</span><span class="sxs-lookup"><span data-stu-id="212c8-121">A null literal.</span></span> <span data-ttu-id="212c8-122">Una expresión con esta clasificación se puede convertir implícitamente a un tipo de referencia o a un tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-122">An expression with this classification can be implicitly converted to a reference type or nullable type.</span></span>
*  <span data-ttu-id="212c8-123">Una función anónima.</span><span class="sxs-lookup"><span data-stu-id="212c8-123">An anonymous function.</span></span> <span data-ttu-id="212c8-124">Una expresión con esta clasificación se puede convertir implícitamente en un tipo de delegado compatible o un tipo de árbol de expresión.</span><span class="sxs-lookup"><span data-stu-id="212c8-124">An expression with this classification can be implicitly converted to a compatible delegate type or expression tree type.</span></span>
*  <span data-ttu-id="212c8-125">Un acceso de propiedad.</span><span class="sxs-lookup"><span data-stu-id="212c8-125">A property access.</span></span> <span data-ttu-id="212c8-126">Cada acceso de propiedad tiene un tipo asociado, es decir, el tipo de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="212c8-126">Every property access has an associated type, namely the type of the property.</span></span> <span data-ttu-id="212c8-127">Además, un acceso de propiedad puede tener una expresión de instancia asociada.</span><span class="sxs-lookup"><span data-stu-id="212c8-127">Furthermore, a property access may have an associated instance expression.</span></span> <span data-ttu-id="212c8-128">Cuando se invoca un descriptor de acceso (el bloque `get` o `set`) de una propiedad de instancia, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="212c8-128">When an accessor (the `get` or `set` block) of an instance property access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)).</span></span>
*  <span data-ttu-id="212c8-129">Un acceso de evento.</span><span class="sxs-lookup"><span data-stu-id="212c8-129">An event access.</span></span> <span data-ttu-id="212c8-130">Cada acceso a eventos tiene un tipo asociado, es decir, el tipo del evento.</span><span class="sxs-lookup"><span data-stu-id="212c8-130">Every event access has an associated type, namely the type of the event.</span></span> <span data-ttu-id="212c8-131">Además, un acceso a eventos puede tener una expresión de instancia asociada.</span><span class="sxs-lookup"><span data-stu-id="212c8-131">Furthermore, an event access may have an associated instance expression.</span></span> <span data-ttu-id="212c8-132">Un acceso a eventos puede aparecer como el operando izquierdo de los operadores `+=` y `-=` ([asignación de eventos](expressions.md#event-assignment)).</span><span class="sxs-lookup"><span data-stu-id="212c8-132">An event access may appear as the left hand operand of the `+=` and `-=` operators ([Event assignment](expressions.md#event-assignment)).</span></span> <span data-ttu-id="212c8-133">En cualquier otro contexto, una expresión clasificada como acceso de evento produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-133">In any other context, an expression classified as an event access causes a compile-time error.</span></span>
*  <span data-ttu-id="212c8-134">Un acceso de indexador.</span><span class="sxs-lookup"><span data-stu-id="212c8-134">An indexer access.</span></span> <span data-ttu-id="212c8-135">Cada acceso de indexador tiene un tipo asociado, es decir, el tipo de elemento del indexador.</span><span class="sxs-lookup"><span data-stu-id="212c8-135">Every indexer access has an associated type, namely the element type of the indexer.</span></span> <span data-ttu-id="212c8-136">Además, un acceso de indexador tiene una expresión de instancia asociada y una lista de argumentos asociada.</span><span class="sxs-lookup"><span data-stu-id="212c8-136">Furthermore, an indexer access has an associated instance expression and an associated argument list.</span></span> <span data-ttu-id="212c8-137">Cuando se invoca un descriptor de acceso (el bloque `get` o `set`) de un acceso de indexador, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)) y el resultado de la evaluación de la lista de argumentos se convierte en el lista de parámetros de la invocación.</span><span class="sxs-lookup"><span data-stu-id="212c8-137">When an accessor (the `get` or `set` block) of an indexer access is invoked, the result of evaluating the instance expression becomes the instance represented by `this` ([This access](expressions.md#this-access)), and the result of evaluating the argument list becomes the parameter list of the invocation.</span></span>
*  <span data-ttu-id="212c8-138">Relación.</span><span class="sxs-lookup"><span data-stu-id="212c8-138">Nothing.</span></span> <span data-ttu-id="212c8-139">Esto sucede cuando la expresión es una invocación de un método con un tipo de valor devuelto de `void`.</span><span class="sxs-lookup"><span data-stu-id="212c8-139">This occurs when the expression is an invocation of a method with a return type of `void`.</span></span> <span data-ttu-id="212c8-140">Una expresión clasificada como Nothing solo es válida en el contexto de *statement_expression* ([instrucciones de expresión](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="212c8-140">An expression classified as nothing is only valid in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

<span data-ttu-id="212c8-141">El resultado final de una expresión nunca es un espacio de nombres, tipo, grupo de métodos o acceso a eventos.</span><span class="sxs-lookup"><span data-stu-id="212c8-141">The final result of an expression is never a namespace, type, method group, or event access.</span></span> <span data-ttu-id="212c8-142">En su lugar, como se indicó anteriormente, estas categorías de expresiones son construcciones intermedias que solo se permiten en determinados contextos.</span><span class="sxs-lookup"><span data-stu-id="212c8-142">Rather, as noted above, these categories of expressions are intermediate constructs that are only permitted in certain contexts.</span></span>

<span data-ttu-id="212c8-143">Un acceso de propiedad o de indexador siempre se reclasifica como un valor realizando una invocación del *descriptor* de acceso get o del *descriptor*de acceso set.</span><span class="sxs-lookup"><span data-stu-id="212c8-143">A property access or indexer access is always reclassified as a value by performing an invocation of the *get accessor* or the *set accessor*.</span></span> <span data-ttu-id="212c8-144">El descriptor de acceso concreto viene determinado por el contexto de la propiedad o del indexador: Si el acceso es el destino de una asignación, se invoca al *descriptor* de acceso set para asignar un nuevo valor ([asignación simple](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="212c8-144">The particular accessor is determined by the context of the property or indexer access: If the access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="212c8-145">De lo contrario, el *descriptor de acceso get* se invoca para obtener el valor actual ([valores de las expresiones](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-145">Otherwise, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="values-of-expressions"></a><span data-ttu-id="212c8-146">Valores de las expresiones</span><span class="sxs-lookup"><span data-stu-id="212c8-146">Values of expressions</span></span>

<span data-ttu-id="212c8-147">En última instancia, la mayoría de las construcciones que implican una expresión requieren que la expresión denote un ***valor***.</span><span class="sxs-lookup"><span data-stu-id="212c8-147">Most of the constructs that involve an expression ultimately require the expression to denote a ***value***.</span></span> <span data-ttu-id="212c8-148">En tales casos, si la expresión real denota un espacio de nombres, un tipo, un grupo de métodos o nada, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-148">In such cases, if the actual expression denotes a namespace, a type, a method group, or nothing, a compile-time error occurs.</span></span> <span data-ttu-id="212c8-149">Sin embargo, si la expresión denota un acceso de propiedad, un acceso a indexador o una variable, el valor de la propiedad, el indexador o la variable se sustituyen implícitamente:</span><span class="sxs-lookup"><span data-stu-id="212c8-149">However, if the expression denotes a property access, an indexer access, or a variable, the value of the property, indexer, or variable is implicitly substituted:</span></span>

*  <span data-ttu-id="212c8-150">El valor de una variable es simplemente el valor almacenado actualmente en la ubicación de almacenamiento identificada por la variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-150">The value of a variable is simply the value currently stored in the storage location identified by the variable.</span></span> <span data-ttu-id="212c8-151">Una variable se debe considerar definitivamente asignada ([asignación definitiva](variables.md#definite-assignment)) antes de que se pueda obtener su valor o, de lo contrario, se producirá un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-151">A variable must be considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) before its value can be obtained, or otherwise a compile-time error occurs.</span></span>
*  <span data-ttu-id="212c8-152">El valor de una expresión de acceso de propiedad se obtiene invocando el *descriptor* de acceso get de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="212c8-152">The value of a property access expression is obtained by invoking the *get accessor* of the property.</span></span> <span data-ttu-id="212c8-153">Si la propiedad no tiene un *descriptor de acceso get*, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-153">If the property has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="212c8-154">De lo contrario, se realiza una invocación de miembro de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) y el resultado de la invocación se convierte en el valor de la expresión de acceso de propiedad.</span><span class="sxs-lookup"><span data-stu-id="212c8-154">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed, and the result of the invocation becomes the value of the property access expression.</span></span>
*  <span data-ttu-id="212c8-155">El valor de una expresión de acceso de indexador se obtiene al invocar el *descriptor* de acceso get del indexador.</span><span class="sxs-lookup"><span data-stu-id="212c8-155">The value of an indexer access expression is obtained by invoking the *get accessor* of the indexer.</span></span> <span data-ttu-id="212c8-156">Si el indizador no tiene ningún *descriptor de acceso get*, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-156">If the indexer has no *get accessor*, a compile-time error occurs.</span></span> <span data-ttu-id="212c8-157">De lo contrario, se realiza una invocación de miembro de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) con la lista de argumentos asociada a la expresión de acceso del indexador y el resultado de la invocación se convierte en el valor del acceso del indexador. Expresiones.</span><span class="sxs-lookup"><span data-stu-id="212c8-157">Otherwise, a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is performed with the argument list associated with the indexer access expression, and the result of the invocation becomes the value of the indexer access expression.</span></span>

## <a name="static-and-dynamic-binding"></a><span data-ttu-id="212c8-158">Enlace estático y dinámico</span><span class="sxs-lookup"><span data-stu-id="212c8-158">Static and Dynamic Binding</span></span>

<span data-ttu-id="212c8-159">El proceso de determinar el significado de una operación basándose en el tipo o valor de las expresiones constituyentes (argumentos, operandos, receptores) a menudo se conoce como ***enlace***.</span><span class="sxs-lookup"><span data-stu-id="212c8-159">The process of determining the meaning of an operation based on the type or value of constituent expressions (arguments, operands, receivers) is often referred to as ***binding***.</span></span> <span data-ttu-id="212c8-160">Por ejemplo, el significado de una llamada al método se determina en función del tipo del receptor y de los argumentos.</span><span class="sxs-lookup"><span data-stu-id="212c8-160">For instance the meaning of a method call is determined based on the type of the receiver and arguments.</span></span> <span data-ttu-id="212c8-161">El significado de un operador se determina en función del tipo de sus operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-161">The meaning of an operator is determined based on the type of its operands.</span></span>

<span data-ttu-id="212c8-162">En C# el significado de una operación se suele determinar en tiempo de compilación, en función del tipo en tiempo de compilación de sus expresiones constituyentes.</span><span class="sxs-lookup"><span data-stu-id="212c8-162">In C# the meaning of an operation is usually determined at compile-time, based on the compile-time type of its constituent expressions.</span></span> <span data-ttu-id="212c8-163">Del mismo modo, si una expresión contiene un error, el compilador detecta el error y lo emite.</span><span class="sxs-lookup"><span data-stu-id="212c8-163">Likewise, if an expression contains an error, the error is detected and reported by the compiler.</span></span> <span data-ttu-id="212c8-164">Este enfoque se conoce como ***enlace estático***.</span><span class="sxs-lookup"><span data-stu-id="212c8-164">This approach is known as ***static binding***.</span></span>

<span data-ttu-id="212c8-165">Sin embargo, si una expresión es una expresión dinámica (es decir, tiene el tipo `dynamic`), esto indica que cualquier enlace en el que participe debería basarse en su tipo en tiempo de ejecución (es decir, el tipo real del objeto que denota en tiempo de ejecución) en lugar de en el tipo en el que se encuentra. tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-165">However, if an expression is a dynamic expression (i.e. has the type `dynamic`) this indicates that any binding that it participates in should be based on its run-time type (i.e. the actual type of the object it denotes at run-time) rather than the type it has at compile-time.</span></span> <span data-ttu-id="212c8-166">Por consiguiente, el enlace de esta operación se aplaza hasta el momento en que se ejecuta la operación durante la ejecución del programa.</span><span class="sxs-lookup"><span data-stu-id="212c8-166">The binding of such an operation is therefore deferred until the time where the operation is to be executed during the running of the program.</span></span> <span data-ttu-id="212c8-167">Esto se conoce como ***enlace dinámico***.</span><span class="sxs-lookup"><span data-stu-id="212c8-167">This is referred to as ***dynamic binding***.</span></span>

<span data-ttu-id="212c8-168">Cuando una operación está enlazada dinámicamente, el compilador realiza poca o ninguna comprobación.</span><span class="sxs-lookup"><span data-stu-id="212c8-168">When an operation is dynamically bound, little or no checking is performed by the compiler.</span></span> <span data-ttu-id="212c8-169">En su lugar, si se produce un error en el enlace en tiempo de ejecución, los errores se registran como excepciones en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="212c8-169">Instead if the run-time binding fails, errors are reported as exceptions at run-time.</span></span>

<span data-ttu-id="212c8-170">Las siguientes operaciones en C# están sujetas al enlace:</span><span class="sxs-lookup"><span data-stu-id="212c8-170">The following operations in C# are subject to binding:</span></span>

*  <span data-ttu-id="212c8-171">Acceso a miembros: `e.M`</span><span class="sxs-lookup"><span data-stu-id="212c8-171">Member access: `e.M`</span></span>
*  <span data-ttu-id="212c8-172">Invocación de método: `e.M(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="212c8-172">Method invocation: `e.M(e1, ..., eN)`</span></span>
*  <span data-ttu-id="212c8-173">Invocación de delegado: `e(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="212c8-173">Delegate invocation:`e(e1, ..., eN)`</span></span>
*  <span data-ttu-id="212c8-174">Acceso a elementos: `e[e1, ..., eN]`</span><span class="sxs-lookup"><span data-stu-id="212c8-174">Element access: `e[e1, ..., eN]`</span></span>
*  <span data-ttu-id="212c8-175">Creación de objetos: `new C(e1, ..., eN)`</span><span class="sxs-lookup"><span data-stu-id="212c8-175">Object creation: `new C(e1, ..., eN)`</span></span>
*  <span data-ttu-id="212c8-176">Operadores unarios sobrecargados: `+`, `-`, @no__t 2, `~`, `++`, `--`, `true`, `false`</span><span class="sxs-lookup"><span data-stu-id="212c8-176">Overloaded unary operators: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`</span></span>
*  <span data-ttu-id="212c8-177">Operadores binarios sobrecargados: `+`, `-`, @no__t 2, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, 0, 1, 2, 3, 4, 5, 6, 7, 8</span><span class="sxs-lookup"><span data-stu-id="212c8-177">Overloaded binary operators: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`</span></span>
*  <span data-ttu-id="212c8-178">Operadores de asignación: `=`, `+=`, @no__t 2, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, 0</span><span class="sxs-lookup"><span data-stu-id="212c8-178">Assignment operators: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`</span></span>
*  <span data-ttu-id="212c8-179">Conversiones implícitas y explícitas</span><span class="sxs-lookup"><span data-stu-id="212c8-179">Implicit and explicit conversions</span></span>

<span data-ttu-id="212c8-180">Cuando no hay ninguna expresión dinámica implicada, C# el valor predeterminado es enlace estático, lo que significa que los tipos en tiempo de compilación de las expresiones constituyentes se usan en el proceso de selección.</span><span class="sxs-lookup"><span data-stu-id="212c8-180">When no dynamic expressions are involved, C# defaults to static binding, which means that the compile-time types of constituent expressions are used in the selection process.</span></span> <span data-ttu-id="212c8-181">Sin embargo, cuando una de las expresiones constituyentes de las operaciones enumeradas anteriormente es una expresión dinámica, la operación se enlaza dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-181">However, when one of the constituent expressions in the operations listed above is a dynamic expression, the operation is instead dynamically bound.</span></span>

### <a name="binding-time"></a><span data-ttu-id="212c8-182">Tiempo de enlace</span><span class="sxs-lookup"><span data-stu-id="212c8-182">Binding-time</span></span>

<span data-ttu-id="212c8-183">El enlace estático se realiza en tiempo de compilación, mientras que el enlace dinámico tiene lugar en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="212c8-183">Static binding takes place at compile-time, whereas dynamic binding takes place at run-time.</span></span> <span data-ttu-id="212c8-184">En las secciones siguientes, el término ***tiempo de enlace*** hace referencia a un tiempo de compilación o a un tiempo de ejecución, en función de cuándo tenga lugar el enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-184">In the following sections, the term ***binding-time*** refers to either compile-time or run-time, depending on when the binding takes place.</span></span>

<span data-ttu-id="212c8-185">En el ejemplo siguiente se muestran las nociones de enlace estático y dinámico y del tiempo de enlace:</span><span class="sxs-lookup"><span data-stu-id="212c8-185">The following example illustrates the notions of static and dynamic binding and of binding-time:</span></span>
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

<span data-ttu-id="212c8-186">Las dos primeras llamadas se enlazan estáticamente: la sobrecarga de `Console.WriteLine` se selecciona en función del tipo en tiempo de compilación de su argumento.</span><span class="sxs-lookup"><span data-stu-id="212c8-186">The first two calls are statically bound: the overload of `Console.WriteLine` is picked based on the compile-time type of their argument.</span></span> <span data-ttu-id="212c8-187">Por lo tanto, el tiempo de enlace es el tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-187">Thus, the binding-time is compile-time.</span></span>

<span data-ttu-id="212c8-188">La tercera llamada está enlazada dinámicamente: la sobrecarga de `Console.WriteLine` se selecciona en función del tipo en tiempo de ejecución de su argumento.</span><span class="sxs-lookup"><span data-stu-id="212c8-188">The third call is dynamically bound: the overload of `Console.WriteLine` is picked based on the run-time type of its argument.</span></span> <span data-ttu-id="212c8-189">Esto sucede porque el argumento es una expresión dinámica (su tipo en tiempo de compilación es `dynamic`).</span><span class="sxs-lookup"><span data-stu-id="212c8-189">This happens because the argument is a dynamic expression -- its compile-time type is `dynamic`.</span></span> <span data-ttu-id="212c8-190">Por lo tanto, el tiempo de enlace para la tercera llamada es en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="212c8-190">Thus, the binding-time for the third call is run-time.</span></span>

### <a name="dynamic-binding"></a><span data-ttu-id="212c8-191">Enlace dinámico</span><span class="sxs-lookup"><span data-stu-id="212c8-191">Dynamic binding</span></span>

<span data-ttu-id="212c8-192">El propósito de los enlaces dinámicos es C# permitir que los programas interactúen con ***objetos dinámicos***, es decir, los objetos que no C# siguen las reglas normales del sistema de tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-192">The purpose of dynamic binding is to allow C# programs to interact with ***dynamic objects***, i.e. objects that do not follow the normal rules of the C# type system.</span></span> <span data-ttu-id="212c8-193">Los objetos dinámicos pueden ser objetos de otros lenguajes de programación con distintos sistemas de tipos, o bien pueden ser objetos que se configuran mediante programación para implementar su propia semántica de enlace para diferentes operaciones.</span><span class="sxs-lookup"><span data-stu-id="212c8-193">Dynamic objects may be objects from other programming languages with different types systems, or they may be objects that are programmatically setup to implement their own binding semantics for different operations.</span></span>

<span data-ttu-id="212c8-194">El mecanismo por el que un objeto dinámico implementa su propia semántica es la implementación definida.</span><span class="sxs-lookup"><span data-stu-id="212c8-194">The mechanism by which a dynamic object implements its own semantics is implementation defined.</span></span> <span data-ttu-id="212c8-195">Una interfaz determinada, que se ha definido de nuevo en la C# implementación, se implementa mediante objetos dinámicos para indicar al tiempo de ejecución que tienen una semántica especial.</span><span class="sxs-lookup"><span data-stu-id="212c8-195">A given interface -- again implementation defined -- is implemented by dynamic objects to signal to the C# run-time that they have special semantics.</span></span> <span data-ttu-id="212c8-196">Por lo tanto, cada vez que las operaciones en un objeto dinámico se enlazan dinámicamente, su propia semántica de C# enlace, en lugar de las de tal como se especifica en este documento, asumen el control.</span><span class="sxs-lookup"><span data-stu-id="212c8-196">Thus, whenever operations on a dynamic object are dynamically bound, their own binding semantics, rather than those of C# as specified in this document, take over.</span></span>

<span data-ttu-id="212c8-197">Aunque el propósito del enlace dinámico es permitir la interoperación con objetos dinámicos C# , permite el enlace dinámico en todos los objetos, tanto si son dinámicos como si no.</span><span class="sxs-lookup"><span data-stu-id="212c8-197">While the purpose of dynamic binding is to allow interoperation with dynamic objects, C# allows dynamic binding on all objects, whether they are dynamic or not.</span></span> <span data-ttu-id="212c8-198">Esto permite una integración más fluida de los objetos dinámicos, ya que los resultados de las operaciones en ellos pueden no ser objetos dinámicos, pero siguen siendo un tipo desconocido para el programador en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-198">This allows for a smoother integration of dynamic objects, as the results of operations on them may not themselves be dynamic objects, but are still of a type unknown to the programmer at compile-time.</span></span> <span data-ttu-id="212c8-199">Además, el enlace dinámico puede ayudar a eliminar el código basado en la reflexión propenso a errores, incluso cuando ningún objeto implicado sea de objetos dinámicos.</span><span class="sxs-lookup"><span data-stu-id="212c8-199">Also dynamic binding can help eliminate error-prone reflection-based code even when no objects involved are dynamic objects.</span></span>

<span data-ttu-id="212c8-200">En las secciones siguientes se describe para cada construcción del lenguaje exactamente cuando se aplica el enlace dinámico, qué comprobación del tiempo de compilación se aplica, en caso de que se aplique, y cuál es el resultado en tiempo de compilación y la clasificación de la expresión.</span><span class="sxs-lookup"><span data-stu-id="212c8-200">The following sections describe for each construct in the language exactly when dynamic binding is applied, what compile time checking -- if any -- is applied, and what the compile-time result and expression classification is.</span></span>

### <a name="types-of-constituent-expressions"></a><span data-ttu-id="212c8-201">Tipos de expresiones constituyentes</span><span class="sxs-lookup"><span data-stu-id="212c8-201">Types of constituent expressions</span></span>

<span data-ttu-id="212c8-202">Cuando una operación se enlaza estáticamente, el tipo de una expresión constituyente (por ejemplo, un receptor, un argumento, un índice o un operando) siempre se considera el tipo en tiempo de compilación de esa expresión.</span><span class="sxs-lookup"><span data-stu-id="212c8-202">When an operation is statically bound, the type of a constituent expression (e.g. a receiver, an argument, an index or an operand) is always considered to be the compile-time type of that expression.</span></span>

<span data-ttu-id="212c8-203">Cuando una operación se enlaza de forma dinámica, el tipo de una expresión constituyente se determina de maneras diferentes en función del tipo en tiempo de compilación de la expresión constituyente:</span><span class="sxs-lookup"><span data-stu-id="212c8-203">When an operation is dynamically bound, the type of a constituent expression is determined in different ways depending on the compile-time type of the constituent expression:</span></span>

*  <span data-ttu-id="212c8-204">Se considera que una expresión constitutiva del tipo en tiempo de compilación `dynamic` tiene el tipo del valor real al que se evalúa la expresión en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="212c8-204">A constituent expression of compile-time type `dynamic` is considered to have the type of the actual value that the expression evaluates to at runtime</span></span>
*  <span data-ttu-id="212c8-205">Expresión constituyente cuyo tipo en tiempo de compilación es un parámetro de tipo se considera que tiene el tipo al que está enlazado el parámetro de tipo en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="212c8-205">A constituent expression whose compile-time type is a type parameter is considered to have the type which the type parameter is bound to at runtime</span></span>
*  <span data-ttu-id="212c8-206">De lo contrario, se considera que la expresión Constituyente tiene su tipo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-206">Otherwise the constituent expression is considered to have its compile-time type.</span></span>

## <a name="operators"></a><span data-ttu-id="212c8-207">Operadores</span><span class="sxs-lookup"><span data-stu-id="212c8-207">Operators</span></span>

<span data-ttu-id="212c8-208">Las expresiones se construyen a partir de ***operandos*** y ***operadores***.</span><span class="sxs-lookup"><span data-stu-id="212c8-208">Expressions are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="212c8-209">Los operadores de una expresión indican qué operaciones se aplican a los operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-209">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="212c8-210">Ejemplos de operadores incluyen `+`, `-`, `*`, `/` y `new`.</span><span class="sxs-lookup"><span data-stu-id="212c8-210">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="212c8-211">Algunos ejemplos de operandos son literales, campos, variables locales y expresiones.</span><span class="sxs-lookup"><span data-stu-id="212c8-211">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="212c8-212">Hay tres tipos de operadores:</span><span class="sxs-lookup"><span data-stu-id="212c8-212">There are three kinds of operators:</span></span>

*  <span data-ttu-id="212c8-213">Operadores unarios.</span><span class="sxs-lookup"><span data-stu-id="212c8-213">Unary operators.</span></span> <span data-ttu-id="212c8-214">Los operadores unarios toman un operando y usan la notación de prefijo (como `--x`) o la notación de postfijo (como `x++`).</span><span class="sxs-lookup"><span data-stu-id="212c8-214">The unary operators take one operand and use either prefix notation (such as `--x`) or postfix notation (such as `x++`).</span></span>
*  <span data-ttu-id="212c8-215">Operadores binarios.</span><span class="sxs-lookup"><span data-stu-id="212c8-215">Binary operators.</span></span> <span data-ttu-id="212c8-216">Los operadores binarios toman dos operandos y todos usan notación infija (como `x + y`).</span><span class="sxs-lookup"><span data-stu-id="212c8-216">The binary operators take two operands and all use infix notation (such as `x + y`).</span></span>
*  <span data-ttu-id="212c8-217">Operador ternario.</span><span class="sxs-lookup"><span data-stu-id="212c8-217">Ternary operator.</span></span> <span data-ttu-id="212c8-218">Solo existe un operador ternario, `?:`; toma tres operandos y usa la notación de infijo (`c ? x : y`).</span><span class="sxs-lookup"><span data-stu-id="212c8-218">Only one ternary operator, `?:`, exists; it takes three operands and uses infix notation (`c ? x : y`).</span></span>

<span data-ttu-id="212c8-219">El orden de evaluación de los operadores de una expresión viene determinado por la ***prioridad*** y la ***asociatividad*** de los operadores ([precedencia y asociatividad](expressions.md#operator-precedence-and-associativity)de los operadores).</span><span class="sxs-lookup"><span data-stu-id="212c8-219">The order of evaluation of operators in an expression is determined by the ***precedence*** and ***associativity*** of the operators ([Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)).</span></span>

<span data-ttu-id="212c8-220">Los operandos de una expresión se evalúan de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="212c8-220">Operands in an expression are evaluated from left to right.</span></span> <span data-ttu-id="212c8-221">Por ejemplo, en `F(i) + G(i++) * H(i)`, se llama al método `F` utilizando el valor anterior de `i`; a continuación, se llama al método `G` con el valor anterior de `i`, y, por último, se llama al método `H` con el nuevo valor de `i`.</span><span class="sxs-lookup"><span data-stu-id="212c8-221">For example, in `F(i) + G(i++) * H(i)`, method `F` is called using the old value of `i`, then method `G` is called with the old value of `i`, and, finally, method `H` is called with the new value of `i`.</span></span> <span data-ttu-id="212c8-222">Esto es independiente de la prioridad de operador y no está relacionada con ella.</span><span class="sxs-lookup"><span data-stu-id="212c8-222">This is separate from and unrelated to operator precedence.</span></span>

<span data-ttu-id="212c8-223">Algunos operadores se pueden ***sobrecargar***.</span><span class="sxs-lookup"><span data-stu-id="212c8-223">Certain operators can be ***overloaded***.</span></span> <span data-ttu-id="212c8-224">La sobrecarga de operadores permite especificar implementaciones de operador definidas por el usuario para las operaciones donde uno o ambos operandos son de una clase definida por el usuario o un tipo de estructura ([sobrecarga de operadores](expressions.md#operator-overloading)).</span><span class="sxs-lookup"><span data-stu-id="212c8-224">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type ([Operator overloading](expressions.md#operator-overloading)).</span></span>

### <a name="operator-precedence-and-associativity"></a><span data-ttu-id="212c8-225">Prioridad y asociatividad de los operadores</span><span class="sxs-lookup"><span data-stu-id="212c8-225">Operator precedence and associativity</span></span>

<span data-ttu-id="212c8-226">Cuando una expresión contiene varios operadores, la ***precedencia*** de los operadores controla el orden en que se evalúan los operadores individuales.</span><span class="sxs-lookup"><span data-stu-id="212c8-226">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="212c8-227">Por ejemplo, la expresión `x + y * z` se evalúa como `x + (y * z)` porque el operador `*` tiene mayor precedencia que el operador `+` binario.</span><span class="sxs-lookup"><span data-stu-id="212c8-227">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the binary `+` operator.</span></span> <span data-ttu-id="212c8-228">La prioridad de un operador se establece mediante la definición de su producción de gramática asociada.</span><span class="sxs-lookup"><span data-stu-id="212c8-228">The precedence of an operator is established by the definition of its associated grammar production.</span></span> <span data-ttu-id="212c8-229">Por ejemplo, un *additive_expression* consta de una secuencia de *multiplicative_expression*s separada por los operadores @no__t 2 o `-`, lo que permite a los operadores @no__t 4 y `-` una menor prioridad que los `*`, `/` y @no__ operadores t-8.</span><span class="sxs-lookup"><span data-stu-id="212c8-229">For example, an *additive_expression* consists of a sequence of *multiplicative_expression*s separated by `+` or `-` operators, thus giving the `+` and `-` operators lower precedence than the `*`, `/`, and `%` operators.</span></span>

<span data-ttu-id="212c8-230">En la tabla siguiente se resumen todos los operadores en orden de prioridad, de mayor a menor:</span><span class="sxs-lookup"><span data-stu-id="212c8-230">The following table summarizes all operators in order of precedence from highest to lowest:</span></span>

| <span data-ttu-id="212c8-231">__Sección__</span><span class="sxs-lookup"><span data-stu-id="212c8-231">__Section__</span></span>                                                                                   | <span data-ttu-id="212c8-232">__Categoría__</span><span class="sxs-lookup"><span data-stu-id="212c8-232">__Category__</span></span>                | <span data-ttu-id="212c8-233">__Operadores__</span><span class="sxs-lookup"><span data-stu-id="212c8-233">__Operators__</span></span> | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [<span data-ttu-id="212c8-234">Expresiones primarias</span><span class="sxs-lookup"><span data-stu-id="212c8-234">Primary expressions</span></span>](expressions.md#primary-expressions)                                     | <span data-ttu-id="212c8-235">Principal</span><span class="sxs-lookup"><span data-stu-id="212c8-235">Primary</span></span>                     | <span data-ttu-id="212c8-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span><span class="sxs-lookup"><span data-stu-id="212c8-236">`x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate`</span></span> | 
| [<span data-ttu-id="212c8-237">Operadores unarios</span><span class="sxs-lookup"><span data-stu-id="212c8-237">Unary operators</span></span>](expressions.md#unary-operators)                                             | <span data-ttu-id="212c8-238">Unario</span><span class="sxs-lookup"><span data-stu-id="212c8-238">Unary</span></span>                       | <span data-ttu-id="212c8-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span><span class="sxs-lookup"><span data-stu-id="212c8-239">`+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x`</span></span> | 
| [<span data-ttu-id="212c8-240">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="212c8-240">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="212c8-241">Multiplicativo</span><span class="sxs-lookup"><span data-stu-id="212c8-241">Multiplicative</span></span>              | <span data-ttu-id="212c8-242">`*`  `/`  `%`</span><span class="sxs-lookup"><span data-stu-id="212c8-242">`*`  `/`  `%`</span></span> | 
| [<span data-ttu-id="212c8-243">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="212c8-243">Arithmetic operators</span></span>](expressions.md#arithmetic-operators)                                   | <span data-ttu-id="212c8-244">Aditivo</span><span class="sxs-lookup"><span data-stu-id="212c8-244">Additive</span></span>                    | <span data-ttu-id="212c8-245">`+`  `-`</span><span class="sxs-lookup"><span data-stu-id="212c8-245">`+`  `-`</span></span>      | 
| [<span data-ttu-id="212c8-246">Operadores de desplazamiento</span><span class="sxs-lookup"><span data-stu-id="212c8-246">Shift operators</span></span>](expressions.md#shift-operators)                                             | <span data-ttu-id="212c8-247">Shift</span><span class="sxs-lookup"><span data-stu-id="212c8-247">Shift</span></span>                       | <span data-ttu-id="212c8-248">`<<`  `>>`</span><span class="sxs-lookup"><span data-stu-id="212c8-248">`<<`  `>>`</span></span>    | 
| [<span data-ttu-id="212c8-249">Operadores de comprobación de tipos y relacionales</span><span class="sxs-lookup"><span data-stu-id="212c8-249">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="212c8-250">Comprobación de tipos y relacional</span><span class="sxs-lookup"><span data-stu-id="212c8-250">Relational and type testing</span></span> | <span data-ttu-id="212c8-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span><span class="sxs-lookup"><span data-stu-id="212c8-251">`<`  `>`  `<=`  `>=`  `is`  `as`</span></span> | 
| [<span data-ttu-id="212c8-252">Operadores de comprobación de tipos y relacionales</span><span class="sxs-lookup"><span data-stu-id="212c8-252">Relational and type-testing operators</span></span>](expressions.md#relational-and-type-testing-operators) | <span data-ttu-id="212c8-253">Igualdad</span><span class="sxs-lookup"><span data-stu-id="212c8-253">Equality</span></span>                    | <span data-ttu-id="212c8-254">`==`  `!=`</span><span class="sxs-lookup"><span data-stu-id="212c8-254">`==`  `!=`</span></span>    | 
| [<span data-ttu-id="212c8-255">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="212c8-255">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="212c8-256">AND lógico</span><span class="sxs-lookup"><span data-stu-id="212c8-256">Logical AND</span></span>                 | `&`           | 
| [<span data-ttu-id="212c8-257">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="212c8-257">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="212c8-258">XOR lógico</span><span class="sxs-lookup"><span data-stu-id="212c8-258">Logical XOR</span></span>                 | `^`           | 
| [<span data-ttu-id="212c8-259">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="212c8-259">Logical operators</span></span>](expressions.md#logical-operators)                                         | <span data-ttu-id="212c8-260">OR lógico</span><span class="sxs-lookup"><span data-stu-id="212c8-260">Logical OR</span></span>                  | <code>&#124;</code>           |
| [<span data-ttu-id="212c8-261">Operadores lógicos condicionales</span><span class="sxs-lookup"><span data-stu-id="212c8-261">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="212c8-262">AND condicional</span><span class="sxs-lookup"><span data-stu-id="212c8-262">Conditional AND</span></span>             | `&&`          | 
| [<span data-ttu-id="212c8-263">Operadores lógicos condicionales</span><span class="sxs-lookup"><span data-stu-id="212c8-263">Conditional logical operators</span></span>](expressions.md#conditional-logical-operators)                 | <span data-ttu-id="212c8-264">OR condicional</span><span class="sxs-lookup"><span data-stu-id="212c8-264">Conditional OR</span></span>              | <code>&#124;&#124;</code>          | 
| [<span data-ttu-id="212c8-265">Operador de uso combinado de NULL</span><span class="sxs-lookup"><span data-stu-id="212c8-265">The null coalescing operator</span></span>](expressions.md#the-null-coalescing-operator)                   | <span data-ttu-id="212c8-266">Uso combinado de NULL</span><span class="sxs-lookup"><span data-stu-id="212c8-266">Null coalescing</span></span>             | `??`          | 
| [<span data-ttu-id="212c8-267">Operador condicional</span><span class="sxs-lookup"><span data-stu-id="212c8-267">Conditional operator</span></span>](expressions.md#conditional-operator)                                   | <span data-ttu-id="212c8-268">Condicional</span><span class="sxs-lookup"><span data-stu-id="212c8-268">Conditional</span></span>                 | `?:`          | 
| <span data-ttu-id="212c8-269">[Operadores de asignación](expressions.md#assignment-operators), [expresiones de función anónimas](expressions.md#anonymous-function-expressions)</span><span class="sxs-lookup"><span data-stu-id="212c8-269">[Assignment operators](expressions.md#assignment-operators), [Anonymous function expressions](expressions.md#anonymous-function-expressions)</span></span>  | <span data-ttu-id="212c8-270">Asignación y expresión lambda</span><span class="sxs-lookup"><span data-stu-id="212c8-270">Assignment and lambda expression</span></span> | <span data-ttu-id="212c8-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span><span class="sxs-lookup"><span data-stu-id="212c8-271">`=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>`</span></span> | 

<span data-ttu-id="212c8-272">Cuando se produce un operando entre dos operadores con la misma prioridad, la asociatividad de los operadores controla el orden en el que se realizan las operaciones:</span><span class="sxs-lookup"><span data-stu-id="212c8-272">When an operand occurs between two operators with the same precedence, the associativity of the operators controls the order in which the operations are performed:</span></span>

*  <span data-ttu-id="212c8-273">Excepto en el caso de los operadores de asignación y el operador de uso combinado de NULL, todos los operadores binarios son ***asociativos***a la izquierda, lo que significa que las operaciones se realizan de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="212c8-273">Except for the assignment operators and the null coalescing operator, all binary operators are ***left-associative***, meaning that operations are performed from left to right.</span></span> <span data-ttu-id="212c8-274">Por ejemplo, `x + y + z` se evalúa como `(x + y) + z`.</span><span class="sxs-lookup"><span data-stu-id="212c8-274">For example, `x + y + z` is evaluated as `(x + y) + z`.</span></span>
*  <span data-ttu-id="212c8-275">Los operadores de asignación, el operador de uso combinado de NULL y el operador condicional (`?:`) son ***asociativos a la derecha***, lo que significa que las operaciones se realizan de derecha a izquierda.</span><span class="sxs-lookup"><span data-stu-id="212c8-275">The assignment operators, the null coalescing operator and the conditional operator (`?:`) are ***right-associative***, meaning that operations are performed from right to left.</span></span> <span data-ttu-id="212c8-276">Por ejemplo, `x = y = z` se evalúa como `x = (y = z)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-276">For example, `x = y = z` is evaluated as `x = (y = z)`.</span></span>

<span data-ttu-id="212c8-277">La precedencia y la asociatividad pueden controlarse mediante paréntesis.</span><span class="sxs-lookup"><span data-stu-id="212c8-277">Precedence and associativity can be controlled using parentheses.</span></span> <span data-ttu-id="212c8-278">Por ejemplo, `x + y * z` primero multiplica `y` por `z` y luego suma el resultado a `x`, pero `(x + y) * z` primero suma `x` y `y` y luego multiplica el resultado por `z`.</span><span class="sxs-lookup"><span data-stu-id="212c8-278">For example, `x + y * z` first multiplies `y` by `z` and then adds the result to `x`, but `(x + y) * z` first adds `x` and `y` and then multiplies the result by `z`.</span></span>

### <a name="operator-overloading"></a><span data-ttu-id="212c8-279">Sobrecarga de operadores</span><span class="sxs-lookup"><span data-stu-id="212c8-279">Operator overloading</span></span>

<span data-ttu-id="212c8-280">Todos los operadores unarios y binarios tienen implementaciones predefinidas que están disponibles automáticamente en cualquier expresión.</span><span class="sxs-lookup"><span data-stu-id="212c8-280">All unary and binary operators have predefined implementations that are automatically available in any expression.</span></span> <span data-ttu-id="212c8-281">Además de las implementaciones predefinidas, las implementaciones definidas por el usuario se pueden introducir incluyendo declaraciones `operator` en clases y Structs ([operadores](classes.md#operators)).</span><span class="sxs-lookup"><span data-stu-id="212c8-281">In addition to the predefined implementations, user-defined implementations can be introduced by including `operator` declarations in classes and structs ([Operators](classes.md#operators)).</span></span> <span data-ttu-id="212c8-282">Las implementaciones de operador definidas por el usuario siempre tienen prioridad sobre las implementaciones de operadores predefinidas: Solo cuando no existan implementaciones de operadores definidos por el usuario aplicables, se tendrán en cuenta las implementaciones de operadores predefinidas, como se describe en resolución de sobrecargas de operador [unario](expressions.md#unary-operator-overload-resolution) y [resolución de sobrecarga de operadores binarios](expressions.md#binary-operator-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="212c8-282">User-defined operator implementations always take precedence over predefined operator implementations: Only when no applicable user-defined operator implementations exist will the predefined operator implementations be considered, as described in [Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution).</span></span>

<span data-ttu-id="212c8-283">Los ***operadores unarios sobrecargables*** son:</span><span class="sxs-lookup"><span data-stu-id="212c8-283">The ***overloadable unary operators*** are:</span></span>
```csharp
+   -   !   ~   ++   --   true   false
```

<span data-ttu-id="212c8-284">Aunque `true` y `false` no se usan explícitamente en expresiones (y, por consiguiente, no se incluyen en la tabla de precedencia en la [precedencia y asociatividad](expressions.md#operator-precedence-and-associativity)de los operadores), se consideran operadores porque se invocan en varias expresiones. contextos: Expresiones booleanas ([Expresiones booleanas](expressions.md#boolean-expressions)) y expresiones que implican al operador condicional ([operador condicional](expressions.md#conditional-operator)) y operadores lógicos condicionales ([operadores lógicos condicionales](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="212c8-284">Although `true` and `false` are not used explicitly in expressions (and therefore are not included in the precedence table in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity)), they are considered operators because they are invoked in several expression contexts: boolean expressions ([Boolean expressions](expressions.md#boolean-expressions)) and expressions involving the conditional ([Conditional operator](expressions.md#conditional-operator)), and conditional logical operators ([Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span>

<span data-ttu-id="212c8-285">Los ***operadores binarios sobrecargables*** son:</span><span class="sxs-lookup"><span data-stu-id="212c8-285">The ***overloadable binary operators*** are:</span></span>
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

<span data-ttu-id="212c8-286">Solo se pueden sobrecargar los operadores enumerados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="212c8-286">Only the operators listed above can be overloaded.</span></span> <span data-ttu-id="212c8-287">En concreto, no es posible sobrecargar el acceso a miembros, la invocación de métodos o los `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, 0, 1 y 2.</span><span class="sxs-lookup"><span data-stu-id="212c8-287">In particular, it is not possible to overload member access, method invocation, or the `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, and `is` operators.</span></span>

<span data-ttu-id="212c8-288">Cuando se sobrecarga un operador binario, el operador de asignación correspondiente, si lo hay, también se sobrecarga de modo implícito.</span><span class="sxs-lookup"><span data-stu-id="212c8-288">When a binary operator is overloaded, the corresponding assignment operator, if any, is also implicitly overloaded.</span></span> <span data-ttu-id="212c8-289">Por ejemplo, una sobrecarga de Operator `*` también es una sobrecarga de Operator `*=`.</span><span class="sxs-lookup"><span data-stu-id="212c8-289">For example, an overload of operator `*` is also an overload of operator `*=`.</span></span> <span data-ttu-id="212c8-290">Esto se describe con más detalle en [asignación compuesta](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="212c8-290">This is described further in [Compound assignment](expressions.md#compound-assignment).</span></span> <span data-ttu-id="212c8-291">Tenga en cuenta que el operador de asignación (`=`) no se puede sobrecargar.</span><span class="sxs-lookup"><span data-stu-id="212c8-291">Note that the assignment operator itself (`=`) cannot be overloaded.</span></span> <span data-ttu-id="212c8-292">Una asignación siempre realiza una copia simple de bits de un valor en una variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-292">An assignment always performs a simple bit-wise copy of a value into a variable.</span></span>

<span data-ttu-id="212c8-293">Las operaciones de conversión, como `(T)x`, se sobrecargan proporcionando conversiones definidas por el usuario ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-293">Cast operations, such as `(T)x`, are overloaded by providing user-defined conversions ([User-defined conversions](conversions.md#user-defined-conversions)).</span></span>

<span data-ttu-id="212c8-294">El acceso a los elementos, como `a[x]`, no se considera un operador sobrecargable.</span><span class="sxs-lookup"><span data-stu-id="212c8-294">Element access, such as `a[x]`, is not considered an overloadable operator.</span></span> <span data-ttu-id="212c8-295">En su lugar, se admite la indexación definida por el usuario a través de indexadores ([indizadores](classes.md#indexers)).</span><span class="sxs-lookup"><span data-stu-id="212c8-295">Instead, user-defined indexing is supported through indexers ([Indexers](classes.md#indexers)).</span></span>

<span data-ttu-id="212c8-296">En las expresiones, se hace referencia a los operadores mediante la notación de operador y, en las declaraciones, se hace referencia a los operadores mediante la notación funcional.</span><span class="sxs-lookup"><span data-stu-id="212c8-296">In expressions, operators are referenced using operator notation, and in declarations, operators are referenced using functional notation.</span></span> <span data-ttu-id="212c8-297">En la tabla siguiente se muestra la relación entre el operador y las notaciones funcionales para los operadores unarios y binarios.</span><span class="sxs-lookup"><span data-stu-id="212c8-297">The following table shows the relationship between operator and functional notations for unary and binary operators.</span></span> <span data-ttu-id="212c8-298">En la primera entrada, *OP* denota cualquier operador de prefijo unario sobrecargable.</span><span class="sxs-lookup"><span data-stu-id="212c8-298">In the first entry, *op* denotes any overloadable unary prefix operator.</span></span> <span data-ttu-id="212c8-299">En la segunda entrada, *OP* denota los operadores unarios `++` y `--`.</span><span class="sxs-lookup"><span data-stu-id="212c8-299">In the second entry, *op* denotes the unary postfix `++` and `--` operators.</span></span> <span data-ttu-id="212c8-300">En la tercera entrada, *OP* denota cualquier operador binario sobrecargable.</span><span class="sxs-lookup"><span data-stu-id="212c8-300">In the third entry, *op* denotes any overloadable binary operator.</span></span>


| <span data-ttu-id="212c8-301">__Notación de operador__</span><span class="sxs-lookup"><span data-stu-id="212c8-301">__Operator notation__</span></span> | <span data-ttu-id="212c8-302">__Notación funcional__</span><span class="sxs-lookup"><span data-stu-id="212c8-302">__Functional notation__</span></span> |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

<span data-ttu-id="212c8-303">Las declaraciones de operador definidas por el usuario siempre requieren que al menos uno de los parámetros sea del tipo de clase o estructura que contiene la declaración de operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-303">User-defined operator declarations always require at least one of the parameters to be of the class or struct type that contains the operator declaration.</span></span> <span data-ttu-id="212c8-304">Por lo tanto, no es posible que un operador definido por el usuario tenga la misma firma que un operador predefinido.</span><span class="sxs-lookup"><span data-stu-id="212c8-304">Thus, it is not possible for a user-defined operator to have the same signature as a predefined operator.</span></span>

<span data-ttu-id="212c8-305">Las declaraciones de operador definidas por el usuario no pueden modificar la sintaxis, la precedencia o la asociatividad de un operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-305">User-defined operator declarations cannot modify the syntax, precedence, or associativity of an operator.</span></span> <span data-ttu-id="212c8-306">Por ejemplo, el operador `/` siempre es un operador binario, siempre tiene el nivel de prioridad especificado en [precedencia y asociatividad](expressions.md#operator-precedence-and-associativity)de los operadores y siempre es asociativo a la izquierda.</span><span class="sxs-lookup"><span data-stu-id="212c8-306">For example, the `/` operator is always a binary operator, always has the precedence level specified in [Operator precedence and associativity](expressions.md#operator-precedence-and-associativity), and is always left-associative.</span></span>

<span data-ttu-id="212c8-307">Aunque es posible que un operador definido por el usuario realice cualquier cálculo, se desaconsejan las implementaciones que generan resultados distintos de los que se esperaban de forma intuitiva.</span><span class="sxs-lookup"><span data-stu-id="212c8-307">While it is possible for a user-defined operator to perform any computation it pleases, implementations that produce results other than those that are intuitively expected are strongly discouraged.</span></span> <span data-ttu-id="212c8-308">Por ejemplo, una implementación de `operator ==` debe comparar los dos operandos para determinar si son iguales y devolver un resultado `bool` adecuado.</span><span class="sxs-lookup"><span data-stu-id="212c8-308">For example, an implementation of `operator ==` should compare the two operands for equality and return an appropriate `bool` result.</span></span>

<span data-ttu-id="212c8-309">Las descripciones de operadores individuales en [expresiones primarias](expressions.md#primary-expressions) a través de [operadores lógicos condicionales](expressions.md#conditional-logical-operators) especifican las implementaciones predefinidas de los operadores y cualquier regla adicional que se aplique a cada operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-309">The descriptions of individual operators in [Primary expressions](expressions.md#primary-expressions) through [Conditional logical operators](expressions.md#conditional-logical-operators) specify the predefined implementations of the operators and any additional rules that apply to each operator.</span></span> <span data-ttu-id="212c8-310">En las descripciones se usa la ***resolución de sobrecarga del operador unario***, la ***resolución de sobrecarga del operador binario***y la ***promoción numérica***, que se encuentran en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="212c8-310">The descriptions make use of the terms ***unary operator overload resolution***, ***binary operator overload resolution***, and ***numeric promotion***, definitions of which are found in the following sections.</span></span>

### <a name="unary-operator-overload-resolution"></a><span data-ttu-id="212c8-311">Resolución de sobrecarga del operador unario</span><span class="sxs-lookup"><span data-stu-id="212c8-311">Unary operator overload resolution</span></span>

<span data-ttu-id="212c8-312">Una operación con el formato `op x` o `x op`, donde `op` es un operador unario sobrecargable y `x` es una expresión de tipo `X`, se procesa como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-312">An operation of the form `op x` or `x op`, where `op` is an overloadable unary operator, and `x` is an expression of type `X`, is processed as follows:</span></span>

*  <span data-ttu-id="212c8-313">El conjunto de operadores candidatos definidos por el usuario que proporciona `X` para la operación `operator op(x)` se determina mediante las reglas de los [operadores candidatos definidos por el usuario](expressions.md#candidate-user-defined-operators).</span><span class="sxs-lookup"><span data-stu-id="212c8-313">The set of candidate user-defined operators provided by `X` for the operation `operator op(x)` is determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span>
*  <span data-ttu-id="212c8-314">Si el conjunto de operadores definidos por el usuario candidatos no está vacío, se convierte en el conjunto de operadores candidatos para la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-314">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="212c8-315">De lo contrario, las implementaciones unaria predefinidas `operator op`, incluidos sus formatos de elevación, se convierten en el conjunto de operadores candidatos para la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-315">Otherwise, the predefined unary `operator op` implementations, including their lifted forms, become the set of candidate operators for the operation.</span></span> <span data-ttu-id="212c8-316">Las implementaciones predefinidas de un operador determinado se especifican en la descripción del operador ([expresiones primarias](expressions.md#primary-expressions) y [operadores unarios](expressions.md#unary-operators)).</span><span class="sxs-lookup"><span data-stu-id="212c8-316">The predefined implementations of a given operator are specified in the description of the operator ([Primary expressions](expressions.md#primary-expressions) and [Unary operators](expressions.md#unary-operators)).</span></span>
*  <span data-ttu-id="212c8-317">Las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution) se aplican al conjunto de operadores candidatos para seleccionar el mejor operador con respecto a la lista de argumentos `(x)`, y este operador se convierte en el resultado del proceso de resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="212c8-317">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="212c8-318">Si la resolución de sobrecarga no selecciona un solo operador mejor, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-318">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="binary-operator-overload-resolution"></a><span data-ttu-id="212c8-319">Resolución de sobrecarga del operador binario</span><span class="sxs-lookup"><span data-stu-id="212c8-319">Binary operator overload resolution</span></span>

<span data-ttu-id="212c8-320">Una operación con el formato `x op y`, donde `op` es un operador binario sobrecargable, @no__t 2 es una expresión de tipo `X` y `y` es una expresión de tipo `Y`, se procesa de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-320">An operation of the form `x op y`, where `op` is an overloadable binary operator, `x` is an expression of type `X`, and `y` is an expression of type `Y`, is processed as follows:</span></span>

*  <span data-ttu-id="212c8-321">Se determina el conjunto de operadores candidatos definidos por el usuario que proporciona `X` y `Y` para la operación `operator op(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-321">The set of candidate user-defined operators provided by `X` and `Y` for the operation `operator op(x,y)` is determined.</span></span> <span data-ttu-id="212c8-322">El conjunto consta de la Unión de los operadores candidatos proporcionados por `X` y los operadores candidatos proporcionados por `Y`, cada uno de los cuales se determina mediante las reglas de los [operadores candidatos definidos](expressions.md#candidate-user-defined-operators)por el usuario.</span><span class="sxs-lookup"><span data-stu-id="212c8-322">The set consists of the union of the candidate operators provided by `X` and the candidate operators provided by `Y`, each determined using the rules of [Candidate user-defined operators](expressions.md#candidate-user-defined-operators).</span></span> <span data-ttu-id="212c8-323">Si `X` y `Y` son del mismo tipo, o si `X` y `Y` se derivan de un tipo base común, los operadores de candidatos compartidos solo se producen en el conjunto combinado una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-323">If `X` and `Y` are the same type, or if `X` and `Y` are derived from a common base type, then shared candidate operators only occur in the combined set once.</span></span>
*  <span data-ttu-id="212c8-324">Si el conjunto de operadores definidos por el usuario candidatos no está vacío, se convierte en el conjunto de operadores candidatos para la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-324">If the set of candidate user-defined operators is not empty, then this becomes the set of candidate operators for the operation.</span></span> <span data-ttu-id="212c8-325">De lo contrario, las implementaciones `operator op` binarias predefinidas, incluidas sus formas de elevación, se convierten en el conjunto de operadores candidatos para la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-325">Otherwise, the predefined binary `operator op` implementations, including their lifted forms,  become the set of candidate operators for the operation.</span></span> <span data-ttu-id="212c8-326">Las implementaciones predefinidas de un operador determinado se especifican en la descripción del operador ([operadores aritméticos](expressions.md#arithmetic-operators) a través de [operadores lógicos condicionales](expressions.md#conditional-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="212c8-326">The predefined implementations of a given operator are specified in the description of the operator ([Arithmetic operators](expressions.md#arithmetic-operators) through [Conditional logical operators](expressions.md#conditional-logical-operators)).</span></span> <span data-ttu-id="212c8-327">En el caso de los operadores de enumeración y delegado predefinidos, los únicos operadores considerados son los definidos por un tipo de delegado o de enumeración que es el tipo en tiempo de enlace de uno de los operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-327">For predefined enum and delegate operators, the only operators considered are those defined by an enum or delegate type that is the binding-time type of one of the operands.</span></span>
*  <span data-ttu-id="212c8-328">Las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution) se aplican al conjunto de operadores candidatos para seleccionar el mejor operador con respecto a la lista de argumentos `(x,y)`, y este operador se convierte en el resultado del proceso de resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="212c8-328">The overload resolution rules of [Overload resolution](expressions.md#overload-resolution) are applied to the set of candidate operators to select the best operator with respect to the argument list `(x,y)`, and this operator becomes the result of the overload resolution process.</span></span> <span data-ttu-id="212c8-329">Si la resolución de sobrecarga no selecciona un solo operador mejor, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-329">If overload resolution fails to select a single best operator, a binding-time error occurs.</span></span>

### <a name="candidate-user-defined-operators"></a><span data-ttu-id="212c8-330">Operadores candidatos definidos por el usuario</span><span class="sxs-lookup"><span data-stu-id="212c8-330">Candidate user-defined operators</span></span>

<span data-ttu-id="212c8-331">Dado un tipo `T` y una operación `operator op(A)`, donde `op` es un operador sobrecargable y `A` es una lista de argumentos, el conjunto de operadores candidatos definidos por el usuario proporcionados por `T` para `operator op(A)` se determina de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-331">Given a type `T` and an operation `operator op(A)`, where `op` is an overloadable operator and `A` is an argument list, the set of candidate user-defined operators provided by `T` for `operator op(A)` is determined as follows:</span></span>

*  <span data-ttu-id="212c8-332">Determine el tipo `T0`.</span><span class="sxs-lookup"><span data-stu-id="212c8-332">Determine the type `T0`.</span></span> <span data-ttu-id="212c8-333">Si `T` es un tipo que acepta valores NULL, `T0` es su tipo subyacente, de lo contrario `T0` es igual a `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-333">If `T` is a nullable type, `T0` is its underlying type, otherwise `T0` is equal to `T`.</span></span>
*  <span data-ttu-id="212c8-334">Para todas las declaraciones de `operator op` en `T0` y todas las formas de elevación de estos operadores, si al menos un operador es aplicable ([miembro de función aplicable](expressions.md#applicable-function-member)) con respecto a la lista de argumentos `A`, el conjunto de operadores candidatos está formado por todos estos operadores aplicables en `T0`.</span><span class="sxs-lookup"><span data-stu-id="212c8-334">For all `operator op` declarations in `T0` and all lifted forms of such operators, if at least one operator is applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the argument list `A`, then the set of candidate operators consists of all such applicable operators in `T0`.</span></span>
*  <span data-ttu-id="212c8-335">De lo contrario, si `T0` es `object`, el conjunto de operadores candidatos está vacío.</span><span class="sxs-lookup"><span data-stu-id="212c8-335">Otherwise, if `T0` is `object`, the set of candidate operators is empty.</span></span>
*  <span data-ttu-id="212c8-336">De lo contrario, el conjunto de operadores candidatos proporcionado por `T0` es el conjunto de operadores candidatos que proporciona la clase base directa de `T0`, o la clase base efectiva de `T0` si `T0` es un parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-336">Otherwise, the set of candidate operators provided by `T0` is the set of candidate operators provided by the direct base class of `T0`, or the effective base class of `T0` if `T0` is a type parameter.</span></span>

### <a name="numeric-promotions"></a><span data-ttu-id="212c8-337">Promociones numéricas</span><span class="sxs-lookup"><span data-stu-id="212c8-337">Numeric promotions</span></span>

<span data-ttu-id="212c8-338">La promoción numérica consiste en realizar automáticamente determinadas conversiones implícitas de los operandos de los operadores binarios y unarios predefinidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-338">Numeric promotion consists of automatically performing certain implicit conversions of the operands of the predefined unary and binary numeric operators.</span></span> <span data-ttu-id="212c8-339">La promoción numérica no es un mecanismo distinto, sino un efecto de aplicar la resolución de sobrecarga a los operadores predefinidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-339">Numeric promotion is not a distinct mechanism, but rather an effect of applying overload resolution to the predefined operators.</span></span> <span data-ttu-id="212c8-340">La promoción numérica específicamente no afecta a la evaluación de operadores definidos por el usuario, aunque se pueden implementar operadores definidos por el usuario para mostrar efectos similares.</span><span class="sxs-lookup"><span data-stu-id="212c8-340">Numeric promotion specifically does not affect evaluation of user-defined operators, although user-defined operators can be implemented to exhibit similar effects.</span></span>

<span data-ttu-id="212c8-341">Como ejemplo de promoción numérica, tenga en cuenta las implementaciones predefinidas del operador `*` binario:</span><span class="sxs-lookup"><span data-stu-id="212c8-341">As an example of numeric promotion, consider the predefined implementations of the binary `*` operator:</span></span>

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

<span data-ttu-id="212c8-342">Cuando se aplican las reglas de resolución de sobrecarga ([resolución de sobrecarga](expressions.md#overload-resolution)) a este conjunto de operadores, el efecto es seleccionar el primero de los operadores para los que existen conversiones implícitas desde los tipos de operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-342">When overload resolution rules ([Overload resolution](expressions.md#overload-resolution)) are applied to this set of operators, the effect is to select the first of the operators for which implicit conversions exist from the operand types.</span></span> <span data-ttu-id="212c8-343">Por ejemplo, para la operación `b * s`, donde `b` es un @no__t 2 y `s` es una `short`, la resolución de sobrecarga selecciona `operator *(int,int)` como el mejor operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-343">For example, for the operation `b * s`, where `b` is a `byte` and `s` is a `short`, overload resolution selects `operator *(int,int)` as the best operator.</span></span> <span data-ttu-id="212c8-344">Por lo tanto, el efecto es que `b` y `s` se convierten a `int` y el tipo del resultado es `int`.</span><span class="sxs-lookup"><span data-stu-id="212c8-344">Thus, the effect is that `b` and `s` are converted to `int`, and the type of the result is `int`.</span></span> <span data-ttu-id="212c8-345">Del mismo modo, para la operación `i * d`, donde `i` es una `int` y `d` es una `double`, la resolución de sobrecarga selecciona `operator *(double,double)` como el mejor operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-345">Likewise, for the operation `i * d`, where `i` is an `int` and `d` is a `double`, overload resolution selects `operator *(double,double)` as the best operator.</span></span>

#### <a name="unary-numeric-promotions"></a><span data-ttu-id="212c8-346">Promociones numéricas unarias</span><span class="sxs-lookup"><span data-stu-id="212c8-346">Unary numeric promotions</span></span>

<span data-ttu-id="212c8-347">La promoción numérica unaria se produce para los operandos de los operadores unarios predefinidos `+`, `-` y `~`.</span><span class="sxs-lookup"><span data-stu-id="212c8-347">Unary numeric promotion occurs for the operands of the predefined `+`, `-`, and `~` unary operators.</span></span> <span data-ttu-id="212c8-348">La promoción numérica unaria simplemente consiste en convertir operandos de tipo `sbyte`, `byte`, @no__t 2, `ushort` o `char` al tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="212c8-348">Unary numeric promotion simply consists of converting operands of type `sbyte`, `byte`, `short`, `ushort`, or `char` to type `int`.</span></span> <span data-ttu-id="212c8-349">Además, para el operador unario `-`, la promoción numérica unaria convierte los operandos del tipo `uint` al tipo `long`.</span><span class="sxs-lookup"><span data-stu-id="212c8-349">Additionally, for the unary `-` operator, unary numeric promotion converts operands of type `uint` to type `long`.</span></span>

#### <a name="binary-numeric-promotions"></a><span data-ttu-id="212c8-350">Promociones numéricas binarias</span><span class="sxs-lookup"><span data-stu-id="212c8-350">Binary numeric promotions</span></span>

<span data-ttu-id="212c8-351">La promoción numérica binaria se produce para los operandos de los valores predefinidos `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, 0, 1, 2 y 3 operadores binarios.</span><span class="sxs-lookup"><span data-stu-id="212c8-351">Binary numeric promotion occurs for the operands of the predefined `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, and `<=` binary operators.</span></span> <span data-ttu-id="212c8-352">La promoción numérica binaria convierte implícitamente ambos operandos en un tipo común que, en el caso de los operadores no relacionales, también se convierte en el tipo de resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-352">Binary numeric promotion implicitly converts both operands to a common type which, in case of the non-relational operators, also becomes the result type of the operation.</span></span> <span data-ttu-id="212c8-353">La promoción numérica binaria consiste en aplicar las siguientes reglas, en el orden en que aparecen aquí:</span><span class="sxs-lookup"><span data-stu-id="212c8-353">Binary numeric promotion consists of applying the following rules, in the order they appear here:</span></span>

*  <span data-ttu-id="212c8-354">Si alguno de los operandos es de tipo `decimal`, el otro operando se convierte al tipo `decimal`, o se produce un error en tiempo de enlace si el otro operando es de tipo `float` o `double`.</span><span class="sxs-lookup"><span data-stu-id="212c8-354">If either operand is of type `decimal`, the other operand is converted to type `decimal`, or a binding-time error occurs if the other operand is of type `float` or `double`.</span></span>
*  <span data-ttu-id="212c8-355">De lo contrario, si alguno de los operandos es de tipo `double`, el otro operando se convierte al tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="212c8-355">Otherwise, if either operand is of type `double`, the other operand is converted to type `double`.</span></span>
*  <span data-ttu-id="212c8-356">De lo contrario, si alguno de los operandos es de tipo `float`, el otro operando se convierte al tipo `float`.</span><span class="sxs-lookup"><span data-stu-id="212c8-356">Otherwise, if either operand is of type `float`, the other operand is converted to type `float`.</span></span>
*  <span data-ttu-id="212c8-357">De lo contrario, si alguno de los operandos es de tipo `ulong`, el otro operando se convierte al tipo `ulong`, o se produce un error en tiempo de enlace si el otro operando es de tipo `sbyte`, `short`, `int` o `long`.</span><span class="sxs-lookup"><span data-stu-id="212c8-357">Otherwise, if either operand is of type `ulong`, the other operand is converted to type `ulong`, or a binding-time error occurs if the other operand is of type `sbyte`, `short`, `int`, or `long`.</span></span>
*  <span data-ttu-id="212c8-358">De lo contrario, si alguno de los operandos es de tipo `long`, el otro operando se convierte al tipo `long`.</span><span class="sxs-lookup"><span data-stu-id="212c8-358">Otherwise, if either operand is of type `long`, the other operand is converted to type `long`.</span></span>
*  <span data-ttu-id="212c8-359">De lo contrario, si alguno de los operandos es de tipo `uint` y el otro operando es de tipo `sbyte`, `short` o `int`, ambos operandos se convierten al tipo `long`.</span><span class="sxs-lookup"><span data-stu-id="212c8-359">Otherwise, if either operand is of type `uint` and the other operand is of type `sbyte`, `short`, or `int`, both operands are converted to type `long`.</span></span>
*  <span data-ttu-id="212c8-360">De lo contrario, si alguno de los operandos es de tipo `uint`, el otro operando se convierte al tipo `uint`.</span><span class="sxs-lookup"><span data-stu-id="212c8-360">Otherwise, if either operand is of type `uint`, the other operand is converted to type `uint`.</span></span>
*  <span data-ttu-id="212c8-361">De lo contrario, ambos operandos se convierten al tipo `int`.</span><span class="sxs-lookup"><span data-stu-id="212c8-361">Otherwise, both operands are converted to type `int`.</span></span>

<span data-ttu-id="212c8-362">Tenga en cuenta que la primera regla no permite ninguna operación que mezcle el tipo `decimal` con los tipos `double` y `float`.</span><span class="sxs-lookup"><span data-stu-id="212c8-362">Note that the first rule disallows any operations that mix the `decimal` type with the `double` and `float` types.</span></span> <span data-ttu-id="212c8-363">La regla sigue el hecho de que no hay conversiones implícitas entre el tipo `decimal` y los tipos `double` y `float`.</span><span class="sxs-lookup"><span data-stu-id="212c8-363">The rule follows from the fact that there are no implicit conversions between the `decimal` type and the `double` and `float` types.</span></span>

<span data-ttu-id="212c8-364">Tenga en cuenta también que no es posible que un operando sea de tipo `ulong` cuando el otro operando es de un tipo entero con signo.</span><span class="sxs-lookup"><span data-stu-id="212c8-364">Also note that it is not possible for an operand to be of type `ulong` when the other operand is of a signed integral type.</span></span> <span data-ttu-id="212c8-365">La razón es que no existe ningún tipo entero que pueda representar el intervalo completo de `ulong`, así como los tipos enteros con signo.</span><span class="sxs-lookup"><span data-stu-id="212c8-365">The reason is that no integral type exists that can represent the full range of `ulong` as well as the signed integral types.</span></span>

<span data-ttu-id="212c8-366">En los dos casos anteriores, se puede usar una expresión de conversión para convertir explícitamente un operando en un tipo que sea compatible con el otro operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-366">In both of the above cases, a cast expression can be used to explicitly convert one operand to a type that is compatible with the other operand.</span></span>

<span data-ttu-id="212c8-367">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-367">In the example</span></span>
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
<span data-ttu-id="212c8-368">se produce un error en tiempo de enlace porque un `decimal` no se puede multiplicar por `double`.</span><span class="sxs-lookup"><span data-stu-id="212c8-368">a binding-time error occurs because a `decimal` cannot be multiplied by a `double`.</span></span> <span data-ttu-id="212c8-369">El error se resuelve convirtiendo explícitamente el segundo operando en `decimal`, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-369">The error is resolved by explicitly converting the second operand to `decimal`, as follows:</span></span>

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a><span data-ttu-id="212c8-370">Operadores de elevación</span><span class="sxs-lookup"><span data-stu-id="212c8-370">Lifted operators</span></span>

<span data-ttu-id="212c8-371">Los ***operadores de elevación*** permiten a los operadores predefinidos y definidos por el usuario que operan en tipos de valor que no aceptan valores NULL usarse también con formas que aceptan valores NULL de esos tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-371">***Lifted operators*** permit predefined and user-defined operators that operate on non-nullable value types to also be used with nullable forms of those types.</span></span> <span data-ttu-id="212c8-372">Los operadores de elevación se construyen a partir de operadores predefinidos y definidos por el usuario que cumplen ciertos requisitos, como se describe a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-372">Lifted operators are constructed from predefined and user-defined operators that meet certain requirements, as described in the following:</span></span>

*   <span data-ttu-id="212c8-373">Para los operadores unarios</span><span class="sxs-lookup"><span data-stu-id="212c8-373">For the unary operators</span></span>

    ```csharp
    +  ++  -  --  !  ~
    ```

    <span data-ttu-id="212c8-374">existe una forma de elevación de un operador si el operando y los tipos de resultado son tipos de valor que no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-374">a lifted form of an operator exists if the operand and result types are both non-nullable value types.</span></span> <span data-ttu-id="212c8-375">La forma de elevación se construye agregando un único modificador `?` a los tipos de operando y de resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-375">The lifted form is constructed by adding a single `?` modifier to the operand and result types.</span></span> <span data-ttu-id="212c8-376">El operador de elevación genera un valor NULL si el operando es NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-376">The lifted operator produces a null value if the operand is null.</span></span> <span data-ttu-id="212c8-377">De lo contrario, el operador de elevación desencapsula el operando, aplica el operador subyacente y ajusta el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-377">Otherwise, the lifted operator unwraps the operand, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="212c8-378">Para los operadores binarios</span><span class="sxs-lookup"><span data-stu-id="212c8-378">For the binary operators</span></span>

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    <span data-ttu-id="212c8-379">existe una forma de elevación de un operador si el operando y los tipos de resultado son todos tipos de valor que no aceptan valores NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-379">a lifted form of an operator exists if the operand and result types are all non-nullable value types.</span></span> <span data-ttu-id="212c8-380">La forma de elevación se construye agregando un único modificador `?` a cada operando y tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-380">The lifted form is constructed by adding a single `?` modifier to each operand and result type.</span></span> <span data-ttu-id="212c8-381">El operador de elevación genera un valor NULL si uno o los dos operandos son NULL (una excepción son los operadores `&` y `|` del tipo `bool?`, como se describe en [operadores lógicos booleanos](expressions.md#boolean-logical-operators)).</span><span class="sxs-lookup"><span data-stu-id="212c8-381">The lifted operator produces a null value if one or both operands are null (an exception being the `&` and `|` operators of the `bool?` type, as described in [Boolean logical operators](expressions.md#boolean-logical-operators)).</span></span> <span data-ttu-id="212c8-382">De lo contrario, el operador de elevación desencapsula los operandos, aplica el operador subyacente y ajusta el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-382">Otherwise, the lifted operator unwraps the operands, applies the underlying operator, and wraps the result.</span></span>

*   <span data-ttu-id="212c8-383">Para los operadores de igualdad</span><span class="sxs-lookup"><span data-stu-id="212c8-383">For the equality operators</span></span>

    ```csharp
    ==  !=
    ```

    <span data-ttu-id="212c8-384">existe una forma de elevación de un operador si los tipos de operando son tipos de valor que no aceptan valores NULL y si el tipo de resultado es `bool`.</span><span class="sxs-lookup"><span data-stu-id="212c8-384">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="212c8-385">La forma de elevación se construye agregando un único modificador `?` a cada tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-385">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="212c8-386">El operador de elevación considera que dos valores NULL son iguales y un valor NULL es distinto de cualquier valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-386">The lifted operator considers two null values equal, and a null value unequal to any non-null value.</span></span> <span data-ttu-id="212c8-387">Si ambos operandos no son NULL, el operador de elevación desencapsula los operandos y aplica el operador subyacente para generar el resultado `bool`.</span><span class="sxs-lookup"><span data-stu-id="212c8-387">If both operands are non-null, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

*   <span data-ttu-id="212c8-388">Para los operadores relacionales</span><span class="sxs-lookup"><span data-stu-id="212c8-388">For the relational operators</span></span>

    ```csharp
    <  >  <=  >=
    ```

    <span data-ttu-id="212c8-389">existe una forma de elevación de un operador si los tipos de operando son tipos de valor que no aceptan valores NULL y si el tipo de resultado es `bool`.</span><span class="sxs-lookup"><span data-stu-id="212c8-389">a lifted form of an operator exists if the operand types are both non-nullable value types and if the result type is `bool`.</span></span> <span data-ttu-id="212c8-390">La forma de elevación se construye agregando un único modificador `?` a cada tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-390">The lifted form is constructed by adding a single `?` modifier to each operand type.</span></span> <span data-ttu-id="212c8-391">El operador de elevación produce el valor `false` si uno o los dos operandos son NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-391">The lifted operator produces the value `false` if one or both operands are null.</span></span> <span data-ttu-id="212c8-392">De lo contrario, el operador de elevación desencapsula los operandos y aplica el operador subyacente para generar el resultado `bool`.</span><span class="sxs-lookup"><span data-stu-id="212c8-392">Otherwise, the lifted operator unwraps the operands and applies the underlying operator to produce the `bool` result.</span></span>

## <a name="member-lookup"></a><span data-ttu-id="212c8-393">Búsqueda de miembros</span><span class="sxs-lookup"><span data-stu-id="212c8-393">Member lookup</span></span>

<span data-ttu-id="212c8-394">Una búsqueda de miembros es el proceso por el que se determina el significado de un nombre en el contexto de un tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-394">A member lookup is the process whereby the meaning of a name in the context of a type is determined.</span></span> <span data-ttu-id="212c8-395">Una búsqueda de miembros se puede producir como parte de la evaluación de *simple_name* ([nombres simples](expressions.md#simple-names)) o de *member_access* ([acceso a miembros](expressions.md#member-access)) en una expresión.</span><span class="sxs-lookup"><span data-stu-id="212c8-395">A member lookup can occur as part of evaluating a *simple_name* ([Simple names](expressions.md#simple-names)) or a *member_access* ([Member access](expressions.md#member-access)) in an expression.</span></span> <span data-ttu-id="212c8-396">Si *simple_name* o *member_access* se produce como *primary_expression* de un *invocation_expression* (invocaciones de[método](expressions.md#method-invocations)), se dice que el miembro se invoca.</span><span class="sxs-lookup"><span data-stu-id="212c8-396">If the *simple_name* or *member_access* occurs as the *primary_expression* of an *invocation_expression* ([Method invocations](expressions.md#method-invocations)), the member is said to be invoked.</span></span>

<span data-ttu-id="212c8-397">Si un miembro es un método o un evento, o si es una constante, un campo o una propiedad de un tipo de delegado ([delegados](delegates.md)) o del tipo `dynamic` ([el tipo dinámico](types.md#the-dynamic-type)), se dice que el miembro es *invocable*.</span><span class="sxs-lookup"><span data-stu-id="212c8-397">If a member is a method or event, or if it is a constant, field or property of either a delegate type ([Delegates](delegates.md)) or the type `dynamic` ([The dynamic type](types.md#the-dynamic-type)), then the member is said to be *invocable*.</span></span>

<span data-ttu-id="212c8-398">La búsqueda de miembros considera no solo el nombre de un miembro, sino también el número de parámetros de tipo que tiene el miembro y si el miembro es accesible.</span><span class="sxs-lookup"><span data-stu-id="212c8-398">Member lookup considers not only the name of a member but also the number of type parameters the member has and whether the member is accessible.</span></span> <span data-ttu-id="212c8-399">En lo que respecta a la búsqueda de miembros, los métodos genéricos y los tipos genéricos anidados tienen el número de parámetros de tipo indicado en sus declaraciones respectivas y todos los demás miembros tienen parámetros de tipo cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-399">For the purposes of member lookup, generic methods and nested generic types have the number of type parameters indicated in their respective declarations and all other members have zero type parameters.</span></span>

<span data-ttu-id="212c8-400">Una búsqueda de miembro de un nombre @ no__t-0 con parámetros `K` @ no__t-2Type en un tipo @ no__t-3 se procesa de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-400">A member lookup of a name `N` with `K` type parameters in a type `T` is processed as follows:</span></span>

*  <span data-ttu-id="212c8-401">En primer lugar, se determina un conjunto de miembros accesibles denominados @ no__t-0:</span><span class="sxs-lookup"><span data-stu-id="212c8-401">First, a set of accessible members named `N` is determined:</span></span>
    * <span data-ttu-id="212c8-402">Si `T` es un parámetro de tipo, el conjunto es la Unión de los conjuntos de miembros accesibles denominados @ no__t-1 en cada uno de los tipos especificados como restricción principal o restricción secundaria ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) para @ no__t-3, junto con el conjunto de miembros accesibles denominados @ no__t-4 en `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-402">If `T` is a type parameter, then the set is the union of the sets of accessible members named `N` in each of the types specified as a primary constraint or secondary constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) for `T`, along with the set of accessible members named `N` in `object`.</span></span>
    * <span data-ttu-id="212c8-403">De lo contrario, el conjunto se compone de todos los miembros accesibles ([acceso a miembros](basic-concepts.md#member-access)) denominados @ no__t-1 en @ no__t-2, incluidos los miembros heredados y los miembros accesibles denominados @ no__t-3 en `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-403">Otherwise, the set consists of all accessible ([Member access](basic-concepts.md#member-access)) members named `N` in `T`, including inherited members and the accessible members named `N` in `object`.</span></span> <span data-ttu-id="212c8-404">Si `T` es un tipo construido, el conjunto de miembros se obtiene sustituyendo los argumentos de tipo tal y como se describe en [miembros de tipos construidos](classes.md#members-of-constructed-types).</span><span class="sxs-lookup"><span data-stu-id="212c8-404">If `T` is a constructed type, the set of members is obtained by substituting type arguments as described in [Members of constructed types](classes.md#members-of-constructed-types).</span></span> <span data-ttu-id="212c8-405">Los miembros que incluyen un modificador `override` se excluyen del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-405">Members that include an `override` modifier are excluded from the set.</span></span>
*  <span data-ttu-id="212c8-406">Después, si `K` es cero, se quitan todos los tipos anidados cuyas declaraciones incluyen parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-406">Next, if `K` is zero, all nested types whose declarations include type parameters are removed.</span></span> <span data-ttu-id="212c8-407">Si `K` no es cero, se quitan todos los miembros con un número diferente de parámetros de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-407">If `K` is not zero, all members with a different number of type parameters are removed.</span></span> <span data-ttu-id="212c8-408">Tenga en cuenta que cuando `K` es cero, no se quitan los métodos que tienen parámetros de tipo, ya que el proceso de inferencia de tipos ([inferencia de tipos](expressions.md#type-inference)) podría inferir los argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-408">Note that when `K` is zero, methods having type parameters are not removed, since the type inference process ([Type inference](expressions.md#type-inference)) might be able to infer the type arguments.</span></span>
*  <span data-ttu-id="212c8-409">Después, si se *invoca*el miembro, todos los miembros que no sean de*invocable* se quitarán del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-409">Next, if the member is *invoked*, all non-*invocable* members are removed from the set.</span></span>
*  <span data-ttu-id="212c8-410">Después, los miembros que están ocultos por otros miembros se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-410">Next, members that are hidden by other members are removed from the set.</span></span> <span data-ttu-id="212c8-411">Para cada miembro `S.M` del conjunto, donde `S` es el tipo en el que se declara el miembro @ no__t-2, se aplican las reglas siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-411">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied:</span></span>
    * <span data-ttu-id="212c8-412">Si `M` es una constante, un campo, una propiedad, un evento o un miembro de enumeración, todos los miembros declarados en un tipo base de `S` se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-412">If `M` is a constant, field, property, event, or enumeration member, then all members declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="212c8-413">Si `M` es una declaración de tipos, todos los tipos no que se declaran en un tipo base de `S` se quitan del conjunto y todas las declaraciones de tipos con el mismo número de parámetros de tipo que `M` declarados en un tipo base de `S` se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-413">If `M` is a type declaration, then all non-types declared in a base type of `S` are removed from the set, and all type declarations with the same number of type parameters as `M` declared in a base type of `S` are removed from the set.</span></span>
    * <span data-ttu-id="212c8-414">Si `M` es un método, todos los miembros que no son de método declarados en un tipo base de `S` se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-414">If `M` is a method, then all non-method members declared in a base type of `S` are removed from the set.</span></span>
*  <span data-ttu-id="212c8-415">Después, los miembros de interfaz que están ocultos por miembros de clase se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-415">Next, interface members that are hidden by class members are removed from the set.</span></span> <span data-ttu-id="212c8-416">Este paso solo tiene efecto si `T` es un parámetro de tipo y `T` tiene una clase base efectiva distinta de `object` y un conjunto de interfaces efectivo no vacío ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="212c8-416">This step only has an effect if `T` is a type parameter and `T` has both an effective base class other than `object` and a non-empty effective interface set ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="212c8-417">Para cada miembro `S.M` del conjunto, donde `S` es el tipo en el que se declara el miembro `M`, se aplican las reglas siguientes si `S` es una declaración de clase distinta de `object`:</span><span class="sxs-lookup"><span data-stu-id="212c8-417">For every member `S.M` in the set, where `S` is the type in which the member `M` is declared, the following rules are applied if `S` is a class declaration other than `object`:</span></span>
    * <span data-ttu-id="212c8-418">Si `M` es una constante, un campo, una propiedad, un evento, un miembro de enumeración o una declaración de tipos, todos los miembros declarados en una declaración de interfaz se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-418">If `M` is a constant, field, property, event, enumeration member, or type declaration, then all members declared in an interface declaration are removed from the set.</span></span>
    * <span data-ttu-id="212c8-419">Si `M` es un método, todos los miembros que no sean de método declarados en una declaración de interfaz se quitan del conjunto, y todos los métodos con la misma signatura que `M` declarados en una declaración de interfaz se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-419">If `M` is a method, then all non-method members declared in an interface declaration are removed from the set, and all methods with the same signature as `M` declared in an interface declaration are removed from the set.</span></span>
*  <span data-ttu-id="212c8-420">Por último, si se han quitado los miembros ocultos, se determina el resultado de la búsqueda:</span><span class="sxs-lookup"><span data-stu-id="212c8-420">Finally, having removed hidden members, the result of the lookup is determined:</span></span>
    * <span data-ttu-id="212c8-421">Si el conjunto está formado por un único miembro que no es un método, este miembro es el resultado de la búsqueda.</span><span class="sxs-lookup"><span data-stu-id="212c8-421">If the set consists of a single member that is not a method, then this member is the result of the lookup.</span></span>
    * <span data-ttu-id="212c8-422">De lo contrario, si el conjunto solo contiene métodos, este grupo de métodos es el resultado de la búsqueda.</span><span class="sxs-lookup"><span data-stu-id="212c8-422">Otherwise, if the set contains only methods, then this group of methods is the result of the lookup.</span></span>
    * <span data-ttu-id="212c8-423">De lo contrario, la búsqueda es ambigua y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-423">Otherwise, the lookup is ambiguous, and a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-424">Para búsquedas de miembros en tipos que no sean de tipo e interfaces, y búsquedas de miembros en interfaces que son estrictamente de herencia única (cada interfaz de la cadena de herencia tiene exactamente cero o una interfaz base directa), el efecto de las reglas de búsqueda es simplemente los miembros derivados ocultan los miembros base con el mismo nombre o signatura.</span><span class="sxs-lookup"><span data-stu-id="212c8-424">For member lookups in types other than type parameters and interfaces, and member lookups in interfaces that are strictly single-inheritance (each interface in the inheritance chain has exactly zero or one direct base interface), the effect of the lookup rules is simply that derived members hide base members with the same name or signature.</span></span> <span data-ttu-id="212c8-425">Dichas búsquedas de herencia única nunca son ambiguas.</span><span class="sxs-lookup"><span data-stu-id="212c8-425">Such single-inheritance lookups are never ambiguous.</span></span> <span data-ttu-id="212c8-426">Las ambigüedades que pueden surgir en las búsquedas de miembros en interfaces de herencia múltiple se describen en [acceso a miembros de interfaz](interfaces.md#interface-member-access).</span><span class="sxs-lookup"><span data-stu-id="212c8-426">The ambiguities that can possibly arise from member lookups in multiple-inheritance interfaces are described in [Interface member access](interfaces.md#interface-member-access).</span></span>

### <a name="base-types"></a><span data-ttu-id="212c8-427">Tipos base</span><span class="sxs-lookup"><span data-stu-id="212c8-427">Base types</span></span>

<span data-ttu-id="212c8-428">En lo que respecta a la búsqueda de miembros, se considera que un tipo `T` tiene los siguientes tipos base:</span><span class="sxs-lookup"><span data-stu-id="212c8-428">For purposes of member lookup, a type `T` is considered to have the following base types:</span></span>

*  <span data-ttu-id="212c8-429">Si `T` es `object`, `T` no tiene tipo base.</span><span class="sxs-lookup"><span data-stu-id="212c8-429">If `T` is `object`, then `T` has no base type.</span></span>
*  <span data-ttu-id="212c8-430">Si `T` es un *enum_type*, los tipos base de `T` son los tipos de clase `System.Enum`, `System.ValueType` y `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-430">If `T` is an *enum_type*, the base types of `T` are the class types `System.Enum`, `System.ValueType`, and `object`.</span></span>
*  <span data-ttu-id="212c8-431">Si `T` es un *struct_type*, los tipos base de `T` son los tipos de clase `System.ValueType` y `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-431">If `T` is a *struct_type*, the base types of `T` are the class types `System.ValueType` and `object`.</span></span>
*  <span data-ttu-id="212c8-432">Si `T` es un *class_type*, los tipos base de `T` son las clases base de `T`, incluido el tipo de clase `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-432">If `T` is a *class_type*, the base types of `T` are the base classes of `T`, including the class type `object`.</span></span>
*  <span data-ttu-id="212c8-433">Si `T` es un *interface_type*, los tipos base de `T` son las interfaces base de `T` y el tipo de clase `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-433">If `T` is an *interface_type*, the base types of `T` are the base interfaces of `T` and the class type `object`.</span></span>
*  <span data-ttu-id="212c8-434">Si `T` es un *array_type*, los tipos base de `T` son los tipos de clase `System.Array` y `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-434">If `T` is an *array_type*, the base types of `T` are the class types `System.Array` and `object`.</span></span>
*  <span data-ttu-id="212c8-435">Si `T` es un *delegate_type*, los tipos base de `T` son los tipos de clase `System.Delegate` y `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-435">If `T` is a *delegate_type*, the base types of `T` are the class types `System.Delegate` and `object`.</span></span>

## <a name="function-members"></a><span data-ttu-id="212c8-436">Miembros de función</span><span class="sxs-lookup"><span data-stu-id="212c8-436">Function members</span></span>

<span data-ttu-id="212c8-437">Los miembros de función son miembros que contienen instrucciones ejecutables.</span><span class="sxs-lookup"><span data-stu-id="212c8-437">Function members are members that contain executable statements.</span></span> <span data-ttu-id="212c8-438">Los miembros de función siempre son miembros de tipos y no pueden ser miembros de espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="212c8-438">Function members are always members of types and cannot be members of namespaces.</span></span> <span data-ttu-id="212c8-439">C#define las siguientes categorías de miembros de función:</span><span class="sxs-lookup"><span data-stu-id="212c8-439">C# defines the following categories of function members:</span></span>

*  <span data-ttu-id="212c8-440">Métodos</span><span class="sxs-lookup"><span data-stu-id="212c8-440">Methods</span></span>
*  <span data-ttu-id="212c8-441">Propiedades</span><span class="sxs-lookup"><span data-stu-id="212c8-441">Properties</span></span>
*  <span data-ttu-id="212c8-442">Events</span><span class="sxs-lookup"><span data-stu-id="212c8-442">Events</span></span>
*  <span data-ttu-id="212c8-443">Indizadores</span><span class="sxs-lookup"><span data-stu-id="212c8-443">Indexers</span></span>
*  <span data-ttu-id="212c8-444">Operadores definidos por el usuario</span><span class="sxs-lookup"><span data-stu-id="212c8-444">User-defined operators</span></span>
*  <span data-ttu-id="212c8-445">Constructores de instancias</span><span class="sxs-lookup"><span data-stu-id="212c8-445">Instance constructors</span></span>
*  <span data-ttu-id="212c8-446">Constructores estáticos</span><span class="sxs-lookup"><span data-stu-id="212c8-446">Static constructors</span></span>
*  <span data-ttu-id="212c8-447">Destructores</span><span class="sxs-lookup"><span data-stu-id="212c8-447">Destructors</span></span>

<span data-ttu-id="212c8-448">A excepción de los destructores y los constructores estáticos (que no se pueden invocar explícitamente), las instrucciones contenidas en miembros de función se ejecutan a través de las invocaciones de miembros de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-448">Except for destructors and static constructors (which cannot be invoked explicitly), the statements contained in function members are executed through function member invocations.</span></span> <span data-ttu-id="212c8-449">La sintaxis real para escribir una invocación de miembros de función depende de la categoría de miembro de función determinada.</span><span class="sxs-lookup"><span data-stu-id="212c8-449">The actual syntax for writing a function member invocation depends on the particular function member category.</span></span>

<span data-ttu-id="212c8-450">La lista de argumentos ([listas de argumentos](expressions.md#argument-lists)) de una invocación de miembros de función proporciona valores reales o referencias de variable para los parámetros del miembro de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-450">The argument list ([Argument lists](expressions.md#argument-lists)) of a function member invocation provides actual values or variable references for the parameters of the function member.</span></span>

<span data-ttu-id="212c8-451">Las invocaciones de métodos genéricos pueden emplear la inferencia de tipos para determinar el conjunto de argumentos de tipo que se van a pasar al método.</span><span class="sxs-lookup"><span data-stu-id="212c8-451">Invocations of generic methods may employ type inference to determine the set of type arguments to pass to the method.</span></span> <span data-ttu-id="212c8-452">Este proceso se describe en [inferencia de tipos](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="212c8-452">This process is described in [Type inference](expressions.md#type-inference).</span></span>

<span data-ttu-id="212c8-453">Las invocaciones de métodos, indizadores, operadores y constructores de instancia emplean la resolución de sobrecarga para determinar cuál de un conjunto candidato de miembros de función se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="212c8-453">Invocations of methods, indexers, operators and instance constructors employ overload resolution to determine which of a candidate set of function members to invoke.</span></span> <span data-ttu-id="212c8-454">Este proceso se describe en [resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="212c8-454">This process is described in [Overload resolution](expressions.md#overload-resolution).</span></span>

<span data-ttu-id="212c8-455">Una vez que se ha identificado un miembro de función determinado en tiempo de enlace, posiblemente a través de la resolución de sobrecarga, el proceso real en tiempo de ejecución de la invocación del miembro de función se describe en la [comprobación en tiempo de compilación de la resolución de sobrecarga dinámica](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="212c8-455">Once a particular function member has been identified at binding-time, possibly through overload resolution, the actual run-time process of invoking the function member is described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="212c8-456">En la tabla siguiente se resume el procesamiento que tiene lugar en construcciones que implican las seis categorías de miembros de función que se pueden invocar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-456">The following table summarizes the processing that takes place in constructs involving the six categories of function members that can be explicitly invoked.</span></span> <span data-ttu-id="212c8-457">En la tabla, `e`, `x`, `y` y `value` indican expresiones clasificadas como variables o valores, `T` indica una expresión clasificada como un tipo, `F` es el nombre simple de un método y `P` es el nombre simple de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="212c8-457">In the table, `e`, `x`, `y`, and `value` indicate expressions classified as variables or values, `T` indicates an expression classified as a type, `F` is the simple name of a method, and `P` is the simple name of a property.</span></span>


| <span data-ttu-id="212c8-458">__Construir__</span><span class="sxs-lookup"><span data-stu-id="212c8-458">__Construct__</span></span>     | <span data-ttu-id="212c8-459">__Ejemplo__</span><span class="sxs-lookup"><span data-stu-id="212c8-459">__Example__</span></span>    | <span data-ttu-id="212c8-460">__Descripción__</span><span class="sxs-lookup"><span data-stu-id="212c8-460">__Description__</span></span> |
|-------------------|----------------|-----------------|
| <span data-ttu-id="212c8-461">Invocación del método.</span><span class="sxs-lookup"><span data-stu-id="212c8-461">Method invocation</span></span> | `F(x,y)`       | <span data-ttu-id="212c8-462">Se aplica la resolución de sobrecarga para seleccionar el mejor método `F` en la clase contenedora o struct.</span><span class="sxs-lookup"><span data-stu-id="212c8-462">Overload resolution is applied to select the best method `F` in the containing class or struct.</span></span> <span data-ttu-id="212c8-463">El método se invoca con la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-463">The method is invoked with the argument list `(x,y)`.</span></span> <span data-ttu-id="212c8-464">Si el método no es `static`, la expresión de instancia es `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-464">If the method is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.F(x,y)`     | <span data-ttu-id="212c8-465">Se aplica la resolución de sobrecarga para seleccionar el mejor método `F` en la clase o struct `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-465">Overload resolution is applied to select the best method `F` in the class or struct `T`.</span></span> <span data-ttu-id="212c8-466">Se produce un error en tiempo de enlace si el método no es `static`.</span><span class="sxs-lookup"><span data-stu-id="212c8-466">A binding-time error occurs if the method is not `static`.</span></span> <span data-ttu-id="212c8-467">El método se invoca con la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-467">The method is invoked with the argument list `(x,y)`.</span></span> | 
|                   | `e.F(x,y)`     | <span data-ttu-id="212c8-468">La resolución de sobrecarga se aplica para seleccionar el mejor método F en la clase, estructura o interfaz dada por el tipo de `e`.</span><span class="sxs-lookup"><span data-stu-id="212c8-468">Overload resolution is applied to select the best method F in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="212c8-469">Se produce un error en tiempo de enlace si el método es `static`.</span><span class="sxs-lookup"><span data-stu-id="212c8-469">A binding-time error occurs if the method is `static`.</span></span> <span data-ttu-id="212c8-470">El método se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-470">The method is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="212c8-471">Property Access</span><span class="sxs-lookup"><span data-stu-id="212c8-471">Property access</span></span>   | `P`            | <span data-ttu-id="212c8-472">Se invoca al descriptor de acceso `get` de la propiedad `P` en la clase o estructura contenedora.</span><span class="sxs-lookup"><span data-stu-id="212c8-472">The `get` accessor of the property `P` in the containing class or struct is invoked.</span></span> <span data-ttu-id="212c8-473">Se produce un error en tiempo de compilación si `P` es de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="212c8-473">A compile-time error occurs if `P` is write-only.</span></span> <span data-ttu-id="212c8-474">Si `P` no es `static`, la expresión de instancia es @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="212c8-474">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `P = value`    | <span data-ttu-id="212c8-475">El descriptor de acceso `set` de la propiedad `P` en la clase o estructura contenedora se invoca con la lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-475">The `set` accessor of the property `P` in the containing class or struct is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="212c8-476">Se produce un error en tiempo de compilación si `P` es de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="212c8-476">A compile-time error occurs if `P` is read-only.</span></span> <span data-ttu-id="212c8-477">Si `P` no es `static`, la expresión de instancia es @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="212c8-477">If `P` is not `static`, the instance expression is `this`.</span></span> | 
|                   | `T.P`          | <span data-ttu-id="212c8-478">Se invoca al descriptor de acceso `get` de la propiedad `P` en la clase o struct `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-478">The `get` accessor of the property `P` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="212c8-479">Se produce un error en tiempo de compilación si `P` no es `static` o si `P` es de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="212c8-479">A compile-time error occurs if `P` is not `static` or if `P` is write-only.</span></span> | 
|                   | `T.P = value`  | <span data-ttu-id="212c8-480">El descriptor de acceso `set` de la propiedad `P` de la clase o estructura `T` se invoca con la lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-480">The `set` accessor of the property `P` in the class or struct `T` is invoked with the argument list `(value)`.</span></span> <span data-ttu-id="212c8-481">Se produce un error en tiempo de compilación si `P` no es `static` o si `P` es de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="212c8-481">A compile-time error occurs if `P` is not `static` or if `P` is read-only.</span></span> | 
|                   | `e.P`          | <span data-ttu-id="212c8-482">El descriptor de acceso `get` de la propiedad `P` en la clase, estructura o interfaz proporcionada por el tipo de `e` se invoca con la expresión de instancia `e`.</span><span class="sxs-lookup"><span data-stu-id="212c8-482">The `get` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="212c8-483">Se produce un error en tiempo de enlace si `P` es `static` o si `P` es de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="212c8-483">A binding-time error occurs if `P` is `static` or if `P` is write-only.</span></span> | 
|                   | `e.P = value`  | <span data-ttu-id="212c8-484">El descriptor de acceso `set` de la propiedad `P` en la clase, estructura o interfaz proporcionada por el tipo de `e` se invoca con la expresión de instancia `e` y la lista de argumentos `(value)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-484">The `set` accessor of the property `P` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e` and the argument list `(value)`.</span></span> <span data-ttu-id="212c8-485">Se produce un error en tiempo de enlace si `P` es `static` o si `P` es de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="212c8-485">A binding-time error occurs if `P` is `static` or if `P` is read-only.</span></span> | 
| <span data-ttu-id="212c8-486">Acceso a eventos</span><span class="sxs-lookup"><span data-stu-id="212c8-486">Event access</span></span>      | `E += value`   | <span data-ttu-id="212c8-487">Se invoca al descriptor de acceso `add` del evento `E` en la clase o estructura contenedora.</span><span class="sxs-lookup"><span data-stu-id="212c8-487">The `add` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="212c8-488">Si `E` no es static, la expresión de instancia es `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-488">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `E -= value`   | <span data-ttu-id="212c8-489">Se invoca al descriptor de acceso `remove` del evento `E` en la clase o estructura contenedora.</span><span class="sxs-lookup"><span data-stu-id="212c8-489">The `remove` accessor of the event `E` in the containing class or struct is invoked.</span></span> <span data-ttu-id="212c8-490">Si `E` no es static, la expresión de instancia es `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-490">If `E` is not static, the instance expression is `this`.</span></span> | 
|                   | `T.E += value` | <span data-ttu-id="212c8-491">Se invoca al descriptor de acceso `add` del evento `E` en la clase o struct `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-491">The `add` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="212c8-492">Se produce un error en tiempo de enlace si `E` no es estático.</span><span class="sxs-lookup"><span data-stu-id="212c8-492">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `T.E -= value` | <span data-ttu-id="212c8-493">Se invoca al descriptor de acceso `remove` del evento `E` en la clase o struct `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-493">The `remove` accessor of the event `E` in the class or struct `T` is invoked.</span></span> <span data-ttu-id="212c8-494">Se produce un error en tiempo de enlace si `E` no es estático.</span><span class="sxs-lookup"><span data-stu-id="212c8-494">A binding-time error occurs if `E` is not static.</span></span> | 
|                   | `e.E += value` | <span data-ttu-id="212c8-495">El descriptor de acceso `add` del evento `E` en la clase, estructura o interfaz proporcionada por el tipo de `e` se invoca con la expresión de instancia `e`.</span><span class="sxs-lookup"><span data-stu-id="212c8-495">The `add` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="212c8-496">Se produce un error en tiempo de enlace si `E` es estático.</span><span class="sxs-lookup"><span data-stu-id="212c8-496">A binding-time error occurs if `E` is static.</span></span> | 
|                   | `e.E -= value` | <span data-ttu-id="212c8-497">El descriptor de acceso `remove` del evento `E` en la clase, estructura o interfaz proporcionada por el tipo de `e` se invoca con la expresión de instancia `e`.</span><span class="sxs-lookup"><span data-stu-id="212c8-497">The `remove` accessor of the event `E` in the class, struct, or interface given by the type of `e` is invoked with the instance expression `e`.</span></span> <span data-ttu-id="212c8-498">Se produce un error en tiempo de enlace si `E` es estático.</span><span class="sxs-lookup"><span data-stu-id="212c8-498">A binding-time error occurs if `E` is static.</span></span> | 
| <span data-ttu-id="212c8-499">Acceso a indizador</span><span class="sxs-lookup"><span data-stu-id="212c8-499">Indexer access</span></span>    | `e[x,y]`       | <span data-ttu-id="212c8-500">La resolución de sobrecarga se aplica para seleccionar el mejor indexador en la clase, estructura o interfaz dada por el tipo de e.</span><span class="sxs-lookup"><span data-stu-id="212c8-500">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of e.</span></span> <span data-ttu-id="212c8-501">El descriptor de acceso `get` del indexador se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-501">The `get` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y)`.</span></span> <span data-ttu-id="212c8-502">Se produce un error en tiempo de enlace si el indizador es de solo escritura.</span><span class="sxs-lookup"><span data-stu-id="212c8-502">A binding-time error occurs if the indexer is write-only.</span></span> | 
|                   | `e[x,y] = value` | <span data-ttu-id="212c8-503">La resolución de sobrecarga se aplica para seleccionar el mejor indexador en la clase, estructura o interfaz que proporciona el tipo de `e`.</span><span class="sxs-lookup"><span data-stu-id="212c8-503">Overload resolution is applied to select the best indexer in the class, struct, or interface given by the type of `e`.</span></span> <span data-ttu-id="212c8-504">El descriptor de acceso `set` del indexador se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y,value)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-504">The `set` accessor of the indexer is invoked with the instance expression `e` and the argument list `(x,y,value)`.</span></span> <span data-ttu-id="212c8-505">Se produce un error en tiempo de enlace si el indizador es de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="212c8-505">A binding-time error occurs if the indexer is read-only.</span></span> | 
| <span data-ttu-id="212c8-506">Invocación de operador</span><span class="sxs-lookup"><span data-stu-id="212c8-506">Operator invocation</span></span> | `-x`         | <span data-ttu-id="212c8-507">La resolución de sobrecarga se aplica para seleccionar el mejor operador unario en la clase o el struct proporcionado por el tipo de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-507">Overload resolution is applied to select the best unary operator in the class or struct given by the type of `x`.</span></span> <span data-ttu-id="212c8-508">El operador seleccionado se invoca con la lista de argumentos `(x)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-508">The selected operator is invoked with the argument list `(x)`.</span></span> | 
|                     | `x + y`      | <span data-ttu-id="212c8-509">La resolución de sobrecarga se aplica para seleccionar el mejor operador binario en las clases o Structs que proporcionan los tipos de `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-509">Overload resolution is applied to select the best binary operator in the classes or structs given by the types of `x` and `y`.</span></span> <span data-ttu-id="212c8-510">El operador seleccionado se invoca con la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-510">The selected operator is invoked with the argument list `(x,y)`.</span></span> | 
| <span data-ttu-id="212c8-511">Invocación del constructor de instancia</span><span class="sxs-lookup"><span data-stu-id="212c8-511">Instance constructor invocation</span></span> | `new T(x,y)` | <span data-ttu-id="212c8-512">La resolución de sobrecarga se aplica para seleccionar el mejor constructor de instancia en la clase o struct `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-512">Overload resolution is applied to select the best instance constructor in the class or struct `T`.</span></span> <span data-ttu-id="212c8-513">Se invoca el constructor de instancia con la lista de argumentos `(x,y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-513">The instance constructor is invoked with the argument list `(x,y)`.</span></span> | 

### <a name="argument-lists"></a><span data-ttu-id="212c8-514">Listas de argumentos</span><span class="sxs-lookup"><span data-stu-id="212c8-514">Argument lists</span></span>

<span data-ttu-id="212c8-515">Cada invocación de miembro de función y delegado incluye una lista de argumentos que proporciona valores reales o referencias de variable para los parámetros del miembro de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-515">Every function member and delegate invocation includes an argument list which provides actual values or variable references for the parameters of the function member.</span></span> <span data-ttu-id="212c8-516">La sintaxis para especificar la lista de argumentos de una invocación de miembros de función depende de la categoría de miembros de función:</span><span class="sxs-lookup"><span data-stu-id="212c8-516">The syntax for specifying the argument list of a function member invocation depends on the function member category:</span></span>

*  <span data-ttu-id="212c8-517">En el caso de constructores de instancia, métodos, indexadores y delegados, los argumentos se especifican como *argument_list*, como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="212c8-517">For instance constructors, methods, indexers and delegates, the arguments are specified as an *argument_list*, as described below.</span></span> <span data-ttu-id="212c8-518">En el caso de los indizadores, al invocar el descriptor de acceso `set`, la lista de argumentos incluye también la expresión especificada como operando derecho del operador de asignación.</span><span class="sxs-lookup"><span data-stu-id="212c8-518">For indexers, when invoking the `set` accessor, the argument list additionally includes the expression specified as the right operand of the assignment operator.</span></span>
*  <span data-ttu-id="212c8-519">En el caso de las propiedades, la lista de argumentos está vacía al invocar el descriptor de acceso `get` y se compone de la expresión especificada como operando derecho del operador de asignación al invocar el descriptor de acceso `set`.</span><span class="sxs-lookup"><span data-stu-id="212c8-519">For properties, the argument list is empty when invoking the `get` accessor, and consists of the expression specified as the right operand of the assignment operator when invoking the `set` accessor.</span></span>
*  <span data-ttu-id="212c8-520">En el caso de los eventos, la lista de argumentos se compone de la expresión especificada como operando derecho del operador `+=` o `-=`.</span><span class="sxs-lookup"><span data-stu-id="212c8-520">For events, the argument list consists of the expression specified as the right operand of the `+=` or `-=` operator.</span></span>
*  <span data-ttu-id="212c8-521">En el caso de los operadores definidos por el usuario, la lista de argumentos se compone del operando único del operador unario o de los dos operandos del operador binario.</span><span class="sxs-lookup"><span data-stu-id="212c8-521">For user-defined operators, the argument list consists of the single operand of the unary operator or the two operands of the binary operator.</span></span>

<span data-ttu-id="212c8-522">Los argumentos de las propiedades ([propiedades](classes.md#properties)), los eventos ([eventos](classes.md#events)) y los operadores definidos por el usuario ([operadores](classes.md#operators)) siempre se pasan como parámetros de valor ([parámetros de valor](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="212c8-522">The arguments of properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), and user-defined operators ([Operators](classes.md#operators)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)).</span></span> <span data-ttu-id="212c8-523">Los argumentos de los indizadores ([indizadores](classes.md#indexers)) siempre se pasan como parámetros de valor ([parámetros de valor](classes.md#value-parameters)) o como matrices de parámetros (matrices de[parámetros](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="212c8-523">The arguments of indexers ([Indexers](classes.md#indexers)) are always passed as value parameters ([Value parameters](classes.md#value-parameters)) or parameter arrays ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="212c8-524">Los parámetros de referencia y de salida no se admiten para estas categorías de miembros de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-524">Reference and output parameters are not supported for these categories of function members.</span></span>

<span data-ttu-id="212c8-525">Los argumentos de una invocación de constructor de instancia, método, indexador o delegado se especifican como *argument_list*:</span><span class="sxs-lookup"><span data-stu-id="212c8-525">The arguments of an instance constructor, method, indexer or delegate invocation are specified as an *argument_list*:</span></span>

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

<span data-ttu-id="212c8-526">Un *argument_list* consta de uno o más *argumentos*, separados por comas.</span><span class="sxs-lookup"><span data-stu-id="212c8-526">An *argument_list* consists of one or more *argument*s, separated by commas.</span></span> <span data-ttu-id="212c8-527">Cada argumento consta de un *argument_name* opcional seguido de un *argument_value*.</span><span class="sxs-lookup"><span data-stu-id="212c8-527">Each argument consists of an optional  *argument_name* followed by an *argument_value*.</span></span> <span data-ttu-id="212c8-528">Un *argumento* con un *argument_name* se conoce como argumento con ***nombre***, mientras que un *argumento* sin *argument_name* es un ***argumento posicional***.</span><span class="sxs-lookup"><span data-stu-id="212c8-528">An *argument* with an *argument_name* is referred to as a ***named argument***, whereas an *argument* without an *argument_name* is a ***positional argument***.</span></span> <span data-ttu-id="212c8-529">Es un error que un argumento posicional aparezca después de un argumento con nombre en un *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="212c8-529">It is an error for a positional argument to appear after a named argument in an *argument_list*.</span></span>

<span data-ttu-id="212c8-530">*Argument_value* puede adoptar uno de los siguientes formatos:</span><span class="sxs-lookup"><span data-stu-id="212c8-530">The *argument_value* can take one of the following forms:</span></span>

*  <span data-ttu-id="212c8-531">Una *expresión*, que indica que el argumento se pasa como parámetro de valor ([parámetros de valor](classes.md#value-parameters)).</span><span class="sxs-lookup"><span data-stu-id="212c8-531">An *expression*, indicating that the argument is passed as a value parameter ([Value parameters](classes.md#value-parameters)).</span></span>
*  <span data-ttu-id="212c8-532">La palabra clave `ref` seguida de una *variable_reference* ([referencias de variable](variables.md#variable-references)), que indica que el argumento se pasa como parámetro de referencia (parámetros de[referencia](classes.md#reference-parameters)).</span><span class="sxs-lookup"><span data-stu-id="212c8-532">The keyword `ref` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as a reference parameter ([Reference parameters](classes.md#reference-parameters)).</span></span> <span data-ttu-id="212c8-533">Una variable debe estar asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) antes de que se pueda pasar como un parámetro de referencia.</span><span class="sxs-lookup"><span data-stu-id="212c8-533">A variable must be definitely assigned ([Definite assignment](variables.md#definite-assignment)) before it can be passed as a reference parameter.</span></span> <span data-ttu-id="212c8-534">La palabra clave `out` seguida de una *variable_reference* ([referencias de variable](variables.md#variable-references)), que indica que el argumento se pasa como parámetro de salida (parámetros de[salida](classes.md#output-parameters)).</span><span class="sxs-lookup"><span data-stu-id="212c8-534">The keyword `out` followed by a *variable_reference* ([Variable references](variables.md#variable-references)), indicating that the argument is passed as an output parameter ([Output parameters](classes.md#output-parameters)).</span></span> <span data-ttu-id="212c8-535">Una variable se considera asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) después de una invocación de miembro de función en la que se pasa la variable como parámetro de salida.</span><span class="sxs-lookup"><span data-stu-id="212c8-535">A variable is considered definitely assigned ([Definite assignment](variables.md#definite-assignment)) following a function member invocation in which the variable is passed as an output parameter.</span></span>

#### <a name="corresponding-parameters"></a><span data-ttu-id="212c8-536">Parámetros correspondientes</span><span class="sxs-lookup"><span data-stu-id="212c8-536">Corresponding parameters</span></span>

<span data-ttu-id="212c8-537">Para cada argumento de una lista de argumentos debe haber un parámetro correspondiente en el miembro de función o el delegado que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="212c8-537">For each argument in an argument list there has to be a corresponding parameter in the function member or delegate being invoked.</span></span>

<span data-ttu-id="212c8-538">La lista de parámetros que se utiliza en lo siguiente se determina de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-538">The parameter list used in the following is determined as follows:</span></span>

*  <span data-ttu-id="212c8-539">En el caso de los métodos virtuales y los indizadores definidos en las clases, la lista de parámetros se elige de la declaración o invalidación más específica del miembro de función, empezando por el tipo estático del receptor y buscando en sus clases base.</span><span class="sxs-lookup"><span data-stu-id="212c8-539">For virtual methods and indexers defined in classes, the parameter list is picked from the most specific declaration or override of the function member, starting with the static type of the receiver, and searching through its base classes.</span></span>
*  <span data-ttu-id="212c8-540">En el caso de los métodos e indexadores de la interfaz, la lista de parámetros se selecciona como la definición más específica del miembro, empezando por el tipo de interfaz y buscando en las interfaces base.</span><span class="sxs-lookup"><span data-stu-id="212c8-540">For interface methods and indexers, the parameter list is picked form the most specific definition of the member, starting with the interface type and searching through the base interfaces.</span></span> <span data-ttu-id="212c8-541">Si no se encuentra ninguna lista de parámetros única, se construye una lista de parámetros con nombres inaccesibles y ningún parámetro opcional, de modo que las invocaciones no pueden usar parámetros con nombre u omitir argumentos opcionales.</span><span class="sxs-lookup"><span data-stu-id="212c8-541">If no unique parameter list is found, a parameter list with inaccessible names and no optional parameters is constructed, so that invocations cannot use named parameters or omit optional arguments.</span></span>
*  <span data-ttu-id="212c8-542">Para los métodos parciales, se usa la lista de parámetros de la declaración de método parcial de definición.</span><span class="sxs-lookup"><span data-stu-id="212c8-542">For partial methods, the parameter list of the defining partial method declaration is used.</span></span>
*  <span data-ttu-id="212c8-543">En el caso de todos los demás miembros de función y delegados, solo hay una lista de parámetros única, que es la que se usa.</span><span class="sxs-lookup"><span data-stu-id="212c8-543">For all other function members and delegates there is only a single parameter list, which is the one used.</span></span>

<span data-ttu-id="212c8-544">La posición de un argumento o parámetro se define como el número de argumentos o parámetros que lo preceden en la lista de argumentos o en la lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-544">The position of an argument or parameter is defined as the number of arguments or parameters preceding it in the argument list or parameter list.</span></span>

<span data-ttu-id="212c8-545">Los parámetros correspondientes para los argumentos de miembro de función se establecen de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-545">The corresponding parameters for function member arguments are established as follows:</span></span>

*  <span data-ttu-id="212c8-546">Argumentos de *argument_list* de constructores de instancia, métodos, indizadores y delegados:</span><span class="sxs-lookup"><span data-stu-id="212c8-546">Arguments in the *argument_list* of instance constructors, methods, indexers and delegates:</span></span>
    * <span data-ttu-id="212c8-547">Un argumento posicional en el que se produce un parámetro fijo en la misma posición en la lista de parámetros corresponde a ese parámetro.</span><span class="sxs-lookup"><span data-stu-id="212c8-547">A positional argument where a fixed parameter occurs at the same position in the parameter list corresponds to that parameter.</span></span>
    * <span data-ttu-id="212c8-548">Un argumento posicional de un miembro de función con una matriz de parámetros invocada en su forma normal corresponde a la matriz de parámetros, que debe aparecer en la misma posición en la lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-548">A positional argument of a function member with a parameter array invoked in its normal form corresponds to the parameter  array, which must occur at the same position in the parameter list.</span></span>
    * <span data-ttu-id="212c8-549">Argumento posicional de un miembro de función con una matriz de parámetros invocada en su forma expandida, donde no se produce ningún parámetro fijo en la misma posición en la lista de parámetros, corresponde a un elemento de la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-549">A positional argument of a function member with a parameter array invoked in its expanded form, where no fixed parameter occurs at the same position in the parameter list, corresponds to an element in the parameter array.</span></span>
    * <span data-ttu-id="212c8-550">Un argumento con nombre corresponde al parámetro con el mismo nombre en la lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-550">A named argument corresponds to the parameter of the same name in the parameter list.</span></span>
    * <span data-ttu-id="212c8-551">En el caso de los indizadores, al invocar el descriptor de acceso `set`, la expresión especificada como operando derecho del operador de asignación corresponde al parámetro `value` implícito de la declaración del descriptor de acceso `set`.</span><span class="sxs-lookup"><span data-stu-id="212c8-551">For indexers, when invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="212c8-552">En el caso de las propiedades, al invocar el descriptor de acceso `get` no hay ningún argumento.</span><span class="sxs-lookup"><span data-stu-id="212c8-552">For properties, when invoking the `get` accessor there are no arguments.</span></span> <span data-ttu-id="212c8-553">Al invocar el descriptor de acceso `set`, la expresión especificada como operando derecho del operador de asignación corresponde al parámetro `value` implícito de la declaración del descriptor de acceso `set`.</span><span class="sxs-lookup"><span data-stu-id="212c8-553">When invoking the `set` accessor, the expression specified as the right operand of the assignment operator corresponds to the implicit `value` parameter of the `set` accessor declaration.</span></span>
*  <span data-ttu-id="212c8-554">En el caso de los operadores unarios definidos por el usuario (incluidas las conversiones), el único operando corresponde al parámetro único de la declaración del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-554">For user-defined unary operators (including conversions), the single operand corresponds to the single parameter of the operator declaration.</span></span>
*  <span data-ttu-id="212c8-555">En el caso de los operadores binarios definidos por el usuario, el operando izquierdo corresponde al primer parámetro y el operando derecho se corresponde con el segundo parámetro de la declaración del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-555">For user-defined binary operators, the left operand corresponds to the first parameter, and the right operand corresponds to the second parameter of the operator declaration.</span></span>

#### <a name="run-time-evaluation-of-argument-lists"></a><span data-ttu-id="212c8-556">Evaluación en tiempo de ejecución de listas de argumentos</span><span class="sxs-lookup"><span data-stu-id="212c8-556">Run-time evaluation of argument lists</span></span>

<span data-ttu-id="212c8-557">Durante el procesamiento en tiempo de ejecución de una invocación de miembro de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), las expresiones o referencias de variables de una lista de argumentos se evalúan en orden, de izquierda a derecha, de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-557">During the run-time processing of a function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), the expressions or variable references of an argument list are evaluated in order, from left to right, as follows:</span></span>

*  <span data-ttu-id="212c8-558">Para un parámetro de valor, se evalúa la expresión de argumento y se realiza una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de parámetro correspondiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-558">For a value parameter, the argument expression is evaluated and an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to the corresponding parameter type is performed.</span></span> <span data-ttu-id="212c8-559">El valor resultante se convierte en el valor inicial del parámetro de valor en la invocación del miembro de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-559">The resulting value becomes the initial value of the value parameter in the function member invocation.</span></span>
*  <span data-ttu-id="212c8-560">Para un parámetro de referencia o de salida, se evalúa la referencia de la variable y la ubicación de almacenamiento resultante se convierte en la ubicación de almacenamiento representada por el parámetro en la invocación del miembro de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-560">For a reference or output parameter, the variable reference is evaluated and the resulting storage location becomes the storage location represented by the parameter in the function member invocation.</span></span> <span data-ttu-id="212c8-561">Si la referencia de variable dada como parámetro de referencia o de salida es un elemento de matriz de un *reference_type*, se realiza una comprobación en tiempo de ejecución para asegurarse de que el tipo de elemento de la matriz es idéntico al tipo del parámetro.</span><span class="sxs-lookup"><span data-stu-id="212c8-561">If the variable reference given as a reference or output parameter is an array element of a *reference_type*, a run-time check is performed to ensure that the element type of the array is identical to the type of the parameter.</span></span> <span data-ttu-id="212c8-562">Si se produce un error en esta comprobación, se produce una `System.ArrayTypeMismatchException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-562">If this check fails, a `System.ArrayTypeMismatchException` is thrown.</span></span>

<span data-ttu-id="212c8-563">Los métodos, indizadores y constructores de instancias pueden declarar su parámetro situado más a la derecha para que sea una matriz de parámetros ([matrices de parámetros](classes.md#parameter-arrays)).</span><span class="sxs-lookup"><span data-stu-id="212c8-563">Methods, indexers, and instance constructors may declare their right-most parameter to be a parameter array ([Parameter arrays](classes.md#parameter-arrays)).</span></span> <span data-ttu-id="212c8-564">Estos miembros de función se invocan en su forma normal o en su forma expandida, en función de cuál sea aplicable ([miembro de función aplicable](expressions.md#applicable-function-member)):</span><span class="sxs-lookup"><span data-stu-id="212c8-564">Such function members are invoked either in their normal form or in their expanded form depending on which is applicable ([Applicable function member](expressions.md#applicable-function-member)):</span></span>

*  <span data-ttu-id="212c8-565">Cuando se invoca un miembro de función con una matriz de parámetros en su forma normal, el argumento dado para la matriz de parámetros debe ser una expresión única que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) al tipo de matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-565">When a function member with a parameter array is invoked in its normal form, the argument given for the parameter array must be a single expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the parameter array type.</span></span> <span data-ttu-id="212c8-566">En este caso, la matriz de parámetros actúa exactamente como un parámetro de valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-566">In this case, the parameter array acts precisely like a value parameter.</span></span>
*  <span data-ttu-id="212c8-567">Cuando un miembro de función con una matriz de parámetros se invoca en su forma expandida, la invocación debe especificar cero o más argumentos posicionales para la matriz de parámetros, donde cada argumento es una expresión que se pueda convertir implícitamente ([conversiones implícitas ](conversions.md#implicit-conversions)) al tipo de elemento de la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-567">When a function member with a parameter array is invoked in its expanded form, the invocation must specify zero or more positional arguments for the parameter array, where each argument is an expression that is implicitly convertible ([Implicit conversions](conversions.md#implicit-conversions)) to the element type of the parameter array.</span></span> <span data-ttu-id="212c8-568">En este caso, la invocación crea una instancia del tipo de matriz de parámetros con una longitud que corresponde al número de argumentos, inicializa los elementos de la instancia de la matriz con los valores de argumento especificados y usa la instancia de matriz recién creada como el actual. argument.</span><span class="sxs-lookup"><span data-stu-id="212c8-568">In this case, the invocation creates an instance of the parameter array type with a length corresponding to the number of arguments, initializes the elements of the array instance with the given argument values, and uses the newly created array instance as the actual argument.</span></span>

<span data-ttu-id="212c8-569">Las expresiones de una lista de argumentos siempre se evalúan en el orden en que se escriben.</span><span class="sxs-lookup"><span data-stu-id="212c8-569">The expressions of an argument list are always evaluated in the order they are written.</span></span> <span data-ttu-id="212c8-570">Por lo tanto, el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-570">Thus, the example</span></span>
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
<span data-ttu-id="212c8-571">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="212c8-571">produces the output</span></span>
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

<span data-ttu-id="212c8-572">Las reglas de covarianza de matriz ([covarianza de matriz](arrays.md#array-covariance)) permiten que un valor de un tipo de matriz `A[]` sea una referencia a una instancia de un tipo de matriz `B[]`, siempre que exista una conversión de referencia implícita de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="212c8-572">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="212c8-573">Debido a estas reglas, cuando se pasa un elemento de matriz de un *reference_type* como parámetro de referencia o de salida, se requiere una comprobación en tiempo de ejecución para asegurarse de que el tipo de elemento real de la matriz es idéntico al del parámetro.</span><span class="sxs-lookup"><span data-stu-id="212c8-573">Because of these rules, when an array element of a *reference_type* is passed as a reference or output parameter, a run-time check is required to ensure that the actual element type of the array is identical to that of the parameter.</span></span> <span data-ttu-id="212c8-574">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-574">In the example</span></span>
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
<span data-ttu-id="212c8-575">la segunda invocación de `F` hace que se inicie una `System.ArrayTypeMismatchException` porque el tipo de elemento real de `b` es `string` y no `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-575">the second invocation of `F` causes a `System.ArrayTypeMismatchException` to be thrown because the actual element type of `b` is `string` and not `object`.</span></span>

<span data-ttu-id="212c8-576">Cuando se invoca un miembro de función con una matriz de parámetros en su forma expandida, la invocación se procesa exactamente como si se hubiera insertado una expresión de creación de matriz con un inicializador de matriz ([expresiones de creación de matriz](expressions.md#array-creation-expressions)) alrededor de los parámetros expandidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-576">When a function member with a parameter array is invoked in its expanded form, the invocation is processed exactly as if an array creation expression with an array initializer ([Array creation expressions](expressions.md#array-creation-expressions)) was inserted around the expanded parameters.</span></span> <span data-ttu-id="212c8-577">Por ejemplo, dada la declaración</span><span class="sxs-lookup"><span data-stu-id="212c8-577">For example, given the declaration</span></span>
```csharp
void F(int x, int y, params object[] args);
```
<span data-ttu-id="212c8-578">las siguientes invocaciones de la forma expandida del método</span><span class="sxs-lookup"><span data-stu-id="212c8-578">the following invocations of the expanded form of the method</span></span>
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
<span data-ttu-id="212c8-579">se corresponde exactamente con</span><span class="sxs-lookup"><span data-stu-id="212c8-579">correspond exactly to</span></span>
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

<span data-ttu-id="212c8-580">En concreto, tenga en cuenta que se crea una matriz vacía cuando no hay ningún argumento dado para la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-580">In particular, note that an empty array is created when there are zero arguments given for the parameter array.</span></span>

<span data-ttu-id="212c8-581">Cuando se omiten los argumentos de un miembro de función con los parámetros opcionales correspondientes, se pasan implícitamente los argumentos predeterminados de la declaración de miembro de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-581">When arguments are omitted from a function member with corresponding optional parameters, the default arguments of the function member declaration are implicitly passed.</span></span> <span data-ttu-id="212c8-582">Dado que siempre son constantes, su evaluación no afectará al orden de evaluación de los argumentos restantes.</span><span class="sxs-lookup"><span data-stu-id="212c8-582">Because these are always constant, their evaluation will not impact the evaluation order of the remaining arguments.</span></span>

### <a name="type-inference"></a><span data-ttu-id="212c8-583">Inferencia de tipos</span><span class="sxs-lookup"><span data-stu-id="212c8-583">Type inference</span></span>

<span data-ttu-id="212c8-584">Cuando se llama a un método genérico sin especificar argumentos de tipo, un proceso de ***inferencia de tipos*** intenta deducir los argumentos de tipo de la llamada.</span><span class="sxs-lookup"><span data-stu-id="212c8-584">When a generic method is called without specifying type arguments, a ***type inference*** process attempts to infer type arguments for the call.</span></span> <span data-ttu-id="212c8-585">La presencia de la inferencia de tipos permite usar una sintaxis más cómoda para llamar a un método genérico y permite al programador evitar especificar información de tipos redundantes.</span><span class="sxs-lookup"><span data-stu-id="212c8-585">The presence of type inference allows a more convenient syntax to be used for calling a generic method, and allows the programmer to avoid specifying redundant type information.</span></span> <span data-ttu-id="212c8-586">Por ejemplo, dada la declaración del método:</span><span class="sxs-lookup"><span data-stu-id="212c8-586">For example, given the method declaration:</span></span>
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
<span data-ttu-id="212c8-587">es posible invocar el método `Choose` sin especificar explícitamente un argumento de tipo:</span><span class="sxs-lookup"><span data-stu-id="212c8-587">it is possible to invoke the `Choose` method without explicitly specifying a type argument:</span></span>
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

<span data-ttu-id="212c8-588">A través de la inferencia de tipos, los argumentos de tipo `int` y `string` se determinan a partir de los argumentos del método.</span><span class="sxs-lookup"><span data-stu-id="212c8-588">Through type inference, the type arguments `int` and `string` are determined from the arguments to the method.</span></span>

<span data-ttu-id="212c8-589">La inferencia de tipos se produce como parte del procesamiento en tiempo de enlace de una invocación de método ([invocaciones de método](expressions.md#method-invocations)) y tiene lugar antes del paso de resolución de sobrecarga de la invocación.</span><span class="sxs-lookup"><span data-stu-id="212c8-589">Type inference occurs as part of the binding-time processing of a method invocation ([Method invocations](expressions.md#method-invocations)) and takes place before the overload resolution step of the invocation.</span></span> <span data-ttu-id="212c8-590">Cuando se especifica un grupo de métodos determinado en una invocación de método y no se especifica ningún argumento de tipo como parte de la invocación del método, se aplica la inferencia de tipos a cada método genérico del grupo de métodos.</span><span class="sxs-lookup"><span data-stu-id="212c8-590">When a particular method group is specified in a method invocation, and no type arguments are specified as part of the method invocation, type inference is applied to each generic method in the method group.</span></span> <span data-ttu-id="212c8-591">Si la inferencia de tipos se realiza correctamente, los argumentos de tipo deducido se usan para determinar los tipos de argumentos para la resolución de sobrecarga subsiguiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-591">If type inference succeeds, then the inferred type arguments are used to determine the types of arguments for subsequent overload resolution.</span></span> <span data-ttu-id="212c8-592">Si la resolución de sobrecarga elige un método genérico como el que se va a invocar, los argumentos de tipo deducido se usan como argumentos de tipo reales para la invocación.</span><span class="sxs-lookup"><span data-stu-id="212c8-592">If overload resolution chooses a generic method as the one to invoke, then the inferred type arguments are used as the actual type arguments for the invocation.</span></span> <span data-ttu-id="212c8-593">Si se produce un error en la inferencia de tipos para un método determinado, ese método no participa en la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="212c8-593">If type inference for a particular method fails, that method does not participate in overload resolution.</span></span> <span data-ttu-id="212c8-594">El error de inferencia de tipos, en y de sí mismo, no produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-594">The failure of type inference, in and of itself, does not cause a binding-time error.</span></span> <span data-ttu-id="212c8-595">Sin embargo, a menudo se produce un error en tiempo de enlace cuando la resolución de sobrecarga no encuentra ningún método aplicable.</span><span class="sxs-lookup"><span data-stu-id="212c8-595">However, it often leads to a binding-time error when overload resolution then fails to find any applicable methods.</span></span>

<span data-ttu-id="212c8-596">Si el número de argumentos proporcionado es diferente del número de parámetros del método, se producirá un error inmediatamente en la inferencia.</span><span class="sxs-lookup"><span data-stu-id="212c8-596">If the supplied number of arguments is different than the number of parameters in the method, then inference immediately fails.</span></span> <span data-ttu-id="212c8-597">En caso contrario, supongamos que el método genérico tiene la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="212c8-597">Otherwise, assume that the generic method has the following signature:</span></span>
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

<span data-ttu-id="212c8-598">Con una llamada al método con el formato `M(E1...Em)`, la tarea de inferencia de tipos es buscar argumentos de tipo únicos `S1...Sn` para cada uno de los parámetros de tipo `X1...Xn` para que la llamada `M<S1...Sn>(E1...Em)` sea válida.</span><span class="sxs-lookup"><span data-stu-id="212c8-598">With a method call of the form `M(E1...Em)` the task of type inference is to find unique type arguments `S1...Sn` for each of the type parameters `X1...Xn` so that the call `M<S1...Sn>(E1...Em)` becomes valid.</span></span>

<span data-ttu-id="212c8-599">Durante el proceso de inferencia, cada parámetro de tipo `Xi` se *fija* en un tipo determinado `Si` o sin *corregir* con un conjunto de *límites*asociado.</span><span class="sxs-lookup"><span data-stu-id="212c8-599">During the process of inference each type parameter `Xi` is either *fixed* to a particular type `Si` or *unfixed* with an associated set of *bounds*.</span></span> <span data-ttu-id="212c8-600">Cada uno de los límites es algún tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-600">Each of the bounds is some type `T`.</span></span> <span data-ttu-id="212c8-601">Inicialmente, cada variable de tipo `Xi` se desfija con un conjunto vacío de límites.</span><span class="sxs-lookup"><span data-stu-id="212c8-601">Initially each type variable `Xi` is unfixed with an empty set of bounds.</span></span>

<span data-ttu-id="212c8-602">La inferencia de tipos tiene lugar en fases.</span><span class="sxs-lookup"><span data-stu-id="212c8-602">Type inference takes place in phases.</span></span> <span data-ttu-id="212c8-603">Cada fase intentará deducir los argumentos de tipo para obtener más variables de tipo basadas en los resultados de la fase anterior.</span><span class="sxs-lookup"><span data-stu-id="212c8-603">Each phase will try to infer type arguments for more type variables based on the findings of the previous phase.</span></span> <span data-ttu-id="212c8-604">La primera fase realiza algunas inferencias iniciales de límites, mientras que en la segunda fase se corrigen las variables de tipo a tipos específicos y se deducen más límites.</span><span class="sxs-lookup"><span data-stu-id="212c8-604">The first phase makes some initial inferences of bounds, whereas the second phase fixes type variables to specific types and infers further bounds.</span></span> <span data-ttu-id="212c8-605">Es posible que la segunda fase se repita varias veces.</span><span class="sxs-lookup"><span data-stu-id="212c8-605">The second phase may have to be repeated a number of times.</span></span>

<span data-ttu-id="212c8-606">*Nota:* La inferencia de tipos no solo tiene lugar cuando se llama a un método genérico.</span><span class="sxs-lookup"><span data-stu-id="212c8-606">*Note:* Type inference takes place not only when a generic method is called.</span></span> <span data-ttu-id="212c8-607">La inferencia de tipos para la conversión de grupos de métodos se describe en [inferencia de tipos para la conversión de grupos de métodos](expressions.md#type-inference-for-conversion-of-method-groups) y encontrar el mejor tipo común de un conjunto de expresiones se describe en [Buscar el mejor tipo común de un conjunto de expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span><span class="sxs-lookup"><span data-stu-id="212c8-607">Type inference for conversion of method groups is described in [Type inference for conversion of method groups](expressions.md#type-inference-for-conversion-of-method-groups) and finding the best common type of a set of expressions is described in [Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).</span></span>

#### <a name="the-first-phase"></a><span data-ttu-id="212c8-608">La primera fase</span><span class="sxs-lookup"><span data-stu-id="212c8-608">The first phase</span></span>

<span data-ttu-id="212c8-609">Para cada uno de los argumentos de método `Ei`:</span><span class="sxs-lookup"><span data-stu-id="212c8-609">For each of the method arguments `Ei`:</span></span>

*   <span data-ttu-id="212c8-610">Si `Ei` es una función anónima, se realiza una *inferencia de tipo de parámetro explícita* ([inferencias de tipos de parámetros explícitos](expressions.md#explicit-parameter-type-inferences)) de `Ei` a `Ti`.</span><span class="sxs-lookup"><span data-stu-id="212c8-610">If `Ei` is an anonymous function, an *explicit parameter type inference* ([Explicit parameter type inferences](expressions.md#explicit-parameter-type-inferences)) is made from `Ei` to `Ti`</span></span>
*   <span data-ttu-id="212c8-611">De lo contrario, si `Ei` tiene un tipo `U` y `xi` es un parámetro de valor, se establece una *inferencia de límite inferior* *entre* @no__t *-5 y* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="212c8-611">Otherwise, if `Ei` has a type `U` and `xi` is a value parameter then a *lower-bound inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="212c8-612">De lo contrario, si `Ei` tiene un tipo `U` y `xi` es un parámetro `ref` o `out`, se realiza una *inferencia exacta* *de* `U` *a* `Ti`.</span><span class="sxs-lookup"><span data-stu-id="212c8-612">Otherwise, if `Ei` has a type `U` and `xi` is a `ref` or `out` parameter then an *exact inference* is made *from* `U` *to* `Ti`.</span></span>
*   <span data-ttu-id="212c8-613">De lo contrario, no se realiza ninguna inferencia para este argumento.</span><span class="sxs-lookup"><span data-stu-id="212c8-613">Otherwise, no inference is made for this argument.</span></span>


#### <a name="the-second-phase"></a><span data-ttu-id="212c8-614">La segunda fase</span><span class="sxs-lookup"><span data-stu-id="212c8-614">The second phase</span></span>

<span data-ttu-id="212c8-615">La segunda fase continúa como sigue:</span><span class="sxs-lookup"><span data-stu-id="212c8-615">The second phase proceeds as follows:</span></span>

*   <span data-ttu-id="212c8-616">Todas las variables de tipo sin *corregir* `Xi` que no *dependen de* ([dependencia](expressions.md#dependence)) cualquier `Xj` son fijas ([corrigiendo](expressions.md#fixing)).</span><span class="sxs-lookup"><span data-stu-id="212c8-616">All *unfixed* type variables `Xi` which do not *depend on* ([Dependence](expressions.md#dependence)) any `Xj` are fixed ([Fixing](expressions.md#fixing)).</span></span>
*   <span data-ttu-id="212c8-617">Si no existen estas variables de tipo, todas las variables de tipo sin *corregir* `Xi` son *fijas* para las que se mantienen:</span><span class="sxs-lookup"><span data-stu-id="212c8-617">If no such type variables exist, all *unfixed* type variables `Xi` are *fixed* for which all of the following hold:</span></span>
    *   <span data-ttu-id="212c8-618">Hay al menos una variable de tipo `Xj` que depende de `Xi`</span><span class="sxs-lookup"><span data-stu-id="212c8-618">There is at least one type variable `Xj` that depends on `Xi`</span></span>
    *   <span data-ttu-id="212c8-619">`Xi` tiene un conjunto de límites no vacío</span><span class="sxs-lookup"><span data-stu-id="212c8-619">`Xi` has a non-empty set of bounds</span></span>
*   <span data-ttu-id="212c8-620">Si no existe ninguna variable de tipo y hay variables de tipo sin *corregir* , se produce un error en la inferencia de tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-620">If no such type variables exist and there are still *unfixed* type variables, type inference fails.</span></span>
*   <span data-ttu-id="212c8-621">De lo contrario, si no existen más variables de tipo sin *corregir* , la inferencia de tipos se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-621">Otherwise, if no further *unfixed* type variables exist, type inference succeeds.</span></span>
*   <span data-ttu-id="212c8-622">De lo contrario, para todos los argumentos `Ei` con el tipo de parámetro correspondiente `Ti`, donde los *tipos de salida* ([tipos de salida](expressions.md#output-types)) contienen variables de tipo no *fijas* `Xj` pero los *tipos de entrada* ([tipos de entrada](expressions.md#input-types)) no,  *la inferencia de tipo de salida* ([inferencias de tipo de salida](expressions.md#output-type-inferences)) se realiza *de* 1 *a* 3.</span><span class="sxs-lookup"><span data-stu-id="212c8-622">Otherwise, for all arguments `Ei` with corresponding parameter type `Ti` where the *output types* ([Output types](expressions.md#output-types)) contain *unfixed* type variables `Xj` but the *input types* ([Input types](expressions.md#input-types)) do not, an *output type inference* ([Output type inferences](expressions.md#output-type-inferences)) is made *from* `Ei` *to* `Ti`.</span></span> <span data-ttu-id="212c8-623">A continuación, se repite la segunda fase.</span><span class="sxs-lookup"><span data-stu-id="212c8-623">Then the second phase is repeated.</span></span>

#### <a name="input-types"></a><span data-ttu-id="212c8-624">Tipos de entrada</span><span class="sxs-lookup"><span data-stu-id="212c8-624">Input types</span></span>

<span data-ttu-id="212c8-625">Si `E` es un grupo de métodos o una función anónima con tipo implícito y `T` es un tipo de delegado o un tipo de árbol de expresión, todos los tipos de parámetro de `T` son *tipos de entrada* de `E` *con el tipo* `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-625">If `E` is a method group or implicitly typed anonymous function and `T` is a delegate type or expression tree type then all the parameter types of `T` are *input types* of `E` *with type* `T`.</span></span>

####  <a name="output-types"></a><span data-ttu-id="212c8-626">Tipos de salida</span><span class="sxs-lookup"><span data-stu-id="212c8-626">Output types</span></span>

<span data-ttu-id="212c8-627">Si `E` es un grupo de métodos o una función anónima y `T` es un tipo de delegado o un tipo de árbol de expresión, el tipo de valor devuelto de `T` es un *tipo de salida de* `E` *con el tipo* `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-627">If `E` is a method group or an anonymous function and `T` is a delegate type or expression tree type then the return type of `T` is an *output type of* `E` *with type* `T`.</span></span>

#### <a name="dependence"></a><span data-ttu-id="212c8-628">Dependencia</span><span class="sxs-lookup"><span data-stu-id="212c8-628">Dependence</span></span>

<span data-ttu-id="212c8-629">Una variable de tipo sin *corregir* `Xi` *depende directamente de* una variable de tipo sin Fixed `Xj` si para algún argumento `Ek` con el tipo `Tk` `Xj` se produce en un *tipo de entrada* de `Ek` con el tipo `Tk` y 0 se produce en una  *tipo de salida* de 2 con el tipo 3.</span><span class="sxs-lookup"><span data-stu-id="212c8-629">An *unfixed* type variable `Xi` *depends directly on* an unfixed type variable `Xj` if for some argument `Ek` with type `Tk` `Xj` occurs in an *input type* of `Ek` with type `Tk` and `Xi` occurs in an *output type* of `Ek` with type `Tk`.</span></span>

<span data-ttu-id="212c8-630">`Xj` *depende de* `Xi` si `Xj` *depende directamente de* `Xi` o si `Xi` *depende directamente de* `Xk` y `Xk` *depende de* 1.</span><span class="sxs-lookup"><span data-stu-id="212c8-630">`Xj` *depends on* `Xi` if `Xj` *depends directly on* `Xi` or if `Xi` *depends directly on* `Xk` and `Xk` *depends on* `Xj`.</span></span> <span data-ttu-id="212c8-631">Por lo tanto, "depende de" es el cierre transitivo pero no reflexivo de "depende directamente de".</span><span class="sxs-lookup"><span data-stu-id="212c8-631">Thus "depends on" is the transitive but not reflexive closure of "depends directly on".</span></span>

#### <a name="output-type-inferences"></a><span data-ttu-id="212c8-632">Inferencias de tipos de salida</span><span class="sxs-lookup"><span data-stu-id="212c8-632">Output type inferences</span></span>

<span data-ttu-id="212c8-633">Una *inferencia de tipo de salida* se realiza *desde* una expresión `E` *a* un tipo `T` de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-633">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="212c8-634">Si `E` es una función anónima con el tipo de valor devuelto inferido `U` ([tipo de valor devuelto deducido](expressions.md#inferred-return-type)) y `T` es un tipo de delegado o un tipo de árbol de expresión con el tipo de valor devuelto `Tb`, una *inferencia de límite inferior* ([inferencias de límite inferior](expressions.md#lower-bound-inferences)) se realiza *desde* `U` *a* 0.</span><span class="sxs-lookup"><span data-stu-id="212c8-634">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="212c8-635">De lo contrario, si `E` es un grupo de métodos y `T` es un tipo de delegado o un tipo de árbol de expresión con tipos de parámetro `T1...Tk` y el tipo de valor devuelto `Tb`, y la resolución de sobrecarga de `E` con los tipos `T1...Tk` produce un único método con el tipo de valor devuelto `U` , una *inferencia de límite inferior* se realiza *de* `U` *a* 1.</span><span class="sxs-lookup"><span data-stu-id="212c8-635">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
*  <span data-ttu-id="212c8-636">De lo contrario, si `E` es una expresión con el tipo `U`, se realiza una *inferencia de enlace inferior* *entre* `U` *y* `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-636">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
*  <span data-ttu-id="212c8-637">De lo contrario, no se realiza ninguna inferencia.</span><span class="sxs-lookup"><span data-stu-id="212c8-637">Otherwise, no inferences are made.</span></span>

#### <a name="explicit-parameter-type-inferences"></a><span data-ttu-id="212c8-638">Inferencias explícitas de tipos de parámetros</span><span class="sxs-lookup"><span data-stu-id="212c8-638">Explicit parameter type inferences</span></span>

<span data-ttu-id="212c8-639">Una *inferencia de tipo de parámetro explícita* se realiza a *partir de* una expresión `E` *a* un tipo `T` de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-639">An *explicit parameter type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>

*  <span data-ttu-id="212c8-640">Si `E` es una función anónima con tipo explícito con tipos de parámetro `U1...Uk` y `T` es un tipo de delegado o un tipo de árbol de expresión con tipos de parámetro `V1...Vk`, para cada `Ui` se realiza una *inferencia exacta* ([inferencias exactas](expressions.md#exact-inferences)) *desde* `Ui`  *hasta* el `Vi` correspondiente</span><span class="sxs-lookup"><span data-stu-id="212c8-640">If `E` is an explicitly typed anonymous function with parameter types `U1...Uk` and `T` is a delegate type or expression tree type with parameter types `V1...Vk` then for each `Ui` an *exact inference* ([Exact inferences](expressions.md#exact-inferences)) is made *from* `Ui` *to* the corresponding `Vi`.</span></span>

#### <a name="exact-inferences"></a><span data-ttu-id="212c8-641">Inferencias exactas</span><span class="sxs-lookup"><span data-stu-id="212c8-641">Exact inferences</span></span>

<span data-ttu-id="212c8-642">Una *inferencia exacta* *de* un tipo `U` *a* un tipo `V` se realiza de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-642">An *exact inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="212c8-643">Si `V` es uno de los @no__t *fijos* -2, se agrega `U` al conjunto de límites exactos para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="212c8-643">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of exact bounds for `Xi`.</span></span>

*  <span data-ttu-id="212c8-644">De lo contrario, establece `V1...Vk` y `U1...Uk` se determinan comprobando si se aplica cualquiera de los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-644">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>

   *  <span data-ttu-id="212c8-645">`V` es un tipo de matriz `V1[...]` y `U` es un tipo de matriz `U1[...]` del mismo rango</span><span class="sxs-lookup"><span data-stu-id="212c8-645">`V` is an array type `V1[...]` and `U` is an array type `U1[...]`  of the same rank</span></span>
   *  <span data-ttu-id="212c8-646">`V` es el tipo `V1?` y `U` es el tipo `U1?`</span><span class="sxs-lookup"><span data-stu-id="212c8-646">`V` is the type `V1?` and `U` is the type `U1?`</span></span>
   *  <span data-ttu-id="212c8-647">`V` es un tipo construido `C<V1...Vk>`and `U` es un tipo construido `C<U1...Uk>`</span><span class="sxs-lookup"><span data-stu-id="212c8-647">`V` is a constructed type `C<V1...Vk>`and `U` is a constructed type `C<U1...Uk>`</span></span>

   <span data-ttu-id="212c8-648">Si se aplica cualquiera de estos casos, se realiza una *inferencia exacta* *desde* cada `Ui` *hasta* el `Vi` correspondiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-648">If any of these cases apply then an *exact inference* is made *from* each `Ui` *to* the corresponding `Vi`.</span></span>

*  <span data-ttu-id="212c8-649">En caso contrario, no se realiza ninguna inferencia.</span><span class="sxs-lookup"><span data-stu-id="212c8-649">Otherwise no inferences are made.</span></span>

#### <a name="lower-bound-inferences"></a><span data-ttu-id="212c8-650">Inferencias con límite inferior</span><span class="sxs-lookup"><span data-stu-id="212c8-650">Lower-bound inferences</span></span>

<span data-ttu-id="212c8-651">Una *inferencia de límite inferior* *desde* un tipo `U` *a* un tipo `V` se realiza de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-651">A *lower-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="212c8-652">Si `V` es uno de los @no__t *fijos* -2, `U` se agrega al conjunto de límites inferiores para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="212c8-652">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of lower bounds for `Xi`.</span></span>
*  <span data-ttu-id="212c8-653">De lo contrario, si `V` es el tipo `V1?`and `U` es el tipo `U1?`, se realiza una inferencia de límite inferior entre `U1` y `V1`.</span><span class="sxs-lookup"><span data-stu-id="212c8-653">Otherwise, if `V` is the type `V1?`and `U` is the type `U1?` then a lower bound inference is made from `U1` to `V1`.</span></span>
*  <span data-ttu-id="212c8-654">De lo contrario, establece `U1...Uk` y `V1...Vk` se determinan comprobando si se aplica cualquiera de los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-654">Otherwise, sets `U1...Uk` and `V1...Vk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="212c8-655">`V` es un tipo de matriz `V1[...]` y `U` es un tipo de matriz `U1[...]` (o un parámetro de tipo cuyo tipo base efectivo es `U1[...]`) del mismo rango</span><span class="sxs-lookup"><span data-stu-id="212c8-655">`V` is an array type `V1[...]` and `U` is an array type `U1[...]` (or a type parameter whose effective base type is `U1[...]`) of the same rank</span></span>
   *  <span data-ttu-id="212c8-656">`V` es uno de `IEnumerable<V1>`, `ICollection<V1>` o `IList<V1>` y `U` es un tipo de matriz unidimensional `U1[]` (o un parámetro de tipo cuyo tipo base efectivo es `U1[]`)</span><span class="sxs-lookup"><span data-stu-id="212c8-656">`V` is one of `IEnumerable<V1>`, `ICollection<V1>` or `IList<V1>` and `U` is a one-dimensional array type `U1[]`(or a type parameter whose effective base type is `U1[]`)</span></span>
   *  <span data-ttu-id="212c8-657">`V` es un tipo de clase, struct, interfaz o delegado construido `C<V1...Vk>` y hay un tipo único @no__t 2, de modo que `U` (o, si `U` es un parámetro de tipo, su clase base efectiva o cualquier miembro de su conjunto de interfaz efectivo) es idéntico a , hereda de (directa o indirectamente) o implementa (directa o indirectamente) `C<U1...Uk>`.</span><span class="sxs-lookup"><span data-stu-id="212c8-657">`V` is a constructed class, struct, interface or delegate type `C<V1...Vk>` and there is a unique type `C<U1...Uk>` such that `U` (or, if `U` is a type parameter, its effective base class or any member of its effective interface set) is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) `C<U1...Uk>`.</span></span>

      <span data-ttu-id="212c8-658">(La restricción de unicidad significa que en la interfaz de Case `C<T> {} class U: C<X>, C<Y> {}`, no se produce ninguna inferencia al deducir de `U` a `C<T>` porque `U1` podría ser `X` o `Y`).</span><span class="sxs-lookup"><span data-stu-id="212c8-658">(The "uniqueness" restriction means that in the case interface `C<T> {} class U: C<X>, C<Y> {}`, then no inference is made when inferring from `U` to `C<T>` because `U1` could be `X` or `Y`.)</span></span>

   <span data-ttu-id="212c8-659">Si se aplica cualquiera de estos casos, se realiza una inferencia *de* cada `Ui` al `Vi` correspondiente, como *se* indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-659">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>

   *  <span data-ttu-id="212c8-660">Si no se sabe que `Ui` es un tipo de referencia, se realiza una *inferencia exacta* .</span><span class="sxs-lookup"><span data-stu-id="212c8-660">If `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="212c8-661">De lo contrario, si `U` es un tipo de matriz, se realiza una *inferencia de límite inferior* .</span><span class="sxs-lookup"><span data-stu-id="212c8-661">Otherwise, if `U` is an array type then a *lower-bound inference* is made</span></span>
   *  <span data-ttu-id="212c8-662">De lo contrario, si `V` es `C<V1...Vk>`, la inferencia depende del parámetro de tipo i-ésima de `C`:</span><span class="sxs-lookup"><span data-stu-id="212c8-662">Otherwise, if `V` is `C<V1...Vk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="212c8-663">Si es covariante, se realiza una *inferencia de límite inferior* .</span><span class="sxs-lookup"><span data-stu-id="212c8-663">If it is covariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="212c8-664">Si es contravariante, se realiza una *inferencia enlazada en el límite superior* .</span><span class="sxs-lookup"><span data-stu-id="212c8-664">If it is contravariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="212c8-665">Si es invariable, se realiza una *inferencia exacta* .</span><span class="sxs-lookup"><span data-stu-id="212c8-665">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="212c8-666">De lo contrario, no se realiza ninguna inferencia.</span><span class="sxs-lookup"><span data-stu-id="212c8-666">Otherwise, no inferences are made.</span></span>

#### <a name="upper-bound-inferences"></a><span data-ttu-id="212c8-667">Inferencias de límite superior</span><span class="sxs-lookup"><span data-stu-id="212c8-667">Upper-bound inferences</span></span>

<span data-ttu-id="212c8-668">Una *inferencia de enlace superior* *desde* un tipo `U` *a* un tipo `V` se realiza de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-668">An *upper-bound inference* *from* a type `U` *to* a type `V` is made as follows:</span></span>

*  <span data-ttu-id="212c8-669">Si `V` es uno de los @no__t *fijos* -2, se agrega `U` al conjunto de límites superiores para `Xi`.</span><span class="sxs-lookup"><span data-stu-id="212c8-669">If `V` is one of the *unfixed* `Xi` then `U` is added to the set of upper bounds for `Xi`.</span></span>
*  <span data-ttu-id="212c8-670">De lo contrario, establece `V1...Vk` y `U1...Uk` se determinan comprobando si se aplica cualquiera de los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-670">Otherwise, sets `V1...Vk` and `U1...Uk` are determined by checking if any of the following cases apply:</span></span>
   *  <span data-ttu-id="212c8-671">`U` es un tipo de matriz `U1[...]` y `V` es un tipo de matriz `V1[...]` del mismo rango</span><span class="sxs-lookup"><span data-stu-id="212c8-671">`U` is an array type `U1[...]` and `V` is an array type `V1[...]` of the same rank</span></span>
   *  <span data-ttu-id="212c8-672">`U` es uno de `IEnumerable<Ue>`, `ICollection<Ue>` o `IList<Ue>` y `V` es un tipo de matriz unidimensional `Ve[]`</span><span class="sxs-lookup"><span data-stu-id="212c8-672">`U` is one of `IEnumerable<Ue>`, `ICollection<Ue>` or `IList<Ue>` and `V` is a one-dimensional array type `Ve[]`</span></span>
   *  <span data-ttu-id="212c8-673">`U` es el tipo `U1?` y `V` es el tipo `V1?`</span><span class="sxs-lookup"><span data-stu-id="212c8-673">`U` is the type `U1?` and `V` is the type `V1?`</span></span>
   *  <span data-ttu-id="212c8-674">`U` es una clase construida, una estructura, una interfaz o un tipo de delegado `C<U1...Uk>` y `V` es una clase, estructura, interfaz o tipo de delegado que es idéntico a, hereda de (directa o indirectamente) o implementa (directa o indirectamente) un tipo único `C<V1...Vk>`</span><span class="sxs-lookup"><span data-stu-id="212c8-674">`U` is constructed class, struct, interface or delegate type `C<U1...Uk>` and `V` is a class, struct, interface or delegate type which is identical to, inherits from (directly or indirectly), or implements (directly or indirectly) a unique type `C<V1...Vk>`</span></span>

      <span data-ttu-id="212c8-675">(La restricción de unicidad significa que, si tenemos `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, no se realiza ninguna inferencia al deducir de `C<U1>` a `V<Q>`.</span><span class="sxs-lookup"><span data-stu-id="212c8-675">(The "uniqueness" restriction means that if we have `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, then no inference is made when inferring from `C<U1>` to `V<Q>`.</span></span> <span data-ttu-id="212c8-676">Las inferencias no se realizan desde `U1` a `X<Q>` o `Y<Q>`).</span><span class="sxs-lookup"><span data-stu-id="212c8-676">Inferences are not made from `U1` to either `X<Q>` or `Y<Q>`.)</span></span>

   <span data-ttu-id="212c8-677">Si se aplica cualquiera de estos casos, se realiza una inferencia *de* cada `Ui` al `Vi` correspondiente, como *se* indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-677">If any of these cases apply then an inference is made *from* each `Ui` *to* the corresponding `Vi` as follows:</span></span>
   *  <span data-ttu-id="212c8-678">Si no se sabe que `Ui` es un tipo de referencia, se realiza una *inferencia exacta* .</span><span class="sxs-lookup"><span data-stu-id="212c8-678">If  `Ui` is not known to be a reference type then an *exact inference* is made</span></span>
   *  <span data-ttu-id="212c8-679">De lo contrario, si `V` es un tipo de matriz, se realiza una *inferencia de límite superior* .</span><span class="sxs-lookup"><span data-stu-id="212c8-679">Otherwise, if `V` is an array type then an *upper-bound inference* is made</span></span>
   *  <span data-ttu-id="212c8-680">De lo contrario, si `U` es `C<U1...Uk>`, la inferencia depende del parámetro de tipo i-ésima de `C`:</span><span class="sxs-lookup"><span data-stu-id="212c8-680">Otherwise, if `U` is `C<U1...Uk>` then inference depends on the i-th type parameter of `C`:</span></span>
      *  <span data-ttu-id="212c8-681">Si es covariante, se realiza una *inferencia enlazada en el límite superior* .</span><span class="sxs-lookup"><span data-stu-id="212c8-681">If it is covariant then an *upper-bound inference* is made.</span></span>
      *  <span data-ttu-id="212c8-682">Si es contravariante, se realiza una *inferencia de límite inferior* .</span><span class="sxs-lookup"><span data-stu-id="212c8-682">If it is contravariant then a *lower-bound inference* is made.</span></span>
      *  <span data-ttu-id="212c8-683">Si es invariable, se realiza una *inferencia exacta* .</span><span class="sxs-lookup"><span data-stu-id="212c8-683">If it is invariant then an *exact inference* is made.</span></span>
*  <span data-ttu-id="212c8-684">De lo contrario, no se realiza ninguna inferencia.</span><span class="sxs-lookup"><span data-stu-id="212c8-684">Otherwise, no inferences are made.</span></span>   

#### <a name="fixing"></a><span data-ttu-id="212c8-685">Corregir</span><span class="sxs-lookup"><span data-stu-id="212c8-685">Fixing</span></span>

<span data-ttu-id="212c8-686">Una variable de tipo sin *corregir* `Xi` con un conjunto de límites se *fija* de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-686">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>

*  <span data-ttu-id="212c8-687">El conjunto de *tipos candidatos* `Uj` comienza como el conjunto de todos los tipos del conjunto de límites de `Xi`.</span><span class="sxs-lookup"><span data-stu-id="212c8-687">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
*  <span data-ttu-id="212c8-688">A continuación, examinaremos cada límite de `Xi`: Para cada @no__t de enlace exacto de `Xi`, todos los tipos `Uj` que no sean idénticos a `U` se quitan del conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="212c8-688">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="212c8-689">Para cada límite inferior `U` de `Xi` todos los tipos `Uj` a los que *no* se ha quitado una conversión implícita de `U` se quitan del conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="212c8-689">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="212c8-690">Para cada límite superior `U` de `Xi` todos los tipos `Uj` desde los que *no* se ha quitado una conversión implícita a `U` del conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="212c8-690">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
*  <span data-ttu-id="212c8-691">Si entre los tipos candidatos restantes `Uj` hay un tipo único `V` desde el que hay una conversión implícita a todos los demás tipos candidatos y, a continuación, @no__t 2 se fija en `V`.</span><span class="sxs-lookup"><span data-stu-id="212c8-691">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then `Xi` is fixed to `V`.</span></span>
*  <span data-ttu-id="212c8-692">De lo contrario, se produce un error en la inferencia de tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-692">Otherwise, type inference fails.</span></span>

#### <a name="inferred-return-type"></a><span data-ttu-id="212c8-693">Tipo de valor devuelto deducido</span><span class="sxs-lookup"><span data-stu-id="212c8-693">Inferred return type</span></span>

<span data-ttu-id="212c8-694">El tipo de valor devuelto deducido de una función anónima `F` se usa durante la inferencia de tipos y la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="212c8-694">The inferred return type of an anonymous function `F` is used during type inference and overload resolution.</span></span> <span data-ttu-id="212c8-695">El tipo de valor devuelto deducido solo se puede determinar para una función anónima en la que se conocen todos los tipos de parámetro, ya sea porque se especifican explícitamente, se proporcionan a través de una conversión de función anónima o se infieren durante la inferencia de tipos en un genérico envolvente. invocación de método.</span><span class="sxs-lookup"><span data-stu-id="212c8-695">The inferred return type can only be determined for an anonymous function where all parameter types are known, either because they are explicitly given, provided through an anonymous function conversion or inferred during type inference on an enclosing generic method invocation.</span></span>

<span data-ttu-id="212c8-696">El ***tipo de resultado deducido*** se determina de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-696">The ***inferred result type*** is determined as follows:</span></span>

*  <span data-ttu-id="212c8-697">Si el cuerpo de `F` es una *expresión* que tiene un tipo, el tipo de resultado deducido de `F` es el tipo de esa expresión.</span><span class="sxs-lookup"><span data-stu-id="212c8-697">If the body of `F` is an *expression* that has a type, then the inferred result type of `F` is the type of that expression.</span></span>
*  <span data-ttu-id="212c8-698">Si el cuerpo de `F` es un *bloque* y el conjunto de expresiones de las instrucciones `return` del bloque tiene un tipo más común `T` ([encontrar el mejor tipo común de un conjunto de expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), el tipo de resultado deducido de `F` es `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-698">If the body of `F` is a *block* and the set of expressions in the block's `return` statements has a best common type `T` ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), then the inferred result type of `F` is `T`.</span></span>
*  <span data-ttu-id="212c8-699">De lo contrario, no se puede inferir un tipo de resultado para `F`.</span><span class="sxs-lookup"><span data-stu-id="212c8-699">Otherwise, a result type cannot be inferred for `F`.</span></span>

<span data-ttu-id="212c8-700">El ***tipo de valor devuelto deducido*** se determina de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-700">The ***inferred return type*** is determined as follows:</span></span>

*  <span data-ttu-id="212c8-701">Si `F` es Async y el cuerpo de `F` es una expresión clasificada como Nothing ([clasificaciones de expresión](expressions.md#expression-classifications)) o un bloque de instrucciones en el que ninguna instrucción return tiene expresiones, el tipo de valor devuelto deducido es `System.Threading.Tasks.Task`</span><span class="sxs-lookup"><span data-stu-id="212c8-701">If `F` is async and the body of `F` is either an expression classified as nothing ([Expression classifications](expressions.md#expression-classifications)), or a statement block where no return statements have expressions, the inferred return type is `System.Threading.Tasks.Task`</span></span>
*  <span data-ttu-id="212c8-702">Si `F` es Async y tiene un tipo de resultado inferido `T`, el tipo de valor devuelto deducido es `System.Threading.Tasks.Task<T>`.</span><span class="sxs-lookup"><span data-stu-id="212c8-702">If `F` is async and has an inferred result type `T`, the inferred return type is `System.Threading.Tasks.Task<T>`.</span></span>
*  <span data-ttu-id="212c8-703">Si `F` no es asincrónico y tiene un tipo de resultado inferido `T`, el tipo de valor devuelto deducido es @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="212c8-703">If `F` is non-async and has an inferred result type `T`, the inferred return type is `T`.</span></span>
*  <span data-ttu-id="212c8-704">De lo contrario, no se puede inferir un tipo de valor devuelto para `F`.</span><span class="sxs-lookup"><span data-stu-id="212c8-704">Otherwise a return type cannot be inferred for `F`.</span></span>

<span data-ttu-id="212c8-705">Como ejemplo de inferencia de tipos que implica funciones anónimas, tenga en cuenta el método de extensión `Select` declarado en la clase `System.Linq.Enumerable`:</span><span class="sxs-lookup"><span data-stu-id="212c8-705">As an example of type inference involving anonymous functions, consider the `Select` extension method declared in the `System.Linq.Enumerable` class:</span></span>
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

<span data-ttu-id="212c8-706">Suponiendo que el espacio de nombres `System.Linq` se importó con una cláusula `using` y, dada una clase `Customer` con una propiedad `Name` de tipo `string`, se puede usar el método `Select` para seleccionar los nombres de una lista de clientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-706">Assuming the `System.Linq` namespace was imported with a `using` clause, and given a class `Customer` with a `Name` property of type `string`, the `Select` method can be used to select the names of a list of customers:</span></span>
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

<span data-ttu-id="212c8-707">La invocación del método de extensión ([invocaciones de método de extensión](expressions.md#extension-method-invocations)) de `Select` se procesa rescribiendo la invocación a una invocación de método estático:</span><span class="sxs-lookup"><span data-stu-id="212c8-707">The extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)) of `Select` is processed by rewriting the invocation to a static method invocation:</span></span>
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

<span data-ttu-id="212c8-708">Dado que los argumentos de tipo no se especificaron explícitamente, se usa la inferencia de tipos para inferir los argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-708">Since type arguments were not explicitly specified, type inference is used to infer the type arguments.</span></span> <span data-ttu-id="212c8-709">En primer lugar, el argumento `customers` está relacionado con el parámetro `source`, infiriendo a @no__t 2 `Customer`.</span><span class="sxs-lookup"><span data-stu-id="212c8-709">First, the `customers` argument is related to the `source` parameter, inferring `T` to be `Customer`.</span></span> <span data-ttu-id="212c8-710">A continuación, con el proceso de inferencia de tipos de función anónima descrito anteriormente, `c` tiene el tipo `Customer` y la expresión `c.Name` está relacionada con el tipo de valor devuelto del parámetro `selector`, infiriendo a `S` para que sea `string`.</span><span class="sxs-lookup"><span data-stu-id="212c8-710">Then, using the anonymous function type inference process described above, `c` is given type `Customer`, and the expression `c.Name` is related to the return type of the `selector` parameter, inferring `S` to be `string`.</span></span> <span data-ttu-id="212c8-711">Por lo tanto, la invocación es equivalente a</span><span class="sxs-lookup"><span data-stu-id="212c8-711">Thus, the invocation is equivalent to</span></span>
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
<span data-ttu-id="212c8-712">y el resultado es de tipo `IEnumerable<string>`.</span><span class="sxs-lookup"><span data-stu-id="212c8-712">and the result is of type `IEnumerable<string>`.</span></span>

<span data-ttu-id="212c8-713">En el ejemplo siguiente se muestra cómo la inferencia de tipos de función anónima permite la información de tipo para "fluir" entre los argumentos de una invocación de método genérico.</span><span class="sxs-lookup"><span data-stu-id="212c8-713">The following example demonstrates how anonymous function type inference allows type information to "flow" between arguments in a generic method invocation.</span></span> <span data-ttu-id="212c8-714">Dado el método:</span><span class="sxs-lookup"><span data-stu-id="212c8-714">Given the method:</span></span>
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

<span data-ttu-id="212c8-715">Inferencia de tipos para la invocación:</span><span class="sxs-lookup"><span data-stu-id="212c8-715">Type inference for the invocation:</span></span>
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
<span data-ttu-id="212c8-716">continúa de la siguiente manera: En primer lugar, el argumento `"1:15:30"` está relacionado con el parámetro `value`, infiriendo a @no__t 2 `string`.</span><span class="sxs-lookup"><span data-stu-id="212c8-716">proceeds as follows: First, the argument `"1:15:30"` is related to the `value` parameter, inferring `X` to be `string`.</span></span> <span data-ttu-id="212c8-717">A continuación, el parámetro de la primera función anónima, `s`, recibe el tipo deducido `string` y la expresión `TimeSpan.Parse(s)` se relaciona con el tipo de valor devuelto de `f1`, infiriendo a `Y` para que sea `System.TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="212c8-717">Then, the parameter of the first anonymous function, `s`, is given the inferred type `string`, and the expression `TimeSpan.Parse(s)` is related to the return type of `f1`, inferring `Y` to be `System.TimeSpan`.</span></span> <span data-ttu-id="212c8-718">Por último, el parámetro de la segunda función anónima, `t`, recibe el tipo deducido `System.TimeSpan` y la expresión `t.TotalSeconds` se relaciona con el tipo de valor devuelto de `f2`, infiriendo a `Z` para que sea `double`.</span><span class="sxs-lookup"><span data-stu-id="212c8-718">Finally, the parameter of the second anonymous function, `t`, is given the inferred type `System.TimeSpan`, and the expression `t.TotalSeconds` is related to the return type of `f2`, inferring `Z` to be `double`.</span></span> <span data-ttu-id="212c8-719">Por lo tanto, el resultado de la invocación es de tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="212c8-719">Thus, the result of the invocation is of type `double`.</span></span>

#### <a name="type-inference-for-conversion-of-method-groups"></a><span data-ttu-id="212c8-720">Inferencia de tipos para la conversión de grupos de métodos</span><span class="sxs-lookup"><span data-stu-id="212c8-720">Type inference for conversion of method groups</span></span>

<span data-ttu-id="212c8-721">De forma similar a las llamadas de métodos genéricos, la inferencia de tipos también se debe aplicar cuando un grupo de métodos `M` que contiene un método genérico se convierte en un tipo de delegado determinado `D` ([conversiones de grupo de métodos](conversions.md#method-group-conversions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-721">Similar to calls of generic methods, type inference must also be applied when a method group `M` containing a generic method is converted to a given delegate type `D` ([Method group conversions](conversions.md#method-group-conversions)).</span></span> <span data-ttu-id="212c8-722">Dado un método</span><span class="sxs-lookup"><span data-stu-id="212c8-722">Given a method</span></span>
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
<span data-ttu-id="212c8-723">y el grupo de métodos `M` que se asigna al tipo de delegado `D` la tarea de inferencia de tipo es buscar argumentos de tipo `S1...Sn` para que la expresión:</span><span class="sxs-lookup"><span data-stu-id="212c8-723">and the method group `M` being assigned to the delegate type `D` the task of type inference is to find type arguments `S1...Sn` so that the expression:</span></span>
```csharp
M<S1...Sn>
```
<span data-ttu-id="212c8-724">se convierte en compatible ([declaraciones de delegado](delegates.md#delegate-declarations)) con `D`.</span><span class="sxs-lookup"><span data-stu-id="212c8-724">becomes compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`.</span></span>

<span data-ttu-id="212c8-725">A diferencia del algoritmo de inferencia de tipos para las llamadas de método genérico, en este caso solo hay *tipos*de argumento, no hay *expresiones*de argumento.</span><span class="sxs-lookup"><span data-stu-id="212c8-725">Unlike the type inference algorithm for generic method calls, in this case there are only argument *types*, no argument *expressions*.</span></span> <span data-ttu-id="212c8-726">En concreto, no hay ninguna función anónima y, por lo tanto, no es necesario que haya varias fases de inferencia.</span><span class="sxs-lookup"><span data-stu-id="212c8-726">In particular, there are no anonymous functions and hence no need for multiple phases of inference.</span></span>

<span data-ttu-id="212c8-727">En su lugar, todos los `Xi` se consideran sin *corregir*y se realiza una *inferencia de enlace inferior* *desde* cada tipo de argumento `Uj` de `D` *al* tipo de parámetro correspondiente `Tj` de `M`.</span><span class="sxs-lookup"><span data-stu-id="212c8-727">Instead, all `Xi` are considered *unfixed*, and a *lower-bound inference* is made *from* each argument type `Uj` of `D` *to* the corresponding parameter type `Tj` of `M`.</span></span> <span data-ttu-id="212c8-728">Si no se encuentra ningún límite en el `Xi`, se produce un error en la inferencia de tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-728">If for any of the `Xi` no bounds were found, type inference fails.</span></span> <span data-ttu-id="212c8-729">De lo contrario, todos los `Xi` se *corrigen* en `Si` correspondiente, que son el resultado de la inferencia de tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-729">Otherwise, all `Xi` are *fixed* to corresponding `Si`, which are the result of type inference.</span></span>

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a><span data-ttu-id="212c8-730">Buscar el mejor tipo común de un conjunto de expresiones</span><span class="sxs-lookup"><span data-stu-id="212c8-730">Finding the best common type of a set of expressions</span></span>

<span data-ttu-id="212c8-731">En algunos casos, es necesario inferir un tipo común para un conjunto de expresiones.</span><span class="sxs-lookup"><span data-stu-id="212c8-731">In some cases, a common type needs to be inferred for a set of expressions.</span></span> <span data-ttu-id="212c8-732">En concreto, los tipos de elemento de matrices con tipo implícito y los tipos de valor devueltos de las funciones anónimas con cuerpos de *bloque* se encuentran de esta manera.</span><span class="sxs-lookup"><span data-stu-id="212c8-732">In particular, the element types of implicitly typed arrays and the return types of anonymous functions with *block* bodies are found in this way.</span></span>

<span data-ttu-id="212c8-733">Intuitivamente, dado un conjunto de expresiones `E1...Em`, esta inferencia debe ser equivalente a llamar a un método.</span><span class="sxs-lookup"><span data-stu-id="212c8-733">Intuitively, given a set of expressions `E1...Em` this inference should be equivalent to calling a method</span></span>
```csharp
Tr M<X>(X x1 ... X xm)
```
<span data-ttu-id="212c8-734">con el `Ei` como argumentos.</span><span class="sxs-lookup"><span data-stu-id="212c8-734">with the `Ei` as arguments.</span></span>

<span data-ttu-id="212c8-735">Más concretamente, la inferencia comienza con una variable de tipo sin *fixed* `X`.</span><span class="sxs-lookup"><span data-stu-id="212c8-735">More precisely, the inference starts out with an *unfixed* type variable `X`.</span></span> <span data-ttu-id="212c8-736">A continuación, se realizan *inferencias de tipos de salida* *de* cada `Ei` *a* `X`.</span><span class="sxs-lookup"><span data-stu-id="212c8-736">*Output type inferences* are then made *from* each `Ei` *to* `X`.</span></span> <span data-ttu-id="212c8-737">Por último, `X` es *fijo* y, si es correcto, el tipo resultante `S` es el mejor tipo común resultante para las expresiones.</span><span class="sxs-lookup"><span data-stu-id="212c8-737">Finally, `X` is *fixed* and, if successful, the resulting type `S` is the resulting best common type for the expressions.</span></span> <span data-ttu-id="212c8-738">Si no existe tal `S`, las expresiones no tienen el mejor tipo común.</span><span class="sxs-lookup"><span data-stu-id="212c8-738">If no such `S` exists, the expressions have no best common type.</span></span>

### <a name="overload-resolution"></a><span data-ttu-id="212c8-739">Resolución de sobrecarga</span><span class="sxs-lookup"><span data-stu-id="212c8-739">Overload resolution</span></span>

<span data-ttu-id="212c8-740">La resolución de sobrecarga es un mecanismo en tiempo de enlace para seleccionar el mejor miembro de función que se va a invocar a partir de una lista de argumentos y un conjunto de miembros de función candidatos.</span><span class="sxs-lookup"><span data-stu-id="212c8-740">Overload resolution is a binding-time mechanism for selecting the best function member to invoke given an argument list and a set of candidate function members.</span></span> <span data-ttu-id="212c8-741">La resolución de sobrecarga selecciona el miembro de función que se va a invocar en C#los siguientes contextos distintos dentro de:</span><span class="sxs-lookup"><span data-stu-id="212c8-741">Overload resolution selects the function member to invoke in the following distinct contexts within C#:</span></span>

*  <span data-ttu-id="212c8-742">Invocación de un método denominado en un *invocation_expression* ([invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="212c8-742">Invocation of a method named in an *invocation_expression* ([Method invocations](expressions.md#method-invocations)).</span></span>
*  <span data-ttu-id="212c8-743">Invocación de un constructor de instancia denominado en un *object_creation_expression* ([expresiones de creación de objetos](expressions.md#object-creation-expressions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-743">Invocation of an instance constructor named in an *object_creation_expression* ([Object creation expressions](expressions.md#object-creation-expressions)).</span></span>
*  <span data-ttu-id="212c8-744">Invocación de un descriptor de acceso de indexador a través de un *element_access* ([acceso a elementos](expressions.md#element-access)).</span><span class="sxs-lookup"><span data-stu-id="212c8-744">Invocation of an indexer accessor through an *element_access* ([Element access](expressions.md#element-access)).</span></span>
*  <span data-ttu-id="212c8-745">Invocación de un operador predefinido o definido por el usuario al que se hace referencia en una expresión (resolución de[sobrecarga del operador unario](expressions.md#unary-operator-overload-resolution) y [resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="212c8-745">Invocation of a predefined or user-defined operator referenced in an expression ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution) and [Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)).</span></span>

<span data-ttu-id="212c8-746">Cada uno de estos contextos define el conjunto de miembros de función candidatas y la lista de argumentos en su propia manera única, tal como se describe en detalle en las secciones mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="212c8-746">Each of these contexts defines the set of candidate function members and the list of arguments in its own unique way, as described in detail in the sections listed above.</span></span> <span data-ttu-id="212c8-747">Por ejemplo, el conjunto de candidatos para una invocación de método no incluye métodos marcados como `override` ([búsqueda de miembros](expressions.md#member-lookup)) y los métodos de una clase base no son candidatos si algún método de una clase derivada es aplicable ([invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="212c8-747">For example, the set of candidates for a method invocation does not include methods marked `override` ([Member lookup](expressions.md#member-lookup)), and methods in a base class are not candidates if any method in a derived class is applicable ([Method invocations](expressions.md#method-invocations)).</span></span>

<span data-ttu-id="212c8-748">Una vez identificados los miembros de la función candidata y la lista de argumentos, la selección del mejor miembro de función es la misma en todos los casos:</span><span class="sxs-lookup"><span data-stu-id="212c8-748">Once the candidate function members and the argument list have been identified, the selection of the best function member is the same in all cases:</span></span>

*  <span data-ttu-id="212c8-749">Dado el conjunto de miembros de función candidatos aplicables, se ubica el mejor miembro de función de ese conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-749">Given the set of applicable candidate function members, the best function member in that set is located.</span></span> <span data-ttu-id="212c8-750">Si el conjunto contiene solo un miembro de función, ese miembro de función es el mejor miembro de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-750">If the set contains only one function member, then that function member is the best function member.</span></span> <span data-ttu-id="212c8-751">De lo contrario, el mejor miembro de función es un miembro de función que es mejor que todos los demás miembros de función con respecto a la lista de argumentos determinada, siempre que cada miembro de función se compare con el resto de miembros de función con las reglas de [mejor función. miembro](expressions.md#better-function-member).</span><span class="sxs-lookup"><span data-stu-id="212c8-751">Otherwise, the best function member is the one function member that is better than all other function members with respect to the given argument list, provided that each function member is compared to all other function members using the rules in [Better function member](expressions.md#better-function-member).</span></span> <span data-ttu-id="212c8-752">Si no hay exactamente un miembro de función que sea mejor que todos los demás miembros de función, la invocación del miembro de función es ambigua y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-752">If there is not exactly one function member that is better than all other function members, then the function member invocation is ambiguous and a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-753">En las secciones siguientes se definen los significados exactos de los términos ***miembro de función aplicable*** y ***mejor miembro de función***.</span><span class="sxs-lookup"><span data-stu-id="212c8-753">The following sections define the exact meanings of the terms ***applicable function member*** and ***better function member***.</span></span>

#### <a name="applicable-function-member"></a><span data-ttu-id="212c8-754">Miembro de función aplicable</span><span class="sxs-lookup"><span data-stu-id="212c8-754">Applicable function member</span></span>

<span data-ttu-id="212c8-755">Se dice que un miembro de función es un ***miembro de función aplicable*** con respecto a una lista de argumentos `A` cuando se cumplen todas las condiciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-755">A function member is said to be an ***applicable function member*** with respect to an argument list `A` when all of the following are true:</span></span>

*  <span data-ttu-id="212c8-756">Cada argumento de `A` corresponde a un parámetro de la declaración de miembro de función tal y como se describe en [los parámetros correspondientes](expressions.md#corresponding-parameters), y los parámetros a los que ningún argumento corresponde es un parámetro opcional.</span><span class="sxs-lookup"><span data-stu-id="212c8-756">Each argument in `A` corresponds to a parameter in the function member declaration as described in [Corresponding parameters](expressions.md#corresponding-parameters), and any parameter to which no argument corresponds is an optional parameter.</span></span>
*  <span data-ttu-id="212c8-757">Para cada argumento de `A`, el modo de paso de parámetros del argumento (es decir, Value, `ref` o `out`) es idéntico al modo de paso de parámetros del parámetro correspondiente, y</span><span class="sxs-lookup"><span data-stu-id="212c8-757">For each argument in `A`, the parameter passing mode of the argument (i.e., value, `ref`, or `out`) is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="212c8-758">para un parámetro de valor o una matriz de parámetros, existe una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) del argumento al tipo del parámetro correspondiente, o bien</span><span class="sxs-lookup"><span data-stu-id="212c8-758">for a value parameter or a parameter array, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="212c8-759">para un parámetro `ref` o `out`, el tipo del argumento es idéntico al tipo del parámetro correspondiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-759">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span> <span data-ttu-id="212c8-760">Después de All, un parámetro `ref` o `out` es un alias para el argumento pasado.</span><span class="sxs-lookup"><span data-stu-id="212c8-760">After all, a `ref` or `out` parameter is an alias for the argument passed.</span></span>

<span data-ttu-id="212c8-761">Para un miembro de función que incluye una matriz de parámetros, si el miembro de función es aplicable a las reglas anteriores, se dice que es aplicable en su ***forma normal***.</span><span class="sxs-lookup"><span data-stu-id="212c8-761">For a function member that includes a parameter array, if the function member is applicable by the above rules, it is said to be applicable in its ***normal form***.</span></span> <span data-ttu-id="212c8-762">Si un miembro de función que incluye una matriz de parámetros no es aplicable en su forma normal, el miembro de función puede ser aplicable en su ***forma expandida***:</span><span class="sxs-lookup"><span data-stu-id="212c8-762">If a function member that includes a parameter array is not applicable in its normal form, the function member may instead be applicable in its ***expanded form***:</span></span>

*  <span data-ttu-id="212c8-763">La forma expandida se construye reemplazando la matriz de parámetros en la declaración de miembro de función con cero o más parámetros de valor del tipo de elemento de la matriz de parámetros, de modo que el número de argumentos de la lista de argumentos `A` coincide con el número total de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-763">The expanded form is constructed by replacing the parameter array in the function member declaration with zero or more value parameters of the element type of the parameter array such that the number of arguments in the argument list `A` matches the total number of parameters.</span></span> <span data-ttu-id="212c8-764">Si `A` tiene menos argumentos que el número de parámetros fijos en la declaración de miembro de función, no se puede construir la forma expandida del miembro de función y, por tanto, no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="212c8-764">If `A` has fewer arguments than the number of fixed parameters in the function member declaration, the expanded form of the function member cannot be constructed and is thus not applicable.</span></span>
*  <span data-ttu-id="212c8-765">De lo contrario, la forma expandida es aplicable si para cada argumento de `A` el modo de paso de parámetros del argumento es idéntico al modo de paso de parámetros del parámetro correspondiente, y</span><span class="sxs-lookup"><span data-stu-id="212c8-765">Otherwise, the expanded form is applicable if for each argument in `A` the parameter passing mode of the argument is identical to the parameter passing mode of the corresponding parameter, and</span></span>
   *  <span data-ttu-id="212c8-766">para un parámetro de valor fijo o un parámetro de valor creado por la expansión, existe una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo del argumento al tipo del parámetro correspondiente, o bien</span><span class="sxs-lookup"><span data-stu-id="212c8-766">for a fixed value parameter or a value parameter created by the expansion, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from the type of the argument to the type of the corresponding parameter, or</span></span>
   *  <span data-ttu-id="212c8-767">para un parámetro `ref` o `out`, el tipo del argumento es idéntico al tipo del parámetro correspondiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-767">for a `ref` or `out` parameter, the type of the argument is identical to the type of the corresponding parameter.</span></span>

#### <a name="better-function-member"></a><span data-ttu-id="212c8-768">Mejor miembro de función</span><span class="sxs-lookup"><span data-stu-id="212c8-768">Better function member</span></span>

<span data-ttu-id="212c8-769">Con el fin de determinar el mejor miembro de función, se construye una lista de argumentos con la que se ha quitado una lista que contiene solo las expresiones de argumento en el orden en que aparecen en la lista de argumentos original.</span><span class="sxs-lookup"><span data-stu-id="212c8-769">For the purposes of determining the better function member, a stripped-down argument list A is constructed containing just the argument expressions themselves in the order they appear in the original argument list.</span></span>

<span data-ttu-id="212c8-770">Las listas de parámetros para cada uno de los miembros de la función candidata se construyen de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-770">Parameter lists for each of the candidate function members are constructed in the following way:</span></span>

*  <span data-ttu-id="212c8-771">La forma expandida se utiliza si el miembro de función solo se aplica en el formulario expandido.</span><span class="sxs-lookup"><span data-stu-id="212c8-771">The expanded form is used if the function member was applicable only in the expanded form.</span></span>
*  <span data-ttu-id="212c8-772">Los parámetros opcionales sin argumentos correspondientes se quitan de la lista de parámetros</span><span class="sxs-lookup"><span data-stu-id="212c8-772">Optional parameters with no corresponding arguments are removed from the parameter list</span></span>
*  <span data-ttu-id="212c8-773">Los parámetros se reordenan para que se produzcan en la misma posición que el argumento correspondiente en la lista de argumentos.</span><span class="sxs-lookup"><span data-stu-id="212c8-773">The parameters are reordered so that they occur at the same position as the corresponding argument in the argument list.</span></span>

<span data-ttu-id="212c8-774">Dada una lista de argumentos `A` con un conjunto de expresiones de argumento `{E1, E2, ..., En}` y dos miembros de función aplicables `Mp` y `Mq` con tipos de parámetro `{P1, P2, ..., Pn}` y `{Q1, Q2, ..., Qn}`, `Mp` se define para ser un ***miembro de función mejor*** que `Mq` si</span><span class="sxs-lookup"><span data-stu-id="212c8-774">Given an argument list `A` with a set of argument expressions `{E1, E2, ..., En}` and two applicable function members `Mp` and `Mq` with parameter types `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}`, `Mp` is defined to be a ***better function member*** than `Mq` if</span></span>

*  <span data-ttu-id="212c8-775">para cada argumento, la conversión implícita de `Ex` a `Qx` no es mejor que la conversión implícita de `Ex` a `Px`, y</span><span class="sxs-lookup"><span data-stu-id="212c8-775">for each argument, the implicit conversion from `Ex` to `Qx` is not better than the implicit conversion from `Ex` to `Px`, and</span></span>
*  <span data-ttu-id="212c8-776">para al menos un argumento, la conversión de `Ex` a `Px` es mejor que la conversión de `Ex` a `Qx`.</span><span class="sxs-lookup"><span data-stu-id="212c8-776">for at least one argument, the conversion from `Ex` to `Px` is better than the conversion from `Ex` to `Qx`.</span></span>

<span data-ttu-id="212c8-777">Al realizar esta evaluación, si `Mp` o `Mq` es aplicable en su forma expandida, `Px` o `Qx` hace referencia a un parámetro en la forma expandida de la lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-777">When performing this evaluation, if `Mp` or `Mq` is applicable in its expanded form, then `Px` or `Qx` refers to a parameter in the expanded form of the parameter list.</span></span>

<span data-ttu-id="212c8-778">En el caso de que las secuencias de tipo de parámetro @ no__t-0 y `{Q1, Q2, ..., Qn}` sean equivalentes (es decir, cada `Pi` tiene una conversión de identidad en el `Qi` correspondiente), se aplican las siguientes reglas de separación de desempates, en orden, para determinar el mejor miembro de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-778">In case the parameter type sequences `{P1, P2, ..., Pn}` and `{Q1, Q2, ..., Qn}` are equivalent (i.e. each `Pi` has an identity conversion to the corresponding `Qi`), the following tie-breaking rules are applied, in order, to determine the better function member.</span></span>

*  <span data-ttu-id="212c8-779">Si `Mp` es un método no genérico y `Mq` es un método genérico, @no__t 2 es mejor que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="212c8-779">If `Mp` is a non-generic method and `Mq` is a generic method, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="212c8-780">De lo contrario, si `Mp` es aplicable en su forma normal y `Mq` tiene una matriz `params` y solo es aplicable en su forma expandida, `Mp` es mejor que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="212c8-780">Otherwise, if `Mp` is applicable in its normal form and `Mq` has a `params` array and is applicable only in its expanded form, then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="212c8-781">De lo contrario, si `Mp` tiene más parámetros declarados que `Mq`, @no__t 2 es mejor que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="212c8-781">Otherwise, if `Mp` has more declared parameters than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="212c8-782">Esto puede ocurrir si ambos métodos tienen matrices `params` y solo se aplican en sus formularios expandidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-782">This can occur if both methods have `params` arrays and are applicable only in their expanded forms.</span></span>
*  <span data-ttu-id="212c8-783">De lo contrario, si todos los parámetros de `Mp` tienen un argumento correspondiente, mientras que los argumentos predeterminados deben sustituirse por al menos un parámetro opcional en `Mq` y, a continuación, @no__t 2 es mejor que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="212c8-783">Otherwise if all parameters of `Mp` have a corresponding argument whereas default arguments need to be substituted for at least one optional parameter in `Mq` then `Mp` is better than `Mq`.</span></span>
*  <span data-ttu-id="212c8-784">De lo contrario, si `Mp` tiene tipos de parámetro más específicos que `Mq`, @no__t 2 es mejor que `Mq`.</span><span class="sxs-lookup"><span data-stu-id="212c8-784">Otherwise, if `Mp` has more specific parameter types than `Mq`, then `Mp` is better than `Mq`.</span></span> <span data-ttu-id="212c8-785">Permita que `{R1, R2, ..., Rn}` y `{S1, S2, ..., Sn}` representen los tipos de parámetro sin instancia y sin expandir de `Mp` y `Mq`.</span><span class="sxs-lookup"><span data-stu-id="212c8-785">Let `{R1, R2, ..., Rn}` and `{S1, S2, ..., Sn}` represent the uninstantiated and unexpanded parameter types of `Mp` and `Mq`.</span></span> <span data-ttu-id="212c8-786">los tipos de parámetro de `Mp` son más específicos que `Mq` si, para cada parámetro, `Rx` no es menos específico que `Sx` y, para al menos un parámetro, `Rx` es más específico que `Sx`:</span><span class="sxs-lookup"><span data-stu-id="212c8-786">`Mp`'s parameter types are more specific than `Mq`'s if, for each parameter, `Rx` is not less specific than `Sx`, and, for at least one parameter, `Rx` is more specific than `Sx`:</span></span>
   *  <span data-ttu-id="212c8-787">Un parámetro de tipo es menos específico que un parámetro sin tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-787">A type parameter is less specific than a non-type parameter.</span></span>
   *  <span data-ttu-id="212c8-788">De forma recursiva, un tipo construido es más específico que otro tipo construido (con el mismo número de argumentos de tipo) si al menos un argumento de tipo es más específico y ningún argumento de tipo es menos específico que el argumento de tipo correspondiente en el otro.</span><span class="sxs-lookup"><span data-stu-id="212c8-788">Recursively, a constructed type is more specific than another constructed type (with the same number of type arguments) if at least one type argument is more specific and no type argument is less specific than the corresponding type argument in the other.</span></span>
   *  <span data-ttu-id="212c8-789">Un tipo de matriz es más específico que otro tipo de matriz (con el mismo número de dimensiones) si el tipo de elemento del primero es más específico que el tipo de elemento del segundo.</span><span class="sxs-lookup"><span data-stu-id="212c8-789">An array type is more specific than another array type (with the same number of dimensions) if the element type of the first is more specific than the element type of the second.</span></span>
*  <span data-ttu-id="212c8-790">De lo contrario, si un miembro es un operador no de elevación y el otro es un operador de elevación, el valor de uno no elevado es mejor.</span><span class="sxs-lookup"><span data-stu-id="212c8-790">Otherwise if one member is a non-lifted operator and  the other is a lifted operator, the non-lifted one is better.</span></span>
*  <span data-ttu-id="212c8-791">De lo contrario, ninguno de los miembros de función es mejor.</span><span class="sxs-lookup"><span data-stu-id="212c8-791">Otherwise, neither function member is better.</span></span>

#### <a name="better-conversion-from-expression"></a><span data-ttu-id="212c8-792">Mejor conversión de la expresión</span><span class="sxs-lookup"><span data-stu-id="212c8-792">Better conversion from expression</span></span>

<span data-ttu-id="212c8-793">Dada una conversión implícita `C1` que convierte de una expresión `E` a un tipo `T1` y una conversión implícita `C2` que convierte de una expresión `E` a un tipo `T2`, `C1` es una ***conversión mejor*** que `C2` si @no__ t-9 no coincide exactamente con 0 y al menos una de las siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-793">Given an implicit conversion `C1` that converts from an expression `E` to a type `T1`, and an implicit conversion `C2` that converts from an expression `E` to a type `T2`, `C1` is a ***better conversion*** than `C2` if `E` does not exactly match `T2` and at least one of the following holds:</span></span>

* <span data-ttu-id="212c8-794">`E` coincide exactamente con `T1` ([expresión de coincidencia exacta](expressions.md#exactly-matching-expression))</span><span class="sxs-lookup"><span data-stu-id="212c8-794">`E` exactly matches `T1` ([Exactly matching Expression](expressions.md#exactly-matching-expression))</span></span>
* <span data-ttu-id="212c8-795">`T1` es un mejor destino de conversión que `T2` ([mejor destino](expressions.md#better-conversion-target)de la conversión)</span><span class="sxs-lookup"><span data-stu-id="212c8-795">`T1` is a better conversion target than `T2` ([Better conversion target](expressions.md#better-conversion-target))</span></span>

#### <a name="exactly-matching-expression"></a><span data-ttu-id="212c8-796">Expresión coincidente exactamente</span><span class="sxs-lookup"><span data-stu-id="212c8-796">Exactly matching Expression</span></span>

<span data-ttu-id="212c8-797">Dada una expresión `E` y un tipo `T`, `E` coincide exactamente con `T` Si uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="212c8-797">Given an expression `E` and a type `T`, `E` exactly matches `T` if one of the following holds:</span></span>

*  <span data-ttu-id="212c8-798">`E` tiene un tipo `S` y existe una conversión de identidad de `S` a `T`</span><span class="sxs-lookup"><span data-stu-id="212c8-798">`E` has a type `S`, and an identity conversion exists from `S` to `T`</span></span>
*  <span data-ttu-id="212c8-799">`E` es una función anónima, `T` es un tipo de delegado `D` o un tipo de árbol de expresión `Expression<D>` y uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-799">`E` is an anonymous function, `T` is either a delegate type `D` or an expression tree type `Expression<D>` and one of the following holds:</span></span>
   *  <span data-ttu-id="212c8-800">Un tipo de valor devuelto deducido `X` existe para `E` en el contexto de la lista de parámetros de `D` ([tipo de valor devuelto deducido](expressions.md#inferred-return-type)) y existe una conversión de identidad de `X` al tipo de valor devuelto de `D`</span><span class="sxs-lookup"><span data-stu-id="212c8-800">An inferred return type `X` exists for `E` in the context of the parameter list of `D` ([Inferred return type](expressions.md#inferred-return-type)), and an identity conversion exists from `X` to the return type of `D`</span></span>
   *  <span data-ttu-id="212c8-801">@No__t-0 no es Async y `D` tiene un tipo de valor devuelto `Y` o `E` es Async y `D` tiene un tipo de valor devuelto `Task<Y>` y uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-801">Either `E` is non-async and `D` has a return type `Y` or `E` is async and `D` has a return type `Task<Y>`, and one of the following holds:</span></span>
      * <span data-ttu-id="212c8-802">El cuerpo de `E` es una expresión que coincide exactamente con `Y`</span><span class="sxs-lookup"><span data-stu-id="212c8-802">The body of `E` is an expression that exactly matches `Y`</span></span>
      * <span data-ttu-id="212c8-803">El cuerpo de `E` es un bloque de instrucciones en el que cada instrucción return devuelve una expresión que coincide exactamente con `Y`</span><span class="sxs-lookup"><span data-stu-id="212c8-803">The body of `E` is a statement block where every return statement returns an expression that exactly matches `Y`</span></span>

#### <a name="better-conversion-target"></a><span data-ttu-id="212c8-804">Mejor destino de la conversión</span><span class="sxs-lookup"><span data-stu-id="212c8-804">Better conversion target</span></span>

<span data-ttu-id="212c8-805">Dados dos tipos diferentes `T1` y `T2`, @no__t 2 es un mejor destino de conversión que `T2` si no existe una conversión implícita de `T2` a `T1` y al menos uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-805">Given two different types `T1` and `T2`, `T1` is a better conversion target than `T2` if no implicit conversion from `T2` to `T1` exists, and at least one of the following holds:</span></span>

*  <span data-ttu-id="212c8-806">Existe una conversión implícita de `T1` a `T2`</span><span class="sxs-lookup"><span data-stu-id="212c8-806">An implicit conversion from `T1` to `T2` exists</span></span>
*  <span data-ttu-id="212c8-807">`T1` es un tipo de delegado `D1` o un tipo de árbol de expresión `Expression<D1>`, `T2` es un tipo de delegado `D2` o un tipo de árbol de expresión `Expression<D2>`, `D1` tiene un tipo de valor devuelto `S1` y uno de los siguientes :</span><span class="sxs-lookup"><span data-stu-id="212c8-807">`T1` is either a delegate type `D1` or an expression tree type `Expression<D1>`, `T2` is either a delegate type `D2` or an expression tree type `Expression<D2>`, `D1` has a return type `S1` and one of the following holds:</span></span>
   * <span data-ttu-id="212c8-808">`D2` es void devolviendo</span><span class="sxs-lookup"><span data-stu-id="212c8-808">`D2` is void returning</span></span>
   * <span data-ttu-id="212c8-809">`D2` tiene un tipo de valor devuelto `S2` y `S1` es un destino de conversión mejor que `S2`</span><span class="sxs-lookup"><span data-stu-id="212c8-809">`D2` has a return type `S2`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="212c8-810">`T1` es `Task<S1>`, @no__t 2 es `Task<S2>` y `S1` es un destino de conversión mejor que `S2`</span><span class="sxs-lookup"><span data-stu-id="212c8-810">`T1` is `Task<S1>`, `T2` is `Task<S2>`, and `S1` is a better conversion target than `S2`</span></span>
*  <span data-ttu-id="212c8-811">`T1` es `S1` o `S1?` donde `S1` es un tipo entero con signo y `T2` es `S2` o `S2?`, donde `S2` es un tipo entero sin signo.</span><span class="sxs-lookup"><span data-stu-id="212c8-811">`T1` is `S1` or `S1?` where `S1` is a signed integral type, and `T2` is `S2` or `S2?` where `S2` is an unsigned integral type.</span></span> <span data-ttu-id="212c8-812">De manera específica:</span><span class="sxs-lookup"><span data-stu-id="212c8-812">Specifically:</span></span>
   * <span data-ttu-id="212c8-813">`S1` es `sbyte` y @no__t 2 es `byte`, `ushort`, `uint` o `ulong`</span><span class="sxs-lookup"><span data-stu-id="212c8-813">`S1` is `sbyte` and `S2` is `byte`, `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="212c8-814">`S1` es `short` y `S2` es `ushort`, `uint` o `ulong`</span><span class="sxs-lookup"><span data-stu-id="212c8-814">`S1` is `short` and `S2` is `ushort`, `uint`, or `ulong`</span></span>
   * <span data-ttu-id="212c8-815">`S1` es `int` y `S2` es `uint` o `ulong`</span><span class="sxs-lookup"><span data-stu-id="212c8-815">`S1` is `int` and `S2` is `uint`, or `ulong`</span></span>
   * <span data-ttu-id="212c8-816">`S1` es `long` y `S2` es `ulong`</span><span class="sxs-lookup"><span data-stu-id="212c8-816">`S1` is `long` and `S2` is `ulong`</span></span>

#### <a name="overloading-in-generic-classes"></a><span data-ttu-id="212c8-817">Sobrecarga en clases genéricas</span><span class="sxs-lookup"><span data-stu-id="212c8-817">Overloading in generic classes</span></span>

<span data-ttu-id="212c8-818">Mientras que las signaturas declaradas deben ser únicas, es posible que la sustitución de los argumentos de tipo dé como resultado firmas idénticas.</span><span class="sxs-lookup"><span data-stu-id="212c8-818">While signatures as declared must be unique, it is possible that substitution of type arguments results in identical signatures.</span></span> <span data-ttu-id="212c8-819">En tales casos, las reglas de separación de sobrecargas de la resolución de sobrecarga anterior seleccionarán el miembro más específico.</span><span class="sxs-lookup"><span data-stu-id="212c8-819">In such cases, the tie-breaking rules of overload resolution above will pick the most specific member.</span></span>

<span data-ttu-id="212c8-820">En los ejemplos siguientes se muestran las sobrecargas válidas y no válidas según esta regla:</span><span class="sxs-lookup"><span data-stu-id="212c8-820">The following examples show overloads that are valid and invalid according to this rule:</span></span>

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a><span data-ttu-id="212c8-821">Comprobación en tiempo de compilación de la resolución dinámica de sobrecarga</span><span class="sxs-lookup"><span data-stu-id="212c8-821">Compile-time checking of dynamic overload resolution</span></span>

<span data-ttu-id="212c8-822">Para la mayoría de las operaciones enlazadas dinámicamente, el conjunto de posibles candidatos para la resolución se desconoce en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-822">For most dynamically bound operations the set of possible candidates for resolution is unknown at compile-time.</span></span> <span data-ttu-id="212c8-823">En algunos casos, sin embargo, el conjunto de candidatos se conoce en tiempo de compilación:</span><span class="sxs-lookup"><span data-stu-id="212c8-823">In certain cases, however the candidate set is known at compile-time:</span></span>

*  <span data-ttu-id="212c8-824">Llamadas a métodos estáticos con argumentos dinámicos</span><span class="sxs-lookup"><span data-stu-id="212c8-824">Static method calls with dynamic arguments</span></span>
*  <span data-ttu-id="212c8-825">Llamadas al método de instancia donde el receptor no es una expresión dinámica</span><span class="sxs-lookup"><span data-stu-id="212c8-825">Instance method calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="212c8-826">Llamadas de indexador en las que el receptor no es una expresión dinámica</span><span class="sxs-lookup"><span data-stu-id="212c8-826">Indexer calls where the receiver is not a dynamic expression</span></span>
*  <span data-ttu-id="212c8-827">Llamadas de constructor con argumentos dinámicos</span><span class="sxs-lookup"><span data-stu-id="212c8-827">Constructor calls with dynamic arguments</span></span>

<span data-ttu-id="212c8-828">En estos casos, se realiza una comprobación limitada en tiempo de compilación para cada candidato a fin de ver si alguna de ellas podría aplicarse en tiempo de ejecución. Esta comprobación consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-828">In these cases a limited compile-time check is performed for each candidate to see if any of them could possibly apply at run-time.This check consists of the following steps:</span></span>

*  <span data-ttu-id="212c8-829">Inferencia de tipos parciales: Cualquier argumento de tipo que no dependa directa o indirectamente de un argumento de tipo `dynamic` se deduce mediante las reglas de [inferencia de tipos](expressions.md#type-inference).</span><span class="sxs-lookup"><span data-stu-id="212c8-829">Partial type inference: Any type argument that does not depend directly or indirectly on an argument of type `dynamic` is inferred using the rules of [Type inference](expressions.md#type-inference).</span></span> <span data-ttu-id="212c8-830">Los argumentos de tipo restantes son desconocidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-830">The remaining type arguments are unknown.</span></span>
*  <span data-ttu-id="212c8-831">Comprobación parcial de aplicabilidad: La aplicabilidad se comprueba según el [miembro de función aplicable](expressions.md#applicable-function-member), pero se omiten los parámetros cuyos tipos son desconocidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-831">Partial applicability check: Applicability is checked according to [Applicable function member](expressions.md#applicable-function-member), but ignoring parameters whose types are unknown.</span></span>
*  <span data-ttu-id="212c8-832">Si ningún candidato pasa esta prueba, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-832">If no candidate passes this test, a compile-time error occurs.</span></span>

### <a name="function-member-invocation"></a><span data-ttu-id="212c8-833">Invocación de miembros de función</span><span class="sxs-lookup"><span data-stu-id="212c8-833">Function member invocation</span></span>

<span data-ttu-id="212c8-834">En esta sección se describe el proceso que tiene lugar en tiempo de ejecución para invocar un miembro de función determinado.</span><span class="sxs-lookup"><span data-stu-id="212c8-834">This section describes the process that takes place at run-time to invoke a particular function member.</span></span> <span data-ttu-id="212c8-835">Se supone que un proceso de tiempo de enlace ya ha determinado el miembro determinado que se va a invocar, posiblemente aplicando la resolución de sobrecarga a un conjunto de miembros de función candidatos.</span><span class="sxs-lookup"><span data-stu-id="212c8-835">It is assumed that a binding-time process has already determined the particular member to invoke, possibly by applying overload resolution to a set of candidate function members.</span></span>

<span data-ttu-id="212c8-836">Con el fin de describir el proceso de invocación, los miembros de función se dividen en dos categorías:</span><span class="sxs-lookup"><span data-stu-id="212c8-836">For purposes of describing the invocation process, function members are divided into two categories:</span></span>

*  <span data-ttu-id="212c8-837">Miembros de función estáticos.</span><span class="sxs-lookup"><span data-stu-id="212c8-837">Static function members.</span></span> <span data-ttu-id="212c8-838">Se trata de constructores de instancia, métodos estáticos, descriptores de acceso de propiedades estáticas y operadores definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="212c8-838">These are instance constructors, static methods, static property accessors, and user-defined operators.</span></span> <span data-ttu-id="212c8-839">Los miembros de función estáticos siempre son no virtuales.</span><span class="sxs-lookup"><span data-stu-id="212c8-839">Static function members are always non-virtual.</span></span>
*  <span data-ttu-id="212c8-840">Miembros de función de instancia.</span><span class="sxs-lookup"><span data-stu-id="212c8-840">Instance function members.</span></span> <span data-ttu-id="212c8-841">Son métodos de instancia, descriptores de acceso de propiedades de instancia y descriptores de acceso de indexador.</span><span class="sxs-lookup"><span data-stu-id="212c8-841">These are instance methods, instance property accessors, and indexer accessors.</span></span> <span data-ttu-id="212c8-842">Los miembros de la función de instancia son no virtuales o virtuales, y siempre se invocan en una instancia determinada.</span><span class="sxs-lookup"><span data-stu-id="212c8-842">Instance function members are either non-virtual or virtual, and are always invoked on a particular instance.</span></span> <span data-ttu-id="212c8-843">La instancia se calcula mediante una expresión de instancia y se vuelve accesible dentro del miembro de función como `this` ([este acceso](expressions.md#this-access)).</span><span class="sxs-lookup"><span data-stu-id="212c8-843">The instance is computed by an instance expression, and it becomes accessible within the function member as `this` ([This access](expressions.md#this-access)).</span></span>

<span data-ttu-id="212c8-844">El procesamiento en tiempo de ejecución de una invocación de miembro de función consta de los pasos siguientes, donde `M` es el miembro de función y, si `M` es un miembro de instancia, `E` es la expresión de instancia:</span><span class="sxs-lookup"><span data-stu-id="212c8-844">The run-time processing of a function member invocation consists of the following steps, where `M` is the function member and, if `M` is an instance member, `E` is the instance expression:</span></span>

*  <span data-ttu-id="212c8-845">Si `M` es un miembro de función estático:</span><span class="sxs-lookup"><span data-stu-id="212c8-845">If `M` is a static function member:</span></span>
   * <span data-ttu-id="212c8-846">La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="212c8-846">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="212c8-847">se invoca `M`.</span><span class="sxs-lookup"><span data-stu-id="212c8-847">`M` is invoked.</span></span>

*  <span data-ttu-id="212c8-848">Si `M` es un miembro de función de instancia declarado en *value_type*:</span><span class="sxs-lookup"><span data-stu-id="212c8-848">If `M` is an instance function member declared in a *value_type*:</span></span>
   * <span data-ttu-id="212c8-849">se evalúa `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-849">`E` is evaluated.</span></span> <span data-ttu-id="212c8-850">Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="212c8-850">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="212c8-851">Si `E` no está clasificado como una variable, se crea una variable local temporal de tipo `E` y se asigna el valor de `E` a esa variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-851">If `E` is not classified as a variable, then a temporary local variable of `E`'s type is created and the value of `E` is assigned to that variable.</span></span> <span data-ttu-id="212c8-852">a continuación, `E` se reclasifica como una referencia a esa variable local temporal.</span><span class="sxs-lookup"><span data-stu-id="212c8-852">`E` is then reclassified as a reference to that temporary local variable.</span></span> <span data-ttu-id="212c8-853">La variable temporal es accesible como `this` dentro de `M`, pero no de otra manera.</span><span class="sxs-lookup"><span data-stu-id="212c8-853">The temporary variable is accessible as `this` within `M`, but not in any other way.</span></span> <span data-ttu-id="212c8-854">Por lo tanto, solo cuando `E` es una variable verdadera, es posible que el llamador Observe los cambios que `M` realiza en `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-854">Thus, only when `E` is a true variable is it possible for the caller to observe the changes that `M` makes to `this`.</span></span>
   * <span data-ttu-id="212c8-855">La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="212c8-855">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="212c8-856">se invoca `M`.</span><span class="sxs-lookup"><span data-stu-id="212c8-856">`M` is invoked.</span></span> <span data-ttu-id="212c8-857">La variable a la que hace referencia `E` se convierte en la variable a la que hace referencia `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-857">The variable referenced by `E` becomes the variable referenced by `this`.</span></span>

*  <span data-ttu-id="212c8-858">Si `M` es un miembro de función de instancia declarado en *reference_type*:</span><span class="sxs-lookup"><span data-stu-id="212c8-858">If `M` is an instance function member declared in a *reference_type*:</span></span>
   * <span data-ttu-id="212c8-859">se evalúa `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-859">`E` is evaluated.</span></span> <span data-ttu-id="212c8-860">Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="212c8-860">If this evaluation causes an exception, then no further steps are executed.</span></span>
   * <span data-ttu-id="212c8-861">La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).</span><span class="sxs-lookup"><span data-stu-id="212c8-861">The argument list is evaluated as described in [Argument lists](expressions.md#argument-lists).</span></span>
   * <span data-ttu-id="212c8-862">Si el tipo de `E` es *value_type*, se realiza una conversión boxing ([conversiones boxing](types.md#boxing-conversions)) para convertir `E` al tipo `object` y `E` se considera de tipo `object` en los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="212c8-862">If the type of `E` is a *value_type*, a boxing conversion ([Boxing conversions](types.md#boxing-conversions)) is performed to convert `E` to type `object`, and `E` is considered to be of type `object` in the following steps.</span></span> <span data-ttu-id="212c8-863">En este caso, `M` solo puede ser un miembro de `System.Object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-863">In this case, `M` could only be a member of `System.Object`.</span></span>
   * <span data-ttu-id="212c8-864">El valor de `E` se comprueba para ser válido.</span><span class="sxs-lookup"><span data-stu-id="212c8-864">The value of `E` is checked to be valid.</span></span> <span data-ttu-id="212c8-865">Si el valor de `E` es `null`, se produce una @no__t 2 y no se ejecuta ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="212c8-865">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
   * <span data-ttu-id="212c8-866">Se determina la implementación del miembro de función que se va a invocar:</span><span class="sxs-lookup"><span data-stu-id="212c8-866">The function member implementation to invoke is determined:</span></span>
     * <span data-ttu-id="212c8-867">Si el tipo de tiempo de enlace de `E` es una interfaz, el miembro de función que se va a invocar es la implementación de `M` proporcionada por el tipo en tiempo de ejecución de la instancia a la que hace referencia `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-867">If the binding-time type of `E` is an interface, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="212c8-868">Este miembro de función se determina aplicando las reglas de asignación de interfaz ([asignación de interfaz](interfaces.md#interface-mapping)) para determinar la implementación de `M` proporcionada por el tipo en tiempo de ejecución de la instancia a la que hace referencia `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-868">This function member is determined by applying the interface mapping rules ([Interface mapping](interfaces.md#interface-mapping)) to determine the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="212c8-869">De lo contrario, si `M` es un miembro de función virtual, el miembro de función que se va a invocar es la implementación de `M` proporcionada por el tipo en tiempo de ejecución de la instancia a la que hace referencia `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-869">Otherwise, if `M` is a virtual function member, the function member to invoke is the implementation of `M` provided by the run-time type of the instance referenced by `E`.</span></span> <span data-ttu-id="212c8-870">Este miembro de función se determina aplicando las reglas para determinar la implementación más derivada ([métodos virtuales](classes.md#virtual-methods)) de `M` con respecto al tipo en tiempo de ejecución de la instancia a la que hace referencia `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-870">This function member is determined by applying the rules for determining the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of `M` with respect to the run-time type of the instance referenced by `E`.</span></span>
     * <span data-ttu-id="212c8-871">De lo contrario, `M` es un miembro de función no virtual y el miembro de función que se va a invocar es `M`.</span><span class="sxs-lookup"><span data-stu-id="212c8-871">Otherwise, `M` is a non-virtual function member, and the function member to invoke is `M` itself.</span></span>
   * <span data-ttu-id="212c8-872">Se invoca la implementación de miembro de función determinada en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="212c8-872">The function member implementation determined in the step above is invoked.</span></span> <span data-ttu-id="212c8-873">El objeto al que hace referencia `E` se convierte en el objeto al que hace referencia `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-873">The object referenced by `E` becomes the object referenced by `this`.</span></span>

#### <a name="invocations-on-boxed-instances"></a><span data-ttu-id="212c8-874">Invocaciones en instancias de conversión boxing</span><span class="sxs-lookup"><span data-stu-id="212c8-874">Invocations on boxed instances</span></span>

<span data-ttu-id="212c8-875">Un miembro de función implementado en un *value_type* se puede invocar a través de una instancia de conversión boxing de ese *value_type* en las situaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-875">A function member implemented in a *value_type* can be invoked through a boxed instance of that *value_type* in the following situations:</span></span>

*  <span data-ttu-id="212c8-876">Cuando el miembro de función es un `override` de un método heredado del tipo `object` y se invoca a través de una expresión de instancia de tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-876">When the function member is an `override` of a method inherited from type `object` and is invoked through an instance expression of type `object`.</span></span>
*  <span data-ttu-id="212c8-877">Cuando el miembro de función es una implementación de un miembro de función de interfaz y se invoca a través de una expresión de instancia de un *interface_type*.</span><span class="sxs-lookup"><span data-stu-id="212c8-877">When the function member is an implementation of an interface function member and is invoked through an instance expression of an *interface_type*.</span></span>
*  <span data-ttu-id="212c8-878">Cuando el miembro de función se invoca a través de un delegado.</span><span class="sxs-lookup"><span data-stu-id="212c8-878">When the function member is invoked through a delegate.</span></span>

<span data-ttu-id="212c8-879">En estas situaciones, se considera que la instancia con conversión boxing contiene una variable de *value_type*y esta variable se convierte en la variable a la que hace referencia `this` dentro de la invocación del miembro de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-879">In these situations, the boxed instance is considered to contain a variable of the *value_type*, and this variable becomes the variable referenced by `this` within the function member invocation.</span></span> <span data-ttu-id="212c8-880">En concreto, esto significa que cuando se invoca un miembro de función en una instancia de conversión boxing, es posible que el miembro de función modifique el valor contenido en la instancia de conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="212c8-880">In particular, this means that when a function member is invoked on a boxed instance, it is possible for the function member to modify the value contained in the boxed instance.</span></span>

## <a name="primary-expressions"></a><span data-ttu-id="212c8-881">Expresiones primarias</span><span class="sxs-lookup"><span data-stu-id="212c8-881">Primary expressions</span></span>

<span data-ttu-id="212c8-882">Las expresiones primarias incluyen las formas más sencillas de las expresiones.</span><span class="sxs-lookup"><span data-stu-id="212c8-882">Primary expressions include the simplest forms of expressions.</span></span>

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

<span data-ttu-id="212c8-883">Las expresiones primarias se dividen entre *array_creation_expression*s y *primary_no_array_creation_expression*s.</span><span class="sxs-lookup"><span data-stu-id="212c8-883">Primary expressions are divided between *array_creation_expression*s and *primary_no_array_creation_expression*s.</span></span> <span data-ttu-id="212c8-884">Al tratar la expresión de creación de matrices de esta manera, en lugar de enumerarla junto con las otras formas de expresión simples, permite que la gramática no permita el código potencialmente confuso, como</span><span class="sxs-lookup"><span data-stu-id="212c8-884">Treating array-creation-expression in this way, rather than listing it along with the other simple expression forms, enables the grammar to disallow potentially confusing code such as</span></span>
```csharp
object o = new int[3][1];
```
<span data-ttu-id="212c8-885">que de otro modo se interpretaría como</span><span class="sxs-lookup"><span data-stu-id="212c8-885">which would otherwise be interpreted as</span></span>
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a><span data-ttu-id="212c8-886">Literales</span><span class="sxs-lookup"><span data-stu-id="212c8-886">Literals</span></span>

<span data-ttu-id="212c8-887">Un *primary_expression* que consta de un *literal* ([literales](lexical-structure.md#literals)) se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-887">A *primary_expression* that consists of a *literal* ([Literals](lexical-structure.md#literals)) is classified as a value.</span></span>


### <a name="interpolated-strings"></a><span data-ttu-id="212c8-888">Cadenas interpoladas</span><span class="sxs-lookup"><span data-stu-id="212c8-888">Interpolated strings</span></span>

<span data-ttu-id="212c8-889">Un *interpolated_string_expression* consta de un signo de @no__t 1 seguido de un literal de cadena normal o textual, en el que los huecos, delimitados por `{` y `}`, incluyen expresiones y especificaciones de formato.</span><span class="sxs-lookup"><span data-stu-id="212c8-889">An *interpolated_string_expression* consists of a `$` sign followed by a regular or verbatim string literal, wherein holes, delimited by `{` and `}`, enclose expressions and formatting specifications.</span></span> <span data-ttu-id="212c8-890">Una expresión de cadena interpolada es el resultado de un *interpolated_string_literal* que se ha dividido en tokens individuales, tal y como se describe en [literales de cadena interpolados](lexical-structure.md#interpolated-string-literals).</span><span class="sxs-lookup"><span data-stu-id="212c8-890">An interpolated string expression is the result of an *interpolated_string_literal* that has been broken up into individual tokens, as described in [Interpolated string literals](lexical-structure.md#interpolated-string-literals).</span></span>

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

<span data-ttu-id="212c8-891">El *constant_expression* de una interpolación debe tener una conversión implícita a `int`.</span><span class="sxs-lookup"><span data-stu-id="212c8-891">The *constant_expression* in an interpolation must have an implicit conversion to `int`.</span></span>

<span data-ttu-id="212c8-892">Un *interpolated_string_expression* se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-892">An *interpolated_string_expression* is classified as a value.</span></span> <span data-ttu-id="212c8-893">Si se convierte inmediatamente en `System.IFormattable` o `System.FormattableString` con una conversión de cadena interpolada implícita ([conversiones de cadenas interpoladas implícitas](conversions.md#implicit-interpolated-string-conversions)), la expresión de cadena interpolada tiene ese tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-893">If it is immediately converted to `System.IFormattable` or `System.FormattableString` with an implicit interpolated string conversion ([Implicit interpolated string conversions](conversions.md#implicit-interpolated-string-conversions)), the interpolated string expression has that type.</span></span> <span data-ttu-id="212c8-894">De lo contrario, tiene el tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="212c8-894">Otherwise, it has the type `string`.</span></span>

<span data-ttu-id="212c8-895">Si el tipo de una cadena interpolada es `System.IFormattable` o `System.FormattableString`, el significado es una llamada a `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="212c8-895">If the type of an interpolated string is `System.IFormattable` or `System.FormattableString`, the meaning is a call to `System.Runtime.CompilerServices.FormattableStringFactory.Create`.</span></span> <span data-ttu-id="212c8-896">Si el tipo es `string`, el significado de la expresión es una llamada a `string.Format`.</span><span class="sxs-lookup"><span data-stu-id="212c8-896">If the type is `string`, the meaning of the expression is a call to `string.Format`.</span></span> <span data-ttu-id="212c8-897">En ambos casos, la lista de argumentos de la llamada consta de un literal de cadena de formato con marcadores de posición para cada interpolación y un argumento para cada expresión correspondiente a los marcadores de posición.</span><span class="sxs-lookup"><span data-stu-id="212c8-897">In both cases, the argument list of the call consists of a format string literal with placeholders for each interpolation, and an argument for each expression corresponding to the place holders.</span></span>

<span data-ttu-id="212c8-898">El literal de cadena de formato se construye como sigue, donde `N` es el número de interpolaciones de *interpolated_string_expression*:</span><span class="sxs-lookup"><span data-stu-id="212c8-898">The format string literal is constructed as follows, where `N` is the number of interpolations in the *interpolated_string_expression*:</span></span>

*  <span data-ttu-id="212c8-899">Si un *interpolated_regular_string_whole* o un *interpolated_verbatim_string_whole* siguen el signo `$`, el literal de cadena de formato es ese token.</span><span class="sxs-lookup"><span data-stu-id="212c8-899">If an *interpolated_regular_string_whole* or an *interpolated_verbatim_string_whole* follows the `$` sign, then the format string literal is that token.</span></span>
*  <span data-ttu-id="212c8-900">De lo contrario, el literal de cadena de formato consta de:</span><span class="sxs-lookup"><span data-stu-id="212c8-900">Otherwise, the format string literal consists of:</span></span> 
   *  <span data-ttu-id="212c8-901">En primer lugar, *interpolated_regular_string_start* o *interpolated_verbatim_string_start*</span><span class="sxs-lookup"><span data-stu-id="212c8-901">First the *interpolated_regular_string_start* or *interpolated_verbatim_string_start*</span></span>
   *  <span data-ttu-id="212c8-902">A continuación, para cada número `I` de `0` a `N-1`:</span><span class="sxs-lookup"><span data-stu-id="212c8-902">Then for each number `I` from `0` to `N-1`:</span></span> 
      * <span data-ttu-id="212c8-903">Representación decimal de `I`</span><span class="sxs-lookup"><span data-stu-id="212c8-903">The decimal representation of `I`</span></span>
      * <span data-ttu-id="212c8-904">Después, si la *interpolación* correspondiente tiene un *constant_expression*, una `,` (coma) seguida de la representación decimal del valor de *constant_expression*</span><span class="sxs-lookup"><span data-stu-id="212c8-904">Then, if the corresponding *interpolation* has a *constant_expression*, a `,` (comma) followed by the decimal representation of the value of the *constant_expression*</span></span>
      * <span data-ttu-id="212c8-905">Después, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* o *interpolated_verbatim_string_end* inmediatamente después de la interpolación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-905">Then the *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* or *interpolated_verbatim_string_end* immediately following the corresponding interpolation.</span></span>

<span data-ttu-id="212c8-906">Los argumentos subsiguientes son simplemente las *expresiones* de las *interpolaciones* (si existen), en orden.</span><span class="sxs-lookup"><span data-stu-id="212c8-906">The subsequent arguments are simply the *expressions* from the *interpolations* (if any), in order.</span></span>

<span data-ttu-id="212c8-907">TODO: ejemplos.</span><span class="sxs-lookup"><span data-stu-id="212c8-907">TODO: examples.</span></span>


### <a name="simple-names"></a><span data-ttu-id="212c8-908">Nombres simples</span><span class="sxs-lookup"><span data-stu-id="212c8-908">Simple names</span></span>

<span data-ttu-id="212c8-909">Un *simple_name* consta de un identificador y, opcionalmente, seguido de una lista de argumentos de tipo:</span><span class="sxs-lookup"><span data-stu-id="212c8-909">A *simple_name* consists of an identifier, optionally followed by a type argument list:</span></span>

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

<span data-ttu-id="212c8-910">Un *simple_name* tiene el formato `I` o el formulario `I<A1,...,Ak>`, donde `I` es un identificador único y `<A1,...,Ak>` es un *type_argument_list*opcional.</span><span class="sxs-lookup"><span data-stu-id="212c8-910">A *simple_name* is either of the form `I` or of the form `I<A1,...,Ak>`, where `I` is a single identifier and `<A1,...,Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="212c8-911">Si no se especifica *type_argument_list* , considere la posibilidad de `K` para que sea cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-911">When no *type_argument_list* is specified, consider `K` to be zero.</span></span> <span data-ttu-id="212c8-912">El *simple_name* se evalúa y se clasifica de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-912">The *simple_name* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="212c8-913">Si `K` es cero y *simple_name* aparece dentro de un *bloque* y si el espacio de declaración de variable local del *bloque*(o el de un *bloque*de[inclusión) contiene](basic-concepts.md#declarations)una variable local, un parámetro o una constante con Name @ no__t-6, el *simple_name* hace referencia a esa variable local, parámetro o constante y se clasifica como una variable o un valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-913">If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>
*  <span data-ttu-id="212c8-914">Si `K` es cero y *simple_name* aparece dentro del cuerpo de una declaración de método genérico y la Declaración incluye un parámetro de tipo con el nombre @ no__t-2, el *simple_name* hace referencia a ese parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-914">If `K` is zero and the *simple_name* appears within the body of a generic method declaration and if that declaration includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
*  <span data-ttu-id="212c8-915">De lo contrario, para cada tipo de instancia @ no__t-0 ([el tipo de instancia](classes.md#the-instance-type)), empezando por el tipo de instancia de la declaración de tipo de inclusión inmediata y continuando con el tipo de instancia de cada declaración de clase o estructura envolvente (si existe):</span><span class="sxs-lookup"><span data-stu-id="212c8-915">Otherwise, for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of the immediately enclosing type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
   *  <span data-ttu-id="212c8-916">Si `K` es cero y la declaración de `T` incluye un parámetro de tipo con el nombre @ no__t-2, el *simple_name* hace referencia a ese parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-916">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *simple_name* refers to that type parameter.</span></span>
   *  <span data-ttu-id="212c8-917">De lo contrario, si una búsqueda de miembro ([búsqueda de miembros](expressions.md#member-lookup)) de `I` en `T` con argumentos `K` @ no__t-4Type genera una coincidencia:</span><span class="sxs-lookup"><span data-stu-id="212c8-917">Otherwise, if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match:</span></span>
      * <span data-ttu-id="212c8-918">Si `T` es el tipo de instancia de la clase o el tipo de estructura de inclusión inmediata y la búsqueda identifica uno o más métodos, el resultado es un grupo de métodos con una expresión de instancia asociada de `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-918">If `T` is the instance type of the immediately enclosing class or struct type and the lookup identifies one or more methods, the result is a method group with an associated instance expression of `this`.</span></span> <span data-ttu-id="212c8-919">Si se especificó una lista de argumentos de tipo, se usa para llamar a un método genérico ([invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="212c8-919">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
      * <span data-ttu-id="212c8-920">De lo contrario, si `T` es el tipo de instancia de la clase o el tipo de estructura que se encuentra inmediatamente, si la búsqueda identifica un miembro de instancia, y si la referencia aparece dentro del cuerpo de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia, el resultado es igual que un acceso a miembro ([acceso a miembros](expressions.md#member-access)) con el formato `this.I`.</span><span class="sxs-lookup"><span data-stu-id="212c8-920">Otherwise, if `T` is the instance type of the immediately enclosing class or struct type, if the lookup identifies an instance member, and if the reference occurs within the body of an instance constructor, an instance method, or an instance accessor, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `this.I`.</span></span> <span data-ttu-id="212c8-921">Esto solo puede ocurrir cuando `K` es cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-921">This can only happen when `K` is zero.</span></span>
      * <span data-ttu-id="212c8-922">De lo contrario, el resultado es el mismo que un acceso a miembro ([acceso a miembros](expressions.md#member-access)) con el formato `T.I` o `T.I<A1,...,Ak>`.</span><span class="sxs-lookup"><span data-stu-id="212c8-922">Otherwise, the result is the same as a member access ([Member access](expressions.md#member-access)) of the form `T.I` or `T.I<A1,...,Ak>`.</span></span> <span data-ttu-id="212c8-923">En este caso, es un error en tiempo de enlace para que *simple_name* haga referencia a un miembro de instancia.</span><span class="sxs-lookup"><span data-stu-id="212c8-923">In this case, it is a binding-time error for the *simple_name* to refer to an instance member.</span></span>

*  <span data-ttu-id="212c8-924">De lo contrario, para cada espacio de nombres @ no__t-0, empezando por el espacio de nombres en el que se produce *simple_name* , continuando con cada espacio de nombres envolvente (si existe) y finalizando con el espacio de nombres global, se evalúan los pasos siguientes hasta que se encuentra una entidad:</span><span class="sxs-lookup"><span data-stu-id="212c8-924">Otherwise, for each namespace `N`, starting with the namespace in which the *simple_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
   *  <span data-ttu-id="212c8-925">Si `K` es cero y `I` es el nombre de un espacio de nombres en @ no__t-2, entonces:</span><span class="sxs-lookup"><span data-stu-id="212c8-925">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
      * <span data-ttu-id="212c8-926">Si la ubicación donde se produce el *simple_name* se incluye en una declaración de espacio de nombres para `N` y la declaración del espacio de nombres contiene un *extern_alias_directive* o un *using_alias_directive* que asocia el nombre @ no__t-4 con un espacio de nombres o tipo, *simple_name* es ambiguo y se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-926">If the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="212c8-927">De lo contrario, *simple_name* hace referencia al espacio de nombres denominado `I` en `N`.</span><span class="sxs-lookup"><span data-stu-id="212c8-927">Otherwise, the *simple_name* refers to the namespace named `I` in `N`.</span></span>
   *  <span data-ttu-id="212c8-928">De lo contrario, si `N` contiene un tipo accesible con los parámetros Name @ no__t-1 y `K` @ no__t-3type, entonces:</span><span class="sxs-lookup"><span data-stu-id="212c8-928">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
      * <span data-ttu-id="212c8-929">Si `K` es cero y la ubicación donde se produce el *simple_name* se incluye en una declaración de espacio de nombres para `N` y la declaración del espacio de nombres contiene un *extern_alias_directive* o un *using_alias_directive* que asocia el Name @ no__t-5 con un espacio de nombres o un tipo, el *simple_name* es ambiguo y se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-929">If `K` is zero and the location where the *simple_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *simple_name* is ambiguous and a compile-time error occurs.</span></span>
      * <span data-ttu-id="212c8-930">De lo contrario, *namespace_or_type_name* hace referencia al tipo construido con los argumentos de tipo especificados.</span><span class="sxs-lookup"><span data-stu-id="212c8-930">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="212c8-931">De lo contrario, si la ubicación donde se produce el *simple_name* se incluye en una declaración de espacio de nombres para @ no__t-1:</span><span class="sxs-lookup"><span data-stu-id="212c8-931">Otherwise, if the location where the *simple_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
      * <span data-ttu-id="212c8-932">Si `K` es cero y la declaración del espacio de nombres contiene un *extern_alias_directive* o un *using_alias_directive* que asocia el nombre @ no__t-3 con un espacio de nombres o tipo importado, el *simple_name* hace referencia a ese espacio de nombres o automáticamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-932">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *simple_name* refers to that namespace or type.</span></span>
      * <span data-ttu-id="212c8-933">De lo contrario, si los espacios de nombres y las declaraciones de tipos importados por *using_namespace_directive*s y *using_static_directive*s de la declaración del espacio de nombres contienen exactamente un tipo accesible o un miembro estático que no es de extensión con el nombre @ No__ los parámetros t-2 y `K` @ no__t-4Type, *simple_name* hace referencia a ese tipo o miembro construido con los argumentos de tipo especificados.</span><span class="sxs-lookup"><span data-stu-id="212c8-933">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_static_directive*s of the namespace declaration contain exactly one accessible type or non-extension static member having name `I` and `K` type parameters, then the *simple_name* refers to that type or member constructed with the given type arguments.</span></span>
      * <span data-ttu-id="212c8-934">De lo contrario, si los espacios de nombres y los tipos importados por *using_namespace_directive*s de la declaración de espacio de nombres contienen más de un tipo accesible o un miembro estático de método que no es de extensión con los parámetros Name @ no__t-1 y `K` @ no__t-3type, a continuación, el *simple_name* es ambiguo y se produce un error.</span><span class="sxs-lookup"><span data-stu-id="212c8-934">Otherwise, if the namespaces and types imported by the *using_namespace_directive*s of the namespace declaration contain more than one accessible type or non-extension-method static member having name `I` and `K` type parameters, then the *simple_name* is ambiguous and an error occurs.</span></span>

   <span data-ttu-id="212c8-935">Tenga en cuenta que todo el paso es exactamente paralelo al paso correspondiente en el procesamiento de un *namespace_or_type_name* (espacio de nombres[y nombres de tipo](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="212c8-935">Note that this entire step is exactly parallel to the corresponding step in the processing of a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span>

*  <span data-ttu-id="212c8-936">De lo contrario, *simple_name* no está definido y se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-936">Otherwise, the *simple_name* is undefined and a compile-time error occurs.</span></span>


### <a name="parenthesized-expressions"></a><span data-ttu-id="212c8-937">Expresiones entre paréntesis</span><span class="sxs-lookup"><span data-stu-id="212c8-937">Parenthesized expressions</span></span>

<span data-ttu-id="212c8-938">Un *parenthesized_expression* consta de una *expresión* entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="212c8-938">A *parenthesized_expression* consists of an *expression* enclosed in parentheses.</span></span>

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

<span data-ttu-id="212c8-939">Un *parenthesized_expression* se evalúa evaluando la *expresión* entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="212c8-939">A *parenthesized_expression* is evaluated by evaluating the *expression* within the parentheses.</span></span> <span data-ttu-id="212c8-940">Si la *expresión* entre paréntesis denota un espacio de nombres o un tipo, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-940">If the *expression* within the parentheses denotes a namespace or type, a compile-time error occurs.</span></span> <span data-ttu-id="212c8-941">De lo contrario, el resultado de *parenthesized_expression* es el resultado de la evaluación de la *expresión*contenida.</span><span class="sxs-lookup"><span data-stu-id="212c8-941">Otherwise, the result of the *parenthesized_expression* is the result of the evaluation of the contained *expression*.</span></span>

### <a name="member-access"></a><span data-ttu-id="212c8-942">Acceso a miembros</span><span class="sxs-lookup"><span data-stu-id="212c8-942">Member access</span></span>

<span data-ttu-id="212c8-943">Un *member_access* consta de *primary_expression*, *predefined_type*o *qualified_alias_member*, seguido de un token "`.`", seguido de un *identificador*, seguido opcionalmente de un *type_argument_list* .</span><span class="sxs-lookup"><span data-stu-id="212c8-943">A *member_access* consists of a *primary_expression*, a *predefined_type*, or a *qualified_alias_member*, followed by a "`.`" token, followed by an *identifier*, optionally followed by a *type_argument_list*.</span></span>

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

<span data-ttu-id="212c8-944">La producción *qualified_alias_member* se define en [calificadores de alias del espacio de nombres](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="212c8-944">The *qualified_alias_member* production is defined in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span>

<span data-ttu-id="212c8-945">Un *member_access* tiene el formato `E.I` o el formulario `E.I<A1, ..., Ak>`, donde `E` es una expresión primaria, @no__t 4 es un identificador único y `<A1, ..., Ak>` es un *type_argument_list*opcional.</span><span class="sxs-lookup"><span data-stu-id="212c8-945">A *member_access* is either of the form `E.I` or of the form `E.I<A1, ..., Ak>`, where `E` is a primary-expression, `I` is a single identifier and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="212c8-946">Si no se especifica *type_argument_list* , considere la posibilidad de `K` para que sea cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-946">When no *type_argument_list* is specified, consider `K` to be zero.</span></span>

<span data-ttu-id="212c8-947">Un *member_access* con un *primary_expression* de tipo `dynamic` está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="212c8-947">A *member_access* with a *primary_expression* of type `dynamic` is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="212c8-948">En este caso, el compilador clasifica el acceso a miembros como un acceso de propiedad de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-948">In this case the compiler classifies the member access as a property access of type `dynamic`.</span></span> <span data-ttu-id="212c8-949">A continuación, se aplican las siguientes reglas para determinar el significado de *member_access* en tiempo de ejecución, utilizando el tipo en tiempo de ejecución en lugar del tipo en tiempo de compilación de *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-949">The rules below to determine the meaning of the *member_access* are then applied at run-time, using the run-time type instead of the compile-time type of the *primary_expression*.</span></span> <span data-ttu-id="212c8-950">Si esta clasificación en tiempo de ejecución conduce a un grupo de métodos, el acceso a miembros debe ser el *primary_expression* de un *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-950">If this run-time classification leads to a method group, then the member access must be the *primary_expression* of an *invocation_expression*.</span></span>

<span data-ttu-id="212c8-951">El *member_access* se evalúa y se clasifica de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-951">The *member_access* is evaluated and classified as follows:</span></span>

*  <span data-ttu-id="212c8-952">Si `K` es cero y `E` es un espacio de nombres y `E` contiene un espacio de nombres anidado con el nombre @ no__t-3, el resultado es dicho espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="212c8-952">If `K` is zero and `E` is a namespace and `E` contains a nested namespace with name `I`, then the result is that namespace.</span></span>
*  <span data-ttu-id="212c8-953">De lo contrario, si `E` es un espacio de nombres y `E` contiene un tipo accesible con los parámetros Name @ no__t-2 y `K` @ no__t-4Type, el resultado será ese tipo construido con los argumentos de tipo especificados.</span><span class="sxs-lookup"><span data-stu-id="212c8-953">Otherwise, if `E` is a namespace and `E` contains an accessible type having name `I` and `K` type parameters, then the result is that type constructed with the given type arguments.</span></span>
*  <span data-ttu-id="212c8-954">Si `E` es un *predefined_type* o un *primary_expression* clasificado como un tipo, si `E` no es un parámetro de tipo, y si una búsqueda de miembro ([búsqueda de miembros](expressions.md#member-lookup)) de `I` en `E` con parámetros `K` @ no__t-8type genera una coincidencia, a continuación, `E.I` se evalúa y se clasifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="212c8-954">If `E` is a *predefined_type* or a *primary_expression* classified as a type, if `E` is not a type parameter, and if a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `E` with `K` type parameters produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="212c8-955">Si `I` identifica un tipo, el resultado es ese tipo construido con los argumentos de tipo especificados.</span><span class="sxs-lookup"><span data-stu-id="212c8-955">If `I` identifies a type, then the result is that type constructed with the given type arguments.</span></span>
   *  <span data-ttu-id="212c8-956">Si `I` identifica uno o más métodos, el resultado es un grupo de métodos sin expresión de instancia asociada.</span><span class="sxs-lookup"><span data-stu-id="212c8-956">If `I` identifies one or more methods, then the result is a method group with no associated instance expression.</span></span> <span data-ttu-id="212c8-957">Si se especificó una lista de argumentos de tipo, se usa para llamar a un método genérico ([invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="212c8-957">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="212c8-958">Si `I` identifica una propiedad `static`, el resultado es un acceso de propiedad sin expresión de instancia asociada.</span><span class="sxs-lookup"><span data-stu-id="212c8-958">If `I` identifies a `static` property, then the result is a property access with no associated instance expression.</span></span>
   *  <span data-ttu-id="212c8-959">Si `I` identifica un campo `static`:</span><span class="sxs-lookup"><span data-stu-id="212c8-959">If `I` identifies a `static` field:</span></span>
      * <span data-ttu-id="212c8-960">Si el campo es `readonly` y la referencia se produce fuera del constructor estático de la clase o estructura en la que se declara el campo, el resultado es un valor, es decir, el valor del campo estático @ no__t-1 en @ no__t-2.</span><span class="sxs-lookup"><span data-stu-id="212c8-960">If the field is `readonly` and the reference occurs outside the static constructor of the class or struct in which the field is declared, then the result is a value, namely the value of the static field `I` in `E`.</span></span>
      * <span data-ttu-id="212c8-961">De lo contrario, el resultado es una variable, es decir, el campo estático @ no__t-0 en @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="212c8-961">Otherwise, the result is a variable, namely the static field `I` in `E`.</span></span>
   *  <span data-ttu-id="212c8-962">Si `I` identifica un evento `static`:</span><span class="sxs-lookup"><span data-stu-id="212c8-962">If `I` identifies a `static` event:</span></span>
      * <span data-ttu-id="212c8-963">Si la referencia aparece dentro de la clase o estructura en la que se declara el evento y el evento se declaró sin *event_accessor_declarations* ([eventos](classes.md#events)), `E.I` se procesa exactamente como si `I` fuera un campo estático.</span><span class="sxs-lookup"><span data-stu-id="212c8-963">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), then `E.I` is processed exactly as if `I` were a static field.</span></span>
      * <span data-ttu-id="212c8-964">De lo contrario, el resultado es un acceso de evento sin expresión de instancia asociada.</span><span class="sxs-lookup"><span data-stu-id="212c8-964">Otherwise, the result is an event access with no associated instance expression.</span></span>
   *  <span data-ttu-id="212c8-965">Si `I` identifica una constante, el resultado es un valor, es decir, el valor de dicha constante.</span><span class="sxs-lookup"><span data-stu-id="212c8-965">If `I` identifies a constant, then the result is a value, namely the value of that constant.</span></span>
    * <span data-ttu-id="212c8-966">Si `I` identifica un miembro de enumeración, el resultado es un valor, es decir, el valor de ese miembro de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="212c8-966">If `I` identifies an enumeration member, then the result is a value, namely the value of that enumeration member.</span></span>
    * <span data-ttu-id="212c8-967">De lo contrario, `E.I` es una referencia de miembro no válida y se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-967">Otherwise, `E.I` is an invalid member reference, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="212c8-968">Si `E` es un acceso de propiedad, un acceso de indexador, una variable o un valor, el tipo de que es @ no__t-1, y una búsqueda de miembro ([búsqueda de miembros](expressions.md#member-lookup)) de `I` en `T` con `K` @ no__t-6type arguments genera una coincidencia, se evalúa `E.I` y se clasifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="212c8-968">If `E` is a property access, indexer access, variable, or value, the type of which is `T`, and a member lookup ([Member lookup](expressions.md#member-lookup)) of `I` in `T` with `K` type arguments produces a match, then `E.I` is evaluated and classified as follows:</span></span>
   *  <span data-ttu-id="212c8-969">En primer lugar, si `E` es un acceso de propiedad o indizador, se obtiene el valor de la propiedad o el acceso del indexador ([valores de las expresiones](expressions.md#values-of-expressions)) y `E` se reclasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-969">First, if `E` is a property or indexer access, then the value of the property or indexer access is obtained ([Values of expressions](expressions.md#values-of-expressions)) and `E` is reclassified as a value.</span></span>
   *  <span data-ttu-id="212c8-970">Si `I` identifica uno o más métodos, el resultado es un grupo de métodos con una expresión de instancia asociada de `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-970">If `I` identifies one or more methods, then the result is a method group with an associated instance expression of `E`.</span></span> <span data-ttu-id="212c8-971">Si se especificó una lista de argumentos de tipo, se usa para llamar a un método genérico ([invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="212c8-971">If a type argument list was specified, it is used in calling a generic method ([Method invocations](expressions.md#method-invocations)).</span></span>
   *  <span data-ttu-id="212c8-972">Si `I` identifica una propiedad de instancia,</span><span class="sxs-lookup"><span data-stu-id="212c8-972">If `I` identifies an instance property,</span></span>
      * <span data-ttu-id="212c8-973">Si `E` es `this`, @no__t 2 identifica una propiedad implementada automáticamente ([propiedades implementadas automáticamente](classes.md#automatically-implemented-properties)) sin un establecedor y la referencia se produce dentro de un constructor de instancia para un tipo de clase o struct `T`, el resultado es una variable, es decir, el campo de respaldo oculto para la propiedad automática proporcionada por `I` en la instancia de `T` proporcionada por `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-973">If `E` is `this`, `I` identifies an automatically implemented property ([Automatically implemented properties](classes.md#automatically-implemented-properties)) without a setter, and the reference occurs within an instance constructor for a class or struct type `T`, then the result is a variable, namely the hidden backing field for the auto-property given by `I` in the instance of `T` given by `this`.</span></span>
      * <span data-ttu-id="212c8-974">De lo contrario, el resultado es un acceso de propiedad con una expresión de instancia asociada de @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="212c8-974">Otherwise, the result is a property access with an associated instance expression of `E`.</span></span>
   *  <span data-ttu-id="212c8-975">Si `T` es un *class_type* y `I` identifica un campo de instancia de ese *class_type*:</span><span class="sxs-lookup"><span data-stu-id="212c8-975">If `T` is a *class_type* and `I` identifies an instance field of that *class_type*:</span></span>
      * <span data-ttu-id="212c8-976">Si el valor de `E` es `null`, se produce una `System.NullReferenceException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-976">If the value of `E` is `null`, then a `System.NullReferenceException` is thrown.</span></span>
      * <span data-ttu-id="212c8-977">De lo contrario, si el campo es `readonly` y la referencia se produce fuera de un constructor de instancia de la clase en la que se declara el campo, el resultado es un valor, es decir, el valor del campo @ no__t-1 en el objeto al que hace referencia @ no__t-2.</span><span class="sxs-lookup"><span data-stu-id="212c8-977">Otherwise, if the field is `readonly` and the reference occurs outside an instance constructor of the class in which the field is declared, then the result is a value, namely the value of the field `I` in the object referenced by `E`.</span></span>
      * <span data-ttu-id="212c8-978">De lo contrario, el resultado es una variable, es decir, el campo @ no__t-0 en el objeto al que hace referencia @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="212c8-978">Otherwise, the result is a variable, namely the field `I` in the object referenced by `E`.</span></span>
   *  <span data-ttu-id="212c8-979">Si `T` es un *struct_type* y `I` identifica un campo de instancia de ese *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="212c8-979">If `T` is a *struct_type* and `I` identifies an instance field of that *struct_type*:</span></span>
      * <span data-ttu-id="212c8-980">Si `E` es un valor, o si el campo es `readonly` y la referencia se produce fuera de un constructor de instancia de la estructura en la que se declara el campo, el resultado es un valor, es decir, el valor del campo @ no__t-2 en la instancia de struct especificada por @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="212c8-980">If `E` is a value, or if the field is `readonly` and the reference occurs outside an instance constructor of the struct in which the field is declared, then the result is a value, namely the value of the field `I` in the struct instance given by `E`.</span></span>
      * <span data-ttu-id="212c8-981">De lo contrario, el resultado es una variable, es decir, el campo @ no__t-0 en la instancia de struct proporcionada por @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="212c8-981">Otherwise, the result is a variable, namely the field `I` in the struct instance given by `E`.</span></span>
   *  <span data-ttu-id="212c8-982">Si `I` identifica un evento de instancia:</span><span class="sxs-lookup"><span data-stu-id="212c8-982">If `I` identifies an instance event:</span></span>
      * <span data-ttu-id="212c8-983">Si la referencia se produce dentro de la clase o estructura en la que se declara el evento y el evento se declaró sin *event_accessor_declarations* ([eventos](classes.md#events)), y la referencia no se produce como el lado izquierdo de un operador `+=` o `-=` , `E.I` se procesa exactamente como si `I` fuera un campo de instancia.</span><span class="sxs-lookup"><span data-stu-id="212c8-983">If the reference occurs within the class or struct in which the event is declared, and the event was declared without *event_accessor_declarations* ([Events](classes.md#events)), and the reference does not occur as the left-hand side of a `+=` or `-=` operator, then `E.I` is processed exactly as if `I` was an instance field.</span></span>
      * <span data-ttu-id="212c8-984">De lo contrario, el resultado es un evento de acceso con una expresión de instancia asociada de @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="212c8-984">Otherwise, the result is an event access with an associated instance expression of `E`.</span></span>
*  <span data-ttu-id="212c8-985">De lo contrario, se realiza un intento de procesar `E.I` como una invocación de método de extensión ([invocaciones de método de extensión](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="212c8-985">Otherwise, an attempt is made to process `E.I` as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="212c8-986">Si se produce un error, `E.I` es una referencia de miembro no válida y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-986">If this fails, `E.I` is an invalid member reference, and a binding-time error occurs.</span></span>

#### <a name="identical-simple-names-and-type-names"></a><span data-ttu-id="212c8-987">Nombres simples y nombres de tipo idénticos</span><span class="sxs-lookup"><span data-stu-id="212c8-987">Identical simple names and type names</span></span>

<span data-ttu-id="212c8-988">En un acceso de miembro con el formato `E.I`, si `E` es un identificador único y si el significado de `E` como *simple_name* ([nombres simples](expressions.md#simple-names)) es una constante, un campo, una propiedad, una variable local o un parámetro con el mismo tipo que el significado de `E` como *type_name* ([espacio de nombres y nombres de tipo](basic-concepts.md#namespace-and-type-names)), se permiten ambos significados posibles de `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-988">In a member access of the form `E.I`, if `E` is a single identifier, and if the meaning of `E` as a *simple_name* ([Simple names](expressions.md#simple-names)) is a constant, field, property, local variable, or parameter with the same type as the meaning of `E` as a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)), then both possible meanings of `E` are permitted.</span></span> <span data-ttu-id="212c8-989">Los dos significados posibles de `E.I` nunca son ambiguos, ya que `I` debe ser necesariamente miembro del tipo `E` en ambos casos.</span><span class="sxs-lookup"><span data-stu-id="212c8-989">The two possible meanings of `E.I` are never ambiguous, since `I` must necessarily be a member of the type `E` in both cases.</span></span> <span data-ttu-id="212c8-990">En otras palabras, la regla simplemente permite el acceso a los miembros estáticos y los tipos anidados de `E`, donde en caso contrario, se produciría un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-990">In other words, the rule simply permits access to the static members and nested types of `E` where a compile-time error would otherwise have occurred.</span></span> <span data-ttu-id="212c8-991">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="212c8-991">For example:</span></span>
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

#### <a name="grammar-ambiguities"></a><span data-ttu-id="212c8-992">Ambigüedades de la gramática</span><span class="sxs-lookup"><span data-stu-id="212c8-992">Grammar ambiguities</span></span>

<span data-ttu-id="212c8-993">Las producciones para *simple_name* ([nombres simples](expressions.md#simple-names)) y *member_access* ([acceso a miembros](expressions.md#member-access)) pueden dar lugar a ambigüedades en la gramática de expresiones.</span><span class="sxs-lookup"><span data-stu-id="212c8-993">The productions for *simple_name* ([Simple names](expressions.md#simple-names)) and *member_access* ([Member access](expressions.md#member-access)) can give rise to ambiguities in the grammar for expressions.</span></span> <span data-ttu-id="212c8-994">Por ejemplo, la instrucción:</span><span class="sxs-lookup"><span data-stu-id="212c8-994">For example, the statement:</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="212c8-995">podría interpretarse como una llamada a `F` con dos argumentos, `G < A` y `B > (7)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-995">could be interpreted as a call to `F` with two arguments, `G < A` and `B > (7)`.</span></span> <span data-ttu-id="212c8-996">Como alternativa, podría interpretarse como una llamada a `F` con un argumento, que es una llamada a un método genérico @ no__t-1 con dos argumentos de tipo y un argumento normal.</span><span class="sxs-lookup"><span data-stu-id="212c8-996">Alternatively, it could be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span>

<span data-ttu-id="212c8-997">Si se puede analizar una secuencia de tokens (en contexto) como *simple_name* ([nombres simples](expressions.md#simple-names)), *member_access* ([acceso a miembros](expressions.md#member-access)) o *pointer_member_access* ([acceso a miembros de puntero](unsafe-code.md#pointer-member-access)) que finaliza con un *type_argument_ List* ([argumentos de tipo](types.md#type-arguments)), se examina el token inmediatamente después del token `>` de cierre.</span><span class="sxs-lookup"><span data-stu-id="212c8-997">If a sequence of tokens can be parsed (in context) as a *simple_name* ([Simple names](expressions.md#simple-names)), *member_access* ([Member access](expressions.md#member-access)), or *pointer_member_access* ([Pointer member access](unsafe-code.md#pointer-member-access)) ending with a *type_argument_list* ([Type arguments](types.md#type-arguments)), the token immediately following the closing `>` token is examined.</span></span> <span data-ttu-id="212c8-998">Si es uno de</span><span class="sxs-lookup"><span data-stu-id="212c8-998">If it is one of</span></span>
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
<span data-ttu-id="212c8-999">a continuación, *type_argument_list* se conserva como parte de *simple_name*, *member_access* o *pointer_member_access* y se descarta cualquier otro posible análisis de la secuencia de tokens.</span><span class="sxs-lookup"><span data-stu-id="212c8-999">then the *type_argument_list* is retained as part of the *simple_name*, *member_access* or *pointer_member_access* and any other possible parse of the sequence of tokens is discarded.</span></span> <span data-ttu-id="212c8-1000">De lo contrario, *type_argument_list* no se considera parte de *simple_name*, *member_access* o *pointer_member_access*, incluso si no hay ningún otro análisis posible de la secuencia de tokens.</span><span class="sxs-lookup"><span data-stu-id="212c8-1000">Otherwise, the *type_argument_list* is not considered to be part of the *simple_name*, *member_access* or *pointer_member_access*, even if there is no other possible parse of the sequence of tokens.</span></span> <span data-ttu-id="212c8-1001">Tenga en cuenta que estas reglas no se aplican al analizar un *type_argument_list* en un *namespace_or_type_name* (espacio de nombres[y nombres de tipo](basic-concepts.md#namespace-and-type-names)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1001">Note that these rules are not applied when parsing a *type_argument_list* in a *namespace_or_type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)).</span></span> <span data-ttu-id="212c8-1002">La instrucción</span><span class="sxs-lookup"><span data-stu-id="212c8-1002">The statement</span></span>
```csharp
F(G<A,B>(7));
```
<span data-ttu-id="212c8-1003">, según esta regla, se interpretará como una llamada a `F` con un argumento, que es una llamada a un método genérico `G` con dos argumentos de tipo y un argumento normal.</span><span class="sxs-lookup"><span data-stu-id="212c8-1003">will, according to this rule, be interpreted as a call to `F` with one argument, which is a call to a generic method `G` with two type arguments and one regular argument.</span></span> <span data-ttu-id="212c8-1004">Las instrucciones</span><span class="sxs-lookup"><span data-stu-id="212c8-1004">The statements</span></span>
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
<span data-ttu-id="212c8-1005">cada uno de ellos se interpretará como una llamada a `F` con dos argumentos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1005">will each be interpreted as a call to `F` with two arguments.</span></span> <span data-ttu-id="212c8-1006">La instrucción</span><span class="sxs-lookup"><span data-stu-id="212c8-1006">The statement</span></span>
```csharp
x = F < A > +y;
```
<span data-ttu-id="212c8-1007">se interpretará como un operador menor que, mayor que y unario más, como si la instrucción se hubiera escrito `x = (F < A) > (+y)`, en lugar de como un *simple_name* con un *type_argument_list* seguido de un operador binario Plus.</span><span class="sxs-lookup"><span data-stu-id="212c8-1007">will be interpreted as a less than operator, greater than operator, and unary plus operator, as if the statement had been written `x = (F < A) > (+y)`, instead of as a *simple_name* with a *type_argument_list* followed by a binary plus operator.</span></span> <span data-ttu-id="212c8-1008">En la instrucción</span><span class="sxs-lookup"><span data-stu-id="212c8-1008">In the statement</span></span>
```csharp
x = y is C<T> + z;
```
<span data-ttu-id="212c8-1009">los tokens `C<T>` se interpretan como *namespace_or_type_name* con *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1009">the tokens `C<T>` are interpreted as a *namespace_or_type_name* with a *type_argument_list*.</span></span>

### <a name="invocation-expressions"></a><span data-ttu-id="212c8-1010">Expresiones de invocación</span><span class="sxs-lookup"><span data-stu-id="212c8-1010">Invocation expressions</span></span>

<span data-ttu-id="212c8-1011">Un *invocation_expression* se usa para invocar un método.</span><span class="sxs-lookup"><span data-stu-id="212c8-1011">An *invocation_expression* is used to invoke a method.</span></span>

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

<span data-ttu-id="212c8-1012">Un *invocation_expression* está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) si al menos uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="212c8-1012">An *invocation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="212c8-1013">*Primary_expression* tiene el tipo en tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1013">The *primary_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="212c8-1014">Al menos un argumento de *argument_list* opcional tiene el tipo en tiempo de compilación `dynamic` y *primary_expression* no tiene un tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1014">At least one argument of the optional *argument_list* has compile-time type `dynamic` and the *primary_expression* does not have a delegate type.</span></span>

<span data-ttu-id="212c8-1015">En este caso, el compilador clasifica *invocation_expression* como un valor de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1015">In this case the compiler classifies the *invocation_expression* as a value of type `dynamic`.</span></span> <span data-ttu-id="212c8-1016">A continuación, se aplican las siguientes reglas para determinar el significado de *invocation_expression* en tiempo de ejecución, utilizando el tipo en tiempo de ejecución en lugar del tipo en tiempo de compilación de los *primary_expression* y argumentos que tienen el tipo en tiempo de compilación @no_ _ t-2.</span><span class="sxs-lookup"><span data-stu-id="212c8-1016">The rules below to determine the meaning of the *invocation_expression* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_expression* and arguments which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="212c8-1017">Si el *primary_expression* no tiene el tipo en tiempo de compilación `dynamic`, la invocación del método sufre una comprobación limitada del tiempo de compilación, como se describe en [comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="212c8-1017">If the *primary_expression* does not have compile-time type `dynamic`, then the method invocation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="212c8-1018">El *primary_expression* de un *invocation_expression* debe ser un grupo de métodos o un valor de *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1018">The *primary_expression* of an *invocation_expression* must be a method group or a value of a *delegate_type*.</span></span> <span data-ttu-id="212c8-1019">Si *primary_expression* es un grupo de métodos, *invocation_expression* es una invocación de método ([invocaciones de método](expressions.md#method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1019">If the *primary_expression* is a method group, the *invocation_expression* is a method invocation ([Method invocations](expressions.md#method-invocations)).</span></span> <span data-ttu-id="212c8-1020">Si *primary_expression* es un valor de *delegate_type*, *invocation_expression* es una invocación de delegado ([invocaciones de delegado](expressions.md#delegate-invocations)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1020">If the *primary_expression* is a value of a *delegate_type*, the *invocation_expression* is a delegate invocation ([Delegate invocations](expressions.md#delegate-invocations)).</span></span> <span data-ttu-id="212c8-1021">Si *primary_expression* no es un grupo de métodos ni un valor de *delegate_type*, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1021">If the *primary_expression* is neither a method group nor a value of a *delegate_type*, a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-1022">El *argument_list* opcional ([listas de argumentos](expressions.md#argument-lists)) proporciona valores o referencias a variables para los parámetros del método.</span><span class="sxs-lookup"><span data-stu-id="212c8-1022">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) provides values or variable references for the parameters of the method.</span></span>

<span data-ttu-id="212c8-1023">El resultado de la evaluación de un *invocation_expression* se clasifica como sigue:</span><span class="sxs-lookup"><span data-stu-id="212c8-1023">The result of evaluating an *invocation_expression* is classified as follows:</span></span>

*  <span data-ttu-id="212c8-1024">Si el *invocation_expression* invoca un método o un delegado que devuelve `void`, el resultado es Nothing.</span><span class="sxs-lookup"><span data-stu-id="212c8-1024">If the *invocation_expression* invokes a method or delegate that returns `void`, the result is nothing.</span></span> <span data-ttu-id="212c8-1025">Una expresión que se clasifique como Nothing solo se permite en el contexto de *statement_expression* ([instrucciones de expresión](statements.md#expression-statements)) o como el cuerpo de un *lambda_expression* ([expresiones de función anónima](expressions.md#anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1025">An expression that is classified as nothing is permitted only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)) or as the body of a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)).</span></span> <span data-ttu-id="212c8-1026">En caso contrario, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1026">Otherwise a binding-time error occurs.</span></span>
*  <span data-ttu-id="212c8-1027">De lo contrario, el resultado es un valor del tipo devuelto por el método o el delegado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1027">Otherwise, the result is a value of the type returned by the method or delegate.</span></span>

#### <a name="method-invocations"></a><span data-ttu-id="212c8-1028">Invocaciones de método</span><span class="sxs-lookup"><span data-stu-id="212c8-1028">Method invocations</span></span>

<span data-ttu-id="212c8-1029">En el caso de una invocación de método, el *primary_expression* de *invocation_expression* debe ser un grupo de métodos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1029">For a method invocation, the *primary_expression* of the *invocation_expression* must be a method group.</span></span> <span data-ttu-id="212c8-1030">El grupo de métodos identifica el método que se va a invocar o el conjunto de métodos sobrecargados desde los que se va a elegir un método específico que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="212c8-1030">The method group identifies the one method to invoke or the set of overloaded methods from which to choose a specific method to invoke.</span></span> <span data-ttu-id="212c8-1031">En el último caso, la determinación del método específico que se va a invocar se basa en el contexto proporcionado por los tipos de los argumentos de *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1031">In the latter case, determination of the specific method to invoke is based on the context provided by the types of the arguments in the *argument_list*.</span></span>

<span data-ttu-id="212c8-1032">El procesamiento en tiempo de enlace de una invocación de método con la forma `M(A)`, donde `M` es un grupo de métodos (posiblemente incluir un *type_argument_list*) y `A` es un *argument_list*opcional, consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1032">The binding-time processing of a method invocation of the form `M(A)`, where `M` is a method group (possibly including a *type_argument_list*), and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="212c8-1033">Se construye el conjunto de métodos candidatos para la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="212c8-1033">The set of candidate methods for the method invocation is constructed.</span></span> <span data-ttu-id="212c8-1034">Para cada método `F` asociado al grupo de métodos `M`:</span><span class="sxs-lookup"><span data-stu-id="212c8-1034">For each method `F` associated with the method group `M`:</span></span>
   *  <span data-ttu-id="212c8-1035">Si `F` no es genérico, `F` es un candidato cuando:</span><span class="sxs-lookup"><span data-stu-id="212c8-1035">If `F` is non-generic, `F` is a candidate when:</span></span>
      * <span data-ttu-id="212c8-1036">`M` no tiene ninguna lista de argumentos de tipo y</span><span class="sxs-lookup"><span data-stu-id="212c8-1036">`M` has no type argument list, and</span></span>
      * <span data-ttu-id="212c8-1037">`F` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1037">`F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="212c8-1038">Si `F` es genérico y `M` no tiene ninguna lista de argumentos de tipo, @no__t 2 es candidata cuando:</span><span class="sxs-lookup"><span data-stu-id="212c8-1038">If `F` is generic and `M` has no type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="212c8-1039">La inferencia de tipos ([inferencia de tipo](expressions.md#type-inference)) se realiza correctamente, deduciendo una lista de argumentos de tipo para la llamada y</span><span class="sxs-lookup"><span data-stu-id="212c8-1039">Type inference ([Type inference](expressions.md#type-inference)) succeeds, inferring a list of type arguments for the call, and</span></span>
      * <span data-ttu-id="212c8-1040">Una vez que los argumentos de tipo deducido se sustituyen por los parámetros de tipo de método correspondientes, todos los tipos construidos en la lista de parámetros de F satisfacen sus restricciones (que cumplen con las[restricciones](types.md#satisfying-constraints)) y la lista de parámetros de `F` es aplicable con respecto a @no__t 2 ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1040">Once the inferred type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
   *  <span data-ttu-id="212c8-1041">Si `F` es genérico y `M` incluye una lista de argumentos de tipo, @no__t 2 es candidata cuando:</span><span class="sxs-lookup"><span data-stu-id="212c8-1041">If `F` is generic and `M` includes a type argument list, `F` is a candidate when:</span></span>
      * <span data-ttu-id="212c8-1042">`F` tiene el mismo número de parámetros de tipo de método que se proporcionaron en la lista de argumentos de tipo y</span><span class="sxs-lookup"><span data-stu-id="212c8-1042">`F` has the same number of method type parameters as were supplied in the type argument list, and</span></span>
      * <span data-ttu-id="212c8-1043">Una vez que los argumentos de tipo se sustituyen por los parámetros de tipo de método correspondientes, todos los tipos construidos en la lista de parámetros de F satisfacen sus restricciones (que cumplen con las[restricciones](types.md#satisfying-constraints)) y la lista de parámetros de `F` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1043">Once the type arguments are substituted for the corresponding method type parameters, all constructed types in the parameter list of F satisfy their constraints ([Satisfying constraints](types.md#satisfying-constraints)), and the parameter list of `F` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span>
*  <span data-ttu-id="212c8-1044">El conjunto de métodos candidatos se reduce para contener solo los métodos de los tipos más derivados: Para cada método `C.F` del conjunto, donde `C` es el tipo en el que se declara el método `F`, todos los métodos declarados en un tipo base de `C` se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-1044">The set of candidate methods is reduced to contain only methods from the most derived types: For each method `C.F` in the set, where `C` is the type in which the method `F` is declared, all methods declared in a base type of `C` are removed from the set.</span></span> <span data-ttu-id="212c8-1045">Además, si `C` es un tipo de clase distinto de `object`, todos los métodos declarados en un tipo de interfaz se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-1045">Furthermore, if `C` is a class type other than `object`, all methods declared in an interface type are removed from the set.</span></span> <span data-ttu-id="212c8-1046">(Esta última regla solo tiene efecto cuando el grupo de métodos era el resultado de una búsqueda de miembros en un parámetro de tipo que tiene una clase base efectiva que no es un objeto y un conjunto de interfaces efectivo no vacío).</span><span class="sxs-lookup"><span data-stu-id="212c8-1046">(This latter rule only has affect when the method group was the result of a member lookup on a type parameter having an effective base class other than object and a non-empty effective interface set.)</span></span>
*  <span data-ttu-id="212c8-1047">Si el conjunto resultante de métodos candidatos está vacío, se abandona el procesamiento posterior a lo largo de los pasos siguientes y, en su lugar, se intenta procesar la invocación como una invocación de método de extensión ([invocaciones de método de extensión](expressions.md#extension-method-invocations)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1047">If the resulting set of candidate methods is empty, then further processing along the following steps are abandoned, and instead an attempt is made to process the invocation as an extension method invocation ([Extension method invocations](expressions.md#extension-method-invocations)).</span></span> <span data-ttu-id="212c8-1048">Si se produce un error, no existe ningún método aplicable y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1048">If this fails, then no applicable methods exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="212c8-1049">El mejor método del conjunto de métodos candidatos se identifica mediante las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="212c8-1049">The best method of the set of candidate methods is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="212c8-1050">Si no se puede identificar un único método mejor, la invocación del método es ambigua y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1050">If a single best method cannot be identified, the method invocation is ambiguous, and a binding-time error occurs.</span></span> <span data-ttu-id="212c8-1051">Al realizar la resolución de sobrecarga, los parámetros de un método genérico se tienen en cuenta después de sustituir los argumentos de tipo (proporcionados o deducidos) para los parámetros de tipo de método correspondientes.</span><span class="sxs-lookup"><span data-stu-id="212c8-1051">When performing overload resolution, the parameters of a generic method are considered after substituting the type arguments (supplied or inferred) for the corresponding method type parameters.</span></span>
*  <span data-ttu-id="212c8-1052">Se realiza la validación final del mejor método elegido:</span><span class="sxs-lookup"><span data-stu-id="212c8-1052">Final validation of the chosen best method is performed:</span></span>
   * <span data-ttu-id="212c8-1053">El método se valida en el contexto del grupo de métodos: Si el mejor método es un método estático, el grupo de métodos debe haber sido el resultado de un *simple_name* o un *member_access* a través de un tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1053">The method is validated in the context of the method group: If the best method is a static method, the method group must have resulted from a *simple_name* or a *member_access* through a type.</span></span> <span data-ttu-id="212c8-1054">Si el mejor método es un método de instancia, el grupo de métodos debe haber sido el resultado de un *simple_name*, un *member_access* a través de una variable o un valor o un *base_access*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1054">If the best method is an instance method, the method group must have resulted from a *simple_name*, a *member_access* through a variable or value, or a *base_access*.</span></span> <span data-ttu-id="212c8-1055">Si ninguno de estos requisitos es true, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1055">If neither of these requirements is true, a binding-time error occurs.</span></span>
   * <span data-ttu-id="212c8-1056">Si el mejor método es un método genérico, los argumentos de tipo (suministrados o deducidos) se comprueban con las restricciones (que[cumplen las restricciones](types.md#satisfying-constraints)) declaradas en el método genérico.</span><span class="sxs-lookup"><span data-stu-id="212c8-1056">If the best method is a generic method, the type arguments (supplied or inferred) are checked against the constraints ([Satisfying constraints](types.md#satisfying-constraints)) declared on the generic method.</span></span> <span data-ttu-id="212c8-1057">Si algún argumento de tipo no satisface las restricciones correspondientes en el parámetro de tipo, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1057">If any type argument does not satisfy the corresponding constraint(s) on the type parameter, a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-1058">Una vez que se ha seleccionado y validado un método en tiempo de enlace según los pasos anteriores, la invocación real en tiempo de ejecución se procesa de acuerdo con las reglas de invocación de miembros de función descritas en la [comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="212c8-1058">Once a method has been selected and validated at binding-time by the above steps, the actual run-time invocation is processed according to the rules of function member invocation described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="212c8-1059">El efecto intuitivo de las reglas de resolución descritas anteriormente es el siguiente: Para buscar el método determinado invocado por una invocación de método, empiece con el tipo indicado por la invocación del método y continúe con la cadena de herencia hasta que se encuentre al menos una declaración de método aplicable, accesible y sin invalidación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1059">The intuitive effect of the resolution rules described above is as follows: To locate the particular method invoked by a method invocation, start with the type indicated by the method invocation and proceed up the inheritance chain until at least one applicable, accessible, non-override method declaration is found.</span></span> <span data-ttu-id="212c8-1060">A continuación, realice la inferencia de tipos y la resolución de sobrecarga en el conjunto de métodos aplicables, accesibles y no invalidaciones declarados en ese tipo e invoque el método de la forma que se seleccione.</span><span class="sxs-lookup"><span data-stu-id="212c8-1060">Then perform type inference and overload resolution on the set of applicable, accessible, non-override methods declared in that type and invoke the method thus selected.</span></span> <span data-ttu-id="212c8-1061">Si no se encuentra ningún método, pruebe en su lugar para procesar la invocación como una invocación de método de extensión.</span><span class="sxs-lookup"><span data-stu-id="212c8-1061">If no method was found, try instead to process the invocation as an extension method invocation.</span></span>

#### <a name="extension-method-invocations"></a><span data-ttu-id="212c8-1062">Invocaciones de métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="212c8-1062">Extension method invocations</span></span>

<span data-ttu-id="212c8-1063">En una invocación de método ([invocaciones en instancias de conversión boxing](expressions.md#invocations-on-boxed-instances)) de uno de los formularios</span><span class="sxs-lookup"><span data-stu-id="212c8-1063">In a method invocation ([Invocations on boxed instances](expressions.md#invocations-on-boxed-instances)) of one of the forms</span></span>
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
<span data-ttu-id="212c8-1064">Si el procesamiento normal de la invocación no encuentra ningún método aplicable, se realiza un intento de procesar la construcción como una invocación de método de extensión.</span><span class="sxs-lookup"><span data-stu-id="212c8-1064">if the normal processing of the invocation finds no applicable methods, an attempt is made to process the construct as an extension method invocation.</span></span> <span data-ttu-id="212c8-1065">Si *expr* o cualquiera de los *argumentos* tiene el tipo en tiempo de compilación `dynamic`, los métodos de extensión no se aplicarán.</span><span class="sxs-lookup"><span data-stu-id="212c8-1065">If *expr* or any of the *args* has compile-time type `dynamic`, extension methods will not apply.</span></span>

<span data-ttu-id="212c8-1066">El objetivo es encontrar el mejor *type_name* `C`, de modo que pueda tener lugar la invocación de método estático correspondiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-1066">The objective is to find the best *type_name* `C`, so that the corresponding static method invocation can take place:</span></span>
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

<span data-ttu-id="212c8-1067">Un método de extensión `Ci.Mj` es ***válido*** si:</span><span class="sxs-lookup"><span data-stu-id="212c8-1067">An extension method `Ci.Mj` is ***eligible*** if:</span></span>

*  <span data-ttu-id="212c8-1068">`Ci` es una clase no anidada no genérica</span><span class="sxs-lookup"><span data-stu-id="212c8-1068">`Ci` is a non-generic, non-nested class</span></span>
*  <span data-ttu-id="212c8-1069">El nombre de `Mj` es el *identificador*</span><span class="sxs-lookup"><span data-stu-id="212c8-1069">The name of `Mj` is *identifier*</span></span>
*  <span data-ttu-id="212c8-1070">`Mj` es accesible y aplicable cuando se aplica a los argumentos como un método estático, como se muestra arriba</span><span class="sxs-lookup"><span data-stu-id="212c8-1070">`Mj` is accessible and applicable when applied to the arguments as a static method as shown above</span></span>
*  <span data-ttu-id="212c8-1071">Existe una conversión implícita de identidad, referencia o conversión boxing de *expr* al tipo del primer parámetro de `Mj`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1071">An implicit identity, reference or boxing conversion exists from *expr* to the type of the first parameter of `Mj`.</span></span>

<span data-ttu-id="212c8-1072">La búsqueda de `C` continúa como sigue:</span><span class="sxs-lookup"><span data-stu-id="212c8-1072">The search for `C` proceeds as follows:</span></span>

*  <span data-ttu-id="212c8-1073">A partir de la declaración de espacio de nombres envolvente más cercana, continuando con cada declaración de espacio de nombres envolvente y finalizando con la unidad de compilación que lo contiene, se realizan sucesivos intentos para buscar un conjunto candidato de métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="212c8-1073">Starting with the closest enclosing namespace declaration, continuing with each enclosing namespace declaration, and ending with the containing compilation unit, successive attempts are made to find a candidate set of extension methods:</span></span>
   * <span data-ttu-id="212c8-1074">Si el espacio de nombres o la unidad de compilación especificados contiene directamente declaraciones de tipos no genéricos `Ci` con métodos de extensión válidos `Mj`, el conjunto de estos métodos de extensión es el conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1074">If the given namespace or compilation unit directly contains non-generic type declarations `Ci` with eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
   * <span data-ttu-id="212c8-1075">Si los tipos `Ci` importados por *using_static_declarations* y se declaran directamente en los espacios de nombres importados por *using_namespace_directive*s en el espacio de nombres o la unidad de compilación especificados directamente contienen métodos de extensión válidos `Mj`, entonces el conjunto de estos métodos de extensión es el conjunto de candidatos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1075">If types `Ci` imported by *using_static_declarations* and directly declared in namespaces imported by *using_namespace_directive*s in the given namespace or compilation unit directly contain eligible extension methods `Mj`, then the set of those extension methods is the candidate set.</span></span>
*  <span data-ttu-id="212c8-1076">Si no se encuentra ningún conjunto candidato en ninguna declaración de espacio de nombres envolvente o unidad de compilación, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1076">If no candidate set is found in any enclosing namespace declaration or compilation unit, a compile-time error occurs.</span></span>
*  <span data-ttu-id="212c8-1077">De lo contrario, se aplica la resolución de sobrecarga al conjunto de candidatos tal y como se describe en ([resolución de sobrecarga](expressions.md#overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1077">Otherwise, overload resolution is applied to the candidate set as described in ([Overload resolution](expressions.md#overload-resolution)).</span></span> <span data-ttu-id="212c8-1078">Si no se encuentra ningún método mejor, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1078">If no single best method is found, a compile-time error occurs.</span></span>
*  <span data-ttu-id="212c8-1079">`C` es el tipo en el que se declara el mejor método como método de extensión.</span><span class="sxs-lookup"><span data-stu-id="212c8-1079">`C` is the type within which the best method is declared as an extension method.</span></span>

<span data-ttu-id="212c8-1080">Al usar `C` como destino, la llamada al método se procesa entonces como una invocación de método estático ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1080">Using `C` as a target, the method call is then processed as a static method invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span>

<span data-ttu-id="212c8-1081">Las reglas anteriores significan que los métodos de instancia tienen prioridad sobre los métodos de extensión, que los métodos de extensión disponibles en las declaraciones de espacios de nombres internos tienen prioridad sobre los métodos de extensión disponibles en las declaraciones de espacios de nombres exteriores y esa extensión los métodos declarados directamente en un espacio de nombres tienen prioridad sobre los métodos de extensión importados en el mismo espacio de nombres con una directiva de espacio de nombres using.</span><span class="sxs-lookup"><span data-stu-id="212c8-1081">The preceding rules mean that instance methods take precedence over extension methods, that extension methods available in inner namespace declarations take precedence over extension methods available in outer namespace declarations, and that extension methods declared directly in a namespace take precedence over extension methods imported into that same namespace with a using namespace directive.</span></span> <span data-ttu-id="212c8-1082">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="212c8-1082">For example:</span></span>
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

<span data-ttu-id="212c8-1083">En el ejemplo, el método de `B` tiene prioridad sobre el primer método de extensión y el método de `C` tiene prioridad sobre ambos métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="212c8-1083">In the example, `B`'s method takes precedence over the first extension method, and `C`'s method takes precedence over both extension methods.</span></span>

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

<span data-ttu-id="212c8-1084">El resultado de este ejemplo es:</span><span class="sxs-lookup"><span data-stu-id="212c8-1084">The output of this example is:</span></span>
```console
E.F(1)
D.G(2)
C.H(3)
```
<span data-ttu-id="212c8-1085">`D.G` tiene prioridad sobre `C.G` y `E.F` tiene prioridad sobre `D.F` y `C.F`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1085">`D.G` takes precedence over `C.G`, and `E.F` takes precedence over both `D.F` and `C.F`.</span></span>

#### <a name="delegate-invocations"></a><span data-ttu-id="212c8-1086">Invocaciones de delegado</span><span class="sxs-lookup"><span data-stu-id="212c8-1086">Delegate invocations</span></span>

<span data-ttu-id="212c8-1087">En el caso de una invocación de delegado, el *primary_expression* de *invocation_expression* debe ser un valor de *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1087">For a delegate invocation, the *primary_expression* of the *invocation_expression* must be a value of a *delegate_type*.</span></span> <span data-ttu-id="212c8-1088">Además, si se considera que el *delegate_type* es un miembro de función con la misma lista de parámetros que el *delegate_type*, el *delegate_type* debe ser aplicable ([miembro de función aplicable](expressions.md#applicable-function-member)) con respecto a la *argument_ lista* de *invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1088">Furthermore, considering the *delegate_type* to be a function member with the same parameter list as the *delegate_type*, the *delegate_type* must be applicable ([Applicable function member](expressions.md#applicable-function-member)) with respect to the *argument_list* of the *invocation_expression*.</span></span>

<span data-ttu-id="212c8-1089">El procesamiento en tiempo de ejecución de una invocación de delegado con el formato `D(A)`, donde `D` es un *primary_expression* de un *delegate_type* y `A` es un *argument_list*opcional, consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1089">The run-time processing of a delegate invocation of the form `D(A)`, where `D` is a *primary_expression* of a *delegate_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="212c8-1090">se evalúa `D`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1090">`D` is evaluated.</span></span> <span data-ttu-id="212c8-1091">Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="212c8-1091">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="212c8-1092">El valor de `D` se comprueba para ser válido.</span><span class="sxs-lookup"><span data-stu-id="212c8-1092">The value of `D` is checked to be valid.</span></span> <span data-ttu-id="212c8-1093">Si el valor de `D` es `null`, se produce una @no__t 2 y no se ejecuta ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="212c8-1093">If the value of `D` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="212c8-1094">De lo contrario, `D` es una referencia a una instancia de delegado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1094">Otherwise, `D` is a reference to a delegate instance.</span></span> <span data-ttu-id="212c8-1095">Las invocaciones de miembros de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se realizan en cada una de las entidades a las que se puede llamar en la lista de invocaciones del delegado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1095">Function member invocations ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) are performed on each of the callable entities in the invocation list of the delegate.</span></span> <span data-ttu-id="212c8-1096">Para las entidades a las que se puede llamar que se componen de un método de instancia y de instancia, la instancia de para la invocación es la instancia contenida en la entidad a la que se puede</span><span class="sxs-lookup"><span data-stu-id="212c8-1096">For callable entities consisting of an instance and instance method, the instance for the invocation is the instance contained in the callable entity.</span></span>

### <a name="element-access"></a><span data-ttu-id="212c8-1097">Acceso a elementos</span><span class="sxs-lookup"><span data-stu-id="212c8-1097">Element access</span></span>

<span data-ttu-id="212c8-1098">Un *element_access* consta de un *primary_no_array_creation_expression*, seguido de un token "`[`", seguido de un *argument_list*, seguido de un token "`]`".</span><span class="sxs-lookup"><span data-stu-id="212c8-1098">An *element_access* consists of a *primary_no_array_creation_expression*, followed by a "`[`" token, followed by an *argument_list*, followed by a "`]`" token.</span></span> <span data-ttu-id="212c8-1099">*Argument_list* consta de uno o más *argumentos*, separados por comas.</span><span class="sxs-lookup"><span data-stu-id="212c8-1099">The *argument_list* consists of one or more *argument*s, separated by commas.</span></span>

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

<span data-ttu-id="212c8-1100">El *argument_list* de una *element_access* no puede contener los argumentos `ref` o `out`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1100">The *argument_list* of an *element_access* is not allowed to contain `ref` or `out` arguments.</span></span>

<span data-ttu-id="212c8-1101">Un *element_access* está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) si al menos uno de los siguientes contiene:</span><span class="sxs-lookup"><span data-stu-id="212c8-1101">An *element_access* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) if at least one of the following holds:</span></span>

* <span data-ttu-id="212c8-1102">*Primary_no_array_creation_expression* tiene el tipo en tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1102">The *primary_no_array_creation_expression* has compile-time type `dynamic`.</span></span>
* <span data-ttu-id="212c8-1103">Al menos una expresión de *argument_list* tiene el tipo en tiempo de compilación `dynamic` y *primary_no_array_creation_expression* no tiene un tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="212c8-1103">At least one expression of the *argument_list* has compile-time type `dynamic` and the *primary_no_array_creation_expression* does not have an array type.</span></span>

<span data-ttu-id="212c8-1104">En este caso, el compilador clasifica *element_access* como un valor de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1104">In this case the compiler classifies the *element_access* as a value of type `dynamic`.</span></span> <span data-ttu-id="212c8-1105">A continuación, se aplican las siguientes reglas para determinar el significado de *element_access* en tiempo de ejecución, utilizando el tipo en tiempo de ejecución en lugar del tipo en tiempo de compilación de los de *primary_no_array_creation_expression* y *argument_list* expresiones que tienen el tipo en tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1105">The rules below to determine the meaning of the *element_access* are then applied at run-time, using the run-time type instead of the compile-time type of those of the *primary_no_array_creation_expression* and *argument_list* expressions which have the compile-time type `dynamic`.</span></span> <span data-ttu-id="212c8-1106">Si el *primary_no_array_creation_expression* no tiene el tipo en tiempo de compilación `dynamic`, el acceso al elemento sufre una comprobación de tiempo de compilación limitada, como se describe en [comprobación en tiempo de compilación de la resolución de sobrecarga dinámica](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="212c8-1106">If the *primary_no_array_creation_expression* does not have compile-time type `dynamic`, then the element access undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="212c8-1107">Si el *primary_no_array_creation_expression* de un *element_access* es un valor de *array_type*, *element_access* es un acceso de matriz ([acceso de matriz](expressions.md#array-access)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1107">If the *primary_no_array_creation_expression* of an *element_access* is a value of an *array_type*, the *element_access* is an array access ([Array access](expressions.md#array-access)).</span></span> <span data-ttu-id="212c8-1108">De lo contrario, *primary_no_array_creation_expression* debe ser una variable o un valor de una clase, estructura o tipo de interfaz que tenga uno o más miembros del indexador, en cuyo caso *element_access* es un acceso de indexador ([acceso del indexador](expressions.md#indexer-access)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1108">Otherwise, the *primary_no_array_creation_expression* must be a variable or value of a class, struct, or interface type that has one or more indexer members, in which case the *element_access* is an indexer access ([Indexer access](expressions.md#indexer-access)).</span></span>

#### <a name="array-access"></a><span data-ttu-id="212c8-1109">Acceso a matriz</span><span class="sxs-lookup"><span data-stu-id="212c8-1109">Array access</span></span>

<span data-ttu-id="212c8-1110">Para un acceso de matriz, el *primary_no_array_creation_expression* de *element_access* debe ser un valor de *array_type*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1110">For an array access, the *primary_no_array_creation_expression* of the *element_access* must be a value of an *array_type*.</span></span> <span data-ttu-id="212c8-1111">Además, el *argument_list* de acceso de una matriz no puede contener argumentos con nombre. El número de expresiones de *argument_list* debe ser el mismo que el rango de *array_type*y cada expresión debe ser de tipo `int`, `uint`, `long`, `ulong` o debe poder convertirse implícitamente a uno o varios de estos tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1111">Furthermore, the *argument_list* of an array access is not allowed to contain named arguments.The number of expressions in the *argument_list* must be the same as the rank of the *array_type*, and each expression must be of type `int`, `uint`, `long`, `ulong`, or must be implicitly convertible to one or more of these types.</span></span>

<span data-ttu-id="212c8-1112">El resultado de evaluar un acceso de matriz es una variable del tipo de elemento de la matriz, es decir, el elemento de matriz seleccionado por los valores de las expresiones de *argument_list*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1112">The result of evaluating an array access is a variable of the element type of the array, namely the array element selected by the value(s) of the expression(s) in the *argument_list*.</span></span>

<span data-ttu-id="212c8-1113">El procesamiento en tiempo de ejecución de un acceso de matriz del formulario `P[A]`, donde `P` es un *primary_no_array_creation_expression* de un *array_type* y `A` es un *argument_list*, consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1113">The run-time processing of an array access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of an *array_type* and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="212c8-1114">se evalúa `P`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1114">`P` is evaluated.</span></span> <span data-ttu-id="212c8-1115">Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="212c8-1115">If this evaluation causes an exception, no further steps are executed.</span></span>
*  <span data-ttu-id="212c8-1116">Las expresiones de índice de *argument_list* se evalúan en orden, de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="212c8-1116">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="212c8-1117">Después de la evaluación de cada expresión de índice, se realiza una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) en uno de los siguientes tipos: `int`, `uint`, `long`, @no__t 4.</span><span class="sxs-lookup"><span data-stu-id="212c8-1117">Following evaluation of each index expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="212c8-1118">Se elige el primer tipo de esta lista para el que existe una conversión implícita.</span><span class="sxs-lookup"><span data-stu-id="212c8-1118">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="212c8-1119">Por ejemplo, si la expresión de índice es de tipo `short`, se realiza una conversión implícita a `int`, ya que las conversiones implícitas de @no__t 2 a `int` y de `short` a `long` son posibles.</span><span class="sxs-lookup"><span data-stu-id="212c8-1119">For instance, if the index expression is of type `short` then an implicit conversion to `int` is performed, since implicit conversions from `short` to `int` and from `short` to `long` are possible.</span></span> <span data-ttu-id="212c8-1120">Si la evaluación de una expresión de índice o de la conversión implícita subsiguiente produce una excepción, no se evalúan más expresiones de índice y no se ejecuta ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="212c8-1120">If evaluation of an index expression or the subsequent implicit conversion causes an exception, then no further index expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="212c8-1121">El valor de `P` se comprueba para ser válido.</span><span class="sxs-lookup"><span data-stu-id="212c8-1121">The value of `P` is checked to be valid.</span></span> <span data-ttu-id="212c8-1122">Si el valor de `P` es `null`, se produce una @no__t 2 y no se ejecuta ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="212c8-1122">If the value of `P` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="212c8-1123">El valor de cada expresión de *argument_list* se comprueba con los límites reales de cada dimensión de la instancia de la matriz a la que hace referencia `P`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1123">The value of each expression in the *argument_list* is checked against the actual bounds of each dimension of the array instance referenced by `P`.</span></span> <span data-ttu-id="212c8-1124">Si uno o más valores están fuera del intervalo, se produce una `System.IndexOutOfRangeException` y no se ejecuta ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="212c8-1124">If one or more values are out of range, a `System.IndexOutOfRangeException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="212c8-1125">Se calcula la ubicación del elemento de matriz proporcionado por las expresiones de índice y esta ubicación se convierte en el resultado del acceso a la matriz.</span><span class="sxs-lookup"><span data-stu-id="212c8-1125">The location of the array element given by the index expression(s) is computed, and this location becomes the result of the array access.</span></span>

#### <a name="indexer-access"></a><span data-ttu-id="212c8-1126">Acceso a indizador</span><span class="sxs-lookup"><span data-stu-id="212c8-1126">Indexer access</span></span>

<span data-ttu-id="212c8-1127">En el caso de un acceso de indexador, el *primary_no_array_creation_expression* de *element_access* debe ser una variable o valor de un tipo de clase, struct o interfaz, y este tipo debe implementar uno o varios indexadores aplicables con respecto al *argument_list* de *element_access*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1127">For an indexer access, the *primary_no_array_creation_expression* of the *element_access* must be a variable or value of a class, struct, or interface type, and this type must implement one or more indexers that are applicable with respect to the *argument_list* of the *element_access*.</span></span>

<span data-ttu-id="212c8-1128">El procesamiento en tiempo de enlace de un acceso de indexador con el formato `P[A]`, donde `P` es un *primary_no_array_creation_expression* de un tipo de clase, struct o interfaz `T`, y `A` es un *argument_list*, consta de lo siguiente: pasos</span><span class="sxs-lookup"><span data-stu-id="212c8-1128">The binding-time processing of an indexer access of the form `P[A]`, where `P` is a *primary_no_array_creation_expression* of a class, struct, or interface type `T`, and `A` is an *argument_list*, consists of the following steps:</span></span>

*  <span data-ttu-id="212c8-1129">Se crea el conjunto de indexadores proporcionado por `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1129">The set of indexers provided by `T` is constructed.</span></span> <span data-ttu-id="212c8-1130">El conjunto está formado por todos los indizadores declarados en `T` o un tipo base de `T` que no son declaraciones `override` y son accesibles en el contexto actual ([acceso a miembros](basic-concepts.md#member-access)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1130">The set consists of all indexers declared in `T` or a base type of `T` that are not `override` declarations and are accessible in the current context ([Member access](basic-concepts.md#member-access)).</span></span>
*  <span data-ttu-id="212c8-1131">El conjunto se reduce a los indizadores aplicables y no ocultos por otros indexadores.</span><span class="sxs-lookup"><span data-stu-id="212c8-1131">The set is reduced to those indexers that are applicable and not hidden by other indexers.</span></span> <span data-ttu-id="212c8-1132">Las siguientes reglas se aplican a cada indizador `S.I` en el conjunto, donde `S` es el tipo en el que se declara el indizador `I`:</span><span class="sxs-lookup"><span data-stu-id="212c8-1132">The following rules are applied to each indexer `S.I` in the set, where `S` is the type in which the indexer `I` is declared:</span></span>
   * <span data-ttu-id="212c8-1133">Si `I` no es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)), `I` se quita del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-1133">If `I` is not applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then `I` is removed from the set.</span></span>
   * <span data-ttu-id="212c8-1134">Si `I` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)), todos los indexadores declarados en un tipo base de `S` se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-1134">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)), then all indexers declared in a base type of `S` are removed from the set.</span></span>
   * <span data-ttu-id="212c8-1135">Si `I` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)) y `S` es un tipo de clase distinto de `object`, todos los indexadores declarados en una interfaz se quitan del conjunto.</span><span class="sxs-lookup"><span data-stu-id="212c8-1135">If `I` is applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)) and `S` is a class type other than `object`, all indexers declared in an interface are removed from the set.</span></span>
*  <span data-ttu-id="212c8-1136">Si el conjunto resultante de indizadores candidatos está vacío, no existe ningún indexador aplicable y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1136">If the resulting set of candidate indexers is empty, then no applicable indexers exist, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="212c8-1137">El mejor indexador del conjunto de indizadores candidatos se identifica mediante las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="212c8-1137">The best indexer of the set of candidate indexers is identified using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="212c8-1138">Si no se puede identificar un único indexador mejor, el acceso del indexador es ambiguo y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1138">If a single best indexer cannot be identified, the indexer access is ambiguous, and a binding-time error occurs.</span></span>
*  <span data-ttu-id="212c8-1139">Las expresiones de índice de *argument_list* se evalúan en orden, de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="212c8-1139">The index expressions of the *argument_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="212c8-1140">El resultado de procesar el acceso del indexador es una expresión clasificada como un acceso de indexador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1140">The result of processing the indexer access is an expression classified as an indexer access.</span></span> <span data-ttu-id="212c8-1141">La expresión de acceso de indexador hace referencia al indizador determinado en el paso anterior y tiene una expresión de instancia asociada de `P` y una lista de argumentos asociada de `A`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1141">The indexer access expression references the indexer determined in the step above, and has an associated instance expression of `P` and an associated argument list of `A`.</span></span>

<span data-ttu-id="212c8-1142">En función del contexto en el que se utiliza, un acceso de indexador provoca la invocación del *descriptor* de acceso get o del *descriptor* de acceso set del indexador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1142">Depending on the context in which it is used, an indexer access causes invocation of either the *get accessor* or the *set accessor* of the indexer.</span></span> <span data-ttu-id="212c8-1143">Si el acceso del indizador es el destino de una asignación, se invoca al *descriptor* de acceso set para asignar un nuevo valor ([asignación simple](expressions.md#simple-assignment)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1143">If the indexer access is the target of an assignment, the *set accessor* is invoked to assign a new value ([Simple assignment](expressions.md#simple-assignment)).</span></span> <span data-ttu-id="212c8-1144">En todos los demás casos, el *descriptor de acceso get* se invoca para obtener el valor actual ([valores de las expresiones](expressions.md#values-of-expressions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1144">In all other cases, the *get accessor* is invoked to obtain the current value ([Values of expressions](expressions.md#values-of-expressions)).</span></span>

### <a name="this-access"></a><span data-ttu-id="212c8-1145">este acceso</span><span class="sxs-lookup"><span data-stu-id="212c8-1145">This access</span></span>

<span data-ttu-id="212c8-1146">Un *this_access* consta de la palabra reservada `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1146">A *this_access* consists of the reserved word `this`.</span></span>

```antlr
this_access
    : 'this'
    ;
```

<span data-ttu-id="212c8-1147">Un *this_access* solo se permite en el *bloque* de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia.</span><span class="sxs-lookup"><span data-stu-id="212c8-1147">A *this_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="212c8-1148">Tiene uno de los significados siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-1148">It has one of the following meanings:</span></span>

*  <span data-ttu-id="212c8-1149">Cuando se usa `this` en un *primary_expression* dentro de un constructor de instancia de una clase, se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-1149">When `this` is used in a *primary_expression* within an instance constructor of a class, it is classified as a value.</span></span> <span data-ttu-id="212c8-1150">El tipo del valor es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la clase en la que se produce el uso y el valor es una referencia al objeto que se está construyendo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1150">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object being constructed.</span></span>
*  <span data-ttu-id="212c8-1151">Cuando se usa `this` en un *primary_expression* dentro de un método de instancia o un descriptor de acceso de instancia de una clase, se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-1151">When `this` is used in a *primary_expression* within an instance method or instance accessor of a class, it is classified as a value.</span></span> <span data-ttu-id="212c8-1152">El tipo del valor es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la clase en la que se produce el uso y el valor es una referencia al objeto para el que se invocó el método o el descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="212c8-1152">The type of the value is the instance type ([The instance type](classes.md#the-instance-type)) of the class within which the usage occurs, and the value is a reference to the object for which the method or accessor was invoked.</span></span>
*  <span data-ttu-id="212c8-1153">Cuando se usa `this` en un *primary_expression* dentro de un constructor de instancia de un struct, se clasifica como una variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-1153">When `this` is used in a *primary_expression* within an instance constructor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="212c8-1154">El tipo de la variable es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) del struct en el que se produce el uso y la variable representa el struct que se está construyendo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1154">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs, and the variable represents the struct being constructed.</span></span> <span data-ttu-id="212c8-1155">La variable `this` de un constructor de instancia de una estructura se comporta exactamente igual que un parámetro `out` del tipo struct, en concreto, esto significa que la variable debe estar asignada definitivamente en todas las rutas de acceso de ejecución del constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="212c8-1155">The `this` variable of an instance constructor of a struct behaves exactly the same as an `out` parameter of the struct type—in particular, this means that the variable must be definitely assigned in every execution path of the instance constructor.</span></span>
*  <span data-ttu-id="212c8-1156">Cuando se usa `this` en un *primary_expression* dentro de un método de instancia o un descriptor de acceso de instancia de un struct, se clasifica como una variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-1156">When `this` is used in a *primary_expression* within an instance method or instance accessor of a struct, it is classified as a variable.</span></span> <span data-ttu-id="212c8-1157">El tipo de la variable es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) del struct en el que se produce el uso.</span><span class="sxs-lookup"><span data-stu-id="212c8-1157">The type of the variable is the instance type ([The instance type](classes.md#the-instance-type)) of the struct within which the usage occurs.</span></span>
   * <span data-ttu-id="212c8-1158">Si el método o el descriptor de acceso no es un iterador ([iteradores](classes.md#iterators)), la variable `this` representa la estructura para la que se invocó el método o descriptor de acceso, y se comporta exactamente igual que un parámetro `ref` del tipo struct.</span><span class="sxs-lookup"><span data-stu-id="212c8-1158">If the method or accessor is not an iterator ([Iterators](classes.md#iterators)), the `this` variable represents the struct for which the method or accessor was invoked, and behaves exactly the same as a `ref` parameter of the struct type.</span></span>
   * <span data-ttu-id="212c8-1159">Si el método o descriptor de acceso es un iterador, la variable `this` representa una copia del struct para el que se invocó el método o descriptor de acceso, y se comporta exactamente igual que un parámetro de valor del tipo de estructura.</span><span class="sxs-lookup"><span data-stu-id="212c8-1159">If the method or accessor is an iterator, the `this` variable represents a copy of the struct for which the method or accessor was invoked, and behaves exactly the same as a value parameter of the struct type.</span></span>

<span data-ttu-id="212c8-1160">El uso de `this` en un *primary_expression* en un contexto distinto de los enumerados anteriormente es un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1160">Use of `this` in a *primary_expression* in a context other than the ones listed above is a compile-time error.</span></span> <span data-ttu-id="212c8-1161">En concreto, no es posible hacer referencia a `this` en un método estático, un descriptor de acceso de propiedad estática o en una *variable_initializer* de una declaración de campo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1161">In particular, it is not possible to refer to `this` in a static method, a static property accessor, or in a *variable_initializer* of a field declaration.</span></span>

### <a name="base-access"></a><span data-ttu-id="212c8-1162">Acceso base</span><span class="sxs-lookup"><span data-stu-id="212c8-1162">Base access</span></span>

<span data-ttu-id="212c8-1163">Un *base_access* consta de la palabra reservada `base` seguido de un token "`.`" y un identificador o un *argument_list* entre corchetes:</span><span class="sxs-lookup"><span data-stu-id="212c8-1163">A *base_access* consists of the reserved word `base` followed by either a "`.`" token and an identifier or an *argument_list* enclosed in square brackets:</span></span>

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

<span data-ttu-id="212c8-1164">Un *base_access* se usa para tener acceso a los miembros de clase base que están ocultos por miembros con el mismo nombre en la clase o el struct actual.</span><span class="sxs-lookup"><span data-stu-id="212c8-1164">A *base_access* is used to access base class members that are hidden by similarly named members in the current class or struct.</span></span> <span data-ttu-id="212c8-1165">Un *base_access* solo se permite en el *bloque* de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia.</span><span class="sxs-lookup"><span data-stu-id="212c8-1165">A *base_access* is permitted only in the *block* of an instance constructor, an instance method, or an instance accessor.</span></span> <span data-ttu-id="212c8-1166">Cuando `base.I` se produce en una clase o struct, `I` debe indicar un miembro de la clase base de esa clase o estructura.</span><span class="sxs-lookup"><span data-stu-id="212c8-1166">When `base.I` occurs in a class or struct, `I` must denote a member of the base class of that class or struct.</span></span> <span data-ttu-id="212c8-1167">Del mismo modo, cuando `base[E]` se produce en una clase, debe existir un indexador aplicable en la clase base.</span><span class="sxs-lookup"><span data-stu-id="212c8-1167">Likewise, when `base[E]` occurs in a class, an applicable indexer must exist in the base class.</span></span>

<span data-ttu-id="212c8-1168">En tiempo de enlace, las expresiones *base_access* de la forma `base.I` y `base[E]` se evalúan exactamente como si se hubieran escrito `((B)this).I` y `((B)this)[E]`, donde `B` es la clase base de la clase o estructura en la que se produce la construcción.</span><span class="sxs-lookup"><span data-stu-id="212c8-1168">At binding-time, *base_access* expressions of the form `base.I` and `base[E]` are evaluated exactly as if they were written `((B)this).I` and `((B)this)[E]`, where `B` is the base class of the class or struct in which the construct occurs.</span></span> <span data-ttu-id="212c8-1169">Por lo tanto, `base.I` y `base[E]` se corresponden con `this.I` y `this[E]`, excepto que `this` se ve como una instancia de la clase base.</span><span class="sxs-lookup"><span data-stu-id="212c8-1169">Thus, `base.I` and `base[E]` correspond to `this.I` and `this[E]`, except `this` is viewed as an instance of the base class.</span></span>

<span data-ttu-id="212c8-1170">Cuando un *base_access* hace referencia a un miembro de función virtual (un método, una propiedad o un indizador), se cambia la determinación del miembro de función que se va a invocar en tiempo de ejecución ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1170">When a *base_access* references a virtual function member (a method, property, or indexer), the determination of which function member to invoke at run-time ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) is changed.</span></span> <span data-ttu-id="212c8-1171">El miembro de función que se invoca se determina mediante la búsqueda de la implementación más derivada ([métodos virtuales](classes.md#virtual-methods)) del miembro de función con respecto a `B` (en lugar de con respecto al tipo en tiempo de ejecución de `this`, como sería habitual en un acceso no base). .</span><span class="sxs-lookup"><span data-stu-id="212c8-1171">The function member that is invoked is determined by finding the most derived implementation ([Virtual methods](classes.md#virtual-methods)) of the function member with respect to `B` (instead of with respect to the run-time type of `this`, as would be usual in a non-base access).</span></span> <span data-ttu-id="212c8-1172">Por lo tanto, dentro de un `override` de un miembro de función `virtual`, se puede usar una *base_access* para invocar la implementación heredada del miembro de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-1172">Thus, within an `override` of a `virtual` function member, a *base_access* can be used to invoke the inherited implementation of the function member.</span></span> <span data-ttu-id="212c8-1173">Si el miembro de función al que hace referencia un *base_access* es abstracto, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1173">If the function member referenced by a *base_access* is abstract, a binding-time error occurs.</span></span>

### <a name="postfix-increment-and-decrement-operators"></a><span data-ttu-id="212c8-1174">Operadores de incremento y decremento posfijos</span><span class="sxs-lookup"><span data-stu-id="212c8-1174">Postfix increment and decrement operators</span></span>

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

<span data-ttu-id="212c8-1175">El operando de una operación de incremento o decremento postfijo debe ser una expresión clasificada como una variable, un acceso de propiedad o un acceso de indexador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1175">The operand of a postfix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="212c8-1176">El resultado de la operación es un valor del mismo tipo que el operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-1176">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="212c8-1177">Si *primary_expression* tiene el tipo en tiempo de compilación `dynamic`, el operador está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)), *post_increment_expression* o *post_decrement_expression* tiene el tipo en tiempo de compilación `dynamic`. y las siguientes reglas se aplican en tiempo de ejecución mediante el tipo en tiempo de ejecución de *primary_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1177">If the *primary_expression* has the compile-time type `dynamic` then the operator is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), the *post_increment_expression* or *post_decrement_expression* has the compile-time type `dynamic` and the following rules are applied at run-time using the run-time type of the *primary_expression*.</span></span>

<span data-ttu-id="212c8-1178">Si el operando de una operación de incremento o decremento postfijo es una propiedad o un indexador, la propiedad o el indexador deben tener un descriptor de acceso `get` y `set`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1178">If the operand of a postfix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="212c8-1179">Si no es así, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1179">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-1180">La resolución de sobrecargas de operador unario ([resolución de sobrecarga de operadores unarios](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1180">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-1181">Los operadores predefinidos `++` y `--` existen para los siguientes tipos: @no__t 2, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 y cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="212c8-1181">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="212c8-1182">Los operadores predefinidos `++` devuelven el valor generado agregando 1 al operando y los operadores `--` predefinidos devuelven el valor generado restando 1 del operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-1182">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="212c8-1183">En un contexto `checked`, si el resultado de esta suma o resta está fuera del intervalo del tipo de resultado y el tipo de resultado es un tipo entero o un tipo de enumeración, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1183">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="212c8-1184">El procesamiento en tiempo de ejecución de una operación de incremento o decremento postfijo con el formato `x++` o `x--` consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1184">The run-time processing of a postfix increment or decrement operation of the form `x++` or `x--` consists of the following steps:</span></span>

*   <span data-ttu-id="212c8-1185">Si `x` está clasificado como una variable:</span><span class="sxs-lookup"><span data-stu-id="212c8-1185">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="212c8-1186">`x` se evalúa para generar la variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-1186">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="212c8-1187">Se guarda el valor de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1187">The value of `x` is saved.</span></span>
    * <span data-ttu-id="212c8-1188">El operador seleccionado se invoca con el valor guardado de `x` como argumento.</span><span class="sxs-lookup"><span data-stu-id="212c8-1188">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="212c8-1189">El valor devuelto por el operador se almacena en la ubicación proporcionada por la evaluación de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1189">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="212c8-1190">El valor guardado de `x` se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1190">The saved value of `x` becomes the result of the operation.</span></span>
*   <span data-ttu-id="212c8-1191">Si `x` se clasifica como un acceso de propiedad o indizador:</span><span class="sxs-lookup"><span data-stu-id="212c8-1191">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="212c8-1192">La expresión de instancia (si `x` no es `static`) y la lista de argumentos (si `x` es un indexador) asociada a `x` se evalúan, y los resultados se usan en las siguientes invocaciones de descriptor de acceso `get` y `set`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1192">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="212c8-1193">Se invoca al descriptor de acceso `get` de `x` y se guarda el valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="212c8-1193">The `get` accessor of `x` is invoked and the returned value is saved.</span></span>
    * <span data-ttu-id="212c8-1194">El operador seleccionado se invoca con el valor guardado de `x` como argumento.</span><span class="sxs-lookup"><span data-stu-id="212c8-1194">The selected operator is invoked with the saved value of `x` as its argument.</span></span>
    * <span data-ttu-id="212c8-1195">El descriptor de acceso `set` de `x` se invoca con el valor devuelto por el operador como su argumento `value`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1195">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="212c8-1196">El valor guardado de `x` se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1196">The saved value of `x` becomes the result of the operation.</span></span>

<span data-ttu-id="212c8-1197">Los operadores `++` y `--` también admiten la notación de prefijo ([operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1197">The `++` and `--` operators also support prefix notation ([Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="212c8-1198">Normalmente, el resultado de `x++` o `x--` es el valor de @no__t 2 antes de la operación, mientras que el resultado de `++x` o `--x` es el valor de `x` después de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1198">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="212c8-1199">En cualquier caso, `x` tiene el mismo valor después de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1199">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="212c8-1200">Se puede invocar una implementación `operator ++` o `operator --` mediante la notación de sufijo o de prefijo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1200">An `operator ++` or `operator --` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="212c8-1201">No es posible tener implementaciones de operador independientes para las dos notaciones.</span><span class="sxs-lookup"><span data-stu-id="212c8-1201">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="the-new-operator"></a><span data-ttu-id="212c8-1202">Operador new</span><span class="sxs-lookup"><span data-stu-id="212c8-1202">The new operator</span></span>

<span data-ttu-id="212c8-1203">El operador `new` se usa para crear nuevas instancias de tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1203">The `new` operator is used to create new instances of types.</span></span>

<span data-ttu-id="212c8-1204">Hay tres formas de expresiones `new`:</span><span class="sxs-lookup"><span data-stu-id="212c8-1204">There are three forms of `new` expressions:</span></span>

*  <span data-ttu-id="212c8-1205">Las expresiones de creación de objetos se utilizan para crear nuevas instancias de tipos de clase y tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-1205">Object creation expressions are used to create new instances of class types and value types.</span></span>
*  <span data-ttu-id="212c8-1206">Las expresiones de creación de matrices se utilizan para crear nuevas instancias de tipos de matriz.</span><span class="sxs-lookup"><span data-stu-id="212c8-1206">Array creation expressions are used to create new instances of array types.</span></span>
*  <span data-ttu-id="212c8-1207">Las expresiones de creación de delegados se utilizan para crear nuevas instancias de tipos de delegado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1207">Delegate creation expressions are used to create new instances of delegate types.</span></span>

<span data-ttu-id="212c8-1208">El operador `new` implica la creación de una instancia de un tipo, pero no implica necesariamente la asignación dinámica de memoria.</span><span class="sxs-lookup"><span data-stu-id="212c8-1208">The `new` operator implies creation of an instance of a type, but does not necessarily imply dynamic allocation of memory.</span></span> <span data-ttu-id="212c8-1209">En concreto, las instancias de tipos de valor no requieren memoria adicional más allá de las variables en las que residen, y no se producen asignaciones dinámicas cuando `new` se usa para crear instancias de tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-1209">In particular, instances of value types require no additional memory beyond the variables in which they reside, and no dynamic allocations occur when `new` is used to create instances of value types.</span></span>

#### <a name="object-creation-expressions"></a><span data-ttu-id="212c8-1210">Expresiones de creación de objetos</span><span class="sxs-lookup"><span data-stu-id="212c8-1210">Object creation expressions</span></span>

<span data-ttu-id="212c8-1211">Se usa un *object_creation_expression* para crear una nueva instancia de *class_type* o *value_type*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1211">An *object_creation_expression* is used to create a new instance of a *class_type* or a *value_type*.</span></span>

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

<span data-ttu-id="212c8-1212">El *tipo* de *object_creation_expression* debe ser *class_type*, *value_type* o *type_parameter*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1212">The *type* of an *object_creation_expression* must be a *class_type*, a *value_type* or a *type_parameter*.</span></span> <span data-ttu-id="212c8-1213">El *tipo* no puede ser `abstract` *class_type*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1213">The *type* cannot be an `abstract` *class_type*.</span></span>

<span data-ttu-id="212c8-1214">Solo se permite *argument_list* ([listas de argumentos](expressions.md#argument-lists)) opcionales si el *tipo* es *class_type* o *struct_type*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1214">The optional *argument_list* ([Argument lists](expressions.md#argument-lists)) is permitted only if the *type* is a *class_type* or a *struct_type*.</span></span>

<span data-ttu-id="212c8-1215">Una expresión de creación de objeto puede omitir la lista de argumentos del constructor y los paréntesis de cierre especificados que incluye un inicializador de objeto o un inicializador de colección.</span><span class="sxs-lookup"><span data-stu-id="212c8-1215">An object creation expression can omit the constructor argument list and enclosing parentheses provided it includes an object initializer or collection initializer.</span></span> <span data-ttu-id="212c8-1216">Omitir la lista de argumentos del constructor y los paréntesis de inclusión equivale a especificar una lista de argumentos vacía.</span><span class="sxs-lookup"><span data-stu-id="212c8-1216">Omitting the constructor argument list and enclosing parentheses is equivalent to specifying an empty argument list.</span></span>

<span data-ttu-id="212c8-1217">El procesamiento de una expresión de creación de objetos que incluye un inicializador de objeto o un inicializador de colección consiste en procesar primero el constructor de instancia y después procesar las inicializaciones de miembro o elemento especificadas por el inicializador de objeto ([ Inicializadores de objeto](expressions.md#object-initializers)) o inicializadores de colección ([inicializadores de colección](expressions.md#collection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1217">Processing of an object creation expression that includes an object initializer or collection initializer consists of first processing the instance constructor and then processing the member or element initializations specified by the object initializer ([Object initializers](expressions.md#object-initializers)) or collection initializer ([Collection initializers](expressions.md#collection-initializers)).</span></span>

<span data-ttu-id="212c8-1218">Si alguno de los argumentos del *argument_list* opcional tiene el tipo en tiempo de compilación `dynamic`, el *object_creation_expression* está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) y se aplican las siguientes reglas en tiempo de ejecución mediante el tiempo de ejecución. tipo de los argumentos de *argument_list* que tienen el tipo de tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1218">If any of the arguments in the optional *argument_list* has the compile-time type `dynamic` then the *object_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)) and the following rules are applied at run-time using the run-time type of those arguments of the *argument_list* that have the compile time type `dynamic`.</span></span> <span data-ttu-id="212c8-1219">Sin embargo, la creación de objetos sufre una comprobación de tiempo de compilación limitada, como se describe en [comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="212c8-1219">However, the object creation undergoes a limited compile time check as described in [Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution).</span></span>

<span data-ttu-id="212c8-1220">El procesamiento en tiempo de enlace de un *object_creation_expression* del formulario `new T(A)`, donde `T` es un *class_type* o un *value_type* y `A` es un *argument_list*opcional, consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1220">The binding-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is a *class_type* or a *value_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="212c8-1221">Si `T` es *value_type* y `A` no está presente:</span><span class="sxs-lookup"><span data-stu-id="212c8-1221">If `T` is a *value_type* and `A` is not present:</span></span>
    * <span data-ttu-id="212c8-1222">*Object_creation_expression* es una invocación de constructor predeterminada.</span><span class="sxs-lookup"><span data-stu-id="212c8-1222">The *object_creation_expression* is a default constructor invocation.</span></span> <span data-ttu-id="212c8-1223">El resultado de *object_creation_expression* es un valor de tipo `T`, es decir, el valor predeterminado de `T` tal y como se define en [el tipo System. ValueType](types.md#the-systemvaluetype-type).</span><span class="sxs-lookup"><span data-stu-id="212c8-1223">The result of the *object_creation_expression* is a value of type `T`, namely the default value for `T` as defined in [The System.ValueType type](types.md#the-systemvaluetype-type).</span></span>
*   <span data-ttu-id="212c8-1224">De lo contrario, si `T` es *type_parameter* y `A` no está presente:</span><span class="sxs-lookup"><span data-stu-id="212c8-1224">Otherwise, if `T` is a *type_parameter* and `A` is not present:</span></span>
    * <span data-ttu-id="212c8-1225">Si no se ha especificado ninguna restricción de tipo de valor o restricción de constructor ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) para `T`, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1225">If no value type constraint or constructor constraint ([Type parameter constraints](classes.md#type-parameter-constraints)) has been specified for `T`, a binding-time error occurs.</span></span>
    * <span data-ttu-id="212c8-1226">El resultado de *object_creation_expression* es un valor del tipo en tiempo de ejecución al que se ha enlazado el parámetro de tipo, es decir, el resultado de invocar el constructor predeterminado de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1226">The result of the *object_creation_expression* is a value of the run-time type that the type parameter has been bound to, namely the result of invoking the default constructor of that type.</span></span> <span data-ttu-id="212c8-1227">El tipo en tiempo de ejecución puede ser un tipo de referencia o un tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-1227">The run-time type may be a reference type or a value type.</span></span>
*   <span data-ttu-id="212c8-1228">De lo contrario, si `T` es *class_type* o *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="212c8-1228">Otherwise, if `T` is a *class_type* or a *struct_type*:</span></span>
    * <span data-ttu-id="212c8-1229">Si `T` es un *class_type*`abstract`, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1229">If `T` is an `abstract` *class_type*, a compile-time error occurs.</span></span>
    * <span data-ttu-id="212c8-1230">El constructor de instancia que se va a invocar se determina mediante las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution).</span><span class="sxs-lookup"><span data-stu-id="212c8-1230">The instance constructor to invoke is determined using the overload resolution rules of [Overload resolution](expressions.md#overload-resolution).</span></span> <span data-ttu-id="212c8-1231">El conjunto de constructores de instancias candidatas se compone de todos los constructores de instancia accesibles declarados en `T`, que son aplicables con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1231">The set of candidate instance constructors consists of all accessible instance constructors declared in `T` which are applicable with respect to `A` ([Applicable function member](expressions.md#applicable-function-member)).</span></span> <span data-ttu-id="212c8-1232">Si el conjunto de constructores de instancia candidata está vacío, o si no se puede identificar un único constructor de instancia mejor, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1232">If the set of candidate instance constructors is empty, or if a single best instance constructor cannot be identified, a binding-time error occurs.</span></span>
    * <span data-ttu-id="212c8-1233">El resultado de *object_creation_expression* es un valor de tipo `T`, es decir, el valor generado al invocar el constructor de instancia determinado en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="212c8-1233">The result of the *object_creation_expression* is a value of type `T`, namely the value produced by invoking the instance constructor determined in the step above.</span></span>
*  <span data-ttu-id="212c8-1234">De lo contrario, el *object_creation_expression* no es válido y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1234">Otherwise, the *object_creation_expression* is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-1235">Aunque el *object_creation_expression* esté enlazado dinámicamente, el tipo en tiempo de compilación sigue siendo `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1235">Even if the *object_creation_expression* is dynamically bound, the compile-time type is still `T`.</span></span>

<span data-ttu-id="212c8-1236">El procesamiento en tiempo de ejecución de un *object_creation_expression* con el formato `new T(A)`, donde `T` es *class_type* o *struct_type* y `A` es un *argument_list*opcional, consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1236">The run-time processing of an *object_creation_expression* of the form `new T(A)`, where `T` is *class_type* or a *struct_type* and `A` is an optional *argument_list*, consists of the following steps:</span></span>

*   <span data-ttu-id="212c8-1237">Si `T` es *class_type*:</span><span class="sxs-lookup"><span data-stu-id="212c8-1237">If `T` is a *class_type*:</span></span>
    * <span data-ttu-id="212c8-1238">Se asigna una nueva instancia de la clase `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1238">A new instance of class `T` is allocated.</span></span> <span data-ttu-id="212c8-1239">Si no hay suficiente memoria disponible para asignar la nueva instancia, se produce una `System.OutOfMemoryException` y no se ejecuta ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="212c8-1239">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="212c8-1240">Todos los campos de la nueva instancia se inicializan con sus valores predeterminados ([valores predeterminados](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1240">All fields of the new instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
    * <span data-ttu-id="212c8-1241">El constructor de instancia se invoca de acuerdo con las reglas de invocación de miembros de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1241">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="212c8-1242">Se pasa automáticamente una referencia a la instancia recién asignada al constructor de instancia y se puede tener acceso a la instancia desde dentro de ese constructor como `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1242">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>
*   <span data-ttu-id="212c8-1243">Si `T` es *struct_type*:</span><span class="sxs-lookup"><span data-stu-id="212c8-1243">If `T` is a *struct_type*:</span></span>
    * <span data-ttu-id="212c8-1244">Se crea una instancia de tipo `T` mediante la asignación de una variable local temporal.</span><span class="sxs-lookup"><span data-stu-id="212c8-1244">An instance of type `T` is created by allocating a temporary local variable.</span></span> <span data-ttu-id="212c8-1245">Dado que se requiere un constructor de instancia de un *struct_type* para asignar definitivamente un valor a cada campo de la instancia que se va a crear, no es necesaria la inicialización de la variable temporal.</span><span class="sxs-lookup"><span data-stu-id="212c8-1245">Since an instance constructor of a *struct_type* is required to definitely assign a value to each field of the instance being created, no initialization of the temporary variable is necessary.</span></span>
    * <span data-ttu-id="212c8-1246">El constructor de instancia se invoca de acuerdo con las reglas de invocación de miembros de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1246">The instance constructor is invoked according to the rules of function member invocation ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).</span></span> <span data-ttu-id="212c8-1247">Se pasa automáticamente una referencia a la instancia recién asignada al constructor de instancia y se puede tener acceso a la instancia desde dentro de ese constructor como `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1247">A reference to the newly allocated instance is automatically passed to the instance constructor and the instance can be accessed from within that constructor as `this`.</span></span>

#### <a name="object-initializers"></a><span data-ttu-id="212c8-1248">Inicializadores de objeto</span><span class="sxs-lookup"><span data-stu-id="212c8-1248">Object initializers</span></span>

<span data-ttu-id="212c8-1249">Un ***inicializador de objeto*** especifica valores para cero o más campos, propiedades o elementos indizados de un objeto.</span><span class="sxs-lookup"><span data-stu-id="212c8-1249">An ***object initializer*** specifies values for zero or more fields, properties or indexed elements of an object.</span></span>

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

<span data-ttu-id="212c8-1250">Un inicializador de objeto consta de una secuencia de inicializadores de miembro, delimitada por los tokens `{` y `}` y separados por comas.</span><span class="sxs-lookup"><span data-stu-id="212c8-1250">An object initializer consists of a sequence of member initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="212c8-1251">Cada *member_initializer* designa un destino para la inicialización.</span><span class="sxs-lookup"><span data-stu-id="212c8-1251">Each *member_initializer* designates a target for the initialization.</span></span> <span data-ttu-id="212c8-1252">Un *identificador* debe nombrar un campo o propiedad accesible del objeto que se va a inicializar, mientras que un *argument_list* entre corchetes debe especificar los argumentos de un indizador accesible en el objeto que se va a inicializar.</span><span class="sxs-lookup"><span data-stu-id="212c8-1252">An *identifier* must name an accessible field or property of the object being initialized, whereas an *argument_list* enclosed in square brackets must specify arguments for an accessible indexer on the object being initialized.</span></span> <span data-ttu-id="212c8-1253">Es un error que un inicializador de objeto incluya más de un inicializador de miembro para el mismo campo o propiedad.</span><span class="sxs-lookup"><span data-stu-id="212c8-1253">It is an error for an object initializer to include more than one member initializer for the same field or property.</span></span>

<span data-ttu-id="212c8-1254">Cada *initializer_target* va seguido de un signo igual y una expresión, un inicializador de objeto o un inicializador de colección.</span><span class="sxs-lookup"><span data-stu-id="212c8-1254">Each *initializer_target* is followed by an equals sign and either an expression, an object initializer or a collection initializer.</span></span> <span data-ttu-id="212c8-1255">No es posible que las expresiones del inicializador de objeto hagan referencia al objeto recién creado que se está inicializando.</span><span class="sxs-lookup"><span data-stu-id="212c8-1255">It is not possible for expressions within the object initializer to refer to the newly created object it is initializing.</span></span>

<span data-ttu-id="212c8-1256">Inicializador de miembro que especifica una expresión después de que el signo igual se procese de la misma manera que una asignación ([asignación simple](expressions.md#simple-assignment)) al destino.</span><span class="sxs-lookup"><span data-stu-id="212c8-1256">A member initializer that specifies an expression after the equals sign is processed in the same way as an assignment ([Simple assignment](expressions.md#simple-assignment)) to the target.</span></span>

<span data-ttu-id="212c8-1257">Inicializador de miembro que especifica un inicializador de objeto después de que el signo igual sea un ***inicializador de objeto anidado***, es decir, una inicialización de un objeto incrustado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1257">A member initializer that specifies an object initializer after the equals sign is a ***nested object initializer***, i.e. an initialization of an embedded object.</span></span> <span data-ttu-id="212c8-1258">En lugar de asignar un nuevo valor al campo o propiedad, las asignaciones en el inicializador de objeto anidado se tratan como asignaciones a los miembros del campo o de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="212c8-1258">Instead of assigning a new value to the field or property, the assignments in the nested object initializer are treated as assignments to members of the field or property.</span></span> <span data-ttu-id="212c8-1259">Los inicializadores de objeto anidados no se pueden aplicar a las propiedades con un tipo de valor o a los campos de solo lectura con un tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-1259">Nested object initializers cannot be applied to properties with a value type, or to read-only fields with a value type.</span></span>

<span data-ttu-id="212c8-1260">Inicializador de miembro que especifica un inicializador de colección después de que el signo igual sea una inicialización de una colección incrustada.</span><span class="sxs-lookup"><span data-stu-id="212c8-1260">A member initializer that specifies a collection initializer after the equals sign is an initialization of an embedded collection.</span></span> <span data-ttu-id="212c8-1261">En lugar de asignar una nueva colección al campo de destino, propiedad o indizador, los elementos proporcionados en el inicializador se agregan a la colección a la que hace referencia el destino.</span><span class="sxs-lookup"><span data-stu-id="212c8-1261">Instead of assigning a new collection to the target field, property or indexer, the elements given in the initializer are added to the collection referenced by the target.</span></span> <span data-ttu-id="212c8-1262">El destino debe ser de un tipo de colección que satisfaga los requisitos especificados en los [inicializadores de colección](expressions.md#collection-initializers).</span><span class="sxs-lookup"><span data-stu-id="212c8-1262">The target must be of a collection type that satisfies the requirements specified in [Collection initializers](expressions.md#collection-initializers).</span></span>

<span data-ttu-id="212c8-1263">Los argumentos de un inicializador de índice siempre se evaluarán exactamente una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-1263">The arguments to an index initializer will always be evaluated exactly once.</span></span> <span data-ttu-id="212c8-1264">Por lo tanto, aunque los argumentos acaben nunca en usarse (por ejemplo, debido a un inicializador anidado vacío), se evaluarán por sus efectos secundarios.</span><span class="sxs-lookup"><span data-stu-id="212c8-1264">Thus, even if the arguments end up never getting used (e.g. because of an empty nested initializer), they will be evaluated for their side effects.</span></span>

<span data-ttu-id="212c8-1265">La clase siguiente representa un punto con dos coordenadas:</span><span class="sxs-lookup"><span data-stu-id="212c8-1265">The following class represents a point with two coordinates:</span></span>
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

<span data-ttu-id="212c8-1266">Se puede crear una instancia de `Point` e inicializarla de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-1266">An instance of `Point` can be created and initialized as follows:</span></span>
```csharp
Point a = new Point { X = 0, Y = 1 };
```
<span data-ttu-id="212c8-1267">que tiene el mismo efecto que</span><span class="sxs-lookup"><span data-stu-id="212c8-1267">which has the same effect as</span></span>
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
<span data-ttu-id="212c8-1268">donde `__a` es una variable temporal invisible e inaccesible en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="212c8-1268">where `__a` is an otherwise invisible and inaccessible temporary variable.</span></span> <span data-ttu-id="212c8-1269">La clase siguiente representa un rectángulo creado a partir de dos puntos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1269">The following class represents a rectangle created from two points:</span></span>
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

<span data-ttu-id="212c8-1270">Se puede crear una instancia de `Rectangle` e inicializarla de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-1270">An instance of `Rectangle` can be created and initialized as follows:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
<span data-ttu-id="212c8-1271">que tiene el mismo efecto que</span><span class="sxs-lookup"><span data-stu-id="212c8-1271">which has the same effect as</span></span>
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
<span data-ttu-id="212c8-1272">donde `__r`, `__p1` y `__p2` son variables temporales que, de lo contrario, son invisibles e inaccesibles.</span><span class="sxs-lookup"><span data-stu-id="212c8-1272">where `__r`, `__p1` and `__p2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="212c8-1273">Si el constructor de `Rectangle` asigna las dos instancias de `Point` incrustadas</span><span class="sxs-lookup"><span data-stu-id="212c8-1273">If `Rectangle`'s constructor allocates the two embedded `Point` instances</span></span>
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
<span data-ttu-id="212c8-1274">la construcción siguiente se puede utilizar para inicializar las instancias de `Point` incrustadas en lugar de asignar nuevas instancias:</span><span class="sxs-lookup"><span data-stu-id="212c8-1274">the following construct can be used to initialize the embedded `Point` instances instead of assigning new instances:</span></span>
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
<span data-ttu-id="212c8-1275">que tiene el mismo efecto que</span><span class="sxs-lookup"><span data-stu-id="212c8-1275">which has the same effect as</span></span>
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

<span data-ttu-id="212c8-1276">Dada una definición adecuada de C, el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-1276">Given an appropriate definition of C, the following example:</span></span>
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
<span data-ttu-id="212c8-1277">es equivalente a esta serie de asignaciones:</span><span class="sxs-lookup"><span data-stu-id="212c8-1277">is equivalent to this series of assignments:</span></span>
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
<span data-ttu-id="212c8-1278">donde `__c`, etc., son variables generadas que son invisibles e inaccesibles para el código fuente.</span><span class="sxs-lookup"><span data-stu-id="212c8-1278">where `__c`, etc., are generated variables that are invisible and inaccessible to the source code.</span></span> <span data-ttu-id="212c8-1279">Tenga en cuenta que los argumentos de `[0,0]` solo se evalúan una vez, y los argumentos de `[1,2]` se evalúan una vez aunque nunca se usan.</span><span class="sxs-lookup"><span data-stu-id="212c8-1279">Note that the arguments for `[0,0]` are evaluated only once, and the arguments for `[1,2]` are evaluated once even though they are never used.</span></span>

#### <a name="collection-initializers"></a><span data-ttu-id="212c8-1280">Inicializadores de colección</span><span class="sxs-lookup"><span data-stu-id="212c8-1280">Collection initializers</span></span>

<span data-ttu-id="212c8-1281">Un inicializador de colección especifica los elementos de una colección.</span><span class="sxs-lookup"><span data-stu-id="212c8-1281">A collection initializer specifies the elements of a collection.</span></span>

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

<span data-ttu-id="212c8-1282">Un inicializador de colección consta de una secuencia de inicializadores de elemento, delimitada por los tokens `{` y `}` y separados por comas.</span><span class="sxs-lookup"><span data-stu-id="212c8-1282">A collection initializer consists of a sequence of element initializers, enclosed by `{` and `}` tokens and separated by commas.</span></span> <span data-ttu-id="212c8-1283">Cada inicializador de elemento especifica un elemento que se va a agregar al objeto de colección que se va a inicializar y consta de una lista de expresiones delimitadas por los tokens `{` y `}` y separados por comas.</span><span class="sxs-lookup"><span data-stu-id="212c8-1283">Each element initializer specifies an element to be added to the collection object being initialized, and consists of a list of expressions enclosed by `{` and `}` tokens and separated by commas.</span></span>  <span data-ttu-id="212c8-1284">Un inicializador de elemento de una sola expresión se puede escribir sin llaves, pero no puede ser una expresión de asignación para evitar la ambigüedad con los inicializadores de miembro.</span><span class="sxs-lookup"><span data-stu-id="212c8-1284">A single-expression element initializer can be written without braces, but cannot then be an assignment expression, to avoid ambiguity with member initializers.</span></span> <span data-ttu-id="212c8-1285">La producción de *non_assignment_expression* se define en la [expresión](expressions.md#expression).</span><span class="sxs-lookup"><span data-stu-id="212c8-1285">The *non_assignment_expression* production is defined in [Expression](expressions.md#expression).</span></span>

<span data-ttu-id="212c8-1286">A continuación se muestra un ejemplo de una expresión de creación de objeto que incluye un inicializador de colección:</span><span class="sxs-lookup"><span data-stu-id="212c8-1286">The following is an example of an object creation expression that includes a collection initializer:</span></span>
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

<span data-ttu-id="212c8-1287">El objeto de colección al que se aplica un inicializador de colección debe ser de un tipo que implementa `System.Collections.IEnumerable` o se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1287">The collection object to which a collection initializer is applied must be of a type that implements `System.Collections.IEnumerable` or a compile-time error occurs.</span></span> <span data-ttu-id="212c8-1288">Para cada elemento especificado en orden, el inicializador de colección invoca un método `Add` en el objeto de destino con la lista de expresiones del inicializador de elemento como lista de argumentos, aplicando la búsqueda de miembros normal y la resolución de sobrecarga para cada invocación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1288">For each specified element in order, the collection initializer invokes an `Add` method on the target object with the expression list of the element initializer as argument list, applying normal member lookup and overload resolution for each invocation.</span></span> <span data-ttu-id="212c8-1289">Por lo tanto, el objeto de colección debe tener una instancia o un método de extensión aplicable con el nombre `Add` para cada inicializador de elemento.</span><span class="sxs-lookup"><span data-stu-id="212c8-1289">Thus, the collection object must have an applicable instance or extension method with the name `Add` for each element initializer.</span></span>

<span data-ttu-id="212c8-1290">La clase siguiente representa un contacto con un nombre y una lista de números de teléfono:</span><span class="sxs-lookup"><span data-stu-id="212c8-1290">The following class represents a contact with a name and a list of phone numbers:</span></span>
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

<span data-ttu-id="212c8-1291">Se puede crear un `List<Contact>` e inicializarlo como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-1291">A `List<Contact>` can be created and initialized as follows:</span></span>
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
<span data-ttu-id="212c8-1292">que tiene el mismo efecto que</span><span class="sxs-lookup"><span data-stu-id="212c8-1292">which has the same effect as</span></span>
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
<span data-ttu-id="212c8-1293">donde `__clist`, `__c1` y `__c2` son variables temporales que, de lo contrario, son invisibles e inaccesibles.</span><span class="sxs-lookup"><span data-stu-id="212c8-1293">where `__clist`, `__c1` and `__c2` are temporary variables that are otherwise invisible and inaccessible.</span></span>

#### <a name="array-creation-expressions"></a><span data-ttu-id="212c8-1294">Expresiones de creación de matrices</span><span class="sxs-lookup"><span data-stu-id="212c8-1294">Array creation expressions</span></span>

<span data-ttu-id="212c8-1295">Se usa un *array_creation_expression* para crear una nueva instancia de un *array_type*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1295">An *array_creation_expression* is used to create a new instance of an *array_type*.</span></span>

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

<span data-ttu-id="212c8-1296">Una expresión de creación de matriz del primer formulario asigna una instancia de matriz del tipo resultante de la eliminación de cada una de las expresiones individuales de la lista de expresiones.</span><span class="sxs-lookup"><span data-stu-id="212c8-1296">An array creation expression of the first form allocates an array instance of the type that results from deleting each of the individual expressions from the expression list.</span></span> <span data-ttu-id="212c8-1297">Por ejemplo, la expresión de creación de matriz `new int[10,20]` genera una instancia de matriz de tipo `int[,]` y la expresión de creación de matriz `new int[10][,]` genera una matriz de tipo `int[][,]`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1297">For example, the array creation expression `new int[10,20]` produces an array instance of type `int[,]`, and the array creation expression `new int[10][,]` produces an array of type `int[][,]`.</span></span> <span data-ttu-id="212c8-1298">Cada expresión de la lista de expresiones debe ser de tipo `int`, `uint`, `long` o `ulong`, o bien se puede convertir implícitamente a uno o varios de estos tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1298">Each expression in the expression list must be of type `int`, `uint`, `long`, or `ulong`, or implicitly convertible to one or more of these types.</span></span> <span data-ttu-id="212c8-1299">El valor de cada expresión determina la longitud de la dimensión correspondiente en la instancia de matriz recién asignada.</span><span class="sxs-lookup"><span data-stu-id="212c8-1299">The value of each expression determines the length of the corresponding dimension in the newly allocated array instance.</span></span> <span data-ttu-id="212c8-1300">Dado que la longitud de una dimensión de matriz no debe ser negativa, es un error en tiempo de compilación que tiene un *constant_expression* con un valor negativo en la lista de expresiones.</span><span class="sxs-lookup"><span data-stu-id="212c8-1300">Since the length of an array dimension must be nonnegative, it is a compile-time error to have a *constant_expression* with a negative value in the expression list.</span></span>

<span data-ttu-id="212c8-1301">Excepto en un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)), no se especifica el diseño de las matrices.</span><span class="sxs-lookup"><span data-stu-id="212c8-1301">Except in an unsafe context ([Unsafe contexts](unsafe-code.md#unsafe-contexts)), the layout of arrays is unspecified.</span></span>

<span data-ttu-id="212c8-1302">Si una expresión de creación de matriz del primer formulario incluye un inicializador de matriz, cada expresión de la lista de expresiones debe ser una constante y las longitudes de rango y dimensión especificadas por la lista de expresiones deben coincidir con las del inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="212c8-1302">If an array creation expression of the first form includes an array initializer, each expression in the expression list must be a constant and the rank and dimension lengths specified by the expression list must match those of the array initializer.</span></span>

<span data-ttu-id="212c8-1303">En una expresión de creación de matriz del segundo o tercer formulario, el rango del tipo de matriz o especificador de rango especificado debe coincidir con el del inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="212c8-1303">In an array creation expression of the second or third form, the rank of the specified array type or rank specifier must match that of the array initializer.</span></span> <span data-ttu-id="212c8-1304">Las longitudes de las dimensiones individuales se deducen del número de elementos de cada uno de los niveles de anidamiento correspondientes del inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="212c8-1304">The individual dimension lengths are inferred from the number of elements in each of the corresponding nesting levels of the array initializer.</span></span> <span data-ttu-id="212c8-1305">Por lo tanto, la expresión</span><span class="sxs-lookup"><span data-stu-id="212c8-1305">Thus, the expression</span></span>
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
<span data-ttu-id="212c8-1306">corresponde exactamente a</span><span class="sxs-lookup"><span data-stu-id="212c8-1306">exactly corresponds to</span></span>
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

<span data-ttu-id="212c8-1307">Se hace referencia a una expresión de creación de matriz del tercer formulario como una ***expresión de creación de matriz con tipo implícito***.</span><span class="sxs-lookup"><span data-stu-id="212c8-1307">An array creation expression of the third form is referred to as an ***implicitly typed array creation expression***.</span></span> <span data-ttu-id="212c8-1308">Es similar a la segunda forma, salvo que el tipo de elemento de la matriz no se proporciona explícitamente, pero se determina como el mejor tipo común ([encontrar el mejor tipo común de un conjunto de expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) del conjunto de expresiones en el inicializador de matriz.</span><span class="sxs-lookup"><span data-stu-id="212c8-1308">It is similar to the second form, except that the element type of the array is not explicitly given, but determined as the best common type ([Finding the best common type of a set of expressions](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) of the set of expressions in the array initializer.</span></span> <span data-ttu-id="212c8-1309">En el caso de una matriz multidimensional, es decir, una donde la *rank_specifier* contiene al menos una coma, este conjunto consta de todas las *expresiones*que se encuentran en los *array_initializer*anidados.</span><span class="sxs-lookup"><span data-stu-id="212c8-1309">For a multidimensional array, i.e., one where the *rank_specifier* contains at least one comma, this set comprises all *expression*s found in nested *array_initializer*s.</span></span>

<span data-ttu-id="212c8-1310">Los inicializadores de matriz se describen con más detalle en [inicializadores de matriz](arrays.md#array-initializers).</span><span class="sxs-lookup"><span data-stu-id="212c8-1310">Array initializers are described further in [Array initializers](arrays.md#array-initializers).</span></span>

<span data-ttu-id="212c8-1311">El resultado de evaluar una expresión de creación de matriz se clasifica como un valor, es decir, una referencia a la instancia de la matriz recién asignada.</span><span class="sxs-lookup"><span data-stu-id="212c8-1311">The result of evaluating an array creation expression is classified as a value, namely a reference to the newly allocated array instance.</span></span> <span data-ttu-id="212c8-1312">El procesamiento en tiempo de ejecución de una expresión de creación de matriz consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1312">The run-time processing of an array creation expression consists of the following steps:</span></span>

*  <span data-ttu-id="212c8-1313">Las expresiones de longitud de dimensión del *expression_list* se evalúan en orden, de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="212c8-1313">The dimension length expressions of the *expression_list* are evaluated in order, from left to right.</span></span> <span data-ttu-id="212c8-1314">Después de la evaluación de cada expresión, se realiza una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) en uno de los siguientes tipos: `int`, `uint`, `long`, @no__t 4.</span><span class="sxs-lookup"><span data-stu-id="212c8-1314">Following evaluation of each expression, an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) to one of the following types is performed: `int`, `uint`, `long`, `ulong`.</span></span> <span data-ttu-id="212c8-1315">Se elige el primer tipo de esta lista para el que existe una conversión implícita.</span><span class="sxs-lookup"><span data-stu-id="212c8-1315">The first type in this list for which an implicit conversion exists is chosen.</span></span> <span data-ttu-id="212c8-1316">Si la evaluación de una expresión o la conversión implícita subsiguiente produce una excepción, no se evalúan más expresiones y no se ejecuta ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="212c8-1316">If evaluation of an expression or the subsequent implicit conversion causes an exception, then no further expressions are evaluated and no further steps are executed.</span></span>
*  <span data-ttu-id="212c8-1317">Los valores calculados de las longitudes de dimensión se validan como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1317">The computed values for the dimension lengths are validated as follows.</span></span> <span data-ttu-id="212c8-1318">Si uno o varios de los valores son menores que cero, se produce una `System.OverflowException` y no se ejecuta ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="212c8-1318">If one or more of the values are less than zero, a `System.OverflowException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="212c8-1319">Se asigna una instancia de matriz con las longitudes de dimensión dadas.</span><span class="sxs-lookup"><span data-stu-id="212c8-1319">An array instance with the given dimension lengths is allocated.</span></span> <span data-ttu-id="212c8-1320">Si no hay suficiente memoria disponible para asignar la nueva instancia, se produce una `System.OutOfMemoryException` y no se ejecuta ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="212c8-1320">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
*  <span data-ttu-id="212c8-1321">Todos los elementos de la nueva instancia de matriz se inicializan con sus valores predeterminados ([valores predeterminados](variables.md#default-values)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1321">All elements of the new array instance are initialized to their default values ([Default values](variables.md#default-values)).</span></span>
*  <span data-ttu-id="212c8-1322">Si la expresión de creación de matriz contiene un inicializador de matriz, cada expresión del inicializador de matriz se evalúa y se asigna a su elemento de matriz correspondiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-1322">If the array creation expression contains an array initializer, then each expression in the array initializer is evaluated and assigned to its corresponding array element.</span></span> <span data-ttu-id="212c8-1323">Las evaluaciones y las asignaciones se realizan en el orden en el que se escriben las expresiones en el inicializador de matriz; es decir, los elementos se inicializan en el orden de índice creciente, con la dimensión situada más a la derecha aumentando primero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1323">The evaluations and assignments are performed in the order the expressions are written in the array initializer—in other words, elements are initialized in increasing index order, with the rightmost dimension increasing first.</span></span> <span data-ttu-id="212c8-1324">Si la evaluación de una expresión determinada o la asignación subsiguiente al elemento de matriz correspondiente produce una excepción, no se inicializa ningún otro elemento (y los demás elementos tendrán sus valores predeterminados).</span><span class="sxs-lookup"><span data-stu-id="212c8-1324">If evaluation of a given expression or the subsequent assignment to the corresponding array element causes an exception, then no further elements are initialized (and the remaining elements will thus have their default values).</span></span>

<span data-ttu-id="212c8-1325">Una expresión de creación de matriz permite la creación de instancias de una matriz con elementos de un tipo de matriz, pero los elementos de dicha matriz se deben inicializar manualmente.</span><span class="sxs-lookup"><span data-stu-id="212c8-1325">An array creation expression permits instantiation of an array with elements of an array type, but the elements of such an array must be manually initialized.</span></span> <span data-ttu-id="212c8-1326">Por ejemplo, la instrucción</span><span class="sxs-lookup"><span data-stu-id="212c8-1326">For example, the statement</span></span>
```csharp
int[][] a = new int[100][];
```
<span data-ttu-id="212c8-1327">crea una matriz unidimensional con 100 elementos de tipo `int[]`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1327">creates a single-dimensional array with 100 elements of type `int[]`.</span></span> <span data-ttu-id="212c8-1328">El valor inicial de cada elemento es `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1328">The initial value of each element is `null`.</span></span> <span data-ttu-id="212c8-1329">No es posible que la misma expresión de creación de matriz cree también instancias de las submatrices y la instrucción</span><span class="sxs-lookup"><span data-stu-id="212c8-1329">It is not possible for the same array creation expression to also instantiate the sub-arrays, and the statement</span></span>
```csharp
int[][] a = new int[100][5];        // Error
```
<span data-ttu-id="212c8-1330">produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1330">results in a compile-time error.</span></span> <span data-ttu-id="212c8-1331">En su lugar, la creación de instancias de las submatrices debe realizarse manualmente, como en</span><span class="sxs-lookup"><span data-stu-id="212c8-1331">Instantiation of the sub-arrays must instead be performed manually, as in</span></span>
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

<span data-ttu-id="212c8-1332">Cuando una matriz de matrices tiene una forma "rectangular", es decir, cuando las submatrices tienen la misma longitud, es más eficaz usar una matriz multidimensional.</span><span class="sxs-lookup"><span data-stu-id="212c8-1332">When an array of arrays has a "rectangular" shape, that is when the sub-arrays are all of the same length, it is more efficient to use a multi-dimensional array.</span></span> <span data-ttu-id="212c8-1333">En el ejemplo anterior, la creación de instancias de la matriz de matrices crea 101 objetos, una matriz externa y submatrices de 100.</span><span class="sxs-lookup"><span data-stu-id="212c8-1333">In the example above, instantiation of the array of arrays creates 101 objects—one outer array and 100 sub-arrays.</span></span> <span data-ttu-id="212c8-1334">En cambio,</span><span class="sxs-lookup"><span data-stu-id="212c8-1334">In contrast,</span></span>
```csharp
int[,] = new int[100, 5];
```
<span data-ttu-id="212c8-1335">crea un solo objeto, una matriz bidimensional y realiza la asignación en una única instrucción.</span><span class="sxs-lookup"><span data-stu-id="212c8-1335">creates only a single object, a two-dimensional array, and accomplishes the allocation in a single statement.</span></span>

<span data-ttu-id="212c8-1336">A continuación se muestran ejemplos de expresiones de creación de matrices con tipo implícito:</span><span class="sxs-lookup"><span data-stu-id="212c8-1336">The following are examples of implicitly typed array creation expressions:</span></span>
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

<span data-ttu-id="212c8-1337">La última expresión genera un error en tiempo de compilación porque ni `int` ni `string` se convierte implícitamente en el otro, por lo que no hay ningún tipo común mejor.</span><span class="sxs-lookup"><span data-stu-id="212c8-1337">The last expression causes a compile-time error because neither `int` nor `string` is implicitly convertible to the other, and so there is no best common type.</span></span> <span data-ttu-id="212c8-1338">En este caso, se debe usar una expresión de creación de matriz con tipo explícito, por ejemplo, especificando el tipo que se va a `object[]`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1338">An explicitly typed array creation expression must be used in this case, for example specifying the type to be `object[]`.</span></span> <span data-ttu-id="212c8-1339">Como alternativa, uno de los elementos se puede convertir a un tipo base común, que luego se convertiría en el tipo de elemento deducido.</span><span class="sxs-lookup"><span data-stu-id="212c8-1339">Alternatively, one of the elements can be cast to a common base type, which would then become the inferred element type.</span></span>

<span data-ttu-id="212c8-1340">Las expresiones de creación de matrices con tipo implícito se pueden combinar con inicializadores de objeto anónimos ([expresiones de creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions)) para crear estructuras de datos con tipos anónimos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1340">Implicitly typed array creation expressions can be combined with anonymous object initializers ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) to create anonymously typed data structures.</span></span> <span data-ttu-id="212c8-1341">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="212c8-1341">For example:</span></span>
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

#### <a name="delegate-creation-expressions"></a><span data-ttu-id="212c8-1342">Expresiones de creación de delegados</span><span class="sxs-lookup"><span data-stu-id="212c8-1342">Delegate creation expressions</span></span>

<span data-ttu-id="212c8-1343">Se usa un *delegate_creation_expression* para crear una nueva instancia de *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1343">A *delegate_creation_expression* is used to create a new instance of a *delegate_type*.</span></span>

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

<span data-ttu-id="212c8-1344">El argumento de una expresión de creación de delegado debe ser un grupo de métodos, una función anónima o un valor del tipo en tiempo de compilación `dynamic` o *delegate_type*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1344">The argument of a delegate creation expression must be a method group, an anonymous function or a value of either the compile time type `dynamic` or a *delegate_type*.</span></span> <span data-ttu-id="212c8-1345">Si el argumento es un grupo de métodos, identifica el método y, para un método de instancia, el objeto para el que se va a crear un delegado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1345">If the argument is a method group, it identifies the method and, for an instance method, the object for which to create a delegate.</span></span> <span data-ttu-id="212c8-1346">Si el argumento es una función anónima, define directamente los parámetros y el cuerpo del método del destino del delegado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1346">If the argument is an anonymous function it directly defines the parameters and method body of the delegate target.</span></span> <span data-ttu-id="212c8-1347">Si el argumento es un valor, identifica una instancia de delegado de la que se va a crear una copia.</span><span class="sxs-lookup"><span data-stu-id="212c8-1347">If the argument is a value it identifies a delegate instance of which to create a copy.</span></span>

<span data-ttu-id="212c8-1348">Si la *expresión* tiene el tipo en tiempo de compilación `dynamic`, el *delegate_creation_expression* está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) y las reglas siguientes se aplican en tiempo de ejecución mediante el tipo en tiempo de ejecución de la *expresión*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1348">If the *expression* has the compile-time type `dynamic`, the *delegate_creation_expression* is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)), and the rules below are applied at run-time using the run-time type of the *expression*.</span></span> <span data-ttu-id="212c8-1349">De lo contrario, las reglas se aplican en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1349">Otherwise the rules are applied at compile-time.</span></span>

<span data-ttu-id="212c8-1350">El procesamiento en tiempo de enlace de un *delegate_creation_expression* del formulario `new D(E)`, donde `D` es un *delegate_type* y @no__t 4 es una *expresión*, consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1350">The binding-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*  <span data-ttu-id="212c8-1351">Si `E` es un grupo de métodos, la expresión de creación de delegado se procesa de la misma manera que una conversión de grupo de métodos ([conversiones de grupo de métodos](conversions.md#method-group-conversions)) de `E` a `D`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1351">If `E` is a method group, the delegate creation expression is processed in the same way as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="212c8-1352">Si `E` es una función anónima, la expresión de creación de delegado se procesa de la misma manera que una conversión de función anónima ([conversiones de funciones anónimas](conversions.md#anonymous-function-conversions)) de `E` a `D`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1352">If `E` is an anonymous function, the delegate creation expression is processed in the same way as an anonymous function conversion ([Anonymous function conversions](conversions.md#anonymous-function-conversions)) from `E` to `D`.</span></span>
*  <span data-ttu-id="212c8-1353">Si `E` es un valor, `E` debe ser compatible ([declaraciones de delegado](delegates.md#delegate-declarations)) con `D` y el resultado es una referencia a un delegado recién creado de tipo `D` que hace referencia a la misma lista de invocación que `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1353">If `E` is a value, `E` must be compatible ([Delegate declarations](delegates.md#delegate-declarations)) with `D`, and the result is a reference to a newly created delegate of type `D` that refers to the same invocation list as `E`.</span></span> <span data-ttu-id="212c8-1354">Si `E` no es compatible con `D`, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1354">If `E` is not compatible with `D`, a compile-time error occurs.</span></span>

<span data-ttu-id="212c8-1355">El procesamiento en tiempo de ejecución de un *delegate_creation_expression* con el formato `new D(E)`, donde `D` es un *delegate_type* y @no__t 4 es una *expresión*, consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1355">The run-time processing of a *delegate_creation_expression* of the form `new D(E)`, where `D` is a *delegate_type* and `E` is an *expression*, consists of the following steps:</span></span>

*   <span data-ttu-id="212c8-1356">Si `E` es un grupo de métodos, la expresión de creación de delegados se evalúa como una conversión de grupo de métodos ([conversiones de grupo de métodos](conversions.md#method-group-conversions)) de `E` a `D`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1356">If `E` is a method group, the delegate creation expression is evaluated as a method group conversion ([Method group conversions](conversions.md#method-group-conversions)) from `E` to `D`.</span></span>
*   <span data-ttu-id="212c8-1357">Si `E` es una función anónima, la creación del delegado se evalúa como una conversión de función anónima de `E` a `D` ([conversiones de funciones anónimas](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1357">If `E` is an anonymous function, the delegate creation is evaluated as an anonymous function conversion from `E` to `D` ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>
*   <span data-ttu-id="212c8-1358">Si `E` es un valor de *delegate_type*:</span><span class="sxs-lookup"><span data-stu-id="212c8-1358">If `E` is a value of a *delegate_type*:</span></span>
    * <span data-ttu-id="212c8-1359">se evalúa `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1359">`E` is evaluated.</span></span> <span data-ttu-id="212c8-1360">Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.</span><span class="sxs-lookup"><span data-stu-id="212c8-1360">If this evaluation causes an exception, no further steps are executed.</span></span>
    * <span data-ttu-id="212c8-1361">Si el valor de `E` es `null`, se produce una @no__t 2 y no se ejecuta ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="212c8-1361">If the value of `E` is `null`, a `System.NullReferenceException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="212c8-1362">Se asigna una nueva instancia del tipo de delegado `D`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1362">A new instance of the delegate type `D` is allocated.</span></span> <span data-ttu-id="212c8-1363">Si no hay suficiente memoria disponible para asignar la nueva instancia, se produce una `System.OutOfMemoryException` y no se ejecuta ningún otro paso.</span><span class="sxs-lookup"><span data-stu-id="212c8-1363">If there is not enough memory available to allocate the new instance, a `System.OutOfMemoryException` is thrown and no further steps are executed.</span></span>
    * <span data-ttu-id="212c8-1364">La nueva instancia de delegado se inicializa con la misma lista de invocación que la instancia de delegado proporcionada por `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1364">The new delegate instance is initialized with the same invocation list as the delegate instance given by `E`.</span></span>

<span data-ttu-id="212c8-1365">La lista de invocaciones de un delegado se determina cuando se crea una instancia del delegado y permanece constante durante toda la duración del delegado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1365">The invocation list of a delegate is determined when the delegate is instantiated and then remains constant for the entire lifetime of the delegate.</span></span> <span data-ttu-id="212c8-1366">En otras palabras, no es posible cambiar las entidades a las que se puede llamar de destino de un delegado una vez que se ha creado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1366">In other words, it is not possible to change the target callable entities of a delegate once it has been created.</span></span> <span data-ttu-id="212c8-1367">Cuando se combinan dos delegados o uno se quita de otro ([declaraciones de delegado](delegates.md#delegate-declarations)), se genera un nuevo delegado. no se ha cambiado el contenido de ningún delegado existente.</span><span class="sxs-lookup"><span data-stu-id="212c8-1367">When two delegates are combined or one is removed from another ([Delegate declarations](delegates.md#delegate-declarations)), a new delegate results; no existing delegate has its contents changed.</span></span>

<span data-ttu-id="212c8-1368">No es posible crear un delegado que haga referencia a una propiedad, un indexador, un operador definido por el usuario, un constructor de instancia, un destructor o un constructor estático.</span><span class="sxs-lookup"><span data-stu-id="212c8-1368">It is not possible to create a delegate that refers to a property, indexer, user-defined operator, instance constructor, destructor, or static constructor.</span></span>

<span data-ttu-id="212c8-1369">Como se describió anteriormente, cuando se crea un delegado a partir de un grupo de métodos, la lista de parámetros formales y el tipo de valor devuelto del delegado determinan cuál de los métodos sobrecargados se deben seleccionar.</span><span class="sxs-lookup"><span data-stu-id="212c8-1369">As described above, when a delegate is created from a method group, the formal parameter list and return type of the delegate determine which of the overloaded methods to select.</span></span> <span data-ttu-id="212c8-1370">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-1370">In the example</span></span>
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
<span data-ttu-id="212c8-1371">el campo `A.f` se inicializa con un delegado que hace referencia al segundo método `Square` porque ese método coincide exactamente con la lista de parámetros formales y el tipo de valor devuelto de `DoubleFunc`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1371">the `A.f` field is initialized with a delegate that refers to the second `Square` method because that method exactly matches the formal parameter list and return type of `DoubleFunc`.</span></span> <span data-ttu-id="212c8-1372">Si el segundo método `Square` no estuviera presente, se habría producido un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1372">Had the second `Square` method not been present, a compile-time error would have occurred.</span></span>

#### <a name="anonymous-object-creation-expressions"></a><span data-ttu-id="212c8-1373">Expresiones de creación de objetos anónimos</span><span class="sxs-lookup"><span data-stu-id="212c8-1373">Anonymous object creation expressions</span></span>

<span data-ttu-id="212c8-1374">Se usa un *anonymous_object_creation_expression* para crear un objeto de un tipo anónimo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1374">An *anonymous_object_creation_expression* is used to create an object of an anonymous type.</span></span>

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

<span data-ttu-id="212c8-1375">Un inicializador de objeto anónimo declara un tipo anónimo y devuelve una instancia de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1375">An anonymous object initializer declares an anonymous type and returns an instance of that type.</span></span> <span data-ttu-id="212c8-1376">Un tipo anónimo es un tipo de clase sin nombre que hereda directamente de `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1376">An anonymous type is a nameless class type that inherits directly from `object`.</span></span> <span data-ttu-id="212c8-1377">Los miembros de un tipo anónimo son una secuencia de propiedades de solo lectura que se deducen del inicializador de objeto anónimo utilizado para crear una instancia del tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1377">The members of an anonymous type are a sequence of read-only properties inferred from the anonymous object initializer used to create an instance of the type.</span></span> <span data-ttu-id="212c8-1378">En concreto, un inicializador de objeto anónimo con el formato</span><span class="sxs-lookup"><span data-stu-id="212c8-1378">Specifically, an anonymous object initializer of the form</span></span>
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
<span data-ttu-id="212c8-1379">declara un tipo anónimo del formulario</span><span class="sxs-lookup"><span data-stu-id="212c8-1379">declares an anonymous type of the form</span></span>
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
<span data-ttu-id="212c8-1380">donde cada `Tx` es el tipo de la expresión correspondiente `ex`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1380">where each `Tx` is the type of the corresponding expression `ex`.</span></span> <span data-ttu-id="212c8-1381">La expresión usada en *member_declarator* debe tener un tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1381">The expression used in a *member_declarator* must have a type.</span></span> <span data-ttu-id="212c8-1382">Por lo tanto, se trata de un error en tiempo de compilación para que una expresión de *member_declarator* sea NULL o una función anónima.</span><span class="sxs-lookup"><span data-stu-id="212c8-1382">Thus, it is a compile-time error for an expression in a *member_declarator* to be null or an anonymous function.</span></span> <span data-ttu-id="212c8-1383">También es un error en tiempo de compilación para que la expresión tenga un tipo no seguro.</span><span class="sxs-lookup"><span data-stu-id="212c8-1383">It is also a compile-time error for the expression to have an unsafe type.</span></span>

<span data-ttu-id="212c8-1384">El compilador genera automáticamente los nombres de un tipo anónimo y del parámetro para su método `Equals` y no se puede hacer referencia a ellos en el texto del programa.</span><span class="sxs-lookup"><span data-stu-id="212c8-1384">The names of an anonymous type and of the parameter to its `Equals` method are automatically generated by the compiler and cannot be referenced in program text.</span></span>

<span data-ttu-id="212c8-1385">En el mismo programa, dos inicializadores de objeto anónimo que especifican una secuencia de propiedades de los mismos nombres y tipos en tiempo de compilación en el mismo orden generarán instancias del mismo tipo anónimo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1385">Within the same program, two anonymous object initializers that specify a sequence of properties of the same names and compile-time types in the same order will produce instances of the same anonymous type.</span></span>

<span data-ttu-id="212c8-1386">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-1386">In the example</span></span>
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
<span data-ttu-id="212c8-1387">la asignación en la última línea se permite porque `p1` y `p2` son del mismo tipo anónimo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1387">the assignment on the last line is permitted because `p1` and `p2` are of the same anonymous type.</span></span>

<span data-ttu-id="212c8-1388">Los métodos `Equals` y `GetHashcode` en los tipos anónimos invalidan los métodos heredados de `object` y se definen en términos de `Equals` y `GetHashcode` de las propiedades, de modo que dos instancias del mismo tipo anónimo son iguales si y solo si todas sus propiedades son sea.</span><span class="sxs-lookup"><span data-stu-id="212c8-1388">The `Equals` and `GetHashcode` methods on anonymous types override the methods inherited from `object`, and are defined in terms of the `Equals` and `GetHashcode` of the properties, so that two instances of the same anonymous type are equal if and only if all their properties are equal.</span></span>

<span data-ttu-id="212c8-1389">Un declarador de miembro se puede abreviar como un nombre simple ([inferencia de tipos](expressions.md#type-inference)), un acceso a miembro ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), un acceso base ([acceso base](expressions.md#base-access)) o un acceso a miembro condicional nulo ([ Expresiones condicionales NULL como inicializadores de proyección](expressions.md#null-conditional-expressions-as-projection-initializers)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1389">A member declarator can be abbreviated to a simple name ([Type inference](expressions.md#type-inference)), a member access ([Compile-time checking of dynamic overload resolution](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), a base access ([Base access](expressions.md#base-access)) or a null-conditional member access ([Null-conditional expressions as projection initializers](expressions.md#null-conditional-expressions-as-projection-initializers)).</span></span> <span data-ttu-id="212c8-1390">Esto se denomina ***inicializador de proyección*** y es la abreviatura de una declaración de y la asignación a una propiedad con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="212c8-1390">This is called a ***projection initializer*** and is shorthand for a declaration of and assignment to a property with the same name.</span></span> <span data-ttu-id="212c8-1391">En concreto, los declaradores de miembros de los formularios</span><span class="sxs-lookup"><span data-stu-id="212c8-1391">Specifically, member declarators of the forms</span></span>
```csharp
identifier
expr.identifier
```
<span data-ttu-id="212c8-1392">son exactamente equivalentes a lo siguiente, respectivamente:</span><span class="sxs-lookup"><span data-stu-id="212c8-1392">are precisely equivalent to the following, respectively:</span></span>
```csharp
identifier = identifier
identifier = expr.identifier
```

<span data-ttu-id="212c8-1393">Por lo tanto, en un inicializador de proyección, el *identificador* selecciona tanto el valor como el campo o la propiedad a los que se asigna el valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-1393">Thus, in a projection initializer the *identifier* selects both the value and the field or property to which the value is assigned.</span></span> <span data-ttu-id="212c8-1394">De manera intuitiva, un inicializador de proyección proyecta no solo un valor, sino también el nombre del valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-1394">Intuitively, a projection initializer projects not just a value, but also the name of the value.</span></span>

### <a name="the-typeof-operator"></a><span data-ttu-id="212c8-1395">El operador typeof</span><span class="sxs-lookup"><span data-stu-id="212c8-1395">The typeof operator</span></span>

<span data-ttu-id="212c8-1396">El operador `typeof` se usa para obtener el objeto `System.Type` para un tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1396">The `typeof` operator is used to obtain the `System.Type` object for a type.</span></span>

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

<span data-ttu-id="212c8-1397">La primera forma de *typeof_expression* consta de una palabra clave `typeof` seguida de un *tipo*entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="212c8-1397">The first form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *type*.</span></span> <span data-ttu-id="212c8-1398">El resultado de una expresión de este formulario es el objeto `System.Type` para el tipo indicado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1398">The result of an expression of this form is the `System.Type` object for the indicated type.</span></span> <span data-ttu-id="212c8-1399">Solo hay un objeto `System.Type` para cualquier tipo dado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1399">There is only one `System.Type` object for any given type.</span></span> <span data-ttu-id="212c8-1400">Esto significa que para un tipo @ no__t-0, `typeof(T) == typeof(T)` siempre es true.</span><span class="sxs-lookup"><span data-stu-id="212c8-1400">This means that for a type `T`, `typeof(T) == typeof(T)` is always true.</span></span> <span data-ttu-id="212c8-1401">El *tipo* no puede ser `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1401">The *type* cannot be `dynamic`.</span></span>

<span data-ttu-id="212c8-1402">La segunda forma de *typeof_expression* consta de una palabra clave `typeof` seguida de un *unbound_type_name*entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="212c8-1402">The second form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized *unbound_type_name*.</span></span> <span data-ttu-id="212c8-1403">Un *unbound_type_name* es muy similar a un *type_name* ([espacio de nombres y nombres de tipo](basic-concepts.md#namespace-and-type-names)), salvo que un *unbound_type_name* contiene *generic_dimension_specifier*s donde *type_name* contiene *tipo argument_list*s.</span><span class="sxs-lookup"><span data-stu-id="212c8-1403">An *unbound_type_name* is very similar to a *type_name* ([Namespace and type names](basic-concepts.md#namespace-and-type-names)) except that an *unbound_type_name* contains *generic_dimension_specifier*s where a *type_name* contains *type_argument_list*s.</span></span> <span data-ttu-id="212c8-1404">Cuando el operando de una *typeof_expression* es una secuencia de tokens que satisface las gramáticas de *unbound_type_name* y *type_name*, es decir, cuando no contiene ningún *generic_dimension_specifier* ni *type_argument _list*, la secuencia de tokens se considera un *type_name*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1404">When the operand of a *typeof_expression* is a sequence of tokens that satisfies the grammars of both *unbound_type_name* and *type_name*, namely when it contains neither a *generic_dimension_specifier* nor a *type_argument_list*, the sequence of tokens is considered to be a *type_name*.</span></span> <span data-ttu-id="212c8-1405">El significado de un *unbound_type_name* se determina de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-1405">The meaning of an *unbound_type_name* is determined as follows:</span></span>

*  <span data-ttu-id="212c8-1406">Convierta la secuencia de tokens en un *type_name* reemplazando cada *generic_dimension_specifier* por un *type_argument_list* que tenga el mismo número de comas y la palabra clave `object` como cada *type_argument*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1406">Convert the sequence of tokens to a *type_name* by replacing each *generic_dimension_specifier* with a *type_argument_list* having the same number of commas and the keyword `object` as each *type_argument*.</span></span>
*  <span data-ttu-id="212c8-1407">Evalúe el *type_name*resultante, pasando por alto todas las restricciones de parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1407">Evaluate the resulting *type_name*, while ignoring all type parameter constraints.</span></span>
*  <span data-ttu-id="212c8-1408">*Unbound_type_name* se resuelve en el tipo genérico sin enlazar asociado con el tipo construido resultante ([tipos enlazados y sin enlazar](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1408">The *unbound_type_name* resolves to the unbound generic type associated with the resulting constructed type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span>

<span data-ttu-id="212c8-1409">El resultado de *typeof_expression* es el objeto `System.Type` para el tipo genérico sin enlazar resultante.</span><span class="sxs-lookup"><span data-stu-id="212c8-1409">The result of the *typeof_expression* is the `System.Type` object for the resulting unbound generic type.</span></span>

<span data-ttu-id="212c8-1410">La tercera forma de *typeof_expression* consta de una palabra clave `typeof` seguida de una palabra clave `void` entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="212c8-1410">The third form of *typeof_expression* consists of a `typeof` keyword followed by a parenthesized `void` keyword.</span></span> <span data-ttu-id="212c8-1411">El resultado de una expresión de este formulario es el objeto `System.Type` que representa la ausencia de un tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1411">The result of an expression of this form is the `System.Type` object that represents the absence of a type.</span></span> <span data-ttu-id="212c8-1412">El objeto de tipo devuelto por `typeof(void)` es distinto del objeto de tipo devuelto para cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1412">The type object returned by `typeof(void)` is distinct from the type object returned for any type.</span></span> <span data-ttu-id="212c8-1413">Este objeto de tipo especial es útil en las bibliotecas de clases que permiten la reflexión en métodos del lenguaje, donde esos métodos desean tener una manera de representar el tipo de valor devuelto de cualquier método, incluidos los métodos void, con una instancia de `System.Type`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1413">This special type object is useful in class libraries that allow reflection onto methods in the language, where those methods wish to have a way to represent the return type of any method, including void methods, with an instance of `System.Type`.</span></span>

<span data-ttu-id="212c8-1414">El operador `typeof` se puede usar en un parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1414">The `typeof` operator can be used on a type parameter.</span></span> <span data-ttu-id="212c8-1415">El resultado es el objeto `System.Type` para el tipo en tiempo de ejecución que se enlazó al parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1415">The result is the `System.Type` object for the run-time type that was bound to the type parameter.</span></span> <span data-ttu-id="212c8-1416">El operador `typeof` también se puede usar en un tipo construido o en un tipo genérico sin enlazar ([tipos enlazados y sin enlazar](types.md#bound-and-unbound-types)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1416">The `typeof` operator can also be used on a constructed type or an unbound generic type ([Bound and unbound types](types.md#bound-and-unbound-types)).</span></span> <span data-ttu-id="212c8-1417">El objeto `System.Type` para un tipo genérico sin enlazar no es el mismo que el objeto `System.Type` del tipo de instancia.</span><span class="sxs-lookup"><span data-stu-id="212c8-1417">The `System.Type` object for an unbound generic type is not the same as the `System.Type` object of the instance type.</span></span> <span data-ttu-id="212c8-1418">El tipo de instancia siempre es un tipo construido cerrado en tiempo de ejecución, por lo que su objeto `System.Type` depende de los argumentos de tipo en tiempo de ejecución en uso, mientras que el tipo genérico sin enlazar no tiene ningún argumento de tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1418">The instance type is always a closed constructed type at run-time so its `System.Type` object depends on the run-time type arguments in use, while the unbound generic type has no type arguments.</span></span>

<span data-ttu-id="212c8-1419">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-1419">The example</span></span>
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
<span data-ttu-id="212c8-1420">genera el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="212c8-1420">produces the following output:</span></span>
```console
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

<span data-ttu-id="212c8-1421">Tenga en cuenta que `int` y `System.Int32` son del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1421">Note that `int` and `System.Int32` are the same type.</span></span>

<span data-ttu-id="212c8-1422">Tenga en cuenta también que el resultado de `typeof(X<>)` no depende del argumento de tipo, pero el resultado de `typeof(X<T>)` sí lo hace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1422">Also note that the result of `typeof(X<>)` does not depend on the type argument but the result of `typeof(X<T>)` does.</span></span>

### <a name="the-checked-and-unchecked-operators"></a><span data-ttu-id="212c8-1423">Operadores checked y unchecked</span><span class="sxs-lookup"><span data-stu-id="212c8-1423">The checked and unchecked operators</span></span>

<span data-ttu-id="212c8-1424">Los operadores `checked` y `unchecked` se utilizan para controlar el ***contexto de comprobación de desbordamiento*** para conversiones y operaciones aritméticas de tipo entero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1424">The `checked` and `unchecked` operators are used to control the ***overflow checking context*** for integral-type arithmetic operations and conversions.</span></span>

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

<span data-ttu-id="212c8-1425">El operador `checked` evalúa la expresión contenida en un contexto comprobado y el operador `unchecked` evalúa la expresión contenida en un contexto no comprobado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1425">The `checked` operator evaluates the contained expression in a checked context, and the `unchecked` operator evaluates the contained expression in an unchecked context.</span></span> <span data-ttu-id="212c8-1426">Un *checked_expression* o *unchecked_expression* corresponde exactamente a un *parenthesized_expression* ([expresiones entre paréntesis](expressions.md#parenthesized-expressions)), salvo que la expresión contenida se evalúa en el contexto de comprobación de desbordamiento determinado. .</span><span class="sxs-lookup"><span data-stu-id="212c8-1426">A *checked_expression* or *unchecked_expression* corresponds exactly to a *parenthesized_expression* ([Parenthesized expressions](expressions.md#parenthesized-expressions)), except that the contained expression is evaluated in the given overflow checking context.</span></span>

<span data-ttu-id="212c8-1427">El contexto de comprobación de desbordamiento también se puede controlar a través de las instrucciones `checked` y `unchecked` ([las instrucciones checked y unchecked](statements.md#the-checked-and-unchecked-statements)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1427">The overflow checking context can also be controlled through the `checked` and `unchecked` statements ([The checked and unchecked statements](statements.md#the-checked-and-unchecked-statements)).</span></span>

<span data-ttu-id="212c8-1428">Las siguientes operaciones se ven afectadas por el contexto de comprobación de desbordamiento establecido por los operadores y las instrucciones `checked` y `unchecked`:</span><span class="sxs-lookup"><span data-stu-id="212c8-1428">The following operations are affected by the overflow checking context established by the `checked` and `unchecked` operators and statements:</span></span>

*  <span data-ttu-id="212c8-1429">Los operadores unarios predefinidos `++` y `--` (operadores de[incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) [postfijo y operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)) cuando el operando es de un tipo entero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1429">The predefined `++` and `--` unary operators ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators) and [Prefix increment and decrement operators](expressions.md#prefix-increment-and-decrement-operators)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="212c8-1430">El operador unario `-` predefinido ([operador unario menos](expressions.md#unary-minus-operator)), cuando el operando es de un tipo entero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1430">The predefined `-` unary operator ([Unary minus operator](expressions.md#unary-minus-operator)), when the operand is of an integral type.</span></span>
*  <span data-ttu-id="212c8-1431">Los operadores binarios `+`, `-`, `*` y `/` predefinidos ([operadores aritméticos](expressions.md#arithmetic-operators)), cuando ambos operandos son de tipos enteros.</span><span class="sxs-lookup"><span data-stu-id="212c8-1431">The predefined `+`, `-`, `*`, and `/` binary operators ([Arithmetic operators](expressions.md#arithmetic-operators)), when both operands are of integral types.</span></span>
*  <span data-ttu-id="212c8-1432">Conversiones numéricas explícitas ([Conversiones numéricas explícitas](conversions.md#explicit-numeric-conversions)) de un tipo entero a otro tipo entero, o de `float` o `double` a un tipo entero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1432">Explicit numeric conversions ([Explicit numeric conversions](conversions.md#explicit-numeric-conversions)) from one integral type to another integral type, or from `float` or `double` to an integral type.</span></span>

<span data-ttu-id="212c8-1433">Cuando una de las operaciones anteriores produce un resultado que es demasiado grande para representarlo en el tipo de destino, el contexto en el que se realiza la operación controla el comportamiento resultante:</span><span class="sxs-lookup"><span data-stu-id="212c8-1433">When one of the above operations produce a result that is too large to represent in the destination type, the context in which the operation is performed controls the resulting behavior:</span></span>

*  <span data-ttu-id="212c8-1434">En un contexto `checked`, si la operación es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)), se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1434">In a `checked` context, if the operation is a constant expression ([Constant expressions](expressions.md#constant-expressions)), a compile-time error occurs.</span></span> <span data-ttu-id="212c8-1435">De lo contrario, cuando la operación se realiza en tiempo de ejecución, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1435">Otherwise, when the operation is performed at run-time, a `System.OverflowException` is thrown.</span></span>
*  <span data-ttu-id="212c8-1436">En un contexto `unchecked`, el resultado se trunca descartando los bits de orden superior que no caben en el tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="212c8-1436">In an `unchecked` context, the result is truncated by discarding any high-order bits that do not fit in the destination type.</span></span>

<span data-ttu-id="212c8-1437">En el caso de expresiones no constantes (expresiones que se evalúan en tiempo de ejecución) que no están incluidas en ningún operador o instrucción `checked` o `unchecked`, el contexto de comprobación de desbordamiento predeterminado es `unchecked` a menos que se produzcan factores externos (como modificadores de compilador y configuración del entorno de ejecución) llamada para la evaluación de `checked`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1437">For non-constant expressions (expressions that are evaluated at run-time) that are not enclosed by any `checked` or `unchecked` operators or statements, the default overflow checking context is `unchecked` unless external factors (such as compiler switches and execution environment configuration) call for `checked` evaluation.</span></span>

<span data-ttu-id="212c8-1438">En el caso de expresiones constantes (expresiones que se pueden evaluar completamente en tiempo de compilación), el contexto de comprobación de desbordamiento predeterminado siempre es `checked`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1438">For constant expressions (expressions that can be fully evaluated at compile-time), the default overflow checking context is always `checked`.</span></span> <span data-ttu-id="212c8-1439">A menos que una expresión constante se coloque explícitamente en un contexto `unchecked`, los desbordamientos que se producen durante la evaluación de la expresión en tiempo de compilación siempre producen errores en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1439">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur during the compile-time evaluation of the expression always cause compile-time errors.</span></span>

<span data-ttu-id="212c8-1440">El cuerpo de una función anónima no se ve afectado por los contextos `checked` o `unchecked` en los que se produce la función anónima.</span><span class="sxs-lookup"><span data-stu-id="212c8-1440">The body of an anonymous function is not affected by `checked` or `unchecked` contexts in which the anonymous function occurs.</span></span>

<span data-ttu-id="212c8-1441">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-1441">In the example</span></span>
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
<span data-ttu-id="212c8-1442">no se detectan errores en tiempo de compilación, ya que ninguna de las expresiones se puede evaluar en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1442">no compile-time errors are reported since neither of the expressions can be evaluated at compile-time.</span></span> <span data-ttu-id="212c8-1443">En tiempo de ejecución, el método `F` produce una `System.OverflowException` y el método `G` devuelve-727379968 (los 32 bits inferiores del resultado fuera del intervalo).</span><span class="sxs-lookup"><span data-stu-id="212c8-1443">At run-time, the `F` method throws a `System.OverflowException`, and the `G` method returns -727379968 (the lower 32 bits of the out-of-range result).</span></span> <span data-ttu-id="212c8-1444">El comportamiento del método `H` depende del contexto de comprobación de desbordamiento predeterminado para la compilación, pero es igual que `F` o igual que `G`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1444">The behavior of the `H` method depends on the default overflow checking context for the compilation, but it is either the same as `F` or the same as `G`.</span></span>

<span data-ttu-id="212c8-1445">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-1445">In the example</span></span>
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
<span data-ttu-id="212c8-1446">los desbordamientos que se producen al evaluar las expresiones constantes en `F` y `H` provocan que se notifiquen los errores en tiempo de compilación porque las expresiones se evalúan en un contexto de @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="212c8-1446">the overflows that occur when evaluating the constant expressions in `F` and `H` cause compile-time errors to be reported because the expressions are evaluated in a `checked` context.</span></span> <span data-ttu-id="212c8-1447">También se produce un desbordamiento al evaluar la expresión constante en `G`, pero como la evaluación tiene lugar en un contexto `unchecked`, el desbordamiento no se registra.</span><span class="sxs-lookup"><span data-stu-id="212c8-1447">An overflow also occurs when evaluating the constant expression in `G`, but since the evaluation takes place in an `unchecked` context, the overflow is not reported.</span></span>

<span data-ttu-id="212c8-1448">Los operadores `checked` y `unchecked` solo afectan al contexto de comprobación de desbordamiento para las operaciones que están contenidas textualmente en los tokens "@no__t 2" y "`)`".</span><span class="sxs-lookup"><span data-stu-id="212c8-1448">The `checked` and `unchecked` operators only affect the overflow checking context for those operations that are textually contained within the "`(`" and "`)`" tokens.</span></span> <span data-ttu-id="212c8-1449">Los operadores no tienen ningún efecto en los miembros de función que se invocan como resultado de evaluar la expresión contenida.</span><span class="sxs-lookup"><span data-stu-id="212c8-1449">The operators have no effect on function members that are invoked as a result of evaluating the contained expression.</span></span> <span data-ttu-id="212c8-1450">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-1450">In the example</span></span>
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
<span data-ttu-id="212c8-1451">el uso de `checked` en `F` no afecta a la evaluación de @no__t 2 en `Multiply`, por lo que `x * y` se evalúa en el contexto de comprobación de desbordamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1451">the use of `checked` in `F` does not affect the evaluation of `x * y` in `Multiply`, so `x * y` is evaluated in the default overflow checking context.</span></span>

<span data-ttu-id="212c8-1452">El operador `unchecked` es práctico al escribir constantes de los tipos enteros con signo en notación hexadecimal.</span><span class="sxs-lookup"><span data-stu-id="212c8-1452">The `unchecked` operator is convenient when writing constants of the signed integral types in hexadecimal notation.</span></span> <span data-ttu-id="212c8-1453">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="212c8-1453">For example:</span></span>
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

<span data-ttu-id="212c8-1454">Las dos constantes hexadecimales anteriores son del tipo `uint`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1454">Both of the hexadecimal constants above are of type `uint`.</span></span> <span data-ttu-id="212c8-1455">Dado que las constantes están fuera del intervalo `int`, sin el operador `unchecked`, las conversiones a `int` generarán errores en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1455">Because the constants are outside the `int` range, without the `unchecked` operator, the casts to `int` would produce compile-time errors.</span></span>

<span data-ttu-id="212c8-1456">Los operadores e instrucciones `checked` y `unchecked` permiten a los programadores controlar determinados aspectos de algunos cálculos numéricos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1456">The `checked` and `unchecked` operators and statements allow programmers to control certain aspects of some numeric calculations.</span></span> <span data-ttu-id="212c8-1457">Sin embargo, el comportamiento de algunos operadores numéricos depende de los tipos de datos de los operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1457">However, the behavior of some numeric operators depends on their operands' data types.</span></span> <span data-ttu-id="212c8-1458">Por ejemplo, si se multiplican dos decimales, siempre se produce una excepción en el desbordamiento incluso dentro de una construcción `unchecked` explícitamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-1458">For example, multiplying two decimals always results in an exception on overflow even within an explicitly `unchecked` construct.</span></span> <span data-ttu-id="212c8-1459">Del mismo modo, si se multiplican dos floats, nunca se produce una excepción en el desbordamiento incluso dentro de una construcción `checked` explícitamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-1459">Similarly, multiplying two floats never results in an exception on overflow even within an explicitly `checked` construct.</span></span> <span data-ttu-id="212c8-1460">Además, otros operadores nunca se ven afectados por el modo de comprobación, ya sea de forma predeterminada o explícita.</span><span class="sxs-lookup"><span data-stu-id="212c8-1460">In addition, other operators are never affected by the mode of checking, whether default or explicit.</span></span>

### <a name="default-value-expressions"></a><span data-ttu-id="212c8-1461">Expresiones de valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="212c8-1461">Default value expressions</span></span>

<span data-ttu-id="212c8-1462">Una expresión de valor predeterminado se usa para obtener el valor predeterminado ([valores predeterminados](variables.md#default-values)) de un tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1462">A default value expression is used to obtain the default value ([Default values](variables.md#default-values)) of a type.</span></span> <span data-ttu-id="212c8-1463">Normalmente, se usa una expresión de valor predeterminado para los parámetros de tipo, ya que es posible que no se conozca si el parámetro de tipo es un tipo de valor o un tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="212c8-1463">Typically a default value expression is used for type parameters, since it may not be known if the type parameter is a value type or a reference type.</span></span> <span data-ttu-id="212c8-1464">(No existe ninguna conversión del literal `null` a un parámetro de tipo a menos que se sepa que el parámetro de tipo es un tipo de referencia).</span><span class="sxs-lookup"><span data-stu-id="212c8-1464">(No conversion exists from the `null` literal to a type parameter unless the type parameter is known to be a reference type.)</span></span>

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

<span data-ttu-id="212c8-1465">Si el *tipo* de un *default_value_expression* se evalúa en tiempo de ejecución en un tipo de referencia, el resultado es `null` convertido en ese tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1465">If the *type* in a *default_value_expression* evaluates at run-time to a reference type, the result is `null` converted to that type.</span></span> <span data-ttu-id="212c8-1466">Si el *tipo* de un *default_value_expression* se evalúa en tiempo de ejecución en un tipo de valor, el resultado es el valor predeterminado de *Value_type*([constructores predeterminados](types.md#default-constructors)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1466">If the *type* in a *default_value_expression* evaluates at run-time to a value type, the result is the *value_type*'s default value ([Default constructors](types.md#default-constructors)).</span></span>

<span data-ttu-id="212c8-1467">Un *default_value_expression* es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)) si el tipo es un tipo de referencia o un parámetro de tipo que se sabe que es un tipo de referencia ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1467">A *default_value_expression* is a constant expression ([Constant expressions](expressions.md#constant-expressions)) if the type is a reference type or a type parameter that is known to be a reference type ([Type parameter constraints](classes.md#type-parameter-constraints)).</span></span> <span data-ttu-id="212c8-1468">Además, un *default_value_expression* es una expresión constante si el tipo es uno de los siguientes tipos de valor: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3 o cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="212c8-1468">In addition, a *default_value_expression* is a constant expression if the type is one of the following value types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, or any enumeration type.</span></span>


### <a name="nameof-expressions"></a><span data-ttu-id="212c8-1469">Expresiones Name</span><span class="sxs-lookup"><span data-stu-id="212c8-1469">Nameof expressions</span></span>

<span data-ttu-id="212c8-1470">Un *nameof_expression* se usa para obtener el nombre de una entidad de programa como una cadena de constante.</span><span class="sxs-lookup"><span data-stu-id="212c8-1470">A *nameof_expression* is used to obtain the name of a program entity as a constant string.</span></span>

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

<span data-ttu-id="212c8-1471">En términos gramaticales, el operando *named_entity* siempre es una expresión.</span><span class="sxs-lookup"><span data-stu-id="212c8-1471">Grammatically speaking, the *named_entity* operand is always an expression.</span></span> <span data-ttu-id="212c8-1472">Dado que `nameof` no es una palabra clave reservada, una expresión name es siempre sintácticamente ambigua con una invocación del nombre simple `nameof`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1472">Because `nameof` is not a reserved keyword, a nameof expression is always syntactically ambiguous with an invocation of the simple name `nameof`.</span></span> <span data-ttu-id="212c8-1473">Por motivos de compatibilidad, si la búsqueda de nombres ([nombres simples](expressions.md#simple-names)) del nombre `nameof` se realiza correctamente, la expresión se trata como un *invocation_expression* , independientemente de si la invocación es legal.</span><span class="sxs-lookup"><span data-stu-id="212c8-1473">For compatibility reasons, if a name lookup ([Simple names](expressions.md#simple-names)) of the name `nameof` succeeds, the expression is treated as an *invocation_expression* -- regardless of whether the invocation is legal.</span></span> <span data-ttu-id="212c8-1474">En caso contrario, es *nameof_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1474">Otherwise it is a *nameof_expression*.</span></span>

<span data-ttu-id="212c8-1475">El significado de *named_entity* de un *nameof_expression* es el significado de este como una expresión; es decir, como *simple_name*, *base_access* o *member_access*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1475">The meaning of the *named_entity* of a *nameof_expression* is the meaning of it as an expression; that is, either as a *simple_name*, a *base_access* or a *member_access*.</span></span> <span data-ttu-id="212c8-1476">Sin embargo, en los casos en los que la búsqueda descrita en [nombres simples](expressions.md#simple-names) y [acceso a miembros](expressions.md#member-access) produce un error porque se encontró un miembro de instancia en un contexto estático, *nameof_expression* no genera este error.</span><span class="sxs-lookup"><span data-stu-id="212c8-1476">However, where the lookup described in [Simple names](expressions.md#simple-names) and [Member access](expressions.md#member-access) results in an error because an instance member was found in a static context, a *nameof_expression* produces no such error.</span></span>

<span data-ttu-id="212c8-1477">Se trata de un error en tiempo de compilación para un *named_entity* que designa un grupo de métodos para que tenga un *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1477">It is a compile-time error for a *named_entity* designating a method group to have a *type_argument_list*.</span></span> <span data-ttu-id="212c8-1478">Es un error en tiempo de compilación para que un *named_entity_target* tenga el tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1478">It is a compile time error for a *named_entity_target* to have the type `dynamic`.</span></span>

<span data-ttu-id="212c8-1479">Un *nameof_expression* es una expresión constante de tipo `string` y no tiene ningún efecto en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="212c8-1479">A *nameof_expression* is a constant expression of type `string`, and has no effect at runtime.</span></span> <span data-ttu-id="212c8-1480">En concreto, su *named_entity* no se evalúa y se omite para los fines del análisis de asignación definitiva ([reglas generales para expresiones simples](variables.md#general-rules-for-simple-expressions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1480">Specifically, its *named_entity* is not evaluated, and is ignored for the purposes of definite assignment analysis ([General rules for simple expressions](variables.md#general-rules-for-simple-expressions)).</span></span> <span data-ttu-id="212c8-1481">Su valor es el último identificador de *named_entity* antes de la *type_argument_list*final opcional, transformada de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-1481">Its value is the last identifier of the *named_entity* before the optional final *type_argument_list*, transformed in the following way:</span></span>

* <span data-ttu-id="212c8-1482">El prefijo`@`"", si se utiliza, se quita.</span><span class="sxs-lookup"><span data-stu-id="212c8-1482">The prefix "`@`", if used, is removed.</span></span>
* <span data-ttu-id="212c8-1483">Cada *unicode_escape_sequence* se transforma en el carácter Unicode correspondiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-1483">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
* <span data-ttu-id="212c8-1484">Se quitan todos los *formatting_characters* .</span><span class="sxs-lookup"><span data-stu-id="212c8-1484">Any *formatting_characters* are removed.</span></span>

<span data-ttu-id="212c8-1485">Se trata de las mismas transformaciones aplicadas en los [identificadores](lexical-structure.md#identifiers) al probar la igualdad entre los identificadores.</span><span class="sxs-lookup"><span data-stu-id="212c8-1485">These are the same transformations applied in [Identifiers](lexical-structure.md#identifiers) when testing equality between identifiers.</span></span>

<span data-ttu-id="212c8-1486">TODO: ejemplos</span><span class="sxs-lookup"><span data-stu-id="212c8-1486">TODO: examples</span></span>

### <a name="anonymous-method-expressions"></a><span data-ttu-id="212c8-1487">Expresiones de método anónimo</span><span class="sxs-lookup"><span data-stu-id="212c8-1487">Anonymous method expressions</span></span>

<span data-ttu-id="212c8-1488">Un *anonymous_method_expression* es una de las dos formas de definir una función anónima.</span><span class="sxs-lookup"><span data-stu-id="212c8-1488">An *anonymous_method_expression* is one of two ways of defining an anonymous function.</span></span> <span data-ttu-id="212c8-1489">Se describen con más detalle en [expresiones de función anónima](expressions.md#anonymous-function-expressions).</span><span class="sxs-lookup"><span data-stu-id="212c8-1489">These are further described in [Anonymous function expressions](expressions.md#anonymous-function-expressions).</span></span>

## <a name="unary-operators"></a><span data-ttu-id="212c8-1490">Operadores unarios</span><span class="sxs-lookup"><span data-stu-id="212c8-1490">Unary operators</span></span>

<span data-ttu-id="212c8-1491">Los operadores `?`, `+`, `-`, `!`, `~`, `++`, `--`, CAST y `await` se denominan operadores unarios.</span><span class="sxs-lookup"><span data-stu-id="212c8-1491">The `?`, `+`, `-`, `!`, `~`, `++`, `--`, cast, and `await` operators are called the unary operators.</span></span>

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

<span data-ttu-id="212c8-1492">Si el operando de un *unary_expression* tiene el tipo en tiempo de compilación `dynamic`, está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1492">If the operand of a *unary_expression* has the compile-time type `dynamic`, it is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="212c8-1493">En este caso, el tipo en tiempo de compilación de *unary_expression* es `dynamic` y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución del operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-1493">In this case the compile-time type of the *unary_expression* is `dynamic`, and the resolution described below will take place at run-time using the run-time type of the operand.</span></span>

### <a name="null-conditional-operator"></a><span data-ttu-id="212c8-1494">Operador condicional null</span><span class="sxs-lookup"><span data-stu-id="212c8-1494">Null-conditional operator</span></span>

<span data-ttu-id="212c8-1495">El operador condicional null aplica una lista de operaciones a su operando solo si ese operando no es NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-1495">The null-conditional operator applies a list of operations to its operand only if that operand is non-null.</span></span> <span data-ttu-id="212c8-1496">De lo contrario, el resultado de aplicar el operador es `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1496">Otherwise the result of applying the operator is `null`.</span></span>

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

<span data-ttu-id="212c8-1497">La lista de operaciones puede incluir operaciones de acceso a miembros y acceso a elementos (que pueden ser a su vez valores condicionales null), así como invocación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1497">The list of operations can include member access and element access operations (which may themselves be null-conditional), as well as invocation.</span></span>

<span data-ttu-id="212c8-1498">Por ejemplo, la expresión `a.b?[0]?.c()` es un *null_conditional_expression* con un *primary_expression* `a.b` y *null_conditional_operations* `?[0]` (acceso de elemento condicional null), `?.c` (miembro condicional null acceso) y `()` (Invocación).</span><span class="sxs-lookup"><span data-stu-id="212c8-1498">For example, the expression `a.b?[0]?.c()` is a *null_conditional_expression* with a *primary_expression* `a.b` and *null_conditional_operations* `?[0]` (null-conditional element access), `?.c` (null-conditional member access) and `()` (invocation).</span></span>

<span data-ttu-id="212c8-1499">En el caso de un *null_conditional_expression* `E` con *primary_expression* `P`, permita que `E0` sea la expresión obtenida quitando textualmente el @no__t 5 inicial de cada una de las *null_conditional_operations* de `E` que tener uno.</span><span class="sxs-lookup"><span data-stu-id="212c8-1499">For a *null_conditional_expression* `E` with a *primary_expression* `P`, let `E0` be the expression obtained by textually removing the leading `?` from each of the *null_conditional_operations* of `E` that have one.</span></span> <span data-ttu-id="212c8-1500">Conceptualmente, `E0` es la expresión que se evaluará si ninguna de las comprobaciones nulas representadas por el `?`s encuentra un `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1500">Conceptually, `E0` is the expression that will be evaluated if none of the null checks represented by the `?`s do find a `null`.</span></span>

<span data-ttu-id="212c8-1501">Además, deje que `E1` sea la expresión obtenida quitando textualmente el @no__t inicial 1 de la primera de las *null_conditional_operations* en `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1501">Also, let `E1` be the expression obtained by textually removing the leading `?` from just the first of the *null_conditional_operations* in `E`.</span></span> <span data-ttu-id="212c8-1502">Esto puede conducir a una *expresión principal* (si solo había un `?`) o a otro *null_conditional_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1502">This may lead to a *primary-expression* (if there was just one `?`) or to another *null_conditional_expression*.</span></span>

<span data-ttu-id="212c8-1503">Por ejemplo, si `E` es la expresión `a.b?[0]?.c()`, `E0` es la expresión `a.b[0].c()` y `E1` es la expresión `a.b[0]?.c()`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1503">For example, if `E` is the expression `a.b?[0]?.c()`, then `E0` is the expression `a.b[0].c()` and `E1` is the expression `a.b[0]?.c()`.</span></span>

<span data-ttu-id="212c8-1504">Si `E0` se clasifica como Nothing, `E` se clasifica como Nothing.</span><span class="sxs-lookup"><span data-stu-id="212c8-1504">If `E0` is classified as nothing, then `E` is classified as nothing.</span></span> <span data-ttu-id="212c8-1505">De lo contrario, E se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-1505">Otherwise E is classified as a value.</span></span>

<span data-ttu-id="212c8-1506">`E0` y `E1` se usan para determinar el significado de `E`:</span><span class="sxs-lookup"><span data-stu-id="212c8-1506">`E0` and `E1` are used to determine the meaning of `E`:</span></span>

*  <span data-ttu-id="212c8-1507">Si `E` se produce como *statement_expression* , el significado de `E` es el mismo que el de la instrucción.</span><span class="sxs-lookup"><span data-stu-id="212c8-1507">If `E` occurs as a *statement_expression* the meaning of `E` is the same as the statement</span></span>

   ```csharp
   if ((object)P != null) E1;
   ```

   <span data-ttu-id="212c8-1508">excepto que P se evalúa solo una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-1508">except that P is evaluated only once.</span></span>

*  <span data-ttu-id="212c8-1509">De lo contrario, si `E0` se clasifica como Nothing, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1509">Otherwise, if `E0` is classified as nothing a compile-time error occurs.</span></span>

*  <span data-ttu-id="212c8-1510">De lo contrario, deje que `T0` sea el tipo de `E0`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1510">Otherwise, let `T0` be the type of `E0`.</span></span>

   *  <span data-ttu-id="212c8-1511">Si `T0` es un parámetro de tipo que no se sabe que es un tipo de referencia o un tipo de valor que no acepta valores NULL, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1511">If `T0` is a type parameter that is not known to be a reference type or a non-nullable value type, a compile-time error occurs.</span></span>

   *  <span data-ttu-id="212c8-1512">Si `T0` es un tipo de valor que no acepta valores NULL, el tipo de `E` es `T0?` y el significado de `E` es el mismo que</span><span class="sxs-lookup"><span data-stu-id="212c8-1512">If `T0` is a non-nullable value type, then the type of `E` is `T0?`, and the meaning of `E` is the same as</span></span>

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      <span data-ttu-id="212c8-1513">salvo que `P` solo se evalúa una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-1513">except that `P` is evaluated only once.</span></span>

   *  <span data-ttu-id="212c8-1514">De lo contrario, el tipo de E es T0 y el significado de E es el mismo que</span><span class="sxs-lookup"><span data-stu-id="212c8-1514">Otherwise the type of E is T0, and the meaning of E is the same as</span></span>

      ```csharp
      ((object)P == null) ? null : E1
      ```

      <span data-ttu-id="212c8-1515">salvo que `P` solo se evalúa una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-1515">except that `P` is evaluated only once.</span></span>

<span data-ttu-id="212c8-1516">Si `E1` es en sí mismo un *null_conditional_expression*, estas reglas se aplican de nuevo, anidando las pruebas para `null` hasta que no haya más `?`, y la expresión se ha reducido hasta la expresión principal `E0`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1516">If `E1` is itself a *null_conditional_expression*, then these rules are applied again, nesting the tests for `null` until there are no further `?`'s, and the expression has been reduced all the way down to the primary-expression `E0`.</span></span>

<span data-ttu-id="212c8-1517">Por ejemplo, si la expresión `a.b?[0]?.c()` se produce como una expresión de instrucción, como en la instrucción:</span><span class="sxs-lookup"><span data-stu-id="212c8-1517">For example, if the expression `a.b?[0]?.c()` occurs as a statement-expression, as in the statement:</span></span>
```csharp
a.b?[0]?.c();
```
<span data-ttu-id="212c8-1518">su significado es equivalente a:</span><span class="sxs-lookup"><span data-stu-id="212c8-1518">its meaning is equivalent to:</span></span>
```csharp
if (a.b != null) a.b[0]?.c();
```
<span data-ttu-id="212c8-1519">lo que es equivalente a:</span><span class="sxs-lookup"><span data-stu-id="212c8-1519">which again is equivalent to:</span></span>
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
<span data-ttu-id="212c8-1520">Salvo que `a.b` y `a.b[0]` solo se evalúan una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-1520">Except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

<span data-ttu-id="212c8-1521">Si se produce en un contexto donde se usa su valor, como en:</span><span class="sxs-lookup"><span data-stu-id="212c8-1521">If it occurs in a context where its value is used, as in:</span></span>
```csharp
var x = a.b?[0]?.c();
```
<span data-ttu-id="212c8-1522">y suponiendo que el tipo de la invocación final no es un tipo de valor que no acepta valores NULL, su significado es equivalente a:</span><span class="sxs-lookup"><span data-stu-id="212c8-1522">and assuming that the type of the final invocation is not a non-nullable value type, its meaning is equivalent to:</span></span>
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
<span data-ttu-id="212c8-1523">salvo que `a.b` y `a.b[0]` solo se evalúan una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-1523">except that `a.b` and `a.b[0]` are evaluated only once.</span></span>

#### <a name="null-conditional-expressions-as-projection-initializers"></a><span data-ttu-id="212c8-1524">Expresiones condicionales NULL como inicializadores de proyección</span><span class="sxs-lookup"><span data-stu-id="212c8-1524">Null-conditional expressions as projection initializers</span></span>

<span data-ttu-id="212c8-1525">Solo se permite una expresión condicional NULL como *member_declarator* en un *anonymous_object_creation_expression* (expresiones de[creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions)) si finaliza con un acceso a miembro (opcionalmente condicional).</span><span class="sxs-lookup"><span data-stu-id="212c8-1525">A null-conditional expression is only allowed as a *member_declarator* in an *anonymous_object_creation_expression* ([Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions)) if it ends with an (optionally null-conditional) member access.</span></span> <span data-ttu-id="212c8-1526">Gramaticalmente, este requisito se puede expresar de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-1526">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

<span data-ttu-id="212c8-1527">Este es un caso especial de la gramática de *null_conditional_expression* anterior.</span><span class="sxs-lookup"><span data-stu-id="212c8-1527">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="212c8-1528">La producción de *member_declarator* en [expresiones de creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions) incluye solo *null_conditional_member_access*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1528">The production for *member_declarator* in [Anonymous object creation expressions](expressions.md#anonymous-object-creation-expressions) then includes only *null_conditional_member_access*.</span></span>

#### <a name="null-conditional-expressions-as-statement-expressions"></a><span data-ttu-id="212c8-1529">Expresiones condicionales NULL como expresiones de instrucción</span><span class="sxs-lookup"><span data-stu-id="212c8-1529">Null-conditional expressions as statement expressions</span></span>

<span data-ttu-id="212c8-1530">Solo se permite una expresión condicional NULL como *statement_expression* (instrucciones de[expresión](statements.md#expression-statements)) si finaliza con una invocación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1530">A null-conditional expression is only allowed as a *statement_expression* ([Expression statements](statements.md#expression-statements)) if it ends with an invocation.</span></span> <span data-ttu-id="212c8-1531">Gramaticalmente, este requisito se puede expresar de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="212c8-1531">Grammatically, this requirement can be expressed as:</span></span>

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

<span data-ttu-id="212c8-1532">Este es un caso especial de la gramática de *null_conditional_expression* anterior.</span><span class="sxs-lookup"><span data-stu-id="212c8-1532">This is a special case of the grammar for *null_conditional_expression* above.</span></span> <span data-ttu-id="212c8-1533">La producción de *statement_expression* en las [instrucciones de expresión](statements.md#expression-statements) solo incluye *null_conditional_invocation_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1533">The production for *statement_expression* in [Expression statements](statements.md#expression-statements) then includes only *null_conditional_invocation_expression*.</span></span>


### <a name="unary-plus-operator"></a><span data-ttu-id="212c8-1534">Operador unario más</span><span class="sxs-lookup"><span data-stu-id="212c8-1534">Unary plus operator</span></span>

<span data-ttu-id="212c8-1535">Para una operación con el formato `+x`, se aplica la resolución de sobrecargas del operador unario ([resolución de sobrecarga del operador unario](expressions.md#unary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1535">For an operation of the form `+x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-1536">El operando se convierte al tipo de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1536">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="212c8-1537">Los operadores unarios más predefinidos más son:</span><span class="sxs-lookup"><span data-stu-id="212c8-1537">The predefined unary plus operators are:</span></span>

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

<span data-ttu-id="212c8-1538">Para cada uno de estos operadores, el resultado es simplemente el valor del operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-1538">For each of these operators, the result is simply the value of the operand.</span></span>

### <a name="unary-minus-operator"></a><span data-ttu-id="212c8-1539">Operador unario menos</span><span class="sxs-lookup"><span data-stu-id="212c8-1539">Unary minus operator</span></span>

<span data-ttu-id="212c8-1540">Para una operación con el formato `-x`, se aplica la resolución de sobrecargas del operador unario ([resolución de sobrecarga del operador unario](expressions.md#unary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1540">For an operation of the form `-x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-1541">El operando se convierte al tipo de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1541">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="212c8-1542">Los operadores de negación predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="212c8-1542">The predefined negation operators are:</span></span>

*  <span data-ttu-id="212c8-1543">Negación de entero:</span><span class="sxs-lookup"><span data-stu-id="212c8-1543">Integer negation:</span></span>

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   <span data-ttu-id="212c8-1544">El resultado se calcula restando `x` de cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1544">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="212c8-1545">Si el valor de `x` es el valor representable más pequeño del tipo de operando (-2 ^ 31 para `int` o-2 ^ 63 para `long`), la negación matemática de `x` no se podrá representar en el tipo de operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-1545">If the value of `x` is the smallest representable value of the operand type (-2^31 for `int` or -2^63 for `long`), then the mathematical negation of `x` is not representable within the operand type.</span></span> <span data-ttu-id="212c8-1546">Si esto ocurre dentro de un contexto `checked`, se produce una `System.OverflowException`; Si se produce dentro de un contexto de @no__t 2, el resultado es el valor del operando y el desbordamiento no se registra.</span><span class="sxs-lookup"><span data-stu-id="212c8-1546">If this occurs within a `checked` context, a `System.OverflowException` is thrown; if it occurs within an `unchecked` context, the result is the value of the operand and the overflow is not reported.</span></span>

   <span data-ttu-id="212c8-1547">Si el operando del operador de negación es de tipo `uint`, se convierte al tipo `long` y el tipo del resultado es `long`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1547">If the operand of the negation operator is of type `uint`, it is converted to type `long`, and the type of the result is `long`.</span></span> <span data-ttu-id="212c8-1548">Una excepción es la regla que permite escribir el 2147483648 valor `int` (-2 ^ 31) como un literal entero decimal ([literales enteros](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1548">An exception is the rule that permits the `int` value -2147483648 (-2^31) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

   <span data-ttu-id="212c8-1549">Si el operando del operador de negación es de tipo `ulong`, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1549">If the operand of the negation operator is of type `ulong`, a compile-time error occurs.</span></span> <span data-ttu-id="212c8-1550">Una excepción es la regla que permite escribir el valor `long`-9.223.372.036.854.775.808 (-2 ^ 63) como un literal entero decimal ([literales enteros](lexical-structure.md#integer-literals)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1550">An exception is the rule that permits the `long` value -9223372036854775808 (-2^63) to be written as a decimal integer literal ([Integer literals](lexical-structure.md#integer-literals)).</span></span>

*  <span data-ttu-id="212c8-1551">Negación de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="212c8-1551">Floating-point negation:</span></span>

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   <span data-ttu-id="212c8-1552">El resultado es el valor de `x` con el signo invertido.</span><span class="sxs-lookup"><span data-stu-id="212c8-1552">The result is the value of `x` with its sign inverted.</span></span> <span data-ttu-id="212c8-1553">Si `x` es NaN, el resultado es también NaN.</span><span class="sxs-lookup"><span data-stu-id="212c8-1553">If `x` is NaN, the result is also NaN.</span></span>

*  <span data-ttu-id="212c8-1554">Negación decimal:</span><span class="sxs-lookup"><span data-stu-id="212c8-1554">Decimal negation:</span></span>

   ```csharp
   decimal operator -(decimal x);
   ```

   <span data-ttu-id="212c8-1555">El resultado se calcula restando `x` de cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1555">The result is computed by subtracting `x` from zero.</span></span> <span data-ttu-id="212c8-1556">La negación decimal es equivalente a usar el operador unario menos de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1556">Decimal negation is equivalent to using the unary minus operator of type `System.Decimal`.</span></span>

### <a name="logical-negation-operator"></a><span data-ttu-id="212c8-1557">Operador lógico de negación</span><span class="sxs-lookup"><span data-stu-id="212c8-1557">Logical negation operator</span></span>

<span data-ttu-id="212c8-1558">Para una operación con el formato `!x`, se aplica la resolución de sobrecargas del operador unario ([resolución de sobrecarga del operador unario](expressions.md#unary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1558">For an operation of the form `!x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-1559">El operando se convierte al tipo de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1559">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="212c8-1560">Solo existe un operador lógico de negación predefinido:</span><span class="sxs-lookup"><span data-stu-id="212c8-1560">Only one predefined logical negation operator exists:</span></span>
```csharp
bool operator !(bool x);
```

<span data-ttu-id="212c8-1561">Este operador calcula la negación lógica del operando: Si el operando es `true`, el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1561">This operator computes the logical negation of the operand: If the operand is `true`, the result is `false`.</span></span> <span data-ttu-id="212c8-1562">Si el operando es `false`, el resultado es `true`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1562">If the operand is `false`, the result is `true`.</span></span>

### <a name="bitwise-complement-operator"></a><span data-ttu-id="212c8-1563">Operador de complemento bit a bit</span><span class="sxs-lookup"><span data-stu-id="212c8-1563">Bitwise complement operator</span></span>

<span data-ttu-id="212c8-1564">Para una operación con el formato `~x`, se aplica la resolución de sobrecargas del operador unario ([resolución de sobrecarga del operador unario](expressions.md#unary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1564">For an operation of the form `~x`, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-1565">El operando se convierte al tipo de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1565">The operand is converted to the parameter type of the selected operator, and the type of the result is the return type of the operator.</span></span> <span data-ttu-id="212c8-1566">Los operadores de complemento bit a bit predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="212c8-1566">The predefined bitwise complement operators are:</span></span>
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

<span data-ttu-id="212c8-1567">Para cada uno de estos operadores, el resultado de la operación es el complemento bit a bit de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1567">For each of these operators, the result of the operation is the bitwise complement of `x`.</span></span>

<span data-ttu-id="212c8-1568">Cada tipo de enumeración `E` proporciona implícitamente el siguiente operador de complemento bit a bit:</span><span class="sxs-lookup"><span data-stu-id="212c8-1568">Every enumeration type `E` implicitly provides the following bitwise complement operator:</span></span>

```csharp
E operator ~(E x);
```

<span data-ttu-id="212c8-1569">El resultado de evaluar `~x`, donde `x` es una expresión de un tipo de enumeración `E` con un tipo subyacente `U`, es exactamente igual que evaluar `(E)(~(U)x)`, salvo que la conversión a `E` siempre se realiza como si estuviera en un contexto de `unchecked` ( [Los operadores Checked y unchecked](expressions.md#the-checked-and-unchecked-operators)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1569">The result of evaluating `~x`, where `x` is an expression of an enumeration type `E` with an underlying type `U`, is exactly the same as evaluating `(E)(~(U)x)`, except that the conversion to `E` is always performed as if in an `unchecked` context ([The checked and unchecked operators](expressions.md#the-checked-and-unchecked-operators)).</span></span>

### <a name="prefix-increment-and-decrement-operators"></a><span data-ttu-id="212c8-1570">Operadores de incremento y decremento prefijos</span><span class="sxs-lookup"><span data-stu-id="212c8-1570">Prefix increment and decrement operators</span></span>

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

<span data-ttu-id="212c8-1571">El operando de una operación de incremento o decremento de prefijo debe ser una expresión clasificada como una variable, un acceso a la propiedad o un acceso a un indexador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1571">The operand of a prefix increment or decrement operation must be an expression classified as a variable, a property access, or an indexer access.</span></span> <span data-ttu-id="212c8-1572">El resultado de la operación es un valor del mismo tipo que el operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-1572">The result of the operation is a value of the same type as the operand.</span></span>

<span data-ttu-id="212c8-1573">Si el operando de una operación de incremento o decremento de prefijo es una propiedad o un indexador, la propiedad o el indexador deben tener un descriptor de acceso `get` y `set`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1573">If the operand of a prefix increment or decrement operation is a property or indexer access, the property or indexer must have both a `get` and a `set` accessor.</span></span> <span data-ttu-id="212c8-1574">Si no es así, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1574">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-1575">La resolución de sobrecargas de operador unario ([resolución de sobrecarga de operadores unarios](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1575">Unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-1576">Los operadores predefinidos `++` y `--` existen para los siguientes tipos: @no__t 2, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 y cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="212c8-1576">Predefined `++` and `--` operators exist for the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, and any enum type.</span></span> <span data-ttu-id="212c8-1577">Los operadores predefinidos `++` devuelven el valor generado agregando 1 al operando y los operadores `--` predefinidos devuelven el valor generado restando 1 del operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-1577">The predefined `++` operators return the value produced by adding 1 to the operand, and the predefined `--` operators return the value produced by subtracting 1 from the operand.</span></span> <span data-ttu-id="212c8-1578">En un contexto `checked`, si el resultado de esta suma o resta está fuera del intervalo del tipo de resultado y el tipo de resultado es un tipo entero o un tipo de enumeración, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1578">In a `checked` context, if the result of this addition or subtraction is outside the range of the result type and the result type is an integral type or enum type, a `System.OverflowException` is thrown.</span></span>

<span data-ttu-id="212c8-1579">El procesamiento en tiempo de ejecución de una operación de incremento o decremento de prefijo con el formato `++x` o `--x` consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-1579">The run-time processing of a prefix increment or decrement operation of the form `++x` or `--x` consists of the following steps:</span></span>

*   <span data-ttu-id="212c8-1580">Si `x` está clasificado como una variable:</span><span class="sxs-lookup"><span data-stu-id="212c8-1580">If `x` is classified as a variable:</span></span>
    * <span data-ttu-id="212c8-1581">`x` se evalúa para generar la variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-1581">`x` is evaluated to produce the variable.</span></span>
    * <span data-ttu-id="212c8-1582">El operador seleccionado se invoca con el valor de `x` como argumento.</span><span class="sxs-lookup"><span data-stu-id="212c8-1582">The selected operator is invoked with the value of `x` as its argument.</span></span>
    * <span data-ttu-id="212c8-1583">El valor devuelto por el operador se almacena en la ubicación proporcionada por la evaluación de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1583">The value returned by the operator is stored in the location given by the evaluation of `x`.</span></span>
    * <span data-ttu-id="212c8-1584">El valor devuelto por el operador se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1584">The value returned by the operator becomes the result of the operation.</span></span>
*   <span data-ttu-id="212c8-1585">Si `x` se clasifica como un acceso de propiedad o indizador:</span><span class="sxs-lookup"><span data-stu-id="212c8-1585">If `x` is classified as a property or indexer access:</span></span>
    * <span data-ttu-id="212c8-1586">La expresión de instancia (si `x` no es `static`) y la lista de argumentos (si `x` es un indexador) asociada a `x` se evalúan, y los resultados se usan en las siguientes invocaciones de descriptor de acceso `get` y `set`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1586">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `get` and `set` accessor invocations.</span></span>
    * <span data-ttu-id="212c8-1587">Se invoca al descriptor de acceso `get` de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1587">The `get` accessor of `x` is invoked.</span></span>
    * <span data-ttu-id="212c8-1588">El operador seleccionado se invoca con el valor devuelto por el descriptor de acceso `get` como su argumento.</span><span class="sxs-lookup"><span data-stu-id="212c8-1588">The selected operator is invoked with the value returned by the `get` accessor as its argument.</span></span>
    * <span data-ttu-id="212c8-1589">El descriptor de acceso `set` de `x` se invoca con el valor devuelto por el operador como su argumento `value`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1589">The `set` accessor of `x` is invoked with the value returned by the operator as its `value` argument.</span></span>
    * <span data-ttu-id="212c8-1590">El valor devuelto por el operador se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1590">The value returned by the operator becomes the result of the operation.</span></span>

<span data-ttu-id="212c8-1591">Los operadores `++` y `--` también admiten la notación de postfijo ([operadores de incremento y decremento de postfijo](expressions.md#postfix-increment-and-decrement-operators)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1591">The `++` and `--` operators also support postfix notation ([Postfix increment and decrement operators](expressions.md#postfix-increment-and-decrement-operators)).</span></span> <span data-ttu-id="212c8-1592">Normalmente, el resultado de `x++` o `x--` es el valor de @no__t 2 antes de la operación, mientras que el resultado de `++x` o `--x` es el valor de `x` después de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1592">Typically, the result of `x++` or `x--` is the value of `x` before the operation, whereas the result of `++x` or `--x` is the value of `x` after the operation.</span></span> <span data-ttu-id="212c8-1593">En cualquier caso, `x` tiene el mismo valor después de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1593">In either case, `x` itself has the same value after the operation.</span></span>

<span data-ttu-id="212c8-1594">Se puede invocar una implementación `operator++` o `operator--` mediante la notación de sufijo o de prefijo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1594">An `operator++` or `operator--` implementation can be invoked using either postfix or prefix notation.</span></span> <span data-ttu-id="212c8-1595">No es posible tener implementaciones de operador independientes para las dos notaciones.</span><span class="sxs-lookup"><span data-stu-id="212c8-1595">It is not possible to have separate operator implementations for the two notations.</span></span>

### <a name="cast-expressions"></a><span data-ttu-id="212c8-1596">Expresiones de conversión</span><span class="sxs-lookup"><span data-stu-id="212c8-1596">Cast expressions</span></span>

<span data-ttu-id="212c8-1597">Se usa un *cast_expression* para convertir explícitamente una expresión a un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1597">A *cast_expression* is used to explicitly convert an expression to a given type.</span></span>

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

<span data-ttu-id="212c8-1598">Un *cast_expression* con el formato `(T)E`, donde `T` es un *tipo* y `E` es *unary_expression*, realiza una conversión explícita ([conversiones explícitas](conversions.md#explicit-conversions)) del valor de `E` al tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1598">A *cast_expression* of the form `(T)E`, where `T` is a *type* and `E` is a *unary_expression*, performs an explicit conversion ([Explicit conversions](conversions.md#explicit-conversions)) of the value of `E` to type `T`.</span></span> <span data-ttu-id="212c8-1599">Si no existe una conversión explícita de `E` a `T`, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-1599">If no explicit conversion exists from `E` to `T`, a binding-time error occurs.</span></span> <span data-ttu-id="212c8-1600">De lo contrario, el resultado es el valor generado por la conversión explícita.</span><span class="sxs-lookup"><span data-stu-id="212c8-1600">Otherwise, the result is the value produced by the explicit conversion.</span></span> <span data-ttu-id="212c8-1601">El resultado siempre se clasifica como un valor, incluso si `E` denota una variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-1601">The result is always classified as a value, even if `E` denotes a variable.</span></span>

<span data-ttu-id="212c8-1602">La gramática de una *cast_expression* conduce a ciertas ambigüedades sintácticas.</span><span class="sxs-lookup"><span data-stu-id="212c8-1602">The grammar for a *cast_expression* leads to certain syntactic ambiguities.</span></span> <span data-ttu-id="212c8-1603">Por ejemplo, la expresión `(x)-y` puede interpretarse como *cast_expression* (una conversión de `-y` al tipo `x`) o como un *additive_expression* combinado con *parenthesized_expression* (que calcula el valor `x - y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1603">For example, the expression `(x)-y` could either be interpreted as a *cast_expression* (a cast of `-y` to type `x`) or as an *additive_expression* combined with a *parenthesized_expression* (which computes the value `x - y)`.</span></span>

<span data-ttu-id="212c8-1604">Para resolver ambigüedades de *cast_expression* , existe la siguiente regla: Una secuencia de uno o más *tokens*([espacio en blanco](lexical-structure.md#white-space)) entre paréntesis se considera el inicio de un *cast_expression* solo si se cumple al menos una de las siguientes condiciones:</span><span class="sxs-lookup"><span data-stu-id="212c8-1604">To resolve *cast_expression* ambiguities, the following rule exists: A sequence of one or more *token*s ([White space](lexical-structure.md#white-space)) enclosed in parentheses is considered the start of a *cast_expression* only if at least one of the following are true:</span></span>

*  <span data-ttu-id="212c8-1605">La secuencia de tokens es la gramática correcta para un *tipo*, pero no para una *expresión*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1605">The sequence of tokens is correct grammar for a *type*, but not for an *expression*.</span></span>
*  <span data-ttu-id="212c8-1606">La secuencia de tokens es la gramática correcta para un *tipo*, y el token que sigue inmediatamente al paréntesis de cierre es el token "`~`", el token "`!`", el token "`(`", un *identificador* (secuencias de[escape de caracteres Unicode ](lexical-structure.md#unicode-character-escape-sequences)), un *literal* ([literales](lexical-structure.md#literals)) o cualquier *palabra clave* ([keywords](lexical-structure.md#keywords)) excepto 0 y 1.</span><span class="sxs-lookup"><span data-stu-id="212c8-1606">The sequence of tokens is correct grammar for a *type*, and the token immediately following the closing parentheses is the token "`~`", the token "`!`", the token "`(`", an *identifier* ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)), a *literal* ([Literals](lexical-structure.md#literals)), or any *keyword* ([Keywords](lexical-structure.md#keywords)) except `as` and `is`.</span></span>

<span data-ttu-id="212c8-1607">El término "gramática correcta" anterior significa que la secuencia de tokens debe ajustarse a la producción gramatical concreta.</span><span class="sxs-lookup"><span data-stu-id="212c8-1607">The term "correct grammar" above means only that the sequence of tokens must conform to the particular grammatical production.</span></span> <span data-ttu-id="212c8-1608">En concreto, no se tiene en cuenta el significado real de los identificadores constituyentes.</span><span class="sxs-lookup"><span data-stu-id="212c8-1608">It specifically does not consider the actual meaning of any constituent identifiers.</span></span> <span data-ttu-id="212c8-1609">Por ejemplo, si `x` y `y` son identificadores, `x.y` es la gramática correcta para un tipo, incluso si `x.y` no denota realmente un tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1609">For example, if `x` and `y` are identifiers, then `x.y` is correct grammar for a type, even if `x.y` doesn't actually denote a type.</span></span>

<span data-ttu-id="212c8-1610">En la regla de desambiguación se sigue que, si `x` y `y` son identificadores, @no__t 2, `(x)(y)` y `(x)(-y)` son *cast_expression*, pero `(x)-y` no es, aunque `x` identifique un tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1610">From the disambiguation rule it follows that, if `x` and `y` are identifiers, `(x)y`, `(x)(y)`, and `(x)(-y)` are *cast_expression*s, but `(x)-y` is not, even if `x` identifies a type.</span></span> <span data-ttu-id="212c8-1611">Sin embargo, si `x` es una palabra clave que identifica un tipo predefinido (como `int`), las cuatro formas son *cast_expression*s (porque tal palabra clave podría ser una expresión por sí misma).</span><span class="sxs-lookup"><span data-stu-id="212c8-1611">However, if `x` is a keyword that identifies a predefined type (such as `int`), then all four forms are *cast_expression*s (because such a keyword could not possibly be an expression by itself).</span></span>

### <a name="await-expressions"></a><span data-ttu-id="212c8-1612">Expresiones Await</span><span class="sxs-lookup"><span data-stu-id="212c8-1612">Await expressions</span></span>

<span data-ttu-id="212c8-1613">El operador Await se usa para suspender la evaluación de la función asincrónica envolvente hasta que se haya completado la operación asincrónica representada por el operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-1613">The await operator is used to suspend evaluation of the enclosing async function until the asynchronous operation represented by the operand has completed.</span></span>

```antlr
await_expression
    : 'await' unary_expression
    ;
```

<span data-ttu-id="212c8-1614">Un *await_expression* solo se permite en el cuerpo de una función asincrónica ([iteradores](classes.md#iterators)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1614">An *await_expression* is only allowed in the body of an async function ([Iterators](classes.md#iterators)).</span></span> <span data-ttu-id="212c8-1615">Dentro de la función asincrónica envolvente más cercana, puede que no se produzca una *await_expression* en estos lugares:</span><span class="sxs-lookup"><span data-stu-id="212c8-1615">Within the nearest enclosing async function, an *await_expression* may not occur in these places:</span></span>

*  <span data-ttu-id="212c8-1616">Dentro de una función anónima anidada (no asincrónica)</span><span class="sxs-lookup"><span data-stu-id="212c8-1616">Inside a nested (non-async) anonymous function</span></span>
*  <span data-ttu-id="212c8-1617">Dentro del bloque de un *lock_statement*</span><span class="sxs-lookup"><span data-stu-id="212c8-1617">Inside the block of a *lock_statement*</span></span>
*  <span data-ttu-id="212c8-1618">En un contexto no seguro</span><span class="sxs-lookup"><span data-stu-id="212c8-1618">In an unsafe context</span></span>

<span data-ttu-id="212c8-1619">Tenga en cuenta que no se puede producir un *await_expression* en la mayoría de los lugares de una *query_expression*, ya que se transforman sintácticamente para usar expresiones lambda no asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="212c8-1619">Note that an *await_expression* cannot occur in most places within a *query_expression*, because those are syntactically transformed to use non-async lambda expressions.</span></span>

<span data-ttu-id="212c8-1620">Dentro de una función asincrónica, `await` no se puede usar como identificador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1620">Inside of an async function, `await` cannot be used as an identifier.</span></span> <span data-ttu-id="212c8-1621">Por lo tanto, no hay ambigüedades sintácticas entre las expresiones Await y varias expresiones que intervienen en identificadores.</span><span class="sxs-lookup"><span data-stu-id="212c8-1621">There is therefore no syntactic ambiguity between await-expressions and various expressions involving identifiers.</span></span> <span data-ttu-id="212c8-1622">Fuera de las funciones asincrónicas, `await` actúa como un identificador normal.</span><span class="sxs-lookup"><span data-stu-id="212c8-1622">Outside of async functions, `await` acts as a normal identifier.</span></span>

<span data-ttu-id="212c8-1623">El operando de un *await_expression* se denomina ***tarea***.</span><span class="sxs-lookup"><span data-stu-id="212c8-1623">The operand of an *await_expression* is called the ***task***.</span></span> <span data-ttu-id="212c8-1624">Representa una operación asincrónica que puede o no completarse en el momento en que se evalúa el *await_expression* .</span><span class="sxs-lookup"><span data-stu-id="212c8-1624">It represents an asynchronous operation that may or may not be complete at the time the *await_expression* is evaluated.</span></span> <span data-ttu-id="212c8-1625">El propósito del operador Await es suspender la ejecución de la función asincrónica envolvente hasta que se complete la tarea en espera y, a continuación, obtener el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1625">The purpose of the await operator is to suspend execution of the enclosing async function until the awaited task is complete, and then obtain its outcome.</span></span>

#### <a name="awaitable-expressions"></a><span data-ttu-id="212c8-1626">Expresiones que admiten Await</span><span class="sxs-lookup"><span data-stu-id="212c8-1626">Awaitable expressions</span></span>

<span data-ttu-id="212c8-1627">Es necesario que la tarea de una expresión Await sea ***Await***.</span><span class="sxs-lookup"><span data-stu-id="212c8-1627">The task of an await expression is required to be ***awaitable***.</span></span> <span data-ttu-id="212c8-1628">Una expresión `t` es que admite Await si uno de los siguientes elementos contiene:</span><span class="sxs-lookup"><span data-stu-id="212c8-1628">An expression `t` is awaitable if one of the following holds:</span></span>

*  <span data-ttu-id="212c8-1629">`t` es del tipo de tiempo de compilación `dynamic`</span><span class="sxs-lookup"><span data-stu-id="212c8-1629">`t` is of compile time type `dynamic`</span></span>
*  <span data-ttu-id="212c8-1630">`t` tiene una instancia o un método de extensión accesible denominado `GetAwaiter` sin parámetros y sin parámetros de tipo, y un tipo de valor devuelto `A` para el que se mantiene todo lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-1630">`t` has an accessible instance or extension method called `GetAwaiter` with no parameters and no type parameters, and a return type `A` for which all of the following hold:</span></span>
   * <span data-ttu-id="212c8-1631">`A` implementa la interfaz `System.Runtime.CompilerServices.INotifyCompletion` (en adelante denominada `INotifyCompletion` por motivos de brevedad)</span><span class="sxs-lookup"><span data-stu-id="212c8-1631">`A` implements the interface `System.Runtime.CompilerServices.INotifyCompletion` (hereafter known as `INotifyCompletion` for brevity)</span></span>
   * <span data-ttu-id="212c8-1632">`A` tiene una propiedad de instancia accesible y legible `IsCompleted` de tipo `bool`</span><span class="sxs-lookup"><span data-stu-id="212c8-1632">`A` has an accessible, readable instance property `IsCompleted` of type `bool`</span></span>
   * <span data-ttu-id="212c8-1633">`A` tiene un método de instancia accesible `GetResult` sin parámetros y sin parámetros de tipo</span><span class="sxs-lookup"><span data-stu-id="212c8-1633">`A` has an accessible instance method `GetResult` with no parameters and no type parameters</span></span>

<span data-ttu-id="212c8-1634">El propósito del método `GetAwaiter` es obtener un objeto ***Await*** para la tarea.</span><span class="sxs-lookup"><span data-stu-id="212c8-1634">The purpose of the `GetAwaiter` method is to obtain an ***awaiter*** for the task.</span></span> <span data-ttu-id="212c8-1635">El tipo `A` se denomina ***tipo de espera*** para la expresión Await.</span><span class="sxs-lookup"><span data-stu-id="212c8-1635">The type `A` is called the ***awaiter type*** for the await expression.</span></span>

<span data-ttu-id="212c8-1636">El propósito de la propiedad `IsCompleted` es determinar si la tarea ya se ha completado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1636">The purpose of the `IsCompleted` property is to determine if the task is already complete.</span></span> <span data-ttu-id="212c8-1637">Si es así, no es necesario suspender la evaluación.</span><span class="sxs-lookup"><span data-stu-id="212c8-1637">If so, there is no need to suspend evaluation.</span></span>

<span data-ttu-id="212c8-1638">El propósito del método `INotifyCompletion.OnCompleted` es registrar una "continuación" en la tarea; es decir, un delegado (de tipo `System.Action`) que se invocará una vez completada la tarea.</span><span class="sxs-lookup"><span data-stu-id="212c8-1638">The purpose of the `INotifyCompletion.OnCompleted` method is to sign up a "continuation" to the task; i.e. a delegate (of type `System.Action`) that will be invoked once the task is complete.</span></span>

<span data-ttu-id="212c8-1639">El propósito del método `GetResult` es obtener el resultado de la tarea una vez completada.</span><span class="sxs-lookup"><span data-stu-id="212c8-1639">The purpose of the `GetResult` method is to obtain the outcome of the task once it is complete.</span></span> <span data-ttu-id="212c8-1640">Este resultado puede ser una finalización correcta, posiblemente con un valor de resultado, o puede ser una excepción producida por el método `GetResult`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1640">This outcome may be successful completion, possibly with a result value, or it may be an exception which is thrown by the `GetResult` method.</span></span>

#### <a name="classification-of-await-expressions"></a><span data-ttu-id="212c8-1641">Clasificación de expresiones Await</span><span class="sxs-lookup"><span data-stu-id="212c8-1641">Classification of await expressions</span></span>

<span data-ttu-id="212c8-1642">La expresión `await t` se clasifica de la misma forma que la expresión `(t).GetAwaiter().GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1642">The expression `await t` is classified the same way as the expression `(t).GetAwaiter().GetResult()`.</span></span> <span data-ttu-id="212c8-1643">Por lo tanto, si el tipo de valor devuelto de `GetResult` es `void`, el *await_expression* se clasifica como Nothing.</span><span class="sxs-lookup"><span data-stu-id="212c8-1643">Thus, if the return type of `GetResult` is `void`, the *await_expression* is classified as nothing.</span></span> <span data-ttu-id="212c8-1644">Si tiene un tipo de valor devuelto distinto de void `T`, el *await_expression* se clasifica como un valor de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1644">If it has a non-void return type `T`, the *await_expression* is classified as a value of type `T`.</span></span>

#### <a name="runtime-evaluation-of-await-expressions"></a><span data-ttu-id="212c8-1645">Evaluación en tiempo de ejecución de expresiones Await</span><span class="sxs-lookup"><span data-stu-id="212c8-1645">Runtime evaluation of await expressions</span></span>

<span data-ttu-id="212c8-1646">En tiempo de ejecución, la expresión `await t` se evalúa como sigue:</span><span class="sxs-lookup"><span data-stu-id="212c8-1646">At runtime, the expression `await t` is evaluated as follows:</span></span>

*  <span data-ttu-id="212c8-1647">Se obtiene un Await `a` evaluando la expresión `(t).GetAwaiter()`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1647">An awaiter `a` is obtained by evaluating the expression `(t).GetAwaiter()`.</span></span>
*  <span data-ttu-id="212c8-1648">Se obtiene un `bool` `b` evaluando la expresión `(a).IsCompleted`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1648">A `bool` `b` is obtained by evaluating the expression `(a).IsCompleted`.</span></span>
*  <span data-ttu-id="212c8-1649">Si `b` es `false`, la evaluación depende de si `a` implementa la interfaz `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (en adelante conocida como `ICriticalNotifyCompletion` por motivos de brevedad).</span><span class="sxs-lookup"><span data-stu-id="212c8-1649">If `b` is `false` then evaluation depends on whether `a` implements the interface `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (hereafter known as `ICriticalNotifyCompletion` for brevity).</span></span> <span data-ttu-id="212c8-1650">Esta comprobación se realiza en el momento del enlace; es decir, en tiempo de ejecución si `a` tiene el tipo de tiempo de compilación `dynamic` y en tiempo de compilación, de lo contrario,.</span><span class="sxs-lookup"><span data-stu-id="212c8-1650">This check is done at binding time; i.e. at runtime if `a` has the compile time type `dynamic`, and at compile time otherwise.</span></span> <span data-ttu-id="212c8-1651">Permita que `r` denote el delegado de reanudación ([iteradores](classes.md#iterators)):</span><span class="sxs-lookup"><span data-stu-id="212c8-1651">Let `r` denote the resumption delegate ([Iterators](classes.md#iterators)):</span></span>
    * <span data-ttu-id="212c8-1652">Si `a` no implementa `ICriticalNotifyCompletion`, se evalúa la expresión `(a as (INotifyCompletion)).OnCompleted(r)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1652">If `a` does not implement `ICriticalNotifyCompletion`, then the expression `(a as (INotifyCompletion)).OnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="212c8-1653">Si `a` implementa `ICriticalNotifyCompletion`, se evalúa la expresión `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1653">If `a` does implement `ICriticalNotifyCompletion`, then the expression `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` is evaluated.</span></span>
    * <span data-ttu-id="212c8-1654">A continuación, la evaluación se suspende y el control se devuelve al autor de la llamada actual de la función asincrónica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1654">Evaluation is then suspended, and control is returned to the current caller of the async function.</span></span>
*  <span data-ttu-id="212c8-1655">Inmediatamente después de (si `b` era `true`) o tras una invocación posterior del delegado de reanudación (si `b` era `false`), se evalúa la expresión `(a).GetResult()`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1655">Either immediately after (if `b` was `true`), or upon later invocation of the resumption delegate (if `b` was `false`), the expression `(a).GetResult()` is evaluated.</span></span> <span data-ttu-id="212c8-1656">Si devuelve un valor, ese valor es el resultado de *await_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-1656">If it returns a value, that value is the result of the *await_expression*.</span></span> <span data-ttu-id="212c8-1657">De lo contrario, el resultado es Nothing.</span><span class="sxs-lookup"><span data-stu-id="212c8-1657">Otherwise the result is nothing.</span></span>

<span data-ttu-id="212c8-1658">La implementación de un espera de los métodos de interfaz `INotifyCompletion.OnCompleted` y `ICriticalNotifyCompletion.UnsafeOnCompleted` debe hacer que el delegado `r` se invoque como máximo una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-1658">An awaiter's implementation of the interface methods `INotifyCompletion.OnCompleted` and `ICriticalNotifyCompletion.UnsafeOnCompleted` should cause the delegate `r` to be invoked at most once.</span></span> <span data-ttu-id="212c8-1659">De lo contrario, el comportamiento de la función asincrónica envolvente es indefinido.</span><span class="sxs-lookup"><span data-stu-id="212c8-1659">Otherwise, the behavior of the enclosing async function is undefined.</span></span>

## <a name="arithmetic-operators"></a><span data-ttu-id="212c8-1660">Operadores aritméticos</span><span class="sxs-lookup"><span data-stu-id="212c8-1660">Arithmetic operators</span></span>

<span data-ttu-id="212c8-1661">Los operadores `*`, `/`, `%`, `+` y `-` se denominan operadores aritméticos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1661">The `*`, `/`, `%`, `+`, and `-` operators are called the arithmetic operators.</span></span>

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

<span data-ttu-id="212c8-1662">Si un operando de un operador aritmético tiene el tipo en tiempo de compilación `dynamic`, la expresión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="212c8-1662">If an operand of an arithmetic operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="212c8-1663">En este caso, el tipo en tiempo de compilación de la expresión es `dynamic`, y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución de los operandos que tienen el tipo en tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1663">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

### <a name="multiplication-operator"></a><span data-ttu-id="212c8-1664">Operador de multiplicación</span><span class="sxs-lookup"><span data-stu-id="212c8-1664">Multiplication operator</span></span>

<span data-ttu-id="212c8-1665">Para una operación con el formato `x * y`, se aplica la resolución de sobrecargas del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1665">For an operation of the form `x * y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-1666">Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1666">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="212c8-1667">A continuación se enumeran los operadores de multiplicación predefinidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1667">The predefined multiplication operators are listed below.</span></span> <span data-ttu-id="212c8-1668">Todos los operadores calculan el producto de `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1668">The operators all compute the product of `x` and `y`.</span></span>

*  <span data-ttu-id="212c8-1669">Multiplicación de enteros:</span><span class="sxs-lookup"><span data-stu-id="212c8-1669">Integer multiplication:</span></span>

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   <span data-ttu-id="212c8-1670">En un contexto `checked`, si el producto está fuera del intervalo del tipo de resultado, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1670">In a `checked` context, if the product is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="212c8-1671">En un contexto `unchecked`, no se informan los desbordamientos y se descartan los bits significativos de orden superior fuera del intervalo del tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1671">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>


*  <span data-ttu-id="212c8-1672">Multiplicación de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="212c8-1672">Floating-point multiplication:</span></span>

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   <span data-ttu-id="212c8-1673">El producto se calcula de acuerdo con las reglas de aritmética de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="212c8-1673">The product is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="212c8-1674">En la tabla siguiente se enumeran los resultados de todas las posibles combinaciones de valores finitos distintos de cero, ceros, infinitos y NaN.</span><span class="sxs-lookup"><span data-stu-id="212c8-1674">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="212c8-1675">En la tabla, `x` y `y` son valores finitos positivos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1675">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="212c8-1676">`z` es el resultado de `x * y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1676">`z` is the result of `x * y`.</span></span> <span data-ttu-id="212c8-1677">Si el resultado es demasiado grande para el tipo de destino, `z` es infinito.</span><span class="sxs-lookup"><span data-stu-id="212c8-1677">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="212c8-1678">Si el resultado es demasiado pequeño para el tipo de destino, `z` es cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1678">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | <span data-ttu-id="212c8-1679">\+ y</span><span class="sxs-lookup"><span data-stu-id="212c8-1679">+y</span></span>   | <span data-ttu-id="212c8-1680">-y</span><span class="sxs-lookup"><span data-stu-id="212c8-1680">-y</span></span>   | <span data-ttu-id="212c8-1681">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1681">+0</span></span>  | <span data-ttu-id="212c8-1682">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1682">-0</span></span>  | <span data-ttu-id="212c8-1683">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1683">+inf</span></span> | <span data-ttu-id="212c8-1684">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1684">-inf</span></span> | <span data-ttu-id="212c8-1685">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1685">NaN</span></span> | 
   | <span data-ttu-id="212c8-1686">+x</span><span class="sxs-lookup"><span data-stu-id="212c8-1686">+x</span></span>   | <span data-ttu-id="212c8-1687">+z</span><span class="sxs-lookup"><span data-stu-id="212c8-1687">+z</span></span>   | <span data-ttu-id="212c8-1688">-z</span><span class="sxs-lookup"><span data-stu-id="212c8-1688">-z</span></span>   | <span data-ttu-id="212c8-1689">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1689">+0</span></span>  | <span data-ttu-id="212c8-1690">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1690">-0</span></span>  | <span data-ttu-id="212c8-1691">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1691">+inf</span></span> | <span data-ttu-id="212c8-1692">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1692">-inf</span></span> | <span data-ttu-id="212c8-1693">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1693">NaN</span></span> | 
   | <span data-ttu-id="212c8-1694">-x</span><span class="sxs-lookup"><span data-stu-id="212c8-1694">-x</span></span>   | <span data-ttu-id="212c8-1695">-z</span><span class="sxs-lookup"><span data-stu-id="212c8-1695">-z</span></span>   | <span data-ttu-id="212c8-1696">+z</span><span class="sxs-lookup"><span data-stu-id="212c8-1696">+z</span></span>   | <span data-ttu-id="212c8-1697">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1697">-0</span></span>  | <span data-ttu-id="212c8-1698">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1698">+0</span></span>  | <span data-ttu-id="212c8-1699">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1699">-inf</span></span> | <span data-ttu-id="212c8-1700">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1700">+inf</span></span> | <span data-ttu-id="212c8-1701">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1701">NaN</span></span> | 
   | <span data-ttu-id="212c8-1702">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1702">+0</span></span>   | <span data-ttu-id="212c8-1703">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1703">+0</span></span>   | <span data-ttu-id="212c8-1704">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1704">-0</span></span>   | <span data-ttu-id="212c8-1705">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1705">+0</span></span>  | <span data-ttu-id="212c8-1706">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1706">-0</span></span>  | <span data-ttu-id="212c8-1707">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1707">NaN</span></span>  | <span data-ttu-id="212c8-1708">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1708">NaN</span></span>  | <span data-ttu-id="212c8-1709">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1709">NaN</span></span> | 
   | <span data-ttu-id="212c8-1710">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1710">-0</span></span>   | <span data-ttu-id="212c8-1711">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1711">-0</span></span>   | <span data-ttu-id="212c8-1712">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1712">+0</span></span>   | <span data-ttu-id="212c8-1713">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1713">-0</span></span>  | <span data-ttu-id="212c8-1714">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1714">+0</span></span>  | <span data-ttu-id="212c8-1715">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1715">NaN</span></span>  | <span data-ttu-id="212c8-1716">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1716">NaN</span></span>  | <span data-ttu-id="212c8-1717">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1717">NaN</span></span> | 
   | <span data-ttu-id="212c8-1718">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1718">+inf</span></span> | <span data-ttu-id="212c8-1719">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1719">+inf</span></span> | <span data-ttu-id="212c8-1720">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1720">-inf</span></span> | <span data-ttu-id="212c8-1721">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1721">NaN</span></span> | <span data-ttu-id="212c8-1722">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1722">NaN</span></span> | <span data-ttu-id="212c8-1723">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1723">+inf</span></span> | <span data-ttu-id="212c8-1724">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1724">-inf</span></span> | <span data-ttu-id="212c8-1725">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1725">NaN</span></span> | 
   | <span data-ttu-id="212c8-1726">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1726">-inf</span></span> | <span data-ttu-id="212c8-1727">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1727">-inf</span></span> | <span data-ttu-id="212c8-1728">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1728">+inf</span></span> | <span data-ttu-id="212c8-1729">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1729">NaN</span></span> | <span data-ttu-id="212c8-1730">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1730">NaN</span></span> | <span data-ttu-id="212c8-1731">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1731">-inf</span></span> | <span data-ttu-id="212c8-1732">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1732">+inf</span></span> | <span data-ttu-id="212c8-1733">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1733">NaN</span></span> | 
   | <span data-ttu-id="212c8-1734">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1734">NaN</span></span>  | <span data-ttu-id="212c8-1735">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1735">NaN</span></span>  | <span data-ttu-id="212c8-1736">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1736">NaN</span></span>  | <span data-ttu-id="212c8-1737">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1737">NaN</span></span> | <span data-ttu-id="212c8-1738">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1738">NaN</span></span> | <span data-ttu-id="212c8-1739">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1739">NaN</span></span>  | <span data-ttu-id="212c8-1740">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1740">NaN</span></span>  | <span data-ttu-id="212c8-1741">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1741">NaN</span></span> | 

*  <span data-ttu-id="212c8-1742">Multiplicación decimal:</span><span class="sxs-lookup"><span data-stu-id="212c8-1742">Decimal multiplication:</span></span>

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   <span data-ttu-id="212c8-1743">Si el valor resultante es demasiado grande para representarlo en el formato `decimal`, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1743">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="212c8-1744">Si el valor del resultado es demasiado pequeño para representarlo en el formato `decimal`, el resultado es cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1744">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="212c8-1745">La escala del resultado, antes de cualquier redondeo, es la suma de las escalas de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1745">The scale of the result, before any rounding, is the sum of the scales of the two operands.</span></span>

   <span data-ttu-id="212c8-1746">La multiplicación decimal es equivalente a usar el operador de multiplicación de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1746">Decimal multiplication is equivalent to using the multiplication operator of type `System.Decimal`.</span></span>


### <a name="division-operator"></a><span data-ttu-id="212c8-1747">Operador de división</span><span class="sxs-lookup"><span data-stu-id="212c8-1747">Division operator</span></span>

<span data-ttu-id="212c8-1748">Para una operación con el formato `x / y`, se aplica la resolución de sobrecargas del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1748">For an operation of the form `x / y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-1749">Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1749">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="212c8-1750">A continuación se enumeran los operadores de división predefinidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1750">The predefined division operators are listed below.</span></span> <span data-ttu-id="212c8-1751">Todos los operadores calculan el cociente de `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1751">The operators all compute the quotient of `x` and `y`.</span></span>

*  <span data-ttu-id="212c8-1752">División de enteros:</span><span class="sxs-lookup"><span data-stu-id="212c8-1752">Integer division:</span></span>

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   <span data-ttu-id="212c8-1753">Si el valor del operando derecho es cero, se produce una `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1753">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="212c8-1754">La división redondea el resultado hacia cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1754">The division rounds the result towards zero.</span></span> <span data-ttu-id="212c8-1755">Por lo tanto, el valor absoluto del resultado es el entero posible más grande que sea menor o igual que el valor absoluto del cociente de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1755">Thus the absolute value of the result is the largest possible integer that is less than or equal to the absolute value of the quotient of the two operands.</span></span> <span data-ttu-id="212c8-1756">El resultado es cero o positivo cuando los dos operandos tienen el mismo signo y cero o negativo cuando los dos operandos tienen signos opuestos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1756">The result is zero or positive when the two operands have the same sign and zero or negative when the two operands have opposite signs.</span></span>

   <span data-ttu-id="212c8-1757">Si el operando izquierdo es el valor más pequeño que se va a representar `int` o `long` y el operando derecho es `-1`, se produce un desbordamiento.</span><span class="sxs-lookup"><span data-stu-id="212c8-1757">If the left operand is the smallest representable `int` or `long` value and the right operand is `-1`, an overflow occurs.</span></span> <span data-ttu-id="212c8-1758">En un contexto `checked`, esto hace que se produzca una `System.ArithmeticException` (o una subclase).</span><span class="sxs-lookup"><span data-stu-id="212c8-1758">In a `checked` context, this causes a `System.ArithmeticException` (or a subclass thereof) to be thrown.</span></span> <span data-ttu-id="212c8-1759">En un contexto `unchecked`, se define como si se produjera una `System.ArithmeticException` (o una subclase de ella) o el desbordamiento se desinforma de que el valor resultante es el del operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1759">In an `unchecked` context, it is implementation-defined as to whether a `System.ArithmeticException` (or a subclass thereof) is thrown or the overflow goes unreported with the resulting value being that of the left operand.</span></span>

*  <span data-ttu-id="212c8-1760">División de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="212c8-1760">Floating-point division:</span></span>

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   <span data-ttu-id="212c8-1761">El cociente se calcula de acuerdo con las reglas de aritmética de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="212c8-1761">The quotient is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="212c8-1762">En la tabla siguiente se enumeran los resultados de todas las posibles combinaciones de valores finitos distintos de cero, ceros, infinitos y NaN.</span><span class="sxs-lookup"><span data-stu-id="212c8-1762">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="212c8-1763">En la tabla, `x` y `y` son valores finitos positivos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1763">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="212c8-1764">`z` es el resultado de `x / y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1764">`z` is the result of `x / y`.</span></span> <span data-ttu-id="212c8-1765">Si el resultado es demasiado grande para el tipo de destino, `z` es infinito.</span><span class="sxs-lookup"><span data-stu-id="212c8-1765">If the result is too large for the destination type, `z` is infinity.</span></span> <span data-ttu-id="212c8-1766">Si el resultado es demasiado pequeño para el tipo de destino, `z` es cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1766">If the result is too small for the destination type, `z` is zero.</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="212c8-1767">\+ y</span><span class="sxs-lookup"><span data-stu-id="212c8-1767">+y</span></span>   | <span data-ttu-id="212c8-1768">-y</span><span class="sxs-lookup"><span data-stu-id="212c8-1768">-y</span></span>   | <span data-ttu-id="212c8-1769">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1769">+0</span></span>   | <span data-ttu-id="212c8-1770">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1770">-0</span></span>   | <span data-ttu-id="212c8-1771">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1771">+inf</span></span> | <span data-ttu-id="212c8-1772">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1772">-inf</span></span> | <span data-ttu-id="212c8-1773">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1773">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1774">+x</span><span class="sxs-lookup"><span data-stu-id="212c8-1774">+x</span></span>   | <span data-ttu-id="212c8-1775">+z</span><span class="sxs-lookup"><span data-stu-id="212c8-1775">+z</span></span>   | <span data-ttu-id="212c8-1776">-z</span><span class="sxs-lookup"><span data-stu-id="212c8-1776">-z</span></span>   | <span data-ttu-id="212c8-1777">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1777">+inf</span></span> | <span data-ttu-id="212c8-1778">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1778">-inf</span></span> | <span data-ttu-id="212c8-1779">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1779">+0</span></span>   | <span data-ttu-id="212c8-1780">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1780">-0</span></span>   | <span data-ttu-id="212c8-1781">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1781">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1782">-x</span><span class="sxs-lookup"><span data-stu-id="212c8-1782">-x</span></span>   | <span data-ttu-id="212c8-1783">-z</span><span class="sxs-lookup"><span data-stu-id="212c8-1783">-z</span></span>   | <span data-ttu-id="212c8-1784">+z</span><span class="sxs-lookup"><span data-stu-id="212c8-1784">+z</span></span>   | <span data-ttu-id="212c8-1785">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1785">-inf</span></span> | <span data-ttu-id="212c8-1786">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1786">+inf</span></span> | <span data-ttu-id="212c8-1787">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1787">-0</span></span>   | <span data-ttu-id="212c8-1788">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1788">+0</span></span>   | <span data-ttu-id="212c8-1789">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1789">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1790">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1790">+0</span></span>   | <span data-ttu-id="212c8-1791">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1791">+0</span></span>   | <span data-ttu-id="212c8-1792">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1792">-0</span></span>   | <span data-ttu-id="212c8-1793">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1793">NaN</span></span>  | <span data-ttu-id="212c8-1794">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1794">NaN</span></span>  | <span data-ttu-id="212c8-1795">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1795">+0</span></span>   | <span data-ttu-id="212c8-1796">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1796">-0</span></span>   | <span data-ttu-id="212c8-1797">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1797">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1798">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1798">-0</span></span>   | <span data-ttu-id="212c8-1799">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1799">-0</span></span>   | <span data-ttu-id="212c8-1800">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1800">+0</span></span>   | <span data-ttu-id="212c8-1801">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1801">NaN</span></span>  | <span data-ttu-id="212c8-1802">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1802">NaN</span></span>  | <span data-ttu-id="212c8-1803">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1803">-0</span></span>   | <span data-ttu-id="212c8-1804">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1804">+0</span></span>   | <span data-ttu-id="212c8-1805">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1805">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1806">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1806">+inf</span></span> | <span data-ttu-id="212c8-1807">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1807">+inf</span></span> | <span data-ttu-id="212c8-1808">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1808">-inf</span></span> | <span data-ttu-id="212c8-1809">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1809">+inf</span></span> | <span data-ttu-id="212c8-1810">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1810">-inf</span></span> | <span data-ttu-id="212c8-1811">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1811">NaN</span></span>  | <span data-ttu-id="212c8-1812">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1812">NaN</span></span>  | <span data-ttu-id="212c8-1813">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1813">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1814">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1814">-inf</span></span> | <span data-ttu-id="212c8-1815">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1815">-inf</span></span> | <span data-ttu-id="212c8-1816">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1816">+inf</span></span> | <span data-ttu-id="212c8-1817">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1817">-inf</span></span> | <span data-ttu-id="212c8-1818">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1818">+inf</span></span> | <span data-ttu-id="212c8-1819">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1819">NaN</span></span>  | <span data-ttu-id="212c8-1820">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1820">NaN</span></span>  | <span data-ttu-id="212c8-1821">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1821">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1822">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1822">NaN</span></span>  | <span data-ttu-id="212c8-1823">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1823">NaN</span></span>  | <span data-ttu-id="212c8-1824">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1824">NaN</span></span>  | <span data-ttu-id="212c8-1825">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1825">NaN</span></span>  | <span data-ttu-id="212c8-1826">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1826">NaN</span></span>  | <span data-ttu-id="212c8-1827">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1827">NaN</span></span>  | <span data-ttu-id="212c8-1828">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1828">NaN</span></span>  | <span data-ttu-id="212c8-1829">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1829">NaN</span></span>  | 

*  <span data-ttu-id="212c8-1830">División decimal:</span><span class="sxs-lookup"><span data-stu-id="212c8-1830">Decimal division:</span></span>

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   <span data-ttu-id="212c8-1831">Si el valor del operando derecho es cero, se produce una `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1831">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="212c8-1832">Si el valor resultante es demasiado grande para representarlo en el formato `decimal`, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1832">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="212c8-1833">Si el valor del resultado es demasiado pequeño para representarlo en el formato `decimal`, el resultado es cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1833">If the result value is too small to represent in the `decimal` format, the result is zero.</span></span> <span data-ttu-id="212c8-1834">La escala del resultado es la menor escala que conservará un resultado igual al valor decimal que se va a representar más cercano al resultado matemático verdadero.</span><span class="sxs-lookup"><span data-stu-id="212c8-1834">The scale of the result is the smallest scale that will preserve a result equal to the nearest representable decimal value to the true mathematical result.</span></span>

   <span data-ttu-id="212c8-1835">La división decimal es equivalente a usar el operador de división de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1835">Decimal division is equivalent to using the division operator of type `System.Decimal`.</span></span>


### <a name="remainder-operator"></a><span data-ttu-id="212c8-1836">Operador de resto</span><span class="sxs-lookup"><span data-stu-id="212c8-1836">Remainder operator</span></span>

<span data-ttu-id="212c8-1837">Para una operación con el formato `x % y`, se aplica la resolución de sobrecargas del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1837">For an operation of the form `x % y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-1838">Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1838">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="212c8-1839">A continuación se enumeran los operadores de resto predefinidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1839">The predefined remainder operators are listed below.</span></span> <span data-ttu-id="212c8-1840">Todos los operadores calculan el resto de la división entre `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1840">The operators all compute the remainder of the division between `x` and `y`.</span></span>

*  <span data-ttu-id="212c8-1841">Resto entero:</span><span class="sxs-lookup"><span data-stu-id="212c8-1841">Integer remainder:</span></span>

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   <span data-ttu-id="212c8-1842">El resultado de `x % y` es el valor generado por `x - (x / y) * y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1842">The result of `x % y` is the value produced by `x - (x / y) * y`.</span></span> <span data-ttu-id="212c8-1843">Si `y` es cero, se produce una `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1843">If `y` is zero, a `System.DivideByZeroException` is thrown.</span></span>

   <span data-ttu-id="212c8-1844">Si el operando izquierdo es el valor más pequeño `int` o `long` y el operando derecho es `-1`, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1844">If the left operand is the smallest `int` or `long` value and the right operand is `-1`, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="212c8-1845">En ningún caso, `x % y` producen una excepción en la que `x / y` no produciría una excepción.</span><span class="sxs-lookup"><span data-stu-id="212c8-1845">In no case does `x % y` throw an exception where `x / y` would not throw an exception.</span></span>

*  <span data-ttu-id="212c8-1846">Resto de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="212c8-1846">Floating-point remainder:</span></span>

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   <span data-ttu-id="212c8-1847">En la tabla siguiente se enumeran los resultados de todas las posibles combinaciones de valores finitos distintos de cero, ceros, infinitos y NaN.</span><span class="sxs-lookup"><span data-stu-id="212c8-1847">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="212c8-1848">En la tabla, `x` y `y` son valores finitos positivos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1848">In the table, `x` and `y` are positive finite values.</span></span> <span data-ttu-id="212c8-1849">`z` es el resultado de `x % y` y se calcula como @no__t 2, donde `n` es el entero posible más grande que sea menor o igual que `x / y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1849">`z` is the result of `x % y` and is computed as `x - n * y`, where `n` is the largest possible integer that is less than or equal to `x / y`.</span></span> <span data-ttu-id="212c8-1850">Este método de calcular el resto es análogo al que se usa para los operandos de enteros, pero difiere de la definición de IEEE 754 (en la que `n` es el entero más próximo a `x / y`).</span><span class="sxs-lookup"><span data-stu-id="212c8-1850">This method of computing the remainder is analogous to that used for integer operands, but differs from the IEEE 754 definition (in which `n` is the integer closest to `x / y`).</span></span>

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="212c8-1851">\+ y</span><span class="sxs-lookup"><span data-stu-id="212c8-1851">+y</span></span>   | <span data-ttu-id="212c8-1852">-y</span><span class="sxs-lookup"><span data-stu-id="212c8-1852">-y</span></span>   | <span data-ttu-id="212c8-1853">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1853">+0</span></span>   | <span data-ttu-id="212c8-1854">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1854">-0</span></span>   | <span data-ttu-id="212c8-1855">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1855">+inf</span></span> | <span data-ttu-id="212c8-1856">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1856">-inf</span></span> | <span data-ttu-id="212c8-1857">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1857">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1858">+x</span><span class="sxs-lookup"><span data-stu-id="212c8-1858">+x</span></span>   | <span data-ttu-id="212c8-1859">+z</span><span class="sxs-lookup"><span data-stu-id="212c8-1859">+z</span></span>   | <span data-ttu-id="212c8-1860">+z</span><span class="sxs-lookup"><span data-stu-id="212c8-1860">+z</span></span>   | <span data-ttu-id="212c8-1861">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1861">NaN</span></span>  | <span data-ttu-id="212c8-1862">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1862">NaN</span></span>  | <span data-ttu-id="212c8-1863">x</span><span class="sxs-lookup"><span data-stu-id="212c8-1863">x</span></span>    | <span data-ttu-id="212c8-1864">x</span><span class="sxs-lookup"><span data-stu-id="212c8-1864">x</span></span>    | <span data-ttu-id="212c8-1865">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1865">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1866">-x</span><span class="sxs-lookup"><span data-stu-id="212c8-1866">-x</span></span>   | <span data-ttu-id="212c8-1867">-z</span><span class="sxs-lookup"><span data-stu-id="212c8-1867">-z</span></span>   | <span data-ttu-id="212c8-1868">-z</span><span class="sxs-lookup"><span data-stu-id="212c8-1868">-z</span></span>   | <span data-ttu-id="212c8-1869">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1869">NaN</span></span>  | <span data-ttu-id="212c8-1870">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1870">NaN</span></span>  | <span data-ttu-id="212c8-1871">-x</span><span class="sxs-lookup"><span data-stu-id="212c8-1871">-x</span></span>   | <span data-ttu-id="212c8-1872">-x</span><span class="sxs-lookup"><span data-stu-id="212c8-1872">-x</span></span>   | <span data-ttu-id="212c8-1873">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1873">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1874">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1874">+0</span></span>   | <span data-ttu-id="212c8-1875">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1875">+0</span></span>   | <span data-ttu-id="212c8-1876">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1876">+0</span></span>   | <span data-ttu-id="212c8-1877">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1877">NaN</span></span>  | <span data-ttu-id="212c8-1878">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1878">NaN</span></span>  | <span data-ttu-id="212c8-1879">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1879">+0</span></span>   | <span data-ttu-id="212c8-1880">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1880">+0</span></span>   | <span data-ttu-id="212c8-1881">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1881">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1882">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1882">-0</span></span>   | <span data-ttu-id="212c8-1883">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1883">-0</span></span>   | <span data-ttu-id="212c8-1884">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1884">-0</span></span>   | <span data-ttu-id="212c8-1885">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1885">NaN</span></span>  | <span data-ttu-id="212c8-1886">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1886">NaN</span></span>  | <span data-ttu-id="212c8-1887">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1887">-0</span></span>   | <span data-ttu-id="212c8-1888">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1888">-0</span></span>   | <span data-ttu-id="212c8-1889">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1889">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1890">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1890">+inf</span></span> | <span data-ttu-id="212c8-1891">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1891">NaN</span></span>  | <span data-ttu-id="212c8-1892">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1892">NaN</span></span>  | <span data-ttu-id="212c8-1893">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1893">NaN</span></span>  | <span data-ttu-id="212c8-1894">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1894">NaN</span></span>  | <span data-ttu-id="212c8-1895">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1895">NaN</span></span>  | <span data-ttu-id="212c8-1896">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1896">NaN</span></span>  | <span data-ttu-id="212c8-1897">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1897">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1898">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1898">-inf</span></span> | <span data-ttu-id="212c8-1899">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1899">NaN</span></span>  | <span data-ttu-id="212c8-1900">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1900">NaN</span></span>  | <span data-ttu-id="212c8-1901">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1901">NaN</span></span>  | <span data-ttu-id="212c8-1902">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1902">NaN</span></span>  | <span data-ttu-id="212c8-1903">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1903">NaN</span></span>  | <span data-ttu-id="212c8-1904">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1904">NaN</span></span>  | <span data-ttu-id="212c8-1905">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1905">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1906">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1906">NaN</span></span>  | <span data-ttu-id="212c8-1907">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1907">NaN</span></span>  | <span data-ttu-id="212c8-1908">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1908">NaN</span></span>  | <span data-ttu-id="212c8-1909">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1909">NaN</span></span>  | <span data-ttu-id="212c8-1910">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1910">NaN</span></span>  | <span data-ttu-id="212c8-1911">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1911">NaN</span></span>  | <span data-ttu-id="212c8-1912">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1912">NaN</span></span>  | <span data-ttu-id="212c8-1913">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1913">NaN</span></span>  | 

*  <span data-ttu-id="212c8-1914">Resto decimal:</span><span class="sxs-lookup"><span data-stu-id="212c8-1914">Decimal remainder:</span></span>

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   <span data-ttu-id="212c8-1915">Si el valor del operando derecho es cero, se produce una `System.DivideByZeroException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1915">If the value of the right operand is zero, a `System.DivideByZeroException` is thrown.</span></span> <span data-ttu-id="212c8-1916">La escala del resultado, antes de cualquier redondeo, es el mayor de las escalas de los dos operandos y el signo del resultado, si es distinto de cero, es el mismo que el de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1916">The scale of the result, before any rounding, is the larger of the scales of the two operands, and the sign of the result, if non-zero, is the same as that of `x`.</span></span>

   <span data-ttu-id="212c8-1917">El resto decimal es equivalente a usar el operador de resto de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1917">Decimal remainder is equivalent to using the remainder operator of type `System.Decimal`.</span></span>


### <a name="addition-operator"></a><span data-ttu-id="212c8-1918">Operador de suma</span><span class="sxs-lookup"><span data-stu-id="212c8-1918">Addition operator</span></span>

<span data-ttu-id="212c8-1919">Para una operación con el formato `x + y`, se aplica la resolución de sobrecargas del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-1919">For an operation of the form `x + y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-1920">Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-1920">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="212c8-1921">A continuación se enumeran los operadores de suma predefinidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1921">The predefined addition operators are listed below.</span></span> <span data-ttu-id="212c8-1922">En el caso de los tipos numéricos y de enumeración, los operadores de suma predefinidos calculan la suma de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1922">For numeric and enumeration types, the predefined addition operators compute the sum of the two operands.</span></span> <span data-ttu-id="212c8-1923">Cuando uno o ambos operandos son de tipo cadena, los operadores de suma predefinidos concatenan la representación de cadena de los operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1923">When one or both operands are of type string, the predefined addition operators concatenate the string representation of the operands.</span></span>

*  <span data-ttu-id="212c8-1924">Suma de enteros:</span><span class="sxs-lookup"><span data-stu-id="212c8-1924">Integer addition:</span></span>

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   <span data-ttu-id="212c8-1925">En un contexto `checked`, si la suma está fuera del intervalo del tipo de resultado, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1925">In a `checked` context, if the sum is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="212c8-1926">En un contexto `unchecked`, no se informan los desbordamientos y se descartan los bits significativos de orden superior fuera del intervalo del tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1926">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="212c8-1927">Adición de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="212c8-1927">Floating-point addition:</span></span>

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   <span data-ttu-id="212c8-1928">La suma se calcula de acuerdo con las reglas de aritmética de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="212c8-1928">The sum is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="212c8-1929">En la tabla siguiente se enumeran los resultados de todas las posibles combinaciones de valores finitos distintos de cero, ceros, infinitos y NaN.</span><span class="sxs-lookup"><span data-stu-id="212c8-1929">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaN's.</span></span> <span data-ttu-id="212c8-1930">En la tabla, `x` y `y` son valores finitos distintos de cero y @no__t 2 es el resultado de `x + y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1930">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x + y`.</span></span> <span data-ttu-id="212c8-1931">Si `x` y `y` tienen la misma magnitud pero signos opuestos, `z` es cero positivo.</span><span class="sxs-lookup"><span data-stu-id="212c8-1931">If `x` and `y` have the same magnitude but opposite signs, `z` is positive zero.</span></span> <span data-ttu-id="212c8-1932">Si `x + y` es demasiado grande para representarlo en el tipo de destino, `z` es un infinito con el mismo signo que @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="212c8-1932">If `x + y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x + y`.</span></span>

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | <span data-ttu-id="212c8-1933">y</span><span class="sxs-lookup"><span data-stu-id="212c8-1933">y</span></span>    | <span data-ttu-id="212c8-1934">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1934">+0</span></span>   | <span data-ttu-id="212c8-1935">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1935">-0</span></span>   | <span data-ttu-id="212c8-1936">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1936">+inf</span></span> | <span data-ttu-id="212c8-1937">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1937">-inf</span></span> | <span data-ttu-id="212c8-1938">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1938">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1939">x</span><span class="sxs-lookup"><span data-stu-id="212c8-1939">x</span></span>    | <span data-ttu-id="212c8-1940">z</span><span class="sxs-lookup"><span data-stu-id="212c8-1940">z</span></span>    | <span data-ttu-id="212c8-1941">x</span><span class="sxs-lookup"><span data-stu-id="212c8-1941">x</span></span>    | <span data-ttu-id="212c8-1942">x</span><span class="sxs-lookup"><span data-stu-id="212c8-1942">x</span></span>    | <span data-ttu-id="212c8-1943">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1943">+inf</span></span> | <span data-ttu-id="212c8-1944">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1944">-inf</span></span> | <span data-ttu-id="212c8-1945">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1945">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1946">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1946">+0</span></span>   | <span data-ttu-id="212c8-1947">y</span><span class="sxs-lookup"><span data-stu-id="212c8-1947">y</span></span>    | <span data-ttu-id="212c8-1948">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1948">+0</span></span>   | <span data-ttu-id="212c8-1949">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1949">+0</span></span>   | <span data-ttu-id="212c8-1950">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1950">+inf</span></span> | <span data-ttu-id="212c8-1951">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1951">-inf</span></span> | <span data-ttu-id="212c8-1952">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1952">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1953">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1953">-0</span></span>   | <span data-ttu-id="212c8-1954">y</span><span class="sxs-lookup"><span data-stu-id="212c8-1954">y</span></span>    | <span data-ttu-id="212c8-1955">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-1955">+0</span></span>   | <span data-ttu-id="212c8-1956">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-1956">-0</span></span>   | <span data-ttu-id="212c8-1957">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1957">+inf</span></span> | <span data-ttu-id="212c8-1958">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1958">-inf</span></span> | <span data-ttu-id="212c8-1959">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1959">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1960">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1960">+inf</span></span> | <span data-ttu-id="212c8-1961">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1961">+inf</span></span> | <span data-ttu-id="212c8-1962">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1962">+inf</span></span> | <span data-ttu-id="212c8-1963">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1963">+inf</span></span> | <span data-ttu-id="212c8-1964">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1964">+inf</span></span> | <span data-ttu-id="212c8-1965">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1965">NaN</span></span>  | <span data-ttu-id="212c8-1966">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1966">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1967">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1967">-inf</span></span> | <span data-ttu-id="212c8-1968">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1968">-inf</span></span> | <span data-ttu-id="212c8-1969">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1969">-inf</span></span> | <span data-ttu-id="212c8-1970">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1970">-inf</span></span> | <span data-ttu-id="212c8-1971">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1971">NaN</span></span>  | <span data-ttu-id="212c8-1972">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-1972">-inf</span></span> | <span data-ttu-id="212c8-1973">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1973">NaN</span></span>  | 
   | <span data-ttu-id="212c8-1974">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1974">NaN</span></span>  | <span data-ttu-id="212c8-1975">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1975">NaN</span></span>  | <span data-ttu-id="212c8-1976">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1976">NaN</span></span>  | <span data-ttu-id="212c8-1977">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1977">NaN</span></span>  | <span data-ttu-id="212c8-1978">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1978">NaN</span></span>  | <span data-ttu-id="212c8-1979">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1979">NaN</span></span>  | <span data-ttu-id="212c8-1980">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-1980">NaN</span></span>  | 

*  <span data-ttu-id="212c8-1981">Suma decimal:</span><span class="sxs-lookup"><span data-stu-id="212c8-1981">Decimal addition:</span></span>

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   <span data-ttu-id="212c8-1982">Si el valor resultante es demasiado grande para representarlo en el formato `decimal`, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1982">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="212c8-1983">La escala del resultado, antes de cualquier redondeo, es el mayor de las escalas de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-1983">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="212c8-1984">La suma decimal es equivalente a usar el operador de suma de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1984">Decimal addition is equivalent to using the addition operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="212c8-1985">Adición de enumeración.</span><span class="sxs-lookup"><span data-stu-id="212c8-1985">Enumeration addition.</span></span> <span data-ttu-id="212c8-1986">Cada tipo de enumeración proporciona implícitamente los siguientes operadores predefinidos, donde `E` es el tipo de enumeración y `U` es el tipo subyacente de `E`:</span><span class="sxs-lookup"><span data-stu-id="212c8-1986">Every enumeration type implicitly provides the following predefined operators, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   <span data-ttu-id="212c8-1987">En tiempo de ejecución, estos operadores se evalúan exactamente como `(E)((U)x + (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1987">At run-time these operators are evaluated exactly as `(E)((U)x + (U)y)`.</span></span>

*  <span data-ttu-id="212c8-1988">Concatenación de cadenas:</span><span class="sxs-lookup"><span data-stu-id="212c8-1988">String concatenation:</span></span>

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   <span data-ttu-id="212c8-1989">Estas sobrecargas del operador binario `+` realizan la concatenación de cadenas.</span><span class="sxs-lookup"><span data-stu-id="212c8-1989">These overloads of the binary `+` operator perform string concatenation.</span></span> <span data-ttu-id="212c8-1990">Si un operando de la concatenación de cadenas es `null`, se sustituye una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="212c8-1990">If an operand of string concatenation is `null`, an empty string is substituted.</span></span> <span data-ttu-id="212c8-1991">De lo contrario, cualquier argumento que no sea de cadena se convierte en su representación de cadena invocando el método virtual `ToString` heredado del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1991">Otherwise, any non-string argument is converted to its string representation by invoking the virtual `ToString` method inherited from type `object`.</span></span> <span data-ttu-id="212c8-1992">Si `ToString` devuelve `null`, se sustituye una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="212c8-1992">If `ToString` returns `null`, an empty string is substituted.</span></span>

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

   El resultado del operador de concatenación de cadenas es una cadena que consta de los caracteres del operando izquierdo seguidos de los caracteres del operando derecho. El operador de concatenación de cadenas nunca devuelve un valor `null`. <span data-ttu-id="212c8-1995">Se puede iniciar una `System.OutOfMemoryException` si no hay suficiente memoria disponible para asignar la cadena resultante.</span><span class="sxs-lookup"><span data-stu-id="212c8-1995">A `System.OutOfMemoryException` may be thrown if there is not enough memory available to allocate the resulting string.</span></span>

*  <span data-ttu-id="212c8-1996">Combinación de delegado.</span><span class="sxs-lookup"><span data-stu-id="212c8-1996">Delegate combination.</span></span> <span data-ttu-id="212c8-1997">Cada tipo de delegado proporciona implícitamente el siguiente operador predefinido, donde `D` es el tipo de delegado:</span><span class="sxs-lookup"><span data-stu-id="212c8-1997">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator +(D x, D y);
   ```

   <span data-ttu-id="212c8-1998">El operador binario `+` realiza la combinación de delegados cuando ambos operandos son de algún tipo de delegado `D`.</span><span class="sxs-lookup"><span data-stu-id="212c8-1998">The binary `+` operator performs delegate combination when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="212c8-1999">(Si los operandos tienen distintos tipos de delegado, se produce un error en tiempo de enlace). Si el primer operando es `null`, el resultado de la operación es el valor del segundo operando (aunque también sea `null`).</span><span class="sxs-lookup"><span data-stu-id="212c8-1999">(If the operands have different delegate types, a binding-time error occurs.) If the first operand is `null`, the result of the operation is the value of the second operand (even if that is also `null`).</span></span> <span data-ttu-id="212c8-2000">De lo contrario, si el segundo operando es `null`, el resultado de la operación es el valor del primer operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-2000">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="212c8-2001">De lo contrario, el resultado de la operación es una nueva instancia de delegado que, cuando se invoca, invoca el primer operando y, a continuación, invoca el segundo operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-2001">Otherwise, the result of the operation is a new delegate instance that, when invoked, invokes the first operand and then invokes the second operand.</span></span> <span data-ttu-id="212c8-2002">Para obtener ejemplos de la combinación de delegados, vea [operador de resta](expressions.md#subtraction-operator) e [invocación de delegado](delegates.md#delegate-invocation).</span><span class="sxs-lookup"><span data-stu-id="212c8-2002">For examples of delegate combination, see [Subtraction operator](expressions.md#subtraction-operator) and [Delegate invocation](delegates.md#delegate-invocation).</span></span> <span data-ttu-id="212c8-2003">Como `System.Delegate` no es un tipo de delegado, `operator` @ no__t-2 no está definido para él.</span><span class="sxs-lookup"><span data-stu-id="212c8-2003">Since `System.Delegate` is not a delegate type, `operator` `+` is not defined for it.</span></span>

### <a name="subtraction-operator"></a><span data-ttu-id="212c8-2004">Operador de resta</span><span class="sxs-lookup"><span data-stu-id="212c8-2004">Subtraction operator</span></span>

<span data-ttu-id="212c8-2005">Para una operación con el formato `x - y`, se aplica la resolución de sobrecargas del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-2005">For an operation of the form `x - y`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-2006">Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-2006">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="212c8-2007">A continuación se enumeran los operadores de resta predefinidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2007">The predefined subtraction operators are listed below.</span></span> <span data-ttu-id="212c8-2008">Todos los operadores restan `y` de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2008">The operators all subtract `y` from `x`.</span></span>

*  <span data-ttu-id="212c8-2009">Resta de enteros:</span><span class="sxs-lookup"><span data-stu-id="212c8-2009">Integer subtraction:</span></span>

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   <span data-ttu-id="212c8-2010">En un contexto `checked`, si la diferencia está fuera del intervalo del tipo de resultado, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2010">In a `checked` context, if the difference is outside the range of the result type, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="212c8-2011">En un contexto `unchecked`, no se informan los desbordamientos y se descartan los bits significativos de orden superior fuera del intervalo del tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2011">In an `unchecked` context, overflows are not reported and any significant high-order bits outside the range of the result type are discarded.</span></span>

*  <span data-ttu-id="212c8-2012">Resta de punto flotante:</span><span class="sxs-lookup"><span data-stu-id="212c8-2012">Floating-point subtraction:</span></span>

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   <span data-ttu-id="212c8-2013">La diferencia se calcula de acuerdo con las reglas de aritmética de IEEE 754.</span><span class="sxs-lookup"><span data-stu-id="212c8-2013">The difference is computed according to the rules of IEEE 754 arithmetic.</span></span> <span data-ttu-id="212c8-2014">En la tabla siguiente se enumeran los resultados de todas las posibles combinaciones de valores finitos distintos de cero, ceros, infinitos y Nan.</span><span class="sxs-lookup"><span data-stu-id="212c8-2014">The following table lists the results of all possible combinations of nonzero finite values, zeros, infinities, and NaNs.</span></span> <span data-ttu-id="212c8-2015">En la tabla, `x` y `y` son valores finitos distintos de cero y @no__t 2 es el resultado de `x - y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2015">In the table, `x` and `y` are nonzero finite values, and `z` is the result of `x - y`.</span></span> <span data-ttu-id="212c8-2016">Si `x` y `y` son iguales, @no__t 2 es cero positivo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2016">If `x` and `y` are equal, `z` is positive zero.</span></span> <span data-ttu-id="212c8-2017">Si `x - y` es demasiado grande para representarlo en el tipo de destino, `z` es un infinito con el mismo signo que @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="212c8-2017">If `x - y` is too large to represent in the destination type, `z` is an infinity with the same sign as `x - y`.</span></span>

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | <span data-ttu-id="212c8-2018">y</span><span class="sxs-lookup"><span data-stu-id="212c8-2018">y</span></span>    | <span data-ttu-id="212c8-2019">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-2019">+0</span></span>   | <span data-ttu-id="212c8-2020">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-2020">-0</span></span>   | <span data-ttu-id="212c8-2021">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2021">+inf</span></span> | <span data-ttu-id="212c8-2022">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2022">-inf</span></span> | <span data-ttu-id="212c8-2023">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2023">NaN</span></span> | 
   | <span data-ttu-id="212c8-2024">x</span><span class="sxs-lookup"><span data-stu-id="212c8-2024">x</span></span>    | <span data-ttu-id="212c8-2025">z</span><span class="sxs-lookup"><span data-stu-id="212c8-2025">z</span></span>    | <span data-ttu-id="212c8-2026">x</span><span class="sxs-lookup"><span data-stu-id="212c8-2026">x</span></span>    | <span data-ttu-id="212c8-2027">x</span><span class="sxs-lookup"><span data-stu-id="212c8-2027">x</span></span>    | <span data-ttu-id="212c8-2028">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2028">-inf</span></span> | <span data-ttu-id="212c8-2029">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2029">+inf</span></span> | <span data-ttu-id="212c8-2030">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2030">NaN</span></span> | 
   | <span data-ttu-id="212c8-2031">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-2031">+0</span></span>   | <span data-ttu-id="212c8-2032">-y</span><span class="sxs-lookup"><span data-stu-id="212c8-2032">-y</span></span>   | <span data-ttu-id="212c8-2033">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-2033">+0</span></span>   | <span data-ttu-id="212c8-2034">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-2034">+0</span></span>   | <span data-ttu-id="212c8-2035">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2035">-inf</span></span> | <span data-ttu-id="212c8-2036">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2036">+inf</span></span> | <span data-ttu-id="212c8-2037">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2037">NaN</span></span> | 
   | <span data-ttu-id="212c8-2038">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-2038">-0</span></span>   | <span data-ttu-id="212c8-2039">-y</span><span class="sxs-lookup"><span data-stu-id="212c8-2039">-y</span></span>   | <span data-ttu-id="212c8-2040">-0</span><span class="sxs-lookup"><span data-stu-id="212c8-2040">-0</span></span>   | <span data-ttu-id="212c8-2041">+0</span><span class="sxs-lookup"><span data-stu-id="212c8-2041">+0</span></span>   | <span data-ttu-id="212c8-2042">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2042">-inf</span></span> | <span data-ttu-id="212c8-2043">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2043">+inf</span></span> | <span data-ttu-id="212c8-2044">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2044">NaN</span></span> | 
   | <span data-ttu-id="212c8-2045">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2045">+inf</span></span> | <span data-ttu-id="212c8-2046">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2046">+inf</span></span> | <span data-ttu-id="212c8-2047">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2047">+inf</span></span> | <span data-ttu-id="212c8-2048">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2048">+inf</span></span> | <span data-ttu-id="212c8-2049">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2049">NaN</span></span>  | <span data-ttu-id="212c8-2050">+inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2050">+inf</span></span> | <span data-ttu-id="212c8-2051">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2051">NaN</span></span> | 
   | <span data-ttu-id="212c8-2052">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2052">-inf</span></span> | <span data-ttu-id="212c8-2053">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2053">-inf</span></span> | <span data-ttu-id="212c8-2054">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2054">-inf</span></span> | <span data-ttu-id="212c8-2055">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2055">-inf</span></span> | <span data-ttu-id="212c8-2056">-inf</span><span class="sxs-lookup"><span data-stu-id="212c8-2056">-inf</span></span> | <span data-ttu-id="212c8-2057">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2057">NaN</span></span>  | <span data-ttu-id="212c8-2058">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2058">NaN</span></span> | 
   | <span data-ttu-id="212c8-2059">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2059">NaN</span></span>  | <span data-ttu-id="212c8-2060">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2060">NaN</span></span>  | <span data-ttu-id="212c8-2061">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2061">NaN</span></span>  | <span data-ttu-id="212c8-2062">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2062">NaN</span></span>  | <span data-ttu-id="212c8-2063">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2063">NaN</span></span>  | <span data-ttu-id="212c8-2064">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2064">NaN</span></span>  | <span data-ttu-id="212c8-2065">NaN</span><span class="sxs-lookup"><span data-stu-id="212c8-2065">NaN</span></span> | 

*  <span data-ttu-id="212c8-2066">Resta decimal:</span><span class="sxs-lookup"><span data-stu-id="212c8-2066">Decimal subtraction:</span></span>

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   <span data-ttu-id="212c8-2067">Si el valor resultante es demasiado grande para representarlo en el formato `decimal`, se produce una `System.OverflowException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2067">If the resulting value is too large to represent in the `decimal` format, a `System.OverflowException` is thrown.</span></span> <span data-ttu-id="212c8-2068">La escala del resultado, antes de cualquier redondeo, es el mayor de las escalas de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2068">The scale of the result, before any rounding, is the larger of the scales of the two operands.</span></span>

   <span data-ttu-id="212c8-2069">La resta decimal es equivalente a usar el operador de resta de tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2069">Decimal subtraction is equivalent to using the subtraction operator of type `System.Decimal`.</span></span>

*  <span data-ttu-id="212c8-2070">Resta de enumeración.</span><span class="sxs-lookup"><span data-stu-id="212c8-2070">Enumeration subtraction.</span></span> <span data-ttu-id="212c8-2071">Cada tipo de enumeración proporciona implícitamente el siguiente operador predefinido, donde `E` es el tipo de enumeración y `U` es el tipo subyacente de `E`:</span><span class="sxs-lookup"><span data-stu-id="212c8-2071">Every enumeration type implicitly provides the following predefined operator, where `E` is the enum type, and `U` is the underlying type of `E`:</span></span>

   ```csharp
   U operator -(E x, E y);
   ```

   <span data-ttu-id="212c8-2072">Este operador se evalúa exactamente como `(U)((U)x - (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2072">This operator is evaluated exactly as `(U)((U)x - (U)y)`.</span></span> <span data-ttu-id="212c8-2073">En otras palabras, el operador calcula la diferencia entre los valores ordinales de `x` y `y`, y el tipo del resultado es el tipo subyacente de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="212c8-2073">In other words, the operator computes the difference between the ordinal values of `x` and `y`, and the type of the result is the underlying type of the enumeration.</span></span>

   ```csharp
   E operator -(E x, U y);
   ```

   <span data-ttu-id="212c8-2074">Este operador se evalúa exactamente como `(E)((U)x - y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2074">This operator is evaluated exactly as `(E)((U)x - y)`.</span></span> <span data-ttu-id="212c8-2075">En otras palabras, el operador resta un valor del tipo subyacente de la enumeración, lo que produce un valor de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="212c8-2075">In other words, the operator subtracts a value from the underlying type of the enumeration, yielding a value of the enumeration.</span></span>

*  <span data-ttu-id="212c8-2076">Eliminación de delegados.</span><span class="sxs-lookup"><span data-stu-id="212c8-2076">Delegate removal.</span></span> <span data-ttu-id="212c8-2077">Cada tipo de delegado proporciona implícitamente el siguiente operador predefinido, donde `D` es el tipo de delegado:</span><span class="sxs-lookup"><span data-stu-id="212c8-2077">Every delegate type implicitly provides the following predefined operator, where `D` is the delegate type:</span></span>

   ```csharp
   D operator -(D x, D y);
   ```

   <span data-ttu-id="212c8-2078">El operador binario `-` realiza la eliminación del delegado cuando ambos operandos son de algún tipo de delegado `D`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2078">The binary `-` operator performs delegate removal when both operands are of some delegate type `D`.</span></span> <span data-ttu-id="212c8-2079">Si los operandos tienen distintos tipos de delegado, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-2079">If the operands have different delegate types, a binding-time error occurs.</span></span> <span data-ttu-id="212c8-2080">Si el primer operando es `null`, el resultado de la operación es `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2080">If the first operand is `null`, the result of the operation is `null`.</span></span> <span data-ttu-id="212c8-2081">De lo contrario, si el segundo operando es `null`, el resultado de la operación es el valor del primer operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-2081">Otherwise, if the second operand is `null`, then the result of the operation is the value of the first operand.</span></span> <span data-ttu-id="212c8-2082">De lo contrario, ambos operandos representan listas de invocación ([declaraciones de delegado](delegates.md#delegate-declarations)) que tienen una o más entradas y el resultado es una nueva lista de invocación que se compone de la lista del primer operando con las entradas del segundo operando que se han quitado, siempre y cuando el segundo la lista de operandos es una sublista contigua adecuada de la primera.</span><span class="sxs-lookup"><span data-stu-id="212c8-2082">Otherwise, both operands represent invocation lists ([Delegate declarations](delegates.md#delegate-declarations)) having one or more entries, and the result is a new invocation list consisting of the first operand's list with the second operand's entries removed from it, provided the second operand's list is a proper contiguous sublist of the first's.</span></span>     <span data-ttu-id="212c8-2083">(Para determinar la igualdad de la lista, las entradas correspondientes se comparan como para el operador de igualdad de delegado ([operadores de igualdad de delegado](expressions.md#delegate-equality-operators))). De lo contrario, el resultado es el valor del operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2083">(To determine sublist equality, corresponding entries are compared as for the delegate equality operator ([Delegate equality operators](expressions.md#delegate-equality-operators)).) Otherwise, the result is the value of the left operand.</span></span> <span data-ttu-id="212c8-2084">En el proceso no se cambia ninguna de las listas de operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2084">Neither of the operands' lists is changed in the process.</span></span> <span data-ttu-id="212c8-2085">Si la lista del segundo operando coincide con varias sublistas de entradas contiguas de la lista del primer operando, se quita la sublista coincidente situada más a la derecha de las entradas contiguas.</span><span class="sxs-lookup"><span data-stu-id="212c8-2085">If the second operand's list matches multiple sublists of contiguous entries in the first operand's list, the right-most matching sublist of contiguous entries is removed.</span></span> <span data-ttu-id="212c8-2086">Si la eliminación da como resultado una lista vacía, el resultado es `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2086">If removal results in an empty list, the result is `null`.</span></span> <span data-ttu-id="212c8-2087">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="212c8-2087">For example:</span></span>

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

## <a name="shift-operators"></a><span data-ttu-id="212c8-2088">Operadores de desplazamiento</span><span class="sxs-lookup"><span data-stu-id="212c8-2088">Shift operators</span></span>

<span data-ttu-id="212c8-2089">Los operadores `<<` y `>>` se utilizan para realizar operaciones de desplazamiento de bits.</span><span class="sxs-lookup"><span data-stu-id="212c8-2089">The `<<` and `>>` operators are used to perform bit shifting operations.</span></span>

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

<span data-ttu-id="212c8-2090">Si un operando de un *shift_expression* tiene el tipo en tiempo de compilación `dynamic`, la expresión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2090">If an operand of a *shift_expression* has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="212c8-2091">En este caso, el tipo en tiempo de compilación de la expresión es `dynamic`, y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución de los operandos que tienen el tipo en tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2091">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="212c8-2092">Para una operación con el formato `x << count` o `x >> count`, se aplica la resolución de sobrecarga del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-2092">For an operation of the form `x << count` or `x >> count`, binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-2093">Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-2093">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="212c8-2094">Al declarar un operador de desplazamiento sobrecargado, el tipo del primer operando siempre debe ser la clase o el struct que contiene la declaración del operador y el tipo del segundo operando siempre debe ser `int`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2094">When declaring an overloaded shift operator, the type of the first operand must always be the class or struct containing the operator declaration, and the type of the second operand must always be `int`.</span></span>

<span data-ttu-id="212c8-2095">A continuación se enumeran los operadores de desplazamiento predefinidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2095">The predefined shift operators are listed below.</span></span>

*  <span data-ttu-id="212c8-2096">Desplazar a la izquierda:</span><span class="sxs-lookup"><span data-stu-id="212c8-2096">Shift left:</span></span>

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   <span data-ttu-id="212c8-2097">El operador `<<` desplaza `x` a la izquierda por un número de bits calculado como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2097">The `<<` operator shifts `x` left by a number of bits computed as described below.</span></span>

   <span data-ttu-id="212c8-2098">Los bits de orden superior fuera del intervalo del tipo de resultado de `x` se descartan, los bits restantes se desplazan a la izquierda y las posiciones de bits vacías de orden inferior se establecen en cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-2098">The high-order bits outside the range of the result type of `x` are discarded, the remaining bits are shifted left, and the low-order empty bit positions are set to zero.</span></span>

*  <span data-ttu-id="212c8-2099">Desplazamiento a la derecha:</span><span class="sxs-lookup"><span data-stu-id="212c8-2099">Shift right:</span></span>

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   <span data-ttu-id="212c8-2100">El operador `>>` desplaza `x` a la derecha un número de bits calculado como se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2100">The `>>` operator shifts `x` right by a number of bits computed as described below.</span></span>

   <span data-ttu-id="212c8-2101">Cuando `x` es de tipo `int` o @no__t 2, se descartan los bits de orden inferior de `x`, los bits restantes se desplazan a la derecha y las posiciones de bits vacías de orden superior se establecen en cero si `x` no es negativo y se establece en uno si `x` es negativo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2101">When `x` is of type `int` or `long`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero if `x` is non-negative and set to one if `x` is negative.</span></span>

   <span data-ttu-id="212c8-2102">Cuando `x` es de tipo `uint` o @no__t 2, se descartan los bits de orden inferior de `x`, los bits restantes se desplazan a la derecha y las posiciones de bits vacías de orden superior se establecen en cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-2102">When `x` is of type `uint` or `ulong`, the low-order bits of `x` are discarded, the remaining bits are shifted right, and the high-order empty bit positions are set to zero.</span></span>

<span data-ttu-id="212c8-2103">En el caso de los operadores predefinidos, el número de bits que se va a desplazar se calcula de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="212c8-2103">For the predefined operators, the number of bits to shift is computed as follows:</span></span>

*  <span data-ttu-id="212c8-2104">Cuando el tipo de `x` es `int` o `uint`, el recuento de desplazamiento viene dado por los cinco bits de orden inferior de `count`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2104">When the type of `x` is `int` or `uint`, the shift count is given by the low-order five bits of `count`.</span></span> <span data-ttu-id="212c8-2105">En otras palabras, el número de turnos se calcula a partir de `count & 0x1F`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2105">In other words, the shift count is computed from `count & 0x1F`.</span></span>
*  <span data-ttu-id="212c8-2106">Cuando el tipo de `x` es `long` o `ulong`, el recuento de desplazamiento viene dado por los seis bits de orden inferior de `count`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2106">When the type of `x` is `long` or `ulong`, the shift count is given by the low-order six bits of `count`.</span></span> <span data-ttu-id="212c8-2107">En otras palabras, el número de turnos se calcula a partir de `count & 0x3F`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2107">In other words, the shift count is computed from `count & 0x3F`.</span></span>

<span data-ttu-id="212c8-2108">Si el número de turnos resultante es cero, los operadores de desplazamiento simplemente devuelven el valor de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2108">If the resulting shift count is zero, the shift operators simply return the value of `x`.</span></span>

<span data-ttu-id="212c8-2109">Las operaciones de desplazamiento nunca causan desbordamientos y producen los mismos resultados en los contextos `checked` y `unchecked`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2109">Shift operations never cause overflows and produce the same results in `checked` and `unchecked` contexts.</span></span>

<span data-ttu-id="212c8-2110">Cuando el operando izquierdo del operador `>>` es de un tipo entero con signo, el operador realiza un desplazamiento aritmético a la derecha, donde el valor del bit más significativo (el bit de signo) del operando se propaga a las posiciones de bits vacías de orden superior.</span><span class="sxs-lookup"><span data-stu-id="212c8-2110">When the left operand of the `>>` operator is of a signed integral type, the operator performs an arithmetic shift right wherein the value of the most significant bit (the sign bit) of the operand is propagated to the high-order empty bit positions.</span></span> <span data-ttu-id="212c8-2111">Cuando el operando izquierdo del operador `>>` es de un tipo entero sin signo, el operador realiza un desplazamiento lógico derecho en el que las posiciones de bits vacías de orden superior siempre se establecen en cero.</span><span class="sxs-lookup"><span data-stu-id="212c8-2111">When the left operand of the `>>` operator is of an unsigned integral type, the operator performs a logical shift right wherein high-order empty bit positions are always set to zero.</span></span> <span data-ttu-id="212c8-2112">Para realizar la operación opuesta a la que se infiere del tipo de operando, se pueden usar conversiones explícitas.</span><span class="sxs-lookup"><span data-stu-id="212c8-2112">To perform the opposite operation of that inferred from the operand type, explicit casts can be used.</span></span> <span data-ttu-id="212c8-2113">Por ejemplo, si `x` es una variable de tipo `int`, la operación `unchecked((int)((uint)x >> y))` realiza un desplazamiento lógico a la derecha de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2113">For example, if `x` is a variable of type `int`, the operation `unchecked((int)((uint)x >> y))` performs a logical shift right of `x`.</span></span>

## <a name="relational-and-type-testing-operators"></a><span data-ttu-id="212c8-2114">Operadores de comprobación de tipos y relacionales</span><span class="sxs-lookup"><span data-stu-id="212c8-2114">Relational and type-testing operators</span></span>

<span data-ttu-id="212c8-2115">Los operadores `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` y `as` se denominan operadores relacionales y de prueba de tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2115">The `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` and `as` operators are called the relational and type-testing operators.</span></span>

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

<span data-ttu-id="212c8-2116">El operador `is` se describe en [el operador is](expressions.md#the-is-operator) y el operador `as` se describe en [el operador as](expressions.md#the-as-operator).</span><span class="sxs-lookup"><span data-stu-id="212c8-2116">The `is` operator is described in [The is operator](expressions.md#the-is-operator) and the `as` operator is described in [The as operator](expressions.md#the-as-operator).</span></span>

<span data-ttu-id="212c8-2117">Los operadores `==`, `!=`, `<`, `>`, `<=` y `>=` son ***operadores de comparación***.</span><span class="sxs-lookup"><span data-stu-id="212c8-2117">The `==`, `!=`, `<`, `>`, `<=` and `>=` operators are ***comparison operators***.</span></span>

<span data-ttu-id="212c8-2118">Si un operando de un operador de comparación tiene el tipo en tiempo de compilación `dynamic`, la expresión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2118">If an operand of a comparison operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="212c8-2119">En este caso, el tipo en tiempo de compilación de la expresión es `dynamic`, y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución de los operandos que tienen el tipo en tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2119">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="212c8-2120">En el caso de una operación con el formato `x` *op* `y`, donde *OP* es un operador de comparación, se aplica la resolución de sobrecarga ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-2120">For an operation of the form `x` *op* `y`, where *op* is a comparison operator, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-2121">Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-2121">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="212c8-2122">Los operadores de comparación predefinidos se describen en las secciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="212c8-2122">The predefined comparison operators are described in the following sections.</span></span> <span data-ttu-id="212c8-2123">Todos los operadores de comparación predefinidos devuelven un resultado de tipo `bool`, tal y como se describe en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2123">All predefined comparison operators return a result of type `bool`, as described in the following table.</span></span>


| <span data-ttu-id="212c8-2124">__Sesión__</span><span class="sxs-lookup"><span data-stu-id="212c8-2124">__Operation__</span></span> | <span data-ttu-id="212c8-2125">__Resultado__</span><span class="sxs-lookup"><span data-stu-id="212c8-2125">__Result__</span></span>                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | <span data-ttu-id="212c8-2126">`true` si `x` es igual a `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="212c8-2126">`true` if `x` is equal to `y`, `false` otherwise</span></span>                 | 
| `x != y`      | <span data-ttu-id="212c8-2127">`true` si `x` no es igual a `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="212c8-2127">`true` if `x` is not equal to `y`, `false` otherwise</span></span>             | 
| `x < y`       | <span data-ttu-id="212c8-2128">`true` si `x` es menor que `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="212c8-2128">`true` if `x` is less than `y`, `false` otherwise</span></span>                | 
| `x > y`       | <span data-ttu-id="212c8-2129">`true` si `x` es mayor que `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="212c8-2129">`true` if `x` is greater than `y`, `false` otherwise</span></span>             | 
| `x <= y`      | <span data-ttu-id="212c8-2130">`true` si `x` es menor o igual que `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="212c8-2130">`true` if `x` is less than or equal to `y`, `false` otherwise</span></span>    | 
| `x >= y`      | <span data-ttu-id="212c8-2131">`true` si `x` es mayor o igual que `y`, `false` en caso contrario</span><span class="sxs-lookup"><span data-stu-id="212c8-2131">`true` if `x` is greater than or equal to `y`, `false` otherwise</span></span> | 

### <a name="integer-comparison-operators"></a><span data-ttu-id="212c8-2132">Operadores de comparación de enteros</span><span class="sxs-lookup"><span data-stu-id="212c8-2132">Integer comparison operators</span></span>

<span data-ttu-id="212c8-2133">Los operadores de comparación de enteros predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="212c8-2133">The predefined integer comparison operators are:</span></span>
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

<span data-ttu-id="212c8-2134">Cada uno de estos operadores compara los valores numéricos de los dos operandos enteros y devuelve un valor `bool` que indica si la relación determinada es `true` o `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2134">Each of these operators compares the numeric values of the two integer operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span>

### <a name="floating-point-comparison-operators"></a><span data-ttu-id="212c8-2135">Operadores de comparación de punto flotante</span><span class="sxs-lookup"><span data-stu-id="212c8-2135">Floating-point comparison operators</span></span>

<span data-ttu-id="212c8-2136">Los operadores de comparación de punto flotante predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="212c8-2136">The predefined floating-point comparison operators are:</span></span>
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

<span data-ttu-id="212c8-2137">Los operadores comparan los operandos según las reglas del estándar IEEE 754:</span><span class="sxs-lookup"><span data-stu-id="212c8-2137">The operators compare the operands according to the rules of the IEEE 754 standard:</span></span>

*  <span data-ttu-id="212c8-2138">Si alguno de los operandos es NaN, el resultado es `false` para todos los operadores excepto `!=`, para los que el resultado es `true`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2138">If either operand is NaN, the result is `false` for all operators except `!=`, for which the result is `true`.</span></span> <span data-ttu-id="212c8-2139">Para dos operandos cualesquiera, `x != y` siempre produce el mismo resultado que `!(x == y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2139">For any two operands, `x != y` always produces the same result as `!(x == y)`.</span></span> <span data-ttu-id="212c8-2140">Sin embargo, cuando uno o ambos operandos son NaN, los operadores `<`, `>`, `<=` y `>=` no producen los mismos resultados que la negación lógica del operador opuesto.</span><span class="sxs-lookup"><span data-stu-id="212c8-2140">However, when one or both operands are NaN, the `<`, `>`, `<=`, and `>=` operators do not produce the same results as the logical negation of the opposite operator.</span></span> <span data-ttu-id="212c8-2141">Por ejemplo, si cualquiera de `x` y `y` es NaN, @no__t 2 es `false`, pero `!(x >= y)` es `true`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2141">For example, if either of `x` and `y` is NaN, then `x < y` is `false`, but `!(x >= y)` is `true`.</span></span>
*  <span data-ttu-id="212c8-2142">Cuando ninguno de los operandos es NaN, los operadores comparan los valores de los dos operandos de punto flotante con respecto a la ordenación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2142">When neither operand is NaN, the operators compare the values of the two floating-point operands with respect to the ordering</span></span>

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   <span data-ttu-id="212c8-2143">donde `min` y `max` son los valores finitos positivos más pequeños y mayores que se pueden representar en el formato de punto flotante dado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2143">where `min` and `max` are the smallest and largest positive finite values that can be represented in the given floating-point format.</span></span> <span data-ttu-id="212c8-2144">Los efectos importantes de este orden son:</span><span class="sxs-lookup"><span data-stu-id="212c8-2144">Notable effects of this ordering are:</span></span>
   * <span data-ttu-id="212c8-2145">Los ceros negativos y positivos se consideran iguales.</span><span class="sxs-lookup"><span data-stu-id="212c8-2145">Negative and positive zeros are considered equal.</span></span>
   * <span data-ttu-id="212c8-2146">Un infinito negativo se considera menor que todos los demás valores, pero es igual a otro infinito negativo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2146">A negative infinity is considered less than all other values, but equal to another negative infinity.</span></span>
   * <span data-ttu-id="212c8-2147">Un infinito positivo se considera mayor que el resto de los valores, pero es igual a otro infinito positivo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2147">A positive infinity is considered greater than all other values, but equal to another positive infinity.</span></span>

### <a name="decimal-comparison-operators"></a><span data-ttu-id="212c8-2148">Operadores de comparación decimal</span><span class="sxs-lookup"><span data-stu-id="212c8-2148">Decimal comparison operators</span></span>

<span data-ttu-id="212c8-2149">Los operadores de comparación decimal predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="212c8-2149">The predefined decimal comparison operators are:</span></span>
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

<span data-ttu-id="212c8-2150">Cada uno de estos operadores compara los valores numéricos de los dos operandos decimales y devuelve un valor `bool` que indica si la relación determinada es `true` o `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2150">Each of these operators compares the numeric values of the two decimal operands and returns a `bool` value that indicates whether the particular relation is `true` or `false`.</span></span> <span data-ttu-id="212c8-2151">Cada comparación decimal es equivalente a usar el operador relacional o de igualdad correspondiente del tipo `System.Decimal`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2151">Each decimal comparison is equivalent to using the corresponding relational or equality operator of type `System.Decimal`.</span></span>

### <a name="boolean-equality-operators"></a><span data-ttu-id="212c8-2152">Operadores de igualdad booleanos</span><span class="sxs-lookup"><span data-stu-id="212c8-2152">Boolean equality operators</span></span>

<span data-ttu-id="212c8-2153">Los operadores de igualdad booleano predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="212c8-2153">The predefined boolean equality operators are:</span></span>
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

<span data-ttu-id="212c8-2154">El resultado de `==` es `true` si tanto `x` como `y` son `true` o si `x` y `y` son `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2154">The result of `==` is `true` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="212c8-2155">De lo contrario, el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2155">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="212c8-2156">El resultado de `!=` es `false` si tanto `x` como `y` son `true` o si `x` y `y` son `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2156">The result of `!=` is `false` if both `x` and `y` are `true` or if both `x` and `y` are `false`.</span></span> <span data-ttu-id="212c8-2157">De lo contrario, el resultado es `true`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2157">Otherwise, the result is `true`.</span></span> <span data-ttu-id="212c8-2158">Cuando los operandos son del tipo `bool`, el operador `!=` produce el mismo resultado que el operador `^`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2158">When the operands are of type `bool`, the `!=` operator produces the same result as the `^` operator.</span></span>

### <a name="enumeration-comparison-operators"></a><span data-ttu-id="212c8-2159">Operadores de comparación de enumeración</span><span class="sxs-lookup"><span data-stu-id="212c8-2159">Enumeration comparison operators</span></span>

<span data-ttu-id="212c8-2160">Cada tipo de enumeración proporciona implícitamente los siguientes operadores de comparación predefinidos:</span><span class="sxs-lookup"><span data-stu-id="212c8-2160">Every enumeration type implicitly provides the following predefined comparison operators:</span></span>
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

<span data-ttu-id="212c8-2161">El resultado de evaluar `x op y`, donde `x` y `y` son expresiones de un tipo de enumeración `E` con un tipo subyacente `U` y `op` es uno de los operadores de comparación, es exactamente igual que evaluar `((U)x) op ((U)y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2161">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the comparison operators, is exactly the same as evaluating `((U)x) op ((U)y)`.</span></span> <span data-ttu-id="212c8-2162">En otras palabras, los operadores de comparación de tipos de enumeración simplemente comparan los valores enteros subyacentes de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2162">In other words, the enumeration type comparison operators simply compare the underlying integral values of the two operands.</span></span>

### <a name="reference-type-equality-operators"></a><span data-ttu-id="212c8-2163">Operadores de igualdad de tipos de referencia</span><span class="sxs-lookup"><span data-stu-id="212c8-2163">Reference type equality operators</span></span>

<span data-ttu-id="212c8-2164">Los operadores de igualdad de tipos de referencia predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="212c8-2164">The predefined reference type equality operators are:</span></span>
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

<span data-ttu-id="212c8-2165">Los operadores devuelven el resultado de comparar las dos referencias de igualdad o de no igualdad.</span><span class="sxs-lookup"><span data-stu-id="212c8-2165">The operators return the result of comparing the two references for equality or non-equality.</span></span>

<span data-ttu-id="212c8-2166">Puesto que los operadores de igualdad de tipos de referencia predefinidos aceptan operandos de tipo `object`, se aplican a todos los tipos que no declaran los miembros `operator ==` y `operator !=` aplicables.</span><span class="sxs-lookup"><span data-stu-id="212c8-2166">Since the predefined reference type equality operators accept operands of type `object`, they apply to all types that do not declare applicable `operator ==` and `operator !=` members.</span></span> <span data-ttu-id="212c8-2167">Por el contrario, todos los operadores de igualdad definidos por el usuario aplicables ocultan de forma eficaz los operadores de igualdad de tipos de referencia predefinidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2167">Conversely, any applicable user-defined equality operators effectively hide the predefined reference type equality operators.</span></span>

<span data-ttu-id="212c8-2168">Los operadores de igualdad de tipos de referencia predefinidos requieren uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="212c8-2168">The predefined reference type equality operators require one of the following:</span></span>

*  <span data-ttu-id="212c8-2169">Ambos operandos son un valor de un tipo conocido como *reference_type* o literal `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2169">Both operands are a value of a type known to be a *reference_type* or the literal `null`.</span></span> <span data-ttu-id="212c8-2170">Además, existe una conversión de referencia explícita ([conversiones de referencia explícitas](conversions.md#explicit-reference-conversions)) desde el tipo de uno de los operandos al tipo del otro operando.</span><span class="sxs-lookup"><span data-stu-id="212c8-2170">Furthermore, an explicit reference conversion ([Explicit reference conversions](conversions.md#explicit-reference-conversions)) exists from the type of either operand to the type of the other operand.</span></span>
*  <span data-ttu-id="212c8-2171">Un operando es un valor de tipo `T` donde `T` es un *type_parameter* y el otro operando es el literal `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2171">One operand is a value of type `T` where `T` is a *type_parameter* and the other operand is the literal `null`.</span></span> <span data-ttu-id="212c8-2172">Además `T` no tiene la restricción de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-2172">Furthermore `T` does not have the value type constraint.</span></span>

<span data-ttu-id="212c8-2173">A menos que se cumpla una de estas condiciones, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-2173">Unless one of these conditions are true, a binding-time error occurs.</span></span> <span data-ttu-id="212c8-2174">Las implicaciones destacadas de estas reglas son:</span><span class="sxs-lookup"><span data-stu-id="212c8-2174">Notable implications of these rules are:</span></span>

*  <span data-ttu-id="212c8-2175">Es un error en tiempo de enlace usar los operadores de igualdad de tipos de referencia predefinidos para comparar dos referencias que se sabe que son diferentes en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-2175">It is a binding-time error to use the predefined reference type equality operators to compare two references that are known to be different at binding-time.</span></span> <span data-ttu-id="212c8-2176">Por ejemplo, si los tipos en tiempo de enlace de los operandos son dos tipos de clase `A` y `B`, y si ninguno de los dos operandos `A` ni `B` se derivan del otro, sería imposible que los dos operandos hagan referencia al mismo objeto.</span><span class="sxs-lookup"><span data-stu-id="212c8-2176">For example, if the binding-time types of the operands are two class types `A` and `B`, and if neither `A` nor `B` derives from the other, then it would be impossible for the two operands to reference the same object.</span></span> <span data-ttu-id="212c8-2177">Por lo tanto, la operación se considera un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-2177">Thus, the operation is considered a binding-time error.</span></span>
*  <span data-ttu-id="212c8-2178">Los operadores de igualdad de tipos de referencia predefinidos no permiten comparar los operandos de tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-2178">The predefined reference type equality operators do not permit value type operands to be compared.</span></span> <span data-ttu-id="212c8-2179">Por lo tanto, a menos que un tipo de estructura declare sus propios operadores de igualdad, no es posible comparar valores de ese tipo de estructura.</span><span class="sxs-lookup"><span data-stu-id="212c8-2179">Therefore, unless a struct type declares its own equality operators, it is not possible to compare values of that struct type.</span></span>
*  <span data-ttu-id="212c8-2180">Los operadores de igualdad de tipos de referencia predefinidos nunca provocan que se produzcan operaciones de conversión boxing para sus operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2180">The predefined reference type equality operators never cause boxing operations to occur for their operands.</span></span> <span data-ttu-id="212c8-2181">No tendría sentido realizar estas operaciones de conversión boxing, ya que las referencias a las instancias de conversión boxing recién asignadas serían necesariamente distintas de las demás referencias.</span><span class="sxs-lookup"><span data-stu-id="212c8-2181">It would be meaningless to perform such boxing operations, since references to the newly allocated boxed instances would necessarily differ from all other references.</span></span>
*  <span data-ttu-id="212c8-2182">Si un operando de un tipo de parámetro de tipo `T` se compara con `null` y el tipo en tiempo de ejecución de `T` es un tipo de valor, el resultado de la comparación es `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2182">If an operand of a type parameter type `T` is compared to `null`, and the run-time type of `T` is a value type, the result of the comparison is `false`.</span></span>

<span data-ttu-id="212c8-2183">En el ejemplo siguiente se comprueba si un argumento de un tipo de parámetro de tipo sin restricciones es `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2183">The following example checks whether an argument of an unconstrained type parameter type is `null`.</span></span>
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

<span data-ttu-id="212c8-2184">La construcción `x == null` se permite aunque `T` pueda representar un tipo de valor y el resultado se define simplemente como @no__t 2 cuando `T` es un tipo de valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-2184">The `x == null` construct is permitted even though `T` could represent a value type, and the result is simply defined to be `false` when `T` is a value type.</span></span>

<span data-ttu-id="212c8-2185">En el caso de una operación con el formato `x == y` o `x != y`, si se cumple alguna `operator ==` o `operator !=` aplicable, las reglas de resolución de sobrecarga del operador ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) seleccionarán ese operador en lugar de la igualdad de tipos de referencia predefinida. Operator.</span><span class="sxs-lookup"><span data-stu-id="212c8-2185">For an operation of the form `x == y` or `x != y`, if any applicable `operator ==` or `operator !=` exists, the operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) rules will select that operator instead of the predefined reference type equality operator.</span></span> <span data-ttu-id="212c8-2186">Sin embargo, siempre es posible seleccionar el operador de igualdad de tipos de referencia predefinidos convirtiendo explícitamente uno o ambos operandos al tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2186">However, it is always possible to select the predefined reference type equality operator by explicitly casting one or both of the operands to type `object`.</span></span> <span data-ttu-id="212c8-2187">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2187">The example</span></span>
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
<span data-ttu-id="212c8-2188">genera el resultado</span><span class="sxs-lookup"><span data-stu-id="212c8-2188">produces the output</span></span>
```console
True
False
False
False
```

<span data-ttu-id="212c8-2189">Las variables `s` y `t` hacen referencia a dos instancias distintas de @no__t 2 que contienen los mismos caracteres.</span><span class="sxs-lookup"><span data-stu-id="212c8-2189">The `s` and `t` variables refer to two distinct `string` instances containing the same characters.</span></span> <span data-ttu-id="212c8-2190">La primera comparación genera `True` porque el operador de igualdad de cadena predefinido ([operadores de igualdad de cadena](expressions.md#string-equality-operators)) está seleccionado cuando ambos operandos son del tipo `string`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2190">The first comparison outputs `True` because the predefined string equality operator ([String equality operators](expressions.md#string-equality-operators)) is selected when both operands are of type `string`.</span></span> <span data-ttu-id="212c8-2191">En el resto se comparan todos los resultados `False` porque se selecciona el operador de igualdad de tipos de referencia predefinido cuando uno o ambos operandos son del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2191">The remaining comparisons all output `False` because the predefined reference type equality operator is selected when one or both of the operands are of type `object`.</span></span>

<span data-ttu-id="212c8-2192">Tenga en cuenta que la técnica anterior no es significativa para los tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-2192">Note that the above technique is not meaningful for value types.</span></span> <span data-ttu-id="212c8-2193">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2193">The example</span></span>
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
<span data-ttu-id="212c8-2194">genera `False` porque las conversiones crean referencias a dos instancias independientes de valores de `int` con conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="212c8-2194">outputs `False` because the casts create references to two separate instances of boxed `int` values.</span></span>

### <a name="string-equality-operators"></a><span data-ttu-id="212c8-2195">Operadores de igualdad de cadenas</span><span class="sxs-lookup"><span data-stu-id="212c8-2195">String equality operators</span></span>

<span data-ttu-id="212c8-2196">Los operadores de igualdad de cadena predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="212c8-2196">The predefined string equality operators are:</span></span>
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

<span data-ttu-id="212c8-2197">Dos valores `string` se consideran iguales cuando se cumple una de las siguientes condiciones:</span><span class="sxs-lookup"><span data-stu-id="212c8-2197">Two `string` values are considered equal when one of the following is true:</span></span>

*  <span data-ttu-id="212c8-2198">Ambos valores son `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2198">Both values are `null`.</span></span>
*  <span data-ttu-id="212c8-2199">Ambos valores son referencias no nulas a instancias de cadena que tienen longitudes idénticas y caracteres idénticos en cada posición de carácter.</span><span class="sxs-lookup"><span data-stu-id="212c8-2199">Both values are non-null references to string instances that have identical lengths and identical characters in each character position.</span></span>

<span data-ttu-id="212c8-2200">Los operadores de igualdad de cadena comparan valores de cadena en lugar de referencias de cadena.</span><span class="sxs-lookup"><span data-stu-id="212c8-2200">The string equality operators compare string values rather than string references.</span></span> <span data-ttu-id="212c8-2201">Cuando dos instancias de cadena independientes contienen exactamente la misma secuencia de caracteres, los valores de las cadenas son iguales, pero las referencias son diferentes.</span><span class="sxs-lookup"><span data-stu-id="212c8-2201">When two separate string instances contain the exact same sequence of characters, the values of the strings are equal, but the references are different.</span></span> <span data-ttu-id="212c8-2202">Como se describe en [operadores de igualdad de tipos de referencia](expressions.md#reference-type-equality-operators), los operadores de igualdad de tipos de referencia se pueden usar para comparar referencias de cadena en lugar de valores de cadena.</span><span class="sxs-lookup"><span data-stu-id="212c8-2202">As described in [Reference type equality operators](expressions.md#reference-type-equality-operators), the reference type equality operators can be used to compare string references instead of string values.</span></span>

### <a name="delegate-equality-operators"></a><span data-ttu-id="212c8-2203">Operadores de igualdad de delegado</span><span class="sxs-lookup"><span data-stu-id="212c8-2203">Delegate equality operators</span></span>

<span data-ttu-id="212c8-2204">Cada tipo de delegado proporciona implícitamente los siguientes operadores de comparación predefinidos:</span><span class="sxs-lookup"><span data-stu-id="212c8-2204">Every delegate type implicitly provides the following predefined comparison operators:</span></span>

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

<span data-ttu-id="212c8-2205">Dos instancias de delegado se consideran iguales como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-2205">Two delegate instances are considered equal as follows:</span></span>

*  <span data-ttu-id="212c8-2206">Si alguna de las instancias de delegado es `null`, son iguales si y solo si ambas son `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2206">If either of the delegate instances is `null`, they are equal if and only if both are `null`.</span></span>
*  <span data-ttu-id="212c8-2207">Si los delegados tienen un tipo diferente en tiempo de ejecución, nunca son iguales.</span><span class="sxs-lookup"><span data-stu-id="212c8-2207">If the delegates have different run-time type they are never equal.</span></span>
*  <span data-ttu-id="212c8-2208">Si ambas instancias de delegado tienen una lista de invocaciones ([declaraciones de delegado](delegates.md#delegate-declarations)), esas instancias son iguales si y solo si sus listas de invocación tienen la misma longitud, y cada entrada de una lista de invocación de un es igual (tal como se define a continuación) al correspondiente entrada, en orden, en la lista de invocación de otro.</span><span class="sxs-lookup"><span data-stu-id="212c8-2208">If both of the delegate instances have an invocation list ([Delegate declarations](delegates.md#delegate-declarations)), those instances are equal if and only if their invocation lists are the same length, and each entry in one's invocation list is equal (as defined below) to the corresponding entry, in order, in the other's invocation list.</span></span>

<span data-ttu-id="212c8-2209">Las siguientes reglas rigen la igualdad de las entradas de la lista de invocación:</span><span class="sxs-lookup"><span data-stu-id="212c8-2209">The following rules govern the equality of invocation list entries:</span></span>

*  <span data-ttu-id="212c8-2210">Si dos entradas de la lista de invocación hacen referencia al mismo método estático, las entradas son iguales.</span><span class="sxs-lookup"><span data-stu-id="212c8-2210">If two invocation list entries both refer to the same static method then the entries are equal.</span></span>
*  <span data-ttu-id="212c8-2211">Si dos entradas de la lista de invocación hacen referencia al mismo método no estático en el mismo objeto de destino (tal y como se define en los operadores de igualdad de referencia), las entradas son iguales.</span><span class="sxs-lookup"><span data-stu-id="212c8-2211">If two invocation list entries both refer to the same non-static method on the same target object (as defined by the reference equality operators) then the entries are equal.</span></span>
*  <span data-ttu-id="212c8-2212">Las entradas de la lista de invocación generadas a partir de la evaluación de *anonymous_method_expression*s semánticamente idénticas o *lambda_expression*s con el mismo conjunto (posiblemente vacío) de instancias de variables externas capturadas se permiten (pero no se requieren) sea.</span><span class="sxs-lookup"><span data-stu-id="212c8-2212">Invocation list entries produced from evaluation of semantically identical *anonymous_method_expression*s or *lambda_expression*s with the same (possibly empty) set of captured outer variable instances are permitted (but not required) to be equal.</span></span>

### <a name="equality-operators-and-null"></a><span data-ttu-id="212c8-2213">Operadores de igualdad y null</span><span class="sxs-lookup"><span data-stu-id="212c8-2213">Equality operators and null</span></span>

<span data-ttu-id="212c8-2214">Los operadores `==` y `!=` permiten que un operando sea un valor de un tipo que acepta valores NULL y el otro para ser el literal @no__t 2, aunque no exista ningún operador predefinido o definido por el usuario (en formato sin levantar o elevado) para la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2214">The `==` and `!=` operators permit one operand to be a value of a nullable type and the other to be the `null` literal, even if no predefined or user-defined operator (in unlifted or lifted form) exists for the operation.</span></span>

<span data-ttu-id="212c8-2215">Para una operación de uno de los formularios</span><span class="sxs-lookup"><span data-stu-id="212c8-2215">For an operation of one of the forms</span></span>
```csharp
x == null
null == x
x != null
null != x
```
<span data-ttu-id="212c8-2216">donde `x` es una expresión de un tipo que acepta valores NULL, si la resolución de sobrecarga del operador ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) no encuentra un operador aplicable, el resultado se calcula a partir de la propiedad `HasValue` de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2216">where `x` is an expression of a nullable type, if operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) fails to find an applicable operator, the result is instead computed from the `HasValue` property of `x`.</span></span> <span data-ttu-id="212c8-2217">En concreto, las dos primeras formas se traducen en `!x.HasValue` y las dos últimas se traducen en `x.HasValue`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2217">Specifically, the first two forms are translated into `!x.HasValue`, and last two forms are translated into `x.HasValue`.</span></span>

### <a name="the-is-operator"></a><span data-ttu-id="212c8-2218">El operador is</span><span class="sxs-lookup"><span data-stu-id="212c8-2218">The is operator</span></span>

<span data-ttu-id="212c8-2219">El operador `is` se usa para comprobar dinámicamente si el tipo en tiempo de ejecución de un objeto es compatible con un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2219">The `is` operator is used to dynamically check if the run-time type of an object is compatible with a given type.</span></span> <span data-ttu-id="212c8-2220">El resultado de la operación `E is T`, donde `E` es una expresión y @no__t 2 es un tipo, es un valor booleano que indica si `E` se puede convertir correctamente al tipo `T` por una conversión de referencia, una conversión boxing o una conversión unboxing.</span><span class="sxs-lookup"><span data-stu-id="212c8-2220">The result of the operation `E is T`, where `E` is an expression and `T` is a type, is a boolean value indicating whether `E` can successfully be converted to type `T` by a reference conversion, a boxing conversion, or an unboxing conversion.</span></span> <span data-ttu-id="212c8-2221">La operación se evalúa como sigue, después de que se hayan sustituido los argumentos de tipo para todos los parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="212c8-2221">The operation is evaluated as follows, after type arguments have been substituted for all type parameters:</span></span>

*  <span data-ttu-id="212c8-2222">Si `E` es una función anónima, se produce un error en tiempo de compilación</span><span class="sxs-lookup"><span data-stu-id="212c8-2222">If `E` is an anonymous function, a compile-time error occurs</span></span>
*  <span data-ttu-id="212c8-2223">Si `E` es un grupo de métodos o el literal `null`, si el tipo de `E` es un tipo de referencia o un tipo que acepta valores NULL y el valor de `E` es null, el resultado es false.</span><span class="sxs-lookup"><span data-stu-id="212c8-2223">If `E` is a method group or the `null` literal, of if the type of `E` is a reference type or a nullable type and the value of `E` is null, the result is false.</span></span>
*  <span data-ttu-id="212c8-2224">De lo contrario, deje que `D` represente el tipo dinámico de `E` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-2224">Otherwise, let `D` represent the dynamic type of `E` as follows:</span></span>
   * <span data-ttu-id="212c8-2225">Si el tipo de `E` es un tipo de referencia, `D` es el tipo en tiempo de ejecución de la referencia de instancia por `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2225">If the type of `E` is a reference type, `D` is the run-time type of the instance reference by `E`.</span></span>
   * <span data-ttu-id="212c8-2226">Si el tipo de `E` es un tipo que acepta valores NULL, `D` es el tipo subyacente de ese tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-2226">If the type of `E` is a nullable type, `D` is the underlying type of that nullable type.</span></span>
   * <span data-ttu-id="212c8-2227">Si el tipo de `E` es un tipo de valor que no acepta valores NULL, `D` es el tipo de `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2227">If the type of `E` is a non-nullable value type, `D` is the type of `E`.</span></span>
*  <span data-ttu-id="212c8-2228">El resultado de la operación depende de `D` y `T` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-2228">The result of the operation depends on `D` and `T` as follows:</span></span>
   * <span data-ttu-id="212c8-2229">Si `T` es un tipo de referencia, el resultado es true si `D` y `T` son del mismo tipo, si `D` es un tipo de referencia y existe una conversión de referencia implícita de `D` a `T`, o bien si `D` es un tipo de valor y una conversión boxing de `D`. para `T` existe.</span><span class="sxs-lookup"><span data-stu-id="212c8-2229">If `T` is a reference type, the result is true if `D` and `T` are the same type, if `D` is a reference type and an implicit reference conversion from `D` to `T` exists, or if `D` is a value type and a boxing conversion from `D` to `T` exists.</span></span>
   * <span data-ttu-id="212c8-2230">Si `T` es un tipo que acepta valores NULL, el resultado es true si `D` es el tipo subyacente de `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2230">If `T` is a nullable type, the result is true if `D` is the underlying type of `T`.</span></span>
   * <span data-ttu-id="212c8-2231">Si `T` es un tipo de valor que no acepta valores NULL, el resultado es true si `D` y `T` son del mismo tipo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2231">If `T` is a non-nullable value type, the result is true if `D` and `T` are the same type.</span></span>
   * <span data-ttu-id="212c8-2232">De lo contrario, el resultado es false.</span><span class="sxs-lookup"><span data-stu-id="212c8-2232">Otherwise, the result is false.</span></span>

<span data-ttu-id="212c8-2233">Tenga en cuenta que las conversiones definidas por el usuario no se tienen en cuenta por el operador `is`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2233">Note that user defined conversions, are not considered by the `is` operator.</span></span>

### <a name="the-as-operator"></a><span data-ttu-id="212c8-2234">El operador as</span><span class="sxs-lookup"><span data-stu-id="212c8-2234">The as operator</span></span>

<span data-ttu-id="212c8-2235">El operador `as` se usa para convertir explícitamente un valor en un tipo de referencia determinado o un tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-2235">The `as` operator is used to explicitly convert a value to a given reference type or nullable type.</span></span> <span data-ttu-id="212c8-2236">A diferencia de una expresión de conversión ([expresiones de conversión](expressions.md#cast-expressions)), el operador `as` nunca produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="212c8-2236">Unlike a cast expression ([Cast expressions](expressions.md#cast-expressions)), the `as` operator never throws an exception.</span></span> <span data-ttu-id="212c8-2237">En su lugar, si la conversión indicada no es posible, el valor resultante es `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2237">Instead, if the indicated conversion is not possible, the resulting value is `null`.</span></span>

<span data-ttu-id="212c8-2238">En una operación con el formato `E as T`, `E` debe ser una expresión y `T` debe ser un tipo de referencia, un parámetro de tipo conocido como un tipo de referencia o un tipo que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-2238">In an operation of the form `E as T`, `E` must be an expression and `T` must be a reference type, a type parameter known to be a reference type, or a nullable type.</span></span> <span data-ttu-id="212c8-2239">Además, al menos uno de los siguientes debe ser true o, de lo contrario, se producirá un error en tiempo de compilación:</span><span class="sxs-lookup"><span data-stu-id="212c8-2239">Furthermore, at least one of the following must be true, or otherwise a compile-time error occurs:</span></span>

*  <span data-ttu-id="212c8-2240">Una identidad ([conversión de identidad](conversions.md#identity-conversion)), implícita que acepta valores NULL ([conversiones implícitas que aceptan valores NULL](conversions.md#implicit-nullable-conversions)), referencia implícita (conversiones de[referencias implícitas](conversions.md#implicit-reference-conversions)), conversión boxing ([conversiones boxing](conversions.md#boxing-conversions)), explícitamente acepta valores NULL ([admite valores NULL explícitos). conversiones](conversions.md#explicit-nullable-conversions)), referencia explícita (conversiones de[referencia explícita](conversions.md#explicit-reference-conversions)) o conversión unboxing ([conversiones unboxing](conversions.md#unboxing-conversions)) de `E` a `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2240">An identity ([Identity conversion](conversions.md#identity-conversion)), implicit nullable ([Implicit nullable conversions](conversions.md#implicit-nullable-conversions)), implicit reference ([Implicit reference conversions](conversions.md#implicit-reference-conversions)), boxing ([Boxing conversions](conversions.md#boxing-conversions)), explicit nullable ([Explicit nullable conversions](conversions.md#explicit-nullable-conversions)), explicit reference ([Explicit reference conversions](conversions.md#explicit-reference-conversions)), or unboxing ([Unboxing conversions](conversions.md#unboxing-conversions)) conversion exists from `E` to `T`.</span></span>
*  <span data-ttu-id="212c8-2241">El tipo de `E` o `T` es un tipo abierto.</span><span class="sxs-lookup"><span data-stu-id="212c8-2241">The type of `E` or `T` is an open type.</span></span>
*  <span data-ttu-id="212c8-2242">`E` es el literal `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2242">`E` is the `null` literal.</span></span>

<span data-ttu-id="212c8-2243">Si el tipo en tiempo de compilación de `E` no es `dynamic`, la operación `E as T` genera el mismo resultado que</span><span class="sxs-lookup"><span data-stu-id="212c8-2243">If the compile-time type of `E` is not `dynamic`, the operation `E as T` produces the same result as</span></span>
```csharp
E is T ? (T)(E) : (T)null
```
<span data-ttu-id="212c8-2244">salvo que `E` solo se evalúa una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-2244">except that `E` is only evaluated once.</span></span> <span data-ttu-id="212c8-2245">Se puede esperar que el compilador optimice `E as T` para realizar como máximo una comprobación de tipos dinámicos en lugar de las dos comprobaciones de tipos dinámicos implícitas por la expansión anterior.</span><span class="sxs-lookup"><span data-stu-id="212c8-2245">The compiler can be expected to optimize `E as T` to perform at most one dynamic type check as opposed to the two dynamic type checks implied by the expansion above.</span></span>

<span data-ttu-id="212c8-2246">Si el tipo en tiempo de compilación de `E` es `dynamic`, a diferencia del operador de conversión, el operador `as` no está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2246">If the compile-time type of `E` is `dynamic`, unlike the cast operator the `as` operator is not dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="212c8-2247">Por lo tanto, la expansión en este caso es:</span><span class="sxs-lookup"><span data-stu-id="212c8-2247">Therefore the expansion in this case is:</span></span>
```csharp
E is T ? (T)(object)(E) : (T)null
```

<span data-ttu-id="212c8-2248">Tenga en cuenta que algunas conversiones, como las conversiones definidas por el usuario, no son posibles con el operador `as` y, en su lugar, deben realizarse mediante expresiones de conversión.</span><span class="sxs-lookup"><span data-stu-id="212c8-2248">Note that some conversions, such as user defined conversions, are not possible with the `as` operator and should instead be performed using cast expressions.</span></span>

<span data-ttu-id="212c8-2249">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2249">In the example</span></span>
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
<span data-ttu-id="212c8-2250">se sabe que el parámetro de tipo `T` de `G` es un tipo de referencia, porque tiene la restricción de clase.</span><span class="sxs-lookup"><span data-stu-id="212c8-2250">the type parameter `T` of `G` is known to be a reference type, because it has the class constraint.</span></span> <span data-ttu-id="212c8-2251">Sin embargo, el parámetro de tipo `U` de `H` no es; por lo tanto, no se permite el uso del operador `as` en `H`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2251">The type parameter `U` of `H` is not however; hence the use of the `as` operator in `H` is disallowed.</span></span>

## <a name="logical-operators"></a><span data-ttu-id="212c8-2252">Operadores lógicos</span><span class="sxs-lookup"><span data-stu-id="212c8-2252">Logical operators</span></span>

<span data-ttu-id="212c8-2253">Los operadores `&`, `^` y `|` se denominan operadores lógicos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2253">The `&`, `^`, and `|` operators are called the logical operators.</span></span>

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

<span data-ttu-id="212c8-2254">Si un operando de un operador lógico tiene el tipo en tiempo de compilación `dynamic`, la expresión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2254">If an operand of a logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="212c8-2255">En este caso, el tipo en tiempo de compilación de la expresión es `dynamic`, y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución de los operandos que tienen el tipo en tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2255">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="212c8-2256">En el caso de una operación con el formato `x op y`, donde `op` es uno de los operadores lógicos, se aplica la resolución de sobrecarga ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica.</span><span class="sxs-lookup"><span data-stu-id="212c8-2256">For an operation of the form `x op y`, where `op` is one of the logical operators, overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) is applied to select a specific operator implementation.</span></span> <span data-ttu-id="212c8-2257">Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.</span><span class="sxs-lookup"><span data-stu-id="212c8-2257">The operands are converted to the parameter types of the selected operator, and the type of the result is the return type of the operator.</span></span>

<span data-ttu-id="212c8-2258">En las secciones siguientes se describen los operadores lógicos predefinidos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2258">The predefined logical operators are described in the following sections.</span></span>

### <a name="integer-logical-operators"></a><span data-ttu-id="212c8-2259">Operadores lógicos enteros</span><span class="sxs-lookup"><span data-stu-id="212c8-2259">Integer logical operators</span></span>

<span data-ttu-id="212c8-2260">Los operadores lógicos de enteros predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="212c8-2260">The predefined integer logical operators are:</span></span>
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

<span data-ttu-id="212c8-2261">El operador `&` calcula el @no__t lógico bit a bit de los dos operandos, el operador `|` calcula el @no__t lógico bit a bit de los dos operandos y el operador `^` calcula la @no__t lógica exclusiva bit a bit de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2261">The `&` operator computes the bitwise logical `AND` of the two operands, the `|` operator computes the bitwise logical `OR` of the two operands, and the `^` operator computes the bitwise logical exclusive `OR` of the two operands.</span></span> <span data-ttu-id="212c8-2262">No es posible realizar desbordamientos en estas operaciones.</span><span class="sxs-lookup"><span data-stu-id="212c8-2262">No overflows are possible from these operations.</span></span>

### <a name="enumeration-logical-operators"></a><span data-ttu-id="212c8-2263">Operadores lógicos de enumeración</span><span class="sxs-lookup"><span data-stu-id="212c8-2263">Enumeration logical operators</span></span>

<span data-ttu-id="212c8-2264">Cada tipo de enumeración `E` proporciona implícitamente los siguientes operadores lógicos predefinidos:</span><span class="sxs-lookup"><span data-stu-id="212c8-2264">Every enumeration type `E` implicitly provides the following predefined logical operators:</span></span>

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

<span data-ttu-id="212c8-2265">El resultado de evaluar `x op y`, donde `x` y `y` son expresiones de un tipo de enumeración `E` con un tipo subyacente `U` y `op` es uno de los operadores lógicos, es exactamente igual que evaluar `(E)((U)x op (U)y)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2265">The result of evaluating `x op y`, where `x` and `y` are expressions of an enumeration type `E` with an underlying type `U`, and `op` is one of the logical operators, is exactly the same as evaluating `(E)((U)x op (U)y)`.</span></span> <span data-ttu-id="212c8-2266">En otras palabras, los operadores lógicos de tipo de enumeración simplemente realizan la operación lógica en el tipo subyacente de los dos operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2266">In other words, the enumeration type logical operators simply perform the logical operation on the underlying type of the two operands.</span></span>

### <a name="boolean-logical-operators"></a><span data-ttu-id="212c8-2267">Operadores lógicos booleanos</span><span class="sxs-lookup"><span data-stu-id="212c8-2267">Boolean logical operators</span></span>

<span data-ttu-id="212c8-2268">Los operadores lógicos booleanos predefinidos son:</span><span class="sxs-lookup"><span data-stu-id="212c8-2268">The predefined boolean logical operators are:</span></span>
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

<span data-ttu-id="212c8-2269">El resultado de `x & y` es `true` si tanto `x` como `y` son `true`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2269">The result of `x & y` is `true` if both `x` and `y` are `true`.</span></span> <span data-ttu-id="212c8-2270">De lo contrario, el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2270">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="212c8-2271">El resultado de `x | y` es `true` si `x` o `y` es `true`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2271">The result of `x | y` is `true` if either `x` or `y` is `true`.</span></span> <span data-ttu-id="212c8-2272">De lo contrario, el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2272">Otherwise, the result is `false`.</span></span>

<span data-ttu-id="212c8-2273">El resultado de `x ^ y` es `true` si @no__t 2 es `true` y `y` es `false`, o `x` es `false` y `y` es `true`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2273">The result of `x ^ y` is `true` if `x` is `true` and `y` is `false`, or `x` is `false` and `y` is `true`.</span></span> <span data-ttu-id="212c8-2274">De lo contrario, el resultado es `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2274">Otherwise, the result is `false`.</span></span> <span data-ttu-id="212c8-2275">Cuando los operandos son del tipo `bool`, el operador `^` calcula el mismo resultado que el operador `!=`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2275">When the operands are of type `bool`, the `^` operator computes the same result as the `!=` operator.</span></span>

### <a name="nullable-boolean-logical-operators"></a><span data-ttu-id="212c8-2276">Operadores lógicos booleanos que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="212c8-2276">Nullable boolean logical operators</span></span>

<span data-ttu-id="212c8-2277">El tipo booleano que acepta valores NULL `bool?` puede representar tres valores, `true`, `false` y `null`, y es conceptualmente similar al tipo de tres valores que se usa para las Expresiones booleanas en SQL.</span><span class="sxs-lookup"><span data-stu-id="212c8-2277">The nullable boolean type `bool?` can represent three values, `true`, `false`, and `null`, and is conceptually similar to the three-valued type used for boolean expressions in SQL.</span></span> <span data-ttu-id="212c8-2278">Para asegurarse de que los resultados generados por los operadores `&` y `|` para los operandos `bool?` son coherentes con la lógica de tres valores de SQL, se proporcionan los siguientes operadores predefinidos:</span><span class="sxs-lookup"><span data-stu-id="212c8-2278">To ensure that the results produced by the `&` and `|` operators for `bool?` operands are consistent with SQL's three-valued logic, the following predefined operators are provided:</span></span>

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

<span data-ttu-id="212c8-2279">En la tabla siguiente se enumeran los resultados generados por estos operadores para todas las combinaciones de los valores `true`, `false` y `null`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2279">The following table lists the results produced by these operators for all combinations of the values `true`, `false`, and `null`.</span></span>

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

## <a name="conditional-logical-operators"></a><span data-ttu-id="212c8-2280">Operadores lógicos condicionales</span><span class="sxs-lookup"><span data-stu-id="212c8-2280">Conditional logical operators</span></span>

<span data-ttu-id="212c8-2281">Los operadores `&&` y `||` se denominan operadores lógicos condicionales.</span><span class="sxs-lookup"><span data-stu-id="212c8-2281">The `&&` and `||` operators are called the conditional logical operators.</span></span> <span data-ttu-id="212c8-2282">También se denominan operadores lógicos "de cortocircuito".</span><span class="sxs-lookup"><span data-stu-id="212c8-2282">They are also called the "short-circuiting" logical operators.</span></span>

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

<span data-ttu-id="212c8-2283">Los operadores `&&` y `||` son versiones condicionales de los operadores @no__t 2 y `|`:</span><span class="sxs-lookup"><span data-stu-id="212c8-2283">The `&&` and `||` operators are conditional versions of the `&` and `|` operators:</span></span>

*  <span data-ttu-id="212c8-2284">La operación `x && y` corresponde a la operación `x & y`, salvo que `y` solo se evalúa si `x` no es `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2284">The operation `x && y` corresponds to the operation `x & y`, except that `y` is evaluated only if `x` is not `false`.</span></span>
*  <span data-ttu-id="212c8-2285">La operación `x || y` corresponde a la operación `x | y`, salvo que `y` solo se evalúa si `x` no es `true`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2285">The operation `x || y` corresponds to the operation `x | y`, except that `y` is evaluated only if `x` is not `true`.</span></span>

<span data-ttu-id="212c8-2286">Si un operando de un operador lógico condicional tiene el tipo en tiempo de compilación `dynamic`, la expresión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2286">If an operand of a conditional logical operator has the compile-time type `dynamic`, then the expression is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="212c8-2287">En este caso, el tipo en tiempo de compilación de la expresión es `dynamic`, y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución de los operandos que tienen el tipo en tiempo de compilación `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2287">In this case the compile-time type of the expression is `dynamic`, and the resolution described below will take place at run-time using the run-time type of those operands that have the compile-time type `dynamic`.</span></span>

<span data-ttu-id="212c8-2288">Una operación con el formato `x && y` o `x || y` se procesa aplicando la resolución de sobrecarga ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) como si la operación estuviera escrita `x & y` o `x | y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2288">An operation of the form `x && y` or `x || y` is processed by applying overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x & y` or `x | y`.</span></span> <span data-ttu-id="212c8-2289">A</span><span class="sxs-lookup"><span data-stu-id="212c8-2289">Then,</span></span>

*  <span data-ttu-id="212c8-2290">Si la resolución de sobrecarga no encuentra un único operador mejor, o si la resolución de sobrecarga selecciona uno de los operadores lógicos de enteros predefinidos, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-2290">If overload resolution fails to find a single best operator, or if overload resolution selects one of the predefined integer logical operators, a binding-time error occurs.</span></span>
*  <span data-ttu-id="212c8-2291">De lo contrario, si el operador seleccionado es uno de los operadores lógicos booleanos predefinidos (operadores[lógicos booleanos](expressions.md#boolean-logical-operators)) o los operadores lógicos booleanos que aceptan valores NULL ([operadores lógicos booleanos que aceptan valores NULL](expressions.md#nullable-boolean-logical-operators)), la operación se procesa como se describe en [ Operadores lógicos condicionales booleanos](expressions.md#boolean-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="212c8-2291">Otherwise, if the selected operator is one of the predefined boolean logical operators ([Boolean logical operators](expressions.md#boolean-logical-operators)) or nullable boolean logical operators ([Nullable boolean logical operators](expressions.md#nullable-boolean-logical-operators)), the operation is processed as described in [Boolean conditional logical operators](expressions.md#boolean-conditional-logical-operators).</span></span>
*  <span data-ttu-id="212c8-2292">De lo contrario, el operador seleccionado es un operador definido por el usuario y la operación se procesa como se describe en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="212c8-2292">Otherwise, the selected operator is a user-defined operator, and the operation is processed as described in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

<span data-ttu-id="212c8-2293">No es posible sobrecargar directamente los operadores lógicos condicionales.</span><span class="sxs-lookup"><span data-stu-id="212c8-2293">It is not possible to directly overload the conditional logical operators.</span></span> <span data-ttu-id="212c8-2294">Sin embargo, dado que los operadores lógicos condicionales se evalúan en términos de los operadores lógicos normales, las sobrecargas de los operadores lógicos normales son, con ciertas restricciones, que también se consideran sobrecargas de los operadores lógicos condicionales.</span><span class="sxs-lookup"><span data-stu-id="212c8-2294">However, because the conditional logical operators are evaluated in terms of the regular logical operators, overloads of the regular logical operators are, with certain restrictions, also considered overloads of the conditional logical operators.</span></span> <span data-ttu-id="212c8-2295">Esto se describe con más detalle en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators).</span><span class="sxs-lookup"><span data-stu-id="212c8-2295">This is described further in [User-defined conditional logical operators](expressions.md#user-defined-conditional-logical-operators).</span></span>

### <a name="boolean-conditional-logical-operators"></a><span data-ttu-id="212c8-2296">Operadores lógicos condicionales booleanos</span><span class="sxs-lookup"><span data-stu-id="212c8-2296">Boolean conditional logical operators</span></span>

<span data-ttu-id="212c8-2297">Cuando los operandos de `&&` o `||` son del tipo `bool`, o cuando los operandos son de tipos que no definen un `operator &` o `operator |` aplicable, pero definen conversiones implícitas a `bool`, la operación se procesa de la siguiente manera: :</span><span class="sxs-lookup"><span data-stu-id="212c8-2297">When the operands of `&&` or `||` are of type `bool`, or when the operands are of types that do not define an applicable `operator &` or `operator |`, but do define implicit conversions to `bool`, the operation is processed as follows:</span></span>

*  <span data-ttu-id="212c8-2298">La operación `x && y` se evalúa como `x ? y : false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2298">The operation `x && y` is evaluated as `x ? y : false`.</span></span> <span data-ttu-id="212c8-2299">En otras palabras, `x` se evalúa primero y se convierte al tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2299">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="212c8-2300">Después, si `x` es `true`, `y` se evalúa y se convierte al tipo `bool`, y esto se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2300">Then, if `x` is `true`, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span> <span data-ttu-id="212c8-2301">De lo contrario, el resultado de la operación es `false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2301">Otherwise, the result of the operation is `false`.</span></span>
*  <span data-ttu-id="212c8-2302">La operación `x || y` se evalúa como `x ? true : y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2302">The operation `x || y` is evaluated as `x ? true : y`.</span></span> <span data-ttu-id="212c8-2303">En otras palabras, `x` se evalúa primero y se convierte al tipo `bool`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2303">In other words, `x` is first evaluated and converted to type `bool`.</span></span> <span data-ttu-id="212c8-2304">A continuación, si `x` es `true`, el resultado de la operación es @no__t 2.</span><span class="sxs-lookup"><span data-stu-id="212c8-2304">Then, if `x` is `true`, the result of the operation is `true`.</span></span> <span data-ttu-id="212c8-2305">De lo contrario, se evalúa `y` y se convierte al tipo `bool`, lo que se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2305">Otherwise, `y` is evaluated and converted to type `bool`, and this becomes the result of the operation.</span></span>

### <a name="user-defined-conditional-logical-operators"></a><span data-ttu-id="212c8-2306">Operadores lógicos condicionales definidos por el usuario</span><span class="sxs-lookup"><span data-stu-id="212c8-2306">User-defined conditional logical operators</span></span>

<span data-ttu-id="212c8-2307">Cuando los operandos de `&&` o `||` son de tipos que declaran un @no__t válido definido por el usuario o `operator |`, deben cumplirse las dos condiciones siguientes, donde `T` es el tipo en el que se declara el operador seleccionado:</span><span class="sxs-lookup"><span data-stu-id="212c8-2307">When the operands of `&&` or `||` are of types that declare an applicable user-defined `operator &` or `operator |`, both of the following must be true, where `T` is the type in which the selected operator is declared:</span></span>

*  <span data-ttu-id="212c8-2308">El tipo de valor devuelto y el tipo de cada parámetro del operador seleccionado deben ser `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2308">The return type and the type of each parameter of the selected operator must be `T`.</span></span> <span data-ttu-id="212c8-2309">En otras palabras, el operador debe calcular el @no__t lógico-0 o el @no__t lógico-1 de dos operandos de tipo `T` y debe devolver un resultado de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2309">In other words, the operator must compute the logical `AND` or the logical `OR` of two operands of type `T`, and must return a result of type `T`.</span></span>
*  <span data-ttu-id="212c8-2310">`T` debe contener declaraciones de `operator true` y `operator false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2310">`T` must contain declarations of `operator true` and `operator false`.</span></span>

<span data-ttu-id="212c8-2311">Se produce un error en tiempo de enlace si no se cumple alguno de estos requisitos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2311">A binding-time error occurs if either of these requirements is not satisfied.</span></span> <span data-ttu-id="212c8-2312">De lo contrario, la operación `&&` o `||` se evalúa combinando el @no__t 2 o el `operator false` definidos por el usuario con el operador definido por el usuario seleccionado:</span><span class="sxs-lookup"><span data-stu-id="212c8-2312">Otherwise, the `&&` or `||` operation is evaluated by combining the user-defined `operator true` or `operator false` with the selected user-defined operator:</span></span>

*  <span data-ttu-id="212c8-2313">La operación `x && y` se evalúa como `T.false(x) ? x : T.&(x, y)`, donde `T.false(x)` es una invocación del `operator false` declarado en `T` y `T.&(x, y)` es una invocación del `operator &` seleccionado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2313">The operation `x && y` is evaluated as `T.false(x) ? x : T.&(x, y)`, where `T.false(x)` is an invocation of the `operator false` declared in `T`, and `T.&(x, y)` is an invocation of the selected `operator &`.</span></span> <span data-ttu-id="212c8-2314">En otras palabras, primero se evalúa `x` y se invoca `operator false` en el resultado para determinar si `x` es definitivamente false.</span><span class="sxs-lookup"><span data-stu-id="212c8-2314">In other words, `x` is first evaluated and `operator false` is invoked on the result to determine if `x` is definitely false.</span></span> <span data-ttu-id="212c8-2315">Después, si `x` es definitivamente false, el resultado de la operación es el valor previamente calculado para `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2315">Then, if `x` is definitely false, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="212c8-2316">De lo contrario, se evalúa `y` y se invoca el `operator &` seleccionado en el valor calculado anteriormente para `x` y el valor calculado para `y` para generar el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2316">Otherwise, `y` is evaluated, and the selected `operator &` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>
*  <span data-ttu-id="212c8-2317">La operación `x || y` se evalúa como `T.true(x) ? x : T.|(x, y)`, donde `T.true(x)` es una invocación del `operator true` declarado en `T` y `T.|(x,y)` es una invocación del `operator|` seleccionado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2317">The operation `x || y` is evaluated as `T.true(x) ? x : T.|(x, y)`, where `T.true(x)` is an invocation of the `operator true` declared in `T`, and `T.|(x,y)` is an invocation of the selected `operator|`.</span></span> <span data-ttu-id="212c8-2318">En otras palabras, primero se evalúa `x` y se invoca `operator true` en el resultado para determinar si `x` es definitivamente true.</span><span class="sxs-lookup"><span data-stu-id="212c8-2318">In other words, `x` is first evaluated and `operator true` is invoked on the result to determine if `x` is definitely true.</span></span> <span data-ttu-id="212c8-2319">Después, si `x` es definitivamente true, el resultado de la operación es el valor previamente calculado para `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2319">Then, if `x` is definitely true, the result of the operation is the value previously computed for `x`.</span></span> <span data-ttu-id="212c8-2320">De lo contrario, se evalúa `y` y se invoca el `operator |` seleccionado en el valor calculado anteriormente para `x` y el valor calculado para `y` para generar el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2320">Otherwise, `y` is evaluated, and the selected `operator |` is invoked on the value previously computed for `x` and the value computed for `y` to produce the result of the operation.</span></span>

<span data-ttu-id="212c8-2321">En cualquiera de estas operaciones, la expresión proporcionada por `x` solo se evalúa una vez, y la expresión proporcionada por `y` no se evalúa ni se evalúa exactamente una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-2321">In either of these operations, the expression given by `x` is only evaluated once, and the expression given by `y` is either not evaluated or evaluated exactly once.</span></span>

<span data-ttu-id="212c8-2322">Para obtener un ejemplo de un tipo que implementa `operator true` y `operator false`, vea [Database Boolean Type](structs.md#database-boolean-type).</span><span class="sxs-lookup"><span data-stu-id="212c8-2322">For an example of a type that implements `operator true` and `operator false`, see [Database boolean type](structs.md#database-boolean-type).</span></span>

## <a name="the-null-coalescing-operator"></a><span data-ttu-id="212c8-2323">Operador de uso combinado de null</span><span class="sxs-lookup"><span data-stu-id="212c8-2323">The null coalescing operator</span></span>

<span data-ttu-id="212c8-2324">El operador `??` se denomina operador de uso combinado de NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-2324">The `??` operator is called the null coalescing operator.</span></span>

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

<span data-ttu-id="212c8-2325">Una expresión de fusión nula con el formato `a ?? b` requiere que `a` sea de un tipo que acepte valores NULL o un tipo de referencia.</span><span class="sxs-lookup"><span data-stu-id="212c8-2325">A null coalescing expression of the form `a ?? b` requires `a` to be of a nullable type or reference type.</span></span> <span data-ttu-id="212c8-2326">Si `a` no es null, el resultado de `a ?? b` es `a`; de lo contrario, el resultado es `b`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2326">If `a` is non-null, the result of `a ?? b` is `a`; otherwise, the result is `b`.</span></span> <span data-ttu-id="212c8-2327">La operación evalúa `b` solo si `a` es NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-2327">The operation evaluates `b` only if `a` is null.</span></span>

<span data-ttu-id="212c8-2328">El operador de uso combinado de NULL es asociativo a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda.</span><span class="sxs-lookup"><span data-stu-id="212c8-2328">The null coalescing operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="212c8-2329">Por ejemplo, una expresión con el formato `a ?? b ?? c` se evalúa como `a ?? (b ?? c)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2329">For example, an expression of the form `a ?? b ?? c` is evaluated as `a ?? (b ?? c)`.</span></span> <span data-ttu-id="212c8-2330">En términos generales, una expresión con el formato `E1 ?? E2 ?? ... ?? En` devuelve el primero de los operandos que no son NULL, o null si todos los operandos son NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-2330">In general terms, an expression of the form `E1 ?? E2 ?? ... ?? En` returns the first of the operands that is non-null, or null if all operands are null.</span></span>

<span data-ttu-id="212c8-2331">El tipo de la expresión `a ?? b` depende de las conversiones implícitas que están disponibles en los operandos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2331">The type of the expression `a ?? b` depends on which implicit conversions are available on the operands.</span></span> <span data-ttu-id="212c8-2332">En orden de preferencia, el tipo de `a ?? b` es `A0`, `A` o `B`, donde `A` es el tipo de `a` (siempre que `a` tenga un tipo), `B` es el tipo de `b` (siempre que `b` tenga un tipo). y 0 es el tipo subyacente de 1 si 2 es un tipo que acepta valores NULL o 3 en caso contrario.</span><span class="sxs-lookup"><span data-stu-id="212c8-2332">In order of preference, the type of `a ?? b` is `A0`, `A`, or `B`, where `A` is the type of `a` (provided that `a` has a type), `B` is the type of `b` (provided that `b` has a type), and `A0` is the underlying type of `A` if `A` is a nullable type, or `A` otherwise.</span></span> <span data-ttu-id="212c8-2333">En concreto, `a ?? b` se procesa como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-2333">Specifically, `a ?? b` is processed as follows:</span></span>

*  <span data-ttu-id="212c8-2334">Si `A` existe y no es un tipo que acepta valores NULL o un tipo de referencia, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2334">If `A` exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>
*  <span data-ttu-id="212c8-2335">Si `b` es una expresión dinámica, el tipo de resultado es `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2335">If `b` is a dynamic expression, the result type is `dynamic`.</span></span> <span data-ttu-id="212c8-2336">En tiempo de ejecución, se evalúa primero `a`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2336">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="212c8-2337">Si `a` no es null, `a` se convierte en dinámico y se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2337">If `a` is not null, `a` is converted to dynamic, and this becomes the result.</span></span> <span data-ttu-id="212c8-2338">De lo contrario, se evalúa `b` y se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2338">Otherwise, `b` is evaluated, and this becomes the result.</span></span>
*  <span data-ttu-id="212c8-2339">De lo contrario, si `A` existe y es un tipo que acepta valores NULL y existe una conversión implícita de `b` a `A0`, el tipo de resultado es `A0`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2339">Otherwise, if `A` exists and is a nullable type and an implicit conversion exists from `b` to `A0`, the result type is `A0`.</span></span> <span data-ttu-id="212c8-2340">En tiempo de ejecución, se evalúa primero `a`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2340">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="212c8-2341">Si `a` no es null, `a` se desencapsulará en el tipo `A0` y se convertirá en el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2341">If `a` is not null, `a` is unwrapped to type `A0`, and this becomes the result.</span></span> <span data-ttu-id="212c8-2342">De lo contrario, se evalúa `b` y se convierte al tipo `A0`, lo que se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2342">Otherwise, `b` is evaluated and converted to type `A0`, and this becomes the result.</span></span>
*  <span data-ttu-id="212c8-2343">De lo contrario, si `A` existe y existe una conversión implícita de `b` a `A`, el tipo de resultado es `A`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2343">Otherwise, if `A` exists and an implicit conversion exists from `b` to `A`, the result type is `A`.</span></span> <span data-ttu-id="212c8-2344">En tiempo de ejecución, se evalúa primero `a`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2344">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="212c8-2345">Si `a` no es null, `a` se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2345">If `a` is not null, `a` becomes the result.</span></span> <span data-ttu-id="212c8-2346">De lo contrario, se evalúa `b` y se convierte al tipo `A`, lo que se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2346">Otherwise, `b` is evaluated and converted to type `A`, and this becomes the result.</span></span>
*  <span data-ttu-id="212c8-2347">De lo contrario, si `b` tiene un tipo `B` y existe una conversión implícita de `a` a `B`, el tipo de resultado es `B`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2347">Otherwise, if `b` has a type `B` and an implicit conversion exists from `a` to `B`, the result type is `B`.</span></span> <span data-ttu-id="212c8-2348">En tiempo de ejecución, se evalúa primero `a`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2348">At run-time, `a` is first evaluated.</span></span> <span data-ttu-id="212c8-2349">Si `a` no es null, `a` no se ajusta al tipo `A0` (si @no__t existe y acepta valores NULL) y se convierte al tipo `B`, y esto se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2349">If `a` is not null, `a` is unwrapped to type `A0` (if `A` exists and is nullable) and converted to type `B`, and this becomes the result.</span></span> <span data-ttu-id="212c8-2350">De lo contrario, se evalúa `b` y se convierte en el resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2350">Otherwise, `b` is evaluated and becomes the result.</span></span>
*  <span data-ttu-id="212c8-2351">De lo contrario, `a` y `b` son incompatibles y se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2351">Otherwise, `a` and `b` are incompatible, and a compile-time error occurs.</span></span>

## <a name="conditional-operator"></a><span data-ttu-id="212c8-2352">Operador condicional</span><span class="sxs-lookup"><span data-stu-id="212c8-2352">Conditional operator</span></span>

<span data-ttu-id="212c8-2353">El operador `?:` se denomina operador condicional.</span><span class="sxs-lookup"><span data-stu-id="212c8-2353">The `?:` operator is called the conditional operator.</span></span> <span data-ttu-id="212c8-2354">En ocasiones también se denomina operador ternario.</span><span class="sxs-lookup"><span data-stu-id="212c8-2354">It is at times also called the ternary operator.</span></span>

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

<span data-ttu-id="212c8-2355">Una expresión condicional con el formato `b ? x : y` primero evalúa la condición `b`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2355">A conditional expression of the form `b ? x : y` first evaluates the condition `b`.</span></span> <span data-ttu-id="212c8-2356">A continuación, si `b` es `true`, se evalúa @no__t 2 y se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2356">Then, if `b` is `true`, `x` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="212c8-2357">De lo contrario, se evalúa `y` y se convierte en el resultado de la operación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2357">Otherwise, `y` is evaluated and becomes the result of the operation.</span></span> <span data-ttu-id="212c8-2358">Una expresión condicional nunca evalúa `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2358">A conditional expression never evaluates both `x` and `y`.</span></span>

<span data-ttu-id="212c8-2359">El operador condicional es asociativo a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda.</span><span class="sxs-lookup"><span data-stu-id="212c8-2359">The conditional operator is right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="212c8-2360">Por ejemplo, una expresión con el formato `a ? b : c ? d : e` se evalúa como `a ? b : (c ? d : e)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2360">For example, an expression of the form `a ? b : c ? d : e` is evaluated as `a ? b : (c ? d : e)`.</span></span>

<span data-ttu-id="212c8-2361">El primer operando del operador `?:` debe ser una expresión que se pueda convertir implícitamente a `bool`, o una expresión de un tipo que implementa `operator true`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2361">The first operand of the `?:` operator must be an expression that can be implicitly converted to `bool`, or an expression of a type that implements `operator true`.</span></span> <span data-ttu-id="212c8-2362">Si no se cumple ninguno de estos requisitos, se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2362">If neither of these requirements is satisfied, a compile-time error occurs.</span></span>

<span data-ttu-id="212c8-2363">Los operandos segundo y tercero, `x` y `y`, del operador `?:` controlan el tipo de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="212c8-2363">The second and third operands, `x` and `y`, of the `?:` operator control the type of the conditional expression.</span></span>

*  <span data-ttu-id="212c8-2364">Si `x` tiene el tipo `X` y `y` tiene el tipo `Y`, entonces</span><span class="sxs-lookup"><span data-stu-id="212c8-2364">If `x` has type `X` and `y` has type `Y` then</span></span>
   * <span data-ttu-id="212c8-2365">Si existe una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) de `X` a `Y`, pero no de `Y` a `X`, `Y` es el tipo de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="212c8-2365">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `X` to `Y`, but not from `Y` to `X`, then `Y` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="212c8-2366">Si existe una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) de `Y` a `X`, pero no de `X` a `Y`, `X` es el tipo de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="212c8-2366">If an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)) exists from `Y` to `X`, but not from `X` to `Y`, then `X` is the type of the conditional expression.</span></span>
   * <span data-ttu-id="212c8-2367">De lo contrario, no se puede determinar ningún tipo de expresión y se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2367">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>
*  <span data-ttu-id="212c8-2368">Si solo uno de `x` y `y` tiene un tipo, y `x` y `y`, de se pueden convertir implícitamente a ese tipo, es decir, el tipo de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="212c8-2368">If only one of `x` and `y` has a type, and both `x` and `y`, of are implicitly convertible to that type, then that is the type of the conditional expression.</span></span>
*  <span data-ttu-id="212c8-2369">De lo contrario, no se puede determinar ningún tipo de expresión y se produce un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2369">Otherwise, no expression type can be determined, and a compile-time error occurs.</span></span>

<span data-ttu-id="212c8-2370">El procesamiento en tiempo de ejecución de una expresión condicional con el formato `b ? x : y` consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-2370">The run-time processing of a conditional expression of the form `b ? x : y` consists of the following steps:</span></span>

*  <span data-ttu-id="212c8-2371">En primer lugar, se evalúa `b` y se determina el valor `bool` de `b`:</span><span class="sxs-lookup"><span data-stu-id="212c8-2371">First, `b` is evaluated, and the `bool` value of `b` is determined:</span></span>
   * <span data-ttu-id="212c8-2372">Si existe una conversión implícita del tipo de `b` a `bool`, esta conversión implícita se realiza para generar un valor `bool`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2372">If an implicit conversion from the type of `b` to `bool` exists, then this implicit conversion is performed to produce a `bool` value.</span></span>
   * <span data-ttu-id="212c8-2373">De lo contrario, se invoca el `operator true` definido por el tipo de `b` para generar un valor `bool`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2373">Otherwise, the `operator true` defined by the type of `b` is invoked to produce a `bool` value.</span></span>
*  <span data-ttu-id="212c8-2374">Si el valor `bool` producido por el paso anterior es `true`, `x` se evalúa y se convierte al tipo de la expresión condicional, y esto se convierte en el resultado de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="212c8-2374">If the `bool` value produced by the step above is `true`, then `x` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>
*  <span data-ttu-id="212c8-2375">De lo contrario, se evalúa `y` y se convierte al tipo de la expresión condicional, lo que se convierte en el resultado de la expresión condicional.</span><span class="sxs-lookup"><span data-stu-id="212c8-2375">Otherwise, `y` is evaluated and converted to the type of the conditional expression, and this becomes the result of the conditional expression.</span></span>

## <a name="anonymous-function-expressions"></a><span data-ttu-id="212c8-2376">Expresiones de función anónima</span><span class="sxs-lookup"><span data-stu-id="212c8-2376">Anonymous function expressions</span></span>

<span data-ttu-id="212c8-2377">Una ***función anónima*** es una expresión que representa una definición de método "en línea".</span><span class="sxs-lookup"><span data-stu-id="212c8-2377">An ***anonymous function*** is an expression that represents an "in-line" method definition.</span></span> <span data-ttu-id="212c8-2378">Una función anónima no tiene un valor o un tipo en y de sí mismo, pero se pueden convertir en un tipo de árbol de expresión o delegado compatible.</span><span class="sxs-lookup"><span data-stu-id="212c8-2378">An anonymous function does not have a value or type in and of itself, but is convertible to a compatible delegate or expression tree type.</span></span> <span data-ttu-id="212c8-2379">La evaluación de una conversión de función anónima depende del tipo de destino de la conversión: Si es un tipo de delegado, la conversión se evalúa como un valor de delegado que hace referencia al método que define la función anónima.</span><span class="sxs-lookup"><span data-stu-id="212c8-2379">The evaluation of an anonymous function conversion depends on the target type of the conversion: If it is a delegate type, the conversion evaluates to a delegate value referencing the method which the anonymous function defines.</span></span> <span data-ttu-id="212c8-2380">Si es un tipo de árbol de expresión, la conversión se evalúa como un árbol de expresión que representa la estructura del método como una estructura de objeto.</span><span class="sxs-lookup"><span data-stu-id="212c8-2380">If it is an expression tree type, the conversion evaluates to an expression tree which represents the structure of the method as an object structure.</span></span>

<span data-ttu-id="212c8-2381">Por motivos históricos, hay dos tipos sintácticos de funciones anónimas, es decir, *lambda_expression*s y *anonymous_method_expression*s.</span><span class="sxs-lookup"><span data-stu-id="212c8-2381">For historical reasons there are two syntactic flavors of anonymous functions, namely *lambda_expression*s and *anonymous_method_expression*s.</span></span> <span data-ttu-id="212c8-2382">Para casi todos los propósitos, *lambda_expression*s son más concisos y expresivos que *anonymous_method_expression*s, que permanecen en el lenguaje por compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="212c8-2382">For almost all purposes, *lambda_expression*s are more concise and expressive than *anonymous_method_expression*s, which remain in the language for backwards compatibility.</span></span>

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

<span data-ttu-id="212c8-2383">El operador `=>` tiene la misma prioridad que la asignación (`=`) y es asociativo a la derecha.</span><span class="sxs-lookup"><span data-stu-id="212c8-2383">The `=>` operator has the same precedence as assignment (`=`) and is right-associative.</span></span>

<span data-ttu-id="212c8-2384">Una función anónima con el modificador `async` es una función asincrónica y sigue las reglas descritas en [iteradores](classes.md#iterators).</span><span class="sxs-lookup"><span data-stu-id="212c8-2384">An anonymous function with the `async` modifier is an async function and follows the rules described in [Iterators](classes.md#iterators).</span></span>

<span data-ttu-id="212c8-2385">Los parámetros de una función anónima en forma de *lambda_expression* se pueden escribir explícita o implícitamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2385">The parameters of an anonymous function in the form of a *lambda_expression* can be explicitly or implicitly typed.</span></span> <span data-ttu-id="212c8-2386">En una lista de parámetros con tipo explícito, se indica explícitamente el tipo de cada parámetro.</span><span class="sxs-lookup"><span data-stu-id="212c8-2386">In an explicitly typed parameter list, the type of each parameter is explicitly stated.</span></span> <span data-ttu-id="212c8-2387">En una lista de parámetros con tipos implícitos, los tipos de los parámetros se deducen del contexto en el que se produce la función anónima; en concreto, cuando la función anónima se convierte en un tipo de delegado o un tipo de árbol de expresión compatible, ese tipo proporciona los tipos de parámetro ([conversiones de funciones anónimas](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2387">In an implicitly typed parameter list, the types of the parameters are inferred from the context in which the anonymous function occurs—specifically, when the anonymous function is converted to a compatible delegate type or expression tree type, that type provides the parameter types ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="212c8-2388">En una función anónima con un solo parámetro con tipo implícito, los paréntesis se pueden omitir en la lista de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-2388">In an anonymous function with a single, implicitly typed parameter, the parentheses may be omitted from the parameter list.</span></span> <span data-ttu-id="212c8-2389">En otras palabras, una función anónima con el formato</span><span class="sxs-lookup"><span data-stu-id="212c8-2389">In other words, an anonymous function of the form</span></span>
```csharp
( param ) => expr
```
<span data-ttu-id="212c8-2390">se puede abreviar como</span><span class="sxs-lookup"><span data-stu-id="212c8-2390">can be abbreviated to</span></span>
```csharp
param => expr
```

<span data-ttu-id="212c8-2391">La lista de parámetros de una función anónima en forma de *anonymous_method_expression* es opcional.</span><span class="sxs-lookup"><span data-stu-id="212c8-2391">The parameter list of an anonymous function in the form of an *anonymous_method_expression* is optional.</span></span> <span data-ttu-id="212c8-2392">Si se especifica, los parámetros se deben escribir explícitamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2392">If given, the parameters must be explicitly typed.</span></span> <span data-ttu-id="212c8-2393">En caso contrario, la función anónima se convertirá en un delegado con cualquier lista de parámetros que no contenga @no__t parámetros-0.</span><span class="sxs-lookup"><span data-stu-id="212c8-2393">If not, the anonymous function is convertible to a delegate with any parameter list not containing `out` parameters.</span></span>

<span data-ttu-id="212c8-2394">Se puede tener acceso al cuerpo de un *bloque* de una función anónima ([puntos de conexión y disponibilidad](statements.md#end-points-and-reachability)) a menos que la función anónima se encuentre dentro de una instrucción inalcanzable.</span><span class="sxs-lookup"><span data-stu-id="212c8-2394">A *block* body of an anonymous function is reachable ([End points and reachability](statements.md#end-points-and-reachability)) unless the anonymous function occurs inside an unreachable statement.</span></span>

<span data-ttu-id="212c8-2395">A continuación se muestran algunos ejemplos de funciones anónimas:</span><span class="sxs-lookup"><span data-stu-id="212c8-2395">Some examples of anonymous functions follow below:</span></span>

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

<span data-ttu-id="212c8-2396">El comportamiento de *lambda_expression*s y *anonymous_method_expression*s es el mismo, salvo los siguientes puntos:</span><span class="sxs-lookup"><span data-stu-id="212c8-2396">The behavior of *lambda_expression*s and *anonymous_method_expression*s is the same except for the following points:</span></span>

*  <span data-ttu-id="212c8-2397">*anonymous_method_expression*s permite omitir completamente la lista de parámetros, lo que produce Convertibility para delegar tipos de cualquier lista de parámetros de valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-2397">*anonymous_method_expression*s permit the parameter list to be omitted entirely, yielding convertibility to delegate types of any list of value parameters.</span></span>
*  <span data-ttu-id="212c8-2398">*lambda_expression*s permite omitir y deducir los tipos de parámetro, mientras que *anonymous_method_expression*s requiere que los tipos de parámetro se indiquen explícitamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2398">*lambda_expression*s permit parameter types to be omitted and inferred whereas *anonymous_method_expression*s require parameter types to be explicitly stated.</span></span>
*  <span data-ttu-id="212c8-2399">El cuerpo de una *lambda_expression* puede ser una expresión o un bloque de instrucciones, mientras que el cuerpo de un *anonymous_method_expression* debe ser un bloque de instrucciones.</span><span class="sxs-lookup"><span data-stu-id="212c8-2399">The body of a *lambda_expression* can be an expression or a statement block whereas the body of an *anonymous_method_expression* must be a statement block.</span></span>
*  <span data-ttu-id="212c8-2400">Solo *lambda_expression*s tienen conversiones a tipos de árbol de expresión compatibles ([tipos de árbol de expresión](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2400">Only *lambda_expression*s have conversions to compatible expression tree types ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-signatures"></a><span data-ttu-id="212c8-2401">Firmas de funciones anónimas</span><span class="sxs-lookup"><span data-stu-id="212c8-2401">Anonymous function signatures</span></span>

<span data-ttu-id="212c8-2402">El *anonymous_function_signature* opcional de una función anónima define los nombres y, opcionalmente, los tipos de los parámetros formales de la función anónima.</span><span class="sxs-lookup"><span data-stu-id="212c8-2402">The optional *anonymous_function_signature* of an anonymous function defines the names and optionally the types of the formal parameters for the anonymous function.</span></span> <span data-ttu-id="212c8-2403">El ámbito de los parámetros de la función anónima es *anonymous_function_body*.</span><span class="sxs-lookup"><span data-stu-id="212c8-2403">The scope of the parameters of the anonymous function is the *anonymous_function_body*.</span></span> <span data-ttu-id="212c8-2404">([Ámbitos](basic-concepts.md#scopes)) Junto con la lista de parámetros (si se especifica), el cuerpo del método anónimo constituye un espacio de declaración ([declaraciones](basic-concepts.md#declarations)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2404">([Scopes](basic-concepts.md#scopes)) Together with the parameter list (if given) the anonymous-method-body constitutes a declaration space ([Declarations](basic-concepts.md#declarations)).</span></span> <span data-ttu-id="212c8-2405">Por lo tanto, es un error en tiempo de compilación que el nombre de un parámetro de la función anónima coincida con el nombre de una variable local, una constante local o un parámetro cuyo ámbito incluya *anonymous_method_expression* o *lambda_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-2405">It is thus a compile-time error for the name of a parameter of the anonymous function to match the name of a local variable, local constant or parameter whose scope includes the *anonymous_method_expression* or *lambda_expression*.</span></span>

<span data-ttu-id="212c8-2406">Si una función anónima tiene un *explicit_anonymous_function_signature*, el conjunto de tipos de delegado compatibles y los tipos de árbol de expresión están restringidos a los que tienen los mismos tipos de parámetros y modificadores en el mismo orden.</span><span class="sxs-lookup"><span data-stu-id="212c8-2406">If an anonymous function has an *explicit_anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have the same parameter types and modifiers in the same order.</span></span> <span data-ttu-id="212c8-2407">A diferencia de las conversiones de grupos de métodos ([conversiones de grupos de métodos](conversions.md#method-group-conversions)), no se admite la varianza de tipos de parámetros de función anónimos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2407">In contrast to method group conversions ([Method group conversions](conversions.md#method-group-conversions)), contra-variance of anonymous function parameter types is not supported.</span></span> <span data-ttu-id="212c8-2408">Si una función anónima no tiene un *anonymous_function_signature*, el conjunto de tipos de delegado compatibles y los tipos de árbol de expresión está restringido a los que no tienen `out` parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-2408">If an anonymous function does not have an *anonymous_function_signature*, then the set of compatible delegate types and expression tree types is restricted to those that have no `out` parameters.</span></span>

<span data-ttu-id="212c8-2409">Tenga en cuenta que un *anonymous_function_signature* no puede incluir atributos ni una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-2409">Note that an *anonymous_function_signature* cannot include attributes or a parameter array.</span></span> <span data-ttu-id="212c8-2410">No obstante, un *anonymous_function_signature* puede ser compatible con un tipo de delegado cuya lista de parámetros contiene una matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="212c8-2410">Nevertheless, an *anonymous_function_signature* may be compatible with a delegate type whose parameter list contains a parameter array.</span></span>

<span data-ttu-id="212c8-2411">Tenga en cuenta también que la conversión a un tipo de árbol de expresión, incluso si es compatible, todavía puede producir un error en tiempo de compilación ([tipos de árbol de expresión](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2411">Note also that conversion to an expression tree type, even if compatible, may still fail at compile-time ([Expression tree types](types.md#expression-tree-types)).</span></span>

### <a name="anonymous-function-bodies"></a><span data-ttu-id="212c8-2412">Cuerpos de función anónimos</span><span class="sxs-lookup"><span data-stu-id="212c8-2412">Anonymous function bodies</span></span>

<span data-ttu-id="212c8-2413">El cuerpo (*expresión* o *bloque*) de una función anónima está sujeto a las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="212c8-2413">The body (*expression* or *block*) of an anonymous function is subject to the following rules:</span></span>

*  <span data-ttu-id="212c8-2414">Si la función anónima incluye una firma, los parámetros especificados en la firma están disponibles en el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2414">If the anonymous function includes a signature, the parameters specified in the signature are available in the body.</span></span> <span data-ttu-id="212c8-2415">Si la función anónima no tiene ninguna firma, se puede convertir en un tipo de delegado o tipo de expresión con parámetros ([conversiones de funciones anónimas](conversions.md#anonymous-function-conversions)), pero no se puede tener acceso a los parámetros en el cuerpo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2415">If the anonymous function has no signature it can be converted to a delegate type or expression type having parameters ([Anonymous function conversions](conversions.md#anonymous-function-conversions)), but the parameters cannot be accessed in the body.</span></span>
*  <span data-ttu-id="212c8-2416">Excepto en el caso de los parámetros `ref` o `out` especificados en la firma (si existe) de la función anónima envolvente más cercana, se trata de un error en tiempo de compilación para que el cuerpo tenga acceso a un parámetro `ref` o `out`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2416">Except for `ref` or `out` parameters specified in the signature (if any) of the nearest enclosing anonymous function, it is a compile-time error for the body to access a `ref` or `out` parameter.</span></span>
*  <span data-ttu-id="212c8-2417">Cuando el tipo de `this` es un tipo de estructura, se trata de un error en tiempo de compilación para que el cuerpo tenga acceso a `this`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2417">When the type of `this` is a struct type, it is a compile-time error for the body to access `this`.</span></span> <span data-ttu-id="212c8-2418">Esto es así si el acceso es explícito (como en `this.x`) o implícito (como en `x` donde `x` es un miembro de instancia de la estructura).</span><span class="sxs-lookup"><span data-stu-id="212c8-2418">This is true whether the access is explicit (as in `this.x`) or implicit (as in `x` where `x` is an instance member of the struct).</span></span> <span data-ttu-id="212c8-2419">Esta regla simplemente prohíbe el acceso y no afecta a si la búsqueda de miembros da como resultado un miembro de la estructura.</span><span class="sxs-lookup"><span data-stu-id="212c8-2419">This rule simply prohibits such access and does not affect whether member lookup results in a member of the struct.</span></span>
*  <span data-ttu-id="212c8-2420">El cuerpo tiene acceso a las variables externas ([variables externas](expressions.md#outer-variables)) de la función anónima.</span><span class="sxs-lookup"><span data-stu-id="212c8-2420">The body has access to the outer variables ([Outer variables](expressions.md#outer-variables)) of the anonymous function.</span></span> <span data-ttu-id="212c8-2421">El acceso de una variable externa hará referencia a la instancia de la variable que está activa en el momento en que se evalúa *lambda_expression* o *anonymous_method_expression* ([evaluación de expresiones de función anónimas](expressions.md#evaluation-of-anonymous-function-expressions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2421">Access of an outer variable will reference the instance of the variable that is active at the time the *lambda_expression* or *anonymous_method_expression* is evaluated ([Evaluation of anonymous function expressions](expressions.md#evaluation-of-anonymous-function-expressions)).</span></span>
*  <span data-ttu-id="212c8-2422">Se trata de un error en tiempo de compilación para que el cuerpo contenga una instrucción `goto`, una instrucción `break` o una instrucción `continue` cuyo destino esté fuera del cuerpo o dentro del cuerpo de una función anónima contenida.</span><span class="sxs-lookup"><span data-stu-id="212c8-2422">It is a compile-time error for the body to contain a `goto` statement, `break` statement, or `continue` statement whose target is outside the body or within the body of a contained anonymous function.</span></span>
*  <span data-ttu-id="212c8-2423">Una instrucción `return` en el cuerpo devuelve el control de una invocación de la función anónima envolvente más cercana, no del miembro de función envolvente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2423">A `return` statement in the body returns control from an invocation of the nearest enclosing anonymous function, not from the enclosing function member.</span></span> <span data-ttu-id="212c8-2424">Una expresión especificada en una instrucción `return` debe poder convertirse implícitamente al tipo de valor devuelto del tipo de delegado o del tipo de árbol de expresión al que se convierte el *lambda_expression* o *anonymous_method_expression* más próximo ( [Conversiones de funciones anónimas](conversions.md#anonymous-function-conversions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2424">An expression specified in a `return` statement must be implicitly convertible to the return type of the delegate type or expression tree type to which the nearest enclosing *lambda_expression* or *anonymous_method_expression* is converted ([Anonymous function conversions](conversions.md#anonymous-function-conversions)).</span></span>

<span data-ttu-id="212c8-2425">No se especifica explícitamente si hay alguna manera de ejecutar el bloque de una función anónima que no sea a través de la evaluación y la invocación de *lambda_expression* o *anonymous_method_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-2425">It is explicitly unspecified whether there is any way to execute the block of an anonymous function other than through evaluation and invocation of the *lambda_expression* or *anonymous_method_expression*.</span></span> <span data-ttu-id="212c8-2426">En concreto, el compilador puede optar por implementar una función anónima mediante la sintetización de uno o varios métodos o tipos con nombre.</span><span class="sxs-lookup"><span data-stu-id="212c8-2426">In particular, the compiler may choose to implement an anonymous function by synthesizing one or more named methods or types.</span></span> <span data-ttu-id="212c8-2427">Los nombres de estos elementos sintetizados deben tener un formato reservado para el uso del compilador.</span><span class="sxs-lookup"><span data-stu-id="212c8-2427">The names of any such synthesized elements must be of a form reserved for compiler use.</span></span>

### <a name="overload-resolution-and-anonymous-functions"></a><span data-ttu-id="212c8-2428">Resolución de sobrecarga y funciones anónimas</span><span class="sxs-lookup"><span data-stu-id="212c8-2428">Overload resolution and anonymous functions</span></span>

<span data-ttu-id="212c8-2429">Las funciones anónimas de una lista de argumentos participan en la inferencia de tipos y en la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="212c8-2429">Anonymous functions in an argument list participate in type inference and overload resolution.</span></span> <span data-ttu-id="212c8-2430">Consulte la [inferencia de tipos](expressions.md#type-inference) y la [resolución de sobrecargas](expressions.md#overload-resolution) para ver las reglas exactas.</span><span class="sxs-lookup"><span data-stu-id="212c8-2430">Please refer to [Type inference](expressions.md#type-inference) and [Overload resolution](expressions.md#overload-resolution) for the exact rules.</span></span>

<span data-ttu-id="212c8-2431">En el ejemplo siguiente se muestra el efecto de las funciones anónimas en la resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="212c8-2431">The following example illustrates the effect of anonymous functions on overload resolution.</span></span>

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

<span data-ttu-id="212c8-2432">La clase `ItemList<T>` tiene dos métodos `Sum`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2432">The `ItemList<T>` class has two `Sum` methods.</span></span> <span data-ttu-id="212c8-2433">Cada toma un argumento `selector`, que extrae el valor que se va a sumar de un elemento de lista.</span><span class="sxs-lookup"><span data-stu-id="212c8-2433">Each takes a `selector` argument, which extracts the value to sum over from a list item.</span></span> <span data-ttu-id="212c8-2434">El valor extraído puede ser un `int` o un `double` y la suma resultante es igualmente una `int` o un `double`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2434">The extracted value can be either an `int` or a `double` and the resulting sum is likewise either an `int` or a `double`.</span></span>

<span data-ttu-id="212c8-2435">Los métodos `Sum` podrían usarse, por ejemplo, para calcular las sumas de una lista de líneas de detalles de un pedido.</span><span class="sxs-lookup"><span data-stu-id="212c8-2435">The `Sum` methods could for example be used to compute sums from a list of detail lines in an order.</span></span>

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

<span data-ttu-id="212c8-2436">En la primera invocación de `orderDetails.Sum`, ambos métodos `Sum` son aplicables porque la función anónima `d => d. UnitCount` es compatible con `Func<Detail,int>` y `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2436">In the first invocation of `orderDetails.Sum`, both `Sum` methods are applicable because the anonymous function `d => d. UnitCount` is compatible with both `Func<Detail,int>` and `Func<Detail,double>`.</span></span> <span data-ttu-id="212c8-2437">Sin embargo, la resolución de sobrecarga selecciona el primer método `Sum` porque la conversión a `Func<Detail,int>` es mejor que la conversión a `Func<Detail,double>`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2437">However, overload resolution picks the first `Sum` method because the conversion to `Func<Detail,int>` is better than the conversion to `Func<Detail,double>`.</span></span>

<span data-ttu-id="212c8-2438">En la segunda invocación de `orderDetails.Sum`, solo se puede aplicar el segundo método `Sum` porque la función anónima `d => d.UnitPrice * d.UnitCount` genera un valor de tipo `double`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2438">In the second invocation of `orderDetails.Sum`, only the second `Sum` method is applicable because the anonymous function `d => d.UnitPrice * d.UnitCount` produces a value of type `double`.</span></span> <span data-ttu-id="212c8-2439">Por lo tanto, la resolución de sobrecarga elige el segundo método `Sum` para esa invocación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2439">Thus, overload resolution picks the second `Sum` method for that invocation.</span></span>

### <a name="anonymous-functions-and-dynamic-binding"></a><span data-ttu-id="212c8-2440">Funciones anónimas y enlace dinámico</span><span class="sxs-lookup"><span data-stu-id="212c8-2440">Anonymous functions and dynamic binding</span></span>

<span data-ttu-id="212c8-2441">Una función anónima no puede ser un receptor, un argumento o un operando de una operación enlazada dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2441">An anonymous function cannot be a receiver, argument or operand of a dynamically bound operation.</span></span>

### <a name="outer-variables"></a><span data-ttu-id="212c8-2442">Variables externas</span><span class="sxs-lookup"><span data-stu-id="212c8-2442">Outer variables</span></span>

<span data-ttu-id="212c8-2443">Cualquier variable local, parámetro de valor o matriz de parámetros cuyo ámbito incluya *lambda_expression* o *anonymous_method_expression* se denomina una ***variable externa*** de la función anónima.</span><span class="sxs-lookup"><span data-stu-id="212c8-2443">Any local variable, value parameter, or parameter array whose scope includes the *lambda_expression* or *anonymous_method_expression* is called an ***outer variable*** of the anonymous function.</span></span> <span data-ttu-id="212c8-2444">En un miembro de función de instancia de una clase, el valor `this` se considera un parámetro de valor y es una variable externa de cualquier función anónima incluida en el miembro de función.</span><span class="sxs-lookup"><span data-stu-id="212c8-2444">In an instance function member of a class, the `this` value is considered a value parameter and is an outer variable of any anonymous function contained within the function member.</span></span>

#### <a name="captured-outer-variables"></a><span data-ttu-id="212c8-2445">Variables externas capturadas</span><span class="sxs-lookup"><span data-stu-id="212c8-2445">Captured outer variables</span></span>

<span data-ttu-id="212c8-2446">Cuando una función anónima hace referencia a una variable externa, se dice que la función anónima ha ***capturado*** la variable externa.</span><span class="sxs-lookup"><span data-stu-id="212c8-2446">When an outer variable is referenced by an anonymous function, the outer variable is said to have been ***captured*** by the anonymous function.</span></span> <span data-ttu-id="212c8-2447">Normalmente, la duración de una variable local se limita a la ejecución del bloque o la instrucción con la que está asociada ([variables locales](variables.md#local-variables)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2447">Ordinarily, the lifetime of a local variable is limited to execution of the block or statement with which it is associated ([Local variables](variables.md#local-variables)).</span></span> <span data-ttu-id="212c8-2448">Sin embargo, la duración de una variable externa capturada se extiende al menos hasta que el delegado o el árbol de expresión creado a partir de la función anónima sea válido para la recolección de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="212c8-2448">However, the lifetime of a captured outer variable is extended at least until the delegate or expression tree created from the anonymous function becomes eligible for garbage collection.</span></span>

<span data-ttu-id="212c8-2449">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2449">In the example</span></span>
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
<span data-ttu-id="212c8-2450">la función anónima captura la variable local `x`, y la duración de `x` se extiende al menos hasta que el delegado devuelto de `F` sea válido para la recolección de elementos no utilizados (que no se produce hasta el final del programa).</span><span class="sxs-lookup"><span data-stu-id="212c8-2450">the local variable `x` is captured by the anonymous function, and the lifetime of `x` is extended at least until the delegate returned from `F` becomes eligible for garbage collection (which doesn't happen until the very end of the program).</span></span> <span data-ttu-id="212c8-2451">Dado que cada invocación de la función anónima funciona en la misma instancia de `x`, el resultado del ejemplo es:</span><span class="sxs-lookup"><span data-stu-id="212c8-2451">Since each invocation of the anonymous function operates on the same instance of `x`, the output of the example is:</span></span>
```console
1
2
3
```

<span data-ttu-id="212c8-2452">Cuando una función anónima captura una variable local o un parámetro de valor, la variable local o el parámetro ya no se considera una variable fija ([variables fijas y móviles](unsafe-code.md#fixed-and-moveable-variables)), sino que se considera una variable móvil.</span><span class="sxs-lookup"><span data-stu-id="212c8-2452">When a local variable or a value parameter is captured by an anonymous function, the local variable or parameter is no longer considered to be a fixed variable ([Fixed and moveable variables](unsafe-code.md#fixed-and-moveable-variables)), but is instead considered to be a moveable variable.</span></span> <span data-ttu-id="212c8-2453">Por lo tanto, cualquier código `unsafe` que toma la dirección de una variable externa capturada debe usar primero la instrucción `fixed` para corregir la variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-2453">Thus any `unsafe` code that takes the address of a captured outer variable must first use the `fixed` statement to fix the variable.</span></span>

<span data-ttu-id="212c8-2454">Tenga en cuenta que, a diferencia de una variable no capturada, una variable local capturada se puede exponer simultáneamente a varios subprocesos de ejecución.</span><span class="sxs-lookup"><span data-stu-id="212c8-2454">Note that unlike an uncaptured variable, a captured local variable can be simultaneously exposed to multiple threads of execution.</span></span>

#### <a name="instantiation-of-local-variables"></a><span data-ttu-id="212c8-2455">Creación de instancias de variables locales</span><span class="sxs-lookup"><span data-stu-id="212c8-2455">Instantiation of local variables</span></span>

<span data-ttu-id="212c8-2456">Se considera que se ***crea una instancia*** de una variable local cuando la ejecución entra en el ámbito de la variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-2456">A local variable is considered to be ***instantiated*** when execution enters the scope of the variable.</span></span> <span data-ttu-id="212c8-2457">Por ejemplo, cuando se invoca el método siguiente, se crea una instancia de la variable local `x` y se inicializa tres veces, una vez para cada iteración del bucle.</span><span class="sxs-lookup"><span data-stu-id="212c8-2457">For example, when the following method is invoked, the local variable `x` is instantiated and initialized three times—once for each iteration of the loop.</span></span>

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="212c8-2458">Sin embargo, si se mueve la declaración de `x` fuera del bucle, se produce una única creación de instancias de `x`:</span><span class="sxs-lookup"><span data-stu-id="212c8-2458">However, moving the declaration of `x` outside the loop results in a single instantiation of `x`:</span></span>
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

<span data-ttu-id="212c8-2459">Cuando no se capturan, no hay forma de observar exactamente la frecuencia con la que se crea una instancia de una variable local, ya que las duraciones de las instancias no se pueden usar para que cada creación de instancias use simplemente la misma ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="212c8-2459">When not captured, there is no way to observe exactly how often a local variable is instantiated—because the lifetimes of the instantiations are disjoint, it is possible for each instantiation to simply use the same storage location.</span></span> <span data-ttu-id="212c8-2460">Sin embargo, cuando una función anónima captura una variable local, los efectos de la creación de instancias se hacen evidentes.</span><span class="sxs-lookup"><span data-stu-id="212c8-2460">However, when an anonymous function captures a local variable, the effects of instantiation become apparent.</span></span>

<span data-ttu-id="212c8-2461">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2461">The example</span></span>
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
<span data-ttu-id="212c8-2462">genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="212c8-2462">produces the output:</span></span>
```console
1
3
5
```

<span data-ttu-id="212c8-2463">Sin embargo, cuando la declaración de `x` se mueve fuera del bucle:</span><span class="sxs-lookup"><span data-stu-id="212c8-2463">However, when the declaration of `x` is moved outside the loop:</span></span>
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
<span data-ttu-id="212c8-2464">El resultado es:</span><span class="sxs-lookup"><span data-stu-id="212c8-2464">the output is:</span></span>
```console
5
5
5
```

<span data-ttu-id="212c8-2465">Si un bucle for declara una variable de iteración, se considera que esa variable se declara fuera del bucle.</span><span class="sxs-lookup"><span data-stu-id="212c8-2465">If a for-loop declares an iteration variable, that variable itself is considered to be declared outside of the loop.</span></span> <span data-ttu-id="212c8-2466">Por lo tanto, si se cambia el ejemplo para capturar la variable de iteración en sí:</span><span class="sxs-lookup"><span data-stu-id="212c8-2466">Thus, if the example is changed to capture the iteration variable itself:</span></span>

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
<span data-ttu-id="212c8-2467">solo se captura una instancia de la variable de iteración, que genera el resultado:</span><span class="sxs-lookup"><span data-stu-id="212c8-2467">only one instance of the iteration variable is captured, which produces the output:</span></span>
```console
3
3
3
```

<span data-ttu-id="212c8-2468">Es posible que los delegados de función anónimos compartan algunas variables capturadas pero tengan instancias independientes de otras.</span><span class="sxs-lookup"><span data-stu-id="212c8-2468">It is possible for anonymous function delegates to share some captured variables yet have separate instances of others.</span></span> <span data-ttu-id="212c8-2469">Por ejemplo, si se cambia `F` a</span><span class="sxs-lookup"><span data-stu-id="212c8-2469">For example, if `F` is changed to</span></span>
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
<span data-ttu-id="212c8-2470">los tres delegados capturan la misma instancia de `x` pero instancias independientes de `y` y el resultado es:</span><span class="sxs-lookup"><span data-stu-id="212c8-2470">the three delegates capture the same instance of `x` but separate instances of `y`, and the output is:</span></span>
```console
1 1
2 1
3 1
```

<span data-ttu-id="212c8-2471">Las funciones anónimas independientes pueden capturar la misma instancia de una variable externa.</span><span class="sxs-lookup"><span data-stu-id="212c8-2471">Separate anonymous functions can capture the same instance of an outer variable.</span></span> <span data-ttu-id="212c8-2472">En el ejemplo:</span><span class="sxs-lookup"><span data-stu-id="212c8-2472">In the example:</span></span>
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
<span data-ttu-id="212c8-2473">las dos funciones anónimas capturan la misma instancia de la variable local `x` y, por lo tanto, pueden "comunicarse" a través de esa variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-2473">the two anonymous functions capture the same instance of the local variable `x`, and they can thus "communicate" through that variable.</span></span> <span data-ttu-id="212c8-2474">La salida del ejemplo es:</span><span class="sxs-lookup"><span data-stu-id="212c8-2474">The output of the example is:</span></span>
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a><span data-ttu-id="212c8-2475">Evaluación de expresiones de función anónimas</span><span class="sxs-lookup"><span data-stu-id="212c8-2475">Evaluation of anonymous function expressions</span></span>

<span data-ttu-id="212c8-2476">Una función anónima `F` siempre debe convertirse en un tipo de delegado `D` o en un tipo de árbol de expresión `E`, ya sea directamente o mediante la ejecución de una expresión de creación de delegado `new D(F)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2476">An anonymous function `F` must always be converted to a delegate type `D` or an expression tree type `E`, either directly or through the execution of a delegate creation expression `new D(F)`.</span></span> <span data-ttu-id="212c8-2477">Esta conversión determina el resultado de la función anónima, como se describe en [conversiones de funciones anónimas](conversions.md#anonymous-function-conversions).</span><span class="sxs-lookup"><span data-stu-id="212c8-2477">This conversion determines the result of the anonymous function, as described in [Anonymous function conversions](conversions.md#anonymous-function-conversions).</span></span>

## <a name="query-expressions"></a><span data-ttu-id="212c8-2478">Expresiones de consulta</span><span class="sxs-lookup"><span data-stu-id="212c8-2478">Query expressions</span></span>

<span data-ttu-id="212c8-2479">Las ***expresiones de consulta*** proporcionan una sintaxis integrada de lenguaje para las consultas que es similar a los lenguajes de consulta jerárquica y relacional, como SQL y XQuery.</span><span class="sxs-lookup"><span data-stu-id="212c8-2479">***Query expressions*** provide a language integrated syntax for queries that is similar to relational and hierarchical query languages such as SQL and XQuery.</span></span>

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

<span data-ttu-id="212c8-2480">Una expresión de consulta comienza con una cláusula `from` y termina con una cláusula `select` o `group`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2480">A query expression begins with a `from` clause and ends with either a `select` or `group` clause.</span></span> <span data-ttu-id="212c8-2481">La cláusula `from` inicial puede ir seguida de cero o más `from`, `let`, `where`, `join` o `orderby`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2481">The initial `from` clause can be followed by zero or more `from`, `let`, `where`, `join` or `orderby` clauses.</span></span> <span data-ttu-id="212c8-2482">Cada cláusula `from` es un generador que introduce una ***variable de rango*** que abarca los elementos de una ***secuencia***.</span><span class="sxs-lookup"><span data-stu-id="212c8-2482">Each `from` clause is a generator introducing a ***range variable*** which ranges over the elements of a ***sequence***.</span></span> <span data-ttu-id="212c8-2483">Cada cláusula `let` introduce una variable de rango que representa un valor calculado por medio de las variables de rango anteriores.</span><span class="sxs-lookup"><span data-stu-id="212c8-2483">Each `let` clause introduces a range variable representing a value computed by means of previous range variables.</span></span> <span data-ttu-id="212c8-2484">Cada cláusula `where` es un filtro que excluye elementos del resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2484">Each `where` clause is a filter that excludes items from the result.</span></span> <span data-ttu-id="212c8-2485">Cada cláusula `join` compara las claves especificadas de la secuencia de origen con claves de otra secuencia, lo que produce pares coincidentes.</span><span class="sxs-lookup"><span data-stu-id="212c8-2485">Each `join` clause compares specified keys of the source sequence with keys of another sequence, yielding matching pairs.</span></span> <span data-ttu-id="212c8-2486">Cada cláusula `orderby` reordena los elementos según los criterios especificados. La cláusula final `select` o `group` especifica la forma del resultado en términos de las variables de rango.</span><span class="sxs-lookup"><span data-stu-id="212c8-2486">Each `orderby` clause reorders items according to specified criteria.The final `select` or `group` clause specifies the shape of the result in terms of the range variables.</span></span> <span data-ttu-id="212c8-2487">Por último, se puede usar una cláusula `into` para "Insertar" consultas mediante el tratamiento de los resultados de una consulta como un generador en una consulta posterior.</span><span class="sxs-lookup"><span data-stu-id="212c8-2487">Finally, an `into` clause can be used to "splice" queries by treating the results of one query as a generator in a subsequent query.</span></span>

### <a name="ambiguities-in-query-expressions"></a><span data-ttu-id="212c8-2488">Ambigüedades en expresiones de consulta</span><span class="sxs-lookup"><span data-stu-id="212c8-2488">Ambiguities in query expressions</span></span>

<span data-ttu-id="212c8-2489">Las expresiones de consulta contienen una serie de "palabras clave contextuales", es decir, identificadores que tienen un significado especial en un contexto determinado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2489">Query expressions contain a number of "contextual keywords", i.e., identifiers that have special meaning in a given context.</span></span> <span data-ttu-id="212c8-2490">En concreto, se trata de `from`, `where`, @no__t 2, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, 0, 1 y 2.</span><span class="sxs-lookup"><span data-stu-id="212c8-2490">Specifically these are `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` and `by`.</span></span> <span data-ttu-id="212c8-2491">Para evitar ambigüedades en las expresiones de consulta producidas por el uso mixto de estos identificadores como palabras clave o nombres simples, estos identificadores se consideran palabras clave cuando se producen en cualquier parte de una expresión de consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2491">In order to avoid ambiguities in query expressions caused by mixed use of these identifiers as keywords or simple names, these identifiers are considered keywords when occurring anywhere within a query expression.</span></span>

<span data-ttu-id="212c8-2492">Para este propósito, una expresión de consulta es cualquier expresión que empiece por "`from identifier`" seguida de cualquier token excepto "`;`", "`=`" o "`,`".</span><span class="sxs-lookup"><span data-stu-id="212c8-2492">For this purpose, a query expression is any expression that starts with "`from identifier`" followed by any token except "`;`", "`=`" or "`,`".</span></span>

<span data-ttu-id="212c8-2493">Para usar estas palabras como identificadores dentro de una expresión de consulta, se les puede anteponer "`@`" ([identificadores](lexical-structure.md#identifiers)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2493">In order to use these words as identifiers within a query expression, they can be prefixed with "`@`" ([Identifiers](lexical-structure.md#identifiers)).</span></span>

### <a name="query-expression-translation"></a><span data-ttu-id="212c8-2494">Traducción de expresiones de consulta</span><span class="sxs-lookup"><span data-stu-id="212c8-2494">Query expression translation</span></span>

<span data-ttu-id="212c8-2495">El C# lenguaje no especifica la semántica de ejecución de las expresiones de consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2495">The C# language does not specify the execution semantics of query expressions.</span></span> <span data-ttu-id="212c8-2496">En su lugar, las expresiones de consulta se convierten en invocaciones de métodos que se adhieren al *patrón de expresión de consulta* ([el patrón de expresión de consulta](expressions.md#the-query-expression-pattern)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2496">Rather, query expressions are translated into invocations of methods that adhere to the *query expression pattern* ([The query expression pattern](expressions.md#the-query-expression-pattern)).</span></span> <span data-ttu-id="212c8-2497">En concreto, las expresiones de consulta se convierten en invocaciones de métodos denominados `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy` y 0. Se espera que estos métodos tengan firmas concretas y tipos de resultado, como se describe en [el patrón de expresión de consulta](expressions.md#the-query-expression-pattern).</span><span class="sxs-lookup"><span data-stu-id="212c8-2497">Specifically, query expressions are translated into invocations of methods named `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, and `Cast`.These methods are expected to have particular signatures and result types, as described in [The query expression pattern](expressions.md#the-query-expression-pattern).</span></span> <span data-ttu-id="212c8-2498">Estos métodos pueden ser métodos de instancia del objeto que se consulta o métodos de extensión que son externos al objeto e implementan la ejecución real de la consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2498">These methods can be instance methods of the object being queried or extension methods that are external to the object, and they implement the actual execution of the query.</span></span>

<span data-ttu-id="212c8-2499">La conversión de las expresiones de consulta a las invocaciones de método es una asignación sintáctica que se produce antes de que se haya realizado cualquier enlace de tipo o resolución de sobrecarga.</span><span class="sxs-lookup"><span data-stu-id="212c8-2499">The translation from query expressions to method invocations is a syntactic mapping that occurs before any type binding or overload resolution has been performed.</span></span> <span data-ttu-id="212c8-2500">Se garantiza que la conversión es sintácticamente correcta, pero no se garantiza que genere código correcto C# semánticamente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2500">The translation is guaranteed to be syntactically correct, but it is not guaranteed to produce semantically correct C# code.</span></span> <span data-ttu-id="212c8-2501">Después de la traducción de las expresiones de consulta, las invocaciones de método resultantes se procesan como invocaciones de método regulares y esto puede a su vez detectar errores, por ejemplo, si los métodos no existen, si los argumentos tienen tipos incorrectos, o si los métodos son genéricos y se produce un error en la inferencia de tipos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2501">Following translation of query expressions, the resulting method invocations are processed as regular method invocations, and this may in turn uncover errors, for example if the methods do not exist, if arguments have wrong types, or if the methods are generic and type inference fails.</span></span>

<span data-ttu-id="212c8-2502">Una expresión de consulta se procesa mediante la aplicación repetida de las siguientes traducciones hasta que no se puedan realizar más reducciones.</span><span class="sxs-lookup"><span data-stu-id="212c8-2502">A query expression is processed by repeatedly applying the following translations until no further reductions are possible.</span></span> <span data-ttu-id="212c8-2503">Las traducciones se muestran por orden de aplicación: cada sección presupone que las traducciones de las secciones anteriores se han realizado de forma exhaustiva y, una vez agotadas, una sección no se volverá a visitar posteriormente en el procesamiento de la misma expresión de consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2503">The translations are listed in order of application: each section assumes that the translations in the preceding sections have been performed exhaustively, and once exhausted, a section will not later be revisited in the processing of the same query expression.</span></span>

<span data-ttu-id="212c8-2504">No se permite la asignación a variables de rango en expresiones de consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2504">Assignment to range variables is not allowed in query expressions.</span></span> <span data-ttu-id="212c8-2505">Sin embargo C# , se permite que una implementación no siempre aplique esta restricción, ya que a veces esto no es posible con el esquema de traducción sintáctica que se muestra aquí.</span><span class="sxs-lookup"><span data-stu-id="212c8-2505">However a C# implementation is permitted to not always enforce this restriction, since this may sometimes not be possible with the syntactic translation scheme presented here.</span></span>

<span data-ttu-id="212c8-2506">Ciertas traducciones insertan variables de rango con identificadores transparentes indicados por `*`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2506">Certain translations inject range variables with transparent identifiers denoted by `*`.</span></span> <span data-ttu-id="212c8-2507">Las propiedades especiales de los identificadores transparentes se tratan en profundidad en los [identificadores transparentes](expressions.md#transparent-identifiers).</span><span class="sxs-lookup"><span data-stu-id="212c8-2507">The special properties of transparent identifiers are discussed further in [Transparent identifiers](expressions.md#transparent-identifiers).</span></span>

#### <a name="select-and-groupby-clauses-with-continuations"></a><span data-ttu-id="212c8-2508">Cláusulas SELECT y GroupBy con continuaciones</span><span class="sxs-lookup"><span data-stu-id="212c8-2508">Select and groupby clauses with continuations</span></span>

<span data-ttu-id="212c8-2509">Expresión de consulta con una continuación</span><span class="sxs-lookup"><span data-stu-id="212c8-2509">A query expression with a continuation</span></span>
```csharp
from ... into x ...
```
<span data-ttu-id="212c8-2510">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2510">is translated into</span></span>
```csharp
from x in ( from ... ) ...
```

<span data-ttu-id="212c8-2511">Las traducciones de las secciones siguientes suponen que las consultas no tienen ninguna continuación `into`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2511">The translations in the following sections assume that queries have no `into` continuations.</span></span>

<span data-ttu-id="212c8-2512">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2512">The example</span></span>
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="212c8-2513">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2513">is translated into</span></span>
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
<span data-ttu-id="212c8-2514">la traducción final que es</span><span class="sxs-lookup"><span data-stu-id="212c8-2514">the final translation of which is</span></span>
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a><span data-ttu-id="212c8-2515">Tipos de variables de rango explícitas</span><span class="sxs-lookup"><span data-stu-id="212c8-2515">Explicit range variable types</span></span>

<span data-ttu-id="212c8-2516">Una cláusula `from` que especifica explícitamente un tipo de variable de rango</span><span class="sxs-lookup"><span data-stu-id="212c8-2516">A `from` clause that explicitly specifies a range variable type</span></span>
```csharp
from T x in e
```
<span data-ttu-id="212c8-2517">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2517">is translated into</span></span>
```csharp
from x in ( e ) . Cast < T > ( )
```

<span data-ttu-id="212c8-2518">Una cláusula `join` que especifica explícitamente un tipo de variable de rango</span><span class="sxs-lookup"><span data-stu-id="212c8-2518">A `join` clause that explicitly specifies a range variable type</span></span>
```csharp
join T x in e on k1 equals k2
```
<span data-ttu-id="212c8-2519">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2519">is translated into</span></span>
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

<span data-ttu-id="212c8-2520">Las traducciones de las secciones siguientes suponen que las consultas no tienen tipos de variable de intervalo explícitos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2520">The translations in the following sections assume that queries have no explicit range variable types.</span></span>

<span data-ttu-id="212c8-2521">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2521">The example</span></span>
```csharp
from Customer c in customers
where c.City == "London"
select c
```
<span data-ttu-id="212c8-2522">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2522">is translated into</span></span>
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
<span data-ttu-id="212c8-2523">la traducción final que es</span><span class="sxs-lookup"><span data-stu-id="212c8-2523">the final translation of which is</span></span>
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

<span data-ttu-id="212c8-2524">Los tipos de variables de rango explícitos son útiles para consultar colecciones que implementan la interfaz `IEnumerable` no genérica, pero no la interfaz genérica de `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2524">Explicit range variable types are useful for querying collections that implement the non-generic `IEnumerable` interface, but not the generic `IEnumerable<T>` interface.</span></span> <span data-ttu-id="212c8-2525">En el ejemplo anterior, este sería el caso si `customers` fuera del tipo `ArrayList`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2525">In the example above, this would be the case if `customers` were of type `ArrayList`.</span></span>

#### <a name="degenerate-query-expressions"></a><span data-ttu-id="212c8-2526">Expresiones de consulta degeneradas</span><span class="sxs-lookup"><span data-stu-id="212c8-2526">Degenerate query expressions</span></span>

<span data-ttu-id="212c8-2527">Una expresión de consulta con el formato</span><span class="sxs-lookup"><span data-stu-id="212c8-2527">A query expression of the form</span></span>
```csharp
from x in e select x
```
<span data-ttu-id="212c8-2528">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2528">is translated into</span></span>
```csharp
( e ) . Select ( x => x )
```

<span data-ttu-id="212c8-2529">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2529">The example</span></span>
```csharp
from c in customers
select c
```
<span data-ttu-id="212c8-2530">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2530">is translated into</span></span>
```csharp
customers.Select(c => c)
```

<span data-ttu-id="212c8-2531">Una expresión de consulta degenerada es aquella que selecciona trivialmente los elementos del origen.</span><span class="sxs-lookup"><span data-stu-id="212c8-2531">A degenerate query expression is one that trivially selects the elements of the source.</span></span> <span data-ttu-id="212c8-2532">Una fase posterior de la traducción quita las consultas degeneradas que se introdujeron en otros pasos de traducción mediante su origen.</span><span class="sxs-lookup"><span data-stu-id="212c8-2532">A later phase of the translation removes degenerate queries introduced by other translation steps by replacing them with their source.</span></span> <span data-ttu-id="212c8-2533">Sin embargo, es importante asegurarse de que el resultado de una expresión de consulta nunca sea el propio objeto de origen, ya que esto revelaría el tipo e identidad del origen al cliente de la consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2533">It is important however to ensure that the result of a query expression is never the source object itself, as that would reveal the type and identity of the source to the client of the query.</span></span> <span data-ttu-id="212c8-2534">Por lo tanto, este paso protege las consultas degeneradas escritas directamente en el código fuente mediante una llamada explícita a `Select` en el origen.</span><span class="sxs-lookup"><span data-stu-id="212c8-2534">Therefore this step protects degenerate queries written directly in source code by explicitly calling `Select` on the source.</span></span> <span data-ttu-id="212c8-2535">A continuación, llega a los implementadores de `Select` y otros operadores de consulta para asegurarse de que estos métodos nunca devuelven el propio objeto de origen.</span><span class="sxs-lookup"><span data-stu-id="212c8-2535">It is then up to the implementers of `Select` and other query operators to ensure that these methods never return the source object itself.</span></span>

#### <a name="from-let-where-join-and-orderby-clauses"></a><span data-ttu-id="212c8-2536">Cláusulas from, Let, Where, join y OrderBy</span><span class="sxs-lookup"><span data-stu-id="212c8-2536">From, let, where, join and orderby clauses</span></span>

<span data-ttu-id="212c8-2537">Una expresión de consulta con una segunda cláusula `from` seguida de una cláusula `select`</span><span class="sxs-lookup"><span data-stu-id="212c8-2537">A query expression with a second `from` clause followed by a `select` clause</span></span>
```csharp
from x1 in e1
from x2 in e2
select v
```
<span data-ttu-id="212c8-2538">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2538">is translated into</span></span>
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="212c8-2539">Una expresión de consulta con una segunda cláusula `from` seguida de una cláusula `select`:</span><span class="sxs-lookup"><span data-stu-id="212c8-2539">A query expression with a second `from` clause followed by something other than a `select` clause:</span></span>

```csharp
from x1 in e1
from x2 in e2
...
```
<span data-ttu-id="212c8-2540">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2540">is translated into</span></span>
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

<span data-ttu-id="212c8-2541">Una expresión de consulta con una cláusula `let`</span><span class="sxs-lookup"><span data-stu-id="212c8-2541">A query expression with a `let` clause</span></span>
```csharp
from x in e
let y = f
...
```
<span data-ttu-id="212c8-2542">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2542">is translated into</span></span>
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

<span data-ttu-id="212c8-2543">Una expresión de consulta con una cláusula `where`</span><span class="sxs-lookup"><span data-stu-id="212c8-2543">A query expression with a `where` clause</span></span>
```csharp
from x in e
where f
...
```
<span data-ttu-id="212c8-2544">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2544">is translated into</span></span>
```csharp
from x in ( e ) . Where ( x => f )
...
```

<span data-ttu-id="212c8-2545">Una expresión de consulta con una cláusula `join` sin un `into` seguida de una cláusula `select`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2545">A query expression with a `join` clause without an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
<span data-ttu-id="212c8-2546">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2546">is translated into</span></span>
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

<span data-ttu-id="212c8-2547">Una expresión de consulta con una cláusula `join` sin un `into` seguido de un valor distinto de una cláusula `select`</span><span class="sxs-lookup"><span data-stu-id="212c8-2547">A query expression with a `join` clause without an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
<span data-ttu-id="212c8-2548">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2548">is translated into</span></span>
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

<span data-ttu-id="212c8-2549">Una expresión de consulta con una cláusula `join` con un `into` seguida de una cláusula `select`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2549">A query expression with a `join` clause with an `into` followed by a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
<span data-ttu-id="212c8-2550">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2550">is translated into</span></span>
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

<span data-ttu-id="212c8-2551">Una expresión de consulta con una cláusula `join` con un `into` seguido de un valor distinto de una cláusula `select`</span><span class="sxs-lookup"><span data-stu-id="212c8-2551">A query expression with a `join` clause with an `into` followed by something other than a `select` clause</span></span>
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
<span data-ttu-id="212c8-2552">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2552">is translated into</span></span>
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

<span data-ttu-id="212c8-2553">Una expresión de consulta con una cláusula `orderby`</span><span class="sxs-lookup"><span data-stu-id="212c8-2553">A query expression with an `orderby` clause</span></span>
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
<span data-ttu-id="212c8-2554">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2554">is translated into</span></span>
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

<span data-ttu-id="212c8-2555">Si una cláusula de ordenación especifica un indicador de dirección `descending`, se produce una invocación de `OrderByDescending` o `ThenByDescending` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="212c8-2555">If an ordering clause specifies a `descending` direction indicator, an invocation of `OrderByDescending` or `ThenByDescending` is produced instead.</span></span>

<span data-ttu-id="212c8-2556">Las siguientes traducciones suponen que no hay ninguna cláusula `let`, `where`, `join` o `orderby`, y no más de una cláusula de `from` inicial en cada expresión de consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2556">The following translations assume that there are no `let`, `where`, `join` or `orderby` clauses, and no more than the one initial `from` clause in each query expression.</span></span>

<span data-ttu-id="212c8-2557">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2557">The example</span></span>
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="212c8-2558">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2558">is translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

<span data-ttu-id="212c8-2559">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2559">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="212c8-2560">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2560">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
<span data-ttu-id="212c8-2561">la traducción final que es</span><span class="sxs-lookup"><span data-stu-id="212c8-2561">the final translation of which is</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
<span data-ttu-id="212c8-2562">donde `x` es un identificador generado por el compilador que, de lo contrario, es invisible e inaccesible.</span><span class="sxs-lookup"><span data-stu-id="212c8-2562">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="212c8-2563">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2563">The example</span></span>
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
<span data-ttu-id="212c8-2564">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2564">is translated into</span></span>
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
<span data-ttu-id="212c8-2565">la traducción final que es</span><span class="sxs-lookup"><span data-stu-id="212c8-2565">the final translation of which is</span></span>
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
<span data-ttu-id="212c8-2566">donde `x` es un identificador generado por el compilador que, de lo contrario, es invisible e inaccesible.</span><span class="sxs-lookup"><span data-stu-id="212c8-2566">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="212c8-2567">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2567">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
<span data-ttu-id="212c8-2568">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2568">is translated into</span></span>
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

<span data-ttu-id="212c8-2569">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2569">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="212c8-2570">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2570">is translated into</span></span>
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
<span data-ttu-id="212c8-2571">la traducción final que es</span><span class="sxs-lookup"><span data-stu-id="212c8-2571">the final translation of which is</span></span>
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
<span data-ttu-id="212c8-2572">donde `x` y `y` son identificadores generados por el compilador que, de lo contrario, son invisibles e inaccesibles.</span><span class="sxs-lookup"><span data-stu-id="212c8-2572">where `x` and `y` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="212c8-2573">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2573">The example</span></span>
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
<span data-ttu-id="212c8-2574">tiene la traducción final</span><span class="sxs-lookup"><span data-stu-id="212c8-2574">has the final translation</span></span>
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a><span data-ttu-id="212c8-2575">Cláusulas Select</span><span class="sxs-lookup"><span data-stu-id="212c8-2575">Select clauses</span></span>

<span data-ttu-id="212c8-2576">Una expresión de consulta con el formato</span><span class="sxs-lookup"><span data-stu-id="212c8-2576">A query expression of the form</span></span>
```csharp
from x in e select v
```
<span data-ttu-id="212c8-2577">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2577">is translated into</span></span>
```csharp
( e ) . Select ( x => v )
```
<span data-ttu-id="212c8-2578">excepto cuando v es el identificador x, la traducción es simplemente</span><span class="sxs-lookup"><span data-stu-id="212c8-2578">except when v is the identifier x, the translation is simply</span></span>
```csharp
( e )
```

<span data-ttu-id="212c8-2579">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2579">For example</span></span>
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
<span data-ttu-id="212c8-2580">simplemente se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2580">is simply translated into</span></span>
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a><span data-ttu-id="212c8-2581">Cláusulas GroupBy</span><span class="sxs-lookup"><span data-stu-id="212c8-2581">Groupby clauses</span></span>

<span data-ttu-id="212c8-2582">Una expresión de consulta con el formato</span><span class="sxs-lookup"><span data-stu-id="212c8-2582">A query expression of the form</span></span>
```csharp
from x in e group v by k
```
<span data-ttu-id="212c8-2583">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2583">is translated into</span></span>
```csharp
( e ) . GroupBy ( x => k , x => v )
```
<span data-ttu-id="212c8-2584">excepto cuando v es el identificador x, la traducción es</span><span class="sxs-lookup"><span data-stu-id="212c8-2584">except when v is the identifier x, the translation is</span></span>
```csharp
( e ) . GroupBy ( x => k )
```

<span data-ttu-id="212c8-2585">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2585">The example</span></span>
```csharp
from c in customers
group c.Name by c.Country
```
<span data-ttu-id="212c8-2586">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2586">is translated into</span></span>
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a><span data-ttu-id="212c8-2587">Identificadores transparentes</span><span class="sxs-lookup"><span data-stu-id="212c8-2587">Transparent identifiers</span></span>

<span data-ttu-id="212c8-2588">Ciertas traducciones insertan variables de rango con ***identificadores transparentes*** indicados por `*`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2588">Certain translations inject range variables with ***transparent identifiers*** denoted by `*`.</span></span> <span data-ttu-id="212c8-2589">Los identificadores transparentes no son una característica de lenguaje adecuada; solo existen como un paso intermedio en el proceso de conversión de expresiones de consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2589">Transparent identifiers are not a proper language feature; they exist only as an intermediate step in the query expression translation process.</span></span>

<span data-ttu-id="212c8-2590">Cuando una traducción de consultas inserta un identificador transparente, los pasos de traducción adicionales propagan el identificador transparente en funciones anónimas e inicializadores de objeto anónimos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2590">When a query translation injects a transparent identifier, further translation steps propagate the transparent identifier into anonymous functions and anonymous object initializers.</span></span> <span data-ttu-id="212c8-2591">En esos contextos, los identificadores transparentes tienen el siguiente comportamiento:</span><span class="sxs-lookup"><span data-stu-id="212c8-2591">In those contexts, transparent identifiers have the following behavior:</span></span>

*  <span data-ttu-id="212c8-2592">Cuando un identificador transparente se produce como un parámetro en una función anónima, los miembros del tipo anónimo asociado están automáticamente en el ámbito del cuerpo de la función anónima.</span><span class="sxs-lookup"><span data-stu-id="212c8-2592">When a transparent identifier occurs as a parameter in an anonymous function, the members of the associated anonymous type are automatically in scope in the body of the anonymous function.</span></span>
*  <span data-ttu-id="212c8-2593">Cuando un miembro con un identificador transparente está en el ámbito, los miembros de ese miembro están también en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="212c8-2593">When a member with a transparent identifier is in scope, the members of that member are in scope as well.</span></span>
*  <span data-ttu-id="212c8-2594">Cuando un identificador transparente se produce como un declarador de miembro en un inicializador de objeto anónimo, introduce un miembro con un identificador transparente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2594">When a transparent identifier occurs as a member declarator in an anonymous object initializer, it introduces a member with a transparent identifier.</span></span>
*  <span data-ttu-id="212c8-2595">En los pasos de traducción descritos anteriormente, los identificadores transparentes siempre se introducen junto con los tipos anónimos, con la intención de capturar varias variables de rango como miembros de un solo objeto.</span><span class="sxs-lookup"><span data-stu-id="212c8-2595">In the translation steps described above, transparent identifiers are always introduced together with anonymous types, with the intent of capturing multiple range variables as members of a single object.</span></span> <span data-ttu-id="212c8-2596">Una implementación de C# puede utilizar un mecanismo diferente que los tipos anónimos para agrupar varias variables de rango.</span><span class="sxs-lookup"><span data-stu-id="212c8-2596">An implementation of C# is permitted to use a different mechanism than anonymous types to group together multiple range variables.</span></span> <span data-ttu-id="212c8-2597">Los siguientes ejemplos de traducción suponen que se usan tipos anónimos y muestran cómo se pueden traducir los identificadores transparentes.</span><span class="sxs-lookup"><span data-stu-id="212c8-2597">The following translation examples assume that anonymous types are used, and show how transparent identifiers can be translated away.</span></span>

<span data-ttu-id="212c8-2598">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2598">The example</span></span>
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
<span data-ttu-id="212c8-2599">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2599">is translated into</span></span>
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

<span data-ttu-id="212c8-2600">que se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2600">which is further translated into</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
<span data-ttu-id="212c8-2601">que, cuando se borran los identificadores transparentes, es equivalente a</span><span class="sxs-lookup"><span data-stu-id="212c8-2601">which, when transparent identifiers are erased, is equivalent to</span></span>
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
<span data-ttu-id="212c8-2602">donde `x` es un identificador generado por el compilador que, de lo contrario, es invisible e inaccesible.</span><span class="sxs-lookup"><span data-stu-id="212c8-2602">where `x` is a compiler generated identifier that is otherwise invisible and inaccessible.</span></span>

<span data-ttu-id="212c8-2603">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2603">The example</span></span>
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="212c8-2604">se traduce en</span><span class="sxs-lookup"><span data-stu-id="212c8-2604">is translated into</span></span>
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
<span data-ttu-id="212c8-2605">se reduce aún más a</span><span class="sxs-lookup"><span data-stu-id="212c8-2605">which is further reduced to</span></span>
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
<span data-ttu-id="212c8-2606">la traducción final que es</span><span class="sxs-lookup"><span data-stu-id="212c8-2606">the final translation of which is</span></span>
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
<span data-ttu-id="212c8-2607">donde `x`, `y` y `z` son identificadores generados por el compilador que, de lo contrario, son invisibles e inaccesibles.</span><span class="sxs-lookup"><span data-stu-id="212c8-2607">where `x`, `y`, and `z` are compiler generated identifiers that are otherwise invisible and inaccessible.</span></span>

### <a name="the-query-expression-pattern"></a><span data-ttu-id="212c8-2608">Patrón de expresión de consulta</span><span class="sxs-lookup"><span data-stu-id="212c8-2608">The query expression pattern</span></span>

<span data-ttu-id="212c8-2609">El ***patrón de expresión de consulta*** establece un patrón de métodos que los tipos pueden implementar para admitir expresiones de consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2609">The ***Query expression pattern*** establishes a pattern of methods that types can implement to support query expressions.</span></span> <span data-ttu-id="212c8-2610">Dado que las expresiones de consulta se convierten en invocaciones de método por medio de una asignación sintáctica, los tipos tienen una gran flexibilidad en cómo implementan el patrón de expresión de consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2610">Because query expressions are translated to method invocations by means of a syntactic mapping, types have considerable flexibility in how they implement the query expression pattern.</span></span> <span data-ttu-id="212c8-2611">Por ejemplo, los métodos del patrón se pueden implementar como métodos de instancia o como métodos de extensión porque los dos tienen la misma sintaxis de invocación y los métodos pueden solicitar delegados o árboles de expresión porque las funciones anónimas son convertibles a ambos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2611">For example, the methods of the pattern can be implemented as instance methods or as extension methods because the two have the same invocation syntax, and the methods can request delegates or expression trees because anonymous functions are convertible to both.</span></span>

<span data-ttu-id="212c8-2612">A continuación se muestra la forma recomendada de un tipo genérico `C<T>` que admite el patrón de expresión de consulta.</span><span class="sxs-lookup"><span data-stu-id="212c8-2612">The recommended shape of a generic type `C<T>` that supports the query expression pattern is shown below.</span></span> <span data-ttu-id="212c8-2613">Se usa un tipo genérico para ilustrar las relaciones apropiadas entre los tipos de parámetro y de resultado, pero también es posible implementar el patrón para tipos no genéricos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2613">A generic type is used in order to illustrate the proper relationships between parameter and result types, but it is possible to implement the pattern for non-generic types as well.</span></span>

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

<span data-ttu-id="212c8-2614">Los métodos anteriores usan los tipos de delegado genérico `Func<T1,R>` y `Func<T1,T2,R>`, pero también podrían haber usado otros tipos de árbol de delegado o de expresión con las mismas relaciones en los tipos de parámetro y de resultado.</span><span class="sxs-lookup"><span data-stu-id="212c8-2614">The methods above use the generic delegate types `Func<T1,R>` and `Func<T1,T2,R>`, but they could equally well have used other delegate or expression tree types with the same relationships in parameter and result types.</span></span>

<span data-ttu-id="212c8-2615">Observe la relación recomendada entre `C<T>` y `O<T>`, lo que garantiza que los métodos @no__t 2 y `ThenByDescending` solo están disponibles en el resultado de `OrderBy` o `OrderByDescending`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2615">Notice the recommended relationship between `C<T>` and `O<T>` which ensures that the `ThenBy` and `ThenByDescending` methods are available only on the result of an `OrderBy` or `OrderByDescending`.</span></span> <span data-ttu-id="212c8-2616">Observe también la forma recomendada del resultado de `GroupBy`--una secuencia de secuencias, donde cada secuencia interna tiene una propiedad `Key` adicional.</span><span class="sxs-lookup"><span data-stu-id="212c8-2616">Also notice the recommended shape of the result of `GroupBy` -- a sequence of sequences, where each inner sequence has an additional `Key` property.</span></span>

<span data-ttu-id="212c8-2617">El espacio de nombres `System.Linq` proporciona una implementación del patrón de operador de consulta para cualquier tipo que implemente la interfaz `System.Collections.Generic.IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2617">The `System.Linq` namespace provides an implementation of the query operator pattern for any type that implements the `System.Collections.Generic.IEnumerable<T>` interface.</span></span>

## <a name="assignment-operators"></a><span data-ttu-id="212c8-2618">Operadores de asignación</span><span class="sxs-lookup"><span data-stu-id="212c8-2618">Assignment operators</span></span>

<span data-ttu-id="212c8-2619">Los operadores de asignación asignan un nuevo valor a una variable, una propiedad, un evento o un elemento de indexador.</span><span class="sxs-lookup"><span data-stu-id="212c8-2619">The assignment operators assign a new value to a variable, a property, an event, or an indexer element.</span></span>

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

<span data-ttu-id="212c8-2620">El operando izquierdo de una asignación debe ser una expresión clasificada como una variable, un acceso de propiedad, un acceso de indexador o un acceso de evento.</span><span class="sxs-lookup"><span data-stu-id="212c8-2620">The left operand of an assignment must be an expression classified as a variable, a property access, an indexer access, or an event access.</span></span>

<span data-ttu-id="212c8-2621">El operador `=` se denomina ***operador de asignación simple***.</span><span class="sxs-lookup"><span data-stu-id="212c8-2621">The `=` operator is called the ***simple assignment operator***.</span></span> <span data-ttu-id="212c8-2622">Asigna el valor del operando derecho a la variable, propiedad o elemento de indexador proporcionado por el operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2622">It assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="212c8-2623">Es posible que el operando izquierdo del operador de asignación simple no sea un acceso a eventos (excepto como se describe en [eventos similares a los de campo](classes.md#field-like-events)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2623">The left operand of the simple assignment operator may not be an event access (except as described in [Field-like events](classes.md#field-like-events)).</span></span> <span data-ttu-id="212c8-2624">El operador de asignación simple se describe en [asignación simple](expressions.md#simple-assignment).</span><span class="sxs-lookup"><span data-stu-id="212c8-2624">The simple assignment operator is described in [Simple assignment](expressions.md#simple-assignment).</span></span>

<span data-ttu-id="212c8-2625">Los operadores de asignación distintos del operador `=` se denominan ***operadores de asignación compuesta***.</span><span class="sxs-lookup"><span data-stu-id="212c8-2625">The assignment operators other than the `=` operator are called the ***compound assignment operators***.</span></span> <span data-ttu-id="212c8-2626">Estos operadores realizan la operación indicada en los dos operandos y, a continuación, asignan el valor resultante a la variable, la propiedad o el elemento indexador proporcionado por el operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2626">These operators perform the indicated operation on the two operands, and then assign the resulting value to the variable, property, or indexer element given by the left operand.</span></span> <span data-ttu-id="212c8-2627">Los operadores de asignación compuesta se describen en [asignación compuesta](expressions.md#compound-assignment).</span><span class="sxs-lookup"><span data-stu-id="212c8-2627">The compound assignment operators are described in [Compound assignment](expressions.md#compound-assignment).</span></span>

<span data-ttu-id="212c8-2628">Los operadores `+=` y `-=` con una expresión de acceso a eventos como operando izquierdo se denominan *operadores de asignación de eventos*.</span><span class="sxs-lookup"><span data-stu-id="212c8-2628">The `+=` and `-=` operators with an event access expression as the left operand are called the *event assignment operators*.</span></span> <span data-ttu-id="212c8-2629">Ningún otro operador de asignación es válido con un acceso de evento como operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2629">No other assignment operator is valid with an event access as the left operand.</span></span> <span data-ttu-id="212c8-2630">Los operadores de asignación de eventos se describen en [asignación de eventos](expressions.md#event-assignment).</span><span class="sxs-lookup"><span data-stu-id="212c8-2630">The event assignment operators are described in [Event assignment](expressions.md#event-assignment).</span></span>

<span data-ttu-id="212c8-2631">Los operadores de asignación son asociativos a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda.</span><span class="sxs-lookup"><span data-stu-id="212c8-2631">The assignment operators are right-associative, meaning that operations are grouped from right to left.</span></span> <span data-ttu-id="212c8-2632">Por ejemplo, una expresión con el formato `a = b = c` se evalúa como `a = (b = c)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2632">For example, an expression of the form `a = b = c` is evaluated as `a = (b = c)`.</span></span>

### <a name="simple-assignment"></a><span data-ttu-id="212c8-2633">Asignación simple</span><span class="sxs-lookup"><span data-stu-id="212c8-2633">Simple assignment</span></span>

<span data-ttu-id="212c8-2634">El operador `=` se denomina operador de asignación simple.</span><span class="sxs-lookup"><span data-stu-id="212c8-2634">The `=` operator is called the simple assignment operator.</span></span>

<span data-ttu-id="212c8-2635">Si el operando izquierdo de una asignación simple tiene el formato `E.P` o `E[Ei]`, donde @no__t 2 tiene el tipo en tiempo de compilación `dynamic`, la asignación está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2635">If the left operand of a simple assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="212c8-2636">En este caso, el tipo en tiempo de compilación de la expresión de asignación es `dynamic`, y la resolución que se describe a continuación se realizará en tiempo de ejecución en función del tipo en tiempo de ejecución de `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2636">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="212c8-2637">En una asignación simple, el operando derecho debe ser una expresión que se pueda convertir implícitamente al tipo del operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2637">In a simple assignment, the right operand must be an expression that is implicitly convertible to the type of the left operand.</span></span> <span data-ttu-id="212c8-2638">La operación asigna el valor del operando derecho a la variable, la propiedad o el elemento indexador proporcionado por el operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2638">The operation assigns the value of the right operand to the variable, property, or indexer element given by the left operand.</span></span>

<span data-ttu-id="212c8-2639">El resultado de una expresión de asignación simple es el valor asignado al operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2639">The result of a simple assignment expression is the value assigned to the left operand.</span></span> <span data-ttu-id="212c8-2640">El resultado tiene el mismo tipo que el operando izquierdo y siempre se clasifica como un valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-2640">The result has the same type as the left operand and is always classified as a value.</span></span>

<span data-ttu-id="212c8-2641">Si el operando izquierdo es una propiedad o un indexador, la propiedad o el indexador deben tener un descriptor de acceso `set`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2641">If the left operand is a property or indexer access, the property or indexer must have a `set` accessor.</span></span> <span data-ttu-id="212c8-2642">Si no es así, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-2642">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-2643">El procesamiento en tiempo de ejecución de una asignación simple del formulario `x = y` consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="212c8-2643">The run-time processing of a simple assignment of the form `x = y` consists of the following steps:</span></span>

*  <span data-ttu-id="212c8-2644">Si `x` está clasificado como una variable:</span><span class="sxs-lookup"><span data-stu-id="212c8-2644">If `x` is classified as a variable:</span></span>
   * <span data-ttu-id="212c8-2645">`x` se evalúa para generar la variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-2645">`x` is evaluated to produce the variable.</span></span>
   * <span data-ttu-id="212c8-2646">`y` se evalúa y, si es necesario, se convierte al tipo de `x` a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2646">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="212c8-2647">Si la variable proporcionada por `x` es un elemento de matriz de un *reference_type*, se realiza una comprobación en tiempo de ejecución para asegurarse de que el valor calculado para `y` es compatible con la instancia de matriz de la que `x` es un elemento.</span><span class="sxs-lookup"><span data-stu-id="212c8-2647">If the variable given by `x` is an array element of a *reference_type*, a run-time check is performed to ensure that the value computed for `y` is compatible with the array instance of which `x` is an element.</span></span> <span data-ttu-id="212c8-2648">La comprobación se realiza correctamente si `y` es `null`, o si existe una conversión de referencia implícita ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions)) del tipo real de la instancia a la que hace referencia `y` en el tipo de elemento real de la instancia de matriz que contiene `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2648">The check succeeds if `y` is `null`, or if an implicit reference conversion ([Implicit reference conversions](conversions.md#implicit-reference-conversions)) exists from the actual type of the instance referenced by `y` to the actual element type of the array instance containing `x`.</span></span> <span data-ttu-id="212c8-2649">De lo contrario, se produce una excepción `System.ArrayTypeMismatchException`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2649">Otherwise, a `System.ArrayTypeMismatchException` is thrown.</span></span>
   * <span data-ttu-id="212c8-2650">El valor resultante de la evaluación y conversión de `y` se almacena en la ubicación proporcionada por la evaluación de `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2650">The value resulting from the evaluation and conversion of `y` is stored into the location given by the evaluation of `x`.</span></span>
*  <span data-ttu-id="212c8-2651">Si `x` se clasifica como un acceso de propiedad o indizador:</span><span class="sxs-lookup"><span data-stu-id="212c8-2651">If `x` is classified as a property or indexer access:</span></span>
   * <span data-ttu-id="212c8-2652">La expresión de instancia (si `x` no es `static`) y la lista de argumentos (si `x` es un indexador) asociada a `x` se evalúan y los resultados se usan en la invocación del descriptor de acceso `set` subsiguiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2652">The instance expression (if `x` is not `static`) and the argument list (if `x` is an indexer access) associated with `x` are evaluated, and the results are used in the subsequent `set` accessor invocation.</span></span>
   * <span data-ttu-id="212c8-2653">`y` se evalúa y, si es necesario, se convierte al tipo de `x` a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2653">`y` is evaluated and, if required, converted to the type of `x` through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
   * <span data-ttu-id="212c8-2654">El descriptor de acceso `set` de `x` se invoca con el valor calculado para `y` como su argumento `value`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2654">The `set` accessor of `x` is invoked with the value computed for `y` as its `value` argument.</span></span>

<span data-ttu-id="212c8-2655">Las reglas de covarianza de matriz ([covarianza de matriz](arrays.md#array-covariance)) permiten que un valor de un tipo de matriz `A[]` sea una referencia a una instancia de un tipo de matriz `B[]`, siempre que exista una conversión de referencia implícita de `B` a `A`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2655">The array co-variance rules ([Array covariance](arrays.md#array-covariance)) permit a value of an array type `A[]` to be a reference to an instance of an array type `B[]`, provided an implicit reference conversion exists from `B` to `A`.</span></span> <span data-ttu-id="212c8-2656">Debido a estas reglas, la asignación a un elemento de matriz de una *reference_type* requiere una comprobación en tiempo de ejecución para asegurarse de que el valor que se está asignando es compatible con la instancia de la matriz.</span><span class="sxs-lookup"><span data-stu-id="212c8-2656">Because of these rules, assignment to an array element of a *reference_type* requires a run-time check to ensure that the value being assigned is compatible with the array instance.</span></span> <span data-ttu-id="212c8-2657">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2657">In the example</span></span>
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
<span data-ttu-id="212c8-2658">la última asignación provoca que se inicie una `System.ArrayTypeMismatchException` porque una instancia de `ArrayList` no puede almacenarse en un elemento de un `string[]`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2658">the last assignment causes a `System.ArrayTypeMismatchException` to be thrown because an instance of `ArrayList` cannot be stored in an element of a `string[]`.</span></span>

<span data-ttu-id="212c8-2659">Cuando una propiedad o un indizador declarado en una *struct_type* es el destino de una asignación, la expresión de instancia asociada con el acceso a la propiedad o indizador debe estar clasificada como una variable.</span><span class="sxs-lookup"><span data-stu-id="212c8-2659">When a property or indexer declared in a *struct_type* is the target of an assignment, the instance expression associated with the property or indexer access must be classified as a variable.</span></span> <span data-ttu-id="212c8-2660">Si la expresión de instancia se clasifica como un valor, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-2660">If the instance expression is classified as a value, a binding-time error occurs.</span></span> <span data-ttu-id="212c8-2661">Debido al [acceso a miembros](expressions.md#member-access), la misma regla también se aplica a los campos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2661">Because of [Member access](expressions.md#member-access), the same rule also applies to fields.</span></span>

<span data-ttu-id="212c8-2662">Dadas las declaraciones:</span><span class="sxs-lookup"><span data-stu-id="212c8-2662">Given the declarations:</span></span>
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
<span data-ttu-id="212c8-2663">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2663">in the example</span></span>
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
<span data-ttu-id="212c8-2664">se permiten las asignaciones a `p.X`, `p.Y`, `r.A` y `r.B` porque `p` y `r` son variables.</span><span class="sxs-lookup"><span data-stu-id="212c8-2664">the assignments to `p.X`, `p.Y`, `r.A`, and `r.B` are permitted because `p` and `r` are variables.</span></span> <span data-ttu-id="212c8-2665">Sin embargo, en el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2665">However, in the example</span></span>
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
<span data-ttu-id="212c8-2666">las asignaciones no son válidas, ya que `r.A` y `r.B` no son variables.</span><span class="sxs-lookup"><span data-stu-id="212c8-2666">the assignments are all invalid, since `r.A` and `r.B` are not variables.</span></span>

### <a name="compound-assignment"></a><span data-ttu-id="212c8-2667">Asignación compuesta</span><span class="sxs-lookup"><span data-stu-id="212c8-2667">Compound assignment</span></span>

<span data-ttu-id="212c8-2668">Si el operando izquierdo de una asignación compuesta tiene el formato `E.P` o `E[Ei]`, donde @no__t 2 tiene el tipo en tiempo de compilación `dynamic`, la asignación está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2668">If the left operand of a compound assignment is of the form `E.P` or `E[Ei]` where `E` has the compile-time type `dynamic`, then the assignment is dynamically bound ([Dynamic binding](expressions.md#dynamic-binding)).</span></span> <span data-ttu-id="212c8-2669">En este caso, el tipo en tiempo de compilación de la expresión de asignación es `dynamic`, y la resolución que se describe a continuación se realizará en tiempo de ejecución en función del tipo en tiempo de ejecución de `E`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2669">In this case the compile-time type of the assignment expression is `dynamic`, and the resolution described below will take place at run-time based on the run-time type of `E`.</span></span>

<span data-ttu-id="212c8-2670">Una operación con el formato `x op= y` se procesa aplicando la resolución de sobrecarga del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) como si la operación se hubiera escrito `x op y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2670">An operation of the form `x op= y` is processed by applying binary operator overload resolution ([Binary operator overload resolution](expressions.md#binary-operator-overload-resolution)) as if the operation was written `x op y`.</span></span> <span data-ttu-id="212c8-2671">A</span><span class="sxs-lookup"><span data-stu-id="212c8-2671">Then,</span></span>

*  <span data-ttu-id="212c8-2672">Si el tipo de valor devuelto del operador seleccionado es implícitamente convertible al tipo de `x`, la operación se evalúa como `x = x op y`, excepto en que `x` solo se evalúa una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-2672">If the return type of the selected operator is implicitly convertible to the type of `x`, the operation is evaluated as `x = x op y`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="212c8-2673">De lo contrario, si el operador seleccionado es un operador predefinido, si el tipo de valor devuelto del operador seleccionado es convertible explícitamente al tipo de `x`, y si `y` es implícitamente convertible al tipo de `x` o el operador es un operador de desplazamiento , la operación se evalúa como `x = (T)(x op y)`, donde `T` es el tipo de `x`, salvo que `x` se evalúa solo una vez.</span><span class="sxs-lookup"><span data-stu-id="212c8-2673">Otherwise, if the selected operator is a predefined operator, if the return type of the selected operator is explicitly convertible to the type of `x`, and if `y` is implicitly convertible to the type of `x` or the operator is a shift operator, then the operation is evaluated as `x = (T)(x op y)`, where `T` is the type of `x`, except that `x` is evaluated only once.</span></span>
*  <span data-ttu-id="212c8-2674">De lo contrario, la asignación compuesta no es válida y se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-2674">Otherwise, the compound assignment is invalid, and a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-2675">El término "evaluado solo una vez" significa que en la evaluación de `x op y`, los resultados de cualquier expresión constitutiva de `x` se guardan temporalmente y se reutilizan al realizar la asignación a `x`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2675">The term "evaluated only once" means that in the evaluation of `x op y`, the results of any constituent expressions of `x` are temporarily saved and then reused when performing the assignment to `x`.</span></span> <span data-ttu-id="212c8-2676">Por ejemplo, en la asignación `A()[B()] += C()`, donde `A` es un método que devuelve `int[]` y `B` y `C` son métodos que devuelven `int`, los métodos se invocan solo una vez, en el orden `A`, `B`, `C`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2676">For example, in the assignment `A()[B()] += C()`, where `A` is a method returning `int[]`, and `B` and `C` are methods returning `int`, the methods are invoked only once, in the order `A`, `B`, `C`.</span></span>

<span data-ttu-id="212c8-2677">Cuando el operando izquierdo de una asignación compuesta es un acceso a propiedad o a un indexador, la propiedad o el indexador deben tener un descriptor de acceso `get` y un descriptor de acceso `set`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2677">When the left operand of a compound assignment is a property access or indexer access, the property or indexer must have both a `get` accessor and a `set` accessor.</span></span> <span data-ttu-id="212c8-2678">Si no es así, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-2678">If this is not the case, a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-2679">La segunda regla anterior permite evaluar `x op= y` como `x = (T)(x op y)` en determinados contextos.</span><span class="sxs-lookup"><span data-stu-id="212c8-2679">The second rule above permits `x op= y` to be evaluated as `x = (T)(x op y)` in certain contexts.</span></span> <span data-ttu-id="212c8-2680">La regla existe de forma que los operadores predefinidos se pueden usar como operadores compuestos cuando el operando izquierdo es de tipo `sbyte`, `byte`, `short`, `ushort` o `char`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2680">The rule exists such that the predefined operators can be used as compound operators when the left operand is of type `sbyte`, `byte`, `short`, `ushort`, or `char`.</span></span> <span data-ttu-id="212c8-2681">Incluso cuando ambos argumentos son de uno de esos tipos, los operadores predefinidos producen un resultado de tipo `int`, tal y como se describe en [promociones numéricas binarias](expressions.md#binary-numeric-promotions).</span><span class="sxs-lookup"><span data-stu-id="212c8-2681">Even when both arguments are of one of those types, the predefined operators produce a result of type `int`, as described in [Binary numeric promotions](expressions.md#binary-numeric-promotions).</span></span> <span data-ttu-id="212c8-2682">Por lo tanto, sin una conversión, no sería posible asignar el resultado al operando izquierdo.</span><span class="sxs-lookup"><span data-stu-id="212c8-2682">Thus, without a cast it would not be possible to assign the result to the left operand.</span></span>

<span data-ttu-id="212c8-2683">El efecto intuitivo de la regla para los operadores predefinidos es simplemente que se permite `x op= y` si se permiten los dos `x op y` y `x = y`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2683">The intuitive effect of the rule for predefined operators is simply that `x op= y` is permitted if both of `x op y` and `x = y` are permitted.</span></span> <span data-ttu-id="212c8-2684">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2684">In the example</span></span>
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
<span data-ttu-id="212c8-2685">la razón intuitiva de cada error es que una asignación simple correspondiente también habría sido un error.</span><span class="sxs-lookup"><span data-stu-id="212c8-2685">the intuitive reason for each error is that a corresponding simple assignment would also have been an error.</span></span>

<span data-ttu-id="212c8-2686">Esto también significa que las operaciones de asignación compuesta admiten operaciones de elevación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2686">This also means that compound assignment operations support lifted operations.</span></span> <span data-ttu-id="212c8-2687">En el ejemplo</span><span class="sxs-lookup"><span data-stu-id="212c8-2687">In the example</span></span>
```csharp
int? i = 0;
i += 1;             // Ok
```
<span data-ttu-id="212c8-2688">se usa el operador de elevación `+(int?,int?)`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2688">the lifted operator `+(int?,int?)` is used.</span></span>

### <a name="event-assignment"></a><span data-ttu-id="212c8-2689">Asignación de eventos</span><span class="sxs-lookup"><span data-stu-id="212c8-2689">Event assignment</span></span>

<span data-ttu-id="212c8-2690">Si el operando izquierdo de un operador `+=` o `-=` se clasifica como un acceso de evento, la expresión se evalúa como sigue:</span><span class="sxs-lookup"><span data-stu-id="212c8-2690">If the left operand of a `+=` or `-=` operator is classified as an event access, then the expression is evaluated as follows:</span></span>

*  <span data-ttu-id="212c8-2691">Expresión de instancia, si existe, del acceso al evento que se va a evaluar.</span><span class="sxs-lookup"><span data-stu-id="212c8-2691">The instance expression, if any, of the event access is evaluated.</span></span>
*  <span data-ttu-id="212c8-2692">Se evalúa el operando derecho del operador `+=` o `-=` y, si es necesario, se convierte al tipo del operando izquierdo a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2692">The right operand of the `+=` or `-=` operator is evaluated, and, if required, converted to the type of the left operand through an implicit conversion ([Implicit conversions](conversions.md#implicit-conversions)).</span></span>
*  <span data-ttu-id="212c8-2693">Se invoca un descriptor de acceso de eventos del evento, con la lista de argumentos que consiste en el operando derecho, después de la evaluación y, si es necesario, conversión.</span><span class="sxs-lookup"><span data-stu-id="212c8-2693">An event accessor of the event is invoked, with argument list consisting of the right operand, after evaluation and, if necessary, conversion.</span></span> <span data-ttu-id="212c8-2694">Si el operador era `+=`, se invoca al descriptor de acceso `add`; Si el operador era `-=`, se invoca al descriptor de acceso `remove`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2694">If the operator was `+=`, the `add` accessor is invoked; if the operator was `-=`, the `remove` accessor is invoked.</span></span>

<span data-ttu-id="212c8-2695">Una expresión de asignación de eventos no produce un valor.</span><span class="sxs-lookup"><span data-stu-id="212c8-2695">An event assignment expression does not yield a value.</span></span> <span data-ttu-id="212c8-2696">Por lo tanto, una expresión de asignación de eventos solo es válida en el contexto de una *statement_expression* ([instrucciones de expresión](statements.md#expression-statements)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2696">Thus, an event assignment expression is valid only in the context of a *statement_expression* ([Expression statements](statements.md#expression-statements)).</span></span>

## <a name="expression"></a><span data-ttu-id="212c8-2697">Expresión</span><span class="sxs-lookup"><span data-stu-id="212c8-2697">Expression</span></span>

<span data-ttu-id="212c8-2698">Una *expresión* es un *non_assignment_expression* o una *asignación*.</span><span class="sxs-lookup"><span data-stu-id="212c8-2698">An *expression* is either a *non_assignment_expression* or an *assignment*.</span></span>

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

## <a name="constant-expressions"></a><span data-ttu-id="212c8-2699">Expresiones de constante</span><span class="sxs-lookup"><span data-stu-id="212c8-2699">Constant expressions</span></span>

<span data-ttu-id="212c8-2700">Un *constant_expression* es una expresión que se puede evaluar por completo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2700">A *constant_expression* is an expression that can be fully evaluated at compile-time.</span></span>

```antlr
constant_expression
    : expression
    ;
```

<span data-ttu-id="212c8-2701">Una expresión constante debe ser el literal `null` o un valor con uno de los siguientes tipos: `sbyte`, @no__t 2, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3, 4 , 5 o cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="212c8-2701">A constant expression must be the `null` literal or a value with one of  the following types: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`, or any enumeration type.</span></span> <span data-ttu-id="212c8-2702">Solo se permiten las siguientes construcciones en expresiones constantes:</span><span class="sxs-lookup"><span data-stu-id="212c8-2702">Only the following constructs are permitted in constant expressions:</span></span>

*  <span data-ttu-id="212c8-2703">Literales (incluido el literal `null`).</span><span class="sxs-lookup"><span data-stu-id="212c8-2703">Literals (including the `null` literal).</span></span>
*  <span data-ttu-id="212c8-2704">Referencias a miembros `const` de tipos de clase y estructura.</span><span class="sxs-lookup"><span data-stu-id="212c8-2704">References to `const` members of class and struct types.</span></span>
*  <span data-ttu-id="212c8-2705">Referencias a miembros de tipos de enumeración.</span><span class="sxs-lookup"><span data-stu-id="212c8-2705">References to members of enumeration types.</span></span>
*  <span data-ttu-id="212c8-2706">Referencias a parámetros `const` o variables locales</span><span class="sxs-lookup"><span data-stu-id="212c8-2706">References to `const` parameters or local variables</span></span>
*  <span data-ttu-id="212c8-2707">Subexpresiones entre paréntesis, que son expresiones constantes.</span><span class="sxs-lookup"><span data-stu-id="212c8-2707">Parenthesized sub-expressions, which are themselves constant expressions.</span></span>
*  <span data-ttu-id="212c8-2708">Expresiones de conversión, siempre que el tipo de destino sea uno de los tipos enumerados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2708">Cast expressions, provided the target type is one of the types listed above.</span></span>
*  <span data-ttu-id="212c8-2709">Expresiones `checked` y `unchecked`</span><span class="sxs-lookup"><span data-stu-id="212c8-2709">`checked` and `unchecked` expressions</span></span>
*  <span data-ttu-id="212c8-2710">Expresiones de valor predeterminado</span><span class="sxs-lookup"><span data-stu-id="212c8-2710">Default value expressions</span></span>
*  <span data-ttu-id="212c8-2711">Expresiones Name</span><span class="sxs-lookup"><span data-stu-id="212c8-2711">Nameof expressions</span></span>
*  <span data-ttu-id="212c8-2712">Los operadores unarios predefinidos `+`, `-`, `!` y `~`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2712">The predefined `+`, `-`, `!`, and `~` unary operators.</span></span>
*  <span data-ttu-id="212c8-2713">El @no__t predefinido-0, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, 0, 1, 2, 3, 4, 5, 6 y 7 operadores binarios, siempre que cada operando sea de tipo enumerado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2713">The predefined `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, and `>=` binary operators, provided each operand is of a type listed above.</span></span>
*  <span data-ttu-id="212c8-2714">Operador condicional `?:`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2714">The `?:` conditional operator.</span></span>

<span data-ttu-id="212c8-2715">Las siguientes conversiones se permiten en expresiones constantes:</span><span class="sxs-lookup"><span data-stu-id="212c8-2715">The following conversions are permitted in constant expressions:</span></span>

*  <span data-ttu-id="212c8-2716">Conversiones de identidad</span><span class="sxs-lookup"><span data-stu-id="212c8-2716">Identity conversions</span></span>
*  <span data-ttu-id="212c8-2717">Conversiones numéricas</span><span class="sxs-lookup"><span data-stu-id="212c8-2717">Numeric conversions</span></span>
*  <span data-ttu-id="212c8-2718">Conversiones de enumeración</span><span class="sxs-lookup"><span data-stu-id="212c8-2718">Enumeration conversions</span></span>
*  <span data-ttu-id="212c8-2719">Conversiones de expresiones constantes</span><span class="sxs-lookup"><span data-stu-id="212c8-2719">Constant expression conversions</span></span>
*  <span data-ttu-id="212c8-2720">Conversiones de referencia implícitas y explícitas, siempre que el origen de las conversiones sea una expresión constante que se evalúe como el valor null.</span><span class="sxs-lookup"><span data-stu-id="212c8-2720">Implicit and explicit reference conversions, provided that the source of the conversions is a constant expression that evaluates to the null value.</span></span>

<span data-ttu-id="212c8-2721">No se permiten otras conversiones, incluidas las conversiones Boxing, unboxing y de referencia implícita de valores no NULL en expresiones constantes.</span><span class="sxs-lookup"><span data-stu-id="212c8-2721">Other conversions including boxing, unboxing and implicit reference conversions of non-null values are not permitted in constant expressions.</span></span> <span data-ttu-id="212c8-2722">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="212c8-2722">For example:</span></span>
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
<span data-ttu-id="212c8-2723">la inicialización de i es un error porque se requiere una conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="212c8-2723">the initialization of i is an error because a boxing conversion is required.</span></span> <span data-ttu-id="212c8-2724">La inicialización de Str es un error porque se requiere una conversión de referencia implícita de un valor distinto de NULL.</span><span class="sxs-lookup"><span data-stu-id="212c8-2724">The initialization of str is an error because an implicit reference conversion from a non-null value is required.</span></span>

<span data-ttu-id="212c8-2725">Siempre que una expresión cumple los requisitos mencionados anteriormente, la expresión se evalúa en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2725">Whenever an expression fulfills the requirements listed above, the expression is evaluated at compile-time.</span></span> <span data-ttu-id="212c8-2726">Esto es así incluso si la expresión es una subexpresión de una expresión mayor que contiene construcciones que no son constantes.</span><span class="sxs-lookup"><span data-stu-id="212c8-2726">This is true even if the expression is a sub-expression of a larger expression that contains non-constant constructs.</span></span>

<span data-ttu-id="212c8-2727">La evaluación en tiempo de compilación de las expresiones constantes utiliza las mismas reglas que la evaluación en tiempo de ejecución de expresiones no constantes, salvo que, en el caso de que la evaluación en tiempo de ejecución hubiera producido una excepción, la evaluación en tiempo de compilación provoca un error en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2727">The compile-time evaluation of constant expressions uses the same rules as run-time evaluation of non-constant expressions, except that where run-time evaluation would have thrown an exception, compile-time evaluation causes a compile-time error to occur.</span></span>

<span data-ttu-id="212c8-2728">A menos que una expresión constante se coloque explícitamente en un contexto `unchecked`, los desbordamientos que se producen en operaciones aritméticas de tipo entero y en las conversiones durante la evaluación en tiempo de compilación de la expresión siempre producen errores en tiempo de compilación ([Constant Expresiones](expressions.md#constant-expressions)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2728">Unless a constant expression is explicitly placed in an `unchecked` context, overflows that occur in integral-type arithmetic operations and conversions during the compile-time evaluation of the expression always cause compile-time errors ([Constant expressions](expressions.md#constant-expressions)).</span></span>

<span data-ttu-id="212c8-2729">Las expresiones constantes se producen en los contextos que se enumeran a continuación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2729">Constant expressions occur in the contexts listed below.</span></span> <span data-ttu-id="212c8-2730">En estos contextos, se produce un error en tiempo de compilación si una expresión no se puede evaluar por completo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="212c8-2730">In these contexts, a compile-time error occurs if an expression cannot be fully evaluated at compile-time.</span></span>

*  <span data-ttu-id="212c8-2731">Declaraciones de constantes ([constantes](classes.md#constants)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2731">Constant declarations ([Constants](classes.md#constants)).</span></span>
*  <span data-ttu-id="212c8-2732">Declaraciones de miembros de enumeración ([miembros de enumeración](enums.md#enum-members)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2732">Enumeration member declarations ([Enum members](enums.md#enum-members)).</span></span>
*  <span data-ttu-id="212c8-2733">Argumentos predeterminados de las listas de parámetros formales ([parámetros de método](classes.md#method-parameters))</span><span class="sxs-lookup"><span data-stu-id="212c8-2733">Default arguments of formal parameter lists ([Method parameters](classes.md#method-parameters))</span></span>
*  <span data-ttu-id="212c8-2734">etiquetas `case` de una instrucción `switch` ([la instrucción switch](statements.md#the-switch-statement)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2734">`case` labels of a `switch` statement ([The switch statement](statements.md#the-switch-statement)).</span></span>
*  <span data-ttu-id="212c8-2735">instrucciones `goto case` ([instrucción Goto](statements.md#the-goto-statement)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2735">`goto case` statements ([The goto statement](statements.md#the-goto-statement)).</span></span>
*  <span data-ttu-id="212c8-2736">Longitudes de dimensión en una expresión de creación de matriz ([expresiones de creación de matrices](expressions.md#array-creation-expressions)) que incluye un inicializador.</span><span class="sxs-lookup"><span data-stu-id="212c8-2736">Dimension lengths in an array creation expression ([Array creation expressions](expressions.md#array-creation-expressions)) that includes an initializer.</span></span>
*  <span data-ttu-id="212c8-2737">Attributes ([atributos](attributes.md)).</span><span class="sxs-lookup"><span data-stu-id="212c8-2737">Attributes ([Attributes](attributes.md)).</span></span>

<span data-ttu-id="212c8-2738">Una conversión de expresión constante implícita ([conversiones de expresión constante implícita](conversions.md#implicit-constant-expression-conversions)) permite convertir una expresión constante de tipo `int` a `sbyte`, `byte`, `short`, `ushort`, `uint` o `ulong`, siempre y cuando el valor de la la expresión constante está dentro del intervalo del tipo de destino.</span><span class="sxs-lookup"><span data-stu-id="212c8-2738">An implicit constant expression conversion ([Implicit constant expression conversions](conversions.md#implicit-constant-expression-conversions)) permits a constant expression of type `int` to be converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, or `ulong`, provided the value of the constant expression is within the range of the destination type.</span></span>

## <a name="boolean-expressions"></a><span data-ttu-id="212c8-2739">expresiones booleanas</span><span class="sxs-lookup"><span data-stu-id="212c8-2739">Boolean expressions</span></span>

<span data-ttu-id="212c8-2740">Una *Boolean_expression* es una expresión que produce un resultado de tipo `bool`; ya sea directamente o a través de la aplicación de `operator true` en determinados contextos, tal y como se especifica en el siguiente.</span><span class="sxs-lookup"><span data-stu-id="212c8-2740">A *boolean_expression* is an expression that yields a result of type `bool`; either directly or through application of `operator true` in certain contexts as specified in the following.</span></span>

```antlr
boolean_expression
    : expression
    ;
```

<span data-ttu-id="212c8-2741">La expresión condicional de control de *una if_statement* ([la instrucción If](statements.md#the-if-statement)) *, while_statement* ([la instrucción while](statements.md#the-while-statement)), *do_statement* ([la instrucción do](statements.md#the-do-statement)) o *for_Statement* ([para Statement](statements.md#the-for-statement)) es una *Boolean_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-2741">The controlling conditional expression of an *if_statement* ([The if statement](statements.md#the-if-statement)), *while_statement* ([The while statement](statements.md#the-while-statement)), *do_statement* ([The do statement](statements.md#the-do-statement)), or *for_statement* ([The for statement](statements.md#the-for-statement)) is a *boolean_expression*.</span></span> <span data-ttu-id="212c8-2742">La expresión condicional de control del operador `?:` ([operador condicional](expressions.md#conditional-operator)) sigue las mismas reglas que una *Boolean_expression*, pero por razones de precedencia de operadores se clasifica como *conditional_or_expression*.</span><span class="sxs-lookup"><span data-stu-id="212c8-2742">The controlling conditional expression of the `?:` operator ([Conditional operator](expressions.md#conditional-operator)) follows the same rules as a *boolean_expression*, but for reasons of operator precedence is classified as a *conditional_or_expression*.</span></span>

<span data-ttu-id="212c8-2743">Se requiere una *boolean_expression* `E` para poder generar un valor de tipo `bool`, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="212c8-2743">A *boolean_expression* `E` is required to be able to produce a value of type `bool`, as follows:</span></span>

*  <span data-ttu-id="212c8-2744">Si `E` es implícitamente convertible a `bool` en tiempo de ejecución, se aplica la conversión implícita.</span><span class="sxs-lookup"><span data-stu-id="212c8-2744">If `E` is implicitly convertible to `bool` then at runtime that implicit conversion is applied.</span></span>
*  <span data-ttu-id="212c8-2745">De lo contrario, se utiliza la resolución de sobrecargas de operador unario ([resolución de sobrecarga de operadores unarios](expressions.md#unary-operator-overload-resolution)) para encontrar una implementación óptima única de Operator `true` en `E` y esa implementación se aplica en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="212c8-2745">Otherwise, unary operator overload resolution ([Unary operator overload resolution](expressions.md#unary-operator-overload-resolution)) is used to find a unique best implementation of operator `true` on `E`, and that implementation is applied at runtime.</span></span>
*  <span data-ttu-id="212c8-2746">Si no se encuentra ningún operador de este tipo, se produce un error en tiempo de enlace.</span><span class="sxs-lookup"><span data-stu-id="212c8-2746">If no such operator is found, a binding-time error occurs.</span></span>

<span data-ttu-id="212c8-2747">El tipo de estructura `DBBool` en [tipo booleano de base de datos](structs.md#database-boolean-type) proporciona un ejemplo de un tipo que implementa `operator true` y `operator false`.</span><span class="sxs-lookup"><span data-stu-id="212c8-2747">The `DBBool` struct type in [Database boolean type](structs.md#database-boolean-type) provides an example of a type that implements `operator true` and `operator false`.</span></span>
