---
ms.openlocfilehash: 8bc4bf6310fb8a8457beee167f18d30aaca10a8e
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876893"
---
# <a name="introduction"></a><span data-ttu-id="2f629-101">Introducción</span><span class="sxs-lookup"><span data-stu-id="2f629-101">Introduction</span></span>

<span data-ttu-id="2f629-102">C# (pronunciado "si sharp" en inglés) es un lenguaje de programación sencillo, moderno, orientado a objetos y con seguridad de tipos.</span><span class="sxs-lookup"><span data-stu-id="2f629-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="2f629-103">C#tiene sus raíces en la familia de lenguajes C y será inmediatamente familiar para los programadores de C, C++y Java.</span><span class="sxs-lookup"><span data-stu-id="2f629-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="2f629-104">C#está normalizado por ECMA International como el estándar ***ECMA-334*** y por ISO/IEC como el estándar ***iso/IEC 23270*** .</span><span class="sxs-lookup"><span data-stu-id="2f629-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="2f629-105">El C# compilador de Microsoft para el .NET Framework es una implementación compatible de ambos estándares.</span><span class="sxs-lookup"><span data-stu-id="2f629-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="2f629-106">C# es un lenguaje orientado a objetos, pero también incluye compatibilidad para programación ***orientada a componentes***.</span><span class="sxs-lookup"><span data-stu-id="2f629-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="2f629-107">El diseño de software contemporáneo se basa cada vez más en componentes de software en forma de paquetes independientes y autodescriptivos de funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="2f629-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="2f629-108">La clave de estos componentes es que presentan un modelo de programación con propiedades, métodos y eventos; tienen atributos que proporcionan información declarativa sobre el componente; e incorporan su propia documentación.</span><span class="sxs-lookup"><span data-stu-id="2f629-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="2f629-109">C#proporciona construcciones de lenguaje para admitir directamente estos conceptos, con C# lo que se crea un lenguaje muy natural en el que crear y usar componentes de software.</span><span class="sxs-lookup"><span data-stu-id="2f629-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="2f629-110">Varias características de C# ayudan en la construcción de aplicaciones sólidas y duraderas: La ***recolección de elementos*** no utilizados reclama automáticamente la memoria ocupada por objetos no usados. el ***control de excepciones*** proporciona un enfoque estructurado y extensible para la detección y recuperación de errores. y el diseño ***con seguridad de tipos*** del lenguaje hace que sea imposible leer desde variables no inicializadas, indizar matrices más allá de sus límites o realizar conversiones de tipo no comprobadas.</span><span class="sxs-lookup"><span data-stu-id="2f629-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="2f629-111">C# tiene un ***sistema de tipo unificado***.</span><span class="sxs-lookup"><span data-stu-id="2f629-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="2f629-112">Todos los tipos de C#, incluidos los tipos primitivos como `int` y `double`, se heredan de un único tipo `object` raíz.</span><span class="sxs-lookup"><span data-stu-id="2f629-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="2f629-113">Por lo tanto, todos los tipos comparten un conjunto de operaciones comunes, y los valores de todos los tipos se pueden almacenar, transportar y utilizar de manera coherente.</span><span class="sxs-lookup"><span data-stu-id="2f629-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="2f629-114">Además, C# admite tipos de valor y tipos de referencia definidos por el usuario, lo que permite la asignación dinámica de objetos, así como almacenamiento en línea de estructuras ligeras.</span><span class="sxs-lookup"><span data-stu-id="2f629-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="2f629-115">Para asegurarse de C# que los programas y las bibliotecas pueden evolucionar con el tiempo de una manera compatible, se ha hecho mucho C#hincapié en el control de ***versiones*** en el diseño de.</span><span class="sxs-lookup"><span data-stu-id="2f629-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="2f629-116">Muchos lenguajes de programación prestan poca atención a este problema y, como resultado, los programas escritos en dichos lenguajes se interrumpen con más frecuencia de lo necesario cuando se introducen nuevas versiones de las bibliotecas dependientes.</span><span class="sxs-lookup"><span data-stu-id="2f629-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="2f629-117">Los aspectos C#del diseño de que se vieron afectados directamente por las consideraciones sobre el `virtual` control `override` de versiones son los modificadores y independientes, las reglas para la resolución de sobrecarga de métodos y la compatibilidad con las declaraciones explícitas de miembros de interfaz.</span><span class="sxs-lookup"><span data-stu-id="2f629-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="2f629-118">En el resto de este capítulo se describen las características esenciales C# del lenguaje.</span><span class="sxs-lookup"><span data-stu-id="2f629-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="2f629-119">Aunque los capítulos posteriores describen las reglas y las excepciones en un modo orientado a los detalles y, a veces, de forma matemática, en este capítulo se realiza una mayor claridad y una brevedad a costa de la integridad.</span><span class="sxs-lookup"><span data-stu-id="2f629-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="2f629-120">La intención es proporcionar al lector una introducción al lenguaje que facilitará la escritura de programas iniciales y la lectura de capítulos posteriores.</span><span class="sxs-lookup"><span data-stu-id="2f629-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="2f629-121">Hola a todos</span><span class="sxs-lookup"><span data-stu-id="2f629-121">Hello world</span></span>

<span data-ttu-id="2f629-122">El programa "Hola mundo" tradicionalmente se usa para presentar un lenguaje de programación.</span><span class="sxs-lookup"><span data-stu-id="2f629-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="2f629-123">En este caso, se usa C#:</span><span class="sxs-lookup"><span data-stu-id="2f629-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="2f629-124">Normalmente, los archivos de código fuente de C# tienen la extensión de archivo `.cs`.</span><span class="sxs-lookup"><span data-stu-id="2f629-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="2f629-125">Suponiendo que el programa "Hola mundo" se almacena en el archivo `hello.cs`, el programa se puede compilar con el compilador de Microsoft C# mediante la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="2f629-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="2f629-126">que genera un ensamblado ejecutable denominado `hello.exe`.</span><span class="sxs-lookup"><span data-stu-id="2f629-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="2f629-127">La salida generada por esta aplicación cuando se ejecuta es</span><span class="sxs-lookup"><span data-stu-id="2f629-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="2f629-128">El programa "Hola mundo" empieza con una directiva `using` que hace referencia al espacio de nombres `System`.</span><span class="sxs-lookup"><span data-stu-id="2f629-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="2f629-129">Los espacios de nombres proporcionan un método jerárquico para organizar las bibliotecas y los programas de C#.</span><span class="sxs-lookup"><span data-stu-id="2f629-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="2f629-130">Los espacios de nombres contienen tipos y otros espacios de nombres; por ejemplo, el espacio de nombres `System` contiene varios tipos, como la clase `Console` a la que se hace referencia en el programa, y otros espacios de nombres, como `IO` y `Collections`.</span><span class="sxs-lookup"><span data-stu-id="2f629-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="2f629-131">Una directiva `using` que hace referencia a un espacio de nombres determinado permite el uso no calificado de los tipos que son miembros de ese espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="2f629-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="2f629-132">Debido a la directiva `using`, puede utilizar el programa `Console.WriteLine` como abreviatura de `System.Console.WriteLine`.</span><span class="sxs-lookup"><span data-stu-id="2f629-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="2f629-133">La clase `Hello` declarada por el programa "Hola mundo" tiene un miembro único, el método llamado `Main`.</span><span class="sxs-lookup"><span data-stu-id="2f629-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="2f629-134">El `Main` método se declara con el `static` modificador.</span><span class="sxs-lookup"><span data-stu-id="2f629-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="2f629-135">Mientras que los métodos de instancia pueden hacer referencia a una instancia de objeto envolvente determinada utilizando la palabra clave `this`, los métodos estáticos funcionan sin referencia a un objeto determinado.</span><span class="sxs-lookup"><span data-stu-id="2f629-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="2f629-136">Por convención, un método estático denominado `Main` sirve como punto de entrada de un programa.</span><span class="sxs-lookup"><span data-stu-id="2f629-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="2f629-137">La salida del programa la genera el método `WriteLine` de la clase `Console` en el espacio de nombres `System`.</span><span class="sxs-lookup"><span data-stu-id="2f629-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="2f629-138">Esta clase la proporcionan las bibliotecas de clases de .NET Framework, a las que, de forma predeterminada, el compilador de Microsoft C# hace referencia automáticamente.</span><span class="sxs-lookup"><span data-stu-id="2f629-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="2f629-139">Tenga en C# cuenta que no tiene una biblioteca en tiempo de ejecución independiente.</span><span class="sxs-lookup"><span data-stu-id="2f629-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="2f629-140">En su lugar, el .NET Framework es la biblioteca en C#tiempo de ejecución de.</span><span class="sxs-lookup"><span data-stu-id="2f629-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="2f629-141">Estructura del programa</span><span class="sxs-lookup"><span data-stu-id="2f629-141">Program structure</span></span>

<span data-ttu-id="2f629-142">Los principales conceptos organizativos en C# son ***programas***, ***espacios de nombres***, ***tipos***, ***miembros*** y ***ensamblados***.</span><span class="sxs-lookup"><span data-stu-id="2f629-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="2f629-143">Los programas de C# constan de uno o más archivos de origen.</span><span class="sxs-lookup"><span data-stu-id="2f629-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="2f629-144">Los programas declaran tipos, que contienen miembros y pueden organizarse en espacios de nombres.</span><span class="sxs-lookup"><span data-stu-id="2f629-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="2f629-145">Las clases e interfaces son ejemplos de tipos.</span><span class="sxs-lookup"><span data-stu-id="2f629-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="2f629-146">Los campos, los métodos, las propiedades y los eventos son ejemplos de miembros.</span><span class="sxs-lookup"><span data-stu-id="2f629-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="2f629-147">Cuando se compilan programas de C#, se empaquetan físicamente en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="2f629-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="2f629-148">Normalmente, los ensamblados tienen `.exe` la `.dll`extensión de archivo o, dependiendo de si implementan ***aplicaciones*** o ***bibliotecas***.</span><span class="sxs-lookup"><span data-stu-id="2f629-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="2f629-149">El ejemplo</span><span class="sxs-lookup"><span data-stu-id="2f629-149">The example</span></span>

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
<span data-ttu-id="2f629-150">declara una clase denominada `Stack` en un espacio de nombres denominado. `Acme.Collections`</span><span class="sxs-lookup"><span data-stu-id="2f629-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="2f629-151">El nombre completo de esta clase es `Acme.Collections.Stack`.</span><span class="sxs-lookup"><span data-stu-id="2f629-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="2f629-152">La clase contiene varios miembros: un campo denominado `top`, dos métodos denominados `Push` y `Pop`, y una clase anidada denominada `Entry`.</span><span class="sxs-lookup"><span data-stu-id="2f629-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="2f629-153">La clase `Entry` contiene además tres miembros: un campo denominado `next`, un campo denominado `data` y un constructor.</span><span class="sxs-lookup"><span data-stu-id="2f629-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="2f629-154">Suponiendo que el código fuente del ejemplo se almacene en el archivo `acme.cs`, la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="2f629-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="2f629-155">compila el ejemplo como una biblioteca (código sin un punto de entrada `Main` y genera un ensamblado denominado `acme.dll`.</span><span class="sxs-lookup"><span data-stu-id="2f629-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="2f629-156">Los ensamblados contienen código ejecutable en forma de instrucciones de ***lenguaje intermedio*** (IL) e información simbólica en forma de ***metadatos***.</span><span class="sxs-lookup"><span data-stu-id="2f629-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="2f629-157">Antes de ejecutarlo, el código de IL de un ensamblado se convierte automáticamente en código específico del procesador mediante el compilador de Just in Time (JIT) de .NET Common Language Runtime.</span><span class="sxs-lookup"><span data-stu-id="2f629-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="2f629-158">Dado que un ensamblado es una unidad autodescriptiva de funcionalidad que contiene código y metadatos, no hay necesidad de directivas `#include` ni archivos de encabezado de C#.</span><span class="sxs-lookup"><span data-stu-id="2f629-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="2f629-159">Los tipos y miembros públicos contenidos en un ensamblado determinado estarán disponibles en un programa de C# simplemente haciendo referencia a dicho ensamblado al compilar el programa.</span><span class="sxs-lookup"><span data-stu-id="2f629-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="2f629-160">Por ejemplo, este programa usa la clase `Acme.Collections.Stack` desde el ensamblado `acme.dll`:</span><span class="sxs-lookup"><span data-stu-id="2f629-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
<span data-ttu-id="2f629-161">Si el programa se almacena en el archivo `test.cs`, cuando `test.cs` se compila, `acme.dll` se puede hacer referencia al ensamblado mediante la opción `/r` del compilador:</span><span class="sxs-lookup"><span data-stu-id="2f629-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="2f629-162">Esto crea un ensamblado ejecutable denominado `test.exe`, que, cuando se ejecuta, produce el resultado:</span><span class="sxs-lookup"><span data-stu-id="2f629-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="2f629-163">C# permite que el texto de origen de un programa se almacene en varios archivos de origen.</span><span class="sxs-lookup"><span data-stu-id="2f629-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="2f629-164">Cuando se compila un programa de C# de varios archivos, todos los archivos de origen se procesan juntos y los archivos de origen pueden hacerse referencia mutuamente de forma libre; desde un punto de vista conceptual, es como si todos los archivos de origen estuvieran concatenados en un archivo de gran tamaño antes de ser procesados.</span><span class="sxs-lookup"><span data-stu-id="2f629-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="2f629-165">En C# nunca se necesitan declaraciones adelantadas porque, excepto en contadas ocasiones, el orden de declaración es insignificante.</span><span class="sxs-lookup"><span data-stu-id="2f629-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="2f629-166">C# no limita un archivo de origen a declarar solamente un tipo público ni precisa que el nombre del archivo de origen coincida con un tipo declarado en el archivo de origen.</span><span class="sxs-lookup"><span data-stu-id="2f629-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="2f629-167">Tipos y variables</span><span class="sxs-lookup"><span data-stu-id="2f629-167">Types and variables</span></span>

<span data-ttu-id="2f629-168">Hay dos clases de tipos en C#: ***tipos de valor*** y ***tipos de referencia***.</span><span class="sxs-lookup"><span data-stu-id="2f629-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="2f629-169">Las variables de tipos de valor contienen directamente los datos, mientras que las variables de los tipos de referencia almacenan referencias a los datos, lo que se conoce como objetos.</span><span class="sxs-lookup"><span data-stu-id="2f629-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="2f629-170">Con los tipos de referencia, es posible que dos variables hagan referencia al mismo objeto y que, por tanto, las operaciones en una variable afecten al objeto al que hace referencia la otra variable.</span><span class="sxs-lookup"><span data-stu-id="2f629-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="2f629-171">Con los tipos de valor, cada variable tiene su propia copia de los datos y no es posible que las operaciones en una variable afecten a la otra (excepto en el caso de las variables de parámetro `ref` y `out`).</span><span class="sxs-lookup"><span data-stu-id="2f629-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="2f629-172">C#los tipos de valor de se dividen en ***tipos simples***, ***tipos de enumeración***, ***tipos de struct***y tipos que C# ***aceptan valores NULL***, y los tipos de referencia de se dividen en ***tipos de clase***, ***tipos de interfaz***, ***matriz tipos***y ***tipos de delegado***.</span><span class="sxs-lookup"><span data-stu-id="2f629-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="2f629-173">En la tabla siguiente se proporciona información C#general sobre el sistema de tipos de.</span><span class="sxs-lookup"><span data-stu-id="2f629-173">The following table provides an overview of C#'s type system.</span></span>

