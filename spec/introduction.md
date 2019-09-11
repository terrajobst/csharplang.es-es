---
ms.openlocfilehash: 8bc4bf6310fb8a8457beee167f18d30aaca10a8e
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876893"
---
# <a name="introduction"></a>Introducción

C# (pronunciado "si sharp" en inglés) es un lenguaje de programación sencillo, moderno, orientado a objetos y con seguridad de tipos. C#tiene sus raíces en la familia de lenguajes C y será inmediatamente familiar para los programadores de C, C++y Java. C#está normalizado por ECMA International como el estándar ***ECMA-334*** y por ISO/IEC como el estándar ***iso/IEC 23270*** . El C# compilador de Microsoft para el .NET Framework es una implementación compatible de ambos estándares.

C# es un lenguaje orientado a objetos, pero también incluye compatibilidad para programación ***orientada a componentes***. El diseño de software contemporáneo se basa cada vez más en componentes de software en forma de paquetes independientes y autodescriptivos de funcionalidad. La clave de estos componentes es que presentan un modelo de programación con propiedades, métodos y eventos; tienen atributos que proporcionan información declarativa sobre el componente; e incorporan su propia documentación. C#proporciona construcciones de lenguaje para admitir directamente estos conceptos, con C# lo que se crea un lenguaje muy natural en el que crear y usar componentes de software.

Varias características de C# ayudan en la construcción de aplicaciones sólidas y duraderas: La ***recolección de elementos*** no utilizados reclama automáticamente la memoria ocupada por objetos no usados. el ***control de excepciones*** proporciona un enfoque estructurado y extensible para la detección y recuperación de errores. y el diseño ***con seguridad de tipos*** del lenguaje hace que sea imposible leer desde variables no inicializadas, indizar matrices más allá de sus límites o realizar conversiones de tipo no comprobadas.

C# tiene un ***sistema de tipo unificado***. Todos los tipos de C#, incluidos los tipos primitivos como `int` y `double`, se heredan de un único tipo `object` raíz. Por lo tanto, todos los tipos comparten un conjunto de operaciones comunes, y los valores de todos los tipos se pueden almacenar, transportar y utilizar de manera coherente. Además, C# admite tipos de valor y tipos de referencia definidos por el usuario, lo que permite la asignación dinámica de objetos, así como almacenamiento en línea de estructuras ligeras.

Para asegurarse de C# que los programas y las bibliotecas pueden evolucionar con el tiempo de una manera compatible, se ha hecho mucho C#hincapié en el control de ***versiones*** en el diseño de. Muchos lenguajes de programación prestan poca atención a este problema y, como resultado, los programas escritos en dichos lenguajes se interrumpen con más frecuencia de lo necesario cuando se introducen nuevas versiones de las bibliotecas dependientes. Los aspectos C#del diseño de que se vieron afectados directamente por las consideraciones sobre el `virtual` control `override` de versiones son los modificadores y independientes, las reglas para la resolución de sobrecarga de métodos y la compatibilidad con las declaraciones explícitas de miembros de interfaz.

En el resto de este capítulo se describen las características esenciales C# del lenguaje. Aunque los capítulos posteriores describen las reglas y las excepciones en un modo orientado a los detalles y, a veces, de forma matemática, en este capítulo se realiza una mayor claridad y una brevedad a costa de la integridad. La intención es proporcionar al lector una introducción al lenguaje que facilitará la escritura de programas iniciales y la lectura de capítulos posteriores.

## <a name="hello-world"></a>Hola a todos

El programa "Hola mundo" tradicionalmente se usa para presentar un lenguaje de programación. En este caso, se usa C#:

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

Normalmente, los archivos de código fuente de C# tienen la extensión de archivo `.cs`. Suponiendo que el programa "Hola mundo" se almacena en el archivo `hello.cs`, el programa se puede compilar con el compilador de Microsoft C# mediante la línea de comandos.
```
csc hello.cs
```
que genera un ensamblado ejecutable denominado `hello.exe`. La salida generada por esta aplicación cuando se ejecuta es
```
Hello, World
```

El programa "Hola mundo" empieza con una directiva `using` que hace referencia al espacio de nombres `System`. Los espacios de nombres proporcionan un método jerárquico para organizar las bibliotecas y los programas de C#. Los espacios de nombres contienen tipos y otros espacios de nombres; por ejemplo, el espacio de nombres `System` contiene varios tipos, como la clase `Console` a la que se hace referencia en el programa, y otros espacios de nombres, como `IO` y `Collections`. Una directiva `using` que hace referencia a un espacio de nombres determinado permite el uso no calificado de los tipos que son miembros de ese espacio de nombres. Debido a la directiva `using`, puede utilizar el programa `Console.WriteLine` como abreviatura de `System.Console.WriteLine`.

La clase `Hello` declarada por el programa "Hola mundo" tiene un miembro único, el método llamado `Main`. El `Main` método se declara con el `static` modificador. Mientras que los métodos de instancia pueden hacer referencia a una instancia de objeto envolvente determinada utilizando la palabra clave `this`, los métodos estáticos funcionan sin referencia a un objeto determinado. Por convención, un método estático denominado `Main` sirve como punto de entrada de un programa.

La salida del programa la genera el método `WriteLine` de la clase `Console` en el espacio de nombres `System`. Esta clase la proporcionan las bibliotecas de clases de .NET Framework, a las que, de forma predeterminada, el compilador de Microsoft C# hace referencia automáticamente. Tenga en C# cuenta que no tiene una biblioteca en tiempo de ejecución independiente. En su lugar, el .NET Framework es la biblioteca en C#tiempo de ejecución de.

## <a name="program-structure"></a>Estructura del programa

Los principales conceptos organizativos en C# son ***programas***, ***espacios de nombres***, ***tipos***, ***miembros*** y ***ensamblados***. Los programas de C# constan de uno o más archivos de origen. Los programas declaran tipos, que contienen miembros y pueden organizarse en espacios de nombres. Las clases e interfaces son ejemplos de tipos. Los campos, los métodos, las propiedades y los eventos son ejemplos de miembros. Cuando se compilan programas de C#, se empaquetan físicamente en ensamblados. Normalmente, los ensamblados tienen `.exe` la `.dll`extensión de archivo o, dependiendo de si implementan ***aplicaciones*** o ***bibliotecas***.

El ejemplo

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
declara una clase denominada `Stack` en un espacio de nombres denominado. `Acme.Collections` El nombre completo de esta clase es `Acme.Collections.Stack`. La clase contiene varios miembros: un campo denominado `top`, dos métodos denominados `Push` y `Pop`, y una clase anidada denominada `Entry`. La clase `Entry` contiene además tres miembros: un campo denominado `next`, un campo denominado `data` y un constructor. Suponiendo que el código fuente del ejemplo se almacene en el archivo `acme.cs`, la línea de comandos

