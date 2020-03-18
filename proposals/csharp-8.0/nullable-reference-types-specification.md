---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2020
ms.locfileid: "79484109"
---
# <a name="nullable-reference-types-specification"></a>Especificación de tipos de referencia que aceptan valores NULL

Se trata de un trabajo en curso; faltan varias partes o están incompletas. ***

## <a name="syntax"></a>Sintaxis

### <a name="nullable-reference-types"></a>Tipos de referencia que aceptan valores NULL

Los tipos de referencia que aceptan valores NULL tienen la misma sintaxis `T?` que la forma abreviada de tipos de valor que aceptan valores NULL, pero no tienen una forma larga correspondiente.

Para los fines de la especificación, se cambia el nombre de la producción de `nullable_type` actual a `nullable_value_type`y se agrega una `nullable_reference_type` producción:

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

El `non_nullable_reference_type` en un `nullable_reference_type` debe ser un tipo de referencia que no acepte valores NULL (clase, interfaz, delegado o matriz) o un parámetro de tipo restringido para ser un tipo de referencia que no acepta valores NULL (a través de la restricción `class` o una clase distinta de `object`).

Los tipos de referencia que aceptan valores NULL no pueden aparecer en las siguientes posiciones:

- como una clase base o una interfaz
- como receptor de una `member_access`
- como `type` en un `object_creation_expression`
- como `delegate_type` en un `delegate_creation_expression`
- como `type` en un `is_expression`, un `catch_clause` o un `type_pattern`
- como `interface` en un nombre de miembro de interfaz completo

Se proporciona una advertencia en un `nullable_reference_type` donde se deshabilita el contexto de anotación que acepta valores NULL.

### <a name="nullable-class-constraint"></a>Restricción de clase que acepta valores NULL

La restricción `class` tiene un homólogo que acepta valores NULL `class?`:

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a>El operador null-permisivo

El operador de `!` posterior a la corrección se denomina operador permisivo nulo.

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

El `primary_expression` debe ser de un tipo de referencia.  

El operador de `!` postfijo no tiene ningún efecto en tiempo de ejecución, sino que se evalúa como el resultado de la expresión subyacente. Su único rol es cambiar el estado null de la expresión y limitar las advertencias que se dan en su uso.

### <a name="nullable-implicitly-typed-local-variables"></a>variables locales con tipo implícito que aceptan valores NULL

`var` deduce un tipo anotado para los tipos de referencia.
Por ejemplo, en `var s = "";` el `var` se deduce como `string?`.

### <a name="nullable-compiler-directives"></a>Directivas de compilador que aceptan valores NULL

las directivas de `#nullable` controlan la anotación y los contextos de advertencia que aceptan valores NULL.

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

las directivas de `#pragma warning` se expanden para permitir cambiar el contexto de advertencia que acepta valores NULL y permitir que se habiliten advertencias individuales en incluso cuando están deshabilitadas de forma predeterminada:

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

Tenga en cuenta que el nuevo formulario de `pragma_warning_body` usa `nullable_action`, no `warning_action`.

## <a name="nullable-contexts"></a>Contextos que aceptan valores NULL

Cada línea de código fuente tiene un *contexto de anotación que acepta valores NULL* y un *contexto de advertencia que acepta valores NULL*. Estos controlan si las anotaciones que aceptan valores NULL tienen efecto y si se proporcionan advertencias de nulabilidad. El contexto de anotación de una línea determinada está *deshabilitado* o *habilitado*. El contexto de advertencia de una línea determinada está *deshabilitado* o *habilitado*.