| <span data-ttu-id="2f629-174">__Categoría__</span><span class="sxs-lookup"><span data-stu-id="2f629-174">__Category__</span></span>    |                 | <span data-ttu-id="2f629-175">__Descripción__</span><span class="sxs-lookup"><span data-stu-id="2f629-175">__Description__</span></span> |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="2f629-176">Tipos de valor</span><span class="sxs-lookup"><span data-stu-id="2f629-176">Value types</span></span>     | <span data-ttu-id="2f629-177">Tipos simples</span><span class="sxs-lookup"><span data-stu-id="2f629-177">Simple types</span></span>    | <span data-ttu-id="2f629-178">Entero con signo: `sbyte`, `short`, `int`,`long`</span><span class="sxs-lookup"><span data-stu-id="2f629-178">Signed integral: `sbyte`, `short`, `int`, `long`</span></span> |
|                 |                 | <span data-ttu-id="2f629-179">Entero sin signo: `byte`, `ushort`, `uint`,`ulong`</span><span class="sxs-lookup"><span data-stu-id="2f629-179">Unsigned integral: `byte`, `ushort`, `uint`, `ulong`</span></span> |
|                 |                 | <span data-ttu-id="2f629-180">Caracteres Unicode: `char`</span><span class="sxs-lookup"><span data-stu-id="2f629-180">Unicode characters: `char`</span></span> |
|                 |                 | <span data-ttu-id="2f629-181">Punto flotante de IEEE: `float`, `double`</span><span class="sxs-lookup"><span data-stu-id="2f629-181">IEEE floating point: `float`, `double`</span></span> |
|                 |                 | <span data-ttu-id="2f629-182">Decimal de alta precisión: `decimal`</span><span class="sxs-lookup"><span data-stu-id="2f629-182">High-precision decimal: `decimal`</span></span> |
|                 |                 | <span data-ttu-id="2f629-183">Booleano: `bool`</span><span class="sxs-lookup"><span data-stu-id="2f629-183">Boolean: `bool`</span></span> |
|                 | <span data-ttu-id="2f629-184">Tipos de enumeración</span><span class="sxs-lookup"><span data-stu-id="2f629-184">Enum types</span></span>      | <span data-ttu-id="2f629-185">Tipos definidos por el usuario con el formato `enum E {...}`</span><span class="sxs-lookup"><span data-stu-id="2f629-185">User-defined types of the form `enum E {...}`</span></span> |
|                 | <span data-ttu-id="2f629-186">Tipos de estructura</span><span class="sxs-lookup"><span data-stu-id="2f629-186">Struct types</span></span>    | <span data-ttu-id="2f629-187">Tipos definidos por el usuario con el formato `struct S {...}`</span><span class="sxs-lookup"><span data-stu-id="2f629-187">User-defined types of the form `struct S {...}`</span></span> |
|                 | <span data-ttu-id="2f629-188">Tipos que aceptan valores NULL</span><span class="sxs-lookup"><span data-stu-id="2f629-188">Nullable types</span></span>  | <span data-ttu-id="2f629-189">Extensiones de todos los demás tipos de valor con un valor `null`</span><span class="sxs-lookup"><span data-stu-id="2f629-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="2f629-190">Tipos de referencia</span><span class="sxs-lookup"><span data-stu-id="2f629-190">Reference types</span></span> | <span data-ttu-id="2f629-191">Tipos de clase</span><span class="sxs-lookup"><span data-stu-id="2f629-191">Class types</span></span>     | <span data-ttu-id="2f629-192">Clase base definitiva de todos los demás tipos: `object`</span><span class="sxs-lookup"><span data-stu-id="2f629-192">Ultimate base class of all other types: `object`</span></span> |
|                 |                 | <span data-ttu-id="2f629-193">Cadenas Unicode: `string`</span><span class="sxs-lookup"><span data-stu-id="2f629-193">Unicode strings: `string`</span></span> |
|                 |                 | <span data-ttu-id="2f629-194">Tipos definidos por el usuario con el formato `class C {...}`</span><span class="sxs-lookup"><span data-stu-id="2f629-194">User-defined types of the form `class C {...}`</span></span> |
|                 | <span data-ttu-id="2f629-195">Tipos de interfaz</span><span class="sxs-lookup"><span data-stu-id="2f629-195">Interface types</span></span> | <span data-ttu-id="2f629-196">Tipos definidos por el usuario con el formato `interface I {...}`</span><span class="sxs-lookup"><span data-stu-id="2f629-196">User-defined types of the form `interface I {...}`</span></span> |
|                 | <span data-ttu-id="2f629-197">Tipos de matriz</span><span class="sxs-lookup"><span data-stu-id="2f629-197">Array types</span></span>     | <span data-ttu-id="2f629-198">Unidimensional y multidimensional; por ejemplo, `int[]` y `int[,]`</span><span class="sxs-lookup"><span data-stu-id="2f629-198">Single- and multi-dimensional, for example, `int[]` and `int[,]`</span></span> |
|                 | <span data-ttu-id="2f629-199">Tipos delegados</span><span class="sxs-lookup"><span data-stu-id="2f629-199">Delegate types</span></span>  | <span data-ttu-id="2f629-200">Tipos definidos por el usuario del formulario, por ejemplo,`delegate int  D(...)`</span><span class="sxs-lookup"><span data-stu-id="2f629-200">User-defined types of the form e.g. `delegate int  D(...)`</span></span> |

<span data-ttu-id="2f629-201">Los ocho tipos enteros proporcionan compatibilidad con valores de 8, 16, 32 y 64 bits en formato con o sin signo.</span><span class="sxs-lookup"><span data-stu-id="2f629-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="2f629-202">Los dos tipos de punto flotante `float` , `double`y, se representan mediante los formatos IEEE 754 de precisión sencilla de 32 bits y de doble precisión de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="2f629-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="2f629-203">El tipo `decimal` es un tipo de datos de 128 bits adecuado para cálculos financieros y monetarios.</span><span class="sxs-lookup"><span data-stu-id="2f629-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="2f629-204">C#el `bool` tipo de se usa para representar valores booleanos: valores que `true` son `false`o.</span><span class="sxs-lookup"><span data-stu-id="2f629-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="2f629-205">El procesamiento de caracteres y cadenas en C# utiliza la codificación Unicode.</span><span class="sxs-lookup"><span data-stu-id="2f629-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="2f629-206">El tipo `char` representa una unidad de código UTF-16 y el tipo `string` representa una secuencia de unidades de código UTF-16.</span><span class="sxs-lookup"><span data-stu-id="2f629-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="2f629-207">En la tabla siguiente se C#resumen los tipos numéricos de.</span><span class="sxs-lookup"><span data-stu-id="2f629-207">The following table summarizes C#'s numeric types.</span></span>


| <span data-ttu-id="2f629-208">__Categoría__</span><span class="sxs-lookup"><span data-stu-id="2f629-208">__Category__</span></span>      | <span data-ttu-id="2f629-209">__Parada__</span><span class="sxs-lookup"><span data-stu-id="2f629-209">__Bits__</span></span> | <span data-ttu-id="2f629-210">__Type__</span><span class="sxs-lookup"><span data-stu-id="2f629-210">__Type__</span></span>  | <span data-ttu-id="2f629-211">__Intervalo/precisión__</span><span class="sxs-lookup"><span data-stu-id="2f629-211">__Range/Precision__</span></span> |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="2f629-212">Entero con signo</span><span class="sxs-lookup"><span data-stu-id="2f629-212">Signed integral</span></span>   | <span data-ttu-id="2f629-213">8</span><span class="sxs-lookup"><span data-stu-id="2f629-213">8</span></span>        | `sbyte`   | <span data-ttu-id="2f629-214">-128... 127</span><span class="sxs-lookup"><span data-stu-id="2f629-214">-128...127</span></span> |
|                   | <span data-ttu-id="2f629-215">16</span><span class="sxs-lookup"><span data-stu-id="2f629-215">16</span></span>       | `short`   | <span data-ttu-id="2f629-216">-32768... 32, 767</span><span class="sxs-lookup"><span data-stu-id="2f629-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="2f629-217">32</span><span class="sxs-lookup"><span data-stu-id="2f629-217">32</span></span>       | `int`     | <span data-ttu-id="2f629-218">-2147483648... 2, 147, 483, 647</span><span class="sxs-lookup"><span data-stu-id="2f629-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="2f629-219">64</span><span class="sxs-lookup"><span data-stu-id="2f629-219">64</span></span>       | `long`    | <span data-ttu-id="2f629-220">-9223372036854775808... 9, 223, 372, 036, 854, 775, 807</span><span class="sxs-lookup"><span data-stu-id="2f629-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="2f629-221">Entero sin signo</span><span class="sxs-lookup"><span data-stu-id="2f629-221">Unsigned integral</span></span> | <span data-ttu-id="2f629-222">8</span><span class="sxs-lookup"><span data-stu-id="2f629-222">8</span></span>        | `byte`    | <span data-ttu-id="2f629-223">0... 255</span><span class="sxs-lookup"><span data-stu-id="2f629-223">0...255</span></span> |
|                   | <span data-ttu-id="2f629-224">16</span><span class="sxs-lookup"><span data-stu-id="2f629-224">16</span></span>       | `ushort`  | <span data-ttu-id="2f629-225">0... 65, 535</span><span class="sxs-lookup"><span data-stu-id="2f629-225">0...65,535</span></span> |
|                   | <span data-ttu-id="2f629-226">32</span><span class="sxs-lookup"><span data-stu-id="2f629-226">32</span></span>       | `uint`    | <span data-ttu-id="2f629-227">0... 4, 294, 967, 295</span><span class="sxs-lookup"><span data-stu-id="2f629-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="2f629-228">64</span><span class="sxs-lookup"><span data-stu-id="2f629-228">64</span></span>       | `ulong`   | <span data-ttu-id="2f629-229">0... 18, 446, 744, 073, 709, 551, 615</span><span class="sxs-lookup"><span data-stu-id="2f629-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="2f629-230">Punto flotante</span><span class="sxs-lookup"><span data-stu-id="2f629-230">Floating point</span></span>    | <span data-ttu-id="2f629-231">32</span><span class="sxs-lookup"><span data-stu-id="2f629-231">32</span></span>       | `float`   | <span data-ttu-id="2f629-232">1,5 × 10 ^ − 45 a 3,4 × 10 ^ 38, precisión de 7 dígitos</span><span class="sxs-lookup"><span data-stu-id="2f629-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="2f629-233">64</span><span class="sxs-lookup"><span data-stu-id="2f629-233">64</span></span>       | `double`  | <span data-ttu-id="2f629-234">5,0 × 10 ^ − 324 a 1,7 × 10 ^ 308, precisión de 15 dígitos</span><span class="sxs-lookup"><span data-stu-id="2f629-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="2f629-235">Decimal</span><span class="sxs-lookup"><span data-stu-id="2f629-235">Decimal</span></span>           | <span data-ttu-id="2f629-236">128</span><span class="sxs-lookup"><span data-stu-id="2f629-236">128</span></span>      | `decimal` | <span data-ttu-id="2f629-237">1,0 × 10 ^ − 28 a 7,9 × 10 ^ 28, precisión de 28 dígitos</span><span class="sxs-lookup"><span data-stu-id="2f629-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="2f629-238">Los programas de C# utilizan ***declaraciones de tipos*** para crear nuevos tipos.</span><span class="sxs-lookup"><span data-stu-id="2f629-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="2f629-239">Una declaración de tipos especifica el nombre y los miembros del nuevo tipo.</span><span class="sxs-lookup"><span data-stu-id="2f629-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="2f629-240">Cinco de C#las categorías de tipos de son definibles por el usuario: tipos de clase, tipos de struct, tipos de interfaz, tipos de enumeración y tipos de delegado.</span><span class="sxs-lookup"><span data-stu-id="2f629-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="2f629-241">Un tipo de clase define una estructura de datos que contiene miembros de datos (campos) y miembros de función (métodos, propiedades, etc.).</span><span class="sxs-lookup"><span data-stu-id="2f629-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="2f629-242">Los tipos de clase admiten herencia única y polimorfismo, mecanismos por los que las clases derivadas pueden extender y especializar clases base.</span><span class="sxs-lookup"><span data-stu-id="2f629-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="2f629-243">Un tipo de estructura es similar a un tipo de clase en que representa una estructura con miembros de datos y miembros de función.</span><span class="sxs-lookup"><span data-stu-id="2f629-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="2f629-244">Sin embargo, a diferencia de las clases, las estructuras son tipos de valor y no requieren la asignación del montón.</span><span class="sxs-lookup"><span data-stu-id="2f629-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="2f629-245">Los tipos struct no admiten la herencia especificada por el usuario y todos los tipos de struct se heredan implícitamente del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="2f629-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="2f629-246">Un tipo de interfaz define un contrato como un conjunto con nombre de miembros de función públicos.</span><span class="sxs-lookup"><span data-stu-id="2f629-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="2f629-247">Una clase o estructura que implementa una interfaz debe proporcionar implementaciones de los miembros de función de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="2f629-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="2f629-248">Una interfaz puede heredar de varias interfaces base, y una clase o estructura puede implementar varias interfaces.</span><span class="sxs-lookup"><span data-stu-id="2f629-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="2f629-249">Un tipo de delegado representa referencias a métodos con una lista de parámetros determinada y un tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="2f629-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="2f629-250">Los delegados permiten tratar métodos como entidades que se puedan asignar a variables y se puedan pasar como parámetros.</span><span class="sxs-lookup"><span data-stu-id="2f629-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="2f629-251">Los delegados son similares al concepto de punteros de función en otros lenguajes, pero a diferencia de los punteros de función, los delegados están orientados a objetos y presentan seguridad de tipos.</span><span class="sxs-lookup"><span data-stu-id="2f629-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="2f629-252">Los tipos de clase, estructura, interfaz y delegado admiten genéricos, mediante los cuales se pueden parametrizar con otros tipos.</span><span class="sxs-lookup"><span data-stu-id="2f629-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="2f629-253">Un tipo de enumeración es un tipo distinto con constantes con nombre.</span><span class="sxs-lookup"><span data-stu-id="2f629-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="2f629-254">Cada tipo de enumeración tiene un tipo subyacente, que debe ser uno de los ocho tipos enteros.</span><span class="sxs-lookup"><span data-stu-id="2f629-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="2f629-255">El conjunto de valores de un tipo de enumeración es igual que el conjunto de valores del tipo subyacente.</span><span class="sxs-lookup"><span data-stu-id="2f629-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="2f629-256">C# admite matrices unidimensionales y multidimensionales de cualquier tipo.</span><span class="sxs-lookup"><span data-stu-id="2f629-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="2f629-257">A diferencia de los tipos enumerados anteriormente, los tipos de matriz no tienen que ser declarados antes de usarlos.</span><span class="sxs-lookup"><span data-stu-id="2f629-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="2f629-258">En su lugar, los tipos de matriz se crean mediante un nombre de tipo entre corchetes.</span><span class="sxs-lookup"><span data-stu-id="2f629-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="2f629-259">Por ejemplo, `int[]` es una matriz unidimensional de `int`, `int[,]` es una matriz bidimensional de `int`y `int[][]` es una matriz unidimensional de Matrices unidimensionales de `int`.</span><span class="sxs-lookup"><span data-stu-id="2f629-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="2f629-260">Los tipos que aceptan valores NULL tampoco tienen que declararse antes de que se puedan utilizar.</span><span class="sxs-lookup"><span data-stu-id="2f629-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="2f629-261">Para cada tipo `T` de valor que no acepta valores NULL, existe un tipo `T?`que acepta valores NULL correspondiente, que puede `null`contener un valor adicional.</span><span class="sxs-lookup"><span data-stu-id="2f629-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="2f629-262">Por ejemplo, `int?` es un tipo que puede contener cualquier entero de 32 bits o el valor `null`.</span><span class="sxs-lookup"><span data-stu-id="2f629-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="2f629-263">C#el sistema de tipos de está unificado, de modo que un valor de cualquier tipo se puede tratar como un objeto.</span><span class="sxs-lookup"><span data-stu-id="2f629-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="2f629-264">Todos los tipos de C# directa o indirectamente se derivan del tipo de clase `object`, y `object` es la clase base definitiva de todos los tipos.</span><span class="sxs-lookup"><span data-stu-id="2f629-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="2f629-265">Los valores de tipos de referencia se tratan como objetos mediante la visualización de los valores como tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="2f629-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="2f629-266">Los valores de los tipos de valor se tratan como objetos realizando operaciones de ***conversión boxing*** y ***unboxing*** .</span><span class="sxs-lookup"><span data-stu-id="2f629-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="2f629-267">En el ejemplo siguiente, un valor `int` se convierte en `object` y vuelve a `int`.</span><span class="sxs-lookup"><span data-stu-id="2f629-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
<span data-ttu-id="2f629-268">Cuando un valor de un tipo de valor se convierte al `object`tipo, se asigna una instancia de objeto, también denominada "box", para contener el valor, y el valor se copia en ese cuadro.</span><span class="sxs-lookup"><span data-stu-id="2f629-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="2f629-269">Por el contrario, cuando una `object` referencia se convierte en un tipo de valor, se realiza una comprobación de que el objeto al que se hace referencia es un cuadro del tipo de valor correcto y, si la comprobación se realiza correctamente, se copia el valor en el cuadro.</span><span class="sxs-lookup"><span data-stu-id="2f629-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="2f629-270">C#el sistema de tipos unificado significa de forma eficaz que los tipos de valor pueden convertirse en objetos "a petición".</span><span class="sxs-lookup"><span data-stu-id="2f629-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="2f629-271">Debido a la unificación, las bibliotecas de uso general que utilizan el tipo `object` pueden usarse con tipos de referencia y tipos de valor.</span><span class="sxs-lookup"><span data-stu-id="2f629-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="2f629-272">Hay varios tipos de ***variables*** en C#, entre otras, campos, elementos de matriz, variables locales y parámetros.</span><span class="sxs-lookup"><span data-stu-id="2f629-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="2f629-273">Las variables representan ubicaciones de almacenamiento, y cada variable tiene un tipo que determina qué valores se pueden almacenar en la variable, como se muestra en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="2f629-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| <span data-ttu-id="2f629-274">__Tipo de variable__</span><span class="sxs-lookup"><span data-stu-id="2f629-274">__Type of Variable__</span></span>    | <span data-ttu-id="2f629-275">__Contenido posible__</span><span class="sxs-lookup"><span data-stu-id="2f629-275">__Possible Contents__</span></span> |
|-------------------------|-----------------------|
| <span data-ttu-id="2f629-276">Tipo de valor distinto a NULL</span><span class="sxs-lookup"><span data-stu-id="2f629-276">Non-nullable value type</span></span> | <span data-ttu-id="2f629-277">Un valor de ese tipo exacto</span><span class="sxs-lookup"><span data-stu-id="2f629-277">A value of that exact type</span></span> |
| <span data-ttu-id="2f629-278">Tipos de valor NULL</span><span class="sxs-lookup"><span data-stu-id="2f629-278">Nullable value type</span></span>     | <span data-ttu-id="2f629-279">Un valor null o un valor de ese tipo exacto</span><span class="sxs-lookup"><span data-stu-id="2f629-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="2f629-280">Una referencia nula, una referencia a un objeto de cualquier tipo de referencia o una referencia a un valor de conversión boxing de cualquier tipo de valor</span><span class="sxs-lookup"><span data-stu-id="2f629-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="2f629-281">Tipo de clase</span><span class="sxs-lookup"><span data-stu-id="2f629-281">Class type</span></span>              | <span data-ttu-id="2f629-282">Una referencia nula, una referencia a una instancia de ese tipo de clase o una referencia a una instancia de una clase derivada de ese tipo de clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="2f629-283">Tipo de interfaz</span><span class="sxs-lookup"><span data-stu-id="2f629-283">Interface type</span></span>          | <span data-ttu-id="2f629-284">Una referencia nula, una referencia a una instancia de un tipo de clase que implementa ese tipo de interfaz o una referencia a un valor de conversión boxing de un tipo de valor que implementa ese tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="2f629-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="2f629-285">Tipo de matriz</span><span class="sxs-lookup"><span data-stu-id="2f629-285">Array type</span></span>              | <span data-ttu-id="2f629-286">Una referencia nula, una referencia a una instancia de ese tipo de matriz o una referencia a una instancia de un tipo de matriz compatible</span><span class="sxs-lookup"><span data-stu-id="2f629-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="2f629-287">Tipo delegado</span><span class="sxs-lookup"><span data-stu-id="2f629-287">Delegate type</span></span>           | <span data-ttu-id="2f629-288">Una referencia nula o una referencia a una instancia de ese tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="2f629-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="2f629-289">Expresiones</span><span class="sxs-lookup"><span data-stu-id="2f629-289">Expressions</span></span>

