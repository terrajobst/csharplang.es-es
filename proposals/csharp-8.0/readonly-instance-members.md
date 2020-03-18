---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79484373"
---
# <a name="readonly-instance-members"></a><span data-ttu-id="140c9-101">Miembros de instancia de solo lectura</span><span class="sxs-lookup"><span data-stu-id="140c9-101">Readonly Instance Members</span></span>

<span data-ttu-id="140c9-102">Problema experto: <https://github.com/dotnet/csharplang/issues/1710></span><span class="sxs-lookup"><span data-stu-id="140c9-102">Championed Issue: <https://github.com/dotnet/csharplang/issues/1710></span></span>

## <a name="summary"></a><span data-ttu-id="140c9-103">Resumen</span><span class="sxs-lookup"><span data-stu-id="140c9-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="140c9-104">Proporcionar una manera de especificar que los miembros de instancia individuales de un struct no modifiquen el estado, de la misma forma que `readonly struct` no especifica que ningún miembro de instancia modifique el estado.</span><span class="sxs-lookup"><span data-stu-id="140c9-104">Provide a way to specify individual instance members on a struct do not modify state, in the same way that `readonly struct` specifies no instance members modify state.</span></span>

<span data-ttu-id="140c9-105">Merece la pena mencionar que `readonly instance member`! = `pure instance member`.</span><span class="sxs-lookup"><span data-stu-id="140c9-105">It is worth noting that `readonly instance member` != `pure instance member`.</span></span> <span data-ttu-id="140c9-106">Un miembro de instancia de `pure` garantiza que no se modificará ningún Estado.</span><span class="sxs-lookup"><span data-stu-id="140c9-106">A `pure` instance member guarantees no state will be modified.</span></span> <span data-ttu-id="140c9-107">Un miembro de instancia de `readonly` solo garantiza que el estado de la instancia no se modificará.</span><span class="sxs-lookup"><span data-stu-id="140c9-107">A `readonly` instance member only guarantees that instance state will not be modified.</span></span>

<span data-ttu-id="140c9-108">Todos los miembros de instancia de un `readonly struct` se pueden considerar implícitamente `readonly instance members`.</span><span class="sxs-lookup"><span data-stu-id="140c9-108">All instance members on a `readonly struct` could be considered implicitly `readonly instance members`.</span></span> <span data-ttu-id="140c9-109">Los `readonly instance members` explícitos declarados en Structs que no son de solo lectura se comportarían de la misma manera.</span><span class="sxs-lookup"><span data-stu-id="140c9-109">Explicit `readonly instance members` declared on non-readonly structs would behave in the same manner.</span></span> <span data-ttu-id="140c9-110">Por ejemplo, también se crean copias ocultas si se llama a un miembro de instancia (en la instancia actual o en un campo de la instancia) que no es de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="140c9-110">For example, they would still create hidden copies if you called an instance member (on the current instance or on a field of the instance) which was itself not-readonly.</span></span>

## <a name="motivation"></a><span data-ttu-id="140c9-111">Motivación</span><span class="sxs-lookup"><span data-stu-id="140c9-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="140c9-112">Hoy en día, los usuarios tienen la capacidad de crear `readonly struct` tipos que el compilador exige que todos los campos sean de solo lectura (y por extensión, que ningún miembro de instancia modifique el estado).</span><span class="sxs-lookup"><span data-stu-id="140c9-112">Today, users have the ability to create `readonly struct` types which the compiler enforces that all fields are readonly (and by extension, that no instance members modify the state).</span></span> <span data-ttu-id="140c9-113">Sin embargo, hay algunos escenarios en los que tiene una API existente que expone los campos accesibles o que tiene una combinación de miembros mutables y no mutantes.</span><span class="sxs-lookup"><span data-stu-id="140c9-113">However, there are some scenarios where you have an existing API that exposes accessible fields or that has a mix of mutating and non-mutating members.</span></span> <span data-ttu-id="140c9-114">En estas circunstancias, no puede marcar el tipo como `readonly` (sería un cambio importante).</span><span class="sxs-lookup"><span data-stu-id="140c9-114">Under these circumstances, you cannot mark the type as `readonly` (it would be a breaking change).</span></span>

