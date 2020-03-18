---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484085"
---
# <a name="default-interface-methods"></a>métodos de interfaz predeterminados

* [x] propuesto
* [] Prototipo: [en curso](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)
* [] Implementación: ninguno
* [] Especificación: en curso, a continuación

## <a name="summary"></a>Resumen
[summary]: #summary

Agregar compatibilidad con _métodos de extensión virtual_ : métodos en interfaces con implementaciones concretas. Una clase o estructura que implementa este tipo de interfaz debe tener una única implementación _más específica_ para el método de interfaz, implementada por la clase o struct, o heredada de sus clases base o interfaces. Los métodos de extensión virtual permiten a un autor de la API agregar métodos a una interfaz en versiones futuras sin interrumpir la compatibilidad binaria o de origen con las implementaciones existentes de esa interfaz.

Son similares a [los "métodos predeterminados"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)de Java.

(Según la técnica de implementación probable) esta característica requiere la compatibilidad correspondiente en la CLI o CLR. Los programas que aprovechan esta característica no se pueden ejecutar en versiones anteriores de la plataforma.

## <a name="motivation"></a>Motivación
[motivation]: #motivation

Las motivaciones principales de esta característica son

- Los métodos de interfaz predeterminados permiten a un autor de la API agregar métodos a una interfaz en versiones futuras sin interrumpir la compatibilidad binaria o de origen con las implementaciones existentes de esa interfaz.
- La característica permite C# a interactuar con las API que tienen como destino [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) e [iOS (SWIFT)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), que admiten características similares.
- A medida que se pasa, agregar implementaciones de interfaces predeterminadas proporciona los elementos de la característica de lenguaje "rasgos" (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>). Los rasgos han demostrado ser una técnica de programación eficaz (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).

## <a name="detailed-design"></a>Diseño detallado
[design]: #detailed-design

La sintaxis de una interfaz se extiende para permitir

- declaraciones de miembros que declaran constantes, operadores, constructores estáticos y tipos anidados;
- *cuerpo* para un método o indizador, propiedad o descriptor de acceso de eventos (es decir, una implementación "predeterminada");
- declaraciones de miembros que declaran campos estáticos, métodos, propiedades, indizadores y eventos;
- declaraciones de miembros mediante la sintaxis de implementación de interfaz explícita; etc
- Modificadores de acceso explícitos (el acceso predeterminado es `public`).

Los miembros con cuerpos permiten que la interfaz proporcione una implementación "predeterminada" para el método en clases y Structs que no proporcionan una implementación de reemplazo.

Es posible que las interfaces no contengan estado de instancia. Aunque los campos estáticos ahora están permitidos, los campos de instancia no se permiten en las interfaces. Las propiedades automáticas de la instancia no se admiten en las interfaces, ya que declararían implícitamente un campo oculto.

Los métodos estáticos y privados permiten una refactorización y organización de código útiles que se usan para implementar la API pública de la interfaz.

Una invalidación de método en una interfaz debe utilizar la sintaxis de implementación de interfaz explícita.

Es un error declarar un tipo de clase, un tipo de estructura o un tipo de enumeración dentro del ámbito de un parámetro de tipo declarado con un *variance_annotation*.  Por ejemplo, la declaración de `C` siguiente es un error.

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a>Métodos concretos en interfaces

La forma más sencilla de esta característica es la capacidad de declarar un *método concreto* en una interfaz, que es un método con un cuerpo.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

Una clase que implementa esta interfaz no necesita implementar su método concreto.

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

La invalidación final de `IA.M` en la clase `C` es el método concreto `M` declarado en `IA`. Tenga en cuenta que una clase no hereda miembros de sus interfaces; Esto no se modifica en esta característica:

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

Dentro de un miembro de instancia de una interfaz, `this` tiene el tipo de la interfaz envolvente.

### <a name="modifiers-in-interfaces"></a>Modificadores en interfaces