<span data-ttu-id="2f629-290">Las ***expresiones*** se construyen con ***operandos*** y ***operadores***.</span><span class="sxs-lookup"><span data-stu-id="2f629-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="2f629-291">Los operadores de una expresión indican qué operaciones se aplican a los operandos.</span><span class="sxs-lookup"><span data-stu-id="2f629-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="2f629-292">Ejemplos de operadores incluyen `+`, `-`, `*`, `/` y `new`.</span><span class="sxs-lookup"><span data-stu-id="2f629-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="2f629-293">Algunos ejemplos de operandos son literales, campos, variables locales y expresiones.</span><span class="sxs-lookup"><span data-stu-id="2f629-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="2f629-294">Cuando una expresión contiene varios operadores, la ***precedencia*** de los operadores controla el orden en que se evalúan los operadores individuales.</span><span class="sxs-lookup"><span data-stu-id="2f629-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="2f629-295">Por ejemplo, la expresión `x + y * z` se evalúa como `x + (y * z)` porque el operador `*` tiene mayor precedencia que el operador `+`.</span><span class="sxs-lookup"><span data-stu-id="2f629-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="2f629-296">La mayoría de los operadores se pueden ***sobrecargar***.</span><span class="sxs-lookup"><span data-stu-id="2f629-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="2f629-297">La sobrecarga de operador permite la especificación de implementaciones de operadores definidas por el usuario para operaciones donde uno o ambos operandos son de un tipo de struct o una clase definidos por el usuario.</span><span class="sxs-lookup"><span data-stu-id="2f629-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="2f629-298">En la tabla siguiente se C#resumen los operadores de, que muestran las categorías de operador en orden de prioridad, de mayor a menor.</span><span class="sxs-lookup"><span data-stu-id="2f629-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="2f629-299">Los operadores de la misma categoría tienen la misma precedencia.</span><span class="sxs-lookup"><span data-stu-id="2f629-299">Operators in the same category have equal precedence.</span></span>


| <span data-ttu-id="2f629-300">__Categoría__</span><span class="sxs-lookup"><span data-stu-id="2f629-300">__Category__</span></span>                     | <span data-ttu-id="2f629-301">__Expresión__</span><span class="sxs-lookup"><span data-stu-id="2f629-301">__Expression__</span></span>    | <span data-ttu-id="2f629-302">__Descripción__</span><span class="sxs-lookup"><span data-stu-id="2f629-302">__Description__</span></span> |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="2f629-303">Principal</span><span class="sxs-lookup"><span data-stu-id="2f629-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="2f629-304">Acceso a miembros</span><span class="sxs-lookup"><span data-stu-id="2f629-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="2f629-305">Invocación de método y delegado</span><span class="sxs-lookup"><span data-stu-id="2f629-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="2f629-306">Acceso a matriz e indizador</span><span class="sxs-lookup"><span data-stu-id="2f629-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="2f629-307">Postincremento</span><span class="sxs-lookup"><span data-stu-id="2f629-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="2f629-308">Postdecremento</span><span class="sxs-lookup"><span data-stu-id="2f629-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="2f629-309">Creación de objetos y delegados</span><span class="sxs-lookup"><span data-stu-id="2f629-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="2f629-310">creación de objetos con inicializador</span><span class="sxs-lookup"><span data-stu-id="2f629-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="2f629-311">inicializador de objetos anónimos</span><span class="sxs-lookup"><span data-stu-id="2f629-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="2f629-312">creación de matriz</span><span class="sxs-lookup"><span data-stu-id="2f629-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="2f629-313">obtener el objeto `System.Type` para `T`</span><span class="sxs-lookup"><span data-stu-id="2f629-313">Obtain `System.Type` object for `T`</span></span> |
|                                  | `checked(x)`      | <span data-ttu-id="2f629-314">Evaluar expresión en contexto comprobado</span><span class="sxs-lookup"><span data-stu-id="2f629-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="2f629-315">Evaluar expresión en contexto no comprobado</span><span class="sxs-lookup"><span data-stu-id="2f629-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="2f629-316">obtener valor predeterminado de tipo `T`</span><span class="sxs-lookup"><span data-stu-id="2f629-316">Obtain default value of type `T`</span></span> |
|                                  | `delegate {...}`  | <span data-ttu-id="2f629-317">Función anónima (método anónimo)</span><span class="sxs-lookup"><span data-stu-id="2f629-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="2f629-318">Unario</span><span class="sxs-lookup"><span data-stu-id="2f629-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="2f629-319">identidad</span><span class="sxs-lookup"><span data-stu-id="2f629-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="2f629-320">Negación</span><span class="sxs-lookup"><span data-stu-id="2f629-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="2f629-321">Negación lógica</span><span class="sxs-lookup"><span data-stu-id="2f629-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="2f629-322">Negación bit a bit</span><span class="sxs-lookup"><span data-stu-id="2f629-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="2f629-323">Preincremento</span><span class="sxs-lookup"><span data-stu-id="2f629-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="2f629-324">Predecremento</span><span class="sxs-lookup"><span data-stu-id="2f629-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="2f629-325">convertir explícitamente `x` en el tipo `T`</span><span class="sxs-lookup"><span data-stu-id="2f629-325">Explicitly convert `x` to type `T`</span></span> |
|                                  | `await x`         | <span data-ttu-id="2f629-326">esperar asincrónicamente a que finalice `x`</span><span class="sxs-lookup"><span data-stu-id="2f629-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="2f629-327">Multiplicativo</span><span class="sxs-lookup"><span data-stu-id="2f629-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="2f629-328">Multiplicación</span><span class="sxs-lookup"><span data-stu-id="2f629-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="2f629-329">División</span><span class="sxs-lookup"><span data-stu-id="2f629-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="2f629-330">Resto</span><span class="sxs-lookup"><span data-stu-id="2f629-330">Remainder</span></span> |
| <span data-ttu-id="2f629-331">Aditivo</span><span class="sxs-lookup"><span data-stu-id="2f629-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="2f629-332">Suma, concatenación de cadenas, combinación de delegados</span><span class="sxs-lookup"><span data-stu-id="2f629-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="2f629-333">Resta, eliminación de delegados</span><span class="sxs-lookup"><span data-stu-id="2f629-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="2f629-334">Shift</span><span class="sxs-lookup"><span data-stu-id="2f629-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="2f629-335">Desplazamiento a la izquierda</span><span class="sxs-lookup"><span data-stu-id="2f629-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="2f629-336">Desplazamiento a la derecha</span><span class="sxs-lookup"><span data-stu-id="2f629-336">Shift right</span></span> |
| <span data-ttu-id="2f629-337">Comprobación de tipos y relacional</span><span class="sxs-lookup"><span data-stu-id="2f629-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="2f629-338">Menor que</span><span class="sxs-lookup"><span data-stu-id="2f629-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="2f629-339">Mayor que</span><span class="sxs-lookup"><span data-stu-id="2f629-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="2f629-340">Menor o igual que</span><span class="sxs-lookup"><span data-stu-id="2f629-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="2f629-341">Mayor o igual que</span><span class="sxs-lookup"><span data-stu-id="2f629-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="2f629-342">volver a ejecutar `true` si `x` es una `T`, de lo contrario `false`</span><span class="sxs-lookup"><span data-stu-id="2f629-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="2f629-343">volver a ejecutar `x` con tipo `T`, o `null` si `x` no es una `T`</span><span class="sxs-lookup"><span data-stu-id="2f629-343">Return `x` typed as `T`, or `null` if `x` is not a `T`</span></span> |
| <span data-ttu-id="2f629-344">Igualdad</span><span class="sxs-lookup"><span data-stu-id="2f629-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="2f629-345">Igual</span><span class="sxs-lookup"><span data-stu-id="2f629-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="2f629-346">No igual</span><span class="sxs-lookup"><span data-stu-id="2f629-346">Not equal</span></span> |
| <span data-ttu-id="2f629-347">AND lógico</span><span class="sxs-lookup"><span data-stu-id="2f629-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="2f629-348">AND bit a bit entero, AND lógico booleano</span><span class="sxs-lookup"><span data-stu-id="2f629-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="2f629-349">XOR lógico</span><span class="sxs-lookup"><span data-stu-id="2f629-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="2f629-350">XOR bit a bit entero, XOR lógico booleano</span><span class="sxs-lookup"><span data-stu-id="2f629-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="2f629-351">OR lógico</span><span class="sxs-lookup"><span data-stu-id="2f629-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="2f629-352">OR bit a bit entero, OR lógico booleano</span><span class="sxs-lookup"><span data-stu-id="2f629-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="2f629-353">AND condicional</span><span class="sxs-lookup"><span data-stu-id="2f629-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="2f629-354">Solo evalúa si `x`es `y``true`</span><span class="sxs-lookup"><span data-stu-id="2f629-354">Evaluates `y` only if `x` is `true`</span></span> |
| <span data-ttu-id="2f629-355">OR condicional</span><span class="sxs-lookup"><span data-stu-id="2f629-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="2f629-356">Solo evalúa si `x`es `y``false`</span><span class="sxs-lookup"><span data-stu-id="2f629-356">Evaluates `y` only if `x` is `false`</span></span> |
| <span data-ttu-id="2f629-357">Uso combinado de NULL</span><span class="sxs-lookup"><span data-stu-id="2f629-357">Null coalescing</span></span>                  | `x ?? y`          | <span data-ttu-id="2f629-358">Se evalúa como `y` si `x` es `null`, en `x` caso contrario,.</span><span class="sxs-lookup"><span data-stu-id="2f629-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="2f629-359">Condicional</span><span class="sxs-lookup"><span data-stu-id="2f629-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="2f629-360">se evalúa como `y` si `x` es `true` o `z` si `x` es `false`</span><span class="sxs-lookup"><span data-stu-id="2f629-360">Evaluates `y` if `x` is `true`, `z` if `x` is `false`</span></span> |
| <span data-ttu-id="2f629-361">Asignación o función anónima</span><span class="sxs-lookup"><span data-stu-id="2f629-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="2f629-362">Asignación</span><span class="sxs-lookup"><span data-stu-id="2f629-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="2f629-363">Asignación compuesta; los operadores admitidos son `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=`<code>&#124;=</code></span><span class="sxs-lookup"><span data-stu-id="2f629-363">Compound assignment; supported operators are `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span></span> |
|                                  | `(T x) => y`      | <span data-ttu-id="2f629-364">Función anónima (expresión lambda)</span><span class="sxs-lookup"><span data-stu-id="2f629-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="2f629-365">Instrucciones</span><span class="sxs-lookup"><span data-stu-id="2f629-365">Statements</span></span>