<span data-ttu-id="140c9-115">Normalmente, esto no tiene un gran impacto, excepto en el caso de `in` parámetros.</span><span class="sxs-lookup"><span data-stu-id="140c9-115">This normally doesn't have much impact, except in the case of `in` parameters.</span></span> <span data-ttu-id="140c9-116">Con `in` parámetros para Structs que no son de solo lectura, el compilador realizará una copia del parámetro para cada invocación de miembro de instancia, ya que no puede garantizar que la invocación no modifica el estado interno.</span><span class="sxs-lookup"><span data-stu-id="140c9-116">With `in` parameters for non-readonly structs, the compiler will make a copy of the parameter for each instance member invocation, since it cannot guarantee that the invocation does not modify internal state.</span></span> <span data-ttu-id="140c9-117">Esto puede conducir a una gran cantidad de copias y peor rendimiento general que si acaba de pasar el struct directamente por valor.</span><span class="sxs-lookup"><span data-stu-id="140c9-117">This can lead to a multitude of copies and worse overall performance than if you had just passed the struct directly by value.</span></span> <span data-ttu-id="140c9-118">Para obtener un ejemplo, vea este código en [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span><span class="sxs-lookup"><span data-stu-id="140c9-118">For an example, see this code on [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)</span></span>

<span data-ttu-id="140c9-119">Algunos otros escenarios en los que se pueden producir copias ocultas son `static readonly fields` y `literals`.</span><span class="sxs-lookup"><span data-stu-id="140c9-119">Some other scenarios where hidden copies can occur include `static readonly fields` and `literals`.</span></span> <span data-ttu-id="140c9-120">Si se admiten en el futuro, `blittable constants` terminaría en el mismo barco; es decir, todo lo que necesita actualmente es una copia completa (en la invocación de miembros de instancia) si el struct no está marcado como `readonly`.</span><span class="sxs-lookup"><span data-stu-id="140c9-120">If they are supported in the future, `blittable constants` would end up in the same boat; that is they all currently necessitate a full copy (on instance member invocation) if the struct is not marked `readonly`.</span></span>

## <a name="design"></a><span data-ttu-id="140c9-121">Diseño</span><span class="sxs-lookup"><span data-stu-id="140c9-121">Design</span></span>
[design]: #design

<span data-ttu-id="140c9-122">Permite que un usuario especifique que un miembro de instancia es, a su vez, `readonly` y no modifica el estado de la instancia (con toda la comprobación adecuada realizada por el compilador, por supuesto).</span><span class="sxs-lookup"><span data-stu-id="140c9-122">Allow a user to specify that an instance member is, itself, `readonly` and does not modify the state of the instance (with all the appropriate verification done by the compiler, of course).</span></span> <span data-ttu-id="140c9-123">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="140c9-123">For example:</span></span>

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

<span data-ttu-id="140c9-124">ReadOnly se puede aplicar a los descriptores de acceso de propiedad para indicar que `this` no se mutarán en el descriptor de acceso.</span><span class="sxs-lookup"><span data-stu-id="140c9-124">Readonly can be applied to property accessors to indicate that `this` will not be mutated in the accessor.</span></span> <span data-ttu-id="140c9-125">Los siguientes ejemplos tienen establecedores de solo lectura porque estos descriptores de acceso modifican el estado de campo de miembro, pero no modifican el valor de ese campo de miembro.</span><span class="sxs-lookup"><span data-stu-id="140c9-125">The following examples have readonly setters because those accessors modify the state of member field, but do not modify the value of that member field.</span></span>

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

<span data-ttu-id="140c9-126">Cuando se aplica `readonly` a la sintaxis de la propiedad, significa que todos los descriptores de acceso están `readonly`.</span><span class="sxs-lookup"><span data-stu-id="140c9-126">When `readonly` is applied to the property syntax, it means that all accessors are `readonly`.</span></span>

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

<span data-ttu-id="140c9-127">ReadOnly solo se puede aplicar a los descriptores de acceso que no conmutan el tipo contenedor.</span><span class="sxs-lookup"><span data-stu-id="140c9-127">Readonly can only be applied to accessors which do not mutate the containing type.</span></span>

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

<span data-ttu-id="140c9-128">ReadOnly se puede aplicar a algunas propiedades implementadas automáticamente, pero no tendrá ningún efecto significativo.</span><span class="sxs-lookup"><span data-stu-id="140c9-128">Readonly can be applied to some auto-implemented properties, but it won't have a meaningful effect.</span></span> <span data-ttu-id="140c9-129">El compilador tratará todos los captadores implementados automáticamente como de solo lectura tanto si la palabra clave `readonly` está presente como si no.</span><span class="sxs-lookup"><span data-stu-id="140c9-129">The compiler will treat all auto-implemented getters as readonly whether or not the `readonly` keyword is present.</span></span>

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

