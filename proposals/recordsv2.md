---
ms.openlocfilehash: 5636157ba54e93587847d6f2f7aac2dc675f3112
ms.sourcegitcommit: af27912886f22cda9b98b7769447d85cd9732736
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "79483929"
---

# <a name="records-v2"></a><span data-ttu-id="6d62c-101">Registros V2</span><span class="sxs-lookup"><span data-stu-id="6d62c-101">Records v2</span></span>

<span data-ttu-id="6d62c-102">En el pasado, hemos pensado los registros como una característica que permite trabajar con datos.</span><span class="sxs-lookup"><span data-stu-id="6d62c-102">In the past we've thought about records as a feature to enable working with data.</span></span>

<span data-ttu-id="6d62c-103">"Trabajar con datos" es un grupo grande con varias caras, por lo que puede ser interesante examinar cada una de ellas de forma aislada.</span><span class="sxs-lookup"><span data-stu-id="6d62c-103">"Working with data" is a big group with a number of facets, so it may be interesting to look at each in isolation.</span></span> <span data-ttu-id="6d62c-104">Comencemos observando un ejemplo de registros hoy y algunas de sus desventajas.</span><span class="sxs-lookup"><span data-stu-id="6d62c-104">Let's start by looking at an example of records today and some of its drawbacks.</span></span>

<span data-ttu-id="6d62c-105">Por ejemplo, un registro simple se definiría hoy de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="6d62c-105">For instance, a simple record would be defined today as follows</span></span>

```C#
public class UserInfo
{
    public string Username { get; set; }
    public string Email { get; set; }
    public bool IsAdmin  { get; set; } = false;
}
```

<span data-ttu-id="6d62c-106">y el uso se leerá</span><span class="sxs-lookup"><span data-stu-id="6d62c-106">and the usage would read</span></span>

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

<span data-ttu-id="6d62c-107">Este código tiene importantes ventajas:</span><span class="sxs-lookup"><span data-stu-id="6d62c-107">There are significant advantages in this code:</span></span>

1. <span data-ttu-id="6d62c-108">La definición es resistente a la versión, las propiedades pueden agregarse o moverse con facilidad</span><span class="sxs-lookup"><span data-stu-id="6d62c-108">The definition is version resilient, properties can easily be added or moved</span></span>
2. <span data-ttu-id="6d62c-109">Las propiedades se pueden establecer en cualquier orden y los nombres de la inicialización siempre coinciden con los descriptores de acceso</span><span class="sxs-lookup"><span data-stu-id="6d62c-109">Properties can be set in any order, and the names in the initialization always match the accessors</span></span>
3. <span data-ttu-id="6d62c-110">Simplemente se pueden omitir las propiedades con valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="6d62c-110">Properties with default values can simply be skipped</span></span>

<span data-ttu-id="6d62c-111">El primer error es que las propiedades deben ser ahora mutables.</span><span class="sxs-lookup"><span data-stu-id="6d62c-111">The first flaw is that the properties must now be mutable.</span></span>

# <a name="mutability"></a><span data-ttu-id="6d62c-112">Mutabilidad</span><span class="sxs-lookup"><span data-stu-id="6d62c-112">Mutability</span></span>

<span data-ttu-id="6d62c-113">Lo que nos gustaría es C# para proporcionar una manera de establecer un miembro de `readonly` en inicializadores de objeto.</span><span class="sxs-lookup"><span data-stu-id="6d62c-113">What we'd like is for C# to provide a way to set a `readonly` member in object initializers.</span></span>
<span data-ttu-id="6d62c-114">Dado que es posible que algunos tipos no se hayan diseñado pensando en esta inicialización, también nos gustaría participar.</span><span class="sxs-lookup"><span data-stu-id="6d62c-114">Since some types may not have been designed with this initialization in mind, we'd also like it to be opt-in.</span></span>

<span data-ttu-id="6d62c-115">La solución propuesta es un nuevo modificador, `initonly`, que se puede aplicar a propiedades y campos:</span><span class="sxs-lookup"><span data-stu-id="6d62c-115">The proposed solution is a new modifier, `initonly`, that can be applied to properties and fields:</span></span>

