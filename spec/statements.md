---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704041"
---
# <a name="statements"></a>Instrucciones

C#proporciona una serie de instrucciones. La mayoría de estas instrucciones resultará familiar a los programadores que hayan programado C++en C y.

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

*Embedded_statement* nonterminal se utiliza para las instrucciones que aparecen dentro de otras instrucciones. El uso de *embedded_statement* en lugar de *Statement* excluye el uso de las instrucciones de declaración y las instrucciones con etiquetas en estos contextos. El ejemplo
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
produce un error en tiempo de compilación porque una instrucción `if` requiere un *embedded_statement* en lugar de una *instrucción* para su bifurcación if. Si se permitía este código, la variable `i` se declararía, pero nunca se podía usar. Tenga en cuenta, sin embargo, `i`que si coloca la declaración de en un bloque, el ejemplo es válido.

## <a name="end-points-and-reachability"></a>Extremos y disponibilidad

Cada instrucción tiene un ***punto final***. En términos intuitivos, el punto final de una instrucción es la ubicación que sigue inmediatamente a la instrucción. Las reglas de ejecución para las instrucciones compuestas (instrucciones que contienen instrucciones incrustadas) especifican la acción que se realiza cuando el control alcanza el punto final de una instrucción incrustada. Por ejemplo, cuando el control alcanza el punto final de una instrucción en un bloque, el control se transfiere a la siguiente instrucción del bloque.

Si la ejecución puede alcanzar una instrucción, se dice que la instrucción es ***accesible***. Por el contrario, si no hay ninguna posibilidad de que se ejecute una instrucción, se dice que la instrucción no es ***accesible***.

En el ejemplo
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
la segunda invocación de `Console.WriteLine` es inaccesible porque no existe ninguna posibilidad de que se ejecute la instrucción.

Se genera una advertencia si el compilador determina que no se puede tener acceso a una instrucción. No se trata de un error específico para que no se pueda tener acceso a una instrucción.

