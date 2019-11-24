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

El *embedded_statement* nonterminal se utiliza para las instrucciones que aparecen dentro de otras instrucciones. El uso de *embedded_statement* en lugar de *Statement* excluye el uso de las instrucciones de declaración y las instrucciones con etiquetas en estos contextos. El ejemplo
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
genera un error en tiempo de compilación porque una instrucción `if` requiere una *embedded_statement* en lugar de una *instrucción* para su bifurcación if. Si se permitiera este código, la variable `i` se declararía, pero nunca se podría usar. Tenga en cuenta, sin embargo, que colocando la declaración de `i`en un bloque, el ejemplo es válido.

## <a name="end-points-and-reachability"></a>Extremos y disponibilidad

Cada instrucción tiene un ***punto final***. En términos intuitivos, el punto final de una instrucción es la ubicación que sigue inmediatamente a la instrucción. Las reglas de ejecución para las instrucciones compuestas (instrucciones que contienen instrucciones incrustadas) especifican la acción que se realiza cuando el control alcanza el punto final de una instrucción incrustada. Por ejemplo, cuando el control alcanza el punto final de una instrucción en un bloque, el control se transfiere a la siguiente instrucción del bloque.

Si la ejecución puede alcanzar una instrucción, se dice que la instrucción es ***accesible***. Por el contrario, si no hay ninguna posibilidad de que se ejecute una instrucción, se dice que la instrucción no es ***accesible***.

en el ejemplo
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

en el ejemplo
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
la expresión booleana de la instrucción `if` es una expresión constante porque ambos operandos del operador `==` son constantes. Como la expresión constante se evalúa en tiempo de compilación, lo que produce el valor `false`, se considera que la invocación del `Console.WriteLine` no es accesible. Sin embargo, si `i` se cambia para ser una variable local
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
la invocación de `Console.WriteLine` se considera accesible, aunque, en realidad, nunca se ejecutará.

El *bloque* de un miembro de función siempre se considera alcanzable. Al evaluar sucesivamente las reglas de disponibilidad de cada instrucción en un bloque, se puede determinar la disponibilidad de cualquier instrucción determinada.

en el ejemplo
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
la disponibilidad del segundo `Console.WriteLine` se determina de la siguiente manera:

*  La primera instrucción de expresión `Console.WriteLine` es accesible porque el bloque del método de `F` es accesible.
*  El punto final de la primera instrucción de expresión `Console.WriteLine` es accesible porque esa instrucción es accesible.
*  La instrucción `if` es accesible porque el punto final de la primera instrucción de expresión `Console.WriteLine` es accesible.
*  La segunda instrucción `Console.WriteLine` expresión es accesible porque la expresión booleana de la instrucción `if` no tiene el valor constante `false`.

Hay dos situaciones en las que se trata de un error en tiempo de compilación para que el punto final de una instrucción sea alcanzable:

*  Dado que la instrucción `switch` no permite que una sección switch pase a la siguiente sección switch, se trata de un error en tiempo de compilación para que el punto final de la lista de instrucciones de una sección switch sea accesible. Si se produce este error, normalmente es una indicación de que falta una instrucción `break`.
*  Es un error en tiempo de compilación el punto final del bloque de un miembro de función que calcula un valor al que se puede tener acceso. Si se produce este error, normalmente es una indicación de que falta una instrucción `return`.

## <a name="blocks"></a>Blocks

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