<span data-ttu-id="140c9-130">ReadOnly se puede aplicar a eventos implementados manualmente, pero no a eventos similares a los campos.</span><span class="sxs-lookup"><span data-stu-id="140c9-130">Readonly can be applied to manually-implemented events, but not field-like events.</span></span> <span data-ttu-id="140c9-131">ReadOnly no se puede aplicar a descriptores de acceso de eventos individuales (Add/Remove).</span><span class="sxs-lookup"><span data-stu-id="140c9-131">Readonly cannot be applied to individual event accessors (add/remove).</span></span>

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

<span data-ttu-id="140c9-132">Otros ejemplos de sintaxis:</span><span class="sxs-lookup"><span data-stu-id="140c9-132">Some other syntax examples:</span></span>

* <span data-ttu-id="140c9-133">Miembros con cuerpo de expresión: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span><span class="sxs-lookup"><span data-stu-id="140c9-133">Expression bodied members: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`</span></span>
* <span data-ttu-id="140c9-134">Restricciones genéricas: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span><span class="sxs-lookup"><span data-stu-id="140c9-134">Generic constraints: `public static readonly void GenericMethod<T>(T value) where T : struct { }`</span></span>

<span data-ttu-id="140c9-135">El compilador emitirá el miembro de instancia, como de costumbre, y también emitirá un atributo reconocido por el compilador que indica que el miembro de instancia no modifica el estado.</span><span class="sxs-lookup"><span data-stu-id="140c9-135">The compiler would emit the instance member, as usual, and would additionally emit a compiler recognized attribute indicating that the instance member does not modify state.</span></span> <span data-ttu-id="140c9-136">Esto hace que el parámetro oculto `this` se convierta en `in T` en lugar de `ref T`.</span><span class="sxs-lookup"><span data-stu-id="140c9-136">This effectively causes the hidden `this` parameter to become `in T` instead of `ref T`.</span></span>

<span data-ttu-id="140c9-137">Esto permitiría al usuario llamar de forma segura a ese método de instancia sin que el compilador necesitara realizar una copia.</span><span class="sxs-lookup"><span data-stu-id="140c9-137">This would allow the user to safely call said instance method without the compiler needing to make a copy.</span></span>

<span data-ttu-id="140c9-138">Entre las restricciones se incluyen:</span><span class="sxs-lookup"><span data-stu-id="140c9-138">The restrictions would include:</span></span>

* <span data-ttu-id="140c9-139">No se puede aplicar el modificador `readonly` a métodos estáticos, constructores o destructores.</span><span class="sxs-lookup"><span data-stu-id="140c9-139">The `readonly` modifier cannot be applied to static methods, constructors or destructors.</span></span>
* <span data-ttu-id="140c9-140">No se puede aplicar el modificador `readonly` a los delegados.</span><span class="sxs-lookup"><span data-stu-id="140c9-140">The `readonly` modifier cannot be applied to delegates.</span></span>
* <span data-ttu-id="140c9-141">No se puede aplicar el modificador `readonly` a los miembros de la clase o la interfaz.</span><span class="sxs-lookup"><span data-stu-id="140c9-141">The `readonly` modifier cannot be applied to members of class or interface.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="140c9-142">Desventajas</span><span class="sxs-lookup"><span data-stu-id="140c9-142">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="140c9-143">Los mismos inconvenientes que existen con los métodos de `readonly struct` hoy en día.</span><span class="sxs-lookup"><span data-stu-id="140c9-143">Same drawbacks as exist with `readonly struct` methods today.</span></span> <span data-ttu-id="140c9-144">Cierto código puede seguir provocando copias ocultas.</span><span class="sxs-lookup"><span data-stu-id="140c9-144">Certain code may still cause hidden copies.</span></span>

## <a name="notes"></a><span data-ttu-id="140c9-145">Notas</span><span class="sxs-lookup"><span data-stu-id="140c9-145">Notes</span></span>
[notes]: #notes

<span data-ttu-id="140c9-146">También es posible usar un atributo u otra palabra clave.</span><span class="sxs-lookup"><span data-stu-id="140c9-146">Using an attribute or another keyword may also be possible.</span></span>

<span data-ttu-id="140c9-147">Esta propuesta está relacionada con (pero es más un subconjunto de) `functional purity` y/o `constant expressions`, y ambas tienen algunas propuestas existentes.</span><span class="sxs-lookup"><span data-stu-id="140c9-147">This proposal is somewhat related to (but is more a subset of) `functional purity` and/or `constant expressions`, both of which have had some existing proposals.</span></span>
