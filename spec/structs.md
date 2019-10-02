---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704027"
---
# <a name="structs"></a>Estructuras

Las estructuras son similares a las clases en que representan las estructuras de datos que pueden contener miembros de datos y miembros de función. Sin embargo, a diferencia de las clases, las estructuras son tipos de valor y no requieren la asignación del montón. Una variable de un tipo de estructura contiene directamente los datos del struct, mientras que una variable de un tipo de clase contiene una referencia a los datos, la última conocida como un objeto.

Los structs son particularmente útiles para estructuras de datos pequeñas que tengan semánticas de valor. Los números complejos, los puntos de un sistema de coordenadas o los pares clave-valor de un diccionario son buenos ejemplos de structs. La clave de estas estructuras de datos es que tienen pocos miembros de datos, que no requieren el uso de la herencia o la identidad referencial, y que se pueden implementar de forma cómoda mediante la semántica de valores donde la asignación copia el valor en lugar de la referencia.

Como se describe en [tipos simples](types.md#simple-types), los tipos simples que C#proporciona, como `int`, `double` y `bool`, son en realidad todos los tipos de estructura. Del mismo modo que estos tipos predefinidos son Structs, también es posible usar estructuras y sobrecarga de operadores para implementar nuevos tipos "primitivos" en el C# lenguaje. Al final de este capítulo ([ejemplos de struct](structs.md#struct-examples)) se proporcionan dos ejemplos de estos tipos.

## <a name="struct-declarations"></a>Declaraciones de struct

Un *struct_declaration* es un *type_declaration* ([declaraciones de tipos](namespaces.md#type-declarations)) que declara un nuevo struct:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

Un *struct_declaration* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *struct_modifier*s ([modificadores de estructura](structs.md#struct-modifiers)), seguido de un modificador `partial` opcional, seguido del parámetro palabra clave `struct` y un *identificador* que nombra el struct, seguido de una especificación *type_parameter_list* opcional ([parámetros de tipo](classes.md#type-parameters)), seguida de una especificación de *struct_interfaces* opcional ([parcial) modificador](structs.md#partial-modifier)), seguido de una especificación de *type_parameter_constraints_clause*s opcional ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), seguida de un *struct_body* ([cuerpo de estructura](structs.md#struct-body)), opcionalmente seguido de un punto y coma.

### <a name="struct-modifiers"></a>Modificadores de struct

Un *struct_declaration* puede incluir opcionalmente una secuencia de modificadores de struct:

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

Es un error en tiempo de compilación que el mismo modificador aparezca varias veces en una declaración de estructura.

Los modificadores de una declaración de struct tienen el mismo significado que los de una declaración de clase ([declaraciones de clase](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Unmodifier (modificador)

El modificador `partial` indica que este *struct_declaration* es una declaración de tipos parciales. Varias declaraciones de struct parciales con el mismo nombre dentro de una declaración de tipo o espacio de nombres envolvente se combinan para formar una declaración de struct, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).

### <a name="struct-interfaces"></a>Interfaces de struct

Una declaración de estructura puede incluir una especificación *struct_interfaces* , en cuyo caso se dice que el struct implementa directamente los tipos de interfaz especificados.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Las implementaciones de interfaz se tratan en las [implementaciones de interfaz](interfaces.md#interface-implementations).

### <a name="struct-body"></a>Cuerpo del struct

El *struct_body* de un struct define los miembros del struct.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Miembros de struct

Los miembros de una estructura se componen de los miembros introducidos por sus *struct_member_declaration*s y los miembros heredados del tipo `System.ValueType`.

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

A excepción de las diferencias que se indican en [diferencias entre clases y estructuras](structs.md#class-and-struct-differences), las descripciones de los miembros de clase proporcionados en [miembros de clase](classes.md#class-members) a través de [iteradores](classes.md#iterators) también se aplican a los miembros de estructura.

## <a name="class-and-struct-differences"></a>Diferencias entre clases y estructuras

Los Structs difieren de las clases de varias maneras importantes:

*  Los Structs son tipos de valor ([semántica de valores](structs.md#value-semantics)).
*  Todos los tipos de struct se heredan implícitamente de la clase `System.ValueType` ([herencia](structs.md#inheritance)).
*  La asignación a una variable de un tipo de struct crea una copia del valor que se asigna ([asignación](structs.md#assignment)).
*  El valor predeterminado de un struct es el valor generado al establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en `null` ([valores predeterminados](structs.md#default-values)).
*  Las operaciones de conversión boxing y unboxing se utilizan para realizar la conversión entre un tipo de struct y `object` ([conversión boxing y unboxing](structs.md#boxing-and-unboxing)).
*  El significado de `this` es diferente para los Structs ([este acceso](expressions.md#this-access)).
*  Las declaraciones de campo de instancia de un struct no pueden incluir inicializadores variables ([inicializadores de campo](structs.md#field-initializers)).
*  Un struct no puede declarar un constructor de instancia sin parámetros ([constructores](structs.md#constructors)).
*  Un struct no puede declarar un[destructor (](structs.md#destructors)destructores).

### <a name="value-semantics"></a>Semántica de valores

Los Structs son tipos de valor ([tipos de valor](types.md#value-types)) y se dice que tienen semántica de valor. Por otro lado, las clases son tipos de referencia ([tipos de referencia](types.md#reference-types)) y se dice que tienen semántica de referencia.

Una variable de un tipo de estructura contiene directamente los datos del struct, mientras que una variable de un tipo de clase contiene una referencia a los datos, la última conocida como un objeto. Cuando un struct `B` contiene un campo de instancia de tipo `A` y `A` es un tipo de estructura, se trata de un error en tiempo de compilación para que `A` dependa de `B` o de un tipo construido a partir de `B`. Un struct `X` ***depende directamente de*** un struct `Y` si `X` contiene un campo de instancia de tipo `Y`. Dada esta definición, el conjunto completo de estructuras de las que depende un struct es el cierre transitivo de la relación ***directamente depende de*** .  Por ejemplo
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
es un error porque `Node` contiene un campo de instancia de su propio tipo.  Otro ejemplo
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
es un error porque cada uno de los tipos `A`, `B` y `C` dependen entre sí.

Con las clases, es posible que dos variables hagan referencia al mismo objeto y, por lo tanto, las operaciones en una variable afecten al objeto al que hace referencia la otra variable. Con las estructuras, cada variable tiene su propia copia de los datos (excepto en el caso de las variables de parámetro `ref` y `out`) y no es posible que las operaciones en una afecten a la otra. Además, dado que los Structs no son tipos de referencia, no es posible que los valores de un tipo struct sean `null`.

Dada la declaración
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
fragmento de código
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
Devuelve el valor `10`. La asignación de `a` a `b` crea una copia del valor y, por tanto, `b` no se ve afectada por la asignación a `a.x`. Si `Point` se hubiera declarado como una clase, el resultado sería `100` porque `a` y `b` hacían referencia al mismo objeto.

### <a name="inheritance"></a>Herencia

Todos los tipos de struct se heredan implícitamente de la clase `System.ValueType`, que, a su vez, hereda de la clase `object`. Una declaración de estructura puede especificar una lista de interfaces implementadas, pero no es posible que una declaración de struct especifique una clase base.

Los tipos de struct nunca son abstractos y siempre están sellados implícitamente. Por lo tanto, los modificadores `abstract` y `sealed` no se permiten en una declaración de estructura.

Puesto que no se admite la herencia para Structs, la accesibilidad declarada de un miembro de struct no puede ser `protected` ni `protected internal`.

Los miembros de función de un struct no pueden ser `abstract` o `virtual`, y el modificador `override` solo se permite para invalidar métodos heredados de `System.ValueType`.

### <a name="assignment"></a>Asignación

La asignación a una variable de un tipo de struct crea una copia del valor que se va a asignar. Esto difiere de la asignación a una variable de un tipo de clase, que copia la referencia pero no el objeto identificado por la referencia.

De forma similar a una asignación, cuando se pasa un struct como parámetro de valor o se devuelve como resultado de un miembro de función, se crea una copia de la estructura. Un struct se puede pasar por referencia a un miembro de función mediante un parámetro `ref` o `out`.

Cuando una propiedad o un indizador de una estructura es el destino de una asignación, la expresión de instancia asociada con el acceso a la propiedad o indizador debe estar clasificada como una variable. Si la expresión de instancia se clasifica como un valor, se produce un error en tiempo de compilación. Esto se describe con más detalle en [asignación simple](expressions.md#simple-assignment).

### <a name="default-values"></a>Valores predeterminados

Tal y como se describe en [valores predeterminados](variables.md#default-values), varios tipos de variables se inicializan automáticamente en su valor predeterminado cuando se crean. En el caso de las variables de tipos de clase y otros tipos de referencia, este valor predeterminado es `null`. Sin embargo, dado que las estructuras son tipos de valor que no pueden ser `null`, el valor predeterminado de una estructura es el valor generado al establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en `null`.

En el ejemplo se hace referencia al struct `Point` declarado anteriormente.
```csharp
Point[] a = new Point[100];
```
Inicializa cada `Point` de la matriz con el valor generado al establecer los campos `x` y `y` en cero.

El valor predeterminado de un struct corresponde al valor devuelto por el constructor predeterminado de la estructura ([constructores predeterminados](types.md#default-constructors)). A diferencia de una clase, un struct no puede declarar un constructor de instancia sin parámetros. En su lugar, cada struct tiene implícitamente un constructor de instancia sin parámetros que siempre devuelve el valor que se obtiene al establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en `null`.

Los Structs se deben diseñar para considerar el estado de inicialización predeterminado como un estado válido. En el ejemplo
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
el constructor de instancia definido por el usuario protege solo los valores NULL cuando se llama explícitamente. En los casos en los que una variable `KeyValuePair` está sujeta a la inicialización de valores predeterminados, los campos `key` y `value` serán NULL y el struct debe estar preparado para controlar este estado.

### <a name="boxing-and-unboxing"></a>Conversión boxing y conversión unboxing

Un valor de un tipo de clase se puede convertir al tipo `object` o a un tipo de interfaz implementado por la clase simplemente tratando la referencia como otro tipo en tiempo de compilación. Del mismo modo, un valor de tipo `object` o un valor de un tipo de interfaz se puede volver a convertir a un tipo de clase sin cambiar la referencia (pero, por supuesto, se requiere una comprobación de tipo en tiempo de ejecución en este caso).

Dado que los Structs no son tipos de referencia, estas operaciones se implementan de forma diferente para los tipos de struct. Cuando un valor de un tipo struct se convierte al tipo `object` o a un tipo de interfaz implementado por la estructura, se produce una operación de conversión boxing. Del mismo modo, cuando un valor de tipo `object` o un valor de un tipo de interfaz se vuelve a convertir a un tipo de estructura, se produce una operación de conversión unboxing. Una diferencia clave de las mismas operaciones en los tipos de clase es que la conversión boxing y la conversión unboxing copia el valor de la estructura dentro o fuera de la instancia de conversión boxing. Por lo tanto, después de una operación de conversión boxing o unboxing, los cambios realizados en la estructura desempaquetada no se reflejan en la estructura con conversión boxing.

Cuando un tipo de struct invalida un método virtual heredado de `System.Object` (como `Equals`, `GetHashCode` o `ToString`), la invocación del método virtual a través de una instancia del tipo struct no produce la conversión boxing. Esto es así incluso cuando el struct se utiliza como parámetro de tipo y la invocación se produce a través de una instancia del tipo de parámetro de tipo. Por ejemplo:
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

La salida del programa es:
```console
1
2
3
```

Aunque el estilo no es válido para que `ToString` tenga efectos secundarios, en el ejemplo se muestra que no se ha producido ninguna conversión boxing para las tres invocaciones de `x.ToString()`.

Del mismo modo, la conversión boxing nunca se produce implícitamente cuando se obtiene acceso a un miembro en un parámetro de tipo restringido. Por ejemplo, supongamos que una interfaz `ICounter` contiene un método `Increment` que se puede utilizar para modificar un valor. Si `ICounter` se utiliza como una restricción, se llama a la implementación del método `Increment` con una referencia a la variable en la que se llamó a `Increment`, nunca a una copia con conversión boxing.

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

La primera llamada a `Increment` modifica el valor de la variable `x`. Esto no es equivalente a la segunda llamada a `Increment`, que modifica el valor de una copia con conversión boxing de `x`. Por lo tanto, la salida del programa es:
```console
0
1
1
```

Para obtener más información sobre las conversiones boxing y unboxing, consulte [Boxing y unboxing](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Significado de este

Dentro de un constructor de instancia o un miembro de función de instancia de una clase, `this` se clasifica como un valor. Por lo tanto, aunque se puede usar `this` para hacer referencia a la instancia para la que se invocó el miembro de función, no es posible asignar a `this` en un miembro de función de una clase.

Dentro de un constructor de instancia de un struct, `this` corresponde a un parámetro `out` del tipo struct, y dentro de un miembro de función de instancia de un struct, `this` corresponde a un parámetro `ref` del tipo struct. En ambos casos, `this` está clasificado como una variable y es posible modificar la estructura completa para la que se invocó el miembro de la función mediante la asignación a `this` o pasando esto como un parámetro `ref` o `out`.

### <a name="field-initializers"></a>Inicializadores de campo

Como se describe en [valores predeterminados](structs.md#default-values), el valor predeterminado de un struct consta del valor que se obtiene al establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en `null`. Por esta razón, un struct no permite que las declaraciones de campo de instancia incluyan inicializadores variables. Esta restricción solo se aplica a los campos de instancia. Los campos estáticos de un struct pueden incluir inicializadores variables.

El ejemplo
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
es un error porque las declaraciones de campo de instancia incluyen inicializadores variables.

### <a name="constructors"></a>Constructores

A diferencia de una clase, un struct no puede declarar un constructor de instancia sin parámetros. En su lugar, cada struct tiene implícitamente un constructor de instancia sin parámetros que siempre devuelve el valor que se obtiene al establecer todos los campos de tipo de valor en sus valores predeterminados y todos los campos de tipo de referencia en null ([constructores predeterminados](types.md#default-constructors)). Un struct puede declarar constructores de instancia que tengan parámetros. Por ejemplo
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

Dada la declaración anterior, las instrucciones
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
ambos crean un `Point` con `x` y `y` inicializado en cero.

Un constructor de instancia de struct no puede incluir un inicializador de constructor con el formato `base(...)`.

Si el constructor de instancia de struct no especifica un inicializador de constructor, la variable `this` corresponde a un parámetro `out` del tipo struct y es similar a un parámetro `out`, `this` debe estar asignado definitivamente ([asignación definitiva ](variables.md#definite-assignment)) en cada ubicación donde el constructor devuelve. Si el constructor de instancia de struct especifica un inicializador de constructor, la variable `this` corresponde a un parámetro `ref` del tipo struct y similar a un parámetro `ref`, `this` se considera definitivamente asignada en la entrada al cuerpo del constructor . Considere la implementación del constructor de instancia a continuación:
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

No se puede llamar a ninguna función miembro de instancia (incluidos los descriptores de acceso set para las propiedades `X` y `Y`) hasta que todos los campos del struct que se está construyendo se hayan asignado definitivamente. La única excepción implica las propiedades implementadas automáticamente ([propiedades implementadas automáticamente](classes.md#automatically-implemented-properties)). Las reglas de asignación definitiva ([expresiones de asignación simple](variables.md#simple-assignment-expressions)) excluyen específicamente la asignación a una propiedad automática de un tipo de estructura dentro de un constructor de instancia de ese tipo de estructura: una asignación se considera una asignación definitiva del objeto oculto campo de respaldo de la propiedad automática. Por lo tanto, se permite lo siguiente:

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a>Destructores

Un struct no puede declarar un destructor.

### <a name="static-constructors"></a>Constructores estáticos

Los constructores estáticos para Structs siguen la mayoría de las mismas reglas que para las clases. La ejecución de un constructor estático para un tipo de struct lo desencadena el primero de los siguientes eventos para que se produzca dentro de un dominio de aplicación:

*  Se hace referencia A un miembro estático del tipo struct.
*  Se llama a un constructor declarado explícitamente del tipo de estructura.

La creación de valores predeterminados ([valores predeterminados](structs.md#default-values)) de tipos struct no desencadena el constructor estático. (Un ejemplo de esto es el valor inicial de los elementos de una matriz).

## <a name="struct-examples"></a>Ejemplos de struct

A continuación se muestran dos ejemplos importantes del uso de tipos `struct` para crear tipos que se pueden usar de forma similar a los tipos predefinidos del lenguaje, pero con semántica modificada.

### <a name="database-integer-type"></a>Tipo entero de base de datos

El struct `DBInt` siguiente implementa un tipo entero que puede representar el conjunto completo de valores del tipo `int`, además de un estado adicional que indica un valor desconocido. Un tipo con estas características se utiliza normalmente en las bases de datos.

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a>Tipo booleano de base de datos

El struct `DBBool` siguiente implementa un tipo lógico de tres valores. Los valores posibles de este tipo son `DBBool.True`, `DBBool.False` y @no__t 2, donde el miembro `Null` indica un valor desconocido. Estos tipos lógicos de tres valores se utilizan normalmente en las bases de datos de.

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