```C#
public class UserInfo
{
    public initonly string Username { get; }
    public initonly string Email { get; }
    public initonly bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="6d62c-116">El CODEGEN para esto es sorprendentemente sencillo: simplemente se establece el campo de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="6d62c-116">The codegen for this is surprisingly straight forward: we just set the readonly field.</span></span>
<span data-ttu-id="6d62c-117">En concreto, las propiedades disminuidas tendrían el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="6d62c-117">Specifically, the lowered properties would look like:</span></span>

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

<span data-ttu-id="6d62c-118">CLR considera la configuración de campos de solo lectura como no comprobables, pero no no seguros.</span><span class="sxs-lookup"><span data-stu-id="6d62c-118">The CLR considers setting readonly fields to be unverifiable, but not unsafe.</span></span> <span data-ttu-id="6d62c-119">Para admitir un comprobador más avanzado, se propone la siguiente regla: un campo de solo lectura solo puede modificarse dentro de los métodos de `initonly`, o en un nuevo objeto que se encuentre en la pila de CLR y no se haya publicado a través de una llamada de método o almacén.</span><span class="sxs-lookup"><span data-stu-id="6d62c-119">To support a more advanced verifier, the following rule is proposed: a readonly field can be modified only inside `initonly` methods, or on a new object that is on the CLR stack and has not been published via a store or method call.</span></span>

<span data-ttu-id="6d62c-120">Esto debería resolver muchos de los problemas con mutabilidad en el `UserInfo` ejemplo, aunque no requiere estrategias de emisión complicadas o frágiles.</span><span class="sxs-lookup"><span data-stu-id="6d62c-120">This should solve many of the problems with mutability in the `UserInfo` example, while not requiring complicated or brittle emit strategies.</span></span> <span data-ttu-id="6d62c-121">Sin embargo, la inmutabilidad presenta un nuevo problema: crear fácilmente un objeto con cambios.</span><span class="sxs-lookup"><span data-stu-id="6d62c-121">However, immutability does present a new problem: easily constructing an object with changes.</span></span>

# <a name="with-ing"></a><span data-ttu-id="6d62c-122">With-Ing</span><span class="sxs-lookup"><span data-stu-id="6d62c-122">With-ing</span></span>

<span data-ttu-id="6d62c-123">Al programar con inmutabilidad, la realización de cambios en un objeto se realiza mediante la creación de una copia con cambios en lugar de realizar los cambios directamente en el objeto.</span><span class="sxs-lookup"><span data-stu-id="6d62c-123">When programming with immutability, making changes to an object is done by constructing a copy with changes instead of making the changes directly on the object.</span></span> <span data-ttu-id="6d62c-124">Desafortunadamente, no hay ninguna manera cómoda de hacer esto en C#, incluso con los registros de estilo actual.</span><span class="sxs-lookup"><span data-stu-id="6d62c-124">Unfortunately, there's no convenient way to do this in C#, even with current-style records.</span></span> <span data-ttu-id="6d62c-125">Antes se ha propuesto que se proporcione algún tipo de método "with" generado automáticamente para los registros que implementan esa funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="6d62c-125">It's been previously proposed that some kind of autogenerated "With" method be provided for records that implements that functionality.</span></span> <span data-ttu-id="6d62c-126">Si tenemos un mecanismo de este tipo, es importante que funcione con `initonly` miembros.</span><span class="sxs-lookup"><span data-stu-id="6d62c-126">If we have such a mechanism, it's important that it work with `initonly` members.</span></span> <span data-ttu-id="6d62c-127">Para lograr esto, se propone que agreguemos una expresión de `with`, análoga a un inicializador de objeto.</span><span class="sxs-lookup"><span data-stu-id="6d62c-127">To achieve this, it's proposed that we add a `with` expression, analogous to an object initializer.</span></span> <span data-ttu-id="6d62c-128">El uso de ejemplo sería el siguiente:</span><span class="sxs-lookup"><span data-stu-id="6d62c-128">Sample usage would be as follows:</span></span>

```C#
var userInfo = new UserInfo() 
{
    Username = "andy",
    Email = "angocke@microsoft.com",
    IsAdmin = true
};
var newUserName = userInfo with { Username = "angocke" };
```

<span data-ttu-id="6d62c-129">El objeto `newUserName` resultante sería una copia de `userInfo`, con `Username` establecida en "angocke".</span><span class="sxs-lookup"><span data-stu-id="6d62c-129">The resulting `newUserName` object would be a copy of `userInfo`, with `Username` set to "angocke".</span></span>
<span data-ttu-id="6d62c-130">La CODEGEN en la expresión `with` también sería similar al inicializador de objeto: se construye un nuevo objeto y, a continuación, se llama a la `initonly` `Username` establecedor en el cuerpo del método.</span><span class="sxs-lookup"><span data-stu-id="6d62c-130">The codegen on the `with` expression would also be similar to the object initializer: a new object is constructed, and then the `initonly` `Username` setter would be called in the method body.</span></span>

<span data-ttu-id="6d62c-131">Por supuesto, la diferencia es que el nuevo objeto que se está construyendo no es una simple creación de un objeto nuevo, sino que es un duplicado del objeto original.</span><span class="sxs-lookup"><span data-stu-id="6d62c-131">Of course, the difference here is that the new object being constructed is not a simple new object creation, it is a duplicate of the original object.</span></span> <span data-ttu-id="6d62c-132">Para proporcionar esta funcionalidad, es necesario que el objeto proporcione un "con constructor" que proporcione un objeto duplicado.</span><span class="sxs-lookup"><span data-stu-id="6d62c-132">To provide this functionality, we require that the object provide a "With constructor" that provides a duplicate object.</span></span> <span data-ttu-id="6d62c-133">Un `With` constructor de ejemplo tendría el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="6d62c-133">A sample `With` constructor would look like:</span></span>

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

<span data-ttu-id="6d62c-134">En particular, la expresión `with` establecerá `initonly` miembros, al igual que el inicializador de objeto, por lo que para admitir la comprobación, debemos asegurarnos de que el objeto no se haya publicado antes de que se establezcan los miembros `initonly`.</span><span class="sxs-lookup"><span data-stu-id="6d62c-134">Notably, the `with` expression will set `initonly` members, just like the object initializer, so to support verification we must ensure that the object cannot have been published before the `initonly` members are set.</span></span> <span data-ttu-id="6d62c-135">Para aplicar esto, el atributo `WithConstructor` (o la sintaxis equivalente) aplicará una nueva regla para el método: todas las instrucciones Return deben contener directamente una expresión de creación de objeto, posiblemente con un inicializador de objeto.</span><span class="sxs-lookup"><span data-stu-id="6d62c-135">To enforce this, the `WithConstructor` attribute (or equivalent syntax) will enforce a new rule for the method: all return statements must directly contain an object creation expression, possibly with an object initializer.</span></span>

<span data-ttu-id="6d62c-136">Si el constructor `With` requiere validación, el usuario puede introducir un constructor para realizar esa validación; por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="6d62c-136">If the `With` constructor requires validation, the user may introduce a constructor to do that validation, e.g.</span></span>

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

<span data-ttu-id="6d62c-137">La última parte de la complejidad asociada a `With` es la herencia.</span><span class="sxs-lookup"><span data-stu-id="6d62c-137">The last piece of complexity associated with `With` is inheritance.</span></span> <span data-ttu-id="6d62c-138">Si el registro es extensible, tendrá que proporcionar una nueva `With` para la subclase.</span><span class="sxs-lookup"><span data-stu-id="6d62c-138">If your record is extensible, you will need to provide a new `With` for the subclass.</span></span> <span data-ttu-id="6d62c-139">Esto se puede lograr de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="6d62c-139">This can be achieved as follows:</span></span>

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

<span data-ttu-id="6d62c-140">Tenga en cuenta una parte adicional de la complejidad aquí: para invalidar el constructor de `With` con el tipo derivado, el lenguaje también necesitará admitir tipos de valor devueltos covariantes en las invalidaciones.</span><span class="sxs-lookup"><span data-stu-id="6d62c-140">Note one additional piece of complexity here: in order to override the `With` constructor with the derived type the language will also need to support covariant return types in overrides.</span></span>
<span data-ttu-id="6d62c-141">[Aquí](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md)ya existe una propuesta independiente para esta característica.</span><span class="sxs-lookup"><span data-stu-id="6d62c-141">There is already a separate proposal for this feature [here](https://github.com/dotnet/csharplang/blob/725763343ad44a9251b03814e6897d87fe553769/proposals/covariant-returns.md).</span></span>

<span data-ttu-id="6d62c-142">**Desventajas**</span><span class="sxs-lookup"><span data-stu-id="6d62c-142">**Drawbacks**</span></span>

- <span data-ttu-id="6d62c-143">La realización de todas las instrucciones Return en `WithConstructor`s contienen nuevas expresiones de objeto es restrictiva.</span><span class="sxs-lookup"><span data-stu-id="6d62c-143">Making all return statements in `WithConstructor`s contain new object expressions is restrictive.</span></span>
  <span data-ttu-id="6d62c-144">Esto podría ser mitigado por el análisis de flujo que garantiza que el nuevo objeto no escapa del método</span><span class="sxs-lookup"><span data-stu-id="6d62c-144">This could be possibly be mitigated by flow analysis that ensures the new object doesn't escape the method</span></span>
- <span data-ttu-id="6d62c-145">La compatibilidad con la varianza en las invalidaciones a través de los trucos del compilador requerirá métodos de código auxiliar, que crecerán de una vez a la profundidad de la herencia.</span><span class="sxs-lookup"><span data-stu-id="6d62c-145">Supporting variance in overrides through compiler tricks will require stub methods, which will grow quadratically with the inheritance depth.</span></span> <span data-ttu-id="6d62c-146">La necesidad de un método de código auxiliar se debe a un requisito de tiempo de ejecución que invalide las firmas coincide exactamente.</span><span class="sxs-lookup"><span data-stu-id="6d62c-146">The need for a stub method is due to a runtime requirement that override signatures match exactly.</span></span> <span data-ttu-id="6d62c-147">Si se afloja el requisito de tiempo de ejecución, los métodos de código auxiliar no serían necesarios en absoluto.</span><span class="sxs-lookup"><span data-stu-id="6d62c-147">If the runtime requirement were loosened, the stub methods would not be required at all.</span></span>
- <span data-ttu-id="6d62c-148">El uso de constructores encadenados del formulario `Type(Type original)` reserva eficazmente ese constructor para el uso del modelo.</span><span class="sxs-lookup"><span data-stu-id="6d62c-148">Using chained constructors of the form `Type(Type original)` effectively reserves that constructor for the use of the pattern.</span></span> <span data-ttu-id="6d62c-149">Dado que los constructores tienen una semántica única y no se puede volver a asignar el nombre, podría estar limitando.</span><span class="sxs-lookup"><span data-stu-id="6d62c-149">Since constructors have unique semantics and cannot be re-named this could be limiting.</span></span>


## <a name="wrapping-it-all-up-records"></a><span data-ttu-id="6d62c-150">Envolver todo: registros</span><span class="sxs-lookup"><span data-stu-id="6d62c-150">Wrapping it all up: Records</span></span>

<span data-ttu-id="6d62c-151">Las características anteriores permiten un estilo de programación que era muy difícil antes.</span><span class="sxs-lookup"><span data-stu-id="6d62c-151">The above features enable a style of programming that was very difficult before.</span></span> <span data-ttu-id="6d62c-152">Pero incluso con las nuevas características, podría ser bastante detallado y propenso a errores anotar todo.</span><span class="sxs-lookup"><span data-stu-id="6d62c-152">But even with the new features it could be quite verbose and error prone to annotate everything yourself.</span></span> <span data-ttu-id="6d62c-153">También hay algunos elementos, como Equals y GetHashCode, que ya se pueden escribir hoy, solo es laborioso.</span><span class="sxs-lookup"><span data-stu-id="6d62c-153">There are also a few items, like Equals and GetHashCode, which can already be written today, it's just laborious.</span></span>
<span data-ttu-id="6d62c-154">Además, un error importante en la implementación de la igualdad sobre estas nuevas primitivas es que la igualdad estructural es algo que debe cambiar con el tipo de datos a medida que se agregan nuevos datos, pero al controlarlo manualmente es probable que estas cosas no estén sincronizadas.</span><span class="sxs-lookup"><span data-stu-id="6d62c-154">Moreover, a significant flaw in implementing equality on top of these new primitives is that structural equality is something that should change with your data type as new data is added, but when handling it manually it is likely that these things can get out of sync.</span></span>

<span data-ttu-id="6d62c-155">Por lo tanto, se propone C# que admitan la nueva sintaxis para los registros, no para proporcionar nuevas características, pero para establecer los valores predeterminados y generar código diseñado para su uso en los registros.</span><span class="sxs-lookup"><span data-stu-id="6d62c-155">Therefore, it is proposed that C# support new syntax for records, not for providing new features, but for setting defaults and generating code designed for use in records.</span></span> <span data-ttu-id="6d62c-156">La sintaxis de ejemplo sería similar a</span><span class="sxs-lookup"><span data-stu-id="6d62c-156">Example syntax would look like</span></span>

```C#
data class UserInfo
{
    public string Username { get; }
    public string Email { get; }
    public bool IsAdmin { get; } = false;
}
```

<span data-ttu-id="6d62c-157">El código generado para esta clase consideraría todos los campos públicos y las propiedades automáticas como miembros estructurales del registro.</span><span class="sxs-lookup"><span data-stu-id="6d62c-157">The generated code for this class would regard all public fields and auto-properties as structural members of the record.</span></span> <span data-ttu-id="6d62c-158">Los miembros de registro se pueden personalizar mediante un nuevo `RecordMember(bool)` atributo que podría usarse para incluir o excluir miembros.</span><span class="sxs-lookup"><span data-stu-id="6d62c-158">Record members could be customized using a new `RecordMember(bool)` attribute that could be used to either include or exclude members.</span></span> <span data-ttu-id="6d62c-159">Los miembros del registro se `initonly` de forma predeterminada y la igualdad se generaría automáticamente para la clase basada en los miembros del registro.</span><span class="sxs-lookup"><span data-stu-id="6d62c-159">Record members would be `initonly` by default and equality would be autogenerated for the class based on the record members.</span></span> <span data-ttu-id="6d62c-160">En cualquier momento, el comportamiento de estos miembros podría personalizarse simplemente declarándolos en el origen.</span><span class="sxs-lookup"><span data-stu-id="6d62c-160">At any point the behavior of these members could be customized simply by declaring them in source.</span></span> <span data-ttu-id="6d62c-161">La implementación escrita por el usuario reemplazaría la implementación predeterminada en todos los patrones de uso.</span><span class="sxs-lookup"><span data-stu-id="6d62c-161">The user-written implementation would replace the default implementation in all pattern usage.</span></span>

<span data-ttu-id="6d62c-162">Tenga en cuenta que la igualdad en el aspecto de la herencia es compleja, pero parece que se ha resuelto adecuadamente en la [otra propuesta de registros](records.md).</span><span class="sxs-lookup"><span data-stu-id="6d62c-162">Note that equality in the face of inheritance is complex, but seems to have been adequately solved in the [other records proposal](records.md).</span></span>

## <a name="primary-constructors"></a><span data-ttu-id="6d62c-163">Constructores principales</span><span class="sxs-lookup"><span data-stu-id="6d62c-163">Primary constructors</span></span>

<span data-ttu-id="6d62c-164">La propuesta de registro anterior también incluye una nueva sintaxis para una lista de parámetros en el propio tipo; por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="6d62c-164">Previous record proposal have also included a new syntax for a parameter list on the type itself, e.g.</span></span>

```C#
class Point(int X, int Y);
```

<span data-ttu-id="6d62c-165">En el nuevo diseño, la lista de parámetros sería una característica C# ortogonal, que se podía integrar perfectamente con los registros.</span><span class="sxs-lookup"><span data-stu-id="6d62c-165">In the new design, the parameter list would be an orthogonal C# feature, which could be cleanly integrated with records.</span></span> <span data-ttu-id="6d62c-166">Si un constructor principal está incluido en un registro, tendría nuevos valores predeterminados, al igual que los campos públicos y las propiedades automáticas: los parámetros del constructor principal se utilizarían para generar propiedades públicas de miembro de registro con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="6d62c-166">If a primary constructor is included in a record, it would have new defaults, just like public fields and auto-properties: the parameters in the primary constructor would be used to generate public record-member properties with the same name.</span></span> <span data-ttu-id="6d62c-167">Además, el constructor principal se podría usar ahora para generar automáticamente un Deconstructor.</span><span class="sxs-lookup"><span data-stu-id="6d62c-167">In addition, the primary constructor could now be used to auto-generate a deconstructor.</span></span>

<span data-ttu-id="6d62c-168">Por ejemplo, el Registro siguiente con un constructor principal</span><span class="sxs-lookup"><span data-stu-id="6d62c-168">For example, the following record with a primary constructor</span></span>

```C#
data class Point(int X, int Y);
```

<span data-ttu-id="6d62c-169">sería equivalente a</span><span class="sxs-lookup"><span data-stu-id="6d62c-169">would be equivalent to</span></span>

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

<span data-ttu-id="6d62c-170">y la última generación de lo anterior sería</span><span class="sxs-lookup"><span data-stu-id="6d62c-170">and the final generation of the above would be</span></span>

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

<span data-ttu-id="6d62c-171">Tenga en cuenta que se ha tomado otra parte de la información para una clase de datos con un constructor principal: en lugar de establecer los campos principales dentro del constructor protegido generado, se delega en el constructor principal.</span><span class="sxs-lookup"><span data-stu-id="6d62c-171">Note that we've taken one other piece of information into account for a data class with a primary constructor: instead of setting the primary fields inside the generated protected constructor, we delegate to the primary constructor.</span></span> <span data-ttu-id="6d62c-172">Si la clase Point tuviera otro miembro de registro no principal, por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="6d62c-172">If the Point class had another non-primary record member, e.g.</span></span>

```C#
data class Point(int X, int Y)
{
    public int Z { get; }
}
```

<span data-ttu-id="6d62c-173">a continuación, se cambiaría el constructor protegido generado de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="6d62c-173">then that would change the generated protected constructor as follows:</span></span>

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

<span data-ttu-id="6d62c-174">En particular, esto no responde qué hacer con la herencia de registros con constructores principales.</span><span class="sxs-lookup"><span data-stu-id="6d62c-174">Notably, this doesn't answer what to do about inheritance of records with primary constructors.</span></span> <span data-ttu-id="6d62c-175">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="6d62c-175">For instance,</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A;
```

<span data-ttu-id="6d62c-176">En lugar de resolver de forma arbitraria, un enfoque más explícito podría requerir que se proporcione una lista de parámetros con la lista base, por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="6d62c-176">Rather than resolving in an arbitrary manner, a more explicit approach could require that a parameter list be provided with the base list, e.g.</span></span>

```C#
data class A(int X, int Y);
data class B(int X, int Y, int Z) : A(X, Y);
```

<span data-ttu-id="6d62c-177">A continuación, la lista de parámetros de la lista base se aplicaría a una llamada `base` en el constructor principal generado:</span><span class="sxs-lookup"><span data-stu-id="6d62c-177">The parameter list in the base list would then be applied to a `base` call in the generated primary constructor:</span></span>

```C#
class B
{
    // ..
    public B(int x, int y, int z)
    : base(x, y)
    // ..
}
```

<span data-ttu-id="6d62c-178">En lo que se refiere a lo que un constructor principal podría significar fuera de un registro, todavía está abierto a una propuesta adicional.</span><span class="sxs-lookup"><span data-stu-id="6d62c-178">As for what a primary constructor could mean outside of a record, that is still open to further proposal.</span></span>