<span data-ttu-id="2f629-366">Las acciones de un programa se expresan mediante ***instrucciones***.</span><span class="sxs-lookup"><span data-stu-id="2f629-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="2f629-367">C# admite varios tipos de instrucciones diferentes, varias de las cuales se definen en términos de instrucciones insertadas.</span><span class="sxs-lookup"><span data-stu-id="2f629-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="2f629-368">Un ***bloque*** permite que se escriban varias instrucciones en contextos donde se permite una única instrucción.</span><span class="sxs-lookup"><span data-stu-id="2f629-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="2f629-369">Un bloque se compone de una lista de instrucciones escritas entre los delimitadores `{` y `}`.</span><span class="sxs-lookup"><span data-stu-id="2f629-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="2f629-370">Las ***instrucciones de declaración*** se usan para declarar variables locales y constantes.</span><span class="sxs-lookup"><span data-stu-id="2f629-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="2f629-371">Las ***instrucciones de expresión*** se usan para evaluar expresiones.</span><span class="sxs-lookup"><span data-stu-id="2f629-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="2f629-372">Entre las expresiones que se pueden utilizar como instrucciones se incluyen las invocaciones de método, las asignaciones `new` de objetos que usan el operador `=` , las asignaciones mediante y los operadores de asignación compuesta, las operaciones de incremento y decremento mediante el `++`operadores `--` y y expresiones Await.</span><span class="sxs-lookup"><span data-stu-id="2f629-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="2f629-373">Las ***instrucciones de selección*** se usan para seleccionar una de varias instrucciones posibles para su ejecución en función del valor de alguna expresión.</span><span class="sxs-lookup"><span data-stu-id="2f629-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="2f629-374">En este grupo están las instrucciones `if` y `switch`.</span><span class="sxs-lookup"><span data-stu-id="2f629-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="2f629-375">Las ***instrucciones de iteración*** se utilizan para ejecutar repetidamente una instrucción incrustada.</span><span class="sxs-lookup"><span data-stu-id="2f629-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="2f629-376">En este grupo están las instrucciones `while`, `do`, `for` y `foreach`.</span><span class="sxs-lookup"><span data-stu-id="2f629-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="2f629-377">Las ***instrucciones de salto*** se usan para transferir el control.</span><span class="sxs-lookup"><span data-stu-id="2f629-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="2f629-378">En este grupo están las instrucciones `break`, `continue`, `goto`, `throw`, `return` y `yield`.</span><span class="sxs-lookup"><span data-stu-id="2f629-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="2f629-379">La instrucción `try`... `catch` se usa para detectar excepciones que se producen durante la ejecución de un bloque, y la instrucción `try`... `finally` se usa para especificar el código de finalización que siempre se ejecuta, tanto si se ha producido una excepción como si no.</span><span class="sxs-lookup"><span data-stu-id="2f629-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="2f629-380">Las `checked` instrucciones `unchecked` y se utilizan para controlar el contexto de comprobación de desbordamiento para conversiones y operaciones aritméticas de tipo entero.</span><span class="sxs-lookup"><span data-stu-id="2f629-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="2f629-381">La instrucción `lock` se usa para obtener el bloqueo de exclusión mutua para un objeto determinado, ejecutar una instrucción y, luego, liberar el bloqueo.</span><span class="sxs-lookup"><span data-stu-id="2f629-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="2f629-382">La instrucción `using` se usa para obtener un recurso, ejecutar una instrucción y, luego, eliminar dicho recurso.</span><span class="sxs-lookup"><span data-stu-id="2f629-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="2f629-383">A continuación se muestran ejemplos de cada tipo de instrucción</span><span class="sxs-lookup"><span data-stu-id="2f629-383">Below are examples of each kind of statement</span></span>

<span data-ttu-id="2f629-384">__Declaraciones de variables locales__</span><span class="sxs-lookup"><span data-stu-id="2f629-384">__Local variable declarations__</span></span>

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


<span data-ttu-id="2f629-385">__Declaración de constante local__</span><span class="sxs-lookup"><span data-stu-id="2f629-385">__Local constant declaration__</span></span>

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


<span data-ttu-id="2f629-386">__Expression (instrucción)__</span><span class="sxs-lookup"><span data-stu-id="2f629-386">__Expression statement__</span></span>

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

<span data-ttu-id="2f629-387">__Instrucción `if`__</span><span class="sxs-lookup"><span data-stu-id="2f629-387">__`if` statement__</span></span>

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


<span data-ttu-id="2f629-388">__Instrucción `switch`__</span><span class="sxs-lookup"><span data-stu-id="2f629-388">__`switch` statement__</span></span>

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

<span data-ttu-id="2f629-389">__Instrucción `while`__</span><span class="sxs-lookup"><span data-stu-id="2f629-389">__`while` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


<span data-ttu-id="2f629-390">__Instrucción `do`__</span><span class="sxs-lookup"><span data-stu-id="2f629-390">__`do` statement__</span></span>

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

<span data-ttu-id="2f629-391">__Instrucción `for`__</span><span class="sxs-lookup"><span data-stu-id="2f629-391">__`for` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="2f629-392">__Instrucción `foreach`__</span><span class="sxs-lookup"><span data-stu-id="2f629-392">__`foreach` statement__</span></span>

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="2f629-393">__Instrucción `break`__</span><span class="sxs-lookup"><span data-stu-id="2f629-393">__`break` statement__</span></span>

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="2f629-394">__Instrucción `continue`__</span><span class="sxs-lookup"><span data-stu-id="2f629-394">__`continue` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="2f629-395">__Instrucción `goto`__</span><span class="sxs-lookup"><span data-stu-id="2f629-395">__`goto` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

<span data-ttu-id="2f629-396">__Instrucción `return`__</span><span class="sxs-lookup"><span data-stu-id="2f629-396">__`return` statement__</span></span>

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

<span data-ttu-id="2f629-397">__Instrucción `yield`__</span><span class="sxs-lookup"><span data-stu-id="2f629-397">__`yield` statement__</span></span>

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

<span data-ttu-id="2f629-398">__`throw`instrucciones `try` y__</span><span class="sxs-lookup"><span data-stu-id="2f629-398">__`throw` and `try` statements__</span></span>

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

<span data-ttu-id="2f629-399">__`checked`instrucciones `unchecked` y__</span><span class="sxs-lookup"><span data-stu-id="2f629-399">__`checked` and `unchecked` statements__</span></span>

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

<span data-ttu-id="2f629-400">__Instrucción `lock`__</span><span class="sxs-lookup"><span data-stu-id="2f629-400">__`lock` statement__</span></span>

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

<span data-ttu-id="2f629-401">__Instrucción `using`__</span><span class="sxs-lookup"><span data-stu-id="2f629-401">__`using` statement__</span></span>

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="2f629-402">Clases y objetos</span><span class="sxs-lookup"><span data-stu-id="2f629-402">Classes and objects</span></span>

<span data-ttu-id="2f629-403">Las ***clases*** son los tipos más fundamentales de C#.</span><span class="sxs-lookup"><span data-stu-id="2f629-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="2f629-404">Una clase es una estructura de datos que combina estados (campos) y acciones (métodos y otros miembros de función) en una sola unidad.</span><span class="sxs-lookup"><span data-stu-id="2f629-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="2f629-405">Una clase proporciona una definición para ***instancias*** creadas dinámicamente de la clase, también conocidas como ***objetos***.</span><span class="sxs-lookup"><span data-stu-id="2f629-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="2f629-406">Las clases admiten ***herencia*** y ***polimorfismo***, mecanismos por los que las ***clases derivadas*** pueden extender y especializar ***clases base***.</span><span class="sxs-lookup"><span data-stu-id="2f629-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="2f629-407">Las clases nuevas se crean mediante declaraciones de clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-407">New classes are created using class declarations.</span></span> <span data-ttu-id="2f629-408">Una declaración de clase se inicia con un encabezado que especifica los atributos y modificadores de la clase, el nombre de la clase, la clase base (si se indica) y las interfaces implementadas por la clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="2f629-409">Al encabezado le sigue el cuerpo de la clase, que consta de una lista de declaraciones de miembros escritas entre los delimitadores `{` y `}`.</span><span class="sxs-lookup"><span data-stu-id="2f629-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="2f629-410">La siguiente es una declaración de una clase simple denominada `Point`:</span><span class="sxs-lookup"><span data-stu-id="2f629-410">The following is a declaration of a simple class named `Point`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="2f629-411">Las instancias de clases se crean mediante el operador `new`, que asigna memoria para una nueva instancia, invoca un constructor para inicializar la instancia y devuelve una referencia a la instancia.</span><span class="sxs-lookup"><span data-stu-id="2f629-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="2f629-412">Las instrucciones siguientes crean dos `Point` objetos y almacenan las referencias a esos objetos en dos variables:</span><span class="sxs-lookup"><span data-stu-id="2f629-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```csharp
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="2f629-413">La memoria ocupada por un objeto se recupera automáticamente cuando el objeto ya no está en uso.</span><span class="sxs-lookup"><span data-stu-id="2f629-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="2f629-414">No es necesario ni posible desasignar explícitamente objetos en C#.</span><span class="sxs-lookup"><span data-stu-id="2f629-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="2f629-415">Miembros</span><span class="sxs-lookup"><span data-stu-id="2f629-415">Members</span></span>

<span data-ttu-id="2f629-416">Los miembros de una clase son ***miembros estáticos*** o ***miembros de instancia***.</span><span class="sxs-lookup"><span data-stu-id="2f629-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="2f629-417">Los miembros estáticos pertenecen a clases y los miembros de instancia pertenecen a objetos (instancias de clases).</span><span class="sxs-lookup"><span data-stu-id="2f629-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="2f629-418">En la tabla siguiente se proporciona información general sobre los tipos de miembros que puede contener una clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| <span data-ttu-id="2f629-419">__Miembro__</span><span class="sxs-lookup"><span data-stu-id="2f629-419">__Member__</span></span>   | <span data-ttu-id="2f629-420">__Descripción__</span><span class="sxs-lookup"><span data-stu-id="2f629-420">__Description__</span></span> |
|------------  |-----------------|
| <span data-ttu-id="2f629-421">Constantes</span><span class="sxs-lookup"><span data-stu-id="2f629-421">Constants</span></span>    | <span data-ttu-id="2f629-422">Valores constantes asociados a la clase</span><span class="sxs-lookup"><span data-stu-id="2f629-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="2f629-423">Campos</span><span class="sxs-lookup"><span data-stu-id="2f629-423">Fields</span></span>       | <span data-ttu-id="2f629-424">Variables de la clase</span><span class="sxs-lookup"><span data-stu-id="2f629-424">Variables of the class</span></span> |
| <span data-ttu-id="2f629-425">Métodos</span><span class="sxs-lookup"><span data-stu-id="2f629-425">Methods</span></span>      | <span data-ttu-id="2f629-426">Cálculos y acciones que pueden realizarse mediante la clase</span><span class="sxs-lookup"><span data-stu-id="2f629-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="2f629-427">Propiedades</span><span class="sxs-lookup"><span data-stu-id="2f629-427">Properties</span></span>   | <span data-ttu-id="2f629-428">Acciones asociadas a la lectura y escritura de propiedades con nombre de la clase</span><span class="sxs-lookup"><span data-stu-id="2f629-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="2f629-429">Indizadores</span><span class="sxs-lookup"><span data-stu-id="2f629-429">Indexers</span></span>     | <span data-ttu-id="2f629-430">Acciones asociadas a la indexación de instancias de la clase como una matriz</span><span class="sxs-lookup"><span data-stu-id="2f629-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="2f629-431">Eventos</span><span class="sxs-lookup"><span data-stu-id="2f629-431">Events</span></span>       | <span data-ttu-id="2f629-432">Notificaciones que puede generar la clase</span><span class="sxs-lookup"><span data-stu-id="2f629-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="2f629-433">Operadores</span><span class="sxs-lookup"><span data-stu-id="2f629-433">Operators</span></span>    | <span data-ttu-id="2f629-434">Conversiones y operadores de expresión admitidos por la clase</span><span class="sxs-lookup"><span data-stu-id="2f629-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="2f629-435">Constructores</span><span class="sxs-lookup"><span data-stu-id="2f629-435">Constructors</span></span> | <span data-ttu-id="2f629-436">Acciones necesarias para inicializar instancias de la clase o la clase propiamente dicha</span><span class="sxs-lookup"><span data-stu-id="2f629-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="2f629-437">Destructores</span><span class="sxs-lookup"><span data-stu-id="2f629-437">Destructors</span></span>  | <span data-ttu-id="2f629-438">Acciones que deben realizarse antes de que las instancias de la clase se descarten de forma permanente</span><span class="sxs-lookup"><span data-stu-id="2f629-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="2f629-439">Tipos</span><span class="sxs-lookup"><span data-stu-id="2f629-439">Types</span></span>        | <span data-ttu-id="2f629-440">Tipos anidados declarados por la clase</span><span class="sxs-lookup"><span data-stu-id="2f629-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="2f629-441">Accesibilidad</span><span class="sxs-lookup"><span data-stu-id="2f629-441">Accessibility</span></span>

