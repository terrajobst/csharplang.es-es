# <a name="delegates"></a>Delegados

Los delegados permiten escenarios que otros lenguajes, como C++, Pascal y Modula--han resuelto con punteros de función. A diferencia de los punteros de función de C++, sin embargo, los delegados son completamente orientada a objetos y, a diferencia de los punteros a funciones miembro de C++, los delegados encapsulan una instancia de objeto y un método.

Una declaración de delegado define una clase que se deriva de la clase `System.Delegate`. Una instancia de delegado encapsula una lista de invocación, que es una lista de uno o varios métodos, cada uno de los cuales se conoce como una entidad que se puede llamar. Por ejemplo, métodos, una entidad que se puede llamar consta de una instancia y un método en esa instancia. Para los métodos estáticos, una entidad que se puede llamar consta de un solo método. Invoca una instancia de delegado con un conjunto adecuado de argumentos, cada una de las entidades en que se puede llamar que se invocan con el conjunto especificado de argumentos del delegado.

Una propiedad interesante y útil de una instancia del delegado es que no conoce ni interesan las clases de los métodos que encapsula; lo único que importa es que esos métodos sean compatibles ([declaraciones de delegado](delegates.md#delegate-declarations)) con el tipo del delegado. Esto hace que los delegados sean perfectamente adecuados para la invocación "anonymous".

## <a name="delegate-declarations"></a>Declaraciones de delegados

Un *delegate_declaration* es un *type_declaration* ([declaraciones de tipo](namespaces.md#type-declarations)) que declara un nuevo tipo de delegado.

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

Es un error en tiempo de compilación para el mismo modificador aparezca varias veces en una declaración de delegado.

El `new` modificador solo se permite en los delegados se declaran dentro de otro tipo, en cuyo caso especifica dicho delegado oculta un miembro heredado con el mismo nombre, como se describe en [el nuevo modificador](classes.md#the-new-modifier).

El `public`, `protected`, `internal`, y `private` modificadores controlan la accesibilidad del tipo de delegado. Dependiendo del contexto en el que se produce la declaración de delegado, algunos de estos modificadores pueden no estar permitidos ([accesibilidad declarada](basic-concepts.md#declared-accessibility)).

Es el nombre del tipo delegado *identificador*.

El elemento opcional *formal_parameter_list* especifica los parámetros del delegado, y *return_type* indica el tipo de valor devuelto del delegado.

El elemento opcional *variant_type_parameter_list* ([listas de parámetros de tipo variante](interfaces.md#variant-type-parameter-lists)) especifica los parámetros de tipo para el delegado en Sí.

El tipo de valor devuelto de un tipo de delegado debe ser `void`, o con seguridad de salida ([seguridad varianza](interfaces.md#variance-safety)).

Todos los tipos de parámetros formales de un tipo de delegado deben ser con seguridad de entrada. Asimismo, cualquier `out` o `ref` tipos de parámetro también deben ser seguro para salida. Tenga en cuenta que, incluso `out` parámetros son necesarios para tener seguridad de entrada, debido a una limitación de la plataforma subyacente de la ejecución.

Tipos de delegados en C# son el nombre equivalente, no es estructuralmente equivalente. En concreto, dos tipos de delegado distintos que tienen el mismo parámetro se enumeran y devolver el tipo se consideran tipos de delegado distintos. Sin embargo, pueden comparar las instancias de dos tipos de delegado distintos pero equivalentes estructuralmente igual ([delegar los operadores de igualdad](expressions.md#delegate-equality-operators)).

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

Los métodos `A.M1` y `B.M1 `son compatibles con ambos tipos de delegado `D1` y `D2` , puesto que tienen el mismo tipo y lista de parámetros de devolver; sin embargo, estos tipos de delegado son dos tipos diferentes, por lo que no son intercambiables. Los métodos `B.M2`, `B.M3`, y `B.M4` no son compatibles con los tipos de delegado `D1` y `D2`, puesto que tienen diferentes tipos de valor devueltos o listas de parámetros.

Al igual que otras declaraciones de tipo genérico, se deben proporcionar argumentos de tipo para crear un tipo de delegado construido. Los tipos de parámetro y el tipo de valor devuelto de un tipo de delegado construido se crean sustituyendo, para cada parámetro de tipo en la declaración de delegado, el argumento de tipo correspondiente del tipo de delegado construido. El tipo de valor devuelto resultante y los tipos de parámetro se usan para determinar qué métodos son compatibles con un tipo de delegado construido. Por ejemplo:

```csharp
delegate bool Predicate<T>(T value);

class X
{
    static bool F(int i) {...}
    static bool G(string s) {...}
}
```

El método `X.F` es compatible con el tipo de delegado `Predicate<int>` y el método `X.G` es compatible con el tipo de delegado `Predicate<string>` .

La única manera de declarar un tipo delegado es a través de un *delegate_declaration*. Un tipo de delegado es un tipo de clase que se deriva de `System.Delegate`. Tipos de delegados son implícitamente `sealed`, por lo que no permite derivar cualquier tipo de un tipo de delegado. También no es permitida para derivar un tipo de clase no delegado de `System.Delegate`. Tenga en cuenta que `System.Delegate` es no escriba un delegado; es un tipo de clase que se derivan todos los tipos de delegado.

C# proporciona una sintaxis especial de delegado y llamada de instancias. Excepto para la creación de instancias, cualquier operación que se puede aplicar a una clase o instancia de la clase también puede aplicarse a una clase de delegado o una instancia, respectivamente. En concreto, es posible acceder a los miembros de la `System.Delegate` tipo a través de la sintaxis de acceso de miembro habitual.

El conjunto de métodos encapsulados por una instancia de delegado se denomina una lista de invocación. Cuando se crea una instancia de delegado ([compatibilidad de delegado](delegates.md#delegate-compatibility)) desde un único método, encapsula ese método y su lista de invocación contiene sólo una entrada. Sin embargo, cuando se combinan dos instancias de delegado no null, se concatenan sus listas de invocación--en el orden en el operando izquierdo operando derecho--para formar una nueva lista de invocación, que contiene dos o más entradas.

Los delegados se combinan mediante el archivo binario `+` ([operador de suma](expressions.md#addition-operator)) y `+=` operadores ([asignación compuesta](expressions.md#compound-assignment)). Se puede quitar un delegado de una combinación de delegados utilizando el archivo binario `-` ([operador de resta](expressions.md#subtraction-operator)) y `-=` operadores ([asignación compuesta](expressions.md#compound-assignment)). Los delegados se pueden comparar la igualdad ([delegar los operadores de igualdad](expressions.md#delegate-equality-operators)).

En el ejemplo siguiente se muestra la creación de instancias de un número de delegados y sus correspondientes listas de llamadas:

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

Cuando `cd1` y `cd2` son instancias, cada uno de ellos encapsular un método. Cuando `cd3` es creación de instancias, tiene una lista de invocaciones de dos métodos, `M1` y `M2`, en ese orden. `cd4`de contiene la lista de invocaciones `M1`, `M2`, y `M1`, en ese orden. Por último, `cd5`de contiene la lista de invocaciones `M1`, `M2`, `M1`, `M1`, y `M2`, en ese orden. Para obtener más ejemplos de cómo combinar delegados (así como eliminar), consulte [invocación de delegado](delegates.md#delegate-invocation).

## <a name="delegate-compatibility"></a>Compatibilidad de delegado

Un método o delegado `M` es ***compatible*** con un tipo de delegado `D` si se cumplen todas las opciones siguientes:

*  `D` y `M` tienen el mismo número de parámetros y cada parámetro de `D` tiene el mismo `ref` o `out` modificadores como el parámetro correspondiente en `M`.
*  Para cada parámetro de valor (un parámetro no `ref` o `out` modificador), una conversión de identidad ([conversión de identidad](conversions.md#identity-conversion)) o una conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) existe el tipo de parámetro en `D` al tipo de parámetro correspondiente en `M`.
*  Para cada `ref` o `out` parámetro, escriba el parámetro `D` es el mismo que el tipo de parámetro en `M`.
*  No existe una identidad o una conversión implícita de referencia de tipo de valor devuelto de `M` para el tipo de valor devuelto de `D`.

## <a name="delegate-instantiation"></a>Creación de instancias de delegado

Se crea una instancia de un delegado mediante un *delegate_creation_expression* ([expresiones de creación de delegados](expressions.md#delegate-creation-expressions)) o una conversión a un tipo de delegado. La instancia del delegado recién creado, a continuación, hace referencia a cualquiera:

*  El método estático al que hace referencia en el *delegate_creation_expression*, o
*  El objeto de destino (que no puede ser `null`) y la instancia de método al que hace referencia en el *delegate_creation_expression*, o
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

Una vez que crea una instancia, instancias de delegado consulte siempre el mismo objeto de destino y método. Recuerde que, cuando se combinan dos delegados o se quita uno de otro, un nuevo resultados delegado con su propia lista de invocaciones; las listas de invocaciones de los delegados combinados o eliminados permanecen sin cambios.

## <a name="delegate-invocation"></a>Invocación de delegado

C# proporciona una sintaxis especial para invocar a un delegado. Cuando se invoca una instancia de delegado no null cuya lista de invocaciones contiene una entrada, invoca el método con los mismos argumentos que se ha proporcionado y devuelve el mismo valor que el que se hace referencia al método. (Consulte [invocaciones de delegado](expressions.md#delegate-invocations) para obtener información detallada sobre la invocación de delegado.) Si se produce una excepción durante la invocación de un delegado y no se detecta esa excepción dentro del método invocado, la búsqueda de una cláusula de excepción catch continúa en el método que llama al delegado, como si ese método hubiera llamado directamente el método al que delegar la que hace referencia.

La invocación de una instancia de delegado cuya lista de invocaciones contiene varias entradas continúa llamando a cada uno de los métodos en la lista de invocaciones, de forma sincrónica, en orden. Cada método llamado por lo que se pasa el mismo conjunto de argumentos que se asignó a la instancia del delegado. Si la invocación de delegado incluye parámetros de referencia ([hacen referencia a parámetros](classes.md#reference-parameters)), se producirá cada invocación de método con una referencia a la misma variable; serán los cambios en esa variable mediante un método en la lista de invocación visible para los métodos más abajo en la lista de invocación. Si la invocación del delegado incluye parámetros de salida o valor devuelto, su valor final procederán de la invocación del último delegado en la lista.

Si se produce una excepción durante el procesamiento de la invocación de un delegado y no se detecta esa excepción dentro del método invocado, la búsqueda de una cláusula de excepción catch continúa en el método que llama al delegado, y todos los métodos más abajo no se invoca la lista de invocación.

Intento de invocar una instancia de delegado cuyo valor es null provoca una excepción de tipo `System.NullReferenceException`.

El ejemplo siguiente muestra cómo crear una instancia, combinar, quitar e invocar a delegados:

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

Como se muestra en la instrucción `cd3 += cd1;`, un delegado puede estar presente en una lista de invocaciones varias veces. En este caso, se le llama una vez por cada instancia. En la lista de invocación como esta, cuando se quita ese delegado, la última aparición en la lista de invocación es lo que se quita en realidad.

Inmediatamente antes de la ejecución de la instrucción final, `cd3 -= cd1;`, el delegado `cd3` hace referencia a una lista de invocaciones vacío. No es un error al intentar quitar un delegado de una lista vacía (o para quitar a un delegado que no existe en una lista de valores no vacíos).

El resultado es:

```
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