La sintaxis de una interfaz es relajada para permitir modificadores en sus miembros. Se permite lo siguiente: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`y `partial`.

> ***Todo***: Compruebe qué otros modificadores existen.

Un miembro de interfaz cuya declaración incluye un cuerpo es un miembro de `virtual` a menos que se use el modificador `sealed` o `private`. El modificador `virtual` se puede utilizar en un miembro de función que, de lo contrario, se `virtual`rá implícitamente. Del mismo modo, aunque `abstract` es el valor predeterminado en los miembros de interfaz sin cuerpos, ese modificador se puede proporcionar explícitamente. Un miembro no virtual se puede declarar con la palabra clave `sealed`.

Es un error que un miembro de función `private` o `sealed` de una interfaz no tenga cuerpo. Un miembro de función `private` no puede tener el modificador `sealed`.

Los modificadores de acceso se pueden usar en los miembros de interfaz de todos los tipos de miembros que se permiten. El nivel de acceso `public` es el valor predeterminado, pero se puede proporcionar explícitamente.

> ***Problema abierto:*** Es necesario especificar el significado preciso de los modificadores de acceso, como `protected` y `internal`, y qué declaraciones hacen y no invalidan (en una interfaz derivada) ni se implementan (en una clase que implementa la interfaz).

Las interfaces pueden declarar `static` miembros, incluidos los tipos anidados, los métodos, los indexadores, las propiedades, los eventos y los constructores estáticos. El nivel de acceso predeterminado para todos los miembros de interfaz es `public`.

Es posible que las interfaces no declaren constructores de instancias, destructores o campos.

> ***Problema cerrado:*** ¿Deben permitirse las declaraciones de operador en una interfaz? Probablemente no se trate de operadores de conversión, pero ¿qué ocurre con otros? ***Decisión***: se permiten los operadores *excepto* los operadores de conversión, igualdad y desigualdad.

> ***Problema cerrado:*** ¿`new` debe permitirse en las declaraciones de miembros de interfaz que ocultan los miembros de las interfaces base? ***Decisión***: sí.

> ***Problema cerrado:*** Actualmente no se permite `partial` en una interfaz ni en sus miembros. Eso requeriría una propuesta independiente. ***Decisión***: sí. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a>Invalidaciones en interfaces

Las declaraciones de invalidación (es decir, las que contienen el modificador `override`) permiten al programador proporcionar una implementación más específica de un miembro virtual en una interfaz en la que el compilador o el tiempo de ejecución no encontrarían uno. También permite convertir un miembro abstracto de una superinterfaz en un miembro predeterminado en una interfaz derivada. Se permite que una declaración de invalidación invalide *explícitamente* un método de interfaz base determinado calificando la declaración con el nombre de la interfaz (en este caso no se permite el modificador de acceso). No se permiten las invalidaciones implícitas.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

Las declaraciones de invalidación en interfaces no se pueden declarar `sealed`.

Los miembros de función `virtual` públicos en una interfaz se pueden invalidar explícitamente en una interfaz derivada (calificando el nombre en la declaración de invalidación con el tipo de interfaz que declaró originalmente el método y omitiendo un modificador de acceso).

`virtual` miembros de función de una interfaz solo se pueden invalidar explícitamente (no implícitamente) en las interfaces derivadas, y los miembros que no están `public` solo se pueden implementar en una clase o struct explícitamente (no implícitamente). En cualquier caso, el miembro invalidado o implementado debe ser *accesible* donde se invalide.

### <a name="reabstraction"></a>Reabstracción

Un método virtual (concreto) declarado en una interfaz se puede invalidar para ser abstracto en una interfaz derivada

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

El modificador `abstract` no es necesario en la declaración de `IB.M` (que es el valor predeterminado en las interfaces), pero probablemente es recomendable ser explícito en una declaración de invalidación.

Esto resulta útil en interfaces derivadas en las que la implementación predeterminada de un método no es adecuada y se debe proporcionar una implementación más adecuada mediante la implementación de clases.

> ***Problema abierto:*** ¿Se debe permitir la reabstracción?

### <a name="the-most-specific-override-rule"></a>La regla de invalidación más específica

Es necesario que todas las interfaces y clases tengan una *invalidación más específica* para cada miembro virtual entre las invalidaciones que aparecen en el tipo o sus interfaces directas e indirectas. La *invalidación más específica* es una invalidación única que es más específica que cualquier otra invalidación. Si no hay ninguna invalidación, el propio miembro se considera la invalidación más específica.

Un `M1` de invalidación se considera *más específico* que otro `M2` de invalidación si `M1` se declara en el tipo `T1`, `M2` se declara en el tipo `T2`y o bien

1. `T1` contiene `T2` entre sus interfaces directas o indirectas, o
2. `T2` es un tipo de interfaz, pero `T1` no es un tipo de interfaz.

Por ejemplo:

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

La regla de invalidación más específica garantiza que el programador resuelva explícitamente un conflicto (es decir, una ambigüedad derivada de la herencia de rombo) en el punto en que se produce el conflicto.

Dado que se admiten invalidaciones abstractas explícitas en interfaces, podríamos hacerlo también en clases.

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> ***Problema de apertura***: ¿se admiten invalidaciones abstractas de interfaz explícitas en las clases?

Además, es un error si en una declaración de clase la invalidación más específica de algún método de interfaz es una invalidación abstracta que se declaró en una interfaz. Se trata de una regla existente que se ha reestado con la nueva terminología.

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

Es posible que una propiedad virtual declarada en una interfaz tenga una invalidación más específica para su descriptor de acceso `get` en una interfaz y una invalidación más específica para su descriptor de acceso `set` en una interfaz diferente. Esto se considera una infracción de la regla de *invalidación más específica* .

### <a name="static-and-private-methods"></a>Métodos `static` y `private`

Dado que las interfaces pueden contener ahora código ejecutable, resulta útil abstraer el código común en métodos privados y estáticos. Ahora se permiten en interfaces.

> ***Problema cerrado***: ¿se deben admitir métodos privados? ¿Se debe admitir métodos estáticos? **Decisión: sí**

> ***Problema de apertura***: ¿se permite que los métodos de interfaz sean `protected` o `internal` u otro acceso? Si es así, ¿cuál es la semántica? ¿Se `virtual` de forma predeterminada? Si es así, ¿hay alguna manera de hacer que sean no virtuales?

> ***Problema abierto***: si se admiten métodos estáticos, ¿se deben admitir operadores estáticos?

### <a name="base-interface-invocations"></a>Invocaciones de interfaz base

El código de un tipo que se deriva de una interfaz con un método predeterminado puede invocar explícitamente la implementación "base" de la interfaz.

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

Se permite a un método de instancia (no estático) invocar la implementación de un método de instancia accesible en una interfaz base directa de forma no virtual mediante el nombre mediante la sintaxis `base(Type).M`. Esto resulta útil cuando se resuelve una invalidación que debe proporcionarse debido a la herencia de rombo mediante la delegación a una implementación base determinada.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

Cuando se tiene acceso a un miembro de `virtual` o de `abstract` mediante la sintaxis `base(Type).M`, es necesario que `Type` contenga una *invalidación más específica* y exclusiva para `M`.

### <a name="binding-base-clauses"></a>Enlazar cláusulas base

Las interfaces ahora contienen tipos.  Estos tipos se pueden usar en la cláusula base como interfaces base.  Al enlazar una cláusula base, es posible que sea necesario conocer el conjunto de interfaces base para enlazar esos tipos (por ejemplo, para buscar en ellos y para resolver el acceso protegido).  Por lo tanto, el significado de la cláusula base de una interfaz se define circularmente.  Para interrumpir el ciclo, se agregan nuevas reglas de lenguaje correspondientes a una regla similar ya implementada para las clases.

A la hora de determinar el significado del *interface_base* de una interfaz, se supone que las interfaces base están vacías de forma temporal. Intuitivamente esto garantiza que el significado de una cláusula base no puede depender recursivamente. 

**Hemos usado las siguientes reglas:**

"Cuando una clase B se deriva de una clase a, es un error en tiempo de compilación para que una dependa de B. Una clase **depende directamente de** su clase base directa (si existe) y **depende directamente de** la ~~**clase**~~ en la que se anida inmediatamente (si existe). Dada esta definición, el conjunto completo de ~~**clases**~~ de las que depende una clase es el cierre reflexivo y transitivo de la relación **directamente depende de** . "

Se trata de un error en tiempo de compilación para una interfaz que hereda directa o indirectamente de sí misma.
Las **interfaces base** de una interfaz son las interfaces base explícitas y sus interfaces base. En otras palabras, el conjunto de interfaces base es el cierre transitivo completo de las interfaces base explícitas, sus interfaces base explícitas, etc.

**Vamos a ajustarlos de la siguiente manera:**

Cuando una clase B se deriva de una clase a, se trata de un error en tiempo de compilación para que dependa de B. Una clase **depende directamente de** su clase base directa (si existe) y **depende directamente** del _**tipo**_ en el que se anida inmediatamente (si existe).

Cuando una interfaz IB extiende una interfaz IA, se trata de un error en tiempo de compilación para que IA dependa de IB. Una interfaz **depende directamente de** sus interfaces base directas (si hay alguna) y **depende directamente** del tipo en el que se anida inmediatamente (si existe).

Dadas estas definiciones, el conjunto completo de **tipos** de los que depende un tipo es el cierre reflexivo y transitivo de la relación **directamente depende de** .

### <a name="effect-on-existing-programs"></a>Efecto en los programas existentes

Las reglas que se presentan aquí están destinadas a no tener ningún efecto en el significado de los programas existentes.

Ejemplo 1:

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

Ejemplo 2:

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

Las mismas reglas proporcionan resultados similares a la situación análoga en la que intervienen los métodos de interfaz predeterminados:

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> ***Problema cerrado***: confirme que se trata de una consecuencia prevista de la especificación. **Decisión: sí**

### <a name="runtime-method-resolution"></a>Resolución de métodos en tiempo de ejecución

> ***Problema cerrado:*** La especificación debe describir el algoritmo de resolución de métodos en tiempo de ejecución en el caso de los métodos predeterminados de interfaz. Necesitamos asegurarnos de que la semántica sea coherente con la semántica del lenguaje, por ejemplo, qué métodos declarados hacen y no invalidan ni implementan un método `internal`.

### <a name="clr-support-api"></a>API de compatibilidad con CLR

Para que los compiladores detecten Cuándo se compilan para un tiempo de ejecución que admite esta característica, las bibliotecas de estos tiempos de ejecución se modifican para anunciar ese hecho a través de la API descrita en <https://github.com/dotnet/corefx/issues/17116>. Agregaremos

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> ***Problema abierto***: ¿es el mejor nombre de la característica de *CLR* ? La característica CLR hace mucho más que eso (por ejemplo, relaja las restricciones de protección, admite invalidaciones en interfaces, etc.). Quizás se deba llamar a algo como "métodos concretos en interfaces" o "rasgos"?

### <a name="further-areas-to-be-specified"></a>Otras áreas que se van a especificar

- [] Sería útil catalogar los tipos de efectos de compatibilidad binarios y de origen que se producen al agregar métodos de interfaz predeterminados e invalidaciones a las interfaces existentes.

## <a name="drawbacks"></a>Desventajas
[drawbacks]: #drawbacks

Esta propuesta requiere una actualización coordinada de la especificación CLR (para admitir métodos concretos en interfaces y resolución de métodos). Por lo tanto, es bastante "costosa" y puede que merezca la pena hacerlo en combinación con otras características que también se prevén para requerir cambios en CLR.

## <a name="alternatives"></a>Alternativas
[alternatives]: #alternatives

Ninguno.

## <a name="unresolved-questions"></a>Preguntas no resueltas
[unresolved]: #unresolved-questions

- Las preguntas abiertas se mencionan a lo largo de la propuesta anterior.
- Vea también <https://github.com/dotnet/csharplang/issues/406> para obtener una lista de preguntas abiertas.
- La especificación detallada debe describir el mecanismo de resolución que se utiliza en tiempo de ejecución para seleccionar el método preciso que se va a invocar.
- La interacción de los metadatos generados por los nuevos compiladores y consumidos por los compiladores más antiguos debe trabajar en detalle. Por ejemplo, debemos asegurarnos de que la representación de los metadatos que usamos no provoca la adición de una implementación predeterminada en una interfaz para interrumpir una clase existente que implementa esa interfaz cuando la compila un compilador anterior. Esto puede afectar a la representación de los metadatos que se puede usar.
- El diseño debe tener en cuenta la interoperación con otros lenguajes y los compiladores existentes para otros lenguajes.

## <a name="resolved-questions"></a>Preguntas resueltas

### <a name="abstract-override"></a>Invalidación abstracta

La antigua especificación de borrador contenía la capacidad de "reabstraer" un método heredado:

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

Mis notas de 2017-03-20 mostraron que decidimos no permitirlo. Sin embargo, hay al menos dos casos de uso para ella:

1. Las API de Java, con las que algunos usuarios de esta característica esperan interoperar, dependen de esta instalación.
2. La programación con *rasgos* se beneficia de este. La reabstracción es uno de los elementos de la característica de lenguaje "rasgos" (https://en.wikipedia.org/wiki/Trait_(computer_programming)). Se permite lo siguiente con las clases:

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

Desafortunadamente, este código no se puede refactorizar como un conjunto de interfaces (rasgos) a menos que se permita. Por el *principio de Jared de expansivo*, se debe permitir.

> ***Problema cerrado:*** ¿Se debe permitir la reabstracción? ? Mis notas eran incorrectas. Las [notas LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) indican que se permite la reabstracción en una interfaz. No está en una clase.

### <a name="virtual-modifier-vs-sealed-modifier"></a>Modificador virtual frente al modificador Sealed

Desde [Aleksey Tsingauz](https://github.com/AlekseyTs):

> Decidimos permitir los modificadores indicados explícitamente en los miembros de la interfaz, a menos que haya una razón para no permitir algunos de ellos. Esto ofrece una pregunta interesante sobre el modificador virtual. ¿Debe ser necesario en los miembros con implementación predeterminada?
>
> Podríamos decir que:
>
> - Si no hay ninguna implementación y no se especifican ni virtual ni Sealed, se supone que el miembro es abstracto.
> - Si hay una implementación de y no se especifican Abstract, ni Sealed, se supone que el miembro es virtual.
> - el modificador Sealed es necesario para que un método no sea virtual ni abstracto.
>
> Como alternativa, podríamos indicar que el modificador virtual es necesario para un miembro virtual. Es decir, si hay un miembro con una implementación no marcada explícitamente con el modificador virtual, no es virtual ni abstracta. Este enfoque puede proporcionar una mejor experiencia cuando se mueve un método de una clase a una interfaz:
>
> - un método abstracto permanece abstracto.
> - un método virtual sigue siendo virtual.
> - un método sin modificador no sigue siendo virtual ni abstracto.
> - el modificador Sealed no se puede aplicar a un método que no sea una invalidación.
>
> ¿Qué te parece?

> ***Problema cerrado:*** ¿Se debe `virtual`implícitamente un método concreto (con implementación)? ?

***Decisiones:*** Realizada en el LDM 2017-04-05:

1. no virtual se debe expresar explícitamente a través de `sealed` o `private`.
2. `sealed` es la palabra clave para crear miembros de instancia de interfaz con cuerpos no virtuales
3. Queremos permitir todos los modificadores en las interfaces  
4. La accesibilidad predeterminada para los miembros de interfaz es pública, incluidos los tipos anidados
5. los miembros de función privados de las interfaces están sellados implícitamente y `sealed` no se permite en ellos.
6. Las clases privadas (en interfaces) están permitidas y se pueden sellar, y eso significa que están selladas en la clase de sellado.
7. Si falta una buena propuesta, no se permite una parcial en las interfaces o sus miembros.

### <a name="binary-compatibility-1"></a>Compatibilidad binaria 1

Cuando una biblioteca proporciona una implementación predeterminada

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

Sabemos que la implementación de `I1.M` en `C` es `I1.M`. Qué sucede si el ensamblado que contiene `I2` se cambia como se indica a continuación y se vuelve a compilar

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

pero `C` no se vuelve a compilar. ¿Qué ocurre cuando se ejecuta el programa? Una invocación de `(C as I1).M()`

1. Ejecuta `I1.M`
2. Ejecuta `I2.M`
3. Produce algún tipo de error en tiempo de ejecución

***Decisión:*** 2017-04-11: ejecuta `I2.M`, que es la invalidación más específica de forma inequívoca en tiempo de ejecución.

### <a name="event-accessors-closed"></a>Descriptores de acceso de eventos (cerrados)

> ***Problema cerrado:*** ¿Se puede invalidar un evento "a trozos"?

Considere este caso:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

Esta implementación "parcial" del evento no está permitida porque, como en una clase, la sintaxis de una declaración de evento no permite solo un descriptor de acceso; deben proporcionarse ambos (o ninguno). Puede lograr lo mismo si se permite que el descriptor de acceso Remove abstracto de la sintaxis sea abstracto implícitamente por la ausencia de un cuerpo:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

Tenga en cuenta que *se trata de una nueva sintaxis (propuesta)* . En la gramática actual, los descriptores de acceso de eventos tienen un cuerpo obligatorio.

> ***Problema cerrado:*** Puede que un descriptor de acceso de eventos sea abstracto (implícitamente) por la omisión de un cuerpo, de forma similar a la forma en que los métodos de las interfaces y los descriptores de acceso de propiedad son abstractos por la omisión de un cuerpo.

***Decisión:*** (2017-04-18) no, las declaraciones de eventos requieren ambos descriptores de acceso concretos (o ninguno).

### <a name="reabstraction-in-a-class-closed"></a>Reabstracción en una clase (cerrada)

***Problema cerrado:*** Debemos confirmar que esto se permite (de lo contrario, agregar una implementación predeterminada sería un cambio importante):

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

***Decisión:*** (2017-04-18) sí, agregar un cuerpo a una declaración de miembro de interfaz no debe romper C.

### <a name="sealed-override-closed"></a>Invalidación sellada (cerrada)

La pregunta anterior presupone implícitamente que el modificador `sealed` se puede aplicar a un `override` en una interfaz. Esto contradice la especificación del borrador. ¿Queremos permitir el sellado de una invalidación? Se deben tener en cuenta los efectos de la compatibilidad binaria y de origen del sellado.

> ***Problema cerrado:*** ¿Se debe permitir el sellado de una invalidación?

***Decisión:*** (2017-04-18) no se permite `sealed` en las invalidaciones de las interfaces. El único uso de `sealed` en los miembros de la interfaz es hacer que no sean virtuales en su declaración inicial.

### <a name="diamond-inheritance-and-classes-closed"></a>Herencia y clases de Diamond (cerradas)

El borrador de la propuesta prefiere invalidaciones de clase a invalidaciones de interfaz en escenarios de herencia de Diamond:

> Es necesario que todas las interfaces y clases tengan una *invalidación más específica* para cada método de interfaz entre las invalidaciones que aparecen en el tipo o sus interfaces directas e indirectas. La *invalidación más específica* es una invalidación única que es más específica que cualquier otra invalidación. Si no hay ninguna invalidación, el propio método se considera la invalidación más específica.
>
> Un `M1` de invalidación se considera *más específico* que otro `M2` de invalidación si `M1` se declara en el tipo `T1`, `M2` se declara en el tipo `T2`y o bien
>
> 1. `T1` contiene `T2` entre sus interfaces directas o indirectas, o
> 2. `T2` es un tipo de interfaz, pero `T1` no es un tipo de interfaz.

El escenario es este

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

Deberíamos confirmar este comportamiento (o decidir de otro modo)

> ***Problema cerrado:*** Confirme la especificación del borrador, anterior, para *la invalidación más específica* , ya que se aplica a clases e interfaces mixtas (una clase tiene prioridad sobre una interfaz). Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.

### <a name="interface-methods-vs-structs-closed"></a>Métodos de interfaz frente a estructuras (cerradas)

Hay algunas interacciones desafortunadas entre los métodos de interfaz predeterminados y los Structs.

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

Tenga en cuenta que los miembros de la interfaz no se heredan:

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

Por consiguiente, el cliente debe tener Box el struct para invocar los métodos de interfaz

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

La conversión boxing anula las ventajas principales de un tipo de `struct`. Además, los métodos de mutación no tendrán ningún efecto aparente, ya que funcionan en una *copia con conversión boxing* del struct:

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> ***Problema cerrado:*** Qué se puede hacer a continuación:
>
> 1. Prohibe que un `struct` herede una implementación predeterminada. Todos los métodos de interfaz se tratarían como abstractos en una `struct`. Después, es posible que tarden tiempo en decidir cómo hacer que funcione mejor.
> 2. Se incluye algún tipo de estrategia de generación de código que evita la conversión boxing. Dentro de un método como `IB.Increment`, el tipo de `this` quizás sea similar a un parámetro de tipo restringido a `IB`. Junto con eso, para evitar la conversión boxing en el autor de la llamada, los métodos no abstractos se heredarían de las interfaces. Esto puede aumentar considerablemente el compilador y la implementación de CLR.
> 3. No se preocupe y simplemente déjelo como wart.
> 4. ¿Otras ideas?

***Decisión:*** No se preocupe y simplemente déjelo como wart. Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.

### <a name="base-interface-invocations-closed"></a>Invocaciones de interfaz base (cerradas)

La especificación del borrador sugiere una sintaxis para las invocaciones de interfaz base inspirados por Java: `Interface.base.M()`. Es necesario seleccionar una sintaxis, al menos para el prototipo inicial. Mi favorito es `base<Interface>.M()`.

> ***Problema cerrado:*** ¿Cuál es la sintaxis de una invocación de miembro base?

***Decisión:*** La sintaxis es `base(Interface).M()`. Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>. La interfaz con el mismo nombre debe ser una interfaz base, pero no es necesario que sea una interfaz base directa.

> ***Problema abierto:*** ¿Deben permitirse las invocaciones de interfaz base en los miembros de clase?

***Decisión***: sí. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a>Invalidar miembros de la interfaz no pública (cerrado)

En una interfaz, los miembros no públicos de las interfaces base se invalidan mediante el modificador `override`. Si es una invalidación "explícita" que nombra la interfaz que contiene el miembro, se omite el modificador de acceso.

> ***Problema cerrado:*** Si es una invalidación "implícita" que no denomina la interfaz, ¿debe coincidir el modificador de acceso?

***Decisión:*** Solo los miembros públicos se pueden invalidar implícitamente y el acceso debe coincidir. Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

> ***Problema abierto:*** ¿El modificador de acceso es obligatorio, opcional u omitido en una invalidación explícita como `override void IB.M() {}`?

> ***Problema abierto:*** ¿Es `override` obligatorio, opcional u omitido en una invalidación explícita como `void IB.M() {}`?

¿Cómo implementa un miembro de interfaz no pública en una clase? ¿Es posible que se realice explícitamente?

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> ***Problema cerrado:*** ¿Cómo implementa un miembro de interfaz no pública en una clase?

***Decisión:*** Solo puede implementar explícitamente miembros de interfaz no públicos. Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

***Decisión***: no se permite ninguna palabra clave `override` en los miembros de la interfaz. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a>Compatibilidad binaria 2 (cerrada)

Considere el siguiente código en el que cada tipo está en un ensamblado independiente.

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

Sabemos que la implementación de `I1.M` en `C` es `I2.M`. Qué sucede si el ensamblado que contiene `I3` se cambia como se indica a continuación y se vuelve a compilar

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

pero `C` no se vuelve a compilar. ¿Qué ocurre cuando se ejecuta el programa? Una invocación de `(C as I1).M()`

1. Ejecuta `I1.M`
2. Ejecuta `I2.M`
3. Ejecuta `I3.M`
4. 2 o 3, de manera determinista
5. Produce algún tipo de excepción en tiempo de ejecución

***Decisión***: inicia una excepción (5). Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.

### <a name="permit-partial-in-interface-closed"></a>¿Permitir `partial` en la interfaz? subtítulos

Dado que las interfaces se pueden usar de maneras análogas a la forma en que se usan las clases abstractas, puede ser útil declararlas `partial`. Esto sería especialmente útil en el caso de los generadores.

> ***Propuesta:*** Quite la restricción de lenguaje que las interfaces y los miembros de las interfaces no se pueden declarar `partial`.

***Decisión***: sí. Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.

### <a name="main-in-an-interface-closed"></a>`Main` en una interfaz? subtítulos

> ***Problema abierto:*** ¿Es un método `static Main` en una interfaz un candidato para ser el punto de entrada del programa?

***Decisión***: sí. Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a>Confirmar intención de admitir métodos públicos no virtuales (cerrados)

¿Podemos confirmar (o revertir) nuestra decisión de permitir métodos públicos no virtuales en una interfaz?

```csharp
interface IA
{
    public sealed void M() { }
}
```

> ***Problema semicerrado:*** (2017-04-18) creemos que será útil, pero volveremos a él. Se trata de un bloque de recorrido de modelos mental.

***Decisión***: sí. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a>¿Una `override` de una interfaz introduce un nuevo miembro? subtítulos

Hay varias maneras de observar si una declaración de invalidación introduce un nuevo miembro o no.

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> ***Problema abierto:*** ¿Una declaración de invalidación en una interfaz introduce un nuevo miembro? subtítulos

En una clase, un método de invalidación es visible en algunos sentidos. Por ejemplo, los nombres de sus parámetros tienen prioridad sobre los nombres de los parámetros del método invalidado. Es posible que se duplique ese comportamiento en las interfaces, ya que siempre hay una invalidación más específica. ¿Pero queremos duplicar ese comportamiento?

Además, es posible "invalidar" un método de invalidación? [Moot]

***Decisión***: no se permite ninguna palabra clave `override` en los miembros de la interfaz. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.

### <a name="properties-with-a-private-accessor-closed"></a>Propiedades con un descriptor de acceso privado (cerrado)

Suponemos que los miembros privados no son virtuales y la combinación de virtual y privada no está permitida. Pero ¿qué ocurre con una propiedad con un descriptor de acceso privado?

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

¿Se permite? ¿Es el descriptor de acceso `set` aquí `virtual` o no? ¿Se puede invalidar Cuando sea accesible? ¿Lo siguiente implementa implícitamente solo el descriptor de acceso `get`?

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

Es, supuestamente, un error debido a que IA. P. set no es virtual y también porque no se puede acceder a él.

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

***Decisión***: el primer ejemplo es válido, mientras que el último no lo es. Esto se resuelve de forma análoga a como funciona en C#. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a>Invocaciones de interfaz base, Round 2 (cerrado)

Nuestra "solución" anterior a la administración de las invocaciones base no proporciona una expresividad suficiente. Resulta que en y en C# CLR, a diferencia de Java, debe especificar tanto la interfaz que contiene la declaración de método como la ubicación de la implementación que desea invocar.

Propongo la siguiente sintaxis para llamadas base en interfaces. No me encanta con él, pero muestra lo que cualquier sintaxis debe poder expresar:

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

Si no hay ambigüedad, puede escribirla más simplemente

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

Or

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

Or

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

***Decisión***: se decidió `base(N.I1<T>).M(s)`, lo que suponen que si tenemos un enlace de invocación podría haber un problema aquí más adelante. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a>ADVERTENCIA para que struct no implemente el método predeterminado subtítulos

@vancem afirma que debemos considerar seriamente la posibilidad de generar una advertencia si una declaración de tipo de valor no invalida algún método de interfaz, incluso si heredaría una implementación de ese método de una interfaz. Dado que hace que las llamadas a la conversión boxing y a los subminas estén limitadas.

***Decisión***: parece algo más adecuado para un analizador. También parece que esta advertencia podría ser ruidosa, ya que se activará incluso si nunca se llama al método de interfaz predeterminado y no se producirá ninguna conversión boxing. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a>Constructores estáticos de interfaz (cerrados)

¿Cuándo se ejecutan los constructores estáticos de interfaz?  El borrador actual de la CLI propone que se produce cuando se tiene acceso al primer método o campo estático. Si no hay ninguno de ellos, ¿es posible que nunca se ejecute??

[2018-10-09 el equipo de CLR propone "va a reflejar lo que hacemos para los ValueTypes (comprobación de cctor en el acceso a cada método de instancia)"]

***Decisión***: los constructores estáticos también se ejecutan en la entrada a métodos de instancia, si el constructor estático no se `beforefieldinit`, en cuyo caso los constructores estáticos se ejecutan antes del acceso al primer campo estático. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a>Design Meetings

[2017-03-08 notas](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md) de la reunión de ldm
[2017-03-21 notas](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) de la reunión de LDM
[2017-03-23 Meeting "comportamiento de CLR para los métodos de interfaz predeterminados"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM notas](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md) de la reunión
[2017-04-11 notas](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md) de [la reunión](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) [LDM
](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) [2017-04-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) [2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) [LDM Notas de la reunión
notas](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md) de la reunión de [ldm de 2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
notas de la reunión del [LDM de 2018-11-14](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)




