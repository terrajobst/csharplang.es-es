---
ms.openlocfilehash: 90001cf3d48f216787fc65e59166ec57c5d0ca34
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229748"
---
# <a name="unsafe-code"></a>Código no seguro

El núcleo del lenguaje C#, tal como se define en los capítulos anteriores, difiere notablemente de C y C++ en su omisión de punteros como un tipo de datos. En su lugar, C# proporciona referencias y la capacidad para crear objetos que son administrados por un recolector de elementos no utilizados. Este diseño, junto con otras características, hace que C# un lenguaje mucho más seguro que C o C++. En el lenguaje básico C# no es posible tener una variable no inicializada, un puntero "pendiente" o una expresión que indiza una matriz más allá de sus límites. Categorías completas de errores que suelen contaminen C y, por tanto, se eliminan los programas de C++.

Aunque prácticamente cualquier construcción del tipo de puntero en C o C++ tiene un tipo de referencia equivalente en C#, sin embargo, hay situaciones donde el acceso a los tipos de puntero se convierte en una necesidad. Por ejemplo, interactuar con el sistema operativo subyacente, obtener acceso a un dispositivo asignado a la memoria o implementar un algoritmo puntuales puede no ser posible ni práctico sin acceso a punteros. Para abordar esta necesidad, C# ofrece la posibilidad de escribir ***código no seguro***.

En código no seguro es posible declarar y operar con punteros, realizar conversiones entre los punteros y los tipos enteros, para tomar la dirección de las variables y así sucesivamente. En cierto sentido, escribir código no seguro es muy parecido a escribir código de C dentro de un programa de C#.

De hecho, el código no seguro es una característica "segura" desde la perspectiva de los desarrolladores y usuarios. Código no seguro debe marcarse con claridad con el modificador `unsafe`, por lo que los desarrolladores no puedan usar características no seguras por accidente, y el motor de ejecución funciona para asegurarse de que no se puede ejecutar código no seguro en un entorno de confianza.

## <a name="unsafe-contexts"></a>Contextos no seguros

Las características de C# no seguras solo están disponibles en contextos no seguros. Se introduce un contexto no seguro mediante la inclusión de un `unsafe` modificador en la declaración de un tipo o miembro, o bien mediante un *unsafe_statement*:

*  Una declaración de clase, struct, interfaz o delegado puede incluir un `unsafe` modificador, en cuyo caso toda la extensión textual de la declaración (incluido el cuerpo de la clase, estructura o interfaz) se considera un contexto no seguro.
*  Una declaración de un campo, método, propiedad, evento, indizador, operador, constructor de instancia, un destructor o constructor estático puede incluir un `unsafe` modificador, en cuyo caso toda la extensión textual de la declaración del miembro se considera un no seguro contexto.
*  Un *unsafe_statement* habilita el uso de un contexto no seguro dentro de un *bloque*. Toda la extensión textual del asociado *bloque* se considera un contexto no seguro.

Las producciones gramaticales asociados se muestran a continuación.

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

En el ejemplo

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

el `unsafe` modificador especificado en la declaración de estructura hace que toda la extensión textual de la declaración de struct para convertirse en un contexto no seguro. Por lo tanto, es posible declarar el `Left` y `Right` campos de un tipo de puntero. El ejemplo anterior también podría escribirse

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

En este caso, el `unsafe` modificadores en declaraciones de campo, hacer que estas declaraciones se consideren contextos no seguros.

Aparte de establecer un contexto no seguro, lo que permite el uso de tipos de puntero, el `unsafe` modificador no tiene ningún efecto en un tipo o miembro. En el ejemplo

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

el `unsafe` modificador en el `F` método `A` simplemente hace que la extensión textual del `F` para convertirse en un contexto no seguro en el que se pueden usar las características del lenguaje no seguras. En el reemplazo de `F` en `B`, no es necesario volver a especificar la `unsafe` modificador--a menos que, por supuesto, el `F` método `B` sí necesita tener acceso a características no seguras.

La situación es ligeramente diferente de un tipo de puntero es parte de la firma del método

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

En este caso, porque `F`de firma incluye un tipo de puntero, solo se puede escribir en un contexto no seguro. Sin embargo, el contexto no seguro se puede introducir convirtiendo toda la clase no seguro, como sucede en `A`, o mediante la inclusión de un `unsafe` modificador en la declaración de método, como es el caso en `B`.

## <a name="pointer-types"></a>Tipos de puntero