```
csc /t:library acme.cs
```
compila el ejemplo como una biblioteca (código sin un punto de entrada `Main` y genera un ensamblado denominado `acme.dll`.

Los ensamblados contienen código ejecutable en forma de instrucciones de ***lenguaje intermedio*** (IL) e información simbólica en forma de ***metadatos***. Antes de ejecutarlo, el código de IL de un ensamblado se convierte automáticamente en código específico del procesador mediante el compilador de Just in Time (JIT) de .NET Common Language Runtime.

Dado que un ensamblado es una unidad autodescriptiva de funcionalidad que contiene código y metadatos, no hay necesidad de directivas `#include` ni archivos de encabezado de C#. Los tipos y miembros públicos contenidos en un ensamblado determinado estarán disponibles en un programa de C# simplemente haciendo referencia a dicho ensamblado al compilar el programa. Por ejemplo, este programa usa la clase `Acme.Collections.Stack` desde el ensamblado `acme.dll`:

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
Si el programa se almacena en el archivo `test.cs`, cuando `test.cs` se compila, `acme.dll` se puede hacer referencia al ensamblado mediante la opción `/r` del compilador:

```
csc /r:acme.dll test.cs
```
Esto crea un ensamblado ejecutable denominado `test.exe`, que, cuando se ejecuta, produce el resultado:

```
100
10
1
```
C# permite que el texto de origen de un programa se almacene en varios archivos de origen. Cuando se compila un programa de C# de varios archivos, todos los archivos de origen se procesan juntos y los archivos de origen pueden hacerse referencia mutuamente de forma libre; desde un punto de vista conceptual, es como si todos los archivos de origen estuvieran concatenados en un archivo de gran tamaño antes de ser procesados. En C# nunca se necesitan declaraciones adelantadas porque, excepto en contadas ocasiones, el orden de declaración es insignificante. C# no limita un archivo de origen a declarar solamente un tipo público ni precisa que el nombre del archivo de origen coincida con un tipo declarado en el archivo de origen.

## <a name="types-and-variables"></a>Tipos y variables

Hay dos clases de tipos en C#: ***tipos de valor*** y ***tipos de referencia***. Las variables de tipos de valor contienen directamente los datos, mientras que las variables de los tipos de referencia almacenan referencias a los datos, lo que se conoce como objetos. Con los tipos de referencia, es posible que dos variables hagan referencia al mismo objeto y que, por tanto, las operaciones en una variable afecten al objeto al que hace referencia la otra variable. Con los tipos de valor, cada variable tiene su propia copia de los datos y no es posible que las operaciones en una variable afecten a la otra (excepto en el caso de las variables de parámetro `ref` y `out`).

C#los tipos de valor de se dividen en ***tipos simples***, ***tipos de enumeración***, ***tipos de struct***y tipos que C# ***aceptan valores NULL***, y los tipos de referencia de se dividen en ***tipos de clase***, ***tipos de interfaz***, ***matriz tipos***y ***tipos de delegado***.

En la tabla siguiente se proporciona información C#general sobre el sistema de tipos de.

| __Categoría__    |                 | __Descripción__ |
|-----------------|-----------------|-----------------|
| Tipos de valor     | Tipos simples    | Entero con signo: `sbyte`, `short`, `int`,`long` |
|                 |                 | Entero sin signo: `byte`, `ushort`, `uint`,`ulong` |
|                 |                 | Caracteres Unicode: `char` |
|                 |                 | Punto flotante de IEEE: `float`, `double` |
|                 |                 | Decimal de alta precisión: `decimal` |
|                 |                 | Booleano: `bool` |
|                 | Tipos de enumeración      | Tipos definidos por el usuario con el formato `enum E {...}` |
|                 | Tipos de estructura    | Tipos definidos por el usuario con el formato `struct S {...}` |
|                 | Tipos que aceptan valores NULL  | Extensiones de todos los demás tipos de valor con un valor `null` |
| Tipos de referencia | Tipos de clase     | Clase base definitiva de todos los demás tipos: `object` |
|                 |                 | Cadenas Unicode: `string` |
|                 |                 | Tipos definidos por el usuario con el formato `class C {...}` |
|                 | Tipos de interfaz | Tipos definidos por el usuario con el formato `interface I {...}` |
|                 | Tipos de matriz     | Unidimensional y multidimensional; por ejemplo, `int[]` y `int[,]` |
|                 | Tipos delegados  | Tipos definidos por el usuario del formulario, por ejemplo,`delegate int  D(...)` |

Los ocho tipos enteros proporcionan compatibilidad con valores de 8, 16, 32 y 64 bits en formato con o sin signo.

Los dos tipos de punto flotante `float` , `double`y, se representan mediante los formatos IEEE 754 de precisión sencilla de 32 bits y de doble precisión de 64 bits.

El tipo `decimal` es un tipo de datos de 128 bits adecuado para cálculos financieros y monetarios.

C#el `bool` tipo de se usa para representar valores booleanos: valores que `true` son `false`o.

El procesamiento de caracteres y cadenas en C# utiliza la codificación Unicode. El tipo `char` representa una unidad de código UTF-16 y el tipo `string` representa una secuencia de unidades de código UTF-16.

En la tabla siguiente se C#resumen los tipos numéricos de.


| __Categoría__      | __Parada__ | __Type__  | __Intervalo/precisión__ |
|-------------------|----------|-----------|---------------------|
| Entero con signo   | 8        | `sbyte`   | -128... 127 |
|                   | 16       | `short`   | -32768... 32, 767 |
|                   | 32       | `int`     | -2147483648... 2, 147, 483, 647 |
|                   | 64       | `long`    | -9223372036854775808... 9, 223, 372, 036, 854, 775, 807 |
| Entero sin signo | 8        | `byte`    | 0... 255 |
|                   | 16       | `ushort`  | 0... 65, 535 |
|                   | 32       | `uint`    | 0... 4, 294, 967, 295 |
|                   | 64       | `ulong`   | 0... 18, 446, 744, 073, 709, 551, 615 |
| Punto flotante    | 32       | `float`   | 1,5 × 10 ^ − 45 a 3,4 × 10 ^ 38, precisión de 7 dígitos |
|                   | 64       | `double`  | 5,0 × 10 ^ − 324 a 1,7 × 10 ^ 308, precisión de 15 dígitos |
| Decimal           | 128      | `decimal` | 1,0 × 10 ^ − 28 a 7,9 × 10 ^ 28, precisión de 28 dígitos |

Los programas de C# utilizan ***declaraciones de tipos*** para crear nuevos tipos. Una declaración de tipos especifica el nombre y los miembros del nuevo tipo. Cinco de C#las categorías de tipos de son definibles por el usuario: tipos de clase, tipos de struct, tipos de interfaz, tipos de enumeración y tipos de delegado.

Un tipo de clase define una estructura de datos que contiene miembros de datos (campos) y miembros de función (métodos, propiedades, etc.). Los tipos de clase admiten herencia única y polimorfismo, mecanismos por los que las clases derivadas pueden extender y especializar clases base.

Un tipo de estructura es similar a un tipo de clase en que representa una estructura con miembros de datos y miembros de función. Sin embargo, a diferencia de las clases, las estructuras son tipos de valor y no requieren la asignación del montón. Los tipos struct no admiten la herencia especificada por el usuario y todos los tipos de struct se heredan implícitamente del tipo `object`.

Un tipo de interfaz define un contrato como un conjunto con nombre de miembros de función públicos. Una clase o estructura que implementa una interfaz debe proporcionar implementaciones de los miembros de función de la interfaz. Una interfaz puede heredar de varias interfaces base, y una clase o estructura puede implementar varias interfaces.

Un tipo de delegado representa referencias a métodos con una lista de parámetros determinada y un tipo de valor devuelto. Los delegados permiten tratar métodos como entidades que se puedan asignar a variables y se puedan pasar como parámetros. Los delegados son similares al concepto de punteros de función en otros lenguajes, pero a diferencia de los punteros de función, los delegados están orientados a objetos y presentan seguridad de tipos.

Los tipos de clase, estructura, interfaz y delegado admiten genéricos, mediante los cuales se pueden parametrizar con otros tipos.

Un tipo de enumeración es un tipo distinto con constantes con nombre. Cada tipo de enumeración tiene un tipo subyacente, que debe ser uno de los ocho tipos enteros. El conjunto de valores de un tipo de enumeración es igual que el conjunto de valores del tipo subyacente.

C# admite matrices unidimensionales y multidimensionales de cualquier tipo. A diferencia de los tipos enumerados anteriormente, los tipos de matriz no tienen que ser declarados antes de usarlos. En su lugar, los tipos de matriz se crean mediante un nombre de tipo entre corchetes. Por ejemplo, `int[]` es una matriz unidimensional de `int`, `int[,]` es una matriz bidimensional de `int`y `int[][]` es una matriz unidimensional de Matrices unidimensionales de `int`.

Los tipos que aceptan valores NULL tampoco tienen que declararse antes de que se puedan utilizar. Para cada tipo `T` de valor que no acepta valores NULL, existe un tipo `T?`que acepta valores NULL correspondiente, que puede `null`contener un valor adicional. Por ejemplo, `int?` es un tipo que puede contener cualquier entero de 32 bits o el valor `null`.

C#el sistema de tipos de está unificado, de modo que un valor de cualquier tipo se puede tratar como un objeto. Todos los tipos de C# directa o indirectamente se derivan del tipo de clase `object`, y `object` es la clase base definitiva de todos los tipos. Los valores de tipos de referencia se tratan como objetos mediante la visualización de los valores como tipo `object`. Los valores de los tipos de valor se tratan como objetos realizando operaciones de ***conversión boxing*** y ***unboxing*** . En el ejemplo siguiente, un valor `int` se convierte en `object` y vuelve a `int`.

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
Cuando un valor de un tipo de valor se convierte al `object`tipo, se asigna una instancia de objeto, también denominada "box", para contener el valor, y el valor se copia en ese cuadro. Por el contrario, cuando una `object` referencia se convierte en un tipo de valor, se realiza una comprobación de que el objeto al que se hace referencia es un cuadro del tipo de valor correcto y, si la comprobación se realiza correctamente, se copia el valor en el cuadro.

C#el sistema de tipos unificado significa de forma eficaz que los tipos de valor pueden convertirse en objetos "a petición". Debido a la unificación, las bibliotecas de uso general que utilizan el tipo `object` pueden usarse con tipos de referencia y tipos de valor.

Hay varios tipos de ***variables*** en C#, entre otras, campos, elementos de matriz, variables locales y parámetros. Las variables representan ubicaciones de almacenamiento, y cada variable tiene un tipo que determina qué valores se pueden almacenar en la variable, como se muestra en la tabla siguiente.


| __Tipo de variable__    | __Contenido posible__ |
|-------------------------|-----------------------|
| Tipo de valor distinto a NULL | Un valor de ese tipo exacto |
| Tipos de valor NULL     | Un valor null o un valor de ese tipo exacto |
| `object`                | Una referencia nula, una referencia a un objeto de cualquier tipo de referencia o una referencia a un valor de conversión boxing de cualquier tipo de valor |
| Tipo de clase              | Una referencia nula, una referencia a una instancia de ese tipo de clase o una referencia a una instancia de una clase derivada de ese tipo de clase. |
| Tipo de interfaz          | Una referencia nula, una referencia a una instancia de un tipo de clase que implementa ese tipo de interfaz o una referencia a un valor de conversión boxing de un tipo de valor que implementa ese tipo de interfaz. |
| Tipo de matriz              | Una referencia nula, una referencia a una instancia de ese tipo de matriz o una referencia a una instancia de un tipo de matriz compatible |
| Tipo delegado           | Una referencia nula o una referencia a una instancia de ese tipo de delegado. |

## <a name="expressions"></a>Expresiones

Las ***expresiones*** se construyen con ***operandos*** y ***operadores***. Los operadores de una expresión indican qué operaciones se aplican a los operandos. Ejemplos de operadores incluyen `+`, `-`, `*`, `/` y `new`. Algunos ejemplos de operandos son literales, campos, variables locales y expresiones.

Cuando una expresión contiene varios operadores, la ***precedencia*** de los operadores controla el orden en que se evalúan los operadores individuales. Por ejemplo, la expresión `x + y * z` se evalúa como `x + (y * z)` porque el operador `*` tiene mayor precedencia que el operador `+`.

La mayoría de los operadores se pueden ***sobrecargar***. La sobrecarga de operador permite la especificación de implementaciones de operadores definidas por el usuario para operaciones donde uno o ambos operandos son de un tipo de struct o una clase definidos por el usuario.

En la tabla siguiente se C#resumen los operadores de, que muestran las categorías de operador en orden de prioridad, de mayor a menor. Los operadores de la misma categoría tienen la misma precedencia.


| __Categoría__                     | __Expresión__    | __Descripción__ |
|----------------------------------|-------------------|-----------------|
| Principal                          | `x.m`             | Acceso a miembros |
|                                  | `x(...)`          | Invocación de método y delegado |
|                                  | `x[...]`          | Acceso a matriz e indizador |
|                                  | `x++`             | Postincremento |
|                                  | `x--`             | Postdecremento |
|                                  | `new T(...)`      | Creación de objetos y delegados |
|                                  | `new T(...){...}` | creación de objetos con inicializador |
|                                  | `new {...}`       | inicializador de objetos anónimos |
|                                  | `new T[...]`      | creación de matriz |
|                                  | `typeof(T)`       | obtener el objeto `System.Type` para `T` |
|                                  | `checked(x)`      | Evaluar expresión en contexto comprobado |
|                                  | `unchecked(x)`    | Evaluar expresión en contexto no comprobado |
|                                  | `default(T)`      | obtener valor predeterminado de tipo `T` |
|                                  | `delegate {...}`  | Función anónima (método anónimo) |
| Unario                            | `+x`              | identidad |
|                                  | `-x`              | Negación |
|                                  | `!x`              | Negación lógica |
|                                  | `~x`              | Negación bit a bit |
|                                  | `++x`             | Preincremento |
|                                  | `--x`             | Predecremento |
|                                  | `(T)x`            | convertir explícitamente `x` en el tipo `T` |
|                                  | `await x`         | esperar asincrónicamente a que finalice `x` |
| Multiplicativo                   | `x * y`           | Multiplicación |
|                                  | `x / y`           | División |
|                                  | `x % y`           | Resto |
| Aditivo                         | `x + y`           | Suma, concatenación de cadenas, combinación de delegados |
|                                  | `x - y`           | Resta, eliminación de delegados |
| Shift                            | `x << y`          | Desplazamiento a la izquierda |
|                                  | `x >> y`          | Desplazamiento a la derecha |
| Comprobación de tipos y relacional      | `x < y`           | Menor que |
|                                  | `x > y`           | Mayor que |
|                                  | `x <= y`          | Menor o igual que |
|                                  | `x >= y`          | Mayor o igual que |
|                                  | `x is T`          | volver a ejecutar `true` si `x` es una `T`, de lo contrario `false` |
|                                  | `x as T`          | volver a ejecutar `x` con tipo `T`, o `null` si `x` no es una `T` |
| Igualdad                         | `x == y`          | Igual      |
|                                  | `x != y`          | No igual |
| AND lógico                      | `x & y`           | AND bit a bit entero, AND lógico booleano |
| XOR lógico                      | `x ^ y`           | XOR bit a bit entero, XOR lógico booleano |
| OR lógico                       | <code>x &#124; y</code> | OR bit a bit entero, OR lógico booleano |
| AND condicional                  | `x && y`          | Solo evalúa si `x`es `y``true` |
| OR condicional                   | <code>x &#124;&#124; y</code> | Solo evalúa si `x`es `y``false` |
| Uso combinado de NULL                  | `x ?? y`          | Se evalúa como `y` si `x` es `null`, en `x` caso contrario,. |
| Condicional                      | `x ? y : z`       | se evalúa como `y` si `x` es `true` o `z` si `x` es `false` |
| Asignación o función anónima | `x = y`           | Asignación |
|                                  | `x op= y`         | Asignación compuesta; los operadores admitidos son `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=`<code>&#124;=</code> |
|                                  | `(T x) => y`      | Función anónima (expresión lambda) |

## <a name="statements"></a>Instrucciones

Las acciones de un programa se expresan mediante ***instrucciones***. C# admite varios tipos de instrucciones diferentes, varias de las cuales se definen en términos de instrucciones insertadas.

Un ***bloque*** permite que se escriban varias instrucciones en contextos donde se permite una única instrucción. Un bloque se compone de una lista de instrucciones escritas entre los delimitadores `{` y `}`.

Las ***instrucciones de declaración*** se usan para declarar variables locales y constantes.

Las ***instrucciones de expresión*** se usan para evaluar expresiones. Entre las expresiones que se pueden utilizar como instrucciones se incluyen las invocaciones de método, las asignaciones `new` de objetos que usan el operador `=` , las asignaciones mediante y los operadores de asignación compuesta, las operaciones de incremento y decremento mediante el `++`operadores `--` y y expresiones Await.

Las ***instrucciones de selección*** se usan para seleccionar una de varias instrucciones posibles para su ejecución en función del valor de alguna expresión. En este grupo están las instrucciones `if` y `switch`.

Las ***instrucciones de iteración*** se utilizan para ejecutar repetidamente una instrucción incrustada. En este grupo están las instrucciones `while`, `do`, `for` y `foreach`.

Las ***instrucciones de salto*** se usan para transferir el control. En este grupo están las instrucciones `break`, `continue`, `goto`, `throw`, `return` y `yield`.

La instrucción `try`... `catch` se usa para detectar excepciones que se producen durante la ejecución de un bloque, y la instrucción `try`... `finally` se usa para especificar el código de finalización que siempre se ejecuta, tanto si se ha producido una excepción como si no.

Las `checked` instrucciones `unchecked` y se utilizan para controlar el contexto de comprobación de desbordamiento para conversiones y operaciones aritméticas de tipo entero.

La instrucción `lock` se usa para obtener el bloqueo de exclusión mutua para un objeto determinado, ejecutar una instrucción y, luego, liberar el bloqueo.

La instrucción `using` se usa para obtener un recurso, ejecutar una instrucción y, luego, eliminar dicho recurso.

A continuación se muestran ejemplos de cada tipo de instrucción

__Declaraciones de variables locales__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__Declaración de constante local__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__Expression (instrucción)__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__Instrucción `if`__

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


__Instrucción `switch`__

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

__Instrucción `while`__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__Instrucción `do`__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__Instrucción `for`__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__Instrucción `foreach`__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__Instrucción `break`__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__Instrucción `continue`__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__Instrucción `goto`__

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

__Instrucción `return`__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__Instrucción `yield`__

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

__`throw`instrucciones `try` y__

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

__`checked`instrucciones `unchecked` y__

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

__Instrucción `lock`__

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

__Instrucción `using`__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a>Clases y objetos

Las ***clases*** son los tipos más fundamentales de C#. Una clase es una estructura de datos que combina estados (campos) y acciones (métodos y otros miembros de función) en una sola unidad. Una clase proporciona una definición para ***instancias*** creadas dinámicamente de la clase, también conocidas como ***objetos***. Las clases admiten ***herencia*** y ***polimorfismo***, mecanismos por los que las ***clases derivadas*** pueden extender y especializar ***clases base***.

Las clases nuevas se crean mediante declaraciones de clase. Una declaración de clase se inicia con un encabezado que especifica los atributos y modificadores de la clase, el nombre de la clase, la clase base (si se indica) y las interfaces implementadas por la clase. Al encabezado le sigue el cuerpo de la clase, que consta de una lista de declaraciones de miembros escritas entre los delimitadores `{` y `}`.

La siguiente es una declaración de una clase simple denominada `Point`:

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
Las instancias de clases se crean mediante el operador `new`, que asigna memoria para una nueva instancia, invoca un constructor para inicializar la instancia y devuelve una referencia a la instancia. Las instrucciones siguientes crean dos `Point` objetos y almacenan las referencias a esos objetos en dos variables:

```csharp
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
La memoria ocupada por un objeto se recupera automáticamente cuando el objeto ya no está en uso. No es necesario ni posible desasignar explícitamente objetos en C#.

### <a name="members"></a>Miembros

Los miembros de una clase son ***miembros estáticos*** o ***miembros de instancia***. Los miembros estáticos pertenecen a clases y los miembros de instancia pertenecen a objetos (instancias de clases).

En la tabla siguiente se proporciona información general sobre los tipos de miembros que puede contener una clase.


| __Miembro__   | __Descripción__ |
|------------  |-----------------|
| Constantes    | Valores constantes asociados a la clase |
| Campos       | Variables de la clase |
| Métodos      | Cálculos y acciones que pueden realizarse mediante la clase |
| Propiedades   | Acciones asociadas a la lectura y escritura de propiedades con nombre de la clase |
| Indizadores     | Acciones asociadas a la indexación de instancias de la clase como una matriz |
| Eventos       | Notificaciones que puede generar la clase |
| Operadores    | Conversiones y operadores de expresión admitidos por la clase |
| Constructores | Acciones necesarias para inicializar instancias de la clase o la clase propiamente dicha |
| Destructores  | Acciones que deben realizarse antes de que las instancias de la clase se descarten de forma permanente |
| Tipos        | Tipos anidados declarados por la clase |

### <a name="accessibility"></a>Accesibilidad

Cada miembro de una clase tiene asociada una accesibilidad, que controla las regiones del texto del programa que pueden tener acceso al miembro. Existen cinco formas posibles de accesibilidad. Se resumen en la tabla siguiente.


| __Accesibilidad__    | __Al__ |
|----------------------|-----------------|
| `public`             | Acceso no limitado |
| `protected`          | Acceso limitado a esta clase o a las clases derivadas de esta clase |
| `internal`           | Acceso limitado a este programa |
| `protected internal` | Acceso limitado a este programa o a las clases derivadas de esta clase |
| `private`            | Acceso limitado a esta clase |

### <a name="type-parameters"></a>Parámetros de tipo

Una definición de clase puede especificar un conjunto de parámetros de tipo poniendo tras el nombre de clase una lista de nombres de parámetro de tipo entre corchetes angulares. Los parámetros de tipo se pueden usar en el cuerpo de las declaraciones de clase para definir los miembros de la clase. En el ejemplo siguiente, los parámetros de tipo de `Pair` son `TFirst` y `TSecond`:

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
Un tipo de clase que se declara para tomar parámetros de tipo se denomina tipo de clase genérico. Los tipos struct, interfaz y delegado también pueden ser genéricos.

Cuando se usa la clase genérica, se deben proporcionar argumentos de tipo para cada uno de los parámetros de tipo:

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
Un tipo genérico con argumentos de tipo proporcionados `Pair<int,string>` , como el anterior, se denomina tipo construido.

### <a name="base-classes"></a>Clases base

Una declaración de clase puede especificar una clase base colocando después del nombre de clase y los parámetros de tipo dos puntos seguidos del nombre de la clase base. Omitir una especificación de la clase base es igual que derivarla del tipo `object`. En el ejemplo siguiente, la clase base de `Point3D` es `Point` y la clase base de `Point` es `object`:

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
Una clase hereda a los miembros de su clase base. La herencia significa que una clase contiene implícitamente todos los miembros de su clase base, excepto la instancia y constructores estáticos, y los destructores de la clase base. Una clase derivada puede agregar nuevos miembros a aquellos de los que hereda, pero no puede quitar la definición de un miembro heredado. En el ejemplo anterior, `Point3D` hereda los campos `x` y `y` de `Point` y cada instancia de `Point3D` contiene tres campos: `x`, `y` y `z`.

Existe una conversión implícita de un tipo de clase a cualquiera de sus tipos de clase base. Por lo tanto, una variable de un tipo de clase puede hacer referencia a una instancia de esa clase o a una instancia de cualquier clase derivada. Por ejemplo, dadas las declaraciones de clase anteriores, una variable de tipo `Point` puede hacer referencia a una instancia de `Point` o `Point3D`:

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>Fields

Un campo es una variable que está asociada a una clase o a una instancia de una clase.

Un campo declarado con el `static` modificador define un ***campo estático***. Un campo estático identifica exactamente una ubicación de almacenamiento. Independientemente del número de instancias de una clase que se creen, siempre solo hay una copia de un campo estático.

Un campo declarado sin el `static` modificador define un ***campo de instancia***. Cada instancia de una clase contiene una copia independiente de todos los campos de instancia de esa clase.

En el ejemplo siguiente, cada instancia de la clase `Color` tiene una copia independiente de los campos de instancia `r`, `g` y `b`, pero solo hay una copia de los campos estáticos `Black`, `White`, `Red`, `Green` y `Blue`:

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
Como se muestra en el ejemplo anterior, los ***campos de solo lectura*** se puede declarar con un modificador `readonly`. La asignación a `readonly` un campo solo se puede producir como parte de la declaración del campo o en un constructor de la misma clase.

### <a name="methods"></a>Métodos

Un ***método*** es un miembro que implementa un cálculo o una acción que puede realizar un objeto o una clase. A los ***métodos estáticos*** se accede a través de la clase. A los ***métodos de instancia*** se accede a través de instancias de la clase.

Los métodos tienen una lista (posiblemente vacía) de ***parámetros***, que representan valores o referencias de variables que se pasan al método, y un tipo de valor ***devuelto***, que especifica el tipo del valor calculado y devuelto por el método. El tipo de valor devuelto `void` de un método es si no devuelve un valor.

Al igual que los tipos, los métodos también pueden tener un conjunto de parámetros de tipo, para lo cuales se deben especificar argumentos de tipo cuando se llama al método. A diferencia de los tipos, los argumentos de tipo a menudo se pueden deducir de los argumentos de una llamada al método y no es necesario proporcionarlos explícitamente.

La ***signatura*** de un método debe ser única en la clase en la que se declara el método. La signatura de un método se compone del nombre del método, el número de parámetros de tipo y el número, los modificadores y los tipos de sus parámetros. La signatura de un método no incluye el tipo de valor devuelto.

#### <a name="parameters"></a>Parámetros

Los parámetros se usan para pasar valores o referencias a variables a métodos. Los parámetros de un método obtienen sus valores reales de los ***argumentos*** que se especifican cuando se invoca el método. Hay cuatro tipos de parámetros: parámetros de valor, parámetros de referencia, parámetros de salida y matrices de parámetros.

Un ***parámetro de valor*** se usa para el paso de parámetros de entrada. Un parámetro de valor corresponde a una variable local que obtiene su valor inicial del argumento que se ha pasado para el parámetro. Las modificaciones en un parámetro de valor no afectan el argumento que se pasa para el parámetro.

Los parámetros de valor pueden ser opcionales; se especifica un valor predeterminado para que se puedan omitir los argumentos correspondientes.

Un ***parámetro de referencia*** se usa para el paso de parámetros de entrada y salida. El argumento pasado para un parámetro de referencia debe ser una variable, y durante la ejecución del método, el parámetro de referencia representa la misma ubicación de almacenamiento que la variable del argumento. Un parámetro de referencia se declara con el modificador `ref`. En el ejemplo siguiente se muestra el uso de parámetros `ref`.

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
Un ***parámetro de salida*** se usa para pasar parámetros de salida. Un parámetro de salida es similar a un parámetro de referencia, salvo que el valor inicial del argumento proporcionado por el llamador no es importante. Un parámetro de salida se declara con el modificador `out`. En el ejemplo siguiente se muestra el uso de parámetros `out`.

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
Una ***matriz de parámetros*** permite que se pasen a un método un número variable de argumentos. Una matriz de parámetros se declara con el modificador `params`. Solo el último parámetro de un método puede ser una matriz de parámetros y el tipo de una matriz de parámetros debe ser un tipo de matriz unidimensional. Los `Write` métodos `WriteLine` y de la `System.Console` clase son buenos ejemplos de uso de la matriz de parámetros. Se declaran de la manera siguiente.

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
Dentro de un método que usa una matriz de parámetros, la matriz de parámetros se comporta exactamente igual que un parámetro normal de un tipo de matriz. Sin embargo, en una invocación de un método con una matriz de parámetros, es posible pasar un único argumento del tipo de matriz de parámetros o cualquier número de argumentos del tipo de elemento de la matriz de parámetros. En este caso, una instancia de matriz se e inicializa automáticamente con los argumentos dados. Este ejemplo

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
es equivalente a escribir lo siguiente.

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>Cuerpo del método y variables locales

El cuerpo de un método especifica las instrucciones que se ejecutan cuando se invoca el método.

Un cuerpo del método puede declarar variables que son específicas de la invocación del método. Estas variables se denominan ***variables locales***. Una declaración de variable local especifica un nombre de tipo, un nombre de variable y, posiblemente, un valor inicial. En el ejemplo siguiente se declara una variable local `i` con un valor inicial de cero y una variable local `j` sin ningún valor inicial.

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
C# requiere que se ***asigne definitivamente*** una variable local antes de que se pueda obtener su valor. Por ejemplo, si la declaración de `i` anterior no incluyera un valor inicial, el compilador notificaría un error con los usos posteriores de `i` porque `i` no se asignaría definitivamente en esos puntos del programa.

Puede usar una instrucción `return` para devolver el control a su llamador. En un método que devuelve `void`, las instrucciones `return` no pueden especificar una expresión. En un método que devuelve instrucciones`void`no `return` , debe incluir una expresión que calcule el valor devuelto.

#### <a name="static-and-instance-methods"></a>Métodos estáticos y de instancia

Un método declarado con un `static` modificador es un ***método estático***. Un método estático no opera en una instancia específica y solo puede acceder directamente a miembros estáticos.

Un método declarado sin un `static` modificador es un ***método de instancia***. Un método de instancia opera en una instancia específica y puede acceder a miembros estáticos y de instancia. Se puede acceder explícitamente a la instancia en la que se invoca un método de instancia como `this`. Es un error hacer referencia a `this` en un método estático.

La siguiente clase `Entity` tiene miembros estáticos y de instancia.

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
Cada instancia `Entity` contiene un número de serie (y probablemente alguna otra información que no se muestra aquí). El constructor `Entity` (que es como un método de instancia) inicializa la nueva instancia con el siguiente número de serie disponible. Dado que el constructor es un miembro de instancia, se le permite acceder al campo de instancia `serialNo` y al campo estático `nextSerialNo`.

Los métodos estáticos `GetNextSerialNo` y `SetNextSerialNo` pueden acceder al campo estático `nextSerialNo`, pero sería un error para ellas acceder directamente al campo de instancia `serialNo`.

En el ejemplo siguiente se muestra el uso `Entity` de la clase.

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
Tenga en cuenta que los métodos estáticos `SetNextSerialNo` y `GetNextSerialNo` se invocan en la clase, mientras que el método de instancia `GetSerialNo` se invoca en instancias de la clase.

#### <a name="virtual-override-and-abstract-methods"></a>Métodos virtual, de reemplazo y abstracto

Cuando una declaración de método de instancia incluye un modificador `virtual`, se dice que el método es un ***método virtual***. Cuando no `virtual` existe ningún modificador, se dice que el método es un ***método no virtual***.

Cuando se invoca un método virtual, el ***tipo en tiempo de ejecución*** de la instancia para la que tiene lugar esa invocación determina la implementación del método real que se invocará. En una invocación de método no virtual, el ***tipo en tiempo de compilación*** de la instancia es el factor determinante.

Un método virtual puede ser ***reemplazado*** en una clase derivada. Cuando una declaración de método de instancia `override` incluye un modificador, el método invalida un método virtual heredado con la misma firma. Mientras que una declaración de método virtual introduce un método nuevo, una declaración de método de reemplazo especializa un método virtual heredado existente proporcionando una nueva implementación de ese método.

Un método ***abstracto*** es un método virtual sin implementación. Un método abstracto se declara con `abstract` el modificador y solo se permite en una clase que también se `abstract`declara. Un método abstracto debe reemplazarse en todas las clases derivadas no abstractas.

En el ejemplo siguiente se declara una clase abstracta, `Expression`, que representa un nodo de árbol de expresión y tres clases derivadas, `Constant`, `VariableReference` y `Operation`, que implementan nodos de árbol de expresión para constantes, referencias a variables y operaciones aritméticas. (Esto es similar a, pero no debe confundirse con los tipos de árbol de expresión introducidos en los [tipos de árbol de expresión](types.md#expression-tree-types)).

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
Las cuatro clases anteriores se pueden usar para modelar expresiones aritméticas. Por ejemplo, usando instancias de estas clases, la expresión `x + 3` se puede representar de la manera siguiente.

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
El método `Evaluate` de una instancia `Expression` se invoca para evaluar la expresión determinada y generar un valor `double`. El método toma como argumento un `Hashtable` que contiene nombres de variables (como claves de las entradas) y valores (como valores de las entradas). El `Evaluate` método es un método abstracto virtual, lo que significa que las clases derivadas no abstractas deben invalidarlo para proporcionar una implementación real.

Una implementación de `Constant` de `Evaluate` simplemente devuelve la constante almacenada. Una `VariableReference`implementación de busca el nombre de la variable en la tabla hash y devuelve el valor resultante. Una implementación de `Operation` evalúa primero los operandos izquierdo y derecho (mediante la invocación recursiva de sus métodos `Evaluate`) y luego realiza la operación aritmética correspondiente.

El siguiente programa usa las clases `Expression` para evaluar la expresión `x * (y + 2)` para los distintos valores de `x` y `y`.

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

#### <a name="method-overloading"></a>Sobrecarga de métodos

La ***sobrecarga*** de métodos permite que varios métodos de la misma clase tengan el mismo nombre mientras tengan signaturas únicas. Al compilar una invocación de un método sobrecargado, el compilador usa la ***resolución de sobrecarga*** para determinar el método concreto que de invocará. La resolución de sobrecarga busca el método que mejor coincida con los argumentos o informa de un error si no se puede encontrar ninguna mejor coincidencia. En el ejemplo siguiente se muestra la resolución de sobrecarga en vigor. El comentario para cada invocación del método `Main` muestra qué método se invoca realmente.

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
Tal como se muestra en el ejemplo, un método determinado siempre se puede seleccionar mediante la conversión explícita de los argumentos en los tipos de parámetros exactos o el suministro explícito de los argumentos de tipo.

### <a name="other-function-members"></a>Otros miembros de función

Los miembros que contienen código ejecutable se conocen colectivamente como ***miembros de función*** de una clase. En la sección anterior se han descrito métodos, que son el tipo principal de los miembros de función. En esta sección se describen los demás tipos de miembros de C#función admitidos por: constructores, propiedades, indexadores, eventos, operadores y destructores.

En el código siguiente se muestra una clase `List<T>`genérica denominada, que implementa una lista de objetos que se va a ampliar. La clase contiene varios ejemplos de los tipos más comunes de miembros de función.


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

#### <a name="constructors"></a>Constructores

C# admite constructores de instancia y estáticos. Un ***constructor de instancia*** es un miembro que implementa las acciones necesarias para inicializar una instancia de una clase. Un ***constructor estático*** es un miembro que implementa las acciones necesarias para inicializar una clase en sí misma cuando se carga por primera vez.

Un constructor se declara como un método sin ningún tipo de valor devuelto y el mismo nombre que la clase contenedora. Si una declaración de constructor incluye `static` un modificador, declara un constructor estático. De lo contrario, declara un constructor de instancia.

Los constructores de instancias se pueden sobrecargar. Por ejemplo, la clase `List<T>` declara dos constructores de instancia, una sin parámetros y otra que toma un parámetro `int`. Los constructores de instancia se invocan mediante el operador `new`. Las instrucciones siguientes asignan `List<string>` dos instancias de mediante cada uno de los constructores de la `List` clase.

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
A diferencia de otros miembros, los constructores de instancia no se heredan y una clase no tiene ningún constructor de instancia que no sea el que se declara realmente en la clase. Si no se proporciona ningún constructor de instancia para una clase, se proporciona automáticamente uno vacío sin ningún parámetro.

#### <a name="properties"></a>Propiedades

Las ***propiedades*** son una extensión natural de los campos. Ambos son miembros con nombre con tipos asociados y la sintaxis para acceder a los campos y las propiedades es la misma. Sin embargo, a diferencia de los campos, las propiedades no denotan ubicaciones de almacenamiento. Las propiedades tienen ***descriptores de acceso*** que especifican las instrucciones que se ejecutan cuando se leen o escriben sus valores.

Una propiedad se declara como un campo, salvo que la declaración finaliza con un `get` descriptor de acceso `set` o un descriptor de acceso `{` escrito `}` entre los delimitadores y en lugar de terminar en un punto y coma. Una propiedad que tiene tanto un `get` descriptor `set` de acceso como un descriptor de acceso es una propiedad ***de lectura y escritura***, una propiedad que solo tiene un `get` descriptor de acceso es una ***propiedad de solo lectura***y una propiedad que solo tiene un `set` descriptor de acceso es un ***propiedad de solo escritura***.

Un `get` descriptor de acceso corresponde a un método sin parámetros con un valor devuelto del tipo de propiedad. Excepto como destino de una asignación, cuando se hace referencia a una propiedad en una expresión, el `get` descriptor de acceso de la propiedad se invoca para calcular el valor de la propiedad.

Un `set` descriptor de acceso corresponde a un método con `value` un solo parámetro denominado y ningún tipo de valor devuelto. Cuando se hace referencia a una propiedad como el destino de una asignación o como el operando `--`de o `set` , el descriptor de `++` acceso se invoca con un argumento que proporciona el nuevo valor.

La clase `List<T>` declara dos propiedades, `Count` y `Capacity`, que son de solo lectura y de lectura y escritura, respectivamente. El siguiente es un ejemplo de uso de estas propiedades.

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
De forma similar a los campos y métodos, C# admite propiedades de instancia y propiedades estáticas. Las propiedades estáticas se declaran con el `static` modificador y las propiedades de instancia se declaran sin ella.

Los descriptores de acceso de una propiedad pueden ser virtuales. Cuando una declaración de propiedad incluye un modificador `virtual`, `abstract` o `override`, se aplica a los descriptores de acceso de la propiedad.

#### <a name="indexers"></a>Indizadores

Un ***indexador*** es un miembro que permite indexar de la misma manera que una matriz. Un indexador se declara como una propiedad, excepto por el hecho que el nombre del miembro es `this`, seguido por una lista de parámetros que se escriben entre los delimitadores `[` y `]`. Los parámetros están disponibles en los descriptores de acceso del indexador. De forma similar a las propiedades, los indexadores pueden ser lectura y escritura, de solo lectura y de solo escritura, y los descriptores de acceso de un indexador pueden ser virtuales.

La clase `List` declara un único indexador de lectura y escritura que toma un parámetro `int`. El indexador permite indexar instancias de `List` con valores `int`. Por ejemplo

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
Los indexadores se pueden sobrecargar, lo que significa que una clase puede declarar varios indexadores siempre y cuando el número o los tipos de sus parámetros sean diferentes.

#### <a name="events"></a>Eventos

Un ***evento*** es un miembro que permite que una clase u objeto proporcionen notificaciones. Un evento se declara como un campo salvo que la Declaración incluye una `event` palabra clave y el tipo debe ser un tipo de delegado.

Dentro de una clase que declara un miembro de evento, el evento se comporta como un campo de un tipo delegado (siempre que el evento no sea abstracto y no declare descriptores de acceso). El campo almacena una referencia a un delegado que representa los controladores de eventos que se han agregado al evento. Si no hay ningún controlador de eventos presente, el `null`campo es.

La clase `List<T>` declara un único miembro de evento llamado `Changed`, lo que indica que se ha agregado un nuevo elemento a la lista. El evento lo desencadena el `OnChanged` método virtual, que primero comprueba si el evento es `null` (lo que significa que no hay ningún controlador presente). `Changed` La noción de generar un evento es equivalente exactamente a invocar el delegado representado por el evento; por lo tanto, no hay ninguna construcción especial de lenguaje para generar eventos.

Los clientes reaccionan a los eventos mediante ***controladores de eventos***. Los controladores de eventos se asocian mediante el operador `+=` y se quitan con el operador `-=`. En el ejemplo siguiente se asocia un controlador de eventos con el evento `Changed` de un objeto `List<string>`.

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
Para escenarios avanzados donde se desea controlar el almacenamiento subyacente de un evento, una declaración de evento puede proporcionar explícitamente los descriptores de acceso `add` y `remove`, que son similares en cierto modo al descriptor de acceso `set` de una propiedad.

#### <a name="operators"></a>Operadores

Un ***operador*** es un miembro que define el significado de aplicar un operador de expresión determinado a las instancias de una clase. Se pueden definir tres tipos de operadores: operadores unarios, operadores binarios y operadores de conversión. Todos los operadores se deben declarar como `public` y `static`.

La clase `List<T>` declara dos operadores, `operator==` y `operator!=`, y de este modo proporciona un nuevo significado a expresiones que aplican esos operadores a instancias `List`. En concreto, los operadores definen la igualdad `List<T>` de dos instancias como la comparación de cada uno de los `Equals` objetos contenidos mediante sus métodos. En el ejemplo siguiente se usa el operador `==` para comparar dos instancias `List<int>`.

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

El primer objeto `Console.WriteLine` genera `True` porque las dos listas contienen el mismo número de objetos con los mismos valores en el mismo orden. Si `List<T>` no hubiera definido `operator==`, el primer objeto `Console.WriteLine` habría generado `False` porque `a` y `b` hacen referencia a diferentes instancias de `List<int>`.

#### <a name="destructors"></a>Destructores

Un ***destructor*** es un miembro que implementa las acciones necesarias para destruir una instancia de una clase. Los destructores no pueden tener parámetros, no pueden tener modificadores de accesibilidad y no se pueden invocar explícitamente. El destructor de una instancia se invoca automáticamente durante la recolección de elementos no utilizados.

El recolector de elementos no utilizados tiene una latitud ancha a la hora de decidir cuándo recopilar objetos y ejecutar destructores. En concreto, el tiempo de las invocaciones de destructor no es determinista y los destructores se pueden ejecutar en cualquier subproceso. Por estas y otras razones, las clases deben implementar destructores solo cuando no sean factibles otras soluciones.

La instrucción `using` proporciona un mejor enfoque para la destrucción de objetos.

## <a name="structs"></a>Estructuras

Al igual que las clases, los ***structs*** son estructuras de datos que pueden contener miembros de datos y miembros de función, pero a diferencia de las clases, los structs son tipos de valor y no requieren asignación del montón. Una variable de un tipo de struct almacena directamente los datos del struct, mientras que una variable de un tipo de clase almacena una referencia a un objeto asignado dinámicamente. Los tipos struct no admiten la herencia especificada por el usuario y todos los tipos de struct se heredan implícitamente del tipo `object`.

Los structs son particularmente útiles para estructuras de datos pequeñas que tengan semánticas de valor. Los números complejos, los puntos de un sistema de coordenadas o los pares clave-valor de un diccionario son buenos ejemplos de structs. El uso de un struct en lugar de una clase para estructuras de datos pequeñas puede suponer una diferencia sustancial en el número de asignaciones de memoria que realiza una aplicación. Por ejemplo, el siguiente programa crea e inicializa una matriz de 100 puntos. Si `Point` se implementa como una clase, se crean instancias de 101 objetos distintos: uno para la matriz y uno por cada uno de los 100 elementos.

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
Una alternativa es crear `Point` un struct.

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
Ahora, se crea la instancia de un solo objeto: la de la matriz, y las instancias de `Point` se asignan en línea dentro de la matriz.

Los structs se invocan con el operador `new`, pero eso no implica que se asigne memoria. En lugar de asignar dinámicamente un objeto y devolver una referencia a él, un constructor de structs simplemente devuelve el valor del struct propiamente dicho (normalmente en una ubicación temporal en la pila) y este valor se copia luego cuando es necesario.

Con las clases, es posible que dos variables hagan referencia al mismo objeto y, que por tanto, las operaciones en una variable afecten al objeto al que hace referencia la otra variable. Con los struct, cada variable tiene su propia copia de los datos y no es posible que las operaciones en una afecten a la otra. Por ejemplo, la salida generada por el fragmento de código siguiente depende `Point` de si es una clase o un struct.

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
Si `Point` es una clase, el resultado es `20` porque `a` y `b` hacen referencia al mismo objeto. Si `Point` es un struct, el resultado es `10` porque la asignación de `a` a `b` crea una copia del valor, y esta copia no se ve afectada por la asignación subsiguiente a `a.x`.

En el ejemplo anterior se resaltan dos de las limitaciones de los structs. En primer lugar, copiar un struct entero normalmente es menos eficaz que copiar una referencia a un objeto, por lo que el paso de parámetros de asignación y valor puede ser más costoso con structs que con tipos de referencia. En segundo lugar, a excepción de los parámetros `ref` y `out`, no es posible crear referencias a structs, que excluyen su uso en varias situaciones.

## <a name="arrays"></a>Matrices

Una ***matriz*** es una estructura de datos que contiene un número de variables a las que se accede mediante índices calculados. Las variables contenidas en una matriz, denominadas también ***elementos*** de la matriz, son todas del mismo tipo y este tipo se conoce como ***tipo de elemento*** de la matriz.

Los tipos de matriz son tipos de referencia, y la declaración de una variable de matriz simplemente establece un espacio reservado para una referencia a una instancia de matriz. Las instancias de matriz reales se crean dinámicamente en tiempo de ejecución `new` mediante el operador. La `new` operación especifica la ***longitud*** de la nueva instancia de la matriz, que se corrige para la duración de la instancia. Los índices de los elementos de una matriz van de `0` a `Length - 1`. El operador `new` inicializa automáticamente los elementos de una matriz a su valor predeterminado, que, por ejemplo, es cero para todos los tipos numéricos y `null` para todos los tipos de referencias.

En el ejemplo siguiente se crea una matriz de elementos `int`, se inicializa la matriz y se imprime el contenido de la matriz.

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
Este ejemplo se crea y se pone en funcionamiento en una ***matriz unidimensional***. C# también admite ***matrices multidimensionales***. El número de dimensiones de un tipo de matriz, conocido también como ***rango*** del tipo de matriz, es una más el número de comas escritas entre los corchetes del tipo de matriz. En el ejemplo siguiente se asigna una matriz unidimensional, bidimensional y de tres dimensiones.

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
La matriz `a1` contiene 10 elementos, la matriz `a2` 50 (10 × 5) elementos y la matriz `a3` 100 (10 × 5 × 2) elementos.

El tipo de elemento de una matriz puede ser cualquiera, incluido un tipo de matriz. Una matriz con elementos de un tipo de matriz a veces se conoce como ***matriz escalonada*** porque las longitudes de las matrices de elementos no tienen que ser iguales. En el ejemplo siguiente se asigna una matriz de matrices de `int`:

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
La primera línea crea una matriz con tres elementos, cada uno de tipo `int[]` y cada uno con un valor inicial de `null`. Las líneas posteriores inicializan entonces los tres elementos con referencias a instancias de matriz individuales de longitud variable.

El `new` operador permite especificar los valores iniciales de los elementos de la matriz mediante un ***inicializador de matriz***, que es una lista de expresiones escritas entre `{` los delimitadores y. `}` En el ejemplo siguiente se asigna e inicializa un tipo `int[]` con tres elementos.

```csharp
int[] a = new int[] {1, 2, 3};
```
Tenga en cuenta que la longitud de la matriz se deduce del número de expresiones `{` entre `}`y. Las declaraciones de variable local y campo se pueden acortar más para que así no sea necesario reformular el tipo de matriz.

```csharp
int[] a = {1, 2, 3};
```
Los dos ejemplos anteriores son equivalentes a lo siguiente:

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>Interfaces

Una ***interfaz*** define un contrato que se puede implementar mediante clases y structs. Una interfaz puede contener métodos, propiedades, eventos e indexadores. Una interfaz no proporciona implementaciones de los miembros que define, simplemente especifica los miembros que se deben proporcionar mediante clases o structs que implementan la interfaz.

Las interfaces pueden usar ***herencia múltiple***. En el ejemplo siguiente, la interfaz `IComboBox` hereda de `ITextBox` y `IListBox`.

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
Las clases y los structs pueden implementar varias interfaces. En el ejemplo siguiente, la clase `EditBox` implementa `IControl` y `IDataBound`.

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
Cuando una clase o un struct implementan una interfaz determinada, las instancias de esa clase o struct se pueden convertir implícitamente a ese tipo de interfaz. Por ejemplo

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
En casos donde una instancia no se conoce estáticamente para implementar una interfaz determinada, se pueden usar conversiones de tipo dinámico. Por ejemplo, las siguientes instrucciones usan conversiones de tipo dinámico para obtener las implementaciones `IDataBound` de un objeto y de la `IControl` interfaz. Dado que el tipo real del objeto es `EditBox`, las conversiones se realizan correctamente.

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
En la clase `EditBox` anterior, el `Paint` método de la `IControl` interfaz y el `Bind` método de la `IDataBound` interfaz se implementan `public` mediante miembros. C#también admite ***implementaciones explícitas de miembros de interfaz***, con las que la clase o estructura puede `public`evitar la creación de miembros. Una implementación de miembro de interfaz explícito se escribe con el nombre de miembro de interfaz completo. Por ejemplo, la clase `EditBox` podría implementar los métodos `IControl.Paint` y `IDataBound.Bind` mediante implementaciones de miembros de interfaz explícitos del modo siguiente.

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
Solo se puede acceder a los miembros de interfaz explícitos mediante el tipo de interfaz. Por ejemplo, la implementación de `IControl.Paint` proporcionada por la clase `EditBox` anterior solo se puede invocar `IControl` convirtiendo primero la `EditBox` referencia al tipo de interfaz.

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>Enumeraciones

Un ***tipo de enumeración*** es un tipo de valor distinto con un conjunto de constantes con nombre. En el ejemplo siguiente se declara y se utiliza un tipo `Color` de enumeración denominado con `Red`tres `Green`valores constantes `Blue`,, y.

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
Cada tipo de enumeración tiene un tipo entero correspondiente denominado el tipo ***subyacente*** del tipo de enumeración. Un tipo de enumeración que no declara explícitamente un tipo subyacente tiene un tipo `int`subyacente de. El formato de almacenamiento y el intervalo de valores posibles de un tipo enum vienen determinados por su tipo subyacente. El conjunto de valores que puede tomar un tipo de enumeración no está limitado por sus miembros de enumeración. En concreto, cualquier valor del tipo subyacente de una enumeración se puede convertir al tipo de enumeración y es un valor válido distinto de ese tipo de enumeración.

En el ejemplo siguiente se declara un tipo de `Alignment` enumeración denominado con un `sbyte`tipo subyacente de.

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
Como se muestra en el ejemplo anterior, una declaración de miembro de enumeración puede incluir una expresión constante que especifique el valor del miembro. El valor constante para cada miembro de la enumeración debe estar en el intervalo del tipo subyacente de la enumeración. Cuando una declaración de miembro de enumeración no especifica explícitamente un valor, el miembro recibe el valor cero (si es el primer miembro del tipo de enumeración) o el valor del miembro de enumeración anterior textual más uno.

Los valores de enumeración se pueden convertir en valores enteros y viceversa mediante conversiones de tipos. Por ejemplo

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
El valor predeterminado de cualquier tipo de enumeración es el valor entero cero convertido al tipo de enumeración. En los casos en los que las variables se inicializan automáticamente en un valor predeterminado, este es el valor dado a las variables de los tipos de enumeración. Para que el valor predeterminado de un tipo de enumeración sea fácilmente disponible, el literal `0` se convierte implícitamente a cualquier tipo de enumeración. Por tanto, el siguiente código es válido.

```csharp
Color c = 0;
```

## <a name="delegates"></a>Delegados

Un ***tipo de delegado*** representa las referencias a métodos con una lista de parámetros determinada y un tipo de valor devuelto. Los delegados permiten tratar métodos como entidades que se puedan asignar a variables y se puedan pasar como parámetros. Los delegados son similares al concepto de punteros de función en otros lenguajes, pero a diferencia de los punteros de función, los delegados están orientados a objetos y presentan seguridad de tipos.

En el siguiente ejemplo se declara y usa un tipo de delegado denominado `Function`.

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
Una instancia del tipo de delegado `Function` puede hacer referencia a cualquier método que tome un argumento `double` y devuelva un valor `double`. El `Apply` método aplica un determinado `Function` a los elementos de un `double[]`, devolviendo `double[]` un con los resultados. En el método `Main`, `Apply` se usa para aplicar tres funciones diferentes a un valor `double[]`.

Un delegado puede hacer referencia a un método estático (como `Square` o `Math.Sin` en el ejemplo anterior) o un método de instancia (como `m.Multiply` en el ejemplo anterior). Un delegado que hace referencia a un método de instancia también hace referencia a un objeto determinado y, cuando se invoca el método de instancia a través del delegado, ese objeto se convierte en `this` en la invocación.

Los delegados también pueden crearse mediante funciones anónimas, que son "métodos insertados" que se crean sobre la marcha. Las funciones anónimas pueden ver las variables locales de los métodos adyacentes. Por lo tanto, el ejemplo de multiplicador anterior se puede escribir más fácilmente `Multiplier` sin usar una clase:

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
Una propiedad interesante y útil de un delegado es que no sabe ni necesita conocer la clase del método al que hace referencia; lo único que importa es que el método al que se hace referencia tenga los mismos parámetros y el tipo de valor devuelto que el delegado.

## <a name="attributes"></a>Atributos

Los tipos, los miembros y otras entidades en un programa de C # admiten modificadores que controlan ciertos aspectos de su comportamiento. Por ejemplo, la accesibilidad de un método se controla mediante los modificadores `public`, `protected`, `internal` y `private`. C # generaliza esta funcionalidad de manera que los tipos de información declarativa definidos por el usuario se puedan adjuntar a las entidades del programa y recuperarse en tiempo de ejecución. Los programas especifican esta información declarativa adicional mediante la definición y el uso de ***atributos***.

En el ejemplo siguiente se declara un atributo `HelpAttribute` que se puede colocar en entidades de programa para proporcionar vínculos a la documentación asociada.

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
Todas las clases de atributo derivan de la `System.Attribute` clase base proporcionada por el .NET Framework. Los atributos se pueden aplicar proporcionando su nombre, junto con cualquier argumento, entre corchetes, justo antes de la declaración asociada. Si el nombre de un atributo termina `Attribute`en, esa parte del nombre se puede omitir cuando se hace referencia al atributo. Por ejemplo, el atributo `HelpAttribute` se puede usar de la manera siguiente.

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
En este ejemplo se adjunta `HelpAttribute` un a `Widget` la `Display` clase y `HelpAttribute` otro al método en la clase. Los constructores públicos de una clase de atributos controlan la información que se debe proporcionar cuando el atributo se adjunta a una entidad de programa. Se puede proporcionar información adicional haciendo referencia a las propiedades públicas de lectura y escritura de la clase de atributos (como la referencia a la propiedad `Topic` usada anteriormente).

En el ejemplo siguiente se muestra cómo se puede recuperar la información de atributos de una entidad de programa determinada en tiempo de ejecución mediante la reflexión.

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
Cuando se solicita un atributo determinado mediante reflexión, se invoca al constructor de la clase de atributos con la información proporcionada en el origen del programa y se devuelve la instancia de atributo resultante. Si se proporciona información adicional mediante propiedades, dichas propiedades se establecen en los valores dados antes de devolver la instancia del atributo.
