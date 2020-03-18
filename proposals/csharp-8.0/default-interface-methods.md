---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484085"
---
# <a name="default-interface-methods"></a><span data-ttu-id="05bab-101">métodos de interfaz predeterminados</span><span class="sxs-lookup"><span data-stu-id="05bab-101">default interface methods</span></span>

* <span data-ttu-id="05bab-102">[x] propuesto</span><span class="sxs-lookup"><span data-stu-id="05bab-102">[x] Proposed</span></span>
* <span data-ttu-id="05bab-103">[] Prototipo: [en curso](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span><span class="sxs-lookup"><span data-stu-id="05bab-103">[ ] Prototype: [In progress](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)</span></span>
* <span data-ttu-id="05bab-104">[] Implementación: ninguno</span><span class="sxs-lookup"><span data-stu-id="05bab-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="05bab-105">[] Especificación: en curso, a continuación</span><span class="sxs-lookup"><span data-stu-id="05bab-105">[ ] Specification: In progress, below</span></span>

## <a name="summary"></a><span data-ttu-id="05bab-106">Resumen</span><span class="sxs-lookup"><span data-stu-id="05bab-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="05bab-107">Agregar compatibilidad con _métodos de extensión virtual_ : métodos en interfaces con implementaciones concretas.</span><span class="sxs-lookup"><span data-stu-id="05bab-107">Add support for _virtual extension methods_ - methods in interfaces with concrete implementations.</span></span> <span data-ttu-id="05bab-108">Una clase o estructura que implementa este tipo de interfaz debe tener una única implementación _más específica_ para el método de interfaz, implementada por la clase o struct, o heredada de sus clases base o interfaces.</span><span class="sxs-lookup"><span data-stu-id="05bab-108">A class or struct that implements such an interface is required to have a single _most specific_ implementation for the interface method, either implemented by the class or struct, or inherited from its base classes or interfaces.</span></span> <span data-ttu-id="05bab-109">Los métodos de extensión virtual permiten a un autor de la API agregar métodos a una interfaz en versiones futuras sin interrumpir la compatibilidad binaria o de origen con las implementaciones existentes de esa interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-109">Virtual extension methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>

<span data-ttu-id="05bab-110">Son similares a [los "métodos predeterminados"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)de Java.</span><span class="sxs-lookup"><span data-stu-id="05bab-110">These are similar to Java's ["Default Methods"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html).</span></span>

<span data-ttu-id="05bab-111">(Según la técnica de implementación probable) esta característica requiere la compatibilidad correspondiente en la CLI o CLR.</span><span class="sxs-lookup"><span data-stu-id="05bab-111">(Based on the likely implementation technique) this feature requires corresponding support in the CLI/CLR.</span></span> <span data-ttu-id="05bab-112">Los programas que aprovechan esta característica no se pueden ejecutar en versiones anteriores de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="05bab-112">Programs that take advantage of this feature cannot run on earlier versions of the platform.</span></span>

## <a name="motivation"></a><span data-ttu-id="05bab-113">Motivación</span><span class="sxs-lookup"><span data-stu-id="05bab-113">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="05bab-114">Las motivaciones principales de esta característica son</span><span class="sxs-lookup"><span data-stu-id="05bab-114">The principal motivations for this feature are</span></span>

- <span data-ttu-id="05bab-115">Los métodos de interfaz predeterminados permiten a un autor de la API agregar métodos a una interfaz en versiones futuras sin interrumpir la compatibilidad binaria o de origen con las implementaciones existentes de esa interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-115">Default interface methods enable an API author to add methods to an interface in future versions without breaking source or binary compatibility with existing implementations of that interface.</span></span>
- <span data-ttu-id="05bab-116">La característica permite C# a interactuar con las API que tienen como destino [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) e [iOS (SWIFT)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), que admiten características similares.</span><span class="sxs-lookup"><span data-stu-id="05bab-116">The feature enables C# to interoperate with APIs targeting [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) and [iOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267), which support similar features.</span></span>
- <span data-ttu-id="05bab-117">A medida que se pasa, agregar implementaciones de interfaces predeterminadas proporciona los elementos de la característica de lenguaje "rasgos" (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span><span class="sxs-lookup"><span data-stu-id="05bab-117">As it turns out, adding default interface implementations provides the elements of the "traits" language feature (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>).</span></span> <span data-ttu-id="05bab-118">Los rasgos han demostrado ser una técnica de programación eficaz (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span><span class="sxs-lookup"><span data-stu-id="05bab-118">Traits have proven to be a powerful programming technique (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>).</span></span>

## <a name="detailed-design"></a><span data-ttu-id="05bab-119">Diseño detallado</span><span class="sxs-lookup"><span data-stu-id="05bab-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="05bab-120">La sintaxis de una interfaz se extiende para permitir</span><span class="sxs-lookup"><span data-stu-id="05bab-120">The syntax for an interface is extended to permit</span></span>

- <span data-ttu-id="05bab-121">declaraciones de miembros que declaran constantes, operadores, constructores estáticos y tipos anidados;</span><span class="sxs-lookup"><span data-stu-id="05bab-121">member declarations that declare constants, operators, static constructors, and nested types;</span></span>
- <span data-ttu-id="05bab-122">*cuerpo* para un método o indizador, propiedad o descriptor de acceso de eventos (es decir, una implementación "predeterminada");</span><span class="sxs-lookup"><span data-stu-id="05bab-122">a *body* for a method or indexer, property, or event accessor (that is, a "default" implementation);</span></span>
- <span data-ttu-id="05bab-123">declaraciones de miembros que declaran campos estáticos, métodos, propiedades, indizadores y eventos;</span><span class="sxs-lookup"><span data-stu-id="05bab-123">member declarations that declare static fields, methods, properties, indexers, and events;</span></span>
- <span data-ttu-id="05bab-124">declaraciones de miembros mediante la sintaxis de implementación de interfaz explícita; etc</span><span class="sxs-lookup"><span data-stu-id="05bab-124">member declarations using the explicit interface implementation syntax; and</span></span>
- <span data-ttu-id="05bab-125">Modificadores de acceso explícitos (el acceso predeterminado es `public`).</span><span class="sxs-lookup"><span data-stu-id="05bab-125">Explicit access modifiers (the default access is `public`).</span></span>

<span data-ttu-id="05bab-126">Los miembros con cuerpos permiten que la interfaz proporcione una implementación "predeterminada" para el método en clases y Structs que no proporcionan una implementación de reemplazo.</span><span class="sxs-lookup"><span data-stu-id="05bab-126">Members with bodies permit the interface to provide a "default" implementation for the method in classes and structs that do not provide an overriding implementation.</span></span>

<span data-ttu-id="05bab-127">Es posible que las interfaces no contengan estado de instancia.</span><span class="sxs-lookup"><span data-stu-id="05bab-127">Interfaces may not contain instance state.</span></span> <span data-ttu-id="05bab-128">Aunque los campos estáticos ahora están permitidos, los campos de instancia no se permiten en las interfaces.</span><span class="sxs-lookup"><span data-stu-id="05bab-128">While static fields are now permitted, instance fields are not permitted in interfaces.</span></span> <span data-ttu-id="05bab-129">Las propiedades automáticas de la instancia no se admiten en las interfaces, ya que declararían implícitamente un campo oculto.</span><span class="sxs-lookup"><span data-stu-id="05bab-129">Instance auto-properties are not supported in interfaces, as they would implicitly declare a hidden field.</span></span>

<span data-ttu-id="05bab-130">Los métodos estáticos y privados permiten una refactorización y organización de código útiles que se usan para implementar la API pública de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-130">Static and private methods permit useful refactoring and organization of code used to implement the interface's public API.</span></span>

<span data-ttu-id="05bab-131">Una invalidación de método en una interfaz debe utilizar la sintaxis de implementación de interfaz explícita.</span><span class="sxs-lookup"><span data-stu-id="05bab-131">A method override in an interface must use the explicit interface implementation syntax.</span></span>

<span data-ttu-id="05bab-132">Es un error declarar un tipo de clase, un tipo de estructura o un tipo de enumeración dentro del ámbito de un parámetro de tipo declarado con un *variance_annotation*.</span><span class="sxs-lookup"><span data-stu-id="05bab-132">It is an error to declare a class type, struct type, or enum type within the scope of a type parameter that was declared with a *variance_annotation*.</span></span>  <span data-ttu-id="05bab-133">Por ejemplo, la declaración de `C` siguiente es un error.</span><span class="sxs-lookup"><span data-stu-id="05bab-133">For example, the declaration of `C` below is an error.</span></span>

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a><span data-ttu-id="05bab-134">Métodos concretos en interfaces</span><span class="sxs-lookup"><span data-stu-id="05bab-134">Concrete methods in interfaces</span></span>

<span data-ttu-id="05bab-135">La forma más sencilla de esta característica es la capacidad de declarar un *método concreto* en una interfaz, que es un método con un cuerpo.</span><span class="sxs-lookup"><span data-stu-id="05bab-135">The simplest form of this feature is the ability to declare a *concrete method* in an interface, which is a method with a body.</span></span>

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

<span data-ttu-id="05bab-136">Una clase que implementa esta interfaz no necesita implementar su método concreto.</span><span class="sxs-lookup"><span data-stu-id="05bab-136">A class that implements this interface need not implement its concrete method.</span></span>

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

<span data-ttu-id="05bab-137">La invalidación final de `IA.M` en la clase `C` es el método concreto `M` declarado en `IA`.</span><span class="sxs-lookup"><span data-stu-id="05bab-137">The final override for `IA.M` in class `C` is the concrete method `M` declared in `IA`.</span></span> <span data-ttu-id="05bab-138">Tenga en cuenta que una clase no hereda miembros de sus interfaces; Esto no se modifica en esta característica:</span><span class="sxs-lookup"><span data-stu-id="05bab-138">Note that a class does not inherit members from its interfaces; that is not changed by this feature:</span></span>

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

<span data-ttu-id="05bab-139">Dentro de un miembro de instancia de una interfaz, `this` tiene el tipo de la interfaz envolvente.</span><span class="sxs-lookup"><span data-stu-id="05bab-139">Within an instance member of an interface, `this` has the type of the enclosing interface.</span></span>

### <a name="modifiers-in-interfaces"></a><span data-ttu-id="05bab-140">Modificadores en interfaces</span><span class="sxs-lookup"><span data-stu-id="05bab-140">Modifiers in interfaces</span></span>

<span data-ttu-id="05bab-141">La sintaxis de una interfaz es relajada para permitir modificadores en sus miembros.</span><span class="sxs-lookup"><span data-stu-id="05bab-141">The syntax for an interface is relaxed to permit modifiers on its members.</span></span> <span data-ttu-id="05bab-142">Se permite lo siguiente: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`y `partial`.</span><span class="sxs-lookup"><span data-stu-id="05bab-142">The following are permitted: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`, and `partial`.</span></span>

> <span data-ttu-id="05bab-143">***Todo***: Compruebe qué otros modificadores existen.</span><span class="sxs-lookup"><span data-stu-id="05bab-143">***TODO***: check what other modifiers exist.</span></span>

<span data-ttu-id="05bab-144">Un miembro de interfaz cuya declaración incluye un cuerpo es un miembro de `virtual` a menos que se use el modificador `sealed` o `private`.</span><span class="sxs-lookup"><span data-stu-id="05bab-144">An interface member whose declaration includes a body is a `virtual` member unless the `sealed` or `private` modifier is used.</span></span> <span data-ttu-id="05bab-145">El modificador `virtual` se puede utilizar en un miembro de función que, de lo contrario, se `virtual`rá implícitamente.</span><span class="sxs-lookup"><span data-stu-id="05bab-145">The `virtual` modifier may be used on a function member that would otherwise be implicitly `virtual`.</span></span> <span data-ttu-id="05bab-146">Del mismo modo, aunque `abstract` es el valor predeterminado en los miembros de interfaz sin cuerpos, ese modificador se puede proporcionar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="05bab-146">Similarly, although `abstract` is the default on interface members without bodies, that modifier may be given explicitly.</span></span> <span data-ttu-id="05bab-147">Un miembro no virtual se puede declarar con la palabra clave `sealed`.</span><span class="sxs-lookup"><span data-stu-id="05bab-147">A non-virtual member may be declared using the `sealed` keyword.</span></span>

<span data-ttu-id="05bab-148">Es un error que un miembro de función `private` o `sealed` de una interfaz no tenga cuerpo.</span><span class="sxs-lookup"><span data-stu-id="05bab-148">It is an error for a `private` or `sealed` function member of an interface to have no body.</span></span> <span data-ttu-id="05bab-149">Un miembro de función `private` no puede tener el modificador `sealed`.</span><span class="sxs-lookup"><span data-stu-id="05bab-149">A `private` function member may not have the modifier `sealed`.</span></span>

<span data-ttu-id="05bab-150">Los modificadores de acceso se pueden usar en los miembros de interfaz de todos los tipos de miembros que se permiten.</span><span class="sxs-lookup"><span data-stu-id="05bab-150">Access modifiers may be used on interface members of all kinds of members that are permitted.</span></span> <span data-ttu-id="05bab-151">El nivel de acceso `public` es el valor predeterminado, pero se puede proporcionar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="05bab-151">The access level `public` is the default but it may be given explicitly.</span></span>

> <span data-ttu-id="05bab-152">***Problema abierto:*** Es necesario especificar el significado preciso de los modificadores de acceso, como `protected` y `internal`, y qué declaraciones hacen y no invalidan (en una interfaz derivada) ni se implementan (en una clase que implementa la interfaz).</span><span class="sxs-lookup"><span data-stu-id="05bab-152">***Open Issue:*** We need to specify the precise meaning of the access modifiers such as `protected` and `internal`, and which declarations do and do not override them (in a derived interface) or implement them (in a class that implements the interface).</span></span>

<span data-ttu-id="05bab-153">Las interfaces pueden declarar `static` miembros, incluidos los tipos anidados, los métodos, los indexadores, las propiedades, los eventos y los constructores estáticos.</span><span class="sxs-lookup"><span data-stu-id="05bab-153">Interfaces may declare `static` members, including nested types, methods, indexers, properties, events, and static constructors.</span></span> <span data-ttu-id="05bab-154">El nivel de acceso predeterminado para todos los miembros de interfaz es `public`.</span><span class="sxs-lookup"><span data-stu-id="05bab-154">The default access level for all interface members is `public`.</span></span>

<span data-ttu-id="05bab-155">Es posible que las interfaces no declaren constructores de instancias, destructores o campos.</span><span class="sxs-lookup"><span data-stu-id="05bab-155">Interfaces may not declare instance constructors, destructors, or fields.</span></span>

> <span data-ttu-id="05bab-156">***Problema cerrado:*** ¿Deben permitirse las declaraciones de operador en una interfaz?</span><span class="sxs-lookup"><span data-stu-id="05bab-156">***Closed Issue:*** Should operator declarations be permitted in an interface?</span></span> <span data-ttu-id="05bab-157">Probablemente no se trate de operadores de conversión, pero ¿qué ocurre con otros?</span><span class="sxs-lookup"><span data-stu-id="05bab-157">Probably not conversion operators, but what about others?</span></span> <span data-ttu-id="05bab-158">***Decisión***: se permiten los operadores *excepto* los operadores de conversión, igualdad y desigualdad.</span><span class="sxs-lookup"><span data-stu-id="05bab-158">***Decision***: Operators are permitted *except* for conversion, equality, and inequality operators.</span></span>

> <span data-ttu-id="05bab-159">***Problema cerrado:*** ¿`new` debe permitirse en las declaraciones de miembros de interfaz que ocultan los miembros de las interfaces base?</span><span class="sxs-lookup"><span data-stu-id="05bab-159">***Closed Issue:*** Should `new` be permitted on interface member declarations that hide members from base interfaces?</span></span> <span data-ttu-id="05bab-160">***Decisión***: sí.</span><span class="sxs-lookup"><span data-stu-id="05bab-160">***Decision***: Yes.</span></span>

> <span data-ttu-id="05bab-161">***Problema cerrado:*** Actualmente no se permite `partial` en una interfaz ni en sus miembros.</span><span class="sxs-lookup"><span data-stu-id="05bab-161">***Closed Issue:*** We do not currently permit `partial` on an interface or its members.</span></span> <span data-ttu-id="05bab-162">Eso requeriría una propuesta independiente.</span><span class="sxs-lookup"><span data-stu-id="05bab-162">That would require a separate proposal.</span></span> <span data-ttu-id="05bab-163">***Decisión***: sí.</span><span class="sxs-lookup"><span data-stu-id="05bab-163">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a><span data-ttu-id="05bab-164">Invalidaciones en interfaces</span><span class="sxs-lookup"><span data-stu-id="05bab-164">Overrides in interfaces</span></span>

<span data-ttu-id="05bab-165">Las declaraciones de invalidación (es decir, las que contienen el modificador `override`) permiten al programador proporcionar una implementación más específica de un miembro virtual en una interfaz en la que el compilador o el tiempo de ejecución no encontrarían uno.</span><span class="sxs-lookup"><span data-stu-id="05bab-165">Override declarations (i.e. those containing the `override` modifier) allow the programmer to provide a most specific implementation of a virtual member in an interface where the compiler or runtime would not otherwise find one.</span></span> <span data-ttu-id="05bab-166">También permite convertir un miembro abstracto de una superinterfaz en un miembro predeterminado en una interfaz derivada.</span><span class="sxs-lookup"><span data-stu-id="05bab-166">It also allows turning an abstract member from a super-interface into a default member in a derived interface.</span></span> <span data-ttu-id="05bab-167">Se permite que una declaración de invalidación invalide *explícitamente* un método de interfaz base determinado calificando la declaración con el nombre de la interfaz (en este caso no se permite el modificador de acceso).</span><span class="sxs-lookup"><span data-stu-id="05bab-167">An override declaration is permitted to *explicitly* override a particular base interface method by qualifying the declaration with the interface name (no access modifier is permitted in this case).</span></span> <span data-ttu-id="05bab-168">No se permiten las invalidaciones implícitas.</span><span class="sxs-lookup"><span data-stu-id="05bab-168">Implicit overrides are not permitted.</span></span>

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

<span data-ttu-id="05bab-169">Las declaraciones de invalidación en interfaces no se pueden declarar `sealed`.</span><span class="sxs-lookup"><span data-stu-id="05bab-169">Override declarations in interfaces may not be declared `sealed`.</span></span>

<span data-ttu-id="05bab-170">Los miembros de función `virtual` públicos en una interfaz se pueden invalidar explícitamente en una interfaz derivada (calificando el nombre en la declaración de invalidación con el tipo de interfaz que declaró originalmente el método y omitiendo un modificador de acceso).</span><span class="sxs-lookup"><span data-stu-id="05bab-170">Public `virtual` function members in an interface may be overridden in a derived interface explicitly (by qualifying the name in the override declaration with the interface type that originally declared the method, and omitting an access modifier).</span></span>

<span data-ttu-id="05bab-171">`virtual` miembros de función de una interfaz solo se pueden invalidar explícitamente (no implícitamente) en las interfaces derivadas, y los miembros que no están `public` solo se pueden implementar en una clase o struct explícitamente (no implícitamente).</span><span class="sxs-lookup"><span data-stu-id="05bab-171">`virtual` function members in an interface may only be overridden explicitly (not implicitly) in derived interfaces, and members that are not `public` may only be implemented in a class or struct explicitly (not implicitly).</span></span> <span data-ttu-id="05bab-172">En cualquier caso, el miembro invalidado o implementado debe ser *accesible* donde se invalide.</span><span class="sxs-lookup"><span data-stu-id="05bab-172">In either case, the overridden or implemented member must be *accessible* where it is overridden.</span></span>

### <a name="reabstraction"></a><span data-ttu-id="05bab-173">Reabstracción</span><span class="sxs-lookup"><span data-stu-id="05bab-173">Reabstraction</span></span>

<span data-ttu-id="05bab-174">Un método virtual (concreto) declarado en una interfaz se puede invalidar para ser abstracto en una interfaz derivada</span><span class="sxs-lookup"><span data-stu-id="05bab-174">A virtual (concrete) method declared in an interface may be overridden to be abstract in a derived interface</span></span>

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

<span data-ttu-id="05bab-175">El modificador `abstract` no es necesario en la declaración de `IB.M` (que es el valor predeterminado en las interfaces), pero probablemente es recomendable ser explícito en una declaración de invalidación.</span><span class="sxs-lookup"><span data-stu-id="05bab-175">The `abstract` modifier is not required in the declaration of `IB.M` (that is the default in interfaces), but it is probably good practice to be explicit in an override declaration.</span></span>

<span data-ttu-id="05bab-176">Esto resulta útil en interfaces derivadas en las que la implementación predeterminada de un método no es adecuada y se debe proporcionar una implementación más adecuada mediante la implementación de clases.</span><span class="sxs-lookup"><span data-stu-id="05bab-176">This is useful in derived interfaces where the default implementation of a method is inappropriate and a more appropriate implementation should be provided by implementing classes.</span></span>

> <span data-ttu-id="05bab-177">***Problema abierto:*** ¿Se debe permitir la reabstracción?</span><span class="sxs-lookup"><span data-stu-id="05bab-177">***Open Issue:*** Should reabstraction be permitted?</span></span>

### <a name="the-most-specific-override-rule"></a><span data-ttu-id="05bab-178">La regla de invalidación más específica</span><span class="sxs-lookup"><span data-stu-id="05bab-178">The most specific override rule</span></span>

<span data-ttu-id="05bab-179">Es necesario que todas las interfaces y clases tengan una *invalidación más específica* para cada miembro virtual entre las invalidaciones que aparecen en el tipo o sus interfaces directas e indirectas.</span><span class="sxs-lookup"><span data-stu-id="05bab-179">We require that every interface and class have a *most specific override* for every virtual member among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="05bab-180">La *invalidación más específica* es una invalidación única que es más específica que cualquier otra invalidación.</span><span class="sxs-lookup"><span data-stu-id="05bab-180">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="05bab-181">Si no hay ninguna invalidación, el propio miembro se considera la invalidación más específica.</span><span class="sxs-lookup"><span data-stu-id="05bab-181">If there is no override, the member itself is considered the most specific override.</span></span>

<span data-ttu-id="05bab-182">Un `M1` de invalidación se considera *más específico* que otro `M2` de invalidación si `M1` se declara en el tipo `T1`, `M2` se declara en el tipo `T2`y o bien</span><span class="sxs-lookup"><span data-stu-id="05bab-182">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>

1. <span data-ttu-id="05bab-183">`T1` contiene `T2` entre sus interfaces directas o indirectas, o</span><span class="sxs-lookup"><span data-stu-id="05bab-183">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
2. <span data-ttu-id="05bab-184">`T2` es un tipo de interfaz, pero `T1` no es un tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-184">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="05bab-185">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="05bab-185">For example:</span></span>

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

<span data-ttu-id="05bab-186">La regla de invalidación más específica garantiza que el programador resuelva explícitamente un conflicto (es decir, una ambigüedad derivada de la herencia de rombo) en el punto en que se produce el conflicto.</span><span class="sxs-lookup"><span data-stu-id="05bab-186">The most specific override rule ensures that a conflict (i.e. an ambiguity arising from diamond inheritance) is resolved explicitly by the programmer at the point where the conflict arises.</span></span>

<span data-ttu-id="05bab-187">Dado que se admiten invalidaciones abstractas explícitas en interfaces, podríamos hacerlo también en clases.</span><span class="sxs-lookup"><span data-stu-id="05bab-187">Because we support explicit abstract overrides in interfaces, we could do so in classes as well</span></span>

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> <span data-ttu-id="05bab-188">***Problema de apertura***: ¿se admiten invalidaciones abstractas de interfaz explícitas en las clases?</span><span class="sxs-lookup"><span data-stu-id="05bab-188">***Open issue***: should we support explicit interface abstract overrides in classes?</span></span>

<span data-ttu-id="05bab-189">Además, es un error si en una declaración de clase la invalidación más específica de algún método de interfaz es una invalidación abstracta que se declaró en una interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-189">In addition, it is an error if in a class declaration the most specific override of some interface method is an abstract override that was declared in an interface.</span></span> <span data-ttu-id="05bab-190">Se trata de una regla existente que se ha reestado con la nueva terminología.</span><span class="sxs-lookup"><span data-stu-id="05bab-190">This is an existing rule restated using the new terminology.</span></span>

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

<span data-ttu-id="05bab-191">Es posible que una propiedad virtual declarada en una interfaz tenga una invalidación más específica para su descriptor de acceso `get` en una interfaz y una invalidación más específica para su descriptor de acceso `set` en una interfaz diferente.</span><span class="sxs-lookup"><span data-stu-id="05bab-191">It is possible for a virtual property declared in an interface to have a most specific override for its `get` accessor in one interface and a most specific override for its `set` accessor in a different interface.</span></span> <span data-ttu-id="05bab-192">Esto se considera una infracción de la regla de *invalidación más específica* .</span><span class="sxs-lookup"><span data-stu-id="05bab-192">This is considered a violation of the *most specific override* rule.</span></span>

### <a name="static-and-private-methods"></a><span data-ttu-id="05bab-193">Métodos `static` y `private`</span><span class="sxs-lookup"><span data-stu-id="05bab-193">`static` and `private` methods</span></span>

<span data-ttu-id="05bab-194">Dado que las interfaces pueden contener ahora código ejecutable, resulta útil abstraer el código común en métodos privados y estáticos.</span><span class="sxs-lookup"><span data-stu-id="05bab-194">Because interfaces may now contain executable code, it is useful to abstract common code into private and static methods.</span></span> <span data-ttu-id="05bab-195">Ahora se permiten en interfaces.</span><span class="sxs-lookup"><span data-stu-id="05bab-195">We now permit these in interfaces.</span></span>

> <span data-ttu-id="05bab-196">***Problema cerrado***: ¿se deben admitir métodos privados?</span><span class="sxs-lookup"><span data-stu-id="05bab-196">***Closed issue***: Should we support private methods?</span></span> <span data-ttu-id="05bab-197">¿Se debe admitir métodos estáticos?</span><span class="sxs-lookup"><span data-stu-id="05bab-197">Should we support static methods?</span></span> <span data-ttu-id="05bab-198">**Decisión: sí**</span><span class="sxs-lookup"><span data-stu-id="05bab-198">**Decision: YES**</span></span>

> <span data-ttu-id="05bab-199">***Problema de apertura***: ¿se permite que los métodos de interfaz sean `protected` o `internal` u otro acceso?</span><span class="sxs-lookup"><span data-stu-id="05bab-199">***Open issue***: should we permit interface methods to be `protected` or `internal` or other access?</span></span> <span data-ttu-id="05bab-200">Si es así, ¿cuál es la semántica?</span><span class="sxs-lookup"><span data-stu-id="05bab-200">If so, what are the semantics?</span></span> <span data-ttu-id="05bab-201">¿Se `virtual` de forma predeterminada?</span><span class="sxs-lookup"><span data-stu-id="05bab-201">Are they `virtual` by default?</span></span> <span data-ttu-id="05bab-202">Si es así, ¿hay alguna manera de hacer que sean no virtuales?</span><span class="sxs-lookup"><span data-stu-id="05bab-202">If so, is there a way to make them non-virtual?</span></span>

> <span data-ttu-id="05bab-203">***Problema abierto***: si se admiten métodos estáticos, ¿se deben admitir operadores estáticos?</span><span class="sxs-lookup"><span data-stu-id="05bab-203">***Open issue***: If we support static methods, should we support (static) operators?</span></span>

### <a name="base-interface-invocations"></a><span data-ttu-id="05bab-204">Invocaciones de interfaz base</span><span class="sxs-lookup"><span data-stu-id="05bab-204">Base interface invocations</span></span>

<span data-ttu-id="05bab-205">El código de un tipo que se deriva de una interfaz con un método predeterminado puede invocar explícitamente la implementación "base" de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-205">Code in a type that derives from an interface with a default method can explicitly invoke that interface's "base" implementation.</span></span>

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

<span data-ttu-id="05bab-206">Se permite a un método de instancia (no estático) invocar la implementación de un método de instancia accesible en una interfaz base directa de forma no virtual mediante el nombre mediante la sintaxis `base(Type).M`.</span><span class="sxs-lookup"><span data-stu-id="05bab-206">An instance (nonstatic) method is permitted to invoke the implementation of an accessible instance method in a direct base interface nonvirtually by naming it using the syntax `base(Type).M`.</span></span> <span data-ttu-id="05bab-207">Esto resulta útil cuando se resuelve una invalidación que debe proporcionarse debido a la herencia de rombo mediante la delegación a una implementación base determinada.</span><span class="sxs-lookup"><span data-stu-id="05bab-207">This is useful when an override that is required to be provided due to diamond inheritance is resolved by delegating to one particular base implementation.</span></span>

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

<span data-ttu-id="05bab-208">Cuando se tiene acceso a un miembro de `virtual` o de `abstract` mediante la sintaxis `base(Type).M`, es necesario que `Type` contenga una *invalidación más específica* y exclusiva para `M`.</span><span class="sxs-lookup"><span data-stu-id="05bab-208">When a `virtual` or `abstract` member is accessed using the syntax `base(Type).M`, it is required that `Type` contains a unique *most specific override* for `M`.</span></span>

### <a name="binding-base-clauses"></a><span data-ttu-id="05bab-209">Enlazar cláusulas base</span><span class="sxs-lookup"><span data-stu-id="05bab-209">Binding base clauses</span></span>

<span data-ttu-id="05bab-210">Las interfaces ahora contienen tipos.</span><span class="sxs-lookup"><span data-stu-id="05bab-210">Interfaces now contain types.</span></span>  <span data-ttu-id="05bab-211">Estos tipos se pueden usar en la cláusula base como interfaces base.</span><span class="sxs-lookup"><span data-stu-id="05bab-211">These types may be used in the base clause as base interfaces.</span></span>  <span data-ttu-id="05bab-212">Al enlazar una cláusula base, es posible que sea necesario conocer el conjunto de interfaces base para enlazar esos tipos (por ejemplo, para buscar en ellos y para resolver el acceso protegido).</span><span class="sxs-lookup"><span data-stu-id="05bab-212">When binding a base clause, we may need to know the set of base interfaces to bind those types (e.g. to lookup in them and to resolve protected access).</span></span>  <span data-ttu-id="05bab-213">Por lo tanto, el significado de la cláusula base de una interfaz se define circularmente.</span><span class="sxs-lookup"><span data-stu-id="05bab-213">The meaning of an interface's base clause is thus circularly defined.</span></span>  <span data-ttu-id="05bab-214">Para interrumpir el ciclo, se agregan nuevas reglas de lenguaje correspondientes a una regla similar ya implementada para las clases.</span><span class="sxs-lookup"><span data-stu-id="05bab-214">To break the cycle, we add a new language rules corresponding to a similar rule already in place for classes.</span></span>

<span data-ttu-id="05bab-215">A la hora de determinar el significado del *interface_base* de una interfaz, se supone que las interfaces base están vacías de forma temporal.</span><span class="sxs-lookup"><span data-stu-id="05bab-215">While determining the meaning of the *interface_base* of an interface, the base interfaces are temporarily assumed to be empty.</span></span> <span data-ttu-id="05bab-216">Intuitivamente esto garantiza que el significado de una cláusula base no puede depender recursivamente.</span><span class="sxs-lookup"><span data-stu-id="05bab-216">Intuitively this ensures that the meaning of a base clause cannot recursively depend on itself.</span></span> 

<span data-ttu-id="05bab-217">**Hemos usado las siguientes reglas:**</span><span class="sxs-lookup"><span data-stu-id="05bab-217">**We used to have the following rules:**</span></span>

<span data-ttu-id="05bab-218">"Cuando una clase B se deriva de una clase a, es un error en tiempo de compilación para que una dependa de B. Una clase **depende directamente de** su clase base directa (si existe) y **depende directamente de** la ~~**clase**~~ en la que se anida inmediatamente (si existe).</span><span class="sxs-lookup"><span data-stu-id="05bab-218">"When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the ~~**class**~~ within which it is immediately nested (if any).</span></span> <span data-ttu-id="05bab-219">Dada esta definición, el conjunto completo de ~~**clases**~~ de las que depende una clase es el cierre reflexivo y transitivo de la relación **directamente depende de** . "</span><span class="sxs-lookup"><span data-stu-id="05bab-219">Given this definition, the complete set of ~~**classes**~~ upon which a class depends is the reflexive and transitive closure of the **directly depends on** relationship."</span></span>

<span data-ttu-id="05bab-220">Se trata de un error en tiempo de compilación para una interfaz que hereda directa o indirectamente de sí misma.</span><span class="sxs-lookup"><span data-stu-id="05bab-220">It is a compile-time error for an interface to directly or indirectly inherit from itself.</span></span>
<span data-ttu-id="05bab-221">Las **interfaces base** de una interfaz son las interfaces base explícitas y sus interfaces base.</span><span class="sxs-lookup"><span data-stu-id="05bab-221">The **base interfaces** of an interface are the explicit base interfaces and their base interfaces.</span></span> <span data-ttu-id="05bab-222">En otras palabras, el conjunto de interfaces base es el cierre transitivo completo de las interfaces base explícitas, sus interfaces base explícitas, etc.</span><span class="sxs-lookup"><span data-stu-id="05bab-222">In other words, the set of base interfaces is the complete transitive closure of the explicit base interfaces, their explicit base interfaces, and so on.</span></span>

<span data-ttu-id="05bab-223">**Vamos a ajustarlos de la siguiente manera:**</span><span class="sxs-lookup"><span data-stu-id="05bab-223">**We are adjusting them as follows:**</span></span>

<span data-ttu-id="05bab-224">Cuando una clase B se deriva de una clase a, se trata de un error en tiempo de compilación para que dependa de B. Una clase **depende directamente de** su clase base directa (si existe) y **depende directamente** del _**tipo**_ en el que se anida inmediatamente (si existe).</span><span class="sxs-lookup"><span data-stu-id="05bab-224">When a class B derives from a class A, it is a compile-time error for A to depend on B. A class **directly depends on** its direct base class (if any) and **directly depends on** the _**type**_ within which it is immediately nested (if any).</span></span>

<span data-ttu-id="05bab-225">Cuando una interfaz IB extiende una interfaz IA, se trata de un error en tiempo de compilación para que IA dependa de IB.</span><span class="sxs-lookup"><span data-stu-id="05bab-225">When an interface IB extends an interface IA, it is a compile-time error for IA to depend on IB.</span></span> <span data-ttu-id="05bab-226">Una interfaz **depende directamente de** sus interfaces base directas (si hay alguna) y **depende directamente** del tipo en el que se anida inmediatamente (si existe).</span><span class="sxs-lookup"><span data-stu-id="05bab-226">An interface **directly depends on** its direct base interfaces (if any) and **directly depends on** the type within which it is immediately nested (if any).</span></span>

<span data-ttu-id="05bab-227">Dadas estas definiciones, el conjunto completo de **tipos** de los que depende un tipo es el cierre reflexivo y transitivo de la relación **directamente depende de** .</span><span class="sxs-lookup"><span data-stu-id="05bab-227">Given these definitions, the complete set of **types** upon which a type depends is the reflexive and transitive closure of the **directly depends on** relationship.</span></span>

### <a name="effect-on-existing-programs"></a><span data-ttu-id="05bab-228">Efecto en los programas existentes</span><span class="sxs-lookup"><span data-stu-id="05bab-228">Effect on existing programs</span></span>

<span data-ttu-id="05bab-229">Las reglas que se presentan aquí están destinadas a no tener ningún efecto en el significado de los programas existentes.</span><span class="sxs-lookup"><span data-stu-id="05bab-229">The rules presented here are intended to have no effect on the meaning of existing programs.</span></span>

<span data-ttu-id="05bab-230">Ejemplo 1:</span><span class="sxs-lookup"><span data-stu-id="05bab-230">Example 1:</span></span>

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

<span data-ttu-id="05bab-231">Ejemplo 2:</span><span class="sxs-lookup"><span data-stu-id="05bab-231">Example 2:</span></span>

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

<span data-ttu-id="05bab-232">Las mismas reglas proporcionan resultados similares a la situación análoga en la que intervienen los métodos de interfaz predeterminados:</span><span class="sxs-lookup"><span data-stu-id="05bab-232">The same rules give similar results to the analogous situation involving default interface methods:</span></span>

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

> <span data-ttu-id="05bab-233">***Problema cerrado***: confirme que se trata de una consecuencia prevista de la especificación.</span><span class="sxs-lookup"><span data-stu-id="05bab-233">***Closed issue***: confirm that this is an intended consequence of the specification.</span></span> <span data-ttu-id="05bab-234">**Decisión: sí**</span><span class="sxs-lookup"><span data-stu-id="05bab-234">**Decision: YES**</span></span>

### <a name="runtime-method-resolution"></a><span data-ttu-id="05bab-235">Resolución de métodos en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="05bab-235">Runtime method resolution</span></span>

> <span data-ttu-id="05bab-236">***Problema cerrado:*** La especificación debe describir el algoritmo de resolución de métodos en tiempo de ejecución en el caso de los métodos predeterminados de interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-236">***Closed Issue:*** The spec should describe the runtime method resolution algorithm in the face of interface default methods.</span></span> <span data-ttu-id="05bab-237">Necesitamos asegurarnos de que la semántica sea coherente con la semántica del lenguaje, por ejemplo, qué métodos declarados hacen y no invalidan ni implementan un método `internal`.</span><span class="sxs-lookup"><span data-stu-id="05bab-237">We need to ensure that the semantics are consistent with the language semantics, e.g. which declared methods do and do not override or implement an `internal` method.</span></span>

### <a name="clr-support-api"></a><span data-ttu-id="05bab-238">API de compatibilidad con CLR</span><span class="sxs-lookup"><span data-stu-id="05bab-238">CLR support API</span></span>

<span data-ttu-id="05bab-239">Para que los compiladores detecten Cuándo se compilan para un tiempo de ejecución que admite esta característica, las bibliotecas de estos tiempos de ejecución se modifican para anunciar ese hecho a través de la API descrita en <https://github.com/dotnet/corefx/issues/17116>.</span><span class="sxs-lookup"><span data-stu-id="05bab-239">In order for compilers to detect when they are compiling for a runtime that supports this feature, libraries for such runtimes are modified to advertise that fact through the API discussed in <https://github.com/dotnet/corefx/issues/17116>.</span></span> <span data-ttu-id="05bab-240">Agregaremos</span><span class="sxs-lookup"><span data-stu-id="05bab-240">We add</span></span>

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

> <span data-ttu-id="05bab-241">***Problema abierto***: ¿es el mejor nombre de la característica de *CLR* ?</span><span class="sxs-lookup"><span data-stu-id="05bab-241">***Open issue***: Is that the best name for the *CLR* feature?</span></span> <span data-ttu-id="05bab-242">La característica CLR hace mucho más que eso (por ejemplo, relaja las restricciones de protección, admite invalidaciones en interfaces, etc.).</span><span class="sxs-lookup"><span data-stu-id="05bab-242">The CLR feature does much more than just that (e.g. relaxes protection constraints, supports overrides in interfaces, etc).</span></span> <span data-ttu-id="05bab-243">Quizás se deba llamar a algo como "métodos concretos en interfaces" o "rasgos"?</span><span class="sxs-lookup"><span data-stu-id="05bab-243">Perhaps it should be called something like "concrete methods in interfaces", or "traits"?</span></span>

### <a name="further-areas-to-be-specified"></a><span data-ttu-id="05bab-244">Otras áreas que se van a especificar</span><span class="sxs-lookup"><span data-stu-id="05bab-244">Further areas to be specified</span></span>

- <span data-ttu-id="05bab-245">[] Sería útil catalogar los tipos de efectos de compatibilidad binarios y de origen que se producen al agregar métodos de interfaz predeterminados e invalidaciones a las interfaces existentes.</span><span class="sxs-lookup"><span data-stu-id="05bab-245">[ ] It would be useful to catalog the kinds of source and binary compatibility effects caused by adding default interface methods and overrides to existing interfaces.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="05bab-246">Desventajas</span><span class="sxs-lookup"><span data-stu-id="05bab-246">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="05bab-247">Esta propuesta requiere una actualización coordinada de la especificación CLR (para admitir métodos concretos en interfaces y resolución de métodos).</span><span class="sxs-lookup"><span data-stu-id="05bab-247">This proposal requires a coordinated update to the CLR specification (to support concrete methods in interfaces and method resolution).</span></span> <span data-ttu-id="05bab-248">Por lo tanto, es bastante "costosa" y puede que merezca la pena hacerlo en combinación con otras características que también se prevén para requerir cambios en CLR.</span><span class="sxs-lookup"><span data-stu-id="05bab-248">It is therefore fairly "expensive" and it may be worth doing in combination with other features that we also anticipate would require CLR changes.</span></span>

## <a name="alternatives"></a><span data-ttu-id="05bab-249">Alternativas</span><span class="sxs-lookup"><span data-stu-id="05bab-249">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="05bab-250">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="05bab-250">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="05bab-251">Preguntas no resueltas</span><span class="sxs-lookup"><span data-stu-id="05bab-251">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="05bab-252">Las preguntas abiertas se mencionan a lo largo de la propuesta anterior.</span><span class="sxs-lookup"><span data-stu-id="05bab-252">Open questions are called out throughout the proposal, above.</span></span>
- <span data-ttu-id="05bab-253">Vea también <https://github.com/dotnet/csharplang/issues/406> para obtener una lista de preguntas abiertas.</span><span class="sxs-lookup"><span data-stu-id="05bab-253">See also <https://github.com/dotnet/csharplang/issues/406> for a list of open questions.</span></span>
- <span data-ttu-id="05bab-254">La especificación detallada debe describir el mecanismo de resolución que se utiliza en tiempo de ejecución para seleccionar el método preciso que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="05bab-254">The detailed specification must describe the resolution mechanism used at runtime to select the precise method to be invoked.</span></span>
- <span data-ttu-id="05bab-255">La interacción de los metadatos generados por los nuevos compiladores y consumidos por los compiladores más antiguos debe trabajar en detalle.</span><span class="sxs-lookup"><span data-stu-id="05bab-255">The interaction of metadata produced by new compilers and consumed by older compilers needs to be worked out in detail.</span></span> <span data-ttu-id="05bab-256">Por ejemplo, debemos asegurarnos de que la representación de los metadatos que usamos no provoca la adición de una implementación predeterminada en una interfaz para interrumpir una clase existente que implementa esa interfaz cuando la compila un compilador anterior.</span><span class="sxs-lookup"><span data-stu-id="05bab-256">For example, we need to ensure that the metadata representation that we use does not cause the addition of a default implementation in an interface to break an existing class that implements that interface when compiled by an older compiler.</span></span> <span data-ttu-id="05bab-257">Esto puede afectar a la representación de los metadatos que se puede usar.</span><span class="sxs-lookup"><span data-stu-id="05bab-257">This may affect the metadata representation that we can use.</span></span>
- <span data-ttu-id="05bab-258">El diseño debe tener en cuenta la interoperación con otros lenguajes y los compiladores existentes para otros lenguajes.</span><span class="sxs-lookup"><span data-stu-id="05bab-258">The design must consider interoperation with other languages and existing compilers for other languages.</span></span>

## <a name="resolved-questions"></a><span data-ttu-id="05bab-259">Preguntas resueltas</span><span class="sxs-lookup"><span data-stu-id="05bab-259">Resolved Questions</span></span>

### <a name="abstract-override"></a><span data-ttu-id="05bab-260">Invalidación abstracta</span><span class="sxs-lookup"><span data-stu-id="05bab-260">Abstract Override</span></span>

<span data-ttu-id="05bab-261">La antigua especificación de borrador contenía la capacidad de "reabstraer" un método heredado:</span><span class="sxs-lookup"><span data-stu-id="05bab-261">The earlier draft spec contained the ability to "reabstract" an inherited method:</span></span>

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

<span data-ttu-id="05bab-262">Mis notas de 2017-03-20 mostraron que decidimos no permitirlo.</span><span class="sxs-lookup"><span data-stu-id="05bab-262">My notes for 2017-03-20 showed that we decided not to allow this.</span></span> <span data-ttu-id="05bab-263">Sin embargo, hay al menos dos casos de uso para ella:</span><span class="sxs-lookup"><span data-stu-id="05bab-263">However, there are at least two use cases for it:</span></span>

1. <span data-ttu-id="05bab-264">Las API de Java, con las que algunos usuarios de esta característica esperan interoperar, dependen de esta instalación.</span><span class="sxs-lookup"><span data-stu-id="05bab-264">The Java APIs, with which some users of this feature hope to interoperate, depend on this facility.</span></span>
2. <span data-ttu-id="05bab-265">La programación con *rasgos* se beneficia de este.</span><span class="sxs-lookup"><span data-stu-id="05bab-265">Programming with *traits* benefits from this.</span></span> <span data-ttu-id="05bab-266">La reabstracción es uno de los elementos de la característica de lenguaje "rasgos" (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span><span class="sxs-lookup"><span data-stu-id="05bab-266">Reabstraction is one of the elements of the "traits" language feature (https://en.wikipedia.org/wiki/Trait_(computer_programming)).</span></span> <span data-ttu-id="05bab-267">Se permite lo siguiente con las clases:</span><span class="sxs-lookup"><span data-stu-id="05bab-267">The following is permitted with classes:</span></span>

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

<span data-ttu-id="05bab-268">Desafortunadamente, este código no se puede refactorizar como un conjunto de interfaces (rasgos) a menos que se permita.</span><span class="sxs-lookup"><span data-stu-id="05bab-268">Unfortunately this code cannot be refactored as a set of interfaces (traits) unless this is permitted.</span></span> <span data-ttu-id="05bab-269">Por el *principio de Jared de expansivo*, se debe permitir.</span><span class="sxs-lookup"><span data-stu-id="05bab-269">By the *Jared principle of greed*, it should be permitted.</span></span>

> <span data-ttu-id="05bab-270">***Problema cerrado:*** ¿Se debe permitir la reabstracción?</span><span class="sxs-lookup"><span data-stu-id="05bab-270">***Closed issue:*** Should reabstraction be permitted?</span></span> <span data-ttu-id="05bab-271">? Mis notas eran incorrectas.</span><span class="sxs-lookup"><span data-stu-id="05bab-271">[YES] My notes were wrong.</span></span> <span data-ttu-id="05bab-272">Las [notas LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) indican que se permite la reabstracción en una interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-272">The [LDM notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) say that reabstraction is permitted in an interface.</span></span> <span data-ttu-id="05bab-273">No está en una clase.</span><span class="sxs-lookup"><span data-stu-id="05bab-273">Not in a class.</span></span>

### <a name="virtual-modifier-vs-sealed-modifier"></a><span data-ttu-id="05bab-274">Modificador virtual frente al modificador Sealed</span><span class="sxs-lookup"><span data-stu-id="05bab-274">Virtual Modifier vs Sealed Modifier</span></span>

<span data-ttu-id="05bab-275">Desde [Aleksey Tsingauz](https://github.com/AlekseyTs):</span><span class="sxs-lookup"><span data-stu-id="05bab-275">From [Aleksey Tsingauz](https://github.com/AlekseyTs):</span></span>

> <span data-ttu-id="05bab-276">Decidimos permitir los modificadores indicados explícitamente en los miembros de la interfaz, a menos que haya una razón para no permitir algunos de ellos.</span><span class="sxs-lookup"><span data-stu-id="05bab-276">We decided to allow modifiers explicitly stated on interface members, unless there is a reason to disallow some of them.</span></span> <span data-ttu-id="05bab-277">Esto ofrece una pregunta interesante sobre el modificador virtual.</span><span class="sxs-lookup"><span data-stu-id="05bab-277">This brings an interesting question around virtual modifier.</span></span> <span data-ttu-id="05bab-278">¿Debe ser necesario en los miembros con implementación predeterminada?</span><span class="sxs-lookup"><span data-stu-id="05bab-278">Should it be required on members with default implementation?</span></span>
>
> <span data-ttu-id="05bab-279">Podríamos decir que:</span><span class="sxs-lookup"><span data-stu-id="05bab-279">We could say that:</span></span>
>
> - <span data-ttu-id="05bab-280">Si no hay ninguna implementación y no se especifican ni virtual ni Sealed, se supone que el miembro es abstracto.</span><span class="sxs-lookup"><span data-stu-id="05bab-280">if there is no implementation and neither virtual, nor sealed are specified, we assume the member is abstract.</span></span>
> - <span data-ttu-id="05bab-281">Si hay una implementación de y no se especifican Abstract, ni Sealed, se supone que el miembro es virtual.</span><span class="sxs-lookup"><span data-stu-id="05bab-281">if there is an implementation and neither abstract, nor sealed are specified, we assume the member is virtual.</span></span>
> - <span data-ttu-id="05bab-282">el modificador Sealed es necesario para que un método no sea virtual ni abstracto.</span><span class="sxs-lookup"><span data-stu-id="05bab-282">sealed modifier is required to make a method neither virtual, nor abstract.</span></span>
>
> <span data-ttu-id="05bab-283">Como alternativa, podríamos indicar que el modificador virtual es necesario para un miembro virtual.</span><span class="sxs-lookup"><span data-stu-id="05bab-283">Alternatively, we could say that virtual modifier is required for a virtual member.</span></span> <span data-ttu-id="05bab-284">Es decir, si hay un miembro con una implementación no marcada explícitamente con el modificador virtual, no es virtual ni abstracta.</span><span class="sxs-lookup"><span data-stu-id="05bab-284">I.e, if there is a member with implementation not explicitly marked with virtual modifier, it is neither virtual, nor abstract.</span></span> <span data-ttu-id="05bab-285">Este enfoque puede proporcionar una mejor experiencia cuando se mueve un método de una clase a una interfaz:</span><span class="sxs-lookup"><span data-stu-id="05bab-285">This approach might provide better experience when a method is moved from a class to an interface:</span></span>
>
> - <span data-ttu-id="05bab-286">un método abstracto permanece abstracto.</span><span class="sxs-lookup"><span data-stu-id="05bab-286">an abstract method stays abstract.</span></span>
> - <span data-ttu-id="05bab-287">un método virtual sigue siendo virtual.</span><span class="sxs-lookup"><span data-stu-id="05bab-287">a virtual method stays virtual.</span></span>
> - <span data-ttu-id="05bab-288">un método sin modificador no sigue siendo virtual ni abstracto.</span><span class="sxs-lookup"><span data-stu-id="05bab-288">a method without any modifier stays neither virtual, nor abstract.</span></span>
> - <span data-ttu-id="05bab-289">el modificador Sealed no se puede aplicar a un método que no sea una invalidación.</span><span class="sxs-lookup"><span data-stu-id="05bab-289">sealed modifier cannot be applied to a method that is not an override.</span></span>
>
> <span data-ttu-id="05bab-290">¿Qué te parece?</span><span class="sxs-lookup"><span data-stu-id="05bab-290">What do you think?</span></span>

> <span data-ttu-id="05bab-291">***Problema cerrado:*** ¿Se debe `virtual`implícitamente un método concreto (con implementación)?</span><span class="sxs-lookup"><span data-stu-id="05bab-291">***Closed Issue:*** Should a concrete method (with implementation) be implicitly `virtual`?</span></span> <span data-ttu-id="05bab-292">?</span><span class="sxs-lookup"><span data-stu-id="05bab-292">[YES]</span></span>

<span data-ttu-id="05bab-293">***Decisiones:*** Realizada en el LDM 2017-04-05:</span><span class="sxs-lookup"><span data-stu-id="05bab-293">***Decisions:*** Made in the LDM 2017-04-05:</span></span>

1. <span data-ttu-id="05bab-294">no virtual se debe expresar explícitamente a través de `sealed` o `private`.</span><span class="sxs-lookup"><span data-stu-id="05bab-294">non-virtual should be explicitly expressed through `sealed` or `private`.</span></span>
2. <span data-ttu-id="05bab-295">`sealed` es la palabra clave para crear miembros de instancia de interfaz con cuerpos no virtuales</span><span class="sxs-lookup"><span data-stu-id="05bab-295">`sealed` is the keyword to make interface instance members with bodies non-virtual</span></span>
3. <span data-ttu-id="05bab-296">Queremos permitir todos los modificadores en las interfaces</span><span class="sxs-lookup"><span data-stu-id="05bab-296">We want to allow all modifiers in interfaces</span></span>  
4. <span data-ttu-id="05bab-297">La accesibilidad predeterminada para los miembros de interfaz es pública, incluidos los tipos anidados</span><span class="sxs-lookup"><span data-stu-id="05bab-297">Default accessibility for interface members is public, including nested types</span></span>
5. <span data-ttu-id="05bab-298">los miembros de función privados de las interfaces están sellados implícitamente y `sealed` no se permite en ellos.</span><span class="sxs-lookup"><span data-stu-id="05bab-298">private function members in interfaces are implicitly sealed, and `sealed` is not permitted on them.</span></span>
6. <span data-ttu-id="05bab-299">Las clases privadas (en interfaces) están permitidas y se pueden sellar, y eso significa que están selladas en la clase de sellado.</span><span class="sxs-lookup"><span data-stu-id="05bab-299">Private classes (in interfaces) are permitted and can be sealed, and that means sealed in the class sense of sealed.</span></span>
7. <span data-ttu-id="05bab-300">Si falta una buena propuesta, no se permite una parcial en las interfaces o sus miembros.</span><span class="sxs-lookup"><span data-stu-id="05bab-300">Absent a good proposal, partial is still not allowed on interfaces or their members.</span></span>

### <a name="binary-compatibility-1"></a><span data-ttu-id="05bab-301">Compatibilidad binaria 1</span><span class="sxs-lookup"><span data-stu-id="05bab-301">Binary Compatibility 1</span></span>

<span data-ttu-id="05bab-302">Cuando una biblioteca proporciona una implementación predeterminada</span><span class="sxs-lookup"><span data-stu-id="05bab-302">When a library provides a default implementation</span></span>

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

<span data-ttu-id="05bab-303">Sabemos que la implementación de `I1.M` en `C` es `I1.M`.</span><span class="sxs-lookup"><span data-stu-id="05bab-303">We understand that the implementation of `I1.M` in `C` is `I1.M`.</span></span> <span data-ttu-id="05bab-304">Qué sucede si el ensamblado que contiene `I2` se cambia como se indica a continuación y se vuelve a compilar</span><span class="sxs-lookup"><span data-stu-id="05bab-304">What if the assembly containing `I2` is changed as follows and recompiled</span></span>

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

<span data-ttu-id="05bab-305">pero `C` no se vuelve a compilar.</span><span class="sxs-lookup"><span data-stu-id="05bab-305">but `C` is not recompiled.</span></span> <span data-ttu-id="05bab-306">¿Qué ocurre cuando se ejecuta el programa?</span><span class="sxs-lookup"><span data-stu-id="05bab-306">What happens when the program is run?</span></span> <span data-ttu-id="05bab-307">Una invocación de `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="05bab-307">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="05bab-308">Ejecuta `I1.M`</span><span class="sxs-lookup"><span data-stu-id="05bab-308">Runs `I1.M`</span></span>
2. <span data-ttu-id="05bab-309">Ejecuta `I2.M`</span><span class="sxs-lookup"><span data-stu-id="05bab-309">Runs `I2.M`</span></span>
3. <span data-ttu-id="05bab-310">Produce algún tipo de error en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="05bab-310">Throws some kind of runtime error</span></span>

<span data-ttu-id="05bab-311">***Decisión:*** 2017-04-11: ejecuta `I2.M`, que es la invalidación más específica de forma inequívoca en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="05bab-311">***Decision:*** Made 2017-04-11: Runs `I2.M`, which is the unambiguously most specific override at runtime.</span></span>

### <a name="event-accessors-closed"></a><span data-ttu-id="05bab-312">Descriptores de acceso de eventos (cerrados)</span><span class="sxs-lookup"><span data-stu-id="05bab-312">Event accessors (closed)</span></span>

> <span data-ttu-id="05bab-313">***Problema cerrado:*** ¿Se puede invalidar un evento "a trozos"?</span><span class="sxs-lookup"><span data-stu-id="05bab-313">***Closed Issue:*** Can an event be overridden "piecewise"?</span></span>

<span data-ttu-id="05bab-314">Considere este caso:</span><span class="sxs-lookup"><span data-stu-id="05bab-314">Consider this case:</span></span>

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

<span data-ttu-id="05bab-315">Esta implementación "parcial" del evento no está permitida porque, como en una clase, la sintaxis de una declaración de evento no permite solo un descriptor de acceso; deben proporcionarse ambos (o ninguno).</span><span class="sxs-lookup"><span data-stu-id="05bab-315">This "partial" implementation of the event is not permitted because, as in a class, the syntax for an event declaration does not permit only one accessor; both (or neither) must be provided.</span></span> <span data-ttu-id="05bab-316">Puede lograr lo mismo si se permite que el descriptor de acceso Remove abstracto de la sintaxis sea abstracto implícitamente por la ausencia de un cuerpo:</span><span class="sxs-lookup"><span data-stu-id="05bab-316">You could accomplish the same thing by permitting the abstract remove accessor in the syntax to be implicitly abstract by the absence of a body:</span></span>

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

<span data-ttu-id="05bab-317">Tenga en cuenta que *se trata de una nueva sintaxis (propuesta)* .</span><span class="sxs-lookup"><span data-stu-id="05bab-317">Note that *this is a new (proposed) syntax*.</span></span> <span data-ttu-id="05bab-318">En la gramática actual, los descriptores de acceso de eventos tienen un cuerpo obligatorio.</span><span class="sxs-lookup"><span data-stu-id="05bab-318">In the current grammar, event accessors have a mandatory body.</span></span>

> <span data-ttu-id="05bab-319">***Problema cerrado:*** Puede que un descriptor de acceso de eventos sea abstracto (implícitamente) por la omisión de un cuerpo, de forma similar a la forma en que los métodos de las interfaces y los descriptores de acceso de propiedad son abstractos por la omisión de un cuerpo.</span><span class="sxs-lookup"><span data-stu-id="05bab-319">***Closed Issue:*** Can an event accessor be (implicitly) abstract by the omission of a body, similarly to the way that methods in interfaces and property accessors are (implicitly) abstract by the omission of a body?</span></span>

<span data-ttu-id="05bab-320">***Decisión:*** (2017-04-18) no, las declaraciones de eventos requieren ambos descriptores de acceso concretos (o ninguno).</span><span class="sxs-lookup"><span data-stu-id="05bab-320">***Decision:*** (2017-04-18) No, event declarations require both concrete accessors (or neither).</span></span>

### <a name="reabstraction-in-a-class-closed"></a><span data-ttu-id="05bab-321">Reabstracción en una clase (cerrada)</span><span class="sxs-lookup"><span data-stu-id="05bab-321">Reabstraction in a Class (closed)</span></span>

<span data-ttu-id="05bab-322">***Problema cerrado:*** Debemos confirmar que esto se permite (de lo contrario, agregar una implementación predeterminada sería un cambio importante):</span><span class="sxs-lookup"><span data-stu-id="05bab-322">***Closed Issue:*** We should confirm that this is permitted (otherwise adding a default implementation would be a breaking change):</span></span>

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

<span data-ttu-id="05bab-323">***Decisión:*** (2017-04-18) sí, agregar un cuerpo a una declaración de miembro de interfaz no debe romper C.</span><span class="sxs-lookup"><span data-stu-id="05bab-323">***Decision:*** (2017-04-18) Yes, adding a body to an interface member declaration shouldn't break C.</span></span>

### <a name="sealed-override-closed"></a><span data-ttu-id="05bab-324">Invalidación sellada (cerrada)</span><span class="sxs-lookup"><span data-stu-id="05bab-324">Sealed Override (closed)</span></span>

<span data-ttu-id="05bab-325">La pregunta anterior presupone implícitamente que el modificador `sealed` se puede aplicar a un `override` en una interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-325">The previous question implicitly assumes that the `sealed` modifier can be applied to an `override` in an interface.</span></span> <span data-ttu-id="05bab-326">Esto contradice la especificación del borrador.</span><span class="sxs-lookup"><span data-stu-id="05bab-326">This contradicts the draft specification.</span></span> <span data-ttu-id="05bab-327">¿Queremos permitir el sellado de una invalidación?</span><span class="sxs-lookup"><span data-stu-id="05bab-327">Do we want to permit sealing an override?</span></span> <span data-ttu-id="05bab-328">Se deben tener en cuenta los efectos de la compatibilidad binaria y de origen del sellado.</span><span class="sxs-lookup"><span data-stu-id="05bab-328">Source and binary compatibility effects of sealing should be considered.</span></span>

> <span data-ttu-id="05bab-329">***Problema cerrado:*** ¿Se debe permitir el sellado de una invalidación?</span><span class="sxs-lookup"><span data-stu-id="05bab-329">***Closed Issue:*** Should we permit sealing an override?</span></span>

<span data-ttu-id="05bab-330">***Decisión:*** (2017-04-18) no se permite `sealed` en las invalidaciones de las interfaces.</span><span class="sxs-lookup"><span data-stu-id="05bab-330">***Decision:*** (2017-04-18) Let's not allowed `sealed` on overrides in interfaces.</span></span> <span data-ttu-id="05bab-331">El único uso de `sealed` en los miembros de la interfaz es hacer que no sean virtuales en su declaración inicial.</span><span class="sxs-lookup"><span data-stu-id="05bab-331">The only use of `sealed` on interface members is to make them non-virtual in their initial declaration.</span></span>

### <a name="diamond-inheritance-and-classes-closed"></a><span data-ttu-id="05bab-332">Herencia y clases de Diamond (cerradas)</span><span class="sxs-lookup"><span data-stu-id="05bab-332">Diamond inheritance and classes (closed)</span></span>

<span data-ttu-id="05bab-333">El borrador de la propuesta prefiere invalidaciones de clase a invalidaciones de interfaz en escenarios de herencia de Diamond:</span><span class="sxs-lookup"><span data-stu-id="05bab-333">The draft of the proposal prefers class overrides to interface overrides in diamond inheritance scenarios:</span></span>

> <span data-ttu-id="05bab-334">Es necesario que todas las interfaces y clases tengan una *invalidación más específica* para cada método de interfaz entre las invalidaciones que aparecen en el tipo o sus interfaces directas e indirectas.</span><span class="sxs-lookup"><span data-stu-id="05bab-334">We require that every interface and class have a *most specific override* for every interface method among the overrides appearing in the type or its direct and indirect interfaces.</span></span> <span data-ttu-id="05bab-335">La *invalidación más específica* es una invalidación única que es más específica que cualquier otra invalidación.</span><span class="sxs-lookup"><span data-stu-id="05bab-335">The *most specific override* is a unique override that is more specific than every other override.</span></span> <span data-ttu-id="05bab-336">Si no hay ninguna invalidación, el propio método se considera la invalidación más específica.</span><span class="sxs-lookup"><span data-stu-id="05bab-336">If there is no override, the method itself is considered the most specific override.</span></span>
>
> <span data-ttu-id="05bab-337">Un `M1` de invalidación se considera *más específico* que otro `M2` de invalidación si `M1` se declara en el tipo `T1`, `M2` se declara en el tipo `T2`y o bien</span><span class="sxs-lookup"><span data-stu-id="05bab-337">One override `M1` is considered *more specific* than another override `M2` if `M1` is declared on type `T1`, `M2` is declared on type `T2`, and either</span></span>
>
> 1. <span data-ttu-id="05bab-338">`T1` contiene `T2` entre sus interfaces directas o indirectas, o</span><span class="sxs-lookup"><span data-stu-id="05bab-338">`T1` contains `T2` among its direct or indirect interfaces, or</span></span>
> 2. <span data-ttu-id="05bab-339">`T2` es un tipo de interfaz, pero `T1` no es un tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-339">`T2` is an interface type but `T1` is not an interface type.</span></span>

<span data-ttu-id="05bab-340">El escenario es este</span><span class="sxs-lookup"><span data-stu-id="05bab-340">The scenario is this</span></span>

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

<span data-ttu-id="05bab-341">Deberíamos confirmar este comportamiento (o decidir de otro modo)</span><span class="sxs-lookup"><span data-stu-id="05bab-341">We should confirm this behavior (or decide otherwise)</span></span>

> <span data-ttu-id="05bab-342">***Problema cerrado:*** Confirme la especificación del borrador, anterior, para *la invalidación más específica* , ya que se aplica a clases e interfaces mixtas (una clase tiene prioridad sobre una interfaz).</span><span class="sxs-lookup"><span data-stu-id="05bab-342">***Closed Issue:*** Confirm the draft spec, above, for *most specific override* as it applies to mixed classes and interfaces (a class takes priority over an interface).</span></span> <span data-ttu-id="05bab-343">Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span><span class="sxs-lookup"><span data-stu-id="05bab-343">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.</span></span>

### <a name="interface-methods-vs-structs-closed"></a><span data-ttu-id="05bab-344">Métodos de interfaz frente a estructuras (cerradas)</span><span class="sxs-lookup"><span data-stu-id="05bab-344">Interface methods vs structs (closed)</span></span>

<span data-ttu-id="05bab-345">Hay algunas interacciones desafortunadas entre los métodos de interfaz predeterminados y los Structs.</span><span class="sxs-lookup"><span data-stu-id="05bab-345">There are some unfortunate interactions between default interface methods and structs.</span></span>

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

<span data-ttu-id="05bab-346">Tenga en cuenta que los miembros de la interfaz no se heredan:</span><span class="sxs-lookup"><span data-stu-id="05bab-346">Note that interface members are not inherited:</span></span>

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

<span data-ttu-id="05bab-347">Por consiguiente, el cliente debe tener Box el struct para invocar los métodos de interfaz</span><span class="sxs-lookup"><span data-stu-id="05bab-347">Consequently, the client must box the struct to invoke interface methods</span></span>

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

<span data-ttu-id="05bab-348">La conversión boxing anula las ventajas principales de un tipo de `struct`.</span><span class="sxs-lookup"><span data-stu-id="05bab-348">Boxing in this way defeats the principal benefits of a `struct` type.</span></span> <span data-ttu-id="05bab-349">Además, los métodos de mutación no tendrán ningún efecto aparente, ya que funcionan en una *copia con conversión boxing* del struct:</span><span class="sxs-lookup"><span data-stu-id="05bab-349">Moreover, any mutation methods will have no apparent effect, because they are operating on a *boxed copy* of the struct:</span></span>

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

> <span data-ttu-id="05bab-350">***Problema cerrado:*** Qué se puede hacer a continuación:</span><span class="sxs-lookup"><span data-stu-id="05bab-350">***Closed Issue:*** What can we do about this:</span></span>
>
> 1. <span data-ttu-id="05bab-351">Prohibe que un `struct` herede una implementación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="05bab-351">Forbid a `struct` from inheriting a default implementation.</span></span> <span data-ttu-id="05bab-352">Todos los métodos de interfaz se tratarían como abstractos en una `struct`.</span><span class="sxs-lookup"><span data-stu-id="05bab-352">All interface methods would be treated as abstract in a `struct`.</span></span> <span data-ttu-id="05bab-353">Después, es posible que tarden tiempo en decidir cómo hacer que funcione mejor.</span><span class="sxs-lookup"><span data-stu-id="05bab-353">Then we may take time later to decide how to make it work better.</span></span>
> 2. <span data-ttu-id="05bab-354">Se incluye algún tipo de estrategia de generación de código que evita la conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="05bab-354">Come up with some kind of code generation strategy that avoids boxing.</span></span> <span data-ttu-id="05bab-355">Dentro de un método como `IB.Increment`, el tipo de `this` quizás sea similar a un parámetro de tipo restringido a `IB`.</span><span class="sxs-lookup"><span data-stu-id="05bab-355">Inside a method like `IB.Increment`, the type of `this` would perhaps be akin to a type parameter constrained to `IB`.</span></span> <span data-ttu-id="05bab-356">Junto con eso, para evitar la conversión boxing en el autor de la llamada, los métodos no abstractos se heredarían de las interfaces.</span><span class="sxs-lookup"><span data-stu-id="05bab-356">In conjunction with that, to avoid boxing in the caller, non-abstract methods would be inherited from interfaces.</span></span> <span data-ttu-id="05bab-357">Esto puede aumentar considerablemente el compilador y la implementación de CLR.</span><span class="sxs-lookup"><span data-stu-id="05bab-357">This may increase compiler and CLR implementation work substantially.</span></span>
> 3. <span data-ttu-id="05bab-358">No se preocupe y simplemente déjelo como wart.</span><span class="sxs-lookup"><span data-stu-id="05bab-358">Not worry about it and just leave it as a wart.</span></span>
> 4. <span data-ttu-id="05bab-359">¿Otras ideas?</span><span class="sxs-lookup"><span data-stu-id="05bab-359">Other ideas?</span></span>

<span data-ttu-id="05bab-360">***Decisión:*** No se preocupe y simplemente déjelo como wart.</span><span class="sxs-lookup"><span data-stu-id="05bab-360">***Decision:*** Not worry about it and just leave it as a wart.</span></span> <span data-ttu-id="05bab-361">Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span><span class="sxs-lookup"><span data-stu-id="05bab-361">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.</span></span>

### <a name="base-interface-invocations-closed"></a><span data-ttu-id="05bab-362">Invocaciones de interfaz base (cerradas)</span><span class="sxs-lookup"><span data-stu-id="05bab-362">Base interface invocations (closed)</span></span>

<span data-ttu-id="05bab-363">La especificación del borrador sugiere una sintaxis para las invocaciones de interfaz base inspirados por Java: `Interface.base.M()`.</span><span class="sxs-lookup"><span data-stu-id="05bab-363">The draft spec suggests a syntax for base interface invocations inspired by Java: `Interface.base.M()`.</span></span> <span data-ttu-id="05bab-364">Es necesario seleccionar una sintaxis, al menos para el prototipo inicial.</span><span class="sxs-lookup"><span data-stu-id="05bab-364">We need to select a syntax, at least for the initial prototype.</span></span> <span data-ttu-id="05bab-365">Mi favorito es `base<Interface>.M()`.</span><span class="sxs-lookup"><span data-stu-id="05bab-365">My favorite is `base<Interface>.M()`.</span></span>

> <span data-ttu-id="05bab-366">***Problema cerrado:*** ¿Cuál es la sintaxis de una invocación de miembro base?</span><span class="sxs-lookup"><span data-stu-id="05bab-366">***Closed Issue:*** What is the syntax for a base member invocation?</span></span>

<span data-ttu-id="05bab-367">***Decisión:*** La sintaxis es `base(Interface).M()`.</span><span class="sxs-lookup"><span data-stu-id="05bab-367">***Decision:*** The syntax is `base(Interface).M()`.</span></span> <span data-ttu-id="05bab-368">Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span><span class="sxs-lookup"><span data-stu-id="05bab-368">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>.</span></span> <span data-ttu-id="05bab-369">La interfaz con el mismo nombre debe ser una interfaz base, pero no es necesario que sea una interfaz base directa.</span><span class="sxs-lookup"><span data-stu-id="05bab-369">The interface so named must be a base interface, but does not need to be a direct base interface.</span></span>

> <span data-ttu-id="05bab-370">***Problema abierto:*** ¿Deben permitirse las invocaciones de interfaz base en los miembros de clase?</span><span class="sxs-lookup"><span data-stu-id="05bab-370">***Open Issue:*** Should base interface invocations be permitted in class members?</span></span>

<span data-ttu-id="05bab-371">***Decisión***: sí.</span><span class="sxs-lookup"><span data-stu-id="05bab-371">***Decision***: Yes.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a><span data-ttu-id="05bab-372">Invalidar miembros de la interfaz no pública (cerrado)</span><span class="sxs-lookup"><span data-stu-id="05bab-372">Overriding non-public interface members (closed)</span></span>

<span data-ttu-id="05bab-373">En una interfaz, los miembros no públicos de las interfaces base se invalidan mediante el modificador `override`.</span><span class="sxs-lookup"><span data-stu-id="05bab-373">In an interface, non-public members from base interfaces are overridden using the `override` modifier.</span></span> <span data-ttu-id="05bab-374">Si es una invalidación "explícita" que nombra la interfaz que contiene el miembro, se omite el modificador de acceso.</span><span class="sxs-lookup"><span data-stu-id="05bab-374">If it is an "explicit" override that names the interface containing the member, the access modifier is omitted.</span></span>

> <span data-ttu-id="05bab-375">***Problema cerrado:*** Si es una invalidación "implícita" que no denomina la interfaz, ¿debe coincidir el modificador de acceso?</span><span class="sxs-lookup"><span data-stu-id="05bab-375">***Closed Issue:*** If it is an "implicit" override that does not name the interface, does the access modifier have to match?</span></span>

<span data-ttu-id="05bab-376">***Decisión:*** Solo los miembros públicos se pueden invalidar implícitamente y el acceso debe coincidir.</span><span class="sxs-lookup"><span data-stu-id="05bab-376">***Decision:*** Only public members may be implicitly overridden, and the access must match.</span></span> <span data-ttu-id="05bab-377">Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="05bab-377">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

> <span data-ttu-id="05bab-378">***Problema abierto:*** ¿El modificador de acceso es obligatorio, opcional u omitido en una invalidación explícita como `override void IB.M() {}`?</span><span class="sxs-lookup"><span data-stu-id="05bab-378">***Open Issue:*** Is the access modifier required, optional, or omitted on an explicit override such as `override void IB.M() {}`?</span></span>

> <span data-ttu-id="05bab-379">***Problema abierto:*** ¿Es `override` obligatorio, opcional u omitido en una invalidación explícita como `void IB.M() {}`?</span><span class="sxs-lookup"><span data-stu-id="05bab-379">***Open Issue:*** Is `override` required, optional, or omitted on an explicit override such as `void IB.M() {}`?</span></span>

<span data-ttu-id="05bab-380">¿Cómo implementa un miembro de interfaz no pública en una clase?</span><span class="sxs-lookup"><span data-stu-id="05bab-380">How does one implement a non-public interface member in a class?</span></span> <span data-ttu-id="05bab-381">¿Es posible que se realice explícitamente?</span><span class="sxs-lookup"><span data-stu-id="05bab-381">Perhaps it must be done explicitly?</span></span>

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

> <span data-ttu-id="05bab-382">***Problema cerrado:*** ¿Cómo implementa un miembro de interfaz no pública en una clase?</span><span class="sxs-lookup"><span data-stu-id="05bab-382">***Closed Issue:*** How does one implement a non-public interface member in a class?</span></span>

<span data-ttu-id="05bab-383">***Decisión:*** Solo puede implementar explícitamente miembros de interfaz no públicos.</span><span class="sxs-lookup"><span data-stu-id="05bab-383">***Decision:*** You can only implement non-public interface members explicitly.</span></span> <span data-ttu-id="05bab-384">Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span><span class="sxs-lookup"><span data-stu-id="05bab-384">See <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.</span></span>

<span data-ttu-id="05bab-385">***Decisión***: no se permite ninguna palabra clave `override` en los miembros de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-385">***Decision***: No `override` keyword permitted on interface members.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a><span data-ttu-id="05bab-386">Compatibilidad binaria 2 (cerrada)</span><span class="sxs-lookup"><span data-stu-id="05bab-386">Binary Compatibility 2 (closed)</span></span>

<span data-ttu-id="05bab-387">Considere el siguiente código en el que cada tipo está en un ensamblado independiente.</span><span class="sxs-lookup"><span data-stu-id="05bab-387">Consider the following code in which each type is in a separate assembly</span></span>

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

<span data-ttu-id="05bab-388">Sabemos que la implementación de `I1.M` en `C` es `I2.M`.</span><span class="sxs-lookup"><span data-stu-id="05bab-388">We understand that the implementation of `I1.M` in `C` is `I2.M`.</span></span> <span data-ttu-id="05bab-389">Qué sucede si el ensamblado que contiene `I3` se cambia como se indica a continuación y se vuelve a compilar</span><span class="sxs-lookup"><span data-stu-id="05bab-389">What if the assembly containing `I3` is changed as follows and recompiled</span></span>

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

<span data-ttu-id="05bab-390">pero `C` no se vuelve a compilar.</span><span class="sxs-lookup"><span data-stu-id="05bab-390">but `C` is not recompiled.</span></span> <span data-ttu-id="05bab-391">¿Qué ocurre cuando se ejecuta el programa?</span><span class="sxs-lookup"><span data-stu-id="05bab-391">What happens when the program is run?</span></span> <span data-ttu-id="05bab-392">Una invocación de `(C as I1).M()`</span><span class="sxs-lookup"><span data-stu-id="05bab-392">An invocation of `(C as I1).M()`</span></span>

1. <span data-ttu-id="05bab-393">Ejecuta `I1.M`</span><span class="sxs-lookup"><span data-stu-id="05bab-393">Runs `I1.M`</span></span>
2. <span data-ttu-id="05bab-394">Ejecuta `I2.M`</span><span class="sxs-lookup"><span data-stu-id="05bab-394">Runs `I2.M`</span></span>
3. <span data-ttu-id="05bab-395">Ejecuta `I3.M`</span><span class="sxs-lookup"><span data-stu-id="05bab-395">Runs `I3.M`</span></span>
4. <span data-ttu-id="05bab-396">2 o 3, de manera determinista</span><span class="sxs-lookup"><span data-stu-id="05bab-396">Either 2 or 3, deterministically</span></span>
5. <span data-ttu-id="05bab-397">Produce algún tipo de excepción en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="05bab-397">Throws some kind of runtime exception</span></span>

<span data-ttu-id="05bab-398">***Decisión***: inicia una excepción (5).</span><span class="sxs-lookup"><span data-stu-id="05bab-398">***Decision***: Throw an exception (5).</span></span> <span data-ttu-id="05bab-399">Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span><span class="sxs-lookup"><span data-stu-id="05bab-399">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.</span></span>

### <a name="permit-partial-in-interface-closed"></a><span data-ttu-id="05bab-400">¿Permitir `partial` en la interfaz?</span><span class="sxs-lookup"><span data-stu-id="05bab-400">Permit `partial` in interface?</span></span> <span data-ttu-id="05bab-401">subtítulos</span><span class="sxs-lookup"><span data-stu-id="05bab-401">(closed)</span></span>

<span data-ttu-id="05bab-402">Dado que las interfaces se pueden usar de maneras análogas a la forma en que se usan las clases abstractas, puede ser útil declararlas `partial`.</span><span class="sxs-lookup"><span data-stu-id="05bab-402">Given that interfaces may be used in ways analogous to the way abstract classes are used, it may be useful to declare them `partial`.</span></span> <span data-ttu-id="05bab-403">Esto sería especialmente útil en el caso de los generadores.</span><span class="sxs-lookup"><span data-stu-id="05bab-403">This would be particularly useful in the face of generators.</span></span>

> <span data-ttu-id="05bab-404">***Propuesta:*** Quite la restricción de lenguaje que las interfaces y los miembros de las interfaces no se pueden declarar `partial`.</span><span class="sxs-lookup"><span data-stu-id="05bab-404">***Proposal:*** Remove the language restriction that interfaces and members of interfaces may not be declared `partial`.</span></span>

<span data-ttu-id="05bab-405">***Decisión***: sí.</span><span class="sxs-lookup"><span data-stu-id="05bab-405">***Decision***: Yes.</span></span> <span data-ttu-id="05bab-406">Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span><span class="sxs-lookup"><span data-stu-id="05bab-406">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.</span></span>

### <a name="main-in-an-interface-closed"></a><span data-ttu-id="05bab-407">`Main` en una interfaz?</span><span class="sxs-lookup"><span data-stu-id="05bab-407">`Main` in an interface?</span></span> <span data-ttu-id="05bab-408">subtítulos</span><span class="sxs-lookup"><span data-stu-id="05bab-408">(closed)</span></span>

> <span data-ttu-id="05bab-409">***Problema abierto:*** ¿Es un método `static Main` en una interfaz un candidato para ser el punto de entrada del programa?</span><span class="sxs-lookup"><span data-stu-id="05bab-409">***Open Issue:*** Is a `static Main` method in an interface a candidate to be the program's entry point?</span></span>

<span data-ttu-id="05bab-410">***Decisión***: sí.</span><span class="sxs-lookup"><span data-stu-id="05bab-410">***Decision***: Yes.</span></span> <span data-ttu-id="05bab-411">Vea <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span><span class="sxs-lookup"><span data-stu-id="05bab-411">See <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.</span></span>

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a><span data-ttu-id="05bab-412">Confirmar intención de admitir métodos públicos no virtuales (cerrados)</span><span class="sxs-lookup"><span data-stu-id="05bab-412">Confirm intent to support public non-virtual methods (closed)</span></span>

<span data-ttu-id="05bab-413">¿Podemos confirmar (o revertir) nuestra decisión de permitir métodos públicos no virtuales en una interfaz?</span><span class="sxs-lookup"><span data-stu-id="05bab-413">Can we please confirm (or reverse) our decision to permit non-virtual public methods in an interface?</span></span>

```csharp
interface IA
{
    public sealed void M() { }
}
```

> <span data-ttu-id="05bab-414">***Problema semicerrado:*** (2017-04-18) creemos que será útil, pero volveremos a él.</span><span class="sxs-lookup"><span data-stu-id="05bab-414">***Semi-Closed Issue:*** (2017-04-18) We think it is going to be useful, but will come back to it.</span></span> <span data-ttu-id="05bab-415">Se trata de un bloque de recorrido de modelos mental.</span><span class="sxs-lookup"><span data-stu-id="05bab-415">This is a mental model tripping block.</span></span>

<span data-ttu-id="05bab-416">***Decisión***: sí.</span><span class="sxs-lookup"><span data-stu-id="05bab-416">***Decision***: Yes.</span></span> <span data-ttu-id="05bab-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span><span class="sxs-lookup"><span data-stu-id="05bab-417"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.</span></span>

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a><span data-ttu-id="05bab-418">¿Una `override` de una interfaz introduce un nuevo miembro?</span><span class="sxs-lookup"><span data-stu-id="05bab-418">Does an `override` in an interface introduce a new member?</span></span> <span data-ttu-id="05bab-419">subtítulos</span><span class="sxs-lookup"><span data-stu-id="05bab-419">(closed)</span></span>

<span data-ttu-id="05bab-420">Hay varias maneras de observar si una declaración de invalidación introduce un nuevo miembro o no.</span><span class="sxs-lookup"><span data-stu-id="05bab-420">There are a few ways to observe whether an override declaration introduces a new member or not.</span></span>

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

> <span data-ttu-id="05bab-421">***Problema abierto:*** ¿Una declaración de invalidación en una interfaz introduce un nuevo miembro?</span><span class="sxs-lookup"><span data-stu-id="05bab-421">***Open Issue:*** Does an override declaration in an interface introduce a new member?</span></span> <span data-ttu-id="05bab-422">subtítulos</span><span class="sxs-lookup"><span data-stu-id="05bab-422">(closed)</span></span>

<span data-ttu-id="05bab-423">En una clase, un método de invalidación es visible en algunos sentidos.</span><span class="sxs-lookup"><span data-stu-id="05bab-423">In a class, an overriding method is "visible" in some senses.</span></span> <span data-ttu-id="05bab-424">Por ejemplo, los nombres de sus parámetros tienen prioridad sobre los nombres de los parámetros del método invalidado.</span><span class="sxs-lookup"><span data-stu-id="05bab-424">For example, the names of its parameters take precedence over the names of parameters in the overridden method.</span></span> <span data-ttu-id="05bab-425">Es posible que se duplique ese comportamiento en las interfaces, ya que siempre hay una invalidación más específica.</span><span class="sxs-lookup"><span data-stu-id="05bab-425">It may be possible to duplicate that behavior in interfaces, as there is always a most specific override.</span></span> <span data-ttu-id="05bab-426">¿Pero queremos duplicar ese comportamiento?</span><span class="sxs-lookup"><span data-stu-id="05bab-426">But do we want to duplicate that behavior?</span></span>

<span data-ttu-id="05bab-427">Además, es posible "invalidar" un método de invalidación?</span><span class="sxs-lookup"><span data-stu-id="05bab-427">Also, it is possible to "override" an override method?</span></span> <span data-ttu-id="05bab-428">[Moot]</span><span class="sxs-lookup"><span data-stu-id="05bab-428">[Moot]</span></span>

<span data-ttu-id="05bab-429">***Decisión***: no se permite ninguna palabra clave `override` en los miembros de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-429">***Decision***: No `override` keyword permitted on interface members.</span></span> <span data-ttu-id="05bab-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span><span class="sxs-lookup"><span data-stu-id="05bab-430"><https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.</span></span>

### <a name="properties-with-a-private-accessor-closed"></a><span data-ttu-id="05bab-431">Propiedades con un descriptor de acceso privado (cerrado)</span><span class="sxs-lookup"><span data-stu-id="05bab-431">Properties with a private accessor (closed)</span></span>

<span data-ttu-id="05bab-432">Suponemos que los miembros privados no son virtuales y la combinación de virtual y privada no está permitida.</span><span class="sxs-lookup"><span data-stu-id="05bab-432">We say that private members are not virtual, and the combination of virtual and private is disallowed.</span></span> <span data-ttu-id="05bab-433">Pero ¿qué ocurre con una propiedad con un descriptor de acceso privado?</span><span class="sxs-lookup"><span data-stu-id="05bab-433">But what about a property with a private accessor?</span></span>

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

<span data-ttu-id="05bab-434">¿Se permite?</span><span class="sxs-lookup"><span data-stu-id="05bab-434">Is this allowed?</span></span> <span data-ttu-id="05bab-435">¿Es el descriptor de acceso `set` aquí `virtual` o no?</span><span class="sxs-lookup"><span data-stu-id="05bab-435">Is the `set` accessor here `virtual` or not?</span></span> <span data-ttu-id="05bab-436">¿Se puede invalidar Cuando sea accesible?</span><span class="sxs-lookup"><span data-stu-id="05bab-436">Can it be overridden where it is accessible?</span></span> <span data-ttu-id="05bab-437">¿Lo siguiente implementa implícitamente solo el descriptor de acceso `get`?</span><span class="sxs-lookup"><span data-stu-id="05bab-437">Does the following implicitly implement only the `get` accessor?</span></span>

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

<span data-ttu-id="05bab-438">Es, supuestamente, un error debido a que IA. P. set no es virtual y también porque no se puede acceder a él.</span><span class="sxs-lookup"><span data-stu-id="05bab-438">Is the following presumably an error because IA.P.set isn't virtual and also because it isn't accessible?</span></span>

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

<span data-ttu-id="05bab-439">***Decisión***: el primer ejemplo es válido, mientras que el último no lo es.</span><span class="sxs-lookup"><span data-stu-id="05bab-439">***Decision***: The first example looks valid, while the last does not.</span></span> <span data-ttu-id="05bab-440">Esto se resuelve de forma análoga a como funciona en C#.</span><span class="sxs-lookup"><span data-stu-id="05bab-440">This is resolved analogously to how it already works in C#.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a><span data-ttu-id="05bab-441">Invocaciones de interfaz base, Round 2 (cerrado)</span><span class="sxs-lookup"><span data-stu-id="05bab-441">Base Interface Invocations, round 2 (closed)</span></span>

<span data-ttu-id="05bab-442">Nuestra "solución" anterior a la administración de las invocaciones base no proporciona una expresividad suficiente.</span><span class="sxs-lookup"><span data-stu-id="05bab-442">Our previous "resolution" to how to handle base invocations doesn't actually provide sufficient expressiveness.</span></span> <span data-ttu-id="05bab-443">Resulta que en y en C# CLR, a diferencia de Java, debe especificar tanto la interfaz que contiene la declaración de método como la ubicación de la implementación que desea invocar.</span><span class="sxs-lookup"><span data-stu-id="05bab-443">It turns out that in C# and the CLR, unlike Java, you need to specify both the interface containing the method declaration and the location of the implementation you want to invoke.</span></span>

<span data-ttu-id="05bab-444">Propongo la siguiente sintaxis para llamadas base en interfaces.</span><span class="sxs-lookup"><span data-stu-id="05bab-444">I propose the following syntax for base calls in interfaces.</span></span> <span data-ttu-id="05bab-445">No me encanta con él, pero muestra lo que cualquier sintaxis debe poder expresar:</span><span class="sxs-lookup"><span data-stu-id="05bab-445">I’m not in love with it, but it illustrates what any syntax must be able to express:</span></span>

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

<span data-ttu-id="05bab-446">Si no hay ambigüedad, puede escribirla más simplemente</span><span class="sxs-lookup"><span data-stu-id="05bab-446">If there is no ambiguity, you can write it more simply</span></span>

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

<span data-ttu-id="05bab-447">Or</span><span class="sxs-lookup"><span data-stu-id="05bab-447">Or</span></span>

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

<span data-ttu-id="05bab-448">Or</span><span class="sxs-lookup"><span data-stu-id="05bab-448">Or</span></span>

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

<span data-ttu-id="05bab-449">***Decisión***: se decidió `base(N.I1<T>).M(s)`, lo que suponen que si tenemos un enlace de invocación podría haber un problema aquí más adelante.</span><span class="sxs-lookup"><span data-stu-id="05bab-449">***Decision***: Decided on `base(N.I1<T>).M(s)`, conceding that if we have an invocation binding there may be problem here later on.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a><span data-ttu-id="05bab-450">ADVERTENCIA para que struct no implemente el método predeterminado</span><span class="sxs-lookup"><span data-stu-id="05bab-450">Warning for struct not implementing default method?</span></span> <span data-ttu-id="05bab-451">subtítulos</span><span class="sxs-lookup"><span data-stu-id="05bab-451">(closed)</span></span>

<span data-ttu-id="05bab-452">@vancem afirma que debemos considerar seriamente la posibilidad de generar una advertencia si una declaración de tipo de valor no invalida algún método de interfaz, incluso si heredaría una implementación de ese método de una interfaz.</span><span class="sxs-lookup"><span data-stu-id="05bab-452">@vancem asserts that we should seriously consider producing a warning if a value type declaration fails to override some interface method, even if it would inherit an implementation of that method from an interface.</span></span> <span data-ttu-id="05bab-453">Dado que hace que las llamadas a la conversión boxing y a los subminas estén limitadas.</span><span class="sxs-lookup"><span data-stu-id="05bab-453">Because it causes boxing and undermines constrained calls.</span></span>

<span data-ttu-id="05bab-454">***Decisión***: parece algo más adecuado para un analizador.</span><span class="sxs-lookup"><span data-stu-id="05bab-454">***Decision***: This seems like something more suited for an analyzer.</span></span> <span data-ttu-id="05bab-455">También parece que esta advertencia podría ser ruidosa, ya que se activará incluso si nunca se llama al método de interfaz predeterminado y no se producirá ninguna conversión boxing.</span><span class="sxs-lookup"><span data-stu-id="05bab-455">It also seems like this warning could be noisy, since it would fire even if the default interface method is never called and no boxing will ever occur.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a><span data-ttu-id="05bab-456">Constructores estáticos de interfaz (cerrados)</span><span class="sxs-lookup"><span data-stu-id="05bab-456">Interface static constructors (closed)</span></span>

<span data-ttu-id="05bab-457">¿Cuándo se ejecutan los constructores estáticos de interfaz?</span><span class="sxs-lookup"><span data-stu-id="05bab-457">When are interface static constructors run?</span></span>  <span data-ttu-id="05bab-458">El borrador actual de la CLI propone que se produce cuando se tiene acceso al primer método o campo estático.</span><span class="sxs-lookup"><span data-stu-id="05bab-458">The current CLI draft proposes that it occurs when the first static method or field is accessed.</span></span> <span data-ttu-id="05bab-459">Si no hay ninguno de ellos, ¿es posible que nunca se ejecute??</span><span class="sxs-lookup"><span data-stu-id="05bab-459">If there are neither of those then it might never be run??</span></span>

<span data-ttu-id="05bab-460">[2018-10-09 el equipo de CLR propone "va a reflejar lo que hacemos para los ValueTypes (comprobación de cctor en el acceso a cada método de instancia)"]</span><span class="sxs-lookup"><span data-stu-id="05bab-460">[2018-10-09 The CLR team proposes "Going to mirror what we do for valuetypes (cctor check on access to each instance method)"]</span></span>

<span data-ttu-id="05bab-461">***Decisión***: los constructores estáticos también se ejecutan en la entrada a métodos de instancia, si el constructor estático no se `beforefieldinit`, en cuyo caso los constructores estáticos se ejecutan antes del acceso al primer campo estático.</span><span class="sxs-lookup"><span data-stu-id="05bab-461">***Decision***: Static constructors are also run on entry to instance methods, if the static constructor was not `beforefieldinit`, in which case static constructors are run before access to the first static field.</span></span> <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a><span data-ttu-id="05bab-462">Design Meetings</span><span class="sxs-lookup"><span data-stu-id="05bab-462">Design meetings</span></span>

<span data-ttu-id="05bab-463">[2017-03-08 notas](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md) de la reunión de ldm
[2017-03-21 notas](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) de la reunión de LDM
[2017-03-23 Meeting "comportamiento de CLR para los métodos de interfaz predeterminados"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM notas](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md) de la reunión
[2017-04-11 notas](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md) de [la reunión](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) [LDM
](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) [2017-04-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) [2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) [LDM Notas de la reunión
notas](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md) de la reunión de [ldm de 2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
notas de la reunión del [LDM de 2018-11-14](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)



</span><span class="sxs-lookup"><span data-stu-id="05bab-463">[2017-03-08 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md)
[2017-03-23 meeting "CLR Behavior for Default Interface Methods"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md)
[2017-04-19 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md)
[2017-05-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md)
[2017-05-31 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md)
[2017-06-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM Meeting Notes](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)</span></span>
