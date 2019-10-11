---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876798"
---
# <a name="variables"></a>variables

Las variables representan ubicaciones de almacenamiento. Cada variable tiene un tipo que determina qué valores se pueden almacenar en la variable. C#es un lenguaje con seguridad de tipos y el C# compilador garantiza que los valores almacenados en variables siempre son del tipo adecuado. El valor de una variable se puede cambiar a través de la asignación o mediante `++` el `--` uso de los operadores y.

Una variable debe estar ***asignada definitivamente*** ([asignación definitiva](variables.md#definite-assignment)) antes de que se pueda obtener su valor.

Tal y como se describe en las secciones siguientes, las variables se ***asignan inicialmente*** o no se ***asignan inicialmente***. Una variable asignada inicialmente tiene un valor inicial bien definido y siempre se considera asignada definitivamente. Una variable no asignada inicialmente no tiene ningún valor inicial. Para que una variable no asignada inicialmente se considere asignada definitivamente en una determinada ubicación, se debe realizar una asignación a la variable en todas las rutas de acceso de ejecución posibles que conducen a esa ubicación.

## <a name="variable-categories"></a>Categorías de variables

C#define siete categorías de variables: variables estáticas, variables de instancia, elementos de matriz, parámetros de valor, parámetros de referencia, parámetros de salida y variables locales. En las secciones siguientes se describe cada una de estas categorías.

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
`x`es una variable estática, `y` es una variable de instancia `v[0]` , es un elemento de `a` matriz, es un parámetro `b` de valor, es un `c` parámetro de referencia, es un `i` parámetro de salida y es una variable local. .

### <a name="static-variables"></a>Variables estáticas

Un campo declarado con el `static` modificador se denomina ***variable estática***. Una variable estática se produce antes de la ejecución del constructor estático ([constructores estáticos](classes.md#static-constructors)) para su tipo contenedor y deja de existir cuando el dominio de aplicación asociado deja de existir.

El valor inicial de una variable estática es el valor predeterminado ([valores predeterminados](variables.md#default-values)) del tipo de la variable.

A efectos de la comprobación de asignación definitiva, se considera que una variable estática está asignada inicialmente.

### <a name="instance-variables"></a>Variables de instancia

Un campo declarado sin el `static` modificador se denomina ***variable de instancia***.

#### <a name="instance-variables-in-classes"></a>Variables de instancia en clases

Una variable de instancia de una clase se produce cuando se crea una nueva instancia de esa clase y deja de existir cuando no hay ninguna referencia a esa instancia y se ha ejecutado el destructor de la instancia (si existe).

El valor inicial de una variable de instancia de una clase es el valor predeterminado ([valores predeterminados](variables.md#default-values)) del tipo de la variable.

Con el fin de comprobar la asignación definitiva, se considera que una variable de instancia de una clase está asignada inicialmente.

#### <a name="instance-variables-in-structs"></a>Variables de instancia en Structs

Una variable de instancia de un struct tiene exactamente la misma duración que la variable de estructura a la que pertenece. En otras palabras, cuando una variable de un tipo struct entra en existencia o deja de existir, también se realizan las variables de instancia de la estructura.

El estado de asignación inicial de una variable de instancia de una estructura es el mismo que el de la variable de estructura contenedora. En otras palabras, cuando se considera que una variable de estructura está asignada inicialmente, también se consideran variables de instancia y, cuando una variable de struct se considera inicialmente sin asignar, sus variables de instancia también se desasignan.

### <a name="array-elements"></a>Elementos de matriz

Los elementos de una matriz se componen cuando se crea una instancia de matriz y dejan de existir cuando no hay ninguna referencia a esa instancia de la matriz.

El valor inicial de cada uno de los elementos de una matriz es el valor predeterminado ([valores predeterminados](variables.md#default-values)) del tipo de los elementos de la matriz.

Con el fin de comprobar la asignación definitiva, se considera que un elemento de la matriz está asignado inicialmente.

### <a name="value-parameters"></a>Parámetros de valor

Un parámetro declarado sin un `ref` modificador o `out` es un ***parámetro de valor***.

Un parámetro de valor se basa en la invocación del miembro de función (método, constructor de instancia, descriptor de acceso o operador) o función anónima a la que pertenece el parámetro, y se inicializa con el valor del argumento proporcionado en la invocación. Normalmente, un parámetro de valor deja de existir cuando se devuelve el miembro de función o la función anónima. Sin embargo, si el parámetro de valor se captura mediante una función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)), su tiempo de vida se extiende al menos hasta que el delegado o el árbol de expresión creado a partir de esa función anónima sea válido para la recolección de elementos no utilizados.

Con el fin de comprobar la asignación definitiva, se considera que un parámetro de valor está asignado inicialmente.

### <a name="reference-parameters"></a>Parámetros de referencia

Un parámetro declarado con un `ref` modificador es un ***parámetro de referencia***.

Un parámetro de referencia no crea una nueva ubicación de almacenamiento. En su lugar, un parámetro de referencia representa la misma ubicación de almacenamiento que la variable proporcionada como argumento en el miembro de función o la invocación de la función anónima. Por lo tanto, el valor de un parámetro de referencia es siempre el mismo que el de la variable subyacente.

Las siguientes reglas de asignación definitiva se aplican a los parámetros de referencia. Tenga en cuenta las diferentes reglas para los parámetros de salida que se describen en [parámetros de salida](variables.md#output-parameters).

*  Una variable debe estar asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) antes de que se pueda pasar como un parámetro de referencia en un miembro de función o una invocación de delegado.
*  Dentro de un miembro de función o una función anónima, se considera que un parámetro de referencia está asignado inicialmente.

Dentro de un método de instancia o un descriptor de acceso `this` de instancia de un tipo de estructura, la palabra clave se comporta exactamente como un parámetro de referencia del tipo de estructura ([este acceso](expressions.md#this-access)).

### <a name="output-parameters"></a>Parámetros de salida

Un parámetro declarado con un `out` modificador es un ***parámetro de salida***.

Un parámetro de salida no crea una nueva ubicación de almacenamiento. En su lugar, un parámetro de salida representa la misma ubicación de almacenamiento que la variable proporcionada como argumento en la invocación del miembro de función o delegado. Por lo tanto, el valor de un parámetro de salida es siempre el mismo que el de la variable subyacente.

Las siguientes reglas de asignación definitiva se aplican a los parámetros de salida. Tenga en cuenta las diferentes reglas para los parámetros de referencia descritos en [parámetros de referencia](variables.md#reference-parameters).

*  No es necesario asignar definitivamente una variable antes de que se pueda pasar como parámetro de salida en un miembro de función o en una invocación de delegado.
*  Tras la finalización normal de una invocación de un miembro de función o delegado, cada variable que se pasó como parámetro de salida se considera asignada en esa ruta de acceso de ejecución.
*  Dentro de un miembro de función o una función anónima, un parámetro de salida se considera inicialmente sin asignar.
*  Todos los parámetros de salida de un miembro de función o de una función anónima deben estar asignados definitivamente ([asignación definitiva](variables.md#definite-assignment)) antes de que el miembro de función o la función anónima devuelvan normalmente.

Dentro de un constructor de instancia de un tipo de `this` estructura, la palabra clave se comporta exactamente como un parámetro de salida del tipo de estructura ([este acceso](expressions.md#this-access)).

### <a name="local-variables"></a>Variables locales

Una ***variable local*** está declarada por un *local_variable_declaration*, que puede producirse en un *bloque*, *for_Statement*, *switch_statement* o *using_statement*; o bien, *foreach_statement* o *specific_catch_clause* para un *try_statement*.

La duración de una variable local es la parte de la ejecución del programa durante la que se garantiza que el almacenamiento está reservado para ella. Esta duración extiende al menos una entrada en el *bloque*, *for_Statement*, *switch_statement*, *using_statement*, *foreach_statement*o *specific_catch_clause* con el que está asociada, hasta que la ejecución de ese *bloque*, *for_Statement*, *switch_statement*, *using_statement*, *foreach_statement*o *specific_catch_clause* finaliza de forma alguna. (Si se escribe un *bloque* incluido o se llama a un método, se suspende, pero no finaliza, la ejecución del *bloque*actual, *for_Statement*, *switch_statement*, *using_statement*, *foreach_statement*o *specific_ catch_clause*). Si una función anónima captura la variable local ([variables externas capturadas](expressions.md#captured-outer-variables)), su duración se extiende al menos hasta que el delegado o árbol de expresión creado a partir de la función anónima, junto con cualquier otro objeto que se refiere a la la variable capturada, es válida para la recolección de elementos no utilizados.

Si el *bloque*primario, *for_Statement*, *switch_statement*, *using_statement*, *foreach_statement*o *specific_catch_clause* se escribe de forma recursiva, se crea una nueva instancia de la variable local. la hora y su *local_variable_initializer*, si existe, se evalúan cada vez.

Una variable local introducida por un *local_variable_declaration* no se inicializa automáticamente y, por tanto, no tiene ningún valor predeterminado. Con el fin de comprobar la asignación definitiva, una variable local introducida por una *local_variable_declaration* se considera inicialmente sin asignar. Un *local_variable_declaration* puede incluir un *local_variable_initializer*, en cuyo caso la variable se considera definitivamente asignada solo después de la expresión de inicialización ([instrucciones de declaración](variables.md#declaration-statements)).

Dentro del ámbito de una variable local introducida por un *local_variable_declaration*, se trata de un error en tiempo de compilación para hacer referencia a esa variable local en una posición textual que precede a su *local_variable_declarator*. Si la declaración de variable local es implícita ([declaraciones de variables locales](statements.md#local-variable-declarations)), también es un error hacer referencia a la variable dentro de su *local_variable_declarator*.

Una variable local introducida por un *foreach_statement* o un *specific_catch_clause* se considera definitivamente asignada en todo el ámbito.

La duración real de una variable local depende de la implementación. Por ejemplo, un compilador puede determinar estáticamente que una variable local de un bloque solo se usa para una pequeña parte de ese bloque. Con este análisis, el compilador podría generar código que hace que el almacenamiento de la variable tenga una duración más corta que el bloque contenedor.

El almacenamiento al que se hace referencia mediante una variable de referencia local se recupera independientemente de la duración de esa variable de referencia local ([Administración automática de memoria](basic-concepts.md#automatic-memory-management)).

## <a name="default-values"></a>Valores predeterminados

Las siguientes categorías de variables se inicializan automáticamente con sus valores predeterminados:

*  Variables estáticas.
*  Variables de instancia de instancias de clase.
*  Elementos de matriz.

El valor predeterminado de una variable depende del tipo de la variable y se determina de la manera siguiente:

*  En el caso de una variable de *value_type*, el valor predeterminado es el mismo que el valor calculado por el constructor predeterminado del *value_type*([constructores predeterminados](types.md#default-constructors)).
*  En el caso de una variable de *reference_type*, el valor `null`predeterminado es.

Normalmente, la inicialización en los valores predeterminados se realiza al hacer que el administrador de memoria o el recolector de elementos no utilizados inicialicen la memoria para todos los bits cero antes de que se asignen para su uso. Por esta razón, es conveniente usar All-bits-cero para representar la referencia nula.

## <a name="definite-assignment"></a>Asignación definitiva

En una ubicación determinada del código ejecutable de un miembro de función, se dice que una variable está ***asignada definitivamente*** si el compilador puede demostrar, mediante un análisis de flujo estático determinado ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)), que la variable se ha inicializado automáticamente o ha sido el destino de al menos una asignación. De manera informativa, las reglas de asignación definitiva son las siguientes:

*  Una variable asignada inicialmente ([variables asignadas inicialmente](variables.md#initially-assigned-variables)) siempre se considera asignada definitivamente.
*  Una variable no asignada inicialmente ([variables inicialmente sin asignar](variables.md#initially-unassigned-variables)) se considera definitivamente asignada en una ubicación determinada si todas las rutas de acceso de ejecución posibles que conducen a esa ubicación contienen al menos uno de los siguientes:
    * Asignación simple ([asignación simple](expressions.md#simple-assignment)) en la que la variable es el operando izquierdo.
    * Una expresión de invocación ([expresiones de invocación](expressions.md#invocation-expressions)) o una expresión de creación de objetos ([expresiones de creación de objetos](expressions.md#object-creation-expressions)) que pasa la variable como parámetro de salida.
    * En el caso de una variable local, es una declaración de variable local ([declaraciones de variables locales](statements.md#local-variable-declarations)) que incluye un inicializador de variable.

La especificación formal subyacente a las reglas informal anteriores se describe en [variables asignadas inicialmente](variables.md#initially-assigned-variables), [variables inicialmente sin asignar](variables.md#initially-unassigned-variables)y [reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment).

El seguimiento de los Estados de asignación definitiva de las variables de instancia de una variable *struct_type* se realiza de forma individual y colectiva. Además de las reglas anteriores, las siguientes reglas se aplican a las variables *struct_type* y sus variables de instancia:

*  Una variable de instancia se considera asignada definitivamente si su variable *struct_type* que contiene se considera asignada definitivamente.
*  Una variable *struct_type* se considera asignada definitivamente si cada una de sus variables de instancia se considera definitivamente asignada.

La asignación definitiva es un requisito en los contextos siguientes:

*  Una variable debe estar asignada definitivamente en cada ubicación donde se obtiene su valor. Esto garantiza que nunca se produzcan valores sin definir. La aparición de una variable en una expresión se considera que obtiene el valor de la variable, excepto cuando
    * la variable es el operando izquierdo de una asignación simple,
    * la variable se pasa como parámetro de salida, o bien
    * la variable es una variable *struct_type* y se produce como operando izquierdo de un acceso de miembro.
*  Una variable debe estar asignada definitivamente en cada ubicación en la que se pasa como parámetro de referencia. Esto garantiza que el miembro de función que se invoca puede considerar el parámetro de referencia asignado inicialmente.
*  Todos los parámetros de salida de un miembro de función deben asignarse definitivamente en cada ubicación en la que el miembro `return` de función devuelva (a través de una instrucción o a través de la ejecución hasta el final del cuerpo del miembro de función). Esto garantiza que los miembros de función no devuelven valores no definidos en los parámetros de salida, lo que permite al compilador considerar una invocación de miembro de función que toma una variable como parámetro de salida equivalente a una asignación a la variable.
*  La `this` variable de un constructor de instancia *struct_type* debe estar asignada definitivamente en cada ubicación en la que el constructor de instancia devuelva.

### <a name="initially-assigned-variables"></a>Variables asignadas inicialmente

Las siguientes categorías de variables se clasifican como asignadas inicialmente:

*  Variables estáticas.
*  Variables de instancia de instancias de clase.
*  Variables de instancia de las variables de struct asignadas inicialmente.
*  Elementos de matriz.
*  Parámetros de valor.
*  Parámetros de referencia.
*  Variables declaradas `catch` en una cláusula `foreach` o instrucción.

### <a name="initially-unassigned-variables"></a>Variables inicialmente sin asignar

Las siguientes categorías de variables se clasifican como sin asignar inicialmente:

*  Variables de instancia de variables de estructura inicialmente sin asignar.
*  Parámetros de salida, incluida `this` la variable de constructores de instancia de struct.
*  Variables locales, excepto las declaradas `catch` en una cláusula `foreach` o una instrucción.

### <a name="precise-rules-for-determining-definite-assignment"></a>Reglas precisas para determinar la asignación definitiva

Con el fin de determinar que cada variable usada está asignada definitivamente, el compilador debe utilizar un proceso equivalente al descrito en esta sección.

El compilador procesa el cuerpo de cada miembro de función que tiene una o varias variables sin asignar inicialmente. Para *cada variable de*inicial sin asignar, el compilador determina un ***Estado de asignación definitiva*** para *v* en cada uno de los siguientes puntos del miembro de función:

*  Al principio de cada instrucción
*  En el punto final ([puntos de conexión y disponibilidad](statements.md#end-points-and-reachability)) de cada instrucción
*  En cada arco que transfiere el control a otra instrucción o al punto final de una instrucción
*  Al principio de cada expresión
*  Al final de cada expresión

El estado de asignación definitiva de *v* puede ser:

*  Definitivamente asignada. Esto indica que en todos los flujos de control posibles hasta este punto, se ha asignado un valor a *v* .
*  No asignado definitivamente. Para el estado de una variable al final de una expresión de tipo `bool`, el estado de una variable que no está asignado definitivamente puede ser uno de los siguientes subestados:
    * Se asigna definitivamente después de la expresión true. Este estado indica que *v* se asigna definitivamente si la expresión booleana se evalúa como true, pero no se asigna necesariamente si la expresión booleana se evalúa como false.
    * Se asigna definitivamente después de la expresión false. Este estado indica que *v* se asigna definitivamente si la expresión booleana se evalúa como false, pero no se asigna necesariamente si la expresión booleana se evalúa como true.

Las siguientes reglas rigen cómo se determina el estado de una variable *v* en cada ubicación.

#### <a name="general-rules-for-statements"></a>Reglas generales para las instrucciones

*  *v* no se asigna definitivamente al principio del cuerpo de un miembro de función.
*  *v* se asigna definitivamente al principio de cualquier instrucción inalcanzable.
*  El estado de asignación definitiva de *v* al principio de cualquier otra instrucción se determina comprobando el estado de asignación definitiva de *v* en todas las transferencias de flujo de control que tienen como destino el inicio de la instrucción. Si (y solo si) *v* se asigna definitivamente en todas estas transferencias de flujo de control, entonces *v* se asigna definitivamente al principio de la instrucción. El conjunto de posibles transferencias de flujo de control se determina de la misma manera que para comprobar la disponibilidad de la instrucción (puntos de conexión[y disponibilidad](statements.md#end-points-and-reachability)).
*  El estado de asignación definitiva de *v* en el punto final de un bloque, `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach` `lock`,, `using`o `switch`la instrucción se determina comprobando el estado de asignación definitiva de *v* en todas las transferencias de flujo de control que tienen como destino el punto final de la instrucción. Si *v* se asigna definitivamente en todas estas transferencias de flujo de control, entonces *v* se asigna definitivamente en el punto final de la instrucción. Casos *v* no se asigna definitivamente en el punto final de la instrucción. El conjunto de posibles transferencias de flujo de control se determina de la misma manera que para comprobar la disponibilidad de la instrucción (puntos de conexión[y disponibilidad](statements.md#end-points-and-reachability)).

#### <a name="block-statements-checked-and-unchecked-statements"></a>Instrucciones de bloqueo, comprobadas y no comprobadas

El estado de asignación definitiva de *v* en la transferencia de control a la primera instrucción de la lista de instrucciones en el bloque (o al punto final del bloque, si la lista de instrucciones está vacía) es igual que la instrucción de asignación definitiva de *v* antes del bloque. instrucción `checked`, o `unchecked` .

#### <a name="expression-statements"></a>Instrucciones de expresión

Para una instrucción de expresión *stmt* que consta de la expresión *expr*:

*  *v* tiene el mismo estado de asignación definitiva al principio de *expr* como al principio de *stmt*.
*  Si *se* asigna definitivamente al final de *expr*, se asigna definitivamente en el punto final de *stmt*; casos no se asigna definitivamente en el punto final de *stmt*.

#### <a name="declaration-statements"></a>Instrucciones de declaración

*  Si *stmt* es una instrucción de declaración sin inicializadores, *v* tiene el mismo estado de asignación definitiva en el punto final de *stmt* que al principio de *stmt*.
*  Si *stmt* es una instrucción de declaración con inicializadores, el estado de asignación definitiva para *v* se determina como si *stmt* fuera una lista de instrucciones, con una instrucción de asignación para cada declaración con un inicializador (en el orden de Declaración).

#### <a name="if-statements"></a>Instrucciones if

Para una `if` instrucción *stmt* de la forma:
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* tiene el mismo estado de asignación definitiva al principio de *expr* como al principio de *stmt*.
*  Si *v* se asigna definitivamente al final de *expr*, se asigna definitivamente en la transferencia de flujo de control a *then_stmt* y a *else_stmt* o al punto final de *stmt* si no hay ninguna cláusula ELSE.
*  Si *v* tiene el estado "definitivamente asignado después de una expresión true" al final de *expr*, se asigna definitivamente en la transferencia del flujo de control a *then_stmt*y no se asigna definitivamente en la transferencia del flujo de control a *else_ stmt* o al punto final de *stmt* si no hay ninguna cláusula ELSE.
*  Si *v* tiene el estado "definitivamente asignado después de la expresión false" al final de *expr*, se asigna definitivamente en la transferencia del flujo de control a *else_stmt*y no se asigna definitivamente en la transferencia del flujo de control a *then_stmt* . Se asigna definitivamente en el punto final de *stmt* si y solo si se asigna definitivamente en el punto final de *then_stmt*.
*  De lo contrario, se considera que *v* no está asignado definitivamente en la transferencia de flujo de control a *then_stmt* o *else_stmt*, o al punto final de *stmt* si no hay ninguna cláusula ELSE.

#### <a name="switch-statements"></a>Instrucciones switch

En una `switch` instrucción *stmt* con una expresión de control *expr*:

*  El estado de asignación definitiva de *v* al principio de *expr* es el mismo que el estado de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* en la lista de transferencias de flujo de control a una lista de instrucciones del bloque switch es igual que el estado de asignación definitiva de *v* al final de *expr*.

#### <a name="while-statements"></a>While (instrucciones)

Para una `while` instrucción *stmt* de la forma:
```csharp
while ( expr ) while_body
```

*  *v* tiene el mismo estado de asignación definitiva al principio de *expr* como al principio de *stmt*.
*  Si *v* se asigna definitivamente al final de *expr*, se asigna definitivamente en la transferencia de flujo de control a *while_body* y al punto final de *stmt*.
*  Si *v* tiene el estado "definitivamente asignado después de una expresión true" al final de *expr*, se asigna definitivamente en la transferencia del flujo de control a *while_body*, pero no se asigna definitivamente en el punto final de *stmt*.
*  Si *v* tiene el estado "definitivamente asignado después de la expresión false" al final de *expr*, se asigna definitivamente en la transferencia del flujo de control al punto final de *stmt*, pero no se asigna definitivamente en la transferencia del flujo de control a *While _body*.

#### <a name="do-statements"></a>Do (instrucciones)

Para una `do` instrucción *stmt* de la forma:
```csharp
do do_body while ( expr ) ;
```

*  *v* tiene el mismo estado de asignación definitiva en la transferencia del flujo de control desde el principio de *stmt* a *do_body* como al principio de *stmt*.
*  *v* tiene el mismo estado de asignación definitiva al principio de *expr* como en el punto final de *do_body*.
*  Si *v* se asigna definitivamente al final de *expr*, se asigna definitivamente en la transferencia del flujo de control al punto final de *stmt*.
*  Si *v* tiene el estado "definitivamente asignado después de la expresión false" al final de *expr*, se asigna definitivamente en la transferencia del flujo de control al punto final de *stmt*.

#### <a name="for-statements"></a>Instrucciones for

Comprobación de asignación definitiva para una `for` instrucción de la forma:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
se realiza como si se hubiera escrito la instrucción:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Si *for_condition* se omite en la instrucción `for` , la evaluación de la asignación definitiva continúa como si *for_condition* se reemplazara por `true` en la expansión anterior.

#### <a name="break-continue-and-goto-statements"></a>Instrucciones break, continue y Goto

El estado de asignación definitiva de *v* en la transferencia de flujo de control provocada `continue`por una `goto` `break`instrucción, o es igual que el estado de asignación definitiva de *v* al principio de la instrucción.

#### <a name="throw-statements"></a>Instrucciones Throw

Para una instrucción *stmt* con el formato
```csharp
throw expr ;
```

El estado de asignación definitiva de *v* al principio de *expr* es igual que el estado de asignación definitiva de *v* al principio de *stmt*.

#### <a name="return-statements"></a>Instrucciones Return

Para una instrucción *stmt* con el formato
```csharp
return expr ;
```

*  El estado de asignación definitiva de *v* al principio de *expr* es igual que el estado de asignación definitiva de *v* al principio de *stmt*.
*  Si *v* es un parámetro de salida, se debe asignar definitivamente:
    * después de *expr*
    * o `finally` al final del bloque de`catch` o queincluye`try` la`return` instrucción. - `try` - `finally` - `finally`

Para una instrucción stmt de la forma:
```csharp
return ;
```

*  Si *v* es un parámetro de salida, se debe asignar definitivamente:
    * antes de *stmt*
    * o `finally` al final del bloque de`catch` o queincluye`try` la`return` instrucción. - `try` - `finally` - `finally`

#### <a name="try-catch-statements"></a>Instrucciones try-catch

Para una instrucción *stmt* de la forma:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  El estado de asignación definitiva de *v* al principio de *try_block* es el mismo que el estado de asignación definitiva de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* al principio de *catch_block_i* (para cualquier *i*) es el mismo que el estado de asignación definitiva de *v*al principio de *stmt*.
*  El estado de asignación definitiva de *v* en el punto final de *stmt* está asignado definitivamente si (y sólo si) *v* se asigna definitivamente en el punto final de  *try_block* y cada *catch_block_i* (para cada *i* de 1 a *n*).

#### <a name="try-finally-statements"></a>Try-finally (instrucciones)

Para una `try` instrucción *stmt* de la forma:
```csharp
try try_block finally finally_block
```

*  El estado de asignación definitiva de *v* al principio de *try_block* es el mismo que el estado de asignación definitiva de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* al principio de *finally_block* es el mismo que el estado de asignación definitiva de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* en el punto final de *stmt* se asigna definitivamente si (y solo si) se cumple al menos una de las siguientes condiciones:
    * *v* se asigna definitivamente en el punto final de *try_block*
    * *v* se asigna definitivamente en el punto final de *finally_block*

Si se realiza una transferencia de flujo de control ( `goto` por ejemplo, una instrucción) que comienza dentro de *try_block*y finaliza fuera de *try_block*, también se considera que *v* está asignado definitivamente en esa transferencia de flujo de control si *v* es se asigna definitivamente en el extremo de *finally_block*. (Esto no es solo si, si *v* se asigna definitivamente por otro motivo en esta transferencia de flujo de control, todavía se considera asignada definitivamente).

#### <a name="try-catch-finally-statements"></a>Instrucciones try-catch-finally

Análisis de asignación definitiva para una `try` - `catch` - instrucción delaforma:`finally`
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
se realiza como si la instrucción fuera una `try` - `try` - `finally` instrucción que incluye una `catch` instrucción:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

En el ejemplo siguiente se muestra cómo los distintos bloques `try` de una instrucción ([la instrucción try](statements.md#the-try-statement)) afectan a la asignación definitiva.
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

Para una `foreach` instrucción *stmt* de la forma:
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  El estado de asignación definitiva de *v* al principio de *expr* es el mismo que el estado de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* en la transferencia de flujo de control a *embedded_statement* o al punto final de *stmt* es el mismo que el estado de *v* al final de *expr*.

#### <a name="using-statements"></a>Instrucciones Using

Para una `using` instrucción *stmt* de la forma:
```csharp
using ( resource_acquisition ) embedded_statement
```

*  El estado de asignación definitiva de *v* al principio de *resource_acquisition* es el mismo que el estado de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* en la transferencia de flujo de control a *embedded_statement* es el mismo que el estado de *v* al final de *resource_acquisition*.

#### <a name="lock-statements"></a>Lock (instrucciones)

Para una `lock` instrucción *stmt* de la forma:
```csharp
lock ( expr ) embedded_statement
```

*  El estado de asignación definitiva de *v* al principio de *expr* es el mismo que el estado de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* en la transferencia de flujo de control a *embedded_statement* es el mismo que el estado de *v* al final de *expr*.

#### <a name="yield-statements"></a>Yield (instrucciones)

Para una `yield return` instrucción *stmt* de la forma:
```csharp
yield return expr ;
```

*  El estado de asignación definitiva de *v* al principio de *expr* es el mismo que el estado de *v* al principio de *stmt*.
*  El estado de asignación definitiva de *v* al final de *stmt* es el mismo que el estado de *v* al final de *expr*.
*  Una `yield break` instrucción no tiene ningún efecto en el estado de asignación definitiva.

#### <a name="general-rules-for-simple-expressions"></a>Reglas generales para expresiones simples

La siguiente regla se aplica a estos tipos de expresiones: literales ([literales](expressions.md#literals)), nombres simples ([nombres simples](expressions.md#simple-names)), expresiones de acceso a miembros ([acceso a miembros](expressions.md#member-access)), expresiones de acceso base no indizadas ([acceso base](expressions.md#base-access)) `typeof`.expresiones ([el operador typeof](expressions.md#the-typeof-operator)), expresiones de valor predeterminado ([expresiones de valor predeterminado](expressions.md#default-value-expressions)) `nameof` y expresiones ([nombre de expresiones](expressions.md#nameof-expressions)).

*  El estado de asignación definitiva de *v* al final de dicha expresión es el mismo que el estado de asignación definitiva de *v* al principio de la expresión.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Reglas generales para expresiones con expresiones incrustadas

Las siguientes reglas se aplican a estos tipos de expresiones: expresiones entre paréntesis ([expresiones entre paréntesis](expressions.md#parenthesized-expressions)), expresiones de acceso a elementos ([acceso a elementos](expressions.md#element-access)), expresiones de acceso base con indexación ([acceso base](expressions.md#base-access)), incremento y Expresiones de decremento ([operadores de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) [postfijo, operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)), expresiones de conversión ([expresiones de conversión](expressions.md#cast-expressions)), `+` unario, `-`, `~`, expresiones `*`, `+` binario, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, expresiones `^` ([operadores aritméticos](expressions.md#arithmetic-operators), [operadores de desplazamiento](expressions.md#shift-operators), [operadores relacionales y de pruebas de tipos](expressions.md#relational-and-type-testing-operators), [operadores lógicos](expressions.md#logical-operators)), expresiones de asignación de compuestos ([asignación de compuestos](expressions.md#compound-assignment)), `checked` y expresiones `unchecked` ([operadores comprobados y sin comprobar](expressions.md#the-checked-and-unchecked-operators)), más matrices y delegado Expresiones de creación ([operador New](expressions.md#the-new-operator)).

Cada una de estas expresiones tiene una o más subexpresiones que se evalúan de forma incondicional en un orden fijo. Por ejemplo, el operador `%` binario evalúa el lado izquierdo del operador y, a continuación, el lado derecho. Una operación de indización evalúa la expresión indizada y, a continuación, evalúa cada una de las expresiones de índice, en orden de izquierda a derecha. En el caso de una expresión *expr*, que tiene subexpresiones *E1, E2,...,* en, evaluadas en ese orden:

*  El estado de asignación definitiva de *v* al principio de *E1* es el mismo que el estado de asignación definitiva al principio de *expr*.
*  El estado de asignación definitiva de *v* al principio de *ei* (*i* mayor que uno) es el mismo que el estado de asignación definitiva al final de la subexpresión anterior.
*  El estado de asignación definitiva de *v* al final de *expr* es el mismo que el estado de asignación definitiva al final de *en*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Expresiones de invocación y expresiones de creación de objetos

Para *una expresión de invocación de la* forma:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
o una expresión de creación de objeto con el formato:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  En el caso de una expresión de invocación, el estado de asignación definitiva de *v* antes de *primary_expression* es el mismo que el estado de *v* antes de *expr*.
*  En el caso de una expresión de invocación, el estado de asignación definitiva de *v* antes de *arg1* es el mismo que el estado de *v* después de *primary_expression*.
*  En el caso de una expresión de creación de objeto, el estado de asignación definitiva de *v* antes de *arg1* es el mismo que el estado de *v* antes de *expr*.
*  Para cada argumento *Argi*, el estado de asignación definitiva de *v* después de *Argi* viene determinado por las reglas de la expresión normal, `ref` omitiendo `out` los modificadores o.
*  Para cada argumento *argi* para cualquier *i* mayor que uno, el estado de asignación definitiva de *v* antes *argi* es el mismo que el estado de *v* después de la anterior *arg*.
*  Si la variable *v* se `out` pasa como argumento (es decir, un argumento con el `out v`formato) en cualquiera de los argumentos, el estado de *v* después de la *expresión* se asigna definitivamente. Casos el estado de *v* después de *expr* es el mismo que el estado de *v* después de *argn*.
*  Para inicializadores de matriz ([expresiones de creación de matriz](expressions.md#array-creation-expressions)), inicializadores de objeto ([inicializadores de objeto](expressions.md#object-initializers)), inicializadores de colección ([inicializadores de colección](expressions.md#collection-initializers)) e inicializadores de objeto anónimos (creación de[objetos anónimos Expresiones](expressions.md#anonymous-object-creation-expressions)), el estado de asignación definitiva viene determinado por la expansión en la que se definen estas construcciones en términos de.

#### <a name="simple-assignment-expressions"></a>Expresiones de asignación simples

Para una expresión *expr* con el formato `w = expr_rhs`:

*  El estado de asignación definitiva de *v* antes de *expr_rhs* es el mismo que el estado de asignación definitiva de *v* antes de *expr*.
*  El estado de asignación definitiva de *v* después de *expr* viene determinado por:
   * Si *w* es la misma variable que *v*, se asigna definitivamente el estado de asignación definitiva de *v* después de *expr* .
   * De lo contrario, si la asignación se produce en el constructor de instancia de un tipo de estructura, si *w* es un acceso de propiedad que designa una propiedad implementada automáticamente *P* en la instancia que se está construyendo y *v* es el campo de respaldo oculto de *P*, después se asigna definitivamente el estado de asignación definitiva de *v* después de *expr* .
   * De lo contrario, el estado de asignación definitiva de *v* después de *expr* es el mismo que el estado de asignación definitiva de *v* después de *expr_rhs*.

#### <a name="-conditional-and-expressions"></a>Expresiones de & & (condicional AND)

Para una expresión *expr* con el formato `expr_first && expr_second`:

*  El estado de asignación definitiva de *v* antes de *expr_first* es el mismo que el estado de asignación definitiva de *v* antes de *expr*.
*  El estado de asignación definitiva de *v* antes de que *expr_second* se asigne definitivamente si el estado de *v* después de *expr_first* se asigna definitivamente o "se asigna definitivamente después de una expresión true". De lo contrario, no se asigna definitivamente.
*  El estado de asignación definitiva de *v* después de *expr* viene determinado por:
    * Si *expr_first* es una expresión constante con el valor `false`, el estado de asignación definitiva de *v* después de *expr* es igual que el estado de asignación definitiva de *v* después de *expr_first*.
    * De lo contrario, si el estado de *v* después de *expr_first* se asigna definitivamente, el estado de *v* después de la *expresión* se asigna definitivamente.
    * De lo contrario, si el estado de *v* después de *expr_second* está asignado definitivamente, y el estado de *v* después de *expr_first* es "definitivamente asignado después de la expresión false", el estado de *v* después de *expr* es definitivamente. vuelve.
    * De lo contrario, si el estado de *v* después de *expr_second* se asigna definitivamente o "se asigna definitivamente después de una expresión true", el estado de *v* después de *expr* es "definitivamente asignado después de una expresión true".
    * De lo contrario, si el estado de *v* después de *expr_first* es "definitivamente asignado después de la expresión false" y el estado de *v* después de *expr_second* es "definitivamente asignado después de la expresión false", el estado de *v* después *de expr* es "definitivamente asignada después de la expresión false".
    * De lo contrario, el estado de *v* después de *expr* no se asigna definitivamente.

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
la variable `i` se considera asignada definitivamente en una de las instrucciones incrustadas `if` de una instrucción, pero no en la otra. En la `if` instrucción de método `F`, la variable `i` se asigna definitivamente en la primera instrucción incrustada porque la ejecución de `(i = y)` la expresión siempre precede a la ejecución de esta instrucción incrustada. Por el contrario, la `i` variable no está asignada definitivamente en la segunda instrucción incrustada, puesto `x >= 0` que podría haber probado false, `i` lo que da lugar a que se Desasigne la variable.

#### <a name="-conditional-or-expressions"></a>|| expresiones (condicionales OR)

Para una expresión *expr* con el formato `expr_first || expr_second`:

*  El estado de asignación definitiva de *v* antes de *expr_first* es el mismo que el estado de asignación definitiva de *v* antes de *expr*.
*  El estado de asignación definitiva de *v* antes de que *expr_second* se asigne definitivamente si el estado de *v* después de *expr_first* se asigna definitivamente o "se asigna definitivamente después de la expresión false". De lo contrario, no se asigna definitivamente.
*  La instrucción de asignación definitiva de *v* después de *expr* viene determinada por:
    * Si *expr_first* es una expresión constante con el valor `true`, el estado de asignación definitiva de *v* después de *expr* es igual que el estado de asignación definitiva de *v* después de *expr_first*.
    * De lo contrario, si el estado de *v* después de *expr_first* se asigna definitivamente, el estado de *v* después de la *expresión* se asigna definitivamente.
    * De lo contrario, si el estado de *v* después de *expr_second* se asigna definitivamente, y el estado de *v* después de *expr_first* es "definitivamente asignado después de una expresión true", el estado de *v* después de *expr* es definitivamente. vuelve.
    * De lo contrario, si el estado de *v* después de *expr_second* se asigna definitivamente o "se asigna definitivamente después de la expresión false", el estado de *v* después de *expr* es "definitivamente asignado después de la expresión false".
    * De lo contrario, si el estado de *v* después de *expr_first* es "definitivamente asignado después de una expresión true" y el estado de *v* después de *expr_second* es "definitivamente asignado después de una expresión true", el estado de *v* después de *expr* está "definitivamente asignada después de una expresión true".
    * De lo contrario, el estado de *v* después de *expr* no se asigna definitivamente.

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
la variable `i` se considera asignada definitivamente en una de las instrucciones incrustadas `if` de una instrucción, pero no en la otra. En la `if` instrucción de método `G`, la variable `i` se asigna definitivamente en la segunda instrucción incrustada porque la ejecución de `(i = y)` la expresión siempre precede a la ejecución de esta instrucción incrustada. Por el contrario, la `i` variable no está asignada definitivamente en la primera instrucción incrustada, ya `x >= 0` que es posible que se haya `i` probado como true, lo que da lugar a que se Desasigne la variable.

#### <a name="-logical-negation-expressions"></a>! expresiones (negación lógica)

Para una expresión *expr* con el formato `! expr_operand`:

*  El estado de asignación definitiva de *v* antes de *expr_operand* es el mismo que el estado de asignación definitiva de *v* antes de *expr*.
*  El estado de asignación definitiva de *v* después de *expr* viene determinado por:
    * Si el estado de *v* después de * expr_operand * se asigna definitivamente, el estado de *v* después de la *expresión* se asigna definitivamente.
    * Si el estado de *v* después de * expr_operand * no se asigna definitivamente, el estado de *v* después de *expr* no se asigna definitivamente.
    * Si el estado de *v* después de * expr_operand * es "definitivamente asignado después de la expresión false", el estado de *v* después de *expr* es "definitivamente asignado después de una expresión true".
    * Si el estado de *v* después de * expr_operand * es "definitivamente asignado después de una expresión true", el estado de *v* después de *expr* es "definitivamente asignado después de la expresión false".

#### <a name="-null-coalescing-expressions"></a>?? expresiones (uso combinado de NULL)

Para una expresión *expr* con el formato `expr_first ?? expr_second`:

*  El estado de asignación definitiva de *v* antes de *expr_first* es el mismo que el estado de asignación definitiva de *v* antes de *expr*.
*  El estado de asignación definitiva de *v* antes de *expr_second* es el mismo que el estado de asignación definitiva de *v* después de *expr_first*.
*  La instrucción de asignación definitiva de *v* después de *expr* viene determinada por:
    * Si *expr_first* es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)) con valor null, el estado de *v* después de *expr* es el mismo que el estado de *v* después de *expr_second*.
*  De lo contrario, el estado de *v* después de *expr* es el mismo que el estado de asignación definitiva de *v* después de *expr_first*.

#### <a name="-conditional-expressions"></a>expresiones?: (condicionales)

Para una expresión *expr* con el formato `expr_cond ? expr_true : expr_false`:

*  El estado de asignación definitiva de *v* antes de *expr_cond* es el mismo que el estado de *v* antes de *expr*.
*  El estado de asignación definitiva de *v* antes de *expr_true* se asigna definitivamente solo si se cumple una de las siguientes opciones:
    * *expr_cond* es una expresión constante con el valor`false`
    * el estado de *v* después de *expr_cond* se asigna definitivamente o "definitivamente se asigna después de una expresión true".
*  El estado de asignación definitiva de *v* antes de *expr_false* se asigna definitivamente solo si se cumple una de las siguientes opciones:
    * *expr_cond* es una expresión constante con el valor`true`
*  el estado de *v* después de *expr_cond* se asigna definitivamente o "se asigna definitivamente después de la expresión false".
*  El estado de asignación definitiva de *v* después de *expr* viene determinado por:
    * Si *expr_cond* es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)) con valor `true` , el estado de *v* después de *expr* es el mismo que el estado de *v* después de *expr_true*.
    * De lo contrario, si *expr_cond* es una expresión constante ([expresiones constantes](expressions.md#constant-expressions)) `false` con valor, el estado de *v* después de *expr* es el mismo que el estado de *v* después de *expr_false*.
    * De lo contrario, si el estado de *v* después de *expr_true* se asigna definitivamente y el estado de *v* después de *expr_false* se asigna definitivamente, el estado de *v* *después de* la asignación definitivamente se asigna.
    * De lo contrario, el estado de *v* después de *expr* no se asigna definitivamente.

#### <a name="anonymous-functions"></a>Funciones anónimas

Para una *expresión* *lambda_expression* o *anonymous_method_expression* con un cuerpo ( *bloque* o *expresión*) *cuerpo*:

*  El estado de asignación definitiva de una variable externa *v* antes del *cuerpo* es el mismo que el estado de *v* antes de *expr*. Es decir, el estado de asignación definitiva de las variables externas se hereda del contexto de la función anónima.
*  El estado de asignación definitiva de una variable externa *v* después de *expr* es igual que el estado de *v* antes de *expr*.

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
genera un error en tiempo de compilación `max` , ya que no se asigna definitivamente donde se declara la función anónima. El ejemplo
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
también genera un error en tiempo de compilación, ya que `n` la asignación a en la función anónima no tiene ningún efecto en el estado `n` de asignación definitiva de fuera de la función anónima.

## <a name="variable-references"></a>Referencias de variables

Un *variable_reference* es una *expresión* que se clasifica como una variable. Un *variable_reference* denota una ubicación de almacenamiento a la que se puede tener acceso para obtener el valor actual y para almacenar un nuevo valor.

```antlr
variable_reference
    : expression
    ;
```

En C y C++, una *variable_reference* se conoce como *lvalue*.

## <a name="atomicity-of-variable-references"></a>Atomicidad de referencias de variables

Las lecturas y escrituras de los siguientes tipos de datos son `bool`atómicas `byte`: `sbyte`, `short` `char`,, `uint`, `int`, `float` `ushort`,,, y los tipos de referencia. Además, las operaciones de lectura y escritura de tipos de enumeración con un tipo subyacente en la lista anterior también son atómicas. No se garantiza que las lecturas y escrituras `long`de `ulong`otros tipos, `decimal`incluidos,, `double`y, así como los tipos definidos por el usuario, sean atómicos. Aparte de las funciones de biblioteca diseñadas para ese propósito, no hay ninguna garantía de lectura-modificación-escritura atómica, como en el caso de incremento o decremento.

