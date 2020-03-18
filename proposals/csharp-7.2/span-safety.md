---
ms.openlocfilehash: 6088a5cd41926d828013f1b8e5736fd2b7939e44
ms.sourcegitcommit: da452002c3f472165a0e1fa7759f494cc703ae31
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "79483965"
---
# <a name="compile-time-enforcement-of-safety-for-ref-like-types"></a>Aplicación del tiempo de compilación de seguridad para los tipos de referencia

## <a name="introduction"></a>Introducción

La razón principal de las reglas de seguridad adicionales cuando se trabaja con tipos como `Span<T>` y `ReadOnlySpan<T>` es que estos tipos se deben limitar a la pila de ejecución.
 
Hay dos razones por las que `Span<T>` y tipos similares deben ser tipos de solo pila.

1. `Span<T>` es semánticamente un struct que contiene una referencia y un intervalo `(ref T data, int length)`. Independientemente de la implementación real, las escrituras en dicho struct no serían atómicas. El "desgarro" simultáneo de este struct provocaría la posibilidad de `length` no coincidir con el `data`, lo que provocaría accesos fuera de intervalo e infracciones de seguridad de tipos, lo que en última instancia podría provocar daños en el montón GC en código aparentemente "seguro".
2. Algunas implementaciones de `Span<T>` contienen literalmente un puntero administrado en uno de sus campos. Los punteros administrados no se admiten como campos de objetos de montón y el código que administra para colocar un puntero administrado en el montón GC normalmente se bloquea en tiempo JIT.

Todos los problemas anteriores se solucionarían si las instancias de `Span<T>` están restringidas a existir solo en la pila de ejecución. 

Se produce un problema adicional debido a la composición. Por lo general, sería conveniente crear tipos de datos más complejos que incrustaran `Span<T>` y `ReadOnlySpan<T>` instancias. Estos tipos compuestos deben ser Structs y compartir todos los peligros y los requisitos de `Span<T>`. Como resultado, las reglas de seguridad que se describen aquí deben verse como corresponda a toda la gama de **_tipos de referencia_** .

