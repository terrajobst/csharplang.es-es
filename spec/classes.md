---
ms.openlocfilehash: 917e2f1e196013f85eefbda21fb3d717cc681084
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193909"
---
# <a name="classes"></a>Clases

Una clase es una estructura de datos que puede contener miembros de datos (constantes y campos), miembros de función (métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos) y tipos anidados. Tipos de clase admiten la herencia, un mecanismo mediante el cual una clase derivada puede extender y especializar una clase base.

## <a name="class-declarations"></a>Declaraciones de clase

Un *class_declaration* es un *type_declaration* ([declaraciones de tipo](namespaces.md#type-declarations)) que declara una clase nueva.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

Un *class_declaration* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *class_modifier*s ([modificadores de clase](classes.md#class-modifiers)), seguido de un elemento opcional `partial` modificador, seguido por la palabra clave `class` y un *identificador* que los nombres de la clase, seguida por un opcional *type_parameter_list* ([parámetros de tipo](classes.md#type-parameters)), seguido de un elemento opcional *class_base* especificación ([base clase especificación de](classes.md#class-base-specification)), seguido de un conjunto opcional de *type_parameter_constraints_clause*s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), seguido de un *class_body*  ([Cuerpo de la clase](classes.md#class-body)), seguido opcionalmente de un punto y coma.

No se puede proporcionar una declaración de clase *type_parameter_constraints_clause*s a menos que también proporciona un *type_parameter_list*.

Una declaración de clase que proporciona un *type_parameter_list* es un ***declaración de clase genérica***. Además, cualquier clase anidada dentro de una declaración de clase genérica o una declaración de struct genérico es una declaración de clase genérica, puesto que se deben proporcionar parámetros de tipo para el tipo de contenedor para crear un tipo construido.

### <a name="class-modifiers"></a>Modificadores de clase

Un *class_declaration* puede incluir opcionalmente una secuencia de modificadores de clase:

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

Es un error en tiempo de compilación para el mismo modificador aparezca varias veces en una declaración de clase.

El `new` modificador está permitido en clases anidadas. Especifica que la clase oculta un miembro heredado con el mismo nombre, como se describe en [el nuevo modificador](classes.md#the-new-modifier). Es un error en tiempo de compilación para el `new` modificador aparezca en una declaración de clase que no es una declaración de clase anidada.

El `public`, `protected`, `internal`, y `private` modificadores controlan el acceso de la clase. Dependiendo del contexto en el que se produce la declaración de clase, algunos de estos modificadores pueden no estar permitidos ([accesibilidad declarada](basic-concepts.md#declared-accessibility)).

El `abstract`, `sealed` y `static` modificadores se tratan en las secciones siguientes.

#### <a name="abstract-classes"></a>Clases abstractas

El `abstract` modificador se usa para indicar que una clase está incompleta y que está pensada para usarse solo como una clase base. Una clase abstracta difiere de una clase no abstracta de las maneras siguientes:

*  Una clase abstracta no se pueden crear instancias directamente, y es un error en tiempo de compilación para usar el `new` operador en una clase abstracta. Aunque es posible tener variables y valores cuyos tipos en tiempo de compilación son abstractos, tales variables y valores necesariamente será `null` o contendrán referencias a instancias de clases no abstractas derivadas de los tipos abstractos.
*  Una clase abstracta se permiten (aunque no es necesario) para contener miembros abstractos.
*  Una clase abstracta no puede estar sellada.

Cuando una clase no abstracta se deriva de una clase abstracta, la clase no abstracta debe incluir implementaciones reales de todos los miembros abstractos heredados, anulando a esos miembros abstractos. En el ejemplo
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
la clase abstracta `A` presenta un método abstracto `F`. Clase `B` presenta un método adicional `G`, pero proporciona una implementación de `F`, `B` debe declararse como abstracta. Clase `C` invalida `F` y proporciona una implementación real. Puesto que no hay ningún miembro abstracto en `C`, `C` se permitía (aunque no es necesario) para ser no abstracta.

#### <a name="sealed-classes"></a>Clases selladas

El `sealed` modificador se usa para impedir la derivación de una clase. Si se especifica una clase sellada como clase base de otra clase, se produce un error de tiempo de compilación.

Una clase sellada no puede ser también una clase abstracta.

El `sealed` modificador se usa principalmente para evitar una derivación imprevista, pero también permite ciertas optimizaciones en tiempo de ejecución. En concreto, dado que una clase sellada no puede tener clases derivadas, es posible transformar las invocaciones de miembros de función virtual en instancias de una clase sellada en llamadas no virtuales.

#### <a name="static-classes"></a>Clases estáticas

El `static` modificador se usa para marcar la clase que se declara como un ***clase estática***. Una clase estática no pueden crearse instancias, no se puede usar como un tipo y puede contener a solo miembros estáticos. Sólo una clase estática puede contener declaraciones de métodos de extensión ([métodos de extensión](classes.md#extension-methods)).

Una declaración de clase estática está sujeto a las restricciones siguientes:

*  Una clase estática no puede incluir un `sealed` o `abstract` modificador. Sin embargo, tenga en cuenta que puesto que no se crea una instancia o derivada de una clase estática, se comporta como si fuese sellada y abstracta.
*  Una clase estática no puede incluir un *class_base* especificación ([clase Especificación base](classes.md#class-base-specification)) y no se puede especificar explícitamente una clase base o una lista de interfaces implementadas. Una clase estática hereda implícitamente de tipo `object`.
*  Una clase estática solo puede contener miembros estáticos ([miembros estáticos y de instancia](classes.md#static-and-instance-members)). Tenga en cuenta que las constantes y tipos anidados se clasifican como miembros estáticos.
*  Una clase estática no puede tener miembros con `protected` o `protected internal` accesibilidad declarada.

Es un error en tiempo de compilación que infringe cualquiera de estas restricciones.

Una clase estática no tiene ningún constructor de instancia. No es posible declarar un constructor de instancia en una clase estática y ningún constructor de instancia predeterminado ([constructores predeterminados](classes.md#default-constructors)) se proporciona para una clase estática.

Los miembros de una clase estática no son automáticamente estáticos y las declaraciones de miembro deben incluir explícitamente una `static` modificador (excepto las constantes y tipos anidados). Cuando una clase se anida dentro de una clase estática externa, la clase anidada no es una clase estática a menos que incluya explícitamente un `static` modificador.

__Hacer referencia a tipos de clase estática__

Un *namespace_or_type_name* ([Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)) tiene permiso para hacer referencia a una clase estática si

*  El *namespace_or_type_name* es el `T` en un *namespace_or_type_name* del formulario `T.I`, o
*  El *namespace_or_type_name* es el `T` en un *typeof_expression* ([listas de argumentos](expressions.md#argument-lists)1) del formulario `typeof(T)`.

Un *primary_expression* ([miembros de función](expressions.md#function-members)) tiene permiso para hacer referencia a una clase estática si

*  El *primary_expression* es el `E` en un *member_access* ([comprobación de la resolución de sobrecarga dinámicas de tiempo de compilación](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) del formulario `E.I`.

En cualquier otro contexto es un error en tiempo de compilación para hacer referencia a una clase estática. Por ejemplo, es un error para una clase estática que se usará como una clase base, un tipo constituyente ([tipos anidados](classes.md#nested-types)) de un miembro, un argumento de tipo genérico o una restricción de parámetro de tipo. Del mismo modo, no se puede usar una clase estática en un tipo de matriz, un tipo de puntero, un `new` expresión, una expresión de conversión, un `is` expresión, un `as` expresión, un `sizeof` expresión o una expresión de valor predeterminado.

### <a name="partial-modifier"></a>Modificador parcial

El `partial` modificador se usa para indicar que este *class_declaration* es una declaración de tipo parcial. Combinan varias declaraciones parciales de tipos con el mismo nombre dentro de una declaración de espacio de nombres o tipo envolvente a la declaración de un tipo de formulario, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).

Tener la declaración de una clase que se distribuye en segmentos independientes del texto del programa puede ser útil si son producidos o mantenerse en contextos diferentes estos segmentos. Por ejemplo, una parte de una declaración de clase puede ser generan automáticamente, mientras que la otra se crea manualmente. Separación textual de los dos impide las actualizaciones en uno respecto a entrar en conflicto con las actualizaciones de otros.

### <a name="type-parameters"></a>Parámetros de tipo

Un parámetro de tipo es un identificador simple que denota un marcador de posición para un argumento de tipo proporcionado para crear un tipo construido. Un parámetro de tipo es un marcador de posición formal para un tipo que se proporcionarán más adelante. Por el contrario, un argumento de tipo ([argumentos de tipo](types.md#type-arguments)) es el tipo real que se sustituye por el parámetro de tipo cuando se crea un tipo construido.

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

Cada parámetro de tipo en una declaración de clase define un nombre en el espacio de declaración ([declaraciones](basic-concepts.md#declarations)) de esa clase. Por lo tanto, no puede tener el mismo nombre que otro parámetro de tipo o un miembro declarado en esa clase. Un parámetro de tipo no puede tener el mismo nombre que el propio tipo.

### <a name="class-base-specification"></a>Especificación de clase base

Una declaración de clase puede incluir un *class_base* especificación, que define la clase base directa de la clase y las interfaces ([Interfaces](interfaces.md)) directamente implementada por la clase.

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

La clase base especificada en una declaración de clase puede ser un tipo de clase construida ([construido tipos](types.md#constructed-types)). Una clase base no puede ser un parámetro de tipo por sí mismo, aunque puede implicar a los parámetros de tipo que están en ámbito.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Clases base

Cuando un *class_type* se incluye en el *class_base*, especifica la clase base directa de la clase que se declara. Si una declaración de clase no tiene ningún *class_base*, o si el *class_base* listas sólo los tipos de interfaz, se supone que la clase base directa se `object`. Una clase hereda los miembros de su clase base directa, como se describe en [herencia](classes.md#inheritance).

En el ejemplo
```csharp
class A {}

class B: A {}
```
clase `A` se dice que la clase base directa de `B`, y `B` se dice que se derive de `A`. Puesto que `A` does explícitamente no especificar una clase base directa, su clase base directa es implícitamente `object`.

Para un tipo de clase construida, si una clase base se especifica en la declaración de clase genérica, se obtiene la clase base del tipo construido sustituyendo, para cada *tipo_parámetro* en la declaración de clase base correspondiente *type_argument* del tipo construido. Dadas las declaraciones de clase genérica
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
la clase base del tipo construido `G<int>` sería `B<string,int[]>`.

La clase base directa de un tipo de clase debe ser al menos tan accesible como el propio tipo de clase ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)). Por ejemplo, es un error en tiempo de compilación para un `public` clase derive de una `private` o `internal` clase.

La clase base directa de un tipo de clase no debe ser cualquiera de los siguientes tipos: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, o `System.ValueType`. Además, no se puede usar una declaración de clase genérica `System.Attribute` como una clase base directa o indirecta.

Al determinar el significado de la especificación de la clase base directa `A` de una clase `B`, la clase base directa de `B` temporalmente se supone que es `object`. Intuitivamente Esto garantiza que el significado de una especificación de clase base no pueden hacerlo de forma recursiva depender de sí misma. El ejemplo:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
es un error desde en la especificación de la clase base `A<C.B>` la clase base directa de `C` se considera `object`y por lo tanto, (según las reglas del [Namespace y nombres de tipo](basic-concepts.md#namespace-and-type-names)) `C` no se considera tener un miembro `B`.

Las clases base de un tipo de clase son la clase base directa y sus clases base. En otras palabras, el conjunto de clases bases es el cierre transitivo de la relación de clase base directa. Que hace referencia en el ejemplo anterior, las clases base de `B` son `A` y `object`. En el ejemplo
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
las clases base de `D<int>` son `C<int[]>`, `B<IComparable<int[]>>`, `A`, y `object`.

Excepto para la clase `object`, cada tipo de clase tiene exactamente una clase base directa. La `object` clase no tiene ninguna clase base directa y es la clase base fundamental de todas las demás clases.

Cuando una clase `B` deriva una clase `A`, es un error de tiempo de compilación de `A` depender `B`. Una clase ***depende directamente de*** su clase base directa (si existe) y ***depende directamente de*** la clase en el que está inmediatamente anidada (si existe). Dada esta definición, el conjunto completo de las clases de los que depende una clase es el cierre reflexivo y transitivo de la ***depende directamente*** relación.

El ejemplo
```csharp
class A: A {}
```
es incorrecto porque depende de la clase en sí mismo. Del mismo modo, el ejemplo
```csharp
class A: B {}
class B: C {}
class C: A {}
```
es un error porque las clases tienen una dependencia circular entre ellas. Por último, el ejemplo
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
se produce un error en tiempo de compilación porque `A` depende `B.C` (su clase base directa), que depende de `B` (su clase inmediatamente envolvente), que depende de forma circular `A`.

Tenga en cuenta que una clase no depende de las clases que están anidadas dentro de él. En el ejemplo
```csharp
class A
{
    class B: A {}
}
```
`B` depende de `A` (porque `A` es su clase base directa y su clase inmediatamente envolvente), pero `A` no depende de `B` (puesto que `B` no es una clase base ni una clase contenedora de `A` ). Por lo tanto, el ejemplo es válido.

No es posible derivar un `sealed` clase. En el ejemplo
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
clase `B` es un error porque intenta derivar el `sealed` clase `A`.

#### <a name="interface-implementations"></a>Implementaciones de interfaces

Un *class_base* especificación puede incluir una lista de tipos de interfaz, en cuyo caso la clase se dice implementar directamente los tipos de interfaz determinado. Implementaciones de interfaz se tratan con más detalle en [implementaciones de interfaz](interfaces.md#interface-implementations).

### <a name="type-parameter-constraints"></a>Restricciones de parámetro de tipo

Las declaraciones de tipo y método genéricas pueden especificar las restricciones del parámetro mediante la inclusión de *type_parameter_constraints_clause*s.

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

Cada *type_parameter_constraints_clause* consta del token `where`, seguido del nombre de un parámetro de tipo, seguido de dos puntos y la lista de restricciones para ese parámetro de tipo. Puede haber a lo sumo un `where` cláusula para cada parámetro de tipo y el `where` cláusulas pueden aparecer en cualquier orden. Al igual que el `get` y `set` tokens en un descriptor de acceso de propiedad, el `where` símbolo (token) no es una palabra clave.

La lista de restricciones en un `where` cláusula puede incluir cualquiera de los siguientes componentes, en este orden: una restricción principal única, una o más restricciones secundarias y la restricción de constructor `new()`.

Una restricción principal puede ser un tipo de clase o el ***hacen referencia a la restricción de tipo*** `class` o ***restricción de tipo de valor*** `struct`. Una restricción secundaria puede ser un *tipo_parámetro* o *interface_type*.

La restricción de tipo de referencia especifica que un argumento de tipo utilizado para el parámetro de tipo debe ser un tipo de referencia. Todos los tipos de clase, tipos de interfaz, tipos de delegado, los tipos de matriz y los parámetros de tipo que se sabe que es un tipo de referencia (como se define a continuación) se aplica esta restricción.

La restricción de tipo de valor especifica que un argumento de tipo utilizado para el parámetro de tipo debe ser un tipo de valor distinto de NULL. Todos los tipos de estructura que no aceptan valores NULL, los tipos de enumeración y los parámetros de tipo que tiene la restricción de tipo de valor se aplica esta restricción. Tenga en cuenta que aunque se clasifica como un tipo de valor, un tipo que acepta valores null ([tipos que aceptan valores NULL](types.md#nullable-types)) no satisface la restricción de tipo de valor. Un parámetro de tipo que tiene la restricción de tipo de valor no puede tener también el *constructor_constraint*.

Tipos de puntero nunca pueden ser argumentos de tipo y no se consideran para satisfacer cualquier las referencia valor o tipo de restricciones de tipo.

Si una restricción es un tipo de clase, un tipo de interfaz o un parámetro de tipo, dicho tipo especifica un tipo base"mínimo" que debe ser compatibles con cada argumento de tipo utilizado para ese parámetro de tipo. Cada vez que se utiliza un tipo construido o método genérico, el argumento de tipo se compara con las restricciones en el parámetro de tipo en tiempo de compilación. El argumento de tipo proporcionado debe cumplir las condiciones descritas en [que satisfacen las restricciones](types.md#satisfying-constraints).

Un *class_type* restricción debe cumplir las reglas siguientes:

*  El tipo debe ser un tipo de clase.
*  El tipo no debe ser `sealed`.
*  El tipo no debe ser uno de los siguientes tipos: `System.Array`, `System.Delegate`, `System.Enum`, o `System.ValueType`.
*  El tipo no debe ser `object`. Dado que todos los tipos que derivan de `object`, este tipo de restricción tendría ningún efecto si se permitiera.
*  A lo sumo una restricción para un parámetro de tipo especificado puede ser un tipo de clase.

Un tipo especificado como un *interface_type* restricción debe cumplir las reglas siguientes:

*  El tipo debe ser un tipo de interfaz.
*  Un tipo no debe especificarse más de una vez en un determinado `where` cláusula.

En cualquier caso, la restricción puede incluir cualquiera de los parámetros de tipo del tipo asociado o declaración de método como parte de un tipo construido y puede implicar al tipo que se declara.

Cualquier tipo de clase o interfaz especificado como una restricción de parámetro de tipo debe ser al menos tan accesible ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)) como el tipo o método genérico que se declara.

Un tipo especificado como un *tipo_parámetro* restricción debe cumplir las reglas siguientes:

*  El tipo debe ser un parámetro de tipo.
*  Un tipo no debe especificarse más de una vez en un determinado `where` cláusula.

Además no debe haber ningún ciclo en el gráfico de dependencias de parámetros de tipo, donde dependencia es una relación transitiva definida por:

*  Si un parámetro de tipo `T` se utiliza como una restricción de parámetro de tipo `S` , a continuación, `S` ***depende*** `T`.
*  Si un parámetro de tipo `S` depende de un parámetro de tipo `T` y `T` depende de un parámetro de tipo `U` , a continuación, `S` ***depende*** `U`.

Dada a esta relación, es un error en tiempo de compilación de un parámetro de tipo a depender de sí misma (directa o indirectamente).

Las restricciones deben ser coherentes entre los parámetros de tipo dependiente. Si el parámetro de tipo `S` depende del parámetro de tipo `T` a continuación:

*  `T` no se debe tener la restricción de tipo de valor. En caso contrario, `T` está sellado eficazmente lo `S` hubiera estado obligado a ser el mismo tipo que `T`, eliminando la necesidad de dos parámetros de tipo.
*  Si `S` tiene la restricción de tipo de valor, a continuación, `T` no debe tener un *class_type* restricción.
*  Si `S` tiene un *class_type* restricción `A` y `T` tiene un *class_type* restricción `B` , a continuación, debe haber una conversión de identidad o implícita hacer referencia a la conversión de `A` a `B` o una conversión implícita de referencia de `B` a `A`.
*  Si `S` también depende del parámetro de tipo `U` y `U` tiene un *class_type* restricción `A` y `T` tiene un *class_type* restricción `B` , a continuación, debe haber una conversión de identidad o una conversión implícita de referencia de `A` a `B` o una conversión implícita de referencia de `B` a `A`.

Es válido para `S` tener la restricción de tipo de valor y `T` a tiene la restricción de tipo de referencia. Esto limita `T` a los tipos de `System.Object`, `System.ValueType`, `System.Enum`y cualquier tipo de interfaz.

Si el `where` cláusula para un parámetro de tipo incluye una restricción de constructor (que tiene el formato `new()`), es posible utilizar el `new` operador para crear instancias del tipo ([lasexpresionesdecreacióndeobjetos](expressions.md#object-creation-expressions)). Cualquier tipo de argumento utilizado para un parámetro de tipo con una restricción de constructor debe tener un constructor público sin parámetros (este constructor existe de manera implícita para cualquier tipo de valor) o ser un parámetro de tipo con la restricción de tipo de valor o la restricción de constructor (consulte la [Restricciones de parámetro de tipo](classes.md#type-parameter-constraints) para obtener más información).

Los siguientes son ejemplos de restricciones:
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

El ejemplo siguiente es error, porque provoca una circularidad en el gráfico de dependencias de los parámetros de tipo:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

Los ejemplos siguientes muestran otras situaciones no válidas:
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

El ***clase base efectiva*** de un parámetro de tipo `T` se define como sigue:

*  Si `T` no tiene ninguna restricción principal o restricciones de parámetro de tipo, su clase base efectiva es `object`.
*  Si `T` tiene la restricción de tipo de valor, su clase base efectiva es `System.ValueType`.
*  Si `T` tiene un *class_type* restricción `C` pero no *tipo_parámetro* restricciones, su clase base efectiva es `C`.
*  Si `T` no tiene ningún *class_type* restricción, pero tiene uno o varios *tipo_parámetro* restricciones, su clase base efectiva es el tipo más abarcado ([eleva la conversión operadores](conversions.md#lifted-conversion-operators)) en el conjunto de clases base efectivas de su *tipo_parámetro* restricciones. Las reglas de coherencia Asegúrese de que existe un tipo más abarcado.
*  Si `T` tiene tanto un *class_type* restricción y uno o más *tipo_parámetro* restricciones, su clase base efectiva es el tipo más abarcado ([eleva la conversión operadores](conversions.md#lifted-conversion-operators)) en el conjunto formado por el *class_type* restricción de `T` y las clases base efectiva de su *tipo_parámetro* restricciones. Las reglas de coherencia Asegúrese de que existe un tipo más abarcado.
*  Si `T` tiene la restricción de tipo de referencia, pero no *class_type* restricciones, su clase base efectiva es `object`.

Con el propósito de estas reglas, si T tiene una restricción `V` que es un *value_type*, utilice en su lugar el tipo más específico de base de `V` que es un *class_type*. Esto no puede ocurrir nunca en una restricción dada explícitamente, pero puede producirse cuando una declaración de método de reemplazo o una implementación explícita de un método de interfaz implícitamente heredan las restricciones de un método genérico.

Estas reglas garantizan que la clase base efectiva es siempre un *class_type*.

El ***conjunto de interfaces efectivas*** de un parámetro de tipo `T` se define como sigue:

*  Si `T` no tiene ningún *secondary_constraints*, su conjunto de interfaces efectivas está vacío.
*  Si `T` tiene *interface_type* restricciones, pero no *tipo_parámetro* restricciones, su conjunto de interfaces efectivas es su conjunto de *interface_type* restricciones.
*  Si `T` no tiene ningún *interface_type* , pero tiene restricciones *tipo_parámetro* restricciones, su conjunto de interfaces efectiva es la unión de los conjuntos de interfaz eficaz de su *tipo_ parámetro* restricciones.
*  Si `T` tiene ambos *interface_type* restricciones y *tipo_parámetro* restricciones, su conjunto de interfaces efectiva es la unión de su conjunto de *interface_type* las restricciones y los conjuntos de interfaz eficaz de su *tipo_parámetro* restricciones.

Es un parámetro de tipo ***sabe que es un tipo de referencia*** si tiene la restricción de tipo de referencia o no es de su clase base efectiva `object` o `System.ValueType`.

Los valores de un tipo de parámetro de tipo restringido pueden utilizarse para tener acceso a los miembros de instancia implicados en las restricciones. En el ejemplo
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
los métodos de `IPrintable` se puede invocar directamente en `x` porque `T` está restringido a implementar siempre `IPrintable`.

### <a name="class-body"></a>Cuerpo de la clase

El *class_body* de una clase define los miembros de esa clase.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Tipos parciales

Una declaración de tipos se puede dividir entre varios ***declaraciones parciales de tipos***. La declaración de tipos se construye a partir de sus partes siguiendo las reglas en esta sección, con lo que se trata como una sola declaración durante el resto del procesamiento de tiempo de compilación y tiempo de ejecución del programa.

Un *class_declaration*, *struct_declaration* o *interface_declaration* representa una declaración de tipo parcial si incluye un `partial` modificador. `partial` no es una palabra clave y sólo actúa como un modificador si aparece inmediatamente antes de una de las palabras clave `class`, `struct` o `interface` en una declaración de tipo, o antes del tipo `void` en una declaración de método. En otros contextos, se puede usar como un identificador normal.

Cada parte de una declaración de tipo parcial debe incluir un `partial` modificador. Debe tener el mismo nombre y declararse en el mismo espacio de nombres o en otras partes de la declaración de tipos. El `partial` modificador indica que las partes adicionales de la declaración de tipos pueden existir en otra parte, pero la existencia de tales elementos adicionales no es un requisito; es válida para un tipo con una sola declaración incluir el `partial` modificador.

Todas las partes de un tipo parcial se deben compilar juntos tal que las partes se pueden combinar en tiempo de compilación en una declaración de tipo único. Tipos parciales no permiten ampliar los tipos ya compilados en concreto.

Los tipos anidados se pueden declarar en varias partes mediante el uso de la `partial` modificador. Normalmente, el tipo contenedor se declara mediante `partial` bien y cada parte del tipo anidado que se declara en una parte distinta del tipo contenedor.

El `partial` modificador no está permitido en declaraciones de delegado o enumeración.

### <a name="attributes"></a>Atributos

Los atributos de un tipo parcial se determinan mediante la combinación, en un orden no especificado, los atributos de cada una de las partes. Si se coloca un atributo en varias partes, es equivalente a especificar el atributo varias veces en el tipo. Por ejemplo, las dos partes:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
son equivalentes a una declaración de tales como:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Atributos de tipos de parámetros se combinan de manera similar.

### <a name="modifiers"></a>Modificadores

Cuando una declaración de tipo parcial incluye una especificación de accesibilidad (el `public`, `protected`, `internal`, y `private` modificadores) debe coincidir con todas las otras partes que incluyen una especificación de accesibilidad. Si ninguna parte de un tipo parcial incluye una especificación de accesibilidad, el tipo tiene la accesibilidad predeterminada apropiada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)).

Si uno o más declaraciones parciales de un tipo anidado incluyen un `new` modificador, no se notifica ninguna advertencia si el tipo anidado oculta un miembro heredado ([ocultar mediante herencia](basic-concepts.md#hiding-through-inheritance)).

Si uno o más declaraciones parciales de una clase incluyen un `abstract` modificador, la clase se considera abstracta ([clases abstractas](classes.md#abstract-classes)). En caso contrario, la clase se considera no abstractas.

Si uno o más declaraciones parciales de una clase incluyen un `sealed` modificador, la clase se considera sellado ([clases Sealed](classes.md#sealed-classes)). En caso contrario, se considera la clase no sellada.

Tenga en cuenta que una clase no puede ser abstract y sealed.

Cuando el `unsafe` modificador se usa en una declaración de tipo parcial, solo esa parte concreta se considera un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)).

### <a name="type-parameters-and-constraints"></a>Parámetros de tipo y restricciones

Si un tipo genérico se declara en varias partes, cada parte debe declarar los parámetros de tipo. Cada parte debe tener el mismo número de parámetros de tipo y el mismo nombre para cada parámetro de tipo, en orden.

Cuando una declaración de tipo genérico parcial incluye restricciones (`where` cláusulas), las restricciones deben coincidir con todas las otras partes que incluyen restricciones. En concreto, cada elemento que incluya restricciones debe tener restricciones para el mismo conjunto de parámetros de tipo, y para cada parámetro de tipo de los conjuntos de principal, secundaria y las restricciones de constructor deben ser equivalentes. Dos conjuntos de restricciones son equivalentes si contienen a los mismos miembros. Si ninguna parte de un tipo genérico parcial especifica las restricciones del parámetro, el tipo se consideran parámetros sin restricciones.

El ejemplo
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
es correcto porque aquellas partes que incluyen restricciones (los dos primeros) de forma eficaz especifican el mismo conjunto de principal, secundaria y las restricciones de constructor para el mismo conjunto de parámetros de tipo, respectivamente.

### <a name="base-class"></a>Clase base

Cuando una declaración de clase parcial incluye una especificación de clase base, debe coincidir con todas las otras partes que incluyen una especificación de clase base. Si ninguna parte de una clase parcial incluye una especificación de clase base, se convierte en la clase base `System.Object` ([clases Base](classes.md#base-classes)).

### <a name="base-interfaces"></a>Interfaces base

El conjunto de interfaces base para un tipo declarado en varias partes es la unión de las interfaces base especificadas en cada parte. Una interfaz base concreta solo puede llamarse una vez en cada parte, pero se permite varias partes de un nombre de las mismas interfaces bases. Solo debe haber una implementación de los miembros de cualquier interfaz base determinada.

En el ejemplo
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
el conjunto de interfaces base para la clase `C` es `IA`, `IB`, y `IC`.

Normalmente, cada parte proporciona una implementación de las interfaces declaradas en esa parte; Sin embargo, esto no es un requisito. Una parte puede proporcionar la implementación para una interfaz declarada en una parte distinta:
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a>Miembros

A excepción de los métodos parciales ([métodos parciales](classes.md#partial-methods)), el conjunto de miembros de un tipo declarado en varias partes es simplemente la unión del conjunto de miembros declarados en cada parte. Los cuerpos de todas las partes de la declaración de tipo comparten el mismo espacio de declaración ([declaraciones](basic-concepts.md#declarations)) y el ámbito de cada miembro ([ámbitos](basic-concepts.md#scopes)) se extiende a los cuerpos de todas las partes. El dominio de accesibilidad de cualquier miembro siempre incluye todas las partes del tipo envolvente. un `private` miembro declarado en una parte es libremente accesible desde otra parte. Es un error en tiempo de compilación para declarar el mismo miembro en más de una parte del tipo, a menos que ese miembro es un tipo con el `partial` modificador.

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

El orden de los miembros dentro de un tipo es rara vez significativo al código de C#, pero puede ser importante cuando se interactúa con otros lenguajes y entornos. En estos casos, el orden de los miembros dentro de un tipo declarado en varias partes es indefinido.

### <a name="partial-methods"></a>Métodos parciales

Métodos parciales pueden definirse en una parte de una declaración de tipo y se implementa en otro. La implementación es opcional; Si ninguna parte implementa el método parcial, la declaración de método parcial y todas las llamadas a la se quitan de la declaración del tipo resultante de la combinación de las partes.

Los métodos parciales no se puede definir los modificadores de acceso, pero son implícitamente `private`. Tipo de valor devuelto debe ser `void`, y no pueden tener sus parámetros la `out` modificador. El identificador `partial` se reconoce como palabra clave especial en una declaración de método sólo si aparece justo antes de que el `void` tipo; en caso contrario, se puede usar como un identificador normal. Un método parcial no puede implementar explícitamente los métodos de interfaz.

Hay dos tipos de declaraciones de método parcial: Si el cuerpo de la declaración del método es un punto y coma, la declaración se dice que un ***definir la declaración de método parcial***. Si el cuerpo se expresa como un *bloque*, la declaración se dice que un ***implementar la declaración de método parcial***. Entre las partes de una declaración de tipo puede haber solo una declaración de método parcial con una firma determinada de definición y puede haber solo una implementación de la declaración de método parcial con una firma dada. Si tiene una declaración de método parcial de implementación, una definición de la declaración de método parcial correspondiente debe existir y deben coincidir con las declaraciones como se especifica en la siguiente:

* Las declaraciones deben tener los mismos modificadores (aunque no necesariamente en el mismo orden), nombre del método, el número de parámetros de tipo y el número de parámetros.
* Los parámetros correspondientes en las declaraciones deben tener los mismos modificadores (aunque no necesariamente en el mismo orden) y los mismos tipos (módulo las diferencias en los nombres de parámetro de tipo).
* Parámetros de tipo correspondientes en las declaraciones de deben tener las mismas restricciones (módulo las diferencias en los nombres de parámetro de tipo).

Una declaración de método parcial de implementación puede aparecer en el mismo elemento como la declaración de método parcial de definición correspondiente.

Solo un método parcial definición participa en la resolución de sobrecarga. Por lo tanto, si se proporciona una declaración de implementación, expresiones de invocación pueden resolver a las invocaciones del método parcial. Dado que siempre devuelve un método parcial `void`, estas expresiones de invocación siempre será las instrucciones de expresión. Además, dado que un método parcial es implícitamente `private`, siempre se produzcan tales instrucciones dentro de una de las partes de la declaración de tipo en el que se declara el método parcial.

Si ninguna parte de una declaración de tipo parcial contiene una declaración de implementación para un determinado método parcial, cualquier instrucción de expresión que realiza la llamada simplemente se quita de la declaración de tipos combinados. Por tanto, la expresión de invocación, incluidas las expresiones constituyentes, tiene ningún efecto en tiempo de ejecución. El método parcial propio también se quita y no será un miembro de la declaración de tipos combinados.

Si existe una declaración de implementación de un método parcial dado, se conservan las invocaciones de métodos parciales. El método parcial da lugar a una declaración de método similar a la declaración de implementación de método parcial, excepto los siguientes:

* El `partial` modificador no se incluye
* Los atributos de la declaración del método resultante son los atributos de la definición y la declaración de método parcial implementación combinados en un orden no especificado. No se quitan los duplicados.
* Los atributos de los parámetros de la declaración del método resultante son los atributos combinados de los parámetros correspondientes de la definición y la declaración de método parcial de implementación en un orden no especificado. No se quitan los duplicados.

Si se especifica, pero no una declaración de implementación de una declaración de definición para un método parcial M, se aplican las restricciones siguientes:

* Es un error en tiempo de compilación para crear un delegado al método ([expresiones de creación de delegados](expressions.md#delegate-creation-expressions)).
* Es un error en tiempo de compilación para hacer referencia a `M` dentro de una función anónima que se convierte en un tipo de árbol de expresión ([evaluación de función anónima conversiones a tipos de árbol de expresión](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* Las expresiones que se producen como parte de una invocación de `M` no afectan el estado de asignación definitiva a ([asignación definitiva](variables.md#definite-assignment)), lo que potencialmente puede provocar errores en tiempo de compilación.
* `M` no puede ser el punto de entrada para una aplicación ([inicio de la aplicación](basic-concepts.md#application-startup)).

Los métodos parciales son útiles para permitir que una parte de una declaración de tipo para personalizar el comportamiento de la otra parte, por ejemplo, uno generado por una herramienta. Considere la siguiente declaración de clase parcial:
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

Si esta clase se compila sin cualquier otra parte, se quitarán las declaraciones de método parcial definición y sus invocaciones y la declaración de clase combinado resultante será equivalente a la siguiente:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

Suponga que otra parte se proporciona, sin embargo, que proporciona las declaraciones de implementación de los métodos parciales:
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

A continuación, la declaración de clase combinado resultante será equivalente a la siguiente:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a>Enlace de nombres

Aunque cada parte de un tipo extensible debe declararse dentro del mismo espacio de nombres, las partes se suelen escribir en las declaraciones de espacio de nombres diferente. Por lo tanto, diferentes `using` directivas ([directivas Using](namespaces.md#using-directives)) pueden estar presentes para cada parte. Al interpretar los nombres simples ([inferencia](expressions.md#type-inference)) dentro de una sola parte, el `using` se consideran las directivas de las declaraciones de espacio de nombres envolvente esa parte. Esto puede producir el mismo identificador con diferentes significados en diferentes partes:
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a>Miembros de la clase

Los miembros de una clase se componen de los miembros introducidos por su *class_member_declaration*s y los miembros heredan de la clase base directa.

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

Los miembros de un tipo de clase se dividen en las siguientes categorías:

*  Constantes, que representan valores constantes asociados a la clase ([constantes](classes.md#constants)).
*  Los campos, que son las variables de la clase ([campos](classes.md#fields)).
*  Métodos, que implementan los cálculos y acciones que pueden realizarse mediante la clase ([métodos](classes.md#methods)).
*  Propiedades, que definen características con nombre y las acciones asociadas a la lectura y escritura de esas características ([propiedades](classes.md#properties)).
*  Eventos, que definen las notificaciones que se pueden generar mediante la clase ([eventos](classes.md#events)).
*  Indizadores, que permiten a las instancias de la clase que se debe indexar de la misma manera (sintácticamente) que las matrices ([indizadores](classes.md#indexers)).
*  Operadores, que definen los operadores de expresión que se pueden aplicar a las instancias de la clase ([operadores](classes.md#operators)).
*  Constructores de instancias, que implementan las acciones necesarias para inicializar instancias de la clase ([constructores de instancias](classes.md#instance-constructors))
*  Los destructores, que implementan las acciones antes de instancias de la clase se descarten de forma permanente ([destructores](classes.md#destructors)).
*  Constructores estáticos, que implementan las acciones necesarias para inicializar la propia clase ([constructores estáticos](classes.md#static-constructors)).
*  Tipos que representan los tipos que son locales a la clase ([tipos anidados](classes.md#nested-types)).

Los miembros que pueden contener código ejecutable se conocen colectivamente como la *miembros de función* del tipo de clase. Los miembros de función de un tipo de clase son los métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos de ese tipo de clase.

Un *class_declaration* crea un nuevo espacio de declaración ([declaraciones](basic-concepts.md#declarations)) y el *class_member_declaration*s contenidas inmediatamente en la *clase _declaration* introducir nuevos miembros en este espacio de declaración. Las siguientes reglas se aplican a *class_member_declaration*s:

*  Constructores de instancias, destructores y constructores estáticos deben tener el mismo nombre que la clase inmediatamente envolvente. Todos los demás miembros deben tener nombres que difieren del nombre de la clase inmediatamente envolvente.
*  El nombre de constante, campo, propiedad, evento o tipo debe ser diferente de los nombres de todos los demás miembros declarados en la misma clase.
*  El nombre de un método debe ser diferente de los nombres de todos los demás de que no son métodos declarados en la misma clase. Además, la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un método debe ser diferentes de las firmas de todos los demás métodos declarados en la misma clase, y dos métodos declarados en la misma clase no pueden tener signaturas que se diferencien únicamente por `ref` y `out`.
*  La firma de un constructor de instancia debe ser diferentes de las firmas de todos los demás constructores de instancia declarados en la misma clase, y dos constructores declarados en la misma clase no pueden tener firmas que se diferencien únicamente por `ref` y `out`.
*  La firma de un indizador debe ser diferente de las firmas de todos los demás indexadores declarados en la misma clase.
*  La firma de un operador debe ser diferente de las firmas de todos los demás operadores declarados en la misma clase.

Los miembros heredados de un tipo de clase ([herencia](classes.md#inheritance)) no forman parte del espacio de declaración de una clase. Por lo tanto, una clase derivada puede declarar a un miembro con el mismo nombre o signatura que un miembro heredado (que en realidad oculta al miembro heredado).

### <a name="the-instance-type"></a>El tipo de instancia

Cada declaración de clase tiene un tipo enlace asociado ([dependientes e independientes tipos](types.md#bound-and-unbound-types)), el ***tipo de instancia***. Para una declaración de clase genérica, el tipo de instancia se forma mediante la creación de un tipo construido ([construido tipos](types.md#constructed-types)) de la declaración de tipo, con cada uno del tipo suministrado argumentos es el correspondiente parámetro de tipo. Puesto que el tipo de instancia utiliza los parámetros de tipo, que puede usarse únicamente donde los parámetros de tipo están en ámbito; es decir, dentro de la declaración de clase. El tipo de instancia es el tipo de `this` para el código escrito dentro de la declaración de clase. Para las clases no genéricas, el tipo de instancia es simplemente la clase declarada. A continuación muestra varias declaraciones de clase junto con sus tipos de instancia: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Miembros de tipos construidos

Los miembros no heredados de un tipo construido se obtienen sustituyendo, para cada *tipo_parámetro* en la declaración de miembro correspondiente *type_argument* del tipo construido. El proceso de sustitución se basa en el significado semántico de declaraciones de tipos y no es una sustitución simplemente textual.

Por ejemplo, dada la declaración de clase genérica
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
el tipo construido `Gen<int[],IComparable<string>>` tiene los siguientes miembros:
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

El tipo del miembro `a` en la declaración de clase genérica `Gen` es "matriz bidimensional de `T`", por lo que el tipo del miembro `a` en el tipo construido anterior es "matriz bidimensional de una matriz unidimensional de `int`", o `int[,][]`.

Dentro de los miembros de la función de instancia, el tipo de `this` es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la declaración del contenedor.

Todos los miembros de una clase genérica pueden usar parámetros de tipo de cualquier clase envolvente, ya sea directamente o como parte de un tipo construido. Cuando se cierra un determinado tipo construido ([abierto y cerrado tipos](types.md#open-and-closed-types)) se usa en tiempo de ejecución, cada uso de un parámetro de tipo es reemplazado por el argumento de tipo real proporcionado para el tipo construido. Por ejemplo:
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a>Herencia

Una clase ***hereda*** los miembros de su tipo de clase base directa. La herencia significa que una clase contiene implícitamente todos los miembros de su tipo de clase base directa, excepto los constructores de instancias, destructores y constructores estáticos de la clase base. Algunos aspectos importantes de herencia son:

*  La herencia es transitiva. Si `C` se deriva de `B`, y `B` se deriva de `A`, a continuación, `C` hereda los miembros declarados en `B` , así como los miembros declarados en `A`.
*  Una clase derivada extiende su clase base directa. Una clase derivada puede agregar nuevos miembros a aquellos de los que hereda, pero no puede quitar la definición de un miembro heredado.
*  Constructores de instancias, destructores y constructores estáticos no se heredan, pero todos los demás miembros, independientemente de su accesibilidad declarada ([acceso a miembros](basic-concepts.md#member-access)). Sin embargo, dependiendo de su accesibilidad declarada, es posible que los miembros heredados no esté accesibles en una clase derivada.
*  Una clase derivada puede ***ocultar*** ([ocultar mediante herencia](basic-concepts.md#hiding-through-inheritance)) los miembros heredados mediante la declaración de nuevos miembros con el mismo nombre o signatura. Tenga en cuenta sin embargo que ocultar un miembro heredado no elimina dicho miembro, simplemente hace que el miembro accesible directamente a través de la clase derivada.
*  Una instancia de una clase contiene un conjunto de todos los campos de instancia declarados en la clase y sus clases base y una conversión implícita ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) existe desde un tipo de clase derivada a cualquiera de sus tipos de clase base. Por lo tanto, una referencia a una instancia de alguna clase derivada puede tratarse como una referencia a una instancia de cualquiera de sus clases base.
*  Una clase puede declarar indizadores, propiedades y métodos virtuales y las clases derivadas pueden invalidar la implementación de estos miembros de función. Esto permite que las clases muestren un comportamiento polimórfico ya que las acciones realizadas por una llamada a función miembro varían según el tipo de tiempo de ejecución de la instancia a través del cual se invoca ese miembro de función.

El miembro heredado de un tipo de clase construida son los miembros del tipo de clase base inmediata ([clases Base](classes.md#base-classes)), que se encuentra, sustituyendo los argumentos de tipo para cada repetición del tipo correspondiente del tipo construido los parámetros en el *class_base* especificación. Estos miembros, se transforman a su vez, sustituyendo, para cada *tipo_parámetro* en la declaración de miembro correspondiente *type_argument* de la *class_base* especificación.

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

En el ejemplo anterior, el tipo construido `D<int>` tiene un miembro no heredado `public int G(string s)` obtiene sustituyendo el argumento de tipo `int` para el parámetro de tipo `T`. `D<int>` También tiene un miembro heredado de la declaración de clase `B`. Este miembro heredado se determina, determine primero el tipo de clase base `B<int[]>` de `D<int>` sustituyendo `int` para `T` en la especificación de la clase base `B<T[]>`. A continuación, como un argumento de tipo `B`, `int[]` se sustituye por `U` en `public U F(long index)`, produciendo el miembro heredado `public int[] F(long index)`.

### <a name="the-new-modifier"></a>El nuevo modificador

Un *class_member_declaration* se pueden declarar un miembro con el mismo nombre o signatura que un miembro heredado. Cuando esto ocurre, se dice que el miembro de clase derivada a ***ocultar*** el miembro de clase base. Ocultar a un miembro heredado no se considera un error, pero hace que el compilador emita una advertencia. Para suprimir la advertencia, la declaración del miembro de clase derivada puede incluir un `new` modificador para indicar que el miembro derivado está diseñado para ocultar el miembro base. En este tema se describe con más detalle en [ocultar mediante herencia](basic-concepts.md#hiding-through-inheritance).

Si un `new` modificador se incluye en una declaración que no oculta un miembro heredado, se emite una advertencia a tal efecto. Se puede suprimir esta advertencia mediante la eliminación de la `new` modificador.

### <a name="access-modifiers"></a>Modificadores de acceso

Un *class_member_declaration* puede tener uno de los cinco posibles tipos de accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , o `private`. Excepto para el `protected internal` combinación, es un error en tiempo de compilación para especificar más de un modificador de acceso. Cuando un *class_member_declaration* no incluye ningún modificador de acceso, `private` se da por hecho.

### <a name="constituent-types"></a>Tipos constituyentes

Tipos que se usan en la declaración de un miembro se denominan tipos constituyentes de ese miembro. Los posibles tipos constituyentes son el tipo de una constante, campo, propiedad, indizador o evento, el tipo de valor devuelto de un método u operador y los tipos de parámetro de un método, indizador, operador o constructor de instancia. Los tipos constituyentes de un miembro deben ser al menos tan accesibles como el propio miembro ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Miembros estáticos y de instancia

Son miembros de una clase ***miembros estáticos*** o ***miembros de instancia***. Por lo general, resulta útil pensar en los miembros estáticos pertenecen a tipos de clases y miembros de instancia como que pertenecen a objetos (instancias de tipos de clase).

Cuando una declaración de campo, método, propiedad, evento, operador o constructor incluye un `static` modificador, declara un miembro estático. Además, una declaración de constante o tipo declara implícitamente un miembro estático. Los miembros estáticos tienen las siguientes características:

*  Cuando un miembro estático `M` se hace referencia en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`, `E` debe denotar un tipo que contenga `M`. Es un error en tiempo de compilación para `E` para denotar una instancia.
*  Un campo estático identifica exactamente una ubicación de almacenamiento para ser compartidos por todas las instancias de un tipo determinado de clases cerrado. Independientemente del número de instancias de un tipo de clase cerrado determinado se crea, hay solo una copia de un campo estático.
*  Un miembro de función estático (método, propiedad, evento, operador o constructor) no funciona en una instancia concreta, y es un error en tiempo de compilación para hacer referencia a `this` en dicho miembro de función.

Cuando una declaración de campo, método, propiedad, evento, indizador, constructor o destructor no incluye un `static` modificador, declara un miembro de instancia. (Un miembro de instancia a veces se denomina a un miembro no estático.) Los miembros de instancia tienen las siguientes características:

*  Cuando un miembro de instancia `M` se hace referencia en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`, `E` debe denotar una instancia de un tipo que contiene `M`. Es un error en tiempo de enlace para `E` para denotar un tipo.
*  Todas las instancias de una clase contiene un conjunto independiente de todos los campos de instancia de la clase.
*  Un miembro de función de instancia (método, propiedad, indizador, constructor de instancia o destructor) funciona en una instancia determinada de la clase, y puede tener acceso a esta instancia como `this` ([este acceso](expressions.md#this-access)).

El ejemplo siguiente muestra las reglas para tener acceso a estáticas y miembros de instancia:
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

El `F` método que muestra en un miembro de función de instancia, un *simple_name* ([nombres simples](expressions.md#simple-names)) puede utilizarse para tener acceso a los miembros de instancia y miembros estáticos. El `G` método muestra que en un miembro de función estático, se produce un error en tiempo de compilación para tener acceso a un miembro de instancia a través de un *simple_name*. El `Main` método muestra que en un *member_access* ([acceso a miembros](expressions.md#member-access)), deben tener acceso a los miembros de instancia a través de instancias, y deben tener acceso a los miembros estáticos a través de tipos.

### <a name="nested-types"></a>Tipos anidados

Se llama a un tipo declarado dentro de una declaración de clase o struct un ***tipo anidado***. Un tipo que se declara dentro de una unidad de compilación o espacio de nombres se denomina un ***tipo no anidado***.

En el ejemplo
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
clase `B` es un tipo anidado porque se declara dentro de la clase `A`y la clase `A` es un tipo no anidado porque se declara dentro de una unidad de compilación.

#### <a name="fully-qualified-name"></a>Nombre completo

El nombre completo ([nombres completos](basic-concepts.md#fully-qualified-names)) para un tipo anidado es `S.N` donde `S` es el nombre completo del tipo en el tipo `N` se declara.

#### <a name="declared-accessibility"></a>Accesibilidad declarada

Los tipos no anidados pueden tener `public` o `internal` accesibilidad declarada y tener `internal` accesibilidad declarada de forma predeterminada. Los tipos anidados pueden tener estas formas de accesibilidad declarada demasiado, además de uno o más formas adicionales de accesibilidad declarada, dependiendo de si el tipo contenedor es una clase o struct:

*  Un tipo anidado que se declara en una clase puede tener cualquiera de las cinco formas de accesibilidad declarada (`public`, `protected internal`, `protected`, `internal`, o `private`) y, al igual que otros miembros de clase, el valor predeterminado es `private` declarado accesibilidad.
*  Un tipo anidado que se declara en un struct puede tener cualquiera de las tres formas de accesibilidad declarada (`public`, `internal`, o `private`) y, al igual que otros miembros de struct, el valor predeterminado es `private` accesibilidad declarada.

El ejemplo
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
declara una clase anidada privada `Node`.

#### <a name="hiding"></a>Ocultar

Puede ocultar un tipo anidado ([ocultación de nombres](basic-concepts.md#name-hiding)) un miembro base. El `new` modificador está permitido en declaraciones de tipo anidado para que se puede expresar explícitamente si se oculta. El ejemplo
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
muestra una clase anidada `M` que oculta el método `M` definido en `Base`.

#### <a name="this-access"></a>Este acceso

Un tipo anidado y su tipo contenedor no tiene una relación especial con respecto a *this_access* ([este acceso](expressions.md#this-access)). En concreto, `this` dentro de un tipo anidado no puede usarse para hacer referencia a los miembros de instancia del tipo contenedor. En casos donde un tipo anidado tiene acceso a los miembros de instancia de su tipo contenedor, se puede proporcionar acceso proporcionando el `this` para la instancia del tipo contenedor como un argumento de constructor para el tipo anidado. El ejemplo siguiente
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
se muestra esta técnica. Una instancia de `C` crea una instancia de `Nested` y pasa su propio `this` a `Nested`del constructor con el fin de proporcionar acceso posterior a `C`de miembros de instancia.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Acceso a los miembros privados y protegidos del tipo contenedor

Un tipo anidado tiene acceso a todos los miembros que son accesibles para su tipo contenedor, incluidos los miembros del tipo contenedor que tienen `private` y `protected` accesibilidad declarada. El ejemplo
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
se muestra una clase `C` que contiene una clase anidada `Nested`. Dentro de `Nested`, el método `G` llama al método estático `F` definido en `C`, y `F` declarado privado accesibilidad.

Un tipo anidado también puede tener acceso a miembros protegidos que se definen en un tipo base de su tipo contenedor. En el ejemplo
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
la clase anidada `Derived.Nested` tiene acceso al método protegido `F` definido en `Derived`de la clase base `Base`, por una llamada a través de una instancia de `Derived`.

#### <a name="nested-types-in-generic-classes"></a>Tipos anidados en clases genéricas

Una declaración de clase genérica puede contener declaraciones de tipo anidado. Los parámetros de tipo de la clase envolvente pueden utilizarse dentro de los tipos anidados. Una declaración de tipo anidado puede contener parámetros de tipo adicionales que se aplican solo al tipo anidado.

Cada declaración de tipos contenido dentro de una declaración de clase genérica es implícitamente una declaración de tipo genérico. Cuando escribe una referencia a un tipo anidado dentro de un tipo genérico, el tipo construido contenedor, incluidos sus argumentos de tipo, debe tener nombres. Sin embargo, desde dentro de la clase externa, el tipo anidado puede utilizar sin calificación; el tipo de instancia de la clase externa puede utilizarse de forma implícita al construir el tipo anidado. El ejemplo siguiente muestra tres maneras diferentes para hacer referencia a un tipo construido creado a partir de `Inner`; los dos primeros son equivalentes:
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

Aunque es recomendable hacerla programación estilo, un parámetro de tipo en un tipo anidado puede ocultar a un miembro o escribir parámetro declarado en el tipo externo:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Nombres de miembro reservado

Para facilitar la subyacente implementación C# tiempo de ejecución, para cada declaración de miembro de origen es una propiedad, evento o indizador, la implementación debe reservar dos firmas de método según el tipo de la declaración de miembro, su nombre y su tipo. Es un error en tiempo de compilación para un programa declarar un miembro cuya firma coincida con una de estas firmas reservadas, incluso si la implementación subyacente de tiempo de ejecución no hace uso de estas reservas.

Los nombres reservados no presentan declaraciones, por lo tanto no participan en la búsqueda de miembros. Sin embargo, una declaración asociada al método reservado firmas participan en la herencia ([herencia](classes.md#inheritance)) y puede estar oculto con el `new` modificador ([el nuevo modificador](classes.md#the-new-modifier)).

La reserva de estos nombres desempeña tres funciones:

*  Para permitir la implementación subyacente utilizar un identificador normal como un nombre de método para obtener o establecer el acceso a la característica del lenguaje C#.
*  Para permitir que otros lenguajes interoperar con un identificador normal como un nombre de método para obtener o establecer el acceso a la característica del lenguaje C#.
*  Para ayudar a garantizar que el origen aceptado por un compilador adecuado es aceptado por el otro, mediante la realización de las particularidades del miembro reservado nombres coherentes en todas las implementaciones de C#.

La declaración de un destructor ([destructores](classes.md#destructors)) también hace que una firma reservar ([nombres de miembros reservados para los destructores](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Nombres de miembro reservados para las propiedades

Para una propiedad `P` ([propiedades](classes.md#properties)) de tipo `T`, las firmas siguientes están reservadas:

```csharp
T get_P();
void set_P(T value);
```

Tanto las firmas están reservadas, incluso si la propiedad es de solo lectura o solo escritura.

En el ejemplo
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
una clase `A` define una propiedad de solo lectura `P`, reservando así las firmas para `get_P` y `set_P` métodos. Una clase `B` deriva `A` y oculta ambas de estas firmas reservadas. El ejemplo genera el resultado:
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Nombres de miembros reservados para los eventos

Para un evento `E` ([eventos](classes.md#events)) del tipo de delegado `T`, las firmas siguientes están reservadas:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Nombres de miembros reservados para los indizadores

Para un indizador ([indizadores](classes.md#indexers)) de tipo `T` con lista de parámetros `L`, las firmas siguientes están reservadas:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Tanto las firmas están reservadas, incluso si el indizador es de solo lectura o solo escritura.

Además del nombre de miembro `Item` está reservado.

#### <a name="member-names-reserved-for-destructors"></a>Nombres de miembros reservados para los destructores

Para una clase que contiene un destructor ([destructores](classes.md#destructors)), se reserva la firma siguiente:
```csharp
void Finalize();
```

## <a name="constants"></a>Constantes

Un ***constante*** es un miembro de clase que representa un valor constante: un valor que se puede calcular en tiempo de compilación. Un *constant_declaration* presenta una o varias constantes de un tipo determinado.

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

Un *constant_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), un `new` modificador ([el nuevo modificador](classes.md#the-new-modifier)), y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)). Los atributos y modificadores se aplican a todos los miembros declarados por el *constant_declaration*. Aunque las constantes se consideran miembros estáticos, un *constant_declaration* no requiere ni permite un `static` modificador. Es un error para el mismo modificador aparezca varias veces en una declaración de constante.

El *tipo* de un *constant_declaration* especifica el tipo de los miembros introducidos por la declaración. El tipo es seguido por una lista de *constant_declarator*s, cada uno de los cuales incluye un nuevo miembro. Un *constant_declarator* consta de un *identificador* que designa el miembro, seguido de un "`=`" símbolo (token), seguido de un *constant_expression* ([ Expresiones constantes](expressions.md#constant-expressions)) que proporciona el valor del miembro.

El *tipo* especificado en una constante debe ser declaración `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, un *enum_type*, o un *reference_type*. Cada *constant_expression* debe dar un valor del tipo de destino o de un tipo que pueda convertirse al tipo de destino mediante una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).

El *tipo* de una constante debe ser al menos tan accesibles como la propia constante ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

Se obtiene el valor de una constante en una expresión que utiliza un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)).

Una constante puede participar en una *constant_expression*. Por lo tanto, se puede utilizar una constante en cualquier constructor que requiere un *constant_expression*. Ejemplos de tales construcciones `case` etiquetas, `goto case` instrucciones, `enum` las declaraciones de miembros, atributos y otras declaraciones de constantes.

Como se describe en [expresiones constantes](expressions.md#constant-expressions), un *constant_expression* es una expresión que se puede evaluar por completo en tiempo de compilación. La única forma para crear un valor distinto de null de un *reference_type* distinto `string` consiste en aplicar el `new` operador y, puesto que el `new` operador no está permitido en un *constant_ expresión*, el único valor posible para las constantes de *reference_type*s distinto `string` es `null`.

Cuando se desea un nombre simbólico para un valor constante, pero cuando el tipo de ese valor no se permite en una declaración de constante o cuando no se puede calcular el valor en tiempo de compilación por una *constant_expression*, un `readonly` campo () [Campos de solo lectura](classes.md#readonly-fields)) se puede usar en su lugar.

Una declaración de constante que declara varias constantes equivale a varias declaraciones de una sola constante con los mismos atributos, los modificadores y tipo. Por ejemplo
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
es equivalente a
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

Las constantes pueden depender de otras constantes dentro del mismo programa siempre y cuando las dependencias no son de naturaleza circular. El compilador organiza automáticamente evaluar las declaraciones de constante en el orden adecuado. En el ejemplo
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
el compilador evalúa primero `A.Y`, a continuación, se evalúa como `B.Z`y, por último, se evalúa como `A.X`, produciendo los valores `10`, `11`, y `12`. Las declaraciones de constantes pueden depender de constantes de otros programas, pero dichas dependencias sólo son posibles en una dirección. Que hace referencia en el ejemplo anterior, si `A` y `B` han sido declarados en programas independientes, sería posible para `A.X` depender `B.Z`, pero `B.Z` , a continuación, podrían depender no de forma simultánea en `A.Y`.

## <a name="fields"></a>Campos

Un ***campo*** es un miembro que representa una variable asociada con un objeto o clase. Un *field_declaration* presenta uno o varios campos de un tipo determinado.

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

Un *field_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), un `new` modificador ([el nuevo modificador](classes.md#the-new-modifier)), un una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)) y un `static` modificador ([campos estáticos y de instancia](classes.md#static-and-instance-fields)). Además, un *field_declaration* puede incluir un `readonly` modificador ([campos de solo lectura](classes.md#readonly-fields)) o un `volatile` modificador ([campos volátiles](classes.md#volatile-fields)), pero no ambos. Los atributos y modificadores se aplican a todos los miembros declarados por el *field_declaration*. Es un error para el mismo modificador aparezca varias veces en una declaración de campo.

El *tipo* de un *field_declaration* especifica el tipo de los miembros introducidos por la declaración. El tipo es seguido por una lista de *variable_declarator*s, cada uno de los cuales incluye un nuevo miembro. Un *variable_declarator* consta de un *identificador* que los nombres de ese miembro, seguido opcionalmente por una "`=`" token y un *variable_initializer* ([ Inicializadores de variables](classes.md#variable-initializers)) que proporciona el valor inicial de ese miembro.

El *tipo* de un campo debe ser al menos tan accesibles como el propio campo ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

Se obtiene el valor de un campo en una expresión que utiliza un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)). El valor de un campo que no son de sólo lectura se modifica mediante una *asignación* ([operadores de asignación](expressions.md#assignment-operators)). El valor de un campo que no son de sólo lectura puede ser obtener y modificar utilizando postfijo de incremento y decremento operadores ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)) y el incremento de prefijo y operadores de decremento ([prefijo operadores de incremento y decremento](expressions.md#prefix-increment-and-decrement-operators)).

Una declaración de campo que declara varios campos equivale a varias declaraciones de campos solo con los mismos atributos, los modificadores y tipo. Por ejemplo
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
es equivalente a
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a>Campos estáticos y de instancia

Cuando una declaración de campo incluye un `static` son los campos introducidos por la declaración de modificador, ***campos estáticos***. Cuando no hay ninguna `static` modificador está presente, los campos introducidos por la declaración son ***campos de instancia***. Los campos estáticos y los campos de instancia son dos de los distintos tipos de variables ([Variables](variables.md)) compatible con C# y, a veces se conocen como ***variables estáticas*** y ***las variables de instancia*** , respectivamente.

Un campo estático no forma parte de una instancia concreta; en su lugar, se comparte entre todas las instancias de un tipo cerrado ([abierto y cerrado tipos](types.md#open-and-closed-types)). Independientemente del número de instancias de un tipo de clase cerrado se crea, hay solo una copia de un campo estático para el dominio de aplicación asociada.

Por ejemplo:
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

Un campo de instancia pertenece a una instancia. En concreto, todas las instancias de una clase contiene un conjunto independiente de todos los campos de instancia de esa clase.

Cuando se hace referencia a un campo en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`si `M` es un campo estático, `E` debe denotar un tipo que contenga `M` y si `M` es un campo de instancia, E debe denotar una instancia de un tipo que contenga `M`.

Las diferencias entre estático y los miembros de instancia se tratan con más detalle en [miembros estáticos y de instancia](classes.md#static-and-instance-members).

### <a name="readonly-fields"></a>Campos de solo lectura

Cuando un *field_declaration* incluye un `readonly` son los campos introducidos por la declaración de modificador, ***campos de solo lectura***. Las asignaciones directas a campos de sólo lectura sólo pueden producirse como parte de la declaración o en un constructor de instancia o un constructor estático de la misma clase. (Un campo de solo lectura puede asignarse a varias veces en estos contextos.) En concreto, las asignaciones directas a una `readonly` campo solo se permiten en los contextos siguientes:

*  En el *variable_declarator* que presenta el campo (incluyendo un *variable_initializer* en la declaración).
*  Para un campo de instancia, en los constructores de instancia de la clase que contiene la declaración de campo; para un campo estático, en el constructor estático de la clase que contiene la declaración de campo. Estos también son los únicos contextos en los que es válido pasar un `readonly` campo como un `out` o `ref` parámetro.

Intenta asignar a un `readonly` campo o pasarla como un `out` o `ref` parámetro en cualquier otro contexto es un error en tiempo de compilación.

#### <a name="using-static-readonly-fields-for-constants"></a>Uso de campos de solo lectura estáticos para constantes

Un `static readonly` campo resulta útil cuando se desea un nombre simbólico para un valor constante, pero cuando el tipo del valor no se permite en un `const` declaración, o cuando no se puede calcular el valor en tiempo de compilación. En el ejemplo
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
el `Black`, `White`, `Red`, `Green`, y `Blue` miembros no pueden declararse como `const` miembros porque sus valores no se puede calcular en tiempo de compilación. Sin embargo, declararlas `static readonly` produce el mismo efecto.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Control de versiones de constantes y campos de solo lectura estático

Constantes y campos de solo lectura tienen semántica de control de versiones binarias diferentes. Cuando una expresión hace referencia a una constante, se obtiene el valor de la constante en tiempo de compilación, pero cuando una expresión hace referencia a un campo de solo lectura, no se obtiene el valor del campo hasta el tiempo de ejecución. Considere la posibilidad de una aplicación que consta de dos programas independientes:
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

El `Program1` y `Program2` espacios de nombres indican dos programas que están compilados por separado. Dado que `Program1.Utils.X` se declara como un campo estático de solo lectura, el resultado de la `Console.WriteLine` instrucción no se conoce en tiempo de compilación, sino que se obtiene en tiempo de ejecución. Por lo tanto, si el valor de `X` se cambia y `Program1` se vuelve a compilar, el `Console.WriteLine` instrucción dará como resultado el nuevo valor incluso si `Program2` no se vuelven a compilar. Sin embargo, tenía `X` sido una constante, el valor de `X` habría obtenido en el momento `Program2` se compiló y no se verán afectadas por cambios en `Program1` hasta `Program2` se vuelve a compilar.

### <a name="volatile-fields"></a>Campos volátiles

Cuando un *field_declaration* incluye un `volatile` son los campos que aparecen en la declaración de modificador, ***campos volátiles***.

Para los campos volátiles, técnicas de optimización que reordenan las instrucciones pueden provocar resultados inesperados e impredecibles en programas multiproceso que tienen acceso a los campos sin sincronización como la proporcionada por el *lock_statement*  ([La instrucción lock](statements.md#the-lock-statement)). Estas optimizaciones pueden ser realizadas por el compilador, el sistema de tiempo de ejecución o el hardware. Para los campos volátiles, dichas optimizaciones de reordenación están restringidos:

*  Se llama a una lectura de un campo volátil un ***lectura volátil***. Una lectura volátil tiene "semántica de adquisición"; es decir, se garantiza que se produzca antes de cualquier referencia a la memoria que se produce después de él en la secuencia de instrucciones.
*  Se llama a una operación de escritura de un campo volátil un ***escritura volátil***. Una escritura volátil tiene "semántica de liberación"; es decir, se garantiza que producirse después de todas las referencias de memoria antes de la instrucción de escritura en la secuencia de instrucciones.

Estas restricciones aseguran que todos los subprocesos observarán las operaciones de escritura volátiles realizadas por cualquier otro subproceso en el orden en que se realizaron. Una implementación compatible no es necesario para proporcionar una ordenación total única de las escrituras volátiles como se muestra en todos los subprocesos de ejecución. El tipo de un campo volátil debe ser uno de los siguientes:

*  Un *reference_type*.
*  El tipo `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, o `System.UIntPtr`.
*  Un *enum_type* con un tipo de base de enumeración de `byte`, `sbyte`, `short`, `ushort`, `int`, o `uint`.

El ejemplo
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
genera el resultado:
```
result = 143
```

En este ejemplo, el método `Main` inicia un nuevo subproceso que ejecuta el método `Thread2`. Este método almacena un valor en un campo no volátil denominado `result`, a continuación, almacena `true` en el campo volátil `finished`. El subproceso principal espera para el campo `finished` para establecerse en `true`, a continuación, lee el campo `result`. Puesto que `finished` se ha declarado `volatile`, el subproceso principal debe leer el valor `143` desde el campo `result`. Si el campo `finished` no se declararon `volatile`, estaría permitida para el almacén de `result` sea visible para el subproceso principal después de la tienda a `finished`y por lo tanto, para el subproceso principal leer el valor `0` desde el campo `result`. Declarar `finished` como un `volatile` campo impide tales incoherencias.

### <a name="field-initialization"></a>Inicialización de campos

El valor inicial de un campo, ya sea en un campo estático o un campo de instancia, es el valor predeterminado ([los valores predeterminados](variables.md#default-values)) del tipo del campo. No es posible observar el valor de un campo antes de que se ha producido esta inicialización predeterminada y un campo, por tanto, nunca es "no inicializado". El ejemplo
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
genera el resultado
```
b = False, i = 0
```
Dado que `b` y `i` se inicializan automáticamente en valores predeterminados.

### <a name="variable-initializers"></a>Inicializadores de variables

Pueden incluir declaraciones de campo *variable_initializer*s. Para los campos estáticos, los inicializadores de variables corresponden a instrucciones de asignación que se ejecutan durante la inicialización de clase. Por ejemplo, campos, los inicializadores de variables corresponden a instrucciones de asignación que se ejecutan cuando se crea una instancia de la clase.

El ejemplo
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
genera el resultado
```
x = 1.4142135623731, i = 100, s = Hello
```
Dado que una asignación a `x` se produce cuando se ejecutan los inicializadores de campo estáticos y las asignaciones a `i` y `s` se producen al ejecutan los inicializadores de campo de instancia.

La inicialización de un valor predeterminado se describe en [inicialización de campos](classes.md#field-initialization) se produce para todos los campos, incluidos los campos que tienen los inicializadores de variables. Por lo tanto, cuando se inicializa una clase, todos los campos estáticos en esa clase primero se inicializan a sus valores predeterminados y, a continuación, los inicializadores de campo estático se ejecutan en orden textual. Del mismo modo, cuando se crea una instancia de una clase, todos los campos de instancia de dicha instancia se inicializan por primera vez en sus valores predeterminados y, a continuación, los inicializadores de campo de instancia se ejecutan en orden textual.

Es posible que los campos estáticos con inicializadores de variables que se deben observar en su estado de valor predeterminado. Sin embargo, esto no se recomienda como una cuestión de estilo. El ejemplo
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
muestra este comportamiento. A pesar de las definiciones circulares de un y b, el programa es válido. Genera la salida
```
a = 1, b = 2
```
Dado que los campos estáticos `a` y `b` se inicializan en `0` (el valor predeterminado de `int`) antes de que se ejecutan sus inicializadores. Cuando el inicializador para `a` se ejecuta, el valor de `b` es cero de modo que `a` se inicializa en `1`. Cuando el inicializador para `b` se ejecuta, el valor de `a` ya está `1`de modo que `b` se inicializa en `2`.

#### <a name="static-field-initialization"></a>Inicialización de campos estáticos

Los inicializadores de variable de campo estático de una clase corresponden a una secuencia de asignaciones que se ejecutan en el orden textual en que aparecen en la declaración de clase. Si un constructor estático ([constructores estáticos](classes.md#static-constructors)) existe en la clase, la ejecución de los inicializadores de campo estático, se produce inmediatamente antes de ejecutar ese constructor estático. En caso contrario, los inicializadores de campo estático se ejecutan en el momento antes del primer uso de un campo estático de esa clase dependen de la implementación. El ejemplo
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
podría producir el resultado:
```
Init A
Init B
1 1
```
o la salida:
```
Init B
Init A
1 1
```
Dado que la ejecución de `X`del inicializador y `Y`del inicializador podría producir en cualquier orden; sólo están restringidos que se produzca antes de las referencias a esos campos. Sin embargo, en el ejemplo:
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
el resultado debe ser:
```
Init B
Init A
1 1
```
Dado que las reglas de ejecución de los constructores estáticos (tal como se define en [constructores estáticos](classes.md#static-constructors)) que proporcionan `B`del constructor estático (y por lo tanto, `B`del inicializadores de campo estático) debe ejecutar antes de `A`del constructor estático y los inicializadores de campo.

#### <a name="instance-field-initialization"></a>Inicialización de campos de instancia

Los inicializadores de variable de un campo de instancia de una clase se corresponden con una secuencia de asignaciones que se ejecutan inmediatamente después de la entrada a cualquiera de los constructores de instancias ([inicializadores de Constructor](classes.md#constructor-initializers)) de esa clase. Los inicializadores de variable se ejecutan en el orden textual en que aparecen en la declaración de clase. El proceso de creación e inicialización de la instancia de clase se describe más detalladamente en [constructores de instancias](classes.md#instance-constructors).

Un inicializador de variable para un campo de instancia no puede hacer referencia a la instancia que se está creando. Por lo tanto, es un error en tiempo de compilación para hacer referencia a `this` en un inicializador de variable, tal como se produce un error de tiempo de compilación de un inicializador de variable hacer referencia a cualquier miembro de instancia a través de un *simple_name*. En el ejemplo
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
el inicializador de variable para `y` da como resultado un error en tiempo de compilación porque hace referencia a un miembro de la instancia que se va a crear.

## <a name="methods"></a>Métodos

Un ***método*** es un miembro que implementa un cálculo o una acción que puede realizar un objeto o una clase. Los métodos se declaran mediante *method_declaration*s:

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

Un *method_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso ](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` ([Invalidar métodos](classes.md#override-methods)), `sealed` ([sellado métodos](classes.md#sealed-methods)), `abstract` ([métodos abstractos](classes.md#abstract-methods)), y `extern` ([Métodos externos](classes.md#external-methods)) modificadores.

Una declaración tiene una combinación válida de modificadores si se cumplen todos los elementos siguientes:

*  La declaración incluye una combinación válida de modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)).
*  La declaración no incluye el mismo modificador varias veces.
*  La declaración incluye al menos uno de los siguientes modificadores: `static`, `virtual`, y `override`.
*  La declaración incluye al menos uno de los siguientes modificadores: `new` y `override`.
*  Si la declaración incluye el `abstract` modificador, a continuación, en la declaración no incluye ninguno de los siguientes modificadores: `static`, `virtual`, `sealed` o `extern`.
*  Si la declaración incluye el `private` modificador, a continuación, en la declaración no incluye ninguno de los siguientes modificadores: `virtual`, `override`, o `abstract`.
*  Si la declaración incluye el `sealed` modificador, a continuación, en la declaración también incluye el `override` modificador.
*  Si la declaración incluye el `partial` modificador, a continuación, que no incluye ninguno de los siguientes modificadores: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, o `extern`.

Un método que tiene el `async` modificador es una función asincrónica y sigue las reglas descritas en [funciones asincrónicas](classes.md#async-functions).

El *return_type* declaración especifica el tipo del valor calculado y devuelto por el método de un método. El *return_type* es `void` si el método no devuelve un valor. Si la declaración incluye el `partial` modificador, el tipo de valor devuelto debe ser `void`.

El *member_name* especifica el nombre del método. A menos que el método es una implementación de miembro de interfaz explícita ([implementaciones de miembros de interfaz explícita](interfaces.md#explicit-interface-member-implementations)), el *member_name* es simplemente un *identificador*. Para obtener una implementación de miembro de interfaz explícita, el *member_name* consta de un *interface_type* seguido de un "`.`" y un *identificador*.

El elemento opcional *type_parameter_list* especifica los parámetros de tipo del método ([parámetros de tipo](classes.md#type-parameters)). Si un *type_parameter_list* se especifica el método es un ***método genérico***. Si el método tiene un `extern` modificador, un *type_parameter_list* no se puede especificar.

El elemento opcional *formal_parameter_list* especifica los parámetros del método ([parámetros del método](classes.md#method-parameters)).

El elemento opcional *type_parameter_constraints_clause*s especificar restricciones en parámetros de tipo individuales ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) y solo se puede especificar si una *type_parameter_ lista* también se proporciona, y el método no tiene un `override` modificador.

El *return_type* y cada uno de los tipos que se hace referenciados en el *formal_parameter_list* de un método debe ser al menos tan accesibles como el propio método ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

El *cuerpoMétodo* ya sea un punto y coma, un ***cuerpo de instrucción*** o un ***cuerpo de expresión***. Consta de un cuerpo de instrucción de un *bloque*, que especifica las instrucciones que se ejecutará cuando se invoca el método. Un cuerpo de expresión consta de `=>` seguido por un *expresión* y un punto y coma y denota una expresión única para realizar cuando se invoca el método. 

Para `abstract` y `extern` métodos, el *method_body* consiste simplemente en un punto y coma. Para `partial` métodos el *method_body* puede constar de un punto y coma, un cuerpo de bloques o un cuerpo de expresión. Para todos los demás métodos, el *method_body* es un cuerpo de bloques o un cuerpo de expresión.

Si el *cuerpoMétodo* consta de un punto y coma, a continuación, la declaración no puede incluir el `async` modificador.

El nombre, la lista de parámetros de tipo y la lista de parámetros formales de un método de definen la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) del método. En concreto, la firma de un método está formada por su nombre, el número de parámetros de tipo y el número, modificadores y tipos de sus parámetros formales. Para estos fines, se identifica cualquier parámetro de tipo del método que se produce en el tipo de un parámetro formal no por su nombre, pero por su posición ordinal en la lista de argumentos de tipo del método. El tipo de valor devuelto no forma parte de una firma de método ni es los nombres de los parámetros de tipo o los parámetros formales.

El nombre de un método debe ser diferente de los nombres de todos los demás de que no son métodos declarados en la misma clase. Además, la firma de un método debe ser diferentes de las firmas de todos los demás métodos declarados en la misma clase, y dos métodos declarados en la misma clase no pueden tener firmas que se diferencien únicamente por `ref` y `out`.

El método *tipo_parámetro*están en el ámbito de la *method_declaration*y se puede usar para tipos de formulario a lo largo de ese ámbito en *return_type*, *cuerpoMétodo*, y *type_parameter_constraints_clause*s, pero no en *atributos*.

Todos los parámetros formales y parámetros de tipo deben tener nombres diferentes.

### <a name="method-parameters"></a>Parámetros de método

Los parámetros de un método, si existe, se declaran en el método *formal_parameter_list*.

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

La lista de parámetros formales consta de uno o más parámetros separados por comas de los cuales sólo el último puede ser un *parameter_array*.

Un *fixed_parameter* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), opcional `ref`, `out` o `this` modificador, un *tipo*, un *identificador* opcional *default_argument*. Cada *fixed_parameter* declara un parámetro del tipo especificado con el nombre especificado. El `this` modificador designa el método como método de extensión y solo se permite en el primer parámetro de un método estático. Métodos de extensión se describen con más detalle en [métodos de extensión](classes.md#extension-methods).

Un *fixed_parameter* con un *default_argument* se conoce como un ***parámetro opcional***, mientras que un *fixed_parameter* sin un *default_argument* es un ***parámetro necesario***. Un parámetro obligatorio no puede aparecer después de un parámetro opcional en un *formal_parameter_list*.

Un `ref` o `out` parámetro no puede tener un *default_argument*. El *expresión* en un *default_argument* debe ser uno de los siguientes:

*  a *constant_expression*
*  una expresión de formato `new S()` donde `S` es un tipo de valor
*  una expresión de formato `default(S)` donde `S` es un tipo de valor

El *expresión* debe poder convertirse implícitamente por una identidad o una conversión que acepta valores NULL al tipo del parámetro.

Si los parámetros opcionales se producen en una declaración de método parcial de implementación ([métodos parciales](classes.md#partial-methods)), una implementación de miembro de interfaz explícita ([implementaciones de miembros de interfaz explícita](interfaces.md#explicit-interface-member-implementations)) o en un declaración de indexador único parámetro ([indizadores](classes.md#indexers)) el compilador debe mostrar una advertencia, ya que estos miembros nunca se pueden invocar de forma que permite argumentos que se omitirán.

Un *parameter_array* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), un `params` modificador, un *array_type*, y un *identificador*. Una matriz de parámetros declara un único parámetro del tipo de matriz determinado con el nombre especificado. El *array_type* de un parámetro de matriz debe ser un tipo de matriz unidimensional ([tipos de matriz](arrays.md#array-types)). En una invocación de método, una matriz de parámetros permite que un único argumento del tipo de matriz determinado que se especifique o permite cero o más argumentos de tipo de elemento de matriz que se especifique. Las matrices de parámetros se describen más detalladamente en [las matrices de parámetros](classes.md#parameter-arrays).

Un *parameter_array* pueden aparecer después de un parámetro opcional, pero no puede tener un valor predeterminado, la omisión de argumentos para una *parameter_array* en su lugar, daría como resultado la creación de una matriz vacía.

El ejemplo siguiente muestra los diferentes tipos de parámetros:
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

En el *formal_parameter_list* para `M`, `i` es un parámetro ref necesarios, `d` es un parámetro de valor requerido, `b`, `s`, `o` y `t` parámetros de valor opcional y `a` es una matriz de parámetros.

Una declaración de método crea un espacio de declaración independiente para los parámetros, parámetros de tipo y las variables locales. Los nombres se incluyen en este espacio de declaración mediante la lista de parámetros de tipo y la lista de parámetros formales del método y las declaraciones de variable locales en el *bloque* del método. Es un error para dos miembros de un espacio de declaración de método tienen el mismo nombre. Es un error para el espacio de declaración de método y el espacio de declaración de variable local de un espacio de declaración anidado contengan elementos con el mismo nombre.

Una invocación de método ([las invocaciones de método](expressions.md#method-invocations)) crea una copia, específica de esa invocación, de los parámetros formales y variables locales del método y la lista de argumentos de la invocación asigna valores o referencias a variables a la acaba de crearse parámetros formales. Dentro de la *bloque* de un método, se pueden hacer referencia a parámetros formales mediante sus identificadores en *simple_name* expresiones ([nombres simples](expressions.md#simple-names)).

Hay cuatro tipos de parámetros formales:

*  Parámetros de valor, que se declaran sin modificadores.
*  Parámetros de referencia, que se declaran con la `ref` modificador.
*  Los parámetros de salida, que se declaran con la `out` modificador.
*  Las matrices de parámetros, que se declaran con la `params` modificador.

Como se describe en [firmas y sobrecarga](basic-concepts.md#signatures-and-overloading), `ref` y `out` modificadores forman parte de una firma de método, pero la `params` modificador no es.

#### <a name="value-parameters"></a>Parámetros de valor

Un parámetro declarado sin modificadores es un parámetro de valor. Un parámetro de valor corresponde a una variable local que obtiene su valor inicial del argumento correspondiente proporcionado en la invocación del método.

Cuando un parámetro formal es un parámetro de valor, el argumento correspondiente en una invocación de método debe ser una expresión que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo de parámetro formal.

Se permite un método para asignar nuevos valores a un parámetro de valor. Estas asignaciones sólo afectan a la ubicación de almacenamiento local representada por el valor del parámetro, no tienen ningún efecto en el argumento real definido en la invocación del método.

#### <a name="reference-parameters"></a>Parámetros de referencia

Un parámetro declarado con un `ref` modificador es un parámetro de referencia. A diferencia de un parámetro de valor, un parámetro de referencia no crea una nueva ubicación de almacenamiento. En su lugar, un parámetro de referencia representa la misma ubicación de almacenamiento que la variable especificada como argumento en la invocación del método.

Cuando un parámetro formal es un parámetro de referencia, el argumento correspondiente en una invocación de método debe constar de la palabra clave `ref` seguido por un *variable_reference* ([reglas precisas para determinar asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) del mismo tipo del parámetro formal. Una variable debe estar asignada definitivamente antes de que se puede pasar como un parámetro de referencia.

Dentro de un método, un parámetro de referencia siempre se considera asignado definitivamente.

Un método declarado como iterador ([iteradores](classes.md#iterators)) no puede tener parámetros de referencia.

El ejemplo
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
genera el resultado
```
i = 2, j = 1
```

Para la invocación de `Swap` en `Main`, `x` representa `i` y `y` representa `j`. Por lo tanto, la invocación tiene el efecto de intercambiar los valores de `i` y `j`.

En un método que toma parámetros de referencia, es posible que varios nombres representar la misma ubicación de almacenamiento. En el ejemplo
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
la invocación de `F` en `G` pasa una referencia al `s` para ambos `a` y `b`. Por lo tanto, para esa invocación, los nombres `s`, `a`, y `b` todas hacen referencia a la misma ubicación de almacenamiento y las tres asignaciones modifican el campo de instancia `s`.

#### <a name="output-parameters"></a>Parámetros de salida

Un parámetro declarado con un `out` modificador es un parámetro de salida. Al igual que un parámetro de referencia, un parámetro de salida no crea una nueva ubicación de almacenamiento. En su lugar, un parámetro de salida representa la misma ubicación de almacenamiento que la variable especificada como argumento en la invocación del método.

Cuando un parámetro formal es un parámetro de salida, el argumento correspondiente en una invocación de método debe constar de la palabra clave `out` seguido por un *variable_reference* ([reglas precisas para determinar asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) del mismo tipo del parámetro formal. Una variable no debe estar asignada definitivamente antes de que se puede pasar como un parámetro de salida, pero después de una invocación donde se pasó una variable como un parámetro de salida, la variable se considera asignada definitivamente.

Dentro de un método, al igual que una variable local variable, un parámetro de salida se considera inicialmente sin asignar y debe estar previamente asignada antes de que se utiliza su valor.

Cada parámetro de salida de un método debe asignarse definitivamente antes de que el método devuelve.

Un método declarado como un método parcial ([métodos parciales](classes.md#partial-methods)) o un iterador ([iteradores](classes.md#iterators)) no puede tener parámetros de salida.

Los parámetros de salida se usan normalmente en los métodos que devuelven varios valores. Por ejemplo:
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

El ejemplo genera el resultado:
```
c:\Windows\System\
hello.txt
```

Tenga en cuenta que el `dir` y `name` pueden estar sin asignadas variables antes de pasarlos a `SplitPath`, y que se considera asignados definitivamente posterior a la llamada.

#### <a name="parameter-arrays"></a>Matrices de parámetros

Un parámetro declarado con un `params` modificador es una matriz de parámetros. Si una lista de parámetros formales incluye una matriz de parámetros, debe ser el último parámetro en la lista y debe ser de un tipo de matriz unidimensional. Por ejemplo, los tipos `string[]` y `string[][]` puede usarse como el tipo de una matriz de parámetros, pero el tipo `string[,]` no puede. No es posible combinar la `params` modificador con los modificadores `ref` y `out`.

Una matriz de parámetros permite argumentos que se especifique en uno de dos formas de una invocación de método:

*  El argumento especificado para una matriz de parámetros puede ser una sola expresión que es implícitamente convertible ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo de matriz de parámetros. En este caso, la matriz de parámetros actúa exactamente como un parámetro de valor.
*  Como alternativa, la invocación puede especificar cero o más argumentos para la matriz de parámetros, donde cada argumento es una expresión que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) para el tipo de elemento de la matriz de parámetros. En este caso, la invocación crea una instancia del parámetro de tipo de matriz con una longitud correspondiente al número de argumentos, inicializa los elementos de la instancia de matriz con los valores de argumento especificado y usa la instancia de matriz recién creado como real argumento.

Excepto para permitir un número variable de argumentos en una invocación, una matriz de parámetros equivale exactamente a un parámetro de valor ([parámetros del valor](classes.md#value-parameters)) del mismo tipo.

El ejemplo
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
genera el resultado
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

La primera invocación de `F` simplemente pasa la matriz `a` como un parámetro de valor. La segunda invocación de `F` crea automáticamente un elemento de cuatro `int[]` con los valores de elemento especificados y pasa ese instancia como un parámetro de valor de matriz. Del mismo modo, la tercera invocación de `F` crea un elemento de cero `int[]` y pasa esa instancia como un parámetro de valor. Las invocaciones de segunda y terceros son exactamente equivalentes a escribir:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Al realizar la resolución de sobrecarga, un método con una matriz de parámetros puede ser aplicable en su forma normal o en su forma expandida ([miembro de función aplicable](expressions.md#applicable-function-member)). La forma expandida de un método está disponible sólo si la forma normal del método no es aplicable, y solo si un método aplicable con la misma firma que la forma expandida ya no está declarado en el mismo tipo.

El ejemplo
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
genera el resultado
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

En el ejemplo, dos de las posibles formas expandidas del método con una matriz de parámetros ya están incluidas en la clase con métodos periódicas. Estas formas expandidas no por lo tanto, se consideran al realizar la resolución de sobrecarga y las invocaciones de método primero y tercero, por tanto, seleccionan los métodos regulares. Cuando una clase declara un método con una matriz de parámetros, no es raro que incluir también algunas de las formas expandidas como métodos regulares. Por lo que es posible evitar la asignación de una matriz se invoca la instancia que se produce cuando una forma expandida de un método con una matriz de parámetros.

Cuando el tipo de una matriz de parámetros es `object[]`, puede producirse una ambigüedad entre la forma normal del método y la forma expandida para un único `object` parámetro. La razón de la ambigüedad es que un `object[]` es implícitamente convertible al tipo `object`. La ambigüedad no presenta ningún problema, sin embargo, puesto que se puede resolver mediante la inserción de una conversión explícita si es necesario.

El ejemplo
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
genera el resultado
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

En la primera y última invocación de `F`, la forma normal de `F` es aplicable porque existe una conversión implícita desde el tipo de argumento para el tipo de parámetro (ambos son de tipo `object[]`). Por lo tanto, la resolución de sobrecarga selecciona la forma normal de `F`, y el argumento se pasa como un parámetro de valor normales. En las invocaciones segunda y terceros, la forma normal de `F` no es aplicable porque no existe ninguna conversión implícita desde el tipo de argumento para el tipo de parámetro (tipo `object` no se puede convertir implícitamente al tipo `object[]`). Sin embargo, la forma expandida de `F` es aplicable, por lo que se selecciona mediante la resolución de sobrecarga. Como resultado, un solo elemento `object[]` se crea mediante la invocación, y el único elemento de la matriz se inicializa con el valor del argumento especificado (que a su vez es una referencia a un `object[]`).

### <a name="static-and-instance-methods"></a>Métodos estáticos y de instancia

Cuando una declaración de método incluye un `static` modificador, que el método se dice que es un método estático. Cuando no hay ninguna `static` modificador está presente, se dice que el método es un método de instancia.

Un método estático no opera en una instancia concreta, y es un error en tiempo de compilación para hacer referencia a `this` en un método estático.

Un método de instancia opera en una instancia determinada de una clase, y puede tener acceso a esa instancia como `this` ([este acceso](expressions.md#this-access)).

Cuando se hace referencia a un método en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`si `M` es un método estático, `E` debe denotar un tipo que contiene `M`y si `M` es un método de instancia, `E` debe denotar una instancia de un tipo que contenga `M`.

Las diferencias entre estático y los miembros de instancia se tratan con más detalle en [miembros estáticos y de instancia](classes.md#static-and-instance-members).

### <a name="virtual-methods"></a>Métodos virtuales

Cuando una declaración de método de instancia incluye un `virtual` modificador, que el método se dice que es un método virtual. Cuando no hay ninguna `virtual` modificador está presente, se dice que el método es un método no virtual.

La implementación de un método no virtual es invariable: La implementación es la misma, si se invoca el método en una instancia de la clase en que se declaró o una instancia de una clase derivada. En cambio, las clases derivadas pueden reemplazar la implementación de un método virtual. El proceso de sustitución de la implementación de un método virtual heredado se conoce como ***reemplazar*** ese método ([invalidar métodos](classes.md#override-methods)).

En una invocación de método virtual, el ***tipo en tiempo de ejecución*** de la instancia para el que tiene esa invocación lugar determina la implementación del método real a invocar. En una invocación de método no virtual, el ***tipo en tiempo de compilación*** de la instancia es el factor determinante. En términos precisos, cuando un método denominado `N` se invoca con una lista de argumentos `A` en una instancia con un tipo de tiempo de compilación `C` y un tipo de tiempo de ejecución `R` (donde `R` sea `C` o una clase derivada desde `C`), la invocación se procesa como sigue:

*  En primer lugar, se aplica la resolución de sobrecarga a `C`, `N`, y `A`para seleccionar un método específico `M` desde el conjunto de métodos declarada en y heredadas por `C`. Esto se describe en [las invocaciones de método](expressions.md#method-invocations).
*  A continuación, si `M` es un método no virtual, `M` se invoca.
*  En caso contrario, `M` es un método virtual y la implementación más derivada de `M` con respecto a `R` se invoca.

Para cada método virtual declarado en o heredado por una clase, existe una ***implementación más derivada*** del método con respecto a esa clase. La implementación más derivada de un método virtual `M` con respecto a una clase `R` se determina como sigue:

*  Si `R` contiene la presentación de `virtual` declaración de `M`, a continuación, se trata de la implementación más derivada de `M`.
*  De lo contrario, si `R` contiene un `override` de `M`, a continuación, se trata de la implementación más derivada de `M`.
*  La mayor parte de lo contrario, la implementación de derivada `M` con respecto a `R` es igual que la implementación más derivada de `M` con respecto a la clase base directa de `R`.

En el ejemplo siguiente se ilustra las diferencias entre los métodos virtuales y no virtuales:
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

En el ejemplo, `A` presenta un método no virtual `F` y un método virtual `G`. La clase `B` presenta un nuevo método no virtual `F`, ocultando así las heredadas `F`y también invalida el método heredado `G`. El ejemplo genera el resultado:
```
A.F
B.F
B.G
B.G
```

Tenga en cuenta que la instrucción `a.G()` invoca `B.G`, no `A.G`. Esto es porque el tipo de tiempo de ejecución de la instancia (que es `B`), no el tipo de tiempo de compilación de la instancia (que es `A`), determina la implementación del método real a invocar.

Dado que se permiten métodos para ocultar los métodos heredados, es posible que una clase que contiene varios métodos virtuales con la misma firma. Esto no presenta un problema de ambigüedad, puesto que se ocultan todos excepto el método más derivado. En el ejemplo
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
el `C` y `D` clases contienen dos métodos virtuales con la misma firma: El introducidos por `A` y otro introducido por `C`. El método introducido por `C` oculta el método heredado de `A`. Por lo tanto, la declaración de reemplazo en `D` invalida el método introducido por `C`, y no es posible que `D` para invalidar el método introducido por `A`. El ejemplo genera el resultado:
```
B.F
B.F
D.F
D.F
```

Tenga en cuenta que es posible invocar el método virtual oculto mediante el acceso a una instancia de `D` a través de una menor derivada tipo en el que el método no está oculto.

### <a name="override-methods"></a>Invalidar métodos

Cuando una declaración de método de instancia incluye un `override` modificador, se dice que el método sea un ***invalidar el método***. Un método de invalidación invalida un método virtual heredado con la misma firma. Mientras que una declaración de método virtual introduce un método nuevo, una declaración de método de reemplazo especializa un método virtual heredado existente proporcionando una nueva implementación de ese método.

El método reemplazado por una `override` declaración se conoce como el ***se invalida el método base***. Para un método de invalidación `M` declarado en una clase `C`, el método base reemplazado se determina examinando cada tipo de clase base del `C`, empezando por el tipo de clase base directa de `C` y continuando con cada sucesivas tipo de clase base directa, hasta en un tipo de clase base determinada, al menos uno es el método accesible encuentra que tiene la misma firma que `M` después de la sustitución de argumentos de tipo. Para los fines de buscar el método base reemplazado, se considera accesible si es un método `public`, si es `protected`, si es `protected internal`, o si es `internal` y se ha declarado en el mismo programa como `C`.

Se produce un error de tiempo de compilación a menos que todos los elementos siguientes son true para una declaración de reemplazo:

*  Un método base invalidado puede encontrarse como se describió anteriormente.
*  No hay exactamente un tal método base invalidado. Esta restricción tiene efecto solo si el tipo de clase base es un tipo construido donde la sustitución de argumentos de tipo hace que la firma de los dos métodos de la misma.
*  El método base reemplazado es un virtual, abstract o invalidar el método. En otras palabras, el método base reemplazado no puede ser estático o no virtual.
*  El método base reemplazado no es un método sellado.
*  El método de reemplazo y el método base reemplazado tienen el mismo tipo de valor devuelto.
*  La declaración de reemplazo y el método base reemplazado tienen la misma accesibilidad declarada. En otras palabras, una declaración de reemplazo no puede cambiar la accesibilidad del método virtual. Sin embargo, si el método base reemplazado es protected internal y se declara en un ensamblado diferente que el ensamblado que contiene el método de invalidación, a continuación, el método de invalidación declarado accesibilidad debe estar protegida.
*  La declaración de reemplazo no especifica el tipo de parámetro restricciones cláusulas. En su lugar, las restricciones se heredan del método base invalidado. Tenga en cuenta que las restricciones que son parámetros de tipo en el método invalidado pueden reemplazarse por argumentos de tipo en la restricción heredada. Esto puede conducir a las restricciones que no sean válidas cuando se especifica explícitamente, como tipos de valor o tipos sellados.

El ejemplo siguiente muestra cómo funcionan las reglas de reemplazo para las clases genéricas:
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

Puede tener acceso a una declaración de reemplazo del método base invalidado utilizando un *base_access* ([Base acceso](expressions.md#base-access)). En el ejemplo
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
el `base.PrintFields()` invocación en `B` invoca el `PrintFields` método declarado en `A`. Un *base_access* deshabilita el mecanismo de llamada virtual y simplemente se trata del método base como un método no virtual. Tenían la invocación de `B` sido escritos `((A)this).PrintFields()`, lo haría de forma recursiva invocar el `PrintFields` método declarado en `B`, no uno que se declara en `A`, puesto que `PrintFields` es virtual y el tipo de tiempo de ejecución de `((A)this)` es `B`.

Mediante la inclusión de un `override` can modificador un método invalidan otro método. En todos los demás casos, un método con la misma firma que un método heredado simplemente oculta el método heredado. En el ejemplo
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
el `F` método `B` no incluye un `override` modificador y, por tanto, no reemplaza el `F` método `A`. En su lugar, el `F` método `B` oculta el método en `A`, y se notifica una advertencia porque la declaración no incluye un `new` modificador.

En el ejemplo
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
el `F` método `B` oculta virtual `F` método hereda `A`. Desde el nuevo `F` en `B` tiene acceso privado, su ámbito sólo incluye el cuerpo de la clase de `B` y no se extiende a `C`. Por lo tanto, la declaración de `F` en `C` se permite reemplazar la `F` hereda `A`.

### <a name="sealed-methods"></a>Métodos sellados

Cuando una declaración de método de instancia incluye un `sealed` modificador, que el método se dice que un ***sellado método***. Si una declaración de método de instancia incluye la `sealed` modificador, también debe incluir el `override` modificador. El uso de la `sealed` modificador impide que una clase derivada siga reemplazando el método.

En el ejemplo
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
la clase `B` proporciona dos métodos de reemplazo: un `F` método que tiene el `sealed` modificador y un `G` método que no lo hace. `B`uso del sellado `modifier` impide `C` invaliden aún más `F`.

### <a name="abstract-methods"></a>Métodos abstractos

Cuando una declaración de método de instancia incluye un `abstract` modificador, que el método se dice que un ***método abstracto***. Aunque un método abstracto es también implícitamente un método virtual, no puede tener el modificador `virtual`.

Una declaración de método abstracto introduce un nuevo método virtual pero no proporciona una implementación de ese método. En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación invalidando este método. Dado que un método abstracto no proporciona ninguna implementación real, el *cuerpoMétodo* de un método abstracto consiste simplemente en un punto y coma.

Solo se permiten declaraciones de métodos abstractos en clases abstractas ([clases abstractas](classes.md#abstract-classes)).

En el ejemplo
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
la `Shape` clase define el concepto abstracto de un objeto de forma geométrica que puede dibujarse a sí mismo. El `Paint` método es abstracto, porque no hay ninguna implementación predeterminada significativa. El `Ellipse` y `Box` clases son concretas `Shape` implementaciones. Dado que estas clases no son abstractas, es necesario reemplazar el `Paint` método y proporcionar una implementación real.

Es un error en tiempo de compilación para un *base_access* ([Base acceso](expressions.md#base-access)) para hacer referencia a un método abstracto. En el ejemplo
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
se notifica un error en tiempo de compilación para el `base.F()` invocación porque hace referencia a un método abstracto.

Una declaración de método abstracto se permite reemplazar un método virtual. Esto permite que una clase abstracta fuerce una nueva implementación del método en las clases derivadas y hace que la implementación original del método no está disponible. En el ejemplo
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
clase `A` declara un método virtual, la clase `B` invalida este método con un método abstracto y una clase `C` reemplaza el método abstracto para proporcionar su propia implementación.

### <a name="external-methods"></a>Métodos externos

Cuando una declaración de método incluye un `extern` modificador, que el método se dice que un ***método externo***. Métodos externos se implementan de forma externa, normalmente mediante un lenguaje distinto de C#. Dado que una declaración de método externo no proporciona ninguna implementación real, el *cuerpoMétodo* de un método externo consiste simplemente en un punto y coma. Un método externo no puede ser genérico.

El `extern` modificador se usa normalmente junto con un `DllImport` atributo ([interoperabilidad con componentes COM y Win32](attributes.md#interoperation-with-com-and-win32-components)), lo que permite métodos externos que se implementa en los archivos DLL (bibliotecas de vínculos dinámicos). El entorno de ejecución puede admitir otros mecanismos mediante el cual se pueden proporcionar implementaciones de métodos externos.

Cuando un método externo incluye un `DllImport` atributo, la declaración del método también debe incluir un `static` modificador. En este ejemplo se muestra el uso de la `extern` modificador y `DllImport` atributo:
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a>Métodos parciales (resumen)

Cuando una declaración de método incluye un `partial` modificador, que el método se dice que un ***método parcial***. Los métodos parciales solo se pueden declarar como miembros de tipos parciales ([tipos parciales](classes.md#partial-types)) y están sujetos a un número de restricciones. Los métodos parciales se describen con más detalle en [métodos parciales](classes.md#partial-methods).

### <a name="extension-methods"></a>Métodos de extensión

Cuando el primer parámetro de un método incluye el `this` modificador, que el método se dice que un ***método de extensión***. Métodos de extensión solo pueden declararse en clases estáticas no genérica y no anidados. El primer parámetro de un método de extensión no puede tener ningún modificador distinto `this`, y el tipo de parámetro no puede ser un tipo de puntero.

El siguiente es un ejemplo de una clase estática que declara dos métodos de extensión:
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

Un método de extensión es un método estático normal. Además, cuando su clase estática envolvente en el ámbito, un método de extensión se puede invocar mediante sintaxis de invocación de método de instancia ([las invocaciones de método de extensión](expressions.md#extension-method-invocations)), mediante la expresión del receptor como primer argumento.

El siguiente programa utiliza los métodos de extensión declarados anteriormente:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

El `Slice` método está disponible en el `string[]`y el `ToInt32` método está disponible en `string`, porque se ha declarado como métodos de extensión. El significado del programa es igual que las llamadas de método estático normal siguiente, mediante:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a>Cuerpo del método

El *cuerpoMétodo* de un método de declaración consta de un cuerpo de bloques, un cuerpo de expresión o un punto y coma.

El ***tipo de resultado*** de un método es `void` si el tipo de valor devuelto es `void`, o si el método es asincrónico y el tipo de valor devuelto es `System.Threading.Tasks.Task`. En caso contrario, el tipo de resultado de un método que no sea asincrónico es su tipo de valor devuelto y el tipo de resultado con tipo de valor devuelto de un método asincrónico `System.Threading.Tasks.Task<T>` es `T`.

Cuando un método que tiene un `void` como resultado de tipo y un cuerpo de bloques, `return` instrucciones ([la instrucción return](statements.md#the-return-statement)) no se permiten en el bloque al especificar una expresión. Si la ejecución del bloque de un método void termina normalmente (es decir, el control llega al final del cuerpo del método), método regresa a su llamador actual.
    
Cuando un método que tiene un `void` resultado y un cuerpo de expresión, la expresión `E` debe ser un *statement_expression*, y el cuerpo es equivalente exactamente a un cuerpo de bloques del formulario `{ E; }`.
    
Cuando un método tiene un tipo de resultado distinto de void y un bloque de cuerpo, cada uno de ellos `return` instrucción del bloque debe especificar una expresión que es implícitamente convertible al tipo de resultado. El punto de conexión de un cuerpo de bloques de un método devuelve un valor no debe ser accesible. En otras palabras, en un método devuelve un valor con un cuerpo de bloques, no se permite que el control al alcanzar el final del cuerpo del método.
    
Cuando un método que tiene un tipo de resultado distinto de void y un cuerpo de expresión, la expresión debe poder convertirse implícitamente al tipo de resultado y el cuerpo equivale exactamente a un cuerpo de bloques del formulario `{ return E; }`.
    
En el ejemplo
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
el valor de devolución `F` método produce un error de tiempo de compilación porque el control puede alcanzar el final del cuerpo del método. El `G` y `H` métodos son correctos, ya que todas las rutas de ejecución posibles terminan en una instrucción return que especifica un valor devuelto. El `I` método es correcto, porque el cuerpo es equivalente a un bloque de instrucciones con una sola instrucción return en ella.

### <a name="method-overloading"></a>Sobrecarga de métodos

Se describen las reglas de resolución de sobrecarga del método en [inferencia de tipo](expressions.md#type-inference).

## <a name="properties"></a>Propiedades

Un ***propiedad*** es un miembro que proporciona acceso a una característica de un objeto o una clase. Ejemplos de propiedades incluyen la longitud de una cadena, el tamaño de una fuente, el título de una ventana, el nombre de un cliente y así sucesivamente. Las propiedades son una extensión natural de los campos, ambos son miembros denominados con tipos asociados, y la sintaxis para tener acceso a los campos y propiedades es la misma. Sin embargo, a diferencia de los campos, las propiedades no denotan ubicaciones de almacenamiento. Las propiedades tienen ***descriptores de acceso*** que especifican las instrucciones que se ejecutan cuando se leen o escriben sus valores. Las propiedades proporcionan un mecanismo para asociar las acciones con la lectura y escritura de los atributos del objeto; Además, permiten esos atributos que se va a calcular.

Las propiedades se declaran mediante *property_declaration*s:

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

Un *property_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso ](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` ([Invalidar métodos](classes.md#override-methods)), `sealed` ([sellado métodos](classes.md#sealed-methods)), `abstract` ([métodos abstractos](classes.md#abstract-methods)), y `extern` ([Métodos externos](classes.md#external-methods)) modificadores.

Las declaraciones de propiedad están sujetos a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores.

El *tipo* de una propiedad de la declaración especifica el tipo de la propiedad que se incluyen en la declaración y el *member_name* especifica el nombre de la propiedad. A menos que la propiedad es una implementación de miembro de interfaz explícita, el *member_name* es simplemente un *identificador*. Para obtener una implementación de miembro de interfaz explícita ([implementaciones de miembros de interfaz explícita](interfaces.md#explicit-interface-member-implementations)), el *member_name* consta de un *interface_type* seguido de un " `.`"y un *identificador*.

El *tipo* de una propiedad debe ser al menos tan accesibles como la propiedad propiamente dicha ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

Un *property_body* puede constar de un ***cuerpo del descriptor de acceso*** o un ***cuerpo de expresión***. En el cuerpo de un descriptor de acceso, *accessor_declarations*, que debe incluirse en "`{`"y"`}`" tokens, declare los descriptores de acceso ([descriptores de acceso](classes.md#accessors)) de la propiedad. Los descriptores de acceso especifican las instrucciones ejecutables asociadas con la lectura y escritura de la propiedad.

Un cuerpo de expresión formada por `=>` seguido por un *expresión* `E` y un punto y coma es igual que en el cuerpo de instrucción `{ get { return E; } }`y por lo tanto, sólo puede utilizarse para especificar solo captador propiedades donde se indica el resultado de Get por una sola expresión.

Un *property_initializer* sólo puede asignarse una propiedad implementada automáticamente ([implementa automáticamente propiedades](classes.md#automatically-implemented-properties)) y hace que la inicialización del campo subyacente de estos las propiedades con el valor especificado por el *expresión*.

Aunque la sintaxis para tener acceso a una propiedad es el mismo que para un campo, una propiedad no está clasificada como una variable. Por lo tanto, no es posible pasar una propiedad como un `ref` o `out` argumento.

Cuando una declaración de propiedad incluye un `extern` modificador, la propiedad se dice que un ***propiedad externa***. Dado que una declaración de propiedad externa no proporciona ninguna implementación real, cada uno de sus *accessor_declarations* consta de un punto y coma.

### <a name="static-and-instance-properties"></a>Propiedades de instancia y estáticos

Cuando una declaración de propiedad incluye un `static` modificador, la propiedad se dice que un ***propiedad estática***. Cuando no hay ninguna `static` modificador está presente, la propiedad se dice que un ***propiedad de instancia***.

Una propiedad estática no está asociada con una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en los descriptores de acceso de una propiedad estática.

Una propiedad de instancia está asociada con una instancia determinada de una clase, y puede tener acceso a esa instancia como `this` ([este acceso](expressions.md#this-access)) en los descriptores de acceso de esa propiedad.

Cuando se hace referencia a una propiedad en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`si `M` es una propiedad estática, `E` debe denotar un tipo que contiene `M`y si `M` es una propiedad de instancia, E debe denotar una instancia de un tipo que contenga `M`.

Las diferencias entre estático y los miembros de instancia se tratan con más detalle en [miembros estáticos y de instancia](classes.md#static-and-instance-members).

### <a name="accessors"></a>Descriptores de acceso

El *accessor_declarations* de una propiedad, especifique las instrucciones ejecutables asociadas con la lectura y escritura de la propiedad.

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

Las declaraciones de descriptor de acceso que constan de un *get_accessor_declaration*, un *set_accessor_declaration*, o ambos. Cada declaración de descriptor de acceso está formado por el token `get` o `set` seguido de un elemento opcional *accessor_modifier* y un *accessor_body*.

El uso de *accessor_modifier*s se rige por las siguientes restricciones:

*  Un *accessor_modifier* no puede usarse en una interfaz o en una implementación de miembro de interfaz explícita.
*  Para una propiedad o indizador que no tiene ningún `override` modificador, un *accessor_modifier* sólo está permitido si la propiedad o indizador tiene tanto un `get` y `set` descriptor de acceso y, a continuación, sólo se permite en una de ellas descriptores de acceso.
*  Para una propiedad o indizador que incluye un `override` modificador, un descriptor de acceso debe coincidir con el *accessor_modifier*, si lo hay, del descriptor de acceso que se va a invalidar.
*  El *accessor_modifier* debe declarar una accesibilidad que es estrictamente más restrictiva que la accesibilidad declarada de la propiedad o el propio indexador. Para ser precisos:
   * Si la propiedad o indizador tiene una accesibilidad declarada de `public`, *accessor_modifier* pueden ser `protected internal`, `internal`, `protected`, o `private`.
   * Si la propiedad o indizador tiene una accesibilidad declarada de `protected internal`, *accessor_modifier* pueden ser `internal`, `protected`, o `private`.
   * Si la propiedad o indizador tiene una accesibilidad declarada de `internal` o `protected`, *accessor_modifier* debe ser `private`.
   * Si la propiedad o indizador tiene una accesibilidad declarada de `private`, no *accessor_modifier* puede utilizarse.

Para `abstract` y `extern` propiedades, el *accessor_body* para cada descriptor de acceso especificado es simplemente un punto y coma. Puede tener una propiedad no abstracta, no extern cada *accessor_body* sea un punto y coma, en cuyo caso es un ***propiedad implementada automáticamente*** ([implementa automáticamente propiedades ](classes.md#automatically-implemented-properties)). Una propiedad implementada automáticamente debe tener al menos un descriptor de acceso get. Para los descriptores de acceso de otra propiedad no abstracta, no externos, el *accessor_body* es un *bloque* que especifica las instrucciones que se ejecuta cuando se invoca el descriptor de acceso correspondiente.

Un `get` descriptor de acceso corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad. Excepto como destino de una asignación, cuando se hace referencia a una propiedad en una expresión, el `get` descriptor de acceso de la propiedad se invoca para calcular el valor de la propiedad ([valores de expresiones](expressions.md#values-of-expressions)). El cuerpo de un `get` descriptor de acceso debe ajustarse a las reglas de devolución por valor métodos descritos en [cuerpo del método](classes.md#method-body). En concreto, todos los `return` instrucciones en el cuerpo de un `get` descriptor de acceso debe especificar una expresión que es implícitamente convertible al tipo de propiedad. Además, el punto de conexión de un `get` descriptor de acceso no debe ser accesible.

Un `set` descriptor de acceso corresponde a un método con un parámetro de valor único del tipo de propiedad y un `void` tipo de valor devuelto. El parámetro implícito de un `set` descriptor de acceso siempre se denomina `value`. Cuando se hace referencia a una propiedad como el destino de una asignación ([operadores de asignación](expressions.md#assignment-operators)), o como el operando de `++` o `--` ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators), [ Prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)), el `set` descriptor de acceso se invoca con un argumento (cuyo valor es el de la derecha de la asignación o el operando de la `++` o `--` operador) que proporciona el nuevo valor ([asignación Simple](expressions.md#simple-assignment)). El cuerpo de un `set` descriptor de acceso debe cumplir las reglas de `void` métodos descritos en [cuerpo del método](classes.md#method-body). En concreto, `return` instrucciones en el `set` cuerpo del descriptor de acceso no se permiten especificar una expresión. Puesto que un `set` descriptor de acceso tiene implícitamente un parámetro denominado `value`, es un error de tiempo de compilación de una declaración de variable o una constante local en un `set` descriptor de acceso de ese nombre.

Según la presencia o ausencia de la `get` y `set` descriptores de acceso, una propiedad se clasifica como sigue:

*  Una propiedad que incluye tanto una `get` descriptor de acceso y un `set` descriptor de acceso se dice que un ***lectura-escritura*** propiedad.
*  Una propiedad que tiene solo un `get` descriptor de acceso se dice que un ***de sólo lectura*** propiedad. Es un error en tiempo de compilación para una propiedad de solo lectura a ser el destino de una asignación.
*  Una propiedad que tiene solo un `set` descriptor de acceso se dice que un ***de solo escritura*** propiedad. Excepto como destino de una asignación, es un error en tiempo de compilación para hacer referencia a una propiedad de solo escritura en una expresión.

En el ejemplo
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
el `Button` control declara una pública `Caption` propiedad. El `get` descriptor de acceso de la `Caption` propiedad devuelve la cadena almacenada en privado `caption` campo. El `set` comprueba si el nuevo valor es diferente del valor actual; en caso afirmativo, almacena el nuevo valor de descriptor de acceso y vuelve a dibujar el control. Las propiedades a menudo siguen el patrón anterior: El `get` descriptor de acceso simplemente devuelve un valor almacenado en un campo privado y el `set` modifica el campo privado de descriptor de acceso y, a continuación, realiza acciones adicionales necesarias para actualizar completamente el estado del objeto.

Dada la `Button` clase anterior, el siguiente es un ejemplo de uso de la `Caption` propiedad:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

En este caso, el `set` se invoca el descriptor de acceso asignando un valor a la propiedad y el `get` descriptor de acceso se invoca haciendo referencia a la propiedad en una expresión.

El `get` y `set` descriptores de acceso de una propiedad no son miembros distintos, y no es posible declarar los descriptores de acceso de una propiedad por separado. Por lo tanto, no es posible que los dos descriptores de acceso de una propiedad de lectura y escritura tener una accesibilidad diferente. El ejemplo
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
no se declara una sola propiedad de lectura y escritura. En su lugar, declaran dos propiedades con el mismo nombre, una de sólo lectura y otra de solo escritura. Dado que dos miembros declarados en la misma clase no pueden tener el mismo nombre, el ejemplo hace que se produzca un error de tiempo de compilación.

Cuando una clase derivada declara una propiedad con el mismo nombre que una propiedad heredada, la propiedad derivada oculta la propiedad heredada con respecto a las operaciones de lectura y escritura. En el ejemplo
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
el `P` propiedad `B` oculta la `P` propiedad `A` con respecto a las operaciones de lectura y escritura. Por lo tanto, en las instrucciones
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
la asignación a `b.P` provoca un error de tiempo de compilación notificará desde sólo lectura `P` propiedad `B` oculta solo escritura `P` propiedad en `A`. Sin embargo, tenga en cuenta que puede usarse una conversión para tener acceso a la oculta `P` propiedad.

A diferencia de los campos públicos, las propiedades proporcionan una separación entre el estado interno de un objeto y su interfaz pública. Considere el ejemplo:
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

En este caso, el `Label` clase utiliza dos `int` campos, `x` y `y`, para almacenar su ubicación. La ubicación es públicamente expuestos como un `X` y un `Y` propiedad y, como un `Location` propiedad de tipo `Point`. Si se encuentra, en una versión futura de `Label`, resulta más conveniente almacenar la ubicación como una `Point` internamente, el cambio se puede realizar sin que afecte a la interfaz pública de la clase:
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

Tenía `x` y `y` en su lugar se ha sido `public readonly` campos, habría sido imposible realizar este cambio a la `Label` clase.

Expone el estado a través de propiedades no es necesariamente menos eficiente que exponer directamente los campos. En concreto, cuando una propiedad no es virtual y contiene sólo una pequeña cantidad de código, el entorno de ejecución puede reemplazar las llamadas a los descriptores de acceso con el código real de los descriptores de acceso. Este proceso se conoce como ***inserción***, y facilita el acceso a la propiedad tan eficaz como el acceso a campos al mismo, conserva la mayor flexibilidad de las propiedades.

Desde la invocación de un `get` es conceptualmente equivalente a leer el valor de un campo de descriptor de acceso, se considera mal estilo de programación para `get` descriptores de acceso tienen efectos observables. En el ejemplo
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
el valor de la `Next` propiedad depende del número de veces que previamente ha accedido a la propiedad. Por lo tanto, el acceso a la propiedad produce un efecto secundario observable y la propiedad debe implementarse como un método en su lugar.

La convención "sin efectos secundarios" para `get` descriptores de acceso no significa que `get` siempre se deben escribir los descriptores de acceso para devolver los valores almacenados en campos. De hecho, `get` descriptores de acceso de proceso a menudo el valor de una propiedad mediante el acceso a varios campos o invocar métodos. Sin embargo, diseñado correctamente `get` descriptor de acceso realiza ninguna acción que provocan cambios observables en el estado del objeto.

Las propiedades se pueden usar para retrasar la inicialización de un recurso hasta el momento en que se hace referencia en primer lugar. Por ejemplo:
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

El `Console` clase contiene tres propiedades `In`, `Out`, y `Error`, que representan la entrada estándar, salida y error de dispositivos, respectivamente. Mediante la exposición de estos miembros como propiedades, la `Console` clase puede retrasar su inicialización hasta que se usan realmente. Por ejemplo, en la primera referencia a la `Out` propiedad, como en
```csharp
Console.Out.WriteLine("hello, world");
```
subyacente `TextWriter` para crear el dispositivo de salida. Sin embargo, si la aplicación no hace referencia a la `In` y `Error` propiedades y, a continuación, no hay ningún objeto que se crea para esos dispositivos.

### <a name="automatically-implemented-properties"></a>Propiedades implementadas automáticamente

Una propiedad implementada automáticamente (o ***autopropiedad*** para abreviar), es una propiedad de no abstracta no extern con cuerpos de descriptor de acceso de sólo punto y coma. Propiedades automáticas deben tener un descriptor de acceso get y, opcionalmente, pueden tener un descriptor de acceso.

Cuando se especifica una propiedad como una propiedad implementada automáticamente, un campo de respaldo oculto está automáticamente disponible para la propiedad y se implementan los descriptores de acceso para leer y escribir en ese campo de respaldo. Si la propiedad automática no tiene ningún descriptor de acceso set, el campo de respaldo se considera `readonly` ([campos de solo lectura](classes.md#readonly-fields)). Al igual que un `readonly` campo, una propiedad automática solo captador puede asignarse a en el cuerpo de un constructor de la clase envolvente. Esta asignación se asigna directamente en el campo de respaldo de solo lectura de la propiedad.

Una propiedad automática podría tener opcionalmente un *property_initializer*, que se aplica directamente en el campo de respaldo como un *variable_initializer* ([inicializadores de variables](classes.md#variable-initializers)) .

En el ejemplo siguiente:
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
es equivalente a la siguiente declaración:
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

En el ejemplo siguiente:
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
es equivalente a la siguiente declaración:
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

Tenga en cuenta que las asignaciones para el campo de solo lectura son válidas, porque se producen dentro del constructor.


### <a name="accessibility"></a>Accesibilidad

Si tiene un descriptor de acceso una *accessor_modifier*, el dominio de accesibilidad ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)) del descriptor de acceso se determina mediante la accesibilidad declarada de la *accessor_modifier* . Si no tiene un descriptor de acceso una *accessor_modifier*, el dominio de accesibilidad del descriptor de acceso viene determinada por la accesibilidad declarada de la propiedad o indizador.

La presencia de un *accessor_modifier* nunca afecta a la búsqueda de miembros ([operadores](expressions.md#operators)) o de resolución de sobrecarga ([de resolución de sobrecarga](expressions.md#overload-resolution)). Los modificadores de la propiedad o indizador siempre determinan qué propiedad o indizador está enlazado, independientemente del contexto del acceso.

Una vez que se ha seleccionado una propiedad o indizador concreto, los dominios de accesibilidad de los descriptores de acceso específicos implicados se utilizan para determinar si el uso es válido:

*  Si el uso es como un valor ([valores de expresiones](expressions.md#values-of-expressions)), el `get` descriptor de acceso debe existir y ser accesible.
*  Si el uso es como el destino de una asignación simple ([asignación Simple](expressions.md#simple-assignment)), el `set` descriptor de acceso debe existir y ser accesible.
*  Si el uso es como el destino de asignación compuesta ([asignación compuesta](expressions.md#compound-assignment)), o como destino de la `++` o `--` operadores ([miembros de función](expressions.md#function-members).9, [ Expresiones de invocación](expressions.md#invocation-expressions)), tanto el `get` descriptores de acceso y el `set` descriptor de acceso debe existir y ser accesible.

En el ejemplo siguiente, la propiedad `A.Text` está oculto por la propiedad `B.Text`, incluso en contextos en los que sólo el `set` se denomina descriptor de acceso. En cambio, la propiedad `B.Count` no es accesible para la clase `M`, por lo que la propiedad accesible `A.Count` se usa en su lugar.

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

Un descriptor de acceso que se usa para implementar una interfaz no puede tener un *accessor_modifier*. Si solo un descriptor de acceso se utiliza para implementar una interfaz, el otro descriptor de acceso se puede declarar con un *accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Virtual, sellado, de reemplazo y descriptores de acceso de propiedad abstracta

Un `virtual` declaración de propiedad especifica que los descriptores de acceso de la propiedad son virtuales. El `virtual` modificador se aplica a ambos descriptores de acceso de una propiedad de lectura y escritura, no es posible que solo un descriptor de acceso de una propiedad de lectura y escritura sea virtual.

Un `abstract` declaración de propiedad especifica que los descriptores de acceso de la propiedad son virtuales, pero no proporciona una implementación real de los descriptores de acceso. En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación para los descriptores de acceso mediante la invalidación de la propiedad. Dado que un descriptor de acceso para una declaración de propiedad abstracta no proporciona ninguna implementación real, su *accessor_body* consiste simplemente en un punto y coma.

Una declaración de propiedad que incluya tanto el `abstract` y `override` modificadores especifica que la propiedad es abstracta y reemplaza una propiedad de base. Los descriptores de acceso de esa propiedad también son abstractas.

Solo se permiten declaraciones de propiedad abstracta en clases abstractas ([clases abstractas](classes.md#abstract-classes)). Los descriptores de acceso de una propiedad virtual heredada se pueden invalidar en una clase derivada incluyendo una declaración de propiedad que especifica un `override` directiva. Esto se conoce como un ***reemplazar la declaración de propiedad***. Una declaración de propiedad reemplazada no declara una nueva propiedad. En su lugar, simplemente especializa las implementaciones de los descriptores de acceso de una propiedad virtual existente.

Una declaración de propiedad reemplazada debe especificar los mismos modificadores de accesibilidad exacto, tipo y nombre que la propiedad heredada. Si la propiedad heredada tiene solo un descriptor de acceso (por ejemplo, si la propiedad heredada es de solo lectura o solo escritura), la propiedad de reemplazo debe incluir sólo ese descriptor de acceso. Si la propiedad heredada incluye ambos descriptores de acceso (es decir, si la propiedad heredada es de lectura y escritura), la propiedad de reemplazo puede incluir un descriptor de acceso o ambos descriptores de acceso.

Puede incluir una declaración de propiedad reemplazada la `sealed` modificador. Uso de este modificador impide que una clase derivada siga reemplazando la propiedad. Los descriptores de acceso de una propiedad sellada también están sellados.

Salvo por diferencias en la declaración e invocación sintaxis, virtual, sellado, de reemplazo y abstractos descriptores de acceso se comportan exactamente igual virtual, sellado, de reemplazo y métodos abstractos. En concreto, las reglas se describen en [métodos virtuales](classes.md#virtual-methods), [invalidar métodos](classes.md#override-methods), [sellado métodos](classes.md#sealed-methods), y [métodos abstractos](classes.md#abstract-methods) aplicar como si los descriptores de acceso fueran métodos de la forma correspondiente:

*  Un `get` descriptor de acceso corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad y los mismos modificadores que la propiedad contenedora.
*  Un `set` descriptor de acceso corresponde a un método con un parámetro de valor único del tipo de propiedad, un `void` devolver el tipo y los mismos modificadores que la propiedad contenedora.

En el ejemplo
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
`X` es una propiedad de solo lectura virtual `Y` es una propiedad de lectura y escritura virtual, y `Z` es una propiedad abstracta de lectura y escritura. Dado que `Z` es abstracto, la clase contenedora `A` debe declararse como abstracta.

Una clase que derive de `A` es muestra a continuación:
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

Aquí, las declaraciones de `X`, `Y`, y `Z` invalida las declaraciones de propiedad. Cada declaración de propiedad coincide exactamente con los modificadores de accesibilidad, tipo y nombre de la propiedad heredada correspondiente. El `get` descriptor de acceso de `X` y `set` descriptor de acceso de `Y` utilizar el `base` palabra clave para tener acceso a los descriptores de acceso heredadas. La declaración de `Z` reemplaza ambos descriptores de acceso, por lo tanto, no hay ningún miembro de función abstracta pendientes en `B`, y `B` puede ser una clase no abstracta.

Cuando se declara una propiedad como un `override`, los descriptores de acceso invalidados deben ser accesibles para el código de invalidación. Además, la accesibilidad declarada de la propiedad o el indexador y de los descriptores de acceso, debe coincidir con el miembro reemplazado y descriptores de acceso. Por ejemplo:
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a>Eventos

Un ***eventos*** es un miembro que permite a un objeto o clase para proporcionar notificaciones. Los clientes pueden adjuntar código ejecutable para eventos proporcionando ***controladores de eventos***.

Los eventos se declaran mediante *event_declaration*s:

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

Un *event_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso ](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` ([Invalidar métodos](classes.md#override-methods)), `sealed` ([sellado métodos](classes.md#sealed-methods)), `abstract` ([métodos abstractos](classes.md#abstract-methods)), y `extern` ([Métodos externos](classes.md#external-methods)) modificadores.

Declaraciones de eventos están sujetos a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores.

El *tipo* de un evento debe especificarse un *delegate_type* ([hacen referencia a tipos](types.md#reference-types)) y que *delegate_type* debe ser al menos como accesible como el propio evento ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

Puede incluir una declaración de evento *event_accessor_declarations*. Sin embargo, si no es así, para que no son externos y no abstractas eventos, el compilador proporciona automáticamente ([eventos como campos](classes.md#field-like-events)); para los eventos externos, los descriptores de acceso se proporcionan externamente.

Una declaración de evento que omite *event_accessor_declarations* define uno o varios eventos: uno para cada uno de los *variable_declarator*s. Los atributos y modificadores se aplican a todos los miembros que se declara como un *event_declaration*.

Es un error en tiempo de compilación para un *event_declaration* para incluir tanto la `abstract` modificador y delimitado por llaves *event_accessor_declarations*.

Cuando una declaración de evento incluye un `extern` modificador, el evento se dice que un ***evento externo***. Dado que una declaración de evento externo no proporciona ninguna implementación real, es un error para que incluya tanto el `extern` modificador y *event_accessor_declarations*.

Es un error en tiempo de compilación para un *variable_declarator* de una declaración de evento con un `abstract` o `external` modificador debe incluir un *variable_initializer*.

Un evento puede utilizarse como operando izquierdo de la `+=` y `-=` operadores ([asignación de evento](expressions.md#event-assignment)). Estos operadores se utilizan, respectivamente, para adjuntar controladores de eventos a o quitar controladores de eventos de un evento, y los modificadores de acceso del evento controlan los contextos en los que se permiten estas operaciones.

Puesto que `+=` y `-=` son las únicas operaciones permitidas en un evento fuera del tipo que declara el evento, el código externo pueden agregar y quitar controladores de evento, pero no en ninguna otra forma de obtener o modificar la lista subyacente del evento controladores.

En una operación del formulario `x += y` o `x -= y`, cuando `x` es un evento y la referencia tiene lugar fuera del tipo que contiene la declaración de `x`, el resultado de la operación tiene el tipo `void` (en lugar de tener el tipo de `x`, con el valor de `x` después de la asignación). Esta regla evita que código externo examine indirectamente el delegado subyacente de un evento.

El ejemplo siguiente muestra cómo se adjuntan los controladores de eventos a instancias de la `Button` clase:
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

En este caso, el `LoginDialog` constructor de instancia, crea dos `Button` instancias y adjunta los controladores de eventos para el `Click` eventos.

### <a name="field-like-events"></a>Eventos como si fueran campos

En el texto de programa de la clase o estructura que contiene la declaración de un evento, ciertos eventos pueden usarse como campos. Para su uso en este modo, no debe ser un evento `abstract` o `extern`y no debe incluir explícitamente *event_accessor_declarations*. Este evento puede utilizarse en cualquier contexto que permite que un campo. El campo contiene un delegado ([delegados](delegates.md)) que hace referencia a la lista de controladores de eventos que se han agregado al evento. Si no hay controladores de eventos se han agregado, el campo contiene `null`.

En el ejemplo
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
`Click` se utiliza como un campo dentro de la `Button` clase. Como se muestra en el ejemplo, el campo puede examinar, modificar y utilizado en expresiones de invocación del delegado. El `OnClick` método en el `Button` clase "genera" el `Click` eventos. La noción de generar un evento es equivalente exactamente a invocar el delegado representado por el evento; por lo tanto, no hay ninguna construcción especial de lenguaje para generar eventos. Tenga en cuenta que la invocación del delegado está precedida por una comprobación que garantiza que el delegado no es null.

Fuera de la declaración de la `Button` (clase), el `Click` miembro solo puede usarse en el lado izquierdo de la `+=` y `-=` operadores, como en
```csharp
b.Click += new EventHandler(...);
```
que anexa un delegado a la lista de invocación de la `Click` eventos, y
```csharp
b.Click -= new EventHandler(...);
```
que quita un delegado de la lista de invocaciones del `Click` eventos.

Cuando se compila un campo como un evento, el compilador crea el almacenamiento para almacenar al delegado y crea los descriptores de acceso para el evento que agregan o quitan controladores de eventos para el campo de delegado. Las operaciones de adición y eliminación son subprocesos y puede (pero no es necesarias) se haya hecho mientras mantiene el bloqueo ([la instrucción lock](statements.md#the-lock-statement)) en el objeto contenedor para un evento de instancia o el objeto de tipo ([anónimo expresiones de creación de objetos](expressions.md#anonymous-object-creation-expressions)) para un evento estático.

Por lo tanto, una instancia declaración de evento del formulario:
```csharp
class X
{
    public event D Ev;
}
```
se compilará en un valor equivalente al:
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
Dentro de la clase `X`, las referencias a `Ev` en el lado izquierdo de la `+=` y `-=` operadores ocasionar el agregar y quitar los descriptores de acceso que se debe invocar. Todas las referencias a `Ev` se compilan para hacer referencia al campo oculto `__Ev` en su lugar ([acceso a miembros](expressions.md#member-access)). El nombre "`__Ev`" es arbitrario; el campo oculto no puede tener cualquier nombre o en absoluto.

### <a name="event-accessors"></a>Descriptores de acceso de un evento

Declaraciones de eventos normalmente se omiten *event_accessor_declarations*, como en el `Button` ejemplo anterior. Una situación para hacerlo es en el caso en que el costo de almacenamiento de un campo por el evento no es aceptable. En tales casos, puede incluir una clase *event_accessor_declarations* y utilizar un mecanismo privado para almacenar la lista de controladores de eventos.

El *event_accessor_declarations* de un evento, especifique las instrucciones ejecutables asociadas con la adición y eliminación de controladores de eventos.

Las declaraciones de descriptor de acceso que constan de un *add_accessor_declaration* y un *remove_accessor_declaration*. Cada declaración de descriptor de acceso está formado por el token `add` o `remove` seguido por un *bloque*. El *bloque* asociado con un *add_accessor_declaration* especifica las instrucciones que se ejecutará cuando se agrega un controlador de eventos y el *bloque* asociado con un *remove_accessor_declaration* especifica las instrucciones que se ejecutará cuando se quita un controlador de eventos.

Cada *add_accessor_declaration* y *remove_accessor_declaration* corresponde a un método con un parámetro de valor único del tipo de evento y un `void` tipo de valor devuelto. El parámetro implícito de un descriptor de acceso de eventos se denomina `value`. Cuando se usa un evento en una asignación de evento, se utiliza el descriptor de acceso de eventos adecuado. En concreto, si el operador de asignación es `+=` , a continuación, se utiliza el descriptor de acceso add y si el operador de asignación es `-=` , a continuación, se utiliza el descriptor de acceso remove. En cualquier caso, el operando derecho del operador de asignación se usa como argumento para el descriptor de acceso de eventos. El bloque de un *add_accessor_declaration* o un *remove_accessor_declaration* debe cumplir las reglas de `void` métodos descritos en [cuerpo del método](classes.md#method-body). En concreto, `return` no se permiten las instrucciones de dicho bloque al especificar una expresión.

Puesto que un descriptor de acceso de eventos tiene implícitamente un parámetro denominado `value`, es un error en tiempo de compilación para una variable o constante local declarados en un descriptor de acceso de eventos para que ese nombre.

En el ejemplo
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
la `Control` clase implementa un mecanismo de almacenamiento interno para los eventos. El `AddEventHandler` método asocia un valor de delegado con una clave, el `GetEventHandler` método devuelve el delegado asociado actualmente a una clave y el `RemoveEventHandler` método quita un delegado como un controlador de eventos para el evento especificado. Presumiblemente, el mecanismo de almacenamiento subyacente está diseñado, como que no hay ningún costo para asociar un `null` delegado de valor con una clave y, por tanto, los eventos no controlados no consumen almacenamiento.

### <a name="static-and-instance-events"></a>Eventos estáticos y de instancia

Cuando una declaración de evento incluye un `static` modificador, el evento se dice que un ***evento estático***. Cuando no hay ninguna `static` modificador está presente, el evento se dice que un ***eventos de instancia***.

Un evento estático no está asociado a una instancia concreta, y es un error en tiempo de compilación para hacer referencia a `this` en los descriptores de acceso de un evento estático.

Un evento de instancia está asociado con una instancia determinada de una clase, y puede tener acceso a esta instancia como `this` ([este acceso](expressions.md#this-access)) en los descriptores de acceso de ese evento.

Cuando se hace referencia a un evento en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`si `M` es un evento estático, `E` debe denotar un tipo que contiene `M`y si `M` es un evento de instancia, E debe denotar una instancia de un tipo que contenga `M`.

Las diferencias entre estático y los miembros de instancia se tratan con más detalle en [miembros estáticos y de instancia](classes.md#static-and-instance-members).

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Virtual, sellado, de reemplazo y descriptores de acceso de un evento abstracto

Un `virtual` declaración de evento especifica que los descriptores de acceso de ese evento son virtuales. El `virtual` modificador se aplica a ambos descriptores de acceso de un evento.

Un `abstract` declaración de evento especifica que los descriptores de acceso del evento son virtuales, pero no proporciona una implementación real de los descriptores de acceso. En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación para los descriptores de acceso invalidando el evento. Dado que una declaración de evento abstracto no proporciona una implementación, no puede proporcionar delimitado por llaves *event_accessor_declarations*.

Una declaración de evento que incluye tanto el `abstract` y `override` modificadores especifica que el evento es abstracto y reemplaza un evento base. Los descriptores de acceso de este evento también son abstractas.

Solo se permiten declaraciones de eventos abstractos en clases abstractas ([clases abstractas](classes.md#abstract-classes)).

Los descriptores de acceso de un evento virtual heredado pueden invalidarse en una clase derivada incluyendo una declaración de evento que especifica un `override` modificador. Esto se conoce como un ***reemplazar la declaración de evento***. Una declaración de evento reemplazado no declara un nuevo evento. En su lugar, simplemente especializa las implementaciones de los descriptores de acceso de un evento virtual existente.

Una declaración de evento reemplazado debe especificar los mismos modificadores de accesibilidad exacto, tipo y nombre que el evento reemplazado.

Puede incluir una declaración de evento reemplazado el `sealed` modificador. Uso de este modificador impide que una clase derivada siga reemplazando el evento. Los descriptores de acceso de un evento sellado también están sellados.

Es un error en tiempo de compilación para una declaración de evento reemplazado incluir un `new` modificador.

Salvo por diferencias en la declaración e invocación sintaxis, virtual, sellado, de reemplazo y abstractos descriptores de acceso se comportan exactamente igual virtual, sellado, de reemplazo y métodos abstractos. En concreto, las reglas se describen en [métodos virtuales](classes.md#virtual-methods), [invalidar métodos](classes.md#override-methods), [sellado métodos](classes.md#sealed-methods), y [métodos abstractos](classes.md#abstract-methods) aplicar como si los descriptores de acceso fueran métodos de la forma correspondiente. Cada descriptor de acceso corresponde a un método con un parámetro de valor único del tipo de evento, un `void` devolver el tipo y los mismos modificadores que el evento que lo contiene.

## <a name="indexers"></a>Indizadores

Un ***indizador*** es un miembro que permite que un objeto que se debe indexar en la misma manera que una matriz. Los indizadores se declaran mediante *indexer_declaration*s:

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

Un *indexer_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso ](classes.md#access-modifiers)), el `new` ([el nuevo modificador](classes.md#the-new-modifier)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` ([invalidar métodos](classes.md#override-methods) ), `sealed` ([Sellado métodos](classes.md#sealed-methods)), `abstract` ([métodos abstractos](classes.md#abstract-methods)), y `extern` ([métodos externos](classes.md#external-methods)) modificadores.

Las declaraciones de indizador están sujetos a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores, con la única excepción es que el modificador "static" no se permite en una declaración de indizador.

Los modificadores `virtual`, `override`, y `abstract` se excluyen mutuamente, excepto en un caso. El `abstract` y `override` modificadores pueden usarse conjuntamente para que un indizador abstracto puede reemplazar a uno virtual.

El *tipo* declaración especifica el tipo de elemento del indizador que se incluyen en la declaración de un indizador. A menos que el indizador es una implementación de miembro de interfaz explícita, el *tipo* va seguido de la palabra clave `this`. Para obtener una implementación de miembro de interfaz explícita, el *tipo* va seguido de un *interface_type*, un "`.`" y la palabra clave `this`. A diferencia de otros miembros, los indizadores no tienen nombres definidos por el usuario.

El *formal_parameter_list* especifica los parámetros del indizador. La lista de parámetros formales de un indizador corresponde a la de un método ([parámetros del método](classes.md#method-parameters)), excepto en que se debe especificar al menos un parámetro y que la `ref` y `out` no se permiten modificadores de parámetro .

El *tipo* de un indizador y cada uno de los tipos que se hace referenciados en el *formal_parameter_list* deben ser al menos tan accesibles como el propio indexador ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

Un *indexer_body* puede constar de un ***cuerpo del descriptor de acceso*** o un ***cuerpo de expresión***. En el cuerpo de un descriptor de acceso, *accessor_declarations*, que debe incluirse en "`{`"y"`}`" tokens, declare los descriptores de acceso ([descriptores de acceso](classes.md#accessors)) de la propiedad. Los descriptores de acceso especifican las instrucciones ejecutables asociadas con la lectura y escritura de la propiedad.

Un cuerpo de expresión que consta de "`=>`" seguido de una expresión `E` y un punto y coma es igual que en el cuerpo de instrucción `{ get { return E; } }`y por lo tanto, sólo puede utilizarse para especificar indizadores de solo captador donde es el resultado de Get proporcionado por una sola expresión.

Aunque la sintaxis para tener acceso a un elemento de indizador es el mismo que para un elemento de matriz, un elemento de indizador no está clasificado como una variable. Por lo tanto, no es posible pasar un elemento de indexador como un `ref` o `out` argumento.

La lista de parámetros formales de un indizador define la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) del indizador. En concreto, la firma de un indexador consta del número y tipos de sus parámetros formales. El tipo de elemento y los nombres de los parámetros formales no forman parte de la firma de un indizador.

La firma de un indizador debe ser diferente de las firmas de todos los demás indexadores declarados en la misma clase.

Los indizadores y propiedades son muy similares en concepto, pero difieren en lo siguiente:

*  Una propiedad se identifica por su nombre, mientras que un indizador se identifica por su firma.
*  Se accede a una propiedad a través de un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)), mientras que un indizador elemento que se accede a través de un *element_access* ([acceso al indizador](expressions.md#indexer-access)).
*  Una propiedad puede ser un `static` miembro, mientras que un indizador siempre es un miembro de instancia.
*  Un `get` descriptor de acceso de una propiedad que corresponde a un método sin parámetros, mientras que un `get` descriptor de acceso de un indizador corresponde a un método con la misma lista de parámetros formales que el indizador.
*  Un `set` descriptor de acceso de una propiedad que corresponde a un método con un solo parámetro denominado `value`, mientras que un `set` descriptor de acceso de un indizador corresponde a un método con la misma lista de parámetros formales que el indizador, además de un parámetro adicional denominado `value`.
*  Es un error en tiempo de compilación para un descriptor de acceso de indexador declarar una variable local con el mismo nombre que un parámetro del indizador.
*  En una declaración de propiedad reemplazada, la propiedad heredada se accede mediante la sintaxis `base.P`, donde `P` es el nombre de propiedad. En una declaración de indexador de reemplazo, el indizador heredado se tiene acceso a la sintaxis `base[E]`, donde `E` es una lista separada por comas de expresiones.
*  No hay ningún concepto de "un indizador implementado automáticamente". Es un error tener un indizador no abstracta, no externo con descriptores de acceso de punto y coma.

Aparte de estas diferencias, todas las reglas se definen en [descriptores de acceso](classes.md#accessors) y [implementa automáticamente propiedades](classes.md#automatically-implemented-properties) se aplican a los descriptores de acceso, así como para los descriptores de acceso de propiedad.

Cuando se incluye una declaración de indexador una `extern` modificador, el indizador se dice que un ***indizador externo***. Dado que la declaración de un indizador externo no proporciona ninguna implementación real, cada uno de sus *accessor_declarations* consta de un punto y coma.

El ejemplo siguiente declara un `BitArray` clase que implementa un indizador para tener acceso a los bits individuales de la matriz de bits.
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

Una instancia de la `BitArray` clase consume mucha menos memoria que correspondiente `bool[]` (ya que cada valor de la primera ocupa un solo bit en lugar de la última de un byte), pero permite las mismas operaciones que un `bool[]`.

La siguiente `CountPrimes` clase utiliza un `BitArray` y el algoritmo de "criba" clásica para calcular el número de primos entre 1 y un máximo determinado:
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

Tenga en cuenta que la sintaxis para tener acceso a los elementos de la `BitArray` es precisamente la misma que para un `bool[]`.

El ejemplo siguiente muestra una clase de 26 * 10 cuadrícula que tiene un indizador con dos parámetros. El primer parámetro debe ser una mayúscula o minúscula en el intervalo A-z y el segundo debe ser un entero en el intervalo 0-9.

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a>Sobrecarga de indizadores

Se describen las reglas de resolución de sobrecarga de indizador en [inferencia de tipo](expressions.md#type-inference).

## <a name="operators"></a>Operadores

Un ***operador*** es un miembro que define el significado de un operador de expresión que se puede aplicar a las instancias de la clase. Los operadores se declaran mediante *operator_declaration*s:

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

Hay tres categorías de operadores sobrecargables: Operadores unarios ([operadores unarios](classes.md#unary-operators)), los operadores binarios ([operadores binarios](classes.md#binary-operators)) y los operadores de conversión ([operadores de conversión](classes.md#conversion-operators)).

El *operator_body* ya sea un punto y coma, un ***cuerpo de instrucción*** o un ***cuerpo de expresión***. Consta de un cuerpo de instrucción de un *bloque*, que especifica las instrucciones que se ejecutará cuando se invoca el operador. El *bloque* debe cumplir las reglas de devolución por valor métodos descritos en [cuerpo del método](classes.md#method-body). Un cuerpo de expresión consta de `=>` seguido de una expresión y un punto y coma y denota una expresión única para realizar cuando se invoca el operador.

Para `extern` operadores, el *operator_body* consiste simplemente en un punto y coma. Para todos los demás operadores, el *operator_body* es un cuerpo de bloques o un cuerpo de expresión.

Las siguientes reglas se aplican a todas las declaraciones de operador:

*  Una declaración de un operador debe incluir tanto un `public` y un `static` modificador.
*  Los parámetros de un operador deben ser parámetros de valor ([parámetros del valor](variables.md#value-parameters)). Es un error en tiempo de compilación para una declaración de un operador especificar `ref` o `out` parámetros.
*  La firma de un operador ([operadores unarios](classes.md#unary-operators), [operadores binarios](classes.md#binary-operators), [operadores de conversión](classes.md#conversion-operators)) deben ser diferentes de las firmas de todos los demás operadores declarados en el misma clase.
*  Todos los tipos que se hace referencia en una declaración de un operador deben ser al menos tan accesibles como el propio operador ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).
*  Es un error para el mismo modificador aparece varias veces en una declaración de operador.

Cada categoría de operador impone restricciones adicionales, como se describe en las secciones siguientes.

Al igual que otros miembros, las clases derivadas heredan los operadores declarados en una clase base. Dado que las declaraciones de operador siempre requieren la clase o estructura en la que se declara el operador para participar en la firma del operador, no es posible que un operador declarado en una clase derivada ocultar un operador declarado en una clase base. Por lo tanto, el `new` modificador nunca es necesaria y, por lo tanto, no se permite en una declaración de operador.

Encontrará información adicional sobre los operadores unarios y binarios en [operadores](expressions.md#operators).

Se puede encontrar información adicional sobre operadores de conversión en [conversiones definidas por el usuario](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Operadores unarios

Las reglas siguientes se aplican a las declaraciones de operador unario, donde `T` denota el tipo de instancia de la clase o estructura que contiene la declaración del operador:

*  Unario `+`, `-`, `!`, o `~` operador debe tomar un único parámetro de tipo `T` o `T?` y puede devolver cualquier tipo.
*  Unario `++` o `--` operador debe tomar un único parámetro de tipo `T` o `T?` y debe devolver que mismo tipo o un tipo derivado de éste.
*  Unario `true` o `false` operador debe tomar un único parámetro de tipo `T` o `T?` y debe devolver un tipo `bool`.

La firma de un operador unario consiste en el símbolo de operador (`+`, `-`, `!`, `~`, `++`, `--`, `true`, o `false`) y el tipo de parámetro formal. El tipo de valor devuelto no es parte de la firma de un operador unario, ni es el nombre del parámetro formal.

El `true` y `false` operadores unarios requieren declaración par a par. Si una clase declara uno de estos operadores sin declarar el otro, se produce un error de tiempo de compilación. El `true` y `false` operadores se describen más detalladamente en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators) y [expresiones booleanas](expressions.md#boolean-expressions).

El ejemplo siguiente muestra una implementación y el uso posterior de `operator ++` para una clase de vector de enteros:
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

Tenga en cuenta cómo el método de operador devuelve el valor producido por sumar 1 al operando, al igual que el incremento de postfijo y operadores de decremento ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)) y el prefijo de incremento y decremento operadores ([prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)). A diferencia de en C++, este método necesita no modifica el valor de su operando directamente. De hecho, modificar el valor del operando infringiría la semántica estándar del operador de incremento de postfijo.

### <a name="binary-operators"></a>Operadores binarios

Las reglas siguientes se aplican a las declaraciones de operador binario, donde `T` denota el tipo de instancia de la clase o estructura que contiene la declaración del operador:

*  Un operador de desplazamiento que no sea binario debe tomar dos parámetros, al menos uno de los cuales debe tener tipo `T` o `T?`y puede devolver cualquier tipo.
*  Un archivo binario `<<` o `>>` operador debe tomar dos parámetros, el primero de los cuales debe tener tipo `T` o `T?` y el segundo de los cuales debe tener tipo `int` o `int?`y puede devolver cualquier tipo.

La firma de un operador binario consiste en el símbolo de operador (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, o `<=`) y los tipos de los dos parámetros formales. El tipo de valor devuelto y los nombres de los parámetros formales no forman parte de la firma de un operador binario.

Algunos operadores binarios requieren declaración par a par. Para todas las declaraciones de cualquier operador de un par, debe haber una declaración del operador del par coincidente. Coincide con dos declaraciones de operador cuando tienen el mismo tipo de valor devuelto y el mismo tipo para cada parámetro. Los siguientes operadores requieren declaración por pares:

*  `operator ==` y `operator !=`
*  `operator >` y `operator <`
*  `operator >=` y `operator <=`

### <a name="conversion-operators"></a>Operadores de conversión

Una declaración de operador de conversión introduce un ***conversión definida por el usuario*** ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)) que aumenta las conversiones explícitas e implícitas predefinidas.

Una declaración de operador de conversión que incluye la `implicit` palabra clave define una conversión implícita de definido por el usuario. Conversiones implícitas pueden ocurrir en una variedad de situaciones, incluidas las llamadas a funciones miembro, expresiones de conversión y asignaciones. Esto se describe más detalladamente en [conversiones implícitas](conversions.md#implicit-conversions).

Una declaración de operador de conversión que incluye la `explicit` palabra clave define una conversión explícita de definido por el usuario. Las conversiones explícitas pueden producir en expresiones de conversión y se describe más detalladamente en [las conversiones explícitas](conversions.md#explicit-conversions).

Un operador de conversión convierte de un tipo de origen, indicado por el tipo de parámetro del operador de conversión a un tipo de destino indicado por el tipo de valor devuelto del operador de conversión.

Para un tipo de origen dado `S` y tipo de destino `T`si `S` o `T` son tipos que aceptan valores NULL, permiten `S0` y `T0` hacen referencia a sus tipos subyacentes, de lo contrario `S0` y `T0` son igual que `S` y `T` respectivamente. Una clase o struct se puede declarar una conversión de un tipo de origen `S` a un tipo de destino `T` sólo si se cumplen todas las opciones siguientes:

*  `S0` y `T0` son tipos diferentes.
*  Ya sea `S0` o `T0` es el tipo de clase o estructura en la que realiza la declaración del operador.
*  Ni `S0` ni `T0` es un *interface_type*.
*  Excluyendo las conversiones definidas por el usuario, no existe una conversión desde `S` a `T` o desde `T` a `S`.

Para los fines de estas reglas, cualquier tipo de parámetros asociados con `S` o `T` se consideran tipos únicos que tienen ninguna relación de herencia con otros tipos y todas las restricciones de tipo de esos parámetros se omiten.

En el ejemplo
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
se permiten las declaraciones de dos operador primera porque, para los fines de [indizadores](classes.md#indexers).3, `T` y `int` y `string` respectivamente se consideran tipos únicos sin ninguna relación. Sin embargo, el tercer operador es un error porque `C<T>` es la clase base de `D<T>`.

Desde la segunda regla que sigue un operador de conversión debe convertir hacia o desde el tipo de clase o estructura en la que se declara el operador. Por ejemplo, es posible que un tipo de clase o struct `C` para definir una conversión de `C` a `int` y desde `int` a `C`, pero no desde `int` a `bool`.

No es posible volver a definir directamente una conversión predefinida. Por lo tanto, no se permiten los operadores de conversión para convertir de o a `object` porque ya existen conversiones implícitas y explícitas entre `object` y todos los demás tipos. Del mismo modo, ni el origen ni los tipos de destino de una conversión pueden ser un tipo base del otro, ya que entonces ya existiría una conversión.

Sin embargo, es posible declarar los operadores en tipos genéricos que, para los argumentos de tipo determinado, especifican las conversiones que ya existen como conversiones predefinidas. En el ejemplo
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
Cuando escriba `object` se especifica como un argumento de tipo para `T`, el segundo operador declara una conversión que ya existe (implícita y por lo tanto también explícito, no existe conversión de cualquier tipo `object`).

En casos donde no existe una conversión predefinida entre dos tipos, se omiten cualquier conversiones definidas por el usuario entre esos tipos. De manera específica:

*  Si una conversión implícita predefinida ([conversiones implícitas](conversions.md#implicit-conversions)) existe desde tipo `S` al tipo `T`, todos los definidos por el usuario conversiones (implícitas o explícitas) de `S` a `T` se omiten.
*  Si una conversión explícita predefinida ([las conversiones explícitas](conversions.md#explicit-conversions)) existe desde tipo `S` al tipo `T`, las conversiones explícitas definidas por el usuario de `S` a `T` se omiten. Furthermore:

Si `T` es un tipo de interfaz definido por el usuario conversiones implícitas de `S` a `T` se omiten.

De lo contrario, definido por el usuario conversiones implícitas de `S` a `T` todavía se consideran.

Para todos los tipos, pero `object`, los operadores declarados por el `Convertible<T>` tipo anterior no entren en conflicto con las conversiones predefinidas. Por ejemplo:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Sin embargo, para el tipo `object`, conversiones predefinidas ocultar las conversiones definidas por el usuario en todos los casos, pero uno:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

No se permiten conversiones definidas por el usuario para convertir de o a *interface_type*s. En concreto, esta restricción garantiza que ninguna transformación definido por el usuario se produce cuando se convierte en un *interface_type*y que una conversión a un *interface_type* se realizará correctamente solo si el objeto que se está convirtiendo implementa realmente especificado *interface_type*.

La firma de un operador de conversión está formada por el tipo de origen y el tipo de destino. (Tenga en cuenta que esta es la única forma de miembro para el que el tipo de valor devuelto participa en la firma.) El `implicit` o `explicit` clasificación de un operador de conversión no forma parte de la firma del operador. Por lo tanto, una clase o struct no puede declarar tanto un `implicit` y un `explicit` operador de conversión con los mismos tipos de origen y de destino.

En general, las conversiones implícitas definido por el usuario deben diseñarse para nunca produzcan excepciones ni pérdida de información. Si una conversión definida por el usuario puede dar lugar a excepciones (por ejemplo, porque el argumento de origen está fuera del intervalo) o pérdida de información (por ejemplo, descartar los bits de orden superior), dicha conversión debe definirse como una conversión explícita.

En el ejemplo
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
la conversión de `Digit` a `byte` es implícita porque nunca produce excepciones o pierde información, pero la conversión de `byte` a `Digit` es explícito desde `Digit` sólo se puede representar un subconjunto de los posibles los valores de un `byte`.

## <a name="instance-constructors"></a>Constructores de instancias

Un ***constructor de instancia*** es un miembro que implementa las acciones necesarias para inicializar una instancia de una clase. Constructores de instancia se declaran mediante *constructor_declaration*s:

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

Un *constructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso ](classes.md#access-modifiers)) y un `extern` ([métodos externos](classes.md#external-methods)) modificador. No se permite una declaración de constructor para incluir el mismo modificador varias veces.

El *identificador* de un *constructor_declarator* debe nombrar la clase en el que se declara el constructor de instancia. Si se especifica ningún otro nombre, se produce un error en tiempo de compilación.

El elemento opcional *formal_parameter_list* de una instancia del constructor está sujeto a las mismas reglas que el *formal_parameter_list* de un método ([métodos](classes.md#methods)). La lista de parámetros formales define la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un constructor de instancia y se controla mediante el cual el proceso de resolución de sobrecarga ([inferencia](expressions.md#type-inference)) selecciona un determinado constructor de instancia en una invocación.

Cada uno de los tipos que se hace referenciados en el *formal_parameter_list* de una instancia del constructor debe ser al menos tan accesible como el propio constructor ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

El elemento opcional *constructor_initializer* especifica otro constructor de instancia para invocar antes de ejecutar las instrucciones que aparecen la *constructor_body* de este constructor de instancia. Esto se describe más detalladamente en [inicializadores de Constructor](classes.md#constructor-initializers).

Cuando una declaración de constructor incluye un `extern` modificador, el constructor se dice que un ***constructor externo***. Dado que una declaración de constructor externo no proporciona ninguna implementación real, su *constructor_body* consta de un punto y coma. Para todos los demás constructores, el *constructor_body* consta de un *bloque* que especifica las instrucciones para inicializar una nueva instancia de la clase. Esto corresponde exactamente a la *bloque* de un método de instancia con un `void` tipo de valor devuelto ([cuerpo del método](classes.md#method-body)).

No se heredan los constructores de instancia. Por lo tanto, una clase no tiene ningún constructor de instancia distinto de aquéllos declarados realmente en la clase. Si una clase no contiene ninguna declaración de constructor de instancia, automáticamente se proporciona un constructor de instancia predeterminado ([constructores predeterminados](classes.md#default-constructors)).

Los constructores de instancia se invocan mediante *object_creation_expression*s ([expresiones de creación de objetos](expressions.md#object-creation-expressions)) y *constructor_initializer*s.

### <a name="constructor-initializers"></a>Inicializadores del constructor

Todos los constructores de instancias (los de la clase excepto `object`) incluyen implícitamente una invocación de otro constructor de instancia inmediatamente antes del *constructor_body*. El constructor debe invocar implícitamente viene determinada por la *constructor_initializer*:

*  Un inicializador de constructor de instancia del formulario `base(argument_list)` o `base()` hace que un constructor de instancia de la clase base directa. Este constructor se selecciona mediante *argument_list* si existe y las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution). El conjunto de constructores de instancias de candidato consta de todos los constructores de instancia accesible contenidos en la clase base directa o el constructor predeterminado ([constructores predeterminados](classes.md#default-constructors)), si se declara ningún constructor de instancia en el clase base directa. Si este conjunto está vacío, o si no se puede identificar un mejor constructor de instancia, se produce un error en tiempo de compilación.
*  Un inicializador de constructor de instancia del formulario `this(argument-list)` o `this()` hace que un constructor de instancia de la clase en sí que se debe invocar. El constructor se selecciona utilizando *argument_list* si existe y las reglas de resolución de sobrecarga de [de resolución de sobrecarga](expressions.md#overload-resolution). El conjunto de constructores de instancias de candidato consta de todos los constructores de instancia accesible declarados en la propia clase. Si este conjunto está vacío, o si no se puede identificar un mejor constructor de instancia, se produce un error en tiempo de compilación. Si una declaración de constructor de instancia incluye a un inicializador de constructor que invoca el constructor en Sí, se produce un error en tiempo de compilación.

Si un constructor de instancia no tiene ningún inicializador de constructor, un inicializador de constructor del formulario `base()` se proporciona implícitamente. Por lo tanto, una declaración de constructor de instancia del formulario
```csharp
C(...) {...}
```
equivale exactamente a
```csharp
C(...): base() {...}
```

El ámbito de los parámetros proporcionados por el *formal_parameter_list* de un constructor de instancia declaración incluye el inicializador de constructor de dicha declaración. Por lo tanto, se permite un inicializador de constructor para tener acceso a los parámetros del constructor. Por ejemplo:
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

Un inicializador de constructor de instancia no puede tener acceso a la instancia que se está creando. Por lo tanto, es un error en tiempo de compilación para hacer referencia a `this` en una expresión de argumento del inicializador del constructor, como es un error en tiempo de compilación para una expresión de argumento hacer referencia a cualquier miembro de instancia a través de un *simple_name*.

### <a name="instance-variable-initializers"></a>Inicializadores de variables de instancia

Cuando un constructor de instancia no tiene ningún inicializador de constructor, o tiene un inicializador de constructor del formulario `base(...)`, implícitamente el constructor realiza las inicializaciones especificadas por el *variable_initializer*s de los campos de instancia declaran en su clase. Esto corresponde a una secuencia de asignaciones que se ejecutan inmediatamente al entrar en el constructor y antes de la invocación del constructor de clase base directa implícita. Los inicializadores de variable se ejecutan en el orden textual en que aparecen en la declaración de clase.

### <a name="constructor-execution"></a>Ejecución de un constructor

Inicializadores de variables se transforman en instrucciones de asignación y se ejecutan estas instrucciones de asignación antes de la invocación del constructor de instancia de clase base. Este orden garantiza que todos los campos de instancia se inicializan por sus inicializadores de variable antes de que se ejecutan las instrucciones que tienen acceso a esa instancia.

Dado el ejemplo:
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
Cuando `new B()` se utiliza para crear una instancia de `B`, se produce el siguiente resultado:
```
x = 1, y = 0
```

El valor de `x` es 1 porque el inicializador de variable se ejecuta antes de invoca el constructor de instancia de la clase base. Sin embargo, el valor de `y` es 0 (el valor predeterminado de un `int`) porque la asignación a `y` no se ejecuta hasta que el constructor de clase base devuelve.

Resulta útil pensar en los inicializadores de variables de instancia y los inicializadores de constructor como instrucciones que se insertan automáticamente antes de la *constructor_body*. El ejemplo
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
contiene a varios inicializadores de variables; También contiene los inicializadores de constructor de ambas formas (`base` y `this`). El ejemplo se corresponde con el código se muestra a continuación, donde cada comentario indica una instrucción que se inserta automáticamente (la sintaxis utilizada para invocar el constructor insertado automáticamente no es válido, pero sirve para ilustrar el mecanismo).

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a>Constructores predeterminados

Si una clase no contiene ninguna declaración de constructor de instancia, se proporciona automáticamente un constructor de instancia predeterminado. El constructor predeterminado simplemente invoca el constructor sin parámetros de la clase base directa. Si la clase es abstracta está protegida la accesibilidad declarada para el constructor predeterminado. En caso contrario, la accesibilidad declarada para el constructor predeterminado es pública. Por lo tanto, es siempre el constructor predeterminado del formulario

```csharp
protected C(): base() {}
```
o
```csharp
public C(): base() {}
```
donde `C` es el nombre de la clase. Si la resolución de sobrecarga no puede determinar a un único mejores candidatos para el inicializador de constructor de clase base, a continuación, se produce un error en tiempo de compilación.

En el ejemplo
```csharp
class Message
{
    object sender;
    string text;
}
```
se proporciona un constructor predeterminado porque la clase no contiene ninguna declaración de constructor de instancia. Por lo tanto, el ejemplo es equivalente a
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Constructores privados

Cuando una clase `T` declara sólo los constructores de instancia privados, no es posible para las clases fuera el texto del programa `T` derivar `T` o crear directamente instancias de `T`. Por lo tanto, si una clase contiene a sólo miembros estáticos y no está diseñada para ejecutarse, agregar un constructor de instancia privada vacía impedirá creación de instancias. Por ejemplo:
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

La `Trig` clase grupos de constantes y métodos relacionados, pero no está pensada para ejecutarse. Por lo tanto, declara un constructor de instancia única de privado vacío. Debe declararse al menos un constructor de instancia para suprimir la generación automática de un constructor predeterminado.

### <a name="optional-instance-constructor-parameters"></a>Parámetros del constructor de instancia opcional

El `this(...)` forma de inicializador de constructor se suele utilizar junto con la sobrecarga para implementar los parámetros del constructor de instancia opcional. En el ejemplo
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
los primeros constructores de instancia de dos simplemente proporcionan los valores predeterminados para los argumentos que faltan. Ambos usan un `this(...)` inicializador de constructor para invocar el tercer constructor de instancia, lo que realmente se encarga de inicializar la nueva instancia. El efecto es el de los parámetros del constructor opcional:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Constructores estáticos

Un ***constructor estático*** es un miembro que implementa las acciones necesarias para inicializar un tipo de clase cerrado. Los constructores estáticos se declaran mediante *static_constructor_declaration*s:

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

Un *static_constructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y un `extern` modificador ([métodos externos](classes.md#external-methods)).

El *identificador* de un *static_constructor_declaration* debe nombrar la clase en el que se declara el constructor estático. Si se especifica ningún otro nombre, se produce un error en tiempo de compilación.

Cuando una declaración de constructor estático incluye un `extern` modificador, el constructor estático se dice que un ***constructor estático***. Dado que una declaración de constructor estático no proporciona ninguna implementación real, su *static_constructor_body* consta de un punto y coma. Para todas las declaraciones de constructor estático, el *static_constructor_body* consta de un *bloque* que especifica las instrucciones que se ejecutarán para inicializar la clase. Esto corresponde exactamente a la *cuerpoMétodo* de un método estático con un `void` tipo de valor devuelto ([cuerpo del método](classes.md#method-body)).

Los constructores estáticos no se heredan y no se puede llamar directamente.

El constructor estático de un tipo de clase cerrado como máximo una vez se ejecuta en un dominio de aplicación determinado. El primero de los siguientes eventos que se producen dentro de un dominio de aplicación, se desencadena la ejecución de un constructor estático:

*  Se crea una instancia del tipo de clase.
*  Se hace referencia a cualquiera de los miembros estáticos del tipo de clase.

Si una clase contiene la `Main` método ([inicio de la aplicación](basic-concepts.md#application-startup)) en que comienza la ejecución, el constructor estático para esa clase se ejecuta antes que la `Main` se llama al método.

Para inicializar un nuevo tipo de clase cerrados, en primer lugar un nuevo conjunto de campos estáticos ([campos estáticos y de instancia](classes.md#static-and-instance-fields)) para ese tipo cerrado determinado se ha creado. Cada uno de los campos estáticos se inicializa en su valor predeterminado ([los valores predeterminados](variables.md#default-values)). A continuación, los inicializadores de campo estático ([inicialización de campos estáticos](classes.md#static-field-initialization)) se ejecutan para esos campos estáticos. Por último, se ejecuta el constructor estático.

El ejemplo
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
el resultado debe ser:
```
Init A
A.F
Init B
B.F
```
Dado que la ejecución de `A`del constructor estático se desencadena por la llamada a `A.F`como la ejecución de `B`del constructor estático se desencadena por la llamada a `B.F`.

Es posible construir dependencias circulares que permiten a los campos estáticos con inicializadores de variables que se deben observar en su estado de valor predeterminado.

El ejemplo
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
genera el resultado
```
X = 1, Y = 2
```

Para ejecutar el `Main` método, el sistema ejecuta por primera vez el inicializador para `B.Y`, antes de la clase `B`del constructor estático. `Y`del inicializador hace `A`del constructor estático para que se puede ejecutar porque el valor de `A.X` se hace referencia. El constructor estático de `A` a su vez va a calcular el valor de `X`y hacer búsquedas por lo que el valor predeterminado de `Y`, que es cero. `A.X` por lo tanto se inicializa a 1. El proceso de ejecución `A`inicializadores de campo estático de constructor estático y, a continuación, finaliza, volviendo al cálculo del valor inicial de `Y`, el resultado se convierte en 2.

Dado que el constructor estático se ejecuta exactamente una vez para cada tipo de clase construida de cerrado, es un lugar conveniente para exigir comprobaciones de tiempo de ejecución en el parámetro de tipo que no se puede comprobar en tiempo de compilación a través de restricciones ([parámetro de tipo restricciones](classes.md#type-parameter-constraints)). Por ejemplo, el siguiente tipo utiliza un constructor estático para exigir que el argumento de tipo es una enumeración:
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a>Destructores

Un ***destructor*** es un miembro que implementa las acciones necesarias para destruir una instancia de una clase. Se declara un destructor con un *destructor_declaration*:

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

Un *destructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)).

El *identificador* de un *destructor_declaration* debe nombrar la clase en el que se declara el destructor. Si se especifica ningún otro nombre, se produce un error en tiempo de compilación.

Cuando una declaración de destructor incluye un `extern` modificador, el destructor se dice que un ***destructor externo***. Dado que una declaración de destructor externo no proporciona ninguna implementación real, su *destructor_body* consta de un punto y coma. Para el resto de los destructores, la *destructor_body* consta de un *bloque* que especifica las instrucciones que se ejecutarán para destruir una instancia de la clase. Un *destructor_body* corresponde exactamente a la *cuerpoMétodo* de un método de instancia con un `void` tipo de valor devuelto ([cuerpo del método](classes.md#method-body)).

No se heredan los destructores. Por lo tanto, una clase tiene el destructor que se puede declarar en esa clase.

Puesto que un destructor debe no tener ningún parámetro, no puede sobrecargarse, por lo que puede tener una clase, como máximo, un destructor.

Los destructores se invocan automáticamente y no se puede invocar explícitamente. Una instancia se vuelve apta para su destrucción cuando ya no es posible que cualquier código para que use esa instancia. La ejecución del destructor para la instancia puede producirse en cualquier momento después de la instancia se vuelve apta para su destrucción. Cuando se destruye una instancia, se llaman a los destructores de cadena de herencia de la instancia en orden, de la más derivada a la menos derivada. Un destructor se puede ejecutar en cualquier subproceso. Para obtener más información sobre las reglas que rigen cuándo y cómo se ejecuta un destructor, vea [administración de memoria automática](basic-concepts.md#automatic-memory-management).

La salida del ejemplo
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
is
```
B's destructor
A's destructor
```
Puesto que los destructores de una cadena de herencia se llaman en orden, de la más derivada a la menos derivada.

Los destructores se implementan mediante la invalidación del método virtual `Finalize` en `System.Object`. Programas de C# no pueden invalidar este método o llame al (o invalidaciones del mismo) directamente. Por ejemplo, el programa
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
contiene dos errores.

El compilador se comporta como si este método y reemplazos, no existen en absoluto. Por lo tanto, este programa:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
es válido, y muestra el método oculta `System.Object`del `Finalize` método.

Para obtener una explicación del comportamiento cuando se produce una excepción desde un destructor, vea [cómo se controlan las excepciones](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Iterators

Un miembro de función ([miembros de función](expressions.md#function-members)) implementa mediante un bloque de iteradores ([bloques](statements.md#blocks)) se denomina un ***iterador***.

Un bloque de iteradores puede utilizarse como el cuerpo de un miembro de función como el tipo de valor devuelto del miembro de función correspondiente es una de las interfaces de enumerador ([interfaces de enumerador](classes.md#enumerator-interfaces)) o una de las interfaces enumerables ([Interfaces enumerables](classes.md#enumerable-interfaces)). Se puede producir como un *cuerpoMétodo*, *operator_body* o *accessor_body*, mientras que los eventos, constructores de instancias, los constructores estáticos y destructores no pueden ser se implementa como iteradores.

Cuando un miembro de función se implementa mediante un bloque de iteradores, es un error en tiempo de compilación para la lista de parámetros formales del miembro de función para especificar cualquier `ref` o `out` parámetros.

### <a name="enumerator-interfaces"></a>Interfaces de enumerador

El ***interfaces de enumerador*** son la interfaz no genérica `System.Collections.IEnumerator` y todas las instancias de la interfaz genérica `System.Collections.Generic.IEnumerator<T>`. Para mayor brevedad, en este capítulo estas interfaces se denominarán `IEnumerator` y `IEnumerator<T>`, respectivamente.

### <a name="enumerable-interfaces"></a>Interfaces enumerables

El ***interfaces enumerables*** son la interfaz no genérica `System.Collections.IEnumerable` y todas las instancias de la interfaz genérica `System.Collections.Generic.IEnumerable<T>`. Para mayor brevedad, en este capítulo estas interfaces se denominarán `IEnumerable` y `IEnumerable<T>`, respectivamente.

### <a name="yield-type"></a>Tipo yield

Un iterador genera una secuencia de valores, todos del mismo tipo. Este tipo se denomina el ***tipo yield*** del iterador.

*  El tipo de rendimiento de un iterador que devuelve `IEnumerator` o `IEnumerable` es `object`.
*  El tipo de rendimiento de un iterador que devuelve `IEnumerator<T>` o `IEnumerable<T>` es `T`.

### <a name="enumerator-objects"></a>Objetos del enumerador

Cuando un miembro de función devuelve un enumerador de tipo de interfaz se implementa mediante un bloque de iteradores, invocar al miembro de función no se ejecuta inmediatamente el código del bloque de iteradores. En su lugar, un ***objeto enumerador*** se crea y devuelve. Este objeto encapsula el código especificado en el bloque de iteradores y la ejecución del código del bloque de iteradores se produce cuando el objeto de enumerador `MoveNext` se invoca el método. Un objeto de enumerador tiene las siguientes características:

*  Implementa `IEnumerator` y `IEnumerator<T>`, donde `T` es el tipo yield del iterador.
*  Implementa `System.IDisposable`.
*  Se inicializa con una copia de los valores de argumento (si existe) y pasa el valor de instancia para el miembro de función.
*  Tiene cuatro estados posibles, ***antes***, ***ejecutando***, ***suspendido***, y ***después***y está inicialmente en el ***antes***  estado.

Un objeto de enumerador es normalmente una instancia de una clase de enumerador generado por el compilador que encapsula el código del bloque de iteradores e implementa las interfaces de enumerador, pero son posibles otros métodos de implementación. Si el compilador genera una clase de enumerador, esa clase se anidarán, directa o indirectamente, en la clase que contiene el miembro de función, tendrá accesibilidad privada, y tendrá un nombre reservado para uso del compilador ([identificadores ](lexical-structure.md#identifiers)).

Un objeto de enumerador puede implementar más interfaces que las especificadas anteriormente.

Las secciones siguientes describen el comportamiento exacto de la `MoveNext`, `Current`, y `Dispose` los miembros de la `IEnumerable` y `IEnumerable<T>` proporcionadas por un objeto de enumerador de implementaciones de interfaz.

Tenga en cuenta que los objetos del enumerador no admiten la `IEnumerator.Reset` método. Invocar este método hace que un `System.NotSupportedException` que se produzca.

#### <a name="the-movenext-method"></a>El método MoveNext

El `MoveNext` método de un objeto de enumerador encapsula el código de un bloque de iteradores. Invocar el `MoveNext` método ejecuta código en el bloque de iteradores y establece el `Current` propiedad del objeto enumerador según corresponda. La acción exacta realizada por `MoveNext` depende del estado del objeto del enumerador cuando `MoveNext` se invoca:

*  Si el estado del objeto del enumerador es ***antes***, al invocar `MoveNext`:
   * Cambia el estado a ***ejecutando***.
   * Inicializa los parámetros (incluido `this`) del bloque de iteradores para los valores de argumento y el valor de instancia guarda cuando se inicializó el objeto de enumerador.
   * Ejecuta el bloque de iteradores desde el principio hasta que se interrumpe la ejecución (como se describe a continuación).
*  Si el estado del objeto del enumerador es ***ejecutando***, el resultado de invocar `MoveNext` no se ha especificado.
*  Si el estado del objeto del enumerador es ***suspendido***, al invocar `MoveNext`:
   * Cambia el estado a ***ejecutando***.
   * Restaura los valores de todas las variables locales y parámetros (incluido this) a los valores guardados cuando por última vez se suspende la ejecución del bloque de iteradores. Tenga en cuenta que el contenido de todos los objetos que hacen referencia estas variables puede haber cambiado desde la anterior llamada a MoveNext.
   * Reanuda la ejecución del bloque de iteradores inmediatamente después de la `yield return` instrucción que causó la suspensión de la ejecución y continúa hasta que se interrumpe la ejecución (como se describe a continuación).
*  Si el estado del objeto del enumerador es ***después***, al invocar `MoveNext` devuelve `false`.


Cuando `MoveNext` ejecuta el bloque de iteradores, puede interrumpir la ejecución de cuatro maneras: Por un `yield return` instrucción, por un `yield break` instrucción cuando se encuentra al final del bloque de iteradores y debido a una excepción que se produce y se propaga fuera del bloque de iteradores.

*  Cuando un `yield return` se encuentra la instrucción ([la instrucción yield](statements.md#the-yield-statement)):
   * La expresión proporcionada en la instrucción se evalúa implícitamente convierte al tipo yield y asignada a la `Current` propiedad del objeto enumerador.
   * Se suspende la ejecución del cuerpo del iterador. Los valores de todas las variables locales y parámetros (incluidos `this`) se guardan, tal como es la ubicación de este `yield return` instrucción. Si el `yield return` instrucción está dentro de uno o varios `try` bloquea asociado `finally` bloques no se ejecutan en este momento.
   * Se cambia el estado del objeto del enumerador a ***suspendido***.
   * El `MoveNext` devuelve del método `true` a su llamador, que indica que la iteración avanzó correctamente al siguiente valor.
*  Cuando un `yield break` se encuentra la instrucción ([la instrucción yield](statements.md#the-yield-statement)):
   * Si el `yield break` instrucción está dentro de uno o varios `try` bloquea asociado `finally` bloques se ejecutan.
   * Se cambia el estado del objeto del enumerador a ***después***.
   * El `MoveNext` devuelve del método `false` a su llamador, que indica que la iteración está completa.
*  Cuando se encuentra el final del cuerpo del iterador:
   * Se cambia el estado del objeto del enumerador a ***después***.
   * El `MoveNext` devuelve del método `false` a su llamador, que indica que la iteración está completa.
*  Cuando se produce una excepción y se propaga fuera del bloque de iteradores:
   * Adecuado `finally` bloques en el cuerpo del iterador habrá ejecutados por la propagación de excepciones.
   * Se cambia el estado del objeto del enumerador a ***después***.
   * Continúa la propagación de excepciones al llamador de la `MoveNext` método.

#### <a name="the-current-property"></a>La propiedad actual

Un objeto de enumerador `Current` propiedad se ve afectada por `yield return` las instrucciones del bloque de iteradores.

Cuando un objeto de enumerador está en el ***suspendido*** de estado, el valor de `Current` es el valor establecido por la llamada anterior a `MoveNext`. Cuando un objeto de enumerador está en el ***antes***, ***ejecutando***, o ***después*** indica, el resultado del acceso a `Current` no se ha especificado.

Para un iterador con un rendimiento escriba distinto `object`, el resultado del acceso a `Current` a través del objeto de enumerador `IEnumerable` implementación corresponde al acceso a `Current` a través del objeto de enumerador `IEnumerator<T>` implementación y convirtiendo el resultado a `object`.

#### <a name="the-dispose-method"></a>El método Dispose

El `Dispose` método se utiliza para limpiar la iteración al traer el objeto de enumerador a la ***después*** estado.

*  Si el estado del objeto del enumerador es ***antes***, al invocar `Dispose` cambia el estado a ***después***.
*  Si el estado del objeto del enumerador es ***ejecutando***, el resultado de invocar `Dispose` no se ha especificado.
*  Si el estado del objeto del enumerador es ***suspendido***, al invocar `Dispose`:
   * Cambia el estado a ***ejecutando***.
   * Ejecute cualquiera, por último, bloques como si ejecuta la última `yield return` instrucción fuera un `yield break` instrucción. Si esto produce una excepción se produce y se propaga fuera del cuerpo del iterador, el estado del objeto de enumerador se establece en ***después*** y la excepción se propaga al llamador de la `Dispose` método.
   * Cambia el estado a ***después***.
*  Si el estado del objeto del enumerador es ***después***, al invocar `Dispose` no tiene ningún efecto.

### <a name="enumerable-objects"></a>Objetos enumerables

Cuando un miembro de función devuelve un tipo de interfaz enumerable se implementa mediante un bloque de iteradores, invocar al miembro de función no se ejecuta inmediatamente el código del bloque de iteradores. En su lugar, un ***objeto enumerable*** se crea y devuelve. El objeto enumerable `GetEnumerator` método devuelve un objeto de enumerador que encapsula el código especificado en el bloque de iteradores y la ejecución del código del bloque de iteradores se produce cuando el objeto de enumerador `MoveNext` se invoca el método. Un objeto enumerable tiene las siguientes características:

*  Implementa `IEnumerable` y `IEnumerable<T>`, donde `T` es el tipo yield del iterador.
*  Se inicializa con una copia de los valores de argumento (si existe) y pasa el valor de instancia para el miembro de función.

Un objeto enumerable normalmente es una instancia de una clase enumerable generado por el compilador que encapsula el código del bloque de iteradores e implementa las interfaces enumerables, pero son posibles otros métodos de implementación. Si el compilador genera una clase enumerable, esa clase se anidarán, directa o indirectamente, en la clase que contiene el miembro de función, tendrá accesibilidad privada, y tendrá un nombre reservado para uso del compilador ([identificadores ](lexical-structure.md#identifiers)).

Un objeto enumerable puede implementar más interfaces que las especificadas anteriormente. En concreto, también puede implementar un objeto enumerable `IEnumerator` y `IEnumerator<T>`, habilitarla para que actúe como un objeto enumerable y un enumerador. En ese tipo de implementación, la primera vez que un objeto enumerable `GetEnumerator` se invoca al método, se devuelve el mismo objeto enumerable. Las siguientes invocaciones del objeto enumerable `GetEnumerator`, si existe, devuelve una copia del objeto enumerable. Por lo tanto, cada uno devuelve enumerador tiene su propio estado y los cambios de un enumerador no afectarán a otro.

#### <a name="the-getenumerator-method"></a>El método GetEnumerator

Un objeto enumerable proporciona una implementación de la `GetEnumerator` métodos de la `IEnumerable` y `IEnumerable<T>` interfaces. Los dos `GetEnumerator` métodos comparten una implementación común que adquiere y devuelve un objeto de enumerador disponible. El objeto de enumerador se inicializa con los valores de argumento y la instancia valor guardado cuando el objeto enumerable que se ha inicializado, pero en caso contrario, las funciones del objeto de enumerador tal como se describe en [objetos enumerador](classes.md#enumerator-objects).

### <a name="implementation-example"></a>Ejemplo de implementación

Esta sección describe una posible implementación de iteradores en cuanto a las construcciones C# estándar. La implementación que se describen aquí se basa en los mismos principios utilizados por el compilador de C# de Microsoft, pero en ningún caso es una implementación obligatoria o el único posible.

La siguiente `Stack<T>` la clase implementa su `GetEnumerator` método mediante un iterador. El iterador enumera los elementos de la pila en orden descendente.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

El `GetEnumerator` método se puede traducir en una instancia de una clase de enumerador generado por el compilador que encapsula el código en el bloque de iteradores, tal como se muestra en la siguiente.

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

En la traducción anterior, se convierte en un equipo de estado y se coloca en el código del bloque de iteradores el `MoveNext` método de la clase de enumerador. Además, la variable local `i` se convierte en un campo en el objeto de enumerador para que pueda seguir existen entre las distintas invocaciones de `MoveNext`.

El ejemplo siguiente imprime una sencilla tabla de multiplicación de enteros del 1 al 10. El `FromTo` método en el ejemplo devuelve un objeto enumerable y se implementa mediante un iterador.

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

El `FromTo` método se puede traducir en una instancia de una clase enumerable generado por el compilador que encapsula el código del bloque de iteradores, tal como se muestra en la siguiente.

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

La clase enumerable implementa las interfaces enumerables y las interfaces de enumerador, habilitarla para que actúe como un objeto enumerable y un enumerador. La primera vez el `GetEnumerator` se invoca al método, se devuelve el mismo objeto enumerable. Las siguientes invocaciones del objeto enumerable `GetEnumerator`, si existe, devuelve una copia del objeto enumerable. Por lo tanto, cada uno devuelve enumerador tiene su propio estado y los cambios de un enumerador no afectarán a otro. El `Interlocked.CompareExchange` método se utiliza para garantizar un funcionamiento seguro para subprocesos.

El `from` y `to` parámetros se convierten en campos de la clase enumerable. Dado que `from` se modifica en el bloque de iteradores adicional `__from` campo se introdujo para contener el valor inicial dado a `from` en cada enumerador.

El `MoveNext` método produce una `InvalidOperationException` si se llama cuando `__state` es `0`. Esto protege contra el uso del objeto enumerable como un objeto de enumerador sin llamar primero a `GetEnumerator`.

El ejemplo siguiente muestra una clase simple de árbol. El `Tree<T>` la clase implementa su `GetEnumerator` método mediante un iterador. El iterador enumera los elementos del árbol en orden infijo.

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

El `GetEnumerator` método se puede traducir en una instancia de una clase de enumerador generado por el compilador que encapsula el código en el bloque de iteradores, tal como se muestra en la siguiente.

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Los objetos temporales generados por el compilador usados en el `foreach` se levantan instrucciones en el `__left` y `__right` campos del objeto enumerador. El `__state` cuidadosamente se actualiza el campo del objeto enumerador para que el valor correcto `Dispose()` se llamará al método correctamente si se produce una excepción. Tenga en cuenta que no es posible escribir el código traducido con simple `foreach` instrucciones.

## <a name="async-functions"></a>Funciones asincrónicas

Un método ([métodos](classes.md#methods)) o una función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) con el `async` modificador se denomina un ***función asincrónica***. En general, el término ***async*** se usa para describir cualquier tipo de función que tiene el `async` modificador.

Es un error en tiempo de compilación para la lista de parámetros formales de una función asincrónica para especificar cualquier `ref` o `out` parámetros.

El *return_type* Async método debe ser `void` o un ***tipo de tarea***. Los tipos de tarea son `System.Threading.Tasks.Task` y tipos construyan desde `System.Threading.Tasks.Task<T>`. Por brevedad, en este capítulo estos tipos se denominan `Task` y `Task<T>`, respectivamente. Un método asincrónico devuelve un tipo de tarea se dice que devuelven tareas.

La definición exacta de los tipos de tarea es implementación definida, pero desde la perspectiva del lenguaje, un tipo de tarea está en uno de los Estados incompletos, se ha realizado correctamente o con errores. Una tarea con error registra una excepción pertinente. Una correcta `Task<T>` registra un resultado de tipo `T`. Tipos de tareas son esperables y puede ser, por tanto, los operandos de expresiones await ([expresiones Await](expressions.md#await-expressions)).

Una invocación de función async tiene la capacidad para suspender la evaluación por medio de expresiones await ([expresiones Await](expressions.md#await-expressions)) en su cuerpo. Más adelante se puede reanudar la evaluación en el punto de suspensión de la expresión por medio de await un ***delegado de reanudación***. El delegado de reanudación es de tipo `System.Action`, y cuando se invoca, evaluación de la invocación de función async se reanudará desde la expresión await, donde se quedó. El ***llamador actual*** de una función asincrónica invocación es el llamador original si nunca se ha suspendido la invocación de función o el llamador del delegado de reanudación más reciente, en caso contrario.

### <a name="evaluation-of-a-task-returning-async-function"></a>Evaluación de una función asincrónica de devolución de tarea

Invocación de una función asincrónica de devolución de tarea hace que una instancia del tipo de tarea devuelta se genere. Esto se denomina la ***task devuelto*** de la función async. La tarea está inicialmente en un estado incompleto.

El cuerpo de la función async, a continuación, se evalúa hasta que se suspende (que se alcanza una expresión await) o se finaliza, en el que se devuelve punto de control al llamador, junto con la tarea devuelto.

Cuando el cuerpo de la función asincrónica termina, la tarea devuelta se mueve fuera del estado incompleto:

*  Si el cuerpo de la función termina como resultado de alcanzar una instrucción return o al final del cuerpo, cualquier valor de resultado se registra en la tarea devuelta, que se coloca en un estado correcto.
*  Si el cuerpo de la función termina como resultado de una excepción no detectada ([la instrucción throw](statements.md#the-throw-statement)) se grabó la excepción en la tarea devuelta que se coloca en un estado de error.

### <a name="evaluation-of-a-void-returning-async-function"></a>Evaluación de una función asincrónica devuelve void

Si el tipo de valor devuelto de la función async es `void`, evaluación difiere de los pasos anteriores en la siguiente manera: Dado que no se devuelve ninguna tarea, la función comunica en su lugar excepciones en el subproceso actual y finalización ***contexto de sincronización***. La definición exacta del contexto de sincronización depende de la implementación, pero es una representación de "donde" se está ejecutando el subproceso actual. El contexto de sincronización se notifica cuando comienza la evaluación de una función asincrónica devuelve void, se completa correctamente o provoca que se produzca una excepción no detectada.

Esto permite que el contexto para realizar un seguimiento de cuántas funciones async devuelven void se ejecutan en él y decidir cómo propagar las excepciones que salen de ellas.
