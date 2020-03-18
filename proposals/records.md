---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "79483881"
---
# <a name="records"></a>records

* [x] propuesto
* [] Prototipo: [completo](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)
* [] Implementación: [en curso](https://github.com/dotnet/roslyn/BRANCH_NAME)
* [] Specification: especificación de borrador entre comillas

## <a name="summary"></a>Resumen
[summary]: #summary

Los registros son un formulario de declaración nuevo y C# simplificado para tipos de clase y struct que combinan las ventajas de una serie de características más sencillas. Se describen las nuevas características (parámetros del receptor llamador y *with-Expression*s), se proporcionan la sintaxis y la semántica para las declaraciones de registro y, a continuación, se proporcionan algunos ejemplos.


## <a name="motivation"></a>Motivación
[motivation]: #motivation

Un número significativo de declaraciones de tipo en C# son poco más que las colecciones de datos de tipo. Desafortunadamente, la declaración de estos tipos requiere una gran cantidad de código reutilizable. *Los registros* de proporcionan un mecanismo para declarar un tipo de de de. para ello, se describen los miembros del agregado junto con el código adicional o las desviaciones del texto reutilizable habitual, si hay alguno.

Vea los [ejemplos](#examples)siguientes.

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a>parámetros del autor de la llamada

Actualmente *, el argumento predeterminado* de un parámetro de método debe ser 
- *expresión constante*; de
- una expresión con el formato `new S()` donde `S` es un tipo de valor; de
- una expresión con el formato `default(S)` donde `S` es un tipo de valor

Lo ampliamos para agregar lo siguiente
- una expresión con el formato `this.Identifier`

Este nuevo formulario se denomina *argumento predeterminado del receptor del autor*de la llamada y solo se permite si se cumplen todos los elementos siguientes.
- El método en el que aparece es un método de instancia; etc
- La expresión `this.Identifier` enlaza a un miembro de instancia del tipo envolvente, que debe ser un campo o una propiedad; etc
- El miembro al que se enlaza (y el descriptor de acceso `get`, si es una propiedad) es al menos tan accesible como el método; etc
- El tipo de `this.Identifier` es convertible implícitamente por una identidad o conversión que acepta valores NULL en el tipo del parámetro (se trata de una restricción existente en el *argumento predeterminado*).

Cuando se omite un argumento de una invocación de un miembro de función para un parámetro opcional correspondiente con un *argumento predeterminado del receptor del autor*de la llamada, se pasa implícitamente el valor del miembro del receptor. 

> **Notas de diseño**: la razón principal del parámetro Caller-Receiver es admitir la *expresión with*. La idea es que puede declarar un método como este.
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> y, a continuación, úsela como esta
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> Para crear un nuevo `Point` igual que una `Point` existente pero con el valor de `X` cambiado.
> 
> Es una pregunta abierta si merece la pena agregar o no la forma sintáctica de la *expresión with* , ya que se admiten los parámetros del receptor del llamador, por lo que es posible hacerlo en *lugar de* hacerlo *además* de *with-Expression*.

- [] **Problema abierto**: ¿Cuál es el orden en el que se evalúa un *argumento-receptor predeterminado del autor* de la llamada con respecto a otros argumentos? ¿Debemos decir que no está especificado?

### <a name="with-expressions"></a>Expresiones with

Se propone un nuevo formulario de expresión:

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

El token `with` es una nueva palabra clave contextual.

Cada *identificador* a la izquierda de un *with_initializer* debe enlazarse a un campo o propiedad de instancia accesible del tipo del *primary_expression* de la *with_expression*. Es posible que no haya ningún nombre duplicado entre estos identificadores de un *with_expression*determinado.

*With_expression* del formulario

> *e1* `with` *identificador* de `{` = *e2*,... `}`

se trata como una invocación del formulario

> *e1*`.With(`*identificador2*`:` *e2*,...`)`

En el caso de cada método denominado `With` que sea un miembro de instancia accesible de *E1*, se selecciona *identificador2* como el nombre del primer parámetro de ese método que tiene un parámetro de receptor llamador que es el mismo miembro que el campo de instancia o la propiedad enlazada a *Identifier*. Si no se puede identificar este parámetro, no se debe tener en cuenta el método. El método que se va a invocar se selecciona entre los candidatos restantes por resolución de sobrecarga.

> **Notas de diseño**: determinados parámetros del receptor de la llamada, muchas de las ventajas de la *expresión with* están disponibles sin esta forma de sintaxis especial. Por lo tanto, estamos considerando si es necesario o no. Su principal ventaja es permitir que uno programe en función de los nombres de campos y propiedades, en lugar de los nombres de los parámetros. De esta manera, se mejora la legibilidad y la calidad de las herramientas (por ejemplo, la ir a la definición en el identificador de un *with_expression* se navegaría a la propiedad en lugar de a un parámetro de método).

- [] **Problema abierto**: esta descripción debe modificarse para admitir métodos de extensión.

### <a name="pattern-matching"></a>coincidencia de patrones

Vea la [especificación de coincidencia de patrones](csharp-8.0/patterns.md#positional-pattern) para obtener una especificación de `Deconstruct` y su relación con la coincidencia de patrones.

> **Notas de diseño**: en virtud del `Deconstruct` generado por el compilador tal y como se especifica aquí, y la especificación para la coincidencia de patrones, una declaración de registro
> ```cs
> public class Point(int X, int Y);
> ```
> admitirá la coincidencia de patrones posicionales como se indica a continuación.
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a>declaraciones de tipos de registro

La sintaxis de una declaración de `class` o `struct` se extiende para admitir parámetros de valor; los parámetros se convierten en propiedades del tipo:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> **Notas de diseño**: dado que los tipos de registro suelen ser útiles sin necesidad de que los miembros se declaren explícitamente en un cuerpo de clase, se modifica la sintaxis de la declaración para permitir que un cuerpo sea simplemente un punto y coma.

Una clase (struct) declarada con los *parámetros de registro* se denomina *clase de registro* (*struct record*), cualquiera de los cuales es un *tipo de registro*.

- [] **Problema abierto**: necesitamos incluir *primary_constructor_body* en la gramática para que pueda aparecer dentro de una declaración de tipo de registro.
- [] **Problema abierto**: ¿Cuáles son las reglas de conflicto de nombres para los nombres de parámetro? Presumiblemente uno no puede entrar en conflicto con un parámetro de tipo u otro *parámetro de registro*.
- [] **Problema abierto**: es necesario especificar el ámbito de los parámetros de registro. ¿Dónde se pueden usar? Presumiblemente dentro de los inicializadores de campo de instancia y *primary_constructor_body* al menos.
- [] **Problema abierto**: ¿una declaración de tipo de registro es parcial? Si es así, ¿deben repetirse los parámetros en cada parte?

#### <a name="members-of-a-record-type"></a>Miembros de un tipo de registro

Además de los miembros declarados en el *cuerpo de clase*, un tipo de registro tiene los siguientes miembros adicionales:

##### <a name="primary-constructor"></a>Constructor principal

Un tipo de registro tiene un constructor de `public` cuya firma corresponde a los parámetros de valor de la declaración de tipos. Esto se denomina *constructor principal* para el tipo y hace que se suprima el *constructor predeterminado* declarado implícitamente.

En tiempo de ejecución del constructor principal

* Inicializa los campos de respaldo generados por el compilador para las propiedades correspondientes a los parámetros de valor (si estas propiedades son proporcionadas por el compilador; [vea 1.1.2](#1.1.2)); a
* ejecuta los inicializadores de campo de instancia que aparecen en el *cuerpo de la clase*; y después
* invoca un constructor de clase base:
    * Si hay argumentos en el *record_base_arguments*, se invoca un constructor base seleccionado por la resolución de sobrecarga con estos argumentos;
    * De lo contrario, se invoca un constructor base sin argumentos.
* ejecuta el cuerpo de cada *primary_constructor_body*, si existe, en el orden de origen.

- [] **Problema abierto**: es necesario especificar ese orden, especialmente en las unidades de compilación de las parciales.
- [] **Problema abierto**: es necesario especificar que cada constructor declarado explícitamente debe encadenarse al constructor principal.
- [] **Problema abierto**: ¿se debe permitir cambiar el modificador de acceso en el constructor principal?
- [] **Problema abierto**: en una estructura de registro, es un error que no haya ningún parámetro de registro?

##### <a name="primary-constructor-body"></a>Cuerpo del constructor principal

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

Un *primary_constructor_body* solo se puede usar dentro de una declaración de tipo de registro. El *identificador* de un *primary_constructor_body* debe nombrar el tipo de registro en el que se declara.

El *primary_constructor_body* no declara un miembro por sí mismo, pero es una forma de que el programador proporcione atributos para y especifica el acceso de un constructor principal de un tipo de registro. También permite al programador proporcionar código adicional que se ejecutará cuando se construya una instancia del tipo de registro.

- [] **Problema abierto**: debemos tener en cuenta que un constructor predeterminado de struct lo omite.
- [] **Problema abierto**: deberíamos especificar el orden de ejecución de la inicialización.
- [] **Problema abierto**: ¿se debe permitir algo como un *primary_constructor_body* (supuestamente sin atributos y modificadores) en una declaración de tipo no de registro y tratarlo como se haría con el código de un inicializador de campo de instancia?

##### <a name="properties"></a>Propiedades

Para cada parámetro de registro de una declaración de tipo de registro, hay un miembro de propiedad `public` correspondiente cuyo nombre y tipo se toman de la declaración del parámetro value. Su nombre es el *identificador* de la *record_property_name*, si está presente, o el *identificador* de la *record_parameter* en caso contrario. Si no hay ninguna propiedad pública concreta (es decir, no abstracta) con un descriptor de acceso `get` y con este nombre y tipo declarados o heredados explícitamente, el compilador los genera de la manera siguiente:

* Para un *struct de registro* o una *clase de registro*de `sealed`:
 * Un `private` campo `readonly` se genera como un campo de respaldo para una propiedad `readonly`. Su valor se inicializa durante la construcción con el valor del parámetro del constructor principal correspondiente.
 * El descriptor de acceso `get` de la propiedad se implementa para devolver el valor del campo de respaldo.
 * Los descriptores de acceso `get` de la propiedad virtual heredada "matching" se reemplazan.

> **Notas de diseño**: en otras palabras, si extiende una clase base o implementa una interfaz que declara una propiedad abstracta pública con el mismo nombre y tipo que un parámetro de registro, esa propiedad se invalida o se implementa.

- [] **Problema abierto**: ¿es posible cambiar el modificador de acceso en una propiedad cuando se declara explícitamente?
- [] **Problema abierto**: ¿debería ser posible sustituir un campo por una propiedad?

##### <a name="object-methods"></a>Métodos de objeto

Para un *struct de registro* o una *clase de registro*de `sealed`, el compilador produce implementaciones de los métodos `object.GetHashCode()` y `object.Equals(object)` a menos que el usuario lo proporcione.

- [] **Problema abierto**: debemos especificar con precisión su implementación.
- [] **Problema abierto**: también debemos agregar la interfaz `IEquatable<T>` para el tipo de registro y especificar que se proporcionen las implementaciones.
- [] **Problema abierto**: también se debe especificar que se implementen todos los `IEquatable<T>.Equals`.
- [] **Problema abierto**: debemos especificar con exactitud cómo se resuelve el problema de Equals en la herencia de registros: en concreto, cómo se generan métodos de igualdad, de modo que son simétricos, transitivos, reflexivos, etc.
- [] **Problema abierto**: se ha propuesto que implementemos `operator ==` y `operator !=` para tipos de registro.
- [] **Problema abierto**: ¿se debe generar automáticamente una implementación de `object.ToString`?

##### `Deconstruct`

Un tipo de registro tiene un método `public` generado por el compilador `void Deconstruct` a menos que el usuario proporcione uno con cualquier firma. Cada parámetro es un parámetro `out` con el mismo nombre y tipo que el parámetro correspondiente del tipo de registro. La implementación proporcionada por el compilador de este método asignará cada `out` parámetro con el valor de la propiedad correspondiente.

Vea [la especificación de coincidencia de patrones](csharp-8.0/patterns.md#positional-pattern) para la semántica de `Deconstruct`.

##### <a name="with-method"></a>Método`With`

A menos que haya un miembro declarado por el usuario denominado `With` declarado, un tipo de registro tiene un método proporcionado por el compilador denominado `With` cuyo tipo de valor devuelto es el propio tipo de registro y que contiene un parámetro de valor que corresponde a cada *parámetro de registro* en el mismo orden en que aparecen en la declaración de tipo de registro. Cada parámetro debe tener un *argumento predeterminado del receptor del llamador* de la propiedad correspondiente.

En una clase de registro de `abstract`, el método de `With` proporcionado por el compilador es abstracto. En una estructura de registro o una clase de registro sellada, el método de `With` proporcionado por el compilador es `sealed`. De lo contrario, el método de `With` proporcionado por el compilador es ' virtual y su implementación devolverá una nueva instancia de generada al invocar el constructor principal con los parámetros como argumentos para crear una nueva instancia a partir de los parámetros y devolver esa nueva instancia.

- [] **Problema abierto**: también debemos especificar en qué condiciones invalidamos o implementamos métodos `With` virtuales heredados o métodos de `With` de interfaces implementadas.
- [] **Problema abierto**: deberíamos indicar lo que sucede cuando se hereda un método de `With` no virtual.

> **Notas de diseño**: dado que los tipos de registro son inmutables de forma predeterminada, el método `With` proporciona una forma de crear una nueva instancia de que es igual que una instancia existente, pero con las propiedades seleccionadas en función de los nuevos valores. Por ejemplo, dado
> ```cs
> public class Point(int X, int Y);
> ```
> existe un miembro proporcionado por el compilador
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> Que habilita una variable del tipo de registro
> ```cs
> var p = new Point(3, 4);
> ```
> para reemplazar por una instancia que tiene una o varias propiedades diferentes
> ```cs
>     p = p.With(X: 5);
> ```
> Esto también se puede expresar mediante el *with_expression*:
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a>Ejemplos

#### <a name="compatibility-of-record-types"></a>Compatibilidad de tipos de registro

Dado que el programador puede Agregar miembros a una declaración de tipo de registro, a menudo es posible cambiar el conjunto de elementos de registro sin que ello afecte a los clientes existentes. Por ejemplo, dada una versión inicial de un tipo de registro

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

Un nuevo elemento del tipo de registro puede agregarse de compatibilidad en la siguiente revisión del tipo sin afectar a la compatibilidad binaria o de origen:

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a>ejemplo de struct de registro

Este struct de registro

```cs
public struct Pair(object First, object Second);
```

se convierte en este código

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- [] **Problema abierto**: ¿la implementación de es igual a (otro par) ser un miembro público de pair?
- [] **Problema abierto**: esta implementación de `Equals` no es simétrica en el aspecto de la herencia.
- [] **Problema abierto**: ¿debe un registro declarar `operator ==` y `operator !=`?

> **Notas de diseño**: dado que un tipo de registro puede heredar de otro y esta implementación de `Equals` no sería simétrica en ese caso, no es correcto. Se propone implementar la igualdad de esta manera:
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> Los registros derivados `override EqualityContract`. La alternativa menos atractiva es restringir la herencia.

#### <a name="sealed-record-example"></a>ejemplo de registro sellado

Esta clase de registro sellado

```cs
public sealed class Student(string Name, decimal Gpa);
```

se convierte en este código

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a>ejemplo de clase de registro abstracta

Esta clase de registro abstracto

```cs
public abstract class Person(string Name);
```

se convierte en este código

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a>combinación de registros abstractos y sellados

Dada la clase de registro Abstract `Person` anterior, esta clase de registro sellado

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

se convierte en este código

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Al igual que con cualquier característica de lenguaje, debemos cuestionar si se reembolsa la complejidad adicional en el idioma en la claridad adicional que se C# ofrece al cuerpo de los programas que se beneficiarían de la característica.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Consideramos agregar *constructores principales* en C# 6. Aunque ocupan la misma superficie sintáctica que esta propuesta, descubrimos que no eran las ventajas que ofrecen los registros.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

Las preguntas abiertas aparecen en el cuerpo de la propuesta.

## <a name="design-meetings"></a>Design Meetings

TBD
