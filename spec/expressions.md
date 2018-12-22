# <a name="expressions"></a>Expresiones

Una expresión es una secuencia de operadores y operandos. Este capítulo define la sintaxis, el orden de evaluación de operandos y operadores y el significado de las expresiones.

## <a name="expression-classifications"></a>Clasificaciones de expresiones

Una expresión se clasifica de las siguientes formas:

*  Un valor. Todos los valores tienen un tipo asociado.
*  Una variable. Cada variable tiene un tipo asociado, es decir, el tipo declarado de la variable.
*  Espacio de nombres. Una expresión con esta clasificación solo puede aparecer como el lado izquierdo de un *member_access* ([acceso a miembros](expressions.md#member-access)). En cualquier otro contexto, una expresión que se clasifica como un espacio de nombres produce un error en tiempo de compilación.
*  Un tipo. Una expresión con esta clasificación solo puede aparecer como el lado izquierdo de un *member_access* ([acceso a miembros](expressions.md#member-access)), o como operando para el `as` operador ([el operador as ](expressions.md#the-as-operator)), el `is` operador ([el operador is](expressions.md#the-is-operator)), o la `typeof` operador ([el operador typeof](expressions.md#the-typeof-operator)). En cualquier otro contexto, una expresión que se clasifica como un tipo produce un error en tiempo de compilación.
*  Un grupo de métodos, que es un conjunto de métodos sobrecargados procedente de una búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)). Un grupo de métodos puede tener una expresión de instancia asociada y una lista de argumentos de tipo asociado. Cuando se invoca un método de instancia, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)). Se permite un grupo de métodos en un *invocation_expression* ([expresiones de invocación](expressions.md#invocation-expressions)), un *delegate_creation_expression* ([delegar la creación expresiones](expressions.md#delegate-creation-expressions)) y como el lado izquierdo de un operador y se puede convertir implícitamente a un tipo delegado compatible ([conversiones de métodos de grupo](conversions.md#method-group-conversions)). En cualquier otro contexto, una expresión que se clasifica como un grupo de métodos produce un error en tiempo de compilación.
*  Un literal null. Una expresión con esta clasificación se puede convertir implícitamente a un tipo de referencia o tipo que acepta valores NULL.
*  Una función anónima. Una expresión con esta clasificación puede convertirse implícitamente en un tipo de árbol de expresión o delegado compatible.
*  Acceso a una propiedad. Cada acceso de propiedad tiene un tipo asociado, el tipo declarado de la propiedad. Además, el acceso a una propiedad puede tener una expresión de instancia asociada. Cuando un descriptor de acceso (la `get` o `set` bloque) de una instancia se invoca el acceso a la propiedad, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)).
*  Acceso de un evento. Cada acceso de eventos tiene un tipo asociado, es decir, el tipo del evento. Además, un acceso de eventos puede tener una expresión de instancia asociada. Un acceso a evento puede aparecer como el operando izquierdo de la `+=` y `-=` operadores ([asignación de evento](expressions.md#event-assignment)). En cualquier otro contexto, una expresión que se clasifica como un acceso a evento produce un error de tiempo de compilación.
*  Acceso de un indizador. Cada acceso al indizador tiene un tipo asociado, es decir, el tipo de elemento del indizador. Además, un acceso de indizador tiene una expresión de instancia asociada y una lista de argumentos asociada. Cuando un descriptor de acceso (la `get` o `set` bloque) de un indizador se invoca el acceso, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)) y el resultado de evaluación de la lista de argumentos se convierte en la lista de parámetros de la invocación.
*  No hay nada. Esto se produce cuando la expresión es una invocación de un método con un tipo de valor devuelto de `void`. Una expresión que se clasifica como nada sólo es válida en el contexto de un *statement_expression* ([las instrucciones de expresión](statements.md#expression-statements)).

El resultado final de una expresión nunca es un espacio de nombres, tipo, grupo de métodos o acceso de eventos. En su lugar, como se mencionó anteriormente, estas categorías de expresiones son construcciones intermedias que solo se permiten en ciertos contextos.

Un acceso de propiedad o indizador siempre es reclasificar como un valor mediante la realización de una invocación de la *descriptor de acceso get* o *descriptor de acceso set*. El descriptor de acceso concreto viene determinada por el contexto del acceso de la propiedad o indizador: Si el acceso es el destino de una asignación, el *descriptor de acceso set* se invoca para asignar un nuevo valor ([asignación Simple](expressions.md#simple-assignment)). En caso contrario, el *descriptor de acceso get* se invoca para obtener el valor actual ([valores de expresiones](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>Valores de expresiones

La mayoría de las construcciones que implican una expresión requiere que en última instancia, la expresión para denotar un ***valor***. En tales casos, si la expresión real denota un espacio de nombres, un tipo, un grupo de métodos o nada, se produce un error en tiempo de compilación. Sin embargo, si la expresión denota un acceso de propiedad, un acceso de indizador o una variable, se sustituye implícitamente el valor de la propiedad, indizador o variable:

*  El valor de una variable es simplemente el valor almacenado actualmente en la ubicación de almacenamiento identificada por la variable. Una variable se considera asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) antes de que se puede obtener su valor o, de lo contrario se produce un error en tiempo de compilación.
*  El valor de una expresión de acceso de propiedad se obtiene al invocar el *descriptor de acceso get* de la propiedad. Si la propiedad no tiene ningún *descriptor de acceso get*, se produce un error de tiempo de compilación. En caso contrario, una invocación de miembros de función ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se realiza, y el resultado de la invocación se convierte en el valor de la expresión de acceso de propiedad.
*  El valor de una expresión de acceso de indizador se obtiene al invocar el *descriptor de acceso get* del indizador. Si el indizador no tiene ningún *descriptor de acceso get*, se produce un error de tiempo de compilación. En caso contrario, una invocación de miembros de función ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se realiza con el argumento de lista asociada con la expresión de acceso de indizador, y el resultado de la invocación se convierte en el valor de la expresión de acceso de indizador.

## <a name="static-and-dynamic-binding"></a>Enlaces estáticos y dinámicos

El proceso de determinar el significado de una operación basada en el tipo o valor de expresiones constituyentes (argumentos, operandos, receptores) a menudo se conoce como ***enlace***. Por ejemplo el significado de una llamada al método se determina según el tipo de los argumentos y el receptor. El significado de un operador se determina según el tipo de sus operandos.

En el significado de una operación normalmente se determina en tiempo de compilación, en función del tipo de tiempo de compilación de sus expresiones constituyentes de C#. Del mismo modo, si una expresión contiene un error, el error se ha detectado y notificado por el compilador. Este enfoque se conoce como ***enlace estático***.

Sin embargo, si una expresión es una expresión dinámica (es decir, tiene el tipo `dynamic`) indica que cualquier enlace que participa en debe basarse en su tipo en tiempo de ejecución (es decir, el tipo real del objeto indica en tiempo de ejecución) en lugar del tipo tiene en tiempo de compilación. Por lo tanto, el enlace de este tipo de operación se aplaza hasta el momento en que la operación es que se ejecutará durante la ejecución del programa. Esto se conoce como ***enlace dinámico***.

Cuando una operación se enlaza dinámicamente, poca o ninguna comprobación se realiza por el compilador. En su lugar, si se produce un error en el enlace en tiempo de ejecución, se notifican errores como excepciones en tiempo de ejecución.

Las siguientes operaciones en C# están sujetos a enlace:

*  Acceso a miembros: `e.M`
*  Invocación de método: `e.M(e1, ..., eN)`
*  Invocación del delegado:`e(e1, ..., eN)`
*  Acceso de elemento: `e[e1, ..., eN]`
*  Creación de objetos: `new C(e1, ..., eN)`
*  Sobrecargar operadores unarios: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Sobrecargar operadores binarios: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`
*  Operadores de asignación: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`
*  Conversiones implícitas y explícitas

Cuando no hay implicada ninguna expresión dinámica, C# tiene como valor predeterminado para el enlace estático, lo que significa que los tipos de tiempo de compilación de expresiones constituyentes se usan en el proceso de selección. Sin embargo, cuando una de las expresiones constituyentes en las operaciones enumeradas anteriormente es una expresión dinámica, la operación en su lugar se enlaza dinámicamente.

### <a name="binding-time"></a>Tiempo de enlace

Enlace estático lleva a cabo en tiempo de compilación, mientras que el enlace dinámico realiza en tiempo de ejecución. En las secciones siguientes, el término ***tiempo de enlace*** se refiere al tiempo de compilación o tiempo de ejecución, dependiendo de cuándo realiza el enlace.

El ejemplo siguiente muestra las nociones de enlaces estáticos y dinámicos y de tiempo de enlace:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

Las dos primeras llamadas están enlazadas estáticamente: la sobrecarga de `Console.WriteLine` se selecciona basándose en el tipo de tiempo de compilación de su argumento. Por lo tanto, el tiempo de enlace es el tiempo de compilación.

La tercera llamada se enlaza dinámicamente: la sobrecarga de `Console.WriteLine` se selecciona basándose en el tipo de tiempo de ejecución de su argumento. Esto sucede porque el argumento es una expresión dinámica, que es su tipo en tiempo de compilación `dynamic`. Por lo tanto, el tiempo de enlace para la tercera llamada es el tiempo de ejecución.

### <a name="dynamic-binding"></a>Enlace dinámico

El propósito de enlace dinámico es permitir que los programas de C# interactuar con ***objetos dinámicos***, es decir, sistema de tipos de objetos que no siguen las reglas normales de C#. Objetos dinámicos pueden ser objetos de otros lenguajes de programación con los sistemas de tipos diferentes, o pueden ser objetos que se configuran para implementar su propia semántica de enlace de distintas operaciones de mediante programación.

El mecanismo por el que un objeto dinámico implementa su propia semántica es implementación definida. Una interfaz determinada--nuevo implementación definida, se implementa mediante objetos dinámicos para señalar a la C#-tiempo de ejecución que tienen una semántica especial. Por lo tanto, siempre que las operaciones en un objeto dinámico se enlazan dinámicamente, su propia semántica de enlace, en lugar de los de C#, tal como se especifica en este documento, asumir.

Aunque el propósito de enlace dinámico es permitir la interoperación con objetos dinámicos, C# permite a enlace dinámico en todos los objetos, ya sean dinámicos o no. Esto permite una integración más fluida de los objetos dinámicos, como los resultados de operaciones en ellos no ellos mismos pueden ser objetos dinámicos, pero son todavía de un tipo desconocido para el programador en tiempo de compilación. También enlace dinámico puede ayudar a eliminar código basado en reflexión propensas a errores incluso cuando no hay objetos implicados son objetos dinámicos.

Las secciones siguientes describen para cada construcción en el lenguaje exactamente cuando enlace dinámico se aplica, lo que compile la comprobación del tiempo: si se aplica, y es lo que la clasificación de resultados y la expresión de tiempo de compilación.

### <a name="types-of-constituent-expressions"></a>Tipos de expresiones constituyentes

Cuando una operación estáticamente está enlazada, el tipo de una expresión que lo constituyen (por ejemplo, un receptor y argumento, un índice o un operando) siempre se considera el tipo de tiempo de compilación de dicha expresión.

Cuando una operación se enlaza dinámicamente, se determina el tipo de una expresión constituyente de maneras diferentes según el tipo de tiempo de compilación de la expresión que lo constituyen:

*  Una expresión de tipo en tiempo de compilación constituyente `dynamic` se considera que tiene el tipo del valor real que la expresión se evalúa como en tiempo de ejecución
*  Una expresión que lo constituyen cuyo tipo de tiempo de compilación es un parámetro de tipo se considera que tiene el tipo que está enlazado el parámetro de tipo en tiempo de ejecución
*  En caso contrario, la expresión constituyente se considera que tiene su tipo en tiempo de compilación.

## <a name="operators"></a>Operadores

Las expresiones se construyen a partir ***operandos*** y ***operadores***. Los operadores de una expresión indican qué operaciones se aplican a los operandos. Ejemplos de operadores incluyen `+`, `-`, `*`, `/` y `new`. Algunos ejemplos de operandos son literales, campos, variables locales y expresiones.

Hay tres tipos de operadores:

*  Operadores unarios. Los operadores unarios toman un operando y usar la notación de prefijo (como `--x`) o la notación de postfijo (como `x++`).
*  Operadores binarios. Los operadores binarios tienen dos operandos y utilizan una notación infija (como `x + y`).
*  operador ternario. Solo un operador ternario, `?:`, existe; toma tres operandos y utiliza notación infija (`c ? x : y`).

El orden de evaluación de los operadores en una expresión viene determinada por la ***prioridad*** y ***asociatividad*** de los operadores ([prioridad y asociatividad](expressions.md#operator-precedence-and-associativity)) .

Los operandos de una expresión se evalúan de izquierda a derecha. Por ejemplo, en `F(i) + G(i++) * H(i)`, método `F` se denomina con el valor antiguo de `i`, método, a continuación, `G` se llama con el valor antiguo de `i`y, por último, el método `H` se denomina con el nuevo valor de `i`. Esto es independiente y no relacionados a la prioridad de operador.

Pueden ser determinados operadores ***sobrecargado***. Sobrecarga de operador permite implementaciones de operadores definidos por el usuario se puede especificar para las operaciones en el que uno o ambos operandos son de un tipo de clase o struct definido por el usuario ([la sobrecarga de operadores](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>Prioridad y asociatividad de los operadores

Cuando una expresión contiene varios operadores, la ***precedencia*** de los operadores controla el orden en que se evalúan los operadores individuales. Por ejemplo, la expresión `x + y * z` se evalúa como `x + (y * z)` porque el `*` operador tiene mayor prioridad que el archivo binario `+` operador. La prioridad de un operador se establece mediante la definición de su producción gramatical asociada. Por ejemplo, un *additive_expression* consta de una secuencia de *multiplicative_expression*s separados por `+` o `-` operadores, lo que da a la `+` y `-` operadores menor prioridad que el `*`, `/`, y `%` operadores.

En la tabla siguiente se resume todos los operadores en orden de prioridad de mayor a menor:

| __Sección__                                                                                   | __Categoría__                | __Operadores__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Expresiones primarias](expressions.md#primary-expressions)                                     | Principal                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
| [Operadores unarios](expressions.md#unary-operators)                                             | Unario                       | `+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x` | 
| [Operadores aritméticos](expressions.md#arithmetic-operators)                                   | Multiplicativo              | `*`  `/`  `%` | 
| [Operadores aritméticos](expressions.md#arithmetic-operators)                                   | Aditivo                    | `+`  `-`      | 
| [Operadores de desplazamiento](expressions.md#shift-operators)                                             | Shift                       | `<<`  `>>`    | 
| [Operadores de comprobación de tipos y relacionales](expressions.md#relational-and-type-testing-operators) | Comprobación de tipos y relacional | `<`  `>`  `<=`  `>=`  `is`  `as` | 
| [Operadores de comprobación de tipos y relacionales](expressions.md#relational-and-type-testing-operators) | Igualdad                    | `==`  `!=`    | 
| [Operadores lógicos](expressions.md#logical-operators)                                         | AND lógico                 | `&`           | 
| [Operadores lógicos](expressions.md#logical-operators)                                         | XOR lógico                 | `^`           | 
| [Operadores lógicos](expressions.md#logical-operators)                                         | OR lógico                  | <code>&#124;</code>           |
| [Operadores lógicos condicionales](expressions.md#conditional-logical-operators)                 | AND condicional             | `&&`          | 
| [Operadores lógicos condicionales](expressions.md#conditional-logical-operators)                 | OR condicional              | <code>&#124;&#124;</code>          | 
| [Operador de uso combinado de NULL](expressions.md#the-null-coalescing-operator)                   | Uso combinado de NULL             | `??`          | 
| [Operador condicional](expressions.md#conditional-operator)                                   | Condicional                 | `?:`          | 
| [Operadores de asignación](expressions.md#assignment-operators), [expresiones de función anónima](expressions.md#anonymous-function-expressions)  | Expresión de asignación y lambda | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

Cuando un operando se encuentra entre dos operadores con la misma prioridad, la asociatividad de los operadores controla el orden en que se realizan las operaciones:

*  Excepto los operadores de asignación y el operador de uso combinado de null, todos los operadores binarios son ***asociativos a la izquierda***, lo que significa que las operaciones se realizan de izquierda a derecha. Por ejemplo, `x + y + z` se evalúa como `(x + y) + z`.
*  Los operadores de asignación, el operador de uso combinado de null y el operador condicional (`?:`) son ***asociativos a la derecha***, lo que significa que las operaciones se realizan de derecha a izquierda. Por ejemplo, `x = y = z` se evalúa como `x = (y = z)`.

La precedencia y la asociatividad pueden controlarse mediante paréntesis. Por ejemplo, `x + y * z` primero multiplica `y` por `z` y luego suma el resultado a `x`, pero `(x + y) * z` primero suma `x` y `y` y luego multiplica el resultado por `z`.

### <a name="operator-overloading"></a>Sobrecarga de operadores

Todos los operadores unarios y binarios tienen implementaciones predefinidas que están disponibles automáticamente en cualquier expresión. Además de las implementaciones predefinidas, pueden introducirse implementaciones definidas por el usuario mediante la inclusión de `operator` las declaraciones de clases y structs ([operadores](classes.md#operators)). Las implementaciones de operador definido por el usuario siempre tienen prioridad sobre las implementaciones de operador predefinidas: Solo cuando no existen implementaciones de ningún operador definido por el usuario correspondiente se consideran las implementaciones de operador predefinido, como se describe en [de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution) y [sobrecarga de operador binario resolución](expressions.md#binary-operator-overload-resolution).

El ***operadores unarios sobrecargables*** son:
```csharp
+   -   !   ~   ++   --   true   false
```

Aunque `true` y `false` explícitamente no se utilizan en expresiones (y, por tanto, no se incluyen en la tabla de prioridad de [prioridad y asociatividad](expressions.md#operator-precedence-and-associativity)), se consideran operadores porque son invoca en varios contextos de expresión: expresiones booleanas ([expresiones booleanas](expressions.md#boolean-expressions)) y expresiones que implican el condicional ([operador condicional](expressions.md#conditional-operator)) y la lógica condicional operadores ([operadores lógicos condicionales](expressions.md#conditional-logical-operators)).

El ***puede sobrecargables operadores binarios*** son:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Solo los operadores enumerados anteriormente se pueden sobrecargar. En concreto, no es posible sobrecargar el acceso de miembro, la invocación de método, o la `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, y `is` operadores.

Cuando se sobrecarga un operador binario, el operador de asignación correspondiente, si lo hay, también se sobrecarga de modo implícito. Por ejemplo, una sobrecarga del operador `*` también es una sobrecarga del operador `*=`. Esto se describe más detalladamente en [asignación compuesta](expressions.md#compound-assignment). Tenga en cuenta que el propio operador de asignación (`=`) no se puede sobrecargar. Una asignación siempre realiza una simple copia bit a bit de un valor en una variable.

Convertir las operaciones, tales como `(T)x`, están sobrecargados, ya que proporciona las conversiones definidas por el usuario ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)).

Acceso de elemento, como `a[x]`, no se considera un operador sobrecargable. En su lugar, se admite la indización definido por el usuario a través de los indizadores ([indizadores](classes.md#indexers)).

En las expresiones, se hace referencia a operadores mediante la notación de operador y, en las declaraciones, los operadores se hace referencia mediante la notación funcional. En la tabla siguiente muestra la relación entre las notaciones funcionales para los operadores unarios y binarios y de operador. En la primera entrada, *op* denota cualquier operador unario sobrecargable de prefijo. En la segunda entrada, *op* denota el postfijo unario `++` y `--` operadores. En la tercera entrada, *op* denota cualquier operador binario sobrecargable.


| __Notación de operador__ | __Notación funcional__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Las declaraciones de operador definido por el usuario siempre requieren al menos uno de los parámetros para ser del tipo de clase o estructura que contiene la declaración del operador. Por lo tanto, no es posible que un operador definido por el usuario tener la misma firma que un operador predefinido.

Las declaraciones de operador definido por el usuario no pueden modificar la sintaxis, prioridad o asociatividad de un operador. Por ejemplo, el `/` operador siempre es un operador binario, siempre se el nivel de prioridad especificado en [prioridad y asociatividad](expressions.md#operator-precedence-and-associativity)y siempre es asociativo a la izquierda.

Aunque es posible que un operador definido por el usuario realizar cualquier cálculo que le interese, se desaconseja las implementaciones que generan resultados distintos a los que se esperan de manera intuitiva. Por ejemplo, una implementación de `operator ==` debe comparar la igualdad de los dos operandos y devolver un adecuado `bool` resultado.

Las descripciones de los operadores individuales en [expresiones primarias](expressions.md#primary-expressions) a través de [operadores lógicos condicionales](expressions.md#conditional-logical-operators) especifican las implementaciones de los operadores y las reglas adicionales que se aplican predefinidas para cada operador. Las descripciones de realizar el uso de los términos ***de resolución de sobrecarga de operador unario***, ***de resolución de sobrecarga de operador binario***, y ***promoción numérica***, definiciones de las cuales se se encuentra en las secciones siguientes.

### <a name="unary-operator-overload-resolution"></a>Resolución de sobrecarga de operador unario

Una operación del formulario `op x` o `x op`, donde `op` es un operador unario sobrecargable, y `x` es una expresión de tipo `X`, se procesa como sigue:

*  El conjunto de operadores definidos por el usuario candidatos suministrados por `X` para la operación `operator op(x)` se determina mediante las reglas de [operadores definidos por el usuario de candidatos](expressions.md#candidate-user-defined-operators).
*  Si el conjunto de operadores candidatos definidos por el usuario no está vacío, se convierte en el conjunto de operadores candidatos para la operación. En caso contrario, el operador unario predefinido `operator op` implementaciones, incluidas sus formatos de elevación, se convierten en el conjunto de operadores candidatos para la operación. Las implementaciones predefinidas de un operador determinado se especifican en la descripción del operador ([expresiones primarias](expressions.md#primary-expressions) y [operadores unarios](expressions.md#unary-operators)).
*  Las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution) se aplican al conjunto de operadores candidatos para seleccionar el mejor operador con respecto a la lista de argumentos `(x)`, y este operador es el resultado de la sobrecarga proceso de resolución. Si se produce un error en la resolución de sobrecarga seleccionar un operador único idóneo, se produce un error en tiempo de enlace.

### <a name="binary-operator-overload-resolution"></a>Resolución de sobrecarga de operador binario

Una operación del formulario `x op y`, donde `op` es un operador binario sobrecargable, `x` es una expresión de tipo `X`, y `y` es una expresión de tipo `Y`, se procesa como sigue:

*  El conjunto de operadores definidos por el usuario candidatos suministrados por `X` y `Y` para la operación `operator op(x,y)` viene determinada. El conjunto consta de la unión de los operadores candidatos suministrados por `X` y los operadores candidatos suministrados por `Y`, cada uno de ellos determinado utilizando las reglas de [operadores definidos por el usuario de candidatos](expressions.md#candidate-user-defined-operators). Si `X` y `Y` son del mismo tipo, o si `X` y `Y` se derivan de un tipo base común, los operadores candidatos compartidos solo se producen en el conjunto combinado una vez.
*  Si el conjunto de operadores candidatos definidos por el usuario no está vacío, se convierte en el conjunto de operadores candidatos para la operación. En caso contrario, el archivo binario predefinido `operator op` implementaciones, incluidas sus formatos de elevación, se convierten en el conjunto de operadores candidatos para la operación. Las implementaciones predefinidas de un operador determinado se especifican en la descripción del operador ([operadores aritméticos](expressions.md#arithmetic-operators) a través de [operadores lógicos condicionales](expressions.md#conditional-logical-operators)). Para los operadores predefinidos de enumeración y delegado, los operadores solo considerados son los definidos por un tipo de enumeración o delegado que es el tipo en tiempo de enlace de uno de los operandos.
*  Las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution) se aplican al conjunto de operadores candidatos para seleccionar el mejor operador con respecto a la lista de argumentos `(x,y)`, y este operador es el resultado de la sobrecarga proceso de resolución. Si se produce un error en la resolución de sobrecarga seleccionar un operador único idóneo, se produce un error en tiempo de enlace.

### <a name="candidate-user-defined-operators"></a>Operadores definidos por el usuario de candidatos

Dado un tipo `T` y una operación `operator op(A)`, donde `op` es un operador sobrecargable y `A` es una lista de argumentos, el conjunto de candidatos a operadores definidos por el usuario proporcionados por `T` para `operator op(A)` viene la siguiente manera:

*  Determinar el tipo `T0`. Si `T` es un tipo que acepta valores NULL, `T0` es su tipo subyacente, de lo contrario, `T0` es igual a `T`.
*  Para todos los `operator op` declaraciones en `T0` y todos los formatos de elevación de operadores de este tipo, si es aplicable al menos un operador ([miembro de función aplicable](expressions.md#applicable-function-member)) con respecto a la lista de argumentos `A`, a continuación, el conjunto de operadores candidatos consta de todos estos operadores aplicable en `T0`.
*  De lo contrario, si `T0` es `object`, el conjunto de operadores candidatos está vacío.
*  En caso contrario, proporciona el conjunto de operadores candidatos `T0` es el conjunto de operadores candidatos suministrado por la clase base directa de `T0`, o la clase base efectiva de `T0` si `T0` es un parámetro de tipo.

### <a name="numeric-promotions"></a>Promociones numéricas

La promoción numérica consiste en llevar automáticamente determinadas conversiones implícitas de los operandos de los predefinidos unario y binarios operadores numéricos. Promoción numérica no es un mecanismo distinto, sino más bien un efecto de aplicar la resolución de sobrecarga a los operadores predefinidos. Promoción numérica específicamente no afectan a la evaluación de operadores definidos por el usuario, aunque los operadores definidos por el usuario se pueden implementar para que presenten efectos similares.

Como ejemplo de promoción numérica, considere la posibilidad de las implementaciones predefinidas del binario `*` operador:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Cuando las reglas de resolución de sobrecarga ([de resolución de sobrecarga](expressions.md#overload-resolution)) se aplican a este conjunto de operadores, el efecto es seleccionar el primero de los operadores para las que existen las conversiones implícitas de los tipos de operando. Por ejemplo, para la operación `b * s`, donde `b` es un `byte` y `s` es un `short`, selecciona la resolución de sobrecarga `operator *(int,int)` como el mejor operador. Por lo tanto, el efecto es que `b` y `s` se convierten en `int`, y el tipo del resultado es `int`. Del mismo modo, para la operación `i * d`, donde `i` es un `int` y `d` es un `double`, selecciona la resolución de sobrecarga `operator *(double,double)` como el mejor operador.

#### <a name="unary-numeric-promotions"></a>Promociones numéricas unarias

Promoción numérica unaria se produce para los operandos de predefinido `+`, `-`, y `~` operadores unarios. Promoción numérica unaria simplemente consiste en convertir los operandos de tipo `sbyte`, `byte`, `short`, `ushort`, o `char` escriba `int`. Además, para el operador unario `-` operador, la promoción numérica unaria convierte operandos de tipo `uint` escriba `long`.

#### <a name="binary-numeric-promotions"></a>Promociones numéricas binarias

Se produce una promoción numérica binaria para los operandos de predefinido `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, y `<=` operadores binarios. Promoción numérica binaria convierte implícitamente ambos operandos a un tipo común que, en el caso de los operadores no relacionales, también se convierte en el tipo de resultado de la operación. Promoción numérica binaria consta de aplicar las reglas siguientes, en el orden que aparecen a continuación:

*  Si alguno de los operandos es de tipo `decimal`, el otro operando se convierte al tipo `decimal`, o se produce un error en tiempo de enlace si el otro operando es de tipo `float` o `double`.
*  En caso contrario, si alguno de los operandos es de tipo `double`, el otro operando se convierte al tipo `double`.
*  En caso contrario, si alguno de los operandos es de tipo `float`, el otro operando se convierte al tipo `float`.
*  En caso contrario, si alguno de los operandos es de tipo `ulong`, el otro operando se convierte al tipo `ulong`, o se produce un error en tiempo de enlace si el otro operando es de tipo `sbyte`, `short`, `int`, o `long`.
*  En caso contrario, si alguno de los operandos es de tipo `long`, el otro operando se convierte al tipo `long`.
*  En caso contrario, si alguno de los operandos es de tipo `uint` y el otro operando es de tipo `sbyte`, `short`, o `int`, ambos operandos se convierten al tipo `long`.
*  En caso contrario, si alguno de los operandos es de tipo `uint`, el otro operando se convierte al tipo `uint`.
*  En caso contrario, ambos operandos se convierten al tipo `int`.

Tenga en cuenta que la primera regla no permite las operaciones que combinan la `decimal` tipo con el `double` y `float` tipos. Sigue la regla del hecho de que no hay ninguna conversión implícita entre el `decimal` tipo y el `double` y `float` tipos.

Tenga en cuenta también que no es posible que un operando de tipo `ulong` cuando el otro operando es de un tipo entero con signo. El motivo es que existe ningún tipo de entero que puede representar el intervalo completo de `ulong` , así como los tipos enteros con signo.

En ambos de los casos anteriores, se puede usar una expresión de conversión para convertir explícitamente un operando a un tipo que es compatible con el otro operando.

En el ejemplo
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
se produce un error en tiempo de enlace porque un `decimal` no se puede multiplicar por un `double`. El error se resuelve mediante la conversión explícita del segundo operando `decimal`, como sigue:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Operadores de elevación

***Operadores de elevación*** permitir que los operadores predefinidos y definidos por el usuario que funcionan en tipos de valor distinto de NULL para utilizarse también con los formularios de esos tipos que aceptan valores NULL. Operadores de elevación se construyen a partir de los operadores predefinidos y definidos por el usuario que cumplen ciertos requisitos, como se describe en la siguiente:

*   Para los operadores unarios

    ```csharp
    +  ++  -  --  !  ~
    ```

    un formato de elevación de un operador existe si los tipos de operando y el resultado son ambos tipos de valor distinto de NULL. El formato de elevación se construye mediante la adición de una sola `?` modificador a los tipos de operando y el resultado. El operador de elevación genera un valor null si el operando es null. En caso contrario, el operador de elevación desajusta el operando, aplica el operador subyacente y ajusta el resultado.

*   Para los operadores binarios

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    un formato de elevación de un operador existe si los tipos de operando y el resultado son todos los tipos de valor distinto de NULL. El formato de elevación se construye mediante la adición de una sola `?` modificador a cada tipo de operando y el resultado. El operador de elevación genera un valor null si uno o ambos operandos son nulos (una excepción que se va a la `&` y `|` operadores de la `bool?` type, tal como se describe en [operadores lógicos Boolean](expressions.md#boolean-logical-operators)). En caso contrario, el operador de elevación desajusta los operandos, aplica el operador subyacente y ajusta el resultado.

*   Para los operadores de igualdad

    ```csharp
    ==  !=
    ```

    existe un formulario de elevación de un operador si los tipos de operando son tipos de valor que no aceptan valores NULL y si el tipo de resultado es `bool`. El formato de elevación se construye mediante la adición de una sola `?` modificador a cada tipo de operando. El operador de elevación considera que los dos valores null son iguales y un valor null no es igual a cualquier valor distinto de null. Si ambos operandos son distintos de null, el operador de elevación desajusta los operandos y aplica el operador subyacente para generar el `bool` resultado.

*   Para los operadores relacionales

    ```csharp
    <  >  <=  >=
    ```

    existe un formulario de elevación de un operador si los tipos de operando son tipos de valor que no aceptan valores NULL y si el tipo de resultado es `bool`. El formato de elevación se construye mediante la adición de una sola `?` modificador a cada tipo de operando. El operador de elevación genera el valor `false` si uno o ambos operandos son nulos. En caso contrario, el operador de elevación desajusta los operandos y aplica el operador subyacente para generar el `bool` resultado.

## <a name="member-lookup"></a>Búsqueda de miembros

Una búsqueda de miembros es el proceso mediante el cual se determina el significado de un nombre en el contexto de un tipo. Puede producirse una búsqueda de miembros como parte de la evaluación de un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)) en un expresión. Si el *simple_name* o *member_access* se produce como el *primary_expression* de un *invocation_expression* ([ Las invocaciones de método](expressions.md#method-invocations)), se dice que se invoca el miembro.

Si un miembro es un método o evento, o si es una constante, campo o propiedad de un tipo de delegado ([delegados](delegates.md)) o el tipo `dynamic` ([el tipo dinámico](types.md#the-dynamic-type)), a continuación, se dice que el miembro es *invocable*.

Búsqueda de miembros se considera que no sólo el nombre de un miembro, pero también el número de parámetros de tipo que el miembro tiene y si el miembro es accesible. Para los fines de búsqueda de miembros, los métodos genéricos y tipos genéricos anidados tienen el número de parámetros de tipo indicado en sus respectivas declaraciones y todos los demás miembros tienen cero parámetros de tipo.

Una búsqueda de miembros de un nombre `N` con `K`  parámetros de tipo en un tipo `T` se procesa como sigue:

*  Primero, un conjunto de miembros accesibles denominado `N` viene determinada:
    * Si `T` es un parámetro de tipo, el conjunto es la unión de los conjuntos de miembros accesibles denominado `N` en cada uno de los tipos especificados como una restricción principal o secundaria restricción ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) para  `T`, junto con el conjunto de miembros accesibles denominado `N` en `object`.
    * En caso contrario, el conjunto formado disponible ([acceso a miembros](basic-concepts.md#member-access)) miembros denominados `N` en `T`, incluidos los miembros heredados y los miembros accesibles denominados `N` en `object`. Si `T` es un tipo construido, el conjunto de miembros se obtiene mediante la sustitución de argumentos de tipo como se describe en [los miembros de tipos construidos](classes.md#members-of-constructed-types). Los miembros que incluyen un `override` modificador se excluyen del conjunto.
*  A continuación, si `K` es cero, se quitan los tipos cuyas declaraciones incluyen parámetros de tipo anidado. Si `K` no es cero, todos los miembros con un número diferente de se quitan los parámetros de tipo. Tenga en cuenta que, cuando `K` es cero, los métodos que tienen el tipo no se quitan los parámetros desde el proceso de inferencia de tipo ([inferencia](expressions.md#type-inference)) podría ser capaz de inferir los argumentos de tipo.
*  A continuación, si el miembro es *invoca*, todos los no-*invocable* se quitan los miembros del conjunto.
*  A continuación, se quitan los miembros que están ocultos por otros miembros del conjunto. Para cada miembro `S.M` en el conjunto, donde `S` es el tipo en el que el miembro `M` se declara, se aplican las reglas siguientes:
    * Si `M` es una constante, campo, propiedad, evento o miembro de enumeración, entonces todos los miembros declaran en un tipo base de `S` se quitan del conjunto.
    * Si `M` es una declaración de tipo y, a continuación, todos los que no son tipos declarados en un tipo base del `S` se quitan del conjunto, y todas las declaraciones con el mismo número de parámetros de tipo como de tipo `M` declarados en un tipo base del `S` se quitan desde el conjunto.
    * Si `M` es un método, entonces todos los miembros que no son métodos declaran en un tipo base de `S` se quitan del conjunto.
*  A continuación, se quitan los miembros de interfaz que están ocultos por miembros de clase del conjunto. Este paso sólo tiene efecto si `T` es un parámetro de tipo y `T` tiene una clase base eficaz distinto `object` y un conjunto de interfaces efectivas vacío ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)). Para cada miembro `S.M` en el conjunto, donde `S` es el tipo en el que el miembro `M` se declara las siguientes reglas se aplican si `S` es una declaración de clase que no sean `object`:
    * Si `M` es constante, campo, propiedad, evento, miembro de enumeración o declaración de tipos, a continuación, se quitan todos los miembros declarados en una declaración de interfaz del conjunto.
    * Si `M` es un método, a continuación, se quitan todos los miembros no son métodos declarados en una declaración de interfaz del conjunto y todos los métodos con la misma firma que `M` declarado en una interfaz de declaración se quitan del conjunto.
*  Por último, quitados los miembros ocultos, se determina el resultado de la búsqueda:
    * Si el conjunto consta de un único miembro que no es un método, este miembro es el resultado de la búsqueda.
    * En caso contrario, si el conjunto contiene solo los métodos, este grupo de métodos es el resultado de la búsqueda.
    * En caso contrario, la búsqueda es ambigua y se produce un error en tiempo de enlace.

Para búsquedas de miembros en tipos distintos de los parámetros de tipo y las interfaces y búsquedas de miembros en las interfaces que son estrictamente de herencia simple (cada interfaz en la cadena de herencia tiene exactamente cero o una interfaz base directa), el efecto de las reglas de búsqueda es solo tiene que derivar miembros ocultar miembros base con el mismo nombre o signatura. Este tipo de búsquedas de herencia simple nunca es ambigua. Las ambigüedades que pueden surgir de búsquedas de miembros en interfaces de herencia múltiple se describen en [acceso a miembros de la interfaz](interfaces.md#interface-member-access).

### <a name="base-types"></a>Tipos base

A efectos de la búsqueda de miembros de un tipo `T` se considera que tiene los siguientes tipos base:

*  Si `T` es `object`, a continuación, `T` no tiene ningún tipo base.
*  Si `T` es un *enum_type*, los tipos base del `T` son los tipos de clase `System.Enum`, `System.ValueType`, y `object`.
*  Si `T` es un *struct_type*, los tipos base del `T` son los tipos de clase `System.ValueType` y `object`.
*  Si `T` es un *class_type*, los tipos base del `T` son las clases base de `T`, incluido el tipo de clase `object`.
*  Si `T` es un *interface_type*, los tipos base del `T` son las interfaces base de `T` y el tipo de clase `object`.
*  Si `T` es un *array_type*, los tipos base del `T` son los tipos de clase `System.Array` y `object`.
*  Si `T` es un *delegate_type*, los tipos base del `T` son los tipos de clase `System.Delegate` y `object`.

## <a name="function-members"></a>Miembros de función

Los miembros de función son los miembros que contienen instrucciones ejecutables. Miembros de función siempre son miembros de tipos y no pueden ser miembros de espacios de nombres. C# define las siguientes categorías de miembros de función:

*  Métodos
*  Propiedades
*  Eventos
*  Indizadores
*  Operadores definidos por el usuario
*  Constructores de instancias
*  Constructores estáticos
*  Destructores

Excepto para los destructores y los constructores estáticos (que no se puede invocar explícitamente), las instrucciones incluidas en los miembros de función se ejecutan a través de las llamadas a funciones miembro. La sintaxis para escribir una invocación de miembros de función depende de la categoría de miembro de función determinada.

La lista de argumentos ([listas de argumentos](expressions.md#argument-lists)) de un miembro de función invocación proporciona los valores reales o referencias de variables para los parámetros del miembro de función.

Las invocaciones de métodos genéricos pueden emplear la inferencia de tipos para determinar el conjunto de argumentos de tipo para pasar al método. Este proceso se describe en [inferencia de tipo](expressions.md#type-inference).

Las llamadas de métodos, indizadores, operadores y constructores de instancias utilizan la resolución de sobrecarga para determinar qué parte de un conjunto candidato de miembros de función para invocar. Este proceso se describe en [de resolución de sobrecarga](expressions.md#overload-resolution).

Una vez que se ha identificado un miembro de función determinada en tiempo de enlace, posiblemente a través de la resolución de sobrecarga, el proceso de tiempo de ejecución real de invocar el miembro de función se describe en [comprobación de la resolución de sobrecarga dinámicadetiempodecompilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

En la tabla siguiente se resume el procesamiento que tiene lugar en las construcciones que implican las seis categorías de miembros de función que se pueden invocar explícitamente. En la tabla, `e`, `x`, `y`, y `value` indican expresiones clasificadas como variables o valores, `T` indica una expresión que se clasifica como un tipo, `F` es el nombre sencillo de un método y `P` es el nombre sencillo de una propiedad.


| __Construcción__     | __Ejemplo__    | __Descripción__ |
|-------------------|----------------|-----------------|
| Invocación del método. | `F(x,y)`       | Se aplica la resolución de sobrecarga para seleccionar el mejor método `F` en la clase o estructura contenedora. El método se invoca con la lista de argumentos `(x,y)`. Si el método no es `static`, la expresión de instancia es `this`. | 
|                   | `T.F(x,y)`     | Se aplica la resolución de sobrecarga para seleccionar el mejor método `F` en la clase o struct `T`. Se produce un error en tiempo de enlace si el método no es `static`. El método se invoca con la lista de argumentos `(x,y)`. | 
|                   | `e.F(x,y)`     | Se aplica la resolución de sobrecarga para seleccionar el mejor método F en la clase, estructura o interfaz dada por el tipo de `e`. Se produce un error en tiempo de enlace si el método es `static`. El método se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y)`. | 
| Property Access   | `P`            | El `get` descriptor de acceso de la propiedad `P` se invoca en la clase o estructura contenedora. Se produce un error de tiempo de compilación si `P` es de solo escritura. Si `P` no `static`, la expresión de instancia es `this`. | 
|                   | `P = value`    | El `set` descriptor de acceso de la propiedad `P` en la clase o estructura contenedora se invoca con la lista de argumentos `(value)`. Se produce un error de tiempo de compilación si `P` es de solo lectura. Si `P` no `static`, la expresión de instancia es `this`. | 
|                   | `T.P`          | El `get` descriptor de acceso de la propiedad `P` en la clase o struct `T` se invoca. Se produce un error de tiempo de compilación si `P` no `static` o si `P` es de solo escritura. | 
|                   | `T.P = value`  | El `set` descriptor de acceso de la propiedad `P` en la clase o struct `T` se invoca con la lista de argumentos `(value)`. Se produce un error de tiempo de compilación si `P` no `static` o si `P` es de solo lectura. | 
|                   | `e.P`          | El `get` descriptor de acceso de la propiedad `P` en la clase, estructura o interfaz dada por el tipo de `e` se invoca con la expresión de instancia `e`. Si se produce un error de enlace `P` es `static` o si `P` es de solo escritura. | 
|                   | `e.P = value`  | El `set` descriptor de acceso de la propiedad `P` en la clase, estructura o interfaz dada por el tipo de `e` se invoca con la expresión de instancia `e` y la lista de argumentos `(value)`. Si se produce un error de enlace `P` es `static` o si `P` es de solo lectura. | 
| Acceso a eventos      | `E += value`   | El `add` descriptor de acceso del evento `E` se invoca en la clase o estructura contenedora. Si `E` es no estático, la expresión de instancia es `this`. | 
|                   | `E -= value`   | El `remove` descriptor de acceso del evento `E` se invoca en la clase o estructura contenedora. Si `E` es no estático, la expresión de instancia es `this`. | 
|                   | `T.E += value` | El `add` descriptor de acceso del evento `E` en la clase o struct `T` se invoca. Se produce un error en tiempo de enlace si `E` no es estático. | 
|                   | `T.E -= value` | El `remove` descriptor de acceso del evento `E` en la clase o struct `T` se invoca. Se produce un error en tiempo de enlace si `E` no es estático. | 
|                   | `e.E += value` | El `add` descriptor de acceso del evento `E` en la clase, estructura o interfaz dada por el tipo de `e` se invoca con la expresión de instancia `e`. Se produce un error en tiempo de enlace si `E` es estático. | 
|                   | `e.E -= value` | El `remove` descriptor de acceso del evento `E` en la clase, estructura o interfaz dada por el tipo de `e` se invoca con la expresión de instancia `e`. Se produce un error en tiempo de enlace si `E` es estático. | 
| Acceso al indizador    | `e[x,y]`       | Se aplica la resolución de sobrecarga para seleccionar el mejor indizador de la clase, estructura o interfaz dada por el tipo de e. El `get` descriptor de acceso del indizador se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y)`. Si el indizador es de solo escritura, se produce un error en tiempo de enlace. | 
|                   | `e[x,y] = value` | Se aplica la resolución de sobrecarga para seleccionar el mejor indizador de la clase, estructura o interfaz dada por el tipo de `e`. El `set` descriptor de acceso del indizador se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y,value)`. Si el indizador es de solo lectura, se produce un error en tiempo de enlace. | 
| Invocación de operador | `-x`         | Se aplica la resolución de sobrecarga para seleccionar el operador unario mejor en la clase o struct dada por el tipo de `x`. Se invoca el operador seleccionado con la lista de argumentos `(x)`. | 
|                     | `x + y`      | Se aplica la resolución de sobrecarga para seleccionar el mejor operador binario de las clases o structs proporcionados por los tipos de `x` y `y`. Se invoca el operador seleccionado con la lista de argumentos `(x,y)`. | 
| Invocación del constructor de instancia | `new T(x,y)` | Se aplica la resolución de sobrecarga para seleccionar el mejor constructor de instancia de la clase o estructura `T`. Se invoca el constructor de instancia con la lista de argumentos `(x,y)`. | 

### <a name="argument-lists"></a>Listas de argumentos

Cada invocación de miembros y el delegado de función incluye una lista de argumentos que proporciona los valores reales o referencias de variables para los parámetros del miembro de función. La sintaxis para especificar la lista de argumentos de una invocación de miembros de función depende de la categoría de miembro de función:

*  Por ejemplo los constructores, métodos, indizadores y delegados, los argumentos se especifican como un *argument_list*, tal y como se describe a continuación. Para los indizadores, al invocar el `set` descriptor de acceso, la lista de argumentos incluye además la expresión especificada como el operando derecho del operador de asignación.
*  Para las propiedades, la lista de argumentos está vacía cuando se invoca el `get` descriptor de acceso y consta de la expresión especificada como el operando derecho del operador de asignación al invocar el `set` descriptor de acceso.
*  Para los eventos, la lista de argumentos consta de la expresión especificada como el operando derecho de la `+=` o `-=` operador.
*  Para los operadores definidos por el usuario, la lista de argumentos está formado por el único operando del operador unario o los dos operandos del operador binario.

Los argumentos de propiedades ([propiedades](classes.md#properties)), los eventos ([eventos](classes.md#events)) y los operadores definidos por el usuario ([operadores](classes.md#operators)) siempre se pasan como parámetros de valor ([ Parámetros del valor](classes.md#value-parameters)). Los argumentos de indizadores ([indizadores](classes.md#indexers)) siempre se pasan como parámetros de valor ([parámetros del valor](classes.md#value-parameters)) o matrices de parámetros ([las matrices de parámetros](classes.md#parameter-arrays)). No se admiten parámetros de referencia y de salida para estas categorías de miembros de función.

Los argumentos de una invocación de constructor, método, indizador o delegado de la instancia se especifican como un *argument_list*:

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

Un *argument_list* consta de uno o varios *argumento*s, separados por comas. Cada argumento consta de un elemento opcional *argument_name* seguido por un *argument_value*. Un *argumento* con un *argument_name* se conoce como un ***argumento con nombre***, mientras que un *argumento* sin un  *argument_name* es un ***argumento posicional***. Es un error para un argumento posicional en aparecer después de un argumento con nombre en un *argument_list*.

El *argument_value* puede tomar uno de los formatos siguientes:

*  Un *expresión*, que indica que el argumento se pasa como un parámetro de valor ([parámetros del valor](classes.md#value-parameters)).
*  La palabra clave `ref` seguido por un *variable_reference* ([referencias de variables](variables.md#variable-references)), que indica que el argumento se pasa como un parámetro de referencia ([hacen referencia a parámetros ](classes.md#reference-parameters)). Una variable debe asignarlo definitivamente ([asignación definitiva](variables.md#definite-assignment)) antes de que se puede pasar como un parámetro de referencia. La palabra clave `out` seguido por un *variable_reference* ([referencias de variables](variables.md#variable-references)), que indica que el argumento se pasa como un parámetro de salida ([parámetrosdesalida](classes.md#output-parameters)). Una variable se considera asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) siguiendo una invocación de miembros de función en el que la variable se pasa como parámetro de salida.

#### <a name="corresponding-parameters"></a>Parámetros correspondientes

Para cada argumento de una lista de argumentos debe haber un parámetro correspondiente en el miembro de función o un delegado que se invoca.

La lista de parámetros utilizada en el siguiente ejemplo se determina como sigue:

*  Para los métodos virtuales y los indizadores definidos en las clases, la lista de parámetros seleccionado de la declaración más específica o reemplazar el miembro de función, empezando por el tipo estático del receptor y buscar a través de sus clases base.
*  Para los métodos de interfaz y los indizadores, se toma la lista de parámetros forman la definición más específica del miembro, empezando por el tipo de interfaz y la búsqueda a través de las interfaces base. Si no se encuentra ninguna lista de parámetros únicos, se construye una lista de parámetros con nombres accesibles y no existen parámetros opcionales, por lo que no se pueden usar parámetros con nombre o se omiten los argumentos opcionales invocaciones.
*  Para los métodos parciales, se usa la lista de parámetros de la declaración de método parcial de definición.
*  No hay sólo una lista de parámetros único, que es el que se usa para todos los demás miembros de función y delegados.

La posición de un argumento o parámetro se define como el número de argumentos o parámetros que le precede en la lista de argumentos o la lista de parámetros.

Los parámetros correspondientes para los argumentos de función miembro se establecen como sigue:

*  Argumentos de la *argument_list* de constructores de instancias, métodos, indizadores y delegados:
    * Un argumento de posición donde se produce un parámetro fijo en la misma posición en la lista de parámetros que se corresponde a ese parámetro.
    * Un argumento posicional de un miembro de función con una matriz de parámetros que se invoca en su forma normal corresponde a la matriz de parámetros, que debe aparecer en la misma posición en la lista de parámetros.
    * Un argumento posicional de un miembro de función con una matriz de parámetros que se invoca en su forma expandida, donde ningún parámetro fijo se produce en la misma posición en la lista de parámetros, corresponde a un elemento de la matriz de parámetros.
    * Un argumento con nombre se corresponde con el parámetro del mismo nombre en la lista de parámetros.
    * Para los indizadores, al invocar el `set` descriptor de acceso, la expresión especificada como el operando derecho del operador de asignación corresponde a implícito `value` parámetro de la `set` declaración del descriptor de acceso.
*  Para las propiedades, al invocar el `get` no son de descriptor de acceso no existe ningún argumento. Al invocar el `set` descriptor de acceso, la expresión especificada como el operando derecho del operador de asignación corresponde a implícito `value` parámetro de la `set` declaración del descriptor de acceso.
*  Para los operadores unarios definido por el usuario (incluidas las conversiones), el único operando corresponde al parámetro único de la declaración del operador.
*  Para los operadores binarios definidos por el usuario, el operando izquierdo corresponde al primer parámetro y el operando derecho se corresponde con el segundo parámetro de la declaración del operador.

#### <a name="run-time-evaluation-of-argument-lists"></a>Evaluación de tiempo de ejecución de las listas de argumentos

Durante el procesamiento de tiempo de ejecución de una invocación de miembros de función ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), las expresiones o referencias a variables de una lista de argumentos se evalúan en orden, de izquierda a derecha, como se indica a continuación:

*  Para un parámetro de valor, se evalúa la expresión de argumento y una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) al parámetro correspondiente del tipo que se realiza. El valor resultante se convierte en el valor inicial del parámetro de valor en la invocación de miembros de función.
*  Para un parámetro de referencia o de salida, se evalúa la referencia de variable y la ubicación de almacenamiento resultante se convierte en la ubicación de almacenamiento representada por el parámetro de la invocación de miembros de función. Si la referencia de variable como un parámetro de referencia o de salida es un elemento de matriz de un *reference_type*, se realiza una comprobación de tiempo de ejecución para asegurarse de que el tipo de elemento de la matriz es idéntico al tipo del parámetro. Si se produce un error en esta comprobación, un `System.ArrayTypeMismatchException` se produce.

Los métodos, indizadores y constructores de instancias pueden declarar su parámetro de derecha a ser una matriz de parámetros ([las matrices de parámetros](classes.md#parameter-arrays)). Estos miembros de función se invocan en su forma normal o en su forma expandida, dependiendo de lo que es aplicable ([miembro de función aplicable](expressions.md#applicable-function-member)):

*  Cuando se invoca un miembro de función con una matriz de parámetros en su forma normal, el argumento especificado para la matriz de parámetros debe ser una sola expresión que es implícitamente convertible ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo de matriz de parámetros. En este caso, la matriz de parámetros actúa exactamente como un parámetro de valor.
*  Cuando se invoca un miembro de función con una matriz de parámetros en su forma expandida, la invocación debe especificar cero o más argumentos posicionales para la matriz de parámetros, donde cada argumento es una expresión que se pueda convertir implícitamente ([implícitas las conversiones](conversions.md#implicit-conversions)) para el tipo de elemento de la matriz de parámetros. En este caso, la invocación crea una instancia del parámetro de tipo de matriz con una longitud correspondiente al número de argumentos, inicializa los elementos de la instancia de matriz con los valores de argumento especificado y usa la instancia de matriz recién creado como real argumento.

Las expresiones de una lista de argumentos siempre se evalúan en el orden en que se escriben. Por lo tanto, el ejemplo
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
genera el resultado
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

Las reglas de covarianza de matriz ([covarianza de matrices](arrays.md#array-covariance)) permiten que un valor de un tipo de matriz `A[]` sea una referencia a una instancia de un tipo de matriz `B[]`, siempre existe una conversión implícita de referencia de `B` a `A`. Debido a estas reglas, cuando un elemento de matriz de un *reference_type* se pasa como un parámetro de referencia o de salida, una comprobación en tiempo de ejecución no es necesaria para asegurarse de que el tipo de elemento real de la matriz es idéntico del parámetro. En el ejemplo
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
la segunda invocación de `F` hace que un `System.ArrayTypeMismatchException` se produce porque el tipo de elemento real de `b` es `string` y no `object`.

Cuando se invoca un miembro de función con una matriz de parámetros en su forma expandida, la invocación se procesa exactamente como si una expresión de creación de matriz con un inicializador de matriz ([expresiones de creación de matriz](expressions.md#array-creation-expressions)) se ha insertado en torno a la parámetros expandidos. Por ejemplo, dada la declaración
```csharp
void F(int x, int y, params object[] args);
```
las siguientes invocaciones de la forma expandida del método
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
corresponden exactamente a
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

En concreto, tenga en cuenta que se crea una matriz vacía cuando no hay ningún argumento para la matriz de parámetros.

Cuando se omiten los argumentos de un miembro de función con parámetros opcionales correspondientes, implícitamente se pasan los argumentos predeterminados de la declaración de miembro de función. Dado que siempre son constantes, la evaluación no afectará el orden de evaluación de los argumentos restantes.

### <a name="type-inference"></a>Inferencia de tipos

Cuando se llama a un método genérico sin especificar argumentos de tipo, un ***inferencia*** proceso intenta inferir los argumentos de tipo para la llamada. La presencia de inferencia de tipos permite una sintaxis más cómoda para usarse para llamar a un método genérico y permite al programador evitar especificar información de tipo redundante. Por ejemplo, dada la declaración de método:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
es posible invocar la `Choose` método sin especificar explícitamente un argumento de tipo:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

A través de la inferencia de tipos, los argumentos de tipo `int` y `string` se determinan a partir de los argumentos al método.

Inferencia de tipos se produce como parte del procesamiento de tiempo de enlace de una invocación de método ([las invocaciones de método](expressions.md#method-invocations)) y tiene lugar antes de los pasos de resolución de sobrecarga de la invocación. Cuando se especifica un grupo de método concreto en una invocación de método y se especifica ningún argumento de tipo como parte de la invocación del método, la inferencia de tipos se aplica a cada método genérico en el grupo de métodos. Si la inferencia de tipos se realiza correctamente, se usan los argumentos de tipo inferidos para determinar los tipos de argumentos para la resolución de sobrecarga subsiguiente. Si la resolución de sobrecarga, elige un método genérico al invocar, los argumentos de tipo se usan como argumentos de tipo real de la invocación. Si se produce un error en la inferencia de tipos para un método concreto, ese método no participa en la resolución de sobrecarga. El error de inferencia de tipos, de por sí, no provoca un error en tiempo de enlace. Sin embargo, a menudo lo conduce a un error en tiempo de enlace, cuando la resolución de sobrecarga, a continuación, no se encontró ningún método aplicable.

Si el número de argumentos proporcionado es diferente del número de parámetros del método, a continuación, inferencia inmediatamente se produce un error. En caso contrario, se supone que el método genérico tiene la siguiente firma:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Con una llamada al método del formulario `M(E1...Em)` la tarea de inferencia de tipos es encontrar los argumentos de tipo único `S1...Sn` para cada uno de los parámetros de tipo `X1...Xn` para que la llamada `M<S1...Sn>(E1...Em)` pasa a ser válido.

Durante el proceso de inferencia de cada parámetro de tipo `Xi` sea *fijo* a un tipo determinado `Si` o *tipo unfixed* con un conjunto de asociados *límites*. Cada uno de los límites es algún tipo `T`. Cada variable de tipo inicialmente `Xi` es de tipo unfixed con un conjunto vacío de límites.

Inferencia de tipos realiza en fases. Cada fase intentará deducir los argumentos de tipo para las variables de tipo más según las conclusiones de la fase anterior. La primera fase hace algunos inferencias iniciales de los límites indicados, mientras que la segunda fase corrige las variables de tipo a tipos específicos y deduce aún más los límites. Puede tener la segunda fase se repite un número de veces.

*Nota:* Tipo inferencia se lleva a cabo no solo cuando se llama a un método genérico. Inferencia de tipos para la conversión de grupos de métodos se describe en [inferencia de tipos para la conversión de grupos de métodos](expressions.md#type-inference-for-conversion-of-method-groups) y encontrar el mejor tipo común de un conjunto de expresiones se describe en [encontrar el mejor tipo común de un conjunto expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).

#### <a name="the-first-phase"></a>La primera fase

Para cada uno de los argumentos del método `Ei`:

*   Si `Ei` es una función anónima, un *inferencia de tipos de parámetro explícito* ([inferencias de tipo de parámetro explícito](expressions.md#explicit-parameter-type-inferences)) se realiza desde `Ei` a `Ti`
*   De lo contrario, si `Ei` tiene un tipo `U` y `xi` es un parámetro de valor un *inferencia de límite inferior* estará *desde* `U` *a* `Ti`.
*   De lo contrario, si `Ei` tiene un tipo `U` y `xi` es un `ref` o `out` parámetro una *exacta inferencia* se realiza *desde* `U` *a* `Ti`.
*   En caso contrario, no se realiza ninguna inferencia para este argumento.


#### <a name="the-second-phase"></a>La segunda fase

La segunda fase procede como sigue:

*   Todos los *tipo unfixed* variables de tipo `Xi` cuáles no *dependen* ([dependencia](expressions.md#dependence)) cualquier `Xj` son fijos ([fijación](expressions.md#fixing)).
*   Si no existe estas variables de tipo, todos *tipo unfixed* variables de tipo `Xi` son *fijo* para el que se mantenga todos los elementos siguientes:
    *   Hay al menos una variable de tipo `Xj` que depende `Xi`
    *   `Xi` tiene un conjunto no vacío de límites
*   Si no existen estas variables de tipo y todavía hay *tipo unfixed* variables de tipo, se produce un error de inferencia de tipo.
*   En caso contrario, si ningún otro *tipo unfixed* existen variables de tipo, la inferencia de tipos se realiza correctamente.
*   En caso contrario, para todos los argumentos `Ei` con el tipo de parámetro correspondiente `Ti` donde el *tipos de salida* ([tipos de salida](expressions.md#output-types)) contienen *tipo unfixed* variables de tipo `Xj` pero la *tipos de entrada* ([tipos de entrada](expressions.md#input-types)) no lo hace, un *inferencia de salida* ([inferencias de tipo de salida ](expressions.md#output-type-inferences)) se realiza *desde* `Ei` *a* `Ti`. A continuación, se repite la segunda fase.

#### <a name="input-types"></a>Tipos de entrada

Si `E` es un grupo de método o función anónima con tipo implícito y `T` es un delegado de tipo o tipo de árbol de expresión, a continuación, todos los tipos de parámetro de `T` son *tipos de entrada* de `E` *con tipo* `T`.

####  <a name="output-types"></a>Tipos de salida

Si `E` es un grupo de métodos o una función anónima y `T` es un delegado de tipo o tipo de árbol de expresión y el tipo de valor devuelto de `T` es un *tipo de salida* `E` *con tipo*  `T`.

#### <a name="dependence"></a>Dependencia

Un *tipo unfixed* variable de tipo `Xi` *depende directamente* una variable de tipo unfixed tipo `Xj` if para algunos argumentos `Ek` con tipo `Tk` `Xj` se produce en un *tipo de entrada* de `Ek` con tipo `Tk` y `Xi` se produce en un *tipo de salida* de `Ek` con tipo `Tk`.

`Xj` *depende de* `Xi` si `Xj` *depende directamente* `Xi` o si `Xi` *depende directamente* `Xk` y `Xk` *depende* `Xj`. Por lo tanto "depende" es el cierre transitivo pero no reflexivo de "depende directamente".

#### <a name="output-type-inferences"></a>Inferencias de tipo de salida

Un *inferencia de salida* estará *desde* una expresión `E` *a* un tipo `T` de la manera siguiente:

*  Si `E` es una función anónima con el tipo de valor devuelto deducido `U` ([tipo de valor devuelto deducido](expressions.md#inferred-return-type)) y `T` es un tipo de delegado o el tipo de árbol de expresión con tipo de valor devuelto `Tb`, a continuación, un *inferencia de límite inferior* ([inferencias de límite inferior](expressions.md#lower-bound-inferences)) se realiza *desde* `U` *a* `Tb`.
*  De lo contrario, si `E` es un grupo de métodos y `T` es un tipo de delegado o el tipo de árbol de expresión con tipos de parámetro `T1...Tk` y tipo de valor devuelto `Tb`y la resolución de sobrecarga de `E` con los tipos de `T1...Tk` da como resultado una método con tipo de valor devuelto único `U`, un *inferencia de límite inferior* estará *desde* `U` *a* `Tb`.
*  De lo contrario, si `E` es una expresión con tipo `U`, un *inferencia de límite inferior* estará *desde* `U` *a* `T`.
*  En caso contrario, no se realizan inferencias.

#### <a name="explicit-parameter-type-inferences"></a>Inferencias de tipo de parámetro explícito

Un *inferencia de tipos de parámetro explícito* estará *desde* una expresión `E` *a* un tipo `T` de la manera siguiente:

*  Si `E` es una función anónima con tipo explícito con tipos de parámetro `U1...Uk` y `T` es un tipo de delegado o el tipo de árbol de expresión con tipos de parámetro `V1...Vk` , a continuación, para cada `Ui` un *exacta inferencia* ([exacta inferencias](expressions.md#exact-inferences)) se realiza *desde* `Ui` *a* correspondiente `Vi`.

#### <a name="exact-inferences"></a>Inferencias exactas

Un *exacta inferencia* *desde* un tipo `U` *a* un tipo `V` se realiza como se indica a continuación:

*  Si `V` es uno de los *tipo unfixed* `Xi` , a continuación, `U` se agrega al conjunto de límites exactos para `Xi`.

*  En caso contrario, establece `V1...Vk` y `U1...Uk` se determinan mediante la comprobación de si se aplica cualquiera de los casos siguientes:

   *  `V` es un tipo de matriz `V1[...]` y `U` es un tipo de matriz `U1[...]` del mismo rango
   *  `V` es el tipo `V1?` y `U` es el tipo `U1?`
   *  `V` es un tipo construido `C<V1...Vk>`y `U` es un tipo construido `C<U1...Uk>`

   Si cualquiera de estos casos, a continuación, aplicar un *exacta inferencia* se realiza *desde* cada `Ui` *a* correspondiente `Vi`.

*  En caso contrario, no se realizan inferencias.

#### <a name="lower-bound-inferences"></a>Límite inferior inferencias

Un *inferencia de límite inferior* *desde* un tipo `U` *a* un tipo `V` se realiza como sigue:

*  Si `V` es uno de los *tipo unfixed* `Xi` , a continuación, `U` se agrega al conjunto de límites inferiores `Xi`.
*  De lo contrario, si `V` es el tipo `V1?`y `U` es el tipo `U1?` , a continuación, se realiza una inferencia de límite inferior desde `U1` a `V1`.
*  En caso contrario, establece `U1...Uk` y `V1...Vk` se determinan mediante la comprobación de si se aplica cualquiera de los casos siguientes:
   *  `V` es un tipo de matriz `V1[...]` y `U` es un tipo de matriz `U1[...]` (o un parámetro de tipo cuyo tipo base eficaz es `U1[...]`) del mismo rango
   *  `V` es uno de `IEnumerable<V1>`, `ICollection<V1>` o `IList<V1>` y `U` es un tipo de matriz unidimensional `U1[]`(o un parámetro de tipo cuyo tipo base eficaz es `U1[]`)
   *  `V` es un tipo construido class, struct, interfaz o delegado `C<V1...Vk>` y hay un único tipo `C<U1...Uk>` que `U` (o, si `U` es un parámetro de tipo, su clase base efectivo o cualquier miembro de su conjunto de interfaces efectivas) es es idéntica a hereda (directa o indirectamente) o implementa (directa o indirectamente) `C<U1...Uk>`.

      (La restricción "unicidad" significa que, en la interfaz case `C<T> {} class U: C<X>, C<Y> {}`, entonces no se realiza ninguna inferencia al deducir de `U` a `C<T>` porque `U1` podría ser `X` o `Y`.)

   Si se aplica cualquiera de estos casos, a continuación, se realiza una inferencia *desde* cada `Ui` *a* correspondiente `Vi` como sigue:

   *  Si `Ui` no se sabe que es un tipo de referencia una *exacta inferencia* se realiza
   *  De lo contrario, si `U` es un tipo de matriz, a continuación, un *inferencia de límite inferior* se realiza
   *  De lo contrario, si `V` es `C<V1...Vk>` , a continuación, inferencia depende del parámetro de tipo i-ésimo de `C`:
      *  Si es covariante un *inferencia de límite inferior* se realiza.
      *  Si es contravariante una *inferencia de límite superior* se realiza.
      *  Si es invariable una *exacta inferencia* se realiza.
*  En caso contrario, no se realizan inferencias.

#### <a name="upper-bound-inferences"></a>Límite superior inferencias

Un *inferencia de límite superior* *desde* un tipo `U` *a* un tipo `V` se realiza como sigue:

*  Si `V` es uno de los *tipo unfixed* `Xi` , a continuación, `U` se agrega al conjunto de límites superiores `Xi`.
*  En caso contrario, establece `V1...Vk` y `U1...Uk` se determinan mediante la comprobación de si se aplica cualquiera de los casos siguientes:
   *  `U` es un tipo de matriz `U1[...]` y `V` es un tipo de matriz `V1[...]` del mismo rango
   *  `U` es uno de `IEnumerable<Ue>`, `ICollection<Ue>` o `IList<Ue>` y `V` es un tipo de matriz unidimensional `Ve[]`
   *  `U` es el tipo `U1?` y `V` es el tipo `V1?`
   *  `U` es la clase construida, struct, interfaz o delegado tipo `C<U1...Uk>` y `V` es una clase, struct, interfaz o delegado de tipo que es idéntica, hereda (directa o indirectamente) o implementa (directa o indirectamente) un tipo único `C<V1...Vk>`

      (La restricción "unicidad" significa que si tenemos `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, a continuación, no se realiza ninguna inferencia al deducir de `C<U1>` a `V<Q>`. No se realizan deducciones de `U1` como `X<Q>` o `Y<Q>`.)

   Si se aplica cualquiera de estos casos, a continuación, se realiza una inferencia *desde* cada `Ui` *a* correspondiente `Vi` como sigue:
   *  Si `Ui` no se sabe que es un tipo de referencia una *exacta inferencia* se realiza
   *  De lo contrario, si `V` es un tipo de matriz una *inferencia de límite superior* se realiza
   *  De lo contrario, si `U` es `C<U1...Uk>` , a continuación, inferencia depende del parámetro de tipo i-ésimo de `C`:
      *  Si es covariante una *inferencia de límite superior* se realiza.
      *  Si es contravariante un *inferencia de límite inferior* se realiza.
      *  Si es invariable una *exacta inferencia* se realiza.
*  En caso contrario, no se realizan inferencias.   

#### <a name="fixing"></a>Corregir

Un *tipo unfixed* variable de tipo `Xi` con un conjunto de límites es *fijo* como sigue:

*  El conjunto de *tipos candidatos* `Uj` comienza como el conjunto de todos los tipos en el conjunto de límites para `Xi`.
*  A continuación, examinaremos cada límite para `Xi` a su vez: Para cada límite exacto `U` de `Xi` todos los tipos de `Uj` que no son idénticas a `U` se quitan del conjunto de candidatos. Para cada límite inferior `U` de `Xi` todos los tipos de `Uj` al cual se está *no* una conversión implícita de `U` se quitan del conjunto de candidatos. Para cada límite superior `U` de `Xi` todos los tipos de `Uj` desde los cuales hay es *no* una conversión implícita a `U` se quitan del conjunto de candidatos.
*  If entre los tipos de candidatos restantes `Uj` hay un único tipo `V` de que hay una conversión implícita a todos los demás tipos de candidato, a continuación, `Xi` está fija en `V`.
*  En caso contrario, se produce un error en la inferencia de tipo.

#### <a name="inferred-return-type"></a>Tipo de valor devuelto deducido

Deducidos tipo de valor devuelto de una función anónima `F` se usa durante la resolución de sobrecarga y de inferencia de tipos. Solo se puede determinar el tipo de valor devuelto deducido para una función anónima donde todos los parámetros se conocen los tipos, ya sea porque se les asigna explícitamente, proporcionan a través de una conversión de función anónima o deduce durante la inferencia de tipos en envolvente genérico invocación del método.

El ***deduce el tipo de resultado*** se determina como sigue:

*  Si el cuerpo de `F` es un *expresión* que tiene un tipo y, a continuación, el tipo de resultado deducido `F` es el tipo de dicha expresión.
*  Si el cuerpo de `F` es un *bloque* y el conjunto de expresiones en el bloque `return` instrucciones tiene un tipo común mejor `T` ([encontrar el mejor tipo común de un conjunto de expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), a continuación, el tipo de resultado deducido `F` es `T`.
*  En caso contrario, no se puede inferir un tipo de resultado para `F`.

El ***deduce el tipo de valor devuelto*** se determina como sigue:

*  Si `F` es asincrónico y el cuerpo de `F` es una expresión se clasifica como nada ([clasificaciones de expresiones](expressions.md#expression-classifications)), o un bloque de instrucciones donde ninguna instrucción return tiene expresiones, deducidos devuelven es de tipo `System.Threading.Tasks.Task`
*  Si `F` es asincrónico y tiene un tipo de resultado deducido `T`, deducidos devuelven es de tipo `System.Threading.Tasks.Task<T>`.
*  Si `F` es no asincrónicas y tiene un tipo de resultado deducido `T`, deducidos devuelven es de tipo `T`.
*  En caso contrario, no se puede inferir un tipo de valor devuelto para `F`.

Consideremos un ejemplo de inferencia de tipos que implican las funciones anónimas, el `Select` declara el método de extensión en el `System.Linq.Enumerable` clase:
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

Suponiendo que el `System.Linq` se ha importado el espacio de nombres con un `using` cláusula y dada una clase `Customer` con un `Name` propiedad de tipo `string`, el `Select` método puede utilizarse para seleccionar los nombres de una lista de clientes:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

La invocación del método de extensión ([las invocaciones de método de extensión](expressions.md#extension-method-invocations)) de `Select` se procesa al reescribir la invocación a una invocación de método estático:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Puesto que los argumentos de tipo no se especifican explícitamente, inferencia de tipo se usa para inferir los argumentos de tipo. En primer lugar, el `customers` argumento está relacionado con la `source` parámetro, inferir `T` sea `Customer`. A continuación, utilizando la función anónima escriba se ha descrito anteriormente, el proceso de inferencia `c` tiene tipo `Customer`y la expresión `c.Name` está relacionado con el tipo de valor devuelto de la `selector` parámetro, deducir `S` sea `string`. Por lo tanto, es equivalente a la invocación
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
y el resultado es de tipo `IEnumerable<string>`.

El ejemplo siguiente muestra el tipo de función anónima cómo esta característica permite la información de tipo "fluir" entre los argumentos en una invocación de método genérico. Dado que el método:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Inferencia de tipos para la invocación:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
continúa como sigue: Primero, el argumento `"1:15:30"` está relacionado con la `value` parámetro, inferir `X` sea `string`. A continuación, el parámetro de la primera función anónima, `s`, se proporciona el tipo deducido `string`y la expresión `TimeSpan.Parse(s)` está relacionado con el tipo de valor devuelto de `f1`, deducir `Y` sea `System.TimeSpan`. Por último, el parámetro de la segunda función anónima, `t`, se proporciona el tipo deducido `System.TimeSpan`y la expresión `t.TotalSeconds` está relacionado con el tipo de valor devuelto de `f2`, deducir `Z` sea `double`. Por lo tanto, el resultado de la invocación es de tipo `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Inferencia de tipos para la conversión de grupos de métodos

Al igual que las llamadas de métodos genéricos, inferencia de tipos también se debe aplicar cuando un grupo de métodos `M` que contiene un método genérico se convierte en un tipo de delegado dado `D` ([conversiones de métodos de grupo](conversions.md#method-group-conversions)). Dado un método
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
y el grupo de métodos `M` que se asigna al tipo de delegado `D` la tarea de inferencia de tipos es encontrar los argumentos de tipo `S1...Sn` para que la expresión:
```csharp
M<S1...Sn>
```
se vuelve compatible ([declaraciones de delegado](delegates.md#delegate-declarations)) con `D`.

A diferencia del algoritmo de inferencia de tipo para las llamadas de método genérico, en este caso hay sólo argumento *tipos*, ningún argumento *expresiones*. En concreto, no hay ninguna función anónima y, por tanto, sin necesidad de varias fases de inferencia.

En su lugar, todos los `Xi` se consideran *tipo unfixed*y un *inferencia de límite inferior* estará *desde* cada tipo de argumento `Uj` de `D` *a* correspondiente tipo de parámetro `Tj` de `M`. If para cualquiera de los `Xi` no se encontraron límites, se produce un error en la inferencia de tipos. De lo contrario, todos los `Xi` son *fijo* correspondiente `Si`, que son el resultado de la inferencia de tipos.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Buscar el mejor tipo común de un conjunto de expresiones

En algunos casos, tiene que deducir un conjunto de expresiones de un tipo común. En particular, los tipos de elemento de matrices con tipo implícito y los tipos devueltos de funciones anónimas con *bloque* se encuentran los cuerpos de esta manera.

De manera intuitiva, dado un conjunto de expresiones `E1...Em` este inferencia debe ser equivalente a llamar a un método
```csharp
Tr M<X>(X x1 ... X xm)
```
con el `Ei` como argumentos.

Más concretamente, la inferencia comienza con un *tipo unfixed* variable de tipo `X`. *Salida tipo inferencias* , a continuación, se realizan *desde* cada `Ei` *a* `X`. Por último, `X` es *fijo* y, si se realiza correctamente, el resultado escriba `S` es el mejor tipo común resultante para las expresiones. Si no existe ese `S` existe, las expresiones no tienen ningún tipo común mejor.

### <a name="overload-resolution"></a>Resolución de sobrecarga

La resolución de sobrecarga es un mecanismo de enlace de tiempo para seleccionar el mejor miembro de función que puede invocarse dados una lista de argumentos y un conjunto de miembros de función candidatos. La resolución de sobrecarga selecciona el miembro de función para invocar en los siguientes contextos distintos dentro de C#:

*  Invocación de un método llamado en un *invocation_expression* ([las invocaciones de método](expressions.md#method-invocations)).
*  Invocación de un constructor de instancia con nombre en un *object_creation_expression* ([expresiones de creación de objeto](expressions.md#object-creation-expressions)).
*  Invocación de un descriptor de acceso de indexador a través de un *element_access* ([acceso de elemento](expressions.md#element-access)).
*  Invocación de un operador predefinido o definidos por el usuario al que hace referencia en una expresión ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution) y [de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)).

Cada uno de estos contextos define el conjunto de miembros de función candidatos y la lista de argumentos en su propia manera única, como se describe en detalle en las secciones anteriores. Por ejemplo, el conjunto de candidatos para una invocación de método no incluye los métodos marcados `override` ([búsqueda de miembros](expressions.md#member-lookup)), y los métodos en una clase base no son candidatos cualquier método en una clase derivada que se apliquen ([ Las invocaciones de método](expressions.md#method-invocations)).

Una vez que se han identificado los miembros de función candidatos y la lista de argumentos, la selección del mejor miembro de función es el mismo en todos los casos:

*  Dado el conjunto de miembros de función candidatos aplicables, el mejor miembro de función conjunto se encuentra. Si el conjunto contiene a sólo un miembro de función, ese miembro de función es el mejor miembro de función. En caso contrario, el mejor miembro de función es el miembro de una función que es mejor que todos los demás miembros de función con respecto a la lista de argumentos determinada, siempre que cada miembro de función se compara con todos los demás miembros de función mediante las reglas de [ Mejor miembro de función](expressions.md#better-function-member). Si no hay exactamente un miembro de función que es mejor que todos los demás miembros de función, a continuación, la invocación de miembros de función es ambigua y se produce un error en tiempo de enlace.

Las secciones siguientes definen el significado exacto de los términos ***miembro de función aplicable*** y ***mejor miembro de función***.

#### <a name="applicable-function-member"></a>Miembro de función aplicable

Un miembro de función se dice que un ***miembro de función aplicable*** con respecto a una lista de argumentos `A` cuando todas las opciones siguientes son verdaderas:

*  Cada argumento de `A` corresponde a un parámetro en la declaración de miembro de función, como se describe en [correspondientes parámetros](expressions.md#corresponding-parameters), y cualquier parámetro a la que se corresponde ningún argumento es un parámetro opcional.
*  Para cada argumento de `A`, el modo del argumento de paso de parámetros (es decir, el valor, `ref`, o `out`) es idéntico al modo de pasar parámetros del parámetro correspondiente, y
   *  para un parámetro de valor o una matriz de parámetros, una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde el argumento al tipo del parámetro correspondiente, o
   *  para un `ref` o `out` parámetro, el tipo del argumento es idéntico al tipo del parámetro correspondiente. Después de todo, un `ref` o `out` parámetro es un alias para el argumento pasado.

Para un miembro de función que incluye una matriz de parámetros, el miembro de función que se apliquen las reglas anteriores, se dice que sean aplicables a su ***forma normal***. Si un miembro de función que incluye una matriz de parámetros no es aplicable en su forma normal, el miembro de función puede ser aplicable en su ***expandido formulario***:

*  La forma expandida se construye mediante la sustitución de la matriz de parámetros en la declaración de miembro de función con cero o más parámetros de valor del tipo de elemento del parámetro de matriz, que el número de argumentos en la lista de argumentos `A` coincide con el total número de parámetros. Si `A` tiene menos argumentos que el número de parámetros fijos en la declaración de miembro de función, no se puede construir de la forma expandida del miembro de función y, por tanto, no es aplicable.
*  En caso contrario, es aplicable si para cada argumento de la forma expandida `A` el modo de pasar parámetros del argumento es idéntico al modo de pasar parámetros del parámetro correspondiente, y
   *  para un parámetro de valor fijo o un parámetro de valor creados por la expansión, una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde el tipo del argumento al tipo del parámetro correspondiente, o
   *  para un `ref` o `out` parámetro, el tipo del argumento es idéntico al tipo del parámetro correspondiente.

#### <a name="better-function-member"></a>Mejor miembro de función

Para los fines de determinar al mejor miembro de función, una lista de argumentos reducida A se construye que contiene solo las expresiones de argumentos a sí mismos en el orden en que aparecen en la lista de argumentos original.

Listas de parámetros para cada una de las funciones candidatas se construyen los miembros de la manera siguiente:

*  Si el miembro de función se aplica sólo en la forma expandida, se usa la forma expandida.
*  Parámetros opcionales con ningún argumento correspondiente se quitan de la lista de parámetros
*  Se puede reordenar los parámetros para que se produzcan en la misma posición que el argumento correspondiente en la lista de argumentos.

Dada una lista de argumentos `A` con un conjunto de expresiones de argumento `{E1, E2, ..., En}` y dos miembros de función aplicable `Mp` y `Mq` con tipos de parámetro `{P1, P2, ..., Pn}` y `{Q1, Q2, ..., Qn}`, `Mp` se define como un ***mejor miembro de función*** que `Mq` si

*  para cada argumento, la conversión implícita de `Ex` a `Qx` no es mejor que la conversión implícita de `Ex` a `Px`, y
*  para al menos un argumento, la conversión de `Ex` a `Px` es mejor que la conversión de `Ex` a `Qx`.

Al realizar esta evaluación, si `Mp` o `Mq` es aplicable en su forma expandida, a continuación, `Px` o `Qx` hace referencia a un parámetro en la forma expandida de la lista de parámetros.

En caso de que el parámetro de tipo secuencias `{P1, P2, ..., Pn}` y `{Q1, Q2, ..., Qn}` son equivalentes (es decir, cada uno `Pi` tiene una conversión de identidad correspondiente `Qi`), se aplican las siguientes reglas de desempate, en orden, para determinar la mejor miembro de función.

*  Si `Mp` es un método no genérico y `Mq` es un método genérico, a continuación, `Mp` es mejor que `Mq`.
*  De lo contrario, si `Mp` es aplicable en su forma normal y `Mq` tiene un `params` de matriz y, a continuación, se aplica sólo en su forma expandida, `Mp` es mejor que `Mq`.
*  De lo contrario, si `Mp` ha declarado más parámetros que `Mq`, a continuación, `Mp` es mejor que `Mq`. Esto puede ocurrir si ambos métodos tienen `params` matrices y se aplica sólo en sus formas expandidas.
*  En caso contrario si todos los parámetros de `Mp` tiene un argumento correspondiente, mientras que los argumentos predeterminados deben sustituirse por al menos un parámetro opcional en `Mq` , a continuación, `Mp` es mejor que `Mq`.
*  De lo contrario, si `Mp` tiene tipos de parámetro más específicos que `Mq`, a continuación, `Mp` es mejor que `Mq`. Permiten `{R1, R2, ..., Rn}` y `{S1, S2, ..., Sn}` representan los tipos de parámetros no expandidos y sin instancias de `Mp` y `Mq`. `Mp`de tipos de parámetro son más específicos que `Mq`de if, para cada parámetro, `Rx` no es menos específico que `Sx`y, para al menos un parámetro `Rx` es más específico que `Sx`:
   *  Un parámetro de tipo es menos específico que un parámetro sin tipo.
   *  De forma recursiva, un tipo construido es más específico que otro tipo construido (con el mismo número de argumentos de tipo) si al menos un argumento de tipo es más específico y ningún argumento de tipo es menos específico que el argumento de tipo correspondiente en el otro.
   *  Un tipo de matriz es más específico que otro tipo de matriz (con el mismo número de dimensiones) si el tipo de elemento de la primera es más específico que el tipo de elemento del segundo.
*  En caso contrario, si un miembro es un operador no elevado y el otro es un operador de elevación, el no elevado es mejor.
*  De lo contrario, ningún miembro de función es mejor.

#### <a name="better-conversion-from-expression"></a>Mejor conversión de expresión

Dada una conversión implícita `C1` que convierte a partir de una expresión `E` a un tipo `T1`y una conversión implícita `C2` que convierte a partir de una expresión `E` a un tipo `T2`, `C1` es un ***mejor conversión*** que `C2` si `E` coincide exactamente `T2` y suspensiones de al menos uno de los siguientes:

* `E` coincide exactamente con `T1` ([que coincidan exactamente con la expresión](expressions.md#exactly-matching-expression))
* `T1` es un destino de conversión mejor que `T2` ([mejor destino de la conversión](expressions.md#better-conversion-target))

#### <a name="exactly-matching-expression"></a>Expresión que coincide exactamente

Dada una expresión `E` y un tipo `T`, `E` coincide exactamente con `T` si uno de los siguientes contiene:

*  `E` tiene un tipo `S`, y existe una conversión de identidad de `S` a `T`
*  `E` es una función anónima, `T` es un tipo de delegado `D` o un tipo de árbol de expresión `Expression<D>` y suspensiones de uno de los siguientes:
   *  Un tipo de valor devuelto deducido `X` existe para `E` en el contexto de la lista de parámetros `D` ([tipo de valor devuelto deducido](expressions.md#inferred-return-type)), y existe una conversión de identidad de `X` para el tipo de valor devuelto `D`
   *  Cualquier `E` es que no es asincrónico y `D` tiene un tipo de valor devuelto `Y` o `E` es asincrónico y `D` tiene un tipo de valor devuelto `Task<Y>`, y uno de los siguientes contiene:
      * El cuerpo de `E` es una expresión que coincida exactamente con `Y`
      * El cuerpo de `E` es un bloque de instrucciones donde cada instrucción return devuelve una expresión que coincida exactamente con `Y`

#### <a name="better-conversion-target"></a>Destino de conversión mejor

Dados dos tipos diferentes `T1` y `T2`, `T1` es un destino de conversión mejor que `T2` si ninguna conversión implícita de `T2` a `T1` existe, y al menos uno de los siguientes contiene:

*  Una conversión implícita de `T1` a `T2` existe
*  `T1` es un tipo de delegado `D1` o un tipo de árbol de expresión `Expression<D1>`, `T2` es un tipo de delegado `D2` o un tipo de árbol de expresión `Expression<D2>`, `D1` tiene un tipo de valor devuelto `S1` y uno de los contiene las siguientes:
   * `D2` Devuelve void
   * `D2` tiene un tipo de valor devuelto `S2`, y `S1` es un destino de conversión mejor que `S2`
*  `T1` es `Task<S1>`, `T2` es `Task<S2>`, y `S1` es un destino de conversión mejor que `S2`
*  `T1` es `S1` o `S1?` donde `S1` es un tipo entero con signo, y `T2` es `S2` o `S2?` donde `S2` es un tipo entero sin signo. De manera específica:
   * `S1` es `sbyte` y `S2` es `byte`, `ushort`, `uint`, o `ulong`
   * `S1` es `short` y `S2` es `ushort`, `uint`, o `ulong`
   * `S1` es `int` y `S2` es `uint`, o `ulong`
   * `S1` es `long` y `S2` es `ulong`

#### <a name="overloading-in-generic-classes"></a>Sobrecarga en clases genéricas

Mientras que las firmas declaradas deben ser únicas, es posible que la sustitución de argumentos de tipo da lugar a firmas idénticas. En tales casos, las reglas de desempate de la resolución de sobrecarga anterior seleccionará al miembro más específico.

Los ejemplos siguientes muestran las sobrecargas que son válidos y no válidos según esta regla:

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación

Para las operaciones enlazadas más dinámicamente el conjunto de posibles candidatos para la resolución es desconocido en tiempo de compilación. Sin embargo en algunos casos, el conjunto de candidatos se conoce en tiempo de compilación:

*  Llamadas a métodos estáticos con argumentos dinámicos
*  Llamadas de método de instancia donde el receptor no es una expresión dinámica
*  Llamadas de indizador donde el receptor no es una expresión dinámica
*  Llamadas de constructor con argumentos dinámicos

En estos casos se realiza una comprobación de tiempo de compilación limitada para que cada candidato ver si alguno de ellos, posiblemente, podría aplicar en tiempo de ejecución. Esta comprobación se compone de los pasos siguientes:

*  Inferencia de tipo parcial: Cualquier tipo de argumento que no dependen directa o indirectamente un argumento de tipo `dynamic` se deduce usando las reglas de [inferencia de tipo](expressions.md#type-inference). Los argumentos de tipo restantes son desconocidos.
*  Comprobación de aplicabilidad parcial: Se comprueba la aplicabilidad según [miembro de función aplicable](expressions.md#applicable-function-member), pero se omiten los parámetros cuyos tipos son desconocidas.
*  Si ningún candidato pasa esta prueba, se produce un error en tiempo de compilación.

### <a name="function-member-invocation"></a>Invocación de función miembro

En esta sección se describe el proceso que tiene lugar en tiempo de ejecución para invocar a un miembro de función determinada. Se supone que un proceso en tiempo de enlace ya ha determinado el miembro que se invoca, posiblemente mediante la aplicación a un conjunto de miembros de función candidatos de resolución de sobrecarga determinado.

Para fines de describir el proceso de invocación, los miembros de función se dividen en dos categorías:

*  Miembros de la función estática. Estos son los constructores de instancias, los métodos estáticos, los descriptores de acceso de propiedad estática y los operadores definidos por el usuario. Miembros de función estáticos siempre son no virtuales.
*  Miembros de función de instancia. Estos son los métodos de instancia, los descriptores de acceso de propiedad de instancia y descriptores de acceso de indizador. Miembros de función de instancia son no virtuales o virtual y siempre se invocan en una instancia determinada. La instancia se calcula mediante una expresión de instancia, y se convierte en accesible dentro del miembro de función como `this` ([este acceso](expressions.md#this-access)).

El procesamiento en tiempo de ejecución de una invocación de miembros de función consta de los pasos siguientes, donde `M` es el miembro de función y, si `M` es un miembro de instancia, `E` es la expresión de instancia:

*  Si `M` es un miembro de función estático:
   * La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).
   * `M` se invoca.

*  Si `M` se declara un miembro de función de la instancia en un *value_type*:
   * `E` se evalúa. Si esta evaluación provoca una excepción, no se ejecuta ningún paso adicional.
   * Si `E` no está clasificado como una variable y, a continuación, una variable local temporal de `E`del tipo se crea y el valor de `E` se asigna a esa variable. `E` a continuación, se reclasifica como una referencia a la variable local temporal. La variable temporal es accesible como `this` en `M`, pero no en ninguna otra forma. Por lo tanto, solo cuando `E` es una variable true permite al llamador observar los cambios que `M` hace `this`.
   * La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).
   * `M` se invoca. La variable al que hace referencia `E` se convierte en la variable al que hace referencia `this`.

*  Si `M` se declara un miembro de función de la instancia en un *reference_type*:
   * `E` se evalúa. Si esta evaluación provoca una excepción, no se ejecuta ningún paso adicional.
   * La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).
   * Si el tipo de `E` es un *value_type*, una conversión boxing ([conversiones Boxing](types.md#boxing-conversions)) se realiza para convertir `E` escriba `object`, y `E` se considera para ser de tipo `object` en los pasos siguientes. En este caso, `M` sólo podría ser un miembro de `System.Object`.
   * El valor de `E` se comprueba para ser válido. Si el valor de `E` es `null`, un `System.NullReferenceException` se inicia y no se ejecuta ningún paso adicional.
   * Se determina la implementación del miembro de función para invocar:
     * Si el enlace de tipo en tiempo de `E` es una interfaz, el miembro de función para invocar es la implementación de `M` proporcionada por el tipo de tiempo de ejecución de la instancia que hace referencia `E`. Este miembro de función se determina aplicando las reglas de asignación de interfaz ([asignación de interfaz](interfaces.md#interface-mapping)) para determinar la implementación de `M` proporcionada por el tipo de tiempo de ejecución de la instancia que hace referencia `E`.
     * De lo contrario, si `M` es un miembro de función virtual, el miembro de función para invocar es la implementación de `M` proporcionada por el tipo de tiempo de ejecución de la instancia que hace referencia `E`. Este miembro de función se determina aplicando las reglas para determinar la implementación más derivada ([métodos virtuales](classes.md#virtual-methods)) de `M` en relación con el tipo de tiempo de ejecución de la instancia que hace referencia `E`.
     * En caso contrario, `M` es un miembro de función no virtual, y es el miembro de función para invocar `M` propio.
   * Se invoca la implementación del miembro de función determinada en el paso anterior. El objeto al que hace referencia `E` se convierte en el objeto al que hace referencia `this`.

#### <a name="invocations-on-boxed-instances"></a>Invocaciones en instancias de conversión boxing

Un miembro de función se implementa en un *value_type* pueden invocarse a través de una instancia con conversión boxing de los que *value_type* en las situaciones siguientes:

*  Cuando el miembro de función es un `override` de un método heredado de tipo `object` y se invoca a través de una expresión de instancia de tipo `object`.
*  Cuando el miembro de función es una implementación de un miembro de función de la interfaz y se invoca a través de una expresión de instancia de un *interface_type*.
*  Cuando se invoca el miembro de función a través de un delegado.

En estas situaciones, se considera la instancia de la conversión boxing para contener una variable de la *value_type*, y esta variable se convierte en la variable al que hace referencia `this` dentro de la invocación de miembros de función. En concreto, esto significa que cuando un miembro de función se invoca en una instancia de la conversión boxing, es posible que el miembro de función modificar el valor contenido en la instancia.

## <a name="primary-expressions"></a>Expresiones primarias

Expresiones primarias incluyen las formas más sencillas de expresiones.

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

Expresiones primarias se dividen entre *array_creation_expression*s y *primary_no_array_creation_expression*s. Tratamiento de expresión de creación de matriz de esta manera, en lugar de enumerarlas junto con las otras formas de expresión simple, permite que la gramática para no permitir código potencialmente confuso, como
```csharp
object o = new int[3][1];
```
lo que de lo contrario se interpretaría como
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Literales

Un *primary_expression* que consta de un *literal* ([literales](lexical-structure.md#literals)) se clasifica como un valor.


### <a name="interpolated-strings"></a>Cadenas interpoladas

Un *interpolated_string_expression* consta de un `$` signo seguida de una cadena literal, normal o textual en agujeros, delimitadas por `{` y `}`, incluya expresiones y formato especificaciones. Una expresión de cadena interpolada es el resultado de una *interpolated_string_literal* que se ha dividido en tokens individuales, como se describe en [interpoladas literales de cadena](lexical-structure.md#interpolated-string-literals).

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

El *constant_expression* en una interpolación debe tener una conversión implícita a `int`.

Un *interpolated_string_expression* se clasifica como un valor. Si se convierte inmediatamente en `System.IFormattable` o `System.FormattableString` con una conversión implícita de cadena interpolada ([implícitas interpolan conversiones de cadenas](conversions.md#implicit-interpolated-string-conversions)), la expresión de cadena interpolada tiene ese tipo. En caso contrario, tiene el tipo `string`.

Si el tipo de una cadena interpolada es `System.IFormattable` o `System.FormattableString`, el significado es una llamada a `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Si el tipo es `string`, el significado de la expresión es una llamada a `string.Format`. En ambos casos, la lista de argumentos de la llamada consta de una cadena de formato literal con marcadores de posición para cada interpolación y un argumento para cada expresión correspondiente a los marcadores de posición.

El literal de cadena de formato se construye como sigue, donde `N` es el número de interpolaciones en el *interpolated_string_expression*:

*  Si un *interpolated_regular_string_whole* o un *interpolated_verbatim_string_whole* sigue el `$` iniciar sesión, a continuación, el literal de cadena de formato es ese token.
*  En caso contrario, el literal de cadena de formato consta de: 
   *  Primera la *interpolated_regular_string_start* o *interpolated_verbatim_string_start*
   *  A continuación, para cada número `I` desde `0` a `N-1`: 
      * La representación decimal de `I`
      * A continuación, si el correspondiente *interpolación* tiene un *constant_expression*, un `,` (coma) seguido de la representación decimal del valor de la *constant_expression*
      * El *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* o *interpolated_ verbatim_string_end* inmediatamente después de la interpolación correspondiente.

Los argumentos subsiguientes son simplemente el *expresiones* desde el *interpolaciones* (si existe), en orden.

TODO: ejemplos.


### <a name="simple-names"></a>Nombres simples

Un *simple_name* consta de un identificador, seguido opcionalmente por una lista de argumentos de tipo:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

Un *simple_name* sea del formulario `I` o tiene el formato `I<A1,...,Ak>`, donde `I` es un identificador único y `<A1,...,Ak>` es una función opcional *type_argument_list*. Cuando no hay ninguna *type_argument_list* está especificado, considere la posibilidad de `K` sea cero. El *simple_name* se evalúa y clasifica como sigue:

*  Si `K` es cero y el *simple_name* aparece dentro de un *bloque* y si el *bloque*de (o envolvente *bloque*del) local espacio de declaración de variable ([declaraciones](basic-concepts.md#declarations)) contiene una variable local, parámetro o constante con nombre `I`, el *simple_name* hace referencia a la variable local, parámetro o una constante y se clasifica como una variable o un valor.
*  Si `K` es cero y el *simple_name* aparece dentro del cuerpo de una declaración de método genérico y si dicha declaración incluye un parámetro de tipo con nombre `I`, el *simple_name*hace referencia a ese parámetro de tipo.
*  En caso contrario, para cada tipo de instancia `T` ([el tipo de instancia](classes.md#the-instance-type)), empezando por el tipo de instancia de la declaración de tipo envolvente inmediato y continuando con el tipo de instancia de cada clase o estructura envolvente declaración (si existe):
   *  Si `K` es cero y la declaración de `T` incluye un parámetro de tipo con nombre `I`, el *simple_name* hace referencia a ese parámetro de tipo.
   *  En caso contrario, si una búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)) de `I` en `T` con `K`  argumentos de tipo produce una coincidencia:
      * Si `T` es el tipo de instancia del tipo de clase o estructura envolvente inmediata y la búsqueda identifica uno o más métodos, el resultado es un grupo de métodos con una expresión de instancia asociada de `this`. Si se ha especificado una lista de argumentos de tipo, se utiliza en una llamada a un método genérico ([las invocaciones de método](expressions.md#method-invocations)).
      * De lo contrario, si `T` es el tipo de instancia del tipo struct o clase inmediatamente envolvente, si la búsqueda identifica un miembro de instancia, y si se produce la referencia dentro del cuerpo de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia, el resultado es el mismo que un acceso a miembros ([acceso a miembros](expressions.md#member-access)) del formulario `this.I`. Esto solo puede ocurrir cuando `K` es cero.
      * En caso contrario, el resultado es el mismo que un acceso a miembros ([acceso a miembros](expressions.md#member-access)) del formulario `T.I` o `T.I<A1,...,Ak>`. En este caso, es un error en tiempo de enlace para el *simple_name* para hacer referencia a un miembro de instancia.

*  En caso contrario, para cada espacio de nombres `N`, empezando por el espacio de nombres en el que el *simple_name* se produce, continúe con cada uno de los espacios de nombres (si existe) envolventes y terminando con el espacio de nombres global, son los pasos siguientes evalúa hasta que se encuentra una entidad:
   *  Si `K` es cero y `I` es el nombre de un espacio de nombres en `N`, a continuación:
      * Si la ubicación donde el *simple_name* se produce entre una declaración de espacio de nombres para `N` y la declaración de espacio de nombres contiene un *extern_alias_directive* o  *using_alias_directive* que asocia el nombre `I` con un espacio de nombres o tipo, el *simple_name* es ambiguo y se produce un error en tiempo de compilación.
      * En caso contrario, el *simple_name* hace referencia al espacio de nombres denominado `I` en `N`.
   *  De lo contrario, si `N` contiene un tipo accesible con nombre `I` y `K`  parámetros de tipo:
      * Si `K` es cero y la ubicación donde el *simple_name* se produce entre una declaración de espacio de nombres para `N` y la declaración de espacio de nombres contiene un *extern_alias_directive*o *using_alias_directive* que asocia el nombre `I` con un espacio de nombres o tipo, el *simple_name* es ambiguo y se produce un error en tiempo de compilación.
      * En caso contrario, el *namespace_or_type_name* hace referencia al tipo construido con los argumentos de tipo especificado.
   *  De lo contrario, si la ubicación donde el *simple_name* se produce entre una declaración de espacio de nombres para `N`:
      * Si `K` es cero y la declaración de espacio de nombres contiene un *extern_alias_directive* o *using_alias_directive* que asocia el nombre `I` con un espacio de nombres importado o tipo, el *simple_name* hace referencia a dicho espacio de nombres o tipo.
      * En caso contrario, si las declaraciones de espacios de nombres y el tipo importan por el *using_namespace_directive*s y *using_static_directive*s de la declaración de espacio de nombres contiene exactamente un tipo accesible o con el nombre de miembro estático sin extensión `I` y `K`  parámetros de tipo, el *simple_name* hace referencia a ese tipo o miembro construido con los argumentos de tipo especificado.
      * En caso contrario, si los tipos y espacios de nombres importan por el *using_namespace_directive*s de la declaración de espacio de nombres contiene más de un tipo accesible o método de extensión que no son miembro estático con el nombre `I` y `K`  parámetros de tipo, el *simple_name* es ambiguo y se produce un error.

   Tenga en cuenta que este paso es paralelo exactamente con el paso correspondiente en el procesamiento de un *namespace_or_type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)).

*  En caso contrario, el *simple_name* es no definido y se produce un error de tiempo de compilación.


### <a name="parenthesized-expressions"></a>Expresiones entre paréntesis

Un *parenthesized_expression* consta de un *expresión* entre paréntesis.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

Un *parenthesized_expression* se evalúa mediante la evaluación de la *expresión* entre paréntesis. Si el *expresión* entre paréntesis denota un espacio de nombres o tipo, se produce un error de tiempo de compilación. En caso contrario, el resultado de la *parenthesized_expression* es el resultado de la evaluación de los contenidos *expresión*.

### <a name="member-access"></a>Acceso a miembros

Un *member_access* consta de un *primary_expression*, un *predefined_type*, o un *qualified_alias_member*, seguido de un"`.`"símbolo (token), seguido de un *identificador*, seguido opcionalmente un *type_argument_list*.

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

El *qualified_alias_member* producción se define en [calificadores de alias Namespace](namespaces.md#namespace-alias-qualifiers).

Un *member_access* sea del formulario `E.I` o tiene el formato `E.I<A1, ..., Ak>`, donde `E` es una expresión primaria, `I` es un identificador único y `<A1, ..., Ak>` es una función opcional  *type_argument_list*. Cuando no hay ninguna *type_argument_list* está especificado, considere la posibilidad de `K` sea cero.

Un *member_access* con un *primary_expression* typu `dynamic` se enlazan dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso el compilador clasifica el acceso a miembros como acceso a una propiedad de tipo `dynamic`. Las reglas siguientes para determinar el significado de la *member_access* , a continuación, se aplican en tiempo de ejecución, mediante el tipo de tiempo de ejecución en lugar del tipo de tiempo de compilación de la *primary_expression*. Si esta clasificación de tiempo de ejecución da lugar a un grupo de métodos, el acceso a miembros debe ser el *primary_expression* de un *invocation_expression*.

El *member_access* se evalúa y clasifica como sigue:

*  Si `K` es cero y `E` es un espacio de nombres y `E` contiene un espacio de nombres anidado con nombre `I`, el resultado será ese espacio de nombres.
*  De lo contrario, si `E` es un espacio de nombres y `E` contiene un tipo accesible con nombre `I` y `K`  parámetros de tipo, el resultado será ese tipo construido con los argumentos de tipo especificado.
*  Si `E` es un *predefined_type* o un *primary_expression* clasificada como un tipo, si `E` no es un parámetro de tipo y si una búsqueda de miembros ([debúsquedademiembros](expressions.md#member-lookup)) de `I` en `E` con `K`  parámetros de tipo produce una coincidencia, a continuación, `E.I` se evalúa y clasifica como sigue:
   *  Si `I` identifica un tipo, el resultado será ese tipo construido con los argumentos de tipo especificado.
   *  Si `I` identifica uno o varios métodos, entonces el resultado es un grupo de métodos sin expresión de instancia asociada. Si se ha especificado una lista de argumentos de tipo, se utiliza en una llamada a un método genérico ([las invocaciones de método](expressions.md#method-invocations)).
   *  Si `I` identifica un `static` propiedad, el resultado es un acceso de propiedad sin expresión de instancia asociada.
   *  Si `I` identifica un `static` campo:
      * Si el campo es `readonly` y la referencia se produce fuera del constructor estático de la clase o estructura en la que se declara el campo, entonces el resultado es un valor, es decir, el valor del campo estático `I` en `E`.
      * En caso contrario, el resultado es una variable, es decir, el campo estático `I` en `E`.
   *  Si `I` identifica un `static` eventos:
      * Si la referencia se produce dentro de la clase o estructura en el que se declara el evento y el evento se ha declarado sin *event_accessor_declarations* ([eventos](classes.md#events)), a continuación, `E.I` se procesa exactamente como si `I` fuera un campo estático.
      * En caso contrario, el resultado es un acceso de eventos sin expresión de instancia asociada.
   *  Si `I` identifica una constante, entonces el resultado es un valor, es decir, el valor de la constante.
    * Si `I` identifica un miembro de enumeración, entonces el resultado es un valor, es decir, el valor de ese miembro de enumeración.
    * En caso contrario, `E.I` es una referencia de miembro no válido, y se produce un error de tiempo de compilación.
*  Si `E` es acceso a la propiedad, indizador, variable o valor, el tipo de los cuales es `T`y una búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)) de `I` en `T` con `K`  argumentos de tipo produce una coincidencia, a continuación, `E.I` se evalúa y clasifica como sigue:
   *  Primero, if `E` es una propiedad o indizador, el valor de la propiedad o se obtiene acceso al indizador ([valores de expresiones](expressions.md#values-of-expressions)) y `E` es reclasificar como un valor.
   *  Si `I` identifica uno o varios métodos, entonces el resultado es un grupo de métodos con una expresión de instancia asociada de `E`. Si se ha especificado una lista de argumentos de tipo, se utiliza en una llamada a un método genérico ([las invocaciones de método](expressions.md#method-invocations)).
   *  Si `I` identifica una propiedad de instancia
      * Si `E` es `this`, `I` identifica una propiedad implementada automáticamente ([implementa automáticamente propiedades](classes.md#automatically-implemented-properties)) sin un establecedor y la referencia ocurre dentro de un constructor de instancia para un tipo de clase o struct `T`, entonces el resultado es una variable, es decir, el campo de respaldo oculto para la propiedad automática proporcionada por `I` en la instancia de `T` proporcionado por `this`.
      * En caso contrario, el resultado es un acceso de propiedad con una expresión de instancia asociada de `E`.
   *  Si `T` es un *class_type* y `I` identifica un campo de instancia de ese *class_type*:
      * Si el valor de `E` es `null`, un `System.NullReferenceException` se produce.
      * En caso contrario, si el campo es `readonly` y la referencia se produce fuera de un constructor de instancia de la clase en el que se declara el campo, entonces el resultado es un valor, es decir, el valor del campo `I` en el objeto al que hace referencia `E`.
      * En caso contrario, el resultado es una variable, es decir, el campo `I` en el objeto al que hace referencia `E`.
   *  Si `T` es un *struct_type* y `I` identifica un campo de instancia de ese *struct_type*:
      * Si `E` es un valor, o si el campo es `readonly` y la referencia se produce fuera de un constructor de instancia de la estructura en el que se declara el campo, entonces el resultado es un valor, es decir, el valor del campo `I` en la instancia de estructura proporcionada por  `E`.
      * En caso contrario, el resultado es una variable, es decir, el campo `I` en la instancia de estructura proporcionada por `E`.
   *  Si `I` identifica un evento de instancia:
      * Si la referencia se produce dentro de la clase o estructura en el que se declara el evento y el evento se ha declarado sin *event_accessor_declarations* ([eventos](classes.md#events)), y la referencia no se realizan como la lado izquierdo de un `+=` o `-=` operador y, a continuación, `E.I` se procesa exactamente como si `I` era un campo de instancia.
      * En caso contrario, el resultado es un acceso de eventos con una expresión de instancia asociada de `E`.
*  En caso contrario, se realiza un intento para procesar `E.I` como una invocación de método de extensión ([las invocaciones de método de extensión](expressions.md#extension-method-invocations)). Si se produce un error, `E.I` es una referencia de miembro no válido, y se produce un error en tiempo de enlace.

#### <a name="identical-simple-names-and-type-names"></a>Idénticos nombres simples y nombres de tipo

En un acceso de miembro del formulario `E.I`si `E` es un identificador único y si el significado de `E` como un *simple_name* ([nombres simples](expressions.md#simple-names)) es una constante, campo, propiedad, variable local o un parámetro con el mismo tipo que el significado de `E` como un *type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)), a continuación, dos significados posibles de `E` son permitido. Los dos significados posibles de `E.I` nunca son ambiguos, desde `I` necesariamente debe ser miembro del tipo `E` en ambos casos. En otras palabras, la regla sencillamente permite el acceso a los miembros estáticos y los tipos anidados de `E` en tiempo de compilación en caso contrario, habría producido un error. Por ejemplo:
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

#### <a name="grammar-ambiguities"></a>Ambigüedades de gramática

Las producciones de *simple_name* ([nombres simples](expressions.md#simple-names)) y *member_access* ([acceso a miembros](expressions.md#member-access)) puede dar lugar a ambigüedades en la gramática de expresiones. Por ejemplo, la instrucción:
```
F(G<A,B>(7));
```
se puede interpretar como una llamada a `F` con dos argumentos, `G < A` y `B > (7)`. Como alternativa, podría interpretarse como una llamada a `F` con un argumento, que es una llamada a un método genérico `G` con dos argumentos de tipo y un argumento normal.

Si se puede analizar una secuencia de tokens (en contexto) como un *simple_name* ([nombres simples](expressions.md#simple-names)), *member_access* ([acceso a miembros](expressions.md#member-access)), o *pointer_member_access* ([acceso a miembros de puntero](unsafe-code.md#pointer-member-access)) termina con un *type_argument_list* ([argumentos de tipo](types.md#type-arguments)), el símbolo (token) inmediatamente después del cierre `>` se examina el token. Si es uno de
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
el *type_argument_list* se conserva como parte de la *simple_name*, *member_access* o *pointer_member_access* y cualquier otro posible análisis de la secuencia de tokens se descartan. En caso contrario, el *type_argument_list* no se considera parte de la *simple_name*, *member_access* o *pointer_member_access*, incluso si no hay ningún otro análisis posible de la secuencia de tokens. Tenga en cuenta que no son de estas reglas se aplica al analizar un *type_argument_list* en un *namespace_or_type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)). La instrucción
```csharp
F(G<A,B>(7));
```
según esta regla, se interpretará como una llamada a `F` con un argumento, que es una llamada a un método genérico `G` con dos argumentos de tipo y un argumento normal. Las instrucciones
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
cada uno se interpretará como una llamada a `F` con dos argumentos. La instrucción
```csharp
x = F < A > +y;
```
se interpretará como un operador menor que, mayor que el operador y operador unario, como si hubiese escrita la instrucción `x = (F < A) > (+y)`, en lugar de como un *simple_name* con un *type_argument_list* seguido de un operador más binario. En la instrucción
```csharp
x = y is C<T> + z;
```
los tokens `C<T>` se interpretan como un *namespace_or_type_name* con un *type_argument_list*.

### <a name="invocation-expressions"></a>Expresiones de invocación

Un *invocation_expression* se utiliza para invocar un método.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

Un *invocation_expression* se enlazan dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) si la contiene al menos uno de los siguientes:

* El *primary_expression* tiene el tipo de tiempo de compilación `dynamic`.
* Al menos un argumento de opcional *argument_list* tiene el tipo de tiempo de compilación `dynamic` y *primary_expression* no tiene un tipo de delegado.

En este caso el compilador clasifica el *invocation_expression* como un valor de tipo `dynamic`. Las reglas siguientes para determinar el significado de la *invocation_expression* , a continuación, se aplican en tiempo de ejecución, mediante el tipo de tiempo de ejecución en lugar del tipo de tiempo de compilación de los de la *primary_expression* y argumentos que tienen el tipo de tiempo de compilación `dynamic`. Si el *primary_expression* no tiene el tipo de tiempo de compilación `dynamic`, a continuación, la invocación del método se somete a una comprobación en tiempo de compilación limitada según se describe en [comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

El *primary_expression* de un *invocation_expression* debe ser un grupo de métodos o un valor de un *delegate_type*. Si el *primary_expression* es un grupo de métodos, el *invocation_expression* es una invocación de método ([las invocaciones de método](expressions.md#method-invocations)). Si el *primary_expression* es un valor de un *delegate_type*, *invocation_expression* es una invocación de delegado ([invocacionesdedelegado](expressions.md#delegate-invocations)). Si el *primary_expression* no es un grupo de métodos ni un valor de un *delegate_type*, se produce un error en tiempo de enlace.

El elemento opcional *argument_list* ([listas de argumentos](expressions.md#argument-lists)) proporciona valores o referencias de variables para los parámetros del método.

El resultado de evaluar una *invocation_expression* se clasifica como sigue:

*  Si el *invocation_expression* invoca un método o delegado que devuelve `void`, el resultado es nothing. Una expresión que se clasifica como nada sólo se permite en el contexto de un *statement_expression* ([las instrucciones de expresión](statements.md#expression-statements)) o como el cuerpo de un *lambda_expression*([Expresiones de función anónima](expressions.md#anonymous-function-expressions)). En caso contrario, se produce un error en tiempo de enlace.
*  En caso contrario, el resultado es un valor del tipo devuelto por el método o delegado.

#### <a name="method-invocations"></a>Invocaciones de método

Para una invocación de método, el *primary_expression* de la *invocation_expression* debe ser un grupo de métodos. El grupo de métodos identifica el método que se invoca o el conjunto de métodos sobrecargados para elegir un método específico para invocar. En el último caso, la determinación del método concreto que se invoca se basa en el contexto proporcionado por los tipos de los argumentos de la *argument_list*.

El procesamiento en tiempo de enlace de una invocación de método del formulario `M(A)`, donde `M` es un grupo de métodos (incluidos posiblemente un *type_argument_list*), y `A` es una función opcional *argument_ lista*, consta de los pasos siguientes:

*  Se construye el conjunto de métodos de candidatos para la invocación del método. Para cada método `F` asociado con el grupo de métodos `M`:
   *  Si `F` es genérica, `F` es un candidato cuando:
      * `M` no tiene ninguna lista de argumentos de tipo, y
      * `F` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).
   *  Si `F` es genérico y `M` no tiene ninguna lista de argumentos de tipo `F` es un candidato cuando:
      * Inferencia de tipos ([inferencia](expressions.md#type-inference)) se realiza correctamente, deduciendo una lista de argumentos de tipo para la llamada, y
      * Una vez que los argumentos de tipo deducido se sustituyen por los parámetros de tipo del método correspondiente, todos los tipos construidos en la lista de parámetros de F cumplen sus restricciones ([que satisfacen las restricciones](types.md#satisfying-constraints)) y la lista de parámetros de `F` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).
   *  Si `F` es genérico y `M` incluye una lista de argumentos de tipo `F` es un candidato cuando:
      * `F` tiene el mismo número de parámetros de tipo de método que se proporcionaron en la lista de argumentos de tipo, y
      * Una vez que los argumentos de tipo se sustituyen por los parámetros de tipo del método correspondiente, todos los tipos construidos en la lista de parámetros de F cumplen sus restricciones ([que satisfacen las restricciones](types.md#satisfying-constraints)) y la lista de parámetros `F` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).
*  El conjunto de métodos candidatos se reduce para que contenga solo los métodos de los tipos más derivados: Para cada método `C.F` en el conjunto, donde `C` es el tipo en el que el método `F` se declara, todos los métodos declaran en un tipo base de `C` se quitan del conjunto. Además, si `C` es distinto de un tipo de clase `object`, se quitan todos los métodos declarados en un tipo de interfaz del conjunto. (Esta última regla sólo tiene efecto cuando el grupo de métodos fue el resultado de una búsqueda de miembros en un parámetro de tipo que tiene una clase base efectiva que no sean de objeto y un conjunto de interfaces efectivas vacío).
*  Si el conjunto de métodos candidatos resultante está vacío, se abandona el procesamiento posterior a lo largo de los pasos siguientes y, en su lugar, se realiza un intento para procesar la invocación de una invocación de método de extensión ([invocaciones de método de extensión](expressions.md#extension-method-invocations)). Si se produce un error, a continuación, no existen métodos aplicables, y se produce un error en tiempo de enlace.
*  El mejor método para el conjunto de métodos candidatos se identifica mediante las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution). Si no se puede identificar un método mejor, la invocación del método es ambigua y se produce un error en tiempo de enlace. Al realizar la resolución de sobrecarga, se consideran los parámetros de un método genérico después de sustituir los argumentos de tipo (suministrados o inferidos) para los parámetros de tipo del método correspondiente.
*  Se realiza una validación final del método elegido mejor:
   * El método se valida en el contexto del grupo de método: Si el método mejor es un método estático, el grupo de métodos debe haberse obtenido de un *simple_name* o un *member_access* a través de un tipo. Si el método mejor es un método de instancia, el grupo de métodos debe haberse obtenido de un *simple_name*, un *member_access* a través de una variable o un valor, o un *base_access*. Si ninguno de estos requisitos es true, se produce un error en tiempo de enlace.
   * Si el método mejor es un método genérico, los argumentos de tipo (suministrados o inferidos) se comprueban con las restricciones ([que satisfacen las restricciones](types.md#satisfying-constraints)) declarados en el método genérico. Si cualquier argumento de tipo no satisface las restricciones correspondientes en el parámetro de tipo, se produce un error en tiempo de enlace.

Una vez que ha seleccionado un método y validado en tiempo de enlace por los pasos anteriores, la invocación de tiempo de ejecución real se procesa según las reglas de invocación de miembros de función se describe en [comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

El efecto intuitivo de las reglas de resolución que se ha descrito anteriormente es como sigue: Para buscar el método concreto invocado por una invocación de método, comience con el tipo indicado por la invocación del método y se continúa en la cadena de herencia hasta que se encuentra la declaración de al menos un método aplicable, accesible y no de reemplazo. A continuación, realizar inferencia de tipos y en el conjunto de métodos aplicables, accesibles y no de reemplazo declarados en dicho tipo de resolución de sobrecarga e invocar el método, por tanto, se selecciona. Si se encontró ningún método, pruebe en su lugar procesar la invocación de una invocación de método de extensión.

#### <a name="extension-method-invocations"></a>Invocaciones de método de extensión

En una invocación de método ([invocaciones en instancias de conversión boxing](expressions.md#invocations-on-boxed-instances)) de uno de los formularios
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
Si el procesamiento normal de la invocación encuentra ningún método aplicable, se realiza un intento para procesar la construcción de una invocación de método de extensión. Si *expr* o cualquiera de los *args* tiene el tipo de tiempo de compilación `dynamic`, no se aplicarán métodos de extensión.

El objetivo es encontrar el mejor *type_name* `C`, de modo que la invocación del método estático correspondiente puede tener lugar:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Un método de extensión `Ci.Mj` es ***aptos*** si:

*  `Ci` es una clase genérica, no anidadas
*  El nombre de `Mj` es *identificador*
*  `Mj` es accesible y es aplicable cuando se aplica a los argumentos como un método estático, como se muestra arriba
*  Existe una identidad implícitos, referencia o una conversión boxing desde *expr* para el tipo del primer parámetro de `Mj`.

La búsqueda de `C` continúa como sigue:

*  A partir de la declaración espacio de nombres envolvente más cercana, continuando con cada declaración de espacio de nombres envolvente y termina con la unidad de compilación que lo contiene, se realizan intentos sucesivos para buscar un conjunto de candidatos de métodos de extensión:
   * Si la unidad de compilación o espacio de nombres determinada directamente contiene las declaraciones de tipo no genérico `Ci` con métodos de extensión aptos `Mj`, a continuación, el conjunto de estos métodos de extensión es el conjunto de candidatos.
   * Si tipos `Ci` importados por *using_static_declarations* y declara directamente en los espacios de nombres importados por *using_namespace_directive*s en la compilación o espacio de nombres de unidad especificada directamente contiene métodos de extensión aptos `Mj`, a continuación, el conjunto de estos métodos de extensión es el conjunto de candidatos.
*  Si no se encuentra ningún conjunto de candidato en cualquier unidad de compilación o de declaración de espacio de nombres envolvente, se produce un error en tiempo de compilación.
*  En caso contrario, se aplica la resolución de sobrecarga como se describe en la versión candidata ([de resolución de sobrecarga](expressions.md#overload-resolution)). Si no se encuentra ningún método mejor único, se produce un error en tiempo de compilación.
*  `C` es el tipo dentro del cual el mejor método se declara como un método de extensión.

Uso de `C` como un destino, la llamada al método, a continuación, se procesa como una invocación de método estático ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Las reglas anteriores significan que los métodos de instancia tienen prioridad sobre los métodos de extensión, que los métodos de extensión disponibles en las declaraciones de espacio de nombres interna tienen prioridad sobre los métodos de extensión disponibles en las declaraciones de espacio de nombres exterior y esa extensión los métodos declarados directamente en un espacio de nombres tienen prioridad sobre los métodos de extensión que se importan en ese mismo espacio de nombres con el uso de una directiva de espacio de nombres. Por ejemplo:
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

En el ejemplo, `B`del método tiene prioridad sobre el primer método de extensión, y `C`del método tiene prioridad sobre ambos métodos de extensión.

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

El resultado de este ejemplo es:
```
E.F(1)
D.G(2)
C.H(3)
```
`D.G` tiene prioridad sobre `C.G`, y `E.F` tiene prioridad sobre ambos `D.F` y `C.F`.

#### <a name="delegate-invocations"></a>Invocaciones del delegado

Para una invocación de delegado, la *primary_expression* de la *invocation_expression* debe ser un valor de un *delegate_type*. Además, teniendo en cuenta la *delegate_type* sea un miembro de función con la misma lista de parámetros como el *delegate_type*, el *delegate_type* debe ser aplicable () [Miembro de función aplicable](expressions.md#applicable-function-member)) con respecto a la *argument_list* de la *invocation_expression*.

El procesamiento en tiempo de ejecución de una invocación de delegado del formulario `D(A)`, donde `D` es un *primary_expression* de un *delegate_type* y `A` es un opcional*argument_list*, consta de los pasos siguientes:

*  `D` se evalúa. Si esta evaluación, produce una excepción, no se ejecuta ningún paso adicional.
*  El valor de `D` se comprueba para ser válido. Si el valor de `D` es `null`, un `System.NullReferenceException` se inicia y no se ejecuta ningún paso adicional.
*  En caso contrario, `D` es una referencia a una instancia de delegado. Invocaciones de miembro de función ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se realizan en cada una de las entidades que se puede llamar en la lista de invocaciones del delegado. Para las entidades que se puede llamar que consta de una instancia y el método de instancia, la instancia para la invocación es la instancia de la entidad que se puede llamar.

### <a name="element-access"></a>Acceso de elemento

Un *element_access* consta de un *primary_no_array_creation_expression*, seguido de un "`[`" símbolo (token), seguido de un *argument_list*, seguido de un " `]`"token. El *argument_list* consta de uno o varios *argumento*s, separados por comas.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

El *argument_list* de un *element_access* no puede contener `ref` o `out` argumentos.

Un *element_access* se enlazan dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) si la contiene al menos uno de los siguientes:

* El *primary_no_array_creation_expression* tiene el tipo de tiempo de compilación `dynamic`.
* Al menos una expresión de la *argument_list* tiene el tipo de tiempo de compilación `dynamic` y *primary_no_array_creation_expression* no tiene un tipo de matriz.

En este caso el compilador clasifica el *element_access* como un valor de tipo `dynamic`. Las reglas siguientes para determinar el significado de la *element_access* , a continuación, se aplican en tiempo de ejecución, mediante el tipo de tiempo de ejecución en lugar del tipo de tiempo de compilación de los de la *primary_no_array_creation_expression*y *argument_list* expresiones que tienen el tipo de tiempo de compilación `dynamic`. Si el *primary_no_array_creation_expression* no tiene el tipo de tiempo de compilación `dynamic`, a continuación, el acceso de elemento se somete a una comprobación en tiempo de compilación limitada según se describe en [comprobación dinámico en tiempo de compilación resolución de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Si el *primary_no_array_creation_expression* de un *element_access* es un valor de un *array_type*, *element_access* es un acceso de la matriz ([matriz acceso](expressions.md#array-access)). En caso contrario, el *primary_no_array_creation_expression* debe ser una variable o un valor de una clase, struct o tipo de interfaz que tiene uno o más miembros de indizador, en cuyo caso el *element_access* es un acceso de indizador ([acceso al indizador](expressions.md#indexer-access)).

#### <a name="array-access"></a>Acceso a la matriz

Para un acceso a la matriz, el *primary_no_array_creation_expression* de la *element_access* debe ser un valor de un *array_type*. Además, el *argument_list* de una matriz no se permite el acceso que contiene los argumentos con nombre. El número de expresiones en el *argument_list* debe ser el mismo que el rango de la *array_type*, y cada expresión debe ser de tipo `int`, `uint`, `long`, `ulong`, o debe poder convertirse implícitamente a uno o varios de estos tipos.

El resultado de evaluar un acceso a la matriz es una variable del tipo de elemento de la matriz, es decir, el elemento de matriz seleccionado por los valores de las expresiones de la *argument_list*.

El procesamiento en tiempo de ejecución de un acceso a la matriz del formulario `P[A]`, donde `P` es un *primary_no_array_creation_expression* de un *array_type* y `A` es un *argument_list*, consta de los pasos siguientes:

*  `P` se evalúa. Si esta evaluación, produce una excepción, no se ejecuta ningún paso adicional.
*  Las expresiones de índice de la *argument_list* se evalúan en orden, de izquierda a derecha. Después de la evaluación de cada expresión de índice, una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) se realiza en uno de los siguientes tipos: `int`, `uint`, `long`, `ulong`. Se elige el primer tipo de esta lista para el que existe una conversión implícita. Por ejemplo, si la expresión de índice es de tipo `short` , a continuación, una conversión implícita a `int` se realiza desde conversiones implícitas de `short` a `int` y desde `short` a `long` son posibles. Si la evaluación de una expresión de índice o la conversión implícita posterior causa una excepción, no hay otras expresiones de índice se evalúan y ninguna otra se ejecutan los pasos.
*  El valor de `P` se comprueba para ser válido. Si el valor de `P` es `null`, un `System.NullReferenceException` se inicia y no se ejecuta ningún paso adicional.
*  El valor de cada expresión en el *argument_list* se compara con los límites reales de cada dimensión de la instancia de matriz al que hace referencia `P`. Si uno o más valores que están fuera del intervalo, un `System.IndexOutOfRangeException` se inicia y no se ejecuta ningún paso adicional.
*  Se calcula la ubicación del elemento de matriz especificado por las expresiones de índice, y esta ubicación es el resultado de acceso a la matriz.

#### <a name="indexer-access"></a>Acceso al indizador

Para un acceso de indizador, el *primary_no_array_creation_expression* de la *element_access* debe ser una variable o un valor de una clase, estructura o tipo de interfaz, y este tipo debe implementar uno o más los indizadores que se aplican con respecto a la *argument_list* de la *element_access*.

El procesamiento en tiempo de enlace de un acceso de indizador del formulario `P[A]`, donde `P` es un *primary_no_array_creation_expression* de una clase, estructura o tipo de interfaz `T`, y `A` es un *argument_list*, consta de los pasos siguientes:

*  El conjunto de indizadores suministrados por `T` se construye. El conjunto formado por todos los indizadores declarados en `T` o de un tipo base `T` que no son `override` declaraciones y son accesibles en el contexto actual ([acceso a miembros](basic-concepts.md#member-access)).
*  El conjunto se reduce a aquellos indizadores que son aplicables y no ocultos por otros indizadores. Las siguientes reglas se aplican a cada indizador `S.I` en el conjunto, donde `S` es el tipo en el que el indizador `I` se ha declarado:
   * Si `I` no es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)), a continuación, `I` se quita del conjunto.
   * Si `I` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)), a continuación, todos los indizadores se declaran en un tipo base de `S` se quitan del conjunto.
   * Si `I` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)) y `S` es distinto de un tipo de clase `object`, se quitan todos los indizadores declarados en una interfaz del conjunto.
*  Si el conjunto resultante de indizadores candidatos está vacío, a continuación, no existen indizadores aplicables, y se produce un error en tiempo de enlace.
*  El mejor indizador del conjunto de indizadores candidatos se identifica mediante las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution). Si no se puede identificar un único indexador mejor, el acceso de indizador es ambiguo y se produce un error en tiempo de enlace.
*  Las expresiones de índice de la *argument_list* se evalúan en orden, de izquierda a derecha. El resultado de procesar el acceso de indizador es una expresión que se clasifica como un acceso de indizador. La expresión de acceso de indizador hace referencia al indizador determinado en el paso anterior y tiene una expresión de instancia asociada de `P` y una lista de argumentos de `A`.

Dependiendo del contexto en el que se utiliza, un acceso de indizador hace que la invocación de uno de ellos la *descriptor de acceso get* o *descriptor de acceso set* del indizador. Si el acceso de indizador es el destino de una asignación, el *descriptor de acceso set* se invoca para asignar un nuevo valor ([asignación Simple](expressions.md#simple-assignment)). En todos los demás casos, el *descriptor de acceso get* se invoca para obtener el valor actual ([valores de expresiones](expressions.md#values-of-expressions)).

### <a name="this-access"></a>Este acceso

Un *this_access* consta de una palabra reservada `this`.

```antlr
this_access
    : 'this'
    ;
```

Un *this_access* se permite únicamente en el *bloque* de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia. Tiene uno de los siguientes significados:

*  Cuando `this` se usa en un *primary_expression* dentro de un constructor de instancia de una clase, se clasifica como un valor. El tipo del valor es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la clase dentro del cual se produce el uso y el valor es una referencia al objeto que se está construyendo.
*  Cuando `this` se usa en un *primary_expression* dentro de un método de instancia o un descriptor de acceso de instancia de una clase, se clasifica como un valor. El tipo del valor es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la clase dentro del cual se produce el uso y el valor es una referencia al objeto para el que se invocó el método o el descriptor de acceso.
*  Cuando `this` se usa en un *primary_expression* dentro de un constructor de instancia de una estructura, se clasifica como una variable. El tipo de la variable es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la estructura dentro del cual se produce el uso y la variable representa la estructura que se está construyendo. El `this` variable de un constructor de instancia de una estructura comporta exactamente igual que un `out` parámetro del tipo struct, en concreto, esto significa que la variable debe asignarlo definitivamente en cada ruta de acceso de ejecución de la instancia constructor.
*  Cuando `this` se usa en un *primary_expression* dentro de un método de instancia o un descriptor de acceso de instancia de una estructura, se clasifica como una variable. El tipo de la variable es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la estructura dentro del cual se produce el uso.
   * Si el método o el descriptor de acceso no es un iterador ([iteradores](classes.md#iterators)), el `this` variable representa la estructura para el que el método o el descriptor de acceso se invocó y se comporta exactamente igual que un `ref` parámetro del tipo struct.
   * Si el método o el descriptor de acceso es un iterador, la `this` variable representa una copia de la estructura para el que el método o el descriptor de acceso se invocó y se comporta exactamente igual que un parámetro de valor del tipo struct.

El uso de `this` en un *primary_expression* en un contexto distinto de los mencionados anteriormente, es un error en tiempo de compilación. En concreto, no es posible hacer referencia a `this` en un método estático, un descriptor de acceso de propiedad estática, o en un *variable_initializer* de una declaración de campo.

### <a name="base-access"></a>Acceso base

Un *base_access* consta de una palabra reservada `base` seguida un "`.`" token y un identificador o un *argument_list* entre corchetes:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

Un *base_access* se usa para tener acceso a miembros de clase base que están ocultos de forma similar a los miembros con nombre en la clase actual o el struct. Un *base_access* se permite únicamente en el *bloque* de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia. Cuando `base.I` se produce en una clase o struct, `I` debe denotar un miembro de la clase base de esa clase o struct. Del mismo modo, cuando `base[E]` se produce en una clase, debe existir un indizador aplicable en la clase base.

En tiempo de enlace, *base_access* expresiones del formulario `base.I` y `base[E]` se evalúan exactamente como si se escribieron `((B)this).I` y `((B)this)[E]`, donde `B` es la clase de la clase base o el struct en el que se produce la construcción. Por lo tanto, `base.I` y `base[E]` corresponden a `this.I` y `this[E]`, excepto `this` se considera como una instancia de la clase base.

Cuando un *base_access* hace referencia a un miembro de función virtual (método, propiedad o indizador), la determinación de los cuales función miembro que desea invocar en tiempo de ejecución ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se cambia. El miembro de función que se invoca se determina mediante la búsqueda de la implementación más derivada ([métodos virtuales](classes.md#virtual-methods)) del miembro de función con respecto a `B` (en lugar de en relación con el tipo de tiempo de ejecución de `this`, como es habitual en un acceso no son de base). Por lo tanto, dentro de un `override` de un `virtual` miembro de función, un *base_access* puede usarse para invocar la implementación heredada del miembro de función. Si el miembro de función al que hace referencia un *base_access* es abstracto, se produce un error en tiempo de enlace.

### <a name="postfix-increment-and-decrement-operators"></a>Operadores postfijos de incremento y decremento

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

El operando de un postfijo de incremento o decremento operación debe ser una expresión que se clasifica como una variable, el acceso a una propiedad o un acceso de indizador. El resultado de la operación es un valor del mismo tipo como operando.

Si el *primary_expression* tiene el tipo de tiempo de compilación `dynamic` , a continuación, el operador se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)), el *post_increment_expression*o *post_decrement_expression* tiene el tipo de tiempo de compilación `dynamic` y las reglas siguientes se aplican en tiempo de ejecución utilizando el tipo de tiempo de ejecución de la *primary_expression*.

Si el operando de un sufijo incrementa u operación de decremento es una propiedad o acceso de indizador, la propiedad o indizador debe tener tanto un `get` y un `set` descriptor de acceso. Si esto no es el caso, se produce un error en tiempo de enlace.

Resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. Predefinidos `++` y `--` operadores existen para los siguientes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`y cualquier tipo de enumeración. Predefinido `++` operadores devuelven el valor generado al sumar 1 al operando y predefinido `--` operadores devuelven el valor generado al restar 1 del operando. En un `checked` contexto, si el resultado de esta suma o resta está fuera del intervalo del tipo del resultado y el tipo de resultado es un tipo entero o un tipo de enumeración, un `System.OverflowException` se produce.

El procesamiento en tiempo de ejecución de un incremento de postfijo o disminuir la operación de la forma `x++` o `x--` consta de los pasos siguientes:

*   Si `x` se clasifica como una variable:
    * `x` se evalúa para producir la variable.
    * El valor de `x` se guarda.
    * Se invoca el operador seleccionado con el valor guardado de `x` como su argumento.
    * El valor devuelto por el operador se almacena en la ubicación dada por la evaluación de `x`.
    * El valor guardado de `x` se convierte en el resultado de la operación.
*   Si `x` se clasifica como un acceso de propiedad o indizador:
    * La expresión de instancia (si `x` no `static`) y la lista de argumentos (si `x` es un acceso de indizador) asociadas con `x` se evalúan, y los resultados se usan en la siguiente `get` y `set` invocaciones de descriptor de acceso.
    * El `get` descriptor de acceso de `x` se invoca y se guarda el valor devuelto.
    * Se invoca el operador seleccionado con el valor guardado de `x` como su argumento.
    * El `set` descriptor de acceso de `x` se invoca con el valor devuelto por el operador como su `value` argumento.
    * El valor guardado de `x` se convierte en el resultado de la operación.

El `++` y `--` operadores también admiten la notación de prefijo ([prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)). Normalmente, el resultado de `x++` o `x--` es el valor de `x` antes de la operación, mientras que el resultado de `++x` o `--x` es el valor de `x` después de la operación. En cualquier caso, `x` sí tiene el mismo valor después de la operación.

Un `operator ++` o `operator --` implementación puede invocarse mediante la notación de prefijo o sufijo. No es posible tener implementaciones de operador independientes para las dos notaciones.

### <a name="the-new-operator"></a>Operador new

El `new` operador se usa para crear nuevas instancias de tipos.

Hay tres formas de `new` expresiones:

*  Expresiones de creación de objetos se utilizan para crear nuevas instancias de tipos de clases y tipos de valor.
*  Expresiones de creación de matriz se utilizan para crear nuevas instancias de tipos de matriz.
*  Expresiones de creación de delegado se utilizan para crear nuevas instancias del delegado de tipos.

El `new` implica la creación de una instancia de un tipo de operador, pero no implica necesariamente una asignación dinámica de memoria. En concreto, las instancias de tipos de valor no requieren más memoria más allá de las variables en el que residen y no hay asignaciones dinámicas se producen cuando `new` se usa para crear instancias de tipos de valor.

#### <a name="object-creation-expressions"></a>Expresiones de creación de objetos

Un *object_creation_expression* se utiliza para crear una nueva instancia de un *class_type* o un *value_type*.

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

El *tipo* de un *object_creation_expression* debe ser un *class_type*, un *value_type* o un *tipo_parámetro* . El *tipo* no puede ser un `abstract` *class_type*.

Opcional *argument_list* ([listas de argumentos](expressions.md#argument-lists)) sólo está permitida si el *tipo* es un *class_type* o un *struct_ tipo*.

Una expresión de creación de objeto puede omitir la lista de argumentos de constructor y envolventes paréntesis proporcionan incluye un inicializador de objeto o un inicializador de colección. Si se omite la lista de argumentos de constructor e incluya entre paréntesis son equivalente a especificar una lista de argumentos vacía.

Procesamiento de una expresión de creación de objetos que incluye un inicializador de objeto o un inicializador de colección consta de procesamiento en primer lugar el constructor de instancia y después procesando las inicializaciones de miembro o el elemento especificadas por el inicializador de objeto ([ Inicializadores de objeto](expressions.md#object-initializers)) o un inicializador de colección ([inicializadores de colección](expressions.md#collection-initializers)).

Si alguno de los argumentos en el elemento opcional *argument_list* tiene el tipo de tiempo de compilación `dynamic` el *object_creation_expression* se enlazan dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) y las reglas siguientes se aplican en tiempo de ejecución con el tipo de tiempo de ejecución de esos argumentos de la *argument_list* que tienen el tipo de tiempo de compilación `dynamic`. Sin embargo, la creación del objeto se somete a una comprobación en tiempo de compilación limitada según se describe en [comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

El procesamiento en tiempo de enlace de un *object_creation_expression* del formulario `new T(A)`, donde `T` es un *class_type* o un *value_type* y `A` es una función opcional *argument_list*, consta de los pasos siguientes:

*   Si `T` es un *value_type* y `A` no está presente:
    * El *object_creation_expression* es una invocación del constructor predeterminado. El resultado de la *object_creation_expression* es un valor de tipo `T`, es decir, el valor predeterminado de `T` tal como se define en [System.ValueType el tipo](types.md#the-systemvaluetype-type).
*   De lo contrario, si `T` es un *tipo_parámetro* y `A` no está presente:
    * Si ninguna restricción de tipo de valor o una restricción de constructor ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) se ha especificado para `T`, se produce un error en tiempo de enlace.
    * El resultado de la *object_creation_expression* es un valor del tipo de tiempo de ejecución que se ha enlazado el parámetro de tipo, es decir, el resultado de invocar el constructor predeterminado de ese tipo. El tipo de tiempo de ejecución puede ser un tipo de referencia o un tipo de valor.
*   De lo contrario, si `T` es un *class_type* o un *struct_type*:
    * Si `T` es un `abstract` *class_type*, se produce un error de tiempo de compilación.
    * Para invocar el constructor de instancia se determina mediante las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution). El conjunto de constructores de instancias de candidato consta de todos los constructores de instancia accesible declarados en `T` que son aplicables con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)). Si el conjunto de constructores de instancias de candidato está vacío, o si no se puede identificar un mejor constructor de instancia, se produce un error en tiempo de enlace.
    * El resultado de la *object_creation_expression* es un valor de tipo `T`, es decir, el valor generado al invocar el constructor de instancia determinado en el paso anterior.
*  En caso contrario, el *object_creation_expression* no es válido, y se produce un error en tiempo de enlace.

Incluso si la *object_creation_expression* dinámicamente está enlazado, el tipo de tiempo de compilación sigue siendo `T`.

El procesamiento en tiempo de ejecución de un *object_creation_expression* del formulario `new T(A)`, donde `T` es *class_type* o un *struct_type* y `A` es una función opcional *argument_list*, consta de los pasos siguientes:

*   Si `T` es un *class_type*:
    * Una nueva instancia de la clase `T` está asignada. Si no hay suficiente memoria disponible para asignar la nueva instancia, un `System.OutOfMemoryException` se inicia y no se ejecuta ningún paso adicional.
    * Todos los campos de la nueva instancia se inicializan en sus valores predeterminados ([los valores predeterminados](variables.md#default-values)).
    * Se invoca el constructor de instancia según las reglas de invocación de función miembro ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Una referencia a la instancia recién asignada automáticamente se pasa al constructor de instancia y la instancia se puede acceder desde dentro de ese constructor como `this`.
*   Si `T` es un *struct_type*:
    * Una instancia del tipo `T` se crea mediante la asignación de una variable local temporal. Desde un constructor de instancia de un *struct_type* es necesario para asignar definitivamente un valor para cada campo de la instancia que se va a crear, no es necesaria ninguna inicialización de la variable temporal.
    * Se invoca el constructor de instancia según las reglas de invocación de función miembro ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Una referencia a la instancia recién asignada automáticamente se pasa al constructor de instancia y la instancia se puede acceder desde dentro de ese constructor como `this`.

#### <a name="object-initializers"></a>Inicializadores de objeto

Un ***inicializador de objeto*** especifica los valores de cero o más campos, propiedades o elementos indizados de un objeto.

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

Un inicializador de objeto se compone de una secuencia de inicializadores de miembro, delimitadas por `{` y `}` tokens y separados por comas. Cada *member_initializer* designa un destino para la inicialización. Un *identificador* debe nombrar un campo accesible o una propiedad del objeto que se está inicializando, mientras que un *argument_list* entre corchetes debe especificar los argumentos de un indizador accesible en el objeto que se está inicializando. Es un error en un inicializador de objeto incluir a más de un inicializador de miembro para el mismo campo o propiedad.

Cada *initializer_target* va seguido de un signo igual y una expresión, un inicializador de objeto o un inicializador de colección. No es posible que las expresiones en el inicializador de objeto para hacer referencia al objeto recién creado que se está inicializando.

Un inicializador de miembro que se especifica una expresión después del signo igual se procesa en la misma manera que una asignación ([asignación Simple](expressions.md#simple-assignment)) al destino.

Un inicializador de miembro que especifica un inicializador de objeto después del signo igual es un ***inicializador de objeto anidado***, es decir, una inicialización de un objeto incrustado. En lugar de asignar un nuevo valor al campo o propiedad, las asignaciones en el inicializador de objeto anidado se tratan como asignaciones a los miembros del campo o propiedad. Inicializadores de objeto anidado no pueden aplicarse a las propiedades con un tipo de valor, o a los campos de solo lectura con un tipo de valor.

Un inicializador de miembro que especifica a un inicializador de colección después del signo igual es una inicialización de una recopilación incrustada. En lugar de asignar una nueva colección en el campo de destino, propiedad o indizador, los elementos contenidos en el inicializador se agregan a la colección al que hace referencia el destino. El destino debe ser de un tipo de colección que cumple los requisitos especificados en [inicializadores de colección](expressions.md#collection-initializers).

Los argumentos de un inicializador de índice siempre se evaluará una sola vez. Por lo tanto, incluso si los argumentos terminan nunca acostumbrando (por ejemplo, debido a un inicializador vacío anidado), se evaluará para sus efectos.

La clase siguiente representa un punto con dos coordenadas:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Una instancia de `Point` pueden crearse y se inicializan como sigue:
```csharp
Point a = new Point { X = 0, Y = 1 };
```
que tiene el mismo efecto que
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
donde `__a` es una variable temporal en caso contrario invisible y accesible. La clase siguiente representa un rectángulo que se crea a partir de dos puntos:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Una instancia de `Rectangle` pueden crearse y se inicializan como sigue:
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
que tiene el mismo efecto que
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
donde `__r`, `__p1` y `__p2` son variables temporales que de otro modo invisible y no son accesibles.

Si `Rectangle`del constructor asigna los dos incrustados `Point` instancias
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
la construcción siguiente puede usarse para inicializar el objeto incrustado `Point` instancias en lugar de asignar nuevas instancias:
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
que tiene el mismo efecto que
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

Dada una definición adecuada de C, en el ejemplo siguiente:
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
es equivalente a esta serie de asignaciones:
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
donde `__c`, etc., son variables generadas que son inaccesibles al código fuente y invisible. Tenga en cuenta que los argumentos para `[0,0]` son evalúa solo una vez y los argumentos para `[1,2]` se evalúan una vez, aunque nunca se usan.

#### <a name="collection-initializers"></a>Inicializadores de colección

Un inicializador de colección especifica los elementos de una colección.

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

Un inicializador de colección consta de una secuencia de inicializadores de elemento, delimitadas por `{` y `}` tokens y separados por comas. Cada inicializador de elemento especifica un elemento que se va a agregarse al objeto de colección que se está inicializando y consta de una lista de expresiones delimitadas por `{` y `}` tokens y separados por comas.  Un inicializador de elemento de expresión simple se puede escribir sin llaves, pero, a continuación, no puede ser una expresión de asignación, para evitar la ambigüedad con inicializadores de miembro. El *non_assignment_expression* producción se define en [expresión](expressions.md#expression).

El siguiente es un ejemplo de una expresión de creación de objetos que incluye a un inicializador de colección:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

El objeto de colección al que se aplica un inicializador de colección debe ser de un tipo que implementa `System.Collections.IEnumerable` o se produce un error de tiempo de compilación. Para cada uno especifica el elemento en orden, el inicializador de colección invoca un `Add` método en el destino con la lista de expresiones del inicializador de elementos de objeto como lista de argumentos, aplicación de búsqueda de miembros normales y para cada invocación de resolución de sobrecarga. Por lo tanto, el objeto de colección debe tener un método de instancia o extensión aplicable con el nombre `Add` cada inicializador de elemento.

La clase siguiente representa un contacto con un nombre y una lista de números de teléfono:
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

Un `List<Contact>` pueden crearse y se inicializan como sigue:
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
que tiene el mismo efecto que
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
donde `__clist`, `__c1` y `__c2` son variables temporales que de otro modo invisible y no son accesibles.

#### <a name="array-creation-expressions"></a>Expresiones de creación de matriz

Un *array_creation_expression* se utiliza para crear una nueva instancia de un *array_type*.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Una expresión de creación de matriz del primer formulario asigna una instancia de matriz del tipo que es el resultado de eliminar cada una de las expresiones individuales de la lista de expresiones. Por ejemplo, la expresión de creación de matriz `new int[10,20]` genera una instancia de la matriz de tipo `int[,]`y la expresión de creación de matriz `new int[10][,]` genera una matriz de tipo `int[][,]`. Cada expresión en la lista de expresiones debe ser de tipo `int`, `uint`, `long`, o `ulong`, o se pueden convertir implícitamente a uno o varios de estos tipos. El valor de cada expresión determina la longitud de la dimensión correspondiente en la instancia de matriz recién asignado. Puesto que la longitud de una dimensión de matriz debe ser no negativa, es un error en tiempo de compilación para tener un *constant_expression* con un valor negativo en la lista de expresiones.

Excepto en un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)), el diseño de las matrices no se especifica.

Si una expresión de creación de matriz del primer formulario incluye a un inicializador de matriz, cada expresión en la lista de expresiones debe ser una constante y las longitudes de rango y dimensión especificadas por la lista de expresiones deben coincidir con los del inicializador de matriz.

En una expresión de creación de matriz del segundo o tercer formulario, el rango del especificador de tipo o rango en la matriz especificada debe coincidir con del inicializador de matriz. Las longitudes de dimensión individuales se infieren del número de elementos en cada uno de los niveles de anidamiento correspondientes del inicializador de matriz. Por lo tanto, la expresión
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
corresponde exactamente a
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Una expresión de creación de matriz del tercer formulario se conoce como un ***expresión de creación de matriz con tipo implícito***. Es similar a la segunda forma, salvo que el tipo de elemento de la matriz no es da explícitamente, pero se determina como el tipo más común ([encontrar el mejor tipo común de un conjunto de expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) del conjunto de expresiones de la matriz inicializador. Para una matriz multidimensional, es decir, donde el *rank_specifier* contiene al menos una coma, este conjunto comprende todos *expresión*s encontrado en anidados *array_initializer*s.

Se describe con más detalle en los inicializadores de matriz [inicializadores de matriz](arrays.md#array-initializers).

El resultado de evaluar una expresión de creación de matriz se clasifica como un valor, es decir, una referencia a la instancia de matriz recién asignado. El procesamiento en tiempo de ejecución de una expresión de creación de matriz consta de los pasos siguientes:

*  Las expresiones de longitud de la dimensión de la *expression_list* se evalúan en orden, de izquierda a derecha. Después de la evaluación de cada expresión, una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) se realiza en uno de los siguientes tipos: `int`, `uint`, `long`, `ulong`. Se elige el primer tipo de esta lista para el que existe una conversión implícita. Si la evaluación de una expresión o la conversión implícita posterior causa una excepción, no hay otras expresiones se evalúan y no se ejecuta ningún paso adicional.
*  Los valores calculados para las longitudes de dimensión se validan como sigue. Si uno o varios de los valores son inferiores a cero, una `System.OverflowException` se inicia y no se ejecuta ningún paso adicional.
*  Se asigna una instancia de matriz con las longitudes de dimensión determinada. Si no hay suficiente memoria disponible para asignar la nueva instancia, un `System.OutOfMemoryException` se inicia y no se ejecuta ningún paso adicional.
*  Todos los elementos de la nueva instancia de la matriz se inicializan en sus valores predeterminados ([los valores predeterminados](variables.md#default-values)).
*  Si la expresión de creación de matriz contiene a un inicializador de matriz, a continuación, cada expresión de inicializador de matriz se evalúan y se asignan a su correspondiente elemento de matriz. Las evaluaciones y asignaciones se realizan en el orden de las expresiones se escriben en el inicializador de matriz, en otras palabras, los elementos se inicializan en sentido ascendente índice, con la dimensión situada más a la derecha. Si la evaluación de una expresión determinada o la asignación posterior al elemento de matriz correspondiente causa una excepción, no hay más elementos se inicializan (y los elementos restantes, por tanto, tendrán sus valores predeterminados).

Una expresión de creación de matriz permite crear una instancia de una matriz con elementos de un tipo de matriz, pero se deben inicializar manualmente los elementos de este tipo de matriz. Por ejemplo, la instrucción
```csharp
int[][] a = new int[100][];
```
crea una matriz unidimensional con 100 elementos de tipo `int[]`. El valor inicial de cada elemento es `null`. No es posible que la misma expresión de creación de la matriz también crear instancias de las submatrices y la instrucción
```csharp
int[][] a = new int[100][5];        // Error
```
genera un error de tiempo de compilación. Creación de instancias de las submatrices en su lugar, debe realizarse manualmente, como en
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Cuando una matriz de matrices tiene forma "rectangular", es decir, cuando las submatrices tienen todos la misma longitud, es más eficaz utilizar una matriz multidimensional. En el ejemplo anterior, la creación de instancias de la matriz de matrices crea 101 objetos, una matriz externa y 100 submatrices. En cambio,
```csharp
int[,] = new int[100, 5];
```
crea un único objeto, una matriz bidimensional y se lleva a cabo la asignación en una sola instrucción.

Los siguientes son ejemplos de expresiones de creación de matriz con tipo implícito:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

La última expresión genera un error de tiempo de compilación porque ni `int` ni `string` es implícitamente convertible a otro y, por lo que es común no mejor escriba. Una expresión de creación de matriz con tipo explícito se debe usar en este caso, por ejemplo si se especifica el tipo es `object[]`. Como alternativa, se puede convertir uno de los elementos en un tipo base común, que se convierte entonces en el tipo de elemento deducido.

Expresiones de creación de matriz con tipo implícito se pueden combinar con inicializadores de objeto anónimo ([expresiones de creación de objeto anónimo](expressions.md#anonymous-object-creation-expressions)) crear de forma anónima con el tipo de estructuras de datos. Por ejemplo:
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

#### <a name="delegate-creation-expressions"></a>Expresiones de creación de delegado

Un *delegate_creation_expression* se utiliza para crear una nueva instancia de un *delegate_type*.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

El argumento de una expresión de creación de delegado debe ser un grupo de métodos, una función anónima o un valor del tipo de tiempo de compilación `dynamic` o un *delegate_type*. Si el argumento es un grupo de métodos, identifica el método y, para un método de instancia, el objeto que se va a crear un delegado. Si el argumento es una función anónima directamente define los parámetros y el cuerpo del método de destino del delegado. Si el argumento es un valor identifica una instancia de delegado que se va a crear una copia.

Si el *expresión* tiene el tipo de tiempo de compilación `dynamic`, *delegate_creation_expression* se enlazan dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) y las reglas siguientes se aplican en tiempo de ejecución utilizando el tipo de tiempo de ejecución de la *expresión*. En caso contrario, las reglas se aplican en tiempo de compilación.

El procesamiento en tiempo de enlace de un *delegate_creation_expression* del formulario `new D(E)`, donde `D` es un *delegate_type* y `E` es un *expresión* , consta de los pasos siguientes:

*  Si `E` es un grupo de métodos, la expresión de creación de delegado se procesa en la misma manera que la conversión de grupos de un método ([conversiones de grupo de métodos](conversions.md#method-group-conversions)) desde `E` a `D`.
*  Si `E` es una función anónima, la expresión de creación de delegado se procesa en la misma manera que una conversión de función anónima ([conversiones de función anónima](conversions.md#anonymous-function-conversions)) desde `E` a `D`.
*  Si `E` es un valor, `E` deben ser compatibles ([declaraciones de delegado](delegates.md#delegate-declarations)) con `D`, y el resultado es una referencia a un delegado recién creado del tipo `D` que hace referencia a la invocación del misma lista como `E`. Si `E` no es compatible con `D`, se produce un error de tiempo de compilación.

El procesamiento en tiempo de ejecución de un *delegate_creation_expression* del formulario `new D(E)`, donde `D` es un *delegate_type* y `E` es un *expresión* , consta de los pasos siguientes:

*   Si `E` es un grupo de métodos, la expresión de creación de delegado se evalúa como la conversión de grupos de un método ([conversiones de grupo de métodos](conversions.md#method-group-conversions)) desde `E` a `D`.
*   Si `E` es una función anónima, la creación del delegado se evalúa como una conversión de función anónima de `E` a `D` ([conversiones de función anónima](conversions.md#anonymous-function-conversions)).
*   Si `E` es un valor de un *delegate_type*:
    * `E` se evalúa. Si esta evaluación, produce una excepción, no se ejecuta ningún paso adicional.
    * Si el valor de `E` es `null`, un `System.NullReferenceException` se inicia y no se ejecuta ningún paso adicional.
    * Una nueva instancia del tipo de delegado `D` está asignada. Si no hay suficiente memoria disponible para asignar la nueva instancia, un `System.OutOfMemoryException` se inicia y no se ejecuta ningún paso adicional.
    * La nueva instancia de delegado se inicializa con la misma lista de invocación que la instancia del delegado proporcionada por `E`.

La lista de invocaciones de un delegado se determina cuando el delegado se crea una instancia y, a continuación, se mantiene constante durante toda la duración del delegado. En otras palabras, no es posible cambiar las entidades de destino que se puede llamar de un delegado, una vez que se ha creado. Cuando se combinan dos delegados o se quita uno de otro ([declaraciones de delegado](delegates.md#delegate-declarations)), se produce un nuevo delegado; cambia el contenido no tiene ningún delegado existente.

No es posible crear a un delegado que hace referencia a una propiedad, el indizador, operador definido por el usuario, el constructor de instancia, un destructor o constructor estático.

Como se describió anteriormente, cuando se crea un delegado de un grupo de métodos, la lista de parámetros formales y tipo de valor devuelto del delegado determinar cuál de los métodos sobrecargados para seleccionar. En el ejemplo
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
el `A.f` campo se inicializa con un delegado que hace referencia a la segunda `Square` método porque ese método coincide exactamente con la lista de parámetros formales y el tipo de valor devuelto de `DoubleFunc`. Tenía el segundo `Square` método no estado presente, se habría producido un error en tiempo de compilación.

#### <a name="anonymous-object-creation-expressions"></a>Expresiones de creación de objeto anónimo

Un *anonymous_object_creation_expression* se utiliza para crear un objeto de un tipo anónimo.

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

Un inicializador de objeto anónimo declara un tipo anónimo y devuelve una instancia de ese tipo. Un tipo anónimo es un tipo sin nombre de clase que hereda directamente de `object`. Los miembros de un tipo anónimo son una secuencia de propiedades de solo lectura que se deducen del inicializador de objeto anónimo usado para crear una instancia del tipo. En concreto, un inicializador de objeto anónimo del formulario
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
declara un tipo anónimo del formulario
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
donde cada `Tx` es el tipo de la expresión correspondiente `ex`. La expresión utilizada en un *member_declarator* debe tener un tipo. Por lo tanto, es un error en tiempo de compilación para una expresión en un *member_declarator* a ser null ni una función anónima. También es un error en tiempo de compilación para que la expresión tiene un tipo no seguro.

Los nombres de un tipo anónimo y del parámetro para su `Equals` método generados automáticamente por el compilador y no puede hacer referencia en el texto del programa.

Dentro del mismo programa, dos inicializadores de objeto anónimos que especifican una secuencia de propiedades de los mismos nombres y tipos de tiempo de compilación en el mismo orden producirán instancias del mismo tipo anónimo.

En el ejemplo
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
se permite la asignación en la última línea porque `p1` y `p2` son del mismo tipo anónimo.

El `Equals` y `GetHashcode` métodos en tipos anónimos invalidación los métodos heredados de `object`y se definen en términos de la `Equals` y `GetHashcode` de las propiedades, para que dos instancias del mismo tipo anónimo son iguales si y sólo si todas sus propiedades son iguales.

Un declarador de miembro puede abreviarse con un nombre simple ([inferencia](expressions.md#type-inference)), un acceso a miembros ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), un acceso base ([Base acceso](expressions.md#base-access)) o un acceso a miembros condicional null ([expresiones condicionales nulos como los inicializadores de proyección](expressions.md#null-conditional-expressions-as-projection-initializers)). Esto se denomina un ***inicializadores de proyección*** y es una abreviatura de una declaración de y la asignación a una propiedad con el mismo nombre. En concreto, los declaradores de miembros de los formularios
```csharp
identifier
expr.identifier
```
son exactamente equivalentes a lo siguiente, respectivamente:
```csharp
identifier = identifier
identifier = expr.identifier
```

Por lo tanto, en un inicializador de proyección el *identificador* selecciona el valor y el campo o propiedad al que se asigna el valor. De manera intuitiva, un inicializador de proyección proyectos no solo un valor, sino también el nombre del valor.

### <a name="the-typeof-operator"></a>El operador typeof

El `typeof` operador se usa para obtener el `System.Type` un tipo de objeto.

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

El primer formulario del *typeof_expression* consta de un `typeof` palabra clave seguido por un entre paréntesis *tipo*. El resultado de una expresión de esta forma es el `System.Type` objeto para el tipo indicado. Solo hay un `System.Type` objeto para un tipo dado. Esto significa que para un tipo `T`, `typeof(T) == typeof(T)` siempre es true. El *tipo* no puede ser `dynamic`.

La segunda forma de *typeof_expression* consta de un `typeof` palabra clave seguido por un entre paréntesis *unbound_type_name*. Un *unbound_type_name* es muy similar a un *type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)), salvo que un *unbound_type_name* contiene *generic_dimension_specifier*s donde un *type_name* contiene *type_argument_list*s. Cuando el operando de un *typeof_expression* es una secuencia de tokens que satisface las gramáticas de ambos *unbound_type_name* y *type_name*, es decir, cuando no contiene ni un *generic_dimension_specifier* ni un *type_argument_list*, la secuencia de tokens se considera un *type_name*. El significado de un *unbound_type_name* se determina como sigue:

*  Convertir la secuencia de tokens para una *type_name* reemplazando cada *generic_dimension_specifier* con un *type_argument_list* con el mismo número de comas y el palabra clave `object` cada *type_argument*.
*  Evaluar resultante *type_name*, mientras se les ignora todas las restricciones de parámetro de tipo.
*  El *unbound_type_name* resuelve el tipo genérico sin enlazar asociado con el tipo construido resultante ([dependientes e independientes tipos](types.md#bound-and-unbound-types)).

El resultado de la *typeof_expression* es la `System.Type` objeto resultante sin delimitar tipo genérico.

La tercera forma de *typeof_expression* consta de un `typeof` palabra clave seguido por un entre paréntesis `void` palabra clave. El resultado de una expresión de esta forma es el `System.Type` objeto que representa la ausencia de un tipo. El objeto de tipo devuelto por `typeof(void)` es distinto del objeto del tipo devuelto por cualquier tipo. Este objeto de tipo especial es útil en las bibliotecas de clases que permiten la reflexión en métodos en el lenguaje, donde dichos métodos desean tener una manera de representar el tipo de valor devuelto de cualquier método, incluidos los métodos void, con una instancia de `System.Type`.

El `typeof` operador puede usarse en un parámetro de tipo. El resultado es el `System.Type` objeto para el tipo de tiempo de ejecución que se enlazó con el parámetro de tipo. El `typeof` operador también puede usarse en un tipo construido o un tipo genérico sin enlazar ([dependientes e independientes tipos](types.md#bound-and-unbound-types)). El `System.Type` de objeto para un tipo genérico sin enlazar no es el mismo que el `System.Type` objeto del tipo de instancia. El tipo de instancia es siempre un tipo construido cerrado en tiempo de ejecución para su `System.Type` objeto depende de los argumentos de tipo de tiempo de ejecución en uso, mientras que el tipo genérico sin enlazar no tiene ningún argumento de tipo.

El ejemplo
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
genera el siguiente resultado:
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

Tenga en cuenta que `int` y `System.Int32` son del mismo tipo.

Tenga en cuenta también que el resultado de `typeof(X<>)` no depende del argumento de tipo, pero el resultado de `typeof(X<T>)` does.

### <a name="the-checked-and-unchecked-operators"></a>Los operadores checked y unchecked

El `checked` y `unchecked` operadores se utilizan para controlar la ***contexto de comprobación de desbordamiento*** para conversiones y operaciones aritméticas de tipo integral.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

El `checked` operador evalúa la expresión contenida en un contexto comprobado y el `unchecked` operador evalúa la expresión contenida en un contexto no comprobado. Un *checked_expression* o *unchecked_expression* corresponde exactamente a un *parenthesized_expression* ([lasexpresionesentreparéntesis](expressions.md#parenthesized-expressions)), excepto en que se evalúa la expresión contenida en el contexto de comprobación de desbordamiento dado.

También se puede controlar el contexto de comprobación de desbordamiento mediante la `checked` y `unchecked` instrucciones ([las instrucciones checked y unchecked](statements.md#the-checked-and-unchecked-statements)).

Las operaciones siguientes se ven afectadas por el contexto establecido por la comprobación de desbordamiento el `checked` y `unchecked` operadores e instrucciones:

*  Predefinido `++` y `--` operadores unarios ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) y [prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)), cuando el operando es de un entero tipo.
*  Predefinido `-` operador unario ([operador unario menos](expressions.md#unary-minus-operator)), cuando el operando es de tipo entero.
*  Predefinido `+`, `-`, `*`, y `/` operadores binarios ([operadores aritméticos](expressions.md#arithmetic-operators)), cuando ambos operandos son de tipos enteros.
*  Conversiones numéricas explícitas ([conversiones numéricas explícitas](conversions.md#explicit-numeric-conversions)) de un tipo integral a otro tipo entero o de `float` o `double` a un tipo entero.

Cuando una de las operaciones anteriores se producirá un resultado que es demasiado grande para representarlo en el tipo de destino, el contexto en que la operación se realiza controles el comportamiento resultante:

*  En un `checked` contexto, si la operación es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)), se produce un error de tiempo de compilación. En caso contrario, cuando la operación se realiza en tiempo de ejecución, un `System.OverflowException` se produce.
*  En un `unchecked` contexto, el resultado se trunca al descartar los bits de orden superior que no caben en el tipo de destino.

Para expresiones no constantes (expresiones que se evalúan en tiempo de ejecución) que no están incluidas por `checked` o `unchecked` operadores o instrucciones, el contexto de comprobación de desbordamiento predeterminado es `unchecked` a menos que los factores externos (por ejemplo, el compilador llamar conmutadores y configuración del entorno de ejecución) para `checked` evaluación.

Las expresiones constantes (expresiones que se pueden evaluar por completo en tiempo de compilación), el contexto de comprobación de desbordamiento predeterminado siempre es `checked`. A menos que explícitamente se coloca una expresión constante en una `unchecked` contexto, desbordamientos que ocurren durante la evaluación de tiempo de compilación de la expresión siempre causan errores de compilación.

El cuerpo de una función anónima no se ve afectado por `checked` o `unchecked` contextos en los que se produce la función anónima.

En el ejemplo
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
Puesto que ninguna de las expresiones se pueden evaluar en tiempo de compilación, se notifica ningún error de tiempo de compilación. En tiempo de ejecución, el `F` método produce una `System.OverflowException`y el `G` método devuelve-727379968 (los 32 bits menores del resultado fuera de intervalo). El comportamiento de la `H` depende del método en el contexto para la compilación de comprobación de desbordamiento predeterminado, pero es el mismo que `F` o igual que `G`.

En el ejemplo
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
los desbordamientos que se producen al evaluar las expresiones constantes en `F` y `H` provocan errores de tiempo de compilación notificará porque las expresiones se evalúan en un `checked` contexto. También se produce un desbordamiento al evaluar la expresión constante en `G`, pero dado que la evaluación realiza un `unchecked` contexto, no se notifica el desbordamiento.

El `checked` y `unchecked` sólo afectan a los operadores del contexto de las operaciones que están contenidas textualmente en comprobación de desbordamiento el "`(`"y"`)`" tokens. Los operadores no tienen ningún efecto en los miembros de función que se invocan como resultado de evaluar la expresión contenida. En el ejemplo
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
el uso de `checked` en `F` no afecta a la evaluación de `x * y` en `Multiply`, por lo que `x * y` se evalúa en el contexto de comprobación de desbordamiento predeterminado.

El `unchecked` operador es conveniente al escribir constantes de los tipos enteros con signo en notación hexadecimal. Por ejemplo:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Dos de las constantes hexadecimales anteriores son de tipo `uint`. Dado que las constantes están fuera de la `int` intervalo, sin el `unchecked` operador, las conversiones a `int` producirían errores de tiempo de compilación.

El `checked` y `unchecked` operadores e instrucciones permiten a los programadores controlar ciertos aspectos de algunos cálculos numéricos. Sin embargo, el comportamiento de algunos operadores numéricos depende de los tipos de datos de sus operandos. Por ejemplo, la multiplicación de dos decimales siempre produce una excepción en caso de desbordamiento incluso dentro un explícitamente `unchecked` construir. De forma similar, multiplicar dos flota nunca los resultados de una excepción en caso de desbordamiento incluso dentro un explícitamente `checked` construir. Además, los otros operadores nunca se ven afectados por el modo de comprobación, ya sea predeterminada o explícita.

### <a name="default-value-expressions"></a>Expresiones de valor predeterminado

Una expresión de valor predeterminado se utiliza para obtener el valor predeterminado ([los valores predeterminados](variables.md#default-values)) de un tipo. Normalmente se utiliza una expresión de valor predeterminado para los parámetros de tipo, ya que no puede conocerse si el parámetro de tipo es un tipo de valor o un tipo de referencia. (No existe ninguna conversión desde el `null` literal a un parámetro de tipo a menos que el parámetro de tipo se conoce como un tipo de referencia.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Si el *tipo* en un *default_value_expression* evalúa en tiempo de ejecución a un tipo de referencia, el resultado es `null` convertir a ese tipo. Si el *tipo* en un *default_value_expression* evalúa en tiempo de ejecución a un tipo de valor, el resultado es el *value_type*del valor predeterminado ([predeterminada constructores](types.md#default-constructors)).

Un *default_value_expression* es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)) si el tipo es un tipo de referencia o un parámetro de tipo que se sabe que es un tipo de referencia ([parámetro de tipo restricciones](classes.md#type-parameter-constraints)). Además, un *default_value_expression* es una expresión constante si el tipo es uno de los siguientes tipos de valor: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, o cualquier tipo de enumeración.


### <a name="nameof-expressions"></a>Expresiones Nameof

Un *nameof_expression* se usa para obtener el nombre de una entidad de programa como una constante de cadena.

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

Gramaticalmente hablando, la *named_entity* operando siempre es una expresión. Dado que `nameof` no es una palabra reservada, siempre es sintácticamente ambigua con una invocación del nombre sencillo de una expresión nameof `nameof`. Por motivos de compatibilidad, si una búsqueda de nombres ([nombres simples](expressions.md#simple-names)) del nombre `nameof` se realiza correctamente, la expresión se trata como un *invocation_expression* , independientemente de si es la invocación legal. En caso contrario, es un *nameof_expression*.

El significado de la *named_entity* de un *nameof_expression* es el significado de la misma que una expresión; es decir, ya sea como un *simple_name*, un *base_access*  o un *member_access*. Sin embargo, donde se describe la búsqueda en [nombres simples](expressions.md#simple-names) y [acceso a miembros](expressions.md#member-access) produce un error porque se encontró un miembro de instancia en un contexto estático, un *nameof_expression*no produce ningún error de ese tipo.

Es un error en tiempo de compilación para un *named_entity* designar un grupo de métodos para tener un *type_argument_list*. Es un error en tiempo de compilación para un *named_entity_target* que el tipo `dynamic`.

Un *nameof_expression* es una expresión constante de tipo `string`, y no tiene ningún efecto en tiempo de ejecución. En concreto, su *named_entity* no se evalúa y se omite para los fines de análisis de asignación definitiva ([reglas generales para las expresiones simples](variables.md#general-rules-for-simple-expressions)). Su valor es el último identificador de la *named_entity* antes de la última opcional *type_argument_list*transformado en la siguiente manera:

* El prefijo "`@`", si se usa, se quita.
* Cada *unicode_escape_sequence* se transforma en su carácter Unicode correspondiente.
* Cualquier *formatting_characters* se quitan.

Estas son las mismas transformaciones que se aplican en [identificadores](lexical-structure.md#identifiers) al probar la igualdad entre los identificadores.

TODO: ejemplos

### <a name="anonymous-method-expressions"></a>Expresiones de métodos anónimos

Un *anonymous_method_expression* es uno de dos maneras de definir una función anónima. Estos se describen en [expresiones de función anónima](expressions.md#anonymous-function-expressions).

## <a name="unary-operators"></a>Operadores unarios

El `?`, `+`, `-`, `!`, `~`, `++`, `--`, convierta, y `await` se denominan los operadores unarios.

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

Si el operando de un *unary_expression* tiene el tipo de tiempo de compilación `dynamic`, se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso se escriba el tiempo de compilación de la *unary_expression* es `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución del operando.

### <a name="null-conditional-operator"></a>Operador condicional null

El operador condicional de null solo aplica una lista de operaciones para su operando si dicho operando no es null. En caso contrario, es el resultado de aplicar el operador `null`.

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

Puede incluir la lista de las operaciones de acceso a miembros y las operaciones de acceso de elemento (que pueden estar condicional de null), así como de invocación.

Por ejemplo, la expresión `a.b?[0]?.c()` es un *null_conditional_expression* con un *primary_expression* `a.b` y *null_conditional_operations* `?[0]` (acceso de elemento condicional de null), `?.c` (acceso condicional de null miembro) y `()` (invocación).

Para un *null_conditional_expression* `E` con un *primary_expression* `P`, permiten `E0` la expresión obtenida extrayendo textualmente el interlineado `?`de cada uno de los *null_conditional_operations* de `E` que tiene uno. Conceptualmente, `E0` es la expresión que se evaluará si ninguna de las comprobaciones de null que representa el `?`s encontrar un `null`.

También, permiten `E1` la expresión obtenido quitando textualmente el interlineado `?` desde solamente el primero del *null_conditional_operations* en `E`. Esto puede dar lugar a un *expresión primaria* (si hubiera una sola `?`) o a otro *null_conditional_expression*.

Por ejemplo, si `E` es la expresión `a.b?[0]?.c()`, a continuación, `E0` es la expresión `a.b[0].c()` y `E1` es la expresión `a.b[0]?.c()`.

Si `E0` se clasifica como nada, a continuación, `E` se clasifica como nada. En caso contrario, E está clasificado como un valor.

`E0` y `E1` se usan para determinar el significado de `E`:

*  Si `E` se produce como un *statement_expression* el significado de `E` es el mismo que la instrucción

   ```csharp
   if ((object)P != null) E1;
   ```

   salvo que P se evalúa solo una vez.

*  De lo contrario, si `E0` se clasifica como nada se produce un error de tiempo de compilación.

*  De lo contrario, deje que `T0` ser el tipo de `E0`.

   *  Si `T0` es un parámetro de tipo que no se conoce como un tipo de referencia o un tipo de valor distinto de null, se produce un error de tiempo de compilación.

   *  Si `T0` es un tipo de valor distinto de null, entonces el tipo de `E` es `T0?`y el significado de `E` es igual que

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      salvo que `P` se evalúa solo una vez.

   *  En caso contrario, el tipo de E es T0, y el significado de E es igual a

      ```csharp
      ((object)P == null) ? null : E1
      ```

      salvo que `P` se evalúa solo una vez.

Si `E1` es en sí mismo un *null_conditional_expression*, a continuación, estas reglas se aplican de nuevo, el anidamiento de las pruebas para `null` hasta que no haya ningún otro `?`de, y se ha reducido la expresión hacia abajo en la expresión primaria `E0`.

Por ejemplo, si la expresión `a.b?[0]?.c()` se produce como una expresión de instrucción, como se muestra en la instrucción:
```csharp
a.b?[0]?.c();
```
es equivalente a su significado:
```csharp
if (a.b != null) a.b[0]?.c();
```
nuevo que es equivalente a:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
Salvo que `a.b` y `a.b[0]` se evalúan una sola vez.

Si se produce en un contexto donde se utiliza su valor, como en:
```csharp
var x = a.b?[0]?.c();
```
y suponiendo que el tipo de la llamada final no es un tipo de valor distinto de null, es equivalente a su significado:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
Salvo que `a.b` y `a.b[0]` se evalúan una sola vez.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>Expresiones condicionales nulos como los inicializadores de proyección

Solo se permite una expresión condicional de null como un *member_declarator* en un *anonymous_object_creation_expression* ([expresiones de creación de objeto anónimo](expressions.md#anonymous-object-creation-expressions)) si finaliza con un acceso de miembro (opcionalmente condicional de null). Gramaticalmente, este requisito puede expresarse como:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Se trata de un caso especial de la gramática de *null_conditional_expression* anteriormente. El entorno de producción *member_declarator* en [expresiones de creación de objeto anónimo](expressions.md#anonymous-object-creation-expressions) , a continuación, se incluye solo *null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Expresiones como expresiones de instrucción condicional de null

Solo se permite una expresión condicional de null como un *statement_expression* ([las instrucciones de expresión](statements.md#expression-statements)) si termina con una invocación. Gramaticalmente, este requisito puede expresarse como:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Se trata de un caso especial de la gramática de *null_conditional_expression* anteriormente. El entorno de producción *statement_expression* en [las instrucciones de expresión](statements.md#expression-statements) , a continuación, se incluye solo *null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Operador unario más

Para una operación del formulario `+x`, resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. El operando se convierte al tipo de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador. El unario más predefinida operadores son:

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

Para cada uno de estos operadores, el resultado es simplemente el valor del operando.

### <a name="unary-minus-operator"></a>Operador unario menos

Para una operación del formulario `-x`, resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. El operando se convierte al tipo de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador. Los operadores de negación predefinidos son:

*  Negación entero:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   El resultado se calcula restando `x` desde cero. Si el valor de `x` es el valor más pequeño que se puede representar el tipo de operando (-2 ^ 31 para `int` o -2 ^ 63 para `long`), a continuación, la negación matemática de `x` no es que se puede representar en el tipo de operando. Si esto ocurre dentro de un `checked` contexto, un `System.OverflowException` se inicia; si se produce dentro de un `unchecked` contexto, el resultado es el valor del operando y no se notifica el desbordamiento.

   Si el operando del operador de negación es de tipo `uint`, se convierte al tipo `long`, y el tipo del resultado es `long`. Una excepción es la regla que permite el `int` valor entre -2147483648 (-2 ^ 31) se deben escribir como un literal entero decimal ([literales enteros](lexical-structure.md#integer-literals)).

   Si el operando del operador de negación es de tipo `ulong`, se produce un error de tiempo de compilación. Una excepción es la regla que permite el `long` valor -9223372036854775808 (-2 ^ 63) se deben escribir como un literal entero decimal ([literales enteros](lexical-structure.md#integer-literals)).

*  Negación de punto flotante:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   El resultado es el valor de `x` con su signo invertido. Si `x` es NaN, el resultado también es NaN.

*  Negación decimal:

   ```csharp
   decimal operator -(decimal x);
   ```

   El resultado se calcula restando `x` desde cero. La negación decimal equivale a usar el operador unario menos de tipo `System.Decimal`.

### <a name="logical-negation-operator"></a>Operador lógico de negación

Para una operación del formulario `!x`, resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. El operando se convierte al tipo de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador. Existe solo un operador de negación lógica predefinidos:
```csharp
bool operator !(bool x);
```

Este operador calcula la negación lógica del operando: Si el operando es `true`, el resultado es `false`. Si el operando es `false`, el resultado es `true`.

### <a name="bitwise-complement-operator"></a>Operador de complemento bit a bit

Para una operación del formulario `~x`, resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. El operando se convierte al tipo de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador. Los operadores de complemento bit a bit predefinidos son:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Para cada uno de estos operadores, el resultado de la operación es el complemento bit a bit de `x`.

Cada tipo de enumeración `E` proporciona implícitamente el siguiente operador de complemento bit a bit:

```csharp
E operator ~(E x);
```

El resultado de evaluar `~x`, donde `x` es una expresión de un tipo de enumeración `E` con un tipo subyacente `U`, es exactamente el mismo que el de evaluar `(E)(~(U)x)`, salvo que la conversión a `E` es siempre se realiza como si se encuentra en un `unchecked` contexto ([los operadores checked y unchecked](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Prefijo de incremento y decremento de operadores

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

El operando de un prefijo de incremento o decremento operación debe ser una expresión que se clasifica como una variable, el acceso a una propiedad o un acceso de indizador. El resultado de la operación es un valor del mismo tipo como operando.

Si el operando de un prefijo de incremento o decremento operación es una propiedad o acceso de indizador, la propiedad o indizador debe tenerlos un `get` y un `set` descriptor de acceso. Si esto no es el caso, se produce un error en tiempo de enlace.

Resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. Predefinidos `++` y `--` operadores existen para los siguientes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`y cualquier tipo de enumeración. Predefinido `++` operadores devuelven el valor generado al sumar 1 al operando y predefinido `--` operadores devuelven el valor generado al restar 1 del operando. En un `checked` contexto, si el resultado de esta suma o resta está fuera del intervalo del tipo del resultado y el tipo de resultado es un tipo entero o un tipo de enumeración, un `System.OverflowException` se produce.

El procesamiento en tiempo de ejecución de un prefijo de incremento o decremento de la operación de la forma `++x` o `--x` consta de los pasos siguientes:

*   Si `x` se clasifica como una variable:
    * `x` se evalúa para producir la variable.
    * Se invoca el operador seleccionado con el valor de `x` como su argumento.
    * El valor devuelto por el operador se almacena en la ubicación dada por la evaluación de `x`.
    * El valor devuelto por el operador se convierte en el resultado de la operación.
*   Si `x` se clasifica como un acceso de propiedad o indizador:
    * La expresión de instancia (si `x` no `static`) y la lista de argumentos (si `x` es un acceso de indizador) asociadas con `x` se evalúan, y los resultados se usan en la siguiente `get` y `set` invocaciones de descriptor de acceso.
    * El `get` descriptor de acceso de `x` se invoca.
    * Se invoca el operador seleccionado con el valor devuelto por la `get` descriptor de acceso como su argumento.
    * El `set` descriptor de acceso de `x` se invoca con el valor devuelto por el operador como su `value` argumento.
    * El valor devuelto por el operador se convierte en el resultado de la operación.

El `++` y `--` operadores también admiten postfijo notación ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)). Normalmente, el resultado de `x++` o `x--` es el valor de `x` antes de la operación, mientras que el resultado de `++x` o `--x` es el valor de `x` después de la operación. En cualquier caso, `x` sí tiene el mismo valor después de la operación.

Un `operator++` o `operator--` implementación puede invocarse mediante la notación de prefijo o sufijo. No es posible tener implementaciones de operador independientes para las dos notaciones.

### <a name="cast-expressions"></a>Expresiones de conversión

Un *cast_expression* se usa para convertir explícitamente una expresión a un tipo determinado.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

Un *cast_expression* del formulario `(T)E`, donde `T` es un *tipo* y `E` es un *unary_expression*, realiza una explícita conversión ([las conversiones explícitas](conversions.md#explicit-conversions)) del valor de `E` escriba `T`. Si no existe ninguna conversión explícita de `E` a `T`, se produce un error en tiempo de enlace. En caso contrario, el resultado es el valor producido por la conversión explícita. El resultado siempre se clasifica como un valor, aunque `E` denota una variable.

La gramática de una *cast_expression* conduce a ciertas ambigüedades sintácticas. Por ejemplo, la expresión `(x)-y` se cualquiera puede interpretar como un *cast_expression* (una conversión de `-y` escriba `x`) o como un *additive_expression* combinado con un *parenthesized_expression* (que calcula el valor `x - y)`.

Para resolver *cast_expression* ambigüedades, la siguiente regla existe: Una secuencia de uno o varios *token*s ([espacio en blanco](lexical-structure.md#white-space)) entre paréntesis se considera el principio de un *cast_expression* sólo si al menos uno de los siguientes es verdaderas:

*  La secuencia de símbolos es correcta gramaticalmente para un *tipo*, pero no para un *expresión*.
*  La secuencia de símbolos es correcta gramaticalmente para un *tipo*, y el token sigue inmediatamente al paréntesis de cierre es el token "`~`", el token "`!`", el token "`(`", un  *identificador* ([secuencias de escape de caracteres Unicode](lexical-structure.md#unicode-character-escape-sequences)), un *literal* ([literales](lexical-structure.md#literals)), o cualquier *palabra clave*([Palabras clave](lexical-structure.md#keywords)) excepto `as` y `is`.

El término "gramaticalmente correcto" significa que la secuencia de tokens debe ajustarse a la producción gramatical particular. En concreto no considera el significado real de sus identificadores constituyentes. Por ejemplo, si `x` y `y` son identificadores, a continuación, `x.y` es gramática correcto para un tipo, incluso si `x.y` realmente no denota un tipo.

Desde la regla de desambiguación se deduce que, si `x` y `y` son identificadores, `(x)y`, `(x)(y)`, y `(x)(-y)` son *cast_expression*s, pero `(x)-y` no lo es, incluso si `x` identifica un tipo. Sin embargo, si `x` es una palabra clave que identifica un tipo predefinido (como `int`), a continuación, las cuatro formas son *cast_expression*s (porque esta palabra clave no podría ser una expresión por sí mismo).

### <a name="await-expressions"></a>Expresiones await

Se usa el operador await para suspender la evaluación de la función async envolvente hasta que se ha completado la operación asincrónica representada por el operando.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

Un *await_expression* solo se permite en el cuerpo de una función asincrónica ([iteradores](classes.md#iterators)). En la envolvente más próximo función asincrónica, un *await_expression* puede no producirse en estos lugares:

*  Dentro de una función anónima (no-asincrónico anidadas)
*  Dentro del bloque de un *lock_statement*
*  En un contexto no seguro

Tenga en cuenta que un *await_expression* no puede aparecer en la mayoría de los lugares dentro de un *expresión_de_consulta*, porque los sintácticamente se transforman para usar las expresiones lambda no son asincrónicas.

Dentro de una función asincrónica, `await` no se puede usar como identificador. Por lo tanto, no hay ninguna ambigüedad sintáctica entre expresiones await y diversas expresiones que implican los identificadores. Fuera de las funciones asincrónicas, `await` actúa como un identificador normal.

El operando de un *await_expression* se denomina el ***tarea***. Representa una operación asincrónica que puede o no estar completa en el momento en el *await_expression* se evalúa. Es el propósito del operador await suspender la ejecución de la función async envolvente hasta que se complete la tarea esperada y, a continuación, obtener su resultado.

#### <a name="awaitable-expressions"></a>Expresiones que admite await

La tarea de una expresión await tiene que ser ***esperable***. Una expresión `t` es esperable si uno de los siguientes contiene:

*  `t` es de tipo en tiempo de compilación `dynamic`
*  `t` tiene un método de extensión o de instancia accesible denominado `GetAwaiter` con un tipo de valor devuelto y ningún parámetro de tipo y sin parámetros `A` para el que se mantenga todos los elementos siguientes:
   * `A` implementa la interfaz `System.Runtime.CompilerServices.INotifyCompletion` (aquí en adelante conocidas como `INotifyCompletion` por razones de brevedad)
   * `A` tiene una propiedad de instancia accesible y legible `IsCompleted` de tipo `bool`
   * `A` tiene un método de instancia accesible `GetResult` sin parámetros ni ningún parámetro de tipo

El propósito de la `GetAwaiter` método consiste en obtener una ***awaiter*** para la tarea. El tipo `A` se denomina el ***tipo awaiter*** para la expresión await.

El propósito de la `IsCompleted` propiedad consiste en determinar si la tarea ya está completa. Si es así, no es necesario para suspender la evaluación.

El propósito de la `INotifyCompletion.OnCompleted` método consiste en registrarse "continuación" a la tarea; es decir, un delegado (de tipo `System.Action`) que se invocará una vez completada la tarea.

El propósito de la `GetResult` método consiste en obtener el resultado de la tarea cuando haya terminado. Este resultado puede ser la finalización correcta, posiblemente con un valor de resultado, o puede ser una excepción que produce el `GetResult` método.

#### <a name="classification-of-await-expressions"></a>Clasificación de expresiones await

La expresión `await t` se clasifican del mismo modo que la expresión `(t).GetAwaiter().GetResult()`. Por lo tanto, si el tipo de valor devuelto de `GetResult` es `void`, *await_expression* se clasifica como nada. Si tiene un tipo de valor devuelto distinto de void `T`, *await_expression* se clasifica como un valor de tipo `T`.

#### <a name="runtime-evaluation-of-await-expressions"></a>En tiempo de ejecución evaluación de expresiones await

En tiempo de ejecución, la expresión `await t` se evalúa como sigue:

*  Un awaiter `a` se obtiene al evaluar la expresión `(t).GetAwaiter()`.
*  Un `bool` `b` se obtiene al evaluar la expresión `(a).IsCompleted`.
*  Si `b` es `false` , a continuación, evaluación depende de si `a` implementa la interfaz `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (aquí en adelante conocidas como `ICriticalNotifyCompletion` por razones de brevedad). Esta comprobación se realiza en tiempo; de enlace es decir, en tiempo de ejecución si `a` tiene el tipo de tiempo de compilación `dynamic`y en tiempo de compilación en caso contrario. Permiten `r` denotan el delegado de reanudación ([iteradores](classes.md#iterators)):
    * Si `a` no implementa `ICriticalNotifyCompletion`, a continuación, la expresión `(a as (INotifyCompletion)).OnCompleted(r)` se evalúa.
    * Si `a` neimplementuje `ICriticalNotifyCompletion`, a continuación, la expresión `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` se evalúa.
    * A continuación, se suspende la evaluación y control se devuelve al llamador actual de la función async.
*  Ya sea inmediatamente después de (si `b` era `true`), o al invocar el delegado de reanudación posterior (si `b` era `false`), la expresión `(a).GetResult()` se evalúa. Si devuelve un valor, ese valor es el resultado de la *await_expression*. En caso contrario, el resultado es nothing.

Implementación de un factor en espera de los métodos de interfaz `INotifyCompletion.OnCompleted` y `ICriticalNotifyCompletion.UnsafeOnCompleted` provocaría que el delegado `r` invocarse como máximo una vez. En caso contrario, el comportamiento de la función envolvente de async es indefinido.

## <a name="arithmetic-operators"></a>Operadores aritméticos

El `*`, `/`, `%`, `+`, y `-` se denominan los operadores aritméticos.

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

Si un operando de un operador aritmético tiene el tipo de tiempo de compilación `dynamic`, a continuación, la expresión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso es el tipo de tiempo de compilación de la expresión `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución de los operandos que tienen el tipo de tiempo de compilación `dynamic`.

### <a name="multiplication-operator"></a>Operador de multiplicación

Para una operación del formulario `x * y`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.

A continuación, se enumeran los operadores de multiplicación predefinidos. Todos los operadores calculan el producto de `x` y `y`.

*  Multiplicación de enteros:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   En un `checked` contexto, si el producto está fuera del intervalo del tipo del resultado, un `System.OverflowException` se produce. En un `unchecked` contexto, no se notifican los desbordamientos y se descartan los bits de orden superior significativa fuera del intervalo del tipo del resultado.


*  Multiplicación de punto flotante:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   El producto se calcula según las reglas aritméticas de IEEE 754. En la tabla siguiente se muestra los resultados de todas las posibles combinaciones de valores finitos distinto de cero, ceros, valores infinitos y NaN. En la tabla, `x` y `y` son los valores positivos finitos. `z` es el resultado de `x * y`. Si el resultado es demasiado grande para el tipo de destino, `z` es infinito. Si el resultado es demasiado pequeño para el tipo de destino, `z` es cero.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | + y   | -y   | +0  | -0  | +inf | -inf | NaN | 
   | + x   | + z   | -z   | +0  | -0  | +inf | -inf | NaN | 
   | -x   | -z   | + z   | -0  | +0  | -inf | +inf | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | +inf | +inf | -inf | NaN | NaN | +inf | -inf | NaN | 
   | -inf | -inf | +inf | NaN | NaN | -inf | +inf | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  Multiplicación de decimales:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Si el valor resultante es demasiado grande para representarlo en el `decimal` formato, un `System.OverflowException` se produce. Si el valor del resultado es demasiado pequeño para representarlo en el `decimal` formato, el resultado es cero. La escala del resultado, antes de cualquier operación de redondeo, es la suma de las escalas de los dos operandos.

   La multiplicación de decimales es equivalente a usar el operador de multiplicación de tipo `System.Decimal`.


### <a name="division-operator"></a>Operador de división

Para una operación del formulario `x / y`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.

A continuación, se enumeran los operadores de división predefinidos. Todos los operadores calculan el cociente de `x` y `y`.

*  División de enteros:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Si el valor del operando derecho es cero, una `System.DivideByZeroException` se produce.

   La división redondea el resultado hacia cero. Por lo tanto, el valor absoluto del resultado es el entero más grande posible que sea menor o igual al valor absoluto del cociente de los dos operandos. El resultado es cero o positivo cuando los dos operandos tienen el mismo signo y cero o negativo si los dos operandos tienen signos opuestos.

   Si el operando izquierdo es el más pequeño que se puede representar `int` o `long` valor y el operando derecho es `-1`, se produce un desbordamiento. En un `checked` contexto, esto hace que un `System.ArithmeticException` (o una subclase de ella) que se produzca. En un `unchecked` contexto está definido por la implementación si un `System.ArithmeticException` (o una subclase de ella) se inicia o se informa del desbordamiento con el valor resultante es que el del operando izquierdo.

*  División de punto flotante:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   Se calcula el cociente según las reglas aritméticas de IEEE 754. En la tabla siguiente se muestra los resultados de todas las posibles combinaciones de valores finitos distinto de cero, ceros, valores infinitos y NaN. En la tabla, `x` y `y` son los valores positivos finitos. `z` es el resultado de `x / y`. Si el resultado es demasiado grande para el tipo de destino, `z` es infinito. Si el resultado es demasiado pequeño para el tipo de destino, `z` es cero.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | + y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | + x   | + z   | -z   | +inf | -inf | +0   | -0   | NaN  | 
   | -x   | -z   | + z   | -inf | +inf | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | +inf | +inf | -inf | +inf | -inf | NaN  | NaN  | NaN  | 
   | -inf | -inf | +inf | -inf | +inf | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  División decimal:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Si el valor del operando derecho es cero, una `System.DivideByZeroException` se produce. Si el valor resultante es demasiado grande para representarlo en el `decimal` formato, un `System.OverflowException` se produce. Si el valor del resultado es demasiado pequeño para representarlo en el `decimal` formato, el resultado es cero. La escala del resultado es el menor escala que conserve un resultado igual que el más próximo al valor decimal que se puede representar en el resultado matemático es true.

   División decimal es equivalente a usar el operador de división de tipo `System.Decimal`.


### <a name="remainder-operator"></a>Operador de resto

Para una operación del formulario `x % y`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.

Los operadores predefinidos resto se enumeran a continuación. Todos los operadores compute el resto de la división entre `x` y `y`.

*  Resto entero:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   El resultado de `x % y` es el valor producido por `x - (x / y) * y`. Si `y` es cero, una `System.DivideByZeroException` se produce.

   Si el operando izquierdo es el más pequeño `int` o `long` valor y el operando derecho es `-1`, un `System.OverflowException` se produce. En ningún caso hace `x % y` produce una excepción donde `x / y` no produciría una excepción.

*  Resto de punto flotante:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   En la tabla siguiente se muestra los resultados de todas las posibles combinaciones de valores finitos distinto de cero, ceros, valores infinitos y NaN. En la tabla, `x` y `y` son los valores positivos finitos. `z` es el resultado de `x % y` y se calcula como `x - n * y`, donde `n` es el entero más grande posible que sea menor o igual que `x / y`. Este método para calcular el resto es análogo a la utilizada para los operandos enteros, pero difiere de la definición de IEEE 754 (en el que `n` es el entero más cercano al `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | + y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | + x   | + z   | + z   | NaN  | NaN  | x    | x    | NaN  | 
   | -x   | -z   | -z   | NaN  | NaN  | -x   | -x   | NaN  | 
   | +0   | +0   | +0   | NaN  | NaN  | +0   | +0   | NaN  | 
   | -0   | -0   | -0   | NaN  | NaN  | -0   | -0   | NaN  | 
   | +inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | -inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Resto decimal:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Si el valor del operando derecho es cero, una `System.DivideByZeroException` se produce. La escala del resultado, antes de cualquier operación de redondeo, es el mayor de las escalas de los dos operandos y el signo del resultado, si es distinto de cero, es el mismo que el de `x`.

   El resto decimal es equivalente a usar el operador de resto de tipo `System.Decimal`.


### <a name="addition-operator"></a>Operador de suma

Para una operación del formulario `x + y`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.

A continuación, se muestran los operadores de suma predefinidos. Para los tipos numéricos y de enumeración, los operadores de suma predefinidos calculan la suma de los dos operandos. Cuando uno o ambos operandos son de tipo cadena, los operadores de suma predefinidos concatenan la representación de cadena de los operandos.

*  Adición de números enteros:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   En un `checked` contexto, si la suma está fuera del intervalo del tipo del resultado, un `System.OverflowException` se produce. En un `unchecked` contexto, no se notifican los desbordamientos y se descartan los bits de orden superior significativa fuera del intervalo del tipo del resultado.

*  Adición de punto flotante:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   La suma se calcula según las reglas aritméticas de IEEE 754. En la tabla siguiente se muestra los resultados de todas las posibles combinaciones de valores finitos distinto de cero, ceros, valores infinitos y NaN. En la tabla, `x` y `y` son valores finitos distinto de cero, y `z` es el resultado de `x + y`. Si `x` y `y` tienen la misma magnitud pero signos opuestos, `z` es cero positivo. Si `x + y` es demasiado grande para representarlo en el tipo de destino, `z` es infinito con el mismo signo que `x + y`.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | y    | +0   | -0   | +inf | -inf | NaN  | 
   | x    | z    | x    | x    | +inf | -inf | NaN  | 
   | +0   | y    | +0   | +0   | +inf | -inf | NaN  | 
   | -0   | y    | +0   | -0   | +inf | -inf | NaN  | 
   | +inf | +inf | +inf | +inf | +inf | NaN  | NaN  | 
   | -inf | -inf | -inf | -inf | NaN  | -inf | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Suma de decimales:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Si el valor resultante es demasiado grande para representarlo en el `decimal` formato, un `System.OverflowException` se produce. La escala del resultado, antes de cualquier operación de redondeo, es el mayor de las escalas de los dos operandos.

   La suma de decimales es equivalente a usar el operador de suma de tipo `System.Decimal`.

*  Adición de la enumeración. Cada tipo de enumeración proporciona implícitamente predefinidos de los siguientes operadores, donde `E` es el tipo enum y `U` es el tipo subyacente de `E`:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   En tiempo de ejecución, estos operadores se evalúan exactamente como `(E)((U)x + (U)y)`.

*  Concatenación de cadenas:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   Estas sobrecargas del binario `+` realizar de operador de concatenación de cadenas. Si es un operando de la concatenación de cadenas `null`, se sustituye una cadena vacía. De lo contrario, cualquier argumento que no son de cadena se convierte en su representación de cadena mediante la invocación virtual `ToString` método hereda del tipo `object`. Si `ToString` devuelve `null`, se sustituye una cadena vacía.

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

   El resultado del operador de concatenación de cadena es una cadena que consta de los caracteres del operando izquierdo seguidos por los caracteres del operando derecho. El operador de concatenación de cadenas nunca devuelve un `null` valor. Un `System.OutOfMemoryException` se puede producir si no hay suficiente memoria disponible para asignar la cadena resultante.

*  Combinación de delegados. Cada tipo de delegado proporciona implícitamente el siguiente operador predefinido, donde `D` es el tipo de delegado:

   ```csharp
   D operator +(D x, D y);
   ```

   El archivo binario `+` operador realiza la combinación de delegados cuando ambos operandos son de un tipo delegado `D`. (Si los operandos tienen tipos de delegado distintos, se produce un error en tiempo de enlace.) Si el primer operando es `null`, el resultado de la operación es el valor del segundo operando (aunque eso es también `null`). En caso contrario, si el segundo operando es `null`, entonces el resultado de la operación es el valor del primer operando. En caso contrario, el resultado de la operación es un nueva instancia de delegado que, cuando se invoca, invoca el primer operando y, a continuación, invoca el segundo operando. Para obtener ejemplos de combinación de delegados, vea [operador de resta](expressions.md#subtraction-operator) y [invocación de delegado](delegates.md#delegate-invocation). Puesto que `System.Delegate` no es un tipo de delegado, `operator`  `+` no está definido para ella.

### <a name="subtraction-operator"></a>Operador de resta

Para una operación del formulario `x - y`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.

Los operadores de resta predefinidos se enumeran a continuación. Los operadores todos restar `y` desde `x`.

*  Resta de enteros:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   En un `checked` contexto, si la diferencia está fuera del intervalo del tipo del resultado, un `System.OverflowException` se produce. En un `unchecked` contexto, no se notifican los desbordamientos y se descartan los bits de orden superior significativa fuera del intervalo del tipo del resultado.

*  Resta de punto flotante:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   La diferencia se calcula según las reglas aritméticas de IEEE 754. En la tabla siguiente se muestra los resultados de todas las posibles combinaciones de valores finitos distinto de cero, ceros, valores infinitos y NaN. En la tabla, `x` y `y` son valores finitos distinto de cero, y `z` es el resultado de `x - y`. Si `x` y `y` son iguales, `z` es cero positivo. Si `x - y` es demasiado grande para representarlo en el tipo de destino, `z` es infinito con el mismo signo que `x - y`.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   | NaN  | y    | +0   | -0   | +inf | -inf | NaN | 
   | x    | z    | x    | x    | -inf | +inf | NaN | 
   | +0   | -y   | +0   | +0   | -inf | +inf | NaN | 
   | -0   | -y   | -0   | +0   | -inf | +inf | NaN | 
   | +inf | +inf | +inf | +inf | NaN  | +inf | NaN | 
   | -inf | -inf | -inf | -inf | -inf | NaN  | NaN | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN | 

*  Resta decimal:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Si el valor resultante es demasiado grande para representarlo en el `decimal` formato, un `System.OverflowException` se produce. La escala del resultado, antes de cualquier operación de redondeo, es el mayor de las escalas de los dos operandos.

   Resta decimal es equivalente a usar el operador de resta de tipo `System.Decimal`.

*  Resta de la enumeración. Cada tipo de enumeración proporciona implícitamente el siguiente operador predefinido, donde `E` es el tipo enum y `U` es el tipo subyacente de `E`:

   ```csharp
   U operator -(E x, E y);
   ```

   Este operador se evalúa exactamente como `(U)((U)x - (U)y)`. En otras palabras, el operador calcula la diferencia entre los valores ordinales de `x` y `y`, y el tipo del resultado es el tipo subyacente de la enumeración.

   ```csharp
   E operator -(E x, U y);
   ```

   Este operador se evalúa exactamente como `(E)((U)x - y)`. En otras palabras, el operador de resta un valor de tipo subyacente de la enumeración, que produce un valor de la enumeración.

*  Eliminación de delegados. Cada tipo de delegado proporciona implícitamente el siguiente operador predefinido, donde `D` es el tipo de delegado:

   ```csharp
   D operator -(D x, D y);
   ```

   El archivo binario `-` operador realiza la eliminación de delegados cuando ambos operandos son de un tipo delegado `D`. Si los operandos tienen tipos de delegado distintos, se produce un error en tiempo de enlace. Si el primer operando es `null`, el resultado de la operación es `null`. En caso contrario, si el segundo operando es `null`, entonces el resultado de la operación es el valor del primer operando. En caso contrario, ambos operandos representan listas de invocación ([declaraciones de delegado](delegates.md#delegate-declarations)) que tiene una o más entradas y el resultado es una nueva lista de invocación que consta de lista del primer operando con entradas del segundo operando eliminadas proporciona la lista del segundo operando es una sublista apropiada contigua de la primera.     (Para determinar la igualdad de sublista, las entradas correspondientes se comparan en cuanto el operador de igualdad del delegado ([delegar los operadores de igualdad](expressions.md#delegate-equality-operators)).) En caso contrario, el resultado es el valor del operando izquierdo. Ninguna de las listas de los operandos se cambia en el proceso. Si la lista del segundo operando coincide con varias sublistas de entradas contiguas de la lista del primer operando, se quita la sublista coincidente más a la derecha de las entradas contiguas. Si la eliminación se produce en una lista vacía, el resultado es `null`. Por ejemplo:

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

## <a name="shift-operators"></a>Operadores de desplazamiento

El `<<` y `>>` operadores se utilizan para realizar operaciones de desplazamiento de bits.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Si un operando de un *shift_expression* tiene el tipo de tiempo de compilación `dynamic`, a continuación, la expresión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso es el tipo de tiempo de compilación de la expresión `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución de los operandos que tienen el tipo de tiempo de compilación `dynamic`.

Para una operación del formulario `x << count` o `x >> count`, resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.

Al declarar un operador de desplazamiento sobrecargado, el tipo del primer operando debe ser siempre la clase o estructura que contiene la declaración del operador y el tipo del segundo operando debe ser siempre `int`.

A continuación, se enumeran los operadores de desplazamiento predefinidos.

*  Desplazamiento a la izquierda:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   El `<<` operador turnos `x` izquierda un número de bits se calcula como se describe a continuación.

   Los bits de orden superior fuera del intervalo del tipo de resultado `x` son descartados, los bits restantes se desplazan a la izquierda y las posiciones de bits vacíos de orden inferior se establecen en cero.

*  Desplazamiento a la derecha:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   El `>>` operador turnos `x` derecha un número de bits se calcula como se describe a continuación.

   Cuando `x` es de tipo `int` o `long`, los bits de orden inferior de `x` son descartan, los bits restantes se desplazan a la derecha y las posiciones de bits vacíos de orden superior se establecen en cero si `x` es no negativo y establecido en uno si `x` es negativo.

   Cuando `x` es de tipo `uint` o `ulong`, los bits de orden inferior de `x` son descartados, los bits restantes se desplazan a la derecha y las posiciones de bits vacíos de orden superior se establecen en cero.

Para los operadores predefinidos, el número de bits de desplazamiento se calcula como sigue:

*  Cuando el tipo de `x` es `int` o `uint`, el valor de desplazamiento viene dado por los cinco bits de orden inferior de `count`. En otras palabras, se calcula el valor de desplazamiento de `count & 0x1F`.
*  Cuando el tipo de `x` es `long` o `ulong`, el valor de desplazamiento viene dado por los seis bits de orden inferior de `count`. En otras palabras, se calcula el valor de desplazamiento de `count & 0x3F`.

Si el valor de desplazamiento resultante es cero, los operadores de desplazamiento simplemente devuelven el valor de `x`.

Operaciones de desplazamiento nunca producen desbordamientos y producen el mismo resultado en `checked` y `unchecked` contextos.

Cuando el operando izquierdo de la `>>` operador es de un tipo entero con signo, el operador realiza un desplazamiento aritmético a la derecha, en la que el valor del bit más significativo (el bit de signo) del operando se propaga a las posiciones de bits vacíos de orden superior. Cuando el operando izquierdo de la `>>` operador es de tipo entero sin signo, el operador realiza un desplazamiento lógico directamente en la que las posiciones de bits vacíos de orden superior siempre se establecen en cero. Para realizar la operación contraria del deriva del tipo de operando, se pueden usar conversiones explícitas. Por ejemplo, si `x` es una variable de tipo `int`, la operación `unchecked((int)((uint)x >> y))` realiza un desplazamiento lógico derecha de `x`.

## <a name="relational-and-type-testing-operators"></a>Operadores relacionales y de comprobación de tipos

El `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` y `as` se denominan los operadores relacionales y comprobación de tipos.

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

El `is` operador se describe en [el operador is](expressions.md#the-is-operator) y el `as` operador se describe en [el operador as](expressions.md#the-as-operator).

El `==`, `!=`, `<`, `>`, `<=` y `>=` operadores son ***operadores de comparación***.

Si un operando de un operador de comparación con el tipo de tiempo de compilación `dynamic`, a continuación, la expresión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso es el tipo de tiempo de compilación de la expresión `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución de los operandos que tienen el tipo de tiempo de compilación `dynamic`.

Para una operación del formulario `x` *op* `y`, donde *op* es un operador de comparación, la resolución de sobrecarga ([deresolucióndesobrecargadeoperadorbinario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.

Los operadores de comparación predefinidos se describen en las secciones siguientes. Todos los operadores de comparación predefinidos devuelven un resultado de tipo `bool`, tal y como se describe en la tabla siguiente.


| __Operación__ | __Resultado__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true` Si `x` es igual a `y`, `false` en caso contrario                 | 
| `x != y`      | `true` Si `x` no es igual a `y`, `false` en caso contrario             | 
| `x < y`       | `true` Si `x` es menor que `y`, `false` en caso contrario                | 
| `x > y`       | `true` Si `x` es mayor que `y`, `false` en caso contrario             | 
| `x <= y`      | `true` Si `x` es menor o igual que `y`, `false` en caso contrario    | 
| `x >= y`      | `true` Si `x` es mayor o igual a `y`, `false` en caso contrario | 

### <a name="integer-comparison-operators"></a>Operadores de comparación de enteros

Los operadores de comparación de enteros predefinidos son:
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

Cada uno de estos operadores compara los valores numéricos de los dos operandos enteros y devuelve un `bool` valor que indica si la relación concreta es `true` o `false`.

### <a name="floating-point-comparison-operators"></a>Operadores de comparación de punto flotante

Los operadores de comparación de punto flotante predefinidos son:
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

Los operadores comparan los operandos según las reglas del estándar IEEE 754:

*  Si alguno de los operandos es NaN, el resultado es `false` para todos los operadores excepto `!=`, para que el resultado es `true`. Para los dos operandos, `x != y` siempre genera el mismo resultado que `!(x == y)`. Sin embargo, cuando uno o ambos operandos son NaN, la `<`, `>`, `<=`, y `>=` operadores no generan los mismos resultados que la negación lógica del operador opuesto. Por ejemplo, si alguno de `x` y `y` es NaN, a continuación, `x < y` es `false`, pero `!(x >= y)` es `true`.
*  Cuando ninguno de los operandos es NaN, los operadores comparan los valores de los dos operandos de punto flotante con respecto al orden

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   donde `min` y `max` son los valores mayor y menor positivos finitos que pueden representarse en el formato de punto flotante especificado. Efectos importantes de esta ordenación son:
   * Ceros negativos y positivos se consideran iguales.
   * Un infinito negativo se considera menor que todos los demás valores, pero igual que otro infinito negativo.
   * Se considera un infinito positivo mayor que todos los demás valores, pero igual que otro infinito positivo.

### <a name="decimal-comparison-operators"></a>Operadores de comparación decimal

Los operadores de comparación de decimales predefinidos son:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Cada uno de estos operadores compara los valores numéricos de los dos operandos decimales y devuelve un `bool` valor que indica si la relación concreta es `true` o `false`. Cada comparación decimal equivale al uso del operador de igualdad de tipo o correspondiente relacional `System.Decimal`.

### <a name="boolean-equality-operators"></a>Operadores de igualdad booleana

Los operadores de igualdad booleana predefinidos son:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

El resultado de `==` es `true` si ambos `x` y `y` son `true` o si ambos `x` y `y` son `false`. De lo contrario, el resultado es `false`.

El resultado de `!=` es `false` si ambos `x` y `y` son `true` o si ambos `x` y `y` son `false`. De lo contrario, el resultado es `true`. Cuando los operandos son de tipo `bool`, `!=` operador genera el mismo resultado que el `^` operador.

### <a name="enumeration-comparison-operators"></a>Operadores de comparación (enumeración)

Cada tipo de enumeración implícitamente proporciona los siguientes operadores de comparación predefinidos:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

El resultado de evaluar `x op y`, donde `x` y `y` son expresiones de un tipo de enumeración `E` con un tipo subyacente `U`, y `op` es uno de los operadores de comparación, es exactamente igual que evaluar `((U)x) op ((U)y)`. En otras palabras, los operadores de comparación de tipo de enumeración simplemente comparan los valores enteros subyacentes de los dos operandos.

### <a name="reference-type-equality-operators"></a>Operadores de igualdad de tipo de referencia

Los operadores de igualdad de tipo de referencia predefinidos son:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

Los operadores devuelven el resultado de comparar las dos referencias de igualdad o desigualdad.

Dado que los operadores de igualdad de referencia predefinidos tipo aceptan operandos de tipo `object`, se aplican a todos los tipos que no declaran aplicable `operator ==` y `operator !=` miembros. Por el contrario, los operadores de igualdad definido por el usuario aplicables ocultan eficazmente los operadores de igualdad de tipo de referencia predefinidos.

Los operadores de igualdad de tipo de referencia predefinidos requieren uno de los siguientes:

*  Ambos operandos son un valor de un tipo conocido como un *reference_type* o el literal `null`. Además, una conversión explícita de referencia ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) existe desde el tipo de uno de los operandos al tipo del otro operando.
*  Un operando es un valor de tipo `T` donde `T` es un *tipo_parámetro* y el otro operando es el literal `null`. Además `T` no tiene la restricción de tipo de valor.

A menos que una de estas condiciones son true, se produce un error en tiempo de enlace. Implicaciones importantes de estas reglas son:

*  Es un error en tiempo de enlace a usar los operadores de igualdad de tipo de referencia predefinidos para comparar dos referencias que se sabe que son diferentes en tiempo de enlace. Por ejemplo, si los tipos en tiempo de enlace de los operandos son dos tipos de clase `A` y `B`y si no `A` ni `B` se deriva de otra, sería imposible que los dos operandos para hacer referencia al mismo objeto. Por lo tanto, la operación se considera un error en tiempo de enlace.
*  Los operadores de igualdad de tipo de referencia predefinidos no permiten valor operandos de tipo va a comparar. Por lo tanto, a menos que un tipo de estructura declara sus propios operadores de igualdad, no es posible comparar valores de ese tipo de estructura.
*  Los operadores de igualdad de tipo de referencia predefinidos nunca producen las operaciones de conversión boxing se producen para sus operandos. Tendría sentido para realizar estas operaciones de conversión boxing, puesto que las referencias a las instancias de conversión boxing recién asignadas necesariamente podrían diferir de todas las demás referencias.
*  Si un operando de un parámetro de tipo `T` se compara con `null`y el tipo de tiempo de ejecución de `T` es un tipo de valor, el resultado de la comparación es `false`.

El ejemplo siguiente se comprueba si un argumento de un tipo de parámetro de tipo sin restricciones es `null`.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

El `x == null` , aunque se permite la construcción `T` podría representar un tipo de valor y el resultado se define simplemente como `false` cuando `T` es un tipo de valor.

Para una operación del formulario `x == y` o `x != y`, si procede cualquier `operator ==` o `operator !=` existe, la resolución de sobrecarga de operador ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) las reglas que seleccionará operador en lugar del operador de igualdad de tipo de referencia predefinidos. Sin embargo, siempre es posible seleccionar el operador de igualdad de referencia predefinidos tipo convirtiendo de forma explícita uno o ambos operandos al tipo `object`. El ejemplo
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
genera el resultado
```
True
False
False
False
```

El `s` y `t` variables hacen referencia a dos distintos `string` instancias que contienen los mismos caracteres. La primera comparación da como resultado `True` porque el operador de igualdad de cadenas predefinido ([operadores de igualdad de cadenas](expressions.md#string-equality-operators)) está activada cuando ambos operandos son del tipo `string`. Salida de todas las comparaciones restantes `False` porque se ha seleccionado el operador de igualdad de tipo de referencia predefinidos cuando uno o ambos operandos son del tipo `object`.

Tenga en cuenta que la técnica anterior no es significativa para los tipos de valor. El ejemplo
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
da como resultado `False` porque las conversiones de tipos para crear referencias a dos instancias independientes de conversión boxing `int` valores.

### <a name="string-equality-operators"></a>Operadores de igualdad de cadenas

Los operadores de igualdad de cadenas predefinidos son:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Dos `string` valores se consideran iguales cuando se cumple una de las siguientes acciones:

*  Los dos valores son `null`.
*  Ambos valores son distintos de null referencias a instancias de cadenas que tienen una longitud idéntica y caracteres idénticos en cada posición de carácter.

Los operadores de igualdad de cadenas comparan los valores de cadena en lugar de las referencias de cadena. Cuando dos instancias de cadena distintas contienen exactamente la misma secuencia de caracteres, los valores de las cadenas son iguales, pero las referencias son diferentes. Como se describe en [operadores de igualdad de referencia tipo](expressions.md#reference-type-equality-operators), los operadores de igualdad de tipo de referencia pueden usarse para comparar las referencias de cadena en lugar de los valores de cadena.

### <a name="delegate-equality-operators"></a>Operadores de igualdad de delegado

Cada tipo de delegado implícitamente proporciona los siguientes operadores de comparación predefinidos:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

Delegado de dos instancias se consideran iguales como sigue:

*  Si se da alguna de las instancias de delegado `null`, son iguales solo si ambos son `null`.
*  Si los delegados tienen diferentes tipos en tiempo de ejecución nunca son iguales.
*  Si ambas de las instancias de delegado tienen una lista de invocación ([declaraciones de delegado](delegates.md#delegate-declarations)), las instancias son iguales si y solo si sus listas de invocaciones tienen la misma longitud, y cada entrada en la lista de invocación es igual (tal y como se define a continuación) a la entrada correspondiente, en orden, en la lista de invocaciones de la otra.

Las siguientes reglas determinan la igualdad de las entradas de la lista de invocación:

*  Si dos invocación entradas de la lista ambos hacen referencia al mismo estático método, a continuación, las entradas son iguales.
*  Si dos invocación entradas de la lista ambos hacen referencia al mismo método no estático en el mismo objeto de destino (como se define por los operadores de igualdad de referencia), a continuación, las entradas son iguales.
*  Genera entradas de la lista de invocación de la evaluación de semánticamente idéntico *anonymous_method_expression*s o *lambda_expression*s con el mismo conjunto (posiblemente vacío) de la variable externa capturada las instancias se permiten (aunque no es necesario) para que sea igual.

### <a name="equality-operators-and-null"></a>NULL y los operadores de igualdad

El `==` y `!=` operadores permiten un operando tenga un valor de un tipo que acepta valores NULL y el otro sea el `null` literal, aunque no exista ningún operador predefinido o definidos por el usuario (en no sea de elevación o formato de elevación) para la operación.

Para una operación de uno de los formularios
```csharp
x == null
null == x
x != null
null != x
```
donde `x` es una expresión de un tipo que acepta valores NULL, si el operador de resolución de sobrecarga ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) no se puede encontrar un operador aplicable, el resultado en su lugar, se calcula a partir del `HasValue` propiedad de `x`. En concreto, se traducen los dos primeros formularios `!x.HasValue`, y se traducen los dos últimos formularios `x.HasValue`.

### <a name="the-is-operator"></a>El operador is

El `is` operador se usa para comprobar si el tipo de tiempo de ejecución de un objeto es compatible con un tipo determinado de manera dinámica. El resultado de la operación `E is T`, donde `E` es una expresión y `T` es un tipo, es un valor booleano que indica el valor si `E` correctamente puede convertirse al tipo `T` mediante una conversión de referencia, una conversión boxing conversión, o una conversión unboxing. La operación se evalúa como sigue, después de argumentos de tipo se sustituyeron por todos los parámetros de tipo:

*  Si `E` es una función anónima, se produce un error de tiempo de compilación
*  Si `E` es un grupo de métodos o `null` literales de if el tipo de `E` es un tipo de referencia o un tipo que acepta valores NULL y el valor de `E` es null, el resultado es false.
*  De lo contrario, deje que `D` representan el tipo dinámico de `E` como sigue:
   * Si el tipo de `E` es un tipo de referencia, `D` es el tipo de tiempo de ejecución de la referencia de instancia por `E`.
   * Si el tipo de `E` es un tipo que acepta valores NULL, `D` es el tipo subyacente de ese tipo que acepta valores NULL.
   * Si el tipo de `E` es un tipo de valor distinto de null, `D` es el tipo de `E`.
*  El resultado de la operación depende `D` y `T` como sigue:
   * Si `T` es un tipo de referencia, el resultado es true si `D` y `T` son del mismo tipo, si `D` es un tipo de referencia y una conversión implícita de referencia de `D` a `T` existe, o si `D` es un tipo de valor y una conversión boxing de `D` a `T` existe.
   * Si `T` es un tipo que acepta valores NULL, el resultado es true si `D` es el tipo subyacente de `T`.
   * Si `T` es un tipo de valor distinto de null, el resultado es true si `D` y `T` son del mismo tipo.
   * En caso contrario, el resultado es false.

Tenga en cuenta que las conversiones definidas por el usuario, no tiene en cuenta el `is` operador.

### <a name="the-as-operator"></a>El operador as

El `as` operador se usa para convertir explícitamente un valor a un tipo que acepta valores NULL o un tipo de referencia especificada. A diferencia de una expresión de conversión ([expresiones de conversión](expressions.md#cast-expressions)), el `as` operador nunca produce una excepción. En su lugar, si la conversión indicada no es posible, el valor resultante es `null`.

En una operación del formulario `E as T`, `E` debe ser una expresión y `T` debe ser un tipo de referencia, un parámetro de tipo que se sabe que es un tipo de referencia o un tipo que acepta valores NULL. Además, al menos uno de los siguientes debe ser true o en caso contrario, se produce un error en tiempo de compilación:

*  Una identidad ([conversión de identidad](conversions.md#identity-conversion)), implícita que acepta valores null ([las conversiones implícitas que acepta valores NULL](conversions.md#implicit-nullable-conversions)), una referencia implícita ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)), la conversión boxing ([ Las conversiones boxing](conversions.md#boxing-conversions)) explícito que acepta valores null ([conversiones que aceptan valores NULL explícitas](conversions.md#explicit-nullable-conversions)), una referencia explícita ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)), unboxing o ([Conversiones Unboxing](conversions.md#unboxing-conversions)) no existe conversión de `E` a `T`.
*  El tipo de `E` o `T` es un tipo abierto.
*  `E` es el `null` literal.

Si el tipo de tiempo de compilación de `E` no `dynamic`, la operación `E as T` produce el mismo resultado que
```csharp
E is T ? (T)(E) : (T)null
```
salvo que `E` solo se evalúa una vez. Cabe esperar que el compilador optimice `E as T` para realizar a lo sumo una comprobación de tipo dinámico en lugar de las dos comprobaciones de tipo dinámico implicadas en la expansión de la anterior.

Si el tipo de tiempo de compilación de `E` es `dynamic`, a diferencia del operador de conversión el `as` operador no se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). Por lo tanto, la expansión en este caso es:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Tenga en cuenta que algunas conversiones, como las conversiones definidas por el usuario no son posibles con la `as` operador y debe realizarse mediante expresiones de conversión.

En el ejemplo
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
el parámetro de tipo `T` de `G` se sabe que es un tipo de referencia, porque tiene la restricción de clase. El parámetro de tipo `U` de `H` no es cambio; por lo tanto, el uso de la `as` operador en `H` no está permitida.

## <a name="logical-operators"></a>Operadores lógicos

El `&`, `^`, y `|` se denominan los operadores lógicos.

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

Si un operando de un operador lógico tiene el tipo de tiempo de compilación `dynamic`, a continuación, la expresión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso es el tipo de tiempo de compilación de la expresión `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución de los operandos que tienen el tipo de tiempo de compilación `dynamic`.

Para una operación del formulario `x op y`, donde `op` es uno de los operadores lógicos, la resolución de sobrecarga ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específico. Los operandos se convierten en los tipos de parámetro del operador seleccionado, y el tipo del resultado es el tipo de valor devuelto del operador.

Los operadores lógicos predefinidos se describen en las secciones siguientes.

### <a name="integer-logical-operators"></a>Operadores lógicos enteros

Los operadores lógicos enteros predefinidos son:
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

El `&` operador calcula bit a bit lógica `AND` de los dos operandos, el `|` operador calcula bit a bit lógica `OR` de los dos operandos y el `^` operador calcula lógica exclusiva bit a bit `OR` de los dos operandos. Los desbordamientos no son posibles en estas operaciones.

### <a name="enumeration-logical-operators"></a>Operadores lógicos de enumeración

Cada tipo de enumeración `E` proporcionan de forma implícita predefinida de los siguientes operadores lógicos:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

El resultado de evaluar `x op y`, donde `x` y `y` son expresiones de un tipo de enumeración `E` con un tipo subyacente `U`, y `op` es uno de los operadores lógicos, es exactamente igual que evaluar `(E)((U)x op (U)y)`. En otras palabras, los operadores lógicos de tipo de enumeración simplemente realizan la operación lógica en el tipo subyacente de los dos operandos.

### <a name="boolean-logical-operators"></a>Operadores lógicos booleanos

Los operadores lógicos boolean predefinidos son:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

El resultado de `x & y` es `true` si tanto `x` como `y` son `true`. De lo contrario, el resultado es `false`.

El resultado de `x | y` es `true` si `x` o `y` es `true`. De lo contrario, el resultado es `false`.

El resultado de `x ^ y` es `true` si `x` es `true` y `y` es `false`, o `x` es `false` y `y` es `true`. De lo contrario, el resultado es `false`. Cuando los operandos son de tipo `bool`, `^` operador calcula el mismo resultado que el `!=` operador.

### <a name="nullable-boolean-logical-operators"></a>Operadores lógicos de tipo boolean que acepta valores null

El tipo booleano que acepta valores NULL `bool?` puede representar los tres valores, `true`, `false`, y `null`y es conceptualmente similar al tipo de tres valores utilizado por expresiones booleanas en SQL. Para asegurarse de que los resultados producidos por el `&` y `|` operadores para `bool?` son coherentes con la lógica de tres valores de SQL, se proporcionan los siguientes operadores predefinidos:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

En la tabla siguiente se enumera los resultados generados por estos operadores para todas las combinaciones de los valores de `true`, `false`, y `null`.

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

## <a name="conditional-logical-operators"></a>Operadores lógicos condicionales

El `&&` y `||` operadores se denominan los operadores lógicos condicionales. También se denominan los operadores lógicos de "cortocircuito".

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

El `&&` y `||` operadores son versiones condicionales de la `&` y `|` operadores:

*  La operación `x && y` corresponde a la operación `x & y`, salvo que `y` solo se evalúa si `x` no `false`.
*  La operación `x || y` corresponde a la operación `x | y`, salvo que `y` solo se evalúa si `x` no `true`.

Si un operando de un operador lógico condicional tiene el tipo de tiempo de compilación `dynamic`, a continuación, la expresión se enlaza dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso es el tipo de tiempo de compilación de la expresión `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución con el tipo de tiempo de ejecución de los operandos que tienen el tipo de tiempo de compilación `dynamic`.

Una operación del formulario `x && y` o `x || y` se procesa mediante la aplicación de la resolución de sobrecarga ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) como si la operación se ha escrito `x & y` o `x | y`. A continuación,

*  Si se produce un error en la resolución de sobrecarga buscar un único operador mejor, o si la resolución de sobrecarga selecciona uno de los operadores lógicos enteros predefinidos, se produce un error en tiempo de enlace.
*  En caso contrario, si el operador seleccionado es uno de los operadores lógicos booleanos predefinidos ([operadores lógicos Boolean](expressions.md#boolean-logical-operators)) o en operadores lógicos de tipo boolean que acepta valores null ([operadores lógicos de tipo boolean que acepta valores NULL](expressions.md#nullable-boolean-logical-operators)), el se ha procesado la operación tal como se describe en [booleano operadores lógicos condicionales](expressions.md#boolean-conditional-logical-operators).
*  En caso contrario, el operador seleccionado es un operador definido por el usuario y la operación se procesa como se describe en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators).

No es posible sobrecargar directamente los operadores lógicos condicionales. Sin embargo, dado que los operadores lógicos condicionales se evalúan en cuanto a los operadores lógicos normales, las sobrecargas de los operadores lógicos normales, con algunas restricciones, también consideran las sobrecargas de los operadores lógicos condicionales. Esto se describe más detalladamente en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators).

### <a name="boolean-conditional-logical-operators"></a>Operadores lógicos condicionales booleanos

Cuando los operandos de `&&` o `||` son del tipo `bool`, o cuando los operandos son de tipos que no definen un aplicable `operator &` o `operator |`, pero definir conversiones implícitas a `bool`, es la operación procesado como sigue:

*  La operación `x && y` se evalúa como `x ? y : false`. En otras palabras, `x` se evalúa primero y se puede convertir al tipo `bool`. A continuación, si `x` es `true`, `y` se evalúa y se convierte al tipo `bool`, y esto se convierte en el resultado de la operación. En caso contrario, el resultado de la operación es `false`.
*  La operación `x || y` se evalúa como `x ? true : y`. En otras palabras, `x` se evalúa primero y se puede convertir al tipo `bool`. A continuación, si `x` es `true`, el resultado de la operación es `true`. En caso contrario, `y` se evalúa y se convierte al tipo `bool`, y esto se convierte en el resultado de la operación.

### <a name="user-defined-conditional-logical-operators"></a>Operadores lógicos condicionales definidos por el usuario

Cuando los operandos de `&&` o `||` son de tipos que declaran aplicable definido por el usuario `operator &` o `operator |`, ambos de los siguientes deben ser verdaderas, donde `T` es el tipo en el que se declara el operador seleccionado:

*  El tipo de valor devuelto y el tipo de cada parámetro del operador seleccionado deben ser `T`. En otras palabras, el operador debe calcular la lógica `AND` o lógico `OR` de dos operandos de tipo `T`y debe devolver un resultado de tipo `T`.
*  `T` debe incluir declaraciones de `operator true` y `operator false`.

Si no se cumple alguno de estos requisitos, se produce un error en tiempo de enlace. En caso contrario, el `&&` o `||` operación se evalúa mediante la combinación de definido por el usuario `operator true` o `operator false` con el operador definido por el usuario seleccionado:

*  La operación `x && y` se evalúa como `T.false(x) ? x : T.&(x, y)`, donde `T.false(x)` es una invocación de la `operator false` declarado en `T`, y `T.&(x, y)` es una invocación del seleccionado `operator &`. En otras palabras, `x` se evalúa primero y `operator false` se invoca en el resultado para determinar si `x` es definitivamente false. A continuación, si `x` es definitivamente false, el resultado de la operación es el valor previamente calculado para `x`. En caso contrario, `y` se evalúa y seleccionado `operator &` se invoca en el valor previamente calculado para `x` y el valor calculado para `y` para generar el resultado de la operación.
*  La operación `x || y` se evalúa como `T.true(x) ? x : T.|(x, y)`, donde `T.true(x)` es una invocación de la `operator true` declarado en `T`, y `T.|(x,y)` es una invocación del seleccionado `operator|`. En otras palabras, `x` se evalúa primero y `operator true` se invoca en el resultado para determinar si `x` es definitivamente true. A continuación, si `x` es definitivamente es true, el resultado de la operación es el valor previamente calculado para `x`. En caso contrario, `y` se evalúa y seleccionado `operator |` se invoca en el valor previamente calculado para `x` y el valor calculado para `y` para generar el resultado de la operación.

En cualquiera de estas operaciones, la expresión proporcionada por `x` es solo evalúa una vez y la expresión proporcionada por `y` no se evalúa o evaluar exactamente una vez.

Para obtener un ejemplo de un tipo que implementa `operator true` y `operator false`, consulte [base de datos de tipo booleano](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>El operador de uso combinado de null

El `??` operador se denomina operador de fusión null.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Una expresión de fusión null del formulario `a ?? b` requiere `a` a ser de un tipo de referencia o de tipo que acepta valores NULL. Si `a` es distinto de null, el resultado de `a ?? b` es `a`; en caso contrario, el resultado es `b`. La operación evalúa `b` solo si `a` es null.

El operador de fusión null es asociativo a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda. Por ejemplo, una expresión de formato `a ?? b ?? c` se evalúa como `a ?? (b ?? c)`. Términos en general, una expresión de formato `E1 ?? E2 ?? ... ?? En` devuelve el primero de los operandos que es distinto de null, o null si todos los operandos son nulos.

El tipo de la expresión `a ?? b` depende de qué conversiones implícitas están disponibles en los operandos. En el orden de preferencia, el tipo de `a ?? b` es `A0`, `A`, o `B`, donde `A` es el tipo de `a` (siempre que `a` tiene un tipo), `B` es el tipo de `b` () siempre que `b` tiene un tipo), y `A0` es el tipo subyacente de `A` si `A` es un tipo que acepta valores NULL, o `A` en caso contrario. En concreto, `a ?? b` se procesa como sigue:

*  Si `A` existe y no es un tipo que acepta valores NULL o un tipo de referencia, se produce un error de tiempo de compilación.
*  Si `b` es una expresión dinámica, el tipo de resultado es `dynamic`. En tiempo de ejecución, `a` se evalúa primero. Si `a` no es null, `a` se convierte en dinámico, y esto se convierte en el resultado. En caso contrario, `b` se evalúa, y esto se convierte en el resultado.
*  De lo contrario, si `A` existe y es un tipo que acepta valores NULL y existe una conversión implícita de `b` a `A0`, el tipo de resultado es `A0`. En tiempo de ejecución, `a` se evalúa primero. Si `a` no es null, `a` se desempaqueta para escriba `A0`, y esto se convierte en el resultado. En caso contrario, `b` se evalúa y se convierte al tipo `A0`, y esto se convierte en el resultado.
*  De lo contrario, si `A` existe y no existe una conversión implícita de `b` a `A`, el tipo de resultado es `A`. En tiempo de ejecución, `a` se evalúa primero. Si `a` no es null, `a` se convierte en el resultado. En caso contrario, `b` se evalúa y se convierte al tipo `A`, y esto se convierte en el resultado.
*  De lo contrario, si `b` tiene un tipo `B` y existe una conversión implícita de `a` a `B`, el tipo de resultado es `B`. En tiempo de ejecución, `a` se evalúa primero. Si `a` no es null, `a` se desempaqueta para escriba `A0` (si `A` existe y que acepta valores NULL) y convertir al tipo `B`, y esto se convierte en el resultado. En caso contrario, `b` se evalúa y se convierte en el resultado.
*  En caso contrario, `a` y `b` son incompatibles y un error en tiempo de compilación se produce.

## <a name="conditional-operator"></a>Operador condicional

El `?:` operador se denomina operador condicional. A veces también se denomina el operador ternario.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Una expresión condicional del formulario `b ? x : y` primero evalúa la condición `b`. A continuación, si `b` es `true`, `x` se evalúa y se convierte en el resultado de la operación. En caso contrario, `y` se evalúa y se convierte en el resultado de la operación. Una expresión condicional nunca evalúa ambos `x` y `y`.

El operador condicional es asociativo a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda. Por ejemplo, una expresión de formato `a ? b : c ? d : e` se evalúa como `a ? b : (c ? d : e)`.

El primer operando de la `?:` operador debe ser una expresión que se puede convertir implícitamente a `bool`, o una expresión de un tipo que implementa `operator true`. Si se cumple ninguno de estos requisitos, se produce un error en tiempo de compilación.

Los operandos segundo y tercero, `x` y `y`, de la `?:` operador controlar el tipo de la expresión condicional.

*  Si `x` tiene tipo `X` y `y` tiene tipo `Y` , a continuación,
   * Si una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde `X` a `Y`, pero no desde `Y` a `X`, a continuación, `Y` es el tipo de la expresión condicional.
   * Si una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde `Y` a `X`, pero no desde `X` a `Y`, a continuación, `X` es el tipo de la expresión condicional.
   * En caso contrario, no se puede determinar ningún tipo de expresión y se produce un error de tiempo de compilación.
*  Si solo uno de `x` y `y` tiene un tipo y ambos `x` y `y`, de son implícitamente convertible a ese tipo, que es el tipo de la expresión condicional.
*  En caso contrario, no se puede determinar ningún tipo de expresión y se produce un error de tiempo de compilación.

El procesamiento en tiempo de ejecución de una expresión condicional del formulario `b ? x : y` consta de los pasos siguientes:

*  Primero, `b` se evalúa y la `bool` valor `b` viene determinada:
   * Si una conversión implícita del tipo de `b` a `bool` existe, se realiza esta conversión implícita para producir un `bool` valor.
   * En caso contrario, el `operator true` definido por el tipo de `b` se invoca para generar un `bool` valor.
*  Si el `bool` es el valor generado por el paso anterior `true`, a continuación, `x` se evalúa y se convierte al tipo de la expresión condicional, y esto se convierte en el resultado de la expresión condicional.
*  En caso contrario, `y` se evalúa y se convierte al tipo de la expresión condicional, y esto se convierte en el resultado de la expresión condicional.

## <a name="anonymous-function-expressions"></a>Expresiones de función anónima

Un ***función anónima*** es una expresión que representa una definición de método "en línea". Una función anónima no tiene un valor o el tipo de por sí, pero puede convertirse en un tipo de árbol de expresión o delegado compatible. La evaluación de una conversión de función anónima depende del tipo de destino de la conversión: Si es un tipo de delegado, la conversión se evalúa como un valor de delegado, hacer referencia al método que define la función anónima. Si es un tipo de árbol de expresión, la conversión se evalúa como un árbol de expresión que representa la estructura del método como una estructura de objeto.

Por motivos históricos hay dos variantes sintácticas de funciones anónimas, es decir, *lambda_expression*s y *anonymous_method_expression*s. Para casi todos los propósitos, *lambda_expression*s son más concisas y expresivas que *anonymous_method_expression*s, que permanecen en el idioma para compatibilidad con versiones anteriores.

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

El `=>` operador tiene la misma prioridad que la asignación (`=`) y es asociativo a la derecha.

Una función anónima con el `async` modificador es una función asincrónica y sigue las reglas descritas en [iteradores](classes.md#iterators).

Los parámetros de una función anónima en forma de un *lambda_expression* puede estar escrito explícitamente o implícitamente. En una lista de parámetros con tipo explícito, se indica explícitamente el tipo de cada parámetro. En una lista de parámetros con tipo implícito se deducen los tipos de los parámetros de contexto en el que se produce la función anónima; en concreto, cuando la función anónima se convierte en un tipo delegado compatible o el tipo de árbol de expresión, que proporciona el tipo los tipos de parámetros ([conversiones de función anónima](conversions.md#anonymous-function-conversions)).

En una función anónima con un parámetro único, implícito, se pueden omitir los paréntesis de la lista de parámetros. En otras palabras, una función anónima del formulario
```csharp
( param ) => expr
```
se puede abreviar a
```csharp
param => expr
```

La lista de parámetros de una función anónima en forma de un *anonymous_method_expression* es opcional. Si no especifica, los parámetros de tipo deben establecerse explícitamente. Si no, la función anónima es convertible a un delegado con cualquier parámetro de lista que no contenga `out` parámetros.

Un *bloque* cuerpo de una función anónima es accesible ([puntos finales y alcance](statements.md#end-points-and-reachability)) a menos que la función anónima que se produce dentro de una instrucción inaccesible.

Algunos ejemplos de funciones anónimas siguen siguientes:

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

El comportamiento de *lambda_expression*s y *anonymous_method_expression*s es el mismo, excepto los siguientes puntos:

*  *anonymous_method_expression*s permiten la lista de parámetros para omitir por completo, lo que produce la conversión de varianza para los tipos de cualquier lista de parámetros con valores de delegado.
*  *lambda_expression*s permite a los tipos de parámetro se omite y se deduce, mientras que *anonymous_method_expression*s requieren tipos de parámetro que se indique explícitamente.
*  El cuerpo de un *lambda_expression* puede ser una expresión o un bloque de instrucciones mientras que el cuerpo de un *anonymous_method_expression* debe ser un bloque de instrucciones.
*  Solo *lambda_expression*s tienen conversiones a tipos de árbol de expresión compatible ([tipos de árbol de expresión](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Firmas de función anónima

El elemento opcional *anonymous_function_signature* de una función anónima define los nombres y, opcionalmente, los tipos de los parámetros formales para la función anónima. El ámbito de los parámetros de la función anónima es el *anonymous_function_body*. ([Ámbitos](basic-concepts.md#scopes)) junto con la lista de parámetros (si se indica) anónimo-method-body constituye un espacio de declaración ([declaraciones](basic-concepts.md#declarations)). Por tanto, es un error en tiempo de compilación para el nombre de un parámetro de la función anónima para que coincida con el nombre de una variable local, constante local o parámetro cuyo ámbito incluye el *anonymous_method_expression* o *lambda_ expresión*.

Si tiene una función anónima un *explicit_anonymous_function_signature*, a continuación, el conjunto de tipos de delegado compatibles y los tipos de árbol de expresión está restringido a los que tienen los mismos tipos de parámetros y modificadores en el mismo orden. A diferencia de las conversiones de grupo de métodos ([conversiones de métodos de grupo](conversions.md#method-group-conversions)), no se admite la contravarianza de tipos de parámetro de función anónima. Si no dispone de una función anónima un *anonymous_function_signature*, a continuación, el conjunto de tipos de delegado compatibles y los tipos de árbol de expresión está restringido a aquellos que no tienen ningún `out` parámetros.

Tenga en cuenta que un *anonymous_function_signature* no puede incluir atributos o una matriz de parámetros. No obstante, un *anonymous_function_signature* puede ser compatible con un tipo de delegado cuya lista de parámetros contiene una matriz de parámetros.

Tenga en cuenta también que la conversión a un tipo de árbol de expresión, incluso si compatible, todavía puede producir un error en tiempo de compilación ([tipos de árbol de expresión](types.md#expression-tree-types)).

### <a name="anonymous-function-bodies"></a>Cuerpos de función anónima

El cuerpo (*expresión* o *bloque*) de una función anónima está sujeto a las reglas siguientes:

*  Si la función anónima incluye una firma, están disponibles en el cuerpo de los parámetros especificados en la firma. Si la función anónima no tiene una firma se puede convertir a un tipo de delegado o expresión con parámetros ([conversiones de función anónima](conversions.md#anonymous-function-conversions)), pero no se puede acceder a los parámetros en el cuerpo.
*  Excepto para `ref` o `out` los parámetros especificados en la firma (si existe) de la envolvente más próximo función anónima, es un error en tiempo de compilación para el cuerpo tener acceso a un `ref` o `out` parámetro.
*  Cuando el tipo de `this` es un tipo struct, es un error en tiempo de compilación para el cuerpo tener acceso a `this`. Esto es cierto si el acceso es explícito (como en `this.x`) o implícita (como en `x` donde `x` es un miembro de instancia de la estructura). Esta regla simplemente prohíbe dicho acceso y no afecta a si los resultados de búsqueda de miembros en un miembro de la estructura.
*  El cuerpo tiene acceso a las variables externas ([Outer variables](expressions.md#outer-variables)) de la función anónima. Acceso de una variable externa hará referencia a la instancia de la variable que está activa en el momento del *lambda_expression* o *anonymous_method_expression* se evalúa ([evaluación de las expresiones de función anónima](expressions.md#evaluation-of-anonymous-function-expressions)).
*  Es un error en tiempo de compilación para el cuerpo contenga un `goto` instrucción, `break` instrucción, o `continue` instrucción cuyo destino es fuera del cuerpo o en el cuerpo de una función anónima contenida.
*  Un `return` instrucción del cuerpo devuelve el control de una invocación de envolvente función anónima, no desde el miembro de función envolvente. Una expresión especificada en un `return` instrucción debe poder convertirse implícitamente al tipo de valor devuelto del tipo de delegado o tipo de árbol de expresión al que envolvente *lambda_expression* o *anonymous_ method_expression* se convierte ([conversiones de función anónima](conversions.md#anonymous-function-conversions)).

No se especifica explícitamente si hay alguna forma de ejecutar el bloque de una función anónima distinto a través de evaluación y la invocación de la *lambda_expression* o *anonymous_method_expression*. En concreto, el compilador puede optar por implementar una función anónima si sintetiza uno o más con el nombre de los métodos o tipos. Los nombres de esos elementos sintetizados deben ser de un formulario reservado para uso del compilador.

### <a name="overload-resolution-and-anonymous-functions"></a>La resolución de sobrecarga y las funciones anónimas

Las funciones anónimas en una lista de argumentos participan en la inferencia de tipos y resolución de sobrecarga. Consulte [inferencia](expressions.md#type-inference) y [de resolución de sobrecarga](expressions.md#overload-resolution) para ver las reglas.

El ejemplo siguiente muestra el efecto de las funciones anónimas en la resolución de sobrecarga.

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

El `ItemList<T>` clase tiene dos `Sum` métodos. Cada uno toma una `selector` argumento, que extrae el valor a suma a través de un elemento de lista. El valor extraído puede ser un `int` o un `double` y la suma resultante sea del mismo modo un `int` o `double`.

El `Sum` métodos por ejemplo podrían utilizarse para calcular las sumas de una lista de líneas de detalles en un orden.

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

En la primera invocación de `orderDetails.Sum`, ambos `Sum` métodos son aplicables porque la función anónima `d => d. UnitCount` es compatible con ambos `Func<Detail,int>` y `Func<Detail,double>`. Sin embargo, la resolución de sobrecarga selecciona la primera `Sum` método porque la conversión a `Func<Detail,int>` es mejor que la conversión a `Func<Detail,double>`.

En la segunda invocación de `orderDetails.Sum`, solo el segundo `Sum` método es aplicable porque la función anónima `d => d.UnitPrice * d.UnitCount` genera un valor de tipo `double`. Por lo tanto, la sobrecarga resolución elige la segunda `Sum` método para esa invocación.

### <a name="anonymous-functions-and-dynamic-binding"></a>Funciones anónimas y el enlace dinámico

Una función anónima no puede ser un receptor, un argumento o un operando de una operación dinámica enlazada.

### <a name="outer-variables"></a>Variables externas

Cualquier variable local, el parámetro de valor o la matriz de parámetros cuyo ámbito incluye el *lambda_expression* o *anonymous_method_expression* se denomina un ***variable externa*** de la función anónima. En un miembro de función de la instancia de una clase, el `this` valor se considera un parámetro de valor y es una variable externa de cualquier función anónima contenida dentro del miembro de función.

#### <a name="captured-outer-variables"></a>Captura de variables externas

Cuando se hace referencia a una variable externa por una función anónima, se dice que la variable externa han sido ***capturan*** por la función anónima. Normalmente, la duración de una variable local está limitada por la ejecución del bloque o instrucción que está asociada ([variables locales](variables.md#local-variables)). Sin embargo, la duración de una variable externa capturada se extiende al menos hasta que el delegado o árbol de expresión que se crea a partir de la función anónima se vuelve apto para la recolección.

En el ejemplo
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
la variable local `x` capturada por la función anónima y la duración de `x` se extiende al menos hasta que el delegado devuelto por `F` se convierte en apto para la recolección de elementos no utilizados (que no ocurre hasta el final de el programa). Puesto que cada invocación de la función anónima opera en la misma instancia de `x`, el resultado del ejemplo es:
```
1
2
3
```

Cuando se captura una variable local o un parámetro de valor mediante una función anónima, el parámetro o variable local ya no se considera que una variable fija ([fijos y variables móviles](unsafe-code.md#fixed-and-moveable-variables)), pero en su lugar, se considera un moveable variable. Por lo tanto, cualquier `unsafe` en primer lugar debe usar el código que toma la dirección de una variable externa capturada el `fixed` instrucción para fijar la variable.

Tenga en cuenta que a diferencia de una variable no capturada, una variable local capturada se puede exponer simultáneamente a varios subprocesos de ejecución.

#### <a name="instantiation-of-local-variables"></a>Creación de instancias de las variables locales

Se considera una variable local ***crea una instancia*** cuando entra la ejecución en el ámbito de la variable. Por ejemplo, cuando se invoca el método siguiente, la variable local `x` se crea una instancia e inicializa tres veces, una vez para cada iteración del bucle.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Sin embargo, mueva la declaración de `x` fuera de los resultados de bucle en una única instancia de `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Cuando no se capturan, no hay ninguna manera de observar exactamente, ¿con qué frecuencia se crea una instancia de una variable local, porque la duración de la creación de instancias no es contiguas, es posible que cada instancia simplemente utilizar la misma ubicación de almacenamiento. Sin embargo, cuando una función anónima captura una variable local, los efectos de la creación de instancias se hacen evidentes.

El ejemplo
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
genera el resultado:
```
1
3
5
```

Sin embargo, cuando la declaración de `x` se mueve fuera del bucle:
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
El resultado es:
```
5
5
5
```

Si un bucle for declara una variable de iteración, esa variable en sí se considera que se declara fuera del bucle. Por lo tanto, si se cambia el ejemplo para capturar la propia variable de iteración:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
se captura solo una instancia de la variable de iteración, lo que genera el resultado:
```
3
3
3
```

Es posible que los delegados de función anónima compartir algunas de las variables capturadas aún tiene instancias separadas de otras. Por ejemplo, si `F` se cambia a
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
la misma instancia de capturan de los tres delegados `x` pero instancias independientes de `y`, y el resultado es:
```
1 1
2 1
3 1
```

Funciones anónimas independientes pueden capturar la misma instancia de una variable externa. En el ejemplo:
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
las dos funciones anónimas capturan la misma instancia de la variable local `x`, y se puede, por tanto, "comunican" a través de esa variable. El resultado del ejemplo es:
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Evaluación de expresiones de función anónima

Una función anónima `F` siempre se deben convertir a un tipo delegado `D` o un tipo de árbol de expresión `E`, ya sea directamente o a través de la ejecución de una expresión de creación de delegado `new D(F)`. Esta conversión determina el resultado de la función anónima, tal como se describe en [conversiones de función anónima](conversions.md#anonymous-function-conversions).

## <a name="query-expressions"></a>Expresiones de consulta

***Las expresiones de consulta*** proporcionan una sintaxis de lenguaje integrado para las consultas que es similar a lenguajes de consulta jerárquica y relacional como SQL y XQuery.

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

Una expresión de consulta comienza con un `from` cláusula y finaliza con ya sea un `select` o `group` cláusula. Inicial `from` cláusula puede ir seguida de cero o más `from`, `let`, `where`, `join` o `orderby` cláusulas. Cada `from` cláusula es un generador de introducir un ***variable de rango*** cuyos intervalos van a través de los elementos de un ***secuencia***. Cada `let` cláusula introduce una variable de rango que representa un valor calculado por medio de variables de rango anteriores. Cada `where` cláusula es un filtro que excluye los elementos del resultado. Cada `join` cláusula compara las claves especificadas de la secuencia de origen con las claves de otra secuencia, produciendo los pares coincidentes. Cada `orderby` cláusula reordena los elementos según los criterios especificados. El último `select` o `group` cláusula especifica la forma del resultado en cuanto a las variables de rango. Por último, un `into` cláusula se puede usar para "unir" consultas tratando los resultados de una consulta como un generador en una consulta posterior.

### <a name="ambiguities-in-query-expressions"></a>Ambigüedades en expresiones de consulta

Las expresiones de consulta contienen un número de "palabras clave contextuales", es decir, los identificadores que tienen un significado especial en un contexto determinado. En concreto, estos son `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` y `by`. Con el fin de evitar la ambigüedad en las expresiones de consulta causadas por el uso combinado de estos identificadores como palabras clave o nombres simples, estos identificadores se consideran palabras clave al que se producen en cualquier lugar dentro de una expresión de consulta.

Para ello, una expresión de consulta es cualquier expresión que comienza por "`from identifier`"seguido de cualquier token excepto"`;`","`=`"o"`,`".

Para poder usar estas palabras como identificadores dentro de una expresión de consulta, pueden agregarse como prefijo "`@`" ([identificadores](lexical-structure.md#identifiers)).

### <a name="query-expression-translation"></a>Conversión de expresiones de consulta

El lenguaje C# no especifica la semántica de ejecución de expresiones de consulta. En su lugar, las expresiones de consulta se traducen las invocaciones de métodos que se adhieren a la *patrón de expresión de consulta* ([el patrón de expresión de consulta](expressions.md#the-query-expression-pattern)). En concreto, las expresiones de consulta se traducen las invocaciones de métodos denominados `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, y `Cast`. Estos métodos se esperan que tengan firmas determinadas y tipos de resultados, como se describe en [el patrón de expresión de consulta](expressions.md#the-query-expression-pattern). Estos métodos pueden ser métodos de instancia del objeto que se va a consultar o métodos de extensión que son externos al objeto, y que implementan la ejecución real de la consulta.

La traducción de expresiones de consulta a las invocaciones de método es una asignación sintáctica que tiene lugar antes de cualquier tipo de enlace o se ha realizado la resolución de sobrecarga. La traducción se garantiza que sea sintácticamente correcto, pero no se garantiza para generar código de C# semánticamente correcto. Siguiendo la traducción de expresiones de consulta, se procesan las invocaciones de método resultante como invocaciones de método normal y, a su vez, esto puede revelar errores, por ejemplo si no existen los métodos, si los argumentos tienen tipos incorrectos, o si los métodos son genéricos y se produce un error en la inferencia de tipos.

Una expresión de consulta se procesa mediante la aplicación varias veces las traducciones siguientes hasta que ninguna reducciones adicionales. Las traducciones se enumeran en orden de aplicación: cada sección se da por supuesto que las traducciones en las secciones anteriores se han realizado de forma exhaustiva y una vez agotado, una sección de no más adelante se revisará en el procesamiento de la misma expresión de consulta.

No se permite la asignación a variables de rango en expresiones de consulta. Sin embargo se permite una implementación de C# no siempre exigir esta restricción, ya que esto a veces no sea posible con el esquema de traducción sintáctica presentado aquí.

Ciertas traducciones insertan las variables de rango con identificadores transparentes denotados por `*`. Las propiedades especiales de identificadores transparentes se tratan con más detalle en [identificadores transparentes](expressions.md#transparent-identifiers).

#### <a name="select-and-groupby-clauses-with-continuations"></a>Cláusulas SELECT y groupby con continuaciones

Una expresión de consulta con una continuación
```csharp
from ... into x ...
```
se traduce en
```csharp
from x in ( from ... ) ...
```

Las traducciones en las secciones siguientes se suponen que las consultas tienen no `into` continuaciones.

El ejemplo
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
se traduce en
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
la traducción de final de los cuales es
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Tipos de variable de rango explícito

Un `from` cláusula que especifica explícitamente un tipo de variable de rango
```csharp
from T x in e
```
se traduce en
```csharp
from x in ( e ) . Cast < T > ( )
```

Un `join` cláusula que especifica explícitamente un tipo de variable de rango
```
join T x in e on k1 equals k2
```
se traduce en
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

Las traducciones en las secciones siguientes se suponen que las consultas no tienen ningún tipo de variable de rango explícito.

El ejemplo
```csharp
from Customer c in customers
where c.City == "London"
select c
```
se traduce en
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
la traducción de final de los cuales es
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Tipos de variable de rango explícitas son útiles para consultar las colecciones que implementan la no genérica `IEnumerable` interfaz, pero no genérico `IEnumerable<T>` interfaz. En el ejemplo anterior, esto sería el caso si `customers` eran de tipo `ArrayList`.

#### <a name="degenerate-query-expressions"></a>Expresiones de consulta degenerado

Una expresión de consulta del formulario
```csharp
from x in e select x
```
se traduce en
```csharp
( e ) . Select ( x => x )
```

El ejemplo
```csharp
from c in customers
select c
```
se traduce en
```csharp
customers.Select(c => c)
```

Una expresión de consulta degenerado es aquella que selecciona los elementos del origen de forma trivial. Una fase posterior de la traducción quita degeneradas consultas introducidas por otros pasos de traducción al reemplazarlos con su origen. Es importante sin embargo, para asegurarse de que el resultado de una consulta de expresión nunca es el objeto de origen, ya que podría revelar el tipo y la identidad del origen al cliente de la consulta. Por lo tanto, este paso protege degeneradas consultas escritas directamente en el código fuente llamando explícitamente a `Select` en el origen. Es decisión los implementadores de `Select` y otros operadores de consulta para asegurarse de que estos métodos no devuelven nunca el propio objeto de origen.

#### <a name="from-let-where-join-and-orderby-clauses"></a>Desde, permiten, where, cláusulas join y orderby

Una expresión de consulta con un segundo `from` cláusula seguido por un `select` cláusula
```csharp
from x1 in e1
from x2 in e2
select v
```
se traduce en
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

Una expresión de consulta con un segundo `from` cláusula seguido de algo que no sea un `select` cláusula:

```csharp
from x1 in e1
from x2 in e2
...
```
se traduce en
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

Una expresión de consulta con un `let` cláusula
```csharp
from x in e
let y = f
...
```
se traduce en
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

Una expresión de consulta con un `where` cláusula
```csharp
from x in e
where f
...
```
se traduce en
```csharp
from x in ( e ) . Where ( x => f )
...
```

Una expresión de consulta con un `join` cláusula sin un `into` seguido por un `select` cláusula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
se traduce en
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Una expresión de consulta con un `join` cláusula sin un `into` seguido de algo que no sea un `select` cláusula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
se traduce en
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

Una expresión de consulta con un `join` cláusula con una `into` seguido por un `select` cláusula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
se traduce en
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Una expresión de consulta con un `join` cláusula con una `into` seguido de algo que no sea un `select` cláusula
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
se traduce en
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

Una expresión de consulta con un `orderby` cláusula
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
se traduce en
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

Si una ordenación cláusula especifica un `descending` indicador de dirección, una invocación de `OrderByDescending` o `ThenByDescending` se produce en su lugar.

Las traducciones siguientes se suponen que no hay ningún `let`, `where`, `join` o `orderby` cláusulas y no más de una inicial `from` cláusula en cada expresión de consulta.

El ejemplo
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
se traduce en
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

El ejemplo
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
se traduce en
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
la traducción de final de los cuales es
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
donde `x` es un identificador generado por el compilador que en caso contrario, no es visible y accesible.

El ejemplo
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
se traduce en
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
la traducción de final de los cuales es
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
donde `x` es un identificador generado por el compilador que en caso contrario, no es visible y accesible.

El ejemplo
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
se traduce en
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

El ejemplo
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
se traduce en
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
la traducción de final de los cuales es
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
donde `x` y `y` son los identificadores generados por el compilador que de otro modo invisible y no son accesibles.

El ejemplo
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
tiene la traducción final
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>Cláusulas SELECT

Una expresión de consulta del formulario
```csharp
from x in e select v
```
se traduce en
```csharp
( e ) . Select ( x => v )
```
excepto cuando v es el identificador x, es simplemente la traducción
```csharp
( e )
```

Por ejemplo
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
simplemente se traduce en
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>Agrupar cláusulas

Una expresión de consulta del formulario
```csharp
from x in e group v by k
```
se traduce en
```csharp
( e ) . GroupBy ( x => k , x => v )
```
excepto cuando v es el identificador x, que es la traducción
```csharp
( e ) . GroupBy ( x => k )
```

El ejemplo
```csharp
from c in customers
group c.Name by c.Country
```
se traduce en
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>Identificadores transparentes

Ciertas traducciones insertan las variables de rango con ***identificadores transparentes*** denotado por `*`. Identificadores transparentes no son una característica del lenguaje adecuado; Existen sólo como un paso intermedio en el proceso de traducción de la expresión de consulta.

Cuando una traducción de la consulta inyecta un identificador transparente, aún más pasos de traducción propagan el identificador transparente a las funciones anónimas y los inicializadores de objeto anónimo. En esos contextos, identificadores transparentes tienen el siguiente comportamiento:

*  Cuando se produce un identificador transparente como un parámetro en una función anónima, los miembros del tipo anónimo asociado son automáticamente en el ámbito en el cuerpo de la función anónima.
*  Cuando un miembro con un identificador transparente está en el ámbito, los miembros de ese miembro están en ámbito también.
*  Cuando se produce un identificador transparente como un declarador de miembro en un inicializador de objeto anónimo, presenta a un miembro con un identificador transparente.
*  En los pasos de traducción que se ha descrito anteriormente, siempre se introducen los identificadores transparentes junto con los tipos anónimos, con la intención de capturar varias variables de rango como miembros de un solo objeto. Una implementación de C# se puede usar un mecanismo diferente que los tipos anónimos para agrupar varias variables de rango. Los ejemplos de traducción siguientes se supone que se usan tipos anónimos y mostrar los identificadores de transparencia se puede traducir inmediatamente.

El ejemplo
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
se traduce en
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

es más que traducen en
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
que, cuando se borran los identificadores transparentes, es equivalente a
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
donde `x` es un identificador generado por el compilador que en caso contrario, no es visible y accesible.

El ejemplo
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
se traduce en
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
que es más reducción a
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
la traducción de final de los cuales es
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
donde `x`, `y`, y `z` son los identificadores generados por el compilador que de otro modo invisible y no son accesibles.

### <a name="the-query-expression-pattern"></a>El patrón de expresión de consulta

El ***patrón de expresión de consulta*** establece un patrón de métodos de tipos pueden implementar para admitir las expresiones de consulta. Dado que las expresiones de consulta se traducen a las invocaciones de método por medio de una asignación sintáctica, tipos tienen una flexibilidad considerable en cómo implementan el patrón de expresión de consulta. Por ejemplo, los métodos del patrón pueden implementarse como métodos de instancia o como métodos de extensión porque los dos tienen la misma sintaxis de invocación, y pueden solicitar los métodos delegados o árboles de expresión porque se pueden convertibles a ambas funciones anónimas.

La forma recomendada de un tipo genérico `C<T>` que admite el patrón de expresión de consulta se muestra a continuación. Un tipo genérico sirve para ilustrar las relaciones entre los tipos de parámetro y el resultado correctas, pero es posible implementar el patrón para tipos no genéricos también.

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

Los métodos anteriores usan los tipos de delegado genérico `Func<T1,R>` y `Func<T1,T2,R>`, pero éstos podrían igualmente bien usado otros tipos de árbol de expresión o delegado con las mismas relaciones en los tipos de parámetro y resultado.

Tenga en cuenta la relación entre recomendada `C<T>` y `O<T>` lo que garantiza que el `ThenBy` y `ThenByDescending` métodos sólo están disponibles en el resultado de una `OrderBy` o `OrderByDescending`. Tenga en cuenta también la forma recomendada de resultado de `GroupBy` --una secuencia de secuencias, donde cada secuencia interna tiene más `Key` propiedad.

El `System.Linq` espacio de nombres proporciona una implementación del patrón de operador de consulta para cualquier tipo que implementa el `System.Collections.Generic.IEnumerable<T>` interfaz.

## <a name="assignment-operators"></a>Operadores de asignación

Los operadores de asignación asignación un nuevo valor a una variable, una propiedad, un evento o un elemento del indizador.

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

El operando izquierdo de una asignación debe ser una expresión que se clasifica como una variable, un acceso de propiedad, un acceso de indizador o un acceso a evento.

El `=` se denomina operador la ***operador de asignación simple***. El valor del operando derecho asigna al elemento de variable, propiedad o indizador dado por el operando izquierdo. El operando izquierdo del operador de asignación simple no puede ser un acceso a evento (excepto como se describe en [eventos como campos](classes.md#field-like-events)). Se describe el operador de asignación simple en [asignación Simple](expressions.md#simple-assignment).

Los operadores de asignación no sea el `=` operador se denominan el ***operadores de asignación compuesta***. Estos operadores realizan la operación indicada en los dos operandos y, a continuación, asignan el valor resultante para el elemento de variable, propiedad o indizador dado por el operando izquierdo. Se describen los operadores de asignación compuesta en [asignación compuesta](expressions.md#compound-assignment).

El `+=` y `-=` operadores con una expresión de acceso de eventos como el operando izquierdo se denominan el *operadores de asignación de evento*. Ningún otro operador de asignación es válido con acceso de un evento como el operando izquierdo. Los operadores de asignación de eventos se describen en [asignación de eventos](expressions.md#event-assignment).

Los operadores de asignación son asociativos a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda. Por ejemplo, una expresión de formato `a = b = c` se evalúa como `a = (b = c)`.

### <a name="simple-assignment"></a>Asignación simple

El `=` operador se denomina operador de asignación simple.

Si el operando izquierdo de una asignación simple tiene el formato `E.P` o `E[Ei]` donde `E` tiene el tipo de tiempo de compilación `dynamic`, a continuación, se enlaza dinámicamente la asignación ([enlace dinámico](expressions.md#dynamic-binding)). En este caso es el tipo de tiempo de compilación de la expresión de asignación `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución según el tipo de tiempo de ejecución de `E`.

En una asignación simple, el operando derecho debe ser una expresión que es implícitamente convertible al tipo del operando izquierdo. La operación asigna el valor del operando derecho para el elemento de variable, propiedad o indizador dado por el operando izquierdo.

El resultado de una expresión de asignación simple es el valor asignado al operando izquierdo. El resultado tiene el mismo tipo que el operando izquierdo y siempre se clasifica como un valor.

Si el operando izquierdo es un acceso de propiedad o indizador, la propiedad o indizador debe tener un `set` descriptor de acceso. Si esto no es el caso, se produce un error en tiempo de enlace.

El procesamiento en tiempo de ejecución de una asignación simple del formulario `x = y` consta de los pasos siguientes:

*  Si `x` se clasifica como una variable:
   * `x` se evalúa para producir la variable.
   * `y` se evalúa y, si es necesario, puede convertir al tipo de `x` a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).
   * Si la variable proporcionada por `x` es un elemento de matriz de un *reference_type*, se realiza una comprobación de tiempo de ejecución para asegurarse de que el valor calculado para `y` es compatible con la instancia de la matriz de los cuales `x` es un elemento. La comprobación se realiza correctamente si `y` es `null`, o si una conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) existe desde el tipo real de la instancia que hace referencia `y` al tipo de elemento real de la instancia de matriz que contiene `x`. De lo contrario, se produce una excepción `System.ArrayTypeMismatchException`.
   * El valor resultante de la evaluación y la conversión de `y` se almacena en la ubicación proporcionada por la evaluación de `x`.
*  Si `x` se clasifica como un acceso de propiedad o indizador:
   * La expresión de instancia (si `x` no `static`) y la lista de argumentos (si `x` es un acceso de indizador) asociado con `x` se evalúan, y los resultados se usan en la siguiente `set` invocación del descriptor de acceso.
   * `y` se evalúa y, si es necesario, puede convertir al tipo de `x` a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).
   * El `set` descriptor de acceso de `x` se invoca con el valor calculado para `y` como su `value` argumento.

Las reglas de covarianza de matriz ([covarianza de matrices](arrays.md#array-covariance)) permiten que un valor de un tipo de matriz `A[]` sea una referencia a una instancia de un tipo de matriz `B[]`, siempre existe una conversión implícita de referencia de `B` a `A`. Debido a estas reglas, la asignación a un elemento de matriz de un *reference_type* requiere una comprobación en tiempo de ejecución para asegurarse de que el valor asignado es compatible con la instancia de matriz. En el ejemplo
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
la última asignación produce un `System.ArrayTypeMismatchException` debido a una instancia de `ArrayList` no pueden almacenarse en un elemento de un `string[]`.

Cuando una propiedad o indizador declarado en un *struct_type* es el destino de una asignación, la expresión de instancia asociado con la propiedad o indizador debe estar clasificada como una variable. Si la expresión de instancia se clasifica como un valor, se produce un error en tiempo de enlace. Debido [acceso a miembros](expressions.md#member-access), la misma regla también se aplica a los campos.

Dadas las declaraciones:
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
En el ejemplo
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
las asignaciones a `p.X`, `p.Y`, `r.A`, y `r.B` se permiten porque `p` y `r` son variables. Sin embargo, en el ejemplo
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
las asignaciones no son válidas, desde `r.A` y `r.B` no son variables.

### <a name="compound-assignment"></a>Asignación compuesta

Si el operando izquierdo de una asignación compuesta tiene el formato `E.P` o `E[Ei]` donde `E` tiene el tipo de tiempo de compilación `dynamic`, a continuación, se enlaza dinámicamente la asignación ([enlace dinámico](expressions.md#dynamic-binding)). En este caso es el tipo de tiempo de compilación de la expresión de asignación `dynamic`, y la resolución que se describe a continuación llevará a cabo en tiempo de ejecución según el tipo de tiempo de ejecución de `E`.

Una operación del formulario `x op= y` se procesa mediante la aplicación de la resolución de sobrecarga de operador binario ([de resolución de sobrecarga de operador binario](expressions.md#binary-operator-overload-resolution)) como si la operación se ha escrito `x op y`. A continuación,

*  Si el tipo de valor devuelto del operador seleccionado es implícitamente convertible al tipo de `x`, la operación se evalúa como `x = x op y`, salvo que `x` se evalúa solo una vez.
*  En caso contrario, si el operador seleccionado es un operador predefinido, si el tipo de valor devuelto del operador seleccionado es convertible explícitamente al tipo de `x`y si `y` es implícitamente convertible al tipo de `x` o el operador es un operador, de desplazamiento, a continuación, la operación se evalúa como `x = (T)(x op y)`, donde `T` es el tipo de `x`, salvo que `x` se evalúa solo una vez.
*  En caso contrario, la asignación compuesta no es válida y se produce un error en tiempo de enlace.

El término "se evalúa solo una vez" significa que, en la evaluación de `x op y`, los resultados de cualquier expresión constituyentes de `x` se guarda temporalmente y, a continuación, volver a usar al realizar la asignación a `x`. Por ejemplo, en la asignación `A()[B()] += C()`, donde `A` es un método que devuelve `int[]`, y `B` y `C` son métodos que devuelven `int`, se invocan los métodos de una sola vez, en el orden `A`, `B`, `C`.

Cuando el operando izquierdo de una asignación compuesta es un acceso a la propiedad o indizador, la propiedad o indizador debe tener tanto un `get` descriptor de acceso y un `set` descriptor de acceso. Si esto no es el caso, se produce un error en tiempo de enlace.

La segunda regla mencionada permite `x op= y` se evalúen como `x = (T)(x op y)` en ciertos contextos. La regla existe de forma que los operadores predefinidos pueden usarse como operadores compuestos cuando el operando izquierdo es de tipo `sbyte`, `byte`, `short`, `ushort`, o `char`. Incluso cuando ambos argumentos son de uno de esos tipos, los operadores predefinidos producen un resultado de tipo `int`, tal y como se describe en [Promociones numéricas binarias](expressions.md#binary-numeric-promotions). Por lo tanto, sin una conversión no sería posible asignar el resultado al operando izquierdo.

El efecto intuitivo de la regla para los operadores predefinidos es simplemente que `x op= y` está permitido si ambos de `x op y` y `x = y` están permitidas. En el ejemplo
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
el motivo intuitivo de cada error es que una asignación simple correspondiente también habría sido un error.

Esto también significa que admiten operaciones de asignación compuesta eleva las operaciones. En el ejemplo
```csharp
int? i = 0;
i += 1;             // Ok
```
el operador de elevación `+(int?,int?)` se utiliza.

### <a name="event-assignment"></a>Asignación de eventos

Si el operando izquierdo de un `+=` o `-=` operador se clasifica como un acceso de eventos, a continuación, la expresión se evalúa como sigue:

*  La expresión de instancia, si los hay, de acceso a lo eventos se evalúa.
*  El operando derecho de la `+=` o `-=` operador se evalúa y, si es necesario, puede convertir al tipo del operando izquierdo a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).
*  Se invoca un descriptor de acceso de eventos del evento, con la lista de argumentos que se compone del operando derecho, después de la evaluación y, si es necesario, conversión. Si el operador era `+=`, el `add` se invoca el descriptor de acceso; si el operador era `-=`, el `remove` se invoca al descriptor de acceso.

Una expresión de evento de asignación no producen un valor. Por lo tanto, una expresión de asignación de eventos solo es válida en el contexto de un *statement_expression* ([las instrucciones de expresión](statements.md#expression-statements)).

## <a name="expression"></a>Expresión

Un *expresión* sea un *non_assignment_expression* o un *asignación*.

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

## <a name="constant-expressions"></a>Expresiones de constante

Un *constant_expression* es una expresión que se puede evaluar por completo en tiempo de compilación.

```antlr
constant_expression
    : expression
    ;
```

Una expresión constante debe ser el `null` literal o un valor con uno de los siguientes tipos: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, o cualquier tipo de enumeración. Las siguientes construcciones de sólo se permiten en expresiones constantes:

*  Literales (incluido el `null` literal).
*  Las referencias a `const` miembros de tipos de clase y estructura.
*  Referencias a los miembros de tipos de enumeración.
*  Las referencias a `const` parámetros o variables locales
*  Subexpresiones entre paréntesis, que son expresiones constantes.
*  Expresiones de conversión, siempre que el tipo de destino es uno de los tipos enumerados anteriormente.
*  `checked` y `unchecked` expresiones
*  Expresiones de valor predeterminado
*  Expresiones Nameof
*  Predefinido `+`, `-`, `!`, y `~` operadores unarios.
*  Predefinido `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, y `>=` proporcionado de operadores binarios, cada operando es de tipo enumerado anteriormente.
*  El `?:` operador condicional.

Las conversiones siguientes se permiten en expresiones constantes:

*  Conversiones de identidad
*  Conversiones numéricas
*  Conversiones de enumeración
*  Conversiones de expresión constante
*  Conversiones implícitas y explícitas de referencia, siempre que el origen de las conversiones de una expresión constante que se evalúa como el valor null.

Otras conversiones, incluidas la conversión boxing, unboxing e implícita las conversiones de referencia de valores distintos de null no se permiten en expresiones constantes. Por ejemplo:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
la inicialización de i es un error porque se requiere una conversión boxing. La inicialización de str es un error porque se requiere una conversión implícita de referencia de un valor distinto de null.

Cada vez que una expresión que cumple los requisitos descritos anteriormente, la expresión se evalúa en tiempo de compilación. Esto es cierto incluso si la expresión es una subexpresión de una expresión mayor que contiene construcciones no constante.

La evaluación de tiempo de compilación de expresiones constantes utiliza las mismas reglas como tiempo de ejecución de evaluación de expresiones no constantes, salvo que en evaluación en tiempo de ejecución se habría iniciado una excepción, la evaluación de tiempo de compilación produce un error de tiempo de compilación que se produzca.

A menos que explícitamente se coloca una expresión constante en una `unchecked` contexto, los desbordamientos que se producen en las conversiones y operaciones aritméticas de tipo integral durante la evaluación de tiempo de compilación de la expresión siempre provocan errores de tiempo de compilación ([Expresiones constantes](expressions.md#constant-expressions)).

Expresiones constantes que se producen en los contextos siguientes. En estos contextos, se produce un error de tiempo de compilación si una expresión no se puede evaluar por completo en tiempo de compilación.

*  Las declaraciones de constantes ([constantes](classes.md#constants)).
*  Las declaraciones de miembros de enumeración ([miembros de enumeración](enums.md#enum-members)).
*  Argumentos de las listas de parámetros formales predeterminados ([parámetros del método](classes.md#method-parameters))
*  `case` las etiquetas de un `switch` instrucción ([la instrucción switch](statements.md#the-switch-statement)).
*  `goto case` instrucciones ([la instrucción goto](statements.md#the-goto-statement)).
*  Longitudes de dimensión en una expresión de creación de matriz ([expresiones de creación de matriz](expressions.md#array-creation-expressions)) que incluye un inicializador.
*  Atributos ([atributos](attributes.md)).

Una conversión implícita de expresión constante ([conversiones implícitas de expresión constante](conversions.md#implicit-constant-expression-conversions)) permite una expresión constante de tipo `int` va a convertir en `sbyte`, `byte`, `short`, `ushort`, `uint`, o `ulong`, siempre que el valor de la expresión constante esté dentro del intervalo del tipo de destino.

## <a name="boolean-expressions"></a>expresiones booleanas

Un *boolean_expression* es una expresión que da como resultado un tipo `bool`; ya sea directamente o a través de la aplicación de `operator true` en ciertos contextos, como se especifica en la siguiente.

```antlr
boolean_expression
    : expression
    ;
```

La expresión condicional de control de un *if_statement* ([if instrucción](statements.md#the-if-statement)), *while_statement* ([la instrucción while](statements.md#the-while-statement)), *do_statement* ([la instrucción do](statements.md#the-do-statement)), o *for_statement* ([la instrucción for](statements.md#the-for-statement)) es un *boolean_ expresión*. La expresión condicional de control de la `?:` operador ([operador condicional](expressions.md#conditional-operator)) sigue las mismas reglas que un *boolean_expression*, pero por motivos de operador se clasifica prioridad como un *conditional_or_expression*.

Un *boolean_expression* `E` debe ser capaz de generar un valor de tipo `bool`, como sigue:

*  Si `E` es implícitamente convertible a `bool` , a continuación, en tiempo de ejecución se aplica esa conversión implícita.
*  En caso contrario, resolución de sobrecarga de operador unario ([de resolución de sobrecarga de operador unario](expressions.md#unary-operator-overload-resolution)) se utiliza para buscar una única mejor implementación del operador `true` en `E`, y que la implementación se aplica en tiempo de ejecución.
*  Si no se encuentra ningún operador de este tipo, se produce un error en tiempo de enlace.

El `DBBool` tipo struct en [base de datos de tipo booleano](structs.md#database-boolean-type) proporciona un ejemplo de un tipo que implementa `operator true` y `operator false`.
