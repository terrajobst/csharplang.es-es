---
ms.openlocfilehash: d162d4b6a489032dcdfca9094a39d88fd03d4013
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704093"
---
# <a name="delegates"></a>Delegados

Los delegados habilitan escenarios en los C++que otros lenguajes, como Pascal y modula, se han direccionado con punteros de función. A diferencia C++ de los punteros de función, sin embargo, los delegados están C++ totalmente orientados a objetos y, a diferencia de los punteros a funciones miembro, los delegados encapsulan una instancia de objeto y un método.

Una declaración de delegado define una clase que se deriva de la clase `System.Delegate`. Una instancia de delegado encapsula una lista de invocación, que es una lista de uno o varios métodos, a la que se hace referencia como una entidad a la que se puede llamar. En el caso de los métodos de instancia, una entidad a la que se puede llamar está formada por una instancia y un método en esa instancia. En el caso de los métodos estáticos, una entidad a la que se puede llamar consta simplemente de un método. Al invocar una instancia de delegado con un conjunto de argumentos adecuado, se invoca a cada una de las entidades a las que se puede llamar del delegado con el conjunto de argumentos especificado.

Una propiedad interesante y útil de una instancia de delegado es que no sabe ni le interesan las clases de los métodos que encapsula; lo único que importa es que esos métodos sean compatibles ([declaraciones de delegado](delegates.md#delegate-declarations)) con el tipo de delegado. Esto hace que los delegados sean perfectamente adecuados para la invocación "anónima".

## <a name="delegate-declarations"></a>Declaraciones de delegado

Un *delegate_declaration* es un *type_declaration* ([declaraciones de tipos](namespaces.md#type-declarations)) que declara un nuevo tipo de delegado.

```antlr
delegate_declaration
    : attributes? delegate_modifier* 'delegate' return_type
      identifier variant_type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;

delegate_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | delegate_modifier_unsafe
    ;
```

Es un error en tiempo de compilación que el mismo modificador aparezca varias veces en una declaración de delegado.

El modificador `new` solo se permite en los delegados declarados dentro de otro tipo, en cuyo caso especifica que dicho delegado oculte un miembro heredado con el mismo nombre, tal y como se describe en [el modificador New](classes.md#the-new-modifier).

Los modificadores `public`, `protected`, `internal`y `private` controlan la accesibilidad del tipo de delegado. En función del contexto en el que se produce la declaración de delegado, es posible que algunos de estos modificadores no estén permitidos (se[declare accesibilidad](basic-concepts.md#declared-accessibility)).

El nombre de tipo del delegado es *Identifier*.

El *formal_parameter_list* opcional especifica los parámetros del delegado y *return_type* indica el tipo de valor devuelto del delegado.

El *variant_type_parameter_list* opcional ([listas de parámetros de tipo variante](interfaces.md#variant-type-parameter-lists)) especifica los parámetros de tipo para el delegado.

El tipo de valor devuelto de un tipo de delegado debe ser `void`o seguro de salida (seguridad de la[varianza](interfaces.md#variance-safety)).

Todos los tipos de parámetros formales de un tipo de delegado deben ser seguros para la entrada. Además, los tipos de parámetros `out` o `ref` también deben ser seguros para la salida. Tenga en cuenta que incluso `out` parámetros deben ser seguros para la entrada, debido a una limitación de la plataforma de ejecución subyacente.

Los tipos de C# delegado de son equivalentes de nombre, no estructuralmente equivalentes. En concreto, dos tipos de delegado diferentes que tienen las mismas listas de parámetros y tipo de valor devuelto se consideran tipos de delegado distintos. Sin embargo, las instancias de dos tipos de delegado distintos, pero estructuralmente equivalentes, pueden compararse como iguales ([operadores de igualdad de delegado](expressions.md#delegate-equality-operators)).

Por ejemplo:

```csharp
delegate int D1(int i, double d);

class A
{
    public static int M1(int a, double b) {...}
}

class B
{
    delegate int D2(int c, double d);
    public static int M1(int f, double g) {...}
    public static void M2(int k, double l) {...}
    public static int M3(int g) {...}
    public static void M4(int g) {...}
}
```

Los métodos `A.M1` y `B.M1` son compatibles con los tipos de delegado `D1` y `D2`, ya que tienen el mismo tipo de valor devuelto y la misma lista de parámetros. sin embargo, estos tipos de delegado son dos tipos diferentes, por lo que no son intercambiables. Los métodos `B.M2`, `B.M3`y `B.M4` son incompatibles con los tipos de delegado `D1` y `D2`, ya que tienen tipos de valor devuelto o listas de parámetros diferentes.

Al igual que otras declaraciones de tipos genéricos, se deben proporcionar argumentos de tipo para crear un tipo de delegado construido. Los tipos de parámetro y el tipo de valor devuelto de un tipo de delegado construido se crean sustituyendo, por cada parámetro de tipo de la declaración de delegado, el argumento de tipo correspondiente del tipo de delegado construido. El tipo de valor devuelto y los tipos de parámetro resultantes se usan para determinar qué métodos son compatibles con un tipo de delegado construido. Por ejemplo:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

El método `X.F` es compatible con el tipo de delegado `Predicate<int>` y el `X.G` de método es compatible con el tipo de delegado `Predicate<string>`.

La única manera de declarar un tipo de delegado es a través de un *delegate_declaration*. Un tipo de delegado es un tipo de clase que se deriva de `System.Delegate`. Los tipos de delegado son implícitamente `sealed`, por lo que no se permite derivar ningún tipo de un tipo de delegado. Tampoco se permite derivar un tipo de clase no delegada de `System.Delegate`. Tenga en cuenta que `System.Delegate` no es un tipo delegado. es un tipo de clase del que se derivan todos los tipos de delegado.

C#proporciona una sintaxis especial para la invocación y creación de instancias de delegado. A excepción de la creación de instancias, cualquier operación que se puede aplicar a una instancia de clase o clase también se puede aplicar a una clase o instancia de delegado, respectivamente. En concreto, es posible tener acceso a los miembros del tipo `System.Delegate` a través de la sintaxis de acceso a miembros habitual.

El conjunto de métodos encapsulado por una instancia de delegado se denomina lista de invocación. Cuando se crea una instancia de delegado ([compatibilidad con delegación](delegates.md#delegate-compatibility)) desde un único método, encapsula ese método y su lista de invocación contiene solo una entrada. Sin embargo, cuando se combinan dos instancias de delegado que no son NULL, sus listas de invocación se concatenan, en el operando izquierdo de orden y operando derecho, para formar una nueva lista de invocación, que contiene dos o más entradas.

Los delegados se combinan mediante el `+` binario ([operador de suma](expressions.md#addition-operator)) y los operadores de `+=` ([asignación compuesta](expressions.md#compound-assignment)). Un delegado se puede quitar de una combinación de delegados, mediante el `-` binario ([operador de resta](expressions.md#subtraction-operator)) y los operadores de `-=` ([asignación compuesta](expressions.md#compound-assignment)). Los delegados se pueden comparar para comprobar si son iguales ([operadores de igualdad de delegado](expressions.md#delegate-equality-operators)).

En el ejemplo siguiente se muestra la creación de instancias de varios delegados y sus listas de invocación correspondientes:

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public static void M2(int i) {...}

}

class Test
{
    static void Main() {
        D cd1 = new D(C.M1);      // M1
        D cd2 = new D(C.M2);      // M2
        D cd3 = cd1 + cd2;        // M1 + M2
        D cd4 = cd3 + cd1;        // M1 + M2 + M1
        D cd5 = cd4 + cd3;        // M1 + M2 + M1 + M1 + M2
    }

}
```

Cuando se crean instancias de `cd1` y `cd2`, cada una de ellas encapsula un método. Cuando se crea una instancia de `cd3`, tiene una lista de invocaciones de dos métodos, `M1` y `M2`, en ese orden. la lista de invocación de `cd4`contiene `M1`, `M2`y `M1`, en ese orden. Por último, la lista de invocación de `cd5`contiene `M1`, `M2`, `M1`, `M1`y `M2`, en ese orden. Para obtener más ejemplos de cómo combinar (y quitar) delegados, vea [invocación de delegado](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Compatibilidad de los delegados

Un método o delegado `M` es ***compatible*** con un tipo de delegado `D` si se cumplen todas las condiciones siguientes:

*  `D` y `M` tienen el mismo número de parámetros y cada parámetro de `D` tiene los mismos modificadores `ref` o `out` que el parámetro correspondiente de `M`.
*  Para cada parámetro de valor (un parámetro sin `ref` o `out` modificador), existe una conversión de identidad ([conversión de identidad](conversions.md#identity-conversion)) o una conversión de referencia implícita ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions)) del tipo de parámetro de `D` al tipo de parámetro correspondiente en `M`.
*  Para cada parámetro `ref` o `out`, el tipo de parámetro de `D` es el mismo que el tipo de parámetro de `M`.
*  Existe una conversión de referencia implícita o de identidad del tipo de valor devuelto de `M` al tipo de valor devuelto de `D`.

## <a name="delegate-instantiation"></a>Creación de instancias de delegado

Una instancia de un delegado se crea mediante un *delegate_creation_expression* ([expresiones de creación de delegados](expressions.md#delegate-creation-expressions)) o una conversión a un tipo de delegado. La instancia de delegado recién creada hace referencia a:

*  Método estático al que se hace referencia en el *delegate_creation_expression*, o bien
*  El objeto de destino (que no se puede `null`) y el método de instancia al que se hace referencia en el *delegate_creation_expression*, o bien
*  Otro delegado.

Por ejemplo:

```csharp
delegate void D(int x);

class C
{
    public static void M1(int i) {...}
    public void M2(int i) {...}
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);        // static method
        C t = new C();
        D cd2 = new D(t.M2);        // instance method
        D cd3 = new D(cd2);        // another delegate
    }
}
```

Una vez creada la instancia, las instancias de delegado siempre hacen referencia al mismo objeto y método de destino. Recuerde que, cuando se combinan dos delegados o uno se quita de otro, se produce un nuevo delegado que tiene su propia lista de invocación. las listas de invocaciones de los delegados combinados o quitados permanecen sin cambios.

## <a name="delegate-invocation"></a>Invocación de delegado

C#proporciona una sintaxis especial para invocar un delegado. Cuando se invoca una instancia de delegado que no es null cuya lista de invocación contiene una entrada, invoca el método con los mismos argumentos en los que se ha dado y devuelve el mismo valor que el método al que se hace referencia. (Vea [invocaciones de delegado](expressions.md#delegate-invocations) para obtener información detallada sobre la invocación de delegado). Si se produce una excepción durante la invocación de este tipo de delegado y esa excepción no se detecta dentro del método que se invocó, la búsqueda de una cláusula catch de excepción continúa en el método que llamó al delegado, como si ese método hubiera llamado directamente al método al que el delegado hizo referencia.

La invocación de una instancia de delegado cuya lista de invocaciones contiene varias entradas continúa invocando cada uno de los métodos de la lista de invocación, de forma sincrónica, en orden. Cada método al que se llama se pasa el mismo conjunto de argumentos que se asignó a la instancia del delegado. Si dicha invocación de delegado incluye parámetros de referencia ([parámetros de referencia](classes.md#reference-parameters)), cada invocación de método se producirá con una referencia a la misma variable. los cambios en esa variable por un método en la lista de invocación serán visibles para los métodos más abajo en la lista de invocación. Si la invocación del delegado incluye parámetros de salida o un valor devuelto, su valor final proviene de la invocación del último delegado en la lista.

Si se produce una excepción durante el procesamiento de la invocación de este delegado, y esa excepción no se detecta dentro del método que se ha invocado, la búsqueda de una cláusula catch de excepción continúa en el método que llamó al delegado y cualquier método más abajo no se invoca la lista de invocación.

Al intentar invocar una instancia de delegado cuyo valor es null, se produce una excepción de tipo `System.NullReferenceException`.

En el ejemplo siguiente se muestra cómo crear instancias, combinar, quitar e invocar delegados:

```csharp
using System;

delegate void D(int x);

class C
{
    public static void M1(int i) {
        Console.WriteLine("C.M1: " + i);
    }

    public static void M2(int i) {
        Console.WriteLine("C.M2: " + i);
    }

    public void M3(int i) {
        Console.WriteLine("C.M3: " + i);
    }
}

class Test
{
    static void Main() { 
        D cd1 = new D(C.M1);
        cd1(-1);                // call M1

        D cd2 = new D(C.M2);
        cd2(-2);                // call M2

        D cd3 = cd1 + cd2;
        cd3(10);                // call M1 then M2

        cd3 += cd1;
        cd3(20);                // call M1, M2, then M1

        C c = new C();
        D cd4 = new D(c.M3);
        cd3 += cd4;
        cd3(30);                // call M1, M2, M1, then M3

        cd3 -= cd1;             // remove last M1
        cd3(40);                // call M1, M2, then M3

        cd3 -= cd4;
        cd3(50);                // call M1 then M2

        cd3 -= cd2;
        cd3(60);                // call M1

        cd3 -= cd2;             // impossible removal is benign
        cd3(60);                // call M1

        cd3 -= cd1;             // invocation list is empty so cd3 is null

        cd3(70);                // System.NullReferenceException thrown

        cd3 -= cd1;             // impossible removal is benign
    }
}
```

Como se muestra en la instrucción `cd3 += cd1;`, un delegado puede estar presente varias veces en una lista de invocación. En este caso, simplemente se invoca una vez por cada repetición. En una lista de invocación como esta, cuando se quita ese delegado, la última aparición en la lista de invocación es la que realmente se quita.

Justo antes de la ejecución de la instrucción final, `cd3 -= cd1;`, el `cd3` delegado hace referencia a una lista de invocación vacía. El intento de quitar un delegado de una lista vacía (o de quitar un delegado que no existe de una lista no vacía) no es un error.

La salida generada es:

```console
C.M1: -1
C.M2: -2
C.M1: 10
C.M2: 10
C.M1: 20
C.M2: 20
C.M1: 20
C.M1: 30
C.M2: 30
C.M1: 30
C.M3: 30
C.M1: 40
C.M2: 40
C.M3: 40
C.M1: 50
C.M2: 50
C.M1: 60
C.M1: 60
```
