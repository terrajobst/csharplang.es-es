---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79484373"
---
# <a name="readonly-instance-members"></a>Miembros de instancia de solo lectura

Problema experto: <https://github.com/dotnet/csharplang/issues/1710>

## <a name="summary"></a>Resumen
[summary]: #summary

Proporcionar una manera de especificar que los miembros de instancia individuales de un struct no modifiquen el estado, de la misma forma que `readonly struct` no especifica que ningún miembro de instancia modifique el estado.

Merece la pena mencionar que `readonly instance member`! = `pure instance member`. Un miembro de instancia de `pure` garantiza que no se modificará ningún Estado. Un miembro de instancia de `readonly` solo garantiza que el estado de la instancia no se modificará.

Todos los miembros de instancia de un `readonly struct` se pueden considerar implícitamente `readonly instance members`. Los `readonly instance members` explícitos declarados en Structs que no son de solo lectura se comportarían de la misma manera. Por ejemplo, también se crean copias ocultas si se llama a un miembro de instancia (en la instancia actual o en un campo de la instancia) que no es de solo lectura.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Hoy en día, los usuarios tienen la capacidad de crear `readonly struct` tipos que el compilador exige que todos los campos sean de solo lectura (y por extensión, que ningún miembro de instancia modifique el estado). Sin embargo, hay algunos escenarios en los que tiene una API existente que expone los campos accesibles o que tiene una combinación de miembros mutables y no mutantes. En estas circunstancias, no puede marcar el tipo como `readonly` (sería un cambio importante).

Normalmente, esto no tiene un gran impacto, excepto en el caso de `in` parámetros. Con `in` parámetros para Structs que no son de solo lectura, el compilador realizará una copia del parámetro para cada invocación de miembro de instancia, ya que no puede garantizar que la invocación no modifica el estado interno. Esto puede conducir a una gran cantidad de copias y peor rendimiento general que si acaba de pasar el struct directamente por valor. Para obtener un ejemplo, vea este código en [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==)

Algunos otros escenarios en los que se pueden producir copias ocultas son `static readonly fields` y `literals`. Si se admiten en el futuro, `blittable constants` terminaría en el mismo barco; es decir, todo lo que necesita actualmente es una copia completa (en la invocación de miembros de instancia) si el struct no está marcado como `readonly`.

## <a name="design"></a>Diseño
[design]: #design

Permite que un usuario especifique que un miembro de instancia es, a su vez, `readonly` y no modifica el estado de la instancia (con toda la comprobación adecuada realizada por el compilador, por supuesto). Por ejemplo:

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

ReadOnly se puede aplicar a los descriptores de acceso de propiedad para indicar que `this` no se mutarán en el descriptor de acceso. Los siguientes ejemplos tienen establecedores de solo lectura porque estos descriptores de acceso modifican el estado de campo de miembro, pero no modifican el valor de ese campo de miembro.

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

Cuando se aplica `readonly` a la sintaxis de la propiedad, significa que todos los descriptores de acceso están `readonly`.

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

ReadOnly solo se puede aplicar a los descriptores de acceso que no conmutan el tipo contenedor.

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

ReadOnly se puede aplicar a algunas propiedades implementadas automáticamente, pero no tendrá ningún efecto significativo. El compilador tratará todos los captadores implementados automáticamente como de solo lectura tanto si la palabra clave `readonly` está presente como si no.

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

ReadOnly se puede aplicar a eventos implementados manualmente, pero no a eventos similares a los campos. ReadOnly no se puede aplicar a descriptores de acceso de eventos individuales (Add/Remove).

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

Otros ejemplos de sintaxis:

* Miembros con cuerpo de expresión: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`
* Restricciones genéricas: `public static readonly void GenericMethod<T>(T value) where T : struct { }`

El compilador emitirá el miembro de instancia, como de costumbre, y también emitirá un atributo reconocido por el compilador que indica que el miembro de instancia no modifica el estado. Esto hace que el parámetro oculto `this` se convierta en `in T` en lugar de `ref T`.

Esto permitiría al usuario llamar de forma segura a ese método de instancia sin que el compilador necesitara realizar una copia.

Entre las restricciones se incluyen:

* No se puede aplicar el modificador `readonly` a métodos estáticos, constructores o destructores.
* No se puede aplicar el modificador `readonly` a los delegados.
* No se puede aplicar el modificador `readonly` a los miembros de la clase o la interfaz.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Los mismos inconvenientes que existen con los métodos de `readonly struct` hoy en día. Cierto código puede seguir provocando copias ocultas.

## <a name="notes"></a>Notas
[notes]: #notes

También es posible usar un atributo u otra palabra clave.

Esta propuesta está relacionada con (pero es más un subconjunto de) `functional purity` y/o `constant expressions`, y ambas tienen algunas propuestas existentes.
