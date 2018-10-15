# <a name="statements"></a>Instrucciones

C# proporciona una serie de instrucciones. La mayoría de estas instrucciones le resultarán familiar a los desarrolladores que se han programado en C y C++.

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

El *embedded_statement* no terminal se usa para las instrucciones que aparecen dentro de otras instrucciones. El uso de *embedded_statement* lugar *instrucción* excluye el uso de instrucciones de declaración e instrucciones con etiquetas en estos contextos. El ejemplo
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
se produce un error en tiempo de compilación porque un `if` instrucción requiere un *embedded_statement* en lugar de un *instrucción* para Sí si rama. Si se permite este código, a continuación, la variable `i` se declararía, pero nunca se puede usar. Sin embargo, tenga en cuenta que al colocar `i`de declaración de un bloque, el ejemplo es válido.

## <a name="end-points-and-reachability"></a>Puntos finales y alcance

Cada instrucción tiene un ***punto final***. En términos intuitivos, el punto final de una instrucción es la ubicación que sigue inmediatamente a la instrucción. Las reglas de ejecución de instrucciones compuestas (instrucciones que contienen instrucciones incrustadas) especifican la acción que se realiza cuando el control alcanza el punto final de una instrucción incrustada. Por ejemplo, cuando el control alcanza el punto final de una instrucción en un bloque, el control se transfiere a la siguiente instrucción del bloque.

Si una instrucción, posiblemente, se puede acceder mediante la ejecución, la instrucción se dice que ***accesible***. Por el contrario, si no hay ninguna posibilidad de que se ejecuta una instrucción, la instrucción se dice que ***inaccesible***.

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
la segunda invocación de `Console.WriteLine` no es accesible porque no hay ninguna posibilidad de que se ejecutará la instrucción.

Se notifica una advertencia si el compilador determina que una instrucción es inaccesible. Es específicamente no es un error de una instrucción sea inaccesible.

