---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229740"
---
# <a name="interfaces"></a>Interfaces

Una interfaz define un contrato. Una clase o estructura que implementa una interfaz debe adherirse a su contrato. Una interfaz puede heredar de varias interfaces base, y una clase o struct puede implementar varias interfaces.

Las interfaces pueden contener métodos, propiedades, eventos e indizadores. La propia interfaz no proporciona implementaciones para los miembros que define. La interfaz sólo especifica a los miembros que se deben proporcionar mediante clases o structs que implementan la interfaz.

## <a name="interface-declarations"></a>Declaraciones de interfaz

Un *interface_declaration* es un *type_declaration* ([declaraciones de tipo](namespaces.md#type-declarations)) que declara un nuevo tipo de interfaz.

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

Un *interface_declaration* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *interface_modifier*s ([modificadores de interfaz](interfaces.md#interface-modifiers)), seguido de un elemento opcional `partial` modificador, seguido por la palabra clave `interface` y un *identificador* que los nombres de la interfaz, seguido de un elemento opcional *variant_type_parameter_list* especificación ([listas de parámetros de tipo variante](interfaces.md#variant-type-parameter-lists)), seguido de un elemento opcional *interface_base* especificación ([interfaces Base](interfaces.md#base-interfaces)), seguido de un elemento opcional *type_parameter_constraints_clause*especificación s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) , seguido por un *interface_body* ([cuerpo de la interfaz](interfaces.md#interface-body)), seguido opcionalmente de un punto y coma.

### <a name="interface-modifiers"></a>Modificadores de interfaz

Un *interface_declaration* puede incluir opcionalmente una secuencia de modificadores de interfaz:

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

Es un error en tiempo de compilación para el mismo modificador aparezca varias veces en una declaración de interfaz.

El `new` modificador sólo se permite en interfaces definidas dentro de una clase. Especifica que la interfaz oculta un miembro heredado con el mismo nombre, como se describe en [el nuevo modificador](classes.md#the-new-modifier).

El `public`, `protected`, `internal`, y `private` modificadores controlan el acceso de la interfaz. Dependiendo del contexto en el que se produce la declaración de interfaz, pueden permitir solo algunos de estos modificadores ([accesibilidad declarada](basic-concepts.md#declared-accessibility)).

### <a name="partial-modifier"></a>Modificador parcial

El `partial` modificador indica que esto *interface_declaration* es una declaración de tipo parcial. Combinan varias declaraciones parciales de interfaz con el mismo nombre dentro de una declaración de espacio de nombres o tipo envolvente a la declaración de una interfaz de formulario, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).

### <a name="variant-type-parameter-lists"></a>Listas de parámetros de tipo variante

Listas de parámetros de tipo de variante solo pueden realizarse con tipos de interfaz y delegado. La diferencia de normal *type_parameter_list*s es opcional *variance_annotation* en cada parámetro de tipo.

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

Si la anotación de varianza es `out`, se dice que el parámetro de tipo se ***covariante***. Si la anotación de varianza es `in`, se dice que el parámetro de tipo se ***contravariante***. Si no hay ninguna anotación de varianza, se dice que el parámetro de tipo sea ***invariable***.

En el ejemplo
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
`X` es covariante, `Y` es contravariante y `Z` es invariable.

#### <a name="variance-safety"></a>Seguridad de variación

La aparición de las anotaciones de varianza en la lista de parámetros de tipo de un tipo restringe los lugares donde pueden producirse tipos dentro de la declaración de tipos.

Un tipo `T` es ***salida unsafe*** si uno de los siguientes contiene:

*  `T` es un parámetro de tipo contravariante
*  `T` es un tipo de matriz con un tipo de elemento de salida no seguros
*  `T` es un tipo de interfaz o delegado `S<A1,...,Ak>` construido a partir de un tipo genérico `S<X1,...,Xk>` where para al menos una `Ai` uno de los siguientes contiene:
   * `Xi` es covariante o invariable y `Ai` no es seguro para la salida.
   * `Xi` es contravariante o invariable y `Ai` tiene seguridad de entrada.
   
Un tipo `T` es ***entrada unsafe*** si uno de los siguientes contiene:

*  `T` es un parámetro de tipo covariante
*  `T` es un tipo de matriz con un tipo de elemento de entrada no seguros
*  `T` es un tipo de interfaz o delegado `S<A1,...,Ak>` construido a partir de un tipo genérico `S<X1,...,Xk>` where para al menos una `Ai` uno de los siguientes contiene:
   * `Xi` es covariante o invariable y `Ai` no es seguro para la entrada.
   * `Xi` es contravariante o invariable y `Ai` no es seguro para la salida.

De manera intuitiva, un tipo de salida no seguros está prohibido en una posición de salida y un tipo de entrada no seguros está prohibido en una posición de entrada.

Es un tipo ***con seguridad de salida*** si no es segura de salida, y ***con seguridad de entrada*** si no no es seguro para la entrada.

#### <a name="variance-conversion"></a>Conversión de varianza

El propósito de las anotaciones de varianza es proporcionar conversiones más flexible (pero todavía seguridad de tipos) en tipos de interfaz y delegado. A este fin las definiciones de implícito ([conversiones implícitas](conversions.md#implicit-conversions)) y las conversiones explícitas ([las conversiones explícitas](conversions.md#explicit-conversions)) asegúrese de usar de la noción de varianza, que se define como sigue:

Un tipo `T<A1,...,An>` es convertible de varianza a un tipo `T<B1,...,Bn>` si `T` se declara una interfaz o un tipo de delegado con los parámetros de tipo de variante `T<X1,...,Xn>`y para cada parámetro de tipo de variante `Xi` uno de los siguientes contiene:

*  `Xi` es covariante y existe una conversión implícita de referencia o identidad de `Ai` a `Bi`
*  `Xi` es contravariante y una referencia implícita o no existe conversión de identidad de `Bi` a `Ai`
*  `Xi` es invariable y una identidad no existe conversión de `Ai` a `Bi`

### <a name="base-interfaces"></a>Interfaces base

Una interfaz puede heredar de cero o más tipos de interfaz, que se denominan el ***interfaces base explícitas*** de la interfaz. Cuando una interfaz tiene una o varias interfaces base explícitas, a continuación, en la declaración de dicha interfaz, el identificador de interfaz es seguido por dos puntos y comas separados por la lista de tipos de interfaz base.

```antlr
interface_base
    : ':' interface_type_list
    ;
```

Para un tipo de interfaz construida, las interfaces base explícitas se forman tomando las declaraciones de interfaz base explícita en la declaración de tipo genérico y sustituyendo, para cada *tipo_parámetro* en la interfaz base declaración, la correspondiente *type_argument* del tipo construido.

Las interfaces base explícitas de una interfaz deben ser al menos tan accesibles como la propia interfaz ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)). Por ejemplo, es un error en tiempo de compilación para especificar un `private` o `internal` interfaz en el *interface_base* de un `public` interfaz.

Es un error en tiempo de compilación para una interfaz para heredar directa o indirectamente de sí misma.

El ***interfaces base*** de una interfaz son las interfaces base explícitas y sus interfaces bases. En otras palabras, el conjunto de interfaces base es el cierre transitivo completando de las interfaces base explícitas, sus interfaces base explícitas y así sucesivamente. Una interfaz hereda a todos los miembros de sus interfaces base. En el ejemplo
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
las interfaces base de `IComboBox` son `IControl`, `ITextBox`, y `IListBox`.

En otras palabras, el `IComboBox` anterior de la interfaz hereda miembros `SetText` y `SetItems` como `Paint`.

Cada interfaz de la base de una interfaz debe tener la seguridad de salida ([seguridad varianza](interfaces.md#variance-safety)). Una clase o estructura que implementa una interfaz implícitamente también implementa todas las interfaces de la interfaz base.

### <a name="interface-body"></a>Cuerpo de la interfaz

El *interface_body* de una interfaz define los miembros de la interfaz.

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a>Miembros de interfaz

Los miembros de una interfaz son los miembros heredados de las interfaces base y los miembros declaran por la propia interfaz.

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

Una declaración de interfaz puede declarar a cero o más miembros. Los miembros de una interfaz deben ser métodos, propiedades, eventos o indizadores. Una interfaz no puede contener constantes, campos, operadores, constructores de instancias, destructores o tipos, ni puede una interfaz contener a miembros estáticos de ningún tipo.

Todos los miembros de interfaz tienen implícitamente acceso público. Es un error en tiempo de compilación para las declaraciones de miembros de interfaz incluir los modificadores. En concreto, los miembros de interfaces no se pueden declarar con los modificadores `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, o `static`.

El ejemplo
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
declara una interfaz que contiene uno de los posibles tipos de miembros: Un método, una propiedad, un evento y un indizador.

Un *interface_declaration* crea un nuevo espacio de declaración ([declaraciones](basic-concepts.md#declarations)) y el *interface_member_declaration*s contenidas inmediatamente en el *interface_declaration* introducir nuevos miembros en este espacio de declaración. Las siguientes reglas se aplican a *interface_member_declaration*s:

*  El nombre de un método debe ser diferente de los nombres de todas las propiedades y los eventos declarados en la misma interfaz. Además, la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un método debe ser diferentes de las firmas de todos los demás métodos declarados en la misma interfaz, y dos métodos declarados en la misma interfaz no pueden tener firmas que se diferencian solamente por `ref` y `out`.
*  El nombre de una propiedad o evento debe ser diferente de los nombres de todos los demás miembros declarados en la misma interfaz.
*  La firma de un indexador debe ser diferente de las firmas de los demás indexadores declarados en la misma interfaz.

Los miembros heredados de una interfaz específicamente no forman parte del espacio de declaración de la interfaz. Por lo tanto, una interfaz puede declarar a un miembro con el mismo nombre o signatura que un miembro heredado. Cuando esto ocurre, se dice que el miembro de interfaz derivada oculta al miembro de interfaz base. Ocultar a un miembro heredado no se considera un error, pero hace que el compilador emita una advertencia. Para suprimir la advertencia, la declaración del miembro de interfaz derivado debe incluir un `new` modificador para indicar que el miembro derivado está diseñado para ocultar el miembro base. En este tema se describe con más detalle en [ocultar mediante herencia](basic-concepts.md#hiding-through-inheritance).

Si un `new` modificador se incluye en una declaración que no oculta un miembro heredado, se emite una advertencia a tal efecto. Se puede suprimir esta advertencia mediante la eliminación de la `new` modificador.

Tenga en cuenta que los miembros de clase `object` no son estrictamente hablando, los miembros de cualquier interfaz ([miembros de interfaz](interfaces.md#interface-members)). Sin embargo, los miembros de clase `object` están disponibles a través de la búsqueda de miembros en cualquier tipo de interfaz ([búsqueda de miembros](expressions.md#member-lookup)).

### <a name="interface-methods"></a>Métodos de interfaz

Los métodos de interfaz se declaran mediante *interface_method_declaration*s:

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

El *atributos*, *return_type*, *identificador*, y *formal_parameter_list* de una declaración de método de interfaz tienen el mismo lo que significa que los de una declaración de método en una clase ([métodos](classes.md#methods)). No se permite una declaración de método de interfaz para especificar un cuerpo de método y la declaración, por tanto, siempre termina con un punto y coma.

Cada tipo de parámetros formales de un método de interfaz debe ser seguro para entrada ([seguridad varianza](interfaces.md#variance-safety)), y el tipo de valor devuelto debe ser `void` o seguridad de salida. Además, cada restricción de tipo de clase, la restricción de tipo de interfaz y la restricción de parámetro de tipo en cualquier parámetro de tipo del método deben ser con seguridad de entrada.

Estas reglas garantizan que cualquier covariante o contravariante uso de la interfaz mantiene la seguridad de tipos. Por ejemplo,
```csharp
interface I<out T> { void M<U>() where U : T; }
```
no es válido porque el uso de `T` como una restricción de parámetro de tipo en `U` no tiene seguridad de entrada.

Había esta restricción no sería posible infracción de seguridad de tipos de la siguiente manera:
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
Esto es realmente una llamada a `C.M<E>`. Pero esa llamada requiere que `E` derivan `D`, por lo que aquí se infringiría la seguridad de tipos.

### <a name="interface-properties"></a>Propiedades de la interfaz

Propiedades de la interfaz se declaran mediante *interface_property_declaration*s:

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

El *atributos*, *tipo*, y *identificador* de una declaración de propiedad de interfaz tienen el mismo significado que los de una declaración de propiedad en una clase ([ Propiedades](classes.md#properties)).

Los descriptores de acceso de una declaración de propiedad de interfaz corresponden a los descriptores de acceso de una declaración de propiedad de clase ([descriptores de acceso](classes.md#accessors)), salvo que el cuerpo del descriptor de acceso siempre debe ser un punto y coma. Por lo tanto, los descriptores de acceso es indican si la propiedad es de lectura y escritura, de solo lectura o solo escritura.

El tipo de una propiedad de interfaz debe ser seguro para la salida si hay un descriptor de acceso get y debe ser seguro para la entrada si hay un descriptor de acceso.

### <a name="interface-events"></a>Eventos de interfaz

Eventos de interfaz se declaran mediante *interface_event_declaration*s:

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

El *atributos*, *tipo*, y *identificador* de una declaración de evento de interfaz tienen el mismo significado que los de una declaración de evento en una clase ([eventos ](classes.md#events)).

El tipo de un evento de interfaz debe ser con seguridad de entrada.

### <a name="interface-indexers"></a>Indizadores en interfaces

Indizadores en interfaces se declaran mediante *interface_indexer_declaration*s:

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

El *atributos*, *tipo*, y *formal_parameter_list* una declaración de interfaz indizador tienen el mismo significado que los de una declaración de indizador en una clase ([ Los indizadores](classes.md#indexers)).

Los descriptores de acceso de una declaración de indexador de interfaz corresponden a los descriptores de acceso de una declaración de indizador de clase ([indizadores](classes.md#indexers)), salvo que el cuerpo del descriptor de acceso siempre debe ser un punto y coma. Por lo tanto, los descriptores de acceso es indican si el indizador es de lectura y escritura, de solo lectura o solo escritura.

Todos los tipos de parámetros formales de un indizador de interfaz deben ser con seguridad de entrada. Además, cualquier `out` o `ref` tipos de parámetros formales también deben ser seguro para salida. Tenga en cuenta que, incluso `out` parámetros son necesarios para tener seguridad de entrada, debido a una limitación de la plataforma subyacente de la ejecución.

El tipo de un indizador de interfaz debe ser seguro para la salida si hay un descriptor de acceso get y debe ser seguro para la entrada si hay un descriptor de acceso.

### <a name="interface-member-access"></a>Acceso a miembros de interfaz

Se tiene acceso a los miembros de interfaz a través de acceso a miembros ([acceso a miembros](expressions.md#member-access)) y el acceso de indizador ([acceso al indizador](expressions.md#indexer-access)) expresiones de la forma `I.M` y `I[A]`, donde `I` es un tipo de interfaz, `M` es un método, propiedad o evento de ese tipo de interfaz, y `A` es una lista de argumentos del indizador.

Para las interfaces que son estrictamente de herencia simple (cada interfaz en la cadena de herencia tiene exactamente cero o una interfaz base directa), los efectos de la búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)), la invocación de método ([ Las invocaciones de método](expressions.md#method-invocations)) y el acceso de indizador ([acceso al indizador](expressions.md#indexer-access)) las reglas son exactamente los mismos que para las clases y estructuras: Ocultar miembros más derivados de menos miembros derivados con el mismo nombre o la firma. Sin embargo, para las interfaces de herencia múltiple, las ambigüedades que pueden producirse cuando dos o más interfaces base no relacionadas declaran a miembros con el mismo nombre o signatura. En esta sección se muestra varios ejemplos de tales situaciones. En todos los casos, se pueden usar conversiones explícitas para resolver las ambigüedades.

En el ejemplo
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
las dos primeras instrucciones provocar errores en tiempo de compilación porque la búsqueda de miembros ([búsqueda de miembros](expressions.md#member-lookup)) de `Count` en `IListCounter` es ambiguo. Como se muestra en el ejemplo, la ambigüedad se resuelve mediante la conversión `x` para el tipo de interfaz base adecuada. Tales conversiones no tienen ningún costo de tiempo de ejecución, simplemente consisten en ver la instancia como un tipo menos derivado en tiempo de compilación.

En el ejemplo
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
la invocación `n.Add(1)` selecciona `IInteger.Add` aplicando las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution). De forma similar a la invocación `n.Add(1.0)` selecciona `IDouble.Add`. Cuando se insertan conversiones explícitas, hay solo un candidato método y, por tanto, no hay ambigüedad.

En el ejemplo
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
el `IBase.F` miembro está oculto por la `ILeft.F` miembro. La invocación `d.F(1)` , por tanto, se selecciona `ILeft.F`, aunque `IBase.F` parece no estar oculto en la ruta de acceso que le guíe por `IRight`.

La regla intuitiva para la ocultación en interfaces de herencia múltiple es simplemente el siguiente: Si un miembro está oculto en cualquier ruta de acceso, se oculta en todas las rutas de acceso. Dado que la ruta de acceso de `IDerived` a `ILeft` a `IBase` oculta `IBase.F`, también se oculta el miembro en la ruta de acceso de `IDerived` a `IRight` a `IBase`.

## <a name="fully-qualified-interface-member-names"></a>Nombres de miembro de interfaz completo

Un miembro de interfaz a veces se conoce por su ***nombre completo***. El nombre completo de un miembro de interfaz consta del nombre de la interfaz en la que el miembro se declara, seguido por un punto, seguido del nombre del miembro. El nombre completo de un miembro hace referencia a la interfaz en la que se declara el miembro. Por ejemplo, dadas las declaraciones
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
el nombre completo de `Paint` es `IControl.Paint` y el nombre completo de `SetText` es `ITextBox.SetText`.

En el ejemplo anterior, no es posible hacer referencia a `Paint` como `ITextBox.Paint`.

Cuando una interfaz es parte de un espacio de nombres, el nombre completo de un miembro de interfaz incluye el espacio de nombres. Por ejemplo
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

Aquí, el nombre completo de la `Clone` método es `System.ICloneable.Clone`.

## <a name="interface-implementations"></a>Implementaciones de interfaces

Interfaces se pueden implementar mediante clases y structs. Para indicar que una clase o estructura implementa directamente una interfaz, el identificador de interfaz se incluye en la lista de clases base de la clase o struct. Por ejemplo:
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

Una clase o estructura que implementa directamente una interfaz también directamente implementa todas las interfaces de base de la interfaz de forma implícita. Esto es cierto incluso si la clase o estructura no muestre explícitamente todas las interfaces base en la lista de clases base. Por ejemplo:
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

En este caso, la clase `TextBox` implementa tanto `IControl` y `ITextBox`.

Cuando una clase `C` directamente implementa una interfaz, todas las clases derivadas de C también implementa la interfaz implícitamente. Las interfaces base especificadas en una declaración de clase pueden ser tipos de interfaz construida ([construido tipos](types.md#constructed-types)). Una interfaz base no puede ser un parámetro de tipo por sí mismo, aunque puede implicar a los parámetros de tipo que están en ámbito. El código siguiente muestra cómo una clase puede implementar y ampliar los tipos construidos:
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

Las interfaces base de una declaración de clase genérica deben cumplir la regla de unicidad se describe en [exclusividad de interfaces implementadas](interfaces.md#uniqueness-of-implemented-interfaces).

### <a name="explicit-interface-member-implementations"></a>Implementaciones de miembros de interfaz explícita

Para fines de implementación de interfaces, puede declarar una clase o struct ***implementaciones de miembros de interfaz explícitas***. Una implementación de miembro de interfaz explícita es una declaración de método, propiedad, evento o indizador que hace referencia a un nombre de miembro de interfaz completo. Por ejemplo
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

Aquí `IDictionary<int,T>.this` y `IDictionary<int,T>.Add` es implementaciones de miembros de interfaz explícita.

En algunos casos, el nombre de un miembro de interfaz no puede ser adecuado para la clase de implementación en el que caso el miembro de interfaz puede implementarse mediante la implementación del miembro de interfaz explícita. Una clase que implementa una abstracción de archivo, por ejemplo, es probable que implementaría una `Close` función miembro que tiene el efecto de liberar el recurso de archivo e implemente el `Dispose` método de la `IDisposable` interfaz explícita de interfaz implementación del miembro:
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

No es posible tener acceso a una implementación de miembro de interfaz explícita a través de su nombre completo en una invocación de método, propiedad o acceso de indizador. Una implementación de miembro de interfaz explícita solo puede obtenerse a través de una instancia de interfaz y se hace referencia en ese caso simplemente por su nombre de miembro.

Es un error en tiempo de compilación para una implementación de miembro de interfaz explícita incluir los modificadores de acceso, y es un error en tiempo de compilación para incluir los modificadores `abstract`, `virtual`, `override`, o `static`.

Implementaciones de miembros de interfaz explícitas tienen características de accesibilidad diferente a otros miembros. Dado que las implementaciones de miembros de interfaz explícita nunca son accesibles a través de su nombre completo en una invocación de método o acceso a una propiedad, son en cierto sentido privadas. Sin embargo, puesto que puede tener acceso a través de una instancia de interfaz, están en un sentido públicas.

Implementaciones de miembros de interfaz explícitas tienen dos propósitos principales:

*  Dado que las implementaciones de miembros de interfaz explícita no son accesibles a través de las instancias de clase o struct, permiten implementaciones de interfaz que se excluirán de la interfaz pública de una clase o struct. Esto es especialmente útil cuando una clase o estructura implementa una interfaz interna que no es de interés a un consumidor de esa clase o struct.
*  Implementaciones de miembros de interfaz explícita permiten anulación de ambigüedades de los miembros de interfaz con la misma firma. Sin implementaciones de miembros de interfaz explícita sería imposible para una clase o struct que diferentes implementaciones de miembros con la misma firma de interfaz y el tipo de valor devuelto, como lo sería imposible que una clase o struct que cualquier implementación en todos los miembros de interfaz con la misma firma pero con diferentes tipos de valor devuelto.

Para que una implementación de miembro de interfaz explícita ser válido, la clase o estructura debe nombrar una interfaz en su lista de clases base que contiene a un miembro cuyo nombre completo, tipo y tipos de parámetros coincidan con los del miembro de interfaz explícita implementación de. Por lo tanto, en la siguiente clase
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
la declaración de `IComparable.CompareTo` da como resultado un error en tiempo de compilación porque `IComparable` no aparece en la lista de clases base `Shape` y no es una interfaz base de `ICloneable`. Del mismo modo, en las declaraciones
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
la declaración de `ICloneable.Clone` en `Ellipse` da como resultado un error en tiempo de compilación porque `ICloneable` no aparecen enumerados en la lista de clases base `Ellipse`.

El nombre completo de un miembro de interfaz debe hacer referencia a la interfaz en el que se declara el miembro. Por lo tanto, en las declaraciones
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
la implementación de miembro de interfaz explícita de `Paint` debe escribirse como `IControl.Paint`.

### <a name="uniqueness-of-implemented-interfaces"></a>Unicidad de interfaces implementadas

Las interfaces implementadas por una declaración de tipo genérico deben ser únicas para todos los posibles tipos construidos. Sin esta regla, sería imposible determinar el método correcto para llamar a determinados tipos construidos. Por ejemplo, supongamos que se permite una declaración de clase genérica que escribirse como sigue:
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

Eran permitiese, sería imposible determinar qué código se ejecute en el caso siguiente:
```csharp
I<int> x = new X<int,int>();
x.F();
```

Para determinar si la lista de interfaces de una declaración de tipo genérico es válida, se realizan los pasos siguientes:

*  Permiten `L` sea la lista de interfaces de especificar directamente en una clase genérica, estructura o declaración de interfaz `C`.
*  Agregar a `L` cualquier base de las interfaces de las interfaces ya existente en `L`.
*  Quite los duplicados de `L`.
*  Si cualquier posible construido creado a partir del tipo `C` después de que se sustituyen los argumentos de tipo en `L`, hacer que las dos interfaces de `L` que ser idénticos y, a continuación, la declaración de `C` no es válido. Las declaraciones de restricción no se consideran al determinar todos los tipos construidos posibles.

En la declaración de clase `X` anteriormente, la lista de interfaces `L` consta de `I<U>` y `I<V>`. La declaración no es válida porque cualquier tipo con construido `U` y `V` del mismo tipo haría que estas dos interfaces sean tipos idénticos.

Es posible que las interfaces especificadas en los niveles de herencia diferente para unificar:
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

Este código es válido aunque `Derived<U,V>` implementa tanto `I<U>` y `I<V>`. El código
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
invoca el método de `Derived`, puesto que `Derived<int,int>` vuelve a implementar eficazmente `I<int>` ([volver a implementar la interfaz](interfaces.md#interface-re-implementation)).

### <a name="implementation-of-generic-methods"></a>Implementación de métodos genéricos

Cuando un método genérico implementa de manera implícita un método de interfaz, las restricciones proporcionadas para cada parámetro de tipo de método de donde debe ser equivalente en ambas declaraciones (después de cualquier tipo de interfaz parámetros se reemplazan con los argumentos de tipo adecuado), método parámetros de tipo se identifican mediante posiciones ordinales, de izquierda a derecha.

Cuando un método genérico implementa explícitamente un método de interfaz, sin embargo, se permiten sin restricciones en el método de implementación. En su lugar, las restricciones se heredan del método de interfaz

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

El método `C.F<T>` implementa de manera implícita `I<object,C,string>.F<T>`. En este caso, `C.F<T>` no se requiere (ni permiten) para especificar la restricción `T:object` porque `object` es una restricción en todos los parámetros de tipo implícita. El método `C.G<T>` implementa de manera implícita `I<object,C,string>.G<T>` porque las restricciones coinciden con los de la interfaz, después de los parámetros de tipo de interfaz se reemplazan por los argumentos de tipo correspondiente. La restricción para el método `C.H<T>` es un error porque los tipos sellados (`string` en este caso) no se puede usar como restricciones. Si se omite la restricción también sería un error ya que deben coincidir con las restricciones de las implementaciones de método de interfaz implícita. Por lo tanto, no podrá implementar de manera implícita `I<object,C,string>.H<T>`. Este método de interfaz solo puede implementarse mediante una implementación de miembro de interfaz explícita:
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

En este ejemplo, la implementación de miembro de interfaz explícita invoca un método público tener restricciones estrictamente más débiles. Tenga en cuenta que la asignación de `t` a `s` es válido desde `T` hereda una restricción de `T:string`, aunque esta restricción no se pueden expresar en código fuente.

### <a name="interface-mapping"></a>Asignación de interfaz

Una clase o estructura debe proporcionar implementaciones de todos los miembros de las interfaces que se muestran en la lista de clases base de la clase o struct. El proceso de buscar las implementaciones de miembros de interfaz en una clase o struct se conoce como ***asignación de interfaz***.

Asignación de interfaz para una clase o struct `C` localiza una implementación para cada miembro de cada interfaz especificada en la lista de clases base `C`. La implementación de un miembro de interfaz determinado `I.M`, donde `I` es la interfaz en la que el miembro `M` se declara, se determina examinando cada clase o struct `S`, comenzando por `C` y repetir para cada clase base sucesiva de `C`, hasta que se encuentra una coincidencia:

*  Si `S` contiene una declaración de una implementación de miembro de interfaz explícita que coincida con `I` y `M`, a continuación, este miembro es la implementación de `I.M`.
*  De lo contrario, si `S` contiene una declaración de un miembro público no estático que coincide con `M`, a continuación, este miembro es la implementación de `I.M`. Si más de un miembro de la coincidencia, no se especifica qué miembro es la implementación de `I.M`. Esta situación solo puede producirse si `S` es un tipo construido, donde los dos miembros, como se declara en el tipo genérico tienen firmas diferentes, pero los argumentos de tipo que sea idéntico sus firmas.

Se produce un error de tiempo de compilación si las implementaciones no se encuentra para todos los miembros de todas las interfaces especificadas en la lista de clases base `C`. Tenga en cuenta que los miembros de una interfaz incluyen a aquellos miembros que se heredan de las interfaces base.

Para los fines de asignación de interfaz, un miembro de clase `A` coincide con un miembro de interfaz `B` cuando:

*  `A` y `B` son métodos y el nombre, tipo y listas de parámetros formales de `A` y `B` son idénticos.
*  `A` y `B` son propiedades, el nombre y tipo de `A` y `B` son idénticos, y `A` tiene el mismos descriptores de acceso como `B` (`A` puede tener los descriptores de acceso adicionales si no es una interfaz explícita implementación de miembro).
*  `A` y `B` son eventos y el nombre y tipo de `A` y `B` son idénticos.
*  `A` y `B` son los indizadores, el tipo y listas de parámetros formales de `A` y `B` son idénticos, y `A` tiene el mismos descriptores de acceso como `B` (`A` puede tener los descriptores de acceso adicionales si no es un implementación de miembro de interfaz explícita).

Implicaciones importantes del algoritmo de asignación de interfaz son:

*  Implementaciones de miembros de interfaz explícitos tienen prioridad sobre otros miembros en la misma clase o struct al determinar al miembro de clase o estructura que implementa a un miembro de interfaz.
*  Los miembros no públicos ni estáticos participan en la asignación de interfaz.

En el ejemplo
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
el `ICloneable.Clone` miembro de `C` se convierte en la implementación de `Clone` en `ICloneable` porque las implementaciones de miembros de interfaz explícitos tienen prioridad sobre los demás miembros.

Si una clase o estructura implementa dos o más interfaces que contiene a un miembro con el mismo nombre, tipo y tipos de parámetros, es posible asignar cada uno de esos miembros de interfaz a un único miembro de clase o struct. Por ejemplo
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

En este caso, el `Paint` métodos de ambos `IControl` y `IForm` se asignan a la `Paint` método `Page`. Por supuesto, también es posible tener implementaciones de miembros de interfaz explícita independiente para los dos métodos.

Si una clase o estructura implementa una interfaz que contiene a los miembros ocultos, necesariamente deben implementarse algunos miembros a través de implementaciones de miembros de interfaz explícita. Por ejemplo
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

Una implementación de esta interfaz requeriría al menos una implementación de miembro de interfaz explícita y tomaría una de las siguientes formas
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

Cuando una clase implementa varias interfaces que tienen la misma interfaz base, puede haber solo una implementación de la interfaz base. En el ejemplo
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
no es posible tener implementaciones independientes para la `IControl` mencionada en la lista de clases base, el `IControl` heredan `ITextBox`y el `IControl` heredan `IListBox`. De hecho, no hay ninguna noción de una identidad diferente para estas interfaces. En su lugar, las implementaciones de `ITextBox` y `IListBox` comparten la misma implementación de `IControl`, y `ComboBox` simplemente se considera implementar tres interfaces `IControl`, `ITextBox`, y `IListBox`.

Los miembros de una clase base participan en la asignación de interfaz. En el ejemplo
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
el método `F` en `Class1` se usa en `Class2`de implementación de `Interface1`.

### <a name="interface-implementation-inheritance"></a>Herencia de la implementación de interfaz

Una clase hereda todas las implementaciones de interfaz proporcionadas por sus clases base.

Sin explícitamente ***volver a implementar*** una interfaz, una clase derivada no puede alterar de ningún modo las asignaciones de interfaz hereda de sus clases base. Por ejemplo, en las declaraciones
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
el `Paint` método `TextBox` oculta la `Paint` método `Control`, pero no modifica la asignación de `Control.Paint` en `IControl.Paint`y las llamadas a `Paint` a través de la clase de instancias y las instancias de la interfaz le tiene los siguientes efectos
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

Sin embargo, cuando un método de interfaz se asigna a un método virtual en una clase, es posible que las clases derivadas reemplazar el método virtual y modificar la implementación de la interfaz. Por ejemplo, volver a escribir las declaraciones anteriores a
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
Ahora se tendrán en cuenta los siguientes efectos
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

Puesto que no se pueden declarar implementaciones de interfaz explícita miembros virtuales, no es posible invalidar una implementación de miembro de interfaz explícita. Sin embargo, es perfectamente válido para una implementación de miembro de interfaz explícita llamar a otro método, y que otro método puede declararse virtual para permitir que las clases derivadas deben invalidarla. Por ejemplo
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

En este caso, las clases derivadas de `Control` puede especializar la implementación de `IControl.Paint` invalidando el `PaintControl` método.

### <a name="interface-re-implementation"></a>Reimplementación de interfaz

Se permite una clase que hereda de una implementación de interfaz ***volver a implementar*** la interfaz mediante su inclusión en la lista de clases base.

Una reimplementación de una interfaz sigue exactamente la misma interfaz reglas de asignación como una implementación inicial de una interfaz. Por lo tanto, la asignación de interfaz heredada no tiene efecto alguno en la asignación de interfaz establecido para la reimplementación de la interfaz. Por ejemplo, en las declaraciones
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
el hecho de que `Control` asigna `IControl.Paint` en `Control.IControl.Paint` no afecta a la reimplementación en `MyControl`, que se asigna `IControl.Paint` en `MyControl.Paint`.

Hereda las declaraciones de miembro público y miembro de interfaz explícita heredada declaraciones participan en el proceso de asignación de interfaz para las interfaces implementadas. Por ejemplo
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

Aquí, la implementación de `IMethods` en `Derived` asigna los métodos de interfaz en `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, y `Base.I`.

Cuando una clase implementa una interfaz, implícitamente también implementa todas las interfaces de base de la interfaz de. Del mismo modo, una reimplementación de una interfaz también es implícitamente una reimplementación de todas las interfaces de la interfaz base. Por ejemplo
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

Aquí, la reimplementación de `IDerived` también vuelve a implementar `IBase`, la asignación `IBase.F` en `D.F`.

### <a name="abstract-classes-and-interfaces"></a>Interfaces y clases abstractas

Al igual que una clase no abstracta, una clase abstracta debe proporcionar implementaciones de todos los miembros de las interfaces que se muestran en la lista de clases base de la clase. Sin embargo, se permite una clase abstracta para asignar los métodos de interfaz a métodos abstractos. Por ejemplo
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

Aquí, la implementación de `IMethods` asigna `F` y `G` a métodos abstractos, que debe invalidarse en clases no abstractas que derivan de `C`.

Tenga en cuenta que las implementaciones de miembros de interfaz explícita no pueden ser abstractas, pero obviamente se permiten las implementaciones de miembros de interfaz explícita para llamar a métodos abstractos. Por ejemplo
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

Aquí, las clases no abstractas que derivan de `C` sería necesario para invalidar `FF` y `GG`, lo que proporciona la implementación real de `IMethods`.
