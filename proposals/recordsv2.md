---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "79483929"
---

# <a name="records-v2"></a>Registros V2

En el pasado, hemos pensado los registros como una característica que permite trabajar con datos.

"Trabajar con datos" es un grupo grande con varias caras, por lo que puede ser interesante examinar cada una de ellas de forma aislada. Comencemos observando un ejemplo de registros hoy y algunas de sus desventajas.

Por ejemplo, un registro simple se definiría hoy de la manera siguiente:

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

y el uso se leerá

```C#
void M()
{
    var userInfo = new UserInfo() 
    {
        Username = "andy",
        Email = "angocke@microsoft.com",
        IsAdmin = true
    };
}
```

Este código tiene importantes ventajas:

1. La definición es resistente a la versión, las propiedades pueden agregarse o moverse con facilidad
2. Las propiedades se pueden establecer en cualquier orden y los nombres de la inicialización siempre coinciden con los descriptores de acceso
3. Simplemente se pueden omitir las propiedades con valores predeterminados

El primer error es que las propiedades deben ser ahora mutables.

# <a name="mutability"></a>Mutabilidad

Lo que nos gustaría es C# para proporcionar una manera de establecer un miembro de `readonly` en inicializadores de objeto.
Dado que es posible que algunos tipos no se hayan diseñado pensando en esta inicialización, también nos gustaría participar.

La solución propuesta es un nuevo modificador, `initonly`, que se puede aplicar a propiedades y campos:

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

El CODEGEN para esto es sorprendentemente sencillo: simplemente se establece el campo de solo lectura.
En concreto, las propiedades disminuidas tendrían el siguiente aspecto:

```C#
public class UserInfo
{
    private readonly string <Backing>_username;
    public string get_Username() => <Backing>_username;
    [return: modreq(initonly)]
    public void set_Username(string value) { <Backing>_username = value; }
    ...
}
```

CLR considera la configuración de campos de solo lectura como no comprobables, pero no no seguros. Para admitir un comprobador más avanzado, se propone la siguiente regla: un campo de solo lectura solo puede modificarse dentro de los métodos de `initonly`, o en un nuevo objeto que se encuentre en la pila de CLR y no se haya publicado a través de una llamada de método o almacén.

Esto debería resolver muchos de los problemas con mutabilidad en el `UserInfo` ejemplo, aunque no requiere estrategias de emisión complicadas o frágiles. Sin embargo, la inmutabilidad presenta un nuevo problema: crear fácilmente un objeto con cambios.

# <a name="with-ing"></a>With-Ing

Al programar con inmutabilidad, la realización de cambios en un objeto se realiza mediante la creación de una copia con cambios en lugar de realizar los cambios directamente en el objeto. Desafortunadamente, no hay ninguna manera cómoda de hacer esto en C#, incluso con los registros de estilo actual. Antes se ha propuesto que se proporcione algún tipo de método "with" generado automáticamente para los registros que implementan esa funcionalidad. Si tenemos un mecanismo de este tipo, es importante que funcione con `initonly` miembros. Para lograr esto, se propone que agreguemos una expresión de `with`, análoga a un inicializador de objeto. El uso de ejemplo sería el siguiente:

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

El objeto `newUserName` resultante sería una copia de `userInfo`, con `Username` establecida en "angocke".
La CODEGEN en la expresión `with` también sería similar al inicializador de objeto: se construye un nuevo objeto y, a continuación, se llama a la `initonly` `Username` establecedor en el cuerpo del método.

Por supuesto, la diferencia es que el nuevo objeto que se está construyendo no es una simple creación de un objeto nuevo, sino que es un duplicado del objeto original. Para proporcionar esta funcionalidad, es necesario que el objeto proporcione un "con constructor" que proporcione un objeto duplicado. Un `With` constructor de ejemplo tendría el siguiente aspecto:

```C#
class UserInfo
{
    ...
    [WithConstructor] // placeholder syntax, up for debate
    public UserInfo With()
    {
        return new UserInfo() { Username = this.Username, Email = this.Email, IsAdmin = this.IsAdmin };
    }
}
```

En particular, la expresión `with` establecerá `initonly` miembros, al igual que el inicializador de objeto, por lo que para admitir la comprobación, debemos asegurarnos de que el objeto no se haya publicado antes de que se establezcan los miembros `initonly`. Para aplicar esto, el atributo `WithConstructor` (o la sintaxis equivalente) aplicará una nueva regla para el método: todas las instrucciones Return deben contener directamente una expresión de creación de objeto, posiblemente con un inicializador de objeto.

Si el constructor `With` requiere validación, el usuario puede introducir un constructor para realizar esa validación; por ejemplo,

```C#
class UserInfo
{
    ...
    private UserInfo(UserInfo original)
    {
        // validation code
    }
    [WithConstructor]
    public UserInfo With() => new UserInfo(this);
}
```

La última parte de la complejidad asociada a `With` es la herencia. Si el registro es extensible, tendrá que proporcionar una nueva `With` para la subclase. Esto se puede lograr de la siguiente manera:

