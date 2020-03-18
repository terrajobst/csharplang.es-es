---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484073"
---
# <a name="pattern-matching-for-c-7"></a>Coincidencia de patrones C# para 7

Las extensiones de coincidencia C# de patrones para permiten muchas de las ventajas de los tipos de datos algebraicos y la coincidencia de patrones de los lenguajes funcionales, pero de una manera que se integra sin problemas con la sensación del lenguaje subyacente. Las características básicas son: [tipos de registros](https://github.com/dotnet/csharplang/blob/master/proposals/records.md), que son tipos cuyo significado semántico se describe mediante la forma de los datos. y la coincidencia de patrones, que es un nuevo formulario de expresión que permite la descomposición de varios niveles de estos tipos de datos. Los elementos de este enfoque están inspirados en las características relacionadas en [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Coincidencia de patrones extensible a través de un lenguaje ligero") los lenguajes de programación y [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Coincidencia de objetos con patrones").

## <a name="is-expression"></a>Expresión is

El operador `is` se extiende para probar una expresión con un *patrón*.

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

Esta forma de *relational_expression* se suma a los formularios existentes en la C# especificación. Es un error en tiempo de compilación si el *relational_expression* a la izquierda del token de `is` no designa un valor o no tiene un tipo.

Cada *identificador* del patrón introduce una nueva variable local que se *asigna definitivamente* después de que el operador de `is` sea `true` (es decir, se *asigna definitivamente cuando es true*).

> Nota: técnicamente existe una ambigüedad entre el *tipo* en un `is-expression` y *constant_pattern*, cualquiera de los cuales puede ser un análisis válido de un identificador calificado. Intentamos enlazarlo como un tipo para la compatibilidad con versiones anteriores del lenguaje; solo si se produce un error, se resuelve como hacemos en otros contextos, lo primero que se encontró (que debe ser una constante o un tipo). Esta ambigüedad solo está presente en la parte derecha de una expresión de `is`.

## <a name="patterns"></a>Patrones

Los patrones se utilizan en el operador `is` y en un *switch_statement* para expresar la forma de los datos con los que se van a comparar los datos entrantes. Los patrones pueden ser recursivos para que se puedan comparar partes de los datos con subpatrones.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> Nota: técnicamente existe una ambigüedad entre el *tipo* en un `is-expression` y *constant_pattern*, cualquiera de los cuales puede ser un análisis válido de un identificador calificado. Intentamos enlazarlo como un tipo para la compatibilidad con versiones anteriores del lenguaje; solo si se produce un error, se resuelve como hacemos en otros contextos, lo primero que se encontró (que debe ser una constante o un tipo). Esta ambigüedad solo está presente en la parte derecha de una expresión de `is`.

### <a name="declaration-pattern"></a>Modelo de declaración

El *declaration_pattern* ambos comprueba que una expresión es de un tipo determinado y la convierte a ese tipo si la prueba se realiza correctamente. Si el *simple_designation* es un identificador, introduce una variable local del tipo especificado denominado por el identificador especificado. Esa variable local se *asigna definitivamente* cuando el resultado de la operación de coincidencia de patrones es true.

```antlr
declaration_pattern
    : type simple_designation
    ;
```

La semántica en tiempo de ejecución de esta expresión es que prueba el tipo en tiempo de ejecución del operando del lado izquierdo *relational_expression* en el *tipo* del patrón. Si es de ese tipo en tiempo de ejecución (o algún subtipo), el resultado del `is operator` es `true`. Declara una nueva variable local denominada por el *identificador* al que se asigna el valor del operando izquierdo cuando el resultado es `true`.

Ciertas combinaciones de tipo estático del lado izquierdo y del tipo especificado se consideran incompatibles y producen un error en tiempo de compilación. Se dice que un valor de tipo estático `E` es *compatible* con el tipo `T` si existe una conversión de identidad, una conversión de referencia implícita, una conversión boxing, una conversión de referencia explícita o una conversión unboxing de `E` a `T`. Es un error en tiempo de compilación si una expresión de tipo `E` no es compatible con el tipo en un patrón de tipo con el que se encuentra una coincidencia.