Un *bloque* que contiene una o varias instrucciones de `yield` ([la instrucción yield](statements.md#the-yield-statement)) se denomina bloque de iteradores. Los bloques de iterador se usan para implementar miembros de función como iteradores ([iteradores](classes.md#iterators)). Se aplican algunas restricciones adicionales a los bloques de iteradores:

*  Es un error en tiempo de compilación que una instrucción `return` aparezca en un bloque de iteradores (pero se permiten `yield return` instrucciones).
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
*  La instrucción es una instrucción con etiqueta y una instrucción de `goto` alcanzable hace referencia a la etiqueta.

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

Se puede usar una instrucción vacía al escribir una instrucción `while` con un cuerpo nulo:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Además, se puede usar una instrucción vacía para declarar una etiqueta justo antes de la "`}`" de cierre de un bloque:
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

Se puede hacer referencia a una etiqueta desde `goto` instrucciones ([la instrucción Goto](statements.md#the-goto-statement)) dentro del ámbito de la etiqueta. Esto significa que `goto` instrucciones pueden transferir el control dentro de los bloques y fuera de los bloques, pero nunca a los bloques.

Las etiquetas tienen su propio espacio de declaración y no interfieren con otros identificadores. El ejemplo
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
es válido y utiliza el nombre `x` como un parámetro y una etiqueta.

La ejecución de una instrucción con etiqueta corresponde exactamente a la ejecución de la instrucción que sigue a la etiqueta.

Además de la disponibilidad proporcionada por el flujo de control normal, se puede tener acceso a una instrucción con etiqueta si se hace referencia a la etiqueta mediante una instrucción de `goto` alcanzable. (Excepción: Si una instrucción `goto` está dentro de un `try` que incluye un bloque `finally` y la instrucción con etiqueta está fuera del `try`, y el punto final del bloque `finally` es inaccesible, la instrucción etiquetada no es accesible desde esa instrucción `goto`).

## <a name="declaration-statements"></a>Instrucciones de declaración

Un *declaration_statement* declara una variable o constante local. Las instrucciones de declaración se permiten en bloques, pero no se permiten como instrucciones incrustadas.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Declaraciones de variables locales

Un *local_variable_declaration* declara una o más variables locales.

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

El *local_variable_type* de una *local_variable_declaration* especifica directamente el tipo de las variables introducidas por la declaración, o indica con el identificador `var` que el tipo se debe inferir en función de un inicializador. El tipo va seguido de una lista de *local_variable_declarator*s, cada uno de los cuales introduce una nueva variable. Un *local_variable_declarator* consta de un *identificador* que asigna un nombre a la variable, seguido opcionalmente por un token "`=`" y un *local_variable_initializer* que proporciona el valor inicial de la variable.

En el contexto de una declaración de variable local, el identificador var actúa como una palabra clave contextual ([palabras clave](lexical-structure.md#keywords)). Cuando el *local_variable_type* se especifica como `var` y ningún tipo denominado `var` está en el ámbito, la declaración es una ***declaración de variable local con tipo implícito***, cuyo tipo se deduce del tipo de la expresión de inicializador asociada. Las declaraciones de variables locales con tipo implícito están sujetas a las siguientes restricciones:

*  El *local_variable_declaration* no puede incluir varios *local_variable_declarator*s.
*  El *local_variable_declarator* debe incluir un *local_variable_initializer*.
*  El *local_variable_initializer* debe ser una *expresión*.
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

El valor de una variable local se obtiene en una expresión usando un *simple_name* ([nombres simples](expressions.md#simple-names)) y el valor de una variable local se modifica mediante una *asignación* ([operadores de asignación](expressions.md#assignment-operators)). Una variable local debe estar asignada definitivamente ([asignación definitiva](variables.md#definite-assignment)) en cada ubicación donde se obtiene su valor.

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

El *tipo* de un *local_constant_declaration* especifica el tipo de las constantes introducidas por la declaración. El tipo va seguido de una lista de *constant_declarator*s, cada uno de los cuales introduce una nueva constante. Un *constant_declarator* consta de un *identificador* que nombra la constante, seguido de un token "`=`", seguido de un *constant_expression* ([expresiones constantes](expressions.md#constant-expressions)) que proporciona el valor de la constante.

El *tipo* y la *constant_expression* de una declaración de constante local deben seguir las mismas reglas que las de una declaración de miembro constante ([constantes](classes.md#constants)).

El valor de una constante local se obtiene en una expresión usando un *simple_name* ([nombres simples](expressions.md#simple-names)).

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

No todas las expresiones se permiten como instrucciones. En concreto, las expresiones como `x + y` y `x == 1` que simplemente calculan un valor (que se descartarán), no se permiten como instrucciones.

La ejecución de una *expression_statement* evalúa la expresión contenida y, a continuación, transfiere el control al punto final de la *expression_statement*. El punto final de una *expression_statement* es accesible si ese *expression_statement* es accesible.

## <a name="selection-statements"></a>Instrucciones de selección

Las instrucciones de selección seleccionan una de varias instrucciones posibles para su ejecución según el valor de alguna expresión.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>La instrucción If

La instrucción `if` selecciona una instrucción para su ejecución basada en el valor de una expresión booleana.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Un elemento `else` está asociado a la `if` anterior léxicamente más cercana permitida por la sintaxis. Por lo tanto, una instrucción `if` del formulario
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

Una instrucción `if` se ejecuta de la siguiente manera:

*  Se evalúa el *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)).
*  Si la expresión booleana produce `true`, el control se transfiere a la primera instrucción incrustada. Cuando y si el control alcanza el punto final de la instrucción, el control se transfiere al punto final de la instrucción `if`.
*  Si la expresión booleana produce `false` y, si hay una parte `else`, el control se transfiere a la segunda instrucción incrustada. Cuando y si el control alcanza el punto final de la instrucción, el control se transfiere al punto final de la instrucción `if`.
*  Si la expresión booleana produce `false` y, si no hay ninguna parte `else`, el control se transfiere al punto final de la instrucción `if`.

La primera instrucción incrustada de una instrucción `if` es accesible si la instrucción de `if` es accesible y la expresión booleana no tiene el valor constante `false`.

La segunda instrucción incrustada de una instrucción `if`, si está presente, es accesible si la instrucción de `if` es accesible y la expresión booleana no tiene el valor constante `true`.

El punto final de una instrucción `if` es accesible si el punto final de al menos una de sus instrucciones incrustadas es accesible. Además, se puede tener acceso al punto final de una instrucción `if` sin ningún elemento `else` si la instrucción `if` es accesible y la expresión booleana no tiene el valor constante `true`.

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

Un *switch_statement* consta de la palabra clave `switch`, seguida de una expresión entre paréntesis (denominada expresión switch), seguida de un *switch_block*. El *switch_block* consta de cero o más *switch_section*s, entre llaves. Cada *switch_section* consta de uno o varios *switch_label*s seguidos de un *statement_list* ([listas de instrucciones](statements.md#statement-lists)).

La expresión switch establece el ***tipo de control*** de una instrucción `switch`.

*  Si el tipo de la expresión switch es `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`o *enum_type*, o si es el tipo que acepta valores NULL correspondiente a uno de estos tipos, es el tipo aplicable de la instrucción `switch`.
*  De lo contrario, debe existir exactamente una conversión implícita definida por el usuario ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)) del tipo de la expresión switch a uno de los siguientes tipos de control posibles: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`o, un tipo que acepta valores NULL correspondiente a uno de esos tipos.
*  De lo contrario, si no existe ninguna conversión implícita, o si existe más de una conversión implícita de este tipo, se produce un error en tiempo de compilación.

La expresión constante de cada etiqueta de `case` debe indicar un valor que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de control de la instrucción `switch`. Se produce un error en tiempo de compilación si dos o más etiquetas de `case` en la misma instrucción `switch` especifican el mismo valor constante.

Puede haber como máximo una `default` etiqueta en una instrucción switch.

Una instrucción `switch` se ejecuta de la siguiente manera:

*  La expresión switch se evalúa y se convierte al tipo aplicable.
*  Si una de las constantes especificadas en una etiqueta de `case` en la misma instrucción `switch` es igual al valor de la expresión switch, el control se transfiere a la lista de instrucciones que sigue a la etiqueta `case` coincidente.
*  Si ninguna de las constantes especificadas en `case` etiquetas de la misma instrucción `switch` es igual al valor de la expresión switch, y si hay una etiqueta `default`, el control se transfiere a la lista de instrucciones que sigue a la etiqueta `default`.
*  Si ninguna de las constantes especificadas en `case` etiquetas de la misma instrucción `switch` es igual al valor de la expresión switch y no hay ninguna etiqueta `default`, el control se transfiere al punto final de la instrucción `switch`.

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
produce un error en tiempo de compilación. Cuando la ejecución de una sección switch va seguida de la ejecución de otra sección switch, se debe usar una instrucción `goto case` o `goto default` explícita:
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

En un *switch_section*se permiten varias etiquetas. El ejemplo
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

La regla "no pasar" evita una clase común de errores que se producen en C y C++ cuando `break` instrucciones se omiten accidentalmente. Además, debido a esta regla, las secciones switch de una instrucción `switch` se pueden reorganizar arbitrariamente sin afectar al comportamiento de la instrucción. Por ejemplo, las secciones de la instrucción `switch` anterior se pueden revertir sin que ello afecte al comportamiento de la instrucción:
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

La lista de instrucciones de una sección switch normalmente finaliza en una instrucción `break`, `goto case`o `goto default`, pero se permite cualquier construcción que representa el punto final de la lista de instrucciones inalcanzable. Por ejemplo, se sabe que una instrucción `while` controlada por la expresión booleana `true` nunca alcanza su punto final. Del mismo modo, una instrucción `throw` o `return` siempre transfiere el control en otro lugar y nunca alcanza su punto final. Por lo tanto, el ejemplo siguiente es válido:
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

El tipo de control de una instrucción `switch` puede ser el tipo `string`. Por ejemplo:
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

Al igual que los operadores de igualdad de cadena ([operadores de igualdad de cadena](expressions.md#string-equality-operators)), la instrucción `switch` distingue entre mayúsculas y minúsculas y ejecutará una sección de modificador determinada solo si la cadena de expresión de modificador coincide exactamente con una constante de etiqueta `case`.

Cuando el tipo de control de una instrucción `switch` es `string`, se permite el valor `null` como una constante de etiqueta de caso.

Los *statement_list*s de un *switch_block* pueden contener instrucciones de declaración ([instrucciones de declaración](statements.md#declaration-statements)). El ámbito de una variable local o constante declarada en un bloque switch es el bloque switch.

La lista de instrucciones de una sección de modificador determinada es accesible si la instrucción `switch` es accesible y se cumple al menos una de las siguientes condiciones:

*  La expresión switch es un valor que no es constante.
*  La expresión switch es un valor constante que coincide con una `case` etiqueta en la sección switch.
*  La expresión switch es un valor constante que no coincide con ninguna etiqueta `case`, y la sección switch contiene la etiqueta `default`.
*  Se hace referencia a una etiqueta switch de la sección switch mediante una instrucción `goto case` o `goto default` accesible.

El punto final de una instrucción `switch` es accesible si se cumple al menos una de las siguientes condiciones:

*  La instrucción `switch` contiene una instrucción `break` alcanzable que sale de la instrucción `switch`.
*  La instrucción `switch` es accesible, la expresión switch es un valor no constante y no hay ninguna etiqueta `default`.
*  La instrucción `switch` es accesible, la expresión switch es un valor constante que no coincide con ninguna etiqueta de `case` y no hay ninguna etiqueta de `default`.

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

La instrucción `while` ejecuta condicionalmente una instrucción incrustada cero o más veces.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

Una instrucción `while` se ejecuta de la siguiente manera:

*  Se evalúa el *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)).
*  Si la expresión booleana produce `true`, el control se transfiere a la instrucción incrustada. Cuando el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de una instrucción `continue`), el control se transfiere al principio de la instrucción `while`.
*  Si la expresión booleana produce `false`, el control se transfiere al punto final de la instrucción `while`.

Dentro de la instrucción incrustada de una instrucción de `while`, se puede usar una instrucción `break` ([instrucción break](statements.md#the-break-statement)) para transferir el control al punto final de la instrucción `while` (con lo que finaliza la iteración de la instrucción incrustada) y se puede usar una instrucción `continue` ([la instrucción continue](statements.md#the-continue-statement)) para transferir el control al punto final de la instrucción incrustada (con lo que se realiza otra `while` iteración

La instrucción incrustada de una instrucción `while` es accesible si la instrucción de `while` es accesible y la expresión booleana no tiene el valor constante `false`.

El punto final de una instrucción `while` es accesible si se cumple al menos una de las siguientes condiciones:

*  La instrucción `while` contiene una instrucción `break` alcanzable que sale de la instrucción `while`.
*  La instrucción `while` es accesible y la expresión booleana no tiene el valor constante `true`.

### <a name="the-do-statement"></a>La instrucción do

La instrucción `do` ejecuta condicionalmente una instrucción incrustada una o varias veces.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

Una instrucción `do` se ejecuta de la siguiente manera:

*  El control se transfiere a la instrucción insertada.
*  Cuando el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de una instrucción `continue`), se evalúa el *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)). Si la expresión booleana produce `true`, el control se transfiere al principio de la instrucción de `do`. De lo contrario, el control se transfiere al punto final de la instrucción `do`.

Dentro de la instrucción incrustada de una instrucción de `do`, se puede usar una instrucción `break` ([la instrucción break](statements.md#the-break-statement)) para transferir el control al punto final de la instrucción `do` (con lo que finaliza la iteración de la instrucción incrustada) y se puede usar una instrucción `continue` ([la instrucción continue](statements.md#the-continue-statement)) para transferir el control al punto final de la instrucción insertada.

La instrucción incrustada de una instrucción `do` es accesible si la instrucción de `do` es accesible.

El punto final de una instrucción `do` es accesible si se cumple al menos una de las siguientes condiciones:

*  La instrucción `do` contiene una instrucción `break` alcanzable que sale de la instrucción `do`.
*  El punto final de la instrucción incrustada es accesible y la expresión booleana no tiene el valor constante `true`.

### <a name="the-for-statement"></a>La instrucción for

La instrucción `for` evalúa una secuencia de expresiones de inicialización y, a continuación, mientras una condición es true, ejecuta repetidamente una instrucción incrustada y evalúa una secuencia de expresiones de iteración.

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

El *for_initializer*, si está presente, consta de un *local_variable_declaration* ([declaraciones de variables locales](statements.md#local-variable-declarations)) o una lista de *statement_expression*s ([instrucciones de expresión](statements.md#expression-statements)) separadas por comas. El ámbito de una variable local declarada por una *for_initializer* comienza en el *local_variable_declarator* de la variable y se extiende hasta el final de la instrucción incrustada. El ámbito incluye el *for_condition* y el *for_iterator*.

El *for_condition*, si está presente, debe ser un *Boolean_expression* ([Expresiones booleanas](expressions.md#boolean-expressions)).

El *for_iterator*, si está presente, consta de una lista de *statement_expression*s ([instrucciones de expresión](statements.md#expression-statements)) separadas por comas.

Una instrucción for se ejecuta de la siguiente manera:

*  Si hay un *for_initializer* , los inicializadores variables o las expresiones de instrucción se ejecutan en el orden en que se escriben. Este paso solo se realiza una vez.
*  Si hay un *for_condition* , se evalúa.
*  Si el *for_condition* no está presente o si la evaluación produce `true`, el control se transfiere a la instrucción insertada. Cuando el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de una instrucción `continue`), las expresiones del *for_iterator*, si las hay, se evalúan en secuencia y, a continuación, se realiza otra iteración, comenzando por la evaluación de la *for_condition* en el paso anterior.
*  Si el *for_condition* está presente y la evaluación produce `false`, el control se transfiere al punto final de la instrucción `for`.

Dentro de la instrucción incrustada de una instrucción `for`, una instrucción `break` ([instrucción break](statements.md#the-break-statement)) se puede utilizar para transferir el control al punto final de la instrucción `for` (por lo tanto, finalizar la iteración de la instrucción incrustada) y se puede usar una instrucción `continue` ([la instrucción continue](statements.md#the-continue-statement)) para transferir el control al punto final de la instrucción incrustada (lo que ejecuta la *for_iterator* y realiza otra iteración de la instrucción `for`, empezando por la *for_condition*

La instrucción insertada de una instrucción `for` es accesible si se cumple una de las siguientes condiciones:

*  La instrucción `for` es accesible y no hay ningún *for_condition* presente.
*  La instrucción `for` es accesible y hay una *for_condition* presente y no tiene el valor constante `false`.

El punto final de una instrucción `for` es accesible si se cumple al menos una de las siguientes condiciones:

*  La instrucción `for` contiene una instrucción `break` alcanzable que sale de la instrucción `for`.
*  La instrucción `for` es accesible y hay una *for_condition* presente y no tiene el valor constante `true`.

### <a name="the-foreach-statement"></a>La instrucción foreach

La instrucción `foreach` enumera los elementos de una colección y ejecuta una instrucción incrustada para cada elemento de la colección.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

El *tipo* y el *identificador* de una instrucción `foreach` declaran la ***variable de iteración*** de la instrucción. Si el identificador de `var` se proporciona como el *local_variable_type*y ningún tipo denominado `var` está en el ámbito, se dice que la variable de iteración es una ***variable de iteración con tipo implícito***y que su tipo es el tipo de elemento de la instrucción de `foreach`, tal y como se especifica a continuación. La variable de iteración corresponde a una variable local de solo lectura con un ámbito que se extiende a través de la instrucción incrustada. Durante la ejecución de una instrucción de `foreach`, la variable de iteración representa el elemento de colección para el que se está realizando actualmente una iteración. Se produce un error en tiempo de compilación si la instrucción insertada intenta modificar la variable de iteración (a través de la asignación o los operadores `++` y `--`) o pasar la variable de iteración como un `ref` o `out` parámetro.

En el siguiente, para mayor brevedad, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` y `IEnumerator<T>` hacen referencia a los tipos correspondientes de los espacios de nombres `System.Collections` y `System.Collections.Generic`.

El procesamiento en tiempo de compilación de una instrucción foreach primero determina el ***tipo de colección***, el tipo de ***enumerador*** y el ***tipo de elemento*** de la expresión. Esta determinación se realiza de la siguiente manera:

*  Si el tipo `X` de *Expression* es un tipo de matriz, hay una conversión de referencia implícita de `X` a la interfaz `IEnumerable` (ya que `System.Array` implementa esta interfaz). El ***tipo de colección*** es la interfaz `IEnumerable`, el ***tipo de enumerador*** es la interfaz `IEnumerator` y el tipo de ***elemento*** es el tipo de elemento del tipo de matriz `X`.
*  Si el tipo `X` de la *expresión* es `dynamic`, hay una conversión implícita de la *expresión* a la interfaz `IEnumerable` ([conversiones dinámicas implícitas](conversions.md#implicit-dynamic-conversions)). El ***tipo de colección*** es la interfaz `IEnumerable` y el ***tipo de enumerador*** es la interfaz `IEnumerator`. Si se proporciona el identificador de `var` como *local_variable_type* , el ***tipo de elemento*** es `dynamic`; en caso contrario, es `object`.
*  De lo contrario, determine si el tipo `X` tiene un método `GetEnumerator` adecuado:
   * Realiza la búsqueda de miembros en el tipo `X` con el identificador `GetEnumerator` y ningún argumento de tipo. Si la búsqueda de miembros no produce una coincidencia, o produce una ambigüedad, o produce una coincidencia que no es un grupo de métodos, busque una interfaz enumerable como se describe a continuación. Se recomienda emitir una advertencia si la búsqueda de miembros produce cualquier cosa, excepto un grupo de métodos o ninguna coincidencia.
   * Realizar la resolución de sobrecarga utilizando el grupo de métodos resultante y una lista de argumentos vacía. Si la resolución de sobrecarga da como resultado ningún método aplicable, da como resultado una ambigüedad o tiene como resultado un único método mejor pero ese método es estático o no público, busque una interfaz enumerable como se describe a continuación. Se recomienda emitir una advertencia si la resolución de sobrecarga produce todo excepto un método de instancia pública inequívoco o ningún método aplicable.
   * Si el tipo de valor devuelto `E` del método `GetEnumerator` no es una clase, una estructura o un tipo de interfaz, se genera un error y no se realiza ningún otro paso.
   * La búsqueda de miembros se realiza en `E` con el identificador `Current` y ningún argumento de tipo. Si la búsqueda de miembros no produce ninguna coincidencia, el resultado es un error o el resultado es cualquier cosa excepto una propiedad de instancia pública que permita la lectura, se genera un error y no se realiza ningún paso más.
   * La búsqueda de miembros se realiza en `E` con el identificador `MoveNext` y ningún argumento de tipo. Si la búsqueda de miembros no produce ninguna coincidencia, el resultado es un error o el resultado es cualquier cosa excepto un grupo de métodos, se genera un error y no se realiza ningún paso más.
   * La resolución de sobrecarga se realiza en el grupo de métodos con una lista de argumentos vacía. Si la resolución de sobrecarga da como resultado ningún método aplicable, da como resultado una ambigüedad o tiene como resultado un único método mejor pero ese método es estático o no público, o su tipo de valor devuelto no se `bool`, se genera un error y no se realiza ningún otro paso.
   * El ***tipo de colección*** es `X`, el ***tipo de enumerador*** es `E`y el tipo de ***elemento*** es el tipo de la propiedad `Current`.

*  De lo contrario, compruebe si hay una interfaz enumerable:
   * Si entre todos los tipos `Ti` para los que hay una conversión implícita de `X` a `IEnumerable<Ti>`, hay un tipo único `T` tal que `T` no se `dynamic` y para el resto `Ti` existe una conversión implícita de `IEnumerable<T>` a `IEnumerable<Ti>`, entonces el ***tipo de colección*** es la interfaz `IEnumerable<T>`, el tipo de ***enumerador*** es la interfaz `IEnumerator<T>`y el ***tipo de elemento*** es `T`.
   * De lo contrario, si hay más de un tipo de este tipo `T`, se genera un error y no se realiza ningún otro paso.
   * De lo contrario, si hay una conversión implícita de `X` a la interfaz `System.Collections.IEnumerable`, el ***tipo de colección*** es esta interfaz, el ***tipo de enumerador*** es la interfaz `System.Collections.IEnumerator`y se `object`el ***tipo de elemento*** .
   * De lo contrario, se genera un error y no se realiza ningún paso más.

Los pasos anteriores, si son correctos, producen inequívocamente un tipo de colección `C`, un tipo de enumerador `E` y un tipo de elemento `T`. Una instrucción foreach con el formato
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

La variable `e` no es visible o accesible para la expresión `x` o la instrucción incrustada o cualquier otro código fuente del programa. La variable `v` es de solo lectura en la instrucción insertada. Si no hay una conversión explícita ([conversiones explícitas](conversions.md#explicit-conversions)) de `T` (el tipo de elemento) en `V` (el *local_variable_type* en la instrucción foreach), se genera un error y no se realiza ningún otro paso. Si `x` tiene el valor `null`, se produce una `System.NullReferenceException` en tiempo de ejecución.

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
Si `v` se declaró fuera del bucle while, se compartirá entre todas las iteraciones y su valor después del bucle for sería el valor final, `13`, que es lo que la invocación de `f` imprimiría. En su lugar, dado que cada iteración tiene su propia variable `v`, la captura de `f` en la primera iteración seguirá conservando el valor `7`, que es lo que se va a imprimir. (Nota: las versiones anteriores C# de declaraban `v` fuera del bucle while).

El cuerpo del bloque finally se construye según los pasos siguientes:

*  Si hay una conversión implícita de `E` a la interfaz `System.IDisposable`,
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

   salvo que si `E` es un tipo de valor o un parámetro de tipo al que se ha creado una instancia de un tipo de valor, la conversión de `e` a `System.IDisposable` no hará que se produzca la conversión boxing.

*  De lo contrario, si `E` es un tipo sellado, la cláusula finally se expande a un bloque vacío:

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

   La variable local `d` no es visible o accesible para cualquier código de usuario. En concreto, no entra en conflicto con ninguna otra variable cuyo ámbito incluya el bloque Finally.

El orden en el que `foreach` atraviesa los elementos de una matriz es el siguiente: para las matrices unidimensionales, los elementos se recorren en orden creciente de índice, empezando por el `0` de índice y terminando por el índice `Length - 1`. En el caso de las matrices multidimensionales, los elementos se recorren de forma que los índices de la dimensión situada más a la derecha aumenten primero, la dimensión izquierda siguiente y así sucesivamente hacia la izquierda.

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

en el ejemplo
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
el tipo de `n` se deduce que se va a `int`, el tipo de elemento de `numbers`.

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

La ejecución de instrucciones de salto es complicada por la presencia de instrucciones `try` intermedias. En ausencia de estas instrucciones `try`, una instrucción de salto transfiere incondicionalmente el control de la instrucción de salto a su destino. En presencia de tales instrucciones de `try` intermedias, la ejecución es más compleja. Si la instrucción de salto sale de uno o varios bloques de `try` con los bloques de `finally` asociados, el control se transfiere inicialmente al bloque de `finally` de la instrucción de `try` más interna. Cuando el control alcanza el punto final de un bloque `finally`, el control se transfiere al bloque `finally` de la siguiente instrucción `try` envolvente. Este proceso se repite hasta que se hayan ejecutado los bloques de `finally` de todas las instrucciones de `try` intermedias.

en el ejemplo
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
los bloques `finally` asociados con dos instrucciones `try` se ejecutan antes de que el control se transfiera al destino de la instrucción de salto.

La salida generada es la siguiente:
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Instrucción break

La instrucción `break` sale de la instrucción envolvente `switch`, `while`, `do`, `for`o `foreach` más cercana.

```antlr
break_statement
    : 'break' ';'
    ;
```

El destino de una instrucción `break` es el punto final de la instrucción envolvente `switch`, `while`, `do`, `for`o `foreach` más cercana. Si una instrucción `break` no está delimitada por una instrucción `switch`, `while`, `do`, `for`o `foreach`, se produce un error en tiempo de compilación.

Cuando varias instrucciones `switch`, `while`, `do`, `for`o `foreach` se anidan entre sí, una instrucción `break` se aplica solo a la instrucción más interna. Para transferir el control a través de varios niveles de anidamiento, se debe usar una instrucción `goto` ([instrucción Goto](statements.md#the-goto-statement)).

Una instrucción `break` no puede salir de un bloque de `finally` ([la instrucción try](statements.md#the-try-statement)). Cuando una instrucción de `break` se produce dentro de un bloque de `finally`, el destino de la instrucción `break` debe estar dentro del mismo bloque de `finally`; de lo contrario, se produce un error en tiempo de compilación.

Una instrucción `break` se ejecuta de la siguiente manera:

*  Si la instrucción `break` sale de uno o varios bloques de `try` con los bloques de `finally` asociados, el control se transfiere inicialmente al bloque de `finally` de la instrucción de `try` más interna. Cuando el control alcanza el punto final de un bloque `finally`, el control se transfiere al bloque `finally` de la siguiente instrucción `try` envolvente. Este proceso se repite hasta que se hayan ejecutado los bloques de `finally` de todas las instrucciones de `try` intermedias.
*  El control se transfiere al destino de la instrucción `break`.

Dado que una instrucción `break` transfiere incondicionalmente el control en otro lugar, nunca se puede obtener acceso al punto final de una instrucción de `break`.

### <a name="the-continue-statement"></a>La instrucción continue

La instrucción `continue` inicia una nueva iteración de la instrucción envolvente `while`, `do`, `for`o `foreach` más cercana.

```antlr
continue_statement
    : 'continue' ';'
    ;
```

El destino de una instrucción `continue` es el punto final de la instrucción incrustada de la instrucción envolvente `while`, `do`, `for`o `foreach` más cercana. Si una instrucción `continue` no está delimitada por una instrucción `while`, `do`, `for`o `foreach`, se produce un error en tiempo de compilación.

Cuando varias instrucciones `while`, `do`, `for`o `foreach` se anidan entre sí, una instrucción `continue` se aplica solo a la instrucción más interna. Para transferir el control a través de varios niveles de anidamiento, se debe usar una instrucción `goto` ([instrucción Goto](statements.md#the-goto-statement)).

Una instrucción `continue` no puede salir de un bloque de `finally` ([la instrucción try](statements.md#the-try-statement)). Cuando una instrucción de `continue` se produce dentro de un bloque de `finally`, el destino de la instrucción `continue` debe estar dentro del mismo bloque de `finally`; en caso contrario, se produce un error en tiempo de compilación.

Una instrucción `continue` se ejecuta de la siguiente manera:

*  Si la instrucción `continue` sale de uno o varios bloques de `try` con los bloques de `finally` asociados, el control se transfiere inicialmente al bloque de `finally` de la instrucción de `try` más interna. Cuando el control alcanza el punto final de un bloque `finally`, el control se transfiere al bloque `finally` de la siguiente instrucción `try` envolvente. Este proceso se repite hasta que se hayan ejecutado los bloques de `finally` de todas las instrucciones de `try` intermedias.
*  El control se transfiere al destino de la instrucción `continue`.

Dado que una instrucción `continue` transfiere incondicionalmente el control en otro lugar, nunca se puede obtener acceso al punto final de una instrucción de `continue`.

### <a name="the-goto-statement"></a>La instrucción goto

La instrucción `goto` transfiere el control a una instrucción marcada por una etiqueta.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

El destino de una instrucción de *identificador* `goto` es la instrucción con etiqueta con la etiqueta especificada. Si no existe una etiqueta con el nombre especificado en el miembro de función actual, o si la instrucción `goto` no está dentro del ámbito de la etiqueta, se produce un error en tiempo de compilación. Esta regla permite el uso de una instrucción `goto` para transferir el control fuera de un ámbito anidado, pero no a un ámbito anidado. en el ejemplo
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
se utiliza una instrucción `goto` para transferir el control fuera de un ámbito anidado.

El destino de una instrucción `goto case` es la lista de instrucciones de la instrucción de `switch` de inclusión inmediata ([la instrucción switch](statements.md#the-switch-statement)), que contiene una etiqueta de `case` con el valor constante especificado. Si la instrucción de `goto case` no está delimitada por una instrucción de `switch`, si el *constant_expression* no se convierte implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) al tipo de control de la instrucción de `switch` envolvente más cercana, o si la instrucción de `switch` envolvente más cercana no contiene una etiqueta de `case` con el valor constante especificado, se produce un error en tiempo de compilación.

El destino de una instrucción `goto default` es la lista de instrucciones de la instrucción de `switch` de inclusión inmediata ([la instrucción switch](statements.md#the-switch-statement)), que contiene una etiqueta de `default`. Si la instrucción de `goto default` no está delimitada por una instrucción de `switch` o si la instrucción de `switch` envolvente más cercana no contiene una etiqueta de `default`, se produce un error en tiempo de compilación.

Una instrucción `goto` no puede salir de un bloque de `finally` ([la instrucción try](statements.md#the-try-statement)). Cuando una instrucción de `goto` se produce dentro de un bloque de `finally`, el destino de la instrucción `goto` debe estar dentro del mismo bloque de `finally` o, de lo contrario, se producirá un error en tiempo de compilación.

Una instrucción `goto` se ejecuta de la siguiente manera:

*  Si la instrucción `goto` sale de uno o varios bloques de `try` con los bloques de `finally` asociados, el control se transfiere inicialmente al bloque de `finally` de la instrucción de `try` más interna. Cuando el control alcanza el punto final de un bloque `finally`, el control se transfiere al bloque `finally` de la siguiente instrucción `try` envolvente. Este proceso se repite hasta que se hayan ejecutado los bloques de `finally` de todas las instrucciones de `try` intermedias.
*  El control se transfiere al destino de la instrucción `goto`.

Dado que una instrucción `goto` transfiere incondicionalmente el control en otro lugar, nunca se puede obtener acceso al punto final de una instrucción de `goto`.

### <a name="the-return-statement"></a>La instrucción return

La instrucción `return` devuelve el control al autor de la llamada actual de la función en la que aparece la instrucción `return`.

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

Una instrucción `return` sin expresión solo se puede usar en un miembro de función que no calcule un valor, es decir, un método con el tipo de resultado ([cuerpo del método](classes.md#method-body)) `void`, el descriptor de acceso `set` de una propiedad o un indizador, los descriptores de acceso `add` y `remove` de un evento, un constructor de instancia, un constructor estático o un destructor.

Una instrucción `return` con una expresión solo se puede usar en un miembro de función que calcula un valor, es decir, un método con un tipo de resultado no void, el descriptor de acceso `get` de una propiedad o un indizador, o un operador definido por el usuario. Debe existir una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo de la expresión al tipo de valor devuelto del miembro de función contenedora.

Las instrucciones Return también se pueden utilizar en el cuerpo de expresiones de función anónimas ([expresiones de función anónimas](expressions.md#anonymous-function-expressions)) y participar en la determinación de las conversiones que existen para esas funciones.

Es un error en tiempo de compilación que una instrucción `return` aparezca en un bloque de `finally` ([la instrucción try](statements.md#the-try-statement)).

Una instrucción `return` se ejecuta de la siguiente manera:

*  Si la instrucción `return` especifica una expresión, se evalúa la expresión y el valor resultante se convierte al tipo de valor devuelto de la función contenedora mediante una conversión implícita. El resultado de la conversión se convierte en el valor de resultado producido por la función.
*  Si la instrucción `return` está incluida en uno o varios bloques `try` o `catch` con los bloques `finally` asociados, el control se transfiere inicialmente al bloque `finally` de la instrucción de `try` más interna. Cuando el control alcanza el punto final de un bloque `finally`, el control se transfiere al bloque `finally` de la siguiente instrucción `try` envolvente. Este proceso se repite hasta que se hayan ejecutado los bloques de `finally` de todas las instrucciones de `try` envolventes.
*  Si la función contenedora no es una función asincrónica, el control se devuelve al autor de la llamada de la función contenedora junto con el valor del resultado, si existe.
*  Si la función contenedora es una función asincrónica, el control se devuelve al llamador actual y el valor del resultado, si existe, se registra en la tarea devuelta tal y como se describe en ([interfaces del enumerador](classes.md#enumerator-interfaces)).

Dado que una instrucción `return` transfiere incondicionalmente el control en otro lugar, nunca se puede obtener acceso al punto final de una instrucción de `return`.

### <a name="the-throw-statement"></a>La instrucción throw

La instrucción `throw` produce una excepción.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

Una instrucción `throw` con una expresión inicia el valor generado al evaluar la expresión. La expresión debe indicar un valor del tipo de clase `System.Exception`, de un tipo de clase que deriva de `System.Exception` o de un tipo de parámetro de tipo que tiene `System.Exception` (o una subclase de ella) como su clase base efectiva. Si la evaluación de la expresión produce `null`, se produce una `System.NullReferenceException` en su lugar.

Una instrucción `throw` sin expresión solo se puede usar en un bloque de `catch`, en cuyo caso la instrucción vuelve a iniciar la excepción que el bloque de `catch` está controlando actualmente.

Dado que una instrucción `throw` transfiere incondicionalmente el control en otro lugar, nunca se puede obtener acceso al punto final de una instrucción de `throw`.

Cuando se produce una excepción, el control se transfiere a la primera cláusula `catch` en una instrucción `try` envolvente que puede controlar la excepción. El proceso que tiene lugar desde el punto de la excepción que se está iniciando hasta el punto de transferir el control a un controlador de excepciones adecuado se conoce como ***propagación de excepciones***. La propagación de una excepción consiste en evaluar repetidamente los siguientes pasos hasta que se encuentre una cláusula `catch` que coincida con la excepción. En esta descripción, el ***punto de inicio*** es inicialmente la ubicación en la que se produce la excepción.

*  En el miembro de función actual, se examina cada instrucción `try` que incluye el punto de inicio. Para cada instrucción `S`, empezando por la instrucción de `try` más interna y terminando con la instrucción de `try` más externa, se evalúan los pasos siguientes:

   * Si el `try` bloque de `S` incluye el punto de inicio y si S tiene una o más cláusulas `catch`, las cláusulas `catch` se examinan en orden de aparición para encontrar un controlador adecuado para la excepción, según las reglas especificadas en [la sección instrucción try](statements.md#the-try-statement). Si se encuentra una cláusula de `catch` coincidente, la propagación de excepciones se completa mediante la transferencia del control al bloque de esa cláusula `catch`.

   * De lo contrario, si el bloque de `try` o un bloque de `catch` de `S` incluye el punto de inicio y si `S` tiene un bloque de `finally`, el control se transfiere al bloque de `finally`. Si el bloque `finally` inicia otra excepción, se finaliza el procesamiento de la excepción actual. De lo contrario, cuando el control alcanza el punto final del bloque `finally`, continúa el procesamiento de la excepción actual.

*  Si no se encuentra un controlador de excepciones en la invocación de función actual, se termina la invocación de la función y se produce uno de los siguientes casos:

   * Si la función actual no es asincrónica, los pasos anteriores se repiten para el llamador de la función con un punto de inicio correspondiente a la instrucción de la que se invocó el miembro de función.

   * Si la función actual es asincrónica y devuelve la tarea, la excepción se registra en la tarea devuelta, que se coloca en un estado de error o cancelado, tal y como se describe en [interfaces de enumerador](classes.md#enumerator-interfaces).

   * Si la función actual es asincrónica y devuelve void, el contexto de sincronización del subproceso actual se notifica como se describe en [interfaces enumerables](classes.md#enumerable-interfaces).

*  Si el procesamiento de excepciones finaliza todas las invocaciones de miembros de función en el subproceso actual, lo que indica que el subproceso no tiene ningún controlador para la excepción, el subproceso se termina. El impacto de dicha terminación está definido por la implementación.

## <a name="the-try-statement"></a>Instrucción try

La instrucción `try` proporciona un mecanismo para detectar las excepciones que se producen durante la ejecución de un bloque. Además, la instrucción `try` proporciona la capacidad de especificar un bloque de código que siempre se ejecuta cuando el control sale de la instrucción `try`.

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

*  Un bloque `try` seguido de uno o varios bloques de `catch`.
*  Un bloque `try` seguido de un bloque `finally`.
*  Un bloque `try` seguido de uno o varios bloques `catch` seguidos de un bloque `finally`.

Cuando una cláusula `catch` especifica un *exception_specifier*, el tipo debe ser `System.Exception`, un tipo que se deriva de `System.Exception` o un tipo de parámetro de tipo que tiene `System.Exception` (o una subclase) como su clase base efectiva.

Cuando una cláusula `catch` especifica un *exception_specifier* con un *identificador*, se declara una ***variable de excepción*** con el nombre y el tipo especificados. La variable de excepción corresponde a una variable local con un ámbito que se extiende por la cláusula `catch`. Durante la ejecución del *exception_filter* y el *bloque*, la variable de excepción representa la excepción que se está controlando actualmente. A efectos de la comprobación de asignación definitiva, la variable de excepción se considera asignada definitivamente en todo el ámbito.

A menos que una cláusula `catch` incluya un nombre de variable de excepción, no es posible tener acceso al objeto de excepción en el bloque de filtro y `catch`.

Una cláusula de `catch` que no especifica una *exception_specifier* se denomina cláusula de `catch` general.

Algunos lenguajes de programación pueden admitir excepciones que no pueden representarse como un objeto derivado de `System.Exception`, aunque estas excepciones nunca podrían generarse mediante C# código. Se puede usar una cláusula de `catch` general para detectar tales excepciones. Por lo tanto, una cláusula de `catch` general es semánticamente diferente de una que especifica el tipo `System.Exception`, en que la primera también puede detectar excepciones de otros lenguajes.

Para buscar un controlador de una excepción, las cláusulas `catch` se examinan en orden léxico. Si una cláusula `catch` especifica un tipo pero no un filtro de excepción, se trata de un error en tiempo de compilación para una cláusula `catch` posterior en la misma instrucción `try` para especificar un tipo que es igual o derivado de ese tipo. Si una cláusula `catch` no especifica ningún tipo y no hay ningún filtro, debe ser la última cláusula `catch` para esa instrucción `try`.

Dentro de un bloque `catch`, se puede usar una instrucción `throw` ([instrucción throw](statements.md#the-throw-statement)) sin expresión para volver a producir la excepción detectada por el bloque `catch`. Las asignaciones a una variable de excepción no modifican la excepción que se vuelve a iniciar.

en el ejemplo
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

Si el primer bloque catch se hubiera producido `e` en lugar de volver a producir la excepción actual, la salida generada sería la siguiente:
```console
Exception in F: G
Exception in Main: F
```

Se trata de un error en tiempo de compilación para que una instrucción `break`, `continue`o `goto` transfiera el control fuera de un bloque de `finally`. Cuando se produce una instrucción `break`, `continue`o `goto` en un bloque de `finally`, el destino de la instrucción debe estar dentro del mismo bloque `finally` o, de lo contrario, se producirá un error en tiempo de compilación.

Se trata de un error en tiempo de compilación para que una instrucción `return` se produzca en un bloque de `finally`.

Una instrucción `try` se ejecuta de la siguiente manera:

*  El control se transfiere al bloque `try`.
*  Cuando el control alcanza el punto final del bloque `try`:
   *  Si la instrucción `try` tiene un bloque `finally`, se ejecuta el bloque `finally`.
   *  El control se transfiere al punto final de la instrucción `try`.

*  Si se propaga una excepción a la instrucción `try` durante la ejecución del bloque de `try`:
   *  Las cláusulas `catch`, si las hay, se examinan en orden de aparición para encontrar un controlador adecuado para la excepción. Si una cláusula `catch` no especifica un tipo, o especifica el tipo de excepción o un tipo base del tipo de excepción:
      *  Si la cláusula `catch` declara una variable de excepción, el objeto de excepción se asigna a la variable de excepción.
      *  Si la cláusula `catch` declara un filtro de excepción, se evalúa el filtro. Si se evalúa como `false`, la cláusula catch no es una coincidencia y la búsqueda continúa a través de las cláusulas de `catch` subsiguientes para un controlador adecuado.
      *  De lo contrario, la cláusula de `catch` se considera una coincidencia y el control se transfiere al bloque de `catch` coincidente.
      *  Cuando el control alcanza el punto final del bloque `catch`:
         * Si la instrucción `try` tiene un bloque `finally`, se ejecuta el bloque `finally`.
         * El control se transfiere al punto final de la instrucción `try`.
      *  Si se propaga una excepción a la instrucción `try` durante la ejecución del bloque de `catch`:
         *  Si la instrucción `try` tiene un bloque `finally`, se ejecuta el bloque `finally`.
         *  La excepción se propaga a la siguiente instrucción `try` envolvente.
   *  Si la instrucción `try` no tiene ninguna cláusula `catch` o si ninguna cláusula `catch` coincide con la excepción:
      *  Si la instrucción `try` tiene un bloque `finally`, se ejecuta el bloque `finally`.
      *  La excepción se propaga a la siguiente instrucción `try` envolvente.

Las instrucciones de un bloque `finally` siempre se ejecutan cuando el control sale de una instrucción `try`. Esto es cierto si la transferencia de control se produce como resultado de la ejecución normal, como resultado de la ejecución de una instrucción `break`, `continue`, `goto`o `return`, o como resultado de propagar una excepción fuera de la instrucción `try`.

Si se produce una excepción durante la ejecución de un bloque `finally` y no se detecta en el mismo bloque Finally, la excepción se propaga a la siguiente instrucción `try` envolvente. Si hay otra excepción en el proceso de propagación, se perderá esa excepción. El proceso de propagación de una excepción se describe con más detalle en la descripción de la instrucción `throw` ([instrucción throw](statements.md#the-throw-statement)).

Se puede tener acceso al bloque `try` de una instrucción `try` si la instrucción `try` es accesible.

Se puede tener acceso a un bloque `catch` de una instrucción `try` si la instrucción `try` es accesible.

Se puede tener acceso al bloque `finally` de una instrucción `try` si la instrucción `try` es accesible.

El punto final de una instrucción `try` es accesible si se cumplen las dos condiciones siguientes:

*  El punto final del bloque `try` es accesible o se puede tener acceso al punto final de al menos un bloque `catch`.
*  Si hay un bloque de `finally`, el punto final del bloque de `finally` es accesible.

## <a name="the-checked-and-unchecked-statements"></a>Las instrucciones checked y unchecked

Las instrucciones `checked` y `unchecked` se utilizan para controlar el ***contexto de comprobación de desbordamiento*** para conversiones y operaciones aritméticas de tipo entero.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

La instrucción `checked` hace que todas las expresiones del *bloque* se evalúen en un contexto comprobado y la instrucción `unchecked` hace que todas las expresiones del *bloque* se evalúen en un contexto no comprobado.

Las instrucciones `checked` y `unchecked` son exactamente equivalentes a los operadores `checked` y `unchecked` ([los operadores Checked y unchecked](expressions.md#the-checked-and-unchecked-operators)), salvo que operan en bloques en lugar de en expresiones.

## <a name="the-lock-statement"></a>La instrucción lock

La instrucción `lock` obtiene el bloqueo de exclusión mutua para un objeto determinado, ejecuta una instrucción y, a continuación, libera el bloqueo.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

La expresión de una instrucción `lock` debe indicar un valor de un tipo conocido como *reference_type*. No se realiza ninguna conversión boxing implícita ([conversiones boxing](conversions.md#boxing-conversions)) en la expresión de una instrucción `lock` y, por tanto, es un error en tiempo de compilación para que la expresión denote un valor de un *value_type*.

Una instrucción `lock` del formulario
```csharp
lock (x) ...
```
donde `x` es una expresión de un *reference_type*, es precisamente equivalente a
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

No se recomienda bloquear `System.Type` objetos para sincronizar el acceso a los datos estáticos. Otro código podría bloquearse en el mismo tipo, lo que puede provocar un interbloqueo. Un mejor enfoque es sincronizar el acceso a los datos estáticos bloqueando un objeto estático privado. Por ejemplo:
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

La instrucción `using` obtiene uno o más recursos, ejecuta una instrucción y, a continuación, desecha el recurso.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

Un ***recurso*** es una clase o estructura que implementa `System.IDisposable`, que incluye un método sin parámetros único denominado `Dispose`. El código que usa un recurso puede llamar a `Dispose` para indicar que el recurso ya no es necesario. Si no se llama a `Dispose`, la eliminación automática se produce finalmente como consecuencia de la recolección de elementos no utilizados.

Si el formato de *resource_acquisition* es *local_variable_declaration* , el tipo de la *local_variable_declaration* debe ser `dynamic` o un tipo que se pueda convertir implícitamente a `System.IDisposable`. Si el formato de *resource_acquisition* es *Expression* , esta expresión debe poder convertirse implícitamente a `System.IDisposable`.

Las variables locales declaradas en un *resource_acquisition* son de solo lectura y deben incluir un inicializador. Se produce un error en tiempo de compilación si la instrucción incrustada intenta modificar estas variables locales (a través de la asignación o los operadores `++` y `--`), tomar la dirección de ellas o pasarlas como `ref` o `out` parámetros.

Una instrucción `using` se traduce en tres partes: adquisición, uso y eliminación. El uso del recurso se adjunta implícitamente en una instrucción `try` que incluye una cláusula `finally`. Esta cláusula `finally` desecha el recurso. Si se adquiere un recurso de `null`, no se realiza ninguna llamada a `Dispose` y no se produce ninguna excepción. Si el recurso es de tipo `dynamic` se convierte dinámicamente mediante una conversión dinámica implícita ([conversiones dinámicas implícitas](conversions.md#implicit-dynamic-conversions)) en `IDisposable` durante la adquisición para asegurarse de que la conversión se realiza correctamente antes del uso y la eliminación.

Una instrucción `using` del formulario
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

De lo contrario, cuando `ResourceType` es un tipo de valor que acepta valores NULL o un tipo de referencia distinto de `dynamic`, la expansión es
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

De lo contrario, cuando se `dynamic``ResourceType`, la expansión es
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

En cualquier expansión, la variable `resource` es de solo lectura en la instrucción incrustada, y no se puede tener acceso a la variable `d` en y no es visible para la instrucción incrustada.

Una implementación puede implementar una instrucción using determinada de forma diferente, por ejemplo, por motivos de rendimiento, siempre que el comportamiento sea coherente con la expansión anterior.

Una instrucción `using` del formulario
```csharp
using (expression) statement
```
tiene las mismas tres expansiones posibles. En este caso `ResourceType` es implícitamente el tipo en tiempo de compilación del `expression`, si tiene uno. En caso contrario, se utiliza la interfaz `IDisposable` misma como `ResourceType`. No se puede obtener acceso a la variable `resource` en y no es visible para la instrucción incrustada.

Cuando un *resource_acquisition* adopta la forma de un *local_variable_declaration*, es posible adquirir varios recursos de un tipo determinado. Una instrucción `using` del formulario
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
es precisamente equivalente a una secuencia de instrucciones `using` anidadas:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

En el ejemplo siguiente se crea un archivo denominado `log.txt` y se escriben dos líneas de texto en el archivo. A continuación, el ejemplo abre el mismo archivo para leer y copia las líneas de texto contenidas en la consola.
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

Dado que las clases `TextWriter` y `TextReader` implementan la interfaz `IDisposable`, el ejemplo puede utilizar las instrucciones `using` para asegurarse de que el archivo subyacente se cierra correctamente después de las operaciones de escritura o lectura.

## <a name="the-yield-statement"></a>La instrucción yield

La instrucción `yield` se usa en un bloque de iteradores ([bloques](statements.md#blocks)) para producir un valor en el objeto enumerador ([objetos Enumerator](classes.md#enumerator-objects)) o el objeto enumerable ([objetos enumerables](classes.md#enumerable-objects)) de un iterador o para indicar el final de la iteración.

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` no es una palabra reservada; tiene un significado especial solo cuando se usa inmediatamente antes de una palabra clave `return` o `break`. En otros contextos, `yield` se puede usar como identificador.

Existen varias restricciones sobre el lugar en el que puede aparecer una instrucción `yield`, como se describe a continuación.

*  Es un error en tiempo de compilación que una instrucción `yield` (de cualquier forma) aparezca fuera de una *method_body*, *operator_body* o *accessor_body*
*  Es un error en tiempo de compilación que una instrucción `yield` (de cualquier forma) aparezca dentro de una función anónima.
*  Es un error en tiempo de compilación que una instrucción `yield` (de cualquier forma) aparezca en la cláusula `finally` de una instrucción `try`.
*  Es un error en tiempo de compilación que una instrucción `yield return` aparezca en cualquier parte de una instrucción `try` que contenga cualquier cláusula `catch`.

En el ejemplo siguiente se muestran algunos usos válidos y no válidos de `yield` instrucciones.

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

Debe existir una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo de la expresión en la instrucción `yield return` al tipo yield ([yield Type](classes.md#yield-type)) del iterador.

Una instrucción `yield return` se ejecuta de la siguiente manera:

*  La expresión dada en la instrucción se evalúa, se convierte implícitamente al tipo Yield y se asigna a la propiedad `Current` del objeto de enumerador.
*  La ejecución del bloque de iterador está suspendida. Si la instrucción `yield return` está en uno o varios bloques de `try`, los bloques de `finally` asociados no se ejecutan en este momento.
*  El método `MoveNext` del objeto de enumerador devuelve `true` a su llamador, lo que indica que el objeto de enumerador avanzó correctamente al siguiente elemento.

La siguiente llamada al método `MoveNext` del objeto de enumerador reanuda la ejecución del bloque de iterador desde donde se suspendió por última vez.

Una instrucción `yield break` se ejecuta de la siguiente manera:

*  Si el `yield break` instrucción está incluido en uno o varios bloques de `try` con bloques de `finally` asociados, el control se transfiere inicialmente al bloque de `finally` de la instrucción de `try` más interna. Cuando el control alcanza el punto final de un bloque `finally`, el control se transfiere al bloque `finally` de la siguiente instrucción `try` envolvente. Este proceso se repite hasta que se hayan ejecutado los bloques de `finally` de todas las instrucciones de `try` envolventes.
*  El control se devuelve al llamador del bloque de iteradores. Este es el método `MoveNext` o `Dispose` método del objeto enumerador.

Dado que una instrucción `yield break` transfiere incondicionalmente el control en otro lugar, nunca se puede obtener acceso al punto final de una instrucción de `yield break`.
