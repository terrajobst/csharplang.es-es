---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483983"
---
# <a name="recursive-pattern-matching"></a>Coincidencia de patrones recursivos

## <a name="summary"></a>Resumen
[summary]: #summary

Las extensiones de coincidencia C# de patrones para permiten muchas de las ventajas de los tipos de datos algebraicos y la coincidencia de patrones de los lenguajes funcionales, pero de una manera que se integra sin problemas con la sensación del lenguaje subyacente. Los elementos de este enfoque están inspirados en las características relacionadas en [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Coincidencia de patrones extensible a través de un lenguaje ligero") los lenguajes de programación y [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Coincidencia de objetos con patrones, página 273").

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

### <a name="is-expression"></a>Expresión is

El operador `is` se extiende para probar una expresión con un *patrón*.

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

Esta forma de *relational_expression* se suma a los formularios existentes en la C# especificación. Es un error en tiempo de compilación si el *relational_expression* a la izquierda del token de `is` no designa un valor o no tiene un tipo.

Cada *identificador* del patrón introduce una nueva variable local que se *asigna definitivamente* después de que el operador de `is` sea `true` (es decir, se *asigna definitivamente cuando es true*).

> Nota: técnicamente existe una ambigüedad entre el *tipo* en un `is-expression` y *constant_pattern*, cualquiera de los cuales puede ser un análisis válido de un identificador calificado. Intentamos enlazarlo como un tipo para la compatibilidad con versiones anteriores del lenguaje; solo si se produce un error, lo resolvemos como hacemos una expresión en otros contextos, lo primero que se encontró (que debe ser una constante o un tipo). Esta ambigüedad solo está presente en la parte derecha de una expresión de `is`.

### <a name="patterns"></a>Patrones

Los patrones se usan en el operador *is_pattern* , en un *switch_statement*y en un *switch_expression* para expresar la forma de los datos en los que se van a comparar los datos entrantes (que llamamos al valor de entrada). Los patrones pueden ser recursivos para que se puedan comparar partes de los datos con subpatrones.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a>Modelo de declaración

```antlr
declaration_pattern
    : type simple_designation
    ;
```

El *declaration_pattern* ambos comprueba que una expresión es de un tipo determinado y la convierte a ese tipo si la prueba se realiza correctamente. Esto puede introducir una variable local del tipo especificado denominado por el identificador dado, si la designación es una *single_variable_designation*. Esa variable local se *asigna definitivamente* cuando se `true`el resultado de la operación de coincidencia de patrones.

La semántica en tiempo de ejecución de esta expresión es que prueba el tipo en tiempo de ejecución del operando del lado izquierdo *relational_expression* en el *tipo* del patrón.  Si es de ese tipo en tiempo de ejecución (o algún subtipo) y no `null`, el resultado de la `is operator` es `true`.

Ciertas combinaciones de tipo estático del lado izquierdo y del tipo especificado se consideran incompatibles y producen un error en tiempo de compilación. Se dice que un valor de tipo estático `E` es *compatible* con el patrón con un tipo `T` si existe una conversión de identidad, una conversión de referencia implícita, una conversión boxing, una conversión de referencia explícita o una conversión unboxing de `E` a `T`, o si uno de esos tipos es un tipo abierto. Es un error en tiempo de compilación si una entrada de tipo `E` no es *compatible* con el patrón con el *tipo* de un patrón de tipo con el que se encuentra una coincidencia.

El patrón de tipo es útil para realizar pruebas de tipo en tiempo de ejecución de tipos de referencia y reemplaza la expresión

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

Con el poco más conciso

```csharp
if (expr is Type v) { // code using v
```

Es un error si el *tipo* es un tipo de valor que acepta valores NULL.

El patrón de tipo se puede usar para probar valores de tipos que aceptan valores NULL: un valor de tipo `Nullable<T>` (o un `T`con conversión boxing) coincide con un patrón de tipo `T2 id` si el valor no es NULL y el tipo de `T2` es `T`, o algún tipo base o interfaz de `T`. Por ejemplo, en el fragmento de código

```csharp
int? x = 3;
if (x is int v) { // code using v
```

La condición de la instrucción `if` es `true` en tiempo de ejecución y la variable `v` contiene el valor `3` de tipo `int` dentro del bloque. Después del bloqueo, la variable `v` está en el ámbito pero no se asigna de forma definitiva.

#### <a name="constant-pattern"></a>Patrón de constante

```antlr
constant_pattern
    : constant_expression
    ;
```

Un patrón de constante prueba el valor de una expresión con respecto a un valor constante. La constante puede ser cualquier expresión constante, como un literal, el nombre de una variable `const` declarada o una constante de enumeración. Cuando el valor de entrada no es un tipo abierto, la expresión constante se convierte implícitamente al tipo de la expresión coincidente; Si el tipo del valor de entrada no es *compatible* con el patrón con el tipo de la expresión constante, la operación de coincidencia de patrones es un error.

El patrón *c* se considera que coincide con el valor de entrada convertido *e* si `object.Equals(c, e)` devolvería `true`.