En un contexto no seguro, un *tipo* ([tipos](types.md)) puede ser un *tipo_de_puntero* , así como un *value_type* o un *reference_type* . Sin embargo, un *tipo_de_puntero* también se puede usar en un `typeof` expresión ([expresiones de creación de objeto anónimo](expressions.md#anonymous-object-creation-expressions)) fuera de un contexto no seguro como tal uso no es seguro.

```antlr
type_unsafe
    : pointer_type
    ;
```

Un *tipo_de_puntero* se escribe como un *unmanaged_type* o la palabra clave `void`, seguido de un `*` token:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

El tipo especificado antes del `*` en un puntero de tipo se denomina el ***tipo referente*** del tipo de puntero. Representa el tipo de la variable a la que apunta un valor del tipo de puntero.

A diferencia de las referencias (valores de tipos de referencia), punteros no se realiza el seguimiento por el recolector de elementos no utilizados, el recolector de elementos no utilizados no tiene conocimiento de los punteros y los datos a los que señalan. Por este motivo, un puntero no se permite para que apunte a una referencia o a una estructura que contenga referencias, y debe ser el tipo referente de un puntero un *unmanaged_type*.

Un *unmanaged_type* es cualquier tipo que no sea un *reference_type* o tipo construido y no contiene *reference_type* o construye los campos de tipo en cualquier nivel de anidamiento. En otras palabras, un *unmanaged_type* es uno de los siguientes:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, o `bool`.
*  Cualquier *enum_type*.
*  Cualquier *tipo_de_puntero*.
*  Ninguno definido por el usuario *struct_type* que no es un tipo construido y contiene los campos de *unmanaged_type*s solo.

La regla intuitiva para mezclar punteros y referencias es que pueden contener punteros referentes de las referencias (objetos), pero referentes de punteros no pueden contener referencias.

En la tabla siguiente figuran algunos ejemplos de tipos de puntero:

| __Ejemplo__ | __Descripción__                               |
|-------------|-----------------------------------------------|
| `byte*`     | puntero a `byte`                             |
| `char*`     | puntero a `char`                             |
| `int**`     | Puntero a puntero a `int`                   |
| `int*[]`    | Matriz unidimensional de punteros a `int` |
| `void*`     | Puntero a un tipo desconocido                       |

Para una implementación determinada, todos los tipos de puntero deben tener el mismo tamaño y la representación.

A diferencia de C y C++, cuando se declaran varios punteros en la misma declaración, en C# el `*` se escribe junto con el tipo subyacente únicamente, no como una puntuación de prefijo en cada nombre de puntero. Por ejemplo

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

El valor de un puntero de tipo `T*` representa la dirección de una variable de tipo `T`. El operador de direccionamiento indirecto del puntero `*` ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)) puede utilizarse para tener acceso a esta variable. Por ejemplo, dada una variable `P` typu `int*`, la expresión `*P` denota el `int` se encontró en la dirección contenida en la variable `P`.

Al igual que una referencia de objeto, puede ser un puntero `null`. Aplicar el operador de direccionamiento indirecto a un `null` puntero da como resultado un comportamiento definido por la implementación. Un puntero con el valor `null` viene dado por todos los bits cero.

El `void*` tipo representa un puntero a un tipo desconocido. Dado que el tipo referente es desconocido, no se puede aplicar el operador de direccionamiento indirecto a un puntero de tipo `void*`, ni puede realizarse ninguna operación aritmética en un puntero. Sin embargo, un puntero de tipo `void*` puede convertirse a cualquier otro tipo de puntero (y viceversa).