```C#
class Base
{
    ...
    protected Base(Base original)
    {
        // validation
    }
    [WithConstructor]
    public virtual Base With() => new Base(this);
}
class Derived : Base
{
    ...
    protected Derived(Derived original)
    : base(original)
    {
        // validation
    }
    [WithConstructor]
    public override Derived With() => new Derived(this);
}
```

Tenga en cuenta una parte adicional de la complejidad aquí: para invalidar el constructor de `With` con el tipo derivado, el lenguaje también necesitará admitir tipos de valor devueltos covariantes en las invalidaciones.
[Aquí](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)ya existe una propuesta independiente para esta característica.

**Desventajas**

- La realización de todas las instrucciones Return en `WithConstructor`s contienen nuevas expresiones de objeto es restrictiva.
  Esto podría ser mitigado por el análisis de flujo que garantiza que el nuevo objeto no escapa del método
- La compatibilidad con la varianza en las invalidaciones a través de los trucos del compilador requerirá métodos de código auxiliar, que crecerán de una vez a la profundidad de la herencia. La necesidad de un método de código auxiliar se debe a un requisito de tiempo de ejecución que invalide las firmas coincide exactamente. Si se afloja el requisito de tiempo de ejecución, los métodos de código auxiliar no serían necesarios en absoluto.
- El uso de constructores encadenados del formulario `Type(Type original)` reserva eficazmente ese constructor para el uso del modelo. Dado que los constructores tienen una semántica única y no se puede volver a asignar el nombre, podría estar limitando.


## <a name="wrapping-it-all-up-records"></a>Envolver todo: registros

Las características anteriores permiten un estilo de programación que era muy difícil antes. Pero incluso con las nuevas características, podría ser bastante detallado y propenso a errores anotar todo. También hay algunos elementos, como Equals y GetHashCode, que ya se pueden escribir hoy, solo es laborioso.
Además, un error importante en la implementación de la igualdad sobre estas nuevas primitivas es que la igualdad estructural es algo que debe cambiar con el tipo de datos a medida que se agregan nuevos datos, pero al controlarlo manualmente es probable que estas cosas no estén sincronizadas.

Por lo tanto, se propone C# que admitan la nueva sintaxis para los registros, no para proporcionar nuevas características, pero para establecer los valores predeterminados y generar código diseñado para su uso en los registros. La sintaxis de ejemplo sería similar a

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

El código generado para esta clase consideraría todos los campos públicos y las propiedades automáticas como miembros estructurales del registro. Los miembros de registro se pueden personalizar mediante un nuevo `RecordMember(bool)` atributo que podría usarse para incluir o excluir miembros. Los miembros del registro se `initonly` de forma predeterminada y la igualdad se generaría automáticamente para la clase basada en los miembros del registro. En cualquier momento, el comportamiento de estos miembros podría personalizarse simplemente declarándolos en el origen. La implementación escrita por el usuario reemplazaría la implementación predeterminada en todos los patrones de uso.

Tenga en cuenta que la igualdad en el aspecto de la herencia es compleja, pero parece que se ha resuelto adecuadamente en la [otra propuesta de registros](records.md).

## <a name="primary-constructors"></a>Constructores principales

La propuesta de registro anterior también incluye una nueva sintaxis para una lista de parámetros en el propio tipo; por ejemplo,

```C#
class Point(int X, int Y);
```

En el nuevo diseño, la lista de parámetros sería una característica C# ortogonal, que se podía integrar perfectamente con los registros. Si un constructor principal está incluido en un registro, tendría nuevos valores predeterminados, al igual que los campos públicos y las propiedades automáticas: los parámetros del constructor principal se utilizarían para generar propiedades públicas de miembro de registro con el mismo nombre. Además, el constructor principal se podría usar ahora para generar automáticamente un Deconstructor.

Por ejemplo, el Registro siguiente con un constructor principal

```C#
data class Point(int X, int Y);
```

sería equivalente a

```C#
data class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }
}
```

y la última generación de lo anterior sería

```C#
class Point
{
    public initonly int X { get; }
    public initonly int Y { get; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    protected Point(Point other)
    : this(other.X, other.Y)
    { }

    [WithConstructor]
    public virtual Point With() => new Point(this);

    public void Deconstruct(out int X, out int Y)
    {
        X = this.X;
        Y = this.Y;
    }

    // Generated equality
}
```

Tenga en cuenta que se ha tomado otra parte de la información para una clase de datos con un constructor principal: en lugar de establecer los campos principales dentro del constructor protegido generado, se delega en el constructor principal. Si la clase Point tuviera otro miembro de registro no principal, por ejemplo,

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

a continuación, se cambiaría el constructor protegido generado de la siguiente manera:

```C#
class Point
{
    // ...
    protected Point(Point other)
    : this(other.X, other.Y)
    {
        Z = other.Z;
    }
    // ...
}
```

En particular, esto no responde qué hacer con la herencia de registros con constructores principales. Por ejemplo,

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

En lugar de resolver de forma arbitraria, un enfoque más explícito podría requerir que se proporcione una lista de parámetros con la lista base, por ejemplo,

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

A continuación, la lista de parámetros de la lista base se aplicaría a una llamada `base` en el constructor principal generado:

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

En lo que se refiere a lo que un constructor principal podría significar fuera de un registro, todavía está abierto a una propuesta adicional.