<span data-ttu-id="2f629-442">Cada miembro de una clase tiene asociada una accesibilidad, que controla las regiones del texto del programa que pueden tener acceso al miembro.</span><span class="sxs-lookup"><span data-stu-id="2f629-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="2f629-443">Existen cinco formas posibles de accesibilidad.</span><span class="sxs-lookup"><span data-stu-id="2f629-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="2f629-444">Se resumen en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="2f629-444">These are summarized in the following table.</span></span>


| <span data-ttu-id="2f629-445">__Accesibilidad__</span><span class="sxs-lookup"><span data-stu-id="2f629-445">__Accessibility__</span></span>    | <span data-ttu-id="2f629-446">__Al__</span><span class="sxs-lookup"><span data-stu-id="2f629-446">__Meaning__</span></span> |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="2f629-447">Acceso no limitado</span><span class="sxs-lookup"><span data-stu-id="2f629-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="2f629-448">Acceso limitado a esta clase o a las clases derivadas de esta clase</span><span class="sxs-lookup"><span data-stu-id="2f629-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="2f629-449">Acceso limitado a este programa</span><span class="sxs-lookup"><span data-stu-id="2f629-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="2f629-450">Acceso limitado a este programa o a las clases derivadas de esta clase</span><span class="sxs-lookup"><span data-stu-id="2f629-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="2f629-451">Acceso limitado a esta clase</span><span class="sxs-lookup"><span data-stu-id="2f629-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="2f629-452">Parámetros de tipo</span><span class="sxs-lookup"><span data-stu-id="2f629-452">Type parameters</span></span>

<span data-ttu-id="2f629-453">Una definición de clase puede especificar un conjunto de parámetros de tipo poniendo tras el nombre de clase una lista de nombres de parámetro de tipo entre corchetes angulares.</span><span class="sxs-lookup"><span data-stu-id="2f629-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="2f629-454">Los parámetros de tipo se pueden usar en el cuerpo de las declaraciones de clase para definir los miembros de la clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="2f629-455">En el ejemplo siguiente, los parámetros de tipo de `Pair` son `TFirst` y `TSecond`:</span><span class="sxs-lookup"><span data-stu-id="2f629-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="2f629-456">Un tipo de clase que se declara para tomar parámetros de tipo se denomina tipo de clase genérico.</span><span class="sxs-lookup"><span data-stu-id="2f629-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="2f629-457">Los tipos struct, interfaz y delegado también pueden ser genéricos.</span><span class="sxs-lookup"><span data-stu-id="2f629-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="2f629-458">Cuando se usa la clase genérica, se deben proporcionar argumentos de tipo para cada uno de los parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="2f629-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="2f629-459">Un tipo genérico con argumentos de tipo proporcionados `Pair<int,string>` , como el anterior, se denomina tipo construido.</span><span class="sxs-lookup"><span data-stu-id="2f629-459">A generic type with type arguments provided, like `Pair<int,string>` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="2f629-460">Clases base</span><span class="sxs-lookup"><span data-stu-id="2f629-460">Base classes</span></span>

<span data-ttu-id="2f629-461">Una declaración de clase puede especificar una clase base colocando después del nombre de clase y los parámetros de tipo dos puntos seguidos del nombre de la clase base.</span><span class="sxs-lookup"><span data-stu-id="2f629-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="2f629-462">Omitir una especificación de la clase base es igual que derivarla del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="2f629-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="2f629-463">En el ejemplo siguiente, la clase base de `Point3D` es `Point` y la clase base de `Point` es `object`:</span><span class="sxs-lookup"><span data-stu-id="2f629-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
<span data-ttu-id="2f629-464">Una clase hereda a los miembros de su clase base.</span><span class="sxs-lookup"><span data-stu-id="2f629-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="2f629-465">La herencia significa que una clase contiene implícitamente todos los miembros de su clase base, excepto la instancia y constructores estáticos, y los destructores de la clase base.</span><span class="sxs-lookup"><span data-stu-id="2f629-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="2f629-466">Una clase derivada puede agregar nuevos miembros a aquellos de los que hereda, pero no puede quitar la definición de un miembro heredado.</span><span class="sxs-lookup"><span data-stu-id="2f629-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="2f629-467">En el ejemplo anterior, `Point3D` hereda los campos `x` y `y` de `Point` y cada instancia de `Point3D` contiene tres campos: `x`, `y` y `z`.</span><span class="sxs-lookup"><span data-stu-id="2f629-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="2f629-468">Existe una conversión implícita de un tipo de clase a cualquiera de sus tipos de clase base.</span><span class="sxs-lookup"><span data-stu-id="2f629-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="2f629-469">Por lo tanto, una variable de un tipo de clase puede hacer referencia a una instancia de esa clase o a una instancia de cualquier clase derivada.</span><span class="sxs-lookup"><span data-stu-id="2f629-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="2f629-470">Por ejemplo, dadas las declaraciones de clase anteriores, una variable de tipo `Point` puede hacer referencia a una instancia de `Point` o `Point3D`:</span><span class="sxs-lookup"><span data-stu-id="2f629-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="2f629-471">Fields</span><span class="sxs-lookup"><span data-stu-id="2f629-471">Fields</span></span>

<span data-ttu-id="2f629-472">Un campo es una variable que está asociada a una clase o a una instancia de una clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="2f629-473">Un campo declarado con el `static` modificador define un ***campo estático***.</span><span class="sxs-lookup"><span data-stu-id="2f629-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="2f629-474">Un campo estático identifica exactamente una ubicación de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="2f629-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="2f629-475">Independientemente del número de instancias de una clase que se creen, siempre solo hay una copia de un campo estático.</span><span class="sxs-lookup"><span data-stu-id="2f629-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="2f629-476">Un campo declarado sin el `static` modificador define un ***campo de instancia***.</span><span class="sxs-lookup"><span data-stu-id="2f629-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="2f629-477">Cada instancia de una clase contiene una copia independiente de todos los campos de instancia de esa clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="2f629-478">En el ejemplo siguiente, cada instancia de la clase `Color` tiene una copia independiente de los campos de instancia `r`, `g` y `b`, pero solo hay una copia de los campos estáticos `Black`, `White`, `Red`, `Green` y `Blue`:</span><span class="sxs-lookup"><span data-stu-id="2f629-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
<span data-ttu-id="2f629-479">Como se muestra en el ejemplo anterior, los ***campos de solo lectura*** se puede declarar con un modificador `readonly`.</span><span class="sxs-lookup"><span data-stu-id="2f629-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="2f629-480">La asignación a `readonly` un campo solo se puede producir como parte de la declaración del campo o en un constructor de la misma clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="2f629-481">Métodos</span><span class="sxs-lookup"><span data-stu-id="2f629-481">Methods</span></span>

<span data-ttu-id="2f629-482">Un ***método*** es un miembro que implementa un cálculo o una acción que puede realizar un objeto o una clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="2f629-483">A los ***métodos estáticos*** se accede a través de la clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="2f629-484">A los ***métodos de instancia*** se accede a través de instancias de la clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="2f629-485">Los métodos tienen una lista (posiblemente vacía) de ***parámetros***, que representan valores o referencias de variables que se pasan al método, y un tipo de valor ***devuelto***, que especifica el tipo del valor calculado y devuelto por el método.</span><span class="sxs-lookup"><span data-stu-id="2f629-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="2f629-486">El tipo de valor devuelto `void` de un método es si no devuelve un valor.</span><span class="sxs-lookup"><span data-stu-id="2f629-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="2f629-487">Al igual que los tipos, los métodos también pueden tener un conjunto de parámetros de tipo, para lo cuales se deben especificar argumentos de tipo cuando se llama al método.</span><span class="sxs-lookup"><span data-stu-id="2f629-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="2f629-488">A diferencia de los tipos, los argumentos de tipo a menudo se pueden deducir de los argumentos de una llamada al método y no es necesario proporcionarlos explícitamente.</span><span class="sxs-lookup"><span data-stu-id="2f629-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="2f629-489">La ***signatura*** de un método debe ser única en la clase en la que se declara el método.</span><span class="sxs-lookup"><span data-stu-id="2f629-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="2f629-490">La signatura de un método se compone del nombre del método, el número de parámetros de tipo y el número, los modificadores y los tipos de sus parámetros.</span><span class="sxs-lookup"><span data-stu-id="2f629-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="2f629-491">La signatura de un método no incluye el tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="2f629-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="2f629-492">Parámetros</span><span class="sxs-lookup"><span data-stu-id="2f629-492">Parameters</span></span>

<span data-ttu-id="2f629-493">Los parámetros se usan para pasar valores o referencias a variables a métodos.</span><span class="sxs-lookup"><span data-stu-id="2f629-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="2f629-494">Los parámetros de un método obtienen sus valores reales de los ***argumentos*** que se especifican cuando se invoca el método.</span><span class="sxs-lookup"><span data-stu-id="2f629-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="2f629-495">Hay cuatro tipos de parámetros: parámetros de valor, parámetros de referencia, parámetros de salida y matrices de parámetros.</span><span class="sxs-lookup"><span data-stu-id="2f629-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="2f629-496">Un ***parámetro de valor*** se usa para el paso de parámetros de entrada.</span><span class="sxs-lookup"><span data-stu-id="2f629-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="2f629-497">Un parámetro de valor corresponde a una variable local que obtiene su valor inicial del argumento que se ha pasado para el parámetro.</span><span class="sxs-lookup"><span data-stu-id="2f629-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="2f629-498">Las modificaciones en un parámetro de valor no afectan el argumento que se pasa para el parámetro.</span><span class="sxs-lookup"><span data-stu-id="2f629-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="2f629-499">Los parámetros de valor pueden ser opcionales; se especifica un valor predeterminado para que se puedan omitir los argumentos correspondientes.</span><span class="sxs-lookup"><span data-stu-id="2f629-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="2f629-500">Un ***parámetro de referencia*** se usa para el paso de parámetros de entrada y salida.</span><span class="sxs-lookup"><span data-stu-id="2f629-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="2f629-501">El argumento pasado para un parámetro de referencia debe ser una variable, y durante la ejecución del método, el parámetro de referencia representa la misma ubicación de almacenamiento que la variable del argumento.</span><span class="sxs-lookup"><span data-stu-id="2f629-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="2f629-502">Un parámetro de referencia se declara con el modificador `ref`.</span><span class="sxs-lookup"><span data-stu-id="2f629-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="2f629-503">En el ejemplo siguiente se muestra el uso de parámetros `ref`.</span><span class="sxs-lookup"><span data-stu-id="2f629-503">The following example shows the use of `ref` parameters.</span></span>

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
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
<span data-ttu-id="2f629-504">Un ***parámetro de salida*** se usa para pasar parámetros de salida.</span><span class="sxs-lookup"><span data-stu-id="2f629-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="2f629-505">Un parámetro de salida es similar a un parámetro de referencia, salvo que el valor inicial del argumento proporcionado por el llamador no es importante.</span><span class="sxs-lookup"><span data-stu-id="2f629-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="2f629-506">Un parámetro de salida se declara con el modificador `out`.</span><span class="sxs-lookup"><span data-stu-id="2f629-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="2f629-507">En el ejemplo siguiente se muestra el uso de parámetros `out`.</span><span class="sxs-lookup"><span data-stu-id="2f629-507">The following example shows the use of `out` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
<span data-ttu-id="2f629-508">Una ***matriz de parámetros*** permite que se pasen a un método un número variable de argumentos.</span><span class="sxs-lookup"><span data-stu-id="2f629-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="2f629-509">Una matriz de parámetros se declara con el modificador `params`.</span><span class="sxs-lookup"><span data-stu-id="2f629-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="2f629-510">Solo el último parámetro de un método puede ser una matriz de parámetros y el tipo de una matriz de parámetros debe ser un tipo de matriz unidimensional.</span><span class="sxs-lookup"><span data-stu-id="2f629-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="2f629-511">Los `Write` métodos `WriteLine` y de la `System.Console` clase son buenos ejemplos de uso de la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="2f629-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="2f629-512">Se declaran de la manera siguiente.</span><span class="sxs-lookup"><span data-stu-id="2f629-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="2f629-513">Dentro de un método que usa una matriz de parámetros, la matriz de parámetros se comporta exactamente igual que un parámetro normal de un tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="2f629-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="2f629-514">Sin embargo, en una invocación de un método con una matriz de parámetros, es posible pasar un único argumento del tipo de matriz de parámetros o cualquier número de argumentos del tipo de elemento de la matriz de parámetros.</span><span class="sxs-lookup"><span data-stu-id="2f629-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="2f629-515">En este caso, una instancia de matriz se e inicializa automáticamente con los argumentos dados.</span><span class="sxs-lookup"><span data-stu-id="2f629-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="2f629-516">Este ejemplo</span><span class="sxs-lookup"><span data-stu-id="2f629-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="2f629-517">es equivalente a escribir lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="2f629-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="2f629-518">Cuerpo del método y variables locales</span><span class="sxs-lookup"><span data-stu-id="2f629-518">Method body and local variables</span></span>

<span data-ttu-id="2f629-519">El cuerpo de un método especifica las instrucciones que se ejecutan cuando se invoca el método.</span><span class="sxs-lookup"><span data-stu-id="2f629-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="2f629-520">Un cuerpo del método puede declarar variables que son específicas de la invocación del método.</span><span class="sxs-lookup"><span data-stu-id="2f629-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="2f629-521">Estas variables se denominan ***variables locales***.</span><span class="sxs-lookup"><span data-stu-id="2f629-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="2f629-522">Una declaración de variable local especifica un nombre de tipo, un nombre de variable y, posiblemente, un valor inicial.</span><span class="sxs-lookup"><span data-stu-id="2f629-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="2f629-523">En el ejemplo siguiente se declara una variable local `i` con un valor inicial de cero y una variable local `j` sin ningún valor inicial.</span><span class="sxs-lookup"><span data-stu-id="2f629-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
<span data-ttu-id="2f629-524">C# requiere que se ***asigne definitivamente*** una variable local antes de que se pueda obtener su valor.</span><span class="sxs-lookup"><span data-stu-id="2f629-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="2f629-525">Por ejemplo, si la declaración de `i` anterior no incluyera un valor inicial, el compilador notificaría un error con los usos posteriores de `i` porque `i` no se asignaría definitivamente en esos puntos del programa.</span><span class="sxs-lookup"><span data-stu-id="2f629-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="2f629-526">Puede usar una instrucción `return` para devolver el control a su llamador.</span><span class="sxs-lookup"><span data-stu-id="2f629-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="2f629-527">En un método que devuelve `void`, las instrucciones `return` no pueden especificar una expresión.</span><span class="sxs-lookup"><span data-stu-id="2f629-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="2f629-528">En un método que devuelve instrucciones`void`no `return` , debe incluir una expresión que calcule el valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="2f629-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="2f629-529">Métodos estáticos y de instancia</span><span class="sxs-lookup"><span data-stu-id="2f629-529">Static and instance methods</span></span>