Tipos de puntero son una categoría de tipos independiente. A diferencia de los tipos de referencia y tipos de valor, tipos de puntero no heredan de `object` y no existe ninguna conversión entre tipos de puntero y `object`. En concreto, la conversión boxing y unboxing ([conversiones Boxing y unboxing](types.md#boxing-and-unboxing)) no son compatibles con punteros. Sin embargo, se permiten las conversiones entre diferentes tipos de puntero y entre tipos de puntero y los tipos enteros. Esto se describe en [conversiones de puntero](unsafe-code.md#pointer-conversions).

Un *tipo_de_puntero* no se puede usar como un argumento de tipo ([construye los tipos](types.md#constructed-types)) e inferencia ([inferencia de tipo](expressions.md#type-inference)) se produce un error en las llamadas de método genérico que habría deduce un tipo de argumento sea un tipo de puntero.

Un *tipo_de_puntero* puede utilizarse como el tipo de un campo volátil ([campos volátiles](classes.md#volatile-fields)).

Aunque se pueden pasar punteros como `ref` o `out` parámetros, si lo hace, por lo que puede provocar un comportamiento indefinido, ya que el puntero puede bien establecerse para que apunte a una variable local que ya no existe cuando se devuelve el método llamado, o el objeto fijo para la TI usa para apuntar, ya no es fijo. Por ejemplo:

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

Un método puede devolver un valor de algún tipo, y ese tipo puede ser un puntero. Por ejemplo, cuando recibe un puntero a una secuencia contigua de `int`s, número de elementos de la secuencia y algunos otros `int` valor, el método siguiente devuelve la dirección de ese valor en la secuencia, si se produce una coincidencia; de lo contrario, devuelve `null`:

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

En un contexto no seguro, existen varias construcciones para operar con punteros:

*  El `*` operador puede usarse para realizar el direccionamiento indirecto del puntero ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)).
*  El `->` operador puede utilizarse para tener acceso a un miembro de un struct a través de un puntero ([acceso a miembros de puntero](unsafe-code.md#pointer-member-access)).
*  El `[]` operador puede utilizarse para indizar un puntero ([acceso de elemento de puntero](unsafe-code.md#pointer-element-access)).
*  El `&` operador se puede utilizar para obtener la dirección de una variable ([el operador address-of](unsafe-code.md#the-address-of-operator)).
*  El `++` y `--` operadores pueden utilizarse para incrementar y disminuir punteros ([puntero incremento y decremento](unsafe-code.md#pointer-increment-and-decrement)).
*  El `+` y `-` operadores se pueden utilizar para realizar la aritmética de puntero ([aritmética de puntero](unsafe-code.md#pointer-arithmetic)).
*  El `==`, `!=`, `<`, `>`, `<=`, y `=>` operadores pueden utilizarse para comparar punteros ([comparación de punteros](unsafe-code.md#pointer-comparison)).
*  El `stackalloc` operador se puede utilizar para asignar memoria de la pila de llamadas ([búferes de tamaño fijo](unsafe-code.md#fixed-size-buffers)).
*  El `fixed` instrucción puede usarse para fijar temporalmente una variable se puede obtener su dirección ([la instrucción fixed](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Variables fijas y móviles

El operador address-of ([el operador address-of](unsafe-code.md#the-address-of-operator)) y el `fixed` instrucción ([la instrucción fixed](unsafe-code.md#the-fixed-statement)) dividen las variables en dos categorías: ***Se ha corregido variables*** y ***variables móviles***.

Variables fijas residen en ubicaciones de almacenamiento que se ven afectadas por la operación del recolector de elementos no utilizados. (Ejemplos de variables fijas incluyen variables locales, parámetros de valor y las variables creadas por desreferenciar punteros). Por otro lado, variables móviles residen en ubicaciones de almacenamiento que están sujetos a reubicación o eliminación por el recolector de elementos no utilizados. (Ejemplos de variables móviles incluyen campos de objetos y elementos de matrices).

El `&` operador ([el operador address-of](unsafe-code.md#the-address-of-operator)) permite obtener la dirección de una variable fija sin restricciones. Sin embargo, dado que una variable móvil está sujeta a reubicación o eliminación por el recolector de elementos no utilizados, la dirección de una variable móvil solo se puede obtener mediante una `fixed` instrucción ([la instrucción fixed](unsafe-code.md#the-fixed-statement)) y esa dirección solo es válido durante el tiempo que dure que `fixed` instrucción.

En términos precisos, una variable fija es uno de los siguientes:

*  Una variable resultante de un *simple_name* ([nombres simples](expressions.md#simple-names)) que hace referencia a una variable local o un parámetro de valor, a menos que la variable capturada por una función anónima.
*  Una variable resultante de un *member_access* ([acceso a miembros](expressions.md#member-access)) del formulario `V.I`, donde `V` es una variable fija de un *struct_type*.
*  Una variable resultante de un *pointer_indirection_expression* ([direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection)) del formulario `*P`, un *pointer_member_access* ([Acceso a miembros de puntero](unsafe-code.md#pointer-member-access)) del formulario `P->I`, o un *pointer_element_access* ([acceso de elemento de puntero](unsafe-code.md#pointer-element-access)) del formulario `P[E]`.

Todas las demás variables se clasifican como variables móviles.

Tenga en cuenta que un campo estático se clasifica como una variable móvil. Tenga en cuenta también que un `ref` o `out` parámetro se clasifica como una variable móvil, incluso si el argumento proporcionado para el parámetro es una variable fija. Por último, tenga en cuenta que una variable generada por desreferencia un puntero siempre se clasifica como una variable fija.

## <a name="pointer-conversions"></a>Conversiones de puntero

En un contexto no seguro, el conjunto de conversiones implícitas disponibles ([conversiones implícitas](conversions.md#implicit-conversions)) se extiende para incluir las siguientes conversiones implícitas de puntero:

*  Desde cualquier *tipo_de_puntero* al tipo `void*`.
*  Desde el `null` literal a cualquier *tipo_de_puntero*.

Además, en un contexto no seguro, el conjunto de conversiones explícitas disponibles ([las conversiones explícitas](conversions.md#explicit-conversions)) se extiende para incluir las siguientes conversiones explícitas de puntero:

*  Desde cualquier *tipo_de_puntero* a cualquier otro *tipo_de_puntero*.
*  Desde `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, o `ulong` a cualquier *tipo_de_puntero*.
*  Desde cualquier *tipo_de_puntero* a `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, o `ulong`.

Por último, en un contexto no seguro, el conjunto de conversiones implícitas estándar ([conversiones implícitas estándar](conversions.md#standard-implicit-conversions)) incluye la siguiente conversión de puntero:

*  Desde cualquier *tipo_de_puntero* al tipo `void*`.

Conversiones entre los dos tipos de puntero nunca cambian el valor de puntero real. En otras palabras, una conversión de un tipo de puntero a la otra tiene ningún efecto en la dirección subyacente dada por el puntero.

Cuando un tipo de puntero se convierte a otro, si aún no está correctamente alineado para el tipo al que apunta el puntero resultante, el comportamiento es indefinido si se desreferencia el resultado. En general, el concepto "correctamente alineado" es transitivo: si un puntero al tipo `A` está correctamente alineado para que un puntero al tipo `B`, que, a su vez, está correctamente alineado para que un puntero al tipo `C`, a continuación, un puntero a tipo `A`está correctamente alineado para que un puntero al tipo `C`.

Tenga en cuenta lo siguiente en el que se tiene acceso a una variable de un tipo determinado a través de un puntero a un tipo diferente:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Cuando un tipo de puntero se convierte en un puntero a byte, el resultado apunta al byte dirigido más bajo de la variable. Incrementos sucesivos del resultado, hasta alcanzar el tamaño de la variable, dan como resultado punteros a los bytes restantes de esa variable. Por ejemplo, el siguiente método muestra cada uno de los ocho bytes en un valor doble como un valor hexadecimal:

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

Por supuesto, el resultado producido depende endian.

Las asignaciones entre los punteros y los enteros son definido por la implementación. Sin embargo, en 32 * y arquitecturas de CPU de 64 bits con un espacio de direcciones lineales, conversiones de punteros a o desde tipos integrales normalmente se comportan exactamente igual que las conversiones de `uint` o `ulong` valores, respectivamente, a o desde esos tipos integrales.

### <a name="pointer-arrays"></a>Matrices de puntero

En un contexto no seguro, se pueden construir matrices de punteros. Sólo algunas de las conversiones que se aplican a otros tipos de matriz se permiten en las matrices de puntero:

*  La conversión implícita de referencia ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions)) desde cualquier *array_type* a `System.Array` y las interfaces que implementa también se aplica a las matrices de puntero. Sin embargo, cualquier intento de obtener acceso a los elementos de matriz a través de `System.Array` o las interfaces que implementa, se producirán una excepción en tiempo de ejecución, como tipos de puntero no son convertibles a `object`.
*  Las conversiones de referencia de implícitas y explícitas ([conversiones implícitas de referencia](conversions.md#implicit-reference-conversions), [conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) de un tipo de matriz unidimensional `S[]` a `System.Collections.Generic.IList<T>` y sus interfaces base genéricos nunca se aplican a las matrices de puntero, ya que los tipos de puntero no se puede usar como argumentos de tipo y no hay ninguna conversión entre tipos de puntero a tipos que no son de puntero.
*  La conversión de referencia explícita ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) desde `System.Array` y las interfaces que implementa a cualquier *array_type* se aplica a las matrices de puntero.
*  Las conversiones de referencia explícita ([conversiones explícitas de referencia](conversions.md#explicit-reference-conversions)) desde `System.Collections.Generic.IList<S>` y sus interfaces base a un tipo de matriz unidimensional `T[]` nunca se aplica a las matrices de puntero, ya que no pueden ser tipos de puntero usar como argumentos de tipo y no hay ninguna conversión entre tipos de puntero a tipos que no son de puntero.

Estas restricciones indican que la expansión para el `foreach` instrucción a través de las matrices se describe en [la instrucción foreach](statements.md#the-foreach-statement) no se puede aplicar a las matrices de puntero. En su lugar, una instrucción foreach del formulario

```csharp
foreach (V v in x) embedded_statement
```

donde el tipo de `x` es un tipo de matriz del formulario `T[,,...,]`, `N` es el número de dimensiones menos 1 y `T` o `V` es un tipo de puntero, se expande con bucles for anidados como sigue:

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

Las variables `a`, `i0`, `i1`,..., `iN` no está visible o accesible a `x` o *embedded_statement* o cualquier otro código fuente del programa. La variable `v` es de solo lectura en la instrucción incrustada. Si no hay una conversión explícita ([conversiones de puntero](unsafe-code.md#pointer-conversions)) desde `T` (el tipo de elemento) para `V`, se genera un error y no se toma ningún paso adicional. Si `x` tiene el valor `null`, un `System.NullReferenceException` se produce en tiempo de ejecución.

## <a name="pointers-in-expressions"></a>Punteros en expresiones

En un contexto no seguro, una expresión puede producir un resultado de un tipo de puntero, pero fuera de un contexto no seguro es un error en tiempo de compilación para una expresión de un tipo de puntero. En términos precisos, fuera de un contexto no seguro durante la compilación se produce un error si cualquier *simple_name* ([nombres simples](expressions.md#simple-names)), *member_access* ([acceso a miembros ](expressions.md#member-access)), *invocation_expression* ([expresiones de invocación](expressions.md#invocation-expressions)), o *element_access* ([acceso de elemento](expressions.md#element-access)) es un tipo de puntero.

En un contexto no seguro, el *primary_no_array_creation_expression* ([expresiones primarias](expressions.md#primary-expressions)) y *unary_expression* ([losoperadoresunarios](expressions.md#unary-operators)) producciones permiten las siguientes construcciones adicionales:

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

Estas construcciones se describen en las secciones siguientes. La prioridad y asociatividad de los operadores no seguros está implícito en la gramática.

### <a name="pointer-indirection"></a>Direccionamiento indirecto del puntero

Un *pointer_indirection_expression* consta de un asterisco (`*`) seguido por un *unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

El operador unario `*` operador denota el direccionamiento indirecto del puntero y se utiliza para obtener la variable a la que señala un puntero. El resultado de evaluar `*P`, donde `P` es una expresión de un tipo de puntero `T*`, es una variable de tipo `T`. Es un error en tiempo de compilación para aplicar el operador unario `*` operador en una expresión de tipo `void*` o en una expresión que no sea de un tipo de puntero.

El efecto de aplicar el operador unario `*` operador para un `null` puntero está definido por la implementación. En concreto, no hay ninguna garantía de que esta operación produce un `System.NullReferenceException`.

Si se ha asignado un valor no válido para el puntero, el comportamiento de unario `*` operador no está definido. Entre los valores no válidos para desreferenciar un puntero unario `*` operadores tienen una dirección incorrectamente alineada para el tipo señalado (vea el ejemplo de [conversiones de puntero](unsafe-code.md#pointer-conversions)) y la dirección de una variable después de la final de su duración.

Para fines de análisis de asignación definitiva, una variable producida al evaluar una expresión de formato `*P` se considera asignado inicialmente ([variables asignadas inicialmente](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>Acceso a miembros de puntero

Un *pointer_member_access* consta de un *primary_expression*, seguido de un "`->`" símbolo (token), seguido de un *identificador* y un opcional*type_argument_list*.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

En un acceso de miembro de puntero del formulario `P->I`, `P` debe ser una expresión de un tipo de puntero distinto `void*`, y `I` debe denotar un miembro accesible del tipo al que `P` puntos.

Un acceso de miembro de puntero de la forma `P->I` se evalúa exactamente como `(*P).I`. Para obtener una descripción del operador de direccionamiento indirecto del puntero (`*`), consulte [direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection). Para obtener una descripción del operador de acceso a miembros (`.`), consulte [acceso a miembros](expressions.md#member-access).

En el ejemplo

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

el `->` operador se usa para tener acceso a campos e invocar un método de un struct a través de un puntero. Dado que la operación `P->I` equivale exactamente a `(*P).I`, el `Main` método podría igual de bien se han escrito:

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a>Acceso de elemento de puntero

Un *pointer_element_access* consta de un *primary_no_array_creation_expression* seguido de una expresión encerrada entre "`[`"y"`]`".

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

En un acceso de elemento de puntero de la forma `P[E]`, `P` debe ser una expresión de un tipo de puntero distinto `void*`, y `E` debe ser una expresión que se puede convertir implícitamente a `int`, `uint`, `long`, o `ulong`.

Un acceso de elemento de puntero de la forma `P[E]` se evalúa exactamente como `*(P + E)`. Para obtener una descripción del operador de direccionamiento indirecto del puntero (`*`), consulte [direccionamiento indirecto del puntero](unsafe-code.md#pointer-indirection). Para obtener una descripción del operador de suma de puntero (`+`), consulte [aritmética de puntero](unsafe-code.md#pointer-arithmetic).

En el ejemplo

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

un acceso de elemento de puntero se usa para inicializar el búfer de caracteres en un `for` bucle. Dado que la operación `P[E]` equivale exactamente a `*(P + E)`, en el ejemplo se podría igual de bien se han escrito:

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

El operador de acceso de elemento de puntero no comprueba fuera de los límites de errores y el comportamiento al obtener acceso a un fuera de los límites elemento no está definido. Esto es lo mismo que C y C++.

### <a name="the-address-of-operator"></a>El operador address-of

Un *addressof_expression* consta de una y comercial (`&`) seguido por un *unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Dada una expresión `E` que es de un tipo `T` y se clasifica como una variable fija ([fijos y variables móviles](unsafe-code.md#fixed-and-moveable-variables)), la construcción `&E` calcula la dirección de la variable proporcionada por `E`. El tipo del resultado es `T*` y se clasifica como un valor. Se produce un error de tiempo de compilación si `E` no está clasificado como una variable, si `E` se clasifica como una variable local de solo lectura, o si `E` denota una variable móvil. En el último caso, una instrucción fixed ([la instrucción fixed](unsafe-code.md#the-fixed-statement)) se puede usar para "corregir" temporalmente la variable antes de obtener su dirección. Como se indicó en [acceso a miembros](expressions.md#member-access), fuera de un constructor de instancia o un constructor estático de una clase o estructura que define un `readonly` campo, dicho campo se considera un valor, no una variable. Por lo tanto, no se pueden tomar su dirección. De forma similar, no se pueden tomar la dirección de una constante.

El `&` operador no requiere que esté asignado definitivamente, pero después su argumento una `&` operación, la variable al que se aplica el operador se considera asignada definitivamente en la ruta de acceso de ejecución en el que se produce la operación. Es responsabilidad del programador asegurarse de que la inicialización correcta de la variable en realidad tienen lugar en esta situación.

En el ejemplo

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

`i` se considera asignado definitivamente siguiendo el `&i` operación utilizada para inicializar `p`. La asignación a `*p` vigente inicializa `i`, pero la inclusión de esta inicialización es responsabilidad del programador y no se producirá ningún error de tiempo de compilación si se quitó la asignación.

Las reglas de asignación definitiva para el `&` operador existe tal que se puede evitar la inicialización redundante de variables locales. Por ejemplo, muchas de las API externas toman un puntero a una estructura que se rellena mediante la API. Las llamadas a estas API suelen pasan la dirección de una variable local, y sin la regla, se requeriría una inicialización redundante de la variable.

### <a name="pointer-increment-and-decrement"></a>Puntero incremento y decremento

En un contexto no seguro, el `++` y `--` operadores ([operadores postfijos de incremento y decremento](expressions.md#postfix-increment-and-decrement-operators) y [prefijo de incremento y decremento operadores](expressions.md#prefix-increment-and-decrement-operators)) se pueden aplicar a puntero las variables de todos los tipos excepto `void*`. Por lo tanto, para cada tipo de puntero `T*`, implícitamente se definen los siguientes operadores:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

Los operadores producen los mismos resultados que `x + 1` y `x - 1`, respectivamente ([aritmética de puntero](unsafe-code.md#pointer-arithmetic)). En otras palabras, para una variable de puntero de tipo `T*`, el `++` operador agrega `sizeof(T)` a la dirección contenida en la variable y el `--` operador resta `sizeof(T)` desde la dirección contenida en la variable.

Si un puntero de incremento o decremento operación desborda el dominio del tipo de puntero, el resultado es definido por la implementación, pero no se producen excepciones.

### <a name="pointer-arithmetic"></a>Aritmética de puntero

En un contexto no seguro, el `+` y `-` operadores ([operador de suma](expressions.md#addition-operator) y [operador de resta](expressions.md#subtraction-operator)) se pueden aplicar a los valores de todos los tipos de puntero excepto `void*`. Por lo tanto, para cada tipo de puntero `T*`, implícitamente se definen los siguientes operadores:

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

Dada una expresión `P` de un tipo de puntero `T*` y una expresión `N` typu `int`, `uint`, `long`, o `ulong`, las expresiones `P + N` y `N + P` calcular el valor de puntero de tipo `T*` resultante de agregar `N * sizeof(T)` a la dirección proporcionada por `P`. Del mismo modo, la expresión `P - N` calcula el valor de puntero de tipo `T*` que da como resultado de restar `N * sizeof(T)` desde la dirección proporcionada por `P`.

Dadas dos expresiones, `P` y `Q`, de un tipo de puntero `T*`, la expresión `P - Q` calcula la diferencia entre las direcciones proporcionadas por `P` y `Q` y, a continuación, divide la diferencia por `sizeof(T)`. El tipo del resultado siempre es `long`. De hecho, `P - Q` se calcula como `((long)(P) - (long)(Q)) / sizeof(T)`.

Por ejemplo:

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

lo que genera el resultado:

```
p - q = -14
q - p = 14
```

Si una operación aritmética de puntero desborda el dominio del tipo de puntero, el resultado se trunca de forma definido por la implementación, pero no se producen excepciones.

### <a name="pointer-comparison"></a>Comparación de punteros

En un contexto no seguro, el `==`, `!=`, `<`, `>`, `<=`, y `=>` operadores ([operadores de comprobación de tipos y relacionales](expressions.md#relational-and-type-testing-operators)) se pueden aplicar a los valores de todos los tipos de puntero. Los operadores de comparación de puntero son:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Ya existe una conversión implícita de cualquier tipo de puntero a la `void*` se puedan comparar el tipo, los operandos de cualquier tipo de puntero mediante estos operadores. Los operadores de comparación comparan las direcciones proporcionadas por los dos operandos, como si fueran enteros sin signo.

### <a name="the-sizeof-operator"></a>El operador sizeof

El `sizeof` operador devuelve el número de bytes ocupados por una variable de un tipo determinado. El tipo como operando para `sizeof` debe ser un *unmanaged_type* ([tipos de puntero](unsafe-code.md#pointer-types)).

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

El resultado de la `sizeof` operador es un valor de tipo `int`. Predefinidos para determinados tipos, el `sizeof` operador produce un valor constante, como se muestra en la tabla siguiente.


| __Expresión__   | __Resultado__ |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

Para los demás tipos, el resultado de la `sizeof` operador define la implementación y se clasifica como un valor, no como una constante.

No se especifica el orden en que los miembros se empaquetan en un struct.

Para la alineación, puede haber relleno al principio de una estructura, dentro de una estructura y al final de la estructura sin nombre. El contenido de los bits utilizados como relleno es indeterminado.

Cuando se aplica a un operando que tiene el tipo de estructura, el resultado es el número total de bytes en una variable de ese tipo, incluido el relleno.

## <a name="the-fixed-statement"></a>La instrucción fixed

En un contexto no seguro, el *embedded_statement* ([instrucciones](statements.md)) producción permite una construcción adicional, la `fixed` instrucción, que se utiliza una variable móvil "corregir" que su dirección permanece constante para la duración de la instrucción.

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

Cada *fixed_pointer_declarator* declara una variable local de la dada *tipo_de_puntero* e inicializa la variable local con la dirección calculada por el correspondiente *fixed_ pointer_initializer*. Una variable local declarada en un `fixed` es accesible en cualquier instrucción *fixed_pointer_initializer*s que se producen a la derecha de la declaración de variable y, en el *embedded_statement* de la `fixed` instrucción. Una variable local declarada por un `fixed` instrucción se considera de solo lectura. Se produce un error de tiempo de compilación si la instrucción incrustada intenta modificar esta variable local (a través de la asignación o el `++` y `--` operadores) o pasarla como un `ref` o `out` parámetro.

Un *fixed_pointer_initializer* puede ser uno de los siguientes:

*  El token "`&`" seguido por un *variable_reference* ([reglas precisas para determinar la asignación definitiva](variables.md#precise-rules-for-determining-definite-assignment)) a una variable móvil ([fijos y variables móviles](unsafe-code.md#fixed-and-moveable-variables)) de un tipo no administrado `T`, siempre que el tipo `T*` es implícitamente convertible al tipo de puntero proporcionado el `fixed` instrucción. En este caso, el inicializador calcula la dirección de la variable dada, y se garantiza que la variable va a permanecer en una dirección fija durante la duración de la `fixed` instrucción.
*  Una expresión de un *array_type* con elementos de un tipo no administrado `T`, siempre que el tipo `T*` es implícitamente convertible al tipo de puntero proporcionado el `fixed` instrucción. En este caso, el inicializador calcula la dirección del primer elemento de la matriz, y se garantiza que toda la matriz permanecerá en una dirección fija durante el tiempo que dure la `fixed` instrucción. Si la expresión de matriz es null o si la matriz tiene cero elementos, el inicializador calcula un igual de dirección a cero.
*  Una expresión de tipo `string`, siempre que el tipo `char*` es implícitamente convertible al tipo de puntero proporcionado el `fixed` instrucción. En este caso, el inicializador calcula la dirección del primer carácter en la cadena y se garantiza que toda la cadena permanecerá en una dirección fija durante el tiempo que dure la `fixed` instrucción. El comportamiento de la `fixed` instrucción se define en la implementación si la expresión de cadena es null.
*  Un *simple_name* o *member_access* que hace referencia a un miembro de búfer de tamaño fijo de una variable móvil, siempre que el tipo del miembro de búfer de tamaño fijo es implícitamente convertible al tipo de puntero proporcionado en el `fixed` instrucción. En este caso, el inicializador calcula un puntero al primer elemento del búfer de tamaño fijo ([búferes de tamaño fijo en expresiones](unsafe-code.md#fixed-size-buffers-in-expressions)), y se garantiza que el búfer de tamaño fijo permanecerá en una dirección fija durante el tiempo que dure la `fixed`instrucción.

Para cada dirección calculada por un *fixed_pointer_initializer* el `fixed` instrucción garantiza que la variable al que hace referencia la dirección no está sujeta a reubicación o eliminación por el recolector de elementos no utilizados para la duración de la `fixed` instrucción. Por ejemplo, si la dirección calculada por un *fixed_pointer_initializer* hace referencia a un campo de un objeto o un elemento de una instancia de matriz, el `fixed` instrucción garantiza que la instancia del objeto contenedor no se reubica o eliminado durante la vigencia de la instrucción.

Es responsabilidad del programador garantizar que los punteros crean por `fixed` instrucciones no permanecen tras la ejecución de esas instrucciones. Por ejemplo, cuando punteros crean por `fixed` instrucciones se pasan a las API externas, es responsabilidad del programador asegurarse de que las API no retienen memoria de estos punteros.

Objetos fijos pueden provocar la fragmentación del montón (porque no se pueden mover). Por ese motivo, se deben corregir los objetos solo cuando sea absolutamente necesario y, a continuación, solo por el menor tiempo posible.

El ejemplo

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

muestra varios usos de la `fixed` instrucción. La primera instrucción fija y obtiene la dirección de un campo estático, la segunda instrucción fija y obtiene la dirección de un campo de instancia y la tercera instrucción fija y obtiene la dirección de un elemento de matriz. En cada caso habría sido un error usar el normal `&` operador dado que las variables se clasifican como variables móviles.

El cuarto `fixed` instrucción en el ejemplo anterior genera un resultado similar a la tercera.

En este ejemplo de la `fixed` instrucción usa `string`:

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

En un contexto no seguro se almacenan los elementos de la matriz de matrices unidimensionales en orden creciente de índice, empezando por el índice `0` y terminando con el índice `Length - 1`. Para matrices multidimensionales, la matriz se almacenan los elementos que aumentan en primer lugar, los índices de la dimensión situada más a continuación, la próxima deja la dimensión, y así sucesivamente hacia la izquierda. Dentro de un `fixed` instrucción que obtiene un puntero `p` a una instancia de matriz `a`, los valores de puntero que abarcan desde `p` a `p + a.Length - 1` representan las direcciones de los elementos de la matriz. Del mismo modo, las variables que van desde `p[0]` a `p[a.Length - 1]` representan los elementos de matriz real. Dada la manera en que se almacenan las matrices, se puede tratar una matriz de cualquier dimensión como si fuese lineal.

Por ejemplo:

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

lo que genera el resultado:

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

En el ejemplo

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

un `fixed` instrucción se utiliza para resolver una matriz para que su dirección se puede pasar a un método que toma un puntero.

En el ejemplo:

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

una instrucción fixed se utiliza para resolver un búfer de tamaño fijo de un struct para que su dirección puede usarse como un puntero.

Un `char*` valor generado mediante la corrección de una instancia de cadena siempre apunta a una cadena terminada en null. Dentro de una instrucción fixed que obtiene un puntero `p` a una instancia de cadena `s`, los valores de puntero que abarcan desde `p` a `p + s.Length - 1` representan las direcciones de los caracteres en la cadena y el valor de puntero `p + s.Length` siempre apunta a un carácter nulo (el carácter con el valor `'\0'`).

Modificación de objetos de tipo administrado mediante punteros fijos puede tener como resultado un comportamiento indefinido. Por ejemplo, dado que las cadenas son inmutables, es responsabilidad del programador asegurarse de que no se modifican los caracteres que se hace referencia a un puntero a una cadena fija.

La terminación null automática de cadenas es especialmente conveniente al llamar a las API externas que esperan cadenas de "estilo C". Sin embargo, tenga en cuenta que una instancia de la cadena puede contener caracteres null. Si existen dichos caracteres null, la cadena aparecerá truncada cuando se trata como terminada en null `char*`.

## <a name="fixed-size-buffers"></a>Búferes de tamaño fijo

Búferes de tamaño fijo se utilizan para declarar matrices en línea de "Estilo C" como miembros de estructuras y son útiles principalmente para interactuar con las API no administradas.

### <a name="fixed-size-buffer-declarations"></a>Declaraciones de búfer de tamaño fijo

Un ***búfer de tamaño fijo*** es un miembro que representa el almacenamiento para un búfer de longitud fija de variables de un tipo determinado. Una declaración de búfer de tamaño fijo introduce uno o varios búferes de tamaño fijo de un tipo de elemento especificado. Búferes de tamaño fijo solo se permiten en declaraciones de estructura y solo puede aparecer en contextos no seguros ([contextos no seguros](unsafe-code.md#unsafe-contexts)).

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

Una declaración de búfer de tamaño fijo puede incluir un conjunto de atributos ([atributos](attributes.md)), un `new` modificador ([modificadores](classes.md#modifiers)), una combinación válida de los cuatro modificadores de acceso ([tipo parámetros y restricciones](classes.md#type-parameters-and-constraints)) y un `unsafe` modificador ([contextos no seguros](unsafe-code.md#unsafe-contexts)). Los atributos y modificadores se aplican a todos los miembros declarados en la declaración del búfer de tamaño fijo. Es un error para el mismo modificador aparezca varias veces en una declaración de búfer de tamaño fijo.

No se permite una declaración de búfer de tamaño fijo para incluir el `static` modificador.

El tipo de elemento de búfer de una declaración de búfer de tamaño fijo especifica el tipo de elemento de los búferes introducidos por la declaración. El tipo de elemento de búfer debe ser uno de los tipos predefinidos `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, o `bool`.

El tipo de elemento de búfer es seguido por una lista de declaradores de búfer de tamaño fijo, cada uno de los cuales incluye a un nuevo miembro. Un declarador de búfer de tamaño fijo consta de un identificador que designa el miembro, seguido de una expresión constante entre `[` y `]` tokens. La expresión constante denota el número de elementos en el miembro introducidos por esa declarador de búfer de tamaño fijo. El tipo de la expresión constante debe ser implícitamente convertible al tipo `int`, y el valor debe ser un entero positivo distinto de cero.

Se garantiza que los elementos de un búfer de tamaño fijo se disponen secuencialmente en la memoria.

Una declaración de búfer de tamaño fijo que declara varios búferes de tamaño fijo es equivalente a varias declaraciones de una declaración de búfer de tamaño fijo solo con los mismos atributos y tipos de elemento. Por ejemplo

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

es equivalente a

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a>Búferes de tamaño fijo en expresiones

Búsqueda de miembros ([operadores](expressions.md#operators)) de un tamaño fijo, miembro de búfer procede exactamente igual que la búsqueda de miembros de un campo.

Se puede hacer referencia a un búfer de tamaño fijo en una expresión que utiliza un *simple_name* ([inferencia](expressions.md#type-inference)) o un *member_access* ([comprobación de tiempo de compilación la resolución de sobrecarga dinámica](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Cuando se hace referencia a un miembro de búfer de tamaño fijo como un nombre simple, el efecto es el mismo que un acceso de miembro del formulario `this.I`, donde `I` es el miembro de búfer de tamaño fijo.

En un acceso de miembro del formulario `E.I`si `E` es de un tipo struct y una búsqueda de miembros de `I` en que el tipo de estructura identifica un miembro de tamaño fijo, a continuación, `E.I` es evalúa y clasifica como sigue:

*  Si la expresión `E.I` no se produce en un contexto no seguro, se produce un error de tiempo de compilación.
*  Si `E` se clasifica como un valor, se produce un error de tiempo de compilación.
*  De lo contrario, si `E` es una variable móvil ([fijos y variables móviles](unsafe-code.md#fixed-and-moveable-variables)) y la expresión `E.I` no es un *fixed_pointer_initializer* ([fijo instrucción](unsafe-code.md#the-fixed-statement)), se produce un error de tiempo de compilación.
*  En caso contrario, `E` hace referencia a una variable fija y el resultado de la expresión es un puntero al primer elemento del miembro de búfer de tamaño fijo `I` en `E`. El resultado es de tipo `S*`, donde `S` es el tipo de elemento de `I`y se clasifica como un valor.

Pueden tener acceso a los elementos subsiguientes del búfer de tamaño fijo con operaciones de puntero desde el primer elemento. A diferencia de tener acceso a las matrices, acceso a los elementos de un búfer de tamaño fijo es una operación no segura y no está activada de intervalo.

El ejemplo siguiente declara y utiliza una estructura con un miembro de búfer de tamaño fijo.

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a>Comprobación de asignación definitiva

Búferes de tamaño fijo no están sujetos a comprobación de asignación definitiva ([asignación definitiva](variables.md#definite-assignment)), y se omiten los miembros de búfer de tamaño fijo para los fines de comprobación de las variables de tipo de estructura de asignación definitiva.

Cuando la variable de estructura contenedora más externo de un miembro de búfer de tamaño fijo es una variable estática, una variable de instancia de una instancia de clase o un elemento de matriz, los elementos del búfer de tamaño fijo se inicializan automáticamente en sus valores predeterminados ([Los valores predeterminados](variables.md#default-values)). En todos los demás casos, el contenido inicial de un búfer de tamaño fijo es indefinido.

## <a name="stack-allocation"></a>Asignación de pila

En un contexto no seguro, una declaración de variable local ([declaraciones de variable Local](statements.md#local-variable-declarations)) puede incluir un inicializador de asignación de pila que asigne memoria de la pila de llamadas.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

El *unmanaged_type* indica el tipo de los elementos que se almacenarán en la ubicación recién asignada, y el *expresión* indica el número de estos elementos. En conjunto, estos especifican el tamaño de asignación necesarios. Puesto que el tamaño de una asignación de pila no puede ser negativo, es un error en tiempo de compilación para especificar el número de elementos como un *constant_expression* que se evalúa como un valor negativo.

Un inicializador de asignación de pila del formulario `stackalloc T[E]` requiere `T` sea un tipo no administrado ([tipos de puntero](unsafe-code.md#pointer-types)) y `E` sea una expresión de tipo `int`. La construcción asigna `E * sizeof(T)` bytes a partir de la llamada de la pila y devuelve un puntero de tipo `T*`, en el bloque recién asignado. Si `E` es un valor negativo, el comportamiento es indefinido. Si `E` es cero, a continuación, no se realiza ninguna asignación, y el puntero devuelto está definido por la implementación. Si no hay suficiente memoria disponible para asignar un bloque de tamaño determinado, un `System.StackOverflowException` se produce.

El contenido de la memoria recién asignada es indefinido.

No se permiten inicializadores de asignación de pila en `catch` o `finally` bloques ([la instrucción try](statements.md#the-try-statement)).

No hay ninguna manera de liberar explícitamente la memoria asignada mediante `stackalloc`. Todos los bloques de memoria asignado a la pila creados durante la ejecución de un miembro de función se descartan automáticamente cuando se devuelve ese miembro de función. Esto corresponde a la `alloca` (función), una extensión que se suelen encontrar en las implementaciones de C y C++.

En el ejemplo

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

un `stackalloc` inicializador se utiliza en el `IntToString` método para asignar un búfer de 16 caracteres en la pila. El búfer se descarta automáticamente cuando el método devuelve.

## <a name="dynamic-memory-allocation"></a>Asignación de memoria dinámica

Excepto para el `stackalloc` operador, C# no proporciona construcciones predefinidas para la administración de memoria de elementos no utilizados no recopilado. Dichos servicios normalmente proporciona bibliotecas de clases auxiliares o importar directamente desde el sistema operativo subyacente. Por ejemplo, el `Memory` clase siguiente muestra cómo se pueden tener acceso a las funciones del montón de un sistema operativo subyacente de C#:

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

Un ejemplo que utiliza el `Memory` clase se indica a continuación:

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

El ejemplo asigna 256 bytes de memoria a través de `Memory.Alloc` e inicializa el bloque de memoria con valores entre 0 y 255. A continuación, asigna una matriz de bytes de 256 elementos y utiliza `Memory.Copy` para copiar el contenido del bloque de memoria en la matriz de bytes. Por último, se libera el bloque de memoria mediante `Memory.Free` y salida en la consola el contenido de la matriz de bytes.
