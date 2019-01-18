---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229733"
---
# <a name="structs"></a>Estructuras

Los structs son similares a las clases que representan las estructuras de datos que pueden contener miembros de datos y miembros de función. Sin embargo, a diferencia de las clases, structs son tipos de valor y no requieren asignación del montón. Una variable de un tipo de estructura contiene directamente los datos de la estructura, mientras que una variable de un tipo de clase contiene una referencia a los datos, que se conoce como un objeto.

Los structs son particularmente útiles para estructuras de datos pequeñas que tengan semánticas de valor. Los números complejos, los puntos de un sistema de coordenadas o los pares clave-valor de un diccionario son buenos ejemplos de structs. Clave de estas estructuras de datos es que tienen pocos miembros de datos, que no requieren el uso de herencia o identidad referencial, y que pueden ser implementadas convenientemente utilizando la semántica de valor donde la asignación copia el valor en lugar de la referencia.

Como se describe en [tipos simples](types.md#simple-types), los tipos simples que proporciona C#, tales como `int`, `double`, y `bool`, son en realidad todos los tipos de struct. Como estos tipos predefinidos son estructuras, también es posible utilizar estructuras y sobrecarga de operadores para implementar nuevos tipos "primitivos" en el lenguaje C#. Se proporcionan dos ejemplos de estos tipos al final de este capítulo ([ejemplos de estructuras](structs.md#struct-examples)).

## <a name="struct-declarations"></a>Declaraciones de estructura

Un *struct_declaration* es un *type_declaration* ([declaraciones de tipo](namespaces.md#type-declarations)) que declara una nueva estructura:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

Un *struct_declaration* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *struct_modifier*s ([Struct modificadores](structs.md#struct-modifiers)), seguido de un elemento opcional `partial` modificador, seguido por la palabra clave `struct` y un *identificador* que denomina el struct, seguido por un opcional *type_parameter_list* especificación ([parámetros de tipo](classes.md#type-parameters)), seguido de un elemento opcional *struct_interfaces* especificación ([Modificador parcial](structs.md#partial-modifier))), seguido de un elemento opcional *type_parameter_constraints_clause*especificación s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), seguido de un *struct_body* ([cuerpo Struct](structs.md#struct-body)), seguido opcionalmente de un punto y coma.

### <a name="struct-modifiers"></a>Modificadores de estructura

Un *struct_declaration* puede incluir opcionalmente una secuencia de modificadores de estructura:

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

Es un error en tiempo de compilación para el mismo modificador aparezca varias veces en una declaración de struct.

Los modificadores de una declaración de estructura tienen el mismo significado que los de una declaración de clase ([declaraciones de clase](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Modificador parcial

El `partial` modificador indica que esto *struct_declaration* es una declaración de tipo parcial. Varias declaraciones de estructura parcial con el mismo nombre dentro de una declaración de espacio de nombres o tipo envolvente se combinan para formar una declaración de struct, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).

### <a name="struct-interfaces"></a>Interfaces de struct

Puede incluir una declaración de struct un *struct_interfaces* especificación, en cuyo caso el struct se dice implementar directamente los tipos de interfaz determinado.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Implementaciones de interfaz se tratan con más detalle en [implementaciones de interfaz](interfaces.md#interface-implementations).

### <a name="struct-body"></a>Cuerpo de estructura

El *struct_body* de una estructura define los miembros de struct.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Miembros de estructura

Los miembros de un struct se componen de los miembros introducidos por su *struct_member_declaration*s y los miembros heredan del tipo `System.ValueType`.

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

Salvo por las diferencias que anotó en [diferencias entre clase y estructura](structs.md#class-and-struct-differences), las descripciones de los miembros de clase proporcionado en [los miembros de clase](classes.md#class-members) a través de [iteradores](classes.md#iterators) se aplican a struct miembros también.

## <a name="class-and-struct-differences"></a>Diferencias entre clase y estructura

Structs difieren de las clases en varios aspectos importantes:

*  Los structs son tipos de valor ([semántica de valores](structs.md#value-semantics)).
*  Todos los tipos de struct se heredan implícitamente de la clase `System.ValueType` ([herencia](structs.md#inheritance)).
*  La asignación a una variable de un tipo de estructura crea una copia del valor asignado ([asignación](structs.md#assignment)).
*  El valor predeterminado de un struct es el valor generado al establecer todos los campos de tipo de valor en sus valores predeterminados y referencia de todos los campos de tipo a `null` ([los valores predeterminados](structs.md#default-values)).
*  Operaciones de conversión boxing y unboxing se utilizan para convertir entre un tipo struct y `object` ([conversiones Boxing y unboxing](structs.md#boxing-and-unboxing)).
*  El significado de `this` es diferente para los structs ([este acceso](expressions.md#this-access)).
*  Las declaraciones de campo de instancia para una estructura no pueden incluir inicializadores de variables ([inicializadores de campo](structs.md#field-initializers)).
*  Un struct no puede declarar un constructor de instancia sin parámetros ([constructores](structs.md#constructors)).
*  Un struct no puede declarar un destructor ([destructores](structs.md#destructors)).

### <a name="value-semantics"></a>Semántica de valores

Los structs son tipos de valor ([los tipos de valor](types.md#value-types)) y se dice que tienen semántica de valores. Las clases, por otro lado, son tipos de referencia ([hacen referencia a tipos](types.md#reference-types)) y se dice que tienen semántica de referencia.

Una variable de un tipo de estructura contiene directamente los datos de la estructura, mientras que una variable de un tipo de clase contiene una referencia a los datos, que se conoce como un objeto. Cuando un struct `B` contiene un campo de instancia del tipo `A` y `A` es un tipo struct, es un error de tiempo de compilación de `A` depender `B` o un tipo construido a partir de `B`. Un struct `X` ***depende directamente de*** un struct `Y` si `X` contiene un campo de instancia del tipo `Y`. Dada esta definición, el conjunto completo de las estructuras de los que depende un struct es el cierre transitivo de los ***depende directamente*** relación.  Por ejemplo
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
es un error porque cada uno de los tipos `A`, `B`, y `C` dependen entre sí.

Con las clases, es posible que dos variables hagan referencia al mismo objeto y, por tanto, las operaciones en una variable afecten al objeto al que hace referencia la otra variable. Estructuras, cada variable tiene su propia copia de los datos (excepto en el caso de `ref` y `out` las variables de parámetro), y no es posible que las operaciones en una variable afecten a la otra. Además, dado que las estructuras no son tipos de referencia, no es posible para los valores de un tipo struct sea `null`.

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
el fragmento de código
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
presenta el valor `10`. La asignación de `a` a `b` crea una copia del valor, y `b` , por tanto, no se ve afectado por la asignación a `a.x`. Tenía `Point` en su lugar se ha sido declarado como una clase, el resultado sería `100` porque `a` y `b` haría referencia al mismo objeto.

### <a name="inheritance"></a>Herencia

Todos los tipos de struct se heredan implícitamente de la clase `System.ValueType`, que, a su vez, hereda de la clase `object`. Una declaración de estructura puede especificar una lista de interfaces implementadas, pero no es posible que una declaración de estructura especificar una clase base.

Tipos de estructura nunca son abstractos y son siempre sellados implícitamente. El `abstract` y `sealed` modificadores, por tanto, no se permiten en una declaración de struct.

Puesto que no se admite la herencia para los structs, la accesibilidad declarada de un miembro de estructura no puede ser `protected` o `protected internal`.

Miembros de función en un struct no pueden ser `abstract` o `virtual`y el `override` modificador sólo se permite para invalidar métodos heredados de `System.ValueType`.

### <a name="assignment"></a>Asignación

La asignación a una variable de un tipo de estructura crea una copia del valor asignado. Esto difiere de la asignación a una variable de un tipo de clase, que copia la referencia pero no el objeto identificado por la referencia.

Al igual que una asignación, cuando un struct se pasa como un parámetro de valor o se devuelve como resultado de un miembro de función, se crea una copia de la estructura. Un struct puede pasarse por referencia a un miembro de función con un `ref` o `out` parámetro.

Cuando una propiedad o indizador de una estructura es el destino de una asignación, la expresión de instancia asociada con el acceso de propiedad o indizador debe estar clasificada como una variable. Si la expresión de instancia se clasifica como un valor, se produce un error en tiempo de compilación. Esto se describe con más detalle en [asignación Simple](expressions.md#simple-assignment).

### <a name="default-values"></a>Valores predeterminados

Como se describe en [los valores predeterminados](variables.md#default-values), varios tipos de variables se inicializan automáticamente en su valor predeterminado cuando se crean. Para las variables de tipos de clase y otros tipos de referencia, este valor predeterminado es `null`. Sin embargo, dado que los structs son tipos de valor no pueden ser `null`, el valor predeterminado de un struct es el valor generado al establecer todos los campos de tipo de valor en sus valores predeterminados y referencia de todos los campos de tipo a `null`.

Que hace referencia a la `Point` estructura declarada anteriormente, en el ejemplo
```csharp
Point[] a = new Point[100];
```
Inicializa cada `Point` en la matriz en el valor generado al establecer el `x` y `y` campos en cero.

El valor predeterminado de un struct se corresponde con el valor devuelto por el constructor predeterminado del struct ([constructores predeterminados](types.md#default-constructors)). A diferencia de una clase, un struct no se permite declarar un constructor de instancia sin parámetros. En su lugar, cada estructura tiene implícitamente un constructor de instancia sin parámetros que siempre devuelve el valor que es el resultado de establecer todos los campos de tipo de valor a sus valores predeterminados y referencia de todos los campos de tipo a `null`.

Las estructuras deben diseñarse a tener en cuenta el estado de inicialización predeterminado con un estado válido. En el ejemplo
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
el constructor de instancia definido por el usuario protege contra los valores null solo cuando es llamado explícitamente. En casos donde un `KeyValuePair` variable está sujeta a la inicialización de un valor predeterminado, el `key` y `value` campos será nulos y la estructura debe estar preparada para controlar este estado.

### <a name="boxing-and-unboxing"></a>Conversión boxing y conversión unboxing

Un valor de un tipo de clase se puede convertir al tipo `object` o a un tipo de interfaz que es implementado por la clase simplemente tratando la referencia como otro tipo en tiempo de compilación. Del mismo modo, un valor de tipo `object` o un valor de un tipo de interfaz se puede convertir a un tipo de clase sin cambiar la referencia (pero evidentemente una comprobación de tipo de tiempo de ejecución es necesaria en este caso).

Dado que las estructuras no son tipos de referencia, estas operaciones se implementan de forma diferente para tipos de estructura. Cuando un valor de un tipo de estructura se convierte al tipo `object` o a un tipo de interfaz implementada por la estructura, realiza una operación de conversión boxing. Del mismo modo, cuando un valor de tipo `object` o un valor de un tipo de interfaz se convierte a un tipo de estructura, realiza una operación de conversión unboxing. Una diferencia fundamental con respecto a las mismas operaciones en tipos de clase es que conversión boxing y unboxing copian el valor de struct, ya sea dentro o fuera de la instancia de la conversión boxing. Por lo tanto, después de una operación de conversión boxing o conversión unboxing, los cambios realizados en la estructura de conversión unboxing no se reflejan en la estructura de conversión boxing.

Cuando un tipo struct invalida un método virtual heredado de `System.Object` (como `Equals`, `GetHashCode`, o `ToString`), la invocación del método virtual a través de una instancia del tipo struct no provoca la conversión boxing se producen. Esto es cierto incluso cuando se utiliza la estructura como un parámetro de tipo y la invocación se produce a través de una instancia del tipo de parámetro de tipo. Por ejemplo:
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
```
1
2
3
```

Aunque es poco recomendable `ToString` para tener efectos secundarios, el ejemplo se muestra que se ha producido ninguna conversión boxing para las tres invocaciones de `x.ToString()`.

De forma similar, la conversión boxing implícitamente nunca se produce cuando el acceso a un miembro en un parámetro de tipo restringido. Por ejemplo, supongamos que una interfaz `ICounter` contiene un método `Increment` que puede usarse para modificar un valor. Si `ICounter` se utiliza como una restricción, la implementación de la `Increment` se llama al método con una referencia a la variable que `Increment` se llamó en nunca una copia con conversión boxing.

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

La primera llamada a `Increment` modifica el valor en la variable `x`. Esto no es equivalente a la segunda llamada a `Increment`, que modifica el valor de una copia encuadrada de `x`. Por lo tanto, la salida del programa es:
```
0
1
1
```

Para obtener más información sobre las conversiones boxing y unboxing, consulte [conversiones Boxing y unboxing](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Significado de esto

Dentro de un constructor de instancia o un miembro de función de la instancia de una clase, `this` se clasifica como un valor. Por lo tanto, mientras `this` puede usarse para hacer referencia a la instancia para que se invoca el miembro de función, no es posible asignar a `this` en un miembro de función de una clase.

Dentro de un constructor de instancia de un struct, `this` corresponde a un `out` parámetro del tipo struct y dentro de un miembro de función de la instancia de un struct, `this` corresponde a un `ref` parámetro del tipo struct. En ambos casos, `this` se clasifica como una variable, y es posible modificar el struct completo para el que el miembro de función se invocó mediante la asignación a `this` o si se pasa como un `ref` o `out` parámetro.

### <a name="field-initializers"></a>Inicializadores de campo

Como se describe en [los valores predeterminados](structs.md#default-values), el valor predeterminado de una estructura formada por el valor que es el resultado de establecer todos los campos de tipo de valor a sus valores predeterminados y referencia de todos los campos de tipo a `null`. Por este motivo, un struct no permite que las declaraciones de campo de instancia para incluir a inicializadores de variables. Esta restricción se aplica solo a los campos de instancia. Los campos estáticos de una estructura pueden incluir a inicializadores de variables.

El ejemplo
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
es un error porque las declaraciones de campo de instancia incluyen a inicializadores de variables.

### <a name="constructors"></a>Constructores

A diferencia de una clase, un struct no se permite declarar un constructor de instancia sin parámetros. En su lugar, cada estructura tiene implícitamente un constructor de instancia sin parámetros que siempre devuelve el valor que se obtiene al establecer todos los campos de tipo de valor a sus valores predeterminados y referencia de todos los campos de tipo null ([constructorespredeterminados](types.md#default-constructors)). Un struct puede declarar constructores de instancia con parámetros. Por ejemplo
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
ambos crean un `Point` con `x` y `y` inicializadas en cero.

Un constructor de instancia de struct no puede incluir un inicializador de constructor del formulario `base(...)`.

Si el constructor de instancia de struct no especifica un inicializador de constructor, el `this` variable corresponde a un `out` parámetro de tipo de estructura y otros similares a un `out` parámetro, `this` debe asignarlo definitivamente () [Asignación definitiva](variables.md#definite-assignment)) en todas las ubicaciones donde se devuelve el constructor. Si el constructor de instancia de struct especifica un inicializador de constructor, el `this` variable corresponde a un `ref` parámetro de tipo de estructura y otros similares a un `ref` parámetro, `this` se considera asignado definitivamente en entrada en el cuerpo del constructor. Tenga en cuenta la siguiente implementación de constructor de instancia:
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

Ninguna función miembro de instancia (incluidos los descriptores de acceso set de las propiedades `X` y `Y`) se puede llamar hasta que se han asignado definitivamente todos los campos de la estructura que se está construyendo. La única excepción implica las propiedades implementadas automáticamente ([implementa automáticamente propiedades](classes.md#automatically-implemented-properties)). Las reglas de asignación definitiva ([expresiones de asignación Simple](variables.md#simple-assignment-expressions)) excluir específicamente la asignación a una propiedad automática de un tipo de estructura dentro de un constructor de instancia de ese tipo de struct: esta asignación se considera un definitiva asignación del campo oculto de respaldo de la propiedad automática. Por lo tanto, se permite lo siguiente:

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

Un struct no se permite declarar un destructor.

### <a name="static-constructors"></a>Constructores estáticos

Los constructores estáticos para las estructuras siguen la mayoría de las mismas reglas que las clases. El primero de los siguientes eventos que se producen dentro de un dominio de aplicación, se desencadena la ejecución de un constructor estático para un tipo struct:

*  Se hace referencia a un miembro estático del tipo struct.
*  Se llama a un constructor declarado explícitamente del tipo struct.

La creación de los valores predeterminados ([los valores predeterminados](structs.md#default-values)) tipos de struct no desencadena el constructor estático. (Un ejemplo de esto es el valor inicial de elementos de matriz).

## <a name="struct-examples"></a>Ejemplos de estructuras

Lo siguiente muestra dos ejemplos importantes de usar `struct` tipos para crear tipos que pueden usarse de forma similar a los tipos predefinidos del lenguaje, pero con semántica modificada.

### <a name="database-integer-type"></a>Tipo de entero de base de datos

El `DBInt` estructura siguiente implementa un tipo entero que puede representar el conjunto completo de valores de la `int` tipo, además de un estado adicional que indica un valor desconocido. Un tipo con estas características se usa habitualmente en las bases de datos.

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

### <a name="database-boolean-type"></a>Tipo booleano de la base de datos

El `DBBool` estructura siguiente implementa un tipo lógico de tres valores. Los valores posibles de este tipo son `DBBool.True`, `DBBool.False`, y `DBBool.Null`, donde el `Null` miembro indica un valor desconocido. Estos tipos lógicos de tres valores se usan habitualmente en las bases de datos.

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