Ambos contextos se pueden especificar en el nivel de proyecto (fuera C# del código fuente) o en cualquier lugar dentro de un archivo de código fuente a través de `#nullable` directivas de preprocesador. Si no se proporciona ninguna configuración de nivel de proyecto, el valor predeterminado es que ambos contextos estén *deshabilitados*.

La Directiva `#nullable` controla la anotación y los contextos de advertencia dentro del texto de origen y tienen prioridad sobre la configuración de nivel de proyecto.

Una Directiva establece los contextos que controla para las siguientes líneas de código, hasta que otra directiva la invalida o hasta el final del archivo de código fuente.

El efecto de las directivas es el siguiente:

- `#nullable disable`: establece la anotación y los contextos de advertencia que aceptan valores NULL en *deshabilitado* .
- `#nullable enable`: establece la anotación y los contextos de advertencia que aceptan valores NULL en *habilitado*
- `#nullable restore`: restaura los contextos de advertencia y anotación que aceptan valores NULL a la configuración del proyecto.
- `#nullable disable annotations`: establece el contexto de anotación que acepta valores NULL en *deshabilitado*
- `#nullable enable annotations`: establece el contexto de anotación que acepta valores NULL en *habilitado*
- `#nullable restore annotations`: restaura el contexto de anotación que acepta valores NULL a la configuración del proyecto
- `#nullable disable warnings`: establece el contexto de advertencia que acepta valores NULL en *deshabilitado*
- `#nullable enable warnings`: establece el contexto de advertencia que acepta valores NULL en *habilitado*
- `#nullable restore warnings`: restaura el contexto de advertencia que acepta valores NULL a la configuración del proyecto

## <a name="nullability-of-types"></a>Nulabilidad de tipos

Un tipo determinado puede tener uno de cuatro nullabilities: *desconocen*, not *Nullable*, *Nullable* y *Unknown*. 

Los tipos que no *aceptan valores NULL* y los *desconocidos* pueden producir advertencias si se les asigna un valor de `null` potencial. Sin embargo, los tipos *desconocen* y que *aceptan valores NULL* son "*asignable null*" y pueden tener `null` valores asignados sin advertencias. 

Los tipos *desconocen* y que no *aceptan valores NULL* se pueden desreferenciar o asignar sin advertencias. Sin embargo, los valores de los tipos que *aceptan valores NULL* y los *desconocidos* son "*produjeron valores NULL*" y pueden provocar advertencias al desreferenciarse o asignarse sin una comprobación nula adecuada. 

El *Estado null predeterminado* de un tipo de rendimiento nulo es "quizás null". El estado Null predeterminado de un tipo de rendimiento que no es NULL es "not null".

El tipo de tipo y el contexto de anotación que acepta valores NULL que tiene lugar en determina su nulabilidad:

- Un tipo de valor que no acepta valores NULL `S` siempre es no *acepta valores NULL*
- Un tipo de valor que acepta valores NULL `S?` siempre *admite valores NULL*
- Un tipo de referencia sin anotar `C` en un contexto de anotación *deshabilitado* es *desconocen*
- Un tipo de referencia sin anotar `C` en un contexto de anotación *habilitado* no *admite valores NULL*
- Un tipo de referencia que acepta valores NULL `C?` *admite valores NULL* (pero se puede producir una advertencia en un contexto de anotación *deshabilitado* )

Los parámetros de tipo también tienen en cuenta las restricciones:

- Un parámetro de tipo `T` en el que todas las restricciones (si existen) son tipos que producen valores NULL (*aceptan valores NULL* y son *desconocidos*) o se *desconoce* la restricción `class?`
- Un parámetro de tipo `T` donde al menos una restricción es *desconocen* o que no *admite valores NULL* , o una de las restricciones de `struct` o `class` es
    - *desconocen* en un contexto de anotación *deshabilitado*
    - no *acepta valores NULL* en un contexto de anotación *habilitado*
- Un parámetro de tipo que acepta valores NULL `T?` donde al menos una de las restricciones de `T`es *desconocen* o que no *admite valores NULL* , o una de las restricciones `struct` o `class`, es
    - *acepta valores NULL* en un contexto de anotación *deshabilitado* (pero se produce una advertencia)
    - *acepta valores NULL* en un contexto de anotación *habilitado*

Para un parámetro de tipo `T`, `T?` solo se permite si se sabe que `T` es un tipo de valor o que se sabe que es un tipo de referencia.

### <a name="oblivious-vs-nonnullable"></a>Desconocen frente a no Nullable

Se considera que un `type` se produce en un contexto de anotación determinado cuando el último token del tipo está dentro de ese contexto.

Que un tipo de referencia determinado `C` en el código fuente se interprete como desconocen o que no admita valores NULL, depende del contexto de anotación de ese código fuente. Pero una vez establecido, se considera parte de ese tipo y "viaja con él", por ejemplo, durante la sustitución de los argumentos de tipo genérico. Es como si hubiera una anotación como `?` en el tipo, pero invisible.

## <a name="constraints"></a>Restricciones

Los tipos de referencia que aceptan valores NULL se pueden usar como restricciones genéricas. Además `object` es válido ahora como una restricción explícita. La ausencia de una restricción es ahora equivalente a una restricción `object?` (en lugar de `object`), pero (a diferencia de `object` antes) `object?` no está prohibida como restricción explícita.

`class?` es una nueva restricción que denota "posible tipo de referencia que acepta valores NULL", mientras que `class` denota "tipo de referencia que no acepta valores NULL".

La nulabilidad de un argumento de tipo o de una restricción no afecta a si el tipo satisface la restricción, excepto en el caso de que ya sea el caso hoy (los tipos de valor que aceptan valores NULL no satisfacen la restricción `struct`). Sin embargo, si el argumento de tipo no satisface los requisitos de nulabilidad de la restricción, se puede proporcionar una advertencia.

## <a name="null-state-and-null-tracking"></a>Estado NULL y seguimiento nulo

Cada expresión de una ubicación de origen determinada tiene un *Estado null*, que indica si se considera que puede evaluarse como null. El estado NULL es "not null" o "maybe null". El estado NULL se usa para determinar si se debe proporcionar una advertencia sobre las conversiones y desreferenciaciones no seguras de NULL.

### <a name="null-tracking-for-variables"></a>Seguimiento de valores NULL para variables

En el caso de ciertas expresiones que indican variables o propiedades, se realiza un seguimiento del estado Null entre las repeticiones, en función de las asignaciones a ellos, las pruebas realizadas en ellos y el flujo de control entre ellos. Esto es similar a la forma en que se realiza el seguimiento de la asignación definitiva para las variables. Las expresiones de las que se ha realizado un seguimiento son las siguientes:

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

Donde los identificadores denotan campos o propiedades.

***Describir las transiciones de estado Null similares a las asignaciones definitivas***

### <a name="null-state-for-expressions"></a>Estado null para expresiones

El estado null de una expresión se deriva de su forma y tipo, y del estado null de las variables implicadas en él.

### <a name="literals"></a>Literales

El estado null de un literal `null` es "maybe null". El estado null de un literal `default` que se está convirtiendo en un tipo que se sabe que no es un tipo de valor que no acepta valores NULL es "quizás null". El estado null de cualquier otro literal es "not null".

### <a name="simple-names"></a>Nombres simples

Si un `simple_name` no se clasifica como un valor, su estado NULL es "not null". En caso contrario, se trata de una expresión de la que se realiza un seguimiento y su estado NULL es el estado NULL del que se realiza el seguimiento en esta ubicación de origen.

### <a name="member-access"></a>Acceso a miembros

Si un `member_access` no se clasifica como un valor, su estado NULL es "not null". De lo contrario, si se trata de una expresión de la que se realiza un seguimiento, su estado NULL es el estado NULL del que se realiza el seguimiento en esta ubicación de origen. De lo contrario, su estado NULL es el estado Null predeterminado de su tipo.

### <a name="invocation-expressions"></a>Expresiones de invocación

Si un `invocation_expression` invoca a un miembro declarado con uno o varios atributos para un comportamiento especial nulo, el estado NULL se determina por esos atributos. De lo contrario, el estado null de la expresión es el estado Null predeterminado de su tipo.

### <a name="element-access"></a>Acceso a elementos

Si un `element_access` invoca un indexador que se declara con uno o más atributos para un comportamiento especial nulo, el estado NULL se determina por esos atributos. De lo contrario, el estado null de la expresión es el estado Null predeterminado de su tipo.

### <a name="base-access"></a>Acceso base

Si `B` denota el tipo base del tipo envolvente, `base.I` tiene el mismo estado null que `((B)this).I` y `base[E]` tiene el mismo estado null que `((B)this)[E]`.

### <a name="default-expressions"></a>Expresiones predeterminadas

`default(T)` tiene el estado null "nonnull" si se sabe que `T` es un tipo de valor que no acepta valores NULL. En caso contrario, tiene el estado null "maybe null".

### <a name="null-conditional-expressions"></a>Expresiones condicionales null

Una `null_conditional_expression` tiene el estado null "maybe null".

### <a name="cast-expressions"></a>Expresiones de conversión

Si una expresión de conversión `(T)E` invoca una conversión definida por el usuario, el estado null de la expresión es el estado Null predeterminado para su tipo. De lo contrario, si `T` tiene un rendimiento nulo (que*acepta valores* null o es *desconocido*), el estado NULL es "maybe null". En caso contrario, el estado NULL es el mismo que el estado null de `E`.

### <a name="await-expressions"></a>Expresiones Await

El estado null de `await E` es el estado Null predeterminado de su tipo.

### <a name="the-as-operator"></a>El operador `as`

Una expresión `as` tiene el estado null "maybe null".

### <a name="the-null-coalescing-operator"></a>Operador de uso combinado de null

`E1 ?? E2` tiene el mismo estado null que `E2`

### <a name="the-conditional-operator"></a>El operador condicional

El estado null de `E1 ? E2 : E3` es "not null" si el estado null de `E2` y `E3` es "not null". En caso contrario, es "quizás null".

### <a name="query-expressions"></a>Expresiones de consulta

El estado null de una expresión de consulta es el estado Null predeterminado de su tipo.

### <a name="assignment-operators"></a>Operadores de asignación

`E1 = E2` y `E1 op= E2` tienen el mismo estado null que `E2` una vez aplicadas las conversiones implícitas.

### <a name="unary-and-binary-operators"></a>Operadores unarios y binarios

Si un operador unario o binario invoca a un operador definido por el usuario que se declara con uno o más atributos para un comportamiento especial nulo, los atributos determinan el estado null. De lo contrario, el estado null de la expresión es el estado Null predeterminado de su tipo.

***¿Algo especial de hacer para `+` binarios en cadenas y delegados?***

### <a name="expressions-that-propagate-null-state"></a>Expresiones que propagan el estado null

`(E)`, `checked(E)` y `unchecked(E)` tienen el mismo estado null que `E`.

### <a name="expressions-that-are-never-null"></a>Expresiones que nunca son NULL

El estado null de los siguientes formatos de expresión siempre es "not null":

- `this` access
- cadenas interpoladas
- Expresiones `new` (expresiones de objeto, delegado, objeto anónimo y creación de matriz)
- Expresiones `typeof`
- Expresiones `nameof`
- funciones anónimas (métodos anónimos y expresiones lambda)
- Expresiones permisivo nulas
- Expresiones `is`

## <a name="type-inference"></a>Inferencia de tipos

### <a name="type-inference-for-var"></a>Inferencia de tipos para `var`

El tipo deducido para las variables locales declaradas con `var` es informado por el estado null de la expresión de inicialización.

```csharp
var x = E;
```

Si el tipo de `E` es un tipo de referencia que acepta valores NULL `C?` y el estado null de `E` es "not null", el tipo deducido para `x` es `C`. De lo contrario, el tipo deducido es el tipo de `E`.

La nulabilidad del tipo deducido para `x` se determina como se describió anteriormente, basándose en el contexto de la anotación del `var`, como si el tipo se hubiera proporcionado explícitamente en esa posición.

### <a name="type-inference-for-var"></a>Inferencia de tipos para `var?`

El tipo deducido para las variables locales declaradas con `var?` es independiente del estado null de la expresión de inicialización.

```csharp
var? x = E;
```

Si el tipo `T` de `E` es un tipo de valor que acepta valores NULL o un tipo de referencia que acepta valores NULL, se `T`el tipo deducido para `x`. De lo contrario, si `T` es un tipo de valor que no acepta valores NULL `S` se `S?`el tipo deducido. De lo contrario, si `T` es un tipo de referencia que no admite valores NULL `C` se `C?`el tipo deducido. De lo contrario, la declaración no es válida.

La nulabilidad del tipo deducido para `x` siempre *admite valores NULL*.

### <a name="generic-type-inference"></a>Inferencia de tipo genérico

La inferencia de tipos genéricos se ha mejorado para ayudar a decidir si los tipos de referencia deducidos deben admitir valores NULL o no. Se trata de un mejor esfuerzo y no produce advertencias en sí mismo, pero puede dar lugar a advertencias que aceptan valores NULL cuando los tipos deducidos de la sobrecarga seleccionada se aplican a los argumentos.

La inferencia de tipos no se basa en el contexto de anotación de los tipos entrantes. En su lugar, se deduce una `type` que adquiere su propio contexto de anotación desde donde "habría sido" si se hubiera expresado explícitamente. Esto subraya el rol de inferencia de tipos por comodidad para lo que podría haber escrito.

Más concretamente, el contexto de anotación para un argumento de tipo deducido es el contexto del token que habría seguido de la `<...>` lista de parámetros de tipo, si hubiera sido uno; es decir, el nombre del método genérico que se va a llamar. En el caso de las expresiones de consulta que se traducen en llamadas de este tipo, el contexto se toma de la palabra clave contextual inicial de la cláusula de consulta a partir de la cual se genera la llamada.

### <a name="the-first-phase"></a>La primera fase

Los tipos de referencia que aceptan valores NULL fluyen en los límites de las expresiones iniciales, como se describe a continuación. Además, se introducen dos nuevos tipos de límites, es decir, `null` y `default`. Su finalidad es llevar a cabo las repeticiones de `null` o `default` en las expresiones de entrada, lo que puede hacer que un tipo deducido acepte valores NULL, incluso cuando no sea así. Esto funciona incluso para los tipos de *valor* que aceptan valores NULL, que se han mejorado para recoger la "nulación" en el proceso de inferencia.

La determinación de los límites que se van a agregar en la primera fase se ha mejorado como se indica a continuación:

Si un `Ei` de argumento tiene un tipo de referencia, el tipo `U` utilizado para la inferencia depende del estado null de `Ei` así como de su tipo declarado:
- Si el tipo declarado es un tipo de referencia que no admite valores NULL `U0` o un tipo de referencia que acepta valores NULL `U0?` entonces
    - Si el estado null de `Ei` es "not null", `U` se `U0`
    - Si el estado nulo de `Ei` es "quizás null", `U` se `U0?`
- De lo contrario, si `Ei` tiene un tipo declarado, `U` es ese tipo.
- De lo contrario, si se `null` `Ei`, `U` es el enlace especial `null`
- De lo contrario, si se `default` `Ei`, `U` es el enlace especial `default`
- En caso contrario, no se realiza ninguna inferencia.

### <a name="exact-upper-bound-and-lower-bound-inferences"></a>Inferencias exactas, de límite superior y de límite inferior

En las inferencias *del* tipo `U` *al* tipo `V`, si `V` es un tipo de referencia que acepta valores NULL `V0?`, se usa `V0` en lugar de `V` en las cláusulas siguientes.
- Si `V` es una de las variables de tipo sin corregir, `U` se agrega como un límite inferior, superior o inferior como antes.
- De lo contrario, si `U` es `null` o `default`, no se realiza ninguna inferencia.
- De lo contrario, si `U` es un tipo de referencia que acepta valores NULL `U0?`, se usa `U0` en lugar de `U` en las cláusulas posteriores.

La esencia es que la nulabilidad que pertenece directamente a una de las variables de tipo sin Fixed se conserva en sus límites. Por otra parte, para las inferencias que se recorren más en los tipos de origen y de destino, se omite la nulabilidad. Puede o no coincidir, pero si no es así, se emitirá una advertencia más adelante si se elige la sobrecarga y se aplica.

### <a name="fixing"></a>Corrección de

Actualmente, la especificación no realiza un buen trabajo de describir lo que sucede cuando varios límites son convertibles entre sí, pero son diferentes. Esto puede ocurrir entre `object` y `dynamic`, entre tipos de tupla que solo difieren en los nombres de elemento, entre los tipos construidos en él y ahora también entre `C` y `C?` para los tipos de referencia.

Además, debemos propagar la "nulación" de las expresiones de entrada al tipo de resultado. 

Para controlar estos agregamos más fases para corregir, que ahora es:

1. Recopile todos los tipos de todos los límites como candidatos, quitando `?` de todos los tipos de referencia que aceptan valores NULL
2. Eliminación de candidatos en función de los requisitos de los límites exactos, inferiores y superiores (manteniendo `null` y `default` límites)
3. Eliminación de candidatos que no tienen una conversión implícita a todos los demás candidatos
4. Si los candidatos restantes no tienen conversiones de identidad entre sí, se produce un error en la inferencia de tipos
5. *Combine* los candidatos restantes como se describe a continuación
6. Si el candidato resultante es un tipo de referencia o un tipo de valor que no acepta valores NULL y *todos* los límites exactos o *alguno* de los límites inferiores son tipos de valor que aceptan valores NULL, tipos de referencia que aceptan valores null, `null` o `default`, se agrega `?` al candidato resultante, convirtiéndolo en un tipo de valor que acepta valores NULL o un tipo de referencia.

La *combinación* se describe entre dos tipos candidatos. Es transitiva y conmutable, por lo que los candidatos se pueden combinar en cualquier orden con el mismo resultado final. No se define si los dos tipos candidatos no son una identidad convertible entre sí.

La función *Merge* toma dos tipos candidatos y una dirección ( *+* o *-* ):

- *Merge*(`T`, `T`, *d*) = t
- *Merge*(`S`, `T?`, *+* ) = *Merge*(`S?`, `T`, *+* ) = *Merge*(`S`, `T`, *+* )`?`
- *Merge*(`S`, `T?`, *-* ) = *Merge*(`S?`, `T`, *-* ) = *Merge*(`S`, `T`, *-* )
- *Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+* ) = `C<`*combinar*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *DN*)`>`, *donde*
    - `di` =  *+* si el parámetro de tipo "th" `i`de `C<...>` es covariante
    - `di` =  *-* si el parámetro de tipo "th" `i`de `C<...>` es compensa-o invariable