Esperamos ver `e is null` como la manera más común de comprobar `null` en el código recién escrito, ya que no puede invocar un `operator==`definido por el usuario.

#### <a name="var-pattern"></a>Patrón var

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

Si la *designación* es una *simple_designation*, una expresión *e* coincide con el patrón. En otras palabras, una coincidencia con un *patrón var* siempre se realiza correctamente con una *simple_designation*. Si el *simple_designation* es un *single_variable_designation*, el valor de *e* se enlaza a una variable local recién introducida. El tipo de la variable local es el tipo estático de *e*.

Si la *designación* es una *tuple_designation*, el patrón es equivalente a un *positional_pattern* con el formato `(var` *designación*,... `)` donde la *designación*son las que se encuentran en el *tuple_designation*.  Por ejemplo, el patrón `var (x, (y, z))` es equivalente a `(var x, (var y, var z))`.

Es un error si el nombre `var` enlaza a un tipo.

#### <a name="discard-pattern"></a>Patrón de descartar

```antlr
discard_pattern
    : '_'
    ;
```

Una expresión *e* coincide con el patrón `_` siempre. En otras palabras, cada expresión coincide con el patrón de descarte.

Un patrón de descarte no se puede usar como el patrón de una *is_pattern_expression*.

#### <a name="positional-pattern"></a>Patrón posicional

Un patrón posicional comprueba que el valor de entrada no se `null`, invoca un método `Deconstruct` adecuado y realiza una mayor coincidencia de patrones en los valores resultantes.  También admite una sintaxis de modelo de tipo tupla (sin el tipo que se proporciona) cuando el tipo del valor de entrada es el mismo que el tipo que contiene `Deconstruct`, o si el tipo del valor de entrada es un tipo de tupla, o si el tipo del valor de entrada es `object` o `ITuple` y el tipo en tiempo de ejecución de la expresión implementa `ITuple`.

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

Si se omite el *tipo* , se toma como el tipo estático del valor de entrada.

Dada una coincidencia de un valor de entrada con el *tipo* de patrón `(` *subpattern_list* `)`, se selecciona un método buscando en el *tipo* para las declaraciones accesibles de `Deconstruct` y seleccionando uno entre ellos con las mismas reglas que para la declaración de desconstrucción.

Es un error si una *positional_pattern* omite el tipo, tiene un único *subpatrón* sin un *identificador*, no tiene *property_subpattern* y no tiene ningún *simple_designation*. Esta anulación de una *constant_pattern* entre paréntesis y una *positional_pattern*.

Para extraer los valores para que coincidan con los patrones de la lista,
- Si se omite el *tipo* y el tipo del valor de entrada es un tipo de tupla, el número de subpatrones debe ser el mismo que la cardinalidad de la tupla. Cada elemento de tupla coincide con el *subpatrón*correspondiente y la coincidencia se realiza correctamente si todos ellos se realizan correctamente. Si algún *subpatrón* tiene un *identificador*, debe denominar un elemento de tupla en la posición correspondiente en el tipo de tupla.
- De lo contrario, si un `Deconstruct` adecuado existe como miembro de *tipo*, es un error en tiempo de compilación si el tipo del valor de entrada no es compatible con el *patrón* con el *tipo*. En tiempo de ejecución, el valor de entrada se prueba con el *tipo*. Si se produce un error, se produce un error en la coincidencia del patrón posicional. Si se realiza correctamente, el valor de entrada se convierte a este tipo y `Deconstruct` se invoca con variables nuevas generadas por el compilador para recibir los parámetros de `out`. Cada valor recibido se compara con el *subpatrón*correspondiente y la coincidencia se realiza correctamente si todos ellos se realizan correctamente. Si algún *subpatrón* tiene un *identificador*, debe asignar un nombre a un parámetro en la posición correspondiente de `Deconstruct`.
- De lo contrario, si se omitió el *tipo* y el valor de entrada es de tipo `object` o `ITuple` o algún tipo que se pueda convertir a `ITuple` por una conversión de referencia implícita, y no aparece ningún *identificador* entre los subpatróns, coincidirá con `ITuple`.
- De lo contrario, el patrón es un error en tiempo de compilación.

El orden en el que se buscan coincidencias con los subpatróns en tiempo de ejecución no se especifica y es posible que no se intente coincidir con todos los subpatróns.

##### <a name="example"></a>Ejemplo

En este ejemplo se usan muchas de las características descritas en esta especificación.

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a>Patrón de propiedad

Un patrón de propiedades comprueba que el valor de entrada no es `null` y coincide de forma recursiva con los valores extraídos por el uso de propiedades o campos accesibles.

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

Es un error si algún _subpatrón_ de una _property_pattern_ no contiene un _identificador_ (debe ser de la segunda forma, que tiene un _identificador_).  Una coma final después del último subpatrón es opcional.

Tenga en cuenta que un patrón de comprobación de valores NULL cae de un patrón de propiedad trivial. Para comprobar si el `s` de la cadena no es null, puede escribir cualquiera de las siguientes formas:

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