Para determinar que si una determinada instrucción o el punto de conexión está accesible, el compilador realiza análisis de flujo según las reglas de disponibilidad definidas para cada instrucción. El análisis de flujo tiene en cuenta los valores de las expresiones constantes ([expresiones constantes](expressions.md#constant-expressions)) que controlan el comportamiento de instrucciones, pero no se consideran los posibles valores de expresiones no constantes. En otras palabras, para fines de análisis de flujo de control, una expresión no constante de un tipo determinado se considera que tiene cualquier valor posible de ese tipo.

En el ejemplo
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
la expresión booleana de la `if` instrucción es una expresión constante porque ambos operandos de la `==` operador son constantes. La expresión constante se evalúa en tiempo de compilación, que genera el valor `false`, el `Console.WriteLine` invocación se considera inaccesible. Sin embargo, si `i` se cambia para ser una variable local
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
el `Console.WriteLine` llamada se considera alcanzable, aunque en realidad, nunca se ejecutará.

El *bloque* de una función miembro siempre se considera alcanzable. Evaluando sucesivamente las reglas de disponibilidad de cada instrucción en un bloque, se puede determinar el alcance de cualquier instrucción determinada.

En el ejemplo
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
la accesibilidad de la segunda `Console.WriteLine` se determina como sigue:

*  La primera `Console.WriteLine` instrucción de expresión es accesible porque el bloque de la `F` método es accesible.
*  El punto final de la primera `Console.WriteLine` instrucción de expresión es accesible porque esa instrucción es alcanzable.
*  El `if` instrucción es alcanzable porque el punto final de la primera `Console.WriteLine` expresión instrucción es alcanzable.
*  El segundo `Console.WriteLine` instrucción de expresión es accesible porque la expresión booleana de la `if` instrucción no tiene el valor constante `false`.

Hay dos situaciones en las que es un error en tiempo de compilación para el punto final de una instrucción sea accesible:

*  Dado que el `switch` instrucción no permite una sección switch "pasen" a la siguiente sección, es un error en tiempo de compilación para el punto final de la lista de instrucciones de una sección switch sea alcanzable. Si se produce este error, normalmente es una indicación de que un `break` falta la instrucción.
*  Es un error en tiempo de compilación para el punto final del bloque de un miembro de función que calcula un valor para que sea accesible. Si se produce este error, normalmente es un valor que indica que un `return` falta la instrucción.

## <a name="blocks"></a>Bloques

Un *bloque* permite que se escriban varias instrucciones en contextos donde se permite una única instrucción.

```antlr
block
    : '{' statement_list? '}'
    ;
```

Un *bloque* consta de un elemento opcional *statement_list* ([instrucción enumera](statements.md#statement-lists)), encerradas entre llaves. Si se omite la lista de instrucciones, se dice que el bloque de estar vacío.

Un bloque puede contener instrucciones de declaración ([las instrucciones de declaración](statements.md#declaration-statements)). El ámbito de una variable o constante local declarada en un bloque es el bloque.

Un bloque se ejecuta como sigue:

*  Si el bloque está vacío, el control se transfiere al punto final del bloque.
*  Si el bloque no está vacío, el control se transfiere a la lista de instrucciones. Cuando el control alcanza el punto final de la lista de instrucciones, se transfiere al punto final del bloque.

La lista de instrucciones de un bloque es alcanzable si el propio bloque es alcanzable.

El punto final de un bloque es accesible si el bloque está vacío o si el punto final de la lista de instrucciones es alcanzable.

Un *bloque* que contiene uno o varios `yield` instrucciones ([la instrucción yield](statements.md#the-yield-statement)) se llama a un bloque de iteradores. Bloques del iterador se usan para implementar miembros de función como iteradores ([iteradores](classes.md#iterators)). Algunas restricciones adicionales se aplican a los bloques del iterador:

*  Es un error en tiempo de compilación para un `return` instrucción que aparezca en un bloque de iteradores (pero `yield return` se permiten instrucciones).
*  Es un error en tiempo de compilación para un bloque de iteradores para que contenga un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)). Un bloque de iteradores siempre define un contexto seguro, incluso cuando su declaración está anidada en un contexto no seguro.

### <a name="statement-lists"></a>Listas de instrucciones

Un ***lista de instrucciones*** consta de uno o más instrucciones escritas en secuencia. Las listas de instrucciones que se producen en *bloque*s ([bloques](statements.md#blocks)) y en *switch_block*s ([la instrucción switch](statements.md#the-switch-statement)).

```antlr
statement_list
    : statement+
    ;
```

Transfiere el control a la primera instrucción ejecuta una lista de instrucciones. Cuando el control alcanza el punto final de una instrucción, se transfiere a la siguiente instrucción. Cuando el control alcanza el punto final de la última instrucción, se transfiere al punto final de la lista de instrucciones.

Una instrucción en una lista de instrucciones es alcanzable si se cumple al menos uno de los siguientes:

*  La instrucción es la primera instrucción y la propia lista de la instrucción es alcanzable.
*  El punto final de la instrucción anterior es alcanzable.
*  La instrucción es una instrucción con etiqueta y la etiqueta se hace referencia mediante un accesible `goto` instrucción.

El punto final de una lista de instrucciones es accesible si el punto final de la última instrucción en la lista es alcanzable.

## <a name="the-empty-statement"></a>Instrucción vacía

Un *empty_statement* no hace nada.

```antlr
empty_statement
    : ';'
    ;
```

Cuando hay ninguna operación para realizar en un contexto donde se requiere una instrucción, se usa una instrucción vacía.

Ejecución de una instrucción vacía simplemente transfiere el control al punto final de la instrucción. Por lo tanto, el punto final de una instrucción vacía es alcanzable si la instrucción vacía es alcanzable.

Se puede usar una instrucción vacía al escribir un `while` instrucción con un cuerpo null:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Además, se puede usar una instrucción vacía para declarar una etiqueta justo antes de cerrar "`}`" de un bloque:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Instrucciones con etiqueta

Un *labeled_statement* permite ir precedida por una etiqueta de una instrucción. Labeled (instrucciones) se permiten en bloques, pero no están permitidas como instrucciones incrustadas.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Una instrucción con etiqueta declara una etiqueta con el nombre proporcionado por el *identificador*. El ámbito de una etiqueta es el bloque completo en el que se declara, incluyendo los bloques anidados. Es un error en tiempo de compilación para las dos etiquetas con el mismo nombre para tener ámbitos que se superponen.

Se puede hacer referencia a una etiqueta de `goto` instrucciones ([la instrucción goto](statements.md#the-goto-statement)) dentro del ámbito de la etiqueta. Esto significa que `goto` instrucciones pueden transferir el control dentro y fuera de los bloques, pero nunca en bloques.

Las etiquetas tienen su propio espacio de declaración y no interfieren con otros identificadores. El ejemplo
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
es válido y utiliza el nombre `x` como un parámetro y una etiqueta.

Ejecución de una instrucción con etiqueta corresponde exactamente con la ejecución de la instrucción siguiente a la etiqueta.

Además de la disponibilidad proporcionado por el flujo de control normal, una instrucción con etiqueta estará accesible si se hace referencia a la etiqueta por un accesible `goto` instrucción. (Excepción: si un `goto` instrucción está dentro de un `try` que incluye un `finally` bloque y la instrucción con etiqueta está fuera de la `try`y el punto final de la `finally` bloque es inaccesible, a continuación, la instrucción con etiqueta no es accesible desde el que `goto` instrucción.)

## <a name="declaration-statements"></a>Instrucciones de declaración

Un *declaration_statement* declara una variable local o una constante. Las instrucciones de declaración se permiten en bloques, pero no están permitidas como instrucciones incrustadas.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Declaraciones de variable locales

Un *local_variable_declaration* declara uno o más variables locales.

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

El *local_variable_type* de un *local_variable_declaration* directa especifica el tipo de las variables que aparecen en la declaración, o indica con el identificador `var` que el tipo debe deducirse basándose en un inicializador. El tipo es seguido por una lista de *local_variable_declarator*s, cada uno de los cuales incluye una nueva variable. Un *local_variable_declarator* consta de un *identificador* que los nombres de la variable, seguida opcionalmente por una "`=`" token y un *local_variable_initializer* que proporciona el valor inicial de la variable.

En el contexto de una declaración de variable local, el identificador var actúa como una palabra clave contextual ([palabras clave](lexical-structure.md#keywords)). Cuando el *local_variable_type* se especifica como `var` y ningún tipo denominado `var` está en el ámbito, la declaración es un ***declaración de variable local con tipo implícito***, cuyo tipo es deduce el tipo de la expresión de inicializador asociado. Las declaraciones de variable locales tipificadas implícitamente son las siguientes restricciones:

*  El *local_variable_declaration* no se puede incluir varios *local_variable_declarator*s.
*  El *local_variable_declarator* debe incluir un *local_variable_initializer*.
*  El *local_variable_initializer* debe ser un *expresión*.
*  El inicializador *expresión* debe tener un tipo de tiempo de compilación.
*  El inicializador *expresión* no puede hacer referencia a la variable declarada en el propio

Los siguientes son ejemplos de declaraciones de variable local tipificadas implícitamente incorrectos:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

Se obtiene el valor de una variable local en una expresión que utiliza un *simple_name* ([nombres simples](expressions.md#simple-names)), y se modifica el valor de una variable local con un *asignación* () [Operadores de asignación](expressions.md#assignment-operators)). Una variable local debe estar previamente asignada ([asignación definitiva](variables.md#definite-assignment)) en cada ubicación donde se obtiene su valor.

El ámbito de una variable local declarada en un *local_variable_declaration* es el bloque en que se produce la declaración. Es un error para hacer referencia a una variable local en una posición textual que precede a la *local_variable_declarator* de la variable local. Dentro del ámbito de una variable local, es un error en tiempo de compilación para declarar otra variable o constante con el mismo nombre local.

Una declaración de variable local que declara varias variables equivale a varias declaraciones de variables únicas con el mismo tipo. Además, un inicializador de variable en una declaración de variable local se corresponde exactamente a una instrucción de asignación que se inserta inmediatamente después de la declaración.

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

En una declaración de variable local tipificada implícitamente, se toma el tipo de la variable local que se declara para ser el mismo que el tipo de la expresión utilizada para inicializar la variable. Por ejemplo:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

El implícito declaraciones de variable local anteriores son exactamente equivalentes a las siguientes declaraciones con tipo explícito:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Declaraciones de constante local

Un *local_constant_declaration* declara uno o más constantes locales.

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

El *tipo* de un *local_constant_declaration* especifica el tipo de las constantes que se incluyen en la declaración. El tipo es seguido por una lista de *constant_declarator*s, cada uno de los cuales incluye una nueva constante. Un *constant_declarator* consta de un *identificador* que nombres de la constante, seguido por un "`=`" símbolo (token), seguido de un *constant_expression* ([ Expresiones constantes](expressions.md#constant-expressions)) que proporciona el valor de la constante.

El *tipo* y *constant_expression* de una declaración de constante local deben seguir las mismas reglas que los de una declaración de miembro de constante ([constantes](classes.md#constants)).

Se obtiene el valor de una constante local en una expresión que utiliza un *simple_name* ([nombres simples](expressions.md#simple-names)).

El ámbito de una constante local es el bloque en que se produce la declaración. Es un error para hacer referencia a una constante local en una posición textual que precede a su *constant_declarator*. Dentro del ámbito de una constante local, es un error en tiempo de compilación para declarar otra variable o constante con el mismo nombre local.

Una declaración de constante local que declara varias constantes equivale a varias declaraciones de una sola constante con el mismo tipo.

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

No todas las expresiones pueden ser instrucciones. En concreto, expresiones como `x + y` y `x == 1` que simplemente calcular un valor (que se descartarán), no se permiten que las instrucciones.

Ejecución de un *expression_statement* evalúa la expresión contenida y, a continuación, transfiere el control al punto final de la *expression_statement*. El punto final de un *expression_statement* estará accesible si ese *expression_statement* es accesible.

## <a name="selection-statements"></a>Instrucciones de selección

Instrucciones de selección seleccione uno de una serie de instrucciones posibles para la ejecución en función del valor de alguna expresión.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If instrucción

El `if` instrucción selecciona una instrucción para ejecutarla en función del valor de una expresión booleana.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Un `else` parte está asociada con la instrucción anterior más cercana `if` permitido por la sintaxis. Por lo tanto, un `if` instrucción del formulario
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

Un `if` instrucción se ejecuta como sigue:

*  El *boolean_expression* ([expresiones booleanas](expressions.md#boolean-expressions)) se evalúa.
*  Si la expresión booleana devuelve `true`, el control se transfiere a la primera instrucción incrustada. Cuando el control alcanza el punto final de dicha instrucción, se transfiere al punto final de la `if` instrucción.
*  Si la expresión booleana devuelve `false` y si un `else` parte está presente, el control se transfiere a la segunda instrucción incrustada. Cuando el control alcanza el punto final de dicha instrucción, se transfiere al punto final de la `if` instrucción.
*  Si la expresión booleana devuelve `false` y si un `else` parte no está presente, el control se transfiere al punto final de la `if` instrucción.

La primera instrucción de incrustada una `if` instrucción es alcanzable si el `if` instrucción es alcanzable y la expresión booleana no tiene el valor constante `false`.

La segunda instrucción de incrustada una `if` instrucción, si está presente, es accesible si el `if` instrucción es alcanzable y la expresión booleana no tiene el valor constante `true`.

El punto final de un `if` instrucción es alcanzable si el punto final de al menos uno de sus instrucciones incrustadas es alcanzable. Además, el punto final de un `if` instrucción sin ningún `else` parte es accesible si el `if` instrucción es alcanzable y la expresión booleana no tiene el valor constante `true`.

### <a name="the-switch-statement"></a>La instrucción switch

La instrucción switch, selecciona para la ejecución de una lista de instrucciones con una etiqueta de conmutador asociado que se corresponde con el valor de la expresión switch.

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

Un *switch_statement* consta de la palabra clave `switch`, seguido de una expresión entre paréntesis (denominada la expresión switch), seguido por un *switch_block*. El *switch_block* consta de cero o más *switch_section*, encerradas entre llaves. Cada *switch_section* consta de uno o varios *switch_label*s seguido por un *statement_list* ([instrucción enumera](statements.md#statement-lists)).

El ***que rigen tipo*** de un `switch` instrucción se establece mediante la expresión switch.

*  Si el tipo de la expresión switch es `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, o un *enum_type*, o si es el tipo que acepta valores NULL correspondiente a uno de estos tipos, que es el que rigen el tipo de la `switch` instrucción.
*  En caso contrario, exactamente uno definido por el usuario conversión implícita ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)) debe existir en el tipo de la expresión switch a uno de los posibles tipos aplicables: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, o bien, un tipo que acepta valores NULL correspondiente a uno de esos tipos.
*  En caso contrario, si no existe una conversión implícita, o si existe más de una conversión implícita, se produce un error en tiempo de compilación.

La expresión constante de cada `case` etiqueta debe indicar un valor que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo aplicable el `switch` instrucción. Se produce un error de tiempo de compilación si dos o más `case` etiquetas en el mismo `switch` instrucción especificar el mismo valor constante.

Puede haber a lo sumo un `default` etiqueta en una instrucción switch.

Un `switch` instrucción se ejecuta como sigue:

*  La expresión switch se evalúa y se convierte al tipo aplicable.
*  Si no especifica una de las constantes en un `case` etiqueta en la misma `switch` instrucción es igual al valor de la expresión switch, el control se transfiere a la lista de la instrucción siguiente correspondiente `case` etiqueta.
*  Si ninguna de las constantes especificadas en `case` etiquetas en el mismo `switch` instrucción es igual al valor de la expresión switch y si un `default` etiqueta está presente, el control se transfiere a la instrucción que sigue lista la `default` etiqueta.
*  Si ninguna de las constantes especificadas en `case` etiquetas en el mismo `switch` instrucción es igual al valor de la expresión switch y si no hay ningún `default` etiqueta está presente, el control se transfiere al punto final de la `switch` instrucción.

Si el punto final de la lista de instrucciones de una sección switch es accesible, se produce un error en tiempo de compilación. Esto se conoce como la regla "sin paso explícito". El ejemplo
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
es válido porque no hay ninguna sección switch tiene un punto de conexión alcanzable. A diferencia de C y C++, la ejecución de una sección switch no permite el "paso explícito" a la siguiente sección switch y el ejemplo
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
genera un error de tiempo de compilación. Cuando la ejecución de una sección switch es seguido de ejecución de otra sección switch, explícita `goto case` o `goto default` se debe usar la instrucción:
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
es válido. El ejemplo no infringir la regla "sin paso explícito" porque las etiquetas `case 2:` y `default:` forman parte del mismo *switch_section*.

La regla "sin paso explícito" evita que una clase común de errores que se producen en C y C++ cuando `break` accidentalmente se omiten las instrucciones. Además, debido a esta regla, las secciones switch de una `switch` instrucción se puede reorganizar arbitrariamente sin afectar al comportamiento de la instrucción. Por ejemplo, las secciones de la `switch` se puede invertir la instrucción anterior sin afectar al comportamiento de la instrucción:
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

La lista de instrucciones de una sección switch normalmente termina en un `break`, `goto case`, o `goto default` se permite la instrucción, pero ninguna construcción que representa el punto final de la lista de instrucciones inaccesible. Por ejemplo, un `while` instrucción controlada por la expresión booleana `true` se sabe que nunca alcance su punto de conexión. Del mismo modo, un `throw` o `return` instrucción siempre transfiere el control en otro lugar y nunca llega a su punto de conexión. Por lo tanto, el ejemplo siguiente es válido:
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

El tipo aplicable una `switch` instrucción puede ser del tipo `string`. Por ejemplo:
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

Al igual que los operadores de igualdad de cadenas ([operadores de igualdad de cadenas](expressions.md#string-equality-operators)), el `switch` instrucción distingue mayúsculas de minúsculas y ejecutará una determinada sección sólo si la cadena de expresión switch coincide exactamente con un `case` etiqueta constante.

Cuando el control de tipo de un `switch` instrucción es `string`, el valor `null` se permite como una constante de etiqueta case.

El *statement_list*s de una *switch_block* pueden contener instrucciones de declaración ([las instrucciones de declaración](statements.md#declaration-statements)). El ámbito de una variable o constante local declarada en un bloque switch es el bloque switch.

La lista de instrucciones de una sección switch determinada es accesible si el `switch` instrucción es alcanzable y al menos uno de los siguientes es true:

*  La expresión switch es un valor no constante.
*  La expresión switch es un valor constante que coincide con un `case` etiqueta en la sección switch.
*  La expresión switch es un valor constante que no coincide con ninguna `case` etiqueta y la sección switch contiene la `default` etiqueta.
*  Se hace referencia a una etiqueta de switch de la sección switch mediante una accesible `goto case` o `goto default` instrucción.

El punto final de un `switch` instrucción es alcanzable si se cumple al menos uno de los siguientes:

*  El `switch` instrucción contiene una accesible `break` instrucción que se cierra el `switch` instrucción.
*  El `switch` instrucción es alcanzable, la expresión switch es un valor no constante y no `default` etiqueta está presente.
*  El `switch` instrucción es alcanzable, la expresión switch es un valor constante que no coincide con ninguna `case` etiqueta y ningún `default` etiqueta está presente.

## <a name="iteration-statements"></a>Instrucciones de iteración

Instrucciones de iteración repetidamente ejecutan una instrucción incrustada.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>La instrucción while

El `while` instrucción condicionalmente ejecuta una instrucción incrustada cero o más veces.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

Un `while` instrucción se ejecuta como sigue:

*  El *boolean_expression* ([expresiones booleanas](expressions.md#boolean-expressions)) se evalúa.
*  Si la expresión booleana devuelve `true`, el control se transfiere a la instrucción incrustada. Cuando y si el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de un `continue` instrucción), el control se transfiere al principio de la `while` instrucción.
*  Si la expresión booleana devuelve `false`, el control se transfiere al punto final de la `while` instrucción.

Dentro de la instrucción incrustada de un `while` (instrucción), un `break` instrucción ([la instrucción break](statements.md#the-break-statement)) puede utilizarse para transferir el control al punto final de la `while` instrucción (terminando así la iteración de los datos incrustados instrucción) y un `continue` instrucción ([la instrucción continue](statements.md#the-continue-statement)) puede utilizarse para transferir el control al punto final de la instrucción incrustada (otra iteración de esta forma se realizará la `while` instrucción).

La instrucción incrustada de un `while` instrucción es alcanzable si el `while` instrucción es alcanzable y la expresión booleana no tiene el valor constante `false`.

El punto final de un `while` instrucción es alcanzable si se cumple al menos uno de los siguientes:

*  El `while` instrucción contiene una accesible `break` instrucción que se cierra el `while` instrucción.
*  El `while` instrucción es alcanzable y la expresión booleana no tiene el valor constante `true`.

### <a name="the-do-statement"></a>La instrucción do

El `do` instrucción condicionalmente ejecuta una instrucción incrustada una o varias veces.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

Un `do` instrucción se ejecuta como sigue:

*  El control se transfiere a la instrucción incrustada.
*  Cuando y si el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de un `continue` instrucción), el *boolean_expression* ([expresiones booleanas](expressions.md#boolean-expressions)) se evalúa. Si la expresión booleana devuelve `true`, el control se transfiere al principio de la `do` instrucción. En caso contrario, el control se transfiere al punto final de la `do` instrucción.

Dentro de la instrucción incrustada de un `do` (instrucción), un `break` instrucción ([la instrucción break](statements.md#the-break-statement)) puede utilizarse para transferir el control al punto final de la `do` instrucción (terminando así la iteración de los datos incrustados instrucción) y un `continue` instrucción ([la instrucción continue](statements.md#the-continue-statement)) puede utilizarse para transferir el control al punto final de la instrucción incrustada.

La instrucción incrustada de un `do` instrucción es alcanzable si el `do` instrucción es alcanzable.

El punto final de un `do` instrucción es alcanzable si se cumple al menos uno de los siguientes:

*  El `do` instrucción contiene una accesible `break` instrucción que se cierra el `do` instrucción.
*  El punto final de la instrucción incrustada es alcanzable y la expresión booleana no tiene el valor constante `true`.

### <a name="the-for-statement"></a>La instrucción for

El `for` instrucción se evalúa como una secuencia de expresiones de inicialización y, a continuación, mientras una condición es true, repetidamente ejecuta una instrucción incrustada y se evalúa como una secuencia de expresiones de iteración.

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

El *for_initializer*, si está presente, consta de un *local_variable_declaration* ([declaraciones de variable Local](statements.md#local-variable-declarations)) o una lista de *statement_ expresión*s ([las instrucciones de expresión](statements.md#expression-statements)) separados por comas. El ámbito de una variable local declarada por un *for_initializer* comienza en el *local_variable_declarator* para la variable y se extiende hasta el final de la instrucción incrustada. El ámbito incluye la *for_condition* y *for_iterator*.

El *for_condition*, si está presente, debe ser un *boolean_expression* ([expresiones booleanas](expressions.md#boolean-expressions)).

El *for_iterator*, si está presente, consta de una lista de *statement_expression*s ([las instrucciones de expresión](statements.md#expression-statements)) separados por comas.

Una instrucción for se ejecuta como sigue:

*  Si un *for_initializer* existe, los inicializadores de variables o expresiones de instrucción se ejecutan en el orden en que se escriben. Este paso se realiza solo una vez.
*  Si un *for_condition* está presente, se evalúa.
*  Si el *for_condition* no está presente o si se produce la evaluación `true`, el control se transfiere a la instrucción incrustada. Cuando y si el control alcanza el punto final de la instrucción incrustada (posiblemente desde la ejecución de un `continue` instrucción), las expresiones de la *for_iterator*, si existe, se evalúa en secuencia y, a continuación, otra iteración es realiza empezando por la evaluación de la *for_condition* en el paso anterior.
*  Si el *for_condition* está presente, la evaluación produce `false`, el control se transfiere al punto final de la `for` instrucción.

Dentro de la instrucción incrustada de un `for` (instrucción), un `break` instrucción ([la instrucción break](statements.md#the-break-statement)) puede utilizarse para transferir el control al punto final de la `for` instrucción (terminando así la iteración de los datos incrustados instrucción) y un `continue` instrucción ([la instrucción continue](statements.md#the-continue-statement)) puede utilizarse para transferir el control al punto final de la instrucción incrustada (ejecutándolos el *for_iterator* y realizar otra iteración de la `for` instrucción, empezando por el *for_condition*).

La instrucción incrustada de un `for` instrucción es alcanzable si se cumple una de las siguientes acciones:

*  El `for` instrucción es alcanzable y no *for_condition* está presente.
*  El `for` instrucción es alcanzable y un *for_condition* está presente y no tiene el valor constante `false`.

El punto final de un `for` instrucción es alcanzable si se cumple al menos uno de los siguientes:

*  El `for` instrucción contiene una accesible `break` instrucción que se cierra el `for` instrucción.
*  El `for` instrucción es alcanzable y un *for_condition* está presente y no tiene el valor constante `true`.

### <a name="the-foreach-statement"></a>La instrucción foreach

El `foreach` instrucción enumera los elementos de una colección y ejecuta una instrucción incrustada para cada elemento de la colección.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

El *tipo* y *identificador* de un `foreach` instrucción declara el ***variable de iteración*** de la instrucción. Si el `var` identificador se expresa como el *local_variable_type*y ningún tipo denominado `var` está en el ámbito, la variable de iteración se dice que un ***variable de iteración implícito***, y su tipo se considera que es el tipo de elemento de la `foreach` instrucción, como se indica a continuación. La variable de iteración corresponde a una variable local de solo lectura con un ámbito que se extiende a través de la instrucción incrustada. Durante la ejecución de un `foreach` instrucción, la variable de iteración representa el elemento de colección para el que se está realizando una iteración. Se produce un error de tiempo de compilación si la instrucción incrustada intenta modificar la variable de iteración (a través de la asignación o el `++` y `--` operadores) o pasar la variable de iteración como un `ref` o `out` parámetro.

A continuación, para mayor brevedad, `IEnumerable`, `IEnumerator`, `IEnumerable<T>` y `IEnumerator<T>` hacen referencia a los tipos correspondientes en los espacios de nombres `System.Collections` y `System.Collections.Generic`.

El procesamiento en tiempo de compilación de una instrucción foreach se determina en primer lugar el ***tipo de colección***, ***tipo de enumerador*** y ***tipo de elemento*** de la expresión. Esta determinación se realiza como sigue:

*  Si el tipo `X` de *expresión* es un tipo de matriz, no hay una conversión implícita de referencia de `X` a la `IEnumerable` interfaz (puesto que `System.Array` implementa esta interfaz). El ***tipo de colección*** es el `IEnumerable` interfaz, el ***tipo de enumerador*** es el `IEnumerator` interfaz y la ***tipo de elemento*** es el tipo de elemento de la tipo de matriz `X`.
*  Si el tipo `X` de *expresión* es `dynamic` , a continuación, hay una conversión implícita de *expresión* a la `IEnumerable` interfaz ([dinámicos implícita las conversiones](conversions.md#implicit-dynamic-conversions)). El ***tipo de colección*** es el `IEnumerable` interfaz y la ***tipo de enumerador*** es el `IEnumerator` interfaz. Si el `var` identificador se expresa como el *local_variable_type* el ***tipo de elemento*** es `dynamic`, de lo contrario es `object`.
*  En caso contrario, determinar si el tipo `X` tiene un adecuado `GetEnumerator` método:
   * Realizar la búsqueda de miembros en el tipo `X` con el identificador de `GetEnumerator` y sin argumentos de tipo. Como se describe a continuación, compruebe si la búsqueda de miembros no produce una coincidencia, o que produce una ambigüedad, o produzca a una coincidencia que no es un grupo de métodos, para una interfaz enumerable. Se recomienda que se emite una advertencia si la búsqueda de miembros genera nada excepto un grupo de métodos o ninguna coincidencia.
   * Realizar la resolución de sobrecarga con el grupo de métodos resultantes y una lista de argumentos vacía. Si la resolución de sobrecarga tiene como resultado ningún método aplicable, da como resultado una ambigüedad o da como resultado un solo método mejor, pero ese método es estático o no público, busque una interfaz enumerable, como se describe a continuación. Se recomienda que se emite una advertencia si la resolución de sobrecarga genera nada excepto un método de instancia pública ambiguo o no hay métodos aplicables.
   * Si el tipo de devolución `E` de la `GetEnumerator` método no es una clase, struct o tipo de interfaz, un error se genera y no se toma ningún paso adicional.
   * Se realiza la búsqueda de miembros en `E` con el identificador `Current` y sin argumentos de tipo. Si la búsqueda de miembros no produce a ninguna coincidencia, el resultado es un error o el resultado es salvo para una propiedad de instancia pública que permite lectura, se produce un error y no se toma ningún paso adicional.
   * Se realiza la búsqueda de miembros en `E` con el identificador `MoveNext` y sin argumentos de tipo. Si la búsqueda de miembros no produce a ninguna coincidencia, el resultado es un error o el resultado es salvo para un grupo de métodos, se produce un error y no se toma ningún paso adicional.
   * Se realiza la resolución de sobrecarga en el grupo de métodos con una lista de argumentos vacía. Si los resultados de la resolución de sobrecarga en ningún métodos aplicables, los resultados en una ambigüedad o los resultados en un único método mejor, pero ese método es estático o no público, o su tipo de valor devuelto no es `bool`, se genera un error y no se toma ningún paso adicional.
   * El ***tipo de colección*** es `X`, el ***tipo de enumerador*** es `E`y el ***tipo de elemento*** es el tipo de la `Current` propiedad.

*  En caso contrario, busque una interfaz enumerable:
   * If entre todos los tipos de `Ti` para que no hay una conversión implícita de `X` a `IEnumerable<Ti>`, hay un único tipo `T` que `T` no `dynamic` y para todos los demás `Ti` hay una conversión implícita de `IEnumerable<T>` a `IEnumerable<Ti>`, el ***tipo de colección*** es la interfaz `IEnumerable<T>`, ***tipo de enumerador*** es la interfaz `IEnumerator<T>`y el ***tipo de elemento*** es `T`.
   * En caso contrario, si hay más de uno de estos tipos `T`, a continuación, se produce un error y no se toma ningún paso adicional.
   * En caso contrario, si hay una conversión implícita de `X` a la `System.Collections.IEnumerable` interfaz, el ***tipo de colección*** es esta interfaz, el ***tipo de enumerador*** es la interfaz `System.Collections.IEnumerator`y el ***tipo de elemento*** es `object`.
   * En caso contrario, se produce un error y no se toma ningún paso adicional.

Los pasos anteriores, si se realiza correctamente, sin ambigüedades generan un tipo de colección `C`, tipo de enumerador `E` y tipo de elemento `T`. Una instrucción foreach del formulario
```csharp
foreach (V v in x) embedded_statement
```
a continuación, se expande en:
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

La variable `e` no es visible o accesible a la expresión `x` o la instrucción incrustada o cualquier otro código fuente del programa. La variable `v` es de solo lectura en la instrucción incrustada. Si no hay una conversión explícita ([las conversiones explícitas](conversions.md#explicit-conversions)) desde `T` (el tipo de elemento) para `V` (el *local_variable_type* en la instrucción foreach), se produce un error y no se realiza ningún paso adicional. Si `x` tiene el valor `null`, un `System.NullReferenceException` se produce en tiempo de ejecución.

Se permite una implementación para implementar una determinada instrucción foreach de manera diferente, por ejemplo, por motivos de rendimiento, siempre y cuando el comportamiento es coherente con la expansión de la anterior.

La colocación de `v` dentro del tiempo es importante para cómo se capturan por cualquier función anónima que se producen en bucle la *embedded_statement*.

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
Si `v` se ha declarado fuera while bucle, se pueden compartir entre todas las iteraciones y su valor después de la para bucle sería el valor final, `13`, que es lo que la invocación de `f` imprimía. En su lugar, porque cada iteración tiene su propia variable `v`, la captura `f` en la primera iteración continuará que contenga el valor `7`, que es lo que se van a imprimir. (Nota: las versiones anteriores de C# declaran `v` bucle exterior de while.)

El cuerpo de la, por último, se construye el bloque según los pasos siguientes:

*  Si no hay una conversión implícita de `E` a la `System.IDisposable` interfaz, a continuación,
   *  Si `E` es un tipo de valor distinto de null, la finalmente cláusula se expande en el equivalente semántico de:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  De lo contrario el finalmente cláusula se expande en el equivalente semántico de:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   salvo que si `E` es un tipo de valor o un parámetro de tipo que se crea una instancia de un tipo de valor y, después, la conversión de `e` a `System.IDisposable` no hará que la conversión boxing que se produzca.

*  De lo contrario, si `E` es un tipo sellado, la, por último, se expande la cláusula en un bloque vacío:

   ```csharp
   finally {
   }
   ```

*  En caso contrario, el, por último, se expande la cláusula para:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   La variable local `d` no es visible o accesible a cualquier código de usuario. En concreto, no está en conflicto con cualquier otra variable cuyo ámbito incluye el bloque finally.

El orden en que `foreach` recorre los elementos de una matriz, es el siguiente: para los elementos de matrices unidimensionales se recorren en orden de índice ascendente, empezando por el índice `0` y terminando con el índice `Length - 1`. Las matrices multidimensionales, se recorren los elementos que tanto los índices de la dimensión más a la derecha se incrementan en primer lugar, a continuación, en la siguiente dimensión izquierda, y así sucesivamente hacia la izquierda.

El ejemplo siguiente se imprime cada valor en una matriz bidimensional, en orden de los elementos:
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
El resultado es como sigue:
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

En el ejemplo
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
el tipo de `n` se infiere para ser `int`, el tipo de elemento de `numbers`.

## <a name="jump-statements"></a>Instrucciones de salto

Instrucciones de salto incondicional transferir el control.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

Se llama a la ubicación a la que una instrucción de salto transfiere el control del ***destino*** de la instrucción de salto.

Cuando se produce una instrucción de salto dentro de un bloque y el destino de esa instrucción de salto está fuera de ese bloque, se dice que la instrucción de salto a ***salir*** el bloque. Mientras una instrucción de salto puede transferir el control fuera de un bloque, nunca puede transferir el control en un bloque.

Ejecución de instrucciones de salto se complica por la presencia de intermedia `try` instrucciones. En ausencia de estos `try` instrucciones, una instrucción de salto transfiere incondicionalmente el control de la instrucción de salto a su destino. En presencia de este tipo intermedia `try` instrucciones, la ejecución es más compleja. Si la instrucción de salto sale de uno o varios `try` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción. Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción. Este proceso se repite hasta que el `finally` bloques de todos los que intervienen `try` se han ejecutado las instrucciones.

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
el `finally` bloques asociados con dos `try` las instrucciones se ejecutan antes de que el control se transfiere al destino de la instrucción de salto.

El resultado es como sigue:
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Break (instrucción)

El `break` instrucción sale envolvente `switch`, `while`, `do`, `for`, o `foreach` instrucción.

```antlr
break_statement
    : 'break' ';'
    ;
```

El destino de un `break` instrucción es el punto final de envolvente `switch`, `while`, `do`, `for`, o `foreach` instrucción. Si un `break` instrucción no se incluye un `switch`, `while`, `do`, `for`, o `foreach` instrucción, que se produce un error en tiempo de compilación.

Cuando varios `switch`, `while`, `do`, `for`, o `foreach` instrucciones están anidadas dentro de otros, un `break` instrucción solo se aplica a la instrucción más interna. Para transferir el control a través de varios niveles de anidamiento, un `goto` instrucción ([la instrucción goto](statements.md#the-goto-statement)) se debe usar.

Un `break` instrucción no puede salir una `finally` bloque ([la instrucción try](statements.md#the-try-statement)). Cuando un `break` instrucción se produce en un `finally` bloquea, el destino de la `break` instrucción debe ser dentro del mismo `finally` bloquear; en caso contrario, se produce un error en tiempo de compilación.

Un `break` instrucción se ejecuta como sigue:

*  Si el `break` instrucción sale de uno o varios `try` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción. Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción. Este proceso se repite hasta que el `finally` bloques de todos los que intervienen `try` se han ejecutado las instrucciones.
*  El control se transfiere al destino de la `break` instrucción.

Dado que un `break` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `break` instrucción nunca es alcanzable.

### <a name="the-continue-statement"></a>La instrucción continue

El `continue` instrucción inicia una nueva iteración de envolvente `while`, `do`, `for`, o `foreach` instrucción.

```antlr
continue_statement
    : 'continue' ';'
    ;
```

El destino de un `continue` instrucción es el punto final de la instrucción incrustada de envolvente `while`, `do`, `for`, o `foreach` instrucción. Si un `continue` instrucción no se incluye un `while`, `do`, `for`, o `foreach` instrucción, que se produce un error en tiempo de compilación.

Cuando varios `while`, `do`, `for`, o `foreach` instrucciones están anidadas dentro de otros, un `continue` instrucción solo se aplica a la instrucción más interna. Para transferir el control a través de varios niveles de anidamiento, un `goto` instrucción ([la instrucción goto](statements.md#the-goto-statement)) se debe usar.

Un `continue` instrucción no puede salir una `finally` bloque ([la instrucción try](statements.md#the-try-statement)). Cuando un `continue` instrucción se produce en un `finally` bloquea, el destino de la `continue` instrucción debe ser dentro del mismo `finally` bloquear; de lo contrario se produce un error en tiempo de compilación.

Un `continue` instrucción se ejecuta como sigue:

*  Si el `continue` instrucción sale de uno o varios `try` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción. Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción. Este proceso se repite hasta que el `finally` bloques de todos los que intervienen `try` se han ejecutado las instrucciones.
*  El control se transfiere al destino de la `continue` instrucción.

Dado que un `continue` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `continue` instrucción nunca es alcanzable.

### <a name="the-goto-statement"></a>La instrucción goto

El `goto` instrucción transfiere el control a una instrucción que está marcada con una etiqueta.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

El destino de un `goto` *identificador* es la instrucción con etiqueta con la etiqueta especificada. Si una etiqueta con el nombre especificado no existe en el miembro de función actual, o si el `goto` instrucción no se está dentro del ámbito de la etiqueta, se produce un error de tiempo de compilación. Esta regla permite el uso de un `goto` instrucción para transferir el control fuera de un ámbito anidado, pero no en un ámbito anidado. En el ejemplo
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
un `goto` instrucción se usa para transferir el control fuera de un ámbito anidado.

El destino de un `goto case` instrucción es la lista de instrucciones en la envolvente inmediato `switch` instrucción ([la instrucción switch](statements.md#the-switch-statement)), que contiene un `case` etiqueta con el valor constante especificado. Si el `goto case` instrucción no se incluye un `switch` instrucción, si la *constant_expression* no es implícitamente convertible ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo aplicable de la más cercano envolvente `switch` instrucción, o si envolvente `switch` instrucción no contenga un `case` etiquetar con el valor constante especificado, se produce un error de tiempo de compilación.

El destino de un `goto default` instrucción es la lista de instrucciones en la envolvente inmediato `switch` instrucción ([la instrucción switch](statements.md#the-switch-statement)), que contiene un `default` etiqueta. Si el `goto default` instrucción no se incluye un `switch` instrucción, o si envolvente `switch` instrucción no contenga un `default` etiqueta, se produce un error de tiempo de compilación.

Un `goto` instrucción no puede salir una `finally` bloque ([la instrucción try](statements.md#the-try-statement)). Cuando un `goto` instrucción se produce en un `finally` bloquea, el destino de la `goto` instrucción debe ser dentro del mismo `finally` bloque, o en caso contrario, se produce un error en tiempo de compilación.

Un `goto` instrucción se ejecuta como sigue:

*  Si el `goto` instrucción sale de uno o varios `try` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción. Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción. Este proceso se repite hasta que el `finally` bloques de todos los que intervienen `try` se han ejecutado las instrucciones.
*  El control se transfiere al destino de la `goto` instrucción.

Dado que un `goto` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `goto` instrucción nunca es alcanzable.

### <a name="the-return-statement"></a>La instrucción return

El `return` instrucción devuelve el control al llamador actual de la función en el que el `return` aparece la instrucción.

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

Un `return` instrucción sin una expresión puede utilizarse únicamente en un miembro de función que no calcula un valor, es decir, un método con el tipo de resultado ([cuerpo del método](classes.md#method-body)) `void`, el `set` descriptor de acceso de una propiedad o indizador, el `add` y `remove` descriptores de acceso de un evento, un constructor de instancia, un constructor estático o un destructor.

Un `return` instrucción con una expresión solo se puede usar en un miembro de función que calcula un valor, es decir, un método con un tipo de resultado distinto de void el `get` descriptor de acceso de una propiedad o indizador o un operador definido por el usuario. Una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) debe existir en el tipo de la expresión para el tipo de valor devuelto del miembro de función que lo contiene.

Devolver instrucciones también se pueden utilizar en el cuerpo de expresiones de función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) y participar en determinar qué conversiones existen para esas funciones.

Es un error en tiempo de compilación para un `return` instrucción que aparezca en un `finally` bloque ([la instrucción try](statements.md#the-try-statement)).

Un `return` instrucción se ejecuta como sigue:

*  Si el `return` instrucción especifica una expresión, la expresión se evalúa y el valor resultante se convierte en el tipo de valor devuelto de la función que lo contiene mediante una conversión implícita. El resultado de la conversión se convierte en el valor del resultado producido por la función.
*  Si el `return` instrucción está dentro de uno o varios `try` o `catch` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción. Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción. Este proceso se repite hasta que el `finally` bloques de inclusión de todos los `try` se han ejecutado las instrucciones.
*  Si la función contenedora no es una función asincrónica, el control se devuelve al autor de llamada de la función contenedora junto con el valor del resultado, si existe.
*  Si la función contenedora es una función asincrónica, el control se devuelve al llamador actual y el valor del resultado, si los hay, se registra en la tarea devuelta como se describe en ([interfaces de enumerador](classes.md#enumerator-interfaces)).

Dado que un `return` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `return` instrucción nunca es alcanzable.

### <a name="the-throw-statement"></a>La instrucción throw

El `throw` instrucción produce una excepción.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

Un `throw` instrucción con una expresión produce el valor generado al evaluar la expresión. La expresión debe denotar un valor del tipo de clase `System.Exception`, de un tipo de clase que derive de `System.Exception` o de un tipo de parámetro de tipo que tiene `System.Exception` (o una subclase de ella) como su clase base efectiva. Si la evaluación de la expresión genera `null`, un `System.NullReferenceException` se produce en su lugar.

Un `throw` instrucción sin una expresión puede utilizarse únicamente en un `catch` bloquear, en cuyo caso esa instrucción vuelve a inicia la excepción que se está controlando actualmente por el que `catch` bloque.

Dado que un `throw` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `throw` instrucción nunca es alcanzable.

Cuando se produce una excepción, el control se transfiere a la primera `catch` cláusula en envolvente `try` instrucción que pueda controlar la excepción. El proceso que tiene lugar desde el punto de la excepción producida en el punto de transferencia de control a un controlador de excepciones adecuado se conoce como ***propagación de excepciones***. Propagación de una excepción consiste en evaluar repetidamente los pasos siguientes hasta que un `catch` cláusula que coincida con la excepción se encuentra. En esta descripción, el ***throw punto*** inicialmente es la ubicación en la que se produce la excepción.

*  En el miembro de función actual, cada `try` se examina la instrucción que encierra el punto de inicio. Para cada instrucción `S`, empezando por el más interno `try` instrucción y finaliza con el exterior `try` instrucción, se evalúan los pasos siguientes:

   * Si el `try` block de `S` encierra el punto de inicio y S tiene uno o varios `catch` cláusulas, la `catch` cláusulas se examinan por orden de aparición para buscar un controlador adecuado para la excepción, según las reglas especificadas en Sección [la instrucción try](statements.md#the-try-statement). Si una coincidencia `catch` cláusula se encuentra, se completa la propagación de excepciones mediante la transferencia de control al bloque de que `catch` cláusula.

   * De lo contrario, si la `try` bloque o una `catch` block de `S` encierra el punto de inicio y si `S` tiene un `finally` bloque, el control se transfiere a la `finally` bloque. Si el `finally` bloque inicia otra excepción, se termina el procesamiento de la excepción actual. En caso contrario, cuando el control alcanza el punto final de la `finally` bloque, continúa el procesamiento de la excepción actual.

*  Si un controlador de excepciones no se encuentra en la invocación de función actual, finaliza la invocación de función y se produce uno de los siguientes:

   * Si la función actual es que no es asincrónico, se repiten los pasos anteriores para que el llamador de la función con un punto de inicio correspondiente a la instrucción desde el que se invoca el miembro de función.

   * Si la función actual es asincrónico y devolución de tarea, la excepción se registra en la tarea devuelta, que se coloca en un estado cancelado o con errores, como se describe en [interfaces de enumerador](classes.md#enumerator-interfaces).

   * Si la función actual es asincrónico y devuelve void, el contexto de sincronización del subproceso actual se notifica como se describe en [interfaces enumerables](classes.md#enumerable-interfaces).

*  Si el procesamiento de la excepción finaliza todas las invocaciones de miembro de función en el subproceso actual, que indica que el subproceso no tiene ningún controlador para la excepción, a continuación, el subproceso está finaliza. El impacto de cuando dicha terminación es definido por la implementación.

## <a name="the-try-statement"></a>La instrucción try

El `try` instrucción proporciona un mecanismo para capturar las excepciones que se producen durante la ejecución de un bloque. Además, el `try` instrucción proporciona la capacidad de especificar un bloque de código que siempre se ejecuta cuando el control abandona el `try` instrucción.

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
*  Un `try` bloque seguido por un `finally` bloque.
*  Un `try` bloque seguido de uno o varios `catch` bloques seguido por un `finally` bloque.

Cuando un `catch` cláusula especifica un *exception_specifier*, el tipo debe ser `System.Exception`, un tipo que derive de `System.Exception` o un parámetro de tipo que tiene `System.Exception` (o una subclase de ella) como su efectivo clase base.

Cuando un `catch` cláusula especifica tanto un *exception_specifier* con un *identificador*, un ***variable de excepción*** del nombre especificado y el tipo se declara. La variable de excepción corresponde a una variable local con un ámbito que se extiende a través de la `catch` cláusula. Durante la ejecución de la *exception_filter* y *bloque*, la variable de excepción representa la excepción que se controla actualmente. Para fines de comprobación de asignación definitiva, la variable de excepción se considera asignado definitivamente en todo su ámbito.

A menos que un `catch` cláusula incluye un nombre de variable de excepción, es imposible obtener acceso al objeto de excepción en el filtro y `catch` bloque.

Un `catch` cláusula que no especifican un *exception_specifier* se denomina general `catch` cláusula.

Algunos lenguajes de programación pueden admitir las excepciones que no se puede representables como un objeto derivado de `System.Exception`, aunque dichas excepciones nunca podrían generar código de C#. General `catch` cláusula puede utilizarse para detectar estas excepciones. Por lo tanto, un general `catch` es semánticamente diferente desde el que especifica el tipo de cláusula `System.Exception`, ya que la primera también puede detectar las excepciones de otros lenguajes.

Para encontrar un controlador para una excepción, `catch` cláusulas se examinan en orden léxico. Si un `catch` cláusula especifica un tipo, pero ningún filtro de excepción, es un error en tiempo de compilación para una versión posterior `catch` cláusula en la misma `try` instrucción para especificar un tipo que es igual o se deriva y que escriba. Si un `catch` cláusula especifica ningún tipo y no hay ningún filtro, debe ser el último `catch` cláusula para que `try` instrucción.

Dentro de un `catch` bloque, un `throw` instrucción ([la instrucción throw](statements.md#the-throw-statement)) sin una expresión puede utilizarse para volver a producir la excepción que ha sido detectada por la `catch` bloque. Las asignaciones a una variable de excepción no modifican la excepción que se vuelve a producir.

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
el método `F` detecta una excepción, escribe información de diagnóstico en la consola, modifica la variable de excepción y vuelve a iniciar la excepción. La excepción que se vuelve a producir es la excepción original, por lo que el resultado es:
```
Exception in F: G
Exception in Main: G
```

Si el primer bloque catch iniciara `e` en lugar de volver a producir la excepción actual, el resultado sería como sigue:
```csharp
Exception in F: G
Exception in Main: F
```

Es un error en tiempo de compilación para un `break`, `continue`, o `goto` instrucción para transferir el control fuera de un `finally` bloque. Cuando un `break`, `continue`, o `goto` instrucción se produce en un `finally` bloque, el destino de la instrucción debe estar dentro del mismo `finally` bloque, o en caso contrario, se produce un error en tiempo de compilación.

Es un error en tiempo de compilación para un `return` instrucción que se produzca en un `finally` bloque.

Un `try` instrucción se ejecuta como sigue:

*  El control se transfiere a la `try` bloque.
*  Cuando y si el control alcanza el punto final de la `try` bloque:
   *  Si el `try` instrucción tiene un `finally` bloque, el `finally` bloque se ejecuta.
   *  El control se transfiere al punto final de la `try` instrucción.

*  Si una excepción se propaga a la `try` instrucción durante la ejecución de la `try` bloque:
   *  El `catch` cláusulas, si existe, se examinan en orden de aparición para buscar un controlador adecuado para la excepción. Si un `catch` no especifica un tipo de cláusula, o especifica el tipo de excepción o un tipo base del tipo de excepción:
      *  Si el `catch` cláusula declara una variable de excepción, el objeto de excepción se asigna a la variable de excepción.
      *  Si el `catch` cláusula declara un filtro de excepción, se evalúa el filtro. Si se evalúa como `false`, la cláusula catch no es una coincidencia y la búsqueda continúa a través de cualquier posteriores `catch` cláusulas para un controlador adecuado.
      *  En caso contrario, el `catch` cláusula se considera una coincidencia, y el control se transfiere a la coincidencia `catch` bloque.
      *  Cuando y si el control alcanza el punto final de la `catch` bloque:
         * Si el `try` instrucción tiene un `finally` bloque, el `finally` bloque se ejecuta.
         * El control se transfiere al punto final de la `try` instrucción.
      *  Si una excepción se propaga a la `try` instrucción durante la ejecución de la `catch` bloque:
         *  Si el `try` instrucción tiene un `finally` bloque, el `finally` bloque se ejecuta.
         *  La excepción se propaga a la siguiente inclusión `try` instrucción.
   *  Si el `try` instrucción no tiene ningún `catch` cláusulas o si no hay ningún `catch` cláusula coincide con la excepción:
      *  Si el `try` instrucción tiene un `finally` bloque, el `finally` bloque se ejecuta.
      *  La excepción se propaga a la siguiente inclusión `try` instrucción.

Las instrucciones de un `finally` bloque siempre se ejecutan cuando el control abandona un `try` instrucción. Esto es cierto si la transferencia de control se produce como resultado de una ejecución normal, como resultado de ejecutar un `break`, `continue`, `goto`, o `return` instrucción, o como resultado de la propagación de una excepción fuera de la `try` instrucción.

Si se produce una excepción durante la ejecución de un `finally` bloquea y no se captura dentro del mismo bloque finally, la excepción se propaga a la siguiente inclusión `try` instrucción. Si fue otra excepción en el proceso de propagación, esa excepción se pierde. El proceso de propagación de una excepción se explica con más detalle en la descripción de la `throw` instrucción ([la instrucción throw](statements.md#the-throw-statement)).

El `try` block de un `try` instrucción es alcanzable si el `try` instrucción es alcanzable.

Un `catch` block de un `try` instrucción es alcanzable si el `try` instrucción es alcanzable.

El `finally` block de un `try` instrucción es alcanzable si el `try` instrucción es alcanzable.

El punto final de un `try` instrucción es alcanzable si se cumplen dos de las siguientes acciones:

*  El punto final de la `try` bloque es alcanzable o el punto final de al menos un `catch` bloque es alcanzable.
*  Si un `finally` bloque está presente, el punto final de la `finally` bloque es alcanzable.

## <a name="the-checked-and-unchecked-statements"></a>Las instrucciones checked y unchecked

El `checked` y `unchecked` instrucciones sirven para controlar la ***contexto de comprobación de desbordamiento*** para conversiones y operaciones aritméticas de tipo integral.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

El `checked` instrucción hace que todas las expresiones de la *bloque* se evalúan en un contexto comprobado y el `unchecked` instrucción hace que todas las expresiones de la *bloque* se evalúan en un un contexto no comprobado.

El `checked` y `unchecked` son exactamente equivalentes a las instrucciones del `checked` y `unchecked` operadores ([los operadores checked y unchecked](expressions.md#the-checked-and-unchecked-operators)), excepto en que operan en bloques en lugar de expresiones .

## <a name="the-lock-statement"></a>La instrucción lock

El `lock` instrucción obtiene el bloqueo de exclusión mutua para un objeto determinado, ejecuta una instrucción y, a continuación, libera el bloqueo.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

La expresión de un `lock` instrucción debe denotar un valor de un tipo conocido como un *reference_type*. No hay conversión boxing implícita ([conversiones Boxing](conversions.md#boxing-conversions)) nunca se realiza para la expresión de un `lock` instrucción y, por lo tanto es un error de tiempo de compilación de la expresión denota un valor de un *value_type*.

Un `lock` instrucción del formulario
```csharp
lock (x) ...
```
donde `x` es una expresión de un *reference_type*, es equivalente a
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

Mientras se mantiene un bloqueo de exclusión mutua, código que se ejecuta en el mismo subproceso de ejecución también puede obtener y liberar el bloqueo. Sin embargo, se bloquea código que se ejecuta en otros subprocesos obtengan el bloqueo hasta que se libere el bloqueo.

Bloqueo `System.Type` objetos con el fin de sincronizar el acceso a los datos estáticos no se recomienda. Puede bloquear otro código en el mismo tipo, que puede producir un interbloqueo. Un mejor enfoque es sincronizar el acceso a los datos estáticos mediante el bloqueo de un objeto estático privado. Por ejemplo:
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

El `using` instrucción obtiene uno o más recursos, ejecuta una instrucción y, a continuación, elimina el recurso.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

Un ***recursos*** es una clase o struct que implemente `System.IDisposable`, que incluye un único método sin parámetros denominado `Dispose`. Puede llamar código que usa un recurso `Dispose` para indicar que el recurso ya no es necesario. Si `Dispose` no se llama disposición automática finalmente se produce como consecuencia de la recolección de elementos.

Si el formulario de *resource_acquisition* es *local_variable_declaration* , a continuación, el tipo de la *local_variable_declaration* debe ser `dynamic` o un tipo que se pueda convertir implícitamente a `System.IDisposable`. Si el formulario de *resource_acquisition* es *expresión* , a continuación, esta expresión debe ser implícitamente convertible a `System.IDisposable`.

Las variables locales declaradas en un *resource_acquisition* son de solo lectura y debe incluir un inicializador. Se produce un error de tiempo de compilación si la instrucción incrustada intenta modificar estas variables locales (por medio de asignación o el `++` y `--` operadores), tomar la dirección de ellos o pasarlos como `ref` o `out` parámetros.

Un `using` instrucción se traduce en tres partes: adquisición, uso y eliminación. Uso del recurso está incluido implícitamente en un `try` instrucción que incluye un `finally` cláusula. Esto `finally` cláusula elimina el recurso. Si un `null` se adquiere el recurso, a continuación, ninguna llamada a `Dispose` se realiza, y se produce ninguna excepción. Si el recurso es de tipo `dynamic` dinámicamente se convierte a través de una conversión implícita dinámica ([conversiones implícitas de dinámicas](conversions.md#implicit-dynamic-conversions)) a `IDisposable` durante la adquisición para asegurarse de que la conversión es finalizar correctamente para el uso y la eliminación.

Un `using` instrucción del formulario
```csharp
using (ResourceType resource = expression) statement
```
corresponde a uno de tres posibles expansiones. Cuando `ResourceType` es un tipo de valor distinto de null, la expansión es:
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

En caso contrario, cuando `ResourceType` es un tipo de valor que acepta valores NULL o un tipo de referencia distinto `dynamic`, la expansión es:
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

En caso contrario, cuando `ResourceType` es `dynamic`, la expansión es:
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

En cualquiera de las expansiones, el `resource` variable es de solo lectura en la instrucción incrustada y el `d` variable es accesible en el e invisible a la instrucción incrustada.

Se permite una implementación para implementar una determinada instrucción using de manera diferente, por ejemplo, por motivos de rendimiento, siempre y cuando el comportamiento es coherente con la expansión de la anterior.

Un `using` instrucción del formulario
```csharp
using (expression) statement
```
tiene las mismas tres posibles expansiones. En este caso `ResourceType` es implícitamente el tipo de tiempo de compilación de la `expression`, si tiene uno. En caso contrario, la interfaz `IDisposable` propio se utiliza como el `ResourceType`. El `resource` variable es accesible en el e invisible a la instrucción incrustada.

Cuando un *resource_acquisition* adopta la forma de un *local_variable_declaration*, es posible adquirir varios recursos de un tipo determinado. Un `using` instrucción del formulario
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
se anida con precisión equivalente a una secuencia de `using` instrucciones:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

El ejemplo siguiente crea un archivo denominado `log.txt` y escribe dos líneas de texto en el archivo. El ejemplo, a continuación, abre el mismo archivo para lectura y copia las líneas de texto independientes en la consola.
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

Puesto que la `TextWriter` y `TextReader` clases implementan la `IDisposable` interfaz, puede usar el ejemplo `using` instrucciones para asegurarse de que el archivo subyacente se cierra correctamente después de la escritura o las operaciones de lectura.

## <a name="the-yield-statement"></a>La instrucción yield

El `yield` instrucción se utiliza en un bloque de iteradores ([bloques](statements.md#blocks)) para producir un valor al objeto enumerador ([objetos enumerador](classes.md#enumerator-objects)) o un objeto enumerable ([objetos enumerables](classes.md#enumerable-objects)) de un iterador o para indicar el final de la iteración.

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` no es una palabra reservada; tiene un significado especial solo cuando se usa inmediatamente antes un `return` o `break` palabra clave. En otros contextos, `yield` puede usarse como identificador.

Existen varias restricciones sobre dónde un `yield` instrucción puede aparecer como se describe en la siguiente.

*  Es un error en tiempo de compilación para un `yield` (informe de cualquiera de las formas) a aparecer fuera de un *cuerpoMétodo*, *operator_body* o *accessor_body*
*  Es un error en tiempo de compilación para un `yield` (informe de cualquiera de las formas) a aparecer dentro de una función anónima.
*  Es un error en tiempo de compilación para un `yield` (informe de cualquiera de las formas) aparezca en el `finally` cláusula de una `try` instrucción.
*  Es un error en tiempo de compilación para un `yield return` instrucción para aparecer en cualquier lugar en un `try` instrucción que incluye cualquier `catch` cláusulas.

El ejemplo siguiente muestra algunos usos válidos y no válidos de `yield` instrucciones.

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

Una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)) debe existir el tipo de la expresión en el `yield return` instrucción al tipo yield ([tipo Yield](classes.md#yield-type)) del iterador.

Un `yield return` instrucción se ejecuta como sigue:

*  La expresión proporcionada en la instrucción se evalúa implícitamente convierte al tipo yield y asignada a la `Current` propiedad del objeto enumerador.
*  Se suspende la ejecución del bloque de iteradores. Si el `yield return` instrucción está dentro de uno o varios `try` bloquea asociado `finally` bloques no se ejecutan en este momento.
*  El `MoveNext` método del objeto enumerador devuelve `true` a su llamador, que indica que el objeto de enumerador avanza correctamente al elemento siguiente.

La siguiente llamada para el objeto de enumerador `MoveNext` método reanuda la ejecución del bloque de iteradores desde donde se suspendió por última vez.

Un `yield break` instrucción se ejecuta como sigue:

*  Si el `yield break` instrucción está dentro de uno o varios `try` bloques con asociados `finally` bloques de control se transfiere inicialmente a la `finally` bloque del interior `try` instrucción. Cuando y si el control alcanza el punto final de un `finally` bloque, el control se transfiere a la `finally` bloque de la siguiente inclusión `try` instrucción. Este proceso se repite hasta que el `finally` bloques de inclusión de todos los `try` se han ejecutado las instrucciones.
*  Control se devuelve al llamador del bloque de iteradores. Puede ser el `MoveNext` método o `Dispose` método del objeto enumerador.

Dado que un `yield break` instrucción incondicionalmente transfiere el control en otra parte, el punto final de un `yield break` instrucción nunca es alcanzable.