- *Merge*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-* ) = `C<`*combinar*(`S1`, `T1`, *d1*)`,...,`*Merge*(`Sn`, `Tn`, *DN*)`>`, *donde*
    - `di` =  *-* si el parámetro de tipo "th" `i`de `C<...>` es covariante
    - `di` =  *+* si el parámetro de tipo "th" `i`de `C<...>` es compensa-o invariable
- *Merge*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*combinar*(`S1`, `T1`, *d*)`n1,...,`*combinar*(`Sn`, `Tn`, *d*) `nn)`, *donde*
    - `ni` no está presente si `si` y `ti` difieren, o si ambos están ausentes
    - `ni` es `si` si `si` y `ti` son iguales
- *Merge*(`object`, `dynamic`) = *Merge*(`dynamic`, `object`) = `dynamic`

## <a name="warnings"></a>Advertencias

### <a name="potential-null-assignment"></a>Posible asignación null

### <a name="potential-null-dereference"></a>Desreferenciación potencial null

### <a name="constraint-nullability-mismatch"></a>No coincide la nulabilidad de restricciones

### <a name="nullable-types-in-disabled-annotation-context"></a>Tipos que aceptan valores NULL en el contexto de anotación deshabilitado

## <a name="attributes-for-special-null-behavior"></a>Atributos para un comportamiento especial nulo