La [especificación del lenguaje de borrador](#draft-language-specification) está pensada para garantizar que los valores de un tipo de referencia solo se producen en la pila.

## <a name="generalized-ref-like-types-in-source-code"></a>Tipos de `ref-like` generalizados en el código fuente

`ref-like` Structs se marcan explícitamente en el código fuente mediante el modificador `ref`:

```csharp
ref struct TwoSpans<T>
{
    // can have ref-like instance fields
    public Span<T> first;
    public Span<T> second;
} 

// error: arrays of ref-like types are not allowed. 
TwoSpans<T>[] arr = null;

```

Designar un struct como referencia permite que el struct tenga campos de instancia de tipo REF y también hará que todos los requisitos de los tipos de referencia se apliquen a la estructura. 

## <a name="metadata-representation-or-ref-like-structs"></a>Representación de metadatos o Structs de tipo Ref

Las estructuras de tipo REF se marcarán con el atributo **System. Runtime. CompilerServices. IsRefLikeAttribute** .

El atributo se agregará a las bibliotecas base comunes, como `mscorlib`. En caso de que el atributo no esté disponible, el compilador generará uno interno de forma similar a otros atributos incrustados a petición, como `IsReadOnlyAttribute`.

Se realizará una medida adicional para evitar el uso de Structs similares en los compiladores que no están familiarizados con las reglas de seguridad C# (esto incluye los compiladores anteriores a aquél en el que se implementa esta característica). 

Si no hay ninguna otra buena alternativa que funcione en los compiladores antiguos sin servicio, se agregará un `Obsolete` atributo con una cadena conocida a todos los Structs de tipo Ref. Los compiladores que sepan usar tipos de tipo REF no tendrán en cuenta esta forma determinada de `Obsolete`.

Una representación de metadatos típica:

```csharp
    [IsRefLike]
    [Obsolete("Types with embedded references are not supported in this version of your compiler.")]
    public struct TwoSpans<T>
    {
       // . . . .
    }
```

Nota: no es el objetivo de hacerlo para que cualquier uso de tipos de tipo REF en los compiladores anteriores produzca un error 100%. Esto es difícil de lograr y no es estrictamente necesario. Por ejemplo, siempre sería una manera de evitar el `Obsolete` mediante código dinámico o, por ejemplo, crear una matriz de tipos de tipo REF a través de la reflexión.

En concreto, si el usuario desea colocar realmente un `Obsolete` o `Deprecated` atributo en un tipo de referencia, no tendrá ninguna opción aparte de no emitir el predefinido, ya que `Obsolete` atributo no se puede aplicar más de una vez.  

## <a name="examples"></a>Ejemplos:

```csharp
SpanLikeType M1(ref SpanLikeType x, Span<byte> y)
{
    // this is all valid, unconcerned with stack-referring stuff
    var local = new SpanLikeType(y);
    x = local;
    return x;
}

void Test1(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    // this is allowed
    stackReferring2 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    stackReferring2 = M1(ref param1, stackReferring1);

    // this is NOT allowed
    param1 = M1(ref stackReferring2, stackReferring1);

    // this is NOT allowed
    param2 = stackReferring1.Slice(10);

    // this is allowed
    param1 = new SpanLikeType(param2);

    // this is allowed
    stackReferring2 = param1;
}

ref SpanLikeType M2(ref SpanLikeType x)
{
    return ref x;
}

ref SpanLikeType Test2(ref SpanLikeType param1, Span<byte> param2)
{
    Span<byte> stackReferring1 = stackalloc byte[10];
    var stackReferring2 = new SpanLikeType(stackReferring1);

    ref var stackReferring3 = M2(ref stackReferring2);

    // this is allowed
    stackReferring3 = M1(ref stackReferring2, stackReferring1);

    // this is allowed
    M2(ref stackReferring3) = stackReferring2;

    // this is NOT allowed
    M1(ref param1) = stackReferring2;

    // this is NOT allowed
    param1 = stackReferring3;

    // this is NOT allowed
    return ref stackReferring3;

    // this is allowed
    return ref param1;
}

```

----------------

## <a name="draft-language-specification"></a>Especificación del lenguaje borrador

A continuación se describe un conjunto de reglas de seguridad para los tipos de referencia (`ref struct`s) para asegurarse de que los valores de estos tipos solo se producen en la pila. Sería posible un conjunto de reglas de seguridad diferente y más sencillo si las variables locales no se pueden pasar por referencia. Esta especificación también permitiría la reasignación segura de variables locales de referencia.

### <a name="overview"></a>Información general

Asociamos con cada expresión en tiempo de compilación el concepto del ámbito al que se permite el escape de la expresión, "Safe-to-escape". Del mismo modo, para cada valor l, se mantiene un concepto del ámbito al que se permite que se escape una referencia, "ref-Safe-to-escape". Para una expresión lvalue determinada, pueden ser diferentes.

Son análogos a "Safe to Return" de la característica Ref locals, pero es más específica. Cuando el valor de "Safe-to-Return" de una expresión solo registra si (o no) puede escapar el método envolvente en su conjunto, los registros de seguridad a escape a los que puede escapar (el ámbito en el que podría no escapar). El mecanismo de seguridad básica se aplica como se indica a continuación. Dada una asignación desde una expresión E1 con un ámbito de salida a escape S1, a una expresión (lvalue) E2 con un ámbito de seguridad a escape S2, es un error si S2 es un ámbito más amplio que S1. Por construcción, los dos ámbitos S1 y S2 están en una relación de anidamiento, ya que una expresión válida siempre es segura para devolver desde algún ámbito que incluya la expresión.

En el momento en que es suficiente, para el análisis, admitir solo dos ámbitos: externo al método y el ámbito de nivel superior del método. Esto se debe a que no se pueden crear valores de tipo REF con ámbitos internos y las variables locales de referencia no admiten la reasignación. Sin embargo, las reglas pueden admitir más de dos niveles de ámbito.

A continuación se indican las reglas precisas para calcular el estado de seguridad de la *devolución* de una expresión y las reglas que rigen la legalidad de las expresiones.

### <a name="ref-safe-to-escape"></a>Ref-Safe-to-escape

*Ref-Safe-to-escape* es un ámbito, que incluye una expresión lvalue, a la que es seguro para una referencia al valor l a la que se debe escapar. Si ese ámbito es el método completo, se indica que una referencia a lvalue es *segura para devolver* desde el método.

### <a name="safe-to-escape"></a>Safe-to-escape

*Safe-to-escape* es un ámbito, que incluye una expresión, a la que es seguro que el valor se escape. Si ese ámbito es el método completo, se indica que un valor es *seguro para volver* del método.

Una expresión cuyo tipo no es un tipo `ref struct` es *seguro para devolver* desde todo el método envolvente. En caso contrario, hacemos referencia a las reglas siguientes.

#### <a name="parameters"></a>Parámetros

Un valor l que designa un parámetro formal es *ref-Safe-to-escape* (por referencia) de la manera siguiente:
- Si el parámetro es un parámetro `ref`, `out`o `in`, es *ref-Safe-to-escape* del método completo (por ejemplo, mediante una instrucción `return ref`); casos
- Si el parámetro es el parámetro `this` de un tipo de estructura, es *ref-Safe-to-escape* en el ámbito de nivel superior del método (pero no en todo el método); [Ejemplo](#struct-this-escape) de
- De lo contrario, el parámetro es un parámetro de valor y es *ref-Safe-to-escape* al ámbito de nivel superior del método (pero no al propio método).

Una expresión que es un valor r que designa el uso de un parámetro formal es *Safe-to-escape* (por valor) desde el método completo (por ejemplo, mediante una instrucción `return`). Esto se aplica también al parámetro `this`.

#### <a name="locals"></a>Locals

Un valor l que designa una variable local es *ref-Safe-to-escape* (por referencia) de la manera siguiente:
- Si la variable es una variable de `ref`, su *ref-Safe-to-escape* se toma de la *referencia-Safe-to-* escape de la expresión de inicialización. casos
- La variable es *ref-Safe-to-escape* el ámbito en el que se declaró.

Una expresión que es un valor r que designa el uso de una variable local es *segura a escape* (por valor) de la manera siguiente:
- Pero la regla general anterior, una variable local cuyo tipo no es un tipo `ref struct` es *seguro para devolver* desde todo el método envolvente.
- Si la variable es una variable de iteración de un bucle `foreach`, el ámbito de *seguridad* de la variable de la variable es el mismo que *el de la* expresión del bucle `foreach`.
- Una variable local de `ref struct` tipo y no inicializado en el punto de declaración es *segura para devolver* desde todo el método de inclusión.
- De lo contrario, el tipo de la variable es un tipo de `ref struct`, y la declaración de la variable requiere un inicializador. El ámbito de *seguridad a escape* de la variable es el mismo *que el del* inicializador.

#### <a name="field-reference"></a>Referencia de campo

Un valor l que designa una referencia a un campo, `e.F`, es *ref-Safe-to-escape* (por referencia) de la manera siguiente:
- Si `e` es de un tipo de referencia, es *ref-Safe-to-escape* del método completo; casos
- Si `e` es de un tipo de valor, su *ref-Safe-to-escape* se toma del *parámetro ref-Safe-to-escape* de `e`.

Un valor r que designe una referencia a un campo, `e.F`, tiene un ámbito de *seguridad a escape* que es el mismo que el de la `e`de *seguridad* .

#### <a name="operators-including-"></a>Operadores que incluyen `?:`

La aplicación de un operador definido por el usuario se trata como una invocación de método.

Para un operador que produce un valor r, como `e1 + e2` o `c ? e1 : e2`, el *valor de Safe-to-escape* del resultado es el ámbito más estrecho entre el *valor de Safe-to-escape* de los operandos del operador.  Como consecuencia, para un operador unario que produce un valor r, como `+e`, el valor de *Safe-to-escape* del resultado es el carácter de *escape seguro* del operando.

Para un operador que produce un valor l, como `c ? ref e1 : ref e2`
- *ref-Safe-to-escape* del resultado es el ámbito más restringido entre *ref-Safe-to-escape* de los operandos del operador.
- el valor de *Safe-to-escape* de los operandos debe coincidir, y es el *valor de Safe-to-escape* del valor l resultante.

#### <a name="method-invocation"></a>Invocación de método

Un valor l resultante de una invocación de método de devolución de referencia `e1.M(e2, ...)` es *ref-Safe-to-escape* el menor de los siguientes ámbitos:
- Todo el método envolvente
- *ref-Safe-to-escape* de todas las expresiones de argumentos `ref` y `out` (sin incluir el receptor)
- Para cada `in` parámetro del método, si hay una expresión correspondiente que es un valor l, su *referencia-Safe-to-escape*, de lo contrario, el ámbito de inclusión más próximo
- el carácter de *escape seguro* de todas las expresiones de argumentos (incluido el receptor)

> Nota: la última viñeta es necesaria para controlar el código como
> ```csharp
> var sp = new Span(...)
> return ref sp[0];
> ```
> or
> ```csharp
> return ref M(sp, 0);
> ```

Un valor r que resulta de una invocación de método `e1.M(e2, ...)` es *seguro para el escape* de la menor de los siguientes ámbitos:
- Todo el método envolvente
- el carácter de *escape seguro* de todas las expresiones de argumentos (incluido el receptor)

#### <a name="an-rvalue"></a>Un valor r
Un valor r es *ref-Safe-to-escape* del ámbito de inclusión más cercano. Esto sucede, por ejemplo, en una invocación como `M(ref d.Length)` donde `d` es de tipo `dynamic`. También es coherente con (y quizás subsumes) nuestro control de los argumentos correspondientes a los parámetros de `in`. *

#### <a name="property-invocations"></a>Invocaciones de propiedad

Una invocación de propiedad (ya sea `get` o `set`) se trata como una invocación de método del método subyacente por parte de las reglas anteriores.

#### `stackalloc`

Una expresión stackalloc es un valor r que es *seguro* para el ámbito de nivel superior del método, pero no del propio método completo.

#### <a name="constructor-invocations"></a>Invocaciones del constructor

Una expresión `new` que invoca un constructor obedece a las mismas reglas que una invocación de método que se considera que devuelve el tipo que se está construyendo.

Además, el valor de *Safe-to-escape* no es más ancho que el más pequeño *de los argumentos* y operandos de la expresión de inicializador de objeto, de forma recursiva, si el inicializador está presente. 

#### <a name="span-constructor"></a>Span (constructor)
El lenguaje se basa en `Span<T>` no tener un constructor con el formato siguiente:

```csharp
void Example(ref int x)
{
    // Create a span of length one
    var span = new Span<int>(ref x); 
}
```

Este tipo de constructor hace `Span<T>` que se usan como campos indistinguibles de un campo `ref`. Las reglas de seguridad descritas en este documento dependen de `ref` campos que no sean una C#construcción válida en, o .net.

#### <a name="default-expressions"></a>Expresiones `default`

Una expresión `default` es *segura a escape* desde el método de inclusión completo.

## <a name="language-constraints"></a>Restricciones de lenguaje

Queremos asegurarnos de que no haya ningún `ref` variable local y que no haya ninguna variable de `ref struct` tipo, que haga referencia a la memoria de la pila o a las variables que ya no estén activas. Por lo tanto, tenemos las siguientes restricciones de lenguaje:

- No se puede elevar un parámetro ref ni una variable local de tipo REF ni un parámetro o una variable local de un tipo `ref struct` en una función lambda o local.

- Ni un parámetro ref ni un parámetro de un tipo `ref struct` pueden ser un argumento en un método iterador o un método `async`.

- Ni una variable local de tipo REF ni una variable local de un tipo `ref struct` pueden estar en el ámbito en el punto de una instrucción `yield return` o una expresión `await`.

- Un tipo de `ref struct` no se puede usar como un argumento de tipo o como un tipo de elemento en un tipo de tupla.

- Un tipo de `ref struct` no puede ser el tipo declarado de un campo, salvo que puede ser el tipo declarado de un campo de instancia de otro `ref struct`.

- Un tipo de `ref struct` no puede ser el tipo de elemento de una matriz.

- No se puede aplicar la conversión boxing a un valor de un tipo `ref struct`:
  - No hay ninguna conversión de un tipo de `ref struct` al tipo `object` o `System.ValueType`de tipo.
  - Un tipo de `ref struct` no se puede declarar para implementar ninguna interfaz
  - No se puede llamar a ningún método de instancia declarado en `object` o en `System.ValueType` pero que no se invalide en un tipo de `ref struct` con un receptor de ese tipo de `ref struct`.
  - No se puede capturar ningún método de instancia de un tipo de `ref struct` mediante la conversión de método a un tipo de delegado.

- En el caso de una `ref e1 = ref e2`de reasignación de referencia, el *parámetro ref-Safe-to-escape* de `e2` debe ser al menos tan ancho como el de la *referencia-Safe-to-escape* de `e1`.

- En el caso de una instrucción Ref Return `return ref e1`, el valor *ref-Safe-to-escape* de `e1` debe ser *ref-Safe-to-escape* desde el método completo. (TODO: ¿también necesitamos una regla que `e1` deba ser *segura para el escape* desde el método completo, o que sea redundante?)

- En el caso de una instrucción return `return e1`, el valor de *Safe-to-escape* de `e1` debe ser *seguro para* el método completo.

- En el caso de una `e1 = e2`de asignación, si el tipo de `e1` es un tipo de `ref struct`, el nivel de *escape seguro* de `e2` debe ser al menos tan ancho como un ámbito de *seguridad* de la `e1`.

- En el caso de una invocación de método si hay un `ref` o `out` argumento de un tipo `ref struct` (incluido el receptor), con *Safe-to-escape* E1, ningún argumento (incluido el receptor) puede tener un método *Safe-to-escape* más estrecho que E1. [Ejemplo](#method-arguments-must-match)

- Es posible que una función local o anónima no haga referencia a una variable local o a un parámetro de `ref struct` tipo declarado en un ámbito de inclusión.

> ***Problema abierto:*** Se necesita una regla que nos permita generar un error cuando se necesita sobrepasar un valor de la pila de un `ref struct` tipo en una expresión Await, por ejemplo en el código.
> ```csharp
> Foo(new Span<int>(...), await e2);
> ```

## <a name="explanations"></a>Explicaciones
Estas explicaciones y ejemplos ayudan a explicar por qué muchas de las reglas de seguridad anteriores existen.

### <a name="method-arguments-must-match"></a>Los argumentos de método deben coincidir
Al invocar un método en el que hay un `out`, `ref` parámetro que es un `ref struct` incluido el receptor y, a continuación, todos los `ref struct` deben tener la misma duración. Esto es necesario porque C# debe tomar todas sus decisiones sobre la seguridad de la duración en función de la información disponible en la firma del método y la duración de los valores en el sitio de la llamada. 

Cuando hay `ref` parámetros que se `ref struct` entonces hay posibilidad que podrían cambiar en torno a su contenido. Por lo tanto, en el sitio de llamada, debemos asegurarnos de que todos estos swaps **potenciales** sean compatibles. Si el lenguaje no aplicó, se permitirá un código incorrecto como el siguiente.

```csharp
void M1(ref Span<int> s1)
{
    Span<int> s2 = stackalloc int[1];
    Swap(ref s1, ref s2);
}

void Swap(ref Span<int> x, ref int Span<int> y)
{
    // This will effectively assign the stackalloc to the s1 parameter and allow it
    // to escape to the caller of M1
    ref x = ref y; 
}
```

La restricción en el receptor es necesaria porque aunque ninguno de sus contenidos es Ref-Safe-to-escape, puede almacenar los valores proporcionados. Esto significa que las duraciones no coinciden, por lo que podría crear una carencia de seguridad de tipos de la siguiente manera:

```csharp
ref struct S
{
    public Span<int> Span;

    public void Set(Span<int> span)
    {
        Span = span;
    }
}

void Broken(ref S s)
{
    Span<int> span = stackalloc int[1];

    // The result of a stackalloc is now stored in s.Span and escaped to the caller
    // of Broken
    s.Set(span); 
}
```

### <a name="struct-this-escape"></a>Struct este escape
Cuando llega a abarcar las reglas de seguridad, el valor `this` de un miembro de instancia se modela como un parámetro para el miembro. Ahora, para un `struct` el tipo de `this` realmente `ref S` en el que en una `class` simplemente es `S` (para los miembros de una `class / struct` denominada S). 

Todavía `this` tiene diferentes reglas de escape que otros parámetros de `ref`. En concreto, no es un carácter de escape de referencia mientras que otros parámetros son:

```csharp
ref struct S
{ 
    int Field;

    // Illegal because this isn't safe to escape as ref
    ref int Get() => ref Field;

    // Legal
    ref int GetParam(ref int p) => ref p;
}
```

La razón de esta restricción realmente tiene poco que hacer con `struct` invocación de miembros. Hay algunas reglas que deben realizarse con respecto a la invocación de miembros en `struct` miembros en los que el receptor es un valor r. Pero es muy accesible. 

La razón de esta restricción es realmente la invocación de la interfaz. En concreto, se refiere a si el siguiente ejemplo debe o no debe compilarse;

```csharp
interface I1
{
    ref int Get();
}

ref int Use<T>(T p)
    where T : I1
{
    return ref p.Get();
}
```

Considere el caso en el que se crea una instancia de `T` como un `struct`. Si el parámetro `this` es Ref-Safe-to-escape, la devolución de `p.Get` podría apuntar a la pila (en concreto, podría ser un campo dentro del tipo de instancia de `T`). Esto significa que el lenguaje no puede permitir que se compile este ejemplo, ya que podría devolver una `ref` a una ubicación de la pila. Por otro lado, si `this` no es Ref-Safe-to-escape, `p.Get` no puede hacer referencia a la pila y, por lo tanto, es seguro volver. 

Este es el motivo por el que la elusión de `this` en un `struct` es realmente una de las interfaces. Se puede realizar de forma absoluta en el trabajo, pero tiene una desventaja. Finalmente, el diseño apareció en favor de que las interfaces sean más flexibles. 

Sin embargo, es posible que esto se Relájese en el futuro. 

## <a name="future-considerations"></a>Consideraciones futuras

### <a name="length-one-spant-over-ref-values"></a>Longitud de un intervalo\<T > sobre valores de referencia
Aunque no es legal hoy en día hay casos en los que la creación de una longitud de una `Span<T>` instancia sobre un valor sería beneficiosa:

```csharp
void RefExample()
{
    int x = ...;

    // Today creating a length one Span<int> requires a stackalloc and a new 
    // local
    Span<int> span1 = stackalloc [] { x };
    Use(span1);
    x = span1[0]; 

    // Simpler to just allow length one span
    var span2 = new Span<int>(ref x);
    Use(span2);
}
```

Esta característica resulta más atractiva si se eliminan las restricciones en los [búferes de tamaño fijo](https://github.com/dotnet/csharplang/blob/master/proposals/fixed-sized-buffers.md) , ya que esto permitiría `Span<T>` instancias de una longitud incluso mayor. 

Si alguna vez es necesario bajar esta ruta de acceso, el lenguaje podría dar cabida a este caso asegurándose de que las instancias de `Span<T>` solo estaban orientadas hacia abajo. Es decir, solo estaban *seguros* para el ámbito en el que se crearon. Esto garantiza que el lenguaje nunca tenía que considerar un valor `ref` escapar un método a través de un `ref struct` devuelto o un campo de `ref struct`. Lo más probable es que también requiera cambios adicionales para reconocer tales constructores como la captura de un parámetro de `ref` de esta manera.
