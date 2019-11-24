---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703979"
---
# <a name="classes"></a>Clases

Una clase es una estructura de datos que puede contener miembros de datos (constantes y campos), miembros de función (métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos) y tipos anidados. Los tipos de clase admiten la herencia, un mecanismo mediante el cual una clase derivada puede extender y especializar una clase base.

## <a name="class-declarations"></a>Declaraciones de clase

Una *class_declaration* es una *type_declaration* ([declaraciones de tipos](namespaces.md#type-declarations)) que declara una nueva clase.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

Un *class_declaration* está compuesto de un conjunto opcional *de atributos* ([atributos](attributes.md)), seguido de un conjunto opcional de *class_modifier*s ([modificadores de clase](classes.md#class-modifiers)), seguido de un modificador de `partial` opcional, seguido de la palabra clave `class` y un *identificador* que nombra la clase, seguido de un *type_parameter_list* opcional (parámetros de[tipo](classes.md#type-parameters)), seguido de una especificación de *class_base* opcional (especificación de[clase base](classes.md#class-base-specification)), por un conjunto opcional de *type_parameter_constraints_clause*s ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)), seguido de un *class_body* ([cuerpo de clase](classes.md#class-body)), opcionalmente seguido de un punto y coma.

Una declaración de clase no puede proporcionar *type_parameter_constraints_clause*s a menos que también proporcione un *type_parameter_list*.

Una declaración de clase que proporciona un *type_parameter_list* es una ***declaración de clase genérica***. Además, cualquier clase anidada dentro de una declaración de clase genérica o una declaración de estructura genérica es una declaración de clase genérica, ya que se deben proporcionar los parámetros de tipo para el tipo contenedor para crear un tipo construido.

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

Es un error en tiempo de compilación que el mismo modificador aparezca varias veces en una declaración de clase.

Se permite el modificador `new` en las clases anidadas. Especifica que la clase oculta un miembro heredado con el mismo nombre, tal y como se describe en [el modificador New](classes.md#the-new-modifier). Es un error en tiempo de compilación que el modificador `new` aparezca en una declaración de clase que no es una declaración de clase anidada.

Los modificadores `public`, `protected`, `internal`y `private` controlan la accesibilidad de la clase. Dependiendo del contexto en el que se produce la declaración de clase, es posible que algunos de estos modificadores no estén permitidos (se[declare accesibilidad](basic-concepts.md#declared-accessibility)).

Los modificadores `abstract`, `sealed` y `static` se describen en las secciones siguientes.

#### <a name="abstract-classes"></a>Clases abstractas

El modificador `abstract` se usa para indicar que una clase está incompleta y que está destinada a usarse solo como una clase base. Una clase abstracta es distinta de una clase no abstracta de las siguientes maneras:

*  No se puede crear una instancia de una clase abstracta directamente y es un error en tiempo de compilación usar el operador `new` en una clase abstracta. Aunque es posible tener variables y valores cuyos tipos en tiempo de compilación sean abstractos, tales variables y valores serán necesariamente `null` o contengan referencias a instancias de clases no abstractas derivadas de los tipos abstractos.
*  Se permite que una clase abstracta contenga miembros abstractos (aunque no es necesario).
*  Una clase abstracta no puede ser sellada.

Cuando una clase no abstracta se deriva de una clase abstracta, la clase no abstracta debe incluir las implementaciones reales de todos los miembros abstractos heredados, con lo que se reemplazan los miembros abstractos. en el ejemplo
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
la clase abstracta `A` presenta un método abstracto `F`. La clase `B` introduce un método adicional `G`, pero dado que no proporciona una implementación de `F`, `B` también debe declararse como abstracta. La clase `C` invalida `F` y proporciona una implementación real. Dado que no hay miembros abstractos en `C`, se permite que `C` (pero no es necesario) sea no abstracto.

#### <a name="sealed-classes"></a>Clases selladas

El modificador `sealed` se utiliza para evitar la derivación de una clase. Se produce un error en tiempo de compilación si se especifica una clase sellada como la clase base de otra clase.

Una clase sellada no puede ser también una clase abstracta.

El modificador `sealed` se utiliza principalmente para evitar la derivación no deseada, pero también permite ciertas optimizaciones en tiempo de ejecución. En concreto, dado que se sabe que una clase sellada nunca tiene clases derivadas, es posible transformar las invocaciones de miembros de función virtual en instancias de clase selladas en invocaciones no virtuales.

#### <a name="static-classes"></a>Clases estáticas

El modificador `static` se usa para marcar la clase que se declara como una ***clase estática***. No se puede crear una instancia de una clase estática, no se puede usar como un tipo y solo puede contener miembros estáticos. Solo una clase estática puede contener declaraciones de métodos de extensión ([métodos de extensión](classes.md#extension-methods)).

Una declaración de clase estática está sujeta a las siguientes restricciones:

*  Una clase estática no puede incluir un modificador `sealed` o `abstract`. Sin embargo, tenga en cuenta que, puesto que no se pueden crear instancias de una clase estática ni derivarse de, se comporta como si fuera Sealed y Abstract.
*  Una clase estática no puede incluir una especificación de *class_base* ([clase base Specification](classes.md#class-base-specification)) y no puede especificar explícitamente una clase base o una lista de interfaces implementadas. Una clase estática hereda implícitamente del tipo `object`.
*  Una clase estática solo puede contener miembros estáticos ([miembros estáticos y de instancia](classes.md#static-and-instance-members)). Tenga en cuenta que las constantes y los tipos anidados se clasifican como miembros estáticos.
*  Una clase estática no puede tener miembros con `protected` o `protected internal` la accesibilidad declarada.

Es un error en tiempo de compilación infringir cualquiera de estas restricciones.

Una clase estática no tiene ningún constructor de instancia. No es posible declarar un constructor de instancia en una clase estática, y no se proporciona ningún constructor de instancia predeterminado ([constructores predeterminados](classes.md#default-constructors)) para una clase estática.

Los miembros de una clase estática no son estáticos automáticamente y las declaraciones de miembros deben incluir explícitamente un modificador `static` (excepto para las constantes y los tipos anidados). Cuando una clase está anidada dentro de una clase externa estática, la clase anidada no es una clase estática a menos que incluya explícitamente un modificador `static`.

__Referencia a tipos de clase estáticos__

Se permite que un *namespace_or_type_name* ([espacio de nombres y nombres de tipo](basic-concepts.md#namespace-and-type-names)) haga referencia a una clase estática Si

*  El *namespace_or_type_name* es el `T` en un *namespace_or_type_name* del formulario `T.I`, o
*  El *namespace_or_type_name* es el `T` en un *typeof_expression* ([listas de argumentos](expressions.md#argument-lists)1) del formulario `typeof(T)`.

Se permite que un *primary_expression* ([miembros de función](expressions.md#function-members)) haga referencia a una clase estática Si

*  El *primary_expression* es el `E` en un *member_access* ([comprobación en tiempo de compilación de la resolución dinámica de sobrecarga](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) del formulario `E.I`.

En cualquier otro contexto, se trata de un error en tiempo de compilación para hacer referencia a una clase estática. Por ejemplo, es un error que una clase estática se use como una clase base, un tipo constituyente ([tipos anidados](classes.md#nested-types)) de un miembro, un argumento de tipo genérico o una restricción de parámetro de tipo. Del mismo modo, una clase estática no se puede usar en un tipo de matriz, un tipo de puntero, una expresión de `new`, una expresión de conversión, una expresión de `is`, una expresión de `as`, una expresión de `sizeof` o una expresión de valor predeterminado.

### <a name="partial-modifier"></a>Unmodifier (modificador)

El modificador `partial` se utiliza para indicar que este *class_declaration* es una declaración de tipos parciales. Varias declaraciones de tipos parciales con el mismo nombre dentro de una declaración de tipo o espacio de nombres envolvente se combinan para formar una declaración de tipos, siguiendo las reglas especificadas en [tipos parciales](classes.md#partial-types).

Tener la declaración de una clase distribuida sobre segmentos independientes del texto del programa puede ser útil si estos segmentos se producen o mantienen en contextos diferentes. Por ejemplo, una parte de una declaración de clase puede ser generada por el equipo, mientras que la otra se crea manualmente. La separación textual de los dos impide que las actualizaciones se realicen en conflicto con las actualizaciones del otro.

### <a name="type-parameters"></a>Parámetros de tipo

Un parámetro de tipo es un identificador simple que denota un marcador de posición para un argumento de tipo proporcionado para crear un tipo construido. Un parámetro de tipo es un marcador de posición formal para un tipo que se proporcionará más adelante. Por el contrario, un argumento de tipo ([argumentos de tipo](types.md#type-arguments)) es el tipo real que se sustituye por el parámetro de tipo cuando se crea un tipo construido.

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

Cada parámetro de tipo de una declaración de clase define un nombre en el espacio de declaración ([declaraciones](basic-concepts.md#declarations)) de esa clase. Por lo tanto, no puede tener el mismo nombre que otro parámetro de tipo o un miembro declarado en esa clase. Un parámetro de tipo no puede tener el mismo nombre que el propio tipo.

### <a name="class-base-specification"></a>Especificación de clase base

Una declaración de clase puede incluir una especificación de *class_base* , que define la clase base directa de la clase y las interfaces ([interfaces](interfaces.md)) implementadas directamente por la clase.

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

La clase base especificada en una declaración de clase puede ser un tipo de clase construido ([tipos construidos](types.md#constructed-types)). Una clase base no puede ser un parámetro de tipo por su cuenta, aunque puede incluir los parámetros de tipo que se encuentran en el ámbito.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Clases base

Cuando se incluye un *class_type* en el *class_base*, especifica la clase base directa de la clase que se está declarando. Si una declaración de clase no tiene *class_base*, o si el *class_base* solo enumera los tipos de interfaz, se supone que la clase base directa se `object`. Una clase hereda los miembros de su clase base directa, como se describe en [herencia](classes.md#inheritance).

en el ejemplo
```csharp
class A {}

class B: A {}
```
la clase `A` se dice que es la clase base directa de `B`y `B` se dice que se deriva de `A`. Puesto que `A` no especifica explícitamente una clase base directa, su clase base directa se `object`implícitamente.

Para un tipo de clase construido, si se especifica una clase base en la declaración de clase genérica, la clase base del tipo construido se obtiene sustituyendo, por cada *type_parameter* en la declaración de clase base, el *type_argument* correspondiente del tipo construido. Dadas las declaraciones de clase genéricas
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
la clase base del tipo construido `G<int>` sería `B<string,int[]>`.

La clase base directa de un tipo de clase debe ser al menos igual de accesible que el propio tipo de clase ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)). Por ejemplo, se trata de un error en tiempo de compilación para que una clase `public` derive de una clase `private` o `internal`.

La clase base directa de un tipo de clase no debe ser ninguno de los siguientes tipos: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`o `System.ValueType`. Además, una declaración de clase genérica no puede usar `System.Attribute` como una clase base directa o indirecta.

A la hora de determinar el significado de la especificación de clase base directa `A` de una `B`de clase, se supone que la clase base directa de `B` está `object`da de forma temporal. Intuitivamente esto garantiza que el significado de una especificación de clase base no puede depender recursivamente. En el ejemplo:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
es un error porque en la especificación de clase base `A<C.B>` la clase base directa de `C` se considera `object`y, por lo tanto, las reglas de [nombres de espacio de nombres y de tipo](basic-concepts.md#namespace-and-type-names), `C` no se considera que tiene un miembro `B`.

Las clases base de un tipo de clase son la clase base directa y sus clases base. En otras palabras, el conjunto de clases base es el cierre transitivo de la relación de clase base directa. En el ejemplo anterior, las clases base de `B` son `A` y `object`. en el ejemplo
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
las clases base de `D<int>` son `C<int[]>`, `B<IComparable<int[]>>`, `A`y `object`.

Excepto en el caso de la clase `object`, cada tipo de clase tiene exactamente una clase base directa. La clase `object` no tiene ninguna clase base directa y es la última clase base de todas las demás clases.

Cuando una clase `B` deriva de una `A`de clase, es un error en tiempo de compilación para que `A` dependa de `B`. Una clase ***depende directamente de*** su clase base directa (si existe) y ***depende directamente de*** la clase en la que se anida inmediatamente (si existe). Dada esta definición, el conjunto completo de clases de las que depende una clase es el cierre reflexivo y transitivo de la relación ***directamente depende de*** .

El ejemplo
```csharp
class A: A {}
```
es erróneo porque la clase depende de sí misma. Del mismo modo, el ejemplo
```csharp
class A: B {}
class B: C {}
class C: A {}
```
es un error porque las clases dependen circularmente por sí mismas. Por último, el ejemplo
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
produce un error en tiempo de compilación porque `A` depende de `B.C` (su clase base directa), que depende de `B` (su clase envolvente inmediata), que depende circularmente de `A`.

Tenga en cuenta que una clase no depende de las clases anidadas en ella. en el ejemplo
```csharp
class A
{
    class B: A {}
}
```
`B` depende de `A` (porque `A` es tanto su clase base directa como su clase envolvente inmediata), pero `A` no depende de `B` (puesto que `B` no es una clase base ni una clase envolvente de `A`). Por lo tanto, el ejemplo es válido.

No es posible derivar de una clase `sealed`. en el ejemplo
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
la `B` de clase tiene un error porque intenta derivar de la clase `sealed` `A`.

#### <a name="interface-implementations"></a>Implementaciones de interfaces

Una especificación de *class_base* puede incluir una lista de tipos de interfaz, en cuyo caso se dice que la clase implementa directamente los tipos de interfaz especificados. Las implementaciones de interfaz se tratan en las [implementaciones de interfaz](interfaces.md#interface-implementations).

### <a name="type-parameter-constraints"></a>Restricciones de parámetros de tipo

Las declaraciones de tipos y métodos genéricos pueden especificar opcionalmente restricciones de parámetros de tipo incluyendo *type_parameter_constraints_clause*s.

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

Cada *type_parameter_constraints_clause* consta del `where`de token, seguido del nombre de un parámetro de tipo, seguido de dos puntos y la lista de restricciones para ese parámetro de tipo. Puede haber como máximo una cláusula `where` para cada parámetro de tipo y las cláusulas `where` se pueden enumerar en cualquier orden. Al igual que los tokens de `get` y `set` en un descriptor de acceso de propiedad, el token de `where` no es una palabra clave.

La lista de restricciones dadas en una cláusula `where` puede incluir cualquiera de los componentes siguientes, en este orden: una restricción Primary única, una o varias restricciones secundarias y la restricción de constructor, `new()`.

Una restricción Primary puede ser un tipo de clase o la ***restricción de tipo de referencia*** `class` o la restricción de tipo de ***valor*** `struct`. Una restricción secundaria puede ser una *type_parameter* o *interface_type*.

La restricción de tipo de referencia especifica que un argumento de tipo usado para el parámetro de tipo debe ser un tipo de referencia. Todos los tipos de clase, tipos de interfaz, tipos de delegado, tipos de matriz y parámetros de tipo que se sabe que son un tipo de referencia (como se define a continuación) satisfacen esta restricción.

La restricción de tipo de valor especifica que un argumento de tipo usado para el parámetro de tipo debe ser un tipo de valor que no acepta valores NULL. Todos los tipos de struct que no aceptan valores NULL, tipos de enumeración y parámetros de tipo con la restricción de tipo de valor cumplen esta restricción. Tenga en cuenta que aunque se clasifique como un tipo de valor, un tipo que acepta valores NULL ([tipos que aceptan valores NULL](types.md#nullable-types)) no satisface la restricción de tipo de valor. Un parámetro de tipo que tiene la restricción de tipo de valor no puede tener también el *constructor_constraint*.

Los tipos de puntero nunca pueden ser argumentos de tipo y no se tienen en cuenta para satisfacer las restricciones de tipo de valor o tipo de referencia.

Si una restricción es un tipo de clase, un tipo de interfaz o un parámetro de tipo, ese tipo especifica un "tipo base" mínimo que cada argumento de tipo utilizado para ese parámetro de tipo debe admitir. Siempre que se usa un tipo construido o un método genérico, el argumento de tipo se compara con las restricciones del parámetro de tipo en tiempo de compilación. El argumento de tipo proporcionado debe cumplir las condiciones descritas en satisfacción de las [restricciones](types.md#satisfying-constraints).

Una restricción *class_type* debe cumplir las siguientes reglas:

*  El tipo debe ser un tipo de clase.
*  El tipo no se debe `sealed`.
*  El tipo no debe ser uno de los tipos siguientes: `System.Array`, `System.Delegate`, `System.Enum`o `System.ValueType`.
*  El tipo no se debe `object`. Dado que todos los tipos se derivan de `object`, este tipo de restricción no tendría ningún efecto si se permitiera.
*  Como máximo, una restricción para un parámetro de tipo determinado puede ser un tipo de clase.

Un tipo especificado como restricción *interface_type* debe cumplir las siguientes reglas:

*  El tipo debe ser un tipo de interfaz.
*  Un tipo no debe especificarse más de una vez en una cláusula `where` determinada.

En cualquier caso, la restricción puede incluir cualquiera de los parámetros de tipo del tipo o la declaración de método asociados como parte de un tipo construido, y puede implicar el tipo que se declara.

Cualquier tipo de clase o interfaz especificado como restricción de parámetro de tipo debe ser al menos igual de accesible ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)) que el tipo o método genérico que se está declarando.

Un tipo especificado como restricción *type_parameter* debe cumplir las siguientes reglas:

*  El tipo debe ser un parámetro de tipo.
*  Un tipo no debe especificarse más de una vez en una cláusula `where` determinada.

Además, no debe haber ningún ciclo en el gráfico de dependencias de los parámetros de tipo, donde Dependency es una relación transitiva definida por:

*  Si se usa un parámetro de tipo `T` como una restricción para el parámetro de tipo `S` `S` ***depende de*** `T`.
*  Si un parámetro de tipo `S` depende de un parámetro de tipo `T` y `T` depende de un parámetro de tipo `U`, a continuación, `S` ***depende de*** `U`.

Dada esta relación, se trata de un error en tiempo de compilación para que un parámetro de tipo dependa de sí mismo (directa o indirectamente).

Cualquier restricción debe ser coherente entre los parámetros de tipo dependiente. Si el parámetro de tipo `S` depende del parámetro de tipo `T` entonces:

*  `T` no deben tener la restricción de tipo de valor. De lo contrario, `T` se sella de forma eficaz, de modo que `S` se forzaría a ser del mismo tipo que `T`, lo que elimina la necesidad de dos parámetros de tipo.
*  Si `S` tiene la restricción de tipo de valor, `T` no debe tener una restricción de *class_type* .
*  Si `S` tiene una restricción *class_type* `A` y `T` tiene una restricción *class_type* `B`, debe haber una conversión de identidad o una conversión de referencia implícita de `A` a `B` o una conversión de referencia implícita de `B` a `A`.
*  Si `S` también depende del parámetro de tipo `U` y `U` tiene una restricción *class_type* `A` y `T` tiene una restricción *class_type* `B`, debe haber una conversión de identidad o una conversión de referencia implícita de `A` a `B` o una conversión de referencia implícita de `B` a `A`.

Es válido para que `S` tenga la restricción de tipo de valor y `T` para tener la restricción de tipo de referencia. De hecho, esto limita `T` a los tipos `System.Object`, `System.ValueType`, `System.Enum`y cualquier tipo de interfaz.

Si la cláusula `where` para un parámetro de tipo incluye una restricción de constructor (que tiene el formato `new()`), es posible utilizar el operador `new` para crear instancias del tipo ([expresiones de creación de objetos](expressions.md#object-creation-expressions)). Cualquier argumento de tipo utilizado para un parámetro de tipo con una restricción de constructor debe tener un constructor sin parámetros público (este constructor existe implícitamente para cualquier tipo de valor) o ser un parámetro de tipo con la restricción de tipo de valor o la restricción de constructor (vea [restricciones de parámetro de tipo](classes.md#type-parameter-constraints) para obtener más detalles).

A continuación se muestran ejemplos de restricciones:
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

El ejemplo siguiente es erróneo porque provoca una circularidad en el gráfico de dependencias de los parámetros de tipo:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

En los siguientes ejemplos se muestran situaciones no válidas adicionales:
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

La ***clase base efectiva*** de un parámetro de tipo `T` se define de la siguiente manera:

*  Si `T` no tiene restricciones PRIMARY o restricciones de parámetro de tipo, se `object`su clase base efectiva.
*  Si `T` tiene la restricción de tipo de valor, se `System.ValueType`su clase base efectiva.
*  Si `T` tiene una restricción *class_type* `C` pero no *type_parameter* restricciones, se `C`su clase base efectiva.
*  Si `T` no tiene ninguna restricción de *class_type* pero tiene una o más restricciones de *type_parameter* , su clase base efectiva es el tipo más abarcado ([operadores de conversión de elevación](conversions.md#lifted-conversion-operators)) en el conjunto de clases base eficaces de sus restricciones de *type_parameter* . Las reglas de coherencia garantizan que exista un tipo más abarcado.
*  Si `T` tiene una restricción de *class_type* y una o varias restricciones de *type_parameter* , su clase base efectiva es el tipo más abarcado (operadores de[conversión levantada](conversions.md#lifted-conversion-operators)) del conjunto que consta de la restricción *class_type* de `T` y las clases base eficaces de sus restricciones de *type_parameter* . Las reglas de coherencia garantizan que exista un tipo más abarcado.
*  Si `T` tiene la restricción de tipo de referencia pero no *class_type* restricciones, se `object`su clase base efectiva.

En el caso de estas reglas, si T tiene una restricción `V` que es una *value_type*, use en su lugar el tipo base más específico de `V` que sea un *class_type*. Esto nunca puede ocurrir en una restricción explícitamente especificada, pero puede producirse cuando las restricciones de un método genérico se heredan implícitamente mediante una declaración de método de reemplazo o una implementación explícita de un método de interfaz.

Estas reglas garantizan que la clase base efectiva siempre sea una *class_type*.

El ***conjunto de interfaces efectivo*** de un parámetro de tipo `T` se define de la siguiente manera:

*  Si `T` no tiene *secondary_constraints*, su conjunto de interfaces efectivo está vacío.
*  Si `T` tiene *interface_type* restricciones pero no *type_parameter* restricciones, su conjunto de interfaces efectivo es su conjunto de restricciones de *interface_type* .
*  Si `T` no tiene restricciones de *interface_type* pero tiene *type_parameter* restricciones, su conjunto de interfaces efectivo es la Unión de los conjuntos de interfaces eficaces de sus restricciones de *type_parameter* .
*  Si `T` tiene restricciones de *interface_type* y restricciones de *type_parameter* , su conjunto de interfaces efectivo es la Unión de su conjunto de restricciones de *interface_type* y los conjuntos de interfaces eficaces de sus restricciones de *type_parameter* .

Se sabe que un parámetro de tipo es ***un tipo de referencia*** si tiene la restricción de tipo de referencia o su clase base efectiva no es `object` o `System.ValueType`.

Los valores de un tipo de parámetro de tipo restringido se pueden utilizar para tener acceso a los miembros de instancia que implican las restricciones. en el ejemplo
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
los métodos de `IPrintable` se pueden invocar directamente en `x` porque `T` está restringido a implementar siempre `IPrintable`.

### <a name="class-body"></a>Cuerpo de clase

El *class_body* de una clase define los miembros de esa clase.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Tipos parciales

Una declaración de tipos se puede dividir en varias ***declaraciones de tipos parciales***. La declaración de tipos se construye a partir de sus elementos siguiendo las reglas de esta sección, en la que se trata como una única declaración durante el resto del procesamiento en tiempo de compilación y en tiempo de ejecución del programa.

Un *class_declaration*, *struct_declaration* o *interface_declaration* representa una declaración de tipos parciales si incluye un modificador `partial`. `partial` no es una palabra clave y solo actúa como modificador si aparece inmediatamente antes de que una de las palabras clave `class`, `struct` o `interface` en una declaración de tipos o antes del tipo `void` en una declaración de método. En otros contextos, se puede usar como un identificador normal.

Cada parte de una declaración de tipo parcial debe incluir un modificador `partial`. Debe tener el mismo nombre y estar declarado en el mismo espacio de nombres o declaración de tipos que las demás partes. El modificador `partial` indica que pueden existir partes adicionales de la declaración de tipos en otro lugar, pero la existencia de tales partes adicionales no es un requisito; es válido para un tipo con una única declaración para incluir el modificador `partial`.

Todas las partes de un tipo parcial se deben compilar de forma que se puedan combinar las partes en tiempo de compilación en una única declaración de tipo. Los tipos parciales no permiten que se extiendan los tipos ya compilados.

Los tipos anidados se pueden declarar en varias partes mediante el modificador `partial`. Normalmente, el tipo contenedor se declara utilizando también `partial`, y cada parte del tipo anidado se declara en una parte diferente del tipo contenedor.

No se permite el modificador `partial` en las declaraciones de delegado o de enumeración.

### <a name="attributes"></a>Atributos

Los atributos de un tipo parcial se determinan mediante la combinación, en un orden no especificado, con los atributos de cada uno de los elementos. Si un atributo se coloca en varias partes, es equivalente a especificar el atributo varias veces en el tipo. Por ejemplo, las dos partes:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
son equivalentes a una declaración como:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Los atributos de los parámetros de tipo se combinan de manera similar.

### <a name="modifiers"></a>Modificadores

Cuando una declaración de tipos parciales incluye una especificación de accesibilidad (los modificadores `public`, `protected`, `internal`y `private`) debe coincidir con todas las demás partes que incluyen una especificación de accesibilidad. Si ninguna parte de un tipo parcial incluye una especificación de accesibilidad, se proporciona al tipo la accesibilidad predeterminada adecuada (la[accesibilidad declarada](basic-concepts.md#declared-accessibility)).

Si una o varias declaraciones parciales de un tipo anidado incluyen un modificador `new`, no se genera ninguna advertencia si el tipo anidado oculta un miembro heredado ([ocultando a través](basic-concepts.md#hiding-through-inheritance)de la herencia).

Si una o varias declaraciones parciales de una clase incluyen un modificador `abstract`, la clase se considera abstracta ([clases abstractas](classes.md#abstract-classes)). De lo contrario, la clase se considera no abstracta.

Si una o varias declaraciones parciales de una clase incluyen un modificador `sealed`, la clase se considera Sealed ([clases selladas](classes.md#sealed-classes)). De lo contrario, se considera que la clase no está sellada.

Tenga en cuenta que una clase no puede ser Abstract y Sealed.

Cuando el modificador `unsafe` se usa en una declaración de tipo parcial, solo esa parte concreta se considera un contexto no seguro ([contextos no seguros](unsafe-code.md#unsafe-contexts)).

### <a name="type-parameters-and-constraints"></a>Parámetros de tipo y restricciones

Si un tipo genérico se declara en varias partes, cada elemento debe indicar los parámetros de tipo. Cada elemento debe tener el mismo número de parámetros de tipo y el mismo nombre para cada parámetro de tipo, en orden.

Cuando una declaración de tipos genéricos parciales incluye restricciones (cláusulas`where`), las restricciones deben coincidir con todas las demás partes que incluyen restricciones. En concreto, cada parte que incluye restricciones debe tener restricciones para el mismo conjunto de parámetros de tipo y, para cada parámetro de tipo, los conjuntos de restricciones PRIMARY, Secondary y constructor deben ser equivalentes. Dos conjuntos de restricciones son equivalentes si contienen los mismos miembros. Si ninguna parte de un tipo genérico parcial especifica las restricciones de parámetro de tipo, los parámetros de tipo se consideran no restringidos.

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
es correcto porque las partes que incluyen restricciones (las dos primeras) especifican de forma eficaz el mismo conjunto de restricciones principales, secundarias y constructores para el mismo conjunto de parámetros de tipo, respectivamente.

### <a name="base-class"></a>Clase base

Cuando una declaración de clase parcial incluye una especificación de clase base, debe coincidir con todas las demás partes que incluyen una especificación de clase base. Si ninguna parte de una clase parcial incluye una especificación de clase base, la clase base se convierte en `System.Object` ([clases base](classes.md#base-classes)).

### <a name="base-interfaces"></a>Interfaces base

El conjunto de interfaces base para un tipo declarado en varias partes es la Unión de las interfaces base especificadas en cada parte. Solo se puede asignar un nombre a una interfaz base determinada una vez en cada parte, pero se permite que varias partes denominen a las mismas interfaces base. Solo debe haber una implementación de los miembros de una interfaz base determinada.

en el ejemplo
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
el conjunto de interfaces base para `C` de clase es `IA`, `IB`y `IC`.

Normalmente, cada elemento proporciona una implementación de las interfaces declaradas en esa parte; sin embargo, esto no es un requisito. Un elemento puede proporcionar la implementación de una interfaz declarada en una parte diferente:
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

A excepción de los métodos parciales ([métodos parciales](classes.md#partial-methods)), el conjunto de miembros de un tipo declarado en varias partes es simplemente la Unión del conjunto de miembros declarado en cada parte. Los cuerpos de todas las partes de la declaración de tipos comparten el mismo espacio de declaración ([declaraciones](basic-concepts.md#declarations)), y el ámbito de cada miembro ([ámbitos](basic-concepts.md#scopes)) se extiende a los cuerpos de todas las partes. El dominio de accesibilidad de cualquier miembro siempre incluye todas las partes del tipo envolvente; un miembro de `private` declarado en una parte es accesible libremente desde otra parte. Es un error en tiempo de compilación declarar el mismo miembro en más de una parte del tipo, a menos que ese miembro sea un tipo con el modificador `partial`.

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

El orden de los miembros dentro de un tipo rara vez C# es significativo para el código, pero puede ser importante al interactuar con otros lenguajes y entornos. En estos casos, el orden de los miembros dentro de un tipo declarado en varias partes es indefinido.

### <a name="partial-methods"></a>Métodos parciales

Los métodos parciales se pueden definir en una parte de una declaración de tipos e implementarse en otro. La implementación es opcional. Si ninguna parte implementa el método parcial, la declaración de método parcial y todas las llamadas a ella se quitan de la declaración de tipos resultante de la combinación de los elementos.

Los métodos parciales no pueden definir modificadores de acceso, pero implícitamente `private`. Su tipo de valor devuelto debe ser `void`y sus parámetros no pueden tener el modificador `out`. El identificador `partial` se reconoce como una palabra clave especial en una declaración de método solo si aparece justo antes del tipo de `void`; de lo contrario, se puede usar como un identificador normal. Un método parcial no puede implementar explícitamente métodos de interfaz.

Hay dos tipos de declaraciones de método parcial: Si el cuerpo de la declaración del método es un punto y coma, se dice que la declaración es una ***declaración de método parcial de definición***. Si el cuerpo se proporciona como un *bloque*, se dice que la declaración es una ***declaración de método parcial de implementación***. En las partes de una declaración de tipos solo puede haber una declaración de método parcial de definición con una firma determinada, y solo puede haber una declaración de método parcial de implementación con una firma determinada. Si se proporciona una declaración de método parcial de implementación, debe existir una declaración de método parcial de definición correspondiente, y las declaraciones deben coincidir como se especifica en lo siguiente:

* Las declaraciones deben tener los mismos modificadores (aunque no necesariamente en el mismo orden), el nombre del método, el número de parámetros de tipo y el número de parámetros.
* Los parámetros correspondientes en las declaraciones deben tener los mismos modificadores (aunque no necesariamente en el mismo orden) y los mismos tipos (diferencias de módulo en los nombres de parámetros de tipo).
* Los parámetros de tipo correspondientes en las declaraciones deben tener las mismas restricciones (diferencias de módulo en los nombres de parámetros de tipo).

Una declaración de método parcial de implementación puede aparecer en la misma parte que la declaración de método parcial de definición correspondiente.

Solo un método parcial de definición participa en la resolución de sobrecarga. Por lo tanto, tanto si se proporciona una declaración de implementación como si no, las expresiones de invocación pueden resolverse en invocaciones del método parcial. Dado que un método parcial siempre devuelve `void`, estas expresiones de invocación siempre serán instrucciones de expresión. Además, dado que un método parcial se `private`implícitamente, estas instrucciones siempre se producirán dentro de una de las partes de la declaración de tipos en la que se declara el método parcial.

Si ninguna parte de una declaración de tipo parcial contiene una declaración de implementación para un método parcial determinado, cualquier instrucción de expresión que lo invoque simplemente se quita de la declaración de tipos combinados. Por lo tanto, la expresión de invocación, incluidas las expresiones constituyentes, no tiene ningún efecto en tiempo de ejecución. También se quita el método parcial en sí y no será miembro de la declaración de tipos combinados.

Si existe una declaración de implementación para un método parcial determinado, se conservan las invocaciones de los métodos parciales. El método parcial da lugar a una declaración de método similar a la declaración de método parcial de implementación, excepto para lo siguiente:

* No se incluye el modificador `partial`
* Los atributos de la declaración del método resultante son los atributos combinados de la definición de y la declaración de método parcial de implementación en un orden no especificado. No se quitan los duplicados.
* Los atributos de los parámetros de la declaración de método resultante son los atributos combinados de los parámetros correspondientes de la definición de y la declaración de método parcial de implementación en un orden no especificado. No se quitan los duplicados.

Si se proporciona una declaración de definición pero no una declaración de implementación para un método parcial M, se aplican las restricciones siguientes:

* Es un error en tiempo de compilación crear un delegado para el método ([expresiones de creación de delegado](expressions.md#delegate-creation-expressions)).
* Es un error en tiempo de compilación hacer referencia a `M` dentro de una función anónima que se convierte en un tipo de árbol[de expresión (evaluación de conversiones de funciones anónimas a tipos de árbol de expresión](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* Las expresiones que se producen como parte de una invocación de `M` no afectan al estado de asignación definitiva ([asignación definitiva](variables.md#definite-assignment)), lo que puede provocar errores en tiempo de compilación.
* `M` no puede ser el punto de entrada de una aplicación (inicio de la[aplicación](basic-concepts.md#application-startup)).

Los métodos parciales son útiles para permitir que una parte de una declaración de tipos Personalice el comportamiento de otra parte, por ejemplo, una generada por una herramienta. Considere la siguiente declaración de clase parcial:
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

Si esta clase se compila sin ningún otro elemento, se quitarán las declaraciones de método parcial de definición y sus invocaciones, y la declaración de clase combinada resultante será equivalente a la siguiente:
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

No obstante, supongamos que se proporciona otra parte, que proporciona declaraciones de implementación de los métodos parciales:
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

A continuación, la declaración de clase combinada resultante será equivalente a la siguiente:
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

### <a name="name-binding"></a>Enlace de nombre

Aunque cada parte de un tipo extensible se debe declarar dentro del mismo espacio de nombres, las partes se escriben normalmente en diferentes declaraciones de espacio de nombres. Por lo tanto, pueden estar presentes directivas de `using` diferentes ([directivas de uso](namespaces.md#using-directives)) para cada parte. Al interpretar nombres simples ([inferencia de tipos](expressions.md#type-inference)) dentro de una parte, solo se tienen en cuenta las directivas de `using` de las declaraciones de espacio de nombres que la forman. Esto puede dar lugar a que el mismo identificador tenga significados diferentes en distintas partes:
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

Los miembros de una clase se componen de los miembros introducidos por su *class_member_declaration*s y los miembros heredados de la clase base directa.

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
*  Campos, que son las variables de la clase ([campos](classes.md#fields)).
*  Los métodos, que implementan los cálculos y las acciones que puede realizar la clase ([métodos](classes.md#methods)).
*  Propiedades, que definen las características con nombre y las acciones asociadas a la lectura y escritura de esas características ([propiedades](classes.md#properties)).
*  Eventos, que definen las notificaciones que puede generar la clase ([eventos](classes.md#events)).
*  Indexadores, que permiten indizar las instancias de la clase de la misma manera (sintácticamente) que las matrices ([indizadores](classes.md#indexers)).
*  Operadores, que definen los operadores de expresión que se pueden aplicar a las instancias de la clase ([operadores](classes.md#operators)).
*  Constructores de instancias, que implementan las acciones necesarias para inicializar las instancias de la clase ([constructores de instancia](classes.md#instance-constructors))
*  Destructores, que implementan las acciones que se deben realizar antes de que las instancias de la clase se descartan de forma permanente ([destructores](classes.md#destructors)).
*  Constructores estáticos, que implementan las acciones necesarias para inicializar la propia clase ([constructores estáticos](classes.md#static-constructors)).
*  Tipos, que representan los tipos locales de la clase ([tipos anidados](classes.md#nested-types)).

Los miembros que pueden contener código ejecutable se conocen colectivamente como *miembros de función* del tipo de clase. Los miembros de función de un tipo de clase son los métodos, propiedades, eventos, indizadores, operadores, constructores de instancias, destructores y constructores estáticos de ese tipo de clase.

Un *class_declaration* crea un nuevo espacio de declaración ([declaraciones](basic-concepts.md#declarations)) y el *class_member_declaration*s que contiene inmediatamente el *class_declaration* introduce nuevos miembros en este espacio de declaración. Las siguientes reglas se aplican a *class_member_declaration*s:

*  Los constructores de instancias, destructores y constructores estáticos deben tener el mismo nombre que la clase de inclusión inmediata. Todos los demás miembros deben tener nombres distintos del nombre de la clase de inclusión inmediata.
*  El nombre de una constante, un campo, una propiedad, un evento o un tipo debe ser distinto de los nombres de todos los demás miembros declarados en la misma clase.
*  El nombre de un método debe ser distinto de los nombres de todos los demás métodos que no se declaran en la misma clase. Además, la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un método debe ser diferente de las firmas de todos los demás métodos declarados en la misma clase, y dos métodos declarados en la misma clase no pueden tener firmas que solo se diferencien por `ref` y `out`.
*  La firma de un constructor de instancia debe ser diferente de las firmas de todos los demás constructores de instancia declaradas en la misma clase, y dos constructores declarados en la misma clase no pueden tener firmas que solo difieran en `ref` y `out`.
*  La firma de un indizador debe ser diferente de las firmas de todos los demás indexadores declarados en la misma clase.
*  La firma de un operador debe ser diferente de las firmas de todos los demás operadores declarados en la misma clase.

Los miembros heredados de un tipo de clase ([herencia](classes.md#inheritance)) no forman parte del espacio de declaración de una clase. Por lo tanto, una clase derivada puede declarar un miembro con el mismo nombre o signatura que un miembro heredado (que en efecto oculta el miembro heredado).

### <a name="the-instance-type"></a>El tipo de instancia

Cada declaración de clase tiene un tipo enlazado asociado ([tipos enlazados y sin enlazar](types.md#bound-and-unbound-types)), el ***tipo de instancia***. En el caso de una declaración de clase genérica, el tipo de instancia se forma creando un tipo construido ([tipos construidos](types.md#constructed-types)) a partir de la declaración de tipos, y cada uno de los argumentos de tipo proporcionados es el parámetro de tipo correspondiente. Dado que el tipo de instancia utiliza los parámetros de tipo, solo se puede usar cuando los parámetros de tipo están en el ámbito; es decir, dentro de la declaración de clase. El tipo de instancia es el tipo de `this` para el código escrito dentro de la declaración de clase. En el caso de las clases no genéricas, el tipo de instancia es simplemente la clase declarada. A continuación se muestran varias declaraciones de clase junto con sus tipos de instancia: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Miembros de tipos construidos

Los miembros no heredados de un tipo construido se obtienen sustituyendo, por cada *type_parameter* en la declaración de miembro, el *type_argument* correspondiente del tipo construido. El proceso de sustitución se basa en el significado semántico de las declaraciones de tipos y no es simplemente una sustitución textual.

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

El tipo del miembro `a` en la declaración de clase genérica `Gen` es "matriz bidimensional de `T`", por lo que el tipo del miembro `a` en el tipo construido anterior es "matriz bidimensional de matriz unidimensional de `int`" o `int[,][]`.

Dentro de los miembros de la función de instancia, el tipo de `this` es el tipo de instancia ([el tipo de instancia](classes.md#the-instance-type)) de la declaración contenedora.

Todos los miembros de una clase genérica pueden usar parámetros de tipo de cualquier clase envolvente, ya sea directamente o como parte de un tipo construido. Cuando se usa un tipo construido cerrado determinado ([tipos abiertos y cerrados](types.md#open-and-closed-types)) en tiempo de ejecución, cada uso de un parámetro de tipo se reemplaza por el argumento de tipo real proporcionado al tipo construido. Por ejemplo:
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

Una clase ***hereda*** los miembros de su tipo de clase base directa. La herencia significa que una clase contiene implícitamente todos los miembros de su tipo de clase base directa, a excepción de los constructores de instancias, destructores y constructores estáticos de la clase base. Algunos aspectos importantes de la herencia son:

*  La herencia es transitiva. Si `C` se deriva de `B`y `B` se deriva de `A`, `C` hereda los miembros declarados en `B` así como los miembros declarados en `A`.
*  Una clase derivada extiende su clase base directa. Una clase derivada puede agregar nuevos miembros a aquellos de los que hereda, pero no puede quitar la definición de un miembro heredado.
*  Los constructores de instancias, destructores y constructores estáticos no se heredan, pero todos los demás miembros son, independientemente de su accesibilidad declarada ([acceso a miembros](basic-concepts.md#member-access)). Sin embargo, en función de la accesibilidad declarada, es posible que los miembros heredados no estén accesibles en una clase derivada.
*  Una clase derivada puede ***ocultar*** ([ocultar a través](basic-concepts.md#hiding-through-inheritance)de la herencia) los miembros heredados declarando nuevos miembros con el mismo nombre o signatura. Sin embargo, tenga en cuenta que ocultar un miembro heredado no quita ese miembro; simplemente hace que ese miembro no sea accesible directamente a través de la clase derivada.
*  Una instancia de una clase contiene un conjunto de todos los campos de instancia declarados en la clase y sus clases base, y existe una conversión implícita ([conversiones de referencia implícita](conversions.md#implicit-reference-conversions)) de un tipo de clase derivada a cualquiera de sus tipos de clase base. Por lo tanto, una referencia a una instancia de alguna clase derivada se puede tratar como una referencia a una instancia de cualquiera de sus clases base.
*  Una clase puede declarar métodos virtuales, propiedades e indizadores, y las clases derivadas pueden invalidar la implementación de estos miembros de función. Esto permite que las clases muestren un comportamiento polimórfico en el que las acciones realizadas por una invocación de miembro de función varían en función del tipo en tiempo de ejecución de la instancia a través de la cual se invoca a ese miembro de función.

El miembro heredado de un tipo de clase construido son los miembros del tipo de clase base inmediato ([clases base](classes.md#base-classes)), que se encuentra sustituyendo los argumentos de tipo del tipo construido por cada aparición de los parámetros de tipo correspondientes en la especificación *class_base* . Estos miembros, a su vez, se transforman sustituyendo, por cada *type_parameter* en la declaración de miembro, el *type_argument* correspondiente de la especificación de *class_base* .

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

En el ejemplo anterior, el tipo construido `D<int>` tiene un miembro no heredado `public int G(string s)` obtener sustituyendo el argumento de tipo `int` por el parámetro de tipo `T`. `D<int>` también tiene un miembro heredado de la declaración de clase `B`. Este miembro heredado se determina determinando primero el tipo de clase base `B<int[]>` de `D<int>` sustituyendo `int` por `T` en la especificación de clase base `B<T[]>`. A continuación, como un argumento de tipo para `B`, `int[]` se sustituye por `U` en `public U F(long index)`, lo que produce el miembro heredado `public int[] F(long index)`.

### <a name="the-new-modifier"></a>El nuevo modificador

Un *class_member_declaration* puede declarar un miembro con el mismo nombre o signatura que un miembro heredado. Cuando esto ocurre, se dice que el miembro de la clase derivada ***oculta*** el miembro de la clase base. Ocultar un miembro heredado no se considera un error, pero hace que el compilador emita una advertencia. Para suprimir la advertencia, la declaración del miembro de la clase derivada puede incluir un modificador `new` para indicar que el miembro derivado está pensado para ocultar el miembro base. Este tema se describe con más detalle en [ocultarse a través](basic-concepts.md#hiding-through-inheritance)de la herencia.

Si se incluye un modificador `new` en una declaración que no oculta un miembro heredado, se emite una advertencia para ese efecto. Esta advertencia se suprime quitando el modificador `new`.

### <a name="access-modifiers"></a>Modificadores de acceso

Un *class_member_declaration* puede tener cualquiera de los cinco tipos posibles de accesibilidad declarada ([accesibilidad declarada](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal`o `private`. A excepción de la combinación de `protected internal`, se trata de un error en tiempo de compilación para especificar más de un modificador de acceso. Cuando una *class_member_declaration* no incluye modificadores de acceso, se supone `private`.

### <a name="constituent-types"></a>Tipos constituyentes

Los tipos que se usan en la declaración de un miembro se denominan tipos constituyentes de ese miembro. Los tipos constituyentes posibles son el tipo de una constante, un campo, una propiedad, un evento o un indizador, el tipo de valor devuelto de un método o un operador, y los tipos de parámetro de un método, indizador, operador o constructor de instancia. Los tipos constituyentes de un miembro deben ser al menos tan accesibles como el propio miembro ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Miembros estáticos y de instancia

Los miembros de una clase son ***miembros estáticos*** o ***miembros de instancia***. En general, resulta útil pensar en que los miembros estáticos pertenecen a los tipos de clase y a los miembros de instancia como pertenecientes a objetos (instancias de tipos de clase).

Cuando un campo, método, propiedad, evento, operador o declaración de constructor incluye un modificador `static`, declara un miembro estático. Además, una declaración de constante o de tipo declara implícitamente un miembro estático. Los miembros estáticos tienen las siguientes características:

*  Cuando se hace referencia a un miembro estático `M` en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, `E` debe indicar un tipo que contenga `M`. Se trata de un error en tiempo de compilación para que `E` denote una instancia.
*  Un campo estático identifica exactamente una ubicación de almacenamiento que compartirán todas las instancias de un tipo de clase cerrada determinado. Independientemente del número de instancias de un tipo de clase cerrada determinado que se creen, solo hay una copia de un campo estático.
*  Un miembro de función estático (método, propiedad, evento, operador o constructor) no funciona en una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en este tipo de miembro de función.

Cuando un campo, un método, una propiedad, un evento, un indexador, un constructor o una declaración de destructor no incluye un modificador `static`, declara un miembro de instancia. (Un miembro de instancia se denomina a veces miembro no estático). Los miembros de instancia tienen las siguientes características:

*  Cuando se hace referencia a un miembro de instancia `M` en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, `E` debe indicar una instancia de un tipo que contenga `M`. Es un error en tiempo de enlace para que `E` denote un tipo.
*  Cada instancia de una clase contiene un conjunto independiente de todos los campos de instancia de la clase.
*  Un miembro de función de instancia (método, propiedad, indexador, constructor de instancia o destructor) opera en una instancia determinada de la clase y se puede tener acceso a esta instancia como `this` ([este acceso](expressions.md#this-access)).

En el ejemplo siguiente se muestran las reglas para tener acceso a los miembros estáticos y de instancia:
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

El método `F` muestra que en un miembro de función de instancia, se puede usar un *simple_name* ([nombres simples](expressions.md#simple-names)) para tener acceso a los miembros de instancia y a los miembros estáticos. El método `G` muestra que en un miembro de función estático, es un error en tiempo de compilación para tener acceso a un miembro de instancia a través de un *simple_name*. El método `Main` muestra que en un *member_access* ([acceso a miembros](expressions.md#member-access)), se debe tener acceso a los miembros de instancia a través de instancias de y se debe tener acceso a los miembros estáticos a través de los tipos.

### <a name="nested-types"></a>Tipos anidados

Un tipo declarado dentro de una declaración de clase o struct se denomina ***tipo anidado***. Un tipo que se declara dentro de una unidad de compilación o espacio de nombres se denomina ***tipo no anidado***.

en el ejemplo
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
la clase `B` es un tipo anidado porque se declara dentro de la clase `A`y la clase `A` es un tipo no anidado porque se declara dentro de una unidad de compilación.

#### <a name="fully-qualified-name"></a>Nombre completo

El nombre completo ([nombres](basic-concepts.md#fully-qualified-names)completos) de un tipo anidado es `S.N` donde `S` es el nombre completo del tipo en el que se declara `N` de tipo.

#### <a name="declared-accessibility"></a>Accesibilidad declarada

Los tipos no anidados pueden tener `public` o `internal` la accesibilidad declarada y tienen `internal` accesibilidad declarada de forma predeterminada. Los tipos anidados también pueden tener estas formas de accesibilidad declarada, además de una o varias formas adicionales de accesibilidad declarada, dependiendo de si el tipo contenedor es una clase o un struct:

*  Un tipo anidado que se declara en una clase puede tener cualquiera de las cinco formas de accesibilidad declarada (`public`, `protected internal`, `protected`, `internal`o `private`) y, al igual que otros miembros de clase, tiene como valor predeterminado `private` accesibilidad declarada.
*  Un tipo anidado que se declara en un struct puede tener cualquiera de las tres formas de accesibilidad declarada (`public`, `internal`o `private`) y, al igual que otros miembros de struct, tiene como valor predeterminado `private` accesibilidad declarada.

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

#### <a name="hiding"></a>Conde

Un tipo anidado puede ocultar ([ocultar nombres](basic-concepts.md#name-hiding)) un miembro base. El modificador `new` se permite en las declaraciones de tipos anidados para que la ocultación se pueda expresar explícitamente. El ejemplo
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
muestra una `M` de clase anidada que oculta el método `M` definido en `Base`.

#### <a name="this-access"></a>Este acceso

Un tipo anidado y su tipo contenedor no tienen una relación especial con respecto a *this_access* ([este acceso](expressions.md#this-access)). En concreto, `this` dentro de un tipo anidado no se puede usar para hacer referencia a los miembros de instancia del tipo contenedor. En los casos en los que un tipo anidado necesita tener acceso a los miembros de instancia de su tipo contenedor, se puede proporcionar acceso proporcionando el `this` para la instancia del tipo contenedor como argumento de constructor para el tipo anidado. En el ejemplo siguiente
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
muestra esta técnica. Una instancia de `C` crea una instancia de `Nested` y pasa su propia `this` al constructor de `Nested`para proporcionar el acceso posterior a los miembros de la instancia de `C`.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Acceso a los miembros privados y protegidos del tipo contenedor.

Un tipo anidado tiene acceso a todos los miembros a los que se puede tener acceso a su tipo contenedor, incluidos los miembros del tipo contenedor que tienen `private` y `protected` la accesibilidad declarada. El ejemplo
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
muestra una clase `C` que contiene una clase anidada `Nested`. Dentro de `Nested`, el método `G` llama al método estático `F` definido en `C`y `F` tiene una accesibilidad declarada privada.

Un tipo anidado también puede tener acceso a los miembros protegidos definidos en un tipo base de su tipo contenedor. en el ejemplo
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
la clase anidada `Derived.Nested` tiene acceso al método protegido `F` definido en la clase base de `Derived`, `Base`, llamando a a través de una instancia de `Derived`.

#### <a name="nested-types-in-generic-classes"></a>Tipos anidados en clases genéricas

Una declaración de clase genérica puede contener declaraciones de tipos anidados. Los parámetros de tipo de la clase envolvente se pueden usar dentro de los tipos anidados. Una declaración de tipos anidados puede contener parámetros de tipo adicionales que solo se aplican al tipo anidado.

Cada declaración de tipos contenida dentro de una declaración de clase genérica es implícitamente una declaración de tipos genéricos. Al escribir una referencia a un tipo anidado dentro de un tipo genérico, el tipo que contiene el tipo construido, incluidos sus argumentos de tipo, debe denominarse. Sin embargo, desde dentro de la clase externa, el tipo anidado se puede usar sin calificación; el tipo de instancia de la clase externa se puede usar implícitamente al construir el tipo anidado. En el ejemplo siguiente se muestran tres formas correctas de hacer referencia a un tipo construido creado a partir de `Inner`; los dos primeros son equivalentes:
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

Aunque el estilo de programación es incorrecto, un parámetro de tipo de un tipo anidado puede ocultar un miembro o un parámetro de tipo declarado en el tipo externo:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Nombres de miembros reservados

Para facilitar la implementación C# subyacente en tiempo de ejecución, para cada declaración de miembro de origen que sea una propiedad, un evento o un indexador, la implementación debe reservar dos firmas de método basadas en el tipo de declaración de miembro, su nombre y su tipo. Se trata de un error en tiempo de compilación para que un programa declare un miembro cuya firma coincide con una de estas firmas reservadas, aunque la implementación de tiempo de ejecución subyacente no use estas reservas.

Los nombres reservados no presentan declaraciones, por lo que no participan en la búsqueda de miembros. Sin embargo, las signaturas de método reservado asociadas a una declaración participan en la herencia ([herencia](classes.md#inheritance)) y se pueden ocultar con el modificador `new` ([el nuevo modificador](classes.md#the-new-modifier)).

La reserva de estos nombres sirve para tres propósitos:

*  Para permitir que la implementación subyacente use un identificador ordinario como un nombre de método para obtener o establecer el acceso a C# la característica de lenguaje.
*  Para permitir que otros lenguajes interoperen usando un identificador ordinario como un nombre de método para obtener o establecer el C# acceso a la característica de lenguaje.
*  Para asegurarse de que el origen aceptado por un compilador compatible lo acepta otro, haciendo que los detalles de los nombres de miembro reservados sean coherentes C# en todas las implementaciones.

La declaración de un destructor ([destructores) también](classes.md#destructors)hace que se Reserve una firma ([los nombres de miembro están reservados para los destructores](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Nombres de miembro reservados para propiedades

En el caso de una propiedad `P` ([propiedades](classes.md#properties)) de tipo `T`, se reservan las siguientes firmas:

```csharp
T get_P();
void set_P(T value);
```

Ambas firmas están reservadas, aunque la propiedad sea de solo lectura o de solo escritura.

en el ejemplo
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
una clase `A` define una propiedad de solo lectura `P`, de modo que se reservan firmas para los métodos `get_P` y `set_P`. Una clase `B` deriva de `A` y oculta ambas firmas reservadas. En el ejemplo se genera el resultado:
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Nombres de miembro reservados para eventos

Para un `E` de eventos ([eventos](classes.md#events)) de tipo delegado `T`, se reservan las siguientes firmas:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Nombres de miembro reservados para los indizadores

Para un indizador ([indexadores](classes.md#indexers)) de tipo `T` con `L`de lista de parámetros, se reservan las siguientes firmas:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Ambas firmas están reservadas, aunque el indizador sea de solo lectura o de solo escritura.

Además, el nombre de miembro `Item` está reservado.

#### <a name="member-names-reserved-for-destructors"></a>Nombres de miembro reservados para destructores

Para una clase que contiene un destructor[(](classes.md#destructors)destructores), se reserva la firma siguiente:
```csharp
void Finalize();
```

## <a name="constants"></a>Constantes

Una ***constante*** es un miembro de clase que representa un valor constante: un valor que se puede calcular en tiempo de compilación. Una *constant_declaration* introduce una o más constantes de un tipo determinado.

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

Un *constant_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), un modificador `new` ([el nuevo modificador](classes.md#the-new-modifier)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)). Los atributos y modificadores se aplican a todos los miembros declarados por el *constant_declaration*. Aunque las constantes se consideran miembros estáticos, una *constant_declaration* no requiere ni permite un modificador `static`. Es un error que el mismo modificador aparezca varias veces en una declaración de constante.

El *tipo* de un *constant_declaration* especifica el tipo de los miembros introducidos por la declaración. El tipo va seguido de una lista de *constant_declarator*s, cada uno de los cuales incorpora un nuevo miembro. Un *constant_declarator* consta de un *identificador* que nombra el miembro, seguido de un token "`=`", seguido de un *constant_expression* ([expresiones constantes](expressions.md#constant-expressions)) que proporciona el valor del miembro.

El *tipo* especificado en una declaración de constante debe ser `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *enum_type*o *reference_type*. Cada *constant_expression* debe producir un valor del tipo de destino o de un tipo que se pueda convertir al tipo de destino mediante una conversión implícita ([conversiones implícitas](conversions.md#implicit-conversions)).

El *tipo* de una constante debe ser al menos tan accesible como la propia constante ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

El valor de una constante se obtiene en una expresión usando un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)).

Una constante puede participar en una *constant_expression*. Por lo tanto, se puede utilizar una constante en cualquier construcción que requiera una *constant_expression*. Algunos ejemplos de estas construcciones son `case` etiquetas, `goto case` instrucciones, `enum` declaraciones de miembros, atributos y otras declaraciones de constantes.

Como se describe en [expresiones constantes](expressions.md#constant-expressions), una *constant_expression* es una expresión que se puede evaluar por completo en tiempo de compilación. Dado que la única forma de crear un valor que no sea NULL de una *reference_type* distinta de `string` es aplicar el operador `new` y, dado que no se permite el operador `new` en una *constant_expression*, el único valor posible para las constantes de *reference_type*s distinto de `string` es `null`.

Cuando se desea un nombre simbólico para un valor constante, pero cuando el tipo de ese valor no se permite en una declaración de constante, o cuando el valor no se puede calcular en tiempo de compilación mediante un *constant_expression*, se puede usar en su lugar un campo `readonly` ([campos de solo lectura](classes.md#readonly-fields)).

Una declaración de constante que declara varias constantes es equivalente a varias declaraciones de constantes únicas con los mismos atributos, modificadores y tipo. Por ejemplo
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

Las constantes pueden depender de otras constantes en el mismo programa, siempre que las dependencias no sean de naturaleza circular. El compilador se organiza automáticamente para evaluar las declaraciones de constantes en el orden adecuado. en el ejemplo
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
en primer lugar, el compilador evalúa `A.Y`, evalúa `B.Z`y, por último, evalúa `A.X`y genera los valores `10`, `11`y `12`. Las declaraciones de constantes pueden depender de constantes de otros programas, pero dichas dependencias solo son posibles en una dirección. Si se hace referencia al ejemplo anterior, si `A` y `B` se declararon en programas independientes, sería posible que `A.X` dependa de `B.Z`, pero `B.Z` podría no depender simultáneamente de `A.Y`.

## <a name="fields"></a>Campos

Un ***campo*** es un miembro que representa una variable asociada con un objeto o una clase. Un *field_declaration* introduce uno o más campos de un tipo determinado.

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

Un *field_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), un modificador `new` ([el nuevo modificador](classes.md#the-new-modifier)), una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)) y un modificador `static` ([campos estáticos y de instancia](classes.md#static-and-instance-fields)). Además, un *field_declaration* puede incluir un modificador `readonly` ([campos ReadOnly](classes.md#readonly-fields)) o un modificador `volatile` ([campos volátiles](classes.md#volatile-fields)), pero no ambos. Los atributos y modificadores se aplican a todos los miembros declarados por el *field_declaration*. Es un error que el mismo modificador aparezca varias veces en una declaración de campo.

El *tipo* de un *field_declaration* especifica el tipo de los miembros introducidos por la declaración. El tipo va seguido de una lista de *variable_declarator*s, cada uno de los cuales incorpora un nuevo miembro. Un *variable_declarator* consta de un *identificador* que asigna un nombre a ese miembro, seguido opcionalmente por un token "`=`" y un *variable_initializer* ([inicializadores de variables](classes.md#variable-initializers)) que proporciona el valor inicial de ese miembro.

El *tipo* de un campo debe ser al menos igual de accesible que el propio campo ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

El valor de un campo se obtiene en una expresión usando un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)). El valor de un campo que no es de solo lectura se modifica mediante una *asignación* ([operadores de asignación](expressions.md#assignment-operators)). El valor de un campo que no es de solo lectura se puede obtener y modificar mediante operadores de incremento y decremento postfijos ([operadores de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)postfijo) y operadores de incremento y decremento prefijos ([operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)).

Una declaración de campo que declara varios campos es equivalente a varias declaraciones de campos únicos con los mismos atributos, modificadores y tipo. Por ejemplo
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

Cuando una declaración de campo incluye un modificador `static`, los campos introducidos por la declaración son ***campos estáticos***. Cuando no hay ningún modificador `static`, los campos introducidos por la declaración son ***campos de instancia***. Los campos estáticos y los campos de instancia son dos de los diversos tipos de variables ([Variables](variables.md)) que admite C# y, en ocasiones, se hace referencia a ellas como ***variables estáticas*** y ***variables de instancia***, respectivamente.

Los campos estáticos no forman parte de una instancia específica; en su lugar, se comparte entre todas las instancias de un tipo cerrado ([tipos abiertos y cerrados](types.md#open-and-closed-types)). Independientemente del número de instancias de un tipo de clase cerrada, solo hay una copia de un campo estático para el dominio de aplicación asociado.

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

Un campo de instancia pertenece a una instancia de. En concreto, cada instancia de una clase contiene un conjunto independiente de todos los campos de instancia de esa clase.

Cuando se hace referencia a un campo en un *member_access* ([acceso a miembros](expressions.md#member-access)) con el formato `E.M`, si `M` es un campo estático, `E` debe indicar un tipo que contiene `M`y, si `M` es un campo de instancia, E debe indicar una instancia de un tipo que contenga `M`.

Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).

### <a name="readonly-fields"></a>Campos de solo lectura

Cuando una *field_declaration* incluye un modificador `readonly`, los campos introducidos por la declaración son ***campos de solo lectura***. Las asignaciones directas a campos de solo lectura solo se pueden producir como parte de la declaración o en un constructor de instancia o en un constructor estático de la misma clase. (Un campo de solo lectura se puede asignar a varias veces en estos contextos). En concreto, las asignaciones directas a un campo `readonly` solo se permiten en los contextos siguientes:

*  En el *variable_declarator* que presenta el campo (incluyendo un *variable_initializer* en la declaración).
*  Para un campo de instancia, en los constructores de instancia de la clase que contiene la declaración de campo; para un campo estático, en el constructor estático de la clase que contiene la declaración de campo. Estos son también los únicos contextos en los que es válido pasar un campo de `readonly` como parámetro `out` o `ref`.

Un error en tiempo de compilación intenta asignar a un campo de `readonly` o pasarlo como un parámetro `out` o `ref` en cualquier otro contexto.

#### <a name="using-static-readonly-fields-for-constants"></a>Usar campos de solo lectura estáticos para constantes

Un campo `static readonly` es útil cuando se desea un nombre simbólico para un valor constante, pero cuando el tipo del valor no está permitido en una declaración de `const` o cuando el valor no se puede calcular en tiempo de compilación. en el ejemplo
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
los miembros `Black`, `White`, `Red`, `Green`y `Blue` no se pueden declarar como miembros de `const` porque sus valores no se pueden calcular en tiempo de compilación. Sin embargo, si se declaran `static readonly` en su lugar, se produce el mismo efecto.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Control de versiones de constantes y campos de solo lectura estáticos

Las constantes y los campos de solo lectura tienen una semántica de versiones binaria diferente. Cuando una expresión hace referencia a una constante, el valor de la constante se obtiene en tiempo de compilación, pero cuando una expresión hace referencia a un campo de solo lectura, el valor del campo no se obtiene hasta el tiempo de ejecución. Considere una aplicación que consta de dos programas independientes:
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

Los espacios de nombres `Program1` y `Program2` denotan dos programas que se compilan por separado. Dado que `Program1.Utils.X` se declara como un campo estático de solo lectura, la salida del valor de la instrucción `Console.WriteLine` no se conoce en tiempo de compilación, sino que se obtiene en tiempo de ejecución. Por lo tanto, si se cambia el valor de `X` y `Program1` se vuelve a compilar, la instrucción `Console.WriteLine` generará el nuevo valor incluso si `Program2` no se vuelve a compilar. Sin embargo, si `X` era una constante, el valor de `X` se habría obtenido en el momento en que se compiló `Program2` y no se vería afectado por los cambios en `Program1` hasta que `Program2` se vuelva a compilar.

### <a name="volatile-fields"></a>Campos volátiles

Cuando una *field_declaration* incluye un modificador `volatile`, los campos introducidos por esa declaración son ***campos volátiles***.

En el caso de los campos no volátiles, las técnicas de optimización que reordenan las instrucciones pueden provocar resultados inesperados e imprevisibles en programas multiproceso que tienen acceso a campos sin sincronización, como los que proporciona el *lock_statement* ([la instrucción lock](statements.md#the-lock-statement)). Estas optimizaciones las puede realizar el compilador, el sistema en tiempo de ejecución o el hardware. En el caso de los campos volátiles, las optimizaciones de reordenación están restringidas:

*  Una lectura de un campo volátil se denomina ***lectura volátil***. Una lectura volátil tiene la "semántica de adquisición"; es decir, se garantiza que se produce antes de las referencias a la memoria que se producen después de ella en la secuencia de instrucciones.
*  La escritura de un campo volátil se denomina ***escritura volátil***. Una escritura volátil tiene "semántica de versión"; es decir, se garantiza que se produce después de cualquier referencia de memoria antes de la instrucción de escritura en la secuencia de instrucciones.

Estas restricciones aseguran que todos los subprocesos observarán las operaciones de escritura volátiles realizadas por cualquier otro subproceso en el orden en que se realizaron. No se requiere una implementación compatible para proporcionar una ordenación total única de escrituras volátiles, como se aprecia en todos los subprocesos de ejecución. El tipo de un campo volátil debe ser uno de los siguientes:

*  *Reference_type*.
*  Tipo `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`o `System.UIntPtr`.
*  *Enum_type* que tiene un tipo base de enumeración de `byte`, `sbyte`, `short`, `ushort`, `int`o `uint`.

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
```console
result = 143
```

En este ejemplo, el método `Main` inicia un nuevo subproceso que ejecuta el método `Thread2`. Este método almacena un valor en un campo no volátil denominado `result`y, a continuación, almacena `true` en el campo volatile `finished`. El subproceso principal espera a que el campo `finished` se establezca en `true`y, a continuación, lee el campo `result`. Como `finished` se ha declarado `volatile`, el subproceso principal debe leer el valor `143` de la `result`de campo. Si el `finished` de campo no se hubiera declarado `volatile`, se permitiría que el almacén `result` fuera visible en el subproceso principal después del almacén para `finished`y, por lo tanto, para que el subproceso principal Lea el valor `0` del campo `result`. Al declarar `finished` como un campo `volatile` se evita cualquier incoherencia.

### <a name="field-initialization"></a>Inicialización de campos

El valor inicial de un campo, ya sea un campo estático o un campo de instancia, es el valor predeterminado ([valores predeterminados](variables.md#default-values)) del tipo de campo. No es posible observar el valor de un campo antes de que se haya producido esta inicialización predeterminada y, por tanto, un campo nunca es "no inicializado". El ejemplo
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
```console
b = False, i = 0
```
Dado que `b` y `i` se inicializan automáticamente en los valores predeterminados.

### <a name="variable-initializers"></a>Inicializadores de variables

Las declaraciones de campo pueden incluir *variable_initializer*s. En los campos estáticos, los inicializadores de variables corresponden a las instrucciones de asignación que se ejecutan durante la inicialización de la clase. En el caso de los campos de instancia, los inicializadores de variables corresponden a las instrucciones de asignación que se ejecutan cuando se crea una instancia de la clase.

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
```console
x = 1.4142135623731, i = 100, s = Hello
```
Dado que una asignación a `x` tiene lugar cuando se ejecutan inicializadores de campo estáticos y las asignaciones a `i` y `s` se producen cuando se ejecutan los inicializadores de campo de instancia.

La inicialización del valor predeterminado que se describe en [inicialización de campos](classes.md#field-initialization) se produce para todos los campos, incluidos los campos que tienen inicializadores variables. Por lo tanto, cuando se inicializa una clase, todos los campos estáticos de esa clase se inicializan primero a sus valores predeterminados y, a continuación, los inicializadores de campo estáticos se ejecutan en orden textual. Del mismo modo, cuando se crea una instancia de una clase, todos los campos de instancia de esa instancia se inicializan por primera vez con sus valores predeterminados y, a continuación, los inicializadores de campo de instancia se ejecutan en orden textual.

Es posible que se respeten los campos estáticos con inicializadores variables en su estado de valor predeterminado. Sin embargo, esto no se recomienda como cuestión de estilo. El ejemplo
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
exhibe este comportamiento. A pesar de las definiciones circulares de a y b, el programa es válido. Da como resultado la salida
```console
a = 1, b = 2
```
Dado que los campos estáticos `a` y `b` se inicializan en `0` (el valor predeterminado para `int`) antes de que se ejecuten sus inicializadores. Cuando se ejecuta el inicializador para `a`, el valor de `b` es cero y, por tanto, se inicializa `a` en `1`. Cuando se ejecuta el inicializador para `b`, el valor de `a` ya está `1`y, por tanto, `b` se inicializa en `2`.

#### <a name="static-field-initialization"></a>Inicialización de campos estáticos

Los inicializadores de variables de campo estáticos de una clase corresponden a una secuencia de asignaciones que se ejecutan en el orden textual en el que aparecen en la declaración de clase. Si un constructor estático ([constructores estáticos](classes.md#static-constructors)) existe en la clase, la ejecución de los inicializadores de campo estáticos se produce inmediatamente antes de ejecutar ese constructor estático. De lo contrario, los inicializadores de campo estáticos se ejecutan en un momento dependiente de la implementación antes del primer uso de un campo estático de esa clase. El ejemplo
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
puede producir la salida:
```console
Init A
Init B
1 1
```
o la salida:
```console
Init B
Init A
1 1
```
Dado que la ejecución del inicializador de `X`y del inicializador de `Y`puede producirse en cualquier orden; solo se restringen a las referencias a esos campos. Sin embargo, en el ejemplo:
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
la salida debe ser:
```console
Init B
Init A
1 1
```
Dado que las reglas para cuando los constructores estáticos se ejecutan (como se definen en [constructores estáticos](classes.md#static-constructors)), proporcionan el constructor estático de `B`(y, por tanto, los inicializadores de campo estáticos de `B`) se deben ejecutar antes de `A`constructores estáticos y los inicializadores de campo de.

#### <a name="instance-field-initialization"></a>Inicialización de campos de instancia

Los inicializadores de variable de campo de instancia de una clase corresponden a una secuencia de asignaciones que se ejecutan inmediatamente después de la entrada a cualquiera de los constructores de instancia ([inicializadores de constructor](classes.md#constructor-initializers)) de esa clase. Los inicializadores de variable se ejecutan en el orden textual en el que aparecen en la declaración de clase. El proceso de creación e inicialización de instancias de clase se describe con más detalle en [constructores de instancias](classes.md#instance-constructors).

Un inicializador de variable para un campo de instancia no puede hacer referencia a la instancia que se está creando. Por lo tanto, se trata de un error en tiempo de compilación para hacer referencia a `this` en un inicializador de variable, ya que se trata de un error en tiempo de compilación para que un inicializador de variable haga referencia a un miembro de instancia a través de un *simple_name*. en el ejemplo
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
el inicializador de variable para `y` produce un error en tiempo de compilación porque hace referencia a un miembro de la instancia que se va a crear.

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

Un *method_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso[(modificadores de acceso](classes.md#access-modifiers)), el `new` ([modificador nuevo](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` (métodos de[invalidación](classes.md#override-methods)), `sealed` (métodos[sellados](classes.md#sealed-methods)), `abstract` (métodos[abstractos](classes.md#abstract-methods)) y modificadores `extern` ([métodos](classes.md#external-methods)

Una declaración tiene una combinación válida de modificadores si se cumplen todas las condiciones siguientes:

*  La Declaración incluye una combinación válida de modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)).
*  La declaración no incluye el mismo modificador varias veces.
*  La Declaración incluye como máximo uno de los siguientes modificadores: `static`, `virtual`y `override`.
*  La Declaración incluye como máximo uno de los siguientes modificadores: `new` y `override`.
*  Si la Declaración incluye el modificador `abstract`, la declaración no incluye ninguno de los siguientes modificadores: `static`, `virtual`, `sealed` o `extern`.
*  Si la Declaración incluye el modificador `private`, la declaración no incluye ninguno de los siguientes modificadores: `virtual`, `override`o `abstract`.
*  Si la Declaración incluye el modificador `sealed`, la Declaración también incluye el modificador `override`.
*  Si la Declaración incluye el modificador `partial`, no incluye ninguno de los siguientes modificadores: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`o `extern`.

Un método que tiene el modificador `async` es una función asincrónica y sigue las reglas descritas en [funciones asincrónicas](classes.md#async-functions).

El *return_type* de una declaración de método especifica el tipo del valor calculado y devuelto por el método. El *return_type* se `void` si el método no devuelve un valor. Si la Declaración incluye el modificador `partial`, el tipo de valor devuelto debe ser `void`.

El *member_name* especifica el nombre del método. A menos que el método sea una implementación explícita de un miembro de interfaz ([implementaciones explícitas de miembros de interfaz](interfaces.md#explicit-interface-member-implementations)), el *member_name* es simplemente un *identificador*. En el caso de una implementación explícita de un miembro de interfaz, el *member_name* se compone de un *interface_type* seguido de un "`.`" y un *identificador*.

El *type_parameter_list* opcional especifica los parámetros de tipo del método ([parámetros de tipo](classes.md#type-parameters)). Si se especifica un *type_parameter_list* el método es un ***método genérico***. Si el método tiene un modificador `extern`, no se puede especificar un *type_parameter_list* .

El *formal_parameter_list* opcional especifica los parámetros del método ([parámetros de método](classes.md#method-parameters)).

Los *type_parameter_constraints_clause*opcionales especifican restricciones en parámetros de tipo individuales ([restricciones de parámetro de tipo](classes.md#type-parameter-constraints)) y solo se pueden especificar si también se proporciona un *type_parameter_list* y el método no tiene un modificador `override`.

El *return_type* y cada uno de los tipos a los que se hace referencia en el *formal_parameter_list* de un método deben ser al menos tan accesibles como el propio método ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

El *method_body* es un punto y coma, un ***cuerpo de instrucción*** o un ***cuerpo de expresión***. Un cuerpo de instrucción consta de un *bloque*, que especifica las instrucciones que se ejecutarán cuando se invoque el método. Un cuerpo de expresión consta de `=>` seguido de una *expresión* y un punto y coma, y denota una expresión única que se debe realizar cuando se invoca el método. 

En el caso de los métodos `abstract` y `extern`, el *method_body* consta simplemente de un punto y coma. Por `partial` métodos, el *method_body* puede constar de un punto y coma, un cuerpo de bloque o un cuerpo de expresión. En el caso de todos los demás métodos, el *method_body* es un cuerpo de bloque o un cuerpo de expresión.

Si el *method_body* consta de un punto y coma, la declaración no puede incluir el modificador `async`.

El nombre, la lista de parámetros de tipo y la lista de parámetros formales de un método definen la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) del método. En concreto, la firma de un método consta de su nombre, el número de parámetros de tipo y el número, modificadores y tipos de sus parámetros formales. Para estos propósitos, cualquier parámetro de tipo del método que se produce en el tipo de un parámetro formal se identifica no por su nombre, sino por su posición ordinal en la lista de argumentos de tipo del método. El tipo de valor devuelto no forma parte de la signatura de un método, ni tampoco los nombres de los parámetros de tipo o los parámetros formales.

El nombre de un método debe ser distinto de los nombres de todos los demás métodos que no se declaran en la misma clase. Además, la firma de un método debe ser diferente de las firmas de todos los demás métodos declarados en la misma clase y dos métodos declarados en la misma clase no pueden tener firmas que difieran únicamente en `ref` y `out`.

Los *type_parameter*s del método están en el ámbito de la *method_declaration*y se pueden usar para formar tipos en ese ámbito en *return_type*, *method_body*y *type_parameter_constraints_clause*s, pero no en *atributos*.

Todos los parámetros formales y los parámetros de tipo deben tener nombres diferentes.

### <a name="method-parameters"></a>Parámetros de método

Los parámetros de un método, si los hay, se declaran mediante el *formal_parameter_list*del método.

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

La lista de parámetros formales está formada por uno o varios parámetros separados por comas, de los cuales solo el último puede ser un *parameter_array*.

Un *fixed_parameter* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), un modificador `ref`, `out` o `this` opcional, un *tipo*, un *identificador* y un *default_argument*opcional. Cada *fixed_parameter* declara un parámetro del tipo especificado con el nombre especificado. El modificador `this` designa el método como un método de extensión y solo se permite en el primer parámetro de un método estático. Los métodos de extensión se describen con más detalle en [métodos de extensión](classes.md#extension-methods).

Un *fixed_parameter* con un *default_argument* se conoce como ***parámetro opcional***, mientras que un *fixed_parameter* sin *default_argument* es un ***parámetro necesario***. Es posible que un parámetro necesario no aparezca después de un parámetro opcional en un *formal_parameter_list*.

Un parámetro `ref` o `out` no puede tener una *default_argument*. La *expresión* de un *default_argument* debe ser una de las siguientes:

*  a *constant_expression*
*  una expresión con el formato `new S()` donde `S` es un tipo de valor
*  una expresión con el formato `default(S)` donde `S` es un tipo de valor

La *expresión* debe poder convertirse implícitamente mediante una identidad o una conversión que acepte valores NULL en el tipo del parámetro.

Si se producen parámetros opcionales en una declaración de método parcial de implementación ([métodos parciales](classes.md#partial-methods)), una implementación explícita de un miembro de interfaz ([implementaciones de miembros de interfaz explícitos](interfaces.md#explicit-interface-member-implementations)) o en una declaración de indexador de un solo parámetro ([indizadores](classes.md#indexers)), el compilador debe proporcionar una advertencia, ya que estos miembros nunca se pueden invocar de forma que permita omitir los argumentos.

Un *parameter_array* consta de un conjunto opcional de *atributos* ([atributos](attributes.md)), un modificador de `params`, un *array_type*y un *identificador*. Una matriz de parámetros declara un parámetro único del tipo de matriz especificado con el nombre especificado. El *array_type* de una matriz de parámetros debe ser un tipo de matriz unidimensional ([tipos de matriz](arrays.md#array-types)). En una invocación de método, una matriz de parámetros permite especificar un único argumento del tipo de matriz especificado o permite que se especifiquen cero o más argumentos del tipo de elemento de matriz. Las matrices de parámetros se describen con más detalle en [matrices de parámetros](classes.md#parameter-arrays).

Una *parameter_array* puede producirse después de un parámetro opcional, pero no puede tener un valor predeterminado, la omisión de los argumentos de un *parameter_array* daría lugar a la creación de una matriz vacía.

En el ejemplo siguiente se muestran diferentes tipos de parámetros:
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

En el *formal_parameter_list* de `M`, `i` es un parámetro ref necesario, `d` es un parámetro de valor obligatorio, `b`, `s`, `o` y `t` son parámetros de valor opcionales y `a` es una matriz de parámetros.

Una declaración de método crea un espacio de declaración independiente para los parámetros, los parámetros de tipo y las variables locales. Los nombres se introducen en este espacio de declaración mediante la lista de parámetros de tipo y la lista de parámetros formales del método y las declaraciones de variables locales en el *bloque* del método. Es un error que dos miembros de un espacio de declaración de método tengan el mismo nombre. Es un error que el espacio de declaración de método y el espacio de declaración de variable local de un espacio de declaración anidado contengan elementos con el mismo nombre.

Una invocación de método ([invocaciones de método](expressions.md#method-invocations)) crea una copia, específica de dicha invocación, de los parámetros formales y las variables locales del método, y la lista de argumentos de la invocación asigna valores o referencias a variables a los parámetros formales recién creados. Dentro del *bloque* de un método, se puede hacer referencia a los parámetros formales por sus identificadores en expresiones de *Simple_name* ([nombres simples](expressions.md#simple-names)).

Hay cuatro tipos de parámetros formales:

*  Parámetros de valor, que se declaran sin ningún modificador.
*  Parámetros de referencia, que se declaran con el modificador `ref`.
*  Parámetros de salida, que se declaran con el modificador `out`.
*  Matrices de parámetros, que se declaran con el modificador `params`.

Como se describe en [firmas y sobrecarga](basic-concepts.md#signatures-and-overloading), los modificadores `ref` y `out` forman parte de la firma de un método, pero el modificador `params` no lo es.

#### <a name="value-parameters"></a>Parámetros de valor

Un parámetro declarado sin modificadores es un parámetro de valor. Un parámetro de valor corresponde a una variable local que obtiene su valor inicial del argumento correspondiente proporcionado en la invocación del método.

Cuando un parámetro formal es un parámetro de valor, el argumento correspondiente en una invocación de método debe ser una expresión que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de parámetro formal.

Un método puede asignar nuevos valores a un parámetro de valor. Dichas asignaciones solo afectan a la ubicación de almacenamiento local representada por el parámetro de valor; no tienen ningún efecto en el argumento real proporcionado en la invocación del método.

#### <a name="reference-parameters"></a>Parámetros de referencia

Un parámetro declarado con un modificador `ref` es un parámetro de referencia. A diferencia de un parámetro de valor, un parámetro de referencia no crea una nueva ubicación de almacenamiento. En su lugar, un parámetro de referencia representa la misma ubicación de almacenamiento que la variable proporcionada como argumento en la invocación del método.

Cuando un parámetro formal es un parámetro de referencia, el argumento correspondiente en una invocación de método debe constar de la palabra clave `ref` seguido de un *variable_reference* ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) del mismo tipo que el parámetro formal. Una variable debe estar asignada definitivamente antes de que se pueda pasar como parámetro de referencia.

Dentro de un método, un parámetro de referencia se considera siempre asignado definitivamente.

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
```console
i = 2, j = 1
```

Para la invocación de `Swap` en `Main`, `x` representa `i` y `y` representa `j`. Por lo tanto, la invocación tiene el efecto de intercambiar los valores de `i` y `j`.

En un método que toma parámetros de referencia, es posible que varios nombres representen la misma ubicación de almacenamiento. en el ejemplo
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
la invocación de `F` en `G` pasa una referencia a `s` para `a` y `b`. Por lo tanto, para esa invocación, los nombres `s`, `a`y `b` hacen referencia a la misma ubicación de almacenamiento, y las tres asignaciones modifican el campo de instancia `s`.

#### <a name="output-parameters"></a>Parámetros de salida

Un parámetro declarado con un modificador `out` es un parámetro de salida. De forma similar a un parámetro de referencia, un parámetro de salida no crea una nueva ubicación de almacenamiento. En su lugar, un parámetro de salida representa la misma ubicación de almacenamiento que la variable proporcionada como argumento en la invocación del método.

Cuando un parámetro formal es un parámetro de salida, el argumento correspondiente en una invocación de método debe constar de la palabra clave `out` seguido de un *variable_reference* ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) del mismo tipo que el parámetro formal. No es necesario asignar definitivamente una variable antes de que se pueda pasar como parámetro de salida, pero después de una invocación en la que se pasó una variable como parámetro de salida, la variable se considera definitivamente asignada.

Dentro de un método, al igual que una variable local, un parámetro de salida se considera inicialmente sin asignar y debe asignarse definitivamente antes de usar su valor.

Todos los parámetros de salida de un método se deben asignar definitivamente antes de que el método devuelva.

Un método declarado como método parcial ([métodos parciales](classes.md#partial-methods)) o un iterador ([iteradores](classes.md#iterators)) no puede tener parámetros de salida.

Los parámetros de salida se usan normalmente en métodos que producen varios valores devueltos. Por ejemplo:
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

En el ejemplo se genera el resultado:
```console
c:\Windows\System\
hello.txt
```

Tenga en cuenta que las variables `dir` y `name` se pueden desasignar antes de que se pasen a `SplitPath`y que se consideran definitivamente asignadas después de la llamada.

#### <a name="parameter-arrays"></a>Matrices de parámetros

Un parámetro declarado con un modificador `params` es una matriz de parámetros. Si una lista de parámetros formales incluye una matriz de parámetros, debe ser el último parámetro de la lista y debe ser de un tipo de matriz unidimensional. Por ejemplo, los tipos `string[]` y `string[][]` se pueden usar como el tipo de una matriz de parámetros, pero el tipo `string[,]` no puede. No es posible combinar el modificador `params` con los modificadores `ref` y `out`.

Una matriz de parámetros permite especificar argumentos de una de estas dos maneras en una invocación de método:

*  El argumento dado para una matriz de parámetros puede ser una expresión única que se pueda convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de matriz de parámetros. En este caso, la matriz de parámetros actúa exactamente como un parámetro de valor.
*  Como alternativa, la invocación puede especificar cero o más argumentos para la matriz de parámetros, donde cada argumento es una expresión que se puede convertir implícitamente ([conversiones implícitas](conversions.md#implicit-conversions)) en el tipo de elemento de la matriz de parámetros. En este caso, la invocación crea una instancia del tipo de matriz de parámetros con una longitud que corresponde al número de argumentos, inicializa los elementos de la instancia de la matriz con los valores de argumento especificados y usa la instancia de matriz recién creada como el actual. argument.

A excepción de permitir un número variable de argumentos en una invocación, una matriz de parámetros es exactamente equivalente a un parámetro de valor ([parámetros de valor](classes.md#value-parameters)) del mismo tipo.

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
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

La primera invocación de `F` simplemente pasa el `a` de la matriz como un parámetro de valor. La segunda invocación de `F` crea automáticamente un `int[]` de cuatro elementos con los valores de elemento especificados y pasa esa instancia de la matriz como un parámetro de valor. Del mismo modo, la tercera invocación de `F` crea un `int[]` de elementos cero y pasa esa instancia como un parámetro de valor. Las invocaciones segunda y tercera son exactamente equivalentes a la escritura:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Al realizar la resolución de sobrecarga, un método con una matriz de parámetros puede ser aplicable en su forma normal o en su forma expandida ([miembro de función aplicable](expressions.md#applicable-function-member)). La forma expandida de un método solo está disponible si la forma normal del método no es aplicable y solo si un método aplicable con la misma signatura que la forma expandida no se ha declarado en el mismo tipo.

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
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

En el ejemplo, dos de las formas expandidas posibles del método con una matriz de parámetros ya están incluidas en la clase como métodos normales. Por lo tanto, estos formularios expandidos no se tienen en cuenta al realizar la resolución de sobrecarga, y las invocaciones del método primero y tercer, por tanto, seleccionan los métodos normales. Cuando una clase declara un método con una matriz de parámetros, no es raro incluir también algunos de los formularios expandidos como métodos normales. Al hacerlo, es posible evitar la asignación de una instancia de matriz que se produce cuando se invoca una forma expandida de un método con una matriz de parámetros.

Cuando se `object[]`el tipo de una matriz de parámetros, se produce una ambigüedad potencial entre la forma normal del método y la forma desgastada para un solo parámetro `object`. La razón de la ambigüedad es que una `object[]` es implícitamente convertible al tipo `object`. Sin embargo, la ambigüedad no presenta ningún problema, ya que se puede resolver mediante la inserción de una conversión si es necesario.

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
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

En la primera y última invocación de `F`, la forma normal de `F` es aplicable porque existe una conversión implícita desde el tipo de argumento al tipo de parámetro (ambos son del tipo `object[]`). Por lo tanto, la resolución de sobrecarga selecciona la forma normal de `F`y el argumento se pasa como un parámetro de valor normal. En la segunda y tercera invocación, la forma normal de `F` no es aplicable porque no existe ninguna conversión implícita del tipo de argumento al tipo de parámetro (el tipo `object` no se puede convertir implícitamente al tipo `object[]`). Sin embargo, la forma expandida de `F` es aplicable, por lo que se selecciona mediante la resolución de sobrecarga. Como resultado, la invocación crea un `object[]` de un elemento y el único elemento de la matriz se inicializa con el valor de argumento dado (que a su vez es una referencia a un `object[]`).

### <a name="static-and-instance-methods"></a>Métodos estáticos y de instancia

Cuando una declaración de método incluye un modificador `static`, se dice que se trata de un método estático. Cuando no existe ningún modificador `static`, se dice que el método es un método de instancia.

Un método estático no funciona en una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en un método estático.

Un método de instancia funciona en una instancia determinada de una clase y se puede tener acceso a esa instancia como `this` ([este acceso](expressions.md#this-access)).

Cuando se hace referencia a un método en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`, si `M` es un método estático, `E` debe indicar un tipo que contiene `M`y, si `M` es un método de instancia, `E` debe indicar una instancia de un tipo que contenga `M`.

Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).

### <a name="virtual-methods"></a>Métodos virtuales

Cuando una declaración de método de instancia incluye un modificador `virtual`, se dice que se trata de un método virtual. Cuando no existe ningún modificador `virtual`, se dice que el método es un método no virtual.

La implementación de un método no virtual es invariable: la implementación es la misma si se invoca el método en una instancia de la clase en la que se declara o una instancia de una clase derivada. Por el contrario, las clases derivadas pueden reemplazar la implementación de un método virtual. El proceso de reemplazar la implementación de un método virtual heredado se conoce como ***invalidar*** ese método ([métodos de invalidación](classes.md#override-methods)).

En una invocación de método virtual, el ***tipo en tiempo de ejecución*** de la instancia para la que tiene lugar la invocación determina la implementación de método real que se va a invocar. En una invocación de método no virtual, el ***tipo en tiempo de compilación*** de la instancia es el factor determinante. En términos precisos, cuando se invoca un método denominado `N` con una lista de argumentos `A` en una instancia con un tipo en tiempo de compilación `C` y un `R` de tipo en tiempo de ejecución (donde `R` es `C` o una clase derivada de `C`), la invocación se procesa de la siguiente manera:

*  En primer lugar, se aplica la resolución de sobrecarga a `C`, `N`y `A`para seleccionar un método concreto `M` del conjunto de métodos declarados en y heredados por `C`. Esto se describe en las [invocaciones de método](expressions.md#method-invocations).
*  A continuación, si `M` es un método no virtual, se invoca `M`.
*  De lo contrario, `M` es un método virtual y se invoca la implementación más derivada de `M` con respecto a `R`.

Para cada método virtual declarado en o heredado por una clase, existe una ***implementación más derivada*** del método con respecto a esa clase. La implementación más derivada de un método virtual `M` con respecto a una clase `R` se determina de la siguiente manera:

*  Si `R` contiene la declaración de introducción `virtual` de `M`, ésta es la implementación más derivada de `M`.
*  De lo contrario, si `R` contiene un `override` de `M`, se trata de la implementación más derivada de `M`.
*  De lo contrario, la implementación más derivada de `M` con respecto a `R` es la misma que la implementación más derivada de `M` con respecto a la clase base directa de `R`.

En el ejemplo siguiente se muestran las diferencias entre los métodos virtuales y los no virtuales:
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

En el ejemplo, `A` presenta un método no virtual `F` y un método virtual `G`. La clase `B` introduce un nuevo método no virtual `F`, lo que oculta el `F`heredado, y también invalida el `G`de método heredado. En el ejemplo se genera el resultado:
```console
A.F
B.F
B.G
B.G
```

Observe que la instrucción `a.G()` invoca `B.G`, no `A.G`. Esto se debe a que el tipo en tiempo de ejecución de la instancia (que es `B`), no el tipo en tiempo de compilación de la instancia (que es `A`), determina la implementación de método real que se va a invocar.

Dado que los métodos pueden ocultar métodos heredados, es posible que una clase contenga varios métodos virtuales con la misma firma. Esto no presenta un problema de ambigüedad, ya que todo menos el método más derivado está oculto. en el ejemplo
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
las clases `C` y `D` contienen dos métodos virtuales con la misma firma: los introducidos por `A` y los introducidos por `C`. El método introducido por `C` oculta el método heredado de `A`. Por lo tanto, la declaración de invalidación en `D` invalida el método introducido por `C`y no es posible que `D` invalide el método introducido por `A`. En el ejemplo se genera el resultado:
```console
B.F
B.F
D.F
D.F
```

Tenga en cuenta que es posible invocar el método virtual oculto mediante el acceso a una instancia de `D` a través de un tipo menos derivado en el que el método no está oculto.

### <a name="override-methods"></a>Métodos de invalidación

Cuando una declaración de método de instancia incluye un modificador `override`, se dice que el método es un ***método de invalidación***. Un método de invalidación invalida un método virtual heredado con la misma firma. Mientras que una declaración de método virtual introduce un método nuevo, una declaración de método de reemplazo especializa un método virtual heredado existente proporcionando una nueva implementación de ese método.

El método invalidado por una declaración de `override` se conoce como el ***método base invalidado***. Para un método de invalidación `M` declara en una `C`de clase, el método base invalidado se determina examinando cada tipo de clase base de `C`, empezando por el tipo de clase base directa de `C` y continuando con cada tipo de clase base directo sucesivo, hasta que en un tipo de clase base determinado se encuentre un método accesible que tenga la misma firma que `M` después de la sustitución de A la hora de localizar el método base invalidado, se considera que un método es accesible si se `public`, si se `protected`, si está `protected internal`, o si se `internal` y se declara en el mismo programa que `C`.

Se produce un error en tiempo de compilación a menos que se cumplan todas las condiciones siguientes para una declaración de invalidación:

*  Un método base invalidado se puede encontrar como se describió anteriormente.
*  Hay exactamente un método base invalidado de este tipo. Esta restricción solo tiene efecto si el tipo de clase base es un tipo construido en el que la sustitución de los argumentos de tipo hace que la firma de dos métodos sea la misma.
*  El método base invalidado es un método virtual, abstracto o de invalidación. En otras palabras, el método base invalidado no puede ser estático o no virtual.
*  El método base invalidado no es un método sellado.
*  El método de invalidación y el método base invalidado tienen el mismo tipo de valor devuelto.
*  La declaración de invalidación y el método base invalidado tienen la misma accesibilidad declarada. En otras palabras, una declaración de invalidación no puede cambiar la accesibilidad del método virtual. Sin embargo, si el método base invalidado es interno protegido y se declara en un ensamblado diferente al del ensamblado que contiene el método de invalidación, se debe proteger la accesibilidad declarada del método de invalidación.
*  La declaración de invalidación no especifica las cláusulas-de-parámetros de tipo. En su lugar, las restricciones se heredan del método base invalidado. Tenga en cuenta que las restricciones que son parámetros de tipo en el método invalidado se pueden reemplazar por argumentos de tipo en la restricción heredada. Esto puede dar lugar a restricciones que no son válidas cuando se especifican explícitamente, como tipos de valor o tipos sellados.

En el ejemplo siguiente se muestra cómo funcionan las reglas de reemplazo para las clases genéricas:
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

Una declaración de invalidación puede tener acceso al método base invalidado mediante un *base_access* ([acceso base](expressions.md#base-access)). en el ejemplo
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
la invocación de `base.PrintFields()` en `B` invoca el método `PrintFields` declarado en `A`. Un *base_access* deshabilita el mecanismo de invocación virtual y simplemente trata el método base como un método no virtual. Una vez que se ha escrito la invocación en `B` `((A)this).PrintFields()`, invocaría de forma recursiva el método `PrintFields` declarado en `B`, no el declarado en `A`, ya que `PrintFields` es virtual y se `((A)this)` el tipo en tiempo de ejecución de `B`.

Solo si se incluye un modificador `override`, un método puede reemplazar a otro método. En todos los demás casos, un método con la misma signatura que un método heredado simplemente oculta el método heredado. en el ejemplo
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
el método `F` de `B` no incluye un modificador `override` y, por tanto, no invalida el método `F` de `A`. En lugar de ello, el método `F` en `B` oculta el método en `A`y se indica una advertencia porque la declaración no incluye un modificador `new`.

en el ejemplo
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
el método `F` de `B` oculta el método de `F` virtual heredado de `A`. Dado que el nuevo `F` en `B` tiene acceso privado, su ámbito solo incluye el cuerpo de la clase de `B` y no se extiende a `C`. Por lo tanto, la declaración de `F` en `C` puede invalidar el `F` heredado de `A`.

### <a name="sealed-methods"></a>Métodos sellados

Cuando una declaración de método de instancia incluye un modificador `sealed`, se dice que se trata de un método ***sellado***. Si una declaración de método de instancia incluye el modificador `sealed`, también debe incluir el modificador `override`. El uso del modificador `sealed` impide que una clase derivada Reemplace el método.

en el ejemplo
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
la clase `B` proporciona dos métodos de invalidación: un método de `F` que tiene el modificador `sealed` y un método `G` que no lo hace. `B`el uso de la `modifier` sellada impide que `C` Reemplace `F`.

### <a name="abstract-methods"></a>Métodos abstractos

Cuando una declaración de método de instancia incluye un modificador `abstract`, se dice que ese método es un ***método abstracto***. Aunque un método abstracto también es implícitamente un método virtual, no puede tener el modificador `virtual`.

Una declaración de método abstracto presenta un nuevo método virtual, pero no proporciona una implementación de ese método. En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación mediante la invalidación de ese método. Dado que un método abstracto no proporciona ninguna implementación real, el *method_body* de un método abstracto simplemente consiste en un punto y coma.

Las declaraciones de método abstracto solo se permiten en clases abstractas ([clases abstractas](classes.md#abstract-classes)).

en el ejemplo
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
la clase `Shape` define la noción abstracta de un objeto de forma geométrica que puede dibujarse a sí mismo. El método `Paint` es abstracto porque no hay ninguna implementación predeterminada significativa. Las clases `Ellipse` y `Box` son implementaciones concretas de `Shape`. Dado que estas clases no son abstractas, son necesarias para invalidar el método `Paint` y proporcionar una implementación real.

Se trata de un error en tiempo de compilación para que un *base_access* ([acceso base](expressions.md#base-access)) haga referencia a un método abstracto. en el ejemplo
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
se ha detectado un error en tiempo de compilación para la invocación de `base.F()` porque hace referencia a un método abstracto.

Se permite que una declaración de método abstracto invalide un método virtual. Esto permite a una clase abstracta forzar la reimplementación del método en las clases derivadas y hace que la implementación original del método no esté disponible. en el ejemplo
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
la clase `A` declara un método virtual, la clase `B` invalida este método con un método abstracto y la clase `C` invalida el método abstracto para proporcionar su propia implementación.

### <a name="external-methods"></a>Métodos externos

Cuando una declaración de método incluye un modificador `extern`, se dice que se trata de un ***método externo***. Los métodos externos se implementan externamente, normalmente con un lenguaje C#distinto de. Dado que una declaración de método externo no proporciona ninguna implementación real, el *method_body* de un método externo consiste simplemente en un punto y coma. Es posible que un método externo no sea genérico.

El modificador `extern` se utiliza normalmente junto con un atributo `DllImport` ([interoperación con componentes com y Win32](attributes.md#interoperation-with-com-and-win32-components)), permitiendo que los métodos externos se implementen mediante archivos DLL (bibliotecas de vínculos dinámicos). El entorno de ejecución puede admitir otros mecanismos en los que se puedan proporcionar implementaciones de métodos externos.

Cuando un método externo incluye un atributo `DllImport`, la declaración del método también debe incluir un modificador `static`. En este ejemplo se muestra el uso del modificador `extern` y el atributo `DllImport`:
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

### <a name="partial-methods-recap"></a>Métodos parciales (Resumen)

Cuando una declaración de método incluye un modificador `partial`, se dice que se trata de un ***método parcial***. Los métodos parciales solo se pueden declarar como miembros de tipos parciales ([tipos parciales](classes.md#partial-types)) y están sujetos a una serie de restricciones. Los métodos parciales se describen con más detalle en [métodos parciales](classes.md#partial-methods).

### <a name="extension-methods"></a>Métodos de extensión

Cuando el primer parámetro de un método incluye el modificador `this`, se dice que se trata de un ***método de extensión***. Los métodos de extensión solo se pueden declarar en clases estáticas no anidadas no genéricas. El primer parámetro de un método de extensión no puede tener modificadores que no sean `this`y el tipo de parámetro no puede ser un tipo de puntero.

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

Un método de extensión es un método estático normal. Además, cuando la clase estática envolvente está en el ámbito, se puede invocar un método de extensión mediante la sintaxis de invocación de método de instancia ([invocaciones de método de extensión](expressions.md#extension-method-invocations)), mediante la expresión de receptor como primer argumento.

El programa siguiente utiliza los métodos de extensión declarados anteriormente:
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

El método `Slice` está disponible en el `string[]`y el método `ToInt32` está disponible en `string`, porque se han declarado como métodos de extensión. El significado del programa es el mismo que el que se indica a continuación, mediante llamadas ordinarias al método estático:
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

El *method_body* de una declaración de método consta de un cuerpo de bloque, un cuerpo de expresión o un punto y coma.

El ***tipo de resultado*** de un método es `void` si el tipo de valor devuelto es `void`, o si el método es Async y el tipo de valor devuelto es `System.Threading.Tasks.Task`. De lo contrario, el tipo de resultado de un método no asincrónico es el tipo de valor devuelto y el tipo de resultado de un método asincrónico con el tipo de valor devuelto `System.Threading.Tasks.Task<T>` es `T`.

Cuando un método tiene un tipo de resultado `void` y un cuerpo de bloque, `return` instrucciones ([la instrucción return](statements.md#the-return-statement)) del bloque no pueden especificar una expresión. Si la ejecución del bloque de un método void se completa normalmente (es decir, el control fluye fuera del final del cuerpo del método), ese método simplemente vuelve a su llamador actual.
    
Cuando un método tiene un resultado `void` y un cuerpo de expresión, la expresión `E` debe ser un *statement_expression*y el cuerpo es exactamente equivalente a un cuerpo de bloque del formulario `{ E; }`.
    
Cuando un método tiene un tipo de resultado no void y un cuerpo de bloque, cada instrucción `return` del bloque debe especificar una expresión que se pueda convertir implícitamente al tipo de resultado. No se puede tener acceso al extremo de un cuerpo de bloque de un método que devuelve un valor. Es decir, en un método que devuelve valores con un cuerpo de bloque, el control no puede fluir fuera del final del cuerpo del método.
    
Cuando un método tiene un tipo de resultado no void y un cuerpo de expresión, la expresión se debe poder convertir implícitamente al tipo de resultado y el cuerpo es exactamente equivalente a un cuerpo de bloque del formulario `{ return E; }`.
    
en el ejemplo
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
el método `F` que devuelve el valor produce un error en tiempo de compilación porque el control puede fluir fuera del final del cuerpo del método. Los métodos `G` y `H` son correctos porque todas las rutas de acceso de ejecución posibles terminan en una instrucción return que especifica un valor devuelto. El método `I` es correcto porque su cuerpo es equivalente a un bloque de instrucciones que solo contiene una instrucción return.

### <a name="method-overloading"></a>Sobrecarga de métodos

Las reglas de resolución de sobrecarga de método se describen en [inferencia de tipos](expressions.md#type-inference).

## <a name="properties"></a>Propiedades

Una ***propiedad*** es un miembro que proporciona acceso a una característica de un objeto o una clase. Entre los ejemplos de propiedades se incluyen la longitud de una cadena, el tamaño de una fuente, el título de una ventana, el nombre de un cliente, etc. Las propiedades son una extensión natural de los campos: ambos son miembros con nombre con tipos asociados y la sintaxis para tener acceso a campos y propiedades es la misma. Sin embargo, a diferencia de los campos, las propiedades no denotan ubicaciones de almacenamiento. Las propiedades tienen ***descriptores de acceso*** que especifican las instrucciones que se ejecutan cuando se leen o escriben sus valores. Por tanto, las propiedades proporcionan un mecanismo para asociar acciones con la lectura y escritura de los atributos de un objeto; Además, permiten calcular tales atributos.

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

Un *property_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso[(modificadores de acceso](classes.md#access-modifiers)), el `new` ([modificador nuevo](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` (métodos de[invalidación](classes.md#override-methods)), `sealed` (métodos[sellados](classes.md#sealed-methods)), `abstract` (métodos[abstractos](classes.md#abstract-methods)) y modificadores `extern` ([métodos](classes.md#external-methods)

Las declaraciones de propiedad están sujetas a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores.

El *tipo* de una declaración de propiedad especifica el tipo de la propiedad introducida por la declaración y el *member_name* especifica el nombre de la propiedad. A menos que la propiedad sea una implementación explícita de un miembro de interfaz, el *member_name* es simplemente un *identificador*. En el caso de una implementación explícita de un miembro de interfaz ([implementaciones de miembros de interfaz explícitos](interfaces.md#explicit-interface-member-implementations)), el *member_name* consta de un *interface_type* seguido de un "`.`" y un *identificador*.

El *tipo* de una propiedad debe ser al menos igual de accesible que la propiedad en sí ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

Un *property_body* puede consistir en un ***cuerpo de descriptor de acceso*** o en un ***cuerpo de expresión***. En el cuerpo de un descriptor de acceso, *accessor_declarations*, que se deben incluir en los tokens "`{`" y "`}`", declare los descriptores de acceso ([descriptores de acceso](classes.md#accessors)) de la propiedad. Los descriptores de acceso especifican las instrucciones ejecutables asociadas a la lectura y la escritura de la propiedad.

Un cuerpo de expresión que consta de `=>` seguido de una *expresión* `E` y un punto y coma es exactamente equivalente al cuerpo de la instrucción `{ get { return E; } }`y, por lo tanto, solo se puede usar para especificar las propiedades de solo captador en las que una sola expresión proporciona el resultado del captador.

Solo se puede proporcionar un *property_initializer* para una propiedad implementada automáticamente ([propiedades implementadas automáticamente](classes.md#automatically-implemented-properties)) y provoca la inicialización del campo subyacente de dichas propiedades con el valor especificado por la *expresión*.

Aunque la sintaxis para tener acceso a una propiedad es la misma que la de un campo, una propiedad no se clasifica como una variable. Por lo tanto, no es posible pasar una propiedad como `ref` o `out` argumento.

Cuando una declaración de propiedad incluye un modificador `extern`, se dice que la propiedad es una ***propiedad externa***. Dado que una declaración de propiedad externa no proporciona ninguna implementación real, cada una de sus *accessor_declarations* consta de un punto y coma.

### <a name="static-and-instance-properties"></a>Propiedades estáticas y de instancia

Cuando una declaración de propiedad incluye un modificador `static`, se dice que la propiedad es una ***propiedad estática***. Cuando no existe ningún modificador `static`, se dice que la propiedad es una ***propiedad de instancia***.

Una propiedad estática no está asociada a una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en los descriptores de acceso de una propiedad estática.

Una propiedad de instancia está asociada a una instancia determinada de una clase y se puede tener acceso a esa instancia como `this` ([este acceso](expressions.md#this-access)) en los descriptores de acceso de esa propiedad.

Cuando se hace referencia a una propiedad en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`, si `M` es una propiedad estática, `E` debe indicar un tipo que contiene `M`y, si `M` es una propiedad de instancia, E debe denotar una instancia de un tipo que contenga `M`.

Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).

### <a name="accessors"></a>Descriptores

El *accessor_declarations* de una propiedad especifica las instrucciones ejecutables asociadas a la lectura y escritura de esa propiedad.

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

Las declaraciones de descriptor de acceso constan de un *get_accessor_declaration*, un *set_accessor_declaration*o ambos. Cada declaración de descriptor de acceso está formada por el token `get` o `set` seguido de un *accessor_modifier* opcional y un *accessor_body*.

El uso de *accessor_modifier*s se rige por las siguientes restricciones:

*  Un *accessor_modifier* no se puede usar en una interfaz o en una implementación explícita de un miembro de interfaz.
*  En el caso de una propiedad o un indizador que no tiene ningún modificador `override`, solo se permite una *accessor_modifier* si la propiedad o indizador tiene un descriptor de acceso `get` y `set` y, a continuación, solo se permite en uno de esos descriptores de acceso.
*  En el caso de una propiedad o un indizador que incluya un modificador `override`, un descriptor de acceso debe coincidir con el *accessor_modifier*, si existe, del descriptor de acceso que se va a invalidar.
*  El *accessor_modifier* debe declarar una accesibilidad que sea estrictamente más restrictiva que la accesibilidad declarada de la propiedad o el propio indexador. Para ser precisos:
   * Si la propiedad o indizador tiene una accesibilidad declarada de `public`, el *accessor_modifier* puede ser `protected internal`, `internal`, `protected`o `private`.
   * Si la propiedad o indizador tiene una accesibilidad declarada de `protected internal`, el *accessor_modifier* puede ser `internal`, `protected`o `private`.
   * Si la propiedad o indizador tiene una accesibilidad declarada de `internal` o `protected`, se debe `private`la *accessor_modifier* .
   * Si la propiedad o indizador tiene una accesibilidad declarada de `private`, no se puede usar ningún *accessor_modifier* .

En el caso de las propiedades `abstract` y `extern`, el *accessor_body* para cada descriptor de acceso especificado es simplemente un punto y coma. Una propiedad no abstracta y no externa puede tener cada *accessor_body* ser un punto y coma, en cuyo caso es una ***propiedad implementada automáticamente*** ([propiedades implementadas automáticamente](classes.md#automatically-implemented-properties)). Una propiedad implementada automáticamente debe tener al menos un descriptor de acceso get. En el caso de los descriptores de acceso de cualquier otra propiedad no abstracta no externa, el *accessor_body* es un *bloque* que especifica las instrucciones que se van a ejecutar cuando se invoca el descriptor de acceso correspondiente.

Un descriptor de acceso `get` corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad. Excepto como destino de una asignación, cuando se hace referencia a una propiedad en una expresión, el descriptor de acceso `get` de la propiedad se invoca para calcular el valor de la propiedad ([valores de las expresiones](expressions.md#values-of-expressions)). El cuerpo de un descriptor de acceso `get` debe ajustarse a las reglas de los métodos que devuelven valores descritos en el [cuerpo del método](classes.md#method-body). En concreto, todas las instrucciones `return` en el cuerpo de un descriptor de acceso `get` deben especificar una expresión que se pueda convertir implícitamente al tipo de propiedad. Además, el punto de conexión de un descriptor de acceso `get` no debe ser accesible.

Un descriptor de acceso `set` corresponde a un método con un parámetro de valor único del tipo de propiedad y un tipo de valor devuelto `void`. El parámetro implícito de un descriptor de acceso `set` siempre se denomina `value`. Cuando se hace referencia a una propiedad como el destino de una asignación[(operadores de asignación](expressions.md#assignment-operators)), o como operando de `++` o `--` (operadores de[incremento y decremento](expressions.md#postfix-increment-and-decrement-operators)de [postfijo, operadores de incremento y decremento de prefijo](expressions.md#prefix-increment-and-decrement-operators)), el descriptor de acceso de `set` se invoca con un argumento (cuyo valor es el del lado derecho de la asignación o el operando del operador `++` o `--`) que proporciona el nuevo valor ([asignación simple](expressions.md#simple-assignment)). El cuerpo de un descriptor de acceso `set` debe ajustarse a las reglas de `void` métodos descritos en el [cuerpo del método](classes.md#method-body). En concreto, `return` instrucciones del cuerpo del descriptor de acceso `set` no pueden especificar una expresión. Dado que un descriptor de acceso `set` tiene implícitamente un parámetro denominado `value`, se trata de un error en tiempo de compilación para que una declaración de constante o variable local de un descriptor de acceso de `set` tenga ese nombre.

En función de la presencia o ausencia de los descriptores de acceso `get` y `set`, se clasifica una propiedad de la manera siguiente:

*  Una propiedad que incluye tanto un descriptor de acceso `get` como un descriptor de acceso de `set` se dice que es una propiedad ***de lectura y escritura*** .
*  Una propiedad que solo tiene un descriptor de acceso `get` se dice que es una propiedad ***de solo lectura*** . Es un error en tiempo de compilación que una propiedad de solo lectura sea el destino de una asignación.
*  Se dice que una propiedad que solo tiene un descriptor de acceso `set` es una propiedad ***de solo escritura*** . Excepto como destino de una asignación, se trata de un error en tiempo de compilación para hacer referencia a una propiedad de solo escritura en una expresión.

en el ejemplo
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
el control `Button` declara una propiedad de `Caption` pública. El descriptor de acceso `get` de la propiedad `Caption` devuelve la cadena almacenada en el campo de `caption` privada. El descriptor de acceso `set` comprueba si el nuevo valor es diferente del valor actual y, en ese caso, almacena el nuevo valor y vuelve a dibujar el control. Las propiedades suelen seguir el patrón mostrado anteriormente: el descriptor de acceso `get` simplemente devuelve un valor almacenado en un campo privado y el descriptor de acceso `set` modifica ese campo privado y, a continuación, realiza las acciones adicionales necesarias para actualizar por completo el estado del objeto.

Dado el `Button` clase anterior, el siguiente es un ejemplo de uso de la propiedad `Caption`:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

En este caso, el descriptor de acceso `set` se invoca asignando un valor a la propiedad y el descriptor de acceso `get` se invoca haciendo referencia a la propiedad en una expresión.

Los descriptores de acceso `get` y `set` de una propiedad no son miembros distintos y no es posible declarar los descriptores de acceso de una propiedad por separado. Como tal, no es posible que los dos descriptores de acceso de una propiedad de lectura y escritura tengan una accesibilidad diferente. El ejemplo
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
no declara una única propiedad de lectura y escritura. En su lugar, declara dos propiedades con el mismo nombre, una de solo lectura y otra de solo escritura. Dado que dos miembros declarados en la misma clase no pueden tener el mismo nombre, en el ejemplo se produce un error en tiempo de compilación.

Cuando una clase derivada declara una propiedad con el mismo nombre que una propiedad heredada, la propiedad derivada oculta la propiedad heredada con respecto a la lectura y la escritura. en el ejemplo
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
la propiedad `P` de `B` oculta la propiedad `P` en `A` con respecto a la lectura y la escritura. Por lo tanto, en las instrucciones
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
la asignación a `b.P` produce un error en tiempo de compilación, ya que la propiedad `P` de solo lectura de `B` oculta la propiedad `P` de solo escritura en `A`. Sin embargo, tenga en cuenta que se puede usar una conversión para tener acceso a la propiedad Hidden `P`.

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

En este caso, la clase `Label` usa dos campos `int`, `x` y `y`, para almacenar su ubicación. La ubicación se expone públicamente como un `X` y una propiedad `Y` y como una propiedad `Location` de tipo `Point`. Si, en una versión futura de `Label`, resulta más cómodo almacenar la ubicación como `Point` internamente, el cambio se puede realizar sin que afecte a la interfaz pública de la clase:
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

Si `x` y `y` en su lugar se `public readonly` campos, habría sido imposible realizar este cambio en la clase de `Label`.

Exponer el estado a través de las propiedades no es necesariamente menos eficaz que exponer los campos directamente. En concreto, cuando una propiedad no es virtual y contiene solo una pequeña cantidad de código, el entorno de ejecución puede reemplazar las llamadas a los descriptores de acceso con el código real de los descriptores de acceso. Este proceso se ***conoce como inserción y hace***que el acceso a la propiedad sea tan eficaz como el acceso al campo, pero conserva la mayor flexibilidad de las propiedades.

Dado que la invocación de un descriptor de acceso `get` es conceptualmente equivalente a leer el valor de un campo, se considera un estilo de programación incorrecto para que los descriptores de acceso `get` tengan efectos secundarios observables. en el ejemplo
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
el valor de la propiedad `Next` depende del número de veces que se ha accedido previamente a la propiedad. Por lo tanto, el acceso a la propiedad produce un efecto secundario observable y la propiedad debe implementarse como un método en su lugar.

La Convención "sin efectos secundarios" para los descriptores de acceso `get` no significa que los descriptores de acceso de `get` siempre se deben escribir para devolver los valores almacenados en los campos. En realidad, los descriptores de acceso `get` suelen calcular el valor de una propiedad mediante el acceso a varios campos o la invocación de métodos. Sin embargo, un descriptor de acceso de `get` diseñado correctamente no realiza ninguna acción que produzca cambios observables en el estado del objeto.

Las propiedades se pueden usar para retrasar la inicialización de un recurso hasta el momento en el que se hace referencia a él por primera vez. Por ejemplo:
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

La clase `Console` contiene tres propiedades, `In`, `Out`y `Error`, que representan los dispositivos de entrada, salida y error estándar, respectivamente. Al exponer estos miembros como propiedades, la clase `Console` puede retrasar su inicialización hasta que se usen realmente. Por ejemplo, al hacer referencia por primera vez a la propiedad `Out`, como en
```csharp
Console.Out.WriteLine("hello, world");
```
se crea el `TextWriter` subyacente para el dispositivo de salida. Pero si la aplicación no hace referencia a las propiedades `In` y `Error`, no se crea ningún objeto para esos dispositivos.

### <a name="automatically-implemented-properties"></a>Propiedades implementadas automáticamente

Una propiedad implementada automáticamente (o ***propiedad automática*** para abreviar) es una propiedad no abstracta no abstracta con cuerpos de descriptor de acceso solo de punto y coma. Las propiedades automáticas deben tener un descriptor de acceso get y, opcionalmente, tener un descriptor de acceso set.

Cuando se especifica una propiedad como una propiedad implementada automáticamente, un campo de respaldo oculto está disponible automáticamente para la propiedad y los descriptores de acceso se implementan para leer y escribir en ese campo de respaldo. Si la propiedad automática no tiene ningún descriptor de acceso set, el campo de respaldo se considera `readonly` ([campos de solo lectura](classes.md#readonly-fields)). Al igual que un campo de `readonly`, también se puede asignar una propiedad automática de solo captador en el cuerpo de un constructor de la clase envolvente. Este tipo de asignación asigna directamente al campo de respaldo de solo lectura de la propiedad.

Una propiedad automática puede tener opcionalmente un *property_initializer*, que se aplica directamente al campo de respaldo como un *variable_initializer* ([inicializadores de variables](classes.md#variable-initializers)).

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

Observe que las asignaciones al campo de solo lectura son válidas, ya que se producen dentro del constructor.


### <a name="accessibility"></a>Accesibilidad

Si un descriptor de acceso tiene un *accessor_modifier*, el dominio de accesibilidad ([dominios de accesibilidad](basic-concepts.md#accessibility-domains)) del descriptor de acceso se determina mediante la accesibilidad declarada de la *accessor_modifier*. Si un descriptor de acceso no tiene un *accessor_modifier*, el dominio de accesibilidad del descriptor de acceso se determina a partir de la accesibilidad declarada de la propiedad o del indizador.

La presencia de un *accessor_modifier* nunca afecta a la búsqueda de miembros ([operadores](expressions.md#operators)) o a la resolución de sobrecarga ([resolución de sobrecarga](expressions.md#overload-resolution)). Los modificadores de la propiedad o el indizador siempre determinan la propiedad o el indizador que está enlazado, independientemente del contexto del acceso.

Una vez que se ha seleccionado una propiedad o un indizador determinados, se usan los dominios de accesibilidad de los descriptores de acceso específicos implicados para determinar si ese uso es válido:

*  Si el uso es como un valor ([valores de expresiones](expressions.md#values-of-expressions)), el descriptor de acceso `get` debe existir y ser accesible.
*  Si el uso es como el destino de una asignación simple ([asignación simple](expressions.md#simple-assignment)), el descriptor de acceso `set` debe existir y ser accesible.
*  Si el uso es como destino de la asignación compuesta ([asignación compuesta](expressions.md#compound-assignment)) o como destino de los operadores `++` o `--` (miembros de[función](expressions.md#function-members)0,9, [expresiones de invocación](expressions.md#invocation-expressions)), los descriptores de acceso `get` y `set` deben existir y ser accesibles.

En el ejemplo siguiente, el `B.Text`de propiedad oculta la propiedad `A.Text`, incluso en contextos donde solo se llama al descriptor de acceso `set`. Por el contrario, la propiedad `B.Count` no es accesible para el `M`de clase, por lo que en su lugar se usa la propiedad accesible `A.Count`.

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

Un descriptor de acceso que se utiliza para implementar una interfaz no puede tener un *accessor_modifier*. Si solo se usa un descriptor de acceso para implementar una interfaz, el otro descriptor de acceso se puede declarar con un *accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Descriptores de acceso de propiedad virtual, Sealed, override y Abstract

Una declaración de propiedad `virtual` especifica que los descriptores de acceso de la propiedad son virtuales. El modificador `virtual` se aplica a ambos descriptores de acceso de una propiedad de lectura y escritura, no es posible que solo un descriptor de acceso de una propiedad de lectura y escritura sea virtual.

Una declaración de propiedad `abstract` especifica que los descriptores de acceso de la propiedad son virtuales, pero no proporciona una implementación real de los descriptores de acceso. En su lugar, se requiere que las clases derivadas no abstractas proporcionen su propia implementación para los descriptores de acceso mediante la invalidación de la propiedad. Dado que un descriptor de acceso de una declaración de propiedad abstracta no proporciona ninguna implementación real, su *accessor_body* simplemente consta de un punto y coma.

Una declaración de propiedad que incluye los modificadores `abstract` y `override` especifica que la propiedad es abstracta e invalida una propiedad base. Los descriptores de acceso de dicha propiedad también son abstractos.

Las declaraciones de propiedad abstracta solo se permiten en clases abstractas ([clases abstractas](classes.md#abstract-classes)). Los descriptores de acceso de una propiedad virtual heredada se pueden invalidar en una clase derivada mediante la inclusión de una declaración de propiedad que especifique una directiva de `override`. Esto se conoce como una ***declaración de propiedad de reemplazo***. Una declaración de propiedad de reemplazo no declara una nueva propiedad. En su lugar, simplemente especializa las implementaciones de los descriptores de acceso de una propiedad virtual existente.

Una declaración de propiedad de reemplazo debe especificar exactamente los mismos modificadores de accesibilidad, tipo y nombre que la propiedad heredada. Si la propiedad heredada solo tiene un descriptor de acceso (es decir, si la propiedad heredada es de solo lectura o de solo escritura), la propiedad de reemplazo solo debe incluir ese descriptor de acceso. Si la propiedad heredada incluye ambos descriptores de acceso (es decir, si la propiedad heredada es de lectura y escritura), la propiedad de reemplazo puede incluir un solo descriptor de acceso o ambos.

Una declaración de propiedad de reemplazo puede incluir el modificador `sealed`. El uso de este modificador evita que una clase derivada Reemplace la propiedad. Los descriptores de acceso de una propiedad sellada también están sellados.

A excepción de las diferencias en la sintaxis de declaración e invocación, los descriptores de acceso virtual, sellado, invalidación y Abstract se comportan exactamente igual que los métodos virtuales, sellados, de invalidación y abstractos. En concreto, las reglas descritas en [métodos virtuales](classes.md#virtual-methods), [métodos de invalidación](classes.md#override-methods), [métodos sellados](classes.md#sealed-methods)y [métodos abstractos](classes.md#abstract-methods) se aplican como si los descriptores de acceso fueran métodos de una forma correspondiente:

*  Un descriptor de acceso `get` corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad y los mismos modificadores que la propiedad que lo contiene.
*  Un descriptor de acceso `set` corresponde a un método con un parámetro de valor único del tipo de propiedad, un tipo de valor devuelto `void` y los mismos modificadores que la propiedad que lo contiene.

en el ejemplo
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
`X` es una propiedad virtual de solo lectura, `Y` es una propiedad de lectura y escritura virtual y `Z` es una propiedad de lectura y escritura abstracta. Dado que `Z` es abstracto, la clase contenedora `A` también debe declararse como abstracta.

A continuación se muestra una clase que deriva de `A`:
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

Aquí, las declaraciones de `X`, `Y`y `Z` invalidan las declaraciones de propiedad. Cada declaración de propiedad coincide exactamente con los modificadores de accesibilidad, el tipo y el nombre de la propiedad heredada correspondiente. El descriptor de acceso `get` de `X` y el descriptor de acceso `set` de `Y` utilizan la palabra clave `base` para tener acceso a los descriptores de acceso heredados La declaración de `Z` invalida ambos descriptores de acceso abstractos; por tanto, no hay miembros de función abstracta pendientes en `B`y se permite que `B` sea una clase no abstracta.

Cuando una propiedad se declara como un `override`, cualquier descriptor de acceso invalidado debe ser accesible para el código de invalidación. Además, la accesibilidad declarada de la propiedad o del indexador, y de los descriptores de acceso, debe coincidir con la del miembro y los descriptores de acceso invalidados. Por ejemplo:
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

Un ***evento*** es un miembro que permite a un objeto o una clase proporcionar notificaciones. Los clientes pueden adjuntar código ejecutable para eventos proporcionando ***controladores de eventos***.

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

Un *event_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso[(modificadores de acceso](classes.md#access-modifiers)), el `new` ([modificador nuevo](classes.md#the-new-modifier)), `static` ([métodos estáticos y de instancia](classes.md#static-and-instance-methods)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` (métodos de[invalidación](classes.md#override-methods)), `sealed` (métodos[sellados](classes.md#sealed-methods)), `abstract` (métodos[abstractos](classes.md#abstract-methods)) y modificadores `extern` ([métodos](classes.md#external-methods)

Las declaraciones de eventos están sujetas a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores.

El *tipo* de una declaración de evento debe ser un *delegate_type* ([tipos de referencia](types.md#reference-types)) y ese *delegate_type* debe ser al menos igual de accesible que el propio evento (restricciones de[accesibilidad](basic-concepts.md#accessibility-constraints)).

Una declaración de evento puede incluir *event_accessor_declarations*. Sin embargo, si no, para los eventos no abstractos y no externos, el compilador los proporciona automáticamente ([eventos similares a los campos](classes.md#field-like-events)); en el caso de los eventos extern, los descriptores de acceso se proporcionan externamente.

Una declaración de evento que omite *event_accessor_declarations* define uno o más eventos, uno para cada una de las *variable_declarator*s. Los atributos y modificadores se aplican a todos los miembros declarados por este tipo de *event_declaration*.

Se trata de un error en tiempo de compilación para que una *event_declaration* incluya tanto el modificador `abstract` como *event_accessor_declarations*delimitado por llaves.

Cuando una declaración de evento incluye un modificador `extern`, se dice que se trata de un ***evento externo***. Dado que una declaración de evento externo no proporciona ninguna implementación real, es un error que incluye el modificador `extern` y el *event_accessor_declarations*.

Se trata de un error en tiempo de compilación para una *variable_declarator* de una declaración de evento con un modificador `abstract` o `external` para incluir un *variable_initializer*.

Un evento se puede usar como operando izquierdo de los operadores `+=` y `-=` (asignación de[eventos](expressions.md#event-assignment)). Estos operadores se utilizan, respectivamente, para adjuntar controladores de eventos a o para quitar controladores de eventos de un evento, y los modificadores de acceso del evento controlan los contextos en los que se permiten esas operaciones.

Como `+=` y `-=` son las únicas operaciones que se permiten en un evento fuera del tipo que declara el evento, el código externo puede Agregar y quitar controladores para un evento, pero no puede obtener o modificar la lista subyacente de controladores de eventos.

En una operación con el formato `x += y` o `x -= y`, cuando `x` es un evento y la referencia tiene lugar fuera del tipo que contiene la declaración de `x`, el resultado de la operación tiene el tipo `void` (en lugar de tener el tipo de `x`, con el valor de `x` después de la asignación). Esta regla prohíbe que el código externo examine indirectamente el delegado subyacente de un evento.

En el ejemplo siguiente se muestra cómo se adjuntan los controladores de eventos a las instancias de la clase `Button`:
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

En este caso, el constructor de instancia de `LoginDialog` crea dos instancias de `Button` y asocia controladores de eventos a los eventos `Click`.

### <a name="field-like-events"></a>Eventos similares a campos

En el texto del programa de la clase o el struct que contiene la declaración de un evento, se pueden usar determinados eventos como campos. Para usarse de esta manera, un evento no debe estar `abstract` ni `extern`y no debe incluir explícitamente *event_accessor_declarations*. Este tipo de evento se puede utilizar en cualquier contexto que permita un campo. El campo contiene un delegado ([delegados](delegates.md)) que hace referencia a la lista de controladores de eventos que se han agregado al evento. Si no se ha agregado ningún controlador de eventos, el campo contiene `null`.

en el ejemplo
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
`Click` se utiliza como un campo dentro de la clase `Button`. Como muestra el ejemplo, el campo se puede examinar, modificar y usar en expresiones de invocación de delegado. El método `OnClick` de la clase `Button` "genera" el evento `Click`. La noción de generar un evento es equivalente exactamente a invocar el delegado representado por el evento; por lo tanto, no hay ninguna construcción especial de lenguaje para generar eventos. Tenga en cuenta que la invocación del delegado está precedida por una comprobación que garantiza que el delegado no es NULL.

Fuera de la declaración de la clase `Button`, el miembro de `Click` solo se puede usar en el lado izquierdo de los operadores `+=` y `-=`, como en
```csharp
b.Click += new EventHandler(...);
```
que anexa un delegado a la lista de invocaciones del evento `Click`, y
```csharp
b.Click -= new EventHandler(...);
```
que quita un delegado de la lista de invocaciones del evento `Click`.

Al compilar un evento como un campo, el compilador crea automáticamente el almacenamiento para contener el delegado y crea los descriptores de acceso para el evento que agregan o quitan controladores de eventos para el campo delegado. Las operaciones de adición y eliminación son seguras para subprocesos y puede (pero no es necesario que) se realicen mientras se mantiene el bloqueo ([la instrucción lock](statements.md#the-lock-statement)) en el objeto contenedor para un evento de instancia o el objeto de tipo ([expresiones de creación de objetos anónimos](expressions.md#anonymous-object-creation-expressions)) para un evento estático.

Por lo tanto, una declaración de evento de instancia con el formato:
```csharp
class X
{
    public event D Ev;
}
```
se compilará en algo equivalente a:
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
Dentro de la `X`de la clase, las referencias a `Ev` en el lado izquierdo de los operadores `+=` y `-=` provocan que se invoquen los descriptores de acceso add y Remove. Todas las demás referencias a `Ev` se compilan para hacer referencia al campo oculto `__Ev` en su lugar ([acceso a miembros](expressions.md#member-access)). El nombre "`__Ev`" es arbitrario; el campo oculto puede tener cualquier nombre o ningún nombre.

### <a name="event-accessors"></a>Descriptores de acceso de un evento

Las declaraciones de evento normalmente omiten *event_accessor_declarations*, como en el ejemplo de `Button` anterior. Una situación para hacerlo implica el caso en el que el costo de almacenamiento de un campo por evento no es aceptable. En tales casos, una clase puede incluir *event_accessor_declarations* y utilizar un mecanismo privado para almacenar la lista de controladores de eventos.

En el *event_accessor_declarations* de un evento se especifican las instrucciones ejecutables asociadas a la adición y eliminación de controladores de eventos.

Las declaraciones de descriptor de acceso constan de un *add_accessor_declaration* y un *remove_accessor_declaration*. Cada declaración de descriptor de acceso está formada por el token `add` o `remove` seguido de un *bloque*. El *bloque* asociado a un *add_accessor_declaration* especifica las instrucciones que se ejecutarán cuando se agregue un controlador de eventos y el *bloque* asociado a un *remove_accessor_declaration* especifica las instrucciones que se ejecutarán cuando se quite un controlador de eventos.

Cada *add_accessor_declaration* y *remove_accessor_declaration* corresponde a un método con un parámetro de valor único del tipo de evento y un tipo de valor devuelto `void`. El parámetro implícito de un descriptor de acceso de eventos se denomina `value`. Cuando se utiliza un evento en una asignación de eventos, se usa el descriptor de acceso de eventos adecuado. En concreto, si el operador de asignación es `+=`, se usa el descriptor de acceso add y, si el operador de asignación es `-=`, se usa el descriptor de acceso Remove. En cualquier caso, el operando derecho del operador de asignación se utiliza como argumento para el descriptor de acceso de eventos. El bloque de una *add_accessor_declaration* o un *remove_accessor_declaration* debe ajustarse a las reglas de los métodos de `void` descritos en el [cuerpo del método](classes.md#method-body). En concreto, `return` instrucciones de este tipo de bloque no pueden especificar una expresión.

Dado que un descriptor de acceso de eventos tiene implícitamente un parámetro denominado `value`, es un error en tiempo de compilación para una variable local o una constante declarada en un descriptor de acceso de eventos para que tenga ese nombre.

en el ejemplo
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
la clase `Control` implementa un mecanismo de almacenamiento interno para los eventos. El método `AddEventHandler` asocia un valor de delegado a una clave, el método `GetEventHandler` devuelve el delegado asociado actualmente a una clave y el método `RemoveEventHandler` quita un delegado como controlador de eventos para el evento especificado. Presumiblemente, el mecanismo de almacenamiento subyacente está diseñado de forma que no hay ningún costo por asociar un `null` valor de delegado con una clave y, por lo tanto, los eventos no controlados no consumen almacenamiento.

### <a name="static-and-instance-events"></a>Eventos estáticos y de instancia

Cuando una declaración de evento incluye un modificador `static`, se dice que se trata de un ***evento estático***. Cuando no hay ningún modificador `static`, se dice que el evento es un ***evento de instancia***.

Un evento estático no está asociado a una instancia específica y es un error en tiempo de compilación para hacer referencia a `this` en los descriptores de acceso de un evento estático.

Un evento de instancia está asociado a una instancia determinada de una clase y se puede tener acceso a esta instancia como `this` ([este acceso](expressions.md#this-access)) en los descriptores de acceso de ese evento.

Cuando se hace referencia a un evento en un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `E.M`, si `M` es un evento estático, `E` debe indicar un tipo que contiene `M`y, si `M` es un evento de instancia, E debe denotar una instancia de un tipo que contenga `M`.

Las diferencias entre los miembros estáticos y de instancia se tratan más adelante en [miembros estáticos y de instancia](classes.md#static-and-instance-members).

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Descriptores de acceso de eventos virtuales, sellados, de invalidación y abstractos

Una declaración de evento `virtual` especifica que los descriptores de acceso de ese evento son virtuales. El modificador `virtual` se aplica a ambos descriptores de acceso de un evento.

Una declaración de evento `abstract` especifica que los descriptores de acceso del evento son virtuales, pero no proporciona una implementación real de los descriptores de acceso. En su lugar, las clases derivadas no abstractas deben proporcionar su propia implementación para los descriptores de acceso invalidando el evento. Dado que una declaración de evento abstracto no proporciona ninguna implementación real, no puede proporcionar *event_accessor_declarations*delimitados por llaves.

Una declaración de evento que incluye los modificadores `abstract` y `override` especifica que el evento es abstracto e invalida un evento base. Los descriptores de acceso de este tipo de evento también son abstractos.

Las declaraciones de eventos abstractos solo se permiten en clases abstractas ([clases abstractas](classes.md#abstract-classes)).

Los descriptores de acceso de un evento virtual heredado se pueden invalidar en una clase derivada mediante la inclusión de una declaración de evento que especifique un modificador `override`. Esto se conoce como una ***declaración de evento de reemplazo***. Una declaración de evento de reemplazo no declara un nuevo evento. En su lugar, simplemente especializa las implementaciones de los descriptores de acceso de un evento virtual existente.

Una declaración de evento de reemplazo debe especificar exactamente los mismos modificadores de accesibilidad, tipo y nombre que el evento invalidado.

Una declaración de evento de reemplazo puede incluir el modificador `sealed`. El uso de este modificador evita que una clase derivada Reemplace el evento. Los descriptores de acceso de un evento sellado también están sellados.

Es un error en tiempo de compilación que una declaración de evento de reemplazo incluya un modificador `new`.

A excepción de las diferencias en la sintaxis de declaración e invocación, los descriptores de acceso virtual, sellado, invalidación y Abstract se comportan exactamente igual que los métodos virtuales, sellados, de invalidación y abstractos. En concreto, las reglas descritas en [métodos virtuales](classes.md#virtual-methods), [métodos de invalidación](classes.md#override-methods), [métodos sellados](classes.md#sealed-methods)y [métodos abstractos](classes.md#abstract-methods) se aplican como si los descriptores de acceso fueran métodos de un formulario correspondiente. Cada descriptor de acceso corresponde a un método con un parámetro de valor único del tipo de evento, un tipo de valor devuelto `void` y los mismos modificadores que el evento que lo contiene.

## <a name="indexers"></a>Indizadores

Un ***indexador*** es un miembro que permite indizar un objeto de la misma manera que una matriz. Los indizadores se declaran mediante *indexer_declaration*s:

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

Un *indexer_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)), el `new` ([modificador nuevo](classes.md#the-new-modifier)), `virtual` ([métodos virtuales](classes.md#virtual-methods)), `override` ([métodos de invalidación](classes.md#override-methods)), `sealed` ([métodos sellados](classes.md#sealed-methods)), `abstract` (métodos[abstractos](classes.md#abstract-methods)) y modificadores `extern` ([métodos externos](classes.md#external-methods)).

Las declaraciones de indexador están sujetas a las mismas reglas que las declaraciones de método ([métodos](classes.md#methods)) con respecto a las combinaciones válidas de modificadores, con la única excepción de que el modificador static no se permite en una declaración de indexador.

Los modificadores `virtual`, `override`y `abstract` se excluyen mutuamente, excepto en un caso. Los modificadores `abstract` y `override` se pueden usar juntos para que un indizador abstracto pueda reemplazar uno virtual.

El *tipo* de una declaración de indexador especifica el tipo de elemento del indizador introducido por la declaración. A menos que el indizador sea una implementación explícita de un miembro de interfaz, el *tipo* va seguido de la palabra clave `this`. Para una implementación explícita de un miembro de interfaz, el *tipo* va seguido de un *interface_type*, un "`.`" y la palabra clave `this`. A diferencia de otros miembros, los indizadores no tienen nombres definidos por el usuario.

En el *formal_parameter_list* se especifican los parámetros del indexador. La lista de parámetros formales de un indizador corresponde a la de un método ([parámetros de método](classes.md#method-parameters)), salvo que se debe especificar al menos un parámetro y que no se permiten los modificadores de parámetro `ref` y `out`.

El *tipo* de un indexador y cada uno de los tipos a los que se hace referencia en el *formal_parameter_list* deben ser al menos tan accesibles como el propio indizador ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

Un *indexer_body* puede consistir en un ***cuerpo de descriptor de acceso*** o en un ***cuerpo de expresión***. En el cuerpo de un descriptor de acceso, *accessor_declarations*, que se deben incluir en los tokens "`{`" y "`}`", declare los descriptores de acceso ([descriptores de acceso](classes.md#accessors)) de la propiedad. Los descriptores de acceso especifican las instrucciones ejecutables asociadas a la lectura y la escritura de la propiedad.

Un cuerpo de expresión que consiste en "`=>`" seguido de una expresión `E` y un punto y coma es exactamente equivalente al cuerpo de la instrucción `{ get { return E; } }`y, por tanto, solo se puede usar para especificar indizadores de solo captador en los que una sola expresión proporciona el resultado del captador.

Aunque la sintaxis para tener acceso a un elemento de indexador es la misma que para un elemento de matriz, un elemento de indexador no se clasifica como una variable. Por lo tanto, no es posible pasar un elemento indexador como `ref` o `out` argumento.

La lista de parámetros formales de un indexador define la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) del indexador. En concreto, la firma de un indexador consta del número y los tipos de sus parámetros formales. El tipo de elemento y los nombres de los parámetros formales no forman parte de la firma de un indexador.

La firma de un indizador debe ser diferente de las firmas de todos los demás indexadores declarados en la misma clase.

Los indexadores y las propiedades son muy similares en concepto, pero difieren de las siguientes maneras:

*  Una propiedad se identifica por su nombre, mientras que un indexador se identifica mediante su firma.
*  Se tiene acceso a una propiedad a través de un *simple_name* ([nombres simples](expressions.md#simple-names)) o un *member_access* ([acceso a miembros](expressions.md#member-access)), mientras que se tiene acceso a un elemento de indexador a través de un *element_access* ([acceso a indexador](expressions.md#indexer-access)).
*  Una propiedad puede ser un miembro de `static`, mientras que un indexador siempre es un miembro de instancia.
*  Un descriptor de acceso de `get` de una propiedad corresponde a un método sin parámetros, mientras que un descriptor de acceso de `get` de un indizador corresponde a un método con la misma lista de parámetros formales que el indexador.
*  Un descriptor de acceso de `set` de una propiedad corresponde a un método con un parámetro único denominado `value`, mientras que un descriptor de acceso de `set` de un indizador corresponde a un método con la misma lista de parámetros formales que el indexador, más un parámetro adicional denominado `value`.
*  Es un error en tiempo de compilación que un descriptor de acceso de indexador declare una variable local con el mismo nombre que un parámetro de indizador.
*  En una declaración de propiedad de reemplazo, se obtiene acceso a la propiedad heredada mediante la sintaxis `base.P`, donde `P` es el nombre de la propiedad. En una declaración de indizador de reemplazo, se tiene acceso al indizador heredado mediante la sintaxis `base[E]`, donde `E` es una lista de expresiones separadas por comas.
*  No hay ningún concepto de "indexador implementado automáticamente". Es un error tener un indexador no externo no abstracto con descriptores de acceso de punto y coma.

Aparte de estas diferencias, todas las reglas definidas en los [descriptores de acceso](classes.md#accessors) y [las propiedades implementadas automáticamente](classes.md#automatically-implemented-properties) se aplican a los descriptores de acceso del indizador y a los descriptores de acceso de propiedades.

Cuando una declaración de indexador incluye un modificador `extern`, se dice que se trata de un ***indizador externo***. Dado que una declaración de indexador externa no proporciona ninguna implementación real, cada una de sus *accessor_declarations* consta de un punto y coma.

En el ejemplo siguiente se declara una clase `BitArray` que implementa un indizador para tener acceso a los bits individuales de la matriz de bits.
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

Una instancia de la clase `BitArray` consume bastante menos memoria que una `bool[]` correspondiente (puesto que cada valor de la antigua solo ocupa un bit en lugar de un byte), pero permite las mismas operaciones que una `bool[]`.

En la siguiente clase de `CountPrimes` se usa un `BitArray` y el algoritmo clásico "tamiz" para calcular el número de primas entre 1 y un máximo determinado:
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

En el ejemplo siguiente se muestra una clase de cuadrícula de 26 * 10 que tiene un indizador con dos parámetros. El primer parámetro debe ser una letra mayúscula o minúscula en el intervalo A-Z, y el segundo debe ser un entero en el intervalo 0-9.

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

### <a name="indexer-overloading"></a>Sobrecarga del indexador

Las reglas de resolución de sobrecarga de indexador se describen en [inferencia de tipos](expressions.md#type-inference).

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

Hay tres categorías de Operadores sobrecargables: operadores unarios ([operadores unarios](classes.md#unary-operators)), operadores binarios ([operadores binarios](classes.md#binary-operators)) y operadores de conversión ([operadores de conversión](classes.md#conversion-operators)).

El *operator_body* es un punto y coma, un ***cuerpo de instrucción*** o un ***cuerpo de expresión***. Un cuerpo de instrucción consta de un *bloque*, que especifica las instrucciones que se ejecutarán cuando se invoque el operador. El *bloque* debe cumplir las reglas de los métodos que devuelven valores descritos en el [cuerpo del método](classes.md#method-body). Un cuerpo de expresión consta de `=>` seguido de una expresión y un punto y coma, y denota una expresión única que se debe realizar cuando se invoca el operador.

Para los operadores de `extern`, el *operator_body* consta simplemente de un punto y coma. En el caso de todos los demás operadores, el *operator_body* es un cuerpo de bloque o un cuerpo de expresión.

Las siguientes reglas se aplican a todas las declaraciones de operador:

*  Una declaración de operador debe incluir un `public` y un modificador `static`.
*  Los parámetros de un operador deben ser parámetros de valor (parámetros de[valor](variables.md#value-parameters)). Es un error en tiempo de compilación que una declaración de operador especifique `ref` o `out` parámetros.
*  La firma de un operador ([operadores unarios](classes.md#unary-operators), [operadores binarios](classes.md#binary-operators)y [operadores de conversión](classes.md#conversion-operators)) debe ser diferente de las firmas de todos los demás operadores declarados en la misma clase.
*  Todos los tipos a los que se hace referencia en una declaración de operador deben ser al menos tan accesibles como el propio operador ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).
*  Es un error que el mismo modificador aparezca varias veces en una declaración de operador.

Cada categoría de operador impone restricciones adicionales, tal y como se describe en las secciones siguientes.

Al igual que otros miembros, las clases derivadas heredan los operadores declarados en una clase base. Dado que las declaraciones de operador siempre requieren la clase o estructura en la que se declara que el operador participa en la firma del operador, no es posible que un operador declarado en una clase derivada oculte un operador declarado en una clase base. Por lo tanto, el modificador `new` nunca es necesario y, por lo tanto, nunca se permite en una declaración de operador.

Puede encontrar información adicional sobre operadores unarios y binarios en [operadores](expressions.md#operators).

Puede encontrar información adicional sobre los operadores de conversión en [conversiones definidas por el usuario](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Operadores unarios

Las siguientes reglas se aplican a las declaraciones de operadores unarios, donde `T` denota el tipo de instancia de la clase o el struct que contiene la declaración del operador:

*  Un operador unario `+`, `-`, `!`o `~` debe tomar un parámetro único de tipo `T` o `T?` y puede devolver cualquier tipo.
*  Un operador unario `++` o `--` debe tomar un parámetro único de tipo `T` o `T?` y debe devolver el mismo tipo o un tipo derivado de él.
*  Un operador unario `true` o `false` debe tomar un parámetro único de tipo `T` o `T?` y debe devolver el tipo `bool`.

La firma de un operador unario consta del símbolo (token) del operador (`+`, `-`, `!`, `~`, `++`, `--`, `true`o `false`) y el tipo del único parámetro formal. El tipo de valor devuelto no forma parte de la firma de un operador unario, ni tampoco el nombre del parámetro formal.

Los operadores unarios `true` y `false` requieren una declaración de pares. Se produce un error en tiempo de compilación si una clase declara uno de estos operadores sin declarar también el otro. Los operadores `true` y `false` se describen con más detalle en [operadores lógicos condicionales definidos por el usuario](expressions.md#user-defined-conditional-logical-operators) y en [Expresiones booleanas](expressions.md#boolean-expressions).

En el ejemplo siguiente se muestra una implementación y el uso posterior de `operator ++` para una clase de vector entero:
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

Observe cómo el método de operador devuelve el valor generado agregando 1 al operando, al igual que los operadores de incremento y decremento de postfijo[(operadores de incremento y](expressions.md#postfix-increment-and-decrement-operators)decremento postfijo) y los operadores de incremento y decremento de prefijo (operadores de incremento y decremento de[prefijo](expressions.md#prefix-increment-and-decrement-operators)). A diferencia de C++en, este método no necesita modificar directamente el valor de su operando. De hecho, la modificación del valor de operando infringiría la semántica estándar del operador de incremento de postfijo.

### <a name="binary-operators"></a>Operadores binarios

Las siguientes reglas se aplican a las declaraciones de operadores binarios, donde `T` denota el tipo de instancia de la clase o el struct que contiene la declaración de operador:

*  Un operador binario sin desplazamiento debe tomar dos parámetros, al menos uno de los cuales debe tener el tipo `T` o `T?`y puede devolver cualquier tipo.
*  Un operador binario `<<` o `>>` debe tomar dos parámetros, el primero debe tener el tipo `T` o `T?` y el segundo debe tener el tipo `int` o `int?`y puede devolver cualquier tipo.

La firma de un operador binario consta del símbolo (token) del operador (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`o `<=`) y los tipos de los dos parámetros formales. El tipo de valor devuelto y los nombres de los parámetros formales no forman parte de la firma de un operador binario.

Ciertos operadores binarios requieren la declaración de pares. Para cada declaración de cualquiera de los operadores de un par, debe haber una declaración coincidente del otro operador del par. Dos declaraciones de operador coinciden cuando tienen el mismo tipo de valor devuelto y el mismo tipo para cada parámetro. Los operadores siguientes requieren una declaración de pares:

*  `operator ==` y `operator !=`
*  `operator >` y `operator <`
*  `operator >=` y `operator <=`

### <a name="conversion-operators"></a>Operadores de conversión

Una declaración de operador de conversión introduce una ***conversión definida por el usuario*** ([conversiones definidas por el usuario](conversions.md#user-defined-conversions)) que aumenta las conversiones implícitas y explícitas predefinidas.

Una declaración de operador de conversión que incluye la palabra clave `implicit` presenta una conversión implícita definida por el usuario. Las conversiones implícitas pueden producirse en diversas situaciones, incluidas las invocaciones de miembros de función, las expresiones de conversión y las asignaciones. Esto se describe con más detalle en [conversiones implícitas](conversions.md#implicit-conversions).

Una declaración de operador de conversión que incluye la palabra clave `explicit` presenta una conversión explícita definida por el usuario. Las conversiones explícitas pueden producirse en expresiones de conversión y se describen con más detalle en [conversiones explícitas](conversions.md#explicit-conversions).

Un operador de conversión convierte de un tipo de origen, indicado por el tipo de parámetro del operador de conversión, en un tipo de destino, indicado por el tipo de valor devuelto del operador de conversión.

Para un tipo de origen determinado `S` y el tipo de destino `T`, si `S` o `T` son tipos que aceptan valores NULL, permita que `S0` y `T0` hagan referencia a sus tipos subyacentes; de lo contrario, `S0` y `T0` son iguales a `S` y `T` respectivamente. Una clase o struct puede declarar una conversión de un tipo de origen `S` a un tipo de destino `T` solo si se cumplen todas las condiciones siguientes:

*  `S0` y `T0` son tipos diferentes.
*  `S0` o `T0` es el tipo de clase o estructura en el que tiene lugar la declaración del operador.
*  Ni `S0` ni `T0` es una *interface_type*.
*  Sin incluir las conversiones definidas por el usuario, no existe una conversión de `S` a `T` o de `T` a `S`.

En el caso de estas reglas, cualquier parámetro de tipo asociado a `S` o `T` se considera que son tipos únicos que no tienen ninguna relación de herencia con otros tipos y se omiten las restricciones en esos parámetros de tipo.

en el ejemplo
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
las dos primeras declaraciones de operador están permitidas porque, con fines de [indexadores](classes.md#indexers). 3, `T` y `int` y `string` respectivamente se consideran tipos únicos sin relación. Sin embargo, el tercer operador es un error porque `C<T>` es la clase base de `D<T>`.

En la segunda regla, se sigue que un operador de conversión debe convertir a o desde el tipo de clase o estructura en el que se declara el operador. Por ejemplo, es posible que un tipo de clase o struct `C` defina una conversión de `C` a `int` y de `int` a `C`, pero no de `int` a `bool`.

No es posible volver a definir directamente una conversión predefinida. Por lo tanto, no se permite a los operadores de conversión convertir de o a `object` porque ya existen conversiones implícitas y explícitas entre `object` y todos los demás tipos. Del mismo modo, ni los tipos de origen ni de destino de una conversión pueden ser un tipo base del otro, ya que una conversión ya existirá.

Sin embargo, es posible declarar operadores en tipos genéricos que, para argumentos de tipo concretos, especifiquen conversiones que ya existan como conversiones predefinidas. en el ejemplo
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
Cuando el tipo `object` se especifica como un argumento de tipo para `T`, el segundo operador declara una conversión que ya existe (una conversión implícita y, por tanto, también existe una conversión explícita de cualquier tipo al tipo `object`).

En los casos en los que existe una conversión predefinida entre dos tipos, se omiten las conversiones definidas por el usuario entre esos tipos. De manera específica:

*  Si existe una conversión implícita predefinida ([conversiones implícitas](conversions.md#implicit-conversions)) del tipo `S` al tipo `T`, se omiten todas las conversiones definidas por el usuario (implícitas o explícitas) de `S` a `T`.
*  Si existe una conversión explícita predefinida ([conversiones explícitas](conversions.md#explicit-conversions)) del tipo `S` al tipo `T`, se omiten las conversiones explícitas definidas por el usuario de `S` a `T`. Asimismo,

Si `T` es un tipo de interfaz, se omiten las conversiones implícitas definidas por el usuario de `S` a `T`.

De lo contrario, las conversiones implícitas definidas por el usuario de `S` a `T` se siguen considerando.

En todos los tipos pero `object`, los operadores declarados por el tipo de `Convertible<T>` anterior no entran en conflicto con las conversiones predefinidas. Por ejemplo:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Sin embargo, para el tipo `object`, las conversiones predefinidas ocultan las conversiones definidas por el usuario en todos los casos, pero una:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

No se permiten conversiones definidas por el usuario para convertir de o a *interface_type*s. En concreto, esta restricción garantiza que no se produzcan transformaciones definidas por el usuario al realizar la conversión a un *interface_type*y que una conversión a una *interface_type* se realice correctamente solo si el objeto que se está convirtiendo implementa realmente el *interface_type*especificado.

La firma de un operador de conversión está formada por el tipo de origen y el tipo de destino. (Tenga en cuenta que esta es la única forma de miembro para la que participa el tipo de valor devuelto en la firma.) La clasificación `implicit` o `explicit` de un operador de conversión no forma parte de la firma del operador. Por lo tanto, una clase o struct no puede declarar un `implicit` y un operador de conversión `explicit` con los mismos tipos de origen y de destino.

En general, las conversiones implícitas definidas por el usuario deben diseñarse para que nunca se produzcan excepciones y nunca se pierda información. Si una conversión definida por el usuario puede dar lugar a excepciones (por ejemplo, porque el argumento de origen está fuera del intervalo) o la pérdida de información (como descartar los bits de orden superior), esa conversión debe definirse como una conversión explícita.

en el ejemplo
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
la conversión de `Digit` a `byte` es implícita porque nunca produce excepciones o pierde información, pero la conversión de `byte` a `Digit` es explícita, ya que `Digit` solo puede representar un subconjunto de los valores posibles de una `byte`.

## <a name="instance-constructors"></a>Constructores de instancias

Un ***constructor de instancia*** es un miembro que implementa las acciones necesarias para inicializar una instancia de una clase. Los constructores de instancias se declaran mediante *constructor_declaration*s:

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

Un *constructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)), una combinación válida de los cuatro modificadores de acceso ([modificadores de acceso](classes.md#access-modifiers)) y un modificador `extern` ([métodos externos](classes.md#external-methods)). No se permite que una declaración de constructor incluya el mismo modificador varias veces.

El *identificador* de un *constructor_declarator* debe nombrar la clase en la que se declara el constructor de instancia. Si se especifica cualquier otro nombre, se produce un error en tiempo de compilación.

El *formal_parameter_list* opcional de un constructor de instancia está sujeto a las mismas reglas que la *formal_parameter_list* de un método ([métodos](classes.md#methods)). La lista de parámetros formales define la firma ([firmas y sobrecarga](basic-concepts.md#signatures-and-overloading)) de un constructor de instancia y rige el proceso por el cual la resolución de sobrecarga ([inferencia de tipos](expressions.md#type-inference)) selecciona un constructor de instancia determinado en una invocación.

Cada uno de los tipos a los que se hace referencia en el *formal_parameter_list* de un constructor de instancia debe ser al menos igual de accesible que el propio constructor ([restricciones de accesibilidad](basic-concepts.md#accessibility-constraints)).

El *constructor_initializer* opcional especifica otro constructor de instancia que se va a invocar antes de ejecutar las instrucciones proporcionadas en el *constructor_body* de este constructor de instancia. Esto se describe con más detalle en [inicializadores de constructor](classes.md#constructor-initializers).

Cuando una declaración de constructor incluye un modificador `extern`, se dice que el constructor es un ***constructor externo***. Dado que una declaración de constructor externo no proporciona ninguna implementación real, su *constructor_body* consta de un punto y coma. En el caso de todos los demás constructores, el *constructor_body* se compone de un *bloque* que especifica las instrucciones para inicializar una nueva instancia de la clase. Esto corresponde exactamente al *bloque* de un método de instancia con un tipo de valor devuelto `void` ([cuerpo del método](classes.md#method-body)).

No se heredan los constructores de instancia. Por lo tanto, una clase no tiene ningún constructor de instancia que no sea el declarado realmente en la clase. Si una clase no contiene ninguna declaración de constructor de instancia, se proporciona automáticamente un constructor de instancia predeterminado ([constructores predeterminados](classes.md#default-constructors)).

Los constructores de instancias se invocan mediante *object_creation_expression*s ([expresiones de creación de objetos](expressions.md#object-creation-expressions)) y a través de *constructor_initializer*s.

### <a name="constructor-initializers"></a>Inicializadores del constructor

Todos los constructores de instancia (excepto los de la clase `object`) incluyen implícitamente una invocación de otro constructor de instancia inmediatamente antes de la *constructor_body*. El constructor para invocar implícitamente está determinado por el *constructor_initializer*:

*  Un inicializador de constructor de instancia con el formato `base(argument_list)` o `base()` hace que se invoque un constructor de instancia de la clase base directa. Ese constructor se selecciona utilizando *argument_list* , si está presente y las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution). El conjunto de constructores de instancias candidatas consta de todos los constructores de instancia accesibles contenidos en la clase base directa, o el constructor predeterminado ([constructores predeterminados](classes.md#default-constructors)), si no se declara ningún constructor de instancia en la clase base directa. Si este conjunto está vacío, o si no se puede identificar un único constructor de instancia mejor, se produce un error en tiempo de compilación.
*  Un inicializador de constructor de instancia con el formato `this(argument-list)` o `this()` hace que se invoque un constructor de instancia de la propia clase. El constructor se selecciona utilizando *argument_list* , si está presente y las reglas de resolución de sobrecarga de la [resolución de sobrecarga](expressions.md#overload-resolution). El conjunto de constructores de instancias candidatas se compone de todos los constructores de instancia accesibles declarados en la propia clase. Si este conjunto está vacío, o si no se puede identificar un único constructor de instancia mejor, se produce un error en tiempo de compilación. Si una declaración de constructor de instancia incluye un inicializador de constructor que invoca al propio constructor, se produce un error en tiempo de compilación.

Si un constructor de instancia no tiene ningún inicializador de constructor, se proporciona implícitamente un inicializador de constructor con el formato `base()`. Por lo tanto, una declaración de constructor de instancia con el formato
```csharp
C(...) {...}
```
es exactamente equivalente a
```csharp
C(...): base() {...}
```

El ámbito de los parámetros proporcionados por la *formal_parameter_list* de una declaración de constructor de instancia incluye el inicializador de constructor de esa declaración. Por lo tanto, se permite a un inicializador de constructor tener acceso a los parámetros del constructor. Por ejemplo:
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

Un inicializador de constructor de instancia no puede tener acceso a la instancia que se está creando. Por lo tanto, se trata de un error en tiempo de compilación para hacer referencia a `this` en una expresión de argumento del inicializador de constructor, como es un error en tiempo de compilación para que una expresión de argumento haga referencia a un miembro de instancia a través de un *simple_name*.

### <a name="instance-variable-initializers"></a>Inicializadores de variables de instancia

Cuando un constructor de instancia no tiene ningún inicializador de constructor o tiene un inicializador de constructor con el formato `base(...)`, ese constructor realiza implícitamente las inicializaciones especificadas por los *variable_initializer*s de los campos de instancia declarados en su clase. Esto corresponde a una secuencia de asignaciones que se ejecutan inmediatamente después de la entrada al constructor y antes de la invocación implícita del constructor de clase base directo. Los inicializadores de variable se ejecutan en el orden textual en el que aparecen en la declaración de clase.

### <a name="constructor-execution"></a>Ejecución del constructor

Los inicializadores de variables se transforman en instrucciones de asignación, y estas instrucciones de asignación se ejecutan antes de la invocación del constructor de instancia de clase base. Este orden garantiza que todos los campos de instancia se inicializan mediante sus inicializadores de variable antes de que se ejecute cualquier instrucción que tenga acceso a esa instancia.

Dado el ejemplo
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
Cuando se usa `new B()` para crear una instancia de `B`, se genera el siguiente resultado:
```console
x = 1, y = 0
```

El valor de `x` es 1 porque el inicializador de variable se ejecuta antes de que se invoque el constructor de instancia de clase base. Sin embargo, el valor de `y` es 0 (el valor predeterminado de un `int`) porque la asignación a `y` no se ejecuta hasta que el constructor de clase base devuelve.

Resulta útil pensar en los inicializadores de variables de instancia y en los inicializadores de constructor como instrucciones que se insertan automáticamente antes de la *constructor_body*. El ejemplo
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
contiene varios inicializadores variables; también contiene inicializadores de constructor de ambos formularios (`base` y `this`). El ejemplo corresponde al código que se muestra a continuación, donde cada comentario indica una instrucción insertada automáticamente (la sintaxis utilizada para las invocaciones de constructor insertadas automáticamente no es válida, pero simplemente sirve para ilustrar el mecanismo).

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

Si una clase no contiene ninguna declaración de constructor de instancia, se proporciona automáticamente un constructor de instancia predeterminado. Ese constructor predeterminado simplemente invoca el constructor sin parámetros de la clase base directa. Si la clase es abstracta, se protege la accesibilidad declarada para el constructor predeterminado. De lo contrario, la accesibilidad declarada para el constructor predeterminado es Public. Por lo tanto, el constructor predeterminado siempre tiene el formato

```csharp
protected C(): base() {}
```
o
```csharp
public C(): base() {}
```
donde `C` es el nombre de la clase. Si la resolución de sobrecarga no puede determinar un mejor candidato único para el inicializador de constructor de clase base, se produce un error en tiempo de compilación.

en el ejemplo
```csharp
class Message
{
    object sender;
    string text;
}
```
se proporciona un constructor predeterminado porque la clase no contiene ninguna declaración de constructor de instancia. Por lo tanto, el ejemplo es exactamente equivalente a
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Constructores privados

Cuando una clase `T` declara solo constructores de instancia privados, no es posible que las clases fuera del texto del programa de `T` deriven de `T` o para crear directamente instancias de `T`. Por lo tanto, si una clase solo contiene miembros estáticos y no está pensado para que se creen instancias, agregar un constructor de instancia privado vacío impedirá la creación de instancias. Por ejemplo:
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

La clase `Trig` agrupa los métodos relacionados y las constantes, pero no está diseñado para crear instancias. Por lo tanto, declara un único constructor de instancia privado vacío. Se debe declarar al menos un constructor de instancia para suprimir la generación automática de un constructor predeterminado.

### <a name="optional-instance-constructor-parameters"></a>Parámetros de constructor de instancia opcionales

La forma `this(...)` del inicializador de constructor se utiliza normalmente junto con la sobrecarga para implementar parámetros de constructor de instancia opcionales. en el ejemplo
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
los dos primeros constructores de instancia simplemente proporcionan los valores predeterminados para los argumentos que faltan. Ambos utilizan un inicializador de constructor `this(...)` para invocar el tercer constructor de instancia, que realmente realiza el trabajo de inicializar la nueva instancia. El efecto es el de los parámetros de constructor opcionales:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Constructores estáticos

Un ***constructor estático*** es un miembro que implementa las acciones necesarias para inicializar un tipo de clase cerrada. Los constructores estáticos se declaran mediante *static_constructor_declaration*s:

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

Un *static_constructor_declaration* puede incluir un conjunto de *atributos* ([atributos](attributes.md)) y un modificador `extern` ([métodos externos](classes.md#external-methods)).

El *identificador* de un *static_constructor_declaration* debe nombrar la clase en la que se declara el constructor estático. Si se especifica cualquier otro nombre, se produce un error en tiempo de compilación.

Cuando una declaración de constructor estático incluye un modificador `extern`, se dice que el constructor estático es un ***constructor estático externo***. Dado que una declaración de constructor estático externo no proporciona ninguna implementación real, su *static_constructor_body* consta de un punto y coma. Para todas las demás declaraciones de constructores estáticos, la *static_constructor_body* está formada por un *bloque* que especifica las instrucciones que se ejecutan para inicializar la clase. Se corresponde exactamente con la *method_body* de un método estático con un tipo de valor devuelto `void` ([cuerpo del método](classes.md#method-body)).

Los constructores estáticos no se heredan y no se pueden llamar directamente.

El constructor estático de un tipo de clase cerrada se ejecuta como máximo una vez en un dominio de aplicación determinado. La ejecución de un constructor estático lo desencadena el primero de los siguientes eventos para que se produzca dentro de un dominio de aplicación:

*  Se crea una instancia del tipo de clase.
*  Se hace referencia a cualquiera de los miembros estáticos del tipo de clase.

Si una clase contiene el método `Main` ([Inicio](basic-concepts.md#application-startup)de la aplicación) en el que comienza la ejecución, el constructor estático de esa clase se ejecuta antes de que se llame al método `Main`.

Para inicializar un nuevo tipo de clase cerrada, primero se crea un nuevo conjunto de campos estáticos ([campos estáticos y de instancia](classes.md#static-and-instance-fields)) para ese tipo cerrado en particular. Cada uno de los campos estáticos se inicializa en su valor predeterminado ([valores predeterminados](variables.md#default-values)). A continuación, se ejecutan los inicializadores de campo estáticos ([inicialización de campos estáticos](classes.md#static-field-initialization)) para esos campos estáticos. Por último, se ejecuta el constructor estático.

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
debe generar la salida:
```console
Init A
A.F
Init B
B.F
```
Dado que la llamada a `A.F`desencadena la ejecución del constructor estático de `A`, y la llamada a `B.F`desencadena la ejecución del constructor estático de `B`.

Es posible crear dependencias circulares que permitan observar los campos estáticos con inicializadores variables en su estado de valor predeterminado.

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
```console
X = 1, Y = 2
```

Para ejecutar el método `Main`, el sistema primero ejecuta el inicializador para `B.Y`, antes del constructor estático de la clase `B`. el inicializador de `Y`hace que se ejecute el constructor estático de `A`porque se hace referencia al valor de `A.X`. A su vez, el constructor estático de `A` continúa para calcular el valor de `X`y, al hacerlo, recupera el valor predeterminado de `Y`, que es cero. por tanto, `A.X` se inicializa en 1. El proceso de ejecución de los inicializadores de campo estáticos y el constructor estático de `A`se completa y vuelve al cálculo del valor inicial de `Y`, cuyo resultado se convierte en 2.

Dado que el constructor estático se ejecuta exactamente una vez para cada tipo de clase construido cerrado, es un lugar cómodo para aplicar comprobaciones en tiempo de ejecución en el parámetro de tipo que no se puede comprobar en tiempo de compilación a través de restricciones ([restricciones de parámetros de tipo](classes.md#type-parameter-constraints)). Por ejemplo, el tipo siguiente usa un constructor estático para exigir que el argumento de tipo sea una enumeración:
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

Un ***destructor*** es un miembro que implementa las acciones necesarias para destruir una instancia de una clase. Un destructor se declara mediante un *destructor_declaration*:

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

El *identificador* de un *destructor_declaration* debe nombrar la clase en la que se declara el destructor. Si se especifica cualquier otro nombre, se produce un error en tiempo de compilación.

Cuando una declaración de destructor incluye un modificador `extern`, se dice que se trata de un ***destructor externo***. Dado que una declaración de destructor externo no proporciona ninguna implementación real, su *destructor_body* consta de un punto y coma. En el caso de todos los demás destructores, el *destructor_body* consta de un *bloque* que especifica las instrucciones que se van a ejecutar para destruir una instancia de la clase. Un *destructor_body* corresponde exactamente al *method_body* de un método de instancia con un tipo de valor devuelto `void` ([cuerpo del método](classes.md#method-body)).

Los destructores no se heredan. Por lo tanto, una clase no tiene ningún destructor que no sea el que se puede declarar en esa clase.

Dado que un destructor debe tener ningún parámetro, no se puede sobrecargar, por lo que una clase puede tener, como máximo, un destructor.

Los destructores se invocan automáticamente y no se pueden invocar explícitamente. Una instancia pasa a ser válida para su destrucción cuando ya no es posible para cualquier código utilizar esa instancia. La ejecución del destructor para la instancia puede producirse en cualquier momento después de que la instancia sea válida para la destrucción. Cuando se destruye una instancia, se llama a los destructores de la cadena de herencia de la instancia, en orden, de la más derivada a la menos derivada. Se puede ejecutar un destructor en cualquier subproceso. Para obtener más información sobre las reglas que rigen cuándo y cómo se ejecuta un destructor, consulte [Administración automática](basic-concepts.md#automatic-memory-management)de la memoria.

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
estará
```
B's destructor
A's destructor
```
Dado que se llama a los destructores en una cadena de herencia en orden, desde la más derivada hasta la menos derivada.

Los destructores se implementan invalidando el método virtual `Finalize` en `System.Object`. C#no se permite a los programas invalidar este método ni llamarlo (o invalidarlo) directamente. Por ejemplo, el programa
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

El compilador se comporta como si este método, y los reemplaza, no existen en absoluto. Por lo tanto, este programa:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
es válido y el método mostrado oculta el método de `Finalize` de `System.Object`.

Para obtener una explicación del comportamiento cuando se produce una excepción desde un destructor, vea [cómo se controlan las excepciones](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Iteradores

Un miembro de función ([miembros de función](expressions.md#function-members)) implementado mediante un bloque de iteradores ([bloques](statements.md#blocks)) se denomina ***iterador***.

Un bloque de iterador se puede usar como cuerpo de un miembro de función siempre que el tipo de valor devuelto del miembro de función correspondiente sea una de las interfaces de enumerador ([interfaces de enumerador](classes.md#enumerator-interfaces)) o una de las interfaces enumerables ([interfaces enumerables](classes.md#enumerable-interfaces)). Puede producirse como *method_body*, *operator_body* o *accessor_body*, mientras que los eventos, constructores de instancias, constructores estáticos y destructores no se pueden implementar como iteradores.

Cuando un miembro de función se implementa mediante un bloque de iteradores, se trata de un error en tiempo de compilación para la lista de parámetros formales del miembro de función a fin de especificar los parámetros `ref` o `out`.

### <a name="enumerator-interfaces"></a>Interfaces de enumerador

Las ***interfaces de enumerador*** son la interfaz no genérica `System.Collections.IEnumerator` y todas las creaciones de instancias de la interfaz genérica `System.Collections.Generic.IEnumerator<T>`. Por motivos de brevedad, en este capítulo se hace referencia a estas interfaces como `IEnumerator` y `IEnumerator<T>`, respectivamente.

### <a name="enumerable-interfaces"></a>Interfaces enumerables

Las ***interfaces enumerables*** son la interfaz no genérica `System.Collections.IEnumerable` y todas las creaciones de instancias de la interfaz genérica `System.Collections.Generic.IEnumerable<T>`. Por motivos de brevedad, en este capítulo se hace referencia a estas interfaces como `IEnumerable` y `IEnumerable<T>`, respectivamente.

### <a name="yield-type"></a>Tipo yield

Un iterador genera una secuencia de valores, todo el mismo tipo. Este tipo se denomina ***tipo yield*** del iterador.

*  El tipo yield de un iterador que devuelve `IEnumerator` o `IEnumerable` es `object`.
*  El tipo yield de un iterador que devuelve `IEnumerator<T>` o `IEnumerable<T>` es `T`.

### <a name="enumerator-objects"></a>Objetos Enumerator

Cuando un miembro de función que devuelve un tipo de interfaz de enumerador se implementa mediante un bloque de iteradores, al invocar al miembro de función no se ejecuta inmediatamente el código en el bloque de iteradores. En su lugar, se crea y se devuelve un ***objeto de enumerador*** . Este objeto encapsula el código especificado en el bloque de iteradores y la ejecución del código en el bloque de iterador se produce cuando se invoca el método de `MoveNext` del objeto de enumerador. Un objeto de enumerador tiene las siguientes características:

*  Implementa `IEnumerator` y `IEnumerator<T>`, donde `T` es el tipo yield del iterador.
*  Implementa `System.IDisposable`.
*  Se inicializa con una copia de los valores de argumento (si existen) y el valor de instancia pasado al miembro de función.
*  Tiene cuatro Estados posibles, ***antes***, en ***ejecución***, ***suspendido***y ***después***, y está inicialmente en el estado ***antes*** .

Un objeto de enumerador suele ser una instancia de una clase de enumerador generada por el compilador que encapsula el código en el bloque de iteradores e implementa las interfaces del enumerador, pero otros métodos de implementación son posibles. Si el compilador genera una clase de enumerador, esa clase se anidará, directa o indirectamente, en la clase que contiene el miembro de función, tendrá accesibilidad privada y tendrá un nombre reservado para uso del compilador ([identificadores](lexical-structure.md#identifiers)).

Un objeto de enumerador puede implementar más interfaces de las especificadas anteriormente.

En las secciones siguientes se describe el comportamiento exacto de los miembros `MoveNext`, `Current`y `Dispose` de las implementaciones de interfaz `IEnumerable` y `IEnumerable<T>` proporcionadas por un objeto de enumerador.

Tenga en cuenta que los objetos de enumerador no admiten el método `IEnumerator.Reset`. Al invocar este método, se produce una `System.NotSupportedException`.

#### <a name="the-movenext-method"></a>El método MoveNext

El método `MoveNext` de un objeto enumerador encapsula el código de un bloque de iteradores. Al invocar el método `MoveNext` se ejecuta código en el bloque de iterador y se establece la propiedad `Current` del objeto de enumerador según corresponda. La acción precisa realizada por `MoveNext` depende del estado del objeto de enumerador cuando se invoca `MoveNext`:

*  Si el estado del objeto de enumerador es ***anterior***, al invocar `MoveNext`:
   * Cambia el estado a en ***ejecución***.
   * Inicializa los parámetros (incluido `this`) del bloque de iteradores en los valores de argumento y el valor de instancia guardados cuando se inicializó el objeto de enumerador.
   * Ejecuta el bloque de iterador desde el principio hasta que se interrumpe la ejecución (como se describe a continuación).
*  Si el estado del objeto de enumerador se está ***ejecutando***, no se especifica el resultado de invocar `MoveNext`.
*  Si el estado del objeto de enumerador es ***suspendido***, al invocar `MoveNext`:
   * Cambia el estado a en ***ejecución***.
   * Restaura los valores de todas las variables locales y los parámetros (incluido este) en los valores guardados cuando la ejecución del bloque de iterador se suspendió por última vez. Tenga en cuenta que el contenido de los objetos a los que hacen referencia estas variables puede haber cambiado desde la llamada anterior a MoveNext.
   * Reanuda la ejecución del bloque de iterador inmediatamente después de la instrucción `yield return` que causó la suspensión de la ejecución y continúa hasta que se interrumpe la ejecución (como se describe a continuación).
*  Si el estado del objeto de enumerador es ***posterior***, al invocar `MoveNext` se devuelve `false`.


Cuando `MoveNext` ejecuta el bloque de iterador, la ejecución se puede interrumpir de cuatro maneras: mediante una instrucción de `yield return`, mediante una instrucción `yield break`, al encontrar el final del bloque de iterador y una excepción que se produce y se propaga fuera del bloque de iterador.

*  Cuando se encuentra una instrucción `yield return` ([la instrucción yield](statements.md#the-yield-statement)):
   * La expresión dada en la instrucción se evalúa, se convierte implícitamente al tipo Yield y se asigna a la propiedad `Current` del objeto de enumerador.
   * Se suspende la ejecución del cuerpo del iterador. Se guardan los valores de todas las variables locales y los parámetros (incluido `this`), al igual que la ubicación de esta instrucción `yield return`. Si la instrucción `yield return` está en uno o varios bloques de `try`, los bloques de `finally` asociados no se ejecutan en este momento.
   * El estado del objeto de enumerador se cambia a ***Suspended***.
   * El método `MoveNext` devuelve `true` a su llamador, lo que indica que la iteración se ha avanzado correctamente al valor siguiente.
*  Cuando se encuentra una instrucción `yield break` ([la instrucción yield](statements.md#the-yield-statement)):
   * Si la instrucción `yield break` está dentro de uno o más bloques `try`, se ejecutan los bloques de `finally` asociados.
   * El estado del objeto de enumerador se cambia a ***después***de.
   * El método `MoveNext` devuelve `false` a su llamador, lo que indica que la iteración ha finalizado.
*  Cuando se encuentra el final del cuerpo del iterador:
   * El estado del objeto de enumerador se cambia a ***después***de.
   * El método `MoveNext` devuelve `false` a su llamador, lo que indica que la iteración ha finalizado.
*  Cuando se produce una excepción y se propaga fuera del bloque de iteradores:
   * La propagación de excepciones habrá ejecutado los bloques de `finally` adecuados en el cuerpo del iterador.
   * El estado del objeto de enumerador se cambia a ***después***de.
   * La propagación de excepciones continúa hasta el autor de la llamada del método `MoveNext`.

#### <a name="the-current-property"></a>Propiedad actual

La propiedad `Current` de un objeto de enumerador se ve afectada por `yield return` instrucciones del bloque de iteradores.

Cuando un objeto de enumerador se encuentra en estado ***suspendido*** , el valor de `Current` es el valor establecido por la llamada anterior a `MoveNext`. Cuando un objeto de enumerador está en los Estados ***Before***, ***Running***o ***After*** , no se especifica el resultado de obtener acceso a `Current`.

En el caso de un iterador con un tipo de Yield distinto de `object`, el resultado de tener acceso a `Current` a través de la implementación de `IEnumerable` del objeto de enumerador corresponde al acceso a `Current` a través de la `IEnumerator<T>` de la implementación del objeto de enumerador y convertir el resultado a `object`.

#### <a name="the-dispose-method"></a>El método Dispose

El método `Dispose` se utiliza para limpiar la iteración, para lo que coloca el objeto enumerador en el estado ***después*** .

*  Si el estado del objeto de enumerador es ***anterior***, al invocar `Dispose` cambia el estado a ***después***de.
*  Si el estado del objeto de enumerador se está ***ejecutando***, no se especifica el resultado de invocar `Dispose`.
*  Si el estado del objeto de enumerador es ***suspendido***, al invocar `Dispose`:
   * Cambia el estado a en ***ejecución***.
   * Ejecuta cualquier bloque Finally como si la última instrucción `yield return` ejecuta fuera una instrucción `yield break`. Si esto hace que se produzca una excepción y se propague fuera del cuerpo del iterador, el estado del objeto de enumerador se establece en ***después*** de y la excepción se propaga al llamador del método `Dispose`.
   * Cambia el estado a ***después***de.
*  Si el estado del objeto de enumerador es ***posterior***, la invocación de `Dispose` no tiene ningún efecto.

### <a name="enumerable-objects"></a>Objetos enumerables

Cuando un miembro de función que devuelve un tipo de interfaz enumerable se implementa mediante un bloque de iteradores, al invocar al miembro de función no se ejecuta inmediatamente el código en el bloque de iteradores. En su lugar, se crea y se devuelve un ***objeto enumerable*** . El método `GetEnumerator` del objeto enumerable devuelve un objeto de enumerador que encapsula el código especificado en el bloque de iterador y la ejecución del código en el bloque de iterador se produce cuando se invoca el método de `MoveNext` del objeto de enumerador. Un objeto enumerable tiene las siguientes características:

*  Implementa `IEnumerable` y `IEnumerable<T>`, donde `T` es el tipo yield del iterador.
*  Se inicializa con una copia de los valores de argumento (si existen) y el valor de instancia pasado al miembro de función.

Un objeto enumerable es normalmente una instancia de una clase Enumerable generada por el compilador que encapsula el código en el bloque de iteradores e implementa las interfaces enumerables, pero otros métodos de implementación son posibles. Si el compilador genera una clase Enumerable, esa clase se anidará, directa o indirectamente, en la clase que contiene el miembro de función, tendrá accesibilidad privada y tendrá un nombre reservado para uso del compilador ([identificadores](lexical-structure.md#identifiers)).

Un objeto enumerable puede implementar más interfaces de las especificadas anteriormente. En concreto, un objeto enumerable también puede implementar `IEnumerator` y `IEnumerator<T>`, lo que permite que sirva como un enumerador y un enumerador. En ese tipo de implementación, la primera vez que se invoca un método `GetEnumerator` del objeto enumerable, se devuelve el propio objeto Enumerable. Las siguientes invocaciones del `GetEnumerator`del objeto enumerable, si las hay, devuelven una copia del objeto Enumerable. Por lo tanto, cada enumerador devuelto tiene su propio estado y los cambios en un enumerador no afectarán a otro.

#### <a name="the-getenumerator-method"></a>El método GetEnumerator

Un objeto enumerable proporciona una implementación de los métodos `GetEnumerator` de las interfaces `IEnumerable` y `IEnumerable<T>`. Los dos métodos `GetEnumerator` comparten una implementación común que adquiere y devuelve un objeto de enumerador disponible. El objeto de enumerador se inicializa con los valores de argumento y el valor de instancia guardados cuando se inicializó el objeto enumerable, pero de lo contrario el objeto de enumerador funciona como se describe en [objetos de enumerador](classes.md#enumerator-objects).

### <a name="implementation-example"></a>Ejemplo de implementación

En esta sección se describe una posible implementación de iteradores en términos C# de construcciones estándar. La implementación que se describe aquí se basa en los mismos principios utilizados por C# el compilador de Microsoft, pero no es una implementación asignada o la única que sea posible.

La siguiente clase de `Stack<T>` implementa su método de `GetEnumerator` mediante un iterador. El iterador enumera los elementos de la pila en orden descendente.

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

El método `GetEnumerator` se puede convertir en una instancia de una clase de enumerador generada por el compilador que encapsula el código en el bloque de iteradores, como se muestra a continuación.

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

En la traducción anterior, el código del bloque de iterador se convierte en una máquina de Estados y se coloca en el método `MoveNext` de la clase de enumerador. Además, la variable local `i` se convierte en un campo en el objeto de enumerador para que pueda seguir existiendo en las invocaciones de `MoveNext`.

En el ejemplo siguiente se imprime una tabla de multiplicación simple de los enteros comprendidos entre 1 y 10. El método `FromTo` del ejemplo devuelve un objeto enumerable y se implementa mediante un iterador.

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

El método `FromTo` se puede convertir en una instancia de una clase Enumerable generada por el compilador que encapsula el código en el bloque de iteradores, como se muestra a continuación.

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

La clase Enumerable implementa las interfaces enumerables y las interfaces de enumerador, lo que permite que sirva como un enumerador y un enumerador. La primera vez que se invoca el método de `GetEnumerator`, se devuelve el propio objeto Enumerable. Las siguientes invocaciones del `GetEnumerator`del objeto enumerable, si las hay, devuelven una copia del objeto Enumerable. Por lo tanto, cada enumerador devuelto tiene su propio estado y los cambios en un enumerador no afectarán a otro. El método `Interlocked.CompareExchange` se usa para garantizar la operación segura para subprocesos.

Los parámetros `from` y `to` se convierten en campos de la clase Enumerable. Dado que `from` se modifica en el bloque de iterador, se introduce un campo de `__from` adicional para contener el valor inicial dado a `from` en cada enumerador.

El método `MoveNext` produce una `InvalidOperationException` si se llama cuando se `0``__state`. Esto protege contra el uso del objeto enumerable como un objeto de enumerador sin llamar primero a `GetEnumerator`.

En el ejemplo siguiente se muestra una clase de árbol simple. La clase `Tree<T>` implementa su método de `GetEnumerator` mediante un iterador. El iterador enumera los elementos del árbol en orden infijo.

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

El método `GetEnumerator` se puede convertir en una instancia de una clase de enumerador generada por el compilador que encapsula el código en el bloque de iteradores, como se muestra a continuación.

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

El compilador generó objetos temporales utilizado en las instrucciones `foreach` se elevan a los campos `__left` y `__right` del objeto enumerador. El campo `__state` del objeto de enumerador se actualiza cuidadosamente para que el método de `Dispose()` correcto se llame correctamente si se produce una excepción. Tenga en cuenta que no es posible escribir el código traducido con instrucciones de `foreach` simples.

## <a name="async-functions"></a>Funciones asincrónicas

Un método ([métodos](classes.md#methods)) o una función anónima ([expresiones de función anónima](expressions.md#anonymous-function-expressions)) con el modificador `async` se denomina ***función asincrónica***. En general, el término ***Async*** se usa para describir cualquier tipo de función que tenga el modificador `async`.

Se trata de un error en tiempo de compilación para la lista de parámetros formales de una función asincrónica con el fin de especificar los parámetros `ref` o `out`.

El *return_type* de un método asincrónico debe ser `void` o un ***tipo de tarea***. Los tipos de tarea son `System.Threading.Tasks.Task` y los tipos construidos a partir de `System.Threading.Tasks.Task<T>`. Por motivos de brevedad, en este capítulo se hace referencia a estos tipos como `Task` y `Task<T>`, respectivamente. Se dice que un método asincrónico que devuelve un tipo de tarea es el que devuelve la tarea.

La definición exacta de los tipos de tarea es la implementación definida, pero desde el punto de vista del idioma, un tipo de tarea se encuentra en uno de los Estados incompleto, correcto o erróneo. Una tarea con errores registra una excepción pertinente. Un `Task<T>` correctamente registra un resultado de tipo `T`. Los tipos de tarea son de espera y, por tanto, pueden ser los operandos de las expresiones Await ([expresiones Await](expressions.md#await-expressions)).

Una invocación de función asincrónica tiene la capacidad de suspender la evaluación por medio de expresiones Await ([expresiones Await](expressions.md#await-expressions)) en su cuerpo. La evaluación se puede reanudar posteriormente en el punto de la expresión Await de suspensión por medio de un ***delegado de reanudación***. El delegado de reanudación es de tipo `System.Action`y, cuando se invoca, la evaluación de la invocación de la función asincrónica se reanudará desde la expresión Await en la que se quedó. El ***llamador actual*** de una invocación de función asincrónica es el autor de la llamada original si la invocación de la función nunca se ha suspendido o el llamador más reciente del delegado de reanudación en caso contrario.

### <a name="evaluation-of-a-task-returning-async-function"></a>Evaluación de una función asincrónica que devuelve una tarea

La invocación de una función asincrónica que devuelve una tarea hace que se genere una instancia del tipo de tarea devuelto. Esto se denomina la ***tarea devuelta*** de la función Async. La tarea está inicialmente en un estado incompleto.

A continuación, se evalúa el cuerpo de la función asincrónica hasta que se suspenda (al llegar a una expresión Await) o finalice, donde el control de punto se devuelve al autor de la llamada, junto con la tarea devuelta.

Cuando finaliza el cuerpo de la función asincrónica, la tarea devuelta se saca del estado incompleto:

*  Si el cuerpo de la función finaliza como resultado de alcanzar una instrucción return o el final del cuerpo, se registra cualquier valor de resultado en la tarea devuelta, que se pone en un estado correcto.
*  Si el cuerpo de la función finaliza como resultado de una excepción no detectada ([la instrucción throw](statements.md#the-throw-statement)), la excepción se registra en la tarea devuelta que se coloca en un estado de error.

### <a name="evaluation-of-a-void-returning-async-function"></a>Evaluación de una función asincrónica que devuelve void

Si el tipo de valor devuelto de la función Async es `void`, la evaluación difiere de la anterior de la siguiente manera: dado que no se devuelve ninguna tarea, la función comunica en su lugar la finalización y las excepciones al ***contexto de sincronización***del subproceso actual. La definición exacta del contexto de sincronización depende de la implementación, pero es una representación de "Dónde" se está ejecutando el subproceso actual. El contexto de sincronización se notifica cuando se inicia la evaluación de una función asincrónica que devuelve void, se completa correctamente o provoca que se produzca una excepción no detectada.

Esto permite que el contexto realice un seguimiento del número de funciones asincrónicas que devuelven void que se están ejecutando en él y de decidir cómo propagar las excepciones que salen de ellas.