> Nota: [en C# 7,1 se amplía esto](../csharp-7.1/generics-pattern-match.md) para permitir una operación de coincidencia de patrones si el tipo de entrada o el tipo `T` es un tipo abierto. Este párrafo se sustituye por lo siguiente:
> 
> Ciertas combinaciones de tipo estático del lado izquierdo y del tipo especificado se consideran incompatibles y producen un error en tiempo de compilación. Se dice que un valor de tipo estático `E` es *compatible* con el tipo `T` si existe una conversión de identidad, una conversión de referencia implícita, una conversión boxing, una conversión de referencia explícita o una conversión unboxing de `E` a `T`, **o bien, si `E` o `T` es un tipo abierto**. Es un error en tiempo de compilación si una expresión de tipo `E` no es compatible con el tipo en un patrón de tipo con el que se encuentra una coincidencia.

El modelo de declaración es útil para realizar pruebas de tipo en tiempo de ejecución de tipos de referencia y reemplaza la expresión

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

Con el poco más conciso

```csharp
if (expr is Type v) { // code using v }
```

Es un error si el *tipo* es un tipo de valor que acepta valores NULL.

El modelo de declaración se puede usar para probar valores de tipos que aceptan valores NULL: un valor de tipo `Nullable<T>` (o un `T`con conversión boxing) coincide con un patrón de tipo `T2 id` si el valor no es NULL y el tipo de `T2` es `T`, o algún tipo base o interfaz de `T`. Por ejemplo, en el fragmento de código

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

La condición de la instrucción `if` es `true` en tiempo de ejecución y la variable `v` contiene el valor `3` de tipo `int` dentro del bloque.

### <a name="constant-pattern"></a>Patrón de constante

```antlr
constant_pattern
    : shift_expression
    ;
```

Un patrón de constante prueba el valor de una expresión con respecto a un valor constante. La constante puede ser cualquier expresión constante, como un literal, el nombre de una variable `const` declarada, una constante de enumeración o una expresión `typeof`.

Si tanto *e* como *c* son de tipos enteros, se considera que el patrón coincide si el resultado de la expresión `e == c` es `true`.

De lo contrario, se considera que el patrón coincide si `object.Equals(e, c)` devuelve `true`. En este caso, se trata de un error en tiempo de compilación si el tipo estático de *e* no es *compatible* con el tipo de la constante.

### <a name="var-pattern"></a>Patrón var

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

Una expresión *e* coincide con un *var_pattern* siempre. En otras palabras, una coincidencia con un *patrón var* siempre se realiza correctamente. Si el *simple_designation* es un identificador, en tiempo de ejecución el valor de *e* se enlaza a una variable local recién introducida. El tipo de la variable local es el tipo estático de *e*.

Es un error si el nombre `var` enlaza a un tipo.

## <a name="switch-statement"></a>Instrucción switch

La instrucción `switch` se extiende para seleccionar la ejecución del primer bloque que tiene un patrón asociado que coincide con la *expresión switch*.

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

No se define el orden en el que se buscan coincidencias con los patrones. Se permite que un compilador coincida con los patrones desordenados y reutilizar los resultados de los patrones ya coincidentes para calcular el resultado de la coincidencia de otros patrones.

Si hay una *protección de casos* , su expresión es de tipo `bool`. Se evalúa como una condición adicional que se debe satisfacer para que el caso se considere satisfecho.

Es un error si un *switch_label* puede no tener ningún efecto en el tiempo de ejecución porque su patrón está en los casos anteriores. [TODO: deberíamos ser más precisos sobre las técnicas que el compilador necesita usar para alcanzar esta resolución.]

Una variable de patrón declarada en un *switch_label* se asigna definitivamente en su bloque case si y solo si ese bloque Case contiene exactamente un *switch_label*.

[TODO: deberíamos especificar Cuándo se puede obtener acceso a un *bloque switch* .]

### <a name="scope-of-pattern-variables"></a>Ámbito de las variables de patrón

El ámbito de una variable declarada en un patrón es el siguiente:

- Si el patrón es una etiqueta de caso, el ámbito de la variable es el *bloque de casos*.

En caso contrario, la variable se declara en una expresión *is_pattern* y su ámbito se basa en la construcción que incluye inmediatamente la expresión que contiene la expresión *is_pattern* de la siguiente manera:

- Si la expresión se encuentra en una expresión lambda con cuerpo de expresión, su ámbito es el cuerpo de la lambda.
- Si la expresión se encuentra en un método o propiedad con forma de expresión, su ámbito es el cuerpo del método o propiedad.
- Si la expresión está en una cláusula `when` de una cláusula `catch`, su ámbito es la cláusula `catch`.
- Si la expresión se encuentra en un *iteration_statement*, su ámbito es simplemente esa instrucción.
- De lo contrario, si la expresión está en otra forma de instrucción, su ámbito es el ámbito que contiene la instrucción.

Con el fin de determinar el ámbito, se considera que una *embedded_statement* está en su propio ámbito. Por ejemplo, la gramática de una *if_statement* es

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Por tanto, si la instrucción controlada de un *if_statement* declara una variable de patrón, su ámbito está restringido a ese *embedded_statement*:

```csharp
if (x) M(y is var z);
```

En este caso, el ámbito de `z` es la instrucción incrustada `M(y is var z);`.

Otros casos son errores por otros motivos (por ejemplo, en el valor predeterminado de un parámetro o en un atributo, ambos son un error porque esos contextos requieren una expresión constante).

> [En C# 7,3 hemos agregado los siguientes contextos](../csharp-7.3/expression-variables-in-initializers.md) en los que se puede declarar una variable de patrón:
> - Si la expresión se encuentra en un *inicializador de constructor*, su ámbito es el *inicializador del constructor* y el cuerpo del constructor.
> - Si la expresión se encuentra en un inicializador de campo, su ámbito es el *equals_value_clause* en el que aparece.
> - Si la expresión se encuentra en una cláusula de consulta que se especifica para traducirse en el cuerpo de una expresión lambda, su ámbito es simplemente esa expresión.

## <a name="changes-to-syntactic-disambiguation"></a>Cambios en la desambiguación sintáctica

Existen situaciones en las que se incluyen los genéricos en los que la C# gramática es ambigua y en la especificación del lenguaje se indica cómo resolver esas ambigüedades:

> #### <a name="7652-grammar-ambiguities"></a>7.6.5.2 ambigüedades de la gramática
> Las producciones de *nombre simple* (§ 7.6.3) y *acceso a miembros* (§ 7.6.5) pueden dar lugar a ambigüedades en la gramática de expresiones. Por ejemplo, la instrucción:
> ```csharp
> F(G<A,B>(7));
> ```
> podría interpretarse como una llamada a `F` con dos argumentos, `G < A` y `B > (7)`. Como alternativa, podría interpretarse como una llamada a `F` con un argumento, que es una llamada a un método genérico `G` con dos argumentos de tipo y un argumento normal.

> Si una secuencia de tokens se puede analizar (en contexto) como un *nombre simple* (§ 7.6.3), *acceso a miembro* (§ 7.6.5) o *puntero-miembro-acceso* (§ 18.5.2) que termina con una *lista de argumentos de tipo* (§ 4.4.1), se examina el token que sigue inmediatamente al cierre `>` token. Si es uno de
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> a continuación, la *lista de argumentos de tipo* se conserva como parte del acceso *simple*, de *acceso a miembros* o de miembro de *puntero* , y se descarta cualquier otro posible análisis de la secuencia de tokens. De lo contrario, la *lista de argumentos de tipo* no se considera parte del *nombre simple*, *acceso a miembros* o > puntero- *miembro-acceso*, incluso si no hay otro posible análisis de la secuencia de tokens. Tenga en cuenta que estas reglas no se aplican al analizar una *lista de argumentos de tipo* en un nombre de espacio de *nombres o-tipo* (§ 3,8). La instrucción
> ```csharp
> F(G<A,B>(7));
> ```
> , según esta regla, se interpretará como una llamada a `F` con un argumento, que es una llamada a un método genérico `G` con dos argumentos de tipo y un argumento normal. Las instrucciones
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> cada uno de ellos se interpretará como una llamada a `F` con dos argumentos. La instrucción
> ```csharp
> x = F < A > +y;
> ```
> se interpretará como un operador menor que, mayor que y unario más, como si la instrucción se hubiera escrito `x = (F < A) > (+y)`, en lugar de como un *nombre simple* con una *lista de argumentos de tipo* seguido de un operador binario Plus. En la instrucción
> ```csharp
> x = y is C<T> + z;
> ```
> los tokens `C<T>` se interpretan como un *espacio de nombres o un nombre de tipo* con una *lista de argumentos de tipo*.

Hay una serie de cambios que se introducen C# en 7 que hacen que estas reglas de desambiguación dejen de ser suficientes para controlar la complejidad del lenguaje.

### <a name="out-variable-declarations"></a>Declaraciones de variable out

Ahora es posible declarar una variable en un argumento out:

```csharp
M(out Type name);
```

Sin embargo, el tipo puede ser genérico: 

```csharp
M(out A<B> name);
```

Dado que la gramática de idioma para el argumento usa *Expression*, este contexto está sujeto a la regla de desambiguación. En este caso, el `>` de cierre va seguido de un *identificador*, que no es uno de los tokens que permite que se trate como una *lista de argumentos de tipo*. Por lo tanto, se propone **Agregar el *identificador* al conjunto de tokens que desencadena la desambiguación en una *lista de argumentos de tipo*.**

### <a name="tuples-and-deconstruction-declarations"></a>Tuplas y declaraciones de desconstrucción

Un literal de tupla se ejecuta exactamente en el mismo problema. Considere la expresión de tupla.

```csharp
(A < B, C > D, E < F, G > H)
```

En las 6 C# reglas anteriores para analizar una lista de argumentos, esto se analizaría como una tupla con cuatro elementos, empezando por `A < B` como primer. Sin embargo, cuando esto aparece a la izquierda de una desconstrucción, queremos que la desambiguación se desencadene por el token del *identificador* como se describió anteriormente:

```csharp
(A<B,C> D, E<F,G> H) = e;
```

Se trata de una declaración de desconstrucción que declara dos variables, la primera de las cuales es de tipo `A<B,C>` y con el nombre `D`. En otras palabras, el literal de tupla contiene dos expresiones, cada una de las cuales es una expresión de declaración.

Para simplificar la especificación y el compilador, propongo que este literal de tupla se analice como una tupla de dos elementos dondequiera que aparezca (ya sea o no aparece en el lado izquierdo de una asignación). Esto sería un resultado natural de la desambiguación descrita en la sección anterior.

### <a name="pattern-matching"></a>Coincidencia de patrones

La coincidencia de patrones introduce un nuevo contexto en el que se produce la ambigüedad del tipo de expresión. Anteriormente, el lado derecho de un operador `is` era un tipo. Ahora puede ser un tipo o una expresión, y si es un tipo, puede ir seguido de un identificador. Técnicamente, esto puede cambiar el significado del código existente:

```csharp
var x = e is T < A > B;
```

Esto podría analizarse en reglas de C# 6 como

```csharp
var x = ((e is T) < A) > B;
```

pero bajo bajo las reglas de C# 7 (con la desambiguación propuesta anteriormente) se analizaría como

```csharp
var x = e is T<A> B;
```

que declara una variable `B` de tipo `T<A>`. Afortunadamente, los compiladores nativo y Roslyn tienen un error por el que dan un error de sintaxis en el código de C# 6. Por lo tanto, este cambio importante concreto no es un problema.

La coincidencia de patrones introduce tokens adicionales que deben impulsar la resolución de ambigüedades al seleccionar un tipo. Los siguientes ejemplos de código de C# 6 válido existente se interrumpirían sin reglas de desambiguación adicionales:

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a>Cambio propuesto en la regla de desambiguación

Propongo revisar la especificación para cambiar la lista de tokens de ambigüedad de

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

to

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

Y, en ciertos contextos, tratamos el *identificador* como un token de ambigüedad. Estos contextos son los lugares en los que la secuencia de los tokens que se van a la ambigüedad está inmediatamente precedida por una de las palabras clave `is`, `case`o `out`, o cuando se analiza el primer elemento de un literal de tupla (en cuyo caso los tokens van precedidos de `(` o `:` y el identificador va seguido de un `,`) o de un elemento subsiguiente de

### <a name="modified-disambiguation-rule"></a>Regla de desambiguación modificada

La regla de desambiguación revisada sería similar a la siguiente

> Si se puede analizar una secuencia de tokens (en contexto) como un *nombre simple* (§ 7.6.3), *acceso a miembro* (§ 7.6.5) o *acceso a miembro de puntero* (§ 18.5.2) que termina con una lista de *argumentos de tipo* (§ 4.4.1), se examina el token inmediatamente posterior al cierre `>` token para ver si es
> - Uno de `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; de
> - Uno de los operadores relacionales `<  >  <=  >=  is as`; de
> - Una palabra clave de consulta contextual que aparece dentro de una expresión de consulta; de
> - En ciertos contextos, tratamos el *identificador* como un token de ambigüedad. Estos contextos son donde la secuencia de tokens que se va a desambigüedadr está inmediatamente precedida por una de las palabras clave `is`, `case` o `out`, o bien al analizar el primer elemento de un literal de tupla (en cuyo caso los tokens van precedidos de `(` o `:` y el identificador va seguido de un `,`) o un elemento subsiguiente de un literal de tupla
> 
> Si el token siguiente se encuentra entre esta lista o un identificador en un contexto de este tipo, la *lista de argumentos de tipo* se conserva como parte del *nombre simple*, acceso a *miembros* o puntero a *miembro de puntero* , y se descarta cualquier otro posible análisis de la secuencia de tokens.  De lo contrario, la *lista de argumentos de tipo* no se considera parte del acceso *simple*, de acceso a *miembros* o de *miembro de puntero*, incluso si no hay otro posible análisis de la secuencia de tokens. Tenga en cuenta que estas reglas no se aplican al analizar una *lista de argumentos de tipo* en un nombre de espacio de *nombres o-tipo* (§ 3,8).

### <a name="breaking-changes-due-to-this-proposal"></a>Cambios importantes debidos a esta propuesta

No se conoce ningún cambio importante debido a esta regla de desambiguación propuesta.

### <a name="interesting-examples"></a>Ejemplos interesantes

Estos son algunos resultados interesantes de estas reglas de desambiguación:

La expresión `(A < B, C > D)` es una tupla con dos elementos, cada una de las cuales es una comparación.

La expresión `(A<B,C> D, E)` es una tupla con dos elementos, el primero de los cuales es una expresión de declaración.

La `M(A < B, C > D, E)` de invocación tiene tres argumentos.

La `M(out A<B,C> D, E)` de invocación tiene dos argumentos, el primero de los cuales es una declaración de `out`.

La expresión `e is A<B> C` utiliza una expresión de declaración.

La etiqueta Case `case A<B> C:` usa una expresión de declaración.

## <a name="some-examples-of-pattern-matching"></a>Algunos ejemplos de coincidencia de patrones

### <a name="is-as"></a>Es como

Podemos reemplazar la expresión

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

Con el aspecto más conciso y directo

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a>Prueba de valores NULL

Podemos reemplazar la expresión

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

Con el aspecto más conciso y directo

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a>Simplificación aritmética

Supongamos que definimos un conjunto de tipos recursivos para representar expresiones (según una propuesta independiente):

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

Ahora se puede definir una función para calcular el derivado (no reducido) de una expresión:

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

Un Simplificador de expresiones muestra patrones posicionales:

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