Dada una coincidencia de una expresión *e* con el *tipo* de patrón `{` *property_pattern_list* `}`, se trata de un error en tiempo de compilación si la expresión *e* no es compatible con el *patrón* con el tipo *t* designado por el *tipo*. Si el tipo está ausente, se toma como el tipo estático de *e*. Si el *identificador* está presente, declara una variable de patrón de tipo *Type*. Cada uno de los identificadores que aparecen en el lado izquierdo de su *property_pattern_list* debe designar una propiedad o campo legible accesible de *T*. Si el *simple_designation* del *property_pattern* está presente, define una variable de patrón de tipo *T*.

En tiempo de ejecución, la expresión se prueba en *T*. Si se produce un error, se produce un error en la coincidencia del patrón de propiedad y el resultado es `false`. Si se realiza correctamente, cada *property_subpattern* campo o propiedad se lee y su valor coincide con su patrón correspondiente. El resultado de la coincidencia completa es `false` solo si el resultado de cualquiera de ellos es `false`. No se especifica el orden en el que coinciden los subpatrones y es posible que no coincidan todos los subpatróns en tiempo de ejecución. Si la coincidencia se realiza correctamente y el *simple_designation* del *property_pattern* es un *single_variable_designation*, define una variable de tipo *T* que tiene asignado el valor coincidente.

> Nota: el patrón de propiedad se puede usar para la coincidencia de patrones con tipos anónimos.

##### <a name="example"></a>Ejemplo

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a>Cambiar expresión

Se agrega un *switch_expression* para admitir la semántica de tipo `switch`para un contexto de expresión.

La C# sintaxis del lenguaje se amplía con las siguientes producciones sintácticas:

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

No se permite el *switch_expression* como *expression_statement*.

> Estamos examinando este aspecto en una revisión futura.

El tipo del *switch_expression* es el [*mejor tipo común*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) de las expresiones que aparecen a la derecha de los tokens de `=>` de la *switch_expression_arm*s si este tipo existe y la expresión en cada brazo de la expresión switch se puede convertir implícitamente a ese tipo.  Además, se agrega una nueva *conversión de expresión switch*, que es una conversión implícita predefinida de una expresión switch a cada tipo `T` para el que existe una conversión implícita de cada expresión de arm en `T`.

Es un error si algún patrón de *switch_expression_arm*no puede afectar al resultado porque algún patrón y una protección anteriores siempre coincidirán.

Se dice que una expresión switch es *exhaustiva* si algún brazo de la expresión switch controla cada valor de su entrada.  El compilador generará una advertencia si una expresión switch no es *exhaustiva*.

En tiempo de ejecución, el resultado de la *switch_expression* es el valor de la *expresión* de la primera *switch_expression_arm* para la que la expresión del lado izquierdo del *switch_expression* coincide con el patrón del *switch_expression_arm*y para el que el *case_guard* de la *switch_expression_arm*, si está presente, se evalúa como `true`. Si no existe tal *switch_expression_arm*, el *switch_expression* inicia una instancia de la excepción `System.Runtime.CompilerServices.SwitchExpressionException`.

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a>Paréntesis opcionales al cambiar un literal de tupla

Para cambiar a un literal de tupla mediante el *switch_statement*, tiene que escribir lo que parece ser un paréntesis redundante

```csharp
switch ((a, b))
{
```

Para permitir

```csharp
switch (a, b)
{
```

los paréntesis de la instrucción switch son opcionales cuando la expresión que se está cambiando es un literal de tupla.

### <a name="order-of-evaluation-in-pattern-matching"></a>Orden de evaluación en coincidencia de patrones

Ofrecer la flexibilidad del compilador al reordenar las operaciones ejecutadas durante la coincidencia de patrones puede permitir la flexibilidad que se puede utilizar para mejorar la eficacia de la coincidencia de patrones. El requisito (no exigido) sería que las propiedades a las que se tiene acceso en un patrón y los métodos de deconstrucción deben ser "puras" (sin efecto secundario, idempotente, etc.). Eso no significa que se agregue la pureza como concepto de lenguaje, solo que se permitiría la flexibilidad del compilador en las operaciones de reordenación.

**Resolución 2018-04-04 LDM**: confirmada: el compilador tiene permiso para reordenar las llamadas a `Deconstruct`, accesos de propiedad e invocaciones de métodos en `ITuple`, y puede suponer que los valores devueltos son los mismos desde varias llamadas. El compilador no debe invocar las funciones que no pueden afectar al resultado, por lo que tendremos mucho cuidado antes de realizar cambios en el orden generado por el compilador de la evaluación en el futuro.

### <a name="some-possible-optimizations"></a>Algunas optimizaciones posibles

La compilación de la coincidencia de patrones puede aprovechar las partes comunes de los patrones. Por ejemplo, si la prueba de tipo de nivel superior de dos patrones sucesivos de un *switch_statement* es del mismo tipo, el código generado puede omitir la prueba de tipo para el segundo patrón.

Cuando algunos de los patrones son enteros o cadenas, el compilador puede generar el mismo tipo de código que genera para una instrucción switch en versiones anteriores del lenguaje.

Para obtener más información sobre estos tipos de optimizaciones, vea [[Scott and Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "¿Cuándo se importa la heurística de compilaciones de coincidencia?").
