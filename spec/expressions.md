---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704088"
---
# <a name="expressions"></a>Expresiones

Una expresión es una secuencia de operadores y operandos. En este capítulo se define la sintaxis, el orden de evaluación de los operandos y operadores y el significado de las expresiones.

## <a name="expression-classifications"></a>Clasificaciones de expresiones

Una expresión se clasifica de las siguientes formas:

*  Un valor. Todos los valores tienen un tipo asociado.
*  Una variable. Cada variable tiene un tipo asociado, es decir, el tipo declarado de la variable.
*  Espacio de nombres. Una expresión con esta clasificación solo puede aparecer como la parte izquierda de un *member_access* ([acceso a miembros](expressions.md#member-access)). En cualquier otro contexto, una expresión clasificada como un espacio de nombres produce un error en tiempo de compilación.
*  Un tipo. Una expresión con esta clasificación solo puede aparecer como el lado izquierdo de un *member_access* ([acceso a miembros](expressions.md#member-access)) o como un operando para el operador `as` ([el operador as](expressions.md#the-as-operator)), el operador `is` ([el operador is](expressions.md#the-is-operator)) o el @no operador __t-6 ([operador typeof](expressions.md#the-typeof-operator)). En cualquier otro contexto, una expresión clasificada como un tipo genera un error en tiempo de compilación.
*  Un grupo de métodos, que es un conjunto de métodos sobrecargados que resultan de una búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)). Un grupo de métodos puede tener una expresión de instancia asociada y una lista de argumentos de tipo asociada. Cuando se invoca un método de instancia, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)). Se permite un grupo de métodos en un *invocation_expression* ([expresiones de invocación](expressions.md#invocation-expressions)), un *delegate_creation_expression* (expresiones de[creación de delegado](expressions.md#delegate-creation-expressions)) y como el lado izquierdo de un operador is y puede ser implícitamente se convierte en un tipo de delegado compatible ([conversiones de grupo de métodos](conversions.md#method-group-conversions)). En cualquier otro contexto, una expresión clasificada como grupo de métodos produce un error en tiempo de compilación.
*  Un literal null. Una expresión con esta clasificación se puede convertir implícitamente a un tipo de referencia o a un tipo que acepta valores NULL.
*  Una función anónima. Una expresión con esta clasificación se puede convertir implícitamente en un tipo de delegado compatible o un tipo de árbol de expresión.
*  Un acceso de propiedad. Cada acceso de propiedad tiene un tipo asociado, es decir, el tipo de la propiedad. Además, un acceso de propiedad puede tener una expresión de instancia asociada. Cuando se invoca un descriptor de acceso (el bloque `get` o `set`) de una propiedad de instancia, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)).
*  Un acceso de evento. Cada acceso a eventos tiene un tipo asociado, es decir, el tipo del evento. Además, un acceso a eventos puede tener una expresión de instancia asociada. Un acceso a eventos puede aparecer como el operando izquierdo de los operadores `+=` y `-=` ([asignación de eventos](expressions.md#event-assignment)). En cualquier otro contexto, una expresión clasificada como acceso de evento produce un error en tiempo de compilación.
*  Un acceso de indexador. Cada acceso de indexador tiene un tipo asociado, es decir, el tipo de elemento del indexador. Además, un acceso de indexador tiene una expresión de instancia asociada y una lista de argumentos asociada. Cuando se invoca un descriptor de acceso (el bloque `get` o `set`) de un acceso de indexador, el resultado de evaluar la expresión de instancia se convierte en la instancia representada por `this` ([este acceso](expressions.md#this-access)) y el resultado de la evaluación de la lista de argumentos se convierte en el lista de parámetros de la invocación.
*  Relación. Esto sucede cuando la expresión es una invocación de un método con un tipo de valor devuelto de `void`. Una expresión clasificada como Nothing solo es válida en el contexto de *statement_expression* ([instrucciones de expresión](statements.md#expression-statements)).

El resultado final de una expresión nunca es un espacio de nombres, tipo, grupo de métodos o acceso a eventos. En su lugar, como se indicó anteriormente, estas categorías de expresiones son construcciones intermedias que solo se permiten en determinados contextos.

Un acceso de propiedad o de indexador siempre se reclasifica como un valor realizando una invocación del *descriptor* de acceso get o del *descriptor*de acceso set. El descriptor de acceso concreto viene determinado por el contexto de la propiedad o del indexador: Si el acceso es el destino de una asignación, se invoca al *descriptor* de acceso set para asignar un nuevo valor ([asignación simple](expressions.md#simple-assignment)). De lo contrario, el *descriptor de acceso get* se invoca para obtener el valor actual ([valores de las expresiones](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>Valores de las expresiones

En última instancia, la mayoría de las construcciones que implican una expresión requieren que la expresión denote un ***valor***. En tales casos, si la expresión real denota un espacio de nombres, un tipo, un grupo de métodos o nada, se produce un error en tiempo de compilación. Sin embargo, si la expresión denota un acceso de propiedad, un acceso a indexador o una variable, el valor de la propiedad, el indexador o la variable se sustituyen implícitamente:

*  El valor de una variable es simplemente el valor almacenado actualmente en la ubicación de almacenamiento identificada por la variable. Una variable se debe considerar definitivamente asignada ([asignación definitiva](variables.md#definite-assignment)) antes de que se pueda obtener su valor o, de lo contrario, se producirá un error en tiempo de compilación.
*  El valor de una expresión de acceso de propiedad se obtiene invocando el *descriptor* de acceso get de la propiedad. Si la propiedad no tiene un *descriptor de acceso get*, se produce un error en tiempo de compilación. De lo contrario, se realiza una invocación de miembro de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) y el resultado de la invocación se convierte en el valor de la expresión de acceso de propiedad.
*  El valor de una expresión de acceso de indexador se obtiene al invocar el *descriptor* de acceso get del indexador. Si el indizador no tiene ningún *descriptor de acceso get*, se produce un error en tiempo de compilación. De lo contrario, se realiza una invocación de miembro de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) con la lista de argumentos asociada a la expresión de acceso del indexador y el resultado de la invocación se convierte en el valor del acceso del indexador. Expresiones.

## <a name="static-and-dynamic-binding"></a>Enlace estático y dinámico

El proceso de determinar el significado de una operación basándose en el tipo o valor de las expresiones constituyentes (argumentos, operandos, receptores) a menudo se conoce como ***enlace***. Por ejemplo, el significado de una llamada al método se determina en función del tipo del receptor y de los argumentos. El significado de un operador se determina en función del tipo de sus operandos.

En C# el significado de una operación se suele determinar en tiempo de compilación, en función del tipo en tiempo de compilación de sus expresiones constituyentes. Del mismo modo, si una expresión contiene un error, el compilador detecta el error y lo emite. Este enfoque se conoce como ***enlace estático***.

Sin embargo, si una expresión es una expresión dinámica (es decir, tiene el tipo `dynamic`), esto indica que cualquier enlace en el que participe debería basarse en su tipo en tiempo de ejecución (es decir, el tipo real del objeto que denota en tiempo de ejecución) en lugar de en el tipo en el que se encuentra. tiempo de compilación. Por consiguiente, el enlace de esta operación se aplaza hasta el momento en que se ejecuta la operación durante la ejecución del programa. Esto se conoce como ***enlace dinámico***.

Cuando una operación está enlazada dinámicamente, el compilador realiza poca o ninguna comprobación. En su lugar, si se produce un error en el enlace en tiempo de ejecución, los errores se registran como excepciones en tiempo de ejecución.

Las siguientes operaciones en C# están sujetas al enlace:

*  Acceso a miembros: `e.M`
*  Invocación de método: `e.M(e1, ..., eN)`
*  Invocación de delegado: `e(e1, ..., eN)`
*  Acceso a elementos: `e[e1, ..., eN]`
*  Creación de objetos: `new C(e1, ..., eN)`
*  Operadores unarios sobrecargados: `+`, `-`, @no__t 2, `~`, `++`, `--`, `true`, `false`
*  Operadores binarios sobrecargados: `+`, `-`, @no__t 2, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, 0, 1, 2, 3, 4, 5, 6, 7, 8
*  Operadores de asignación: `=`, `+=`, @no__t 2, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, 0
*  Conversiones implícitas y explícitas

Cuando no hay ninguna expresión dinámica implicada, C# el valor predeterminado es enlace estático, lo que significa que los tipos en tiempo de compilación de las expresiones constituyentes se usan en el proceso de selección. Sin embargo, cuando una de las expresiones constituyentes de las operaciones enumeradas anteriormente es una expresión dinámica, la operación se enlaza dinámicamente.

### <a name="binding-time"></a>Tiempo de enlace

El enlace estático se realiza en tiempo de compilación, mientras que el enlace dinámico tiene lugar en tiempo de ejecución. En las secciones siguientes, el término ***tiempo de enlace*** hace referencia a un tiempo de compilación o a un tiempo de ejecución, en función de cuándo tenga lugar el enlace.

En el ejemplo siguiente se muestran las nociones de enlace estático y dinámico y del tiempo de enlace:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

Las dos primeras llamadas se enlazan estáticamente: la sobrecarga de `Console.WriteLine` se selecciona en función del tipo en tiempo de compilación de su argumento. Por lo tanto, el tiempo de enlace es el tiempo de compilación.

La tercera llamada está enlazada dinámicamente: la sobrecarga de `Console.WriteLine` se selecciona en función del tipo en tiempo de ejecución de su argumento. Esto sucede porque el argumento es una expresión dinámica (su tipo en tiempo de compilación es `dynamic`). Por lo tanto, el tiempo de enlace para la tercera llamada es en tiempo de ejecución.

### <a name="dynamic-binding"></a>Enlace dinámico

El propósito de los enlaces dinámicos es C# permitir que los programas interactúen con ***objetos dinámicos***, es decir, los objetos que no C# siguen las reglas normales del sistema de tipos. Los objetos dinámicos pueden ser objetos de otros lenguajes de programación con distintos sistemas de tipos, o bien pueden ser objetos que se configuran mediante programación para implementar su propia semántica de enlace para diferentes operaciones.

El mecanismo por el que un objeto dinámico implementa su propia semántica es la implementación definida. Una interfaz determinada, que se ha definido de nuevo en la C# implementación, se implementa mediante objetos dinámicos para indicar al tiempo de ejecución que tienen una semántica especial. Por lo tanto, cada vez que las operaciones en un objeto dinámico se enlazan dinámicamente, su propia semántica de C# enlace, en lugar de las de tal como se especifica en este documento, asumen el control.

Aunque el propósito del enlace dinámico es permitir la interoperación con objetos dinámicos C# , permite el enlace dinámico en todos los objetos, tanto si son dinámicos como si no. Esto permite una integración más fluida de los objetos dinámicos, ya que los resultados de las operaciones en ellos pueden no ser objetos dinámicos, pero siguen siendo un tipo desconocido para el programador en tiempo de compilación. Además, el enlace dinámico puede ayudar a eliminar el código basado en la reflexión propenso a errores, incluso cuando ningún objeto implicado sea de objetos dinámicos.

En las secciones siguientes se describe para cada construcción del lenguaje exactamente cuando se aplica el enlace dinámico, qué comprobación del tiempo de compilación se aplica, en caso de que se aplique, y cuál es el resultado en tiempo de compilación y la clasificación de la expresión.

### <a name="types-of-constituent-expressions"></a>Tipos de expresiones constituyentes

Cuando una operación se enlaza estáticamente, el tipo de una expresión constituyente (por ejemplo, un receptor, un argumento, un índice o un operando) siempre se considera el tipo en tiempo de compilación de esa expresión.

Cuando una operación se enlaza de forma dinámica, el tipo de una expresión constituyente se determina de maneras diferentes en función del tipo en tiempo de compilación de la expresión constituyente:

*  Se considera que una expresión constitutiva del tipo en tiempo de compilación `dynamic` tiene el tipo del valor real al que se evalúa la expresión en tiempo de ejecución.
*  Expresión constituyente cuyo tipo en tiempo de compilación es un parámetro de tipo se considera que tiene el tipo al que está enlazado el parámetro de tipo en tiempo de ejecución.
*  De lo contrario, se considera que la expresión Constituyente tiene su tipo en tiempo de compilación.

## <a name="operators"></a>Operadores

Las expresiones se construyen a partir de ***operandos*** y ***operadores***. Los operadores de una expresión indican qué operaciones se aplican a los operandos. Ejemplos de operadores incluyen `+`, `-`, `*`, `/` y `new`. Algunos ejemplos de operandos son literales, campos, variables locales y expresiones.

Hay tres tipos de operadores:

*  Operadores unarios. Los operadores unarios toman un operando y usan la notación de prefijo (como `--x`) o la notación de postfijo (como `x++`).
*  Operadores binarios. Los operadores binarios toman dos operandos y todos usan notación infija (como `x + y`).
*  Operador ternario. Solo existe un operador ternario, `?:`; toma tres operandos y usa la notación de infijo (`c ? x : y`).

El orden de evaluación de los operadores de una expresión viene determinado por la ***prioridad*** y la ***asociatividad*** de los operadores ([precedencia y asociatividad](expressions.md#operator-precedence-and-associativity)de los operadores).

Los operandos de una expresión se evalúan de izquierda a derecha. Por ejemplo, en `F(i) + G(i++) * H(i)`, se llama al método `F` utilizando el valor anterior de `i`; a continuación, se llama al método `G` con el valor anterior de `i`, y, por último, se llama al método `H` con el nuevo valor de `i`. Esto es independiente de la prioridad de operador y no está relacionada con ella.

Algunos operadores se pueden ***sobrecargar***. La sobrecarga de operadores permite especificar implementaciones de operador definidas por el usuario para las operaciones donde uno o ambos operandos son de una clase definida por el usuario o un tipo de estructura ([sobrecarga de operadores](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>Prioridad y asociatividad de los operadores

Cuando una expresión contiene varios operadores, la ***precedencia*** de los operadores controla el orden en que se evalúan los operadores individuales. Por ejemplo, la expresión `x + y * z` se evalúa como `x + (y * z)` porque el operador `*` tiene mayor precedencia que el operador `+` binario. La prioridad de un operador se establece mediante la definición de su producción de gramática asociada. Por ejemplo, un *additive_expression* consta de una secuencia de *multiplicative_expression*s separada por los operadores @no__t 2 o `-`, lo que permite a los operadores @no__t 4 y `-` una menor prioridad que los `*`, `/` y @no__ operadores t-8.

En la tabla siguiente se resumen todos los operadores en orden de prioridad, de mayor a menor:

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
| [Operadores de asignación](expressions.md#assignment-operators), [expresiones de función anónimas](expressions.md#anonymous-function-expressions)  | Asignación y expresión lambda | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

Cuando se produce un operando entre dos operadores con la misma prioridad, la asociatividad de los operadores controla el orden en el que se realizan las operaciones:

*  Excepto en el caso de los operadores de asignación y el operador de uso combinado de NULL, todos los operadores binarios son ***asociativos***a la izquierda, lo que significa que las operaciones se realizan de izquierda a derecha. Por ejemplo, `x + y + z` se evalúa como `(x + y) + z`.
*  Los operadores de asignación, el operador de uso combinado de NULL y el operador condicional (`?:`) son ***asociativos a la derecha***, lo que significa que las operaciones se realizan de derecha a izquierda. Por ejemplo, `x = y = z` se evalúa como `x = (y = z)`.

La precedencia y la asociatividad pueden controlarse mediante paréntesis. Por ejemplo, `x + y * z` primero multiplica `y` por `z` y luego suma el resultado a `x`, pero `(x + y) * z` primero suma `x` y `y` y luego multiplica el resultado por `z`.

### <a name="operator-overloading"></a>Sobrecarga de operadores

Todos los operadores unarios y binarios tienen implementaciones predefinidas que están disponibles automáticamente en cualquier expresión. Además de las implementaciones predefinidas, las implementaciones definidas por el usuario se pueden introducir incluyendo declaraciones `operator` en clases y Structs ([operadores](classes.md#operators)). Las implementaciones de operador definidas por el usuario siempre tienen prioridad sobre las implementaciones de operadores predefinidas: Solo cuando no existan implementaciones de operadores definidos por el usuario aplicables, se tendrán en cuenta las implementaciones de operadores predefinidas, como se describe en resolución de sobrecargas de operador [unario](expressions.md#unary-operator-overload-resolution) y [resolución de sobrecarga de operadores binarios](expressions.md#binary-operator-overload-resolution).

Los ***operadores unarios sobrecargables*** son:
```csharp
+   -   !   ~   ++   --   true   false
```

Aunque `true` y `false` no se usan explícitamente en expresiones (y, por consiguiente, no se incluyen en la tabla de precedencia en la [precedencia y asociatividad](expressions.md#operator-precedence-and-associativity)de los operadores), se consideran operadores porque se invocan en varias expresiones. contextos: Expresiones booleanas ([Expresiones booleanas](expressions.md#boolean-expressions)) y expresiones que implican al operador condicional ([operador condicional](expressions.md#conditional-operator)) y operadores lógicos condicionales ([operadores lógicos condicionales](expressions.md#conditional-logical-operators)).

Los ***operadores binarios sobrecargables*** son:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Solo se pueden sobrecargar los operadores enumerados anteriormente. En concreto, no es posible sobrecargar el acceso a miembros, la invocación de métodos o los `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, 0, 1 y 2.

Cuando se sobrecarga un operador binario, el operador de asignación correspondiente, si lo hay, también se sobrecarga de modo implícito. Por ejemplo, una sobrecarga de Operator `*` también es una sobrecarga de Operator `*=`. Esto se describe con más detalle en [asignación compuesta](expressions.md#compound-assignment). Tenga en cuenta que el operador de asignación (`=`) no se puede sobrecargar. Una asignación siempre realiza una copia simple de bits de un valor en una variable.

Las operaciones de conversión, como `(T)x`, se sobrecargan proporcionando conversiones definidas por el usuario ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)).

El acceso a los elementos, como `a[x]`, no se considera un operador sobrecargable. En su lugar, se admite la indexación definida por el usuario a través de indexadores ([indizadores](classes.md#indexers)).

En las expresiones, se hace referencia a los operadores mediante la notación de operador y, en las declaraciones, se hace referencia a los operadores mediante la notación funcional. En la tabla siguiente se muestra la relación entre el operador y las notaciones funcionales para los operadores unarios y binarios. En la primera entrada, *OP* denota cualquier operador de prefijo unario sobrecargable. En la segunda entrada, *OP* denota los operadores unarios `++` y `--`. En la tercera entrada, *OP* denota cualquier operador binario sobrecargable.


| __Notación de operador__ | __Notación funcional__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Las declaraciones de operador definidas por el usuario siempre requieren que al menos uno de los parámetros sea del tipo de clase o estructura que contiene la declaración de operador. Por lo tanto, no es posible que un operador definido por el usuario tenga la misma firma que un operador predefinido.

Las declaraciones de operador definidas por el usuario no pueden modificar la sintaxis, la precedencia o la asociatividad de un operador. Por ejemplo, el operador `/` siempre es un operador binario, siempre tiene el nivel de prioridad especificado en [precedencia y asociatividad](expressions.md#operator-precedence-and-associativity)de los operadores y siempre es asociativo a la izquierda.

Aunque es posible que un operador definido por el usuario realice cualquier cálculo, se desaconsejan las implementaciones que generan resultados distintos de los que se esperaban de forma intuitiva. Por ejemplo, una implementación de `operator ==` debe comparar los dos operandos para determinar si son iguales y devolver un resultado `bool` adecuado.

Las descripciones de operadores individuales en [expresiones primarias](expressions.md#primary-expressions) a través de [operadores lógicos condicionales](expressions.md#conditional-logical-operators) especifican las implementaciones predefinidas de los operadores y cualquier regla adicional que se aplique a cada operador. En las descripciones se usa la ***resolución de sobrecarga del operador unario***, la ***resolución de sobrecarga del operador binario***y la ***promoción numérica***, que se encuentran en las secciones siguientes.

### <a name="unary-operator-overload-resolution"></a>Resolución de sobrecarga del operador unario

Una operación con el formato `op x` o `x op`, donde `op` es un operador unario sobrecargable y `x` es una expresión de tipo `X`, se procesa como se indica a continuación:

*  El conjunto de operadores candidatos definidos por el usuario que proporciona `X` para la operación `operator op(x)` se determina mediante las reglas de los [operadores candidatos definidos por el usuario](expressions.md#candidate-user-defined-operators).
*  Si el conjunto de operadores definidos por el usuario candidatos no está vacío, se convierte en el conjunto de operadores candidatos para la operación. De lo contrario, las implementaciones unaria predefinidas `operator op`, incluidos sus formatos de elevación, se convierten en el conjunto de operadores candidatos para la operación. Las implementaciones predefinidas de un operador determinado se especifican en la descripción del operador ([expresiones primarias](expressions.md#primary-expressions) y [operadores unarios](expressions.md#unary-operators)).
*  Las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution) se aplican al conjunto de operadores candidatos para seleccionar el mejor operador con respecto a la lista de argumentos `(x)`, y este operador se convierte en el resultado del proceso de resolución de sobrecarga. Si la resolución de sobrecarga no selecciona un solo operador mejor, se produce un error en tiempo de enlace.

### <a name="binary-operator-overload-resolution"></a>Resolución de sobrecarga del operador binario

Una operación con el formato `x op y`, donde `op` es un operador binario sobrecargable, @no__t 2 es una expresión de tipo `X` y `y` es una expresión de tipo `Y`, se procesa de la siguiente manera:

*  Se determina el conjunto de operadores candidatos definidos por el usuario que proporciona `X` y `Y` para la operación `operator op(x,y)`. El conjunto consta de la Unión de los operadores candidatos proporcionados por `X` y los operadores candidatos proporcionados por `Y`, cada uno de los cuales se determina mediante las reglas de los [operadores candidatos definidos](expressions.md#candidate-user-defined-operators)por el usuario. Si `X` y `Y` son del mismo tipo, o si `X` y `Y` se derivan de un tipo base común, los operadores de candidatos compartidos solo se producen en el conjunto combinado una vez.
*  Si el conjunto de operadores definidos por el usuario candidatos no está vacío, se convierte en el conjunto de operadores candidatos para la operación. De lo contrario, las implementaciones `operator op` binarias predefinidas, incluidas sus formas de elevación, se convierten en el conjunto de operadores candidatos para la operación. Las implementaciones predefinidas de un operador determinado se especifican en la descripción del operador ([operadores aritméticos](expressions.md#arithmetic-operators) a través de [operadores lógicos condicionales](expressions.md#conditional-logical-operators)). En el caso de los operadores de enumeración y delegado predefinidos, los únicos operadores considerados son los definidos por un tipo de delegado o de enumeración que es el tipo en tiempo de enlace de uno de los operandos.
*  Las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution) se aplican al conjunto de operadores candidatos para seleccionar el mejor operador con respecto a la lista de argumentos `(x,y)`, y este operador se convierte en el resultado del proceso de resolución de sobrecarga. Si la resolución de sobrecarga no selecciona un solo operador mejor, se produce un error en tiempo de enlace.

### <a name="candidate-user-defined-operators"></a>Operadores candidatos definidos por el usuario

Dado un tipo `T` y una operación `operator op(A)`, donde `op` es un operador sobrecargable y `A` es una lista de argumentos, el conjunto de operadores candidatos definidos por el usuario proporcionados por `T` para `operator op(A)` se determina de la manera siguiente:

*  Determine el tipo `T0`. Si `T` es un tipo que acepta valores NULL, `T0` es su tipo subyacente, de lo contrario `T0` es igual a `T`.
*  Para todas las declaraciones de `operator op` en `T0` y todas las formas de elevación de estos operadores, si al menos un operador es aplicable ([miembro de función aplicable](expressions.md#applicable-function-member)) con respecto a la lista de argumentos `A`, el conjunto de operadores candidatos está formado por todos estos operadores aplicables en `T0`.
*  De lo contrario, si `T0` es `object`, el conjunto de operadores candidatos está vacío.
*  De lo contrario, el conjunto de operadores candidatos proporcionado por `T0` es el conjunto de operadores candidatos que proporciona la clase base directa de `T0`, o la clase base efectiva de `T0` si `T0` es un parámetro de tipo.

### <a name="numeric-promotions"></a>Promociones numéricas

La promoción numérica consiste en realizar automáticamente determinadas conversiones implícitas de los operandos de los operadores binarios y unarios predefinidos. La promoción numérica no es un mecanismo distinto, sino un efecto de aplicar la resolución de sobrecarga a los operadores predefinidos. La promoción numérica específicamente no afecta a la evaluación de operadores definidos por el usuario, aunque se pueden implementar operadores definidos por el usuario para mostrar efectos similares.

Como ejemplo de promoción numérica, tenga en cuenta las implementaciones predefinidas del operador `*` binario:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Cuando se aplican las reglas de resolución de sobrecarga ([resolución de sobrecarga](expressions.md#overload-resolution)) a este conjunto de operadores, el efecto es seleccionar el primero de los operadores para los que existen conversiones implícitas desde los tipos de operando. Por ejemplo, para la operación `b * s`, donde `b` es un @no__t 2 y `s` es una `short`, la resolución de sobrecarga selecciona `operator *(int,int)` como el mejor operador. Por lo tanto, el efecto es que `b` y `s` se convierten a `int` y el tipo del resultado es `int`. Del mismo modo, para la operación `i * d`, donde `i` es una `int` y `d` es una `double`, la resolución de sobrecarga selecciona `operator *(double,double)` como el mejor operador.

#### <a name="unary-numeric-promotions"></a>Promociones numéricas unarias

La promoción numérica unaria se produce para los operandos de los operadores unarios predefinidos `+`, `-` y `~`. La promoción numérica unaria simplemente consiste en convertir operandos de tipo `sbyte`, `byte`, @no__t 2, `ushort` o `char` al tipo `int`. Además, para el operador unario `-`, la promoción numérica unaria convierte los operandos del tipo `uint` al tipo `long`.

#### <a name="binary-numeric-promotions"></a>Promociones numéricas binarias

La promoción numérica binaria se produce para los operandos de los valores predefinidos `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, 0, 1, 2 y 3 operadores binarios. La promoción numérica binaria convierte implícitamente ambos operandos en un tipo común que, en el caso de los operadores no relacionales, también se convierte en el tipo de resultado de la operación. La promoción numérica binaria consiste en aplicar las siguientes reglas, en el orden en que aparecen aquí:

*  Si alguno de los operandos es de tipo `decimal`, el otro operando se convierte al tipo `decimal`, o se produce un error en tiempo de enlace si el otro operando es de tipo `float` o `double`.
*  De lo contrario, si alguno de los operandos es de tipo `double`, el otro operando se convierte al tipo `double`.
*  De lo contrario, si alguno de los operandos es de tipo `float`, el otro operando se convierte al tipo `float`.
*  De lo contrario, si alguno de los operandos es de tipo `ulong`, el otro operando se convierte al tipo `ulong`, o se produce un error en tiempo de enlace si el otro operando es de tipo `sbyte`, `short`, `int` o `long`.
*  De lo contrario, si alguno de los operandos es de tipo `long`, el otro operando se convierte al tipo `long`.
*  De lo contrario, si alguno de los operandos es de tipo `uint` y el otro operando es de tipo `sbyte`, `short` o `int`, ambos operandos se convierten al tipo `long`.
*  De lo contrario, si alguno de los operandos es de tipo `uint`, el otro operando se convierte al tipo `uint`.
*  De lo contrario, ambos operandos se convierten al tipo `int`.

Tenga en cuenta que la primera regla no permite ninguna operación que mezcle el tipo `decimal` con los tipos `double` y `float`. La regla sigue el hecho de que no hay conversiones implícitas entre el tipo `decimal` y los tipos `double` y `float`.

Tenga en cuenta también que no es posible que un operando sea de tipo `ulong` cuando el otro operando es de un tipo entero con signo. La razón es que no existe ningún tipo entero que pueda representar el intervalo completo de `ulong`, así como los tipos enteros con signo.

En los dos casos anteriores, se puede usar una expresión de conversión para convertir explícitamente un operando en un tipo que sea compatible con el otro operando.

En el ejemplo
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
se produce un error en tiempo de enlace porque un `decimal` no se puede multiplicar por `double`. El error se resuelve convirtiendo explícitamente el segundo operando en `decimal`, como se indica a continuación:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Operadores de elevación

Los ***operadores de elevación*** permiten a los operadores predefinidos y definidos por el usuario que operan en tipos de valor que no aceptan valores NULL usarse también con formas que aceptan valores NULL de esos tipos. Los operadores de elevación se construyen a partir de operadores predefinidos y definidos por el usuario que cumplen ciertos requisitos, como se describe a continuación:

*   Para los operadores unarios

    ```csharp
    +  ++  -  --  !  ~
    ```

    existe una forma de elevación de un operador si el operando y los tipos de resultado son tipos de valor que no aceptan valores NULL. La forma de elevación se construye agregando un único modificador `?` a los tipos de operando y de resultado. El operador de elevación genera un valor NULL si el operando es NULL. De lo contrario, el operador de elevación desencapsula el operando, aplica el operador subyacente y ajusta el resultado.

*   Para los operadores binarios

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    existe una forma de elevación de un operador si el operando y los tipos de resultado son todos tipos de valor que no aceptan valores NULL. La forma de elevación se construye agregando un único modificador `?` a cada operando y tipo de resultado. El operador de elevación genera un valor NULL si uno o los dos operandos son NULL (una excepción son los operadores `&` y `|` del tipo `bool?`, como se describe en [operadores lógicos booleanos](expressions.md#boolean-logical-operators)). De lo contrario, el operador de elevación desencapsula los operandos, aplica el operador subyacente y ajusta el resultado.

*   Para los operadores de igualdad

    ```csharp
    ==  !=
    ```

    existe una forma de elevación de un operador si los tipos de operando son tipos de valor que no aceptan valores NULL y si el tipo de resultado es `bool`. La forma de elevación se construye agregando un único modificador `?` a cada tipo de operando. El operador de elevación considera que dos valores NULL son iguales y un valor NULL es distinto de cualquier valor distinto de NULL. Si ambos operandos no son NULL, el operador de elevación desencapsula los operandos y aplica el operador subyacente para generar el resultado `bool`.

*   Para los operadores relacionales

    ```csharp
    <  >  <=  >=
    ```

    existe una forma de elevación de un operador si los tipos de operando son tipos de valor que no aceptan valores NULL y si el tipo de resultado es `bool`. La forma de elevación se construye agregando un único modificador `?` a cada tipo de operando. El operador de elevación produce el valor `false` si uno o los dos operandos son NULL. De lo contrario, el operador de elevación desencapsula los operandos y aplica el operador subyacente para generar el resultado `bool`.

## <a name="member-lookup"></a>Búsqueda de miembros

Una búsqueda de miembros es el proceso por el que se determina el significado de un nombre en el contexto de un tipo. Una búsqueda de miembros se puede producir como parte de la evaluación de *simple_name* ([nombres simples](expressions.md#simple-names)) o de *member_access* ([acceso a miembros](expressions.md#member-access)) en una expresión. Si *simple_name* o *member_access* se produce como *primary_expression* de un *invocation_expression* (invocaciones de[método](expressions.md#method-invocations)), se dice que el miembro se invoca.

Si un miembro es un método o un evento, o si es una constante, un campo o una propiedad de un tipo de delegado ([delegados](delegates.md)) o del tipo `dynamic` ([el tipo dinámico](types.md#the-dynamic-type)), se dice que el miembro es *invocable*.

La búsqueda de miembros considera no solo el nombre de un miembro, sino también el número de parámetros de tipo que tiene el miembro y si el miembro es accesible. En lo que respecta a la búsqueda de miembros, los métodos genéricos y los tipos genéricos anidados tienen el número de parámetros de tipo indicado en sus declaraciones respectivas y todos los demás miembros tienen parámetros de tipo cero.

Una búsqueda de miembro de un nombre @ no__t-0 con parámetros `K` @ no__t-2Type en un tipo @ no__t-3 se procesa de la manera siguiente:

*  En primer lugar, se determina un conjunto de miembros accesibles denominados @ no__t-0:
    * Si `T` es un parámetro de tipo, el conjunto es la Unión de los conjuntos de miembros accesibles denominados @ no__t-1 en cada uno de los tipos especificados como restricción principal o restricción secundaria ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) para @ no__t-3, junto con el conjunto de miembros accesibles denominados @ no__t-4 en `object`.
    * De lo contrario, el conjunto se compone de todos los miembros accesibles ([acceso a miembros](basic-concepts.md#member-access)) denominados @ no__t-1 en @ no__t-2, incluidos los miembros heredados y los miembros accesibles denominados @ no__t-3 en `object`. Si `T` es un tipo construido, el conjunto de miembros se obtiene sustituyendo los argumentos de tipo tal y como se describe en [miembros de tipos construidos](classes.md#members-of-constructed-types). Los miembros que incluyen un modificador `override` se excluyen del conjunto.
*  Después, si `K` es cero, se quitan todos los tipos anidados cuyas declaraciones incluyen parámetros de tipo. Si `K` no es cero, se quitan todos los miembros con un número diferente de parámetros de tipo. Tenga en cuenta que cuando `K` es cero, no se quitan los métodos que tienen parámetros de tipo, ya que el proceso de inferencia de tipos ([inferencia de tipos](expressions.md#type-inference)) podría inferir los argumentos de tipo.
*  Después, si se *invoca*el miembro, todos los miembros que no sean de*invocable* se quitarán del conjunto.
*  Después, los miembros que están ocultos por otros miembros se quitan del conjunto. Para cada miembro `S.M` del conjunto, donde `S` es el tipo en el que se declara el miembro @ no__t-2, se aplican las reglas siguientes:
    * Si `M` es una constante, un campo, una propiedad, un evento o un miembro de enumeración, todos los miembros declarados en un tipo base de `S` se quitan del conjunto.
    * Si `M` es una declaración de tipos, todos los tipos no que se declaran en un tipo base de `S` se quitan del conjunto y todas las declaraciones de tipos con el mismo número de parámetros de tipo que `M` declarados en un tipo base de `S` se quitan del conjunto.
    * Si `M` es un método, todos los miembros que no son de método declarados en un tipo base de `S` se quitan del conjunto.
*  Después, los miembros de interfaz que están ocultos por miembros de clase se quitan del conjunto. Este paso solo tiene efecto si `T` es un parámetro de tipo y `T` tiene una clase base efectiva distinta de `object` y un conjunto de interfaces efectivo no vacío ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)). Para cada miembro `S.M` del conjunto, donde `S` es el tipo en el que se declara el miembro `M`, se aplican las reglas siguientes si `S` es una declaración de clase distinta de `object`:
    * Si `M` es una constante, un campo, una propiedad, un evento, un miembro de enumeración o una declaración de tipos, todos los miembros declarados en una declaración de interfaz se quitan del conjunto.
    * Si `M` es un método, todos los miembros que no sean de método declarados en una declaración de interfaz se quitan del conjunto, y todos los métodos con la misma signatura que `M` declarados en una declaración de interfaz se quitan del conjunto.
*  Por último, si se han quitado los miembros ocultos, se determina el resultado de la búsqueda:
    * Si el conjunto está formado por un único miembro que no es un método, este miembro es el resultado de la búsqueda.
    * De lo contrario, si el conjunto solo contiene métodos, este grupo de métodos es el resultado de la búsqueda.
    * De lo contrario, la búsqueda es ambigua y se produce un error en tiempo de enlace.

Para búsquedas de miembros en tipos que no sean de tipo e interfaces, y búsquedas de miembros en interfaces que son estrictamente de herencia única (cada interfaz de la cadena de herencia tiene exactamente cero o una interfaz base directa), el efecto de las reglas de búsqueda es simplemente los miembros derivados ocultan los miembros base con el mismo nombre o signatura. Dichas búsquedas de herencia única nunca son ambiguas. Las ambigüedades que pueden surgir en las búsquedas de miembros en interfaces de herencia múltiple se describen en [acceso a miembros de interfaz](interfaces.md#interface-member-access).

### <a name="base-types"></a>Tipos base

En lo que respecta a la búsqueda de miembros, se considera que un tipo `T` tiene los siguientes tipos base:

*  Si `T` es `object`, `T` no tiene tipo base.
*  Si `T` es un *enum_type*, los tipos base de `T` son los tipos de clase `System.Enum`, `System.ValueType` y `object`.
*  Si `T` es un *struct_type*, los tipos base de `T` son los tipos de clase `System.ValueType` y `object`.
*  Si `T` es un *class_type*, los tipos base de `T` son las clases base de `T`, incluido el tipo de clase `object`.
*  Si `T` es un *interface_type*, los tipos base de `T` son las interfaces base de `T` y el tipo de clase `object`.
*  Si `T` es un *array_type*, los tipos base de `T` son los tipos de clase `System.Array` y `object`.
*  Si `T` es un *delegate_type*, los tipos base de `T` son los tipos de clase `System.Delegate` y `object`.

## <a name="function-members"></a>Miembros de función

Los miembros de función son miembros que contienen instrucciones ejecutables. Los miembros de función siempre son miembros de tipos y no pueden ser miembros de espacios de nombres. C#define las siguientes categorías de miembros de función:

*  Métodos
*  Propiedades
*  Events
*  Indizadores
*  Operadores definidos por el usuario
*  Constructores de instancias
*  Constructores estáticos
*  Destructores

A excepción de los destructores y los constructores estáticos (que no se pueden invocar explícitamente), las instrucciones contenidas en miembros de función se ejecutan a través de las invocaciones de miembros de función. La sintaxis real para escribir una invocación de miembros de función depende de la categoría de miembro de función determinada.

La lista de argumentos ([listas de argumentos](expressions.md#argument-lists)) de una invocación de miembros de función proporciona valores reales o referencias de variable para los parámetros del miembro de función.

Las invocaciones de métodos genéricos pueden emplear la inferencia de tipos para determinar el conjunto de argumentos de tipo que se van a pasar al método. Este proceso se describe en [inferencia de tipos](expressions.md#type-inference).

Las invocaciones de métodos, indizadores, operadores y constructores de instancia emplean la resolución de sobrecarga para determinar cuál de un conjunto candidato de miembros de función se va a invocar. Este proceso se describe en [resolución de sobrecarga](expressions.md#overload-resolution).

Una vez que se ha identificado un miembro de función determinado en tiempo de enlace, posiblemente a través de la resolución de sobrecarga, el proceso real en tiempo de ejecución de la invocación del miembro de función se describe en la [comprobación en tiempo de compilación de la resolución de sobrecarga dinámica](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

En la tabla siguiente se resume el procesamiento que tiene lugar en construcciones que implican las seis categorías de miembros de función que se pueden invocar explícitamente. En la tabla, `e`, `x`, `y` y `value` indican expresiones clasificadas como variables o valores, `T` indica una expresión clasificada como un tipo, `F` es el nombre simple de un método y `P` es el nombre simple de una propiedad.


| __Construir__     | __Ejemplo__    | __Descripción__ |
|-------------------|----------------|-----------------|
| Invocación del método. | `F(x,y)`       | Se aplica la resolución de sobrecarga para seleccionar el mejor método `F` en la clase contenedora o struct. El método se invoca con la lista de argumentos `(x,y)`. Si el método no es `static`, la expresión de instancia es `this`. | 
|                   | `T.F(x,y)`     | Se aplica la resolución de sobrecarga para seleccionar el mejor método `F` en la clase o struct `T`. Se produce un error en tiempo de enlace si el método no es `static`. El método se invoca con la lista de argumentos `(x,y)`. | 
|                   | `e.F(x,y)`     | La resolución de sobrecarga se aplica para seleccionar el mejor método F en la clase, estructura o interfaz dada por el tipo de `e`. Se produce un error en tiempo de enlace si el método es `static`. El método se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y)`. | 
| Property Access   | `P`            | Se invoca al descriptor de acceso `get` de la propiedad `P` en la clase o estructura contenedora. Se produce un error en tiempo de compilación si `P` es de solo escritura. Si `P` no es `static`, la expresión de instancia es @no__t 2. | 
|                   | `P = value`    | El descriptor de acceso `set` de la propiedad `P` en la clase o estructura contenedora se invoca con la lista de argumentos `(value)`. Se produce un error en tiempo de compilación si `P` es de solo lectura. Si `P` no es `static`, la expresión de instancia es @no__t 2. | 
|                   | `T.P`          | Se invoca al descriptor de acceso `get` de la propiedad `P` en la clase o struct `T`. Se produce un error en tiempo de compilación si `P` no es `static` o si `P` es de solo escritura. | 
|                   | `T.P = value`  | El descriptor de acceso `set` de la propiedad `P` de la clase o estructura `T` se invoca con la lista de argumentos `(value)`. Se produce un error en tiempo de compilación si `P` no es `static` o si `P` es de solo lectura. | 
|                   | `e.P`          | El descriptor de acceso `get` de la propiedad `P` en la clase, estructura o interfaz proporcionada por el tipo de `e` se invoca con la expresión de instancia `e`. Se produce un error en tiempo de enlace si `P` es `static` o si `P` es de solo escritura. | 
|                   | `e.P = value`  | El descriptor de acceso `set` de la propiedad `P` en la clase, estructura o interfaz proporcionada por el tipo de `e` se invoca con la expresión de instancia `e` y la lista de argumentos `(value)`. Se produce un error en tiempo de enlace si `P` es `static` o si `P` es de solo lectura. | 
| Acceso a eventos      | `E += value`   | Se invoca al descriptor de acceso `add` del evento `E` en la clase o estructura contenedora. Si `E` no es static, la expresión de instancia es `this`. | 
|                   | `E -= value`   | Se invoca al descriptor de acceso `remove` del evento `E` en la clase o estructura contenedora. Si `E` no es static, la expresión de instancia es `this`. | 
|                   | `T.E += value` | Se invoca al descriptor de acceso `add` del evento `E` en la clase o struct `T`. Se produce un error en tiempo de enlace si `E` no es estático. | 
|                   | `T.E -= value` | Se invoca al descriptor de acceso `remove` del evento `E` en la clase o struct `T`. Se produce un error en tiempo de enlace si `E` no es estático. | 
|                   | `e.E += value` | El descriptor de acceso `add` del evento `E` en la clase, estructura o interfaz proporcionada por el tipo de `e` se invoca con la expresión de instancia `e`. Se produce un error en tiempo de enlace si `E` es estático. | 
|                   | `e.E -= value` | El descriptor de acceso `remove` del evento `E` en la clase, estructura o interfaz proporcionada por el tipo de `e` se invoca con la expresión de instancia `e`. Se produce un error en tiempo de enlace si `E` es estático. | 
| Acceso a indizador    | `e[x,y]`       | La resolución de sobrecarga se aplica para seleccionar el mejor indexador en la clase, estructura o interfaz dada por el tipo de e. El descriptor de acceso `get` del indexador se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y)`. Se produce un error en tiempo de enlace si el indizador es de solo escritura. | 
|                   | `e[x,y] = value` | La resolución de sobrecarga se aplica para seleccionar el mejor indexador en la clase, estructura o interfaz que proporciona el tipo de `e`. El descriptor de acceso `set` del indexador se invoca con la expresión de instancia `e` y la lista de argumentos `(x,y,value)`. Se produce un error en tiempo de enlace si el indizador es de solo lectura. | 
| Invocación de operador | `-x`         | La resolución de sobrecarga se aplica para seleccionar el mejor operador unario en la clase o el struct proporcionado por el tipo de `x`. El operador seleccionado se invoca con la lista de argumentos `(x)`. | 
|                     | `x + y`      | La resolución de sobrecarga se aplica para seleccionar el mejor operador binario en las clases o Structs que proporcionan los tipos de `x` y `y`. El operador seleccionado se invoca con la lista de argumentos `(x,y)`. | 
| Invocación del constructor de instancia | `new T(x,y)` | La resolución de sobrecarga se aplica para seleccionar el mejor constructor de instancia en la clase o struct `T`. Se invoca el constructor de instancia con la lista de argumentos `(x,y)`. | 

### <a name="argument-lists"></a>Listas de argumentos

Cada invocación de miembro de función y delegado incluye una lista de argumentos que proporciona valores reales o referencias de variable para los parámetros del miembro de función. La sintaxis para especificar la lista de argumentos de una invocación de miembros de función depende de la categoría de miembros de función:

*  En el caso de constructores de instancia, métodos, indexadores y delegados, los argumentos se especifican como *argument_list*, como se describe a continuación. En el caso de los indizadores, al invocar el descriptor de acceso `set`, la lista de argumentos incluye también la expresión especificada como operando derecho del operador de asignación.
*  En el caso de las propiedades, la lista de argumentos está vacía al invocar el descriptor de acceso `get` y se compone de la expresión especificada como operando derecho del operador de asignación al invocar el descriptor de acceso `set`.
*  En el caso de los eventos, la lista de argumentos se compone de la expresión especificada como operando derecho del operador `+=` o `-=`.
*  En el caso de los operadores definidos por el usuario, la lista de argumentos se compone del operando único del operador unario o de los dos operandos del operador binario.

Los argumentos de las propiedades ([propiedades](classes.md#properties)), los eventos ([eventos](classes.md#events)) y los operadores definidos por el usuario ([operadores](classes.md#operators)) siempre se pasan como parámetros de valor ([parámetros de valor](classes.md#value-parameters)). Los argumentos de los indizadores ([indizadores](classes.md#indexers)) siempre se pasan como parámetros de valor ([parámetros de valor](classes.md#value-parameters)) o como matrices de parámetros (matrices de[parámetros](classes.md#parameter-arrays)). Los parámetros de referencia y de salida no se admiten para estas categorías de miembros de función.

Los argumentos de una invocación de constructor de instancia, método, indexador o delegado se especifican como *argument_list*:

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

Un *argument_list* consta de uno o más *argumentos*, separados por comas. Cada argumento consta de un *argument_name* opcional seguido de un *argument_value*. Un *argumento* con un *argument_name* se conoce como argumento con ***nombre***, mientras que un *argumento* sin *argument_name* es un ***argumento posicional***. Es un error que un argumento posicional aparezca después de un argumento con nombre en un *argument_list*.

*Argument_value* puede adoptar uno de los siguientes formatos:

*  Una *expresión*, que indica que el argumento se pasa como parámetro de valor ([parámetros de valor](classes.md#value-parameters)).
*  La palabra clave `ref` seguida de una *variable_reference* ([referencias de variable](variables.md#variable-references)), que indica que el argumento se pasa como parámetro de referencia (parámetros de[referencia](classes.md#reference-parameters)). Una variable debe estar asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) antes de que se pueda pasar como un parámetro de referencia. La palabra clave `out` seguida de una *variable_reference* ([referencias de variable](variables.md#variable-references)), que indica que el argumento se pasa como parámetro de salida (parámetros de[salida](classes.md#output-parameters)). Una variable se considera asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) después de una invocación de miembro de función en la que se pasa la variable como parámetro de salida.

#### <a name="corresponding-parameters"></a>Parámetros correspondientes

Para cada argumento de una lista de argumentos debe haber un parámetro correspondiente en el miembro de función o el delegado que se va a invocar.

La lista de parámetros que se utiliza en lo siguiente se determina de la siguiente manera:

*  En el caso de los métodos virtuales y los indizadores definidos en las clases, la lista de parámetros se elige de la declaración o invalidación más específica del miembro de función, empezando por el tipo estático del receptor y buscando en sus clases base.
*  En el caso de los métodos e indexadores de la interfaz, la lista de parámetros se selecciona como la definición más específica del miembro, empezando por el tipo de interfaz y buscando en las interfaces base. Si no se encuentra ninguna lista de parámetros única, se construye una lista de parámetros con nombres inaccesibles y ningún parámetro opcional, de modo que las invocaciones no pueden usar parámetros con nombre u omitir argumentos opcionales.
*  Para los métodos parciales, se usa la lista de parámetros de la declaración de método parcial de definición.
*  En el caso de todos los demás miembros de función y delegados, solo hay una lista de parámetros única, que es la que se usa.

La posición de un argumento o parámetro se define como el número de argumentos o parámetros que lo preceden en la lista de argumentos o en la lista de parámetros.

Los parámetros correspondientes para los argumentos de miembro de función se establecen de la siguiente manera:

*  Argumentos de *argument_list* de constructores de instancia, métodos, indizadores y delegados:
    * Un argumento posicional en el que se produce un parámetro fijo en la misma posición en la lista de parámetros corresponde a ese parámetro.
    * Un argumento posicional de un miembro de función con una matriz de parámetros invocada en su forma normal corresponde a la matriz de parámetros, que debe aparecer en la misma posición en la lista de parámetros.
    * Argumento posicional de un miembro de función con una matriz de parámetros invocada en su forma expandida, donde no se produce ningún parámetro fijo en la misma posición en la lista de parámetros, corresponde a un elemento de la matriz de parámetros.
    * Un argumento con nombre corresponde al parámetro con el mismo nombre en la lista de parámetros.
    * En el caso de los indizadores, al invocar el descriptor de acceso `set`, la expresión especificada como operando derecho del operador de asignación corresponde al parámetro `value` implícito de la declaración del descriptor de acceso `set`.
*  En el caso de las propiedades, al invocar el descriptor de acceso `get` no hay ningún argumento. Al invocar el descriptor de acceso `set`, la expresión especificada como operando derecho del operador de asignación corresponde al parámetro `value` implícito de la declaración del descriptor de acceso `set`.
*  En el caso de los operadores unarios definidos por el usuario (incluidas las conversiones), el único operando corresponde al parámetro único de la declaración del operador.
*  En el caso de los operadores binarios definidos por el usuario, el operando izquierdo corresponde al primer parámetro y el operando derecho se corresponde con el segundo parámetro de la declaración del operador.

#### <a name="run-time-evaluation-of-argument-lists"></a>Evaluación en tiempo de ejecución de listas de argumentos

Durante el procesamiento en tiempo de ejecución de una invocación de miembro de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), las expresiones o referencias de variables de una lista de argumentos se evalúan en orden, de izquierda a derecha, de la manera siguiente:

*  Para un parámetro de valor, se evalúa la expresión de argumento y se realiza una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de parámetro correspondiente. El valor resultante se convierte en el valor inicial del parámetro de valor en la invocación del miembro de función.
*  Para un parámetro de referencia o de salida, se evalúa la referencia de la variable y la ubicación de almacenamiento resultante se convierte en la ubicación de almacenamiento representada por el parámetro en la invocación del miembro de función. Si la referencia de variable dada como parámetro de referencia o de salida es un elemento de matriz de un *reference_type*, se realiza una comprobación en tiempo de ejecución para asegurarse de que el tipo de elemento de la matriz es idéntico al tipo del parámetro. Si se produce un error en esta comprobación, se produce una `System.ArrayTypeMismatchException`.

Los métodos, indizadores y constructores de instancias pueden declarar su parámetro situado más a la derecha para que sea una matriz de parámetros ([matrices de parámetros](classes.md#parameter-arrays)). Estos miembros de función se invocan en su forma normal o en su forma expandida, en función de cuál sea aplicable ([miembro de función aplicable](expressions.md#applicable-function-member)):

*  Cuando se invoca un miembro de función con una matriz de parámetros en su forma normal, el argumento dado para la matriz de parámetros debe ser una expresión única que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) al tipo de matriz de parámetros. En este caso, la matriz de parámetros actúa exactamente como un parámetro de valor.
*  Cuando un miembro de función con una matriz de parámetros se invoca en su forma expandida, la invocación debe especificar cero o más argumentos posicionales para la matriz de parámetros, donde cada argumento es una expresión que se pueda convertir implícitamente ([conversiones implícitas ](conversions.md#implicit-conversions)) al tipo de elemento de la matriz de parámetros. En este caso, la invocación crea una instancia del tipo de matriz de parámetros con una longitud que corresponde al número de argumentos, inicializa los elementos de la instancia de la matriz con los valores de argumento especificados y usa la instancia de matriz recién creada como el actual. argument.

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
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

Las reglas de covarianza de matriz ([covarianza de matriz](arrays.md#array-covariance)) permiten que un valor de un tipo de matriz `A[]` sea una referencia a una instancia de un tipo de matriz `B[]`, siempre que exista una conversión de referencia implícita de `B` a `A`. Debido a estas reglas, cuando se pasa un elemento de matriz de un *reference_type* como parámetro de referencia o de salida, se requiere una comprobación en tiempo de ejecución para asegurarse de que el tipo de elemento real de la matriz es idéntico al del parámetro. En el ejemplo
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
la segunda invocación de `F` hace que se inicie una `System.ArrayTypeMismatchException` porque el tipo de elemento real de `b` es `string` y no `object`.

Cuando se invoca un miembro de función con una matriz de parámetros en su forma expandida, la invocación se procesa exactamente como si se hubiera insertado una expresión de creación de matriz con un inicializador de matriz ([expresiones de creación de matriz](expressions.md#array-creation-expressions)) alrededor de los parámetros expandidos. Por ejemplo, dada la declaración
```csharp
void F(int x, int y, params object[] args);
```
las siguientes invocaciones de la forma expandida del método
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
se corresponde exactamente con
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

En concreto, tenga en cuenta que se crea una matriz vacía cuando no hay ningún argumento dado para la matriz de parámetros.

Cuando se omiten los argumentos de un miembro de función con los parámetros opcionales correspondientes, se pasan implícitamente los argumentos predeterminados de la declaración de miembro de función. Dado que siempre son constantes, su evaluación no afectará al orden de evaluación de los argumentos restantes.

### <a name="type-inference"></a>Inferencia de tipos

Cuando se llama a un método genérico sin especificar argumentos de tipo, un proceso de ***inferencia de tipos*** intenta deducir los argumentos de tipo de la llamada. La presencia de la inferencia de tipos permite usar una sintaxis más cómoda para llamar a un método genérico y permite al programador evitar especificar información de tipos redundantes. Por ejemplo, dada la declaración del método:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
es posible invocar el método `Choose` sin especificar explícitamente un argumento de tipo:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

A través de la inferencia de tipos, los argumentos de tipo `int` y `string` se determinan a partir de los argumentos del método.

La inferencia de tipos se produce como parte del procesamiento en tiempo de enlace de una invocación de método ([invocaciones de método](expressions.md#method-invocations)) y tiene lugar antes del paso de resolución de sobrecarga de la invocación. Cuando se especifica un grupo de métodos determinado en una invocación de método y no se especifica ningún argumento de tipo como parte de la invocación del método, se aplica la inferencia de tipos a cada método genérico del grupo de métodos. Si la inferencia de tipos se realiza correctamente, los argumentos de tipo deducido se usan para determinar los tipos de argumentos para la resolución de sobrecarga subsiguiente. Si la resolución de sobrecarga elige un método genérico como el que se va a invocar, los argumentos de tipo deducido se usan como argumentos de tipo reales para la invocación. Si se produce un error en la inferencia de tipos para un método determinado, ese método no participa en la resolución de sobrecarga. El error de inferencia de tipos, en y de sí mismo, no produce un error en tiempo de enlace. Sin embargo, a menudo se produce un error en tiempo de enlace cuando la resolución de sobrecarga no encuentra ningún método aplicable.

Si el número de argumentos proporcionado es diferente del número de parámetros del método, se producirá un error inmediatamente en la inferencia. En caso contrario, supongamos que el método genérico tiene la siguiente firma:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Con una llamada al método con el formato `M(E1...Em)`, la tarea de inferencia de tipos es buscar argumentos de tipo únicos `S1...Sn` para cada uno de los parámetros de tipo `X1...Xn` para que la llamada `M<S1...Sn>(E1...Em)` sea válida.

Durante el proceso de inferencia, cada parámetro de tipo `Xi` se *fija* en un tipo determinado `Si` o sin *corregir* con un conjunto de *límites*asociado. Cada uno de los límites es algún tipo `T`. Inicialmente, cada variable de tipo `Xi` se desfija con un conjunto vacío de límites.

La inferencia de tipos tiene lugar en fases. Cada fase intentará deducir los argumentos de tipo para obtener más variables de tipo basadas en los resultados de la fase anterior. La primera fase realiza algunas inferencias iniciales de límites, mientras que en la segunda fase se corrigen las variables de tipo a tipos específicos y se deducen más límites. Es posible que la segunda fase se repita varias veces.

*Nota:* La inferencia de tipos no solo tiene lugar cuando se llama a un método genérico. La inferencia de tipos para la conversión de grupos de métodos se describe en [inferencia de tipos para la conversión de grupos de métodos](expressions.md#type-inference-for-conversion-of-method-groups) y encontrar el mejor tipo común de un conjunto de expresiones se describe en [Buscar el mejor tipo común de un conjunto de expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).

#### <a name="the-first-phase"></a>La primera fase

Para cada uno de los argumentos de método `Ei`:

*   Si `Ei` es una función anónima, se realiza una *inferencia de tipo de parámetro explícita* ([inferencias de tipos de parámetros explícitos](expressions.md#explicit-parameter-type-inferences)) de `Ei` a `Ti`.
*   De lo contrario, si `Ei` tiene un tipo `U` y `xi` es un parámetro de valor, se establece una *inferencia de límite inferior* *entre* @no__t *-5 y* `Ti`.
*   De lo contrario, si `Ei` tiene un tipo `U` y `xi` es un parámetro `ref` o `out`, se realiza una *inferencia exacta* *de* `U` *a* `Ti`.
*   De lo contrario, no se realiza ninguna inferencia para este argumento.


#### <a name="the-second-phase"></a>La segunda fase

La segunda fase continúa como sigue:

*   Todas las variables de tipo sin *corregir* `Xi` que no *dependen de* ([dependencia](expressions.md#dependence)) cualquier `Xj` son fijas ([corrigiendo](expressions.md#fixing)).
*   Si no existen estas variables de tipo, todas las variables de tipo sin *corregir* `Xi` son *fijas* para las que se mantienen:
    *   Hay al menos una variable de tipo `Xj` que depende de `Xi`
    *   `Xi` tiene un conjunto de límites no vacío
*   Si no existe ninguna variable de tipo y hay variables de tipo sin *corregir* , se produce un error en la inferencia de tipos.
*   De lo contrario, si no existen más variables de tipo sin *corregir* , la inferencia de tipos se realiza correctamente.
*   De lo contrario, para todos los argumentos `Ei` con el tipo de parámetro correspondiente `Ti`, donde los *tipos de salida* ([tipos de salida](expressions.md#output-types)) contienen variables de tipo no *fijas* `Xj` pero los *tipos de entrada* ([tipos de entrada](expressions.md#input-types)) no,  *la inferencia de tipo de salida* ([inferencias de tipo de salida](expressions.md#output-type-inferences)) se realiza *de* 1 *a* 3. A continuación, se repite la segunda fase.

#### <a name="input-types"></a>Tipos de entrada

Si `E` es un grupo de métodos o una función anónima con tipo implícito y `T` es un tipo de delegado o un tipo de árbol de expresión, todos los tipos de parámetro de `T` son *tipos de entrada* de `E` *con el tipo* `T`.

####  <a name="output-types"></a>Tipos de salida

Si `E` es un grupo de métodos o una función anónima y `T` es un tipo de delegado o un tipo de árbol de expresión, el tipo de valor devuelto de `T` es un *tipo de salida de* `E` *con el tipo* `T`.

#### <a name="dependence"></a>Dependencia

Una variable de tipo sin *corregir* `Xi` *depende directamente de* una variable de tipo sin Fixed `Xj` si para algún argumento `Ek` con el tipo `Tk` `Xj` se produce en un *tipo de entrada* de `Ek` con el tipo `Tk` y 0 se produce en una  *tipo de salida* de 2 con el tipo 3.

`Xj` *depende de* `Xi` si `Xj` *depende directamente de* `Xi` o si `Xi` *depende directamente de* `Xk` y `Xk` *depende de* 1. Por lo tanto, "depende de" es el cierre transitivo pero no reflexivo de "depende directamente de".

#### <a name="output-type-inferences"></a>Inferencias de tipos de salida

Una *inferencia de tipo de salida* se realiza *desde* una expresión `E` *a* un tipo `T` de la siguiente manera:

*  Si `E` es una función anónima con el tipo de valor devuelto inferido `U` ([tipo de valor devuelto deducido](expressions.md#inferred-return-type)) y `T` es un tipo de delegado o un tipo de árbol de expresión con el tipo de valor devuelto `Tb`, una *inferencia de límite inferior* ([inferencias de límite inferior](expressions.md#lower-bound-inferences)) se realiza *desde* `U` *a* 0.
*  De lo contrario, si `E` es un grupo de métodos y `T` es un tipo de delegado o un tipo de árbol de expresión con tipos de parámetro `T1...Tk` y el tipo de valor devuelto `Tb`, y la resolución de sobrecarga de `E` con los tipos `T1...Tk` produce un único método con el tipo de valor devuelto `U` , una *inferencia de límite inferior* se realiza *de* `U` *a* 1.
*  De lo contrario, si `E` es una expresión con el tipo `U`, se realiza una *inferencia de enlace inferior* *entre* `U` *y* `T`.
*  De lo contrario, no se realiza ninguna inferencia.

#### <a name="explicit-parameter-type-inferences"></a>Inferencias explícitas de tipos de parámetros

Una *inferencia de tipo de parámetro explícita* se realiza a *partir de* una expresión `E` *a* un tipo `T` de la siguiente manera:

*  Si `E` es una función anónima con tipo explícito con tipos de parámetro `U1...Uk` y `T` es un tipo de delegado o un tipo de árbol de expresión con tipos de parámetro `V1...Vk`, para cada `Ui` se realiza una *inferencia exacta* ([inferencias exactas](expressions.md#exact-inferences)) *desde* `Ui`  *hasta* el `Vi` correspondiente

#### <a name="exact-inferences"></a>Inferencias exactas

Una *inferencia exacta* *de* un tipo `U` *a* un tipo `V` se realiza de la manera siguiente:

*  Si `V` es uno de los @no__t *fijos* -2, se agrega `U` al conjunto de límites exactos para `Xi`.

*  De lo contrario, establece `V1...Vk` y `U1...Uk` se determinan comprobando si se aplica cualquiera de los casos siguientes:

   *  `V` es un tipo de matriz `V1[...]` y `U` es un tipo de matriz `U1[...]` del mismo rango
   *  `V` es el tipo `V1?` y `U` es el tipo `U1?`
   *  `V` es un tipo construido `C<V1...Vk>`and `U` es un tipo construido `C<U1...Uk>`

   Si se aplica cualquiera de estos casos, se realiza una *inferencia exacta* *desde* cada `Ui` *hasta* el `Vi` correspondiente.

*  En caso contrario, no se realiza ninguna inferencia.

#### <a name="lower-bound-inferences"></a>Inferencias con límite inferior

Una *inferencia de límite inferior* *desde* un tipo `U` *a* un tipo `V` se realiza de la manera siguiente:

*  Si `V` es uno de los @no__t *fijos* -2, `U` se agrega al conjunto de límites inferiores para `Xi`.
*  De lo contrario, si `V` es el tipo `V1?`and `U` es el tipo `U1?`, se realiza una inferencia de límite inferior entre `U1` y `V1`.
*  De lo contrario, establece `U1...Uk` y `V1...Vk` se determinan comprobando si se aplica cualquiera de los casos siguientes:
   *  `V` es un tipo de matriz `V1[...]` y `U` es un tipo de matriz `U1[...]` (o un parámetro de tipo cuyo tipo base efectivo es `U1[...]`) del mismo rango
   *  `V` es uno de `IEnumerable<V1>`, `ICollection<V1>` o `IList<V1>` y `U` es un tipo de matriz unidimensional `U1[]` (o un parámetro de tipo cuyo tipo base efectivo es `U1[]`)
   *  `V` es un tipo de clase, struct, interfaz o delegado construido `C<V1...Vk>` y hay un tipo único @no__t 2, de modo que `U` (o, si `U` es un parámetro de tipo, su clase base efectiva o cualquier miembro de su conjunto de interfaz efectivo) es idéntico a , hereda de (directa o indirectamente) o implementa (directa o indirectamente) `C<U1...Uk>`.

      (La restricción de unicidad significa que en la interfaz de Case `C<T> {} class U: C<X>, C<Y> {}`, no se produce ninguna inferencia al deducir de `U` a `C<T>` porque `U1` podría ser `X` o `Y`).

   Si se aplica cualquiera de estos casos, se realiza una inferencia *de* cada `Ui` al `Vi` correspondiente, como *se* indica a continuación:

   *  Si no se sabe que `Ui` es un tipo de referencia, se realiza una *inferencia exacta* .
   *  De lo contrario, si `U` es un tipo de matriz, se realiza una *inferencia de límite inferior* .
   *  De lo contrario, si `V` es `C<V1...Vk>`, la inferencia depende del parámetro de tipo i-ésima de `C`:
      *  Si es covariante, se realiza una *inferencia de límite inferior* .
      *  Si es contravariante, se realiza una *inferencia enlazada en el límite superior* .
      *  Si es invariable, se realiza una *inferencia exacta* .
*  De lo contrario, no se realiza ninguna inferencia.

#### <a name="upper-bound-inferences"></a>Inferencias de límite superior

Una *inferencia de enlace superior* *desde* un tipo `U` *a* un tipo `V` se realiza de la manera siguiente:

*  Si `V` es uno de los @no__t *fijos* -2, se agrega `U` al conjunto de límites superiores para `Xi`.
*  De lo contrario, establece `V1...Vk` y `U1...Uk` se determinan comprobando si se aplica cualquiera de los casos siguientes:
   *  `U` es un tipo de matriz `U1[...]` y `V` es un tipo de matriz `V1[...]` del mismo rango
   *  `U` es uno de `IEnumerable<Ue>`, `ICollection<Ue>` o `IList<Ue>` y `V` es un tipo de matriz unidimensional `Ve[]`
   *  `U` es el tipo `U1?` y `V` es el tipo `V1?`
   *  `U` es una clase construida, una estructura, una interfaz o un tipo de delegado `C<U1...Uk>` y `V` es una clase, estructura, interfaz o tipo de delegado que es idéntico a, hereda de (directa o indirectamente) o implementa (directa o indirectamente) un tipo único `C<V1...Vk>`

      (La restricción de unicidad significa que, si tenemos `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, no se realiza ninguna inferencia al deducir de `C<U1>` a `V<Q>`. Las inferencias no se realizan desde `U1` a `X<Q>` o `Y<Q>`).

   Si se aplica cualquiera de estos casos, se realiza una inferencia *de* cada `Ui` al `Vi` correspondiente, como *se* indica a continuación:
   *  Si no se sabe que `Ui` es un tipo de referencia, se realiza una *inferencia exacta* .
   *  De lo contrario, si `V` es un tipo de matriz, se realiza una *inferencia de límite superior* .
   *  De lo contrario, si `U` es `C<U1...Uk>`, la inferencia depende del parámetro de tipo i-ésima de `C`:
      *  Si es covariante, se realiza una *inferencia enlazada en el límite superior* .
      *  Si es contravariante, se realiza una *inferencia de límite inferior* .
      *  Si es invariable, se realiza una *inferencia exacta* .
*  De lo contrario, no se realiza ninguna inferencia.   

#### <a name="fixing"></a>Corregir

Una variable de tipo sin *corregir* `Xi` con un conjunto de límites se *fija* de la siguiente manera:

*  El conjunto de *tipos candidatos* `Uj` comienza como el conjunto de todos los tipos del conjunto de límites de `Xi`.
*  A continuación, examinaremos cada límite de `Xi`: Para cada @no__t de enlace exacto de `Xi`, todos los tipos `Uj` que no sean idénticos a `U` se quitan del conjunto de candidatos. Para cada límite inferior `U` de `Xi` todos los tipos `Uj` a los que *no* se ha quitado una conversión implícita de `U` se quitan del conjunto de candidatos. Para cada límite superior `U` de `Xi` todos los tipos `Uj` desde los que *no* se ha quitado una conversión implícita a `U` del conjunto de candidatos.
*  Si entre los tipos candidatos restantes `Uj` hay un tipo único `V` desde el que hay una conversión implícita a todos los demás tipos candidatos y, a continuación, @no__t 2 se fija en `V`.
*  De lo contrario, se produce un error en la inferencia de tipos.

#### <a name="inferred-return-type"></a>Tipo de valor devuelto deducido

El tipo de valor devuelto deducido de una función anónima `F` se usa durante la inferencia de tipos y la resolución de sobrecarga. El tipo de valor devuelto deducido solo se puede determinar para una función anónima en la que se conocen todos los tipos de parámetro, ya sea porque se especifican explícitamente, se proporcionan a través de una conversión de función anónima o se infieren durante la inferencia de tipos en un genérico envolvente. invocación de método.

El ***tipo de resultado deducido*** se determina de la siguiente manera:

*  Si el cuerpo de `F` es una *expresión* que tiene un tipo, el tipo de resultado deducido de `F` es el tipo de esa expresión.
*  Si el cuerpo de `F` es un *bloque* y el conjunto de expresiones de las instrucciones `return` del bloque tiene un tipo más común `T` ([encontrar el mejor tipo común de un conjunto de expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), el tipo de resultado deducido de `F` es `T`.
*  De lo contrario, no se puede inferir un tipo de resultado para `F`.

El ***tipo de valor devuelto deducido*** se determina de la siguiente manera:

*  Si `F` es Async y el cuerpo de `F` es una expresión clasificada como Nothing ([clasificaciones de expresión](expressions.md#expression-classifications)) o un bloque de instrucciones en el que ninguna instrucción return tiene expresiones, el tipo de valor devuelto deducido es `System.Threading.Tasks.Task`
*  Si `F` es Async y tiene un tipo de resultado inferido `T`, el tipo de valor devuelto deducido es `System.Threading.Tasks.Task<T>`.
*  Si `F` no es asincrónico y tiene un tipo de resultado inferido `T`, el tipo de valor devuelto deducido es @no__t 2.
*  De lo contrario, no se puede inferir un tipo de valor devuelto para `F`.

Como ejemplo de inferencia de tipos que implica funciones anónimas, tenga en cuenta el método de extensión `Select` declarado en la clase `System.Linq.Enumerable`:
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

Suponiendo que el espacio de nombres `System.Linq` se importó con una cláusula `using` y, dada una clase `Customer` con una propiedad `Name` de tipo `string`, se puede usar el método `Select` para seleccionar los nombres de una lista de clientes:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

La invocación del método de extensión ([invocaciones de método de extensión](expressions.md#extension-method-invocations)) de `Select` se procesa rescribiendo la invocación a una invocación de método estático:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Dado que los argumentos de tipo no se especificaron explícitamente, se usa la inferencia de tipos para inferir los argumentos de tipo. En primer lugar, el argumento `customers` está relacionado con el parámetro `source`, infiriendo a @no__t 2 `Customer`. A continuación, con el proceso de inferencia de tipos de función anónima descrito anteriormente, `c` tiene el tipo `Customer` y la expresión `c.Name` está relacionada con el tipo de valor devuelto del parámetro `selector`, infiriendo a `S` para que sea `string`. Por lo tanto, la invocación es equivalente a
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
y el resultado es de tipo `IEnumerable<string>`.

En el ejemplo siguiente se muestra cómo la inferencia de tipos de función anónima permite la información de tipo para "fluir" entre los argumentos de una invocación de método genérico. Dado el método:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Inferencia de tipos para la invocación:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
continúa de la siguiente manera: En primer lugar, el argumento `"1:15:30"` está relacionado con el parámetro `value`, infiriendo a @no__t 2 `string`. A continuación, el parámetro de la primera función anónima, `s`, recibe el tipo deducido `string` y la expresión `TimeSpan.Parse(s)` se relaciona con el tipo de valor devuelto de `f1`, infiriendo a `Y` para que sea `System.TimeSpan`. Por último, el parámetro de la segunda función anónima, `t`, recibe el tipo deducido `System.TimeSpan` y la expresión `t.TotalSeconds` se relaciona con el tipo de valor devuelto de `f2`, infiriendo a `Z` para que sea `double`. Por lo tanto, el resultado de la invocación es de tipo `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Inferencia de tipos para la conversión de grupos de métodos

De forma similar a las llamadas de métodos genéricos, la inferencia de tipos también se debe aplicar cuando un grupo de métodos `M` que contiene un método genérico se convierte en un tipo de delegado determinado `D` ([conversiones de grupo de métodos](conversions.md#method-group-conversions)). Dado un método
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
y el grupo de métodos `M` que se asigna al tipo de delegado `D` la tarea de inferencia de tipo es buscar argumentos de tipo `S1...Sn` para que la expresión:
```csharp
M<S1...Sn>
```
se convierte en compatible ([declaraciones de delegado](delegates.md#delegate-declarations)) con `D`.

A diferencia del algoritmo de inferencia de tipos para las llamadas de método genérico, en este caso solo hay *tipos*de argumento, no hay *expresiones*de argumento. En concreto, no hay ninguna función anónima y, por lo tanto, no es necesario que haya varias fases de inferencia.

En su lugar, todos los `Xi` se consideran sin *corregir*y se realiza una *inferencia de enlace inferior* *desde* cada tipo de argumento `Uj` de `D` *al* tipo de parámetro correspondiente `Tj` de `M`. Si no se encuentra ningún límite en el `Xi`, se produce un error en la inferencia de tipos. De lo contrario, todos los `Xi` se *corrigen* en `Si` correspondiente, que son el resultado de la inferencia de tipos.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Buscar el mejor tipo común de un conjunto de expresiones

En algunos casos, es necesario inferir un tipo común para un conjunto de expresiones. En concreto, los tipos de elemento de matrices con tipo implícito y los tipos de valor devueltos de las funciones anónimas con cuerpos de *bloque* se encuentran de esta manera.

Intuitivamente, dado un conjunto de expresiones `E1...Em`, esta inferencia debe ser equivalente a llamar a un método.
```csharp
Tr M<X>(X x1 ... X xm)
```
con el `Ei` como argumentos.

Más concretamente, la inferencia comienza con una variable de tipo sin *fixed* `X`. A continuación, se realizan *inferencias de tipos de salida* *de* cada `Ei` *a* `X`. Por último, `X` es *fijo* y, si es correcto, el tipo resultante `S` es el mejor tipo común resultante para las expresiones. Si no existe tal `S`, las expresiones no tienen el mejor tipo común.

### <a name="overload-resolution"></a>Resolución de sobrecarga

La resolución de sobrecarga es un mecanismo en tiempo de enlace para seleccionar el mejor miembro de función que se va a invocar a partir de una lista de argumentos y un conjunto de miembros de función candidatos. La resolución de sobrecarga selecciona el miembro de función que se va a invocar en C#los siguientes contextos distintos dentro de:

*  Invocación de un método denominado en un *invocation_expression* ([invocaciones de método](expressions.md#method-invocations)).
*  Invocación de un constructor de instancia denominado en un *object_creation_expression* ([expresiones de creación de objetos](expressions.md#object-creation-expressions)).
*  Invocación de un descriptor de acceso de indexador a través de un *element_access* ([acceso a elementos](expressions.md#element-access)).
*  Invocación de un operador predefinido o definido por el usuario al que se hace referencia en una expresión (resolución de[sobrecarga del operador unario](expressions.md#unary-operator-overload-resolution) y [resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)).

Cada uno de estos contextos define el conjunto de miembros de función candidatas y la lista de argumentos en su propia manera única, tal como se describe en detalle en las secciones mencionadas anteriormente. Por ejemplo, el conjunto de candidatos para una invocación de método no incluye métodos marcados como `override` ([búsqueda de miembros](expressions.md#member-lookup)) y los métodos de una clase base no son candidatos si algún método de una clase derivada es aplicable ([invocaciones de método](expressions.md#method-invocations)).

Una vez identificados los miembros de la función candidata y la lista de argumentos, la selección del mejor miembro de función es la misma en todos los casos:

*  Dado el conjunto de miembros de función candidatos aplicables, se ubica el mejor miembro de función de ese conjunto. Si el conjunto contiene solo un miembro de función, ese miembro de función es el mejor miembro de función. De lo contrario, el mejor miembro de función es un miembro de función que es mejor que todos los demás miembros de función con respecto a la lista de argumentos determinada, siempre que cada miembro de función se compare con el resto de miembros de función con las reglas de [mejor función. miembro](expressions.md#better-function-member). Si no hay exactamente un miembro de función que sea mejor que todos los demás miembros de función, la invocación del miembro de función es ambigua y se produce un error en tiempo de enlace.

En las secciones siguientes se definen los significados exactos de los términos ***miembro de función aplicable*** y ***mejor miembro de función***.

#### <a name="applicable-function-member"></a>Miembro de función aplicable

Se dice que un miembro de función es un ***miembro de función aplicable*** con respecto a una lista de argumentos `A` cuando se cumplen todas las condiciones siguientes:

*  Cada argumento de `A` corresponde a un parámetro de la declaración de miembro de función tal y como se describe en [los parámetros correspondientes](expressions.md#corresponding-parameters), y los parámetros a los que ningún argumento corresponde es un parámetro opcional.
*  Para cada argumento de `A`, el modo de paso de parámetros del argumento (es decir, Value, `ref` o `out`) es idéntico al modo de paso de parámetros del parámetro correspondiente, y
   *  para un parámetro de valor o una matriz de parámetros, existe una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) del argumento al tipo del parámetro correspondiente, o bien
   *  para un parámetro `ref` o `out`, el tipo del argumento es idéntico al tipo del parámetro correspondiente. Después de All, un parámetro `ref` o `out` es un alias para el argumento pasado.

Para un miembro de función que incluye una matriz de parámetros, si el miembro de función es aplicable a las reglas anteriores, se dice que es aplicable en su ***forma normal***. Si un miembro de función que incluye una matriz de parámetros no es aplicable en su forma normal, el miembro de función puede ser aplicable en su ***forma expandida***:

*  La forma expandida se construye reemplazando la matriz de parámetros en la declaración de miembro de función con cero o más parámetros de valor del tipo de elemento de la matriz de parámetros, de modo que el número de argumentos de la lista de argumentos `A` coincide con el número total de parámetros. Si `A` tiene menos argumentos que el número de parámetros fijos en la declaración de miembro de función, no se puede construir la forma expandida del miembro de función y, por tanto, no es aplicable.
*  De lo contrario, la forma expandida es aplicable si para cada argumento de `A` el modo de paso de parámetros del argumento es idéntico al modo de paso de parámetros del parámetro correspondiente, y
   *  para un parámetro de valor fijo o un parámetro de valor creado por la expansión, existe una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo del argumento al tipo del parámetro correspondiente, o bien
   *  para un parámetro `ref` o `out`, el tipo del argumento es idéntico al tipo del parámetro correspondiente.

#### <a name="better-function-member"></a>Mejor miembro de función

Con el fin de determinar el mejor miembro de función, se construye una lista de argumentos con la que se ha quitado una lista que contiene solo las expresiones de argumento en el orden en que aparecen en la lista de argumentos original.

Las listas de parámetros para cada uno de los miembros de la función candidata se construyen de la siguiente manera:

*  La forma expandida se utiliza si el miembro de función solo se aplica en el formulario expandido.
*  Los parámetros opcionales sin argumentos correspondientes se quitan de la lista de parámetros
*  Los parámetros se reordenan para que se produzcan en la misma posición que el argumento correspondiente en la lista de argumentos.

Dada una lista de argumentos `A` con un conjunto de expresiones de argumento `{E1, E2, ..., En}` y dos miembros de función aplicables `Mp` y `Mq` con tipos de parámetro `{P1, P2, ..., Pn}` y `{Q1, Q2, ..., Qn}`, `Mp` se define para ser un ***miembro de función mejor*** que `Mq` si

*  para cada argumento, la conversión implícita de `Ex` a `Qx` no es mejor que la conversión implícita de `Ex` a `Px`, y
*  para al menos un argumento, la conversión de `Ex` a `Px` es mejor que la conversión de `Ex` a `Qx`.

Al realizar esta evaluación, si `Mp` o `Mq` es aplicable en su forma expandida, `Px` o `Qx` hace referencia a un parámetro en la forma expandida de la lista de parámetros.

En el caso de que las secuencias de tipo de parámetro @ no__t-0 y `{Q1, Q2, ..., Qn}` sean equivalentes (es decir, cada `Pi` tiene una conversión de identidad en el `Qi` correspondiente), se aplican las siguientes reglas de separación de desempates, en orden, para determinar el mejor miembro de función.

*  Si `Mp` es un método no genérico y `Mq` es un método genérico, @no__t 2 es mejor que `Mq`.
*  De lo contrario, si `Mp` es aplicable en su forma normal y `Mq` tiene una matriz `params` y solo es aplicable en su forma expandida, `Mp` es mejor que `Mq`.
*  De lo contrario, si `Mp` tiene más parámetros declarados que `Mq`, @no__t 2 es mejor que `Mq`. Esto puede ocurrir si ambos métodos tienen matrices `params` y solo se aplican en sus formularios expandidos.
*  De lo contrario, si todos los parámetros de `Mp` tienen un argumento correspondiente, mientras que los argumentos predeterminados deben sustituirse por al menos un parámetro opcional en `Mq` y, a continuación, @no__t 2 es mejor que `Mq`.
*  De lo contrario, si `Mp` tiene tipos de parámetro más específicos que `Mq`, @no__t 2 es mejor que `Mq`. Permita que `{R1, R2, ..., Rn}` y `{S1, S2, ..., Sn}` representen los tipos de parámetro sin instancia y sin expandir de `Mp` y `Mq`. los tipos de parámetro de `Mp` son más específicos que `Mq` si, para cada parámetro, `Rx` no es menos específico que `Sx` y, para al menos un parámetro, `Rx` es más específico que `Sx`:
   *  Un parámetro de tipo es menos específico que un parámetro sin tipo.
   *  De forma recursiva, un tipo construido es más específico que otro tipo construido (con el mismo número de argumentos de tipo) si al menos un argumento de tipo es más específico y ningún argumento de tipo es menos específico que el argumento de tipo correspondiente en el otro.
   *  Un tipo de matriz es más específico que otro tipo de matriz (con el mismo número de dimensiones) si el tipo de elemento del primero es más específico que el tipo de elemento del segundo.
*  De lo contrario, si un miembro es un operador no de elevación y el otro es un operador de elevación, el valor de uno no elevado es mejor.
*  De lo contrario, ninguno de los miembros de función es mejor.

#### <a name="better-conversion-from-expression"></a>Mejor conversión de la expresión

Dada una conversión implícita `C1` que convierte de una expresión `E` a un tipo `T1` y una conversión implícita `C2` que convierte de una expresión `E` a un tipo `T2`, `C1` es una ***conversión mejor*** que `C2` si @no__ t-9 no coincide exactamente con 0 y al menos una de las siguientes:

* `E` coincide exactamente con `T1` ([expresión de coincidencia exacta](expressions.md#exactly-matching-expression))
* `T1` es un mejor destino de conversión que `T2` ([mejor destino](expressions.md#better-conversion-target)de la conversión)

#### <a name="exactly-matching-expression"></a>Expresión coincidente exactamente

Dada una expresión `E` y un tipo `T`, `E` coincide exactamente con `T` Si uno de los siguientes contiene:

*  `E` tiene un tipo `S` y existe una conversión de identidad de `S` a `T`
*  `E` es una función anónima, `T` es un tipo de delegado `D` o un tipo de árbol de expresión `Expression<D>` y uno de los siguientes:
   *  Un tipo de valor devuelto deducido `X` existe para `E` en el contexto de la lista de parámetros de `D` ([tipo de valor devuelto deducido](expressions.md#inferred-return-type)) y existe una conversión de identidad de `X` al tipo de valor devuelto de `D`
   *  @No__t-0 no es Async y `D` tiene un tipo de valor devuelto `Y` o `E` es Async y `D` tiene un tipo de valor devuelto `Task<Y>` y uno de los siguientes:
      * El cuerpo de `E` es una expresión que coincide exactamente con `Y`
      * El cuerpo de `E` es un bloque de instrucciones en el que cada instrucción return devuelve una expresión que coincide exactamente con `Y`

#### <a name="better-conversion-target"></a>Mejor destino de la conversión

Dados dos tipos diferentes `T1` y `T2`, @no__t 2 es un mejor destino de conversión que `T2` si no existe una conversión implícita de `T2` a `T1` y al menos uno de los siguientes:

*  Existe una conversión implícita de `T1` a `T2`
*  `T1` es un tipo de delegado `D1` o un tipo de árbol de expresión `Expression<D1>`, `T2` es un tipo de delegado `D2` o un tipo de árbol de expresión `Expression<D2>`, `D1` tiene un tipo de valor devuelto `S1` y uno de los siguientes :
   * `D2` es void devolviendo
   * `D2` tiene un tipo de valor devuelto `S2` y `S1` es un destino de conversión mejor que `S2`
*  `T1` es `Task<S1>`, @no__t 2 es `Task<S2>` y `S1` es un destino de conversión mejor que `S2`
*  `T1` es `S1` o `S1?` donde `S1` es un tipo entero con signo y `T2` es `S2` o `S2?`, donde `S2` es un tipo entero sin signo. De manera específica:
   * `S1` es `sbyte` y @no__t 2 es `byte`, `ushort`, `uint` o `ulong`
   * `S1` es `short` y `S2` es `ushort`, `uint` o `ulong`
   * `S1` es `int` y `S2` es `uint` o `ulong`
   * `S1` es `long` y `S2` es `ulong`

#### <a name="overloading-in-generic-classes"></a>Sobrecarga en clases genéricas

Mientras que las signaturas declaradas deben ser únicas, es posible que la sustitución de los argumentos de tipo dé como resultado firmas idénticas. En tales casos, las reglas de separación de sobrecargas de la resolución de sobrecarga anterior seleccionarán el miembro más específico.

En los ejemplos siguientes se muestran las sobrecargas válidas y no válidas según esta regla:

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Comprobación en tiempo de compilación de la resolución dinámica de sobrecarga

Para la mayoría de las operaciones enlazadas dinámicamente, el conjunto de posibles candidatos para la resolución se desconoce en tiempo de compilación. En algunos casos, sin embargo, el conjunto de candidatos se conoce en tiempo de compilación:

*  Llamadas a métodos estáticos con argumentos dinámicos
*  Llamadas al método de instancia donde el receptor no es una expresión dinámica
*  Llamadas de indexador en las que el receptor no es una expresión dinámica
*  Llamadas de constructor con argumentos dinámicos

En estos casos, se realiza una comprobación limitada en tiempo de compilación para cada candidato a fin de ver si alguna de ellas podría aplicarse en tiempo de ejecución. Esta comprobación consta de los siguientes pasos:

*  Inferencia de tipos parciales: Cualquier argumento de tipo que no dependa directa o indirectamente de un argumento de tipo `dynamic` se deduce mediante las reglas de [inferencia de tipos](expressions.md#type-inference). Los argumentos de tipo restantes son desconocidos.
*  Comprobación parcial de aplicabilidad: La aplicabilidad se comprueba según el [miembro de función aplicable](expressions.md#applicable-function-member), pero se omiten los parámetros cuyos tipos son desconocidos.
*  Si ningún candidato pasa esta prueba, se produce un error en tiempo de compilación.

### <a name="function-member-invocation"></a>Invocación de miembros de función

En esta sección se describe el proceso que tiene lugar en tiempo de ejecución para invocar un miembro de función determinado. Se supone que un proceso de tiempo de enlace ya ha determinado el miembro determinado que se va a invocar, posiblemente aplicando la resolución de sobrecarga a un conjunto de miembros de función candidatos.

Con el fin de describir el proceso de invocación, los miembros de función se dividen en dos categorías:

*  Miembros de función estáticos. Se trata de constructores de instancia, métodos estáticos, descriptores de acceso de propiedades estáticas y operadores definidos por el usuario. Los miembros de función estáticos siempre son no virtuales.
*  Miembros de función de instancia. Son métodos de instancia, descriptores de acceso de propiedades de instancia y descriptores de acceso de indexador. Los miembros de la función de instancia son no virtuales o virtuales, y siempre se invocan en una instancia determinada. La instancia se calcula mediante una expresión de instancia y se vuelve accesible dentro del miembro de función como `this` ([este acceso](expressions.md#this-access)).

El procesamiento en tiempo de ejecución de una invocación de miembro de función consta de los pasos siguientes, donde `M` es el miembro de función y, si `M` es un miembro de instancia, `E` es la expresión de instancia:

*  Si `M` es un miembro de función estático:
   * La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).
   * se invoca `M`.

*  Si `M` es un miembro de función de instancia declarado en *value_type*:
   * se evalúa `E`. Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.
   * Si `E` no está clasificado como una variable, se crea una variable local temporal de tipo `E` y se asigna el valor de `E` a esa variable. a continuación, `E` se reclasifica como una referencia a esa variable local temporal. La variable temporal es accesible como `this` dentro de `M`, pero no de otra manera. Por lo tanto, solo cuando `E` es una variable verdadera, es posible que el llamador Observe los cambios que `M` realiza en `this`.
   * La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).
   * se invoca `M`. La variable a la que hace referencia `E` se convierte en la variable a la que hace referencia `this`.

*  Si `M` es un miembro de función de instancia declarado en *reference_type*:
   * se evalúa `E`. Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.
   * La lista de argumentos se evalúa como se describe en [listas de argumentos](expressions.md#argument-lists).
   * Si el tipo de `E` es *value_type*, se realiza una conversión boxing ([conversiones boxing](types.md#boxing-conversions)) para convertir `E` al tipo `object` y `E` se considera de tipo `object` en los pasos siguientes. En este caso, `M` solo puede ser un miembro de `System.Object`.
   * El valor de `E` se comprueba para ser válido. Si el valor de `E` es `null`, se produce una @no__t 2 y no se ejecuta ningún otro paso.
   * Se determina la implementación del miembro de función que se va a invocar:
     * Si el tipo de tiempo de enlace de `E` es una interfaz, el miembro de función que se va a invocar es la implementación de `M` proporcionada por el tipo en tiempo de ejecución de la instancia a la que hace referencia `E`. Este miembro de función se determina aplicando las reglas de asignación de interfaz ([asignación de interfaz](interfaces.md#interface-mapping)) para determinar la implementación de `M` proporcionada por el tipo en tiempo de ejecución de la instancia a la que hace referencia `E`.
     * De lo contrario, si `M` es un miembro de función virtual, el miembro de función que se va a invocar es la implementación de `M` proporcionada por el tipo en tiempo de ejecución de la instancia a la que hace referencia `E`. Este miembro de función se determina aplicando las reglas para determinar la implementación más derivada ([métodos virtuales](classes.md#virtual-methods)) de `M` con respecto al tipo en tiempo de ejecución de la instancia a la que hace referencia `E`.
     * De lo contrario, `M` es un miembro de función no virtual y el miembro de función que se va a invocar es `M`.
   * Se invoca la implementación de miembro de función determinada en el paso anterior. El objeto al que hace referencia `E` se convierte en el objeto al que hace referencia `this`.

#### <a name="invocations-on-boxed-instances"></a>Invocaciones en instancias de conversión boxing

Un miembro de función implementado en un *value_type* se puede invocar a través de una instancia de conversión boxing de ese *value_type* en las situaciones siguientes:

*  Cuando el miembro de función es un `override` de un método heredado del tipo `object` y se invoca a través de una expresión de instancia de tipo `object`.
*  Cuando el miembro de función es una implementación de un miembro de función de interfaz y se invoca a través de una expresión de instancia de un *interface_type*.
*  Cuando el miembro de función se invoca a través de un delegado.

En estas situaciones, se considera que la instancia con conversión boxing contiene una variable de *value_type*y esta variable se convierte en la variable a la que hace referencia `this` dentro de la invocación del miembro de función. En concreto, esto significa que cuando se invoca un miembro de función en una instancia de conversión boxing, es posible que el miembro de función modifique el valor contenido en la instancia de conversión boxing.

## <a name="primary-expressions"></a>Expresiones primarias

Las expresiones primarias incluyen las formas más sencillas de las expresiones.

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

Las expresiones primarias se dividen entre *array_creation_expression*s y *primary_no_array_creation_expression*s. Al tratar la expresión de creación de matrices de esta manera, en lugar de enumerarla junto con las otras formas de expresión simples, permite que la gramática no permita el código potencialmente confuso, como
```csharp
object o = new int[3][1];
```
que de otro modo se interpretaría como
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Literales

Un *primary_expression* que consta de un *literal* ([literales](lexical-structure.md#literals)) se clasifica como un valor.


### <a name="interpolated-strings"></a>Cadenas interpoladas

Un *interpolated_string_expression* consta de un signo de @no__t 1 seguido de un literal de cadena normal o textual, en el que los huecos, delimitados por `{` y `}`, incluyen expresiones y especificaciones de formato. Una expresión de cadena interpolada es el resultado de un *interpolated_string_literal* que se ha dividido en tokens individuales, tal y como se describe en [literales de cadena interpolados](lexical-structure.md#interpolated-string-literals).

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

El *constant_expression* de una interpolación debe tener una conversión implícita a `int`.

Un *interpolated_string_expression* se clasifica como un valor. Si se convierte inmediatamente en `System.IFormattable` o `System.FormattableString` con una conversión de cadena interpolada implícita ([conversiones de cadenas interpoladas implícitas](conversions.md#implicit-interpolated-string-conversions)), la expresión de cadena interpolada tiene ese tipo. De lo contrario, tiene el tipo `string`.

Si el tipo de una cadena interpolada es `System.IFormattable` o `System.FormattableString`, el significado es una llamada a `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Si el tipo es `string`, el significado de la expresión es una llamada a `string.Format`. En ambos casos, la lista de argumentos de la llamada consta de un literal de cadena de formato con marcadores de posición para cada interpolación y un argumento para cada expresión correspondiente a los marcadores de posición.

El literal de cadena de formato se construye como sigue, donde `N` es el número de interpolaciones de *interpolated_string_expression*:

*  Si un *interpolated_regular_string_whole* o un *interpolated_verbatim_string_whole* siguen el signo `$`, el literal de cadena de formato es ese token.
*  De lo contrario, el literal de cadena de formato consta de: 
   *  En primer lugar, *interpolated_regular_string_start* o *interpolated_verbatim_string_start*
   *  A continuación, para cada número `I` de `0` a `N-1`: 
      * Representación decimal de `I`
      * Después, si la *interpolación* correspondiente tiene un *constant_expression*, una `,` (coma) seguida de la representación decimal del valor de *constant_expression*
      * Después, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* o *interpolated_verbatim_string_end* inmediatamente después de la interpolación correspondiente.

Los argumentos subsiguientes son simplemente las *expresiones* de las *interpolaciones* (si existen), en orden.

TODO: ejemplos.


### <a name="simple-names"></a>Nombres simples

Un *simple_name* consta de un identificador y, opcionalmente, seguido de una lista de argumentos de tipo:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

Un *simple_name* tiene el formato `I` o el formulario `I<A1,...,Ak>`, donde `I` es un identificador único y `<A1,...,Ak>` es un *type_argument_list*opcional. Si no se especifica *type_argument_list* , considere la posibilidad de `K` para que sea cero. El *simple_name* se evalúa y se clasifica de la manera siguiente:

*  Si `K` es cero y *simple_name* aparece dentro de un *bloque* y si el espacio de declaración de variable local del *bloque*(o el de un *bloque*de[inclusión) contiene](basic-concepts.md#declarations)una variable local, un parámetro o una constante con Name @ no__t-6, el *simple_name* hace referencia a esa variable local, parámetro o constante y se clasifica como una variable o un valor.
*  Si `K` es cero y *simple_name* aparece dentro del cuerpo de una declaración de método genérico y la Declaración incluye un parámetro de tipo con el nombre @ no__t-2, el *simple_name* hace referencia a ese parámetro de tipo.
*  De lo contrario, para cada tipo de instancia @ no__t-0 ([el tipo de instancia](classes.md#the-instance-type)), empezando por el tipo de instancia de la declaración de tipo de inclusión inmediata y continuando con el tipo de instancia de cada declaración de clase o estructura envolvente (si existe):
   *  Si `K` es cero y la declaración de `T` incluye un parámetro de tipo con el nombre @ no__t-2, el *simple_name* hace referencia a ese parámetro de tipo.
   *  De lo contrario, si una búsqueda de miembro ([búsqueda de miembros](expressions.md#member-lookup)) de `I` en `T` con argumentos `K` @ no__t-4Type genera una coincidencia:
      * Si `T` es el tipo de instancia de la clase o el tipo de estructura de inclusión inmediata y la búsqueda identifica uno o más métodos, el resultado es un grupo de métodos con una expresión de instancia asociada de `this`. Si se especificó una lista de argumentos de tipo, se usa para llamar a un método genérico ([invocaciones de método](expressions.md#method-invocations)).
      * De lo contrario, si `T` es el tipo de instancia de la clase o el tipo de estructura que se encuentra inmediatamente, si la búsqueda identifica un miembro de instancia, y si la referencia aparece dentro del cuerpo de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia, el resultado es igual que un acceso a miembro ([acceso a miembros](expressions.md#member-access)) con el formato `this.I`. Esto solo puede ocurrir cuando `K` es cero.
      * De lo contrario, el resultado es el mismo que un acceso a miembro ([acceso a miembros](expressions.md#member-access)) con el formato `T.I` o `T.I<A1,...,Ak>`. En este caso, es un error en tiempo de enlace para que *simple_name* haga referencia a un miembro de instancia.

*  De lo contrario, para cada espacio de nombres @ no__t-0, empezando por el espacio de nombres en el que se produce *simple_name* , continuando con cada espacio de nombres envolvente (si existe) y finalizando con el espacio de nombres global, se evalúan los pasos siguientes hasta que se encuentra una entidad:
   *  Si `K` es cero y `I` es el nombre de un espacio de nombres en @ no__t-2, entonces:
      * Si la ubicación donde se produce el *simple_name* se incluye en una declaración de espacio de nombres para `N` y la declaración del espacio de nombres contiene un *extern_alias_directive* o un *using_alias_directive* que asocia el nombre @ no__t-4 con un espacio de nombres o tipo, *simple_name* es ambiguo y se produce un error en tiempo de compilación.
      * De lo contrario, *simple_name* hace referencia al espacio de nombres denominado `I` en `N`.
   *  De lo contrario, si `N` contiene un tipo accesible con los parámetros Name @ no__t-1 y `K` @ no__t-3type, entonces:
      * Si `K` es cero y la ubicación donde se produce el *simple_name* se incluye en una declaración de espacio de nombres para `N` y la declaración del espacio de nombres contiene un *extern_alias_directive* o un *using_alias_directive* que asocia el Name @ no__t-5 con un espacio de nombres o un tipo, el *simple_name* es ambiguo y se produce un error en tiempo de compilación.
      * De lo contrario, *namespace_or_type_name* hace referencia al tipo construido con los argumentos de tipo especificados.
   *  De lo contrario, si la ubicación donde se produce el *simple_name* se incluye en una declaración de espacio de nombres para @ no__t-1:
      * Si `K` es cero y la declaración del espacio de nombres contiene un *extern_alias_directive* o un *using_alias_directive* que asocia el nombre @ no__t-3 con un espacio de nombres o tipo importado, el *simple_name* hace referencia a ese espacio de nombres o automáticamente.
      * De lo contrario, si los espacios de nombres y las declaraciones de tipos importados por *using_namespace_directive*s y *using_static_directive*s de la declaración del espacio de nombres contienen exactamente un tipo accesible o un miembro estático que no es de extensión con el nombre @ No__ los parámetros t-2 y `K` @ no__t-4Type, *simple_name* hace referencia a ese tipo o miembro construido con los argumentos de tipo especificados.
      * De lo contrario, si los espacios de nombres y los tipos importados por *using_namespace_directive*s de la declaración de espacio de nombres contienen más de un tipo accesible o un miembro estático de método que no es de extensión con los parámetros Name @ no__t-1 y `K` @ no__t-3type, a continuación, el *simple_name* es ambiguo y se produce un error.

   Tenga en cuenta que todo el paso es exactamente paralelo al paso correspondiente en el procesamiento de un *namespace_or_type_name* (espacio de nombres[y nombres de tipo](basic-concepts.md#namespace-and-type-names)).

*  De lo contrario, *simple_name* no está definido y se produce un error en tiempo de compilación.


### <a name="parenthesized-expressions"></a>Expresiones entre paréntesis

Un *parenthesized_expression* consta de una *expresión* entre paréntesis.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

Un *parenthesized_expression* se evalúa evaluando la *expresión* entre paréntesis. Si la *expresión* entre paréntesis denota un espacio de nombres o un tipo, se produce un error en tiempo de compilación. De lo contrario, el resultado de *parenthesized_expression* es el resultado de la evaluación de la *expresión*contenida.

### <a name="member-access"></a>Acceso a miembros

Un *member_access* consta de *primary_expression*, *predefined_type*o *qualified_alias_member*, seguido de un token "`.`", seguido de un *identificador*, seguido opcionalmente de un *type_argument_list* .

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

La producción *qualified_alias_member* se define en [calificadores de alias del espacio de nombres](namespaces.md#namespace-alias-qualifiers).

Un *member_access* tiene el formato `E.I` o el formulario `E.I<A1, ..., Ak>`, donde `E` es una expresión primaria, @no__t 4 es un identificador único y `<A1, ..., Ak>` es un *type_argument_list*opcional. Si no se especifica *type_argument_list* , considere la posibilidad de `K` para que sea cero.

Un *member_access* con un *primary_expression* de tipo `dynamic` está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso, el compilador clasifica el acceso a miembros como un acceso de propiedad de tipo `dynamic`. A continuación, se aplican las siguientes reglas para determinar el significado de *member_access* en tiempo de ejecución, utilizando el tipo en tiempo de ejecución en lugar del tipo en tiempo de compilación de *primary_expression*. Si esta clasificación en tiempo de ejecución conduce a un grupo de métodos, el acceso a miembros debe ser el *primary_expression* de un *invocation_expression*.

El *member_access* se evalúa y se clasifica de la manera siguiente:

*  Si `K` es cero y `E` es un espacio de nombres y `E` contiene un espacio de nombres anidado con el nombre @ no__t-3, el resultado es dicho espacio de nombres.
*  De lo contrario, si `E` es un espacio de nombres y `E` contiene un tipo accesible con los parámetros Name @ no__t-2 y `K` @ no__t-4Type, el resultado será ese tipo construido con los argumentos de tipo especificados.
*  Si `E` es un *predefined_type* o un *primary_expression* clasificado como un tipo, si `E` no es un parámetro de tipo, y si una búsqueda de miembro ([búsqueda de miembros](expressions.md#member-lookup)) de `I` en `E` con parámetros `K` @ no__t-8type genera una coincidencia, a continuación, `E.I` se evalúa y se clasifica como sigue:
   *  Si `I` identifica un tipo, el resultado es ese tipo construido con los argumentos de tipo especificados.
   *  Si `I` identifica uno o más métodos, el resultado es un grupo de métodos sin expresión de instancia asociada. Si se especificó una lista de argumentos de tipo, se usa para llamar a un método genérico ([invocaciones de método](expressions.md#method-invocations)).
   *  Si `I` identifica una propiedad `static`, el resultado es un acceso de propiedad sin expresión de instancia asociada.
   *  Si `I` identifica un campo `static`:
      * Si el campo es `readonly` y la referencia se produce fuera del constructor estático de la clase o estructura en la que se declara el campo, el resultado es un valor, es decir, el valor del campo estático @ no__t-1 en @ no__t-2.
      * De lo contrario, el resultado es una variable, es decir, el campo estático @ no__t-0 en @ no__t-1.
   *  Si `I` identifica un evento `static`:
      * Si la referencia aparece dentro de la clase o estructura en la que se declara el evento y el evento se declaró sin *event_accessor_declarations* ([eventos](classes.md#events)), `E.I` se procesa exactamente como si `I` fuera un campo estático.
      * De lo contrario, el resultado es un acceso de evento sin expresión de instancia asociada.
   *  Si `I` identifica una constante, el resultado es un valor, es decir, el valor de dicha constante.
    * Si `I` identifica un miembro de enumeración, el resultado es un valor, es decir, el valor de ese miembro de la enumeración.
    * De lo contrario, `E.I` es una referencia de miembro no válida y se produce un error en tiempo de compilación.
*  Si `E` es un acceso de propiedad, un acceso de indexador, una variable o un valor, el tipo de que es @ no__t-1, y una búsqueda de miembro ([búsqueda de miembros](expressions.md#member-lookup)) de `I` en `T` con `K` @ no__t-6type arguments genera una coincidencia, se evalúa `E.I` y se clasifica como sigue:
   *  En primer lugar, si `E` es un acceso de propiedad o indizador, se obtiene el valor de la propiedad o el acceso del indexador ([valores de las expresiones](expressions.md#values-of-expressions)) y `E` se reclasifica como un valor.
   *  Si `I` identifica uno o más métodos, el resultado es un grupo de métodos con una expresión de instancia asociada de `E`. Si se especificó una lista de argumentos de tipo, se usa para llamar a un método genérico ([invocaciones de método](expressions.md#method-invocations)).
   *  Si `I` identifica una propiedad de instancia,
      * Si `E` es `this`, @no__t 2 identifica una propiedad implementada automáticamente ([propiedades implementadas automáticamente](classes.md#automatically-implemented-properties)) sin un establecedor y la referencia se produce dentro de un constructor de instancia para un tipo de clase o struct `T`, el resultado es una variable, es decir, el campo de respaldo oculto para la propiedad automática proporcionada por `I` en la instancia de `T` proporcionada por `this`.
      * De lo contrario, el resultado es un acceso de propiedad con una expresión de instancia asociada de @ no__t-0.
   *  Si `T` es un *class_type* y `I` identifica un campo de instancia de ese *class_type*:
      * Si el valor de `E` es `null`, se produce una `System.NullReferenceException`.
      * De lo contrario, si el campo es `readonly` y la referencia se produce fuera de un constructor de instancia de la clase en la que se declara el campo, el resultado es un valor, es decir, el valor del campo @ no__t-1 en el objeto al que hace referencia @ no__t-2.
      * De lo contrario, el resultado es una variable, es decir, el campo @ no__t-0 en el objeto al que hace referencia @ no__t-1.
   *  Si `T` es un *struct_type* y `I` identifica un campo de instancia de ese *struct_type*:
      * Si `E` es un valor, o si el campo es `readonly` y la referencia se produce fuera de un constructor de instancia de la estructura en la que se declara el campo, el resultado es un valor, es decir, el valor del campo @ no__t-2 en la instancia de struct especificada por @ no__t-3.
      * De lo contrario, el resultado es una variable, es decir, el campo @ no__t-0 en la instancia de struct proporcionada por @ no__t-1.
   *  Si `I` identifica un evento de instancia:
      * Si la referencia se produce dentro de la clase o estructura en la que se declara el evento y el evento se declaró sin *event_accessor_declarations* ([eventos](classes.md#events)), y la referencia no se produce como el lado izquierdo de un operador `+=` o `-=` , `E.I` se procesa exactamente como si `I` fuera un campo de instancia.
      * De lo contrario, el resultado es un evento de acceso con una expresión de instancia asociada de @ no__t-0.
*  De lo contrario, se realiza un intento de procesar `E.I` como una invocación de método de extensión ([invocaciones de método de extensión](expressions.md#extension-method-invocations)). Si se produce un error, `E.I` es una referencia de miembro no válida y se produce un error en tiempo de enlace.

#### <a name="identical-simple-names-and-type-names"></a>Nombres simples y nombres de tipo idénticos

En un acceso de miembro con el formato `E.I`, si `E` es un identificador único y si el significado de `E` como *simple_name* ([nombres simples](expressions.md#simple-names)) es una constante, un campo, una propiedad, una variable local o un parámetro con el mismo tipo que el significado de `E` como *type_name* ([espacio de nombres y nombres de tipo](basic-concepts.md#namespace-and-type-names)), se permiten ambos significados posibles de `E`. Los dos significados posibles de `E.I` nunca son ambiguos, ya que `I` debe ser necesariamente miembro del tipo `E` en ambos casos. En otras palabras, la regla simplemente permite el acceso a los miembros estáticos y los tipos anidados de `E`, donde en caso contrario, se produciría un error en tiempo de compilación. Por ejemplo:
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

#### <a name="grammar-ambiguities"></a>Ambigüedades de la gramática

Las producciones para *simple_name* ([nombres simples](expressions.md#simple-names)) y *member_access* ([acceso a miembros](expressions.md#member-access)) pueden dar lugar a ambigüedades en la gramática de expresiones. Por ejemplo, la instrucción:
```csharp
F(G<A,B>(7));
```
podría interpretarse como una llamada a `F` con dos argumentos, `G < A` y `B > (7)`. Como alternativa, podría interpretarse como una llamada a `F` con un argumento, que es una llamada a un método genérico @ no__t-1 con dos argumentos de tipo y un argumento normal.

Si se puede analizar una secuencia de tokens (en contexto) como *simple_name* ([nombres simples](expressions.md#simple-names)), *member_access* ([acceso a miembros](expressions.md#member-access)) o *pointer_member_access* ([acceso a miembros de puntero](unsafe-code.md#pointer-member-access)) que finaliza con un *type_argument_ List* ([argumentos de tipo](types.md#type-arguments)), se examina el token inmediatamente después del token `>` de cierre. Si es uno de
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
a continuación, *type_argument_list* se conserva como parte de *simple_name*, *member_access* o *pointer_member_access* y se descarta cualquier otro posible análisis de la secuencia de tokens. De lo contrario, *type_argument_list* no se considera parte de *simple_name*, *member_access* o *pointer_member_access*, incluso si no hay ningún otro análisis posible de la secuencia de tokens. Tenga en cuenta que estas reglas no se aplican al analizar un *type_argument_list* en un *namespace_or_type_name* (espacio de nombres[y nombres de tipo](basic-concepts.md#namespace-and-type-names)). La instrucción
```csharp
F(G<A,B>(7));
```
, según esta regla, se interpretará como una llamada a `F` con un argumento, que es una llamada a un método genérico `G` con dos argumentos de tipo y un argumento normal. Las instrucciones
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
cada uno de ellos se interpretará como una llamada a `F` con dos argumentos. La instrucción
```csharp
x = F < A > +y;
```
se interpretará como un operador menor que, mayor que y unario más, como si la instrucción se hubiera escrito `x = (F < A) > (+y)`, en lugar de como un *simple_name* con un *type_argument_list* seguido de un operador binario Plus. En la instrucción
```csharp
x = y is C<T> + z;
```
los tokens `C<T>` se interpretan como *namespace_or_type_name* con *type_argument_list*.

### <a name="invocation-expressions"></a>Expresiones de invocación

Un *invocation_expression* se usa para invocar un método.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

Un *invocation_expression* está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) si al menos uno de los siguientes contiene:

* *Primary_expression* tiene el tipo en tiempo de compilación `dynamic`.
* Al menos un argumento de *argument_list* opcional tiene el tipo en tiempo de compilación `dynamic` y *primary_expression* no tiene un tipo de delegado.

En este caso, el compilador clasifica *invocation_expression* como un valor de tipo `dynamic`. A continuación, se aplican las siguientes reglas para determinar el significado de *invocation_expression* en tiempo de ejecución, utilizando el tipo en tiempo de ejecución en lugar del tipo en tiempo de compilación de los *primary_expression* y argumentos que tienen el tipo en tiempo de compilación @no_ _ t-2. Si el *primary_expression* no tiene el tipo en tiempo de compilación `dynamic`, la invocación del método sufre una comprobación limitada del tiempo de compilación, como se describe en [comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

El *primary_expression* de un *invocation_expression* debe ser un grupo de métodos o un valor de *delegate_type*. Si *primary_expression* es un grupo de métodos, *invocation_expression* es una invocación de método ([invocaciones de método](expressions.md#method-invocations)). Si *primary_expression* es un valor de *delegate_type*, *invocation_expression* es una invocación de delegado ([invocaciones de delegado](expressions.md#delegate-invocations)). Si *primary_expression* no es un grupo de métodos ni un valor de *delegate_type*, se produce un error en tiempo de enlace.

El *argument_list* opcional ([listas de argumentos](expressions.md#argument-lists)) proporciona valores o referencias a variables para los parámetros del método.

El resultado de la evaluación de un *invocation_expression* se clasifica como sigue:

*  Si el *invocation_expression* invoca un método o un delegado que devuelve `void`, el resultado es Nothing. Una expresión que se clasifique como Nothing solo se permite en el contexto de *statement_expression* ([instrucciones de expresión](statements.md#expression-statements)) o como el cuerpo de un *lambda_expression* ([expresiones de función anónima](expressions.md#anonymous-function-expressions)). En caso contrario, se produce un error en tiempo de enlace.
*  De lo contrario, el resultado es un valor del tipo devuelto por el método o el delegado.

#### <a name="method-invocations"></a>Invocaciones de método

En el caso de una invocación de método, el *primary_expression* de *invocation_expression* debe ser un grupo de métodos. El grupo de métodos identifica el método que se va a invocar o el conjunto de métodos sobrecargados desde los que se va a elegir un método específico que se va a invocar. En el último caso, la determinación del método específico que se va a invocar se basa en el contexto proporcionado por los tipos de los argumentos de *argument_list*.

El procesamiento en tiempo de enlace de una invocación de método con la forma `M(A)`, donde `M` es un grupo de métodos (posiblemente incluir un *type_argument_list*) y `A` es un *argument_list*opcional, consta de los siguientes pasos:

*  Se construye el conjunto de métodos candidatos para la invocación del método. Para cada método `F` asociado al grupo de métodos `M`:
   *  Si `F` no es genérico, `F` es un candidato cuando:
      * `M` no tiene ninguna lista de argumentos de tipo y
      * `F` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).
   *  Si `F` es genérico y `M` no tiene ninguna lista de argumentos de tipo, @no__t 2 es candidata cuando:
      * La inferencia de tipos ([inferencia de tipo](expressions.md#type-inference)) se realiza correctamente, deduciendo una lista de argumentos de tipo para la llamada y
      * Una vez que los argumentos de tipo deducido se sustituyen por los parámetros de tipo de método correspondientes, todos los tipos construidos en la lista de parámetros de F satisfacen sus restricciones (que cumplen con las[restricciones](types.md#satisfying-constraints)) y la lista de parámetros de `F` es aplicable con respecto a @no__t 2 ([miembro de función aplicable](expressions.md#applicable-function-member)).
   *  Si `F` es genérico y `M` incluye una lista de argumentos de tipo, @no__t 2 es candidata cuando:
      * `F` tiene el mismo número de parámetros de tipo de método que se proporcionaron en la lista de argumentos de tipo y
      * Una vez que los argumentos de tipo se sustituyen por los parámetros de tipo de método correspondientes, todos los tipos construidos en la lista de parámetros de F satisfacen sus restricciones (que cumplen con las[restricciones](types.md#satisfying-constraints)) y la lista de parámetros de `F` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)).
*  El conjunto de métodos candidatos se reduce para contener solo los métodos de los tipos más derivados: Para cada método `C.F` del conjunto, donde `C` es el tipo en el que se declara el método `F`, todos los métodos declarados en un tipo base de `C` se quitan del conjunto. Además, si `C` es un tipo de clase distinto de `object`, todos los métodos declarados en un tipo de interfaz se quitan del conjunto. (Esta última regla solo tiene efecto cuando el grupo de métodos era el resultado de una búsqueda de miembros en un parámetro de tipo que tiene una clase base efectiva que no es un objeto y un conjunto de interfaces efectivo no vacío).
*  Si el conjunto resultante de métodos candidatos está vacío, se abandona el procesamiento posterior a lo largo de los pasos siguientes y, en su lugar, se intenta procesar la invocación como una invocación de método de extensión ([invocaciones de método de extensión](expressions.md#extension-method-invocations)). Si se produce un error, no existe ningún método aplicable y se produce un error en tiempo de enlace.
*  El mejor método del conjunto de métodos candidatos se identifica mediante las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution). Si no se puede identificar un único método mejor, la invocación del método es ambigua y se produce un error en tiempo de enlace. Al realizar la resolución de sobrecarga, los parámetros de un método genérico se tienen en cuenta después de sustituir los argumentos de tipo (proporcionados o deducidos) para los parámetros de tipo de método correspondientes.
*  Se realiza la validación final del mejor método elegido:
   * El método se valida en el contexto del grupo de métodos: Si el mejor método es un método estático, el grupo de métodos debe haber sido el resultado de un *simple_name* o un *member_access* a través de un tipo. Si el mejor método es un método de instancia, el grupo de métodos debe haber sido el resultado de un *simple_name*, un *member_access* a través de una variable o un valor o un *base_access*. Si ninguno de estos requisitos es true, se produce un error en tiempo de enlace.
   * Si el mejor método es un método genérico, los argumentos de tipo (suministrados o deducidos) se comprueban con las restricciones (que[cumplen las restricciones](types.md#satisfying-constraints)) declaradas en el método genérico. Si algún argumento de tipo no satisface las restricciones correspondientes en el parámetro de tipo, se produce un error en tiempo de enlace.

Una vez que se ha seleccionado y validado un método en tiempo de enlace según los pasos anteriores, la invocación real en tiempo de ejecución se procesa de acuerdo con las reglas de invocación de miembros de función descritas en la [comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

El efecto intuitivo de las reglas de resolución descritas anteriormente es el siguiente: Para buscar el método determinado invocado por una invocación de método, empiece con el tipo indicado por la invocación del método y continúe con la cadena de herencia hasta que se encuentre al menos una declaración de método aplicable, accesible y sin invalidación. A continuación, realice la inferencia de tipos y la resolución de sobrecarga en el conjunto de métodos aplicables, accesibles y no invalidaciones declarados en ese tipo e invoque el método de la forma que se seleccione. Si no se encuentra ningún método, pruebe en su lugar para procesar la invocación como una invocación de método de extensión.

#### <a name="extension-method-invocations"></a>Invocaciones de métodos de extensión

En una invocación de método ([invocaciones en instancias de conversión boxing](expressions.md#invocations-on-boxed-instances)) de uno de los formularios
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
Si el procesamiento normal de la invocación no encuentra ningún método aplicable, se realiza un intento de procesar la construcción como una invocación de método de extensión. Si *expr* o cualquiera de los *argumentos* tiene el tipo en tiempo de compilación `dynamic`, los métodos de extensión no se aplicarán.

El objetivo es encontrar el mejor *type_name* `C`, de modo que pueda tener lugar la invocación de método estático correspondiente:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Un método de extensión `Ci.Mj` es ***válido*** si:

*  `Ci` es una clase no anidada no genérica
*  El nombre de `Mj` es el *identificador*
*  `Mj` es accesible y aplicable cuando se aplica a los argumentos como un método estático, como se muestra arriba
*  Existe una conversión implícita de identidad, referencia o conversión boxing de *expr* al tipo del primer parámetro de `Mj`.

La búsqueda de `C` continúa como sigue:

*  A partir de la declaración de espacio de nombres envolvente más cercana, continuando con cada declaración de espacio de nombres envolvente y finalizando con la unidad de compilación que lo contiene, se realizan sucesivos intentos para buscar un conjunto candidato de métodos de extensión:
   * Si el espacio de nombres o la unidad de compilación especificados contiene directamente declaraciones de tipos no genéricos `Ci` con métodos de extensión válidos `Mj`, el conjunto de estos métodos de extensión es el conjunto de candidatos.
   * Si los tipos `Ci` importados por *using_static_declarations* y se declaran directamente en los espacios de nombres importados por *using_namespace_directive*s en el espacio de nombres o la unidad de compilación especificados directamente contienen métodos de extensión válidos `Mj`, entonces el conjunto de estos métodos de extensión es el conjunto de candidatos.
*  Si no se encuentra ningún conjunto candidato en ninguna declaración de espacio de nombres envolvente o unidad de compilación, se produce un error en tiempo de compilación.
*  De lo contrario, se aplica la resolución de sobrecarga al conjunto de candidatos tal y como se describe en ([resolución de sobrecarga](expressions.md#overload-resolution)). Si no se encuentra ningún método mejor, se produce un error en tiempo de compilación.
*  `C` es el tipo en el que se declara el mejor método como método de extensión.

Al usar `C` como destino, la llamada al método se procesa entonces como una invocación de método estático ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Las reglas anteriores significan que los métodos de instancia tienen prioridad sobre los métodos de extensión, que los métodos de extensión disponibles en las declaraciones de espacios de nombres internos tienen prioridad sobre los métodos de extensión disponibles en las declaraciones de espacios de nombres exteriores y esa extensión los métodos declarados directamente en un espacio de nombres tienen prioridad sobre los métodos de extensión importados en el mismo espacio de nombres con una directiva de espacio de nombres using. Por ejemplo:
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

En el ejemplo, el método de `B` tiene prioridad sobre el primer método de extensión y el método de `C` tiene prioridad sobre ambos métodos de extensión.

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
```console
E.F(1)
D.G(2)
C.H(3)
```
`D.G` tiene prioridad sobre `C.G` y `E.F` tiene prioridad sobre `D.F` y `C.F`.

#### <a name="delegate-invocations"></a>Invocaciones de delegado

En el caso de una invocación de delegado, el *primary_expression* de *invocation_expression* debe ser un valor de *delegate_type*. Además, si se considera que el *delegate_type* es un miembro de función con la misma lista de parámetros que el *delegate_type*, el *delegate_type* debe ser aplicable ([miembro de función aplicable](expressions.md#applicable-function-member)) con respecto a la *argument_ lista* de *invocation_expression*.

El procesamiento en tiempo de ejecución de una invocación de delegado con el formato `D(A)`, donde `D` es un *primary_expression* de un *delegate_type* y `A` es un *argument_list*opcional, consta de los siguientes pasos:

*  se evalúa `D`. Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.
*  El valor de `D` se comprueba para ser válido. Si el valor de `D` es `null`, se produce una @no__t 2 y no se ejecuta ningún otro paso.
*  De lo contrario, `D` es una referencia a una instancia de delegado. Las invocaciones de miembros de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) se realizan en cada una de las entidades a las que se puede llamar en la lista de invocaciones del delegado. Para las entidades a las que se puede llamar que se componen de un método de instancia y de instancia, la instancia de para la invocación es la instancia contenida en la entidad a la que se puede

### <a name="element-access"></a>Acceso a elementos

Un *element_access* consta de un *primary_no_array_creation_expression*, seguido de un token "`[`", seguido de un *argument_list*, seguido de un token "`]`". *Argument_list* consta de uno o más *argumentos*, separados por comas.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

El *argument_list* de una *element_access* no puede contener los argumentos `ref` o `out`.

Un *element_access* está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) si al menos uno de los siguientes contiene:

* *Primary_no_array_creation_expression* tiene el tipo en tiempo de compilación `dynamic`.
* Al menos una expresión de *argument_list* tiene el tipo en tiempo de compilación `dynamic` y *primary_no_array_creation_expression* no tiene un tipo de matriz.

En este caso, el compilador clasifica *element_access* como un valor de tipo `dynamic`. A continuación, se aplican las siguientes reglas para determinar el significado de *element_access* en tiempo de ejecución, utilizando el tipo en tiempo de ejecución en lugar del tipo en tiempo de compilación de los de *primary_no_array_creation_expression* y *argument_list* expresiones que tienen el tipo en tiempo de compilación `dynamic`. Si el *primary_no_array_creation_expression* no tiene el tipo en tiempo de compilación `dynamic`, el acceso al elemento sufre una comprobación de tiempo de compilación limitada, como se describe en [comprobación en tiempo de compilación de la resolución de sobrecarga dinámica](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Si el *primary_no_array_creation_expression* de un *element_access* es un valor de *array_type*, *element_access* es un acceso de matriz ([acceso de matriz](expressions.md#array-access)). De lo contrario, *primary_no_array_creation_expression* debe ser una variable o un valor de una clase, estructura o tipo de interfaz que tenga uno o más miembros del indexador, en cuyo caso *element_access* es un acceso de indexador ([acceso del indexador](expressions.md#indexer-access)).

#### <a name="array-access"></a>Acceso a matriz

Para un acceso de matriz, el *primary_no_array_creation_expression* de *element_access* debe ser un valor de *array_type*. Además, el *argument_list* de acceso de una matriz no puede contener argumentos con nombre. El número de expresiones de *argument_list* debe ser el mismo que el rango de *array_type*y cada expresión debe ser de tipo `int`, `uint`, `long`, `ulong` o debe poder convertirse implícitamente a uno o varios de estos tipos.

El resultado de evaluar un acceso de matriz es una variable del tipo de elemento de la matriz, es decir, el elemento de matriz seleccionado por los valores de las expresiones de *argument_list*.

El procesamiento en tiempo de ejecución de un acceso de matriz del formulario `P[A]`, donde `P` es un *primary_no_array_creation_expression* de un *array_type* y `A` es un *argument_list*, consta de los siguientes pasos:

*  se evalúa `P`. Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.
*  Las expresiones de índice de *argument_list* se evalúan en orden, de izquierda a derecha. Después de la evaluación de cada expresión de índice, se realiza una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) en uno de los siguientes tipos: `int`, `uint`, `long`, @no__t 4. Se elige el primer tipo de esta lista para el que existe una conversión implícita. Por ejemplo, si la expresión de índice es de tipo `short`, se realiza una conversión implícita a `int`, ya que las conversiones implícitas de @no__t 2 a `int` y de `short` a `long` son posibles. Si la evaluación de una expresión de índice o de la conversión implícita subsiguiente produce una excepción, no se evalúan más expresiones de índice y no se ejecuta ningún paso más.
*  El valor de `P` se comprueba para ser válido. Si el valor de `P` es `null`, se produce una @no__t 2 y no se ejecuta ningún otro paso.
*  El valor de cada expresión de *argument_list* se comprueba con los límites reales de cada dimensión de la instancia de la matriz a la que hace referencia `P`. Si uno o más valores están fuera del intervalo, se produce una `System.IndexOutOfRangeException` y no se ejecuta ningún paso más.
*  Se calcula la ubicación del elemento de matriz proporcionado por las expresiones de índice y esta ubicación se convierte en el resultado del acceso a la matriz.

#### <a name="indexer-access"></a>Acceso a indizador

En el caso de un acceso de indexador, el *primary_no_array_creation_expression* de *element_access* debe ser una variable o valor de un tipo de clase, struct o interfaz, y este tipo debe implementar uno o varios indexadores aplicables con respecto al *argument_list* de *element_access*.

El procesamiento en tiempo de enlace de un acceso de indexador con el formato `P[A]`, donde `P` es un *primary_no_array_creation_expression* de un tipo de clase, struct o interfaz `T`, y `A` es un *argument_list*, consta de lo siguiente: pasos

*  Se crea el conjunto de indexadores proporcionado por `T`. El conjunto está formado por todos los indizadores declarados en `T` o un tipo base de `T` que no son declaraciones `override` y son accesibles en el contexto actual ([acceso a miembros](basic-concepts.md#member-access)).
*  El conjunto se reduce a los indizadores aplicables y no ocultos por otros indexadores. Las siguientes reglas se aplican a cada indizador `S.I` en el conjunto, donde `S` es el tipo en el que se declara el indizador `I`:
   * Si `I` no es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)), `I` se quita del conjunto.
   * Si `I` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)), todos los indexadores declarados en un tipo base de `S` se quitan del conjunto.
   * Si `I` es aplicable con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)) y `S` es un tipo de clase distinto de `object`, todos los indexadores declarados en una interfaz se quitan del conjunto.
*  Si el conjunto resultante de indizadores candidatos está vacío, no existe ningún indexador aplicable y se produce un error en tiempo de enlace.
*  El mejor indexador del conjunto de indizadores candidatos se identifica mediante las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution). Si no se puede identificar un único indexador mejor, el acceso del indexador es ambiguo y se produce un error en tiempo de enlace.
*  Las expresiones de índice de *argument_list* se evalúan en orden, de izquierda a derecha. El resultado de procesar el acceso del indexador es una expresión clasificada como un acceso de indexador. La expresión de acceso de indexador hace referencia al indizador determinado en el paso anterior y tiene una expresión de instancia asociada de `P` y una lista de argumentos asociada de `A`.

En función del contexto en el que se utiliza, un acceso de indexador provoca la invocación del *descriptor* de acceso get o del *descriptor* de acceso set del indexador. Si el acceso del indizador es el destino de una asignación, se invoca al *descriptor* de acceso set para asignar un nuevo valor ([asignación simple](expressions.md#simple-assignment)). En todos los demás casos, el *descriptor de acceso get* se invoca para obtener el valor actual ([valores de las expresiones](expressions.md#values-of-expressions)).

### <a name="this-access"></a>este acceso

Un *this_access* consta de la palabra reservada `this`.

```antlr
this_access
    : 'this'
    ;
```

Un *this_access* solo se permite en el *bloque* de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia. Tiene uno de los significados siguientes:

*  Cuando se usa `this` en un *primary_expression* dentro de un constructor de instancia de una clase, se clasifica como un valor. El tipo del valor es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la clase en la que se produce el uso y el valor es una referencia al objeto que se está construyendo.
*  Cuando se usa `this` en un *primary_expression* dentro de un método de instancia o un descriptor de acceso de instancia de una clase, se clasifica como un valor. El tipo del valor es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la clase en la que se produce el uso y el valor es una referencia al objeto para el que se invocó el método o el descriptor de acceso.
*  Cuando se usa `this` en un *primary_expression* dentro de un constructor de instancia de un struct, se clasifica como una variable. El tipo de la variable es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) del struct en el que se produce el uso y la variable representa el struct que se está construyendo. La variable `this` de un constructor de instancia de una estructura se comporta exactamente igual que un parámetro `out` del tipo struct, en concreto, esto significa que la variable debe estar asignada definitivamente en todas las rutas de acceso de ejecución del constructor de instancia.
*  Cuando se usa `this` en un *primary_expression* dentro de un método de instancia o un descriptor de acceso de instancia de un struct, se clasifica como una variable. El tipo de la variable es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) del struct en el que se produce el uso.
   * Si el método o el descriptor de acceso no es un iterador ([iteradores](classes.md#iterators)), la variable `this` representa la estructura para la que se invocó el método o descriptor de acceso, y se comporta exactamente igual que un parámetro `ref` del tipo struct.
   * Si el método o descriptor de acceso es un iterador, la variable `this` representa una copia del struct para el que se invocó el método o descriptor de acceso, y se comporta exactamente igual que un parámetro de valor del tipo de estructura.

El uso de `this` en un *primary_expression* en un contexto distinto de los enumerados anteriormente es un error en tiempo de compilación. En concreto, no es posible hacer referencia a `this` en un método estático, un descriptor de acceso de propiedad estática o en una *variable_initializer* de una declaración de campo.

### <a name="base-access"></a>Acceso base

Un *base_access* consta de la palabra reservada `base` seguido de un token "`.`" y un identificador o un *argument_list* entre corchetes:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

Un *base_access* se usa para tener acceso a los miembros de clase base que están ocultos por miembros con el mismo nombre en la clase o el struct actual. Un *base_access* solo se permite en el *bloque* de un constructor de instancia, un método de instancia o un descriptor de acceso de instancia. Cuando `base.I` se produce en una clase o struct, `I` debe indicar un miembro de la clase base de esa clase o estructura. Del mismo modo, cuando `base[E]` se produce en una clase, debe existir un indexador aplicable en la clase base.

En tiempo de enlace, las expresiones *base_access* de la forma `base.I` y `base[E]` se evalúan exactamente como si se hubieran escrito `((B)this).I` y `((B)this)[E]`, donde `B` es la clase base de la clase o estructura en la que se produce la construcción. Por lo tanto, `base.I` y `base[E]` se corresponden con `this.I` y `this[E]`, excepto que `this` se ve como una instancia de la clase base.

Cuando un *base_access* hace referencia a un miembro de función virtual (un método, una propiedad o un indizador), se cambia la determinación del miembro de función que se va a invocar en tiempo de ejecución ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). El miembro de función que se invoca se determina mediante la búsqueda de la implementación más derivada ([métodos virtuales](classes.md#virtual-methods)) del miembro de función con respecto a `B` (en lugar de con respecto al tipo en tiempo de ejecución de `this`, como sería habitual en un acceso no base). . Por lo tanto, dentro de un `override` de un miembro de función `virtual`, se puede usar una *base_access* para invocar la implementación heredada del miembro de función. Si el miembro de función al que hace referencia un *base_access* es abstracto, se produce un error en tiempo de enlace.

### <a name="postfix-increment-and-decrement-operators"></a>Operadores de incremento y decremento posfijos

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

El operando de una operación de incremento o decremento postfijo debe ser una expresión clasificada como una variable, un acceso de propiedad o un acceso de indexador. El resultado de la operación es un valor del mismo tipo que el operando.

Si *primary_expression* tiene el tipo en tiempo de compilación `dynamic`, el operador está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)), *post_increment_expression* o *post_decrement_expression* tiene el tipo en tiempo de compilación `dynamic`. y las siguientes reglas se aplican en tiempo de ejecución mediante el tipo en tiempo de ejecución de *primary_expression*.

Si el operando de una operación de incremento o decremento postfijo es una propiedad o un indexador, la propiedad o el indexador deben tener un descriptor de acceso `get` y `set`. Si no es así, se produce un error en tiempo de enlace.

La resolución de sobrecargas de operador unario ([resolución de sobrecarga de operadores unarios](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específica. Los operadores predefinidos `++` y `--` existen para los siguientes tipos: @no__t 2, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 y cualquier tipo de enumeración. Los operadores predefinidos `++` devuelven el valor generado agregando 1 al operando y los operadores `--` predefinidos devuelven el valor generado restando 1 del operando. En un contexto `checked`, si el resultado de esta suma o resta está fuera del intervalo del tipo de resultado y el tipo de resultado es un tipo entero o un tipo de enumeración, se produce una `System.OverflowException`.

El procesamiento en tiempo de ejecución de una operación de incremento o decremento postfijo con el formato `x++` o `x--` consta de los siguientes pasos:

*   Si `x` está clasificado como una variable:
    * `x` se evalúa para generar la variable.
    * Se guarda el valor de `x`.
    * El operador seleccionado se invoca con el valor guardado de `x` como argumento.
    * El valor devuelto por el operador se almacena en la ubicación proporcionada por la evaluación de `x`.
    * El valor guardado de `x` se convierte en el resultado de la operación.
*   Si `x` se clasifica como un acceso de propiedad o indizador:
    * La expresión de instancia (si `x` no es `static`) y la lista de argumentos (si `x` es un indexador) asociada a `x` se evalúan, y los resultados se usan en las siguientes invocaciones de descriptor de acceso `get` y `set`.
    * Se invoca al descriptor de acceso `get` de `x` y se guarda el valor devuelto.
    * El operador seleccionado se invoca con el valor guardado de `x` como argumento.
    * El descriptor de acceso `set` de `x` se invoca con el valor devuelto por el operador como su argumento `value`.
    * El valor guardado de `x` se convierte en el resultado de la operación.

Los operadores `++` y `--` también admiten la notación de prefijo ([operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)). Normalmente, el resultado de `x++` o `x--` es el valor de @no__t 2 antes de la operación, mientras que el resultado de `++x` o `--x` es el valor de `x` después de la operación. En cualquier caso, `x` tiene el mismo valor después de la operación.

Se puede invocar una implementación `operator ++` o `operator --` mediante la notación de sufijo o de prefijo. No es posible tener implementaciones de operador independientes para las dos notaciones.

### <a name="the-new-operator"></a>Operador new

El operador `new` se usa para crear nuevas instancias de tipos.

Hay tres formas de expresiones `new`:

*  Las expresiones de creación de objetos se utilizan para crear nuevas instancias de tipos de clase y tipos de valor.
*  Las expresiones de creación de matrices se utilizan para crear nuevas instancias de tipos de matriz.
*  Las expresiones de creación de delegados se utilizan para crear nuevas instancias de tipos de delegado.

El operador `new` implica la creación de una instancia de un tipo, pero no implica necesariamente la asignación dinámica de memoria. En concreto, las instancias de tipos de valor no requieren memoria adicional más allá de las variables en las que residen, y no se producen asignaciones dinámicas cuando `new` se usa para crear instancias de tipos de valor.

#### <a name="object-creation-expressions"></a>Expresiones de creación de objetos

Se usa un *object_creation_expression* para crear una nueva instancia de *class_type* o *value_type*.

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

El *tipo* de *object_creation_expression* debe ser *class_type*, *value_type* o *type_parameter*. El *tipo* no puede ser `abstract` *class_type*.

Solo se permite *argument_list* ([listas de argumentos](expressions.md#argument-lists)) opcionales si el *tipo* es *class_type* o *struct_type*.

Una expresión de creación de objeto puede omitir la lista de argumentos del constructor y los paréntesis de cierre especificados que incluye un inicializador de objeto o un inicializador de colección. Omitir la lista de argumentos del constructor y los paréntesis de inclusión equivale a especificar una lista de argumentos vacía.

El procesamiento de una expresión de creación de objetos que incluye un inicializador de objeto o un inicializador de colección consiste en procesar primero el constructor de instancia y después procesar las inicializaciones de miembro o elemento especificadas por el inicializador de objeto ([ Inicializadores de objeto](expressions.md#object-initializers)) o inicializadores de colección ([inicializadores de colección](expressions.md#collection-initializers)).

Si alguno de los argumentos del *argument_list* opcional tiene el tipo en tiempo de compilación `dynamic`, el *object_creation_expression* está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) y se aplican las siguientes reglas en tiempo de ejecución mediante el tiempo de ejecución. tipo de los argumentos de *argument_list* que tienen el tipo de tiempo de compilación `dynamic`. Sin embargo, la creación de objetos sufre una comprobación de tiempo de compilación limitada, como se describe en [comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

El procesamiento en tiempo de enlace de un *object_creation_expression* del formulario `new T(A)`, donde `T` es un *class_type* o un *value_type* y `A` es un *argument_list*opcional, consta de los siguientes pasos:

*   Si `T` es *value_type* y `A` no está presente:
    * *Object_creation_expression* es una invocación de constructor predeterminada. El resultado de *object_creation_expression* es un valor de tipo `T`, es decir, el valor predeterminado de `T` tal y como se define en [el tipo System. ValueType](types.md#the-systemvaluetype-type).
*   De lo contrario, si `T` es *type_parameter* y `A` no está presente:
    * Si no se ha especificado ninguna restricción de tipo de valor o restricción de constructor ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) para `T`, se produce un error en tiempo de enlace.
    * El resultado de *object_creation_expression* es un valor del tipo en tiempo de ejecución al que se ha enlazado el parámetro de tipo, es decir, el resultado de invocar el constructor predeterminado de ese tipo. El tipo en tiempo de ejecución puede ser un tipo de referencia o un tipo de valor.
*   De lo contrario, si `T` es *class_type* o *struct_type*:
    * Si `T` es un *class_type*`abstract`, se produce un error en tiempo de compilación.
    * El constructor de instancia que se va a invocar se determina mediante las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution). El conjunto de constructores de instancias candidatas se compone de todos los constructores de instancia accesibles declarados en `T`, que son aplicables con respecto a `A` ([miembro de función aplicable](expressions.md#applicable-function-member)). Si el conjunto de constructores de instancia candidata está vacío, o si no se puede identificar un único constructor de instancia mejor, se produce un error en tiempo de enlace.
    * El resultado de *object_creation_expression* es un valor de tipo `T`, es decir, el valor generado al invocar el constructor de instancia determinado en el paso anterior.
*  De lo contrario, el *object_creation_expression* no es válido y se produce un error en tiempo de enlace.

Aunque el *object_creation_expression* esté enlazado dinámicamente, el tipo en tiempo de compilación sigue siendo `T`.

El procesamiento en tiempo de ejecución de un *object_creation_expression* con el formato `new T(A)`, donde `T` es *class_type* o *struct_type* y `A` es un *argument_list*opcional, consta de los siguientes pasos:

*   Si `T` es *class_type*:
    * Se asigna una nueva instancia de la clase `T`. Si no hay suficiente memoria disponible para asignar la nueva instancia, se produce una `System.OutOfMemoryException` y no se ejecuta ningún otro paso.
    * Todos los campos de la nueva instancia se inicializan con sus valores predeterminados ([valores predeterminados](variables.md#default-values)).
    * El constructor de instancia se invoca de acuerdo con las reglas de invocación de miembros de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Se pasa automáticamente una referencia a la instancia recién asignada al constructor de instancia y se puede tener acceso a la instancia desde dentro de ese constructor como `this`.
*   Si `T` es *struct_type*:
    * Se crea una instancia de tipo `T` mediante la asignación de una variable local temporal. Dado que se requiere un constructor de instancia de un *struct_type* para asignar definitivamente un valor a cada campo de la instancia que se va a crear, no es necesaria la inicialización de la variable temporal.
    * El constructor de instancia se invoca de acuerdo con las reglas de invocación de miembros de función ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Se pasa automáticamente una referencia a la instancia recién asignada al constructor de instancia y se puede tener acceso a la instancia desde dentro de ese constructor como `this`.

#### <a name="object-initializers"></a>Inicializadores de objeto

Un ***inicializador de objeto*** especifica valores para cero o más campos, propiedades o elementos indizados de un objeto.

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

Un inicializador de objeto consta de una secuencia de inicializadores de miembro, delimitada por los tokens `{` y `}` y separados por comas. Cada *member_initializer* designa un destino para la inicialización. Un *identificador* debe nombrar un campo o propiedad accesible del objeto que se va a inicializar, mientras que un *argument_list* entre corchetes debe especificar los argumentos de un indizador accesible en el objeto que se va a inicializar. Es un error que un inicializador de objeto incluya más de un inicializador de miembro para el mismo campo o propiedad.

Cada *initializer_target* va seguido de un signo igual y una expresión, un inicializador de objeto o un inicializador de colección. No es posible que las expresiones del inicializador de objeto hagan referencia al objeto recién creado que se está inicializando.

Inicializador de miembro que especifica una expresión después de que el signo igual se procese de la misma manera que una asignación ([asignación simple](expressions.md#simple-assignment)) al destino.

Inicializador de miembro que especifica un inicializador de objeto después de que el signo igual sea un ***inicializador de objeto anidado***, es decir, una inicialización de un objeto incrustado. En lugar de asignar un nuevo valor al campo o propiedad, las asignaciones en el inicializador de objeto anidado se tratan como asignaciones a los miembros del campo o de la propiedad. Los inicializadores de objeto anidados no se pueden aplicar a las propiedades con un tipo de valor o a los campos de solo lectura con un tipo de valor.

Inicializador de miembro que especifica un inicializador de colección después de que el signo igual sea una inicialización de una colección incrustada. En lugar de asignar una nueva colección al campo de destino, propiedad o indizador, los elementos proporcionados en el inicializador se agregan a la colección a la que hace referencia el destino. El destino debe ser de un tipo de colección que satisfaga los requisitos especificados en los [inicializadores de colección](expressions.md#collection-initializers).

Los argumentos de un inicializador de índice siempre se evaluarán exactamente una vez. Por lo tanto, aunque los argumentos acaben nunca en usarse (por ejemplo, debido a un inicializador anidado vacío), se evaluarán por sus efectos secundarios.

La clase siguiente representa un punto con dos coordenadas:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Se puede crear una instancia de `Point` e inicializarla de la siguiente manera:
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
donde `__a` es una variable temporal invisible e inaccesible en caso contrario. La clase siguiente representa un rectángulo creado a partir de dos puntos:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Se puede crear una instancia de `Rectangle` e inicializarla de la siguiente manera:
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
donde `__r`, `__p1` y `__p2` son variables temporales que, de lo contrario, son invisibles e inaccesibles.

Si el constructor de `Rectangle` asigna las dos instancias de `Point` incrustadas
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
la construcción siguiente se puede utilizar para inicializar las instancias de `Point` incrustadas en lugar de asignar nuevas instancias:
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

Dada una definición adecuada de C, el ejemplo siguiente:
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
donde `__c`, etc., son variables generadas que son invisibles e inaccesibles para el código fuente. Tenga en cuenta que los argumentos de `[0,0]` solo se evalúan una vez, y los argumentos de `[1,2]` se evalúan una vez aunque nunca se usan.

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

Un inicializador de colección consta de una secuencia de inicializadores de elemento, delimitada por los tokens `{` y `}` y separados por comas. Cada inicializador de elemento especifica un elemento que se va a agregar al objeto de colección que se va a inicializar y consta de una lista de expresiones delimitadas por los tokens `{` y `}` y separados por comas.  Un inicializador de elemento de una sola expresión se puede escribir sin llaves, pero no puede ser una expresión de asignación para evitar la ambigüedad con los inicializadores de miembro. La producción de *non_assignment_expression* se define en la [expresión](expressions.md#expression).

A continuación se muestra un ejemplo de una expresión de creación de objeto que incluye un inicializador de colección:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

El objeto de colección al que se aplica un inicializador de colección debe ser de un tipo que implementa `System.Collections.IEnumerable` o se produce un error en tiempo de compilación. Para cada elemento especificado en orden, el inicializador de colección invoca un método `Add` en el objeto de destino con la lista de expresiones del inicializador de elemento como lista de argumentos, aplicando la búsqueda de miembros normal y la resolución de sobrecarga para cada invocación. Por lo tanto, el objeto de colección debe tener una instancia o un método de extensión aplicable con el nombre `Add` para cada inicializador de elemento.

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

Se puede crear un `List<Contact>` e inicializarlo como se indica a continuación:
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
donde `__clist`, `__c1` y `__c2` son variables temporales que, de lo contrario, son invisibles e inaccesibles.

#### <a name="array-creation-expressions"></a>Expresiones de creación de matrices

Se usa un *array_creation_expression* para crear una nueva instancia de un *array_type*.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Una expresión de creación de matriz del primer formulario asigna una instancia de matriz del tipo resultante de la eliminación de cada una de las expresiones individuales de la lista de expresiones. Por ejemplo, la expresión de creación de matriz `new int[10,20]` genera una instancia de matriz de tipo `int[,]` y la expresión de creación de matriz `new int[10][,]` genera una matriz de tipo `int[][,]`. Cada expresión de la lista de expresiones debe ser de tipo `int`, `uint`, `long` o `ulong`, o bien se puede convertir implícitamente a uno o varios de estos tipos. El valor de cada expresión determina la longitud de la dimensión correspondiente en la instancia de matriz recién asignada. Dado que la longitud de una dimensión de matriz no debe ser negativa, es un error en tiempo de compilación que tiene un *constant_expression* con un valor negativo en la lista de expresiones.

Excepto en un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)), no se especifica el diseño de las matrices.

Si una expresión de creación de matriz del primer formulario incluye un inicializador de matriz, cada expresión de la lista de expresiones debe ser una constante y las longitudes de rango y dimensión especificadas por la lista de expresiones deben coincidir con las del inicializador de matriz.

En una expresión de creación de matriz del segundo o tercer formulario, el rango del tipo de matriz o especificador de rango especificado debe coincidir con el del inicializador de matriz. Las longitudes de las dimensiones individuales se deducen del número de elementos de cada uno de los niveles de anidamiento correspondientes del inicializador de matriz. Por lo tanto, la expresión
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
corresponde exactamente a
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Se hace referencia a una expresión de creación de matriz del tercer formulario como una ***expresión de creación de matriz con tipo implícito***. Es similar a la segunda forma, salvo que el tipo de elemento de la matriz no se proporciona explícitamente, pero se determina como el mejor tipo común ([encontrar el mejor tipo común de un conjunto de expresiones](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) del conjunto de expresiones en el inicializador de matriz. En el caso de una matriz multidimensional, es decir, una donde la *rank_specifier* contiene al menos una coma, este conjunto consta de todas las *expresiones*que se encuentran en los *array_initializer*anidados.

Los inicializadores de matriz se describen con más detalle en [inicializadores de matriz](arrays.md#array-initializers).

El resultado de evaluar una expresión de creación de matriz se clasifica como un valor, es decir, una referencia a la instancia de la matriz recién asignada. El procesamiento en tiempo de ejecución de una expresión de creación de matriz consta de los siguientes pasos:

*  Las expresiones de longitud de dimensión del *expression_list* se evalúan en orden, de izquierda a derecha. Después de la evaluación de cada expresión, se realiza una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) en uno de los siguientes tipos: `int`, `uint`, `long`, @no__t 4. Se elige el primer tipo de esta lista para el que existe una conversión implícita. Si la evaluación de una expresión o la conversión implícita subsiguiente produce una excepción, no se evalúan más expresiones y no se ejecuta ningún otro paso.
*  Los valores calculados de las longitudes de dimensión se validan como se indica a continuación. Si uno o varios de los valores son menores que cero, se produce una `System.OverflowException` y no se ejecuta ningún otro paso.
*  Se asigna una instancia de matriz con las longitudes de dimensión dadas. Si no hay suficiente memoria disponible para asignar la nueva instancia, se produce una `System.OutOfMemoryException` y no se ejecuta ningún otro paso.
*  Todos los elementos de la nueva instancia de matriz se inicializan con sus valores predeterminados ([valores predeterminados](variables.md#default-values)).
*  Si la expresión de creación de matriz contiene un inicializador de matriz, cada expresión del inicializador de matriz se evalúa y se asigna a su elemento de matriz correspondiente. Las evaluaciones y las asignaciones se realizan en el orden en el que se escriben las expresiones en el inicializador de matriz; es decir, los elementos se inicializan en el orden de índice creciente, con la dimensión situada más a la derecha aumentando primero. Si la evaluación de una expresión determinada o la asignación subsiguiente al elemento de matriz correspondiente produce una excepción, no se inicializa ningún otro elemento (y los demás elementos tendrán sus valores predeterminados).

Una expresión de creación de matriz permite la creación de instancias de una matriz con elementos de un tipo de matriz, pero los elementos de dicha matriz se deben inicializar manualmente. Por ejemplo, la instrucción
```csharp
int[][] a = new int[100][];
```
crea una matriz unidimensional con 100 elementos de tipo `int[]`. El valor inicial de cada elemento es `null`. No es posible que la misma expresión de creación de matriz cree también instancias de las submatrices y la instrucción
```csharp
int[][] a = new int[100][5];        // Error
```
produce un error en tiempo de compilación. En su lugar, la creación de instancias de las submatrices debe realizarse manualmente, como en
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Cuando una matriz de matrices tiene una forma "rectangular", es decir, cuando las submatrices tienen la misma longitud, es más eficaz usar una matriz multidimensional. En el ejemplo anterior, la creación de instancias de la matriz de matrices crea 101 objetos, una matriz externa y submatrices de 100. En cambio,
```csharp
int[,] = new int[100, 5];
```
crea un solo objeto, una matriz bidimensional y realiza la asignación en una única instrucción.

A continuación se muestran ejemplos de expresiones de creación de matrices con tipo implícito:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

La última expresión genera un error en tiempo de compilación porque ni `int` ni `string` se convierte implícitamente en el otro, por lo que no hay ningún tipo común mejor. En este caso, se debe usar una expresión de creación de matriz con tipo explícito, por ejemplo, especificando el tipo que se va a `object[]`. Como alternativa, uno de los elementos se puede convertir a un tipo base común, que luego se convertiría en el tipo de elemento deducido.

Las expresiones de creación de matrices con tipo implícito se pueden combinar con inicializadores de objeto anónimos ([expresiones de creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions)) para crear estructuras de datos con tipos anónimos. Por ejemplo:
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

#### <a name="delegate-creation-expressions"></a>Expresiones de creación de delegados

Se usa un *delegate_creation_expression* para crear una nueva instancia de *delegate_type*.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

El argumento de una expresión de creación de delegado debe ser un grupo de métodos, una función anónima o un valor del tipo en tiempo de compilación `dynamic` o *delegate_type*. Si el argumento es un grupo de métodos, identifica el método y, para un método de instancia, el objeto para el que se va a crear un delegado. Si el argumento es una función anónima, define directamente los parámetros y el cuerpo del método del destino del delegado. Si el argumento es un valor, identifica una instancia de delegado de la que se va a crear una copia.

Si la *expresión* tiene el tipo en tiempo de compilación `dynamic`, el *delegate_creation_expression* está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)) y las reglas siguientes se aplican en tiempo de ejecución mediante el tipo en tiempo de ejecución de la *expresión*. De lo contrario, las reglas se aplican en tiempo de compilación.

El procesamiento en tiempo de enlace de un *delegate_creation_expression* del formulario `new D(E)`, donde `D` es un *delegate_type* y @no__t 4 es una *expresión*, consta de los siguientes pasos:

*  Si `E` es un grupo de métodos, la expresión de creación de delegado se procesa de la misma manera que una conversión de grupo de métodos ([conversiones de grupo de métodos](conversions.md#method-group-conversions)) de `E` a `D`.
*  Si `E` es una función anónima, la expresión de creación de delegado se procesa de la misma manera que una conversión de función anónima ([conversiones de funciones anónimas](conversions.md#anonymous-function-conversions)) de `E` a `D`.
*  Si `E` es un valor, `E` debe ser compatible ([declaraciones de delegado](delegates.md#delegate-declarations)) con `D` y el resultado es una referencia a un delegado recién creado de tipo `D` que hace referencia a la misma lista de invocación que `E`. Si `E` no es compatible con `D`, se produce un error en tiempo de compilación.

El procesamiento en tiempo de ejecución de un *delegate_creation_expression* con el formato `new D(E)`, donde `D` es un *delegate_type* y @no__t 4 es una *expresión*, consta de los siguientes pasos:

*   Si `E` es un grupo de métodos, la expresión de creación de delegados se evalúa como una conversión de grupo de métodos ([conversiones de grupo de métodos](conversions.md#method-group-conversions)) de `E` a `D`.
*   Si `E` es una función anónima, la creación del delegado se evalúa como una conversión de función anónima de `E` a `D` ([conversiones de funciones anónimas](conversions.md#anonymous-function-conversions)).
*   Si `E` es un valor de *delegate_type*:
    * se evalúa `E`. Si esta evaluación provoca una excepción, no se ejecuta ningún paso más.
    * Si el valor de `E` es `null`, se produce una @no__t 2 y no se ejecuta ningún otro paso.
    * Se asigna una nueva instancia del tipo de delegado `D`. Si no hay suficiente memoria disponible para asignar la nueva instancia, se produce una `System.OutOfMemoryException` y no se ejecuta ningún otro paso.
    * La nueva instancia de delegado se inicializa con la misma lista de invocación que la instancia de delegado proporcionada por `E`.

La lista de invocaciones de un delegado se determina cuando se crea una instancia del delegado y permanece constante durante toda la duración del delegado. En otras palabras, no es posible cambiar las entidades a las que se puede llamar de destino de un delegado una vez que se ha creado. Cuando se combinan dos delegados o uno se quita de otro ([declaraciones de delegado](delegates.md#delegate-declarations)), se genera un nuevo delegado. no se ha cambiado el contenido de ningún delegado existente.

No es posible crear un delegado que haga referencia a una propiedad, un indexador, un operador definido por el usuario, un constructor de instancia, un destructor o un constructor estático.

Como se describió anteriormente, cuando se crea un delegado a partir de un grupo de métodos, la lista de parámetros formales y el tipo de valor devuelto del delegado determinan cuál de los métodos sobrecargados se deben seleccionar. En el ejemplo
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
el campo `A.f` se inicializa con un delegado que hace referencia al segundo método `Square` porque ese método coincide exactamente con la lista de parámetros formales y el tipo de valor devuelto de `DoubleFunc`. Si el segundo método `Square` no estuviera presente, se habría producido un error en tiempo de compilación.

#### <a name="anonymous-object-creation-expressions"></a>Expresiones de creación de objetos anónimos

Se usa un *anonymous_object_creation_expression* para crear un objeto de un tipo anónimo.

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

Un inicializador de objeto anónimo declara un tipo anónimo y devuelve una instancia de ese tipo. Un tipo anónimo es un tipo de clase sin nombre que hereda directamente de `object`. Los miembros de un tipo anónimo son una secuencia de propiedades de solo lectura que se deducen del inicializador de objeto anónimo utilizado para crear una instancia del tipo. En concreto, un inicializador de objeto anónimo con el formato
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
donde cada `Tx` es el tipo de la expresión correspondiente `ex`. La expresión usada en *member_declarator* debe tener un tipo. Por lo tanto, se trata de un error en tiempo de compilación para que una expresión de *member_declarator* sea NULL o una función anónima. También es un error en tiempo de compilación para que la expresión tenga un tipo no seguro.

El compilador genera automáticamente los nombres de un tipo anónimo y del parámetro para su método `Equals` y no se puede hacer referencia a ellos en el texto del programa.

En el mismo programa, dos inicializadores de objeto anónimo que especifican una secuencia de propiedades de los mismos nombres y tipos en tiempo de compilación en el mismo orden generarán instancias del mismo tipo anónimo.

En el ejemplo
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
la asignación en la última línea se permite porque `p1` y `p2` son del mismo tipo anónimo.

Los métodos `Equals` y `GetHashcode` en los tipos anónimos invalidan los métodos heredados de `object` y se definen en términos de `Equals` y `GetHashcode` de las propiedades, de modo que dos instancias del mismo tipo anónimo son iguales si y solo si todas sus propiedades son sea.

Un declarador de miembro se puede abreviar como un nombre simple ([inferencia de tipos](expressions.md#type-inference)), un acceso a miembro ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), un acceso base ([acceso base](expressions.md#base-access)) o un acceso a miembro condicional nulo ([ Expresiones condicionales NULL como inicializadores de proyección](expressions.md#null-conditional-expressions-as-projection-initializers)). Esto se denomina ***inicializador de proyección*** y es la abreviatura de una declaración de y la asignación a una propiedad con el mismo nombre. En concreto, los declaradores de miembros de los formularios
```csharp
identifier
expr.identifier
```
son exactamente equivalentes a lo siguiente, respectivamente:
```csharp
identifier = identifier
identifier = expr.identifier
```

Por lo tanto, en un inicializador de proyección, el *identificador* selecciona tanto el valor como el campo o la propiedad a los que se asigna el valor. De manera intuitiva, un inicializador de proyección proyecta no solo un valor, sino también el nombre del valor.

### <a name="the-typeof-operator"></a>El operador typeof

El operador `typeof` se usa para obtener el objeto `System.Type` para un tipo.

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

La primera forma de *typeof_expression* consta de una palabra clave `typeof` seguida de un *tipo*entre paréntesis. El resultado de una expresión de este formulario es el objeto `System.Type` para el tipo indicado. Solo hay un objeto `System.Type` para cualquier tipo dado. Esto significa que para un tipo @ no__t-0, `typeof(T) == typeof(T)` siempre es true. El *tipo* no puede ser `dynamic`.

La segunda forma de *typeof_expression* consta de una palabra clave `typeof` seguida de un *unbound_type_name*entre paréntesis. Un *unbound_type_name* es muy similar a un *type_name* ([espacio de nombres y nombres de tipo](basic-concepts.md#namespace-and-type-names)), salvo que un *unbound_type_name* contiene *generic_dimension_specifier*s donde *type_name* contiene *tipo argument_list*s. Cuando el operando de una *typeof_expression* es una secuencia de tokens que satisface las gramáticas de *unbound_type_name* y *type_name*, es decir, cuando no contiene ningún *generic_dimension_specifier* ni *type_argument _list*, la secuencia de tokens se considera un *type_name*. El significado de un *unbound_type_name* se determina de la siguiente manera:

*  Convierta la secuencia de tokens en un *type_name* reemplazando cada *generic_dimension_specifier* por un *type_argument_list* que tenga el mismo número de comas y la palabra clave `object` como cada *type_argument*.
*  Evalúe el *type_name*resultante, pasando por alto todas las restricciones de parámetro de tipo.
*  *Unbound_type_name* se resuelve en el tipo genérico sin enlazar asociado con el tipo construido resultante ([tipos enlazados y sin enlazar](types.md#bound-and-unbound-types)).

El resultado de *typeof_expression* es el objeto `System.Type` para el tipo genérico sin enlazar resultante.

La tercera forma de *typeof_expression* consta de una palabra clave `typeof` seguida de una palabra clave `void` entre paréntesis. El resultado de una expresión de este formulario es el objeto `System.Type` que representa la ausencia de un tipo. El objeto de tipo devuelto por `typeof(void)` es distinto del objeto de tipo devuelto para cualquier tipo. Este objeto de tipo especial es útil en las bibliotecas de clases que permiten la reflexión en métodos del lenguaje, donde esos métodos desean tener una manera de representar el tipo de valor devuelto de cualquier método, incluidos los métodos void, con una instancia de `System.Type`.

El operador `typeof` se puede usar en un parámetro de tipo. El resultado es el objeto `System.Type` para el tipo en tiempo de ejecución que se enlazó al parámetro de tipo. El operador `typeof` también se puede usar en un tipo construido o en un tipo genérico sin enlazar ([tipos enlazados y sin enlazar](types.md#bound-and-unbound-types)). El objeto `System.Type` para un tipo genérico sin enlazar no es el mismo que el objeto `System.Type` del tipo de instancia. El tipo de instancia siempre es un tipo construido cerrado en tiempo de ejecución, por lo que su objeto `System.Type` depende de los argumentos de tipo en tiempo de ejecución en uso, mientras que el tipo genérico sin enlazar no tiene ningún argumento de tipo.

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

Tenga en cuenta que `int` y `System.Int32` son del mismo tipo.

Tenga en cuenta también que el resultado de `typeof(X<>)` no depende del argumento de tipo, pero el resultado de `typeof(X<T>)` sí lo hace.

### <a name="the-checked-and-unchecked-operators"></a>Operadores checked y unchecked

Los operadores `checked` y `unchecked` se utilizan para controlar el ***contexto de comprobación de desbordamiento*** para conversiones y operaciones aritméticas de tipo entero.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

El operador `checked` evalúa la expresión contenida en un contexto comprobado y el operador `unchecked` evalúa la expresión contenida en un contexto no comprobado. Un *checked_expression* o *unchecked_expression* corresponde exactamente a un *parenthesized_expression* ([expresiones entre paréntesis](expressions.md#parenthesized-expressions)), salvo que la expresión contenida se evalúa en el contexto de comprobación de desbordamiento determinado. .

El contexto de comprobación de desbordamiento también se puede controlar a través de las instrucciones `checked` y `unchecked` ([las instrucciones checked y unchecked](statements.md#the-checked-and-unchecked-statements)).

Las siguientes operaciones se ven afectadas por el contexto de comprobación de desbordamiento establecido por los operadores y las instrucciones `checked` y `unchecked`:

*  Los operadores unarios predefinidos `++` y `--` (operadores de[incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) [postfijo y operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)) cuando el operando es de un tipo entero.
*  El operador unario `-` predefinido ([operador unario menos](expressions.md#unary-minus-operator)), cuando el operando es de un tipo entero.
*  Los operadores binarios `+`, `-`, `*` y `/` predefinidos ([operadores aritméticos](expressions.md#arithmetic-operators)), cuando ambos operandos son de tipos enteros.
*  Conversiones numéricas explícitas ([Conversiones numéricas explícitas](conversions.md#explicit-numeric-conversions)) de un tipo entero a otro tipo entero, o de `float` o `double` a un tipo entero.

Cuando una de las operaciones anteriores produce un resultado que es demasiado grande para representarlo en el tipo de destino, el contexto en el que se realiza la operación controla el comportamiento resultante:

*  En un contexto `checked`, si la operación es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)), se produce un error en tiempo de compilación. De lo contrario, cuando la operación se realiza en tiempo de ejecución, se produce una `System.OverflowException`.
*  En un contexto `unchecked`, el resultado se trunca descartando los bits de orden superior que no caben en el tipo de destino.

En el caso de expresiones no constantes (expresiones que se evalúan en tiempo de ejecución) que no están incluidas en ningún operador o instrucción `checked` o `unchecked`, el contexto de comprobación de desbordamiento predeterminado es `unchecked` a menos que se produzcan factores externos (como modificadores de compilador y configuración del entorno de ejecución) llamada para la evaluación de `checked`.

En el caso de expresiones constantes (expresiones que se pueden evaluar completamente en tiempo de compilación), el contexto de comprobación de desbordamiento predeterminado siempre es `checked`. A menos que una expresión constante se coloque explícitamente en un contexto `unchecked`, los desbordamientos que se producen durante la evaluación de la expresión en tiempo de compilación siempre producen errores en tiempo de compilación.

El cuerpo de una función anónima no se ve afectado por los contextos `checked` o `unchecked` en los que se produce la función anónima.

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
no se detectan errores en tiempo de compilación, ya que ninguna de las expresiones se puede evaluar en tiempo de compilación. En tiempo de ejecución, el método `F` produce una `System.OverflowException` y el método `G` devuelve-727379968 (los 32 bits inferiores del resultado fuera del intervalo). El comportamiento del método `H` depende del contexto de comprobación de desbordamiento predeterminado para la compilación, pero es igual que `F` o igual que `G`.

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
los desbordamientos que se producen al evaluar las expresiones constantes en `F` y `H` provocan que se notifiquen los errores en tiempo de compilación porque las expresiones se evalúan en un contexto de @no__t 2. También se produce un desbordamiento al evaluar la expresión constante en `G`, pero como la evaluación tiene lugar en un contexto `unchecked`, el desbordamiento no se registra.

Los operadores `checked` y `unchecked` solo afectan al contexto de comprobación de desbordamiento para las operaciones que están contenidas textualmente en los tokens "@no__t 2" y "`)`". Los operadores no tienen ningún efecto en los miembros de función que se invocan como resultado de evaluar la expresión contenida. En el ejemplo
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
el uso de `checked` en `F` no afecta a la evaluación de @no__t 2 en `Multiply`, por lo que `x * y` se evalúa en el contexto de comprobación de desbordamiento predeterminado.

El operador `unchecked` es práctico al escribir constantes de los tipos enteros con signo en notación hexadecimal. Por ejemplo:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Las dos constantes hexadecimales anteriores son del tipo `uint`. Dado que las constantes están fuera del intervalo `int`, sin el operador `unchecked`, las conversiones a `int` generarán errores en tiempo de compilación.

Los operadores e instrucciones `checked` y `unchecked` permiten a los programadores controlar determinados aspectos de algunos cálculos numéricos. Sin embargo, el comportamiento de algunos operadores numéricos depende de los tipos de datos de los operandos. Por ejemplo, si se multiplican dos decimales, siempre se produce una excepción en el desbordamiento incluso dentro de una construcción `unchecked` explícitamente. Del mismo modo, si se multiplican dos floats, nunca se produce una excepción en el desbordamiento incluso dentro de una construcción `checked` explícitamente. Además, otros operadores nunca se ven afectados por el modo de comprobación, ya sea de forma predeterminada o explícita.

### <a name="default-value-expressions"></a>Expresiones de valor predeterminado

Una expresión de valor predeterminado se usa para obtener el valor predeterminado ([valores predeterminados](variables.md#default-values)) de un tipo. Normalmente, se usa una expresión de valor predeterminado para los parámetros de tipo, ya que es posible que no se conozca si el parámetro de tipo es un tipo de valor o un tipo de referencia. (No existe ninguna conversión del literal `null` a un parámetro de tipo a menos que se sepa que el parámetro de tipo es un tipo de referencia).

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Si el *tipo* de un *default_value_expression* se evalúa en tiempo de ejecución en un tipo de referencia, el resultado es `null` convertido en ese tipo. Si el *tipo* de un *default_value_expression* se evalúa en tiempo de ejecución en un tipo de valor, el resultado es el valor predeterminado de *Value_type*([constructores predeterminados](types.md#default-constructors)).

Un *default_value_expression* es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)) si el tipo es un tipo de referencia o un parámetro de tipo que se sabe que es un tipo de referencia ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)). Además, un *default_value_expression* es una expresión constante si el tipo es uno de los siguientes tipos de valor: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3 o cualquier tipo de enumeración.


### <a name="nameof-expressions"></a>Expresiones Name

Un *nameof_expression* se usa para obtener el nombre de una entidad de programa como una cadena de constante.

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

En términos gramaticales, el operando *named_entity* siempre es una expresión. Dado que `nameof` no es una palabra clave reservada, una expresión name es siempre sintácticamente ambigua con una invocación del nombre simple `nameof`. Por motivos de compatibilidad, si la búsqueda de nombres ([nombres simples](expressions.md#simple-names)) del nombre `nameof` se realiza correctamente, la expresión se trata como un *invocation_expression* , independientemente de si la invocación es legal. En caso contrario, es *nameof_expression*.

El significado de *named_entity* de un *nameof_expression* es el significado de este como una expresión; es decir, como *simple_name*, *base_access* o *member_access*. Sin embargo, en los casos en los que la búsqueda descrita en [nombres simples](expressions.md#simple-names) y [acceso a miembros](expressions.md#member-access) produce un error porque se encontró un miembro de instancia en un contexto estático, *nameof_expression* no genera este error.

Se trata de un error en tiempo de compilación para un *named_entity* que designa un grupo de métodos para que tenga un *type_argument_list*. Es un error en tiempo de compilación para que un *named_entity_target* tenga el tipo `dynamic`.

Un *nameof_expression* es una expresión constante de tipo `string` y no tiene ningún efecto en tiempo de ejecución. En concreto, su *named_entity* no se evalúa y se omite para los fines del análisis de asignación definitiva ([reglas generales para expresiones simples](variables.md#general-rules-for-simple-expressions)). Su valor es el último identificador de *named_entity* antes de la *type_argument_list*final opcional, transformada de la siguiente manera:

* El prefijo`@`"", si se utiliza, se quita.
* Cada *unicode_escape_sequence* se transforma en el carácter Unicode correspondiente.
* Se quitan todos los *formatting_characters* .

Se trata de las mismas transformaciones aplicadas en los [identificadores](lexical-structure.md#identifiers) al probar la igualdad entre los identificadores.

TODO: ejemplos

### <a name="anonymous-method-expressions"></a>Expresiones de método anónimo

Un *anonymous_method_expression* es una de las dos formas de definir una función anónima. Se describen con más detalle en [expresiones de función anónima](expressions.md#anonymous-function-expressions).

## <a name="unary-operators"></a>Operadores unarios

Los operadores `?`, `+`, `-`, `!`, `~`, `++`, `--`, CAST y `await` se denominan operadores unarios.

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

Si el operando de un *unary_expression* tiene el tipo en tiempo de compilación `dynamic`, está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso, el tipo en tiempo de compilación de *unary_expression* es `dynamic` y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución del operando.

### <a name="null-conditional-operator"></a>Operador condicional null

El operador condicional null aplica una lista de operaciones a su operando solo si ese operando no es NULL. De lo contrario, el resultado de aplicar el operador es `null`.

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

La lista de operaciones puede incluir operaciones de acceso a miembros y acceso a elementos (que pueden ser a su vez valores condicionales null), así como invocación.

Por ejemplo, la expresión `a.b?[0]?.c()` es un *null_conditional_expression* con un *primary_expression* `a.b` y *null_conditional_operations* `?[0]` (acceso de elemento condicional null), `?.c` (miembro condicional null acceso) y `()` (Invocación).

En el caso de un *null_conditional_expression* `E` con *primary_expression* `P`, permita que `E0` sea la expresión obtenida quitando textualmente el @no__t 5 inicial de cada una de las *null_conditional_operations* de `E` que tener uno. Conceptualmente, `E0` es la expresión que se evaluará si ninguna de las comprobaciones nulas representadas por el `?`s encuentra un `null`.

Además, deje que `E1` sea la expresión obtenida quitando textualmente el @no__t inicial 1 de la primera de las *null_conditional_operations* en `E`. Esto puede conducir a una *expresión principal* (si solo había un `?`) o a otro *null_conditional_expression*.

Por ejemplo, si `E` es la expresión `a.b?[0]?.c()`, `E0` es la expresión `a.b[0].c()` y `E1` es la expresión `a.b[0]?.c()`.

Si `E0` se clasifica como Nothing, `E` se clasifica como Nothing. De lo contrario, E se clasifica como un valor.

`E0` y `E1` se usan para determinar el significado de `E`:

*  Si `E` se produce como *statement_expression* , el significado de `E` es el mismo que el de la instrucción.

   ```csharp
   if ((object)P != null) E1;
   ```

   excepto que P se evalúa solo una vez.

*  De lo contrario, si `E0` se clasifica como Nothing, se produce un error en tiempo de compilación.

*  De lo contrario, deje que `T0` sea el tipo de `E0`.

   *  Si `T0` es un parámetro de tipo que no se sabe que es un tipo de referencia o un tipo de valor que no acepta valores NULL, se produce un error en tiempo de compilación.

   *  Si `T0` es un tipo de valor que no acepta valores NULL, el tipo de `E` es `T0?` y el significado de `E` es el mismo que

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      salvo que `P` solo se evalúa una vez.

   *  De lo contrario, el tipo de E es T0 y el significado de E es el mismo que

      ```csharp
      ((object)P == null) ? null : E1
      ```

      salvo que `P` solo se evalúa una vez.

Si `E1` es en sí mismo un *null_conditional_expression*, estas reglas se aplican de nuevo, anidando las pruebas para `null` hasta que no haya más `?`, y la expresión se ha reducido hasta la expresión principal `E0`.

Por ejemplo, si la expresión `a.b?[0]?.c()` se produce como una expresión de instrucción, como en la instrucción:
```csharp
a.b?[0]?.c();
```
su significado es equivalente a:
```csharp
if (a.b != null) a.b[0]?.c();
```
lo que es equivalente a:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
Salvo que `a.b` y `a.b[0]` solo se evalúan una vez.

Si se produce en un contexto donde se usa su valor, como en:
```csharp
var x = a.b?[0]?.c();
```
y suponiendo que el tipo de la invocación final no es un tipo de valor que no acepta valores NULL, su significado es equivalente a:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
salvo que `a.b` y `a.b[0]` solo se evalúan una vez.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>Expresiones condicionales NULL como inicializadores de proyección

Solo se permite una expresión condicional NULL como *member_declarator* en un *anonymous_object_creation_expression* (expresiones de[creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions)) si finaliza con un acceso a miembro (opcionalmente condicional). Gramaticalmente, este requisito se puede expresar de la siguiente manera:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Este es un caso especial de la gramática de *null_conditional_expression* anterior. La producción de *member_declarator* en [expresiones de creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions) incluye solo *null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Expresiones condicionales NULL como expresiones de instrucción

Solo se permite una expresión condicional NULL como *statement_expression* (instrucciones de[expresión](statements.md#expression-statements)) si finaliza con una invocación. Gramaticalmente, este requisito se puede expresar de la siguiente manera:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Este es un caso especial de la gramática de *null_conditional_expression* anterior. La producción de *statement_expression* en las [instrucciones de expresión](statements.md#expression-statements) solo incluye *null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Operador unario más

Para una operación con el formato `+x`, se aplica la resolución de sobrecargas del operador unario ([resolución de sobrecarga del operador unario](expressions.md#unary-operator-overload-resolution)) para seleccionar una implementación de operador específica. El operando se convierte al tipo de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador. Los operadores unarios más predefinidos más son:

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

Para una operación con el formato `-x`, se aplica la resolución de sobrecargas del operador unario ([resolución de sobrecarga del operador unario](expressions.md#unary-operator-overload-resolution)) para seleccionar una implementación de operador específica. El operando se convierte al tipo de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador. Los operadores de negación predefinidos son:

*  Negación de entero:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   El resultado se calcula restando `x` de cero. Si el valor de `x` es el valor representable más pequeño del tipo de operando (-2 ^ 31 para `int` o-2 ^ 63 para `long`), la negación matemática de `x` no se podrá representar en el tipo de operando. Si esto ocurre dentro de un contexto `checked`, se produce una `System.OverflowException`; Si se produce dentro de un contexto de @no__t 2, el resultado es el valor del operando y el desbordamiento no se registra.

   Si el operando del operador de negación es de tipo `uint`, se convierte al tipo `long` y el tipo del resultado es `long`. Una excepción es la regla que permite escribir el 2147483648 valor `int` (-2 ^ 31) como un literal entero decimal ([literales enteros](lexical-structure.md#integer-literals)).

   Si el operando del operador de negación es de tipo `ulong`, se produce un error en tiempo de compilación. Una excepción es la regla que permite escribir el valor `long`-9.223.372.036.854.775.808 (-2 ^ 63) como un literal entero decimal ([literales enteros](lexical-structure.md#integer-literals)).

*  Negación de punto flotante:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   El resultado es el valor de `x` con el signo invertido. Si `x` es NaN, el resultado es también NaN.

*  Negación decimal:

   ```csharp
   decimal operator -(decimal x);
   ```

   El resultado se calcula restando `x` de cero. La negación decimal es equivalente a usar el operador unario menos de tipo `System.Decimal`.

### <a name="logical-negation-operator"></a>Operador lógico de negación

Para una operación con el formato `!x`, se aplica la resolución de sobrecargas del operador unario ([resolución de sobrecarga del operador unario](expressions.md#unary-operator-overload-resolution)) para seleccionar una implementación de operador específica. El operando se convierte al tipo de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador. Solo existe un operador lógico de negación predefinido:
```csharp
bool operator !(bool x);
```

Este operador calcula la negación lógica del operando: Si el operando es `true`, el resultado es `false`. Si el operando es `false`, el resultado es `true`.

### <a name="bitwise-complement-operator"></a>Operador de complemento bit a bit

Para una operación con el formato `~x`, se aplica la resolución de sobrecargas del operador unario ([resolución de sobrecarga del operador unario](expressions.md#unary-operator-overload-resolution)) para seleccionar una implementación de operador específica. El operando se convierte al tipo de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador. Los operadores de complemento bit a bit predefinidos son:
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

El resultado de evaluar `~x`, donde `x` es una expresión de un tipo de enumeración `E` con un tipo subyacente `U`, es exactamente igual que evaluar `(E)(~(U)x)`, salvo que la conversión a `E` siempre se realiza como si estuviera en un contexto de `unchecked` ( [Los operadores Checked y unchecked](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Operadores de incremento y decremento prefijos

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

El operando de una operación de incremento o decremento de prefijo debe ser una expresión clasificada como una variable, un acceso a la propiedad o un acceso a un indexador. El resultado de la operación es un valor del mismo tipo que el operando.

Si el operando de una operación de incremento o decremento de prefijo es una propiedad o un indexador, la propiedad o el indexador deben tener un descriptor de acceso `get` y `set`. Si no es así, se produce un error en tiempo de enlace.

La resolución de sobrecargas de operador unario ([resolución de sobrecarga de operadores unarios](expressions.md#unary-operator-overload-resolution)) se aplica para seleccionar una implementación de operador específica. Los operadores predefinidos `++` y `--` existen para los siguientes tipos: @no__t 2, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 y cualquier tipo de enumeración. Los operadores predefinidos `++` devuelven el valor generado agregando 1 al operando y los operadores `--` predefinidos devuelven el valor generado restando 1 del operando. En un contexto `checked`, si el resultado de esta suma o resta está fuera del intervalo del tipo de resultado y el tipo de resultado es un tipo entero o un tipo de enumeración, se produce una `System.OverflowException`.

El procesamiento en tiempo de ejecución de una operación de incremento o decremento de prefijo con el formato `++x` o `--x` consta de los siguientes pasos:

*   Si `x` está clasificado como una variable:
    * `x` se evalúa para generar la variable.
    * El operador seleccionado se invoca con el valor de `x` como argumento.
    * El valor devuelto por el operador se almacena en la ubicación proporcionada por la evaluación de `x`.
    * El valor devuelto por el operador se convierte en el resultado de la operación.
*   Si `x` se clasifica como un acceso de propiedad o indizador:
    * La expresión de instancia (si `x` no es `static`) y la lista de argumentos (si `x` es un indexador) asociada a `x` se evalúan, y los resultados se usan en las siguientes invocaciones de descriptor de acceso `get` y `set`.
    * Se invoca al descriptor de acceso `get` de `x`.
    * El operador seleccionado se invoca con el valor devuelto por el descriptor de acceso `get` como su argumento.
    * El descriptor de acceso `set` de `x` se invoca con el valor devuelto por el operador como su argumento `value`.
    * El valor devuelto por el operador se convierte en el resultado de la operación.

Los operadores `++` y `--` también admiten la notación de postfijo ([operadores de incremento y decremento de postfijo](expressions.md#postfix-increment-and-decrement-operators)). Normalmente, el resultado de `x++` o `x--` es el valor de @no__t 2 antes de la operación, mientras que el resultado de `++x` o `--x` es el valor de `x` después de la operación. En cualquier caso, `x` tiene el mismo valor después de la operación.

Se puede invocar una implementación `operator++` o `operator--` mediante la notación de sufijo o de prefijo. No es posible tener implementaciones de operador independientes para las dos notaciones.

### <a name="cast-expressions"></a>Expresiones de conversión

Se usa un *cast_expression* para convertir explícitamente una expresión a un tipo determinado.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

Un *cast_expression* con el formato `(T)E`, donde `T` es un *tipo* y `E` es *unary_expression*, realiza una conversión explícita ([conversiones explícitas](conversions.md#explicit-conversions)) del valor de `E` al tipo `T`. Si no existe una conversión explícita de `E` a `T`, se produce un error en tiempo de enlace. De lo contrario, el resultado es el valor generado por la conversión explícita. El resultado siempre se clasifica como un valor, incluso si `E` denota una variable.

La gramática de una *cast_expression* conduce a ciertas ambigüedades sintácticas. Por ejemplo, la expresión `(x)-y` puede interpretarse como *cast_expression* (una conversión de `-y` al tipo `x`) o como un *additive_expression* combinado con *parenthesized_expression* (que calcula el valor `x - y)`.

Para resolver ambigüedades de *cast_expression* , existe la siguiente regla: Una secuencia de uno o más *tokens*([espacio en blanco](lexical-structure.md#white-space)) entre paréntesis se considera el inicio de un *cast_expression* solo si se cumple al menos una de las siguientes condiciones:

*  La secuencia de tokens es la gramática correcta para un *tipo*, pero no para una *expresión*.
*  La secuencia de tokens es la gramática correcta para un *tipo*, y el token que sigue inmediatamente al paréntesis de cierre es el token "`~`", el token "`!`", el token "`(`", un *identificador* (secuencias de[escape de caracteres Unicode ](lexical-structure.md#unicode-character-escape-sequences)), un *literal* ([literales](lexical-structure.md#literals)) o cualquier *palabra clave* ([keywords](lexical-structure.md#keywords)) excepto 0 y 1.

El término "gramática correcta" anterior significa que la secuencia de tokens debe ajustarse a la producción gramatical concreta. En concreto, no se tiene en cuenta el significado real de los identificadores constituyentes. Por ejemplo, si `x` y `y` son identificadores, `x.y` es la gramática correcta para un tipo, incluso si `x.y` no denota realmente un tipo.

En la regla de desambiguación se sigue que, si `x` y `y` son identificadores, @no__t 2, `(x)(y)` y `(x)(-y)` son *cast_expression*, pero `(x)-y` no es, aunque `x` identifique un tipo. Sin embargo, si `x` es una palabra clave que identifica un tipo predefinido (como `int`), las cuatro formas son *cast_expression*s (porque tal palabra clave podría ser una expresión por sí misma).

### <a name="await-expressions"></a>Expresiones Await

El operador Await se usa para suspender la evaluación de la función asincrónica envolvente hasta que se haya completado la operación asincrónica representada por el operando.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

Un *await_expression* solo se permite en el cuerpo de una función asincrónica ([iteradores](classes.md#iterators)). Dentro de la función asincrónica envolvente más cercana, puede que no se produzca una *await_expression* en estos lugares:

*  Dentro de una función anónima anidada (no asincrónica)
*  Dentro del bloque de un *lock_statement*
*  En un contexto no seguro

Tenga en cuenta que no se puede producir un *await_expression* en la mayoría de los lugares de una *query_expression*, ya que se transforman sintácticamente para usar expresiones lambda no asincrónicas.

Dentro de una función asincrónica, `await` no se puede usar como identificador. Por lo tanto, no hay ambigüedades sintácticas entre las expresiones Await y varias expresiones que intervienen en identificadores. Fuera de las funciones asincrónicas, `await` actúa como un identificador normal.

El operando de un *await_expression* se denomina ***tarea***. Representa una operación asincrónica que puede o no completarse en el momento en que se evalúa el *await_expression* . El propósito del operador Await es suspender la ejecución de la función asincrónica envolvente hasta que se complete la tarea en espera y, a continuación, obtener el resultado.

#### <a name="awaitable-expressions"></a>Expresiones que admiten Await

Es necesario que la tarea de una expresión Await sea ***Await***. Una expresión `t` es que admite Await si uno de los siguientes elementos contiene:

*  `t` es del tipo de tiempo de compilación `dynamic`
*  `t` tiene una instancia o un método de extensión accesible denominado `GetAwaiter` sin parámetros y sin parámetros de tipo, y un tipo de valor devuelto `A` para el que se mantiene todo lo siguiente:
   * `A` implementa la interfaz `System.Runtime.CompilerServices.INotifyCompletion` (en adelante denominada `INotifyCompletion` por motivos de brevedad)
   * `A` tiene una propiedad de instancia accesible y legible `IsCompleted` de tipo `bool`
   * `A` tiene un método de instancia accesible `GetResult` sin parámetros y sin parámetros de tipo

El propósito del método `GetAwaiter` es obtener un objeto ***Await*** para la tarea. El tipo `A` se denomina ***tipo de espera*** para la expresión Await.

El propósito de la propiedad `IsCompleted` es determinar si la tarea ya se ha completado. Si es así, no es necesario suspender la evaluación.

El propósito del método `INotifyCompletion.OnCompleted` es registrar una "continuación" en la tarea; es decir, un delegado (de tipo `System.Action`) que se invocará una vez completada la tarea.

El propósito del método `GetResult` es obtener el resultado de la tarea una vez completada. Este resultado puede ser una finalización correcta, posiblemente con un valor de resultado, o puede ser una excepción producida por el método `GetResult`.

#### <a name="classification-of-await-expressions"></a>Clasificación de expresiones Await

La expresión `await t` se clasifica de la misma forma que la expresión `(t).GetAwaiter().GetResult()`. Por lo tanto, si el tipo de valor devuelto de `GetResult` es `void`, el *await_expression* se clasifica como Nothing. Si tiene un tipo de valor devuelto distinto de void `T`, el *await_expression* se clasifica como un valor de tipo `T`.

#### <a name="runtime-evaluation-of-await-expressions"></a>Evaluación en tiempo de ejecución de expresiones Await

En tiempo de ejecución, la expresión `await t` se evalúa como sigue:

*  Se obtiene un Await `a` evaluando la expresión `(t).GetAwaiter()`.
*  Se obtiene un `bool` `b` evaluando la expresión `(a).IsCompleted`.
*  Si `b` es `false`, la evaluación depende de si `a` implementa la interfaz `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (en adelante conocida como `ICriticalNotifyCompletion` por motivos de brevedad). Esta comprobación se realiza en el momento del enlace; es decir, en tiempo de ejecución si `a` tiene el tipo de tiempo de compilación `dynamic` y en tiempo de compilación, de lo contrario,. Permita que `r` denote el delegado de reanudación ([iteradores](classes.md#iterators)):
    * Si `a` no implementa `ICriticalNotifyCompletion`, se evalúa la expresión `(a as (INotifyCompletion)).OnCompleted(r)`.
    * Si `a` implementa `ICriticalNotifyCompletion`, se evalúa la expresión `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)`.
    * A continuación, la evaluación se suspende y el control se devuelve al autor de la llamada actual de la función asincrónica.
*  Inmediatamente después de (si `b` era `true`) o tras una invocación posterior del delegado de reanudación (si `b` era `false`), se evalúa la expresión `(a).GetResult()`. Si devuelve un valor, ese valor es el resultado de *await_expression*. De lo contrario, el resultado es Nothing.

La implementación de un espera de los métodos de interfaz `INotifyCompletion.OnCompleted` y `ICriticalNotifyCompletion.UnsafeOnCompleted` debe hacer que el delegado `r` se invoque como máximo una vez. De lo contrario, el comportamiento de la función asincrónica envolvente es indefinido.

## <a name="arithmetic-operators"></a>Operadores aritméticos

Los operadores `*`, `/`, `%`, `+` y `-` se denominan operadores aritméticos.

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

Si un operando de un operador aritmético tiene el tipo en tiempo de compilación `dynamic`, la expresión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso, el tipo en tiempo de compilación de la expresión es `dynamic`, y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución de los operandos que tienen el tipo en tiempo de compilación `dynamic`.

### <a name="multiplication-operator"></a>Operador de multiplicación

Para una operación con el formato `x * y`, se aplica la resolución de sobrecargas del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica. Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.

A continuación se enumeran los operadores de multiplicación predefinidos. Todos los operadores calculan el producto de `x` y `y`.

*  Multiplicación de enteros:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   En un contexto `checked`, si el producto está fuera del intervalo del tipo de resultado, se produce una `System.OverflowException`. En un contexto `unchecked`, no se informan los desbordamientos y se descartan los bits significativos de orden superior fuera del intervalo del tipo de resultado.


*  Multiplicación de punto flotante:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   El producto se calcula de acuerdo con las reglas de aritmética de IEEE 754. En la tabla siguiente se enumeran los resultados de todas las posibles combinaciones de valores finitos distintos de cero, ceros, infinitos y NaN. En la tabla, `x` y `y` son valores finitos positivos. `z` es el resultado de `x * y`. Si el resultado es demasiado grande para el tipo de destino, `z` es infinito. Si el resultado es demasiado pequeño para el tipo de destino, `z` es cero.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | \+ y   | -y   | +0  | -0  | +inf | -inf | NaN | 
   | +x   | +z   | -z   | +0  | -0  | +inf | -inf | NaN | 
   | -x   | -z   | +z   | -0  | +0  | -inf | +inf | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | +inf | +inf | -inf | NaN | NaN | +inf | -inf | NaN | 
   | -inf | -inf | +inf | NaN | NaN | -inf | +inf | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  Multiplicación decimal:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Si el valor resultante es demasiado grande para representarlo en el formato `decimal`, se produce una `System.OverflowException`. Si el valor del resultado es demasiado pequeño para representarlo en el formato `decimal`, el resultado es cero. La escala del resultado, antes de cualquier redondeo, es la suma de las escalas de los dos operandos.

   La multiplicación decimal es equivalente a usar el operador de multiplicación de tipo `System.Decimal`.


### <a name="division-operator"></a>Operador de división

Para una operación con el formato `x / y`, se aplica la resolución de sobrecargas del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica. Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.

A continuación se enumeran los operadores de división predefinidos. Todos los operadores calculan el cociente de `x` y `y`.

*  División de enteros:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Si el valor del operando derecho es cero, se produce una `System.DivideByZeroException`.

   La división redondea el resultado hacia cero. Por lo tanto, el valor absoluto del resultado es el entero posible más grande que sea menor o igual que el valor absoluto del cociente de los dos operandos. El resultado es cero o positivo cuando los dos operandos tienen el mismo signo y cero o negativo cuando los dos operandos tienen signos opuestos.

   Si el operando izquierdo es el valor más pequeño que se va a representar `int` o `long` y el operando derecho es `-1`, se produce un desbordamiento. En un contexto `checked`, esto hace que se produzca una `System.ArithmeticException` (o una subclase). En un contexto `unchecked`, se define como si se produjera una `System.ArithmeticException` (o una subclase de ella) o el desbordamiento se desinforma de que el valor resultante es el del operando izquierdo.

*  División de punto flotante:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   El cociente se calcula de acuerdo con las reglas de aritmética de IEEE 754. En la tabla siguiente se enumeran los resultados de todas las posibles combinaciones de valores finitos distintos de cero, ceros, infinitos y NaN. En la tabla, `x` y `y` son valores finitos positivos. `z` es el resultado de `x / y`. Si el resultado es demasiado grande para el tipo de destino, `z` es infinito. Si el resultado es demasiado pequeño para el tipo de destino, `z` es cero.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | \+ y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | +x   | +z   | -z   | +inf | -inf | +0   | -0   | NaN  | 
   | -x   | -z   | +z   | -inf | +inf | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | +inf | +inf | -inf | +inf | -inf | NaN  | NaN  | NaN  | 
   | -inf | -inf | +inf | -inf | +inf | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  División decimal:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Si el valor del operando derecho es cero, se produce una `System.DivideByZeroException`. Si el valor resultante es demasiado grande para representarlo en el formato `decimal`, se produce una `System.OverflowException`. Si el valor del resultado es demasiado pequeño para representarlo en el formato `decimal`, el resultado es cero. La escala del resultado es la menor escala que conservará un resultado igual al valor decimal que se va a representar más cercano al resultado matemático verdadero.

   La división decimal es equivalente a usar el operador de división de tipo `System.Decimal`.


### <a name="remainder-operator"></a>Operador de resto

Para una operación con el formato `x % y`, se aplica la resolución de sobrecargas del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica. Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.

A continuación se enumeran los operadores de resto predefinidos. Todos los operadores calculan el resto de la división entre `x` y `y`.

*  Resto entero:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   El resultado de `x % y` es el valor generado por `x - (x / y) * y`. Si `y` es cero, se produce una `System.DivideByZeroException`.

   Si el operando izquierdo es el valor más pequeño `int` o `long` y el operando derecho es `-1`, se produce una `System.OverflowException`. En ningún caso, `x % y` producen una excepción en la que `x / y` no produciría una excepción.

*  Resto de punto flotante:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   En la tabla siguiente se enumeran los resultados de todas las posibles combinaciones de valores finitos distintos de cero, ceros, infinitos y NaN. En la tabla, `x` y `y` son valores finitos positivos. `z` es el resultado de `x % y` y se calcula como @no__t 2, donde `n` es el entero posible más grande que sea menor o igual que `x / y`. Este método de calcular el resto es análogo al que se usa para los operandos de enteros, pero difiere de la definición de IEEE 754 (en la que `n` es el entero más próximo a `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | \+ y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | +x   | +z   | +z   | NaN  | NaN  | x    | x    | NaN  | 
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

   Si el valor del operando derecho es cero, se produce una `System.DivideByZeroException`. La escala del resultado, antes de cualquier redondeo, es el mayor de las escalas de los dos operandos y el signo del resultado, si es distinto de cero, es el mismo que el de `x`.

   El resto decimal es equivalente a usar el operador de resto de tipo `System.Decimal`.


### <a name="addition-operator"></a>Operador de suma

Para una operación con el formato `x + y`, se aplica la resolución de sobrecargas del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica. Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.

A continuación se enumeran los operadores de suma predefinidos. En el caso de los tipos numéricos y de enumeración, los operadores de suma predefinidos calculan la suma de los dos operandos. Cuando uno o ambos operandos son de tipo cadena, los operadores de suma predefinidos concatenan la representación de cadena de los operandos.

*  Suma de enteros:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   En un contexto `checked`, si la suma está fuera del intervalo del tipo de resultado, se produce una `System.OverflowException`. En un contexto `unchecked`, no se informan los desbordamientos y se descartan los bits significativos de orden superior fuera del intervalo del tipo de resultado.

*  Adición de punto flotante:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   La suma se calcula de acuerdo con las reglas de aritmética de IEEE 754. En la tabla siguiente se enumeran los resultados de todas las posibles combinaciones de valores finitos distintos de cero, ceros, infinitos y NaN. En la tabla, `x` y `y` son valores finitos distintos de cero y @no__t 2 es el resultado de `x + y`. Si `x` y `y` tienen la misma magnitud pero signos opuestos, `z` es cero positivo. Si `x + y` es demasiado grande para representarlo en el tipo de destino, `z` es un infinito con el mismo signo que @no__t 2.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | y    | +0   | -0   | +inf | -inf | NaN  | 
   | x    | z    | x    | x    | +inf | -inf | NaN  | 
   | +0   | y    | +0   | +0   | +inf | -inf | NaN  | 
   | -0   | y    | +0   | -0   | +inf | -inf | NaN  | 
   | +inf | +inf | +inf | +inf | +inf | NaN  | NaN  | 
   | -inf | -inf | -inf | -inf | NaN  | -inf | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Suma decimal:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Si el valor resultante es demasiado grande para representarlo en el formato `decimal`, se produce una `System.OverflowException`. La escala del resultado, antes de cualquier redondeo, es el mayor de las escalas de los dos operandos.

   La suma decimal es equivalente a usar el operador de suma de tipo `System.Decimal`.

*  Adición de enumeración. Cada tipo de enumeración proporciona implícitamente los siguientes operadores predefinidos, donde `E` es el tipo de enumeración y `U` es el tipo subyacente de `E`:

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

   Estas sobrecargas del operador binario `+` realizan la concatenación de cadenas. Si un operando de la concatenación de cadenas es `null`, se sustituye una cadena vacía. De lo contrario, cualquier argumento que no sea de cadena se convierte en su representación de cadena invocando el método virtual `ToString` heredado del tipo `object`. Si `ToString` devuelve `null`, se sustituye una cadena vacía.

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

   El resultado del operador de concatenación de cadenas es una cadena que consta de los caracteres del operando izquierdo seguidos de los caracteres del operando derecho. El operador de concatenación de cadenas nunca devuelve un valor `null`. Se puede iniciar una `System.OutOfMemoryException` si no hay suficiente memoria disponible para asignar la cadena resultante.

*  Combinación de delegado. Cada tipo de delegado proporciona implícitamente el siguiente operador predefinido, donde `D` es el tipo de delegado:

   ```csharp
   D operator +(D x, D y);
   ```

   El operador binario `+` realiza la combinación de delegados cuando ambos operandos son de algún tipo de delegado `D`. (Si los operandos tienen distintos tipos de delegado, se produce un error en tiempo de enlace). Si el primer operando es `null`, el resultado de la operación es el valor del segundo operando (aunque también sea `null`). De lo contrario, si el segundo operando es `null`, el resultado de la operación es el valor del primer operando. De lo contrario, el resultado de la operación es una nueva instancia de delegado que, cuando se invoca, invoca el primer operando y, a continuación, invoca el segundo operando. Para obtener ejemplos de la combinación de delegados, vea [operador de resta](expressions.md#subtraction-operator) e [invocación de delegado](delegates.md#delegate-invocation). Como `System.Delegate` no es un tipo de delegado, `operator` @ no__t-2 no está definido para él.

### <a name="subtraction-operator"></a>Operador de resta

Para una operación con el formato `x - y`, se aplica la resolución de sobrecargas del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica. Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.

A continuación se enumeran los operadores de resta predefinidos. Todos los operadores restan `y` de `x`.

*  Resta de enteros:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   En un contexto `checked`, si la diferencia está fuera del intervalo del tipo de resultado, se produce una `System.OverflowException`. En un contexto `unchecked`, no se informan los desbordamientos y se descartan los bits significativos de orden superior fuera del intervalo del tipo de resultado.

*  Resta de punto flotante:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   La diferencia se calcula de acuerdo con las reglas de aritmética de IEEE 754. En la tabla siguiente se enumeran los resultados de todas las posibles combinaciones de valores finitos distintos de cero, ceros, infinitos y Nan. En la tabla, `x` y `y` son valores finitos distintos de cero y @no__t 2 es el resultado de `x - y`. Si `x` y `y` son iguales, @no__t 2 es cero positivo. Si `x - y` es demasiado grande para representarlo en el tipo de destino, `z` es un infinito con el mismo signo que @no__t 2.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | y    | +0   | -0   | +inf | -inf | NaN | 
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

   Si el valor resultante es demasiado grande para representarlo en el formato `decimal`, se produce una `System.OverflowException`. La escala del resultado, antes de cualquier redondeo, es el mayor de las escalas de los dos operandos.

   La resta decimal es equivalente a usar el operador de resta de tipo `System.Decimal`.

*  Resta de enumeración. Cada tipo de enumeración proporciona implícitamente el siguiente operador predefinido, donde `E` es el tipo de enumeración y `U` es el tipo subyacente de `E`:

   ```csharp
   U operator -(E x, E y);
   ```

   Este operador se evalúa exactamente como `(U)((U)x - (U)y)`. En otras palabras, el operador calcula la diferencia entre los valores ordinales de `x` y `y`, y el tipo del resultado es el tipo subyacente de la enumeración.

   ```csharp
   E operator -(E x, U y);
   ```

   Este operador se evalúa exactamente como `(E)((U)x - y)`. En otras palabras, el operador resta un valor del tipo subyacente de la enumeración, lo que produce un valor de la enumeración.

*  Eliminación de delegados. Cada tipo de delegado proporciona implícitamente el siguiente operador predefinido, donde `D` es el tipo de delegado:

   ```csharp
   D operator -(D x, D y);
   ```

   El operador binario `-` realiza la eliminación del delegado cuando ambos operandos son de algún tipo de delegado `D`. Si los operandos tienen distintos tipos de delegado, se produce un error en tiempo de enlace. Si el primer operando es `null`, el resultado de la operación es `null`. De lo contrario, si el segundo operando es `null`, el resultado de la operación es el valor del primer operando. De lo contrario, ambos operandos representan listas de invocación ([declaraciones de delegado](delegates.md#delegate-declarations)) que tienen una o más entradas y el resultado es una nueva lista de invocación que se compone de la lista del primer operando con las entradas del segundo operando que se han quitado, siempre y cuando el segundo la lista de operandos es una sublista contigua adecuada de la primera.     (Para determinar la igualdad de la lista, las entradas correspondientes se comparan como para el operador de igualdad de delegado ([operadores de igualdad de delegado](expressions.md#delegate-equality-operators))). De lo contrario, el resultado es el valor del operando izquierdo. En el proceso no se cambia ninguna de las listas de operandos. Si la lista del segundo operando coincide con varias sublistas de entradas contiguas de la lista del primer operando, se quita la sublista coincidente situada más a la derecha de las entradas contiguas. Si la eliminación da como resultado una lista vacía, el resultado es `null`. Por ejemplo:

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

Los operadores `<<` y `>>` se utilizan para realizar operaciones de desplazamiento de bits.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Si un operando de un *shift_expression* tiene el tipo en tiempo de compilación `dynamic`, la expresión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso, el tipo en tiempo de compilación de la expresión es `dynamic`, y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución de los operandos que tienen el tipo en tiempo de compilación `dynamic`.

Para una operación con el formato `x << count` o `x >> count`, se aplica la resolución de sobrecarga del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica. Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.

Al declarar un operador de desplazamiento sobrecargado, el tipo del primer operando siempre debe ser la clase o el struct que contiene la declaración del operador y el tipo del segundo operando siempre debe ser `int`.

A continuación se enumeran los operadores de desplazamiento predefinidos.

*  Desplazar a la izquierda:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   El operador `<<` desplaza `x` a la izquierda por un número de bits calculado como se describe a continuación.

   Los bits de orden superior fuera del intervalo del tipo de resultado de `x` se descartan, los bits restantes se desplazan a la izquierda y las posiciones de bits vacías de orden inferior se establecen en cero.

*  Desplazamiento a la derecha:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   El operador `>>` desplaza `x` a la derecha un número de bits calculado como se describe a continuación.

   Cuando `x` es de tipo `int` o @no__t 2, se descartan los bits de orden inferior de `x`, los bits restantes se desplazan a la derecha y las posiciones de bits vacías de orden superior se establecen en cero si `x` no es negativo y se establece en uno si `x` es negativo.

   Cuando `x` es de tipo `uint` o @no__t 2, se descartan los bits de orden inferior de `x`, los bits restantes se desplazan a la derecha y las posiciones de bits vacías de orden superior se establecen en cero.

En el caso de los operadores predefinidos, el número de bits que se va a desplazar se calcula de la manera siguiente:

*  Cuando el tipo de `x` es `int` o `uint`, el recuento de desplazamiento viene dado por los cinco bits de orden inferior de `count`. En otras palabras, el número de turnos se calcula a partir de `count & 0x1F`.
*  Cuando el tipo de `x` es `long` o `ulong`, el recuento de desplazamiento viene dado por los seis bits de orden inferior de `count`. En otras palabras, el número de turnos se calcula a partir de `count & 0x3F`.

Si el número de turnos resultante es cero, los operadores de desplazamiento simplemente devuelven el valor de `x`.

Las operaciones de desplazamiento nunca causan desbordamientos y producen los mismos resultados en los contextos `checked` y `unchecked`.

Cuando el operando izquierdo del operador `>>` es de un tipo entero con signo, el operador realiza un desplazamiento aritmético a la derecha, donde el valor del bit más significativo (el bit de signo) del operando se propaga a las posiciones de bits vacías de orden superior. Cuando el operando izquierdo del operador `>>` es de un tipo entero sin signo, el operador realiza un desplazamiento lógico derecho en el que las posiciones de bits vacías de orden superior siempre se establecen en cero. Para realizar la operación opuesta a la que se infiere del tipo de operando, se pueden usar conversiones explícitas. Por ejemplo, si `x` es una variable de tipo `int`, la operación `unchecked((int)((uint)x >> y))` realiza un desplazamiento lógico a la derecha de `x`.

## <a name="relational-and-type-testing-operators"></a>Operadores de comprobación de tipos y relacionales

Los operadores `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` y `as` se denominan operadores relacionales y de prueba de tipos.

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

El operador `is` se describe en [el operador is](expressions.md#the-is-operator) y el operador `as` se describe en [el operador as](expressions.md#the-as-operator).

Los operadores `==`, `!=`, `<`, `>`, `<=` y `>=` son ***operadores de comparación***.

Si un operando de un operador de comparación tiene el tipo en tiempo de compilación `dynamic`, la expresión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso, el tipo en tiempo de compilación de la expresión es `dynamic`, y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución de los operandos que tienen el tipo en tiempo de compilación `dynamic`.

En el caso de una operación con el formato `x` *op* `y`, donde *OP* es un operador de comparación, se aplica la resolución de sobrecarga ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica. Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.

Los operadores de comparación predefinidos se describen en las secciones siguientes. Todos los operadores de comparación predefinidos devuelven un resultado de tipo `bool`, tal y como se describe en la tabla siguiente.


| __Sesión__ | __Resultado__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true` si `x` es igual a `y`, `false` en caso contrario                 | 
| `x != y`      | `true` si `x` no es igual a `y`, `false` en caso contrario             | 
| `x < y`       | `true` si `x` es menor que `y`, `false` en caso contrario                | 
| `x > y`       | `true` si `x` es mayor que `y`, `false` en caso contrario             | 
| `x <= y`      | `true` si `x` es menor o igual que `y`, `false` en caso contrario    | 
| `x >= y`      | `true` si `x` es mayor o igual que `y`, `false` en caso contrario | 

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

Cada uno de estos operadores compara los valores numéricos de los dos operandos enteros y devuelve un valor `bool` que indica si la relación determinada es `true` o `false`.

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

*  Si alguno de los operandos es NaN, el resultado es `false` para todos los operadores excepto `!=`, para los que el resultado es `true`. Para dos operandos cualesquiera, `x != y` siempre produce el mismo resultado que `!(x == y)`. Sin embargo, cuando uno o ambos operandos son NaN, los operadores `<`, `>`, `<=` y `>=` no producen los mismos resultados que la negación lógica del operador opuesto. Por ejemplo, si cualquiera de `x` y `y` es NaN, @no__t 2 es `false`, pero `!(x >= y)` es `true`.
*  Cuando ninguno de los operandos es NaN, los operadores comparan los valores de los dos operandos de punto flotante con respecto a la ordenación.

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   donde `min` y `max` son los valores finitos positivos más pequeños y mayores que se pueden representar en el formato de punto flotante dado. Los efectos importantes de este orden son:
   * Los ceros negativos y positivos se consideran iguales.
   * Un infinito negativo se considera menor que todos los demás valores, pero es igual a otro infinito negativo.
   * Un infinito positivo se considera mayor que el resto de los valores, pero es igual a otro infinito positivo.

### <a name="decimal-comparison-operators"></a>Operadores de comparación decimal

Los operadores de comparación decimal predefinidos son:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Cada uno de estos operadores compara los valores numéricos de los dos operandos decimales y devuelve un valor `bool` que indica si la relación determinada es `true` o `false`. Cada comparación decimal es equivalente a usar el operador relacional o de igualdad correspondiente del tipo `System.Decimal`.

### <a name="boolean-equality-operators"></a>Operadores de igualdad booleanos

Los operadores de igualdad booleano predefinidos son:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

El resultado de `==` es `true` si tanto `x` como `y` son `true` o si `x` y `y` son `false`. De lo contrario, el resultado es `false`.

El resultado de `!=` es `false` si tanto `x` como `y` son `true` o si `x` y `y` son `false`. De lo contrario, el resultado es `true`. Cuando los operandos son del tipo `bool`, el operador `!=` produce el mismo resultado que el operador `^`.

### <a name="enumeration-comparison-operators"></a>Operadores de comparación de enumeración

Cada tipo de enumeración proporciona implícitamente los siguientes operadores de comparación predefinidos:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

El resultado de evaluar `x op y`, donde `x` y `y` son expresiones de un tipo de enumeración `E` con un tipo subyacente `U` y `op` es uno de los operadores de comparación, es exactamente igual que evaluar `((U)x) op ((U)y)`. En otras palabras, los operadores de comparación de tipos de enumeración simplemente comparan los valores enteros subyacentes de los dos operandos.

### <a name="reference-type-equality-operators"></a>Operadores de igualdad de tipos de referencia

Los operadores de igualdad de tipos de referencia predefinidos son:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

Los operadores devuelven el resultado de comparar las dos referencias de igualdad o de no igualdad.

Puesto que los operadores de igualdad de tipos de referencia predefinidos aceptan operandos de tipo `object`, se aplican a todos los tipos que no declaran los miembros `operator ==` y `operator !=` aplicables. Por el contrario, todos los operadores de igualdad definidos por el usuario aplicables ocultan de forma eficaz los operadores de igualdad de tipos de referencia predefinidos.

Los operadores de igualdad de tipos de referencia predefinidos requieren uno de los siguientes:

*  Ambos operandos son un valor de un tipo conocido como *reference_type* o literal `null`. Además, existe una conversión de referencia explícita ([conversiones de referencia explícitas](conversions.md#explicit-reference-conversions)) desde el tipo de uno de los operandos al tipo del otro operando.
*  Un operando es un valor de tipo `T` donde `T` es un *type_parameter* y el otro operando es el literal `null`. Además `T` no tiene la restricción de tipo de valor.

A menos que se cumpla una de estas condiciones, se produce un error en tiempo de enlace. Las implicaciones destacadas de estas reglas son:

*  Es un error en tiempo de enlace usar los operadores de igualdad de tipos de referencia predefinidos para comparar dos referencias que se sabe que son diferentes en tiempo de enlace. Por ejemplo, si los tipos en tiempo de enlace de los operandos son dos tipos de clase `A` y `B`, y si ninguno de los dos operandos `A` ni `B` se derivan del otro, sería imposible que los dos operandos hagan referencia al mismo objeto. Por lo tanto, la operación se considera un error en tiempo de enlace.
*  Los operadores de igualdad de tipos de referencia predefinidos no permiten comparar los operandos de tipo de valor. Por lo tanto, a menos que un tipo de estructura declare sus propios operadores de igualdad, no es posible comparar valores de ese tipo de estructura.
*  Los operadores de igualdad de tipos de referencia predefinidos nunca provocan que se produzcan operaciones de conversión boxing para sus operandos. No tendría sentido realizar estas operaciones de conversión boxing, ya que las referencias a las instancias de conversión boxing recién asignadas serían necesariamente distintas de las demás referencias.
*  Si un operando de un tipo de parámetro de tipo `T` se compara con `null` y el tipo en tiempo de ejecución de `T` es un tipo de valor, el resultado de la comparación es `false`.

En el ejemplo siguiente se comprueba si un argumento de un tipo de parámetro de tipo sin restricciones es `null`.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

La construcción `x == null` se permite aunque `T` pueda representar un tipo de valor y el resultado se define simplemente como @no__t 2 cuando `T` es un tipo de valor.

En el caso de una operación con el formato `x == y` o `x != y`, si se cumple alguna `operator ==` o `operator !=` aplicable, las reglas de resolución de sobrecarga del operador ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) seleccionarán ese operador en lugar de la igualdad de tipos de referencia predefinida. Operator. Sin embargo, siempre es posible seleccionar el operador de igualdad de tipos de referencia predefinidos convirtiendo explícitamente uno o ambos operandos al tipo `object`. El ejemplo
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
```console
True
False
False
False
```

Las variables `s` y `t` hacen referencia a dos instancias distintas de @no__t 2 que contienen los mismos caracteres. La primera comparación genera `True` porque el operador de igualdad de cadena predefinido ([operadores de igualdad de cadena](expressions.md#string-equality-operators)) está seleccionado cuando ambos operandos son del tipo `string`. En el resto se comparan todos los resultados `False` porque se selecciona el operador de igualdad de tipos de referencia predefinido cuando uno o ambos operandos son del tipo `object`.

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
genera `False` porque las conversiones crean referencias a dos instancias independientes de valores de `int` con conversión boxing.

### <a name="string-equality-operators"></a>Operadores de igualdad de cadenas

Los operadores de igualdad de cadena predefinidos son:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Dos valores `string` se consideran iguales cuando se cumple una de las siguientes condiciones:

*  Ambos valores son `null`.
*  Ambos valores son referencias no nulas a instancias de cadena que tienen longitudes idénticas y caracteres idénticos en cada posición de carácter.

Los operadores de igualdad de cadena comparan valores de cadena en lugar de referencias de cadena. Cuando dos instancias de cadena independientes contienen exactamente la misma secuencia de caracteres, los valores de las cadenas son iguales, pero las referencias son diferentes. Como se describe en [operadores de igualdad de tipos de referencia](expressions.md#reference-type-equality-operators), los operadores de igualdad de tipos de referencia se pueden usar para comparar referencias de cadena en lugar de valores de cadena.

### <a name="delegate-equality-operators"></a>Operadores de igualdad de delegado

Cada tipo de delegado proporciona implícitamente los siguientes operadores de comparación predefinidos:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

Dos instancias de delegado se consideran iguales como se indica a continuación:

*  Si alguna de las instancias de delegado es `null`, son iguales si y solo si ambas son `null`.
*  Si los delegados tienen un tipo diferente en tiempo de ejecución, nunca son iguales.
*  Si ambas instancias de delegado tienen una lista de invocaciones ([declaraciones de delegado](delegates.md#delegate-declarations)), esas instancias son iguales si y solo si sus listas de invocación tienen la misma longitud, y cada entrada de una lista de invocación de un es igual (tal como se define a continuación) al correspondiente entrada, en orden, en la lista de invocación de otro.

Las siguientes reglas rigen la igualdad de las entradas de la lista de invocación:

*  Si dos entradas de la lista de invocación hacen referencia al mismo método estático, las entradas son iguales.
*  Si dos entradas de la lista de invocación hacen referencia al mismo método no estático en el mismo objeto de destino (tal y como se define en los operadores de igualdad de referencia), las entradas son iguales.
*  Las entradas de la lista de invocación generadas a partir de la evaluación de *anonymous_method_expression*s semánticamente idénticas o *lambda_expression*s con el mismo conjunto (posiblemente vacío) de instancias de variables externas capturadas se permiten (pero no se requieren) sea.

### <a name="equality-operators-and-null"></a>Operadores de igualdad y null

Los operadores `==` y `!=` permiten que un operando sea un valor de un tipo que acepta valores NULL y el otro para ser el literal @no__t 2, aunque no exista ningún operador predefinido o definido por el usuario (en formato sin levantar o elevado) para la operación.

Para una operación de uno de los formularios
```csharp
x == null
null == x
x != null
null != x
```
donde `x` es una expresión de un tipo que acepta valores NULL, si la resolución de sobrecarga del operador ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) no encuentra un operador aplicable, el resultado se calcula a partir de la propiedad `HasValue` de `x`. En concreto, las dos primeras formas se traducen en `!x.HasValue` y las dos últimas se traducen en `x.HasValue`.

### <a name="the-is-operator"></a>El operador is

El operador `is` se usa para comprobar dinámicamente si el tipo en tiempo de ejecución de un objeto es compatible con un tipo determinado. El resultado de la operación `E is T`, donde `E` es una expresión y @no__t 2 es un tipo, es un valor booleano que indica si `E` se puede convertir correctamente al tipo `T` por una conversión de referencia, una conversión boxing o una conversión unboxing. La operación se evalúa como sigue, después de que se hayan sustituido los argumentos de tipo para todos los parámetros de tipo:

*  Si `E` es una función anónima, se produce un error en tiempo de compilación
*  Si `E` es un grupo de métodos o el literal `null`, si el tipo de `E` es un tipo de referencia o un tipo que acepta valores NULL y el valor de `E` es null, el resultado es false.
*  De lo contrario, deje que `D` represente el tipo dinámico de `E` como se indica a continuación:
   * Si el tipo de `E` es un tipo de referencia, `D` es el tipo en tiempo de ejecución de la referencia de instancia por `E`.
   * Si el tipo de `E` es un tipo que acepta valores NULL, `D` es el tipo subyacente de ese tipo que acepta valores NULL.
   * Si el tipo de `E` es un tipo de valor que no acepta valores NULL, `D` es el tipo de `E`.
*  El resultado de la operación depende de `D` y `T` como se indica a continuación:
   * Si `T` es un tipo de referencia, el resultado es true si `D` y `T` son del mismo tipo, si `D` es un tipo de referencia y existe una conversión de referencia implícita de `D` a `T`, o bien si `D` es un tipo de valor y una conversión boxing de `D`. para `T` existe.
   * Si `T` es un tipo que acepta valores NULL, el resultado es true si `D` es el tipo subyacente de `T`.
   * Si `T` es un tipo de valor que no acepta valores NULL, el resultado es true si `D` y `T` son del mismo tipo.
   * De lo contrario, el resultado es false.

Tenga en cuenta que las conversiones definidas por el usuario no se tienen en cuenta por el operador `is`.

### <a name="the-as-operator"></a>El operador as

El operador `as` se usa para convertir explícitamente un valor en un tipo de referencia determinado o un tipo que acepta valores NULL. A diferencia de una expresión de conversión ([expresiones de conversión](expressions.md#cast-expressions)), el operador `as` nunca produce una excepción. En su lugar, si la conversión indicada no es posible, el valor resultante es `null`.

En una operación con el formato `E as T`, `E` debe ser una expresión y `T` debe ser un tipo de referencia, un parámetro de tipo conocido como un tipo de referencia o un tipo que acepta valores NULL. Además, al menos uno de los siguientes debe ser true o, de lo contrario, se producirá un error en tiempo de compilación:

*  Una identidad ([conversión de identidad](conversions.md#identity-conversion)), implícita que acepta valores NULL ([conversiones implícitas que aceptan valores NULL](conversions.md#implicit-nullable-conversions)), referencia implícita (conversiones de[referencias implícitas](conversions.md#implicit-reference-conversions)), conversión boxing ([conversiones boxing](conversions.md#boxing-conversions)), explícitamente acepta valores NULL ([admite valores NULL explícitos). conversiones](conversions.md#explicit-nullable-conversions)), referencia explícita (conversiones de[referencia explícita](conversions.md#explicit-reference-conversions)) o conversión unboxing ([conversiones unboxing](conversions.md#unboxing-conversions)) de `E` a `T`.
*  El tipo de `E` o `T` es un tipo abierto.
*  `E` es el literal `null`.

Si el tipo en tiempo de compilación de `E` no es `dynamic`, la operación `E as T` genera el mismo resultado que
```csharp
E is T ? (T)(E) : (T)null
```
salvo que `E` solo se evalúa una vez. Se puede esperar que el compilador optimice `E as T` para realizar como máximo una comprobación de tipos dinámicos en lugar de las dos comprobaciones de tipos dinámicos implícitas por la expansión anterior.

Si el tipo en tiempo de compilación de `E` es `dynamic`, a diferencia del operador de conversión, el operador `as` no está enlazado dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). Por lo tanto, la expansión en este caso es:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Tenga en cuenta que algunas conversiones, como las conversiones definidas por el usuario, no son posibles con el operador `as` y, en su lugar, deben realizarse mediante expresiones de conversión.

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
se sabe que el parámetro de tipo `T` de `G` es un tipo de referencia, porque tiene la restricción de clase. Sin embargo, el parámetro de tipo `U` de `H` no es; por lo tanto, no se permite el uso del operador `as` en `H`.

## <a name="logical-operators"></a>Operadores lógicos

Los operadores `&`, `^` y `|` se denominan operadores lógicos.

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

Si un operando de un operador lógico tiene el tipo en tiempo de compilación `dynamic`, la expresión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso, el tipo en tiempo de compilación de la expresión es `dynamic`, y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución de los operandos que tienen el tipo en tiempo de compilación `dynamic`.

En el caso de una operación con el formato `x op y`, donde `op` es uno de los operadores lógicos, se aplica la resolución de sobrecarga ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) para seleccionar una implementación de operador específica. Los operandos se convierten en los tipos de parámetro del operador seleccionado y el tipo del resultado es el tipo de valor devuelto del operador.

En las secciones siguientes se describen los operadores lógicos predefinidos.

### <a name="integer-logical-operators"></a>Operadores lógicos enteros

Los operadores lógicos de enteros predefinidos son:
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

El operador `&` calcula el @no__t lógico bit a bit de los dos operandos, el operador `|` calcula el @no__t lógico bit a bit de los dos operandos y el operador `^` calcula la @no__t lógica exclusiva bit a bit de los dos operandos. No es posible realizar desbordamientos en estas operaciones.

### <a name="enumeration-logical-operators"></a>Operadores lógicos de enumeración

Cada tipo de enumeración `E` proporciona implícitamente los siguientes operadores lógicos predefinidos:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

El resultado de evaluar `x op y`, donde `x` y `y` son expresiones de un tipo de enumeración `E` con un tipo subyacente `U` y `op` es uno de los operadores lógicos, es exactamente igual que evaluar `(E)((U)x op (U)y)`. En otras palabras, los operadores lógicos de tipo de enumeración simplemente realizan la operación lógica en el tipo subyacente de los dos operandos.

### <a name="boolean-logical-operators"></a>Operadores lógicos booleanos

Los operadores lógicos booleanos predefinidos son:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

El resultado de `x & y` es `true` si tanto `x` como `y` son `true`. De lo contrario, el resultado es `false`.

El resultado de `x | y` es `true` si `x` o `y` es `true`. De lo contrario, el resultado es `false`.

El resultado de `x ^ y` es `true` si @no__t 2 es `true` y `y` es `false`, o `x` es `false` y `y` es `true`. De lo contrario, el resultado es `false`. Cuando los operandos son del tipo `bool`, el operador `^` calcula el mismo resultado que el operador `!=`.

### <a name="nullable-boolean-logical-operators"></a>Operadores lógicos booleanos que aceptan valores NULL

El tipo booleano que acepta valores NULL `bool?` puede representar tres valores, `true`, `false` y `null`, y es conceptualmente similar al tipo de tres valores que se usa para las Expresiones booleanas en SQL. Para asegurarse de que los resultados generados por los operadores `&` y `|` para los operandos `bool?` son coherentes con la lógica de tres valores de SQL, se proporcionan los siguientes operadores predefinidos:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

En la tabla siguiente se enumeran los resultados generados por estos operadores para todas las combinaciones de los valores `true`, `false` y `null`.

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

Los operadores `&&` y `||` se denominan operadores lógicos condicionales. También se denominan operadores lógicos "de cortocircuito".

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

Los operadores `&&` y `||` son versiones condicionales de los operadores @no__t 2 y `|`:

*  La operación `x && y` corresponde a la operación `x & y`, salvo que `y` solo se evalúa si `x` no es `false`.
*  La operación `x || y` corresponde a la operación `x | y`, salvo que `y` solo se evalúa si `x` no es `true`.

Si un operando de un operador lógico condicional tiene el tipo en tiempo de compilación `dynamic`, la expresión está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso, el tipo en tiempo de compilación de la expresión es `dynamic`, y la resolución que se describe a continuación se realiza en tiempo de ejecución mediante el tipo en tiempo de ejecución de los operandos que tienen el tipo en tiempo de compilación `dynamic`.

Una operación con el formato `x && y` o `x || y` se procesa aplicando la resolución de sobrecarga ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) como si la operación estuviera escrita `x & y` o `x | y`. A

*  Si la resolución de sobrecarga no encuentra un único operador mejor, o si la resolución de sobrecarga selecciona uno de los operadores lógicos de enteros predefinidos, se produce un error en tiempo de enlace.
*  De lo contrario, si el operador seleccionado es uno de los operadores lógicos booleanos predefinidos (operadores[lógicos booleanos](expressions.md#boolean-logical-operators)) o los operadores lógicos booleanos que aceptan valores NULL ([operadores lógicos booleanos que aceptan valores NULL](expressions.md#nullable-boolean-logical-operators)), la operación se procesa como se describe en [ Operadores lógicos condicionales booleanos](expressions.md#boolean-conditional-logical-operators).
*  De lo contrario, el operador seleccionado es un operador definido por el usuario y la operación se procesa como se describe en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators).

No es posible sobrecargar directamente los operadores lógicos condicionales. Sin embargo, dado que los operadores lógicos condicionales se evalúan en términos de los operadores lógicos normales, las sobrecargas de los operadores lógicos normales son, con ciertas restricciones, que también se consideran sobrecargas de los operadores lógicos condicionales. Esto se describe con más detalle en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators).

### <a name="boolean-conditional-logical-operators"></a>Operadores lógicos condicionales booleanos

Cuando los operandos de `&&` o `||` son del tipo `bool`, o cuando los operandos son de tipos que no definen un `operator &` o `operator |` aplicable, pero definen conversiones implícitas a `bool`, la operación se procesa de la siguiente manera: :

*  La operación `x && y` se evalúa como `x ? y : false`. En otras palabras, `x` se evalúa primero y se convierte al tipo `bool`. Después, si `x` es `true`, `y` se evalúa y se convierte al tipo `bool`, y esto se convierte en el resultado de la operación. De lo contrario, el resultado de la operación es `false`.
*  La operación `x || y` se evalúa como `x ? true : y`. En otras palabras, `x` se evalúa primero y se convierte al tipo `bool`. A continuación, si `x` es `true`, el resultado de la operación es @no__t 2. De lo contrario, se evalúa `y` y se convierte al tipo `bool`, lo que se convierte en el resultado de la operación.

### <a name="user-defined-conditional-logical-operators"></a>Operadores lógicos condicionales definidos por el usuario

Cuando los operandos de `&&` o `||` son de tipos que declaran un @no__t válido definido por el usuario o `operator |`, deben cumplirse las dos condiciones siguientes, donde `T` es el tipo en el que se declara el operador seleccionado:

*  El tipo de valor devuelto y el tipo de cada parámetro del operador seleccionado deben ser `T`. En otras palabras, el operador debe calcular el @no__t lógico-0 o el @no__t lógico-1 de dos operandos de tipo `T` y debe devolver un resultado de tipo `T`.
*  `T` debe contener declaraciones de `operator true` y `operator false`.

Se produce un error en tiempo de enlace si no se cumple alguno de estos requisitos. De lo contrario, la operación `&&` o `||` se evalúa combinando el @no__t 2 o el `operator false` definidos por el usuario con el operador definido por el usuario seleccionado:

*  La operación `x && y` se evalúa como `T.false(x) ? x : T.&(x, y)`, donde `T.false(x)` es una invocación del `operator false` declarado en `T` y `T.&(x, y)` es una invocación del `operator &` seleccionado. En otras palabras, primero se evalúa `x` y se invoca `operator false` en el resultado para determinar si `x` es definitivamente false. Después, si `x` es definitivamente false, el resultado de la operación es el valor previamente calculado para `x`. De lo contrario, se evalúa `y` y se invoca el `operator &` seleccionado en el valor calculado anteriormente para `x` y el valor calculado para `y` para generar el resultado de la operación.
*  La operación `x || y` se evalúa como `T.true(x) ? x : T.|(x, y)`, donde `T.true(x)` es una invocación del `operator true` declarado en `T` y `T.|(x,y)` es una invocación del `operator|` seleccionado. En otras palabras, primero se evalúa `x` y se invoca `operator true` en el resultado para determinar si `x` es definitivamente true. Después, si `x` es definitivamente true, el resultado de la operación es el valor previamente calculado para `x`. De lo contrario, se evalúa `y` y se invoca el `operator |` seleccionado en el valor calculado anteriormente para `x` y el valor calculado para `y` para generar el resultado de la operación.

En cualquiera de estas operaciones, la expresión proporcionada por `x` solo se evalúa una vez, y la expresión proporcionada por `y` no se evalúa ni se evalúa exactamente una vez.

Para obtener un ejemplo de un tipo que implementa `operator true` y `operator false`, vea [Database Boolean Type](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>Operador de uso combinado de null

El operador `??` se denomina operador de uso combinado de NULL.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Una expresión de fusión nula con el formato `a ?? b` requiere que `a` sea de un tipo que acepte valores NULL o un tipo de referencia. Si `a` no es null, el resultado de `a ?? b` es `a`; de lo contrario, el resultado es `b`. La operación evalúa `b` solo si `a` es NULL.

El operador de uso combinado de NULL es asociativo a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda. Por ejemplo, una expresión con el formato `a ?? b ?? c` se evalúa como `a ?? (b ?? c)`. En términos generales, una expresión con el formato `E1 ?? E2 ?? ... ?? En` devuelve el primero de los operandos que no son NULL, o null si todos los operandos son NULL.

El tipo de la expresión `a ?? b` depende de las conversiones implícitas que están disponibles en los operandos. En orden de preferencia, el tipo de `a ?? b` es `A0`, `A` o `B`, donde `A` es el tipo de `a` (siempre que `a` tenga un tipo), `B` es el tipo de `b` (siempre que `b` tenga un tipo). y 0 es el tipo subyacente de 1 si 2 es un tipo que acepta valores NULL o 3 en caso contrario. En concreto, `a ?? b` se procesa como se indica a continuación:

*  Si `A` existe y no es un tipo que acepta valores NULL o un tipo de referencia, se produce un error en tiempo de compilación.
*  Si `b` es una expresión dinámica, el tipo de resultado es `dynamic`. En tiempo de ejecución, se evalúa primero `a`. Si `a` no es null, `a` se convierte en dinámico y se convierte en el resultado. De lo contrario, se evalúa `b` y se convierte en el resultado.
*  De lo contrario, si `A` existe y es un tipo que acepta valores NULL y existe una conversión implícita de `b` a `A0`, el tipo de resultado es `A0`. En tiempo de ejecución, se evalúa primero `a`. Si `a` no es null, `a` se desencapsulará en el tipo `A0` y se convertirá en el resultado. De lo contrario, se evalúa `b` y se convierte al tipo `A0`, lo que se convierte en el resultado.
*  De lo contrario, si `A` existe y existe una conversión implícita de `b` a `A`, el tipo de resultado es `A`. En tiempo de ejecución, se evalúa primero `a`. Si `a` no es null, `a` se convierte en el resultado. De lo contrario, se evalúa `b` y se convierte al tipo `A`, lo que se convierte en el resultado.
*  De lo contrario, si `b` tiene un tipo `B` y existe una conversión implícita de `a` a `B`, el tipo de resultado es `B`. En tiempo de ejecución, se evalúa primero `a`. Si `a` no es null, `a` no se ajusta al tipo `A0` (si @no__t existe y acepta valores NULL) y se convierte al tipo `B`, y esto se convierte en el resultado. De lo contrario, se evalúa `b` y se convierte en el resultado.
*  De lo contrario, `a` y `b` son incompatibles y se produce un error en tiempo de compilación.

## <a name="conditional-operator"></a>Operador condicional

El operador `?:` se denomina operador condicional. En ocasiones también se denomina operador ternario.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Una expresión condicional con el formato `b ? x : y` primero evalúa la condición `b`. A continuación, si `b` es `true`, se evalúa @no__t 2 y se convierte en el resultado de la operación. De lo contrario, se evalúa `y` y se convierte en el resultado de la operación. Una expresión condicional nunca evalúa `x` y `y`.

El operador condicional es asociativo a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda. Por ejemplo, una expresión con el formato `a ? b : c ? d : e` se evalúa como `a ? b : (c ? d : e)`.

El primer operando del operador `?:` debe ser una expresión que se pueda convertir implícitamente a `bool`, o una expresión de un tipo que implementa `operator true`. Si no se cumple ninguno de estos requisitos, se produce un error en tiempo de compilación.

Los operandos segundo y tercero, `x` y `y`, del operador `?:` controlan el tipo de la expresión condicional.

*  Si `x` tiene el tipo `X` y `y` tiene el tipo `Y`, entonces
   * Si existe una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) de `X` a `Y`, pero no de `Y` a `X`, `Y` es el tipo de la expresión condicional.
   * Si existe una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) de `Y` a `X`, pero no de `X` a `Y`, `X` es el tipo de la expresión condicional.
   * De lo contrario, no se puede determinar ningún tipo de expresión y se produce un error en tiempo de compilación.
*  Si solo uno de `x` y `y` tiene un tipo, y `x` y `y`, de se pueden convertir implícitamente a ese tipo, es decir, el tipo de la expresión condicional.
*  De lo contrario, no se puede determinar ningún tipo de expresión y se produce un error en tiempo de compilación.

El procesamiento en tiempo de ejecución de una expresión condicional con el formato `b ? x : y` consta de los siguientes pasos:

*  En primer lugar, se evalúa `b` y se determina el valor `bool` de `b`:
   * Si existe una conversión implícita del tipo de `b` a `bool`, esta conversión implícita se realiza para generar un valor `bool`.
   * De lo contrario, se invoca el `operator true` definido por el tipo de `b` para generar un valor `bool`.
*  Si el valor `bool` producido por el paso anterior es `true`, `x` se evalúa y se convierte al tipo de la expresión condicional, y esto se convierte en el resultado de la expresión condicional.
*  De lo contrario, se evalúa `y` y se convierte al tipo de la expresión condicional, lo que se convierte en el resultado de la expresión condicional.

## <a name="anonymous-function-expressions"></a>Expresiones de función anónima

Una ***función anónima*** es una expresión que representa una definición de método "en línea". Una función anónima no tiene un valor o un tipo en y de sí mismo, pero se pueden convertir en un tipo de árbol de expresión o delegado compatible. La evaluación de una conversión de función anónima depende del tipo de destino de la conversión: Si es un tipo de delegado, la conversión se evalúa como un valor de delegado que hace referencia al método que define la función anónima. Si es un tipo de árbol de expresión, la conversión se evalúa como un árbol de expresión que representa la estructura del método como una estructura de objeto.

Por motivos históricos, hay dos tipos sintácticos de funciones anónimas, es decir, *lambda_expression*s y *anonymous_method_expression*s. Para casi todos los propósitos, *lambda_expression*s son más concisos y expresivos que *anonymous_method_expression*s, que permanecen en el lenguaje por compatibilidad con versiones anteriores.

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

El operador `=>` tiene la misma prioridad que la asignación (`=`) y es asociativo a la derecha.

Una función anónima con el modificador `async` es una función asincrónica y sigue las reglas descritas en [iteradores](classes.md#iterators).

Los parámetros de una función anónima en forma de *lambda_expression* se pueden escribir explícita o implícitamente. En una lista de parámetros con tipo explícito, se indica explícitamente el tipo de cada parámetro. En una lista de parámetros con tipos implícitos, los tipos de los parámetros se deducen del contexto en el que se produce la función anónima; en concreto, cuando la función anónima se convierte en un tipo de delegado o un tipo de árbol de expresión compatible, ese tipo proporciona los tipos de parámetro ([conversiones de funciones anónimas](conversions.md#anonymous-function-conversions)).

En una función anónima con un solo parámetro con tipo implícito, los paréntesis se pueden omitir en la lista de parámetros. En otras palabras, una función anónima con el formato
```csharp
( param ) => expr
```
se puede abreviar como
```csharp
param => expr
```

La lista de parámetros de una función anónima en forma de *anonymous_method_expression* es opcional. Si se especifica, los parámetros se deben escribir explícitamente. En caso contrario, la función anónima se convertirá en un delegado con cualquier lista de parámetros que no contenga @no__t parámetros-0.

Se puede tener acceso al cuerpo de un *bloque* de una función anónima ([puntos de conexión y disponibilidad](statements.md#end-points-and-reachability)) a menos que la función anónima se encuentre dentro de una instrucción inalcanzable.

A continuación se muestran algunos ejemplos de funciones anónimas:

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

El comportamiento de *lambda_expression*s y *anonymous_method_expression*s es el mismo, salvo los siguientes puntos:

*  *anonymous_method_expression*s permite omitir completamente la lista de parámetros, lo que produce Convertibility para delegar tipos de cualquier lista de parámetros de valor.
*  *lambda_expression*s permite omitir y deducir los tipos de parámetro, mientras que *anonymous_method_expression*s requiere que los tipos de parámetro se indiquen explícitamente.
*  El cuerpo de una *lambda_expression* puede ser una expresión o un bloque de instrucciones, mientras que el cuerpo de un *anonymous_method_expression* debe ser un bloque de instrucciones.
*  Solo *lambda_expression*s tienen conversiones a tipos de árbol de expresión compatibles ([tipos de árbol de expresión](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Firmas de funciones anónimas

El *anonymous_function_signature* opcional de una función anónima define los nombres y, opcionalmente, los tipos de los parámetros formales de la función anónima. El ámbito de los parámetros de la función anónima es *anonymous_function_body*. ([Ámbitos](basic-concepts.md#scopes)) Junto con la lista de parámetros (si se especifica), el cuerpo del método anónimo constituye un espacio de declaración ([declaraciones](basic-concepts.md#declarations)). Por lo tanto, es un error en tiempo de compilación que el nombre de un parámetro de la función anónima coincida con el nombre de una variable local, una constante local o un parámetro cuyo ámbito incluya *anonymous_method_expression* o *lambda_expression*.

Si una función anónima tiene un *explicit_anonymous_function_signature*, el conjunto de tipos de delegado compatibles y los tipos de árbol de expresión están restringidos a los que tienen los mismos tipos de parámetros y modificadores en el mismo orden. A diferencia de las conversiones de grupos de métodos ([conversiones de grupos de métodos](conversions.md#method-group-conversions)), no se admite la varianza de tipos de parámetros de función anónimos. Si una función anónima no tiene un *anonymous_function_signature*, el conjunto de tipos de delegado compatibles y los tipos de árbol de expresión está restringido a los que no tienen `out` parámetros.

Tenga en cuenta que un *anonymous_function_signature* no puede incluir atributos ni una matriz de parámetros. No obstante, un *anonymous_function_signature* puede ser compatible con un tipo de delegado cuya lista de parámetros contiene una matriz de parámetros.

Tenga en cuenta también que la conversión a un tipo de árbol de expresión, incluso si es compatible, todavía puede producir un error en tiempo de compilación ([tipos de árbol de expresión](types.md#expression-tree-types)).

### <a name="anonymous-function-bodies"></a>Cuerpos de función anónimos

El cuerpo (*expresión* o *bloque*) de una función anónima está sujeto a las siguientes reglas:

*  Si la función anónima incluye una firma, los parámetros especificados en la firma están disponibles en el cuerpo. Si la función anónima no tiene ninguna firma, se puede convertir en un tipo de delegado o tipo de expresión con parámetros ([conversiones de funciones anónimas](conversions.md#anonymous-function-conversions)), pero no se puede tener acceso a los parámetros en el cuerpo.
*  Excepto en el caso de los parámetros `ref` o `out` especificados en la firma (si existe) de la función anónima envolvente más cercana, se trata de un error en tiempo de compilación para que el cuerpo tenga acceso a un parámetro `ref` o `out`.
*  Cuando el tipo de `this` es un tipo de estructura, se trata de un error en tiempo de compilación para que el cuerpo tenga acceso a `this`. Esto es así si el acceso es explícito (como en `this.x`) o implícito (como en `x` donde `x` es un miembro de instancia de la estructura). Esta regla simplemente prohíbe el acceso y no afecta a si la búsqueda de miembros da como resultado un miembro de la estructura.
*  El cuerpo tiene acceso a las variables externas ([variables externas](expressions.md#outer-variables)) de la función anónima. El acceso de una variable externa hará referencia a la instancia de la variable que está activa en el momento en que se evalúa *lambda_expression* o *anonymous_method_expression* ([evaluación de expresiones de función anónimas](expressions.md#evaluation-of-anonymous-function-expressions)).
*  Se trata de un error en tiempo de compilación para que el cuerpo contenga una instrucción `goto`, una instrucción `break` o una instrucción `continue` cuyo destino esté fuera del cuerpo o dentro del cuerpo de una función anónima contenida.
*  Una instrucción `return` en el cuerpo devuelve el control de una invocación de la función anónima envolvente más cercana, no del miembro de función envolvente. Una expresión especificada en una instrucción `return` debe poder convertirse implícitamente al tipo de valor devuelto del tipo de delegado o del tipo de árbol de expresión al que se convierte el *lambda_expression* o *anonymous_method_expression* más próximo ( [Conversiones de funciones anónimas](conversions.md#anonymous-function-conversions)).

No se especifica explícitamente si hay alguna manera de ejecutar el bloque de una función anónima que no sea a través de la evaluación y la invocación de *lambda_expression* o *anonymous_method_expression*. En concreto, el compilador puede optar por implementar una función anónima mediante la sintetización de uno o varios métodos o tipos con nombre. Los nombres de estos elementos sintetizados deben tener un formato reservado para el uso del compilador.

### <a name="overload-resolution-and-anonymous-functions"></a>Resolución de sobrecarga y funciones anónimas

Las funciones anónimas de una lista de argumentos participan en la inferencia de tipos y en la resolución de sobrecarga. Consulte la [inferencia de tipos](expressions.md#type-inference) y la [resolución de sobrecargas](expressions.md#overload-resolution) para ver las reglas exactas.

En el ejemplo siguiente se muestra el efecto de las funciones anónimas en la resolución de sobrecarga.

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

La clase `ItemList<T>` tiene dos métodos `Sum`. Cada toma un argumento `selector`, que extrae el valor que se va a sumar de un elemento de lista. El valor extraído puede ser un `int` o un `double` y la suma resultante es igualmente una `int` o un `double`.

Los métodos `Sum` podrían usarse, por ejemplo, para calcular las sumas de una lista de líneas de detalles de un pedido.

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

En la primera invocación de `orderDetails.Sum`, ambos métodos `Sum` son aplicables porque la función anónima `d => d. UnitCount` es compatible con `Func<Detail,int>` y `Func<Detail,double>`. Sin embargo, la resolución de sobrecarga selecciona el primer método `Sum` porque la conversión a `Func<Detail,int>` es mejor que la conversión a `Func<Detail,double>`.

En la segunda invocación de `orderDetails.Sum`, solo se puede aplicar el segundo método `Sum` porque la función anónima `d => d.UnitPrice * d.UnitCount` genera un valor de tipo `double`. Por lo tanto, la resolución de sobrecarga elige el segundo método `Sum` para esa invocación.

### <a name="anonymous-functions-and-dynamic-binding"></a>Funciones anónimas y enlace dinámico

Una función anónima no puede ser un receptor, un argumento o un operando de una operación enlazada dinámicamente.

### <a name="outer-variables"></a>Variables externas

Cualquier variable local, parámetro de valor o matriz de parámetros cuyo ámbito incluya *lambda_expression* o *anonymous_method_expression* se denomina una ***variable externa*** de la función anónima. En un miembro de función de instancia de una clase, el valor `this` se considera un parámetro de valor y es una variable externa de cualquier función anónima incluida en el miembro de función.

#### <a name="captured-outer-variables"></a>Variables externas capturadas

Cuando una función anónima hace referencia a una variable externa, se dice que la función anónima ha ***capturado*** la variable externa. Normalmente, la duración de una variable local se limita a la ejecución del bloque o la instrucción con la que está asociada ([variables locales](variables.md#local-variables)). Sin embargo, la duración de una variable externa capturada se extiende al menos hasta que el delegado o el árbol de expresión creado a partir de la función anónima sea válido para la recolección de elementos no utilizados.

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
la función anónima captura la variable local `x`, y la duración de `x` se extiende al menos hasta que el delegado devuelto de `F` sea válido para la recolección de elementos no utilizados (que no se produce hasta el final del programa). Dado que cada invocación de la función anónima funciona en la misma instancia de `x`, el resultado del ejemplo es:
```console
1
2
3
```

Cuando una función anónima captura una variable local o un parámetro de valor, la variable local o el parámetro ya no se considera una variable fija ([variables fijas y móviles](unsafe-code.md#fixed-and-moveable-variables)), sino que se considera una variable móvil. Por lo tanto, cualquier código `unsafe` que toma la dirección de una variable externa capturada debe usar primero la instrucción `fixed` para corregir la variable.

Tenga en cuenta que, a diferencia de una variable no capturada, una variable local capturada se puede exponer simultáneamente a varios subprocesos de ejecución.

#### <a name="instantiation-of-local-variables"></a>Creación de instancias de variables locales

Se considera que se ***crea una instancia*** de una variable local cuando la ejecución entra en el ámbito de la variable. Por ejemplo, cuando se invoca el método siguiente, se crea una instancia de la variable local `x` y se inicializa tres veces, una vez para cada iteración del bucle.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Sin embargo, si se mueve la declaración de `x` fuera del bucle, se produce una única creación de instancias de `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Cuando no se capturan, no hay forma de observar exactamente la frecuencia con la que se crea una instancia de una variable local, ya que las duraciones de las instancias no se pueden usar para que cada creación de instancias use simplemente la misma ubicación de almacenamiento. Sin embargo, cuando una función anónima captura una variable local, los efectos de la creación de instancias se hacen evidentes.

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
```console
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
```console
5
5
5
```

Si un bucle for declara una variable de iteración, se considera que esa variable se declara fuera del bucle. Por lo tanto, si se cambia el ejemplo para capturar la variable de iteración en sí:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
solo se captura una instancia de la variable de iteración, que genera el resultado:
```console
3
3
3
```

Es posible que los delegados de función anónimos compartan algunas variables capturadas pero tengan instancias independientes de otras. Por ejemplo, si se cambia `F` a
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
los tres delegados capturan la misma instancia de `x` pero instancias independientes de `y` y el resultado es:
```console
1 1
2 1
3 1
```

Las funciones anónimas independientes pueden capturar la misma instancia de una variable externa. En el ejemplo:
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
las dos funciones anónimas capturan la misma instancia de la variable local `x` y, por lo tanto, pueden "comunicarse" a través de esa variable. La salida del ejemplo es:
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Evaluación de expresiones de función anónimas

Una función anónima `F` siempre debe convertirse en un tipo de delegado `D` o en un tipo de árbol de expresión `E`, ya sea directamente o mediante la ejecución de una expresión de creación de delegado `new D(F)`. Esta conversión determina el resultado de la función anónima, como se describe en [conversiones de funciones anónimas](conversions.md#anonymous-function-conversions).

## <a name="query-expressions"></a>Expresiones de consulta

Las ***expresiones de consulta*** proporcionan una sintaxis integrada de lenguaje para las consultas que es similar a los lenguajes de consulta jerárquica y relacional, como SQL y XQuery.

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

Una expresión de consulta comienza con una cláusula `from` y termina con una cláusula `select` o `group`. La cláusula `from` inicial puede ir seguida de cero o más `from`, `let`, `where`, `join` o `orderby`. Cada cláusula `from` es un generador que introduce una ***variable de rango*** que abarca los elementos de una ***secuencia***. Cada cláusula `let` introduce una variable de rango que representa un valor calculado por medio de las variables de rango anteriores. Cada cláusula `where` es un filtro que excluye elementos del resultado. Cada cláusula `join` compara las claves especificadas de la secuencia de origen con claves de otra secuencia, lo que produce pares coincidentes. Cada cláusula `orderby` reordena los elementos según los criterios especificados. La cláusula final `select` o `group` especifica la forma del resultado en términos de las variables de rango. Por último, se puede usar una cláusula `into` para "Insertar" consultas mediante el tratamiento de los resultados de una consulta como un generador en una consulta posterior.

### <a name="ambiguities-in-query-expressions"></a>Ambigüedades en expresiones de consulta

Las expresiones de consulta contienen una serie de "palabras clave contextuales", es decir, identificadores que tienen un significado especial en un contexto determinado. En concreto, se trata de `from`, `where`, @no__t 2, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, 0, 1 y 2. Para evitar ambigüedades en las expresiones de consulta producidas por el uso mixto de estos identificadores como palabras clave o nombres simples, estos identificadores se consideran palabras clave cuando se producen en cualquier parte de una expresión de consulta.

Para este propósito, una expresión de consulta es cualquier expresión que empiece por "`from identifier`" seguida de cualquier token excepto "`;`", "`=`" o "`,`".

Para usar estas palabras como identificadores dentro de una expresión de consulta, se les puede anteponer "`@`" ([identificadores](lexical-structure.md#identifiers)).

### <a name="query-expression-translation"></a>Traducción de expresiones de consulta

El C# lenguaje no especifica la semántica de ejecución de las expresiones de consulta. En su lugar, las expresiones de consulta se convierten en invocaciones de métodos que se adhieren al *patrón de expresión de consulta* ([el patrón de expresión de consulta](expressions.md#the-query-expression-pattern)). En concreto, las expresiones de consulta se convierten en invocaciones de métodos denominados `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy` y 0. Se espera que estos métodos tengan firmas concretas y tipos de resultado, como se describe en [el patrón de expresión de consulta](expressions.md#the-query-expression-pattern). Estos métodos pueden ser métodos de instancia del objeto que se consulta o métodos de extensión que son externos al objeto e implementan la ejecución real de la consulta.

La conversión de las expresiones de consulta a las invocaciones de método es una asignación sintáctica que se produce antes de que se haya realizado cualquier enlace de tipo o resolución de sobrecarga. Se garantiza que la conversión es sintácticamente correcta, pero no se garantiza que genere código correcto C# semánticamente. Después de la traducción de las expresiones de consulta, las invocaciones de método resultantes se procesan como invocaciones de método regulares y esto puede a su vez detectar errores, por ejemplo, si los métodos no existen, si los argumentos tienen tipos incorrectos, o si los métodos son genéricos y se produce un error en la inferencia de tipos.

Una expresión de consulta se procesa mediante la aplicación repetida de las siguientes traducciones hasta que no se puedan realizar más reducciones. Las traducciones se muestran por orden de aplicación: cada sección presupone que las traducciones de las secciones anteriores se han realizado de forma exhaustiva y, una vez agotadas, una sección no se volverá a visitar posteriormente en el procesamiento de la misma expresión de consulta.

No se permite la asignación a variables de rango en expresiones de consulta. Sin embargo C# , se permite que una implementación no siempre aplique esta restricción, ya que a veces esto no es posible con el esquema de traducción sintáctica que se muestra aquí.

Ciertas traducciones insertan variables de rango con identificadores transparentes indicados por `*`. Las propiedades especiales de los identificadores transparentes se tratan en profundidad en los [identificadores transparentes](expressions.md#transparent-identifiers).

#### <a name="select-and-groupby-clauses-with-continuations"></a>Cláusulas SELECT y GroupBy con continuaciones

Expresión de consulta con una continuación
```csharp
from ... into x ...
```
se traduce en
```csharp
from x in ( from ... ) ...
```

Las traducciones de las secciones siguientes suponen que las consultas no tienen ninguna continuación `into`.

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
la traducción final que es
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Tipos de variables de rango explícitas

Una cláusula `from` que especifica explícitamente un tipo de variable de rango
```csharp
from T x in e
```
se traduce en
```csharp
from x in ( e ) . Cast < T > ( )
```

Una cláusula `join` que especifica explícitamente un tipo de variable de rango
```csharp
join T x in e on k1 equals k2
```
se traduce en
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

Las traducciones de las secciones siguientes suponen que las consultas no tienen tipos de variable de intervalo explícitos.

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
la traducción final que es
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Los tipos de variables de rango explícitos son útiles para consultar colecciones que implementan la interfaz `IEnumerable` no genérica, pero no la interfaz genérica de `IEnumerable<T>`. En el ejemplo anterior, este sería el caso si `customers` fuera del tipo `ArrayList`.

#### <a name="degenerate-query-expressions"></a>Expresiones de consulta degeneradas

Una expresión de consulta con el formato
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

Una expresión de consulta degenerada es aquella que selecciona trivialmente los elementos del origen. Una fase posterior de la traducción quita las consultas degeneradas que se introdujeron en otros pasos de traducción mediante su origen. Sin embargo, es importante asegurarse de que el resultado de una expresión de consulta nunca sea el propio objeto de origen, ya que esto revelaría el tipo e identidad del origen al cliente de la consulta. Por lo tanto, este paso protege las consultas degeneradas escritas directamente en el código fuente mediante una llamada explícita a `Select` en el origen. A continuación, llega a los implementadores de `Select` y otros operadores de consulta para asegurarse de que estos métodos nunca devuelven el propio objeto de origen.

#### <a name="from-let-where-join-and-orderby-clauses"></a>Cláusulas from, Let, Where, join y OrderBy

Una expresión de consulta con una segunda cláusula `from` seguida de una cláusula `select`
```csharp
from x1 in e1
from x2 in e2
select v
```
se traduce en
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

Una expresión de consulta con una segunda cláusula `from` seguida de una cláusula `select`:

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

Una expresión de consulta con una cláusula `let`
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

Una expresión de consulta con una cláusula `where`
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

Una expresión de consulta con una cláusula `join` sin un `into` seguida de una cláusula `select`.
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
se traduce en
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Una expresión de consulta con una cláusula `join` sin un `into` seguido de un valor distinto de una cláusula `select`
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

Una expresión de consulta con una cláusula `join` con un `into` seguida de una cláusula `select`.
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
se traduce en
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Una expresión de consulta con una cláusula `join` con un `into` seguido de un valor distinto de una cláusula `select`
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

Una expresión de consulta con una cláusula `orderby`
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

Si una cláusula de ordenación especifica un indicador de dirección `descending`, se produce una invocación de `OrderByDescending` o `ThenByDescending` en su lugar.

Las siguientes traducciones suponen que no hay ninguna cláusula `let`, `where`, `join` o `orderby`, y no más de una cláusula de `from` inicial en cada expresión de consulta.

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
la traducción final que es
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
donde `x` es un identificador generado por el compilador que, de lo contrario, es invisible e inaccesible.

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
la traducción final que es
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
donde `x` es un identificador generado por el compilador que, de lo contrario, es invisible e inaccesible.

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
la traducción final que es
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
donde `x` y `y` son identificadores generados por el compilador que, de lo contrario, son invisibles e inaccesibles.

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

#### <a name="select-clauses"></a>Cláusulas Select

Una expresión de consulta con el formato
```csharp
from x in e select v
```
se traduce en
```csharp
( e ) . Select ( x => v )
```
excepto cuando v es el identificador x, la traducción es simplemente
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

#### <a name="groupby-clauses"></a>Cláusulas GroupBy

Una expresión de consulta con el formato
```csharp
from x in e group v by k
```
se traduce en
```csharp
( e ) . GroupBy ( x => k , x => v )
```
excepto cuando v es el identificador x, la traducción es
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

Ciertas traducciones insertan variables de rango con ***identificadores transparentes*** indicados por `*`. Los identificadores transparentes no son una característica de lenguaje adecuada; solo existen como un paso intermedio en el proceso de conversión de expresiones de consulta.

Cuando una traducción de consultas inserta un identificador transparente, los pasos de traducción adicionales propagan el identificador transparente en funciones anónimas e inicializadores de objeto anónimos. En esos contextos, los identificadores transparentes tienen el siguiente comportamiento:

*  Cuando un identificador transparente se produce como un parámetro en una función anónima, los miembros del tipo anónimo asociado están automáticamente en el ámbito del cuerpo de la función anónima.
*  Cuando un miembro con un identificador transparente está en el ámbito, los miembros de ese miembro están también en el ámbito.
*  Cuando un identificador transparente se produce como un declarador de miembro en un inicializador de objeto anónimo, introduce un miembro con un identificador transparente.
*  En los pasos de traducción descritos anteriormente, los identificadores transparentes siempre se introducen junto con los tipos anónimos, con la intención de capturar varias variables de rango como miembros de un solo objeto. Una implementación de C# puede utilizar un mecanismo diferente que los tipos anónimos para agrupar varias variables de rango. Los siguientes ejemplos de traducción suponen que se usan tipos anónimos y muestran cómo se pueden traducir los identificadores transparentes.

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

que se traduce en
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
donde `x` es un identificador generado por el compilador que, de lo contrario, es invisible e inaccesible.

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
se reduce aún más a
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
la traducción final que es
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
donde `x`, `y` y `z` son identificadores generados por el compilador que, de lo contrario, son invisibles e inaccesibles.

### <a name="the-query-expression-pattern"></a>Patrón de expresión de consulta

El ***patrón de expresión de consulta*** establece un patrón de métodos que los tipos pueden implementar para admitir expresiones de consulta. Dado que las expresiones de consulta se convierten en invocaciones de método por medio de una asignación sintáctica, los tipos tienen una gran flexibilidad en cómo implementan el patrón de expresión de consulta. Por ejemplo, los métodos del patrón se pueden implementar como métodos de instancia o como métodos de extensión porque los dos tienen la misma sintaxis de invocación y los métodos pueden solicitar delegados o árboles de expresión porque las funciones anónimas son convertibles a ambos.

A continuación se muestra la forma recomendada de un tipo genérico `C<T>` que admite el patrón de expresión de consulta. Se usa un tipo genérico para ilustrar las relaciones apropiadas entre los tipos de parámetro y de resultado, pero también es posible implementar el patrón para tipos no genéricos.

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

Los métodos anteriores usan los tipos de delegado genérico `Func<T1,R>` y `Func<T1,T2,R>`, pero también podrían haber usado otros tipos de árbol de delegado o de expresión con las mismas relaciones en los tipos de parámetro y de resultado.

Observe la relación recomendada entre `C<T>` y `O<T>`, lo que garantiza que los métodos @no__t 2 y `ThenByDescending` solo están disponibles en el resultado de `OrderBy` o `OrderByDescending`. Observe también la forma recomendada del resultado de `GroupBy`--una secuencia de secuencias, donde cada secuencia interna tiene una propiedad `Key` adicional.

El espacio de nombres `System.Linq` proporciona una implementación del patrón de operador de consulta para cualquier tipo que implemente la interfaz `System.Collections.Generic.IEnumerable<T>`.

## <a name="assignment-operators"></a>Operadores de asignación

Los operadores de asignación asignan un nuevo valor a una variable, una propiedad, un evento o un elemento de indexador.

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

El operando izquierdo de una asignación debe ser una expresión clasificada como una variable, un acceso de propiedad, un acceso de indexador o un acceso de evento.

El operador `=` se denomina ***operador de asignación simple***. Asigna el valor del operando derecho a la variable, propiedad o elemento de indexador proporcionado por el operando izquierdo. Es posible que el operando izquierdo del operador de asignación simple no sea un acceso a eventos (excepto como se describe en [eventos similares a los de campo](classes.md#field-like-events)). El operador de asignación simple se describe en [asignación simple](expressions.md#simple-assignment).

Los operadores de asignación distintos del operador `=` se denominan ***operadores de asignación compuesta***. Estos operadores realizan la operación indicada en los dos operandos y, a continuación, asignan el valor resultante a la variable, la propiedad o el elemento indexador proporcionado por el operando izquierdo. Los operadores de asignación compuesta se describen en [asignación compuesta](expressions.md#compound-assignment).

Los operadores `+=` y `-=` con una expresión de acceso a eventos como operando izquierdo se denominan *operadores de asignación de eventos*. Ningún otro operador de asignación es válido con un acceso de evento como operando izquierdo. Los operadores de asignación de eventos se describen en [asignación de eventos](expressions.md#event-assignment).

Los operadores de asignación son asociativos a la derecha, lo que significa que las operaciones se agrupan de derecha a izquierda. Por ejemplo, una expresión con el formato `a = b = c` se evalúa como `a = (b = c)`.

### <a name="simple-assignment"></a>Asignación simple

El operador `=` se denomina operador de asignación simple.

Si el operando izquierdo de una asignación simple tiene el formato `E.P` o `E[Ei]`, donde @no__t 2 tiene el tipo en tiempo de compilación `dynamic`, la asignación está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso, el tipo en tiempo de compilación de la expresión de asignación es `dynamic`, y la resolución que se describe a continuación se realizará en tiempo de ejecución en función del tipo en tiempo de ejecución de `E`.

En una asignación simple, el operando derecho debe ser una expresión que se pueda convertir implícitamente al tipo del operando izquierdo. La operación asigna el valor del operando derecho a la variable, la propiedad o el elemento indexador proporcionado por el operando izquierdo.

El resultado de una expresión de asignación simple es el valor asignado al operando izquierdo. El resultado tiene el mismo tipo que el operando izquierdo y siempre se clasifica como un valor.

Si el operando izquierdo es una propiedad o un indexador, la propiedad o el indexador deben tener un descriptor de acceso `set`. Si no es así, se produce un error en tiempo de enlace.

El procesamiento en tiempo de ejecución de una asignación simple del formulario `x = y` consta de los siguientes pasos:

*  Si `x` está clasificado como una variable:
   * `x` se evalúa para generar la variable.
   * `y` se evalúa y, si es necesario, se convierte al tipo de `x` a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).
   * Si la variable proporcionada por `x` es un elemento de matriz de un *reference_type*, se realiza una comprobación en tiempo de ejecución para asegurarse de que el valor calculado para `y` es compatible con la instancia de matriz de la que `x` es un elemento. La comprobación se realiza correctamente si `y` es `null`, o si existe una conversión de referencia implícita ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions)) del tipo real de la instancia a la que hace referencia `y` en el tipo de elemento real de la instancia de matriz que contiene `x`. De lo contrario, se produce una excepción `System.ArrayTypeMismatchException`.
   * El valor resultante de la evaluación y conversión de `y` se almacena en la ubicación proporcionada por la evaluación de `x`.
*  Si `x` se clasifica como un acceso de propiedad o indizador:
   * La expresión de instancia (si `x` no es `static`) y la lista de argumentos (si `x` es un indexador) asociada a `x` se evalúan y los resultados se usan en la invocación del descriptor de acceso `set` subsiguiente.
   * `y` se evalúa y, si es necesario, se convierte al tipo de `x` a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).
   * El descriptor de acceso `set` de `x` se invoca con el valor calculado para `y` como su argumento `value`.

Las reglas de covarianza de matriz ([covarianza de matriz](arrays.md#array-covariance)) permiten que un valor de un tipo de matriz `A[]` sea una referencia a una instancia de un tipo de matriz `B[]`, siempre que exista una conversión de referencia implícita de `B` a `A`. Debido a estas reglas, la asignación a un elemento de matriz de una *reference_type* requiere una comprobación en tiempo de ejecución para asegurarse de que el valor que se está asignando es compatible con la instancia de la matriz. En el ejemplo
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
la última asignación provoca que se inicie una `System.ArrayTypeMismatchException` porque una instancia de `ArrayList` no puede almacenarse en un elemento de un `string[]`.

Cuando una propiedad o un indizador declarado en una *struct_type* es el destino de una asignación, la expresión de instancia asociada con el acceso a la propiedad o indizador debe estar clasificada como una variable. Si la expresión de instancia se clasifica como un valor, se produce un error en tiempo de enlace. Debido al [acceso a miembros](expressions.md#member-access), la misma regla también se aplica a los campos.

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
se permiten las asignaciones a `p.X`, `p.Y`, `r.A` y `r.B` porque `p` y `r` son variables. Sin embargo, en el ejemplo
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
las asignaciones no son válidas, ya que `r.A` y `r.B` no son variables.

### <a name="compound-assignment"></a>Asignación compuesta

Si el operando izquierdo de una asignación compuesta tiene el formato `E.P` o `E[Ei]`, donde @no__t 2 tiene el tipo en tiempo de compilación `dynamic`, la asignación está enlazada dinámicamente ([enlace dinámico](expressions.md#dynamic-binding)). En este caso, el tipo en tiempo de compilación de la expresión de asignación es `dynamic`, y la resolución que se describe a continuación se realizará en tiempo de ejecución en función del tipo en tiempo de ejecución de `E`.

Una operación con el formato `x op= y` se procesa aplicando la resolución de sobrecarga del operador binario ([resolución de sobrecarga del operador binario](expressions.md#binary-operator-overload-resolution)) como si la operación se hubiera escrito `x op y`. A

*  Si el tipo de valor devuelto del operador seleccionado es implícitamente convertible al tipo de `x`, la operación se evalúa como `x = x op y`, excepto en que `x` solo se evalúa una vez.
*  De lo contrario, si el operador seleccionado es un operador predefinido, si el tipo de valor devuelto del operador seleccionado es convertible explícitamente al tipo de `x`, y si `y` es implícitamente convertible al tipo de `x` o el operador es un operador de desplazamiento , la operación se evalúa como `x = (T)(x op y)`, donde `T` es el tipo de `x`, salvo que `x` se evalúa solo una vez.
*  De lo contrario, la asignación compuesta no es válida y se produce un error en tiempo de enlace.

El término "evaluado solo una vez" significa que en la evaluación de `x op y`, los resultados de cualquier expresión constitutiva de `x` se guardan temporalmente y se reutilizan al realizar la asignación a `x`. Por ejemplo, en la asignación `A()[B()] += C()`, donde `A` es un método que devuelve `int[]` y `B` y `C` son métodos que devuelven `int`, los métodos se invocan solo una vez, en el orden `A`, `B`, `C`.

Cuando el operando izquierdo de una asignación compuesta es un acceso a propiedad o a un indexador, la propiedad o el indexador deben tener un descriptor de acceso `get` y un descriptor de acceso `set`. Si no es así, se produce un error en tiempo de enlace.

La segunda regla anterior permite evaluar `x op= y` como `x = (T)(x op y)` en determinados contextos. La regla existe de forma que los operadores predefinidos se pueden usar como operadores compuestos cuando el operando izquierdo es de tipo `sbyte`, `byte`, `short`, `ushort` o `char`. Incluso cuando ambos argumentos son de uno de esos tipos, los operadores predefinidos producen un resultado de tipo `int`, tal y como se describe en [promociones numéricas binarias](expressions.md#binary-numeric-promotions). Por lo tanto, sin una conversión, no sería posible asignar el resultado al operando izquierdo.

El efecto intuitivo de la regla para los operadores predefinidos es simplemente que se permite `x op= y` si se permiten los dos `x op y` y `x = y`. En el ejemplo
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
la razón intuitiva de cada error es que una asignación simple correspondiente también habría sido un error.

Esto también significa que las operaciones de asignación compuesta admiten operaciones de elevación. En el ejemplo
```csharp
int? i = 0;
i += 1;             // Ok
```
se usa el operador de elevación `+(int?,int?)`.

### <a name="event-assignment"></a>Asignación de eventos

Si el operando izquierdo de un operador `+=` o `-=` se clasifica como un acceso de evento, la expresión se evalúa como sigue:

*  Expresión de instancia, si existe, del acceso al evento que se va a evaluar.
*  Se evalúa el operando derecho del operador `+=` o `-=` y, si es necesario, se convierte al tipo del operando izquierdo a través de una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).
*  Se invoca un descriptor de acceso de eventos del evento, con la lista de argumentos que consiste en el operando derecho, después de la evaluación y, si es necesario, conversión. Si el operador era `+=`, se invoca al descriptor de acceso `add`; Si el operador era `-=`, se invoca al descriptor de acceso `remove`.

Una expresión de asignación de eventos no produce un valor. Por lo tanto, una expresión de asignación de eventos solo es válida en el contexto de una *statement_expression* ([instrucciones de expresión](statements.md#expression-statements)).

## <a name="expression"></a>Expresión

Una *expresión* es un *non_assignment_expression* o una *asignación*.

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

Una expresión constante debe ser el literal `null` o un valor con uno de los siguientes tipos: `sbyte`, @no__t 2, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3, 4 , 5 o cualquier tipo de enumeración. Solo se permiten las siguientes construcciones en expresiones constantes:

*  Literales (incluido el literal `null`).
*  Referencias a miembros `const` de tipos de clase y estructura.
*  Referencias a miembros de tipos de enumeración.
*  Referencias a parámetros `const` o variables locales
*  Subexpresiones entre paréntesis, que son expresiones constantes.
*  Expresiones de conversión, siempre que el tipo de destino sea uno de los tipos enumerados anteriormente.
*  Expresiones `checked` y `unchecked`
*  Expresiones de valor predeterminado
*  Expresiones Name
*  Los operadores unarios predefinidos `+`, `-`, `!` y `~`.
*  El @no__t predefinido-0, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, 0, 1, 2, 3, 4, 5, 6 y 7 operadores binarios, siempre que cada operando sea de tipo enumerado anteriormente.
*  Operador condicional `?:`.

Las siguientes conversiones se permiten en expresiones constantes:

*  Conversiones de identidad
*  Conversiones numéricas
*  Conversiones de enumeración
*  Conversiones de expresiones constantes
*  Conversiones de referencia implícitas y explícitas, siempre que el origen de las conversiones sea una expresión constante que se evalúe como el valor null.

No se permiten otras conversiones, incluidas las conversiones Boxing, unboxing y de referencia implícita de valores no NULL en expresiones constantes. Por ejemplo:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
la inicialización de i es un error porque se requiere una conversión boxing. La inicialización de Str es un error porque se requiere una conversión de referencia implícita de un valor distinto de NULL.

Siempre que una expresión cumple los requisitos mencionados anteriormente, la expresión se evalúa en tiempo de compilación. Esto es así incluso si la expresión es una subexpresión de una expresión mayor que contiene construcciones que no son constantes.

La evaluación en tiempo de compilación de las expresiones constantes utiliza las mismas reglas que la evaluación en tiempo de ejecución de expresiones no constantes, salvo que, en el caso de que la evaluación en tiempo de ejecución hubiera producido una excepción, la evaluación en tiempo de compilación provoca un error en tiempo de compilación.

A menos que una expresión constante se coloque explícitamente en un contexto `unchecked`, los desbordamientos que se producen en operaciones aritméticas de tipo entero y en las conversiones durante la evaluación en tiempo de compilación de la expresión siempre producen errores en tiempo de compilación ([Constant Expresiones](expressions.md#constant-expressions)).

Las expresiones constantes se producen en los contextos que se enumeran a continuación. En estos contextos, se produce un error en tiempo de compilación si una expresión no se puede evaluar por completo en tiempo de compilación.

*  Declaraciones de constantes ([constantes](classes.md#constants)).
*  Declaraciones de miembros de enumeración ([miembros de enumeración](enums.md#enum-members)).
*  Argumentos predeterminados de las listas de parámetros formales ([parámetros de método](classes.md#method-parameters))
*  etiquetas `case` de una instrucción `switch` ([la instrucción switch](statements.md#the-switch-statement)).
*  instrucciones `goto case` ([instrucción Goto](statements.md#the-goto-statement)).
*  Longitudes de dimensión en una expresión de creación de matriz ([expresiones de creación de matrices](expressions.md#array-creation-expressions)) que incluye un inicializador.
*  Attributes ([atributos](attributes.md)).

Una conversión de expresión constante implícita ([conversiones de expresión constante implícita](conversions.md#implicit-constant-expression-conversions)) permite convertir una expresión constante de tipo `int` a `sbyte`, `byte`, `short`, `ushort`, `uint` o `ulong`, siempre y cuando el valor de la la expresión constante está dentro del intervalo del tipo de destino.

## <a name="boolean-expressions"></a>expresiones booleanas

Una *Boolean_expression* es una expresión que produce un resultado de tipo `bool`; ya sea directamente o a través de la aplicación de `operator true` en determinados contextos, tal y como se especifica en el siguiente.

```antlr
boolean_expression
    : expression
    ;
```

La expresión condicional de control de *una if_statement* ([la instrucción If](statements.md#the-if-statement)) *, while_statement* ([la instrucción while](statements.md#the-while-statement)), *do_statement* ([la instrucción do](statements.md#the-do-statement)) o *for_Statement* ([para Statement](statements.md#the-for-statement)) es una *Boolean_expression*. La expresión condicional de control del operador `?:` ([operador condicional](expressions.md#conditional-operator)) sigue las mismas reglas que una *Boolean_expression*, pero por razones de precedencia de operadores se clasifica como *conditional_or_expression*.

Se requiere una *boolean_expression* `E` para poder generar un valor de tipo `bool`, como se indica a continuación:

*  Si `E` es implícitamente convertible a `bool` en tiempo de ejecución, se aplica la conversión implícita.
*  De lo contrario, se utiliza la resolución de sobrecargas de operador unario ([resolución de sobrecarga de operadores unarios](expressions.md#unary-operator-overload-resolution)) para encontrar una implementación óptima única de Operator `true` en `E` y esa implementación se aplica en tiempo de ejecución.
*  Si no se encuentra ningún operador de este tipo, se produce un error en tiempo de enlace.

El tipo de estructura `DBBool` en [tipo booleano de base de datos](structs.md#database-boolean-type) proporciona un ejemplo de un tipo que implementa `operator true` y `operator false`.