<span data-ttu-id="2f629-530">Un método declarado con un `static` modificador es un ***método estático***.</span><span class="sxs-lookup"><span data-stu-id="2f629-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="2f629-531">Un método estático no opera en una instancia específica y solo puede acceder directamente a miembros estáticos.</span><span class="sxs-lookup"><span data-stu-id="2f629-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="2f629-532">Un método declarado sin un `static` modificador es un ***método de instancia***.</span><span class="sxs-lookup"><span data-stu-id="2f629-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="2f629-533">Un método de instancia opera en una instancia específica y puede acceder a miembros estáticos y de instancia.</span><span class="sxs-lookup"><span data-stu-id="2f629-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="2f629-534">Se puede acceder explícitamente a la instancia en la que se invoca un método de instancia como `this`.</span><span class="sxs-lookup"><span data-stu-id="2f629-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="2f629-535">Es un error hacer referencia a `this` en un método estático.</span><span class="sxs-lookup"><span data-stu-id="2f629-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="2f629-536">La siguiente clase `Entity` tiene miembros estáticos y de instancia.</span><span class="sxs-lookup"><span data-stu-id="2f629-536">The following `Entity` class has both static and instance members.</span></span>

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
<span data-ttu-id="2f629-537">Cada instancia `Entity` contiene un número de serie (y probablemente alguna otra información que no se muestra aquí).</span><span class="sxs-lookup"><span data-stu-id="2f629-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="2f629-538">El constructor `Entity` (que es como un método de instancia) inicializa la nueva instancia con el siguiente número de serie disponible.</span><span class="sxs-lookup"><span data-stu-id="2f629-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="2f629-539">Dado que el constructor es un miembro de instancia, se le permite acceder al campo de instancia `serialNo` y al campo estático `nextSerialNo`.</span><span class="sxs-lookup"><span data-stu-id="2f629-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="2f629-540">Los métodos estáticos `GetNextSerialNo` y `SetNextSerialNo` pueden acceder al campo estático `nextSerialNo`, pero sería un error para ellas acceder directamente al campo de instancia `serialNo`.</span><span class="sxs-lookup"><span data-stu-id="2f629-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="2f629-541">En el ejemplo siguiente se muestra el uso `Entity` de la clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-541">The following example shows the use of the `Entity` class.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
<span data-ttu-id="2f629-542">Tenga en cuenta que los métodos estáticos `SetNextSerialNo` y `GetNextSerialNo` se invocan en la clase, mientras que el método de instancia `GetSerialNo` se invoca en instancias de la clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="2f629-543">Métodos virtual, de reemplazo y abstracto</span><span class="sxs-lookup"><span data-stu-id="2f629-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="2f629-544">Cuando una declaración de método de instancia incluye un modificador `virtual`, se dice que el método es un ***método virtual***.</span><span class="sxs-lookup"><span data-stu-id="2f629-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="2f629-545">Cuando no `virtual` existe ningún modificador, se dice que el método es un ***método no virtual***.</span><span class="sxs-lookup"><span data-stu-id="2f629-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="2f629-546">Cuando se invoca un método virtual, el ***tipo en tiempo de ejecución*** de la instancia para la que tiene lugar esa invocación determina la implementación del método real que se invocará.</span><span class="sxs-lookup"><span data-stu-id="2f629-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="2f629-547">En una invocación de método no virtual, el ***tipo en tiempo de compilación*** de la instancia es el factor determinante.</span><span class="sxs-lookup"><span data-stu-id="2f629-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="2f629-548">Un método virtual puede ser ***reemplazado*** en una clase derivada.</span><span class="sxs-lookup"><span data-stu-id="2f629-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="2f629-549">Cuando una declaración de método de instancia `override` incluye un modificador, el método invalida un método virtual heredado con la misma firma.</span><span class="sxs-lookup"><span data-stu-id="2f629-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="2f629-550">Mientras que una declaración de método virtual introduce un método nuevo, una declaración de método de reemplazo especializa un método virtual heredado existente proporcionando una nueva implementación de ese método.</span><span class="sxs-lookup"><span data-stu-id="2f629-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="2f629-551">Un método ***abstracto*** es un método virtual sin implementación.</span><span class="sxs-lookup"><span data-stu-id="2f629-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="2f629-552">Un método abstracto se declara con `abstract` el modificador y solo se permite en una clase que también se `abstract`declara.</span><span class="sxs-lookup"><span data-stu-id="2f629-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="2f629-553">Un método abstracto debe reemplazarse en todas las clases derivadas no abstractas.</span><span class="sxs-lookup"><span data-stu-id="2f629-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="2f629-554">En el ejemplo siguiente se declara una clase abstracta, `Expression`, que representa un nodo de árbol de expresión y tres clases derivadas, `Constant`, `VariableReference` y `Operation`, que implementan nodos de árbol de expresión para constantes, referencias a variables y operaciones aritméticas.</span><span class="sxs-lookup"><span data-stu-id="2f629-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="2f629-555">(Esto es similar a, pero no debe confundirse con los tipos de árbol de expresión introducidos en los [tipos de árbol de expresión](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="2f629-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
<span data-ttu-id="2f629-556">Las cuatro clases anteriores se pueden usar para modelar expresiones aritméticas.</span><span class="sxs-lookup"><span data-stu-id="2f629-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="2f629-557">Por ejemplo, usando instancias de estas clases, la expresión `x + 3` se puede representar de la manera siguiente.</span><span class="sxs-lookup"><span data-stu-id="2f629-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="2f629-558">El método `Evaluate` de una instancia `Expression` se invoca para evaluar la expresión determinada y generar un valor `double`.</span><span class="sxs-lookup"><span data-stu-id="2f629-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="2f629-559">El método toma como argumento un `Hashtable` que contiene nombres de variables (como claves de las entradas) y valores (como valores de las entradas).</span><span class="sxs-lookup"><span data-stu-id="2f629-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="2f629-560">El `Evaluate` método es un método abstracto virtual, lo que significa que las clases derivadas no abstractas deben invalidarlo para proporcionar una implementación real.</span><span class="sxs-lookup"><span data-stu-id="2f629-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="2f629-561">Una implementación de `Constant` de `Evaluate` simplemente devuelve la constante almacenada.</span><span class="sxs-lookup"><span data-stu-id="2f629-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="2f629-562">Una `VariableReference`implementación de busca el nombre de la variable en la tabla hash y devuelve el valor resultante.</span><span class="sxs-lookup"><span data-stu-id="2f629-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="2f629-563">Una implementación de `Operation` evalúa primero los operandos izquierdo y derecho (mediante la invocación recursiva de sus métodos `Evaluate`) y luego realiza la operación aritmética correspondiente.</span><span class="sxs-lookup"><span data-stu-id="2f629-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="2f629-564">El siguiente programa usa las clases `Expression` para evaluar la expresión `x * (y + 2)` para los distintos valores de `x` y `y`.</span><span class="sxs-lookup"><span data-stu-id="2f629-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a><span data-ttu-id="2f629-565">Sobrecarga de métodos</span><span class="sxs-lookup"><span data-stu-id="2f629-565">Method overloading</span></span>

<span data-ttu-id="2f629-566">La ***sobrecarga*** de métodos permite que varios métodos de la misma clase tengan el mismo nombre mientras tengan signaturas únicas.</span><span class="sxs-lookup"><span data-stu-id="2f629-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="2f629-567">Al compilar una invocación de un método sobrecargado, el compilador usa la ***resolución de sobrecarga*** para determinar el método concreto que de invocará.</span><span class="sxs-lookup"><span data-stu-id="2f629-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="2f629-568">La resolución de sobrecarga busca el método que mejor coincida con los argumentos o informa de un error si no se puede encontrar ninguna mejor coincidencia.</span><span class="sxs-lookup"><span data-stu-id="2f629-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="2f629-569">En el ejemplo siguiente se muestra la resolución de sobrecarga en vigor.</span><span class="sxs-lookup"><span data-stu-id="2f629-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="2f629-570">El comentario para cada invocación del método `Main` muestra qué método se invoca realmente.</span><span class="sxs-lookup"><span data-stu-id="2f629-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
<span data-ttu-id="2f629-571">Tal como se muestra en el ejemplo, un método determinado siempre se puede seleccionar mediante la conversión explícita de los argumentos en los tipos de parámetros exactos o el suministro explícito de los argumentos de tipo.</span><span class="sxs-lookup"><span data-stu-id="2f629-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="2f629-572">Otros miembros de función</span><span class="sxs-lookup"><span data-stu-id="2f629-572">Other function members</span></span>

<span data-ttu-id="2f629-573">Los miembros que contienen código ejecutable se conocen colectivamente como ***miembros de función*** de una clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="2f629-574">En la sección anterior se han descrito métodos, que son el tipo principal de los miembros de función.</span><span class="sxs-lookup"><span data-stu-id="2f629-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="2f629-575">En esta sección se describen los demás tipos de miembros de C#función admitidos por: constructores, propiedades, indexadores, eventos, operadores y destructores.</span><span class="sxs-lookup"><span data-stu-id="2f629-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="2f629-576">En el código siguiente se muestra una clase `List<T>`genérica denominada, que implementa una lista de objetos que se va a ampliar.</span><span class="sxs-lookup"><span data-stu-id="2f629-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="2f629-577">La clase contiene varios ejemplos de los tipos más comunes de miembros de función.</span><span class="sxs-lookup"><span data-stu-id="2f629-577">The class contains several examples of the most common kinds of function members.</span></span>


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a><span data-ttu-id="2f629-578">Constructores</span><span class="sxs-lookup"><span data-stu-id="2f629-578">Constructors</span></span>

<span data-ttu-id="2f629-579">C# admite constructores de instancia y estáticos.</span><span class="sxs-lookup"><span data-stu-id="2f629-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="2f629-580">Un ***constructor de instancia*** es un miembro que implementa las acciones necesarias para inicializar una instancia de una clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="2f629-581">Un ***constructor estático*** es un miembro que implementa las acciones necesarias para inicializar una clase en sí misma cuando se carga por primera vez.</span><span class="sxs-lookup"><span data-stu-id="2f629-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="2f629-582">Un constructor se declara como un método sin ningún tipo de valor devuelto y el mismo nombre que la clase contenedora.</span><span class="sxs-lookup"><span data-stu-id="2f629-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="2f629-583">Si una declaración de constructor incluye `static` un modificador, declara un constructor estático.</span><span class="sxs-lookup"><span data-stu-id="2f629-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="2f629-584">De lo contrario, declara un constructor de instancia.</span><span class="sxs-lookup"><span data-stu-id="2f629-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="2f629-585">Los constructores de instancias se pueden sobrecargar.</span><span class="sxs-lookup"><span data-stu-id="2f629-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="2f629-586">Por ejemplo, la clase `List<T>` declara dos constructores de instancia, una sin parámetros y otra que toma un parámetro `int`.</span><span class="sxs-lookup"><span data-stu-id="2f629-586">For example, the `List<T>` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="2f629-587">Los constructores de instancia se invocan mediante el operador `new`.</span><span class="sxs-lookup"><span data-stu-id="2f629-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="2f629-588">Las instrucciones siguientes asignan `List<string>` dos instancias de mediante cada uno de los constructores de la `List` clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-588">The following statements allocate two `List<string>` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="2f629-589">A diferencia de otros miembros, los constructores de instancia no se heredan y una clase no tiene ningún constructor de instancia que no sea el que se declara realmente en la clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="2f629-590">Si no se proporciona ningún constructor de instancia para una clase, se proporciona automáticamente uno vacío sin ningún parámetro.</span><span class="sxs-lookup"><span data-stu-id="2f629-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="2f629-591">Propiedades</span><span class="sxs-lookup"><span data-stu-id="2f629-591">Properties</span></span>

<span data-ttu-id="2f629-592">Las ***propiedades*** son una extensión natural de los campos.</span><span class="sxs-lookup"><span data-stu-id="2f629-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="2f629-593">Ambos son miembros con nombre con tipos asociados y la sintaxis para acceder a los campos y las propiedades es la misma.</span><span class="sxs-lookup"><span data-stu-id="2f629-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="2f629-594">Sin embargo, a diferencia de los campos, las propiedades no denotan ubicaciones de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="2f629-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="2f629-595">Las propiedades tienen ***descriptores de acceso*** que especifican las instrucciones que se ejecutan cuando se leen o escriben sus valores.</span><span class="sxs-lookup"><span data-stu-id="2f629-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="2f629-596">Una propiedad se declara como un campo, salvo que la declaración finaliza con un `get` descriptor de acceso `set` o un descriptor de acceso `{` escrito `}` entre los delimitadores y en lugar de terminar en un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="2f629-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="2f629-597">Una propiedad que tiene tanto un `get` descriptor `set` de acceso como un descriptor de acceso es una propiedad ***de lectura y escritura***, una propiedad que solo tiene un `get` descriptor de acceso es una ***propiedad de solo lectura***y una propiedad que solo tiene un `set` descriptor de acceso es un ***propiedad de solo escritura***.</span><span class="sxs-lookup"><span data-stu-id="2f629-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="2f629-598">Un `get` descriptor de acceso corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad.</span><span class="sxs-lookup"><span data-stu-id="2f629-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="2f629-599">Excepto como destino de una asignación, cuando se hace referencia a una propiedad en una expresión, el `get` descriptor de acceso de la propiedad se invoca para calcular el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="2f629-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="2f629-600">Un `set` descriptor de acceso corresponde a un método con `value` un solo parámetro denominado y ningún tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="2f629-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="2f629-601">Cuando se hace referencia a una propiedad como el destino de una asignación o como el operando `--`de o `set` , el descriptor de `++` acceso se invoca con un argumento que proporciona el nuevo valor.</span><span class="sxs-lookup"><span data-stu-id="2f629-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="2f629-602">La clase `List<T>` declara dos propiedades, `Count` y `Capacity`, que son de solo lectura y de lectura y escritura, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="2f629-602">The `List<T>` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="2f629-603">El siguiente es un ejemplo de uso de estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="2f629-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="2f629-604">De forma similar a los campos y métodos, C# admite propiedades de instancia y propiedades estáticas.</span><span class="sxs-lookup"><span data-stu-id="2f629-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="2f629-605">Las propiedades estáticas se declaran con el `static` modificador y las propiedades de instancia se declaran sin ella.</span><span class="sxs-lookup"><span data-stu-id="2f629-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="2f629-606">Los descriptores de acceso de una propiedad pueden ser virtuales.</span><span class="sxs-lookup"><span data-stu-id="2f629-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="2f629-607">Cuando una declaración de propiedad incluye un modificador `virtual`, `abstract` o `override`, se aplica a los descriptores de acceso de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="2f629-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="2f629-608">Indizadores</span><span class="sxs-lookup"><span data-stu-id="2f629-608">Indexers</span></span>

<span data-ttu-id="2f629-609">Un ***indexador*** es un miembro que permite indexar de la misma manera que una matriz.</span><span class="sxs-lookup"><span data-stu-id="2f629-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="2f629-610">Un indexador se declara como una propiedad, excepto por el hecho que el nombre del miembro es `this`, seguido por una lista de parámetros que se escriben entre los delimitadores `[` y `]`.</span><span class="sxs-lookup"><span data-stu-id="2f629-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="2f629-611">Los parámetros están disponibles en los descriptores de acceso del indexador.</span><span class="sxs-lookup"><span data-stu-id="2f629-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="2f629-612">De forma similar a las propiedades, los indexadores pueden ser lectura y escritura, de solo lectura y de solo escritura, y los descriptores de acceso de un indexador pueden ser virtuales.</span><span class="sxs-lookup"><span data-stu-id="2f629-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="2f629-613">La clase `List` declara un único indexador de lectura y escritura que toma un parámetro `int`.</span><span class="sxs-lookup"><span data-stu-id="2f629-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="2f629-614">El indexador permite indexar instancias de `List` con valores `int`.</span><span class="sxs-lookup"><span data-stu-id="2f629-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="2f629-615">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="2f629-615">For example</span></span>

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
<span data-ttu-id="2f629-616">Los indexadores se pueden sobrecargar, lo que significa que una clase puede declarar varios indexadores siempre y cuando el número o los tipos de sus parámetros sean diferentes.</span><span class="sxs-lookup"><span data-stu-id="2f629-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="2f629-617">Eventos</span><span class="sxs-lookup"><span data-stu-id="2f629-617">Events</span></span>

<span data-ttu-id="2f629-618">Un ***evento*** es un miembro que permite que una clase u objeto proporcionen notificaciones.</span><span class="sxs-lookup"><span data-stu-id="2f629-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="2f629-619">Un evento se declara como un campo salvo que la Declaración incluye una `event` palabra clave y el tipo debe ser un tipo de delegado.</span><span class="sxs-lookup"><span data-stu-id="2f629-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="2f629-620">Dentro de una clase que declara un miembro de evento, el evento se comporta como un campo de un tipo delegado (siempre que el evento no sea abstracto y no declare descriptores de acceso).</span><span class="sxs-lookup"><span data-stu-id="2f629-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="2f629-621">El campo almacena una referencia a un delegado que representa los controladores de eventos que se han agregado al evento.</span><span class="sxs-lookup"><span data-stu-id="2f629-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="2f629-622">Si no hay ningún controlador de eventos presente, el `null`campo es.</span><span class="sxs-lookup"><span data-stu-id="2f629-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="2f629-623">La clase `List<T>` declara un único miembro de evento llamado `Changed`, lo que indica que se ha agregado un nuevo elemento a la lista.</span><span class="sxs-lookup"><span data-stu-id="2f629-623">The `List<T>` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="2f629-624">El evento lo desencadena el `OnChanged` método virtual, que primero comprueba si el evento es `null` (lo que significa que no hay ningún controlador presente). `Changed`</span><span class="sxs-lookup"><span data-stu-id="2f629-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="2f629-625">La noción de generar un evento es equivalente exactamente a invocar el delegado representado por el evento; por lo tanto, no hay ninguna construcción especial de lenguaje para generar eventos.</span><span class="sxs-lookup"><span data-stu-id="2f629-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="2f629-626">Los clientes reaccionan a los eventos mediante ***controladores de eventos***.</span><span class="sxs-lookup"><span data-stu-id="2f629-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="2f629-627">Los controladores de eventos se asocian mediante el operador `+=` y se quitan con el operador `-=`.</span><span class="sxs-lookup"><span data-stu-id="2f629-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="2f629-628">En el ejemplo siguiente se asocia un controlador de eventos con el evento `Changed` de un objeto `List<string>`.</span><span class="sxs-lookup"><span data-stu-id="2f629-628">The following example attaches an event handler to the `Changed` event of a `List<string>`.</span></span>

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
<span data-ttu-id="2f629-629">Para escenarios avanzados donde se desea controlar el almacenamiento subyacente de un evento, una declaración de evento puede proporcionar explícitamente los descriptores de acceso `add` y `remove`, que son similares en cierto modo al descriptor de acceso `set` de una propiedad.</span><span class="sxs-lookup"><span data-stu-id="2f629-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="2f629-630">Operadores</span><span class="sxs-lookup"><span data-stu-id="2f629-630">Operators</span></span>

<span data-ttu-id="2f629-631">Un ***operador*** es un miembro que define el significado de aplicar un operador de expresión determinado a las instancias de una clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="2f629-632">Se pueden definir tres tipos de operadores: operadores unarios, operadores binarios y operadores de conversión.</span><span class="sxs-lookup"><span data-stu-id="2f629-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="2f629-633">Todos los operadores se deben declarar como `public` y `static`.</span><span class="sxs-lookup"><span data-stu-id="2f629-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="2f629-634">La clase `List<T>` declara dos operadores, `operator==` y `operator!=`, y de este modo proporciona un nuevo significado a expresiones que aplican esos operadores a instancias `List`.</span><span class="sxs-lookup"><span data-stu-id="2f629-634">The `List<T>` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="2f629-635">En concreto, los operadores definen la igualdad `List<T>` de dos instancias como la comparación de cada uno de los `Equals` objetos contenidos mediante sus métodos.</span><span class="sxs-lookup"><span data-stu-id="2f629-635">Specifically, the operators define equality of two `List<T>` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="2f629-636">En el ejemplo siguiente se usa el operador `==` para comparar dos instancias `List<int>`.</span><span class="sxs-lookup"><span data-stu-id="2f629-636">The following example uses the `==` operator to compare two `List<int>` instances.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

<span data-ttu-id="2f629-637">El primer objeto `Console.WriteLine` genera `True` porque las dos listas contienen el mismo número de objetos con los mismos valores en el mismo orden.</span><span class="sxs-lookup"><span data-stu-id="2f629-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="2f629-638">Si `List<T>` no hubiera definido `operator==`, el primer objeto `Console.WriteLine` habría generado `False` porque `a` y `b` hacen referencia a diferentes instancias de `List<int>`.</span><span class="sxs-lookup"><span data-stu-id="2f629-638">Had `List<T>` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="2f629-639">Destructores</span><span class="sxs-lookup"><span data-stu-id="2f629-639">Destructors</span></span>

<span data-ttu-id="2f629-640">Un ***destructor*** es un miembro que implementa las acciones necesarias para destruir una instancia de una clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="2f629-641">Los destructores no pueden tener parámetros, no pueden tener modificadores de accesibilidad y no se pueden invocar explícitamente.</span><span class="sxs-lookup"><span data-stu-id="2f629-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="2f629-642">El destructor de una instancia se invoca automáticamente durante la recolección de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="2f629-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="2f629-643">El recolector de elementos no utilizados tiene una latitud ancha a la hora de decidir cuándo recopilar objetos y ejecutar destructores.</span><span class="sxs-lookup"><span data-stu-id="2f629-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="2f629-644">En concreto, el tiempo de las invocaciones de destructor no es determinista y los destructores se pueden ejecutar en cualquier subproceso.</span><span class="sxs-lookup"><span data-stu-id="2f629-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="2f629-645">Por estas y otras razones, las clases deben implementar destructores solo cuando no sean factibles otras soluciones.</span><span class="sxs-lookup"><span data-stu-id="2f629-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="2f629-646">La instrucción `using` proporciona un mejor enfoque para la destrucción de objetos.</span><span class="sxs-lookup"><span data-stu-id="2f629-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="2f629-647">Estructuras</span><span class="sxs-lookup"><span data-stu-id="2f629-647">Structs</span></span>

<span data-ttu-id="2f629-648">Al igual que las clases, los ***structs*** son estructuras de datos que pueden contener miembros de datos y miembros de función, pero a diferencia de las clases, los structs son tipos de valor y no requieren asignación del montón.</span><span class="sxs-lookup"><span data-stu-id="2f629-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="2f629-649">Una variable de un tipo de struct almacena directamente los datos del struct, mientras que una variable de un tipo de clase almacena una referencia a un objeto asignado dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="2f629-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="2f629-650">Los tipos struct no admiten la herencia especificada por el usuario y todos los tipos de struct se heredan implícitamente del tipo `object`.</span><span class="sxs-lookup"><span data-stu-id="2f629-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="2f629-651">Los structs son particularmente útiles para estructuras de datos pequeñas que tengan semánticas de valor.</span><span class="sxs-lookup"><span data-stu-id="2f629-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="2f629-652">Los números complejos, los puntos de un sistema de coordenadas o los pares clave-valor de un diccionario son buenos ejemplos de structs.</span><span class="sxs-lookup"><span data-stu-id="2f629-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="2f629-653">El uso de un struct en lugar de una clase para estructuras de datos pequeñas puede suponer una diferencia sustancial en el número de asignaciones de memoria que realiza una aplicación.</span><span class="sxs-lookup"><span data-stu-id="2f629-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="2f629-654">Por ejemplo, el siguiente programa crea e inicializa una matriz de 100 puntos.</span><span class="sxs-lookup"><span data-stu-id="2f629-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="2f629-655">Si `Point` se implementa como una clase, se crean instancias de 101 objetos distintos: uno para la matriz y uno por cada uno de los 100 elementos.</span><span class="sxs-lookup"><span data-stu-id="2f629-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
<span data-ttu-id="2f629-656">Una alternativa es crear `Point` un struct.</span><span class="sxs-lookup"><span data-stu-id="2f629-656">An alternative is to make `Point` a struct.</span></span>

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="2f629-657">Ahora, se crea la instancia de un solo objeto: la de la matriz, y las instancias de `Point` se asignan en línea dentro de la matriz.</span><span class="sxs-lookup"><span data-stu-id="2f629-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="2f629-658">Los structs se invocan con el operador `new`, pero eso no implica que se asigne memoria.</span><span class="sxs-lookup"><span data-stu-id="2f629-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="2f629-659">En lugar de asignar dinámicamente un objeto y devolver una referencia a él, un constructor de structs simplemente devuelve el valor del struct propiamente dicho (normalmente en una ubicación temporal en la pila) y este valor se copia luego cuando es necesario.</span><span class="sxs-lookup"><span data-stu-id="2f629-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="2f629-660">Con las clases, es posible que dos variables hagan referencia al mismo objeto y, que por tanto, las operaciones en una variable afecten al objeto al que hace referencia la otra variable.</span><span class="sxs-lookup"><span data-stu-id="2f629-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="2f629-661">Con los struct, cada variable tiene su propia copia de los datos y no es posible que las operaciones en una afecten a la otra.</span><span class="sxs-lookup"><span data-stu-id="2f629-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="2f629-662">Por ejemplo, la salida generada por el fragmento de código siguiente depende `Point` de si es una clase o un struct.</span><span class="sxs-lookup"><span data-stu-id="2f629-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="2f629-663">Si `Point` es una clase, el resultado es `20` porque `a` y `b` hacen referencia al mismo objeto.</span><span class="sxs-lookup"><span data-stu-id="2f629-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="2f629-664">Si `Point` es un struct, el resultado es `10` porque la asignación de `a` a `b` crea una copia del valor, y esta copia no se ve afectada por la asignación subsiguiente a `a.x`.</span><span class="sxs-lookup"><span data-stu-id="2f629-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="2f629-665">En el ejemplo anterior se resaltan dos de las limitaciones de los structs.</span><span class="sxs-lookup"><span data-stu-id="2f629-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="2f629-666">En primer lugar, copiar un struct entero normalmente es menos eficaz que copiar una referencia a un objeto, por lo que el paso de parámetros de asignación y valor puede ser más costoso con structs que con tipos de referencia.</span><span class="sxs-lookup"><span data-stu-id="2f629-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="2f629-667">En segundo lugar, a excepción de los parámetros `ref` y `out`, no es posible crear referencias a structs, que excluyen su uso en varias situaciones.</span><span class="sxs-lookup"><span data-stu-id="2f629-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="2f629-668">Matrices</span><span class="sxs-lookup"><span data-stu-id="2f629-668">Arrays</span></span>

<span data-ttu-id="2f629-669">Una ***matriz*** es una estructura de datos que contiene un número de variables a las que se accede mediante índices calculados.</span><span class="sxs-lookup"><span data-stu-id="2f629-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="2f629-670">Las variables contenidas en una matriz, denominadas también ***elementos*** de la matriz, son todas del mismo tipo y este tipo se conoce como ***tipo de elemento*** de la matriz.</span><span class="sxs-lookup"><span data-stu-id="2f629-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="2f629-671">Los tipos de matriz son tipos de referencia, y la declaración de una variable de matriz simplemente establece un espacio reservado para una referencia a una instancia de matriz.</span><span class="sxs-lookup"><span data-stu-id="2f629-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="2f629-672">Las instancias de matriz reales se crean dinámicamente en tiempo de ejecución `new` mediante el operador.</span><span class="sxs-lookup"><span data-stu-id="2f629-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="2f629-673">La `new` operación especifica la ***longitud*** de la nueva instancia de la matriz, que se corrige para la duración de la instancia.</span><span class="sxs-lookup"><span data-stu-id="2f629-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="2f629-674">Los índices de los elementos de una matriz van de `0` a `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="2f629-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="2f629-675">El operador `new` inicializa automáticamente los elementos de una matriz a su valor predeterminado, que, por ejemplo, es cero para todos los tipos numéricos y `null` para todos los tipos de referencias.</span><span class="sxs-lookup"><span data-stu-id="2f629-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="2f629-676">En el ejemplo siguiente se crea una matriz de elementos `int`, se inicializa la matriz y se imprime el contenido de la matriz.</span><span class="sxs-lookup"><span data-stu-id="2f629-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
<span data-ttu-id="2f629-677">Este ejemplo se crea y se pone en funcionamiento en una ***matriz unidimensional***.</span><span class="sxs-lookup"><span data-stu-id="2f629-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="2f629-678">C# también admite ***matrices multidimensionales***.</span><span class="sxs-lookup"><span data-stu-id="2f629-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="2f629-679">El número de dimensiones de un tipo de matriz, conocido también como ***rango*** del tipo de matriz, es una más el número de comas escritas entre los corchetes del tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="2f629-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="2f629-680">En el ejemplo siguiente se asigna una matriz unidimensional, bidimensional y de tres dimensiones.</span><span class="sxs-lookup"><span data-stu-id="2f629-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="2f629-681">La matriz `a1` contiene 10 elementos, la matriz `a2` 50 (10 × 5) elementos y la matriz `a3` 100 (10 × 5 × 2) elementos.</span><span class="sxs-lookup"><span data-stu-id="2f629-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="2f629-682">El tipo de elemento de una matriz puede ser cualquiera, incluido un tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="2f629-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="2f629-683">Una matriz con elementos de un tipo de matriz a veces se conoce como ***matriz escalonada*** porque las longitudes de las matrices de elementos no tienen que ser iguales.</span><span class="sxs-lookup"><span data-stu-id="2f629-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="2f629-684">En el ejemplo siguiente se asigna una matriz de matrices de `int`:</span><span class="sxs-lookup"><span data-stu-id="2f629-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="2f629-685">La primera línea crea una matriz con tres elementos, cada uno de tipo `int[]` y cada uno con un valor inicial de `null`.</span><span class="sxs-lookup"><span data-stu-id="2f629-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="2f629-686">Las líneas posteriores inicializan entonces los tres elementos con referencias a instancias de matriz individuales de longitud variable.</span><span class="sxs-lookup"><span data-stu-id="2f629-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="2f629-687">El `new` operador permite especificar los valores iniciales de los elementos de la matriz mediante un ***inicializador de matriz***, que es una lista de expresiones escritas entre `{` los delimitadores y. `}`</span><span class="sxs-lookup"><span data-stu-id="2f629-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="2f629-688">En el ejemplo siguiente se asigna e inicializa un tipo `int[]` con tres elementos.</span><span class="sxs-lookup"><span data-stu-id="2f629-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="2f629-689">Tenga en cuenta que la longitud de la matriz se deduce del número de expresiones `{` entre `}`y.</span><span class="sxs-lookup"><span data-stu-id="2f629-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="2f629-690">Las declaraciones de variable local y campo se pueden acortar más para que así no sea necesario reformular el tipo de matriz.</span><span class="sxs-lookup"><span data-stu-id="2f629-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="2f629-691">Los dos ejemplos anteriores son equivalentes a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2f629-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="2f629-692">Interfaces</span><span class="sxs-lookup"><span data-stu-id="2f629-692">Interfaces</span></span>

<span data-ttu-id="2f629-693">Una ***interfaz*** define un contrato que se puede implementar mediante clases y structs.</span><span class="sxs-lookup"><span data-stu-id="2f629-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="2f629-694">Una interfaz puede contener métodos, propiedades, eventos e indexadores.</span><span class="sxs-lookup"><span data-stu-id="2f629-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="2f629-695">Una interfaz no proporciona implementaciones de los miembros que define, simplemente especifica los miembros que se deben proporcionar mediante clases o structs que implementan la interfaz.</span><span class="sxs-lookup"><span data-stu-id="2f629-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="2f629-696">Las interfaces pueden usar ***herencia múltiple***.</span><span class="sxs-lookup"><span data-stu-id="2f629-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="2f629-697">En el ejemplo siguiente, la interfaz `IComboBox` hereda de `ITextBox` y `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="2f629-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

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
<span data-ttu-id="2f629-698">Las clases y los structs pueden implementar varias interfaces.</span><span class="sxs-lookup"><span data-stu-id="2f629-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="2f629-699">En el ejemplo siguiente, la clase `EditBox` implementa `IControl` y `IDataBound`.</span><span class="sxs-lookup"><span data-stu-id="2f629-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
<span data-ttu-id="2f629-700">Cuando una clase o un struct implementan una interfaz determinada, las instancias de esa clase o struct se pueden convertir implícitamente a ese tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="2f629-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="2f629-701">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="2f629-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="2f629-702">En casos donde una instancia no se conoce estáticamente para implementar una interfaz determinada, se pueden usar conversiones de tipo dinámico.</span><span class="sxs-lookup"><span data-stu-id="2f629-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="2f629-703">Por ejemplo, las siguientes instrucciones usan conversiones de tipo dinámico para obtener las implementaciones `IDataBound` de un objeto y de la `IControl` interfaz.</span><span class="sxs-lookup"><span data-stu-id="2f629-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="2f629-704">Dado que el tipo real del objeto es `EditBox`, las conversiones se realizan correctamente.</span><span class="sxs-lookup"><span data-stu-id="2f629-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="2f629-705">En la clase `EditBox` anterior, el `Paint` método de la `IControl` interfaz y el `Bind` método de la `IDataBound` interfaz se implementan `public` mediante miembros.</span><span class="sxs-lookup"><span data-stu-id="2f629-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="2f629-706">C#también admite ***implementaciones explícitas de miembros de interfaz***, con las que la clase o estructura puede `public`evitar la creación de miembros.</span><span class="sxs-lookup"><span data-stu-id="2f629-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="2f629-707">Una implementación de miembro de interfaz explícito se escribe con el nombre de miembro de interfaz completo.</span><span class="sxs-lookup"><span data-stu-id="2f629-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="2f629-708">Por ejemplo, la clase `EditBox` podría implementar los métodos `IControl.Paint` y `IDataBound.Bind` mediante implementaciones de miembros de interfaz explícitos del modo siguiente.</span><span class="sxs-lookup"><span data-stu-id="2f629-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="2f629-709">Solo se puede acceder a los miembros de interfaz explícitos mediante el tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="2f629-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="2f629-710">Por ejemplo, la implementación de `IControl.Paint` proporcionada por la clase `EditBox` anterior solo se puede invocar `IControl` convirtiendo primero la `EditBox` referencia al tipo de interfaz.</span><span class="sxs-lookup"><span data-stu-id="2f629-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="2f629-711">Enumeraciones</span><span class="sxs-lookup"><span data-stu-id="2f629-711">Enums</span></span>

<span data-ttu-id="2f629-712">Un ***tipo de enumeración*** es un tipo de valor distinto con un conjunto de constantes con nombre.</span><span class="sxs-lookup"><span data-stu-id="2f629-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="2f629-713">En el ejemplo siguiente se declara y se utiliza un tipo `Color` de enumeración denominado con `Red`tres `Green`valores constantes `Blue`,, y.</span><span class="sxs-lookup"><span data-stu-id="2f629-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
<span data-ttu-id="2f629-714">Cada tipo de enumeración tiene un tipo entero correspondiente denominado el tipo ***subyacente*** del tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="2f629-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="2f629-715">Un tipo de enumeración que no declara explícitamente un tipo subyacente tiene un tipo `int`subyacente de.</span><span class="sxs-lookup"><span data-stu-id="2f629-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="2f629-716">El formato de almacenamiento y el intervalo de valores posibles de un tipo enum vienen determinados por su tipo subyacente.</span><span class="sxs-lookup"><span data-stu-id="2f629-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="2f629-717">El conjunto de valores que puede tomar un tipo de enumeración no está limitado por sus miembros de enumeración.</span><span class="sxs-lookup"><span data-stu-id="2f629-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="2f629-718">En concreto, cualquier valor del tipo subyacente de una enumeración se puede convertir al tipo de enumeración y es un valor válido distinto de ese tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="2f629-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="2f629-719">En el ejemplo siguiente se declara un tipo de `Alignment` enumeración denominado con un `sbyte`tipo subyacente de.</span><span class="sxs-lookup"><span data-stu-id="2f629-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="2f629-720">Como se muestra en el ejemplo anterior, una declaración de miembro de enumeración puede incluir una expresión constante que especifique el valor del miembro.</span><span class="sxs-lookup"><span data-stu-id="2f629-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="2f629-721">El valor constante para cada miembro de la enumeración debe estar en el intervalo del tipo subyacente de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="2f629-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="2f629-722">Cuando una declaración de miembro de enumeración no especifica explícitamente un valor, el miembro recibe el valor cero (si es el primer miembro del tipo de enumeración) o el valor del miembro de enumeración anterior textual más uno.</span><span class="sxs-lookup"><span data-stu-id="2f629-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="2f629-723">Los valores de enumeración se pueden convertir en valores enteros y viceversa mediante conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="2f629-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="2f629-724">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="2f629-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="2f629-725">El valor predeterminado de cualquier tipo de enumeración es el valor entero cero convertido al tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="2f629-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="2f629-726">En los casos en los que las variables se inicializan automáticamente en un valor predeterminado, este es el valor dado a las variables de los tipos de enumeración.</span><span class="sxs-lookup"><span data-stu-id="2f629-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="2f629-727">Para que el valor predeterminado de un tipo de enumeración sea fácilmente disponible, el literal `0` se convierte implícitamente a cualquier tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="2f629-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="2f629-728">Por tanto, el siguiente código es válido.</span><span class="sxs-lookup"><span data-stu-id="2f629-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="2f629-729">Delegados</span><span class="sxs-lookup"><span data-stu-id="2f629-729">Delegates</span></span>

<span data-ttu-id="2f629-730">Un ***tipo de delegado*** representa las referencias a métodos con una lista de parámetros determinada y un tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="2f629-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="2f629-731">Los delegados permiten tratar métodos como entidades que se puedan asignar a variables y se puedan pasar como parámetros.</span><span class="sxs-lookup"><span data-stu-id="2f629-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="2f629-732">Los delegados son similares al concepto de punteros de función en otros lenguajes, pero a diferencia de los punteros de función, los delegados están orientados a objetos y presentan seguridad de tipos.</span><span class="sxs-lookup"><span data-stu-id="2f629-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="2f629-733">En el siguiente ejemplo se declara y usa un tipo de delegado denominado `Function`.</span><span class="sxs-lookup"><span data-stu-id="2f629-733">The following example declares and uses a delegate type named `Function`.</span></span>

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
<span data-ttu-id="2f629-734">Una instancia del tipo de delegado `Function` puede hacer referencia a cualquier método que tome un argumento `double` y devuelva un valor `double`.</span><span class="sxs-lookup"><span data-stu-id="2f629-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="2f629-735">El `Apply` método aplica un determinado `Function` a los elementos de un `double[]`, devolviendo `double[]` un con los resultados.</span><span class="sxs-lookup"><span data-stu-id="2f629-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="2f629-736">En el método `Main`, `Apply` se usa para aplicar tres funciones diferentes a un valor `double[]`.</span><span class="sxs-lookup"><span data-stu-id="2f629-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="2f629-737">Un delegado puede hacer referencia a un método estático (como `Square` o `Math.Sin` en el ejemplo anterior) o un método de instancia (como `m.Multiply` en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="2f629-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="2f629-738">Un delegado que hace referencia a un método de instancia también hace referencia a un objeto determinado y, cuando se invoca el método de instancia a través del delegado, ese objeto se convierte en `this` en la invocación.</span><span class="sxs-lookup"><span data-stu-id="2f629-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="2f629-739">Los delegados también pueden crearse mediante funciones anónimas, que son "métodos insertados" que se crean sobre la marcha.</span><span class="sxs-lookup"><span data-stu-id="2f629-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="2f629-740">Las funciones anónimas pueden ver las variables locales de los métodos adyacentes.</span><span class="sxs-lookup"><span data-stu-id="2f629-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="2f629-741">Por lo tanto, el ejemplo de multiplicador anterior se puede escribir más fácilmente `Multiplier` sin usar una clase:</span><span class="sxs-lookup"><span data-stu-id="2f629-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="2f629-742">Una propiedad interesante y útil de un delegado es que no sabe ni necesita conocer la clase del método al que hace referencia; lo único que importa es que el método al que se hace referencia tenga los mismos parámetros y el tipo de valor devuelto que el delegado.</span><span class="sxs-lookup"><span data-stu-id="2f629-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="2f629-743">Atributos</span><span class="sxs-lookup"><span data-stu-id="2f629-743">Attributes</span></span>

<span data-ttu-id="2f629-744">Los tipos, los miembros y otras entidades en un programa de C # admiten modificadores que controlan ciertos aspectos de su comportamiento.</span><span class="sxs-lookup"><span data-stu-id="2f629-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="2f629-745">Por ejemplo, la accesibilidad de un método se controla mediante los modificadores `public`, `protected`, `internal` y `private`.</span><span class="sxs-lookup"><span data-stu-id="2f629-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="2f629-746">C # generaliza esta funcionalidad de manera que los tipos de información declarativa definidos por el usuario se puedan adjuntar a las entidades del programa y recuperarse en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="2f629-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="2f629-747">Los programas especifican esta información declarativa adicional mediante la definición y el uso de ***atributos***.</span><span class="sxs-lookup"><span data-stu-id="2f629-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="2f629-748">En el ejemplo siguiente se declara un atributo `HelpAttribute` que se puede colocar en entidades de programa para proporcionar vínculos a la documentación asociada.</span><span class="sxs-lookup"><span data-stu-id="2f629-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
<span data-ttu-id="2f629-749">Todas las clases de atributo derivan de la `System.Attribute` clase base proporcionada por el .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2f629-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="2f629-750">Los atributos se pueden aplicar proporcionando su nombre, junto con cualquier argumento, entre corchetes, justo antes de la declaración asociada.</span><span class="sxs-lookup"><span data-stu-id="2f629-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="2f629-751">Si el nombre de un atributo termina `Attribute`en, esa parte del nombre se puede omitir cuando se hace referencia al atributo.</span><span class="sxs-lookup"><span data-stu-id="2f629-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="2f629-752">Por ejemplo, el atributo `HelpAttribute` se puede usar de la manera siguiente.</span><span class="sxs-lookup"><span data-stu-id="2f629-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="2f629-753">En este ejemplo se adjunta `HelpAttribute` un a `Widget` la `Display` clase y `HelpAttribute` otro al método en la clase.</span><span class="sxs-lookup"><span data-stu-id="2f629-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="2f629-754">Los constructores públicos de una clase de atributos controlan la información que se debe proporcionar cuando el atributo se adjunta a una entidad de programa.</span><span class="sxs-lookup"><span data-stu-id="2f629-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="2f629-755">Se puede proporcionar información adicional haciendo referencia a las propiedades públicas de lectura y escritura de la clase de atributos (como la referencia a la propiedad `Topic` usada anteriormente).</span><span class="sxs-lookup"><span data-stu-id="2f629-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="2f629-756">En el ejemplo siguiente se muestra cómo se puede recuperar la información de atributos de una entidad de programa determinada en tiempo de ejecución mediante la reflexión.</span><span class="sxs-lookup"><span data-stu-id="2f629-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
<span data-ttu-id="2f629-757">Cuando se solicita un atributo determinado mediante reflexión, se invoca al constructor de la clase de atributos con la información proporcionada en el origen del programa y se devuelve la instancia de atributo resultante.</span><span class="sxs-lookup"><span data-stu-id="2f629-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="2f629-758">Si se proporciona información adicional mediante propiedades, dichas propiedades se establecen en los valores dados antes de devolver la instancia del atributo.</span><span class="sxs-lookup"><span data-stu-id="2f629-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