Para determinar si una instrucción o un extremo concreto es accesible, el compilador realiza el análisis de flujo según las reglas de disponibilidad definidas para cada instrucción. El análisis de flujo tiene en cuenta los valores de expresiones constantes ([expresiones constantes](expressions.md#constant-expressions)) que controlan el comportamiento de las instrucciones, pero no se tienen en cuenta los valores posibles de las expresiones que no son constantes. En otras palabras, a efectos del análisis de flujo de control, se considera que una expresión no constante de un tipo determinado tiene cualquier valor posible de ese tipo.

En el ejemplo
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
la expresión booleana de la `if` instrucción es una expresión constante porque ambos operandos `==` del operador son constantes. Como la expresión constante se evalúa en tiempo de compilación, lo que genera `false`el valor `Console.WriteLine` , se considera que la invocación no es accesible. Sin embargo, `i` si se cambia para ser una variable local
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
la `Console.WriteLine` invocación se considera accesible, aunque, en realidad, nunca se ejecutará.

El *bloque* de un miembro de función siempre se considera alcanzable. Al evaluar sucesivamente las reglas de disponibilidad de cada instrucción en un bloque, se puede determinar la disponibilidad de cualquier instrucción determinada.

En el ejemplo
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
la disponibilidad de la segunda `Console.WriteLine` se determina de la siguiente manera:

*  La primera `Console.WriteLine` instrucción `F` de expresión es accesible porque el bloque del método es accesible.
*  El punto final de la primera `Console.WriteLine` instrucción de expresión es accesible porque esa instrucción es accesible.
*  La `if` instrucción es accesible porque el punto final de la primera `Console.WriteLine` instrucción de expresión es accesible.
*  La segunda `Console.WriteLine` instrucción de expresión es accesible porque la expresión booleana de la `if` instrucción no tiene el valor `false`constante.

Hay dos situaciones en las que se trata de un error en tiempo de compilación para que el punto final de una instrucción sea alcanzable:

*  Dado que `switch` la instrucción no permite que una sección switch pase a la siguiente sección switch, se trata de un error en tiempo de compilación para que el punto final de la lista de instrucciones de una sección switch sea accesible. Si se produce este error, normalmente es una indicación de que `break` falta una instrucción.
*  Es un error en tiempo de compilación el punto final del bloque de un miembro de función que calcula un valor al que se puede tener acceso. Si se produce este error, normalmente es una indicación de que `return` falta una instrucción.

## <a name="blocks"></a>Bloques

Un *bloque* permite que se escriban varias instrucciones en contextos donde se permite una única instrucción.

```antlr
block
    : '{' statement_list? '}'
    ;
```

Un *bloque* consta de un *statement_list* opcional ([listas de instrucciones](statements.md#statement-lists)), entre llaves. Si se omite la lista de instrucciones, se dice que el bloque está vacío.

Un bloque puede contener instrucciones de declaración ([instrucciones de declaración](statements.md#declaration-statements)). El ámbito de una variable local o constante declarada en un bloque es el bloque.

Un bloque se ejecuta de la siguiente manera:

*  Si el bloque está vacío, el control se transfiere al punto final del bloque.
*  Si el bloque no está vacío, el control se transfiere a la lista de instrucciones. Cuando y si el control alcanza el punto final de la lista de instrucciones, el control se transfiere al punto final del bloque.

La lista de instrucciones de un bloque es accesible si el propio bloque es accesible.

El punto final de un bloque es accesible si el bloque está vacío o si el punto final de la lista de instrucciones es accesible.

Un *bloque* que contiene una o más `yield` instrucciones ([la instrucción yield](statements.md#the-yield-statement)) se denomina bloque de iteradores. Los bloques de iterador se usan para implementar miembros de función como iteradores ([iteradores](classes.md#iterators)). Se aplican algunas restricciones adicionales a los bloques de iteradores:

*  Se trata de un error en tiempo de compilación `return` para que una instrucción aparezca en un bloque de `yield return` iteradores (pero se permiten las instrucciones).
*  Es un error en tiempo de compilación que un bloque de iteradores contenga un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)). Un bloque de iterador siempre define un contexto seguro, incluso cuando su declaración está anidada en un contexto no seguro.

### <a name="statement-lists"></a>Listas de instrucciones

Una ***lista de instrucciones*** consta de una o varias instrucciones escritas en secuencia. Las listas de instrucciones aparecen en *bloque*s ([bloques](statements.md#blocks)) y en *switch_block*s ([la instrucción switch](statements.md#the-switch-statement)).

```antlr
statement_list
    : statement+
    ;
```

Una lista de instrucciones se ejecuta mediante la transferencia de control a la primera instrucción. Cuando y si el control alcanza el punto final de una instrucción, el control se transfiere a la siguiente instrucción. Cuando el control alcanza el punto final de la última instrucción, se transfiere el control al punto final de la lista de instrucciones.

Se puede tener acceso a una instrucción de una lista de instrucciones si se cumple al menos una de las siguientes condiciones:

*  La instrucción es la primera instrucción y la propia lista de instrucciones es accesible.
*  El punto final de la instrucción anterior es accesible.
*  La instrucción es una instrucción con etiqueta y una `goto` instrucción accesible hace referencia a la etiqueta.

El punto final de una lista de instrucciones es accesible si el punto final de la última instrucción de la lista es accesible.

## <a name="the-empty-statement"></a>Instrucción vacía

Un *empty_statement* no hace nada.

```antlr
empty_statement
    : ';'
    ;
```

Se utiliza una instrucción vacía cuando no hay ninguna operación que realizar en un contexto donde se requiere una instrucción.

La ejecución de una instrucción vacía simplemente transfiere el control al punto final de la instrucción. Por lo tanto, el punto final de una instrucción vacía es accesible si la instrucción vacía es accesible.

Se puede usar una instrucción vacía al escribir una `while` instrucción con un cuerpo nulo:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Además, se puede usar una instrucción vacía para declarar una etiqueta justo antes del cierre "`}`" de un bloque:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Instrucciones con etiqueta

Un *labeled_statement* permite a una instrucción ir precedida por una etiqueta. Las instrucciones con etiqueta se permiten en bloques, pero no se permiten como instrucciones incrustadas.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Una instrucción con etiqueta declara una etiqueta con el nombre proporcionado por el *identificador*. El ámbito de una etiqueta es el bloque entero en el que se declara la etiqueta, incluidos los bloques anidados. Es un error en tiempo de compilación que dos etiquetas con el mismo nombre tienen ámbitos superpuestos.

Se puede hacer referencia a una etiqueta `goto` desde instrucciones ([instrucción Goto](statements.md#the-goto-statement)) dentro del ámbito de la etiqueta. Esto significa que `goto` las instrucciones pueden transferir el control dentro de los bloques y fuera de los bloques, pero nunca a los bloques.

Las etiquetas tienen su propio espacio de declaración y no interfieren con otros identificadores. El ejemplo
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
es válido y usa el nombre `x` como un parámetro y una etiqueta.

La ejecución de una instrucción con etiqueta corresponde exactamente a la ejecución de la instrucción que sigue a la etiqueta.

Además de la disponibilidad proporcionada por el flujo de control normal, se puede tener acceso a una instrucción con etiqueta si se hace referencia a la etiqueta mediante una `goto` instrucción alcanzable. Excepcional Si una `goto` instrucción está dentro de `try` un que incluye `finally` un bloque y la instrucción con etiqueta está fuera `finally` de `try`, y no se puede tener acceso al punto final del bloque, la instrucción con etiqueta no es accesible desde esa `goto` instrucción).

## <a name="declaration-statements"></a>Instrucciones de declaración

Un *declaration_statement* declara una variable o constante local. Las instrucciones de declaración se permiten en bloques, pero no se permiten como instrucciones incrustadas.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Declaraciones de variables locales

Un *local_variable_declaration* declara una o varias variables locales.

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

El *local_variable_type* de un *local_variable_declaration* especifica directamente el tipo de las variables introducidas por la declaración, o indica con el identificador `var` que el tipo se debe inferir en función de un inicializador. El tipo va seguido de una lista de *local_variable_declarator*s, cada uno de los cuales introduce una nueva variable. Un *local_variable_declarator* consta de un *identificador* que asigna un nombre a la variable, seguido opcionalmente por un token "`=`" y un *local_variable_initializer* que proporciona el valor inicial de la variable.

En el contexto de una declaración de variable local, el identificador var actúa como una palabra clave contextual ([palabras clave](lexical-structure.md#keywords)). Cuando *local_variable_type* se especifica como `var` y ningún tipo denominado `var` está en el ámbito, la declaración es una ***declaración de variable local con tipo implícito***, cuyo tipo se deduce del tipo de la expresión de inicializador asociada. Las declaraciones de variables locales con tipo implícito están sujetas a las siguientes restricciones:

*  *Local_variable_declaration* no puede incluir varios *local_variable_declarator*.
*  *Local_variable_declarator* debe incluir un *local_variable_initializer*.
*  *Local_variable_initializer* debe ser una *expresión*.
*  La *expresión* de inicializador debe tener un tipo en tiempo de compilación.
*  La *expresión* de inicializador no puede hacer referencia a la variable declarada en sí misma

A continuación se muestran ejemplos de declaraciones de variables locales con tipo implícito incorrectas:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

El valor de una variable local se obtiene en una expresión usando *simple_name* ([nombres simples](expressions.md#simple-names)) y el valor de una variable local se modifica mediante una *asignación* (operadores de[asignación](expressions.md#assignment-operators)). Una variable local debe estar asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) en cada ubicación donde se obtiene su valor.

El ámbito de una variable local declarada en un *local_variable_declaration* es el bloque en el que se produce la declaración. Es un error hacer referencia a una variable local en una posición textual que precede al *local_variable_declarator* de la variable local. Dentro del ámbito de una variable local, se trata de un error en tiempo de compilación para declarar otra variable o constante local con el mismo nombre.

Una declaración de variable local que declara varias variables es equivalente a varias declaraciones de variables únicas con el mismo tipo. Además, un inicializador de variable en una declaración de variable local se corresponde exactamente con una instrucción de asignación que se inserta inmediatamente después de la declaración.

El ejemplo
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
corresponde exactamente a
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

En una declaración de variable local con tipo implícito, el tipo de la variable local que se está declarando se toma como el mismo tipo de la expresión que se usa para inicializar la variable. Por ejemplo:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

Las declaraciones de variables locales con tipo implícito anterior son exactamente equivalentes a las siguientes declaraciones con tipo explícito:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Declaraciones de constantes locales

Un *local_constant_declaration* declara una o más constantes locales.

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

El *tipo* de *local_constant_declaration* especifica el tipo de las constantes introducidas por la declaración. El tipo va seguido de una lista de *constant_declarator*s, cada uno de los cuales introduce una nueva constante. Un *constant_declarator* consta de un *identificador* que nombra la constante, seguido de un token "`=`", seguido de un *constant_expression* ([expresiones constantes](expressions.md#constant-expressions)) que proporciona el valor de la constante.

El *tipo* y *constant_expression* de una declaración de constante local deben seguir las mismas reglas que las de una declaración de miembro constante ([constantes](classes.md#constants)).

El valor de una constante local se obtiene en una expresión usando *simple_name* ([nombres simples](expressions.md#simple-names)).

El ámbito de una constante local es el bloque en el que se produce la declaración. Es un error hacer referencia a una constante local en una posición textual que precede a su *constant_declarator*. Dentro del ámbito de una constante local, es un error en tiempo de compilación declarar otra variable o constante local con el mismo nombre.

Una declaración de constante local que declara varias constantes es equivalente a varias declaraciones de constantes únicas con el mismo tipo.

## <a name="expression-statements"></a>Instrucciones de expresión

Un *expression_statement* evalúa una expresión determinada. El valor calculado por la expresión, si existe, se descarta.

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

No todas las expresiones se permiten como instrucciones. En concreto, las expresiones `x + y` como y `x == 1` que simplemente calculan un valor (que se descartarán) no se permiten como instrucciones.

La ejecución de un *expression_statement* evalúa la expresión contenida y, a continuación, transfiere el control al punto final de *expression_statement*. El punto final de un *expression_statement* es accesible si ese *expression_statement* es accesible.

## <a name="selection-statements"></a>Instrucciones de selección

Las instrucciones de selección seleccionan una de varias instrucciones posibles para su ejecución según el valor de alguna expresión.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>La instrucción If

La `if` instrucción selecciona una instrucción para su ejecución basada en el valor de una expresión booleana.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Un `else` elemento está asociado a la parte anterior `if` léxicamente más cercana permitida por la sintaxis. Por lo tanto `if` , una instrucción con el formato
```csharp
if (x) if (y) F(); else G();
```
es equivalente a
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

Una `if` instrucción se ejecuta de la siguiente manera:

*  Se evalúa *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)).
*  Si la expresión booleana produce `true`, el control se transfiere a la primera instrucción incrustada. Cuando y si el control alcanza el punto final de la instrucción, el control se transfiere al punto final de `if` la instrucción.
*  Si la expresión booleana produce `false` y si un `else` elemento está presente, el control se transfiere a la segunda instrucción incrustada. Cuando y si el control alcanza el punto final de la instrucción, el control se transfiere al punto final de `if` la instrucción.
*  Si la expresión booleana produce `false` y si un `else` elemento no está presente, el control se transfiere al punto final de la `if` instrucción.

La primera instrucción incrustada de `if` una instrucción es accesible si la `if` instrucción es accesible y la expresión booleana no tiene el valor `false`constante.

La segunda instrucción incrustada de `if` una instrucción, si está presente, es accesible si `if` la instrucción es accesible y la expresión booleana no tiene el valor `true`constante.

El punto final de una `if` instrucción es accesible si el punto final de al menos una de sus instrucciones incrustadas es accesible. Además, el `if` punto final de una instrucción sin ninguna `else` parte es accesible si la `if` instrucción es accesible y la expresión booleana no tiene el valor `true`constante.

### <a name="the-switch-statement"></a>La instrucción switch

La instrucción switch selecciona la ejecución de una lista de instrucciones que tiene una etiqueta de conmutador asociada que corresponde al valor de la expresión switch.

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

Un *switch_statement* consta de la palabra clave `switch`, seguido de una expresión entre paréntesis (denominada expresión switch), seguida de un *switch_block*. *Switch_block* consta de cero o más *switch_section*s, entre llaves. Cada *switch_section* consta de una o más *switch_label*s seguidos de una *statement_list* ([listas de instrucciones](statements.md#statement-lists)).

El ***tipo de control*** de `switch` una instrucción se establece mediante la expresión switch.

*  Si el tipo de la expresión switch es `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, 0 o un *enum_type*, o si es el tipo que acepta valores NULL correspondiente a uno de estos tipos , que es el tipo de control de la instrucción 2.
*  De lo contrario, debe existir exactamente una conversión implícita definida por el usuario ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)) del tipo de la expresión switch a uno de los siguientes tipos de `sbyte`control `byte`posibles: `ushort` ,, `short`, , `int`, `uint`, ,`long` ,`char`,o, un tipo que acepta valores NULL correspondiente a uno de esos tipos. `ulong` `string`
*  De lo contrario, si no existe ninguna conversión implícita, o si existe más de una conversión implícita de este tipo, se produce un error en tiempo de compilación.

La expresión constante de cada `case` etiqueta debe indicar un valor que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo aplicable de la `switch` instrucción. Se produce un error en tiempo de compilación si dos `case` o más etiquetas de `switch` la misma instrucción especifican el mismo valor constante.

Puede haber como máximo `default` una etiqueta en una instrucción switch.

Una `switch` instrucción se ejecuta de la siguiente manera:

*  La expresión switch se evalúa y se convierte al tipo aplicable.
*  Si una de las constantes especificadas en una `case` etiqueta en la misma `switch` instrucción es igual al valor de la expresión switch, el control se transfiere a la `case` lista de instrucciones que sigue a la etiqueta coincidente.
*  Si ninguna de las constantes especificadas en `case` las etiquetas de la `switch` misma instrucción es igual al valor de la expresión switch y, si hay `default` una etiqueta, el control se transfiere a la lista de instrucciones que `default` sigue a la rótulo.
*  Si ninguna de las constantes especificadas en `case` las etiquetas de la `switch` misma instrucción es igual al valor de la expresión switch y no hay ninguna `default` etiqueta, el control se transfiere al punto final de la `switch` instrucción.

Si el punto final de la lista de instrucciones de una sección switch es accesible, se producirá un error en tiempo de compilación. Esto se conoce como la regla "no pasar de un paso". El ejemplo
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
es válido porque ninguna sección del modificador tiene un punto final alcanzable. A diferencia de C C++y, no se permite la ejecución de una sección switch a la siguiente sección switch y el ejemplo
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
produce un error en tiempo de compilación. Cuando la ejecución de una sección switch va seguida de la ejecución de otra sección switch, se debe `goto case` usar `goto default` una instrucción or explícita:
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

Se permiten varias etiquetas en un *switch_section*. El ejemplo
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
es válido. En el ejemplo no se infringe la regla "no hay que pasar" porque las etiquetas `case 2:` y `default:` forman parte del mismo *switch_section*.

La regla "no pasar" evita una clase común de errores que se producen en C y C++ cuando `break` las instrucciones se omiten accidentalmente. Además, debido a esta regla, las secciones switch de una `switch` instrucción se pueden reorganizar arbitrariamente sin afectar al comportamiento de la instrucción. Por ejemplo, las secciones de la `switch` instrucción anterior se pueden revertir sin afectar al comportamiento de la instrucción:
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

La lista de instrucciones de una sección switch normalmente finaliza en `break`una `goto case`instrucción, `goto default` o, pero se permite cualquier construcción que representa el punto final de la lista de instrucciones inaccesible. Por ejemplo, se `while` sabe que una instrucción controlada por `true` la expresión booleana nunca alcanza su punto final. Del mismo modo `throw` , `return` una instrucción o siempre transfiere el control en otro lugar y nunca alcanza su punto final. Por lo tanto, el ejemplo siguiente es válido:
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

El tipo de control de `switch` una instrucción puede ser el `string`tipo. Por ejemplo:
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

Al igual que los operadores de igualdad de cadena (operadores de `switch` igualdad de[cadena](expressions.md#string-equality-operators)), la instrucción distingue entre mayúsculas y minúsculas y ejecutará una sección de `case` modificador determinada solo si la cadena de expresión de modificador coincide exactamente con una constante de etiqueta.

Cuando el tipo de control de `switch` una instrucción `string`es, se `null` permite el valor como una constante de etiqueta de caso.

*Statement_list*s de un *switch_block* puede contener instrucciones de declaración ([instrucciones de declaración](statements.md#declaration-statements)). El ámbito de una variable local o constante declarada en un bloque switch es el bloque switch.

La lista de instrucciones de una sección de modificador determinada es accesible `switch` si la instrucción es accesible y se cumple al menos una de las siguientes condiciones:

*  La expresión switch es un valor que no es constante.
*  La expresión switch es un valor constante que coincide con `case` una etiqueta en la sección switch.
*  La expresión switch es un valor constante que no coincide con `case` ninguna etiqueta y la sección switch contiene la `default` etiqueta.
*  Se hace referencia a una etiqueta switch de la sección switch mediante una instrucción `goto case` o `goto default` alcanzable.

El punto final de una `switch` instrucción es accesible si se cumple al menos una de las siguientes condiciones:

*  La `switch` instrucción contiene una `break` instrucción accesible que sale de la `switch` instrucción.
*  La `switch` instrucción es accesible, la expresión switch es un valor no constante y no hay ninguna `default` etiqueta.
*  La `switch` instrucción es accesible, la expresión switch es un valor constante que no coincide con ninguna `case` etiqueta y no hay `default` ninguna etiqueta.

## <a name="iteration-statements"></a>Instrucciones de iteración

Las instrucciones de iteración ejecutan repetidamente una instrucción incrustada.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>La instrucción while

La `while` instrucción ejecuta condicionalmente una instrucción incrustada cero o más veces.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

Una `while` instrucción se ejecuta de la siguiente manera:

*  Se evalúa *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)).
*  Si la expresión booleana produce `true`, el control se transfiere a la instrucción incrustada. Cuando el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de `continue` una instrucción), el control se transfiere al principio de `while` la instrucción.
*  Si la expresión booleana produce `false`, el control se transfiere al punto final de la `while` instrucción.

Dentro de la instrucción incrustada `while` de una instrucción `break` , se puede usar una instrucción ([la instrucción break](statements.md#the-break-statement)) para transferir el control al punto final `while` de la instrucción (por lo tanto, finalizar la iteración de la instrucción incrustada) y un `continue` la instrucción ([la instrucción continue](statements.md#the-continue-statement)) se puede utilizar para transferir el control al punto final de la instrucción incrustada (con lo que se `while` realiza otra iteración de la instrucción).

La instrucción incrustada de `while` una instrucción es accesible si la `while` instrucción es accesible y la expresión booleana no tiene el valor `false`constante.

El punto final de una `while` instrucción es accesible si se cumple al menos una de las siguientes condiciones:

*  La `while` instrucción contiene una `break` instrucción accesible que sale de la `while` instrucción.
*  La `while` instrucción es accesible y la expresión booleana no tiene el valor `true`constante.

### <a name="the-do-statement"></a>La instrucción do

La `do` instrucción ejecuta condicionalmente una instrucción incrustada una o varias veces.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

Una `do` instrucción se ejecuta de la siguiente manera:

*  El control se transfiere a la instrucción insertada.
*  Cuando el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de una instrucción `continue`), se evalúa la *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)). Si la expresión booleana produce `true`, el control se transfiere al principio de la `do` instrucción. De lo contrario, el control se transfiere al punto final `do` de la instrucción.

Dentro de la instrucción incrustada `do` de una instrucción `break` , se puede usar una instrucción ([la instrucción break](statements.md#the-break-statement)) para transferir el control al punto final `do` de la instrucción (por lo tanto, finalizar la iteración de la instrucción incrustada) y un `continue` la instrucción ([la instrucción continue](statements.md#the-continue-statement)) se puede utilizar para transferir el control al punto final de la instrucción insertada.

La instrucción insertada de `do` una instrucción es accesible si la `do` instrucción es accesible.

El punto final de una `do` instrucción es accesible si se cumple al menos una de las siguientes condiciones:

*  La `do` instrucción contiene una `break` instrucción accesible que sale de la `do` instrucción.
*  El punto final de la instrucción incrustada es accesible y la expresión booleana no tiene el valor `true`constante.

### <a name="the-for-statement"></a>La instrucción for

La `for` instrucción evalúa una secuencia de expresiones de inicialización y, a continuación, mientras una condición es true, ejecuta repetidamente una instrucción incrustada y evalúa una secuencia de expresiones de iteración.

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

El *for_initializer*, si está presente, consta de un *local_variable_declaration* ([declaraciones de variables locales](statements.md#local-variable-declarations)) o una lista de *statement_expression*s ([instrucciones de expresión](statements.md#expression-statements)) separadas por comas. El ámbito de una variable local declarada por un *for_initializer* comienza en el *local_variable_declarator* para la variable y se extiende hasta el final de la instrucción incrustada. El ámbito incluye *for_condition* y *for_iterator*.

El *for_condition*, si está presente, debe ser una *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)).

*For_iterator*, si está presente, consta de una lista de *statement_expression*s ([instrucciones de expresión](statements.md#expression-statements)) separadas por comas.

Una instrucción for se ejecuta de la siguiente manera:

*  Si hay un *for_initializer* , los inicializadores variables o las expresiones de instrucción se ejecutan en el orden en que se escriben. Este paso solo se realiza una vez.
*  Si un *for_condition* está presente, se evalúa.
*  Si *for_condition* no está presente o si la evaluación produce `true`, el control se transfiere a la instrucción insertada. Cuando el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de una instrucción `continue`), las expresiones de *for_iterator*, si hay alguna, se evalúan en secuencia y, a continuación, se realiza otra iteración, empezando por evaluación de *for_condition* en el paso anterior.
*  Si el *for_condition* está presente y la evaluación produce `false`, el control se transfiere al punto final de la instrucción `for`.

Dentro de la instrucción incrustada de una instrucción `for`, se puede usar una instrucción `break` ([instrucción break](statements.md#the-break-statement)) para transferir el control al punto final de la instrucción `for` (por lo tanto, finalizar la iteración de la instrucción incrustada) y una instrucción `continue` ([ La instrucción continue](statements.md#the-continue-statement)) se puede usar para transferir el control al punto final de la instrucción incrustada (con lo que se ejecuta *for_iterator* y se realiza otra iteración de la instrucción `for`, comenzando por *for_condition*).

La instrucción insertada de `for` una instrucción es accesible si se cumple una de las siguientes condiciones:

*  La instrucción `for` es accesible y no hay *for_condition* .
*  La instrucción `for` es accesible y está presente un *for_condition* y no tiene el valor constante `false`.

El punto final de una `for` instrucción es accesible si se cumple al menos una de las siguientes condiciones:

*  La `for` instrucción contiene una `break` instrucción accesible que sale de la `for` instrucción.
*  La instrucción `for` es accesible y está presente un *for_condition* y no tiene el valor constante `true`.

### <a name="the-foreach-statement"></a>La instrucción foreach

La `foreach` instrucción enumera los elementos de una colección y ejecuta una instrucción incrustada para cada elemento de la colección.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

El *tipo* y el *identificador* de una `foreach` instrucción declaran la ***variable de iteración*** de la instrucción. Si se proporciona el identificador `var` como *local_variable_type*y no hay ningún tipo denominado `var` en el ámbito, se dice que la variable de iteración es una ***variable de iteración con tipo implícito***y se considera que su tipo es el tipo de elemento de `foreach`. tal y como se especifica a continuación. La variable de iteración corresponde a una variable local de solo lectura con un ámbito que se extiende a través de la instrucción incrustada. Durante la ejecución de `foreach` una instrucción, la variable de iteración representa el elemento de colección para el que se está realizando actualmente una iteración. Se produce un error en tiempo de compilación si la instrucción incrustada intenta modificar la variable de iteración ( `++` a `--` través de la asignación o los operadores y) `ref` o `out` pasar la variable de iteración como un parámetro o.

En el siguiente, por motivos de `IEnumerable`brevedad `IEnumerator`, `IEnumerable<T>` , `IEnumerator<T>` y hacen referencia a los tipos correspondientes de los `System.Collections` espacios `System.Collections.Generic`de nombres y.

El procesamiento en tiempo de compilación de una instrucción foreach primero determina el ***tipo de colección***, el tipo de ***enumerador*** y el ***tipo de elemento*** de la expresión. Esta determinación se realiza de la siguiente manera:

*  Si el tipo `X` de *expresión* es un tipo de matriz, hay una conversión de referencia implícita `X` de a `IEnumerable` la interfaz ( `System.Array` puesto que implementa esta interfaz). El ***tipo de colección*** es `IEnumerable` la interfaz, el ***tipo de enumerador*** es `IEnumerator` la interfaz ***y el tipo de elemento es*** el tipo de `X`elemento del tipo de matriz.
*  Si el tipo `X` de *expresión* es `dynamic` , hay una conversión implícita de la *expresión* a la `IEnumerable` interfaz ([conversiones dinámicas implícitas](conversions.md#implicit-dynamic-conversions)). El ***tipo de colección*** es `IEnumerable` la interfaz y el ***tipo de enumerador*** es la `IEnumerator` interfaz. Si se proporciona el identificador `var` como *local_variable_type* , el tipo de ***elemento*** es `dynamic`, de lo contrario es `object`.
*  De lo contrario, determine `X` si el tipo `GetEnumerator` tiene un método adecuado:
   * Realiza la búsqueda de miembros en `X` el tipo con `GetEnumerator` el identificador y ningún argumento de tipo. Si la búsqueda de miembros no produce una coincidencia, o produce una ambigüedad, o produce una coincidencia que no es un grupo de métodos, busque una interfaz enumerable como se describe a continuación. Se recomienda emitir una advertencia si la búsqueda de miembros produce cualquier cosa, excepto un grupo de métodos o ninguna coincidencia.
   * Realizar la resolución de sobrecarga utilizando el grupo de métodos resultante y una lista de argumentos vacía. Si la resolución de sobrecarga da como resultado ningún método aplicable, da como resultado una ambigüedad o tiene como resultado un único método mejor pero ese método es estático o no público, busque una interfaz enumerable como se describe a continuación. Se recomienda emitir una advertencia si la resolución de sobrecarga produce todo excepto un método de instancia pública inequívoco o ningún método aplicable.
   * Si el tipo `E` `GetEnumerator` de valor devuelto del método no es una clase, una estructura o un tipo de interfaz, se genera un error y no se realiza ningún otro paso.
   * La búsqueda de miembros se `E` realiza en con el `Current` identificador y sin argumentos de tipo. Si la búsqueda de miembros no produce ninguna coincidencia, el resultado es un error o el resultado es cualquier cosa excepto una propiedad de instancia pública que permita la lectura, se genera un error y no se realiza ningún paso más.
   * La búsqueda de miembros se `E` realiza en con el `MoveNext` identificador y sin argumentos de tipo. Si la búsqueda de miembros no produce ninguna coincidencia, el resultado es un error o el resultado es cualquier cosa excepto un grupo de métodos, se genera un error y no se realiza ningún paso más.
   * La resolución de sobrecarga se realiza en el grupo de métodos con una lista de argumentos vacía. Si la resolución de sobrecarga da como resultado ningún método aplicable, da como resultado una ambigüedad o tiene como resultado un único método mejor pero ese método es estático o no público, o su tipo de `bool`valor devuelto no es, se genera un error y no se realiza ningún otro paso.
   * El ***tipo*** de colección `X`es, el tipo de `E` ***enumerador*** es y el tipo de ***elemento*** es el `Current` tipo de la propiedad.

*  De lo contrario, compruebe si hay una interfaz enumerable:
   * Si entre todos los tipos `Ti` para los que hay una conversión implícita de `X` a `IEnumerable<Ti>`, hay un tipo `T` único, de modo `T` que no `dynamic` es y para todos los `Ti` demás hay un conversión implícita de `IEnumerable<T>` a `IEnumerable<Ti>`, el ***tipo de colección*** es la interfaz `IEnumerable<T>`, el ***tipo de enumerador*** es `IEnumerator<T>`la interfaz y el ***tipo de elemento*** es `T`.
   * De lo contrario, si hay más de un tipo `T`de este tipo, se genera un error y no se realiza ningún paso más.
   * De lo contrario, si hay una conversión implícita `X` de en `System.Collections.IEnumerable` la interfaz, el ***tipo de colección*** es esta interfaz, el ***tipo de enumerador*** es la interfaz `System.Collections.IEnumerator`y el ***tipo de elemento*** es `object`.
   * De lo contrario, se genera un error y no se realiza ningún paso más.

Los pasos anteriores, si son correctos, producen de forma inequívoca `C`un tipo de `E` colección, un `T`tipo de enumerador y un tipo de elemento. Una instrucción foreach con el formato
```csharp
foreach (V v in x) embedded_statement
```
se expande a continuación hasta:
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

La variable `e` no es visible o accesible para la expresión `x` o la instrucción incrustada ni cualquier otro código fuente del programa. La variable `v` es de solo lectura en la instrucción insertada. Si no hay una conversión explícita ([conversiones explícitas](conversions.md#explicit-conversions)) de `T` (el tipo de elemento) a `V` ( *local_variable_type* en la instrucción foreach), se genera un error y no se realiza ningún otro paso. Si `x` tiene el valor `null`, se `System.NullReferenceException` produce una excepción en tiempo de ejecución.

Una implementación puede implementar una instrucción foreach determinada de forma diferente, por ejemplo, por motivos de rendimiento, siempre que el comportamiento sea coherente con la expansión anterior.

La colocación de `v` dentro del bucle while es importante para que lo Capture cualquier función anónima que se produzca en el *embedded_statement*.

Por ejemplo:
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
Si `v` se declaró fuera del bucle while, se compartirá entre todas las iteraciones y su valor después del bucle for sería el valor final, `13`, que es lo que la invocación de `f` imprimiría. En su lugar, dado que cada iteración tiene `v`su propia variable, la `f` que captura en la primera iteración seguirá conservando `7`el valor, que es lo que se va a imprimir. (Nota: versiones anteriores de C# declaradas `v` fuera del bucle while).

El cuerpo del bloque finally se construye según los pasos siguientes:

*  Si hay una conversión implícita de `E` en la `System.IDisposable` interfaz,
   *  Si `E` es un tipo de valor que no acepta valores NULL, la cláusula finally se expande al equivalente semántico de:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  En caso contrario, la cláusula finally se expande al equivalente semántico de:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   salvo que si `E` es un tipo de valor o un parámetro de tipo al que se ha creado una instancia de un tipo `e` de `System.IDisposable` valor, la conversión de a no hará que se produzca la conversión boxing.

*  De lo contrario `E` , si es un tipo sellado, la cláusula finally se expande a un bloque vacío:

   ```csharp
   finally {
   }
   ```

*  De lo contrario, la cláusula finally se expande a:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   La variable `d` local no es visible o accesible para cualquier código de usuario. En concreto, no entra en conflicto con ninguna otra variable cuyo ámbito incluya el bloque Finally.

El orden en el `foreach` que recorre los elementos de una matriz es el siguiente: En el caso de las matrices unidimensionales, los elementos se recorren en orden `0` creciente de índice, `Length - 1`empezando por el índice y terminando por el índice. En el caso de las matrices multidimensionales, los elementos se recorren de forma que los índices de la dimensión situada más a la derecha aumenten primero, la dimensión izquierda siguiente y así sucesivamente hacia la izquierda.

En el ejemplo siguiente se imprime cada valor en una matriz bidimensional, en el orden de los elementos:
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
La salida generada es la siguiente:
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

En el ejemplo
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
el tipo de `n` se deduce `int`como, el tipo de elemento de `numbers`.

## <a name="jump-statements"></a>Instrucciones de salto

Las instrucciones de salto transfieren el control incondicionalmente.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

La ubicación a la que una instrucción de salto transfiere el control se denomina ***destino*** de la instrucción de salto.

Cuando una instrucción de salto se produce dentro de un bloque y el destino de la instrucción de salto está fuera del bloque, se dice que la instrucción de salto ***sale*** del bloque. Aunque una instrucción de salto puede transferir el control fuera de un bloque, nunca puede transferir el control a un bloque.

La ejecución de instrucciones de salto es complicada por la presencia de `try` instrucciones intermedias. En ausencia de estas `try` instrucciones, una instrucción de salto transfiere incondicionalmente el control de la instrucción de salto a su destino. En presencia de dichas `try` instrucciones intermedias, la ejecución es más compleja. Si la instrucción de salto sale de uno o `try` más bloques con `finally` bloques asociados, el `finally` control se transfiere inicialmente al bloque de la `try` instrucción más interna. Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión. Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones que intervienen.

En el ejemplo
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
los `finally` bloques asociados a dos `try` instrucciones se ejecutan antes de que el control se transfiera al destino de la instrucción de salto.

La salida generada es la siguiente:
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Instrucción break

La `break` instrucción sale de la instrucción de inclusión `switch`, `while` `do` `for`,, o `foreach` más cercana.

```antlr
break_statement
    : 'break' ';'
    ;
```

El destino de una `break` instrucción es el punto final de la instrucción envolvente `switch`, `while`, `do` `for`, o `foreach` más cercana. Si una `break` instrucción no está delimitada por `switch`una `while`instrucción `do`, `for`,, `foreach` o, se produce un error en tiempo de compilación.

Cuando varias `switch`instrucciones `while` `break` , `do` ,,`foreach` o se anidan entre sí, una instrucción solo se aplica a la instrucción más interna. `for` Para transferir el control entre varios niveles de anidamiento, `goto` se debe usar una instrucción ([instrucción Goto](statements.md#the-goto-statement)).

Una `break` instrucción no puede salir `finally` de un bloque ([la instrucción try](statements.md#the-try-statement)). Cuando una `break` instrucción aparece dentro de `finally` un bloque, el destino de `break` la instrucción debe estar dentro del `finally` mismo bloque; de lo contrario, se produce un error en tiempo de compilación.

Una `break` instrucción se ejecuta de la siguiente manera:

*  Si la `break` instrucción sale de uno o más `try` bloques con `finally` bloques `finally` asociados, el control se transfiere inicialmente al bloque de la instrucción `try` más interna. Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión. Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones que intervienen.
*  El control se transfiere al destino de la `break` instrucción.

Dado que `break` una instrucción transfiere el control incondicionalmente a otra parte, el `break` punto final de una instrucción nunca es accesible.

### <a name="the-continue-statement"></a>La instrucción continue

La `continue` instrucción inicia una nueva iteración de la instrucción envolvente `while`, `do`, `for`o `foreach` más cercana.

```antlr
continue_statement
    : 'continue' ';'
    ;
```

El destino de una `continue` instrucción es el punto final de la instrucción incrustada de la instrucción envolvente `while`, `do`, `for`o `foreach` más cercana. Si una `continue` instrucción no está delimitada por `while`una `do`instrucción `for`,, `foreach` o, se produce un error en tiempo de compilación.

Cuando varias `while`instrucciones `do`, `for`, `continue` o `foreach` se anidan entre sí, una instrucción solo se aplica a la instrucción más interna. Para transferir el control entre varios niveles de anidamiento, `goto` se debe usar una instrucción ([instrucción Goto](statements.md#the-goto-statement)).

Una `continue` instrucción no puede salir `finally` de un bloque ([la instrucción try](statements.md#the-try-statement)). Cuando una `continue` instrucción aparece dentro de `finally` un bloque, el destino de `continue` la instrucción debe estar dentro del `finally` mismo bloque; de lo contrario, se produce un error en tiempo de compilación.

Una `continue` instrucción se ejecuta de la siguiente manera:

*  Si la `continue` instrucción sale de uno o más `try` bloques con `finally` bloques `finally` asociados, el control se transfiere inicialmente al bloque de la instrucción `try` más interna. Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión. Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones que intervienen.
*  El control se transfiere al destino de la `continue` instrucción.

Dado que `continue` una instrucción transfiere el control incondicionalmente a otra parte, el `continue` punto final de una instrucción nunca es accesible.

### <a name="the-goto-statement"></a>La instrucción goto

La `goto` instrucción transfiere el control a una instrucción marcada por una etiqueta.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

El destino de una `goto` instrucción *Identifier* es la instrucción con etiqueta con la etiqueta especificada. Si no existe una etiqueta con el nombre especificado en el miembro de función actual, o si la `goto` instrucción no está dentro del ámbito de la etiqueta, se produce un error en tiempo de compilación. Esta regla permite el uso de una `goto` instrucción para transferir el control fuera de un ámbito anidado, pero no a un ámbito anidado. En el ejemplo
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
una `goto` instrucción se usa para transferir el control fuera de un ámbito anidado.

El destino de una `goto case` instrucción es la lista de instrucciones de la instrucción de `switch` inclusión inmediata ([la instrucción switch](statements.md#the-switch-statement)), que contiene `case` una etiqueta con el valor constante especificado. Si la instrucción `goto case` no está delimitada por una instrucción `switch`, si *constant_expression* no se pueden convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) al tipo de control de la instrucción envolvente de `switch` más cercana, o si el contenedor más cercano la instrucción `switch` no contiene una etiqueta `case` con el valor constante especificado, se produce un error en tiempo de compilación.

El destino de una `goto default` instrucción es la lista de instrucciones de la instrucción de `switch` inclusión inmediata ([la instrucción switch](statements.md#the-switch-statement)), que contiene `default` una etiqueta. Si la `goto default` instrucción no está delimitada por `switch` una instrucción, o si la instrucción de `switch` inclusión más cercana no contiene `default` una etiqueta, se produce un error en tiempo de compilación.

Una `goto` instrucción no puede salir `finally` de un bloque ([la instrucción try](statements.md#the-try-statement)). Cuando una `goto` instrucción aparece dentro de `finally` un bloque, el destino de `goto` la instrucción debe estar dentro del `finally` mismo bloque o, de lo contrario, se producirá un error en tiempo de compilación.

Una `goto` instrucción se ejecuta de la siguiente manera:

*  Si la `goto` instrucción sale de uno o más `try` bloques con `finally` bloques `finally` asociados, el control se transfiere inicialmente al bloque de la instrucción `try` más interna. Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión. Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones que intervienen.
*  El control se transfiere al destino de la `goto` instrucción.

Dado que `goto` una instrucción transfiere el control incondicionalmente a otra parte, el `goto` punto final de una instrucción nunca es accesible.

### <a name="the-return-statement"></a>La instrucción return

La `return` instrucción devuelve el control al llamador actual de la función en la que `return` aparece la instrucción.

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

Una `return` instrucción sin expresión solo se puede usar en un miembro de función que no calcule un valor, es decir, un método con el tipo de resultado ([cuerpo del método](classes.md#method-body)) `void`, el `set` descriptor de acceso de una propiedad o `add` un indizador, el los `remove` descriptores de acceso de un evento, un constructor de instancia, un constructor estático o un destructor.

Una `return` instrucción con una expresión solo se puede usar en un miembro de función que calcula un valor, es decir, un método con un tipo de resultado no void, el `get` descriptor de acceso de una propiedad o un indizador, o un operador definido por el usuario. Debe existir una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo de la expresión al tipo de valor devuelto del miembro de función contenedora.

Las instrucciones Return también se pueden utilizar en el cuerpo de expresiones de función anónimas ([expresiones de función anónimas](expressions.md#anonymous-function-expressions)) y participar en la determinación de las conversiones que existen para esas funciones.

Se trata de un error en tiempo de compilación `return` para que una instrucción aparezca `finally` en un bloque ([la instrucción try](statements.md#the-try-statement)).

Una `return` instrucción se ejecuta de la siguiente manera:

*  Si la `return` instrucción especifica una expresión, se evalúa la expresión y el valor resultante se convierte al tipo de valor devuelto de la función que la contiene mediante una conversión implícita. El resultado de la conversión se convierte en el valor de resultado producido por la función.
*  Si la `return` instrucción está incluida en uno o varios `try` bloques `catch` o con bloques `finally` asociados, el `finally` control se transfiere inicialmente al bloque de la instrucción `try` más interna. Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión. Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones envolventes.
*  Si la función contenedora no es una función asincrónica, el control se devuelve al autor de la llamada de la función contenedora junto con el valor del resultado, si existe.
*  Si la función contenedora es una función asincrónica, el control se devuelve al llamador actual y el valor del resultado, si existe, se registra en la tarea devuelta tal y como se describe en ([interfaces del enumerador](classes.md#enumerator-interfaces)).

Dado que `return` una instrucción transfiere el control incondicionalmente a otra parte, el `return` punto final de una instrucción nunca es accesible.

### <a name="the-throw-statement"></a>La instrucción throw

La `throw` instrucción produce una excepción.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

Una `throw` instrucción con una expresión inicia el valor generado al evaluar la expresión. La expresión debe indicar un valor del tipo `System.Exception`de clase, de un tipo de clase que se deriva de o de un tipo de parámetro de tipo que tiene `System.Exception` (o una subclase de `System.Exception` ella) como su clase base efectiva. Si la evaluación de la expresión `null`produce, `System.NullReferenceException` se produce una excepción en su lugar.

Una `throw` instrucción sin expresión solo se puede usar en un `catch` bloque, en cuyo caso la instrucción vuelve a iniciar la excepción que está controlando actualmente ese `catch` bloque.

Dado que `throw` una instrucción transfiere el control incondicionalmente a otra parte, el `throw` punto final de una instrucción nunca es accesible.

Cuando se produce una excepción, el control se transfiere a la `catch` primera cláusula de una instrucción `try` de inclusión que puede controlar la excepción. El proceso que tiene lugar desde el punto de la excepción que se está iniciando hasta el punto de transferir el control a un controlador de excepciones adecuado se conoce como ***propagación de excepciones***. La propagación de una excepción consiste en evaluar repetidamente los siguientes pasos hasta `catch` que se encuentre una cláusula que coincida con la excepción. En esta descripción, el ***punto de inicio*** es inicialmente la ubicación en la que se produce la excepción.

*  En el miembro de función actual, `try` se examina cada instrucción que incluye el punto de inicio. Para cada instrucción `S`, empezando por la instrucción `try` más interna y finalizando con `try` la instrucción externa, se evalúan los pasos siguientes:

   * Si el `try` bloque de `S` incluye el punto de inicio y si S tiene una o más `catch` cláusulas, las `catch` cláusulas se examinan en orden de aparición para encontrar un controlador adecuado para la excepción, de acuerdo con las reglas especificadas en. Sección [de la instrucción try](statements.md#the-try-statement). Si se encuentra `catch` una cláusula coincidente, la propagación de excepciones se completa mediante la transferencia del control al bloque de esa `catch` cláusula.

   * De lo contrario, `try` si el bloque `catch` o un `S` bloque de incluye el punto de inicio y `S` si tiene `finally` un bloque, el `finally` control se transfiere al bloque. Si el `finally` bloque produce otra excepción, finaliza el procesamiento de la excepción actual. De lo contrario, cuando el control alcanza el punto `finally` final del bloque, continúa el procesamiento de la excepción actual.

*  Si no se encuentra un controlador de excepciones en la invocación de función actual, se termina la invocación de la función y se produce uno de los siguientes casos:

   * Si la función actual no es asincrónica, los pasos anteriores se repiten para el llamador de la función con un punto de inicio correspondiente a la instrucción de la que se invocó el miembro de función.

   * Si la función actual es asincrónica y devuelve la tarea, la excepción se registra en la tarea devuelta, que se coloca en un estado de error o cancelado, tal y como se describe en [interfaces de enumerador](classes.md#enumerator-interfaces).

   * Si la función actual es asincrónica y devuelve void, el contexto de sincronización del subproceso actual se notifica como se describe en [interfaces enumerables](classes.md#enumerable-interfaces).

*  Si el procesamiento de excepciones finaliza todas las invocaciones de miembros de función en el subproceso actual, lo que indica que el subproceso no tiene ningún controlador para la excepción, el subproceso se termina. El impacto de dicha terminación está definido por la implementación.

## <a name="the-try-statement"></a>Instrucción try

La `try` instrucción proporciona un mecanismo para detectar las excepciones que se producen durante la ejecución de un bloque. Además, la `try` instrucción proporciona la capacidad de especificar un bloque de código que siempre se ejecuta cuando el control sale `try` de la instrucción.

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

Hay tres formas posibles de `try` instrucciones:

*  Un `try` bloque seguido de uno o varios `catch` bloques.
*  Un `try` bloque seguido de un `finally` bloque.
*  Un `try` bloque seguido de uno o varios `catch` bloques seguidos de `finally` un bloque.

Cuando una cláusula `catch` especifica un *exception_specifier*, el tipo debe ser `System.Exception`, un tipo que se deriva de `System.Exception` o un tipo de parámetro de tipo que tiene `System.Exception` (o una subclase de ella) como su clase base efectiva.

Cuando una cláusula `catch` especifica un *exception_specifier* con un *identificador*, se declara una ***variable de excepción*** con el nombre y el tipo especificados. La variable de excepción corresponde a una variable local con un ámbito que se extiende `catch` por encima de la cláusula. Durante la ejecución del *exception_filter* y el *bloque*, la variable de excepción representa la excepción que se está controlando actualmente. A efectos de la comprobación de asignación definitiva, la variable de excepción se considera asignada definitivamente en todo el ámbito.

A menos `catch` que una cláusula incluya un nombre de variable de excepción, no es posible tener acceso al objeto de `catch` excepción en el filtro y bloque.

Una cláusula `catch` que no especifica un *exception_specifier* se denomina cláusula general de @no__t 2.

Algunos lenguajes de programación pueden admitir excepciones que no se pueden representar como un objeto derivado `System.Exception`de, aunque estas excepciones nunca se pueden generar C# mediante código. Se puede `catch` usar una cláusula general para detectar tales excepciones. Por lo tanto, `catch` una cláusula general es semánticamente diferente de una que especifica `System.Exception`el tipo, en que la primera también puede detectar excepciones de otros lenguajes.

Para buscar un controlador de una excepción, `catch` las cláusulas se examinan en orden léxico. Si una `catch` cláusula especifica un tipo pero no un filtro de excepción, se trata de un error en tiempo de `catch` compilación para una cláusula `try` posterior en la misma instrucción para especificar un tipo que sea igual o derivado de ese tipo. Si una `catch` cláusula no especifica ningún tipo y no hay ningún filtro, debe ser `catch` la última cláusula `try` para esa instrucción.

Dentro de `catch` un bloque, `throw` se puede usar una instrucción ([instrucción throw](statements.md#the-throw-statement)) sin expresión para volver a producir la excepción detectada por el `catch` bloque. Las asignaciones a una variable de excepción no modifican la excepción que se vuelve a iniciar.

En el ejemplo
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
el método `F` detecta una excepción, escribe información de diagnóstico en la consola, modifica la variable de excepción y vuelve a producir la excepción. La excepción que se vuelve a producir es la excepción original, por lo que la salida generada es:
```console
Exception in F: G
Exception in Main: G
```

Si se produjo `e` el primer bloque catch en lugar de volver a producir la excepción actual, la salida generada sería la siguiente:
```console
Exception in F: G
Exception in Main: F
```

Es un error en tiempo de compilación para una `break`instrucción `continue`, o `goto` para transferir el control fuera de un `finally` bloque. Cuando una `break`instrucción `continue`, o `goto` aparece en un `finally` bloque, el destino de la instrucción debe estar dentro del mismo `finally` bloque o, de lo contrario, se produce un error en tiempo de compilación.

Se trata de un error en tiempo de compilación `return` para que una instrucción se `finally` produzca en un bloque.

Una `try` instrucción se ejecuta de la siguiente manera:

*  El `try` control se transfiere al bloque.
*  When y si el control alcanza el punto final del `try` bloque:
   *  Si la `try` instrucción tiene un `finally` bloque, se `finally` ejecuta el bloque.
   *  El control se transfiere al punto final de la `try` instrucción.

*  Si una excepción se propaga a la `try` instrucción durante la `try` ejecución del bloque:
   *  Las `catch` cláusulas, si las hay, se examinan en orden de aparición para encontrar un controlador adecuado para la excepción. Si una `catch` cláusula no especifica un tipo, o especifica el tipo de excepción o un tipo base del tipo de excepción:
      *  Si la `catch` cláusula declara una variable de excepción, el objeto de excepción se asigna a la variable de excepción.
      *  Si la `catch` cláusula declara un filtro de excepción, se evalúa el filtro. Si se evalúa como `false`, la cláusula catch no es una coincidencia y la búsqueda continúa a través de las cláusulas posteriores `catch` de un controlador adecuado.
      *  De lo contrario `catch` , la cláusula se considera una coincidencia y el control se transfiere al `catch` bloque coincidente.
      *  When y si el control alcanza el punto final del `catch` bloque:
         * Si la `try` instrucción tiene un `finally` bloque, se `finally` ejecuta el bloque.
         * El control se transfiere al punto final de la `try` instrucción.
      *  Si una excepción se propaga a la `try` instrucción durante la `catch` ejecución del bloque:
         *  Si la `try` instrucción tiene un `finally` bloque, se `finally` ejecuta el bloque.
         *  La excepción se propaga a la siguiente instrucción de inclusión `try` .
   *  Si la `try` instrucción no `catch` tiene cláusulas o si ninguna `catch` cláusula coincide con la excepción:
      *  Si la `try` instrucción tiene un `finally` bloque, se `finally` ejecuta el bloque.
      *  La excepción se propaga a la siguiente instrucción de inclusión `try` .

Las instrucciones de un `finally` bloque siempre se ejecutan cuando el control `try` sale de una instrucción. Esto es cierto si la transferencia de control se produce como resultado de la ejecución normal, como resultado de la ejecución `break`de `continue`una `goto`instrucción, `return` , o, o como resultado de propagar una excepción fuera de la `try` privacidad.

Si se produce una excepción durante la ejecución de `finally` un bloque y no se detecta en el mismo bloque Finally, la excepción se propaga a la siguiente instrucción envolvente `try` . Si hay otra excepción en el proceso de propagación, se perderá esa excepción. El proceso de propagación de una excepción se describe con más detalle en la descripción `throw` de la instrucción ([la instrucción throw](statements.md#the-throw-statement)).

El `try` bloque de una `try` instrucción es accesible si la `try` instrucción es accesible.

Se `catch` puede tener acceso `try` a un bloque de una instrucción `try` si la instrucción es accesible.

El `finally` bloque de una `try` instrucción es accesible si la `try` instrucción es accesible.

El punto final de una `try` instrucción es accesible si se cumplen las dos condiciones siguientes:

*  El punto final del `try` bloque es accesible o se puede tener acceso al punto final de al menos un `catch` bloque.
*  Si hay `finally` un bloque, se puede tener acceso al punto `finally` final del bloque.

## <a name="the-checked-and-unchecked-statements"></a>Las instrucciones checked y unchecked

Las `checked` instrucciones `unchecked` y se utilizan para controlar el ***contexto de comprobación de desbordamiento*** para conversiones y operaciones aritméticas de tipo entero.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

La `checked` instrucción hace que todas las expresiones del *bloque* se evalúen en un contexto comprobado, `unchecked` y la instrucción hace que todas las expresiones del *bloque* se evalúen en un contexto no comprobado.

Las `checked` instrucciones `unchecked` y son exactamente equivalentes a `checked` los `unchecked` operadores y ([los operadores Checked y unchecked](expressions.md#the-checked-and-unchecked-operators)), salvo que operan en bloques en lugar de en expresiones.

## <a name="the-lock-statement"></a>La instrucción lock

La `lock` instrucción obtiene el bloqueo de exclusión mutua para un objeto determinado, ejecuta una instrucción y, a continuación, libera el bloqueo.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

La expresión de una instrucción `lock` debe indicar un valor de un tipo conocido como *reference_type*. No se realiza ninguna conversión boxing implícita ([conversiones boxing](conversions.md#boxing-conversions)) en la expresión de una instrucción `lock` y, por lo tanto, es un error en tiempo de compilación para que la expresión denote un valor de *value_type*.

Una `lock` instrucción con el formato
```csharp
lock (x) ...
```
donde `x` es una expresión de *reference_type*, es exactamente equivalente a
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
salvo que `x` solo se evalúa una vez.

Mientras se mantiene un bloqueo de exclusión mutua, el código que se ejecuta en el mismo subproceso de ejecución también puede obtener y liberar el bloqueo. Sin embargo, el código que se ejecuta en otros subprocesos no obtiene el bloqueo hasta que se libera el bloqueo.

No `System.Type` se recomienda el bloqueo de objetos para sincronizar el acceso a los datos estáticos. Otro código podría bloquearse en el mismo tipo, lo que puede provocar un interbloqueo. Un mejor enfoque es sincronizar el acceso a los datos estáticos bloqueando un objeto estático privado. Por ejemplo:
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

## <a name="the-using-statement"></a>La instrucción using

La `using` instrucción obtiene uno o más recursos, ejecuta una instrucción y, a continuación, desecha el recurso.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

Un ***recurso*** es una clase o estructura que implementa `System.IDisposable`, que incluye un único método sin parámetros denominado. `Dispose` El código que usa un recurso puede llamar `Dispose` a para indicar que el recurso ya no es necesario. Si `Dispose` no se llama a, la eliminación automática se produce finalmente como consecuencia de la recolección de elementos no utilizados.

Si el formato de *resource_acquisition* es *local_variable_declaration* , el tipo de *local_variable_declaration* debe ser `dynamic` o un tipo que se pueda convertir implícitamente a `System.IDisposable`. Si el formato de *resource_acquisition* es *Expression* , esta expresión debe poder convertirse implícitamente a `System.IDisposable`.

Las variables locales declaradas en un *resource_acquisition* son de solo lectura y deben incluir un inicializador. Se produce un error en tiempo de compilación si la instrucción incrustada intenta modificar estas variables locales (a través `++` de `--` la asignación o los operadores y), tomar la dirección de ellas `ref` o `out` pasarlas como parámetros o.

Una `using` instrucción se traduce en tres partes: adquisición, uso y eliminación. El uso del recurso se adjunta implícitamente en una `try` instrucción que incluye una `finally` cláusula. Esta `finally` cláusula desecha el recurso. Si se `null` adquiere un recurso, no se realiza ninguna `Dispose` llamada a y no se produce ninguna excepción. Si el recurso es de tipo `dynamic` , se convierte dinámicamente a través de una conversión dinámica implícita ([conversiones dinámicas implícitas](conversions.md#implicit-dynamic-conversions)) en `IDisposable` durante la adquisición para asegurarse de que la conversión se realiza correctamente antes del uso y elimina.

Una `using` instrucción con el formato
```csharp
using (ResourceType resource = expression) statement
```
corresponde a una de las tres expansiones posibles. Cuando `ResourceType` es un tipo de valor que no acepta valores NULL, la expansión es
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

De lo contrario `ResourceType` , cuando es un tipo de valor que acepta valores NULL o `dynamic`un tipo de referencia distinto de, la expansión es
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

De lo contrario `ResourceType` , `dynamic`cuando es, la expansión es
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

En cualquier expansión, la `resource` variable es de solo lectura en la instrucción incrustada, y `d` no se puede obtener acceso a la variable en la instrucción incrustada y no es visible para ella.

Una implementación puede implementar una instrucción using determinada de forma diferente, por ejemplo, por motivos de rendimiento, siempre que el comportamiento sea coherente con la expansión anterior.

Una `using` instrucción con el formato
```csharp
using (expression) statement
```
tiene las mismas tres expansiones posibles. En este caso `ResourceType` `expression`, es implícitamente el tipo en tiempo de compilación de, si tiene uno. En caso contrario `IDisposable` , la propia interfaz se `ResourceType`utiliza como. No `resource` se puede obtener acceso a la variable en y no es visible para la instrucción incrustada.

Cuando un *resource_acquisition* toma la forma de un *local_variable_declaration*, es posible adquirir varios recursos de un tipo determinado. Una `using` instrucción con el formato
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
es exactamente equivalente a una secuencia de `using` instrucciones anidadas:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

En el ejemplo siguiente se crea un `log.txt` archivo denominado y se escriben dos líneas de texto en el archivo. A continuación, el ejemplo abre el mismo archivo para leer y copia las líneas de texto contenidas en la consola.
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

Dado que `TextWriter` las `TextReader` clases y implementan la `IDisposable` interfaz, el ejemplo `using` puede utilizar instrucciones para asegurarse de que el archivo subyacente está cerrado correctamente después de las operaciones de escritura o lectura.

## <a name="the-yield-statement"></a>La instrucción yield

La `yield` instrucción se usa en un bloque de iteradores ([bloques](statements.md#blocks)) para obtener un valor para el objeto enumerador ([objetos enumerador](classes.md#enumerator-objects)) o el objeto enumerable ([objetos enumerables](classes.md#enumerable-objects)) de un iterador o para señalar el final de la iteración.

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield`no es una palabra reservada; tiene un significado especial solo cuando se usa inmediatamente antes `return` de `break` una palabra clave o. En otros contextos, `yield` se puede usar como identificador.

Hay varias restricciones sobre dónde puede aparecer `yield` una instrucción, tal y como se describe a continuación.

*  Es un error en tiempo de compilación que una instrucción `yield` (de cualquier forma) aparezca fuera de *method_body*, *operator_body* o *accessor_body*
*  Es un error en tiempo de compilación para una `yield` instrucción (de cualquier forma) que aparezca dentro de una función anónima.
*  Es un error en tiempo de compilación para una `yield` instrucción (de cualquier forma) que aparezca en la `finally` cláusula de una `try` instrucción.
*  Se trata de un error en tiempo de compilación `yield return` para que una instrucción aparezca en `try` cualquier parte de una `catch` instrucción que contenga cláusulas.

En el ejemplo siguiente se muestran algunos usos válidos `yield` y no válidos de las instrucciones.

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

Debe existir una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo de la expresión en la `yield return` instrucción al tipo yield ([yield Type](classes.md#yield-type)) del iterador.

Una `yield return` instrucción se ejecuta de la siguiente manera:

*  La expresión dada en la instrucción se evalúa, se convierte implícitamente al tipo Yield y se asigna a la `Current` propiedad del objeto enumerador.
*  La ejecución del bloque de iterador está suspendida. Si la `yield return` instrucción está dentro de uno o `try` más bloques, los `finally` bloques asociados no se ejecutan en este momento.
*  El `MoveNext` método del objeto de enumerador `true` vuelve a su llamador, lo que indica que el objeto de enumerador avanzó correctamente al siguiente elemento.

La siguiente llamada al método del `MoveNext` objeto de enumerador reanuda la ejecución del bloque de iterador desde donde se suspendió por última vez.

Una `yield break` instrucción se ejecuta de la siguiente manera:

*  Si la `yield break` instrucción está delimitada por uno o `try` más bloques con `finally` bloques asociados, el `finally` control se transfiere inicialmente al bloque de la `try` instrucción más interna. Cuando y si el control alcanza el punto final de `finally` un bloque, el `finally` control se transfiere al bloque de la siguiente instrucción `try` de inclusión. Este proceso se repite hasta `finally` que se hayan ejecutado los `try` bloques de todas las instrucciones envolventes.
*  El control se devuelve al llamador del bloque de iteradores. Este es el `MoveNext` método o `Dispose` el método del objeto de enumerador.

Dado que `yield break` una instrucción transfiere el control incondicionalmente a otra parte, el `yield break` punto final de una instrucción nunca es accesible.
