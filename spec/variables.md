---
ms.openlocfilehash: ff285fc202d14c2060c5f005c319c7886458a168
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66174236"
---
# <a name="variables"></a>Variables

Las variables representan ubicaciones de almacenamiento. Cada variable tiene un tipo que determina qué valores se pueden almacenar en la variable. C# es un lenguaje con seguridad de tipos y el compilador de C# garantiza que los valores almacenados en variables siempre son del tipo adecuado. El valor de una variable se puede cambiar mediante una asignación o mediante el uso de la `++` y `--` operadores.

Debe ser una variable ***asignado definitivamente*** ([asignación definitiva](variables.md#definite-assignment)) antes de que se puede obtener su valor.

Como se describe en las secciones siguientes, las variables son ***asignado inicialmente*** o ***inicialmente sin asignar***. Una variable asignada inicialmente tiene un valor inicial bien definido y siempre se considera asignado definitivamente. Una variable no asignada inicialmente no tiene ningún valor inicial. Para que una variable no asignada inicialmente se considera asignado definitivamente en una determinada ubicación, se debe producir una asignación a la variable en cada ruta de acceso de ejecución posibles que dan lugar a esa ubicación.

## <a name="variable-categories"></a>Categorías de variables

C# define siete categorías de variables: variables estáticas, variables de instancia, elementos de matriz, parámetros de valor, los parámetros de referencia, parámetros de salida y variables locales. Las secciones siguientes describen cada una de estas categorías.

En el ejemplo
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
`x` es una variable estática, `y` es una variable de instancia, `v[0]` es un elemento de matriz, `a` es un parámetro de valor, `b` es un parámetro de referencia, `c` es un parámetro de salida, y `i` es una variable local.

### <a name="static-variables"></a>Variables estáticas

Un campo declarado con el `static` modificador se denomina un ***variable estática***. Una variable estática entra en existencia antes de la ejecución del constructor estático ([constructores estáticos](classes.md#static-constructors)) para su tipo contenedor y deja de existir cuando deja de existir el dominio de aplicación asociado.

El valor inicial de una variable estática es el valor predeterminado ([los valores predeterminados](variables.md#default-values)) del tipo de variable.

Para fines de comprobación de asignación definitiva, una variable estática se considera asignado inicialmente.

### <a name="instance-variables"></a>Variables de instancia

Un campo declarado sin el `static` modificador se denomina un ***variable de instancia***.

#### <a name="instance-variables-in-classes"></a>Variables de instancia en clases

Una variable de instancia de una clase entra en existencia cuando se crea una nueva instancia de esa clase y deja de existir cuando no hay ninguna referencia a esa instancia y se ha ejecutado el destructor de la instancia (si existe).

El valor inicial de una variable de instancia de una clase es el valor predeterminado ([los valores predeterminados](variables.md#default-values)) del tipo de variable.

Con el fin de comprobación de asignación definitiva, una variable de instancia de una clase se considera asignado inicialmente.

#### <a name="instance-variables-in-structs"></a>Variables de instancia en structs

Una variable de instancia de una estructura tiene exactamente la misma duración que la variable de estructura a la que pertenece. En otras palabras, cuando una variable de tipo struct entra en la existencia o deja de existir, lo hacer demasiado de las variables de instancia de la estructura.

El estado de asignación inicial de una variable de instancia de una estructura es el mismo que el de la variable de estructura. En otras palabras, cuando una variable de estructura se considera asignada inicialmente, lo son demasiado sus variables de instancia, y cuando una variable de estructura se considera inicialmente sin asignar, sus variables de instancia del mismo modo están asignados.

### <a name="array-elements"></a>Elementos de matriz

Los elementos de una matriz nazcan cuando se crea una instancia de matriz y dejan de existir cuando no hay referencias a esa instancia de matriz.

El valor inicial de cada uno de los elementos de una matriz es el valor predeterminado ([los valores predeterminados](variables.md#default-values)) del tipo de los elementos de matriz.

Con el fin de comprobación de asignación definitiva, un elemento de matriz se considera asignado inicialmente.

### <a name="value-parameters"></a>Parámetros de valor

Un parámetro declarado sin un `ref` o `out` modificador es un ***parámetro value***.

Un parámetro de valor entra en existencia al invocar el miembro de función (método, constructor de instancia, operador o descriptor de acceso) o una función anónima al que pertenece el parámetro y se inicializa con el valor del argumento especificado en la invocación. Un parámetro de valor normalmente deja de existir cuando se devuelve el miembro de función o función anónima. Sin embargo, si el valor del parámetro se captura mediante una función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)), su duración se extiende al menos hasta que el delegado o árbol de expresión que se crea a partir de esa función anónima es apto para colección de elementos no utilizados.

Con el fin de comprobación de asignación definitiva, un parámetro de valor se considera asignado inicialmente.

### <a name="reference-parameters"></a>Parámetros de referencia

Un parámetro declarado con un `ref` modificador es un ***hacer referencia al parámetro***.

Un parámetro de referencia no crea una nueva ubicación de almacenamiento. En su lugar, un parámetro de referencia representa la misma ubicación de almacenamiento que la variable especificada como argumento en el miembro de función o la invocación de función anónima. Por lo tanto, el valor de un parámetro de referencia es siempre el mismo que la variable subyacente.

Se aplican las siguientes reglas de asignación definitiva a los parámetros de referencia. Tenga en cuenta las distintas reglas de los parámetros de salida que se describe en [parámetros de salida](variables.md#output-parameters).

*  Una variable debe asignarlo definitivamente ([asignación definitiva](variables.md#definite-assignment)) antes de que se puede pasar como un parámetro de referencia en una invocación de función miembro o un delegado.
*  Dentro de un miembro de función o función anónima, un parámetro de referencia se considera asignado inicialmente.

Dentro de un método de instancia o un descriptor de acceso de instancia de un tipo de estructura, el `this` palabra clave se comporta exactamente como un parámetro de referencia de tipo struct ([este acceso](expressions.md#this-access)).

### <a name="output-parameters"></a>Parámetros de salida

Un parámetro declarado con un `out` modificador es un ***parámetro de salida***.

Un parámetro de salida no crea una nueva ubicación de almacenamiento. En su lugar, un parámetro de salida representa la misma ubicación de almacenamiento que la variable especificada como argumento en la invocación de función miembro o un delegado. Por lo tanto, el valor de un parámetro de salida es siempre el mismo que la variable subyacente.

Se aplican las siguientes reglas de asignación definitiva a los parámetros de salida. Tenga en cuenta las distintas reglas de los parámetros de referencia se describe en [hacen referencia a parámetros](variables.md#reference-parameters).

*  Una variable no debe estar asignada definitivamente antes de que se puede pasar como un parámetro de salida en un miembro de función o la invocación de delegado.
*  Tras la finalización normal de una invocación de miembro o un delegado de función, cada variable que se pasó como un parámetro de salida se considera asignado en esa ruta de acceso de ejecución.
*  Dentro de un miembro de función o función anónima, un parámetro de salida se considera inicialmente sin asignar.
*  Cada parámetro de salida de un miembro de función o función anónima debe asignarlo definitivamente ([asignación definitiva](variables.md#definite-assignment)) antes de la función miembro o función anónima vuelve con normalidad.

Dentro de un constructor de instancia de un tipo de estructura, el `this` palabra clave se comporta exactamente como un parámetro de salida de tipo struct ([este acceso](expressions.md#this-access)).

### <a name="local-variables"></a>Variables locales

Un ***variable local*** se declara mediante un *local_variable_declaration*, lo que puede producirse en un *bloque*, un *for_statement*, un *switch_statement* o un *using_statement*; o por un *foreach_statement* o un *specific_catch_clause* para un *try_statement*.

La duración de una variable local es la parte de la ejecución del programa durante el cual almacenamiento garantiza que se va a reservar para él. Esta duración se extiende al menos desde la entrada en el *bloque*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, o *specific_catch_clause* con el que está asociado, hasta la ejecución de ese *bloque*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, o *specific_catch_clause* extremos de ninguna manera. (Escriba un texto delimitado *bloque* o llamar a un método se suspende, pero no finaliza, la ejecución del elemento actual *bloque*, *for_statement*, *switch_statement* , *using_statement*, *foreach_statement*, o *specific_catch_clause*.) Si se captura la variable local mediante una función anónima ([capturan las variables externas](expressions.md#captured-outer-variables)), su duración se extiende al menos hasta que el árbol de expresión o delegado creado a partir de la función anónima, junto con cualquier otro objeto que vienen a referencia a la variable capturada, son aptas para la recolección.

Si el elemento primario *bloque*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, o *specific_catch_clause* se escribe de forma recursiva, se crea una nueva instancia de la variable local cada hora y su *local_variable_initializer*, si existe, se evalúa. cada vez.

Una variable local introducida por un *local_variable_declaration* no se inicializa automáticamente y, por tanto, no tiene ningún valor predeterminado. Con el fin de comprobación de asignación definitiva, una variable local introducidos por un *local_variable_declaration* se considera inicialmente sin asignar. Un *local_variable_declaration* puede incluir un *local_variable_initializer*, en cuyo caso la variable se considera asignada definitivamente solo después de la expresión de inicialización ([ Las instrucciones de declaración](variables.md#declaration-statements)).

Dentro del ámbito de una variable local introducido por un *local_variable_declaration*, es un error en tiempo de compilación para hacer referencia a la variable local en una posición textual que precede a su *local_variable_declarator*. Si la declaración de variable local es implícita ([declaraciones de variable Local](statements.md#local-variable-declarations)), también es un error hacer referencia a la variable dentro de su *local_variable_declarator*.

Una variable local introducida por un *foreach_statement* o un *specific_catch_clause* se considera asignado definitivamente en todo su ámbito.

La duración real de una variable local es depende de la implementación. Por ejemplo, un compilador puede determinar estáticamente que solo se usa una variable local en un bloque de una pequeña parte de ese bloque. Con este análisis, el compilador podría generar código que es el resultado en el almacenamiento de la variable tiene una duración más corta que su bloque contenedor.

El almacenamiento al que hace referencia una variable de referencia local se reclama independientemente de la duración de esa variable de referencia local ([administración de memoria automática](basic-concepts.md#automatic-memory-management)).

## <a name="default-values"></a>Valores predeterminados

Las siguientes categorías de variables se inicializan automáticamente en sus valores predeterminados:

*  Variables estáticas.
*  Variables de instancia de instancias de clase.
*  Elementos de la matriz.

El valor predeterminado de una variable depende del tipo de la variable y se determina como sigue:

*  Para una variable de un *value_type*, el valor predeterminado es el mismo que el valor calculado por el *value_type*del constructor predeterminado ([constructores predeterminados](types.md#default-constructors)).
*  Para una variable de un *reference_type*, el valor predeterminado es `null`.

Inicialización de los valores predeterminados normalmente se consigue haciendo que el Administrador de memoria o el recolector de elementos no utilizados inicializar la memoria para todos los bits cero antes de está asignada para su uso. Por este motivo, es conveniente utilizar todos los bits cero para representar la referencia nula.

## <a name="definite-assignment"></a>Asignación definitiva

En una ubicación especificada en el código ejecutable de un miembro de función, una variable se dice que ***asignado definitivamente*** si el compilador puede probar, mediante un análisis de flujo estático concreto ([reglas precisas para determinar definitiva asignación](variables.md#precise-rules-for-determining-definite-assignment)), que la variable se ha inicializado automáticamente o ha sido el destino de al menos una asignación. De manera informal se ha indicado, las reglas de asignación definitiva son:

*  Una variable asignada inicialmente ([variables asignadas inicialmente](variables.md#initially-assigned-variables)) siempre se considera asignado definitivamente.
*  Una variable no asignada inicialmente ([inicialmente sin asignar variables](variables.md#initially-unassigned-variables)) se considera asignado definitivamente en una ubicación determinada si todas las rutas de ejecución posibles que llevan a esa ubicación contengan al menos uno de los siguientes:
    * Una asignación simple ([asignación Simple](expressions.md#simple-assignment)) en el que la variable es el operando izquierdo.
    * Una expresión de invocación ([expresiones de invocación](expressions.md#invocation-expressions)) o una expresión de creación de objeto ([expresiones de creación de objeto](expressions.md#object-creation-expressions)) que pasa la variable como un parámetro de salida.
    * Para una variable local, una declaración de variable local ([declaraciones de variable Local](statements.md#local-variable-declarations)) que incluye un inicializador de variable.

La especificación formal subyacente a las reglas informales anteriores se describe en [variables asignadas inicialmente](variables.md#initially-assigned-variables), [inicialmente sin asignar variables](variables.md#initially-unassigned-variables), y [reglas precisas para determinar asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment).

Los Estados de asignación definitiva de variables de instancia de un *struct_type* variable se realiza un seguimiento individual y también colectivo. En adicionales a las reglas anteriores, las reglas siguientes se aplican a *struct_type* variables y sus variables de instancia:

*  Una variable de instancia se considera asignada definitivamente si lo contiene *struct_type* variable se considera asignado definitivamente.
*  Un *struct_type* variable se considera asignado definitivamente si cada uno de sus variables de instancia se considera asignado definitivamente.

Asignación definitiva es un requisito en los contextos siguientes:

*  Una variable debe estar asignada definitivamente en cada ubicación donde se obtiene su valor. Esto garantiza que nunca se producen valores no definidos. Se considera la aparición de una variable en una expresión para obtener el valor de la variable, excepto cuando
    * la variable es el operando izquierdo de una asignación simple,
    * la variable se pasa como parámetro de salida, o
    * la variable es un *struct_type* variable y se produce como el operando izquierdo de un acceso de miembro.
*  Una variable debe estar asignada definitivamente en cada ubicación donde se pasa como un parámetro de referencia. Esto garantiza que el miembro de función que se invoca puede tener en cuenta el parámetro de referencia asignado inicialmente.
*  Todos los parámetros de salida de un miembro de función deben asignarlo definitivamente en cada ubicación donde se devuelve el miembro de función (a través de un `return` instrucción o a través de la ejecución llega al final del cuerpo de miembro de función). Esto garantiza que los miembros de función no devuelven valores no definidos en los parámetros de salida, lo que permite al compilador que considere la posibilidad de una invocación de miembros de función que toma una variable como un parámetro de salida equivalente a una asignación a la variable.
*  El `this` variable de un *struct_type* constructor de instancia debe asignarlo definitivamente en cada ubicación donde se devuelve ese constructor de instancia.

### <a name="initially-assigned-variables"></a>Variables asignadas inicialmente

Las siguientes categorías de variables se clasifican como asignadas inicialmente:

*  Variables estáticas.
*  Variables de instancia de instancias de clase.
*  Variables de instancia de variables de estructura asignadas inicialmente.
*  Elementos de la matriz.
*  Parámetros de valor.
*  Parámetros de referencia.
*  Las variables declaradas en un `catch` cláusula o `foreach` instrucción.

### <a name="initially-unassigned-variables"></a>Variables inicialmente sin asignar

Las siguientes categorías de variables se clasifican como inicialmente sin asignar:

*  Variables de instancia de struct inicialmente sin asignar variables.
*  Parámetros de salida, incluida la `this` variable de constructores de instancia de struct.
*  Las variables locales, excepto los declarados en un `catch` cláusula o `foreach` instrucción.

### <a name="precise-rules-for-determining-definite-assignment"></a>Reglas precisas para determinar la asignación definitiva

Con el fin de determinar que cada variable utilizada se asigna definitivamente, el compilador debe usar un proceso que es equivalente a la descrita en esta sección.

El compilador procesa el cuerpo de cada miembro de función que tiene una o más variables inicialmente sin asignar. Para cada variable inicialmente sin asignar *v*, el compilador determina un ***estado de asignación definitiva*** para *v* en cada uno de los siguientes puntos en el miembro de función:

*  Al principio de cada instrucción
*  En el punto final ([puntos finales y alcance](statements.md#end-points-and-reachability)) de cada instrucción
*  En cada arco que transfiere el control a otra instrucción o al punto final de una instrucción
*  Al principio de cada expresión
*  Al final de cada expresión

El estado de asignación definitiva de *v* puede ser:

*  Asigna definitivamente. Indica que todos los flujos de control posibles en este punto, *v* se ha asignado un valor.
*  No asignado definitivamente. El estado de una variable al final de una expresión de tipo `bool`, el estado de una variable que no se ha asignado definitivamente mayo (pero no necesariamente) se dividen en uno de los subestados siguientes:
    * Se asigna definitivamente después de una expresión es true. Este estado indica que *v* se asigna definitivamente si la expresión booleana se evalúa como true, pero no se asigna necesariamente si la expresión booleana se evalúa como false.
    * Se asigna definitivamente después de una expresión es false. Este estado indica que *v* se asigna definitivamente si la expresión booleana se evalúa como false, pero no se asigna necesariamente si la expresión booleana se evalúa como true.

Las siguientes reglas determinan cómo el estado de una variable *v* viene determinada en cada ubicación.

#### <a name="general-rules-for-statements"></a>Reglas generales para las instrucciones

*  *v* no se asigna definitivamente al principio de un cuerpo de función miembro.
*  *v* se asigna definitivamente al principio de cualquier instrucción inalcanzable.
*  El estado de asignación definitiva de *v* al principio de cualquier otra instrucción se determina comprobando el estado de asignación definitiva de *v* en todas las transferencias de flujo de control que tienen como destino el principio de la que instrucción. Si (y sólo si) *v* se asigna definitivamente en todas estas transferencias de flujo de control, a continuación, *v* se asigna definitivamente al principio de la instrucción. El conjunto de transferencias de flujo de control posibles se determina en la misma manera que el alcance de la instrucción de comprobación ([puntos finales y alcance](statements.md#end-points-and-reachability)).
*  El estado de asignación definitiva de *v* en el punto final de un bloque, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, o `switch` viene determinado por la comprobación del estado de asignación definitiva de *v* en todas las transferencias de flujo de control que tienen como destino el punto final de esa instrucción. Si *v* se asigna definitivamente en todas estas transferencias de flujo de control, a continuación, *v* se asigna definitivamente en el punto final de la instrucción. En caso contrario, *v* no se asigna definitivamente en el punto final de la instrucción. El conjunto de transferencias de flujo de control posibles se determina en la misma manera que el alcance de la instrucción de comprobación ([puntos finales y alcance](statements.md#end-points-and-reachability)).

#### <a name="block-statements-checked-and-unchecked-statements"></a>Instrucciones de bloque checked y unchecked

El estado de asignación definitiva de *v* en el control de transferencia de la primera instrucción de la lista de instrucciones en el bloque (o al punto final del bloque, si la lista de instrucciones está vacía) es el mismo que la instrucción de asignación definitiva de *v* antes del bloque, `checked`, o `unchecked` instrucción.

#### <a name="expression-statements"></a>Instrucciones de expresión

Para una instrucción de expresión *stmt* que consta de la expresión *expr*:

*  *v* tiene el mismo estado de asignación definitiva al principio de *expr* como al principio de *stmt*.
*  Si *v* si asigna definitivamente al final de *expr*, se asigna definitivamente en el punto final de *stmt*; en caso contrario, no se asigna definitivamente en el punto final de *stmt*.

#### <a name="declaration-statements"></a>Instrucciones de declaración

*  Si *stmt* es una instrucción de declaración sin inicializadores, a continuación, *v* tiene el mismo estado de asignación definitiva en el punto final de *stmt* como al principio de *stmt*.
*  Si *stmt* es una instrucción de declaración con inicializadores, a continuación, el estado de asignación definitiva para *v* se determina como si *stmt* eran una lista de instrucciones, con una asignación instrucción para cada declaración con un inicializador (en el orden de declaración).

#### <a name="if-statements"></a>Si las instrucciones

Para un `if` instrucción *stmt* del formulario:
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* tiene el mismo estado de asignación definitiva al principio de *expr* como al principio de *stmt*.
*  Si *v* se asigna definitivamente al final de *expr*, a continuación, se asigna definitivamente en la transferencia de flujo de control a *then_stmt* y cualquiera *else_stmt*  o para el punto de conexión de *stmt* si no hay ninguna cláusula else.
*  Si *v* tiene el estado "asignado definitivamente después de una expresión true" al final de *expr*, a continuación, se asigna definitivamente en la transferencia de flujo de control a *then_stmt*y no asignado definitivamente en la transferencia de flujo de control como *else_stmt* o para el punto de conexión de *stmt* si no hay ninguna cláusula else.
*  Si *v* tiene el estado "asignado definitivamente después de una expresión false" al final de *expr*, a continuación, se asigna definitivamente en la transferencia de flujo de control a *else_stmt*y no se asigna definitivamente en la transferencia de flujo de control a *then_stmt*. Se asigna definitivamente en el punto final de *stmt* solo si se asigna definitivamente en el punto final de *then_stmt*.
*  En caso contrario, *v* no se considera asignado definitivamente en la transferencia de flujo de control como el *then_stmt* o *else_stmt*, o para el punto de conexión de  *stmt* si no hay ninguna cláusula else.

#### <a name="switch-statements"></a>Instrucciones switch

En un `switch` instrucción *stmt* con una expresión de control *expr*:

*  El estado de asignación definitiva de *v* al principio de *expr* es el mismo que el estado de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* transferencia a una lista de instrucciones switch puede obtener acceso al bloque en el flujo de control es el mismo que el estado de asignación definitiva de *v* al final de *expr*.

#### <a name="while-statements"></a>Instrucciones while

Para un `while` instrucción *stmt* del formulario:
```csharp
while ( expr ) while_body
```

*  *v* tiene el mismo estado de asignación definitiva al principio de *expr* como al principio de *stmt*.
*  Si *v* se asigna definitivamente al final de *expr*, a continuación, se asigna definitivamente en la transferencia de flujo de control a *while_body* y para el punto final de  *stmt*.
*  Si *v* tiene el estado "asignado definitivamente después de una expresión true" al final de *expr*, a continuación, se asigna definitivamente en la transferencia de flujo de control a *while_body*, pero no se asigna definitivamente en el punto final de *stmt*.
*  Si *v* tiene el estado "asignado definitivamente después de una expresión false" al final de *expr*, a continuación, se asigna definitivamente en la transferencia de flujo de control para el punto final de *stmt* , pero no asignado definitivamente en la transferencia de flujo de control a *while_body*.

#### <a name="do-statements"></a>Siga las instrucciones

Para un `do` instrucción *stmt* del formulario:
```csharp
do do_body while ( expr ) ;
```

*  *v* tiene el mismo estado de asignación definitiva en la transferencia de flujo de control desde el principio del *stmt* a *do_body* como al principio de *stmt*.
*  *v* tiene el mismo estado de asignación definitiva al principio de *expr* como en el punto final de *do_body*.
*  Si *v* se asigna definitivamente al final de *expr*, a continuación, se asigna definitivamente en la transferencia de flujo de control para el punto final de *stmt*.
*  Si *v* tiene el estado "asignado definitivamente después de una expresión false" al final de *expr*, a continuación, se asigna definitivamente en la transferencia de flujo de control para el punto final de *stmt* .

#### <a name="for-statements"></a>Para las instrucciones

Asignación definitiva buscando un `for` instrucción del formulario:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
se realiza como si hubiera escrito la instrucción:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Si el *for_condition* se omite de la `for` instrucción y, a continuación, la evaluación de asignación definitiva continúa como si *for_condition* se reemplazaron con `true` en la expansión anterior .

#### <a name="break-continue-and-goto-statements"></a>Break, continue y las instrucciones goto

El estado de asignación definitiva de *v* en la transferencia de flujo de control causada por un `break`, `continue`, o `goto` instrucción es el mismo que el estado de asignación definitiva de *v* en el a partir de la instrucción.

#### <a name="throw-statements"></a>Throw (instrucciones)

Para una instrucción *stmt* del formulario
```csharp
throw expr ;
```

El estado de asignación definitiva de *v* al principio de *expr* es el mismo que el estado de asignación definitiva de *v* al principio de *stmt*.

#### <a name="return-statements"></a>Instrucciones return

Para una instrucción *stmt* del formulario
```csharp
return expr ;
```

*  El estado de asignación definitiva de *v* al principio de *expr* es el mismo que el estado de asignación definitiva de *v* al principio de *stmt*.
*  Si *v* es un parámetro output, a continuación, se debe asignarlo definitivamente cualquiera:
    * una vez *expr*
    * o al final de la `finally` block de un `try` - `finally` o `try` - `catch` - `finally` que rodea el `return` instrucción.

Para una instrucción stmt del formulario:
```csharp
return ;
```

*  Si *v* es un parámetro output, a continuación, se debe asignarlo definitivamente cualquiera:
    * before *stmt*
    * o al final de la `finally` block de un `try` - `finally` o `try` - `catch` - `finally` que rodea el `return` instrucción.

#### <a name="try-catch-statements"></a>Instrucciones try-catch

Para una instrucción *stmt* del formulario:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  El estado de asignación definitiva de *v* al principio de *try_block* es el mismo que el estado de asignación definitiva de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* al principio de *catch_block_i* (para cualquier *i*) es el mismo que el estado de asignación definitiva de *v*al principio de *stmt*.
*  El estado de asignación definitiva de *v* en el punto final de *stmt* está asignado definitivamente si (y sólo si) *v* se asigna definitivamente en el punto final de  *try_block* y cada *catch_block_i* (para cada *i* de 1 a *n*).

#### <a name="try-finally-statements"></a>Instrucciones try-finally

Para un `try` instrucción *stmt* del formulario:
```csharp
try try_block finally finally_block
```

*  El estado de asignación definitiva de *v* al principio de *try_block* es el mismo que el estado de asignación definitiva de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* al principio de *finally_block* es el mismo que el estado de asignación definitiva de *v* al principio de *stmt* .
*  El estado de asignación definitiva de *v* en el punto final de *stmt* está asignado definitivamente si (y sólo si) al menos uno de los siguientes es true:
    * *v* se asigna definitivamente en el punto final de *try_block*
    * *v* se asigna definitivamente en el punto final de *finally_block*

Si una transferencia de flujo de control (por ejemplo, un `goto` instrucción) se realiza alguna que comienza en *try_block*y termina fuera de *try_block*, a continuación, *v* también es considera asignado definitivamente en esa transferencia de flujo de control si *v* se asigna definitivamente en el punto final de *finally_block*. (Esta condición no es exclusiva: si *v* se asigna definitivamente por otra razón en esta transferencia de flujo de control, a continuación, se considera asignado definitivamente.)

#### <a name="try-catch-finally-statements"></a>Instrucciones try-catch-finally

Análisis de asignación definitiva para un `try` - `catch` - `finally` instrucción del formulario:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
se realiza como si la instrucción fuera un `try` - `finally` instrucción envolvente un `try` - `catch` instrucción:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

En el ejemplo siguiente se muestra cómo los diferentes bloques de un `try` instrucción ([la instrucción try](statements.md#the-try-statement)) afectan a la asignación definitiva.
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a>Instrucciones foreach

Para un `foreach` instrucción *stmt* del formulario:
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  El estado de asignación definitiva de *v* al principio de *expr* es el mismo que el estado de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* en la transferencia de flujo de control a *embedded_statement* o hasta el punto final de *stmt* es el mismo que el estado de *v* al final de *expr*.

#### <a name="using-statements"></a>Las instrucciones using

Para un `using` instrucción *stmt* del formulario:
```csharp
using ( resource_acquisition ) embedded_statement
```

*  El estado de asignación definitiva de *v* al principio de *resource_acquisition* es el mismo que el estado de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* en la transferencia de flujo de control a *embedded_statement* es el mismo que el estado de *v* al final de *resource_ adquisición*.

#### <a name="lock-statements"></a>Instrucciones de bloqueo

Para un `lock` instrucción *stmt* del formulario:
```csharp
lock ( expr ) embedded_statement
```

*  El estado de asignación definitiva de *v* al principio de *expr* es el mismo que el estado de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* en la transferencia de flujo de control a *embedded_statement* es el mismo que el estado de *v* al final de *expr*.

#### <a name="yield-statements"></a>Instrucciones yield

Para un `yield return` instrucción *stmt* del formulario:
```csharp
yield return expr ;
```

*  El estado de asignación definitiva de *v* al principio de *expr* es el mismo que el estado de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* al final de *stmt* es el mismo que el estado de *v* al final de *expr*.
*  Un `yield break` instrucción no tiene ningún efecto sobre el estado de asignación definitiva.

#### <a name="general-rules-for-simple-expressions"></a>Reglas generales para las expresiones simples

La siguiente regla se aplica a estos tipos de expresiones: literales ([literales](expressions.md#literals)), nombres simples ([nombres simples](expressions.md#simple-names)), expresiones de acceso a miembro ([acceso a miembros](expressions.md#member-access)), las expresiones de acceso base no indizada ([Base acceso](expressions.md#base-access)), `typeof` expresiones ([el operador typeof](expressions.md#the-typeof-operator)), expresiones de valor predeterminado ([expresiones de valor predeterminado ](expressions.md#default-value-expressions)) y `nameof` expresiones ([expresiones Nameof](expressions.md#nameof-expressions)).

*  El estado de asignación definitiva de *v* al final de este tipo de expresión es el mismo que el estado de asignación definitiva de *v* al principio de la expresión.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Reglas generales para expresiones con expresiones incrustadas

Las reglas siguientes se aplican a estos tipos de expresiones: las expresiones entre paréntesis ([las expresiones entre paréntesis](expressions.md#parenthesized-expressions)), las expresiones de acceso de elemento ([acceso de elemento](expressions.md#element-access)) base, obtener acceso a las expresiones con indexación ([Base acceso](expressions.md#base-access)), aumentar y disminuir las expresiones ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators), [prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)), las expresiones de conversión ([expresiones de conversión](expressions.md#cast-expressions)), unario `+`, `-`, `~`, `*` expresiones, binarias `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` expresiones ([operadores aritméticos](expressions.md#arithmetic-operators), [operadores de desplazamiento](expressions.md#shift-operators), [operadores de comprobación de tipos y relacionales](expressions.md#relational-and-type-testing-operators) [Operadores lógicos](expressions.md#logical-operators)), compuesta de expresiones de asignación ([asignación compuesta](expressions.md#compound-assignment)), `checked` y `unchecked` expresiones ([checked y unchecked operadores](expressions.md#the-checked-and-unchecked-operators)), además de las expresiones de creación de matriz y un delegado ([el operador new](expressions.md#the-new-operator)).

Cada una de estas expresiones tiene uno o más subexpresiones que se evalúan incondicionalmente en un orden fijo. Por ejemplo, el archivo binario `%` operador evalúa el lado izquierdo del operador y, después, en el lado derecho. Una operación de indización evalúa la expresión indizada y, a continuación, evalúa cada una de las expresiones de índice, en orden de izquierda a derecha. Para una expresión *expr*, que tiene las subexpresiones *e1, e2,..., eN*, evaluada en ese orden:

*  El estado de asignación definitiva de *v* al principio de *e1* es el mismo que el estado de asignación definitiva al principio de *expr*.
*  El estado de asignación definitiva de *v* al principio de *ei* (*i* mayor que uno) es el mismo que el estado de asignación definitiva al final de la subexpresión anterior.
*  El estado de asignación definitiva de *v* al final de *expr* es el mismo que el estado de asignación definitiva al final de *eN*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Expresiones de invocación y expresiones de creación de objeto

Para una expresión de invocación *expr* del formulario:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
o una expresión de creación de objeto del formulario:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Para una expresión de invocación, el estado de asignación definitiva de *v* antes *primary_expression* es el mismo que el estado de *v* antes *expr*.
*  Para una expresión de invocación, el estado de asignación definitiva de *v* antes *arg1* es el mismo que el estado de *v* después *primary_expression*.
*  Para una expresión de creación de objeto, el estado de asignación definitiva de *v* antes *arg1* es el mismo que el estado de *v* antes *expr*.
*  Para cada argumento *argi*, el estado de asignación definitiva de *v* después *argi* viene determinada por las reglas de expresión normales, se omitirá cualquier `ref` o `out`modificadores.
*  Para cada argumento *argi* para cualquier *i* mayor que uno, el estado de asignación definitiva de *v* antes *argi* es el mismo que el estado de *v* después de la anterior *arg*.
*  Si la variable *v* se pasa como un `out` argumento (es decir, un argumento del formulario `out v`) en cualquiera de los argumentos y, a continuación, el estado de *v* después *expr* se ha asignado definitivamente. En caso contrario, el estado de *v* después *expr* es el mismo que el estado de *v* después *argN*.
*  Inicializadores de matriz ([expresiones de creación de matriz](expressions.md#array-creation-expressions)), inicializadores de objeto ([inicializadores de objeto](expressions.md#object-initializers)), los inicializadores de colección ([inicializadores de colección](expressions.md#collection-initializers)) y los inicializadores de objeto anónimo ([expresiones de creación de objeto anónimo](expressions.md#anonymous-object-creation-expressions)), el estado de asignación definitiva viene determinada por la expansión que estas construcciones se definen en términos de.

#### <a name="simple-assignment-expressions"></a>Expresiones de asignación simple

Para una expresión *expr* del formulario `w = expr_rhs`:

*  El estado de asignación definitiva de *v* antes *expr_rhs* es el mismo que el estado de asignación definitiva de *v* antes *expr*.
*  El estado de asignación definitiva de *v* después *expr* viene determinada por:
   * Si *w* es la misma variable como *v*, a continuación, el estado de asignación definitiva de *v* después *expr* se asigna definitivamente.
   * En caso contrario, si la asignación se produce dentro del constructor de instancia de un tipo de estructura, si *w* es un acceso de propiedad que designa una propiedad implementada automáticamente *P* en la instancia que se está construyendo y *v* es el campo de respaldo oculto de *P*, a continuación, el estado de asignación definitiva de *v* después *expr* definitivamente asignado.
   * En caso contrario, el estado de asignación definitiva de *v* después *expr* es el mismo que el estado de asignación definitiva de *v* después *expr_rhs*.

#### <a name="-conditional-and-expressions"></a>& & (AND condicional) expresiones

Para una expresión *expr* del formulario `expr_first && expr_second`:

*  El estado de asignación definitiva de *v* antes *expr_first* es el mismo que el estado de asignación definitiva de *v* antes *expr*.
*  El estado de asignación definitiva de *v* antes *expr_second* se asigna definitivamente si el estado de *v* después *expr_first* sea asignado definitivamente o "asignado definitivamente después de una expresión true". En caso contrario, no se asigna definitivamente.
*  El estado de asignación definitiva de *v* después *expr* viene determinada por:
    * Si *expr_first* es una expresión constante con el valor `false`, a continuación, el estado de asignación definitiva de *v* después *expr* es igual que la asignación definitiva estado de *v* después *expr_first*.
    * De lo contrario, si el estado de *v* después *expr_first* se asigna definitivamente, a continuación, el estado de *v* después *expr* se asigna definitivamente.
    * De lo contrario, si el estado de *v* después *expr_second* se asigna definitivamente y el estado de *v* después *expr_first* "definitivamente asignado después de una expresión false", entonces el estado de *v* después *expr* se asigna definitivamente.
    * De lo contrario, si el estado de *v* después *expr_second* se asigna definitivamente o "asignado definitivamente después de una expresión true", entonces el estado de *v* después  *Expr* es "asignado definitivamente después de una expresión true".
    * De lo contrario, si el estado de *v* después *expr_first* es "asignado definitivamente después de una expresión false" y el estado de *v* después *expr_second* es "asignado definitivamente después de una expresión false", entonces el estado de *v* después *expr* es "asignado definitivamente después de una expresión false".
    * En caso contrario, el estado de *v* después *expr* no se ha asignado definitivamente.

En el ejemplo
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
la variable `i` se considera asignado definitivamente en una de las instrucciones incrustadas de una `if` instrucción pero no en el otro. En el `if` instrucción en el método `F`, la variable `i` se asigna definitivamente en la primera instrucción incrustada porque la ejecución de la expresión `(i = y)` siempre precede a la ejecución de esta instrucción incrustada. En cambio, la variable `i` no se asigna definitivamente en la segunda instrucción incrustada, ya que `x >= 0` podría dar como resultado false, lo que resulta en la variable `i` sin asignar.

#### <a name="-conditional-or-expressions"></a>|| (OR condicional) expresiones

Para una expresión *expr* del formulario `expr_first || expr_second`:

*  El estado de asignación definitiva de *v* antes *expr_first* es el mismo que el estado de asignación definitiva de *v* antes *expr*.
*  El estado de asignación definitiva de *v* antes *expr_second* se asigna definitivamente si el estado de *v* después *expr_first* sea asignado definitivamente o "asignado definitivamente después de una expresión false". En caso contrario, no se asigna definitivamente.
*  La instrucción de asignación definitiva de *v* después *expr* viene determinada por:
    * Si *expr_first* es una expresión constante con el valor `true`, a continuación, el estado de asignación definitiva de *v* después *expr* es igual que la asignación definitiva estado de *v* después *expr_first*.
    * De lo contrario, si el estado de *v* después *expr_first* se asigna definitivamente, a continuación, el estado de *v* después *expr* se asigna definitivamente.
    * De lo contrario, si el estado de *v* después *expr_second* se asigna definitivamente y el estado de *v* después *expr_first* "definitivamente asignado después de una expresión true", entonces el estado de *v* después *expr* se asigna definitivamente.
    * De lo contrario, si el estado de *v* después *expr_second* se asigna definitivamente o "asignado definitivamente después de una expresión false", entonces el estado de *v* después *expr* es "asignado definitivamente después de una expresión false".
    * De lo contrario, si el estado de *v* después *expr_first* es "asignado definitivamente después de una expresión true" y el estado de *v* después *expr_second*es "asignado definitivamente después de una expresión true", entonces el estado de *v* después *expr* es "asignado definitivamente después de una expresión true".
    * En caso contrario, el estado de *v* después *expr* no se ha asignado definitivamente.

En el ejemplo
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
la variable `i` se considera asignado definitivamente en una de las instrucciones incrustadas de una `if` instrucción pero no en el otro. En el `if` instrucción en el método `G`, la variable `i` se asigna definitivamente en la segunda instrucción incrustada porque la ejecución de la expresión `(i = y)` siempre precede a la ejecución de esta instrucción incrustada. En cambio, la variable `i` no se asigna definitivamente en la primera instrucción incrustada, ya que `x >= 0` podría dar como resultado true, lo que resulta en la variable `i` sin asignar.

#### <a name="-logical-negation-expressions"></a>! expresiones (negación lógica)

Para una expresión *expr* del formulario `! expr_operand`:

*  El estado de asignación definitiva de *v* antes *expr_operand* es el mismo que el estado de asignación definitiva de *v* antes *expr*.
*  El estado de asignación definitiva de *v* después *expr* viene determinada por:
    * Si el estado de *v* después * expr_operand * se asigna definitivamente, a continuación, el estado de *v* después *expr* se asigna definitivamente.
    * Si el estado de *v* después * expr_operand * no es asigna definitivamente, a continuación, el estado de *v* después *expr* no se ha asignado definitivamente.
    * Si el estado de *v* después * expr_operand * es "asignado definitivamente después de una expresión false", entonces el estado de *v* después *expr* es "asignado definitivamente después true expresión".
    * Si el estado de *v* después * expr_operand * es "asignado definitivamente después de una expresión true", entonces el estado de *v* después *expr* es "asignado definitivamente después false expresión".

#### <a name="-null-coalescing-expressions"></a>?? expresiones (uso combinado de null)

Para una expresión *expr* del formulario `expr_first ?? expr_second`:

*  El estado de asignación definitiva de *v* antes *expr_first* es el mismo que el estado de asignación definitiva de *v* antes *expr*.
*  El estado de asignación definitiva de *v* antes *expr_second* es el mismo que el estado de asignación definitiva de *v* después *expr_first*.
*  La instrucción de asignación definitiva de *v* después *expr* viene determinada por:
    * Si *expr_first* es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)) con valor null, entonces el estado de *v* después *expr* es igual que el estado de *v* después *expr_second*.
*  En caso contrario, el estado de *v* después *expr* es el mismo que el estado de asignación definitiva de *v* después *expr_first*.

#### <a name="-conditional-expressions"></a>?: (condicionales) expresiones

Para una expresión *expr* del formulario `expr_cond ? expr_true : expr_false`:

*  El estado de asignación definitiva de *v* antes *expr_cond* es el mismo que el estado de *v* antes *expr*.
*  El estado de asignación definitiva de *v* antes *expr_true* se asigna definitivamente solo si uno de los siguientes contiene:
    * *expr_cond* es una expresión constante con el valor `false`
    * el estado de *v* después *expr_cond* se asigna definitivamente o "asignado definitivamente después de una expresión true".
*  El estado de asignación definitiva de *v* antes *expr_false* se asigna definitivamente solo si uno de los siguientes contiene:
    * *expr_cond* es una expresión constante con el valor `true`
*  el estado de *v* después *expr_cond* se asigna definitivamente o "asignado definitivamente después de una expresión false".
*  El estado de asignación definitiva de *v* después *expr* viene determinada por:
    * Si *expr_cond* es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)) con el valor `true` , a continuación, el estado de *v* después *expr* es el mismo que el estado de *v* después *expr_true*.
    * De lo contrario, si *expr_cond* es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)) con el valor `false` , a continuación, el estado de *v* después *expr* es el mismo que el estado de *v* después *expr_false*.
    * De lo contrario, si el estado de *v* después *expr_true* se asigna definitivamente y el estado de *v* después *expr_false* definitivamente asigna, a continuación, el estado de *v* después *expr* se asigna definitivamente.
    * En caso contrario, el estado de *v* después *expr* no se ha asignado definitivamente.

#### <a name="anonymous-functions"></a>Funciones anónimas

Para un *lambda_expression* o *anonymous_method_expression* *expr* con un cuerpo (ya sea *bloque* o *expresión* ) *cuerpo*:

*  El estado de asignación definitiva de una variable externa *v* antes *cuerpo* es el mismo que el estado de *v* antes *expr*. Es decir, el estado de asignación definitiva de variables externas se hereda el contexto de la función anónima.
*  El estado de asignación definitiva de una variable externa *v* después *expr* es el mismo que el estado de *v* antes *expr*.

El ejemplo
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
genera un error en tiempo de compilación desde `max` no se ha asignado definitivamente donde se declara la función anónima. El ejemplo
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
También genera un error en tiempo de compilación desde la asignación a `n` en la función anónima no tiene ningún efecto sobre el estado de asignación definitiva de `n` fuera de la función anónima.

## <a name="variable-references"></a>Referencias de variables

Un *variable_reference* es un *expresión* que se clasifica como una variable. Un *variable_reference* denota una ubicación de almacenamiento que se puede acceder tanto para capturar el valor actual y para almacenar un nuevo valor.

```antlr
variable_reference
    : expression
    ;
```

En C y C++, un *variable_reference* se conoce como un *lvalue*.

## <a name="atomicity-of-variable-references"></a>Atomicidad de las referencias de variables

Lecturas y escrituras de los siguientes tipos de datos son atómicas: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`y tipos de referencia. Además, las lecturas y escrituras de los tipos de enumeración con un tipo subyacente de la lista anterior son también atómicas. Lecturas y escrituras de otros tipos, incluidos `long`, `ulong`, `double`, y `decimal`, así como los tipos definidos por el usuario, no se garantiza que sea atómica. Aparte de las funciones de biblioteca destinadas para dicho propósito, hay ninguna garantía de atomic lectura-modificación-escritura, como en el caso de incremento o decremento.

